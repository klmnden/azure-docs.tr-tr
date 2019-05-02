---
title: Karma bağlantı 2 katmanlı uygulamayla | Microsoft Docs
description: Azure'da çok katmanlı bir uygulama ortamı oluşturmak için UDR ve sanal Gereçleri dağıtma hakkında bilgi edinin
services: virtual-network
documentationcenter: na
author: KumudD
manager: carmonm
editor: tysonn
ms.assetid: 1f509bec-bdd1-470d-8aa4-3cf2bb7f6134
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/05/2016
ms.author: kumud
ms.openlocfilehash: 1bdc485dfb352144e8a8d0fb75965cbb78288e2c
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64575598"
---
# <a name="virtual-appliance-scenario"></a>Sanal gereç senaryosu
Daha büyük bir Azure müşterisi arasında sık karşılaşılan bir senaryodur, Internet'e erişimi geri katmanı için bir şirket içi veri merkezlerinden verirken gösterilen iki katmanlı bir uygulama sağlamak için gerekli değildir. Bu belge aşağıdaki gereksinimleri karşılayan bir iki katmanlı ortamı dağıtmak için kullanıcı tanımlı yollar (UDR), bir VPN ağ geçidi ve ağ sanal Gereçleri kullanarak bir senaryoyu adım yol gösterir:

* Web uygulaması, yalnızca ortak Internet üzerinden erişilebilir olmalıdır.
* Uygulamayı barındıran web sunucusu, bir arka uç uygulama sunucusu erişebilir olmalıdır.
* İnternet'ten gelen tüm trafiği web uygulaması güvenlik duvarı sanal Gereci gitmeniz gerekir. Bu sanal gereç yalnızca Internet trafiği için kullanılır.
* Uygulama sunucusuna giden tüm trafiği bir güvenlik duvarı sanal Gereci gitmeniz gerekir. Bu sanal gereç, arka uç sunucu erişimi ve VPN ağ geçidi aracılığıyla şirket içi ağdan gelen erişim için kullanılır.
* Yöneticiler şirket bilgisayarlarından güvenlik duvarı sanal cihazları yönetebilir, üçüncü bir güvenlik duvarı kullanarak yalnızca yönetim amaçları için kullanılan sanal gereç.

Bu, bir çevre ağı ve korumalı bir ağ standart çevre ağ (Ayrıca DMZ olarak knowns) senaryodur. Bu senaryo, Azure'da Nsg'ler, güvenlik duvarı sanal Gereçleri veya her ikisinin bir birleşimi kullanılarak oluşturulabilir. Aşağıdaki tabloda Artıları ve eksileri Nsg'ler ve güvenlik duvarı sanal Gereçleri arasında bazıları gösterilmektedir.

|  | Uzmanları | Simgeler |
| --- | --- | --- |
| NSG |Ücretsiz. <br/>Azure RBAC ile tümleşik. <br/>Azure Resource Manager şablonlarında kuralları oluşturulabilir. |Karmaşıklık büyük ortamlarda gösterebilir. |
| Güvenlik duvarı |Veri düzlemi üzerinde tam denetim sağlar. <br/>Güvenlik Duvarı Konsolu aracılığıyla merkezi yönetimi. |Güvenlik Duvarı Gereci maliyeti. <br/>Azure RBAC ile tümleşik değil. |

Aşağıdaki çözüm, bir çevre ağındaki (DMZ) uygulamak için güvenlik duvarı sanal gereçler kullanan / ağ senaryosu korumalı.

## <a name="considerations"></a>Dikkat edilmesi gerekenler
Bugün, aşağıdaki gibi farklı özelliklerin kullanarak Azure'da yukarıda açıklanan ortam dağıtabilirsiniz.

