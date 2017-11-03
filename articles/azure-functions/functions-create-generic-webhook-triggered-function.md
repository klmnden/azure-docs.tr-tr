---
title: "Azure genel bir Web kancası tarafından tetiklenen bir işlev oluşturun | Microsoft Docs"
description: "Azure işlevleri, bir Web kancası Azure tarafından çağrılan sunucusuz bir işlev oluşturmak için kullanın."
services: functions
documentationcenter: na
author: ggailey777
manager: cfowler
editor: 
tags: 
ms.assetid: fafc10c0-84da-4404-b4fa-eea03c7bf2b1
ms.service: functions
ms.devlang: multiple
ms.topic: quickstart
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 08/12/2017
ms.author: glenga
ms.custom: mvc
ms.openlocfilehash: f283f8d79c5ae5fb6a72c84c9e9edb7bb8de4a83
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="create-a-function-triggered-by-a-generic-webhook"></a>Genel bir Web kancası tarafından tetiklenen bir işlev oluşturun

Azure İşlevleri, öncelikle bir VM oluşturmak veya bir web uygulaması yayımlamak zorunda kalmadan kodunuzu sunucusuz bir ortamda yürütmenize olanak tanır. Örneğin, Azure İzleyici tarafından verilen bir uyarı tarafından tetiklenen bir işlev yapılandırabilirsiniz. Bu konu bir kaynak grubu aboneliğinize eklendiğinde, C# kodunun nasıl çalıştırılacağını gösterir.   

![Azure portalında işlevi genel Web kancası tetiklendi](./media/functions-create-generic-webhook-triggered-function/function-completed.png)

## <a name="prerequisites"></a>Ön koşullar 

Bu öğreticiyi tamamlamak için:

+ Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

[!INCLUDE [functions-portal-favorite-function-apps](../../includes/functions-portal-favorite-function-apps.md)]

## <a name="create-an-azure-function-app"></a>Azure İşlev uygulaması oluşturma

[!INCLUDE [Create function app Azure portal](../../includes/functions-create-function-app-portal.md)]

Ardından, yeni işlev uygulamasında bir işlev oluşturun.

## <a name="create-function"></a>Genel Web kancası tetiklenen bir işlev oluşturun

1. İşlev uygulamanızı genişletin ve **İşlevler**'in yanındaki **+** düğmesine tıklayın. Bu işlev listedeki birinci işlevi uygulamanız gerekiyorsa seçin **özel işlevi**. Böylece işlev şablonlarının tamamı görüntülenir.

    ![Azure portalındaki İşlevler hızlı başlangıç sayfası](./media/functions-create-generic-webhook-triggered-function/add-first-function.png)

2. Seçin **Genel Web kancası - C#** şablonu. C# işlevi için bir ad yazın ve ardından **oluşturma**.

     ![Azure portalında tetiklenen Genel Web kancası işlevi oluşturma](./media/functions-create-generic-webhook-triggered-function/functions-create-generic-webhook-trigger.png) 

2. Yeni işlevinizi tıklatın **<> / Get işlevi URL**, daha sonra kopyalayın ve değeri kaydedin. Web kancası yapılandırmak için bu değeri kullanın. 

    ![İşlev kodunu gözden geçirme](./media/functions-create-generic-webhook-triggered-function/functions-copy-function-url.png)
         
Ardından, bir etkinlik günlüğü uyarı Azure İzleyicisi'nde bir Web kancası uç noktası oluşturun. 

## <a name="create-an-activity-log-alert"></a>Bir etkinlik günlüğü uyarı oluşturabilir.

1. Azure portalında gidin **İzleyici** hizmeti, select **uyarıları**, tıklatıp **etkinlik günlüğü uyarı Ekle**.   

    ![İzleme](./media/functions-create-generic-webhook-triggered-function/functions-monitor-add-alert.png)

2. Tabloda belirtilen ayarları kullanın:

    ![Bir etkinlik günlüğü uyarı oluşturabilir.](./media/functions-create-generic-webhook-triggered-function/functions-monitor-add-alert-settings.png)

    | Ayar      |  Önerilen değer   | Açıklama                              |
    | ------------ |  ------- | -------------------------------------------------- |
    | **Etkinlik günlüğü uyarı adı** | kaynak-grubu-oluştur-uyarı | Etkinlik günlüğü uyarı adı. |
    | **Abonelik** | Aboneliğiniz | Bu öğretici için kullanmakta olduğunuz abonelik. | 
    |  **Kaynak Grubu** | myResourceGroup | Uyarı kaynakları dağıtılan kaynak grubu. Aynı kaynak grubunda işlevi kullanarak öğreticiyi tamamladıktan sonra temizlemek kolaylaştırır. |
    | **Olay kategorisi** | Yönetim | Bu kategori Azure kaynaklarında yapılan değişiklikleri içerir.  |
    | **Kaynak türü** | Kaynak grupları | Kaynak grubu etkinlikler için uyarı filtreleri. |
    | **Kaynak Grubu**<br/>ve **kaynak** | Tümü | Tüm kaynaklar izleyin. |
    | **İşlem adı** | Kaynak Grubu oluşturma | İşlemler oluşturmak için uyarıları filtreler. |
    | **Düzeyi** | Bilgilendirme | Bilgilendirici düzeyi uyarılar içerir. | 
    | **Durumu** | Başarılı oldu | Uyarılar başarıyla tamamladınız eylemlerine filtreler. |
    | **Eylem grubu** | Yeni | Bir uyarı verildiğinde hangi eylemini alır tanımlayan yeni bir eylem grubu oluşturun. |
    | **Eylem grup adı** | Web kancası işlevi | Eylem grubunu tanımlamak için bir ad.  | 
    | **Kısa ad** | funcwebhook | Eylem grubu için kısa bir ad. |  

