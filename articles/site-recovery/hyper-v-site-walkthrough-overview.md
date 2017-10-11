---
title: "Hyper-V sanal makineleri Azure Site Recovery ile Azure'a çoğaltma | Microsoft Docs"
description: "Çoğaltma, yük devretme ve şirket içi Hyper-V sanal makineleri kurtarma Azure düzenlendiğini açıklar"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: efddd986-bc13-4a1d-932d-5484cdc7ad8d
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 06/21/2017
ms.author: raynew
ms.openlocfilehash: da10b213bc2543942b5ac77cf5c5d8547c00220c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="replicate-hyper-v-virtual-machines-without-vmm-to-azure"></a>Hyper-V sanal makinelerini (VMM olmadan) Azure'a çoğaltma 

> [!div class="op_single_selector"]
> * [Azure portal](site-recovery-hyper-v-site-to-azure.md)
> * [Azure klasik](site-recovery-hyper-v-site-to-azure-classic.md)
> * [PowerShell - Resource Manager](site-recovery-deploy-with-powershell-resource-manager.md)
>
>

Bu makalede Azure için şirket içi Hyper-V sanal makineleri çoğaltmak için gerekli olan adımları genel bir bakış sağlar kullanarak [Azure Site Recovery](site-recovery-overview.md) Azure portalında. Bu dağıtım Hyper-V VM System Center Virtual Machine Manager (VMM) tarafından yönetilmiyor.


Bu makaleyi okuduktan sonra altındaki bir yorum gönderin ya da teknik sorular [Azure kurtarma Hizmetleri Forumu](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


## <a name="step-1-review-architecture-and-prerequisites"></a>1. adım: mimarisi ve önkoşulları gözden geçirin

Dağıtıma başlamadan önce senaryo mimarisinin gözden geçirin ve dağıtmak için gereken tüm bileşenleri anladığınızdan emin olun

Git [1. adım: mimarisi gözden geçirin](hyper-v-site-walkthrough-architecture.md)


## <a name="step-2-review-prerequisites"></a>2. adım: Gözden geçirme önkoşulları

Önkoşullar her dağıtım bileşen için yerinde olduğundan emin olun:

- **Azure önkoşulları**: bir Microsoft Azure hesabı, Azure ağları ve depolama hesapları gerekir.
- **Şirket içi Hyper-V önkoşulları**: Hyper-V konakları Site Recovery dağıtımı için hazır olduğundan emin olun.
- **Çoğaltılan VM'ler**: çoğaltmak istediğiniz sanal makineleri gereken Azure gereksinimlerine uymak.

Git [2. adım: Önkoşullar ve sınırlamalar doğrulayın](hyper-v-site-walkthrough-prerequisites.md)

## <a name="step-3-plan-capacity"></a>3. adım: Planı kapasite

Tam dağıtımını yaptığınız ihtiyacınız hangi çoğaltma kaynakları şekil gerekir. Birkaç bu yapmanıza yardımcı olmak için kullanılabilen araçlar vardır. 2. adıma geçin. Hızlı kümesi yaptığınız yukarı test ortamı için bu adımı atlayabilirsiniz.

[3. Adım: Kapasiteyi planlama](hyper-v-site-walkthrough-capacity.md)’ya gidin.

## <a name="step-4-plan-networking"></a>4. adım: ağ planlama

Bazı ağ yük devretme gerçekleştikten sonra sahip oldukları doğru IP adreslerini Azure Vm'lerinin ağlara bağlandığından emin olmak planlama yapmanız gerekir.

Git [4. adım: ağ planı](hyper-v-site-walkthrough-network.md)

##  <a name="step-5-prepare-azure-resources"></a>5. adım: Azure kaynaklarını hazırlama

Başlamadan önce Azure ağları ve depolama ayarlayın. Dağıtım sırasında bunu yapabilirsiniz ancak başlamadan önce bunu öneririz.

Git [5. adım: Azure hazırlama](hyper-v-site-walkthrough-prepare-azure.md)


## <a name="step-6-prepare-hyper-v"></a>6. adım: Hyper-V hazırlama

Hyper-V sunucuları Site Recovery dağıtım gereksinimleri karşıladığından emin olun.

Git [6. adım: Hyper-V hazırlama](hyper-v-site-walkthrough-prepare-hyper-v.md)

## <a name="step-7-set-up-a-vault"></a>7. adım: Bir kasa ayarlama

Düzenlemek ve çoğaltmayı yönetmek için bir kurtarma Hizmetleri kasasını ayarlamanız gerekir. Kasasını ayarladığınızda, çoğaltmak istediğiniz ve kendisine çoğaltmak istediğiniz yeri belirtin.

Git [adım 7: bir kasa oluşturun](hyper-v-site-walkthrough-create-vault.md)

## <a name="step-8-configure-source-and-target-settings"></a>8. adım: kaynak ve hedef ayarlarını yapılandırma

Kaynak ve çoğaltma için kullanılan hedef ayarlayın. Kaynak ayarları kurma, bir Hyper-V sitesi için Site Recovery sağlayıcısı ve kurtarma Hizmetleri Aracısı her Hyper-V ana bilgisayarda yükleme ve site kurtarma Hizmetleri kasaya kaydetmeyi Hyper-V konakları eklemeyi içerir.

Git [adım 8: kaynak ve hedef ayarlama](hyper-v-site-walkthrough-source-target.md)

## <a name="step-9-set-up-a-replication-policy"></a>9. adım: Bir çoğaltma ilkesi ayarlama

Hyper-V sanal makineleri için çoğaltma ayarlarını kasaya belirtmek için bir ilke ayarlayın.

Git [adım 9: bir çoğaltma ilkesini ayarlayın](hyper-v-site-walkthrough-replication.md)


## <a name="step-10-enable-replication"></a>10. adım: Çoğaltma etkinleştirme

Etkinleştirildikten sonra bir çoğaltma ilkesi yerinde aldıktan sonra VM başlangıç çoğaltması gerçekleşir.

Git [adım 10: çoğaltmasını etkinleştir](hyper-v-site-walkthrough-enable-replication.md)

## <a name="step-11-run-a-test-failover"></a>11. adım: yük devretme testi çalıştırma

İlk çoğaltma sonlandırıldıktan ve değişim çoğaltması çalıştıran sonra her şeyin beklendiği gibi çalıştığından emin olmak için yük devretme testi çalıştırabilirsiniz.

Git [11. adım: yük devretme testi çalıştırma](hyper-v-site-walkthrough-test-failover.md)
