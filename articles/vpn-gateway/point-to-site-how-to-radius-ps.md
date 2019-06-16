---
title: 'Bir bilgisayar, bir sanal ağa noktadan siteye ve RADIUS kimlik doğrulaması kullanarak bağlanın: PowerShell | Azure'
description: Windows ve Mac OS X istemcileri, P2S ve RADIUS kimlik doğrulaması kullanarak bir sanal ağa güvenli bir şekilde bağlanın.
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: conceptual
ms.date: 02/27/2019
ms.author: cherylmc
ms.openlocfilehash: 1096c120b4e7731fabd574c4096e70fe02b6272d
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66146962"
---
# <a name="configure-a-point-to-site-connection-to-a-vnet-using-radius-authentication-powershell"></a>RADIUS kimlik doğrulaması kullanarak bir sanal ağa noktadan siteye bağlantı yapılandırma: PowerShell

Bu makalede, RADIUS kimlik doğrulaması kullanan bir noktadan siteye bağlantı ile VNet oluşturma işlemini gösterir. Bu yapılandırma yalnızca Resource Manager dağıtım modeli için kullanılabilir.

Noktadan Siteye (P2S) VPN ağ geçidi, ayrı bir istemci bilgisayardan sanal ağınıza güvenli bir bağlantı oluşturmanıza olanak sağlar. Noktadan siteye VPN bağlantıları, evden veya bir Konferanstan zaman telecommuting gibi uzak bir konumdan sanal ağınıza bağlanmak istediğinizde faydalıdır. P2S VPN ayrıca, bir sanal ağa bağlanması gereken yalnızca birkaç istemciniz olduğunda Siteden Siteye VPN yerine kullanabileceğiniz yararlı bir çözümüdür.

Windows ve Mac cihazlardan bir P2S VPN bağlantısı başlatılır. Bağlanma istemcileri aşağıdaki kimlik doğrulama yöntemlerini kullanabilir: 

* RADIUS sunucusu
* VPN Gateway yerel sertifika doğrulaması

Bu makalede bir P2S yapılandırması RADIUS sunucusu kullanarak kimlik doğrulaması ile yapılandırmanıza yardımcı olur. Bunun yerine oluşturulan sertifika ve VPN ağ geçidi yerel sertifika doğrulaması kullanarak kimlik doğrulaması yapmak istiyorsanız bkz [VPN ağ geçidi yerel sertifika doğrulaması kullanarak bir sanal ağa noktadan siteye bağlantı yapılandırma](vpn-gateway-howto-point-to-site-rm-ps.md).

![Bağlantı diyagramı - RADIUS](./media/point-to-site-how-to-radius-ps/p2sradius.png)

Noktadan Siteye bağlantılar için bir VPN cihazına veya genel kullanıma yönelik bir IP adresine gerek yoktur. P2S, VPN bağlantısını SSTP (Güvenli Yuva Tünel Protokolü) veya IKEv2 üzerinden oluşturur.

* SSTP yalnızca Windows istemci platformlarında desteklenen SSL tabanlı bir VPN tünelidir. Güvenlik duvarlarından geçebildiği için, herhangi bir yerden Azure’a bağlanmak için ideal bir seçenektir. Sunucu tarafında 1.0, 1.1 ve 1.2 SSTP sürümlerini destekliyoruz. Kullanılacak sürüm, istemci tarafından belirlenir. Windows 8.1 ve sonraki sürümlerinde, SSTP'de varsayılan olarak 1.2 kullanılır.

* IKEv2 VPN, standart tabanlı bir IPsec VPN çözümüdür. IKEv2 VPN, Mac cihazlardan (OSX sürüm 10.11 ve üzeri) bağlantı kurmak için kullanılabilir.

P2S bağlantıları aşağıdakileri gerektirir:

* RouteBased VPN ağ geçidi. 
* Kullanıcı kimlik doğrulamasını işlemek için bir RADIUS sunucusu. Şirket içinde dağıtılması, RADIUS sunucusu olabilir veya Azure sanal ağı.
* Bir VPN istemcisi yapılandırma paketini Vnet'e bağlanacak Windows cihazlar için. Bir VPN istemcisi yapılandırma paketi, P2S üzerinden bağlanmak bir VPN istemcisi için gerekli olan ayarları sağlar.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="aboutad"></a>P2S VPN için Active Directory (AD) etki alanı kimlik hakkında

