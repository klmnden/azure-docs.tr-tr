---
title: 'Bir bilgisayar, bir sanal ağa noktadan siteye ve sertifika kimlik doğrulaması kullanarak bağlanın: Klasik Azure portalı | Microsoft Docs'
description: Azure portalını kullanarak klasik bir Noktadan siteye VPN ağ geçidi bağlantısı oluşturun.
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: jpconnock
editor: ''
tags: azure-service-management
ms.assetid: 65e14579-86cf-4d29-a6ac-547ccbd743bd
ms.service: vpn-gateway
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 12/11/2018
ms.author: cherylmc
ms.openlocfilehash: 74940f3b89237233acd575aa5df441163e00d178
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60845638"
---
# <a name="configure-a-point-to-site-connection-by-using-certificate-authentication-classic"></a>Sertifika kimlik doğrulaması (Klasik) kullanarak noktadan siteye bağlantı yapılandırma

[!INCLUDE [deployment models](../../includes/vpn-gateway-classic-deployment-model-include.md)]

Bu makalede bir noktadan siteye bağlantı ile VNet oluşturma gösterilmektedir. Azure portalını kullanarak Klasik dağıtım modeliyle bu sanal ağ oluşturun. Bu yapılandırma, bağlanan istemcinin kimliğini doğrulamak için otomatik olarak imzalanan veya CA tarafından verilen sertifikaları kullanır. Ayrıca bu yapılandırma bir başka dağıtım aracını veya modeli ile aşağıdaki makalelerde açıklanan seçeneklerini kullanarak oluşturabileceğiniz:

> [!div class="op_single_selector"]
> * [Azure portal](vpn-gateway-howto-point-to-site-resource-manager-portal.md)
> * [PowerShell](vpn-gateway-howto-point-to-site-rm-ps.md)
> * [Azure portal (klasik)](vpn-gateway-howto-point-to-site-classic-azure-portal.md)
>

Noktadan siteye (P2S) VPN ağ geçidi, bir tek tek istemci bilgisayardan sanal ağınıza güvenli bir bağlantı oluşturmaya kullanın. Noktadan siteye VPN bağlantıları, uzak bir konumdan sanal ağınıza bağlanmak istediğinizde faydalıdır. Bir sanal ağa bağlanması gereken yalnızca birkaç istemciniz olduğunda, bir P2S VPN siteden siteye VPN yerine kullanabileceğiniz yararlı bir çözümüdür. P2S VPN bağlantısı, istemci bilgisayardan başlatılmasıyla oluşturulur.

> [!IMPORTANT]
> Klasik dağıtım modeli yalnızca Windows VPN istemcilerini destekler ve SSL tabanlı VPN protokolü olan Güvenli Yuva Tünel Protokolünü (SSTP) kullanır. Windows harici VPN istemcilerini desteklemek için Resource Manager dağıtım modeliyle, sanal ağ oluşturmanız gerekir. Resource Manager dağıtım modeli SSTP’ye ek olarak IKEv2 VPN desteği sunar. Daha fazla bilgi için bkz. [P2S bağlantıları hakkında](point-to-site-about.md).
>
>

![Noktadan Siteye diyagramı](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/point-to-site-connection-diagram.png)

## <a name="prerequisites"></a>Önkoşullar

Noktadan siteye sertifika kimlik doğrulaması bağlantıları, aşağıdaki önkoşulları gerektirir:

* Bir Dinamik VPN ağ geçidi.
* Azure’a yüklenmiş bir kök sertifikanın ortak anahtarı (.cer dosyası). Bu anahtar, güvenilen bir sertifika olarak kabul edilir ve kimlik doğrulaması için kullanılır.
* Kök sertifikadan oluşturulmuş ve bağlanacak her bir istemci bilgisayara yüklenmiş istemci sertifikası. Bu sertifika, istemci kimlik doğrulaması için kullanılır.
* Bir VPN istemcisi yapılandırma paketi oluşturulmalı ve bağlanan her istemci bilgisayara yüklenmelidir. İstemci yapılandırma paketi zaten var olan sanal ağa bağlanmak için gerekli bilgileri işletim sisteminde yerel VPN istemcisini yapılandırır.

