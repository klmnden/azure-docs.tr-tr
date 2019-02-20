---
title: NetApp Azure dosyaları için ölçümleri | Microsoft Docs
description: Ölçümler, Azure için NetApp dosyaları açıklar.
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
ms.topic: concepts
ms.date: 02/15/2019
ms.author: b-juche
ms.openlocfilehash: 866aa808f4706fa3bce72495dc56f438d567ecb2
ms.sourcegitcommit: 9aa9552c4ae8635e97bdec78fccbb989b1587548
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/20/2019
ms.locfileid: "56430797"
---
# <a name="metrics-for-azure-netapp-files"></a>NetApp Azure dosyaları için ölçümleri

Azure NetApp dosyaları ayrılan depolama alanını, gerçek depolama alanı kullanımı, birim aktarım hızı, IOPS ve gecikme süresi ölçümlerini sağlar. Bu ölçümleri analiz ederek daha iyi anlamak kullanım desenini ve toplu performansını NetApp hesaplarınız elde edebilirsiniz.  

## <a name="capacity_pools"></a>Kapasite havuzları için kullanım ölçümleri

- *Birim ayrılmamış havuz boyutu*  
    Sağlanan kapasitesi havuzu boyutu (GiB) budur.  
- *Ayrılmış birim havuzu kullanılan*  
    Bu birimi kotası (GiB) verilen kapasite havuzunda (diğer bir deyişle, sağlanan boyutları kapasitesi havuzu birimlerinin toplam) toplamıdır. Bu, birim oluşturma sırasında seçtiğiniz boyutudur.  
- *Birim havuzu toplam mantıksal boyutu*  
    Kapasite havuzundaki birimler arasında kullanılan mantıksal alanı (GiB) toplamıdır.  
- *Birim havuzu toplam anlık görüntü boyutu*  
    Bu anlık görüntüler görüntülenerek kullanılan artımlı mantıksal alanı toplamıdır.  

## <a name="volumes"></a>Birimler için kullanım ölçümleri

- *Boyutu ayrılmış birim*   
    Birim boyutu (kota) cinsinden sağlanan budur.  
- *Mantıksal birim boyutu*   
    Bir birim (GiB) kullanılan toplam mantıksal alanı budur. Bu boyut etkin dosya sistemleri ve anlık görüntüleri tarafından kullanılan mantıksal alanı içerir.  
- *Birim anlık görüntü boyutu*   
    Bir birim anlık görüntü tarafından kullanılan artımlı mantıksal alanı budur.  

## <a name="next-steps"></a>Sonraki adımlar

* [NetApp dosyaları Azure depolama hiyerarşisini anlama](azure-netapp-files-understand-storage-hierarchy.md)
* [Kapasitesi havuzunu ayarlama](azure-netapp-files-set-up-capacity-pool.md)
* [Azure NetApp Files için birim oluşturma](azure-netapp-files-create-volumes.md)
