---
title: "Noktadan Siteye bağlantısı ve yerel Azure sertifika doğrulaması kullanarak bir bilgisayarı Azure sanal ağına bağlama: PowerShell | Microsoft Docs"
description: "VPN ağ geçidi yerel Azure sertifika doğrulaması ile Noktadan Siteye VPN ağ geçidi bağlantısı oluşturarak bir bilgisayarı sanal ağınıza güvenli bir şekilde bağlayın. Bu makale Resource Manager dağıtım modelleri için geçerlidir ve PowerShell kullanır."
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 3eddadf6-2e96-48c4-87c6-52a146faeec6
ms.service: vpn-gateway
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/25/2017
ms.author: cherylmc
ms.translationtype: HT
ms.sourcegitcommit: 7dceb7bb38b1dac778151e197db3b5be49dd568a
ms.openlocfilehash: 8c4b2d578a8a586fc63c972ab5da694b2dd9d571
ms.contentlocale: tr-tr
ms.lasthandoff: 09/25/2017

---
# <a name="configure-a-point-to-site-connection-to-a-vnet-using-native-azure-certificate-authentication-powershell"></a>Yerel Azure sertifika doğrulaması kullanarak Noktadan Siteye sanal ağ bağlantısını yapılandırma: PowerShell

Bu makalede PowerShell kullanılarak Resource Manager dağıtım modelinde Noktadan Siteye bağlantı ile sanal ağ oluşturma işlemi gösterilmektedir. Bu yapılandırma, kimlik doğrulaması için sertifikaları kullanır. Bu yapılandırmada sertifika doğrulaması, bir RADIUS sunucusu yerine Azure VPN ağ geçidi tarafından gerçekleştirilir. Ayrıca aşağıdaki listeden farklı bir seçenek belirtip farklı bir dağıtım aracı veya dağıtım modeli kullanarak da bu yapılandırmayı oluşturabilirsiniz:

> [!div class="op_single_selector"]
> * [Azure portal](vpn-gateway-howto-point-to-site-resource-manager-portal.md)
> * [PowerShell](vpn-gateway-howto-point-to-site-rm-ps.md)
> * [Azure portal (klasik)](vpn-gateway-howto-point-to-site-classic-azure-portal.md)
>
>

Noktadan Siteye (P2S) VPN ağ geçidi, ayrı bir istemci bilgisayardan sanal ağınıza güvenli bir bağlantı oluşturmanıza olanak sağlar. Noktadan Siteye VPN bağlantıları, ev veya bir konferans gibi uzak bir noktadan Vnet'inize bağlanmak istediğinizde faydalıdır. P2S VPN ayrıca, bir sanal ağa bağlanması gereken yalnızca birkaç istemciniz olduğunda Siteden Siteye VPN yerine kullanabileceğiniz yararlı bir çözümüdür. Windows ve Mac cihazlardan bir P2S VPN bağlantısı başlatılır. 

Bağlanma istemcileri aşağıdaki kimlik doğrulama yöntemlerini kullanabilir:

* RADIUS sunucusu - Şu anda Önizleme sürümündedir
* VPN Gateway yerel Azure sertifika doğrulaması

Bu makale, yerel Azure sertifika doğrulamasını kullanarak kimlik doğrulaması ile bir P2S yapılandırmanıza yardımcı olur. Bağlanan kullanıcıların kimliğini doğrulamak için RADIUS kullanmak istiyorsanız bkz. [RADIUS kimlik doğrulaması kullanarak P2S](point-to-site-how-to-radius-ps.md).

![Bir bilgisayarı Azure sanal ağına bağlama - Noktadan Siteye bağlantı diyagramı](./media/vpn-gateway-howto-point-to-site-rm-ps/p2snativeps.png)

Noktadan Siteye bağlantılar için bir VPN cihazına veya genel kullanıma yönelik bir IP adresine gerek yoktur. P2S, VPN bağlantısını SSTP (Güvenli Yuva Tünel Protokolü) veya IKEv2 üzerinden oluşturur.

* SSTP yalnızca Windows istemci platformlarında desteklenen SSL tabanlı bir VPN tünelidir. Güvenlik duvarlarından geçebildiği için, herhangi bir yerden Azure’a bağlanmak için ideal bir seçenektir. Sunucu tarafında 1.0, 1.1 ve 1.2 SSTP sürümlerini destekliyoruz. Kullanılacak sürüm, istemci tarafından belirlenir. Windows 8.1 ve sonraki sürümlerinde, SSTP'de varsayılan olarak 1.2 kullanılır.

