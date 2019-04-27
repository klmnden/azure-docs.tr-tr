---
title: Ağustos 1 2015 Önizleme - Azure Logic Apps için şema güncelleştirmeleri | Microsoft Docs
description: Güncelleştirilmiş şema sürümü 2015-08-01-Önizleme için Azure Logic apps'te mantıksal uygulama tanımları
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: kevinlam1
ms.author: klam
ms.reviewer: estfan, LADocs
ms.assetid: 0d03a4d4-e8a8-4c81-aed5-bfd2a28c7f0c
ms.topic: article
ms.date: 05/31/2016
ms.openlocfilehash: 92f522c72f69218e55b1ee4cfff74511a30288b0
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60553768"
---
# <a name="schema-updates-for-azure-logic-apps---august-1-2015-preview"></a>Azure Logic Apps - 1 Ağustos 2015 önizlemesi için şema güncelleştirmeleri

Bu şema ve API sürümü için Azure Logic Apps, logic apps, daha güvenilir ve kullanmayı daha kolay hale anahtar iyileştirmeler içerir:

* **APIApp** eylem türü artık adlı [ **APIConnection**](#api-connections).
* **Yineleyin** eylemi artık adlı [ **Foreach**](#foreach).
* [ **HTTP dinleyicisi** API uygulaması](#http-listener) artık gerekli değildir.
* Alt iş akışlarını çağırma kullanan bir [yeni şema](#child-workflows).

<a name="api-connections"></a>

## <a name="move-to-api-connections"></a>API bağlantıları Taşı

Büyük değişiklik, artık API'lerini kullanabilmesi için API uygulamaları Azure aboneliğinize dağıtmak olmasıdır. API'leri kullanabileceğiniz yollar şunlardır:

* Yönetilen API'ler
* Özel Web API'leri

Bunların yönetimi ve barındırma modellerini farklı olduğu için her iki yöntemle biraz farklı şekilde ele alınır. Artık Azure kaynak grubunuzda dağıtılan kaynaklar için kısıtlı bu modelin avantajlarından biri. 

### <a name="managed-apis"></a>Yönetilen API'ler

Microsoft Office 365, Salesforce, Twitter ve FTP gibi sizin adınıza bazı API'leri yönetir. Bazı yönetilen API'ler olarak kullanabileceğiniz-Bing çeviri gibi diğerleri ise yapılandırma gerektirir, ancak olarak da adlandırılan bir *bağlantı*.

Örneğin, Office 365'i kullandığınızda, Office 365 oturum açma belirteciniz içeren bir bağlantı oluşturmanız gerekir. Belirtecinizi güvenli bir şekilde depolanır ve mantıksal uygulamanızın her zaman Office 365 API çağırması yayımlanmadığını. SQL veya FTP sunucunuza bağlanmak istiyorsanız, bağlantı dizesi olan bir bağlantı oluşturmanız gerekir. 

Bu tanımında, bu eylemleri adlandırılır `APIConnection`. Office 365'e-posta göndermek için çağıran bağlantının bir örnek aşağıda verilmiştir:

``` json
{
   "actions": {
      "Send_an_email": {
         "type": "ApiConnection",
         "inputs": {
            "host": {
               "api": {
                  "runtimeUrl": "https://msmanaged-na.azure-apim.net/apim/office365"
               },
               "connection": {
                  "name": "@parameters('$connections')['shared_office365']['connectionId']"
               }
            },
            "method": "POST",
            "body": {
               "Subject": "Reminder",
               "Body": "Don't forget!",
               "To": "me@contoso.com"
            },
            "path": "/Mail"
         }
      }
   }
}
```

`host` Nesnedir, API bağlantıları için benzersizdir ve bölümleri içeren bir parçası girişleri: `api` ve `connection`. `api` Nesne çalışma zamanı URL'si, API yönetildiği barındırıldığını belirtir. Bu yöntemi çağırarak tüm kullanılabilir yönetilen API'ler görebilirsiniz:

```text
GET https://management.azure.com/subscriptions/<Azure-subscription-ID>/providers/Microsoft.Web/locations/<location>/managedApis?api-version=2015-08-01-preview
```

Bir API'yi kullandığınızda, bu API olabilir veya tüm tanımlamış olabileceği değil *bağlantı parametrelerini*. Bu nedenle, bu parametreleri API tanımlamıyorsa, bağlantı gereklidir. API bu parametreleri tanımlarsanız, belirtilen bir ada sahip bir bağlantı oluşturmanız gerekir.  
Ardından bu ada başvuru `connection` içine `host` nesne. Bir kaynak grubunda bir bağlantı oluşturmak için bu yöntemi çağırın:

```text
PUT https://management.azure.com/subscriptions/<Azure-subscription-ID>/resourceGroups/<Azure-resource-group-name>/providers/Microsoft.Web/connections/<name>?api-version=2015-08-01-preview
```

Aşağıdaki gövdesi:

``` json
{
   "properties": {
      "api": {
         "id": "/subscriptions/<Azure-subscription-ID>/providers/Microsoft.Web/managedApis/azureblob"
      },
      "parameterValues": {
         "accountName": "<Azure-storage-account-name-with-different-parameters-for-each-API>"
      }
   },
   "location": "<logic-app-location>"
}
```

### <a name="deploy-managed-apis-in-an-azure-resource-manager-template"></a>Bir Azure Resource Manager şablonunda yönetilen API'ler dağıtın

Etkileşimli oturum açma gerekli olmadığı durumlarda, Resource Manager şablonu kullanarak tam bir uygulama oluşturabilirsiniz.
Oturum açma gereklidir, Resource Manager şablonu kullanmaya devam edebilirsiniz, ancak Azure portalı üzerinden bağlantılarını yetkilendirmek zorunda. 

``` json
"resources": [ {
   "apiVersion": "2015-08-01-preview",
   "name": "azureblob",
   "type": "Microsoft.Web/connections",
   "location": "[resourceGroup().location]",
   "properties": {
      "api": {
         "id": "[concat(subscription().id,'/providers/Microsoft.Web/locations/westus/managedApis/azureblob')]"
      },
      "parameterValues": {
         "accountName": "[parameters('storageAccountName')]",
         "accessKey": "[parameters('storageAccountKey')]"
      }
    },
},
{
   "type": "Microsoft.Logic/workflows",
   "apiVersion": "2015-08-01-preview",
   "name": "[parameters('logicAppName')]",
   "location": "[resourceGroup().location]",
   "dependsOn": ["[resourceId('Microsoft.Web/connections', 'azureblob')]"],
   "properties": {
      "sku": {
         "name": "[parameters('sku')]",
         "plan": {
            "id": "[concat(resourceGroup().id, '/providers/Microsoft.Web/serverfarms/', parameters('svcPlanName'))]"
         }
      },
      "parameters": {
         "$connections": {
             "value": {
                  "azureblob": {
                     "connectionId": "[concat(resourceGroup().id,'/providers/Microsoft.Web/connections/azureblob')]",
                     "connectionName": "azureblob",
                     "id": "[concat(subscription().id,'/providers/Microsoft.Web/locations/westus/managedApis/azureblob')]"
                  }
             }
         }
      },
      "definition": {
         "$schema": "https://schema.management.azure.com/schemas/2016-06-01/Microsoft.Logic.json",
         "contentVersion": "1.0.0.0",
         "parameters": {
            "type": "Object",
            "$connections": {
               "defaultValue": {},
 
            }
         },
         "triggers": {
            "Recurrence": {
               "type": "Recurrence",
               "recurrence": {
                  "frequency": "Day",
                  "interval": 1
               }
            }
         },
         "actions": {
            "Create_file": {
               "type": "ApiConnection",
               "inputs": {
                  "host": {
                     "api": {
                        "runtimeUrl": "https://logic-apis-westus.azure-apim.net/apim/azureblob"
                     },
                     "connection": {
                       "name": "@parameters('$connections')['azureblob']['connectionId']"
                     }
                  },
                  "method": "POST",
                  "queries": {
                     "folderPath": "[concat('/', parameters('containerName'))]",
                     "name": "helloworld.txt"
                  },
                  "body": "@decodeDataUri('data:, Hello+world!')",
                  "path": "/datasets/default/files"
               },
               "conditions": []
            }
         },
         "outputs": {}
      }
   }
} ]
```

Bu örnekte, bağlantılar, kaynak grubunuzda Canlı yalnızca kaynakların olduğunu görebilirsiniz. Aboneliğinizde kullanılabilen yönetilen API'ler başvuruyor.

### <a name="your-custom-web-apis"></a>Özel Web API'leri

Yerleşik olanları Microsoft tarafından yönetilen yerine kendi Apı'lerinize kullanırsanız kullanın **HTTP** Apı'lerinizi çağrılacak eylem. İdeal olarak, API'niz için bir Swagger uç noktası sağlamanız gerekir. Bu uç nokta, Logic Apps Tasarımcısı'nın, API'NİZİN giriş ve çıkışlarını görüntülemek yardımcı olur. Swagger uç nokta tasarımcıya yalnızca girişler ve çıkışlar donuk bir JSON nesnesi gösterebilirsiniz.

İşte yeni gösteren bir örnek `metadata.apiDefinitionUrl` özelliği:

``` json
"actions": {
   "mycustomAPI": {
      "type": "Http",
      "metadata": {
         "apiDefinitionUrl": "https://mysite.azurewebsites.net/api/apidef/"  
      },
      "inputs": {
         "uri": "https://mysite.azurewebsites.net/api/getsomedata",
         "method": "GET"
      }
   }
}
```

Azure App Service'te Web API'nizi barındırıyorsanız, Web API'niz Tasarımcısı'nda kullanılabilir eylemler listesinden otomatik olarak görünür. Aksi takdirde, URL'yi doğrudan yapıştırmak zorunda. Swagger'ı destekleyen tüm yöntemleri ile API kendisini güvenliğini sağlayabilirsiniz ancak Logic Apps Tasarımcısı'nda kullanılabilir olması için Swagger uç noktası kimliği doğrulanmamış gerekir.

### <a name="call-deployed-api-apps-with-2015-08-01-preview"></a>API apps 2015-08-01-preview ile çağrı dağıtılan

API uygulaması, daha önce dağıttıysanız, bu uygulamayla çağırabilirsiniz **HTTP** eylem.
Örneğin, Dropbox dosyaları listelemek için kullanırsanız, **2014-12-01-preview** şema sürüm tanımına aşağıdaki gibi olabilir:

``` json
"definition": {
   "$schema": "https://schema.management.azure.com/schemas/2016-06-01/Microsoft.Logic.json",
   "contentVersion": "1.0.0.0",
   "parameters": {
      "/subscriptions/<Azure-subscription-ID>/resourcegroups/avdemo/providers/Microsoft.AppService/apiapps/dropboxconnector/token": {
         "defaultValue": "eyJ0eX...wCn90",
         "type": "String",
         "metadata": {
            "token": {
               "name": "/subscriptions/<Azure-subscription-ID>/resourcegroups/avdemo/providers/Microsoft.AppService/apiapps/dropboxconnector/token"
            }
         }
      }
    },
    "actions": {
       "dropboxconnector": {
          "type": "ApiApp",
          "inputs": {
             "apiVersion": "2015-01-14",
             "host": {
                "id": "/subscriptions/<Azure-subscription-ID>/resourcegroups/avdemo/providers/Microsoft.AppService/apiapps/dropboxconnector",
                "gateway": "https://avdemo.azurewebsites.net"
             },
             "operation": "ListFiles",
             "parameters": {
                "FolderPath": "/myfolder"
             },
             "authentication": {
                "type": "Raw",
                "scheme": "Zumo",
                "parameter": "@parameters('/subscriptions/<Azure-subscription-ID>/resourcegroups/avdemo/providers/Microsoft.AppService/apiapps/dropboxconnector/token')"
             }
          }
       }
    }
}
```

Şimdi, artık derleme benzer bir HTTP eylemi ve mantıksal uygulama tanımının bırakın `parameters` bölüm değiştirmeden, örneğin:

``` json
"actions": {
   "dropboxconnector": {
      "type": "Http",
      "metadata": {
         "apiDefinitionUrl": "https://avdemo.azurewebsites.net/api/service/apidef/dropboxconnector/?api-version=2015-01-14&format=swagger-2.0-standard"  
      },
      "inputs": {
         "uri": "https://avdemo.azurewebsites.net/api/service/invoke/dropboxconnector/ListFiles?api-version=2015-01-14",
         "method": "POST",
         "body": {
            "FolderPath": "/myfolder"
         },
         "authentication": {
            "type": "Raw",
            "scheme": "Zumo",
            "parameter": "@parameters('/subscriptions/<Azure-subscription-ID>/resourcegroups/avdemo/providers/Microsoft.AppService/apiapps/dropboxconnector/token')"
         }
      }
   }
}
```

Bu özellikler tek tek yürüyen:

| Action özelliği | Açıklama |
| --- | --- |
| `type` | `Http` Onun yerine `APIapp` |
| `metadata.apiDefinitionUrl` | Mantıksal Uygulama Tasarımcısı'nda bu eylemi kullanabilmek için nesnesinden oluşturulan meta veri uç noktası şunlardır: `{api app host.gateway}/api/service/apidef/{last segment of the api app host.id}/?api-version=2015-01-14&format=swagger-2.0-standard` |
| `inputs.uri` | Oluşturulan: `{api app host.gateway}/api/service/invoke/{last segment of the api app host.id}/{api app operation}?api-version=2015-01-14` |
| `inputs.method` | Her zaman `POST` |
| `inputs.body` | API uygulaması parametreleri aynı |
| `inputs.authentication` | API uygulaması kimlik doğrulaması ile aynı |

Bu yaklaşım, tüm API uygulaması eylemleri için çalışır. Ancak, bu önceki API Apps artık desteklenmeyen unutmayın. Bu nedenle, iki farklı önceki seçenek, yönetilen bir API veya özel Web API'niz barındırma birine taşımanız gerekir.

<a name="foreach"></a>

## <a name="renamed-repeat-to-foreach"></a>'Repeat' 'foreach' olarak yeniden adlandırıldı.

Önceki şema sürümü için çok müşteri geri bildirim aldık, **yineleyin** eylem adı kafa karıştırıcı ve düzgün şekilde yakalama yaramadı **yineleyin** gerçekten için-her bir döngü olup. Bu nedenle, biz olarak yeniden adlandırıldı `repeat` için `foreach`. Daha önce bu eylemi şu örnekteki gibi yazmalısınız:

``` json
"actions": {
   "pingBing": {
      "type": "Http",
      "repeat": "@range(0,2)",
      "inputs": {
         "method": "GET",
         "uri": "https://www.bing.com/search?q=@{repeatItem()}"
      }
   }
}
```

Artık bu sürümü yerine yazmalısınız:

``` json
"actions": {
   "pingBing": {
      "type": "Http",
      "foreach": "@range(0,2)",
      "inputs": {
         "method": "GET",
         "uri": "https://www.bing.com/search?q=@{item()}"
      }
   }
}
```

Ayrıca, `repeatItem()` döngünün geçerli yineleme sırasında işleme öğesi başvurulan, işlevi artık yeniden adlandırılan `item()`. 

### <a name="reference-outputs-from-foreach"></a>'Foreach' başvuru çıkışları

Basitleştirme, çıkışları için `foreach` Eylemler artık adlı bir nesne içinde sarmalanmış `repeatItems`. Ayrıca, bu değişiklikler ile `repeatItem()`, `repeatBody()`, ve `repeatOutputs()` işlevleri kaldırılır.

Bu nedenle, önceki'yi kullanarak `repeat` örnek, bu çıkışları alın:

``` json
"repeatItems": [ {
   "name": "pingBing",
   "inputs": {
      "uri": "https://www.bing.com/search?q=0",
      "method": "GET"
   },
   "outputs": {
      "headers": { },
      "body": "<!DOCTYPE html><html lang=\"en\" xml:lang=\"en\" xmlns=\"http://www.w3.org/1999/xhtml\" xmlns:Web=\"https://schemas.live.com/Web/\">...</html>"
   },
   "status": "Succeeded"
} ]
```

Artık bu çıkışları yerine alın:

``` json
[ {
   "name": "pingBing",
      "inputs": {
         "uri": "https://www.bing.com/search?q=0",
         "method": "GET"
      },
      "outputs": {
         "headers": { },
         "body": "<!DOCTYPE html><html lang=\"en\" xml:lang=\"en\" xmlns=\"http://www.w3.org/1999/xhtml\" xmlns:Web=\"https://schemas.live.com/Web/\">...</html>"
      },
      "status": "Succeeded"
} ]
```

Daha önce alınacak `body` bu çıkışları başvururken eylem:

``` json
"actions": {
   "secondAction": {
      "type": "Http",
      "repeat": "@outputs('pingBing').repeatItems",
      "inputs": {
         "method": "POST",
         "uri": "https://www.example.com",
         "body": "@repeatItem().outputs.body"
      }
   }
}
```

Artık bu sürümü yerine kullanabilirsiniz:

``` json
"actions": {
   "secondAction": {
      "type": "Http",
      "foreach": "@outputs('pingBing')",
      "inputs": {
         "method": "POST",
         "uri": "https://www.example.com",
         "body": "@item().outputs.body"
      }
   }
}
```

<a name="http-listener"></a>

## <a name="native-http-listener"></a>Yerel HTTP dinleyicisi

HTTP dinleyicisi özellikleri, artık bir HTTP dinleyicisi API uygulamasına dağıtmak zorunda kalmamak için yerleşiktir. Daha fazla bilgi için bilgi nasıl [mantıksal uygulama uç noktanızı çağrılabilir olun](../logic-apps/logic-apps-http-endpoint.md). 

Logic Apps bu değişikliklerle değiştirir `@accessKeys()` işleviyle `@listCallbackURL()` işlevin gerektiğinde uç noktasını alır. Ayrıca, mantıksal uygulamanız artık en az bir tetikleyici tanımlamanız gerekir. İsterseniz `/run` iş akışı tetikleyicisi bunlardan birini kullanmak zorunda: `Manual`, `ApiConnectionWebhook`, veya `HttpWebhook`

<a name="child-workflows"></a>

## <a name="call-child-workflows"></a>Alt iş akışlarını çağırma

Daha önce alt iş akışlarını çağırarak iş akışına gidip erişim belirteci alma ve o alt iş akışını çağırmak istediğiniz mantıksal uygulama tanımında belirteç yapıştırma gereklidir. Herhangi bir gizli anahtar tanımında yapıştırmak zorunda kalmamak için bu şema ile Logic Apps altyapısı bir SAS alt iş akışı için çalışma zamanında otomatik olarak oluşturur. Örnek aşağıda verilmiştir:

``` json
"myNestedWorkflow": {
   "type": "Workflow",
   "inputs": {
      "host": {
         "id": "/subscriptions/<Azure-subscription-ID>/resourceGroups/<Azure-resource-group-name>/providers/Microsoft.Logic/myWorkflow001",
         "triggerName": "myEndpointTrigger"
      },
      "queries": {
         "extrafield": "specialValue"
      },
      "headers": {
         "x-ms-date": "@utcnow()",
         "Content-type": "application/json"
      },
      "body": {
         "contentFieldOne": "value100",
         "anotherField": 10.001
      }
   },
   "conditions": []
}
```

Ayrıca, alt iş akışları, gelen istek tam erişim elde edin. Bu nedenle, parametrelere geçirebilirsiniz `queries` bölümü ve `headers` nesne. Tüm da tam olarak tanımlayabilirsiniz `body` bölümü.

Son olarak, alt iş akışları şu gerekli değişiklikler var. Daha önce verebilir ve doğrudan bir alt iş akışı arama çağırmak üst iş akışı şimdi bir tetikleyici uç noktası tanımlamanız gerekir. Genel olarak, sahip bir tetikleyici ekleme `Manual` yazın ve ardından üst tanımında tetikleyicisi. `host` Özellikle özelliğine sahip bir `triggerName` tetikleyici her zaman belirtmeniz gerekir çünkü arıyoruz.

## <a name="other-changes"></a>Diğer değişiklikler

### <a name="new-queries-property"></a>Yeni 'queries' özelliğini

Tüm eylem türleri artık adlı yeni bir giriş Destek `queries`. Bu giriş, yapılandırılmış bir nesne yerine, dize el ile derlemek sahip olabilir.

### <a name="renamed-parse-function-to-json"></a>'Json()' olarak yeniden adlandırıldı 'parse()' işlevi

`parse()` İşlevi artık yeniden adlandırılan `json()` gelecekteki içerik türleri için işlevi.

## <a name="enterprise-integration-apis"></a>Kurumsal tümleştirme API'leri

Bu şema gibi AS2 Kurumsal tümleştirme API'ler için yönetilen sürümler henüz desteklemiyor. Ancak, HTTP eylemi aracılığıyla dağıtılan mevcut BizTalk Apı'lerinize kullanabilirsiniz. Daha fazla bilgi için bkz: "içinde zaten dağıtılmış API uygulamalarınızı kullanma" [tümleştirme Haritası](https://www.zdnet.com/article/microsoft-outlines-its-cloud-and-server-integration-roadmap-for-2016/). 
