---
title: "Klasik sanal ağlar Azure Resource Manager sanal ağlara bağlanma: portalı | Microsoft Docs"
description: "Klasik sanal ağlar ve Resource Manager VPN ağ geçidi ve Portalı'nı kullanarak sanal ağlar arasında bir VPN bağlantısı oluşturma hakkında bilgi edinin"
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-service-management,azure-resource-manager
ms.assetid: 5a90498c-4520-4bd3-a833-ad85924ecaf9
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/27/2017
ms.author: cherylmc
ms.openlocfilehash: 8fd058d74d00ecc980d295ee6bd9680ff832f891
ms.sourcegitcommit: 821b6306aab244d2feacbd722f60d99881e9d2a4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/18/2017
---
# <a name="connect-virtual-networks-from-different-deployment-models-using-the-portal"></a>Farklı dağıtım modelinden Portalı'nı kullanarak sanal ağlara bağlanabilir

Bu makalede Resource Manager birbirleri ile iletişim kurmak için ayrı bir dağıtım modellerindeki kaynaklara izin vermek için sanal ağlar için Klasik sanal ağlara bağlanma gösterilmektedir. Bu makaledeki adımları öncelikle Azure portalını kullanın, ancak makaleyi bu listeden seçerek PowerShell kullanarak bu yapılandırmayı de oluşturabilirsiniz.

> [!div class="op_single_selector"]
> * [Portal](vpn-gateway-connect-different-deployment-models-portal.md)
> * [PowerShell](vpn-gateway-connect-different-deployment-models-powershell.md)
> 
> 

Bir Resource Manager Vnet'i klasik bir VNet bağlama, bir şirket içi site konumuna bir sanal ağa bağlanma benzer. Her iki bağlantı türü de IPsec/IKE kullanarak güvenli bir tünel sunmak üzere bir VPN ağ geçidi kullanır. Farklı Aboneliklerde ve farklı bölgelerdeki sanal ağlar arasında bir bağlantı oluşturabilirsiniz. Dinamik ya da rota tabanlı ağ geçidi ile yapılandırılmamış olduğu sürece şirket içi ağlara bağlantılar zaten sanal ağlar da bağlanabilirsiniz. Sanal ağlar arası bağlantılar hakkında daha fazla bilgi için bu makalenin sonunda yer alan [Sanal ağlar arası bağlantılar hakkında SSS](#faq) bölümünü inceleyin. 

Sanal ağlar aynı bölgede varsa, bunun yerine bunları VNet eşlemesi kullanmanın bağlayarak göz önünde bulundurun isteyebilirsiniz. VNet eşlemesi VPN ağ geçidini kullanmaz. Daha fazla bilgi için bkz. [VNet eşlemesi](../virtual-network/virtual-network-peering-overview.md). 

### <a name="before"></a>Başlamadan önce

* Bu adımları iki sanal ağlar zaten oluşturulmuş olduğunu varsayalım. Bu makalede bir alıştırma olarak kullanıyorsanız ve sanal ağlar yoksa bunları oluşturmanıza yardımcı olması için adımlarda bağlantıları vardır.
* Sanal ağlar için adres aralıklarını değil birbirleri ile üst üste, veya ağ geçitleri bağlı olabilir diğer bağlantılar için aralıklardan herhangi biriyle çakışmayan doğrulayın.
* Kaynak Yöneticisi ve Hizmet Yönetimi (Klasik) için en son PowerShell cmdlet'lerini yükleyin. Bu makalede, Azure portal ve PowerShell kullanın. PowerShell Resource Manager Vnet'i Klasik sanal ağdan bağlantı oluşturmak için gereklidir. Daha fazla bilgi için bkz. [Azure PowerShell’i yükleme ve yapılandırma](/powershell/azure/overview). 

### <a name="values"></a>Örnek ayarlar

Bu değerleri kullanarak bir test ortamı oluşturabilir veya bu makaledeki örnekleri daha iyi anlamak için bunlara bakabilirsiniz.

**Klasik VNet**

Sanal ağ adı ClassicVNet = <br>
Adres alanı 10.0.0.0/24 = <br>
Alt ağ adı Subnet-1 = <br>
Alt ağ adres aralığı 10.0.0.0/27 = <br>
Abonelik kullanmak istediğiniz aboneliği = <br>
Kaynak grubu ClassicRG = <br>
Konum Batı ABD = <br>
GatewaySubnet 10.0.0.32/28 = <br>
Yerel site RMVNetLocal = <br>