* IKEv2 VPN, standart tabanlı bir IPsec VPN çözümüdür. IKEv2 VPN, Mac cihazlardan (OSX sürüm 10.11 ve üzeri) bağlantı kurmak için kullanılabilir. IKEv2 şu anda Önizleme sürümündedir.

>[!NOTE]
>P2S için IKEv2 şu anda Önizleme sürümündedir.
>

Noktadan Siteye yerel Azure sertifika doğrulaması bağlantıları aşağıdakileri gerektirir:

* RouteBased VPN ağ geçidi.
* Azure’a yüklenmiş bir kök sertifikanın ortak anahtarı (.cer dosyası). Sertifika karşıya yüklendikten sonra güvenilen bir sertifika olarak kabul edilir ve kimlik doğrulaması için kullanılır.
* Kök sertifikadan oluşturulmuş ve VNet’e bağlanacak her bir istemci bilgisayara yüklenmiş bir istemci sertifikası. Bu sertifika, istemci kimlik doğrulaması için kullanılır.
* VPN istemcisi yapılandırması. VPN istemci yapılandırması dosyaları, istemcinin sanal ağa bağlanması için gerekli bilgileri içerir. Dosyalar, işletim sisteminde yerel olarak bulunan VPN istemcisini yapılandırır. Bağlanan her istemcinin yapılandırma dosyalarındaki ayarlar kullanılarak yapılandırılması gerekir.

Noktadan Siteye bağlantılar hakkında daha fazla bilgi edinmek için bkz. [Noktadan Siteye bağlantılar hakkında](point-to-site-about.md).

## <a name="before-you-begin"></a>Başlamadan önce

