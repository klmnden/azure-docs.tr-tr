---
title: Azure sanal ağ topolojisi görüntülemek | Microsoft Docs
description: Bir sanal ağ ve kaynakları arasındaki ilişkileri kaynakları görüntülemek öğrenin.
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
ms.openlocfilehash: 6ef165ddc481bf84c6189635e36b97eb9518261e
ms.sourcegitcommit: fc64acba9d9b9784e3662327414e5fe7bd3e972e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/12/2018
---
# <a name="view-the-topology-of-an-azure-virtual-network"></a>Bir Azure sanal ağ topolojisini görüntülemek

Bu makalede, Microsoft Azure sanal ağı ve kaynakları arasındaki ilişkileri kaynakları görüntülemek nasıl öğrenin. Örneğin, bir sanal ağ alt ağlar içeriyor. Alt ağlar kaynaklar, Azure sanal makineler (VM) gibi içerir. Sanal makineleri bir veya daha fazla ağ arabirimine sahip. Her alt ağ, bir ağ güvenlik grubu ve için ilişkili bir yol tablosu sahip olabilir. Azure Ağ İzleyicisi topoloji yeteneğini tüm kaynakları bir sanal ağ içinde bir sanal ağ ve kaynakları arasındaki ilişkileri kaynaklara ilişkili kaynakları görüntülemek etkinleştirir.

