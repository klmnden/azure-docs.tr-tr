---
title: "Azure&quot;da kuyruk iletileri tarafından tetiklenen bir işlev oluşturma | Microsoft Docs"
description: "Bir Azure Depolama kuyruğuna gönderilmiş iletiler tarafından çağrılan sunucusuz işlev oluşturmak için Azure İşlevlerini kullanın."
services: azure-functions
documentationcenter: na
author: ggailey777
manager: erikre
editor: 
tags: 
ms.assetid: 0b609bc0-c264-4092-8e3e-0784dcc23b5d
ms.service: functions
ms.devlang: multiple
ms.topic: get-started-article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 05/02/2017
ms.author: glenga
ms.translationtype: Human Translation
ms.sourcegitcommit: 71fea4a41b2e3a60f2f610609a14372e678b7ec4
ms.openlocfilehash: 0e2501b0eb218d3c8a62dd4959b08ff85ec565eb
ms.contentlocale: tr-tr
ms.lasthandoff: 05/10/2017


---
# <a name="add-messages-to-an-azure-storage-queue-using-functions"></a>İşlevleri kullanarak bir Azure Depolama kuyruğuna ileti ekleme

Azure İşlevleri’nde giriş ve çıkış bağlamaları, işlevinizden dış hizmet verilerine bağlanmanın bildirim temelli bir yöntemini sağlar. Bu konu başlığında, Azure Kuyruk Depolama’ya iletiler gönderen bir çıkış bağlaması ekleyerek mevcut bir işlevi güncelleştirmeyi öğrenebilirsiniz.  

![Günlüklerde iletiyi görüntüleyin.](./media/functions-integrate-storage-queue-output-binding/functions-integrate-storage-binding-in-portal.png)

Bu konu başlığı altındaki adımların tümünü beş dakikadan kısa bir sürede tamamlamalısınız.

## <a name="prerequisites"></a>Ön koşullar 

[!INCLUDE [Previous topics](../../includes/functions-quickstart-previous-topics.md)]