AD etki alanı kimlik doğrulaması, kuruluş etki alanı kimlik bilgilerini kullanarak Azure'da oturum açmalarını sağlar. Bu AD server ile tümleşen bir RADIUS sunucusu gerektirir. Kuruluşlar, mevcut bir RADIUS dağıtımı da yararlanabilirsiniz.
 
RADIUS sunucusu şirket içinde yer alabilir ya da Azure vnet'inizde. Kimlik doğrulaması sırasında RADIUS sunucusu ve bağlanan cihaz arasında sürekli geçiş ve ileten bir kimlik doğrulama iletilerinde VPN ağ geçidi görür. VPN ağ geçidi RADIUS sunucusuna ulaşabildiğinden olması önemlidir. RADIUS sunucusu şirket içi ise, azure'dan şirket içi siteye bir VPN siteden siteye bağlantı gereklidir.

Active Directory dışında bir RADIUS sunucusu, ayrıca diğer dış kimlik sistemleri ile tümleştirebilirsiniz. Bu noktadan siteye VPN, MFA seçenekleri dahil olmak üzere kimlik doğrulama seçeneklerini bolca yukarı açar. İle tümleşir kimlik sistemlerinin listesini almak için RADIUS sunucusu satıcı belgelerinize bakın.

![Bağlantı diyagramı - RADIUS](./media/point-to-site-how-to-radius-ps/radiusimage.png)

> [!IMPORTANT]
>Bir RADIUS sunucusuna bağlanmak için yalnızca bir VPN siteden siteye bağlantı kullanılabilir şirket içi. ExpressRoute bağlantısı kullanılamaz.
>
>

## <a name="before"></a>Başlamadan önce