**Resource Manager Vnet'i**

Sanal ağ adı RMVNet = <br>
Adres alanı 192.168.0.0/16 = <br>
Kaynak grubu RG1 = <br>
Konum Doğu ABD = <br>
Alt ağ adı Subnet-1 = <br>
Adres aralığı 192.168.1.0/24 = <br>
GatewaySubnet 192.168.0.0/26 = <br>
Sanal ağ geçidi adı RMGateway = <br>
Ağ geçidi türü VPN = <br>
VPN türü = rota tabanlı <br>
SKU VpnGw1 = <br>
Konum Doğu ABD = <br>
Sanal ağ RMVNet = <br> (Bu sanal ağa VPN ağ geçidi ilişkilendirmek) İlk IP yapılandırması rmgwpip = <br> (ağ geçidi genel IP adresi) Yerel ağ geçidi ClassicVNetLocal = <br>
Bağlantı adı RMtoClassic =

### <a name="connectoverview"></a>Bağlantı genel bakış

Bu yapılandırma için sanal ağlar arasında bir IPSec/IKE VPN tüneli üzerinden VPN ağ geçidi bağlantı oluşturun. Sanal ağ Aralıklarınızın hiçbiri birbirleriyle ya da bağlandıkları yerel ağlar biriyle çakışmadığından emin olun.

Aşağıdaki tabloda örnek sanal ağlar ve yerel siteleri nasıl tanımlanan bir örnek gösterilmektedir:

| Sanal Ağ | Adres Alanı | Bölge | Yerel ağ sitesine bağlanır |
|:--- |:--- |:--- |:--- |
| ClassicVNet |(10.0.0.0/24) |Batı ABD | RMVNetLocal (192.168.0.0/16) |
| RMVNet | (192.168.0.0/16) |Doğu ABD |ClassicVNetLocal (10.0.0.0/24) |

## <a name="classicvnet"></a>1. Bölüm - Klasik VNet ayarlarını yapılandırın

