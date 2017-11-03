---
title: "Bir işlev için bir OpenAPI tanımı oluşturun | Microsoft Docs"
description: "Diğer uygulamalar ve Azure içinde bir işlevi çağırmak hizmetleri sağlayan bir OpenAPI tanımı oluşturun."
services: functions
keywords: "OpenAPI, Swagger, bulut uygulamaları, bulut Hizmetleri,"
documentationcenter: 
author: mgblythe
manager: cfowler
editor: 
ms.assetid: 
ms.service: functions
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 08/25/2017
ms.author: mblythe; glenga
ms.custom: mvc
ms.openlocfilehash: a196df5b4ab47b234b48594da45cd4d72f604086
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="create-an-openapi-definition-for-a-function"></a>Bir işlev için bir OpenAPI tanımı oluştur
REST API'leri, OpenAPI tanımı kullanılarak genellikle açıklanmıştır (önceki adıyla bilinen bir [Swagger](http://swagger.io/) dosyası). Bu tanım hangi işlemlerin bir API kullanılabilir olduğunu ve API istek ve yanıt verilerini nasıl yapılandırılacağını hakkında bilgi içerir.

Bu öğreticide, bir Acil Onarım Rüzgar Türbin üzerinde uygun maliyetli olup olmadığını belirleyen bir işlev oluşturun. Böylece işlevi diğer uygulama ve hizmetlere çağrılabilir ardından işlev uygulaması için bir OpenAPI tanımı oluşturun.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Bir işlev oluşturma
> * OpenAPI Araçları'nı kullanarak bir OpenAPI tanımı oluştur
> * Ek meta verilerini sağlamak için tanımını değiştirin
> * İşlevini çağırarak tanımı test

## <a name="create-a-function-app"></a>İşlev uygulaması oluşturma

İşlevlerinizin yürütülmesini barındıran bir işlev uygulamasına sahip olmanız gerekir. İşlevler ölçekleme ve kaynak paylaşımı, bir mantıksal birim olarak daha kolay yönetim için dağıtım, Grup işlevi uygulama sağlar. 

[!INCLUDE [Create function app Azure portal](../../includes/functions-create-function-app-portal.md)]

[!INCLUDE [functions-portal-favorite-function-apps](../../includes/functions-portal-favorite-function-apps.md)]


## <a name="create-the-function"></a>İşlevi oluşturma

Bu öğretici iki parametre isteyen bir HTTP tetiklenen işlevi kullanır: (saat); onarma türbin yapmak için gereken tahmini süre ve (içinde kilowatt) Türbin kapasitesi. İşlev sonra ne kadar onarım maliyetlidir, hesaplar ve ne kadar gelir türbin yapmak 24 saatlik süre içinde.

1. İşlev uygulamanızı genişletin, tıklayın  **+**  düğmesine **işlevleri**, tıklatın **HTTPTrigger** şablonu. Girin `TurbineRepair` işlevi için **adı** tıklatıp **oluşturma**.

    ![İşlev uygulamalar dikey penceresinde, İşlevler +](media/functions-openapi-definition/add-function.png)

1. Run.csx dosyasının içeriğini aşağıdaki kodla değiştirin ve ardından **kaydetmek**:

    ```c#
    using System.Net;

    const double revenuePerkW = 0.12; 
    const double technicianCost = 250; 
    const double turbineCost = 100;

    public static async Task<HttpResponseMessage> Run(HttpRequestMessage req, TraceWriter log)
    {   

        //Get request body
        dynamic data = await req.Content.ReadAsAsync<object>();
        int hours = data.hours;
        int capacity = data.capacity;

        //Formulas to calculate revenue and cost
        double revenueOpportunity = capacity * revenuePerkW * 24;  
        double costToFix = (hours * technicianCost) +  turbineCost;
        string repairTurbine;

        if (revenueOpportunity > costToFix){
            repairTurbine = "Yes";
        }
        else {
            repairTurbine = "No";
        }

        return req.CreateResponse(HttpStatusCode.OK, new{
            message = repairTurbine,
            revenueOpportunity = "$"+ revenueOpportunity,
            costToFix = "$"+ costToFix         
        }); 
    }
    ```
    Bu işlev kodu bir ileti döndürür `Yes` veya `No` Acil Durum Onarım uygun maliyetli, olup yanı sıra Türbin temsil eden gelir fırsatı ve maliyet Türbin düzeltmek için belirtmek için. 

