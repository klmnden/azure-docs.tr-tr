---
title: Azure NetApp dosyaları için yönergeler ağ planlaması | Microsoft Docs
description: Azure NetApp dosyalarını kullanarak bir etkin ağ mimarisi tasarlamanıza yardımcı olabilecek yönergeleri açıklar.
services: azure-netapp-files
documentationcenter: ''
author: b-juche
manager: ''
editor: ''
ms.assetid: ''
ms.service: azure-netapp-files
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/08/2019
ms.author: b-juche
ms.openlocfilehash: 5b54d8f21f4cb1cdd7bb06871df6ac22d19d1ab6
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2019
ms.locfileid: "67705211"
---
# <a name="guidelines-for-azure-netapp-files-network-planning"></a>Azure NetApp Files ağ planlaması yönergeleri

Ağ mimarisi planlama, herhangi bir uygulama altyapısı tasarlamanın önemli bir öğedir. Bu makalede Azure NetApp dosyaları zengin özelliklerden yararlanmak iş yükleriniz için bir etkin ağ mimarisi tasarlamanıza yardımcı olur.

Azure NetApp dosya birimleri olarak adlandırılan özel amaçlı alt ağda bulunması için tasarlanmış bir [alt temsilci](https://docs.microsoft.com/azure/virtual-network/virtual-network-manage-subnet) Azure sanal ağınız içinde. Bu nedenle, birimleri ağınızdan doğrudan, aynı bölgedeki eşlenmiş sanal ağlar veya şirket içi bir sanal ağ geçidi (ExpressRoute veya VPN ağ geçidi) gerektiği gibi üzerinden erişebilirsiniz. Alt ağ, Azure için NetApp dosyaları ayrılmıştır ve diğer Azure hizmetleriyle veya Internet bağlantısı yok.

## <a name="considerations"></a>Dikkat edilmesi gerekenler  

Azure NetApp dosyaları ağ için planlama yaparken bazı önemli noktalar anlamanız gerekir.

### <a name="constraints"></a>Kısıtlamalar

Aşağıdaki özellikler, Azure için NetApp dosyaları şu anda desteklenmiyor: 

* Temsilci alt ağa uygulanan ağ güvenlik grupları (Nsg'ler)
* Kullanıcı tanımlı yollar (Udr) ile Azure NetApp dosya alt ağ sonraki atlama
* Azure ilkeleri (örneğin, özel adlandırma ilkeleri) Azure NetApp dosyaları arabirimi
* NetApp dosya Azure trafiği için yük Dengeleyiciler

Azure için NetApp dosyaları aşağıdaki ağ kısıtlamalar uygulanır:

* Bir sanal ağ ile Azure NetApp (eşlenen sanal ağlar da dahil olmak üzere) dosyaları kullanımda IP sayısı 1000 aşamaz.
* Her Azure sanal ağı (VNet), yalnızca bir alt ağ, Azure için NetApp dosyaları atanabilir.


### <a name="supported-network-topologies"></a>Desteklenen ağ topolojileri

Aşağıdaki tabloda Azure NetApp dosyaları tarafından desteklenen ağ topolojileri açıklanır.  Ayrıca, desteklenmeyen Topolojileri için geçici çözümler açıklar. 

|    Topolojileri    |    desteklenir    |     Geçici Çözüm    |
|-------------------------------------------------------------------------------------------------------------------------------|--------------------|-----------------------------------------------------------------------------|
|    Birim yerel bir sanal ağ bağlantısı    |    Evet    |         |
|    Bağlantı birime eşlenmiş vnet'teki (aynı bölge)    |    Evet    |         |
|    Birim (çapraz bölge veya genel eşdüzey hizmet sağlama) eşlenmiş VNet içinde bağlantısı    |    Hayır    |    None    |
|    Bir birime ExpressRoute ağ geçidi üzerinden bağlantı    |    Evet    |         |
|    ExpressRoute ağ geçidi ve VNet eşlemesi ile ağ geçidi geçişi üzerinden uç sanal ağı'nda bir birim için şirket içi bağlantı    |    Hayır    |    Merkez sanal ağa (Azure VNet ağ geçidi ile) yetkilendirilmiş bir alt ağ oluşturun    |
|    Bir birime uçtaki bir sanal ağ VPN ağ geçidi üzerinden şirket içi bağlantı    |    Evet    |         |
|    VPN gateway ve VNet eşlemesi ile ağ geçidi geçişi üzerinden uç sanal ağı'nda bir birim için şirket içi bağlantı    |    Evet    |         |


## <a name="virtual-network-for-azure-netapp-files-volumes"></a>Sanal ağ için Azure NetApp dosyaları birimleri

Bu bölümde, sanal ağ planlamaya yardımcı kavramlar açıklanır.

### <a name="azure-virtual-networks"></a>Azure sanal ağları

Bir Azure NetApp dosyaları toplu sağlamadan önce bir Azure sanal ağı (VNet) oluşturun veya aboneliğinizdeki mevcut bir gerekir. Sanal ağ, ağ sınırı birimin tanımlar.  Sanal ağlar oluşturma hakkında daha fazla bilgi için bkz. [Azure sanal ağ belgeleri](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview).

### <a name="subnets"></a>Alt ağlar

Alt ağlar, sanal ağ içindeki Azure kaynaklarını tarafından kullanılabilen ayrı adres alanları içine segmentlere ayırın.  Azure dosyaları NetApp birimleri adlı bir özel amaçlı alt ağda yer alan bir [alt temsilci](https://docs.microsoft.com/azure/virtual-network/virtual-network-manage-subnet). 

Alt ağ temsilci Azure NetApp dosyaları hizmet alt ağında hizmete özgü kaynakları oluşturmak için açık izinler verir.  Hizmet dağıtma konusunda benzersiz bir tanımlayıcı kullanır. Bu durumda, Azure NetApp dosyaları bağlantıyı etkinleştirmek için bir ağ arabirimi oluşturulur.

Yeni bir VNet kullanırsanız, bir alt ağ oluşturun ve Azure NetApp dosyaları alt konusundaki yönergeleri izleyerek temsilci [bir alt ağ Azure NetApp dosyaları temsilci](azure-netapp-files-delegate-subnet.md). Ayrıca, diğer hizmetleri temsilci olmayan mevcut bir boş alt devredebilirsiniz.

Sanal ağ başka bir VNet ile eşlenirse VNet adres alanı genişletilemiyor. Bu nedenle, yeni temsilci alt VNet adres alanı içinde oluşturulması gerekir. Adres alanı genişletmeniz gerekiyorsa, VNet adres alanı genişletmeden önce eşlemesi silmeniz gerekir.

### <a name="udrs-and-nsgs"></a>Udr ve Nsg'ler

Kullanıcı tanımlı yollar (Udr) ve ağ güvenlik grupları (Nsg'ler) temsilci alt ağlarda Azure NetApp dosyaları için desteklenmez.

Geçici bir çözüm olarak, diğer alt ağlara izin vermek veya Azure NetApp temsilci dosyaları alt ağından gelen ve giden trafiği reddetmek için Nsg'leri uygulayabilirsiniz.  

## <a name="azure-native-environments"></a>Azure yerel ortamları

Aşağıdaki diyagramda bir Azure-yerel ortam gösterilmektedir:

![Azure yerel ağ ortamı](../media/azure-netapp-files/azure-netapp-files-network-azure-native-environment.png)

### <a name="local-vnet"></a>Local VNet

Oluşturun veya aynı VNet içindeki bir sanal makineden (VM) bir Azure NetApp dosyaları birimine bağlanmak için temel bir senaryodur. Yukarıdaki diyagramda VNet 2 birim 1 yetkilendirilmiş bir alt ağ içinde oluşturulur ve VM 1'de varsayılan alt ağında bağlanabilir.

### <a name="vnet-peering"></a>VNet eşlemesi

Ek sanal ağlar aynı bölgede birbirlerinin kaynaklara erişmesi gereken varsa, sanal ağlar kullanarak bağlanabilir [VNet eşlemesi](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview) Azure altyapısı aracılığıyla güvenli bağlantıyı etkinleştirmek için. 

Yukarıdaki diyagramda VNet 2 ve 3 VNet düşünün. VM 2 VM 3 veya birim 2'ye bağlanması gerekiyorsa veya VM 3 VM 2 veya birim 1'e bağlanması gerekiyorsa, VNet 2 ve 3 sanal ağ arasında eşleme VNet etkinleştirmeniz gerekir. 

Ayrıca, burada VNet 1 ile VNet 2 eşlenirse ve VNet 2 VNet 3 aynı bölgedeki eşlenmiş bir senaryo düşünün. VNet 1 kaynaklardan VNet 2'deki kaynaklara bağlanabilir ancak VNet 1 ve 3 VNet eşlendikten sürece VNet 3 ' teki kaynaklara bağlanamazsınız. 

Yukarıdaki diyagramda, VM 3 birim 1'e bağlanabilir ancak VM 4 birim 2'ye bağlanamıyor.  Uç sanal ağları değil eşlenmiş olduğunu, bunun nedeni olan ve _geçiş yönlendirmesi desteklenmez VNet eşlemesi üzerinden_.

## <a name="hybrid-environments"></a>Karma ortamlar

Aşağıdaki diyagramda karma bir ortamda gösterilmektedir: 

![Hibrit ağ ortamı](../media/azure-netapp-files/azure-netapp-files-network-hybrid-environment.png)

Karma senaryosunda, azure'daki kaynaklara erişim uygulamalar şirket içi veri merkezlerinden gerekir.  Bu, veri merkezinizi Azure'a genişletmek istediğiniz ya da yerel Azure hizmetlerini kullanmak istediğiniz durumda veya olağanüstü durum kurtarma için. Bkz: [VPN Gateway planlama seçenekleri](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways?toc=%2fazure%2fvirtual-network%2ftoc.json#planningtable) birden çok kaynak şirket içi bir siteden siteye VPN veya ExpressRoute üzerinden azure'daki kaynaklara bağlanma hakkında bilgi için.

Bir karma merkez-uç topolojisinde merkez sanal ağa azure'da, merkezi bir şirket içi ağınıza bağlantı noktası olarak görev yapar. Uçlar hub'ı ile eşlenmiş sanal ağlar ve iş yüklerini yalıtmak için kullanılabilirler.

Yapılandırmasına bağlı olarak, hub ve bağlı bileşenler kaynakları şirket içi kaynaklara bağlanabilir.

Yukarıda gösterilen topoloji, şirket içi ağınızı Azure Vnet'te hub'ına bağlı ve merkez sanal ağı ile 2 uç aynı bölgedeki sanal ağlar eşlenmiş vardır.  Bu senaryoda, Azure NetApp dosyaları birimleri için desteklenen bağlantı seçenekleri aşağıdaki gibidir:

* Şirket içi kaynaklar VM 1 ve 2 VM birimi 1 için bir siteden siteye VPN veya ExpressRoute devresi üzerinden hub'ı bağlantı kurabilir. 
* Şirket içi kaynaklara VM 1 ve VM 2 birim 2 veya 3 birim için bir siteden siteye VPN ve bölgesel sanal ağ eşlemesi üzerinden bağlanabilirsiniz.
* Hub'ında VM 3 VNet birim 2'ye uç sanal ağ 1 ve toplu 3 uç VNet 2 bağlanabilirsiniz.
* Uç sanal ağ 1 4 VM ve VM 5'ten uç VNet 2 birim 1 merkez sanal ağa bağlanabilir.

VM 4'te uç sanal ağ 1 uç VNet 2 birim 3'e bağlanılamıyor. Ayrıca, VM 5'te uç VNet2 uç sanal ağ 1 Birim 2'ye bağlanamıyor. Uç sanal ağlar eşlenmiş değil çünkü bu durumda ve _geçiş yönlendirmesi desteklenmez VNet eşlemesi üzerinden_.

## <a name="next-steps"></a>Sonraki adımlar

[Temsilci bir alt ağ Azure NetApp dosyaları](azure-netapp-files-delegate-subnet.md)
