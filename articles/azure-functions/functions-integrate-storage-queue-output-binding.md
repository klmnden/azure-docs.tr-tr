---
title: "Azure'da kuyruk iletileri tarafından tetiklenen bir işlev oluşturma | Microsoft Docs"
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
ms.custom: mvc
ms.translationtype: Human Translation
ms.sourcegitcommit: cb4d075d283059d613e3e9d8f0a6f9448310d96b
ms.openlocfilehash: d1ddfbe9a0a0c7c7e0a060776938bd68a87e1ba5
ms.contentlocale: tr-tr
ms.lasthandoff: 06/26/2017

---
<a id="add-messages-to-an-azure-storage-queue-using-functions" class="xliff"></a>

# İşlevleri kullanarak bir Azure Depolama kuyruğuna ileti ekleme

Azure İşlevleri’nde giriş ve çıkış bağlamaları, işlevinizden dış hizmet verilerine bağlanmanın bildirim temelli bir yöntemini sağlar. Bu konu başlığında, Azure Kuyruk Depolama’ya iletiler gönderen bir çıkış bağlaması ekleyerek mevcut bir işlevi güncelleştirmeyi öğrenebilirsiniz.  

![Günlüklerde iletiyi görüntüleyin.](./media/functions-integrate-storage-queue-output-binding/functions-integrate-storage-binding-in-portal.png)

<a id="prerequisites" class="xliff"></a>

## Ön koşullar 

[!INCLUDE [Previous topics](../../includes/functions-quickstart-previous-topics.md)]

