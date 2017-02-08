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
ms.date: 01/19/2017
ms.author: cherylmc
translationtype: Human Translation
ms.sourcegitcommit: 70b6c9b8cb14e4e0b1162162071c03174bb150c1
ms.openlocfilehash: 48f93102d70da0e1e593afe1cea1a0af3a78f8e0


---
# <a name="configure-a-point-to-site-connection-to-a-vnet-using-powershell"></a>PowerShell'i kullanarak VNet’e yönelik bir Noktadan Siteye bağlantısı yapılandırma
> [!div class="op_single_selector"]
> * [Resource Manager - Azure Portal](vpn-gateway-howto-point-to-site-resource-manager-portal.md)
> * [Resource Manager - PowerShell](vpn-gateway-howto-point-to-site-rm-ps.md)
> * [Klasik - Azure Portal](vpn-gateway-howto-point-to-site-classic-azure-portal.md)
> 
> 

Noktadan Siteye (P2S) yapılandırması, ayrı bir istemci bilgisayardan bir sanal ağa yönelik güvenli bağlantı oluşturmanıza olanak sağlar. Sanal ağınıza uzak bir konumdan (örneğin, evden veya bir konferanstan) bağlanmak istediğinizde ya da sanal bir ağa bağlanması gereken yalnızca birkaç istemciniz bulunduğunda P2S bağlantısı kullanışlıdır. 

Noktadan Siteye bağlantıların çalışması için bir VPN cihazına veya genel kullanıma yönelik bir IP adresine gerek yoktur. VPN bağlantısı, bağlantının istemci bilgisayardan başlatılmasıyla oluşturulur. Noktadan Siteye bağlantılar hakkında daha fazla bilgi edinmek için [VPN Gateway ile ilgili SSS](vpn-gateway-vpn-faq.md#point-to-site-connections) ve [Planlama ve Tasarım](vpn-gateway-plan-design.md) başlıklı makalelere bakın. 

Bu makalede PowerShell kullanılarak Resource Manager dağıtım modelinde Noktadan Siteye bağlantı ile sanal ağ oluşturma işlemi adım adım açıklanmaktadır.

### <a name="deployment-models-and-methods-for-p2s-connections"></a>P2S bağlantıları için dağıtım modelleri ve yöntemleri
[!INCLUDE [deployment models](../../includes/vpn-gateway-deployment-models-include.md)]

Aşağıdaki tabloda, P2S yapılandırmaları için iki dağıtım modeli ve kullanılabilir dağıtım yöntemleri gösterilmektedir. Yapılandırma adımlarını içeren bir makale olduğunda, bu tablodan makaleye yönelik doğrudan bağlantı oluştururuz.

[!INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-table-point-to-site-include.md)]

## <a name="basic-workflow"></a>Temel iş akışı
![Bir bilgisayarı Azure sanal ağına bağlama - Noktadan Siteye bağlantı diyagramı](./media/vpn-gateway-howto-point-to-site-rm-ps/point-to-site-diagram.png)

Bu senaryoda, Noktadan Siteye bağlantı ile bir sanal ağ oluşturacaksınız. Yönergeler ayrıca bu yapılandırma için gerekli olan sertifikaları oluşturmanıza yardımcı olur. P2S bağlantısı şu öğelerden oluşur: VPN ağ geçidine sahip bir VNet, kök sertifika .cer dosyası (ortak anahtar), istemci sertifikası ve istemci üzerindeki VPN yapılandırması. 

