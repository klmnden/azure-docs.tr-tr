---
title: Azure'da bir Hizmet Dokusu kümesini silme | Microsoft Docs
description: Bu öğreticide Azure'da barındırılan bir Service Fabric kümesini ve kaynaklarını silmeyi öğreneceksiniz. Kümeyi içeren kaynak grubunu ya da seçerek kaynakları silebilirsiniz.
services: service-fabric
documentationcenter: .net
author: aljo-microsoft
manager: chackdan
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: tutorial
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 09/26/2018
ms.author: aljo
ms.custom: mvc
ms.openlocfilehash: 0e5137a8183f378ee5960846e281222c6ecaaa47
ms.sourcegitcommit: bf509e05e4b1dc5553b4483dfcc2221055fa80f2
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2019
ms.locfileid: "59995719"
---
# <a name="tutorial-remove-a-service-fabric-cluster-running-in-azure"></a>Öğretici: Azure'da çalışan bir Service Fabric kümesini Kaldır

Bu öğretici, beş oluşan bir serinin parçasıdır ve Azure'da çalışan Service Fabric küme silme işlemi gösterilmektedir. Bir Service Fabric kümesini tamamen silmek için küme tarafından kullanılan tüm kaynakları da silmeniz gerekir. İki seçeneğiniz vardır: kümenin bulunduğu kaynak grubunu silmek (küme kaynağını ve kaynak grubundaki diğer tüm kaynakları siler) veya özel olarak küme kaynağını ve (kaynak grubundaki diğer kaynakları silmeden) kümenin ilişkili kaynaklarını silmek.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Bir kaynak grubunu ve tüm kaynaklarını silmek
> * Kaynak grubundan seçerek kaynak silmek

Bu öğretici dizisinde şunların nasıl yapıldığını öğrenirsiniz:
> [!div class="checklist"]
> * Güvenli oluşturma [Windows Küme](service-fabric-tutorial-create-vnet-and-windows-cluster.md) bir şablonu kullanarak azure'da
> * [Bir kümesini izleme](service-fabric-tutorial-monitor-cluster.md)
> * [Bir kümenin ölçeğini daraltma veya genişletme](service-fabric-tutorial-scale-cluster.md)
> * [Bir kümenin çalışma zamanını yükseltme](service-fabric-tutorial-upgrade-cluster.md)
> * Küme silme


[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiye başlamadan önce:

* Azure aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun
* Yükleme [Azure Powershell](https://docs.microsoft.com/powershell/azure//install-Az-ps) veya [Azure CLI](/cli/azure/install-azure-cli).
* Güvenli oluşturma [Windows Küme](service-fabric-tutorial-create-vnet-and-windows-cluster.md) azure'da

## <a name="delete-the-resource-group-containing-the-service-fabric-cluster"></a>Service Fabric kümesini içeren kaynak grubunu silme
Kümeyi ve kullandığı tüm kaynakları silmenin en basit yolu, kaynak grubunun silinmesidir.

Azure'da oturum açın ve kümeyi kaldırmak istediğiniz abonelik Kimliğini seçin.  Abonelik kimliğinizi, [Azure portalında](https://portal.azure.com) oturum açarak öğrenebilirsiniz. Kaynak grubu ve kullanarak tüm küme kaynaklarını silin [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) cmdlet'ini veya [az grubu Sil](/cli/azure/group?view=azure-cli-latest) komutu.

```powershell
Connect-AzAccount
Set-AzContext -SubscriptionId <guid>
$groupname = "sfclustertutorialgroup"
Remove-AzResourceGroup -Name $groupname -Force
```

```azurecli
az login
az account set --subscription <guid>
ResourceGroupName="sfclustertutorialgroup"
az group delete --name $ResourceGroupName
```

## <a name="selectively-delete-the-cluster-resource-and-the-associated-resources"></a>Küme kaynağını ve ilişkili kaynakları seçerek silme
Kaynak grubunuzda sadece silmek istediğiniz Service Fabric kümesiyle ilgili kaynaklar varsa, kaynak grubunun tamamını silmek daha kolaydır. Kaynak grubunuzdaki kaynakları seçerek silmek ve kümeyle ilişkili olmayan kaynakları korumak istiyorsanız, bu durumda aşağıdaki adımları izleyin.

Kaynak grubundaki kaynakları listeleyin:

```powershell
Connect-AzAccount
Set-AzContext -SubscriptionId <guid>
$groupname = "sfclustertutorialgroup"
Get-AzResource -ResourceGroupName $groupname | ft
```

```azurecli
az login
az account set --subscription <guid>
ResourceGroupName="sfclustertutorialgroup"
az resource list --resource-group $ResourceGroupName
```

Silmek istediğiniz kaynaklardan her biri için aşağıdaki betiği çalıştırın:

```powershell
Remove-AzResource -ResourceName "<name of the Resource>" -ResourceType "<Resource Type>" -ResourceGroupName $groupname -Force
```

```azurecli
az resource delete --name "<name of the Resource>" --resource-type "<Resource Type>" --resource-group $ResourceGroupName
```

Küme kaynağını silmek için aşağıdaki betiği çalıştırın:

```powershell
Remove-AzResource -ResourceName "<name of the Resource>" -ResourceType "Microsoft.ServiceFabric/clusters" -ResourceGroupName $groupname -Force
```

```azurecli
az resource delete --name "<name of the Resource>" --resource-type "Microsoft.ServiceFabric/clusters" --resource-group $ResourceGroupName
```

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Bir kaynak grubunu ve tüm kaynaklarını silmek
> * Kaynak grubundan seçerek kaynak silmek

Bu öğreticiyi tamamladığınıza göre aşağıdakileri deneyebilirsiniz:
* [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md)'ı kullanarak Service Fabric kümesini inceleme ve yönetme hakkında bilgi edinin.
* Bilgi edinmek için nasıl [Windows işletim sistemi düzeltme eki](service-fabric-patch-orchestration-application.md) küme düğümlerinin.
* Toplama ve olaylarını toplayan öğrenin [Windows kümeleri](service-fabric-diagnostics-event-aggregation-wad.md) ve [Log Analytics Kurulum](service-fabric-diagnostics-oms-setup.md) küme olayları izlemek için.
