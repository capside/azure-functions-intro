# Demo

## Cognitive services

* Agregar -> Cognitve services
* Account name=fotoscs, API type=Computer Vision API, Pricing=F0, Resource group: fotos, Account creation=enabled (dos veces!)
* Copiar Key: e51619ce39fa4428a40d41fcd188fa7d

### Test

* [Consola](https://westeurope.dev.cognitive.microsoft.com/docs/services/56f91f2d778daf23d8ec6739)
* [Gato](http://www.readersdigest.ca/wp-content/uploads/2011/01/4-ways-cheer-up-depressed-cat.jpg)

## Function 

* Agregar -> Function App
* AppName=fotosapp, Resource Group=fotos, Hosting Plan=Consumption, Location=West Europe, Storage=fotosstorage
* Abrir editor function

## Nueva function

* fotosapp -> functions -> [+] 
* custom -> BlobTrigger-Javascript
* Name=clasificador, Path=samples-workitems/{name}, Storage->New->fotosstorage
* (La variable con el contenedor será fotosstorage_STORAGE)

```
var util = require('util'), 
    http = require('http'),
    url = require('url');

module.exports = function (context, myBlob) {
    var cognitiveUrl = url.parse('https://westeurope.api.cognitive.microsoft.com/vision/v1.0/analyze?visualFeatures=Categories&language=en');
    var options = {
        host: cognitiveUrl.host,
        path: cognitiveUrl.path,
        port: cognitiveUrl.port ? cognitiveUrl.port : 80,
        method: 'POST',
        headers: {
            'Content-Type' : 'application/json',
            'Ocp-Apim-Subscription-Key' : 'e51619ce39fa4428a40d41fcd188fa7d'
        }
    };

    callback = function(response) {
        var str = ''
        response.on('data', function (chunk) {
            str += chunk;
        });
        req.on('error', function(e) {
            context.log('problem with request: ' + e.message);
            context.done();
        });
        response.on('end', function () {
            context.log(str);
            context.done();
        });
    }

    var req = http.request(options, callback);
    req.write(JSON.stringify({"url":context.bindingData.uri}));
    req.end();
        
};
```

# Subir foto

* All resources -> fotosstorage -> containers -> [+Container]
* Name=samples-workitems, Access-type: Blob
* [Upload] -> Files=(C:\azureboot\cat6.jpg)
* Function->Clasificador-> Logs





