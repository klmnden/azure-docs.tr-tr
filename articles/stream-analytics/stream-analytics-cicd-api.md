---
title: REST API'lerini kullanarak IOT Edge üzerinde Azure Stream Analytics için CI/CD uygulayabileceğinizi
description: Sürekli tümleştirme ve dağıtım işlem hattı REST API'lerini kullanarak Azure Stream Analytics için uygulamayı öğrenin.
services: stream-analytics
author: mamccrea
ms.author: mamccrea
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 12/04/2018
ms.openlocfilehash: 40beb620e037061b189762a51e3c29d0fd251b27
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61362085"
---
# <a name="implement-cicd-for-stream-analytics-on-iot-edge-using-apis"></a>API'ler kullanarak IOT Edge üzerinde Stream Analytics için CI/CD uygulayabileceğinizi

Sürekli tümleştirme ve dağıtım REST API'lerini kullanarak Azure Stream Analytics işleri için etkinleştirebilirsiniz. Bu makalede hangi API'leri ve bunları nasıl kullanacağınızı örnekler sağlar. REST API'leri, Azure Cloud Shell üzerinde desteklenmez.

## <a name="call-apis-from-different-environments"></a>Farklı ortamları API'lerini çağırma

REST API'leri, hem Linux hem de Windows çağrılabilir. Aşağıdaki komutları API çağrısı için doğru sözdizimi gösterilmektedir. Özel API kullanımı bu makalenin sonraki bölümlerde özetlenen.

### <a name="linux"></a>Linux

Linux için kullanabileceğiniz `Curl` veya `Wget` komutları:

```bash
curl -u { <username:password> }  -H "Content-Type: application/json" -X { <method> } -d "{ <request body>}” { <url> }   
```

```bash
wget -q -O- --{ <method> }-data="<request body>”--header=Content-Type:application/json --auth-no-challenge --http-user="<Admin>" --http-password="<password>" <url>
```
 
### <a name="windows"></a>Windows

Windows için Powershell kullanın: 

```powershell 
$user = "<username>" 
$pass = "<password>" 
$encodedCreds = [Convert]::ToBase64String([Text.Encoding]::ASCII.GetBytes(("{0}:{1}" -f $user,$pass))) 
$basicAuthValue = "Basic $encodedCreds" 
$headers = New-Object "System.Collections.Generic.Dictionary[[String],[String]]" 
$headers.Add("Content-Type", 'application/json') 
$headers.Add("Authorization", $basicAuthValue) 
$content = "<request body>" 
$response = Invoke-RestMethod <url>-Method <method> -Body $content -Headers $Headers 
echo $response 
```
 
## <a name="create-an-asa-job-on-edge"></a>Edge üzerinde ASA işi oluşturma 
 
Stream Analytics işi oluşturmak için Stream Analytics API'si kullanarak PUT yöntemini çağırın.

|Yöntem|İstek URL'si|
|------|-----------|
|PUT|https://management.azure.com/subscriptions/{**Abonelik kimliği**} /resourcegroups/ {**kaynak grubu adı**} / providers/Microsoft.StreamAnalytics/streamingjobs/ {**iş adı**}? api sürümü = 2017-04-01-Önizleme|
 
Komutunu kullanarak örnek **curl**:

```curl
curl -u { <username:password> }  -H "Content-Type: application/json" -X { <method> } -d "{ <request body>}” https://management.azure.com/subscriptions/{subscription-id}/resourcegroups/{resource-group-name}/providers/Microsoft.StreamAnalytics/streamingjobs/{jobname}?api-version=2017-04-01-preview  
``` 
 
İstek gövdesinde JSON örneği:

```json
{ 
  "location": "West US", 
  "tags": { "key": "value", "ms-suppressjobstatusmetrics": "true" }, 
  "sku": {  
      "name": "Standard" 
    }, 
  "properties": { 
    "sku": {  
      "name": "standard" 
    }, 
       "eventsLateArrivalMaxDelayInSeconds": 1, 
       "jobType": "edge", 
    "inputs": [ 
      { 
        "name": "{inputname}", 
        "properties": { 
         "type": "stream", 
          "serialization": { 
            "type": "JSON", 
            "properties": { 
              "fieldDelimiter": ",", 
              "encoding": "UTF8" 
            } 
          }, 
          "datasource": { 
            "type": "GatewayMessageBus", 
            "properties": { 
            } 
          } 
        } 
      } 
    ], 
    "transformation": { 
      "name": "{queryName}", 
      "properties": { 
        "query": "{query}" 
      } 
    }, 
    "package": { 
      "storageAccount" : { 
        "accountName": "{blobstorageaccountname}", 
        "accountKey": "{blobstorageaccountkey}" 
      }, 
      "container": "{blobcontaine}]" 
    }, 
    "outputs": [ 
      { 
        "name": "{outputname}", 
        "properties": { 
          "serialization": { 
            "type": "JSON", 
            "properties": { 
              "fieldDelimiter": ",", 
              "encoding": "UTF8" 
            } 
          }, 
          "datasource": { 
            "type": "GatewayMessageBus", 
            "properties": { 
            } 
          } 
        } 
      } 
    ] 
  } 
} 
```
 
Daha fazla bilgi için [API belgeleri](/rest/api/streamanalytics/stream-analytics-job).  
 
## <a name="publish-edge-package"></a>Edge Paketi Yayımlama 
 
Bir IOT Edge üzerinde Stream Analytics işi yayımlamak için Edge Paketi Yayımlama API'sini kullanarak POST yöntemini çağırın.

