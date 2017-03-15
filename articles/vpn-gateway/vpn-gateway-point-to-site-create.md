---
title: "Klasik portalı kullanarak bir Azure Sanal Ağa Noktadan Siteye bir VPN ağ geçidi bağlantısı yapılandırma | Microsoft Belgeleri"
description: "Noktadan Siteye bir VPN ağ geçidi bağlantısı oluşturarak Azure Sanal Ağınıza güvenli bir şekilde bağlanın."
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: carmonm
editor: 
tags: azure-service-management
ms.assetid: 4f5668a5-9b3d-4d60-88bb-5d16524068e0
ms.service: vpn-gateway
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/02/2017
ms.author: cherylmc
translationtype: Human Translation
ms.sourcegitcommit: cea53acc33347b9e6178645f225770936788f807
ms.openlocfilehash: 3ff2dcba568ed7ff83154cb6e1f1861ffb32a0d2
ms.lasthandoff: 03/03/2017


---
# <a name="configure-a-point-to-site-connection-to-a-vnet-using-the-classic-portal"></a>Klasik portalı kullanarak bir Sanal Ağa Noktadan Siteye bir bağlantı yapılandırma
> [!div class="op_single_selector"]
> * [Resource Manager - Azure Portal](vpn-gateway-howto-point-to-site-resource-manager-portal.md)
> * [Resource Manager - PowerShell](vpn-gateway-howto-point-to-site-rm-ps.md)
> * [Klasik - Azure Portal](vpn-gateway-howto-point-to-site-classic-azure-portal.md)
> * [Klasik - Klasik Portal](vpn-gateway-point-to-site-create.md)
> 
> 

Noktadan Siteye (P2S) yapılandırması, ayrı bir istemci bilgisayardan bir sanal ağa yönelik güvenli bağlantı oluşturmanıza olanak sağlar. Sanal ağınıza uzak bir konumdan (örneğin, evden veya bir konferanstan) bağlanmak istediğinizde ya da sanal bir ağa bağlanması gereken yalnızca birkaç istemciniz bulunduğunda P2S bağlantısı kullanışlıdır.

Noktadan Siteye bağlantıların çalışması için bir VPN cihazına veya genel kullanıma yönelik bir IP adresine gerek yoktur. VPN bağlantısı, bağlantının istemci bilgisayardan başlatılmasıyla oluşturulur. Noktadan Siteye bağlantılar hakkında daha fazla bilgi edinmek için bu makalenin sonunda yer alan [Noktadan Siteye hakkında SSS](#faq) bölümünü inceleyin.

Bu makalede klasik portal kullanılarak klasik dağıtım modelinde Noktadan Siteye bağlantı ile sanal ağ oluşturma işlemi adım adım açıklanır.

### <a name="deployment-models-and-methods-for-p2s-connections"></a>P2S bağlantıları için dağıtım modelleri ve yöntemleri
[!INCLUDE [deployment models](../../includes/vpn-gateway-deployment-models-include.md)]

Aşağıdaki tabloda, P2S yapılandırmaları için iki dağıtım modeli ve kullanılabilir dağıtım yöntemleri gösterilmektedir. Yapılandırma adımlarını içeren bir makale olduğunda, bu tablodan makaleye yönelik doğrudan bağlantı oluştururuz.

[!INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-table-point-to-site-include.md)]

## <a name="basic-workflow"></a>Temel iş akışı
![Noktadan Siteye Diyagramı](./media/vpn-gateway-point-to-site-create/p2sclassic.png "point-to-site")

Aşağıda, sanal ağa yönelik güvenli bir Noktadan Siteye bağlantı oluşturma işlemi adım adım açıklanmaktadır. 

Noktadan Siteye bağlantı yapılandırması&4; bölüme ayrılmıştır. Bu bölümlerin her birini hangi sırada yapılandıracağınız önemlidir. Adımları atlamayın ve aşağıdaki sıraya uygun ilerleyin.

* **1. Bölüm** Bir sanal ağ ve VPN ağ geçidi oluşturun.
* **2. Bölüm** Kimlik doğrulaması için kullanılan sertifikaları oluşturun ve karşıya yükleyin.
* **3. Bölüm** İstemci sertifikalarınızı dışarı aktarın ve yükleyin.
* **4. Bölüm** VPN istemcinizi yapılandırın.

