<properties 
   pageTitle="Resource Manager dağıtım modelini kullanarak sanal ağa yönelik bir Noktadan Siteye VPN ağ geçidi bağlantısı yapılandırma | Microsoft Azure"
   description="Noktadan Siteye bir VPN ağ geçidi bağlantısı oluşturarak Azure Sanal Ağınıza güvenli bir şekilde bağlanın."
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"/>
<tags 
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/31/2016"
   ms.author="cherylmc" />

# PowerShell'i kullanarak VNet’e yönelik bir Noktadan Siteye bağlantısı yapılandırma

> [AZURE.SELECTOR]
- [PowerShell - Resource Manager](vpn-gateway-howto-point-to-site-rm-ps.md)
- [Portal - Klasik](vpn-gateway-point-to-site-create.md)

Noktadan Siteye (P2S) yapılandırması, ayrı bir istemci bilgisayardan bir sanal ağa yönelik güvenli bağlantı oluşturmanıza olanak sağlar. Sanal ağınıza uzak bir konumdan (örneğin, evden veya bir konferanstan) bağlanmak istediğinizde ya da sanal bir ağa bağlanması gereken yalnızca birkaç istemciniz bulunduğunda P2S bağlantısı kullanışlıdır. 

Bu makalede **Resource Manager dağıtım modelinde** Noktadan Siteye bağlantı ile VNet oluşturma işlemi adım adım açıklanmaktadır. Bu adımlar PowerShell gerektirir. Şu anda bu çözümü Azure portalında uçtan uca oluşturamazsınız.

Noktadan Siteye bağlantıların çalışması için bir VPN cihazına veya genel kullanıma yönelik bir IP adresine gerek yoktur. VPN bağlantısı, bağlantının istemci bilgisayardan başlatılmasıyla oluşturulur. Noktadan Siteye bağlantılar hakkında daha fazla bilgi edinmek için [VPN Gateway ile ilgili SSS](vpn-gateway-vpn-faq.md#point-to-site-connections) ve [Planlama ve Tasarım](vpn-gateway-plan-design.md) başlıklı makalelere bakın.



**Azure dağıtım modelleri hakkında**

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)] 

**Noktadan Siteye bağlantılar için dağıtım modelleri ve araçları**

[AZURE.INCLUDE [vpn-gateway-table-point-to-site](../../includes/vpn-gateway-table-point-to-site-include.md)] 

![Noktadan Siteye diyagramı](./media/vpn-gateway-howto-point-to-site-rm-ps/p2srm.png "point-to-site")


## Bu yapılandırma hakkında

Bu senaryoda Noktadan Siteye bağlantı ile bir sanal ağ oluşturursunuz. Yönergeler ayrıca bu yapılandırma için gerekli olan sertifikaları oluşturmanıza yardımcı olur. P2S bağlantısı şu öğelerden oluşur: VPN ağ geçidine sahip bir VNet, kök sertifika .cer dosyası (genel anahtar), istemci sertifikası ve istemci üzerindeki VPN yapılandırması. 

