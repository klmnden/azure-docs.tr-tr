---
title: include dosyası
description: include dosyası
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: include
ms.date: 09/03/2018
ms.author: raynew
ms.custom: include file
ms.openlocfilehash: afeae4af9b41bf434b26833a3bd927118a4697ae
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2019
ms.locfileid: "67188597"
---
**Fiziksel sunucu çoğaltması için yapılandırma/işlem sunucusu gereksinimleri**

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
IIS | -Önceden var olan varsayılan Web sitesi <br> -Önceden var olan Web sitesi/443 numaralı bağlantı noktasını dinlemeye uygulama <br>-Etkinleştir [anonim kimlik doğrulaması](https://technet.microsoft.com/library/cc731244(v=ws.10).aspx) <br> -Etkinleştir [Fastcgı](https://technet.microsoft.com/library/cc753077(v=ws.10).aspx) ayarı.
IP adresi türü | Statik 
| 
**ERİŞİM AYARLARI** | 
MYSQL | MySQL yapılandırma sunucusunda yüklü olması. El ile yükleyebilirsiniz veya Site Recovery dağıtımı sırasında yükleyebilirsiniz. Site Recovery'nın yüklemek makine erişebildiğini denetleyin http://cdn.mysql.com/archives/mysql-5.5/mysql-5.5.37-win32.msi.
URL'leri | Yapılandırma sunucusunun şu URL'lere erişimi gerekir (doğrudan veya proxy aracılığıyla):<br/><br/> Azure AD: `login.microsoftonline.com`; `login.microsoftonline.us`; `*.accesscontrol.windows.net`<br/><br/> Çoğaltma veri aktarımı: `*.backup.windowsazure.com`; `*.backup.windowsazure.us`<br/><br/> Çoğaltma yönetimi: `*.hypervrecoverymanager.windowsazure.com`; `*.hypervrecoverymanager.windowsazure.us`; `https://management.azure.com`; `*.services.visualstudio.com`<br/><br/> Depolama erişim: `*.blob.core.windows.net`; `*.blob.core.usgovcloudapi.net`<br/><br/> Zaman eşitleme: `time.nist.gov`; `time.windows.com`<br/><br/> Telemetri (isteğe bağlı): `dc.services.visualstudio.com`
Güvenlik duvarı | IP adresi tabanlı güvenlik duvarı kuralları iletişim Azure URL'lere izin vermelidir. Basitleştirmek ve IP aralıklarını sınırlamak için URL filtreleme kullanmanızı öneririz.<br/><br/>**Ticari IP'ler için:**<br/><br/>-İzin ver [Azure veri merkezi IP aralıkları](https://www.microsoft.com/download/confirmation.aspx?id=41653)ve HTTPS (443) bağlantı noktası.<br/><br/> -Batı ABD (Access Control ve Identity Management için kullanılır) için IP adres aralıklarına izin verin.<br/><br/> -IP adresi aralıkları URL'leri desteklemek için aboneliğinizin Azure bölgesi için Azure Active Directory için yedekleme, çoğaltma ve depolama gerekli izin verir.<br/><br/> **Kamu IP'ler için:**<br/><br/> -Azure devlet kurumları veri merkezi IP aralıkları ve HTTPS (443) bağlantı noktasına izin verin.<br/><br/> -IP adresi aralıkları tüm ABD Devleti URL'lerini destekleyen bölgeler (Virginia, Texas, Arizona ve Iowa) için Azure Active Directory için yedekleme, çoğaltma ve depolama gerekli izin verir.
Bağlantı Noktaları | 443 (denetim kanalı düzenleme) izin ver<br/><br/> 9443 (veri aktarımı) izin ver 


**Yapılandırma/işlem sunucusu boyutlandırma gereksinimleri**

**CPU** | **Bellek** | **Önbellek diski** | **Veri değişiklik oranı** | **Çoğaltılan makineler**
--- | --- | --- | --- | ---
8 Vcpu<br/><br/> Yuva 2 * 4 çekirdek \@ 2.5 GHz | 16GB | 300 GB | 500 GB veya daha az | < 100 makineler
12 Vcpu<br/><br/> 2 socks * 6 çekirdek \@ 2.5 GHz | 18 GB | 600 GB | 500 GB - 1 TB | 100-150 makineler
16 Vcpu<br/><br/> 2 socks * 8 çekirdek \@ 2.5 GHz | 32 GB | 1 TB | 1-2 TB | 150 -200 makineler

