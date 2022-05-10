
# PAF Publisher Integration Guide

# Tech Stack Requirements and Expectations

In order to integrate PAF, a Website needs:

1.  Prebid.js
    
2.  At least one participating SSP or DSP (e.g Criteo CDB, or another SSP/DSP interested)
    
3.  A CMP provider, or a “homemade” CMP, which will need to be significantly changed to support PAF
    
4.  Google Publisher Tag
    

# Remodeling the Consent UI

## Roles


| **Role<br>**    	| **Description<br>**                                                                                                                                                                                                                                       	|
|-----------------	|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------	|
| Website Owner   	| Self explanatory                                                                                                                                                                                                                                          	|
| CMP             	| Consent Management Platform<br>Provides the UX to let users express their data processing preferences<br>Example partners: Sourcepoint, OneTrust, Quantcast …                                                                                             	|
| PAF Client Node 	| Provides key PAF functions for the Website Owners:<br>Key Management and Identity Endpoint<br>Sign communications with the PAF Operator (acts as an operator proxy)<br>Generate Seeds to initiate PAF Transactions<br>Collate the Audit Log for each Ads  	|
| PAF Operator    	| Manages the storage and synchronization of ids and Preferences across the PAF network.    

The Website Owner and PAF Operator roles need to be filled by separate entities.

The CMP and the PAF Client Node roles can be taken by separate entities, or by either the Website Owner or by the partner providing the PAF Operator.

## Available Partners

### CMP Partners

No CMP currently supports PAF, we’ll have to get yours onboard.

### PAF Client Node Partners

| **Vendor<br>** 	| **IP or Domain Name<br>**                                	| **Comments<br>**                                                                             	|
|----------------	|----------------------------------------------------------	|----------------------------------------------------------------------------------------------	|
| Criteo         	| Sub  domain of the client, paf.website.com for instance. 	| We will need to sync with the client so that they accept the client node domain in their DNS 	|

Alternatives:

-   Take that up yourself
    
-   Ask your CMP partner to provide this service
    
-   Ask your PAF Operator to provide this service
    

### PAF Operator Partners


| **Vendor<br>** 	| **Domain<br>**      	| **Comments<br>** 	|
|----------------	|---------------------	|------------------	|
| Criteo         	| crto.onekey.network 	|                  	|
| IPONWEB        	| TBD                 	| Coming in Q2     	|


## Incorporating the Model Terms

-   The Model Terms must be incorporated in the Main Agreement between the Website Owner and the PAF Operator.
    
-   If the CMP is provided by a 3rd party partner, the Main Agreement between the Website and the CMP partner must also include the Model Terms.
    
-   Same applies if the PAF Client Node is provided to the Website by a 3rd party partner.
    

## Website Owner Tasks

### Build the PAF components from sources (if needed)

     mkdir paf-from-source
     cd paf-from-source
     git clone https://github.com/prebid/paf-mvp-implementation.git
     cd paf-mvp-implementation
     npm i
     npm run build-front
     ls paf-mvp-demo-express/public/assets/paf-lib.js

### Setup the PAF Client Node

#### Create DNS records for the Client Node back-end

Pick a subdomain name for each website’s registrable domain (e.g. `www.example-website.com` => `paf.example-website.com`).

If the Client Node partner gave you IP addresses, then create A/AAAA DNS records:

    paf.example-website.com   A       12.34.56.78
    paf.example-website.com   AAAA    4001:41d0:2:80c4::

If the Client Node partner gave you a domain name, then create an ALIAS DNS record

    paf.example-website.com   ALIAS   pafoperatorclient.vendor.com

#### Configure the Client Node backend

-   See below
    

#### Add the Client Node front-end to your website

