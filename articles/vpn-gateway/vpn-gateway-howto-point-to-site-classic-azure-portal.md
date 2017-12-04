---
title: "Noktadan Siteye bağlantısı ve istemci kimlik doğrulaması kullanarak bir bilgisayarı sanal ağa bağlama: Klasik Azure portalı | Microsoft Docs"
description: "Azure portalı ile Noktadan Siteye bir VPN Gateway bağlantısı oluşturarak klasik Azure Sanal Ağınıza güvenli bir şekilde bağlanın."
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 65e14579-86cf-4d29-a6ac-547ccbd743bd
ms.service: vpn-gateway
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/12/2017
ms.author: cherylmc
ms.openlocfilehash: 00a9e580a324ded8e979c2a3c58d51319091b628
ms.sourcegitcommit: cfd1ea99922329b3d5fab26b71ca2882df33f6c2
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/30/2017
---
# <a name="configure-a-point-to-site-connection-to-a-vnet-using-certificate-authentication-classic-azure-portal"></a>Sertifika kimlik doğrulaması kullanarak Noktadan Siteye VNet bağlantısını yapılandırma (klasik): Azure portalı

[!INCLUDE [deployment models](../../includes/vpn-gateway-classic-deployment-model-include.md)]

Bu makalede, Azure portalı kullanılarak klasik dağıtım modelinde Noktadan Siteye bağlantı ile sanal ağ oluşturma işlemi gösterilmektedir. Bu yapılandırma, bağlanan istemcinin kimliğini doğrulamak için sertifikaları kullanır. Ayrıca aşağıdaki listeden farklı bir seçenek belirtip farklı bir dağıtım aracı veya dağıtım modeli kullanarak da bu yapılandırmayı oluşturabilirsiniz:

> [!div class="op_single_selector"]
> * [Azure portal](vpn-gateway-howto-point-to-site-resource-manager-portal.md)
> * [PowerShell](vpn-gateway-howto-point-to-site-rm-ps.md)
> * [Azure portal (klasik)](vpn-gateway-howto-point-to-site-classic-azure-portal.md)
>

Noktadan Siteye (P2S) VPN ağ geçidi, ayrı bir istemci bilgisayardan sanal ağınıza güvenli bir bağlantı oluşturmanıza olanak sağlar. Noktadan Siteye VPN bağlantıları, ev veya bir konferans gibi uzak bir noktadan Vnet'inize bağlanmak istediğinizde faydalıdır. P2S VPN ayrıca, bir sanal ağa bağlanması gereken yalnızca birkaç istemciniz olduğunda Siteden Siteye VPN yerine kullanabileceğiniz yararlı bir çözümüdür. P2S VPN bağlantısı, istemci bilgisayardan başlatılmasıyla oluşturulur.

> [!IMPORTANT]
> Klasik dağıtım modeli yalnızca Windows VPN istemcilerini destekler ve SSL tabanlı VPN protokolü olan Güvenli Yuva Tünel Protokolünü (SSTP) kullanır. Windows harici VPN istemcilerini desteklemek için sanal ağınızın Resource Manager dağıtım modeliyle oluşturulması gerekir. Resource Manager dağıtım modeli SSTP’ye ek olarak IKEv2 VPN desteği sunar. Daha fazla bilgi için bkz. [P2S bağlantıları hakkında](point-to-site-about.md).
>
>

![Noktadan Siteye diyagramı](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/point-to-site-connection-diagram.png)


Noktadan Siteye sertifika kimlik doğrulaması bağlantıları aşağıdakileri gerektirir:

* Bir Dinamik VPN ağ geçidi.
* Azure’a yüklenmiş bir kök sertifikanın ortak anahtarı (.cer dosyası). Bu dosya, güvenilen bir sertifika olarak kabul edilir ve kimlik doğrulaması için kullanılır.
* Kök sertifikadan oluşturulmuş ve bağlanacak her bir istemci bilgisayara yüklenmiş istemci sertifikası. Bu sertifika, istemci kimlik doğrulaması için kullanılır.
* Bir VPN istemcisi yapılandırma paketi oluşturulmalı ve bağlanan her istemci bilgisayara yüklenmelidir. İstemci yapılandırma paketi, işletim sistemi üzerinde zaten bulunan yerel VPN istemcisini sanal ağa bağlanmak için gereken bilgilerle yapılandırır.

