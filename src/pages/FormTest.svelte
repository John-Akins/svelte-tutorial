<script> 
    import { onMount } from 'svelte';

    const apiBaseUrl = "https://ndb99xkpdk.execute-api.eu-west-2.amazonaws.com/dev"

    let selectedValues = [];

    const test = (e) => {
        let instance = M.FormSelect.getInstance(e.target);
        selectedValues = instance.getSelectedValues();
    }
</script>

<style>
</style>

<br>
<br>
<br>
<div class="row">
    <div class="input-field col s12">
        <select name="selectOptions" multiple on:change={test}>
        <option value="" disabled selected>Choose your option</option>
        <option value="1">Option 1</option>
        <option value="2">Option 2</option>
        <option value="3">Option 3</option>
        </select>
        <label>Materialize Select</label>
    </div>
</div>
<br>
<br>
<br>

{#each selectedValues as item, i}
         <li class="white black-text" >{i} : {item}</li>
{/each}
