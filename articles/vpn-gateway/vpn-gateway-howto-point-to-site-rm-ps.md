---
title: "Noktadan Siteye bağlantısı kullanarak bir bilgisayarı Azure sanal ağına bağlama: PowerShell | Microsoft Docs"
description: "Noktadan Siteye VPN ağ geçidi bağlantısı oluşturarak, bir bilgisayarı Azure Sanal Ağınıza güvenli bir şekilde bağlayın."
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
ms.date: 04/10/2017
ms.author: cherylmc
translationtype: Human Translation
ms.sourcegitcommit: aaf97d26c982c1592230096588e0b0c3ee516a73
ms.openlocfilehash: d340210d799f995cb10a20cf48a9245bbd3bc8d3
ms.lasthandoff: 04/27/2017


---
# <a name="configure-a-point-to-site-connection-to-a-vnet-using-powershell"></a>PowerShell'i kullanarak sanal ağa yönelik bir Noktadan Siteye bağlantısı yapılandırma
> [!div class="op_single_selector"]
> * [Resource Manager - Azure Portal](vpn-gateway-howto-point-to-site-resource-manager-portal.md)
> * [Resource Manager - PowerShell](vpn-gateway-howto-point-to-site-rm-ps.md)
> * [Klasik - Azure Portal](vpn-gateway-howto-point-to-site-classic-azure-portal.md)
> 
> 

Noktadan Siteye (P2S) yapılandırması, ayrı bir istemci bilgisayardan bir sanal ağa yönelik güvenli bağlantı oluşturmanıza olanak sağlar. P2S, SSTP (Güvenli Yuva Tünel Protokolü) aracılığıyla gerçekleşen bir VPN bağlantısıdır. Sanal ağınıza uzak bir konumdan (örneğin, evden veya bir konferanstan) bağlanmak istediğinizde ya da sanal bir ağa bağlanması gereken yalnızca birkaç istemciniz bulunduğunda Noktadan Siteye bağlantıları kullanışlıdır. P2S bağlantılarının bir VPN cihazına veya genel kullanıma yönelik bir IP adresine gerek yoktur. VPN bağlantısını istemci bilgisayardan kurarsınız.

Bu makalede PowerShell kullanılarak Resource Manager dağıtım modelinde Noktadan Siteye bağlantı ile sanal ağ oluşturma işlemi adım adım açıklanmaktadır. Noktadan Siteye bağlantılar hakkında daha fazla bilgi edinmek için bu makalenin sonunda yer alan [Noktadan Siteye hakkında SSS](#faq) bölümünü inceleyin.

### <a name="deployment-models-and-methods-for-p2s-connections"></a>P2S bağlantıları için dağıtım modelleri ve yöntemleri
[!INCLUDE [deployment models](../../includes/vpn-gateway-deployment-models-include.md)]

Aşağıdaki tabloda, P2S yapılandırmaları için iki dağıtım modeli ve kullanılabilir dağıtım yöntemleri gösterilmektedir. Yapılandırma adımlarını içeren bir makale olduğunda, bu tablodan makaleye yönelik doğrudan bağlantı oluştururuz.

[!INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-table-point-to-site-include.md)]

## <a name="basic-workflow"></a>Temel iş akışı
![Bir bilgisayarı Azure sanal ağına bağlama - Noktadan Siteye bağlantı diyagramı](./media/vpn-gateway-howto-point-to-site-rm-ps/point-to-site-diagram.png)

Bu senaryoda Noktadan Siteye bağlantı ile bir sanal ağ oluşturursunuz. Yönergeler ayrıca bu yapılandırma için gerekli olan sertifikaları oluşturmanıza yardımcı olur. P2S bağlantısı şu öğelerden oluşur: VPN ağ geçidine sahip bir sanal ağ, kök sertifika .cer dosyası (ortak anahtar), istemci sertifikası ve istemci üzerindeki VPN yapılandırması. 

