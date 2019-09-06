<script>
  import { onMount } from "svelte";
  import axios from "axios";
  import ArticleOverview from "./ArticleOverview.svelte";

  let articles;
  let spin = false;
  let error = null;

  onMount(fetchArticles);

  async function fetchArticles() {
    try {
      const { data } = await axios({
        method: "POST",
        headers: {
          "Access-Control-Allow-Origin": "*"
        },
        url: "http://localhost:5001/get-news",
        data: {
          url: true
        }
      });

      articles = data.articles;
      error = null;
    } catch (error) {
      error = "Oops, something went wrong";
      console.error(error);
    }
  }
</script>

<style lang="scss">
  $mainColor: hsla(0, 90%, 51%, 0.835);
  .wrapper {
    display: grid;
    grid-template-areas:
      "header header"
      "nav main"
      "footer footer";
    height: 100vh;
  }

  header {
    display: flex;
    color: #fff;
    align-items: center;
    justify-content: center;
    background: $mainColor;
    margin-bottom: 20px;
    padding: 15px 0;
    grid-area: header;
    box-shadow: 0 4px 3px rgba(0, 0, 0, 0.3);

    i {
      font-size: 1.9rem;
      margin-right: 10px;
    }
  }

  nav {
    grid-area: nav;
    width: 200px;
    display: flex;
    flex-flow: column;

    button {
      align-self: end;
      transition: all 300ms ease-in-out;

      &:hover {
        background: #f5f5f5;
        border-radius: 50%;
      }
    }

    ul {
      padding: 10px;
    }
  }

  main {
    padding: 0 10px;
    grid-area: main;
    overflow: auto;
  }

  .articles {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
    grid-gap: 20px;
  }

  button {
    border: 0;
    background: unset;
    cursor: pointer;
  }

  footer {
    grid-area: footer;
    background: $mainColor;
    color: #fff;
    padding: 10px;
    margin-top: 20px;
    box-shadow: 0 -4px 3px rgba(0, 0, 0, 0.3);
  }
</style>

<div class="wrapper">
  <header>
    <i class="fa fa-newspaper" />
    <h1>Cool News Fetcher</h1>
  </header>

  <nav>
    <button>
      <i class="fa fa-arrow-left" />
    </button>

    <ul>
      <li>Die Zeit</li>
    </ul>
  </nav>

  <main>
    <button on:click={fetchArticles}>
      <i
        title="Refetch Articles"
        on:mouseover={() => (spin = true)}
        on:mouseleave={() => (spin = false)}
        class={`fa fa-sync ${spin ? 'fa-spin' : ''}`} />
    </button>

    <div class="articles">
      {#if articles}
        {#each articles as article}
          <ArticleOverview {...article} />
        {/each}
      {:else if error}
        <div>{error}</div>
      {:else}
        <p>Fetching News...</p>
      {/if}
    </div>
  </main>

  <footer>Created by Gh05d</footer>
</div>
