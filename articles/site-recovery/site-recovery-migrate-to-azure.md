---
title: "Site Recovery ile Azure’a Geçiş | Microsoft Belgeleri"
description: "Bu makale, VM’leri ve fiziksel sunucuları Azure Site Recovery kullanarak Azure’a geçirme süreci hakkında genel bakış sağlar"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: jwhit
editor: 
ms.assetid: c413efcd-d750-4b22-b34b-15bcaa03934a
ms.service: site-recovery
ms.workload: backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 01/04/2017
ms.author: raynew
translationtype: Human Translation
ms.sourcegitcommit: cfe4957191ad5716f1086a1a332faf6a52406770
ms.openlocfilehash: 981155c38bdc8cb54639d2271be1f3bd3036125c
ms.lasthandoff: 03/09/2017


---
# <a name="migrate-to-azure-with-site-recovery"></a>Site Recovery ile Azure’a Geçiş

Sanal makineleri ve fiziksel sunucuları Azure Site Recovery hizmetini kullanarak geçirme hakkında genel bakış için bu makaleyi okuyun.

Site Recovery, şirket içi fiziksel sunucuların ve sanal makinelerin buluta (Azure) veya ikincil bir veri merkezine çoğaltılmasını düzenleyerek BCDR stratejinize katkı sağlayan bir Azure hizmetidir. Kesinti birincil konumunuzda meydana gelirse uygulamaları ve iş yüklerini kullanılabilir durumda tutmak için ikincil konuma yük devredersiniz. Normal çalışma koşullarına dönüldüğünde de yükü birincil konuma geri alabilirsiniz. [Site Recovery nedir?](site-recovery-overview.md) bölümünden daha fazla bilgi edinebilirsiniz. Bulut yolculuğunuzu hızlandırmak ve Azure tarafından sunulan çeşitli özelliklerden yararlanmak amacıyla Site Recovery’yi kullanarak da mevcut şirket içi iş yüklerinizi Azure’a geçirebilirsiniz.

