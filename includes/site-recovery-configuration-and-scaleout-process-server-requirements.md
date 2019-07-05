---
title: include dosyası
description: include dosyası
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: include
ms.date: 06/10/2018
ms.author: raynew
ms.custom: include file
ms.openlocfilehash: 3b4992a16061bef782f012aa7887b248e3423234
ms.sourcegitcommit: d3b1f89edceb9bff1870f562bc2c2fd52636fc21
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/04/2019
ms.locfileid: "67568385"
---
**Yapılandırma/işlem sunucusu gereksinimleri**

**Bileşen** | **Gereksinim** 
--- | ---
**DONANIM AYARLARI** | 
CPU çekirdekleri | 8 
RAM | 16 GB
Disk sayısı | işletim sistemi diski, işlem sunucusu önbellek diski ve yeniden çalışma için bekletme sürücüsü dahil olmak üzere, 3 
Boş disk alanı (işlem sunucusu önbelleği) | 600 GB
Boş disk alanı (bekletme diski) | 600 GB
 | 
**YAZILIM AYARLARI** | 
İşletim sistemi | Windows Server 2012 R2 <br> Windows Server 2016
İşletim sistemi yerel ayarı | İngilizce (en-us)
Windows Server rolleri | Bu rolleri etkinleştirmeyin: <br> - Active Directory Domain Services <br>- İnternet Bilgi Hizmetleri <br> - Hyper-V 
Grup İlkeleri | Bu grup ilkeleri etkinleştirme: <br> -Komut istemine erişimi engelleyin. <br> -Kayıt defteri düzenleme araçlarına erişimi engelleyin. <br> -Mantıksal dosya ekleri için güven. <br> -Betik yürütmeyi açma. <br> [Daha fazla bilgi edinin](https://technet.microsoft.com/library/gg176671(v=ws.10).aspx)
IIS | -Önceden mevcut olan varsayılan Web sitesi <br> -Önceden var olan Web sitesi/443 numaralı bağlantı noktasını dinlemeye uygulama <br>-Etkinleştir [anonim kimlik doğrulaması](https://technet.microsoft.com/library/cc731244(v=ws.10).aspx) <br> -Etkinleştir [Fastcgı](https://technet.microsoft.com/library/cc753077(v=ws.10).aspx) ayarı 
| 
**AĞ AYARLARI** | 
IP adresi türü | Statik 
Bağlantı Noktaları | 443 (Denetim kanalı düzenleme)<br>9443 (Veri aktarımı) 
NIC türü | VMXNET3 (yapılandırma sunucusu VMware VM ise)
 |
**Internet erişimi** (sunucunun aşağıdaki URL'lere - doğrudan veya proxy üzerinden erişmesi):|
\*.backup.windowsazure.com | Çoğaltılmış veri aktarımı ve düzenlemesi için kullanılır
\*.store.core.windows.net | Çoğaltılmış veri aktarımı ve düzenlemesi için kullanılır
\*.blob.core.windows.net | Çoğaltılan verileri depolayan depolama hesabına erişmek için kullanılan
\*.hypervrecoverymanager.windowsazure.com | Çoğaltma yönetimi işlemleri ve düzenleme için kullanılır
https:\//management.azure.com | Çoğaltma yönetimi işlemleri ve düzenleme için kullanılır 
*.services.visualstudio.com | (İsteğe bağlı) telemetri amaçlar için kullanılır
time.nist.gov | Sistem ile genel saat arasındaki saat eşitlemesini denetlemek için kullanılır.
time.windows.com | Sistem ile genel saat arasındaki saat eşitlemesini denetlemek için kullanılır.
| <ul> <li> https:\//login.microsoftonline.com </li><li> https:\//secure.aadcdn.microsoftonline-p.com </li><li> https:\//login.live.com </li><li> https:\//graph.windows.net </li><li> https:\//login.windows.net </li><li> https:\//www.live.com </li><li> https:\//www.microsoft.com </li></ul> | OVF ayarlamak bu URL'lere erişimi olmalıdır. Erişim denetimi ve kimlik yönetimi için Azure Active Directory tarafından kullanılır
https:\//dev.mysql.com/get/Downloads/MySQLInstaller/mysql-installer-community-5.7.20.0.msi  | MySQL indirmeyi tamamlamak için. </br> Bazı bölgelerde indirme CDN URL'ye yeniden yönlendirilen olabilir. CDN URL'sine izin verilenler listesinde, aynı zamanda olduğunu, gerekirse emin olun.
|
**YÜKLENECEK YAZILIM** | 
VMware vSphere Powerclı | [Powerclı 6.0 sürümünün](https://my.vmware.com/web/vmware/details?productId=491&downloadGroup=PCLI600R1) yapılandırma sunucusunu bir VMware sanal makine üzerinde çalışıyorsa yüklü olması gerekir.
MYSQL | MySQL yüklü olması gerekir. El ile yükleyebilirsiniz veya Site Recovery yükleyebilirsiniz. (Bakın [ayarlarını yapılandırma](../articles/site-recovery/vmware-azure-deploy-configuration-server.md#configure-settings) daha fazla bilgi için)

**Yapılandırma/işlem sunucusu boyutlandırma gereksinimleri**

**CPU** | **Bellek** | **Önbellek diski** | **Veri değişiklik oranı** | **Çoğaltılan makineler**
--- | --- | --- | --- | ---
8 Vcpu<br/><br/> Yuva 2 * 4 çekirdek \@ 2.5 GHz | 16GB | 300 GB | 500 GB veya daha az | < 100 makineler
12 Vcpu<br/><br/> 2 socks * 6 çekirdek \@ 2.5 GHz | 18 GB | 600 GB | 500 GB - 1 TB | 100-150 makineler
16 Vcpu<br/><br/> 2 socks * 8 çekirdek \@ 2.5 GHz | 32 GB | 1 TB | 1-2 TB | 150 -200 makineler

