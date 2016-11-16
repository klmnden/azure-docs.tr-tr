---
title: "Resource Manager dağıtım modelini ve Azure portalını kullanarak sanal ağa yönelik bir Noktadan Konuma VPN ağ geçidi bağlantısı yapılandırma | Microsoft Belgeleri"
description: "Resource Manager ve Azure portalı ile Noktadan Siteye bir VPN Gateway bağlantısı oluşturarak Azure Sanal Ağınıza güvenli bir şekilde bağlanın."
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: carmonm
editor: 
tags: azure-resource-manager
ms.assetid: a15ad327-e236-461f-a18e-6dbedbf74943
ms.service: vpn-gateway
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/17/2016
ms.author: cherylmc
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: aeebed0c733b9fbac964cdc532ff9d364683609b


---
# <a name="configure-a-pointtosite-connection-to-a-vnet-using-the-azure-portal"></a>Azure Portalı’nı kullanarak bir sanal ağa Noktadan Siteye bir bağlantı yapılandırma
> [!div class="op_single_selector"]
> * [Resource Manager - Azure Portal](vpn-gateway-howto-point-to-site-resource-manager-portal.md)
> * [Resource Manager - PowerShell](vpn-gateway-howto-point-to-site-rm-ps.md)
> * [Klasik - Azure Portal](vpn-gateway-howto-point-to-site-classic-azure-portal.md)
> 
> 

Noktadan Siteye (P2S) yapılandırması, ayrı bir istemci bilgisayardan bir sanal ağa yönelik güvenli bağlantı oluşturmanıza olanak sağlar. Sanal ağınıza uzak bir konumdan (örneğin, evden veya bir konferanstan) bağlanmak istediğinizde ya da sanal bir ağa bağlanması gereken yalnızca birkaç istemciniz bulunduğunda P2S bağlantısı kullanışlıdır. 

Noktadan Siteye bağlantıların çalışması için bir VPN cihazına veya genel kullanıma yönelik bir IP adresine gerek yoktur. VPN bağlantısı, bağlantının istemci bilgisayardan başlatılmasıyla oluşturulur. Noktadan Siteye bağlantılar hakkında daha fazla bilgi edinmek için [VPN Gateway ile ilgili SSS](vpn-gateway-vpn-faq.md#point-to-site-connections) ve [Planlama ve Tasarım](vpn-gateway-plan-design.md) başlıklı makalelere bakın.

Bu makalede Azure portalı kullanılarak Resource Manager dağıtım modelinde Noktadan Siteye bağlantı ile sanal ağ oluşturma işlemi adım adım açıklanmaktadır.

### <a name="deployment-models-and-methods-for-p2s-connections"></a>P2S bağlantıları için dağıtım modelleri ve yöntemleri
[!INCLUDE [deployment models](../../includes/vpn-gateway-deployment-models-include.md)]

Aşağıdaki tabloda, P2S yapılandırmaları için iki dağıtım modeli ve kullanılabilir dağıtım yöntemleri gösterilmektedir. Yapılandırma adımlarını içeren bir makale olduğunda, bu tablodan makaleye yönelik doğrudan bağlantı oluştururuz.

[!INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-table-point-to-site-include.md)]

## <a name="basic-workflow"></a>Temel iş akışı
![Noktadan Siteye diyagramı](./media/vpn-gateway-howto-point-to-site-rm-ps/p2srm.png "point-to-site")

### <a name="a-nameexampleaexample-values"></a><a name="example"></a>Örnek değerler
* **Ad: VNet1**
* **Adres alanı: 192.168.0.0/16**<br>Bu örnekte yalnızca bir adres alanı kullanılmaktadır. Sanal ağınıza ait birden fazla adres alanı olabilir.
* **Alt ağ adı: FrontEnd**
* **Alt ağ adres aralığı: 192.168.1.0/24**
* **Abonelik:** Birden fazla aboneliğiniz varsa doğru aboneliği kullandığınızdan emin olun.
* **Kaynak Grubu: TestRG**
* **Konum: Doğu ABD**
* **Ağ Geçidi Alt Ağı: 192.168.200.0/24**
* **Sanal ağ geçidi adı: VNet1GW**
* **Ağ geçidi türü: VPN**
* **VPN türü: Rota tabanlı**
* **Genel IP adresi: VNet1GWpip**
* **Bağlantı türü: Noktadan siteye**
* **İstemci adres havuzu: 172.16.201.0/24**<br>Sanal ağa bu Noktadan Siteye bağlantıyı kullanarak bağlanan VPN istemcileri, istemci adres havuzdan bir IP adresi alır.

