---
title: "Replicate fiziksel şirket içi sunucular Azure Site Recovery ile azure'a | Microsoft Docs"
description: "Azure Site Recovery hizmeti ile Azure için şirket içi Windows/Linux fiziksel sunucularında çalışan iş yüklerini çoğaltmak için adımlara genel bir bakış sağlar."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 20122f01-929a-4675-b85b-a9b99d2618bc
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2017
ms.author: raynew
ms.openlocfilehash: 0a09b35e98dc0b2f5283c2a707a3a2b8ac9a39f2
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="replicate-physical-servers-to-azure-with-site-recovery"></a>Azure Site Recovery ile fiziksel sunucuları çoğaltma

Bu makalede Azure için şirket içi Windows/Linux fiziksel sunucuları çoğaltmak için gerekli adımlar genel bir bakış sağlar kullanarak [Azure Site Recovery](site-recovery-overview.md) Azure portalında hizmet.


## <a name="step-1-review-architecture-and-prerequisites"></a>1. adım: mimarisi ve önkoşulları gözden geçirin

Dağıtıma başlamadan önce senaryo mimarisinin gözden geçirin ve dağıtımı ayarlamak için gereken tüm bileşenleri anladığınızdan emin olun.

Git [1. adım: mimarisi gözden geçirin](physical-walkthrough-architecture.md)


## <a name="step-2-review-prerequisites"></a>2. adım: Gözden geçirme önkoşulları

Önkoşullar her dağıtım bileşen için yerinde olduğundan emin olun:

- **Azure önkoşulları**: bir Microsoft Azure hesabı, Azure ağları ve depolama hesapları gerekir.
- **Şirket içi Site Recovery bileşenlerini**: şirket içi Site Recovery bileşenlerini çalıştıran bir makineye gerekir.
- **Makineler çoğaltılan**: sunucuları çoğaltmak istediğiniz şirket içi ve Azure gereksinimleri uymak gerekir.

Git [2. adım: gözden Önkoşullar ve sınırlamalar](physical-walkthrough-prerequisites.md)

## <a name="step-3-plan-capacity"></a>3. adım: Planı kapasite

Tam dağıtımını yaptığınız ihtiyacınız hangi çoğaltma kaynakları şekil gerekir. Test ortamı için ayarladığınız hızlı yaptığınız varsa, bu adımı atlayabilirsiniz.

[3. Adım: Kapasiteyi planlama](physical-walkthrough-capacity.md)’ya gidin.

## <a name="step-4-plan-networking"></a>4. adım: ağ planlama

Bazı ağ yük devretme gerçekleştikten sonra sahip oldukları doğru IP adreslerini Azure Vm'lerinin ağlara bağlandığından emin olmak planlama yapmanız gerekir.

Git [4. adım: ağ planı](physical-walkthrough-network.md)

##  <a name="step-5-prepare-azure-resources"></a>5. adım: Azure kaynaklarını hazırlama

Başlamadan önce Azure ağları ve depolama ayarlayın. 

Git [5. adım: Azure hazırlama](physical-walkthrough-prepare-azure.md)


## <a name="step-6-set-up-a-vault"></a>6. adım: Bir kasa ayarlama

Düzenlemek ve çoğaltmayı yönetmek için bir kurtarma Hizmetleri kasasını ayarlayın. Kasasını ayarladığınızda, çoğaltmak istediğiniz ve kendisine çoğaltmak istediğiniz yeri belirtin.

Git [adım 6: bir kasasını oluşturup](physical-walkthrough-create-vault.md)

## <a name="step-7-configure-source-and-target-settings"></a>7. adım: kaynak ve hedef ayarlarını yapılandırma

(Azure) site hedef ve kaynak için ayarları yapılandırın. Kaynak ayarları birleşik şirket içi Site Recovery bileşenlerini yüklemek için Kurulumu çalıştıran içerir.

Git [adım 7: kaynak ve hedef ayarlama](physical-walkthrough-source-target.md)

## <a name="step-8-set-up-a-replication-policy"></a>8. adım: Bir çoğaltma ilkesi ayarlama

Ayarladığınız nasıl fiziksel sunucuları belirtmek için bir ilke çoğaltılması.

Git [adım 8: bir çoğaltma ilkesini ayarlayın](physical-walkthrough-replication.md)

## <a name="step-9-install-the-mobility-service"></a>9. adım: mobilite hizmeti yükleme

Mobility hizmetinin çoğaltmak istediğiniz her sunucuda yüklenmesi gerekir. Hizmetle iterek ister çekerek yükleme ayarlamak için birkaç yolu vardır.

Git [adım 9: Mobility hizmetini yükleme](physical-walkthrough-install-mobility.md)

## <a name="step-10-enable-replication"></a>10. adım: Çoğaltma etkinleştirme

Mobility hizmeti bir sunucuda çalışmaya başladıktan sonra çoğaltmayı etkinleştirebilirsiniz. Etkinleştirdikten sonra VM başlangıç çoğaltması gerçekleşir.

Git [adım 10: çoğaltmasını etkinleştir](physical-walkthrough-enable-replication.md)

## <a name="step-11-run-a-test-failover"></a>11. adım: yük devretme testi çalıştırma

İlk çoğaltma sonlandırıldıktan sonra değişim çoğaltması çalıştığından, her şeyin beklendiği gibi çalıştığından emin olmak için yük devretme testi çalıştırabilirsiniz.

Git [11. adım: yük devretme testi çalıştırma](physical-walkthrough-test-failover.md)

