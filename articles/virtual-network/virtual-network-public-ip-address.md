---
title: "Oluşturma, değiştirme veya silme Azure genel bir IP adresi | Microsoft Docs"
description: "Oluşturma, değiştirme veya genel bir IP adresi silme öğrenin."
services: virtual-network
documentationcenter: na
author: jimdial
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.assetid: bb71abaf-b2d9-4147-b607-38067a10caf6
ms.service: virtual-network
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/25/2017
ms.author: jdial
ms.openlocfilehash: 5bffea350061231e1dc664b3abcbf79950a54402
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="create-change-or-delete-a-public-ip-address"></a>Oluşturma, değiştirme veya genel bir IP adresi silme

Bir ortak IP adresi ve oluşturmak, değiştirmek ve silmek nasıl hakkında bilgi edinin. Bir ortak IP adresi, kendi yapılandırılabilir ayarlar ile bir kaynaktır. Diğer Azure kaynaklarına genel bir IP adresi atayarak sağlar:
- Azure sanal makineler, Azure sanal makine ölçek kümeleri, Azure VPN ağ geçidi, uygulama ağ geçitleri ve Internet'e yönelik Azure yük dengeleyici gibi kaynaklara gelen Internet bağlantısı. Azure kaynaklarını atanan genel bir IP adresi olmaksızın Internet'ten gelen iletişimi alamaz. Bazı Azure kaynakları aracılığıyla ortak IP adresleri kendiliğinden erişilebilir olmasına karşın, diğer kaynakları genel IP adresleri Internet'ten erişilebilir olmasını atanmış olması gerekir.
- Tahmin edilebilir bir IP adresi kullanarak Internet bağlantısını giden. Örneğin, bir sanal makine, bir ortak IP adresi olmadan Internet'e giden kendisine atanmış, ancak Azure tarafından beklenmeyen bir ortak adresine çevrilmiş ağ adresi kendi adresidir iletişim kurabilir. Bir kaynağa genel bir IP adresi atayarak hangi IP adresi giden bağlantı için kullanılan bilmenizi sağlar. Tahmin edilebilir olsa, seçilen atama yöntemine bağlı olarak adresi değiştirebilirsiniz. Daha fazla bilgi için bkz: [genel bir IP adresi oluşturma](#create-a-public-ip-address). Azure kaynaklarını giden bağlantılar hakkında daha fazla bilgi için okuma [giden bağlantılar anlamak](../load-balancer/load-balancer-outbound-connections.md?toc=%2fazure%2fvirtual-network%2ftoc.json) makalesi.

## <a name="before-you-begin"></a>Başlamadan önce

Bu makalenin herhangi bölümündeki tüm adımları tamamlanmadan önce aşağıdaki görevleri tamamlayın:

- Gözden geçirme [Azure sınırlar](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits) makale ortak IP adresleri için sınırları hakkında bilgi edinin.
- Azure oturum açma [portal](https://portal.azure.com), Azure komut satırı arabirimi (CLI) ya da Azure PowerShell ile bir Azure hesabı. Zaten bir Azure hesabınız yoksa, kaydolun bir [ücretsiz deneme sürümü hesabı](https://azure.microsoft.com/free).
- Bu makalede, görevleri tamamlamak için PowerShell komutlarını kullanarak, [Azure PowerShell'i yükleme ve yapılandırma](/powershell/azureps-cmdlets-docs?toc=%2fazure%2fvirtual-network%2ftoc.json). Yüklü Azure PowerShell cmdlet'leri'nın en son sürümüne sahip olun. PowerShell komutlarıyla örnekler, yardım almanın yazın `get-help <command> -full`.
- Bu makalede, görevleri tamamlamak için Azure komut satırı arabirimi (CLI) komutlarını kullanarak, [Azure CLI'yi yükleme ve yapılandırma](/cli/azure/install-azure-cli?toc=%2fazure%2fvirtual-network%2ftoc.json). Yüklü Azure CLI'ın en son sürümüne sahip olun. CLI komutları için Yardım almak için yazın `az <command> --help`. CLI ve ön koşullar yüklemek yerine, Azure bulut Kabuğu'nu kullanabilirsiniz. Azure Cloud Shell doğrudan Azure portalının içinde çalıştırabileceğiniz ücretsiz bir Bash kabuğudur. Azure CLI, kabuğa önceden yüklenmiştir ve kabuk, hesabınızla birlikte kullanılacak şekilde yapılandırılmıştır. Bulut Kabuğu'nu kullanmak için bulut Kabuğu'nu tıklatın. **> _** en üstündeki düğmesi [portal](https://portal.azure.com).

Nominal bir ücret ortak IP adresine sahip. Fiyatlandırma görüntülemek için okuma [IP adresi fiyatlandırma](https://azure.microsoft.com/pricing/details/ip-addresses) sayfası. 

## <a name="create-a-public-ip-address"></a>Genel IP adresi oluşturma

1. Oturum [Azure portal](https://portal.azure.com) bir hesapla aboneliğiniz için ağ katılımcı rolü için diğer bir deyişle (en az) atanan izinleri. Okuma [Azure rol tabanlı erişim denetimi için yerleşik roller](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) rolleri ve izinleri hesaplarına atama hakkında daha fazla bilgi için makalenin.
2. Metni içeren kutusunda *arama kaynakları* Azure portalının en üstünde yazın *genel IP adresi*. Zaman **ortak IP adresleri** görünür arama sonuçlarında tıklatın.
3. Tıklatın **+ Ekle** içinde **genel IP adresi** görünür dikey.
4. Girin veya seçin aşağıdaki ayarları için değerleri **ortak IP adresi oluştur** görünür, ardından dikey **oluşturma**:

    |Ayar|Gerekli mi?|Ayrıntılar|
    |---|---|---|
    |SKU|Evet|SKU'ları giriş önce oluşturulan tüm genel IP adresleri **temel** SKU genel IP adresleri.  Genel IP adresi oluşturulduktan sonra SKU değiştirilemiyor. Tek başına sanal makine, sanal makinelerin bir kullanılabilirlik kümesi ya da sanal makine ölçek kümeleri içinde temel veya standart SKU'ları kullanabilirsiniz.  Kullanılabilirlik kümeleri veya ölçek kümeleri içinde sanal makineler arasında SKU'ları karıştırma izin verilmiyor. **Temel** SKU: kullanılabilirlik bölgeyi destekleyen bir bölgede bir ortak IP adresi oluşturuyorsanız **kullanılabilirlik bölge** ayar *hiçbiri* varsayılan olarak. Ortak IP adresi için belirli bir bölgenin güvence altına almak için bir kullanılabilirlik bölgeyi seçmek seçebilirsiniz. **Standart** SKU: A standart SKU genel IP sanal makine ya da bir yük dengeleyici ön uç ilişkili olabilir. Kullanılabilirlik bölgeyi destekleyen bir bölgede bir ortak IP adresi oluşturuyorsanız **kullanılabilirlik bölge** ayar *bölge olarak yedekli* varsayılan olarak. Kullanılabilirlik bölgeler hakkında daha fazla bilgi için bkz: **kullanılabilirlik bölge** ayarı. Standart SKU, standart yük dengeleyici adresine ilişkilendirirseniz gereklidir. Standart yük Dengeleyiciler hakkında daha fazla bilgi için bkz: [Azure yük dengeleyici standart SKU](../load-balancer/load-balancer-standard-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json). Standart SKU Önizleme sürümünde ' dir. Standart SKU genel bir IP adresi oluşturmadan önce ilk adımları tamamlamanız gerekir [standart SKU Önizleme için kaydetmek](#register-for-the-standard-sku-preview) ve genel IP adresi (bölge) desteklenen bir konumda oluşturun. Desteklenen konumlar listesi için bkz: [bölge kullanılabilirliği](../load-balancer/load-balancer-standard-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json#region-availability) ve izleme [Azure sanal ağı güncelleştirir](https://azure.microsoft.com/updates/?product=virtual-network) ek bölge desteği için sayfa. Standart bir SKU genel IP adresini bir sanal makinenin ağ arabirimine atadığınızda amaçlanan trafiğe bir [ağ güvenlik grubuyla](security-overview.md#network-security-groups) açıkça izin vermeniz gerekir. Bir ağ güvenlik grubu oluşturup ilişkilendirene ve istenen trafiğe açıkça izin verene kadar kaynakla erişim kurma girişimleri başarısız olur.|
    |Ad|Evet|Adı kaynak grubun içinde benzersiz olmalıdır.|
    |IP sürümü|Evet| IPv4 veya IPv6 seçin. Ortak IPv4 adreslerinin bazı Azure kaynakları atanabilir olsa da, bir IPv6 ortak IP adresi yalnızca bir Internet'e yönelik Yük Dengeleyici atanabilir. Yük Dengeleyici Yük Dengelemesi IPv6 trafiği için Azure sanal makineler. Daha fazla bilgi edinmek [Yük Dengeleme IPv6 trafiğini sanal makinelere](../load-balancer/load-balancer-ipv6-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json). Seçtiyseniz **standart SKU**, seçmek için seçeneğinden yararlanamazsınız *IPv6*. Yalnızca bir IPv4 adresi kullanırken oluşturabileceğiniz **standart SKU**.|
    |IP adresi ataması|Evet|**Dinamik:** genel IP adresi için ilişkili tamamlandıktan sonra bir sanal makine ve sanal makineye bağlı bir ağ arabirimi ilk kez başlatıldığında yalnızca dinamik adresler atanır. Ağ arabiriminin bağlı olduğu sanal makine (serbest bırakıldığında) durdurulursa dinamik adresler değiştirebilirsiniz. Sanal makine yeniden veya durduruldu (ancak değil serbest) adresi aynı kalır. **Statik:** statik adresleri genel IP adresi oluşturulduğu zaman atanır. Sanal makine durduruldu (serbest bırakıldığında) durumda yerleştirirseniz bile statik adresleri değiştirmeyin. Ağ arabirimi silindiğinde adresi yalnızca yayımlanır. Ağ arabirimi oluşturulduktan sonra atama yöntemi değiştirebilirsiniz. Seçerseniz *IPv6* için **IP sürüm**, atama yöntemi *dinamik*. Seçerseniz *standart* için **SKU**, atama yöntemi *statik*.|
    |Boşta kalma zaman aşımı (dakika)|Hayır|Etkin tutma iletileri göndermek için istemcileri kullanmadan bir TCP veya HTTP bağlantısını açık tutmak için kaç dakika. IPv6 için seçerseniz **IP sürüm**, bu değer değiştirilemez. |
    |DNS ad etiketi|Hayır|Adı (tüm abonelikleri ve tüm müşteriler arasında) oluşturmak Azure konum içinde benzersiz olmalıdır. Ada sahip bir kaynağa bağlanabilmesi için Azure ortak DNS hizmet adı ve IP adresi otomatik olarak kaydeder. Azure ekler *location.cloudapp.azure.com* (konum seçtiğiniz konum olduğu) adına, tam DNS adı oluşturmak için sağlayın. Her iki adres sürümü oluşturmayı seçerseniz, aynı DNS adı IPv4 ve IPv6 adreslerine atanır. Azure DNS hizmeti, bir IPv4 ve IPv6 AAAA ad kayıtlarını içerir ve DNS adı arandığında hem kayıtlarıyla yanıt verir. İstemci ile iletişim kurmak için adres (IPv4 veya IPv6) seçer.|
    |Bir IPv6 (ya da IPv4) adresi oluşturma|Hayır| IPv6 ya da IPv4 görüntülenip görüntülenmeyeceğini ne için seçtiğiniz üzerinde bağımlı **IP sürüm**. Örneğin, **IPv4** için **IP sürüm**, **IPv6** burada görüntülenir. Seçerseniz *standart* için **SKU**, bir IPv6 adresi oluşturma seçeneğiniz yoktur.
    |Adı (size işaretlediyseniz görünür **bir IPv6 (ya da IPv4) adresi oluşturma** onay kutusunu)|Evet seçeneğini belirlerseniz, **IPv6 oluşturmak** (veya IPv4) onay kutusu.|Ad için girdiğiniz adından farklı olmalıdır ilk **adı** bu listede. Bir IPv4 ve IPv6 adresi oluşturmayı seçerseniz, iki ayrı ortak IP adresi kaynakları, kendisine atanmış her IP adresi sürümüne sahip bir portal oluşturur.|
    |IP adresi ataması (size işaretlediyseniz görünür **bir IPv6 (ya da IPv4) adresi oluşturma** onay kutusunu)|Evet seçeneğini belirlerseniz, **IPv6 oluşturmak** (veya IPv4) onay kutusu.|Onay kutusu diyorsa **bir IPv4 adresi oluşturma**, bir atama yöntemini seçin. Onay kutusu diyorsa **bir IPv6 adresi oluşturma**, bunu olması gerektiği bir atama yöntemi seçemezsiniz **dinamik**.|
    |Abonelik|Evet|Aynı bulunmalıdır [abonelik](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#subscription) genel IP adresi ilişkilendirmek istediğiniz kaynak olarak.|
    |Kaynak grubu|Evet|Aynı veya farklı bulunabilir [kaynak grubu](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#resource-group) genel IP adresi ilişkilendirmek istediğiniz kaynak olarak.|
    |Konum|Evet|Aynı bulunmalıdır [konumu](https://azure.microsoft.com/regions), genel IP ilişkilendirmek istediğiniz kaynak olarak adresi bölge da denir.|
    |Kullanılabilirlik bölge| Hayır | Desteklenen bir konum seçin, bu ayar yalnızca görünür. Desteklenen konumlar listesi için bkz: [kullanılabilirlik bölgeleri genel bakış](../availability-zones/az-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json). Kullanılabilirlik bölgeler şu anda Önizleme sürümünde içindedir. Bir bölgeyle veya bölge olarak yedekli seçeneği belirlemeden önce ilk adımları tamamlamanız gereken [kullanılabilirlik bölgeleri Önizleme için kaydetmek](../availability-zones/az-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json#get-started-with-the-availability-zones-preview). Seçtiyseniz **temel** SKU, *hiçbiri* sizin için otomatik olarak seçilir. Belirli bir bölgenin garanti tercih ederseniz, belirli bir bölgenin seçebilirsiniz. Her iki seçenek bölge olarak yedekli değil. Seçtiyseniz **standart** SKU: bölge olarak yedekli sizin için otomatik olarak seçilir ve veri yolu bölge hatalarına karşı dayanıklı hale getirir. Bölge hatalarına karşı dayanıklı değil, belirli bir bölgenin güvence altına almak tercih ederseniz, belirli bir bölgenin seçebilirsiniz.
  

**Komutları**

Portal, iki ortak IP adresi kaynakları (bir IPv4 ve bir IPv6) oluşturma seçeneği sağlar ancak aşağıdaki CLI ve PowerShell komutlarını bir kaynak için bir IP sürümü veya başka bir adres ile oluşturun. İki ortak IP adresi kaynakları, her IP sürümü için bir tane isterseniz, farklı adlar ve sürümler için genel IP adresi kaynakları belirtme komutu, iki kez çalıştırmanız gerekir. 

|Aracı|Komut|
|---|---|
|CLI|[az ağ genel IP oluşturun](/cli/azure/network/public-ip?toc=%2fazure%2fvirtual-network%2ftoc.json#create)|
|PowerShell|[AzureRmPublicIpAddress yeni](/powershell/module/azurerm.network/new-azurermpublicipaddress)|

## <a name="view-change-settings-for-or-delete-a-public-ip-address"></a>Görüntüleme, ayarlarını değiştirmek veya bir ortak IP adresini Sil

1. Oturum [Azure portal](https://portal.azure.com) bir hesapla aboneliğiniz için ağ katılımcı rolü için diğer bir deyişle (en az) atanan izinleri. Okuma [Azure rol tabanlı erişim denetimi için yerleşik roller](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) rolleri ve izinleri hesaplarına atama hakkında daha fazla bilgi için makalenin.
2. Metni içeren kutusunda *arama kaynakları* Azure portalının en üstünde yazın *genel IP adresi*. Zaman **ortak IP adresleri** görünür arama sonuçlarında tıklatın.
3. İçinde **ortak IP adresleri** görünür, dikey penceresini görüntülemek için ayarları değiştirmek veya silmek istediğiniz genel IP adresi adını tıklatın.
4. Görüntülenen dikey penceresinde ortak IP adresi için görüntülemek, silmek veya genel IP adresini değiştirmek istediğinize bağlı olarak aşağıdaki seçeneklerden birini tamamlayın.
    - **Görünüm**: **genel bakış** dikey bölümde ilişkili için (adresi bir ağ arabirimiyle ilişkilendirilmiş ise) ağ arabirimi gibi ortak IP adresi için anahtar ayarlarını gösterir. Portal sürümü (IPv4 veya IPv6) adresinin görüntülemez. Sürüm bilgilerini görüntülemek için genel IP adresini görüntülemek için PowerShell veya CLI komutunu kullanın. IP adresi sürümü IPv6 ise, atanan adresi portal, PowerShell veya CLI tarafından görüntülenmez. 
    - **Silme**: genel IP adresini silmek için tıklatın **silmek** içinde **genel bakış** dikey bölüm. Adres geçerli bir IP yapılandırmasıyla ilişkilendirilmiş ise silinemiyor. Adresi şu anda bir yapılandırmasıyla ilişkili olduğundan,'ı tıklatın **ilişkiyi** IP yapılandırması adresinden ilişkilendirmesini kaldırmak.
    - **Değişiklik**: tıklatın **yapılandırma**. 4. adımda bilgileri kullanarak ayarları değiştirme [genel bir IP adresi oluşturma](#create-a-public-ip-address) bu makalenin. Bir IPv4 adresi atamanın statik olan dinamik olarak değiştirmek için önce ilişkili olduğu IP yapılandırması gelen ortak IPv4 adresi ilişkilendirmesini kaldırmanız gerekir. Atama yöntemi dinamik olarak değiştirin ve tıklatın **ilişkilendirmek** IP ilişkilendirmek için aynı IP yapılandırması, farklı bir yapılandırma veya adresine ilkenin ilişkisi bırakılabilir. İçinde bir ortak IP adresi ilişkilendirmesini kaldırmak **genel bakış** 'yi tıklatın **ilişkiyi**.

>[!WARNING]
>Atama yöntemi statik olan dinamik olarak değiştirdiğinizde, genel IP adresi atanmış IP adresi kaybedersiniz. Statik veya dinamik adresleri ve (bir tanımladıysanız) tüm DNS ad etiketi arasında bir eşleme Azure ortak DNS sunucularını bakım yaparken, sanal makine durduruldu (serbest bırakıldığında) durumda kaldıktan sonra başlatıldığında dinamik bir IP adresi değiştirebilirsiniz. Adresinin değişmesini engellemek için bir statik IP adresi atayın.

**Komutları**

|Aracı|Komut|
|---|---|
|CLI|[az ağ ortak IP listesi](/cli/azure/network/public-ip?toc=%2fazure%2fvirtual-network%2ftoc.json#list) listesi genel IP adresleri için [az ağ ortak IP Göster](/cli/azure/network/public-ip?toc=%2fazure%2fvirtual-network%2ftoc.json#show) ayarları; göstermek için [az ağ ortak IP güncelleştirmesi](/cli/azure/network/public-ip?toc=%2fazure%2fvirtual-network%2ftoc.json#update) güncelleştirmek için; [az ağ ortak IP silme](/cli/azure/network/public-ip?toc=%2fazure%2fvirtual-network%2ftoc.json#delete) silmek için|
|PowerShell|[Get-AzureRmPublicIpAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress?toc=%2fazure%2fvirtual-network%2ftoc.json) bir ortak IP adres nesnesini almak ve ayarlarını görüntülemek için [kümesi AzureRmPublicIpAddress](/powershell/resourcemanager/azurerm.network/set-azurermpublicipaddress?toc=%2fazure%2fvirtual-network%2ftoc.json) ayarlarını; güncelleştirmek için [Kaldır AzureRmPublicIpAddress](/powershell/module/azurerm.network/remove-azurermpublicipaddress) silmek için|

## <a name="register-for-the-standard-sku-preview"></a>Standart SKU Önizleme için kaydolun

> [!NOTE]
> Genel kullanılabilirlik özellikleri sürüm gibi özellikleri Önizleme sürümde aynı düzeyde kullanılabilirlik ve güvenilirlik olmayabilir. Önizleme özellikleri desteklenmez, yetenekleri kısıtlı ve tüm Azure konumlarda kullanılamayabilir. 

Standart SKU genel bir IP adresi oluşturabilmeniz için önce önizleme için kaydetmeniz gerekir. Önizleme için kaydetmek için aşağıdaki adımları tamamlayın:

1. Yükleme ve Azure yapılandırma [PowerShell](/powershell/azure/install-azurerm-ps).
2. Çalıştırma `Get-Module -ListAvailable AzureRM` AzureRM modülünün hangi sürümünün yüklü olduğunu görmek için komutu. Daha yüksek yüklü veya sürüm 4.4.0 olmalıdır. Bunu yapmazsanız, en son sürümü yükleyebilirsiniz [PowerShell Galerisi](https://www.powershellgallery.com/packages/AzureRM).
3. Oturum açtığınızda Azure ile `login-azurermaccount` komutu.
4. Önizleme için kaydetmek için aşağıdaki komutu girin:
   
    ```powershell
    Register-AzureRmProviderFeature -FeatureName AllowLBPreview -ProviderNamespace Microsoft.Network
    ```

5. Aşağıdaki komutu girerek Önizleme için kayıtlı olduklarını doğrulayın:

    ```powershell
    Get-AzureRmProviderFeature -FeatureName AllowLBPreview -ProviderNamespace Microsoft.Network
    ```

## <a name="next-steps"></a>Sonraki adımlar
Genel IP adresleri aşağıdaki Azure kaynakları oluşturulurken ata:

- [Windows](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-network%2ftoc.json) veya [Linux](../virtual-machines/linux/quick-create-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json) sanal makineler
- [Internet'e yönelik Azure yük dengeleyici](../load-balancer/load-balancer-get-started-internet-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
- [Azure uygulama ağ geçidi](../application-gateway/application-gateway-create-gateway-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
- [Bir Azure VPN ağ geçidi kullanarak siteden siteye bağlantı](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
- [Azure sanal makine ölçek kümesi](../virtual-machine-scale-sets/virtual-machine-scale-sets-portal-create.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
