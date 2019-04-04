---
title: Azure Stack ExpressRoute kullanarak Azure'a bağlanma
description: Azure Stack sanal ağları ExpressRoute kullanarak azure'daki sanal ağlara bağlanma hakkında bilgi edinin.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 03/22/2019
ms.author: sethm
ms.reviewer: unknown
ms.lastreviewed: 10/22/2018
ms.openlocfilehash: bd5e5a3b6fa72698f04969219b1db3cdb0bde3a5
ms.sourcegitcommit: 0dd053b447e171bc99f3bad89a75ca12cd748e9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2019
ms.locfileid: "58486708"
---
# <a name="connect-azure-stack-to-azure-using-azure-expressroute"></a>Azure Stack, Azure ExpressRoute kullanarak Azure'a bağlanma

*Uygulama hedefi: Azure Stack tümleşik sistemleri ve Azure Stack Geliştirme Seti*

Bu makalede bir Azure Stack sanal ağı kullanarak bir Azure sanal ağına bağlanmak nasıl bir [Microsoft Azure ExpressRoute](/azure/expressroute/) doğrudan bağlantı.

Bu makale bir öğretici olarak kullanın ve aynı test ortamını ayarlamak için örnekleri kullanın. Veya, makale, kendi ExpressRoute ortamını ayarlamanız konusunda size rehberlik edecek bir kılavuz olarak kullanabilirsiniz.

## <a name="overview-assumptions-and-prerequisites"></a>Genel bakış, tahminler ve Önkoşullar

Azure ExpressRoute, bağlantı sağlayıcı tarafından sağlanan özel bir bağlantı üzerinden, şirket içi ağlarınızı Microsoft bulutuna genişletmenizi sağlar. ExpressRoute VPN bağlantısını genel internet üzerinden değil.

