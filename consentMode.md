Ci sono due modalità testate per il Consent Mode di Google e non prevedono l'utilizzo di CMP quali Iubenda, Cookiebot etc.


1. La prima richiama la funzione consentGranted tramite il click sul pulsante di accettazione dei cookies. 
Si faccia riferimento al [seguente file](https://github.com/paolobtl/consentmode/blob/338673c9498658bf00d097b9afe15cc7dd69470b/consent.html) per testare l'implementazione.
```html
<!-- Global site tag (gtag.js) - Google Analytics -->
<script async src="https://www.googletagmanager.com/gtag/js?id=GA_MEASUREMENT_ID"></script>
<script>
  window.dataLayer = window.dataLayer || [];
  function gtag(){dataLayer.push(arguments);}

  // Default ad_storage to 'denied'.
  gtag('consent', 'default', {
    'ad_storage': 'denied',
    'analytics_storage': 'denied'
  });

<!-- in alternativa qui si puó incollare il codice di Tag Manager togliendo il primo <script> -->
  gtag('js', new Date());
  gtag('config', 'GA_MEASUREMENT_ID');
</script>

<!-- TODO this section should be updated based on your business requirements. 
Ovvero la funzione potrebbe essere richiamata dal pulsante di accettazione dei cookies -->
<script>
  function consentGranted() {
    gtag('consent', 'update', {
      'ad_storage': 'granted',
      'analytics_storage': 'granted'
    });
  }
</script>

<body>
  ...
  <button onclick="consentGranted()">Yes</button>
  ...
</body>
```
***
2. La seconda implementazione coinvolge Tag Manager ed ha la seguente struttura (Utilizzare [questo contenitore](https://github.com/paolobtl/consentmode/blob/0fe28f6927be678159fb11578dd8a4bf18a876b0/GTM_consentMode.json) come esempio):
```html
<script>
  window.dataLayer = window.dataLayer || [];
  function gtag(){dataLayer.push(arguments);}

  // Default ad_storage to 'denied'.
  gtag('consent', 'default', {
    'ad_storage': 'denied',
    'analytics_storage': 'denied'
  });
<!-- Google Tag Manager -->
(function(w,d,s,l,i){w[l]=w[l]||[];w[l].push({'gtm.start':
new Date().getTime(),event:'gtm.js'});var f=d.getElementsByTagName(s)[0],
j=d.createElement(s),dl=l!='dataLayer'?'&l='+l:'';j.async=true;j.src=
'https://www.googletagmanager.com/gtm.js?id='+i+dl;f.parentNode.insertBefore(j,f);
})(window,document,'script','dataLayer','GTM-0000000');</script>
<!-- End Google Tag Manager -->
</script>

<body>
  ...
  <button onclick="gtag('event', 'consentGranted');">YES!</button>
  ...
</body>
```

Si noti come il click del pulsante Yes invia un evento demominato <code>consentGranted</code>. Questo evento ci servirá per attivare lo snippet 

```js
gtag('consent', 'update', {
      'ad_storage': 'granted',
      'analytics_storage': 'granted'
    });
```

ll sovramenzionato snippet andrà inserito in un tag HTML personalizzato.
[Guida di Google Developers](https://developers.google.com/gtagjs/devguide/consent)
