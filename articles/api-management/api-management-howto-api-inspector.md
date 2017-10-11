---
title: "API denetçisi - Azure API Management ile çağrıları izleme | Microsoft Docs"
description: "Azure API Management'te API Inspector'ı kullanarak çağrıları izleme öğrenin."
services: api-management
documentationcenter: 
author: steved0x
manager: erikre
editor: 
ms.assetid: 4b222327-c8a4-4f33-9a06-adff2a9834d9
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: apimpm
ms.openlocfilehash: a9d4d3be7f046af975f6dc25670070204848588c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-use-the-api-inspector-to-trace-calls-in-azure-api-management"></a>Azure API Management'te izleme API denetleyici kullanma çağırır
API Management Apı'lerinizi sorun giderme ve hata ayıklama ile yardımcı olacak bir API denetçisi aracı sağlar. API denetçisi program aracılığıyla kullanılabilir ve doğrudan Geliştirici portalından da kullanılabilir. 

İzleme işlemlerinin ek olarak, API Inspector ayrıca izler [ilke ifadesi](https://msdn.microsoft.com/library/azure/dn910913.aspx) değerlendirmeleri. Bir örnek için bkz: [bulut kapak bölüm 177: diğer API Management Özellikler](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/) ve 21:00 için ileri sarma.

Bu kılavuz, API Inspector'ı kullanarak bir adım adım kılavuz sağlar.

> [!NOTE]
> API denetçisi izlemeleri yalnızca oluşturulan ve ait abonelik anahtarlarını içeren istekler için kullanılabilir hale [yönetici](api-management-howto-create-groups.md) hesabı.
> 
> 

## <a name="trace-call"></a> Kullanım API Inspector'ı bir çağrı izlemek için
API denetçisi kullanmak için ekleyin bir **ocp apim izleme: true** isteği işlemi aramanız başlığına ve yükleyin ve belirtilen URL'yi kullanarak izleme incelemek **ocp apim izleme konumu** yanıt Üstbilgi. Bu programlı şekilde yapılabilir ve doğrudan Geliştirici portalından da yapılabilir.

Bu öğretici yapılandırılan temel hesaplayıcı API'si kullanarak izleme işlemleri için API denetçisi kullanmayı gösterir [ilk API'nizi yönetme](api-management-get-started.md) başlama Öğreticisi. Bu öğreticinin tamamlanan yapmadıysanız, yalnızca temel hesaplayıcı API'si almak için birkaç dakika sürer veya seçtiğiniz Echo API'si gibi başka bir API kullanabilirsiniz. Her API Management hizmeti örneği, API Management’i denemek ve hakkında bilgi almak için kullanılabilecek bir Echo API’si ile önceden yapılandırılmış olarak gelir. Echo API'si, hangi giriş kendisine gönderilen geri döndürür. Kullanmak için bir HTTP fiili çağırabileceği ve dönüş değeri, yalnızca size gönderilen olacaktır. 

Kullanmaya başlamak için tıklayın **Geliştirici Portalı** API Management hizmetiniz için Azure Portalı'nda. İşlemleri görüntülemek ve bir API'nin işlemlerini test etmek için kullanışlı bir yol sağlayan doğrudan Geliştirici portalından çağrılabilir.

> Henüz bir API Management hizmeti örneği oluşturmadıysanız, bkz: [bir API Management hizmet örneği oluşturma] [ Create an API Management service instance] içinde [Azure API Management ile çalışmaya başlama] [ Get started with Azure API Management] Öğreticisi.
> 
> 

![API Management Geliştirici Portalı][api-management-developer-portal-menu]

Tıklatın **API'leri** üst menüsünden ve ardından **temel hesaplayıcı**.

![Echo API’si][api-management-api]

Tıklatın **deneyin** denemek için **iki tamsayı Ekle** işlemi.

![Deneyin][api-management-open-console]

Varsayılan parametre değerlerini koruyun ve abonelik anahtar için kullanmak istediğiniz ürün **abonelik anahtarı** açılır.

Geliştirici Portalı'nda varsayılan **Ocp Apim izleme** üstbilgisi zaten ayarlandı **doğru**. Bir izleme oluşturulan olup olmadığına bakılmaksızın bu başlığı yapılandırır.

![Gönder][api-management-http-get]

Tıklatın **Gönder** işlemini çağırmak için.

![Gönder][api-management-send-results]

Yanıt Üstbilgileri olacak bir **ocp apim izleme konumu** aşağıdaki örneğe benzer bir değere sahip.

```
ocp-apim-trace-location : https://contosoltdxw7zagdfsprykd.blob.core.windows.net/apiinspectorcontainer/ZW3e23NsW4wQyS-SHjS0Og2-2?sv=2013-08-15&sr=b&sig=Mgx7cMHsLmVDv%2B%2BSzvg3JR8qGTHoOyIAV7xDsZbF7%2Bk%3D&se=2014-05-04T21%3A00%3A13Z&sp=r&verify_guid=a56a17d83de04fcb8b9766df38514742
```

İzleme belirtilen konumdan indirilir ve sonraki adımda gösterildiği gibi gözden. Yalnızca son 100 günlük girişlerini depolanır ve günlük konumlarını dönüş yeniden unutmayın. 100'den fazla çağrı yapmak isterseniz bunu izlemenin etkin, sonunda yerinde ilk izlemeleri üzerine başlar.

## <a name="inspect-trace"></a>İzleme inceleyin.
İzleme değerlerde gözden geçirmek için izleme dosyasını indirin **ocp apim izleme konumu** URL. JSON biçiminde bir metin dosyasıdır ve aşağıdaki örneğe benzer girdiler içerir.

