<script>
  import Single from './Single.svelte';
  import Drop from './Drop.svelte';
  import Search from './Search.svelte';
  import Results from './Results.svelte';

  import { onMount } from 'svelte';

  function filterCountries(countries, searchTerm) {
    let filteredCountries;

    if (!searchTerm) {
      filteredCountries = countries;
    } else searchTerm = searchTerm.toLowerCase().replace('+', '');

    filteredCountries = countries.filter(
      (c) =>
        `${c.phoneCode}`.toLowerCase().includes(searchTerm) ||
        `${c.name}`.toLowerCase().includes(searchTerm) ||
        `${c.alpha2Code}`.toLowerCase().includes(searchTerm),
    );

    if (filteredCountries.length === 0) {
      filteredCountries = [];
    }

    return filteredCountries;
  }

  function toggleDrop() {
    if (dropOpen && searchTerm) {
      searchTerm = '';
      displayCountries = countries;
    }
    dropOpen = !dropOpen;
  }

  function selectCountry(countries, phoneCode) {
    let countryToSelect = countries.find(c => c.phoneCode === phoneCode);
    toggleDrop();
    return countryToSelect;
  }

  let placeholder = '...';
  let countries = [];
  let searchTerm = '';
  let displayCountries = [];
  let selectedCountry = null;
  let dropOpen = false;


  onMount(async () => {
    const res = await fetch(`data.json`);

    countries = await res.json();
    countries = countries.map((c) => ({
      name: c.name,
      phoneCode: c.phoneCode,
      alpha2Code: c.alpha2Code.toLowerCase(),
      id: c.id,
      src: `/flags/1x1/${c.alpha2Code.toLowerCase()}.svg`,
    }));
    displayCountries = countries;
    selectedCountry = displayCountries[0];
  });
</script>

<style global lang="scss">
  @import "../styles/main.scss";
</style>

<div class="prfs-ctphdp" class:prfs-ctphdp--opened="{dropOpen}">
  <Single
    src="{selectedCountry && selectedCountry.src}"
    name="{selectedCountry && selectedCountry.name}"
    phoneCode="{selectedCountry && selectedCountry.phoneCode}"
    placeholder="{placeholder}" on:singleClick={toggleDrop}
  />

  {#if dropOpen}
    <Drop>
      <Search
        bind:searchTerm
        on:updateSearch={() => {
        displayCountries = filterCountries(countries, searchTerm);
      }} />
      <Results
        bind:selectedCountry={selectedCountry}
        bind:countries={displayCountries}
        on:countryClick={(event) => {
          selectedCountry = selectCountry(countries, event.detail.phoneCode);
        }}
      />
    </Drop>
  {/if}
</div>


