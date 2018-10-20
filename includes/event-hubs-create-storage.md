---
title: include dosyası
description: include dosyası
services: event-hubs
author: spelluru
ms.service: event-hubs
ms.topic: include
ms.date: 10/16/2018
ms.author: spelluru
ms.custom: include file
ms.openlocfilehash: e499a0f7bec47e672c599c729a15cc3e3d04a28a
ms.sourcegitcommit: 62759a225d8fe1872b60ab0441d1c7ac809f9102
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/19/2018
ms.locfileid: "49471661"
---
## <a name="create-a-storage-account-for-event-processor-host"></a>Olay İşleyicisi Ana Bilgisayarı için bir depolama hesabı oluşturma
Olay İşleyicisi Ana Bilgisayarı, olay hub’larına ait kalıcı denetim noktalarını ve paralel alımları yöneterek bu olay hub’larından olay almayı basitleştiren akıllı bir aracıdır. Olay İşleyicisi Ana Bilgisayarı, denetim noktası için bir depolama hesabına ihtiyaç duyar. Aşağıdaki örnekte depolama hesabı oluşturma ve erişim için anahtarını alma adımları gösterilmiştir:

1. Azure portal ve select **kaynak Oluştur** , ekranın sol üst köşesindeki.

2. **Depolama**’yı ve sonra **Depolama hesabı - blob, dosya, tablo, kuyruk** öğesini seçin.
   
    ![Depolama Hesabı Seç](./media/event-hubs-create-storage/create-storage1.png)

3. Üzerinde **depolama hesabı oluşturma** sayfasında, aşağıdaki adımları uygulayın: 

    1. Depolama hesabı için bir ad girin. 
    2. Olay hub'ı içeren bir Azure aboneliği seçin.
    3. Olay hub'ı içeren kaynak grubunu seçin.
    4. Kaynağın oluşturulacağı konumu seçin. 
    5. Ardından **gözden geçir + Oluştur**.
   
    ![Depolama hesabı oluşturma - sayfası](./media/event-hubs-create-storage/create-storage2.png)

4. Üzerinde **gözden + Oluştur** sayfasında değerleri gözden geçirin ve seçin **Oluştur**. 

    ![Depolama hesabı ayarlarını gözden geçir ve Oluştur](./media/event-hubs-create-storage/review-create-storage-account.png)
5. Gördükten sonra **dağıtımlar başarılı** ileti, select **kaynağa var** sayfanın üstünde. Depolama hesabı sayfasında kaynak listesinden depolama hesabınızı seçerek de başlatabilirsiniz.  

    ![Dağıtımdan depolama hesabını seçin](./media/event-hubs-create-storage/select-storage-deployment.png) 
7. İçinde **Essentials** penceresinde **Blobları**. 

    ![BLOB hizmeti seçin](./media/event-hubs-create-storage/select-blobs-service.png)
1. Seçin **+ kapsayıcı** girin en üstünde bir **adı** kapsayıcı ve seçin için **Tamam**. 

    ![Blob kapsayıcısı oluşturma](./media/event-hubs-create-storage/create-blob-container.png)
1. Seçin **erişim anahtarları** sol taraftaki menüde ve değerini kopyalayın **key1**. 

    Aşağıdaki değerleri Not Defteri veya başka bir geçici konuma kaydedin.
    - Depolama hesabının adı
    - Depolama hesabının erişim anahtarı
    - Kapsayıcının adı