## <a name="vnetvpn"></a>1. Bölüm - Sanal ağ ve VPN ağ geçidi oluşturma
### <a name="part-1-create-a-virtual-network"></a>1. Kısım: Sanal ağ oluşturma
1. [Klasik Azure portalında](https://manage.windowsazure.com) oturum açın. Buradaki adımlarda Azure portalı değil, klasik portal kullanılmaktadır. Şu anda Azure portalını kullanarak bir P2S bağlantısı oluşturamazsınız.
2. Ekranın sol alt köşesinde **Yeni**’ye tıklayın. Gezinme bölmesinde **Ağ Hizmetleri**’ne, sonra da **Virtual Network**’a tıklayın. Yapılandırma sihirbazını başlatmak için **Özel Oluştur**’a tıklayın.
3. **Sanal Ağ Ayrıntıları** sayfasında aşağıdaki bilgileri girin ve sağ alt köşedeki ileri okuna tıklayın.
   
   * **Ad**: Sanal ağınıza bir ad verin. Örneğin, "VNet1". Bu, bu VNet'e VM dağıtırken başvuracağınız addır.
   * **Konum**: Konum, kaynaklarınızın (VM’ler) bulunmasını istediğiniz fiziksel konum (bölge) ile doğrudan ilişkilidir. Örneğin, bu sanal ağa dağıttığınız VM’lerin fiziksel olarak Doğu ABD’de bulunmasını istiyorsanız, o konumu seçin. Sanal ağınızı oluşturduktan sonra sanal ağınızla ilişkili bölgeyi değiştiremezsiniz.
4. **DNS Sunucuları ve VPN Bağlantısı** sayfasında aşağıdaki bilgileri girin ve sağ alt köşedeki ileri okuna tıklayın.
   
   * **DNS Sunucuları**: DNS sunucusunun adını ve IP adresini girin veya kısayol menüsünden, önceden kaydedilmiş bir DNS sunucusu seçin. Bu ayarla bir DNS sunucusu oluşturulmaz. Söz konusu ayar, bu sanal ağa ilişkin ad çözümlemesi için kullanmak istediğiniz DNS sunucularını belirtmenize olanak sağlar. Azure varsayılan ad çözümleme hizmetini kullanmak istiyorsanız bu bölümü boş bırakın.
   * **Noktadan Siteye VPN’yi Yapılandır**: Onay kutusunu işaretleyin.
5. **Noktadan Siteye Bağlantı** sayfasında, VPN istemcilerinizin bağlandıklarında bir IP adresi alacağı IP adresi aralığını belirtin. Belirleyebileceğiniz adres aralıklarına ilişkin bazı kurallar vardır. Belirttiğiniz aralığın, şirket içi ağınızda yer alan aralıkların hiçbiriyle çakışmadığını doğrulamanız önemlidir.
6. Aşağıdaki bilgileri girin ve ileri okuna tıklayın.
   
   * **Adres Alanı**: Başlangıç IP’sini ve CIDR’yi (Adres Sayısı) ekleyin.
   * **Adres alanı ekle**: Yalnızca ağ tasarımınız için gerekiyorsa adres alanı ekleyin.
7. **Sanal Ağ Adres Alanları** sayfasında sanal ağınız için kullanmak istediğiniz adres aralığını belirtin. Belirlediğiniz adresler, bu sanal ağa dağıtacağınız VM’ler ve diğer rol örneklerine atanacak olan dinamik IP adresleridir (DIPS).<br><br>Şirket içi ağınız için kullanılan aralıklardan herhangi biriyle çakışmayan bir aralık seçmeniz çok önemlidir. Bu konuda ağ yöneticinizle birlikte çalışmanız gerekir. Ağ yöneticinizin, sanal ağınızda kullanabilmeniz için şirket içi ağ adresi alanından bir IP adresi aralığı ayırması gerekebilir.
8. Aşağıdaki bilgileri girin ve onay işaretine tıklayarak sanal ağınızı oluşturmaya başlayın.
   
   * **Adres Alanı**: Başlangıç IP’si ve Sayı dahil olmak üzere, bu sanal ağ için kullanmak istediğiniz iç IP adresi aralığını ekleyin. Şirket içi ağınız için kullanılan aralıklardan herhangi biriyle çakışmayan bir aralık seçmeniz önemlidir. 
   * **Alt ağ ekle**: Ek alt ağlar şart değildir, ancak statik DIPS’leri olacak VM’ler için ayrı bir alt ağ oluşturmak isteyebilirsiniz. Ayrıca VM’lerinizi diğer rol örneklerinizden ayrı bir alt ağda tutmak da isteyebilirsiniz.
   * **Ağ geçidi alt ağı ekle**: Noktadan Siteye VPN için ağ geçidi alt ağı gereklidir. Ağ geçidi alt ağı eklemek için tıklayın. Ağ geçidi alt ağı, yalnızca sanal ağ geçidi için kullanılır.
9. Sanal ağınız oluşturulduğunda, klasik Azure portalının ağlar sayfasındaki **Durum** seçeneğinin altında **Oluşturuldu** ifadesinin yer aldığını görürsünüz. Sanal ağınız oluşturulduktan sonra, dinamik yönlendirme ağ geçidinizi oluşturabilirsiniz.

### <a name="part-2-create-a-dynamic-routing-gateway"></a>2. Kısım: Dinamik yönlendirme ağ geçidi oluşturma
Ağ geçidi türü, dinamik şeklinde yapılandırılmalıdır. Statik yönlendirme ağ geçitleri bu özellikle kullanılamaz.

1. Klasik Azure portalındaki **Ağlar** sayfasında, oluşturduğunuz sanal ağa tıklayın ve **Pano** sayfasına gidin.
2. **Pano** sayfasının en altında yer alan **Ağ Geçidi Oluştur**’a tıklayın. **"VNet1" sanal ağınız için bir ağ geçidi oluşturmak ister misiniz?** şeklinde bir ileti göreceksiniz. Ağ geçidini oluşturmaya başlamak için **Evet**’e tıklayın. Ağ geçidinin oluşturulması 45 dakikayı bulabilir.

## <a name="generate"></a>2. Bölüm - Sertifika oluşturma ve karşıya yükleme
Noktadan Siteye VPN’lerde VPN istemcilerinin kimlik doğrulamasını yapmak için sertifikalar kullanılır. Kurumsal bir sertifika çözümü tarafından oluşturulan bir kök sertifika veya otomatik olarak imzalanan sertifika kullanabilirsiniz. Azure’a en çok 20 kök sertifikası yükleyebilirsiniz. .cer dosyası karşıya yüklendiğinde Azure, bu dosyada yer alan bilgileri kullanarak istemci sertifikası yüklü olan istemcilerin kimliklerini doğrulayabilir. İstemci sertifikası, .cer dosyasının temsil ettiği aynı sertifikadan oluşturulmuş olmalıdır.

Bu bölümde şunları yapacaksınız:

* Bir kök sertifikaya ilişkin .cer dosyasını alma. Bu, otomatik olarak imzalanan sertifika olabileceği gibi kendi kurumsal sertifika sisteminizi de kullanabilirsiniz.
* .cer dosyasını Azure'a yükleme.
* İstemci sertifikalarını oluşturma.

### <a name="root"></a>1. Kısım: Kök sertifikaya ilişkin .cer dosyasını alma
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

### <a name="upload"></a>2. Kısım: Kök sertifika .cer dosyasını klasik Azure portalına yükleme
Azure'a güvenilen bir sertifika ekleyin. Azure'a Base64 ile kodlanmış bir X.509 (*.cer) dosyası eklediğinizde, Azure'a dosyanın temsil ettiği kök sertifikaya güvenebileceğini söylemiş olursunuz.

1. Klasik Azure Portalı’nda, sanal ağınıza ait **Sertifikalar** sayfasında **Upload a root certificate** (Kök sertifikası yükle) seçeneğine tıklayın.
2. **Upload Certificate** (Sertifika Yükle) sayfasında .cer kök sertifikasını bulun ve onay işaretine tıklayın.

### <a name="createclientcert"></a>3. Kısım: İstemci sertifikası oluşturma
Bir sonraki adımda istemci sertifikalarını oluşturacaksınız. Bağlanacak her istemci için benzersiz bir sertifika oluşturabileceğiniz gibi, birden çok istemcide aynı sertifikayı da kullanabilirsiniz. Benzersiz istemci sertifikaları oluşturmanın avantajı, gerektiğinde tek bir sertifikayı iptal edebiliyor olmanızdır. Herkesin aynı istemci sertifikasını kullandığı bir durumda bir istemcinin sertifikasını iptal etmeniz gerektiğinde, kimlik doğrulaması için söz konusu sertifikayı kullanan tüm istemciler için yeni sertifikalar oluşturmanız ve yüklemeniz gerekir.

####<a name="enterprise-certificate"></a>Kurumsal sertifika
- Kurumsal bir sertifika çözümü kullanıyorsanız, 'etkialaniadi\kullaniciadi' biçimini kullanmak yerine, yaygın olarak kullanılan 'name@yourdomain.com' ad değer biçimiyle bir istemci sertifikası oluşturun.
- Verdiğiniz istemci sertifikasının, kullanım listesindeki ilk öğe olarak Akıllı Kart Oturumu, vb. yerine ‘İstemci Kimlik Doğrulaması’na sahip ‘Kullanıcı’ sertifikası şablonunu temel alarak hazırlandığından emin olun. İstemci sertifikasına sağ tıklayıp **Ayrıntılar > Gelişmiş Anahtar Kullanımı**’nı görüntüleyerek sertifikayı denetleyebilirsiniz.

####<a name="self-signed-certificate"></a>Otomatik olarak imzalanan sertifika 
Otomatik olarak imzalanan sertifika kullanıyorsanız istemci sertifikası oluşturmak için bkz. [Noktadan Siteye yapılandırmaları için otomatik olarak imzalanan kök sertifikalar ile çalışma](vpn-gateway-certificates-point-to-site.md).

## <a name="installclientcert"></a>3. Bölüm - İstemci sertifikasını dışarı aktarma ve yükleme
Sanal ağa bağlamak istediğiniz her bilgisayara bir istemci sertifikası yükleyin. Kimlik doğrulaması için istemci sertifikası gereklidir. İstemci sertifikası yükleme işlemini otomatik hale getirebilir veya elle yapabilirsiniz. Aşağıda, istemci sertifikasını dışarı aktarma ve elle yükleme işlemleri adım adım açıklanmıştır.

1. İstemci sertifikasını dışarı aktarmak için *certmgr.msc* kullanabilirsiniz. Dışarı aktarmak istediğiniz istemci sertifikasına sağ tıklayın, **tüm görevler**’e ve ardından **dışarı aktar**’a tıklayın.
2. Özel anahtara sahip istemci sertifikasını dışarı aktarın. Bu bir *.pfx* dosyasıdır. Bu sertifika için ayarladığınız parolayı (anahtar) kaydettiğinizden ya da unutmayacağınızdan emin olun.
3. *.pfx* dosyasını istemci bilgisayara kopyalayın. İstemci bilgisayarda *.pfx* dosyasına çift tıklayarak yükleme işlemini gerçekleştirin. İstendiğinde parolayı girin. Yükleme konumunu değiştirmeyin.

## <a name="vpnclientconfig"></a>4. Bölüm - VPN istemcinizi yapılandırma
Sanal ağa bağlanmak için bir VPN istemcisi de yapılandırmanız gerekir. İstemcinin bağlanması için bir istemci sertifikasına ve uygun VPN istemcisi yapılandırmasına gerek vardır. Bir VPN istemcisi yapılandırmak için aşağıdaki adımları sırayla uygulayın.

### <a name="part-1-create-the-vpn-client-configuration-package"></a>1. Kısım: VPN istemcisi yapılandırma paketini oluşturma
1. Klasik Azure portalında, sanal ağınıza ilişkin **Pano** sayfasında sağ üst köşedeki hızlı bakış menüsüne gidin. VPN istemci paketi, Windows'da yerleşik bulunan VPN istemci yazılımını yapılandırmaya yönelik yapılandırma bilgileri içerir. Paket, ek yazılım yüklemez. Ayarlar bağlanmak istediğiniz sanal ağa özeldir. Desteklenen istemci işletim sistemlerinin listesi için bu makalenin sonundaki [Noktadan Siteye bağlantılar hakkında SSS](#faq) bölümüne bakın.<br><br>Yükleneceği istemci işletim sistemine karşılık gelen indirme paketini seçin:
   
   * 32 bit istemciler için **32 bit İstemci VPN Paketini İndir**’i seçin.
   * 64 bit istemciler için **64 bit İstemci VPN Paketini İndir**’i seçin.
2. İstemci paketinizin oluşturulması birkaç dakika sürer. Paket tamamlandığında dosyayı indirebilirsiniz. İndirdiğiniz *.exe* dosyası yerel bilgisayarınızda güvenle depolanabilir.
3. Klasik Azure Portalı’nda VPN istemci paketini oluşturup indirdikten sonra, sanal ağınıza bağlanmak istediğiniz istemci bilgisayara istemci paketini yükleyebilirsiniz. VPN istemci paketini birden çok istemci bilgisayara yüklemeyi planlıyorsanız, bu bilgisayarların her birine bir istemci sertifikası da yüklü olduğundan emin olun.

### <a name="part-2-install-the-vpn-configuration-package-on-the-client"></a>Bölüm 2: VPN yapılandırma paketini istemciye yükleme
1. Yapılandırma dosyasını, sanal ağınıza bağlamak istediğiniz bilgisayara yerel olarak kopyalayın ve .exe dosyasına çift tıklayın. 
2. Paket yüklendikten sonra VPN bağlantısını başlatabilirsiniz. Yapılandırma paketi Microsoft tarafından imzalanmamıştır. Paketi, kuruluşunuzun imzalama hizmetini kullanarak veya kendiniz [SignTool](http://go.microsoft.com/fwlink/p/?LinkId=699327) ile imzalamak isteyebilirsiniz. Paketin imzasız olarak kullanılmasında bir sorun yoktur. Ancak paket imzasızsa, yüklendiğinde bir uyarı görüntülenir.
3. İstemci bilgisayarda **Ağ Ayarları**’na gidin ve **VPN** öğesine tıklayın. Bağlantının listelendiğini görürsünüz. Bu listede bağlantı kurulacak sanal ağın adı gösterilir ve liste aşağıdaki gibi görünür: 
   
    ![VPN istemcisi](./media/vpn-gateway-point-to-site-create/vpn.png "VPN client")

### <a name="part-3-connect-to-azure"></a>Bölüm 3 - Azure'a Bağlanma
1. İstemci bilgisayarda VNet'inize bağlanmak için VPN bağlantılarında gezinin ve oluşturduğunuz VPN bağlantısını bulun. Bu VPN bağlantısı sanal ağınızla aynı ada sahiptir. **Bağlan**'a tıklayın. Sertifika kullanımına ilişkin bir açılır ileti görüntülenebilir. Böyle bir durumla karşılaşırsanız yükseltilmiş ayrıcalıkları kullanmak için **Devam**'a tıklayın. 
2. **Bağlantı** durum sayfasında **Bağlan**'a tıklayarak bağlantıyı başlatın. Bir **Sertifika Seç** ekranı çıkarsa, gösterilen istemci sertifikasının bağlanmak için kullanmak istediğiniz sertifika olduğunu doğrulayın. Başka bir sertifika gösteriliyorsa, açılan liste okunu kullanarak doğru sertifikayı seçin ve **Tamam**’a tıklayın.
   
    ![VPN istemcisi 2](./media/vpn-gateway-point-to-site-create/clientconnect.png "VPN client connection")
3. Artık bağlantı kurabilirsiniz.
   
    ![VPN istemcisi 3](./media/vpn-gateway-point-to-site-create/connected.png "VPN client connection 2")

> [!NOTE]
> Bir Kuruluş Sertifika Yetkilisi çözümü kullanarak verilen bir sertifika kullanıyor ve kimlik doğrulama sorunu yaşıyorsanız, istemci sertifikasındaki kimlik doğrulama sırasını denetleyin. Kimlik doğrulama listesinin sırasını istemci sertifikasına çift tıklayıp **Ayrıntılar > Gelişmiş Anahtar Kullanımı**’na giderek denetleyebilirsiniz. Listede ilk öğe olarak ‘İstemci Kimlik Doğrulaması’nın göründüğünden emin olun. Aksi takdirde, listedeki ilk öğe olarak İstemci Kimlik Doğrulaması’nı içeren Kullanıcı şablonunu temel alarak oluşturulmuş bir istemci sertifikası vermeniz gerekir. 
>
>

### <a name="part-4-verify-the-vpn-connection"></a>Bölüm 4: VPN bağlantısını doğrulama
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

## <a name="faq"></a>Noktadan Siteye hakkında SSS

[!INCLUDE [Point-to-Site FAQ](../../includes/vpn-gateway-point-to-site-faq-include.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bağlantınız tamamlandıktan sonra sanal ağlarınıza sanal makineler ekleyebilirsiniz. Daha fazla bilgi için bkz. [Sanal Makineler](https://docs.microsoft.com/azure/#pivot=services&panel=Compute).

Sanal Ağlar hakkında daha fazla bilgi edinmek için [Sanal Ağ Belgeleri](/azure/virtual-network) sayfasına bakın.


