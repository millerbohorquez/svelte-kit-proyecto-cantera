<script>
  // No necesitas exportar `goToPage` si usas SvelteKit's routing.
</script>

<style>
  header {
    background-color: #0049ff; 
    color: white; 
    padding: 15px 0; 
    background: linear-gradient(135deg, #207cca, #3c6484, #207cca, #5f8cae); 
  }
  
  .header-container {
    display: flex;
    justify-content: space-between; 
    align-items: center;
    max-width: 1200px; 
    margin: 0 auto; 
    padding: 0 20px; 
  }
  
  .header-logo img {
    height: 40px; 
    cursor: pointer; 
  }
  
  .header-links {
    display: flex; 
    gap: 20px; 
  }
  
  .header-link {
    text-decoration: none;
    color: white; 
    padding: 10px;
    font-size: 16px;
    border-radius: 5px;
  }
  
  .header-link:hover {
    text-decoration: underline; 
    background: linear-gradient(135deg, #207cca, #3c6484, #207cca, #5f8cae); 
    box-shadow: 0px 0px 5px #5C8BFF,
                0px 0px 30px #5C8BFF,
                0px 0px 90px #5C8BFF;
  }
</style>

<header>
  <div class="header-container">
    <!-- Logo que redirige al inicio -->
    <div class="header-logo">
      <a href="/">
        <img src="https://linktic.com/media/2024/03/logo-footer.svg" alt="LINKTIC Logo" />
      </a>
    </div>

    <!-- Links de navegación -->
    <nav class="header-links">
      <a href="/" class="header-link">Quiénes somos</a>
      <a href="/solicitud" class="header-link">Formulario de Solicitud</a>
      <a href="/generalidades" class="header-link">Stakeholders</a>
      <a href="/backlog" class="header-link">BackLog de Alto Nivel</a>
    </nav>
  </div>
</header>