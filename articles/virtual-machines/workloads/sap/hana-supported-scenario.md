---
title: Desteklenen senaryoları SAP HANA (büyük örnekler) Azure'da | Microsoft Docs
description: Desteklenen senaryolar ve mimari ayrıntılarının (büyük örnekler) Azure üzerinde SAP HANA için
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
ms.date: 07/06/2018
ms.author: saghorpa
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 501c5ffa86f2360e44c187e087f7285bbf4084fd
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60477785"
---
# <a name="supported-scenarios-for-hana-large-instances"></a>HANA büyük örnekler için desteklenen senaryolar
Bu belgede, HANA büyük örnekleri (HLI) için kendi mimari ayrıntılarıyla birlikte desteklenen senaryolar açıklanmaktadır.

>[!NOTE]
>Gerekli senaryonuz burada belirtilmezse, gereksinimlerinizi değerlendirmek için Microsoft Service Management ekibiyle iletişim kurun.
Sağlama HLI birim devam etmeden önce tasarım SAP veya hizmet uygulama iş ortağınız ile doğrulayın.

## <a name="terms-and-definitions"></a>Terimleri ve tanımları
Terimleri ve tanımları da belgede kullanılan bakalım.

- SID: HANA sistemi için sistem tanımlayıcısı.
- HLI: Hana büyük örnekleri.
- DR: Bir olağanüstü durum kurtarma sitesi.
- Normal DR: Yalnızca kullanılan DR amaç için adanmış bir kaynak sistemi kuruluma.
- Çok amaçlı DR: Bir sistem DR sitesinde DR olay için kullanmak üzere yapılandırılmış üretim örneği ile birlikte üretim dışı ortamda kullanmak üzere yapılandırılmış. 
- Çoklu SID:  Bir örnek yüklü sistemiyle.
- Çoklu SID: Yapılandırılmış birden çok örnek ile sistem. MCOS ortam olarak da adlandırılır.


## <a name="overview"></a>Genel Bakış
HANA büyük örnekleri mimarileri iş gereksinimlerinizi gerçekleştirmek için çeşitli destekler. Aşağıdaki listede, senaryoları ve yapılandırma ayrıntılarını içerir. 

Türetilmiş Mimari Tasarım tamamen altyapı açısından olduğu ve HANA dağıtım için SAP veya uygulama iş ortaklarınızla başvurun. Senaryolarınız listelenmeyen mimariyi gözden geçirmeniz ve sizin için bir çözüm türetmek için Microsoft hesap ekibinize başvurun.

**Bu mimariyi TDI (veri tümleştirme uyarlanmış) tasarım ile tamamen uyumlu ve SAP tarafından desteklenir.**

Bu belge, desteklenen her mimari iki bileşenlerin ayrıntılarını açıklar:

- Ethernet
- Depolama

### <a name="ethernet"></a>Ethernet

Sağlanan her bir sunucu ethernet arabirimleri kümesi ile önceden yapılandırılmış olarak gelir. Her HLI birim üzerinde yapılandırılmış ethernet arabirimleri ilişkin ayrıntılar aşağıdadır.

- **A**: Bu arabirim, istemci erişimi için/tarafından kullanılır.
- **B**: Bu arabirim, düğümden düğüme iletişim için kullanılır. Bu arabirim (istenen topolojisi) bağımsız olarak tüm sunucularda yapılandırılmış ancak için yalnızca kullanılan 
- Ölçek genişletme senaryoları.
- **C**: Bu arabirim, düğümün depolama bağlantısı için kullanılır.
- **D**: Bu arabirim, İSCSI STONITH kurulumu için cihazı bağlantı düğümü için kullanılır. Bu arabirim, yalnızca HSR Kurulum istendiğinde yapılandırılır.  

| NIC MANTIKSAL BİRİMLERİ | SKU TÜRÜ | SUSE işletim sistemi adı | RHEL işletim sistemi adı | Kullanım örneği|
| --- | --- | --- | --- | --- |
| A | BEN YAZIN | eth0.tenant | eno1.tenant | HLI istemcisi |
| B | BEN YAZIN | eth2.tenant | eno3.tenant | Düğüm için düğüm |
| C | BEN YAZIN | eth1.tenant | eno2.tenant | Depolama düğümü |
| D | BEN YAZIN | eth4.tenant | eno4.tenant | STONITH |
| A | TYPE II | VLAN\<tenantNo > | team0.tenant | HLI istemcisi |
| B | TYPE II | vlan\<tenantNo+2> | team0.tenant + 2 | Düğüm için düğüm |
| C | TYPE II | VLAN\<tenantNo + 1 > | team0.tenant + 1 | Depolama düğümü |
| D | TYPE II | VLAN\<tenantNo + 3 > | team0.tenant + 3 | STONITH |

HLI biriminde yapılandırılmış topolojisi temel arabirimler kullanırsınız. Örneğin, "B" arabirimi yapılandırılmış bir ölçek genişletme topolojiye sahipseniz, kullanışlı olan düğümden düğüme iletişimi için ayarlanır. Tek düğümlü ölçek büyütme yapılandırmasını söz konusu olduğunda, bu arabirim kullanılmaz. Arabirim kullanımı hakkında daha fazla bilgi almak için gerekli senaryolarınızı (Bu belgenin ilerleyen bölümlerinde) gözden geçirin. 

