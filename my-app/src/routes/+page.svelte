<script lang="ts">
  /* root - +page.svelte */
  let firstName: string = 'John'
  let lastName: string = 'Doe'
  let role: string = 'APM Manager'
  let departement: string = 'Développement applicatif'
  let phoneNumber: string = '+3242424242'
  let mobilePhoneNumber: string = '+32420424242'
</script>

<svelte:head>
  <title>✍️ Générateur de signature d'email personnalisée pour RESA</title>
  <meta name="description" content="Générer votre signature d'email RESA personnalisée en toute simplicité !">
</svelte:head>

<wrapper>
  <h1>
    ✍️ Générateur de signature d'email personnalisée pour RESA
  </h1>
  <form>
    <label for="first-name">
      Prénom
      <input name="first-name" placeholder="John" type="text" bind:value={firstName}>
    </label>
    <label for="last-name">
      Nom de famille
      <input name="last-name" placeholder="Doe" type="text" bind:value={lastName}>
    </label>
    <label for="function">
      Fonction
      <input name="function" placeholder="APM Manager" type="text" bind:value={role}>
    </label>
    <label for="departement">
      Département
      <!-- select + option should be more appropriate for that field -->
      <input name="departement" placeholder="Développement applicatif" type="text" bind:value={departement}>
    </label>
    <label for="phone-number">
      Numéro de téléphone fixe
      <input name="phone-number" placeholder="+32420424242" type="text" bind:value={phoneNumber}>
    </label>
    <label for="mobilePhoneNumber">
      Numéro de téléphone portable
      <input name="mobilePhoneNumber" placeholder="+32420424242" type="text" bind:value={mobilePhoneNumber}>
    </label>
  </form>
  <table style="line-height: 1.2;">
    <thead>
      <tr>
        <td>
          <span style="font-size: 16px; font-family: 'Calibri', sans-serif; font-weight: bold; color: #E84E0F;">
            <span style="text-transform: capitalize;">{firstName}</span> 
            <span style="text-transform: uppercase;">{lastName}</span>
          </span>
        </td>
      </tr>
      <tr>
        <td>
          <span style="font-size: 14px; font-family: 'Calibri', sans-serif; text-transform: capitalize;">
            {role}
          </span>
        </td>
      </tr>
      <tr>
        <td>
          <span style="font-size: 14px; font-family: 'Calibri', sans-serif; text-transform: capitalize;">
            {departement}
          </span>
        </td>
      </tr>
      <tr>
        <td>
          {#if phoneNumber.length > 0}
          <span style="font-size: 14px; font-family: 'Calibri', sans-serif; text-transform: capitalize;">
            <span style="color: #E84E0F; font-weight: bold;">T</span>
            <span style="width: 12px;"></span>
            <a href="tel:{phoneNumber.trim().replace('+32', '0032')}" title="Appeler {firstName} {lastName} au {phoneNumber}">{phoneNumber}</a>
          </span>
          {/if}
          {#if mobilePhoneNumber.length > 0}
          <span style="font-size: 14px; font-family: 'Calibri', sans-serif; text-transform: capitalize;">
            <span style="color: #E84E0F; font-weight: bold;">M</span>
            <span style="width: 12px;"></span>
            <a href="tel:{mobilePhoneNumber.trim().replace('+32', '0032')}" title="Appeler {firstName} {lastName} au {mobilePhoneNumber}">{mobilePhoneNumber}</a>
          </span>
          {/if}
        </td>
      </tr>
      <!-- vertical spacing hack because we can't apply any margin inside table -->
      <tr style="height: 16px;">
        <td></td>
      </tr>
      <tr style="vertical-align: top;">
        <td>
          <span style="font-size: 24px;">
            <!-- svelte-ignore a11y_invalid_attribute -->
            <!-- svelte-ignore a11y_consider_explicit_label -->
            <a href="https://www.facebook.com/people/RESA/100063492733041/" title="Accéder à la page Facebook de RESA">
              <img src="logo-facebook.png" alt="Logo de Facebook" style="vertical-align: middle; height: 24px;">
            </a>
            <!-- svelte-ignore a11y_invalid_attribute -->
            <!-- svelte-ignore a11y_consider_explicit_label -->
            <a href="https://www.linkedin.com/company/resa-grd/" title="Accéder à la page LinkedIn de RESA">
              <img src="logo-linkedin.png" alt="Logo de LinkedIn" style="vertical-align: middle; height: 24px;">
            </a>
            <!-- svelte-ignore a11y_invalid_attribute -->
            <!-- svelte-ignore a11y_consider_explicit_label -->
            <a href="https://www.youtube.com/@resasa8766" title="Accéder à la page YouTube de RESA">
              <img src="logo-youtube.png" alt="Logo de YouTube" style="vertical-align: middle; height: 24px;">
            </a>
            <!-- svelte-ignore a11y_invalid_attribute -->
            <!-- svelte-ignore a11y_consider_explicit_label -->
            <a href="https://www.resa.be" title="Accéder au site web de RESA">
              <img src="logo-resa.png" alt="Logo de RESA" style="vertical-align: middle; height: 24px;">
            </a>
          </span>
        </td>
      </tr>
      <tr>
        <td>
          <img src="banner-resa.png"  alt="Bannière de RESA" style="width: 240px;">
        </td>
      </tr>
    </thead>
  </table>
</wrapper>

<style>
  wrapper {
    display: flex;
    flex-flow: column;
  }

  form, 
  label {
    display: flex;
    flex-flow: column;
  }
  
  form {
    gap: .5em;
    margin-bottom: 2em;
  }

  input {
    max-width: 320px;
  }
</style>