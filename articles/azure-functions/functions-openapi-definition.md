---
title: Azure API Management ile bir işlev için Openapı tanımı oluşturma
description: Azure’da diğer uygulama ve hizmetlerin işlevinize çağrı yapmasına imkan tanıyan bir OpenAPI tanımı oluşturun.
services: functions
keywords: OpenAPI, Swagger, bulut uygulamaları, bulut hizmetleri,
author: ggailey777
manager: jeconnoc
ms.service: azure-functions
ms.topic: tutorial
ms.date: 05/08/2019
ms.author: glenga
ms.reviewer: sunayv
ms.custom: mvc, cc996988-fb4f-47
ms.openlocfilehash: 255a7c9d0b9da15176fca90c6934a84fa0f863ed
ms.sourcegitcommit: 1d257ad14ab837dd13145a6908bc0ed7af7f50a2
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/09/2019
ms.locfileid: "65501866"
---
# <a name="create-an-openapi-definition-for-a-function-with-azure-api-management"></a>Azure API Management ile bir işlev için Openapı tanımı oluşturma

REST API'ler genellikle bir Openapı tanımı kullanarak açıklanmıştır. Bu tanım, bir API’de hangi işlemlerin kullanılabildiğinin yanı sıra API için istek ve yanıt verilerinin nasıl yapılandırılması gerektiğiyle ilgili bilgileri içerir.

Bu öğreticide, bir rüzgar türbini için acil onarımın uygun maliyetli olup olmadığını belirleyen bir işlev oluşturursunuz. Ardından kullanarak işlev uygulaması için Openapı tanımı oluşturma [Azure API Management](../api-management/api-management-key-concepts.md) böylece diğer uygulama ve hizmetlerden işlevi çağrılabilir.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Azure’da işlev oluşturma
> * Azure API Management ile Openapı tanımı oluşturma
> * İşleve çağrı yaparak tanımı test etme

## <a name="create-a-function-app"></a>İşlev uygulaması oluşturma

İşlevlerinizin yürütülmesini barındıran bir işlev uygulamasına sahip olmanız gerekir. Ölçeklendirme ve kaynaklarının paylaşımı bir mantıksal birim için daha kolay yönetilmesi, dağıtılması, işlevleri Grup işlevi uygulaması sağlar.

[!INCLUDE [Create function app Azure portal](../../includes/functions-create-function-app-portal.md)]

## <a name="create-the-function"></a>İşlevi oluşturma

Bu öğreticide, iki parametre alan bir HTTP ile tetiklenen işlev kullanılır:

* Onarma, saat türbin yapmak için gereken tahmini süre.
* Kilovat cinsinden türbinin kapasitesi. 

