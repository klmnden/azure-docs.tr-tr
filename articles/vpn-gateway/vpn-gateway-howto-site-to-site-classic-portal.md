---
title: "Şirket içi ağınızı bir Azure sanal ağına bağlama: Siteden Siteye VPN (klasik): Portal | Microsoft Docs"
description: "Şirket içi ağınız ile bir Azure sanal ağı arasında genel İnternet üzerinden bir IPSec bağlantısı oluşturma adımları. Bu adımlar portalı kullanarak Siteden Siteye şirket içi ve dışı karışık VPN Gateway bağlantısı oluşturmanıza yardımcı olur."
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: vpn-gateway
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/24/2017
ms.author: cherylmc
translationtype: Human Translation
ms.sourcegitcommit: b0c27ca561567ff002bbb864846b7a3ea95d7fa3
ms.openlocfilehash: fd7c834e8e061ba51b116ade88769dde05abcf9a
ms.lasthandoff: 04/25/2017


---
# <a name="create-a-site-to-site-connection-using-the-azure-portal-classic"></a>Azure portalını (klasik) kullanarak Siteden Siteye bağlantı oluşturma

Bu makalede, Azure portalını kullanarak şirket içi ağınızdan VNet’e Siteden Siteye VPN ağ geçidi bağlantısı oluşturma işlemi gösterilir. Bu makaledeki adımlar klasik dağıtım modeli için geçerlidir. Ayrıca aşağıdaki listeden farklı bir seçenek belirtip farklı bir dağıtım aracı veya dağıtım modeli kullanarak da bu yapılandırmayı oluşturabilirsiniz:

> [!div class="op_single_selector"]
> * [Resource Manager - Azure portalı](vpn-gateway-howto-site-to-site-resource-manager-portal.md)
> * [Resource Manager - PowerShell](vpn-gateway-create-site-to-site-rm-powershell.md)
> * [Resource Manager - CLI](vpn-gateway-howto-site-to-site-resource-manager-cli.md)
> * [Klasik - Azure portalı](vpn-gateway-howto-site-to-site-classic-portal.md)
> * [Klasik - klasik portal](vpn-gateway-site-to-site-create.md)
> 
>

![Siteden Siteye şirket içi ve dışı karışık VPN Gateway bağlantısı diyagramı](./media/vpn-gateway-howto-site-to-site-classic-portal/site-to-site-diagram.png)


Siteden Siteye VPN ağ geçidi bağlantısı, şirket içi ağınızı bir IPsec/IKE (IKEv1 veya IKEv2) tüneli üzerinden Azure sanal ağına bağlamak için kullanılır. Bu bağlantı türü için, şirket içinde yer alan ve kendisine atanmış dışarıya yönelik bir genel IP adresi atanmış olan bir VPN cihazı gerekir. VPN ağ geçitleri hakkında daha fazla bilgi için bkz. [VPN ağ geçidi hakkında](vpn-gateway-about-vpngateways.md).

## <a name="before-you-begin"></a>Başlamadan önce

Yapılandırmaya başlamadan önce aşağıdaki ölçütleri karşıladığınızı doğrulayın:

* Klasik dağıtım modeliyle çalışmak istediğinizi doğrulayın. [!INCLUDE [deployment models](../../includes/vpn-gateway-deployment-models-include.md)] 
* Uyumlu bir VPN cihazı ve bu cihazı yapılandırabilecek biri. Uyumlu VPN cihazları ve cihaz yapılandırması hakkında daha fazla bilgi için bkz.[VPN Cihazları Hakkında](vpn-gateway-about-vpn-devices.md).
* VPN cihazınız için dışarıya yönelik genel bir IPv4 IP adresi. Bu IP adresi bir NAT’nin arkasında olamaz.
* Şirket içi ağ yapılandırmanızda bulunan IP adresi aralıklarıyla ilgili fazla bilginiz yoksa size bu ayrıntıları sağlayabilecek biriyle çalışmanız gerekir. Bu yapılandırmayı oluşturduğunuzda, Azure’un şirket içi konumunuza yönlendireceği IP adres aralığı ön eklerini oluşturmanız gerekir. Şirket içi ağınızın alt ağlarından hiçbiri, bağlanmak istediğiniz sanal ağ alt ağlarıyla çakışamaz.
* Şu anda, ortak anahtarı belirtmek ve VPN ağ geçidi bağlantısını oluşturmak için PowerShell gereklidir. Azure Hizmet Yönetimi (SM) PowerShell cmdlet’lerinin en son sürümünü yükleyin. Daha fazla bilgi için bkz. [Azure PowerShell’i yükleme ve yapılandırma](/powershell/azureps-cmdlets-docs). Bu yapılandırma için PowerShell ile çalışırken yönetici olarak işlem yaptığınızdan emin olun. 

