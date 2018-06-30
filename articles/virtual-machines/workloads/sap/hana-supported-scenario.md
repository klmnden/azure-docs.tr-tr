---
title: Desteklenen senaryoları SAP HANA (büyük örnekler) Azure üzerinde | Microsoft Docs
description: Desteklenen senaryolar ve bunların mimarisi ayrıntılarını SAP HANA azure'da (büyük örnekleri)
services: virtual-machines-linux
documentationcenter: ''
author: saghorpa
manager: jeconnoc
editor: ''
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 06/27/2018
ms.author: saghorpa
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 656ba21abf06ad0f079e3ce425d3221724d195d4
ms.sourcegitcommit: 5a7f13ac706264a45538f6baeb8cf8f30c662f8f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/29/2018
ms.locfileid: "37113587"
---
# <a name="supported-scenarios-for-hana-large-instances"></a>HANA büyük örnekleri için desteklenen senaryolar
Bu belgede HANA büyük örnekleri (HLI) için Mimari ayrıntılarını birlikte desteklenen senaryolar açıklanmaktadır.

>[!NOTE]
>Gerekli senaryonuz burada belirtilen değil, gereksinimlerinizi değerlendirmek için Microsoft Hizmet Yönetimi ekibine başvurun.
Sağlama HLI birimiyle devam etmeden önce tasarım SAP veya hizmet uygulaması ortağınız ile doğrulayın.

## <a name="terms-and-definitions"></a>Terimleri ve tanımları
Terimleri ve tanımları belgede kullanılan anlayalım.

- SID: HANA sistem sistem tanımlayıcısı.
- HLI: Hana büyük örnekleri.
- DR: Bir olağanüstü durum kurtarma sitesi.
- Normal DR: bir sistem kurulum yalnızca kullanılan DR amaç için adanmış bir kaynakla.
- Çok amaçlı DR: Üretim ortamı dışındaki DR olaya kullanmak üzere yapılandırılmış üretim örneği ile birlikte kullanmak üzere yapılandırılmış DR sitede bir sistem. 
- Tek SID: Yüklü bir örneği ile bir sistem.
- Çoklu SID: Yapılandırılmış birden çok örneğiyle sistem. MCOS ortam olarak da bilinir.


## <a name="overview"></a>Genel Bakış
HANA büyük örnekleri mimarileri iş gereksinimlerinizi gerçekleştirmek için çeşitli destekler. Aşağıdaki listede senaryoları ve yapılandırma ayrıntılarını içerir. 

Türetilen Mimari Tasarım tamamen altyapı açısından olduğu ve HANA dağıtımı için SAP veya uygulama iş ortakları bakın. Senaryolarınız listelenmemişse mimarisi gözden geçirin ve sizin için bir çözüm türetilmesi için Microsoft hesap ekibi başvurun.

**Bu mimari TDI (özel veri tümleştirme) tasarım ile tamamen uyumlu olan ve SAP tarafından desteklenir.**

Bu belgede, desteklenen her mimari iki bileşenlerinde ayrıntılarını açıklanmaktadır:

- Ethernet
- Depolama

### <a name="ethernet"></a>Ethernet

Sağlanan her sunucu ethernet kümeleriyle önceden yapılandırılmış olarak gelir. Aşağıda, her HLI birimler üzerinde yapılandırılmış ethernet Ayrıntılar verilmiştir.

- **A**: Bu kullanılan/istemci erişim.
- **B**: Bu düğümden düğüme iletişimi için kullanılır. Bu (yedeklemiş istenen topolojisi) tüm sunucularda yapılandırılmış ancak yalnızca genişleme senaryoları için kullanılır.
- **C**: Bu arabirim depolama bağlantısı düğüme için kullanılır.
- **D**: Bu arabirim İSCSI cihaz bağlantısı STONITH kurulumu için düğüme için kullanılır. Bu arabirim, yalnızca HSR Kurulum istendiğinde yapılandırılır.  

| NIC MANTIKSAL BİRİMLERİ | SKU TÜRÜ | SUSE işletim sistemi adı | RHEL işletim sistemi adı | Kullanım örneği|
| --- | --- | --- | --- | --- |
| A | T TÜRÜ | eth0.tenant | eno1.tenant | HLI istemciye |
| B | T TÜRÜ | eth2.tenant | eno3.tenant | Düğüm düğüme |
| C | T TÜRÜ | eth1.tenant | eno2.tenant | Depolama düğümü |
| D | T TÜRÜ | eth4.tenant | eno4.tenant | STONITH |
| A | TYPE II | VLAN<tenantNo> | team0.tenant | HLI istemciye |
| B | TYPE II | VLAN < tenantNo + 2 > | team0.tenant + 2 | Düğüm düğüme |
| C | TYPE II | VLAN < tenantNo + 1 > | team0.tenant + 1 | Depolama düğümü |
| D | TYPE II | VLAN < tenantNo + 3 > | team0.tenant + 3 | STONITH |

HLI biriminde yapılandırılmış topoloji göre arabirimleri kullanır. Örneğin, "B" arabirimi yapılandırılmış bir genişleme topolojiye sahipseniz, yararlı olan düğümden düğüme iletişimi için ayarlanır. Tek düğüm büyütme yapılandırması söz konusu olduğunda, bu arabirim kullanılmaz. Arabirim kullanımı hakkında daha fazla bilgi almak için gerekli senaryolarınızı (Bu belgenin sonraki bölümlerinde) inceleyin. 

