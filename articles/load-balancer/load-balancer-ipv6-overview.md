---
title: Azure Load Balancer için IPv6 genel bakış
titlesuffix: Azure Load Balancer
description: Azure Load Balancer ve yük dengeli VM'ler için IPv6 desteğini anlama.
services: load-balancer
documentationcenter: na
author: KumudD
keywords: IPv6, azure yük dengeleyici, ikili yığın, genel IP, yerel IPv6, mobil veya IOT
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.custom: seodec18
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/24/2018
ms.author: kumud
ms.openlocfilehash: 894a56c2e51e8fa8a2d72253563d218416ace4cb
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60861958"
---
# <a name="overview-of-ipv6-for-azure-load-balancer"></a>Azure Load Balancer için IPv6 genel bakış


>[!NOTE] 
>Azure Load Balancer iki farklı türü destekler: Temel ve Standart. Bu makalede Temel Yük Dengeleyici anlatılmaktadır. Standard Load Balancer hakkında daha fazla bilgi için bkz: [standart Load Balancer'a genel bakış](load-balancer-standard-overview.md).

Bir IPv6 adresi ile Internet'e yönelik Yük Dengeleyiciler dağıtılabilir. IPv4 bağlantı ek olarak, bu aşağıdaki özellikleri sağlar:

* Yerel uçtan uca IPv6 bağlantısı arasında genel Internet istemcileri ve Azure sanal makineler (VM) yük dengeleyici üzerinden.
* Yerel uçtan uca IPv6 giden bağlantı VM'ler ve genel Internet IPv6 özellikli istemciler arasında.

Aşağıdaki resimde, Azure Load Balancer için IPv6 işlevlerini göstermektedir.

![Azure Load Balancer'da IPv6 ile](./media/load-balancer-ipv6-overview/load-balancer-ipv6.png)

Dağıtıldıktan sonra bir IPv4 veya IPv6 özellikli bir Internet istemcisi genel IPv4 veya IPv6 adresleri (veya ana bilgisayar adları ile) Azure Internet'e yönelik Yük Dengeleyici iletişim kurabilir. Yük Dengeleyici, ağ adresi çevirisi (NAT) kullanarak Vm'leri özel IPv6 adresleri için IPv6 paketlerini yönlendirir. IPv6 Internet İstemci doğrudan sanal makinelerin IPv6 adresiyle iletişim kuramıyor.

## <a name="features"></a>Özellikler

Azure Resource Manager aracılığıyla dağıtılan VM'ler için yerel IPv6 desteği sağlar:

1. İnternetteki istemcileri IPv6 yük dengeli IPv6 Hizmetleri
2. ("Yığılmış çift") vm'lerinde yerel IPv6 ve IPv4 uç noktaları
3. Gelen ve giden tarafından başlatılan yerel IPv6 bağlantıları
4. Desteklenen protokoller TCP, UDP ve HTTP (S) gibi çeşitli sayıda hizmet mimarisi etkinleştir

## <a name="benefits"></a>Avantajlar

Bu işlev, aşağıdaki faydaları sağlar:

* Yeni uygulamalar yalnızca IPv6 istemciler tarafından erişilebilir olmasını gerektiren kamu düzenlemelerini karşılamak
* Etkin mobil ve nesnelerin interneti (IOT) geliştiricileri, ikili yığın (IPv4 + IPv6) Azure sanal makineler, büyüyen mobil ve IOT pazarlara yönelik olarak kullanmak için

## <a name="details-and-limitations"></a>Ayrıntıları ve sınırlamalar

Ayrıntılar

* Azure DNS hizmeti hem bir IPv4 ve IPv6 AAAA ad kayıtları içerir ve yük dengeleyici için iki kaydı ile yanıt verir. İstemci ile iletişim kurmak için adresi (IPv4 veya IPv6) seçer.
* Bir VM'ye genel bir IPv6 İnternet'e bağlı cihazdaki bir bağlantı başlattığında, sanal makinenin IPv6 adresi çevrilmiş ağ adresine (NAT) yük dengeleyici genel IPv6 adresi kaynağıdır.
* Linux işletim sistemini çalıştıran VM'ler, DHCP aracılığıyla bir IPv6 IP adresi almak için yapılandırılmalıdır. Azure Galerisi'nde Linux görüntüleri çoğunu zaten IPv6 değiştirilmeden destekleyecek şeiklde yapılandırılan. Daha fazla bilgi için [Linux Vm'leri için DHCPv6 yapılandırma](load-balancer-ipv6-for-linux.md)
* Load balancer'ınız ile bir durum yoklaması kullanmayı seçerseniz, bir IPv4 araştırması oluşturun ve hem IPv4 hem de IPv6 uç noktaları ile kullanın. Sanal makinenizin hizmette kalırsa, IPv4 ve IPv6 uç noktaları döndürme dışına alınır.

Sınırlamalar

* Azure portalında IPv6 Yük Dengeleme kuralları eklenemiyor. Kuralları yalnızca şablon CLI, PowerShell oluşturulabilir.
* IPv6 adresleri kullanmak için mevcut Vm'leri yükseltme değil. Yeni sanal makineler dağıtmanız gerekir.
* Tek bir IPv6 adresi, her sanal makine bir tek bir ağ arabirimine atanabilir.
* Bir VM'ye genel IPv6 adresleri atanamaz. Bunlar, yalnızca bir yük dengeleyiciye atanabilir.
* Ters DNS arama, genel IPv6 adresleri için yapılandıramazsınız.
* IPv6 adresleri ile Vm'leri bir Azure bulut hizmeti üyeleri olamaz. Bunlar, bir Azure sanal ağına (VNet) bağlanabilir ve IPv4 adresleri birbiriyle iletişim.
* Özel IPv6 adresleri, tek VM'ler içinde bir kaynak grubu dağıtılabilir ancak ölçek kümeleri aracılığıyla bir kaynak grubuna dağıtılamıyor.
* Azure sanal makineleri, diğer Vm'leri, diğer Azure hizmetlerine veya şirket içi cihazlar için IPv6 üzerinden bağlanamıyor. Bunlar yalnızca ile Azure load balancer'da IPv6 üzerinden iletişim kurabilir. Ancak, IPv4 kullanarak bu diğer kaynaklarla iletişim kurabilir.
* IPv4 için ağ güvenlik grubu (NSG) koruma, ikili yığın (IPv4 + IPv6) dağıtımlarında desteklenir. Nsg'ler IPv6 uç noktalar için geçerli değildir.
* VM IPv6 uç noktada doğrudan internet'e gösterilmez. Bu yük dengeleyici arkasında olur. Yalnızca Yük Dengeleyici kurallarında belirtilen bağlantı noktaları, IPv6 üzerinden erişilebilir.
* IPv6 için IdleTimeout parametre değiştirme **şu anda desteklenmeyen**. Varsayılan dört dakikadır.
* IPv6 için loadDistributionMethod parametre değiştirme **şu anda desteklenmeyen**.
* Ayrılmış IP'ler IPv6 (burada Ipallocationmethod statik =) olan **şu anda desteklenmeyen**.
* NAT64 (IPv4, IPv6 çevirisi) desteklenmiyor.

## <a name="next-steps"></a>Sonraki adımlar

IPv6 ile bir yük dengeleyici dağıtmayı öğrenin.

* [IPv6 bölgelere göre kullanılabilirlik](https://go.microsoft.com/fwlink/?linkid=828357)
* [Bir şablon kullanarak bir yük dengeleyici IPv6 ile dağıtma](load-balancer-ipv6-internet-template.md)
* [Azure PowerShell kullanarak bir yük dengeleyici IPv6 ile dağıtma](load-balancer-ipv6-internet-ps.md)
* [Azure CLI kullanarak bir yük dengeleyici IPv6 ile dağıtma](load-balancer-ipv6-internet-cli.md)
