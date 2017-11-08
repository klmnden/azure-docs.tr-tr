---
title: "Bir Azure olay hub'ı oluşturma | Microsoft Docs"
description: "Azure Event Hubs ad alanı ve Azure portalını kullanarak bir event hub oluşturma"
services: event-hubs
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: ff99e327-c8db-4354-9040-9c60c51a2191
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/07/2017
ms.author: sethm
ms.openlocfilehash: e444c4505d4744c95e08c4ef0d33566356785c81
ms.sourcegitcommit: 0930aabc3ede63240f60c2c61baa88ac6576c508
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/07/2017
---
# <a name="create-an-event-hubs-namespace-and-an-event-hub-using-the-azure-portal"></a>Bir olay hub'ları ad alanı ve Azure portalını kullanarak bir event hub oluşturma

## <a name="create-an-event-hubs-namespace"></a>Bir olay hub'ları ad alanı oluşturma
1. Oturum [Azure portal][Azure portal], tıklatıp **kaynak oluşturma** en üst ekranın sol.
1. Tıklatın **nesnelerin interneti**ve ardından **olay hub'ları**.
   
    ![](./media/event-hubs-create/create-event-hub9.png)
1. İçinde **ad alanı oluşturma**, bir ad alanı adı girin. Adın kullanılabilirliği sistem tarafından hemen denetlenir.
   
    ![](./media/event-hubs-create/create-event-hub1.png)
1. Ad alanı adının kullanılabilir durumda olduğundan emin olduktan sonra fiyatlandırma katmanını (Temel veya Standart) seçin. Ayrıca, bir Azure aboneliği, kaynak grubu ve kaynağın oluşturulacağı konumu seçin. 
1. Ad alanını oluşturmak için **Oluştur**’a tıklayın. Tam olarak kaynakları sağlamak üzere sistem için bir kaç dakika beklemeniz gerekebilir.
2. Ad alanları portal listesinde yeni oluşturulan ad alanına tıklayın.
2. Tıklatın **paylaşılan erişim ilkeleri**ve ardından **RootManageSharedAccessKey**.
    
    ![](./media/event-hubs-create/create-event-hub7.png)

3. **RootManageSharedAccessKey** bağlantı dizesini panoya kopyalamak için kopyala düğmesine tıklayın. Bu bağlantı dizesi daha sonra kullanmak üzere not defteri gibi geçici bir konuma kaydedin.
    
    ![](./media/event-hubs-create/create-event-hub8.png)

## <a name="create-an-event-hub"></a>Olay hub’ı oluşturma

1. Olay hub'ları ad listesinde yeni oluşturulan ad alanına tıklayın.      
   
    ![](./media/event-hubs-create/create-event-hub2.png) 

2. Ad alanı dikey penceresinde **Event Hubs**’a tıklayın.
   
    ![](./media/event-hubs-create/create-event-hub3.png)

1. Dikey pencerenin en üstündeki **Olay Hub’ı Ekle** seçeneğine tıklayın.
   
    ![](./media/event-hubs-create/create-event-hub4.png)
1. Olay hub'ınız için bir ad yazın ve ardından **oluşturma**.
   
    ![](./media/event-hubs-create/create-event-hub5.png)

Olay hub'ınız şimdi oluşturulur ve olayları alıp göndermek için gereken bağlantı dizelerine sahipsiniz.

## <a name="next-steps"></a>Sonraki adımlar
Event Hubs hakkında daha fazla bilgi için aşağıdaki bağlantıları ziyaret edin:

* [Event Hubs’a genel bakış](event-hubs-what-is-event-hubs.md)
* [Event Hubs API’sine genel bakış](event-hubs-api-overview.md)

[Azure portal]: https://portal.azure.com/