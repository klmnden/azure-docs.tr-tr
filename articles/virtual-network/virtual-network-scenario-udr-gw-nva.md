---
title: 2 katmanlı uygulama ile karma bağlantı | Microsoft Docs
description: Sanal gereçler ve Azure'da çok katmanlı uygulama ortamı oluşturmak için UDR dağıtmayı öğrenin
services: virtual-network
documentationcenter: na
author: jimdial
manager: carmonm
editor: tysonn
ms.assetid: 1f509bec-bdd1-470d-8aa4-3cf2bb7f6134
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/05/2016
ms.author: jdial
ms.openlocfilehash: 544ba6484b23da425d53594622122b1e18b92359
ms.sourcegitcommit: e5355615d11d69fc8d3101ca97067b3ebb3a45ef
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/31/2017
ms.locfileid: "23943227"
---
# <a name="virtual-appliance-scenario"></a>Sanal gereç senaryosu
Yaygın bir senaryo büyük Azure müşteri arasında Internet'e erişim geri katmanı için bir şirket içi veri merkezinden verirken açık iki katmanlı bir uygulama sağlamanız gerekiyor. Bu belge aşağıdaki gereksinimleri karşılayan iki katmanlı bir ortam dağıtmak için kullanıcı tanımlı yolları (UDR), bir VPN ağ geçidi ve ağ sanal Gereçleri kullanma bir senaryo yol gösterecek:

* Web uygulaması yalnızca ortak Internet üzerinden erişilebilir olması gerekir.
* Uygulamayı barındıran web sunucusunun bir arka uç uygulama sunucusu erişebilir olması gerekir.
* Web uygulaması Internet'ten gelen tüm trafiği bir güvenlik duvarı sanal gereç aracılığıyla gitmeniz gerekir. Bu sanal gereç yalnızca Internet trafiği için kullanılır.
* Uygulama sunucusuna giden tüm trafiği bir güvenlik duvarı sanal gereç aracılığıyla gitmeniz gerekir. Bu sanal gereç, arka uç sunucu erişimi ve VPN ağ geçidi şirket içi ağdan gelen erişim için kullanılır.
* Yöneticiler, şirket içi bilgisayarlarından güvenlik duvarı sanal gereçler yönetebilmek için, üçüncü bir güvenlik duvarı kullanarak özel yönetim amacıyla kullanılan sanal gereç.

Bu standart DMZ DMZ ve korumalı ağ ile bir senaryodur. Bu senaryo, Azure'da Nsg'ler, güvenlik duvarı sanal gereçler veya her ikisinin birleşimini kullanarak oluşturulabilir. Aşağıdaki tablo bazı Artıları ve eksileri Nsg'ler ve güvenlik duvarı sanal gereçler arasında gösterir.

|  | Uzmanları | Simgeler |
| --- | --- | --- |
| NSG |Ücret ödemeden. <br/>Azure RBAC ile tümleşik. <br/>ARM şablonlarını kuralları oluşturulabilir. |Karmaşıklık büyük ortamlarda değişiklik yapacaktır. |
| Güvenlik duvarı |Veri düzlemi üzerinde tam denetim. <br/>Güvenlik Duvarı konsolunu aracılığıyla merkezi yönetimi. |Güvenlik Duvarı gerecini maliyeti. <br/>Azure RBAC ile tümleşik değildir. |

Çözüm DMZ ve korumalı ağ senaryosunu uygulamak için güvenlik duvarı sanal gereçler kullanır.

## <a name="considerations"></a>Dikkat edilmesi gerekenler
Bugün, aşağıdaki gibi farklı özellikler kullanılabilir kullanarak Azure'da yukarıda açıklanan ortamı dağıtabilirsiniz.