Gerekirse, Ek NIC kartları kendiniz tanımlayabilirsiniz. Bununla birlikte, mevcut Nic'leri yapılandırmasına değiştirilemez.

>[!NOTE]
>Fiziksel arabirimleri veya bağlama olan ek arabirimleri yine de bulabilirsiniz. Kullanılan durumunuz için yukarıda belirtilen arabirimleri dikkate almanız gereken, rest yoksayılan / veya ile geliştirildiğinde değil.

Dağıtım için iki IP adresi atanmış olan birimleri gibi görünmelidir:

- Ethernet "A", Microsoft'a gönderilen sunucu IP havuzu adres aralığı dışında bir IP adresi atanmış olmalıdır. Bu IP adresi/etc/hosts işletim sistemi içinde korumak için kullanılır.

- Ethernet "C", NFS için iletişim için kullanılan bir IP adresi atanmış olmalıdır. Bu nedenle, bu adresleri yapmak **değil** etc/hosts, kiracıda örneği, örnek trafiğine izin vermek için saklanması gerekir.

HANA sistem çoğaltması veya HANA genişleme dağıtımı durumlarda iki IP adresi atanmış olan bir dikey pencere yapılandırmasına uygun değil. VLAN atanmışsa iki IP adresi yalnızca atanmış olması ve bu tür bir yapılandırma dağıtmak isteyen bir üçüncü üçüncü bir IP adresi almak için Azure hizmet yönetimi üzerinde SAP HANA ile iletişime geçin. Üç NIC bağlantı noktalarında atanmış üç IP adreslerine sahip olmasına HANA büyük örneği birimleri için aşağıdaki kullanım kuralları geçerlidir:

- Ethernet "A", Microsoft'a gönderilen sunucu IP havuzu adres aralığı dışında bir IP adresi atanmış olmalıdır. Bu nedenle bu IP adresi/etc/hosts işletim sistemi koruma için kullanılmaması.

- Ethernet "B", vb./ana bilgisayarları farklı örnekleri arasında iletişim için tutulması için özel olarak kullanılmalıdır. Bu adresler ayrıca genişleme HANA yapılandırmalarında HANA düğümler arası yapılandırmasını kullanan IP adresleri olarak güncelleştirilmesi gereken IP adresleri olacaktır.

- Ethernet "C", NFS depolama iletişimi için kullanılan bir IP adresi atanmış olmalıdır. Bu nedenle bu tür bir adresleri etc/hosts bulunacak değil.

- Ethernet "D" pacemaker için STONITH cihaza erişmek için özel olarak kullanılmalıdır. Bu arabirim, HANA sistem çoğaltması (HSR) yapılandırmak ve temel SBD cihaz kullanarak işletim sistemi otomatik yük devretme elde etmek istiyorsanız gereklidir.


### <a name="storage"></a>Depolama
Depolama, istenen topolojisini temel önceden yapılandırılmıştır. Birim boyutları ve takma noktası sunucuları, SKU'ları ve yapılandırılmış topolojisi sayısına göre değişir. Daha fazla bilgi almak için gerekli senaryolarınızı (Bu belgenin ilerleyen bölümlerinde) gözden geçirin. Daha fazla depolama alanı gerekiyorsa, bir TB artış satın alabilirsiniz.

>[!NOTE]
>Takmanoktası/usr/sap/\<SID >/hana/paylaşılan mountpoint sembolik bir bağlantıdır.


## <a name="supported-scenarios"></a>Desteklenen senaryolar

Mimari diyagramları aşağıdaki gösterimler grafiklerini kullanılır:

![Legends.PNG](media/hana-supported-scenario/Legends.PNG)

Aşağıdaki listede desteklenen senaryolar gösterilmektedir:

1. Tek düğümlü bir SID ile
2. Tek düğüm MCOS
3. DR (Normal) ile tek düğüm
4. DR (çok amaçlı) ile tek düğüm
5. STONITH ile HSR
6. DR ile HSR (Normal / çok amaçlı) 
7. Ana bilgisayar otomatik yük devretme (1 + 1) 
8. Beklemeyle ölçeklendirme
9. Ölçek genişletme bekleme
10. DR ile ölçeklendirme



## <a name="1-single-node-with-one-sid"></a>1. Tek düğümlü bir SID ile

Bu topoloji, bir düğüm yapılandırması bir SID ile bir ölçek destekler.

### <a name="architecture-diagram"></a>Mimari diyagramı  

![Tek düğüm ile tek SID.png](media/hana-supported-scenario/Single-node-with-one-SID.png)

### <a name="ethernet"></a>Ethernet
Önceden yapılandırılmış ağ arabirimleri:

| NIC MANTIKSAL BİRİMLERİ | SKU TÜRÜ | SUSE işletim sistemi adı | RHEL işletim sistemi adı | Kullanım örneği|
| --- | --- | --- | --- | --- |
| A | BEN YAZIN | eth0.tenant | eno1.tenant | HLI istemcisi |
| B | BEN YAZIN | eth2.tenant | eno3.tenant | Kullanımda olmayan yapılandırılmış |
| C | BEN YAZIN | eth1.tenant | eno2.tenant | Depolama düğümü |
| D | BEN YAZIN | eth4.tenant | eno4.tenant | Kullanımda olmayan yapılandırılmış |
| A | TYPE II | VLAN\<tenantNo > | team0.tenant | HLI istemcisi |
| B | TYPE II | vlan\<tenantNo+2> | team0.tenant + 2 | Kullanımda olmayan yapılandırılmış |
| C | TYPE II | VLAN\<tenantNo + 1 > | team0.tenant + 1 | Depolama düğümü |
| D | TYPE II | VLAN\<tenantNo + 3 > | team0.tenant + 3 | Kullanımda olmayan yapılandırılmış |

