---
title: Azure Cloud Services'ı etkileyen kesinti olması durumunda bir Azure yapmanız gerekenler servis | Microsoft Docs
description: Azure Cloud Services'ı etkileyen bir Azure hizmet kesintisi olması durumunda yapmanız gerekenler öğrenin.
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
ms.openlocfilehash: 4b355a779a2e9f78f4cbf8ed5425200ce1df2f1d
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60337276"
---
# <a name="what-to-do-in-the-event-of-an-azure-service-disruption-that-impacts-azure-cloud-services"></a>Azure Cloud Services'ı etkileyen kesinti olması durumunda bir Azure hizmet gerekenler
Microsoft'ta, sabit ihtiyaç duyduğunuzda hizmetlerimizin her zaman sizin için kullanılabilir olduğundan emin olmak için çalışıyoruz. Bize zorlar denetimimiz dışında bazen plansız bir hizmet kesintilerine neden şekillerde etkiler.

Microsoft olarak çalışma süresi ve bağlantı için bir taahhüt hizmetlerinin için bir hizmet düzeyi sözleşmesi (SLA) sağlar. Tek tek Azure hizmetlerinin SLA'sı şu yolda bulunabilir: [Azure hizmet düzeyi sözleşmeleri](https://azure.microsoft.com/support/legal/sla/).

Azure, yüksek kullanılabilirliğe sahip uygulamalar destekleyen birçok yerleşik platform özellikleri zaten var. Bu hizmetler hakkında daha fazla bilgi için okuma [olağanüstü durum kurtarma ve Azure uygulamaları için yüksek kullanılabilirlik](../resiliency/resiliency-disaster-recovery-high-availability-azure-applications.md).

Bu makale, bir tam bölge ana doğal afet veya yaygın hizmet kesintisi nedeniyle kesinti yaşandığında gerçek bir olağanüstü durum kurtarma senaryosuna kapsar. Nadir oluşum bunlar, ancak tüm bir bölgenin kesinti olma olasılığını için hazırlamanız gerekir. Bir bölge tamamen bir hizmet kesintisi oluşursa, yerel olarak yedekli kopyalar, verilerinizin geçici olarak kullanılamaz. Coğrafi çoğaltma etkinleştirildiğinde, Azure depolama BLOB'ları ve tabloları üç ek kopya farklı bir bölgede depolanır. Tam bölgesel bir kesinti veya birincil bölgenin kurtarılabilir değil bir olağanüstü durum olması durumunda, Azure DNS girdilerini coğrafi olarak çoğaltılmış bölgeye yeniden eşlemesi.

> [!NOTE]
> Bu işlem üzerinde herhangi bir denetim yok ve yalnızca veri merkezi genelinde hizmet kesintileri için meydana gelir unutmayın. Bu nedenle, aynı zamanda yüksek düzeyde kullanılabilirlik elde etmek için diğer uygulamaya özgü yedekleme stratejiler hakkında durumda. Daha fazla bilgi için [Microsoft Azure üzerinde derlenen uygulamalar için yüksek kullanılabilirlik ve olağanüstü durum kurtarma](../resiliency/resiliency-disaster-recovery-high-availability-azure-applications.md). Kendi yük devretme etkileyen yapmak istiyorsanız, kullanımını düşünmek isteyebilirsiniz [okuma erişimli coğrafi olarak yedekli depolama (RA-GRS)](../storage/common/storage-redundancy-grs.md#read-access-geo-redundant-storage), başka bir bölgede verilerinizin salt okunur bir kopyasını oluşturur.
>
>


## <a name="option-1-use-a-backup-deployment-through-azure-traffic-manager"></a>1. seçenek: Azure Traffic Manager aracılığıyla yedekleme dağıtımı kullanın
Uygulamanızın farklı bölgelerdeki birden fazla dağıtım bakımını yapma ve ardından kullanarak en güçlü olağanüstü durum kurtarma çözümü gerektirir [Azure Traffic Manager](../traffic-manager/traffic-manager-overview.md) arasındaki trafiği yönlendirmek için. Azure Traffic Manager, birden çok sağlar [yönlendirme yöntemleri](../traffic-manager/traffic-manager-routing-methods.md), böylece bir birincil/yedekleme modeli kullanarak dağıtımlarınızı yönetmek mi, yoksa aralarındaki trafik bölme seçebilirsiniz.

![Azure Cloud Services, Azure Traffic Manager ile bölgeye Dengeleme](./media/cloud-services-disaster-recovery-guidance/using-azure-traffic-manager.png)

Bir bölge kaybı için en hızlı yanıt için Traffic Manager'ın yapılandırdığınız önemli [uç nokta izleme](../traffic-manager/traffic-manager-monitoring.md).

## <a name="option-2-deploy-your-application-to-a-new-region"></a>2. seçenek: Uygulamanızı yeni bir bölgeye dağıtın
Önceki seçeneği açıklandığı gibi birden çok etkin dağıtımlara koruma ek devam eden maliyetler doğurur. Kurtarma süresi hedefini (RTO) esnektir ve özgün koda veya derlenmiş bulut Hizmetleri paketi varsa, başka bir bölgede uygulamanızın yeni bir örneğini oluşturun ve DNS kayıtlarınızı yeni dağıtımına işaret edecek şekilde güncelleştirin.

Bir bulut hizmeti uygulaması oluşturma ve dağıtma konusunda daha fazla ayrıntı için [bir bulut hizmeti oluşturma ve dağıtma konusunda](cloud-services-how-to-create-deploy-portal.md).

Uygulama veri kaynaklarınıza bağlı olarak, uygulama veri kaynağı için kurtarma prosedürleri denetlemeniz gerekebilir.

* Azure depolama veri kaynakları için bkz: [Azure depolama çoğaltma](../storage/common/storage-redundancy-grs.md#read-access-geo-redundant-storage) denetlemek için kullanılabilen seçenekler uygulamanız için seçtiğiniz çoğaltma modeli temel.
* SQL veritabanı kaynakları için okuma [genel bakış: SQL veritabanı ile iş sürekliliği ve veritabanı olağanüstü durum kurtarma bulut](../sql-database/sql-database-business-continuity.md) denetlemek için kullanılabilen seçenekler uygulamanız için seçilen çoğaltma modeli temel.


## <a name="option-3-wait-for-recovery"></a>Seçenek 3: Kurtarma işleminin tamamlanmasını bekleyip
Bu durumda, sizin herhangi bir eylemi gerekli değildir, ancak hizmetinizi bölge geri yüklenene kadar kullanılamaz. Geçerli hizmet durumunu görebilirsiniz [Azure hizmet durumu Panosu](https://azure.microsoft.com/status/).

## <a name="next-steps"></a>Sonraki adımlar
Bir olağanüstü durum kurtarma ve yüksek kullanılabilirlik stratejisinin gerçekleştirme hakkında daha fazla bilgi için bkz: [olağanüstü durum kurtarma ve Azure uygulamaları için yüksek kullanılabilirlik](../resiliency/resiliency-disaster-recovery-high-availability-azure-applications.md).

Bir bulut platformunun özelliklerinden ayrıntılı teknik bir anlayış geliştirmek için bkz: [Azure dayanıklılık teknik Kılavuzu](../resiliency/resiliency-technical-guidance.md).