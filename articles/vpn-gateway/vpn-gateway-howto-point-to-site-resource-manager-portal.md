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
ms.date: 03/02/2017
ms.author: cherylmc
translationtype: Human Translation
ms.sourcegitcommit: cea53acc33347b9e6178645f225770936788f807
ms.openlocfilehash: bc3d0252b3aef6a62c863b79839557e165d9f121
ms.lasthandoff: 03/03/2017


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

### <a name="example"></a>Örnek değerler
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

## <a name="createvnet"></a>1. Bölüm - Sanal ağ oluşturma
Bu yapılandırmayı bir alıştırma olarak oluşturuyorsanız [örnek değerlere](#example) başvurabilirsiniz.

[!INCLUDE [vpn-gateway-basic-vnet-rm-portal](../../includes/vpn-gateway-basic-vnet-rm-portal-include.md)]

## <a name="address"></a>2. Kısım - Adres alanı ve alt ağ belirtme
Ek adres alanı ve alt ağları VNet’iniz oluşturulduktan sonra ekleyebilirsiniz.

[!INCLUDE [vpn-gateway-additional-address-space](../../includes/vpn-gateway-additional-address-space-include.md)]

## <a name="gatewaysubnet"></a>3. Kısım - Ağ geçidi alt ağı ekleme

Sanal ağınızı bir ağ geçidine bağlamadan önce, bağlamak istediğiniz sanal ağ için ağ geçidi alt ağını oluşturmanız gerekir. Ağ geçidi hizmetleri, ağ geçidi alt ağında belirtilen IP adreslerini kullanır. Mümkünse, gelecekte eklenecek yapılandırma gereksinimlerine yetecek sayıda IP adresi sağlamak için ağ geçidi alt ağını /28 veya /27 değerine sahip bir CIDR bloğu kullanarak oluşturun.

Bu bölümdeki ekran görüntüleri örnek amaçlıdır. Ağ Geçidi Alt Ağı adres aralığına yapılandırmanız için gerekli olan değerleri girdiğinizden emin olun.

###<a name="to-create-a-gateway-subnet"></a>Bir ağ geçidi alt ağı oluşturmak için

[!INCLUDE [vpn-gateway-add-gwsubnet-rm-portal](../../includes/vpn-gateway-add-gwsubnet-rm-portal-include.md)]

## <a name="dns"></a>4. Kısım - DNS sunucusu belirtme (isteğe bağlı)
[!INCLUDE [vpn-gateway-add-dns-rm-portal](../../includes/vpn-gateway-add-dns-rm-portal-include.md)]

## <a name="creategw"></a>5. Kısım - Sanal ağ geçidi oluşturma
Noktadan siteye bağlantılar için şu ayarların yapılması gerekir:

* Ağ geçidi türü: VPN
* VPN türü: Rota tabanlı

### <a name="to-create-a-virtual-network-gateway"></a>Bir sanal ağ geçidi oluşturmak için
[!INCLUDE [vpn-gateway-add-gw-rm-portal](../../includes/vpn-gateway-add-gw-rm-portal-include.md)]

## <a name="generatecert"></a>6. Kısım - Sertifika oluşturma
Noktadan Siteye VPN’lerde VPN istemcilerinin kimlik doğrulamasını yapmak için Azure tarafından sertifikalar kullanılır. Genel sertifika verilerini (özel anahtarı değil), kurumsal bir sertifika çözümü tarafından oluşturulan veya otomatik olarak imzalanan bir kök sertifikadan Base-64 ile kodlanmış X.509 .cer dosyası olarak dışarı aktarırsınız. Ardından kök sertifikanın genel sertifika verilerini Azure'a aktarırsınız. Ek olarak, istemciler için kök sertifikadan bir istemci sertifikası oluşturmanız gerekir. P2S bağlantısı kullanarak sanal ağa bağlanmak isteyen her istemcide, kök sertifikadan oluşturulmuş bir istemci sertifikası yüklü olmalıdır.

### <a name="getcer"></a>1. Adım: Kök sertifikaya ilişkin .cer dosyasını alma

Bir kuruluş çözümü kullanıyorsanız var olan sertifika zincirinizi kullanabilirsiniz. Kullanmak istediğiniz kök sertifika için .cer dosyasını alın.

Kurumsal bir sertifika çözümü kullanmıyorsanız, otomatik olarak imzalanan bir sertifika oluşturmanız gerekir. P2S kimlik doğrulaması için gerekli alanları içeren bir otomatik olarak imzalanan sertifika oluşturmak için makecert kullanın. [P2S bağlantıları için otomatik olarak imzalanan bir kök sertifika oluşturma](vpn-gateway-certificates-point-to-site.md) bölümü, otomatik olarak imzalanan bir kök sertifika oluşturma konusunda size adım adım yol gösterir. Makecert kullanım dışı bırakılmış olsa da, şu an için desteklenen çözümdür.

>[!NOTE]
>Otomatik olarak imzalanan sertifikalar oluşturmak için PowerShell kullanılması mümkün olsa da, PowerShell kullanılarak oluşturulan sertifika Noktadan Siteye kimlik doğrulaması için gereken alanları içermez.
>


#### <a name="to-obtain-the-cer-file-from-a-self-signed-root-certificate"></a>Otomatik olarak imzalanan bir kök sertifika için .cer dosyasını alma

1. Otomatik olarak imzalanan bir kök sertifika için .cer dosyasını almak için, **certmgr.msc**’yi açın ve oluşturduğunuz kök sertifikayı bulun. Sertifika genellikle 'Sertifikalar-Geçerli Kullanıcı/ Kişisel/Sertifikalar' içinde bulunur ve oluşturduğunuz zaman seçtiğiniz ada sahiptir. Otomatik olarak imzalanmış kök sertifikaya sağ tıklayın, **Tüm görevler**'e ve ardından **Dışarı aktar**'a tıklayın. **Sertifika Dışarı Aktarma Sihirbazı** açılır.
2. Sihirbazda **İleri**'ye tıklayın, **Hayır, özel anahtarı dışarı aktarma**'yı seçin ve **İleri**'ye tıklayın.
3. **Dışarı Aktarma Dosyası Biçimi** sayfasında **Base-64 ile kodlanmış X.509 (.CER)** seçeneğini belirleyin. Ardından **İleri**'ye tıklayın.
4. **Dışarı Aktarılan Dosya** sayfasında **Gözat**'a tıklayarak sertifika için dışarı aktarma konumunu seçin. **Dosya adı** alanına, sertifika dosyası için bir ad girin. Ardından **İleri**'ye tıklayın.
5. Sertifikayı dışarı aktarmak için **Son**'a tıklayın.

### <a name="generateclientcert"></a>2. Adım - İstemci sertifikası oluşturma
Sanal ağa bağlanacak her istemci için benzersiz bir sertifika oluşturabilir veya birden çok istemcide aynı sertifikayı kullanabilirsiniz. Benzersiz istemci sertifikaları oluşturmanın avantajı, gerektiğinde tek bir sertifikayı iptal edebiliyor olmanızdır. Aksi takdirde, herkes aynı istemci sertifikasını kullanıyorsa ve bir istemci için sertifikayı iptal etmeniz gerekirse, kimlik doğrulaması için bu sertifikayı kullanan tüm istemciler için yeni sertifikalar oluşturup yüklemeniz gerekir.

####<a name="enterprise-certificate"></a>Kurumsal sertifika
- Kurumsal bir sertifika çözümü kullanıyorsanız, 'etkialaniadi\kullaniciadi' biçimini kullanmak yerine, yaygın olarak kullanılan 'name@yourdomain.com' ad değer biçimiyle bir istemci sertifikası oluşturun.
- Verdiğiniz istemci sertifikasının, kullanım listesindeki ilk öğe olarak Akıllı Kart Oturumu, vb. yerine ‘İstemci Kimlik Doğrulaması’na sahip ‘Kullanıcı’ sertifikası şablonunu temel alarak hazırlandığından emin olun. İstemci sertifikasına sağ tıklayıp **Ayrıntılar > Gelişmiş Anahtar Kullanımı**’nı görüntüleyerek sertifikayı denetleyebilirsiniz.

####<a name="self-signed-certificate"></a>Otomatik olarak imzalanan sertifika 
Otomatik olarak imzalanan sertifika kullanıyorsanız istemci sertifikası oluşturmak için bkz. [Noktadan Siteye yapılandırmaları için otomatik olarak imzalanan kök sertifikalar ile çalışma](vpn-gateway-certificates-point-to-site.md).

### <a name="exportclientcert"></a>3. Adım - İstemci sertifikasını dışarı aktarma
Kimlik doğrulaması için istemci sertifikası gereklidir. İstemci sertifikasını oluşturduktan sonra dışarı aktarın. Dışarı aktardığınız istemci sertifikası, daha sonra tüm istemci bilgisayarlara yüklenecek.

1. İstemci sertifikasını dışarı aktarmak için *certmgr.msc* kullanabilirsiniz. Dışarı aktarmak istediğiniz istemci sertifikasına sağ tıklayın, **tüm görevler**’e ve ardından **dışarı aktar**’a tıklayın.
2. Özel anahtara sahip istemci sertifikasını dışarı aktarın. Bu bir *.pfx* dosyasıdır. Bu sertifika için ayarladığınız parolayı (anahtar) kaydettiğinizden ya da unutmayacağınızdan emin olun.

## <a name="addresspool"></a>7. Kısım - İstemci adres havuzunu ekleme
1. Sanal ağ geçidini oluşturduktan sonra dikey penceresindeki **Ayarlar** bölümüne gidin. **Ayarlar** bölümünde **Noktadan siteye yapılandırma**’ya tıklayarak **Yapılandırma** dikey penceresini açın.
   
    ![Noktadan Siteye dikey penceresi](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/configuration.png)
2. **Adres havuzu**, bağlanan istemcilerin alacağı IP adreslerinin bulunduğu havuzdur. Adres havuzunu ekleyip **Kaydet**’e tıklayın.
   
    ![İstemci adres havuzu](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/ipaddresspool.png)

## <a name="uploadfile"></a>8. Kısım - Kök sertifika .cer dosyasını yükleme
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

## <a name="clientconfig"></a>9. Kısım - VPN istemcisi yapılandırma paketini indirme ve yükleme
P2S kullanarak Azure’a bağlanan istemcilerde hem bir istemci sertifikası hem de bir VPN istemcisi yapılandırma paketi yüklü olmalıdır. Windows istemcileri için VPN istemcisi yapılandırma paketleri kullanılabilir. 

VPN istemci paketi, Windows'da yerleşik bulunan VPN istemci yazılımını yapılandırmaya yönelik bilgileri içerir. Bu yapılandırma, bağlanmak istediğiniz VPN’e özeldir. Paket, ek yazılım yüklemez.

Sürümünün istemci mimarisiyle eşleşmesi şartıyla, her istemci bilgisayarda aynı VPN istemcisi yapılandırma paketini kullanabilirsiniz.

### <a name="step-1---download-the-client-configuration-package"></a>1 Adım - İstemci yapılandırma paketini indirme

1. **Noktadan siteye yapılandırma** dikey penceresinde **VPN istemcisini indir**’e tıklayarak **VPN istemcisini indir** dikey penceresini açın. Paketin oluşturulması birkaç dakika sürer.
   
    ![VPN istemcisi indirme 1](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/downloadvpnclient1.png)
2. İstemciniz için doğru paketi seçip **İndir**’e tıklayın. Yapılandırma paketi dosyasını kaydedin. Bu dosyayı, sanal ağa bağlanacak her istemci bilgisayara yükleyeceksiniz.

    ![VPN istemcisi indirme 2](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/client.png)

   * 64 bit istemciler için **AMD64**’ü seçin.
   * 32 bit istemciler için **x86**’yı seçin.

### <a name="step-2---install-the-client-configuration-package"></a>2. Adım: İstemci yapılandırma paketini yükleme

1. Yapılandırma dosyasını, sanal ağınıza bağlamak istediğiniz bilgisayara yerel olarak kopyalayın. 
2. Paketi istemci bilgisayara yüklemek için .exe dosyasına çift tıklayın. Yapılandırma paketi sizin tarafınızdan oluşturulduğu için imzalanmamıştır. Bu, bir uyarı görebileceğiniz anlamına gelir. Bir Windows SmartScreen açılır penceresi görürseniz **Daha fazla bilgi**’ye (solda) ve ardından **Yine de çalıştır**’a tıklayarak paketi yükleyin.
3. İstemci bilgisayarda **Ağ Ayarları**’na gidin ve **VPN** öğesine tıklayın. Bağlantının listelendiğini görürsünüz. Bu listede bağlantı kurulacak sanal ağın adı gösterilir ve liste aşağıdaki gibi görünür:
3. Paketi istemci bilgisayara yükleyin. Bir Windows SmartScreen açılır penceresi görürseniz **Daha fazla bilgi**’ye (solda) ve ardından **Yine de çalıştır**’a tıklayarak paketi yükleyin.
4. İstemci bilgisayarda **Ağ Ayarları**’na gidin ve **VPN** öğesine tıklayın. Bağlantının listelendiğini görürsünüz. Bu listede bağlantı kurulacak sanal ağın adı gösterilir ve liste aşağıdaki örneğe benzer şekilde görünür: 
   
    ![VPN istemcisi](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/vpn.png)


## <a name="installclientcert"></a>10. Kısım - İstemci sertifikasını yükleme
Her istemci bilgisayarın kimlik doğrulaması için bir istemci sertifikası olmalıdır. İstemci sertifikasını yüklerken, istemci sertifikası dışarı aktarılırken oluşturulan parola gerekir.

1. .pfx dosyasını istemci bilgisayara kopyalayın.
2. .pfx dosyasına çift tıklayarak yükleme işlemini gerçekleştirin. Yükleme konumunu değiştirmeyin.

## <a name="connect"></a>11. Kısım - Azure'a bağlanma
1. İstemci bilgisayarda VNet'inize bağlanmak için VPN bağlantılarında gezinin ve oluşturduğunuz VPN bağlantısını bulun. Bu VPN bağlantısı sanal ağınızla aynı ada sahiptir. **Bağlan**'a tıklayın. Sertifika kullanımına ilişkin bir açılır ileti görüntülenebilir. Böyle bir durumla karşılaşırsanız yükseltilmiş ayrıcalıkları kullanmak için **Devam**'a tıklayın. 
2. **Bağlantı** durum sayfasında **Bağlan**'a tıklayarak bağlantıyı başlatın. Bir **Sertifika Seç** ekranı çıkarsa, gösterilen istemci sertifikasının bağlanmak için kullanmak istediğiniz sertifika olduğunu doğrulayın. Başka bir sertifika gösteriliyorsa, açılan liste okunu kullanarak doğru sertifikayı seçin ve **Tamam**’a tıklayın.
   
    ![Azure’a bağlanan VPN istemcisi](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/clientconnect.png)

    
3. Artık bağlantı kurabilirsiniz.
   
    ![Azure’a bağlı VPN istemcisi](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/connected.png)
                                                                                                                                                                           

> [!NOTE]
> Bir Kuruluş Sertifika Yetkilisi çözümü kullanarak verilen bir sertifika kullanıyor ve kimlik doğrulama sorunu yaşıyorsanız, istemci sertifikasındaki kimlik doğrulama sırasını denetleyin. Kimlik doğrulama listesinin sırasını istemci sertifikasına çift tıklayıp **Ayrıntılar > Gelişmiş Anahtar Kullanımı**’na giderek denetleyebilirsiniz. Listede ilk öğe olarak ‘İstemci Kimlik Doğrulaması’nın göründüğünden emin olun. Aksi takdirde, listedeki ilk öğe olarak İstemci Kimlik Doğrulaması’nı içeren Kullanıcı şablonunu temel alarak oluşturulmuş bir istemci sertifikası vermeniz gerekir. 
>
>

## <a name="verify"></a>12. Kısım - Bağlantınızı doğrulama
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

## <a name="add"></a>Güvenilen kök sertifika ekleme veya kaldırma
Azure’da güvenilen kök sertifikayı ekleyebilir veya kaldırabilirsiniz. Güvenilen kök sertifikayı kaldırdığınızda, kök sertifikadan oluşturulan istemci sertifikaları artık Noktadan Siteye bağlantı aracılığıyla Azure'a bağlanamaz. İstemciler, bağlanabilmek için Azure'da güvenilen bir sertifikadan oluşturan yeni bir istemci sertifikası yüklemelidir.

### <a name="to-add-a-trusted-root-certificate"></a>Güvenilen kök sertifika ekleme

Azure'a en fazla 20 güvenilen kök sertifika .cer dosyası ekleyebilirsiniz. Yönergeler için, bu makaledeki [Güvenilen bir kök sertifikayı karşıya yükleme](#uploadfile) bölümüne bakın.

### <a name="to-remove-a-trusted-root-certificate"></a>Güvenilen kök sertifikayı kaldırmak için

1. Güvenilen kök sertifikayı kaldırmak için, sanal ağ geçidinizin **Noktadan siteye yapılandırma** dikey penceresine gidin.
2. Dikey pencerenin **Kök sertifika** bölümünde, kaldırmak istediğiniz sertifikayı bulun.
3. Sertifikanın yanındaki üç noktaya tıklayın ve ardından ‘Kaldır’a tıklayın.

## <a name="revokeclient"></a>İstemci sertifikasını iptal etme
İstemci sertifikalarını iptal edebilirsiniz. Sertifika iptal listesi sayesinde, ayrı istemci sertifikalarına göre Noktadan Siteye bağlantıyı seçmeli olarak reddedebilirsiniz. Bu, güvenilen kök sertifikayı kaldırma işleminden farklıdır. Azure’dan güvenilen kök sertifika .cer dosyasını kaldırırsanız iptal edilen kök sertifika tarafından oluşturulan/imzalanan tüm istemci sertifikaları reddedilir. Kök sertifika yerine istemci sertifikasını iptal etmek, kök sertifikadan oluşturulan diğer sertifikaların Noktadan Siteye bağlantı için kimlik doğrulaması amacıyla kullanılmaya devam edilmesine olanak sağlar.

Genellikle ekip ve kuruluş düzeylerinde erişimi yönetmek için kök sertifika kullanılırken ayrı kullanıcılar üzerinde ayrıntılı erişim denetimi için iptal edilen istemci sertifikaları kullanılır.

### <a name="to-revoke-a-client-certificate"></a>İstemci sertifikasını iptal etme

Parmak izini iptal listesine ekleyerek bir istemci sertifikasını iptal edebilirsiniz.

1. İstemci sertifikasının parmak izini alın. Daha fazla bilgi için bkz. [Bir Sertifikanın Parmak İzini alma](https://msdn.microsoft.com/library/ms734695.aspx).
2. Bilgileri bir metin düzenleyicisine kopyalayın ve sürekli bir dize haline getirmek için tüm boşlukları kaldırın.
3. Sanal ağ geçidi **Noktadan siteye yapılandırma** dikey penceresine gidin. Bu, [güvenilen kök sertifika yüklemek](#uploadfile) için kullandığınız dikey penceredir.
4. **İptal edilen sertifikalar** bölümünde, sertifika için bir kolay ad girin (sertifika genel adından farklı olabilir).
5. Parmak izi dizesini kopyalayın ve **Parmak izi** alanına yapıştırın.
6. Parmak izi doğrulanır ve otomatik olarak iptal listesine eklenir. Ekranda listenin güncelleştirildiğini belirten bir ileti görürsünüz. 
7. Güncelleştirme tamamlandıktan sonra sertifika artık bağlanmak için kullanılamaz. Bu sertifikayı kullanarak bağlanmaya çalışan istemciler sertifikanın artık geçerli olmadığını belirten bir ileti alır.

## <a name="faq"></a>Noktadan Siteye hakkında SSS

[!INCLUDE [Point-to-Site FAQ](../../includes/vpn-gateway-point-to-site-faq-include.md)]

## <a name="next-steps"></a>Sonraki adımlar
Bağlantınız tamamlandıktan sonra sanal ağlarınıza sanal makineler ekleyebilirsiniz. Daha fazla bilgi için bkz. [Sanal Makineler](https://docs.microsoft.com/azure/#pivot=services&panel=Compute).


