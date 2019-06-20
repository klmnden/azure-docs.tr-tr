---
title: 'Bir bilgisayarı noktadan siteye ve yerel Azure sertifika doğrulaması kullanarak bir Azure sanal ağına bağlama: Azure portalı | Microsoft Docs'
description: Windows, Mac OS X ve Linux istemcileri, bir Azure sanal ağı kullanarak P2S ve otomatik olarak imzalanan veya CA tarafından verilen sertifikaları güvenli bir şekilde bağlanın. Bu makalede Azure portalı kullanılmaktadır.
services: vpn-gateway
author: cherylmc
tags: azure-resource-manager
ms.service: vpn-gateway
ms.topic: conceptual
ms.date: 6/18/2019
ms.author: cherylmc
ms.openlocfilehash: 07bcf50a816c090ccef846909dff671486e514c4
ms.sourcegitcommit: b7a44709a0f82974578126f25abee27399f0887f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2019
ms.locfileid: "67203054"
---
# <a name="configure-a-point-to-site-connection-to-a-vnet-using-native-azure-certificate-authentication-azure-portal"></a>Yerel Azure sertifika doğrulaması kullanarak bir sanal ağa noktadan siteye bağlantı yapılandırma: Azure portal

Bu makale, Azure sanal ağına Windows, Linux veya Mac OS X çalıştıran bireysel istemcileri güvenli bir şekilde bağlamanıza yardımcı olur. Noktadan Siteye VPN bağlantıları, ev veya bir konferans gibi uzak bir noktadan Vnet'inize bağlanmak istediğinizde faydalıdır. Bir sanal ağa bağlanması gereken yalnızca birkaç istemciniz olduğunda Siteden Siteye VPN yerine P2S’yi de kullanabilirsiniz. Noktadan Siteye bağlantılar için bir VPN cihazına veya genel kullanıma yönelik bir IP adresine gerek yoktur. P2S, VPN bağlantısını SSTP (Güvenli Yuva Tünel Protokolü) veya IKEv2 üzerinden oluşturur. Noktadan Siteye VPN hakkında daha fazla bilgi edinmek için bkz. [Noktadan Siteye VPN hakkında](point-to-site-about.md).

![Bir bilgisayarı Azure sanal ağına bağlama - Noktadan Siteye bağlantı diyagramı](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/p2snativeportal.png)


## <a name="architecture"></a>Mimari

Noktadan Siteye yerel Azure sertifika kimlik doğrulaması bağlantıları, bu alıştırmada yapılandıracağınız aşağıdaki öğeleri kullanır:

* RouteBased VPN ağ geçidi.
* Azure’a yüklenmiş bir kök sertifikanın ortak anahtarı (.cer dosyası). Sertifika karşıya yüklendikten sonra güvenilen bir sertifika olarak kabul edilir ve kimlik doğrulaması için kullanılır.
* Kök sertifikasından oluşturulan bir istemci sertifikası. Sanal ağa bağlanacak her istemci bilgisayarda yüklü olan istemci sertifikası. Bu sertifika, istemci kimlik doğrulaması için kullanılır.
* VPN istemcisi yapılandırması. VPN istemci yapılandırması dosyaları, istemcinin sanal ağa bağlanması için gerekli bilgileri içerir. Dosyalar, işletim sisteminde yerel olarak bulunan VPN istemcisini yapılandırır. Bağlanan her istemcinin yapılandırma dosyalarındaki ayarlar kullanılarak yapılandırılması gerekir.

#### <a name="example"></a>Örnek değerler

Aşağıdaki değerleri kullanarak bir test ortamı oluşturabilir veya bu makaledeki örnekleri daha iyi anlamak için bu değerlere bakabilirsiniz:

