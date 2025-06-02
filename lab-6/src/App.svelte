<script>
  import Search from "./lib/Search.svelte";
  import ImageGrid from "./lib/ImageGrid.svelte";

  let images;

  let page = 1;
  let pages;
  let term;

  function handleSearch(event) {
    if (term !== event.detail.term) page = 1;
    term = event.detail.term;
    images = getSearchResults(term);
  }

  async function getSearchResults(term) {
    // Search the V&A API
    const query =
      'https://api.vam.ac.uk/v2/objects/search?q="' +
      term +
      '"&images_exist=1&page_size=15&page=' +
      page;

    const results = await fetch(query).then((res) => res.json());

    // Set pages from results
    page = results?.info?.page;
    pages = results?.info?.pages;

    // Fetch objects records for each search result item
    const items = results.records.map(
      (r) => "https://api.vam.ac.uk/v2/museumobject/" + r?.systemNumber,
    );

    const promises = items.map((url) =>
      fetch(url)
        .then((res) => res.json())
        .then((body) => body)
        .catch((err) => console.warn(err)),
    );

    const records = await Promise.all(promises);

    // Map returned object records
    const _images = records.map((r) => {
      const img_source = r?.meta?.images?._iiif_image;
      if (img_source) {
        return {
          src: img_source,
          alt: r?._primaryTitle,
          description: r?.record?.briefDescription,
          accessionYear: r?.record?.accessionYear,
          objectType: r?.record?.objectType,
        };
      }
    });

    return _images;
  }
</script>

<main>
  <Search placeholder="Find images" on:search="{handleSearch}" />

  {#if images}
    {#await images}
      <p>Loading...</p>
    {:then img}
      <ImageGrid images="{img}" />
    {:catch error}
      <p style="color: red">{error.message}</p>
    {/await}
  {/if}
</main>