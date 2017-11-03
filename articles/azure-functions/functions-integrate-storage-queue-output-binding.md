---
title: "İşlevler kullanılarak bir Azure depolama kuyruğu iletileri ekleme | Microsoft Docs"
description: "Bir Azure Depolama kuyruğuna gönderilmiş iletiler tarafından çağrılan sunucusuz işlev oluşturmak için Azure İşlevlerini kullanın."
services: azure-functions
documentationcenter: na
author: ggailey777
manager: cfowler
editor: 
tags: 
ms.assetid: 0b609bc0-c264-4092-8e3e-0784dcc23b5d
ms.service: functions
ms.devlang: multiple
ms.topic: quickstart
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 09/19/2017
ms.author: glenga
ms.custom: mvc
ms.openlocfilehash: 822879861ee8189cdd413f0061f26fb91819d88d
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="add-messages-to-an-azure-storage-queue-using-functions"></a>İşlevleri kullanarak bir Azure Depolama kuyruğuna ileti ekleme

Azure İşlevleri’nde giriş ve çıkış bağlamaları, işlevinizden dış hizmet verilerine bağlanmanın bildirim temelli bir yöntemini sağlar. Bu konu başlığında, Azure Kuyruk Depolama’ya iletiler gönderen bir çıkış bağlaması ekleyerek mevcut bir işlevi güncelleştirmeyi öğrenebilirsiniz.  

![Günlüklerde iletiyi görüntüleyin.](./media/functions-integrate-storage-queue-output-binding/functions-integrate-storage-binding-in-portal.png)

## <a name="prerequisites"></a>Ön koşullar 

[!INCLUDE [Previous topics](../../includes/functions-quickstart-previous-topics.md)]