Daha sonra, işlev tarafından onarım maliyetinin ne olacağı ve türbinin 24 saatlik bir dönemde ne kadar gelir kazandırabileceği hesaplanır. HTTP oluşturmak için işlev tetiklenen [Azure portalında](https://portal.azure.com).

1. İşlev uygulamanızı genişletin ve **İşlevler**'in yanındaki **+** düğmesini seçin. Seçin **portal** > **devam**.

1. Seçin **daha fazla şablon...** , ardından **bitirme ve görünüm şablonları**

1. Select HTTP tetikleyicisi, türü `TurbineRepair` işlevi **adı**, seçin `Function` için  **[kimlik doğrulama düzeyi](functions-bindings-http-webhook.md#http-auth)** ve ardından  **Oluşturma**.  

    ![HTTP işlev için Openapı oluşturma](media/functions-openapi-definition/select-http-trigger-openapi.png)

1. Run.csx içeriklerini C# komut dosyasındaki kodu aşağıdaki kodla ve ardından **Kaydet**:

    ```csharp
    #r "Newtonsoft.Json"
    
    using System.Net;
    using Microsoft.AspNetCore.Mvc;
    using Microsoft.Extensions.Primitives;
    using Newtonsoft.Json;
    
    const double revenuePerkW = 0.12;
    const double technicianCost = 250;
    const double turbineCost = 100;
    
    public static async Task<IActionResult> Run(HttpRequest req, ILogger log)
    {
        // Get query strings if they exist
        int tempVal;
        int? hours = Int32.TryParse(req.Query["hours"], out tempVal) ? tempVal : (int?)null;
        int? capacity = Int32.TryParse(req.Query["capacity"], out tempVal) ? tempVal : (int?)null;
    
        // Get request body
        string requestBody = await new StreamReader(req.Body).ReadToEndAsync();
        dynamic data = JsonConvert.DeserializeObject(requestBody);
    
        // Use request body if a query was not sent
        capacity = capacity ?? data?.capacity;
        hours = hours ?? data?.hours;
    
        // Return bad request if capacity or hours are not passed in
        if (capacity == null || hours == null){
            return new BadRequestObjectResult("Please pass capacity and hours on the query string or in the request body");
        }
        // Formulas to calculate revenue and cost
        double? revenueOpportunity = capacity * revenuePerkW * 24;  
        double? costToFix = (hours * technicianCost) +  turbineCost;
        string repairTurbine;
    
        if (revenueOpportunity > costToFix){
            repairTurbine = "Yes";
        }
        else {
            repairTurbine = "No";
        };
    
        return (ActionResult)new OkObjectResult(new{
            message = repairTurbine,
            revenueOpportunity = "$"+ revenueOpportunity,
            costToFix = "$"+ costToFix
        });
    }
    ```

    Bu işlev kodu, acil onarımın uygun maliyetli olup olmadığının yanı sıra türbinin temsil ettiği gelir fırsatını ve türbin onarımının maliyetini gösteren `Yes` veya `No` şeklinde bir ileti döndürür.

1. İşlevi test etmek için en sağdaki **Test** seçeneğine tıklayarak test sekmesini genişletin. **İstek gövdesi** için aşağıdaki değeri girip **Çalıştır**’a tıklayın.

    ```json
    {
    "hours": "6",
    "capacity": "2500"
    }
    ```

    ![İşlevi Azure portalında test etme](media/functions-openapi-definition/test-function.png)

    Yanıtın gövdesinde aşağıdaki değer döndürülür.

    ```json
    {"message":"Yes","revenueOpportunity":"$7200","costToFix":"$1600"}
    ```

Acil onarımların maliyet açısından uygunluğunu belirleyen bir işleviniz oldu. Ardından, işlev uygulaması için bir Openapı tanımı oluşturur.

## <a name="generate-the-openapi-definition"></a>OpenAPI tanımını oluşturma

Artık OpenAPI tanımını oluşturmaya hazırsınız.

1. İşlev uygulaması, ardından **Platform özellikleri**, seçin **API Management** seçip **Yeni Oluştur** altında **API Management**.

    ![API yönetimi platformu özellikleri seçin](media/functions-openapi-definition/select-all-settings-openapi.png)

1. Görüntünün altındaki tabloda belirtilen API yönetimi ayarlarını kullanın.

    ![Yeni API Management hizmeti oluşturma](media/functions-openapi-definition/new-apim-service-openapi.png)

    | Ayar      | Önerilen değer  | Açıklama                                        |
    | ------------ |  ------- | -------------------------------------------------- |
    | **Ad** | Genel olarak benzersiz bir ad | İşlev uygulamanızın adına dayalı bir ad oluşturulur. |
    | **Abonelik** | Aboneliğiniz | Bu yeni kaynak oluşturulduğu abonelik. |  
    | **[Kaynak Grubu](../azure-resource-manager/resource-group-overview.md)** |  myResourceGroup | Aynı kaynak için ayarlamalısınız, işlev uygulaması. |
    | **Konum** | Batı ABD | Batı ABD konumu seçin. |
    | **Kuruluş adı** | Contoso | Geliştirici portalında ve e-posta bildirimleri için kullanılan kuruluş adı. |
    | **Yönetici e-postası** | e-postanız | Sistem bildirimlerini API Yönetimi'nden alınan e-posta. |
    | **Fiyatlandırma katmanı** | Tüketim (önizleme) | Tüketim katmanı Önizleme aşamasındadır ve tüm bölgelerde kullanılamaz. Tüm fiyatlandırma ayrıntıları için bkz. [API Management'ın fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/api-management/) |

1. Seçin **Oluştur** birkaç dakika sürebilir API Yönetimi örneğini oluşturmak için.

1. Seçin **Application ınsights'ı etkinleştirme** aynı yerde işlevi uygulama günlüklerini göndermek için daha sonra kalan varsayılan değerleri ve seçim kabul **bağlantısı API'sini**.

1. **Alma Azure işlevleri** açılır **TurbineRepair** vurgulanmış işlevi. Seçin **seçin** devam etmek için.

    ![API yönetimi ile Azure işlevleri içeri aktarma](media/functions-openapi-definition/import-function-openapi.png)

1. İçinde **işlev uygulamasından Oluştur** sayfasında, Varsayılanları kabul edin ve seçin **oluştur**

    ![İşlev Uygulamasından oluştur](media/functions-openapi-definition/create-function-openapi.png)

API, artık işlev için oluşturulur.

## <a name="test-the-openapi-definition"></a>OpenAPI tanımını test etme

API tanımı kullanmadan önce çalışır durumda olduğunu doğrulamanız gerekir.

1. Üzerinde **Test** sekmesini seçin, işlevin **POST** işlemi

1. İçin değerler girin **saat** ve **kapasite**

    ```json
    {
    "hours": "6",
    "capacity": "2500"
    }
    ```

1. Tıklayın **Gönder**, ardından HTTP yanıtı görüntüleyin.

    ![API'yi test et işlevi](media/functions-openapi-definition/test-function-api-openapi.png)

[!INCLUDE [clean-up-section-portal](../../includes/clean-up-section-portal.md)]

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [API yönetimi hakkında daha fazla bilgi edinin](../api-management/api-management-key-concepts.md)
