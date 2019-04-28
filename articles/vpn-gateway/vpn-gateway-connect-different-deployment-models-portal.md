---
title: 'Klasik sanal ağlar, Azure Resource Manager sanal ağlarına bağlama: Portal | Microsoft Docs'
description: Klasik sanal ağları Resource Manager VPN ağ geçidi ve portalını kullanarak sanal ağlara bağlanmak için adımları
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: conceptual
ms.workload: infrastructure-services
ms.date: 10/17/2018
ms.author: cherylmc
ms.openlocfilehash: bf7d80bbbe63204cda47719a7d7c019013ad800b
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62124042"
---
# <a name="connect-virtual-networks-from-different-deployment-models-using-the-portal"></a>Portalı kullanarak farklı dağıtım modellerindeki sanal ağları bağlama

Bu makalede, Klasik sanal ağlar birbiriyle iletişim kurmak için ayrı dağıtım modellerindeki kaynaklara izin vermek için Resource Manager sanal ağlarına bağlanma işlemini göstermektedir. Bu makaledeki adımlarda, öncelikle Azure portalını kullanın, ancak ayrıca makalede bu listeden seçerek PowerShell kullanarak bu yapılandırmayı oluşturabilirsiniz.

> [!div class="op_single_selector"]
> * [Portal](vpn-gateway-connect-different-deployment-models-portal.md)
> * [PowerShell](vpn-gateway-connect-different-deployment-models-powershell.md)
> 
> 

İçin Resource Manager Vnet'i klasik bir VNet bağlama VNet bir şirket içi site konumuna bağlamakla aynıdır. Her iki bağlantı türü de IPsec/IKE kullanarak güvenli bir tünel sunmak üzere bir VPN ağ geçidi kullanır. Farklı Aboneliklerdeki ve farklı bölgelerdeki sanal ağlar arasında bir bağlantı oluşturabilirsiniz. Dinamik ya da rota tabanlı ağ geçidi ile yapılandırılmış olduğu sürece şirket içi ağlara bağlantıları olan sanal ağlar da bağlanabilirsiniz. Sanal ağlar arası bağlantılar hakkında daha fazla bilgi için bu makalenin sonunda yer alan [Sanal ağlar arası bağlantılar hakkında SSS](#faq) bölümünü inceleyin. 

Zaten bir sanal ağ geçidi yok ve oluşturmak istemiyorsanız, bunun yerine kullanarak VNet eşlemesi, sanal ağları bağlama düşünün isteyebilirsiniz. VNet eşlemesi VPN ağ geçidini kullanmaz. Daha fazla bilgi için bkz. [VNet eşlemesi](../virtual-network/virtual-network-peering-overview.md).

### <a name="before"></a>Başlamadan önce

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

* Bu adımlar, her iki Vnet'in zaten oluşturulmuş olduğunu varsayar. Bu makalede bir alıştırma olarak kullanıyorsanız ve sanal ağlar yoksa, bunları oluşturmanıza yardımcı olması için adımlarda bağlantıları vardır.
* Sanal ağlar için adres aralıklarını değil ile çakıştığı için ağ geçitlerinin bağlı diğer bağlantılar aralıkların hiçbiriyle olduğunu doğrulayın veya.
* Resource Manager ve Hizmet Yönetimi (Klasik) için en son PowerShell cmdlet'lerini yükleyin. Bu makalede, Azure portalı ve PowerShell'de kullanırız. PowerShell için Resource Manager Vnet'i Klasik sanal ağdan bağlantı oluşturmak için gereklidir. Daha fazla bilgi için bkz. [Azure PowerShell’i yükleme ve yapılandırma](/powershell/azure/overview). 

### <a name="values"></a>Örnek ayarlar

Bu değerleri kullanarak bir test ortamı oluşturabilir veya bu makaledeki örnekleri daha iyi anlamak için bunlara bakabilirsiniz.

**Klasik sanal ağ**

VNet adı ClassicVNet = <br>
Adres alanı 10.0.0.0/24 = <br>
Alt ağ adı Subnet-1 = <br>
Alt ağ adres aralığı 10.0.0.0/27 = <br>
Abonelik kullanmak istediğiniz aboneliği = <br>
Resource Group = ClassicRG <br>
Konum Batı ABD = <br>
GatewaySubnet 10.0.0.32/28 = <br>
Yerel site RMVNetLocal = <br>

**Resource Manager VNet**

