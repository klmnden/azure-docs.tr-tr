---
title: Azure'da bir Hizmet Dokusu kümesini silme | Microsoft Docs
description: Bu öğreticide Azure'da barındırılan bir Service Fabric kümesini ve kaynaklarını silmeyi öğreneceksiniz. Kümeyi içeren kaynak grubunu ya da seçerek kaynakları silebilirsiniz.
services: service-fabric
documentationcenter: .net
author: aljo-microsoft
manager: timlt
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
ms.openlocfilehash: cfb54a2d5cd71b2327f9422c63818bc51a9762a3
ms.sourcegitcommit: f24fdd1ab23927c73595c960d8a26a74e1d12f5d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2019
ms.locfileid: "58499820"
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

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiye başlamadan önce:

* Azure aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun
* [Azure Powershell modülü sürüm 4.1 veya üzerini](https://docs.microsoft.com/powershell/azure/azurerm/install-azurerm-ps) ya da [Azure CLI](/cli/azure/install-azure-cli)'yı yükleyin.
* Güvenli oluşturma [Windows Küme](service-fabric-tutorial-create-vnet-and-windows-cluster.md) azure'da

## <a name="delete-the-resource-group-containing-the-service-fabric-cluster"></a>Service Fabric kümesini içeren kaynak grubunu silme
Kümeyi ve kullandığı tüm kaynakları silmenin en basit yolu, kaynak grubunun silinmesidir.

Azure'da oturum açın ve kümeyi kaldırmak istediğiniz abonelik Kimliğini seçin.  Abonelik kimliğinizi, [Azure portalında](https://portal.azure.com) oturum açarak öğrenebilirsiniz. [Remove-AzureRMResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) cmdlet’ini veya [az group delete](/cli/azure/group?view=azure-cli-latest) komutunu kullanarak kaynak grubunu ve tüm küme kaynaklarını silin.

```powershell
Connect-AzureRmAccount
Set-AzureRmContext -SubscriptionId <guid>
$groupname = "sfclustertutorialgroup"
Remove-AzureRmResourceGroup -Name $groupname -Force
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
Connect-AzureRmAccount
Set-AzureRmContext -SubscriptionId <guid>
$groupname = "sfclustertutorialgroup"
Get-AzureRmResource -ResourceGroupName $groupname | ft
```

```azurecli
az login
az account set --subscription <guid>
ResourceGroupName="sfclustertutorialgroup"
az resource list --resource-group $ResourceGroupName
```

Silmek istediğiniz kaynaklardan her biri için aşağıdaki betiği çalıştırın:

```powershell
Remove-AzureRmResource -ResourceName "<name of the Resource>" -ResourceType "<Resource Type>" -ResourceGroupName $groupname -Force
```

```azurecli
az resource delete --name "<name of the Resource>" --resource-type "<Resource Type>" --resource-group $ResourceGroupName
```

Küme kaynağını silmek için aşağıdaki betiği çalıştırın:

```powershell
Remove-AzureRmResource -ResourceName "<name of the Resource>" -ResourceType "Microsoft.ServiceFabric/clusters" -ResourceGroupName $groupname -Force
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
