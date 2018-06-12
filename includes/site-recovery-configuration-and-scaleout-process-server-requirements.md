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
ms.openlocfilehash: 601819756b78ffe8762bdfbfd5f802bc2d76e9c5
ms.sourcegitcommit: 1b8665f1fff36a13af0cbc4c399c16f62e9884f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/11/2018
ms.locfileid: "35268062"
---
**Yapılandırma/işlem sunucusu gereksinimleri**

**Bileşen** | **Gereksinim** 
--- | ---
**DONANIM AYARLARI** | 
CPU çekirdekleri | 8 
RAM | 16 GB
Disk sayısı | işletim sistemi diski, işlem sunucusu önbellek disk ve yeniden çalışma için bekletme sürücüsü dahil olmak üzere 3 
Boş disk alanı (işlem sunucusunun önbellek) | 600 GB
Boş disk alanı (bekletme disk) | 600 GB
 | 
**YAZILIM AYARLARI** | 
İşletim sistemi | Windows Server 2012 R2 <br> Windows Server 2016
İşletim sistemi yerel ayarı | İngilizce (en-us)
Windows Server rolleri | Bu rolleri etkinleştirme: <br> - Active Directory Domain Services <br>- İnternet Bilgi Hizmetleri <br> - Hyper-V 
Grup İlkeleri | Bu grup ilkeleri etkinleştirme: <br> -Komut istemi erişimi engelleyin. <br> -Düzenleme araçları kayıt defterine erişim engelleyin. <br> -Dosya ekleri için mantığı güven. <br> -Komut dosyası yürütme açın. <br> [Daha fazla bilgi](https://technet.microsoft.com/library/gg176671(v=ws.10).aspx)
IIS | -Önceden var olan varsayılan Web sitesi <br> -Önceden var olan Web sitesi/443 numaralı bağlantı noktasını dinlemeye uygulama <br>-Etkinleştirin [anonim kimlik doğrulaması](https://technet.microsoft.com/library/cc731244(v=ws.10).aspx) <br> -Etkinleştirin [Fastcgı](https://technet.microsoft.com/library/cc753077(v=ws.10).aspx) ayarı 
| 
**AĞ AYARLARI** | 
IP adresi türü | Statik 
İnternet erişimi | Sunucu bu URL'leri erişmesi (doğrudan veya proxy üzerinden) <br> - \*.accesscontrol.windows.net<br> - \*.backup.windowsazure.com <br>- \*.store.core.windows.net<br> - \*.blob.core.windows.net<br> - \*.hypervrecoverymanager.windowsazure.com <br> - https://cdn.mysql.com/archives/mysql-5.5/mysql-5.5.37-win32.msi (bir yapılandırma sunucusunu ayarlamayı ayarladığınız ise) <br> - time.nist.gov <br> - time.windows.com 
Bağlantı Noktaları | 443 (Denetim kanalı düzenleme)<br>9443 (Veri aktarımı) 
NIC türü | (Yapılandırma sunucusu VMware VM ise) VMXNET3
 | 
**YAZILIM YÜKLEME İÇİN** | 
VMware vSphere Powerclı | [Powerclı sürüm 6.0](https://my.vmware.com/web/vmware/details?productId=491&downloadGroup=PCLI600R1) yapılandırma sunucusunu bir VMware VM üzerinde çalışıyorsa, yüklü olmalıdır.
MYSQL | MySQL yüklenmesi gerekir. El ile yükleyebilirsiniz veya Site Recovery yükleyebilirsiniz.

**Yapılandırma/işlem sunucusu gereksinimleri boyutlandırma**

**CPU** | **Bellek** | **Önbellek disk** | **Veri değişikliği oranı** | **Çoğaltılan makineler**
--- | --- | --- | --- | ---
8 Vcpu'lar<br/><br/> 2 yuva * @ 2,5 GHz 4 çekirdek | 16GB | 300 GB | 500 GB veya daha az | < 100 makineler
12 Vcpu'lar<br/><br/> 2 socks * 2,5 GHz @ 6 çekirdekler | 18 GB | 600 GB | 500 GB - 1 TB | için 100 150 makineler
16 Vcpu<br/><br/> 2 socks * 8 çekirdek 2,5 GHz @ | 32 GB | 1 TB | 1-2 TB | 150 -200 makineler

