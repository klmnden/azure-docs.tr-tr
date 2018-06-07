---
title: Bir Azure olay hub'ı oluşturma | Microsoft Docs
description: Azure Event Hubs ad alanı ve Azure portalını kullanarak bir event hub oluşturma
services: event-hubs
author: sethmanheim
manager: timlt
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.date: 05/29/2017
ms.author: sethm
ms.openlocfilehash: 9b466d4e727c1511ca2318c0da3ec2807a965a5d
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34625551"
---
# <a name="create-an-event-hubs-namespace-and-an-event-hub-using-the-azure-portal"></a>Bir olay hub'ları ad alanı ve Azure portalını kullanarak bir event hub oluşturma

## <a name="create-an-event-hubs-namespace"></a>Bir olay hub'ları ad alanı oluşturma

1. Oturum [Azure portal][Azure portal], tıklatıp **kaynak oluşturma** en üst ekranın sol.
2. Tıklatın **nesnelerin interneti**ve ardından **olay hub'ları**.
   
    ![](./media/event-hubs-create/create-event-hub9.png)

3. İçinde **ad alanı oluşturma**, bir ad alanı adı girin. Adın kullanılabilirliği sistem tarafından hemen denetlenir.  

4. Ad alanı adının kullanılabilir durumda olduğundan emin olduktan sonra fiyatlandırma katmanını (Temel veya Standart) seçin. Ayrıca, bir Azure aboneliği, kaynak grubu ve kaynağın oluşturulacağı konumu seçin.
 
5. Ad alanını oluşturmak için **Oluştur**’a tıklayın. Sistemin kaynakları tam olarak sağlaması için birkaç dakika beklemeniz gerekebilir.

    ![](./media/event-hubs-create/create-event-hub1.png)

6. Ad alanları portal listesinde yeni oluşturulan ad alanına tıklayın.

7. Tıklatın **paylaşılan erişim ilkeleri**ve ardından **RootManageSharedAccessKey**.
    
    ![](./media/event-hubs-create/create-event-hub7.png)

8. **RootManageSharedAccessKey** bağlantı dizesini panoya kopyalamak için kopyala düğmesine tıklayın. Bu bağlantı dizesi daha sonra kullanmak üzere not defteri gibi geçici bir konuma kaydedin.
    
    ![](./media/event-hubs-create/create-event-hub8.png)

## <a name="create-an-event-hub"></a>Olay hub’ı oluşturma

1. Olay hub'ları ad listesinde yeni oluşturulan ad alanına tıklayın.      
   
    ![](./media/event-hubs-create/create-event-hub2.png) 

2. Ad alanı dikey penceresinde **Event Hubs**’a tıklayın.
   
    ![](./media/event-hubs-create/create-event-hub3.png)

3. Dikey pencerenin üstündeki **+ olay hub'ı**.
   
    ![](./media/event-hubs-create/create-event-hub4.png)
4. Olay hub'ınız için bir ad yazın ve ardından **oluşturma**. 

Olay hub'ınız şimdi oluşturulur ve olayları alıp göndermek için gereken bağlantı dizelerine sahipsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Event Hubs hakkında daha fazla bilgi için aşağıdaki bağlantıları ziyaret edin:

* [Event Hubs’a genel bakış](event-hubs-what-is-event-hubs.md)
* [Event Hubs API’sine genel bakış](event-hubs-api-overview.md)

[Azure portal]: https://portal.azure.com/