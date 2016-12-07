---
title: "Azure portalını kullanarak bir Azure Sanal Ağa yönelik Noktadan Konuma VPN Gateway bağlantısı yapılandırma | Microsoft Belgeleri"
description: "Azure portalı ile Noktadan Siteye bir VPN Gateway bağlantısı oluşturarak Azure Sanal Ağınıza güvenli bir şekilde bağlanın."
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: carmonm
editor: 
tags: azure-service-management
ms.assetid: 65e14579-86cf-4d29-a6ac-547ccbd743bd
ms.service: vpn-gateway
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/17/2016
ms.author: cherylmc
translationtype: Human Translation
ms.sourcegitcommit: f6fa6511c8d54e191de27fda73aad9feb734191f
ms.openlocfilehash: 11d27b786522d1f780a701229ed0a695224e9eb6


---
# <a name="configure-a-point-to-site-connection-to-a-vnet-using-the-azure-portal"></a>Azure portalı kullanarak bir sanal ağa Noktadan Siteye bir bağlantı yapılandırma
> [!div class="op_single_selector"]
> * [Resource Manager - Azure Portal](vpn-gateway-howto-point-to-site-resource-manager-portal.md)
> * [Resource Manager - PowerShell](vpn-gateway-howto-point-to-site-rm-ps.md)
> * [Klasik - Azure Portal](vpn-gateway-howto-point-to-site-classic-azure-portal.md)
> 
> 

Bu makalede Azure portalı kullanılarak klasik dağıtım modelinde Noktadan Siteye bağlantı ile sanal ağ oluşturma işlemi adım adım açıklanmaktadır. Noktadan Siteye (P2S) yapılandırması, ayrı bir istemci bilgisayardan bir sanal ağa yönelik güvenli bağlantı oluşturmanıza olanak sağlar. Sanal ağınıza uzak bir konumdan (örneğin, evden veya bir konferanstan) bağlanmak istediğinizde P2S bağlantısı kullanışlıdır. Veya, bir sanal ağa bağlanması gereken yalnızca birkaç istemciniz olduğunda bu bağlantı yararlıdır.

Noktadan Siteye bağlantıların çalışması için bir VPN cihazına veya genel kullanıma yönelik bir IP adresine gerek yoktur. VPN bağlantısı, bağlantının istemci bilgisayardan başlatılmasıyla oluşturulur. Noktadan Siteye bağlantılar hakkında daha fazla bilgi edinmek için [VPN Gateway ile ilgili SSS](vpn-gateway-vpn-faq.md#point-to-site-connections) ve [VPN Gateway Hakkında](vpn-gateway-about-vpngateways.md#point-to-site) başlıklı makalelere bakın.

### <a name="deployment-models-and-methods-for-p2s-connections"></a>P2S bağlantıları için dağıtım modelleri ve yöntemleri
[!INCLUDE [deployment models](../../includes/vpn-gateway-deployment-models-include.md)]

Aşağıdaki tabloda, P2S yapılandırmaları için iki dağıtım modeli ve kullanılabilir dağıtım yöntemleri gösterilmektedir. Yapılandırma adımlarını içeren bir makale olduğunda, bu tablodan makaleye yönelik doğrudan bağlantı oluştururuz.

[!INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-table-point-to-site-include.md)]

## <a name="basic-workflow"></a>Temel iş akışı
![Noktadan Siteye diyagramı](./media/vpn-gateway-howto-point-to-site-rm-ps/p2srm.png "point-to-site")

Aşağıdaki bölümlerde, sanal ağa yönelik güvenli bir Noktadan Siteye bağlantı oluşturma işlemi adım adım açıklanmaktadır. 

1. Sanal ağ ve VPN Gateway oluşturma
2. İstemci sertifikaları oluşturma
3. .cer dosyasını karşıya yükleme
4. VPN istemcisi yapılandırma paketini oluşturma
5. İstemci bilgisayarı yapılandırma
6. Azure'a Bağlanma

### <a name="example-settings"></a>Örnek ayarlar
Aşağıdaki örnek ayarları kullanabilirsiniz:

* **Ad: VNet1**
* **Adres alanı: 192.168.0.0/16**
* **Alt ağ adı: FrontEnd**
* **Alt ağ adres aralığı: 192.168.1.0/24**
* **Abonelik:** Birden fazla aboneliğiniz varsa doğru aboneliği kullandığınızdan emin olun.
* **Kaynak Grubu: TestRG**
* **Konum: Doğu ABD**
* **Bağlantı türü: Noktadan siteye**
* **İstemci Adres Alanı: 172.16.201.0/24**. Sanal ağa bu Noktadan Siteye bağlantıyı kullanarak bağlanan VPN istemcileri belirtilen havuzdan bir IP adresi alır.
* **GatewaySubnet: 192.168.200.0/24**. Ağ Geçidi alt ağı "GatewaySubnet" adını kullanmalıdır.
* **Boyut:** Kullanmak istediğiniz ağ geçidi SKU’sunu seçin.
* **Yönlendirme Türü: Dinamik**

## <a name="a-namevnetvpnasection-1---create-a-virtual-network-and-a-vpn-gateway"></a><a name="vnetvpn"></a>1. Bölüm - Sanal ağ ve VPN ağ geçidi oluşturma
### <a name="a-namecreatevnetapart-1-create-a-virtual-network"></a><a name="createvnet"></a>1. Kısım - Sanal ağ oluşturma
Sanal ağınız yoksa bir sanal ağ oluşturun. Ekran görüntüleri örnek olarak verilmiştir. Değerlerin kendinizinkilerle değiştirildiğinden emin olun. Azure portalını kullanarak sanal ağ oluşturmak için şu adımları uygulayın: 

