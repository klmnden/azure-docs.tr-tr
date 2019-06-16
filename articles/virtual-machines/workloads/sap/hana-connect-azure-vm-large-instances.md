---
title: SAP hana (büyük örnekler) azure'da sanal makinelerden bağlantı kurulumu | Microsoft Docs
description: SAP HANA (büyük örnekler) Azure'da kullanmak için sanal makinelerden bağlantı kurulumu.
services: virtual-machines-linux
documentationcenter: ''
author: msjuergent
manager: patfilot
editor: ''
tags: azure-resource-manager
keywords: ''
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/25/2019
ms.author: juergent
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: df60b31ce950cc6c242c8077e59d90c41771e4c3
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66239546"
---
# <a name="connecting-azure-vms-to-hana-large-instances"></a>Azure VM'lerini HANA Büyük Örnekleri'ne bağlama

Makaleyi [SAP HANA (büyük örnekler) azure'da nedir?](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-overview-architecture) ilgili Azure SAP uygulama katmanında en az dağıtımla HANA büyük örnekleri aşağıdaki gibi görünür:

![SAP HANA (büyük örnekler) Azure ve şirket içi Azure Vnet'e bağlanır](./media/hana-overview-architecture/image1-architecture.png)

Azure sanal ağı tarafında daha yakından baktığımızda gerek yoktur:

- SAP uygulama katmanı Vm'leri dağıtma olacak bir Azure sanal ağı tanımı.
- Bir varsayılan alt ağında gerçekten VM'ler içine dağıtılır biri olan Azure sanal ağı tanımı.
- En az bir VM alt ağı ve bir Azure ExpressRoute sanal ağ geçidi alt ağı oluşturulan Azure sanal ağı gerekir. Bu alt ağlar, IP adresi aralıklarını belirtilen ve ele alındığı olarak aşağıdaki bölümlerde atanmalıdır.


## <a name="create-the-azure-virtual-network-for-hana-large-instances"></a>HANA büyük örnekler için Azure sanal ağı oluşturma

>[!Note]
>HANA büyük örnekler için Azure sanal ağı Azure Resource Manager dağıtım modeliyle oluşturulması gerekir. Genellikle Klasik dağıtım modeli olarak bilinen eski Azure dağıtım modeli, HANA büyük örneği çözüm tarafından desteklenmiyor.

Sanal ağ oluşturmak için Azure portalı, PowerShell, Azure şablonu veya Azure CLI'yı kullanabilirsiniz. (Daha fazla bilgi için [Azure portalını kullanarak bir sanal ağ oluşturma](../../../virtual-network/manage-virtual-network.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json#create-a-virtual-network)). Aşağıdaki örnekte, Azure portalı kullanılarak oluşturulan bir sanal ağ atacağız.

Söz konusu olduğunda **adres alanı** bu belgede, Azure sanal ağı kullanmasına izin verilen adres alanına. Bu adres alanı ayrıca sanal ağı için BGP yolu yayılmasını kullanan adresi aralığıdır. Bu **adres alanı** burada görebilirsiniz:

![Azure Portalı'nda görüntülenen bir Azure sanal ağının adres alanı](./media/hana-overview-connectivity/image1-azure-vnet-address-space.png)

Önceki örnekte, 10.16.0.0/16 ile Azure sanal ağı kullanmak için bir oldukça büyük ve daha geniş IP adresi aralığı sağlandı. Bu nedenle, sonraki alt ağlar bu sanal ağ içindeki tüm IP adres aralıklarını, aralıkları, adres alanı içinde olabilir. Azure'da tek bir sanal ağ için böyle bir büyük adres aralığı genellikle önerilmemektedir. Ancak Azure sanal ağ içinde tanımlanan alt ağları içine göz atalım:

![Azure sanal ağ alt ağları ve IP adres aralıkları](./media/hana-overview-connectivity/image2b-vnet-subnets.png)

(Burada "varsayılan" olarak adlandırılır) ilk bir VM alt ağı "GatewaySubnet" adlı bir alt ağ ile sanal ağ ile bakacağız.

İki önceki grafik **sanal ağ adres alanı** hem de kapsayan **Azure VM alt ağı IP adres aralığı** ve sanal ağ geçidi.

Kısıtlayabilirsiniz **sanal ağ adres alanı** her alt ağ tarafından kullanılan belirli aralıklara. Ayrıca tanımlayabilirsiniz **sanal ağ adres alanı** bir sanal olarak birden çok belirli aralıkları, burada gösterildiği gibi ağ:

![Azure sanal ağ adres alanı ile iki boşluk](./media/hana-overview-connectivity/image3-azure-vnet-address-space_alternate.png)

Bu durumda, **sanal ağ adres alanı** tanımlı iki boşluk bulunmaz. Bunlar, alt ağ IP adresi aralığı Azure VM ve sanal ağ geçidi için tanımlanan IP adresi aralıklarını ile aynı olur. 

İstediğiniz herhangi bir adlandırma standardı, bu Kiracı alt ağları (VM ağları) için kullanabilirsiniz. Ancak, **ayrıca bir ve yalnızca bir ağ geçidi alt ağı için sanal ağın her zaman olmalıdır** SAP HANA (büyük örnekler) Azure ExpressRoute bağlantı hattı için bağlanan. **Bu ağ geçidi alt ağı "GatewaySubnet" olarak adlandırılması gerektiğini** ExpressRoute ağ geçidi düzgün bir şekilde yerleştirildiğinden emin olmak için.

> [!WARNING] 
> Ağ geçidi alt ağı "GatewaySubnet" her zaman adlandırılması önemlidir.

Birden fazla VM alt ağları ve bitişik olmayan adres aralıkları kullanabilirsiniz. Bu adres aralıkları tarafından ele alınması gereken **sanal ağ adres alanı** sanal ağ. Bunlar toplu bir halde olabilir. Ayrıca VM alt ağları ve ağ geçidi alt ağı aralıklarının tam bir listesi halinde olabilirler.

HANA büyük örnekleri bağlanan bir Azure sanal ağ hakkında önemli bilgiler bir özeti aşağıda verilmiştir:

- Göndermeniz gerekir **sanal ağ adres alanı** ilk dağıtımının bir HANA büyük örnekleri gerçekleştirirken Microsoft'a. 
- **Sanal ağ adres alanı** her iki alt ağ IP adresi aralığı için Azure VM ve sanal ağ geçidi aralıkları kapsayan daha büyük bir aralık olabilir.
- Ya da farklı IP kapsayan birden çok aralık adres aralıklarını VM alt ağı IP adresi aralıklarının ve sanal ağ geçidi IP adresi aralığı gönderebilirsiniz.
- Tanımlanan **sanal ağ adres alanı** BGP yönlendirme yayılma için kullanılır.
- Ağ geçidi alt ağı adı olması gerekir: **"GatewaySubnet"** .
- Adres alanı, HANA büyük örneği tarafında bir filtre olarak izin vermek veya trafiği HANA büyük örneği birimine Azure'dan engellemek için kullanılır. Azure sanal ağı HANA büyük örneği tarafta filtreleme için yapılandırılan IP adresi aralıklarını ve BGP yönlendirme bilgileri eşleşmesi gerekir. Aksi takdirde, bağlantı sorunları oluşabilir.
- Sonraki bölümde açıklanan bazı ayrıntılar ağ geçidi alt ağı hakkında **HANA büyük örneği ExpressRoute için sanal ağa bağlanma.**



## <a name="different-ip-address-ranges-to-be-defined"></a>Tanımlanacak farklı IP adresi aralıkları 

HANA büyük örnekleri dağıtmak için gerekli IP adresi aralıklarını bazıları zaten kullanıma. Ancak, ayrıca önemli daha fazla IP adresi aralıklarını vardır. Aşağıdaki IP adresi aralıklarını tüm, Microsoft'a gönderilmesi gerekiyor. Ancak, ilk dağıtım için bir istek göndermeden önce bunları tanımlamanız gerekir:

- **Sanal ağ adres alanı**: **Sanal ağ adres alanı** adres alanı parametreniz Azure sanal ağlarda bulunan atadığınız IP adresi aralıklarını olduğu. Bu ağlar, SAP HANA büyük örneği ortama bağlanın. Bu adres alanı parametresi bir çok satırlı değer olduğunu öneririz. Azure VM alt ağ aralığını ve Azure ağ geçidi alt ağı aralıklarının oluşmalıdır. Bu alt ağ aralığı, önceki grafikleri gösterildi. Şirket içi veya sunucu IP havuzu veya ER P2P adres aralığı ile çakışmamalıdır. Bu IP adresi aralıklarının nasıl alır? Kurumsal ağ ekip veya hizmet sağlayıcısı konumunuza ağınızda kullanılmayan bir veya birden çok IP adresi sağlamanız gerekir. Örneğin, 10.0.1.0/24 Azure VM alt ağı olan ve Azure ağ geçidi alt ağınızın alt 10.0.2.0/28 yer.  Azure sanal ağ adres alanınızı olarak tanımlanır öneririz: 10.0.1.0/24 ve 10.0.2.0/28. Adres alanı değerleri toplanabilir, ancak bunları için alt ağ aralıklarını eşleşen öneririz. Bu şekilde ağınızdaki başka bir yerde daha büyük bir adres alanları içindeki kullanılmayan IP adresi aralıklarını yeniden yanlışlıkla önleyebilirsiniz. **Sanal ağ adres alanını IP adresi aralığıdır. İlk bir dağıtım için sorduğunuzda, Microsoft'a gönderilmesi gereken**.
- **Azure VM alt ağı IP adres aralığı:** Bu IP adresi aralığı için Azure sanal ağ alt ağı parametre atadığınız olur. Bu parametre, Azure sanal ağdaki ve SAP HANA büyük örneği ortamına bağlar. Bu IP adresi aralığı, Azure Vm'lerinizi IP adresleri atamak için kullanılır. Bu aralık dışında bir IP adresleri, SAP HANA büyük örneği sunucuları için bağlama izin verilir. Gerekirse, birden çok Azure VM alt ağı kullanabilirsiniz. Bir/24 öneririz her Azure VM alt ağ için CIDR bloğu. Bu adres aralığının bir parçası Azure sanal ağ adres alanında kullanılan değerleri olmalıdır. Bu IP adresi aralığı nasıl alır? Kurumsal ağ ekip veya hizmet sağlayıcısı, ağınızda kullanılmayan IP adresi aralığı sağlamalıdır.
- **Sanal ağ geçidi alt ağı IP adres aralığı:** Kullanmayı planladığınız özelliklerine bağlı olarak, önerilen boyutu şu ise:
   - Çok yüksek performanslı ExpressRoute ağ geçidi: / 26 adres bloğu--SKU'ları Type II sınıfı için gereklidir.
   - VPN ve ExpressRoute kullanarak bir yüksek performanslı ExpressRoute sanal ağ geçidi ile birlikte bulunma (veya daha küçük): / 27 adres bloğu.
   - Diğer tüm durumlarda: / 28 adres bloğu. Bu adres aralığının bir parçası "VNet adres alanı" değerler kullanılan değerleri olmalıdır. Bu adres aralığı, Microsoft'a gönderdiğiniz Azure sanal ağ adres alanı değerleri kullanılan değerlerin bir parçası olması gerekir. Bu IP adresi aralığı nasıl alır? Kurumsal ağ ekip veya hizmet sağlayıcısı, ağınızda şu anda kullanılmakta bir IP adresi aralığı sağlamalıdır. 
- **ER P2P bağlantı adres aralığı:** SAP HANA büyük örneği ExpressRoute (ER) P2P bağlantınız için bir IP aralığı rozsah. Bu IP adresi aralığı bir/29 olmalıdır CIDR IP adresi aralığı. Bu aralık ile şirket içi veya diğer Azure IP adresi aralıklarını çakışmamalıdır. Bu IP adresi aralığı, SAP HANA büyük örneği sunucularına, ExpressRoute sanal ağ geçidi'ndeki ER bağlantı kurmak için kullanılır. Bu IP adresi aralığı nasıl alır? Kurumsal ağ ekip veya hizmet sağlayıcısı, ağınızda şu anda kullanılmakta bir IP adresi aralığı sağlamalıdır. **Bir IP adresi aralığı rozsah. İlk bir dağıtım için sorduğunuzda, Microsoft'a gönderilmesi gereken**.  
- **Sunucu IP havuzu adres aralığı:** Bu IP adresi aralığı, HANA büyük örnek sunucularına tek bir IP adresi atamak için kullanılır. Bir/24 önerilen alt boyutudur CIDR bloğu. Gerekirse, en az 64 IP adresleri ile daha küçük olabilir. Bu aralıktaki ilk 30 IP adresleri, Microsoft tarafından kullanım için ayrılmıştır. Aralık boyutu seçtiğinizde bu olgu için dikkate aldığınızdan emin olun. Bu aralık, şirket içi veya diğer Azure IP adresleri çakışmamalıdır. Bu IP adresi aralığı nasıl alır? Kurumsal ağ ekip veya hizmet sağlayıcısı, ağınızda şu anda kullanılmakta bir IP adresi aralığı sağlamalıdır.  **Bu aralığı ilk bir dağıtım için isterken Microsoft'a gönderilmesi gereken bir IP adresi aralığı**.

Sonunda, Microsoft'a gönderilmesi gereken isteğe bağlı IP adresi aralığı:

- Kullanmayı tercih ederseniz [ExpressRoute Global erişim](https://docs.microsoft.com/azure/expressroute/expressroute-global-reach) şirket içinden HANA büyük örneği birimlerine doğrudan yönlendirme etkinleştirmek için başka bir /29 ayırmak gereken IP adresi aralığı. Bu aralık önce tanımlanan diğer IP adresi aralıklarını hiçbiriyle değil.
- Kullanmayı tercih ederseniz [ExpressRoute Global erişim](https://docs.microsoft.com/azure/expressroute/expressroute-global-reach) doğrudan bir HANA büyük örneği kiracısında bir Azure bölgesindeki başka bir Azure bölgesindeki başka bir HANA büyük örneği Kiracı için yönlendirmeyi etkinleştirmek için başka bir /29 ayırmak gereken IP adresi aralığı . Bu aralık önce tanımlanan diğer IP adresi aralıklarını hiçbiriyle değil.

ExpressRoute Global erişim ve kullanım etrafında HANA büyük örnekleri hakkında daha fazla bilgi için belgelere bakın:

- [SAP HANA (büyük örnekler) ağ mimarisi](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-network-architecture)
- [HANA büyük örnekleri için bir sanal ağı bağlama](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-connect-vnet-express-route)
 
Tanımlamak ve daha önce açıklanan IP adres aralıklarını planlama gerekir. Ancak, tüm bunları Microsoft'a iletmek gerekmez. Microsoft ad için gerekli olan IP adresi aralıklarını şunlardır:

- Azure sanal ağ adres alanları
- ER P2P bağlantı adres aralığı
- Sunucu IP havuzu adres aralığı

HANA büyük örneklerine bağlanmak için gereken ek sanal ağlar eklerseniz, Microsoft'a eklediğiniz yeni bir Azure sanal ağ adres alanı göndermek zorunda. 

Yapılandırma ve sonunda Microsoft'a sağlamak gereken farklı aralıklar ve bazı örnek aralıklar ın bir örneği verilmiştir. Azure sanal ağ adres alanı değeri ilk örnekte toplu değil. Ancak, ilk Azure VM alt ağ adres aralığı IP aralıklarını ve sanal ağ geçidi alt ağı IP adresi aralığı tanımlanır. 

Yapılandırma ve Azure sanal ağ adres alanının bir parçası ek VM alt ağı, başarılı ek IP adres aralıklarını gönderin Azure sanal ağ içindeki birden fazla VM alt ağları kullanabilirsiniz.

![IP adresi aralıkları SAP HANA (büyük örnekler) Azure üzerinde çok az dağıtım gerekli](./media/hana-overview-connectivity/image4b-ip-addres-ranges-necessary.png)

Grafik konumunuza isteğe bağlı olarak kullanılması ExpressRoute Global erişim için gerekli olan ek IP adresi göstermez.

Microsoft'a gönderdiğiniz verileri de toplayabilirsiniz. Bu durumda, Azure sanal ağının adres alanı, yalnızca tek bir boşluk içerir. Önceki örnekte IP adresi aralıkları kullanarak toplu sanal ağ adres alanını aşağıdaki gibi görünebilir:

![İkinci IP adresi aralıkları SAP HANA (büyük örnekler) Azure üzerinde çok az dağıtım gerekli olasılığı](./media/hana-overview-connectivity/image5b-ip-addres-ranges-necessary-one-value.png)

Örnekte, Azure sanal ağ, adres alanında tanımlanmış iki küçük aralık yerine 4096 IP adresleri kapsayan tek bir büyük aralık sahibiz. Adres alanının büyük tanımı bazı yerine büyük aralıklar kullanılmamış olarak bırakır. Sanal ağ adres alanı değerleri için BGP yolu yayılmasını kullanılmadığından kullanım kullanılmayan aralıkları şirket içi veya ağınızdaki başka bir yerde yönlendirme sorunlara neden olabilir. Grafik konumunuza isteğe bağlı olarak kullanılması ExpressRoute Global erişim için gerekli olan ek IP adresi göstermez.

Kullandığınız gerçek bir alt ağ adres alanı ile sıkı bir şekilde hizalı adres alanı tutmanızı öneririz. Sanal ağda, kapalı kalma süresi olmaksızın gerekirse her zaman yeni bir adres alanı değerleri daha sonra ekleyebilirsiniz.
 
> [!IMPORTANT] 
> Her bir IP adres aralığı ER P2P, sunucu IP havuzu ve Azure sanal ağ adres alanını gerekir **değil** birbiriyle veya ağınızda kullanılan herhangi bir aralığı ile çakışıyor. Her ayrık olmalıdır. Önceki iki grafik gösterdiği gibi bunlar ayrıca bir alt ağdan başka bir aralığı olamaz. Aralıkları arasında çakışma meydana gelirse, Azure sanal ağı ExpressRoute devresine bağlama değil.

## <a name="next-steps-after-address-ranges-have-been-defined"></a>Adres aralıkları tanımlandıktan sonra sonraki adımlar
IP adresi aralıklarını tanımlandıktan sonra yapılması gerekenler:

1. Azure sanal ağ adres alanı, ER P2P bağlantı ve sunucu IP havuzu adres aralığı, belgenin başında listelenen diğer verileriyle birlikte IP adresi aralıklarını gönderin. Bu noktada, sanal ağ ve VM alt ağı oluşturmak de başlatabilir. 
2. Bir ExpressRoute bağlantı hattı, Azure aboneliğinizi ve HANA büyük örneği damgasında arasında Microsoft tarafından oluşturulur.
3. Bir kiracı ağ üzerinde büyük örneği damgasında Microsoft tarafından oluşturulur.
4. Microsoft, IP adresleri, Azure sanal ağ adresi alanından HANA büyük örnekleri ile iletişim kuran kabul etmek için SAP hana (büyük örnekler) Azure altyapı ağı yapılandırır.
5. Microsoft, satın aldığınız Azure (büyük örnekler) SKU üzerinde belirli SAP HANA bağlı olarak bir kiracı ağını bir işlem biriminde atar. Ayrıca ayırır ve depolama bağlar ve (SUSE veya Red Hat Linux) işletim sistemini yükler. Bu birimleri için IP adresleri Microsoft'a gönderdiğiniz sunucu IP havuzu adres aralığı dışına alınır.

Dağıtım işleminin sonunda, Microsoft aşağıdaki veriler olanak sağlar:
- HANA büyük örnekler için Azure sanal ağları bağlayan ExpressRoute bağlantı hattı, Azure sanal ağlara bağlanmak için gerekli olan bilgileri:
     - Yetkilendirme anahtarları
     - ExpressRoute PeerID
- ExpressRoute bağlantı hattı ve Azure sanal ağı oluşturduktan sonra HANA büyük örnekleri erişmek için veriler.

Ayrıca, HANA büyük örnekleri belgede bağlanma sırası bulabilirsiniz [SAP HANA (büyük örnekler) Azure Kurulumu](https://azure.microsoft.com/resources/sap-hana-on-azure-large-instances-setup/). Aşağıdaki adımların birçok örnek bir dağıtım söz konusu belgedeki gösterilir. 

## <a name="next-steps"></a>Sonraki adımlar

- Başvurmak [HANA büyük örneği ExpressRoute için sanal ağa bağlanma](hana-connect-vnet-express-route.md).