Azure ExpressRoute hakkında daha fazla bilgi için bkz. [Expressroute'a genel bakış](../expressroute/expressroute-introduction.md).

### <a name="assumptions"></a>Varsayımlar

Bu makalede, olduğunu varsayar:

* Azure bilgisine sahip.
* Azure Stack temel bir anlayışa sahip.
* Ağ temel bir anlayışa sahip.

### <a name="prerequisites"></a>Önkoşullar

Azure Stack ve Azure ExpressRoute kullanarak bağlanmak için aşağıdaki gereksinimleri karşılaması gerekir:

* Sağlanan bir [ExpressRoute bağlantı hattı](../expressroute/expressroute-circuit-peerings.md) aracılığıyla bir [bağlantı sağlayıcısı](../expressroute/expressroute-locations.md).
* Azure'da bir ExpressRoute bağlantı hattı ve sanal ağlar oluşturmak için bir Azure aboneliği.
* Yönlendirici gerekir:
  * LAN arabirimini ve Azure Stack çok kullanıcılı ağ geçidi arasında siteden siteye VPN bağlantılarını destekler.
  * Azure Stack Dağıtımınızda birden fazla Kiracı varsa birden çok VRFs (sanal Yönlendirme ve iletme) oluşturmayı destekler.
* Sahip bir yönlendirici:
  * Bir WAN bağlantı, ExpressRoute işlem hattına bağlı.
  * Azure Stack çok kullanıcılı ağ geçidi için LAN bağlantı noktasına bağlı.

### <a name="expressroute-network-architecture"></a>ExpressRoute ağ mimarisi

ExpressRoute kurulumunu tamamladıktan sonra aşağıdaki diyagramda Azure Stack ve Azure ortamları, bu makaledeki örnekler kullanarak gösterilmektedir:

*Şekil 1. ExpressRoute network*

![ExpressRoute ağ](media/azure-stack-connect-expressroute/Conceptual.png)

Aşağıdaki diyagramda, nasıl birden çok kiracının Azure Stack altyapısından ExpressRoute yönlendirici üzerinden Azure'a Microsoft edge bağlama gösterilmektedir:

*Şekil 2. Çok kiracılı bağlantıları*

![Çok kiracılı bağlantılar ile ExpressRoute](media/azure-stack-connect-expressroute/Architecture.png)

Bu makaledeki örnek, Azure Stack ExpressRoute özel eşlemesini kullanarak Azure'a bağlanmak için Şekil 2 ' gösterilen aynı çok kiracılı mimari kullanır. Bağlantı yapılır bir sanal ağ geçidine siteden siteye VPN bağlantısını bir ExpressRoute yönlendiricisine otomatik olarak Azure Stack'te kullanarak.

Bu makaledeki adımlarda azure'da karşılık gelen sanal ağlar için Azure Stack'te iki farklı kiracılardan gelen iki Vnet arasında bir uçtan uca bağlantısı oluşturma işlemini göstermektedir. İki kiracılar'ı ayarlama isteğe bağlıdır; Ayrıca, tek bir kiracı için aşağıdaki adımları kullanabilirsiniz.

## <a name="configure-azure-stack"></a>Azure yığını yapılandırma

İlk Kiracı için Azure Stack ortamı ayarlamak için adımları aşağıdaki diyagramda bir kılavuz olarak kullanın. Birden fazla Kiracı tutunun, bu adımları yineleyin:

>[!NOTE]
>Bu adımlar Azure Stack portalını kullanarak kaynak oluşturma işlemini gösterir, ancak PowerShell de kullanabilirsiniz.

![Azure Stack ağ kurulumu](media/azure-stack-connect-expressroute/image2.png)

### <a name="before-you-begin"></a>Başlamadan önce

Azure Stack yapılandırmaya başlamadan önce ihtiyacınız vardır:

* Bir Azure Stack tümleşik sistemi dağıtımı veya bir Azure Stack geliştirme Seti'ni (ASDK) dağıtımı. ASDK dağıtma hakkında daha fazla bilgi için bkz: [Azure Stack Geliştirme Seti dağıtımına Hızlı Başlangıç](azure-stack-deploy-overview.md).
* Bir teklifi Azure stack'teki kullanıcılarınız için abone olabilirsiniz. Daha fazla bilgi için [planlar, teklifler ve abonelikler](azure-stack-plan-offer-quota-overview.md).

### <a name="create-network-resources-in-azure-stack"></a>Azure Stack'te ağ kaynakları oluşturma

Gerekli ağ kaynaklarına Azure Stack için bir kiracı oluşturmak için aşağıdaki yordamları kullanın.

#### <a name="create-the-virtual-network-and-vm-subnet"></a>Sanal ağ ve VM alt ağı oluşturma

1. Kullanıcı Portalı bir kullanıcı (Kiracı) hesabıyla oturum açın.

2. Portalında **+ kaynak Oluştur**.

3. Altında **Azure Marketi**seçin **ağ**.

4. Altında **öne çıkan**seçin **sanal ağ**.

5. Altında **sanal ağ oluştur**, uygun alanlara aşağıdaki tabloda gösterilen değerleri girin:

   |Alan  |Değer  |
   |---------|---------|
   |Ad     |Tenant1VNet1         |
   |Adres alanı     |10.1.0.0/16|
   |Alt ağ adı     |Tenant1 Abon1|
   |Alt ağ adres aralığı     |10.1.1.0/24|

6. Daha önce oluşturduğunuz aboneliği görmelisiniz **abonelik** alan. Kalan alanlar için:

    * Altında **kaynak grubu**seçin **Yeni Oluştur** zaten, yoksa yeni bir kaynak grubu oluşturun veya **var olanı kullan**.
    * Varsayılan doğrulama **konumu**.
    * **Oluştur**’a tıklayın.
    * (İsteğe bağlı) Tıklayın **panoya Sabitle**.

#### <a name="create-the-gateway-subnet"></a>Ağ geçidi alt ağını oluşturma

1. Altında **sanal ağ**seçin **Tenant1VNet1**.
1. **AYARLAR** altında **Alt ağlar**’ı seçin.
1. Seçin **+ ağ geçidi alt ağı** sanal ağa bir ağ geçidi alt ağı eklemek için.
1. Alt ağın adı varsayılan olarak **GatewaySubnet** şeklinde ayarlanır. Ağ geçidi alt ağları, özel bir durumdur ve düzgün çalışması için bu adını kullanması gerekir.
1. Doğrulayın **adres aralığı** olduğu **10.1.0.0/24**.
1. Tıklayın **Tamam** ağ geçidi alt ağı oluşturmak için.

#### <a name="create-the-virtual-network-gateway"></a>Sanal ağ geçidini oluşturma

1. Azure Stack Kullanıcı Portalı'nda tıklatın **+ kaynak Oluştur**.
1. Altında **Azure Marketi**seçin **ağ**.
1. Ağ kaynakları listesinden **Sanal ağ geçidi**’ni seçin.
1. İçinde **adı** alanına **GW1**.
1. **Sanal ağ**'ı seçin.
1. Seçin **Tenant1VNet1** aşağı açılan listeden.
1. Seçin **genel IP adresi**, ardından **genel IP adresi seçin**ve ardından **Yeni Oluştur**.
1. İçinde **adı** alanına **GW1-PiP**ve ardından **Tamam**.
1. **VPN türü** için varsayılan olarak **yol tabanlı** seçili olmalıdır. Bu ayarı tutun.
1. **Abonelik** ve **Konum** seçeneklerinin doğruluğunu onaylayın. **Oluştur**’a tıklayın.

#### <a name="create-the-local-network-gateway"></a>Yerel ağ geçidini oluşturma

VPN bağlantısının diğer ucundaki uzak ağ geçidini yerel ağ geçidi kaynağı tanımlar. Bu örnekte, bağlantı uzak uç LAN alt ExpressRoute yönlendirici arabirimidir. Kiracı 1, 2, gösterildiği uzak adres 10.60.3.255 içindir.

1. Azure Stack Kullanıcı Portalı kullanıcı hesabınızla oturum açın ve seçin **+ kaynak Oluştur**.
1. Altında **Azure Marketi**seçin **ağ**.
1. Kaynak listesinden **yerel ağ geçidi**’ni seçin.
1. İçinde **adı** alanına **ER yönlendirici GW**.
1. İçin **IP adresi** alan, Şekil 2 bakın. 10.60.3.255 ExpressRoute yönlendirici LAN alt arabiriminin Kiracı 1 için IP adresidir. Kendi ortamınızda yönlendiricinizin karşılık gelen arabiriminin IP adresini girin.
1. İçinde **adres alanı** Azure'da bağlanmak istediğiniz sanal ağ adres alanını girin. Alt ağlar için Kiracı 1'de *Şekil 2* aşağıdaki gibidir:

   * Azure Vnet'te hub 192.168.2.0/24 olur.
   * Azure Vnet'te uç 10.100.0.0/16 olur.

   > [!IMPORTANT]
   > Bu örnek, statik yollar Azure Stack ağ geçidi ve ExpressRoute yönlendirici arasında siteden siteye VPN bağlantısı için kullandığınızı varsayar.

1. Doğrulayın, **abonelik**, **kaynak grubu**, ve **konumu** doğrudur. Ardından **Oluştur**’u seçin.

#### <a name="create-the-connection"></a>Bağlantı oluşturma

1. Azure Stack Kullanıcı Portalı'nda seçin **+ kaynak Oluştur**.
1. Altında **Azure Marketi**seçin **ağ**.
1. Kaynak listesinden **Bağlantı**’yı seçin.
1. Altında **Temelleri**, seçin **siteden siteye (IPSec)** olarak **bağlantı türü**.
1. Seçin **abonelik**, **kaynak grubu**, ve **konumu**. **Tamam** düğmesine tıklayın.
1. Altında **ayarları**seçin **sanal ağ geçidi**ve ardından **GW1**.
1. Seçin **yerel ağ geçidi**ve ardından **ER yönlendirici GW**.
1. İçinde **bağlantı adı** alanına **ConnectToAzure**.
1. İçinde **paylaşılan anahtar (PSK)** alanına **abc123** seçip **Tamam**.
1. Altında **özeti**seçin **Tamam**.

#### <a name="get-the-virtual-network-gateway-public-ip-address"></a>Sanal ağ geçidi genel IP adresini alma

Sanal ağ geçidi oluşturduktan sonra ağ geçidinin genel IP adresini alabilirsiniz. Daha sonra dağıtımınız için ihtiyaç durumunda bu adresini not edin. Dağıtımınıza bağlı olarak, bu adresi olarak kullanılan **iç IP adresi**.

1. Azure Stack Kullanıcı Portalı'nda seçin **tüm kaynakları**.
1. Altında **tüm kaynakları**, olan sanal ağ geçidi seçin **GW1** örnekte.
1. Altında **sanal ağ geçidi**seçin **genel bakış** kaynakları listesinden. Alternatif olarak, seçebileceğiniz **özellikleri**.
1. Not istediğiniz IP adresi altında listelenen **genel IP adresi**. Örnek yapılandırma için bu 192.68.102.1 adresidir.

#### <a name="create-a-virtual-machine"></a>Sanal makine oluşturma

Veri trafiği VPN bağlantısı üzerinden test etmek için Azure Stack Vnet'te veri alıp göndermek için sanal makineler gerekir. Bir sanal makine oluşturun ve VM alt ağı için sanal ağınıza dağıtma.

1. Azure Stack Kullanıcı Portalı'nda seçin **+ kaynak Oluştur**.
1. Altında **Azure Marketi**seçin **işlem**.
1. Sanal makine görüntüleri listesinde seçin **Windows Server 2016 Datacenter değerlendirme** görüntü.

   >[!NOTE]
   >Bu makale için kullanılan görüntü kullanılamıyor, farklı bir Windows Server görüntüsüne sağlamak için Azure Stack operatörü isteyin.

1. İçinde **sanal makine oluşturma**seçin **Temelleri**, Anahtar'a **VM01** olarak **adı**.
1. Geçerli kullanıcı adı ve parolayı girin. Bu hesap, oluşturulduktan sonra VM'de oturum açmak için kullanacaksınız.
1. Sağlayan bir **abonelik**, **kaynak grubu**ve **konumu**. **Tamam**’ı seçin.
1. Altında **bir boyut seçin**, bu örneği için bir sanal makine boyutu seçin ve ardından **seçin**.
1. Altında **ayarları**, onaylayın:

   * Sanal ağ **Tenant1VNet1**.
   * Alt kümesine **10.1.1.0/24**.

   Varsayılan ayarları kullanın ve tıklayın **Tamam**.

1. Altında **özeti**, VM yapılandırmasını gözden geçirin ve ardından **Tamam**.

Daha fazla Kiracı eklemek için bu bölümlerdeki uyguladığınız adımları yineleyin:

* [Sanal ağ ve VM alt ağı oluşturma](#create-the-virtual-network-and-vm-subnet)
* [Ağ geçidi alt ağı oluşturma](#create-the-gateway-subnet)
* [Sanal ağ geçidi oluşturma](#create-the-virtual-network-gateway)
* [Yerel ağ geçidi oluşturma](#create-the-local-network-gateway)
* [Bağlantıyı oluşturun](#create-the-connection)
* [Sanal makine oluşturun](#create-a-virtual-machine)

Örnek olarak Kiracı 2 kullanıyorsanız, çakışan önlemek için IP adreslerini değiştirmeyi unutmayın.

### <a name="configure-the-nat-virtual-machine-for-gateway-traversal"></a>Ağ geçidi geçişi için NAT sanal makineyi yapılandırın

> [!IMPORTANT]
> Bu bölüm, yalnızca Azure Stack geliştirme Seti'ni (ASDK) dağıtımlar için değildir. NAT çok düğümlü dağıtımlar için gerekli değildir.

Azure Stack geliştirme Seti'ni müstakil ve fiziksel ana bilgisayar dağıtıldığı ağdan yalıtılmış ' dir. Ağ geçidi bağlı VIP ağının dış değil; Ağ adresi çevirisi (NAT) gerçekleştiren bir yönlendiricinin arkasında gizlenmiştir.

Windows Server sanal makine (AzS-BGPNAT01) yönlendirme ve Uzaktan Erişim Hizmetleri (RRAS) rolünü çalıştıran yönlendiricidir. Her iki End'i bağlanmak siteden siteye VPN bağlantısını etkinleştirmek için AzS-BGPNAT01 sanal makine, NAT yapılandırmanız gerekir.

#### <a name="configure-the-nat"></a>NAT yapılandırma

1. Azure Stack ana bilgisayar yönetici hesabınızla oturum açın.
1. Kopyalayın ve aşağıdaki PowerShell betiğini düzenleyin. Değiştirin `your administrator password` yönetici parolası ve yükseltilmiş bir PowerShell ıse'de betik çalıştırın. Bu betik döndürür, **dış BGPNAT adresi**.

   ```powershell
   cd \AzureStack-Tools-master\connect
   Import-Module .\AzureStack.Connect.psm1
   $Password = ConvertTo-SecureString "your administrator password" `
    -AsPlainText `
    -Force
   Get-AzureStackNatServerAddress `
    -HostComputer "azs-bgpnat01" `
    -Password $Password
   ```

1. NAT'ı yapılandırmak için kopyalayın ve aşağıdaki PowerShell betiğini düzenleyin. Değiştirilecek betiği düzenleyin `External BGPNAT address` ve `Internal IP address` şu örnek değerleri ile:

   * İçin *dış BGPNAT adresi* 10.10.0.62 kullanın
   * İçin *iç IP adresi* 192.168.102.1 kullanın

   Yükseltilmiş bir PowerShell ISE'den aşağıdaki betiği çalıştırın:

   ```powershell
   $ExtBgpNat = 'External BGPNAT address'
   $IntBgpNat = 'Internal IP address'

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
   # Create a static NAT mapping to map the external address to the Gateway public IP address to map the ISAKMP port 500 for PHASE 1 of the IPSEC tunnel.
   Invoke-Command `
    -ComputerName azs-bgpnat01 `
     {Add-NetNatStaticMapping `
      -NatName BGPNAT `
      -Protocol UDP `
      -ExternalIPAddress $Using:ExtBgpNat `
      -InternalIPAddress $Using:IntBgpNat `
      -ExternalPort 500 `
      -InternalPort 500}
   # Configure NAT traversal which uses port 4500 to  establish the complete IPSEC tunnel over NAT devices.
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

## <a name="configure-azure"></a>Azure'ı yapılandırma

Azure Stack'ı yapılandırmayı tamamladıktan sonra Azure kaynaklarını dağıtabilirsiniz. Aşağıdaki şekilde, Azure'da bir kiracı sanal ağına örneği gösterilmektedir. Ağınızda Azure için herhangi bir ad ve adres düzeni'ni kullanabilirsiniz. Ancak, Azure'da ve Azure Stack sanal ağ adres aralığı benzersiz olmalıdır ve çakışmaması gerekir:

*Şekil 3. Azure sanal ağlar*

![Azure sanal ağlar](media/azure-stack-connect-expressroute/AzureArchitecture.png)

Azure Stack'te dağıtılan kaynakların, Azure'da dağıttığınız kaynakları benzerdir. Aşağıdaki bileşenler dağıttığınız:

* Sanal ağlar ve alt ağlar
* Bir ağ geçidi alt ağı
* Bir sanal ağ geçidi
* Bir bağlantı
* Bir ExpressRoute bağlantı hattı

Örnek Azure ağ altyapısı aşağıdaki gibi yapılandırılır:

* Bir standart hub (192.168.2.0/24) ve bağlı bileşen (10.100.0.0./16) sanal ağ modeli. Merkez-uç ağ topolojisi hakkında daha fazla bilgi için bkz: [Azure'da merkez-uç ağ topolojisi uygulama](/azure/architecture/reference-architectures/hybrid-networking/hub-spoke).
* Uç sanal ağı dağıtılan iş yükleri ve ExpressRoute bağlantı hattı merkez sanal ağa bağlanır.
* İki sanal ağ, VNet eşlemesi kullanarak bağlanır.

### <a name="configure-the-azure-vnets"></a>Azure sanal ağları yapılandırma

1. Azure kimlik bilgilerinizle Azure portalında oturum açın.
1. Merkez sanal ağa 192.168.2.0/24 adres aralığını kullanarak oluşturun.
1. 192.168.2.0/25 adres aralığını kullanarak bir alt ağ oluşturun ve 192.168.2.128/27 adres aralığını kullanarak bir ağ geçidi alt ağını ekleyin.
1. Uç oluşturma VNet ve 10.100.0.0/16 kullanarak alt ağ adres aralığı.

Azure'da sanal ağlar oluşturma hakkında daha fazla bilgi için bkz. [sanal ağ oluşturma](../virtual-network/manage-virtual-network.md#create-a-virtual-network).

### <a name="configure-an-expressroute-circuit"></a>Bir ExpressRoute bağlantı hattını yapılandırın

1. ExpressRoute önkoşulları gözden geçirin [ExpressRoute önkoşulları ve denetim listesi](../expressroute/expressroute-prerequisites.md).

1. Bağlantısındaki [oluşturun ve bir ExpressRoute bağlantı hattını değiştirme](../expressroute/expressroute-howto-circuit-portal-resource-manager.md) Azure aboneliğinizi kullanarak ExpressRoute devresi oluşturma.

   >[!NOTE]
   >Bunlar ExpressRoute devreniz kendi sonunda ayarlayabilirsiniz bu nedenle, bağlantı hattı için hizmet anahtarı hizmetinize verin.

1. Bağlantısındaki [oluşturun ve bir ExpressRoute bağlantı hattı için eşleme değiştirme](../expressroute/expressroute-howto-routing-portal-resource-manager.md) bir ExpressRoute bağlantı hattında özel eşdüzey hizmet sağlama yapılandırmak için.

### <a name="create-the-virtual-network-gateway"></a>Sanal ağ geçidini oluşturma

Bağlantısındaki [PowerShell kullanarak ExpressRoute için sanal ağ geçidi yapılandırma](../expressroute/expressroute-howto-add-gateway-resource-manager.md) merkez sanal ağı ExpressRoute için sanal ağ geçidi oluşturma.

### <a name="create-the-connection"></a>Bağlantı oluşturma

ExpressRoute bağlantı hattı merkez sanal ağa bağlamak için adımları izleyin. [bir sanal ağı ExpressRoute devresine bağlama](../expressroute/expressroute-howto-linkvnet-portal-resource-manager.md).

### <a name="peer-the-vnets"></a>Sanal ağları eşleme

Eş merkez ve uç sanal ağları içindeki adımları kullanarak [Azure portalını kullanarak bir sanal ağ eşlemesi oluşturma](../virtual-network/virtual-networks-create-vnetpeering-arm-portal.md). VNet eşlemesi yapılandırırken aşağıdaki seçenekleri kullandığınızdan emin olun:

* Bileşen için hub'ından **ağ geçidi aktarımına izin ver**.
* Gelen hub'ına uç **uzak ağ geçidini kullan**.

### <a name="create-a-virtual-machine"></a>Sanal makine oluşturma

İş yükü sanal makinelerinize uç sanal ağı dağıtın.

Herhangi bir ek Kiracı ilgili kendi ExpressRoute bağlantı hatları Azure'da bağlanmak istediğiniz sanal ağlar için bu adımları yineleyin.

## <a name="configure-the-router"></a>Yönlendirici yapılandırma

ExpressRoute yönlendiriciniz yapılandırmak için aşağıdaki ExpressRoute yönlendirici yapılandırma diyagramda bir kılavuz olarak kullanabilirsiniz. Bu diyagram, karşılık gelen ExpressRoute bağlantı hatları ile iki Kiracı (1 Kiracı ve Kiracı 2) gösterir. Her kiracının kendi VRF (sanal Yönlendirme ve iletme) ExpressRoute yönlendirici LAN ve WAN yan bağlanır. Bu yapılandırma, iki Kiracı arasında uçtan uca yalıtımı sağlar. Yapılandırma örneği takip yönlendirici arabirimlerde kullanılan IP adreslerini not alın.

*Şekil 4. ExpressRoute için yönlendirici yapılandırma*

![ExpressRoute için yönlendirici yapılandırma](media/azure-stack-connect-expressroute/EndToEnd.png)

Azure Stack siteden siteye VPN bağlantısı sonlandırmak için Ikev2 VPN ve BGP destekleyen herhangi bir yönlendirici kullanabilirsiniz. Aynı yönlendirici, bir ExpressRoute bağlantı hattı kullanılarak Azure'a bağlanmak için kullanılır.

Cisco ASR 1000 Series toplama Hizmetleri yönlendirici yapılandırması aşağıda gösterilen ağ altyapısını destekler *ExpressRoute yönlendirici yapılandırması* diyagramı.

```shell
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
 description PRIMARY ExpressRoute Link to AZURE over Equinix
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
 description BACKUP ExpressRoute Link to AZURE over Equinix
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

## <a name="test-the-connection"></a>Bağlantıyı sınama

Siteden siteye bağlantı ve ExpressRoute bağlantı hattı kurduktan sonra bağlantınızı sınayın.

Aşağıdaki çalıştırılabilen ping testleri gerçekleştirin:

* Bir Azure sanal ağınızdaki sanal makinelerin oturum açın ve Azure Stack'te oluşturduğunuz sanal makineye ping.
* Azure Stack ve Azure VNet içinde oluşturduğunuz sanal makinede ping oluşturulan sanal makinelerin birine oturum açın.

>[!NOTE]
>Siteden siteye ve ExpressRoute bağlantıları üzerinden trafik gönderiyorsanız emin olmak için hem uç adresindeki sanal makinenin ayrılmış IP (DIP) adresi ve VIP adresi değil sanal makinenin ping gerekir.

### <a name="allow-icmp-in-through-the-firewall"></a>ICMP, güvenlik duvarı üzerinden izin ver

Varsayılan olarak, Windows Server 2016 güvenlik duvarı üzerinden gelen ICMP paketleri izin vermez. Çalıştırılabilen ping testleri için kullandığınız her sanal makine için gelen ICMP paketleri izin vermeniz gerekir. ICMP için bir güvenlik duvarı kuralı oluşturmak için yükseltilmiş bir PowerShell penceresinde aşağıdaki cmdlet'i çalıştırın:

```powershell
# Create ICMP firewall rule.
New-NetFirewallRule `
  –DisplayName “Allow ICMPv4-In” `
  –Protocol ICMPv4
```

### <a name="ping-the-azure-stack-virtual-machine"></a>Azure Stack sanal makineye ping

1. Bir kiracı hesabı kullanarak Azure Stack kullanıcı portalında oturum açın.

1. Sanal makineyi bulun ve sanal makineyi seçin.

1. **Bağlan**’ı seçin.

1. Yükseltilmiş Windows veya PowerShell komut isteminden, girin **ipconfig/all**. Çıkışında döndürülen IPv4 adresini not alın.

1. Azure vnet'teki sanal makineden ping IPv4 adresi.

   Örnek ortamda 10.1.1.x/24 alt ağdan IPv4 adresidir. Ortamınızda adresi farklı olabilir ancak Kiracı sanal ağ alt ağı için oluşturduğunuz alt ağ içinde olmalıdır.

### <a name="view-data-transfer-statistics"></a>Veri aktarımı istatistiklerini görüntüleme

Ne kadar trafik, bağlantı geçtiğini bilmek istiyorsanız, Azure Stack Kullanıcı Portalı bu bilgileri bulabilirsiniz. Bu ayrıca VPN ve ExpressRoute bağlantıları ping testi verinizi yayınlanmıştı olup olmadığını bulmak için iyi bir yoldur.

1. Kiracı hesabınızı kullanarak Azure Stack kullanıcı portalında oturum açın ve seçin **tüm kaynakları**.
1. VPN ağ geçidiniz için kaynak grubunu bulun ve seçin **bağlantı** nesne türü.
1. Seçin **ConnectToAzure** listeden bağlantıyı.
1. Altında **bağlantıları** > **genel bakış**, istatistiklerini görebilirsiniz **verilerinde** ve **verileri**. Bazı sıfır olmayan değerler görmeniz gerekir.

   ![Verileri ve veri çıkışı](media/azure-stack-connect-expressroute/DataInDataOut.png)

## <a name="next-steps"></a>Sonraki adımlar

[Azure ve Azure uygulama dağıtma yığını](azure-stack-solution-pipeline.md)