1. Tarayıcıdan [Azure portalına](http://portal.azure.com) gidin ve gerekiyorsa Azure hesabınızda oturum açın.
2. **Yeni**’ye tıklayın. **Market’te ara** alanına "Sanal Ağ" yazın. Döndürülen listeden **Sanal Ağ**’ı bulun ve tıklayarak **Sanal Ağ** dikey penceresini açın.
   
    ![Sanal ağ ara dikey penceresi](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/newvnetportal700.png "Search for virtual network blade")
3. Sanal Ağ dikey penceresinin altı yakınlarında, **Bir dağıtım modeli seçin** listesinden **Klasik**’i seçip **Oluştur**’a tıklayın.
   
    ![Dağıtım modeli seçme](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/selectmodel.png "Select deployment model")
4. **Sanal ağ oluştur** dikey penceresinde VNet ayarlarını yapılandırın. Bu dikey pencerede, ilk adres alanınızı ve tek alt ağ adres aralığınızı eklersiniz. VNet oluşturma işlemini tamamladıktan sonra geri dönüp ek alt ağları ve adres alanlarını ekleyin.
   
    ![Sanal ağ oluştur dikey penceresi](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/vnet125.png "Create virtual network blade")
5. **Abonelik** alanında doğru bir giriş olduğunu doğrulayın. Açılan listeyi kullanarak abonelikleri değiştirebilirsiniz.
6. **Kaynak grubu**’na tıklayın, ya varolan bir kaynak grubunu seçin ya da yeni kaynak grubunuz için bir ad yazarak yeni bir tane oluşturun. Yeni bir grup oluşturuyorsanız, planlanan yapılandırma değerlerinize göre kaynak grubunu adlandırın. Kaynak grupları hakkında daha fazla bilgi için [Azure Resource Manager’a Genel Bakış](../azure-resource-manager/resource-group-overview.md#resource-groups)’ı ziyaret edin.
7. Ardından, VNet’iniz için **Konum** ayarlarını seçin. Bu Sanal ağa dağıttığınız kaynakların nerede olacağını konum belirler.
8. VNet’inizi panoda kolay bulmak istiyorsanız **Panoya sabitle**’yi seçin ve ardından **Oluştur**’a tıklayın.
   
    ![Panoya sabitle](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/pintodashboard150.png "Pin to dashboard")
9. Oluştur’a tıkladıktan sonra, panonuzda VNet’inizin ilerleme durumunu yansıtacak bir kutucuk göreceksiniz. Sanal ağ oluşturulurken kutucuk değişir.
   
    ![Sanal ağ kutucuğu oluşturma](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/deploying150.png "Creating virtual network tile")
10. Sanal ağınızı oluşturduktan sonra ad çözünürlüğünü işlemek için bir DNS sunucusunun IP adresini ekleyebilirsiniz. Sanal ağınızın ayarlarını açın, DNS sunucularına tıklayın ve kullanmak istediğiniz DNS sunucusunun IP adresini ekleyin. Bu ayarla yeni bir DNS sunucusu oluşturulmaz. Kaynaklarınızın iletişim kurabileceği bir DNS sunucusu eklediğinizden emin olun.

Sanal ağınız oluşturulduğunda, klasik Azure portalının ağlar sayfasındaki **Durum** seçeneğinin altında **Oluşturuldu** ifadesinin yer aldığını görürsünüz.

### <a name="a-namegatewayapart-2-create-gateway-subnet-and-a-dynamic-routing-gateway"></a><a name="gateway"></a>2. Kısım: Ağ geçidi alt ağı ve dinamik yönlendirme ağ geçidi oluşturma
Bu adımda bir ağ geçidi alt ağı ve dinamik yönlendirme ağ geçidi oluşturacaksınız. Klasik dağıtım modeli için Azure portalında aynı ağ geçidi alt ağı ve ağ geçidi, aynı yapılandırma pencerelerinden oluşturulabilir.

1. Portalda, ağ geçidi oluşturmak istediğiniz sanal ağa gidin.
2. Sanal ağınızın dikey penceresindeki **Genel Bakış** dikey penceresinin VPN bağlantıları bölümünde **Ağ Geçidi**’ne tıklayın.
   
    ![Bir ağ geçidi oluşturmak için buraya tıklayın](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/beforegw125.png "Click here to create a gateway")
3. **Yeni VPN Bağlantısı** dikey penceresinde **Noktadan siteye** öğesini seçin.
   
    ![P2S bağlantı türü](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/newvpnconnect.png "P2S connection type")
4. **İstemci Adres Alanı** için IP adresi aralığını ekleyin. Bu aralık, VPN istemcilerinin bağlanırken bir IP adresi alacağı aralıktır. Otomatik olarak doldurulan aralığı silin ve kendi aralığınızı ekleyin.
   
    ![İstemci adres alanı](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/clientaddress.png "Client address space")
5. **Ağ geçidini hemen oluştur** onay kutusunu seçin.
   
    ![Ağ geçidini hemen oluşturma](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/creategwimm.png "Create gateway immediately")
6. **İsteğe bağlı ağ geçidi yapılandırması**’na tıklayarak **Ağ geçidi yapılandırması** dikey penceresini açın.
   
    ![İsteğe bağlı ağ geçidi yapılandırması'na tıklayın](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/optsubnet125.png "Click optional gateway configuration")
7. **Alt Ağ Yapılandırma zorunlu ayarları**’na tıklayarak **ağ geçidi alt ağı** ekleyin. /29 kadar küçük bir ağ geçidi alt ağı oluşturmak mümkün olsa da en az /28 veya /27’yi seçerek daha fazla adres içeren büyük bir alt ağ oluşturmanızı öneririz. Bu, gelecekte isteyebileceğiniz ek yapılandırmaları da içerecek yeteri kadar adres sağlayacaktır.
   
   > [!IMPORTANT]
   > Ağ geçidi alt ağlarıyla çalışırken, ağ güvenlik grubunu (NSG) ağ geçidi alt ağıyla ilişkilendirmekten kaçının. Ağ güvenlik grubunun bu alt ağ ile ilişkilendirilmesi, VPN Gateway’inizin beklendiği gibi çalışmayı durdurmasına neden olabilir. Ağ güvenlik grupları hakkında daha fazla bilgi için bkz. [Ağ güvenlik grubu nedir?](../virtual-network/virtual-networks-nsg.md)
   > 
   > 
   
    ![GatewaySubnet ekleme](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/gwsubnet125.png "Add GatewaySubnet")
8. Ağ geçidi **Boyutu** seçin. Bu seçenek, sanal ağ geçidinizi oluşturmak için kullanacağınız ağ geçidi SKU’sudur. Portalda Varsayılan SKU, **Temel**’dir. Ağ geçidi SKU’ları hakkında bilgi için bkz. [VPN Gateway Ayarları Hakkında](vpn-gateway-about-vpn-gateway-settings.md#gwsku).
   
    ![ağ geçidi boyutu](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/gwsize125.png)
9. Ağ geçidiniz için **Yönlendirme Türü** seçin. P2S yapılandırmaları bir **Dinamik** yönlendirme türü gerektirir. Bu dikey pencereyi yapılandırmayı bitirdiğinizde **Tamam**’a tıklayın.
   
    ![Yönlendirme türünü yapılandırma](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/routingtype125.png "Configure routing type")
10. **Yeni VPN Bağlantısı** dikey penceresinde, sanal ağ geçidinizi oluşturmaya başlamak için dikey pencerenin en altındaki **Tamam**’a tıklayın. Bu işlemin tamamlanması 45 dakika sürebilir. 

## <a name="a-namegeneratecertsasection-2---generate-certificates"></a><a name="generatecerts"></a>Bölüm 2 - Sertifika oluşturma
Noktadan Siteye VPN’lerde VPN istemcilerinin kimlik doğrulamasını yapmak için Azure tarafından sertifikalar kullanılır. Genel sertifika verilerini (özel anahtarı değil), kurumsal bir sertifika çözümü tarafından oluşturulan veya otomatik olarak imzalanan bir kök sertifikadan Base-64 ile kodlanmış X.509 .cer dosyası olarak dışarı aktarırsınız. Ardından kök sertifikanın genel sertifika verilerini Azure'a aktarırsınız. Ek olarak, istemciler için kök sertifikadan bir istemci sertifikası oluşturmanız gerekir. P2S bağlantısı kullanarak sanal ağa bağlanmak isteyen her istemcide, kök sertifikadan oluşturulmuş bir istemci sertifikası yüklü olmalıdır.

### <a name="a-namecerapart-1-obtain-the-cer-file-for-the-root-certificate"></a><a name="cer"></a>1. Kısım: Kök sertifikaya ilişkin .cer dosyasını alma
Bir kuruluş çözümü kullanıyorsanız var olan sertifika zincirinizi kullanabilirsiniz. Kuruluş CA çözümü kullanmıyorsanız otomatik olarak imzalanan bir kök sertifika oluşturabilirsiniz. Otomatik olarak imzalanan sertifika oluşturmanın bir yolu makecert yöntemidir.

* Kurumsal bir sertifika sistemi kullanıyorsanız kullanmak istediğiniz kök sertifikaya ilişkin .cer dosyasını alın. 
* Kurumsal bir sertifika çözümü kullanmıyorsanız, otomatik olarak imzalanan bir sertifika oluşturmanız gerekir. Windows 10’a yönelik adımlar için [Working with self-signed root certificates for Point-to-Site configurations](vpn-gateway-certificates-point-to-site.md) (Noktadan Siteye yapılandırmaları için otomatik olarak imzalanan kök sertifikalar ile çalışma) makalesine başvurabilirsiniz.

1. Sertifikadan .cer dosyası almak için **certmgr.msc**'yi açın ve kök sertifikayı bulun. Otomatik olarak imzalanmış kök sertifikaya sağ tıklayın, **Tüm görevler**'e ve ardından **Dışarı aktar**'a tıklayın. **Sertifika Dışarı Aktarma Sihirbazı** açılır.
2. Sihirbazda **İleri**'ye tıklayın, **Hayır, özel anahtarı dışarı aktarma**'yı seçin ve **İleri**'ye tıklayın.
3. **Dışarı Aktarma Dosyası Biçimi** sayfasında **Base-64 ile kodlanmış X.509 (.CER)** seçeneğini belirleyin. Ardından **İleri**'ye tıklayın. 
4. **Dışarı Aktarılan Dosya** sayfasında **Gözat**'a tıklayarak sertifika için dışarı aktarma konumunu seçin. **Dosya adı** alanına, sertifika dosyası için bir ad girin. Ardından **İleri**'ye tıklayın.
5. Sertifikayı dışarı aktarmak için **Son**'a tıklayın.

### <a name="a-namegenclientcertapart-2-generate-a-client-certificate"></a><a name="genclientcert"></a>2. Kısım: İstemci sertifikası oluşturma
Bağlanacak her istemci için benzersiz bir sertifika oluşturabileceğiniz gibi, birden çok istemcide aynı sertifikayı da kullanabilirsiniz. Benzersiz istemci sertifikaları oluşturmanın avantajı, gerektiğinde tek bir sertifikayı iptal edebiliyor olmanızdır. Herkesin aynı istemci sertifikasını kullandığı bir durumda bir istemcinin sertifikasını iptal etmeniz gerektiğinde, kimlik doğrulaması için söz konusu sertifikayı kullanan tüm istemciler için yeni sertifikalar oluşturmanız ve yüklemeniz gerekir.

* Kurumsal bir sertifika çözümü kullanıyorsanız, 'etkialanıadı\kullanıcıadı' biçimini kullanmak yerine, yaygın olarak kullanılan 'name@yourdomain.com', ad değer biçimiyle bir istemci sertifikası oluşturun. 
* Otomatik olarak imzalanan sertifika kullanıyorsanız istemci sertifikası oluşturmak için bkz. [Noktadan Siteye yapılandırmaları için otomatik olarak imzalanan kök sertifikalar ile çalışma](vpn-gateway-certificates-point-to-site.md).

### <a name="a-nameexportclientcertapart-3-export-the-client-certificate"></a><a name="exportclientcert"></a>3. Kısım - İstemci sertifikasını dışarı aktarma
Sanal ağa bağlamak istediğiniz her bilgisayara bir istemci sertifikası yükleyin. Kimlik doğrulaması için istemci sertifikası gereklidir. İstemci sertifikası yükleme işlemini otomatik hale getirebilir veya elle yapabilirsiniz. Aşağıda, istemci sertifikasını dışarı aktarma ve elle yükleme işlemleri adım adım açıklanmıştır.

1. İstemci sertifikasını dışarı aktarmak için *certmgr.msc* kullanabilirsiniz. Dışarı aktarmak istediğiniz istemci sertifikasına sağ tıklayın, **tüm görevler**’e ve ardından **dışarı aktar**’a tıklayın.
2. Özel anahtara sahip istemci sertifikasını dışarı aktarın. Bu bir *.pfx* dosyasıdır. Bu sertifika için ayarladığınız parolayı (anahtar) kaydettiğinizden ya da unutmayacağınızdan emin olun.

## <a name="a-nameuploadasection-3---upload-the-root-certificate-cer-file"></a><a name="upload"></a>3. Bölüm - Kök sertifika .cer dosyasını karşıya yükleme
Ağ geçidi oluşturulduktan sonra güvenilen bir kök sertifika için .cer dosyasını Azure’a yükleyebilirsiniz. 20 adede kadar kök sertifikasının dosyasını yükleyebilirsiniz. Kök sertifikanın özel anahtarını Azure'a yüklemezsiniz. .cer dosyası yüklendikten sonra Azure, sanal ağa bağlanan istemcilerin kimliğini doğrulamak için bu anahtarı kullanır.

1. Sanal ağınıza ait dikey pencerenin **VPN bağlantıları** bölümünde **istemciler** grafiğine tıklayarak **Noktadan siteye VPN bağlantısı** dikey penceresini açın.
   
    ![İstemciler](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/clients125.png "Clients")
2. **Noktadan siteye bağlantı** dikey penceresinde **Sertifikaları yönet**’e tıklayarak **Sertifikalar** dikey penceresini açın.<br>
   
    ![Sertifikalar dikey penceresi](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/ptsmanage.png "Certificates blade")<br><br>
3. **Sertifikalar** dikey penceresinde **Karşıya Yükle**’ye tıklayarak **Sertifikayı karşıya yükle** dikey penceresini açın.<br>
   
    ![Sertifikaları karşıya yükle dikey penceresi](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/uploadcerts.png "Upload certificates blade")<br>
4. .cer dosyasına göz atmak için klasör grafiğine tıklayın. Dosyayı seçin ve ardından **Tamam**’a tıklayın. **Sertifikalar** dikey penceresine yüklenen sertifikayı görmek için sayfayı yenileyin.
   
    ![Sertifikayı karşıya yükleme](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/upload.png "Upload certificate")<br>

## <a name="a-namevpnclientconfigasection-4---generate-the-vpn-client-configuration-package"></a><a name="vpnclientconfig"></a>Bölüm 4 - VPN istemcisi yapılandırma paketini oluşturma
Sanal ağa bağlanmak için bir VPN istemcisi de yapılandırmanız gerekir. İstemci bilgisayarın bağlanması için bir istemci sertifikasına ve uygun VPN istemcisi yapılandırma paketine gerek vardır.

VPN istemci paketi, Windows'da yerleşik bulunan VPN istemci yazılımını yapılandırmaya yönelik yapılandırma bilgileri içerir. Paket, ek yazılım yüklemez. Ayarlar bağlanmak istediğiniz sanal ağa özeldir. Desteklenen istemci işletim sistemlerinin listesi için VPN Gateway ile ilgili SSS başlıklı makalenin [Noktadan Siteye bağlantılar](vpn-gateway-vpn-faq.md#point-to-site-connections) bölümüne göz atın. 

### <a name="to-generate-the-vpn-client-configuration-package"></a>VPN istemcisi yapılandırma paketini oluşturmak için
1. Azure portalında, sanal ağınızın **Genel Bakış** dikey penceresindeki **VPN bağlantıları** menüsünde istemci grafiğine tıklayarak **Noktadan siteye VPN bağlantısı** dikey penceresini açın.
2. **Noktadan siteye VPN bağlantısı** dikey penceresinin üst kısmında, yükleneceği istemci işletim sistemine karşılık gelen indirme paketini seçin:
   
   * 64 bit istemciler için **VPN İstemcisi (64 bit)** seçeneğini belirleyin.
   * 32 bit istemciler için **VPN İstemcisi (32 bit)** seçeneğini belirleyin.
     
     ![VPN istemcisi yapılandırma paketini indirme](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/dlclient.png "Download VPN client configuration package")<br>
3. Azure’un sanal ağ için VPN istemcisi yapılandırma paketini oluşturduğuna ilişkin bir ileti görürsünüz. Birkaç dakika sonra paket oluşturulur ve yerel bilgisayarınızda paketin indirildiğine yönelik bir ileti görürsünüz. Yapılandırma paketi dosyasını kaydedin. Bu dosyayı, P2S kullanarak sanal ağa bağlanacak her istemci bilgisayara yükleyeceksiniz.

## <a name="a-nameclientconfigurationasection-5---configure-the-client-computer"></a><a name="clientconfiguration"></a>5. Bölüm - İstemci bilgisayarı yapılandırma
### <a name="part-1-install-the-client-certificate"></a>1. Kısım - İstemci sertifikasını yükleme
Her istemci bilgisayarda kimlik doğrulaması için bir istemci sertifikası olması gerekir. İstemci sertifikasını yüklerken, istemci sertifikası dışarı aktarılırken oluşturulan parola gerekir.

1. .pfx dosyasını istemci bilgisayara kopyalayın.
2. .pfx dosyasına çift tıklayarak yükleme işlemini gerçekleştirin. Yükleme konumunu değiştirmeyin.

### <a name="part-2-install-the-vpn-client-configuration-package"></a>2. Kısım: VPN istemcisi yapılandırma paketini yükleme
Sürümünün istemci mimarisiyle eşleşmesi şartıyla, her istemci bilgisayarda aynı VPN istemcisi yapılandırma paketini kullanabilirsiniz.

1. Yapılandırma dosyasını, sanal ağınıza bağlamak istediğiniz bilgisayara yerel olarak kopyalayın ve .exe dosyasına çift tıklayın. 
2. Paket yüklendikten sonra VPN bağlantısını başlatabilirsiniz. Yapılandırma paketi Microsoft tarafından imzalanmamıştır. Paketi, kuruluşunuzun imzalama hizmetini kullanarak veya kendiniz [SignTool](http://go.microsoft.com/fwlink/p/?LinkId=699327) ile imzalamak isteyebilirsiniz. Paketin imzasız olarak kullanılmasında bir sorun yoktur. Ancak paket imzasızsa, yüklendiğinde bir uyarı görüntülenir.
3. İstemci bilgisayarda **Ağ Ayarları**’na gidin ve **VPN** öğesine tıklayın. Bağlantının listelendiğini görürsünüz. Bu listede bağlantı kurulacak sanal ağın adı gösterilir ve liste aşağıdaki gibi görünür: 
   
    ![VPN istemcisi](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/vpn.png "VNet VPN client")

## <a name="a-nameconnectasection-6---connect-to-azure"></a><a name="connect"></a>6. Bölüm - Azure'a bağlanma
### <a name="connect-to-your-vnet"></a>Sanal ağınıza bağlanma
1. İstemci bilgisayarda VNet'inize bağlanmak için VPN bağlantılarında gezinin ve oluşturduğunuz VPN bağlantısını bulun. Bu VPN bağlantısı sanal ağınızla aynı ada sahiptir. **Bağlan**'a tıklayın. Sertifika kullanımına ilişkin bir açılır ileti görüntülenebilir. Böyle bir durumla karşılaşırsanız yükseltilmiş ayrıcalıkları kullanmak için **Devam**'a tıklayın. 
2. **Bağlantı** durum sayfasında **Bağlan**'a tıklayarak bağlantıyı başlatın. Bir **Sertifika Seç** ekranı çıkarsa, gösterilen istemci sertifikasının bağlanmak için kullanmak istediğiniz sertifika olduğunu doğrulayın. Başka bir sertifika gösteriliyorsa, açılan liste okunu kullanarak doğru sertifikayı seçin ve **Tamam**’a tıklayın.
   
    ![Bağlan](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/clientconnect.png "VPN client connection")
3. Artık bağlantı kurabilirsiniz.
   
    ![Kurulan bağlantı](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/connected.png "Connection established")

### <a name="verify-the-vpn-connection"></a>VPN bağlantısını doğrulama
1. VPN bağlantınızın etkin olduğunu doğrulamak için, yükseltilmiş bir komut istemi açın ve *ipconfig/all* komutunu çalıştırın.
2. Sonuçlara bakın. Aldığınız IP adresinin, sanal ağınızı oluştururken belirlediğiniz Noktadan Siteye bağlantı adres aralığı içerisinden bir adres olduğuna dikkat edin. Sonuçlar aşağıdakine benzer olmalıdır:

Örnek:

    PPP adapter VNet1:
        Connection-specific DNS Suffix .:
        Description.....................: VNet1
        Physical Address................:
        DHCP Enabled....................: No
        Autoconfiguration Enabled.......: Yes
        IPv4 Address....................: 192.168.130.2(Preferred)
        Subnet Mask.....................: 255.255.255.255
        Default Gateway.................:
        NetBIOS over Tcpip..............: Enabled

## <a name="next-steps"></a>Sonraki adımlar
Bağlantınız tamamlandıktan sonra sanal ağlarınıza sanal makineler ekleyebilirsiniz. Daha fazla bilgi için bkz. [Sanal Makineler](https://docs.microsoft.com/azure/#pivot=services&panel=Compute).




<!--HONumber=Nov16_HO3-->