* [Microsoft Azure Depolama Gezgini](http://storageexplorer.com/)'ni yükleyin.

## <a name="add-binding"></a>Çıkış bağlaması ekleme
 
1. İşlev uygulamanızı ve işlevinizi genişletin.

2. Seçin **tümleştir** ve **+ yeni çıktı**, ardından **Azure kuyruk depolama** ve **seçin**.
    
    ![Azure portalındaki bir işleve Kuyruk depolama çıkış bağlaması ekleyin.](./media/functions-integrate-storage-queue-output-binding/function-add-queue-storage-output-binding.png)

3. Tabloda belirtilen ayarları kullanın: 

    ![Azure portalındaki bir işleve Kuyruk depolama çıkış bağlaması ekleyin.](./media/functions-integrate-storage-queue-output-binding/function-add-queue-storage-output-binding-2.png)

    | Ayar      |  Önerilen değer   | Açıklama                              |
    | ------------ |  ------- | -------------------------------------------------- |
    | **Kuyruk adı**   | myqueue-items    | Depolama hesabınızdaki bağlantı kurulacak kuyruğun adı. |
    | **Depolama hesabı bağlantısı** | AzureWebJobStorage | İşlev uygulamanız tarafından kullanılmakta olan depolama hesabı bağlantısını kullanabilir veya yeni bir bağlantı oluşturabilirsiniz.  |
    | **İleti parametre adı** | outputQueueItem | Çıkış bağlama parametresinin adı. | 

4. Bağlamayı kaydetmek için **Kaydet**’e tıklayın.
 
Bir çıkış bağlaması tanımladığınıza göre, bir kuyruğa ileti eklemek üzere bağlamayı kullanmak için kodu güncelleştirmeniz gerekir.  

## <a name="update-the-function-code"></a>İşlev kodunu güncelleştirme

1. İşlev kodunu düzenleyicide görüntülemek için işlevinizi seçin. 

2. C#, bir işlev için aşağıdaki gibi eklemek için işlev tanımının güncelleştirme **outputQueueItem** depolama bağlama parametresi. JavaScript işlevi için bu adımı atlayın.

    ```cs   
    public static async Task<HttpResponseMessage> Run(HttpRequestMessage req, 
        ICollector<string> outputQueueItem, TraceWriter log)
    {
        ....
    }
    ```

3. Yöntemden dönülmeden hemen önce aşağıdaki kodu işleve ekleyin. İşlevinizin diline uygun parçacığı kullanın.

    ```javascript
    context.bindings.outputQueueItem = "Name passed to the function: " + 
                (req.query.name || req.body.name);
    ```

    ```cs
    outputQueueItem.Add("Name passed to the function: " + name);     
    ```

4. Değişiklikleri kaydetmek için **Kaydet**'i seçin.

HTTP tetikleyicisine geçirilen değer, kuyruğa alınmış bir iletiye eklenir.
 
## <a name="test-the-function"></a>İşlevi test etme 

1. Kod değişiklikleri kaydedildikten sonra **Çalıştır**'ı seçin. 

    ![Azure portalındaki bir işleve Kuyruk depolama çıkış bağlaması ekleyin.](./media/functions-integrate-storage-queue-output-binding/functions-test-run-function.png)

2. İşlevin başarılı olduğundan emin olmak için günlükleri denetleyin. Çıkış bağlaması ilk kez kullanıldığında, Depolama hesabınızda İşlevler çalışma zamanı tarafından **outqueue** adlı yeni bir kuyruk oluşturulur.

Bundan sonra, yeni kuyruğunuzu ve ona eklediğiniz iletiyi doğrulamak için depolama hesabınıza bağlanabilirsiniz. 

## <a name="connect-to-the-queue"></a>Kuyruğa bağlanma

Depolama Gezgini’ni daha önce yükleyip depolama hesabınıza bağladıysanız, ilk üç adımı atlayın.    

1. İşlevinde seçin **tümleştir** ve yeni **Azure kuyruk depolama** bağlama çıktı sonra genişletin **belgelerine**. Hem **Hesap adı** hem de **Hesap anahtarı** değerlerini kopyalayın. Depolama hesabına bağlanmak için bu kimlik bilgilerini kullanacaksınız.
 
    ![Depolama hesabı bağlantısı için kimlik bilgilerini alın.](./media/functions-integrate-storage-queue-output-binding/function-get-storage-account-credentials.png)

2. [Microsoft Azure Depolama Gezgini](http://storageexplorer.com/) aracını çalıştırın, sol taraftaki bağlanma simgesini seçin, **Depolama hesabı adı ve anahtarı kullan**'ı seçip **İleri**'ye tıklayın.

    ![Depolama Hesabı Gezgini aracını çalıştırın.](./media/functions-integrate-storage-queue-output-binding/functions-storage-manager-connect-1.png)
    
3. 1 adımdaki **Hesap adını** ve **Hesap anahtarını** uygun alanlara yapıştırıp **İleri** ve **Bağlan** seçeneklerini belirleyin. 
  
    ![Depolama kimlik bilgilerini yapıştırın ve bağlanın.](./media/functions-integrate-storage-queue-output-binding/functions-storage-manager-connect-2.png)

4. Bağlı depolama hesabı genişletin, **sıraları** ve bir sıraya adlı doğrulayın **Sıram öğeleri** bulunmaktadır. Ayrıca zaten kuyrukta olan bir ileti görmeniz gerekir.  
 
    ![Depolama kuyruğu oluşturun.](./media/functions-integrate-storage-queue-output-binding/function-queue-storage-output-view-queue.png)
 

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [Next steps note](../../includes/functions-quickstart-cleanup.md)]

## <a name="next-steps"></a>Sonraki adımlar

Var olan bir işleve çıkış bağlaması eklediniz. 

[!INCLUDE [Next steps note](../../includes/functions-quickstart-next-steps.md)]

Kuyruk depolamaya bağlama hakkında daha fazla bilgi için bkz. [Azure İşlevleri Depolama kuyruğu bağlamaları](functions-bindings-storage-queue.md). 



