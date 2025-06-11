<script lang="ts">
  import { onMount } from "svelte"
  import { fade } from "svelte/transition"

  import { jsonConfigurationPlaceHolder, m } from "$lib/paraglide/messages"
  import { getLocale, setLocale } from "$lib/paraglide/runtime"
  
  let colorCode: string = $state('#000000')
  let colorCode2: string = $state('#121212')
  let fontSize: number = $state(14)
  let firstName: string = $state('John')
  let lastName: string = $state('Doe')
  let companyName: string = $state('The Fictive Company')
  let role: string = $state('APM Manager')
  let departmentName: string = $state('Transformation & Digitalisation')
  let phoneNumber: string = $state('+3242424242')
  let mobilePhoneNumber: string = $state('+32420424242')
  let emailAddress: string = $state('hello@thefictivecompany.xyz')
  let websiteURL: string = $state('https://www.thefictivecompany.xyz')
  let facebookPageURL: string = $state('https://www.facebook.com/yourcompanyurl')
  let linkedinPageURL: string = $state('https://www.linkedin.com/yourcompanyurl')
  let instagramPageUrl: string = $state('https://www.instagram.com/yourcompanyurl')
  let youtubePageUrl: string = $state('https://www.youtube.com/yourcompanyurl')
  let tiktokPageUrl: string = $state('https://www.tiktok.com/yourcompanyurl')
  let xPageUrl: string = $state('https://www.x.com/yourcompanyurl')
  let selectedLanguage: LanguageAvailable = $state('fr')
  let lineHeight: number = $state(1.2)
  let visible: boolean = $state(false)
  
  let configurationText: string = $state('')
  
  let exampleConfiguration = $derived(JSON.stringify({
    companyName,
    websiteURL,
    facebookPageURL,
    linkedinPageURL,
    instagramPageUrl,
    youtubePageUrl,
    tiktokPageUrl,
    xPageUrl,
    colorCode,
    colorCode2,
    fontSize,
    selectedLanguage
  }, null, 2))
  
  function applyConfigurationFromText() {
    try {
      const config = JSON.parse(configurationText)
      
      if (config.companyName) companyName = config.companyName
      if (config.websiteURL) websiteURL = config.websiteURL
      if (config.facebookPageURL) facebookPageURL = config.facebookPageURL
      if (config.linkedinPageURL) linkedinPageURL = config.linkedinPageURL
      if (config.instagramPageUrl) instagramPageUrl = config.instagramPageUrl
      if (config.youtubePageUrl) youtubePageUrl = config.youtubePageUrl
      if (config.tiktokPageUrl) tiktokPageUrl = config.tiktokPageUrl
      if (config.xPageUrl) xPageUrl = config.xPageUrl
      if (config.colorCode) colorCode = config.colorCode
      if (config.colorCode2) colorCode2 = config.colorCode2
      if (config.fontSize) {
        fontSize = config.fontSize
        updateLineHeight(fontSize)
      }
      if (config.selectedLanguage) {
        selectedLanguage = config.selectedLanguage
        setLocale(config.selectedLanguage)
      }
    } catch (e) {
      alert('❌ Configuration JSON invalide.')
    }
  }
  
  function updateLineHeight(fontSize: number) {
    if (fontSize === 12) lineHeight = 1
    if (fontSize === 14) lineHeight = 1.2
    if (fontSize >= 16) lineHeight = 1.4
  }
  
  onMount(() => {
    selectedLanguage = getLocale()
    visible = true
  })
</script>

<svelte:head>
<title>{m.title()}</title>
<meta name="description" content="{m.metadescription()}">
</svelte:head>

