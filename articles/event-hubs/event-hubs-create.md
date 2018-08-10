---
title: Bir Azure olay hub'ı oluşturma | Microsoft Docs
description: Bir Azure Event Hubs ad alanı ve Azure portalını kullanarak bir olay hub'ı oluşturma
services: event-hubs
author: ShubhaVijayasarathy
manager: timlt
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.date: 05/29/2017
ms.author: shvija
ms.openlocfilehash: fb2020f7bfc8521e141adb4eefbc227e8123a269
ms.sourcegitcommit: d0ea925701e72755d0b62a903d4334a3980f2149
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/09/2018
ms.locfileid: "40002629"
---
# <a name="create-an-event-hubs-namespace-and-an-event-hub-using-the-azure-portal"></a>Bir Event Hubs ad alanı ve Azure portalını kullanarak bir olay hub'ı oluşturma

## <a name="create-an-event-hubs-namespace"></a>Event Hubs ad alanı oluşturma

1. Oturum [Azure portalında][Azure portal], tıklatıp **kaynak Oluştur** , ekranın sol üst köşesindeki.
2. **Nesnelerin İnterneti**’ne ve ardından **Event Hubs**’a tıklayın.
   
    ![](./media/event-hubs-create/create-event-hub9.png)

3. **Ad alanı oluştur** bölümüne bir ad alanı adı girin. Adın kullanılabilirliği sistem tarafından hemen denetlenir.  

4. Ad alanı adının kullanılabilir durumda olduğundan emin olduktan sonra fiyatlandırma katmanını (Temel veya Standart) seçin. Ayrıca, bir Azure aboneliği, kaynak grubu ve kaynağın oluşturulacağı konumu seçin.
 
5. Ad alanını oluşturmak için **Oluştur**’a tıklayın. Sistemin kaynakları tam olarak sağlaması için birkaç dakika beklemeniz gerekebilir.

    ![](./media/event-hubs-create/create-event-hub1.png)

6. Portaldaki ad alanları listesinde, yeni oluşturulan ad alanına tıklayın.

7. **Paylaşılan erişim ilkeleri**'ne ve ardından **RootManageSharedAccessKey** öğesine tıklayın.
    
    ![](./media/event-hubs-create/create-event-hub7.png)

8. **RootManageSharedAccessKey** bağlantı dizesini panoya kopyalamak için kopyala düğmesine tıklayın. Bu bağlantı dizesini daha sonra kullanmak üzere Not Defteri gibi geçici bir konuma kaydedin.
    
    ![](./media/event-hubs-create/create-event-hub8.png)

## <a name="create-an-event-hub"></a>Olay hub’ı oluşturma

1. Event Hubs ad alanı listesinde yeni oluşturulan ad alanına tıklayın.      
   
    ![](./media/event-hubs-create/create-event-hub2.png) 

2. Ad alanı dikey penceresinde **Event Hubs**’a tıklayın.
   
    ![](./media/event-hubs-create/create-event-hub3.png)

3. Dikey pencerenin en üstünde tıklayın **+ olay hub'ı**.
   
    ![](./media/event-hubs-create/create-event-hub4.png)
4. Olay hub'ınız için bir ad yazın, ardından **Oluştur**’a tıklayın. 

Olay hub'ınız şimdi oluşturuldu ve olayları alıp göndermek için gereken bağlantı dizelerine sahipsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Event Hubs hakkında daha fazla bilgi için şu bağlantıları ziyaret edin:

* [Event Hubs’a genel bakış](event-hubs-what-is-event-hubs.md)
* [Event Hubs API’sine genel bakış](event-hubs-api-overview.md)

[Azure portal]: https://portal.azure.com/