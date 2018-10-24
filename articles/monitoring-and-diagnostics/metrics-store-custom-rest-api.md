---
title: Özel ölçümleriniz Azure İzleyici ölçüm için bir Azure kaynağı için bir REST API kullanarak depolama Gönder
description: Özel ölçümleriniz Azure İzleyici ölçüm için bir Azure kaynağı için bir REST API kullanarak depolama Gönder
author: anirudhcavale
services: azure-monitor
ms.service: azure-monitor
ms.topic: howto
ms.date: 09/24/2018
ms.author: ancav
ms.component: metrics
ms.openlocfilehash: d36697e6b5765ecf35ed9b3add45cff6c33823a5
ms.sourcegitcommit: 5c00e98c0d825f7005cb0f07d62052aff0bc0ca8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/24/2018
ms.locfileid: "49958235"
---
# <a name="send-custom-metrics-for-an-azure-resource-to-the-azure-monitor-metric-store-using-a-rest-api"></a>Özel ölçümleriniz Azure İzleyici ölçüm için bir Azure kaynağı için bir REST API kullanarak depolama Gönder

Bu makalede Azure kaynakları için özel ölçümleri bir REST API aracılığıyla Azure İzleyici ölçümleri mağazası gönderme işlemini gösterir.  Azure İzleyicisi'nde ölçümler olduktan sonra bunları grafik, uyarı verme, bunları yönlendirme diğer harici araçlar, vb. gibi standart ölçümlerle bunu ile her şey yapabilirsiniz.  

>[!NOTE] 
>REST API, yalnızca Azure kaynakları için özel ölçümleri gönderme verir. Farklı ortamlar veya şirket içi kaynakların ölçümlerini göndermek için kullanabileceğiniz [Application Insights](../application-insights/app-insights-api-custom-events-metrics.md).    


## <a name="create-and-authorize-a-service-principal-to-emit-metrics"></a>Oluşturma ve ölçümleri yaymak için bir hizmet sorumlusu yetkilendirme 

Azure Active Directory kiracınızdaki konusunda bulunan yönergeleri kullanarak bir hizmet ilkesi oluşturma [hizmet sorumlusu oluşturma](../active-directory/develop/howto-create-service-principal-portal.md). 

Bu süreç boyunca çalışırken aşağıdakileri unutmayın: 

- Herhangi bir URL için oturum açma URL'si girebilirsiniz.  
- Bu uygulama için yeni istemci gizli anahtarı oluşturma  
- Sonraki adımlarda, anahtar ve kullanmak için istemci kimliği kaydedin.  

Uygulama adımının bir parçası oluşturulan ölçümler karşı yayma istediğiniz kaynağa 1 "İzleme ölçümleri Publisher" izinleri verin. Özel ölçümler birçok kaynağa karşı yaymak için uygulamayı kullanmayı planlıyorsanız, kaynak grubu veya abonelik düzeyinde bu izinleri verebilir. 

## <a name="get-an-authorization-token"></a>Bir yetkilendirme belirteci alma
Bir komut istemi açın ve aşağıdaki komutu çalıştırın
```shell
curl -X POST https://login.microsoftonline.com/<yourtenantid>/oauth2/token -F "grant_type=client_credentials" -F "client_id=<insert clientId from earlier step> " -F "client_secret=<insert client secret from earlier step>" -F "resource=https://monitoring.azure.com/"
```
Yanıttan erişim belirtecini kaydetme

![Erişim Belirteci](./media/metrics-store-custom-rest-api/accesstoken.png)

## <a name="emit-the-metric-via-the-rest-api"></a>REST API aracılığıyla ölçüm yayma 

1. Aşağıdaki JSON bir dosyaya yapıştırın ve bunu yerel bilgisayarınızda custommetric.json kaydedin. JSON dosyasındaki süre parametresini güncelleştirin. 
    
    ```json
    { 
        "time": "2018-09-13T16:34:20”, 
        "data": { 
            "baseData": { 
                "metric": "QueueDepth", 
                "namespace": "QueueProcessing", 
                "dimNames": [ 
                  "QueueName", 
                  "MessageType" 
                ], 
                "series": [ 
                  { 
                    "dimValues": [ 
                      "ImagesToProcess", 
                      "JPEG" 
                    ], 
                    "min": 3, 
                    "max": 20, 
                    "sum": 28, 
                    "count": 3 
                  } 
                ] 
            } 
        } 
    } 
    ``` 

1. Ölçüm verilerini, komut istemi penceresinde gönderin 
    - Azure bölgesi – Dağıtım bölgesi için ölçümleri yayma kaynak eşleşmesi gerekir. 
    - ResourceId – ölçümün izlemekte olduğunuz Azure kaynağının kaynak kimliği'ni tıklatın.  
    - Erişim belirteci-daha önce edinilen belirteci yapıştırma

    ```Shell 
    curl -X POST curl -X POST https://<azureRegion>.monitoring.azure.com/<resourceId> /metrics -H "Content-Type: application/json" -H "Authorization: Bearer <AccessToken>" -d @custommetric.json 
    ```
1. Zaman damgası ve JSON dosyasında değerlerini değiştirin. 
1. Birden çok dakikalığına verileriniz için önceki iki adımı birkaç kez yineleyin.

## <a name="troubleshooting"></a>Sorun giderme 
Aşağıdakileri de göz önünde bulundurur işleminin bir parçası olan bir hata iletisi alırsanız

1. Bir abonelik veya kaynak grubu karşı ölçümleri, bir Azure kaynağı olarak gönderilemez. 
1. Bir ölçüm 20 dakikadan eski olan depoya konulamaz. Ölçüm deposu, uyarı ve gerçek zamanlı grafik için optimize edilmiştir. 
2. Boyut adı sayısı değerleri ve tersi eşleşmesi gerekir. Değerleri kontrol edin. 
2. Özel ölçümler desteklemeyen bir bölge karşı ölçümleri yayma. Bkz: [desteklenen bölgeler özel ölçüm (Önizleme)](metrics-custom-overview.md#supported-regions) 



## <a name="view-your-metrics"></a>Ölçümlerinizi görüntüleyin 

1. Azure portalında oturum açma 

1. Sol taraftaki menüde **İzleyicisi** 

1. İzleyici sayfasında tıklayın **ölçümleri**. 

   ![Erişim Belirteci](./media/metrics-store-custom-rest-api/metrics.png) 

1. Toplama dönemini değiştirmek **son 30 dakika**.  

1. İçinde *kaynak* açılan listesinde, ilgili ölçümün yayılan kaynağı seçin.  

1. İçinde *ad alanları* açılan listesinde, select **QueueProcessing** 

1. İçinde *ölçümleri* seçin, açılan menü **QueueDepth**.  

 
## <a name="next-steps"></a>Sonraki adımlar
- Daha fazla bilgi edinin [özel ölçümler](metrics-custom-overview.md).