Noktadan Siteye bağlantılar için bir VPN cihazına veya şirket içi genel kullanıma yönelik bir IP adresine gerek yoktur. VPN bağlantısı SSTP (Güvenli Yuva Tünel Protokolü) üzerinden oluşturulur. Sunucu tarafında 1.0, 1.1 ve 1.2 SSTP sürümlerini destekliyoruz. Kullanılacak sürüm, istemci tarafından belirlenir. Windows 8.1 ve sonraki sürümlerinde, SSTP'de varsayılan olarak 1.2 kullanılır. 

Noktadan Siteye bağlantılar hakkında daha fazla bilgi edinmek için bu makalenin sonunda yer alan [Noktadan Siteye hakkında SSS](#faq) bölümünü inceleyin.

### <a name="example-settings"></a>Örnek ayarlar

Aşağıdaki değerleri kullanarak bir test ortamı oluşturabilir veya bu makaledeki örnekleri daha iyi anlamak için bu değerlere bakabilirsiniz:

* **Ad: VNet1**
* **Adres alanı: 192.168.0.0/16**<br>Bu örnekte yalnızca bir adres alanı kullanılmaktadır. Sanal ağınıza ait birden fazla adres alanı olabilir.
* **Alt ağ adı: FrontEnd**
* **Alt ağ adres aralığı: 192.168.1.0/24**
* **Abonelik:** Birden fazla aboneliğiniz varsa doğru aboneliği kullandığınızdan emin olun.
* **Kaynak Grubu: TestRG**
* **Konum: Doğu ABD**
* **Bağlantı türü: Noktadan siteye**
* **İstemci Adres Alanı: 172.16.201.0/24**. Sanal ağa bu Noktadan Siteye bağlantıyı kullanarak bağlanan VPN istemcileri belirtilen havuzdan bir IP adresi alır.
* **GatewaySubnet: 192.168.200.0/24**. Ağ geçidi alt ağı 'GatewaySubnet' adını kullanmalıdır.
* **Boyut:** Kullanmak istediğiniz ağ geçidi SKU’sunu seçin.
* **Yönlendirme Türü: Dinamik**

## <a name="vnetvpn"></a>1. Sanal ağ ve VPN ağ geçidi oluşturma

Başlamadan önce, bir Azure aboneliğiniz olduğunu doğrulayın. Henüz Azure aboneliğiniz yoksa [MSDN abonelik avantajlarınızı](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details) etkinleştirebilir veya [ücretsiz bir hesap](https://azure.microsoft.com/pricing/free-trial) için kaydolabilirsiniz.

### <a name="createvnet"></a>1. Kısım - Sanal ağ oluşturma

Sanal ağınız yoksa bir sanal ağ oluşturun. Ekran görüntüleri örnek olarak verilmiştir. Değerlerin kendinizinkilerle değiştirildiğinden emin olun. Azure portalını kullanarak sanal ağ oluşturmak için şu adımları uygulayın:

1. Tarayıcıdan [Azure portalına](http://portal.azure.com) gidin ve gerekiyorsa Azure hesabınızda oturum açın.
2. **Yeni**’ye tıklayın. **Markette ara** alanına 'Sanal Ağ' yazın. Döndürülen listeden **Sanal Ağ**’ı bulun ve tıklayarak **Sanal Ağ** sayfasını açın.

  ![Sanal ağ ara sayfası](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/newvnetportal700.png)
3. Sanal Ağ sayfasının en altına doğru, **Bir dağıtım modeli seçin** listesinden **Klasik**’i seçip **Oluştur**’a tıklayın.

  ![Dağıtım modeli seçme](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/selectmodel.png)
4. **Sanal ağ oluştur** sayfasında sanal ağ ayarlarını yapılandırın. Bu sayfada, ilk adres alanınızı ve tek alt ağ adres aralığınızı eklersiniz. Sanal ağ oluşturma işlemini tamamladıktan sonra geri dönüp ek alt ağları ve adres alanlarını ekleyin.

  ![Sanal ağ oluştur sayfası](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/vnet125.png)
5. **Abonelik** alanında doğru bir giriş olduğunu doğrulayın. Açılan listeyi kullanarak abonelikleri değiştirebilirsiniz.
6. **Kaynak grubu**’na tıklayın, ya varolan bir kaynak grubunu seçin ya da yeni kaynak grubunuz için bir ad yazarak yeni bir tane oluşturun. Yeni bir kaynak grubu oluşturuyorsanız, planlanan yapılandırma değerlerinize göre kaynak grubunu adlandırın. Kaynak grupları hakkında daha fazla bilgi için [Azure Resource Manager’a Genel Bakış](../azure-resource-manager/resource-group-overview.md#resource-groups)’ı ziyaret edin.
7. Ardından, sanal ağınız için **Konum** ayarlarını seçin. Konum bu sanal ağa dağıttığınız kaynakların nerede olacağını belirler.
8. VNet’inizi panoda kolay bulmak istiyorsanız **Panoya sabitle**’yi seçin ve ardından **Oluştur**’a tıklayın.

  ![Panoya sabitle](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/pintodashboard150.png)
9. Oluştur’a tıkladıktan sonra, panonuzda sanal ağınızın ilerleme durumunu yansıtacak bir kutucuk görünür. Sanal ağ oluşturulurken kutucuk değişir.

  ![Sanal ağ kutucuğu oluşturma](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/deploying150.png)
10. Sanal ağınız oluşturulduktan sonra **Oluşturuldu**'ya bakın.
11. DNS sunucusu ekleme (isteğe bağlı). Sanal ağınızı oluşturduktan sonra ad çözümlemesi için bir DNS sunucusunun IP adresini ekleyebilirsiniz. Belirttiğiniz DNS sunucusu IP adresi, sanal ağınızdaki kaynakların adlarını çözümleyebilen bir DNS sunucusunun adresi olmalıdır.<br>Bir DNS sunucusu eklemek için sanal ağınızın ayarlarını açın, DNS sunucularına tıklayın ve kullanmak istediğiniz DNS sunucusunun IP adresini ekleyin.

### <a name="gateway"></a>2. Kısım: Ağ geçidi alt ağı ve dinamik yönlendirme ağ geçidi oluşturma

Bu adımda bir ağ geçidi alt ağı ve dinamik yönlendirme ağ geçidi oluşturacaksınız. Klasik dağıtım modeli için Azure portalında aynı ağ geçidi alt ağı ve ağ geçidi, aynı yapılandırma sayfalarından oluşturulabilir.

1. Portalda, ağ geçidi oluşturmak istediğiniz sanal ağa gidin.
2. Sanal ağınızın sayfasındaki **Genel Bakış** sayfasının VPN bağlantıları bölümünde **Ağ Geçidi**’ne tıklayın.

  ![Ağ geçidi oluşturmak için tıklayın](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/beforegw125.png)
3. **Yeni VPN Bağlantısı** sayfasında **Noktadan siteye** öğesini seçin.

  ![Noktadan Siteye bağlantı türü](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/newvpnconnect.png)
4. **İstemci Adres Alanı** için IP adresi aralığını ekleyin. Bu aralık, VPN istemcilerinin bağlanırken bir IP adresi aldığı aralıktır. Bağlantıyı kuracağınız şirket içi konum veya bağlanmak istediğiniz sanal ağ ile çakışmayan özel bir IP adresi aralığı kullanın. Otomatik olarak doldurulan aralığı silebilir, ardından kullanmak istediğiniz özel IP adresi aralığını ekleyebilirsiniz.

  ![İstemci adres alanı](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/clientaddress.png)
5. **Ağ geçidini hemen oluştur** onay kutusunu seçin.

  ![Ağ geçidini hemen oluşturma](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/creategwimm.png)
6. **İsteğe bağlı ağ geçidi yapılandırması**’na tıklayarak **Ağ geçidi yapılandırması** sayfasını açın.

  ![İsteğe bağlı ağ geçidi yapılandırması'na tıklayın](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/optsubnet125.png)
7. **Alt Ağ Yapılandırma zorunlu ayarları**’na tıklayarak **ağ geçidi alt ağı** ekleyin. /29 kadar küçük bir ağ geçidi alt ağı oluşturmak mümkün olsa da en az /28 veya /27’yi seçerek daha fazla adres içeren büyük bir alt ağ oluşturmanızı öneririz. Bu, gelecekte isteyebileceğiniz ek yapılandırmaları da içerecek yeteri kadar adres sağlayacaktır. Ağ geçidi alt ağlarıyla çalışırken, ağ güvenlik grubunu (NSG) ağ geçidi alt ağıyla ilişkilendirmekten kaçının. Ağ güvenlik grubunun bu alt ağ ile ilişkilendirilmesi, VPN Gateway’inizin beklendiği gibi çalışmayı durdurmasına neden olabilir.

  ![GatewaySubnet ekleme](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/gwsubnet125.png)
8. Ağ geçidi **Boyutu** seçin. Boyut, sanal ağ geçidinizin ağ geçidi SKU’sudur. Portalda Varsayılan SKU, **Temel**’dir. Ağ geçidi SKU’ları hakkında bilgi için bkz. [VPN Gateway Ayarları Hakkında](vpn-gateway-about-vpn-gateway-settings.md#gwsku).

  ![Ağ geçidi boyutu](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/gwsize125.png)
9. Ağ geçidiniz için **Yönlendirme Türü** seçin. P2S yapılandırmaları bir **Dinamik** yönlendirme türü gerektirir. Bu sayfayı yapılandırmayı bitirdiğinizde **Tamam**’a tıklayın.

  ![Yönlendirme türünü yapılandırma](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/routingtype125.png)
10. **Yeni VPN Bağlantısı** sayfasında, sanal ağ geçidinizi oluşturmaya başlamak için sayfanın en altından **Tamam**’a tıklayın. Bir VPN ağ geçidi işleminin tamamlanması, seçtiğiniz ağ geçidi sku'suna bağlı olarak 45 dakikaya kadar sürebilir.

## <a name="generatecerts"></a>2. Sertifika oluşturma

Noktadan Siteye VPN’lerde VPN istemcilerinin kimlik doğrulamasını yapmak için Azure tarafından sertifikalar kullanılır. Kök sertifikanın ortak anahtar bilgilerini Azure'a yükleyin. Bundan sonra ortak anahtar, 'güvenilir' olarak kabul edilir. Güvenilir kök sertifikadan istemci sertifikaları oluşturulmalı ve sonra Sertifikalar-Geçerli Kullanıcı/Kişisel sertifika deposundaki her bir istemci bilgisayara yüklenmelidir. Sertifika, sanal ağ ile bağlantı başlattığında istemcinin kimliğini doğrulamak için kullanılır. 

Otomatik olarak imzalanan sertifikalar kullanıyorsanız bu sertifikaların belirli parametreler kullanılarak oluşturulması gerekir. [PowerShell ve Windows 10](vpn-gateway-certificates-point-to-site.md)'a yönelik yönergeler veya [MakeCert](vpn-gateway-certificates-point-to-site-makecert.md) yardımıyla, otomatik olarak imzalanan sertifika oluşturabilirsiniz. Otomatik olarak imzalanan kök sertifikalarla çalışırken ve otomatik olarak imzalanan kök sertifikadan istemci sertifikası oluştururken bu yönergelerdeki adımları uygulamanız önemlidir. Aksi halde, oluşturduğunuz sertifikalar P2S bağlantılarıyla uyumlu olmaz ve bağlantı hatası alırsınız.

### <a name="cer"></a>1. Kısım: Kök sertifikaya ilişkin ortak anahtarı (.cer) alma

[!INCLUDE [vpn-gateway-basic-vnet-rm-portal](../../includes/vpn-gateway-p2s-rootcert-include.md)]

### <a name="genclientcert"></a>2. Kısım: İstemci sertifikası oluşturma

[!INCLUDE [vpn-gateway-basic-vnet-rm-portal](../../includes/vpn-gateway-p2s-clientcert-include.md)]

## <a name="upload"></a>3. Kök sertifika .cer dosyasını karşıya yükleme

Ağ geçidi oluşturulduktan sonra, güvenilen kök sertifikanın .cer dosyasını (ortak anahtar bilgilerini içerir) Azure’a yükleyebilirsiniz. Kök sertifikanın özel anahtarını Azure'a yüklemezsiniz. Bir .cer dosyası karşıya yüklendikten sonra Azure, güvenilir kök sertifikadan oluşturulmuş bir istemci sertifikasının yüklü olduğu istemcilerin kimliklerini doğrulamak için bu dosyayı kullanabilir. Daha sonra gerekirse, toplam 20 adede kadar güvenilir kök sertifika dosyasını karşıya yükleyebilirsiniz.  

1. Sanal ağınıza ait sayfanın **VPN bağlantıları** bölümünde **istemciler** grafiğine tıklayarak **Noktadan siteye VPN bağlantısı** sayfasını açın.

  ![İstemciler](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/clients125.png)
2. **Noktadan siteye bağlantı** sayfasında **Sertifikaları yönet**’e tıklayarak **Sertifikalar** sayfasını açın.<br>

  ![Sertifikalar sayfası](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/ptsmanage.png)<br><br>
3. **Sertifikalar** sayfasında **Karşıya Yükle**’ye tıklayarak **Sertifikayı karşıya yükle** sayfasını açın.<br>

    ![Sertifikaları karşıya yükle sayfası](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/uploadcerts.png)<br>
4. .cer dosyasına göz atmak için klasör grafiğine tıklayın. Dosyayı seçin ve ardından **Tamam**’a tıklayın. **Sertifikalar** sayfasına yüklenen sertifikayı görmek için sayfayı yenileyin.

  ![Sertifikayı karşıya yükleme](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/upload.png)<br>

## <a name="vpnclientconfig"></a>4. İstemciyi yapılandırma

Noktadan Siteye VPN kullanarak bir sanal ağa bağlanmak için her istemcinin yerel Windows VPN istemcisini yapılandırmaya yönelik bir paket yüklemesi gerekir. Yapılandırma paketi, sanal ağa bağlanmak için gereken ayarlarla yerel Windows VPN istemcisini yapılandırır.

Sürümünün istemci mimarisiyle eşleşmesi şartıyla, her istemci bilgisayarda aynı VPN istemcisi yapılandırma paketini kullanabilirsiniz. Desteklenen istemci işletim sistemlerinin listesi için bu makalenin sonundaki [Noktadan Siteye bağlantılar hakkında SSS](#faq) bölümüne bakın.

### <a name="generateconfigpackage"></a>1. Bölüm - VPN istemcisi yapılandırma paketini oluşturma ve yükleme

1. Azure portalında, sanal ağınızın **Genel Bakış** sayfasındaki **VPN bağlantıları** menüsünde istemci grafiğine tıklayarak **Noktadan siteye VPN bağlantısı** sayfasını açın.
2. **Noktadan siteye VPN bağlantısı** sayfasının üst kısmında, yükleneceği istemci işletim sistemine karşılık gelen indirme paketini seçin:

  * 64 bit istemciler için **VPN İstemcisi (64 bit)** seçeneğini belirleyin.
  * 32 bit istemciler için **VPN İstemcisi (32 bit)** seçeneğini belirleyin.

  ![VPN istemcisi yapılandırma paketini indirme](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/dlclient.png)<br>
3. Paket oluşturulduktan sonra istemci bilgisayarınıza indirip yükleyin. Bir SmartScreen açılır penceresi görürseniz **Daha fazla bilgi**’ye ve ardından **Yine de çalıştır**’a tıklayın. Paketi ayrıca diğer istemci bilgisayarlara yüklemek üzere kaydedebilirsiniz.

### <a name="installclientcert"></a>2. Kısım: İstemci sertifikasını yükleme

İstemci sertifikalarını oluşturmak için kullandığınız bilgisayardan farklı bir istemci bilgisayarda bir P2S bağlantı oluşturmak istiyorsanız, bir istemci sertifikası yüklemeniz gerekir. Bir istemci sertifikası yüklenirken, istemci sertifikası dışarı aktarılırken oluşturulan parola gerekir. Bu işlem genellikle sertifikaya çift tıklayıp sertifikayı yükleme adımlarından oluşur. Daha fazla bilgi için bkz. [Dışarı aktarılan istemci sertifikasını yükleme](vpn-gateway-certificates-point-to-site.md#install).

## <a name="connect"></a>5. Azure'a Bağlanma

### <a name="connect-to-your-vnet"></a>Sanal ağınıza bağlanma

1. İstemci bilgisayarda sanal ağınıza bağlanmak için VPN bağlantılarında gezinin ve oluşturduğunuz VPN bağlantısını bulun. Bu VPN bağlantısı sanal ağınızla aynı ada sahiptir. **Bağlan**'a tıklayın. Sertifika kullanımına ilişkin bir açılır ileti görüntülenebilir. Böyle bir durumla karşılaşırsanız yükseltilmiş ayrıcalıkları kullanmak için **Devam**'a tıklayın.
2. **Bağlantı** durum sayfasında **Bağlan**'a tıklayarak bağlantıyı başlatın. Bir **Sertifika Seç** ekranı çıkarsa, gösterilen istemci sertifikasının bağlanmak için kullanmak istediğiniz sertifika olduğunu doğrulayın. Başka bir sertifika gösteriliyorsa, açılan liste okunu kullanarak doğru sertifikayı seçin ve **Tamam**’a tıklayın.

  ![VPN istemci bağlantısı](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/clientconnect.png)
3. Bağlantınız kurulur.

  ![Kurulan bağlantı](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/connected.png)

#### <a name="troubleshooting-p2s-connections"></a>P2S bağlantılarının sorunlarını giderme

[!INCLUDE [verify-client-certificates](../../includes/vpn-gateway-certificates-verify-client-cert-include.md)]

### <a name="verifyvpnconnect"></a>VPN bağlantısını doğrulama

1. VPN bağlantınızın etkin olduğunu doğrulamak için, yükseltilmiş bir komut istemi açın ve *ipconfig/all* komutunu çalıştırın.
2. Sonuçlara bakın. Aldığınız IP adresinin, sanal ağınızı oluştururken belirlediğiniz Noktadan Siteye bağlantı adres aralığı içerisinden bir adres olduğuna dikkat edin. Sonuçlar şu örneğe benzer olmalıdır:

  ```
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
  ```

## <a name="connectVM"></a>Sanal makineye bağlanma

[!INCLUDE [Connect to a VM](../../includes/vpn-gateway-connect-vm-p2s-classic-include.md)]

## <a name="add"></a>Güvenilen kök sertifika ekleme veya kaldırma

Azure’da güvenilen kök sertifikayı ekleyebilir veya kaldırabilirsiniz. Bir kök sertifikayı kaldırdığınızda, o kökten oluşturulmuş bir sertifikaya sahip istemciler kimlik doğrulaması yapamaz ve bu nedenle bağlantı kuramaz. Bir istemcinin kimlik doğrulaması yapmasını ve bağlanmasını istiyorsanız, Azure’da güvenilen (karşıya yüklenmiş) bir kök sertifikadan oluşturulmuş yeni bir istemci sertifikası yüklemeniz gerekir.

### <a name="addtrustedroot"></a>Güvenilen kök sertifika ekleme

Azure'a en fazla 20 güvenilen kök sertifika .cer dosyası ekleyebilirsiniz. Yönergeler için bkz. [3. Bölüm - Kök sertifika .cer dosyasını karşıya yükleme](#upload).

### <a name="removetrustedroot"></a>Güvenilen kök sertifikayı kaldırmak için

1. Sanal ağınıza ait sayfanın **VPN bağlantıları** bölümünde **istemciler** grafiğine tıklayarak **Noktadan siteye VPN bağlantısı** sayfasını açın.

  ![İstemciler](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/clients125.png)
2. **Noktadan siteye bağlantı** sayfasında **Sertifikaları yönet**’e tıklayarak **Sertifikalar** sayfasını açın.<br>

  ![Sertifikalar sayfası](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/ptsmanage.png)<br><br>
3. **Sertifikalar** sayfasında, kaldırmak istediğiniz sertifikanın yanındaki üç noktaya ve ardından **Sil**’e tıklayın.

  ![Kök sertifikayı sil](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/deleteroot.png)<br>

## <a name="revokeclient"></a>İstemci sertifikasını iptal etme

İstemci sertifikalarını iptal edebilirsiniz. Sertifika iptal listesi sayesinde, ayrı istemci sertifikalarına göre Noktadan Siteye bağlantıyı seçmeli olarak reddedebilirsiniz. Bu, güvenilen kök sertifikayı kaldırma işleminden farklıdır. Azure’dan güvenilen kök sertifika .cer dosyasını kaldırırsanız iptal edilen kök sertifika tarafından oluşturulan/imzalanan tüm istemci sertifikaları reddedilir. Kök sertifika yerine istemci sertifikasını iptal etmek, kök sertifikadan oluşturulan diğer sertifikaların Noktadan Siteye bağlantı için kimlik doğrulaması amacıyla kullanılmaya devam edilmesine olanak sağlar.

Genellikle ekip ve kuruluş düzeylerinde erişimi yönetmek için kök sertifika kullanılırken ayrı kullanıcılar üzerinde ayrıntılı erişim denetimi için iptal edilen istemci sertifikaları kullanılır.

### <a name="revokeclientcert"></a>İstemci sertifikasını iptal etmek için

Parmak izini iptal listesine ekleyerek bir istemci sertifikasını iptal edebilirsiniz.

1. İstemci sertifikasının parmak izini alın. Daha fazla bilgi için bkz. [Nasıl yapılır: Bir Sertifikanın Parmak İzini Alma](https://msdn.microsoft.com/library/ms734695.aspx).
2. Bilgileri bir metin düzenleyicisine kopyalayın ve sürekli bir dize haline getirmek için tüm boşlukları kaldırın.
3. **'klasik sanal ağ adı' > Noktadan siteye VPN bağlantısı > Sertifikalar** sayfasına gidin ve ardından İptal listesi sayfasını açmak için **İptal listesi**’ne tıklayın. 
4. **İptal listesi** sayfasında, **İptal listesine sertifika ekle** sayfasını açmak için **+Sertifika ekle**’ye tıklayın.
5. **İptal listesine sertifika ekle** sayfasında, sertifika parmak izini boşluk içermeyen sürekli bir metin satırı olarak yapıştırın. Sayfanın altından **Tamam**’a tıklayın.
6. Güncelleştirme tamamlandıktan sonra sertifika artık bağlanmak için kullanılamaz. Bu sertifikayı kullanarak bağlanmaya çalışan istemciler sertifikanın artık geçerli olmadığını belirten bir ileti alır.

## <a name="faq"></a>Noktadan Siteye hakkında SSS

[!INCLUDE [Point-to-Site FAQ](../../includes/vpn-gateway-faq-point-to-site-include.md)]

## <a name="next-steps"></a>Sonraki adımlar
Bağlantınız tamamlandıktan sonra sanal ağlarınıza sanal makineler ekleyebilirsiniz. Daha fazla bilgi için bkz. [Sanal Makineler](https://docs.microsoft.com/azure/#pivot=services&panel=Compute). Ağ ve sanal makineler hakkında daha fazla bilgi edinmek için, bkz. [Azure ve Linux VM ağına genel bakış](../virtual-machines/linux/azure-vm-network-overview.md).