Bu yapılandırma için aşağıdaki değerler kullanılır. Değişkenler makalenin [1](#declare). bölümünde ayarlanır. İzlenecek yol olarak adımları kullanıp değerleri değiştirmeden uygulayabilir veya ortamınızı yansıtacak şekilde değiştirebilirsiniz. 

### <a name="a-nameexampleaexample-values"></a><a name="example"></a>Örnek değerler
* **Ad: VNet1**
* **Adres alanı: 192.168.0.0/16** ve **10.254.0.0/16**<br>Bu örnekte, bu yapılandırmanın birden çok adres alanıyla çalışacağını göstermek için birden fazla adres alanı kullanıyoruz. Ancak, bu yapılandırma için birden çok adres alanı gerekli değildir.
* **Alt ağ adı: FrontEnd**
  * **Alt ağ adres aralığı: 192.168.1.0/24**
* **Alt ağ adı: BackEnd**
  * **Alt ağ adres aralığı: 10.254.1.0/24**
* **Alt ağ adı: GatewaySubnet**<br>VPN ağ geçidinin çalışması için Alt Ağ adı olarak *GatewaySubnet*'in kullanılması zorunludur.
  * **Alt ağ adres aralığı: 192.168.200.0/24** 
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
* Azure Resource Manager PowerShell cmdlet'lerinin en son sürümünü yükleyin. PowerShell cmdlet'lerini yükleme hakkında daha fazla bilgi için bkz. [Azure PowerShell'i yükleme ve yapılandırma](/powershell/azureps-cmdlets-docs). Bu yapılandırma için PowerShell ile çalışırken yönetici olarak işlem yaptığınızdan emin olun. 

## <a name="a-namedeclareapart-1---log-in-and-set-variables"></a><a name="declare"></a>1. Bölüm - Oturum açma ve değişkenleri ayarlama
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
        $GWName = "VNet1GW"
        $GWIPName = "VNet1GWPIP"
        $GWIPconfName = "gwipconf"

## <a name="a-nameconfigurevnetapart-2---configure-a-vnet"></a><a name="ConfigureVNet"></a>2. Kısım - Sanal ağ yapılandırma
1. Bir kaynak grubu oluşturun.
   
        New-AzureRmResourceGroup -Name $RG -Location $Location
2. Sanal ağ için alt ağ yapılandırmalarını oluşturup *FrontEnd*, *BackEnd* ve *GatewaySubnet* olarak adlandırın. Bu ön ekler bildirdiğiniz VNet adres alanının parçası olmalıdır.
   
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

## <a name="a-namecertificatesapart-3---certificates"></a><a name="Certificates"></a>3. Kısım - Sertifikalar
Noktadan Siteye VPN’lerde VPN istemcilerinin kimlik doğrulamasını yapmak için Azure tarafından sertifikalar kullanılır. Genel sertifika verilerini (özel anahtarı değil), kurumsal bir sertifika çözümü tarafından oluşturulan veya otomatik olarak imzalanan bir kök sertifikadan Base-64 ile kodlanmış X.509 .cer dosyası olarak dışarı aktarırsınız. Ardından kök sertifikanın genel sertifika verilerini Azure'a aktarırsınız. Ek olarak, istemciler için kök sertifikadan bir istemci sertifikası oluşturmanız gerekir. P2S bağlantısı kullanarak sanal ağa bağlanmak isteyen her istemcide, kök sertifikadan oluşturulmuş bir istemci sertifikası yüklü olmalıdır.

### <a name="a-namecerastep-1---obtain-the-cer-file-for-the-root-certificate"></a><a name="cer"></a>1. Adım: Kök sertifikaya ilişkin .cer dosyasını alma
Kullanmak istediğiniz kök sertifikanın genel sertifika verilerini almanız gerekir.

* Kurumsal bir sertifika sistemi kullanıyorsanız kullanmak istediğiniz kök sertifikaya ilişkin .cer dosyasını alın. 
* Kurumsal bir sertifika çözümü kullanmıyorsanız, otomatik olarak imzalanan bir sertifika oluşturmanız gerekir. P2S kullanımına yönelik otomatik olarak imzalanan bir sertifika oluşturmak için önerilen yöntem makecert’tir. Otomatik olarak imzalanan sertifikalar oluşturmak için PowerShell kullanılması mümkün olsa da, PowerShell kullanılarak oluşturulan sertifika P2S bağlantıları için gereken alanları içermez. Windows 10’a yönelik adımlar için [Working with self-signed root certificates for Point-to-Site configurations](vpn-gateway-certificates-point-to-site.md) (Noktadan Siteye yapılandırmaları için otomatik olarak imzalanan kök sertifikalar ile çalışma) makalesine başvurabilirsiniz.

1. Sertifikadan .cer dosyası almak için **certmgr.msc**'yi açın ve kök sertifikayı bulun. Otomatik olarak imzalanmış kök sertifikaya sağ tıklayın, **Tüm görevler**'e ve ardından **Dışarı aktar**'a tıklayın. **Sertifika Dışarı Aktarma Sihirbazı** açılır.
2. Sihirbazda **İleri**'ye tıklayın, **Hayır, özel anahtarı dışarı aktarma**'yı seçin ve **İleri**'ye tıklayın.
3. **Dışarı Aktarma Dosyası Biçimi** sayfasında **Base-64 ile kodlanmış X.509 (.CER)** seçeneğini belirleyin. Ardından **İleri**'ye tıklayın. 
4. **Dışarı Aktarılan Dosya** sayfasında **Gözat**'a tıklayarak sertifika için dışarı aktarma konumunu seçin. **Dosya adı** alanına, sertifika dosyası için bir ad girin. Ardından **İleri**'ye tıklayın.
5. Sertifikayı dışarı aktarmak için **Son**'a tıklayın.

### <a name="a-namegenerateastep-2---generate-the-client-certificate"></a><a name="generate"></a>2. Adım - İstemci sertifikasını oluşturma
Bir sonraki adımda istemci sertifikalarını oluşturacaksınız. Bağlanacak her istemci için benzersiz bir sertifika oluşturabileceğiniz gibi, birden çok istemcide aynı sertifikayı da kullanabilirsiniz. Benzersiz istemci sertifikaları oluşturmanın avantajı, gerektiğinde tek bir sertifikayı iptal edebiliyor olmanızdır. Herkesin aynı istemci sertifikasını kullandığı bir durumda bir istemcinin sertifikasını iptal etmeniz gerektiğinde, kimlik doğrulaması için söz konusu sertifikayı kullanan tüm istemciler için yeni sertifikalar oluşturmanız ve yüklemeniz gerekir. Bu alıştırmada daha sonra, istemci sertifikalarının tüm istemci bilgisayarlara yüklendiğini göreceğiz.

* Kurumsal bir sertifika çözümü kullanıyorsanız NetBIOS "ETKİALANI\kullanıcıadı" biçimini kullanmak yerine, yaygın olarak kullanılan 'name@yourdomain.com', ad değer biçimiyle bir istemci sertifikası oluşturun. 
* Otomatik olarak imzalanan sertifika kullanıyorsanız istemci sertifikası oluşturmak için bkz. [Noktadan Siteye yapılandırmaları için otomatik olarak imzalanan kök sertifikalar ile çalışma](vpn-gateway-certificates-point-to-site.md).

### <a name="a-nameexportclientcertastep-3---export-the-client-certificate"></a><a name="exportclientcert"></a>3. Adım - İstemci sertifikasını dışarı aktarma
Kimlik doğrulaması için istemci sertifikası gereklidir. İstemci sertifikasını oluşturduktan sonra dışarı aktarın. Dışarı aktardığınız istemci sertifikası, daha sonra tüm istemci bilgisayarlara yüklenecek.

1. İstemci sertifikasını dışarı aktarmak için *certmgr.msc* kullanabilirsiniz. Dışarı aktarmak istediğiniz istemci sertifikasına sağ tıklayın, **tüm görevler**’e ve ardından **dışarı aktar**’a tıklayın.
2. Özel anahtara sahip istemci sertifikasını dışarı aktarın. Bu bir *.pfx* dosyasıdır. Bu sertifika için ayarladığınız parolayı (anahtar) kaydettiğinizden ya da unutmayacağınızdan emin olun.

### <a name="a-nameuploadastep-4---upload-the-root-certificate-cer-file"></a><a name="upload"></a>4. Adım - Kök sertifika .cer dosyasını yükleme
Sertifika adınızın değişkenini tanımlamak için değeri kendi değerinizle değiştirin:

        $P2SRootCertName = "Mycertificatename.cer"

Kök sertifikanın genel sertifika verilerini Azure'a ekleyin. 20 adede kadar kök sertifikasının dosyasını yükleyebilirsiniz. Kök sertifikanın özel anahtarını Azure'a yüklemezsiniz. .cer dosyası yüklendikten sonra Azure, sanal ağa bağlanan istemcilerin kimliğini doğrulamak için bu anahtarı kullanır. 

Dosya yolunu kendi dosya yolunuzla değiştirin ve ardından cmdlet'leri çalıştırın.

        $filePathForCert = "C:\cert\Mycertificatename.cer"
        $cert = new-object System.Security.Cryptography.X509Certificates.X509Certificate2($filePathForCert)
        $CertBase64 = [system.convert]::ToBase64String($cert.RawData)
        $p2srootcert = New-AzureRmVpnClientRootCertificate -Name $P2SRootCertName -PublicCertData $CertBase64

## <a name="a-namecreategatewayapart-4---create-the-vpn-gateway"></a><a name="creategateway"></a>4. Kısım - VPN ağ geçidini oluşturma
VNet'iniz için sanal ağ geçidini yapılandırın ve oluşturun. *-GatewayType* değeri **Vpn** ve *-VpnType* değeri **RouteBased** olmalıdır. Bu işlemin tamamlanması 45 dakika sürebilir.

        New-AzureRmVirtualNetworkGateway -Name $GWName -ResourceGroupName $RG `
        -Location $Location -IpConfigurations $ipconf -GatewayType Vpn `
        -VpnType RouteBased -EnableBgp $false -GatewaySku Standard `
        -VpnClientAddressPool $VPNClientAddressPool -VpnClientRootCertificates $p2srootcert

## <a name="a-nameclientconfigapart-5---download-the-vpn-client-configuration-package"></a><a name="clientconfig"></a>5. Kısım - VPN istemci yapılandırma paketini indirme
P2S kullanarak Azure’a bağlanan istemcilerde hem bir istemci sertifikası hem de bir VPN istemcisi yapılandırma paketi yüklü olmalıdır. Windows istemcileri için VPN istemcisi yapılandırma paketleri kullanılabilir. VPN istemci paketi Windows’ta yerleşik olan VPN istemci yazılımını yapılandırmaya yönelik bilgiler içerir ve bağlanmak istediğiniz VPN’e özeldir. Paket, ek yazılım yüklemez. Daha fazla bilgi edinmek için bkz. [VPN Gateway ile ilgili SSS](vpn-gateway-vpn-faq.md#point-to-site-connections).

1. Ağ geçidi oluşturulduktan sonra istemci yapılandırma paketini indirebilirsiniz. Bu örnekte 64 bit istemcilere yönelik paket indirildi. 32 bit istemcisini yüklemek istiyorsanız 'Amd64' öğesini 'x86' olarak değiştirin. VPN istemcisini, Azure portalını kullanarak da indirebilirsiniz.
   
        Get-AzureRmVpnClientPackage -ResourceGroupName $RG `
        -VirtualNetworkGatewayName $GWName -ProcessorArchitecture Amd64
2. PowerShell cmdlet'i bir URL bağlantısı döndürür. Bu örnek döndürülen URL'nin nasıl göründüğüne ilişkin bir örnektir:
   
        "https://mdsbrketwprodsn1prod.blob.core.windows.net/cmakexe/4a431aa7-b5c2-45d9-97a0-859940069d3f/amd64/4a431aa7-b5c2-45d9-97a0-859940069d3f.exe?sv=2014-02-14&sr=b&sig=jSNCNQ9aUKkCiEokdo%2BqvfjAfyhSXGnRG0vYAv4efg0%3D&st=2016-01-08T07%3A10%3A08Z&se=2016-01-08T08%3A10%3A08Z&sp=r&fileExtension=.exe"
3. Paketi indirmek için döndürülen bağlantıyı kopyalayıp bir web tarayıcısına yapıştırın. Ardından paketi istemci bilgisayara yükleyin. SmartScreen penceresi açılırsa paketi yüklemek için **Daha fazla bilgi**'ye ve ardından **Yine de çalıştır**'a tıklayın.
4. İstemci bilgisayarda **Ağ Ayarları**’na gidin ve **VPN** öğesine tıklayın. Bağlantının listelendiğini görürsünüz. Bu listede bağlantı kurulacak sanal ağın adı gösterilir ve liste aşağıdaki örneğe benzer şekilde görünür: 
   
    ![VPN istemcisi](./media/vpn-gateway-howto-point-to-site-rm-ps/vpn.png)

## <a name="a-nameclientcertificateapart-6---install-the-client-certificate"></a><a name="clientcertificate"></a>6. Kısım - İstemci sertifikasını yükleme
Her istemci bilgisayarda kimlik doğrulaması için bir istemci sertifikası olması gerekir. İstemci sertifikasını yüklerken, istemci sertifikası dışarı aktarılırken oluşturulan parola gerekir.

1. .pfx dosyasını istemci bilgisayara kopyalayın.
2. .pfx dosyasına çift tıklayarak yükleme işlemini gerçekleştirin. Yükleme konumunu değiştirmeyin.

## <a name="a-nameconnectapart-7---connect-to-azure"></a><a name="connect"></a>7. Kısım - Azure'a bağlanma
1. İstemci bilgisayarda VNet'inize bağlanmak için VPN bağlantılarında gezinin ve oluşturduğunuz VPN bağlantısını bulun. Bu VPN bağlantısı sanal ağınızla aynı ada sahiptir. **Bağlan**'a tıklayın. Sertifika kullanımına ilişkin bir açılır ileti görüntülenebilir. Böyle bir durumla karşılaşırsanız yükseltilmiş ayrıcalıkları kullanmak için **Devam**'a tıklayın. 
2. **Bağlantı** durum sayfasında **Bağlan**'a tıklayarak bağlantıyı başlatın. Bir **Sertifika Seç** ekranı çıkarsa, gösterilen istemci sertifikasının bağlanmak için kullanmak istediğiniz sertifika olduğunu doğrulayın. Başka bir sertifika gösteriliyorsa, açılan liste okunu kullanarak doğru sertifikayı seçin ve **Tamam**’a tıklayın.
   
    ![Azure’a VPN istemcisi bağlama](./media/vpn-gateway-howto-point-to-site-rm-ps/clientconnect.png)
3. Artık bağlantı kurabilirsiniz.
   
    ![Bağlantı kuruldu](./media/vpn-gateway-howto-point-to-site-rm-ps/connected.png)

## <a name="a-nameverifyapart-8---verify-your-connection"></a><a name="verify"></a>8. Kısım - Bağlantınızı doğrulama
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

## <a name="a-nameaddremovecertato-add-or-remove-a-trusted-root-certificate"></a><a name="addremovecert"></a>Güvenilen kök sertifika ekleme veya kaldırma
Noktadan Siteye VPN’lerde VPN istemcilerinin kimlik doğrulamasını yapmak için sertifikalar kullanılır. Aşağıdaki adımlarda kök sertifika ekleme ve kaldırma işlemleri ayrıntılı olarak açıklanmıştır. Azure'a Base64 ile kodlanmış bir X.509 (*.cer) dosyası eklediğinizde, Azure'a dosyanın temsil ettiği kök sertifikaya güvenebileceğini söylemiş olursunuz. 

PowerShell kullanarak veya Azure portalında güvenilir kök sertifikaları ekleyebilir veya kaldırabilirsiniz. Bu işlemi Azure portalı ile yapmak istiyorsanız **sanal ağ geçidi > ayarlar > Noktadan siteye yapılandırma > Kök sertifikalar** konumuna gidin. Aşağıdaki adımlar bu görevleri PowerShell kullanarak gerçekleştirmeyi açıklamaktadır. 

### <a name="add-a-trusted-root-certificate"></a>Güvenilen kök sertifika ekleme
Azure'a en fazla 20 güvenilen kök sertifika .cer dosyası ekleyebilirsiniz. Kök sertifika eklemek için aşağıdaki adımları uygulayın.

1. Azure'a ekleyeceğiniz yeni kök sertifikayı oluşturup hazırlayın. Ortak anahtarı Base-64 kodlanmış X.509 (.CER) olarak dışarı aktarın ve bir metin düzenleyicisi ile açın. Ardından yalnızca aşağıda gösterilen bölümü kopyalayın. 
   
    Aşağıdaki örnekte gösterildiği gibi değerleri kopyalayın:
   
    ![sertifika](./media/vpn-gateway-howto-point-to-site-rm-ps/copycert.png)
2. Sertifika adını ve anahtar bilgilerini bir değişken olarak belirtin. Bilgileri aşağıdaki örnekte gösterildiği gibi kendi bilgilerinizle değiştirin:
   
        $P2SRootCertName2 = "ARMP2SRootCert2.cer"
        $MyP2SCertPubKeyBase64_2 = "MIIC/zCCAeugAwIBAgIQKazxzFjMkp9JRiX+tkTfSzAJBgUrDgMCHQUAMBgxFjAUBgNVBAMTDU15UDJTUm9vdENlcnQwHhcNMTUxMjE5MDI1MTIxWhcNMzkxMjMxMjM1OTU5WjAYMRYwFAYDVQQDEw1NeVAyU1Jvb3RDZXJ0MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAyjIXoWy8xE/GF1OSIvUaA0bxBjZ1PJfcXkMWsHPzvhWc2esOKrVQtgFgDz4ggAnOUFEkFaszjiHdnXv3mjzE2SpmAVIZPf2/yPWqkoHwkmrp6BpOvNVOpKxaGPOuK8+dql1xcL0eCkt69g4lxy0FGRFkBcSIgVTViS9wjuuS7LPo5+OXgyFkAY3pSDiMzQCkRGNFgw5WGMHRDAiruDQF1ciLNojAQCsDdLnI3pDYsvRW73HZEhmOqRRnJQe6VekvBYKLvnKaxUTKhFIYwuymHBB96nMFdRUKCZIiWRIy8Hc8+sQEsAML2EItAjQv4+fqgYiFdSWqnQCPf/7IZbotgQIDAQABo00wSzBJBgNVHQEEQjBAgBAkuVrWvFsCJAdK5pb/eoCNoRowGDEWMBQGA1UEAxMNTXlQMlNSb290Q2VydIIQKazxzFjMkp9JRiX+tkTfSzAJBgUrDgMCHQUAA4IBAQA223veAZEIar9N12ubNH2+HwZASNzDVNqspkPKD97TXfKHlPlIcS43TaYkTz38eVrwI6E0yDk4jAuPaKnPuPYFRj9w540SvY6PdOUwDoEqpIcAVp+b4VYwxPL6oyEQ8wnOYuoAK1hhh20lCbo8h9mMy9ofU+RP6HJ7lTqupLfXdID/XevI8tW6Dm+C/wCeV3EmIlO9KUoblD/e24zlo3YzOtbyXwTIh34T0fO/zQvUuBqZMcIPfM1cDvqcqiEFLWvWKoAnxbzckye2uk1gHO52d8AVL3mGiX8wBJkjc/pMdxrEvvCzJkltBmqxTM6XjDJALuVh16qFlqgTWCIcb7ju"
3. Yeni kök sertifikayı ekleyin. Tek seferde yalnızca bir sertifika ekleyebilirsiniz.
   
        Add-AzureRmVpnClientRootCertificate -VpnClientRootCertificateName $P2SRootCertName2 -VirtualNetworkGatewayname "VNet1GW" -ResourceGroupName "TestRG" -PublicCertData $MyP2SCertPubKeyBase64_2
4. Aşağıdaki cmdlet'i kullanarak yeni sertifikanın düzgün şekilde eklendiğini doğrulayabilirsiniz.
   
        Get-AzureRmVpnClientRootCertificate -ResourceGroupName "TestRG" `
        -VirtualNetworkGatewayName "VNet1GW"

### <a name="to-remove-a-trusted-root-certificate"></a>Güvenilen kök sertifikayı kaldırmak için
Azure'dan güvenilen kök sertifikayı kaldırabilirsiniz. Güvenilen kök sertifikayı kaldırdığınızda, kök sertifikadan oluşturulan istemci sertifikaları artık Noktadan Siteye bağlantı aracılığıyla Azure'a bağlanamaz. İstemciler, bağlanabilmek için Azure'da güvenilen bir sertifikadan oluşturan yeni bir istemci sertifikası yüklemelidir.

1. Güvenilen bir kök sertifikayı kaldırmak için aşağıdaki örneği değiştirin:
   
    Değişkenleri bildirin.
   
        $P2SRootCertName2 = "ARMP2SRootCert2.cer"
        $MyP2SCertPubKeyBase64_2 = "MIIC/zCCAeugAwIBAgIQKazxzFjMkp9JRiX+tkTfSzAJBgUrDgMCHQUAMBgxFjAUBgNVBAMTDU15UDJTUm9vdENlcnQwHhcNMTUxMjE5MDI1MTIxWhcNMzkxMjMxMjM1OTU5WjAYMRYwFAYDVQQDEw1NeVAyU1Jvb3RDZXJ0MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAyjIXoWy8xE/GF1OSIvUaA0bxBjZ1PJfcXkMWsHPzvhWc2esOKrVQtgFgDz4ggAnOUFEkFaszjiHdnXv3mjzE2SpmAVIZPf2/yPWqkoHwkmrp6BpOvNVOpKxaGPOuK8+dql1xcL0eCkt69g4lxy0FGRFkBcSIgVTViS9wjuuS7LPo5+OXgyFkAY3pSDiMzQCkRGNFgw5WGMHRDAiruDQF1ciLNojAQCsDdLnI3pDYsvRW73HZEhmOqRRnJQe6VekvBYKLvnKaxUTKhFIYwuymHBB96nMFdRUKCZIiWRIy8Hc8+sQEsAML2EItAjQv4+fqgYiFdSWqnQCPf/7IZbotgQIDAQABo00wSzBJBgNVHQEEQjBAgBAkuVrWvFsCJAdK5pb/eoCNoRowGDEWMBQGA1UEAxMNTXlQMlNSb290Q2VydIIQKazxzFjMkp9JRiX+tkTfSzAJBgUrDgMCHQUAA4IBAQA223veAZEIar9N12ubNH2+HwZASNzDVNqspkPKD97TXfKHlPlIcS43TaYkTz38eVrwI6E0yDk4jAuPaKnPuPYFRj9w540SvY6PdOUwDoEqpIcAVp+b4VYwxPL6oyEQ8wnOYuoAK1hhh20lCbo8h9mMy9ofU+RP6HJ7lTqupLfXdID/XevI8tW6Dm+C/wCeV3EmIlO9KUoblD/e24zlo3YzOtbyXwTIh34T0fO/zQvUuBqZMcIPfM1cDvqcqiEFLWvWKoAnxbzckye2uk1gHO52d8AVL3mGiX8wBJkjc/pMdxrEvvCzJkltBmqxTM6XjDJALuVh16qFlqgTWCIcb7ju"
2. Sertifikayı kaldırın.
   
        Remove-AzureRmVpnClientRootCertificate -VpnClientRootCertificateName $P2SRootCertName2 -VirtualNetworkGatewayName $GWName -ResourceGroupName $RG -PublicCertData $MyP2SCertPubKeyBase64_2
3. Sertifikanın başarıyla kaldırıldığını doğrulamak için aşağıdaki cmdlet'i kullanın. 
   
        Get-AzureRmVpnClientRootCertificate -ResourceGroupName "TestRG" `
        -VirtualNetworkGatewayName "VNet1GW"

## <a name="a-namerevokeato-manage-the-list-of-revoked-client-certificates"></a><a name="revoke"></a>İptal edilen istemci sertifikalarının listesini yönetmek için
İstemci sertifikalarını iptal edebilirsiniz. Sertifika iptal listesi sayesinde, ayrı istemci sertifikalarına göre Noktadan Siteye bağlantıyı seçmeli olarak reddedebilirsiniz. Azure'dan kök sertifika .cer dosyasını kaldırırsanız iptal edilen kök sertifika tarafından oluşturulan/imzalanan tüm istemci sertifikaları reddedilir. Kök sertifika yerine belirli bir istemci sertifikasını iptal etmek istiyorsanız bu işlemi gerçekleştirebilirsiniz. Bu sayede, kök sertifika tarafından oluşturulan diğer sertifikalar geçerli olmaya devam eder.

Genellikle ekip ve kuruluş düzeylerinde erişimi yönetmek için kök sertifika kullanılırken ayrı kullanıcılar üzerinde ayrıntılı erişim denetimi için iptal edilen istemci sertifikaları kullanılır.

### <a name="revoke-a-client-certificate"></a>İstemci sertifikasını iptal etme
1. İptal edilecek istemci sertifikasının parmak izini alın.
   
        $RevokedClientCert1 = "ClientCert1"
        $RevokedThumbprint1 = "?ef2af033d0686820f5a3c74804d167b88b69982f"
2. Parmak izini, iptal edilen parmak izi listesine ekleyin.
   
        Add-AzureRmVpnClientRevokedCertificate -VpnClientRevokedCertificateName $RevokedClientCert1 `
        -VirtualNetworkGatewayName $GWName -ResourceGroupName $RG -Thumbprint $RevokedThumbprint1
3. Parmak izinin, sertifika iptal listesine eklendiğini doğrulayın. Tek seferde yalnızca bir parmak izi eklemeniz gerekir.
   
        Get-AzureRmVpnClientRevokedCertificate -VirtualNetworkGatewayName $GWName -ResourceGroupName $RG

### <a name="reinstate-a-client-certificate"></a>İstemci sertifikasını yeniden devreye sokma
Parmak izini, iptal edilen istemci sertifikaları listesinden kaldırarak bir istemci sertifikasını yeniden devreye sokabilirsiniz.

1. Parmak izini, iptal edilen istemci sertifikası parmak izi listesinden kaldırın.
   
       Remove-AzureRmVpnClientRevokedCertificate -VpnClientRevokedCertificateName $RevokedClientCert1 `
       -VirtualNetworkGatewayName $GWName -ResourceGroupName $RG -Thumbprint $RevokedThumbprint1
2. Parmak izinin, iptal edilen parmak izi listesinden kaldırılıp kaldırılmadığını kontrol edin.
   
        Get-AzureRmVpnClientRevokedCertificate -VirtualNetworkGatewayName $GWName -ResourceGroupName $RG

## <a name="next-steps"></a>Sonraki adımlar
Bağlantınız tamamlandıktan sonra sanal ağlarınıza sanal makineler ekleyebilirsiniz. Daha fazla bilgi için bkz. [Sanal Makineler](https://docs.microsoft.com/azure/#pivot=services&panel=Compute).




<!--HONumber=Jan17_HO4-->


