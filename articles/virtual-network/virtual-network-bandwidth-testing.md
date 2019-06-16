---
title: Azure sanal makine ağ aktarım hızı testi
titlesuffix: Azure Virtual Network
description: Azure sanal makine ağ aktarım hızı testi hakkında bilgi edinin.
services: virtual-network
documentationcenter: na
author: steveesp
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/21/2017
ms.author: steveesp
ms.openlocfilehash: 80e8a5e5de1da2098d895e09b36fb209050743a0
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60743096"
---
# <a name="bandwidththroughput-testing-ntttcp"></a>Bant genişliği/aktarım hızı (NTTTCP) test etme

Azure'da ağ verimliliği performansından test ederken, test etmek için ağ hedefleyen ve performansını etkileyebilecek diğer kaynaklarının kullanımını en aza indiren bir araç kullanmanız en iyisidir. NTTTCP önerilir.

Aracı aynı boyutta iki Azure sanal makinelerine kopyalayın. Bir VM GÖNDERENİ ve ALICISI olarak görür.

#### <a name="deploying-vms-for-testing"></a>Test etmek için Vm'leri dağıtma
Bu testin amaçları doğrultusunda, böylece biz de iç Ip'lerini kullanın ve test çalıştırmasından yük Dengeleyiciler hariç iki VM aynı bulut hizmeti veya aynı kullanılabilirlik kümesi içinde olmalıdır. VIP ile test edilebilir, ancak bu tür testler bu belgenin kapsamı dışındadır.

ALICININ IP adresini not edin. Bu IP "a.b.c.r" adlandıralım

VM çekirdek sayısını not edin. Bu adlandıralım "\#num\_çekirdek"

NTTTCP test VM gönderen ve alıcı VM 300 saniye (veya 5 dakika) çalıştırın.

İpucu: Bu test ilk kez ayarlarken daha erken geri bildirim almak için daha kısa bir test süre çalışabilir. Aracı beklendiği gibi çalışmaya başladığında, 300 saniye en doğru sonuçlar için test süresi içinde genişletin.

> [!NOTE]
> Gönderen **ve** alıcıya belirtmelidir **aynı** süre parametresini (-t).

10 saniye boyunca tek bir TCP akışı test etmek için:

Alıcı parametreleri: ntttcp - r -t 10 - P 1

Gönderen parametreleri: ntttcp-s10.27.33.7 -t 10 - n 1 -P 1

> [!NOTE]
> Önceki örnekte, yalnızca yapılandırmanızı onaylamak için kullanılmalıdır. Testin geçerli örnekler, bu belgenin sonraki bölümlerinde ele alınmaktadır.

## <a name="testing-vms-running-windows"></a>WINDOWS çalıştıran sanal makineleri test:

#### <a name="get-ntttcp-onto-the-vms"></a>Vm'leri üzerine NTTTCP alın.

En son sürümü yükleyin: <https://gallery.technet.microsoft.com/NTttcp-Version-528-Now-f8b12769>

Veya bu taşıdıysanız arayın: <https://www.bing.com/search?q=ntttcp+download> \< --ilk tıklama

C: gibi ayrı bir klasörde NTTTCP yerleştirmeyi göz önünde bulundurun\\araçları

#### <a name="allow-ntttcp-through-the-windows-firewall"></a>Windows Güvenlik Duvarı üzerinden NTTTCP izin ver
Bir alıcı ile gelmesi NTTTCP trafiğe izin vermek için Windows Güvenlik Duvarı izin verme kuralı oluşturun. İzin vermek için belirli bir TCP bağlantı noktalarına gelen yerine tüm NTTTCP program adına göre izin vermek en kolay yöntemdir.

Bu gibi Windows Güvenlik Duvarı üzerinden ntttcp izin ver:

Netsh advfirewall güvenlik duvarı kuralı program Ekle =\<yolu\>\\ntttcp.exe adı "ntttcp" protocol = herhangi bir dizini = Action = etkinleştir izin yes profile = = ANY

Örneğin için ntttcp.exe kopyaladıysanız, "c:\\Araçları" klasörü, bu komut şöyle olabilir: 

