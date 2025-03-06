<script>
    import { onMount } from 'svelte';
    import { navigate } from 'svelte-routing';
    import { fetchLinkById, addComment, addVote } from '../api';
    
    export let id;
    
    let link = null;
    let userName = '';
    let commentText = '';
    let loading = true;
    let error = null;
    
    $: totalVotes = link && link.votes 
      ? link.votes.reduce((sum, vote) => sum + vote.value, 0) 
      : 0;
    
    async function loadLink() {
      try {
        loading = true;
        link = await fetchLinkById(id);
        error = null;
      } catch (err) {
        error = 'Error al cargar los detalles del enlace.';
        console.error(err);
      } finally {
        loading = false;
      }
    }
    
    async function handleVote() {
      try {
        await addVote(id);
        await loadLink();
        alert('Voto registrado correctamente');
      } catch (err) {
        alert('Error al registrar el voto');
        console.error(err);
      }
    }
    
    async function handleSubmitComment(e) {
      if (!userName.trim() || !commentText.trim()) {
        alert('Por favor completa todos los campos');
        return;
      }
  
      try {
        await addComment(id, { 
          userName, 
          text: commentText 
        });
        await loadLink();
        userName = '';
        commentText = '';
        alert('Comentario añadido correctamente');
      } catch (err) {
        alert('Error al añadir el comentario');
        console.error(err);
      }
    }
    
    onMount(loadLink);
  </script>
  
  {#if loading}
    <p>Cargando detalles del enlace...</p>
  {:else if error}
    <p class="error">{error}</p>
  {:else if !link}
    <p>No se encontró el enlace.</p>
  {:else}
    <div class="link-detail">
      <h2>{link.title}</h2>
      <p>{link.description || ''}</p>
      <p>
        <a href={link.url} target="_blank" rel="noopener noreferrer">{link.url}</a>
      </p>
      
      <div>
        {#each link.tags as tag}
          <span class="tag">{tag.name}</span>
        {/each}
      </div>
      
      <div class="vote-section">
        <h3>Votar por este enlace</h3>
        <p>Votos actuales: <strong>{totalVotes}</strong></p>
        <button on:click={handleVote}>Me gusta</button>
      </div>
      
      <div class="comments-section">
        <h3>Comentarios</h3>
        
        {#if !link.comments || link.comments.length === 0}
          <p>Aún no hay comentarios.</p>
        {:else}
          {#each link.comments as comment}
            <div class="comment">
              <p>{comment.text}</p>
              <small>Por: {comment.userName}</small>
            </div>
          {/each}
        {/if}
        
        <h4>Añadir un Comentario</h4>
        <form on:submit|preventDefault={handleSubmitComment}>
          <div>
            <label for="userName">Tu Nombre:</label>
            <input
              type="text"
              id="userName"
              bind:value={userName}
              required
            />
          </div>
          <div>
            <label for="commentText">Comentario:</label>
            <textarea
              id="commentText"
              bind:value={commentText}
              required
            ></textarea>
          </div>
          <button type="submit">Enviar</button>
        </form>
      </div>
  
      <button on:click={() => navigate('/')}>Volver a la lista</button>
    </div>
  {/if}