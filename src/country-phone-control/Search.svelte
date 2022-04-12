<script>
    import {createEventDispatcher} from 'svelte';

    const dispatch = createEventDispatcher();
    export let searchTerm;
</script>

<div class="prfs-ctphdp__prepend">
  <input
      type="text"
      class="prfs-ctphdp__search"
      bind:value={searchTerm}
      on:keyup={() => {dispatch('updateSearch');}}/>
  <i class="prfs-ctphdp__icon"/>
</div>