Netsh advfirewall güvenlik duvarı kuralı program Ekle = c:\\Araçları\\ntttcp.exe adı "ntttcp" protocol = herhangi bir dizini = Action = etkinleştir izin yes profile = = ANY

#### <a name="running-ntttcp-tests"></a>NTTTCP testlerini çalıştırma

Alıcı NTTTCP Başlat (**CMD'den çalıştırmak**, powershell'den değil):

ntttcp -r –m [2\*\#num\_cores],\*,a.b.c.r -t 300

VM dört çekirdek ve 10.0.0.4 IP adresi varsa, şöyle görünebilir:

ntttcp -r –m 8,\*,10.0.0.4 -t 300


Gönderenin NTTTCP Başlat (**CMD'den çalıştırma**, powershell'den değil):

ntttcp -s –m 8,\*,10.0.0.4 -t 300 

Sonuçlar için bekleyin.


## <a name="testing-vms-running-linux"></a>LINUX çalıştıran Vm'leri test:

Linux için nttcp kullanın. Kullanılabilir <https://github.com/Microsoft/ntttcp-for-linux>

Linux sanal makinelerine (gönderen ve alıcı) ntttcp için linux Vm'leriniz üzerinde hazırlamak için şu komutları çalıştırın:

CentOS - yükleme Git:
``` bash
  yum install gcc -y  
  yum install git -y
```
Ubuntu - yükleme Git:
``` bash
 apt-get -y install build-essential  
 apt-get -y install git
```
Yapın ve hem de yükleyin:
``` bash
 git clone https://github.com/Microsoft/ntttcp-for-linux
 cd ntttcp-for-linux/src
 make && make install
```

Windows örnekte olduğu gibi biz Linux ALICININ IP 10.0.0.4 olduğu varsayılır.

Linux için NTTTCP ALICIDA başlatın:

``` bash
ntttcp -r -t 300
```

Ve gönderen üzerinde çalıştırın:

``` bash
ntttcp -s10.0.0.4 -t 300
```
 
Varsayılan olarak 60 saniyede hiçbir zaman parametresi, test uzunluğu verildiğinde

## <a name="testing-between-vms-running-windows-and-linux"></a>Windows ve LINUX çalıştıran Vm'leri arasında test:

Test çalıştırabilmeniz için bu senaryolara biz eşitlemesi modunu etkinleştirmeniz gerekir. Bu kullanılarak yapılır **-N bayrağı** Linux için ve **-ns bayrağı** Windows için.

#### <a name="from-linux-to-windows"></a>Linux'taki Windows için:

Alıcı \<Windows >:

``` bash
ntttcp -r -m <2 x nr cores>,*,<Windows server IP>
```

Gönderen \<Linux >:

``` bash
ntttcp -s -m <2 x nr cores>,*,<Windows server IP> -N -t 300
```

#### <a name="from-windows-to-linux"></a>Linux için Windows:

Alıcı \<Linux >:

``` bash
ntttcp -r -m <2 x nr cores>,*,<Linux server IP>
```

Gönderen \<Windows >:

``` bash
ntttcp -s -m <2 x nr cores>,*,<Linux  server IP> -ns -t 300
```
## <a name="testing-cloud-service-instances"></a>Bulut hizmeti örnekleri test:
Bölümde, ServiceDefinition.csdef eklemeniz gerekir
```xml
<Endpoints>
  <InternalEndpoint name="Endpoint3" protocol="any" />
</Endpoints> 
```

## <a name="next-steps"></a>Sonraki adımlar
* Sonuçlarına bağlı olarak olabilir odasına [ağ aktarım hızı makineleri en iyi duruma getirme](virtual-network-optimize-network-bandwidth.md) senaryonuz için.
* Nasıl çalıştıracağınızı okuyun [bant genişliği, sanal makinelere ayrılır](virtual-machine-network-throughput.md)
* Daha fazla bilgi edinin [Azure sanal ağı sık sorulan sorular (SSS)](virtual-networks-faq.md)
