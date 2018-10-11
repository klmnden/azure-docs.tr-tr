---
title: Azure Site Recovery ile Azure VM’leri için ikincil bir Azure bölgesine olağanüstü durum kurtarma tatbikatı çalıştırma
description: Azure Site Recovery ile Azure VM’leri için ikincil bir Azure bölgesine olağanüstü durum kurtarma tatbikatını nasıl çalıştıracağınızı öğrenin.
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: tutorial
ms.date: 10/10/2018
ms.author: raynew
ms.custom: mvc
ms.openlocfilehash: 90d82abb6570028308b8d516a868fa981c75eb94
ms.sourcegitcommit: 7b0778a1488e8fd70ee57e55bde783a69521c912
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/10/2018
ms.locfileid: "49066998"
---
# <a name="run-a-disaster-recovery-drill-for-azure-vms-to-a-secondary-azure-region"></a>Azure VM’leri için ikincil bir Azure bölgesine olağanüstü durum kurtarma tatbikatı çalıştırma

[Azure Site Recovery](site-recovery-overview.md) hizmeti, planlı ve plansız kesintiler sırasında iş uygulamalarınızı çalışır durumda tutarak, iş sürekliliğinize ve olağanüstü durum kurtarma (BCDR) stratejinize katkıda bulunur. Site Recovery, şirket içi makinelerin ve Azure sanal makinelerinin çoğaltma, yük devretme ve kurtarma gibi olağanüstü durum kurtarma işlemlerini yönetir ve düzenler.

Bu öğretici, bir Azure VM için yük devretme testiyle bir Azure bölgesinden diğerine nasıl olağanüstü durum kurtarma tatbikatı çalıştıracağınızı gösterir. Tatbikat, çoğaltma stratejinizi veri kaybı veya kesinti süresi olmadan doğrular ve üretim ortamınızı etkilemez. Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Önkoşulları denetleme
> * Tek bir VM için yük devretme testi çalıştırma



## <a name="prerequisites"></a>Ön koşullar

- Yük devretme testini çalıştırmadan önce, her şeyin beklenildiği gibi gittiğinden emin olmak için VM özelliklerini doğrulamanızı öneririz.  **Çoğaltılmış öğeler** bölümünde VM özelliklerine erişin. **Temel bileşenler** dikey penceresi, makinelerin ayarları ve durumuyla ilgili bilgileri gösterir.
- Yük devretme testi için, çoğaltmayı etkinleştirdiğinizde ayarlanmış varsayılan ağ yerine ayrı bir Azure VM ağını kullanmanızı öneririz.


## <a name="run-a-test-failover"></a>Yük devretme testi çalıştırma

1. **Ayarlar** > **Çoğaltılmış Öğeler** bölümünde, **+Yük Devretme Testi** simgesine tıklayın.

2. **Yük Devretme Testi** bölümünde, yük devretmede kullanılması için bir kurtarma noktası seçin:

   - **En son işlenen**: VM’nin yükünü, Site Recovery hizmeti tarafından işlenen en son kurtarma noktasına devreder. Zaman damgası gösterilir. Bu seçenekle veri işlemeye zaman harcanmadığından düşük RTO sağlanılır (Kurtarma Süresi Hedefi)
   - **Uygulamayla tutarlı olan son**: Bu seçenek, tüm VM’lerin yükünü uygulamayla tutarlı olan en son kurtarma noktasına devreder. Zaman damgası gösterilir.
   - **Özel**: Herhangi bir kurtarma noktası seçin.

3. Yük devretme gerçekleştikten sonra ikincil bölgedeki Azure VM’lerin bağlanacağı hedef Azure sanal makinelerini seçin.

4. Yük devretmeyi başlatmak için, **Tamam**’a tıklayın. İlerleme durumunu izlemek için, VM’ye tıklayarak özelliklerini açın. Ya da kasa adı > **Ayarlar** > **İşler** > **Site Recovery işleri** bölümünde **Yük Devretme Testi** işine tıklayabilirsiniz.
5. Yük devretme bittikten sonra, çoğaltma Azure VM, Azure portalı > **Sanal Makineler** bölümünde görünür. VM’nin çalıştığından, uygun şekilde boyutlandırıldığından ve uygun ağa bağlı olduğundan emin olun.
6. Yük devretme testi sırasında oluşturulan sanal makineleri silmek için, çoğaltılmış öğede veya kurtarma planında **Yük devretme testini temizle**’ye tıklayın. Yük devretme testiyle ilişkili gözlemlerinizi **Notlar**’da kaydedin veya saklayın.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Üretim yük devretmesini çalıştırma](azure-to-azure-tutorial-failover-failback.md)
