## Consent Mode

This repo aims to be an easy and understandable guide to the implementation of the [Consent Mode](https://support.google.com/google-ads/answer/10000067?hl=en) by Google if you are using your own cookie banner.
There are three files:
1. **consent.html** is an HTML page that show the logic behind the consent mode
2. **GTM_consentMode.json** it is a Google Tag Manager container that can be [imported](https://support.google.com/tagmanager/answer/6106997?hl=en#import) in your GTM account
3. **consentMode.md** explains two ways to implement this feature if you are using your own cookie banner
***
The logic behind the Consent Mode itself it is quite easy:
- The command <code>gtag('consent', 'default'</code> is called before any Google tag and before the cookie banner. At this stage the Analytics/Ads or GTM tag **has** to be   loaded as well. No cookies will be saved on visitor's machine
- The cookied banner loads and waits for the user's choice
- The selection on the cookie banner calls the command <code>gtag('consent', 'update'</code> which should update the tags accordingly to the user's preferencs
***
I am using two ways to implement the Consent Mode without using CMPs as Iubenda, Cookiebot etc.
Ideally, you wanna implement one of this if you want to create your own cookie banner or you are using a generic cookie banner.

Additional links from Google:
- [Consent management platform integrations](https://support.google.com/tagmanager/answer/10718549#cmp-integrations)
- [Google’s Additional Consent Mode technical specification - D&V360](https://support.google.com/displayvideo/answer/9681920?hl=en)
- [About consent mode](https://support.google.com/google-ads/answer/10000067?hl=en)
- [About consent mode modeling](https://support.google.com/google-ads/answer/10548233?hl=en&ref_topic=3119145)
- [Google Ads integration with the IAB Transparency & Consent Framework (TCF)](https://support.google.com/google-ads/answer/10021549)
- [Adjust tag behavior based on consent](https://developers.google.com/gtagjs/devguide/consent)
- [Consent configuration](https://support.google.com/tagmanager/answer/10718549)
- [How Google uses consent mode data](https://support.google.com/google-ads/answer/10031513)
- [Blog announcement](https://blog.google/products/marketingplatform/360/measure-conversions-while-respecting-user-consent-choices/)