## <a name="before-beginning"></a>Başlamadan önce
* Azure aboneliğiniz olduğunu doğrulayın. Henüz Azure aboneliğiniz yoksa [MSDN abonelik avantajlarınızı](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) etkinleştirebilir veya [ücretsiz bir hesap](https://azure.microsoft.com/pricing/free-trial/) için kaydolabilirsiniz.

## <a name="a-namecreatevnetapart-1-create-a-virtual-network"></a><a name="createvnet"></a>1. Bölüm - Sanal ağ oluşturma
Bu yapılandırmayı bir alıştırma olarak oluşturuyorsanız [örnek değerlere](#example) başvurabilirsiniz.

[!INCLUDE [vpn-gateway-basic-vnet-rm-portal](../../includes/vpn-gateway-basic-vnet-rm-portal-include.md)]

### <a name="2-add-additional-address-space-and-subnets"></a>2. Ek adres alanı ve alt ağları ekleme
Ek adres alanı ve alt ağları VNet’iniz oluşturulduktan sonra ekleyebilirsiniz.

[!INCLUDE [vpn-gateway-additional-address-space](../../includes/vpn-gateway-additional-address-space-include.md)]

### <a name="3-create-a-gateway-subnet"></a>3. Ağ geçidi alt ağı oluşturma
Sanal ağınızı bir ağ geçidine bağlamadan önce, bağlamak istediğiniz sanal ağ için ağ geçidi alt ağını oluşturmanız gerekir. Mümkünse, gelecekteki ek yapılandırma gereksinimlerini karşılamaya yetecek sayıda IP adresi sağlamak için /28 veya /27 CIDR bloğu kullanılarak ağ geçidi alt ağı oluşturulması idealdir.

Bu bölümdeki ekran görüntüleri örnek amaçlıdır. Ağ Geçidi Alt Ağı adres aralığına yapılandırmanız için gerekli olan değerleri girdiğinizden emin olun.

#### <a name="to-create-a-gateway-subnet"></a>Bir ağ geçidi alt ağı oluşturmak için
[!INCLUDE [vpn-gateway-add-gwsubnet-rm-portal](../../includes/vpn-gateway-add-gwsubnet-rm-portal-include.md)]

### <a name="a-namednsa4-specify-a-dns-server-optional"></a><a name="dns"></a>4. DNS sunucusu belirtme (isteğe bağlı)
[!INCLUDE [vpn-gateway-add-dns-rm-portal](../../includes/vpn-gateway-add-dns-rm-portal-include.md)]

## <a name="a-namecreategwapart-2-create-a-virtual-network-gateway"></a><a name="creategw"></a>2. Bölüm - Sanal ağ geçidi oluşturma
Noktadan siteye bağlantılar için şu ayarların yapılması gerekir:

* Ağ geçidi türü: VPN
* VPN türü: Rota tabanlı

### <a name="to-create-a-virtual-network-gateway"></a>Bir sanal ağ geçidi oluşturmak için
[!INCLUDE [vpn-gateway-add-gw-rm-portal](../../includes/vpn-gateway-add-gw-rm-portal-include.md)]

## <a name="a-namegeneratecertapart-3-generate-certificates"></a><a name="generatecert"></a>3. Bölüm - Sertifika oluşturma
Noktadan Siteye VPN’lerde VPN istemcilerinin kimlik doğrulamasını yapmak için Azure tarafından sertifikalar kullanılır. Genel sertifika verilerini (özel anahtarı değil), kurumsal bir sertifika çözümü tarafından oluşturulan veya otomatik olarak imzalanan bir kök sertifikadan Base-64 ile kodlanmış X.509 .cer dosyası olarak dışarı aktarırsınız. Ardından kök sertifikanın genel sertifika verilerini Azure'a aktarırsınız. Ek olarak, istemciler için kök sertifikadan bir istemci sertifikası oluşturmanız gerekir. P2S bağlantısı kullanarak sanal ağa bağlanmak isteyen her istemcide, kök sertifikadan oluşturulmuş bir istemci sertifikası yüklü olmalıdır.

### <a name="a-namegetcera1-obtain-the-cer-file-for-the-root-certificate"></a><a name="getcer"></a>1. Kök sertifikaya ilişkin .cer dosyasını alma
Bir kuruluş çözümü kullanıyorsanız var olan sertifika zincirinizi kullanabilirsiniz. Kuruluş CA çözümü kullanmıyorsanız otomatik olarak imzalanan bir kök sertifika oluşturabilirsiniz. Otomatik olarak imzalanan sertifika oluşturmanın bir yolu makecert yöntemidir.

* Kurumsal bir sertifika sistemi kullanıyorsanız kullanmak istediğiniz kök sertifikaya ilişkin .cer dosyasını alın. 
* Kurumsal bir sertifika çözümü kullanmıyorsanız, otomatik olarak imzalanan bir sertifika oluşturmanız gerekir. Windows 10’a yönelik adımlar için [Working with self-signed root certificates for Point-to-Site configurations](vpn-gateway-certificates-point-to-site.md) (Noktadan Siteye yapılandırmaları için otomatik olarak imzalanan kök sertifikalar ile çalışma) makalesine başvurabilirsiniz.

1. Sertifikadan .cer dosyası almak için **certmgr.msc**'yi açın ve kök sertifikayı bulun. Otomatik olarak imzalanmış kök sertifikaya sağ tıklayın, **Tüm görevler**'e ve ardından **Dışarı aktar**'a tıklayın. **Sertifika Dışarı Aktarma Sihirbazı** açılır.
2. Sihirbazda **İleri**'ye tıklayın, **Hayır, özel anahtarı dışarı aktarma**'yı seçin ve **İleri**'ye tıklayın.
3. **Dışarı Aktarma Dosyası Biçimi** sayfasında **Base-64 ile kodlanmış X.509 (.CER)** seçeneğini belirleyin. Ardından **İleri**'ye tıklayın. 
4. **Dışarı Aktarılan Dosya** sayfasında **Gözat**'a tıklayarak sertifika için dışarı aktarma konumunu seçin. **Dosya adı** alanına, sertifika dosyası için bir ad girin. Ardından **İleri**'ye tıklayın.
5. Sertifikayı dışarı aktarmak için **Son**'a tıklayın.

### <a name="a-namegenerateclientcerta2-generate-a-client-certificate"></a><a name="generateclientcert"></a>2. İstemci sertifikası oluşturma
Bağlanacak her istemci için benzersiz bir sertifika oluşturabileceğiniz gibi, birden çok istemcide aynı sertifikayı da kullanabilirsiniz. Benzersiz istemci sertifikaları oluşturmanın avantajı, gerektiğinde tek bir sertifikayı iptal edebiliyor olmanızdır. Herkesin aynı istemci sertifikasını kullandığı bir durumda bir istemcinin sertifikasını iptal etmeniz gerektiğinde, kimlik doğrulaması için söz konusu sertifikayı kullanan tüm istemciler için yeni sertifikalar oluşturmanız ve yüklemeniz gerekir.

* Kurumsal bir sertifika çözümü kullanıyorsanız, 'etkialanıadı\kullanıcıadı' biçimini kullanmak yerine, yaygın olarak kullanılan 'name@yourdomain.com', ad değer biçimiyle bir istemci sertifikası oluşturun. 
* Otomatik olarak imzalanan sertifika kullanıyorsanız istemci sertifikası oluşturmak için bkz. [Noktadan Siteye yapılandırmaları için otomatik olarak imzalanan kök sertifikalar ile çalışma](vpn-gateway-certificates-point-to-site.md).

### <a name="a-nameexportclientcerta3-export-the-client-certificate"></a><a name="exportclientcert"></a>3. İstemci sertifikasını dışarı aktarma
Kimlik doğrulaması için istemci sertifikası gereklidir. İstemci sertifikasını oluşturduktan sonra dışarı aktarın. Dışarı aktardığınız istemci sertifikası, daha sonra tüm istemci bilgisayarlara yüklenecek.

1. İstemci sertifikasını dışarı aktarmak için *certmgr.msc* kullanabilirsiniz. Dışarı aktarmak istediğiniz istemci sertifikasına sağ tıklayın, **tüm görevler**’e ve ardından **dışarı aktar**’a tıklayın.
2. Özel anahtara sahip istemci sertifikasını dışarı aktarın. Bu bir *.pfx* dosyasıdır. Bu sertifika için ayarladığınız parolayı (anahtar) kaydettiğinizden ya da unutmayacağınızdan emin olun.

## <a name="a-nameaddresspoolapart-4-add-the-client-address-pool"></a><a name="addresspool"></a>4. Bölüm - İstemci adres havuzunu ekleme
1. Sanal ağ geçidini oluşturduktan sonra dikey penceresindeki **Ayarlar** bölümüne gidin. **Ayarlar** bölümünde **Noktadan siteye yapılandırma**’ya tıklayarak **Yapılandırma** dikey penceresini açın.
   
    ![noktadan siteye dikey penceresi](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/configuration.png "point to site blade")
2. **Adres havuzu**, bağlanan istemcilerin alacağı IP adreslerinin bulunduğu havuzdur. Adres havuzunu ekleyip **Kaydet**’e tıklayın.
   
    ![istemci adres havuzu](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/addresspool.png "client address pool")

## <a name="a-nameuploadfileapart-5-upload-the-root-certificate-cer-file"></a><a name="uploadfile"></a>5. Bölüm - Kök sertifika .cer dosyasını yükleme
Ağ geçidi oluşturulduktan sonra güvenilen bir kök sertifika için .cer dosyasını Azure’a yükleyebilirsiniz. 20 adede kadar kök sertifikasının dosyasını yükleyebilirsiniz. Kök sertifikanın özel anahtarını Azure'a yüklemezsiniz. .cer dosyası yüklendikten sonra Azure, sanal ağa bağlanan istemcilerin kimliğini doğrulamak için bu anahtarı kullanır.

1. **Noktadan siteye yapılandırma** dikey penceresini açın. .cer dosyalarını bu dikey pencerenin **Kök sertifika** bölümüne ekleyin.
   
    ![noktadan siteye dikey penceresi](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/rootcert.png "point to site blade")
2. Kök sertifikayı Base-64 ile kodlanmış X.509 (.cer) dosyası olarak dışarı aktardığınızdan emin olun. Sertifikayı metin düzenleyiciyle açabilmek için bu biçimde dışa aktarmanız gerekir.
3. Sertifikayı Not Defteri gibi bir metin düzenleyiciyle açın. Yalnızca aşağıdaki bölümü kopyalayın:
   
    ![sertifika verileri](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/copycert.png "certificate data")
4. Sertifika verilerini portalın **Genel Sertifika Verileri** bölümüne yapıştırın. Sertifikanın adını **Ad** alanına girin ve **Kaydet**’e tıklayın. En fazla 20 güvenilen kök sertifika ekleyebilirsiniz.
   
    ![sertifika yükleme](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/uploadcert.png "certificate upload")

## <a name="a-nameclientconfigapart-6-download-and-install-the-vpn-client-configuration-package"></a><a name="clientconfig"></a>6. Bölüm - VPN istemcisi yapılandırma paketini indirme ve yükleme
P2S kullanarak Azure’a bağlanan istemcilerde hem bir istemci sertifikası hem de bir VPN istemcisi yapılandırma paketi yüklü olmalıdır. Windows istemcileri için VPN istemcisi yapılandırma paketleri kullanılabilir. 

VPN istemci paketi, Windows'da yerleşik bulunan VPN istemci yazılımını yapılandırmaya yönelik bilgileri içerir. Bu yapılandırma, bağlanmak istediğiniz VPN’e özeldir. Paket, ek yazılım yüklemez. Daha fazla bilgi edinmek için bkz. [VPN Gateway ile ilgili SSS](vpn-gateway-vpn-faq.md#point-to-site-connections).

1. **Noktadan siteye yapılandırma** dikey penceresinde **VPN istemcisini indir**’e tıklayarak **VPN istemcisini indir** dikey penceresini açın.
   
    ![VPN istemcisini indirme](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/downloadclient.png "VPN client download")
2. İstemciniz için doğru paketi seçip **İndir**’e tıklayın. 64 bit istemciler için **AMD64**’ü seçin. 32 bit istemciler için **x86**’yı seçin.
3. Paketi istemci bilgisayara yükleyin. SmartScreen penceresi açılırsa, paketi yüklemek için **Daha fazla bilgi**’ye ve ardından **Yine de çalıştır**’a tıklayın.
4. İstemci bilgisayarda **Ağ Ayarları**’na gidin ve **VPN** öğesine tıklayın. Bağlantının listelendiğini görürsünüz. Bu listede bağlantı kurulacak sanal ağın adı gösterilir ve liste aşağıdaki örneğe benzer şekilde görünür: 
   
    ![VPN istemcisi](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/vpn.png "VPN client")

## <a name="a-nameinstallclientcertapart-7-install-the-client-certificate"></a><a name="installclientcert"></a>7. Bölüm - İstemci sertifikasını yükleme
Her istemci bilgisayarda kimlik doğrulaması için bir istemci sertifikası olması gerekir. İstemci sertifikasını yüklerken, istemci sertifikası dışarı aktarılırken oluşturulan parola gerekir.

1. .pfx dosyasını istemci bilgisayara kopyalayın.
2. .pfx dosyasına çift tıklayarak yükleme işlemini gerçekleştirin. Yükleme konumunu değiştirmeyin.

## <a name="a-nameconnectapart-8-connect-to-azure"></a><a name="connect"></a>8. Bölüm - Azure’a bağlanma
1. İstemci bilgisayarda VNet'inize bağlanmak için VPN bağlantılarında gezinin ve oluşturduğunuz VPN bağlantısını bulun. Bu VPN bağlantısı sanal ağınızla aynı ada sahiptir. **Bağlan**'a tıklayın. Sertifika kullanımına ilişkin bir açılır ileti görüntülenebilir. Böyle bir durumla karşılaşırsanız yükseltilmiş ayrıcalıkları kullanmak için **Devam**'a tıklayın. 
2. **Bağlantı** durum sayfasında **Bağlan**'a tıklayarak bağlantıyı başlatın. Bir **Sertifika Seç** ekranı çıkarsa, gösterilen istemci sertifikasının bağlanmak için kullanmak istediğiniz sertifika olduğunu doğrulayın. Başka bir sertifika gösteriliyorsa, açılan liste okunu kullanarak doğru sertifikayı seçin ve **Tamam**’a tıklayın.
   
    ![VPN istemcisi 2](./media/vpn-gateway-howto-point-to-site-rm-ps/clientconnect.png "VPN client connection")
3. Artık bağlantı kurabilirsiniz.
   
    ![VPN istemcisi 3](./media/vpn-gateway-howto-point-to-site-rm-ps/connected.png "VPN client connection 2")

## <a name="a-nameverifyapart-9-verify-your-connection"></a><a name="verify"></a>9. Bölüm - Bağlantınızı doğrulama
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

## <a name="a-nameaddato-add-or-remove-trusted-root-certificates"></a><a name="add"></a>Güvenilen kök sertifika ekleme veya kaldırma
Azure'dan güvenilen kök sertifikayı kaldırabilirsiniz. Güvenilen kök sertifikayı kaldırdığınızda, kök sertifikadan oluşturulan istemci sertifikaları artık Noktadan Siteye bağlantı aracılığıyla Azure'a bağlanamaz. İstemciler, bağlanabilmek için Azure'da güvenilen bir sertifikadan oluşturan yeni bir istemci sertifikası yüklemelidir.

İptal edilen istemci sertifikalarının listesini **Noktadan siteye yapılandırma** dikey penceresinde yönetebilirsiniz. Bu, [güvenilen kök sertifika yüklemek](#uploadfile) için kullandığınız dikey penceredir.

## <a name="a-namerevokeclientato-manage-the-list-of-revoked-client-certificates"></a><a name="revokeclient"></a>İptal edilen istemci sertifikalarının listesini yönetmek için
İstemci sertifikalarını iptal edebilirsiniz. Sertifika iptal listesi sayesinde, ayrı istemci sertifikalarına göre Noktadan Siteye bağlantıyı seçmeli olarak reddedebilirsiniz. Azure'dan kök sertifika .cer dosyasını kaldırırsanız iptal edilen kök sertifika tarafından oluşturulan/imzalanan tüm istemci sertifikaları reddedilir. Kök sertifika yerine belirli bir istemci sertifikasını iptal etmek istiyorsanız bu işlemi gerçekleştirebilirsiniz. Bu sayede, kök sertifika tarafından oluşturulan diğer sertifikalar geçerli olmaya devam eder. 

Genellikle ekip ve kuruluş düzeylerinde erişimi yönetmek için kök sertifika kullanılırken ayrı kullanıcılar üzerinde ayrıntılı erişim denetimi için iptal edilen istemci sertifikaları kullanılır.

İptal edilen istemci sertifikalarının listesini **Noktadan siteye yapılandırma** dikey penceresinde yönetebilirsiniz. Bu, [güvenilen kök sertifika yüklemek](#uploadfile) için kullandığınız dikey penceredir.

## <a name="next-steps"></a>Sonraki adımlar
Sanal ağınıza sanal makine ekleyebilirsiniz. Adımlar için bkz. [Sanal Makine Oluşturma](../virtual-machines/virtual-machines-windows-hero-tutorial.md).




<!--HONumber=Nov16_HO2-->