VNet adı RMVNet = <br>
Adres alanı 192.168.0.0/16 = <br>
Kaynak grubu RG1 = <br>
Konumu Doğu ABD = <br>
Alt ağ adı Subnet-1 = <br>
Adres aralığı 192.168.1.0/24 = <br>
GatewaySubnet 192.168.0.0/26 = <br>
Sanal ağ geçidi adı RMGateway = <br>
Ağ geçidi türü VPN = <br>
VPN türü = rota tabanlı <br>
SKU = VpnGw1 <br>
Konumu Doğu ABD = <br>
Sanal ağ RMVNet = <br> (Bu sanal ağ VPN ağ geçidine ilişkilendirmek) İlk IP yapılandırması rmgwpip = <br> (ağ geçidi genel IP adresi) Yerel ağ geçidi ClassicVNetLocal = <br>
Bağlantı adı RMtoClassic =

### <a name="connectoverview"></a>Genel bakış

Bu yapılandırma için sanal ağlar arasında bir IPSec/IKE VPN tüneli üzerinden bir VPN ağ geçidi bağlantısı oluşturun. Sanal ağ Aralıklarınızın hiçbir birbiriyle veya bağlandıkları yerel ağlar biriyle çakışıyor emin olun.

Aşağıdaki tabloda örnek sanal ağlar ve yerel siteleri nasıl tanımlandığı bir örnek gösterilmektedir:

| Sanal Ağ | Adres Alanı | Bölge | Yerel ağ alanına bağlanır. |
|:--- |:--- |:--- |:--- |
| ClassicVNet |(10.0.0.0/24) |Batı ABD | RMVNetLocal (192.168.0.0/16) |
| RMVNet | (192.168.0.0/16) |Doğu ABD |ClassicVNetLocal (10.0.0.0/24) |

## <a name="classicvnet"></a>1. Bölüm - Klasik sanal ağ ayarlarını yapılandırma

