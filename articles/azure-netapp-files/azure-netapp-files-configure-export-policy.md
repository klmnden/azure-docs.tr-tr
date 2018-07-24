---
title: Azure NetApp Files birimi için dışarı aktarma ilkesi yapılandırma | Microsoft Docs
description: Azure NetApp Files birimine erişimi denetlemek için dışarı aktarma ilkesi yapılandırma işlemi açıklanır
services: azure-netapp-files
documentationcenter: ''
author: b-juche
manager: ''
editor: ''
ms.assetid: ''
ms.service: azure-netapp-files
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 03/28/2018
ms.author: b-juche
ms.openlocfilehash: 08de7e2ca8cd993a0931f5b16cb9d6c9a04e53dc
ms.sourcegitcommit: e0a678acb0dc928e5c5edde3ca04e6854eb05ea6
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/13/2018
ms.locfileid: "39010965"
---
# <a name="configure-export-policy-for-a-volume-optional"></a>Birim için dışarı aktarma ilkesini yapılandırma (isteğe bağlı)

İsteğe bağlı olarak Azure NetApp Files birimine erişimi denetlemek için dışarı aktarma ilkesi yapılandırabilirsiniz. 

## <a name="steps"></a>Adımlar 

1.  Birim Yönetme dikey penceresindeki **Dışarı aktarma İlkesi Oluştur** dikey penceresine tıklayın. 

2.  İlke kuralını oluşturmak veya dışarı aktarmak için aşağıdaki alanların bilgilerini belirtin:   
    *  **Dizin**   
        Kuralın dizin numarasını belirtin.  
        Dışarı aktarma ilkesi en fazla beş kural içerir. Kurallar, dizin numaraları listesindeki sıraya göre değerlendirilir. Dizin numarası en düşük olanlar ilk değerlendirilir. Örneğin dizin numarası 1 olan kural dizin numarası 2 olan kuraldan önce değerlendirilir. 

    * **İzin Verilen İstemciler**   
        Değeri belirtirken aşağıdaki biçimlerden birini kullanın:  
        * IPv4 adresi, örneğin `10.1.12.24` 
        * Bit numaraları olarak belirtilen alt ağ maskesine sahip IPv4 adresi, örneğin `10.1.12.10/4`

    * **Erişim**  
        Aşağıdaki erişim türlerinden birini seçin:  
        * Erişim Yok 
        * Okuma ve Yazma
        * Salt Okunur

    * **Protokoller**   
        Dışarı aktarma ilkesinde kullanılacak protokolü belirtin.   
        Şu anda Azure NetApp Files yalnızca NFSv3'ü desteklemektedir.

    ![İlkeyi dışarı aktarma](../media/azure-netapp-files/azure-netapp-files-export-policy.png) 


## <a name="next-steps"></a>Sonraki adımlar 
* [Birimleri yönetme](azure-netapp-files-manage-volumes.md)
* [Sanal makineler için birimi bağlama veya ayırma](azure-netapp-files-mount-unmount-volumes-for-virtual-machines.md)
* [Anlık görüntüleri yönetme](azure-netapp-files-manage-snapshots.md)
