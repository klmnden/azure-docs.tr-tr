---
title: Azure genel IP adresi ön eki | Microsoft Docs
description: Kaynaklarınıza öngörülebilir genel IP adresleri atayabilirim, hangi bir Azure genel IP adresi ön eki ve nasıl yardımcı olabileceğini öğrenin.
services: virtual-network
documentationcenter: na
author: anavinahar
manager: narayan
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-network
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/24/2018
ms.author: anavin
ms.openlocfilehash: 23cd77d4a2d0c8203670039dd44c878bf7217fd3
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2019
ms.locfileid: "65799116"
---
# <a name="public-ip-address-prefix"></a>Genel IP adresi ön eki

Ayrılmış IP adresleri azure'da genel uç noktalarınız için genel bir IP adresi ön eki var. Azure aboneliğinize kaç belirttiğiniz üzerinde temel adres bitişik aralığını ayırır. Ortak adres bilmiyorsanız bkz [genel IP adresleri.](virtual-network-ip-addresses-overview-arm.md#public-ip-addresses)

Genel IP adresleri, her bir Azure bölgesinde adresi havuzundan atanır. Yapabilecekleriniz [indirme](https://www.microsoft.com/download/details.aspx?id=56519) Azure kullanan her bir bölge için aralıklarının listesi. Örneğin, 40.121.0.0/16 Azure kullanan Doğu ABD bölgesinde 100'den fazla aralıkları biridir. 40.121.0.1 - kullanılabilir adresleri aralığı içeren 40.121.255.254.

Genel bir IP adresi ön eki bir Azure bölgesi ve abonelikte bir ad belirterek oluşturun ve ön ek dahil etmek istediğiniz kaç adresi. 28 genel bir IP adresi ön eki oluşturursanız, örneğin, Azure 16 adresleri, aralıkları birinden sizin için ayırır. Aralığın oluşturana kadar Azure atar aralığı bilmiyorsanız, ancak bitişik adresleridir. Genel IP adresi ön eklerini bir ücreti vardır. Ayrıntılar için bkz [genel IP adresi fiyatlandırması](https://azure.microsoft.com/pricing/details/ip-addresses).

## <a name="why-create-a-public-ip-address-prefix"></a>Neden bir genel IP adresi ön eki oluşturulsun mu?

Genel IP adresi kaynağı oluşturduğunuzda, Azure atayın kullanılabilir bir genel IP adresi herhangi bir bölgede kullanılan aralık. Azure adresini atar sonra adresi ne olduğunu bilmeniz, ancak Azure adresini atar kadar hangi adresi atanabilir bilmiyorum. Bu, örneğin, siz veya iş ortaklarınızla, belirli IP adreslerine izin veren güvenlik duvarı kuralları ayarla sorunlara neden olabilir. Her zaman bir kaynak için yeni bir ortak IP adresi atamak için güvenlik duvarı kuralı eklenecek adresine sahiptir. Kaynaklarınıza ait genel bir IP adres öneklerini adresleri atadığınızda, güvenlik duvarı kuralları adreslerden birini atadığınız her zaman çok çeşitli Kural eklenemiyor çünkü güncelleştirilmesi gerekmez.

## <a name="benefits"></a>Avantajlar

- Bilinen bir arasındadır ortak IP adresi kaynakları oluşturabilirsiniz.
- Siz veya iş ortaklarınızla henüz atamadınız adresleri yanı sıra, şu anda atanmış genel IP adresleri içeren aralıkları ile güvenlik duvarı kuralları oluşturabilirsiniz. Bu, yeni kaynaklara IP adresleri atamak gibi güvenlik duvarı kurallarını değiştirmek için ihtiyacını ortadan kaldırır.
- Oluşturabileceğiniz bir aralığı varsayılan boyutu/28 ya da 16 IP adresleri.
- Oluşturabileceğiniz kaç aralıklar için sınır yoktur, ancak bir Azure aboneliğinde olabilir bir statik genel IP adresi sayısı üst sınırı vardır. Sonuç olarak, oluşturduğunuz aralık sayısı, aboneliğinizde olabilir. daha fazla statik genel IP adresleri kapsayamaz. Daha fazla bilgi için [Azure sınırları](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits).
- Önekini kullanarak oluşturduğunuz adresleri bir genel IP adresi atayabilirsiniz herhangi bir Azure kaynağa atanabilir.
- Ayrıca, hangi ayrılan ve henüz aralığında ayrılan IP adreslerini kolayca görebilirsiniz.

## <a name="scenarios"></a>Senaryolar
Aşağıdaki kaynaklar için bir statik genel IP adresi bir önekten ilişkilendirebilirsiniz:

|Resource|Senaryo|Adımlar|
|---|---|---|
|Virtual Machines| Bir Güvenlik Duvarı'nda IP'ler beyaz listeye ekleme söz konusu olduğunda ilişkilendirerek bir ön ek ortak Ip'lerden için sanal makinelerinizi azure'da yönetim yükünü azaltır. Yalnızca tek bir güvenlik duvarı kuralı ile tüm bir önek beyaz liste kullanabilirsiniz. Azure'da sanal makinelerle ölçeklendirirken, aynı ön maliyet, zaman ve yönetim yükünü kaydetme IP'ler ilişkilendirebilirsiniz.| IP'leri, sanal makinenize bir önekten ilişkilendirmek için: 1. [Bir önek oluşturun.](manage-public-ip-address-prefix.md) 2. [Bir IP önekten oluşturun.](manage-public-ip-address-prefix.md) 3. [IP ve sanal makinenizin ağ arabirimine ilişkilendirin.](virtual-network-network-interface-addresses.md#add-ip-addresses)
| Yük Dengeleyiciler | İlişkilendirerek bir ön ek ortak Ip'lerden için ön uç IP yapılandırması veya yük dengeleyici giden kuralı, Azure genel IP adresi alanı basitleştirme sağlar. Bir genel IP ön eke göre tanımlanan bitişik IP adresleri aralığı kaynaklandığı için giden bağlantılar temizlik tarafından senaryonuz basitleştirebilir. | IP'ler için Load balancer'ınız bir önekten ilişkilendirmek için: 1. [Bir önek oluşturun.](manage-public-ip-address-prefix.md) 2. [Bir IP önekten oluşturun.](manage-public-ip-address-prefix.md) 3. Yük Dengeleyici oluştururken seçin veya 2. adımda yük dengeleyici ön uç IP olarak oluşturulan IP güncelleştirin. |
| Azure Güvenlik Duvarı | Giden SNAT için bir önek bir ortak IP kullanabilirsiniz. Bu sanal ağ trafiğini giden çevrilir anlamına gelir [Azure Güvenlik Duvarı](../firewall/overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json) genel IP. Bu IP önceden belirlenmiş bir önekten geldiğinden, azure'da genel IP kaplama alanınızı nasıl görüneceğini önceden bilmesi çok daha kolaydır. | 1. [Bir önek oluşturun.](manage-public-ip-address-prefix.md) 2. [Bir IP önekten oluşturun.](manage-public-ip-address-prefix.md) 3. Olduğunda, [Azure Güvenlik Duvarı'nı dağıtma](../firewall/tutorial-firewall-deploy-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json#deploy-the-firewall), önceden ayrılmış IP önekten seçtiğinizden emin olun.|

## <a name="constraints"></a>Kısıtlamalar

- IP adreslerini ön eki belirtemezsiniz. Azure, belirttiğiniz boyutuna göre ön eki, IP adresi ayırır.
- En fazla 16 IP adresini veya/28 öneki oluşturabilirsiniz. Daha fazla bilgi için [Azure sınırları](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits).
- Önek oluşturduktan sonra aralığını değiştiremezsiniz.
- Yalnızca IPv4 adresleri aralığıdır. IPv6 adresleri aralığını içermiyor.
- Standart SKU ile oluşturulan yalnızca statik genel IP adresi ön eki'nın aralığından atanabilir. SKU'ları adresi genel IP hakkında daha fazla bilgi edinmek için [genel IP adresi](virtual-network-ip-addresses-overview-arm.md#public-ip-addresses).
- Adres aralığı yalnızca Azure Resource Manager kaynaklarına atanabilir. Adresleri, Klasik dağıtım modelinde kaynaklara atanamaz.
- Önekten oluşturulan tüm genel IP adresleri ön eki olarak abonelik ve aynı Azure bölgesinde bulunması gerekir ve aynı bölge ve abonelik kaynaklarına atanması gerekir.
- İçindeki herhangi bir adresi bir kaynağa ilişkili genel IP adresi kaynağı atanmış olan bir önek silemezsiniz. IP adresi ön ekini ilk atanan tüm genel IP adresi kaynaklarını ilişkisini kaldırın.


## <a name="next-steps"></a>Sonraki adımlar

- [Oluşturma](manage-public-ip-address-prefix.md) genel bir IP adresi ön eki
