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
ms.openlocfilehash: b84b0a8e09bf739ce62dee167ff751b491765c66
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/23/2019
ms.locfileid: "66154558"
---
### <a name="create-a-storage-account-for-event-processor-host"></a>Olay İşleyicisi Ana Bilgisayarı için bir depolama hesabı oluşturma
Olay İşleyicisi Ana Bilgisayarı, olay hub’larına ait kalıcı denetim noktalarını ve paralel alımları yöneterek bu olay hub’larından olay almayı basitleştiren akıllı bir aracıdır. Olay İşleyicisi Ana Bilgisayarı, denetim noktası için bir depolama hesabına ihtiyaç duyar. Aşağıdaki örnekte depolama hesabı oluşturma ve erişim için anahtarını alma adımları gösterilmiştir:

1. Azure portalda ekranın sol üst köşesindeki **Kaynak oluştur**'u seçin.

2. **Depolama**’yı ve sonra **Depolama hesabı - blob, dosya, tablo, kuyruk** öğesini seçin.
   
    ![Depolama Hesabı Seç](./media/event-hubs-create-storage/create-storage1.png)

3. **Depolama hesabı oluştur** sayfasında aşağıdaki adımları gerçekleştirin: 

   1. Depolama hesabı için bir ad girin. 
   2. Olay hub'ını içeren Azure aboneliğini seçin.
   3. Olay hub'ının bulunduğu kaynak grubunu seçin.
   4. Kaynağın oluşturulacağı konumu seçin. 
   5. Ardından **Gözden geçir + oluştur**’a tıklayın.
   
      ![Depolama hesabı oluştur - sayfa](./media/event-hubs-create-storage/create-storage2.png)

4. **Gözden geçir + oluştur** sayfasında değerleri gözden geçirin ve **Oluştur**'u seçin. 

    ![Depolama hesabı ayarlarını gözden geçirme ve oluşturma](./media/event-hubs-create-storage/review-create-storage-account.png)
5. Gördükten sonra **dağıtımlar başarılı** ileti, select **kaynağa Git** sayfanın üstünde. Depolama hesabı sayfasında kaynak listesinden depolama hesabınızı seçerek de başlatabilirsiniz.  

    ![Dağıtımda depolama hesabını seçme](./media/event-hubs-create-storage/select-storage-deployment.png) 
7. **Temel Bileşenler** penceresinde **Bloblar**'u seçin. 

    ![BLOB hizmeti seçin](./media/event-hubs-create-storage/select-blobs-service.png)
1. Seçin **+ kapsayıcı** girin en üstünde bir **adı** kapsayıcı ve seçin için **Tamam**. 

    ![Blob kapsayıcısı oluşturma](./media/event-hubs-create-storage/create-blob-container.png)
1. Seçin **erişim anahtarları** sol taraftaki menüde ve değerini kopyalayın **key1**. 

    Aşağıdaki değerleri Not Defteri veya başka bir geçici konuma kaydedin.
    - Depolama hesabının adı
    - Depolama hesabının erişim anahtarı
    - Kapsayıcının adı