Gerekirse, Ek NIC kartları, kendi tanımlayabilirsiniz. Ancak, var olan Nic'leri yapılandırmasına değiştirilemez.

>[!NOTE]
>Fiziksel arabirimleri veya bağlama olan ek arabirimleri hala bulabilirsiniz. Kullanılan durumunuz için yukarıda belirtilen arabirimleri dikkate almanız gereken, rest dikkate / veya ile tempered değil.

İki IP adresi atanmış birimleriyle dağıtımı gibi görünmelidir:

- Ethernet "A", Microsoft'a gönderilen sunucu IP havuzu adres aralığı dışında atanmış bir IP adresi olmalıdır. Bu IP adresi/etc/hosts işletim sisteminin içinde sürdürmek için kullanılır.

- Ethernet "C" NFS iletişimi için kullanılan bir IP adresi olmalıdır. Bu nedenle, bu adresleri yapmak **değil** etc/hosts örneği, örnek trafiği Kiracı içinde izin vermek üzere saklanması gerekir.

Dağıtım durumlarda HANA sistem çoğaltma veya HANA genişleme, dikey yapılandırması atanan iki IP adreslerine sahip uygun değil. Yalnızca atanan iki IP adreslerine sahip olmasına ve isteyen bu tür bir yapılandırma dağıtmanız, SAP HANA üçüncü üçüncü bir IP adresi almak için Azure Hizmet Yönetimi başvurun VLAN atanmışsa. Üç NIC noktalarına atanan üç IP adreslerine sahip olmasına HANA büyük örneği birimleri için aşağıdaki kullanım kurallar geçerlidir:

- Ethernet "A", Microsoft'a gönderilen sunucu IP havuzu adres aralığı dışında atanmış bir IP adresi olmalıdır. Bu nedenle bu IP adresi/etc/hosts işletim sisteminin içinde sürdürmek için kullanılacak işaretçi yok.

- Ethernet "B", etc/hosts farklı örnekleri arasında iletişim için sürdürülebilmesi için özel olarak kullanılmalıdır. Bu adresler ayrıca genişleme HANA yapılandırmalarında HANA düğümler arası yapılandırmasını kullanan IP adresleri olarak güncelleştirilmesi gereken IP adresleri olacaktır.

- Ethernet "C" iletişim NFS depolama için kullanılan bir IP adresi olmalıdır. Bu nedenle bu tür adresleri etc/hosts saklanması gereken değil.

- Ethernet "D", pacemaker STONITH cihaza erişmek için özel olarak kullanılmalıdır. HANA sistemi çoğaltma (HSR) yapılandırmak ve bir temel SBD aygıtı kullanarak işletim sistemi otomatik yük devretme elde etmek istediğinizde, bu gereklidir.


### <a name="storage"></a>Depolama
Depolama istenen topolojisine bağlı önceden yapılandırılmıştır. Birim boyutlarını ve mountpoint sunucular, SKU'ları ve yapılandırılmış topoloji sayısına göre değişir. Daha fazla bilgi almak için gerekli senaryolarınızı (Bu belgenin sonraki bölümlerinde) inceleyin. Daha fazla depolama alanı gerekiyorsa, bir TB aralıkla satın alabilirsiniz.

>[!NOTE]
>Mountpoint/usr/sap/ <SID> /hana/paylaşılan mountpoint sembolik bir bağlantıdır.


## <a name="supported-scenarios"></a>Desteklenen senaryolar

Mimari diyagramlarında aşağıdaki gösterimler grafiklerini kullanılır:

![Legends.PNG](media/hana-supported-scenario/Legends.PNG)

Aşağıdaki listede, desteklenen senaryolar gösterilmektedir:

1. Tek düğümlü bir SID ile
2. Tek düğüm MCOS
3. DR (Normal) ile tek düğüm
4. DR (çok amaçlı) ile tek düğüm
5. HSR STONITH ile
6. DR ile HSR (Normal / çok amaçlı) 
7. Konak otomatik yük devretme (1 + 1) 
8. Bekleme ölçeğini genişletme
9. Bekleme ölçeğini genişletme
10. DR ile ölçeğini genişletme



## <a name="1-single-node-with-one-sid"></a>1. Tek düğümlü bir SID ile

Bu topoloji bir düğüm yapılandırması bir SID ile ölçek destekler.

### <a name="architecture-diagram"></a>Mimari diyagramı  

![Tek düğüm ile bir SID.png](media/hana-supported-scenario/Single-node-with-one-SID.png)

### <a name="ethernet"></a>Ethernet
Aşağıdaki ağ arabirimleri yapılandırılmış:

| NIC MANTIKSAL BİRİMLERİ | SKU TÜRÜ | SUSE işletim sistemi adı | RHEL işletim sistemi adı | Kullanım örneği|
| --- | --- | --- | --- | --- |
| A | T TÜRÜ | eth0.tenant | eno1.tenant | HLI istemciye |
| B | T TÜRÜ | eth2.tenant | eno3.tenant | Yapılandırılmış içinde değil kullanır |
| C | T TÜRÜ | eth1.tenant | eno2.tenant | Depolama düğümü |
| D | T TÜRÜ | eth4.tenant | eno4.tenant | Yapılandırılmış içinde değil kullanır |
| A | TYPE II | VLAN<tenantNo> | team0.tenant | HLI istemciye |
| B | TYPE II | VLAN < tenantNo + 2 > | team0.tenant + 2 | Yapılandırılmış içinde değil kullanır |
| C | TYPE II | VLAN < tenantNo + 1 > | team0.tenant + 1 | Depolama düğümü |
| D | TYPE II | VLAN < tenantNo + 3 > | team0.tenant + 3 | Yapılandırılmış içinde değil kullanır |

