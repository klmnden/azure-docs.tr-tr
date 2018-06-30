---
title: Altyapı ve SAP HANA azure'da (büyük örnekler) bağlanma | Microsoft Docs
description: SAP HANA (büyük örnekler) Azure üzerinde kullanmak için gerekli bağlantı altyapıyı yapılandırın.
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
ms.date: 06/04/2018
ms.author: rclaus
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 4c0f5d0c5ed3814495a68d7fd49d41cec521bbd7
ms.sourcegitcommit: 5a7f13ac706264a45538f6baeb8cf8f30c662f8f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/29/2018
ms.locfileid: "37114567"
---
# <a name="sap-hana-large-instances-infrastructure-and-connectivity-on-azure"></a>SAP HANA (büyük örnekler) altyapısı ve Azure ile ilgili bağlantı 

Bu kılavuzu okuduktan önce bazı tanımları önceden. İçinde [SAP HANA (büyük örnekler) genel bakış ve Azure ile ilgili mimari](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-overview-architecture) ile HANA büyük örneği birimlerin farklı iki sınıf gösterdiğimizi:

- S72, S72m, S144, S144m, S192, S192m ve adlandırdığımız 'ı sınıf türü olarak' S192xm SKU.
- S384, S384m, S384xm, S384xxm, S576m, S576xm, S768m, S768xm ve adlandırdığımız 'Type II sınıfı' SKU'ları S960m.

Sınıfı tanımlayıcıları HANA büyük örnek belge boyunca sonunda farklı özellikleri ve gereksinimleri HANA büyük örneği SKU'larında dayalı kullanılabilir olacak.

Sık kullandığınız diğer tanımların şunlardır:
- **Büyük örneği damga:** SAP HANA TDI olan bir donanım altyapı yığını sertifikalı ve Azure içindeki SAP HANA örnekleri çalıştırmak için ayrılmış.
- **SAP HANA azure'da (büyük örnekler):** SAP HANA büyük örneği Damgalar farklı Azure bölgelerinde dağıtılmış donanım sertifikalı TDI üzerinde HANA çalıştırmak Azure teklifin resmi adı örnekler içinde. İlgili dönem **HANA büyük örneği** azure'da (büyük örnekler) kısaltması SAP HANA ve yaygın olarak kullanılan bu teknik dağıtım kılavuzu.
 

SAP HANA azure'da (büyük örnekler) satın, arasında Microsoft Kurumsal hesap ekibi sonlandırılır sonra aşağıdaki bilgileri HANA büyük örneği birimleri dağıtmak için Microsoft tarafından gereklidir:

- Müşteri adı
- İş kişi bilgileri (e-posta adresi ve telefon numarası dahil)
- Teknik kişi bilgileri (e-posta adresi ve telefon numarası dahil)
- Teknik ağ kişi bilgileri (e-posta adresi ve telefon numarası dahil)
- Azure dağıtım bölge (Batı ABD, Doğu ABD, Avustralya Doğu, Avustralya Güneydoğu, Batı Avrupa ve Kuzey Avrupa Temmuz 2017 itibariyle)
- SAP HANA Azure (büyük örnekler) SKU (yapılandırma) üzerinde onaylayın
- Zaten genel bakış ve mimari belgede için dağıtılan her Azure bölgesinin HANA büyük örnekleri için ayrıntılı:
    - / 29 HANA büyük örneklerine Azure sanal ağları bağlama ER P2P bağlantıları için IP adresi aralığı
    - Bir/24 CIDR bloğu HANA büyük örnekleri sunucu IP havuzu için kullanılan
- HANA büyük örneklerine bağlanan her Azure vnet'in VNet adres alanı özniteliğinde kullanılan IP adres aralığı değerleri
- Verileri her HANA büyük örneklerinin sistem için:
  - İstenen ana bilgisayar - tam etki alanı adı ile idealdir.
  - Sunucu IP havuzu adres aralığı dışında-HANA büyük örneği biriminin istediğiniz IP adresini kullanmaya devam aklınızda sunucu IP havuzu adres aralığı ilk 30 IP adreslerinin HANA büyük örnekleri içinde iç kullanım için ayrılmıştır
  - SAP HANA SID adı (gerekli SAP HANA ilgili disk birimleri oluşturmak için gerekli) SAP HANA örneği için. HANA SID HANA büyük örneği birimine bağlı NFS birimlerde sidadm izinlerini oluşturmak için gereklidir. Ayrıca takılı disk birimi adı bileşenlerden biri olarak kullanılır. Biriminde birden fazla HANA örneği çalıştırmak istiyorsanız, birden çok HANA SID listesi gerekir. Her biri, atanan birimleri ayrı bir kümesini alır.
  - Linux işletim sisteminde sidadm kullanıcının sahip GroupID gerekli SAP HANA ilgili disk birimleri oluşturmak için gereklidir. SAP HANA yükleme genellikle 1001 Grup kimliğine sahip sapsys grubu oluşturur. Sidadm kullanıcı bu grubun parçası olan
  - Linux işletim sisteminde sidadm kullanıcının sahip UserID gerekli SAP HANA ilgili disk birimleri oluşturmak için gereklidir. Tüm liste gerek HANA birden çok birim üzerinde çalıştırıyorsanız, <sid>adm kullanıcılar 