### <a name="values"></a>Bu alıştırma için örnek yapılandırma değerleri

Bu makaledeki örneklerde aşağıdaki değerler kullanılır. Bu değerleri kullanarak bir test ortamı oluşturabilir veya bu makaledeki örnekleri daha iyi anlamak için bunlara bakabilirsiniz.

* **VNet Name:** TestVNet1
* **Adres Alanı:** 
    * 10.11.0.0/16
    * 10.12.0.0/16 (bu alıştırma için isteğe bağlı)
* **Alt ağlar:**
  * FrontEnd: 10.11.0.0/24
  * BackEnd: 10.12.0.0/24 (bu alıştırma için isteğe bağlı)
* **GatewaySubnet:** 10.11.255.0/27
* **Kaynak Grubu:** TestRG1
* **Konum:** Doğu ABD
* **DNS Sunucusu:** 8.8.8.8 (bu alıştırma için isteğe bağlı)
* **Yerel site adı:** Site2

## <a name="CreatVNet"></a>1. Sanal ağ oluşturma

S2S bağlantısı için kullanılacak bir sanal ağ oluşturduğunuzda, belirttiğiniz adres alanlarının, bağlanmak istediğiniz yerel siteler için istemci adres alanlarından herhangi biriyle çakışmadığından emin olun. Çakışan alt ağlarınız varsa bağlantınız düzgün şekilde gerçekleşmeyebilir.

* Zaten bir sanal ağınız varsa, ayarların VPN ağ geçidi tasarımınızla uyumlu olduğunu doğrulayın. Diğer ağlarla çakışabilecek herhangi bir alt ağ olup olmadığına özellikle dikkat edin. 

* Sanal ağınız yoksa bir sanal ağ oluşturun. Ekran görüntüleri örnek olarak verilmiştir. Değerlerin kendinizinkilerle değiştirildiğinden emin olun.

### <a name="to-create-a-virtual-network"></a>Sanal ağ oluşturmak için