1. İşlevi test etmek için tıklatın **Test** test sekmesi genişletmek sağ uçta. Aşağıdaki değerini girmeniz **istek gövdesinde**ve ardından **çalıştırmak**.

    ```json
    {
    "hours": "6",
    "capacity": "2500"
    }
    ```

    ![Azure portalında işlevi test etme](media/functions-openapi-definition/test-function.png)

    Aşağıdaki değeri yanıt gövdesi döndürülür.

    ```json
    {"message":"Yes","revenueOpportunity":"$7200","costToFix":"$1600"}
    ```

Artık Acil Durum Onarım düşük belirleyen işlev var. Ardından, oluşturmak ve işlev uygulaması için bir OpenAPI tanımı değiştirin.

## <a name="generate-the-openapi-definition"></a>OpenAPI tanımı oluştur

Artık OpenAPI tanımı oluşturmak hazırsınız. Bu tanım API uygulamaları gibi diğer Microsoft teknolojileri tarafından kullanılan [PowerApps](functions-powerapps-scenario.md) ve [Microsoft Flow](../azure-functions/app-service-export-api-to-powerapps-and-flow.md), iyi gibi üçüncü taraf geliştirici araçları dilediğiniz şekilde [Postman](https://www.getpostman.com/docs/importing_swagger) ve [pek çok daha fazla paket](http://swagger.io/tools/).

1. Yalnızca seçin *fiiller* API'nizi (Bu örnek POSTASINA) destekler. Bu, oluşturulan API tanımı temizleyici sağlar.

    1. Üzerinde **tümleştir** yeni HTTP tetikleyicisini işlevinizi değişiklik sekmesinde **izin verilen HTTP yöntemleri** için **seçili yöntemleri**

    1. İçinde **seçili HTTP yöntemleri**, dışındaki her bir seçeneğin işaretini **POST**.

        ![Seçili HTTP yöntemleri](media/functions-openapi-definition/selected-http-methods.png)
        
1. İşlev uygulaması adınıza tıklayın (gibi **işlevi demo enerji**) > **Platform özellikleri** > **API tanımı**.

    ![API tanımı](media/functions-openapi-definition/api-definition.png)

1. Üzerinde **API tanımı** sekmesini tıklatın, **işlevi**.

    ![API tanımı kaynak](media/functions-openapi-definition/api-definition-source.png)

    Bu adım bir paketi işlevi uygulamanızın etki alanı, bir satır içi kopyasını OpenAPI dosyasından barındırmak için bir uç nokta da dahil olmak üzere işlevi uygulamanız için OpenAPI seçeneklerini etkinleştirir [OpenAPI Düzenleyicisi](http://editor.swagger.io)ve bir API tanımı şablon oluşturucu.

1. Tıklatın **oluşturmak API tanımı şablonu** > **kaydetmek**.

    ![API tanımı şablonu oluştur](media/functions-openapi-definition/generate-template.png)

    Azure işlevi uygulamanız HTTP tetikleyicisini işlevleri için tarar ve OpenAPI tanımı oluşturmak için functions.json bilgilerinizi kullanır. Oluşturulan tanımı aşağıda verilmiştir:

    ```yaml
    swagger: '2.0'
    info:
    title: function-demo-energy.azurewebsites.net
    version: 1.0.0
    host: function-demo-energy.azurewebsites.net
    basePath: /
    schemes:
    - https
    - http
    paths:
    /api/TurbineRepair:
        post:
        operationId: /api/TurbineRepair/post
        produces: []
        consumes: []
        parameters: []
        description: >-
            Replace with Operation Object
            #http://swagger.io/specification/#operationObject
        responses:
            '200':
            description: Success operation
        security:
            - apikeyQuery: []
    definitions: {}
    securityDefinitions:
    apikeyQuery:
        type: apiKey
        name: code
        in: query
    ```

    Bu tanım olarak tanımlanan bir _şablonu_ , tam bir OpenAPI tanımı olması için daha fazla meta veri gerektiriyor. Sonraki adımda tanımı değiştireceksiniz.

## <a name="modify-the-openapi-definition"></a>OpenAPI tanımını değiştirin
Bir şablon tanımı sahip olduğunuza göre API işlemleri ve veri yapıları hakkında ek meta veriler sağlayacak şekilde değiştirin. İçinde **API tanımı**, oluşturulan tanımından Sil `post` tanımı altına, içeriği yapıştırın ve tıklatın **kaydetmek**.

```yaml
    post:
      operationId: CalculateCosts
      description: Determines if a technician should be sent for repair
      summary: Calculates costs
      x-ms-summary: Calculates costs
      x-ms-visibility: important
      produces:
        - application/json
      consumes:
        - application/json
      parameters:
        - name: body
          in: body
          description: Hours and capacity used to calculate costs
          x-ms-summary: Hours and capacity
          x-ms-visibility: important
          required: true
          schema:
            type: object
            properties:
              hours:
                description: The amount of effort in hours required to conduct repair
                type: number
                x-ms-summary: Hours
                x-ms-visibility: important
              capacity:
                description: The max output of a turbine in kilowatts
                type: number
                x-ms-summary: Capacity
                x-ms-visibility: important
      responses:
        200:
          description: Message with cost and revenue numbers
          x-ms-summary: Message
          schema:
           type: object
           properties:
            message:
              type: string
              description: Returns Yes or No depending on calculations
              x-ms-summary: Message 
            revenueOpportunity:
              type: string
              description: The revenue opportunity cost
              x-ms-summary: RevenueOpportunity 
            costToFix:
              type: string
              description: The cost in $ to fix the turbine
              x-ms-summary: CostToFix
      security:
        - apikeyQuery: []
definitions: {}
securityDefinitions:
  apikeyQuery:
    type: apiKey
    name: code
    in: query
```

Bu durumda yalnızca güncelleştirilmiş meta verilerde yapıştırın, ancak biz varsayılan şablona yapılan değişiklikler türlerini anlamak önemlidir:

+ Belirtilen API üretir ve verileri JSON biçiminde kullanır.

+ Gerekli parametre adları ve veri türleriyle belirtildi.

+ Başarılı bir yanıt için dönüş değerlerini kendi adları ve veri türleriyle belirtildi.

+ Kolay özetler ve açıklamaları API ve işlemleri ve parametreleri için sağlanan. Bu, bu işlev kullanan kişiler için önemlidir.

+ Kullanıcı Arabiriminde Microsoft Flow ve Logic Apps için kullanılan x-ms-Özet ve x-ms-görünürlük eklendi. Daha fazla bilgi için bkz: [Microsoft Flow özel API'leri için OpenAPI uzantıları](https://preview.flow.microsoft.com/documentation/customapi-how-to-swagger/).

> [!NOTE]
> Biz güvenlik tanımı API anahtarı varsayılan kimlik doğrulama yöntemiyle kalmadı. Farklı türde bir kimlik doğrulaması kullanılırsa bu bölümde tanımının değiştirirsiniz.

API işlemleri tanımlama hakkında daha fazla bilgi için bkz: [açık API belirtimine](https://swagger.io/specification/#operationObject).

## <a name="test-the-openapi-definition"></a>OpenAPI tanımı test
API tanımı kullanmadan önce Azure işlevleri Arabiriminde test etmek için iyi bir fikirdir.

1. Üzerinde **Yönet** işlevinizi sekmesi altında **ana bilgisayar anahtarları**, kopya **varsayılan** anahtarı.

    ![API anahtarını kopyalayın](media/functions-openapi-definition/copy-api-key.png)

    > [!NOTE]
    >Test etmek için bu anahtarı kullanın ve bir uygulama veya hizmet API çağrısı de kullanılır.

1. API tanımı geri gidin: **işlevi demo enerji** > **Platform özellikleri** > **API tanımı**.

1. Sağ bölmede **kimlik doğrulamayı Değiştir**, kopyalanır ve tıklatın API anahtarını girin **kimlik doğrulama**.

    ![API anahtarı ile kimlik doğrulaması](media/functions-openapi-definition/authenticate-api-key.png)

1. Aşağı kaydırın ve tıklatın **bu işlemi deneyin**.

    ![Bu işlemi deneyin](media/functions-openapi-definition/try-operation.png)

1. İçin değerler girin **saatleri** ve **kapasite**.

    ![Parametreleri girin](media/functions-openapi-definition/parameters.png)

    Kullanıcı arabirimini API tanımından açıklamaları nasıl kullandığını dikkat edin.

1. Tıklatın **bir istek göndermesini**, ardından **oldukça** çıkışı görmek için sekme.

    ![Bir isteği gönder](media/functions-openapi-definition/send-request.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Bir işlev oluşturma
> * OpenAPI Araçları'nı kullanarak bir OpenAPI tanımı oluştur
> * Ek meta verilerini sağlamak için tanımını değiştirin
> * İşlevini çağırarak tanımı test

Oluşturduğunuz OpenAPI tanımını kullanan bir PowerApps uygulamasının nasıl oluşturulacağını öğrenmek için sonraki konu ilerleyin.
> [!div class="nextstepaction"]
> [PowerApps bir işlev çağrısı](functions-powerapps-scenario.md)
