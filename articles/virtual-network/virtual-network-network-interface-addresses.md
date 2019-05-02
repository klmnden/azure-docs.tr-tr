---
title: Azure ağ arabirimi için IP adreslerini yapılandırın | Microsoft Docs
description: Ekleme, değiştirme ve bir ağ arabirimi için özel ve genel IP adreslerini kaldırın öğrenin.
services: virtual-network
documentationcenter: na
author: KumudD
manager: twooley
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-network
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/24/2017
ms.author: kumud
ms.openlocfilehash: a6635b811dfa9c46facfffee1c57b2871cb4c738
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64719712"
---
# <a name="add-change-or-remove-ip-addresses-for-an-azure-network-interface"></a>Ekleme, değiştirme veya bir Azure ağ arabirimi için IP adreslerini kaldırın

Ekleme, değiştirme ve bir ağ arabirimi için genel ve özel IP adreslerini kaldırın öğrenin. Bir ağ arabirimine atanmış özel IP adresleri, bir sanal makine, bir Azure sanal ağı ve bağlı ağların diğer kaynaklarla iletişim kurmak etkinleştirin. Özel bir IP adresi de öngörülemeyen bir IP adresi kullanarak İnternet'e giden iletişim sağlar. A [genel IP adresi](virtual-network-public-ip-address.md) bir ağ arabirimi etkinleştirir gelen iletişimi bir sanal makineye Internet'ten atanmış. Adres, ayrıca öngörülebilir bir IP adresi kullanarak İnternet'e sanal makineden giden iletişim sağlar. Ayrıntılar için bkz [azure'da giden bağlantıları anlama](../load-balancer/load-balancer-outbound-connections.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

Gerektiğinde oluşturmak için değiştirmek veya bir ağ arabirimi silme, okuma [bir ağ arabirimi yönetmek](virtual-network-network-interface.md) makalesi. Ağ arabirimlerine ekleyin veya ağ arabirimleri sanal okuma bir makineden kaldırmak gerekiyorsa [ekleme veya kaldırma ağ arabirimleri](virtual-network-network-interface-vm.md) makalesi.

## <a name="before-you-begin"></a>Başlamadan önce

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Bu makalenin bir bölümündeki adımları tamamlamadan önce aşağıdaki görevleri tamamlayın:

- Azure hesabınız yoksa, kaydolmaya bir [ücretsiz deneme hesabınızı](https://azure.microsoft.com/free).
- Portalı kullanarak, açık https://portal.azure.comve Azure hesabınızda oturum.
- Bu makaledeki görevleri tamamlamak için PowerShell komutlarını kullanarak, ya da komutları çalıştırmak [Azure Cloud Shell](https://shell.azure.com/powershell), veya PowerShell bilgisayarınızdan çalıştırarak. Azure Cloud Shell, bu makaledeki adımları çalıştırmak için kullanabileceğiniz ücretsiz bir etkileşimli kabuktur. Yaygın Azure araçları, kabuğa önceden yüklenmiştir ve kabuk, hesabınızla birlikte kullanılacak şekilde yapılandırılmıştır. Bu öğretici Azure PowerShell modülü sürüm 1.0.0 gerektirir veya üzeri. Yüklü sürümü bulmak için `Get-Module -ListAvailable Az` komutunu çalıştırın. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-az-ps). PowerShell'i yerel olarak çalıştırıyorsanız Azure bağlantısı oluşturmak için `Connect-AzAccount` komutunu da çalıştırmanız gerekir.
- Bu makaledeki görevleri tamamlamak için Azure komut satırı arabirimi (CLI) komutlarını kullanarak, ya da komutları çalıştırmak [Azure Cloud Shell](https://shell.azure.com/bash), veya bilgisayarınızdan CLI çalıştırarak. Bu öğretici, Azure CLI Sürüm 2.0.31 gerektirir veya üzeri. Yüklü sürümü bulmak için `az --version` komutunu çalıştırın. Yükleme veya yükseltme yapmanız gerekiyorsa bkz. [Azure CLI'yı yükleme](/cli/azure/install-azure-cli). Azure CLI'yi yerel olarak çalıştırıyorsanız, aynı zamanda çalıştırmak ihtiyacınız `az login` Azure ile bir bağlantı oluşturmak için.

Oturum açın ya da Azure ile bağlandığınız hesabı atanmalıdır [ağ Katılımcısı](../role-based-access-control/built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) rolü veya bir [özel rol](../role-based-access-control/custom-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json) içinde listelenen uygun eylemleri atanan [ağ Arabirim izinleri](virtual-network-network-interface.md#permissions).

## <a name="add-ip-addresses"></a>IP adreslerini ekleyin

Kadar ekleyebilirsiniz [özel](#private) ve [genel](#public) [IPv4](#ipv4) adresleri listelenen limitler dahilinde bir ağ arabirimi için gerekirse [Azurelimitleri](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits) makalesi. Portal (portal ağ arabirimini oluşturun, bir ağ arabirimi için özel bir IPv6 adresi eklemek için kullanabileceğiniz rağmen) bir IPv6 adresi için var olan bir ağ arabirimi eklemek için kullanamazsınız. PowerShell veya CLI için özel bir IPv6 adresi eklemek için kullanabileceğiniz [ikincil IP yapılandırması](#secondary) (var olduğu sürece hiçbir mevcut ikincil IP yapılandırmaları) bir sanal makineye bağlı olmayan mevcut ağ arabirimi için. Bir ağ arabirimine bir genel IPv6 adresi eklemek için herhangi bir aracı kullanamazsınız. Bkz: [IPv6](#ipv6) IPv6 adresleri kullanma hakkında ayrıntılı bilgi.

1. Metni içeren kutuya *kaynak Ara* Azure portalının üst kısmında, yazın *ağ arabirimleri*. Zaman **ağ arabirimleri** arama sonuçlarında görünmesini, onu seçin.
2. Listeden bir IPv4 adresi için eklemek istediğiniz ağ arabirimi seçin.
3. Altında **ayarları**seçin **IP yapılandırmaları**.
4. Altında **IP yapılandırmaları**seçin **+ Ekle**.
5. Aşağıdakileri belirtin ve ardından **Tamam**:

   |Ayar|Gerekli mi?|Ayrıntılar|
   |---|---|---|
   |Ad|Evet|Ağ arabirimi için benzersiz olmalıdır|
   |Tür|Evet|Varolan bir ağ arabirimi IP yapılandırması ekleme ve her ağ arabirimine sahip olmalıdır bir [birincil](#primary) IP yapılandırması, tek seçeneğiniz olduğunu **ikincil**.|
   |Özel IP adresi ataması yöntemi|Evet|[**Dinamik**](#dynamic): Azure ağ arabirimi dağıtıldığı alt ağ adres aralığı için bir sonraki kullanılabilir adresi atar. [**Statik**](#static): Ağ arabiriminin dağıtıldığı alt ağ adres aralığı için kullanılmayan bir adresi atayın.|
   |Genel IP adresi|Hayır|**Devre dışı:** Genel IP adresine kaynak IP yapılandırması için şu anda ilişkilidir. **Etkin:** Var olan bir IPv4 genel IP adresi seçin veya yeni bir tane oluşturun. Genel IP adresi oluşturma konusunda bilgi edinmek için [genel IP adresleri](virtual-network-public-ip-address.md#create-a-public-ip-address) makalesi.|
6. İkincil özel IP adresleri yönergeleri izleyerek sanal makine işletim sistemini el ile eklemeniz [birden çok IP adresi sanal makine işletim sistemlerine atayın](virtual-network-multiple-ip-addresses-portal.md#os-config) makalesi. Bkz: [özel](#private) el ile bir sanal makine işletim sistemine IP adresleri eklemeden önce özel konular için IP adresi. Herhangi bir genel IP adresleri, sanal makine işletim sistemine eklemeyin.

**Komutları**

|Tool|Komut|
|---|---|
|CLI|[az network nic ip-config create](/cli/azure/network/nic/ip-config)|
|PowerShell|[Add-AzNetworkInterfaceIpConfig](/powershell/module/az.network/add-aznetworkinterfaceipconfig)|

## <a name="change-ip-address-settings"></a>IP adresi ayarlarını değiştir

Gerektiğinde bir IPv4 adresi atama yöntemini değiştirmek için statik IPv4 adresini değiştirmek veya bir ağ arabirimine atanan genel IP adresini değiştirin. Bir sanal makinede bir ikincil ağ arabirimi ile ilişkili bir ikincil IP yapılandırmasının özel IPv4 adresi değiştirmekte olduğunuz varsa (daha fazla bilgi edinin [birincil ve ikincil ağ arabirimleri](virtual-network-network-interface-vm.md)), sanal makineyi yerleştirmek Aşağıdaki adımları tamamlanmadan önce durduruldu (serbest bırakıldığında) duruma:

1. Metni içeren kutuya *kaynak Ara* Azure portalının üst kısmında, yazın *ağ arabirimleri*. Zaman **ağ arabirimleri** arama sonuçlarında görünmesini, onu seçin.
2. Görüntülemek veya listeden IP adresi ayarlarını değiştirmek istediğiniz ağ arabirimi seçin.
3. Altında **ayarları**seçin **IP yapılandırmaları**.
4. Listeden değiştirmek istediğiniz IP Yapılandırması'nı seçin.
5. 5. adımda ayarlarıyla ilgili bilgileri kullanarak, istenen ayarları [IP Yapılandırması Ekle](#add-ip-addresses).
6. **Kaydet**’i seçin.

>[!NOTE]
>Birden fazla IP yapılandırması birincil ağ arabirimi varsa ve birincil IP yapılandırmasının özel IP adresini değiştirmek, birincil ve ikincil IP adreslerini ağ arabirimine (Linux için gerekli değildir) Windows içinde el ile yeniden atamanız gerekir . El ile bir ağ arabirimi bir işletim sistemi içinde IP adresleri atamak için bkz: [birden çok IP adresi için sanal makinelere Atayabilmenizi](virtual-network-multiple-ip-addresses-portal.md#os-config). El ile bir sanal makine işletim sistemine IP adresleri eklemeden önce özel konular için bkz. [özel](#private) IP adresleri. Herhangi bir genel IP adresleri, sanal makine işletim sistemine eklemeyin.

**Komutları**

|Tool|Komut|
|---|---|
|CLI|[az ağ NIC IP-config update](/cli/azure/network/nic/ip-config)|
|PowerShell|[Set-AzNetworkInterfaceIpConfig](/powershell/module/az.network/set-aznetworkinterfaceipconfig)|

## <a name="remove-ip-addresses"></a>IP adreslerini kaldırın

Kaldırabilirsiniz [özel](#private) ve [genel](#public) bir ağ arabirimi IP adresleri, ancak bir ağ arabirimi her zaman özel en az bir IPv4 adresi atanmış olmalıdır.

1. Metni içeren kutuya *kaynak Ara* Azure portalının üst kısmında, yazın *ağ arabirimleri*. Zaman **ağ arabirimleri** arama sonuçlarında görünmesini, onu seçin.
2. IP adresleri listesinden kaldırmak istediğiniz ağ arabirimi seçin.
3. Altında **ayarları**seçin **IP yapılandırmaları**.
4. Sağ tıklayarak seçin bir [ikincil](#secondary) IP yapılandırması (nelze odstranit [birincil](#primary) yapılandırması) seçin, silmek istediğiniz **Sil**, ardından  **Evet**, silmeyi onaylamak için. Yapılandırma ile ilişkili bir genel IP adresi kaynağı varsa, kaynak IP yapılandırmasından ilkenin ilişkisi, ancak kaynak silinmez.

**Komutları**

|Tool|Komut|
|---|---|
|CLI|[az ağ NIC IP yapılandırmasını Sil](/cli/azure/network/nic/ip-config)|
|PowerShell|[Remove-AzNetworkInterfaceIpConfig](/powershell/module/az.network/remove-aznetworkinterfaceipconfig)|

## <a name="ip-configurations"></a>IP yapılandırmaları

[Özel](#private) ve (isteğe bağlı olarak) [genel](#public) bir ağ arabirimine atanan bir veya daha fazla IP yapılandırması için IP adresi atanır. İki tür IP yapılandırmaları vardır:

### <a name="primary"></a>Birincil

Her ağ arabirimine adet birincil IP yapılandırmasına atanır. Birincil IP yapılandırması:

- Sahip bir [özel](#private) [IPv4](#ipv4) adresi atanmış. Özel bir atanamaz [IPv6](#ipv6) adresi için bir birincil IP yapılandırması.
- Ayrıca olabilir bir [genel](#public) atanmış IPv4 adresi. Bir birincil veya ikincil IP yapılandırması için genel IPv6 adresi atanamıyor. Bununla birlikte, bir sanal makinenin özel IPv6 adresine trafiğin yükünü dengelemenizi yükleyebilir ve bir Azure yük dengeleyici genel IPv6 adresi atayın. Daha fazla bilgi için [ayrıntıları ve IPv6 için sınırlamalar](../load-balancer/load-balancer-ipv6-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json#details-and-limitations).

### <a name="secondary"></a>İkincil

Bir birincil IP yapılandırmasına ek olarak, bir ağ arabirimi atanmış sıfır veya daha fazla ikincil IP yapılandırmaları olabilir. İkincil bir IP yapılandırması:

- Özel bir IPv4 veya IPv6 adresi kendisine atanmış olmalıdır. IPv6 adresi ise ağ arabirimi yalnızca bir ikincil IP yapılandırmasına sahip olabilir. IPv4 adresi ise ağ arabirimine atanmış birden fazla ikincil IP yapılandırmaları olabilir. Kaç özel ve genel IPv4 adresi bir ağ arabirimine atanabilir hakkında daha fazla bilgi için bkz: [Azure limitleri](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits) makalesi.
- Özel IP adresi IPv4 ise ayrıca genel bir IPv4 adresi, atanmış. Özel IP adresi IPv6 ise, IP yapılandırması için genel bir IPv4 veya IPv6 adresi atanamıyor. Bir ağ arabirimi için birden çok IP adresleri atama gibi senaryolarda yararlıdır:
  - Tek bir sunucuda farklı IP adreslerine ve SSL sertifikalarına sahip birden fazla web sitesi veya hizmetin barındırılması.
  - Bir güvenlik duvarı veya yük dengeleyici gibi bir ağ sanal Gereci olarak hizmet veren bir sanal makine.
  - Tüm ağ arabirimleri için özel IPv4 adreslerinin herhangi bir Azure Load Balancer arka uç havuzuna ekleme yeteneği. Geçmişte, arka uç havuzuna yalnızca birincil IPv4 adresi için birincil ağ arabirimi eklenemedi. Birden fazla IPv4 yapılandırması Yük Dengeleme hakkında daha fazla bilgi edinmek için bkz. [birden fazla IP yapılandırmasının Yük Dengelemesi](../load-balancer/load-balancer-multiple-ip.md?toc=%2fazure%2fvirtual-network%2ftoc.json) makalesi. 
  - Yükleme olanağı, bir ağ arabirimine atanmış bir IPv6 adresi dengeleyin. Yük Dengelemesi özel bir IPv6 adresi için hakkında daha fazla bilgi için bkz: [Yük Dengeleme IPv6 adresleri](../load-balancer/load-balancer-ipv6-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json) makalesi.

## <a name="address-types"></a>Adres türü

Aşağıdaki tür için IP adresi atamak için bir [IP yapılandırması](#ip-configurations):

### <a name="private"></a>Özel

Özel [IPv4](#ipv4) adresleri diğer kaynaklara bir sanal ağ ya da bağlı diğer ağlar ile iletişim kurmak bir sanal makine etkinleştirin. Bir sanal makine için gelen iletileceği olamaz ya da sanal makineyi bir özel ile giden iletişim kurabilmesi [IPv6](#ipv6) adresiyle bir özel durum. Bir sanal makine, bir IPv6 adresi kullanarak bir Azure yük dengeleyici ile iletişim kurabilir. Daha fazla bilgi için [ayrıntıları ve IPv6 için sınırlamalar](../load-balancer/load-balancer-ipv6-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json#details-and-limitations).

Varsayılan olarak, Azure DHCP sunucuları için özel IPv4 adresi atayın [birincil IP yapılandırması](#primary) sanal makine işletim sistemi içinde ağ arabirimine Azure ağ arabiriminin. Sürece gerekirse, hiçbir zaman el ile bir ağ arabirimi sanal makinenin işletim sistemi içinde IP adresi ayarlamanız.

> [!WARNING]
> IPv4 adresini bir sanal makinenin işletim sistemi içinde bir ağ arabiriminin birincil IP adresi olarak ayarlarsanız birincil ağ arabiriminin birincil IP yapılandırması için atanmış olan özel IPv4 adresi her zamankinden farklı bir sanal makineye eklenir Azure içinde sanal makine bağlantısını kaybedebilir.

Bir ağ arabirimi sanal makinenin işletim sistemi içinde IP adresini el ile ayarlamak gerekli olduğu senaryolar da vardır. Örneğin, el ile bir Windows işletim sistemi birincil ve ikincil IP adreslerini birden çok IP adresi için bir Azure sanal makinesine eklerken ayarlamanız gerekir. Bir Linux sanal makine için yalnızca ikincil IP adreslerini el ile ayarlamanız gerekebilir. Bkz: [ekleme IP adresleri için bir VM işletim sistemi](virtual-network-multiple-ip-addresses-portal.md#os-config) Ayrıntılar için. Bir IP yapılandırması için atanmış olan adresi değiştirmek gerekiyorsa, önerilir:

1. Sanal makinenin adres Azure DHCP sunucularından aldığından emin olun. Sonra DHCP işletim sistemi içinde IP adresi atamasının geri değiştirin ve sanal makineyi yeniden başlatın.
2. Durdurun (serbest bırakın) sanal makine.
3. Azure'daki IP yapılandırması için IP adresini değiştirin.
4. Sanal makineyi başlatın.
5. [El ile yapılandırmanız](virtual-network-multiple-ip-addresses-portal.md#os-config) işletim sistemi (ve ayrıca Windows içinde birincil IP adresi) Azure içinde ayarlanmış eşleştirmek için ikincil IP adresinden.

Önceki adımlar, Azure içindeki ve içinde bir sanal makinenin işletim sistemi, ağ arabirimine atanmış özel IP adresini izleyerek aynı kalır. Hangi sanal makinelerin, bir işletim sistemi içinde IP adreslerini el ile ayarlamanız, aboneliğiniz kapsamındaki izlemek için bir Azure eklemeyi göz önünde bulundurun [etiketi](../azure-resource-manager/resource-group-using-tags.md) sanal makineleri için. Kullanabileceğinize "IP adresi ataması: Statik", örneğin. Bu şekilde, işletim sistemi içinde IP adresini el ile ayarlamanız aboneliğinizde bulunan sanal makineleri kolayca bulabilirsiniz.

Bir sanal makine aynı ya da bağlı sanal ağlar içinde diğer kaynaklarla iletişim kurmak etkinleştirmeye ek olarak, özel bir IP adresi de Internet'e giden iletişim kurmak bir sanal makine sağlar. Giden bağlantılar, Azure tarafından beklenmeyen bir genel IP adresine çevrilen kaynak ağ adresi gerçekleştirilir. Azure giden Internet bağlantısı hakkında daha fazla bilgi edinmek için [Azure giden Internet bağlantısı](../load-balancer/load-balancer-outbound-connections.md?toc=%2fazure%2fvirtual-network%2ftoc.json) makalesi. Gelen bir sanal makinenin özel IP adresi için Internet'ten iletişim kuramıyor. Giden bağlantılar tahmin edilebilir bir genel IP adresi gerekiyorsa, genel bir IP adresi kaynağı bir ağ arabirimine ilişkilendirin.

### <a name="public"></a>Genel

Genel bir IP adresi kaynağına atanmış genel IP adresleri, Internet'ten bir sanal makineye gelen bağlantılara etkinleştirin. İnternet'e giden bağlantılar, tahmin edilebilir bir IP adresi kullanın. Bkz: [azure'da giden bağlantıları anlama](../load-balancer/load-balancer-outbound-connections.md?toc=%2fazure%2fvirtual-network%2ftoc.json) Ayrıntılar için. Bir IP yapılandırması için genel bir IP adresi atama, ancak gerekli değildir. Bir genel IP adresi kaynağı ile ilişkilendirilmesi yoluyla bir sanal makineye bir genel IP adresi atamazsanız, sanal makine hala İnternet'e giden iletişim kurabilir. Bu durumda, özel IP adresini kaynak ağ adresi öngörülemeyen bir genel IP adresleri Azure tarafından çevrilmiş ' dir. Genel IP adresi kaynakları hakkında daha fazla bilgi için bkz: [genel IP adresi kaynağı](virtual-network-public-ip-address.md).

Bir ağ arabirimine atayabilirsiniz özel ve genel IP adresi sayısını limitleri vardır. Ayrıntılı bilgi edinmek için [Azure limitleri](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits) makalesi.

> [!NOTE]
> Azure, sanal makinenin özel IP adresi genel IP adresine çevrilir. Sonuç olarak, bir sanal makinenin işletim sistemi vardır, bu nedenle hiç olmadığı kadar el ile işletim sistemi içinde genel IP adresi atamak için gerek kendisine atanmış bir genel IP adresinin farkında değil.

## <a name="assignment-methods"></a>Atama yöntemi

Genel ve özel IP adresleri atama aşağıdaki yöntemlerden birini kullanarak atanır:

### <a name="dynamic"></a>Dinamik

(İsteğe bağlı) özel dinamik IPv4 ve IPv6 adresleri varsayılan olarak atanır.

- **Yalnızca genel**: Azure, her bir Azure bölgesine adresi benzersiz aralıktan atar. Hangi aralıkları her bir bölgeye atanan bilgi edinmek için [Microsoft Azure veri merkezi IP aralıkları](https://www.microsoft.com/download/details.aspx?id=41653). Bir sanal makine (serbest bırakıldı) yeniden Başlatmadığınız durdurulduğunda adresi değişebilir. Atama yöntemlerden birini kullanarak bir IP yapılandırması için genel IPv6 adresi atanamıyor.
- **Yalnızca özel**: Azure her alt ağ adres aralığında ilk dört adresi ayırır ve bu adresleri atamaz. Azure, alt ağ adres aralığından sonraki kullanılabilir adresi bir kaynağa atar. Örneğin, alt ağ adres aralığının 10.0.0.0/16 olduğunu varsayarsak ve 10.0.0.0.4-10.0.0.14 adresleri zaten atanmışsa (.0-.3 ayrılmıştır), Azure kaynağa 10.0.0.15 adresini atar. Dinamik, varsayılan ayırma yöntemidir. Dinamik IP adresleri bir kez atandıktan sonra, ancak ağ arabirimi silinirse, aynı sanal ağ içinde farklı bir alt ağa atanırsa veya ayırma yöntemi statik olarak değiştirilip farklı bir IP adresi belirtilirse serbest bırakılır. Varsayılan olarak, dinamik olan ayırma yöntemini statik olarak değiştirdiğinizde Azure dinamik olarak atanmış önceki adresi statik adres olarak atar. Yalnızca dinamik atama yöntemiyle özel bir IPv6 adresi de atayabilirsiniz.

### <a name="static"></a>Statik

(İsteğe bağlı olarak), bir IP yapılandırmasına bir genel veya özel statik IPv4 adresi atayabilirsiniz. Bir IP yapılandırması için statik bir genel veya özel IPv6 adresi atanamıyor. Azure'nın statik genel IPv4 adreslerinin nasıl atar hakkında daha fazla bilgi için bkz: [genel IP adresleri](virtual-network-public-ip-address.md).

- **Yalnızca genel**: Azure, her bir Azure bölgesine adresi benzersiz aralıktan atar. Azure [Genel](https://www.microsoft.com/download/details.aspx?id=56519), [US government](https://www.microsoft.com/download/details.aspx?id=57063), [Çin](https://www.microsoft.com/download/details.aspx?id=57062) ve [Almanya](https://www.microsoft.com/download/details.aspx?id=57064) bulutları için bu aralıkların (ön ekler) listesini indirebilirsiniz. Dinamik atama yöntemini değişmesi veya atanan genel IP adresi kaynağı silinmiş kadar adresi değişmez. Genel IP adresi kaynağına bir IP yapılandırmasıyla ilişkili ise, atama yöntemi değiştirmeden önce IP yapılandırmasından ilişkisi olmalıdır.
- **Yalnızca özel**: Seçin ve alt ağın adres aralığından bir adresi atayabilirsiniz. Alt ağ adres aralığından atadığınız adres, alt ağa adres aralığının ilk dört adresi dışında kalan ve şu anda alt ağda başka bir kaynağa atanmamış olan herhangi bir adres olabilir. Statik adresler ancak ağ arabirimi silindiğinde serbest bırakılır. Ayırma yöntemini dinamik olarak değiştirirseniz, Azure daha önce statik IP adresi olarak atanmış olan adresi dinamik adres olarak atar. Bu adres, alt ağ adres aralığında bir sonraki kullanılabilir adres olmayabilir. Ağ arabirimi aynı sanal ağ içinde farklı bir alt ağa atandığında da adres değişir; ama ağ arabirimini farklı bir alt ağa atamak için, önce statik ayırma yöntemini dinamik olarak değiştirmeniz gerekir. Ağ arabirimini farklı bir alt ağa atadıktan sonra, ayırma yöntemini yine statik yapabilir ve yeni alt ağ adres aralığından bir IP adresi atayabilirsiniz.

## <a name="ip-address-versions"></a>IP adresi sürümleri

Adresleri atamasını yaparken aşağıdaki sürümleri belirtebilirsiniz:

### <a name="ipv4"></a>IPv4

Her ağ arabirimi olması gerekir [birincil](#primary) atanan bir IP yapılandırmasıyla [özel](#private) [IPv4](#ipv4) adresi. Bir veya daha fazla ekleyebilirsiniz [ikincil](#secondary) her sahip özel bir IPv4 ve (isteğe bağlı olarak) bir IPv4 IP yapılandırması [genel](#public) IP adresi.

### <a name="ipv6"></a>IPv6

Sıfır veya bir özel atayabilirsiniz [IPv6](#ipv6) adresine bir ağ arabirimi bir ikincil IP yapılandırması. Ağ arabirimi var olan tüm ikincil IP yapılandırmaları sahip olamaz. Portalı kullanarak bir IPv6 adresi, IP yapılandırması ekleyemezsiniz. Varolan bir ağ arabirimi IP yapılandırması ile özel bir IPv6 adresi eklemek için PowerShell veya CLI kullanın. Mevcut bir VM'ye ağ arabirimi iliştirilemez.

> [!NOTE]
> Portalı kullanarak bir IPv6 adresiyle bir ağ arabirimi oluşturabilirsiniz ancak mevcut bir ağ arabirimi portalı kullanarak yeni veya mevcut bir sanal makineye ekleyemezsiniz. Özel bir IPv6 adresi ile bir ağ arabirimi oluşturmak için PowerShell veya Azure CLI'yı kullanın ve ardından ağ arabirimi bir sanal makine oluştururken ekleyin. Varolan bir sanal makineye atanmış özel bir IPv6 adresi ile bir ağ arabirimine eklenemiyor. Herhangi bir aracı (portal, CLI veya PowerShell) kullanarak bir sanal makineye bağlı her ağ arabirimi için bir IP yapılandırmasına özel bir IPv6 adresi ekleyemezsiniz.

Bir birincil veya ikincil IP yapılandırması için genel IPv6 adresi atanamıyor.

## <a name="skus"></a>SKU'lar

Genel bir IP adresi, temel veya standart SKU ile oluşturulur. SKU farklılıklar hakkında daha fazla bilgi için bkz. [yönetmek genel IP adresleri](virtual-network-public-ip-address.md).

> [!NOTE]
> Standart bir SKU genel IP adresini bir sanal makinenin ağ arabirimine atadığınızda amaçlanan trafiğe bir [ağ güvenlik grubuyla](security-overview.md#network-security-groups) açıkça izin vermeniz gerekir. Bir ağ güvenlik grubu oluşturup ilişkilendirene ve istenen trafiğe açıkça izin verene kadar kaynakla erişim kurma girişimleri başarısız olur.

## <a name="next-steps"></a>Sonraki adımlar
Farklı IP yapılandırması içeren bir sanal makine oluşturmak için bu makaleleri okuyun:

|Görev|Tool|
|---|---|
|Birden çok ağ arabirimi ile VM oluşturma|[CLI](../virtual-machines/linux/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json), [PowerShell](../virtual-machines/windows/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json)|
|Birden çok IPv4 adresi ile tek bir NIC VM oluşturma|[CLI](virtual-network-multiple-ip-addresses-cli.md), [PowerShell](virtual-network-multiple-ip-addresses-powershell.md)|
|Özel bir IPv6 adresi (arkasında, Azure Load Balancer) ile tek bir NIC VM oluşturma|[CLI](../load-balancer/load-balancer-ipv6-internet-cli.md?toc=%2fazure%2fvirtual-network%2ftoc.json), [PowerShell](../load-balancer/load-balancer-ipv6-internet-ps.md?toc=%2fazure%2fvirtual-network%2ftoc.json), [Azure Resource Manager şablonu](../load-balancer/load-balancer-ipv6-internet-template.md?toc=%2fazure%2fvirtual-network%2ftoc.json)|