- Hangi SAP HANA Azure HANA üzerinde büyük örnekleri doğrudan bağlı olacak Azure aboneliği için Azure abonelik kimliği. Bu abonelik kimliği ile HANA büyük örneği birimlerinin uygulanacak giderek Azure aboneliği başvurur.

Bilgileri verdikten sonra Microsoft azure'da (büyük örnekler) SAP HANA sağlar ve Azure Vnet'ler HANA büyük örneklerine bağlamak için ve HANA büyük örneği birimleri erişmek için gerekli bilgileri döndürür.

## <a name="connecting-azure-vms-to-hana-large-instances"></a>Azure VM'ler HANA büyük örneklerine bağlanma

Zaten belirtilen [SAP HANA (büyük örnekler) genel bakış ve Azure ile ilgili mimari](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-overview-architecture) Azure görünüyor SAP uygulama katmanında en az dağıtımla HANA büyük örneklerinin ister:

![Azure sanal ağı Azure (büyük örnekler) ve şirket içi SAP HANA bağlı](./media/hana-overview-architecture/image3-on-premises-infrastructure.png)

Azure VNet tarafında yakın baktığınızda, biz gereksinimini unutmayın:

- SAP uygulama katmana sanal makineleri dağıtmak için kullanılacak bir Azure VNet tanımı.
- Otomatik olarak bir varsayılan alt Azure sanal ağında anlamına gelir, tanımladığınız gerçekten içine sanal makineleri dağıtmak için kullanılan adrestir.
- En az bir VM alt ağı ve bir ExpressRoute ağ geçidi alt ağı oluşturulan Azure VNet olmalıdır. Bu alt ağlar aşağıdaki bölümlerde, belirtilen ve ele olarak IP adres aralıklarını atanması gerekir.

Bu nedenle, HANA büyük örnekleri için Azure VNet oluşturma içine biraz daha yakın bir göz atalım

### <a name="creating-the-azure-vnet-for-hana-large-instances"></a>HANA büyük örnekleri için Azure VNet oluşturma

>[!Note]
>Azure VNet HANA büyük örneği için Azure Resource Manager dağıtım modeli kullanılarak oluşturulmuş olması gerekir. Yaygın olarak Klasik dağıtım modeli olarak bilinen eski Azure dağıtım modeli HANA büyük örneği çözümüyle desteklenmiyor.