### <a name="storage"></a>Depolama
Aşağıdaki bağlama yapılandırılmış:

| Takma noktası | Kullanım örneği | 
| --- | --- |
|/hana/Shared/SID | HANA yükleyin | 
|/hana/Data/SID/mnt00001 | Veri dosyalarını yükleme | 
|/hana/log/SID/mnt00001 | Günlük dosyaları yükleme | 
|/hana/logbackups/SID | Günlükleri Yinele |

### <a name="key-considerations"></a>Dikkat edilmesi gereken temel konular
- /usr/SAP/SID /hana/shared/SID sembolik bir bağlantıdır.

## <a name="2-single-node-mcos"></a>2. Tek düğüm MCOS

Bu topoloji, bir düğüm yapılandırması çoklu SID ile bir ölçek destekler.

### <a name="architecture-diagram"></a>Mimari diyagramı  

![single-node-mcos.png](media/hana-supported-scenario/single-node-mcos.png)

### <a name="ethernet"></a>Ethernet
Önceden yapılandırılmış ağ arabirimleri:

| NIC MANTIKSAL BİRİMLERİ | SKU TÜRÜ | SUSE işletim sistemi adı | RHEL işletim sistemi adı | Kullanım örneği|
| --- | --- | --- | --- | --- |
| A | BEN YAZIN | eth0.tenant | eno1.tenant | HLI istemcisi |
| B | BEN YAZIN | eth2.tenant | eno3.tenant | Kullanımda olmayan yapılandırılmış |
| C | BEN YAZIN | eth1.tenant | eno2.tenant | Depolama düğümü |
| D | BEN YAZIN | eth4.tenant | eno4.tenant | Kullanımda olmayan yapılandırılmış |
| A | TYPE II | VLAN\<tenantNo > | team0.tenant | HLI istemcisi |
| B | TYPE II | vlan\<tenantNo+2> | team0.tenant + 2 | Kullanımda olmayan yapılandırılmış |
| C | TYPE II | VLAN\<tenantNo + 1 > | team0.tenant + 1 | Depolama düğümü |
| D | TYPE II | VLAN\<tenantNo + 3 > | team0.tenant + 3 | Kullanımda olmayan yapılandırılmış |

### <a name="storage"></a>Depolama
Aşağıdaki bağlama yapılandırılmış:

| Takma noktası | Kullanım örneği | 
| --- | --- |
|/hana/Shared/SID1 | SID1 HANA yükleme | 
|/hana/Data/SID1/mnt00001 | Veri dosyaları için SID1 yükleyin | 
|/hana/log/SID1/mnt00001 | Günlük dosyaları için SID1 yükleyin | 
|/hana/logbackups/SID1 | Günlükler için SID1 Yinele |
|/hana/Shared/SID2 | SID2 HANA yükleme | 
|/hana/Data/SID2/mnt00001 | Veri dosyaları için SID2 yükleyin | 
|/hana/log/SID2/mnt00001 | Günlük dosyaları için SID2 yükleyin | 
|/hana/logbackups/SID2 | Günlükler için SID2 Yinele |

### <a name="key-considerations"></a>Dikkat edilmesi gereken temel konular
- /usr/SAP/SID /hana/shared/SID sembolik bir bağlantıdır.
- Birim boyutu dağıtım kapalı bellek veritabanı boyutunu temel alır. Başvuru [genel bakışı ve mimarisi](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-overview-architecture) bellekte hangi veritabanı boyutları öğrenmek için bölüm multisid ortamı ile desteklenir.

## <a name="3-single-node-with-dr-normal"></a>3. DR (Normal) ile tek düğüm
 
Bu topoloji, bir düğüm yapılandırması birincil SID DR sitesine depolama tabanlı çoğaltma ile bir veya birden çok SID ile bir ölçek destekler. Diyagram birincil sitede yalnızca tek bir SID belirtilmiştir, ancak multisid (MCOS) de desteklenir.

### <a name="architecture-diagram"></a>Mimari diyagramı  

![Single-node-with-dr.png](media/hana-supported-scenario/Single-node-with-dr.png)

### <a name="ethernet"></a>Ethernet
Önceden yapılandırılmış ağ arabirimleri:

| NIC MANTIKSAL BİRİMLERİ | SKU TÜRÜ | SUSE işletim sistemi adı | RHEL işletim sistemi adı | Kullanım örneği|
| --- | --- | --- | --- | --- |
| A | BEN YAZIN | eth0.tenant | eno1.tenant | HLI istemcisi |
| B | BEN YAZIN | eth2.tenant | eno3.tenant | Kullanımda olmayan yapılandırılmış |
| C | BEN YAZIN | eth1.tenant | eno2.tenant | Depolama düğümü |
| D | BEN YAZIN | eth4.tenant | eno4.tenant | Kullanımda olmayan yapılandırılmış |
| A | TYPE II | VLAN\<tenantNo > | team0.tenant | HLI istemcisi |
| B | TYPE II | vlan\<tenantNo+2> | team0.tenant + 2 | Kullanımda olmayan yapılandırılmış |
| C | TYPE II | VLAN\<tenantNo + 1 > | team0.tenant + 1 | Depolama düğümü |
| D | TYPE II | VLAN\<tenantNo + 3 > | team0.tenant + 3 | Kullanımda olmayan yapılandırılmış |

