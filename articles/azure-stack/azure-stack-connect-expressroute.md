---
title: "ExpressRoute kullanarak Azure Azure yığın Bağlan"
description: "Sanal ağlar Azure yığınında ExpressRoute kullanarak azure'daki sanal ağlara bağlanma."
services: azure-stack
documentationcenter: 
author: brenduns
manager: femila
editor: 
ms.assetid: 
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 9/25/2017
ms.author: brenduns
ms.reviewer: 
ms.openlocfilehash: 544fc1bcc9212fd38938d58447f5050df2a08796
ms.sourcegitcommit: 8c3267c34fc46c681ea476fee87f5fb0bf858f9e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/09/2018
---
# <a name="connect-azure-stack-to-azure-using-expressroute"></a>ExpressRoute kullanarak Azure Azure yığın Bağlan

*Uygulandığı öğe: Azure yığın tümleşik sistemleri ve Azure yığın Geliştirme Seti*

Azure'da sanal ağlar Azure yığınında sanal ağlara bağlanmak için desteklenen iki yöntem vardır:
   * **Siteden siteye**

     Bir VPN bağlantısı üzerinden IPSec (IKE v1 ve IKE v2). Bu bağlantı türüne şirket içi bir VPN cihazı ya da RRAS gerekir. Daha fazla bilgi için bkz: [Azure yığınına VPN kullanarak Azure](azure-stack-connect-vpn.md).
   * **ExpressRoute**

     Doğrudan bağlantı oluşturmak için Azure, Azure yığın dağıtımından. Expressroute'dur **değil** genel Internet üzerinden bir VPN bağlantısı. Azure ExpressRoute hakkında daha fazla bilgi için bkz: [ExpressRoute genel bakış](../expressroute/expressroute-introduction.md).

Bu makalede Azure yığın Azure'a bağlanmak için ExpressRoute kullanan bir örnek gösterilmektedir.
## <a name="requirements"></a>Gereksinimler
Azure yığını ve Azure ExpressRoute kullanarak bağlanmak için belirli gereksinimleri şunlardır:
* Bir expressroute bağlantı hattı ve sanal ağlar oluşturmak için Azure aboneliği.
* Bir expressroute bağlantı hattı üzerinden sağlanan bir [bağlantı sağlayıcı](../expressroute/expressroute-locations.md).
* Expressroute bağlantı hattı, WAN bağlantı noktalarına bağlı olan bir yönlendirici.
* Yönlendirici LAN tarafında Azure yığını çok kullanıcılı ağ geçidi bağlanır.
* Yönlendirici, LAN arabirimini ve Azure yığını çok kullanıcılı ağ geçidi arasında siteden siteye VPN bağlantıları desteklemesi gerekir.
* Birden çok Kiracı Azure yığın dağıtımınızda eklediyseniz, yönlendirici (sanal Yönlendirme ve iletme) birden çok VRFs oluşturmak mümkün olması gerekir.

Yapılandırmayı tamamladıktan sonra aşağıdaki diyagramda kavramsal bir ağ görünümü gösterilmektedir:

![Kavramsal diyagramı](media/azure-stack-connect-expressroute/Conceptual.png)

**Diyagram 1**

Aşağıdaki mimarisi diyagramı nasıl birden çok kiracıya ExpressRoute yönlendirici üzerinden Azure yığın altyapınızdan Azure'a Microsoft edge bağlanmak gösterir:

![Mimari diyagramı](media/azure-stack-connect-expressroute/Architecture.png)

**Diyagram 2**

Bu makalede gösterilen örnek aynı mimarisi için Azure ExpressRoute özel eşleme aracılığıyla bağlanmak için kullanır. Yapıldığını sanal ağ geçidi bir siteden siteye VPN bağlantısını ExpressRoute yönlendiriciye Azure yığınında kullanarak. Aşağıdaki adımlar bu makalede Azure ilgili sanal ağları için Azure yığınında iki farklı kiracılardan gelen iki Vnet arasında uçtan uca bağlantısının nasıl oluşturulacağını gösterir. Birçok Kiracı sanal ağlar ve her bir kiracı için adımları çoğaltmak veya yalnızca tek bir kiracı Vnet'i dağıtmak için bu örneği kullanmak olarak eklemeyi seçebilirsiniz.

## <a name="configure-azure-stack"></a>Azure yığını yapılandırma
Şimdi Kiracı olarak Azure yığın ortamınızı ayarlamak için ihtiyacınız olan kaynakları oluşturun. Aşağıdaki adımları gerçekleştirmeniz gereken gösterilmektedir. Bu yönergeler Azure yığın Portalı'nı kullanarak kaynakların oluşturulacağını gösterir, ancak PowerShell de kullanabilirsiniz.