### <a name="storage"></a>Depolama
Aşağıdaki bağlama yapılandırılmış:

| Mountpoint | Kullanım örneği | 
| --- | --- |
|/hana/Shared/SID | HANA yükleme | 
|/hana/Data/SID/mnt00001 | Veri dosyalarını yükleme | 
|/hana/log/SID/mnt00001 | Günlük dosyalarını yükleyin | 
|/hana/logbackups/SID | Günlükleri Yinele |

### <a name="key-considerations"></a>Dikkat edilmesi gereken temel konular
- /usr/SAP/SID /hana/shared/SID sembolik bağlantısıdır.

## <a name="2-single-node-mcos"></a>2. Tek düğüm MCOS

Bu topoloji bir düğüm yapılandırması birden çok SID'lerle ölçeğinde destekler.

### <a name="architecture-diagram"></a>Mimari diyagramı  

![tek düğüm mcos.png](media/hana-supported-scenario/single-node-mcos.png)

### <a name="ethernet"></a>Ethernet
Aşağıdaki ağ arabirimleri yapılandırılmış:

| NIC MANTIKSAL BİRİMLERİ | SKU TÜRÜ | SUSE işletim sistemi adı | RHEL işletim sistemi adı | Kullanım örneği|
| --- | --- | --- | --- | --- |
| A | T TÜRÜ | eth0.tenant | eno1.tenant | HLI istemciye |
| B | T TÜRÜ | eth2.tenant | eno3.tenant | Yapılandırılmış içinde değil kullanır |
| C | T TÜRÜ | eth1.tenant | eno2.tenant | Depolama düğümü |
| D | T TÜRÜ | eth4.tenant | eno4.tenant | Yapılandırılmış içinde değil kullanır |
| A | TYPE II | VLAN<tenantNo> | team0.tenant | HLI istemciye |
| B | TYPE II | VLAN < tenantNo + 2 > | team0.tenant + 2 | Yapılandırılmış içinde değil kullanır |
| C | TYPE II | VLAN < tenantNo + 1 > | team0.tenant + 1 | Depolama düğümü |
| D | TYPE II | VLAN < tenantNo + 3 > | team0.tenant + 3 | Yapılandırılmış içinde değil kullanır |

### <a name="storage"></a>Depolama
Aşağıdaki bağlama yapılandırılmış:

| Mountpoint | Kullanım örneği | 
| --- | --- |
|/hana/Shared/SID1 | SID1 HANA yükleme | 
|/hana/Data/SID1/mnt00001 | Veri dosyaları için SID1 yükleyin | 
|/hana/log/SID1/mnt00001 | SID1 için günlük dosyalarını yükleyin | 
|/hana/logbackups/SID1 | Günlükleri için SID1 Yinele |
|/hana/Shared/SID2 | SID2 HANA yükleme | 
|/hana/Data/SID2/mnt00001 | Veri dosyaları için SID2 yükleyin | 
|/hana/log/SID2/mnt00001 | SID2 için günlük dosyalarını yükleyin | 
|/hana/logbackups/SID2 | Günlükleri için SID2 Yinele |

