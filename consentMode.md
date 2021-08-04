Ci sono due modalità testate per il Consent Mode di Google e non prevedono l'utilizzo di CMP quali Iubenda, Cookiebot etc.


1. The first method calls the function <code>consentGranted</code> by clicking the Cookie acceptance button. 
Please refer to [this file](https://github.com/paolobtl/consentmode/blob/338673c9498658bf00d097b9afe15cc7dd69470b/consent.html) to test this implementation.
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Consent Mode</title>


    <!-- Global site tag (gtag.js) - Google Analytics -->
<script async src="https://www.googletagmanager.com/gtag/js?id=UA-00000-3"></script>
<script>
  window.dataLayer = window.dataLayer || [];
  function gtag(){dataLayer.push(arguments);}
    gtag('js', new Date());
    gtag('config', 'UA-000000-3');

    gtag('set', 'url_passthrough', true);
    gtag('set', 'ads_data_redaction', false);
  
    <!-- This conditional evaluates if there are cookies already and prevents blocking Google Tags on a page reload -->
    if(document.cookie === ""){
      gtag('consent', 'default', {
        'ad_storage': 'denied',
        'analytics_storage': 'denied',
        'wait_for_update': 500
        });
      }
  else{
      gtag('consent', 'update', {
        'ad_storage': 'granted',
        'analytics_storage':'granted'
        });
     }
</script>


<script>
  function consentGranted() {
    gtag('consent', 'update', {
      'ad_storage': 'granted',
      'analytics_storage':'granted'
    });
  }
</script>
</head>
<body>
  ...
  <button onclick="consentGranted()">Yes</button>
  ...
</body>

</html>
```
***
2. The second implementation involves Tag Manager and has the following structure (Use [this container](https://github.com/paolobtl/consentmode/blob/0fe28f6927be678159fb11578dd8a4bf18a876b0/GTM_consentMode.json) as example):
```html
<script>
  window.dataLayer = window.dataLayer || [];
  function gtag(){dataLayer.push(arguments);}

  gtag('set', 'url_passthrough', true);
   gtag('set', 'ads_data_redaction', false);
  
    if(document.cookie === ""){
      gtag('consent', 'default', {
        'ad_storage': 'denied',
        'analytics_storage': 'denied',
        'wait_for_update': 500
        });
      }
  else{
      gtag('consent', 'update', {
        'ad_storage': 'granted',
        'analytics_storage':'granted'
        });
     }
  
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

Not that the button sends an event called <code>consentGranted</code>. This event will be used in GTM to send the following snippet 

```js
gtag('consent', 'update', {
      'ad_storage': 'granted',
      'analytics_storage': 'granted'
    });
```

The aforementioned snippet will be added as a custom HTML tag.
[Google Developers Article](https://developers.google.com/gtagjs/devguide/consent)