{#if visible}
<wrapper transition:fade>
  <h1>
    {m.genererVotreSignature()}
  </h1>
  <signature-generator-container>
    <form-and-title-container>
      <form>
        <h2>{m.vosDonnees()}</h2>
        <fieldset>
          <legend>{m.generateurDeStyle()}</legend>
          <language-container>
            <label for="selectedLanguage">{m.langageSelectionne()}</label>
            <select name="selectedLanguage" bind:value={selectedLanguage} onchange={(event) => setLocale((event.target as HTMLSelectElement).value as LanguageAvailable)}>
              <option disabled selected>{m.langageSelectionne()}</option>
              <option value="fr">Français</option>
              <option value="en">English</option>
              <option value="nl">Nederlands</option>
            </select>
            <tip>{@html m.infoChangementLangue()}</tip>         
          </language-container>
          <colors-code-container>
            <label for="colorCode">
              {m.codeCouleur()}
              <input name="colorCode" type="color" bind:value={colorCode}>
            </label>
            <label for="colorCode2">
              {m.codeCouleurDeux()}
              <input name="colorCode" type="color" bind:value={colorCode2}>
            </label>
          </colors-code-container>
          <label for="fontSize">
            {m.taille()}
            <select bind:value={fontSize} onchange={() => updateLineHeight(fontSize)}>
              <option value={12}>{m.tailleSmall()}</option>
              <option value={14}>{m.tailleNormal()}</option>
              <option value={16}>{m.tailleLarge()}</option>
              <option value={20}>{m.tailleXLarge()}</option>
            </select>
          </label>
        </fieldset>
        <fieldset>
          <legend>{m.vosInformations()}</legend>
          <label for="companyName">
            {m.nomSociete()}
            <input name="companyName" placeholder="The Fictive Company" type="text" bind:value={companyName}>
          </label>
          <label for="firstName">
            {m.prenom()}
            <input name="firstName" placeholder="John" type="text" bind:value={firstName}>
          </label>
          <label for="lastName">
            {m.nomFamille()}
            <input name="lastName" placeholder="Doe" type="text" bind:value={lastName}>
          </label>
          <label for="function">
            {m.fonction()}
            <input name="function" placeholder="APM Manager" type="text" bind:value={role}>
          </label>
          <label for="departmentName">
            {m.departement()}
            <input name="departmentName" placeholder="Transformation & Digitalisation" type="text" bind:value={departmentName}>
          </label>
        </fieldset>
        <fieldset>
          <legend>{m.contact()}</legend>
          <label for="phoneNumber">
            {m.numeroFixe()}
            <input name="phoneNumber" placeholder="+32420424242" type="text" bind:value={phoneNumber}>
          </label>
          <tip>{m.tipNumeroFixe()}</tip>
          {#if phoneNumber}
          <errors-container>
            {#if !phoneNumber.includes('+')}
            <error>{m.errorManquePlus()}</error>
            {/if}
            {#if phoneNumber.length < 11}
            <error>{m.errorNumeroFixeManquant({ count: phoneNumber.length })}</error>
            {/if}
            {#if phoneNumber.length > 11}
            <error>{m.errorNumeroFixeTropLong({ count: phoneNumber.length })}</error>
            {/if}
            {#if /[^\d+]/.test(phoneNumber)}
            <error>{m.errorNumeroCaracteresInvalides()}</error>
            {/if}
          </errors-container>
          {/if}
          <label for="mobilePhoneNumber">
            {m.numeroPortable()}
            <input name="mobilePhoneNumber" placeholder="+32420424242" type="text" bind:value={mobilePhoneNumber}>
          </label>
          <tip>{m.tipNumeroPortable()}</tip>
          {#if mobilePhoneNumber}
          <errors-container>
            {#if !mobilePhoneNumber.includes('+')}
            <error>{m.errorManquePlus()}</error>
            {/if}
            {#if mobilePhoneNumber.length < 12}
            <error>{m.errorNumeroPortableManquant({ count: mobilePhoneNumber.length })}</error>
            {/if}
            {#if mobilePhoneNumber.length > 12}
            <error>{m.errorNumeroPortableTropLong({ count: mobilePhoneNumber.length })}</error>
            {/if}
            {#if /[^\d+]/.test(mobilePhoneNumber)}
            <error>{m.errorNumeroCaracteresInvalides()}</error>
            {/if}
          </errors-container>
          {/if}        
          <label for="emailAddress">
            {m.adresseEmail()}
            <input name="emailAddress" placeholder="hello@thefictivecompany.xyz" type="text" bind:value={emailAddress}>
          </label>
        </fieldset>
        <fieldset>
          <legend>{m.liens()}</legend>
          <label for="websiteURL">
            {m.urlSiteWeb()}
            <input name="websiteURL" placeholder="https://www.thefictivecompany.xyz" type="text" bind:value={websiteURL}>
          </label>
          <label for="facebookPageURL">
            {m.urlFacebook()}
            <input name="facebookPageURL" placeholder="https://www.facebook.com/yourcompanyurl" type="text" bind:value={facebookPageURL}>
          </label>
          <label for="instagramPageUrl">
            {m.urlInstagram()}
            <input name="instagramPageUrl" placeholder="https://www.instagram.com/yourcompanyurl" type="text" bind:value={instagramPageUrl}>
          </label>
          <label for="linkedinPageURL">
            {m.urlLinkedin()}
            <input name="linkedinPageURL" placeholder="https://www.linkedin.com/yourcompanyurl" type="text" bind:value={linkedinPageURL}>
          </label>
          <label for="youtubePageUrl">
            {m.urlYoutube()}
            <input name="youtubePageUrl" placeholder="https://www.youtube.com/yourcompanyurl" type="text" bind:value={youtubePageUrl}>
          </label>
          <label for="tiktopPageUrl">
            {m.urlTiktok()}
            <input name="tiktopPageUrl" placeholder="https://www.tiktok.com/yourcompanyurl" type="text" bind:value={tiktokPageUrl}>
          </label>
          <label for="xPageUrl">
            {m.urlX()}
            <input name="xPageUrl" placeholder="https://www.x.com/yourcompanyurl" type="text" bind:value={xPageUrl}>
          </label>
        </fieldset>
      </form>
    </form-and-title-container>
    <table-and-title-container>
      <h2>{m.previsualisation()}</h2>
      <preview-and-configuration-container>
        <preview-container>
          <table style="line-height: {lineHeight};">
            <thead>
              {#if firstName || lastName}
              <tr>
                <td>
                  <span style="font-size: {(fontSize * 1.25).toFixed(0)}px; font-weight: bold; color: {colorCode};">
                    {#if firstName}
                    <span>{firstName}</span> 
                    {/if}
                    {#if lastName}
                    <span>{lastName}</span>
                    {/if}
                  </span>
                </td>
              </tr>
              {/if}
              {#if companyName}
              <tr>
                <td>
                  <span style="font-size: {fontSize}px; text-transform: capitalize; font-weight: bold; color: {colorCode};">
                    {companyName}
                  </span>
                </td>
              </tr>
              {:else}
              <tr><td></td></tr>
              {/if}
              
              {#if role || departmentName}
              <tr style="line-height: .75;">
                <td>
                  {#if role}
                  <span style="font-size: {fontSize}px; text-transform: capitalize; color: {colorCode2};">
                    {role}
                  </span>
                  {/if}
                  {#if role && departmentName}
                  <span style="font-size: {fontSize}px; text-transform: capitalize; color: {colorCode2};">
                    -
                  </span>
                  {/if}
                  {#if departmentName}
                  <span style="font-size: {fontSize}px; text-transform: capitalize; color: {colorCode2};">
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
                  <span style="font-size: {fontSize}px; color: {colorCode2};">
                    <span style="font-weight: bold; color: {colorCode};">T</span>
                    <a href="tel:{phoneNumber.trim().replace('+', '00')}" title={m.phoneCall({ firstName, lastName, number: phoneNumber })} style="color: {colorCode2};">{phoneNumber}</a>
                  </span>
                  {/if}
                  {#if mobilePhoneNumber.length > 0}
                  <span style="font-size: {fontSize}px; color: {colorCode2};">
                    <span style="font-weight: bold; color: {colorCode};">M</span>
                    <a href="tel:{mobilePhoneNumber.trim().replace('+32', '00')}" title={m.phoneCall({ firstName, lastName, number: mobilePhoneNumber })} style="color: {colorCode2};">{mobilePhoneNumber}</a>
                  </span>
                  {/if}
                </td>
              </tr>
              {#if emailAddress}
              <tr>
                <td>
                  {#if emailAddress.length > 0}
                  <span style="font-size: {fontSize}px; {colorCode2}">
                    <span style="font-weight: bold; color: {colorCode};">E</span>
                    <a href="mailto:{emailAddress}" title={m.emailTo({ firstName, lastName, email: emailAddress })} style="color: {colorCode2};">{emailAddress}</a>
                  </span>
                  {/if}
                </td>
              </tr>
              {/if}
              {/if}
              
              {#if facebookPageURL || instagramPageUrl || linkedinPageURL || youtubePageUrl || tiktokPageUrl || xPageUrl}
              <tr>
                <td>
                  <span style="font-size: 24px;">
                    {#if facebookPageURL}
                    <a href={facebookPageURL} title={companyName ? `${m.titleFacebook()} de ${companyName}` : m.titleFacebook()}>
                      <img src="https://img.icons8.com/?size=100&id=118497&format=png&color=000000" alt={m.altFacebook()} width={fontSize * 2} height={fontSize * 2} style="vertical-align: middle;">
                    </a>
                    {/if}
                    {#if instagramPageUrl}
                    <a href={instagramPageUrl} title={companyName ? `${m.titleInstagram()} de ${companyName}` : m.titleInstagram()}>
                      <img src="https://img.icons8.com/?size=100&id=32323&format=png&color=000000" alt={m.altInstagram()} width={fontSize * 2} height={fontSize * 2} style="vertical-align: middle;">
                    </a>
                    {/if}
                    {#if linkedinPageURL}
                    <a href={linkedinPageURL} title={companyName ? `${m.titleLinkedin()} de ${companyName}` : m.titleLinkedin()}>
                      <img src="https://img.icons8.com/?size=100&id=13930&format=png&color=000000" alt={m.altLinkedin()} width={fontSize * 2} height={fontSize * 2} style="vertical-align: middle;">
                    </a>
                    {/if}
                    {#if youtubePageUrl}
                    <a href={youtubePageUrl} title={companyName ? `${m.titleYoutube()} de ${companyName}` : m.titleYoutube()}>
                      <img src="https://img.icons8.com/?size=100&id=19318&format=png&color=000000" alt={m.altYoutube()} width={fontSize * 2} height={fontSize * 2} style="vertical-align: middle;">
                    </a>
                    {/if}
                    {#if tiktokPageUrl}
                    <a href={tiktokPageUrl} title={companyName ? `${m.titleTiktok()} de ${companyName}` : m.titleTiktok()}>
                      <img src="https://img.icons8.com/?size=100&id=118640&format=png&color=000000" alt={m.altTiktok()} width={fontSize * 2} height={fontSize * 2} style="vertical-align: middle;">
                    </a>
                    {/if}
                    {#if xPageUrl}
                    <a href={xPageUrl} title={companyName ? `${m.titleX()} de ${companyName}` : m.titleX()}>
                      <img src="https://img.icons8.com/?size=100&id=ClbD5JTFM7FA&format=png&color=000000" alt={m.altX()} width={fontSize * 2} height={fontSize * 2} style="vertical-align: middle;">
                    </a>
                    {/if}
                  </span>
                </td>
              </tr>
              {/if}
            </thead>
          </table>
        </preview-container>
        <configuration-container>
          <div>
            {exampleConfiguration}
          </div>
          <textarea bind:value={configurationText} placeholder="{m.jsonConfigurationPlaceHolder()}" rows="14"></textarea>
          <button onclick={applyConfigurationFromText}>{m.appliquer()}</button>
        </configuration-container>
      </preview-and-configuration-container>
    </table-and-title-container>
  </signature-generator-container>
  <seo-content-container>
    <h2>Pourquoi utiliser ce générateur de signature d'email ?</h2>
    <h3>Simple et efficace</h3>
    <p>
      Lorem ipsum dolor, sit amet consectetur adipisicing elit. 
      Blanditiis totam quisquam sapiente quo, eius harum! 
      Inventore commodi unde vitae quo eveniet odit dignissimos nobis perspiciatis consequuntur enim. Odit, fuga est.
    </p>
    <h3>Open source</h3>
    <p>
      Lorem ipsum dolor, sit amet consectetur adipisicing elit. 
      Blanditiis totam quisquam sapiente quo, eius harum! 
      Inventore commodi unde vitae quo eveniet odit dignissimos nobis perspiciatis consequuntur enim. Odit, fuga est.
    </p>
    <h3>100% gratuit et sans publicité</h3>
    <p>
      Lorem ipsum dolor, sit amet consectetur adipisicing elit. 
      Blanditiis totam quisquam sapiente quo, eius harum! 
      Inventore commodi unde vitae quo eveniet odit dignissimos nobis perspiciatis consequuntur enim. Odit, fuga est.
    </p>
    <h2>Qui est le développeur de ce générateur de signature d'email ?</h2>
    <p>
      Lorem ipsum dolor, sit amet consectetur adipisicing elit. 
      Blanditiis totam quisquam sapiente quo, eius harum! 
      Inventore commodi unde vitae quo eveniet odit dignissimos nobis perspiciatis consequuntur enim. Odit, fuga est.
    </p>
  </seo-content-container>
</wrapper>
{/if}

<style>
  wrapper {
    width: 1024px;
    max-width: 100%;
    display: flex;
    flex-flow: column;
    margin: auto;
    border-radius: 1em;
  }
  
  h1 {
    margin: left;
  }
  
  language-container,
  colors-code-container {
    display: flex;
    margin-bottom: .5em;
  }
  
  colors-code-container {
    flex-wrap: wrap;
    column-gap: 1em;
  }
  
  language-container {
    flex-flow: column;
  }
  
  signature-generator-container {
    width: 100%;
    display: flex;
    flex-wrap: wrap;
    column-gap: 2em;
  }
  
  form-and-title-container {
    width: 100%;
    
    @media(min-width: 1040px) {
      max-width: 480px;
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
  
  legend {
    font-weight: bold;
    padding: 0 1em;
  }
  
  fieldset {
    margin-top: -.25em;
    padding-bottom: 1em;
    border: 1px solid white;
    border-radius: 4px;
  }
  
  label {
    font-size: 12px;
  }
  
  label + label:not([for="colorCode2"]) {
    margin-top: .5em;
  }
  
  errors-container {
    display: flex;
    flex-flow: column;
  }
  
  errors-container error:last-child {
    margin-bottom: .5em;
  }
  
  tip {
    display: block;
    margin-bottom: .5em;
    font-style: italic;
  }
  
  :global :has(tip ~ error-container) {
    margin-bottom: 0;
  }
  
  tip, error {
    font-size: 12px;
  }
  
  table-and-title-container {
    width: 100%;

    @media(min-width: 1024px) {
      width: 480px;
    }
  }

  table {
    height: fit-content;
  }
  
  preview-and-configuration-container {
    width: 100%;
    max-width: 100%;
    display: flex;
    flex-flow: column;
    gap: 1em;
    
    @media(min-width: 1024px) {
      width: 480px;
    }
  }
  
  preview-container {
    background-color: white;
    display: block;
    padding: 1em;
    border-radius: 12px;
  }
  
  configuration-container {
    width: 100%;
    display: flex;
    flex-flow: column;
    gap: 1em;
  }
  
  textarea {
    resize: vertical;
  }
</style>