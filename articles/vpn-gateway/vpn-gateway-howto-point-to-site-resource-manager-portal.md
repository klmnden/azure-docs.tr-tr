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
ms.date: 06/27/2017
ms.author: cherylmc
ms.translationtype: Human Translation
ms.sourcegitcommit: 857267f46f6a2d545fc402ebf3a12f21c62ecd21
ms.openlocfilehash: 4045dd501ba70b04cb8ea06901c76072fc009018
ms.contentlocale: tr-tr
ms.lasthandoff: 06/28/2017


---
<a id="configure-a-point-to-site-connection-to-a-vnet-using-the-azure-portal" class="xliff"></a>

# Azure portalı kullanarak bir sanal ağa Noktadan Siteye bir bağlantı yapılandırma

Bu makalede, Azure portalı kullanılarak Resource Manager dağıtım modelinde Noktadan Siteye bağlantı ile sanal ağ oluşturma işlemi gösterilmektedir. Ayrıca aşağıdaki listeden farklı bir seçenek belirtip farklı bir dağıtım aracı veya dağıtım modeli kullanarak da bu yapılandırmayı oluşturabilirsiniz:

> [!div class="op_single_selector"]
> * [Azure portal](vpn-gateway-howto-point-to-site-resource-manager-portal.md)
> * [PowerShell](vpn-gateway-howto-point-to-site-rm-ps.md)
> * [Azure portal (klasik)](vpn-gateway-howto-point-to-site-classic-azure-portal.md)
>
>

Noktadan Siteye (P2S) yapılandırması, ayrı bir istemci bilgisayardan bir sanal ağa yönelik güvenli bağlantı oluşturmanıza olanak sağlar. Sanal ağınıza uzak bir konumdan (örneğin, evden veya bir konferanstan) bağlanmak istediğinizde ya da sanal bir ağa bağlanması gereken yalnızca birkaç istemciniz bulunduğunda Noktadan Siteye bağlantıları kullanışlıdır. P2S VPN bağlantısı, yerel Windows VPN istemcisi kullanılarak istemci bilgisayardan başlatılır. Bağlanan istemciler kimlik doğrulamak için sertifika kullanır. 


![Noktadan Siteye diyagramı](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/point-to-site-connection-diagram.png)