* **Sanal ağ (VNet)**. Bir Azure sanal bir şirket içi ağ ile benzer şekilde davranır ve trafik yalıtımı ve sorunları ayrılması sağlamak için bir veya daha fazla alt bölümlenmiş.
* **Sanal gereç**. Birkaç iş ortakları, yukarıda açıklanan üç güvenlik duvarları için kullanılan Azure markette sanal gereçler sağlar. 
* **Kullanıcı tanımlı yolları (UDR)**. Yönlendirme tabloları bir sanal ağ içindeki paketlerin akışı denetlemek için Azure ağ tarafından kullanılan Udr'ler içerebilir. Bu yol tablolarını alt ağlara uygulanabilir. Azure'da yeni özellikler de Azure VNet içinde bir sanal gereç karma bağlantı'dan gelen tüm trafiği iletme yeteneği sağlamak GatewaySubnet, bir yol tablosu uygulamak yeteneğidir.
* **IP iletimi**. Varsayılan olarak, Azure ağ altyapısı iletin sanal ağ arabirimi kartlarıyla (NIC) paketleri yalnızca paketin hedef IP adresi NIC IP adresini eşleşiyorsa. Bu nedenle, bir UDR bir paket için belirli bir sanal gereç gönderilmelidir tanımlıyorsa, Azure ağ altyapısı o paketin bırakın. Paketin paket için gerçek hedef değil bir VM (Bu durumda bir sanal gereç) teslim emin olmak için IP iletimi sanal Gereci için etkinleştirmeniz gerekir.
* **Ağ güvenlik grupları (Nsg'ler)**. Aşağıdaki örnek Nsg'ler kullanmak yapmaz, ancak daha fazla giden trafiği filtrelemek bu alt ağ ve NIC'ler filtrelemek için Nsg'ler alt ağları ve/veya NIC bu çözümde uygulanan kullanabilirsiniz.

![IPv6 bağlantısı](./media/virtual-network-scenario-udr-gw-nva/figure01.png)

Bu örnekte, aşağıdakileri içeren bir abonelik vardır:

* şemada gösterilmez 2 kaynak grubu. 
  * **ONPREMRG**. Bir şirket içi ağ benzetimini yapmak gerekli tüm kaynaklar içeriyor.
  * **AZURERG**. Azure sanal ağ ortamınız için gerekli tüm kaynaklar içeriyor. 
* Adlı bir VNet **onpremvnet** aşağıda listelenen bölümlenmiş bir şirket içi veri merkezi taklit etmek üzere kullanılır.
  * **onpremsn1**. Bir şirket içi sunucu taklit etmek üzere Ubuntu çalıştıran sanal makine (VM) içeren bir alt ağ.
  * **onpremsn2**. Bir yönetici tarafından kullanılan bir şirket içi bilgisayarın taklit etmek üzere Ubuntu çalıştıran bir VM'de içeren alt ağ.
* Adlı bir güvenlik duvarı sanal Gereci yoktur **OPFW** üzerinde **onpremvnet** tünel korumak için kullanılan **azurevnet**.
* Adlı bir VNet **azurevnet** aşağıda listelenen bölümlenmiş.
  * **azsn1**. Dış güvenlik duvarı alt ağı yalnızca dış güvenlik duvarı için kullanılır. Tüm Internet trafiğini bu alt ağ ile birlikte gelir. Bu alt ağ yalnızca dış Güvenlik Duvarı'na bağlı bir NIC içerir.
  * **azsn2**. Internet üzerinden erişilen bir web sunucusu olarak çalışan bir VM barındıran ön uç alt.
  * **azsn3**. Arka uç alt ağ ön uç web sunucusu tarafından erişilecek bir arka uç uygulama sunucusu çalıştıran bir VM'de barındırma.
  * **azsn4**. Yönetim alt ağı yalnızca tüm güvenlik duvarı sanal gereçler yönetim erişim sağlamak için kullanılır. Bu alt ağ yalnızca çözümde kullanılan her güvenlik duvarı sanal Gereci için bir NIC içerir.
  * **GatewaySubnet**. Azure karma bağlantı alt ağı ExpressRoute ve VPN ağ geçidi için Azure sanal ağlar ve diğer ağlar arasında bağlantı sağlamak için gereklidir. 
* 3 güvenlik duvarı sanal gereçler içinde vardır **azurevnet** ağ. 
  * **AZF1**. Azure'da genel IP adresi kaynağı kullanarak genel Internet'e açık dış güvenlik duvarı. Bir şablon marketten ya da doğrudan satıcınızdan gereç, bu hükümleri 3-NIC sanal gereç elinizde emin olmanız gerekir.
  * **AZF2**. Arasındaki trafiği denetlemek için kullanılan iç güvenlik duvarı **azsn2** ve **azsn3**. Bu, 3-NIC sanal gereç de geçerlidir.
  * **AZF3**. Yönetim ve güvenlik duvarı erişilebilir Yöneticiler için şirket içi veri merkezi, tüm güvenlik duvarı Gereçleri yönetmek için kullanılan bir yönetim alt ağına bağlı. 2 NIC sanal gereç şablonları markette bulunamadı veya doğrudan Gereci satıcınızdan isteyin.

## <a name="user-defined-routing-udr"></a>Kullanıcı tanımlı yönlendirme (UDR)
Azure her alt ağ trafiği alt yönlendirildiği nasıl başlatılan tanımlamak için kullanılan bir UDR tabloya bağlanabilir. Hiçbir Udr'ler tanımlanmışsa, Azure bir alt ağdan diğerine akışına izin vermek için varsayılan yolları kullanır. Udr'ler daha iyi anlamak için ziyaret [kullanıcı tanımlı yollar ve IP iletimi nedir](virtual-networks-udr-overview.md).

Yukarıdaki son gereksinimi temel alan doğru güvenlik duvarı Gereci aracılığıyla iletişim yapılır emin olmak için Udr'ler de içeren aşağıdaki yol tablosu oluşturmanız gerekir **azurevnet**.

### <a name="azgwudr"></a>azgwudr
Bu senaryoda, şirket içinden Azure'a yalnızca trafiği bağlanarak güvenlik duvarları yönetmek için kullanılacak **AZF3**, ve trafik iç güvenlik duvarı üzerinden gitmelidir **AZF2**. Bu nedenle, yalnızca bir rota gerekli **GatewaySubnet** aşağıda gösterildiği gibi.

| Hedef | Sonraki atlama | Açıklama |
| --- | --- | --- |
| 10.0.4.0/24 |10.0.3.11 |Yönetimi Güvenlik Duvarı ulaşmak şirket içi trafiğe izin veren **AZF3** |

### <a name="azsn2udr"></a>azsn2udr
| Hedef | Sonraki atlama | Açıklama |
| --- | --- | --- |
| 10.0.3.0/24 |10.0.2.11 |Uygulama sunucusu üzerinden barındırma arka uç alt ağ trafiğine izin verir **AZF2** |
| 0.0.0.0/0 |10.0.2.10 |Aracılığıyla yönlendirilecek tüm trafiğe izin veren **AZF1** |

### <a name="azsn3udr"></a>azsn3udr
| Hedef | Sonraki atlama | Açıklama |
| --- | --- | --- |
| 10.0.2.0/24 |10.0.3.10 |Trafiğine izin verir **azsn2** Web uygulaması sunucusundan akışına için **AZF2** |

Ayrıca alt yol tablolarını oluşturmanıza gerek **onpremvnet** şirket içi veri merkezi taklit etmek üzere.

### <a name="onpremsn1udr"></a>onpremsn1udr
| Hedef | Sonraki atlama | Açıklama |
| --- | --- | --- |
| 192.168.2.0/24 |192.168.1.4 |Trafiğine izin verir **onpremsn2** aracılığıyla **OPFW** |

### <a name="onpremsn2udr"></a>onpremsn2udr
| Hedef | Sonraki atlama | Açıklama |
| --- | --- | --- |
| 10.0.3.0/24 |192.168.2.4 |Azure yedeklenen alt ağ trafiğine izin verir **OPFW** |
| 192.168.1.0/24 |192.168.2.4 |Trafiğine izin verir **onpremsn1** aracılığıyla **OPFW** |

## <a name="ip-forwarding"></a>IP İletimi
UDR ve IP iletimini özelliklerdir kullanabileceğiniz bir Azure sanal ağ trafiğinin akışını denetlemek için kullanılacak sanal gereçler izin vermek için bir arada.  Sanal gereç, güvenlik duvarı veya NAT cihazı gibi ağ trafiğini işlemek için kullanılan bir uygulamayı çalıştıran bir VM'den fazlası değildir.

Bu sanal gereç VM'si, kendisine yönelik olmayan gelen trafiği alabilmelidir. Bir VM'nin başka hedeflere yönelik trafiği alabilmesine izin vermek için VM'de IP İletimini etkinleştirmeniz gerekir. Bu ayar konuk işletim sisteminin değil, Azure'ın bir ayarıdır. Sanal gereç gelen trafiği işlemek ve uygun şekilde yönlendirmek için uygulamanın bazı türünü çalıştırmak hala gerekiyor.

IP iletimi hakkında daha fazla bilgi için [kullanıcı tanımlı yollar ve IP iletimi nedir](virtual-networks-udr-overview.md).

Örnek olarak, bir Azure sanal aşağıdaki Kurulum olduğunu düşünün:

* Alt ağ **onpremsn1** adlı bir VM'den içeren **onpremvm1**.
* Alt ağ **onpremsn2** adlı bir VM'den içeren **onpremvm2**.
* Adlı bir sanal gereç **OPFW** bağlı **onpremsn1** ve **onpremsn2**.
* Bir kullanıcı tanımlı yol bağlı **onpremsn1** tüm için trafiği belirtir **onpremsn2** gönderilmelidir **OPFW**.

AT noktada, varsa **onpremvm1** ile bağlantı kurmaya **onpremvm2**UDR kullanılacak ve trafik gönderilecek **OPFW** sonraki atlama olarak. Gerçek paket hedef değil değiştirilmiş olduğunu aklınızda tutun, hala yazacaktır **onpremvm2** hedefi. 

IP iletme için etkinleştirilmiş olmadan **OPFW**, yalnızca VM'ın IP adresi paket için hedef ise bir VM'ye gönderilmek üzere paketleri verdiğinden Azure sanal ağ mantığı paketleri bırakın.

IP iletimi ile Azure sanal ağı mantığı paketleri OPFW için özgün hedef adresini değiştirmeden iletir. **OPFW** paketlerini işlemek ve bunlarla yapmanız gerekenler belirlemek gerekir.

Bu senaryo için yukarıdaki çalışması için IP iletimi için nıc'lerde etkinleştirmelisiniz **OPFW**, **AZF1**, **AZF2**, ve **AZF3** yönlendirme için kullanılır (yönetimi alt ağa bağlı olanlar dışındaki tüm NIC'ler). 

