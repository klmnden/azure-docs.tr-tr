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
ms.topic: overview
ms.date: 04/03/2019
ms.author: b-juche
ms.openlocfilehash: 357bd5eac41b0da50a1d7035e8e8045a9f21144c
ms.sourcegitcommit: 031e4165a1767c00bb5365ce9b2a189c8b69d4c0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/13/2019
ms.locfileid: "59545531"
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
- Kapasite (örneğin, 4 TiB Kapasite) satın alınan sabit SKU'ları tarafından sağlanır.
- Kapasite havuzunda tek bir hizmet düzeyi bulunabilir.  
- Her kapasitesi havuzu, yalnızca bir NetApp hesabına ait olabilir. Ancak, bir NetApp hesabında birden fazla kapasitesi havuzu olabilir.  
- Kapasite havuzu NetApp hesapları arasında taşınamaz.   
  Örneğin, aşağıdaki [Depolama hiyerarşisi kavramsal diyagramında](#conceptual_diagram_of_storage_hierarchy) Capacity Pool 1 adlı kapasite havuzu US East NetApp hesabından US West 2 NetApp hesabına taşınamaz.  

## <a name="volumes"></a>Birimler

- Bir birimi mantıksal kapasite tüketimini ölçülür ve ölçeklenebilir. 
- Birimin kapasite kullanımı, havuzunun sağlanan kapasitesinden sayılır.
- Her birim tek bir havuza ait olsa da, bir havuzda birden çok birim olabilir. 
- Aynı NetApp hesabı içinde, birimi havuzlar arasında taşıyabilirsiniz.    
  Örneğin, aşağıdaki [Depolama hiyerarşisinin kavramsal diyagramında](#conceptual_diagram_of_storage_hierarchy) birimleri Capacity Pool 1'den Capacity Pool 2'ye taşıyabilirsiniz.

## <a name="conceptual_diagram_of_storage_hierarchy"></a>Depolama hiyerarşisinin kavramsal diyagramı 
Aşağıdaki örnekte Azure aboneliği, NetApp hesapları, kapasite havuzları ve birimlerin birbirleriyle ilişkileri gösterilir.   

![Depolama hiyerarşisinin kavramsal diyagramı](../media/azure-netapp-files/azure-netapp-files-storage-hierarchy.png)

## <a name="next-steps"></a>Sonraki adımlar

- [Azure NetApp Files için kaynak sınırları](azure-netapp-files-resource-limits.md)
- [NetApp Azure dosyaları için kaydolun](azure-netapp-files-register.md)
