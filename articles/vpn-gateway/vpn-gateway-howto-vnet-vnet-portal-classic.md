---
title: "Sanal ağlar arasında bir bağlantı oluşturun: Klasik: Azure portal | Microsoft Docs"
description: "Birlikte PowerShell ve Azure Klasik portalı kullanarak Azure sanal ağlara bağlanma."
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/02/2017
ms.author: cherylmc
ms.openlocfilehash: 77097d59077cd8e199acdb5dc0d8427369565eea
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="configure-a-vnet-to-vnet-connection-classic"></a>VNet-VNet bağlantı (Klasik) yapılandırma

[!INCLUDE [deployment models](../../includes/vpn-gateway-classic-deployment-model-include.md)]

Bu makalede, sanal ağlar arasında VPN ağ geçidi bağlantısının nasıl oluşturulduğu gösterilir. Sanal ağlar aynı ya da farklı bölgelerde ve aynı ya da farklı aboneliklerde bulunuyor olabilirler. Bu makaledeki adımları Klasik dağıtım modeli ve Azure portalı için geçerlidir. Ayrıca aşağıdaki listeden farklı bir seçenek belirtip farklı bir dağıtım aracı veya dağıtım modeli kullanarak da bu yapılandırmayı oluşturabilirsiniz:

> [!div class="op_single_selector"]
> * [Azure portal](vpn-gateway-howto-vnet-vnet-resource-manager-portal.md)
> * [PowerShell](vpn-gateway-vnet-vnet-rm-ps.md)
> * [Azure CLI](vpn-gateway-howto-vnet-vnet-cli.md)
> * [Azure portal (klasik)](vpn-gateway-howto-vnet-vnet-portal-classic.md)
> * [Farklı dağıtım modellerini bağlama - Azure portalı](vpn-gateway-connect-different-deployment-models-portal.md)
> * [Farklı dağıtım modellerini bağlama - PowerShell](vpn-gateway-connect-different-deployment-models-powershell.md)
>
>

![VNet'ten Vnet'e bağlantı diyagramı](./media/vpn-gateway-howto-vnet-vnet-portal-classic/v2vclassic.png)

## <a name="about-vnet-to-vnet-connections"></a>Sanal Ağdan Sanal Ağa bağlantıları hakkında

Başka bir sanal ağ (VNet-VNet) bir VPN ağ geçidi kullanarak Klasik dağıtım modelinde bir sanal ağa bağlanma, bir sanal ağ bir şirket içi site konumuna bağlanma benzer. Her iki bağlantı türü de IPsec/IKE kullanarak güvenli bir tünel sunmak üzere bir VPN ağ geçidi kullanır.

Bağlandığınız sanal ağlar farklı Aboneliklerde ve farklı bölgelerde olabilir. Çok siteli yapılandırmalarla VNet iletişim VNet birleştirebilirsiniz. Bu özellik şirket içi ve şirket dışı bağlantıyla ağ içi bağlantıyı birleştiren ağ topolojileri kurabilmenize olanak sağlar.

![VNet-VNet bağlantıları](./media/vpn-gateway-howto-vnet-vnet-portal-classic/aboutconnections.png)

### <a name="why"></a>Sanal ağları neden bağlamalıyız?

Sanal ağları aşağıdaki sebeplerden dolayı bağlamak isteyebilirsiniz:

* **Çapraz bölge coğrafi artıklığı ve coğrafi-durum**

  * Kendi coğrafi çoğaltma veya eşitlemenizi, güvenli bağlantıyla İnternet’te uç noktalara gitmeden ayarlayabilirsiniz.
  * Azure yük dengeleyici ve Microsoft veya üçüncü taraf kümeleme teknolojisi yüksek oranda kullanılabilir iş yükü coğrafi yedeklilik ile birçok Azure bölgesinde ayarlayabilirsiniz. Buna önemli bir örnek olarak SQL Always On ile birden fazla Azure bölgesine yayılan Kullanılabilirlik Grupları’nı birlikte kurmak verilebilir.
* **Güçlü yalıtım sınırı ile bölgesel çok katmanlı uygulamalar**

  * Aynı bölge içinde güçlü yalıtım ve güvenli arası katmanı iletişimi ile birlikte bağlı birden çok sanal ağlar ile çok katmanlı uygulamalar da ayarlayabilirsiniz.