|Yöntem|İstek URL'si|
|------|-----------|
|POST|https://management.azure.com/subscriptions/{**subscriptionıd**} /resourceGroups/ {**resourcegroupname**} / providers/Microsoft.StreamAnalytics/streamingjobs/ {**jobname**} / publishedgepackage? api sürümü = 2017-04-01 - Önizleme|

İş başarıyla yayımlandı kadar bu zaman uyumsuz işlem 202 durumunu döndürür. İşlemin durumunu almak için kullanılan URI konumu yanıt üst bilgisi içerir. İşlem devam ederken, location üst bilgisini URI'de bir çağrı 202 durumunu döndürür. İşlem tamamlandığında, location üst bilgisini URI'de 200 durumunu döndürür. 

Örnek Edge paketi yayımlama kullanarak **curl**: 

```bash
curl -d -X POST https://management.azure.com/subscriptions/{subscriptionid}/resourceGroups/{resourcegroupname}/providers/Microsoft.StreamAnalytics/streamingjobs/{jobname}/publishedgepackage?api-version=2017-04-01-preview
```
 
POST çağrısına yaptıktan sonra boş gövdesi ile bir yanıt beklemelisiniz. Yanıtın baş içinde bulunan URL arayın ve başka amaçlarla kullanmak için kaydedebilirsiniz.
 
Yanıt DOSYASININ örnekten URL'si:

```
https://management.azure.com/subscriptions/{**subscriptionid**}/resourcegroups/{**resourcegroupname**}/providers/Microsoft.StreamAnalytics/StreamingJobs/{**resourcename**}/OperationResults/023a4d68-ffaf-4e16-8414-cb6f2e14fe23?api-version=2017-04-01-preview 
```
URL ile API çağrısı yapmak için aşağıdaki komutu çalıştırmadan önce bir ila iki dakika bekleyip yanıtın baş içinde bulunamadı. 200 yanıt alamazsanız komutu yeniden deneyin.
 
API çağrısı ile yapma örnek URL'si ile döndürülen **curl**:

```bash
curl -d –X GET https://management.azure.com/subscriptions/{subscriptionid}/resourceGroups/{resourcegroupname}/providers/Microsoft.StreamAnalytics/streamingjobs/{resourcename}/publishedgepackage?api-version=2017-04-01-preview 
```

Yanıt, Edge dağıtımı komut dosyasına eklemek için gereken bilgileri içerir. Aşağıdaki örnekler, ne toplamak için gereken bilgileri ve nereye dağıtım eklemek bildirim gösterir.
 
Örnek yanıt gövdesi başarıyla yayımladıktan sonra:

```json
{ 
  edgePackageUrl : null 
  error : null 
  manifest : "{"supportedPlatforms":[{"os":"linux","arch":"amd64","features":[]},{"os":"linux","arch":"arm","features":[]},{"os":"windows","arch":"amd64","features":[]}],"schemaVersion":"2","name":"{jobname}","version":"1.0.0.0","type":"docker","settings":{"image":"{imageurl}","createOptions":null},"endpoints":{"inputs":["],"outputs":["{outputnames}"]},"twin":{"contentType":"assignments","content":{"properties.desired":{"ASAJobInfo":"{asajobsasurl}","ASAJobResourceId":"{asajobresourceid}","ASAJobEtag":"{etag}",”PublishTimeStamp”:”{publishtimestamp}”}}}}" 
  status : "Succeeded" 
} 
```

Örnek dağıtım bildiriminin: 

```json
{ 
  "modulesContent": { 
    "$edgeAgent": { 
      "properties.desired": { 
        "schemaVersion": "1.0", 
        "runtime": { 
          "type": "docker", 
          "settings": { 
            "minDockerVersion": "v1.25", 
            "loggingOptions": "", 
            "registryCredentials": {} 
          } 
        }, 
        "systemModules": { 
          "edgeAgent": { 
            "type": "docker", 
            "settings": { 
              "image": "mcr.microsoft.com/azureiotedge-agent:1.0", 
              "createOptions": "{}" 
            } 
          }, 
          "edgeHub": { 
            "type": "docker", 
            "status": "running", 
            "restartPolicy": "always", 
            "settings": { 
              "image": "mcr.microsoft.com/azureiotedge-hub:1.0", 
              "createOptions": "{}" 
            } 
          } 
        }, 
        "modules": { 
          "<asajobname>": { 
            "version": "1.0", 
            "type": "docker", 
            "status": "running", 
            "restartPolicy": "always", 
            "settings": { 
              "image": "<settings.image>", 
              "createOptions": "<settings.createOptions>" 
            } 
            "version": "<version>", 
             "env": { 
              "PlanId": { 
               "value": "stream-analytics-on-iot-edge" 
          } 
        } 
      } 
    }, 
    "$edgeHub": { 
      "properties.desired": { 
        "schemaVersion": "1.0", 
        "routes": { 
            "route": "FROM /* INTO $upstream" 
        }, 
        "storeAndForwardConfiguration": { 
          "timeToLiveSecs": 7200 
        } 
      } 
    }, 
    "<asajobname>": { 
      "properties.desired": {<twin.content.properties.desired>} 
    } 
  } 
} 
```

Dağıtım bildirimi yapılandırmadan sonra başvurmak [Azure CLI ile dağıtma Azure IOT Edge modülleri](../iot-edge/how-to-deploy-modules-cli.md) dağıtımı için.


## <a name="next-steps"></a>Sonraki adımlar 
 
* [IOT Edge üzerinde Azure Stream Analytics](stream-analytics-edge.md)
* [Öğretici IOT Edge üzerinde ASA](https://docs.microsoft.com/azure/iot-edge/tutorial-deploy-stream-analytics)
* [Visual Studio Araçları'nı kullanarak Stream Analytics Edge işlerini geliştirme](stream-analytics-tools-for-visual-studio-edge-jobs.md)
