---
title: Oluşturma, değiştirme veya bir Azure yol tablosu silme | Microsoft Docs
description: Oluşturma, değiştirme veya bir yol tablosu silme öğrenin.
services: virtual-network
documentationcenter: na
author: jimdial
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-network
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/09/2018
ms.author: jdial
ms.openlocfilehash: d6a4701c0318edf8292c777615196a2170a68750
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/19/2018
---
# <a name="create-change-or-delete-a-route-table"></a>Oluşturma, değiştirme veya bir yol tablosu silme

Azure otomatik olarak Azure alt ağlar, sanal ağlar arasında trafiği yönlendirir ve şirket içi ağlar. Azure'nın varsayılan yönlendirme değiştirmek istiyorsanız, bir yol tablosu oluşturarak bunu. Azure yönlendirme ile bilmiyorsanız okuma öneririz [yönlendirmeye genel bakış](virtual-networks-udr-overview.md) tamamlayarak [bir yol tablosu ile ağ trafiği yönlendirmek](tutorial-create-route-table-portal.md) eğitmen, bu makaledeki görevleri tamamlamadan önce.

## <a name="before-you-begin"></a>Başlamadan önce

Bu makalenin herhangi bir bölümdeki adımları gerçekleştirmeden önce aşağıdaki görevleri tamamlayın:

- Zaten bir Azure hesabınız yoksa, kaydolun bir [ücretsiz deneme sürümü hesabı](https://azure.microsoft.com/free).
- Portalı kullanarak, açık https://portal.azure.comve Azure hesabınızda oturum.
- Bu makalede görevleri tamamlamak için PowerShell komutlarını kullanarak, ya da komutları çalıştırmak [Azure bulut Kabuk](https://shell.azure.com/powershell), veya bilgisayarınızdan PowerShell çalıştırarak. Azure Cloud Shell, bu makaledeki adımları çalıştırmak için kullanabileceğiniz ücretsiz bir etkileşimli kabuktur. Yaygın Azure araçları, kabuğa önceden yüklenmiştir ve kabuk, hesabınızla birlikte kullanılacak şekilde yapılandırılmıştır. Bu öğreticide Azure PowerShell modülü sürümü 5.2.0 gerektirir veya sonraki bir sürümü. Yüklü sürümü bulmak için `Get-Module -ListAvailable AzureRM` komutunu çalıştırın. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-azurerm-ps). PowerShell'i yerel olarak çalıştırıyorsanız Azure bağlantısı oluşturmak için `Connect-AzureRmAccount` komutunu da çalıştırmanız gerekir.
- Bu makalede görevleri tamamlamak için Azure komut satırı arabirimi (CLI) komutlarını kullanarak, ya da komutları çalıştırmak [Azure bulut Kabuk](https://shell.azure.com/bash), veya bilgisayarınızdan CLI çalıştırarak. Bu öğretici Azure CLI Sürüm 2.0.26 gerektirir veya sonraki bir sürümü. Yüklü sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI 2.0 yükleme](/cli/azure/install-azure-cli). Azure CLI yerel olarak çalıştırıyorsanız, ayrıca çalıştırmanız gereken `az login` Azure ile bir bağlantı oluşturmak için.

## <a name="create-a-route-table"></a>Yönlendirme tablosu oluşturma

Kaç tane yönlendirme tabloları Azure konumu ve abonelik oluşturmak için bir sınır yoktur. Ayrıntılar için [Azure limitleri](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits) makalesini inceleyin.

1. Portalın sol üst köşede seçin **+ kaynak oluşturma**.
2. Seçin **ağ**seçeneğini belirleyip **yol tablosu**.
3. Girin bir **adı** için yol tablosu seçin, **abonelik**, yeni bir **kaynak grubu**, veya var olan bir kaynak grubunu seçin, bir **konumu** seçeneğini belirleyip **oluşturma**. **Devre dışı BGP rota yayma** seçenek, şirket içi yollar BGP aracılığıyla ağ arabirimleri için yol tablosu ilişkili herhangi bir alt ağ için yayılmasını engeller. Sanal ağ (VPN ya da ExpressRoute) bir Azure ağ geçidine bağlı değilse seçeneği bırakın *devre dışı*.

**Komutları**

- Azure CLI: [az ağ rota tablosu oluştur](/cli/azure/network/route-table/route#az_network_route_table_create)
- PowerShell: [AzureRmRouteTable yeni](/powershell/module/azurerm.network/new-azurermroutetable)

## <a name="view-route-tables"></a>Görünüm yönlendirme tabloları

Portal üstündeki arama kutusuna girin *yol tablosu* arama kutusuna. Zaman **yol tablosu** arama sonuçlarında görünür. Aboneliğinizde var yönlendirme tabloları listelenir.

**Komutları**

- Azure CLI: [az ağ yol tablosu listesi](/cli/azure/network/route-table/route#az_network_route_table_list)
- PowerShell: [Get-AzureRmRouteTable](/powershell/module/azurerm.network/get-azurermroutetable)

## <a name="view-details-of-a-route-table"></a>Bir yol tablosu ayrıntılarını görüntüleme

1. Portal üstündeki arama kutusuna girin *yol tablosu* arama kutusuna. Zaman **yol tablosu** arama sonuçlarında görünür.
2. Yol tablosu ayrıntılarını görüntülemek istediğiniz listeyi seçin. Altında **ayarları** görüntüleyebileceğiniz **yollar** rota tablosunda ve **alt ağlar** için yol tablosu ilişkilidir.
3. Ortak Azure ayarları hakkında daha fazla bilgi için aşağıdaki bilgileri bakın:
    *   [Etkinlik Günlüğü](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json#activity-logs)
    *   [Erişim denetimi (IAM)](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json#access-control)
    *   [Etiketler](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json#tags)
    *   [Kilitler](../azure-resource-manager/resource-group-lock-resources.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
    *   [Otomasyon komut dosyası](../azure-resource-manager/resource-manager-export-template.md?toc=%2fazure%2fvirtual-network%2ftoc.json#export-the-template-from-resource-group)

**Komutları**

- Azure CLI: [az ağ yönlendirme tablosunu göster](/cli/azure/network/route-table/route#az_network_route_table_show)
- PowerShell: [Get-AzureRmRouteTable](/powershell/module/azurerm.network/get-azurermroutetable)

## <a name="change-a-route-table"></a>Bir yol tablosu değiştirme

1. Portal üstündeki arama kutusuna girin *yol tablosu* arama kutusuna. Zaman **yol tablosu** arama sonuçlarında görünür.
2. Değiştirmek istediğiniz yol tablosu seçin. En yaygın değişiklikler [ekleme](#create-a-route) veya [kaldırma](#delete-a-route) yollar ve [ilişkilendirme](#associate-a-route-table-to-a-subnet) yol tablosu için veya [kaldırdıktan](#dissociate-a-route-table-from-a-subnet) yönlendirme tabloları alt ağı.

**Komutları**

- Azure CLI: [az ağ yol tablosu güncelleştirme](/cli/azure/network/route-table/route#az_network_route_table_update)
- PowerShell: [kümesi AzureRmRouteTable](/powershell/module/azurerm.network/set-azurermroutetable)

## <a name="associate-a-route-table-to-a-subnet"></a>Yönlendirme tablosunu bir alt ağ ile ilişkilendirme

Bir alt ağ için ilişkili sıfır veya bir yol tablosu olabilir. Bir yol tablosu sıfır veya birden çok alt ağlara ilişkili olabilir. Yönlendirme tabloları sanal ağlara ilişkili olmadığından bir yol tablosu ile ilişkili yol tablosu istediğiniz her alt ağa ilişkilendirmeniz gerekir. Alt ağdan çıkan tüm trafik yönlendirme tabloları içinde oluşturduğunuz yollar göre yönlendirilir [varsayılan yollar](virtual-networks-udr-overview.md#default), sanal ağ bağlıysa, bir Azure sanal ağı ağ geçidi (yollar yayıldığı bir şirket içi ağ üzerinden ExpressRoute, veya bir VPN ağ geçidi ile BGP kullanıyorsanız, VPN). Yalnızca bir yol tablosu rota tablosu olarak abonelik ve aynı Azure konumunda bulunan sanal ağlardaki alt ağlara ilişkilendirebilirsiniz.

1. Portal üstündeki arama kutusuna girin *sanal ağlar* arama kutusuna. Zaman **sanal ağlar** arama sonuçlarında görünür.
2. Sanal ağ için bir yol tablosu ilişkilendirmek istediğiniz alt ağ içeren listeyi seçin.
3. Seçin **alt ağlar** altında **ayarları**.
4. Yol tablosu ilişkilendirmek istediğiniz alt ağ seçin.
5. Seçin **yol tablosu**, istediğiniz alt ağa ilişkilendirin ve ardından seçmek için yol tablosu seçin **kaydetmek**.

**Komutları**

- Azure CLI: [az ağ sanal ağ alt ağı güncelleştirme](/cli/azure/network/vnet/subnet?view=azure-cli-latest#az_network_vnet_subnet_update)
- PowerShell: [kümesi AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/set-azurermvirtualnetworksubnetconfig)

## <a name="dissociate-a-route-table-from-a-subnet"></a>Bir yol tablosu bir alt ağdan ilişkilendirmesini Kaldır

Bir yol tablosu bir alt ağdan ilişkilendirmesini kaldırmanız, Azure temel trafiğini yönlendiren kendi [varsayılan yollar](virtual-networks-udr-overview.md#default).

1. Portal üstündeki arama kutusuna girin *sanal ağlar* arama kutusuna. Zaman **sanal ağlar** arama sonuçlarında görünür.
2. Bir rota tablosundan ilişkilendirmesini kaldırmak istediğiniz alt ağ içeren sanal ağı seçin.
3. Seçin **alt ağlar** altında **ayarları**.
4. Rota tablosundan ilişkilendirmesini kaldırmak istediğiniz alt ağ seçin.
5. Seçin **yol tablosu**seçin **hiçbiri**seçeneğini belirleyip **kaydetmek**.

**Komutları**

- Azure CLI: [az ağ sanal ağ alt ağı güncelleştirme](/cli/azure/network/vnet/subnet?view=azure-cli-latest#az_network_vnet_subnet_update)
- PowerShell: [kümesi AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/set-azurermvirtualnetworksubnetconfig) 

## <a name="delete-a-route-table"></a>Bir yol tablosu Sil

Hiçbir alt ağ için bir yol tablosu ilişkiliyse, silinemez. [İlişkilendirmesini](#dissociate-a-route-table-from-a-subnet) silmeye çalışmadan önce tüm alt rota tablosundan.

1. Portal üstündeki arama kutusuna girin *yol tablosu* arama kutusuna. Zaman **yol tablosu** arama sonuçlarında görünür.
2. Seçin **...**  silmek için rota tablosunu sağ tarafında.
3. Seçin **silmek**ve ardından **Evet**.

**Komutları**

- Azure CLI: [az ağ rota-tablo silme](/cli/azure/network/route-table/route#az_network_route_table_delete)
- PowerShell: [AzureRmRouteTable Sil](/powershell/module/azurerm.network/delete-azurermroutetable) 

## <a name="create-a-route"></a>Yönlendirme oluşturma

Yol tablosu başına kaç tane rota Azure konumu ve abonelik oluşturabilmeniz için bir sınır yoktur. Ayrıntılar için [Azure limitleri](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits) makalesini inceleyin.

1. Portal üstündeki arama kutusuna girin *yol tablosu* arama kutusuna. Zaman **yol tablosu** arama sonuçlarında görünür.
2. Yol tablosu bir rotaya eklemek istediğiniz listeyi seçin.
3. Seçin **yollar**altında **ayarları**.
4. **+ Ekle** öğesini seçin.
5. Benzersiz bir girin **adı** rota tablosu içindeki rota için.
6. Girin **adres ön eki**, trafiğini yönlendirmek istediğiniz CIDR gösteriminde. Önek içinde başka bir önek olabilir ancak önek rota tablosu içindeki birden fazla yol çoğaltılamaz. Örneğin, bir rota öneki olarak 10.0.0.0/16 tanımlanmışsa 10.0.0.0/24 adres ön ekine sahip başka bir yol tanımlayabilirsiniz. Azure üzerinde en uzun ön ek eşleşmesi göre trafiği için bir rota seçer. Azure yollar nasıl seçtiği hakkında daha fazla bilgi için bkz: [yönlendirmeye genel bakış](virtual-networks-udr-overview.md#how-azure-selects-a-route).
7. Seçin bir **sonraki atlama türü**. Tüm sonraki atlama türlerini ayrıntılı bir açıklaması için bkz: [yönlendirmeye genel bakış](virtual-networks-udr-overview.md).
8. İçin bir IP adresi girin **sonraki atlama adresi**. Seçtiyseniz, yalnızca bir adres girebilirsiniz *sanal Gereci* için **sonraki atlama türü**.
9. **Tamam**’ı seçin. 

**Komutları**

- Azure CLI: [az ağ yol tablosu yol oluşturma](/cli/azure/network/route-table/route?view=azure-cli-latest#az_network_route_table_route_create)
- PowerShell: [AzureRmRouteConfig yeni](/powershell/module/azurerm.network/new-azurermrouteconfig)

## <a name="view-routes"></a>Görünüm yolları

Bir rota tablosu sıfır veya birden çok yolları içerir. Yollar görüntülerken listelenen bilgileri hakkında daha fazla bilgi için bkz: [yönlendirmeye genel bakış](virtual-networks-udr-overview.md).

1. Portal üstündeki arama kutusuna girin *yol tablosu* arama kutusuna. Zaman **yol tablosu** arama sonuçlarında görünür.
2. Yol tablosu yollar için görüntülemek istediğiniz listeyi seçin.
3. Seçin **yollar** altında **ayarları**.

**Komutları**

- Azure CLI: [az ağ yol tablosu rota listesi](/cli/azure/network/route-table/route?view=azure-cli-latest#az_network_route_table_route_list)
- PowerShell: [Get-AzureRmRouteConfig](/powershell/module/azurerm.network/get-azurermrouteconfig)

## <a name="view-details-of-a-route"></a>Bir rota ayrıntılarını görüntüleme

1. Portal üstündeki arama kutusuna girin *yol tablosu* arama kutusuna. Zaman **yol tablosu** arama sonuçlarında görünür.
2. İçin bir rota ayrıntılarını görüntülemek istediğiniz yol tablosu seçin.
3. Seçin **yollar**.
4. Ayrıntılarını görüntülemek istediğiniz yolu seçin.

**Komutları**

- Azure CLI: [az ağ yol tablosu rota Göster](/cli/azure/network/route-table/route?view=azure-cli-latest#az_network_route_table_route_show)
- PowerShell: [Get-AzureRmRouteConfig](/powershell/module/azurerm.network/get-azurermrouteconfig)

## <a name="change-a-route"></a>Bir rota değiştirme

1. Portal üstündeki arama kutusuna girin *yol tablosu* arama kutusuna. Zaman **yol tablosu** arama sonuçlarında görünür.
2. Bir rota için değiştirmek istediğiniz yol tablosu seçin.
3. Seçin **yollar**.
4. Değiştirmek istediğiniz yolu seçin.
5. Yeni ayarlarına varolan ayarlarını değiştirin ve ardından **kaydetmek**.

**Komutları**

- Azure CLI: [az ağ yol tablosu rota güncelleştirme](/cli/azure/network/route-table/route?view=azure-cli-latest#az_network_route_table_route_update)
- PowerShell: [Set-AzureRmRouteConfig](/powershell/module/azurerm.network/set-azurermrouteconfig)

## <a name="delete-a-route"></a>Bir rota Sil

1. Portal üstündeki arama kutusuna girin *yol tablosu* arama kutusuna. Zaman **yol tablosu** arama sonuçlarında görünür.
2. Bir rota için silmek istediğiniz yol tablosu seçin.
3. Seçin **yollar**.
4. Yollar listesinden seçin **...**  silmek istediğiniz yolun sağ taraftaki.
5. Seçin **silmek**seçeneğini belirleyip **Evet**.

**Komutları**

- Azure CLI: [az ağ yol tablosu rota Sil](/cli/azure/network/route-table/route?view=azure-cli-latest#az_network_route_table_route_delete)
- PowerShell: [Remove-AzureRmRouteConfig](/powershell/module/azurerm.network/remove-azurermrouteconfig)

## <a name="view-effective-routes"></a>Görünüm etkili yolları

Bir sanal makineye bağlı her ağ arabirimi için etkili rotaları oluşturmuş olduğunuz bir birleşimi rota tablolar Azure'nın varsayılan yolların ve tüm yollar ağlardan şirket içi BGP aracılığıyla bir Azure sanal ağı ağ geçidi üzerinden yayılan. Bir ağ arabirimi için etkili rotaları anlama yönlendirme sorunlarını gidermede yardımcı olur. Çalışan bir sanal makineye bağlı herhangi bir ağ arabirimi için etkili rotaları görüntüleyebilirsiniz.

1. Portal üstündeki arama kutusuna, etkili yollar için görüntülemek istediğiniz bir sanal makine adını girin. Bir sanal makinenin adını bilmiyorsanız, girin *sanal makineleri* arama kutusuna. Zaman **sanal makineleri** arama sonuçlarında görünür ve listeden bir sanal makineyi seçin.
2. Seçin **ağ** altında **ayarları**.
3. Bir ağ arabirimi adı seçin.
4. Seçin **etkili yolları** altında **destek + sorun giderme**.
5. Doğru yol trafiği yönlendirmek istediğiniz için olup olmadığını belirlemek için etkili yolların listesini gözden geçirin. Bu listede gördüğünüz sonraki atlama türlerini hakkında daha fazla bilgi [yönlendirmeye genel bakış](virtual-networks-udr-overview.md).

**Komutları**

- Azure CLI: [az ağ NIC Göster-etkin-yol-tablosu](/cli/azure/network/nic?view=azure-cli-latest#az_network_nic_show_effective_route_table)
- PowerShell: [Get-AzureRmEffectiveRouteTable](/powershell/module/azurerm.network/remove-azurermrouteconfig) 

## <a name="validate-routing-between-two-endpoints"></a>İki uç noktaları arasında yönlendirme doğrula

Bir sanal makine ve başka bir Azure kaynak, bir şirket içi kaynağa ya da Internet üzerindeki bir kaynak IP adresi arasındaki sonraki atlama türü belirleyebilirsiniz. Belirleme Azure'nın yönlendirme yönlendirme sorunlarını gidermede yardımcı olur. Bu görevi tamamlamak için var olan bir Ağ İzleyicisi olması gerekir. Varolan bir Ağ İzleyicisi yoksa, içindeki adımları tamamlayarak oluşturmak [bir Ağ İzleyicisi örneği oluşturmayı](../network-watcher/network-watcher-create.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

1. Portal üstündeki arama kutusuna girin *Ağ İzleyicisi* arama kutusuna. Zaman **Ağ İzleyicisi** arama sonuçlarında görünür.
2. Seçin **sonraki atlama** altında **Ağ Tanılama Araçları**.
3. Seçin, **abonelik** ve **kaynak grubu** gelen yönlendirme doğrulamak istediğiniz kaynak sanal makinesini.
4. Seçin **sanal makine**, **ağ arabirimi** sanal makineye bağlı ve **kaynak IP adresi** doğrulamak istediğiniz ağ arabirimine atanmış gelen yönlendirme.
5. Girin **hedef IP adresi** yönlendirme doğrulamak istediğiniz.
6. Seçin **sonraki atlama**.
7. Kısa bir bekleme, sonraki atlama türü ve trafiği yönlendirilmiş yol Kimliğini belirten bilgi döndürülür. Döndürülen gördüğünüz sonraki atlama türleri hakkında daha fazla bilgi [yönlendirmeye genel bakış](virtual-networks-udr-overview.md).

**Komutları**

- Azure CLI: [az Ağ İzleyicisi Göster sonraki atlama](/cli/azure/network/watcher?view=azure-cli-latest#az_network_watcher_show_next_hop)
- PowerShell: [Get-AzureRmNetworkWatcherNextHop](/powershell/module/azurerm.network/get-azurermnetworkwatchernexthop) 
 
## <a name="permissions"></a>İzinler

Yönlendirme tabloları ve yollar görevleri gerçekleştirmek için hesabınızı atanmalıdır [ağ Katılımcısı](../role-based-access-control/built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) rolü veya bir [özel](../role-based-access-control/custom-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json) uygun izinleri atanmış rolü aşağıdaki tabloda listelenen:

|İşlem                                                       |   İşlem adı                               |
|--------------------------------------------------------------  |   -------------------------------------------  |
|Microsoft.Network/routeTables/read                              |   Yönlendirme tablosunu Al                              |
|Microsoft.Network/routeTables/write                             |   Yol tablosu güncelle                 |
|Microsoft.Network/routeTables/delete                            |   Yol tablosu Sil                           |
|Microsoft.Network/routeTables/join/action                       |   Yol tablosu birleştirme                             |
|Microsoft.Network/routeTables/routes/read                       |   Rota Al                                    |
|Microsoft.Network/routeTables/routes/write                      |   Yol oluştur veya güncelleştir                       |
|Microsoft.Network/routeTables/routes/delete                     |   Rota Sil                                 |
|Microsoft.Network/networkInterfaces/effectiveRouteTable/action  |   Ağ arabirimi etkin yönlendirme tablosunu Al  | 
|Microsoft.Network/networkWatchers/nextHop/action                |   Bir sanal makineden sonraki atlama alır                  |

*Birleştirme yol tablosu* işlemi, bir alt ağ için bir yol tablosu ilişkilendirmek için gereklidir.