![Ağ kaynak adımları](media/azure-stack-connect-expressroute/image2.png)
### <a name="before-you-begin"></a>Başlamadan önce
Yapılandırmaya başlamadan önce gerekir:
* Bir Azure yığın dağıtımı.

   Azure yığın Geliştirme Seti dağıtma hakkında daha fazla bilgi için bkz: [Azure yığın Geliştirme Seti dağıtım quickstart](azure-stack-deploy-overview.md).
* Bir teklifi Azure yığında kullanıcınız için abone olabilirsiniz.

  Yönergeler için bkz: [sanal makineler kullanılabilir duruma Azure yığın kullanıcılarınıza](azure-stack-tutorial-tenant-vm.md).

### <a name="create-network-resources-in-azure-stack"></a>Ağ kaynakları Azure yığınında oluşturun

Gerekli ağ kaynaklarına Azure yığındaki her bir kiracı oluşturmak için aşağıdaki yordamları kullanın:

#### <a name="create-the-virtual-network-and-vm-subnet"></a>Sanal ağ ve VM alt ağı oluşturma
1. Bir kullanıcı (Kiracı) hesabıyla Kullanıcı Portalı'nda oturum açın.

2. Portalı'nda tıklatın **yeni**.

   ![](media/azure-stack-connect-expressroute/MAS-new.png)

3. Market menüsünden **Ağ** seçeneğini belirleyin.

4. Menüde **Sanal ağ** öğesine tıklayın.

5. Değerleri aşağıdaki tabloda kullanarak uygun alanlarına yazın:

   |Alan  |Değer  |
   |---------|---------|
   |Ad     |Tenant1VNet1         |
   |Adres alanı     |10.1.0.0/16|
   |Alt ağ adı     |Tenant1-Sub1|
   |Alt ağ adres aralığı     |10.1.1.0/24|

6. Daha önce oluşturduğunuz Aboneliği, **Abonelik** alanında görmeniz gerekir.

    a. Kaynak grubu için bir kaynak grubu oluşturun veya zaten bir varsa seçin **var olanı kullan**.

    b. Varsayılan konumu doğrulayın.

    c. Tıklatın **panoya Sabitle**.

    d. **Oluştur**’a tıklayın.



#### <a name="create-the-gateway-subnet"></a>Ağ geçidi alt ağını oluşturma
1. Panodan (Tenant1VNet1) oluşturulan sanal ağ kaynağı açın.
2. Ayarları bölümündeki seçin **alt ağlar**.
3. Sanal ağa bir ağ geçidi alt ağı eklemek için **Ağ Geçidi Alt Ağı**’na tıklayın.
   
    ![](media/azure-stack-connect-expressroute/gatewaysubnet.png)
4. Alt ağın adı varsayılan olarak **GatewaySubnet** şeklinde ayarlanır.
   Ağ geçidi alt ağları özeldir ve düzgün şekilde çalışabilmesi için bu ada sahip olmalıdır.
5. İçinde **adres aralığı** alan, adresi doğrulayın **10.1.0.0/24**.
6. Tıklatın **Tamam** ağ geçidi alt ağı oluşturmak için.

#### <a name="create-the-virtual-network-gateway"></a>Sanal ağ geçidini oluşturma
1. Azure yığın Kullanıcı Portalı'nda tıklatın **yeni**.
   
2. Market menüsünden **Ağ** seçeneğini belirleyin.
3. Ağ kaynakları listesinden **Sanal ağ geçidi**’ni seçin.
4. **Ad** alanına **GW1** yazın.
5. Bir sanal ağ seçmek için **Sanal ağ** öğesine tıklayın.
   Seçin **Tenant1VNet1** listeden.
6. **Genel IP adresi** menü öğesine tıklayın. Zaman **genel IP adresi seçin** bölümü açılır tıklatın **Yeni Oluştur**.
7. **Ad** alanına **GW1-PiP** yazıp **Tamam**’a tıklayın.
8. **VPN türü** için varsayılan olarak **yol tabanlı** seçili olmalıdır.
    Bu ayarı tutun.
9. **Abonelik** ve **Konum** seçeneklerinin doğruluğunu onaylayın. İstiyorsanız, kaynak panoya sabitleyebilirsiniz. **Oluştur**’a tıklayın.

#### <a name="create-the-local-network-gateway"></a>Yerel ağ geçidini oluşturma

