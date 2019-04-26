---
title: Şirket içi makinelerin ve Azure Vm'leri Azure Site Recovery geçişi hakkında | Microsoft Docs
description: Bu makalede, şirket içi ve Azure Iaas Vm'leri Azure Site Recovery hizmetini kullanarak Azure'a geçirmek açıklar.
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 03/18/2019
ms.author: raynew
ms.openlocfilehash: 856d03b1ecc1c7a3bd527eb265061f9a305d8f50
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60196057"
---
# <a name="about-migration"></a>Geçiş hakkında

Nasıl hızlı bir genel bakış için bu makaleyi okuyun [Azure Site Recovery](site-recovery-overview.md) hizmet makineleri geçirmenizi yardımcı olur. 

İşte değiştirebilecekleriniz Site Recovery kullanarak geçirme:

- **Şirket içinden Azure'a geçirme**: Şirket içi Hyper-V Vm'leri, VMware Vm'leri ve fiziksel sunucuları Azure'a geçirin. Geçişten sonra şirket içi makinelerde çalışan iş yükleri Azure VM'ler üzerinde çalışacaktır. 
- **Azure içinde geçiş**: Azure Vm'lerinin Azure bölgeleri arasında geçiş yapma. 
- **AWS geçirme**: AWS Windows örneklerini Azure Iaas'ye geçirin. 


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