Azure aboneliğiniz olduğunu doğrulayın. Henüz Azure aboneliğiniz yoksa [MSDN abonelik avantajlarınızı](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details) etkinleştirebilir veya [ücretsiz bir hesap](https://azure.microsoft.com/pricing/free-trial) için kaydolabilirsiniz.

[!INCLUDE [powershell](../../includes/vpn-gateway-cloud-shell-powershell-about.md)]

### <a name="example"></a>Örnek değerler

Örnek değerleri kullanarak bir test ortamı oluşturabilir veya bu makaledeki örnekleri daha iyi anlamak için bu değerlere bakabilirsiniz. İzlenecek yol olarak adımları kullanıp değerleri değiştirmeden uygulayabilir veya ortamınızı yansıtacak şekilde değiştirebilirsiniz.

* **Adı: VNet1**
* **Adres alanı: 192.168.0.0/16** ve **10.254.0.0/16**<br>Bu örnekte, bu yapılandırmanın birden çok adres alanıyla çalıştığını göstermek için birden fazla adres alanı kullanıyoruz. Ancak, bu yapılandırma için birden çok adres alanı gerekli değildir.
* **Alt ağ adı: Ön uç**
  * **Alt ağ adres aralığı: 192.168.1.0/24**
* **Alt ağ adı: Arka uç**
  * **Alt ağ adres aralığı: 10.254.1.0/24**
* **Alt ağ adı: GatewaySubnet**<br>VPN ağ geçidinin çalışması için Alt Ağ adı olarak *GatewaySubnet*'in kullanılması zorunludur.
  * **Adres aralığı: 192.168.200.0/24** 
* **VPN istemcisi adres havuzu: 172.16.201.0/24**<br>Sanal ağa, bu Noktadan Siteye bağlantıyı kullanarak bağlanan VPN istemcileri, VPN istemci adresi havuzundan bir IP adresi alır.
* **Abonelik:** Birden fazla aboneliğiniz varsa doğru olanı kullandığınızı doğrulayın.
* **Kaynak grubu: TestRG**
* **Konum: East US**
* **DNS sunucusu: IP adresi** ağınız için ad çözümlemesi için kullanmak istediğiniz DNS sunucusunun. (isteğe bağlı)
* **Ağ geçidi adı: Vnet1GW**
* **Genel IP adı: VNet1GWPIP**
* **VPN türü: RouteBased**


## <a name="signin"></a>Oturum açın ve değişkenleri ayarlama

[!INCLUDE [sign in](../../includes/vpn-gateway-cloud-shell-ps-login.md)]

### <a name="declare-variables"></a>Değişkenleri bildirin

Kullanmak istediğiniz değişkenleri bildirin. Aşağıdaki örneği kullanın ve gerektiğinde, değerleri kendi değerlerinizle değiştirin. Alıştırma sırasında PowerShell/Cloud Shell oturumunuzu herhangi bir noktada kapatırsanız, yalnızca kopyalayın ve yeniden değişkenleri yeniden bildirmek için değerleri yapıştırın.

  ```azurepowershell-interactive
  $VNetName  = "VNet1"
  $FESubName = "FrontEnd"
  $BESubName = "Backend"
  $GWSubName = "GatewaySubnet"
  $VNetPrefix1 = "192.168.0.0/16"
  $VNetPrefix2 = "10.254.0.0/16"
  $FESubPrefix = "192.168.1.0/24"
  $BESubPrefix = "10.254.1.0/24"
  $GWSubPrefix = "192.168.200.0/26"
  $VPNClientAddressPool = "172.16.201.0/24"
  $RG = "TestRG"
  $Location = "East US"
  $GWName = "VNet1GW"
  $GWIPName = "VNet1GWPIP"
  $GWIPconfName = "gwipconf"
  ```

## 1. <a name="vnet"></a>Kaynak grubu, sanal ağ ve genel IP oluşturma adresi

Aşağıdaki adımlar, kaynak grubunda üç alt ağ ile bir kaynak grubunu ve sanal ağ oluşturur. Değerlerinizi yerleştirirken ağ geçidi alt ağınızı her zaman özel olarak 'GatewaySubnet' adını önemlidir. Başka bir ad kullanırsanız ağ geçidi oluşturma işleminiz başarısız olur;

1. Bir kaynak grubu oluşturun.

   ```azurepowershell-interactive
   New-AzResourceGroup -Name "TestRG" -Location "East US"
   ```
2. Sanal ağ için alt ağ yapılandırmalarını oluşturup *FrontEnd*, *BackEnd* ve *GatewaySubnet* olarak adlandırın. Bu ön ekler bildirdiğiniz sanal adres alanının parçası olmalıdır.

   ```azurepowershell-interactive
   $fesub = New-AzVirtualNetworkSubnetConfig -Name "FrontEnd" -AddressPrefix "192.168.1.0/24"  
   $besub = New-AzVirtualNetworkSubnetConfig -Name "Backend" -AddressPrefix "10.254.1.0/24"  
   $gwsub = New-AzVirtualNetworkSubnetConfig -Name "GatewaySubnet" -AddressPrefix "192.168.200.0/24"
   ```
3. Sanal ağı oluşturun.

   Bu örnekte, -DnsServer sunucu parametresi isteğe bağlıdır. Bir değer belirtildiğinde yeni bir DNS sunucusu oluşturulmaz. Belirttiğiniz DNS sunucusu IP adresi, sanal ağınızdan bağlandığınız kaynakların adlarını çözümleyebilen bir DNS sunucusu olmalıdır. Bu örnek için özel bir IP adresi kullandık, ancak DNS sunucunuzun IP adresi muhtemelen bu değildir. Kendi değerlerinizi kullandığınızdan emin olun. Belirttiğiniz değer, P2S bağlantı tarafından değil sanal ağa dağıttığınız kaynaklar tarafından kullanılır.

   ```azurepowershell-interactive
   New-AzVirtualNetwork -Name "VNet1" -ResourceGroupName "TestRG" -Location "East US" -AddressPrefix "192.168.0.0/16","10.254.0.0/16" -Subnet $fesub, $besub, $gwsub -DnsServer 10.2.1.3
   ```
4. Bir VPN ağ geçidinin genel bir IP adresi olmalıdır. İlk olarak IP adresi kaynağını istemeniz, sonra sanal ağ geçidinizi oluştururken bu kaynağa başvurmanız gerekir. VPN ağ geçidi oluşturulurken, IP adresi kaynağa dinamik olarak atanır. VPN Gateway hizmeti şu anda yalnızca *Dinamik* Genel IP adresi ayırmayı desteklemektedir. Statik bir Genel IP adresi ataması isteğinde bulunamazsınız. Ancak, bu durum IP adresinin VPN ağ geçidinize atandıktan sonra değiştiği anlamına gelmez. Genel IP adresi, yalnızca ağ geçidi silinip yeniden oluşturulduğunda değişir. VPN ağ geçidiniz üzerinde gerçekleştirilen yeniden boyutlandırma, sıfırlama veya diğer iç bakım/yükseltme işlemleri sırasında değişmez.

   Dinamik olarak atanmış bir genel IP adresi istemek için değişkenleri belirtir.

   ```azurepowershell-interactive
   $vnet = Get-AzVirtualNetwork -Name "VNet1" -ResourceGroupName "TestRG"  
   $subnet = Get-AzVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet 
   $pip = New-AzPublicIpAddress -Name "VNet1GWPIP" -ResourceGroupName "TestRG" -Location "East US" -AllocationMethod Dynamic 
   $ipconf = New-AzVirtualNetworkGatewayIpConfig -Name "gwipconf" -Subnet $subnet -PublicIpAddress $pip
   ```

## 2. <a name="radius"></a>RADIUS sunucunuzun ayarlayın

Oluşturma ve sanal ağ geçidi yapılandırma önce kimlik doğrulaması için RADIUS sunucunuzun doğru şekilde yapılandırılması.

1. Dağıtılan bir RADIUS sunucusuna sahip değilseniz, birini dağıtın. Dağıtım adımları için RADIUS satıcınız tarafından sağlanan kurulum kılavuzuna başvurun.  
2. VPN ağ geçidi üzerinde RADIUS RADIUS istemcisi olarak yapılandırın. RADIUS istemcisini eklerken, sanal ağ, oluşturduğunuz GatewaySubnet belirtin. 
3. RADIUS sunucusu ayarladıktan sonra RADIUS sunucusunun IP adresi ve RADIUS istemcisi RADIUS sunucusuna konuşabilir kullanması gereken paylaşılan gizli anahtarını alın. RADIUS sunucusu Azure Vnet'te VM RADIUS sunucusunun CA IP kullanın.

[Ağ ilkesi sunucusu (NPS)](https://docs.microsoft.com/windows-server/networking/technologies/nps/nps-top) makale AD etki alanı kimlik doğrulaması için Windows RADIUS sunucusu (NPS) yapılandırma hakkında rehberlik sağlar.

## 3. <a name="creategw"></a>VPN ağ geçidi oluşturma

Yapılandırın ve sanal ağınıza ait VPN ağ geçidi oluşturun.

* 'Vpn' - GatewayType olmalıdır ve - VpnType 'RouteBased' olmalıdır.
* Bir VPN ağ geçidi bağlı tamamlanması 45 dakika sürebilir [ağ geçidi SKU'sunu](vpn-gateway-about-vpn-gateway-settings.md#gwsku) , seçin.

```azurepowershell-interactive
New-AzVirtualNetworkGateway -Name $GWName -ResourceGroupName $RG `
-Location $Location -IpConfigurations $ipconf -GatewayType Vpn `
-VpnType RouteBased -EnableBgp $false -GatewaySku VpnGw1
```

## 4. <a name="addradius"></a>RADIUS sunucusu ve istemci adres havuzu ekleme
 
* -RadiusServer adı veya IP adresi belirtilebilir. Adı belirtin ve şirket içi sunucunun bulunduğu, VPN ağ geçidi adını çözümleyebilmesi olmayabilir. Ardından bu durumda, sunucunun IP adresini belirtmek daha iyi. 
* -RadiusSecret RADIUS sunucusunda yapılandırılan eşleşmesi gerekir.
* -VpnClientAddressPool bağlanan VPN istemcileri bir IP adresi alacağı aralıktır. Bağlantıyı kuracağınız şirket içi konum veya bağlanmak istediğiniz sanal ağ ile çakışmayan özel bir IP adresi aralığı kullanın. Yapılandırılmış bir büyüklükte adres havuzu olduğundan emin olun.  

1. Güvenli bir dize için RADIUS gizli anahtar oluşturun.

   ```azurepowershell-interactive
   $Secure_Secret=Read-Host -AsSecureString -Prompt "RadiusSecret"
   ```

2. RADIUS parolayı girmeniz istenir. Girdiğiniz karakterleri görüntülenmeyecek ve bunun yerine ile değiştirilecek "*" karakteri.

   ```azurepowershell-interactive
   RadiusSecret:***
   ```
3. VPN istemcisi adres havuzu ve RADIUS sunucusu bilgilerini ekleyin.

   SSTP yapılandırmaları için:

    ```azurepowershell-interactive
    $Gateway = Get-AzVirtualNetworkGateway -ResourceGroupName $RG -Name $GWName
    Set-AzVirtualNetworkGateway -VirtualNetworkGateway $Gateway `
    -VpnClientAddressPool "172.16.201.0/24" -VpnClientProtocol "SSTP" `
    -RadiusServerAddress "10.51.0.15" -RadiusServerSecret $Secure_Secret
    ```

   Ikev2 yapılandırmaları için:

    ```azurepowershell-interactive
    $Gateway = Get-AzVirtualNetworkGateway -ResourceGroupName $RG -Name $GWName
    Set-AzVirtualNetworkGateway -VirtualNetworkGateway $Gateway `
    -VpnClientAddressPool "172.16.201.0/24" -VpnClientProtocol "IKEv2" `
    -RadiusServerAddress "10.51.0.15" -RadiusServerSecret $Secure_Secret
    ```

   SSTP ve Ikev2 için

    ```azurepowershell-interactive
    $Gateway = Get-AzVirtualNetworkGateway -ResourceGroupName $RG -Name $GWName
    Set-AzVirtualNetworkGateway -VirtualNetworkGateway $Gateway `
    -VpnClientAddressPool "172.16.201.0/24" -VpnClientProtocol @( "SSTP", "IkeV2" ) `
    -RadiusServerAddress "10.51.0.15" -RadiusServerSecret $Secure_Secret
    ```

## 5. <a name="vpnclient"></a>VPN istemcisi yapılandırma paketini indirme ve VPN istemcisi ayarlama

VPN istemci yapılandırması, P2S bağlantısı üzerinden Vnet'e bağlama cihazlara olanak tanır. Bir VPN istemcisi yapılandırma paketini oluşturma ve VPN istemcisi ayarlama hakkında bilgi için bkz: [VPN istemci yapılandırması RADIUS kimlik doğrulaması için oluşturma](point-to-site-vpn-client-configuration-radius.md).

## <a name="connect"></a>6. Azure'a Bağlanma

### <a name="to-connect-from-a-windows-vpn-client"></a>Windows VPN istemcisinden bağlanmak için

1. İstemci bilgisayarda sanal ağınıza bağlanmak için VPN bağlantılarında gezinin ve oluşturduğunuz VPN bağlantısını bulun. Bu VPN bağlantısı sanal ağınızla aynı ada sahiptir. Etki alanı kimlik bilgilerinizi girin ve "Bağlan"'a tıklayın. Yükseltilmiş haklara isteyen bir açılır ileti görüntülenir. Kabul etmek ve kimlik bilgilerini girin.

   ![VPN istemcisinin Azure’a bağlanması](./media/point-to-site-how-to-radius-ps/client.png)
2. Bağlantınız kurulur.

   ![Bağlantı kuruldu](./media/point-to-site-how-to-radius-ps/connected.png)

### <a name="connect-from-a-mac-vpn-client"></a>Mac VPN istemcisinden bağlanmak

Ağ iletişim kutusunda kullanmak istediğiniz istemci profilini bulup **Bağlan**’a tıklayın.

  ![Mac bağlantısı](./media/vpn-gateway-howto-point-to-site-rm-ps/applyconnect.png)

## <a name="verify"></a>Bağlantınızı doğrulamak için

1. VPN bağlantınızın etkin olduğunu doğrulamak için, yükseltilmiş bir komut istemi açın ve *ipconfig/all* komutunu çalıştırın.
2. Sonuçlara bakın. Aldığınız IP adresinin, yapılandırmanızda belirttiğiniz Noktadan Siteye VPN İstemcisi Adres Havuzu'ndaki adreslerden biri olduğuna dikkat edin. Sonuçları şu örneğe benzer:

   ```
   PPP adapter VNet1:
      Connection-specific DNS Suffix .:
      Description.....................: VNet1
      Physical Address................:
      DHCP Enabled....................: No
      Autoconfiguration Enabled.......: Yes
      IPv4 Address....................: 172.16.201.3(Preferred)
      Subnet Mask.....................: 255.255.255.255
      Default Gateway.................:
      NetBIOS over Tcpip..............: Enabled
   ```

P2S bağlantı sorunlarını giderme için bkz: [sorun giderme Azure noktadan siteye bağlantılar](vpn-gateway-troubleshoot-vpn-point-to-site-connection-problems.md).

## <a name="connectVM"></a>Sanal makineye bağlanma

[!INCLUDE [Connect to a VM](../../includes/vpn-gateway-connect-vm-p2s-include.md)]

## <a name="faq"></a>SSS

Bu SSS, P2S için geçerli RADIUS kimlik doğrulaması kullanma

[!INCLUDE [Point-to-Site RADIUS FAQ](../../includes/vpn-gateway-faq-p2s-radius-include.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bağlantınız tamamlandıktan sonra sanal ağlarınıza sanal makineler ekleyebilirsiniz. Daha fazla bilgi için bkz. [Sanal Makineler](https://docs.microsoft.com/azure/). Ağ ve sanal makineler hakkında daha fazla bilgi edinmek için, bkz. [Azure ve Linux VM ağına genel bakış](../virtual-machines/linux/azure-vm-network-overview.md).
