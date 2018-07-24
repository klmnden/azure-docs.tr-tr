---
title: Azure NetApp Files’ın depolama hiyerarşisini anlama | Microsoft Docs
description: Azure NetApp Files hesapları, kapasite havuzları ve birimleri de dahil olmak üzere depolama hiyerarşisi açıklanır.
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
ms.openlocfilehash: 6f5ed4e7ede9a098d69b7a40f44dd60f9b400472
ms.sourcegitcommit: e0a678acb0dc928e5c5edde3ca04e6854eb05ea6
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/13/2018
ms.locfileid: "39010999"
---
# <a name="understand-the-storage-hierarchy-of-azure-netapp-files"></a>Azure NetApp Files’ın depolama hiyerarşisini anlama

Azure NetApp Files’da birim oluşturmadan önce, sağlanan kapasite için bir havuz satın alıp ayarlamalısınız.  Kapasite havuzunu ayarlamak için NetApp hesabınız olmalıdır. Depolama hiyerarşisini anlamanız, Azure NetApp Files kaynaklarını ayarlamanıza ve yönetmenize yardımcı olur.

## <a name="azure_netapp_files_account"></a>NetApp hesapları

- NetApp hesabı, destekçi kapasite havuzlarının yönetim gruplandırması işlevi görür.  
- NetApp hesabı genel Azure depolama hesabınızla aynı değildir. 
- NetApp hesabı bölgesel kapsamdadır.   
- Bir bölgede birden çok NetApp hesabınız olabilir ama her NetApp hesabı tek bir bölgeye bağlıdır.

## <a name="capacity_pools"></a>Kapasite havuzları

- Kapasite havuzu sağlanan kapasitesiyle ölçülür.  
- Kapasite, satın aldığınız sabit SKU'lar tarafından sağlanır (örneğin, 4 TB kapasiteli).
- Kapasite havuzunda tek bir hizmet düzeyi bulunabilir.  
  Şu anda yalnızca Premium hizmet düzeyi vardır.
- Her kapasite havuzu tek bir NetApp hesabına aittir.  
- Kapasite havuzu NetApp hesapları arasında taşınamaz.   
  Örneğin, aşağıdaki [Depolama hiyerarşisi kavramsal diyagramında](#conceptual_diagram_of_storage_hierarchy) Capacity Pool 1 adlı kapasite havuzu US East NetApp hesabından US West 2 NetApp hesabına taşınamaz.  

## <a name="volumes"></a>Birimler

- Birimler, mantıksal kapasite kullanımıyla ölçülür ve birim başına 100 TB'a kadar ölçeklenebilir.
- Birimin kapasite kullanımı, havuzunun sağlanan kapasitesinden sayılır.
- Her birim tek bir havuza ait olsa da, bir havuzda birden çok birim olabilir. 
- Aynı NetApp hesabı içinde, birimi havuzlar arasında taşıyabilirsiniz.    
  Örneğin, aşağıdaki [Depolama hiyerarşisinin kavramsal diyagramında](#conceptual_diagram_of_storage_hierarchy) birimleri Capacity Pool 1'den Capacity Pool 2'ye taşıyabilirsiniz.

## <a name="conceptual_diagram_of_storage_hierarchy"></a>Depolama hiyerarşisinin kavramsal diyagramı 
Aşağıdaki örnekte Azure aboneliği, NetApp hesapları, kapasite havuzları ve birimlerin birbirleriyle ilişkileri gösterilir.   

![Depolama hiyerarşisinin kavramsal diyagramı](../media/azure-netapp-files/azure-netapp-files-storage-hierarchy.png)

## <a name="next-steps"></a>Sonraki adımlar

1. [NetApp hesabı oluşturma](azure-netapp-files-create-netapp-account.md)
2. [Kapasitesi havuzunu ayarlama](azure-netapp-files-set-up-capacity-pool.md)
3. [Azure NetApp Files için birim oluşturma](azure-netapp-files-create-volumes.md)
4. [Birim için dışarı aktarma ilkesini yapılandırma (isteğe bağlı)](azure-netapp-files-configure-export-policy.md)