Geçiş işlemini gerçekleştirmeye ilişkin hızlı bir genel bakış için lütfen bu videoya bakın.
>[!VIDEO https://channel9.msdn.com/Series/Azure-Site-Recovery/ASRHowTo-Video2-Migrate-Virtual-Machines-to-Azure/player]

Bu makale, [Azure portalında](https://portal.azure.com) dağıtımı açıklamaktadır. Var olan Site Recovery kasalarının bakımını yapmak için [klasik Azure portalı](https://manage.windowsazure.com/) kullanılabilir, ancak buradan yeni kasa oluşturamazsınız.

Yorumlarınızı bu makalenin altında paylaşabilirsiniz. Teknik sorular için [Azure Kurtarma Hizmetleri Forumu](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr)'nu kullanın.


## <a name="what-do-we-mean-by-migration"></a>Geçiş ile kast edilen nedir?

Şirket içi sanal makinelerin ve fiziksel sunucuların Azure’a veya ikincil bir konuma çoğaltılması için Site Recovery dağıtabilirsiniz. Makineleri çoğaltabilir, kesinti gerçekleştiğinde birincil konumdan devredebilir ve kurtarıldığında yeniden birincil siteye devredebilirsiniz. Buna ek olarak, VM’leri ve fiziksel sunucuları Azure’a geçirip kullanıcıların bunlara Azure VM’leri olarak erişmesini sağlamak üzere Site Recovery’yi kullanabilirsiniz. Geçiş, çoğaltma ve birincil konumdan Azure’a yük devrinin yanı sıra tam bir geçiş hareketi içerir.

## <a name="what-can-site-recovery-migrate"></a>Site Recovery nelerin geçişini yapabilir?

Şunları yapabilirsiniz:

- Şirket içi Hyper-V VM’leri, VMware VM’leri ve fiziksel sunucular üzerinde çalışan iş yüklerinin Azure VM’leri üzerinde çalışacak şekilde geçişini yapabilirsiniz. Ayrıca bu senaryoda tam çoğaltma ve bu yeniden çalışma gerçekleştirebilirsiniz.
- Bir Azure bölgesinden diğerine [Azure IaaS VM’lerini](site-recovery-migrate-azure-to-azure.md) geçirebilirsiniz. Şu anda bu senaryoda yalnızca geçiş desteklenmektedir ve yeniden çalışma desteği yoktur.
- [AWS Windows örneklerini](site-recovery-migrate-aws-to-azure.md) Azure IaaS VM’lerine geçirebilirsiniz. Şu anda bu senaryoda yalnızca geçiş desteklenmektedir ve yeniden çalışma desteği yoktur.

## <a name="migrate-on-premises-vms-and-physical-servers"></a>Şirket içi VM’leri ve fiziksel sunucuları geçirme

Şirket içi Hyper-V VM’leri, VMware VM’leri ve fiziksel sunucuları geçirmek için normal çoğaltma için kullanılan adımlara yakın bir süreç izlersiniz. Kurtarma Hizmetleri kasası kurar, gerekli yönetim sunucularını yapılandırır (geçirmek istediğiniz nesneye göre), bunları kasaya ekler ve çoğaltma ayarlarını belirlersiniz. Geçirmek istediğiniz makinelerde çoğaltmayı etkinleştirir ve her şeyin düzgün çalıştığından emin olmak için hızlı bir yük devretme testi yaparsınız.

Çoğaltma ortamınızın çalıştığını doğruladıktan sonra senaryonuz için [desteklenen özelliklere](site-recovery-failover.md) göre planlanmış veya planlanmamış yük devretme seçeneğini kullanırsınız. Geçiş için yük devretme yapmanız gerekmez. Bunun yerine geçirmek istediğiniz her makine için **Geçişi Tamamla** seçeneğini belirlemeniz gerekir. **Geçişi Tamamla** işlemi, geçiş işlemini tamamlar, makine için çoğaltmayı kaldırır ve makinede Site Recovery faturalandırmasını durdurur.

![tamgeçiş](./media/site-recovery-hyper-v-site-to-azure/migrate.png)

## <a name="migrate-between-azure-regions"></a>Azure bölgeleri arasında geçiş yapma

Site Recovery kullanarak farklı bölgelerdeki Azure VM’leri arasında geçiş yapabilirsiniz. Bu senaryoda yalnızca geçiş desteklenir. Başka bir deyişle, Azure sanal makinelerini çoğaltıp başka bir bölgede yük devretme gerçekleştirebilirsiniz ancak yeniden çalışma özelliğini kullanamazsınız. Bu senaryoda bir Kurtarma Hizmetleri kasası kurar, çoğaltmayı yönetmek için şirket içi yapılandırma sunucusu dağıtır, kasaya ekler ve çoğaltma ayarlarını belirtirsiniz. Geçirmek istediğiniz makinelerde çoğaltmayı etkinleştirir ve hızlı bir yük devretme testi yaparsınız. Ardından, planlanmamış yük devretme çalıştırmak için **Geçişi Tamamla** seçeneğini belirlersiniz.

## <a name="migrate-aws-to-azure"></a>AWS örneklerini Azure’a geçirme

AWS örneklerini Azure VM’lerine geçirebilirsiniz. Bu senaryoda yalnızca geçiş desteklenir. Başka bir deyişle, AWS örneklerini çoğaltıp Azure’da yük devretme gerçekleştirebilirsiniz ancak yeniden çalışma özelliğini kullanamazsınız. AWS örnekleri, geçiş işlemleri açısından fiziksel sunucularla aynı şekilde işlenir. Bir Kurtarma Hizmetleri kasası kurar, çoğaltmayı yönetmek için şirket içi yapılandırma sunucusu dağıtır, kasaya ekler ve çoğaltma ayarlarını belirtirsiniz. Geçirmek istediğiniz makinelerde çoğaltmayı etkinleştirir ve hızlı bir yük devretme testi yaparsınız. Ardından, planlanmamış yük devretme çalıştırmak için **Geçişi Tamamla** seçeneğini belirlersiniz.




## <a name="next-steps"></a>Sonraki adımlar

- [VMware VM’lerini Azure’a geçirme](site-recovery-vmware-to-azure.md)
- [VMM bulutlarındaki Hyper-V VM’lerini Azure’a geçirme](site-recovery-vmm-to-azure.md)
- [Hyper-V sanal makinelerini (VMM olmadan) Azure’a geçirme](site-recovery-hyper-v-site-to-azure.md)
- [Azure VM’lerini bir Azure bölgesinden diğerine geçirme](site-recovery-migrate-azure-to-azure.md)
- [AWS örneklerini Azure’a geçirme](site-recovery-migrate-aws-to-azure.md)

