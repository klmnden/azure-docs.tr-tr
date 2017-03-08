---
title: "Azure’da ağ arabirimleri | Microsoft Docs"
description: "Azure ağ arabirimleri ve sanal makinelerdeki kullanımı hakkında bilgi edinin."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: f58b503f-18bf-4377-aa63-22fc8a96e4be
ms.service: virtual-network
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/24/2016
ms.author: jdial
ms.custom: H1Hack27Feb2017
translationtype: Human Translation
ms.sourcegitcommit: 63f2f6dde56c1b5c4b3ad2591700f43f6542874d
ms.openlocfilehash: 395cff80b3f97b6340e15f370c13f783e2f5dde3
ms.lasthandoff: 02/28/2017


---
# <a name="what-are-network-interfaces"></a>Ağ arabirimi nedir?

Ağ arabirimi (NIC), bir Sanal Makine (VM) ile onun yazılım altyapısı arasındaki çift yönlü bağlantıdır. Bu makalede bir ağ arabiriminin ne olduğu ve Azure Resource Manager dağıtım modelinde nasıl kullanıldığı açıklanmıştır.

Microsoft yeni kaynakların Resource Manager dağıtım modeli kullanılarak dağıtılmasını önerir, ancak ağ bağlantısı [klasik](virtual-network-ip-addresses-overview-classic.md) dağıtım modelinde olan VM’ler de dağıtabilirsiniz. Klasik model hakkında bilgi sahibiyseniz, Resource Manager dağıtım modelindeki VM ağ bağlantısında önemli farklar vardır. [Sanal makine ağ bağlantısı - Klasik](virtual-network-ip-addresses-overview-classic.md#differences-between-resource-manager-and-classic-deployments) makalesini okuyarak farklar hakkında daha fazla bilgi edinin.

Azure’daki bir ağ arabirimi:

1. Oluşturulabilen, silinebilen ve kendi yapılandırılabilir ayarlarına sahip olan bir kaynaktır.
2. Oluşturulduğunda Azure Sanal Ağı’ndaki (VNet) bir alt ağa bağlanmalıdır. VNet’ler konusunda bilgi sahibi değilseniz [Sanal ağa genel bakış](virtual-networks-overview.md) makalesini okuyarak bunlar hakkında daha fazla bilgi edinin. NIC’nin NIC ile aynı Azure [konumunda](https://azure.microsoft.com/regions) ve [aboneliğinde](../azure-glossary-cloud-terminology.md#subscription) bulunan bir VNet’e bağlanması gerekir. Bir NIC oluşturulduktan sonra NIC’nin bağlı olduğu alt ağ değiştirebilir, ancak bağlı olduğu VNet’i değiştiremezsiniz.
3. Kendisine bir ad atandığından, NIC oluşturulduktan sonra değiştirilemez. Adın bir Azure [kaynak grubunda](../azure-resource-manager/resource-group-overview.md#resource-groups) benzersiz olması gerekir, ancak abonelikte, oluşturulduğu Azure konumunda ya da bağlı olduğu VNet’te benzersiz olması gerekmez. Bir Azure aboneliğinde genellikle birkaç NIC oluşturulur. Birden çok NIC’nin yönetilmesini varsayılan adların kullanılmasına göre daha kolay hale getiren bir adlandırma kuralı bulmanız önerilir. Öneriler için bkz. [Azure kaynakları için önerilen adlandırma kuralları](../guidance/guidance-naming-conventions.md).
4. Bir VM’ye bağlı olabilir, ancak yalnızca NIC ile aynı konumdaki tek bir VM’ye bağlanabilir.
5. Bir MAC adresi vardır ve NIC bir VM’ye bağlı kaldığı sürece bu adres kalıcıdır. Azure Portal, Azure PowerShell veya Azure Komut Satırı Arabirimi kullanılarak VM yeniden başlatıldığında veya durdurulup (serbest bırakılıp) başlatıldığında MAC adresi değişmez. NIC bir VM’den ayrılıp farklı bir VM’ye bağlanırsa başka bir MAC adresi alır. NIC silinirse MAC adresi diğer NIC’lere atanır.
6. Bir **birincil** *IPv4* statik veya dinamik IP adresi atanmış olmalıdır.
7. İlişkili bir veya daha fazla genel IP adresi kaynağı bulunabilir, daha fazla bilgi için [NIC başına birden fazla IP adresi](virtual-network-multiple-ip-addresses-portal.md) belgesini okuyun.
8. Microsoft Windows Server işletim sisteminin belirli sürümlerini çalıştıran belirli VM boyutları için tek köklü G/Ç sanallaştırması (SR-IOV) ile hızlandırılmış ağı destekler. Bu ÖNİZLEME özelliği hakkında daha fazla bilgi edinmek için [Sanal makine için hızlandırılmış ağ](virtual-network-accelerated-networking-powershell.md) makalesini okuyun.
9. NIC için IP iletme etkinleştirilmişse kendisine atanan özel IP adreslerine yönelik olmayan trafiği alabilir. Örneğin, bir VM güvenlik duvarı yazılımı çalıştırıyorsa kendi IP adreslerine yönelik olmayan paketleri yönlendirir. VM’nin yine de trafiği yönlendirebilen veya iletebilen yazılımlar çalıştırması gerekir, ancak bunu yapabilmesi, bir NIC için IP iletiminin etkin olmasını gerektirir.
10. Genellikle bağlı olduğu VM veya VNet ile aynı kaynak grubunda oluşturulur, ancak bu gerekli değildir.

## <a name="vms-with-multiple-network-interfaces"></a>Birden çok ağ arabirimine sahip VM’ler
Bir VM’ye birden çok NIC bağlanabilir, ancak bunu yaparken aşağıdakilere dikkat edin:  

* VM boyutu birden çok NIC’yi desteklemelidir. Hangi VM boyutlarının birden çok NIC’yi desteklediği hakkında daha fazla bilgi edinmek için [Windows Server VM boyutları](../virtual-machines/virtual-machines-windows-sizes.md) veya [Linux VM boyutları](../virtual-machines/virtual-machines-linux-sizes.md) makalesini okuyun.
* VM en az NIC ile oluşturulmalıdır. VM boyutu birden çok NIC’yi desteklemesine rağmen VM yalnızca bir NIC ile oluşturulursa, VM oluşturulduktan sonra VM’ye ek NIC bağlayamazsınız. VM en az iki NIC ile oluşturulduğu ve VM boyutu ikiden fazla NIC’yi desteklediği sürece, VM oluşturulduktan sonra VM’ye ek NIC’ler bağlayabilirsiniz.  
* VM’ye bağlı en az üç NIC varsa ikincil NIC’leri VM’den ayırabilirsiniz (birincil NIC ayrılamaz). VM’ye bağlı iki veya daha az NIC varsa NIC’leri ayıramazsınız.  
* VM içindeki NIC’lerin sıralaması rastgele olur ve Azure altyapı güncelleştirmeleri arasında da farklılık gösterebilir. Ancak, IP adresleri ve onlara karşılık gelen ethernet MAC adresleri aynı kalır. Örneğin, işletim sisteminin Azure NIC1’i Eth1 olarak tanımladığını varsayalım. Eth1’in IP adresi 10.1.0.100, MAC adresi 00-0D-3A-B0-39-0D olsun. Bir Azure altyapı güncelleştirmesi ve yeniden başlatma işleminden sonra işletim sistemi Azure NIC1’i Eth2 olarak tanımlamaya başlayabilir, ancak IP ve MAC adresleri, Azure NIC1’in işletim sistemi tarafından Eth1 olarak tanımlandığında olduğu gibi kalır. Bir yeniden başlatma işlemi müşteri tarafından başlatıldıysa işletim sistemi içinde NIC sıralaması aynı kalır.  
* VM bir [kullanılabilirlik kümesinin](../azure-glossary-cloud-terminology.md#availability-set) üyesiyse, kullanılabilirlik kümesindeki VM’lerin tamamı tek NIC veya birden çok NIC içermelidir. VM’ler birden çok NIC içeriyorsa, her bir VM’nin ikiden fazla NIC’si olduğu sürece her birinin sahip olduğu NIC sayısı aynı olmak zorunda değildir.

## <a name="next-steps"></a>Sonraki adımlar
* [VM oluşturma](../virtual-machines/virtual-machines-windows-hero-tutorial.md) makalesini okuyarak tek NIC’li VM oluşturmayı öğrenin.
* [Birden çok NIC’li VM dağıtma](virtual-network-deploy-multinic-arm-ps.md) makalesini okuyarak birden çok NIC içeren bir VM oluşturmayı öğrenin.
* [Azure sanal makineleri için birden çok IP adresi](virtual-network-multiple-ip-addresses-powershell.md) makalesini okuyarak birden çok IP yapılandırması içeren bir NIC oluşturmayı öğrenin.


