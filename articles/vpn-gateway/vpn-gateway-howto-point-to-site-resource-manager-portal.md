---
title: "Noktadan Siteye bağlantısı kullanarak bir bilgisayarı Azure sanal ağına bağlama: Portal | Microsoft Docs"
description: "Resource Manager ve Azure portalı ile Noktadan Siteye bir VPN Gateway bağlantısı oluşturarak Azure Sanal Ağınıza güvenli bir şekilde bağlanın."
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: a15ad327-e236-461f-a18e-6dbedbf74943
ms.service: vpn-gateway
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/17/2017
ms.author: cherylmc
translationtype: Human Translation
ms.sourcegitcommit: cf72197aba2c6e6c7a51f96d1161cf1fbe88a0c5
ms.openlocfilehash: 61ef9523739ebd8bd105c6faccf42405c7c62567


---
# <a name="configure-a-point-to-site-connection-to-a-vnet-using-the-azure-portal"></a>Azure portalı kullanarak bir sanal ağa Noktadan Siteye bir bağlantı yapılandırma
> [!div class="op_single_selector"]
> * [Resource Manager - Azure Portal](vpn-gateway-howto-point-to-site-resource-manager-portal.md)
> * [Resource Manager - PowerShell](vpn-gateway-howto-point-to-site-rm-ps.md)
> * [Klasik - Azure Portal](vpn-gateway-howto-point-to-site-classic-azure-portal.md)
> 
> 

Noktadan Siteye (P2S) yapılandırması, ayrı bir istemci bilgisayardan bir sanal ağa yönelik güvenli bağlantı oluşturmanıza olanak sağlar. Sanal ağınıza uzak bir konumdan (örneğin, evden veya bir konferanstan) bağlanmak istediğinizde ya da sanal bir ağa bağlanması gereken yalnızca birkaç istemciniz bulunduğunda P2S bağlantısı kullanışlıdır. 

Noktadan Siteye bağlantıların çalışması için bir VPN cihazına veya genel kullanıma yönelik bir IP adresine gerek yoktur. VPN bağlantısı, bağlantının istemci bilgisayardan başlatılmasıyla oluşturulur. Noktadan Siteye bağlantılar hakkında daha fazla bilgi edinmek için bu makalenin sonunda yer alan [Noktadan Siteye hakkında SSS](#faq) bölümünü inceleyin.

Bu makalede, Azure portalını kullanarak Noktadan Siteye bağlantı içeren bir VNet oluşturma işlemi adım adım anlatılır. Bu adımlar Resource Manager dağıtım modeli için geçerlidir.

### <a name="deployment-models-and-methods-for-p2s-connections"></a>P2S bağlantıları için dağıtım modelleri ve yöntemleri
[!INCLUDE [deployment models](../../includes/vpn-gateway-deployment-models-include.md)]

Aşağıdaki tabloda, P2S yapılandırmaları için iki dağıtım modeli ve kullanılabilir dağıtım yöntemleri gösterilmektedir. Yapılandırma adımlarını içeren bir makale olduğunda, bu tablodan makaleye yönelik doğrudan bağlantı oluştururuz.

[!INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-table-point-to-site-include.md)]

