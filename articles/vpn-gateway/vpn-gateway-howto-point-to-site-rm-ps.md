<properties 
   pageTitle="Resource Manager dağıtım modelini kullanarak sanal ağa yönelik bir Noktadan Siteye VPN bağlantısı yapılandırma | Microsoft Azure"
   description="Noktadan Siteye bir VPN bağlantısı oluşturarak Azure Sanal Ağınıza güvenli bir şekilde bağlanın."
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
   ms.date="08/16/2016"
   ms.author="cherylmc" />

# PowerShell'i kullanarak sanal ağa yönelik bir Noktadan Siteye bağlantısı yapılandırma

> [AZURE.SELECTOR]
- [PowerShell - Resource Manager](vpn-gateway-howto-point-to-site-rm-ps.md)
- [Portal - Klasik](vpn-gateway-point-to-site-create.md)

Noktadan Siteye (P2S) yapılandırması, ayrı bir istemci bilgisayardan bir sanal ağa yönelik güvenli bağlantı oluşturmanıza olanak sağlar. Sanal ağınıza uzak bir konumdan (örneğin, evden veya bir konferanstan) bağlanmak istediğinizde ya da sanal bir ağa bağlanması gereken yalnızca birkaç istemciniz bulunduğunda P2S bağlantısı kullanışlıdır. 

Noktadan Siteye bağlantıların çalışması için bir VPN cihazına veya genel kullanıma yönelik bir IP adresine gerek yoktur. VPN bağlantısı, bağlantının istemci bilgisayardan başlatılmasıyla oluşturulur. Noktadan Siteye bağlantılar hakkında daha fazla bilgi edinmek için [VPN Gateway ile ilgili SSS](vpn-gateway-vpn-faq.md#point-to-site-connections) ve [Planlama ve Tasarım](vpn-gateway-plan-design.md) başlıklı makalelere bakın.

Bu makalede, Resource Manager dağıtım modeli kullanılarak oluşturulan bir sanal ağa yönelik Noktadan Siteye bağlantılar ele alınmaktadır. Bu makaledeki adımlarda PowerShell kullanılmaktadır.

**Azure dağıtım modelleri hakkında**

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)] 

**Noktadan Siteye bağlantılar için dağıtım modelleri ve araçları**

[AZURE.INCLUDE [vpn-gateway-table-point-to-site](../../includes/vpn-gateway-table-point-to-site-include.md)] 

![Noktadan Siteye diyagramı](./media/vpn-gateway-point-to-site-create/point2site.png "point-to-site")


## Bu yapılandırma hakkında

Bu senaryoda, Noktadan Siteye bağlantı ile bir sanal ağ oluşturacaksınız. P2S bağlantısı şu öğelerden oluşur: Ağ geçidine sahip bir VNet, kök sertifika .cer dosyası, istemci sertifikası ve istemci tarafındaki VPN yapılandırması. 

Bu yapılandırma için şu değerleri kullanacağız:

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
    
