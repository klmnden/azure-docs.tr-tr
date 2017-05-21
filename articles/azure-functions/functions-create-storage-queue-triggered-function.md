---
title: "Azure&quot;da kuyruk iletileri tarafından tetiklenen bir işlev oluşturma | Microsoft Docs"
description: "Bir Azure Depolama kuyruğuna gönderilmiş iletiler tarafından çağrılan sunucusuz işlev oluşturmak için Azure İşlevlerini kullanın."
services: azure-functions
documentationcenter: na
author: ggailey777
manager: erikre
editor: 
tags: 
ms.assetid: 361da2a4-15d1-4903-bdc4-cc4b27fc3ff4
ms.service: functions
ms.devlang: multiple
ms.topic: get-started-article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 04/25/2017
ms.author: glenga
ms.translationtype: Human Translation
ms.sourcegitcommit: 71fea4a41b2e3a60f2f610609a14372e678b7ec4
ms.openlocfilehash: 93b3910ba2f95bb6f3228926a51f71f96ccd1e23
ms.contentlocale: tr-tr
ms.lasthandoff: 05/10/2017


---
# <a name="create-a-function-triggered-by-azure-queue-storage"></a>Azure Kuyruk Depolama tarafından tetiklenen bir işlev oluşturma

Bir Azure Depolama kuyruğuna ileti gönderildiğinde tetiklenen bir işlev oluşturmayı öğrenin.  

![Günlüklerde iletiyi görüntüleyin.](./media/functions-create-storage-queue-triggered-function/function-app-in-portal-editor.png)

Bu konu başlığı altındaki adımların tümünü beş dakikadan kısa bir sürede tamamlamalısınız.

## <a name="prerequisites"></a>Ön koşullar

[!INCLUDE [Previous quickstart note](../../includes/functions-quickstart-previous-topics.md)]

Ayrıca [Microsoft Azure Depolama Gezgini](http://storageexplorer.com/)'ni de indirip yüklemeniz gerekir. 

[!INCLUDE [functions-portal-favorite-function-apps](../../includes/functions-portal-favorite-function-apps.md)] 

## <a name="create-function"></a>Kuyruk ile tetiklenen bir işlev oluşturma

İşlev uygulamanızı genişletin, **İşlevler**'in yanındaki **+** düğmesine ve ardından tercih ettiğiniz dildeki **QueueTrigger** şablonuna tıklayın. Ardından tabloda belirtilen ayarları kullanın ve **Oluştur**'a tıklayın.

![Depolama kuyruğu ile tetiklenen işlevi oluşturun.](./media/functions-create-storage-queue-triggered-function/functions-create-queue-storage-trigger-portal.png)
    
| Ayar      |  Önerilen değer   | Açıklama                                        |
| ------------ |  ----------------- | -------------------------------------------------- |
| **Kuyruk adı**   | myqueue-items    | Depolama hesabınızdaki bağlantı kurulacak kuyruğun adı. |
| **Depolama hesabı bağlantısı** | AzureWebJobStorage | İşlev uygulamanız tarafından kullanılmakta olan depolama hesabı bağlantısını kullanabilir veya yeni bir bağlantı oluşturabilirsiniz.  |
| **İşlevinizi adlandırın** | İşlev uygulamanızda benzersiz olmalıdır | Kuyruk tarafından tetiklenen bu işlevin adı. |  

Ardından Azure Depolama hesabınıza bağlanıp **myqueue-items** depolama kuyruğunu oluşturun.

## <a name="create-the-queue"></a>Kuyruk oluşturma

1. İşlevinizde **Tümleştir**'e tıklayın, **Belgeler**'i genişletin ve hem **Hesap adı** hem de **Hesap anahtarı** değerlerini kopyalayın. Depolama hesabına bağlanmak için bu kimlik bilgilerini kullanacaksınız. Depolama hesabınıza önceden bağlandıysanız 4. adıma geçin.
 
    ![Depolama hesabı bağlantısı için kimlik bilgilerini alın.](./media/functions-create-storage-queue-triggered-function/functions-storage-account-connection.png)v

2. [Microsoft Azure Depolama Gezgini](http://storageexplorer.com/) aracını çalıştırın, sol taraftaki bağlan simgesine tıklayın, **Depolama hesabı adı ve anahtarı kullan**'ı seçin ve **İleri**'ye tıklayın.

    ![Depolama Hesabı Gezgini aracını çalıştırın.](./media/functions-create-storage-queue-triggered-function/functions-storage-manager-connect-1.png)
    
3. 1. adımda kopyaladığınız **Hesap adı** ve **Hesap anahtarı** değerlerini girin, **İleri**'ye ve ardından **Bağlan**'a tıklayın. 
  
    ![Depolama kimlik bilgilerini girin ve bağlanın.](./media/functions-create-storage-queue-triggered-function/functions-storage-manager-connect-2.png)

4. Eklediğiniz depolama hesabını genişletin, **Kuyruklar**'a sağ tıklayın, **Kuyruk oluştur**'a tıklayın, `myqueue-items` yazın ve Enter tuşuna basın.
 
    ![Depolama kuyruğu oluşturun.](./media/functions-create-storage-queue-triggered-function/functions-storage-manager-create-queue.png)

Artık bir depolama kuyruğunuz var ve kuyruğa ileti ekleyerek işlevi test edebilirsiniz.  

## <a name="test-the-function"></a>İşlevi test etme

1. Azure portalına dönün, işlevinizi bulun, sayfanın en altındaki **Günlükler** bölümünü genişletin ve günlük akışının duraklatılmış olmadığından emin olun.

2. Depolama Gezgini'nde depolama hesabınızı genişletin, **Kuyruklar**'ı ve **myqueue-items** öğesini seçip **İleti ekle**'ye tıklayın. 

    ![Kuyruğa bir ileti ekleyin.](./media/functions-create-storage-queue-triggered-function/functions-storage-manager-add-message.png)

2. "Hello World!" iletinizi **İleti metni** alanına yazın ve **Tamam**'a tıklayın.
 
3. Birkaç saniye bekledikten sonra işlev günlüklerinize dönün ve yeni iletinin kuyruktan okunmuş olduğunu doğrulayın. 

    ![Günlüklerde iletiyi görüntüleyin.](./media/functions-create-storage-queue-triggered-function/functions-queue-storage-trigger-view-logs.png)

4. Depolama Gezgini'ne dönüp **Yenile**'ye tıklayın ve iletinin işlenip kuyruktan kaldırılmış olduğunu doğrulayın.

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [Next steps note](../../includes/functions-quickstart-cleanup.md)]

## <a name="next-steps"></a>Sonraki adımlar

Depolama kuyruğuna bir ileti eklendiğinde çalışacak bir işlev oluşturdunuz. 

[!INCLUDE [Next steps note](../../includes/functions-quickstart-next-steps.md)]

Kuyruk depolama tetikleyicileri hakkında daha fazla bilgi için bkz. [Azure İşlevleri Kuyruk depolama bağlamaları](functions-bindings-storage-queue.md). 




