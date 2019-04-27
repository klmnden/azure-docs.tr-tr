---
title: Azure hızlı başlangıç - Azure portalını kullanarak Azure Depolama'daki bir kuyruk oluşturun | Microsoft Docs
description: Bu hızlı başlangıçta, bir kuyruk oluşturmak için Azure portalını kullanın. Sonra bir ileti ekleyin, ileti özelliklerini görüntüleme ve iletiyi sıradan çıkar için Azure portalını kullanın.
services: storage
author: tamram
ms.custom: mvc
ms.service: storage
ms.topic: quickstart
ms.date: 03/06/2019
ms.author: tamram
ms.openlocfilehash: 8a5dba5f949d5c20e0fbf752caac4125b8aecd52
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60460847"
---
# <a name="quickstart-create-a-queue-and-add-a-message-with-the-azure-portal"></a>Hızlı Başlangıç: Bir kuyruk oluşturun ve Azure portalıyla bir ileti ekleyin

Bu hızlı başlangıçta nasıl kullanılacağını öğrenin [Azure portalında](https://portal.azure.com/) Azure Depolama'da bir kuyruk oluşturmak ve eklemek ve iletileri sıradan çıkarma için.

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [storage-quickstart-prereq-include](../../../includes/storage-quickstart-prereq-include.md)]

## <a name="create-a-queue"></a>Bir kuyruk oluşturma

Azure portalında bir kuyruk oluşturmak için aşağıdaki adımları izleyin:

1. Azure portalında yeni depolama hesabınıza gidin.
2. Depolama hesabı için sol taraftaki menüde kaydırarak **kuyruk hizmeti** bölümüne ve ardından **kuyrukları**.
3. Seçin **+ kuyruk** düğmesi.
4. Yeni sıranız için bir ad yazın. Kuyruk adı küçük harfle yazılmalıdır, bir harf veya sayı ile başlamalıdır ve yalnızca harf, rakam ve tire (-) karakteri içerebilir.
6. Seçin **Tamam** sırayı oluşturmak için.

    ![Azure portalında bir kuyruk oluşturulacağını gösteren ekran görüntüsü](media/storage-quickstart-queues-portal/create-queue.png)

## <a name="add-a-message"></a>Bir ileti ekleyin

Ardından, yeni kuyruğa bir ileti ekleyin. Bir ileti boyutu 64 KB'ye kadar olabilir.

1. Yeni Kuyruk depolama hesabındaki kuyrukların listesi seçin.
1. Seçin **+ Ekle ileti** düğmesini kuyruğa bir ileti ekleyin. Bir ileti girin **ileti metni** alan. 
1. İleti süresinin sona erdiği belirtin. Bir ileti kuyruğu kalabileceği en uzun süre 7 gündür.
1. İletiyi Base64 olarak kodlamak bu seçeneği belirtin. İkili veri kodlama önerilir.
1. Seçin **Tamam** düğmesini ileti ekleyin.

    ![Bir ileti kuyruğuna gösteren ekran görüntüsü](media/storage-quickstart-queues-portal/add-message.png)

## <a name="view-message-properties"></a>İleti özelliklerini görüntüleme

Bir ileti ekledikten sonra Azure portalında sırada tüm iletilerin listesini görüntüler. İleti kimliği, ileti, ileti ekleme zamanı ve ileti süre sonu içeriğini görüntüleyebilirsiniz. Bu ileti dequeued kaç kez de görebilirsiniz.

![Ekran gösterme ileti özellikleri](media/storage-quickstart-queues-portal/view-message-properties.png)

## <a name="dequeue-a-message"></a>Bir iletiyi sıradan çıkar

Azure portalından sıranın önüne gelen iletiyi sıradan çıkar. Bir iletiyi sıradan çıkar ileti silinir. 

Dequeueing, her zaman en eski ileti kuyrukta kaldırır. 

![Portaldan bir iletiyi sıradan çıkar gösteren ekran görüntüsü](media/storage-quickstart-queues-portal/dequeue-message.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, bir kuyruk oluşturmak, bir ileti ekleyin, ileti özelliklerini görüntüleme ve Azure portalında bir iletiyi sıradan çıkar öğrendiniz.

> [!div class="nextstepaction"]
> [Azure kuyrukları nelerdir?](storage-queues-introduction.md)
