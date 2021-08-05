Here I am using two ways to implement the Consent Mode without using CMPs as Iubenda, Cookiebot etc.
Ideally, you wanna implement one of this if you want to create your own cookie banner or you are using a generic cookie banner.

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
    
  //Initialize Data Layer
  window.dataLayer = window.dataLayer || [];
  function gtag(){dataLayer.push(arguments);}
    gtag('js', new Date());
    gtag('config', 'UA-000000-3');
    
    //Update these boolean parameters on your needs
    gtag('set', 'url_passthrough', true);
    gtag('set', 'ads_data_redaction', true);
    
    //Check if cookies exist, if not, sets the storages as "denied" adn calls the "default" command.
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
    <!-- Ideally you wanna call and change the 'update' command based on user's preferences . -->
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
2. The second implementation involves Tag Manager and has the following structure (Use [this container](https://raw.githubusercontent.com/paolobtl/consentmode/main/GTM_consentMode.json) as example. Press CTRL or CMD + S to save it):
```html
<script>
  window.dataLayer = window.dataLayer || [];
  function gtag(){dataLayer.push(arguments);}

  gtag('set', 'url_passthrough', true);
   gtag('set', 'ads_data_redaction', true);
  
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

Note that the button sends an event called <code>consentGranted</code>. This event will be used in GTM to send the following snippet 

```js
gtag('consent', 'update', {
      'ad_storage': 'granted',
      'analytics_storage': 'granted'
    });
```

The aforementioned snippet will be added as a custom HTML tag.
[Google Developers Article](https://developers.google.com/gtagjs/devguide/consent)
