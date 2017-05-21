---
title: "Azure&quot;da Blob depolama tarafından tetiklenen bir işlev oluşturma | Microsoft Docs"
description: "Azure Blob depolamaya eklenen öğeler tarafından çağrılan sunucusuz bir işlev oluşturmak için Azure İşlevlerini kullanın."
services: azure-functions
documentationcenter: na
author: ggailey777
manager: erikre
editor: 
tags: 
ms.assetid: d6bff41c-a624-40c1-bbc7-80590df29ded
ms.service: functions
ms.devlang: multiple
ms.topic: get-started-article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 05/02/2017
ms.author: glenga
ms.translationtype: Human Translation
ms.sourcegitcommit: 71fea4a41b2e3a60f2f610609a14372e678b7ec4
ms.openlocfilehash: b00f9e7491048a5dd60e0decab1727a1ff01448e
ms.contentlocale: tr-tr
ms.lasthandoff: 05/10/2017


---
# <a name="create-a-function-triggered-by-azure-blob-storage"></a>Azure Blob depolama ile tetiklenen bir işlev oluşturma

Azure Blob depolamada dosyalar karşıya yüklendiğinde veya güncelleştirildiğinde tetiklenen bir işlev oluşturma hakkında bilgi edinin.  

![Günlüklerde iletiyi görüntüleyin.](./media/functions-create-storage-blob-triggered-function/function-app-in-portal-editor.png)

Bu konu başlığı altındaki adımların tümünü beş dakikadan kısa bir sürede tamamlamalısınız.

## <a name="prerequisites"></a>Ön koşullar

[!INCLUDE [Previous quickstart note](../../includes/functions-quickstart-previous-topics.md)]

Ayrıca [Microsoft Azure Depolama Gezgini](http://storageexplorer.com/)'ni de indirip yüklemeniz gerekir. 

[!INCLUDE [functions-portal-favorite-function-apps](../../includes/functions-portal-favorite-function-apps.md)] 

## <a name="create-function"></a>Blob depolama ile tetiklenen bir işlev oluşturma

İşlev uygulamanızı genişletin, **İşlevler**'in yanındaki **+** düğmesine ve ardından tercih ettiğiniz dildeki **BlobTrigger** şablonuna tıklayın. Sonra, tabloda belirtilen ayarları kullanın ve **Oluştur**'a tıklayın.

![Blob depolama ile tetiklenen bir işlev oluşturun.](./media/functions-create-storage-blob-triggered-function/functions-create-blob-storage-trigger-portal.png)
    
| Ayar      |  Önerilen değer   | Açıklama                                        |
| ------------ |  ----------------- | -------------------------------------------------- |
| **Path**   | mycontainer/{name}    | İzlenmekte olan Blob depolamanın konumu. Blob’un dosya adı bağlamaya _name_ parametresi olarak geçirilir.  |
| **Depolama hesabı bağlantısı** | AzureWebJobStorage | İşlev uygulamanız tarafından kullanılmakta olan depolama hesabı bağlantısını kullanabilir veya yeni bir bağlantı oluşturabilirsiniz.  |
| **İşlevinizi adlandırın** | İşlev uygulamanızda benzersiz olmalıdır | Kuyruk tarafından tetiklenen bu işlevin adı. | 

Ardından Azure Depolama hesabınıza bağlanıp **mycontainer** kapsayıcısını oluşturun.

## <a name="create-the-container"></a>Kapsayıcı oluşturma

1. İşlevinizde **Tümleştir**'e tıklayın, **Belgeler**'i genişletin ve hem **Hesap adı** hem de **Hesap anahtarı** değerlerini kopyalayın. Depolama hesabına bağlanmak için bu kimlik bilgilerini kullanacaksınız. Depolama hesabınıza önceden bağlandıysanız 4. adıma geçin.
 
    ![Depolama hesabı bağlantısı için kimlik bilgilerini alın.](./media/functions-create-storage-blob-triggered-function/functions-storage-account-connection.png)

2. [Microsoft Azure Depolama Gezgini](http://storageexplorer.com/) aracını çalıştırın, sol taraftaki bağlan simgesine tıklayın, **Depolama hesabı adı ve anahtarı kullan**'ı seçin ve **İleri**'ye tıklayın.

    ![Depolama Hesabı Gezgini aracını çalıştırın.](./media/functions-create-storage-blob-triggered-function/functions-storage-manager-connect-1.png)
    
3. 1. adımda kopyaladığınız **Hesap adı** ve **Hesap anahtarı** değerlerini girin, **İleri**'ye ve ardından **Bağlan**'a tıklayın. 
  
    ![Depolama kimlik bilgilerini girin ve bağlanın.](./media/functions-create-storage-blob-triggered-function/functions-storage-manager-connect-2.png)

4. Bağlı depolama hesabını genişletin, **Blob kapsayıcılar**’a sağ tıklayın, **Blob kapsayıcı oluştur**’a tıklayın, `mycontainer` yazın ve ardından Enter tuşuna basın.
 
    ![Depolama kuyruğu oluşturun.](./media/functions-create-storage-blob-triggered-function/functions-storage-manager-create-blob-container.png)

Artık bir blob kapsayıcısına sahip olduğunuza göre, kapsayıcıya bir dosya yükleyerek işlevi test edebilirsiniz.  

## <a name="test-the-function"></a>İşlevi test etme

1. Azure portalına dönün, işlevinizi bulun, sayfanın en altındaki **Günlükler** bölümünü genişletin ve günlük akışının duraklatılmış olmadığından emin olun.

2. Depolama Gezgini’nde depolama hesabınızı genişletin, **Blob kapsayıcıları** ve **mycontainer** öğesine tıklayın. **Karşıya Yükle**’ye ve ardından **Dosyaları yükle...** seçeneğine tıklayın.

    ![Dosyayı blob kapsayıcısına yükleyin.](./media/functions-create-storage-blob-triggered-function/functions-storage-manager-upload-file-blob.png)

2. **Dosyaları karşıya yükle** iletişim kutusunda **Dosyalar** alanına tıklayın. Yerel bilgisayarınızda görüntü dosyası gibi bir dosyaya gidin, seçin, **Aç**’ı ve ardından **Karşıya Yükle**’yi seçin.   
 
3. İşlev günlüklerinize geri dönün ve blob’un okunduğunu doğrulayın. 

   ![Günlüklerde iletiyi görüntüleyin.](./media/functions-create-storage-blob-triggered-function/functions-blob-storage-trigger-view-logs.png)

    >[!NOTE]
    > İşlev uygulamanız varsayılan Tüketim planında çalıştığında, blob’un eklenmesi veya güncelleştirilmesi ile işlevin tetiklenmesi arasında birkaç dakika gecikme olabilir. Blob ile tetiklenen işlevlerde düşük gecikme süresi gerekiyorsa, işlev uygulamanızı bir App Service planında çalıştırmayı düşünün. 

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [Next steps note](../../includes/functions-quickstart-cleanup.md)]

## <a name="next-steps"></a>Sonraki adımlar

Blob depolamaya bir blob eklendiğinde çalışacak bir işlev oluşturdunuz. 

[!INCLUDE [Next steps note](../../includes/functions-quickstart-next-steps.md)]

Blob depolama tetikleyicileri hakkında daha fazla bilgi için bkz. [Azure İşlevleri Blob depolama bağlamaları](functions-bindings-storage-blob.md). 




