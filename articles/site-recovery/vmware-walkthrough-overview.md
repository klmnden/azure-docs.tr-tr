---
title: "VMware Vm'lerini Azure Site Recovery ile Azure'a çoğaltma | Microsoft Docs"
description: "Azure'da VMware Vm'lerinde çalışan iş yükleri çoğaltmak için adımlara genel bir bakış sağlar."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: dab98aa5-9c41-4475-b7dc-2e07ab1cfd18
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2017
ms.author: raynew
ms.openlocfilehash: db6f5f95929503e82a529dba26b56af1edb0767f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="replicate-vmware-vms-to-azure-with-site-recovery"></a>VMware sanal makinelerini Site Recovery ile Azure'a çoğaltma

Bu makalede şirket içi VMware sanal makinelerini Azure'a çoğaltma için gereken adımlar genel bir bakış sağlar kullanarak [Azure Site Recovery](site-recovery-overview.md) Azure portalında hizmet.


![Dağıtım işlemi](./media/vmware-walkthrough-overview/vmware-to-azure-process.png)

**Şekil 1: Dağıtım işlemi özeti**

## <a name="step-1-review-architecture-and-prerequisites"></a>1. adım: mimarisi ve önkoşulları gözden geçirin

Dağıtıma başlamadan önce senaryo mimarisinin gözden geçirin ve dağıtmak için gereken tüm bileşenleri anladığınızdan emin olun

Git [1. adım: mimarisi gözden geçirin](vmware-walkthrough-architecture.md)


## <a name="step-2-review-prerequisites"></a>2. adım: Gözden geçirme önkoşulları

Önkoşullar her dağıtım bileşen için yerinde olduğundan emin olun:

- **Azure önkoşulları**: bir Microsoft Azure hesabı, Azure ağları ve depolama hesapları gerekir.
- **Şirket içi Site Recovery bileşenlerini**: şirket içi Site Recovery bileşenlerini çalıştıran bir makineye gerekir.
- **Şirket içi VMware Önkoşullar**: Site Recovery VMware sunucularını ve Vm'leri erişebilmesi için hesapları ayarlamanız gerekir.
- **Çoğaltılan VM'ler**: çoğaltmak istediğiniz sanal makineleri gereken Azure gereksinimlerine uymak ve Mobility hizmeti bileşeninin yüklü olması.

Git [2. adım: gözden Önkoşullar ve sınırlamalar](vmware-walkthrough-prerequisites.md)

## <a name="step-3-plan-capacity"></a>3. adım: Planı kapasite

Tam dağıtımını yaptığınız ihtiyacınız hangi çoğaltma kaynakları şekil gerekir. Birkaç bu yapmanıza yardımcı olmak için kullanılabilen araçlar vardır. 2. adıma geçin. Hızlı kümesi yaptığınız yukarı test ortamı için bu adımı atlayabilirsiniz.

[3. Adım: Kapasiteyi planlama](vmware-walkthrough-capacity.md)’ya gidin.

## <a name="step-4-plan-networking"></a>4. adım: ağ planlama

Bazı ağ yük devretme gerçekleştikten sonra sahip oldukları doğru IP adreslerini Azure Vm'lerinin ağlara bağlandığından emin olmak planlama yapmanız gerekir.

Git [4. adım: ağ planı](vmware-walkthrough-network.md)

##  <a name="step-5-prepare-azure-resources"></a>5. adım: Azure kaynaklarını hazırlama

Başlamadan önce Azure ağları ve depolama ayarlayın. Dağıtım sırasında bunu yapabilirsiniz ancak başlamadan önce bunu öneririz.

Git [5. adım: Azure hazırlama](vmware-walkthrough-prepare-azure.md)


## <a name="step-6-prepare-vmware"></a>6. adım: VMware hazırlama

Site Recovery için kullanacağınız hesaplar ayarlamanız gerekir:

- Sanal makineleri otomatik olarak algılamak için erişim VMware sanallaştırma sunucuları.
- Mobilite hizmetinin yüklenmesi için erişim VM'ler. Çoğaltmak istediğiniz her bir VM Mobility Hizmeti Aracısı çoğaltmayı etkinleştirmeden önce yüklü olması gerekir.

Git [6. adım: VMware hazırlama](vmware-walkthrough-prepare-vmware.md)

## <a name="step-7-set-up-a-vault"></a>7. adım: Bir kasa ayarlama

Düzenlemek ve çoğaltmayı yönetmek için bir kurtarma Hizmetleri kasasını ayarlamanız gerekir. Kasasını ayarladığınızda, çoğaltmak istediğiniz ve kendisine çoğaltmak istediğiniz yeri belirtin.

Git [adım 7: bir kasasını oluşturup](vmware-walkthrough-create-vault.md)

## <a name="step-8-configure-source-and-target-settings"></a>8. adım: kaynak ve hedef ayarlarını yapılandırma

Kaynak ve çoğaltma için kullanılan hedef ayarlayın. Kaynak ayarları ayarı birleşik şirket içi Site Recovery bileşenlerini yüklemek için Kurulum'u çalıştırmayı içerir.

Git [adım 8: kaynak ve hedef ayarlama](vmware-walkthrough-source-target.md)

## <a name="step-9-set-up-a-replication-policy"></a>9. adım: Bir çoğaltma ilkesi ayarlama

VMware Vm'leri için çoğaltma ayarlarını kasaya belirtmek için bir ilke ayarlayın.

Git [adım 9: bir çoğaltma ilkesini ayarlayın](vmware-walkthrough-replication.md)

## <a name="step-10-install-the-mobility-service"></a>10. adım: mobilite hizmeti yükleme

Mobility hizmetinin çoğaltmak istediğiniz her bir VM üzerinde yüklü olmalıdır. Anında iletme ve çekme yükleme hizmetiyle ayarlamak için birkaç yolu vardır.

Git [adım 10: Mobility hizmetini yükleme](vmware-walkthrough-install-mobility.md)

## <a name="step-11-enable-replication"></a>11. adım: Çoğaltma etkinleştirme

Mobility hizmeti bir VM üzerinde çalışmaya başladıktan sonra çoğaltmayı etkinleştirebilirsiniz. Etkinleştirdikten sonra VM başlangıç çoğaltması gerçekleşir.

Git [adım 11: çoğaltmasını etkinleştir](vmware-walkthrough-enable-replication.md)

## <a name="step-12-run-a-test-failover"></a>12. adım: yük devretme testi çalıştırma

İlk çoğaltma sonlandırıldıktan ve değişim çoğaltması çalıştıran sonra her şeyin beklendiği gibi çalıştığından emin olmak için yük devretme testi çalıştırabilirsiniz.

Git [adım 12: yük devretme testi çalıştırma](vmware-walkthrough-test-failover.md)
