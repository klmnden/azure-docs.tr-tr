---
title: REST API'sini kullanarak bir Azure kaynağı için özel ölçümler Azure İzleyici ölçüm depoya gönder
description: REST API'sini kullanarak bir Azure kaynağı için özel ölçümler Azure İzleyici ölçüm depoya gönder
author: anirudhcavale
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 09/24/2018
ms.author: ancav
ms.subservice: metrics
ms.openlocfilehash: aa842979bf86410e9dab97d6209f336eb6b02bd3
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60253856"
---
# <a name="send-custom-metrics-for-an-azure-resource-to-the-azure-monitor-metric-store-by-using-a-rest-api"></a>REST API'sini kullanarak bir Azure kaynağı için özel ölçümler Azure İzleyici ölçüm depoya gönder

Bu makalede Azure kaynakları için özel ölçümleri bir REST API aracılığıyla Azure İzleyici ölçümleri mağazası gönderme işlemini gösterir. Azure İzleyicisi'nde ölçümler olduktan sonra standart ölçümleri ile bunu bunlarla birlikte her şeyi yapabilirsiniz. Örnek uyarı, grafik ve bunları diğer dış araçlar için yönlendirme.  

>[!NOTE]  
>REST API, yalnızca Azure kaynakları için özel ölçümleri gönderme verir. Farklı ortamlar veya şirket içi kaynakların ölçümlerini göndermek için kullanabileceğiniz [Application Insights](../../azure-monitor/app/api-custom-events-metrics.md).    


## <a name="create-and-authorize-a-service-principal-to-emit-metrics"></a>Oluşturma ve ölçümleri yaymak için bir hizmet sorumlusu yetkilendirme 

Konusunda bulunan yönergeleri kullanarak Azure Active Directory kiracınızda bir hizmet sorumlusu oluşturma [hizmet sorumlusu oluşturma](../../active-directory/develop/howto-create-service-principal-portal.md). 

Bu süreçte giderken aşağıdakilere dikkat edin: 

- Herhangi bir URL için oturum açma URL'si girebilirsiniz.  
- Bu uygulama için yeni bir istemci gizli anahtarı oluşturun.  
- Sonraki adımlarda, anahtar ve kullanmak için istemci kimliği kaydedin.  

Adım 1, izleme, ölçüm yayımcı izinleri karşı ölçümleri yayma istediğiniz kaynak bir parçası olarak oluşturulan uygulama verin. Özel ölçümler birçok kaynağa karşı yaymak için uygulamayı kullanmayı planlıyorsanız, kaynak grubu veya abonelik düzeyinde bu izinleri verebilir. 

## <a name="get-an-authorization-token"></a>Bir yetkilendirme belirteci alma
Bir komut istemi açın ve aşağıdaki komutu çalıştırın:

```shell
curl -X POST https://login.microsoftonline.com/<yourtenantid>/oauth2/token -F "grant_type=client_credentials" -F "client_id=<insert clientId from earlier step>" -F "client_secret=<insert client secret from earlier step>" -F "resource=https://monitoring.azure.com/"
```
Erişim belirteci yanıttan kaydedin.

![erişim belirteci](./media/metrics-store-custom-rest-api/accesstoken.png)

## <a name="emit-the-metric-via-the-rest-api"></a>REST API aracılığıyla ölçüm yayma 

1. Aşağıdaki JSON bir dosyaya yapıştırın ve kaydedileceği **custommetric.json** yerel bilgisayarınızda. JSON dosyasındaki süre parametresini güncelleştirin: 
    
    ```json
    { 
        "time": "2018-09-13T16:34:20", 
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

1. Komut İstemi penceresinde, ölçüm verilerini gönder: 
   - **azureRegion**. Kaynak ölçümleri için yayma Dağıtım bölgesi eşleşmesi gerekir. 
   - **ResourceId**.  Kaynak ölçümüne karşı takip ettiğiniz bir Azure kaynak kimliği.  
   - **AccessToken**. Önceden edindiğiniz belirteci yapıştırın.

     ```Shell 
     curl -X POST https://<azureRegion>.monitoring.azure.com/<resourceId>/metrics -H "Content-Type: application/json" -H "Authorization: Bearer <AccessToken>" -d @custommetric.json 
     ```
1. Zaman damgası ve JSON dosyasında değerlerini değiştirin. 
1. Birkaç dakika verileriniz için önceki iki adımı birkaç kez yineleyin.

## <a name="troubleshooting"></a>Sorun giderme 
İşleminin bir parçası olan bir hata iletisi alırsanız, aşağıdaki sorun giderme bilgileri göz önünde bulundurun:

1. Bir abonelik veya kaynak grubu karşı ölçümleri, bir Azure kaynağı olarak gönderilemez. 
1. Bir ölçüm 20 dakikadan eski olan depoya konulamaz. Ölçüm deposu, uyarı ve gerçek zamanlı grafik için optimize edilmiştir. 
2. Boyut adları sayısını değerlerin eşleşmesi gerekir ve bunun tersi de geçerlidir. Değerleri kontrol edin. 
2. Özel ölçümler desteklemeyen bir bölge karşı ölçümleri yayma. Bkz: [desteklenen bölgeler](../../azure-monitor/platform/metrics-custom-overview.md#supported-regions). 



## <a name="view-your-metrics"></a>Ölçümlerinizi görüntüleyin 

1. Azure Portal’da oturum açın. 

1. Sol taraftaki menüde **İzleyici**. 

1. Üzerinde **İzleyici** sayfasında **ölçümleri**. 

   ![Ölçümleri Seç](./media/metrics-store-custom-rest-api/metrics.png) 

1. Toplama dönemini değiştirmek **son 30 dakika**.  

1. İçinde **kaynak** açılan menüsünde, ilgili ölçümün yayılan kaynağı seçin.  

1. İçinde **ad alanları** açılan menüsünde, select **QueueProcessing**. 

1. İçinde **ölçümleri** açılan menüsünde, select **QueueDepth**.  

 
## <a name="next-steps"></a>Sonraki adımlar
- Daha fazla bilgi edinin [özel ölçümler](../../azure-monitor/platform/metrics-custom-overview.md).

