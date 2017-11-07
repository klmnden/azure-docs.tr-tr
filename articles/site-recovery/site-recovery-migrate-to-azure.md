---
title: "Site Recovery ile şirket içi VM’leri ve fiziksel sunucuları Azure’a geçirme | Microsoft Docs"
description: "Bu makale, şirket içi VM’leri ve fiziksel sunucuları Azure Site Recovery ile Azure’a geçirme işlemini açıklar"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: c413efcd-d750-4b22-b34b-15bcaa03934a
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 10/30/2017
ms.author: raynew
ms.openlocfilehash: 423a1727efb0e1fd54eb0f8d5971ace3f8efc6cb
ms.sourcegitcommit: 43c3d0d61c008195a0177ec56bf0795dc103b8fa
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/01/2017
---
# <a name="migrate-to-azure-with-site-recovery"></a>Site Recovery ile Azure’a Geçiş

[Azure Site Recovery](site-recovery-overview.md) hizmetini kullanarak şirket içi sanal makineleri (VM) ve fiziksel sunucuları Azure VM’lerine geçirme hakkında bilgi edinmek için bu makaleyi okuyun.

## <a name="before-you-start"></a>Başlamadan önce

Azure'a geçiş yapmak için gereken adımlara hızlı bir genel bakış için bu videoyu izleyin.
>[!VIDEO https://channel9.msdn.com/Series/Azure-Site-Recovery/ASRHowTo-Video2-Migrate-Virtual-Machines-to-Azure/player]


## <a name="what-do-we-mean-by-migration"></a>Geçiş ile kast edilen nedir?

Site Recovery’yi şirket içi VM’leri ve fiziksel sunucuları çoğaltmak ve geçirmek için dağıtabilirsiniz.

- Çoğalttığınızda, şirket içi makineleri düzenli olarak Azure’a çoğaltılacak şekilde yapılandırabilirsiniz. Daha sonra bir kesinti oluştuğunda, makineleri şirket içi siteden Azure’a devredebilir ve makinelere oradan erişebilirsiniz. Şirket içi siteniz yeniden kullanılabilir olduğunda Azure’dan yeniden çalıştırabilirsiniz.
- Geçiş için Site Recovery kullandığınızda şirket içi makineleri Azure’a çoğaltabilirsiniz. Daha sonra şirket içi sitenizden Azure’da yeniden çalıştırabilir ve geçiş işlemini tamamlayabilirsiniz. Yeniden çalışma işlemi yapılmaz.  

## <a name="what-can-site-recovery-migrate"></a>Site Recovery nelerin geçişini yapabilir?

Şunları yapabilirsiniz:

- **Şirket içinden geçiş**: Şirket içi Hyper-V VM’lerini, VMware VM’lerini ve fiziksel sunucuları Azure’a geçirin. Geçişten sonra şirket içi makinelerde çalışan iş yükleri Azure VM'ler üzerinde çalışacaktır. 
- **Azure içinde geçiş**: Azure VM’lerini bir Azure bölgesinden diğerine geçirin. 
- **AWS geçirme**: AWS Windows örneklerini Azure IaaS’ye geçirin. 

## <a name="migrate-from-on-premises-to-azure"></a>Şirket içinden Azure'a geçirme

Şirket içi VMware VM’leri, Hyper-V VM’leri ve fiziksel sunucuları geçirirken tam çoğaltma için kullanılan adımların neredeyse aynısını izlersiniz. 


## <a name="migrate-between-azure-regions"></a>Azure bölgeleri arasında geçiş yapma

Azure VM'leri bir bölgeden diğerine geçirirken tam geçiş için kullanılan adımların neredeyse aynısını izlersiniz.

1. Geçirmek istediğiniz makineler için [çoğaltmayı etkinleştirirsiniz](azure-to-azure-tutorial-enable-replication.md).
2. Her şeyin çalıştığından emin olmak için bir [hızlı yük devretme testi çalıştırırsınız](azure-to-azure-tutorial-dr-drill.md).
3. Ardından, **Geçişi Tamamla** seçeneği ile [planlanmamış yük devretme çalıştırırsınız](azure-to-azure-tutorial-failover-failback.md).
4. Geçişi tamamladıktan sonra, geçiş yaptığınız Azure bölgesinden ikincil bir bölgeye [olağanüstü durum kurtarma için çoğaltmayı ayarlayabilirsiniz](site-recovery-azure-to-azure-after-migration.md).



## <a name="migrate-aws-to-azure"></a>AWS örneklerini Azure’a geçirme

AWS örneklerini Azure VM’lerine geçirebilirsiniz.
- Bu senaryoda yalnızca geçiş desteklenir. Başka bir deyişle, AWS örneklerini çoğaltıp Azure’da yük devretme gerçekleştirebilirsiniz ancak yeniden çalışma özelliğini kullanamazsınız.
- AWS örnekleri, geçiş işlemleri açısından fiziksel sunucularla aynı şekilde işlenir. Bir Kurtarma Hizmetleri kasası kurar, çoğaltmayı yönetmek için şirket içi yapılandırma sunucusu dağıtır, kasaya ekler ve çoğaltma ayarlarını belirtirsiniz.
- Geçirmek istediğiniz makinelerde çoğaltmayı etkinleştirir ve hızlı bir yük devretme testi yaparsınız. Ardından, planlanmamış yük devretme çalıştırmak için **Geçişi Tamamla** seçeneğini belirlersiniz.






## <a name="next-steps"></a>Sonraki adımlar

- [Şirket içi makineleri Azure’a geçirme](tutorial-migrate-on-premises-to-azure.md)
- [VM’leri bir Azure bölgesinden diğerine geçirme](site-recovery-migrate-azure-to-azure.md)
- [AWS’yi Azure'a geçirme](tutorial-migrate-aws-to-azure.md)
