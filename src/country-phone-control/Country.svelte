<script>
  import { createEventDispatcher } from 'svelte';

  const dispatch = createEventDispatcher();
  export let name;
  export let phoneCode;
  export let src;
  export let selectedCountry;
  let activeClass = false;

  if (selectedCountry.phoneCode === phoneCode) {
    activeClass = true
  }
</script>

<style>
    .flag {
        height: 2em;
        width: 2em;
        margin-left: 0.4em;
        border-radius: 50%;
    }

    .code {
        white-space: nowrap;
        margin-left: 0.4em;
    }

    .name {
        margin-left: 0.4em;
        opacity: 0.5;
        font-size: .8em;
    }
</style>

<li class="prfs-ctphdp__item" class:prfs-ctphdp__item--selected={activeClass} on:click={() => {dispatch('countryClick', {phoneCode})}}>
  <img class="flag" {src} alt="`flag icon for {name}`" />
  <span class="code">+{phoneCode}</span>
  <span class="name">{name}</span>
</li>
