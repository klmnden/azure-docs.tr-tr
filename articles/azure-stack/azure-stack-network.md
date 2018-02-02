---
title: "Ağ Azure tümleşik yığını sistemleri için tümleştirme konuları | Microsoft Docs"
description: "Birden çok düğümlü Azure yığını ile veri merkezi ağ tümleştirme planlamak için yapabileceğinizi öğrenin."
services: azure-stack
documentationcenter: 
author: jeffgilb
manager: femila
editor: 
ms.assetid: 
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/31/2018
ms.author: jeffgilb
ms.reviewer: wamota
ms.openlocfilehash: a198ff5fe7135e17301025d6a712236b76be0ede
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/01/2018
---
# <a name="network-connectivity"></a>Ağ bağlantısı
Bu makale Azure yığın mevcut ağ ortamınıza en iyi tümleştirmek nasıl karar vermenize yardımcı olacak Azure yığın ağ altyapı bilgileri sağlar. 

> [!NOTE]
> Dış DNS adlarını (örneğin, www.bing.com) Azure yığından çözümlemek için DNS isteklerini iletmek için DNS sunucuları sağlamanız gerekir. Azure yığın DNS gereksinimleri hakkında daha fazla bilgi için bkz: [Azure yığın datacenter tümleştirmesi - DNS](azure-stack-integrate-dns.md).

## <a name="physical-network-design"></a>Fiziksel ağ tasarımı
Azure yığın çözüm, işlem ve hizmetlerini desteklemek için esnek ve yüksek oranda kullanılabilir fiziksel bir altyapı gerektirir. Aşağıdaki diyagramda, önerilen tasarımı gösterir:

![Önerilen Azure yığın ağ tasarımı](media/azure-stack-network/recommended-design.png)


