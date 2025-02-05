---
title: Import API using swagger
parent: Adding API's
has_children: false
nav_order: 2
---



#### Import API using swagger

Instead of importing operations one by one, you can also import a full API. The [OpenAPI specification](https://www.openapis.org/) (aka [Swagger](https://swagger.io)) is a definition format to describe RESTful APIs. The specification creates a RESTful interface for easily developing and consuming an API by effectively mapping all the resources and operations associated with it.

As a demo we will use an API that offers a simple calculator service : [Calc API](http://calcapi.cloudapp.net/)

![](../../assets/images/APIMCalcAPI.png)

Go to APIs blade and select [Add OpenAPI Specification]
- Specify swagger URL <http://calcapi.cloudapp.net/calcapi.json>
  - Some of the fields will be populated from the swagger definition
  - Use "calc" in URL for API suffix
  - Add products Starter and Unlimited

![](../../assets/images/APIMAddCalcAPI1.png)

![](../../assets/images/APIMAddCalcAPI2.png)

- Look at Calculator API in the Developer portal
  - Try the Add Two Integers operation
- Look at Response / Trace

![](../../assets/images/APIMCalcTryIt1.png)

![](../../assets/images/APIMCalcTryIt2.png)

We can inspect / edit the Open API definition by selecting the `Edit` icon from the Frontend block:

![](../../assets/images/APIMCalcSwagger.png)

![](../../assets/images/APIMCalcSwagger2.png)

Let's use another example - the [Colors API](https://markcolorapi.azurewebsites.net/swagger/)

![](../../assets/images/APIMColorAPI.png)

- Create a new API with OpenAPI specification and import swagger from > <https://markcolorapi.azurewebsites.net/swagger/v1/swagger.json>
  - Use "color" in URL for API

![](../../assets/images/APIMAddColorAPI1.png)

![](../../assets/images/APIMAddColorAPI2.png)


The swagger file did not contain the name of the host so you need to update it manually

- Select [Settings]
- Amend Web Service URL to <https://markcolorapi.azurewebsites.net>

![](../../assets/images/APIMAddColorAPI3.png)

We can test this from the [Test] tab

![](../../assets/images/APIMAddColorAPI.png)

Switch to the Developer portal and look at Color API
  - Try the RandomColor operation
- Look at Response / Trace ... see the random color returned

![](../../assets/images/APIMColorTryIt1.png)

![](../../assets/images/APIMColorTryIt2.png)

#### Rate limit

Use [Colors website](https://markcolorweb.azurewebsites.net) which displays 500 lights.  Each light will at random intervals make a call to the RandomColor API - and then display the color returned.

![](../../assets/images/APIMColorWeb.png)

First we will need to enable CORS for the domain name of the frontend. To achieve this we have to do the following:

- On the left Menu, click on `APIs`
- Then select the `All APIs` option
- Then go to the `Inbound Processing` area
- There you will find a `cors` policy(this was added in part 2, when we used the "Enable Cors" button)
- Click on the `edit` icon

![](../../assets/images/apim-policy-cors-all-apis.png)  

Here we will se this form, where we can add the domain name of our frontend `https://markcolorweb.azurewebsites.net` or the `*` for all domains:

![](../../assets/images/apim-policy-cors-all-apis2.png)  



Via the menu - there is a configuration page to specify the API endpoint

![](../../assets/images/APIMColorWebConfig.png)

Open Developer portal, go in the Profile page and get API keys for Starter and Unlimited products
- Open Notepad - make note of URLs
  - <https://YOURAPIM.azure-api.net/color/api/RandomColor?key=> *Starter-Key*
  - <https://YOURAPIM.azure-api.net/color/api/RandomColor?key=> *Unlimited-Key*

![](../../assets/images/APIMColorWebKeys.png)

- To see that Unlimited product has no rate limits
  - Configure Color Website to use Unlimited URL
  - Select [Start]
  - Notice there is no Rate Limit - every light is randomly updated

![](../../assets/images/APIMColorWebUnlimited.png)

- To see that Starter product is limited to 5 calls per minute
  - Configure Color Website to use Starter URL
  - Select [Start]
  - Notice that only 5 lights get colored
- Try URL with the Starter key, directly in the web browser address bar
  - Notice the error status / message returned
  - Example: *{ "statusCode": 429, "message": "Rate limit is exceeded. Try again in 54 seconds." }*

![](../../assets/images/APIMColorWebStarter.png)

