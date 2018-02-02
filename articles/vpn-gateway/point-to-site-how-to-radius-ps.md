---
title: "Bir bilgisayarı noktadan siteye ve RADIUS kimlik doğrulaması kullanarak sanal bir ağa bağlayın: PowerShell | Azure"
description: "Güvenli bir şekilde bir bilgisayar, RADIUS kimlik doğrulaması kullanan bir noktadan siteye VPN gateway bağlantısı oluşturarak Azure sanal ağınıza bağlayın."
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 12/04/2017
ms.author: anzaman
ms.openlocfilehash: 13ae129eefb717f22db25ab29232fe1efe69a8ce
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/01/2018
---
# <a name="configure-a-point-to-site-connection-to-a-vnet-using-radius-authentication-powershell"></a>RADIUS kimlik doğrulaması kullanarak bir sanal ağa noktadan siteye bağlantı yapılandırma: PowerShell

Bu makalede RADIUS kimlik doğrulaması kullanan bir noktadan siteye bağlantı ile bir sanal ağ oluşturulacağını gösterir. Bu yapılandırma yalnızca Resource Manager dağıtım modeli için kullanılabilir.

Noktadan Siteye (P2S) VPN ağ geçidi, ayrı bir istemci bilgisayardan sanal ağınıza güvenli bir bağlantı oluşturmanıza olanak sağlar. Noktadan siteye VPN bağlantıları, evden veya bir Konferanstan zaman telecommuting gibi uzak bir konumdan Vnet'iniz bağlanmak istediğinizde faydalıdır. P2S VPN ayrıca, bir sanal ağa bağlanması gereken yalnızca birkaç istemciniz olduğunda Siteden Siteye VPN yerine kullanabileceğiniz yararlı bir çözümüdür.

Windows ve Mac cihazlardan bir P2S VPN bağlantısı başlatılır. Bağlanma istemcileri aşağıdaki kimlik doğrulama yöntemlerini kullanabilir:

* RADIUS sunucusu
* VPN ağ geçidi yerel sertifika kimlik doğrulaması

Bu makalede P2S yapılandırma RADIUS sunucusu kullanan kimlik doğrulaması ile yapılandırmanıza yardımcı olur. Oluşturulan sertifika ve VPN ağ geçidi yerel sertifika kimlik doğrulaması kullanmayı kimliğini doğrulamak isterseniz bkz [VPN ağ geçidi yerel sertifika kimlik doğrulaması kullanan bir sanal ağa noktadan siteye bağlantı yapılandırma](vpn-gateway-howto-point-to-site-rm-ps.md).

![Bağlantı diyagramı - RADIUS](./media/point-to-site-how-to-radius-ps/p2sradius.png)

Noktadan Siteye bağlantılar için bir VPN cihazına veya genel kullanıma yönelik bir IP adresine gerek yoktur. P2S, VPN bağlantısını SSTP (Güvenli Yuva Tünel Protokolü) veya IKEv2 üzerinden oluşturur.

* SSTP yalnızca Windows istemci platformlarında desteklenen SSL tabanlı bir VPN tünelidir. Güvenlik duvarlarından geçebildiği için, herhangi bir yerden Azure’a bağlanmak için ideal bir seçenektir. Sunucu tarafında 1.0, 1.1 ve 1.2 SSTP sürümlerini destekliyoruz. Kullanılacak sürüm, istemci tarafından belirlenir. Windows 8.1 ve sonraki sürümlerinde, SSTP'de varsayılan olarak 1.2 kullanılır.

* IKEv2 VPN, standart tabanlı bir IPsec VPN çözümüdür. IKEv2 VPN, Mac cihazlardan (OSX sürüm 10.11 ve üzeri) bağlantı kurmak için kullanılabilir.

P2S bağlantıları aşağıdakileri gerektirir:

* RouteBased VPN ağ geçidi. 
* Kullanıcı kimlik doğrulamasını işlemek için bir RADIUS sunucusu. RADIUS sunucusu şirket içinde dağıtılabilir, olabilir ya da Azure VNet içinde.
* Bir VPN istemcisi yapılandırma paketini Vnet'e bağlanacak Windows cihazları için. VPN istemcisi yapılandırma paketini P2S bağlanmak VPN istemcisi için gerekli ayarları sağlar.

## <a name="aboutad"></a>Active Directory (AD) etki alanı kimlik doğrulaması hakkında P2S VPN'ler için

AD etki alanı kimlik doğrulama, kuruluş etki alanı kimlik bilgilerini kullanarak kullanıcıların Azure'da oturum açma olanak tanır. AD sunucusu ile tümleşen bir RADIUS sunucusu gerektirir. Kuruluşlar, kullanıcıların varolan RADIUS dağıtım da kullanabilirsiniz.
 
RADIUS sunucusu şirket bulunabilir veya Azure sanal. Kimlik doğrulaması sırasında VPN ağ geçidi ve geriye RADIUS sunucusu ve bir bağlantı aygıtı arasında doğrudan ve ileten bir kimlik doğrulama iletileri gibi davranır. VPN ağ geçidi RADIUS sunucusuna ulaşabilir olması önemlidir. RADIUS sunucusu şirket içinde ise, şirket içi site arasında Azure VPN siteden siteye bağlantı gereklidir.

Active Directory dışında bir RADIUS sunucusu ayrıca diğer dış kimlik sistemleri ile tümleştirebilirsiniz. Bu, MFA seçenekleri de dahil olmak üzere, noktadan siteye VPN için kimlik doğrulama seçenekleri bolca yukarı açar. İle tümleştirilir kimlik sistemlerinin listesini almak için RADIUS sunucusu satıcı belgelerinize bakın.

![Bağlantı diyagramı - RADIUS](./media/point-to-site-how-to-radius-ps/radiusimage.png)

> [!IMPORTANT]
>Bir RADIUS sunucusuna bağlanmak için yalnızca bir VPN siteden siteye bağlantı kullanılabilir şirket içi. ExpressRoute bağlantısı kullanılamaz.
>
>

## <a name="before"></a>Başlamadan önce