* **Sanal ağ (VNet)**. Bir Azure sanal ağı şirket içi ağa benzer şekilde davranır ve trafik yalıtımı ve görev ayrımı nettir sağlamak için bir veya daha fazla alt ağa bölümlenebilecek.
* **Sanal gereç**. Birkaç iş ortakları, yukarıda açıklanan üç güvenlik duvarları için kullanılabilecek Azure Marketi'nde sanal Gereçleri sağlar. 
* **Kullanıcı tanımlı yollar (UDR)**. Rota tabloları sanal ağ içindeki paketleri akışını denetlemek için Azure ağ tarafından kullanılan Udr'ler içerebilir. Bu rota tablolarını alt ağlara uygulanabilir. Azure'da yeni özelliklerden biri, bir yol tablosu Azure Vnet'te bir sanal gerece bir karma bağlantı'dan gelen tüm trafiği iletmesi için olanak vererek GatewaySubnet uygulamak için olanağıdır.
* **IP iletimi**. Varsayılan olarak, Azure ağ altyapısı İleri sanal ağ arabirim kartları (NIC) paketleri yalnızca NIC IP adresi paketin hedef IP adresi eşleşmesi durumunda. Bu nedenle, bir paket için belirli bir sanal gereç gönderilmelidir UDR tanımlar, Azure ağ altyapısı, paket bırakın. Paketin paket için gerçek hedef değil bir sanal makineye (Bu durumda bir sanal gereç) teslim sağlamak için sanal Gereci için IP iletimini etkinleştirmeniz gerekir.
* **Ağ güvenlik grupları (Nsg'ler)**. Aşağıdaki örnekte Nsg'ler kullanılmaz, ancak daha fazla ve bu alt ağları ve Nıc'lere giden trafiği filtrelemek için bu çözümde alt ağlar ve/veya NIC için uygulanan Nsg kullanabilirsiniz.

![IPv6 bağlantısı](./media/virtual-network-scenario-udr-gw-nva/figure01.png)

Bu örnekte aşağıdakileri içeren bir aboneliği vardır:

* şemada gösterilmez 2 kaynak grubu. 
  * **ONPREMRG**. Bir şirket içi ağı benzetmek gerekli tüm kaynakları içerir.
  * **AZURERG**. Azure sanal ağ ortamı için gerekli tüm kaynakları içerir. 
* Bir VNet **onpremvnet** aşağıda listelenen bölümlenmiş bir şirket içi veri merkezine taklit etmek için kullanılır.
  * **onpremsn1**. Bir şirket içi sunucu taklit etmek için Ubuntu çalıştıran sanal makine (VM) içeren bir alt ağ.
  * **onpremsn2**. Bir yönetici tarafından kullanılan bir şirket içi bilgisayarınıza taklit etmek için Ubuntu çalıştıran bir VM içeren bir alt ağ.
* Adlı bir güvenlik duvarı sanal Gereci yoktur **OPFW** üzerinde **onpremvnet** tünel korumak için kullanılan **azurevnet**.
* Bir VNet **azurevnet** aşağıda listelenen bölümlenmiş.
  * **azsn1**. Dış güvenlik duvarı dış güvenlik duvarı için özel olarak kullanılan alt ağ. Tüm Internet trafiğini bu alt ağ ile birlikte gelir. Bu alt ağı, yalnızca dış güvenlik duvarı bağlı bir NIC içerir.
  * **azsn2**. Ön uç alt ağı, Internet üzerinden erişilecek bir web sunucusu olarak çalışan bir sanal makine barındırma.
  * **azsn3**. Arka uç alt ağı ön uç web sunucusu tarafından erişilecek bir arka uç uygulama sunucusu çalıştıran bir sanal makine barındırma.
  * **azsn4**. Yönetim alt ağından yalnızca tüm güvenlik duvarı sanal Gereçleri yönetim erişimi sağlamak için kullanılır. Bu alt ağı, yalnızca bir NIC çözümde kullanılan her bir güvenlik duvarı sanal Gereci için içerir.
  * **GatewaySubnet**. Azure hibrit bağlantı alt ağ Azure sanal ağları ve diğer ağlar arasında bağlantı sağlamak için ExpressRoute ve VPN ağ geçidi için gereklidir. 
* 3 güvenlik duvarı sanal Gereçleri de vardır **azurevnet** ağ. 
  * **AZF1**. Azure'da bir genel IP adresi kaynağı kullanarak genel Internet'e açık dış güvenlik duvarı. Şablon Market ya da bu hükümler, gerecin satıcısına doğrudan bir 3-NIC sanal gereç olduğundan emin olun gerekir.
  * **AZF2**. Arasındaki trafiği denetlemek için kullanılan iç güvenlik duvarı **azsn2** ve **azsn3**. 3-NIC sanal gereç de budur.
  * **AZF3**. Yönetim ve güvenlik duvarı Yöneticiler şirket içi veri merkezinden erişilebilir, tüm güvenlik duvarı cihazları yönetmek için kullanılan bir yönetim alt ağına bağlı. 2 NIC sanal gereç şablonlarını Market'te bulun veya doğrudan Gereci satıcınızdan isteyin.

## <a name="user-defined-routing-udr"></a>Kullanıcı tanımlı yönlendirme (UDR)
Azure her alt ağ, alt ağ yönlendirildiği trafiği nasıl başlatılan tanımlamak için kullanılan bir UDR tabloya bağlanabilir. Azure, varsayılan yollar hiçbir Udr'ler tanımlanmışsa, bir alt ağdan diğerine trafiğe izin vermek için kullanır. Udr'ler daha iyi anlamak için şurayı ziyaret edin [kullanıcı tanımlı yollar ve IP iletimi nedir](virtual-networks-udr-overview.md).

İletişim, yukarıdaki son gereksinimi temel alan, doğru güvenlik duvarı Gereci aracılığıyla yapılır emin olmak için Udr içeren aşağıdaki yol tablosu oluşturmanız gerekir **azurevnet**.

### <a name="azgwudr"></a>azgwudr
Bu senaryoda, şirket içinden Azure'a yalnızca trafiği bağlanarak güvenlik duvarları yönetmek için kullanılacak **AZF3**, ve trafiği iç duvarından geçmelidir **AZF2**. Bu nedenle, yalnızca bir yol gerekli **GatewaySubnet** aşağıda gösterildiği gibi.

| Hedef | Sonraki atlama | Açıklama |
| --- | --- | --- |
| 10.0.4.0/24 |10.0.3.11 |Yönetimi Güvenlik Duvarı ulaşmak şirket içi trafiğe izin veren **AZF3** |

### <a name="azsn2udr"></a>azsn2udr
| Hedef | Sonraki atlama | Açıklama |
| --- | --- | --- |
| 10.0.3.0/24 |10.0.2.11 |Uygulama sunucusu aracılığıyla barındıran arka uç alt trafiğe izin veren **AZF2** |
| 0.0.0.0/0 |10.0.2.10 |Üzerinden yönlendirilmesini tüm trafiğe izin veren **AZF1** |

### <a name="azsn3udr"></a>azsn3udr
| Hedef | Sonraki atlama | Açıklama |
| --- | --- | --- |
| 10.0.2.0/24 |10.0.3.10 |Trafiğe izin veren **azsn2** için Web sunucusu uygulama sunucusundan flow'a **AZF2** |

Ayrıca alt ağlar için rota tabloları oluşturmak gereken **onpremvnet** şirket içi veri Merkezinize taklit etmek için.

### <a name="onpremsn1udr"></a>onpremsn1udr
| Hedef | Sonraki atlama | Açıklama |
| --- | --- | --- |
| 192.168.2.0/24 |192.168.1.4 |Trafiğe izin veren **onpremsn2** aracılığıyla **OPFW** |

### <a name="onpremsn2udr"></a>onpremsn2udr
| Hedef | Sonraki atlama | Açıklama |
| --- | --- | --- |
| 10.0.3.0/24 |192.168.2.4 |Azure'da yedeklenen alt trafiğe izin veren **OPFW** |
| 192.168.1.0/24 |192.168.2.4 |Trafiğe izin veren **onpremsn1** aracılığıyla **OPFW** |

## <a name="ip-forwarding"></a>IP İletimi
UDR ve IP iletimi özelliklerdir kullanabileceğiniz bir Azure sanal ağ trafiği akışını denetlemek için kullanılacak sanal Gereçleri izin vermek için birlikte.  Sanal gereç, güvenlik duvarı veya NAT cihazı gibi ağ trafiğini işlemek için kullanılan bir uygulamayı çalıştıran bir VM'den fazlası değildir.

Bu sanal gereç VM'si, kendisine yönelik olmayan gelen trafiği alabilmelidir. Bir VM'nin başka hedeflere yönelik trafiği alabilmesine izin vermek için VM'de IP İletimini etkinleştirmeniz gerekir. Bu ayar konuk işletim sisteminin değil, Azure'ın bir ayarıdır. Sanal gerecinize hala bazı gelen trafiği işlemek ve uygun şekilde yönlendirmek üzere uygulamayı çalıştırmak gerekir.

IP iletimi hakkında daha fazla bilgi edinmek için [kullanıcı tanımlı yollar ve IP iletimi nedir](virtual-networks-udr-overview.md).

Örneğin, bir Azure sanal ağ içinde aşağıdaki kurulumun düşünün:

* Alt ağ **onpremsn1** adlı bir VM içeren **onpremvm1**.
* Alt ağ **onpremsn2** adlı bir VM içeren **onpremvm2**.
* Adlı bir sanal gereç **OPFW** bağlı **onpremsn1** ve **onpremsn2**.
* Kullanıcı tanımlı bir yolun bağlı **onpremsn1** tüm için trafiği belirtir **onpremsn2** gönderilmelidir **OPFW**.

AT bu noktasında, **onpremvm1** ile bağlantı kurmaya **onpremvm2**, UDR kullanılacak ve trafik gönderilir **OPFW** sonraki atlama olarak. Gerçek bir paketi hedef değil değiştirilirken aklınızda tutun, yine de ifadesini içeren **onpremvm2** hedefi. 

IP iletme için etkinleştirilmiş olmadan **OPFW**, yalnızca paketler paket için hedef sanal makinenin IP adresi ise, bir VM'ye gönderilmesine izin verdiğinden, Azure sanal ağ mantıksal paketlerin düşeceği unutulmamalıdır.

IP iletimi ile Azure sanal ağı mantığı paketleri OPFW için özgün hedef adresini değiştirmeden iletir. **OPFW** paketlerini işlemek ve bunlarla yapmanız gerekenler belirlemek gerekir.

Yukarıdaki senaryoyu çözmek için üzerindeki NIC için IP iletimini etkinleştirmeniz gerekir **OPFW**, **AZF1**, **AZF2**, ve **AZF3** için kullanılır (tüm NIC'ler yönetim alt ağına bağlı olanlar hariç) yönlendirme. 

## <a name="firewall-rules"></a>Güvenlik Duvarı Kuralları
Yukarıda açıklandığı gibi yalnızca IP iletimi sanal Gereçleri için gönderilen paket sağlar. Bu paketleri ile yapmak karar verirken gerecinize hala gerekir. Yukarıdaki senaryoda gereçleriniz içinde aşağıdaki kuralları oluşturmanız gerekecektir:

### <a name="opfw"></a>OPFW
Aşağıdaki kuralları içeren bir şirket içi cihaz OPFW gösterir:

* **Rota**: Tüm trafiği 10.0.0.0/16 (**azurevnet**) tünel aracılığıyla gönderilmelidir **ONPREMAZURE**.
* **İlke**: Arasındaki tüm çift yönlü trafiğine izin **port2** ve **ONPREMAZURE**.

### <a name="azf1"></a>AZF1
AZF1 aşağıdaki kuralları içeren bir Azure sanal Gereci temsil eder:

* **İlke**: Arasındaki tüm çift yönlü trafiğine izin **port1** ve **port2**.

### <a name="azf2"></a>AZF2
AZF2 aşağıdaki kuralları içeren bir Azure sanal Gereci temsil eder:

* **Rota**: Tüm trafiği 10.0.0.0/16 (**onpremvnet**) için Azure ağ geçidi IP adresi (yani, 10.0.0.1) aracılığıyla gönderilmelidir **port1**.
* **İlke**: Arasındaki tüm çift yönlü trafiğine izin **port1** ve **port2**.

## <a name="network-security-groups-nsgs"></a>Ağ güvenlik grupları (Nsg'ler)
Bu senaryoda, Nsg'ler kullanılmayan. Ancak Nsg'ler gelen ve giden trafiği kısıtlamak için her alt ağa uygulayabilirsiniz. Örneğin, dış FW alt ağa aşağıdaki NSG kuralları uygulayabilirsiniz.

**gelen**

* Tüm TCP trafiği Internet'ten alt ağdaki herhangi bir VM'de 80 numaralı bağlantı noktasına izin verin.
* Diğer tüm trafiği Internet'ten reddet.

**Giden**

* Tüm trafiği Internet'e izin verme.

## <a name="high-level-steps"></a>Yüksek düzey adımları
Bu senaryoyu dağıtmak için aşağıdaki üst düzey adımları izleyin.

1. Azure aboneliğinizde oturum açın.
2. Şirket içi ağ taklit etmek için bir VNet dağıtmak istiyorsanız, bir parçası olan kaynakları sağlama **ONPREMRG**.
3. Parçası olan kaynakları sağlamak **AZURERG**.
4. Gelen Tünel sağlama **onpremvnet** için **azurevnet**.
5. Tüm kaynaklar sağlandıktan sonra oturum **onpremvm2** ve arasındaki bağlantıyı sınamak için 10.0.3.101 ping **onpremsn2** ve **azsn3**.

