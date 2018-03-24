---
title: IPv6 Azure yük dengeleyici için genel bakış | Microsoft Docs
description: IPv6 desteği Azure yük dengeleyici ve yük dengeli sanal makineleri anlama.
services: load-balancer
documentationcenter: na
author: KumudD
manager: jeconnoc
editor: ''
keywords: IPv6, azure yük dengeleyici, çift yığın, genel IP, yerel IPv6, mobil, IOT
ms.assetid: 6a1d583f-a305-40fd-a94b-fa42e1943bbb
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/25/2017
ms.author: kumud
ms.openlocfilehash: 9622ad4922aa98efe093e7f809a490a8797eb1fd
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/23/2018
---
# <a name="overview-of-ipv6-for-azure-load-balancer"></a>IPv6 Azure yük dengeleyici için genel bakış


>[!NOTE] 
>Azure Load Balancer iki farklı türü destekler: Temel ve Standart. Bu makalede Temel Yük Dengeleyici anlatılmaktadır. Standart yük dengeleyici hakkında daha fazla bilgi için bkz: [standart yük dengeleyici genel bakış](load-balancer-standard-overview.md).

Internet'e yönelik Yük Dengeleyici, bir IPv6 adresi ile dağıtılabilir. IPv4 bağlantısı ek olarak, bu aşağıdaki yetenekleri sağlar:

* Yerel uçtan uca IPv6 bağlantısı ortak Internet istemcileri ve Azure sanal makineleri (VM'ler) arasında yük dengeleyici üzerinden.
* Yerel uçtan uca IPv6 giden bağlantı VM'ler ve ortak Internet IPv6 özellikli istemciler arasında.

Aşağıdaki resimde, Azure yük dengeleyici için IPv6 işlevselliği gösterilmektedir.

![IPv6 Azure yük dengeleyici](./media/load-balancer-ipv6-overview/load-balancer-ipv6.png)

Dağıtıldığında, bir IPv4 veya IPv6 özellikli Internet istemcisi ortak IPv4 veya IPv6 adresleri (veya ana bilgisayar adları) Azure Internet'e yönelik Yük Dengeleyici iletişim kurabilir. Yük Dengeleyici ağ adresi çevirisi (NAT) kullanarak sanal makineleri özel IPv6 adreslerini IPv6 paketlerini yönlendirir. IPv6 Internet istemcisi doğrudan IPv6 adresi VM'ler ile iletişim kuramıyor.

## <a name="features"></a>Özellikler

Azure Resource Manager aracılığıyla dağıtılan VM'ler için yerel IPv6 desteği sağlar:

1. IPv6 Internet üzerindeki istemciler için yük dengeli IPv6 Hizmetleri
2. Yerel IPv6 ve IPv4 uç noktaları vm'lerde ("Yığılmış çift")
3. Gelen ve giden başlatılan yerel IPv6 bağlantılar
4. TCP, UDP ve HTTP (S) gibi desteklenen protokoller eksiksiz bir hizmet mimarileri etkinleştir

## <a name="benefits"></a>Avantajlar

Bu işlevsellik, aşağıdaki faydaları sağlar:

* Yeni uygulamalar yalnızca IPv6 istemciler için erişilebilir olmasını gerektiren kamu düzenlemeleri karşılayan
* Etkinleştirme mobil ve nesnelerin interneti (IOT) geliştiriciler çift yığın (IPv4 + IPv6) Azure sanal makineler büyüyen mobil & IOT pazarlara yönelik olarak kullanmak için

## <a name="details-and-limitations"></a>Ayrıntılar ve sınırlamalar

Ayrıntılar

* Azure DNS hizmeti, bir IPv4 ve IPv6 AAAA ad kayıtlarını içerir ve yük dengeleyici için her iki kayıt ile yanıt verir. İstemci ile iletişim kurmak için adres (IPv4 veya IPv6) seçer.
* Bir VM ortak Internet IPv6 bağlı aygıtına bir bağlantı başlattığında, VM'nin IPv6 adresi çevrilmiş ağ adresine (NAT) yük dengeleyicinin genel IPv6 adresi kaynağıdır.
* Linux işletim sistemi çalıştıran VM'ler, DHCP aracılığıyla bir IPv6 IP adresi almak için yapılandırılmalıdır. Çoğu Azure Galerisi'ndeki Linux görüntülerinin IPv6 değişiklik yapmadan desteklemek için zaten yapılandırılmış. Daha fazla bilgi için bkz: [yapılandırma DHCPv6 Linux VM'ler](load-balancer-ipv6-for-linux.md)
* Yük Dengeleyici ile bir sistem durumu araştırması kullanmayı seçerseniz, bir IPv4 araştırması oluşturabilir ve hem IPv4 hem de IPv6 uç ile kullanabilirsiniz. VM hizmette kullanılamaz hale gelirse IPv4 ve IPv6 uç noktalar dışında döndürme alınır.

Sınırlamalar

* Azure portalında IPv6 Yük Dengeleme kuralları eklenemiyor. Kuralları yalnızca şablonu aracılığıyla, CLI, PowerShell oluşturulabilir.
* IPv6 adresleri kullanmak için var olan VM'ler yükseltme değil. Yeni VM'ler dağıtmanız gerekir.
* Tek bir IPv6 adresi her VM tek bir ağ arabiriminde atanabilir.
* Genel IPv6 adresi için bir VM atanamaz. Bir yük dengeleyiciye yalnızca atanabilir.
* Ters DNS araması, genel IPv6 adresleri için yapılandıramazsınız.
* IPv6 adresleri ile sanal makineleri bir Azure bulut hizmeti üyesi olamaz. Bunlar, bir Azure sanal ağı (VNet) bağlanabilir ve IPv4 adresleri birbirleriyle iletişim.
* Özel IPv6 adresleri, bir kaynak grubunda tek tek sanal makineleri üzerinde dağıtılabilir ancak ölçek kümeleri aracılığıyla bir kaynak grubuna dağıtılamıyor.
* Azure VM'ler, diğer sanal makineleri, diğer Azure hizmetlerine veya şirket içi cihazlar için IPv6 üzerinden bağlanamıyor. Bunlar yalnızca Azure yük dengeleyici ile IPv6 üzerinden iletişim kurabilir. Ancak, IPv4 kullanarak bu diğer kaynaklarla iletişim kurabilir.
* Ağ güvenlik grubu (NSG) koruma IPv4 için ikili yığını (IPv4 + IPv6) dağıtımlarda desteklenir. Nsg'ler IPv6 uç noktaları için geçerli değildir.
* VM IPv6 uç noktada doğrudan Internet'e açık değil. Bir yük dengeleyicinin arkasına olur. Yalnızca Yük Dengeleyici kurallarında belirtilen bağlantı noktalarını IPv6 üzerinden erişilebilir.
* IPv6 için IdleTimeout parametre değiştirme **şu anda desteklenmiyor**. Varsayılan dört dakikadır.
* IPv6 için loadDistributionMethod parametre değiştirme **şu anda desteklenmiyor**.
* Ayrılan IPv6 IP'leri (burada Ipallocationmethod statik =) olan **şu anda desteklenmiyor**.

## <a name="next-steps"></a>Sonraki adımlar

IPv6 olan yük dengeleyici dağıtmayı öğrenin.

* [Bölgeye göre IPv6 kullanılabilirliği](https://go.microsoft.com/fwlink/?linkid=828357)
* [Bir şablonu kullanarak bir yük dengeleyici IPv6 ile dağıtma](load-balancer-ipv6-internet-template.md)
* [Azure PowerShell kullanarak bir yük dengeleyici IPv6 ile dağıtma](load-balancer-ipv6-internet-ps.md)
* [Azure CLI kullanarak bir yük dengeleyici IPv6 ile dağıtma](load-balancer-ipv6-internet-cli.md)
