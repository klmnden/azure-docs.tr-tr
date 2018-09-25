---
title: VM'ler için Azure İzleyici nedir? | Microsoft Docs
description: VM'ler için Azure İzleyici bir sistem durumunu ve uygulama bileşenleri ve diğer kaynaklarla ilgili bağımlılıkları otomatik olarak keşfetme yanı sıra Azure VM'nin işletim sistemi izleme performansını birleştirir ve iletişimi eşleyen bir Azure İzleyici özelliğidir. Bunlar arasında. Bu makalede, genel bir bakış sağlar.
services: azure-monitor
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: tysonn
ms.assetid: ''
ms.service: azure-monitor
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/24/2018
ms.author: magoedte
ms.openlocfilehash: 26fcc3eb78af53360cca57382b4c06b017f36c0e
ms.sourcegitcommit: cc4fdd6f0f12b44c244abc7f6bc4b181a2d05302
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/25/2018
ms.locfileid: "47063277"
---
# <a name="what-is-azure-monitor-for-vms"></a>VM'ler için Azure İzleyici nedir?

VM'ler için Azure İzleyici, Windows ve Linux Vm'leri, farklı işlemler ve diğer kaynakları ve dış işlemlere birbirine bağımlılıkları da dahil olmak üzere, sistem durumu ve performansı analiz ederek, uygun ölçekte Azure sanal makinelerinizi (VM) izler. Uygulama bağımlılıkları VM'ler için şirket içi veya başka bir bulut sağlayıcısı barındırılan ve çözüm, performans izleme için destek içerir.  Daha ayrıntılı bu öngörüleri sunmak için üç önemli özellikleri içerir:

* Değerlendirilen koşul karşılandığında mantıksal bileşenler Azure sanal makinelerinin Windows ve Linux işletim sistemi çalıştıran bir dizi önceden yapılandırılmış durumu ölçütlerini ve uyarılar üzerinde göre ölçülür.  
* İşlemci, bellek, disk ve ağ bağdaştırıcısının Konuk VM işletim sisteminin çekirdek performans ölçümleri toplanır ve popüler önceden tanımlı performans grafiklerde sunulur.
* Bağımlılık Haritası bulunan birbirine bağlı bileşenleri bu VM ile birden çok kaynak grupları ve abonelikler gösteriliyor.  

Bu özellikler, üç Perspektifler düzenlenmiştir:

* Durum
* Performans
* Eşleme

>[!NOTE]
>Şu anda, sistem durumu özelliği yalnızca Azure sanal makineler için sunulur.
>

Log Analytics ile tümleştirme, güçlü bir toplama, filtreleme ve zaman içinde eğilim analizi veri gerçekleştirme becerisi sunar. Kapsamlı izleme iş yüklerinizi tek başına Azure İzleyici, hizmet eşlemesi veya Log Analytics ile elde edemiyor.  

Bu veriler tek bir VM sanal makineden bağlamında doğrudan görüntüleyebileceğiniz veya isteğe bağlı olarak Azure İzleyici ile aşağıdaki açısından her bir özellik göre sanal makinelerinizin toplu bir görünümünü sunar:

* Durumu - bir kaynak grubu için ilgili sanal makineleri
* Harita ve performans - Vm'leri belirli bir Log Analytics çalışma alanına raporlama yapacak yapılandırılmış

![Sanal makine ınsights portalı açısından](./media/monitoring-vminsights-overview/vminsights-azmon-directvm-01.png)

DevOps etkili bir şekilde tahmin edilebilir performans ve önemli uygulamalarınızın kullanılabilirliğini, kritik işletim sistemi olayları ve performans sorunlarını, ağ sorunları tanımlayarak teslim eder ve bir sorun için diğer bağımlılıkları ilişkili olup olmadığını anlamak.  

## <a name="data-usage"></a>Veri kullanımı 

Kısa sürede, sanal makineler, sanal makineleriniz tarafından toplanan veriler için yerleşik Azure İzleyici alınır ve Azure İzleyici'de depolanan.  VM'ler için Azure İzleyici, veri alınan ve korunur, ölçüm zaman serisi izlenen, uyarı kuralları oluşturduğunuz, bildirimler, başına fiyatlandırma yayımlanan Azure izleme üzerinde gönderilen durumu ölçütü sayısı faturalandırılır [fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/monitor/)

Günlük boyutu üzerinde sayaçları dize uzunluğu göre değişir ve mantıksal diskler ve ağ bağdaştırıcılarının sayısını artırabilirsiniz.  Zaten bir çalışma alanı varsa ve bu sayaçlar toplama uygulanan yinelenen ücretlerine olmayacak.  Hizmet eşlemesi zaten kullanıyorsanız, gördüğünüz tek değişiklik, Azure İzleyici gönderilen ek bağlantı verilerdir.

## <a name="next-steps"></a>Sonraki adımlar
Gözden geçirme [sanal makineler için yerleşik Azure İzleyici](monitoring-vminsights-onboard.md) gereksinimleri ve yöntemler, sanal makinelerin izlemeyi etkinleştirmek için anlamak için.
