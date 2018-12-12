---
title: Azure İzleyici VM'ler (Önizleme) nedir? | Microsoft Docs
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
ms.date: 11/07/2018
ms.author: magoedte
ms.openlocfilehash: c7d2004da52d83ceda62dc31583797d9a218ef48
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/08/2018
ms.locfileid: "53085460"
---
# <a name="what-is-azure-monitor-for-vms-preview"></a>Azure İzleyici VM'ler (Önizleme) nedir?

Azure sanal makinelerinizi (VM) sanal makineler için Azure İzleyici izler ve uygun ölçekte sanal makine ölçek kümeleri. Hizmet, Windows ve Linux Vm'leri, işlemlerini ve diğer kaynakları ve işlemleri dış bağımlılıkları izleme durumunu ve performansını analiz eder. 

VM'ler için Azure İzleyici, bir çözüm olarak, başka bir bulut sağlayıcısında veya şirket içi VM'ler için performans ve uygulama bağımlılıklarını izlemek için destek içerir. Üç anahtar özellik kapsamlı Öngörüler sunun:

* **Windows ve Linux çalıştıran Azure VM'lerin mantıksal bileşenler**: önceden yapılandırılmış bir sistem durumu ölçütlerine göre ölçülür ve değerlendirilen koşul karşılandığında, sizi uyarır.  

* **Performans grafiklerini popüler önceden tanımlanmış**: Konuk VM işletim sisteminden temel performans ölçümlerini görüntüleyin.

* **Bağımlılık Haritası**: çeşitli kaynak gruplarında ve Aboneliklerde VM'den birbirine bağlı bileşenlerle görüntüler.  

Özellikler, üç Perspektifler düzenlenmiştir:

* Durum
* Performans
* Eşleme

>[!NOTE]
>Şu anda, sistem durumu özelliği yalnızca Azure sanal makineler ve sanal makine ölçek kümeleri sunulur. Performans ve harita özellikleri, ortamınızda veya diğer bulut sağlayıcısı Azure Vm'leri hem de barındırılan sanal makinelerin destekler.

Log Analytics ile tümleştirme, güçlü toplama ve filtreleme sunar ve zaman içinde veri eğilimlerini çözümleyebilirsiniz. Kapsamlı iş yükü izleme gibi Azure İzleyici, hizmet eşlemesi ya da yalnızca Log Analytics ile elde edilemeyecek.  

Bu veriler tek bir sanal makinede sanal makineden doğrudan görüntüleyebileceğiniz veya sanal makinelerinizin toplu bir görünümünü sunmak için Azure İzleyicisi'ni kullanabilirsiniz. Bu görünüm, her özelliğin perspektif dayanır:

* **Sistem durumu**: VM'ler, bir kaynak grubuna ilişkilidir.
* **Harita** ve **performans**: Vm'leri belirli bir Log Analytics çalışma alanına rapor şekilde yapılandırılır.

![Azure portalında sanal makine ınsights perspektifi](./media/vminsights-overview/vminsights-azmon-directvm-01.png)

Azure DevOps, tahmin edilebilir performans ve kullanılabilirliği önemli uygulamalara teslim edebilirsiniz. Bu kritik işletim sistemi olayları, performans sorunlarını ve ağ sorunları tanımlar. Azure DevOps bir sorun için diğer bağımlılıkları ilişkili olup olmadığını anlamanıza da yardımcı olabilir.  

## <a name="data-usage"></a>Veri kullanımı 

VM'ler için Azure İzleyici'yi dağıttığınızda, sanal makineleriniz tarafından toplanan verileri alınır ve Azure İzleyici'de depolanan. Yayımlanan fiyatlandırmaya göre [Azure fiyatlandırma sayfasını İzleyici](https://azure.microsoft.com/pricing/details/monitor/), VM'ler için Azure İzleyici için faturalandırılır:
* Alınan ve depolanan veriler.
* Zaman serisi, izlenen sistem durumu ölçütlerini ölçüm sayısı.
* Oluşturulan uyarı kuralları.
* Gönderilen bildirimleri. 

Sayaçları dize uzunluğu günlük boyutunu değişir ve mantıksal diskler ve ağ bağdaştırıcıları sayısıyla artırabilirsiniz. Zaten bir çalışma alanı varsa ve bu sayaçları toplamak, yinelenen herhangi bir ücret uygulanır. Hizmet eşlemesi zaten kullanıyorsanız, gördüğünüz tek değişiklik, Azure İzleyici gönderilen ek bağlantı verilerdir.

## <a name="next-steps"></a>Sonraki adımlar
Sanal makinelerinizi izlemenize yardımcı yöntemler ve gereksinimleri hakkında bilgilere [VM'ler için Azure İzleyici'ı Dağıtma](vminsights-onboard.md).