## <a name="firewall-rules"></a>Güvenlik Duvarı Kuralları
Yukarıda açıklandığı gibi yalnızca IP iletimi sanal gereçler gönderilen paket sağlar. Aygıtınızın hala bu paketleri ile Yapılacaklar karar vermeniz gerekir. Yukarıdaki senaryoda, cihazları aşağıdaki kurallar oluşturmanız gerekir:

### <a name="opfw"></a>OPFW
OPFW aşağıdaki kuralları içeren bir şirket içi aygıtını temsil eder:

* **Rota**: 10.0.0.0/16 giden tüm trafiği (**azurevnet**) tünel üzerinden gönderilen **ONPREMAZURE**.
* **İlke**: arasındaki tüm çift yönlü trafiğine izin **port2** ve **ONPREMAZURE**.

### <a name="azf1"></a>AZF1
AZF1 aşağıdaki kuralları içeren bir Azure sanal Gereci temsil eder:

* **İlke**: arasındaki tüm çift yönlü trafiğine izin **port1** ve **port2**.

### <a name="azf2"></a>AZF2
AZF2 aşağıdaki kuralları içeren bir Azure sanal Gereci temsil eder:

* **Rota**: 10.0.0.0/16 giden tüm trafiği (**onpremvnet**) için Azure ağ geçidi IP adresi (örneğin, 10.0.0.1) üzerinden gönderilmesi gereken **port1**.
* **İlke**: arasındaki tüm çift yönlü trafiğine izin **port1** ve **port2**.