Kullanabileceğiniz [Azure portal](#azure-portal), [Azure CLI](#azure-cli), veya [PowerShell](#powershell) bir topolojisini görüntülemek için.

## <a name = "azure-portal"></a>Görünüm topolojisi - Azure portalı

1. İçine oturum [Azure portal](https://portal.azure.com) gerekli olan bir hesap ile [izinleri](required-rbac-permissions.md).
2. Üstte sol alt köşe portalı, select **tüm hizmetleri**.
3. İçinde **tüm hizmetleri** filtre kutusu, girin *Ağ İzleyicisi*. Zaman **Ağ İzleyicisi** göründüğü sonuçlarında seçin.
4. Seçin **topoloji**. Bir topoloji oluşturmak için topoloji oluşturmak istediğiniz sanal ağ var. aynı bölgede bir Ağ İzleyicisi gerektirir. Sanal ağ için bir topolojiyi oluşturmak istediğiniz bulunduğu bölgede etkin bir Ağ İzleyicisi yoksa, ağ izleyicileri otomatik olarak sizin için tüm bölgelerde oluşturulur. Ağ izleyicileri adlı bir kaynak grubunda oluşturulan **NetworkWatcherRG**.
5. Kaynak grubu, bir sanal ağ topolojisini, görüntülemek istediğiniz aboneliği seçin ve ardından sanal ağ seçin. Aşağıdaki resimde, bir topoloji adlı bir sanal ağ için gösterilen *MyVnet*, kaynak grubunda adlı *MyResourceGroup*:

    ![Topolojiyi görüntüle](./media/view-network-topology/view-topology.png)

    Önceki resimde gördüğünüz gibi sanal ağ üç alt ağ içerir. Bir alt ağda dağıtılmış bir VM sahiptir. VM ekli bir ağ arabirimi ve için ilişkili ortak bir IP adresi vardır. Diğer iki alt ağ için ilişkili bir yol tablosu var. Her yol tablosu iki yol içerir. Bir alt ağ için ilişkili bir ağ güvenlik grubu vardır. Kaynaklar için topoloji bilgilerini gösterilen yalnızca: - aynı kaynak grubunu ve bölge içinde *myVnet* sanal ağ. Örneğin, bir kaynak grubunda dışındaki mevcut bir ağ güvenlik grubu *MyResourceGroup*, ağ güvenlik grubu için bir alt ağda ilişkili olsa bile, gösterilmeyen *MyVnet* sanal ağ .
        -İçinde ya da, içindeki kaynaklara ilişkili *myVnet* sanal ağ. Örneğin, bir alt ağ veya ağ arabirimi ile ilişkili olmayan bir ağ güvenlik grubu *myVnet* sanal ağ değil gösterilen, ağ güvenlik grubu olsa bile *MyResourceGroup* kaynak grubu.

    Aşağıdaki resimde gösterilen dağıttıktan sonra oluşturulan sanal ağ için topolojidir **yönlendirme trafiği ağ sanal gereç komut dosyası örneği üzerinden**, hangi kullanarak dağıtabilirsiniz [Azure CLI](../virtual-network/scripts/virtual-network-cli-sample-route-traffic-through-nva.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json), veya [PowerShell](../virtual-network/scripts/virtual-network-powershell-sample-route-traffic-through-nva.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json).

6. Seçin **karşıdan topoloji** görüntü svg biçiminde düzenlenebilir bir dosya olarak indirmek için.

Diyagramda gösterildiği kaynaklar sanal ağda ağ bileşenlerinin bir alt kümesidir. Örneğin, bir ağ güvenlik grubu gösterilmese içindeki güvenlik kuralları şemada gösterilmez. Diyagramda Ayrıştırılan değil de, satırları iki ilişki birini temsil eden: *kapsama* veya *ilişkili*. Sanal ağ ve kaynakları arasındaki ilişki türünü kaynaklarında tam listesini görmek için topolojisiyle oluşturmak [PowerShell](#powershell) veya [Azure CLI](#azure-cli).

## <a name = "azure-cli"></a>Topolojisi - Azure CLI görüntüleyin

Aşağıdaki adımlarda komutları çalıştırabilirsiniz:
- Seçerek Azure bulut Kabuğu'nda **deneyin** üst sağına herhangi bir komutu. Azure bulut Kabuk önceden ve hesabınızla kullanmak üzere yapılandırılmış ortak Azure Araçları olduğundan boş bir etkileşimli kabuk ' dir.
- CLI bilgisayarınızdan çalıştırarak. CLI bilgisayarınızdan çalıştırırsanız, bu makaledeki adımları Azure CLI Sürüm 2.0.31 gerektiren veya sonraki bir sürümü. Yüklü sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI 2.0 yükleme](/cli/azure/install-azure-cli). Azure CLI yerel olarak çalıştırıyorsanız, ayrıca çalıştırmanız gereken `az login` Azure ile bir bağlantı oluşturmak için.

Kullandığınız hesabın gerekli olmalıdır [izinleri](required-rbac-permissions.md).

1. Ağ İzleyici için bir topolojiyi oluşturmak istediğiniz sanal ağ ile aynı bölgede zaten varsa, 3. adıma atlayın. Ağ İzleyicisi ile içerecek şekilde bir kaynak grubu oluşturmak [az grubu oluşturma](/cli/azure/group#az_group_create). Aşağıdaki örnek, kaynak grubunu oluşturur *eastus* bölge:

    ```azurecli-interactive
    az group create --name NetworkWatcherRG --location eastus
    ```

2. Ağ İzleyicisi ile oluşturma [az Ağ İzleyicisi'ni yapılandırma](/cli/azure/network/watcher#az-network-watcher-configure). Aşağıdaki örnek, bir Ağ İzleyicisi oluşturur *eastus* bölge:

    ```azurecli-interactive
    az network watcher configure \
      --resource-group NetworkWatcherRG \
      --location eastus \
      --enabled true
    ```

3. Topolojisiyle görüntülemek [az Ağ İzleyicisi Göster-topolojisi](/cli/azure/network/watcher#az-network-watcher-show-topology). Aşağıdaki örnekte bir kaynak grubu için topoloji görünümleri *MyResourceGroup*:

    ```azurecli-interactive
    az network watcher show-topology --resource-group MyResourceGroup
    ```

    Topoloji bilgilerini aynı kaynak grubunda kaynaklar için döndürülen yalnızca *MyResourceGroup* kaynak grubu ve Ağ İzleyicisi'ni aynı bölgede. Örneğin, bir kaynak grubunda dışındaki mevcut bir ağ güvenlik grubu *MyResourceGroup*, ağ güvenlik grubu için bir alt ağda ilişkili olsa bile, gösterilmeyen *MyVnet* sanal ağ .

  Daha fazla bilgi edinmek [ilişkileri](#relationhips) ve [özellikleri](#properties) döndürülen çıkışı. İçin bir topolojiyi görüntülemek için mevcut bir sanal ağa sahip değilseniz, kullanarak bir tane oluşturabilirsiniz [trafiği ağ sanal gereç aracılığıyla yönlendirmek](../virtual-network/scripts/virtual-network-cli-sample-route-traffic-through-nva.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json) komut dosyası örneği. Topoloji diyagramı görüntülemek ve düzenlenebilir bir dosyasını karşıdan yüklemek için kullanmak [portal](#azure-portal).

## <a name = "powershell"></a>Görünüm topolojisi - PowerShell

Aşağıdaki adımlarda komutları çalıştırabilirsiniz:
- Seçerek Azure bulut Kabuğu'nda **deneyin** üst sağına herhangi bir komutu. Azure bulut Kabuk önceden ve hesabınızla kullanmak üzere yapılandırılmış ortak Azure Araçları olduğundan boş bir etkileşimli kabuk ' dir.
- PowerShell bilgisayarınızdan çalıştırarak. PowerShell bilgisayarınızdan çalıştırırsanız, bu makaledeki adımları sürümünü 5.7.0 gerektiren veya üzeri AzureRm modülü. Yüklü sürümü bulmak için `Get-Module -ListAvailable AzureRM` komutunu çalıştırın. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-azurerm-ps). PowerShell'i yerel olarak çalıştırıyorsanız Azure bağlantısı oluşturmak için `Login-AzureRmAccount` komutunu da çalıştırmanız gerekir.

Kullandığınız hesabın gerekli olmalıdır [izinleri](required-rbac-permissions.md).

1. Ağ İzleyici için bir topolojiyi oluşturmak istediğiniz sanal ağ ile aynı bölgede zaten varsa, 3. adıma atlayın. Ağ İzleyicisi ile içerecek şekilde bir kaynak grubu oluşturmak [New-AzureRmResourceGroup](/powershell/module/AzureRM.Resources/New-AzureRmResourceGroup). Aşağıdaki örnek, kaynak grubunu oluşturur *eastus* bölge:

    ```azurepowershell-interactive
    New-AzureRmResourceGroup -Name NetworkWatcherRG -Location EastUS
    ```

2. Ağ İzleyicisi ile oluşturma [yeni AzureRmNetworkWatcher](/powershell/module/azurerm.network/new-azurermnetworkwatcher). Aşağıdaki örnek, bir Ağ İzleyicisi eastus bölgede oluşturur:

    ```azurepowershell-interactive
    New-AzureRmNetworkWatcher `
      -Name NetworkWatcher_eastus `
      -ResourceGroupName NetworkWatcherRG
    ```

3. Ağ İzleyicisi örneğiyle almak [Get-AzureRmNetworkWatcher](/powershell/module/azurerm.network/get-azurermnetworkwatcher). Aşağıdaki örnek, bir Ağ İzleyicisi Doğu ABD bölgesinde alır:

    ```azurepowershell-interactive
    $nw = Get-AzurermResource `
      | Where {$_.ResourceType -eq "Microsoft.Network/networkWatchers" -and $_.Location -eq "EastUS" }
    $networkWatcher = Get-AzureRmNetworkWatcher `
      -Name $nw.Name `
      -ResourceGroupName $nw.ResourceGroupName
    ```

4. Bir topolojisiyle almak [Get-AzureRmNetworkWatcherTopology](/powershell/module/azurerm.network/get-azurermnetworkwatchertopology). Aşağıdaki örnek adlı kaynak grubundaki sanal ağ için bir topolojiyi alır *MyResourceGroup*:

    ```azurepowershell-interactive
    Get-AzureRmNetworkWatcherTopology `
      -NetworkWatcher $networkWatcher `
      -TargetResourceGroupName MyResourceGroup
    ```

   Topoloji bilgilerini aynı kaynak grubunda kaynaklar için döndürülen yalnızca *MyResourceGroup* kaynak grubu ve Ağ İzleyicisi'ni aynı bölgede. Örneğin, bir kaynak grubunda dışındaki mevcut bir ağ güvenlik grubu *MyResourceGroup*, ağ güvenlik grubu için bir alt ağda ilişkili olsa bile, gösterilmeyen *MyVnet* sanal ağ .

  Daha fazla bilgi edinmek [ilişkileri](#relationhips) ve [özellikleri](#properties) döndürülen çıkışı. İçin bir topolojiyi görüntülemek için mevcut bir sanal ağa sahip değilseniz, kullanarak bir tane oluşturabilirsiniz [trafiği ağ sanal gereç aracılığıyla yönlendirmek](../virtual-network/scripts/virtual-network-powershell-sample-route-traffic-through-nva.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json) komut dosyası örneği. Topoloji diyagramı görüntülemek ve düzenlenebilir bir dosyasını karşıdan yüklemek için kullanmak [portal](#azure-portal).

## <a name="relationships"></a>İlişkiler

Tüm kaynakların bir topolojisinde döndürülen başka bir kaynağa ilişkinin aşağıdaki türlerden biri vardır:

| İlişki türü | Örnek                                                                                                |
| ---               | ---                                                                                                    |
| Kapsama       | Bir sanal ağ alt ağını içeriyor. Bir alt ağ bir ağ arabirimi içerir.                            |
| İlişkili        | Bir ağ arabirimine bir VM ile ilişkilidir. Bir ortak IP adresi, bir ağ arabirimine ilişkilidir. |

## <a name="properties"></a>Özellikler

Tüm kaynakların bir topolojisinde döndürülen aşağıdaki özelliklere sahiptir:

- **Ad**: kaynak adı
- **Kimliği**: kaynak URI'si.
- **Konum**: kaynak bulunmaktadır Azure bölgesi.
- **İlişkileri**: başvurulan nesne ilişkilerini listesi. Her bir ilişkilendirme aşağıdaki özelliklere sahiptir:
    - **AssociationType**: alt nesne ve üst arasındaki ilişkiyi başvuruyor. Geçerli değerler *içerir* veya *ilişkilendirilmiş*.
    - **Ad**: başvurulan kaynağın adı.
    - **ResourceId**:-association'ında başvurulan kaynak URI'si.

## <a name="next-steps"></a>Sonraki adımlar

- Bilgi nasıl [için veya bir VM ağ trafiği filtre sorunu tanılamak](diagnose-vm-network-traffic-filtering-problem.md) Ağ İzleyicisi'nin IP akış kullanarak yetenek doğrulayın
- Bilgi edinmek için nasıl [bir VM'den gelen ağ trafiğini yönlendirme sorunu tanılamak](diagnose-vm-network-routing-problem.md) Ağ İzleyicisi'nin sonraki atlama özelliğini kullanma