Noktadan Siteye bağlantılar için bir VPN cihazına veya genel kullanıma yönelik bir IP adresine gerek yoktur. P2S'de, VPN bağlantısı SSTP (Güvenli Yuva Tünel Protokolü) üzerinden oluşturulur. Sunucu tarafında 1.0, 1.1 ve 1.2 SSTP sürümlerini destekliyoruz. Kullanılacak sürüm, istemci tarafından belirlenir. Windows 8.1 ve sonraki sürümlerinde, SSTP'de varsayılan olarak 1.2 kullanılır. Noktadan Siteye bağlantılar hakkında daha fazla bilgi edinmek için bu makalenin sonunda yer alan [Noktadan Siteye hakkında SSS](#faq) bölümünü inceleyin.

P2S bağlantıları aşağıdakileri gerektirir:

* RouteBased VPN ağ geçidi.
* Azure’a yüklenmiş bir kök sertifikanın ortak anahtarı (.cer dosyası). Bu dosya, güvenilen bir sertifika olarak kabul edilir ve kimlik doğrulaması için kullanılır.
* Kök sertifikadan oluşturulmuş ve bağlanacak her bir istemci bilgisayara yüklenmiş istemci sertifikası. Bu sertifika, istemci kimlik doğrulaması için kullanılır.
* Bir VPN istemcisi yapılandırma paketi oluşturulmalı ve bağlanan her istemci bilgisayara yüklenmelidir. İstemci yapılandırma paketi, işletim sistemi üzerinde zaten bulunan yerel VPN istemcisini sanal ağa bağlanmak için gereken bilgilerle yapılandırır.

### <a name="example"></a>Örnek değerler

Aşağıdaki değerleri kullanarak bir test ortamı oluşturabilir veya bu makaledeki örnekleri daha iyi anlamak için bu değerlere bakabilirsiniz:

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

## <a name="createvnet"></a>1 - Sanal ağ oluşturma

Başlamadan önce, bir Azure aboneliğiniz olduğunu doğrulayın. Henüz Azure aboneliğiniz yoksa [MSDN abonelik avantajlarınızı](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details) etkinleştirebilir veya [ücretsiz bir hesap](https://azure.microsoft.com/pricing/free-trial) için kaydolabilirsiniz. Bu yapılandırmayı bir alıştırma olarak oluşturuyorsanız [örnek değerlere](#example) başvurabilirsiniz.

[!INCLUDE [vpn-gateway-basic-vnet-rm-portal](../../includes/vpn-gateway-basic-vnet-rm-portal-include.md)]

## <a name="address"></a>2 - Adres alanı ve alt ağ belirtme

Ek adres alanı ve alt ağları sanal ağınız oluşturulduktan sonra ekleyebilirsiniz.

[!INCLUDE [vpn-gateway-additional-address-space](../../includes/vpn-gateway-additional-address-space-include.md)]

## <a name="gatewaysubnet"></a>3 - Ağ geçidi alt ağı ekleme

Sanal ağınızı bir ağ geçidine bağlamadan önce, bağlamak istediğiniz sanal ağ için ağ geçidi alt ağını oluşturmanız gerekir. Ağ geçidi hizmetleri, ağ geçidi alt ağında belirtilen IP adreslerini kullanır. Mümkünse, gelecekte eklenecek yapılandırma gereksinimlerine yetecek sayıda IP adresi sağlamak için ağ geçidi alt ağını /28 veya /27 değerine sahip bir CIDR bloğu kullanarak oluşturun.

Bu bölümdeki ekran görüntüleri örnek amaçlıdır. Ağ Geçidi Alt Ağı adres aralığına yapılandırmanız için gerekli olan değerleri girdiğinizden emin olun.

<a id="to-create-a-gateway-subnet" class="xliff"></a>

### Bir ağ geçidi alt ağı oluşturmak için

[!INCLUDE [vpn-gateway-add-gwsubnet-rm-portal](../../includes/vpn-gateway-add-gwsubnet-rm-portal-include.md)]

## <a name="dns"></a>4 - DNS sunucusu belirtme (isteğe bağlı)

Sanal ağınızı oluşturduktan sonra ad çözünürlüğünü işlemek için bir DNS sunucusunun IP adresini ekleyebilirsiniz. Belirtilen DNS sunucusu, bağlandığınız kaynakların adlarını çözümleyebilen bir DNS sunucusu olmalıdır. Daha sonraki bir adımda oluşturacağınız VPN istemcisi yapılandırma paketi, bu ayarda belirttiğiniz DNS sunucularının IP adreslerini içerecektir. Gelecekte DNS sunucularının listesini güncelleştirmeniz gerekirse, yeni listeyi yansıtan yeni VPN istemci yapılandırma paketlerini oluşturup yükleyebilirsiniz.

[!INCLUDE [vpn-gateway-add-dns-rm-portal](../../includes/vpn-gateway-add-dns-rm-portal-include.md)]

## <a name="creategw"></a>5 - Sanal ağ geçidi oluşturma

Noktadan siteye bağlantılar için şu ayarların yapılması gerekir:

* Ağ geçidi türü: VPN
* VPN türü: Rota tabanlı

<a id="to-create-a-virtual-network-gateway" class="xliff"></a>

### Bir sanal ağ geçidi oluşturmak için

[!INCLUDE [create a vnet gateway](../../includes/vpn-gateway-add-gw-rm-portal-include.md)]

## <a name="generatecert"></a>6 - Sertifika oluşturma

Noktadan Siteye VPN’lerde VPN istemcilerinin kimlik doğrulamasını yapmak için Azure tarafından sertifikalar kullanılır. Kök sertifikanın ortak anahtar bilgilerini Azure'a yükleyin. Bundan sonra ortak anahtar, 'güvenilir' olarak kabul edilir. Güvenilir kök sertifikadan istemci sertifikaları oluşturulmalı ve sonra Sertifikalar-Geçerli Kullanıcı/Kişisel sertifika deposundaki her bir istemci bilgisayara yüklenmelidir. Sertifika, sanal ağ ile bağlantı başlattığında istemcinin kimliğini doğrulamak için kullanılır. 

Otomatik olarak imzalanan sertifikalar kullanıyorsanız bu sertifikaların belirli parametreler kullanılarak oluşturulması gerekir. [PowerShell ve Windows 10](vpn-gateway-certificates-point-to-site.md)'a yönelik yönergeler veya [MakeCert](vpn-gateway-certificates-point-to-site-makecert.md) yardımıyla, otomatik olarak imzalanan sertifika oluşturabilirsiniz. Otomatik olarak imzalanan kök sertifikalarla çalışırken ve otomatik olarak imzalanan kök sertifikadan istemci sertifikası oluştururken bu yönergelerdeki adımları uygulamanız önemlidir. Aksi halde, oluşturduğunuz sertifikalar P2S bağlantılarıyla uyumlu olmaz ve bağlantı hatası alırsınız.

### <a name="getcer"></a>1. Adım: Kök sertifikaya ilişkin .cer dosyasını alma

[!INCLUDE [obtain root certificate](../../includes/vpn-gateway-p2s-rootcert-include.md)]

### <a name="generateclientcert"></a>2. Adım - İstemci sertifikası oluşturma

[!INCLUDE [generate client certificate](../../includes/vpn-gateway-p2s-clientcert-include.md)]

## <a name="addresspool"></a>7 - İstemci adres havuzu ekleme

İstemci adres havuzu, belirttiğiniz özel IP adresleri aralığıdır. P2S üzerinden bağlanan istemciler bu aralıktaki bir IP adresini alır. Bağlantıyı kuracağınız şirket içi konum veya bağlanmak istediğiniz sanal ağ ile çakışmayan özel bir IP adresi aralığı kullanın.

1. Sanal ağ geçidini oluşturduktan sonra dikey penceresindeki **Ayarlar** bölümüne gidin. **Ayarlar** bölümünde **Noktadan siteye yapılandırma**’ya tıklayarak **Yapılandırma** dikey penceresini açın.

  ![Noktadan Siteye dikey penceresi](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/configuration.png)
2. Otomatik olarak doldurulan aralığı silebilir, ardından kullanmak istediğiniz özel IP adresi aralığını ekleyebilirsiniz. Ayarı doğrulayıp kaydetmek için **Kaydet**’e tıklayın.

  ![İstemci adres havuzu](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/ipaddresspool.png)

## <a name="uploadfile"></a>8 - Kök sertifika .cer dosyasını karşıya yükleme

Ağ geçidi oluşturulduktan sonra, güvenilen kök sertifikanın .cer dosyasını (ortak anahtar bilgilerini içerir) Azure’a yükleyebilirsiniz. Bir .cer dosyası karşıya yüklendikten sonra Azure, güvenilir kök sertifikadan oluşturulmuş bir istemci sertifikasının yüklü olduğu istemcilerin kimliklerini doğrulamak için bu dosyayı kullanabilir. Daha sonra gerekirse, toplam 20 adede kadar güvenilir kök sertifika dosyasını karşıya yükleyebilirsiniz. 

1. Sertifikalar **Kök sertifikası** bölümünün **Noktadan siteye yapılandırma** dikey penceresine eklenir.  
2. Kök sertifikayı Base-64 ile kodlanmış X.509 (.cer) dosyası olarak dışarı aktardığınızdan emin olun. Sertifikayı metin düzenleyiciyle açabilmek için bu biçimde dışa aktarmanız gerekir.
3. Sertifikayı Not Defteri gibi bir metin düzenleyiciyle açın. Sertifika verilerini kopyalarken, metni satır başları ya da satır besleme karakterleri içermeyen tek ve kesintisiz bir satır şeklinde kopyaladığınızdan emin olun. Satır başlarını ve satır besleme karakterlerini görmek için metin düzenleyicisindeki görünümünüzü 'Sembolü Göster/Tüm karakterleri göster' olarak değiştirmeniz gerekebilir. Yalnızca aşağıdaki bölümü kesintisiz bir satır olarak kopyalayın:

  ![Sertifika verileri](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/copycert.png)
4. Sertifika verilerini **Ortak Sertifika Verileri** alanına yapıştırın. Sertifikayı **Adlandırın** ve **Kaydet**’e tıklayın. En fazla 20 güvenilen kök sertifika ekleyebilirsiniz.

  ![Sertifika yükleme](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/rootcertupload.png)

## <a name="clientconfig"></a>9 - VPN istemcisi yapılandırma paketini yükleme

Noktadan Siteye VPN kullanarak bir sanal ağa bağlanmak için her istemcinin yerel Windows VPN istemcisini yapılandırmaya yönelik bir paket yüklemesi gerekir. Yapılandırma paketi, yerel Windows VPN istemcisini sanal ağa bağlanmak için gereken ayarlarla yapılandırır ve sanal ağınız için bir DNS sunucusu belirttiyseniz, istemcinin ad çözümlemesi için kullanacağı DNS sunucusu IP adresini içerir. Belirtilen DNS sunucusunu daha sonra değiştirirseniz, istemci yapılandırma paketini oluşturduktan sonra istemci bilgisayarlarınıza yüklenecek yeni bir istemci yapılandırma paketi oluşturduğunuzdan emin olun.

Sürümünün istemci mimarisiyle eşleşmesi şartıyla, her istemci bilgisayarda aynı VPN istemcisi yapılandırma paketini kullanabilirsiniz. Desteklenen istemci işletim sistemlerinin listesi için bu makalenin sonundaki [Noktadan Siteye bağlantılar hakkında SSS](#faq) bölümüne bakın.

<a id="step-1---download-the-client-configuration-package" class="xliff"></a>

### 1 Adım - İstemci yapılandırma paketini indirme

1. **Noktadan siteye yapılandırma** dikey penceresinde **VPN istemcisini indir**’e tıklayarak **VPN istemcisini indir** dikey penceresini açın. Paketin oluşturulması birkaç dakika sürer.

  ![VPN istemcisi indirme 1](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/downloadvpnclient1.png)
2. İstemciniz için doğru paketi seçip **İndir**’e tıklayın. Yapılandırma paketi dosyasını kaydedin. VPN istemcisi yapılandırma paketini, sanal ağa bağlanan her istemci bilgisayara yükleyin.

  ![VPN istemcisi indirme 2](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/vpnclient.png)

<a id="step-2---install-the-client-configuration-package" class="xliff"></a>

### 2. Adım: İstemci yapılandırma paketini yükleme

1. Yapılandırma dosyasını, sanal ağınıza bağlamak istediğiniz bilgisayara yerel olarak kopyalayın. 
2. Paketi istemci bilgisayara yüklemek için .exe dosyasına çift tıklayın. Yapılandırma paketini siz oluşturduğunuz için paket imzalanmamıştır ve bir uyarı görebilirsiniz. Bir Windows SmartScreen açılır penceresi görürseniz **Daha fazla bilgi**’ye (solda) ve ardından **Yine de çalıştır**’a tıklayarak paketi yükleyin.
3. Paketi istemci bilgisayara yükleyin. Bir Windows SmartScreen açılır penceresi görürseniz **Daha fazla bilgi**’ye (solda) ve ardından **Yine de çalıştır**’a tıklayarak paketi yükleyin.
4. İstemci bilgisayarda **Ağ Ayarları**’na gidin ve **VPN** öğesine tıklayın. VPN bağlantısı, bağlandığı sanal ağın adını gösterir.

## <a name="installclientcert"></a>10 - İstemci sertifikasını yükleme

İstemci sertifikalarını oluşturmak için kullandığınız bilgisayardan farklı bir istemci bilgisayarda bir P2S bağlantı oluşturmak istiyorsanız, bir istemci sertifikası yüklemeniz gerekir. Bir istemci sertifikası yüklenirken, istemci sertifikası dışarı aktarılırken oluşturulan parola gerekir. Bu işlem genellikle sertifikaya çift tıklayıp sertifikayı yükleme adımlarından oluşur. Daha fazla bilgi için bkz. [Dışarı aktarılan istemci sertifikasını yükleme](vpn-gateway-certificates-point-to-site.md#install).

## <a name="connect"></a>11 - Azure'a bağlanma

1. İstemci bilgisayarda sanal ağınıza bağlanmak için VPN bağlantılarında gezinin ve oluşturduğunuz VPN bağlantısını bulun. Bu VPN bağlantısı sanal ağınızla aynı ada sahiptir. **Bağlan**'a tıklayın. Sertifika kullanımına ilişkin bir açılır ileti görüntülenebilir. Yükseltilmiş ayrıcalıklar kullanmak için **Devam**’a tıklayın.

2. **Bağlantı** durum sayfasında **Bağlan**'a tıklayarak bağlantıyı başlatın. Bir **Sertifika Seç** ekranı çıkarsa, gösterilen istemci sertifikasının bağlanmak için kullanmak istediğiniz sertifika olduğunu doğrulayın. Başka bir sertifika gösteriliyorsa, açılan liste okunu kullanarak doğru sertifikayı seçin ve **Tamam**’a tıklayın.

  ![VPN istemcisinin Azure’a bağlanması](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/clientconnect.png)
3. Bağlantınız kurulur.

  ![Bağlantı kuruldu](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/connected.png)

Bağlanmayla ilgili sorun yaşıyorsanız aşağıdakileri denetleyin:

- **Kullanıcı sertifikalarını yönet** menüsünü açıp **Güvenilen Kök Sertifika Yetkilileri\Sertifikalar**’a gidin. Kök sertifikanın listelenmiş olduğunu doğrulayın. Kimlik doğrulamasının çalışması için kök sertifika mevcut olmalıdır. Bir .pfx istemci sertifikasını varsayılan 'Mümkünse sertifika yolundaki tüm sertifikaları ekle' değerini kullanarak dışarı aktardığınızda, kök sertifika bilgileri de dışarı aktarılır. İstemci sertifikasını yüklediğinizde, kök sertifika da istemci bilgisayara yüklenir. 

- Bir Kuruluş Sertifika Yetkilisi çözümü kullanarak verilen bir sertifika kullanıyor ve kimlik doğrulama sorunu yaşıyorsanız, istemci sertifikasındaki kimlik doğrulama sırasını denetleyin. Kimlik doğrulama listesinin sırasını istemci sertifikasına çift tıklayıp **Ayrıntılar > Gelişmiş Anahtar Kullanımı**’na giderek denetleyebilirsiniz. Listede ilk öğe olarak ‘İstemci Kimlik Doğrulaması’nın göründüğünden emin olun. Aksi takdirde, listedeki ilk öğe olarak İstemci Kimlik Doğrulaması’nı içeren Kullanıcı şablonunu temel alarak oluşturulmuş bir istemci sertifikası vermeniz gerekir.


## <a name="verify"></a>12 - Bağlantınızı doğrulama

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


Bir sanal makineye P2S üzerinden bağlanırken sorun yaşıyorsanız, 'ipconfig' seçeneğini kullanarak bağlantıyı kurduğunuz bilgisayardaki Ethernet bağdaştırıcısına atanmış IPv4 adresini denetleyin. IP adresi bağlanacağınız sanal ağın adres aralığında veya VPNClientAddressPool adres aralığında ise, bu durum çakışan bir adres alanı olarak adlandırılır. Adres alanınız bu şekilde çakıştığında, ağ trafiği Azure’a ulaşmaz ve yerel ağda kalır. Ağ adres alanlarınız çakışmadığı halde sanal makinenize bağlanamıyorsanız, bkz. [VM ile Uzak Masaüstü bağlantıları için sorun giderme](../virtual-machines/windows/troubleshoot-rdp-connection.md).

## <a name="connectVM"></a>Sanal makineye bağlanma

[!INCLUDE [Connect to a VM](../../includes/vpn-gateway-connect-vm-p2s-include.md)]

## <a name="add"></a>Güvenilen kök sertifika ekleme veya kaldırma

Azure’da güvenilen kök sertifikayı ekleyebilir veya kaldırabilirsiniz. Bir kök sertifikayı kaldırdığınızda, o kökten oluşturulmuş bir sertifikaya sahip istemciler kimlik doğrulaması yapamaz ve bu nedenle bağlantı kuramaz. Bir istemcinin kimlik doğrulaması yapmasını ve bağlanmasını istiyorsanız, Azure’da güvenilen (karşıya yüklenmiş) bir kök sertifikadan oluşturulmuş yeni bir istemci sertifikası yüklemeniz gerekir.

<a id="to-add-a-trusted-root-certificate" class="xliff"></a>

### Güvenilen kök sertifika ekleme

Azure'a en fazla 20 güvenilen kök sertifika .cer dosyası ekleyebilirsiniz. Yönergeler için, bu makaledeki [Güvenilen bir kök sertifikayı karşıya yükleme](#uploadfile) bölümüne bakın.

<a id="to-remove-a-trusted-root-certificate" class="xliff"></a>

### Güvenilen kök sertifikayı kaldırmak için

1. Güvenilen kök sertifikayı kaldırmak için, sanal ağ geçidinizin **Noktadan siteye yapılandırma** dikey penceresine gidin.
2. Dikey pencerenin **Kök sertifika** bölümünde, kaldırmak istediğiniz sertifikayı bulun.
3. Sertifikanın yanındaki üç noktaya tıklayın ve ardından ‘Kaldır’a tıklayın.

## <a name="revokeclient"></a>İstemci sertifikasını iptal etme

İstemci sertifikalarını iptal edebilirsiniz. Sertifika iptal listesi sayesinde, ayrı istemci sertifikalarına göre Noktadan Siteye bağlantıyı seçmeli olarak reddedebilirsiniz. Bu, güvenilen kök sertifikayı kaldırma işleminden farklıdır. Azure’dan güvenilen kök sertifika .cer dosyasını kaldırırsanız iptal edilen kök sertifika tarafından oluşturulan/imzalanan tüm istemci sertifikaları reddedilir. Kök sertifika yerine istemci sertifikasını iptal etmek, kök sertifikadan oluşturulan diğer sertifikaların kimlik doğrulaması amacıyla kullanılmaya devam edilmesine olanak sağlar.

Genellikle ekip ve kuruluş düzeylerinde erişimi yönetmek için kök sertifika kullanılırken ayrı kullanıcılar üzerinde ayrıntılı erişim denetimi için iptal edilen istemci sertifikaları kullanılır.

<a id="to-revoke-a-client-certificate" class="xliff"></a>

### İstemci sertifikasını iptal etme

Parmak izini iptal listesine ekleyerek bir istemci sertifikasını iptal edebilirsiniz.

1. İstemci sertifikasının parmak izini alın. Daha fazla bilgi için bkz. [Bir Sertifikanın Parmak İzini alma](https://msdn.microsoft.com/library/ms734695.aspx).
2. Bilgileri bir metin düzenleyicisine kopyalayın ve sürekli bir dize haline getirmek için tüm boşlukları kaldırın.
3. Sanal ağ geçidi **Noktadan siteye yapılandırma** dikey penceresine gidin. Bu, [güvenilen kök sertifika yüklemek](#uploadfile) için kullandığınız dikey pencerenin aynısıdır.
4. **İptal edilen sertifikalar** bölümünde, sertifika için bir kolay ad girin (sertifika genel adından farklı olabilir).
5. Parmak izi dizesini kopyalayın ve **Parmak izi** alanına yapıştırın.
6. Parmak izi doğrulanır ve otomatik olarak iptal listesine eklenir. Ekranda listenin güncelleştirildiğini belirten bir ileti görünür. 
7. Güncelleştirme tamamlandıktan sonra sertifika artık bağlanmak için kullanılamaz. Bu sertifikayı kullanarak bağlanmaya çalışan istemciler sertifikanın artık geçerli olmadığını belirten bir ileti alır.

## <a name="faq"></a>Noktadan Siteye hakkında SSS

[!INCLUDE [Point-to-Site FAQ](../../includes/vpn-gateway-point-to-site-faq-include.md)]

<a id="next-steps" class="xliff"></a>

## Sonraki adımlar
Bağlantınız tamamlandıktan sonra sanal ağlarınıza sanal makineler ekleyebilirsiniz. Daha fazla bilgi için bkz. [Sanal Makineler](https://docs.microsoft.com/azure/#pivot=services&panel=Compute). Ağ ve sanal makineler hakkında daha fazla bilgi edinmek için, bkz. [Azure ve Linux VM ağına genel bakış](../virtual-machines/linux/azure-vm-network-overview.md).

