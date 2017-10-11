---
title: "Kaynak ve hedef Hyper-V çoğaltma Azure Site Recovery ile Azure'a (System Center VMM olmadan) için ayarlama | Microsoft Docs"
description: "Hyper-V sanal makineleri çoğaltma Azure Site Recovery ile Azure depolama için kaynak ve hedef ayarları ayarlamak için adımları özetler"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: d2010d85-77fd-4dea-84f3-1c960ed4c63f
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 06/22/2017
ms.author: raynew
ms.openlocfilehash: b38eb3a011d46f2239891ea1d1bcac2a4059a866
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="step-8-set-up-the-source-and-target-for-hyper-v-replication-to-azure"></a>8. adım: kaynak ve hedef Hyper-V çoğaltma Azure için ayarlayın

Bu makalede çoğaltma azure'a, Hyper-V sanal makineleri (System Center VMM olmadan) şirket içi, kaynak ve hedef ayarlarının nasıl yapılandırılacağı kullanarak [Azure Site Recovery](site-recovery-overview.md) Azure portalında hizmet.

POST açıklamaları ve soruları alt bu makalenin veya üzerinde [Azure kurtarma Hizmetleri Forumu](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


## <a name="set-up-the-source-environment"></a>Kaynak ortamı ayarlama

Hyper-V sitesini kurmak, Azure Site kurtarma Sağlayıcısı'nı ve Azure kurtarma Hizmetleri aracısını Hyper-V ana bilgisayarlarına yükleyin ve site kasaya kaydetmek.

1. İçinde **altyapıyı hazırlama**, tıklatın **kaynak**. Hyper-V konakları veya kümeleri için yeni bir Hyper-V sitesi bir kapsayıcı olarak eklemek için tıklatın **+ Hyper-V sitesi**.

    ![Kaynağı ayarlama](./media/hyper-v-site-walkthrough-source-target/set-source1.png)
2. İçinde **oluşturma Hyper-V sitesi**, site için bir ad belirtin. Daha sonra, **Tamam**'a tıklayın. Şimdi, oluşturduğunuz ve tıklatın siteyi seçin **+ Hyper-V Server** siteye bir sunucu ekleme.

    ![Kaynağı ayarlama](./media/hyper-v-site-walkthrough-source-target/set-source2.png)

3. İçinde **Sunucu Ekle** > **sunucu türü**, denetleyin **Hyper-V server** görüntülenir.

    - Eklemek istediğiniz Hyper-V server ile uyumlu olduğundan emin olun [Önkoşullar](#on-premises-prerequisites)ve belirtilen URL'leri erişebilir.
    - Azure Site Recovery Sağlayıcısı yükleme dosyasını indirin. Her Hyper-V ana bilgisayarda sağlayıcı ve kurtarma Hizmetleri Aracısı'nı yüklemek için bu dosyayı çalıştırın.

    ![Kaynağı ayarlama](./media/hyper-v-site-walkthrough-source-target/set-source3.png)


## <a name="install-the-provider-and-agent"></a>Sağlayıcı ve aracı yükleme

1. Sağlayıcıyı Hyper-V siteye eklenen her ana bilgisayarda kurulum dosyasını çalıştırın. Hyper-V kümesi üzerinde yüklüyorsanız, her küme düğümünde Kurulumu çalıştırın. Düğümleri arasında geçirmek olsa bile yükleme ve her Hyper-V küme düğümü kaydetme VM'ler korunduğunu sağlar.
2. **Microsoft Update** kısmından güncelleştirmeleri seçebilirsiniz. Böylece, Sağlayıcı güncelleştirmeleri Microsoft Update ilkenize uygun şekilde yüklenir.
3. **Yükleme** bölümünde varsayılan Sağlayıcı yükleme konumunu kabul edin veya değiştirin ve **Yükle**'ye tıklayın.
4. İçinde **kasa ayarlarını**, tıklatın **Gözat** indirdiğiniz kasa anahtarı dosyasını seçin. Azure Site Recovery abonelik, kasa adını ve Hyper-V sunucusuna ait olduğu Hyper-V sitesi belirtin.

    ![Sunucu kaydı](./media/hyper-v-site-walkthrough-source-target/provider3.png)

5. İçinde **Proxy ayarlarını**, Hyper-V konakları üzerinde çalışan sağlayıcının Azure Site Recovery için Internet üzerinden nasıl bağlandığını belirtin.

    * Sağlayıcı'nın doğrudan bağlanmasını istiyorsanız **Proxy olmadan doğrudan Azure Site Recovery hizmetine bağlan** seçeneğini belirleyin.
    * Var olan ara sunucunuz kimlik doğrulaması gerektiren veya sağlayıcı bağlantılarında özel bir ara sunucu kullanmak istiyorsanız **Azure Site Recovery proxy sunucu kullanma Bağlan**.
    * Bir proxy sunucu kullanıyorsanız:
        - Adresi, bağlantı noktası ve kimlik bilgilerini belirtin
        - Bölümünde açıklanan URL'lere emin olun [Önkoşullar](#prerequisites) proxy üzerinden izin verilir.

    ![internet](./media/hyper-v-site-walkthrough-source-target/provider7.png)

6. Yükleme tamamlandıktan sonra tıklatın **kaydetmek** sunucuyu kasaya kaydetmek için.

    ![Yükleme konumu](./media/hyper-v-site-walkthrough-source-target/provider2.png)

7. Kayıt tamamlandıktan sonra Hyper-V sunucusundan meta veriler, Azure Site Recovery tarafından alınır ve sunucu görüntülenen **Site Recovery altyapısı** > **Hyper-V konakları**.


## <a name="set-up-the-target-environment"></a>Hedef ortamı ayarlama

Azure depolama hesabı için çoğaltma ve yük devretme sonrasında Azure Vm'lerinin bağlanacağı Azure ağını belirtin.

1. Tıklatın **altyapıyı hazırlama** > **hedef**.
2. Abonelik ve yük devretme sonrasında Azure sanal makinelerini oluşturmak istediğiniz kaynak grubunu seçin. Azure (Klasik veya resource management) VM'ler için kullanmak istediğiniz dağıtım modelini seçin.

3. Site Recovery, bir veya birden çok uyumlu Azure depolama hesabınızın ve ağınızın olup olmadığını denetler.

    - Bir depolama hesabınız yoksa tıklatın **+ depolama** bir Resource Manager tabanlı hesap satır içi oluşturmak için. 
    - Bir Azure ağı yoksa tıklatın **+ ağ** bir Resource Manager tabanlı ağ satır içi oluşturmak için.






## <a name="next-steps"></a>Sonraki adımlar

Git [adım 9: bir çoğaltma ilkesini ayarlayın](hyper-v-site-walkthrough-replication.md)
