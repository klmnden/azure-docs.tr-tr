---
title: SAP hana (büyük örnekler) azure'da sanal makinelerden bağlantı kurulumu | Microsoft Docs
description: SAP HANA (büyük örnekler) Azure'da kullanmak için sanal makinelerden bağlantı kurulumu.
services: virtual-machines-linux
documentationcenter: ''
author: RicksterCDN
manager: jeconnoc
editor: ''
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 09/10/2018
ms.author: rclaus
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: f0e09e6dfce5d0ede525f461193866219b7f9429
ms.sourcegitcommit: 2d961702f23e63ee63eddf52086e0c8573aec8dd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/07/2018
ms.locfileid: "44164781"
---
# <a name="connecting-azure-vms-to-hana-large-instances"></a>Azure sanal makineleri HANA büyük örneklerine bağlanma

Zaten belirtilen [SAP HANA (büyük örnekler) genel bakışı ve mimarisi azure'da](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-overview-architecture) en az dağıtımı HANA büyük örnekleri ile SAP uygulama katmanında Azure görünüyor ister:

![SAP HANA (büyük örnekler) Azure ve şirket içi Azure Vnet'e bağlanır](./media/hana-overview-architecture/image3-on-premises-infrastructure.png)

Azure sanal ağı tarafında daha yakından baktığımızda biz gereksinimini dikkat edin:

- SAP uygulama katmanında Vm'leri dağıtmak için kullanılacak edecek Azure sanal ağı tanımı.
- Otomatik olarak bir varsayılan alt ağ Azure sanal ağ içindeki anlamına gelir, tanımladığınız gerçekten Vm'leri dağıtmak için kullanılan bir bileşendir.
- En az bir VM alt ağı ve bir ExpressRoute ağ geçidi alt ağı oluşturulan Azure sanal ağı gerekir. Bu alt ağlar, IP adresi aralıklarını belirtilen ve ele alındığı olarak aşağıdaki bölümlerde atanmalıdır.

Bu nedenle, HANA büyük örnekler için Azure VNet oluşturulduğu içine biraz daha yakından bakalım

## <a name="creating-the-azure-vnet-for-hana-large-instances"></a>HANA büyük örnekler için Azure VNet oluşturma

>[!Note]
>Azure sanal ağı HANA büyük örneği için Azure Resource Manager dağıtım modeli kullanılarak oluşturulmalıdır. Genellikle, Klasik dağıtım modeli bilinen eski Azure dağıtım modeli, HANA büyük örneği çözümüyle desteklenmiyor.

Azure portalı, PowerShell, Azure şablonu veya Azure CLI kullanarak sanal ağ oluşturulabilir (bkz [Azure portalını kullanarak bir sanal ağ oluşturma](../../../virtual-network/manage-virtual-network.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json#create-a-virtual-network)). Aşağıdaki örnekte, Azure Portalı aracılığıyla oluşturulan bir sanal ağı içine bakacağız.

Azure Portalı aracılığıyla Azure VNet tanımlarını içine baktığımızda, bazı tanımlarını ve bu sorundan listelemek için nasıl ilişkilendirilmesi halinde bakalım farklı IP adresi aralıkları. Biz hakkında konuşmak gibi **adres alanı**, Azure sanal ağı kullanmak için izin verilen adres alanı demek isteriz. Bu adres alanı ayrıca sanal ağı için BGP yolu yayılmasını kullanan adresi aralığıdır. Bu **adres alanı** burada görebilirsiniz:

![Azure VNet, adres alanı Azure portalında gösterilen](./media/hana-overview-connectivity/image1-azure-vnet-address-space.png)

Bu durumda, önceki 10.16.0.0/16 ile Azure sanal ağı kullanmak için bir çok büyük ve daha geniş IP adresi aralığı verildi. Sonraki alt ağlar bu sanal ağ içindeki tüm IP adresi aralığı, 'adres alanı' içinde kendi aralıkları olabilir anlamına gelir. Genellikle bu tür bir büyük adres aralığı azure'da tek bir VNet için öneriyoruz değil. Ancak, bir adım daha ileri alma, Azure Vnet'te tanımlanan alt ağları uygulamasına göz atalım:

![Azure sanal ağ alt ağları ve IP adres aralıkları](./media/hana-overview-connectivity/image2b-vnet-subnets.png)

Gördüğünüz gibi VNet (burada 'default' olarak da bilinir) ilk bir VM alt ağı "GatewaySubnet" adlı bir alt ağ ile bakacağız.
Aşağıdaki bölümde, grafik 'default' çağrıldı alt ağın IP adresi aralığı diyoruz **Azure VM alt ağ IP adresi aralığı**. Aşağıdaki bölümlerde, ağ geçidi alt ağının IP adresi aralığı diyoruz **VNet ağ geçidi alt ağı IP adresi aralığı**. 

Yukarıdaki iki grafik tarafından gösterilen durumda da, gördüğünüz **VNet adres alanının** her ikisi de kapsayan **Azure VM alt ağ IP adresi aralığı** ve **VNet ağ geçidi alt ağı IP adresi aralığı**. 

Gerek duyduğunuz tasarruf etmek veya belirli IP adresi aralıklarınızı ile diğer durumlarda, kısıtlayabilirsiniz **VNet adres alanının** bir sanal ağın her alt ağ tarafından kullanılan belirli aralıklara. Bu durumda, tanımlayabileceğiniz **VNet adres alanının** bir VNet gibi birden çok özel aralıkları burada gösterildiği gibi:

![Azure sanal ağ adres alanı ile iki boşluk](./media/hana-overview-connectivity/image3-azure-vnet-address-space_alternate.png)

Bu durumda, **VNet adres alanının** tanımlı iki boşluk bulunmaz. Bu iki alanları için tanımlanan IP adresi aralıklarını aynı **Azure VM alt ağ IP adresi aralığı** ve **VNet ağ geçidi alt ağı IP adresi aralığı.**

İstediğiniz herhangi bir adlandırma standardı, bu Kiracı alt ağları (VM ağları) için kullanabilirsiniz. Ancak, **ayrıca bir ve yalnızca bir ağ geçidi alt ağı her sanal ağa her zaman olmalıdır** SAP HANA (büyük örnekler) Azure ExpressRoute bağlantı hattı için bağlanan. Ve **bu ağ geçidi alt ağı "GatewaySubnet" her zaman adlandırılmalıdır** ExpressRoute ağ geçidini uygun yerleşimini emin olmak için.

> [!WARNING] 
> Ağ geçidi alt ağı "GatewaySubnet" her zaman adlandırılır önemlidir

Hatta bitişik olmayan adres aralıkları kullanan birden fazla VM alt ağları kullanılabilir. Ancak daha önce belirtildiği gibi bu adres aralıkları tarafından ele alınması gereken **VNet adres alanının** vnet'in toplu halde veya VM alt ağları ve ağ geçidi alt ağı tam aralıkları listesi.

HANA büyük örnekleri bağlayan Azure sanal ağı hakkında önemli olgu özetleme:

- Microsoft'a göndermek gereken **VNet adres alanının** HANA büyük örnekleri ilk dağıtımı yaparken. 
- **VNet adres alanının** Azure VM alt ağı IP adresi aralıklarının ve VNet ağ geçidi alt ağı IP adresi aralığını kapsayan tek bir büyük aralık olabilir.
- Veya olarak gönderebilirsiniz **VNet adres alanının** farklı IP kapsayan birden çok aralık adres aralıklarını VM alt ağı IP adresi aralıklarının ve VNet ağ geçidi alt ağı IP adresi aralığı.
- Tanımlanan **VNet adres alanının** kullanılan BGP yönlendirme yayma olduğu.
- Ağ geçidi alt ağı adı olması gerekir: **"GatewaySubnet"**
- **VNet adres alanının** HANA büyük örneği tarafında bir filtre olarak izin vermek veya trafiği HANA büyük örneği birimine Azure'dan engellemek için kullanılır. Azure sanal ağı ve IP adresi aralıklarını HANA büyük örneği tarafta filtreleme için yapılandırılmış BGP yönlendirme bilgilerini eşleşmiyorsa bağlantı sorunları oluşabilir.
- Ağ geçidi alt ağı ilgili olan bazı ayrıntılar aşağıya 'HANA büyük örneği ExpressRoute için bir VNet bağlama' bölümünde ele alınan vardır



## <a name="different-ip-address-ranges-to-be-defined"></a>Tanımlanacak farklı IP adresi aralıkları 

Zaten bazı HANA büyük örnekleri önceki bölümlerindeki dağıtmak gerekli IP adresi aralıklarını kullanıma sunduk. Ancak, önemli bazı daha fazla IP adresi aralıklarını vardır. Şimdi, diğer bazı ayrıntılar geçelim. Tüm Microsoft'a gönderilmesi gerekiyor aşağıdaki IP adresleri, ilk dağıtım için bir istek göndermeden önce tanımlanması gerekir:

- **VNet adres alanı:** zaten daha önce sunulan, veya IP adresi konumunuza atamış (veya atamak için plan) Azure sanal ağ (VNet) SAP HANA büyük örneği ortama bağlanma adresi alanı parametreniz üzeresiniz. Bu adres alanı parametre grafikler, daha önce gösterildiği gibi Azure VM alt ağı aralıklarının ve Azure ağ geçidi alt ağ aralığı oluşan bir çok satırlı değer olduğunu önerilir. Bu aralık, şirket içi veya ER P2P ya da sunucu IP havuzu adres aralığı ile çakışmamalıdır. Bu veya bu IP adresi aralıklarının almak nasıl? Kurumsal ağ ekip veya hizmet sağlayıcısı, bir veya birden çok IP adresi veya ağınızda kullanılmayan aralıklarının, sağlamanız gerekir. Örnek: 10.0.1.0/24 (daha önce bakın), Azure VM alt ağı olduğundan ve (aşağıdaki bakın) Azure ağ geçidi alt ağınızı 10.0.2.0/28, ardından Azure VNet adres alanınızı iki satır olması önerilir; 10.0.1.0/24 ve 10.0.2.0/28. Adres alanı değerlerin toplanabildiği olsa da, bunları için alt ağ aralıklarını eşleşen ağınızda kullanılmayan IP adresi aralıkları içinde büyük adres alanları gelecekte başka bir yerde yanlışlıkla kullanılmasını önlemek için önerilir. **VNET adres alanı olduğu için ilk dağıtımı isteyen, Microsoft'a gönderilmesi gereken bir IP adresi aralığı**

- **Azure VM alt ağ IP adresi aralığı:** bu IP adresi aralığı, daha önce zaten ele aynıdır atamış bir (veya atamak için plan) Azure sanal ağ alt ağı parametresine Azure vnet'inizde SAP HANA büyük örneği ortamla bağlantı kuruluyor. Bu IP adresi aralığı, Azure Vm'lerinizi IP adresleri atamak için kullanılır. Bu aralık dışında bir IP adresleri, SAP HANA büyük örneği sunucuları için bağlama izin verilir. Gerekirse, birden çok Azure VM alt ağları kullanılabilir. Bir/24 CIDR bloğu, Microsoft tarafından her Azure VM alt ağı için önerilir. Bu adres aralığının bir parçası Azure sanal ağ adres alanında kullanılan değerleri olmalıdır. Bu IP adresi aralığı almak nasıl? Kurumsal ağ ekip veya hizmet sağlayıcısı, şu anda ağınızda kullanılmayan bir IP adresi aralığı sağlamalıdır.

- **VNet ağ geçidi alt ağı IP adres aralığı:** kullanmayı planladığınız özelliklerine bağlı olarak, önerilen boyut olacaktır:
   - Çok yüksek performanslı ExpressRoute ağ geçidi: / 26 adres bloğu - SKU'ları Type II sınıfı için gerekli
   - VPN ve ExpressRoute kullanarak bir yüksek performanslı ExpressRoute ağ geçidi ile birlikte kullanımı (veya daha küçük): / 27 adres bloğu
   - Diğer tüm durumlarda: / 28 adres bloğu. Bu adres aralığının bir parçası "VNet adres alanı" değerler kullanılan değerleri olmalıdır. Bu adres aralığının bir parçası olarak Microsoft'a göndermek için gereken Azure VNet adres alanının değerleri kullanılan değerleri olmalıdır. Bu IP adresi aralığı almak nasıl? Kurumsal ağ ekip veya hizmet sağlayıcısı, şu anda ağınızda kullanılmayan bir IP adresi aralığı sağlamalıdır. 

- **Adres aralığını ER P2P bağlantı:** SAP HANA büyük örneği ExpressRoute (ER) P2P bağlantınız için bir IP aralığı bu aralığı. Bu IP adresi aralığı bir/29 olmalıdır CIDR IP adresi aralığı. Bu aralık ile şirket içi veya diğer Azure IP adresi aralıklarını çakışmamalıdır. Bu IP adresi aralığı, Azure sanal ağ ExpressRoute ağ geçidi'ndeki SAP HANA büyük örneği sunucuları ER bağlantı kurmak için kullanılır. Bu IP adresi aralığı almak nasıl? Kurumsal ağ ekip veya hizmet sağlayıcısı, şu anda ağınızda kullanılmayan bir IP adresi aralığı sağlamalıdır. **Bu aralığı ilk bir dağıtım için isterken Microsoft'a gönderilmesi gereken bir IP adresi aralığı**
  
- **Sunucu IP havuzu adres aralığı:** bu IP adresi aralığı, HANA büyük örnek sunucularına tek bir IP adresi atamak için kullanılır. Bir/24 önerilen alt boyutudur CIDR block - ancak olabilir 64 IP adreslerini sağlayan en küçük. Bu aralıktaki ilk 30 IP adresleri, Microsoft tarafından kullanım için ayrılmıştır. Bu durum için aralık boyutu seçerken dikkate alındığından emin olun. Bu aralık, şirket içi veya diğer Azure IP adresleri çakışmamalıdır. Bu IP adresi aralığı almak nasıl? Kurumsal ağ ekip veya hizmet sağlayıcısı, şu anda ağınızda kullanılmayan bir IP adresi aralığı sağlamalıdır. Bir/24, SAP HANA (büyük örnekler) azure'da gereken belirli IP adresleri atamak için kullanılacak (benzersiz CIDR önerilir) engelleyin. **Bu aralığı ilk bir dağıtım için isterken Microsoft'a gönderilmesi gereken bir IP adresi aralığı**
 
Tanımlama ve yukarıdaki IP adres aralıklarını planlama yapmanız da, tüm bunları Microsoft'a iletilmesi gerekir. Yukarıdaki özetlemek için Microsoft'a adlandırmak için gerekli IP adresi aralıklarını şunlardır:

- Azure sanal ağ adres alanları
- ER P2P bağlantı adres aralığı
- Sunucu IP havuzu adres aralığı

HANA büyük örneklerine bağlanmak için gereken ek sanal ağlar eklemek, yeni Azure VNet adres Microsoft'a ekliyoruz alanı göndermesini gerektirir. 

Yapılandırma ve sonunda Microsoft'a sağlamak gereken farklı aralıklar ve bazı örnek aralıklar ın bir örneği verilmiştir. Gördüğünüz gibi Azure VNet adres alanının değeri, ilk örnekte toplanmaz ancak ilk Azure VM alt ağı IP adresi aralığını ve VNet ağ geçidi alt ağı IP adresi aralığı aralığından tanımlı değil. Azure sanal ağ içindeki birden fazla VM alt ağları kullanılması uygun şekilde yapılandırmayı ve gönderme ek IP adresi aralıkları ek VM alt ağı, başarılı, Azure VNet adres alanının bir parçası.

![IP adresi aralıkları SAP HANA (büyük örnekler) Azure üzerinde çok az dağıtım gerekli](./media/hana-overview-connectivity/image4b-ip-addres-ranges-necessary.png)

Ayrıca Microsoft'a gönderdiğiniz verileri toplama olanağına sahip olursunuz. Bu durumda, Azure VNet adres alanının yalnızca bir alanı içerir. Önceki örnekte kullanılan IP adresi aralıkları kullanarak. Bu toplu VNet adres alanı gibi görünebilir:

![İkinci IP adresi aralıkları SAP HANA (büyük örnekler) Azure üzerinde çok az dağıtım gerekli olasılığı](./media/hana-overview-connectivity/image5b-ip-addres-ranges-necessary-one-value.png)

Azure sanal ağ adres alanında tanımlanmış iki küçük aralık yerine, yukarıdaki görebileceğiniz gibi 4096 IP adresleri kapsayan tek bir büyük aralık sahibiz. Adres alanının büyük tanımı bazı yerine büyük aralıklar kullanılmamış olarak bırakır. VNet adres alanının değerleri için BGP yolu yayılmasını kullanılmadığından kullanım kullanılmayan aralıkları şirket içi veya ağınızdaki başka bir yerde yönlendirme sorunlara neden olabilir. Bu nedenle kullanılan gerçek bir alt ağ adres alanı ile sıkı bir şekilde hizalı adres alanı tutmanız önerilir. VNet üzerinde kapalı kalma süresi olmaksızın gerekirse her zaman yeni bir adres alanı değerleri daha sonra ekleyebilirsiniz.
 
> [!IMPORTANT] 
> Her IP adresi aralığı, ER-P2P, sunucu IP havuzu, Azure VNet adres alanının gerekir **değil** birbiriyle çakışmaması veya ağınızdaki başka bir yerde herhangi bir aralığı kullanılan; her ayrık olmalıdır ve iki grafik olarak önceki show olmayabilir bir alt ağdan başka bir aralığı. Aralıkları arasında çakışma meydana gelirse, Azure sanal ağı ExpressRoute devresine bağlama yok.

## <a name="next-steps-after-address-ranges-have-been-defined"></a>Adres aralıkları tanımlandıktan sonra sonraki adımlar
IP adresi aralıklarını tanımladıktan sonra aşağıdaki etkinlikleri olması gerekir:

1. IP adresi aralıklarını, belgenin başında listelenen diğer verileriyle birlikte Azure VNet adres alanı, ER P2P bağlantı ve sunucu IP havuzu adres aralığı için gönderin. Bu anda, sanal ağ ve VM alt ağı oluşturmak de başlatabilir. 
2. Bir Express Route bağlantı hattı, Azure aboneliğinizi ve HANA büyük örneği damgasında arasında Microsoft tarafından oluşturulur.
3. Bir kiracı ağ üzerinde büyük örneği damgasında Microsoft tarafından oluşturulur.
4. Microsoft, Azure sanal ağ adresi alanından iletişim kuran HANA büyük örnekleri ile IP adresleri kabul etmek için SAP HANA (büyük örnekler) Azure altyapı ağı yapılandırır.
5. Satın alınan Azure (büyük örnekler) SKU üzerinde belirli SAP HANA, bağlı olarak bir kiracı ağını bir işlem biriminde Microsoft atar, ayırmak ve depolama bağlamak ve (SUSE veya Red Hat Linux) işletim sistemini yükleyin. Bu birimleri için IP adresleri Microsoft'a sunucu IP havuzu adres aralığı gönderdiğiniz dışında alınır.

Dağıtım işleminin sonunda, Microsoft aşağıdaki veriler olanak sağlar:
- ExpressRoute bağlantı hattına HANA büyük örnekler için Azure sanal ağları bağlayan Azure Vnet'lerinizdeki bağlanmak için gereken bilgileri:
     - Yetkilendirme anahtarları
     - ExpressRoute PeerID
- ExpressRoute bağlantı hattı ve Azure sanal ağı belirttikten sonra HANA büyük örnekleri erişmek için veriler.

Ayrıca, HANA büyük örnekleri belgede bağlanma sırası bulabilirsiniz [SAP HANA büyük örnekler için uçtan uca Kurulum](https://azure.microsoft.com/resources/sap-hana-on-azure-large-instances-setup/). Aşağıdaki adımların birçok örnek bir dağıtım söz konusu belgedeki gösterilir. 

**Sonraki adımlar**

- Başvuru [HANA büyük örnek ExpressRoute için bir VNet bağlama](hana-connect-vnet-express-route.md).