Bu yapılandırma için aşağıdaki değerler kullanılır. Değişkenler makalenin [1](#declare). bölümünde ayarlanır. İzlenecek yol olarak adımları kullanıp değerleri değiştirmeden uygulayabilir veya ortamınızı yansıtacak şekilde değiştirebilirsiniz. 

### <a name="example"></a>Örnek değerler
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
* DNS Sunucusu: Ad çözümlemesi için kullanmak istediğiniz **DNS sunucusunun IP adresi**.
* **Ağ Geçidi Adı: Vnet1GW**
* **Ortak IP adı: VNet1GWPIP**
* **VpnType: RouteBased**

## <a name="before-beginning"></a>Başlamadan önce
* Azure aboneliğiniz olduğunu doğrulayın. Henüz Azure aboneliğiniz yoksa [MSDN abonelik avantajlarınızı](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details) etkinleştirebilir veya [ücretsiz bir hesap](https://azure.microsoft.com/pricing/free-trial) için kaydolabilirsiniz.
* Azure Resource Manager PowerShell cmdlet'lerinin en son sürümünü yükleyin. PowerShell cmdlet'lerini yükleme hakkında daha fazla bilgi için bkz. [Azure PowerShell'i yükleme ve yapılandırma](/powershell/azure/overview). 

## <a name="declare"></a>1. Bölüm - Oturum açma ve değişkenleri ayarlama
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
  $DNS = "8.8.8.8"
  $GWName = "VNet1GW"
  $GWIPName = "VNet1GWPIP"
  $GWIPconfName = "gwipconf"
  ```

## <a name="ConfigureVNet"></a>2. Kısım - Sanal ağ yapılandırma
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
3. Sanal ağı oluşturun. Belirtilen DNS sunucusu, bağlandığınız kaynakların adlarını çözümleyebilen bir DNS sunucusu olmalıdır. Bu örnekte genel IP adresi kullandık. Kendi değerlerinizi kullandığınızdan emin olun.

  ```powershell
  New-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $RG -Location $Location -AddressPrefix $VNetPrefix1,$VNetPrefix2 -Subnet $fesub, $besub, $gwsub -DnsServer $DNS
  ```
4. Oluşturduğunuz sanal ağa ilişkin değişkenleri belirtin.

  ```powershell
  $vnet = Get-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $RG
  $subnet = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet
  ```
5. Dinamik olarak atanan bir genel IP adresi isteyin. Ağ geçidinin düzgün çalışması için bu IP adresi gereklidir. Ağ geçidini daha sonra ağ geçidi IP yapılandırmasına bağlayacaksınız.

  ```powershell
  $pip = New-AzureRmPublicIpAddress -Name $GWIPName -ResourceGroupName $RG -Location $Location -AllocationMethod Dynamic
  $ipconf = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GWIPconfName -Subnet $subnet -PublicIpAddress $pip
  ```


## <a name="Certificates"></a>3. Kısım - Sertifikalar

Noktadan Siteye VPN’lerde VPN istemcilerinin kimlik doğrulamasını yapmak için Azure tarafından sertifikalar kullanılır. Kök sertifikayı oluşturduktan sonra, ortak sertifika verilerini (özel anahtarı değil) Base-64 olarak kodlanmış X.509 .cer dosyası olarak dışarı aktarırsınız. Ardından kök sertifikanın genel sertifika verilerini Azure’a yüklersiniz.

Noktadan Siteye bağlantı kullanarak bir sanal ağa bağlanan her istemci bilgisayarda bir istemci sertifikası yüklü olmalıdır. İstemci sertifikası kök sertifikadan oluşturulur ve her bir istemci bilgisayara yüklenir. Geçerli bir istemci sertifikası yüklü değilse ve istemci sanal ağa bağlanmaya çalışırsa, kimlik doğrulaması başarısız olur.

### <a name="cer"></a>1. Adım: Kök sertifikaya ilişkin .cer dosyasını alma

#### <a name="enterprise-certificate"></a>Kurumsal sertifika
 
Bir kuruluş çözümü kullanıyorsanız var olan sertifika zincirinizi kullanabilirsiniz. Kullanmak istediğiniz kök sertifika için .cer dosyasını alın.

#### <a name="self-signed-root-certificate"></a>Otomatik olarak imzalanan kök sertifika

Kurumsal bir sertifika çözümü kullanmıyorsanız, otomatik olarak imzalanan bir sertifika oluşturmanız gerekir. P2S kimlik doğrulaması için gerekli alanları içeren bir otomatik olarak imzalanan kök sertifika oluşturmak için PowerShell kullanabilirsiniz. [PowerShell kullanarak Noktadan Siteye bağlantıları için otomatik olarak imzalanan bir kök sertifika oluşturma](vpn-gateway-certificates-point-to-site.md) bölümü, otomatik olarak imzalanan bir kök sertifika oluşturma konusunda size adım adım yol gösterir.

> [!NOTE]
> Daha önce, otomatik olarak imzalanan kök sertifikalar oluşturmak ve Noktadan Siteye bağlantılar için istemci sertifikaları oluşturmak için makecert metodu öneriliyordu. Artık bu sertifikaları oluşturmak için PowerShell kullanabilirsiniz. PowerShell kullanmanın bir avantajı SHA-2 sertifikaları oluşturma özelliğidir. Gerekli değerler için bkz. [PowerShell kullanarak Noktadan Siteye bağlantıları için otomatik olarak imzalanan bir kök sertifika oluşturma](vpn-gateway-certificates-point-to-site.md).
>
>

#### <a name="to-export-the-public-key-for-a-self-signed-root-certificate"></a>Otomatik olarak imzalanan bir kök sertifika için ortak anahtarı dışarı aktarmak için

Noktadan Siteye bağlantılar, ortak anahtarın (.cer) Azure’a yüklenmesini gerektirir. Aşağıdaki adımlar otomatik olarak imzalanan kök sertifikanız için .cer dosyasını dışarı aktarmanıza yardımcı olur.

1. Sertifikadan bir .cer dosyası almak için **Kullanıcı sertifikalarını yönet** menüsünü açın.
2. Otomatik olarak imzalanan 'P2SRootCert' kök sertifikasını bulun ('Sertifikalar - Geçerli Kullanıcı\Kişisel\Sertifikalar' konumundadır) ve sağ tıklayın. **Tüm Görevler**’e ve ardından **Dışarı Aktar**’a tıklayarak **Sertifika Verme Sihirbazı**’nı açın.
3. Sihirbazda, **İleri**’ye tıklayın. **Hayır, özel anahtarı dışarı aktarma**’yı seçin ve **İleri**’ye tıklayın.
4. **Dışarı Aktarma Dosyası Biçimi** sayfasında **Base-64 ile kodlanmış X.509 (.CER)** seçeneğini belirleyin ve **İleri**’ye tıklayın. 
5. **Dışarı Aktarılacak Dosya** sayfasında 'C:' dizinine göz atın, 'cert' adlı bir alt dizin oluşturup seçin. Sertifika dosyasını 'P2SRootCert.cer' olarak adlandırıp **Kaydet**’e tıklayın. 
6. **İleri** ve ardından **Son**’a tıklayarak sertifikayı dışarı aktarın. **Dışarı aktarma başarılı** ifadesi görüntülenir. **Tamam**’a tıklayarak sihirbazı kapatın.

### <a name="generate"></a>2. Adım - İstemci sertifikası oluşturma
Her istemci için benzersiz bir sertifika oluşturabileceğiniz gibi, birden çok istemcide aynı sertifikayı da kullanabilirsiniz. Benzersiz istemci sertifikaları oluşturmanın avantajı, tek bir sertifikayı iptal edebiliyor olmanızdır. Aksi takdirde, herkes aynı istemci sertifikasını kullanıyorsa ve sertifikayı iptal etmeniz gerekirse, kimlik doğrulaması için bu sertifikayı kullanan tüm istemciler için yeni sertifikalar oluşturup yüklemeniz gerekir.

#### <a name="enterprise-certificate"></a>Kurumsal sertifika
- Kurumsal bir sertifika çözümü kullanıyorsanız, 'etkialaniadi\kullaniciadi' biçimini kullanmak yerine, yaygın olarak kullanılan 'name@yourdomain.com' ad değer biçimiyle bir istemci sertifikası oluşturun.
- İstemci sertifikasının, kullanım listesindeki ilk öğe olarak Akıllı Kart Oturumu, vb. yerine ‘İstemci Kimlik Doğrulaması’na sahip ‘Kullanıcı’ sertifikası şablonunu temel alarak hazırlandığından emin olun. İstemci sertifikasına sağ tıklayıp **Ayrıntılar > Gelişmiş Anahtar Kullanımı**’nı görüntüleyerek sertifikayı denetleyebilirsiniz.

#### <a name="self-signed-root-certificate"></a>Otomatik olarak imzalanan kök sertifika 
Otomatik olarak imzalanan bir kök sertifika kullanıyorsanız, Noktadan Siteye bağlantılar ile uyumlu bir istemci sertifikası oluşturmak için [PowerShell kullanarak istemci sertifikası oluşturma](vpn-gateway-certificates-point-to-site.md#clientcert) bölümüne bakın.


### <a name="exportclientcert"></a>3. Adım - İstemci sertifikasını dışarı aktarma

[PowerShell](vpn-gateway-certificates-point-to-site.md#clientcert) yönergelerini kullanarak bir otomatik olarak imzalanan kök sertifikadan bir istemci sertifikası oluşturursanız sertifika, sertifikayı oluşturmak için kullandığınız bilgisayara otomatik olarak yüklenir. İstemci sertifikasını başka bir istemci bilgisayara yüklemek istiyorsanız, sertifikayı dışarı aktarmanız gerekir.
 
1. Bir istemci sertifikasını dışarı aktarmak için **Kullanıcı sertifikalarını yönet** menüsünü açın. Dışarı aktarmak istediğiniz istemci sertifikasına sağ tıklayın, **tüm görevler**’e ve ardından **dışarı aktar**’a tıklayarak **Sertifika Verme Sihirbazı**’nı açın.
2. Sihirbazda **İleri**’ye tıklayın, **Evet, özel anahtarı dışarı aktar**’ı seçin ve **İleri**’ye tıklayın.
3. **Dışarı Aktarma Dosyası Biçimi** sayfasında, varsayılan ayarları seçili bırakın. Gerekli kök sertifika bilgilerini de dışarı aktarmak için **Mümkünse tüm sertifika yolundaki tüm sertifikaları ekle** seçeneğinin işaretlendiğinden emin olun. Ardından **İleri**'ye tıklayın.
4. **Güvenlik** sayfasında, özel anahtarı korumanız gerekir. Bir parola kullanmayı seçerseniz, bu sertifika için ayarladığınız parolayı kaydettiğinizden ya da unutmayacağınızdan emin olun. Ardından **İleri**'ye tıklayın.
5. **Dışarı Aktarılan Dosya** sayfasında **Gözat**'a tıklayarak sertifika için dışarı aktarma konumunu seçin. **Dosya adı** alanına, sertifika dosyası için bir ad girin. Ardından **İleri**'ye tıklayın.
6. Sertifikayı dışarı aktarmak için **Son**'a tıklayın.

### <a name="upload"></a>4. Adım - Kök sertifika .cer dosyasını yükleme

Ağ geçidi oluşturulduktan sonra güvenilen bir kök sertifika için .cer dosyasını Azure’a yükleyebilirsiniz. 20 adede kadar kök sertifikasının dosyasını yükleyebilirsiniz. Kök sertifikanın özel anahtarını Azure'a yüklemezsiniz. .cer dosyası yüklendikten sonra Azure, sanal ağa bağlanan istemcilerin kimliğini doğrulamak için bu anahtarı kullanır.

1. Sertifika adınızın değişkenini tanımlamak için değeri kendi değerinizle değiştirin:

  ```powershell
  $P2SRootCertName = "Mycertificatename.cer"
  ```
2. Dosya yolunu kendi dosya yolunuzla değiştirin ve ardından cmdlet'leri çalıştırın.

  ```powershell
  $filePathForCert = "C:\cert\Mycertificatename.cer"
  $cert = new-object System.Security.Cryptography.X509Certificates.X509Certificate2($filePathForCert)
  $CertBase64 = [system.convert]::ToBase64String($cert.RawData)
  $p2srootcert = New-AzureRmVpnClientRootCertificate -Name $P2SRootCertName -PublicCertData $CertBase64
  ```


## <a name="creategateway"></a>4. Kısım - VPN ağ geçidini oluşturma
Sanal ağınız için sanal ağ geçidini yapılandırın ve oluşturun. *-GatewayType* değeri **Vpn** ve *-VpnType* değeri **RouteBased** olmalıdır. Bir VPN ağ geçidinin tamamlanması 45 dakika sürebilir.

```powershell
New-AzureRmVirtualNetworkGateway -Name $GWName -ResourceGroupName $RG `
-Location $Location -IpConfigurations $ipconf -GatewayType Vpn `
-VpnType RouteBased -EnableBgp $false -GatewaySku Standard `
-VpnClientAddressPool $VPNClientAddressPool -VpnClientRootCertificates $p2srootcert
```


## <a name="clientconfig"></a>5. Kısım - VPN istemci yapılandırma paketini indirme
Noktadan Siteye VPN kullanarak bir sanal ağa bağlanmak için her istemcinin bir VPN istemcisi yapılandırma paketi yüklemesi gerekir. Paket bir VPN istemcisi yüklemez. Sanal ağa bağlanmak için gereken ayarlarla yerel Windows VPN istemcisini yapılandırır. Desteklenen istemci işletim sistemlerinin listesi için bu makalenin sonundaki [Noktadan Siteye bağlantılar hakkında SSS](#faq) bölümüne bakın.

1. Ağ geçidi oluşturulduktan sonra istemci yapılandırma paketini oluşturup indirebilirsiniz. Bu örnekte 64 bit istemcilere yönelik paket indirildi. 32 bit istemcisini yüklemek istiyorsanız 'Amd64' öğesini 'x86' olarak değiştirin. VPN istemcisini, Azure portalını kullanarak da indirebilirsiniz.

  ```powershell
  Get-AzureRmVpnClientPackage -ResourceGroupName $RG `
  -VirtualNetworkGatewayName $GWName -ProcessorArchitecture Amd64
  ```
2. Paketi indirmek için döndürülen bağlantıyı kopyalayıp bağlantıyı çevreleyen """ işaretlerini kaldırarak bir web tarayıcısına yapıştırın. 
3. Paketi indirip istemci bilgisayara yükleyin. Bir SmartScreen açılır penceresi görürseniz **Daha fazla bilgi**’ye ve ardından **Yine de çalıştır**’a tıklayın. Paketi ayrıca diğer istemci bilgisayarlara yüklemek üzere kaydedebilirsiniz.
4. İstemci bilgisayarda **Ağ Ayarları**’na gidin ve **VPN** öğesine tıklayın. VPN bağlantısı, bağlandığı sanal ağın adını gösterir.



## <a name="clientcertificate"></a>6. Kısım: Dışarı aktarılan bir istemci sertifikasını yükleme

İstemci sertifikalarını oluşturmak için kullandığınız bilgisayardan farklı bir istemci bilgisayarda bir P2S bağlantı oluşturmak istiyorsanız, bir istemci sertifikası yüklemeniz gerekir. Bir istemci sertifikası yüklenirken, istemci sertifikası dışarı aktarılırken oluşturulan parola gerekir.

1. *.pfx* dosyasını bulun ve istemci bilgisayara kopyalayın. İstemci bilgisayarda *.pfx* dosyasına çift tıklayarak yükleme işlemini gerçekleştirin. **Depolama Konumu**’nu **Geçerli Kullanıcı** olarak bırakın ve **İleri**’ye tıklayın.
2. İçeri aktarılacak **Dosya** sayfasında herhangi bir değişiklik yapmayın. **İleri**'ye tıklayın.
3. **Özel anahtar koruma** sayfasında, sertifika parolasını girin veya güvenlik sorumlusunun doğru olduğunu onaylayıp **İleri**’ye tıklayın.
4. **Sertifika Deposu** sayfasında, varsayılan konumu bırakın ve **İleri**’ye tıklayın.
5. **Son**'a tıklayın. Sertifika yüklemesi için **Güvenlik Uyarısı**’nda, **Evet**’e tıklayın. Sertifikayı siz oluşturduğunuz için güvenle ‘Evet’ seçeneğine tıklayabilirsiniz. Sertifika başarıyla içeri aktarılır.

## <a name="connect"></a>7. Kısım - Azure'a bağlanma
1. İstemci bilgisayarda sanal ağınıza bağlanmak için VPN bağlantılarında gezinin ve oluşturduğunuz VPN bağlantısını bulun. Bu VPN bağlantısı sanal ağınızla aynı ada sahiptir. **Bağlan**'a tıklayın. Sertifika kullanımına ilişkin bir açılır ileti görüntülenebilir. Yükseltilmiş ayrıcalıklar kullanmak için **Devam**’a tıklayın. 
2. **Bağlantı** durum sayfasında **Bağlan**'a tıklayarak bağlantıyı başlatın. Bir **Sertifika Seç** ekranı çıkarsa, gösterilen istemci sertifikasının bağlanmak için kullanmak istediğiniz sertifika olduğunu doğrulayın. Başka bir sertifika gösteriliyorsa, açılan liste okunu kullanarak doğru sertifikayı seçin ve **Tamam**’a tıklayın.
   
    ![VPN istemcisinin Azure’a bağlanması](./media/vpn-gateway-howto-point-to-site-rm-ps/clientconnect.png)
3. Bağlantınız kurulur.
   
    ![Bağlantı kuruldu](./media/vpn-gateway-howto-point-to-site-rm-ps/connected.png)

Bağlanmayla ilgili sorun yaşıyorsanız aşağıdakileri denetleyin:

- **Kullanıcı sertifikalarını yönet** menüsünü açıp **Güvenilen Kök Sertifika Yetkilileri\Sertifikalar**’a gidin. Kök sertifikanın listelenmiş olduğunu doğrulayın. Kimlik doğrulamasının çalışması için kök sertifika mevcut olmalıdır. Bir .pfx istemci sertifikasını varsayılan 'Mümkünse sertifika yolundaki tüm sertifikaları ekle' değerini kullanarak dışarı aktardığınızda, kök sertifika bilgileri de dışarı aktarılır. İstemci sertifikasını yüklediğinizde, kök sertifika da istemci bilgisayara yüklenir. 

- Bir Kuruluş Sertifika Yetkilisi çözümü kullanarak verilen bir sertifika kullanıyor ve kimlik doğrulama sorunu yaşıyorsanız, istemci sertifikasındaki kimlik doğrulama sırasını denetleyin. Kimlik doğrulama listesinin sırasını istemci sertifikasına çift tıklayıp **Ayrıntılar > Gelişmiş Anahtar Kullanımı**’na giderek denetleyebilirsiniz. Listede ilk öğe olarak ‘İstemci Kimlik Doğrulaması’nın göründüğünden emin olun. Aksi takdirde, listedeki ilk öğe olarak İstemci Kimlik Doğrulaması’nı içeren Kullanıcı şablonunu temel alarak oluşturulmuş bir istemci sertifikası vermeniz gerekir.  


## <a name="verify"></a>8. Kısım - Bağlantınızı doğrulama
1. VPN bağlantınızın etkin olduğunu doğrulamak için, yükseltilmiş bir komut istemi açın ve *ipconfig/all* komutunu çalıştırın.
2. Sonuçlara bakın. Aldığınız IP adresinin, yapılandırmanızda belirttiğiniz Noktadan Siteye VPN İstemcisi Adres Havuzu'ndaki adreslerden biri olduğuna dikkat edin. Sonuçları şu örneğe benzer:
   
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


## <a name="connectVM"></a>Sanal makineye bağlanma

1. Sanal ağınıza bağlandıktan sonra P2S bağlantınız üzerinden bir sanal makineye bağlanabilirsiniz. Sanal makineye bağlanmak için sanal makinenin özel IP adresi gerekir. Aşağıdaki örnek, [Get-AzureRmNetworkInterface](/powershell/module/azurerm.network/get-azurermnetworkinterface) ile özel IP adresini almanıza yardımcı olur. Sonuçlar VM’leri ve tüm Kaynak Gruplarınızdaki karşılık gelen özel IP adreslerini içeren bir liste döndürür. 

  ```powershell   
  $vms = get-azurermvm
  $nics = get-azurermnetworkinterface | where VirtualMachine -NE $null #skip Nics with no VM
  
  foreach($nic in $nics)
  {
    $vm = $vms | where-object -Property Id -EQ $nic.VirtualMachine.id
    $prv =  $nic.IpConfigurations | select-object -ExpandProperty PrivateIpAddress
    $alloc =  $nic.IpConfigurations | select-object -ExpandProperty PrivateIpAllocationMethod
    Write-Output "$($vm.Name) : $prv , $alloc"
  }
  ```
2. Sanal makine bir uzak masaüstü oturumu oluşturmak için aşağıdaki komutu kullanın. IP adresini, bağlanmak istediğiniz VM ile ilişkili özel IP adresi ile değiştirin. İstendiğinde, sanal makine oluşturulurken kullanılan kimlik bilgilerini girin. 

  ```powershell   
  mstsc /v:192.168.1.4
  ```

Bir sanal makineye P2S üzerinden bağlanırken sorun yaşıyorsanız, 'ipconfig' seçeneğini kullanarak bağlantıyı kurduğunuz bilgisayardaki Ethernet bağdaştırıcısına atanmış IPv4 adresini denetleyin. IP adresi bağlanacağınız sanal ağın adres aralığında veya VPNClientAddressPool adres aralığında ise, bu durum çakışan bir adres alanı olarak adlandırılır. Adres alanınız bu şekilde çakıştığında, ağ trafiği Azure’a ulaşmaz ve yerel ağda kalır. Ağ adres alanlarınız çakışmadığı halde sanal makinenize bağlanamıyorsanız, bkz. [VM ile Uzak Masaüstü bağlantıları için sorun giderme](../virtual-machines/windows/troubleshoot-rdp-connection.md).

## <a name="addremovecert"></a>Güvenilen kök sertifika ekleme veya kaldırma

Azure’da güvenilen kök sertifikayı ekleyebilir veya kaldırabilirsiniz. Güvenilen kök sertifikayı kaldırdığınızda, kök sertifikadan oluşturulan istemci sertifikaları artık Noktadan Siteye bağlantı aracılığıyla Azure'a bağlanamaz. İstemcilerin bağlanmasını istiyorsanız, bağlanabilmek için Azure'da güvenilen bir sertifikadan oluşturan yeni bir istemci sertifikası yüklemeniz gerekir.

### <a name="to-add-a-trusted-root-certificate"></a>Güvenilen kök sertifika ekleme
Azure'a en fazla 20 güvenilen kök sertifika .cer dosyası ekleyebilirsiniz. Aşağıdaki adımlar bir kök sertifika eklemenize yardımcı olur:

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


### <a name="to-remove-a-trusted-root-certificate"></a>Güvenilen kök sertifikayı kaldırmak için


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

## <a name="revoke"></a>İstemci sertifikasını iptal etme
İstemci sertifikalarını iptal edebilirsiniz. Sertifika iptal listesi sayesinde, ayrı istemci sertifikalarına göre Noktadan Siteye bağlantıyı seçmeli olarak reddedebilirsiniz. Bu, güvenilen kök sertifikayı kaldırma işleminden farklıdır. Azure’dan güvenilen kök sertifika .cer dosyasını kaldırırsanız iptal edilen kök sertifika tarafından oluşturulan/imzalanan tüm istemci sertifikaları reddedilir. Kök sertifika yerine istemci sertifikasını iptal etmek, kök sertifikadan oluşturulan diğer sertifikaların kimlik doğrulaması amacıyla kullanılmaya devam edilmesine olanak sağlar.

Genellikle ekip ve kuruluş düzeylerinde erişimi yönetmek için kök sertifika kullanılırken ayrı kullanıcılar üzerinde ayrıntılı erişim denetimi için iptal edilen istemci sertifikaları kullanılır.

### <a name="to-revoke-a-client-certificate"></a>İstemci sertifikasını iptal etme

1. İstemci sertifikasının parmak izini alın. Daha fazla bilgi için bkz. [Bir Sertifikanın Parmak İzini alma](https://msdn.microsoft.com/library/ms734695.aspx).
2. Bilgileri bir metin düzenleyicisine kopyalayın ve sürekli bir dize haline getirmek için tüm boşlukları kaldırın. Bu değer sonraki adımda bir değişken olarak bildirilir.
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

### <a name="to-reinstate-a-client-certificate"></a>İstemci sertifikasını yeniden devreye sokma
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

[!INCLUDE [Point-to-Site FAQ](../../includes/vpn-gateway-point-to-site-faq-include.md)]

## <a name="next-steps"></a>Sonraki adımlar
Bağlantınız tamamlandıktan sonra sanal ağlarınıza sanal makineler ekleyebilirsiniz. Daha fazla bilgi için bkz. [Sanal Makineler](https://docs.microsoft.com/azure/#pivot=services&panel=Compute). Ağ ve sanal makineler hakkında daha fazla bilgi edinmek için, bkz. [Azure ve Linux VM ağına genel bakış](../virtual-machines/linux/azure-vm-network-overview.md).

