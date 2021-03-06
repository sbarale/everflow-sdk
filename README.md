## Usage

Get the lastest minified version of the SDK via the release section.

Configure the SDK before doing any tracking.

```javascript
EF.configure({
    // You only need to set the tracking domain you want to use
    tracking_domain: 'https://<tracking-domain>.com',
})
```

### Click tracking

```javascript
EF.click({
    offer_id: 1, // Required. The offer id
    affiliate_id: 1, //Required. The affiliate id

    transaction_id: '', // Optional. The transaction id, if a click was already generated. Used to enhance regular tracking and store a first party cookie.

    disable_fingerprinting: false, // Optional. Wheter or not to disable browser fingerprinting used enhance user tracking. Defaults to false

    // Optional. Sub placement values.
    sub1: '',
    sub2: '',
    sub3: '',
    sub4: '',
    sub5: '',
    source_id: '',
});
```

#### Direct Linking

When working with direct linking, your affiliate id will most likely be dynamic. In order to dynamically set the affiliate id, use the following method to extract any URL value. In the following scenario, the script executes on a landing page with the `affid` url parameter (https://destination-url.com?affid=5)

```javascript
EF.click({
    offer_id: EF.urlParameter('oid'), 
    affiliate_id: EF.urlParameter('affid'), 
});
```

#### ITP workaround and first-party cookie tracking

You can use the Everflow SDK to enhance traditional tracking when you own landing pages. In order to do so, fill the `transaction_id` parameter when using `EF.click`. Normally, you would configure your destination url to include the `transaction_id`, `offer_id` and `affiliate_id` macros. Given the following destination url: https://destination-url.com?transaction_id=af189e77650e4e908af797b61b03ac0b&oid=1&affid=5

```javascript
EF.click({
    offer_id: EF.urlParameter('oid'), 
    affiliate_id: EF.urlParameter('affid'),
    transaction_id: EF.urlParameter('transaction_id')
});
```

### Conversion Tracking

 ```javascript
   EF.conversion({
        offer_id: 1, // Required. The offer id
        transaction_id: '', // Optional. The transaction id. If you have access to it, otherwise the SDK tries to locate one for the current user.

        amount: 0, // Optional. Amount in case of RPS offer

        event_id: 0, // Optional. The event id
        coupon_code: '', // Optional. Coupon code

        adv_event_id: 0, // Optional. If you use global advertiser events

        // Optional. Adv placement values.
        adv1: '',
        adv2: '',
        adv3: '',
        adv4: '',
        adv5: '',

        //Optional. Free form parameters to me appended on the conversion URL
        parameters: {
            
        }
   })
 ```
 
 ### Accessing the transaction ids
 
 You can access the latest transaction id that was generated for an offer using the following method:
 
 ```javascript
 EF.getTransactionId(1); // 1 is the offer id you want to query for.
 ```
 
 You can also access the transaction id from an `EF.click` call. The method returns a promise which you can resolve:
 
 ```javascript
 EF.click({
    offer_id: EF.urlParameter('oid'), 
    affiliate_id: EF.urlParameter('affid'),
 })
 .then((transactionId) => console.log(transactionId))
 ```

### Accessing the conversion ids

You can access the conversion id that was generated for any conversion by resolving the Promise that is returned:

```javascript
EF.conversion({
    offer_id: 1,
})
.then((conversion) => {
    console.log(conversion.conversion_id);
    console.log(conversion.transaction_id)
})
```