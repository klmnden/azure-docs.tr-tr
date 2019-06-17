---
title: Azure Site Recovery ile azure'a VMware olağanüstü durum kurtarma için yapılandırma sunucusu gereksinimleri | Microsoft Docs
description: Bu makalede Azure Site Recovery ile azure'a VMware olağanüstü durum kurtarması için yapılandırma sunucusunu dağıtırken desteği ve gereksinimleri açıklanır
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
services: site-recovery
ms.topic: article
ms.date: 05/30/2019
ms.author: raynew
ms.openlocfilehash: 94f410b7bd3b7c2eb3d7d6a9316323092010338e
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66418344"
---
# <a name="configuration-server-requirements-for-vmware-disaster-recovery-to-azure"></a>Vmware'den azure'a olağanüstü durum kurtarma için yapılandırma sunucusu gereksinimleri

Kullanırken bir şirket içi yapılandırma sunucusunu dağıtma [Azure Site Recovery](site-recovery-overview.md) VMware Vm'lerini ve fiziksel sunucuları azure'a olağanüstü durum kurtarma için.

- Yapılandırma sunucusu koordinatları iletişimleri arasında şirket içi VMware ve Azure. Veri çoğaltma işlemlerini yönetir.
- [Daha fazla bilgi edinin](vmware-azure-architecture.md) yapılandırma sunucusu bileşenleri ve süreçleri hakkında.

## <a name="configuration-server-deployment"></a>Yapılandırma sunucusu dağıtımı

Azure'da olağanüstü durum kurtarma, VMware Vm'leri için yapılandırma sunucusunu bir VMware VM olarak dağıtma.

- Site Recovery, Azure portalından indirin ve vCenter sunucusuna yapılandırma sunucusu sanal makine kurmak için içe OVA şablonu sağlar.
- OVA şablonunu kullanarak yapılandırma sunucusunu dağıtma, VM otomatik olarak bu makalede listelenen gereksinimleri ile uyumludur.
- OVA şablonunu kullanarak yapılandırma sunucusunu ayarladıktan ayarlamanızı öneririz. VMware Vm'leri için olağanüstü durum kurtarma işlemini ayarladığınız ve OVA şablon kullanamazsınız, ancak yapılandırma sunucusu kullanarak dağıtabileceğiniz [sağlanan bu yönergeleri](physical-azure-set-up-source.md).
- Şirket içi fiziksel makinelerin olağanüstü durum kurtarması için yapılandırma sunucusunu Azure'a dağıtıyorsanız, yönergeleri izleyin [bu makalede](physical-azure-set-up-source.md). 


## <a name="hardware-requirements"></a>Donanım gereksinimleri

**Bileşen** | **Gereksinim** 
--- | ---
CPU çekirdekleri | 8 
RAM | 16 GB
Disk sayısı | işletim sistemi diski, işlem sunucusu önbellek diski ve yeniden çalışma için bekletme sürücüsü dahil olmak üzere, 3 
Boş disk alanı (işlem sunucusu önbelleği) | 600 GB
Boş disk alanı (bekletme diski) | 600 GB

## <a name="software-requirements"></a>Yazılım gereksinimleri