Yerel ağ geçidi kaynağı amacı VPN bağlantısının diğer ucundaki uzak ağ geçidi belirtmektir. Bu örnekte, uzak tarafı ExpressRoute yönlendiricinin LAN arabirim ' dir. Bu örnekte Kiracı 1 için Uzak 10.60.3.255 diyagramı 2'de gösterildiği gibi adresidir.

1. Azure Stack fiziksel makinesinde oturum açın.
2. Kullanıcı Portalı kullanıcı hesabınızla oturum açın ve tıklatın **yeni**.
3. Market menüsünden **Ağ** seçeneğini belirleyin.
4. Kaynak listesinden **yerel ağ geçidi**’ni seçin.
5. İçinde **adı** alan türü **ER yönlendirici GW**.
6. İçin **IP adresi** alan, Diyagram 2'ye bakın. 10.60.3.255 ExpressRoute yönlendiricinin LAN arabirim Kiracı 1 için IP adresidir. Kendi ortamı için yönlendiricinizin karşılık gelen arabiriminin IP adresini yazın.
7. İçinde **adres alanı** alan, Azure'da bağlanmak istediğiniz sanal ağlar, adres alanını yazın. Bu örnekte, Diyagram 2'ye bakın. Kiracı 1 için gerekli alt ağlar olduğuna dikkat edin **192.168.2.0/24** (Bu, Azure Hub Vnet) ve **10.100.0.0/16** (bağlı bileşen VNet Azure içinde budur). Kendi ortamınız için karşılık gelen alt ağları yazın.
   > [!IMPORTANT]
   > Bu örnek, Azure yığın ağ geçidi ve ExpressRoute yönlendirici arasında siteden siteye VPN bağlantısı için statik yollar kullandığınızı varsayar.

8. Doğrulayın, **abonelik**, **kaynak grubu**, ve **konumu** doğru olduğunu ve tıklayın **oluşturma**.

#### <a name="create-the-connection"></a>Bağlantı oluşturma
1. Azure yığın Kullanıcı Portalı'nda tıklatın **yeni**.
2. Market menüsünden **Ağ** seçeneğini belirleyin.
3. Kaynak listesinden **Bağlantı**’yı seçin.
4. İçinde **Temelleri** Ayarlar bölümünde seçin **siteden siteye (IPSec)** olarak **bağlantı türü**.
5. Seçin **abonelik**, **kaynak grubu**, ve **konumu** tıklatıp **Tamam**.
6. İçinde **ayarları** 'yi tıklatın **sanal ağ geçidi** tıklatın **GW1**.
7. Tıklatın **yerel ağ geçidi**, tıklatıp **ER yönlendirici GW**.
8. İçinde **bağlantı adı** alanında, yazın **ConnectToAzure**.
9. İçinde **paylaşılan anahtar (PSK)** alanında, yazın **abc123** tıklatıp **Tamam**.
10. Üzerinde **Özet** 'yi tıklatın **Tamam**.

    Bağlantı kurulduktan sonra sanal ağ geçidi tarafından kullanılan genel IP adresi görebilirsiniz. Adres Azure yığın Portalı'nda bulmak için sanal ağ geçidinizin göz atın. İçinde **genel bakış**, bulma **genel IP adresi**. Bu adres unutmayın; olarak kullanacağınız *iç IP adresi* sonraki bölümde (dağıtımınız için varsa).

    ![](media/azure-stack-connect-expressroute/GWPublicIP.png)

#### <a name="create-a-virtual-machine"></a>Sanal makine oluşturun
VPN bağlantısı üzerinden yolculuk verileri doğrulamak için Azure yığın Vnet içinde veri alıp göndermek için sanal makineler gerekir. Şimdi bir sanal makine oluşturun ve sizin VM alt ağdaki sanal ağınızda yerleştirin.

1. Azure yığın Kullanıcı Portalı'nda tıklatın **yeni**.
2. Market menüsünden **Sanal Makineler**’i seçin.
3. Sanal makine görüntülerini listesinde seçin **Windows Server 2016 Datacenter Eval** görüntü ve tıklatın **oluşturma**.
4. Üzerinde **Temelleri** bölümünde **adı** alan türü **VM01**.
5. Geçerli bir kullanıcı adı ve parola yazın. Bu hesabı oluşturduktan sonra VM’de oturum açmak için kullanacaksınız.
6. Sağlayan bir **abonelik**, **kaynak grubu**, ve **konumu** ve ardından **Tamam**.
7. Üzerinde **boyutu** bölümünde, bir sanal makine boyutu bu örneğe tıklayın ve ardından **seçin**.
8. Üzerinde **ayarları** bölümünde, Varsayılanları kabul edebilir. Ancak seçilen sanal ağ olduğundan emin olun **Tenant1VNet1** ve alt ağ ayarı **10.1.1.0/24**. **Tamam**’a tıklayın.
9. Ayarları gözden **Özet** 'ye tıklayın **Tamam**.