Ayrıca [Microsoft Azure Depolama Gezgini](http://storageexplorer.com/)'ni de indirip yüklemeniz gerekir. 

[!INCLUDE [functions-portal-favorite-function-apps](../../includes/functions-portal-favorite-function-apps.md)] 

## <a name="add-binding"></a>Çıkış bağlaması ekleme
 
1. İşlev uygulamanızı ve işlevinizi genişletin.

2. **Tümleştir** ve **+ Yeni çıkış**’a, ardından **Azure Kuyruk depolama** ve **Seç**’e tıklayın.
    
    ![Azure portalındaki bir işleve Kuyruk depolama çıkış bağlaması ekleyin.](./media/functions-integrate-storage-queue-output-binding/function-add-queue-storage-output-binding.png)

3. Tabloda belirtilen ayarları kullanın ve **Kaydet**'e tıklayın: 

    ![Azure portalındaki bir işleve Kuyruk depolama çıkış bağlaması ekleyin.](./media/functions-integrate-storage-queue-output-binding/function-add-queue-storage-output-binding-2.png)

    | Ayar      |  Önerilen değer   | Açıklama                              |
    | ------------ |  ------- | -------------------------------------------------- |
    | **Kuyruk adı**   | myqueue-items    | Depolama hesabınızdaki bağlantı kurulacak kuyruğun adı. |
    | **Depolama hesabı bağlantısı** | AzureWebJobStorage | İşlev uygulamanız tarafından kullanılmakta olan depolama hesabı bağlantısını kullanabilir veya yeni bir bağlantı oluşturabilirsiniz.  |
    | **İleti parametre adı** | outQueueItem | Çıkış bağlama parametresinin adı. | 

Bir çıkış bağlaması tanımladığınıza göre, bir kuyruğa ileti eklemek üzere bağlamayı kullanmak için kodu güncelleştirmeniz gerekir.  

## <a name="update-the-function-code"></a>İşlev kodunu güncelleştirme

1. İşlev kodunu düzenleyicide görüntülemek için işlevinize tıklayın. 

2. Bir C# işlevi için, **outQueueItem** depolama bağlama parametresini eklemek üzere işlev tanımınızı aşağıdaki gibi güncelleştirin. JavaScript işlevi için bu adımı atlayın.

    ```cs   
    public static async Task<HttpResponseMessage> Run(HttpRequestMessage req, 
        ICollector<string> outQueueItem, TraceWriter log)
    {
        ....
    }
    ```

3. Yöntem döndürülmeden hemen önce aşağıdaki kodu işleve ekleyin. İşlevinizin diline uygun parçacığı kullanın.

    ```javascript
    context.bindings.outQueueItem = "Name passed to the function: " + 
                (req.query.name || req.body.name);
    ```

    ```cs
    outQueueItem.Add("Name passed to the function: " + name);     
    ```

4. Değişiklikleri kaydetmek için **Kaydet**’e tıklayın.

HTTP tetikleyicisine geçirilen değer, kuyruğa alınmış bir iletiye eklenir.
 
## <a name="test-the-function"></a>İşlevi test etme 

1. Kod değişiklikleri kaydedildikten sonra **Çalıştır**’a tıklayın. 

    ![Azure portalındaki bir işleve Kuyruk depolama çıkış bağlaması ekleyin.](./media/functions-integrate-storage-queue-output-binding/functions-test-run-function.png)

2. İşlevin başarılı olduğundan emin olmak için günlükleri denetleyin. Çıkış bağlaması ilk kez kullanıldığında, Depolama hesabınızda İşlevler çalışma zamanı tarafından **outqueue** adlı yeni bir kuyruk oluşturulur.

Bundan sonra, yeni kuyruğunuzu ve ona eklediğiniz iletiyi doğrulamak için depolama hesabınıza bağlanabilirsiniz. 

## <a name="connect-to-the-queue"></a>Kuyruğa bağlanma

Depolama Gezgini’ni daha önce yükleyip depolama hesabınıza bağladıysanız, ilk üç adımı atlayın.    

1. İşlevinizde **Tümleştir**’e ve yeni **Azure Kuyruk depolama** çıkış bağlamasına tıklayın, ardından **Belgeler**’i genişletin. Hem **Hesap adı** hem de **Hesap anahtarı** değerlerini kopyalayın. Depolama hesabına bağlanmak için bu kimlik bilgilerini kullanacaksınız.
 
    ![Depolama hesabı bağlantısı için kimlik bilgilerini alın.](./media/functions-integrate-storage-queue-output-binding/function-get-storage-account-credentials.png)

2. [Microsoft Azure Depolama Gezgini](http://storageexplorer.com/) aracını çalıştırın, sol taraftaki bağlan simgesine tıklayın, **Depolama hesabı adı ve anahtarı kullan**'ı seçin ve **İleri**'ye tıklayın.

    ![Depolama Hesabı Gezgini aracını çalıştırın.](./media/functions-integrate-storage-queue-output-binding/functions-storage-manager-connect-1.png)
    
3. 1. adımda kopyaladığınız **Hesap adı** ve **Hesap anahtarı** değerlerini girin, **İleri**'ye ve ardından **Bağlan**'a tıklayın. 
  
    ![Depolama kimlik bilgilerini girin ve bağlanın.](./media/functions-integrate-storage-queue-output-binding/functions-storage-manager-connect-2.png)

4. Bağlı depolama hesabını genişletin, **Kuyruklar**’a sağ tıklayın ve **myqueue-items** adlı bir kuyruğun var olduğunu doğrulayın. Ayrıca zaten kuyrukta olan bir ileti görmeniz gerekir.  
 
    ![Depolama kuyruğu oluşturun.](./media/functions-integrate-storage-queue-output-binding/function-queue-storage-output-view-queue.png)
 

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [Next steps note](../../includes/functions-quickstart-cleanup.md)]

## <a name="next-steps"></a>Sonraki adımlar

Var olan bir işleve çıkış bağlaması eklediniz. 

[!INCLUDE [Next steps note](../../includes/functions-quickstart-next-steps.md)]

Kuyruk depolamaya bağlama hakkında daha fazla bilgi için bkz. [Azure İşlevleri Depolama kuyruğu bağlamaları](functions-bindings-storage-queue.md). 




