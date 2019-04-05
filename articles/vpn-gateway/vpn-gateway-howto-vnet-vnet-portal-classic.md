---
title: 'Sanal ağlar arası bağlantı oluşturma: Klasik: Azure portalı | Microsoft Docs'
description: PowerShell ve Azure portalını kullanarak Azure sanal ağları bağlayabilir.
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: jpconnock
editor: ''
tags: azure-service-management
ms.assetid: ''
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/14/2018
ms.author: cherylmc
ms.openlocfilehash: e323a8d71bbffd1d29ad793dff7b5b4a072b6979
ms.sourcegitcommit: 8313d5bf28fb32e8531cdd4a3054065fa7315bfd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/05/2019
ms.locfileid: "59046131"
---
# <a name="configure-a-vnet-to-vnet-connection-classic"></a>Bir VNet-VNet bağlantısı (Klasik) yapılandırma

[!INCLUDE [deployment models](../../includes/vpn-gateway-classic-deployment-model-include.md)]

Bu makalede, sanal ağlar arasında bir VPN gateway bağlantısı oluşturmanıza yardımcı olur. Sanal ağlar aynı ya da farklı bölgelerde ve aynı ya da farklı aboneliklerde bulunuyor olabilirler. Bu makaledeki adımlar Klasik dağıtım modelini ve Azure portalı için geçerlidir. Ayrıca aşağıdaki listeden farklı bir seçenek belirtip farklı bir dağıtım aracı veya dağıtım modeli kullanarak da bu yapılandırmayı oluşturabilirsiniz:

