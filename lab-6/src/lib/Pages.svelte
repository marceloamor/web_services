<script>
    import pagination from "./pagination.js";
    import { createEventDispatcher } from "svelte";
  
    export let pages;
    export let page;

    const dispatch = createEventDispatcher();
  
    function changePage(pg) {
      // Dispatch event called change here
      dispatch('change', { page: pg });
    }
  </script>
  
  <nav class="pagination" role="navigation" aria-label="pagination">
    {#if pages > 1}
      {#if page <= pages && page > 1}
        <button
          class="pagination-previous"
          on:click="{changePage(page - 1)}">
        Previous
        </button>
      {/if}
  
      {#if page < pages}
        <button
          class="pagination-next"
          on:click="{changePage(page + 1)}">
        Next
        </button>
      {/if}
  
      <ul class="pagination-list">
        {#each pagination(page, pages) as pg}
          <li>
            {#if pg == "..."}
              <span class="pagination-ellipsis">&hellip;</span>
            {:else}
              <button
                class="pagination-link"
                class:is-current="{pg === page}"
                aria-current="{pg === page}"
                on:click="{changePage(pg)}">{pg}</button
              >
            {/if}
          </li>
        {/each}
      </ul>
    {/if}
  </nav>