Her bir kiracı bağlanmak istediğiniz sanal ağ'ı için önceki adımları yineleyin **VM alt ağı ve sanal ağ oluşturma** aracılığıyla **bir sanal makine oluşturmak** bölümler.

### <a name="configure-the-nat-virtual-machine-for-gateway-traversal"></a>Ağ geçidi geçişi için NAT sanal makine yapılandırma
> [!IMPORTANT]
> Bu bölüm, yalnızca Azure yığın Geliştirme Seti dağıtımları için değildir. NAT çok düğümlü dağıtımlar için gerekli değildir.

Azure yığın Geliştirme Seti kendi içinde bulunan ve fiziksel ana bilgisayar dağıtıldığı ağdan yalıtılmış oluşur. Bu nedenle, ağ geçitleri bağlı "Dış" VIP ağ dış değildir, ancak bunun yerine yapılması ağ adresi çevirisi (NAT) yönlendiricisi arkasında gizlenir.
 
Bir Windows Server sanal makine yönlendiricidir (**AzS BGPNAT01**) yönlendirme ve Uzaktan Erişim Hizmetleri (RRAS) rol Azure yığın Geliştirme Seti altyapısında çalışan. NAT AzS BGPNAT01 sanal makinedeki her iki ucunun bağlanmak siteden siteye VPN bağlantısı izin verecek şekilde yapılandırmanız gerekir.

#### <a name="configure-the-nat"></a>NAT yapılandırın

1. Azure yığın fiziksel makine Yönetici hesabınızla oturum açın.
2. Kopyalayın ve aşağıdaki PowerShell komut dosyasını düzenleyin ve bir yükseltilmiş Windows PowerShell ISE çalıştırın. Yönetici parolanızı değiştirin. Adresi döndürülmezse, *dış BGPNAT adresi*.

   ```
   cd \AzureStack-Tools-master\connect
   Import-Module .\AzureStack.Connect.psm1
   $Password = ConvertTo-SecureString "<your administrator password>" `
    -AsPlainText `
    -Force
   Get-AzureStackNatServerAddress `
    -HostComputer "azs-bgpnat01" `
    -Password $Password
   ```
4. NAT yapılandırmak için aşağıdaki PowerShell betiğini düzenleyin ve bir yükseltilmiş Windows PowerShell ISE çalıştırın. Değiştirmek için komut dosyasını düzenleyerek *dış BGPNAT adresi* ve *iç IP adresi* (hangi daha önce de not ettiğiniz **bağlantı oluşturmak** bölümü).

   Örnek diyagramlarındaki *dış BGPNAT adresi* 10.10.0.62 olduğu ve *iç IP adresi* 192.168.102.1 değil.

   ```
   $ExtBgpNat = '<External BGPNAT address>'
   $IntBgpNat = '<Internal IP address>'

   # Designate the external NAT address for the ports that use the IKE authentication.
   Invoke-Command `
    -ComputerName azs-bgpnat01 `
     {Add-NetNatExternalAddress `
      -NatName BGPNAT `
      -IPAddress $Using:ExtBgpNat `
      -PortStart 499 `
      -PortEnd 501}
   Invoke-Command `
    -ComputerName azs-bgpnat01 `
     {Add-NetNatExternalAddress `
      -NatName BGPNAT `
      -IPAddress $Using:ExtBgpNat `
      -PortStart 4499 `
      -PortEnd 4501}
   # create a static NAT mapping to map the external address to the Gateway
   # Public IP Address to map the ISAKMP port 500 for PHASE 1 of the IPSEC tunnel
   Invoke-Command `
    -ComputerName azs-bgpnat01 `
     {Add-NetNatStaticMapping `
      -NatName BGPNAT `
      -Protocol UDP `
      -ExternalIPAddress $Using:ExtBgpNat `
      -InternalIPAddress $Using:IntBgpNat `
      -ExternalPort 500 `
      -InternalPort 500}
   # Finally, configure NAT traversal which uses port 4500 to
   # successfully establish the complete IPSEC tunnel over NAT devices
   Invoke-Command `
    -ComputerName azs-bgpnat01 `
     {Add-NetNatStaticMapping `
      -NatName BGPNAT `
      -Protocol UDP `
      -ExternalIPAddress $Using:ExtBgpNat `
      -InternalIPAddress $Using:IntBgpNat `
      -ExternalPort 4500 `
      -InternalPort 4500}
   ```