Noktadan siteye bağlantıları yok gerektiren bir VPN cihazına veya şirket içi genel kullanıma yönelik IP adresi. VPN bağlantısı SSTP (Güvenli Yuva Tünel Protokolü) üzerinden oluşturulur. Sunucu tarafında 1.0, 1.1 ve 1.2 SSTP sürümlerini destekliyoruz. Kullanılacak sürüm, istemci tarafından belirlenir. Windows 8.1 ve sonraki sürümlerinde, SSTP'de varsayılan olarak 1.2 kullanılır. 

Noktadan siteye bağlantılar hakkında daha fazla bilgi için bkz. [noktadan siteye hakkında SSS](#point-to-site-faq).

### <a name="example-settings"></a>Örnek ayarlar

Bir test ortamı oluşturabilir veya bu değerlere bu makaledeki örnekleri daha iyi anlamak için aşağıdaki değerleri kullanın:

- **Sanal ağ (Klasik) ayarları oluşturma**
   - **Ad**: Girin *VNet1*.
   - **Adres alanı**: Girin *192.168.0.0/16*. Bu örnekte yalnızca bir adres alanı kullanılmaktadır. Diyagramda gösterildiği gibi, sanal ağınıza ait birden fazla adres alanı olabilir.
   - **Alt ağ adı**: Girin *ön uç*.
   - **Alt ağ adres aralığı**: Girin *192.168.1.0/24*.
   - **Abonelik**: Kullanılabilir abonelikler listesinden bir abonelik seçin.
   - **Kaynak grubu**: Girin *TestRG*. Seçin **Yeni Oluştur**, kaynak grubu yok.
   - **Konum**: Seçin **Doğu ABD** listeden.

  - **VPN bağlantısı ayarları**
    - **Bağlantı türü**: Seçin **noktadan siteye**.
    - **İstemci adres alanı**: Girin *172.16.201.0/24*. Vnet'e bu noktadan siteye bağlantıyı kullanarak bağlanan VPN istemcileri belirtilen havuzdan bir IP adresi alır.

- **Ağ geçidi yapılandırması alt ağ ayarları**
   - **Ad**: İle Autofilled *GatewaySubnet*.
   - **Adres aralığı**: Girin *192.168.200.0/24*. 

- **Ağ geçidi yapılandırma ayarlarını**:
   - **Boyutu**: Kullanmak istediğiniz SKU ağ geçidi seçin.
   - **Yönlendirme türü**: Seçin **dinamik**.

## <a name="create-a-virtual-network-and-a-vpn-gateway"></a>Sanal ağ ve VPN ağ geçidi oluşturma

Başlamadan önce bir Azure aboneliğine sahip olduğunuzu doğrulayın. Henüz Azure aboneliğiniz yoksa [MSDN abonelik avantajlarınızı](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details) etkinleştirebilir veya [ücretsiz bir hesap](https://azure.microsoft.com/pricing/free-trial) için kaydolabilirsiniz.

### <a name="part-1-create-a-virtual-network"></a>1\. Bölüm: Sanal ağ oluşturma

Bir sanal ağın (VNet) yoksa bir tane oluşturun. Ekran görüntüleri örnek olarak verilmiştir. Değerlerin kendinizinkilerle değiştirildiğinden emin olun. Azure portalını kullanarak sanal ağ oluşturmak için şu adımları uygulayın:

1. Oturum [Azure portalında](https://portal.azure.com) seçip **kaynak Oluştur**. **Yeni** sayfası açılır. 

2. İçinde **markette Ara** alanına *sanal ağ* seçip **sanal ağ** döndürülen listeden. **Sanal ağ** sayfası açılır.

3. Gelen **dağıtım modeli seçin** listesinde, seçin **Klasik**ve ardından **Oluştur**. **Sanal ağ oluştur** sayfası açılır.

4. **Sanal ağ oluştur** sayfasında sanal ağ ayarlarını yapılandırın. Bu sayfada, ilk adres alanınızı ve tek alt ağ adres aralığınızı eklersiniz. Sanal ağ oluşturma işlemini tamamladıktan sonra geri dönüp ek alt ağları ve adres alanlarını ekleyin.

   ![Sanal ağ oluştur sayfası](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/vnet125.png)

5. Seçin **abonelik** aşağı açılan listeden kullanmak istediğiniz.

6. Mevcut bir seçin **kaynak grubu**. Veya seçerek yeni bir kaynak grubu oluşturma **Yeni Oluştur** ve bir ad girin. Yeni bir kaynak grubu oluşturuyorsanız, planlanan yapılandırma değerlerinize göre kaynak grubunu adlandırın. Kaynak grupları hakkında daha fazla bilgi için bkz. [Azure Resource Manager’a genel bakış](../azure-resource-manager/resource-group-overview.md#resource-groups).

7. Seçin bir **konumu** ağınıza. Bu ayar, bu sanal ağa dağıttığınız kaynakların coğrafi konumunu belirler.

8. Seçin **Oluştur** VNet oluşturmak için. Gelen **bildirimleri** sayfasında, gördüğünüz bir **dağıtım sürüyor** ileti.

8. Sanal ağınız üzerinde oluşturuldu sonra iletiyi **bildirimleri** sayfa değişikliklerini **dağıtım başarılı**. Seçin **panoya Sabitle** Vnet'inizi Panoda kolayca bulmak istiyorsanız. 

10. DNS sunucusu ekleme (isteğe bağlı). Sanal ağınızı oluşturduktan sonra ad çözümlemesi için bir DNS sunucusunun IP adresini ekleyebilirsiniz. Belirttiğiniz DNS sunucusu IP adresi, sanal ağınızdaki kaynakların adlarını çözümleyebilen bir DNS sunucusunun adresi olmalıdır.

    Bir DNS sunucusu eklemek için seçin **DNS sunucuları** VNet sayfanızdan. Ardından, seçin ve kullanmak istediğiniz DNS sunucusunun IP adresini girin **Kaydet**.

### <a name="part-2-create-a-gateway-subnet-and-a-dynamic-routing-gateway"></a>2\. Bölüm: Bir ağ geçidi alt ağı ve dinamik yönlendirme ağ geçidi oluşturma

Bu adımda, bir ağ geçidi alt ağı ve dinamik yönlendirme ağ geçidi oluşturun. Klasik dağıtım modeli için Azure portalında, ağ geçidi alt ağı ve ağ geçidiyle aynı yapılandırma sayfaları oluşturun. Ağ geçidi alt ağı, yalnızca ağ geçidi Hizmetleri için kullanın. Ağ geçidi alt ağına hiçbir şeyi (VM’ler veya diğer hizmetler) doğrudan dağıtmayın.

1. Azure portalında bir ağ geçidi oluşturmak istediğiniz sanal ağa gidin.

2. Sanal ağınızın sayfasında **genel bakış**hem de **VPN bağlantıları** bölümünden **ağ geçidi**.

   ![Bir ağ geçidi oluşturmak için işaretleyin](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/beforegw125.png)
3. **Yeni VPN Bağlantısı** sayfasında **Noktadan siteye** öğesini seçin.

   ![Noktadan Siteye bağlantı türü](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/newvpnconnect.png)
4. İçin **istemci adres alanı**, VPN istemcilerin alacağı IP adresi bağlanırken bir IP adresi aralığını ekleyin. Bağlandığınız şirket içi konum veya bağlandığınız sanal ağ ile çakışmayacak bir özel IP adresi aralığı kullanın. Kullanmak istediğiniz özel IP adresi aralığı ile autofilled aralığı üzerine yazabilirsiniz. Bu örnek autofilled aralığı gösterir. 

   ![İstemci adres alanı](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/clientaddress.png)
5. Seçin **ağ geçidini hemen Oluştur**ve ardından **isteğe bağlı ağ geçidi Yapılandırması** açmak için **ağ geçidi Yapılandırması** sayfası.

   ![İsteğe bağlı ağ geçidi yapılandırması](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/optsubnet125.png)

6. Gelen **ağ geçidi Yapılandırması** sayfasında **alt** ağ geçidi alt ağı eklemek için. Bir ağ geçidi alt ağı/29 kadar küçük oluşturmak mümkündür. Ancak, en az/28 veya/27'i seçerek daha fazla adres içeren büyük bir alt ağ oluşturmanızı öneririz. Bunun yapılması, gelecekte isteyebileceğiniz ek yapılandırmaları barındırmak için yeterli adresi bulunmasını sağlar. Ağ geçidi alt ağlarıyla çalışırken, ağ güvenlik grubunu (NSG) ağ geçidi alt ağıyla ilişkilendirmekten kaçının. Bu alt ağ için ağ güvenlik grubu ilişkilendirilmesi, VPN ağ geçidinize beklendiği gibi çalışmamasına neden olabilir. Seçin **Tamam** bu ayarı kaydedilemiyor.

   ![GatewaySubnet ekleme](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/gwsubnet125.png)
7. Ağ geçidi **Boyutu** seçin. Boyut, sanal ağ geçidinizin ağ geçidi SKU’sudur. Azure portalda varsayılan SKU, **varsayılan**. Ağ geçidi SKU'ları hakkında daha fazla bilgi için bkz. [hakkında VPN gateway ayarları](vpn-gateway-about-vpn-gateway-settings.md#gwsku).

   ![Ağ geçidi boyutu](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/gwsize125.png)
8. Ağ geçidiniz için **Yönlendirme Türü** seçin. P2S yapılandırmaları bir **Dinamik** yönlendirme türü gerektirir. Seçin **Tamam** bittiğinde bu sayfa yapılandırır.

   ![Yönlendirme türünü yapılandırma](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/routingtype125.png)

9. Üzerinde **yeni VPN bağlantısı** sayfasında **Tamam** sanal ağ geçidinizi oluşturmaya başlamak için sayfanın alt kısmındaki. Bir VPN ağ geçidi SKU'su ağ geçidine göre tamamlanması 45 dakika sürebilir.
 
## <a name="generatecerts"></a>Sertifikaları oluşturma

Azure için noktadan siteye VPN'lerde VPN istemcilerinin kimlik doğrulaması için sertifikaları kullanır. Kök sertifikanın ortak anahtar bilgilerini Azure'a yükleyin. Ardından ortak anahtarı kabul edilir *güvenilen*. İstemci sertifikaları gerekir güvenilir kök sertifikadan oluşturulmuş ve sonra Sertifikalar-Geçerli kullanıcı\kişisel\sertifikalar sertifika deposunda her istemci bilgisayarda yüklü. Sertifika, sanal ağa bağlandığında istemci kimlik doğrulaması için kullanılır. 

Otomatik olarak imzalanan sertifikalar kullanıyorsanız, belirli parametreler kullanılarak oluşturulmalıdır. Yönergeleri kullanarak otomatik olarak imzalanan bir sertifika oluşturabilir [PowerShell ve Windows 10](vpn-gateway-certificates-point-to-site.md), veya [MakeCert](vpn-gateway-certificates-point-to-site-makecert.md). Otomatik olarak imzalanan kök sertifikalar kullanmak ve otomatik olarak imzalanan kök sertifikadan istemci sertifikaları bu Yönergelerdeki adımları izlemeniz önemlidir. Aksi halde, oluşturduğunuz sertifikalar P2S bağlantılarıyla uyumlu olmaz ve bağlantı hatası alırsınız.

### <a name="acquire-the-public-key-cer-for-the-root-certificate"></a>Kök sertifikanın ortak anahtarı (.cer) alma

[!INCLUDE [vpn-gateway-basic-vnet-rm-portal](../../includes/vpn-gateway-p2s-rootcert-include.md)]

### <a name="generate-a-client-certificate"></a>İstemci sertifikası oluşturma

[!INCLUDE [vpn-gateway-basic-vnet-rm-portal](../../includes/vpn-gateway-p2s-clientcert-include.md)]

## <a name="upload-the-root-certificate-cer-file"></a>Kök sertifika .cer dosyasını karşıya yükleme

Ağ geçidi oluşturulduktan sonra Azure sunucusu için bir güvenilen kök sertifikanın .cer dosyasını (ortak anahtar bilgilerini içeren) yükleyin. Kök sertifikanın özel anahtarı karşıya yüklemeyin. Sertifika karşıya yüklenmesinin ardından, Azure güvenilir kök sertifikadan oluşturulmuş bir istemci sertifikasının yüklü olduğu istemcilerin kimliklerini doğrulamak için kullanır. Daha sonra gerektiğinde ek güvenilen kök sertifika dosyası (en fazla 20), karşıya yükleyebilirsiniz.  

1. Üzerinde **VPN bağlantıları** bölümünde sanal ağınıza ait sayfanın açmak için istemcileri grafiği seçin **noktadan siteye VPN bağlantısı** sayfası.

   ![İstemciler](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/clients125.png)

2. Üzerinde **noktadan siteye VPN bağlantısı** sayfasında **sertifikayı Yönet** açmak için **sertifikaları** sayfası.

   ![Sertifikalar sayfası](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/ptsmanage.png)

1. Üzerinde **sertifikaları** sayfasında **karşıya** açmak için **sertifikayı karşıya yükle** sayfası.

    ![Sertifikaları karşıya yükle sayfası](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/uploadcerts.png)

4. .cer dosyasına gözatmak için klasör grafiğine seçin. Dosyayı seçin ve ardından **Tamam**. Yüklenen sertifikayı görünür **sertifikaları** sayfası.

   ![Sertifikayı karşıya yükleme](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/upload.png)


## <a name="configure-the-client"></a>İstemciyi yapılandırma

Noktadan siteye VPN kullanarak bir sanal ağa bağlanmak için her istemcinin yerel Windows VPN istemcisi yapılandırma paketini yüklemeniz gerekir. Yapılandırma paketi, sanal ağa bağlanmak için gereken ayarlarla yerel Windows VPN istemcisini yapılandırır.

Sürümünün istemci mimarisiyle eşleşmesi şartıyla, her istemci bilgisayarda aynı VPN istemcisi yapılandırma paketini kullanabilirsiniz. Desteklenen istemci işletim sistemlerinin listesi için bkz: [noktadan siteye bağlantılar hakkında SSS](#point-to-site-faq).

### <a name="generate-and-install-a-vpn-client-configuration-package"></a>Oluşturma ve bir VPN istemcisi yapılandırma paketini yükleme

1. Azure portalında, **genel bakış** sayfasında, sanal ağ için **VPN bağlantıları**, açmak için istemci grafiği seçin **noktadan siteye VPN bağlantısı** sayfası.

2. Gelen **noktadan siteye VPN bağlantısı** sayfasında, yüklü olduğu istemci işletim sistemine karşılık gelen indirme paketini seçin:

   * 64 bit istemciler için **VPN İstemcisi (64 bit)** seçeneğini belirleyin.
   * 32 bit istemciler için **VPN İstemcisi (32 bit)** seçeneğini belirleyin.

   ![VPN istemcisi yapılandırma paketini indirme](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/dlclient.png)

3. Paketini oluşturduktan sonra indirin ve ardından istemci bilgisayarınıza yükleyin. Bir SmartScreen açılır penceresi görürseniz seçin **daha fazla bilgi**, ardından **yine de Çalıştır**. Paketi ayrıca diğer istemci bilgisayarlara yüklemek üzere kaydedebilirsiniz.

### <a name="install-a-client-certificate"></a>Bir istemci sertifikasını yükleme

İstemci sertifikalarını oluşturmak için kullanılan olandan farklı bir istemci bilgisayardan bir P2S bağlantı oluşturmak için bir istemci sertifikası yükleyin. Bir istemci sertifikasını yüklediğinizde, istemci sertifikası dışarı aktarılırken oluşturulan parola gerekir. Genellikle, yalnızca çift tıklayarak sertifikayı yükleyebilirsiniz. Daha fazla bilgi için bkz. [Dışarı aktarılan istemci sertifikasını yükleme](vpn-gateway-certificates-point-to-site.md#install).


## <a name="connect-to-your-vnet"></a>Sanal ağınıza bağlanma

>[!NOTE]
>Bağlanmakta olduğunuz istemci bilgisayarında Yönetici haklarına sahip olmanız gerekir.
>
>

1. İstemci bilgisayarda sanal ağınıza bağlanmak için gidin **VPN bağlantıları** Azure portalında oluşturduğunuz VPN bağlantısını bulun. VPN bağlantısı sanal ağınızla aynı ada sahip. **Bağlan**’ı seçin. Sertifikayla ilgili bir açılır ileti görüntülenirse, seçin **devam** yükseltilmiş ayrıcalıkları kullanmak için.

2. Üzerinde **bağlantı** durumu sayfasında **Connect** bağlantıyı başlatın. Görürseniz **Sertifika Seç** ekranında, görüntülenen istemci sertifikası doğru olduğunu doğrulayın. Aksi takdirde, açılan listeden doğru sertifikayı seçin ve ardından **Tamam**.

3. Bağlantınız başarılı olursa, gördüğünüz bir **bağlı** bildirim.


### <a name="troubleshooting-p2s-connections"></a>P2S bağlantılarının sorunlarını giderme

[!INCLUDE [verify-client-certificates](../../includes/vpn-gateway-certificates-verify-client-cert-include.md)]

## <a name="verify-the-vpn-connection"></a>VPN bağlantısını doğrulama

1. VPN bağlantınızın etkin olduğunu doğrulayın. İstemci bilgisayarınızda yükseltilmiş bir komut istemi açın ve çalıştırın **ipconfig/all**.
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

## <a name="connect-to-a-virtual-machine"></a>Sanal makineye bağlanma

[!INCLUDE [Connect to a VM](../../includes/vpn-gateway-connect-vm-p2s-classic-include.md)]

## <a name="add-or-remove-trusted-root-certificates"></a>Güvenilen kök sertifika ekleme veya kaldırma

Azure’da güvenilen kök sertifikayı ekleyebilir veya kaldırabilirsiniz. Bir kök sertifikayı kaldırdığınızda, o kökten oluşturulmuş bir sertifikaya sahip istemciler artık kimlik doğrulaması bağlanın ve. Bu istemciler kimlik doğrulaması ve yeniden bağlanmak Azure tarafından güvenilen bir kök sertifikadan oluşturulmuş yeni bir istemci sertifikası yüklemeniz gerekir.

### <a name="to-add-a-trusted-root-certificate"></a>Güvenilen kök sertifika ekleme

Azure'a en fazla 20 güvenilen kök sertifika .cer dosyası ekleyebilirsiniz. Kök sertifika .cer dosyasını karşıya yükleme yönergeleri için bkz.

### <a name="to-remove-a-trusted-root-certificate"></a>Güvenilen kök sertifikayı kaldırmak için

1. Üzerinde **VPN bağlantıları** bölümünde sanal ağınıza ait sayfanın açmak için istemcileri grafiği seçin **noktadan siteye VPN bağlantısı** sayfası.

   ![İstemciler](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/clients125.png)

2. Üzerinde **noktadan siteye VPN bağlantısı** sayfasında **sertifikayı Yönet** açmak için **sertifikaları** sayfası.

   ![Sertifikalar sayfası](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/ptsmanage.png)

3. Üzerinde **sertifikaları** sayfasında, kaldırmak ve ardından istediğiniz sertifikanın yanındaki üç noktayı seçin **Sil**.

   ![Kök sertifikayı sil](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/deleteroot.png)

## <a name="revoke-a-client-certificate"></a>İstemci sertifikasını iptal etme

Gerekirse, bir istemci sertifikasını iptal edebilirsiniz. Sertifika iptal listesi sayesinde, ayrı istemci sertifikalarına göre Noktadan Siteye bağlantıyı seçmeli olarak reddedebilirsiniz. Bu yöntem, güvenilen kök sertifika kaldırma işleminden farklıdır. Azure’dan güvenilen kök sertifika .cer dosyasını kaldırırsanız iptal edilen kök sertifika tarafından oluşturulan/imzalanan tüm istemci sertifikaları reddedilir. Kök sertifika yerine istemci sertifikasını iptal etmek, kök sertifikadan oluşturulan diğer sertifikaların Noktadan Siteye bağlantı için kimlik doğrulaması amacıyla kullanılmaya devam edilmesine olanak sağlar.

Genellikle ekip ve kuruluş düzeylerinde erişimi yönetmek için kök sertifika kullanılırken ayrı kullanıcılar üzerinde ayrıntılı erişim denetimi için iptal edilen istemci sertifikaları kullanılır.

### <a name="to-revoke-a-client-certificate"></a>İstemci sertifikasını iptal etme

Parmak izini iptal listesine ekleyerek bir istemci sertifikasını iptal edebilirsiniz.

1. İstemci sertifikasının parmak izini alın. Daha fazla bilgi için [nasıl yapılır: Bir sertifikanın parmak izini alma](https://msdn.microsoft.com/library/ms734695.aspx).
2. Bilgileri bir metin düzenleyicisine kopyalayın ve sürekli bir dize olmasını, boşlukları Kaldır.
3. Klasik bir sanal ağa gidin. Seçin **noktadan siteye VPN bağlantısı**, ardından **sertifikayı Yönet** açmak için **sertifikaları** sayfası.
4. Seçin **iptal listesi** açmak için **iptal listesi** sayfası. 
5. Seçin **sertifika Ekle** açmak için **iptal listesine sertifika Ekle** sayfası.
6. İçinde **parmak izi**, sertifika parmak izini boşluk içermeyen bir sürekli satırı metin olarak yapıştırın. Seçin **Tamam** tamamlanması.

Güncelleştirme tamamlandıktan sonra sertifika artık bağlanmak için kullanılamaz. Bu sertifikayı kullanarak bağlanmaya çalışan istemciler sertifikanın artık geçerli olmadığını belirten bir ileti alır.

## <a name="point-to-site-faq"></a>Noktadan siteye hakkında SSS

[!INCLUDE [Point-to-Site FAQ](../../includes/vpn-gateway-faq-point-to-site-classic-include.md)]

## <a name="next-steps"></a>Sonraki adımlar

- Bağlantınız tamamlandıktan sonra sanal ağlarınıza sanal makineler ekleyebilirsiniz. Daha fazla bilgi için bkz. [Sanal Makineler](https://docs.microsoft.com/azure/). 

- Ağ ve Linux sanal makineleri hakkında daha fazla bilgi için bkz: [Azure ve Linux VM ağına genel bakış](../virtual-machines/linux/network-overview.md).

- P2S sorun giderme bilgileri için [Azure noktadan siteye bağlantıları sorununu giderme](vpn-gateway-troubleshoot-vpn-point-to-site-connection-problems.md).
