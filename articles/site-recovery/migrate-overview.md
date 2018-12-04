---
title: Şirket içi makinelerin ve Azure Vm'leri Azure Site Recovery geçişi hakkında | Microsoft Docs
description: Bu makalede, şirket içi ve Azure Iaas Vm'leri Azure Site Recovery hizmetini kullanarak Azure'a geçirmek açıklar.
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 11/27/2018
ms.author: raynew
ms.openlocfilehash: 9a5c45b66e775ce83625e1f6d9507aaf456c7c7b
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2018
ms.locfileid: "52843997"
---
# <a name="about-migration"></a>Geçiş hakkında

Nasıl hızlı bir genel bakış için bu makaleyi okuyun [Azure Site Recovery](site-recovery-overview.md) hizmet makineleri geçirmenizi yardımcı olur. 

İşte değiştirebilecekleriniz Site Recovery kullanarak geçirme:

- **Şirket içinden Azure'a geçirme**: şirket içi Hyper-V Vm'leri, VMware Vm'leri ve fiziksel sunucuları Azure'a geçirin. Geçişten sonra şirket içi makinelerde çalışan iş yükleri Azure VM'ler üzerinde çalışacaktır. 
- **Azure içinde geçiş**: Azure VM’lerini bir Azure bölgesinden diğerine geçirin. 
- **AWS geçirme**: AWS Windows örneklerini Azure IaaS’ye geçirin. 


## <a name="what-do-we-mean-by-migration"></a>Geçiş ile kast edilen nedir?

Site Recovery, şirket içi ve Azure Vm'lerinde olağanüstü durum kurtarma için kullanmanın yanı sıra bunları geçirmek için Site Recovery hizmetini kullanabilirsiniz. Fark nedir?

- Olağanüstü durum kurtarma için düzenli olarak makineleri Azure'a çoğaltabilirsiniz. Bir kesinti oluştuğunda, makineleri birincil siteden ikincil bir Azure sitesine yük devretme ve makinelere oradan erişebilirsiniz. Birincil sitenin yeniden kullanılabilir olduğunda, Azure'dan yeniden çalışma.
- Geçiş için şirket içi makineleri Azure'da veya Azure Vm'leri için ikincil bir bölgeye çoğaltma. Ardından VM birincil siteden ikincil veritabanına yük devretme ve geçiş işlemini tamamlayın. Yeniden çalışma işlemi yapılmaz.  


## <a name="migration-scenarios"></a>Geçiş senaryoları

**Senaryo** | **Ayrıntılar**
--- | ---
**Şirket içinden Azure'a geçirme** | Şirket içi VMware Vm'leri, Hyper-V Vm'leri ve fiziksel sunucuları Azure'a geçirebilirsiniz. Bunu yapmak için eksiksiz olağanüstü durum kurtarma için yaptığınız gibi neredeyse aynı adımları tamamlayın. Yalnızca azure'dan geri makineleri şirket içi siteye başarısız yok.
**Azure bölgeleri arasında geçiş yapma** | Azure Vm'leri bir Azure bölgesinden diğerine geçişini yapabilirsiniz. Geçiş tamamlandıktan sonra artık için geçişiniz ikincil bölgedeki Azure Vm'leri için olağanüstü durum kurtarmayı yapılandırabilirsiniz.
**AWS’yi Azure'a geçirme** | AWS örneklerini Azure VM’lerine geçirebilirsiniz. Site Recovery, AWS örneklerini geçiş işlemleri açısından fiziksel sunucularla değerlendirir. 

## <a name="next-steps"></a>Sonraki adımlar

- [Şirket içi makineleri Azure’a geçirme](migrate-tutorial-on-premises-azure.md)
- [VM’leri bir Azure bölgesinden diğerine geçirme](azure-to-azure-tutorial-migrate.md)
- [AWS’yi Azure'a geçirme](migrate-tutorial-aws-azure.md)
