<script>
    import { navigate } from 'svelte-routing';
    import { createLink } from '../api';
    
    let title = '';
    let url = '';
    let description = '';
    let tagsInput = '';
    let error = null;
    
    async function handleSubmit() {
      if (!title.trim() || !url.trim()) {
        error = 'Por favor completa los campos obligatorios';
        return;
      }
  
      const tags = tagsInput
        .split(',')
        .map(tag => tag.trim())
        .filter(tag => tag !== '');
  
      try {
        await createLink({ title, url, description, tags });
        alert('Enlace añadido correctamente');
        navigate('/');
      } catch (err) {
        error = 'Error al crear el enlace. Por favor intenta de nuevo.';
        console.error(err);
      }
    }
  </script>
  
  <div>
    <h2>Añadir Nuevo Enlace</h2>
    
    {#if error}
      <p class="error">{error}</p>
    {/if}
    
    <form on:submit|preventDefault={handleSubmit}>
      <div>
        <label for="title">Título:</label>
        <input
          type="text"
          id="title"
          bind:value={title}
          required
        />
      </div>
      
      <div>
        <label for="url">URL:</label>
        <input
          type="url"
          id="url"
          bind:value={url}
          required
        />
      </div>
      
      <div>
        <label for="description">Descripción:</label>
        <textarea
          id="description"
          bind:value={description}
        ></textarea>
      </div>
      
      <div>
        <label for="tags">Etiquetas (separadas por comas):</label>
        <input
          type="text"
          id="tags"
          bind:value={tagsInput}
          placeholder="ej. tecnología, programación, svelte"
        />
      </div>
      
      <div class="form-buttons">
        <button type="submit">Guardar</button>
        <button type="button" on:click={() => navigate('/')}>Cancelar</button>
      </div>
    </form>
  </div>