> [!div class="op_single_selector"]
> * [Azure portal](vpn-gateway-howto-vnet-vnet-resource-manager-portal.md)
> * [PowerShell](vpn-gateway-vnet-vnet-rm-ps.md)
> * [Azure CLI](vpn-gateway-howto-vnet-vnet-cli.md)
> * [Azure portalını (Klasik)](vpn-gateway-howto-vnet-vnet-portal-classic.md)
> * [Farklı dağıtım modelleri - Azure portal'ı bağlama](vpn-gateway-connect-different-deployment-models-portal.md)
> * [Farklı dağıtım modellerini bağlama - PowerShell](vpn-gateway-connect-different-deployment-models-powershell.md)
>
>

![Vnet'ten Vnet'e bağlantı diyagramı](./media/vpn-gateway-howto-vnet-vnet-portal-classic/v2vclassic.png)

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="about-vnet-to-vnet-connections"></a>Sanal Ağdan Sanal Ağa bağlantıları hakkında

Başka bir sanal ağa (VNet-VNet) bir VPN ağ geçidi kullanarak Klasik dağıtım modelinde bir sanal ağa bağlanan bir sanal ağ bir şirket içi site konumuna bağlamakla aynıdır. Her iki bağlantı türü de IPsec/IKE kullanarak güvenli bir tünel sunmak üzere bir VPN ağ geçidi kullanır.

Vnet'ler farklı Aboneliklerde ve farklı bölgelerde olabilir. Sanal ağ, sanal ağa iletişim çok siteli yapılandırmalarla birleştirebilirsiniz. Bu özellik şirket içi ve şirket dışı bağlantıyla ağ içi bağlantıyı birleştiren ağ topolojileri kurabilmenize olanak sağlar.

![Vnet'ten Vnet'e bağlantılar](./media/vpn-gateway-howto-vnet-vnet-portal-classic/aboutconnections.png)

### <a name="why"></a>Sanal ağları neden bağlamalıyız?

Sanal ağları aşağıdaki sebeplerden dolayı bağlamak isteyebilirsiniz:

* **Çapraz bölgede coğrafi yedeklilik ve iletişim durumu**

  * Kendi coğrafi çoğaltma veya eşitlemenizi, güvenli bağlantıyla İnternet’te uç noktalara gitmeden ayarlayabilirsiniz.
  * Azure Load Balancer ve Microsoft veya üçüncü taraf Kümeleme Teknolojisi coğrafi yedeklilik ile yüksek oranda kullanılabilir iş yükü birden çok Azure bölgeleri arasında ayarlayabilirsiniz. Buna önemli bir örnek olarak SQL Always On ile birden fazla Azure bölgesine yayılan Kullanılabilirlik Grupları’nı birlikte kurmak verilebilir.
* **Güçlü yalıtım sınırı ile bölgesel çok katmanlı uygulamalar**

  * Aynı bölge içinde güçlü yalıtım ve güvenli arası katmanı bağlantı birlikte bağlı birden fazla sanal ağ ile çok katmanlı uygulamalar da ayarlayabilirsiniz.
* **Çapraz abonelik, azure'da kuruluş arası iletişim**

  * Birden çok Azure aboneliğiniz varsa, iş yüklerini farklı aboneliklere ait birlikte güvenli bir sanal ağ arasında bağlanabilirsiniz.
  * Kuruluşlar veya hizmet sağlayıcılarının, Azure güvenli VPN teknolojisi ile kuruluşlar arası iletişimi etkinleştirebilirsiniz.

Sanal ağlar arası bağlantılar hakkında daha fazla bilgi için bu makalenin sonunda yer alan [Sanal ağdan sanal ağa dikkat edilecek noktalar](#faq) bölümünü inceleyin.

### <a name="before-you-begin"></a>Başlamadan önce

Bu alıştırmaya başlamadan önce indirin ve Azure Hizmet Yönetimi (SM) PowerShell cmdlet'lerinin en son sürümünü yükleyin. Daha fazla bilgi için bkz. [Azure PowerShell’i yükleme ve yapılandırma](/powershell/azure/overview). Portal adımların çoğu için kullanıyoruz, ancak sanal ağlar arasında bağlantı oluşturmak için PowerShell kullanmanız gerekir. Azure portalını kullanarak bağlantı oluşturulamıyor.

## <a name="plan"></a>1. adım -, IP adres aralıklarını planlama

Sanal ağlarınız yapılandırmak için kullanacağınız aralıklara karar vermeniz önemlidir. Bu yapılandırma için sanal ağ aralıklarınız hiçbiri birbiriyle veya bağlandıkları yerel ağlar biriyle çakışıyor emin olmanız gerekir.

Aşağıdaki tabloda, sanal ağlarınız tanımlamak nasıl bir örnek gösterilmektedir. Aralıkları, yalnızca bir kılavuz olarak kullanın. Sanal ağlarınız için aralığı yazın. Sonraki adımlar için bu bilgileri ihtiyacınız var.

**Örnek**

| Sanal Ağ | Adres Alanı | Bölge | Yerel ağ alanına bağlanır. |
|:--- |:--- |:--- |:--- |
| TestVNet1 |TestVNet1<br>(10.11.0.0/16)<br>(10.12.0.0/16) |Doğu ABD |VNet4Local<br>(10.41.0.0/16)<br>(10.42.0.0/16) |
| TestVNet4 |TestVNet4<br>(10.41.0.0/16)<br>(10.42.0.0/16) |Batı ABD |VNet1Local<br>(10.11.0.0/16)<br>(10.12.0.0/16) |

## <a name="vnetvalues"></a>2. adım - sanal ağlar oluşturma

İki sanal ağ oluşturma [Azure portalında](https://portal.azure.com). Klasik sanal ağlar oluşturma adımları için bkz: [Klasik sanal ağ oluşturma](../virtual-network/virtual-networks-create-vnet-classic-pportal.md). 

Klasik bir sanal ağ oluşturmak için portal'ı kullanarak, aşağıdaki adımları kullanarak sanal ağ sayfasına gitmeniz gerekir, aksi takdirde bir Klasik sanal ağ oluşturma seçeneğini görünmez:

1. Tıklayın '+' için 'New' sayfasını açın.
2. 'Markette Ara' alanına 'Sanal ağ' yazın. Bunun yerine, ağı seçerseniz -> sanal ağ, klasik bir VNet oluşturmak için bu seçeneği almazsınız.
3. Döndürülen listeden 'Sanal ağ' bulun ve sanal ağ sayfasını açmak için tıklayın. 
4. Sanal ağınızın sayfasında Klasik VNet oluşturmak için ' Klasik' seçin. 

Bu makalede bir alıştırma olarak kullanırken, şu örnek değerleri kullanabilirsiniz:

**TestVNet1 için değerler**

Ad: TestVNet1<br>
Adres alanı: 10.11.0.0/16, 10.12.0.0/16 (isteğe bağlı)<br>
Alt ağ adı: varsayılan<br>
Alt ağ adres aralığı: 10.11.0.1/24<br>
Kaynak grubu: ClassicRG<br>
Konum: Doğu ABD<br>
GatewaySubnet: 10.11.1.0/27

**TestVNet4 için değerler**

Ad: TestVNet4<br>
Adres alanı: 10.41.0.0/16, 10.42.0.0/16 (isteğe bağlı)<br>
Alt ağ adı: varsayılan<br>
Alt ağ adres aralığı: 10.41.0.1/24<br>
Kaynak grubu: ClassicRG<br>
Konum: Batı ABD<br>
GatewaySubnet: 10.41.1.0/27

**Sanal ağlarınız oluştururken aşağıdaki ayarlara dikkat edin:**

* **Sanal ağ adres alanları** – sanal ağ adres alanları sayfasında, sanal ağınız için kullanmak istediğiniz adres aralığını belirtin. Bu sanal makineleri ve bu sanal ağa dağıttığınız diğer rol örneklerine atanacak olan dinamik IP adresleridir.<br>Seçtiğiniz adres alanlarının adres alanları için bu sanal ağ bağlanacağı diğer sanal ağlara veya şirket içi konumlardan birini ile çakışamaz.

* **Konum** – bir sanal ağ oluşturduğunuzda, Azure konum (bölge) ile ilişkilendirin. Örneğin, Batı ABD bölgesinde fiziksel olarak bulunması için sanal ağınızda dağıtılan sanal makinelerinizin istiyorsanız, o konumu seçin. Oluşturduktan sonra sanal ağınızla ilişkili konumu değiştirilemiyor.

**Sanal ağlarınız oluşturduktan sonra aşağıdaki ayarları ekleyebilirsiniz:**

* **Adres alanı** – ek adres alanı, bu yapılandırma için gerekli değildir, ancak sanal ağı oluşturduktan sonra ek adres alanı ekleyebilirsiniz.

* **Alt ağlar** – ek alt ağlar, bu yapılandırma için gerekli değildir, ancak Vm'lerinizi diğer rol örneklerinizden ayrı bir alt ağda tutmak isteyebilirsiniz.

* **DNS sunucuları** – IP adresi ve DNS sunucusu adı girin. Bu ayarla bir DNS sunucusu oluşturulmaz. Söz konusu ayar, bu sanal ağa ilişkin ad çözümlemesi için kullanmak istediğiniz DNS sunucularını belirtmenize olanak sağlar.

Bu bölümde, bağlantı türü, yerel site yapılandırma ve ağ geçidi oluşturun.

## <a name="localsite"></a>3. adım - yerel siteyi yapılandırma

Azure, sanal ağlar arasındaki trafiği nasıl yönlendireceğini belirlemek için her yerel ağ alanında belirtilen ayarları kullanır. Her sanal ağ trafiğini yönlendirmek istediğinizi ilgili yerel ağ işaret etmelidir. Her yerel ağ alanına başvurmak için kullanmak istediğiniz adı belirleyin. Açıklayıcı bir şey kullanmak en iyisidir.

Örneğin, TestVNet1 'VNet4Local' adlı bir yerel ağ sitesi için bağlanır. VNet4Local ayarlarını adres ön ekleri için testvnet4'ü içerir.

Her sanal ağ için yerel site başka bir sanal ağ ' dir. Aşağıdaki örnek değerler için sunduğumuz yapılandırması kullanılır:

| Sanal Ağ | Adres Alanı | Bölge | Yerel ağ alanına bağlanır. |
|:--- |:--- |:--- |:--- |
| TestVNet1 |TestVNet1<br>(10.11.0.0/16)<br>(10.12.0.0/16) |Doğu ABD |VNet4Local<br>(10.41.0.0/16)<br>(10.42.0.0/16) |
| TestVNet4 |TestVNet4<br>(10.41.0.0/16)<br>(10.42.0.0/16) |Batı ABD |VNet1Local<br>(10.11.0.0/16)<br>(10.12.0.0/16) |

1. Testvnet1'i Azure portalında bulun. İçinde **VPN bağlantıları** Bölümü sayfasına tıklayın **ağ geçidi**.

    ![Ağ geçidi yok](./media/vpn-gateway-howto-vnet-vnet-portal-classic/nogateway.png)
2. Üzerinde **yeni VPN bağlantısı** sayfasında **siteden siteye**.
3. Tıklayın **yerel site** yerel site sayfasını açmak için ayarları yapılandırın.
4. Üzerinde **yerel site** sayfasında, yerel sitenizin adı. Bizim örneğimizde, biz yerel site 'VNet4Local' olarak adlandırın.
5. İçin **VPN ağ geçidi IP adresini**, geçerli bir biçimde olduğu sürece, istediğiniz herhangi bir IP adresi kullanabilirsiniz. Genellikle, VPN cihazı için gerçek dış IP adresi kullanırsınız. Ancak, klasik bir VNet-VNet yapılandırmasını ağınız için ağ geçidine atanan genel IP adresini kullanın. Sanal ağ geçidi henüz oluşturduğunuz düşünüldüğünde, bir yer tutucu olarak herhangi bir geçerli genel IP adresi belirtin.<br>Bu boş bırakmayın - bu yapılandırma için isteğe bağlı değil. Bir sonraki adımda bu ayarlarına geri dönün ve Azure, oluşturduktan sonra bunları karşılık gelen sanal ağ geçidi IP adresleriyle yapılandırın.
6. İçin **istemci adres alanı**, diğer sanal ağın adres alanı kullanın. Planlama, örneğe bakın. Tıklayın **Tamam** ayarlarınızı kaydetmek ve geri dönmek için **yeni VPN bağlantısı** sayfası.

    ![yerel site](./media/vpn-gateway-howto-vnet-vnet-portal-classic/localsite.png)

## <a name="gw"></a>4. adım - sanal ağ geçidi oluşturma

Her bir sanal ağ, sanal ağ geçidi olması gerekir. Sanal ağ geçidi yönlendirir ve trafiği şifreler.

1. **Yeni VPN Bağlantısı** sayfasında, **Ağ geçidini hemen oluştur** onay kutusunu işaretleyin.
2. Tıklayın **alt ağ, boyut ve yönlendirme türü**. Üzerinde **ağ geçidi Yapılandırması** sayfasında **alt**.
3. Ağ geçidi alt ağ adı 'GatewaySubnet' gerekli ad ile otomatik olarak doldurulur. **Adres aralığı** VPN ağ geçidi hizmetlerine ayrılır IP adreslerini içerir. Bazı yapılandırmalar bir ağ geçidi alt ağı / 29 izin ver, ancak daha fazla IP adresi için Ağ Geçidi Hizmetleri gerektirebilir gelecek yapılandırmaları barındırmak için/28 veya/27 kullanmak en iyisidir. Bizim örnek ayarlarda 10.11.1.0/27 kullanırız. Adres alanı tıklayabilir, ardından tıklayın **Tamam**.
4. Yapılandırma **ağ geçidi boyutu**. Bu ayar başvurduğu [ağ geçidi SKU'su](vpn-gateway-about-vpn-gateway-settings.md#gwsku).
5. Yapılandırma **yönlendirme türü**. Yönlendirme türü bu yapılandırma için **dinamik**. Ağ geçidini kaldırmak ve yeni bir tane oluşturun, yönlendirme türü daha sonra değiştiremezsiniz.
6. **Tamam** düğmesine tıklayın.
7. Üzerinde **yeni VPN bağlantısı** sayfasında **Tamam** sanal ağ geçidi oluşturmaya başlamak için. Bir ağ geçidinin oluşturulması, seçili ağ geçidi SKU’suna bağlı olarak 45 dakika veya daha uzun sürebilir.

## <a name="vnet4settings"></a>5. adım - testvnet4'ü ayarlarını yapılandırma

Adımları tekrar [bir yerel site oluşturma](#localsite) ve [sanal ağ geçidi oluşturma](#gw) testvnet4'ü, gerektiğinde değerleri değiştirerek yapılandırmak için. Bu bir alıştırma olarak yapıyor kullanırsanız [örnek değerleri](#vnetvalues).

## <a name="updatelocal"></a>6. adım - yerel siteleri güncelleştir

Sanal ağ geçitleri için her iki Vnet'in oluşturulduktan sonra yerel siteler kılınmasından **VPN ağ geçidi IP adresi** değerleri.

|Sanal ağ adı|Bağlantılı site|Ağ geçidi IP adresi|
|:--- |:--- |:--- |
|TestVNet1|VNet4Local|TestVNet4 için VPN ağ geçidi IP adresi|
|TestVNet4|VNet1Local|TestVNet1 için VPN ağ geçidi IP adresi|

### <a name="part-1---get-the-virtual-network-gateway-public-ip-address"></a>1. Kısım - sanal ağ geçidi genel IP adresini alma

1. Sanal ağınızı Azure portalında bulun.
2. Sanal Ağ'ı açmak için tıklayın **genel bakış** sayfası. Sayfasında, **VPN bağlantıları**, sanal ağ geçidiniz için IP adresini görüntüleyebilirsiniz.

   ![Genel IP](./media/vpn-gateway-howto-vnet-vnet-portal-classic/publicIP.png)
3. IP adresini kopyalayın. Sonraki bölümde kullanır.
4. TestVNet4 için bu adımları yineleyin

### <a name="part-2---modify-the-local-sites"></a>2. Kısım - yerel sitelerini değiştirmek

1. Sanal ağınızı Azure portalında bulun.
2. VNet üzerinde **genel bakış** sayfasında, yerel site'a tıklayın.

   ![Yerel site oluşturuldu](./media/vpn-gateway-howto-vnet-vnet-portal-classic/local.png)
3. Üzerinde **siteden siteye VPN bağlantıları** sayfasında, değiştirmek istediğiniz sitenin yerel adına tıklayın.

   ![Yerel site Aç](./media/vpn-gateway-howto-vnet-vnet-portal-classic/openlocal.png)
4. Tıklayın **yerel site** değiştirmek istediğiniz.

   ![Site değiştirme](./media/vpn-gateway-howto-vnet-vnet-portal-classic/connections.png)
5. Güncelleştirme **VPN ağ geçidi IP adresi** tıklatıp **Tamam** ayarları kaydetmek için.

   ![Ağ geçidi IP](./media/vpn-gateway-howto-vnet-vnet-portal-classic/gwupdate.png)
6. Diğer sayfalara kapatın.
7. TestVNet4 için bu adımları yineleyin.

## <a name="getvalues"></a>7. adım - ağ yapılandırma dosyasından değerleri alma

Azure portalında Klasik sanal ağları oluşturduğunuzda, görüntülediğiniz adı için PowerShell kullanan tam adı değil. Örneğin, adlandırılacak şekilde görünen bir VNet **TestVNet1** portalda, ağ yapılandırma dosyasında bir çok uzun bir ad olabilir. Ad şunun gibi görünür: **ClassicRG TestVNet1 grup**. Bağlantılarınızı oluşturduğunuzda, ağ yapılandırma dosyasında gördüğünüz değerleri kullanmak önemlidir.

Aşağıdaki adımlarda, Azure hesabınıza bağlanın ve indirip bağlantılarınız için gerekli olan değerleri almak için ağ yapılandırma dosyasını görüntüleyebilirsiniz.

1. Azure Hizmet Yönetimi (SM) PowerShell cmdlet'lerinin en son sürümünü indirip yeniden açın. Daha fazla bilgi için bkz. [Azure PowerShell’i yükleme ve yapılandırma](/powershell/azure/overview).

2. PowerShell konsolunuzu yükseltilmiş haklarla açın ve hesabınıza bağlanın. Bağlanmanıza yardımcı olması için aşağıdaki örneği kullanın:

   ```powershell
   Connect-AzAccount
   ```

   Hesapla ilişkili abonelikleri kontrol edin.

   ```powershell
   Get-AzSubscription
   ```

   Birden fazla aboneliğiniz varsa, kullanmak istediğiniz aboneliği seçin.

   ```powershell
   Select-AzSubscription -SubscriptionName "Replace_with_your_subscription_name"
   ```

   Ardından, Azure aboneliğiniz için PowerShell Klasik dağıtım modeli için eklemek için aşağıdaki cmdlet'i kullanın.

   ```powershell
   Add-AzureAccount
   ```
3. Dışarı aktarma ve ağ yapılandırma dosyasını görüntüleyin. Bilgisayarınızda bir dizin oluşturun ve sonra ağ yapılandırma dosyasını dizine aktarın. Bu örnekte, için ağ yapılandırma dosyasını dışa **C:\AzureNet**.

   ```powershell
   Get-AzureVNetConfig -ExportToFile C:\AzureNet\NetworkConfig.xml
   ```
4. Dosyayı bir metin düzenleyiciyle açın ve sanal ağlar ve siteler adlarını görüntüleyin. Bunlar, bağlantılarınızı oluştururken kullandığınız ad olacaktır.<br>Sanal ağ adları olarak listelenen **VirtualNetworkSite name =**<br>Site adları olarak listelenen **LocalNetworkSiteRef adı =**

## <a name="createconnections"></a>8. adım - VPN gateway bağlantıları oluşturma

Önceki tüm adımları tamamladıktan sonra IPSec/IKE önceden paylaşılan anahtarları ayarlayabilir ve bağlantıyı oluşturun. Bu adım kümesini PowerShell kullanır. Azure portalında Klasik dağıtım modeli için VNet-VNet bağlantılarında yapılandırılamaz.

Örneklerde paylaşılan anahtarı tam olarak aynı olduğuna dikkat edin. Paylaşılan anahtarı her zaman eşleşmelidir. Sanal ağlar ve yerel ağ siteleri için tam adları ile bu örneklerde değerleri değiştirdiğinizden emin olun.

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
* Sanal ağlar aynı veya farklı Aboneliklerde olabilir.
* Sanal ağlar aynı ya da farklı Azure bölgelerinde (konumlarında) bulunabilir.
* Bir bulut hizmeti ya da yük dengeleme uç noktası, birbirlerine bağlı olsa da sanal ağlara yayılamaz.
* Birden çok sanal ağları birbirine bağlama, tüm VPN cihazları gerektirmez.
* VNet-VNet Azure sanal ağları bağlamayı destekler. Bağlanan sanal makineleri desteklemez veya Bulut Hizmetleri, bir sanal ağa dağıtılmaz.
* VNet-VNet dinamik yönlendirme ağ geçitleri gerekir. Azure statik yönlendirme ağ geçitleri desteklenmez.
* Sanal ağ bağlantısı, çoklu site VPN’leri ile eşzamanlı olarak kullanılabilir. En fazla 10 VPN tünelleri ya da diğer sanal ağlar, veya şirket içi sitelere bağlanan sanal ağ VPN ağ geçidi için yoktur.
* Sanal ağlar ve şirket içi yerel ağ sitelerinin adres alanları çakışmamalıdır. Adres alanlarının çakışması oluşturulmasını sanal ağlara veya karşıya yükleme netcfg yapılandırma dosyalarını, başarısız olmasına neden olur.
* Bir çift sanal ağ arasındaki yedekli tüneller desteklenmez.
* P2S VPN'leri içeren bir VNet için tüm VPN tünelleri, VPN ağ geçidi ve aynı VPN ağ geçidi çalışma süresi SLA'sı üzerinde kullanılabilir bant genişliğini paylaşır.
* VNet-VNet trafiği Azure omurgası üzerinden dolaşır.

## <a name="next-steps"></a>Sonraki adımlar
Bağlantılarınızı doğrulayın. Bkz: [bir VPN Gateway bağlantısını doğrulama](vpn-gateway-verify-connection-resource-manager.md).
