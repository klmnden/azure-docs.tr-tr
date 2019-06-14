---
title: Microsoft Azure Backup sunucusu yenilikler
description: Microsoft Azure Backup sunucusu, VM'ler, dosyaları ve klasörleri, iş yükleri ve daha fazla korumak için yedekleme gelişmiş özellikler sunar. Yüklemek veya yükseltmek için Azure Backup sunucusu V3 öğrenin.
services: backup
author: rayne-wiselman
manager: carmonm
ms.service: backup
ms.topic: conceptual
ms.date: 11/13/2018
ms.author: adigan
ms.openlocfilehash: 5718064994a80266c216ae6040746be29194adc9
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60254718"
---
# <a name="whats-new-in-microsoft-azure-backup-server"></a>Microsoft Azure Backup sunucusu yenilikler

Microsoft Azure Backup sunucusu sürüm 3 (MABS V3) en son yükseltme ve Kritik hata düzeltmeleri, Windows Server 2019 desteği, 2017 SQL desteği ve diğer özellikleri ve geliştirmeleri içerir. Düzeltilen hataların listesi ve yükleme yönergeleri için bkz. KB makalesi MABS V3 görüntülemek için [4457852](https://support.microsoft.com/en-us/help/4457852/microsoft-azure-backup-server-v3).

MABS V3 sürümünde aşağıdaki özellikler bulunmaktadır:

## <a name="volume-to-volume-migration"></a>Birim için birim geçişi
Modern yedekleme depolama alanı (MBS ile) MABS V2'de, belirli depolama alanına yedeklenmesi bazı iş yüklerinin yapılandırdığınız yer iş yükü algılayan depolama, depolama özelliklerine bağlı duyurduk. Ancak, yapılandırmadan sonra en iyi duruma getirilmiş kaynak kullanımı için başka bir depolama için yedeklemeleri belirli veri kaynaklarının uzaklaşmaya gerek bulabilirsiniz. MABS V3 yedeklemeleriniz geçirme ve bunları farklı bir birim için depolanan yapılandırma yeteneği verir [3 adımda](https://blogs.technet.microsoft.com/dpm/2017/10/24/storage-migration-with-dpm-2016-mbs/).

## <a name="prevent-unexpected-data-loss"></a>Beklenmeyen veri kaybı önleme
Kuruluşlarda, MABS yöneticileri ekibi tarafından yönetilir. Yedekleme depolama alanı olarak MABS verilen yanlış bir birim, yedeklemeler için kullanılması gereken depolama yönergeleri varken kritik veri kaybına neden olabilir. MABS V3 ile bu birimleri depolama kullanmak için mevcut olmayan sürücüler olarak yapılandırarak bu senaryolara engelleyebilir [bu PowerShell cmdlet'lerini](https://docs.microsoft.com/system-center/dpm/add-storage#volume-exclusion).

## <a name="custom-size-allocation"></a>Özel Boyut ayırma
Modern yedekleme depolama alanı (MBS), depolama, ölçülü kaynak, gerektiğinde gerekli tüketir. Bunu yapmak için MABS için koruma yapılandırıldığında, yedeklenmekte olan verilerin boyutunu hesaplar. Ancak, birçok dosya ve klasörleri birlikte olduğu gibi bir dosya sunucusu durumunu yedeklenen, boyut hesaplaması uzun zaman alabilir. MABS V3 ile bu nedenle zamandan tasarruf MABS her dosyanın boyutu hesaplanması yerine varsayılan olarak birim boyutunu kabul edecek şekilde yapılandırabilirsiniz.

## <a name="optimized-cc-for-rct-vms"></a>RCT Vm'leri için en iyi duruma getirilmiş CC
RCT VM kilitlenmeler gibi hangi azaldıkça zaman harcayan tutarlılık için gereken senaryolarda denetler. (Hyper-V yerel değişiklik izleme), MABS kullanır. RCT, VSS anlık görüntü tabanlı yedeklemelerin sağladığı daha iyi esneklik değişiklik izleme sağlar. MABS V3 tüm tutarlılık denetimleri sırasında yalnızca değiştirilen verileri aktararak ek ağ ve depolama tüketiminden en iyi duruma getirir.

## <a name="support-to-tls-12"></a>TLS 1.2 desteği
TLS 1.2 ile sınıfının en iyisi, şifreleme Microsoft tarafından önerilen iletişimin güvenli bir yoludur. MABS MABS ve korumalı sunucular arasındaki iletişimi TLS 1.2 bulut yedeklemeleri ve sertifika tabanlı kimlik doğrulaması için artık desteklemektedir.

## <a name="vmware-vm-protection-support"></a>VMware VM koruma desteği
VMware VM yedekleme, artık üretim dağıtımları için desteklenir. Aşağıdaki VMware VM koruması için MABS V3 sunar:

-   5\.5 ve 6.0 desteği ile birlikte vCenter ve ESXi 6.5 desteği.
- Otomatik koruma buluta VMware vm'lerinin. Yeni VMware Vm'leri korumalı bir klasöre eklenirse, bunlar disk ile bulut için otomatik olarak korunur.
- VMware alternatif konuma kurtarma için kurtarma Verimlilik geliştirmeleri.

## <a name="sql-2017-support"></a>SQL 2017 desteği
MABS V3 MABS veritabanı olarak SQL 2017 ile yüklenebilir. SQL server SQL 2016'dan SQL 2017'ye yükseltmeniz veya yeni yükleyin. Ayrıca SQL 2017 iş yükü hem kümelenmiş ve kümelenmemiş bir ortamda MABS V3 ile yedekleyebilirsiniz.

## <a name="windows-server-2019-support"></a>Windows Server 2019 desteği
Üzerinde Windows Server 2019 MABS V3 yüklenebilir. MABS V3 WS2019 ile kullanmak için işletim sistemi için işletim sisteminizi yüklenirken/yükseltilirken MABS V3 veya yükseltmeden önce WS2019 yüklenirken/yükseltilirken WS2016 V3 gönderin ya da yükseltme yapabilirsiniz.

MABS V3 tam bir sürüm olduğundan doğrudan Windows Server 2016, Windows Server 2019 yüklenebilir veya MABS V2'den yükseltilebilir. Yükseltme veya yedekleme sunucusu V3'ı yüklemeden önce yükleme önkoşulları hakkında okuyun.
Yüklemesi/yükseltmesi hakkında daha fazla bilgi için MABS adımları Bul [burada](https://docs.microsoft.com/azure/backup/backup-azure-microsoft-azure-backup#software-package).


> [!NOTE]
> 
> MABS, aynı kod tabanını System Center Data Protection Manager olarak sahiptir. Data Protection Manager 1807 için MABS v3 eşdeğerdir.

## <a name="next-steps"></a>Sonraki adımlar

Sunucunuzu hazırlama veya bir iş yükü korumaya başlamak öğrenin:
- [MABS V3'de bilinen sorunlar](backup-mabs-release-notes-v3.md)
- [Backup sunucusu iş yükleri hazırlama](backup-azure-microsoft-azure-backup.md)
- [Bir VMware sunucusunu yedeklemek için Backup sunucusu kullanma](backup-azure-backup-server-vmware.md)
- [SQL sunucusunu yedeklemek için Backup sunucusu kullanma](backup-azure-sql-mabs.md)
- [Modern yedekleme depolama alanı ile Backup sunucusu kullanma](backup-mabs-add-storage.md)
