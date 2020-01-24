<script>
    import { createEventDispatcher } from 'svelte';
    
    const dispatch = createEventDispatcher();

    export let apiBaseUrl;
    export let editingPost; 

    const POST_RESET = {
        id: null,
        title: "",
        body: "",
    }

    let loading = false;

    let title, body;

    let url, method;
    let newPost = {};

    $: title = editingPost.title;
    $: body = editingPost.body;
    
    $: newPost = editingPost.id ? editingPost : {
        title, body
    }

    $: formLabelClass = {
        title: (title === "") ? "" : "active",
        body: (body === "") ? "" : "active",       
    }

    const clearForm = async () => {
        editingPost = POST_RESET;
        return
    }

    const postRequest = async () => {
        if (editingPost.id) {
            url = `${apiBaseUrl}/post/${editingPost.id}`;
            method = "PUT";
            newPost = editingPost
        } else {
            url = `${apiBaseUrl}/post`;
            method = "POST";        
            newPost = { title, body }
        }   

        const req = await fetch(url , {
            method, 
            body: JSON.stringify(newPost)
        });
        const res = await req.json();

        (method === "PUT") ? dispatch('postUpdated', res) : dispatch('postCreated', res);

        return;
    }

    const onSubmit = async (e) => {
        e.preventDefault();

        if(title.trim() === '' && body.trim() === '') {
            return;
        }

        loading = true;
        await postRequest();
        loading = false;
        await clearForm();
    }

</script>

<style>
    form {
        margin: 50px;
    }
</style>

{#if !loading}
<form on:submit={onSubmit} id="post-form">
    <div class="input-field">
        <label for="title"  class={formLabelClass.title}>Title</label>
        <input type="text" name="title" bind:value={editingPost.title}>
    </div>
    <div class="input-field">
        <label for="title" class={formLabelClass.body}>Body</label>
        <textarea id="textarea1" class="materialize-textarea" bind:value={editingPost.body}></textarea>
    </div>
    <button class="btn-flat waves-effect white-text grey lighen-5 waves-light">
        {editingPost.id ? 'Update' : 'Add'}
    </button>
    <span class="btn-flat waves-effect white-text red accent-1 waves-light" on:click={clearForm}>Cancel</span>
</form>
{:else if loading === true}
    <div>Sending...</div>
{/if}
