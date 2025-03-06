<script>
    import { onMount } from 'svelte';
    import { fetchLinks, fetchTags } from '../api';
    import { Link } from 'svelte-routing';
    
    let links = [];
    let tags = [];
    let selectedTag = '';
    let loading = true;
    let error = null;
    
    async function loadData() {
      try {
        loading = true;
        await loadLinks();
        await loadTags();
        error = null;
      } catch (err) {
        error = 'Error al cargar datos. Por favor intenta de nuevo.';
        console.error(err);
      } finally {
        loading = false;
      }
    }
    
    async function loadLinks() {
      try {
        links = await fetchLinks(selectedTag);
      } catch (err) {
        error = 'Error al cargar enlaces.';
        console.error(err);
      }
    }
    
    async function loadTags() {
      try {
        tags = await fetchTags();
      } catch (err) {
        error = 'Error al cargar etiquetas.';
        console.error(err);
      }
    }
    
    function handleTagChange(e) {
      selectedTag = e.target.value;
      loadLinks();
    }
    
    onMount(loadData);
  </script>
  
  <h2>Todos los Enlaces</h2>
  
  <div class="tag-filter">
    <label>
      Filtrar por etiqueta:
      <select value={selectedTag} on:change={handleTagChange}>
        <option value="">Todas las etiquetas</option>
        {#each tags as tag}
          <option value={tag.name}>{tag.name}</option>
        {/each}
      </select>
    </label>
  </div>
  
  {#if loading}
    <p>Cargando enlaces...</p>
  {:else if error}
    <p class="error">{error}</p>
  {:else if links.length === 0}
    <p>No se encontraron enlaces.</p>
  {:else}
    <div class="links-list">
      {#each links as link}
        <div class="link-card">
          <h3>
            <Link to={`/link/${link.id}`}>{link.title}</Link>
          </h3>
          <p>{link.description || ''}</p>
          <p>
            <a href={link.url} target="_blank" rel="noopener noreferrer">{link.url}</a>
          </p>
          <div>
            {#each link.tags as tag}
              <span class="tag">{tag.name}</span>
            {/each}
          </div>
        </div>
      {/each}
    </div>
  {/if}