Bu bölümde, oluşturduğunuz Klasik VNet yerel ağ (yerel site) ve sanal ağ geçidi. Ekran görüntüleri örnek olarak verilmiştir. Değerleri kendinizinkilerle değiştirin veya kullanmak mutlaka [örnek](#values) değerleri.

### 1. <a name="classicvnet"></a>Klasik VNet oluşturma

Klasik bir VNet yoktur ve bu adımları bir alıştırma olarak çalıştırıyorsanız, kullanarak bir sanal ağ oluşturabilirsiniz [bu makalede](../virtual-network/virtual-networks-create-vnet-classic-pportal.md) ve [örnek](#values) üstten ayarlarının değerleri.

Bir VPN ağ geçidi ile bir VNet zaten varsa, ağ geçidini dinamik olduğundan emin olun. Statik varsa, devam etmeden önce VPN ağ geçidi silmeniz gerekir [yerel site yapılandırma](#local).

1. Açık [Azure portal](https://ms.portal.azure.com) ve Azure hesabınızla oturum açın.
2. Tıklatın **+ kaynak oluşturma** 'Yeni' sayfasını açın.
3. 'Market arama' alanına 'Sanal ağ' yazın. Bunun yerine, ağ seçerseniz sanal ağ ->, klasik bir VNet oluşturma seçeneğini almazsınız.
4. 'Sanal ağ' döndürülen listeden bulun ve sanal ağ sayfasını açmak için tıklatın. 
5. Sanal ağ sayfasında klasik bir VNet oluşturmak için ' Klasik' seçin. Varsayılan burada izlerseniz, Resource Manager Vnet'i ile bunun yerine Rüzgar.

### 2. <a name="local"></a>Yerel site yapılandırma

1. Gidin **tüm kaynakları** ve bulun **ClassicVNet** listesinde.
2. Üzerinde **genel bakış** sayfasında **VPN bağlantıları** 'yi tıklatın **ağ geçidi** bir ağ geçidi oluşturmak için.
  ![Bir VPN ağ geçidi yapılandırma](./media/vpn-gateway-connect-different-deployment-models-portal/gatewaygraphic.png "bir VPN ağ geçidi yapılandırma")
3. Üzerinde **yeni VPN bağlantısı** sayfası için **bağlantı türü**seçin **siteden siteye**.
4. İçin **yerel site**, tıklatın **gerekli ayarları Yapılandır**. Bu açılır **yerel site** sayfa.
5. Üzerinde **yerel site** sayfasında, Resource Manager Vnet'i başvurmak için bir ad oluşturun. Örneğin, 'RMVNetLocal'.
6. Resource Manager Vnet'i bir genel IP adresi zaten için VPN ağ geçidi değeri kullanırsanız **VPN ağ geçidi IP adresi** alan. Bu adımları bir alıştırma olarak işleminden olduğundan veya bir sanal ağ geçidi için Resource Manager Vnet'i henüz yoksa, bir yer tutucu IP adresini yapabilirsiniz. Yer tutucu IP adresi geçerli bir biçim kullandığından emin olun. Daha sonra kaynak yöneticisi sanal ağ geçidi genel IP adresiyle yer tutucu IP adresini değiştirin.
7. İçin **istemci adres alanı**, kullanın [değerleri](#connectoverview) için sanal ağ IP adresi alanları için Resource Manager Vnet'i. Bu ayar Kaynak Yöneticisi sanal ağa yönlendirmek için adres alanları belirtmek için kullanılır. Örnekte, 192.168.0.0/16, adres aralığı RMVNet için kullanırız.
8. Tıklatın **Tamam** değerleri kaydetmek ve geri dönmek için **yeni VPN bağlantısı** sayfası.

### <a name="classicgw"></a>3. Sanal ağ geçidini oluşturma

1. Üzerinde **yeni VPN bağlantısı** sayfasında, **ağ geçidini hemen Oluştur** onay kutusu.
2. **İsteğe bağlı ağ geçidi yapılandırması**’na tıklayarak **Ağ geçidi yapılandırması** sayfasını açın.

  ![Açık gateway yapılandırma sayfasında](./media/vpn-gateway-connect-different-deployment-models-portal/optionalgatewayconfiguration.png "açık ağ geçidi yapılandırma sayfası")
3. Tıklatın **Subnet - gerekli ayarları Yapılandır** açmak için **alt ağ Ekle** sayfası. **Adı** gerekli değeriyle zaten yapılandırıldı: **GatewaySubnet**.
4. **Adres aralığı** ağ geçidi alt ağı için izin verilen aralığın başvuruyor. Bir ağ geçidi alt ağı ile/29 oluşturabilseniz adres aralığı (3 adresleri), daha fazla IP adreslerini içeren bir ağ geçidi alt ağı oluşturma öneririz. Bu, daha fazla kullanılabilir IP adreslerini gerektirebilir gelecekteki yapılandırmaları uyum sağlayacak. Mümkünse, / 27 veya /28 kullanın. Bu adımları bir alıştırma olarak kullanıyorsanız, başvurabilirsiniz [örnek değerler](#values). Bu örnekte, '10.0.0.32/28' kullanın. Tıklatın **Tamam** ağ geçidi alt ağı oluşturmak için.
5. Üzerinde **ağ geçidi Yapılandırması** sayfasında **boyutu** ağ geçidi SKU'su başvuruyor. Ağ geçidi SKU'su VPN ağ geçidinizi için seçin.
6. Doğrulayın **yönlendirme türü** olan **dinamik**, ardından **Tamam** geri dönmek için **yeni VPN bağlantısı** sayfası.
7. Üzerinde **yeni VPN bağlantısı** sayfasında, **Tamam** VPN ağ geçidinizi oluşturmaya başlamak için. Bir VPN ağ geçidi oluşturma tamamlamak 45 dakika kadar sürebilir.

### <a name="ip"></a>4. Sanal ağ geçidi genel IP adresi kopyalayın

Sanal ağ geçidi oluşturulduktan sonra ağ geçidi IP adresini görüntüleyebilirsiniz. 

1. Klasik ağınızı gidin ve tıklayın **genel bakış**.
2. Tıklatın **VPN bağlantıları** VPN bağlantıları sayfasını açın. VPN bağlantıları sayfasında, genel IP adresini görüntüleyebilirsiniz. Bu sanal ağ geçidiniz için atanan ortak IP adresidir. IP adresini not edin. Resource Manager yerel ağ geçidi yapılandırma ayarlarınızı ile çalışırken, sonraki adımlarda kullanın. 
3. Ağ geçidi bağlantılarınızı durumunu görüntüleyebilirsiniz. Oluşturduğunuz yerel ağ sitesine 'Bağlanıyor' olarak listelenen dikkat edin. Bağlantılarınızı oluşturduktan sonra durumunu değiştirir. Durum görüntülemeyi tamamladığınızda bu sayfayı kapatabilirsiniz.

## <a name="rmvnet"></a>2. Bölüm - Resource Manager VNet ayarlarını yapılandırın

Bu bölümde, Kaynak Yöneticisi'ni ağınız için sanal ağ geçidi ve yerel ağ geçidi oluşturun. Ekran görüntüleri örnek olarak verilmiştir. Değerleri kendinizinkilerle değiştirin veya kullanmak mutlaka [örnek](#values) değerleri.

### <a name="1-create-a-virtual-network"></a>1. Sanal ağ oluşturma

**Örnek değerler:**

* Sanal ağ adı RMVNet = <br>
* Adres alanı 192.168.0.0/16 = <br>
* Kaynak grubu RG1 = <br>
* Konum Doğu ABD = <br>
* Alt ağ adı Subnet-1 = <br>
* Adres aralığı 192.168.1.0/24 = <br>


Resource Manager Vnet'i yoktur ve bu adımları bir alıştırma olarak çalıştırıyorsanız, kullanarak bir sanal ağ oluşturabilirsiniz [bu makalede](../virtual-network/virtual-networks-create-vnet-arm-pportal.md) ve örnek değerleri.

### <a name="2-create-a-gateway-subnet"></a>2. Ağ geçidi alt ağı oluşturma

**Örnek değer:** GatewaySubnet 192.168.0.0/26 =

Bir sanal ağ geçidi oluşturmadan önce ilk ağ geçidi alt ağı oluşturmanız gerekir. CIDR sayısıyla 28 ya da daha büyük bir ağ geçidi alt ağı oluşturun (/ 27, / 26, vb..). Bu bölümü bir alıştırma olarak oluşturuyorsanız, örnek değerleri kullanabilirsiniz.

[!INCLUDE [vpn-gateway-no-nsg-include](../../includes/vpn-gateway-no-nsg-include.md)]

[!INCLUDE [vpn-gateway-add-gwsubnet-rm-portal](../../includes/vpn-gateway-add-gwsubnet-rm-portal-include.md)]

### <a name="creategw"></a>3. Sanal ağ geçidi oluşturma

**Örnek değerler:**

* Sanal ağ geçidi adı RMGateway = <br>
* Ağ geçidi türü VPN = <br>
* VPN türü = rota tabanlı <br>
* SKU VpnGw1 = <br>
* Konum Doğu ABD = <br>
* Sanal ağ RMVNet = <br>
* İlk IP yapılandırması rmgwpip = <br>

[!INCLUDE [vpn-gateway-add-gw-rm-portal](../../includes/vpn-gateway-add-gw-rm-portal-include.md)]

### <a name="createlng"></a>4. Yerel ağ geçidi oluşturma

**Örnek değerler:** yerel ağ geçidi ClassicVNetLocal =

| Sanal Ağ | Adres Alanı | Bölge | Yerel ağ sitesine bağlanır |Ağ geçidi genel IP adresi|
|:--- |:--- |:--- |:--- |:--- |
| ClassicVNet |(10.0.0.0/24) |Batı ABD | RMVNetLocal (192.168.0.0/16) |ClassicVNet ağ geçidi için atanan ortak IP adresi|
| RMVNet | (192.168.0.0/16) |Doğu ABD |ClassicVNetLocal (10.0.0.0/24) |RMVNet ağ geçidi için atanan ortak IP adresi.|

Yerel ağ geçidi adresi aralığı ve klasik VNet ve kendi sanal ağ geçidi ile ilişkilendirilmiş ortak IP adresini belirtir. Bu adımları bir alıştırma olarak yapıyorsanız, örnek değerleri bakın.

[!INCLUDE [vpn-gateway-add-lng-rm-portal](../../includes/vpn-gateway-add-lng-rm-portal-include.md)]

## <a name="modifylng"></a>3. Bölüm - Klasik VNet yerel site ayarlarını değiştirme

Bu bölümde, yerel site ayarlarını Resource Manager VPN ağ geçidi IP adresi ile belirtirken kullanılan yer tutucu IP adresini değiştirin. Bu bölümde, Klasik (SM) PowerShell cmdlet'lerini kullanır.

1. Azure portalında Klasik sanal ağa gidin.
2. Sanal ağınız için sayfada tıklatın **genel bakış**.
3. İçinde **VPN bağlantıları** bölümünde, yerel grafiği sitenizdeki adına tıklayın.

    ![VPN bağlantıları](./media/vpn-gateway-connect-different-deployment-models-portal/vpnconnections.png "VPN bağlantıları")
4. Üzerinde **siteden siteye VPN bağlantıları** sayfasında, sitenin adını tıklatın.

    ![Site adı](./media/vpn-gateway-connect-different-deployment-models-portal/sitetosite3.png "yerel site adı")
5. Yerel sitenin bağlantısı sayfasında açmak için yerel site adını tıklatın **yerel site** sayfa.

    ![Açık yerel site](./media/vpn-gateway-connect-different-deployment-models-portal/openlocal.png "yerel site Aç")
6. Üzerinde **yerel site** sayfasında, yerine **VPN ağ geçidi IP adresi** Resource Manager ağ geçidinin IP adresine sahip.

    ![Ağ geçidi IP adresi](./media/vpn-gateway-connect-different-deployment-models-portal/gwipaddress.png "ağ geçidi IP adresi")
7. Tıklatın **Tamam** IP adresi güncelleştirilemedi.

## <a name="RMtoclassic"></a>4. Bölüm - Resource Manager Klasik bağlantı oluşturun.

Bu adımlarda, Azure portalını kullanarak Klasik VNet ile Resource Manager Vnet'i arasındaki bağlantı yapılandırın.

1. İçinde **tüm kaynakları**, yerel ağ geçidi bulun. Bizim örneğimizde, yerel ağ geçididir **ClassicVNetLocal**.
2. Tıklatın **yapılandırma** ve IP adresi değeri Klasik VNet için VPN ağ geçidi olduğunu doğrulayın. Gerekirse, güncelleştirin ve ardından **kaydetmek**. Sayfasını kapatın.
3. İçinde **tüm kaynakları**, yerel ağ geçidi'ı tıklatın.
4. Tıklatın **bağlantıları** Bağlantılar sayfasını açın.
5. Üzerinde **bağlantıları** sayfasında,  **+**  bağlantı eklemek için.
6. Üzerinde **Bağlantı Ekle** sayfasında, bağlantı adı. Örneğin, 'RMtoClassic'.
7. **Siteden siteye** bu sayfada zaten seçilidir.
8. Bu site ile ilişkilendirmek istediğiniz sanal ağ geçidi seçin.
9. Oluşturma bir **paylaşılan anahtar**. Bu anahtar, Resource Manager Vnet'i Klasik sanal ağdan oluşturduğunuz bağlantı de kullanılır. Anahtarı oluşturmak veya bir oluşturur. Örneğimizde 'abc123' kullanıyoruz, ancak daha karmaşık bir şey olabilir ve kullanmanız gerekir.
10. Tıklatın **Tamam** bağlantı oluşturmak için.

##<a name="classictoRM"></a>Bölüm 5 - Klasik'ten kaynak yöneticisi bağlantısı oluşturma

Bu adımlarda, Resource Manager Vnet'i Klasik VNet arasında bağlantı yapılandırın. Bu adımları PowerShell gerektirir. Portalda bu bağlantı oluşturamıyor. İndirilen ve Klasik (SM) ve Kaynak Yöneticisi (RM) PowerShell cmdlet'lerini yüklü emin olun.

### <a name="1-connect-to-your-azure-account"></a>1. Azure hesabınıza bağlanma

Yükseltilmiş haklarla PowerShell konsolu açın ve Azure hesabınızda oturum açın. Aşağıdaki cmdlet'i Azure hesabınız için oturum açma kimlik bilgilerini ister. Oturum açtıktan sonra Azure PowerShell kullanılabilir olacak şekilde, hesap ayarlarınızı karşıdan yüklenir.

```powershell
Login-AzureRmAccount
```
   
Birden fazla aboneliğiniz varsa, Azure aboneliklerinize listesini alın.

```powershell
Get-AzureRmSubscription
```

Kullanmak istediğiniz aboneliği belirtin. 

```powershell
Select-AzureRmSubscription -SubscriptionName "Name of subscription"
```

Azure Klasik PowerShell cmdlet'lerini (SM) kullanmak için hesabınızı ekleyin. Bunu yapmak için aşağıdaki komutu kullanabilirsiniz:

```powershell
Add-AzureAccount
```

### <a name="2-view-the-network-configuration-file-values"></a>2. Ağ yapılandırma dosyası değerlerini görüntüleme

Azure portalında VNet oluşturduğunuzda, Azure kullandığı tam adı Azure portalında görünür değil. Örneğin, 'ClassicVNet' Azure portalında adlandırılması görünür bir VNet ağ yapılandırma dosyasında bir kadar daha uzun bir ad olabilir. Adı şöyle görünebilir: 'Grup ClassicRG ClassicVNet'. Bu adımda, ağ yapılandırma dosyası indirme ve değerleri görüntüleyin.

Bilgisayarınızda bir dizin oluşturun ve sonra ağ yapılandırma dosyasını dizine aktarın. Bu örnekte, ağ yapılandırma dosyası C:\AzureNet dizinine aktarılır.

```powershell
Get-AzureVNetConfig -ExportToFile C:\AzureNet\NetworkConfig.xml
```

Dosyayı bir metin düzenleyicisiyle açın ve klasik sanal ağınızı adını görüntüleyin. Adları PowerShell cmdlet'lerini çalıştırırken ağ yapılandırma dosyası kullanın.

- Sanal ağ adları olarak listelenen **VirtualNetworkSite adı =**
- Site adları olarak listelenen **LocalNetworkSite adı =**

### <a name="3-create-the-connection"></a>3. Bağlantı oluşturma

Paylaşılan anahtar ayarlayın ve Resource Manager Vnet'i Klasik sanal ağdan bağlantı oluşturun. Portalı kullanarak paylaşılan anahtar ayarlanamıyor. Klasik sürümü PowerShell cmdlet'lerini kullanarak oturum açıp aşağıdaki adımları çalıştırdığınızdan emin olun. Bunu yapmak için kullanın **Add-AzureAccount**. Aksi takdirde ayarlamak mümkün olmaz '-AzureVNetGatewayKey'.

- Bu örnekte, **- vnetname adlı** aynılarını Klasik VNet ağ yapılandırma dosyanızda adıdır. 
- **- LocalNetworkSiteName** olarak yerel site için belirtilen ad, ağ yapılandırma dosyasında bulunamadı.
- **- SharedKey** oluşturmak ve belirten bir değer. Bu örnekte, kullandık *abc123*, ancak daha karmaşık bir şey oluşturabilirsiniz. Burada belirttiğiniz değer, Resource Manager Klasik bağlantı oluştururken, belirtilen değerle aynı değere olmalıdır önemli şeydir.

```powershell
Set-AzureVNetGatewayKey -VNetName "Group ClassicRG ClassicVNet" `
-LocalNetworkSiteName "172B9E16_RMVNetLocal" -SharedKey abc123
```

##<a name="verify"></a>Bölüm 6 - bağlantılarınızı doğrulayın

Bağlantılarınızı Azure portal veya PowerShell kullanarak doğrulayabilirsiniz. Doğrulandığında, bir dakika beklemeniz gerekebilir veya iki bağlantı olarak oluşturuldu. Bağlantı başarılı olduğunda, bağlantı durumu 'Nden bağlanma' 'Bağlı' değiştirir.

### <a name="to-verify-the-connection-from-your-classic-vnet-to-your-resource-manager-vnet"></a>Resource Manager Vnet'i Klasik VNet arasında bağlantı doğrulamak için

[!INCLUDE [vpn-gateway-verify-connection-azureportal-classic](../../includes/vpn-gateway-verify-connection-azureportal-classic-include.md)]

###<a name="to-verify-the-connection-from-your-resource-manager-vnet-to-your-classic-vnet"></a>Resource Manager Vnet'i bağlantısından Klasik vnet doğrulamak için

[!INCLUDE [vpn-gateway-verify-connection-portal-rm](../../includes/vpn-gateway-verify-connection-portal-rm-include.md)]

## <a name="faq"></a>Sanal ağlar arası bağlantılar hakkında SSS

[!INCLUDE [vpn-gateway-vnet-vnet-faq](../../includes/vpn-gateway-faq-vnet-vnet-include.md)]
