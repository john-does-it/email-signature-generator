<script lang="ts">
  import { m } from './paraglide/messages.js'
  import { onMount } from "svelte"
  
  /* root - +page.svelte */
  let colorCode: any 
  let colorCode2: any
  let fontSize: number
  let firstName: string
  let lastName: string
  let companyName: string
  let role: string
  let departmentName: string
  let phoneNumber: string
  let mobilePhoneNumber: string
  let emailAddress: string
  let websiteURL: string
  let facebookPageURL: string
  let linkedinPageURL: string
  let instagramPageUrl: string
  let youtubePageUrl: string
  let tiktokPageUrl: string
  let xPageUrl: string
  
  onMount(() => {
    colorCode = '#000000'
    colorCode2 = '#121212'
    firstName = 'John'
    lastName = 'Doe'
    companyName = 'The Fictive Company'
    role = 'APM Manager'
    departmentName = 'Transformation & Digitalisation'
    phoneNumber = '+3242424242'
    mobilePhoneNumber = '+32420424242'
    emailAddress = 'hello@thefictivecompany.xyz'
    websiteURL = 'https://www.thefictivecompany.xyz'
    facebookPageURL = 'https://www.facebook.com/yourcompanyurl'
    linkedinPageURL = 'https://www.linkedin.com/yourcompanyurl'
    instagramPageUrl = 'https://www.instagram.com/yourcompanyurl'
    youtubePageUrl = 'https://www.youtube.com/yourcompanyurl'
    tiktokPageUrl = 'https://www.tiktok.com/yourcompanyurl'
    xPageUrl = 'https://www.x.com/yourcompanyurl'
  })
</script>

<svelte:head>
  <title>✍️ Générateur de signature d'email personnalisée</title>
  <meta name="description" content="Créer une signature d'email personnalisée en toute simplicité !">
</svelte:head>

