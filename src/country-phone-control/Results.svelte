<script>
  import VirtualList from '@sveltejs/svelte-virtual-list';
  import Country from './Country.svelte';
  import { onMount, afterUpdate } from 'svelte';

  export let countries;
  export let selectedCountry;

  let selectedIndex = countries.findIndex((country) => country.phoneCode === selectedCountry.phoneCode);
  let scrollTop = 0;

  onMount(() => {

  });

  afterUpdate(() => {
    setTimeout(() => {
        const items = document.querySelectorAll('.prfs-ctphdp__item');
        const viewport = document.querySelector('svelte-virtual-list-viewport');
        const viewportHeight = viewport.offsetHeight;
        let itemHeight = 0;
        if(items.length >= 1) {
          itemHeight = items[items.length - 1].offsetTop / (items.length - 1);
        }

        let encorePoint = (itemHeight * selectedIndex) - ((viewportHeight - itemHeight) / 2);
        viewport.scroll({ top: encorePoint});
      },
      1,
    );
  });
</script>
<ul class="prfs-ctphdp__results">
  <VirtualList height="{34*5}px" itemHeight={34} items={countries} let:item >
    <Country {...item} {selectedCountry} on:countryClick />
  </VirtualList>
</ul>
{#if !countries}
  no results found
{/if}