### <a name="storage"></a>Depolama
Aşağıdaki bağlama yapılandırılmış:

| Takma noktası | Kullanım örneği | 
| --- | --- |
|/hana/Shared/SID | SID HANA yükleme | 
|/hana/Data/SID/mnt00001 | Veri dosyaları için SID yükleyin | 
|/hana/log/SID/mnt00001 | Günlük dosyaları için SID yükleyin | 
|/hana/logbackups/SID | Günlükler için SID Yinele |


### <a name="key-considerations"></a>Dikkat edilmesi gereken temel konular
- /usr/SAP/SID /hana/shared/SID sembolik bir bağlantıdır.
- MCOS için: Birim boyutu dağıtım kapalı bellek veritabanı boyutunu temel alır. Başvuru [genel bakışı ve mimarisi](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-overview-architecture) bellekte hangi veritabanı boyutları öğrenmek için bölüm multisid ortamı ile desteklenir.
- DR sırasında: Bağlama ve birimler ("HANA yüklemesi için gerekli olarak" işaretlenmiştir) yapılandırılmış olan üretim DR HLI biriminde HANA örneği yükleme için. 
- DR sırasında: Anlık görüntüden üretim sitesini aracılığıyla veri logbackups ve paylaşılan birimler ("Depolama çoğaltma" işaretlenmiştir) çoğaltılır. Bu birimleri, yalnızca yük devretme süre boyunca bağlanır. Daha fazla bilgi için belgeyi okumak [olağanüstü durum kurtarma yük devretme yordamı](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-overview-high-availability-disaster-recovery) daha fazla ayrıntı için.
- Önyükleme birimi için **SKU miyim sınıf türü** DR düğüme çoğaltılır.


## <a name="4-single-node-with-dr-multipurpose"></a>4. DR (çok amaçlı) ile tek düğüm
 
Bu topoloji, bir düğüm yapılandırması birincil SID DR sitesine depolama tabanlı çoğaltma ile bir veya birden çok SID ile bir ölçek destekler. Diyagram birincil sitede yalnızca tek bir SID belirtilmiştir, ancak multisid (MCOS) de desteklenir. Birincil siteden üretim işlemleri çalıştırırken DR sitede HLI birim QA örneği için kullanılır. DR Yük Devretmesini (veya yük devretme testi) zaman QA örnek kurtarma sitesinde alınır.

### <a name="architecture-diagram"></a>Mimari diyagramı  

![tek düğüm ile dr multipurpose.png](media/hana-supported-scenario/single-node-with-dr-multipurpose.png)

### <a name="ethernet"></a>Ethernet
Önceden yapılandırılmış ağ arabirimleri:

| NIC MANTIKSAL BİRİMLERİ | SKU TÜRÜ | SUSE işletim sistemi adı | RHEL işletim sistemi adı | Kullanım örneği|
| --- | --- | --- | --- | --- |
| A | BEN YAZIN | eth0.tenant | eno1.tenant | HLI istemcisi |
| B | BEN YAZIN | eth2.tenant | eno3.tenant | Kullanımda olmayan yapılandırılmış |
| C | BEN YAZIN | eth1.tenant | eno2.tenant | Depolama düğümü |
| D | BEN YAZIN | eth4.tenant | eno4.tenant | Kullanımda olmayan yapılandırılmış |
| A | TYPE II | VLAN\<tenantNo > | team0.tenant | HLI istemcisi |
| B | TYPE II | vlan\<tenantNo+2> | team0.tenant + 2 | Kullanımda olmayan yapılandırılmış |
| C | TYPE II | VLAN\<tenantNo + 1 > | team0.tenant + 1 | Depolama düğümü |
| D | TYPE II | VLAN\<tenantNo + 3 > | team0.tenant + 3 | Kullanımda olmayan yapılandırılmış |

### <a name="storage"></a>Depolama
Aşağıdaki bağlama yapılandırılmış:

| Takma noktası | Kullanım örneği | 
| --- | --- |
|**Birincil sitede**|
|/hana/Shared/SID | Üretim SID için HANA yükleyin | 
|/hana/Data/SID/mnt00001 | Üretim için SID veri dosyalarını yükleme | 
|/hana/log/SID/mnt00001 | SID üretim için günlük dosyalarını yükleme | 
|/hana/logbackups/SID | Üretim SID günlüklerini Yinele |
|**DR sitede**|
|/hana/Shared/SID | Üretim SID için HANA yükleyin | 
|/hana/Data/SID/mnt00001 | Üretim için SID veri dosyalarını yükleme | 
|/hana/log/SID/mnt00001 | SID üretim için günlük dosyalarını yükleme | 
|/hana/Shared/QA-SID | QA SID HANA yükleme | 
|/hana/Data/QA-SID/mnt00001 | QA SID veri dosyalarını yükleme | 
|/hana/log/QA-SID/mnt00001 | Günlük dosyaları için QA SID yükleyin |
|/hana/logbackups/QA-SID | QA SID günlükleri Yinele |