* **VNet adı:** VNet1
* **Adres alanı:** 192.168.0.0/16<br>Bu örnekte yalnızca bir adres alanı kullanılmaktadır. Sanal ağınıza ait birden fazla adres alanı olabilir.
* **Alt ağ adı:** FrontEnd
* **Alt ağ adres aralığı:** 192.168.1.0/24
* **Abonelik:** Birden fazla aboneliğiniz varsa doğru olanı kullandığınızı doğrulayın.
* **Kaynak grubu:** TestRG
* **Konum:** Doğu ABD
* **GatewaySubnet:** 192.168.200.0/24<br>
* **DNS Sunucusu (İsteğe Bağlı):** Ad çözümlemesi için kullanmak istediğiniz DNS sunucusunun IP adresi.
* **Sanal ağ geçidi adı:** VNet1GW
* **Ağ geçidi türü:** VPN
* **VPN türü:** Rota tabanlı
* **Genel IP adresi adı:** VNet1GWpip
* **Bağlantı türü:** Noktadan siteye
* **İstemci adres havuzu:** 172.16.201.0/24<br>Sanal ağa bu Noktadan Siteye bağlantıyı kullanarak bağlanan VPN istemcileri, istemci adres havuzdan bir IP adresi alır.

## <a name="createvnet"></a>1. Sanal ağ oluşturma

