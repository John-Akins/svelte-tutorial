<script> 
    import { onMount } from 'svelte';
    import PostForm from '../components/PostForm.svelte';

    export let apiBaseUrl;
    export let M;

    const posts = {
        loaded: false,
        items: [],
    };

    const POST_RESET = {
        id: null,
        title: "",
        body: "",
    }

    let editingPost = {
        id: null,
        title: "",
        body: "",
    };

    let limit = 6;

    onMount((async () => {
        M.AutoInit();
        const res = await fetch(`${apiBaseUrl}/posts`);
        posts.items = await res.json();  
        posts.loaded = posts.items.length > 0 ? true : false
    }));

    const editPost = (post) => {
        editingPost = POST_RESET;
        console.log("post")
        console.log(post)
        console.log(posts)
        editingPost = post;
    }

    const addPost = async ({ detail: post }) => {
        posts.items = [post, ...posts.items]
    }

    const updatePost = async({ detail: post }) => {
        const index = posts.items.findIndex(p => p.id === post.id);
        let postsUpdated = posts.items;
        postsUpdated.splice(index, 1, post)
        posts.items = postsUpdated;
    }

    const deletePost = async (id) => {
        confirm(`Are you sure?`);
        const req = fetch(`${apiBaseUrl}/post/${id}`, {
            method: "DELETE",
        });
        const res = await req;
        posts.items = posts.items.filter(p => p.id != id);
    }

    const setLimit = async () => {
        const res = await fetch(`${apiBaseUrl}/posts/${limit}`);
        posts.items = await res.json();          
    }

</script>

<style>
    .card .card-content .card-title {
        margin-bottom: 0;
    }
    .card .card-content p.timestamp {
        margin-bottom: 10px;
        color: #999;
    }
    .card.form-container {
        border: 0px solid #999;
        border-radius: 5px;
        box-shadow: unset !important;
        background: ghostwhite;
        margin-top: 10px;
    }
</style>

<div class="row">
    <div class="col s6">
        <div class="card form-container">
            <div class="card-content">
                <PostForm on:postCreated={addPost} on:postUpdated={updatePost} {apiBaseUrl} {editingPost} />        
            </div>
        </div>
    </div>
    <div class="col s6">
        <div class="card form-container">
            <div class="card-content">
                <h5>Set Content Limit</h5>
                <div>
                    <input type="number" bind:value={limit}/>
                    <button class="btn-flat waves-effect white-text teal accent-4 waves-light" on:click={setLimit}>
                    Set Limit
                    </button>
                </div>
            </div>
        </div>
    </div>
</div>



<div class="row">
    {#if posts.loaded === false}
        <div class="">
            Loading posts...
        </div>
    {:else}
        {#each posts.items as post}
            <div class="col s6">
                <div class="card">
                    <div class="card-content">
                        <p class="card-title">
                            {post.title}
                        </p>
                        <p class="timestamp">{post.createdAt}</p>
                        <p>{post.body}</p>
                    </div>
                    <div class="card-action">
                        <a href="#post-form" class="btn-flat waves-effect  waves-grey" on:click = {() => editPost(post)}>Edit</a>
                        <span class="red-text waves-effect btn-flat waves-red" on:click = {() => deletePost(post.id)}>Delete</span>
                    </div>
                </div>
            </div>
        {/each}    
    {/if}
</div>