## <a name="basic-workflow"></a>Temel iş akışı
![Noktadan Siteye diyagramı](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/point-to-site-connection-diagram.png)

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
* Azure aboneliğiniz olduğunu doğrulayın. Henüz Azure aboneliğiniz yoksa [MSDN abonelik avantajlarınızı](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details) etkinleştirebilir veya [ücretsiz bir hesap](https://azure.microsoft.com/pricing/free-trial) için kaydolabilirsiniz.

## <a name="a-namecreatevnetapart-1---create-a-virtual-network"></a><a name="createvnet"></a>1. Bölüm - Sanal ağ oluşturma
Bu yapılandırmayı bir alıştırma olarak oluşturuyorsanız [örnek değerlere](#example) başvurabilirsiniz.

[!INCLUDE [vpn-gateway-basic-vnet-rm-portal](../../includes/vpn-gateway-basic-vnet-rm-portal-include.md)]

## <a name="a-nameaddressapart-2---specify-address-space-and-subnets"></a><a name="address"></a>2. Kısım - Adres alanı ve alt ağ belirtme
Ek adres alanı ve alt ağları VNet’iniz oluşturulduktan sonra ekleyebilirsiniz.

[!INCLUDE [vpn-gateway-additional-address-space](../../includes/vpn-gateway-additional-address-space-include.md)]

## <a name="a-namegatewaysubnetapart-3---add-a-gateway-subnet"></a><a name="gatewaysubnet"></a>3. Kısım - Ağ geçidi alt ağı ekleme

Sanal ağınızı bir ağ geçidine bağlamadan önce, bağlamak istediğiniz sanal ağ için ağ geçidi alt ağını oluşturmanız gerekir. Ağ geçidi hizmetleri, ağ geçidi alt ağında belirtilen IP adreslerini kullanır. Mümkünse, gelecekte eklenecek yapılandırma gereksinimlerine yetecek sayıda IP adresi sağlamak için ağ geçidi alt ağını /28 veya /27 değerine sahip bir CIDR bloğu kullanarak oluşturun.

Bu bölümdeki ekran görüntüleri örnek amaçlıdır. Ağ Geçidi Alt Ağı adres aralığına yapılandırmanız için gerekli olan değerleri girdiğinizden emin olun.

###<a name="to-create-a-gateway-subnet"></a>Bir ağ geçidi alt ağı oluşturmak için

[!INCLUDE [vpn-gateway-add-gwsubnet-rm-portal](../../includes/vpn-gateway-add-gwsubnet-rm-portal-include.md)]

## <a name="a-namednsapart-4---specify-a-dns-server-optional"></a><a name="dns"></a>4. Kısım - DNS sunucusu belirtme (isteğe bağlı)
[!INCLUDE [vpn-gateway-add-dns-rm-portal](../../includes/vpn-gateway-add-dns-rm-portal-include.md)]

## <a name="a-namecreategwapart-5---create-a-virtual-network-gateway"></a><a name="creategw"></a>5. Kısım - Sanal ağ geçidi oluşturma
Noktadan siteye bağlantılar için şu ayarların yapılması gerekir:

* Ağ geçidi türü: VPN
* VPN türü: Rota tabanlı

### <a name="to-create-a-virtual-network-gateway"></a>Bir sanal ağ geçidi oluşturmak için
[!INCLUDE [vpn-gateway-add-gw-rm-portal](../../includes/vpn-gateway-add-gw-rm-portal-include.md)]

## <a name="a-namegeneratecertapart-6---generate-certificates"></a><a name="generatecert"></a>6. Kısım - Sertifika oluşturma
Noktadan Siteye VPN’lerde VPN istemcilerinin kimlik doğrulamasını yapmak için Azure tarafından sertifikalar kullanılır. Genel sertifika verilerini (özel anahtarı değil), kurumsal bir sertifika çözümü tarafından oluşturulan veya otomatik olarak imzalanan bir kök sertifikadan Base-64 ile kodlanmış X.509 .cer dosyası olarak dışarı aktarırsınız. Ardından kök sertifikanın genel sertifika verilerini Azure'a aktarırsınız. Ek olarak, istemciler için kök sertifikadan bir istemci sertifikası oluşturmanız gerekir. P2S bağlantısı kullanarak sanal ağa bağlanmak isteyen her istemcide, kök sertifikadan oluşturulmuş bir istemci sertifikası yüklü olmalıdır.

### <a name="a-namegetcerastep-1---obtain-the-cer-file-for-the-root-certificate"></a><a name="getcer"></a>1. Adım: Kök sertifikaya ilişkin .cer dosyasını alma

Kök sertifikası için .cer dosyasını edinmeniz gerekir. Kurumsal bir çözümden edindiğiniz bir kök sertifikasını kullanabilir ya da [makecert’i kullanarak otomatik olarak imzalanan bir kök sertifikası oluşturma](vpn-gateway-certificates-point-to-site.md) yöntemini tercih edebilirsiniz. Otomatik olarak imzalanan sertifikalar oluşturmak için PowerShell kullanılması mümkün olsa da, PowerShell kullanılarak oluşturulan sertifika P2S bağlantıları için gereken alanları içermez.

1. Sertifikadan .cer dosyası almak için **certmgr.msc**'yi açın ve kök sertifikayı bulun. Otomatik olarak imzalanmış kök sertifikaya sağ tıklayın, **Tüm görevler**'e ve ardından **Dışarı aktar**'a tıklayın. **Sertifika Dışarı Aktarma Sihirbazı** açılır.
2. Sihirbazda **İleri**'ye tıklayın, **Hayır, özel anahtarı dışarı aktarma**'yı seçin ve **İleri**'ye tıklayın.
3. **Dışarı Aktarma Dosyası Biçimi** sayfasında **Base-64 ile kodlanmış X.509 (.CER)** seçeneğini belirleyin. Ardından **İleri**'ye tıklayın. 
4. **Dışarı Aktarılan Dosya** sayfasında **Gözat**'a tıklayarak sertifika için dışarı aktarma konumunu seçin. **Dosya adı** alanına, sertifika dosyası için bir ad girin. Ardından **İleri**'ye tıklayın.
5. Sertifikayı dışarı aktarmak için **Son**'a tıklayın.

### <a name="a-namegenerateclientcertastep-2---generate-a-client-certificate"></a><a name="generateclientcert"></a>2. Adım - İstemci sertifikası oluşturma
Sanal ağa bağlanacak her istemci için benzersiz bir sertifika oluşturabilir veya birden çok istemcide aynı sertifikayı kullanabilirsiniz. Benzersiz istemci sertifikaları oluşturmanın avantajı, gerektiğinde tek bir sertifikayı iptal edebiliyor olmanızdır. Aksi takdirde, herkes aynı istemci sertifikasını kullanıyorsa ve bir istemci için sertifikayı iptal etmeniz gerekirse, kimlik doğrulaması için bu sertifikayı kullanan tüm istemciler için yeni sertifikalar oluşturup yüklemeniz gerekir.

####<a name="enterprise-certificate"></a>Kurumsal sertifika
- Kurumsal bir sertifika çözümü kullanıyorsanız, 'etkialanıadı\kullanıcıadı' biçimini kullanmak yerine, yaygın olarak kullanılan 'name@yourdomain.com', ad değer biçimiyle bir istemci sertifikası oluşturun.
- Verdiğiniz istemci sertifikasının, kullanım listesindeki ilk öğe olarak Akıllı Kart Oturumu, vb. yerine ‘İstemci Kimlik Doğrulaması’na sahip ‘Kullanıcı’ sertifikası şablonunu temel alarak hazırlandığından emin olun. İstemci sertifikasına sağ tıklayıp **Ayrıntılar > Gelişmiş Anahtar Kullanımı**’nı görüntüleyerek sertifikayı denetleyebilirsiniz.

####<a name="self-signed-certificate"></a>Otomatik olarak imzalanan sertifika 
Otomatik olarak imzalanan sertifika kullanıyorsanız istemci sertifikası oluşturmak için bkz. [Noktadan Siteye yapılandırmaları için otomatik olarak imzalanan kök sertifikalar ile çalışma](vpn-gateway-certificates-point-to-site.md).

### <a name="a-nameexportclientcertastep-3---export-the-client-certificate"></a><a name="exportclientcert"></a>3. Adım - İstemci sertifikasını dışarı aktarma
Kimlik doğrulaması için istemci sertifikası gereklidir. İstemci sertifikasını oluşturduktan sonra dışarı aktarın. Dışarı aktardığınız istemci sertifikası, daha sonra tüm istemci bilgisayarlara yüklenecek.

1. İstemci sertifikasını dışarı aktarmak için *certmgr.msc* kullanabilirsiniz. Dışarı aktarmak istediğiniz istemci sertifikasına sağ tıklayın, **tüm görevler**’e ve ardından **dışarı aktar**’a tıklayın.
2. Özel anahtara sahip istemci sertifikasını dışarı aktarın. Bu bir *.pfx* dosyasıdır. Bu sertifika için ayarladığınız parolayı (anahtar) kaydettiğinizden ya da unutmayacağınızdan emin olun.

## <a name="a-nameaddresspoolapart-7---add-the-client-address-pool"></a><a name="addresspool"></a>7. Kısım - İstemci adres havuzunu ekleme
1. Sanal ağ geçidini oluşturduktan sonra dikey penceresindeki **Ayarlar** bölümüne gidin. **Ayarlar** bölümünde **Noktadan siteye yapılandırma**’ya tıklayarak **Yapılandırma** dikey penceresini açın.
   
    ![Noktadan Siteye dikey penceresi](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/configuration.png)
2. **Adres havuzu**, bağlanan istemcilerin alacağı IP adreslerinin bulunduğu havuzdur. Adres havuzunu ekleyip **Kaydet**’e tıklayın.
   
    ![İstemci adres havuzu](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/ipaddresspool.png)

## <a name="a-nameuploadfileapart-8---upload-the-root-certificate-cer-file"></a><a name="uploadfile"></a>8. Kısım - Kök sertifika .cer dosyasını yükleme
Ağ geçidi oluşturulduktan sonra güvenilen bir kök sertifika için .cer dosyasını Azure’a yükleyebilirsiniz. 20 adede kadar kök sertifikasının dosyasını yükleyebilirsiniz. Kök sertifikanın özel anahtarını Azure'a yüklemezsiniz. .cer dosyası yüklendikten sonra Azure, sanal ağa bağlanan istemcilerin kimliğini doğrulamak için bu anahtarı kullanır.

1. Sertifikalar **Kök sertifikası** bölümünün **Noktadan siteye yapılandırma** dikey penceresine eklenir.  
2. Kök sertifikayı Base-64 ile kodlanmış X.509 (.cer) dosyası olarak dışarı aktardığınızdan emin olun. Sertifikayı metin düzenleyiciyle açabilmek için bu biçimde dışa aktarmanız gerekir.
3. Sertifikayı Not Defteri gibi bir metin düzenleyiciyle açın. Yalnızca aşağıdaki bölümü kesintisiz bir satır olarak kopyalayın:
   
    ![Sertifika verileri](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/copycert.png)

    > [!NOTE]
    > Sertifika verilerini kopyalarken, metni satır başları ya da satır besleme karakterleri içermeyen tek ve kesintisiz bir satır şeklinde kopyaladığınızdan emin olun. Satır başlarını ve satır besleme karakterlerini görmek için metin düzenleyicisindeki görünümünüzü 'Sembolü Göster/Tüm karakterleri göster' olarak değiştirmeniz gerekebilir.                                                                                                                                                                            
    >
    >

4. Sertifika verilerini **Ortak Sertifika Verileri** alanına yapıştırın. Sertifikayı **Adlandırın** ve **Kaydet**’e tıklayın. En fazla 20 güvenilen kök sertifika ekleyebilirsiniz.
   
    ![Sertifika yükleme](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/rootcertupload.png)

## <a name="a-nameclientconfigapart-9---download-and-install-the-vpn-client-configuration-package"></a><a name="clientconfig"></a>9. Kısım - VPN istemcisi yapılandırma paketini indirme ve yükleme
P2S kullanarak Azure’a bağlanan istemcilerde hem bir istemci sertifikası hem de bir VPN istemcisi yapılandırma paketi yüklü olmalıdır. Windows istemcileri için VPN istemcisi yapılandırma paketleri kullanılabilir. 

VPN istemci paketi, Windows'da yerleşik bulunan VPN istemci yazılımını yapılandırmaya yönelik bilgileri içerir. Bu yapılandırma, bağlanmak istediğiniz VPN’e özeldir. Paket, ek yazılım yüklemez.

1. **Noktadan siteye yapılandırma** dikey penceresinde **VPN istemcisini indir**’e tıklayarak **VPN istemcisini indir** dikey penceresini açın.
   
    ![VPN istemcisi indirme 1](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/downloadvpnclient1.png)
2. İstemciniz için doğru paketi seçip **İndir**’e tıklayın. 64 bit istemciler için **AMD64**’ü seçin. 32 bit istemciler için **x86**’yı seçin.

    ![VPN istemcisi indirme 2](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/client.png)
3. Paketi istemci bilgisayara yükleyin. Bir SmartScreen açılır penceresi görürseniz **Daha fazla bilgi**’ye ve ardından **Yine de çalıştır**’a tıklayarak paketi yükleyin.
4. İstemci bilgisayarda **Ağ Ayarları**’na gidin ve **VPN** öğesine tıklayın. Bağlantının listelendiğini görürsünüz. Bu listede bağlantı kurulacak sanal ağın adı gösterilir ve liste aşağıdaki örneğe benzer şekilde görünür: 
   
    ![VPN istemcisi](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/vpn.png)


## <a name="a-nameinstallclientcertapart-10---install-the-client-certificate"></a><a name="installclientcert"></a>10. Kısım - İstemci sertifikasını yükleme
Her istemci bilgisayarın kimlik doğrulaması için bir istemci sertifikası olmalıdır. İstemci sertifikasını yüklerken, istemci sertifikası dışarı aktarılırken oluşturulan parola gerekir.

1. .pfx dosyasını istemci bilgisayara kopyalayın.
2. .pfx dosyasına çift tıklayarak yükleme işlemini gerçekleştirin. Yükleme konumunu değiştirmeyin.

## <a name="a-nameconnectapart-11---connect-to-azure"></a><a name="connect"></a>11. Kısım - Azure'a bağlanma
1. İstemci bilgisayarda VNet'inize bağlanmak için VPN bağlantılarında gezinin ve oluşturduğunuz VPN bağlantısını bulun. Bu VPN bağlantısı sanal ağınızla aynı ada sahiptir. **Bağlan**'a tıklayın. Sertifika kullanımına ilişkin bir açılır ileti görüntülenebilir. Böyle bir durumla karşılaşırsanız yükseltilmiş ayrıcalıkları kullanmak için **Devam**'a tıklayın. 
2. **Bağlantı** durum sayfasında **Bağlan**'a tıklayarak bağlantıyı başlatın. Bir **Sertifika Seç** ekranı çıkarsa, gösterilen istemci sertifikasının bağlanmak için kullanmak istediğiniz sertifika olduğunu doğrulayın. Başka bir sertifika gösteriliyorsa, açılan liste okunu kullanarak doğru sertifikayı seçin ve **Tamam**’a tıklayın.
   
    ![Azure’a bağlanan VPN istemcisi](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/clientconnect.png)

    
3. Artık bağlantı kurabilirsiniz.
   
    ![Azure’a bağlı VPN istemcisi](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/connected.png)
                                                                                                                                                                           

> [!NOTE]
> Bir Kuruluş Sertifika Yetkilisi çözümü kullanarak verilen bir sertifika kullanıyor ve kimlik doğrulama sorunu yaşıyorsanız, istemci sertifikasındaki kimlik doğrulama sırasını denetleyin. Kimlik doğrulama listesinin sırasını istemci sertifikasına çift tıklayıp **Ayrıntılar > Gelişmiş Anahtar Kullanımı**’na giderek denetleyebilirsiniz. Listede ilk öğe olarak ‘İstemci Kimlik Doğrulaması’nın göründüğünden emin olun. Aksi takdirde, listedeki ilk öğe olarak İstemci Kimlik Doğrulaması’nı içeren Kullanıcı şablonunu temel alarak oluşturulmuş bir istemci sertifikası vermeniz gerekir. 
>
>

## <a name="a-nameverifyapart-12---verify-your-connection"></a><a name="verify"></a>12. Kısım - Bağlantınızı doğrulama
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

İptal edilen istemci sertifikalarının listesini **Noktadan siteye yapılandırma** dikey penceresinde yönetebilirsiniz. Bu, [güvenilen kök sertifika yüklemek](#uploadfile) için kullandığınız dikey penceredir. Sertifika adını ve parmak izini ekleyip kaydedin.

## <a name="a-namefaqapoint-to-site-faq"></a><a name="faq"></a>Noktadan Siteye hakkında SSS

[!INCLUDE [Point-to-Site FAQ](../../includes/vpn-gateway-point-to-site-faq-include.md)]

## <a name="next-steps"></a>Sonraki adımlar
Bağlantınız tamamlandıktan sonra sanal ağlarınıza sanal makineler ekleyebilirsiniz. Daha fazla bilgi için bkz. [Sanal Makineler](https://docs.microsoft.com/azure/#pivot=services&panel=Compute).




<!--HONumber=Feb17_HO3-->


