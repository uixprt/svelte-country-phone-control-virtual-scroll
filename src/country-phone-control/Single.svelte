<script>
    import {createEventDispatcher} from 'svelte';

    const dispatch = createEventDispatcher();
    export let name;
    export let phoneCode;
    export let src;
    export let placeholder;
</script>

<style>
  .flag {
    height: 2em;
    width: 2em;
    border-radius: 50%;
    box-shadow: 0 2px 10px 0 rgb(0 0 0 / 15%);
  }
  .code {
    white-space: nowrap;
    margin-left: 0.4em;
  }
  .prfs-ctphdp__phonePrefix {
    border-right: 1px solid currentColor;
  }
  .prfs-ctphdp__phone {
    background-color: transparent;
  }
</style>

<div class="prfs-ctphdp__single" on:click={() => dispatch('singleClick')}>
    <div class="prfs-ctphdp__phonePrefix">
        {#if phoneCode}
            <img class="flag" {src} alt="{name}"/>
            <span class="code">+{phoneCode}</span>
        {:else}
            {placeholder}
        {/if}
        <div class="prfs-ctphdp__caret"></div>
    </div>
    <input type="text" class="prfs-ctphdp__phone" placeholder="enter your phone...">
</div>