Bu yapılandırma için aşağıdaki değerler kullanılır. Değişkenler makalenin [1](#declare). bölümünde ayarlanır. İzlenecek yol olarak adımları kullanıp değerleri değiştirmeden uygulayabilir veya ortamınızı yansıtacak şekilde değiştirebilirsiniz. 

- Ad: **VNet1**, kullanılan adres alanları: **192.168.0.0/16** ve **10.254.0.0/16**. Bir VNet için birden fazla adres alanı kullanabileceğinizi unutmayın.
- Alt ağ adı: **FrontEnd**, kullanılan değer: **192.168.1.0/24**
- Alt ağ adı: **BackEnd**, kullanılan değer: **10.254.1.0/24**
- Alt ağ adı: **GatewaySubnet**, kullanılan değer: **192.168.200.0/24**. Ağ geçidinin çalışması için Alt Ağ adı olarak *GatewaySubnet*'in kullanılması zorunludur. 
- VPN istemcisi adres havuzu: **172.16.201.0/24**. VNet'e bu Noktadan Siteye bağlantıyı kullanarak bağlanan VPN istemcileri bu havuzdan bir IP adresi alır.
- Abonelik: Birden fazla aboneliğiniz varsa doğru aboneliğe sahip olduğunuzu doğrulayın.
- Kaynak Grubu: **TestRG**
- Konum: **Doğu ABD**
- DNS Sunucusu: Ad çözümlemesi için kullanmak istediğiniz DNS sunucusunun **IP adresi**.
- Ağ Geçidi Adı: **GW**
- Genel IP adı: **GWIP**
- VPN Türü: **RouteBased**


## Başlamadan önce

- Azure aboneliğiniz olduğunu doğrulayın. Henüz Azure aboneliğiniz yoksa [MSDN abonelik avantajlarınızı](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) etkinleştirebilir veya [ücretsiz bir hesap](https://azure.microsoft.com/pricing/free-trial/) için kaydolabilirsiniz.
    
- Azure Resource Manager PowerShell cmdlet'lerini (1.0.2 veya sonraki bir sürümü) yükleyin. PowerShell cmdlet'lerini yükleme hakkında daha fazla bilgi için bkz. [Azure PowerShell'i yükleme ve yapılandırma](../powershell-install-configure.md). Bu yapılandırma için PowerShell ile çalışırken yönetici olarak işlem yaptığınızdan emin olun. 

## <a name="declare"></a>Bölüm 1 - Oturum açma ve değişkenleri ayarlama

Bu bölümde oturum açıp bu yapılandırma için kullanılan değerleri bildirirsiniz. Belirtilen değerler örnek betiklerde kullanılır. Değerleri, ortamınızı yansıtacak şekilde değiştirin. Veya, bildirilen değerleri kullanın ve bir alıştırma olarak adımları uygulayın.

1. PowerShell konsoluna giderek Azure hesabınızda oturum açın. Bu cmdlet, Azure Hesabınıza ilişkin oturum açma kimlik bilgilerinizi girmenizi ister. Oturum açtıktan sonra, Azure PowerShell'de kullanabilmeniz için hesap ayarlarınızı indirir.

        Login-AzureRmAccount 

2. Azure aboneliklerinizin bir listesini alın.

        Get-AzureRmSubscription

3. Kullanmak istediğiniz aboneliği belirtin. 

        Select-AzureRmSubscription -SubscriptionName "Name of subscription"

4. Kullanmak istediğiniz değişkenleri bildirin. Aşağıdaki örneği kullanın ve gerektiğinde, değerleri kendi değerlerinizle değiştirin.

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
        $GWName = "GW"
        $GWIPName = "GWIP"
        $GWIPconfName = "gwipconf"
        $P2SRootCertName = "ARMP2SRootCert.cer"


## Bölüm 2 - Bir VNet yapılandırma 


1. Bir kaynak grubu oluşturun.

        New-AzureRmResourceGroup -Name $RG -Location $Location

2. Sanal ağ için alt ağ yapılandırmalarını oluşturup *FrontEnd*, *BackEnd* ve *GatewaySubnet* olarak adlandırın. Bu ön ekler, yukarıda belirtilen VNet adres alanının parçası olmalıdır.

        $fesub = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName -AddressPrefix $FESubPrefix
        $besub = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName -AddressPrefix $BESubPrefix
        $gwsub = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName -AddressPrefix $GWSubPrefix

3. Sanal ağı oluşturun. Belirtilen DNS sunucusu, bağlandığınız kaynakların adlarını çözümleyebilen bir DNS sunucusu olmalıdır. Bu örnekte genel IP adresi kullandık. Kendi değerlerinizi kullandığınızdan emin olun.
    
        New-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $RG -Location $Location -AddressPrefix $VNetPrefix1,$VNetPrefix2 -Subnet $fesub, $besub, $gwsub -DnsServer $DNS

4. Oluşturduğunuz sanal ağa ilişkin değişkenleri belirtin.

        $vnet = Get-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $RG
        $subnet = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet

5. Dinamik olarak atanan bir genel IP adresi isteyin. Ağ geçidinin düzgün çalışması için bu IP adresi gereklidir. Ağ geçidini daha sonra ağ geçidi IP yapılandırmasına bağlayacaksınız.
        
        $pip = New-AzureRmPublicIpAddress -Name $GWIPName -ResourceGroupName $RG -Location $Location -AllocationMethod Dynamic
        $ipconf = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GWIPconfName -Subnet $subnet -PublicIpAddress $pip

## Bölüm 3 - Güvenilir sertifika ekleme

Azure, sertifikaları kullanarak P2S üzerinden bağlanmak isteyen istemcilerin kimliğini doğrular. Azure’a bir kök sertifika için .cer dosyasını (genel anahtar) aktarın. Azure'a Base64 ile kodlanmış bir X.509 (*.cer) dosyası eklediğinizde, Azure'a dosyanın temsil ettiği kök sertifikaya güvenebileceğini söylemiş olursunuz.

Bir kuruluş çözümü kullanıyorsanız var olan sertifika zincirinizi kullanabilirsiniz. Kuruluş CA çözümü kullanmıyorsanız otomatik olarak imzalanan bir kök sertifika oluşturabilirsiniz. Otomatik olarak imzalanan sertifika oluşturmanın bir yolu makecert yöntemidir. *makecert* ile otomatik olarak imzalanan kök sertifikası oluşturmaya ilişkin yönergeler için bkz. [Noktadan Siteye yapılandırmaları için otomatik olarak imzalanan kök sertifikalar ile çalışma](vpn-gateway-certificates-point-to-site.md).

Sertifikayı nasıl edindiğinize bakılmaksızın, .cer dosyasını Azure’a yükleyin ve ayrıca istemci sertifikaları oluşturarak bağlanmak istediğiniz istemcilere yükleyin. Otomatik olarak imzalanan sertifikayı doğrudan istemciye yüklemeyin. İstemci sertifikalarını daha sonra bu makalenin [İstemci Yapılandırması](#cc) bölümünde oluşturabilirsiniz.
        
Azure'a en fazla 20 güvenilen sertifika ekleyebilirsiniz. Ortak anahtarı almak için sertifikayı Base64 ile kodlanmış bir X.509 (.cer) dosyası olarak dışarı aktarın. Cer dosyasını dışarı aktarmanın bir yolu **certmger.msc** dosyasını açıp Kişisel/Sertifikaları yolunda sertifikayı bulmaktır. Sağ tıklayın ve özel anahtar olmadan "Base-64 kodlanmış X.509(.CER)" olarak dışarı aktarın. .cer dosyasını aktardığınız dosya yolunu not edin. Bu, sertifikanızın Base64 dize gösterimini almaya ilişkin bir örnektir. 

Güvenilir sertifikayı Azure’a ekleyin. Bu adımda kendi .cer dosya yolunuzu kullanmanız gerekir. Bu makalenin 1. Bölümünde ayarladığınız $P2SRootCertName = "ARMP2SRootCert.cer" değişkenine özellikle dikkat edin. Sertifikanızın adı farklıysa değişkeni buna göre ayarlayın.
    
        $filePathForCert = "pasteYourCerFilePathHere"
        $cert = new-object System.Security.Cryptography.X509Certificates.X509Certificate2($filePathForCert)
        $CertBase64 = [system.convert]::ToBase64String($cert.RawData)
        $p2srootcert = New-AzureRmVpnClientRootCertificate -Name $P2SRootCertName -PublicCertData $CertBase64

## Bölüm 4 - VPN ağ geçidi oluşturma

VNet'iniz için sanal ağ geçidini yapılandırın ve oluşturun. *-GatewayType* değeri **Vpn** ve *-VpnType* değeri **RouteBased** olmalıdır. Bu işlemin tamamlanması 45 dakika sürebilir.

        New-AzureRmVirtualNetworkGateway -Name $GWName -ResourceGroupName $RG `
        -Location $Location -IpConfigurations $ipconf -GatewayType Vpn `
        -VpnType RouteBased -EnableBgp $false -GatewaySku Standard `
        -VpnClientAddressPool $VPNClientAddressPool -VpnClientRootCertificates $p2srootcert

## Bölüm 5 - VPN istemcisi yapılandırma paketini indirin

P2S kullanarak Azure’a bağlanan istemcilerde hem bir istemci sertifikası hem de bir VPN istemcisi yapılandırma paketi yüklü olmalıdır. Windows istemcileri için VPN istemcisi yapılandırma paketleri kullanılabilir. VPN istemci paketi Windows’ta yerleşik olan VPN istemci yazılımını yapılandırmaya yönelik bilgiler içerir ve bağlanmak istediğiniz VPN’e özeldir. Paket, ek yazılım yüklemez. Daha fazla bilgi edinmek için bkz. [VPN Gateway ile ilgili SSS](vpn-gateway-vpn-faq.md#point-to-site-connections).

1. Ağ geçidi oluşturulduktan sonra istemci yapılandırma paketini indirmek için aşağıdaki örneği kullanın:

        Get-AzureRmVpnClientPackage -ResourceGroupName $RG `
        -VirtualNetworkGatewayName $GWName -ProcessorArchitecture Amd64

2. PowerShell cmdlet'i bir URL bağlantısı döndürür. Bu örnek döndürülen URL'nin nasıl göründüğüne ilişkin bir örnektir:

        "https://mdsbrketwprodsn1prod.blob.core.windows.net/cmakexe/4a431aa7-b5c2-45d9-97a0-859940069d3f/amd64/4a431aa7-b5c2-45d9-97a0-859940069d3f.exe?sv=2014-02-14&sr=b&sig=jSNCNQ9aUKkCiEokdo%2BqvfjAfyhSXGnRG0vYAv4efg0%3D&st=2016-01-08T07%3A10%3A08Z&se=2016-01-08T08%3A10%3A08Z&sp=r&fileExtension=.exe"

3. Paketi indirmek için döndürülen bağlantıyı kopyalayıp bir web tarayıcısına yapıştırın. Ardından paketi istemci bilgisayara yükleyin.

4. İstemci bilgisayarda **Ağ Ayarları**’na gidin ve **VPN** öğesine tıklayın. Bağlantının listelendiğini görürsünüz. Bu listede bağlantı kurulacak sanal ağın adı gösterilir ve liste aşağıdaki gibi görünür: 

    ![VPN istemcisi](./media/vpn-gateway-howto-point-to-site-rm-ps/vpn.png "VPN client")

## <a name="cc"></a>Bölüm 6 - İstemci sertifikası oluşturma

Bir sonraki adımda istemci sertifikalarını oluşturacaksınız. Bağlanacak her istemci için benzersiz bir sertifika oluşturabileceğiniz gibi, birden çok istemcide aynı sertifikayı da kullanabilirsiniz. Benzersiz istemci sertifikaları oluşturmanın avantajı, gerektiğinde tek bir sertifikayı iptal edebiliyor olmanızdır. Herkesin aynı istemci sertifikasını kullandığı bir durumda bir istemcinin sertifikasını iptal etmeniz gerektiğinde, kimlik doğrulaması için söz konusu sertifikayı kullanan tüm istemciler için yeni sertifikalar oluşturmanız ve yüklemeniz gerekir.

- Kurumsal bir sertifika çözümü kullanıyorsanız NetBIOS "ETKİALANI\kullanıcıadı" biçimini kullanmak yerine, yaygın olarak kullanılan "ad@etkialanınız.com" ad değer biçimiyle bir istemci sertifikası oluşturun. 

- Otomatik olarak imzalanan sertifika kullanıyorsanız istemci sertifikası oluşturmak için bkz. [Noktadan Siteye yapılandırmaları için otomatik olarak imzalanan kök sertifikalar ile çalışma](vpn-gateway-certificates-point-to-site.md).

## Bölüm 7 - İstemci sertifikası yükleme

Sanal ağa bağlamak istediğiniz her bilgisayara bir istemci sertifikası yükleyin. Kimlik doğrulaması için istemci sertifikası gereklidir. İstemci sertifikası yükleme işlemini otomatik hale getirebilir veya elle yapabilirsiniz. Aşağıda, istemci sertifikasını dışarı aktarma ve elle yükleme işlemleri adım adım açıklanmıştır.

1. İstemci sertifikasını dışarı aktarmak için *certmgr.msc* kullanabilirsiniz. Dışarı aktarmak istediğiniz istemci sertifikasına sağ tıklayın, **tüm görevler**’e ve ardından **dışarı aktar**’a tıklayın.
2. Özel anahtara sahip istemci sertifikasını dışarı aktarın. Bu bir *.pfx* dosyasıdır. Bu sertifika için ayarladığınız parolayı (anahtar) kaydettiğinizden ya da unutmayacağınızdan emin olun.
3. *.pfx* dosyasını istemci bilgisayara kopyalayın. İstemci bilgisayarda *.pfx* dosyasına çift tıklayarak yükleme işlemini gerçekleştirin. İstendiğinde parolayı girin. Yükleme konumunu değiştirmeyin.


## Bölüm 8 - Azure'a Bağlanma

1. İstemci bilgisayarda VNet'inize bağlanmak için VPN bağlantılarında gezinin ve oluşturduğunuz VPN bağlantısını bulun. Bu VPN bağlantısı sanal ağınızla aynı ada sahiptir. **Bağlan**'a tıklayın. Sertifika kullanımına ilişkin bir açılır ileti görüntülenebilir. Böyle bir durumla karşılaşırsanız yükseltilmiş ayrıcalıkları kullanmak için **Devam**'a tıklayın. 

2. **Bağlantı** durum sayfasında **Bağlan**'a tıklayarak bağlantıyı başlatın. Bir **Sertifika Seç** ekranı çıkarsa, gösterilen istemci sertifikasının bağlanmak için kullanmak istediğiniz sertifika olduğunu doğrulayın. Başka bir sertifika gösteriliyorsa, açılan liste okunu kullanarak doğru sertifikayı seçin ve **Tamam**’a tıklayın.

    ![VPN istemcisi 2](./media/vpn-gateway-howto-point-to-site-rm-ps/clientconnect.png "VPN client connection")

3. Artık bağlantı kurabilirsiniz.

    ![VPN istemcisi 3](./media/vpn-gateway-howto-point-to-site-rm-ps/connected.png "VPN client connection 2")

## Bölüm 9 - Bağlantınızı doğrulama

1. VPN bağlantınızın etkin olduğunu doğrulamak için, yükseltilmiş bir komut istemi açın ve *ipconfig/all* komutunu çalıştırın.

2. Sonuçlara bakın. Aldığınız IP adresinin, yapılandırmanızda belirttiğiniz Noktadan Siteye VPN İstemcisi Adres Havuzu'ndaki adreslerden biri olduğuna dikkat edin. Sonuçlar aşağıdakine benzer olmalıdır:
    
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

## Güvenilen kök sertifika ekleme veya kaldırma

Noktadan Siteye VPN’lerde VPN istemcilerinin kimlik doğrulamasını yapmak için sertifikalar kullanılır. Aşağıdaki adımlarda kök sertifika ekleme ve kaldırma işlemleri ayrıntılı olarak açıklanmıştır. Azure'a Base64 ile kodlanmış bir X.509 (*.cer) dosyası eklediğinizde, Azure'a dosyanın temsil ettiği kök sertifikaya güvenebileceğini söylemiş olursunuz. 

PowerShell kullanarak veya Azure portalında güvenilir kök sertifikaları ekleyebilir veya kaldırabilirsiniz. Bu işlemi Azure portalı ile yapmak istiyorsanız **sanal ağ geçidi > ayarlar > Noktadan siteye yapılandırma > Kök sertifikalar** konumuna gidin. Aşağıdaki adımlar bu görevleri PowerShell kullanarak gerçekleştirmeyi açıklamaktadır. 

### Güvenilen kök sertifika ekleme

Azure'a en fazla 20 güvenilen kök sertifika .cer dosyası ekleyebilirsiniz. Kök sertifika eklemek için aşağıdaki adımları uygulayın.

1. Azure'a ekleyeceğiniz yeni kök sertifikayı oluşturup hazırlayın. Genel anahtarı Base-64 kodlanmış X.509 (.CER) olarak dışarı aktarın ve bir metin düzenleyicisi ile açın. Ardından yalnızca aşağıda gösterilen bölümü kopyalayın. 
 
    Aşağıdaki örnekte gösterildiği gibi değerleri kopyalayın.

    ![sertifika](./media/vpn-gateway-howto-point-to-site-rm-ps/copycert.png "certificate")
    
2. Aşağıdaki örnekte sertifika adını ve anahtar bilgilerini bir değişken olarak belirtin. Bilgileri kendi bilgilerinizle değiştirin.

        $P2SRootCertName2 = "ARMP2SRootCert2.cer"
        $MyP2SCertPubKeyBase64_2 = "MIIC/zCCAeugAwIBAgIQKazxzFjMkp9JRiX+tkTfSzAJBgUrDgMCHQUAMBgxFjAUBgNVBAMTDU15UDJTUm9vdENlcnQwHhcNMTUxMjE5MDI1MTIxWhcNMzkxMjMxMjM1OTU5WjAYMRYwFAYDVQQDEw1NeVAyU1Jvb3RDZXJ0MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAyjIXoWy8xE/GF1OSIvUaA0bxBjZ1PJfcXkMWsHPzvhWc2esOKrVQtgFgDz4ggAnOUFEkFaszjiHdnXv3mjzE2SpmAVIZPf2/yPWqkoHwkmrp6BpOvNVOpKxaGPOuK8+dql1xcL0eCkt69g4lxy0FGRFkBcSIgVTViS9wjuuS7LPo5+OXgyFkAY3pSDiMzQCkRGNFgw5WGMHRDAiruDQF1ciLNojAQCsDdLnI3pDYsvRW73HZEhmOqRRnJQe6VekvBYKLvnKaxUTKhFIYwuymHBB96nMFdRUKCZIiWRIy8Hc8+sQEsAML2EItAjQv4+fqgYiFdSWqnQCPf/7IZbotgQIDAQABo00wSzBJBgNVHQEEQjBAgBAkuVrWvFsCJAdK5pb/eoCNoRowGDEWMBQGA1UEAxMNTXlQMlNSb290Q2VydIIQKazxzFjMkp9JRiX+tkTfSzAJBgUrDgMCHQUAA4IBAQA223veAZEIar9N12ubNH2+HwZASNzDVNqspkPKD97TXfKHlPlIcS43TaYkTz38eVrwI6E0yDk4jAuPaKnPuPYFRj9w540SvY6PdOUwDoEqpIcAVp+b4VYwxPL6oyEQ8wnOYuoAK1hhh20lCbo8h9mMy9ofU+RP6HJ7lTqupLfXdID/XevI8tW6Dm+C/wCeV3EmIlO9KUoblD/e24zlo3YzOtbyXwTIh34T0fO/zQvUuBqZMcIPfM1cDvqcqiEFLWvWKoAnxbzckye2uk1gHO52d8AVL3mGiX8wBJkjc/pMdxrEvvCzJkltBmqxTM6XjDJALuVh16qFlqgTWCIcb7ju"


3. Yeni kök sertifikayı ekleyin. Tek seferde yalnızca bir sertifika ekleyebilirsiniz.

        Add-AzureRmVpnClientRootCertificate -VpnClientRootCertificateName $P2SRootCertName2 -VirtualNetworkGatewayname "GW" -ResourceGroupName "TestRG" -PublicCertData $MyP2SCertPubKeyBase64_2


4. Aşağıdaki cmdlet'i kullanarak yeni sertifikanın düzgün şekilde eklendiğini doğrulayabilirsiniz.

        Get-AzureRmVpnClientRootCertificate -ResourceGroupName "TestRG" `
        -VirtualNetworkGatewayName "GW"

### Güvenilen kök sertifikayı kaldırmak için

Azure'dan güvenilen kök sertifikayı kaldırabilirsiniz. Güvenilen kök sertifikayı kaldırdığınızda, kök sertifikadan oluşturulan istemci sertifikaları artık Noktadan Siteye bağlantı aracılığıyla Azure'a bağlanamaz. İstemciler, bağlanabilmek için Azure'da güvenilen bir sertifikadan oluşturan yeni bir istemci sertifikası yüklemelidir.

1. Güvenilen bir kök sertifikayı kaldırmak için aşağıdaki örneği değiştirin:

    Değişkenleri bildirin.

        $P2SRootCertName2 = "ARMP2SRootCert2.cer"
        $MyP2SCertPubKeyBase64_2 = "MIIC/zCCAeugAwIBAgIQKazxzFjMkp9JRiX+tkTfSzAJBgUrDgMCHQUAMBgxFjAUBgNVBAMTDU15UDJTUm9vdENlcnQwHhcNMTUxMjE5MDI1MTIxWhcNMzkxMjMxMjM1OTU5WjAYMRYwFAYDVQQDEw1NeVAyU1Jvb3RDZXJ0MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAyjIXoWy8xE/GF1OSIvUaA0bxBjZ1PJfcXkMWsHPzvhWc2esOKrVQtgFgDz4ggAnOUFEkFaszjiHdnXv3mjzE2SpmAVIZPf2/yPWqkoHwkmrp6BpOvNVOpKxaGPOuK8+dql1xcL0eCkt69g4lxy0FGRFkBcSIgVTViS9wjuuS7LPo5+OXgyFkAY3pSDiMzQCkRGNFgw5WGMHRDAiruDQF1ciLNojAQCsDdLnI3pDYsvRW73HZEhmOqRRnJQe6VekvBYKLvnKaxUTKhFIYwuymHBB96nMFdRUKCZIiWRIy8Hc8+sQEsAML2EItAjQv4+fqgYiFdSWqnQCPf/7IZbotgQIDAQABo00wSzBJBgNVHQEEQjBAgBAkuVrWvFsCJAdK5pb/eoCNoRowGDEWMBQGA1UEAxMNTXlQMlNSb290Q2VydIIQKazxzFjMkp9JRiX+tkTfSzAJBgUrDgMCHQUAA4IBAQA223veAZEIar9N12ubNH2+HwZASNzDVNqspkPKD97TXfKHlPlIcS43TaYkTz38eVrwI6E0yDk4jAuPaKnPuPYFRj9w540SvY6PdOUwDoEqpIcAVp+b4VYwxPL6oyEQ8wnOYuoAK1hhh20lCbo8h9mMy9ofU+RP6HJ7lTqupLfXdID/XevI8tW6Dm+C/wCeV3EmIlO9KUoblD/e24zlo3YzOtbyXwTIh34T0fO/zQvUuBqZMcIPfM1cDvqcqiEFLWvWKoAnxbzckye2uk1gHO52d8AVL3mGiX8wBJkjc/pMdxrEvvCzJkltBmqxTM6XjDJALuVh16qFlqgTWCIcb7ju"

2. Sertifikayı kaldırma

        Remove-AzureRmVpnClientRootCertificate -VpnClientRootCertificateName $P2SRootCertName2 -VirtualNetworkGatewayName $GWName -ResourceGroupName $RG -PublicCertData $MyP2SCertPubKeyBase64_2
 
3. Sertifikanın başarıyla kaldırıldığını doğrulamak için aşağıdaki cmdlet'i kullanın. 

        Get-AzureRmVpnClientRootCertificate -ResourceGroupName "TestRG" `
        -VirtualNetworkGatewayName "GW"

## İptal edilen istemci sertifikalarının listesini yönetmek için

İstemci sertifikalarını iptal edebilirsiniz. Sertifika iptal listesi sayesinde, ayrı istemci sertifikalarına göre Noktadan Siteye bağlantıyı seçmeli olarak reddedebilirsiniz. Azure'dan kök sertifika .cer dosyasını kaldırırsanız iptal edilen kök sertifika tarafından oluşturulan/imzalanan tüm istemci sertifikaları reddedilir. Kök sertifika yerine belirli bir istemci sertifikasını iptal etmek istiyorsanız bu işlemi gerçekleştirebilirsiniz. Bu sayede, kök sertifika tarafından oluşturulan diğer sertifikalar geçerli olmaya devam eder. Genellikle ekip ve kuruluş düzeylerinde erişimi yönetmek için kök sertifika kullanılırken ayrı kullanıcılar üzerinde ayrıntılı erişim denetimi için iptal edilen istemci sertifikaları kullanılır.

### İstemci sertifikasını iptal etme

1. İptal edilecek istemci sertifikasının parmak izini alın.

        $RevokedClientCert1 = "ClientCert1"
        $RevokedThumbprint1 = "‎ef2af033d0686820f5a3c74804d167b88b69982f"

2. Parmak izini, iptal edilen parmak izi listesine ekleyin.

        Add-AzureRmVpnClientRevokedCertificate -VpnClientRevokedCertificateName $RevokedClientCert1 `
        -VirtualNetworkGatewayName $GWName -ResourceGroupName $RG -Thumbprint $RevokedThumbprint1

3. Parmak izinin, sertifika iptal listesine eklendiğini doğrulayın. Tek seferde yalnızca bir parmak izi eklemeniz gerekir.

        Get-AzureRmVpnClientRevokedCertificate -VirtualNetworkGatewayName $GWName -ResourceGroupName $RG

### İstemci sertifikasını yeniden devreye sokma

Parmak izini, iptal edilen istemci sertifikaları listesinden kaldırarak bir istemci sertifikasını yeniden devreye sokabilirsiniz.

1.  Parmak izini, iptal edilen istemci sertifikası parmak izi listesinden kaldırın.

        Remove-AzureRmVpnClientRevokedCertificate -VpnClientRevokedCertificateName $RevokedClientCert1 `
        -VirtualNetworkGatewayName $GWName -ResourceGroupName $RG -Thumbprint $RevokedThumbprint1

2. Parmak izinin, iptal edilen parmak izi listesinden kaldırılıp kaldırılmadığını kontrol edin.

        Get-AzureRmVpnClientRevokedCertificate -VirtualNetworkGatewayName $GWName -ResourceGroupName $RG

## Sonraki adımlar

Sanal ağınıza sanal makine ekleyebilirsiniz. Adımlar için bkz. [Sanal Makine Oluşturma](../virtual-machines/virtual-machines-windows-hero-tutorial.md).





<!--HONumber=sep16_HO1-->


