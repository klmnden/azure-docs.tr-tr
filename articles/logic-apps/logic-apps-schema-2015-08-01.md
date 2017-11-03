---
title: "Şema güncelleştirmeleri Ağustos 1 2015 preview - Azure Logic Apps | Microsoft Docs"
description: "Şema sürüm 2015-08-01-Önizleme ile Azure Logic Apps için JSON tanımları oluşturma"
author: stepsic-microsoft-com
manager: anneta
editor: 
services: logic-apps
documentationcenter: 
ms.assetid: 0d03a4d4-e8a8-4c81-aed5-bfd2a28c7f0c
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.custom: H1Hack27Feb2017
ms.date: 05/31/2016
ms.author: LADocs; stepsic
ms.openlocfilehash: 35d7a56d5607dcc18a4407c65b92962d3d0dcd1d
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="schema-updates-for-azure-logic-apps---august-1-2015-preview"></a>Azure mantıksal uygulamaları - 1 Ağustos 2015 preview şema güncelleştirmeleri

Bu yeni şema ve API sürümü Azure mantıksal uygulamaları için daha güvenilir ve kullanmayı daha kolay logic apps kılmak anahtar iyileştirmeler içerir:

*   **APIApp** eylem türü yeni bir güncelleştirilir [ **APIConnection** ](#api-connections) eylem türü.
*   **Yineleme** adlandırılır [ **Foreach**](#foreach).
*   [ **HTTP dinleyicisi** API uygulaması](#http-listener) artık gerekli değildir.
*   Alt iş akışlarını çağırma kullanan bir [yeni şema](#child-workflows).

<a name="api-connections"></a>
## <a name="move-to-api-connections"></a>API bağlantıları taşıyın

Büyük değişiklik, artık API'lerini kullanabilmek için API uygulamaları Azure aboneliğinize dağıtmak olmasıdır. API kullanabileceğiniz yollar şunlardır:

* Yönetilen API'ler
* Özel Web API'leri

Kendi yönetim ve barındırma modellerini farklı olduğundan her şekilde biraz farklı şekilde ele alınır. Bu model bir avantajı, artık Azure kaynak grubunda dağıtılan kaynaklara sınırlı ' dir. 

### <a name="managed-apis"></a>Yönetilen API'ler

Microsoft Office 365, Salesforce, Twitter ve FTP gibi sizin adınıza bazı API'leri yönetir. Bazı yönetilen API'ler kullanabilirsiniz-diğer yapılandırma gerektirirken, Bing Çevir gibi ise. Bu yapılandırma adlı bir *bağlantı*.

Örneğin, Office 365 kullandığınızda, Office 365 oturum açma belirtecinizi içeren bir bağlantı oluşturmanız gerekir. Bu belirteç güvenli bir şekilde depolanır ve böylece mantıksal uygulamanızı her zaman Office 365 API çağrısı yenilenir. Alternatif olarak, SQL ya da FTP sunucunuza bağlanmak istiyorsanız, bağlantı dizesi bir bağlantı oluşturmanız gerekir. 

Bu tanımı'nda, bu eylemleri denir `APIConnection`. Bir e-posta göndermek için Office 365 çağıran bir bağlantı bir örneği burada verilmiştir:

```
{
    "actions": {
        "Send_Email": {
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
                "method": "post",
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

`host` Nesnesidir API bağlantıları için benzersizdir ve satırındaki bölümleri içeren girişleri kısmı: `api` ve `connection`.

`api` , API yönetildiği URL'sini barındırılan çalışma zamanı sahiptir. Tüm kullanılabilir yönetilen API'ler çağırarak görebilirsiniz `GET https://management.azure.com/subscriptions/{subid}/providers/Microsoft.Web/managedApis/?api-version=2015-08-01-preview`.

Bir API kullandığınızda, API sahip olabilir veya herhangi bir değil *bağlantı parametrelerini* tanımlanmış. API seçili değilse, hiçbir *bağlantı* gereklidir. API varsa, bir bağlantı oluşturmanız gerekir. Oluşturulan bağlantı seçtiğiniz adına sahip. Ardından adına başvuru `connection` içinde nesne `host` nesne. Bir kaynak grubunda bir bağlantı oluşturmak için arayın:

```
PUT https://management.azure.com/subscriptions/{subid}/resourceGroups/{rgname}/providers/Microsoft.Web/connections/{name}?api-version=2015-08-01-preview
```

Aşağıdaki gövde ile:

```
{
  "properties": {
    "api": {
      "id": "/subscriptions/{subid}/providers/Microsoft.Web/managedApis/azureblob"
    },
    "parameterValues": {
        "accountName": "{The name of the storage account -- the set of parameters is different for each API}"
    }
  },
  "location": "{Logic app's location}"
}
```

### <a name="deploy-managed-apis-in-an-azure-resource-manager-template"></a>Yönetilen API'leri bir Azure Resource Manager şablonu dağıtma

Etkileşimli oturum açma gerekli olmadığı sürece, tüm bir uygulamayı bir Azure Resource Manager şablonu oluşturabilirsiniz.
Oturum açma gerekiyorsa, her şeyi Azure Resource Manager şablonu ile ayarlayabilirsiniz ancak bağlantılarını yetkilendirmek için portalını ziyaret etmek devam ediyor. 

```
    "resources": [{
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
        }
    }, {
        "type": "Microsoft.Logic/workflows",
        "apiVersion": "2015-08-01-preview",
        "name": "[parameters('logicAppName')]",
        "location": "[resourceGroup().location]",
        "dependsOn": ["[resourceId('Microsoft.Web/connections', 'azureblob')]"
        ],
        "properties": {
            "sku": {
                "name": "[parameters('sku')]",
                "plan": {
                    "id": "[concat(resourceGroup().id, '/providers/Microsoft.Web/serverfarms/',parameters('svcPlanName'))]"
                }
            },
            "definition": {
                "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2015-08-01-preview/workflowdefinition.json#",
                "actions": {
                    "Create_file": {
                        "type": "apiconnection",
                        "inputs": {
                            "host": {
                                "api": {
                                    "runtimeUrl": "https://logic-apis-westus.azure-apim.net/apim/azureblob"
                                },
                                "connection": {
                                    "name": "@parameters('$connections')['azureblob']['connectionId']"
                                }
                            },
                            "method": "post",
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
                "contentVersion": "1.0.0.0",
                "outputs": {},
                "parameters": {
                    "$connections": {
                        "defaultValue": {},
                        "type": "Object"
                    }
                },
                "triggers": {
                    "recurrence": {
                        "type": "Recurrence",
                        "recurrence": {
                            "frequency": "Day",
                            "interval": 1
                        }
                    }
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
            }
        }
    }]
```

Bu örnekte bağlantıların kaynak grubunuzdaki Canlı yalnızca kaynakları olduğunu görebilirsiniz. Aboneliğinizdeki kullanılabilir yönetilen API'ler başvuruyor.

### <a name="your-custom-web-apis"></a>Özel Web API'leri

Yerleşik değil Microsoft tarafından yönetilen olanları kendi API kullanırsanız kullanın **HTTP** bunları çağrılacak eylem. İdeal bir deneyim için API için Swagger uç nokta maruz bırakmamalısınız. Bu uç noktası girişleri oluşturmak mantığı Uygulama Tasarımcısı sağlar ve API için çıkartır. Swagger Tasarımcı yalnızca girişleri ve çıkışları donuk JSON nesneler olarak gösterebilir.

İşte yeni gösteren bir örnek `metadata.apiDefinitionUrl` özelliği:

```
{
   "actions": {
        "mycustomAPI": {
            "type": "http",
            "metadata": {
              "apiDefinitionUrl": "https://mysite.azurewebsites.net/api/apidef/"  
            },
            "inputs": {
                "uri": "https://mysite.azurewebsites.net/api/getsomedata",
                "method": "GET"
            }
        }
    }
}
```

Azure App Service'te Web API'nizi barındırıyorsanız, Web API Tasarımcısı'nda kullanılabilir eylemlerin listesini otomatik olarak görünür. Aksi durumda, URL'de doğrudan yapıştırmak zorunda. Swagger destekler ne olursa olsun yöntemleriyle API kendisini güvenli rağmen mantığı Uygulama Tasarımcısı'nda kullanılabilmesi için Swagger uç nokta kimliği doğrulanmamış gerekir.

### <a name="call-deployed-api-apps-with-2015-08-01-preview"></a>Dağıtılan API apps 2015-08-01-Önizleme ile çağırın

Bir API uygulamasını daha önce dağıttıysanız, uygulamayla çağırabilirsiniz **HTTP** eylem.

Örneğin, Dropbox dosyaları listelemek için kullanırsanız, **2014-12-01-Önizleme** şema sürümü tanımı şöyle olabilir:

```
{
    "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2014-12-01-preview/workflowdefinition.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "/subscriptions/423db32d-...-b59f14c962f1/resourcegroups/avdemo/providers/Microsoft.AppService/apiapps/dropboxconnector/token": {
            "defaultValue": "eyJ0eX...wCn90",
            "type": "String",
            "metadata": {
                "token": {
                    "name": "/subscriptions/423db32d-...-b59f14c962f1/resourcegroups/avdemo/providers/Microsoft.AppService/apiapps/dropboxconnector/token"
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
                    "id": "/subscriptions/423db32d-...-b59f14c962f1/resourcegroups/avdemo/providers/Microsoft.AppService/apiapps/dropboxconnector",
                    "gateway": "https://avdemo.azurewebsites.net"
                },
                "operation": "ListFiles",
                "parameters": {
                    "FolderPath": "/myfolder"
                },
                "authentication": {
                    "type": "Raw",
                    "scheme": "Zumo",
                    "parameter": "@parameters('/subscriptions/423db32d-...-b59f14c962f1/resourcegroups/avdemo/providers/Microsoft.AppService/apiapps/dropboxconnector/token')"
                }
            }
        }
    }
}
```

Mantıksal uygulama tanımını parametreleri bölümünü değişmeden kalır ancak bu örnek gibi eşdeğer HTTP eylemi oluşturabileceğiniz:

```
{
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
                    "parameter": "@parameters('/subscriptions/423db32d-...-b59f14c962f1/resourcegroups/avdemo/providers/Microsoft.AppService/apiapps/dropboxconnector/token')"
                }
            }
        }
    }
}
```

Bu özellikleri tek tek taramasını:

| Action özelliği | Açıklama |
| --- | --- |
| `type` |`Http`Onun yerine`APIapp` |
| `metadata.apiDefinitionUrl` |Bu eylem mantıksal Uygulama Tasarımcısı'nda kullanmak için gelen oluşturulan meta veri uç noktasının şunları içerir:`{api app host.gateway}/api/service/apidef/{last segment of the api app host.id}/?api-version=2015-01-14&format=swagger-2.0-standard` |
| `inputs.uri` |Gelen oluşturulur:`{api app host.gateway}/api/service/invoke/{last segment of the api app host.id}/{api app operation}?api-version=2015-01-14` |
| `inputs.method` |Her zaman`POST` |
| `inputs.body` |API uygulaması parametreleri özdeş |
| `inputs.authentication` |API uygulaması kimlik doğrulama için aynı |

Bu yaklaşım, tüm API uygulaması eylemler için çalışması gerekir. Ancak, bu önceki API uygulamaları artık desteklendiğini unutmayın. Bu nedenle bir iki diğer önceki seçenekleri, yönetilen bir API veya özel Web API'nizi barındıran taşımanız gerekir.

<a name="foreach"></a>
## <a name="renamed-repeat-to-foreach"></a>'Repeat' 'foreach' yeniden adlandırıldı

Önceki şema sürümü için çok müşteri geri bildirimi aldık, **yineleyin** kafa karıştırıcı ve düzgün şekilde yakalama alamadık **yineleyin** gerçekten için-her bir döngü oluştu. Sonuç olarak, biz adlandırdınız `repeat` için `foreach`. Örneğin, daha önce aşağıdakileri yazarsınız:

```
{
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
}
```

Şimdi, yazarsınız:

```
{
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
}
```

İşlev `@repeatItem()` üzerinden yinelendiğinde geçerli öğe başvurmak için daha önce kullanılmış. Bu işlev artık için Basitleştirilmiş `@item()`. 

### <a name="reference-outputs-from-foreach"></a>'Foreach' başvuru çıkışlarından

Basitleştirme, çıkışlarından için `foreach` Eylemler adlı bir nesne değil sarılır `repeatItems`. While önceki çıkışları `repeat` örnek olan:

```
{
    "repeatItems": [
        {
            "name": "pingBing",
            "inputs": {
                "uri": "https://www.bing.com/search?q=0",
                "method": "GET"
            },
            "outputs": {
                "headers": { },
                "body": "<!DOCTYPE html><html lang=\"en\" xml:lang=\"en\" xmlns=\"http://www.w3.org/1999/xhtml\" xmlns:Web=\"http://schemas.live.com/Web/\">...</html>"
            }
            "status": "Succeeded"
        }
    ]
}
```

Bu çıktı sunulmuştur:

```
[
    {
        "name": "pingBing",
        "inputs": {
            "uri": "https://www.bing.com/search?q=0",
            "method": "GET"
        },
        "outputs": {
            "headers": { },
            "body": "<!DOCTYPE html><html lang=\"en\" xml:lang=\"en\" xmlns=\"http://www.w3.org/1999/xhtml\" xmlns:Web=\"http://schemas.live.com/Web/\">...</html>"
        }
        "status": "Succeeded"
    }
]
```

Daha önce bu çıktılar başvururken eylem gövdesini almak için:

```
{
    "actions": {
        "secondAction": {
            "type": "Http",
            "repeat": "@outputs('pingBing').repeatItems",
            "inputs": {
                "method": "POST",
                "uri": "http://www.example.com",
                "body": "@repeatItem().outputs.body"
            }
        }
    }
}
```

Bunun yerine şimdi yapabilirsiniz:

```
{
    "actions": {
        "secondAction": {
            "type": "Http",
            "foreach": "@outputs('pingBing')",
            "inputs": {
                "method": "POST",
                "uri": "http://www.example.com",
                "body": "@item().outputs.body"
            }
        }
    }
}
```

Bu değişikliklerle işlevleri `@repeatItem()`, `@repeatBody()`, ve `@repeatOutputs()` kaldırılır.

<a name="http-listener"></a>
## <a name="native-http-listener"></a>Yerel HTTP dinleyicisi

HTTP dinleyicisi özellikleri artık yerleşiktir. Bu nedenle, artık bir HTTP dinleyicisi API uygulamasına dağıtmanız gerekir. Bkz: [mantığı uygulama uç noktanızı aranabilir burada nasıl yapılacağını tam ayrıntılarını](../logic-apps/logic-apps-http-endpoint.md). 

Biz bu değişikliklerle kaldırılan `@accessKeys()` biz yerine işlevi `@listCallbackURL()` gerektiğinde uç nokta alma işlevi. Ayrıca, mantıksal uygulamanızı şimdi en az bir tetikleyici tanımlamanız gerekir. İsterseniz `/run` iş akışını tetikler birine sahip olmalıdır: `manual`, `apiConnectionWebhook`, veya `httpWebhook`.

<a name="child-workflows"></a>
## <a name="call-child-workflows"></a>Alt iş akışlarını çağırma

Daha önce alt iş akışlarını çağırma akışına giderek, erişim belirtecini alma ve alt iş akışının çağırmak için istediğiniz mantıksal uygulama tanımında belirteç yapıştırma gereklidir. Tüm gizli tanımı yapıştırma zorunda kalmamak için yeni şema ile Logic Apps altyapısı SAS alt iş akışı için çalışma zamanında otomatik olarak oluşturur. Örnek aşağıda verilmiştir:

```
"mynestedwf": {
    "type": "workflow",
    "inputs": {
        "host": {
            "id": "/subscriptions/xxxxyyyyzzz/resourceGroups/rg001/providers/Microsoft.Logic/mywf001",
            "triggerName": "myendpointtrigger"
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

İkinci bir geliştirme biz alt iş akışları için gelen istek tam erişimi vermiş olur. Anlamına parametrelerinde geçirebilirsiniz *sorguları* bölüm ve *üstbilgileri* nesne ve tüm gövdesinin tam olarak tanımlayabilirsiniz.

Son olarak, alt iş akışı için gerekli bir değişiklik yoktur. Şimdi daha önce bir alt iş akışı doğrudan çağırabilirsiniz olsa da, çağırmak üst iş akışında bir tetikleyici uç noktasını tanımlamanız gerekir. Genellikle, sahip bir tetikleyici eklemek `manual` yazın ve ardından üst tanımında tetikleyicisi kullanın. Not `host` özelliği özellikle sahip bir `triggerName` tetikleyicinin her zaman belirtmeniz gerekir çünkü çağırma.

## <a name="other-changes"></a>Diğer değişiklikler

### <a name="new-queries-property"></a>Yeni 'sorguları' özelliği

Tüm eylem türleri şimdi adlı yeni bir giriş Destek `queries`. Bu giriş, yapılandırılmış nesne yerine dize el ile birleştirin gerek kalmadan olabilir.

### <a name="renamed-parse-function-to-json"></a>'Json()' olarak yeniden adlandırıldı 'parse()' işlevine

Biz yeniden adlandırılmış şekilde daha fazla içerik türleri yakında ekliyoruz `parse()` için işlev `json()`.

## <a name="coming-soon-enterprise-integration-apis"></a>Yakında: Kurumsal tümleştirme API'leri

Biz yönetilen sürümleri henüz AS2 gibi Kurumsal tümleştirme API'leri yok. Bu arada, var olan dağıtılmış BizTalk API'leri aracılığıyla HTTP eylemi kullanabilirsiniz. Ayrıntılar için bkz: "zaten dağıtılmış API uygulamalarınızı kullanarak" [tümleştirme Haritası](http://www.zdnet.com/article/microsoft-outlines-its-cloud-and-server-integration-roadmap-for-2016/). 
