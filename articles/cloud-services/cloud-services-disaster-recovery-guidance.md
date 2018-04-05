---
title: Azure Cloud Services etkiler kesintisi hizmet durumunda Azure yapmanız gerekenler | Microsoft Docs
description: Azure Cloud Services etkiler bir Azure hizmet kesintisi durumunda yapmanız gerekenler hakkında bilgi edinin.
services: cloud-services
documentationcenter: ''
author: mmccrory
manager: timlt
editor: ''
ms.assetid: e52634ab-003d-4f1e-85fa-794f6cd12ce4
ms.service: cloud-services
ms.workload: cloud-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/04/2017
ms.author: mmccrory
ms.openlocfilehash: 7028417c95aa6969793c00d0bb270c96e56164fb
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2018
---
# <a name="what-to-do-in-the-event-of-an-azure-service-disruption-that-impacts-azure-cloud-services"></a>Azure Cloud Services etkiler kesintisi olması durumunda bir Azure hizmet gerekenler
Microsoft, biz sabit gereksinim duyduğunuzda hizmetlerimizle her zaman için kullanılabilir olduğundan emin olmak için çalışır. Bizim denetim ötesinde zorlar bazen bize planlanmamış hizmet kesintilerine neden şekillerde etkiler.

Microsoft açık kalma süresi ve bağlantı için taahhüdü olarak hizmetlerinin için bir hizmet düzeyi sözleşmesi (SLA) sağlar. SLA tek tek Azure Hizmetleri için şu adreste bulunabilir: [Azure hizmet düzeyi sözleşmeleri](https://azure.microsoft.com/support/legal/sla/).

Azure, yüksek oranda kullanılabilir uygulamaları destekleyen birçok yerleşik platform özellikleri zaten var. Bu hizmetler hakkında daha fazla bilgi için okuma [Azure uygulamaları için yüksek kullanılabilirlik ve olağanüstü durum kurtarma](../resiliency/resiliency-disaster-recovery-high-availability-azure-applications.md).

Tüm bölge kesinti ana doğal afet ya da yaygın bir hizmet kesintisi nedeniyle yaşadığında bu makalede bir true olağanüstü durum kurtarma senaryosunda kapsar. Nadir oluşum bunlar, ancak tüm bölgesinin bir kesinti olma olasılığını için hazırlamanız gerekir. Tüm bir bölgeyi hizmet kesintisi yaşarsa, verilerinizin yerel olarak yedekli kopyasını geçici olarak kullanılamaz durumda olurdu. Coğrafi çoğaltma etkinleştirildiğinde, tabloları ve Azure Storage bloblarında üç ek kopyalarını farklı bir bölgede depolanır. Azure tam bölgesel bir kesintinin ya da bir olağanüstü durumda birincil bölge kurtarılabilir değil, tüm coğrafi olarak çoğaltılmış bölge için DNS girdilerini remaps.

> [!NOTE]
> Bu işlemi üzerinde hiçbir denetimi yoktur ve yalnızca veri merkezi çapında hizmet kesilmelerini meydana gelir unutmayın. Bu nedenle, aynı zamanda yüksek düzeyde kullanılabilirlik elde etmek için diğer uygulamaya özgü yedek bir stratejileri kullanmanız gerekir. Daha fazla bilgi için bkz: [Microsoft Azure üzerinde oluşturulan uygulamalar için yüksek kullanılabilirlik ve olağanüstü durum kurtarma](../resiliency/resiliency-disaster-recovery-high-availability-azure-applications.md). Kendi yük devretme etkileyen yapabilmek istiyorsanız kullanımını düşünmek isteyebilirsiniz [okuma erişimli coğrafi olarak yedekli depolama (RA-GRS)](../storage/common/storage-redundancy-grs.md#read-access-geo-redundant-storage), başka bir bölgede verilerinizin salt okunur bir kopyasını oluşturur.
>
>


## <a name="option-1-use-a-backup-deployment-through-azure-traffic-manager"></a>Seçenek 1: yedekleme dağıtım aracılığıyla Azure trafik Yöneticisi'ni kullanın
En güçlü olağanüstü durum kurtarma çözümü farklı bölgelerde, uygulamanızın birden çok dağıtım koruyarak ve ardından kullanarak içerir [Azure Traffic Manager](../traffic-manager/traffic-manager-overview.md) aralarındaki trafik yönlendirmek için. Azure Traffic Manager sağlayan birden çok [yönlendirme yöntemleri](../traffic-manager/traffic-manager-routing-methods.md), böylece birincil/yedekleme modelini kullanarak, dağıtımları yönetmek mi, yoksa aralarındaki trafik bölmek için seçebilirsiniz.

![Bölgelere Azure Traffic Manager ile Azure Cloud Services Dengeleme](./media/cloud-services-disaster-recovery-guidance/using-azure-traffic-manager.png)

Bir bölge kaybı için en hızlı yanıt için trafik Yöneticisi'nin yapılandırma önemli [uç nokta izleme](../traffic-manager/traffic-manager-monitoring.md).

## <a name="option-2-deploy-your-application-to-a-new-region"></a>Seçenek 2: yeni bir bölge uygulamanızı dağıtmak
Önceki seçenek açıklandığı gibi birden fazla etkin dağıtımlarını sürdürme ek devam eden maliyetler doğurur. Kurtarma süresi hedefi (RTO) yeterince esnektir ve özgün kod veya derlenmiş bulut Hizmetleri paket varsa, başka bir bölgede, uygulamanızın yeni bir örneğini oluşturun ve yeni dağıtım noktası için DNS kayıtlarını güncelleştirin.

Oluşturma ve bir bulut hizmeti uygulaması dağıtma hakkında daha fazla ayrıntı için [nasıl oluşturulacağı ve bir bulut hizmetinin dağıtılacağı](cloud-services-how-to-create-deploy-portal.md).

Uygulama veri kaynaklarınıza bağlı olarak, uygulama veri kaynağı için kurtarma yordamları denetlemeniz gerekebilir.

* Azure Storage veri kaynakları için bkz: [Azure Storage çoğaltma](../storage/common/storage-redundancy-grs.md#read-access-geo-redundant-storage) denetlemek için uygulamanız için seçtiğiniz çoğaltma modeli kullanılabilir olan seçenekler bağlı.
* SQL veritabanı kaynakları için okuma [genel bakış: Bulut iş devamlılığı ve veritabanı olağanüstü durum kurtarma SQL Database](../sql-database/sql-database-business-continuity.md) denetlemek için seçili çoğaltma modeli uygulamanız için kullanılabilir olan seçenekler bağlı.


## <a name="option-3-wait-for-recovery"></a>Seçenek 3: Kurtarma için bekleyin
Bu durumda, herhangi bir eyleminiz gereklidir, ancak bölge geri yüklenene kadar hizmet kullanılamaz. Geçerli Hizmet durumu görebilirsiniz [Azure hizmet sağlığı panosunu](https://azure.microsoft.com/status/).

## <a name="next-steps"></a>Sonraki adımlar
Bir olağanüstü durum kurtarma ve yüksek kullanılabilirlik stratejisini uygulama hakkında daha fazla bilgi için bkz: [Azure uygulamaları için yüksek kullanılabilirlik ve olağanüstü durum kurtarma](../resiliency/resiliency-disaster-recovery-high-availability-azure-applications.md).

Bir bulut platformun özellikleri ayrıntılı teknik bir anlayış geliştirmek için bkz: [Azure dayanıklılık teknik kılavuz](../resiliency/resiliency-technical-guidance.md).