3. İçinde **Eylemler**, tabloda belirtildiği gibi ayarları kullanarak bir eylem ekleyin: 

    ![Bir eylem grubu Ekle](./media/functions-create-generic-webhook-triggered-function/functions-monitor-add-alert-settings-2.png)

    | Ayar      |  Önerilen değer   | Açıklama                              |
    | ------------ |  ------- | -------------------------------------------------- |
    | **Ad** | CallFunctionWebhook | Eylem için bir ad. |
    | **Eylem türü** | Web Kancası | Uyarı yanıta bir Web kancası URL'si olarak adlandırılmasıdır. |
    | **Ayrıntılar** | İşlev URL'si | Daha önce kopyaladığınız işlevi Web kancası URL'sini yapıştırın. |v

4. Tıklatın **Tamam** uyarı ve eylem grubu oluşturulamıyor.  

Bir kaynak grubu, aboneliğinizde oluşturulduğunda Web kancası şimdi adı verilir. Ardından, istek gövdesinde JSON günlük verileri işlemek için işlev kodunu güncelleştirin.   

## <a name="update-the-function-code"></a>İşlev kodunu güncelleştirme

1. Portal işlevi uygulamanızda geri gidin ve işlevinizi genişletin. 

2. Portalda işlevindeki C# betik kodu aşağıdaki kodla değiştirin:

    ```csharp
    #r "Newtonsoft.Json"
    
    using System;
    using System.Net;
    using Newtonsoft.Json;
    using Newtonsoft.Json.Linq;
    
    public static async Task<object> Run(HttpRequestMessage req, TraceWriter log)
    {
        log.Info($"Webhook was triggered!");
    
        // Get the activityLog object from the JSON in the message body.
        string jsonContent = await req.Content.ReadAsStringAsync();
        JToken activityLog = JObject.Parse(jsonContent.ToString())
            .SelectToken("data.context.activityLog");
    
        // Return an error if the resource in the activity log isn't a resource group. 
        if (activityLog == null || !string.Equals((string)activityLog["resourceType"], 
            "Microsoft.Resources/subscriptions/resourcegroups"))
        {
            log.Error("An error occured");
            return req.CreateResponse(HttpStatusCode.BadRequest, new
            {
                error = "Unexpected message payload or wrong alert received."
            });
        }
    
        // Write information about the created resource group to the streaming log.
        log.Info(string.Format("Resource group '{0}' was {1} on {2}.",
            (string)activityLog["resourceGroupName"],
            ((string)activityLog["subStatus"]).ToLower(), 
            (DateTime)activityLog["submissionTimestamp"]));
    
        return req.CreateResponse(HttpStatusCode.OK);    
    }
    ```

Şimdi, aboneliğinizde yeni bir kaynak grubu oluşturarak işlevi test edebilirsiniz.

## <a name="test-the-function"></a>İşlevi test etme

1. Kaynak grubu seçin Azure portalının sol simgesini **+ Ekle**, bir **kaynak grubu adı**ve seçin **oluşturma** boş bir kaynak grubu oluşturmak için.
    
    ![Bir kaynak grubu oluşturun.](./media/functions-create-generic-webhook-triggered-function/functions-create-resource-group.png)

2. İşlevinizi için geri dönün ve genişletin **günlükleri** penceresi. Kaynak grubu oluşturulduktan sonra Web kancası etkinlik günlüğü uyarıyı tetikleyen ve işlev yürütür. Günlüklere yazılan yeni kaynak grubu adını görürsünüz.  

    ![Bir test uygulama ayarı ekleyin.](./media/functions-create-generic-webhook-triggered-function/function-view-logs.png)

3. (İsteğe bağlı) Geri dönün ve sizin oluşturduğunuz kaynak grubunu silebilirsiniz. Bu etkinlik Tetik işlevi değil unutmayın. Bunun nedeni, operations filtrelenir uyarı tarafından silin. 

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [Next steps note](../../includes/functions-quickstart-cleanup.md)]

## <a name="next-steps"></a>Sonraki adımlar

Genel bir Web kancası bir istek alındığında çalıştırılan bir işlev oluşturdunuz. 

[!INCLUDE [Next steps note](../../includes/functions-quickstart-next-steps.md)]

Web kancası bağlamaları hakkında daha fazla bilgi için bkz. [Azure İşlevleri HTTP ve web kancası bağlamaları](functions-bindings-http-webhook.md). C# işlevleri geliştirme hakkında daha fazla bilgi için bkz: [Azure işlevleri C# betik Geliştirici Başvurusu](functions-reference-csharp.md).