## <a name="configure-azure"></a>Azure yapılandırma
Azure yığını yapılandırma tamamladığınıza göre bazı Azure kaynakları dağıtabilirsiniz. Aşağıdaki diyagramda bir örnek Kiracı gösterilmektedir Azure sanal ağında. Azure, sanal ağınızda herhangi bir ad ve adres düzeni kullanabilirsiniz. Ancak, sanal ağlar Azure ve Azure yığında adres aralığı benzersiz olmalı ve çakışmamalıdır.

![Azure sanal ağlar](media/azure-stack-connect-expressroute/AzureArchitecture.png)

**Diyagram 3**

Azure'da dağıtmak kaynakları Azure yığınında dağıtılan kaynak benzerdir. Benzer şekilde, dağıttığınız:
* Sanal ağlar ve alt ağlar
* Bir ağ geçidi alt ağı
* Bir sanal ağ geçidi
* Bir bağlantı
* Bir expressroute bağlantı hattı

Örnek Azure ağ altyapısı aşağıdaki şekilde yapılandırılır:
* Standart hub (192.168.2.0/24) ve bağlı bileşen (10.100.0.0./16) VNet modeli kullanılır.
* İş yükleri spoke Vnet dağıtılır ve expressroute bağlantı hattı hub'a VNet bağlanır.
* İki sanal ağlar sanal ağ eşleme özelliğini kullanarak bağlanır.

### <a name="configure-vnets"></a>Sanal ağlar yapılandırın
1. Azure portalında Azure kimlik bilgilerinizle oturum açın.
2. Hub 192.168.2.0/24 adres alanı kullanarak VNet oluşturun. 192.168.2.0/25 adres aralığını kullanarak bir alt ağ oluşturmak ve 192.168.2.128/27 adres aralığını kullanarak bir ağ geçidi alt ağı ekleyin.
3. Spoke oluşturmak VNet ve 10.100.0.0/16 kullanarak alt ağ adres aralığı.


Azure'da sanal ağlar oluşturma hakkında daha fazla bilgi için bkz: [bir sanal ağ oluşturma](../virtual-network/manage-virtual-network.md#create-a-virtual-network).

### <a name="configure-an-expressroute-circuit"></a>Bir expressroute bağlantı hattını yapılandırın

1. ExpressRoute önkoşulları gözden [ExpressRoute önkoşulları ve denetim listesi](../expressroute/expressroute-prerequisites.md).
2. Adımları [oluşturma ve bir expressroute bağlantı hattı değiştirme](../expressroute/expressroute-howto-circuit-portal-resource-manager.md) Azure aboneliğinizi kullanarak bir expressroute bağlantı hattı oluşturmak için.
3. Hizmet anahtarı önceki adımdan barındırma/kendi sonunda, expressroute bağlantı hattı sağlayacak sağlayıcınız ile paylaşır.
4. Adımları [oluşturma ve bir ExpressRoute bağlantı hattı için eşlemesini değiştirmek](../expressroute/expressroute-howto-routing-portal-resource-manager.md) expressroute bağlantı hattında özel eşdüzey hizmet sağlamanın yapılandırmak için.

### <a name="create-the-virtual-network-gateway"></a>Sanal ağ geçidini oluşturma

* Adımları [ExpressRoute için PowerShell kullanarak bir sanal ağ geçidi yapılandırma](../expressroute/expressroute-howto-add-gateway-resource-manager.md) hub Vnet'i ExpressRoute için bir sanal ağ geçidi oluşturmak için.

### <a name="create-the-connection"></a>Bağlantı oluşturma

* Expressroute bağlantı hattı VNet hub'a bağlanmak için adımları [bir expressroute bağlantı hattı için bir sanal ağa bağlanma](../expressroute/expressroute-howto-linkvnet-portal-resource-manager.md).

### <a name="peer-the-vnets"></a>Sanal ağlar eş

* Hub ve bağlı bileşen Vnet'ler içindeki adımları kullanarak eş [Azure portalını kullanarak bir sanal ağ eşlemesi oluşturma](../virtual-network/virtual-networks-create-vnetpeering-arm-portal.md). VNet eşlemesi yapılandırırken, aşağıdaki seçenekleri belirleyin emin olun:
   * Spoke hub'dan: **ağ geçidi transit izin ver**
   * Hub'ına bağlı bileşen gelen: **uzak ağ geçidini kullan**

### <a name="create-a-virtual-machine"></a>Sanal makine oluşturun

* İş yükü sanal makinelerinize VNet spoke dağıtın.

Bir ek Kiracı ilgili kendi ExpressRoute bağlantı hatları Azure'da bağlanmak istediğiniz sanal ağlar için bu adımları yineleyin.

