---
title: "Azure Site kurtarma geçişi hakkında | Microsoft Docs"
description: "Bu makalede, şirket içi ve Azure Azure Site Recovery hizmetini kullanarak sanal makineleri nasıl geçirileceği açıklanmaktadır."
services: site-recovery
author: rayne-wiselman
ms.service: site-recovery
ms.topic: article
ms.date: 01/07/2018
ms.author: raynew
ms.openlocfilehash: 966d532a33626a8fcc3a3b93790d203aadfd81b4
ms.sourcegitcommit: 6fb44d6fbce161b26328f863479ef09c5303090f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/10/2018
---
# <a name="about-migration"></a>Geçiş hakkında

Nasıl hızlı bir genel bakış için bu makaleyi okuyun [Azure Site Recovery](site-recovery-overview.md) hizmet makineleri geçirmek yardımcı olur. 

İşte neler Site Recovery kullanarak geçirme:

- **Şirket içinden Azure'a geçirmek**: şirket içi Hyper-V sanal makineleri, VMware Vm'lerini ve fiziksel sunucuları Azure'a geçirmek. Geçişten sonra şirket içi makinelerde çalışan iş yükleri Azure VM'ler üzerinde çalışacaktır. 
- **Azure içinde geçiş**: Azure VM’lerini bir Azure bölgesinden diğerine geçirin. 
- **AWS geçirme**: AWS Windows örneklerini Azure IaaS’ye geçirin. 


## <a name="what-do-we-mean-by-migration"></a>Geçiş ile kast edilen nedir?

Şirket içi ve Azure Vm'leri olağanüstü durum kurtarma için site RECOVERY'yi kullanarak yanı sıra bunları geçirmek için Site Recovery hizmeti kullanabilirsiniz. Fark nedir?

- Olağanüstü durum kurtarma için düzenli olarak makinelerini Azure'a çoğaltma. Kesinti oluştuğunda makineler birincil siteden ikincil Azure siteye yük devri ve buradan erişin. Birincil sitenin yeniden kullanılabilir duruma geldiğinde, Azure'dan başarısız.
- Geçiş için şirket içi makineler Azure ya da Azure VM'ler için bir ikincil bölge'ye çoğaltılır. Daha sonra VM birincil siteden ikincil yük devri ve geçiş işlemi tamamlayın. Yeniden çalışma işlemi yapılmaz.  


## <a name="migration-scenarios"></a>Geçiş senaryoları

**Senaryo** | **Ayrıntılar**
--- | ---
**Şirket içinden Azure'a geçirme** | Şirket içi VMware sanal makineleri, Hyper-V sanal makineleri ve fiziksel sunucuları Azure'a geçirebilirsiniz. Bunu yapmak için tam olağanüstü durum kurtarma için olduğu gibi neredeyse aynı adımları tamamlayın. Yalnızca şirket içi siteye geri azure'dan makineler başarısız yok.
**Azure bölgeleri arasında geçiş yapma** | Azure sanal makineleri başka bir bir Azure bölgesinden geçirebilirsiniz. Geçiş işlemi tamamlandıktan sonra artık, geçirdiğiniz ikincil bölge içindeki Azure VM'ler için olağanüstü durum kurtarma yapılandırabilirsiniz.
**AWS’yi Azure'a geçirme** | AWS örneklerini Azure VM’lerine geçirebilirsiniz. Site Recovery AWS örnekleri için fiziksel sunucuları geçiş amacıyla olarak değerlendirir. 

## <a name="next-steps"></a>Sonraki adımlar

- [Şirket içi makineleri Azure’a geçirme](tutorial-migrate-on-premises-to-azure.md)
- [VM’leri bir Azure bölgesinden diğerine geçirme](tutorial-migrate-azure-to-azure.md)
- [AWS’yi Azure'a geçirme](tutorial-migrate-aws-to-azure.md)