Azure portalı, PowerShell, Azure şablonu veya Azure CLI kullanarak VNet oluşturulabilir (bkz [Azure portalını kullanarak bir sanal ağ oluşturma](../../../virtual-network/manage-virtual-network.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json#create-a-virtual-network)). Aşağıdaki örnekte, biz Azure portalından oluşturulan bir VNet içine bakın.

Biz Azure Portalı aracılığıyla Azure VNet tanımlarını içine bakarsanız, bazı tanımları ve ne olanlar biz listelemek için ilişkili uygulamasına göz atalım farklı IP adresi aralıkları. Biz hakkında konuşurken gibi **adres alanı**, Azure VNet kullanmasına izin verilen adres alanı demek isteriz. Bu adres alanı de BGP rota yayılması için sanal ağ kullanan adres aralığıdır. Bu **adres alanı** burada görebilirsiniz:

![Azure VNet, adres alanı Azure portalında gösterilen](./media/hana-overview-connectivity/image1-azure-vnet-address-space.png)

10.16.0.0/16 ile önceki durumda Azure VNet kullanmak için bir oldukça büyük ve geniş IP adres aralığı verildi. Bu VNet içindeki sonraki alt ağlara tüm IP adres aralıklarını o 'adres alanı' içinde kendi aralıkları gerektiği anlamına gelir. Genellikle bu tür bir büyük adres aralığı azure'da tek VNet için öneriyoruz değil. Ancak, bir adım ileri alma, Azure VNet içinde tanımlanan alt ağları içine bakalım:

![Azure sanal ağ alt ağları ve kendi IP adres aralıkları](./media/hana-overview-connectivity/image2b-vnet-subnets.png)

Gördüğünüz gibi (burada 'varsayılan' olarak da adlandırılır) ilk VM alt ağı ve 'GatewaySubnet' adlı bir alt ağ ile biz VNet bakın.
Aşağıdaki bölümde, biz 'varsayılan' grafik çağrıldı alt ağın IP adresi aralığı başvurmak **Azure VM alt ağ IP adresi aralığı**. Aşağıdaki bölümlerde, biz ağ geçidi alt ağı IP adres aralığı başvurmak **VNet ağ geçidi alt ağını IP adresi aralığı**. 

Yukarıdaki iki grafik tarafından gösterilen durumda, gördüğünüz **VNet adres alanı** her ikisini de kapsayan **Azure VM alt ağ IP adresi aralığı** ve **VNet ağ geçidi alt ağını IP adresi aralığı**. 

Gereken korumak veya IP adres aralığı ile belirli diğer durumlarda, kısıtlayabilirsiniz **VNet adres alanı** her alt ağı tarafından kullanılan belirli aralıklara vnet'in. Bu durumda, tanımlayabilirsiniz **VNet adres alanı** bir VNet gibi birden çok özel aralıkları aşağıda gösterildiği gibi:

![İki alanları ile Azure VNet adres alanı](./media/hana-overview-connectivity/image3-azure-vnet-address-space_alternate.png)

Bu durumda, **VNet adres alanı** tanımlanan iki boşluk içeriyor. Bu iki alanları için tanımlanan IP adres aralıkları için aynı **Azure VM alt ağ IP adresi aralığı** ve **VNet ağ geçidi alt ağını IP adresi aralığı.**

İstediğiniz herhangi bir adlandırma standart bu Kiracı alt ağları (VM ağları) için kullanabilirsiniz. Ancak, **ayrıca bir ve sadece bir ağ geçidi alt ağı her VNet için her zaman olmalıdır** (büyük örnekler) Azure expressroute bağlantı hattı üzerinde SAP HANA bağlanan. Ve **bu ağ geçidi alt ağı her zaman "GatewaySubnet" olarak adlandırılmalıdır** ExpressRoute ağ geçidi'nin uygun yerleştirme emin olmak için.

> [!WARNING] 
> Ağ geçidi alt ağı her zaman "GatewaySubnet." adlı önemlidir

Bitişik olmayan adres aralıklarını bile kullanan birden çok VM alt ağları kullanılabilir. Ancak daha önce belirtildiği gibi bu adres aralıklarını tarafından ele alınması gereken **VNet adres alanı** vnet'in toplu halde veya VM alt ağları ve ağ geçidi alt ağı tam aralıkları listesi.

HANA büyük örneklerine bağlanan bir Azure sanal hakkında önemli olgu özetleme:

- Microsoft'a göndermek gereken **VNet adres alanı** HANA büyük örnekleri ilk dağıtımda gerçekleştirirken. 
- **VNet adres alanı** Azure VM alt ağ IP adresi aralıklarının ve sanal ağ geçidi alt ağı IP adresi aralığı aralığını kapsayan tek bir büyük aralık olabilir.
- Veya olarak gönderebilirsiniz **VNet adres alanı** farklı IP kapak birden çok aralık adres aralıklarını VM alt ağ IP adresi aralıklarının ve sanal ağ geçidi alt ağı IP adresi aralığı.
- Tanımlı **VNet adres alanı** kullanılan BGP yönlendirme yayma değil.
- Ağ geçidi alt ağı adı olması gerekir: **"GatewaySubnet."**
- **VNet adres alanı** HANA büyük örneği tarafında bir filtre olarak izin vermek veya Azure'dan HANA büyük örneği birimleri trafiği engellemek için kullanılır. Azure VNet HANA büyük örneği tarafında filtreleme için yapılandırılmış IP adresi aralıkları ve BGP yönlendirme bilgileri eşleşmiyorsa, bağlantı sorunları ortaya çıkabilir.
- Ağ geçidi alt ağı ilgili olan bazı ayrıntılar aşağıya 'HANA büyük örneği ExpressRoute için bir sanal ağa bağlanma' bölümünde ele alınan vardır



### <a name="different-ip-address-ranges-to-be-defined"></a>Tanımlanacak farklı IP adresi aralıkları 

Biz zaten HANA büyük örnekleri önceki bölümlerde dağıtmak için gereken IP adresi aralıklarını bazıları sunmuştur. Ancak önemli bazı daha fazla IP adres aralıklarını vardır. Bazı daha ayrıntılı bilgi edelim. Tüm Microsoft'a gönderilen gerek aşağıdaki IP adresleri, ilk dağıtım için bir istek göndermeden önce tanımlanmış olması gerekir:

- **VNet adres alanı:** zaten daha önce sunulan, veya IP adresi range(s) atamış (veya atamak için plan) Azure sanal ağ (VNet) SAP HANA büyük örneği ortama bağlanma adresi alanı parametre olarak. Bu adres alanı parametre grafikleri daha önce gösterildiği gibi Azure VM alt ağı aralıklarının ve Azure ağ geçidi alt ağ aralığı oluşan bir çok satırlı değer olduğunu önerilir. Bu aralık, şirket içi veya sunucu IP havuzu veya ER P2P adres aralıkları ile çakışmaması gerekir. Bu veya bu IP adresi aralıklarının almak nasıl? Şirket ağı takım ya da hizmet sağlayıcısı, bir veya birden çok IP adresi veya, ağınızda kullanılmaz aralıklarının, sağlamanız gerekir. Örnek: 10.0.1.0/24 (önceki bakın), Azure VM alt ağı olan ve 10.0.2.0/28 (aşağıdaki bakın) Azure ağ geçidi alt ağınızı ise, ardından Azure VNet adres alanı iki satır olması önerilir; 10.0.1.0/24 ve 10.0.2.0/28. Adres alanı değerleri kümelenebilir rağmen ağınızda büyük adres alanları ileride başka bir yerde içinde kullanılmayan IP adres aralığı yanlışlıkla kullanılmasını önlemek için alt ağ aralıklarını eşleşen önerilir. **VNET adres alanı olan bir ilk dağıtımınızı isterken Microsoft'a gönderilmesi gereken bir IP adresi aralığı**

- **Azure VM alt ağ IP adresi aralığı:** bu IP adresi aralığı, zaten daha önce bahsedildiği gibi etmektir atamış bir (veya atamak için plan) Azure SAP HANA büyük örneği ortama bağlanma ağınızı Azure sanal alt ağ parametresinde. Bu IP adresi aralığı, Azure VM'ler için IP adresleri atamak için kullanılır. Bu aralık dışında IP adresleri, SAP HANA büyük örneği sunucusuna/sunucularına bağlanmak için izin verilir. Gerekirse, birden çok Azure VM alt ağı kullanılabilir. A/24 CIDR bloğu, Microsoft tarafından her Azure VM alt ağı için önerilir. Bu adres aralığı Azure VNet adres alanı kullanılan değerleri bir parçası olması gerekir. Bu IP adresi aralığı almak nasıl? Şirket ağı takım ya da hizmet sağlayıcısı, ağınızda kullanılan olmayan bir IP adresi aralığı sağlamalıdır.

- **Sanal ağ geçidi alt ağını IP adresi aralığı:** kullanmayı düşünüyorsanız özelliklerine bağlı olarak, önerilen boyut olacaktır:
   - Çok yüksek performanslı ExpressRoute ağ geçidi: / 26 adres bloğu - SKU'ları türü II sınıfı için gerekli
   - VPN ve yüksek performanslı ExpressRoute ağ geçidi kullanarak ExpressRoute ile birlikte bulunma (veya daha küçük): / 27 adres bloğu
   - Diğer durumlarda: / 28 adres bloğu. Bu adres aralığı "VNet adres alanı" değerler kullanılan değerleri bir parçası olması gerekir. Bu adres aralığı Microsoft'a göndermek için gereken Azure VNet adres alanı değerleri kullanılan değerleri bir parçası olması gerekir. Bu IP adresi aralığı almak nasıl? Şirket ağı takım ya da hizmet sağlayıcısı, ağınızda kullanılan olmayan bir IP adresi aralığı sağlamalıdır. 

- **Adres aralığı ER P2P bağlantısını:** bu aralıktaki IP SAP HANA büyük örneği ExpressRoute (ER) P2P bağlantınızı arasındadır. Bu IP adresi aralığı/29 olmalıdır CIDR IP adresi aralığı. Bu aralık, şirket içi veya diğer Azure IP adres aralıklarını çakışmaması gerekir. Bu IP adresi aralığı, Azure sanal ağ ExpressRoute ağ geçidi SAP HANA büyük örnek sunucuları ile arasındaki ER bağlantı kurmak için kullanılır. Bu IP adresi aralığı almak nasıl? Şirket ağı takım ya da hizmet sağlayıcısı, ağınızda kullanılan olmayan bir IP adresi aralığı sağlamalıdır. **Bu aralığı ilk bir dağıtım için isterken Microsoft'a gönderilmesi gereken bir IP adresi aralığı**
  
- **Sunucu IP havuzu adres aralığı:** bu IP adresi aralığı HANA büyük örnek sunucuları tek tek IP adresi atamak için kullanılır. Bir/24 önerilen alt boyutudur CIDR engelle -, ancak olabilir daha küçük bir en düşük düzeyde 64 IP adresleri sağlama. Bu aralıktan, ilk 30 IP adresleri Microsoft tarafından kullanılmak üzere ayrılmış. Bu olgu için bir aralığı boyutu seçerken eklendiğinden emin olun. Bu aralık, şirket içi veya diğer Azure IP adresleri çakışmaması gerekir. Bu IP adresi aralığı almak nasıl? Şirket ağı takım ya da hizmet sağlayıcısı, ağınızda kullanılan olmayan bir IP adresi aralığı sağlamalıdır. Bir/24, SAP HANA azure'da (büyük örnekler) için gereken belirli IP adresleri atamak için kullanılacak (benzersiz CIDR önerilir) engelleyin. **Bu aralığı ilk bir dağıtım için isterken Microsoft'a gönderilmesi gereken bir IP adresi aralığı**
 
Yukarıdaki IP adres aralıklarını planlama ve tanımlamak gereken ancak bunları tüm Microsoft'a iletilmesi gerekir. Yukarıdaki özetlemek için Microsoft'a adı için gereken IP adresi aralıklarını şunlardır:

- Azure sanal ağ adres alanları
- ER P2P bağlantı adres aralığı
- Sunucu IP havuzu adres aralığı

HANA büyük örneklerine bağlanmak için gereken ek sanal ağlar ekleme, yeni Azure VNet adres Microsoft'a eklediğiniz alanı göndermek gerektirir. 

Yapılandırma ve sonunda Microsoft'a sağlamak gerek duyduğunuz farklı aralıkları ve bazı örnek aralıkları örneği aşağıdadır. Gördüğünüz gibi Azure VNet adres alanı için değer ilk örnekte toplanmaz ancak ilk Azure VM alt ağ IP adresi aralığı ve sanal ağ geçidi alt ağı IP adresi aralığı aralığından tanımlanmadı. Azure VNet içinde birden çok VM alt ağı kullanmak işe göre uygun şekilde yapılandırarak ve gönderme ek IP adresi aralıkları ek VM alt ağı, Azure VNet adres alanı bir parçası olarak.

![SAP HANA Azure (büyük örnekler) en az dağıtımı için gerekli IP adresi aralıkları](./media/hana-overview-connectivity/image4b-ip-addres-ranges-necessary.png)

Ayrıca Microsoft'a göndermek verileri toplama olanağına sahip olursunuz. Bu durumda, Azure sanal adres alanının yalnızca bir boşluk içerir. Önceki örnekte kullanılan IP adresi aralıkları kullanarak. Bu toplu VNet adres alanı gibi görünebilir:

![SAP HANA Azure (büyük örnekler) en az dağıtımı için gerekli IP adresi aralıklarını ikinci olasılığı](./media/hana-overview-connectivity/image5b-ip-addres-ranges-necessary-one-value.png)

Azure sanal adres alanının tanımlanan iki küçük aralıkları yerine yukarıda gördüğünüz gibi 4096 IP adreslerini kapsayan tek bir büyük aralık sunuyoruz. Adres alanının büyük tanımı bazı yerine büyük aralıkları kullanılmayan bırakır. VNet adres alanı değerler BGP rota yayılması için kullanıldığından, kullanım kullanılmayan aralıkları şirket içi veya başka bir yerde, ağınızdaki yönlendirme sorunlara neden olabilir. Bu nedenle, sıkı bir şekilde kullanılan gerçek alt ağ adres alanıyla hizalı adres alanı tutmak için önerilir. Kapalı kalma süresi VNet üzerinde yansıtılmasını olmadan gerekirse, her zaman yeni adres alanı değerleri daha sonra ekleyebilirsiniz.
 
> [!IMPORTANT] 
> Her IP adresi aralığı, ER-P2P, sunucu IP havuzuna, Azure VNet adres alanı gerekir **değil** birbirleri ile üst üste veya başka bir aralığı ağınızdaki başka bir yerde kullanılan; her ayrık olmalıdır ve iki grafik olarak önceki Göster olmayabilir bir alt ağdan başka bir aralığı. Aralıkları arasında çakışmalar meydana gelirse, expressroute bağlantı hattı için Azure VNet bağlanamayabilir.

### <a name="next-steps-after-address-ranges-have-been-defined"></a>Adres aralıklarını tanımlanan sonraki adımlar
IP adres aralıklarını tanımlandıktan sonra aşağıdaki etkinliklerin gerçekleşmesi gerekir:

1. IP adres aralıkları, belgenin başında listelenen diğer verileri birlikte Azure VNet adres alanı, ER P2P bağlantısını ve sunucu IP havuzu adres aralığı, gönderin. Bu anda, sanal ağ ve VM alt ağları oluşturmak de başlayabilirsiniz. 
2. Hızlı rota devresi, Azure aboneliğinizin ve HANA büyük örneği damga arasında Microsoft tarafından oluşturulur.
3. Bir kiracı ağ üzerinde büyük örneği damga Microsoft tarafından oluşturulur.
4. Microsoft Azure VNet adres iletişim kuran alanınızı HANA büyük örneğiyle IP adreslerinden kabul etmek için SAP HANA (büyük örnekler) Azure altyapı ağı yapılandırır.
5. Belirli SAP HANA satın alınan Azure (büyük örnekler) SKU üzerinde bağlı olarak bir işlem birimi bir kiracı ağı içinde Microsoft atar, ayırmak ve depolama bağlayın ve (SUSE veya Red Hat Linux) işletim sistemini yükleyin. Bu birimleri için IP adresleri için Microsoft sunucu IP havuzu adres aralığı gönderdiğiniz dışında alınır.

Dağıtım işlemi sonunda, Microsoft aşağıdaki veriler, sunar:
- Azure sanal ağlar HANA büyük örneklerine bağlayan expressroute bağlantı hattı için Azure VNet(s) bağlanmak için gereken bilgileri:
     - Yetkilendirme anahtarları
     - ExpressRoute PeerID
- Expressroute bağlantı hattı ve Azure VNet belirttikten sonra HANA büyük örneklere erişmek için verileri.

Belgede HANA büyük örnekleri bağlayan dizisi bulabileceğiniz [SAP HANA büyük örnekleri için uçtan uca Kurulum](https://azure.microsoft.com/resources/sap-hana-on-azure-large-instances-setup/). Çoğu aşağıdaki adımları bu belgede örnek bir dağıtım gösterilir. 


## <a name="connecting-a-vnet-to-hana-large-instance-expressroute"></a>HANA büyük örneği ExpressRoute için bir sanal ağa bağlanma

Tüm IP adres aralıklarını tanımlanan ve şimdi verileri Microsoft'tan aldım gibi önce oluşturulan VNet HANA büyük örneklerine bağlanma başlatabilirsiniz. Azure Vnet'iniz oluşturulduktan sonra bir ExpressRoute ağ geçidi müşteri Kiracı büyük örneği damga üzerinde bağlandığı expressroute bağlantı hattına bir VNet bağlama VNet üzerinde oluşturulmuş olması gerekir.

> [!NOTE] 
> Bu adım, yeni ağ geçidine atanmış Azure aboneliği için oluşturulan ve belirtilen Azure VNet bağlı olarak tamamlanması 30 dakika sürebilir.

Bir ağ geçidi zaten varsa, onu bir ExpressRoute ağ geçidi olup olmadığını denetleyin. Değilse, ağ geçidi silinmiş ve bir ExpressRoute ağ geçidi yeniden. Şirket içi bağlantısı için expressroute bağlantı hattı için Azure VNet zaten bağlı bu yana bir ExpressRoute ağ geçidi zaten, kurulamazsa, sanal ağlara bağlama bölümüne geçin.

- Kullanın ya da (yeni) [Azure portal](https://portal.azure.com/), veya bir ExpressRoute VPN ağ geçidi oluşturmak için PowerShell Vnet'iniz bağlı.
  - Azure portalını kullanıyorsanız, yeni bir ekleyin **sanal ağ geçidi** ve ardından **ExpressRoute** ağ geçidi türü.
  - Bunun yerine PowerShell seçerseniz, ilk indirin ve en son kullanma [Azure PowerShell SDK](https://azure.microsoft.com/downloads/) bir en iyi deneyimi sağlamak için. Aşağıdaki komutlar bir ExpressRoute ağ geçidi oluşturun. Metinleri öncesinde tarafından bir _$_ belirli bilgilerinizi ile güncelleştirilmesi gereken kullanıcı tanımlı değişkenlerdir.

```PowerShell
# These Values should already exist, update to match your environment
$myAzureRegion = "eastus"
$myGroupName = "SAP-East-Coast"
$myVNetName = "VNet01"

# These values are used to create the gateway, update for how you wish the GW components to be named
$myGWName = "VNet01GW"
$myGWConfig = "VNet01GWConfig"
$myGWPIPName = "VNet01GWPIP"
$myGWSku = "HighPerformance" # Supported values for HANA Large Instances are: HighPerformance or UltraPerformance

# These Commands create the Public IP and ExpressRoute Gateway
$vnet = Get-AzureRmVirtualNetwork -Name $myVNetName -ResourceGroupName $myGroupName
$subnet = Get-AzureRmVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -VirtualNetwork $vnet
New-AzureRmPublicIpAddress -Name $myGWPIPName -ResourceGroupName $myGroupName `
-Location $myAzureRegion -AllocationMethod Dynamic
$gwpip = Get-AzureRmPublicIpAddress -Name $myGWPIPName -ResourceGroupName $myGroupName
$gwipconfig = New-AzureRmVirtualNetworkGatewayIpConfig -Name $myGWConfig -SubnetId $subnet.Id `
-PublicIpAddressId $gwpip.Id

New-AzureRmVirtualNetworkGateway -Name $myGWName -ResourceGroupName $myGroupName -Location $myAzureRegion `
-IpConfigurations $gwipconfig -GatewayType ExpressRoute `
-GatewaySku $myGWSku -VpnType PolicyBased -EnableBgp $true
```

Bu örnekte, HighPerformance ağ geçidi SKU'su kullanıldı. Seçenekleriniz, SAP HANA azure'da (büyük örnekler) için desteklenen SKU'ları yalnızca ağ geçidi olarak HighPerformance veya UltraPerformance şunlardır.

> [!IMPORTANT]
> HANA büyük örnekleri için SKU türü II classs, UltraPerformance ağ geçidi SKU'su kullanımını zorunludur.

### <a name="linking-vnets"></a>Sanal ağları bağlama

Bir ExpressRoute ağ geçidi Azure VNet sahiptir, bu bağlantı için oluşturulan Azure (büyük örnekler) expressroute bağlantı hattı üzerinde SAP HANA ExpressRoute ağ geçidine bağlanmak için Microsoft tarafından sağlanan yetkilendirme bilgileri kullanın. Bu adım Azure portal veya PowerShell kullanılarak gerçekleştirilebilir. Portal önerilir ancak PowerShell yönergeleri aşağıdaki gibidir. 

- Her bağlantı için farklı bir AuthGUID kullanarak her sanal ağ geçidi için aşağıdaki komutları yürütün. Komut dosyası aşağıda gösterilen ilk iki girişleri Microsoft tarafından sağlanan bilgileri gelmektedir. Ayrıca, AuthGUID her VNet ve alt ağ geçidi için özeldir. Anlamına gelir, başka bir Azure sanal eklemek istiyorsanız, Azure'da HANA büyük örnekleri bağlanır, expressroute bağlantı hattı için başka bir AuthID edinmeniz gerekir. 

```PowerShell
# Populate with information provided by Microsoft Onboarding team
$PeerID = "/subscriptions/9cb43037-9195-4420-a798-f87681a0e380/resourceGroups/Customer-USE-Circuits/providers/Microsoft.Network/expressRouteCircuits/Customer-USE01"
$AuthGUID = "76d40466-c458-4d14-adcf-3d1b56d1cd61"

# Your ExpressRoute Gateway Information
$myGroupName = "SAP-East-Coast"
$myGWName = "VNet01GW"
$myGWLocation = "East US"

# Define the name for your connection
$myConnectionName = "VNet01GWConnection"

# Create a new connection between the ER Circuit and your Gateway using the Authorization
$gw = Get-AzureRmVirtualNetworkGateway -Name $myGWName -ResourceGroupName $myGroupName
    
New-AzureRmVirtualNetworkGatewayConnection -Name $myConnectionName `
-ResourceGroupName $myGroupName -Location $myGWLocation -VirtualNetworkGateway1 $gw `
-PeerId $PeerID -ConnectionType ExpressRoute -AuthorizationKey $AuthGUID
```

Aboneliğinizle ilişkili birden çok ExpressRoute bağlantı hatları ağ geçidine bağlanmak isterseniz, bu adımı birden çok kez çalıştırmak gerekebilir. Örneğin, VNet şirket içi ağınıza bağlanır expressroute bağlantı hattı aynı sanal ağ geçidine bağlanmak için büyük olasılıkla çağıracaksınız.

## <a name="adding-more-ip-addresses-or-subnets"></a>Daha fazla IP adresleri veya alt ekleme

Azure portalı, PowerShell veya ekleme daha fazla IP adresleri kullanılırken CLI veya alt ağları kullanın.

Bu durumda, yeni IP adresi aralığı yeni aralık olarak yeni bir birleşik aralık oluşturmak yerine VNet adres alanı eklemek için önerilir. Her iki durumda da, bu değişiklik, istemci bağlantısı HANA büyük örneği birimleri için yeni IP adres aralığı dışında izin Microsoft'a göndermek gerekir. Eklenen yeni VNet adres alanı almak için bir Azure destek isteği açabilirsiniz. Onay aldıktan sonra sonraki adımları gerçekleştirin.

Azure portalından ek bir alt ağ oluşturmak için makalesine bakın [Azure portalını kullanarak bir sanal ağ oluşturma](../../../virtual-network/manage-virtual-network.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json#create-a-virtual-network)ve Powershell'den oluşturmak için bkz: [PowerShell kullanarak bir sanal ağ oluşturma](../../../virtual-network/manage-virtual-network.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json#create-a-virtual-network).

## <a name="adding-vnets"></a>Sanal ağlar ekleme

Bir veya daha fazla Azure Vnet başlangıçta bağladıktan sonra Azure (büyük örnekler) üzerinde SAP HANA'ya erişmek ek olanları eklemek isteyebilirsiniz. İlk olarak, bir Azure destek isteği göndermek, bu istekte ikisi de belirli Azure dağıtım ve IP adresi alanı aralıklarının Azure VNet adres alanı tanımlama belirli bilgileri içerir. SAP HANA Azure hizmet yönetimi üzerinde daha sonra ek sanal ağlar ve ExpressRoute bağlanmak için gereken bilgileri sağlar. Her sanal ağ için expressroute bağlantı hattı HANA büyük örneklerine bağlanmak için benzersiz bir yetkilendirme anahtar gerekir.

Yeni Azure VNet eklemek için adımları:

1. Onboarding işleminin ilk adımında tamamına bakın _Azure VNet oluşturma_ bölümü.
2. Tam ekleme işleminde ikinci adım bkz _ağ geçidi alt ağı oluşturma_ bölümü.
3. HANA büyük örneği expressroute bağlantı hattı için ek sanal ağlara bağlanmak için yeni VNet hakkında bilgi içeren bir Azure destek isteği açın ve yeni bir yetkilendirme anahtar isteyin.
4. Bildirim yetkilendirme tamamlandıktan sonra üçüncü tamamlamak için Microsoft tarafından sağlanan yetkilendirme bilgileri adım ekleme işleminde bkz _sanal ağlara bağlama_ bölümü.

## <a name="increasing-expressroute-circuit-bandwidth"></a>ExpressRoute bağlantı hattı bant genişliğini artırma

SAP HANA Azure Hizmet Yönetimi başvurun. SAP HANA (büyük örnekler) Azure expressroute bağlantı hattı üzerinde bant genişliğini artırmak için önerilir, Azure destek isteği oluşturun. (Tek hattı bant genişliğini en fazla 10 GB/sn artış isteğinde bulunabilirsiniz.) İşlem tamamlandıktan sonra sonra bildirim alırsınız; Bu daha yüksek hız azure'da etkinleştirmek ek Eylem gerekmiyor.

## <a name="adding-an-additional-expressroute-circuit"></a>Ek bir expressroute bağlantı hattı ekleme

Ek bir expressroute bağlantı hattı, destek isteği yeni bir expressroute bağlantı hattı oluşturmak için (ve buna bağlanmak için yetkilendirme bilgilerini almak için) Azure yapmak gerektiğini önerilir, Azure Hizmet Yönetimi, SAP HANA başvurun. Gittiği adres alanı, üzerinde kullanılabilir yetkilendirme sağlamak için sırayla SAP HANA Azure hizmet yönetimi için istek yapmadan önce sanal ağlar tanımlanması gerekir.

Yeni bağlantı hattı oluşturup SAP HANA Azure Hizmet Yönetimi Yapılandırma tamamlandıktan sonra devam etmek gereken bildirim bilgilerle alınacaktır. Oluşturma ve yeni VNet bu ek devresine bağlama yukarıda verilen adımları izleyin. Başka bir SAP HANA aynı Azure bölgesinde (büyük örneği) Azure expressroute bağlantı hattı üzerinde zaten bağlı ek bağlantı hattı için Azure sanal ağlara bağlanmak mümkün değildir.

## <a name="deleting-a-subnet"></a>Bir alt ağı silme

Bir sanal alt kaldırmak için Azure portal, PowerShell veya CLI kullanılabilir. Azure sanal ağ IP adresi aralığı/Azure VNet adres alanı bir toplanmış aralığı olduğu durumda, Microsoft ile sizin için hiçbir izleme yoktur. Sanal ağ silinen alt içerir BGP rota adres alanı hala yayılıyor dışında. Azure sanal ağ IP adresi aralığı/Azure VNet adres alanı hangisinin bir silinen alt ağına atanmış birden çok IP adres aralığı olarak tanımlanmışsa, VNet adres alanınızı dışında silmeli ve daha sonra Azure hizmet yönetimi için SAP HANA bildirin. SAP HANA azure'da (büyük örnekler) ile iletişim kurmak için izin verilen aralıklar kaldırın.

Bir alt ağını silmek için bkz: [bir alt ağını silmek](../../../virtual-network/virtual-network-manage-subnet.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json#delete-a-subnet) alt ağları oluşturma hakkında daha fazla bilgi için.

## <a name="deleting-a-vnet"></a>Bir sanal ağı silme

Bir sanal ağı silmek için bkz: [bir sanal ağı silme](../../../virtual-network/manage-virtual-network.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json#delete-a-virtual-network). SAP HANA Azure hizmet yönetimi üzerinde Azure (büyük örnekler) expressroute bağlantı hattı üzerinde SAP HANA üzerinde varolan yetkilerini kaldırır ve Azure sanal ağ IP adresi aralığı/Azure HANA büyük örneğiyle iletişim için VNet adres alanı kaldırın.

VNet kaldırıldıktan sonra kaldırılacak IP adresi alanı aralıklarının sağlamak için bir Azure destek isteği açın.

Her şeyi kaldırılır emin olmak için aşağıdaki öğeleri silin:

- ExpressRoute bağlantısı, sanal ağ geçidi, sanal ağ geçidi genel IP ve sanal ağ

## <a name="deleting-an-expressroute-circuit"></a>Bir expressroute bağlantı hattı silme

Ek bir SAP HANA (büyük örnekler) Azure expressroute bağlantı hattı üzerinde kaldırmak için SAP HANA Azure Hizmet Yönetimi ile bir Azure destek isteği açın ve bağlantı hattı silinip silinmediğini isteyin. Azure aboneliği silebilir veya gerektiği VNet tutun. Ancak, HANA büyük örnekleri expressroute bağlantı hattı ve bağlı sanal ağ geçidi arasındaki bağlantı silmeniz gerekir.

Ayrıca bir VNet kaldırmak istiyorsanız, bir VNet yukarıdaki bölümde silmeye ilişkin yönergeleri izleyin.