**Bileşen** | **Gereksinim** 
--- | ---
İşletim sistemi | Windows Server 2012 R2 <br> Windows Server 2016
İşletim sistemi yerel ayarı | İngilizce (en-us)
Windows Server rolleri | Bu rolleri etkinleştirmeyin: <br> - Active Directory Domain Services <br>- İnternet Bilgi Hizmetleri <br> - Hyper-V 
Grup İlkeleri | Bu grup ilkeleri etkinleştirme: <br> -Komut istemine erişimi engelleyin. <br> -Kayıt defteri düzenleme araçlarına erişimi engelleyin. <br> -Mantıksal dosya ekleri için güven. <br> -Betik yürütmeyi açma. <br> [Daha fazla bilgi edinin](https://technet.microsoft.com/library/gg176671(v=ws.10).aspx)
IIS | -Önceden var olan varsayılan Web sitesi <br> -Önceden var olan Web sitesi/443 numaralı bağlantı noktasını dinlemeye uygulama <br>-Etkinleştir [anonim kimlik doğrulaması](https://technet.microsoft.com/library/cc731244(v=ws.10).aspx) <br> -Etkinleştir [Fastcgı](https://technet.microsoft.com/library/cc753077(v=ws.10).aspx) ayarı 

## <a name="network-requirements"></a>Ağ gereksinimleri

**Bileşen** | **Gereksinim** 
--- | --- 
IP adresi türü | Statik 
İnternet erişimi | Sunucusunun şu URL'lere erişimi olmalıdır (doğrudan veya proxy aracılığıyla): <br> - \*.accesscontrol.windows.net<br> - \*.backup.windowsazure.com <br>- \*.store.core.windows.net<br> - \*.blob.core.windows.net<br> - \*.hypervrecoverymanager.windowsazure.com  <br> - https:\//management.azure.com <br> - *.services.visualstudio.com <br> - time.nist.gov <br> - time.windows.com <br> OVF ayrıca aşağıdaki URL'lere erişim gerekir: <br> - https:\//login.microsoftonline.com <br> -https:\//secure.aadcdn.microsoftonline-p.com <br> - https:\//login.live.com  <br> -https:\//auth.gfx.ms <br> -https:\//graph.windows.net <br> -https:\//login.windows.net <br> - https:\//www.live.com <br> -https:\//www.microsoft.com <br> -https:\//dev.mysql.com/get/Downloads/MySQLInstaller/mysql-installer-community-5.7.20.0.msi 
Bağlantı Noktaları | 443 (Denetim kanalı düzenleme)<br>9443 (Veri aktarımı) 
NIC türü | VMXNET3 (yapılandırma sunucusu VMware VM ise)

## <a name="required-software"></a>Gerekli yazılım

**Bileşen** | **Gereksinim** 
--- | ---
VMware vSphere Powerclı | [Powerclı 6.0 sürümünün](https://my.vmware.com/web/vmware/details?productId=491&downloadGroup=PCLI600R1) yapılandırma sunucusunu bir VMware sanal makine üzerinde çalışıyorsa yüklü olması gerekir.
MYSQL | MySQL yüklü olması gerekir. El ile yükleyebilirsiniz veya Site Recovery yükleyebilirsiniz.

## <a name="sizing-and-capacity-requirements"></a>Boyutlandırma ve kapasite gereksinimleri

Yapılandırma sunucusu için kapasite gereksinimleri aşağıdaki tabloda özetlenmiştir. Birden çok VMware sanal makinelerini çoğaltıyorsanız, gözden geçirmeniz gereken [kapasite planlaması konuları](site-recovery-plan-capacity-vmware.md), çalıştırıp [Azure Site Recovery dağıtım Planlayıcısı](site-recovery-deployment-planner.md) VMWare replication.read için aracı 

**Bileşen** | **Gereksinim** 
--- | ---

| **CPU** | **Bellek** | **Önbellek diski** | **Veri değişiklik oranı** | **Çoğaltılan makineler** |
| --- | --- | --- | --- | --- |
| 8 Vcpu<br/><br/> Yuva 2 * 4 çekirdek \@ 2.5 GHz | 16 GB | 300 GB | 500 GB veya daha az | LES 100 makineler daha |
| 12 Vcpu<br/><br/> 2 socks * 6 çekirdek \@ 2.5 GHz | 18 GB | 600 GB | 500 GB - 1 TB | 100-150 makineler |
| 16 Vcpu<br/><br/> 2 socks * 8 çekirdek \@ 2.5 GHz | 32 GB | 1 TB | 1-2 TB | 150-200 makineler | 



## <a name="next-steps"></a>Sonraki adımlar
Olağanüstü durum kurtarma ayarlama [VMware Vm'lerini](vmware-azure-tutorial.md) azure'a.
