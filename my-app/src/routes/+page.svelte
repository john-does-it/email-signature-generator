<script lang="ts">
  import { onMount } from "svelte"
  import { fade } from "svelte/transition"

  import { m } from "$lib/paraglide/messages"
  import { getLocale, setLocale } from "$lib/paraglide/runtime"

  let fontFamily: string = $state('System-ui')
  let picture: string = $state('https://i0.wp.com/faildesk.net/wp-content/uploads/2012/11/funny-sexy-fail-bill-gates.jpg') 
  let pictureStyle: 'square' | 'round' | 'slightly-round' = $state('square')
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
  let githubPageURL: string = $state('https://www.github.com/yourcompanyurl')
  let facebookPageURL: string = $state('https://www.facebook.com/yourcompanyurl')
  let linkedinPageURL: string = $state('https://www.linkedin.com/yourcompanyurl')
  let instagramPageUrl: string = $state('https://www.instagram.com/yourcompanyurl')
  let youtubePageUrl: string = $state('https://www.youtube.com/yourcompanyurl')
  let tiktokPageUrl: string = $state('https://www.tiktok.com/yourcompanyurl')
  let xPageUrl: string = $state('https://www.x.com/yourcompanyurl')
  let selectedLanguage: LanguageAvailable = $state('fr')
  let lineHeight: number = $state(1.2)
  let visible: boolean = $state(false)

  let heightOfPreviewContainer = $state()
  let seoContentContainer: HTMLDivElement | undefined = $state()

  let configurationText: string = $state('')

  let exampleConfiguration = $derived(JSON.stringify({
    pictureStyle,
    companyName,
    websiteURL,
    githubPageURL,
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

  // ---------- Phone validation helpers (Rune-friendly) ----------
  const allowedCharRegex = /[^0-9+\s()\-]/ // flags any char NOT allowed
  const digitsOnly = (n: string) => n.replace(/\D/g, '')

  // Normalize (trim) inputs via $derived values
  const normPhone = $derived(phoneNumber.trim())
  const normMobile = $derived(mobilePhoneNumber.trim())

  // Leading + checks
  const phoneHasLeadingPlus = $derived(/^\+/.test(normPhone))
  const mobileHasLeadingPlus = $derived(/^\+/.test(normMobile))

  const phoneHasExtraPlus = $derived(normPhone.slice(1).includes('+'))
  const mobileHasExtraPlus = $derived(normMobile.slice(1).includes('+'))

  // Invalid characters (anything outside digits, +, space, (), -)
  const phoneHasInvalidChars = $derived(allowedCharRegex.test(normPhone))
  const mobileHasInvalidChars = $derived(allowedCharRegex.test(normMobile))

  // E.164-ish length (8–15 digits excluding +)
  const phoneDigitsLen = $derived(digitsOnly(normPhone).length)
  const mobileDigitsLen = $derived(digitsOnly(normMobile).length)

  const phoneLenInvalid = $derived(phoneDigitsLen < 8 || phoneDigitsLen > 15)
  const mobileLenInvalid = $derived(mobileDigitsLen < 8 || mobileDigitsLen > 15)

  // tel: href builder — keep a single leading + and digits
  const toTelHref = (n: string) => {
    const t = n.trim()
    const plus = t.startsWith('+') ? '+' : ''
    const digits = t.replace(/\D/g, '')
    return 'tel:' + plus + digits
  }

  function applyConfigurationFromText() {
    try {
      const config = JSON.parse(configurationText)

      if ('pictureStyle' in config) pictureStyle = config.pictureStyle
      if ('companyName' in config) companyName = config.companyName
      if ('websiteURL' in config) websiteURL = config.websiteURL
      if ('githubPageURL' in config) githubPageURL = config.githubPageURL
      if ('facebookPageURL' in config) facebookPageURL = config.facebookPageURL
      if ('linkedinPageURL' in config) linkedinPageURL = config.linkedinPageURL
      if ('instagramPageUrl' in config) instagramPageUrl = config.instagramPageUrl
      if ('youtubePageUrl' in config) youtubePageUrl = config.youtubePageUrl
      if ('tiktokPageUrl' in config) tiktokPageUrl = config.tiktokPageUrl
      if ('xPageUrl' in config) xPageUrl = config.xPageUrl
      if ('colorCode' in config) colorCode = config.colorCode
      if ('colorCode2' in config) colorCode2 = config.colorCode2

      if (config.fontSize) {
        fontSize = config.fontSize
        updateLineHeight(fontSize)
      }

      if (config.selectedLanguage) {
        selectedLanguage = config.selectedLanguage
        setLocale(config.selectedLanguage)
      }
    } catch (error) {
      console.log('❌ Configuration JSON invalide.'+ error)
      return false
    }
  }

  function isJsonValid(json: string): boolean {
    try {
      JSON.parse(json)
      return true
    } catch {
      return false
    }
  }

  function updateLineHeight(fontSize: number) {
    if (fontSize === 12) lineHeight = 1
    if (fontSize === 14) lineHeight = 1.2
    if (fontSize >= 16) lineHeight = 1.4
  }

  function updatePaddingBottom() {
    if (seoContentContainer && window.innerWidth < 1024) {
      seoContentContainer.style.paddingBottom = `${heightOfPreviewContainer}px`
    } else if (seoContentContainer && window.innerWidth > 1024) {
      seoContentContainer.style.paddingBottom = '0px'
    }
  }

  onMount(() => {
    selectedLanguage = getLocale()
    visible = true
  })
</script>

<svelte:window on:resize={updatePaddingBottom} />

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
            <select name="selectedLanguage" id="selectedLanguage" bind:value={selectedLanguage} onchange={(event) => setLocale((event.target as HTMLSelectElement).value as LanguageAvailable)}>
              <option disabled selected>{m.langageSelectionne()}</option>
              <option value="fr">Français</option>
              <option value="en">English</option>
              <option value="nl">Nederlands</option>
            </select>
            <tip>{@html m.infoChangementLangue()}</tip>         
          </language-container>
          <font-container>
            <label for="fontFamily">{m.police()}</label>
            <select name="fontFamily" id="fontFamily" bind:value={fontFamily}>
              <option disabled selected>System-ui</option>
              <option value="System-ui">System-ui</option>
              <option value="Monospace">Monospace</option>
              <option value="Arial">Arial</option>
              <option value="Sans-Serif">Sans-serif</option>
              <option value="Serif">Serif</option>
            </select>
          </font-container>
          <colors-code-container>
            <label for="colorCode">
              {m.codeCouleur()}
              <input name="colorCode" id="colorCode" type="color" bind:value={colorCode}>
            </label>
            <label for="colorCode2">
              {m.codeCouleurDeux()}
              <input name="colorCode2" id="colorCode2" type="color" bind:value={colorCode2}>
            </label>
          </colors-code-container>
          <label for="fontSize">
            {m.taille()}
            <select bind:value={fontSize} id="fontSize" onchange={() => updateLineHeight(fontSize)}>
              <option value={12}>{m.tailleSmall()}</option>
              <option value={14}>{m.tailleNormal()}</option>
              <option value={16}>{m.tailleLarge()}</option>
              <option value={20}>{m.tailleXLarge()}</option>
            </select>
          </label>
        </fieldset>
        <fieldset>
          <legend>{m.vosInformations()}</legend>
          <label for="picture">
            {m.photo()}
            <input name="picture" id="picture" placeholder="https://i0.wp.com/faildesk.net/wp-content/uploads/2012/11/funny-sexy-fail-bill-gates.jpg" type="text" bind:value={picture}>
          </label>
          <tip>{m.tipPicture()}</tip>
          {#if picture}
          <label for="pictureStyle">
            {m.stylePhoto()}
            <select bind:value={pictureStyle} id="pictureStyle">
              <option value="square">{m.carree()}</option>
              <option value="round">{m.ronde()}</option>
              <option value="slightly-round">{m.arrondie()}</option>
            </select>
          </label>
          {/if}
          <label for="companyName">
            {m.nomSociete()}
            <input name="companyName" id="companyName" placeholder="The Fictive Company" type="text" bind:value={companyName}>
          </label>
          <label for="firstName">
            {m.prenom()}
            <input name="firstName" id="firstName" placeholder="John" type="text" bind:value={firstName}>
          </label>
          <label for="lastName">
            {m.nomFamille()}
            <input name="lastName" id="lastName" placeholder="Doe" type="text" bind:value={lastName}>
          </label>
          <label for="function">
            {m.fonction()}
            <input name="function" id="function" placeholder="APM Manager" type="text" bind:value={role}>
          </label>
          <label for="departmentName">
            {m.departement()}
            <input name="departmentName" id="departmentName" placeholder="Transformation & Digitalisation" type="text" bind:value={departmentName}>
          </label>
        </fieldset>
        <fieldset>
          <legend>{m.contact()}</legend>
          <label for="phoneNumber">
            {m.numeroFixe()}
            <input name="phoneNumber" id="phoneNumber" placeholder="+32420424242" type="text" bind:value={phoneNumber}>
          </label>
          <tip>{m.tipNumeroFixe()}</tip>
          {#if normPhone}
          <errors-container>
            {#if !phoneHasLeadingPlus}
              <error-container>{m.errorNeCommencePasAvecPlus()}</error-container>
            {/if}
            {#if phoneHasExtraPlus}
              <error-container>{m.errorPlusEnTrop()}</error-container>
            {/if}
            {#if phoneHasInvalidChars}
              <error-container>{m.errorNumeroCaracteresInvalides()}</error-container>
            {/if}
            {#if phoneLenInvalid}
              <error-container>{m.errorLongueurE164()}</error-container>
            {/if}
          </errors-container>
          {/if}

          <label for="mobilePhoneNumber">
            {m.numeroPortable()}
            <input name="mobilePhoneNumber" id="mobilePhoneNumber" placeholder="+3242042424242" type="text" bind:value={mobilePhoneNumber}>
          </label>
          <tip>{m.tipNumeroPortable()}</tip>
          {#if normMobile}
          <errors-container>
            {#if !mobileHasLeadingPlus}
              <error-container>{m.errorNeCommencePasAvecPlus()}</error-container>
            {/if}
            {#if mobileHasExtraPlus}
              <error-container>{m.errorPlusEnTrop()}</error-container>
            {/if}
            {#if mobileHasInvalidChars}
              <error-container>{m.errorNumeroCaracteresInvalides()}</error-container>
            {/if}
            {#if mobileLenInvalid}
              <error-container>{m.errorLongueurE164()}</error-container>
            {/if}
          </errors-container>
          {/if}        

          <label for="emailAddress">
            {m.adresseEmail()}
            <input name="emailAddress" id="emailAddress" placeholder="hello@thefictivecompany.xyz" type="text" bind:value={emailAddress}>
          </label>
          <errors-container>
            {#if emailAddress.length > 0 && !emailAddress.includes('@')}
              <error-container>{m.errorManqueArobase()}</error-container>
            {/if}
            {#if emailAddress.length > 0 && !emailAddress.includes('.')}
              <error-container>{m.errorManquePoint()}</error-container>
            {/if}
          </errors-container>
        </fieldset>
        <fieldset>
          <legend>{m.liens()}</legend>
          <label for="websiteURL">
            {m.urlSiteWeb()}
            <input name="websiteURL" id="websiteURL" placeholder="https://www.thefictivecompany.xyz" type="text" bind:value={websiteURL}>
          </label>
          <label for="githubPageURL">
            {m.urlGithub()}
            <input name="githubPageURL" id="githubPageURL" placeholder="https://www.github.com/yourcompanyurl" type="text" bind:value={githubPageURL}>
          </label>
          <label for="facebookPageURL">
            {m.urlFacebook()}
            <input name="facebookPageURL" id="facebookPageURL" placeholder="https://www.facebook.com/yourcompanyurl" type="text" bind:value={facebookPageURL}>
          </label>
          <label for="instagramPageUrl">
            {m.urlInstagram()}
            <input name="instagramPageUrl" id="instagramPageUrl" placeholder="https://www.instagram.com/yourcompanyurl" type="text" bind:value={instagramPageUrl}>
          </label>
          <label for="linkedinPageURL">
            {m.urlLinkedin()}
            <input name="linkedinPageURL" id="linkedinPageURL" placeholder="https://www.linkedin.com/yourcompanyurl" type="text" bind:value={linkedinPageURL}>
          </label>
          <label for="youtubePageUrl">
            {m.urlYoutube()}
            <input name="youtubePageUrl" id="youtubePageUrl" placeholder="https://www.youtube.com/yourcompanyurl" type="text" bind:value={youtubePageUrl}>
          </label>
          <label for="tiktopPageUrl">
            {m.urlTiktok()}
            <input name="tiktopPageUrl" id="tiktopPageUrl" placeholder="https://www.tiktok.com/yourcompanyurl" type="text" bind:value={tiktokPageUrl}>
          </label>
          <label for="xPageUrl">
            {m.urlX()}
            <input name="xPageUrl" id="xPageUrl" placeholder="https://www.x.com/yourcompanyurl" type="text" bind:value={xPageUrl}>
          </label>
        </fieldset>
      </form>
    </form-and-title-container>

    <preview-and-title-container bind:clientHeight={heightOfPreviewContainer}>
      <h2>{m.previsualisation()}</h2>
      <preview-container>
        <!-- TODO: add fallback fonts inside style attributes -->
        <table style="line-height: {lineHeight};">
          <thead>
            {#if picture}
            <tr>
              <td>
                {#if pictureStyle === "square"}
                  <img src={picture} alt={m.altPhoto({firstName, lastName})} width=100>
                {/if}
                {#if pictureStyle === "round"}
                  <img src={picture} alt={m.altPhoto({firstName, lastName})} width=100 style="border-radius: 100%;">
                {/if}
                {#if pictureStyle === "slightly-round"}
                  <img src={picture} alt={m.altPhoto({firstName, lastName})} width=100 style="border-radius: 8px;">
                {/if}
              </td>
            </tr>
            {/if}

            {#if firstName || lastName}
            <tr>
              <td>
                <span style="font-size: {(fontSize * 1.25).toFixed(0)}px; font-weight: bold; color: {colorCode}; font-family: {fontFamily}, System-ui, Sans-serif, Arial, Serif;;">
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
                <span style="font-size: {fontSize}px; text-transform: capitalize; font-weight: bold; color: {colorCode}; font-family: {fontFamily}, System-ui, Sans-serif, Arial, Serif;;">
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
                <span style="font-size: {fontSize}px; text-transform: capitalize; color: {colorCode2}; font-family: {fontFamily}, System-ui, Sans-serif, Arial, Serif;;">
                  {role}
                </span>
                {/if}
                {#if role && departmentName}
                <span style="font-size: {fontSize}px; text-transform: capitalize; color: {colorCode2}; font-family: {fontFamily}, System-ui, Sans-serif, Arial, Serif;;">
                  -
                </span>
                {/if}
                {#if departmentName}
                <span style="font-size: {fontSize}px; text-transform: capitalize; color: {colorCode2}; font-family: {fontFamily}, System-ui, Sans-serif, Arial, Serif;;">
                  {departmentName}
                </span>
                {/if}
              </td>
            </tr>
            {/if}

            {#if phoneNumber || mobilePhoneNumber}
            <tr>
              <td>
                {#if normPhone.length > 0}
                <span style="font-size: {fontSize}px; color: {colorCode2}; font-family: {fontFamily}, System-ui, Sans-serif, Arial, Serif;;">
                  <span style="font-weight: bold; color: {colorCode};">T</span>
                  <a href={toTelHref(normPhone)} title={m.phoneCall({ firstName, lastName, number: normPhone })} style="color: {colorCode2};">{phoneNumber}</a>
                </span>
                {/if}
                {#if normMobile.length > 0}
                <span style="font-size: {fontSize}px; color: {colorCode2}; font-family: {fontFamily}, System-ui, Sans-serif, Arial, Serif;;">
                  <span style="font-weight: bold; color: {colorCode};">M</span>
                  <a href={toTelHref(normMobile)} title={m.phoneCall({ firstName, lastName, number: normMobile })} style="color: {colorCode2};">{mobilePhoneNumber}</a>
                </span>
                {/if}
              </td>
            </tr>
            {/if}

            {#if emailAddress}
            <tr>
              <td>
                {#if emailAddress.length > 0}
                <span style="font-size: {fontSize}px; color: {colorCode2}; font-family: {fontFamily}, System-ui, Sans-serif, Arial, Serif;;">
                  <span style="font-weight: bold; color: {colorCode};">E</span>
                  <a href="mailto:{emailAddress}" title={m.emailTo({ firstName, lastName, email: emailAddress })} style="color: {colorCode2};">{emailAddress}</a>
                </span>
                {/if}
              </td>
            </tr>
            {/if}

            {#if websiteURL ||githubPageURL || facebookPageURL || instagramPageUrl || linkedinPageURL || youtubePageUrl || tiktokPageUrl || xPageUrl}
            <tr>
              <td>
                <span style="font-size: 24px;">
                  {#if websiteURL}
                  <a href={websiteURL} title={companyName ? `${m.titleCompanyWebsite({companyName})}` : m.titleGenericWebsite()}>
                    <img src="https://img.icons8.com/?size=100&id=1349&format=png&color=000000" alt={m.altWebsite()} width={fontSize * 2} height={fontSize * 2} style="vertical-align: middle;">
                  </a>
                  {/if}
                  {#if githubPageURL}
                  <a href={githubPageURL} title={companyName ? `${m.titleGithub()} ${companyName}` : m.titleGithub()}>
                    <img src="https://img.icons8.com/?size=100&id=12599&format=png&color=000000" alt={m.altGithub()} width={fontSize * 2} height={fontSize * 2} style="vertical-align: middle;">
                  </a>
                  {/if}
                  {#if facebookPageURL}
                  <a href={facebookPageURL} title={companyName ? `${m.titleFacebook()} ${companyName}` : m.titleFacebook()}>
                    <img src="https://img.icons8.com/?size=100&id=118497&format=png&color=000000" alt={m.altFacebook()} width={fontSize * 2} height={fontSize * 2} style="vertical-align: middle;">
                  </a>
                  {/if}
                  {#if instagramPageUrl}
                  <a href={instagramPageUrl} title={companyName ? `${m.titleInstagram()} ${companyName}` : m.titleInstagram()}>
                    <img src="https://img.icons8.com/?size=100&id=32323&format=png&color=000000" alt={m.altInstagram()} width={fontSize * 2} height={fontSize * 2} style="vertical-align: middle;">
                  </a>
                  {/if}
                  {#if linkedinPageURL}
                  <a href={linkedinPageURL} title={companyName ? `${m.titleLinkedin()} ${companyName}` : m.titleLinkedin()}>
                    <img src="https://img.icons8.com/?size=100&id=13930&format=png&color=000000" alt={m.altLinkedin()} width={fontSize * 2} height={fontSize * 2} style="vertical-align: middle;">
                  </a>
                  {/if}
                  {#if youtubePageUrl}
                  <a href={youtubePageUrl} title={companyName ? `${m.titleYoutube()} ${companyName}` : m.titleYoutube()}>
                    <img src="https://img.icons8.com/?size=100&id=19318&format=png&color=000000" alt={m.altYoutube()} width={fontSize * 2} height={fontSize * 2} style="vertical-align: middle;">
                  </a>
                  {/if}
                  {#if tiktokPageUrl}
                  <a href={tiktokPageUrl} title={companyName ? `${m.titleTiktok()} ${companyName}` : m.titleTiktok()}>
                    <img src="https://img.icons8.com/?size=100&id=118640&format=png&color=000000" alt={m.altTiktok()} width={fontSize * 2} height={fontSize * 2} style="vertical-align: middle;">
                  </a>
                  {/if}
                  {#if xPageUrl}
                  <a href={xPageUrl} title={companyName ? `${m.titleX()} ${companyName}` : m.titleX()}>
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
    </preview-and-title-container>
  </signature-generator-container>

  <configuration-container>
    <h2>{m.configurationJson()}</h2>
    <example-configuration-container>
      {exampleConfiguration}
    </example-configuration-container>
    <textarea name="json-configurator" bind:value={configurationText} placeholder="{m.jsonConfigurationPlaceHolder()}" rows="14"></textarea>
    {#if configurationText.length === 0 || isJsonValid(configurationText) === false}
      <button disabled>{m.appliquer()}</button>
    {:else}
      <button onclick={applyConfigurationFromText}>{m.appliquer()}</button>
    {/if}
  </configuration-container>

  <seo-content-container bind:this={seoContentContainer as HTMLDivElement} style={window.innerWidth < 1024 ? `padding-bottom: ${heightOfPreviewContainer}px;` : ''}> 
    <h2>{m.pourquoiUtiliser()}</h2>
    <h3>{m.simpleEfficace()}</h3>
    <p>{m.paragrapheSimpleEfficace()}</p>

    <h3>{m.openSource()}</h3>
    <p>{@html m.paragrapheOpenSource()}</p>

    <h3>{m.gratuitSansPub()}</h3>
    <p>{m.paragrapheGratuit()}</p>

    <h2>{m.quiEstDev()}</h2>
    <p>{@html m.paragrapheDev()}</p>

    <h2>{m.credits()}</h2>
    <p>{@html m.creditIcons()}</p>
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

  font-container, 
  colors-code-container {
    margin-bottom: .5em;
  }

  font-container,
  language-container{
    display: flex;
    flex-flow: column;
  }

  colors-code-container {
    display: flex;
    flex-wrap: wrap;
    column-gap: 1em;
  }

  language-container {
    flex-flow: column;
  }

  signature-generator-container {
    width: 100%;
    display: flex;
    justify-content: space-between;
    column-gap: 2em;
  }

  form-and-title-container {
    width: 100%;
    @media(min-width: 1024px) {
      width: auto;
    }
  }

  form-and-title-container h2 {
    margin: 0 0 .5em 0;
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
    border: 1px solid var(--soft-sand);
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

  errors-container error-container:last-child {
    margin-bottom: .5em;
  }

  label ~ errors-container {
    margin-top: .25em;
  }

  tip {
    display: block;
    margin-bottom: .5em;
    font-style: italic;
  }

  :global :has(tip ~ error-container) {
    margin-bottom: 0;
  }

  tip, error-container {
    font-size: 12px;
  }

  preview-and-title-container {
    width: 100%;
    position: fixed;
    bottom: 0;
    left: 0;
    background-color: var(--black);
    border-top: 1px solid var(--soft-sand);
    @media(min-width: 1024px) {
      width: 480px;
      position: relative;
      border-top: none;
    }
    h2 {
      margin-left: 1em;
      @media(min-width: 1024px) {
        margin-left: 0;
        margin-top: 0;
      }
    }
  }

  table {
    height: fit-content;
  }

  preview-container {
    background-color: var(--white);
    display: block;
    padding: 1em;
    border-radius: 12px;
  }

  configuration-container {
    width: 100%;
    display: flex;
    flex-flow: column;
    gap: 1em;
    h2 {
      margin-top: 2em;
      margin-bottom: 0;
    }
    button {
      align-self: flex-end;
    }
  }

  textarea {
    resize: vertical;
  }
</style>
