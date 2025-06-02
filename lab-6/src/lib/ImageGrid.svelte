<script>
    import { beforeUpdate } from "svelte";
    export let images;
  
    beforeUpdate(() => {
      if (images?.length > 0) {
        window.scrollTo({ top: 0, behavior: "smooth" });
      }
    });
  </script>
  
  <div class="grid is-col-min-9">
    {#each images as image}
      <div class="cell">
        <div class="card">
          <div class="card-image">
            <figure class="image">
              <img
                src="{image.src + 'full/!400,400/0/default.jpg'}"
                alt="Placeholder image"
              />
            </figure>
          </div>
          <div class="card-content content">
            <div class="content">
              {@html image.description}
              <br />
              <span class="tag is-info mt-2">{image.objectType}</span>
            </div>
          </div>
        </div>
      </div>
    {/each}
  </div>