### <a name="key-considerations"></a>Dikkat edilmesi gereken temel konular
- /usr/SAP/SID /hana/shared/SID sembolik bir bağlantıdır.
- MCOS için: Birim boyutu dağıtım kapalı bellek veritabanı boyutunu temel alır. Başvuru [genel bakışı ve mimarisi](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-overview-architecture) bellekte hangi veritabanı boyutları öğrenmek için bölüm multisid ortamı ile desteklenir.
- DR sırasında: Bağlama ve birimler ("HANA yüklemesi için gerekli olarak" işaretlenmiştir) yapılandırılmış olan üretim DR HLI biriminde HANA örneği yükleme için. 
- DR sırasında: Anlık görüntüden üretim sitesini aracılığıyla veri logbackups ve paylaşılan birimler ("Depolama çoğaltma" işaretlenmiştir) çoğaltılır. Bu birimleri, yalnızca yük devretme süre boyunca bağlanır. Daha fazla bilgi için belgeyi okumak [olağanüstü durum kurtarma yük devretme yordamı](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-overview-high-availability-disaster-recovery) daha fazla ayrıntı için. 
- DR sırasında: Veriler, logbackups, günlük, QA ("QA örneği yükleme" işaretlenmiştir) için paylaşılan birimler, QA örneği yükleme için yapılandırılır.
- Önyükleme birimi için **SKU miyim sınıf türü** DR düğüme çoğaltılır.

## <a name="5-hsr-with-stonith"></a>5. STONITH ile HSR
 
Bu topoloji, HANA sistem çoğaltması (HSR) yapılandırma için iki düğüm destekler. Bu yapılandırma, yalnızca bir düğümde tek HANA örnekleri için desteklenir. Anlamına gelir, MCOS senaryoları desteklenmez.

**Şu anda, bu mimari yalnızca SUSE işletim sistemi için desteklenir.**


### <a name="architecture-diagram"></a>Mimari diyagramı  

![HSR-with-STONITH.png](media/hana-supported-scenario/HSR-with-STONITH.png)



### <a name="ethernet"></a>Ethernet
Önceden yapılandırılmış ağ arabirimleri:

| NIC MANTIKSAL BİRİMLERİ | SKU TÜRÜ | SUSE işletim sistemi adı | RHEL işletim sistemi adı | Kullanım örneği|
| --- | --- | --- | --- | --- |
| A | BEN YAZIN | eth0.tenant | eno1.tenant | HLI istemcisi |
| B | BEN YAZIN | eth2.tenant | eno3.tenant | Kullanımda olmayan yapılandırılmış |
| C | BEN YAZIN | eth1.tenant | eno2.tenant | Depolama düğümü |
| D | BEN YAZIN | eth4.tenant | eno4.tenant | STONITH için kullanılan |
| A | TYPE II | VLAN\<tenantNo > | team0.tenant | HLI istemcisi |
| B | TYPE II | vlan\<tenantNo+2> | team0.tenant + 2 | Kullanımda olmayan yapılandırılmış |
| C | TYPE II | VLAN\<tenantNo + 1 > | team0.tenant + 1 | Depolama düğümü |
| D | TYPE II | VLAN\<tenantNo + 3 > | team0.tenant + 3 | STONITH için kullanılan |

### <a name="storage"></a>Depolama
Aşağıdaki bağlama yapılandırılmış:

| Takma noktası | Kullanım örneği | 
| --- | --- |
|**Birincil düğüm üzerinde**|
|/hana/Shared/SID | Üretim SID için HANA yükleyin | 
|/hana/Data/SID/mnt00001 | Üretim için SID veri dosyalarını yükleme | 
|/hana/log/SID/mnt00001 | SID üretim için günlük dosyalarını yükleme | 
|/hana/logbackups/SID | Üretim SID günlüklerini Yinele |
|**İkincil düğümünde**|
|/hana/Shared/SID | HANA yüklemek için ikincil SID | 
|/hana/Data/SID/mnt00001 | Veri dosyaları yüklemek için ikincil SID | 
|/hana/log/SID/mnt00001 | Günlük dosyaları için ikincil SID yükleyin | 
|/hana/logbackups/SID | Günlükler için ikincil SID Yinele |

### <a name="key-considerations"></a>Dikkat edilmesi gereken temel konular
- /usr/SAP/SID /hana/shared/SID sembolik bir bağlantıdır.
- MCOS için: Birim boyutu dağıtım kapalı bellek veritabanı boyutunu temel alır. Başvuru [genel bakışı ve mimarisi](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-overview-architecture) bellekte hangi veritabanı boyutları öğrenmek için bölüm multisid ortamı ile desteklenir.
- STONITH: Bir SBD STONITH kurulumu için yapılandırılır. Ancak, STONITH kullanımı isteğe bağlıdır.


## <a name="6-hsr-with-dr"></a>6. DR ile HSR
 
Bu topoloji, HANA sistem çoğaltması (HSR) yapılandırma için iki düğüm destekler. Hem normal hem de çok amaçlı DR desteklenir. Bu yapılandırmalar yalnızca bir düğümde tek HANA örnekleri için desteklenir. Anlamına gelir, bu yapılandırmalar ile MCOS senaryoları desteklenmez.

Çizimde, çok amaçlı senaryo DR sitesinde burada belirtilmiştir, birincil siteden üretim işlemleri çalıştırırken HLI birim QA örneği için kullanılır. DR Yük Devretmesini (veya yük devretme testi) zaman QA örnek kurtarma sitesinde alınır. 



### <a name="architecture-diagram"></a>Mimari diyagramı  

![HSR-with-DR.png](media/hana-supported-scenario/HSR-with-DR.png)

