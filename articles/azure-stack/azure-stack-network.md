---
title: Ağ tümleştirme konuları için Azure Stack tümleşik sistemleri | Microsoft Docs
description: Çok düğümlü Azure Stack ile veri merkezi ağ tümleştirmesi planlamak için neler yapabileceğinizi öğrenin.
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/30/2018
ms.author: jeffgilb
ms.reviewer: wamota
ms.lastreviewed: 08/30/2018
ms.openlocfilehash: a4e04bc9071f4f677230d112a7baa708e9e8dc34
ms.sourcegitcommit: 898b2936e3d6d3a8366cfcccc0fccfdb0fc781b4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/30/2019
ms.locfileid: "55243241"
---
# <a name="network-connectivity"></a>Ağ bağlantısı
Bu makalede, Azure Stack mevcut ağ ortamınıza en iyi şekilde tümleştirmek nasıl karar vermenize yardımcı olmak için Azure Stack ağ altyapı bilgileri sağlar. 

> [!NOTE]
> Azure Stack (örneğin, www.bing.com) dış DNS adları çözümlemek için DNS sunucuları, DNS istekleri iletmek üzere vermeniz gerekir. Azure Stack DNS gereksinimleri hakkında daha fazla bilgi için bkz. [Azure Stack'i veri merkezi tümleştirmesi - DNS](azure-stack-integrate-dns.md).

## <a name="physical-network-design"></a>Fiziksel ağ tasarımı
Azure Stack çözümünün çalışmasını ve hizmetlerini desteklemek için dayanıklı ve yüksek kullanılabilirliğe sahip bir fiziksel altyapı gerekir. Yukarı bağlantılar ToR üzerinden kenarlık anahtarlar, SFP + veya SFP28 medya ve 1 GB, 10 GB ve 25 GB hızları için sınırlıdır. Kullanılabilirlik için orijinal ekipman üreticisi (OEM) donanım satıcınıza başvurun. Aşağıdaki diyagramda bizim önerilen tasarımı gösterir:

![Önerilen Azure Stack ağ tasarımı](media/azure-stack-network/recommended-design.png)


## <a name="logical-networks"></a>Mantıksal Ağlar
Mantıksal ağlar temeldeki fiziksel ağ altyapısının bir soyutlamasını temsil eder. Bunlar, konaklar, sanal makineler ve hizmetler için ağ atamalarını düzenlemek ve kolaylaştırmak için kullanılır. Mantıksal ağ oluşturmanın bir parçası, ağ siteleri, sanal yerel ağları (VLAN'lar), IP alt ağları ve her fiziksel konumdaki mantıksal ağ ile ilişkili IP alt ağı/VLAN çiftleri tanımlamak için oluşturulur.

Aşağıdaki tabloda, mantıksal ağlar ve planlamanız gereken ilişkili IPv4 alt ağ aralıklarını gösterilmektedir:

| Mantıksal Ağ | Açıklama | Boyut | 
| -------- | ------------- | ------------ | 
| Genel VIP | Azure Stack 31 adresleri bu ağ üzerinden toplam kullanır. Rest, Kiracı sanal makineler tarafından kullanılır ve sekiz genel IP adresleri Azure Stack'i Hizmetleri küçük bir kümesi için kullanılır. App Service ve SQL kaynak Sağlayıcısı'nı kullanmayı planlıyorsanız, 7 daha fazla adresleri kullanılır. Kalan 15 IP'ler, gelecekte Azure Hizmetleri için ayrılmıştır. | / 26 (62 konakları) - /22 (1022 ana)<br><br>Önerilen /24 (254 ana bilgisayardan) = | 
| Geçiş altyapısı | Noktadan noktaya yönlendirme amacıyla, adanmış IP adresleri yönetim arabirimleri ve anahtara atanmış geri döngü adresi geçin. | /26 | 
| Altyapı | Azure Stack iç bileşenleri için iletişim kurmak için kullanılır. | /24 |
| Özel | Depolama ağı ve özel VIP'ler için kullanılır. | /24 | 
| BMC | Fiziksel konaklardaki bmc ile iletişim kurmak için kullanılır. | /26 | 
| | | |

## <a name="network-infrastructure"></a>Ağ altyapısı
Azure Stack için ağ altyapısını anahtarlar üzerinde yapılandırılmış birkaç mantıksal ağları oluşur. Aşağıdaki diyagramda, bu mantıksal ağları ve nasıl bunların üst raf ile (TOR), temel kart yönetim denetleyicisine (BMC) tümleştirin ve (müşteri ağı) anahtarları kenarlık gösterilmektedir.

![Mantıksal ağ diyagramı ve anahtar bağlantıları](media/azure-stack-network/NetworkDiagram.png)

### <a name="bmc-network"></a>BMC ağ
Bu ağ tüm temel kart yönetim denetleyicileri (olarak da bilinen hizmet işlemcileri, örneğin iDRAC, iLO, iBMC, vb.) bağlanma yönetim ağına ayrılır. Varsa, donanım yaşam döngüsü ana bilgisayar (HLH) Bu ağda bulunan ve OEM-belirli yazılım, donanım bakım ya da izleme için sağlayabilir. 

HLH ayrıca dağıtım VM (DVM) barındırır. DVM Azure Stack dağıtımı sırasında kullanılan ve dağıtım tamamlandıktan sonra kaldırılır. DVM bağlı dağıtım senaryolarında sınamak için doğrulama ve birden çok bileşenlerine erişmek için Internet erişimi gerektirir. Bu bileşenler, içinde ve Şirket ağınızın dışında olabilir. Örneğin, NTP, DNS ve Azure. Bağlantı gereksinimleri hakkında daha fazla bilgi için bkz: [Azure Stack güvenlik duvarı tümleştirmesi NAT bölümünde](azure-stack-firewall.md#network-address-translation). 

### <a name="private-network"></a>Özel ağ
Bu /24 (254 ana bilgisayarı IP'ler) ağ (Azure Stack bölgesinin kenarlık anahtar cihazları genişletmeyen) Azure Stack bölgeye özeldir ve iki alt ağına bölünür:

- **Depolama ağı**. Bir /25 (126 konak IP'ler) ağ alanları doğrudan'ı ve sunucu ileti bloğu (SMB) depolama trafiği ve sanal makine dinamik geçişi desteklemek için kullanılır. 
- **İç sanal IP ağ**. A/25 yazılım yük dengeleyici için yalnızca dahili ayrılmış VIP ağ.

### <a name="azure-stack-infrastructure-network"></a>Azure Stack altyapı ağı
Bu/24 ağ iletişim kurmak ve aralarında veri değişimi iç Azure Stack bileşenleri için ayrılmış. Bu alt ağ yönlendirilebilir IP adreslerinin gerektirir, ancak erişim denetim listeleri (ACL'ler) kullanarak çözüme özel tutulur. Kenarlık anahtarlar için bir/27 boyutu eşdeğer küçük bir aralık dışında ötesinde yönlendirilmesini beklenen değil, dış kaynaklara ve/veya internet erişimi gerektirdiğinde bu hizmetlerden bazıları tarafından kullanılan ağ. 

### <a name="public-infrastructure-network"></a>Ortak altyapı ağı
Bunun/27 ağdır küçük bir aralık daha önce bahsedilen Azure Stack altyapısını alt ağ, genel IP adresleri olması gerekmez, ancak bir NAT veya saydam Ara sunucu üzerinden internet erişimi gerektirmez. Bu ağ, Acil Durum Kurtarma konsolunu sistemini (ERCS) için atanacak, azure'a kayıt sırasında ve altyapı yedekleme sırasında ERCS VM Internet erişimi gerektirir. ERCS VM sorun giderme amacıyla yönetim ağınıza yönlendirilebilir olmalıdır.

### <a name="public-vip-network"></a>Genel VIP ağları
Genel VIP ağları Ağ denetleyicisi Azure Stack'te atanır. Bir mantıksal ağ anahtarı üzerindeki değil. SLB adres havuzu kullanır ve atar/Kiracı İş yükleri için 32 ağlar. Geçiş yönlendirme tablosunda 32 bu IP'lerin BGP aracılığıyla kullanılabilir olan bir yol olarak bildirildiğini. Bu ağa harici erişilebilir veya genel IP adreslerini içerir. Azure Stack altyapısının kalan Vm'leri Kiracı tarafından kullanılırken bu genel VIP ağları ilk 31 adreslerinden saklı tutar. Bu alt ağ boyutu en fazla /22 (1022 ana) en az /26 (64 ana) değişebilir, için bir/24 planlamanızı öneririz ağ.

### <a name="switch-infrastructure-network"></a>Altyapı ağı Değiştir
Bu/26 ağdır yönlendirilebilir noktadan noktaya IP 30 (2 ana bilgisayar IP) alt ve ayrılmış geri döngüler içeren alt ağ/32 alt ağlar için bant dışı yönetimi ile BGP yönlendiricisi kimliği geçiş Bu IP adresi aralığı, veri merkeziniz için Azure Stack çözümün dışarıdan yönlendirilebilen olmalıdır, özel veya genel IP'ler olması olabilir.

### <a name="switch-management-network"></a>Anahtar Yönetim ağı
Bu /29 (6 konak IP'ler) ağ ayrılmış yönetim bağlantı noktalarına anahtarların bağlanma. Dağıtım, yönetim ve sorun giderme için bant dışı erişim sağlar. Yukarıda belirtilen anahtar altyapı ağ üzerinden hesaplanır.

## <a name="publish-azure-stack-services"></a>Azure Stack hizmetleri yayımlama
Azure Stack Hizmetleri kullanıcıların dış Azure yığını kullanılabilmesi gerekir. Azure Stack altyapısını rolleri için çeşitli uç ayarlar. Bu uç noktaları VIP'ler genel IP adresi havuzundan atanır. Dağıtım sırasında belirtilen dış DNS bölgesi içindeki her bir uç nokta için bir DNS girişi oluşturulur. Örneğin, kullanıcı portalı, portal'ın DNS konak girişi atanır.  *&lt;bölge >.&lt; FQDN >*.

### <a name="ports-and-urls"></a>Bağlantı noktaları ve URL'ler
Azure Stack hizmetlerinin yapma (portalları gibi Azure Resource Manager, DNS, vb.) dış ağlara kullanılabilir, bu uç noktalarına gelen trafiği belirli URL'ler, bağlantı noktaları ve protokoller için izin vermeniz gerekir.
 
Bir dağıtımda saydam proxy yukarı bağlantılar burada geleneksel proxy sunucusu için belirli bağlantı noktaları ve URL'ler için izin vermelidir [gelen](https://docs.microsoft.com/azure/azure-stack/azure-stack-integrate-endpoints#ports-and-protocols-inbound) ve [giden](https://docs.microsoft.com/azure/azure-stack/azure-stack-integrate-endpoints#ports-and-urls-outbound) iletişim. Bu bağlantı noktaları ve URL'ler için kimlik, Market, düzeltme eki ve güncelleştirme, kayıt ve kullanım verilerini içerir.

## <a name="next-steps"></a>Sonraki adımlar
[Kenarlık bağlantısı](azure-stack-border-connectivity.md)