## <a name="logical-networks"></a>Mantıksal ağlar
Mantıksal ağlar temeldeki fiziksel ağ altyapısının bir soyutlamasını temsil eder. Bunlar, konaklar, sanal makineleri ve Hizmetleri için ağ atamalarını düzenlemek ve kolaylaştırmak için kullanılır. Mantıksal ağ oluşturmanın bir parçası ağ alanları sanal yerel ağlar (VLAN'lar), IP alt ağları ve her fiziksel konumdaki mantıksal ağ ile ilişkili IP alt ağı/VLAN çiftleri tanımlamak için oluşturulur.

Aşağıdaki tabloda, mantıksal ağlar ve planlamanız gerekir ilişkili IPv4 alt ağ aralıklarını gösterilmektedir:

| Mantıksal ağ | Açıklama | Boyut | 
| -------- | ------------- | ------------ | 
| Genel VIP | Azure yığın hizmetleriyle Kiracı sanal makineler tarafından kullanılan rest, küçük bir kümesini ortak IP adresleridir. Azure yığın altyapısı bu ağ 32 adreslerini kullanır. Uygulama hizmeti ve SQL kaynak sağlayıcıları kullanmayı planlıyorsanız, 7 daha fazla adresleri kullanılır. | / 26 (62 konakları) - /22 (1022 ana bilgisayarları)<br><br>Önerilen /24 (254 ana) = | 
| Anahtar altyapısı | Noktadan noktaya IP adreslerini yönlendirme amacıyla, ayrılmış yönetim arabirimleri ve anahtara atanmalıdır geri döngü adresleri geçin. | /26 | 
| Altyapı | Azure yığın iç bileşenleri için iletişim kurmak için kullanılır. | /24 |
| Özel | Depolama ağı ve özel VIP'ler için kullanılır. | /24 | 
| BMC | Fiziksel ana bilgisayarlarda bmc'li iletişim kurmak için kullanılır. | /27 | 
| | | |

## <a name="network-infrastructure"></a>Ağ altyapısı
Azure yığını için ağ altyapısı anahtarlar yapılandırılmış birden fazla mantıksal ağları oluşur. Aşağıdaki diyagramda, bu mantıksal ağları ve bunlar üst üstü ile (TOR), temel kart yönetim denetleyicisi (BMC) tümleştirme ve nasıl (müşteri ağ) anahtarları kenarlık gösterilmektedir.

![Mantıksal ağ bağlantılarının diyagramını ve anahtarı oluştur](media/azure-stack-network/NetworkDiagram.png)

### <a name="bmc-network"></a>BMC ağ
Bu ağ yönetim ağı için tüm temel kart yönetim denetleyicileri (olarak da bilinen Hizmet işlemciler, örneğin, iDRAC, Ilo, iBMC, vb.) bağlanma ayrılır. Varsa, (HLH) donanım yaşam döngüsü ana bu ağ üzerinde bulunan ve donanım Bakım ve/veya izleme için OEM belirli yazılım sağlayabilir. 

### <a name="private-network"></a>Özel ağ
Bu /24 (254 ana bilgisayar IP'ın) ağ (Azure yığın bölgesinin kenarlık anahtar aygıtları genişletmez) Azure yığın bölgeye özeldir ve iki alt ağa ayrılmıştır:

- **Depolama ağı**. Dinamik geçiş (126 ana bilgisayar IP'ın) ağ alanları doğrudan ve sunucu ileti bloğu (SMB) depolama trafiği ve sanal makine kullanımını desteklemek için kullanılan bir /25. 
- **İç sanal IP ağ**. A/25-yalnızca iç atanmış VIP'ler yazılım yük dengeleyici için ağ.

### <a name="azure-stack-infrastructure-network"></a>Azure yığın altyapı ağı
Bu/24 ağ iletişim kurmak ve aralarında veri değişimi iç Azure yığın bileşenleri için ayrılmış. Bu alt ağ olarak yönlendirilebilir IP adreslerinin gerektiriyor, ancak erişim denetim listeleri (ACL'ler) kullanarak çözüme özel tutulur. Kenarlık anahtarlar için/27 boyutu eşdeğer küçük bir aralık dışında ötesinde yönlendirilecek beklenen değil dış kaynaklara ve/veya internet erişimi gerektirdiğinde bu hizmetlerden bazılarını tarafından kullanılan ağ. 

### <a name="public-infrastructure-network"></a>Ortak altyapı ağı
Bu/27 ağdır daha önce bahsedilen Azure yığın altyapı alt ağdan küçük bir aralık, genel IP adresleri gerektirmez, ancak bir NAT veya saydam Proxy üzerinden Internet erişimi gerektirir. Bu ağ Acil Durum Kurtarma Konsolu sistem (ERCS) için ayrılacak, ERCS VM Azure kayıt sırasında Internet erişimi gerektirir ve sorun giderme amacıyla yönetim ağınıza yönlendirilebilir olmalıdır.

### <a name="public-vip-network"></a>Ortak VIP ağ
Ortak VIP ağ Azure yığınında Ağ denetleyicisi atanır. Bir mantıksal ağ anahtarı üzerindeki değil. SLB adres havuzu kullanır ve atar/32 Kiracı İş yükleri için ağları. Geçiş yönlendirme tablosu üzerinde bu 32 IP'leri BGP aracılığıyla kullanılabilir bir yolu olarak bildirildiğini. Bu ağ, dış erişilebilir veya genel IP adresleri içerir. Kalan Kiracı VM'ler tarafından kullanılırken Azure yığın altyapısı bu ortak VIP ağdan en az 8 adres kullanır. Bu alt ağ boyutu en fazla /22 (1022 konakları) için en az /26 (64 konakları) aralığı, bir/24 için planlama öneririz ağ.

### <a name="switch-infrastructure-network"></a>Anahtar altyapı ağı
Bu/26 ağdır (2 ana bilgisayar IP's) yönlendirilebilir noktadan noktaya IP/30 alt ve ayrılmış geri döngüler içeren alt ağ/32 alt ağlar için bant dışı yönetim ve BGP yönlendirici kimliği geçiş Bu IP adresi aralığı dışarıdan merkeziniz için Azure yığın çözümün yönlendirilebilir olmalıdır, özel veya genel IP'ler olabilir.

### <a name="switch-management-network"></a>Anahtar Yönetim ağı
Bu /29 (6 ana bilgisayar IP) ayrılmış ağ anahtarlarını yönetim bağlantı noktalarını bağlanma. Dağıtım, yönetim ve sorun giderme için bant dışı erişim sağlar. Yukarıda belirtilen anahtar altyapı ağ üzerinden hesaplanır.

## <a name="publish-azure-stack-services"></a>Azure yığın Hizmetleri'ne yayımlama
Azure yığın Hizmetleri kullanıcıların dış Azure yığınından kullanılabilmesi gerekir. Azure yığın altyapı rolleri için çeşitli uç noktaları ayarlar. Bu uç noktalar VIP'ler ortak IP adresi havuzundan atanır. Dağıtım sırasında belirtilen dış DNS bölge içindeki her bir uç nokta için bir DNS girişi oluşturulur. Örneğin, kullanıcı portalı Portalı'nın DNS ana bilgisayar girişi atanır.  *&lt;bölge >.&lt; FQDN >*.

### <a name="ports-and-urls"></a>Bağlantı noktaları ve URL'leri
Azure yığın Hizmetleri yapma (portalları gibi Azure Resource Manager, DNS, vb.) kullanılabilir dış ağlara, gelen trafiğe Bu uç noktalar için belirli URL'leri, bağlantı noktalarını ve protokolleri izin vermeniz gerekir.
 
Bir dağıtımda saydam proxy yukarı bağlantılar burada geleneksel proxy sunucusu belirli bağlantı noktalarını ve URL'leri her ikisi için izin vermelidir [gelen](https://docs.microsoft.com/azure/azure-stack/azure-stack-integrate-endpoints#ports-and-protocols-inbound) ve [giden](https://docs.microsoft.com/azure/azure-stack/azure-stack-integrate-endpoints#ports-and-urls-outbound) iletişim. Bu bağlantı noktaları ve URL'leri kimlik, Market dağıtım, düzeltme ve güncelleştirme, kayıt ve kullanım verilerini içerir.

## <a name="next-steps"></a>Sonraki adımlar
[Kenarlık bağlantısı](azure-stack-border-connectivity.md)