<wrapper>
  <p>
    {m.example_message()}
  </p>
  <h1>
    Générer votre signature d'email personnalisée
  </h1>
  <signature-generator-container>
    <form-and-title-container>
      <form>
        <h2>Vos données</h2>
        <fieldset>
          <legend>Générateur de style</legend>
          <colors-code-container>
            <label for="colorCode">
              Code couleur
              <input name="colorCode" type="color" bind:value={colorCode}>
            </label>
            <label for="colorCode2">
              Code couleur 2
              <input name="colorCode" type="color" bind:value={colorCode2}>
            </label>
          </colors-code-container>
          <label for="fontSize">
            Taille de la police
            <select bind:value={fontSize}>
              <option value="12">12px</option>
              <option value="14" selected>14px</option>
              <option value="16">16px</option>
              <option value="20">20px</option>
            </select>
          </label>
        </fieldset>
        <fieldset>
          <legend>Vos informations</legend>
          <label for="companyName">
            Nom de la société
            <input name="companyName" placeholder="The Fictive Company" type="text" bind:value={companyName}>
          </label>
          <label for="firstName">
            Prénom
            <input name="firstName" placeholder="John" type="text" bind:value={firstName}>
          </label>
          <label for="lastName">
            Nom de famille
            <input name="lastName" placeholder="Doe" type="text" bind:value={lastName}>
          </label>
          <label for="function">
            Fonction
            <input name="function" placeholder="APM Manager" type="text" bind:value={role}>
          </label>
          <label for="departmentName">
            Département
            <input name="departmentName" placeholder="Transformation & Digitalisation" type="text" bind:value={departmentName}>
          </label>
        </fieldset>
        <fieldset>
          <legend>Contact</legend>
          <label for="phoneNumber">
            Numéro de téléphone fixe
            <input name="phoneNumber" placeholder="+32420424242" type="text" bind:value={phoneNumber}>
          </label>
          <label for="mobilePhoneNumber">
            Numéro de téléphone portable
            <input name="mobilePhoneNumber" placeholder="+32420424242" type="text" bind:value={mobilePhoneNumber}>
          </label>
          <label for="emailAddress">
            Adresse email
            <input name="emailAddress" placeholder="hello@thefictivecompany.xyz" type="text" bind:value={emailAddress}>
          </label>
        </fieldset>
        <fieldset>
          <legend>Liens</legend>
          <label for="websiteURL">
            Url de votre site web
            <input name="websiteURL" placeholder="https://www.thefictivecompany.xyz" type="text" bind:value={websiteURL}>
          </label>
          <label for="facebookPageURL">
            Url de votre page Facebook
            <input name="facebookPageURL" placeholder="https://www.facebook.com/yourcompanyurl" type="text" bind:value={facebookPageURL}>
          </label>
          <label for="instagramPageUrl">
            Url de votre page Instagram
            <input name="instagramPageUrl" placeholder="https://www.instagram.com/yourcompanyurl" type="text" bind:value={instagramPageUrl}>
          </label>
          <label for="linkedinPageURL">
            Url de votre page LinkedIn
            <input name="linkedinPageURL" placeholder="https://www.linkedin.com/yourcompanyurl" type="text" bind:value={linkedinPageURL}>
          </label>
          <label for="youtubePageUrl">
            Url de votre page YouTube
            <input name="youtubePageUrl" placeholder="https://www.youtube.com/yourcompanyurl" type="text" bind:value={youtubePageUrl}>
          </label>
          <label for="tiktopPageUrl">
            Url de votre page TikTok
            <input name="tiktopPageUrl" placeholder="https://www.tiktok.com/yourcompanyurl" type="text" bind:value={tiktokPageUrl}>
          </label>
          <label for="xPageUrl">
            Url de votre page X
            <input name="xPageUrl" placeholder="https://www.x.com/yourcompanyurl" type="text" bind:value={xPageUrl}>
          </label>
        </fieldset>
      </form>
    </form-and-title-container>
    <table-and-title-container>
      <h2>Prévisualisation</h2>
      <preview-container>
        <table>
          <thead>
            {#if firstName || lastName}
            <tr style="line-height: .5;">
              <td>
                <span style="font-size: {(fontSize * 1.25).toFixed(0)}px; font-family: 'Calibri', sans-serif; font-weight: bold; color: {colorCode};">
                  {#if firstName}
                  <span style="text-transform: capitalize;">{firstName}</span> 
                  {/if}
                  {#if lastName}
                  <span style="text-transform: uppercase;">{lastName}</span>
                  {/if}
                </span>
              </td>
            </tr>
            {/if}
            {#if companyName}
            <tr>
              <td>
                <span style="font-size: {fontSize}px; font-family: 'Calibri', sans-serif; text-transform: capitalize; font-weight: bold; color: {colorCode};">
                  {companyName}
                </span>
              </td>
            </tr>
            {:else}
            <!-- add a bit of spacing if no companyName defined -->
            <tr>
              <td></td>
            </tr>
            {/if}
            {#if role || departmentName}
            <tr style="line-height: .75;">
              <td>
                {#if role}
                <span style="font-size: {fontSize}px; font-family: 'Calibri', sans-serif; text-transform: capitalize; color: {colorCode2};">
                  {role}
                </span>
                {/if}
                {#if role && departmentName}
                <span style="font-size: {fontSize}px; font-family: 'Calibri', sans-serif; text-transform: capitalize; color: {colorCode2};">
                  -
                </span>
                {/if}
                {#if departmentName}
                <span style="font-size: {fontSize}px; font-family: 'Calibri', sans-serif; text-transform: capitalize; color: {colorCode2};">
                  {departmentName}
                </span>
                {/if}
              </td>
            </tr>
            {/if}
            {#if phoneNumber || mobilePhoneNumber}
            <tr>
              <td>
                {#if phoneNumber.length > 0}
                <span style="font-size: {fontSize}px; font-family: 'Calibri', sans-serif; color: {colorCode2};">
                  <span style="font-weight: bold; color: {colorCode};">T</span>
                  <a href="tel:{phoneNumber.trim().replace('+32', '0032')}" title="Appeler {firstName} {lastName} au {phoneNumber}" style="color: {colorCode2};">{phoneNumber}</a>
                </span>
                {/if}
                {#if mobilePhoneNumber.length > 0}
                <span style="font-size: {fontSize}px; font-family: 'Calibri', sans-serif; color: {colorCode2};">
                  <span style="font-weight: bold; color: {colorCode};">M</span>
                  <a href="tel:{mobilePhoneNumber.trim().replace('+32', '0032')}" title="Appeler {firstName} {lastName} au {mobilePhoneNumber}" style="color: {colorCode2};">{mobilePhoneNumber}</a>
                </span>
                {/if}
              </td>
            </tr>
            {#if emailAddress}
            <tr>
              <td>
                {#if emailAddress.length > 0}
                <span style="font-size: {fontSize}px; font-family: 'Calibri', sans-serif; {colorCode2}">
                  <span style="font-weight: bold; color: {colorCode};">E</span>
                  <a href="mailto:{emailAddress}" title="Envoyer un email à {firstName} {lastName} via {emailAddress}" style="color: {colorCode2};">{emailAddress}</a>
                </span>
                {/if}
              </td>
            </tr>
            {/if}
            {/if}
            {#if facebookPageURL || instagramPageUrl || linkedinPageURL || youtubePageUrl || tiktokPageUrl}
            <tr>
              <td>
                <span style="font-size: 24px;">
                  {#if facebookPageURL}
                  <a href="{facebookPageURL}" title="Accéder à la page Facebook de {companyName}">
                    <img src="https://img.icons8.com/?size=100&id=118497&format=png&color=000000" alt="icône Facebook" width="{fontSize * 2}" height="{fontSize * 2}" style="vertical-align: middle;">
                    <!-- 
                    <img src="logo-facebook.png" alt="Logo de Facebook" style="vertical-align: middle; height: 24px;">
                    <svg xmlns="http://www.w3.org/2000/svg" x="0px" y="0px" width="{fontSize * 2}" height="{fontSize * 2}" viewBox="0 0 48 48" style="vertical-align: middle;">
                    <path fill="#039be5" d="M24 5A19 19 0 1 0 24 43A19 19 0 1 0 24 5Z"></path><path fill="#fff" d="M26.572,29.036h4.917l0.772-4.995h-5.69v-2.73c0-2.075,0.678-3.915,2.619-3.915h3.119v-4.359c-0.548-0.074-1.707-0.236-3.897-0.236c-4.573,0-7.254,2.415-7.254,7.917v3.323h-4.701v4.995h4.701v13.729C22.089,42.905,23.032,43,24,43c0.875,0,1.729-0.08,2.572-0.194V29.036z"></path>
                    </svg> 
                    -->
                  </a>
                  {/if}
                  {#if instagramPageUrl}
                  <a href="{instagramPageUrl}" title="Accéder à la page Instagram de {companyName}">
                    <img src="https://img.icons8.com/?size=100&id=32323&format=png&color=000000" alt="icône Instagram" width="{fontSize * 2}" height="{fontSize * 2}" style="vertical-align: middle;">
                  </a>
                  {/if}
                  {#if linkedinPageURL}
                  <a href="{linkedinPageURL}" title="Accéder à la page LinkedIn de {companyName}">
                    <img src="https://img.icons8.com/?size=100&id=13930&format=png&color=000000" alt="icône LinkedIn" width="{fontSize * 2}" height="{fontSize * 2}" style="vertical-align: middle;">
                  </a>
                  {/if}
                  {#if youtubePageUrl}
                  <a href="{youtubePageUrl}" title="Accéder à la page YouTube de {companyName}">
                    <img src="https://img.icons8.com/?size=100&id=19318&format=png&color=000000" alt="icône YouTube" width="{fontSize * 2}" height="{fontSize * 2}" style="vertical-align: middle;">
                  </a>
                  {/if}
                  {#if tiktokPageUrl}
                  <a href="{tiktokPageUrl}" title="Accéder à la page TikTok de {companyName}">
                    <img src="https://img.icons8.com/?size=100&id=118640&format=png&color=000000" alt="icône TikTok" width="{fontSize * 2}" height="{fontSize * 2}" style="vertical-align: middle;">
                  </a>
                  {/if}
                  {#if xPageUrl}
                  <a href="{xPageUrl}" title="Accéder à la page X de {companyName}">
                    <img src="https://img.icons8.com/?size=100&id=ClbD5JTFM7FA&format=png&color=000000" alt="icône X" width="{fontSize * 2}" height="{fontSize * 2}" style="vertical-align: middle;">
                  </a>
                  {/if}
                </span>
              </td>
            </tr>
            {/if}
            <!-- {#if websiteURL}
            <tr>
            <td>
            {#if websiteURL}
            <a href="{websiteURL}" title="Accéder au site web de {companyName}" style="font-size: {fontSize}px; font-family: 'Calibri', sans-serif; {colorCode2}">
            {websiteURL}
            </a>
            {/if}
            </td>
            </tr>
            {/if} -->
          </thead>
        </table>
      </preview-container>
    </table-and-title-container>
  </signature-generator-container>
</wrapper>

<style>
  wrapper {
    width: 1280px;
    max-width: 100%;
    display: flex;
    flex-flow: column;
    margin: auto;
    border-radius: 1em;
  }
  
  colors-code-container {
    display: flex;
    flex-wrap: wrap;
    column-gap: 1em;
    margin-bottom: .5em;
  }
  
  signature-generator-container {
    width: 100%;
    display: flex;
    flex-wrap: wrap;
    justify-content: center;
    column-gap: 2em;
  }
  
  form-and-title-container {
    width: 100%;
    
    @media(min-width: 780px) {
      width: 50%;
    }
  }
  
  form-and-title-container h2 {
    margin: 0 0 .5em 0;
  }
  table-and-title-container h2 {
    margin: 0 0 1em 0;
  }
  
  form, 
  label {
    display: flex;
    flex-flow: column;
  }
  
  form {
    width: 100%;
    gap: .5em;
    margin-bottom: 2em;
  }
  
  label {
    font-size: 12px;
  }
  
  label + label:not([for="colorCode2"]) {
    margin-top: .5em;
  }
  
  table {
    height: fit-content;
  }
  
  preview-container {
    width: 480px;
    max-width: 100%;
    background-color: white;
    display: block;
    padding: 1em;
    border-radius: 12px;
  }
</style>