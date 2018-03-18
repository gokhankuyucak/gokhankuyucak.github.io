---
title: "Postman: Writing Test Scripts with XML"
excerpt: "Postman supports only JSON syntax when running test scripts. Writing test script is a little bit more complicated for a XML service than for a JSON service."
last_modified_at: 2018-03-17T11:45:06-05:00

tags: 
  - Postman
  - XML
  - SOAP
  - JSON
  - Remove Namespace Prefix
toc: false
---

I prefer to use [Postman](https://www.getpostman.com/) to test SOAP services rather than [SOAP UI](https://www.soapui.org/). Postman has a nice interface and easy to use. But writing test scripts for XML data is more complicated than for JSON data. Because Postman supports only JSON syntax when running test scripts. To handle XML test scripts, you need to convert XML data to JSON. 

One of the conversion method is using xml2Json:

```javascript
var jsonObject = xml2Json(responseBody);
console.log(jsonObject);
```

the json log result is:

```json
{
  "soap:Envelope": {
    "$": {
      "xmlns:soap": "http://schemas.xmlsoap.org/soap/envelope/"
    },
    "soap:Body": {
      "con:activate": {
        "$": {
          "xmlns:con": "http://master/body"
        },
        "con:CampaignID": "1905"
      }
    },
    "soap:Header": {
      "ave:Header": {
        "$": {
          "xmlns:ave": "http://master/header"
        },
        "ave:Tid": "1203057239823H892"
      }
    }
  }
}
```

To access the CampaignID:
```javascript
jsonObject["soap:Envelope"]["soap:Body"]["con:activate"]["con:CampaignID"]
```


To discard namespace prefixes you need to use xml2js library:

```javascript
var stripNS = require('xml2js').processors.stripPrefix;

var parseString = require('xml2js').parseString;

parseString(responseBody, {
    tagNameProcessors: [stripNS]
}, function (err, result) {
    console.log(result);
});
```
and you get 

```json
{
    "Envelope": {
        "$": {
            "xmlns:soap": "http://schemas.xmlsoap.org/soap/envelope/"
        },
        "Body": [{
            "activate": [{
                "$": {
                    "xmlns:con": "http://master/body"
                },
                "CampaignID": [
                    "1905"
                ]
            }]
        }],
        "Header": [{
            "Header": [{
                "$": {
                    "xmlns:ave": "http://master/header"
                },
                "Tid": [
                    "1203057239823H892"
                ]
            }]
        }]
    }
}
```
To access the CampaignID:
```javascript
jsonObject.Envelope.Body["0"].activate["0"].CampaignID
```

In the first example you get the JSON data with namespace prefixes, but in the second example, the prefixes are removed. On the other hand in second example some of the objects are created as an array.

To learn the exact path of the object in JSON, you can use developer tools in Postman. Click View > Developer > Show Dev Tools (Ctrl + Shift + I) Run your test again. In the developer tools, expand the JSON object and right click to target object and select "Copy Property Path". 
This will copy the exact path of the JSON object. 

eg. 
```javascript
.Envelope.Body["0"].activate["0"].CampaignID["0"]
```
<figure><img src="/assets/images/postman-json-exact-path.png">
    <figcaption>Selecting exact path of a JSON object</figcaption>
</figure>
