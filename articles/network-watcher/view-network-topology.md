---
title: Azure sanal ağ topolojisini görüntüleme | Microsoft Docs
description: Kaynakları bir sanal ağ ve kaynakları arasındaki ilişkiler görüntülemeyi öğrenin.
services: network-watcher
documentationcenter: na
author: jimdial
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/09/2018
ms.author: jdial
ms.openlocfilehash: 501659a93306342c7a212d135b4fdd89be096451
ms.sourcegitcommit: 9999fe6e2400cf734f79e2edd6f96a8adf118d92
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/22/2019
ms.locfileid: "54428180"
---
# <a name="view-the-topology-of-an-azure-virtual-network"></a>Bir Azure sanal ağ topolojisini görüntüleme

Bu makalede, Microsoft Azure sanal ağı ve kaynakları arasındaki ilişkiler kaynakların nasıl öğrenin. Örneğin, bir sanal ağ alt ağları içerir. Alt ağlar, Azure sanal makineler (VM) gibi kaynakları içerir. VM'ler, bir veya daha fazla ağ arabirimine sahip. Her alt ağ bir ağ güvenlik grubu ve ilişkili bir yol tablosu olabilir. Azure Ağ İzleyicisi topolojisi yeteneğini tüm kaynakları bir sanal ağ kaynaklara bir sanal ağ ve kaynakları arasındaki ilişkiler ilişkili kaynakları görüntülemek sağlar.