* Azure aboneliğiniz olduğunu doğrulayın. Henüz Azure aboneliğiniz yoksa [MSDN abonelik avantajlarınızı](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details) etkinleştirebilir veya [ücretsiz bir hesap](https://azure.microsoft.com/pricing/free-trial) için kaydolabilirsiniz.

* Azure Resource Manager PowerShell cmdlet'lerinin en son sürümünü yükleyin. PowerShell cmdlet'lerini yükleme hakkında daha fazla bilgi için bkz. [Azure PowerShell'i yükleme ve yapılandırma](/powershell/azure/overview).

### <a name="log-in"></a>Oturum açma

[!INCLUDE [Log in](../../includes/vpn-gateway-ps-login-include.md)]

### <a name="example"></a>Örnek değerler

Örnek değerleri kullanarak bir test ortamı oluşturabilir veya bu makaledeki örnekleri daha iyi anlamak için bu değerlere bakabilirsiniz. İzlenecek yol olarak adımları kullanıp değerleri değiştirmeden uygulayabilir veya ortamınızı yansıtacak şekilde değiştirebilirsiniz.

* **Ad: VNet1**
* **Adres alanı: 192.168.0.0/16** ve **10.254.0.0/16**<br>Bu örnekte, bu yapılandırmanın birden çok adres alanıyla çalıştığını göstermek için birden fazla adres alanı kullanıyoruz. Ancak, bu yapılandırma için birden çok adres alanı gerekli değildir.
* **Alt ağ adı: FrontEnd**
  * **Alt ağ adres aralığı: 192.168.1.0/24**
* **Alt ağ adı: BackEnd**
  * **Alt ağ adres aralığı: 10.254.1.0/24**
* **Alt ağ adı: GatewaySubnet**<br>VPN ağ geçidinin çalışması için Alt Ağ adı olarak *GatewaySubnet*'in kullanılması zorunludur.
  * **Ağ Geçidi Alt Ağ adres aralığı: 192.168.200.0/24** 
* **VPN istemcisi adres havuzu: 172.16.201.0/24**<br>Sanal ağa, bu Noktadan Siteye bağlantıyı kullanarak bağlanan VPN istemcileri, VPN istemci adresi havuzundan bir IP adresi alır.
* **Abonelik:** Birden fazla aboneliğiniz varsa doğru aboneliği kullandığınızdan emin olun.
* **Kaynak Grubu: TestRG**
* **Konum: Doğu ABD**
* **DNS sunucusu: IP adresi** ağınız için ad çözümlemesi için kullanmak istediğiniz DNS sunucusunun. (isteğe bağlı)
* **Ağ Geçidi Adı: Vnet1GW**
* **Ortak IP adı: VNet1GWPIP**
* **VpnType: RouteBased** 

## 1. <a name="vnet"></a>Kaynak grubu, VNet ve genel IP adresi

Aşağıdaki adımları üç alt ağ ile kaynak grubundaki bir kaynak grubu ve sanal ağ oluşturun. Değerleri değiştirerek, ağ geçidi alt ağınızı her zaman özellikle 'GatewaySubnet' adını önemlidir. Başka bir ad kullanırsanız ağ oluşturma işleminiz başarısız olur;

1. Bir kaynak grubu oluşturun.

  ```powershell
  New-AzureRmResourceGroup -Name "TestRG" -Location "East US"
  ```
2. Sanal ağ için alt ağ yapılandırmalarını oluşturup *FrontEnd*, *BackEnd* ve *GatewaySubnet* olarak adlandırın. Bu ön ekler bildirdiğiniz sanal adres alanının parçası olmalıdır.

  ```powershell
  $fesub = New-AzureRmVirtualNetworkSubnetConfig -Name "FrontEnd" -AddressPrefix "192.168.1.0/24"  
  $besub = New-AzureRmVirtualNetworkSubnetConfig -Name "Backend" -AddressPrefix "10.254.1.0/24"  
  $gwsub = New-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -AddressPrefix "192.168.200.0/24"
  ```
3. Sanal ağı oluşturun.

  Bu örnekte, -DnsServer sunucu parametresi isteğe bağlıdır. Bir değer belirtildiğinde yeni bir DNS sunucusu oluşturulmaz. Belirttiğiniz DNS sunucusu IP adresi, sanal ağınızdan bağlandığınız kaynakların adlarını çözümleyebilen bir DNS sunucusu olmalıdır. Bu örnek için özel bir IP adresi kullandık, ancak DNS sunucunuzun IP adresi muhtemelen bu değildir. Kendi değerlerinizi kullandığınızdan emin olun. Belirttiğiniz değerle değil P2S bağlantısı tarafından Vnet'e dağıttığınız kaynakların tarafından kullanılır.

  ```powershell
  New-AzureRmVirtualNetwork -Name "VNet1" -ResourceGroupName "TestRG" -Location "East US" -AddressPrefix "192.168.0.0/16","10.254.0.0/16" -Subnet $fesub, $besub, $gwsub -DnsServer 10.2.1.3
  ```
4. Bir VPN ağ geçidinin genel bir IP adresi olmalıdır. İlk olarak IP adresi kaynağını istemeniz, sonra sanal ağ geçidinizi oluştururken bu kaynağa başvurmanız gerekir. VPN ağ geçidi oluşturulurken, IP adresi kaynağa dinamik olarak atanır. VPN Gateway hizmeti şu anda yalnızca *Dinamik* Genel IP adresi ayırmayı desteklemektedir. Statik bir Genel IP adresi ataması isteğinde bulunamazsınız. Ancak, bu durum IP adresinin VPN ağ geçidinize atandıktan sonra değiştiği anlamına gelmez. Genel IP adresi, yalnızca ağ geçidi silinip yeniden oluşturulduğunda değişir. VPN ağ geçidiniz üzerinde gerçekleştirilen yeniden boyutlandırma, sıfırlama veya diğer iç bakım/yükseltme işlemleri sırasında değişmez.

  Dinamik olarak atanmış bir ortak IP adresi istemek için değişkenleri belirtir.

  ```powershell
  $vnet = Get-AzureRmVirtualNetwork -Name "VNet1" -ResourceGroupName "TestRG"  
  $subnet = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet 
  $pip = New-AzureRmPublicIpAddress -Name "VNet1GWPIP" -ResourceGroupName "TestRG" -Location "East US" -AllocationMethod Dynamic 
  $ipconf = New-AzureRmVirtualNetworkGatewayIpConfig -Name "gwipconf" -Subnet $subnet -PublicIpAddress $pip
  ```

## 2. <a name="radius"></a>RADIUS sunucusunu ayarlama

Oluşturma ve sanal ağ geçidi yapılandırma önce RADIUS sunucunuz kimlik doğrulaması için doğru şekilde yapılandırılmalıdır.

1. Dağıtılan bir RADIUS sunucusuna sahip değilseniz, birini dağıtın. Dağıtım adımları, için RADIUS satıcınız tarafından sağlanan Kurulum Kılavuzu'na bakın.  
2. RADIUS sunucusunda bir RADIUS istemcisi olarak VPN ağ geçidi yapılandırın. Bu RADIUS istemcisi eklerken, oluşturduğunuz GatewaySubnet sanal ağ belirtin. 
3. RADIUS hizmet vermemesini ayarladıktan sonra RADIUS sunucusunun IP adresini ve RADIUS istemcileri RADIUS sunucusuna anlaşmak için kullanması gereken paylaşılan gizliliği alın. RADIUS sunucusu Azure vnet'se RADIUS sunucusunun VM CA IP kullanın.

[Ağ ilkesi sunucusu (NPS)](https://docs.microsoft.com/windows-server/networking/technologies/nps/nps-top) makale AD etki alanı kimlik doğrulaması için Windows RADIUS sunucusu (NPS) yapılandırma hakkında yönergeler sağlar.

## 3. <a name="creategw"></a>VPN ağ geçidi oluşturma

Yapılandırın ve VPN ağ geçidi için sanal ağınızı oluşturun.

* -GatewayType 'Vpn' olmalı ve - VpnType 'RouteBased' olmalıdır.
* Bir VPN ağ geçidi bağlı olarak tamamlamak için 45 dakika sürebilir [ağ geçidi SKU'su](vpn-gateway-about-vpn-gateway-settings.md#gwsku) seçin.

```powershell
New-AzureRmVirtualNetworkGateway -Name $GWName -ResourceGroupName $RG `
-Location $Location -IpConfigurations $ipconf -GatewayType Vpn `
-VpnType RouteBased -EnableBgp $false -GatewaySku VpnGw1
```

## 4. <a name="addradius"></a>RADIUS sunucusu ve istemci adres havuzu ekleme
 
* -RadiusServer adına veya IP adresine göre belirtilebilir. Adı belirtirseniz ve şirket içi sunucusunun bulunduğu, VPN ağ geçidi adını çözümleyebilmesi olmayabilir. Ardından bu durumda, sunucunun IP adresini belirtin en iyisidir. 
* -RadiusSecret RADIUS sunucusu olarak yapılandırılmış eşleşmesi gerekir.
* -VpnCientAddressPool bağlanan VPN istemcileri bir IP adresi aldıkları aralıktır. Bağlantıyı kuracağınız şirket içi konum veya bağlanmak istediğiniz sanal ağ ile çakışmayan özel bir IP adresi aralığı kullanın. Yapılandırılmış bir büyüklükte adres havuzu olduğundan emin olun.  

1. Güvenli bir dize için RADIUS gizli oluşturun.

  ```powershell
  $Secure_Secret=Read-Host -AsSecureString -Prompt "RadiusSecret"
  ```

2. RADIUS parolayı girmeniz istenir. Girdiğiniz karakterler görüntülenmeyecek ve bunun yerine tarafından değiştirilecek "*" karakteri.

  ```powershell
  RadiusSecret:***
  ```
3. VPN istemci adres havuzu ve RADIUS sunucusu bilgilerini ekleyin.

  SSTP yapılandırmalar için:

    ```powershell
    $Gateway = Get-AzureRmVirtualNetworkGateway -ResourceGroupName $RG -Name $GWName
    Set-AzureRmVirtualNetworkGateway -VirtualNetworkGateway $Gateway `
    -VpnClientAddressPool "172.16.201.0/24" -VpnClientProtocol "SSTP" `
    -RadiusServerAddress "10.51.0.15" -RadiusServerSecret $Secure_Secret
    ```

  Ikev2 yapılandırmalar için:

    ```powershell
    $Gateway = Get-AzureRmVirtualNetworkGateway -ResourceGroupName $RG -Name $GWName
    Set-AzureRmVirtualNetworkGateway -VirtualNetworkGateway $Gateway `
    -VpnClientAddressPool "172.16.201.0/24" -VpnClientProtocol "IKEv2" `
    -RadiusServerAddress "10.51.0.15" -RadiusServerSecret $Secure_Secret
    ```

  SSTP + Ikev2 için

    ```powershell
    $Gateway = Get-AzureRmVirtualNetworkGateway -ResourceGroupName $RG -Name $GWName
    Set-AzureRmVirtualNetworkGateway -VirtualNetworkGateway $Gateway `
    -VpnClientAddressPool "172.16.201.0/24" -VpnClientProtocol @( "SSTP", "IkeV2" ) `
    -RadiusServerAddress "10.51.0.15" -RadiusServerSecret $Secure_Secret
    ```

## 5. <a name="vpnclient"></a>VPN istemcisi yapılandırma paketini indirin ve VPN istemcisi ayarlama

VPN istemci yapılandırmasında bir sanal ağa bir P2S bağlantısı üzerinden cihazları sağlar. VPN istemcisi yapılandırma paketini oluşturma ve VPN istemcisi ayarlama hakkında bilgi için bkz: [RADIUS kimlik doğrulaması için bir VPN istemci yapılandırma oluşturma](point-to-site-vpn-client-configuration-radius.md).

## <a name="connect"></a>6. Azure'a Bağlanma

### <a name="to-connect-from-a-windows-vpn-client"></a>Windows VPN istemcisinden bağlanmak için

1. İstemci bilgisayarda sanal ağınıza bağlanmak için VPN bağlantılarında gezinin ve oluşturduğunuz VPN bağlantısını bulun. Bu VPN bağlantısı sanal ağınızla aynı ada sahiptir. Etki alanı kimlik bilgilerinizi girin ve 'Bağlan' tıklayın. Yükseltilmiş haklara isteyen bir açılır ileti görüntülenir. Kabul ediyor ve kimlik bilgilerini girin.

  ![VPN istemcisinin Azure’a bağlanması](./media/point-to-site-how-to-radius-ps/client.png)
2. Bağlantınız kurulur.

  ![Bağlantı kuruldu](./media/point-to-site-how-to-radius-ps/connected.png)

### <a name="connect-from-a-mac-vpn-client"></a>Mac VPN istemciden bağlanma

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

## <a name="connectVM"></a>Sanal makineye bağlanma

[!INCLUDE [Connect to a VM](../../includes/vpn-gateway-connect-vm-p2s-include.md)]

## <a name="faq"></a>SSS

Bu SSS P2S için geçerlidir RADIUS kimlik doğrulaması kullanma

[!INCLUDE [Point-to-Site RADIUS FAQ](../../includes/vpn-gateway-faq-p2s-radius-include.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bağlantınız tamamlandıktan sonra sanal ağlarınıza sanal makineler ekleyebilirsiniz. Daha fazla bilgi için bkz. [Sanal Makineler](https://docs.microsoft.com/azure/#pivot=services&panel=Compute). Ağ ve sanal makineler hakkında daha fazla bilgi edinmek için, bkz. [Azure ve Linux VM ağına genel bakış](../virtual-machines/linux/azure-vm-network-overview.md).
