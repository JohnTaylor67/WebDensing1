<script>
  let websiteName = '';
  let websitePurpose = 'information'; // Alapértelmezett érték
  let requiredPages = { home: true }; // "Főoldal" alapértelmezés szerint kiválasztva
  let stylePreference = 'modern'; 
  let colorScheme = 'blue'; 
  let customColor = false;
  let customColorInput = '';
  let userAccounts = false;
  let languages = { english: true }; // Angol alapértelmezés szerint kiválasztva
  let externalServicesEnabled = false; // Inicializálva, hogy kezelje a checkbox állapotát
  let externalServices = {}; 
  let technicalRequirements = '';

  let formErrors = []; // Tárolja a form validációs hibákat

  function handleSubmit() {
    formErrors = validateForm();
    if (formErrors.length === 0) {
      const formData = {
        websiteName,
        websitePurpose,
        requiredPages: Object.entries(requiredPages).filter(([_, value]) => value).map(([key, _]) => key),
        stylePreference,
        colorScheme,
        customColorInput: customColor ? customColorInput : '',
        userAccounts,
        languages: Object.entries(languages).filter(([_, value]) => value).map(([key, _]) => key),
        externalServices: externalServicesEnabled ? Object.entries(externalServices).filter(([_, value]) => value).map(([key, _]) => key) : [],
        technicalRequirements
      };

      console.log('Form data submitted:', formData);
      // Itt küldheted el az adatokat egy API-nak vagy tárolhatod lokálisan
    } else {
      console.log('Validation errors:', formErrors);
      // Kezeld a hibákat, pl. hibaüzenetek megjelenítése
    }
  }

  function validateForm() {
    let errors = [];
    if (!websiteName.trim()) errors.push('A weboldal neve kötelező.');
    if (!websitePurpose) errors.push('A weboldal célja kötelező.');
    if (Object.keys(requiredPages).length === 0) errors.push('Legalább egy oldalt ki kell választani.');
    if (!stylePreference) errors.push('A design stílus preferenciája kötelező.');
    if (!colorScheme) errors.push('A színkombináció kötelező.');
    return errors;
  }
</script>

  
  {#if formErrors.length > 0}
    <div class="error">
      {#each formErrors as error}
        <p>{error}</p>
      {/each}
    </div>
  {/if}
  
  <form on:submit|preventDefault={handleSubmit}>
    <div class="section">
      <label>Weboldal neve:
        <input type="text" bind:value={websiteName} placeholder="Adja meg a weboldal nevét">
      </label>
    </div>

    <div class="section">
        <span>Weboldal célja:</span>
        {#each ['information', 'business', 'blog', 'portfolio', 'other'] as purpose}
          <label>
            <input type="radio" bind:group={websitePurpose} value={purpose}>
            {purpose === 'information' ? 'Információs' :
             purpose === 'business' ? 'Üzleti' :
             purpose === 'blog' ? 'Blog' :
             purpose === 'portfolio' ? 'Portfólió' : 'Egyéb'}
          </label>
        {/each}
      </div>

      <div class="section">
        <span>Szükséges oldalak:</span>
        {#each ['home', 'about', 'services', 'contact', 'blog'] as page}
          <label>
            <input type="checkbox" bind:checked={requiredPages[page]}>
            {page}
          </label>
        {/each}
      </div>

    <div class="section">
    <span>Design stílus preferencia:</span>
    {#each ['modern', 'classic', 'minimal', 'custom'] as style}
        <label>
        <input type="radio" bind:group={stylePreference} value={style}>
        {style === 'modern' ? 'Modern' :
        style === 'classic' ? 'Klasszikus' :
        style === 'minimal' ? 'Minimalista' : 'Egyedi'}
        </label>
    {/each}
    </div>

    <div class="section">
    <span>Színkombináció:</span>
    {#each ['blue', 'red', 'green', 'custom'] as color}
        <label>
        <input type="radio" bind:group={colorScheme} value={color}>
        {color === 'blue' ? 'Kék' :
        color === 'red' ? 'Piros' :
        color === 'green' ? 'Zöld' : 'Egyéni'}
        </label>
    {/each}
    {#if colorScheme === 'custom'}
        <div>
        <label>
            <input type="radio" bind:group={customColor} value={true}>
            Egyéni színkód
        </label>
        <label>
            <input type="radio" bind:group={customColor} value={false}>
            Színharmónia
        </label>
        {#if customColor}
            <input type="text" bind:value={customColorInput} placeholder="#FFFFFF vagy 'red, blue, green'">
        {:else}
            <textarea bind:value={customColorInput} placeholder="Adj meg több színt, pl. #FF5733, #33C1FF, #33FF57"></textarea>
        {/if}
        </div>
    {/if}
    </div>

    <div class="section">
        <label>
        <input type="checkbox" bind:checked={externalServicesEnabled}>
        Külső szolgáltatásokat integrálni?
        </label>
        {#if externalServicesEnabled}
        <div>
            {#each ['google_maps', 'external_apis', 'other'] as service}
            <label>
                <input type="checkbox" bind:checked={externalServices[service]}>
                {service === 'google_maps' ? 'Google Maps' : 
                service === 'external_apis' ? 'Külső API-k' : 'Egyéb'}
            </label>
            {/each}
        </div>
        {/if}
    </div>
    <div class="section">
    <label>
        Felhasználói fiókok kezelése?
        <input type="checkbox" bind:checked={userAccounts}>
    </label>
    </div>

    <div class="section">
        <span>Nyelvek:</span>
        {#each ['hungarian', 'english', 'german', 'french', 'spanish'] as language}
          <label>
            <input type="checkbox" bind:checked={languages[language]}>
            {language}
          </label>
        {/each}
      </div>

    <div class="section">
    <label>Technikai követelmények:
        <textarea bind:value={technicalRequirements} placeholder="SEO, Teljesítmény, Hozzáférhetőség, stb."></textarea>
    </label>
    </div>

    <button type="submit">Beküld</button>
</form>

<style>
    form {
      display: flex;
      flex-direction: column;
      gap: 20px;
      max-width: 600px;
      margin: 20px auto;
    }
    .section {
      background-color: #f9f9f9;
      padding: 20px;
      border-radius: 8px;
      box-shadow: 0 2px 4px rgba(0,0,0,0.1);
    }
    input, select, textarea, button {
      padding: 8px;
      font-size: 16px;
      width: 100%;
    }
    button {
      background-color: #007BFF;
      color: white;
      border: none;
      cursor: pointer;
      padding: 10px;
      font-size: 18px;
    }
    button:hover {
      background-color: #0056b3;
    }

    .error {
    color: red;
  }

  </style>