### <a name="ethernet"></a>Ethernet
Önceden yapılandırılmış ağ arabirimleri:

| NIC MANTIKSAL BİRİMLERİ | SKU TÜRÜ | SUSE işletim sistemi adı | RHEL işletim sistemi adı | Kullanım örneği|
| --- | --- | --- | --- | --- |
| A | BEN YAZIN | eth0.tenant | eno1.tenant | HLI istemcisi |
| B | BEN YAZIN | eth2.tenant | eno3.tenant | Kullanımda olmayan yapılandırılmış |
| C | BEN YAZIN | eth1.tenant | eno2.tenant | Depolama düğümü |
| D | BEN YAZIN | eth4.tenant | eno4.tenant | STONITH için kullanılan |
| A | TYPE II | VLAN\<tenantNo > | team0.tenant | HLI istemcisi |
| B | TYPE II | vlan\<tenantNo+2> | team0.tenant + 2 | Kullanımda olmayan yapılandırılmış |
| C | TYPE II | VLAN\<tenantNo + 1 > | team0.tenant + 1 | Depolama düğümü |
| D | TYPE II | VLAN\<tenantNo + 3 > | team0.tenant + 3 | STONITH için kullanılan |

### <a name="storage"></a>Depolama
Aşağıdaki bağlama yapılandırılmış:

| Takma noktası | Kullanım örneği | 
| --- | --- |
|**Birincil sitedeki birincil düğüm üzerinde**|
|/hana/Shared/SID | Üretim SID için HANA yükleyin | 
|/hana/Data/SID/mnt00001 | Üretim için SID veri dosyalarını yükleme | 
|/hana/log/SID/mnt00001 | SID üretim için günlük dosyalarını yükleme | 
|/hana/logbackups/SID | Üretim SID günlüklerini Yinele |
|**Birincil sitedeki ikincil düğümünde**|
|/hana/Shared/SID | HANA yüklemek için ikincil SID | 
|/hana/Data/SID/mnt00001 | Veri dosyaları yüklemek için ikincil SID | 
|/hana/log/SID/mnt00001 | Günlük dosyaları için ikincil SID yükleyin | 
|/hana/logbackups/SID | Günlükler için ikincil SID Yinele |
|**DR sitede**|
|/hana/Shared/SID | Üretim SID için HANA yükleyin | 
|/hana/Data/SID/mnt00001 | Üretim için SID veri dosyalarını yükleme | 
|/hana/log/SID/mnt00001 | SID üretim için günlük dosyalarını yükleme | 
|/hana/Shared/QA-SID | QA SID HANA yükleme | 
|/hana/Data/QA-SID/mnt00001 | QA SID veri dosyalarını yükleme | 
|/hana/log/QA-SID/mnt00001 | Günlük dosyaları için QA SID yükleyin |
|/hana/logbackups/QA-SID | QA SID günlükleri Yinele |

### <a name="key-considerations"></a>Dikkat edilmesi gereken temel konular
- /usr/SAP/SID /hana/shared/SID sembolik bir bağlantıdır.
- MCOS için: Birim boyutu dağıtım kapalı bellek veritabanı boyutunu temel alır. Başvuru [genel bakışı ve mimarisi](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-overview-architecture) bellekte hangi veritabanı boyutları öğrenmek için bölüm multisid ortamı ile desteklenir.
- STONITH: Bir SBD STONITH kurulumu için yapılandırılır. Ancak, STONITH kullanımı isteğe bağlıdır.
- DR sırasında: **İki depolama birimleri gerekli** birincil ve ikincil düğüm çoğaltması için.
- DR sırasında: Bağlama ve birimler ("HANA yüklemesi için gerekli olarak" işaretlenmiştir) yapılandırılmış olan üretim DR HLI biriminde HANA örneği yükleme için. 
- DR sırasında: Anlık görüntüden üretim sitesini aracılığıyla veri logbackups ve paylaşılan birimler ("Depolama çoğaltma" işaretlenmiştir) çoğaltılır. Bu birimleri, yalnızca yük devretme süre boyunca bağlanır. Daha fazla bilgi için belgeyi okumak [olağanüstü durum kurtarma yük devretme yordamı](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-overview-high-availability-disaster-recovery) daha fazla ayrıntı için. 
- DR sırasında: Veriler, logbackups, günlük, QA ("QA örneği yükleme" işaretlenmiştir) için paylaşılan birimler, QA örneği yükleme için yapılandırılır.
- Önyükleme birimi için **SKU miyim sınıf türü** DR düğüme çoğaltılır.


## <a name="7-host-auto-failover-11"></a>7. Ana bilgisayar otomatik yük devretme (1 + 1)
 
Bu topoloji, bir ana bilgisayar otomatik yük devretme yapılandırmasında iki düğüm destekler. Ana/çalışan rolü ve başka bir düğüm bir yedek olarak yoktur. **SAP s/4 HANA için yalnızca bu senaryoyu destekler.** OSS Not başvurun "[2408419 - SAP S/4HANA - çok düğümlü Destek](https://launchpad.support.sap.com/#/notes/2408419)" daha fazla ayrıntı için.



### <a name="architecture-diagram"></a>Mimari diyagramı  

![ska](media/hana-supported-scenario/scaleup-with-standby.png)

### <a name="ethernet"></a>Ethernet
Önceden yapılandırılmış ağ arabirimleri:

| NIC MANTIKSAL BİRİMLERİ | SKU TÜRÜ | SUSE işletim sistemi adı | RHEL işletim sistemi adı | Kullanım örneği|
| --- | --- | --- | --- | --- |
| A | BEN YAZIN | eth0.tenant | eno1.tenant | HLI istemcisi |
| B | BEN YAZIN | eth2.tenant | eno3.tenant | Düğüm düğüm iletişim |
| C | BEN YAZIN | eth1.tenant | eno2.tenant | Depolama düğümü |
| D | BEN YAZIN | eth4.tenant | eno4.tenant | Kullanımda olmayan yapılandırılmış |
| A | TYPE II | VLAN\<tenantNo > | team0.tenant | HLI istemcisi |
| B | TYPE II | vlan\<tenantNo+2> | team0.tenant + 2 | Düğüm düğüm iletişim |
| C | TYPE II | VLAN\<tenantNo + 1 > | team0.tenant + 1 | Depolama düğümü |
| D | TYPE II | VLAN\<tenantNo + 3 > | team0.tenant + 3 | Kullanımda olmayan yapılandırılmış |

### <a name="storage"></a>Depolama
Aşağıdaki bağlama yapılandırılmış:

| Takma noktası | Kullanım örneği | 
| --- | --- |
|**Ana ve bekleme düğümler üzerinde**|
|/ hana/paylaşılan | Üretim SID için HANA yükleyin | 
|/hana/Data/SID/mnt00001 | Üretim için SID veri dosyalarını yükleme | 
|/hana/log/SID/mnt00001 | SID üretim için günlük dosyalarını yükleme | 
|/hana/logbackups/SID | Üretim SID günlüklerini Yinele |



### <a name="key-considerations"></a>Dikkat edilmesi gereken temel konular
- /usr/SAP/SID /hana/shared/SID sembolik bir bağlantıdır.
- Bekleme: Bağlama ve birimler ("HANA yüklemesi için gerekli olarak" işaretlenmiştir) yapılandırılmış bekleme birim HANA örneği yüklenebilir.
 

## <a name="8-scale-out-with-standby"></a>8. Beklemeyle ölçeklendirme
 
Bu topoloji, bir genişleme yapılandırmasında birden çok düğüm destekler. Yedek olarak yöneticisi rolü, çalışan rolü ile bir veya daha fazla düğümü ve bir veya daha fazla düğümü olan bir düğümü vardır. Ancak, süresinin belirli bir anda yalnızca bir ana düğümü olabilir.


### <a name="architecture-diagram"></a>Mimari diyagramı  

![genişletme nm standby.png](media/hana-supported-scenario/scaleout-nm-standby.png)

### <a name="ethernet"></a>Ethernet
Önceden yapılandırılmış ağ arabirimleri:

| NIC MANTIKSAL BİRİMLERİ | SKU TÜRÜ | SUSE işletim sistemi adı | RHEL işletim sistemi adı | Kullanım örneği|
| --- | --- | --- | --- | --- |
| A | BEN YAZIN | eth0.tenant | eno1.tenant | HLI istemcisi |
| B | BEN YAZIN | eth2.tenant | eno3.tenant | Düğüm düğüm iletişim |
| C | BEN YAZIN | eth1.tenant | eno2.tenant | Depolama düğümü |
| D | BEN YAZIN | eth4.tenant | eno4.tenant | Kullanımda olmayan yapılandırılmış |
| A | TYPE II | VLAN\<tenantNo > | team0.tenant | HLI istemcisi |
| B | TYPE II | vlan\<tenantNo+2> | team0.tenant + 2 | Düğüm düğüm iletişim |
| C | TYPE II | VLAN\<tenantNo + 1 > | team0.tenant + 1 | Depolama düğümü |
| D | TYPE II | VLAN\<tenantNo + 3 > | team0.tenant + 3 | Kullanımda olmayan yapılandırılmış |

### <a name="storage"></a>Depolama
Aşağıdaki bağlama yapılandırılmış:

| Takma noktası | Kullanım örneği | 
| --- | --- |
|**Ana, alt ve bekleme düğümler üzerinde**|
|/ hana/paylaşılan | Üretim SID için HANA yükleyin | 
|/hana/Data/SID/mnt00001 | Üretim için SID veri dosyalarını yükleme | 
|/hana/log/SID/mnt00001 | SID üretim için günlük dosyalarını yükleme | 
|/hana/logbackups/SID | Üretim SID günlüklerini Yinele |


## <a name="9-scale-out-without-standby"></a>9. Ölçek genişletme bekleme
 
Bu topoloji, bir genişleme yapılandırmasında birden çok düğüm destekler. Yöneticisi rolüne sahip bir düğümü ve bir veya çalışan rolü modu düğümleri yoktur. Ancak, süresinin belirli bir anda yalnızca bir ana düğümü olabilir.


### <a name="architecture-diagram"></a>Mimari diyagramı  

![nm.png ölçeğini genişletme](media/hana-supported-scenario/scaleout-nm.png)


### <a name="ethernet"></a>Ethernet
Önceden yapılandırılmış ağ arabirimleri:

| NIC MANTIKSAL BİRİMLERİ | SKU TÜRÜ | SUSE işletim sistemi adı | RHEL işletim sistemi adı | Kullanım örneği|
| --- | --- | --- | --- | --- |
| A | BEN YAZIN | eth0.tenant | eno1.tenant | HLI istemcisi |
| B | BEN YAZIN | eth2.tenant | eno3.tenant | Düğüm düğüm iletişim |
| C | BEN YAZIN | eth1.tenant | eno2.tenant | Depolama düğümü |
| D | BEN YAZIN | eth4.tenant | eno4.tenant | Kullanımda olmayan yapılandırılmış |
| A | TYPE II | VLAN\<tenantNo > | team0.tenant | HLI istemcisi |
| B | TYPE II | vlan\<tenantNo+2> | team0.tenant + 2 | Düğüm düğüm iletişim |
| C | TYPE II | VLAN\<tenantNo + 1 > | team0.tenant + 1 | Depolama düğümü |
| D | TYPE II | VLAN\<tenantNo + 3 > | team0.tenant + 3 | Kullanımda olmayan yapılandırılmış |

### <a name="storage"></a>Depolama
Aşağıdaki bağlama yapılandırılmış:

| Takma noktası | Kullanım örneği | 
| --- | --- |
|**Ana ve alt düğümler üzerinde**|
|/ hana/paylaşılan | Üretim SID için HANA yükleyin | 
|/hana/Data/SID/mnt00001 | Üretim için SID veri dosyalarını yükleme | 
|/hana/log/SID/mnt00001 | SID üretim için günlük dosyalarını yükleme | 
|/hana/logbackups/SID | Üretim SID günlüklerini Yinele |


### <a name="key-considerations"></a>Dikkat edilmesi gereken temel konular
- /usr/SAP/SID /hana/shared/SID sembolik bir bağlantıdır.

## <a name="10-scale-out-with-dr"></a>10. DR ile ölçeklendirme
 
Bu topoloji ölçek genişletme bir DR ile birden çok düğüm destekler. Hem normal hem de çok amaçlı DR desteklenir. , Yalnızca tek amaçlı DR diyagramda gösterilmiştir. Bu topoloji ile veya olmadan bekleme düğüm isteyebilir.


### <a name="architecture-diagram"></a>Mimari diyagramı  

![dr.png ile genişletme](media/hana-supported-scenario/scaleout-with-dr.png)


### <a name="ethernet"></a>Ethernet
Önceden yapılandırılmış ağ arabirimleri:

| NIC MANTIKSAL BİRİMLERİ | SKU TÜRÜ | SUSE işletim sistemi adı | RHEL işletim sistemi adı | Kullanım örneği|
| --- | --- | --- | --- | --- |
| A | BEN YAZIN | eth0.tenant | eno1.tenant | HLI istemcisi |
| B | BEN YAZIN | eth2.tenant | eno3.tenant | Düğüm düğüm iletişim |
| C | BEN YAZIN | eth1.tenant | eno2.tenant | Depolama düğümü |
| D | BEN YAZIN | eth4.tenant | eno4.tenant | Kullanımda olmayan yapılandırılmış |
| A | TYPE II | VLAN\<tenantNo > | team0.tenant | HLI istemcisi |
| B | TYPE II | vlan\<tenantNo+2> | team0.tenant + 2 | Düğüm düğüm iletişim |
| C | TYPE II | VLAN\<tenantNo + 1 > | team0.tenant + 1 | Depolama düğümü |
| D | TYPE II | VLAN\<tenantNo + 3 > | team0.tenant + 3 | Kullanımda olmayan yapılandırılmış |

### <a name="storage"></a>Depolama
Aşağıdaki bağlama yapılandırılmış:

| Takma noktası | Kullanım örneği | 
| --- | --- |
|**Birincil düğüm üzerinde**|
|/ hana/paylaşılan | Üretim SID için HANA yükleyin | 
|/hana/Data/SID/mnt00001 | Üretim için SID veri dosyalarını yükleme | 
|/hana/log/SID/mnt00001 | SID üretim için günlük dosyalarını yükleme | 
|/hana/logbackups/SID | Üretim SID günlüklerini Yinele |
|**DR düğümde**|
|/ hana/paylaşılan | Üretim SID için HANA yükleyin | 
|/hana/Data/SID/mnt00001 | Üretim için SID veri dosyalarını yükleme | 
|/hana/log/SID/mnt00001 | SID üretim için günlük dosyalarını yükleme | 


### <a name="key-considerations"></a>Dikkat edilmesi gereken temel konular
- /usr/SAP/SID /hana/shared/SID sembolik bir bağlantıdır.
-  DR sırasında: Bağlama ve birimler ("HANA yüklemesi için gerekli olarak" işaretlenmiştir) yapılandırılmış olan üretim DR HLI biriminde HANA örneği yükleme için. 
- DR sırasında: Anlık görüntüden üretim sitesini aracılığıyla veri logbackups ve paylaşılan birimler ("Depolama çoğaltma" işaretlenmiştir) çoğaltılır. Bu birimleri, yalnızca yük devretme süre boyunca bağlanır. Daha fazla bilgi için belgeyi okumak [olağanüstü durum kurtarma yük devretme yordamı](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-overview-high-availability-disaster-recovery) daha fazla ayrıntı için. 
- Önyükleme birimi için **SKU miyim sınıf türü** DR düğüme çoğaltılır.


## <a name="next-steps"></a>Sonraki adımlar
- Başvuru [altyapı ve bağlantı](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-overview-infrastructure-connectivity) HLI için
- Başvuru [yüksek kullanılabilirlik ve olağanüstü durum kurtarma](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-overview-high-availability-disaster-recovery) HLI için