Bu bölümde, oluşturduğunuz Klasik VNet yerel ağ (yerel site) ve sanal ağ geçidi. Ekran görüntüleri örnek olarak verilmiştir. Değerleri kendi değerlerinizle değiştirin veya bunları kullanmanızı mutlaka [örnek](#values) değerleri.

### 1. <a name="classicvnet"></a>Klasik sanal ağ oluşturma

Klasik sanal ağ yok ve bu adımları bir alıştırma olarak çalıştırıyorsanız, kullanarak bir sanal ağ oluşturabilirsiniz [bu makalede](../virtual-network/virtual-networks-create-vnet-classic-pportal.md) ve [örnek](#values) Yukarıdaki ayarları değerleri.

Bir sanal ağ VPN ağ geçidi ile zaten varsa, ağ geçidini dinamik olduğunu doğrulayın. Statik ise, için devam etmeden önce VPN ağ geçidi silmelisiniz [yerel siteyi yapılandırma](#local).

1. [Azure Portal](https://ms.portal.azure.com)'ı açın ve Azure hesabınızla oturum açın.
2. Tıklayın **+ kaynak Oluştur** 'New' sayfasını açın.
3. 'Markette Ara' alanına 'Sanal ağ' yazın. Bunun yerine, ağı seçerseniz -> sanal ağ, klasik bir VNet oluşturmak için bu seçeneği almazsınız.
4. Döndürülen listeden 'Sanal ağ' bulun ve sanal ağ sayfasını açmak için tıklayın. 
5. Sanal ağınızın sayfasında Klasik VNet oluşturmak için ' Klasik' seçin. Varsayılan burada izlerseniz, bir Resource Manager sanal ağı ile bunun yerine Rüzgar.

### 2. <a name="local"></a>Yerel siteyi yapılandırma

1. Gidin **tüm kaynakları** bulun **ClassicVNet** listesinde.
2. Üzerinde **genel bakış** sayfasında **VPN bağlantıları** bölümünde **ağ geçidi** bir ağ geçidi oluşturmak için.
  ![Bir VPN ağ geçidi yapılandırma](./media/vpn-gateway-connect-different-deployment-models-portal/gatewaygraphic.png "bir VPN ağ geçidi yapılandırma")
3. Üzerinde **yeni VPN bağlantısı** sayfası için **bağlantı türü**seçin **siteden siteye**.
4. İçin **yerel site**, tıklayın **gerekli ayarları Yapılandır**. Bu açılır **yerel site** sayfası.
5. Üzerinde **yerel site** sayfasında, Resource Manager sanal ağa başvurmak için bir ad oluşturun. Örneğin, 'RMVNetLocal'.
6. VPN ağ geçidi için Resource Manager sanal ağı zaten bir genel IP adresi değeri kullanırsanız **VPN ağ geçidi IP adresi** alan. Bu adımları bir alıştırma olarak uygulamadan olan ya da Resource Manager sanal ağınıza ait sanal ağ geçidi henüz yoksa bir yer tutucu IP adresini yapabilirsiniz. Yer tutucu IP adresi geçerli bir biçim kullandığından emin olun. Daha sonra yer tutucu IP adresi Resource Manager sanal ağ geçidi genel IP adresiyle değiştirin.
7. İçin **istemci adres alanı**, kullanın [değerleri](#connectoverview) Resource Manager sanal ağı için sanal ağ IP adresi alanları. Bu ayar, Resource Manager sanal ağına yönlendirmek için adres alanlarını belirtmek için kullanılır. Örnekte, 192.168.0.0/16, adres aralığı RMVNet için kullanırız.
8. Tıklayın **Tamam** değerleri kaydedin ve dönmek için **yeni VPN bağlantısı** sayfası.

### <a name="classicgw"></a>3. Sanal ağ geçidini oluşturma

1. Üzerinde **yeni VPN bağlantısı** sayfasında **ağ geçidini hemen Oluştur** onay kutusu.
2. **İsteğe bağlı ağ geçidi yapılandırması**’na tıklayarak **Ağ geçidi yapılandırması** sayfasını açın.

   ![Açık gateway yapılandırma sayfasında](./media/vpn-gateway-connect-different-deployment-models-portal/optionalgatewayconfiguration.png "açık ağ geçidi yapılandırma sayfası")
3. Tıklayın **alt ağ - gerekli ayarları Yapılandır** açmak için **alt ağ Ekle** sayfası. **Adı** gerekli değeri ile zaten yapılandırıldı: **GatewaySubnet**.
4. **Adres aralığı** ağ geçidi alt ağı için aralığına başvurur. İle/29 bir ağ geçidi alt ağı oluşturmanız mümkün olsa da adres aralığı (3 adresleri), daha fazla IP adresi içeren bir ağ geçidi alt ağı oluşturma öneririz. Bu, daha fazla kullanılabilir IP adresleri gerektirebilir gelecek yapılandırmaları barındırmak. Mümkünse, / 27 veya/28'i kullanın. Bu adımları bir alıştırma olarak kullanırken, başvurabilirsiniz [örnek değerleri](#values). Bu örnekte, '10.0.0.32/28' kullanırız. Tıklayın **Tamam** ağ geçidi alt ağı oluşturmak için.
5. Üzerinde **ağ geçidi Yapılandırması** sayfasında **boyutu** ağ geçidi SKU'sunu ifade eder. VPN ağ geçidiniz için ağ geçidi SKU'sunu seçin.
6. Doğrulayın **yönlendirme türü** olduğu **dinamik**, ardından **Tamam** dönmek için **yeni VPN bağlantısı** sayfası.
7. Üzerinde **yeni VPN bağlantısı** sayfasında **Tamam** VPN ağ geçidinizi oluşturmaya başlamak için. Bir VPN ağ geçidinin oluşturulması 45 dakika sürebilir.

### <a name="ip"></a>4. Sanal ağ geçidi genel IP adresini kopyalayın

Sanal ağ geçidi oluşturulduktan sonra ağ geçidi IP adresini görüntüleyebilirsiniz. 

1. Klasik sanal ağınıza gidin ve tıklayın **genel bakış**.
2. Tıklayın **VPN bağlantıları** VPN bağlantılar sayfasını açın. VPN bağlantıları sayfasında, genel IP adresini görüntüleyebilirsiniz. Bu, sanal ağ geçidine atanan genel IP adresidir. IP adresini not edin. Resource Manager yerel ağ geçidi yapılandırma ayarlarınızı ile çalışırken, sonraki adımlarda kullanın. 
3. Ağ geçidi bağlantılarınızı durumunu görüntüleyebilirsiniz. Oluşturduğunuz yerel ağ alanına 'Bağlanıyor' olarak listelenen dikkat edin. Bağlantılarınızı oluşturduktan sonra durumu değişir. İşiniz bittiğinde durumu görüntüleme için bu sayfayı kapatabilirsiniz.

## <a name="rmvnet"></a>2. Bölüm - Resource Manager sanal ağ ayarlarını yapılandırma

Bu bölümde, Resource Manager sanal ağınıza ait sanal ağ geçidi ve yerel ağ geçidi oluşturun. Ekran görüntüleri örnek olarak verilmiştir. Değerleri kendi değerlerinizle değiştirin veya bunları kullanmanızı mutlaka [örnek](#values) değerleri.

### <a name="1-create-a-virtual-network"></a>1. Sanal ağ oluşturma

**Örnek değerler:**

* VNet adı RMVNet = <br>
* Adres alanı 192.168.0.0/16 = <br>
* Kaynak grubu RG1 = <br>
* Konumu Doğu ABD = <br>
* Alt ağ adı Subnet-1 = <br>
* Adres aralığı 192.168.1.0/24 = <br>


Resource Manager Vnet'i yoksa ve bu adımları bir alıştırma olarak çalıştırıyorsanız, sanal ağ oluşturma adımları ile [sanal ağ oluşturma](../virtual-network/quick-create-portal.md), örnek değerleri kullanarak.

### <a name="2-create-a-gateway-subnet"></a>2. Ağ geçidi alt ağı oluşturma

**Örnek değer:** GatewaySubnet 192.168.0.0/26 =

Bir sanal ağ geçidi oluşturmadan önce ilk olarak ağ geçidi alt ağını oluşturmanız gerekir. CIDR sayısıyla 28 ya da daha büyük bir ağ geçidi alt ağı oluşturun (/ 27, / 26, vb..). Bu bölümü bir alıştırma olarak oluşturuyorsanız örnek değerleri kullanabilirsiniz.

[!INCLUDE [vpn-gateway-add-gwsubnet-rm-portal](../../includes/vpn-gateway-add-gwsubnet-rm-portal-include.md)]

[!INCLUDE [vpn-gateway-no-nsg-include](../../includes/vpn-gateway-no-nsg-include.md)]

### <a name="creategw"></a>3. Sanal ağ geçidi oluşturma

**Örnek değerler:**

* Sanal ağ geçidi adı RMGateway = <br>
* Ağ geçidi türü VPN = <br>
* VPN türü = rota tabanlı <br>
* SKU = VpnGw1 <br>
* Konumu Doğu ABD = <br>
* Sanal ağ RMVNet = <br>
* İlk IP yapılandırması rmgwpip = <br>

[!INCLUDE [vpn-gateway-add-gw-rm-portal](../../includes/vpn-gateway-add-gw-rm-portal-include.md)]

### <a name="createlng"></a>4. Yerel ağ geçidi oluşturma

**Örnek değerler:** Yerel ağ geçidi ClassicVNetLocal =

| Sanal Ağ | Adres Alanı | Bölge | Yerel ağ alanına bağlanır. |Ağ geçidi genel IP adresi|
|:--- |:--- |:--- |:--- |:--- |
| ClassicVNet |(10.0.0.0/24) |Batı ABD | RMVNetLocal (192.168.0.0/16) |ClassicVNet ağ geçidine atanan genel IP adresi|
| RMVNet | (192.168.0.0/16) |Doğu ABD |ClassicVNetLocal (10.0.0.0/24) |RMVNet ağ geçidine atanan genel IP adresi.|

Yerel ağ geçidi adres aralığını ve klasik sanal ağınıza, sanal ağ geçidi ile ilişkili genel IP adresini belirtir. Bu adımları bir alıştırma olarak devreye alıyorsanız örnek değerleri bakın.

[!INCLUDE [vpn-gateway-add-lng-rm-portal](../../includes/vpn-gateway-add-lng-rm-portal-include.md)]

## <a name="modifylng"></a>3. Bölüm - Klasik sanal ağ yerel sitesi ayarlarını değiştirme

Bu bölümde, yerel site ayarları ile Resource Manager VPN ağ geçidi IP adresi belirtirken kullanılan yer tutucu IP adresini değiştirin. Bu bölümde, Klasik (SM) PowerShell cmdlet'lerini kullanır.

1. Azure portalında, klasik bir sanal ağa gidin.
2. Sanal ağınızın sayfasında tıklayın **genel bakış**.
3. İçinde **VPN bağlantıları** bölümünde, yerel grafiği sitenizdeki adına tıklayın.

   ![VPN bağlantıları](./media/vpn-gateway-connect-different-deployment-models-portal/vpnconnections.png "VPN bağlantıları")
4. Üzerinde **siteden siteye VPN bağlantıları** sayfasında, sitenin adını tıklayın.

   ![Site adı](./media/vpn-gateway-connect-different-deployment-models-portal/sitetosite3.png "yerel site adı")
5. Yerel sitesinin bağlantısı sayfasında açmak için yerel site adına **yerel site** sayfası.

   ![Açık yerel site](./media/vpn-gateway-connect-different-deployment-models-portal/openlocal.png "yerel site Aç")
6. Üzerinde **yerel site** sayfasında, yerine **VPN ağ geçidi IP adresi** Resource Manager ağ geçidi IP adresi ile.

   ![Ağ geçidi IP adresini](./media/vpn-gateway-connect-different-deployment-models-portal/gwipaddress.png "ağ geçidi IP adresi")
7. Tıklayın **Tamam** IP adresini güncelleştirin.

## <a name="RMtoclassic"></a>4. Bölüm - Resource Manager'ı Klasik bağlantı oluşturun

Bu adımlarda, Azure portalını kullanarak Klasik VNet ile Resource Manager Vnet'i bağlantı yapılandırın.

1. İçinde **tüm kaynakları**, yerel ağ geçidini bulun. Bizim örneğimizde, yerel ağ geçididir **ClassicVNetLocal**.
2. Tıklayın **yapılandırma** ve klasik vnet VPN ağ geçidi IP adresi değeri olduğundan emin olun. Gerekirse, güncelleştirin, ardından tıklatın **Kaydet**. Sayfayı kapatın.
3. İçinde **tüm kaynakları**, yerel ağ geçidine tıklayın.
4. Tıklayın **bağlantıları** Bağlantılar sayfasını açın.
5. Üzerinde **bağlantıları** sayfasında **+** bağlantı eklemek için.
6. Üzerinde **Bağlantı Ekle** sayfasında, bağlantının adı. Örneğin, 'RMtoClassic'.
7. **Siteden siteye** bu sayfada zaten seçildi.
8. Bu site ile ilişkilendirmek istediğiniz sanal ağ geçidi seçin.
9. Oluşturma bir **paylaşılan anahtar**. Bu anahtar için Resource Manager Vnet'i Klasik sanal ağdan oluşturduğunuz bağlantı da kullanılır. Anahtarı oluşturmak veya biri olun. Örnekte 'abc123' kullanılır, ancak daha karmaşık bir şey kullanmak kullanabilirsiniz ve kullanmalısınız.
10. Tıklayın **Tamam** bağlantı oluşturmak için.

## <a name="classictoRM"></a>5. Bölüm - Klasik'ten Resource Manager bağlantı oluşturma

Bu adımlarda, Resource Manager Vnet'i Klasik VNet arasında bağlantı yapılandırın. Bu adımlar PowerShell gerektirir. Portalda, bu bağlantı oluşturamazsınız. İndirilen ve hem Klasik (SM) hem de kaynak yöneticisi (RM) PowerShell cmdlet'lerinin yüklü olduğundan emin olun.

### <a name="1-connect-to-your-azure-account"></a>1. Azure hesabınıza bağlanma

Yükseltilmiş haklarla PowerShell konsolu açın ve Azure hesabınızda oturum açın. Azure PowerShell için kullanılabilir olacak şekilde açtıktan sonra hesap ayarlarınızı indirilir. Aşağıdaki cmdlet sizden, Resource Manager dağıtım modeli için Azure hesabınız için oturum açma kimlik bilgileri ister:

```powershell
Connect-AzAccount
```

Azure aboneliklerinizin bir listesini alın.

```powershell
Get-AzSubscription
```

Birden fazla aboneliğiniz varsa, kullanmak istediğiniz aboneliği belirtin.

```powershell
Select-AzSubscription -SubscriptionName "Name of subscription"
```

Ardından, Klasik PowerShell cmdlet'leri (Hizmet Yönetimi) kullanmak için oturum açın. Klasik dağıtım modeli için Azure hesabınızı eklemek için aşağıdaki komutu kullanın:

```powershell
Add-AzureAccount
```

Aboneliklerinizin listesini alın. Hizmet Yönetimi cmdlet'leri, Azure modülünüzde bağlı olarak ekleme yüklediğinizde bu adım gerekli olabilir.

```powershell
Get-AzureSubscription
```

Birden fazla aboneliğiniz varsa, kullanmak istediğiniz aboneliği belirtin.

```powershell
Select-AzureSubscription -SubscriptionName "Name of subscription"
```

### <a name="2-view-the-network-configuration-file-values"></a>2. Ağ yapılandırma dosyası değerlerini görüntüleme

Azure portalında sanal ağ oluşturduğunuzda, Azure kullanan tam adı Azure Portalı'nda görünür değil. Örneğin, Azure portalında 'ClassicVNet' adlandırılacak şekilde görünen bir VNet ağ yapılandırma dosyasında bir çok uzun bir ad olabilir. Ad şunun gibi görünür: 'Grup ClassicRG ClassicVNet'. Bu adımda, ağ yapılandırma dosyasını indirin ve değerleri görüntüleyin.

Bilgisayarınızda bir dizin oluşturun ve sonra ağ yapılandırma dosyasını dizine aktarın. Bu örnekte, ağ yapılandırma dosyası C:\AzureNet dizinine aktarılır.

```powershell
Get-AzureVNetConfig -ExportToFile C:\AzureNet\NetworkConfig.xml
```

Dosyayı bir metin düzenleyiciyle açın ve klasik sanal adını görüntüleyin. Adları, PowerShell cmdlet'lerinizi çalıştırırken ağ yapılandırma dosyasında kullanın.

- Sanal ağ adları olarak listelenen **VirtualNetworkSite name =**
- Site adları olarak listelenen **LocalNetworkSite name =**

### <a name="3-create-the-connection"></a>3. Bağlantı oluşturma

Paylaşılan anahtarı ayarlayabilir ve bağlantıyı için Resource Manager Vnet'i Klasik sanal ağdan oluşturun. Portalı kullanarak paylaşılan anahtar ayarlanamıyor. Bu adımlar Klasik sürümü PowerShell cmdlet'lerini kullanarak oturum açmış durumdayken çalıştırdığınızdan emin olun. Bunu yapmak için **Add-AzureAccount**. Aksi takdirde, ayarlanacak mümkün olmayacaktır '-AzureVNetGatewayKey'.

- Bu örnekte, **- VNetName** aynılarını Klasik VNet ağ yapılandırma dosyanızdaki adıdır. 
- **- LocalNetworkSiteName** olarak yerel site için belirttiğiniz ad, ağ yapılandırma dosyasında bulunur.
- **- SharedKey** sizin oluşturup belirttiğiniz bir değerdir. Bu örnekte, kullandık *abc123*, ancak daha karmaşık bir şey oluşturabilirsiniz. Önemli olan, burada belirttiğiniz değerin, Resource Manager'a Klasik bağlantı oluştururken belirttiğiniz değerle aynı olmalıdır olmasıdır.

```powershell
Set-AzureVNetGatewayKey -VNetName "Group ClassicRG ClassicVNet" `
-LocalNetworkSiteName "172B9E16_RMVNetLocal" -SharedKey abc123
```

## <a name="verify"></a>6. Bölüm - bağlantılarınızı doğrulama

Bağlantılar, Azure portal veya PowerShell kullanarak doğrulayabilirsiniz. Doğrulanırken bir dakika beklemeniz gerekebilir veya iki bağlantı olarak oluşturulur. Bağlantı başarılı olduğunda, bağlantı durumu 'Bağlanıyor' öğesinden "Bağlandı" olarak değişir.

### <a name="to-verify-the-connection-from-your-classic-vnet-to-your-resource-manager-vnet"></a>Resource Manager Vnet'i Klasik ağınızdan bağlantıyı doğrulamak için

[!INCLUDE [vpn-gateway-verify-connection-azureportal-classic](../../includes/vpn-gateway-verify-connection-azureportal-classic-include.md)]

### <a name="to-verify-the-connection-from-your-resource-manager-vnet-to-your-classic-vnet"></a>Bağlantının, Resource Manager Vnet'i Klasik sanal ağınıza doğrulamak için

[!INCLUDE [vpn-gateway-verify-connection-portal-rm](../../includes/vpn-gateway-verify-connection-portal-rm-include.md)]

## <a name="faq"></a>Sanal ağlar arası bağlantılar hakkında SSS

[!INCLUDE [vpn-gateway-vnet-vnet-faq](../../includes/vpn-gateway-faq-vnet-vnet-include.md)]