## <a name="configure-the-router"></a>Yönlendirici yapılandırın

ExpressRoute yönlendiricinizin yapılandırma kılavuzu için aşağıdaki uçtan uca altyapı diyagramı kullanabilirsiniz. Bu diyagramda iki kiracılar (Kiracı 1 ve Kiracı 2) kendi ilgili Expressroute bağlantı hatları ile gösterilmektedir. Her iki kiracılar arasında uçtan uca yalıtımı sağlamak üzere ExpressRoute yönlendirici LAN ve WAN tarafındaki kendi VRF (sanal Yönlendirme ve iletme) bağlıdır. Örnek yapılandırma yönergeleri izleyin. yönlendirici arabirimleri kullanılan IP adreslerini not edin.

![Diyagram uçtan uca](media/azure-stack-connect-expressroute/EndToEnd.png)

**Diyagram 4**

Ikev2 VPN ve BGP Azure yığından siteden siteye VPN bağlantısı sonlandırmak için destekleyen herhangi bir yönlendirici kullanabilirsiniz. Aynı yönlendirici, bir expressroute bağlantı hattı kullanarak Azure'a bağlanmak için kullanılır. 

Diyagram 4'te gösterilen ağ altyapısını destekleyen bir Cisco ASR 1000 örnek yapılandırmasından şöyledir:

```
ip vrf Tenant 1
 description Routing Domain for PRIVATE peering to Azure for Tenant 1
 rd 1:1
!
ip vrf Tenant 2
 description Routing Domain for PRIVATE peering to Azure for Tenant 2
 rd 1:5
!
crypto ikev2 proposal V2-PROPOSAL2 
description IKEv2 proposal for Tenant 1 
encryption aes-cbc-256
 integrity sha256
 group 2
crypto ikev2 proposal V4-PROPOSAL2 
description IKEv2 proposal for Tenant 2 
encryption aes-cbc-256
 integrity sha256
 group 2
!
crypto ikev2 policy V2-POLICY2 
description IKEv2 Policy for Tenant 1 
match fvrf Tenant 1
 match address local 10.60.3.255
 proposal V2-PROPOSAL2
description IKEv2 Policy for Tenant 2
crypto ikev2 policy V4-POLICY2 
 match fvrf Tenant 2
 match address local 10.60.3.251
 proposal V4-PROPOSAL2
!
crypto ikev2 profile V2-PROFILE
description IKEv2 profile for Tenant 1 
match fvrf Tenant 1
 match address local 10.60.3.255
 match identity remote any
 authentication remote pre-share key abc123
 authentication local pre-share key abc123
 ivrf Tenant 1
!
crypto ikev2 profile V4-PROFILE
description IKEv2 profile for Tenant 2
 match fvrf Tenant 2
 match address local 10.60.3.251
 match identity remote any
 authentication remote pre-share key abc123
 authentication local pre-share key abc123
 ivrf Tenant 2
!
crypto ipsec transform-set V2-TRANSFORM2 esp-gcm 256 
 mode tunnel
crypto ipsec transform-set V4-TRANSFORM2 esp-gcm 256 
 mode tunnel
!
crypto ipsec profile V2-PROFILE
 set transform-set V2-TRANSFORM2 
 set ikev2-profile V2-PROFILE
!
crypto ipsec profile V4-PROFILE
 set transform-set V4-TRANSFORM2 
 set ikev2-profile V4-PROFILE
!
interface Tunnel10
description S2S VPN Tunnel for Tenant 1
 ip vrf forwarding Tenant 1
 ip address 11.0.0.2 255.255.255.252
 ip tcp adjust-mss 1350
 tunnel source TenGigabitEthernet0/1/0.211
 tunnel mode ipsec ipv4
 tunnel destination 10.10.0.62
 tunnel vrf Tenant 1
 tunnel protection ipsec profile V2-PROFILE
!
interface Tunnel20
description S2S VPN Tunnel for Tenant 2
 ip vrf forwarding Tenant 2
 ip address 11.0.0.2 255.255.255.252
 ip tcp adjust-mss 1350
 tunnel source TenGigabitEthernet0/1/0.213
 tunnel mode ipsec ipv4
 tunnel destination 10.10.0.62
 tunnel vrf VNET3
 tunnel protection ipsec profile V4-PROFILE
!
interface GigabitEthernet0/0/1
 description PRIMARY Express Route Link to AZURE over Equinix
 no ip address
 negotiation auto
!
interface GigabitEthernet0/0/1.100
description Primary WAN interface of Tenant 1
 description PRIMARY ER link supporting Tenant 1 to Azure
 encapsulation dot1Q 101
 ip vrf forwarding Tenant 1
 ip address 192.168.1.1 255.255.255.252
!
interface GigabitEthernet0/0/1.102
description Primary WAN interface of Tenant 2
 description PRIMARY ER link supporting Tenant 2 to Azure
 encapsulation dot1Q 102
 ip vrf forwarding Tenant 2
 ip address 192.168.1.17 255.255.255.252
!
interface GigabitEthernet0/0/2
 description BACKUP Express Route Link to AZURE over Equinix
 no ip address
 negotiation auto
!
interface GigabitEthernet0/0/2.100
description Secondary WAN interface of Tenant 1
 description BACKUP ER link supporting Tenant 1 to Azure
 encapsulation dot1Q 101
 ip vrf forwarding Tenant 1
 ip address 192.168.1.5 255.255.255.252
!
interface GigabitEthernet0/0/2.102
description Secondary WAN interface of Tenant 2 
description BACKUP ER link supporting Tenant 2 to Azure
 encapsulation dot1Q 102
 ip vrf forwarding Tenant 2
 ip address 192.168.1.21 255.255.255.252
!
interface TenGigabitEthernet0/1/0
 description Downlink to ---Port 1/47
 no ip address
!
interface TenGigabitEthernet0/1/0.211
 description LAN interface of Tenant 1
description Downlink to --- Port 1/47.211
 encapsulation dot1Q 211
 ip vrf forwarding Tenant 1
 ip address 10.60.3.255 255.255.255.254
!
interface TenGigabitEthernet0/1/0.213
description LAN interface of Tenant 2
 description Downlink to --- Port 1/47.213
 encapsulation dot1Q 213
 ip vrf forwarding Tenant 2
 ip address 10.60.3.251 255.255.255.254
!
router bgp 65530
 bgp router-id <removed>
 bgp log-neighbor-changes
 description BGP neighbor config and route advertisement for Tenant 1 VRF 
 address-family ipv4 vrf Tenant 1
  network 10.1.0.0 mask 255.255.0.0
  network 10.60.3.254 mask 255.255.255.254
  network 192.168.1.0 mask 255.255.255.252
  network 192.168.1.4 mask 255.255.255.252
  neighbor 10.10.0.62 remote-as 65100
  neighbor 10.10.0.62 description VPN-BGP-PEER-for-Tenant 1
  neighbor 10.10.0.62 ebgp-multihop 5
  neighbor 10.10.0.62 activate
  neighbor 10.60.3.254 remote-as 4232570301
  neighbor 10.60.3.254 description LAN peer for CPEC:INET:2112 VRF
  neighbor 10.60.3.254 activate
  neighbor 10.60.3.254 route-map BLOCK-ALL out
  neighbor 192.168.1.2 remote-as 12076
  neighbor 192.168.1.2 description PRIMARY ER peer for Tenant 1 to Azure
  neighbor 192.168.1.2 ebgp-multihop 5
  neighbor 192.168.1.2 activate
  neighbor 192.168.1.2 soft-reconfiguration inbound
  neighbor 192.168.1.2 route-map Tenant 1-ONLY out
  neighbor 192.168.1.6 remote-as 12076
  neighbor 192.168.1.6 description BACKUP ER peer for Tenant 1 to Azure
  neighbor 192.168.1.6 ebgp-multihop 5
  neighbor 192.168.1.6 activate
  neighbor 192.168.1.6 soft-reconfiguration inbound
  neighbor 192.168.1.6 route-map Tenant 1-ONLY out
  maximum-paths 8
 exit-address-family
 !
description BGP neighbor config and route advertisement for Tenant 2 VRF 
address-family ipv4 vrf Tenant 2
  network 10.1.0.0 mask 255.255.0.0
  network 10.60.3.250 mask 255.255.255.254
  network 192.168.1.16 mask 255.255.255.252
  network 192.168.1.20 mask 255.255.255.252
  neighbor 10.10.0.62 remote-as 65300
  neighbor 10.10.0.62 description VPN-BGP-PEER-for-Tenant 2
  neighbor 10.10.0.62 ebgp-multihop 5
  neighbor 10.10.0.62 activate
  neighbor 10.60.3.250 remote-as 4232570301
  neighbor 10.60.3.250 description LAN peer for CPEC:INET:2112 VRF
  neighbor 10.60.3.250 activate
  neighbor 10.60.3.250 route-map BLOCK-ALL out
  neighbor 192.168.1.18 remote-as 12076
  neighbor 192.168.1.18 description PRIMARY ER peer for Tenant 2 to Azure
  neighbor 192.168.1.18 ebgp-multihop 5
  neighbor 192.168.1.18 activate
  neighbor 192.168.1.18 soft-reconfiguration inbound
  neighbor 192.168.1.18 route-map VNET-ONLY out
  neighbor 192.168.1.22 remote-as 12076
  neighbor 192.168.1.22 description BACKUP ER peer for Tenant 2 to Azure
  neighbor 192.168.1.22 ebgp-multihop 5
  neighbor 192.168.1.22 activate
  neighbor 192.168.1.22 soft-reconfiguration inbound
  neighbor 192.168.1.22 route-map VNET-ONLY out
  maximum-paths 8
 exit-address-family
!
ip forward-protocol nd
!
ip as-path access-list 1 permit ^$
ip route vrf Tenant 1 10.1.0.0 255.255.0.0 Tunnel10
ip route vrf Tenant 2 10.1.0.0 255.255.0.0 Tunnel20
!
ip prefix-list BLOCK-ALL seq 5 deny 0.0.0.0/0 le 32
!
route-map BLOCK-ALL permit 10
 match ip address prefix-list BLOCK-ALL
!
route-map VNET-ONLY permit 10
 match as-path 1
!
```

