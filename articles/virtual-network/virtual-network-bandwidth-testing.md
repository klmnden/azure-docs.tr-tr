---
title: "Test Azure VM ağ verimliliği | Microsoft Docs"
description: "Azure sanal makine ağ verimliliği test öğrenin."
services: virtual-network
documentationcenter: na
author: steveesp
manager: Gerald DeGrace
editor: 
ms.assetid: 
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/21/2017
ms.author: steveesp
ms.openlocfilehash: ccebc722386a19014674d7a59757a3685bd50793
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="bandwidththroughput-testing-ntttcp"></a>Bant genişliği/işleme (NTTTCP) test etme

Azure'da ağ verimliliği performansından test edilirken performansını etkileyebilir diğer kaynaklarının kullanımını en aza indirir ve test etmek için ağ hedefleyen bir aracı kullanmak en iyisidir. NTTTCP önerilir.

Aracı aynı boyutta iki Azure VM kopyalayın. GÖNDERENİ ve ALICISI olarak başka bir VM görür.

#### <a name="deploying-vms-for-testing"></a>Test etmek için sanal makineleri dağıtma
Böylece biz kendi iç IP'leri kullanın ve yük dengeleyici testten hariç bu test amaçları doğrultusunda, iki VM aynı bulut hizmeti ya da aynı kullanılabilirlik kümesi içinde olmalıdır. VIP ile test mümkündür ancak bu tür sınama bu belgenin kapsamı dışındadır.
 
ALICININ IP adresini not edin. Şimdi bu IP "a.b.c.r" çağırın

VM çekirdek sayısını not edin. Şimdi bu çağırın "\#num\_çekirdek"
 
NTTTCP test VM gönderici ve alıcı VM 300 saniye (veya 5 dakika) çalıştırın.

İpucu: Bu test için ilk kez ayarlarken daha erken geri bildirim almak için daha kısa bir test süresi deneyebilirsiniz. Aracı beklendiği gibi çalıştığını sonra 300 saniye en doğru sonuçlar için test süresi genişletir.

> [!NOTE]
> Gönderenin **ve** alıcı belirtmelisiniz **aynı** süresi parametresi test (-t).

Tek bir TCP akışı için 10 saniye test etmek için:

Alıcı parametreleri: ntttcp - r -t 10 - P 1

Gönderen parametreleri: ntttcp-s10.27.33.7 -t 10 - n 1 -P 1

> [!NOTE]
> Önceki örnekte, yalnızca yapılandırmanızı onaylamak için kullanılmalıdır. Sınama geçerli örnekleri bu belgenin sonraki bölümlerinde ele alınmaktadır.

## <a name="testing-vms-running-windows"></a>WINDOWS çalıştıran VM'ler sınama:

#### <a name="get-ntttcp-onto-the-vms"></a>Sanal makineleri üzerine NTTTCP alın.

En son sürümü yükleyin: <https://gallery.technet.microsoft.com/NTttcp-Version-528-Now-f8b12769>

Ya da onu taşınsa arayın: <https://www.bing.com/search?q=ntttcp+download> \< --ilk isabet

C: gibi ayrı bir klasör içinde NTTTCP yerleştirmeyi düşünün\\araçları

#### <a name="allow-ntttcp-through-the-windows-firewall"></a>NTTTCP Windows Güvenlik Duvarı aracılığıyla izin ver
Bir alıcı ile Windows Güvenlik Duvarı'nda gelmesi NTTTCP trafiğine izin vermek için bir izin verme kuralı oluşturun. İzin vermek için belirli bir TCP bağlantı noktalarında gelen yerine tüm NTTTCP programın adına göre izin vermek en kolay yoludur.

Bu gibi Windows Güvenlik Duvarı aracılığıyla ntttcp izin ver:

Netsh advfirewall güvenlik duvarı kuralı program Ekle =\<yolu\>\\ntttcp.exe adı "ntttcp" protocol = tüm dir = Action = etkinleştir izin yes profile = = ANY

Örneğin, ntttcp.exe için kopyaladıysanız "c:\\Araçları" klasörü, bu komut olur: 

Netsh advfirewall güvenlik duvarı kuralı program Ekle = c:\\Araçları\\ntttcp.exe adı "ntttcp" protocol = tüm dir = eylemde = = etkinleştir izin yes profile = = ANY

#### <a name="running-ntttcp-tests"></a>NTTTCP testlerini çalıştırma

Alıcı NTTTCP Başlat (**CMD'den çalıştırmak**PowerShell dosyalardan değil):

ntttcp - r – m [2\*\#num\_çekirdek],\*, a.b.c.r -t 300

VM dört çekirdek ve 10.0.0.4 IP adresi varsa, onu şöyle olabilir:

ntttcp - r – m 8,\*, 10.0.0.4 -t 300


Gönderenin NTTTCP Başlat (**CMD'den çalıştırmak**PowerShell dosyalardan değil):

ntttcp -s-m 8,\*, 10.0.0.4 -t 300 

Sonuçlar için bekleyin.


## <a name="testing-vms-running-linux"></a>LINUX çalıştıran Vm'leri sınama:

Linux için nttcp kullanın. Kullanılabilir <https://github.com/Microsoft/ntttcp-for-linux>

Linux sanal makinelerin (GÖNDERENİ ve ALICISI), ntttcp için linux Vm'lerinizi üzerinde hazırlamak için şu komutları çalıştırın:

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
Olun ve hem de yükleyin:
``` bash
 git clone <https://github.com/Microsoft/ntttcp-for-linux>
 cd ntttcp-for-linux/src
 make && make install
```

Windows örnekteki biz Linux ALICININ IP 10.0.0.4 olduğunu varsayalım.

Linux için NTTTCP alıcı başlatın:

``` bash
ntttcp -r -t 300
```

Ve gönderenin üzerinde çalıştırın:

``` bash
ntttcp -s10.0.0.4 -t 300
```
 
Test uzunluktaki varsayılanlar hiçbir zaman parametresi, 60 saniye olarak verilir

## <a name="testing-between-vms-running-windows-and-linux"></a>Windows ve LINUX çalıştıran VM'ler arasında sınama:

Test çalıştırabilmeniz için bu senaryoları biz eşitlemesi modu etkinleştirmeniz gerekir. Bu kullanılarak yapılır **-N bayrağı** Linux için ve **-ns bayrağı** Windows için.

#### <a name="from-linux-to-windows"></a>Windows için Linux:

Alıcı <Windows>:

``` bash
ntttcp -r -m <2 x nr cores>,*,<Windows server IP>
```

Gönderen <Linux> :

``` bash
ntttcp -s -m <2 x nr cores>,*,<Windows server IP> -N -t 300
```

#### <a name="from-windows-to-linux"></a>Windows Linux için:

Alıcı <Linux>:

``` bash
ntttcp -r -m <2 x nr cores>,*,<Linux server IP>
```

Gönderen <Windows>:

``` bash
ntttcp -s -m <2 x nr cores>,*,<Linux  server IP> -ns -t 300
```

## <a name="next-steps"></a>Sonraki adımlar
* Sonuçlar bağlı olarak, olabilir odasına [ağ verimliliği makineler en iyi duruma getirme](virtual-network-optimize-network-bandwidth.md) senaryonuz için.
* Daha fazla bilgi edinin [Azure Virtual Network sık sorulan sorular (SSS)](virtual-networks-faq.md)