Başlamadan önce, bir Azure aboneliğiniz olduğunu doğrulayın. Henüz Azure aboneliğiniz yoksa [MSDN abonelik avantajlarınızı](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details) etkinleştirebilir veya [ücretsiz bir hesap](https://azure.microsoft.com/pricing/free-trial) için kaydolabilirsiniz.
[!INCLUDE [Basic Point-to-Site VNet](../../includes/vpn-gateway-basic-p2s-vnet-rm-portal-include.md)]

## <a name="gatewaysubnet"></a>2. Ağ geçidi alt ağı ekleme

Sanal ağınızı bir ağ geçidine bağlamadan önce, bağlamak istediğiniz sanal ağ için ağ geçidi alt ağını oluşturmanız gerekir. Ağ geçidi hizmetleri, ağ geçidi alt ağında belirtilen IP adreslerini kullanır. Mümkünse, gelecekte eklenecek yapılandırma gereksinimlerine yetecek sayıda IP adresi sağlamak için ağ geçidi alt ağını /28 veya /27 değerine sahip bir CIDR bloğu kullanarak oluşturun.

[!INCLUDE [vpn-gateway-add-gwsubnet-rm-portal](../../includes/vpn-gateway-add-gwsubnet-p2s-rm-portal-include.md)]

## <a name="dns"></a>3. DNS sunucusu belirtme (isteğe bağlı)

Sanal ağınızı oluşturduktan sonra ad çözünürlüğünü işlemek için bir DNS sunucusunun IP adresini ekleyebilirsiniz. Bu yapılandırma için DNS sunucusu isteğe bağlıdır, ancak ad çözümlemesi istiyorsanız gereklidir. Bir değer belirtildiğinde yeni bir DNS sunucusu oluşturulmaz. Belirttiğiniz DNS sunucusu IP adresi, bağlandığınız kaynakların adlarını çözümleyebilen bir DNS sunucusu olmalıdır. Bu örnek için özel bir IP adresi kullandık, ancak DNS sunucunuzun IP adresi muhtemelen bu değildir. Kendi değerlerinizi kullandığınızdan emin olun. Belirttiğiniz değer, P2S bağlantısı veya VPN istemcisi tarafından değil, sanal ağa dağıttığınız kaynaklar tarafından kullanılıyor.

[!INCLUDE [vpn-gateway-add-dns-rm-portal](../../includes/vpn-gateway-add-dns-rm-portal-include.md)]

## <a name="creategw"></a>4. Sanal ağ geçidi oluşturma

[!INCLUDE [create-gateway](../../includes/vpn-gateway-add-gw-p2s-rm-portal-include.md)]

>[!NOTE]
>Temel SKU, IKEv2 veya RADIUS kimlik doğrulamasını desteklemez. Mac istemcileri, sanal ağınıza bağlanmak zorunda planlıyorsanız, temel SKU kullanmayın.
>

## <a name="generatecert"></a>5. Sertifika oluşturma

Sertifikalar, Noktadan Noktaya VPN bağlantısı üzerinden VNet’e bağlanan istemcilerin kimliğini doğrulamak için Azure tarafından kullanılır. Kök sertifika edindiğinizde, ortak anahtar bilgilerini Azure'a [yüklersiniz](#uploadfile). Bunun üzerine, kök sertifika P2S üzerinden sanal ağa bağlantı için Azure tarafından 'güvenilir' olarak görülür. Ayrıca, güvenilen kök sertifikadan istemci sertifikaları da oluşturur ve bunları her bir istemci bilgisayara yüklersiniz. İstemci sertifikası, sanal ağ ile bağlantı başlattığında istemcinin kimliğini doğrulamak için kullanılır. 

### <a name="getcer"></a>1. Kök sertifikaya ilişkin .cer dosyasını alma

[!INCLUDE [root-certificate](../../includes/vpn-gateway-p2s-rootcert-include.md)]

### <a name="generateclientcert"></a>2. İstemci sertifikası oluşturma

[!INCLUDE [generate-client-cert](../../includes/vpn-gateway-p2s-clientcert-include.md)]

## <a name="addresspool"></a>6. İstemci adres havuzu ekleme

İstemci adres havuzu, belirttiğiniz özel IP adresleri aralığıdır. Noktadan Siteye VPN üzerinden bağlanan istemciler dinamik olarak bu aralıktaki bir IP adresini alır. Bağlantı kurduğunuz şirket içi konum veya bağlanmak istediğiniz sanal ağ ile çakışmayan özel bir IP adresi aralığı kullanın.

1. Sanal ağ geçidi oluşturulduktan sonra sanal ağ geçidi sayfasının **Ayarlar** bölümüne gidin. **Ayarlar** bölümünde, **Noktadan siteye yapılandırması**’na tıklayın.

   ![Noktadan Siteye sayfası](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/gatewayblade.png) 
2. Yapılandırma sayfasını açmak için **Şimdi yapılandır**’a tıklayın.

   ![Şimdi yapılandır](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/configurenow.png)
3. **Noktadan siteye** yapılandırma sayfasında, **Adres havuzu** kutusunda kullanmak istediğiniz özel IP adresi aralığını ekleyin. VPN istemcileri, belirttiğiniz aralıktan dinamik olarak bir IP adresi alır. 29 bit Aktif/Pasif ve 28 bit aktif/aktif yapılandırma için en az bir alt ağ maskesidir. Ayarı doğrulayıp kaydetmek için **Kaydet**’e tıklayın.

   ![İstemci adres havuzu](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/addresspool.png)

   >[!NOTE]
   >Bu sayfadaki portalda Tünel türü veya Kimlik Doğrulama türü görmüyorsanız, ağ geçidiniz Temel SKU kullanıyor demektir. Temel SKU, IKEv2 veya RADIUS kimlik doğrulamasını desteklemez.
   >

## <a name="tunneltype"></a>7. Tünel türünü yapılandırma

Tünel türünü seçebilirsiniz. Tünel OpenVPN, SSTP ve Ikev2 seçeneklerdir. Android ve Linux üzerindeki strongSwan istemcisi ile iOS ve OSX üzerindeki yerel IKEv2 VPN istemcisi, bağlanmak için yalnızca IKEv2 tünelini kullanır. Windows istemcileri önce IKEv2’yi dener ve bağlanamazsa SSTP’ye döner. OpenVPN istemci OpenVPN tünel türüne bağlamak için kullanabilirsiniz.

![Tünel türü](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/tunneltype.png)

## <a name="authenticationtype"></a>8. Kimlik doğrulaması türünü yapılandırma

**Azure sertifikası**’nı seçin.

  ![Tünel türü](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/authenticationtype.png)

## <a name="uploadfile"></a>9. Kök sertifika ortak sertifika verilerini karşıya yükleme

Toplam 20 adede kadar güvenilir kök sertifikayı karşıya yükleyebilirsiniz. Ortak sertifika verileri karşıya yüklendikten sonra Azure, güvenilir kök sertifikadan oluşturulmuş bir istemci sertifikasının yüklü olduğu istemcilerin kimliklerini doğrulamak için bu dosyayı kullanabilir. Kök sertifikanın ortak anahtar bilgilerini Azure'a yükleyin.

1. Sertifikalar **Kök sertifikası** bölümünün **Noktadan siteye yapılandırma** sayfasına eklenir.
2. Kök sertifikayı Base-64 ile kodlanmış X.509 (.cer) dosyası olarak dışarı aktardığınızdan emin olun. Sertifikayı metin düzenleyiciyle açabilmek için bu biçimde dışa aktarmanız gerekir.
3. Sertifikayı Not Defteri gibi bir metin düzenleyiciyle açın. Sertifika verilerini kopyalarken, metni satır başları ya da satır besleme karakterleri içermeyen tek ve kesintisiz bir satır şeklinde kopyaladığınızdan emin olun. Satır başlarını ve satır besleme karakterlerini görmek için metin düzenleyicisindeki görünümünüzü 'Sembolü Göster/Tüm karakterleri göster' olarak değiştirmeniz gerekebilir. Yalnızca aşağıdaki bölümü kesintisiz bir satır olarak kopyalayın:

   ![Sertifika verileri](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/notepadroot.png)
4. Sertifika verilerini **Ortak Sertifika Verileri** alanına yapıştırın. Sertifikayı **Adlandırın** ve **Kaydet**’e tıklayın. En fazla 20 güvenilen kök sertifika ekleyebilirsiniz.

   ![Sertifika yükleme](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/uploaded.png)
5. Yapılandırma ayarlarının tümünü kaydetmek için sayfanın en üstünde yer alan **Kaydet**’e tıklayın.

   ![Kaydet](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/save.png)

## <a name="installclientcert"></a>10. Dışarı aktarılan bir istemci sertifikasını yükleme

İstemci sertifikalarını oluşturmak için kullandığınız bilgisayardan farklı bir istemci bilgisayarda bir P2S bağlantı oluşturmak istiyorsanız, bir istemci sertifikası yüklemeniz gerekir. Bir istemci sertifikası yüklenirken, istemci sertifikası dışarı aktarılırken oluşturulan parola gerekir.

İstemci sertifikasının tüm sertifika zinciriyle birlikte .pfx dosyası olarak (varsayılan seçenek) dışarı aktarılmadığından emin olun. Aksi takdirde kök sertifika bilgileri istemci bilgisayarda bulunmaz ve istemci, kimliğini düzgün bir biçimde doğrulayamaz.

Yükleme adımları için bkz. [İstemci sertifikası yükleme](point-to-site-how-to-vpn-client-install-azure-cert.md).

## <a name="clientconfig"></a>11. VPN istemcisi yapılandırma paketini oluşturma ve yükleme

VPN istemcisi yapılandırma dosyaları, P2S bağlantısı üzerinden bir sanal ağa bağlanmak üzere cihazları yapılandırmak için gereken ayarları içerir. VPN istemcisi yapılandırma dosyalarını oluşturma ve yükleme yönergeleri için bkz. [Yerel Azure sertifika doğrulaması P2S yapılandırmaları için VPN istemci yapılandırma dosyalarını oluşturma ve yükleme](point-to-site-vpn-client-configuration-azure-cert.md).

## <a name="connect"></a>12. Azure'a Bağlanma

### <a name="to-connect-from-a-windows-vpn-client"></a>Windows VPN istemcisinden bağlanmak için

>[!NOTE]
>Bağlanmakta olduğunuz Windows istemci bilgisayarında Yönetici haklarına sahip olmanız gerekir.
>
>

1. İstemci bilgisayarda sanal ağınıza bağlanmak için VPN bağlantılarında gezinin ve oluşturduğunuz VPN bağlantısını bulun. Bu VPN bağlantısı sanal ağınızla aynı ada sahiptir. **Bağlan**'a tıklayın. Sertifika kullanımına ilişkin bir açılır ileti görüntülenebilir. Yükseltilmiş ayrıcalıklar kullanmak için **Devam**’a tıklayın.

2. **Bağlantı** durum sayfasında **Bağlan**'a tıklayarak bağlantıyı başlatın. Bir **Sertifika Seç** ekranı çıkarsa, gösterilen istemci sertifikasının bağlanmak için kullanmak istediğiniz sertifika olduğunu doğrulayın. Başka bir sertifika gösteriliyorsa, açılan liste okunu kullanarak doğru sertifikayı seçin ve **Tamam**’a tıklayın.

   ![VPN istemcisinin Azure’a bağlanması](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/clientconnect.png)
3. Bağlantınız kurulur.

   ![Bağlantı kuruldu](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/connected.png)

#### <a name="troubleshoot-windows-p2s-connections"></a>Windows P2S bağlantılarının sorunlarını giderme

[!INCLUDE [verifies client certificates](../../includes/vpn-gateway-certificates-verify-client-cert-include.md)]

### <a name="to-connect-from-a-mac-vpn-client"></a>Mac VPN istemcisinden bağlanmak için

Ağ iletişim kutusunda kullanmak istediğiniz istemci profilini bulun, [VpnSettings.xml](point-to-site-vpn-client-configuration-azure-cert.md#installmac) dosyasından ayarları belirleyin ve **Bağlan**’a tıklayın.

Denetleme [yükleyin - Mac (OS X)](https://docs.microsoft.com/azure/vpn-gateway/point-to-site-vpn-client-configuration-azure-cert#installmac) ayrıntılı yönergeler için. Bağlantı sorunları yaşıyorsanız, sanal ağ geçidi temel SKU kullanmadığından emin olun. Temel SKU, Mac istemcileri için desteklenmiyor.

  ![Mac bağlantısı](./media/vpn-gateway-howto-point-to-site-rm-ps/applyconnect.png)

## <a name="verify"></a>Bağlantınızı doğrulamak için

Bu yönergeler, Windows istemcileri için geçerlidir.

1. VPN bağlantınızın etkin olduğunu doğrulamak için, yükseltilmiş bir komut istemi açın ve *ipconfig/all* komutunu çalıştırın.
2. Sonuçlara bakın. Aldığınız IP adresinin, yapılandırmanızda belirttiğiniz Noktadan Siteye VPN İstemcisi Adres Havuzu'ndaki adreslerden biri olduğuna dikkat edin. Sonuçları şu örneğe benzer:

   ```
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
   ```

## <a name="connectVM"></a>Sanal makineye bağlanma

Bu yönergeler, Windows istemcileri için geçerlidir.

[!INCLUDE [Connect to a VM](../../includes/vpn-gateway-connect-vm-p2s-include.md)]

## <a name="add"></a>Güvenilen kök sertifika ekleme veya kaldırma

Azure’da güvenilen kök sertifikayı ekleyebilir veya kaldırabilirsiniz. Bir kök sertifikayı kaldırdığınızda, o kökten oluşturulmuş bir sertifikaya sahip istemciler kimlik doğrulaması yapamaz ve bu nedenle bağlantı kuramaz. Bir istemcinin kimlik doğrulaması yapmasını ve bağlanmasını istiyorsanız, Azure’da güvenilen (karşıya yüklenmiş) bir kök sertifikadan oluşturulmuş yeni bir istemci sertifikası yüklemeniz gerekir.

### <a name="to-add-a-trusted-root-certificate"></a>Güvenilen kök sertifika ekleme

Azure'a en fazla 20 güvenilen kök sertifika .cer dosyası ekleyebilirsiniz. Yönergeler için, bu makaledeki [Güvenilen bir kök sertifikayı karşıya yükleme](#uploadfile) bölümüne bakın.

### <a name="to-remove-a-trusted-root-certificate"></a>Güvenilen kök sertifikayı kaldırmak için

1. Güvenilen kök sertifikayı kaldırmak için, sanal ağ geçidinizin **Noktadan siteye yapılandırma** sayfasına gidin.
2. Sayfanın **Kök sertifika** bölümünde, kaldırmak istediğiniz sertifikayı bulun.
3. Sertifikanın yanındaki üç noktaya tıklayın ve ardından ‘Kaldır’a tıklayın.

## <a name="revokeclient"></a>İstemci sertifikasını iptal etmek için

İstemci sertifikalarını iptal edebilirsiniz. Sertifika iptal listesi sayesinde, ayrı istemci sertifikalarına göre Noktadan Siteye bağlantıyı seçmeli olarak reddedebilirsiniz. Bu, güvenilen kök sertifika kaldırma işleminden farklıdır. Azure’dan güvenilen kök sertifika .cer dosyasını kaldırırsanız iptal edilen kök sertifika tarafından oluşturulan/imzalanan tüm istemci sertifikaları reddedilir. Kök sertifika yerine istemci sertifikasını iptal etmek, kök sertifikadan oluşturulan diğer sertifikaların kimlik doğrulaması amacıyla kullanılmaya devam edilmesine olanak sağlar.

Genellikle ekip ve kuruluş düzeylerinde erişimi yönetmek için kök sertifika kullanılırken ayrı kullanıcılar üzerinde ayrıntılı erişim denetimi için iptal edilen istemci sertifikaları kullanılır.

### <a name="revoke-a-client-certificate"></a>İstemci sertifikasını iptal etme

Parmak izini iptal listesine ekleyerek bir istemci sertifikasını iptal edebilirsiniz.

1. İstemci sertifikasının parmak izini alın. Daha fazla bilgi için bkz. [Bir Sertifikanın Parmak İzini alma](https://msdn.microsoft.com/library/ms734695.aspx).
2. Bilgileri bir metin düzenleyicisine kopyalayın ve sürekli bir dize haline getirmek için tüm boşlukları kaldırın.
3. Sanal ağ geçidi **Noktadan siteye yapılandırma** sayfasına gidin. Bu, [güvenilen kök sertifika yüklemek](#uploadfile) için kullandığınız sayfanın aynısıdır.
4. **İptal edilen sertifikalar** bölümünde, sertifika için bir kolay ad girin (sertifika genel adından farklı olabilir).
5. Parmak izi dizesini kopyalayın ve **Parmak izi** alanına yapıştırın.
6. Parmak izi doğrulanır ve otomatik olarak iptal listesine eklenir. Ekranda listenin güncelleştirildiğini belirten bir ileti görünür. 
7. Güncelleştirme tamamlandıktan sonra sertifika artık bağlanmak için kullanılamaz. Bu sertifikayı kullanarak bağlanmaya çalışan istemciler sertifikanın artık geçerli olmadığını belirten bir ileti alır.

## <a name="faq"></a>Noktadan Siteye hakkında SSS

[!INCLUDE [Point-to-Site FAQ](../../includes/vpn-gateway-faq-p2s-azurecert-include.md)]

## <a name="next-steps"></a>Sonraki adımlar
Bağlantınız tamamlandıktan sonra sanal ağlarınıza sanal makineler ekleyebilirsiniz. Daha fazla bilgi için bkz. [Sanal Makineler](https://docs.microsoft.com/azure/). Ağ ve sanal makineler hakkında daha fazla bilgi edinmek için, bkz. [Azure ve Linux VM ağına genel bakış](../virtual-machines/linux/azure-vm-network-overview.md).

P2S sorun giderme bilgileri için [Azure noktadan siteye bağlantıları sorununu giderme](vpn-gateway-troubleshoot-vpn-point-to-site-connection-problems.md).