- Azure Resource Manager PowerShell cmdlet'lerini (1.0.2 veya sonraki bir sürümü) yükleyin. PowerShell cmdlet'lerini yükleme hakkında daha fazla bilgi için bkz. [Azure PowerShell'i yükleme ve yapılandırma](../powershell-install-configure.md).

## Azure için Noktadan Siteye bağlantısı yapılandırma

1. PowerShell konsoluna giderek Azure hesabınızda oturum açın. Bu cmdlet, Azure Hesabınıza ilişkin oturum açma kimlik bilgilerinizi girmenizi ister. Oturum açtıktan sonra, Azure PowerShell'de kullanabilmeniz için hesap ayarlarınızı indirir.

        Login-AzureRmAccount 

2. Azure aboneliklerinizin bir listesini alın.

        Get-AzureRmSubscription

3. Kullanmak istediğiniz aboneliği belirtin. 

        Select-AzureRmSubscription -SubscriptionName "Name of subscription"

4. Bu yapılandırmada kullanmak istediğiniz değerlerle birlikte aşağıdaki PowerShell değişkenleri belirtilir. Belirtilen değerler örnek betiklerde kullanılır. Kullanmak istediğiniz değerleri belirtin. Aşağıdaki örneği kullanın ve gerektiğinde, değerleri kendi değerlerinizle değiştirin. 

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

5. Yeni bir kaynak grubu oluşturun.

        New-AzureRmResourceGroup -Name $RG -Location $Location

6. Sanal ağ için alt ağ yapılandırmalarını oluşturup *FrontEnd*, *BackEnd* ve *GatewaySubnet* olarak adlandırın. Bu ön ekler, yukarıda belirtilen VNet adres alanının parçası olmalıdır.

        $fesub = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName -AddressPrefix $FESubPrefix
        $besub = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName -AddressPrefix $BESubPrefix
        $gwsub = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName -AddressPrefix $GWSubPrefix

7. Sanal ağı oluşturun. Belirtilen DNS sunucusu, bağlandığınız kaynakların adlarını çözümleyebilen bir DNS sunucusu olmalıdır. Bu örnekte genel IP adresi kullandık. Kendi değerlerinizi kullandığınızdan emin olun.
    
        New-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $RG -Location $Location -AddressPrefix $VNetPrefix1,$VNetPrefix2 -Subnet $fesub, $besub, $gwsub -DnsServer $DNS

8. Oluşturduğunuz sanal ağa ilişkin değişkenleri belirtin.

        $vnet = Get-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $RG
        $subnet = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet

9. Dinamik olarak atanan bir genel IP adresi isteyin. Ağ geçidinin düzgün çalışması için bu IP adresi gereklidir. Daha sonra, ağ geçidini ağ geçidi IP yapılandırmasına bağlayacaksınız.
        
        $pip = New-AzureRmPublicIpAddress -Name $GWIPName -ResourceGroupName $RG -Location $Location -AllocationMethod Dynamic
        $ipconf = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GWIPconfName -Subnet $subnet -PublicIpAddress $pip
        
10. Azure'a güvenilen bir sertifika ekleyin. En fazla 20 sertifika ekleyebilirsiniz. *makecert* ile otomatik olarak imzalanan kök sertifikası oluşturmaya ilişkin yönergeler için bkz. [Noktadan Siteye yapılandırmaları için otomatik olarak imzalanan kök sertifikalar ile çalışma](vpn-gateway-certificates-point-to-site.md). Azure'a Base64 ile kodlanmış bir X.509 (*.cer) dosyası eklediğinizde, Azure'a dosyanın temsil ettiği kök sertifikaya güvenebileceğini söylemiş olursunuz.

    Ortak anahtarı almak için sertifikayı Base64 ile kodlanmış bir X.509 (.CER) dosyası olarak dışarı aktarın. .cer dosyasını aktardığınız dosya yolunu not edin. Bu, sertifikanızın Base64 dize gösterimini almaya ilişkin bir örnektir. Bu adımda kendi .cer dosya yolunuzu kullanmanız gerekir.
    
        $filePathForCert = "pasteYourCerFilePathHere"
        $cert = new-object System.Security.Cryptography.X509Certificates.X509Certificate2($filePathForCert)
        $CertBase64 = [system.convert]::ToBase64String($cert.RawData)
        $p2srootcert = New-AzureRmVpnClientRootCertificate -Name $P2SRootCertName -PublicCertData $CertBase64


11. VNet'iniz için sanal ağ geçidini oluşturun. *-GatewayType* değeri **Vpn** ve *-VpnType* değeri **RouteBased** olmalıdır.

        New-AzureRmVirtualNetworkGateway -Name $GWName -ResourceGroupName $RG `
        -Location $Location -IpConfigurations $ipconf -GatewayType Vpn `
        -VpnType RouteBased -EnableBgp $false -GatewaySku Standard `
        -VpnClientAddressPool $VPNClientAddressPool -VpnClientRootCertificates $p2srootcert

## İstemci yapılandırması

Noktadan Siteye bağlantısını kullanarak Azure'a bağlanan tüm istemciler için şu iki şey zorunludur: VPN istemcisi, bağlanmak üzere yapılandırılmalıdır ve istemcide bir istemci sertifikası yüklü olmalıdır. Windows istemcileri için VPN istemcisi yapılandırma paketleri kullanılabilir. Daha fazla bilgi edinmek için bkz. [VPN Gateway ile ilgili SSS](vpn-gateway-vpn-faq.md#point-to-site-connections). 

1. VPN istemcisi yapılandırma paketini indirin. Bu adımda aşağıdaki örneği kullanarak istemci yapılandırma paketini indirin.

        Get-AzureRmVpnClientPackage -ResourceGroupName $RG `
        -VirtualNetworkGatewayName $GWName -ProcessorArchitecture Amd64

    PowerShell cmdlet'i bir URL bağlantısı döndürür. Paketi bilgisayarınıza indirmek için, döndürülen bağlantıyı kopyalayıp bir web tarayıcısına yapıştırın. Bu, döndürülen URL'nin nasıl göründüğüne ilişkin bir örnektir.

        "https://mdsbrketwprodsn1prod.blob.core.windows.net/cmakexe/4a431aa7-b5c2-45d9-97a0-859940069d3f/amd64/4a431aa7-b5c2-45d9-97a0-859940069d3f.exe?sv=2014-02-14&sr=b&sig=jSNCNQ9aUKkCiEokdo%2BqvfjAfyhSXGnRG0vYAv4efg0%3D&st=2016-01-08T07%3A10%3A08Z&se=2016-01-08T08%3A10%3A08Z&sp=r&fileExtension=.exe"
    
2. İstemci bilgisayarlardaki kök sertifikadan oluşturulan istemci sertifikalarını (*.pfx) oluşturup yükleyin. Yükleme işlemi için istediğiniz herhangi bir yöntemi kullanabilirsiniz. Otomatik olarak imzalanan kök sertifika kullanıyor ve bu işlemi nasıl gerçekleştireceğinizi bilmiyorsanız [Noktadan Siteye yapılandırmaları için otomatik olarak imzalanan kök sertifikalar ile çalışma](vpn-gateway-certificates-point-to-site.md) adlı makaleye göz atabilirsiniz. 

3. İstemci bilgisayarda VNet'inize bağlanmak için VPN bağlantılarında gezinin ve oluşturduğunuz VPN bağlantısını bulun. Bu VPN bağlantısı sanal ağınızla aynı ada sahiptir. **Bağlan**'a tıklayın. Sertifika kullanımına ilişkin bir açılır ileti görüntülenebilir. Böyle bir durumla karşılaşırsanız yükseltilmiş ayrıcalıkları kullanmak için **Devam**'a tıklayın. 

4. **Bağlantı** durum sayfasında **Bağlan**'a tıklayarak bağlantıyı başlatın. Bir **Sertifika Seç** ekranı çıkarsa, gösterilen istemci sertifikasının bağlanmak için kullanmak istediğiniz sertifika olduğunu doğrulayın. Başka bir sertifika gösteriliyorsa, açılan liste okunu kullanarak doğru sertifikayı seçin ve **Tamam**’a tıklayın.

5. Artık bağlantı kurabilirsiniz. Aşağıdaki yordamı kullanarak bağlantınızı doğrulayabilirsiniz.

## Bağlantınızı doğrulama

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

### Güvenilen kök sertifika ekleme

Azure'a en fazla 20 güvenilen kök sertifika .cer dosyası ekleyebilirsiniz. Kök sertifika eklemek için aşağıdaki adımları uygulayın.

1. Azure'a ekleyeceğiniz yeni kök sertifikayı oluşturup hazırlayın.

        $P2SRootCertName2 = "ARMP2SRootCert2.cer"
        $MyP2SCertPubKeyBase64_2 = "MIIC/zCCAeugAwIBAgIQKazxzFjMkp9JRiX+tkTfSzAJBgUrDgMCHQUAMBgxFjAUBgNVBAMTDU15UDJTUm9vdENlcnQwHhcNMTUxMjE5MDI1MTIxWhcNMzkxMjMxMjM1OTU5WjAYMRYwFAYDVQQDEw1NeVAyU1Jvb3RDZXJ0MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAyjIXoWy8xE/GF1OSIvUaA0bxBjZ1PJfcXkMWsHPzvhWc2esOKrVQtgFgDz4ggAnOUFEkFaszjiHdnXv3mjzE2SpmAVIZPf2/yPWqkoHwkmrp6BpOvNVOpKxaGPOuK8+dql1xcL0eCkt69g4lxy0FGRFkBcSIgVTViS9wjuuS7LPo5+OXgyFkAY3pSDiMzQCkRGNFgw5WGMHRDAiruDQF1ciLNojAQCsDdLnI3pDYsvRW73HZEhmOqRRnJQe6VekvBYKLvnKaxUTKhFIYwuymHBB96nMFdRUKCZIiWRIy8Hc8+sQEsAML2EItAjQv4+fqgYiFdSWqnQCPf/7IZbotgQIDAQABo00wSzBJBgNVHQEEQjBAgBAkuVrWvFsCJAdK5pb/eoCNoRowGDEWMBQGA1UEAxMNTXlQMlNSb290Q2VydIIQKazxzFjMkp9JRiX+tkTfSzAJBgUrDgMCHQUAA4IBAQA223veAZEIar9N12ubNH2+HwZASNzDVNqspkPKD97TXfKHlPlIcS43TaYkTz38eVrwI6E0yDk4jAuPaKnPuPYFRj9w540SvY6PdOUwDoEqpIcAVp+b4VYwxPL6oyEQ8wnOYuoAK1hhh20lCbo8h9mMy9ofU+RP6HJ7lTqupLfXdID/XevI8tW6Dm+C/wCeV3EmIlO9KUoblD/e24zlo3YzOtbyXwTIh34T0fO/zQvUuBqZMcIPfM1cDvqcqiEFLWvWKoAnxbzckye2uk1gHO52d8AVL3mGiX8wBJkjc/pMdxrEvvCzJkltBmqxTM6XjDJALuVh16qFlqgTWCIcb7ju"

2. Yeni kök sertifikayı ekleyin. Tek seferde yalnızca bir sertifika ekleyebilirsiniz.

        Add-AzureRmVpnClientRootCertificate -VpnClientRootCertificateName $P2SRootCertName2 -VirtualNetworkGatewayname $GWName -ResourceGroupName $RG -PublicCertData $MyP2SCertPubKeyBase64_2

3. Aşağıdaki cmdlet'i kullanarak yeni sertifikanın düzgün şekilde eklendiğini doğrulayabilirsiniz.

        Get-AzureRmVpnClientRootCertificate -ResourceGroupName $RG `
        -VirtualNetworkGatewayName $GWName

### Güvenilen kök sertifikayı kaldırma

Azure'dan güvenilen kök sertifikayı kaldırabilirsiniz. Güvenilen kök sertifikayı kaldırdığınızda, kök sertifikadan oluşturulan istemci sertifikaları artık Noktadan Siteye bağlantı aracılığıyla Azure'a bağlanamaz. İstemciler, bağlanabilmek için Azure'da güvenilen bir sertifikadan oluşturan yeni bir istemci sertifikası yüklemelidir.

1. Güvenilen bir kök sertifikayı kaldırmak için aşağıdaki örneği değiştirin.

        Remove-AzureRmVpnClientRootCertificate -VpnClientRootCertificateName $P2SRootCertName2 -VirtualNetworkGatewayName $GWName -ResourceGroupName $RG -PublicCertData "MIIC/zCCAeugAwIBAgIQKazxzFjMkp9JRiX+tkTfSzAJBgUrDgMCHQUAMBgxFjAUBgNVBAMTDU15UDJTUm9vdENlcnQwHhcNMTUxMjE5MDI1MTIxWhcNMzkxMjMxMjM1OTU5WjAYMRYwFAYDVQQDEw1NeVAyU1Jvb3RDZXJ0MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAyjIXoWy8xE/GF1OSIvUaA0bxBjZ1PJfcXkMWsHPzvhWc2esOKrVQtgFgDz4ggAnOUFEkFaszjiHdnXv3mjzE2SpmAVIZPf2/yPWqkoHwkmrp6BpOvNVOpKxaGPOuK8+dql1xcL0eCkt69g4lxy0FGRFkBcSIgVTViS9wjuuS7LPo5+OXgyFkAY3pSDiMzQCkRGNFgw5WGMHRDAiruDQF1ciLNojAQCsDdLnI3pDYsvRW73HZEhmOqRRnJQe6VekvBYKLvnKaxUTKhFIYwuymHBB96nMFdRUKCZIiWRIy8Hc8+sQEsAML2EItAjQv4+fqgYiFdSWqnQCPf/7IZbotgQIDAQABo00wSzBJBgNVHQEEQjBAgBAkuVrWvFsCJAdK5pb/eoCNoRowGDEWMBQGA1UEAxMNTXlQMlNSb290Q2VydIIQKazxzFjMkp9JRiX+tkTfSzAJBgUrDgMCHQUAA4IBAQA223veAZEIar9N12ubNH2+HwZASNzDVNqspkPKD97TXfKHlPlIcS43TaYkTz38eVrwI6E0yDk4jAuPaKnPuPYFRj9w540SvY6PdOUwDoEqpIcAVp+b4VYwxPL6oyEQ8wnOYuoAK1hhh20lCbo8h9mMy9ofU+RP6HJ7lTqupLfXdID/XevI8tW6Dm+C/wCeV3EmIlO9KUoblD/e24zlo3YzOtbyXwTIh34T0fO/zQvUuBqZMcIPfM1cDvqcqiEFLWvWKoAnxbzckye2uk1gHO52d8AVL3mGiX8wBJkjc/pMdxrEvvCzJkltBmqxTM6XjDJALuVh16qFlqgTWCIcb7ju"

 
2. Sertifikanın başarıyla kaldırıldığını doğrulamak için aşağıdaki cmdlet'i kullanın. 

        Get-AzureRmVpnClientRootCertificate -ResourceGroupName $RG -VirtualNetworkGatewayName $GWName

## İptal edilen istemci sertifikalarının listesini yönetme

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





<!--HONumber=Aug16_HO4-->


