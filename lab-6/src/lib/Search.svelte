<script>
    import { createEventDispatcher } from "svelte";
    const dispatch = createEventDispatcher();
  
    export let placeholder;
    let term = "";
  
    function search() {
      if (term) {
        dispatch("search", {
          term: term,
        });
      }
    }
  
    function key(e) {
      if (e.keyCode === 13) search();
    }
  </script>
  
  <nav role="navigation" aria-label="main navigation">
    <div class="field has-addons">
      <div class="control is-expanded">
        <input
          class="input"
          type="text"
          placeholder="{placeholder}"
          on:keydown="{key}"
          bind:value="{term}"
        />
      </div>
      <div class="control">
        <button class="button is-info" id="search" on:click="{search}">
          Search
        </button>
      </div>
    </div>
  </nav>