* Azure aboneliğiniz olduğunu doğrulayın. Henüz Azure aboneliğiniz yoksa [MSDN abonelik avantajlarınızı](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details) etkinleştirebilir veya [ücretsiz bir hesap](https://azure.microsoft.com/pricing/free-trial) için kaydolabilirsiniz.
* Resource Manager PowerShell cmdlet'lerinin en son sürümünü yükleyin. PowerShell cmdlet'lerini yükleme hakkında daha fazla bilgi için bkz. [Azure PowerShell'i yükleme ve yapılandırma](/powershell/azure/overview).

### <a name="example"></a>Örnek değerler

Örnek değerleri kullanarak bir test ortamı oluşturabilir veya bu makaledeki örnekleri daha iyi anlamak için bu değerlere bakabilirsiniz. Değişkenler makalenin [1](#declare). bölümünde ayarlanır. İzlenecek yol olarak adımları kullanıp değerleri değiştirmeden uygulayabilir veya ortamınızı yansıtacak şekilde değiştirebilirsiniz.

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
* DNS Sunucusu: Ad çözümlemesi için kullanmak istediğiniz **DNS sunucusunun IP adresi**. (isteğe bağlı)
* **Ağ Geçidi Adı: Vnet1GW**
* **Ortak IP adı: VNet1GWPIP**
* **VpnType: RouteBased** 

## <a name="declare"></a>1. Oturum açma ve değişkenleri ayarlama

Bu bölümde oturum açıp bu yapılandırma için kullanılan değerleri bildirirsiniz. Belirtilen değerler örnek betiklerde kullanılır. Değerleri, ortamınızı yansıtacak şekilde değiştirin. Veya, bildirilen değerleri kullanın ve bir alıştırma olarak adımları uygulayın.

1. PowerShell konsolunuzu yükseltilmiş ayrıcalıklarla açın ve Azure hesabınızda oturum açın. Bu cmdlet, oturum açma kimlik bilgilerinizi girmenizi ister. Oturum açtıktan sonra, Azure PowerShell'de kullanabilmeniz için hesap ayarlarınızı indirir.

  ```powershell
  Login-AzureRmAccount
  ```
2. Azure aboneliklerinizin bir listesini alın.

  ```powershell
  Get-AzureRmSubscription
  ```
3. Kullanmak istediğiniz aboneliği belirtin.

  ```powershell
  Select-AzureRmSubscription -SubscriptionName "Name of subscription"
  ```
4. Kullanmak istediğiniz değişkenleri bildirin. Aşağıdaki örneği kullanın ve gerektiğinde, değerleri kendi değerlerinizle değiştirin.

  ```powershell
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

## <a name="ConfigureVNet"></a>2. Sanal ağ yapılandırma

1. Bir kaynak grubu oluşturun.

  ```powershell
  New-AzureRmResourceGroup -Name $RG -Location $Location
  ```
2. Sanal ağ için alt ağ yapılandırmalarını oluşturup *FrontEnd*, *BackEnd* ve *GatewaySubnet* olarak adlandırın. Bu ön ekler bildirdiğiniz sanal adres alanının parçası olmalıdır.

  ```powershell
  $fesub = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName -AddressPrefix $FESubPrefix
  $besub = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName -AddressPrefix $BESubPrefix
  $gwsub = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName -AddressPrefix $GWSubPrefix
  ```
3. Sanal ağı oluşturun.

  Bu örnekte, -DnsServer sunucu parametresi isteğe bağlıdır. Bir değer belirtildiğinde yeni bir DNS sunucusu oluşturulmaz. Belirttiğiniz DNS sunucusu IP adresi, sanal ağınızdan bağlandığınız kaynakların adlarını çözümleyebilen bir DNS sunucusu olmalıdır. Bu örnek için özel bir IP adresi kullandık, ancak DNS sunucunuzun IP adresi muhtemelen bu değildir. Kendi değerlerinizi kullandığınızdan emin olun. Belirttiğiniz değer, P2S bağlantısı veya VPN istemcisi tarafından değil, sanal ağa dağıttığınız kaynaklar tarafından kullanılıyor.

  ```powershell
  New-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $RG -Location $Location -AddressPrefix $VNetPrefix1,$VNetPrefix2 -Subnet $fesub, $besub, $gwsub -DnsServer 10.2.1.3
  ```
4. Oluşturduğunuz sanal ağa ilişkin değişkenleri belirtin.

  ```powershell
  $vnet = Get-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $RG
  $subnet = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet
  ```
5. Bir VPN ağ geçidinin genel bir IP adresi olmalıdır. İlk olarak IP adresi kaynağını istemeniz, sonra sanal ağ geçidinizi oluştururken bu kaynağa başvurmanız gerekir. VPN ağ geçidi oluşturulurken, IP adresi kaynağa dinamik olarak atanır. VPN Gateway hizmeti şu anda yalnızca *Dinamik* Genel IP adresi ayırmayı desteklemektedir. Statik bir Genel IP adresi ataması isteğinde bulunamazsınız. Ancak, bu durum IP adresinin VPN ağ geçidinize atandıktan sonra değiştiği anlamına gelmez. Genel IP adresi, yalnızca ağ geçidi silinip yeniden oluşturulduğunda değişir. VPN ağ geçidiniz üzerinde gerçekleştirilen yeniden boyutlandırma, sıfırlama veya diğer iç bakım/yükseltme işlemleri sırasında değişmez.

  Dinamik olarak atanan bir genel IP adresi isteyin.

  ```powershell
  $pip = New-AzureRmPublicIpAddress -Name $GWIPName -ResourceGroupName $RG -Location $Location -AllocationMethod Dynamic
  $ipconf = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GWIPconfName -Subnet $subnet -PublicIpAddress $pip
  ```

## <a name="creategateway"></a>3. VPN ağ geçidini oluşturma

Sanal ağınız için sanal ağ geçidini yapılandırın ve oluşturun.

* *-GatewayType* değeri **Vpn** ve *-VpnType* değeri **RouteBased** olmalıdır.
* -VpnClientProtocols, etkinleştirmek istediğiniz tünel türlerini belirtmek için kullanılır. **SSTP** ve **IKEv2** olmak üzere iki tünel seçeneği bulunur. Birini veya ikisini birden etkinleştirmeyi seçebilirsiniz. İkisini birden etkinleştirmek istiyorsanız, her iki adı da virgülle ayrılmış olarak belirtin. Android ve Linux üzerindeki Strongswan istemcisi ile iOS ve OSX üzerindeki yerel IKEv2 VPN istemcisi, bağlanmak için yalnızca IKEv2 kullanır. Windows istemcileri önce IKEv2’yi dener ve bağlanamazsa SSTP’ye döner.
* Bir VPN ağ geçidi işleminin tamamlanması, seçtiğiniz [ağ geçidi sku'suna](vpn-gateway-about-vpn-gateway-settings.md) bağlı olarak 45 dakikaya kadar sürebilir. Bu örnekte şu anda Önizleme sürümünde olan IKEv2 uygulamasını kullanıyoruz.

```powershell
New-AzureRmVirtualNetworkGateway -Name $GWName -ResourceGroupName $RG `
-Location $Location -IpConfigurations $ipconf -GatewayType Vpn `
-VpnType RouteBased -EnableBgp $false -GatewaySku VpnGw1 -VpnClientProtocols "IKEv2"
```

## <a name="addresspool"></a>4. VPN istemcisi adres havuzunu ekleme

VPN ağ geçidi oluşturduktan sonra VPN istemcisi adres havuzunu ekleyebilirsiniz. VPN istemcisi adres havuzu, VPN istemcilerinin bağlanırken bir IP adresi aldığı aralıktır. Bağlandığınız şirket içi konum veya bağlanmak istediğiniz sanal ağ ile çakışmayan özel bir IP adresi aralığı kullanın. Bu örnekte, VPN istemcisi adres havuzu 1. Adımda bir [değişken](#declare) olarak bildirilir.

```powershell
$Gateway = Get-AzureRmVirtualNetworkGateway -ResourceGroupName $RG -Name $GWName
Set-AzureRmVirtualNetworkGateway -VirtualNetworkGateway $Gateway -VpnClientAddressPool $VPNClientAddressPool
```

## <a name="Certificates"></a>5. İstemci sertifikaları oluşturma

Noktadan Siteye VPN’lerde VPN istemcilerinin kimlik doğrulamasını yapmak için Azure tarafından sertifikalar kullanılır. Kök sertifikanın ortak anahtar bilgilerini Azure'a yükleyin. Bundan sonra ortak anahtar, 'güvenilir' olarak kabul edilir. Güvenilir kök sertifikadan istemci sertifikaları oluşturulmalı ve sonra Sertifikalar-Geçerli Kullanıcı/Kişisel sertifika deposundaki her bir istemci bilgisayara yüklenmelidir. Sertifika, sanal ağ ile bağlantı başlattığında istemcinin kimliğini doğrulamak için kullanılır. 

Otomatik olarak imzalanan sertifikalar kullanıyorsanız bu sertifikaların belirli parametreler kullanılarak oluşturulması gerekir. [PowerShell ve Windows 10](vpn-gateway-certificates-point-to-site.md)'a yönelik yönergeler yardımıyla, otomatik olarak imzalanan sertifika oluşturabilir veya Windows 10'a sahip değilseniz [MakeCert](vpn-gateway-certificates-point-to-site-makecert.md) kullanabilirsiniz. Otomatik olarak imzalanan kök sertifika ve istemci sertifikası oluştururken yönergelerdeki adımları uygulamanız önemlidir. Aksi halde, oluşturduğunuz sertifikalar P2S bağlantılarıyla uyumlu olmaz ve bağlantı hatası alırsınız.

### <a name="cer"></a>1. Kök sertifikaya ilişkin .cer dosyasını alma

[!INCLUDE [vpn-gateway-basic-vnet-rm-portal](../../includes/vpn-gateway-p2s-rootcert-include.md)]


### <a name="generate"></a>2. İstemci sertifikası oluşturma

[!INCLUDE [vpn-gateway-basic-vnet-rm-portal](../../includes/vpn-gateway-p2s-clientcert-include.md)]

## <a name="upload"></a>6. Kök sertifikanın ortak anahtar bilgilerini karşıya yükleme

VPN ağ geçidini oluşturma işleminin tamamlandığını doğrulayın. İşlem tamamlandıktan sonra, güvenilen kök sertifikanın .cer dosyasını (ortak anahtar bilgilerini içerir) Azure’a yükleyebilirsiniz. Bir .cer dosyası karşıya yüklendikten sonra Azure, güvenilir kök sertifikadan oluşturulmuş bir istemci sertifikasının yüklü olduğu istemcilerin kimliklerini doğrulamak için bu dosyayı kullanabilir. Daha sonra gerekirse, toplam 20 adede kadar güvenilir kök sertifika dosyasını karşıya yükleyebilirsiniz.

1. Sertifika adınızın değişkenini tanımlamak için değeri kendi değerinizle değiştirin.

  ```powershell
  $P2SRootCertName = "P2SRootCert.cer"
  ```
2. Dosya yolunu kendi dosya yolunuzla değiştirin ve ardından cmdlet'leri çalıştırın.

  ```powershell
  $filePathForCert = "C:\cert\P2SRootCert.cer"
  $cert = new-object System.Security.Cryptography.X509Certificates.X509Certificate2($filePathForCert)
  $CertBase64 = [system.convert]::ToBase64String($cert.RawData)
  $p2srootcert = New-AzureRmVpnClientRootCertificate -Name $P2SRootCertName -PublicCertData $CertBase64
  ```
3. Ortak anahtar bilgilerini Azure'a yükleyin. Sertifika bilgileri karşıya yüklendikten sonra, Azure bunu bir güvenilen kök sertifika olarak dikkate alır.

  ```powershell
  Add-AzureRmVpnClientRootCertificate -VpnClientRootCertificateName $P2SRootCertName -VirtualNetworkGatewayname "VNet1GW" -ResourceGroupName "TestRG" -PublicCertData $CertBase64
  ```

## <a name="clientcertificate"></a>7. Dışarı aktarılan bir istemci sertifikasını yükleme

İstemci sertifikalarını oluşturmak için kullandığınız bilgisayardan farklı bir istemci bilgisayarda bir P2S bağlantı oluşturmak istiyorsanız, bir istemci sertifikası yüklemeniz gerekir. Bir istemci sertifikası yüklenirken, istemci sertifikası dışarı aktarılırken oluşturulan parola gerekir.

İstemci sertifikasının tüm sertifika zinciriyle birlikte .pfx dosyası olarak (varsayılan seçenek) dışarı aktarılmadığından emin olun. Aksi takdirde kök sertifika bilgileri istemci bilgisayarda bulunmaz ve istemci, kimliğini düzgün bir biçimde doğrulayamaz. 

Yükleme adımları için bkz. [İstemci sertifikası yükleme](point-to-site-how-to-vpn-client-install-azure-cert.md).

## <a name="clientconfig"></a>8. Yerel VPN istemcisini yapılandırma

VPN istemcisi yapılandırma dosyaları, P2S bağlantısı üzerinden bir sanal ağa bağlanmak üzere cihazları yapılandırmak için gereken ayarları içerir. VPN istemcisi yapılandırma dosyalarını oluşturma ve yükleme yönergeleri için bkz. [Yerel Azure sertifika doğrulaması P2S yapılandırmaları için VPN istemci yapılandırma dosyalarını oluşturma ve yükleme](point-to-site-vpn-client-configuration-azure-cert.md).

## <a name="connect"></a>9. Azure'a Bağlanma

### <a name="to-connect-from-a-windows-vpn-client"></a>Windows VPN istemcisinden bağlanmak için

1. İstemci bilgisayarda sanal ağınıza bağlanmak için VPN bağlantılarında gezinin ve oluşturduğunuz VPN bağlantısını bulun. Bu VPN bağlantısı sanal ağınızla aynı ada sahiptir. **Bağlan**'a tıklayın. Sertifika kullanımına ilişkin bir açılır ileti görüntülenebilir. Yükseltilmiş ayrıcalıklar kullanmak için **Devam**’a tıklayın. 
2. **Bağlantı** durum sayfasında **Bağlan**'a tıklayarak bağlantıyı başlatın. Bir **Sertifika Seç** ekranı çıkarsa, gösterilen istemci sertifikasının bağlanmak için kullanmak istediğiniz sertifika olduğunu doğrulayın. Başka bir sertifika gösteriliyorsa, açılan liste okunu kullanarak doğru sertifikayı seçin ve **Tamam**’a tıklayın.

  ![VPN istemcisinin Azure’a bağlanması](./media/vpn-gateway-howto-point-to-site-rm-ps/clientconnect.png)
3. Bağlantınız kurulur.

  ![Bağlantı kuruldu](./media/vpn-gateway-howto-point-to-site-rm-ps/connected.png)

#### <a name="troubleshooting-windows-client-p2s-connections"></a>Windows istemcisi P2S bağlantı sorunlarını giderme

[!INCLUDE [client certificates](../../includes/vpn-gateway-certificates-verify-client-cert-include.md)]

### <a name="to-connect-from-a-mac-vpn-client"></a>Mac VPN istemcisinden bağlanmak için

Ağ iletişim kutusunda kullanmak istediğiniz istemci profilini bulup **Bağlan**’a tıklayın.

  ![Mac bağlantısı](./media/vpn-gateway-howto-point-to-site-rm-ps/applyconnect.png)

## <a name="verify"></a>Bağlantınızı doğrulamak için

Bu yönergeler, Windows istemcileri için geçerlidir.

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

Bu yönergeler, Windows istemcileri için geçerlidir.

[!INCLUDE [Connect to a VM](../../includes/vpn-gateway-connect-vm-p2s-include.md)]

## <a name="addremovecert"></a>Kök sertifika eklemek veya kaldırmak için

Azure’da güvenilen kök sertifikayı ekleyebilir veya kaldırabilirsiniz. Bir kök sertifikayı kaldırdığınızda, o kökten oluşturulmuş bir sertifikaya sahip istemciler kimlik doğrulaması yapamaz ve bağlantı kuramaz. Bir istemcinin kimlik doğrulaması yapmasını ve bağlanmasını istiyorsanız, Azure’da güvenilen (karşıya yüklenmiş) bir kök sertifikadan oluşturulmuş yeni bir istemci sertifikası yüklemeniz gerekir.

### <a name="addtrustedroot"></a>Güvenilen kök sertifika ekleme

Azure'a en fazla 20 kök sertifika .cer dosyası ekleyebilirsiniz. Aşağıdaki adımlar bir kök sertifika eklemenize yardımcı olur:

#### <a name="certmethod1"></a>1. Yöntem

Bu, bir kök sertifikayı karşıya yüklemenin en verimli yöntemdir.

1. Karşıya yüklenecek .cer dosyasını hazırlayın:

  ```powershell
  $filePathForCert = "C:\cert\P2SRootCert3.cer"
  $cert = new-object System.Security.Cryptography.X509Certificates.X509Certificate2($filePathForCert)
  $CertBase64_3 = [system.convert]::ToBase64String($cert.RawData)
  $p2srootcert = New-AzureRmVpnClientRootCertificate -Name $P2SRootCertName -PublicCertData $CertBase64_3
  ```
2. Dosyayı karşıya yükleyin. Aynı anda yalnızca bir dosya yükleyebilirsiniz.

  ```powershell
  Add-AzureRmVpnClientRootCertificate -VpnClientRootCertificateName $P2SRootCertName -VirtualNetworkGatewayname "VNet1GW" -ResourceGroupName "TestRG" -PublicCertData $CertBase64_3
  ```

3. Sertifika dosyasının karşıya yüklendiğini doğrulamak için:

  ```powershell
  Get-AzureRmVpnClientRootCertificate -ResourceGroupName "TestRG" `
  -VirtualNetworkGatewayName "VNet1GW"
  ```

#### <a name="certmethod2"></a>2. Yöntem

Bu yöntemde 1. Yöntem'den daha fazla adım vardır, ancak aynı sonucu verir. Sertifika verilerini görüntülemenizi gerektirebilecek durumlar için dahil edilmiştir.

1. Azure'a eklenecek yeni kök sertifikayı oluşturup hazırlayın. Ortak anahtarı Base-64 kodlanmış X.509 (.CER) olarak dışarı aktarın ve bir metin düzenleyicisi ile açın. Aşağıdaki örnekte gösterildiği gibi değerleri kopyalayın:

  ![sertifika](./media/vpn-gateway-howto-point-to-site-rm-ps/copycert.png)

  > [!NOTE]
  > Sertifika verilerini kopyalarken, metni satır başları ya da satır besleme karakterleri içermeyen tek ve kesintisiz bir satır şeklinde kopyaladığınızdan emin olun. Satır başlarını ve satır besleme karakterlerini görmek için metin düzenleyicisindeki görünümünüzü 'Sembolü Göster/Tüm karakterleri göster' olarak değiştirmeniz gerekebilir.
  >
  >

2. Sertifika adını ve anahtar bilgilerini bir değişken olarak belirtin. Bilgileri aşağıdaki örnekte gösterildiği gibi kendi bilgilerinizle değiştirin:

  ```powershell
  $P2SRootCertName2 = "ARMP2SRootCert2.cer"
  $MyP2SCertPubKeyBase64_2 = "MIIC/zCCAeugAwIBAgIQKazxzFjMkp9JRiX+tkTfSzAJBgUrDgMCHQUAMBgxFjAUBgNVBAMTDU15UDJTUm9vdENlcnQwHhcNMTUxMjE5MDI1MTIxWhcNMzkxMjMxMjM1OTU5WjAYMRYwFAYDVQQDEw1NeVAyU1Jvb3RDZXJ0MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAyjIXoWy8xE/GF1OSIvUaA0bxBjZ1PJfcXkMWsHPzvhWc2esOKrVQtgFgDz4ggAnOUFEkFaszjiHdnXv3mjzE2SpmAVIZPf2/yPWqkoHwkmrp6BpOvNVOpKxaGPOuK8+dql1xcL0eCkt69g4lxy0FGRFkBcSIgVTViS9wjuuS7LPo5+OXgyFkAY3pSDiMzQCkRGNFgw5WGMHRDAiruDQF1ciLNojAQCsDdLnI3pDYsvRW73HZEhmOqRRnJQe6VekvBYKLvnKaxUTKhFIYwuymHBB96nMFdRUKCZIiWRIy8Hc8+sQEsAML2EItAjQv4+fqgYiFdSWqnQCPf/7IZbotgQIDAQABo00wSzBJBgNVHQEEQjBAgBAkuVrWvFsCJAdK5pb/eoCNoRowGDEWMBQGA1UEAxMNTXlQMlNSb290Q2VydIIQKazxzFjMkp9JRiX+tkTfSzAJBgUrDgMCHQUAA4IBAQA223veAZEIar9N12ubNH2+HwZASNzDVNqspkPKD97TXfKHlPlIcS43TaYkTz38eVrwI6E0yDk4jAuPaKnPuPYFRj9w540SvY6PdOUwDoEqpIcAVp+b4VYwxPL6oyEQ8wnOYuoAK1hhh20lCbo8h9mMy9ofU+RP6HJ7lTqupLfXdID/XevI8tW6Dm+C/wCeV3EmIlO9KUoblD/e24zlo3YzOtbyXwTIh34T0fO/zQvUuBqZMcIPfM1cDvqcqiEFLWvWKoAnxbzckye2uk1gHO52d8AVL3mGiX8wBJkjc/pMdxrEvvCzJkltBmqxTM6XjDJALuVh16qFlqgTWCIcb7ju"
  ```
3. Yeni kök sertifikayı ekleyin. Tek seferde yalnızca bir sertifika ekleyebilirsiniz.

  ```powershell
  Add-AzureRmVpnClientRootCertificate -VpnClientRootCertificateName $P2SRootCertName2 -VirtualNetworkGatewayname "VNet1GW" -ResourceGroupName "TestRG" -PublicCertData $MyP2SCertPubKeyBase64_2
  ```
4. Aşağıdaki örneği kullanarak yeni sertifikanın düzgün şekilde eklendiğini doğrulayabilirsiniz:

  ```powershell
  Get-AzureRmVpnClientRootCertificate -ResourceGroupName "TestRG" `
  -VirtualNetworkGatewayName "VNet1GW"
  ```

### <a name="removerootcert"></a>Kök sertifika kaldırmak için

1. Değişkenleri bildirin.

  ```powershell
  $GWName = "Name_of_virtual_network_gateway"
  $RG = "Name_of_resource_group"
  $P2SRootCertName2 = "ARMP2SRootCert2.cer"
  $MyP2SCertPubKeyBase64_2 = "MIIC/zCCAeugAwIBAgIQKazxzFjMkp9JRiX+tkTfSzAJBgUrDgMCHQUAMBgxFjAUBgNVBAMTDU15UDJTUm9vdENlcnQwHhcNMTUxMjE5MDI1MTIxWhcNMzkxMjMxMjM1OTU5WjAYMRYwFAYDVQQDEw1NeVAyU1Jvb3RDZXJ0MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAyjIXoWy8xE/GF1OSIvUaA0bxBjZ1PJfcXkMWsHPzvhWc2esOKrVQtgFgDz4ggAnOUFEkFaszjiHdnXv3mjzE2SpmAVIZPf2/yPWqkoHwkmrp6BpOvNVOpKxaGPOuK8+dql1xcL0eCkt69g4lxy0FGRFkBcSIgVTViS9wjuuS7LPo5+OXgyFkAY3pSDiMzQCkRGNFgw5WGMHRDAiruDQF1ciLNojAQCsDdLnI3pDYsvRW73HZEhmOqRRnJQe6VekvBYKLvnKaxUTKhFIYwuymHBB96nMFdRUKCZIiWRIy8Hc8+sQEsAML2EItAjQv4+fqgYiFdSWqnQCPf/7IZbotgQIDAQABo00wSzBJBgNVHQEEQjBAgBAkuVrWvFsCJAdK5pb/eoCNoRowGDEWMBQGA1UEAxMNTXlQMlNSb290Q2VydIIQKazxzFjMkp9JRiX+tkTfSzAJBgUrDgMCHQUAA4IBAQA223veAZEIar9N12ubNH2+HwZASNzDVNqspkPKD97TXfKHlPlIcS43TaYkTz38eVrwI6E0yDk4jAuPaKnPuPYFRj9w540SvY6PdOUwDoEqpIcAVp+b4VYwxPL6oyEQ8wnOYuoAK1hhh20lCbo8h9mMy9ofU+RP6HJ7lTqupLfXdID/XevI8tW6Dm+C/wCeV3EmIlO9KUoblD/e24zlo3YzOtbyXwTIh34T0fO/zQvUuBqZMcIPfM1cDvqcqiEFLWvWKoAnxbzckye2uk1gHO52d8AVL3mGiX8wBJkjc/pMdxrEvvCzJkltBmqxTM6XjDJALuVh16qFlqgTWCIcb7ju"
  ```
2. Sertifikayı kaldırın.

  ```powershell
  Remove-AzureRmVpnClientRootCertificate -VpnClientRootCertificateName $P2SRootCertName2 -VirtualNetworkGatewayName $GWName -ResourceGroupName $RG -PublicCertData $MyP2SCertPubKeyBase64_2
  ```
3. Sertifikanın başarıyla kaldırıldığını doğrulamak için aşağıdaki örneği kullanın.

  ```powershell
  Get-AzureRmVpnClientRootCertificate -ResourceGroupName "TestRG" `
  -VirtualNetworkGatewayName "VNet1GW"
  ```

## <a name="revoke"></a>İstemci sertifikasını iptal etmek için

İstemci sertifikalarını iptal edebilirsiniz. Sertifika iptal listesi sayesinde, ayrı istemci sertifikalarına göre Noktadan Siteye bağlantıyı seçmeli olarak reddedebilirsiniz. Bu, güvenilen kök sertifika kaldırma işleminden farklıdır. Azure’dan güvenilen kök sertifika .cer dosyasını kaldırırsanız iptal edilen kök sertifika tarafından oluşturulan/imzalanan tüm istemci sertifikaları reddedilir. Kök sertifika yerine istemci sertifikasını iptal etmek, kök sertifikadan oluşturulan diğer sertifikaların kimlik doğrulaması amacıyla kullanılmaya devam edilmesine olanak sağlar.

Genellikle ekip ve kuruluş düzeylerinde erişimi yönetmek için kök sertifika kullanılırken ayrı kullanıcılar üzerinde ayrıntılı erişim denetimi için iptal edilen istemci sertifikaları kullanılır.

### <a name="revokeclientcert"></a>İstemci sertifikasını iptal etme

1. İstemci sertifikasının parmak izini alın. Daha fazla bilgi için bkz. [Bir Sertifikanın Parmak İzini alma](https://msdn.microsoft.com/library/ms734695.aspx).
2. Bilgileri bir metin düzenleyicisine kopyalayın ve sürekli bir dize haline getirmek için tüm boşlukları kaldırın. Bu dize sonraki adımda bir değişken olarak bildirilir.
3. Değişkenleri bildirin. Önceki adımda alınan parmak izini bildirdiğinizden emin olun.

  ```powershell
  $RevokedClientCert1 = "NameofCertificate"
  $RevokedThumbprint1 = "‎51ab1edd8da4cfed77e20061c5eb6d2ef2f778c7"
  $GWName = "Name_of_virtual_network_gateway"
  $RG = "Name_of_resource_group"
  ```
4. Parmak izini, iptal edilen sertifika listesine ekleyin. Parmak izi eklendiğinde “Başarılı” ifadesini görürsünüz.

  ```powershell
  Add-AzureRmVpnClientRevokedCertificate -VpnClientRevokedCertificateName $RevokedClientCert1 `
  -VirtualNetworkGatewayName $GWName -ResourceGroupName $RG `
  -Thumbprint $RevokedThumbprint1
  ```
5. Parmak izinin, sertifika iptal listesine eklendiğini doğrulayın.

  ```powershell
  Get-AzureRmVpnClientRevokedCertificate -VirtualNetworkGatewayName $GWName -ResourceGroupName $RG
  ```
6. Parmak izi eklendikten sonra sertifika artık bağlanmak için kullanılamaz. Bu sertifikayı kullanarak bağlanmaya çalışan istemciler sertifikanın artık geçerli olmadığını belirten bir ileti alır.

### <a name="reinstateclientcert"></a>İstemci sertifikasını yeniden devreye sokma

Parmak izini, iptal edilen istemci sertifikaları listesinden kaldırarak bir istemci sertifikasını yeniden devreye sokabilirsiniz.

1. Değişkenleri bildirin. Yeniden devreye sokmak istediğiniz sertifika için doğru parmak izini bildirdiğinizden emin olun.

  ```powershell
  $RevokedClientCert1 = "NameofCertificate"
  $RevokedThumbprint1 = "‎51ab1edd8da4cfed77e20061c5eb6d2ef2f778c7"
  $GWName = "Name_of_virtual_network_gateway"
  $RG = "Name_of_resource_group"
  ```
2. Sertifika parmak izini sertifika iptal listesinden kaldırın.

  ```powershell
  Remove-AzureRmVpnClientRevokedCertificate -VpnClientRevokedCertificateName $RevokedClientCert1 `
  -VirtualNetworkGatewayName $GWName -ResourceGroupName $RG -Thumbprint $RevokedThumbprint1
  ```
3. Parmak izinin, iptal edilen parmak izi listesinden kaldırılıp kaldırılmadığını kontrol edin.

  ```powershell
  Get-AzureRmVpnClientRevokedCertificate -VirtualNetworkGatewayName $GWName -ResourceGroupName $RG
  ```

## <a name="faq"></a>Noktadan Siteye hakkında SSS

[!INCLUDE [Point-to-Site FAQ](../../includes/vpn-gateway-faq-p2s-azurecert-include.md)]

## <a name="next-steps"></a>Sonraki adımlar
Bağlantınız tamamlandıktan sonra sanal ağlarınıza sanal makineler ekleyebilirsiniz. Daha fazla bilgi için bkz. [Sanal Makineler](https://docs.microsoft.com/azure/#pivot=services&panel=Compute). Ağ ve sanal makineler hakkında daha fazla bilgi edinmek için, bkz. [Azure ve Linux VM ağına genel bakış](../virtual-machines/linux/azure-vm-network-overview.md).