* **Abonelik, Azure arası kuruluş iletişiminde arası**

  * Birden çok Azure aboneliğiniz varsa, farklı Aboneliklerde iş yüklerini birlikte güvenli bir şekilde sanal ağlar arasında bağlanabilirsiniz.
  * Kuruluşlar veya hizmet sağlayıcıları için Azure içinde güvenli VPN teknolojisi ile çapraz kuruluş iletişim etkinleştirebilirsiniz.

Sanal ağlar arası bağlantılar hakkında daha fazla bilgi için bu makalenin sonunda yer alan [Sanal ağdan sanal ağa dikkat edilecek noktalar](#faq) bölümünü inceleyin.

### <a name="before-you-begin"></a>Başlamadan önce

Bu alıştırmaya başlamadan önce indirin ve Azure Hizmet Yönetimi (SM) PowerShell cmdlet'lerinin en yeni sürümünü yükleyin. Daha fazla bilgi için bkz. [Azure PowerShell’i yükleme ve yapılandırma](/powershell/azure/overview). Portal adımları çoğu için kullanıyoruz, ancak sanal ağlar arasında bağlantı oluşturmak için PowerShell kullanmanız gerekir. Azure Portalı'nı kullanarak bağlantı oluşturulamıyor.

## <a name="plan"></a>1. adım -, IP adres aralıklarını planlama

Sanal ağları yapılandırmak için kullanacağınız aralıklara karar vermeniz önemlidir. Bu yapılandırma için sanal ağ Aralıklarınızın hiçbiri birbirleriyle ya da bağlandıkları yerel ağlar biriyle çakışmayan emin olmanız gerekir.

Aşağıdaki tabloda, sanal ağlar tanımlamak nasıl bir örneği gösterilmektedir. Aralıkları yalnızca bir kılavuz olarak kullanın. Sanal ağlar için aralıkları yazın. Sonraki adımlar için bu bilgileri gerekir.

**Örnek**

| Sanal Ağ | Adres alanı | Bölge | Yerel ağ sitesine bağlanır |
|:--- |:--- |:--- |:--- |
| TestVNet1 |TestVNet1<br>(10.11.0.0/16)<br>(10.12.0.0/16) |Doğu ABD |VNet4Local<br>(10.41.0.0/16)<br>(10.42.0.0/16) |
| TestVNet4 |TestVNet4<br>(10.41.0.0/16)<br>(10.42.0.0/16) |Batı ABD |VNet1Local<br>(10.11.0.0/16)<br>(10.12.0.0/16) |

## <a name="vnetvalues"></a>2. adım - sanal ağlar oluşturun

İki sanal ağ oluşturma [Azure portal](https://portal.azure.com). Klasik sanal ağlar oluşturmak adımlar için bkz: [Klasik sanal ağ oluşturma](../virtual-network/virtual-networks-create-vnet-classic-pportal.md). 

Klasik sanal ağ oluşturmak için portal'ı kullanırken, aşağıdaki adımları kullanarak sanal ağ dikey penceresine gidin gerekir, aksi takdirde Klasik sanal ağ oluşturma seçeneğini görünmez:

1. Tıklatın için '+' 'Yeni' dikey penceresini açın.
2. 'Market arama' alanına 'Sanal ağ' yazın. Bunun yerine, ağ seçerseniz sanal ağ ->, klasik bir VNet oluşturma seçeneğini almazsınız.
3. 'Sanal ağ' döndürülen listeden bulun ve sanal ağ dikey penceresini açmak için tıklatın. 
4. Sanal ağ dikey penceresinde, klasik bir VNet oluşturmak için ' Klasik' seçin. 

Bu makalede bir alıştırma olarak kullanıyorsanız, aşağıdaki örnek değerleri kullanabilirsiniz:

**TestVNet1 için değerler**

Ad: TestVNet1<br>
Adres alanı: 10.11.0.0/16, 10.12.0.0/16 (isteğe bağlı)<br>
Alt ağ adı: varsayılan<br>
Alt ağ adresi aralığı: 10.11.0.1/24<br>
Kaynak grubu: ClassicRG<br>
Location: East US<br>
GatewaySubnet: 10.11.1.0/27

**TestVNet4 için değerler**

Ad: TestVNet4<br>
Adres alanı: 10.41.0.0/16, 10.42.0.0/16 (isteğe bağlı)<br>
Alt ağ adı: varsayılan<br>
Alt ağ adresi aralığı: 10.41.0.1/24<br>
Kaynak grubu: ClassicRG<br>
Location: West US<br>
GatewaySubnet: 10.41.1.0/27

**Sanal ağlar oluştururken, aşağıdaki ayarları göz önünde bulundurun:**

* **Sanal ağ adres alanları** – sanal ağ adres alanları sayfasında sanal ağınız için kullanmak istediğiniz adres aralığını belirtin. Bu sanal makineleri ve bu sanal ağa dağıttığınız diğer rol örneklerine atanacak olan dinamik IP adresleridir.<br>Seçtiğiniz adres alanları konumlardan herhangi birinde bu VNet bağlanacağı diğer sanal ağlara veya şirket içi için adres alanları ile örtüşemez.

* **Konum** – bir sanal ağ oluşturduğunuzda, Azure bir konum (bölge) ile ilişkilendirin. Örneğin, Batı ABD fiziksel olarak bulunması, sanal ağınızda dağıtılan Vm'leriniz istiyorsanız, o konumu seçin. Oluşturduktan sonra sanal ağınızla ilişkili konumu değiştirilemiyor.

**Sanal ağlar oluşturduktan sonra aşağıdaki ayarları ekleyebilirsiniz:**

* **Adres alanı** – ek adres alanı, bu yapılandırma için gerekli değildir, ancak sanal ağ oluşturduktan sonra ek adres alanı ekleyebilirsiniz.

* **Alt ağlar** – ek alt ağlar bu yapılandırma için gerekli değildir, ancak Vm'lerinizi diğer rol örneklerinizden ayrı bir alt ağda tutmak isteyebilirsiniz.

* **DNS sunucuları** – IP adresi ve DNS sunucusu adı girin. Bu ayarla bir DNS sunucusu oluşturulmaz. Söz konusu ayar, bu sanal ağa ilişkin ad çözümlemesi için kullanmak istediğiniz DNS sunucularını belirtmenize olanak sağlar.

Bu bölümde, bağlantı türü, yerel site yapılandırın ve ağ geçidi oluşturun.

## <a name="localsite"></a>3. adım - yerel site yapılandırma

Azure sanal ağlar arasında trafiği yönlendirmek nasıl belirlemek için her bir yerel ağ sitesi belirtilen ayarları kullanır. Her sanal ağ trafiğini yönlendirmek istediğiniz ilgili yerel ağ işaret etmelidir. Her yerel ağ sitesine başvurmak için kullanmak istediğiniz adı belirleyin. Açıklayıcı bir şey kullanmak en iyisidir.

Örneğin, TestVNet1 'VNet4Local' adlı, oluşturduğunuz bir yerel ağ sitesine bağlanır. VNet4Local ayarlarını TestVNet4 için adres önekleri içerir.

Her sanal ağ yerel sitesi diğer Vnet'in içindir. Aşağıdaki örnek değerler yapılandırmamızın için kullanılır:

| Sanal Ağ | Adres alanı | Bölge | Yerel ağ sitesine bağlanır |
|:--- |:--- |:--- |:--- |
| TestVNet1 |TestVNet1<br>(10.11.0.0/16)<br>(10.12.0.0/16) |Doğu ABD |VNet4Local<br>(10.41.0.0/16)<br>(10.42.0.0/16) |
| TestVNet4 |TestVNet4<br>(10.41.0.0/16)<br>(10.42.0.0/16) |Batı ABD |VNet1Local<br>(10.11.0.0/16)<br>(10.12.0.0/16) |

1. TestVNet1 Azure portalında bulun. İçinde **VPN bağlantıları** bölüm için dikey pencerenin tıklatın **ağ geçidi**.

    ![Ağ geçidi yok](./media/vpn-gateway-howto-vnet-vnet-portal-classic/nogateway.png)
2. Üzerinde **yeni VPN bağlantısı** sayfasında, **siteden siteye**.
3. Tıklatın **yerel site** yerel site sayfasını açın ve ayarlarını yapılandırmak için.
4. Üzerinde **yerel site** sayfasında, yerel site adı. Bizim örneğimizde, biz 'VNet4Local' yerel site adı.
5. İçin **VPN ağ geçidi IP adresi**, geçerli bir biçimde olduğu sürece, istediğiniz herhangi bir IP adresi kullanabilirsiniz. Genellikle, bir VPN cihazı için gerçek dış IP adresi kullanırsınız. Ancak, klasik bir VNet-VNet yapılandırması için ağ geçidi ağınız için atanan ortak IP adresi kullanın. Sanal ağ geçidi henüz oluşturduğunuz koşuluyla, bir yer tutucu olarak herhangi bir geçerli ortak IP adresi belirtin.<br>Bu boş bırakmayın - bu yapılandırma için isteğe bağlı değil. Sonraki adımda, bu ayarlar geri dönün ve Azure ürettiği sonra bunları karşılık gelen sanal ağ ağ geçidi IP adresiyle yapılandırın.
6. İçin **istemci adres alanı**, diğer vnet'in adres alanı kullanın. Planlama, örneğe bakın. Tıklatın **Tamam** ayarlarınızı kaydetmek ve geri dönmek için **yeni VPN bağlantısı** dikey.

    ![yerel site](./media/vpn-gateway-howto-vnet-vnet-portal-classic/localsite.png)

## <a name="gw"></a>4. adım - sanal ağ geçidi oluşturma

Her sanal ağ, bir sanal ağ geçidi olmalıdır. Sanal ağ geçidi yönlendirir ve trafiği şifreler.

1. **Yeni VPN Bağlantısı** dikey penceresinde, **Ağ geçidini hemen oluştur** onay kutusunu işaretleyin.
2. Tıklatın **alt ağ, boyut, yönlendirme türü**. Üzerinde **ağ geçidi Yapılandırması** dikey penceresinde tıklatın **alt**.
3. Ağ geçidi alt ağ adı gerekli 'GatewaySubnet' adıyla otomatik olarak doldurulur. **Adres aralığı** VPN ağ geçidi Hizmetleri için ayrılan IP adreslerini içerir. Bazı yapılandırmalar bir ağ geçidi alt ağı /29 izin ver, ancak daha fazla IP adresi için Ağ Geçidi Hizmetleri gerektirebilir gelecekteki yapılandırmalarını karşılamak için/28 ya da /27 kullanmak en iyisidir. Bizim örnek ayarlarında 10.11.1.0/27 kullanırız. Adres alanı ayarlamanız ve ardından **Tamam**.
4. Yapılandırma **ağ geçidi boyutu**. Bu ayar başvurduğu [ağ geçidi SKU'su](vpn-gateway-about-vpn-gateway-settings.md#gwsku).
5. Yapılandırma **yönlendirme türü**. Bu yapılandırmayı olmalıdır için türü yönlendirme **dinamik**. Ağ geçidi kesmeden ve yeni bir tane oluşturun sürece yönlendirme türü daha sonra değiştiremezsiniz.
6. **Tamam** düğmesine tıklayın.
7. Üzerinde **yeni VPN bağlantısı** dikey penceresinde tıklatın **Tamam** sanal ağ geçidi oluşturmaya başlamak için. Bir ağ geçidinin oluşturulması, seçili ağ geçidi SKU’suna bağlı olarak 45 dakika veya daha uzun sürebilir.

## <a name="vnet4settings"></a>5. adım - TestVNet4 ayarlarını yapılandırma

Adımlarını yineleyin [bir yerel site oluşturma](#localsite) ve [sanal ağ geçidi oluşturma](#gw) gerektiğinde değerleri değiştirerek TestVNet4 yapılandırmak için. Bu bir alıştırma olarak yapmakta olduğunuz kullanırsanız [örnek değerler](#vnetvalues).

## <a name="updatelocal"></a>6. adım - yerel siteleri güncelleştir

Her iki sanal ağlar için sanal ağ geçitlerini oluşturulduktan sonra yerel siteleri ayarlamanız gerekir **VPN ağ geçidi IP adresi** değerleri.

|VNet adı|Bağlantılı site|Ağ geçidi IP adresi|
|:--- |:--- |:--- |
|TestVNet1|VNet4Local|TestVNet4 için VPN ağ geçidi IP adresi|
|TestVNet4|VNet1Local|TestVNet1 için VPN ağ geçidi IP adresi|

### <a name="part-1---get-the-virtual-network-gateway-public-ip-address"></a>Bölüm 1 - sanal ağ geçidi genel IP adresi al

1. Sanal ağınızı Azure portalında bulun.
2. VNet açmak için tıklatın **genel bakış** dikey. Dikey olarak **VPN bağlantıları**, sanal ağ geçidiniz için IP adresini görüntüleyebilirsiniz.

  ![Genel IP](./media/vpn-gateway-howto-vnet-vnet-portal-classic/publicIP.png)
3. IP adresi kopyalayın. Sonraki bölümde kullanır.
4. TestVNet4 için bu adımları yineleyin

### <a name="part-2---modify-the-local-sites"></a>Bölüm 2 - yerel siteleri değiştirme

1. Sanal ağınızı Azure portalında bulun.
2. VNet üzerinde **genel bakış** dikey penceresinde, yerel site'ı tıklatın.

  ![Yerel site oluşturuldu](./media/vpn-gateway-howto-vnet-vnet-portal-classic/local.png)
3. Üzerinde **siteden siteye VPN bağlantıları** dikey penceresinde değiştirmek istediğiniz yerel sitenin adını tıklatın.

  ![Yerel site Aç](./media/vpn-gateway-howto-vnet-vnet-portal-classic/openlocal.png)
4. Tıklatın **yerel site** değiştirmek istediğiniz.

  ![Site değiştirme](./media/vpn-gateway-howto-vnet-vnet-portal-classic/connections.png)
5. Güncelleştirme **VPN ağ geçidi IP adresi** tıklatıp **Tamam** ayarları kaydetmek için.

  ![ağ geçidi IP](./media/vpn-gateway-howto-vnet-vnet-portal-classic/gwupdate.png)
6. Diğer dikey pencereleri kapatın.
7. TestVNet4 için bu adımları yineleyin.

## <a name="getvalues"></a>7. adım - ağ yapılandırma dosyası alma değerleri

Azure portalında Klasik sanal ağlar oluşturduğunuzda, görüntülediğiniz adı için PowerShell kullanın tam adı değil. Örneğin, adlandırılması görünür bir VNet **TestVNet1** Portalı'nda Ağ yapılandırma dosyasında bir kadar daha uzun bir ad olabilir. Adı şöyle görünebilir: **Grup ClassicRG TestVNet1**. Bağlantılarınızı oluşturduğunuzda, ağ yapılandırma dosyasında gördüğünüz değerleri kullanmak önemlidir.

Aşağıdaki adımlarda, Azure hesabınıza bağlanın ve indirip bağlantılarınız için gerekli değerleri almak için ağ yapılandırma dosyasını görüntüleyin.

1. Azure Hizmet Yönetimi (SM) PowerShell cmdlet'lerinin en son sürümünü yükleyip yeniden açın. Daha fazla bilgi için bkz. [Azure PowerShell’i yükleme ve yapılandırma](/powershell/azure/overview).

2. PowerShell konsolunuzu yükseltilmiş haklarla açın ve hesabınıza bağlanın. Bağlanmanıza yardımcı olması için aşağıdaki örneği kullanın:

  ```powershell
  Login-AzureRmAccount
  ```

  Hesapla ilişkili abonelikleri kontrol edin.

  ```powershell
  Get-AzureRmSubscription
  ```

  Birden fazla aboneliğiniz varsa, kullanmak istediğiniz aboneliği seçin.

  ```powershell
  Select-AzureRmSubscription -SubscriptionName "Replace_with_your_subscription_name"
  ```

  Ardından, Klasik dağıtım modeli için PowerShell için Azure aboneliğinize eklemek için aşağıdaki cmdlet'i kullanın.

  ```powershell
  Add-AzureAccount
  ```
3. Dışarı aktarma ve ağ yapılandırma dosyasını görüntüleyin. Bilgisayarınızda bir dizin oluşturun ve sonra ağ yapılandırma dosyasını dizine aktarın. Bu örnekte, ağ yapılandırma dosyası için dışa aktarılan **C:\AzureNet**.

  ```powershell
  Get-AzureVNetConfig -ExportToFile C:\AzureNet\NetworkConfig.xml
  ```
4. Dosyayı bir metin düzenleyicisiyle açın ve sanal ağlar ve siteler için adlar görüntüleyin. Bunlar, bağlantılarınızı oluştururken kullandığınız ad olacaktır.<br>Sanal ağ adları olarak listelenen **VirtualNetworkSite adı =**<br>Site adları olarak listelenen **LocalNetworkSiteRef adı =**

## <a name="createconnections"></a>8. adım - VPN ağ geçidi bağlantıları oluşturma

Önceki tüm adımları tamamladıktan sonra IPSec/IKE önceden paylaşılan anahtarlar ayarlayabilir ve bağlantı oluşturabilirsiniz. Bu adımlar, PowerShell kullanıyor. Azure portalında Klasik dağıtım modeli için VNet-VNet bağlantıları yapılandırılamaz.

Örneklerde paylaşılan anahtar tam olarak aynı olduğuna dikkat edin. Paylaşılan anahtarı her zaman aynı olmalıdır. Sanal ağlar ve yerel ağ siteleri için tam adlarıyla Bu örneklerde değerleri değiştirdiğinizden emin olun.

1. TestVNet1 - TestVNet4 bağlantısını oluşturun.

  ```powershell
  Set-AzureVNetGatewayKey -VNetName 'Group ClassicRG TestVNet1' `
  -LocalNetworkSiteName '17BE5E2C_VNet4Local' -SharedKey A1b2C3D4
  ```
2. TestVNet4 - TestVNet1 bağlantısı oluşturun.

  ```powershell
  Set-AzureVNetGatewayKey -VNetName 'Group ClassicRG TestVNet4' `
  -LocalNetworkSiteName 'F7F7BFC7_VNet1Local' -SharedKey A1b2C3D4
  ```
3. Bağlantılar başlatmak bekleyin. Ağ geçidi başlatıldı sonra durumu 'Başarılı' şeklindedir.

  ```
  Error          :
  HttpStatusCode : OK
  Id             :
  Status         : Successful
  RequestId      :
  StatusCode     : OK
  ```

## <a name="faq"></a>Klasik sanal ağlar için VNet-VNet konuları
* Sanal ağlar aynı ya da farklı Aboneliklerde olabilir.
* Sanal ağlar aynı ya da farklı Azure bölgelerinde (konumlarında) bulunabilir.
* Bir bulut hizmeti ya da yük dengeleme uç noktası, birbirlerine bağlı olsa da sanal ağlara yayılamaz.
* Birden çok sanal ağları birbirine bağlama hiçbir VPN cihazı gerektirmez.
* VNet-VNet Azure sanal ağları bağlamayı destekler. Bağlanan sanal makineleri desteklemez veya bir sanal ağa dağıtılmaz Hizmetleri bulut.
* VNet-VNet dinamik yönlendirme ağ geçitleri gerektirir. Azure statik yönlendirme ağ geçitleri desteklenmez.
* Sanal ağ bağlantısı, çoklu site VPN’leri ile eşzamanlı olarak kullanılabilir. En fazla 10 VPN tünelleri ya da diğer sanal ağlar, veya şirket içi sitelere bağlanan sanal ağ VPN ağ geçidi için yoktur.
* Sanal ağlar ve şirket içi yerel ağ sitelerinin adres alanları çakışmamalıdır. Çakışan adres alanlarının oluşturulması sanal ağlar veya karşıya yükleme netcfg yapılandırma dosyaları başarısız olmasına neden olur.
* Bir çift sanal ağ arasındaki yedekli tüneller desteklenmez.
* Tüm VPN tünelleri P2S VPN'lerde dahil VNet için VPN ağ geçidi ve SLA'sı üzerinde aynı VPN ağ geçidi çalışma süresi için kullanılabilir bant genişliğini paylaşır.
* VNet-VNet trafiği Azure omurga geçen.

## <a name="next-steps"></a>Sonraki adımlar
Bağlantılarınızı doğrulayın. Bkz: [bir VPN ağ geçidi bağlantısını doğrulama](vpn-gateway-verify-connection-resource-manager.md).