* [Microsoft Azure Depolama Gezgini](http://storageexplorer.com/)'ni yükleyin.

[!INCLUDE [functions-portal-favorite-function-apps](../../includes/functions-portal-favorite-function-apps.md)] 

## <a name="add-binding"></a>Çıkış bağlaması ekleme
 
1. İşlev uygulamanızı ve işlevinizi genişletin.

2. **Tümleştir** ve **+ Yeni çıkış** seçeneklerini belirleyin ve ardından **Azure Kuyruk depolama**'ya ve **Seç**'e tıklayın.
    
    ![Azure portalındaki bir işleve Kuyruk depolama çıkış bağlaması ekleyin.](./media/functions-integrate-storage-queue-output-binding/function-add-queue-storage-output-binding.png)

3. Tabloda belirtilen ayarları kullanın ve ardından **Kaydet**'i seçin: 

    ![Azure portalındaki bir işleve Kuyruk depolama çıkış bağlaması ekleyin.](./media/functions-integrate-storage-queue-output-binding/function-add-queue-storage-output-binding-2.png)

    | Ayar      |  Önerilen değer   | Açıklama                              |
    | ------------ |  ------- | -------------------------------------------------- |
    | **Kuyruk adı**   | myqueue-items    | Depolama hesabınızdaki bağlantı kurulacak kuyruğun adı. |
    | **Depolama hesabı bağlantısı** | AzureWebJobStorage | İşlev uygulamanız tarafından kullanılmakta olan depolama hesabı bağlantısını kullanabilir veya yeni bir bağlantı oluşturabilirsiniz.  |
    | **İleti parametre adı** | outQueueItem | Çıkış bağlama parametresinin adı. | 

Bir çıkış bağlaması tanımladığınıza göre, bir kuyruğa ileti eklemek üzere bağlamayı kullanmak için kodu güncelleştirmeniz gerekir.  

<a id="update-the-function-code" class="xliff"></a>

## İşlev kodunu güncelleştirme

1. İşlev kodunu düzenleyicide görüntülemek için işlevinizi seçin. 

2. Bir C# işlevi için, **outQueueItem** depolama bağlama parametresini eklemek üzere işlev tanımınızı aşağıdaki gibi güncelleştirin. JavaScript işlevi için bu adımı atlayın.

    ```cs   
    public static async Task<HttpResponseMessage> Run(HttpRequestMessage req, 
        ICollector<string> outQueueItem, TraceWriter log)
    {
        ....
    }
    ```

3. Yöntemden dönülmeden hemen önce aşağıdaki kodu işleve ekleyin. İşlevinizin diline uygun parçacığı kullanın.

    ```javascript
    context.bindings.outQueueItem = "Name passed to the function: " + 
                (req.query.name || req.body.name);
    ```

    ```cs
    outQueueItem.Add("Name passed to the function: " + name);     
    ```

4. Değişiklikleri kaydetmek için **Kaydet**'i seçin.

HTTP tetikleyicisine geçirilen değer, kuyruğa alınmış bir iletiye eklenir.
 
<a id="test-the-function" class="xliff"></a>

## İşlevi test etme 

1. Kod değişiklikleri kaydedildikten sonra **Çalıştır**'ı seçin. 

    ![Azure portalındaki bir işleve Kuyruk depolama çıkış bağlaması ekleyin.](./media/functions-integrate-storage-queue-output-binding/functions-test-run-function.png)

2. İşlevin başarılı olduğundan emin olmak için günlükleri denetleyin. Çıkış bağlaması ilk kez kullanıldığında, Depolama hesabınızda İşlevler çalışma zamanı tarafından **outqueue** adlı yeni bir kuyruk oluşturulur.

Bundan sonra, yeni kuyruğunuzu ve ona eklediğiniz iletiyi doğrulamak için depolama hesabınıza bağlanabilirsiniz. 

<a id="connect-to-the-queue" class="xliff"></a>

## Kuyruğa bağlanma

Depolama Gezgini’ni daha önce yükleyip depolama hesabınıza bağladıysanız, ilk üç adımı atlayın.    

1. İşlevinizde **Tümleştir**'i ve yeni **Azure Kuyruk depolama** çıkış bağlamasını seçip **Belgeler**'i genişletin. Hem **Hesap adı** hem de **Hesap anahtarı** değerlerini kopyalayın. Depolama hesabına bağlanmak için bu kimlik bilgilerini kullanacaksınız.
 
    ![Depolama hesabı bağlantısı için kimlik bilgilerini alın.](./media/functions-integrate-storage-queue-output-binding/function-get-storage-account-credentials.png)

2. [Microsoft Azure Depolama Gezgini](http://storageexplorer.com/) aracını çalıştırın, sol taraftaki bağlanma simgesini seçin, **Depolama hesabı adı ve anahtarı kullan**'ı seçip **İleri**'ye tıklayın.

    ![Depolama Hesabı Gezgini aracını çalıştırın.](./media/functions-integrate-storage-queue-output-binding/functions-storage-manager-connect-1.png)
    
3. 1. adımdaki **Hesap adını** ve **Hesap anahtarını** uygun alanlara yapıştırıp **İleri** ve **Bağlan** seçeneklerini belirleyin. 
  
    ![Depolama kimlik bilgilerini yapıştırın ve bağlanın.](./media/functions-integrate-storage-queue-output-binding/functions-storage-manager-connect-2.png)

4. Bağlı depolama hesabını genişletin, **Kuyruklar**’a sağ tıklayın ve **myqueue-items** adlı bir kuyruğun var olduğunu doğrulayın. Ayrıca zaten kuyrukta olan bir ileti görmeniz gerekir.  
 
    ![Depolama kuyruğu oluşturun.](./media/functions-integrate-storage-queue-output-binding/function-queue-storage-output-view-queue.png)
 

<a id="clean-up-resources" class="xliff"></a>

## Kaynakları temizleme

[!INCLUDE [Next steps note](../../includes/functions-quickstart-cleanup.md)]

<a id="next-steps" class="xliff"></a>

## Sonraki adımlar

Var olan bir işleve çıkış bağlaması eklediniz. 

[!INCLUDE [Next steps note](../../includes/functions-quickstart-next-steps.md)]

Kuyruk depolamaya bağlama hakkında daha fazla bilgi için bkz. [Azure İşlevleri Depolama kuyruğu bağlamaları](functions-bindings-storage-queue.md). 




