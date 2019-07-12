---
title: include dosyası
description: include dosyası
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 07/08/2019
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: b1287f9c7e946c7b4d035b2ad6301947ffad3cea
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2019
ms.locfileid: "67717129"
---
# <a name="azure-premium-storage-design-for-high-performance"></a>Azure premium Depolama: yüksek performans tasarımı

Bu makalede, Azure Premium depolama kullanan yüksek performanslı uygulamalar oluşturmak için yönergeler sağlar. Performansı en iyi uygulamaları, uygulamanız tarafından kullanılan teknolojiler için geçerli bir araya geldiğinde bu belgede sağlanan yönergeleri kullanabilirsiniz. Yönergeler göstermek için bu belge boyunca örnek olarak Premium depolama üzerinde çalışan SQL Server kullandınız.

Biz bu makalede depolama katmanı için performans senaryosu, ancak uygulama katmanına en iyi duruma getirmek gerekir. Örneğin, Azure Premium depolama bir SharePoint Çiftliğini barındırıyorsanız, veritabanı sunucusu en iyi duruma getirmek için bu makalede SQL Server örneklerinden kullanabilirsiniz. Ayrıca, SharePoint grubunun Web sunucusu ve uygulama sunucusu çoğu performansı elde etmek için iyileştirin.

Bu makalede, Azure Premium depolama üzerinde uygulama performansını iyileştirme hakkında sık sorulan sorular aşağıdaki yanıt yardımcı olur,

* Uygulama performansını ölçmek nasıl?  
* Neden beklenen yüksek performanslı görmediğinizden?  
* Hangi faktörlerin Premium depolama, uygulama performansınızı etkileyen?  
* Nasıl bu faktörlerin Premium depolama, uygulamanızın performansını etkileyen?  
* Nasıl, IOPS, bant genişliği ve gecikme süresi için en iyi duruma getirebilirsiniz?  

Premium depolama alanında çalışan iş yükleri yüksek performans duyarlı olduğu için özel olarak Premium depolama için bu yönergeleri sağladık. Uygun yerlerde örnek sağladık. Ayrıca standart depolama diskleri ile Iaas Vm'lerinde çalışan uygulamaları bu yönergeleri bazıları uygulayabilirsiniz.