## <a name="test-the-connection"></a>Bağlantıyı test et

Siteden siteye bağlantı ve expressroute bağlantı hattı kurulduktan sonra bağlantınızı sınayın. Bu basit bir görevdir.  Azure sanal ağınızda oluşturulan sanal makineler birine oturum açın ve Azure yığın ortamında veya tersi oluşturduğunuz sanal makineye ping. 

Siteden siteye ve ExpressRoute bağlantıları üzerinden trafiği gönderme emin olmak için sanal makinenin hem uçları ayrılmış IP'yi (DIP) adresini ve VIP adresi değil sanal makinenin ping gerekir. Bu nedenle, bulma ve diğer ucundaki bağlantı adresi not edin.

### <a name="allow-icmp-in-through-the-firewall"></a>ICMP, güvenlik duvarı aracılığıyla izin ver
Varsayılan olarak, Windows Server 2016 ICMP paketleri güvenlik duvarı aracılığıyla izin vermiyor. Bu nedenle, her testinde kullanmak için sanal makine, yükseltilmiş bir PowerShell penceresinde aşağıdaki cmdlet'i çalıştırın:


   ```
   New-NetFirewallRule `
    –DisplayName “Allow ICMPv4-In” `
    –Protocol ICMPv4
   ```

### <a name="ping-the-azure-stack-virtual-machine"></a>Ping Azure yığın sanal makine

1. Bir kiracı hesabını kullanarak Azure yığın kullanıcı portalında oturum açın.
2. Sol gezinti çubuğunda **Sanal Makineler**’e tıklayın.
3. Daha önce oluşturduğunuz sanal makine bulun ve tıklatın.
4. Sanal makine için bölümünü tıklatın **Bağlan**.
5. Yükseltilmiş bir PowerShell açmak penceresi ve türü **ipconfig/all**.
6. IPv4 adresi çıktısında bulun ve Not. Bu adres Azure VNet içindeki sanal makineden ping işlemi yapın. Örnek ortamında 10.1.1.x/24 alt ağdan adresidir. Ortamınızda adresi farklı olabilir. Ancak, Kiracı sanal alt ağ için oluşturduğunuz alt ağ içinde olmalıdır.


### <a name="view-data-transfer-statistics"></a>Veri aktarımı istatistiklerini görüntüleme

Ne kadar trafik bağlantınızı geçirme bilmek istiyorsanız, bu bilgilere bağlantı bölümü Azure yığın Kullanıcı Portalı'nda bulabilirsiniz. Bu bilgiler aynı zamanda yalnızca gönderilen ping gerçekte üzerinden VPN ve ExpressRoute bağlantıları oluştu doğrulamak için başka bir iyi yoldur.

1. Kiracı hesabınızı kullanarak Microsoft Azure yığın kullanıcı portalında oturum açın.
2. VPN ağ geçidinizi oluşturulduğu kaynak grubuna gidin ve seçin **bağlantıları** nesne türü.
3. Tıklatın **ConnectToAzure** bağlantısını listesinde.
4. Üzerinde **bağlantı** bölümünde istatistiklerini görebilirsiniz **verileri** ve **verileri**. Burada sıfır olmayan bazı değerler görürsünüz.

   ![Verilerini](media/azure-stack-connect-expressroute/DataInDataOut.png)

## <a name="next-steps"></a>Sonraki adımlar
[Azure ve Azure uygulamaları dağıtmak yığını](azure-stack-solution-pipeline.md)