## <a name="network-security-groups-nsgs"></a>Ağ güvenlik grupları (Nsg'ler)
Bu senaryoda, Nsg'ler kullanılmayan. Bununla birlikte, her alt ağa gelen ve giden trafiği kısıtlamak için Nsg'leri uygulayabilirsiniz. Örneğin, aşağıdaki NSG kuralları dış FW alt ağına uygulayabilirsiniz.

**Gelen**

* Tüm TCP trafiğini Internet'ten alt ağdaki tüm VM üzerinde 80 numaralı bağlantı noktasına izin verin.
* Diğer tüm trafik, Internet'ten reddedin.

**Giden**

* Tüm trafik Internet'e reddedin.

## <a name="high-level-steps"></a>Yüksek düzey adımları
Bu senaryoyu dağıtmak için aşağıdaki üst düzey adımları izleyin.

1. Azure aboneliğinizde oturum açın.
2. Şirket içi ağını taklit etmek üzere bir Vnet'i dağıtmak istiyorsanız, parçası olan kaynakları sağlamak **ONPREMRG**.
3. Parçası olan kaynakları sağlamak **AZURERG**.
4. Gelen Tünel sağlamak **onpremvnet** için **azurevnet**.
5. Tüm kaynaklar sağlandıktan sonra oturum **onpremvm2** ve 10.0.3.101 arasındaki bağlantıyı sınamak için ping **onpremsn2** ve **azsn3**.