Kullanabileceğiniz [Azure portalında](#azure-portal), [Azure CLI](#azure-cli), veya [PowerShell](#powershell) bir topolojiyi görüntülemek için.

## <a name = "azure-portal"></a>Azure portalı - topolojisini görüntüleme

1. Oturum [Azure portalında](https://portal.azure.com) gerekli olan bir hesapla [izinleri](required-rbac-permissions.md).
2. Köşe seçin Portalı'nın sol üst köşesindeki, **tüm hizmetleri**.
3. İçinde **tüm hizmetleri** filtre kutusu, girin *Ağ İzleyicisi*. **Ağ İzleyicisi**, sonuçlarda görüntülendiğinde onu seçin.
4. Seçin **topolojisi**. Bir topoloji oluşturma topolojisini oluşturmak istediğiniz sanal ağın içinde bulunduğu aynı bölgede bir Ağ İzleyicisi gerektirir. Bir topoloji için oluşturmak istediğiniz sanal ağ bölgesinde etkin bir Ağ İzleyicisi yoksa, Ağ İzleyicisi otomatik olarak sizin için tüm bölgelerde oluşturulur. Ağ izleyicileri adlı bir kaynak grubunda oluşturulan **NetworkWatcherRG**.
5. Bir abonelik topolojisini, görüntülemek istediğiniz bir sanal ağın kaynak grubu seçin ve ardından sanal ağ'ı seçin. Aşağıdaki resimde, bir topoloji adlı bir sanal ağ için gösterilen *MyVnet*, adlı kaynak grubunda *MyResourceGroup*:

    ![Topolojiyi görüntüle](./media/view-network-topology/view-topology.png)

    Önceki resimde görebileceğiniz gibi sanal ağ üç alt ağ içerir. Bir alt ağ içinde dağıtılan bir sanal makine var. VM olarak bağlanmış bir ağ arabirimi ve ilişkili bir genel IP adresi vardır. Diğer iki alt avantajlarla ilişkili bir yol tablosu var. Her bir yol tablosu iki yolları içerir. Bir alt ağ ile ilişkili ağ güvenlik grubu vardır. Topoloji bilgilerini olan kaynaklar için yalnızca gösterilmektedir:
    
    - Aynı kaynak grubunda ve bölgede içinde *myVnet* sanal ağ. Örneğin, bir kaynak grubunda dışındaki mevcut bir ağ güvenlik grubu *MyResourceGroup*, bir alt ağda ilişkili ağ güvenlik grubu olsa bile, gösterilmiyor *MyVnet* sanal ağ .
    - İçinde ya da kaynaklara, içine ilişkili *myVnet* sanal ağ. Örneğin, bir alt ağ veya ağ arabirimi ile ilişkili olmayan bir ağ güvenlik grubu *myVnet* sanal ağ göremiyorsanız, ağ güvenlik grubu olmasa dahi *MyResourceGroup* kaynak grubu.

  Resimde gösterilen dağıttıktan sonra oluşturduğunuz sanal ağı için topolojidir **yönlendirme trafiği ağ sanal Gereci betik örneği-**, kullanarak dağıtabileceğiniz [Azure CLI](../virtual-network/scripts/virtual-network-cli-sample-route-traffic-through-nva.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json), veya [PowerShell](../virtual-network/scripts/virtual-network-powershell-sample-route-traffic-through-nva.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json).

6. Seçin **indirme topolojisi** svg biçiminde düzenlenebilir bir dosya olarak kullanmak üzere görüntüyü indirmek için.

Aşağıdaki diyagramda gösterilen bir alt kümesi sanal ağda ağ bileşenleri kaynaklardır. Örneğin, bir ağ güvenlik grubu gösterilmese içindeki güvenlik kuralları şemada gösterilmez. Diyagramda Ayrıştırılan değil de, satırları iki ilişkisi birini temsil eder: *Kapsama* veya *ilişkili*. Sanal ağ ve kaynakları arasındaki ilişki türü kaynakları tam listesini görmek için topoloji ile oluşturmak [PowerShell](#powershell) veya [Azure CLI](#azure-cli).

## <a name = "azure-cli"></a>Topolojiyi - Azure CLI görüntülemek

İzleyen adımları komutları çalıştırabilirsiniz:
- Azure Cloud shell'de seçerek, **deneyin** üst sağında herhangi bir komutu. Azure Cloud Shell'i yüklenmiştir ve Kabuk, hesabınızla birlikte kullanılacak şekilde yapılandırılmış yaygın Azure Araçları ücretsiz bir etkileşimli kabuktur.
- CLI'yı bilgisayarınızdan çalıştırarak. CLI'yı bilgisayarınızdan çalıştırırsanız, bu makaledeki adımlarda, Azure CLI Sürüm 2.0.31 gerektirir veya üzeri. Yüklü sürümü bulmak için `az --version` komutunu çalıştırın. Yükleme veya yükseltme yapmanız gerekirse bkz. [Azure CLI’yı yükleme](/cli/azure/install-azure-cli). Azure CLI'yi yerel olarak çalıştırıyorsanız, aynı zamanda çalıştırmak ihtiyacınız `az login` Azure ile bir bağlantı oluşturmak için.

Kullandığınız hesabın gerekli olmalıdır [izinleri](required-rbac-permissions.md).

1. Bir topoloji için oluşturmak istediğiniz sanal ağ ile aynı bölgede Ağ İzleyicisi zaten varsa, 3. adımına geçin. Ağ İzleyicisi ile içerecek bir kaynak grubu oluşturma [az grubu oluşturma](/cli/azure/group#az_group_create). Aşağıdaki örnekte kaynak grubu oluşturulmaktadır *eastus* bölgesi:

    ```azurecli-interactive
    az group create --name NetworkWatcherRG --location eastus
    ```

2. Ağ İzleyicisi ile oluşturma [az Ağ İzleyicisi'ni yapılandırma](/cli/azure/network/watcher#az-network-watcher-configure). Aşağıdaki örnek, bir Ağ İzleyicisi oluşturur *eastus* bölgesi:

    ```azurecli-interactive
    az network watcher configure \
      --resource-group NetworkWatcherRG \
      --location eastus \
      --enabled true
    ```

3. İle topolojiyi görüntülemek [az network watcher show-topology](/cli/azure/network/watcher#az-network-watcher-show-topology). Aşağıdaki örnekte adlı bir kaynak grubu için topoloji görünümleri *MyResourceGroup*:

    ```azurecli-interactive
    az network watcher show-topology --resource-group MyResourceGroup
    ```

    Aynı kaynak grubu içindeki kaynaklar için yalnızca topoloji bilgilerini döndürülen *MyResourceGroup* kaynak grubunu ve aynı bölgede Ağ İzleyicisi. Örneğin, bir kaynak grubunda dışındaki mevcut bir ağ güvenlik grubu *MyResourceGroup*, bir alt ağda ilişkili ağ güvenlik grubu olsa bile, gösterilmiyor *MyVnet* sanal ağ .

  Daha fazla bilgi edinin [ilişkileri](#relationhips) ve [özellikleri](#properties) döndürülen çıktı. İçin bir topolojiyi görüntülemek için mevcut bir sanal ağınız yoksa, kullanarak bir tane oluşturabilirsiniz [ağ sanal Gereci trafiği yönlendirme](../virtual-network/scripts/virtual-network-cli-sample-route-traffic-through-nva.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json) betik örneği. İçinde düzenlenebilir bir dosya indirin ve topoloji diyagramı görüntülemek için kullanın [portalı](#azure-portal).

## <a name = "powershell"></a>Topolojisini görüntüleme - PowerShell

İzleyen adımları komutları çalıştırabilirsiniz:
- Azure Cloud shell'de seçerek, **deneyin** üst sağında herhangi bir komutu. Azure Cloud Shell'i yüklenmiştir ve Kabuk, hesabınızla birlikte kullanılacak şekilde yapılandırılmış yaygın Azure Araçları ücretsiz bir etkileşimli kabuktur.
- PowerShell kullanarak bilgisayarınızdan çalıştırarak. PowerShell bilgisayarınızdan çalıştırırsanız, bu makaledeki adımlarda sürümü 5.7.0 gerekir veya üzeri AzureRm modülü. Yüklü sürümü bulmak için `Get-Module -ListAvailable AzureRM` komutunu çalıştırın. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/azurerm/install-azurerm-ps). PowerShell'i yerel olarak çalıştırıyorsanız Azure bağlantısı oluşturmak için `Login-AzureRmAccount` komutunu da çalıştırmanız gerekir.

Kullandığınız hesabın gerekli olmalıdır [izinleri](required-rbac-permissions.md).

1. Bir topoloji için oluşturmak istediğiniz sanal ağ ile aynı bölgede Ağ İzleyicisi zaten varsa, 3. adımına geçin. Ağ İzleyicisi ile içerecek bir kaynak grubu oluşturma [New-AzureRmResourceGroup](/powershell/module/AzureRM.Resources/New-AzureRmResourceGroup). Aşağıdaki örnekte kaynak grubu oluşturulmaktadır *eastus* bölgesi:

    ```azurepowershell-interactive
    New-AzureRmResourceGroup -Name NetworkWatcherRG -Location EastUS
    ```

2. Ağ İzleyicisi ile oluşturma [yeni AzureRmNetworkWatcher](/powershell/module/azurerm.network/new-azurermnetworkwatcher). Aşağıdaki örnekte eastus bölgede Ağ İzleyicisi oluşturur:

    ```azurepowershell-interactive
    New-AzureRmNetworkWatcher `
      -Name NetworkWatcher_eastus `
      -ResourceGroupName NetworkWatcherRG
    ```

3. Ağ İzleyicisi örneğiyle almak [Get-AzureRmNetworkWatcher](/powershell/module/azurerm.network/get-azurermnetworkwatcher). Aşağıdaki örnek, Doğu ABD bölgesinde bir Ağ İzleyicisi alır:

    ```azurepowershell-interactive
    $nw = Get-AzurermResource `
      | Where {$_.ResourceType -eq "Microsoft.Network/networkWatchers" -and $_.Location -eq "EastUS" }
    $networkWatcher = Get-AzureRmNetworkWatcher `
      -Name $nw.Name `
      -ResourceGroupName $nw.ResourceGroupName
    ```

4. Bir topoloji ile alma [Get-AzureRmNetworkWatcherTopology](/powershell/module/azurerm.network/get-azurermnetworkwatchertopology). Aşağıdaki örnekte adlı kaynak grubunda bir sanal ağ için bir topolojiyi alır *MyResourceGroup*:

    ```azurepowershell-interactive
    Get-AzureRmNetworkWatcherTopology `
      -NetworkWatcher $networkWatcher `
      -TargetResourceGroupName MyResourceGroup
    ```

   Aynı kaynak grubu içindeki kaynaklar için yalnızca topoloji bilgilerini döndürülen *MyResourceGroup* kaynak grubunu ve aynı bölgede Ağ İzleyicisi. Örneğin, bir kaynak grubunda dışındaki mevcut bir ağ güvenlik grubu *MyResourceGroup*, bir alt ağda ilişkili ağ güvenlik grubu olsa bile, gösterilmiyor *MyVnet* sanal ağ .

  Daha fazla bilgi edinin [ilişkileri](#relationhips) ve [özellikleri](#properties) döndürülen çıktı. İçin bir topolojiyi görüntülemek için mevcut bir sanal ağınız yoksa, kullanarak bir tane oluşturabilirsiniz [ağ sanal Gereci trafiği yönlendirme](../virtual-network/scripts/virtual-network-powershell-sample-route-traffic-through-nva.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json) betik örneği. İçinde düzenlenebilir bir dosya indirin ve topoloji diyagramı görüntülemek için kullanın [portalı](#azure-portal).

## <a name="relationships"></a>İlişkiler

Bir topolojide döndürülen tüm kaynakları başka bir kaynağa ilişkisinin aşağıdaki türlerinden biri vardır:

| İlişki türü | Örnek                                                                                                |
| ---               | ---                                                                                                    |
| Kapsama       | Bir sanal ağ, bir alt ağı içeriyor. Bir alt ağ bir ağ arabirimi içerir.                            |
| İlişkili        | Bir ağ arabiriminin VM ile ilişkilidir. Genel bir IP adresi, bir ağ arabirimi ile ilişkilidir. |

## <a name="properties"></a>Özellikler

Bir topolojide döndürülen tüm kaynakları, aşağıdaki özelliklere sahiptir:

- **Ad**: Kaynağın adı
- **Kimliği**: Kaynak URI'si.
- **Konum**: Kaynak var. Azure bölgesi.
- **İlişkilendirmeleri**: Başvurulan nesne ilişkilerini listesi. Her bir ilişkilendirme aşağıdaki özelliklere sahiptir:
    - **AssociationType**: Alt nesneye ve üst arasındaki ilişkiyi gösterir. Geçerli değerler *içerir* veya *ilişkilendirilmiş*.
    - **Ad**: Başvurulan kaynağın adı.
    - **ResourceId**:-ilişkilendirmeyi başvurulan kaynak URI.

## <a name="next-steps"></a>Sonraki adımlar

- Bilgi edinmek için nasıl [bir ağ trafik filtresi sorununu ya da bir VM'den tanılama](diagnose-vm-network-traffic-filtering-problem.md) kullanarak Ağ İzleyicisi'nin IP akışını doğrulayın özelliği
- Bilgi nasıl [bir VM'den bir ağ trafiği yönlendirme sorunu tanılama](diagnose-vm-network-routing-problem.md) Ağ İzleyicisi'nin sonraki atlama özelliğini kullanma