Get the front-end Javascript from [https://github.com/prebid/paf-mvp-implementation/tree/main/paf-mvp-frontend](https://github.com/prebid/paf-mvp-implementation/tree/main/paf-mvp-frontend "https://github.com/prebid/paf-mvp-implementation/tree/main/paf-mvp-frontend").

Add it in the <head> section:

    <head>
    <script
      src="https://my-cdn.domain/assets/paf-lib.js"
    ></script>
    </head>

### Setup the CMP

Add the CMP frontend JavaScript in the head section of your website pages

1.  Make sure there is an agreed space for the CMP’s audit log viewer button next to ad zones
    
2.  Make sure there is an agreed space for the CMP’s button to let the user access the preferences edit UX
    
3.  Provide the hostname of the Client Node backend into the CMP configuration.
    

More details on the CMP setup can be found here [CMP-integration-guide](./Integration-guides/CMP-remodeling-guide.md)

## PAF Client Node Tasks

For a new website to become a PAF participant, a **PAF client node** must be running.

The PAF Client Node front end will be used by the prebid modules and the callback to get the id and preferences, generate seeds, and collate the transmissions into audit logs for each ad.

The PAF client node is currently hosted by Criteo, but could be hosted by someone else.

-   _[Ask Client node provider to]_ Add a new instance for the publisher website
    
    -   Configure **the server** to accept connections on the vhost of the subdomain the Website Owner allocated (e.g. `paf.example-website.com`)
        
        -   ⚠️ the TLD+1 must be the same as the Website (ex: `www.example-website.com` and `some-sub-site.example-website.com` must use a PAF with domain `.example-website.com`)
            


# Configuring the Ad Supply Chain

## Roles


| **Role<br>**       	|                                                                                                                                                                                                                                                           	|
|--------------------	|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------	|
| Website Owner      	| Self explanatory                                                                                                                                                                                                                                          	|
| PAF Client Node    	| Provides key PAF functions for the Website Owners:<br>Key Management and Identity Endpoint<br>Sign communications with the PAF Operator (acts as an operator proxy)<br>Generate Seeds to initiate PAF Transactions<br>Collate the Audit Log for each Ads  	|
| Direct SSPs & DSPs 	| Provide marketing content on request from the Website in compliance with Model Terms.                                                                                                                                                                     	|

Direct SSPs & DSPs

Provide marketing content on request from the Website in compliance with Model Terms.

## Incorporating the Model Terms

-   The Model Terms must be incorporated:
    
    -   in the Main Agreements between the Website Owner and SSP and DSP the website is directly connected with.
        
    -   in the Main Agreements between the SSPs and DSPs that are directly connected
        

## Website Owner Tasks

### Build a Modified Prebid.js

Use the temporary fork of prebid.js which provides the PAF id module and the PAF RTD module: [https://github.com/openx/Prebid.js](https://github.com/openx/Prebid.js)

Build [prebid.j](http://prebid.je/ "http://prebid.je")s with the 2 additional PAF modules

    git clone https://github.com/openx/Prebid.js.git
     cd Prebid.js
     npm ci
     gulp build --modules=userId,pafIdSystem,rtdModule,pafRtdProvider,appnexusBidAdapter

Don’t forget to add the adapters that you need as modules in the build command.

### Use and Configure the Modified Prebid.js

1.  Configure the 2 modules
    
    1.  PAF id module: allow the bidder adapters of the PAF-ready direct SSPs and DSPs to access the id
        
    2.  PAF RTB module: whitelist the same bidder adapters so they get access to the PAF seeds
        

The prebid Js can be configured as follow (from this file [https://github.com/prebid/paf-mvp-implementation/blob/main/paf-mvp-demo-express/src/views/publisher/index.hbs#L82](https://github.com/prebid/paf-mvp-implementation/blob/main/paf-mvp-demo-express/src/views/publisher/index.hbs#L82) ) :  
  

    var pbjs = pbjs || {};
    pbjs.que = pbjs.que || [];

    pbjs.que.push(function() {
        pbjs.addAdUnits(adUnits);
        pbjs.setConfig({
            debug: true,
            realTimeData: {
                auctionDelay: 1000,
                dataProviders: [
                        {
                        name: "paf",
                        waitForIt: true,
                        params: {proxyHostName: "cmp.pafdemopublisher.com"}
                    }
                ]
            },
            userSync: {
                userIds: [{
                    name: "pafData",
                    params: {}
                }],
                auctionDelay: 1000,
                syncDelay: 3000
            }
        });
        pbjs.requestBids({
            bidsBackHandler: initAdserver,
            timeout: PREBID_TIMEOUT
        });
    });

    function initAdserver() {
        if (pbjs.initAdserverSet) return;
        pbjs.initAdserverSet = true;
        googletag.cmd.push(function() {
            pbjs.que.push(function() {
                pbjs.setTargetingForGPTAsync();
                googletag.pubads().refresh();
            });
        });
    }
    // in case PBJS doesn't load
    setTimeout(function() {
        initAdserver();
    }, FAILSAFE_TIMEOUT);

Warning, `params: {proxyHostName: "cmp.pafdemopublisher.com"}` should lead to the Client Node

To be developed: On May 2nd 2022 the method to whitelist PAF-members bid adapter is not available yet. It means non member could get and use PAF ids without pledging to follow its principle.

### Add a Prebid.js “bidWon” Callback to Retrieve PAF Transmissions

The callback retrieves the PAF transmissions from the winning bid responses and makes the audit log available for the CMP’s Audit Log Viewer

To be confirmed: On May 2nd 2022 the method to make the audit log available is not fully finalized

    window.pbjs.onEvent("bidWon", (bid) =>
      if (bid.meta && bid.meta.paf) {
        var pafObj = bid.meta.paf;
        PAF.registerTransmissionResponse({
            prebidTransactionId: bid.transactionId,
            adUnitCode: bid.adUnitCode
            contentId: pafObj.content_id
        },
        pafObj.transmission);
      }
    ));

Make sure prebid.js is wired with Google Publisher Tag in a way that ensures that Prebid’s AdUnitCodes matches Google Publisher Tag DivIds as shown in the screenshot below

![adunit_to_div_id](https://github.com/prebid/addressability-framework/blob/main/mvp-spec/assets/adunit_to_divid.png)

### Set up the client node:

-   Prepare some **mandatory attributes** such as:
    
    -   Name
        
    -   DPO email address
        
    -   Privacy page URL
        
-   Generate and configure the pair of **cryptographic keys** for the website
    
    -   This can be done with openssl binaries:
        
        -   private key: `openssl ecparam -name prime256v1 -genkey -noout -out private-key.pem`
            
        -   public key: `openssl ec -in private-key.pem -pubout -out public-key.pem`
            
    -   Define the **end date** of the current private / public keys pair,
        
-   ⚠️ the TLD+1 must be the same as the Website (ex: `www.example-website.com` and `some-sub-site.example-website.com` must use a PAF with domain `.example-website.com`), so you need to add the client node to your DNS
    

