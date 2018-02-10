---
title: "Bir Azure ağ arabirimi için IP adreslerini yapılandırın | Microsoft Docs"
description: "Ekleme, değiştirme ve bir ağ arabirimi için özel ve genel IP adresleri kaldırma öğrenin."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-network
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/24/2017
ms.author: jdial
ms.openlocfilehash: 47f72fcfe2a4c9ab6e89314a64dae0027ef76924
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/09/2018
---
# <a name="add-change-or-remove-ip-addresses-for-an-azure-network-interface"></a>Ekleme, değiştirme veya bir Azure ağ arabirimi için IP adreslerini kaldırın

Ekleme, değiştirme ve bir ağ arabirimi için genel ve özel IP adresleri kaldırma öğrenin. Özel IP adresleri bir ağ arabirimine atanmış bir Azure sanal ağı ve bağlı ağların diğer kaynaklar ile iletişim kurmak bir sanal makine etkinleştirin. Özel bir IP adresi ayrıca öngörülemeyen bir IP adresi kullanarak Internet'e giden iletişim sağlar. A [genel IP adresi](virtual-network-public-ip-address.md) bir ağ arabirimi etkinleştirir gelen iletişimi bir sanal makine için Internet'ten atanmış. Adres de tahmin edilebilir bir IP adresi kullanarak Internet'e sanal makineden giden iletişim sağlar. Ayrıntılar için bkz [azure'da giden bağlantılar anlama](../load-balancer/load-balancer-outbound-connections.md?toc=%2fazure%2fvirtual-network%2ftoc.json). 

Gerektiğinde oluşturmak için değiştirmek veya bir ağ arabirimi silin, okuyun [bir ağ arabirimi yönetmek](virtual-network-network-interface.md) makalesi. Ağ arabirimlerine eklemek veya ağ arabirimleri sanal okuma bir makineden kaldırmak gerekiyorsa [ekleme veya kaldırma ağ arabirimleri](virtual-network-network-interface-vm.md) makalesi. 


## <a name="before-you-begin"></a>Başlamadan önce

Bu makalenin herhangi bölümündeki tüm adımları tamamlanmadan önce aşağıdaki görevleri tamamlayın:

- Gözden geçirme [Azure sınırlar](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits) makale ortak ve özel IP adreslerini yönelik sınırlar hakkında bilgi edinin.
- Azure oturum açma [portal](https://portal.azure.com), Azure komut satırı arabirimi (CLI) ya da Azure PowerShell ile bir Azure hesabı. Zaten bir Azure hesabınız yoksa, kaydolun bir [ücretsiz deneme sürümü hesabı](https://azure.microsoft.com/free).
- Bu makalede, görevleri tamamlamak için PowerShell komutlarını kullanarak, [Azure PowerShell'i yükleme ve yapılandırma](/powershell/azureps-cmdlets-docs?toc=%2fazure%2fvirtual-network%2ftoc.json). Yüklü Azure PowerShell cmdlet'leri'nın en son sürümüne sahip olun. PowerShell komutlarıyla örnekler, yardım almanın yazın `get-help <command> -full`.
- Bu makalede, görevleri tamamlamak için Azure komut satırı arabirimi (CLI) komutlarını kullanarak, [Azure CLI'yi yükleme ve yapılandırma](/cli/azure/install-azure-cli?toc=%2fazure%2fvirtual-network%2ftoc.json). Yüklü Azure CLI'ın en son sürümüne sahip olun. CLI komutları için Yardım almak için yazın `az <command> --help`. CLI ve ön koşullar yüklemek yerine, Azure bulut Kabuğu'nu kullanabilirsiniz. Azure Cloud Shell doğrudan Azure portalının içinde çalıştırabileceğiniz ücretsiz bir Bash kabuğudur. Azure CLI, kabuğa önceden yüklenmiştir ve kabuk, hesabınızla birlikte kullanılacak şekilde yapılandırılmıştır. Bulut Kabuğu'nu kullanmak için bulut Kabuğu'nu tıklatın. **> _** en üstündeki düğmesi [portal](https://portal.azure.com).

## <a name="add-ip-addresses"></a>IP adreslerini ekleyin

Kadar ekleyebilirsiniz [özel](#private) ve [ortak](#public) [IPv4](#ipv4) adresleri listelenen sınırları içinde bir ağ arabirimi için gereken [Azuresınırlar](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits) makalesi. Portal (portal ağ arabirimi oluşturduğunuzda, bir ağ arabirimi için özel bir IPv6 adresi eklemek için kullanabileceğiniz rağmen) var olan bir ağ arabirimine bir IPv6 adresi eklemek için kullanamazsınız. PowerShell veya CLI için özel bir IPv6 adresi eklemek için kullanabileceğiniz [ikincil IP yapılandırması](#secondary) (var olduğu sürece hiçbir var olan ikincil IP yapılandırmaları) bir sanal makineye bağlı olmayan mevcut bir ağ arabirimi için. Bir ağ arabirimi genel bir IPv6 adresi eklemek için herhangi bir aracı kullanamazsınız. Bkz: [IPv6](#ipv6) IPv6 adresleri kullanma hakkında ayrıntılı bilgi için. 

1. Oturum [Azure portal](https://portal.azure.com) bir hesapla aboneliğiniz için ağ katılımcı rolü için diğer bir deyişle (en az) atanan izinleri. Okuma [Azure rol tabanlı erişim denetimi için yerleşik roller](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) rolleri ve izinleri hesaplarına atama hakkında daha fazla bilgi için makalenin.
2. Metni içeren kutusunda *arama kaynakları* Azure portalının en üstünde yazın *ağ arabirimleri*. Zaman **ağ arabirimleri** görünür arama sonuçlarında tıklatın.
3. İçinde **ağ arabirimleri** görünür, dikey penceresinde, bir IPv4 adresi için eklemek istediğiniz ağ arabirimi'ı tıklatın.
4. Tıklatın **IP yapılandırmaları** içinde **ayarları** bölümü, seçtiğiniz ağ arabirimi için dikey pencerenin.
5. Tıklatın **+ Ekle** açılan dikey penceresinde IP yapılandırmaları için.
6. Aşağıdakileri belirtin ve ardından **Tamam** kapatmak için **eklemek IP yapılandırması** dikey penceresinde:

    |Ayar|Gerekli mi?|Ayrıntılar|
    |---|---|---|
    |Ad|Evet|Ağ arabirimi için benzersiz olmalıdır|
    |Tür|Evet|Varolan bir ağ arabirimine bir IP yapılandırması ekleme ve her bir ağ arabirimine sahip olmalıdır bu yana bir [birincil](#primary) IP yapılandırması, tek seçenektir **ikincil**.|
    |Özel IP adres ataması yöntemi|Evet|[**Dinamik**](#dynamic): Azure ağ arabirimi dağıtılır alt ağ adres aralığı için sonraki kullanılabilir adresi atar. [**Statik**](#static): ağ arabirimi dağıtılır alt ağ adres aralığı için kullanılmayan bir adresi atayın.|
    |Genel IP adresi|Hayır|**Devre dışı:** hiçbir ortak IP adresi kaynağı şu anda IP yapılandırmasıyla ilişkilendirilmiş. **Etkin:** var olan bir IPv4 ortak IP adresi seçin veya yeni bir tane oluşturun. Bir ortak IP adresi oluşturmayı öğrenmek için okuma [ortak IP adresleri](virtual-network-public-ip-address.md#create-a-public-ip-address) makalesi.|
7. Yönergeleri izleyerek ikincil özel IP adresleri için sanal makine işletim sistemini el ile eklemeniz [sanal makine işletim sistemleri için birden çok IP adresi atamak](virtual-network-multiple-ip-addresses-portal.md#os-config) makalesi. Bkz: [özel](#private) el ile bir sanal makine işletim sistemine IP adreslerini eklemeden önce özel konular için IP adresi. Herhangi bir ortak IP adresi sanal makine işletim sistemine eklemeyin.

**Komutları**

|Aracı|Komut|
|---|---|
|CLI|[az ağ NIC IP-config oluşturma](/cli/azure/network/nic/ip-config?toc=%2fazure%2fvirtual-network%2ftoc.json#az_network_nic_ip_config_create)|
|PowerShell|[Add-AzureRmNetworkInterfaceIpConfig](/powershell/module/azurerm.network/add-azurermnetworkinterfaceipconfig?toc=%2fazure%2fvirtual-network%2ftoc.json)|

## <a name="change-ip-address-settings"></a>IP adresi ayarlarını değiştirme

Gerektiğinde bir IPv4 adresi atama yöntemini değiştirmek için statik IPv4 adresini değiştirin veya bir ağ arabirimine atanmış ortak IP adresini değiştirin. Özel bir IPv4 adresi, bir sanal makinede bir ikincil ağ arabirimi ile ilişkili bir ikincil IP yapılandırmasının değiştirirsiniz varsa (hakkında daha fazla bilgi [birincil ve ikincil ağ arabirimleri](virtual-network-network-interface-vm.md)), sanal makineyi yerleştirin Aşağıdaki adımları tamamlamadan önce durduruldu (serbest bırakıldığında) durumuna geçer: 

1. Oturum [Azure portal](https://portal.azure.com) bir hesapla aboneliğiniz için ağ katılımcı rolü için diğer bir deyişle (en az) atanan izinleri. Okuma [Azure rol tabanlı erişim denetimi için yerleşik roller](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) rolleri ve izinleri hesaplarına atama hakkında daha fazla bilgi için makalenin.
2. Metni içeren kutusunda *arama kaynakları* Azure portalının en üstünde yazın *ağ arabirimleri*. Zaman **ağ arabirimleri** görünür arama sonuçlarında tıklatın.
3. İçinde **ağ arabirimleri** görünür, dikey, IP adresi ayarlarını görüntülemek veya değiştirmek istediğiniz ağ arabirimi'ı tıklatın.
4. Tıklatın **IP yapılandırmaları** içinde **ayarları** bölümü, seçtiğiniz ağ arabirimi için dikey pencerenin.
5. Açılan dikey IP yapılandırmaları için listesinden değiştirmek istediğiniz IP Yapılandırması'nı tıklatın.
6. Ayarları hakkında bilgi yordamının 6. adımında ayarları kullanarak, istediğiniz değiştirme [bir IP Yapılandırması Ekle](#create-ip-config) bu makalenin. Tıklatın **kaydetmek** değiştirdiğiniz IP yapılandırması için dikey penceresini kapatın.

>[!NOTE]
>Birden fazla IP yapılandırması birincil ağ arabirimi varsa ve birincil IP yapılandırmasının özel IP adresini değiştirmek, birincil ve ikincil IP adresleri (Linux için gerekli değil) Windows içinde ağ arabirimi için el ile atamanız gerekir . El ile bir işletim sistemi içinde bir ağ arabirimi için IP adresleri atamak için okuma [birden çok IP adresi sanal makinelere atamak](virtual-network-multiple-ip-addresses-portal.md#os-config) makalesi. Bkz: [özel](#private) el ile bir sanal makine işletim sistemine IP adreslerini eklemeden önce özel konular için IP adresi. Herhangi bir ortak IP adresi sanal makine işletim sistemine eklemeyin.

**Komutları**

|Aracı|Komut|
|---|---|
|CLI|[az network nic ip-config update](/cli/azure/network/nic/ip-config?toc=%2fazure%2fvirtual-network%2ftoc.json#az_network_nic_ip_config_update)|
|PowerShell|[Set-AzureRMNetworkInterfaceIpConfig](/powershell/module/azurerm.network/set-azurermnetworkinterfaceipconfig?toc=%2fazure%2fvirtual-network%2ftoc.json)|

## <a name="remove-ip-addresses"></a>IP adreslerini kaldırın

Kaldırabileceğiniz [özel](#private) ve [ortak](#public) IP adresleri bir ağ arabiriminden, ancak bir ağ arabirimi her zaman kendisine atanmış en az bir özel bir IPv4 adresi olmalıdır.

1. Oturum [Azure portal](https://portal.azure.com) bir hesapla aboneliğiniz için ağ katılımcı rolü için diğer bir deyişle (en az) atanan izinleri. Okuma [Azure rol tabanlı erişim denetimi için yerleşik roller](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) rolleri ve izinleri hesaplarına atama hakkında daha fazla bilgi için makalenin.
2. Metni içeren kutusunda *arama kaynakları* Azure portalının en üstünde yazın *ağ arabirimleri*. Zaman **ağ arabirimleri** görünür arama sonuçlarında tıklatın.
3. İçinde **ağ arabirimleri** görünür dikey penceresinde, IP kaldırmak istediğiniz ağ arabirimi adresleri tıklatın.
4. Tıklatın **IP yapılandırmaları** içinde **ayarları** bölümü, seçtiğiniz ağ arabirimi için dikey pencerenin.
5. Sağ tıklatın bir [ikincil](#secondary) IP yapılandırması (silemezsiniz [birincil](#primary) yapılandırma) istediğiniz silmek için ' ı tıklatın **silmek**, ardından **Evet** silme işlemini onaylayın. Yapılandırma için ilişkili ortak bir IP adresi kaynağı varsa, kaynak IP yapılandırmasından ilkenin ilişkisi ancak kaynak silinmez.
6. Kapat **IP yapılandırmaları** dikey.

**Komutları**

|Aracı|Komut|
|---|---|
|CLI|[az ağ NIC IP-config Sil](/cli/azure/network/nic/ip-config?toc=%2fazure%2fvirtual-network%2ftoc.json#az_network_nic_ip_config_delete)|
|PowerShell|[Remove-AzureRmNetworkInterfaceIpConfig](/powershell/module/azurerm.network/remove-azurermnetworkinterfaceipconfig?toc=%2fazure%2fvirtual-network%2ftoc.json)|

## <a name="ip-configurations"></a>IP yapılandırması

[Özel](#private) ve (isteğe bağlı) [ortak](#public) IP adresleri, bir ağ arabirimine atanmış bir veya daha fazla IP yapılandırması atanır. İki tür IP yapılandırmaları vardır:

### <a name="primary"></a>Birincil

Her bir ağ arabirimine bir birincil IP yapılandırmasına atanır. Birincil bir IP yapılandırması:

- Sahip bir [özel](#private) [IPv4](#ipv4) kendisine atanmış adresi. Özel atayamazsınız [IPv6](#ipv6) birincil IP yapılandırmasının adresine.
- Ayrıca bir [ortak](#public) kendisine atanmış bir IPv4 adresi. Bir birincil veya ikincil IP yapılandırması için genel bir IPv6 adresi atanamıyor. Ancak, trafiği dengelemek için bir sanal makinenin özel IPv6 adresi yükleyebilir ve Azure yük dengeleyici, genel bir IPv6 adresi atayın. Daha fazla bilgi için bkz: [ayrıntıları ve IPv6 için sınırlamalar](../load-balancer/load-balancer-ipv6-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json#details-and-limitations).

### <a name="secondary"></a>İkincil

Birincil IP yapılandırmasına ek olarak, bir ağ arabirimine atanmış sıfır veya daha fazla ikincil IP yapılandırmasına sahip. İkincil bir IP yapılandırması:

- Özel bir IPv4 veya IPv6 adresi atanmış gerekir. Adresi IPv6 ise, ağ arabiriminin yalnızca bir ikincil IP yapılandırmasına sahip olabilir. Adresi IPv4 ise, ağ arabiriminin kendisine atanmış birden fazla ikincil IP yapılandırması olabilir. Kaç tane özel ve ortak IPv4 adresleri bir ağ arabirimine atanabilir hakkında daha fazla bilgi için bkz: [Azure sınırlar](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits) makalesi.  
- Özel IP adresi IPv4 ise da genel bir IPv4 adresi, atanmış. Özel IP adresi IPv6 ise, IP yapılandırması için genel bir IPv4 veya IPv6 adresi atanamıyor. Bir ağ arabirimine birden çok IP adresleri atama gibi senaryolarda kullanışlıdır:
    - Tek bir sunucuda farklı IP adreslerine ve SSL sertifikalarına sahip birden fazla web sitesi veya hizmetin barındırılması.
    - Bir Güvenlik Duvarı'nı veya yük dengeleyici gibi bir ağ sanal gereç olarak hizmet veren bir sanal makine.
    - Özel IPv4 adreslerini herhangi bir ağ arabirimleri için herhangi bir Azure yük dengeleyici arka uç havuzuna ekleme yeteneği. Geçmişte, yalnızca birincil IPv4 adresi birincil ağ arabirimi için bir arka uç havuzuna eklenemiyor. Birden çok IPv4 yapılandırmaları Yük Dengeleme hakkında daha fazla bilgi için bkz: [Yük Dengeleme birden fazla IP yapılandırması](../load-balancer/load-balancer-multiple-ip.md?toc=%2fazure%2fvirtual-network%2ftoc.json) makalesi. 
    - Yükleme özelliğini bir ağ arabirimine atanmış bir IPv6 adresi dengeleyin. Özel bir IPv6 adresi dengelemek hakkında daha fazla bilgi için bkz: [Yük Dengelemesi IPv6 adresleri](../load-balancer/load-balancer-ipv6-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json) makalesi.


## <a name="address-types"></a>Adres türleri

Aşağıdaki IP adreslerine türlerini atayabilirsiniz bir [IP yapılandırması](#ip-configurations):

### <a name="private"></a>Özel

Özel [IPv4](#ipv4) adresleri sanal ağ veya diğer bağlı ağlara diğer kaynakları ile iletişim kurmak bir sanal makine etkinleştirin. Bir sanal makine için gelen iletilen olamaz ya da sanal makineyi bir özel olan giden iletişim kurabilir [IPv6](#ipv6) adresiyle bir özel durum. Bir sanal makine, bir IPv6 adresi kullanarak Azure yük dengeleyici ile iletişim kurabilir. Daha fazla bilgi için bkz: [ayrıntıları ve IPv6 için sınırlamalar](../load-balancer/load-balancer-ipv6-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json#details-and-limitations). 

Varsayılan olarak, Azure DHCP sunucuları için özel bir IPv4 adresi atamak [birincil IP yapılandırmasının](#primary) ağ arabirimi sanal makine işletim sistemi içinde Azure ağ arabiriminin. Sürece gerekirse, hiçbir zaman el ile sanal makinenin işletim sistemi içinde bir ağ arabiriminin IP adresini ayarlamanız. 

> [!WARNING]
> IPv4 adresi birincil bir sanal makinenin işletim sistemi içinde bir ağ arabiriminin IP adresini olarak ayarlarsanız birincil ağ arabirimi birincil IP yapılandırmasının atanan özel bir IPv4 adresi herhangi bir zamanda farklı bir sanal makineye bağlı Azure içinde sanal makine bağlantısı kesilir.

Sanal makinenin işletim sistemi içinde bir ağ arabiriminin IP adresini el ile ayarlamak gerekli olduğu senaryolar vardır. Örneğin, el ile bir Windows işletim sistemi birincil ve ikincil IP adreslerini bir Azure sanal makinesi birden çok IP adresi eklerken ayarlamanız gerekir. Bir Linux sanal makine için yalnızca ikincil IP adreslerini el ile ayarlamanız gerekebilir. Bkz: [eklemek IP adresleri bir VM işletim sistemine](virtual-network-multiple-ip-addresses-portal.md#os-config) Ayrıntılar için. Bir IP yapılandırması için atanan adresi değiştirmek gerekiyorsa, önermiştir:

1. Sanal makineyi Azure DHCP sunucularından bir adresi aldığından emin olun. Bulduktan sonra işletim sisteminde DHCP dön IP adresinin atamasını değiştirmek ve sanal makineyi yeniden başlatın.
2. Durdur (deallocate) sanal makine.
3. Azure'daki IP yapılandırması için IP adresini değiştirin.
4. Sanal makineyi başlatın.
5. [El ile yapılandırmanız](virtual-network-multiple-ip-addresses-portal.md#os-config) işletim sistemi (ve ayrıca Windows içinde birincil IP adresi) Azure içinde Ayarla eşleşecek şekilde içindeki ikincil IP adresleri.
 
İzleyerek önceki adımlar, Azure içinde ve bir sanal makinenin işletim sistemi içinde ağ arabirimine atanmış özel IP adresi aynı kalır. Hangi sanal makinelerin IP adresleri için bir işletim sistemi içinde el ile ayarladınız, abonelik içindeki izlemek için Azure eklemeyi düşünün [etiketi](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json#tags) sanal makinelere. Kullanabileceğinize "IP adresi ataması: statik", örneğin. Bu şekilde, işletim sistemi içinde IP adresini el ile ayarladınız, abonelik içindeki sanal makineleri kolayca bulabilirsiniz.

Diğer kaynaklar aynı ya da bağlı sanal ağlar ile iletişim kurmak bir sanal makine etkinleştirmeye ek olarak, özel IP adresini de Internet'e giden iletişim kurmak bir sanal makine sağlar. Giden bağlantılar, Azure tarafından beklenmeyen bir ortak IP adresi çevrilmiş kaynak ağ adresi gerçekleştirilir. Azure giden Internet bağlantısı hakkında daha fazla bilgi için okuma [Azure giden Internet bağlantısı](../load-balancer/load-balancer-outbound-connections.md?toc=%2fazure%2fvirtual-network%2ftoc.json) makalesi. Gelen bir sanal makinenin özel IP adresi Internet üzerinden iletişim kuramıyor. Giden bağlantılar tahmin edilebilir bir ortak IP adresi gerekiyorsa, bir ağ arabirimi genel bir IP adresi kaynağı ilişkilendirin.

### <a name="public"></a>Genel

Genel bir IP adresi kaynağı atanmış ortak IP adresleri, Internet'ten bir sanal makineye gelen bağlantı etkinleştirin. İnternet giden bağlantılar tahmin edilebilir bir IP adresi kullanın. Bkz: [azure'da giden bağlantılar anlama](../load-balancer/load-balancer-outbound-connections.md?toc=%2fazure%2fvirtual-network%2ftoc.json) Ayrıntılar için. Bir ortak IP adresi bir IP yapılandırmasına atayabilir, ancak gerekli değildir. Genel bir IP adresi kaynağı ilişkilendirerek bir sanal makine için bir ortak IP adresi atamazsanız, sanal makine hala internet giden iletişim kurabilir. Bu durumda, özel IP adresi Azure tarafından beklenmeyen bir ortak IP adresi çevrilmiş kaynak ağ adresi değil. Genel IP adresi kaynakları hakkında daha fazla bilgi için bkz: [genel IP adresi kaynağı](virtual-network-public-ip-address.md).

Bir ağ arabirimine atayabilirsiniz özel ve genel IP adresi sayısı sınırı vardır. Ayrıntılar için [Azure sınırlar](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits) makalesi.

> [!NOTE]
> Azure genel bir IP adresi için bir sanal makinenin özel IP adresi çevirir. Sonuç olarak, bir sanal makinenin işletim sistemi; böylece herhangi bir zamanda el ile işletim sistemi içinde genel bir IP adresi atamak için gerekli kendisine atanmış bir ortak IP adresinin farkında değildir.

## <a name="assignment-methods"></a>Atama yöntemleri

Ortak ve özel IP adreslerini aşağıdaki atama yöntemlerden birini kullanarak atanır:

### <a name="dynamic"></a>Dinamik

(İsteğe bağlı) dinamik özel IPv4 ve IPv6 adresleri varsayılan olarak atanır. 

- **Yalnızca ortak**: Azure her bir Azure bölgesine benzersiz bir aralıktan adına atar. Hangi aralıkları her bölgeye atanan öğrenmek için bkz: [Microsoft Azure veri merkezi IP aralıkları](https://www.microsoft.com/download/details.aspx?id=41653). Bir sanal makine (serbest bırakıldığında) yeniden başlatıldı durdurulduğunda adres değiştirebilirsiniz. Atama yöntemlerden birini kullanarak bir IP yapılandırması için genel bir IPv6 adresi atanamıyor.
- **Yalnızca özel**: Azure her alt ağ adres aralığındaki ilk dört adresini ayırır ve adresleri atamaz. Azure, alt ağ adres aralığından sonraki kullanılabilir adresi bir kaynağa atar. Örneğin, alt ağ adres aralığının 10.0.0.0/16 olduğunu varsayarsak ve 10.0.0.0.4-10.0.0.14 adresleri zaten atanmışsa (.0-.3 ayrılmıştır), Azure kaynağa 10.0.0.15 adresini atar. Dinamik, varsayılan ayırma yöntemidir. Dinamik IP adresleri bir kez atandıktan sonra, ancak ağ arabirimi silinirse, aynı sanal ağ içinde farklı bir alt ağa atanırsa veya ayırma yöntemi statik olarak değiştirilip farklı bir IP adresi belirtilirse serbest bırakılır. Varsayılan olarak, dinamik olan ayırma yöntemini statik olarak değiştirdiğinizde Azure dinamik olarak atanmış önceki adresi statik adres olarak atar. Yalnızca dinamik atama yöntemini kullanarak özel bir IPv6 adresi atayabilirsiniz.

### <a name="static"></a>Statik

(İsteğe bağlı olarak), bir IP yapılandırması için genel veya özel statik bir IPv4 adresi atayabilirsiniz. Bir IP yapılandırması için bir statik genel veya özel bir IPv6 adresi atanamıyor. Azure statik genel IPv4 adresi nasıl atar hakkında daha fazla bilgi için bkz: [genel IP adresi](virtual-network-public-ip-address.md) makalesi.

- **Yalnızca ortak**: Azure her bir Azure bölgesine benzersiz bir aralıktan adına atar. Hangi aralıkları her bölgeye atanan öğrenmek için bkz: [Microsoft Azure veri merkezi IP aralıkları](https://www.microsoft.com/download/details.aspx?id=41653). Adres için atanan ortak IP adresi kaynağı silinmiş veya atama yöntemi dinamik değiştirildiğinde kadar değiştirmez. Bir IP yapılandırması için genel bir IP adresi kaynağı ilişkiliyse, kendi atama yöntemi değiştirmeden önce IP yapılandırmasından ilişkisi gerekir.
- **Yalnızca özel**: seçin ve alt ağın adres aralığından bir adresi atayın. Alt ağ adres aralığından atadığınız adres, alt ağa adres aralığının ilk dört adresi dışında kalan ve şu anda alt ağda başka bir kaynağa atanmamış olan herhangi bir adres olabilir. Statik adresler ancak ağ arabirimi silindiğinde serbest bırakılır. Ayırma yöntemini dinamik olarak değiştirirseniz, Azure daha önce statik IP adresi olarak atanmış olan adresi dinamik adres olarak atar. Bu adres, alt ağ adres aralığında bir sonraki kullanılabilir adres olmayabilir. Ağ arabirimi aynı sanal ağ içinde farklı bir alt ağa atandığında da adres değişir; ama ağ arabirimini farklı bir alt ağa atamak için, önce statik ayırma yöntemini dinamik olarak değiştirmeniz gerekir. Ağ arabirimini farklı bir alt ağa atadıktan sonra, ayırma yöntemini yine statik yapabilir ve yeni alt ağ adres aralığından bir IP adresi atayabilirsiniz.

## <a name="ip-address-versions"></a>IP adresi sürümleri

Adresleri atarken aşağıdaki sürümlerini belirtebilirsiniz:

### <a name="ipv4"></a>IPv4

Her bir ağ arabirimine bir olmalıdır [birincil](#primary) atanmış bir IP yapılandırmasıyla [özel](#private) [IPv4](#ipv4) adresi. Bir veya daha fazla ekleyebilirsiniz [ikincil](#secondary) her sahip özel bir IPv4 ve (isteğe bağlı) bir IPv4 IP yapılandırmaları [ortak](#public) IP adresi.

### <a name="ipv6"></a>IPv6

Sıfır veya bir özel atayabilirsiniz [IPv6](#ipv6) adresine bir ağ arabirimi bir ikincil IP yapılandırması. Ağ arabirimi var olan tüm ikincil IP yapılandırmaları sahip olamaz. Portalı kullanarak bir IPv6 adresi ile bir IP yapılandırması ekleyemezsiniz. Varolan bir ağ arabirimine sahip özel bir IPv6 adresi bir IP yapılandırması eklemek için PowerShell veya CLI kullanın. Ağ arabirimi için mevcut bir VM'yi eklenemiyor.

> [!NOTE]
> Portalı kullanarak bir IPv6 adresiyle bir ağ arabirimi oluşturabilirsiniz ancak Portalı'nı kullanarak yeni veya var olan bir sanal makineye mevcut bir ağ arabirimini ekleyemezsiniz. Özel bir IPv6 adresiyle bir ağ arabirimi oluşturmak için PowerShell veya Azure CLI 2.0 kullanın, sonra bir sanal makine oluştururken, ağ arabirimi ekleyin. Varolan bir sanal makineye atanmış özel bir IPv6 adresine sahip bir ağ arabirimine eklenemiyor. Herhangi bir aracı (portal, CLI veya PowerShell) kullanarak bir sanal makineye bağlı herhangi bir ağ arabirimi için bir IP yapılandırmasına özel bir IPv6 adresi ekleyemezsiniz.

Bir birincil veya ikincil IP yapılandırması için genel bir IPv6 adresi atanamıyor.

## <a name="skus"></a>SKU'lar

Bir ortak IP adresi ile temel veya standart SKU oluşturulur.  SKU farklar hakkında daha fazla ayrıntı için bkz: [yönetmek genel IP adresleri](virtual-network-public-ip-address.md).

> [!NOTE]
> Standart bir SKU genel IP adresini bir sanal makinenin ağ arabirimine atadığınızda amaçlanan trafiğe bir [ağ güvenlik grubuyla](security-overview.md#network-security-groups) açıkça izin vermeniz gerekir. Bir ağ güvenlik grubu oluşturup ilişkilendirene ve istenen trafiğe açıkça izin verene kadar kaynakla erişim kurma girişimleri başarısız olur.

## <a name="next-steps"></a>Sonraki adımlar
Farklı IP yapılandırmaları ile bir sanal makine oluşturmak için aşağıdaki makaleyi okuyun:

|Görev|Aracı|
|---|---|
|Birden çok NIC ile VM oluşturma|[CLI](../virtual-machines/linux/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json), [PowerShell](../virtual-machines/windows/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json)|
|Birden çok IPv4 adresleriyle tek bir NIC VM oluşturma|[CLI](virtual-network-multiple-ip-addresses-cli.md), [PowerShell](virtual-network-multiple-ip-addresses-powershell.md)|
|Özel bir IPv6 adresi (arkasında bir Azure yük dengeleyici) ile tek bir NIC VM oluşturma|[CLI](../load-balancer/load-balancer-ipv6-internet-cli.md?toc=%2fazure%2fvirtual-network%2ftoc.json), [PowerShell](../load-balancer/load-balancer-ipv6-internet-ps.md?toc=%2fazure%2fvirtual-network%2ftoc.json), [Azure Resource Manager şablonu](../load-balancer/load-balancer-ipv6-internet-template.md?toc=%2fazure%2fvirtual-network%2ftoc.json)|
