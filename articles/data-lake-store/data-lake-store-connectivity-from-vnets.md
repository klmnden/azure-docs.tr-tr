---
title: Ait sanal ağlar için Azure Data Lake depolama Gen1 bağlayın | Microsoft Docs
description: Azure Data Lake depolama Gen1 için Azure sanal ağları birbirine bağlama
services: data-lake-store,data-catalog
documentationcenter: ''
author: esung22
manager: mtillman
editor: cgronlun
ms.assetid: 683fcfdc-cf93-46c3-b2d2-5cb79f5e9ea5
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.date: 01/31/2018
ms.author: elsung
ms.openlocfilehash: c8d028a981d7811ed2c864db5750afc83ab93b2b
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60878877"
---
# <a name="access-azure-data-lake-storage-gen1-from-vms-within-an-azure-vnet"></a>Bir Azure sanal ağ içindeki vm'lerden erişim Azure Data Lake depolama Gen1
Azure Data Lake depolama Gen1 genel Internet IP adreslerinde çalıştırılan bir PaaS hizmetidir. Genel Internet'e bağlanabilen herhangi bir sunucu genellikle de Azure Data Lake depolama Gen1 Uç noktalara bağlanabilirsiniz. Varsayılan olarak, Azure sanal ağ tüm VM'lerin İnternet'e erişebilir ve bu nedenle Azure Data Lake depolama Gen1 erişebilir. Ancak, internet erişimi olmaması için bir vnet'teki VM'ler yapılandırmak mümkündür. Bu tür VM'ler için Azure Data Lake depolama Gen1 erişimi de sınırlıdır. Azure sanal ağlardaki sanal makineler için genel Internet erişimini engelleyen yapılabilir aşağıdaki yaklaşımlardan birini kullanarak:

* Ağ güvenlik grupları (NSG) yapılandırarak
* Kullanıcı tanımlı yollar (UDR) yapılandırarak
* Yollar BGP (endüstri standardı dinamik yönlendirme protokolü) üzerinden değiştirerek ExpressRoute kullanıldığında, bu erişimi engelleme Internet'e

Bu makalede, Azure Data Lake depolama Gen1 için daha önce listelenen üç yöntemden birini kullanarak kaynaklara erişim için sınırlandı Azure sanal erişmesini öğreneceksiniz.

## <a name="enabling-connectivity-to-azure-data-lake-storage-gen1-from-vms-with-restricted-connectivity"></a>Bağlantı için Azure Data Lake depolama Gen1 sanal makinelerinden kısıtlı bağlantısı ile etkinleştirme
Azure Data Lake depolama Gen1 gibi Vm'lerden erişmek için Azure Data Lake depolama Gen1 hesabı kullanılabilir olduğu bölge için IP adresi erişmek için bunları yapılandırmanız gerekir. Hesaplarınızı DNS adlarını çözerek, Data Lake depolama Gen1 hesabı bölgeler için IP adreslerini tanımlayabilirsiniz (`<account>.azuredatalakestore.net`). Hesaplarınızı DNS adlarını çözümlemek için Araçlar gibi kullanabilirsiniz **nslookup**. Bilgisayarınızda bir komut istemi açın ve aşağıdaki komutu çalıştırın:

    nslookup mydatastore.azuredatalakestore.net

Çıktı aşağıdakine benzer. Değeri ile karşılaştırarak **adresi** özelliği, Data Lake depolama Gen1 hesabınızla ilişkili IP adresidir.

    Non-authoritative answer:
    Name:    1434ceb1-3a4b-4bc0-9c69-a0823fd69bba-mydatastore.projectcabostore.net
    Address:  104.44.88.112
    Aliases:  mydatastore.azuredatalakestore.net


### <a name="enabling-connectivity-from-vms-restricted-by-using-nsg"></a>NSG kullanarak kısıtlı vm'lerden bağlantıyı etkinleştirme
Bir NSG kuralı, Internet erişimi engellemek için kullanıldığında, Data Lake depolama Gen1 IP adresine erişime izin veren başka bir NSG oluşturabilirsiniz. NSG kuralları hakkında daha fazla bilgi için bkz: [ağ güvenlik gruplarına genel bakış](../virtual-network/security-overview.md). Nsg'leri oluşturma hakkında yönergeler için bkz: [bir ağ güvenlik grubu oluşturma](../virtual-network/tutorial-filter-network-traffic.md).

### <a name="enabling-connectivity-from-vms-restricted-by-using-udr-or-expressroute"></a>UDR ya da ExpressRoute kullanarak kısıtlı vm'lerden bağlantıyı etkinleştirme
Yollar, Udr veya yollar BGP değişimi, Internet erişimi engellemek için kullanıldığında, bir özel yol VM'ler gibi alt ağlarda Data Lake depolama Gen1 uç noktaları erişebilmesi için yapılandırılması gerekir. Daha fazla bilgi için [kullanıcı tanımlı yollara genel bakış](../virtual-network/virtual-networks-udr-overview.md). Udr'ler oluşturma ile ilgili yönergeler için bkz: [Udr oluşturun Kaynak Yöneticisi'nde](../virtual-network/tutorial-create-route-table-powershell.md).

### <a name="enabling-connectivity-from-vms-restricted-by-using-expressroute"></a>ExpressRoute kullanarak kısıtlı vm'lerden bağlantıyı etkinleştirme
Bir ExpressRoute bağlantı hattı yapılandırıldığında, şirket içi sunucular genel eşdüzey hizmet sağlama ile Data Lake depolama Gen1 erişebilirsiniz. Kullanılabilir genel eşdüzey hizmet sağlama için Expressroute'u yapılandırma hakkında daha fazla ayrıntı [ExpressRoute SSS](../expressroute/expressroute-faqs.md).

## <a name="see-also"></a>Ayrıca bkz.
* [Azure Data Lake depolama Gen1 genel bakış](data-lake-store-overview.md)
* [Azure Data Lake depolama Gen1 depolanan verilerin güvenliğini sağlama](data-lake-store-security-overview.md)

