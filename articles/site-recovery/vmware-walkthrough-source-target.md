---
title: "Kaynak ve hedef VMware Azure'a çoğaltma için Azure Site Recovery ile ayarlama | Microsoft Docs"
description: "Kaynak ve hedef ayarlarını Azure Site Recovery ile Azure depolama çoğaltma işlemi VMware sanal makinelerini ayarlama adımlarını özetler"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: d99e422e-daf7-4fa8-af3c-af2340340136
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2017
ms.author: raynew
ms.openlocfilehash: 94b629a62c3a54eee69ee397b2f27e3f20b753d5
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="step-8-set-up-the-source-and-target-for-vmware-replication-to-azure"></a>8. adım: kaynak ve hedef VMware çoğaltma Azure için ayarlayın

Bu makale, şirket içi VMware sanal makinelerini Azure'a çoğaltırken kaynak ve hedef ayarlarını yapılandırmak açıklamaktadır kullanarak [Azure Site Recovery](site-recovery-overview.md) Azure portalında hizmet.

POST açıklamaları ve soruları alt bu makalenin veya üzerinde [Azure kurtarma Hizmetleri Forumu](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


## <a name="set-up-the-source-environment"></a>Kaynak ortamı ayarlama

Yapılandırma sunucusunu ayarlamayı, kasaya kaydetmek ve Vm'leri bulma.

1. Tıklatın **Site kurtarma** > **1. adım: altyapıyı hazırlama** > **kaynak**.
2. Yapılandırma sunucusu yoksa, tıklatın **+ yapılandırma sunucusu**.
3. İçinde **Sunucu Ekle**, denetleyin **yapılandırma sunucusu** görünür **sunucu türü**.
4. Site Recovery birleşik Kurulumu yükleme dosyasını indirin.
5. Kasa kayıt anahtarını indir Birleşik Kurulum'u çalıştırdığınızda bu gerekir. Anahtar, oluşturulduktan sonra beş gün boyunca geçerlidir.

   ![Kaynağı ayarlama](./media/vmware-walkthrough-source-target/set-source2.png)


## <a name="register-the-configuration-server-in-the-vault"></a>Yapılandırma sunucusunu kasaya kaydetmek

Başlayın, ardından birleştirilmiş yapılandırma sunucusuna işlem sunucusu ve ana hedef sunucusu yüklemek üzere kurulumu çalıştırmadan önce aşağıdakileri yapın.
    - Hızlı bir video özeti

        > [!VIDEO https://channel9.msdn.com/Series/Azure-Site-Recovery/VMware-to-Azure-with-ASR-Video1-Source-Infrastructure-Setup/player]

    - VM yapılandırma sunucusundaki sistem saatinin eşitlendiğinden emin olun bir [saat sunucusu](https://technet.microsoft.com/windows-server-docs/identity/ad-ds/get-started/windows-time-service/windows-time-service). Eşleşmelidir. Önde 15 dakika ise veya arkasında kurulum başarısız olabilir.
    - Kurulum, VM yapılandırma sunucusunda yerel yönetici olarak çalıştırın.
    - TLS 1.0 VM etkin olduğundan emin olun.


[!INCLUDE [site-recovery-add-configuration-server](../../includes/site-recovery-add-configuration-server.md)]

> [!NOTE]
> Yapılandırma sunucusu da yüklenebilir [komut satırından](http://aka.ms/installconfigsrv).



## <a name="connect-to-vmware-servers"></a>VMware sunucularına bağlanmak

Azure Site Recovery, şirket içi ortamınızda çalışan sanal makineleri bulmak izin vermek için VMware vCenter Server veya vSphere ESXi konakları Site Recovery ile bağlanmanız gerekir. Başlamadan önce aşağıdakilere dikkat edin:

- VCenter sunucusu veya Site kurtarma vSphere ana yönetici ayrıcalıklarına sahip olmayan bir hesapla sunucuda eklerseniz, hesap etkin Bu ayrıcalıkları gerekir:
    - Datacenter, veri deposu, klasör, ana bilgisayar, ağ, kaynak, sanal makine, vSphere dağıtılmış anahtar.
    - VCenter sunucusu depolama görünümleri izinleri olması gerekir.
- Site Recovery VMware sunucular eklediğinizde, 15 dakika alabilir ya da sunumların portalda görünebilmesi daha uzun.

### <a name="add-the-account-for-automatic-discovery"></a>Otomatik bulma hesabı Ekle

[!INCLUDE [site-recovery-add-vcenter-account](../../includes/site-recovery-add-vcenter-account.md)]

### <a name="set-up-a-connection"></a>Bağlantı kurma

Sunuculara şu şekilde bağlanın:

1. Seçin **+ vCenter** bir VMware vCenter sunucusu veya bir VMware vSphere ESXi konağı bağlanma başlatmak için.
2. **vCenter Ekle** menüsünde vSphere konağı veya vCenter sunucusu için bir kolay ad belirtin ve ardından sunucunun IP adresini ya da FQDN’sini belirtin.
3. VMware sunucularınız farklı bir bağlantı noktasındaki istekleri dinleyecek şekilde yapılandırılmadıkça bağlantı noktasını 443 olarak bırakın. VMware vCenter veya vSphere ESXi sunucusuna bağlanacak hesabı seçin. **Tamam** düğmesine tıklayın.
4. Site Recovery belirtilen ayarları kullanarak VMware sunucularına bağlanır ve sanal makineleri bulur.

> [!NOTE]
> Bir sunucu veya vCenter veya ana bilgisayar sunucusunda yönetici ayrıcalıklarına sahip olmayan bir hesapla konak ekliyorsanız, hesabın etkin Bu ayrıcalıkları olduğundan emin olun: Datacenter, veri deposu, klasör, ana bilgisayar, ağ, kaynak, sanal makine ve vSphere dağıtılmış anahtar. Ayrıca, VMware vCenter server etkin depolama görünümleri ayrıcalık gerekiyor.


## <a name="set-up-the-target-environment"></a>Hedef ortamı ayarlama

Hedef ortamını ayarlama önce bir Azure depolama hesabı ve sanal ağ kurulumu olduğundan emin olun.

1. **Altyapıyı hazırlama** > **Hedef** seçeneklerine tıklayıp kullanmak istediğiniz Azure aboneliğini seçin.
2. Belirtin, hedef dağıtım modeli Resource Manager tabanlı veya Klasik olup.
3. Site Recovery, bir veya birden çok uyumlu Azure depolama hesabınızın ve ağınızın olup olmadığını denetler.

   ![Hedef](./media/vmware-walkthrough-source-target/gs-target.png)
4. Bir depolama hesabı veya ağı oluşturmadıysanız, tıklatın **+ depolama hesabı** veya **+ ağ**, bir kaynak yöneticisi hesabı oluşturun veya satır içi ağ.

## <a name="next-steps"></a>Sonraki adımlar

Git [adım 9: bir çoğaltma ilkesini ayarlayın](vmware-walkthrough-replication.md)