1. Tarayıcıdan [Azure portalına](http://portal.azure.com) gidin ve gerekiyorsa Azure hesabınızda oturum açın.
2. **Yeni**’ye tıklayın. **Markette ara** alanına 'Sanal Ağ' yazın. Döndürülen listeden **Sanal Ağ**’ı bulun ve tıklayarak **Sanal Ağ** dikey penceresini açın.

    ![Sanal ağ ara dikey penceresi](./media/vpn-gateway-howto-site-to-site-classic-portal/newvnetportal700.png)
3. Sanal Ağ dikey penceresinin altı yakınlarında, **Bir dağıtım modeli seçin** listesinden **Klasik**’i seçip **Oluştur**’a tıklayın.

    ![Dağıtım modeli seçme](./media/vpn-gateway-howto-site-to-site-classic-portal/selectmodel.png)
4. **Sanal ağ oluştur** dikey penceresinde sanal ağ ayarlarını yapılandırın. Bu dikey pencerede, ilk adres alanınızı ve tek alt ağ adres aralığınızı eklersiniz. Sanal ağ oluşturma işlemini tamamladıktan sonra geri dönüp ek alt ağları ve adres alanlarını ekleyin.

    ![Sanal ağ oluşturma dikey penceresi](./media/vpn-gateway-howto-site-to-site-classic-portal/createvnet.png "Create virtual network blade")
5. **Abonelik** alanında doğru bir giriş olduğunu doğrulayın. Açılan listeyi kullanarak abonelikleri değiştirebilirsiniz.
6. **Kaynak grubu**’na tıklayın, ya varolan bir kaynak grubunu seçin ya da yeni kaynak grubunuz için bir ad yazarak yeni bir tane oluşturun. Kaynak grupları hakkında daha fazla bilgi için [Azure Resource Manager’a Genel Bakış](../azure-resource-manager/resource-group-overview.md#resource-groups)’ı ziyaret edin.
7. Ardından, sanal ağınız için **Konum** ayarlarını seçin. Konum bu sanal ağa dağıttığınız kaynakların nerede olacağını belirler.
8. Sanal ağınızı panoda kolay bulmak istiyorsanız **Panoya sabitle**’yi seçin ve ardından **Oluştur**’a tıklayın.

    ![Panoya sabitle](./media/vpn-gateway-howto-site-to-site-classic-portal/pintodashboard150.png "Pin to dashboard")
9. “Oluştur”a tıkladıktan sonra, panoda sanal ağınızın ilerleme durumunu yansıtan bir kutucuk görünür. Sanal ağ oluşturulurken kutucuk değişir.

    ![Sanal ağ oluşturma kutucuğu](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/deploying150.png "Creating virtual network")

Sanal ağınız oluşturulduğunda, klasik Azure portalının ağlar sayfasındaki **Durum** seçeneğinin altında **Oluşturuldu** ifadesinin yer aldığını görürsünüz.

## <a name="additionaladdress"></a>2. Başka adres alanı ekleme

Sanal ağınızı oluşturduktan sonra başka adres alanı ekleyebilirsiniz. Başka adres alanı eklenmesi S2S yapılandırmasının gerekli bir kısmı değildir, ancak birden fazla adres alanı gerekliyse aşağıdaki adımları kullanın:

1. Portalda sanal ağları bulun.
2. Sanal ağınızın dikey penceresindeki **Ayarlar** bölümünde **Adres alanı**’na tıklayın.
3. Adres alanı dikey penceresinde **+Ekle**’ye tıklayın ve diğer adres alanını girin.
 
## <a name="dns"></a>3. DNS sunucusu belirleme
DNS ayarları, S2S yapılandırmasının gerekli bir kısmı değildir, ancak ad çözünürlüğü istiyorsanız DNS gereklidir.

Sanal ağınızı oluşturduktan sonra ad çözünürlüğünü işlemek için bir DNS sunucusunun IP adresini ekleyebilirsiniz. Sanal ağınızın ayarlarını açın, DNS sunucularına tıklayın ve ad çözünürlüğü kullanmak istediğiniz DNS sunucusunun IP adresini ekleyin. Bu ayarla bir DNS sunucusu oluşturulmaz. Örnek ayarlarda bir ortak DNS sunucusu kullanılmıştır. Genellikle özel bir DNS sunucusu kullanmak istemezsiniz. Kaynaklarınızın iletişim kurabileceği bir DNS sunucusu eklediğinizden emin olun.

1. Portalda sanal ağları bulun.
2. Sanal ağınızın dikey penceresindeki **Ayarlar** bölümünde **DNS sunucuları**’na tıklayın.
3. Bir DNS sunucusu ekleyin.
4. Ayarlarınızı kaydetmek için sayfanın en üstünde yer alan **Kaydet**’e tıklayın.
 
## <a name="localsite"></a>4. Yerel siteyi yapılandırma

Yerel site genellikle şirket içi konumunuzu ifade eder. Bağlantı oluşturacağınız VPN cihazının IP adresini ve VPN ağ geçidi üzerinden VPN cihazına yönlendirilecek IP aralıklarını içerir.

1. Portalda, ağ geçidi oluşturmak istediğiniz sanal ağa gidin.
2. Sanal ağınızın dikey penceresindeki **Genel Bakış** dikey penceresinin VPN bağlantıları bölümünde **Ağ Geçidi**’ne tıklayarak **Yeni VPN Bağlantısı** dikey penceresini açın.

    ![Ağ geçidi ayarlarını yapılandırmak için tıklayın](./media/vpn-gateway-howto-site-to-site-classic-portal/beforegw125.png "Click to configure gateway settings")
3. **Yeni VPN Bağlantısı** dikey penceresinde **Siteden siteye** öğesini seçin.

    ![Siteden siteye](./media/vpn-gateway-howto-site-to-site-classic-portal/site-to-site.png "site-to-site") öğesine tıklayın
4. **Yerel site - Gerekli ayarları yapılandırın**’a tıklayarak **Yerel site** dikey penceresini açın. Ayarları yapılandırın ve ardından **Tamam**’a tıklayarak ayarları kaydedin.
    - **Ad:** Tanımlamanızı kolaylaştırmak için yerel sitenize ait bir ad oluşturun.
    - **VPN ağ geçidi IP adresi:** Şirket içi ağınızdaki VPN cihazının genel IP adresidir. VPN cihazı, IPv4 genel IP adresi gerektirir. Bağlanmak istediğiniz VPN cihazı için geçerli bir genel IP adresi belirtin. NAT’nin ardında olamaz ve Azure tarafından erişilebilir olması gerekir.
    - **İstemci Adres alanı:** Bu ağ geçidi aracılığıyla yerel şirket içi ağınıza yönlendirilmesini istediğiniz IP adres aralıklarını listeleyin. Birden fazla adres alanı aralığı ekleyebilirsiniz. Burada belirttiğiniz aralıkların, sanal ağınızın bağlandığı diğer ağlarla ve sanal ağın kendi adres aralıklarıyla çakışmadığından emin olun.

    ![Yerel site](./media/vpn-gateway-howto-site-to-site-classic-portal/localnetworksite.png "Configure local site")

## <a name="gatewaysubnet"></a>5. Ağ geçidi alt ağını yapılandırma

VPN ağ geçidiniz için bir ağ geçidi alt ağı oluşturmanız gerekir. Ağ geçidi alt ağı, VPN ağ geçidi hizmetlerinin kullandığı IP adreslerini içerir.

1. **Yeni VPN Bağlantısı** dikey penceresinde, **Ağ geçidini hemen oluştur** onay kutusunu işaretleyin. 'İsteğe bağlı ağ geçidi yapılandırması' dikey penceresi görüntülenir. Onay kutusunu seçmezseniz, ağ geçidi alt ağını yapılandırmak için dikey pencereyi görmezsiniz.

    ![Ağ geçidi yapılandırması - Alt ağ, boyut, yönlendirme türü](./media/vpn-gateway-howto-site-to-site-classic-portal/optional.png "Gateway configuration - Subnet, size, routing type")
2. **İsteğe bağlı ağ geçidi yapılandırması - Alt ağ, boyut ve yönlendirme türü**’ne tıklayarak **Ağ geçidi yapılandırması** dikey penceresini açın.
3. **Ağ Geçidi Yapılandırması** dikey penceresinde **Alt Ağ - Gerekli ayarları yapılandırın** öğesine tıklayarak **Alt ağ ekle** dikey penceresini açın.

    ![Ağ geçidi yapılandırması - ağ geçidi alt ağı](./media/vpn-gateway-howto-site-to-site-classic-portal/subnetrequired.png "Gateway configuration - gateway subnet")
4. **Alt ağ ekle** dikey penceresinde ağ geçidi alt ağını ekleyin. Belirttiğiniz ağ geçidi alt ağının boyutu, oluşturmak istediğiniz VPN ağ geçidi yapılandırmasına bağlıdır. /29 kadar küçük bir ağ geçidi alt ağı oluşturmak mümkün olsa da, /27 veya /28’i seçerek daha fazla adres içeren büyük bir alt ağ oluşturmanızı öneririz. Daha büyük bir ağ geçidi alt ağı kullanmak, olası gelecek yapılandırmaları barındırmak için yeterli IP adresi bulunmasını sağlar.

    ![Ağ geçidi alt ağı ekleme](./media/vpn-gateway-howto-site-to-site-classic-portal/addgwsubnet.png "Add gateway subnet")

## <a name="sku"></a>6. SKU ve VPN türünü belirtme
1. Ağ geçidi **Boyutu** seçin. Bu seçenek, sanal ağ geçidinizi oluşturmak için kullandığınız ağ geçidi SKU’sudur. Portalda, 'Varsayılan SKU' = **Temel** şeklindedir. Ağ geçidi SKU’ları hakkında bilgi için bkz. [VPN Gateway Ayarları Hakkında](vpn-gateway-about-vpn-gateway-settings.md#gwsku).

    ![SKUL ve VPN türü seçme](./media/vpn-gateway-howto-site-to-site-classic-portal/sku.png "Select SKU and VPN type")
2. Ağ geçidiniz için **Yönlendirme Türü** seçin. Bu seçenek VPN türü olarak da bilinir. Ağ geçidini bir türden diğerine geçiremeyeceğiniz için doğru ağ geçidi türünün seçilmesi önemlidir. VPN cihazınız seçtiğiniz yönlendirme türü ile uyumlu olmalıdır. VPN türü hakkında daha fazla bilgi için bkz. [VPN Gateway Ayarları hakkında](vpn-gateway-about-vpn-gateway-settings.md#vpntype). 'RouteBased' ve 'PolicyBased' VPN türlerine başvuran makaleler görebilirsiniz. 'Dinamik' seçeneği 'RouteBased', 'Statik' seçeneği ise 'PolicyBased' değerine karşılık gelir.
3. Ayarları kaydetmek için **Tamam**’a tıklayın.
4. **Yeni VPN Bağlantısı** dikey penceresinde, sanal ağ geçidinizi oluşturmaya başlamak için dikey pencerenin en altındaki **Tamam**’a tıklayın. Bu işlemin tamamlanması 45 dakika sürebilir.

## <a name="vpndevice"></a>7. VPN cihazınızı yapılandırma

[!INCLUDE [vpn-gateway-configure-vpn-device-rm](../../includes/vpn-gateway-configure-vpn-device-rm-include.md)]

## <a name="CreateConnection"></a>8. Bağlantı oluşturma
Bu adımda, paylaşılan anahtarı ayarlayabilir ve bağlantıyı oluşturabilirsiniz. Ayarladığınız anahtar, VPN cihazı yapılandırmasında kullanılan anahtarla aynı olmalıdır.

> [!NOTE]
> Şu anda, bu yapılandırma Azure portalında mevcut değildir. Azure PowerShell cmdlet’lerinin Hizmet Yönetimi (SM) sürümünü kullanmanız gerekir.
>

### <a name="step-1-connect-to-your-azure-account"></a>1. Adım Azure hesabınıza bağlanma

1. PowerShell konsolunuzu yükseltilmiş haklarla açın ve hesabınıza bağlanın. Bağlanmanıza yardımcı olması için aşağıdaki örneği kullanın:

  ```powershell
  Login-AzureRmAccount
  ```
2. Hesapla ilişkili abonelikleri kontrol edin.

  ```powershell
  Get-AzureRmSubscription
  ```
3. Birden fazla aboneliğiniz varsa, kullanmak istediğiniz aboneliği seçin.

  ```powershell
  Select-AzureRmSubscription -SubscriptionName "Replace_with_your_subscription_name"
  ```
4. PowerShell cmdlet'lerinin SM sürümünü ekleyin.

  ```powershell
  Add-AzureAccount
  ```

### <a name="step-2-set-the-shared-key-and-create-the-connection"></a>2. Adım Paylaşılan anahtarı ayarlama ve bağlantıyı oluşturma

PowerShell ve klasik dağıtım modeli ile çalışırken, portaldaki kaynakların adları bazı durumlarda Azure’un PowerShell kullanırken görmeyi beklediği adlar olmaz. Aşağıdaki adımlar, adların tam değerlerini almak için ağ yapılandırma dosyasını dışarı aktarmanıza yardımcı olur.

1. Bilgisayarınızda bir dizin oluşturun ve sonra ağ yapılandırma dosyasını dizine aktarın. Bu örnekte, ağ yapılandırma dosyası C:\AzureNet dizinine aktarılır.

  ```powershell
  Get-AzureVNetConfig -ExportToFile C:\AzureNet\NetworkConfig.xml
  ```
2. Ağ yapılandırma dosyasını bir xml düzenleyicisi ile açın ve 'LocalNetworkSite name' ile 'VirtualNetworkSite name' değerlerini denetleyin. Örneği, değerleri yansıtacak şekilde değiştirin. Boşluk içeren bir ad belirtirken, değeri tek tırnak işaretleri içine alın.

3. Paylaşılan anahtarı ayarlayıp bağlantıyı oluşturun. '-SharedKey' sizin oluşturup belirttiğiniz bir değerdir. Örnekte 'abc123' değeri kullanılmıştır, ancak siz daha karmaşık bir değer oluşturabilirsiniz (ve oluşturmalısınız). Önemli olan, burada belirttiğiniz değerin VPN cihazınızı yapılandırırken belirttiğiniz değerle aynı olmasıdır.

  ```powershell
  Set-AzureVNetGatewayKey -VNetName 'Group TestRG1 TestVNet1' `
  -LocalNetworkSiteName 'D1BFC9CB_Site2' -SharedKey abc123
  ```
Bağlantı oluşturulduğunda, sonuç şu şekildedir: **Durum: Başarılı**.

## <a name="verify"></a>9. Bağlantınızı doğrulama

[!INCLUDE [vpn-gateway-verify-connection-azureportal-classic](../../includes/vpn-gateway-verify-connection-azureportal-classic-include.md)]

## <a name="next-steps"></a>Sonraki adımlar
Bağlantınız tamamlandıktan sonra sanal ağlarınıza sanal makineler ekleyebilirsiniz. Daha fazla bilgi için bkz. [Sanal Makineler](https://docs.microsoft.com/azure/#pivot=services&panel=Compute).