```json
{
    "traceId": "abcd8ea63d134c1fabe6371566c7cbea",
    "traceEntries": {
        "inbound": [
            {
                "source": "handler",
                "timestamp": "2015-06-23T19:51:35.2998610Z",
                "elapsed": "00:00:00.0725926",
                "data": {
                    "request": {
                        "method": "GET",
                        "url": "https://contoso5.azure-api.net/calc/add?a=51&b=49",
                        "headers": [
                            {
                                "name": "Ocp-Apim-Subscription-Key",
                                "value": "5d7c41af64a44a68a2ea46580d271a59"
                            },
                            {
                                "name": "Connection",
                                "value": "Keep-Alive"
                            },
                            {
                                "name": "Host",
                                "value": "contoso5.azure-api.net"
                            }
                        ]
                    }
                }
            },
            {
                "source": "mapper",
                "timestamp": "2015-06-23T19:51:35.2998610Z",
                "elapsed": "00:00:00.0726213",
                "data": {
                    "configuration": {
                        "api": {
                            "from": "/calc",
                            "to": {
                                "scheme": "http",
                                "host": "calcapi.cloudapp.net",
                                "port": 80,
                                "path": "/api",
                                "queryString": "",
                                "query": {},
                                "isDefaultPort": true
                            }
                        },
                        "operation": {
                            "method": "GET",
                            "uriTemplate": "/add?a={a}&b={b}"
                        },
                        "user": {
                            "id": 1,
                            "groups": [
                                "Administrators",
                                "Developers"
                            ]
                        },
                        "product": {
                            "id": 1
                        }
                    }
                }
            },
            {
                "source": "handler",
                "timestamp": "2015-06-23T19:51:35.2998610Z",
                "elapsed": "00:00:00.0727522",
                "data": {
                    "message": "Request is being forwarded to the backend service.",
                    "request": {
                        "method": "GET",
                        "url": "http://calcapi.cloudapp.net/api/add?a=51&b=49",
                        "headers": [
                            {
                                "name": "Ocp-Apim-Subscription-Key",
                                "value": "5d7c41af64a44a68a2ea46580d271a59"
                            },
                            {
                                "name": "X-Forwarded-For",
                                "value": "33.52.215.35"
                            }
                        ]
                    }
                }
            }
        ],
        "outbound": [
            {
                "source": "handler",
                "timestamp": "2015-06-23T19:51:35.4256650Z",
                "elapsed": "00:00:00.1960601",
                "data": {
                    "response": {
                        "status": {
                            "code": 200,
                            "reason": "OK"
                        },
                        "headers": [
                            {
                                "name": "Pragma",
                                "value": "no-cache"
                            },
                            {
                                "name": "Content-Length",
                                "value": "124"
                            },
                            {
                                "name": "Cache-Control",
                                "value": "no-cache"
                            },
                            {
                                "name": "Content-Type",
                                "value": "application/xml; charset=utf-8"
                            },
                            {
                                "name": "Date",
                                "value": "Tue, 23 Jun 2015 19:51:35 GMT"
                            },
                            {
                                "name": "Expires",
                                "value": "-1"
                            },
                            {
                                "name": "Server",
                                "value": "Microsoft-IIS/8.5"
                            },
                            {
                                "name": "X-AspNet-Version",
                                "value": "4.0.30319"
                            },
                            {
                                "name": "X-Powered-By",
                                "value": "ASP.NET"
                            }
                        ]
                    }
                }
            },
            {
                "source": "handler",
                "timestamp": "2015-06-23T19:51:35.4256650Z",
                "elapsed": "00:00:00.1961112",
                "data": {
                    "message": "Response headers have been sent to the caller. Starting to stream the response body."
                }
            },
            {
                "source": "handler",
                "timestamp": "2015-06-23T19:51:35.4256650Z",
                "elapsed": "00:00:00.1963155",
                "data": {
                    "message": "Response body streaming to the caller is complete."
                }
            }
        ]
    }
}
```

## <a name="next-steps"> </a>Sonraki adımlar
* İlke ifadelerinde izleme, gösterisini izleyin [bulut kapak bölüm 177: diğer API Management Özellikler](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/). 21: demo görmek için 00 için ileri sarma.

> [!VIDEO https://channel9.msdn.com/Shows/Cloud+Cover/Episode-177-More-API-Management-Features-with-Vlad-Vinogradsky/player]
> 
> 

[Use API Inspector to trace a call]: #trace-call
[Inspect the trace]: #inspect-trace
[Next steps]: #next-steps

[Configure API settings]: api-management-howto-create-apis.md#configure-api-settings
[Responses]: api-management-howto-add-operations.md#responses
[How create and publish a product]: api-management-howto-add-products.md

[Get started with Azure API Management]: api-management-get-started.md
[Create an API Management service instance]: api-management-get-started.md#create-service-instance
[Azure Classic Portal]: https://manage.windowsazure.com/


[api-management-developer-portal-menu]: ./media/api-management-howto-api-inspector/api-management-developer-portal-menu.png
[api-management-api]: ./media/api-management-howto-api-inspector/api-management-api.png
[api-management-echo-api-get]: ./media/api-management-howto-api-inspector/api-management-echo-api-get.png
[api-management-developer-key]: ./media/api-management-howto-api-inspector/api-management-developer-key.png
[api-management-open-console]: ./media/api-management-howto-api-inspector/api-management-open-console.png
[api-management-http-get]: ./media/api-management-howto-api-inspector/api-management-http-get.png
[api-management-send-results]: ./media/api-management-howto-api-inspector/api-management-send-results.png