### <a name="key-considerations"></a>Dikkat edilmesi gereken temel konular
- /usr/SAP/SID /hana/shared/SID sembolik bağlantısıdır.
- Birim boyutu dağıtım veritabanı boyutu bellekte kapalı temel alır. Başvuru [genel bakış ve mimari](https://docs.microsoft.com/en-us/azure/virtual-machines/workloads/sap/hana-overview-architecture) bellekte hangi veritabanı boyutları öğrenmek için bölüm multisid ortamıyla desteklenir.

## <a name="3-single-node-with-dr-normal"></a>3. DR (Normal) ile tek düğüm
 
Bu topoloji bir düğüm yapılandırması bir veya birden çok SID'lerle birincil SID DR sitesi için depolama tabanlı çoğaltma ile ölçek destekler. Diyagramda, yalnızca tek SID birincil sitede belirtilmiştir, ancak multisid (MCOS) de desteklenir.

### <a name="architecture-diagram"></a>Mimari diyagramı  

![Tek düğüm ile dr.png](media/hana-supported-scenario/Single-node-with-dr.png)

### <a name="ethernet"></a>Ethernet
Aşağıdaki ağ arabirimleri yapılandırılmış:

| NIC MANTIKSAL BİRİMLERİ | SKU TÜRÜ | SUSE işletim sistemi adı | RHEL işletim sistemi adı | Kullanım örneği|
| --- | --- | --- | --- | --- |
| A | T TÜRÜ | eth0.tenant | eno1.tenant | HLI istemciye |
| B | T TÜRÜ | eth2.tenant | eno3.tenant | Yapılandırılmış içinde değil kullanır |
| C | T TÜRÜ | eth1.tenant | eno2.tenant | Depolama düğümü |
| D | T TÜRÜ | eth4.tenant | eno4.tenant | Yapılandırılmış içinde değil kullanır |
| A | TYPE II | VLAN<tenantNo> | team0.tenant | HLI istemciye |
| B | TYPE II | VLAN < tenantNo + 2 > | team0.tenant + 2 | Yapılandırılmış içinde değil kullanır |
| C | TYPE II | VLAN < tenantNo + 1 > | team0.tenant + 1 | Depolama düğümü |
| D | TYPE II | VLAN < tenantNo + 3 > | team0.tenant + 3 | Yapılandırılmış içinde değil kullanır |

### <a name="storage"></a>Depolama
Aşağıdaki bağlama yapılandırılmış:

| Mountpoint | Kullanım örneği | 
| --- | --- |
|/hana/Shared/SID | SID HANA yükleme | 
|/hana/Data/SID/mnt00001 | Veri dosyaları için SID yükleyin | 
|/hana/log/SID/mnt00001 | Günlük dosyaları için SID yükleyin | 
|/hana/logbackups/SID | Günlükleri için SID Yinele |


### <a name="key-considerations"></a>Dikkat edilmesi gereken temel konular
- /usr/SAP/SID /hana/shared/SID sembolik bağlantısıdır.
- MCOS için: Birim boyutu dağıtım veritabanı boyutu bellekte kapalı temel alır. Başvuru [genel bakış ve mimari](https://docs.microsoft.com/en-us/azure/virtual-machines/workloads/sap/hana-overview-architecture) bellekte hangi veritabanı boyutları öğrenmek için bölüm multisid ortamıyla desteklenir.
- DR sırasında: bağlama ve birimler ("HANA yükleme için gerekli"olarak işaretlenen) yapılandırılmış HANA örneği yükleme DR HLI biriminde üretim için. 
- DR sırasında: veri, logbackups ve paylaşılan birimler ("Depolama çoğaltma" işaretlenen) üretim sitesinden anlık görüntü aracılığıyla çoğaltılır. Bu birimleri, yalnızca yük devretme süre boyunca bağlanır. Başvuru [olağanüstü durum kurtarma yük devretme yordamı](https://docs.microsoft.com/en-us/azure/virtual-machines/workloads/sap/hana-overview-high-availability-disaster-recovery#disaster-recovery-failover-procedure) daha fazla ayrıntı için.
- Önyükleme birimi için **SKU ı sınıf türü** DR düğüme çoğaltılır.


## <a name="4-single-node-with-dr-multipurpose"></a>4. DR (çok amaçlı) ile tek düğüm
 
Bu topoloji bir düğüm yapılandırması bir veya birden çok SID'lerle birincil SID DR sitesi için depolama tabanlı çoğaltma ile ölçek destekler. Diyagramda, yalnızca tek SID birincil sitede belirtilmiştir, ancak multisid (MCOS) de desteklenir. Birincil siteden üretim işlemleri çalışırken DR sitede HLI birim QA örneği için kullanılır. DR yük devretme (veya yük devretme testi) aynı anda QA örneği DR siteye alınır.

### <a name="architecture-diagram"></a>Mimari diyagramı  

![tek düğüm ile dr multipurpose.png](media/hana-supported-scenario/single-node-with-dr-multipurpose.png)

### <a name="ethernet"></a>Ethernet
Aşağıdaki ağ arabirimleri yapılandırılmış:

| NIC MANTIKSAL BİRİMLERİ | SKU TÜRÜ | SUSE işletim sistemi adı | RHEL işletim sistemi adı | Kullanım örneği|
| --- | --- | --- | --- | --- |
| A | T TÜRÜ | eth0.tenant | eno1.tenant | HLI istemciye |
| B | T TÜRÜ | eth2.tenant | eno3.tenant | Yapılandırılmış içinde değil kullanır |
| C | T TÜRÜ | eth1.tenant | eno2.tenant | Depolama düğümü |
| D | T TÜRÜ | eth4.tenant | eno4.tenant | Yapılandırılmış içinde değil kullanır |
| A | TYPE II | VLAN<tenantNo> | team0.tenant | HLI istemciye |
| B | TYPE II | VLAN < tenantNo + 2 > | team0.tenant + 2 | Yapılandırılmış içinde değil kullanır |
| C | TYPE II | VLAN < tenantNo + 1 > | team0.tenant + 1 | Depolama düğümü |
| D | TYPE II | VLAN < tenantNo + 3 > | team0.tenant + 3 | Yapılandırılmış içinde değil kullanır |

### <a name="storage"></a>Depolama
Aşağıdaki bağlama yapılandırılmış:

| Mountpoint | Kullanım örneği | 
| --- | --- |
|**Birincil sitede**|
|/hana/Shared/SID | Üretim SID HANA yükleme | 
|/hana/Data/SID/mnt00001 | Üretim için SID veri dosyalarını yükleme | 
|/hana/log/SID/mnt00001 | SID üretim için günlük dosyalarını yükleyin | 
|/hana/logbackups/SID | Üretim SID günlüklerinde Yinele |
|**DR sitede**|
|/hana/Shared/SID | Üretim SID HANA yükleme | 
|/hana/Data/SID/mnt00001 | Üretim için SID veri dosyalarını yükleme | 
|/hana/log/SID/mnt00001 | SID üretim için günlük dosyalarını yükleyin | 
|/hana/Shared/QA-SID | QA SID HANA yükleme | 
|/hana/Data/QA-SID/mnt00001 | QA SID için veri dosyalarını yükleme | 
|/hana/log/QA-SID/mnt00001 | QA SID için günlük dosyalarını yükleyin |
|/hana/logbackups/QA-SID | Günlükleri için QA SID Yinele |

### <a name="key-considerations"></a>Dikkat edilmesi gereken temel konular
- /usr/SAP/SID /hana/shared/SID sembolik bağlantısıdır.
- MCOS için: Birim boyutu dağıtım veritabanı boyutu bellekte kapalı temel alır. Başvuru [genel bakış ve mimari](https://docs.microsoft.com/en-us/azure/virtual-machines/workloads/sap/hana-overview-architecture) bellekte hangi veritabanı boyutları öğrenmek için bölüm multisid ortamıyla desteklenir.
- DR sırasında: bağlama ve birimler ("HANA yükleme için gerekli"olarak işaretlenen) yapılandırılmış HANA örneği yükleme DR HLI biriminde üretim için. 
- DR sırasında: veri, logbackups ve paylaşılan birimler ("Depolama çoğaltma" işaretlenen) üretim sitesinden anlık görüntü aracılığıyla çoğaltılır. Bu birimleri, yalnızca yük devretme süre boyunca bağlanır. Başvuru [olağanüstü durum kurtarma yük devretme yordamı](https://docs.microsoft.com/en-us/azure/virtual-machines/workloads/sap/hana-overview-high-availability-disaster-recovery#disaster-recovery-failover-procedure) daha fazla ayrıntı için. 
- DR sırasında: verileri, logbackups, günlük, paylaşılan birimler ("QA örneği yükleme" olarak işaretlenen) QA için QA örneği yükleme için yapılandırılır.
- Önyükleme birimi için **SKU ı sınıf türü** DR düğüme çoğaltılır.

## <a name="5-hsr-with-stonith"></a>5. HSR STONITH ile
 
Bu topoloji iki düğüm HANA sistemi çoğaltma (HSR) yapılandırmasını destekler. 

**Şimdi itibariyle Bu mimarinin yalnızca SUSE işletim sistemi için desteklenir.**


### <a name="architecture-diagram"></a>Mimari diyagramı  

![STONITH.png ile HSR](media/hana-supported-scenario/HSR-with-STONITH.png)



### <a name="ethernet"></a>Ethernet
Aşağıdaki ağ arabirimleri yapılandırılmış:

| NIC MANTIKSAL BİRİMLERİ | SKU TÜRÜ | SUSE işletim sistemi adı | RHEL işletim sistemi adı | Kullanım örneği|
| --- | --- | --- | --- | --- |
| A | T TÜRÜ | eth0.tenant | eno1.tenant | HLI istemciye |
| B | T TÜRÜ | eth2.tenant | eno3.tenant | Yapılandırılmış içinde değil kullanır |
| C | T TÜRÜ | eth1.tenant | eno2.tenant | Depolama düğümü |
| D | T TÜRÜ | eth4.tenant | eno4.tenant | STONITH için kullanılır |
| A | TYPE II | VLAN<tenantNo> | team0.tenant | HLI istemciye |
| B | TYPE II | VLAN < tenantNo + 2 > | team0.tenant + 2 | Yapılandırılmış içinde değil kullanır |
| C | TYPE II | VLAN < tenantNo + 1 > | team0.tenant + 1 | Depolama düğümü |
| D | TYPE II | VLAN < tenantNo + 3 > | team0.tenant + 3 | STONITH için kullanılır |

### <a name="storage"></a>Depolama
Aşağıdaki bağlama yapılandırılmış:

| Mountpoint | Kullanım örneği | 
| --- | --- |
|**Birincil düğüm üzerinde**|
|/hana/Shared/SID | Üretim SID HANA yükleme | 
|/hana/Data/SID/mnt00001 | Üretim için SID veri dosyalarını yükleme | 
|/hana/log/SID/mnt00001 | SID üretim için günlük dosyalarını yükleyin | 
|/hana/logbackups/SID | Üretim SID günlüklerinde Yinele |
|**İkincil düğüm üzerinde**|
|/hana/Shared/SID | HANA yüklemek için ikincil SID | 
|/hana/Data/SID/mnt00001 | Veri dosyaları için ikincil SID yükleyin | 
|/hana/log/SID/mnt00001 | İçin ikincil SID günlük dosyalarını yükleyin | 
|/hana/logbackups/SID | Günlükleri için ikincil SID Yinele |

### <a name="key-considerations"></a>Dikkat edilmesi gereken temel konular
- /usr/SAP/SID /hana/shared/SID sembolik bağlantısıdır.
- MCOS için: Birim boyutu dağıtım veritabanı boyutu bellekte kapalı temel alır. Başvuru [genel bakış ve mimari](https://docs.microsoft.com/en-us/azure/virtual-machines/workloads/sap/hana-overview-architecture) bellekte hangi veritabanı boyutları öğrenmek için bölüm multisid ortamıyla desteklenir.
- STONITH: Bir SBD STONITH kurulumu için yapılandırılır. Ancak, STONITH kullanımı isteğe bağlıdır.


## <a name="6-hsr-with-dr"></a>6. DR ile HSR
 
Bu topoloji iki düğüm HANA sistemi çoğaltma (HSR) yapılandırmasını destekler. Hem normal ve çok amaçlı DR desteklenir. 

Çizimde, çok amaçlı senaryo DR sitesi yerlerde belirtilmiştir, üretim işlemlerini birincil siteden çalışırken HLI birim QA örneği için kullanılır. DR yük devretme (veya yük devretme testi) aynı anda QA örneği DR siteye alınır. 



### <a name="architecture-diagram"></a>Mimari diyagramı  

![DR.png ile HSR](media/hana-supported-scenario/HSR-with-DR.png)

### <a name="ethernet"></a>Ethernet
Aşağıdaki ağ arabirimleri yapılandırılmış:

| NIC MANTIKSAL BİRİMLERİ | SKU TÜRÜ | SUSE işletim sistemi adı | RHEL işletim sistemi adı | Kullanım örneği|
| --- | --- | --- | --- | --- |
| A | T TÜRÜ | eth0.tenant | eno1.tenant | HLI istemciye |
| B | T TÜRÜ | eth2.tenant | eno3.tenant | Yapılandırılmış içinde değil kullanır |
| C | T TÜRÜ | eth1.tenant | eno2.tenant | Depolama düğümü |
| D | T TÜRÜ | eth4.tenant | eno4.tenant | STONITH için kullanılır |
| A | TYPE II | VLAN<tenantNo> | team0.tenant | HLI istemciye |
| B | TYPE II | VLAN < tenantNo + 2 > | team0.tenant + 2 | Yapılandırılmış içinde değil kullanır |
| C | TYPE II | VLAN < tenantNo + 1 > | team0.tenant + 1 | Depolama düğümü |
| D | TYPE II | VLAN < tenantNo + 3 > | team0.tenant + 3 | STONITH için kullanılır |

### <a name="storage"></a>Depolama
Aşağıdaki bağlama yapılandırılmış:

| Mountpoint | Kullanım örneği | 
| --- | --- |
|**Birincil düğüm üzerinde birincil site**|
|/hana/Shared/SID | Üretim SID HANA yükleme | 
|/hana/Data/SID/mnt00001 | Üretim için SID veri dosyalarını yükleme | 
|/hana/log/SID/mnt00001 | SID üretim için günlük dosyalarını yükleyin | 
|/hana/logbackups/SID | Üretim SID günlüklerinde Yinele |
|**İkincil düğüm üzerinde birincil site**|
|/hana/Shared/SID | HANA yüklemek için ikincil SID | 
|/hana/Data/SID/mnt00001 | Veri dosyaları için ikincil SID yükleyin | 
|/hana/log/SID/mnt00001 | İçin ikincil SID günlük dosyalarını yükleyin | 
|/hana/logbackups/SID | Günlükleri için ikincil SID Yinele |
|**DR sitede**|
|/hana/Shared/SID | Üretim SID HANA yükleme | 
|/hana/Data/SID/mnt00001 | Üretim için SID veri dosyalarını yükleme | 
|/hana/log/SID/mnt00001 | SID üretim için günlük dosyalarını yükleyin | 
|/hana/Shared/QA-SID | QA SID HANA yükleme | 
|/hana/Data/QA-SID/mnt00001 | QA SID için veri dosyalarını yükleme | 
|/hana/log/QA-SID/mnt00001 | QA SID için günlük dosyalarını yükleyin |
|/hana/logbackups/QA-SID | Günlükleri için QA SID Yinele |

### <a name="key-considerations"></a>Dikkat edilmesi gereken temel konular
- /usr/SAP/SID /hana/shared/SID sembolik bağlantısıdır.
- MCOS için: Birim boyutu dağıtım veritabanı boyutu bellekte kapalı temel alır. Başvuru [genel bakış ve mimari](https://docs.microsoft.com/en-us/azure/virtual-machines/workloads/sap/hana-overview-architecture) bellekte hangi veritabanı boyutları öğrenmek için bölüm multisid ortamıyla desteklenir.
- STONITH: Bir SBD STONITH kurulumu için yapılandırılır. Ancak, STONITH kullanımı isteğe bağlıdır.
- DR sırasında: **iki depolama birimleri kümesi gereklidir** birincil ve ikincil düğüm çoğaltma için.
- DR sırasında: bağlama ve birimler ("HANA yükleme için gerekli"olarak işaretlenen) yapılandırılmış HANA örneği yükleme DR HLI biriminde üretim için. 
- DR sırasında: veri, logbackups ve paylaşılan birimler ("Depolama çoğaltma" işaretlenen) üretim sitesinden anlık görüntü aracılığıyla çoğaltılır. Bu birimleri, yalnızca yük devretme süre boyunca bağlanır. Başvuru [olağanüstü durum kurtarma yük devretme yordamı](https://docs.microsoft.com/en-us/azure/virtual-machines/workloads/sap/hana-overview-high-availability-disaster-recovery#disaster-recovery-failover-procedure) daha fazla ayrıntı için. 
- DR sırasında: verileri, logbackups, günlük, paylaşılan birimler ("QA örneği yükleme" olarak işaretlenen) QA için QA örneği yükleme için yapılandırılır.
- Önyükleme birimi için **SKU ı sınıf türü** DR düğüme çoğaltılır.


## <a name="7-host-auto-failover-11"></a>7. Konak otomatik yük devretme (1 + 1)
 
Bu topoloji, bir konak otomatik yük devretme yapılandırmasında iki düğüm destekler. Ana/çalışan rolü ve diğer bir düğümle bir yedek olarak yoktur. **SAP yalnızca S/4 HANA için bu senaryoyu destekler.** OSS Not başvurun "[2408419 - SAP S/4HANA - çok düğümlü Destek](https://launchpad.support.sap.com/#/notes/2408419)" daha fazla ayrıntı için.



### <a name="architecture-diagram"></a>Mimari diyagramı  

![SCA](media/hana-supported-scenario/scaleup-with-standby.png)

### <a name="ethernet"></a>Ethernet
Aşağıdaki ağ arabirimleri yapılandırılmış:

| NIC MANTIKSAL BİRİMLERİ | SKU TÜRÜ | SUSE işletim sistemi adı | RHEL işletim sistemi adı | Kullanım örneği|
| --- | --- | --- | --- | --- |
| A | T TÜRÜ | eth0.tenant | eno1.tenant | HLI istemciye |
| B | T TÜRÜ | eth2.tenant | eno3.tenant | Düğüm iletişimi düğüme |
| C | T TÜRÜ | eth1.tenant | eno2.tenant | Depolama düğümü |
| D | T TÜRÜ | eth4.tenant | eno4.tenant | Yapılandırılmış içinde değil kullanır |
| A | TYPE II | VLAN<tenantNo> | team0.tenant | HLI istemciye |
| B | TYPE II | VLAN < tenantNo + 2 > | team0.tenant + 2 | Düğüm iletişimi düğüme |
| C | TYPE II | VLAN < tenantNo + 1 > | team0.tenant + 1 | Depolama düğümü |
| D | TYPE II | VLAN < tenantNo + 3 > | team0.tenant + 3 | Yapılandırılmış içinde değil kullanır |

### <a name="storage"></a>Depolama
Aşağıdaki bağlama yapılandırılmış:

| Mountpoint | Kullanım örneği | 
| --- | --- |
|**Ana ve bekleme düğümlerinde**|
|/ hana/paylaşılan | Üretim SID HANA yükleme | 
|/hana/Data/SID/mnt00001 | Üretim için SID veri dosyalarını yükleme | 
|/hana/log/SID/mnt00001 | SID üretim için günlük dosyalarını yükleyin | 
|/hana/logbackups/SID | Üretim SID günlüklerinde Yinele |



### <a name="key-considerations"></a>Dikkat edilmesi gereken temel konular
- /usr/SAP/SID /hana/shared/SID sembolik bağlantısıdır.
- Bekleme: bağlama ve birimler ("HANA yükleme için gerekli"olarak işaretlenen) yapılandırılmış olan bekleme birim HANA örneği yüklenebilir.
 

## <a name="8-scale-out-with-standby"></a>8. Bekleme ölçeğini genişletme
 
Bu topoloji birden çok düğüm yapılandırması ölçeklendirme destekler. Yedek olarak yöneticisi rolünü, bir veya daha fazla düğüm çalışan rolü ile ve bir veya daha fazla düğüm tek bir düğüm yok. Ancak, süresi belirli bir anda yalnızca bir yönetici düğümü olabilir.


### <a name="architecture-diagram"></a>Mimari diyagramı  

![genişletme nm standby.png](media/hana-supported-scenario/scaleout-nm-standby.png)

### <a name="ethernet"></a>Ethernet
Aşağıdaki ağ arabirimleri yapılandırılmış:

| NIC MANTIKSAL BİRİMLERİ | SKU TÜRÜ | SUSE işletim sistemi adı | RHEL işletim sistemi adı | Kullanım örneği|
| --- | --- | --- | --- | --- |
| A | T TÜRÜ | eth0.tenant | eno1.tenant | HLI istemciye |
| B | T TÜRÜ | eth2.tenant | eno3.tenant | Düğüm iletişimi düğüme |
| C | T TÜRÜ | eth1.tenant | eno2.tenant | Depolama düğümü |
| D | T TÜRÜ | eth4.tenant | eno4.tenant | Yapılandırılmış içinde değil kullanır |
| A | TYPE II | VLAN<tenantNo> | team0.tenant | HLI istemciye |
| B | TYPE II | VLAN < tenantNo + 2 > | team0.tenant + 2 | Düğüm iletişimi düğüme |
| C | TYPE II | VLAN < tenantNo + 1 > | team0.tenant + 1 | Depolama düğümü |
| D | TYPE II | VLAN < tenantNo + 3 > | team0.tenant + 3 | Yapılandırılmış içinde değil kullanır |

### <a name="storage"></a>Depolama
Aşağıdaki bağlama yapılandırılmış:

| Mountpoint | Kullanım örneği | 
| --- | --- |
|**Master, çalışan ve bekleme düğümlerinde**|
|/ hana/paylaşılan | Üretim SID HANA yükleme | 
|/hana/Data/SID/mnt00001 | Üretim için SID veri dosyalarını yükleme | 
|/hana/log/SID/mnt00001 | SID üretim için günlük dosyalarını yükleyin | 
|/hana/logbackups/SID | Üretim SID günlüklerinde Yinele |


## <a name="9-scale-out-without-standby"></a>9. Bekleme ölçeğini genişletme
 
Bu topoloji birden çok düğüm yapılandırması ölçeklendirme destekler. Bir düğüm Yöneticisi rolüne sahip ve bir veya çalışan rolü ile modu düğüm yoktur. Ancak, süresi belirli bir anda yalnızca bir yönetici düğümü olabilir.


### <a name="architecture-diagram"></a>Mimari diyagramı  

![genişletme nm.png](media/hana-supported-scenario/scaleout-nm.png)


### <a name="ethernet"></a>Ethernet
Aşağıdaki ağ arabirimleri yapılandırılmış:

| NIC MANTIKSAL BİRİMLERİ | SKU TÜRÜ | SUSE işletim sistemi adı | RHEL işletim sistemi adı | Kullanım örneği|
| --- | --- | --- | --- | --- |
| A | T TÜRÜ | eth0.tenant | eno1.tenant | HLI istemciye |
| B | T TÜRÜ | eth2.tenant | eno3.tenant | Düğüm iletişimi düğüme |
| C | T TÜRÜ | eth1.tenant | eno2.tenant | Depolama düğümü |
| D | T TÜRÜ | eth4.tenant | eno4.tenant | Yapılandırılmış içinde değil kullanır |
| A | TYPE II | VLAN<tenantNo> | team0.tenant | HLI istemciye |
| B | TYPE II | VLAN < tenantNo + 2 > | team0.tenant + 2 | Düğüm iletişimi düğüme |
| C | TYPE II | VLAN < tenantNo + 1 > | team0.tenant + 1 | Depolama düğümü |
| D | TYPE II | VLAN < tenantNo + 3 > | team0.tenant + 3 | Yapılandırılmış içinde değil kullanır |

### <a name="storage"></a>Depolama
Aşağıdaki bağlama yapılandırılmış:

| Mountpoint | Kullanım örneği | 
| --- | --- |
|**Ana ve alt düğümlerinde**|
|/ hana/paylaşılan | Üretim SID HANA yükleme | 
|/hana/Data/SID/mnt00001 | Üretim için SID veri dosyalarını yükleme | 
|/hana/log/SID/mnt00001 | SID üretim için günlük dosyalarını yükleyin | 
|/hana/logbackups/SID | Üretim SID günlüklerinde Yinele |


### <a name="key-considerations"></a>Dikkat edilmesi gereken temel konular
- /usr/SAP/SID /hana/shared/SID sembolik bağlantısıdır.

## <a name="10-scale-out-with-dr"></a>10. DR ile ölçeğini genişletme
 
Bu topoloji, bir DR ile bir ölçeklendirme birden çok düğüm destekler. Normal ve çok amaçlı DR desteklenir. Aşağıdaki çizimde, yalnızca tek amaçlı DR belirtilmiştir. Bu topoloji olan veya olmayan bir bekleme düğümünün isteyebilir.


### <a name="architecture-diagram"></a>Mimari diyagramı  

![dr.png ile genişletme](media/hana-supported-scenario/scaleout-with-dr.png)


### <a name="ethernet"></a>Ethernet
Aşağıdaki ağ arabirimleri yapılandırılmış:

| NIC MANTIKSAL BİRİMLERİ | SKU TÜRÜ | SUSE işletim sistemi adı | RHEL işletim sistemi adı | Kullanım örneği|
| --- | --- | --- | --- | --- |
| A | T TÜRÜ | eth0.tenant | eno1.tenant | HLI istemciye |
| B | T TÜRÜ | eth2.tenant | eno3.tenant | Düğüm iletişimi düğüme |
| C | T TÜRÜ | eth1.tenant | eno2.tenant | Depolama düğümü |
| D | T TÜRÜ | eth4.tenant | eno4.tenant | Yapılandırılmış içinde değil kullanır |
| A | TYPE II | VLAN<tenantNo> | team0.tenant | HLI istemciye |
| B | TYPE II | VLAN < tenantNo + 2 > | team0.tenant + 2 | Düğüm iletişimi düğüme |
| C | TYPE II | VLAN < tenantNo + 1 > | team0.tenant + 1 | Depolama düğümü |
| D | TYPE II | VLAN < tenantNo + 3 > | team0.tenant + 3 | Yapılandırılmış içinde değil kullanır |

### <a name="storage"></a>Depolama
Aşağıdaki bağlama yapılandırılmış:

| Mountpoint | Kullanım örneği | 
| --- | --- |
|**Birincil düğüm üzerinde**|
|/ hana/paylaşılan | Üretim SID HANA yükleme | 
|/hana/Data/SID/mnt00001 | Üretim için SID veri dosyalarını yükleme | 
|/hana/log/SID/mnt00001 | SID üretim için günlük dosyalarını yükleyin | 
|/hana/logbackups/SID | Üretim SID günlüklerinde Yinele |
|**DR düğümde**|
|/ hana/paylaşılan | Üretim SID HANA yükleme | 
|/hana/Data/SID/mnt00001 | Üretim için SID veri dosyalarını yükleme | 
|/hana/log/SID/mnt00001 | SID üretim için günlük dosyalarını yükleyin | 


### <a name="key-considerations"></a>Dikkat edilmesi gereken temel konular
- /usr/SAP/SID /hana/shared/SID sembolik bağlantısıdır.
-  DR sırasında: bağlama ve birimler ("HANA yükleme için gerekli"olarak işaretlenen) yapılandırılmış HANA örneği yükleme DR HLI biriminde üretim için. 
- DR sırasında: veri, logbackups ve paylaşılan birimler ("Depolama çoğaltma" işaretlenen) üretim sitesinden anlık görüntü aracılığıyla çoğaltılır. Bu birimleri, yalnızca yük devretme süre boyunca bağlanır. Başvuru [olağanüstü durum kurtarma yük devretme yordamı](https://docs.microsoft.com/en-us/azure/virtual-machines/workloads/sap/hana-overview-high-availability-disaster-recovery#disaster-recovery-failover-procedure) daha fazla ayrıntı için. 
- Önyükleme birimi için **SKU ı sınıf türü** DR düğüme çoğaltılır.


## <a name="next-steps"></a>Sonraki adımlar
- Başvuru [altyapı ve bağlantı](https://docs.microsoft.com/en-us/azure/virtual-machines/workloads/sap/hana-overview-infrastructure-connectivity) HLI için
- Başvuru [yüksek kullanılabilirlik ve olağanüstü durum kurtarma](https://docs.microsoft.com/en-us/azure/virtual-machines/workloads/sap/hana-overview-high-availability-disaster-recovery) HLI için