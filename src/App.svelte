<script>
  import { Router, Link, Route } from "svelte-routing";
  import Home from "./routes/Home.svelte";
  import Detail from "./routes/Detail.svelte";
  import AddLink from "./routes/AddLink.svelte";
  
  export let url = "";
</script>

<Router {url}>
  <div class="app">
    <header class="header">
      <h1>Links App</h1>
      <nav>
        <Link to="/" class="nav-button">Inicio</Link>
        <Link to="/add" class="nav-button">Añadir Link</Link>
      </nav>
    </header>

    <main>
      <Route path="/" component={Home} />
      <Route path="/link/:id" component={Detail} />
      <Route path="/add" component={AddLink} />
    </main>
  </div>
</Router>

<style>
  .app {
    max-width: 800px;
    margin: 0 auto;
    padding: 20px;
  }

  .header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-bottom: 20px;
    padding-bottom: 10px;
    border-bottom: 1px solid #ddd;
  }


  :global(.nav-button) {
    background-color: #4285f4;
    color: white;
    border: none;
    padding: 8px 16px;
    border-radius: 4px;
    cursor: pointer;
    margin-right: 10px;
  }

  :global(.link-card) {
    border: 1px solid #ddd;
    padding: 15px;
    margin-bottom: 15px;
    border-radius: 4px;
  }

  :global(.tag) {
    display: inline-block;
    background-color: #eee;
    padding: 3px 8px;
    border-radius: 12px;
    margin-right: 5px;
    font-size: 12px;
  }

  :global(.comment) {
    border-top: 1px solid #eee;
    padding: 10px 0;
  }

  :global(form) {
    margin-bottom: 20px;
  }

  :global(input), 
  :global(textarea), 
  :global(select) {
    width: 100%;
    padding: 8px;
    margin-bottom: 10px;
    border: 1px solid #ddd;
    border-radius: 4px;
  }

  :global(label) {
    display: block;
    margin-bottom: 5px;
  }

  :global(button) {
    background-color: #4285f4;
    color: white;
    border: none;
    padding: 8px 16px;
    border-radius: 4px;
    cursor: pointer;
    margin-right: 10px;
  }

  :global(.error) {
    color: red;
    background-color: #ffeeee;
    padding: 10px;
    border-radius: 4px;
    margin-bottom: 10px;
  }
</style>