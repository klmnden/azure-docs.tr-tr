---
title: Oluşturma, değiştirme veya bir Azure rota tablosunu sil | Microsoft Docs
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
ms.openlocfilehash: 04db7655f3f4b63edffcb731d0e92db25f1847b9
ms.sourcegitcommit: 4ea0cea46d8b607acd7d128e1fd4a23454aa43ee
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/15/2018
ms.locfileid: "42056445"
---
# <a name="create-change-or-delete-a-route-table"></a>Oluşturma, değiştirme veya bir rota tablosunu sil

Azure otomatik olarak Azure alt ağlar, sanal ağlar arasındaki trafiği yönlendirir ve şirket içi ağlara. Azure'nın varsayılan yönlendirmesini değiştirmek istiyorsanız, bir yol tablosu oluşturarak bunu. Sanal ağlarda yönlendirmeyle yeniyseniz, içinde hakkında daha fazla bilgi edinebilirsiniz [yönlendirmeye genel bakış](virtual-networks-udr-overview.md) veya tamamlayarak bir [öğretici](tutorial-create-route-table-portal.md).

## <a name="before-you-begin"></a>Başlamadan önce

Bu makalenin bir bölümündeki adımları tamamlamadan önce aşağıdaki görevleri tamamlayın:

- Azure hesabınız yoksa, kaydolmaya bir [ücretsiz deneme hesabınızı](https://azure.microsoft.com/free).
- Portalı kullanarak, açık https://portal.azure.comve Azure hesabınızda oturum.
- Bu makaledeki görevleri tamamlamak için PowerShell komutlarını kullanarak, ya da komutları çalıştırmak [Azure Cloud Shell](https://shell.azure.com/powershell), veya PowerShell bilgisayarınızdan çalıştırarak. Azure Cloud Shell, bu makaledeki adımları çalıştırmak için kullanabileceğiniz ücretsiz bir etkileşimli kabuktur. Yaygın Azure araçları, kabuğa önceden yüklenmiştir ve kabuk, hesabınızla birlikte kullanılacak şekilde yapılandırılmıştır. Bu öğretici, Azure PowerShell modülü 5.7.0 veya sonraki bir sürümü gerektirir. Yüklü sürümü bulmak için `Get-Module -ListAvailable AzureRM` komutunu çalıştırın. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-azurerm-ps). PowerShell'i yerel olarak çalıştırıyorsanız Azure bağlantısı oluşturmak için `Connect-AzureRmAccount` komutunu da çalıştırmanız gerekir.
- Bu makaledeki görevleri tamamlamak için Azure komut satırı arabirimi (CLI) komutlarını kullanarak, ya da komutları çalıştırmak [Azure Cloud Shell](https://shell.azure.com/bash), veya bilgisayarınızdan CLI çalıştırarak. Bu öğretici, Azure CLI Sürüm 2.0.31 gerektirir veya üzeri. Yüklü sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI 2.0 yükleme](/cli/azure/install-azure-cli). Azure CLI'yi yerel olarak çalıştırıyorsanız, aynı zamanda çalıştırmak ihtiyacınız `az login` Azure ile bir bağlantı oluşturmak için.

Oturum açın ya da Azure ile bağlandığınız hesabı atanmalıdır [ağ Katılımcısı](../role-based-access-control/built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) rolü veya bir [özel rol](../role-based-access-control/custom-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json) içinde listelenen uygun eylemleri atanan [izinleri ](#permissions).

## <a name="create-a-route-table"></a>Yönlendirme tablosu oluşturma

Kaç yönlendirme tablolarını Azure konumu ve abonelik oluşturmak için bir sınır yoktur. Ayrıntılar için [Azure limitleri](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits) makalesini inceleyin.

1. Portalın sol üst köşedeki seçin **+ kaynak Oluştur**.
2. Seçin **ağ**, ardından **yol tablosu**.
3. Girin bir **adı** için rota tablosu seçin, **abonelik**, yeni bir **kaynak grubu**, veya mevcut bir kaynak grubunu seçin, bir **konumu** , ardından **Oluştur**. Bir VPN ağ geçidi üzerinden şirket içi ağınıza bağlı sanal ağ içindeki alt ağ yönlendirme tablosunu ilişkilendirme planlıyorsanız ve devre dışı bırakırsanız **BGP yol yaymayı**, şirket içi yollarınızı için yayılmaz alt ağ arabirimleri.

**Komutları**

- Azure CLI: [az ağ yönlendirme tablosu oluşturma](/cli/azure/network/route-table/route#az_network_route_table_create)
- PowerShell: [AzureRmRouteTable yeni](/powershell/module/azurerm.network/new-azurermroutetable)

## <a name="view-route-tables"></a>Görünüm rota tabloları

Portalın üst kısmındaki arama kutusuna girin *rota tabloları* arama kutusuna. Zaman **rota tabloları** arama sonuçlarında görünmesini, onu seçin. Aboneliğinizde mevcut rota tabloları listelenir.

**Komutları**

- Azure CLI: [az ağ route-table listesi](/cli/azure/network/route-table/route#az_network_route_table_list)
- PowerShell: [Get-AzureRmRouteTable](/powershell/module/azurerm.network/get-azurermroutetable)

## <a name="view-details-of-a-route-table"></a>Bir yol tablosu ayrıntılarını görüntüle

1. Portalın üst kısmındaki arama kutusuna girin *rota tabloları* arama kutusuna. Zaman **rota tabloları** arama sonuçlarında görünmesini, onu seçin.
2. Rota tablosunu ayrıntılarını görüntülemek istediğiniz listeyi seçin. Altında **ayarları**, görüntüleyebileceğiniz **yollar** yol tablosundaki ve **alt ağlar** için rota tablosunu ilişkilidir.
3. Azure ortak ayarları hakkında daha fazla bilgi için aşağıdaki bilgileri bakın:
    *   [Etkinlik Günlüğü](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json#activity-logs)
    *   [Erişim denetimi (IAM)](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json#access-control)
    *   [Etiketler](../azure-resource-manager/resource-group-using-tags.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
    *   [Kilitler](../azure-resource-manager/resource-group-lock-resources.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
    *   [Otomasyon betiği](../azure-resource-manager/resource-manager-export-template.md?toc=%2fazure%2fvirtual-network%2ftoc.json#export-the-template-from-resource-group)

**Komutları**

- Azure CLI: [az ağ route-table show](/cli/azure/network/route-table/route#az_network_route_table_show)
- PowerShell: [Get-AzureRmRouteTable](/powershell/module/azurerm.network/get-azurermroutetable)

## <a name="change-a-route-table"></a>Bir yol tablosu değiştirme

1. Portalın üst kısmındaki arama kutusuna girin *rota tabloları* arama kutusuna. Zaman **rota tabloları** arama sonuçlarında görünmesini, onu seçin.
2. Değiştirmek istediğiniz rota tablosunu seçin. En yaygın değişiklikler [ekleme](#create-a-route) veya [kaldırma](#delete-a-route) yolları ve [ilişkilendirme](#associate-a-route-table-to-a-subnet) rota tabloları için veya [kaldırdıktan](#dissociate-a-route-table-from-a-subnet) rota tabloları alt ağ.

**Komutları**

- Azure CLI: [az ağ route-table update](/cli/azure/network/route-table/route#az_network_route_table_update)
- PowerShell: [AzureRmRouteTable Ayarla](/powershell/module/azurerm.network/set-azurermroutetable)

## <a name="associate-a-route-table-to-a-subnet"></a>Yönlendirme tablosunu bir alt ağ ile ilişkilendirme

Bir alt ağ ile ilişkili sıfır veya bir yol tablosu olabilir. Sıfır veya birden çok alt ağa bir yol tablosu ilişkilendirilebilir. Rota tabloları sanal ağlara ilişkili olmadığından bir yol tablosu ile ilişkili yol tablosuna istediğiniz her bir alt ağa ilişkilendirmeniz gerekir. Alt ağdan çıkan tüm trafiği rota tabloları içinde oluşturulmuş rotalar göre yönlendirilir [varsayılan yolları](virtual-networks-udr-overview.md#default), ve yollar yayılan bir şirket içi ağdan sanal ağa bağlı olması durumunda bir Azure sanal ağ geçidi (için ExpressRoute veya VPN ağ geçidi ile BGP kullanıyorsanız, VPN). Yalnızca bir yol tablosu aynı Azure konumunda ve aboneliğinde yol tablosu olarak mevcut olan sanal ağlardaki alt ağlara ilişkilendirebilirsiniz.

1. Portalın üst kısmındaki arama kutusuna girin *sanal ağlar* arama kutusuna. Zaman **sanal ağlar** arama sonuçlarında görünmesini, onu seçin.
2. Sanal ağ için bir yol tablosu ilişkilendirmek istediğiniz alt ağ içeren listeyi seçin.
3. Seçin **alt ağlar** altında **ayarları**.
4. Rota tablosunu ilişkilendirmek istediğiniz alt ağ seçin.
5. Seçin **yol tablosu**, istediğiniz alt ağ ile ilişkilendirin ve ardından seçmek için rota tablosu seçin **Kaydet**.

Sanal ağınız bir Azure VPN ağ geçidine bağlıysa, rota tablosunu 0.0.0.0/0 hedefine sahip bir rota içeren [ağ geçidi alt ağına](../vpn-gateway/vpn-gateway-about-vpn-gateway-settings.md?toc=%2fazure%2fvirtual-network%2ftoc.json#gwsub) ilişkilendirmeyin. Bunun yapılması, ağ geçidinin düzgün çalışmasını engelleyebilir. Bir yolda 0.0.0.0/0 kullanma hakkında daha fazla bilgi için bkz. [sanal ağ trafiği yönlendirme](virtual-networks-udr-overview.md#default-route).

**Komutları**

- Azure CLI: [az ağ sanal ağ alt ağı güncelleştirme](/cli/azure/network/vnet/subnet?view=azure-cli-latest#az_network_vnet_subnet_update)
- PowerShell: [Set-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/set-azurermvirtualnetworksubnetconfig)

## <a name="dissociate-a-route-table-from-a-subnet"></a>Bir yol tablosu bir alt ağdan ilişkilendirmesini Kaldır

Bir alt ağdan bir yol tablosu ile ilişkisini kaldırma, Azure temel trafiği yönlendirir, [varsayılan yolları](virtual-networks-udr-overview.md#default).

1. Portalın üst kısmındaki arama kutusuna girin *sanal ağlar* arama kutusuna. Zaman **sanal ağlar** arama sonuçlarında görünmesini, onu seçin.
2. Bir rota tablosundan ilişkilendirmesini kaldırmak istediğiniz alt ağ içeren sanal ağı seçin.
3. Seçin **alt ağlar** altında **ayarları**.
4. Rota tablosundan ilişkilendirmesini kaldırmak istediğiniz alt ağ seçin.
5. Seçin **yol tablosu**seçin **hiçbiri**, ardından **Kaydet**.

**Komutları**

- Azure CLI: [az ağ sanal ağ alt ağı güncelleştirme](/cli/azure/network/vnet/subnet?view=azure-cli-latest#az_network_vnet_subnet_update)
- PowerShell: [Set-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/set-azurermvirtualnetworksubnetconfig) 

## <a name="delete-a-route-table"></a>Rota tablosunu sil

Hiçbir alt ağ için bir yol tablosu ilişkiliyse, bu komut dosyası silinemiyor. [İlişkisini](#dissociate-a-route-table-from-a-subnet) silmeye çalışmadan önce tüm alt ağların yol tablosundan.

1. Portalın üst kısmındaki arama kutusuna girin *rota tabloları* arama kutusuna. Zaman **rota tabloları** arama sonuçlarında görünmesini, onu seçin.
2. Seçin **...**  silmek istediğiniz yol tablosuna sağ tarafında.
3. Seçin **Sil**ve ardından **Evet**.

**Komutları**

- Azure CLI: [az ağ route-table delete](/cli/azure/network/route-table/route#az_network_route_table_delete)
- PowerShell: [AzureRmRouteTable Sil](/powershell/module/azurerm.network/delete-azurermroutetable) 

## <a name="create-a-route"></a>Yönlendirme oluşturma

Yol tablosu başına kaç rota Azure konumu ve abonelik oluşturabilmeniz için bir sınır yoktur. Ayrıntılar için [Azure limitleri](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits) makalesini inceleyin.

1. Portalın üst kısmındaki arama kutusuna girin *rota tabloları* arama kutusuna. Zaman **rota tabloları** arama sonuçlarında görünmesini, onu seçin.
2. Yol tablosu bir yol eklemek istediğiniz listeyi seçin.
3. Seçin **yollar**altında **ayarları**.
4. **+ Ekle** öğesini seçin.
5. Benzersiz bir girin **adı** için rota tablosu içindeki rota.
6. Girin **adres ön eki**, trafiği yönlendirmek için istediğiniz CIDR gösteriminde. Önek içinde başka bir önek olabilir ancak rota tablosu içindeki birden fazla yol ön eki çoğaltılamaz. Örneğin, tek bir yol ön eki olarak 10.0.0.0/16 tanımlı değilse, başka bir yol 10.0.0.0/24 adres ön eki ile tanımlayabilirsiniz. Azure en uzun ön ek eşleşmesini alarak trafiği için bir yol seçer. Azure yolların nasıl seçtiği hakkında daha fazla bilgi için bkz: [yönlendirmeye genel bakış](virtual-networks-udr-overview.md#how-azure-selects-a-route).
7. Seçin bir **sonraki atlama türü**. Tüm sonraki atlama türleri ayrıntılı bir açıklaması için bkz. [yönlendirmeye genel bakış](virtual-networks-udr-overview.md).
8. İçin bir IP adresi girin **sonraki atlama adresi**. Seçtiyseniz, yalnızca bir adresi girebilirsiniz *sanal gereç* için **sonraki atlama türü**.
9. **Tamam**’ı seçin.

**Komutları**

- Azure CLI: [az ağ route-table route oluşturma](/cli/azure/network/route-table/route?view=azure-cli-latest#az_network_route_table_route_create)
- PowerShell: [AzureRmRouteConfig yeni](/powershell/module/azurerm.network/new-azurermrouteconfig)

## <a name="view-routes"></a>Görünüm yolları

Bir yol tablosu, sıfır veya birden çok yol içerir. Yollar görüntülerken listelenen bilgiler hakkında daha fazla bilgi için bkz. [yönlendirmeye genel bakış](virtual-networks-udr-overview.md).

1. Portalın üst kısmındaki arama kutusuna girin *rota tabloları* arama kutusuna. Zaman **rota tabloları** arama sonuçlarında görünmesini, onu seçin.
2. Yol tablosu yolları için görüntülemek istediğiniz listeyi seçin.
3. Seçin **yollar** altında **ayarları**.

**Komutları**

- Azure CLI: [az ağ route-table route listesi](/cli/azure/network/route-table/route?view=azure-cli-latest#az_network_route_table_route_list)
- PowerShell: [Get-AzureRmRouteConfig](/powershell/module/azurerm.network/get-azurermrouteconfig)

## <a name="view-details-of-a-route"></a>Bir yolun ayrıntılarını görüntüleme

1. Portalın üst kısmındaki arama kutusuna girin *rota tabloları* arama kutusuna. Zaman **rota tabloları** arama sonuçlarında görünmesini, onu seçin.
2. İçin bir yol ayrıntılarını görüntülemek istediğiniz rota tablosunu seçin.
3. Seçin **yollar**.
4. Ayrıntılarını görüntülemek istediğiniz yolu seçin.

**Komutları**

- Azure CLI: [az ağ route-table route show](/cli/azure/network/route-table/route?view=azure-cli-latest#az_network_route_table_route_show)
- PowerShell: [Get-AzureRmRouteConfig](/powershell/module/azurerm.network/get-azurermrouteconfig)

## <a name="change-a-route"></a>Bir rota değiştirme

1. Portalın üst kısmındaki arama kutusuna girin *rota tabloları* arama kutusuna. Zaman **rota tabloları** arama sonuçlarında görünmesini, onu seçin.
2. Bir rota için değiştirmek istediğiniz rota tablosunu seçin.
3. Seçin **yollar**.
4. Değiştirmek istediğiniz yolu seçin.
5. Yeni ayarlarına varolan ayarları değiştirin ve ardından **Kaydet**.

**Komutları**

- Azure CLI: [az ağ route-table route update](/cli/azure/network/route-table/route?view=azure-cli-latest#az_network_route_table_route_update)
- PowerShell: [Set-AzureRmRouteConfig](/powershell/module/azurerm.network/set-azurermrouteconfig)

## <a name="delete-a-route"></a>Bir rota Sil

1. Portalın üst kısmındaki arama kutusuna girin *rota tabloları* arama kutusuna. Zaman **rota tabloları** arama sonuçlarında görünmesini, onu seçin.
2. Bir rota için silmek istediğiniz rota tablosunu seçin.
3. Seçin **yollar**.
4. Rota listesinden **...**  chcete odstranit yolun sağ taraftaki.
5. Seçin **Sil**, ardından **Evet**.

**Komutları**

- Azure CLI: [az ağ route-table route delete](/cli/azure/network/route-table/route?view=azure-cli-latest#az_network_route_table_route_delete)
- PowerShell: [Remove-AzureRmRouteConfig](/powershell/module/azurerm.network/remove-azurermrouteconfig)

## <a name="view-effective-routes"></a>Geçerli yollar bölümünü inceleyin

Bir sanal makineye bağlı her ağ arabirimi için geçerli rotalar oluşturduğunuz rota tabloları birleşimi, Azure'nın varsayılan yollarını ve ağlardan şirket içi BGP üzerinden bir Azure sanal ağ geçidinden yayılan her yol ' dir. Bir ağ arabirimi için geçerli rotalar anlama yönlendirme sorunları giderirken yararlıdır. Çalışan bir sanal makineye bağlı her ağ arabirimi için geçerli rotalar görüntüleyebilirsiniz.

1. Portalın üst kısmındaki arama kutusuna, geçerli rotalar için görüntülemek istediğiniz bir sanal makine adını girin. Bir sanal makinenin adını bilmiyorsanız, girin *sanal makineler* arama kutusuna. Zaman **sanal makineler** arama sonuçlarında görünmesini, onu seçin ve listeden bir sanal makine seçin.
2. Seçin **ağ** altında **ayarları**.
3. Bir ağ arabirimi adını seçin.
4. Seçin **geçerli rotalar** altında **destek + sorun giderme**.
5. Doğru yol, trafiği yönlendirmek istediğiniz için olup olmadığını belirlemek için geçerli rotalar listesini gözden geçirin. Bu listede görmek sonraki atlama türleri hakkında daha fazla bilgi [yönlendirmeye genel bakış](virtual-networks-udr-overview.md).

**Komutları**

- Azure CLI: [az network nic show-etkin-yönlendirme-tablosunu](/cli/azure/network/nic?view=azure-cli-latest#az_network_nic_show_effective_route_table)
- PowerShell: [Get-AzureRmEffectiveRouteTable](/powershell/module/azurerm.network/get-azurermeffectiveroutetable) 

## <a name="validate-routing-between-two-endpoints"></a>İki uç nokta arasında yönlendirme doğrula

Sonraki atlama türü arasında bir sanal makine ve başka bir Azure kaynak, bir şirket içi kaynağa ya da Internet üzerindeki bir kaynağa IP adresini saptayabilirsiniz. Belirleme Azure'nın yönlendirme yönlendirme sorunları giderirken yararlıdır. Bu görevi tamamlamak için var olan bir Ağ İzleyicisi olması gerekir. Var olan bir Ağ İzleyicisi yoksa, oluşturmak adımları tamamlayarak [Ağ İzleyicisi örneği oluşturma](../network-watcher/network-watcher-create.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

1. Portalın üst kısmındaki arama kutusuna girin *Ağ İzleyicisi* arama kutusuna. **Ağ İzleyicisi**, arama sonuçlarında görüntülendiğinde seçin.
2. Seçin **sonraki atlama** altında **Ağ Tanılama Araçları**.
3. Seçin, **abonelik** ve **kaynak grubu** gelen yönlendirme doğrulamak istediğiniz kaynak sanal makinenin.
4. Seçin **sanal makine**, **ağ arabirimi** sanal makineye bağlı ve **kaynak IP adresi** doğrulamak istediğiniz ağ arabirimine atanan gelen yönlendirme.
5. Girin **hedef IP adresi** yönlendirme doğrulamak istediğiniz.
6. Seçin **sonraki atlama**.
7. Kısa bir beklemeden sonra başarılı olduğunu, sonraki atlama türü ve trafik yönlendirilmiş yol kimliği anlatan bilgi döndürülür. Döndürülen gördüğünüz sonraki atlama türleri hakkında daha fazla bilgi [yönlendirmeye genel bakış](virtual-networks-udr-overview.md).

**Komutları**

- Azure CLI: [az network watcher show-next-hop](/cli/azure/network/watcher?view=azure-cli-latest#az_network_watcher_show_next_hop)
- PowerShell: [Get-AzureRmNetworkWatcherNextHop](/powershell/module/azurerm.network/get-azurermnetworkwatchernexthop) 

## <a name="permissions"></a>İzinler

Rota tabloları ve yollar görevleri gerçekleştirmek için hesabınızı atanmalıdır [ağ Katılımcısı](../role-based-access-control/built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) rolü veya bir [özel](../role-based-access-control/custom-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json) uygun eylemleri atanan rolü aşağıdaki tabloda listelenen:

| Eylem                                                          |   Ad                                                  |
|--------------------------------------------------------------   |   -------------------------------------------           |
| Microsoft.Network/routeTables/read                              |   Bir yol tablosu okuyun                                    |
| Microsoft.Network/routeTables/write                             |   Bir yol tablosu güncelle                        |
| Microsoft.Network/routeTables/delete                            |   Rota tablosunu sil                                  |
| Microsoft.Network/routeTables/join/action                       |   Yönlendirme tablosunu bir alt ağ ile ilişkilendirme                   |
| Microsoft.Network/routeTables/routes/read                       |   Bir rota okuyun                                          |
| Microsoft.Network/routeTables/routes/write                      |   Bir rota güncelle                              |
| Microsoft.Network/routeTables/routes/delete                     |   Bir rota Sil                                        |
| Microsoft.Network/networkInterfaces/effectiveRouteTable/action  |   Bir ağ arabirimi için geçerli bir rota tablosunu alın |
| Microsoft.Network/networkWatchers/nextHop/action                |   Sonraki atlama bir VM'den alır                           |

## <a name="next-steps"></a>Sonraki adımlar

- Kullanarak bir yönlendirme tablosu oluşturma [PowerShell](powershell-samples.md) veya [Azure CLI](cli-samples.md) örnek komut dosyaları veya Azure kullanarak [Resource Manager şablonları](template-samples.md)
- Oluşturma ve uygulama [Azure İlkesi](policy-samples.md) sanal ağlar için
