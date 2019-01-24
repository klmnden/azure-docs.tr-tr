---
title: Azure'da bir Hizmet Dokusu kümesini silme | Microsoft Docs
description: Bu öğreticide Azure'da barındırılan bir Service Fabric kümesini ve kaynaklarını silmeyi öğreneceksiniz. Kümeyi içeren kaynak grubunu ya da seçerek kaynakları silebilirsiniz.
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: tutorial
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 09/26/2018
ms.author: ryanwi
ms.custom: mvc
ms.openlocfilehash: 5174bb696b456a66cfc0666ac5c475437e441d82
ms.sourcegitcommit: 8115c7fa126ce9bf3e16415f275680f4486192c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2019
ms.locfileid: "54848861"
---
# <a name="tutorial-remove-a-service-fabric-cluster-running-in-azure"></a>Öğretici: Azure'da çalışan bir Service Fabric kümesini Kaldır

Bu öğretici bir serinin dördüncü bölümüdür ve Azure'da çalışan bir Service Fabric kümesini silme işlemi göstermektedir. Bir Service Fabric kümesini tamamen silmek için küme tarafından kullanılan tüm kaynakları da silmeniz gerekir. İki seçeneğiniz vardır: kümenin bulunduğu kaynak grubunu silmek (küme kaynağını ve kaynak grubundaki diğer tüm kaynakları siler) veya özel olarak küme kaynağını ve (kaynak grubundaki diğer kaynakları silmeden) kümenin ilişkili kaynaklarını silmek.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Bir kaynak grubunu ve tüm kaynaklarını silmek
> * Kaynak grubundan seçerek kaynak silmek

Bu öğretici dizisinde şunların nasıl yapıldığını öğrenirsiniz:
> [!div class="checklist"]
> * Şablon kullanarak Azure'da güvenli bir [Windows kümesi](service-fabric-tutorial-create-vnet-and-windows-cluster.md) veya [Linux kümesi](service-fabric-tutorial-create-vnet-and-linux-cluster.md) oluşturma
> * [Bir kümenin ölçeğini daraltma veya genişletme](service-fabric-tutorial-scale-cluster.md)
> * [Bir kümenin çalışma zamanını yükseltme](service-fabric-tutorial-upgrade-cluster.md)
> * Küme silme

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiye başlamadan önce:

* Azure aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun
* [Azure Powershell modülü sürüm 4.1 veya üzerini](https://docs.microsoft.com/powershell/azure/azurerm/install-azurerm-ps) ya da [Azure CLI](/cli/azure/install-azure-cli)'yı yükleyin.
* Azure'da güvenli bir [Windows kümesi](service-fabric-tutorial-create-vnet-and-windows-cluster.md) veya [Linux kümesi](service-fabric-tutorial-create-vnet-and-linux-cluster.md) oluşturma

## <a name="delete-the-resource-group-containing-the-service-fabric-cluster"></a>Service Fabric kümesini içeren kaynak grubunu silme
Kümeyi ve kullandığı tüm kaynakları silmenin en basit yolu, kaynak grubunun silinmesidir.

Azure’da oturum açın ve kümeyi kaldırmak istediğiniz abonelik kimliğini seçin.  Abonelik kimliğinizi, [Azure portalında](http://portal.azure.com) oturum açarak öğrenebilirsiniz. [Remove-AzureRMResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) cmdlet’ini veya [az group delete](/cli/azure/group?view=azure-cli-latest) komutunu kullanarak kaynak grubunu ve tüm küme kaynaklarını silin.

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
* Küme düğümlerindeki [Windows işletim sistemine düzeltme eki uygulama](service-fabric-patch-orchestration-application.md) veya [Linux işletim sistemine düzeltme eki uygulama](service-fabric-patch-orchestration-application-linux.md) hakkında bilgi edinin.
* [Windows kümesi](service-fabric-diagnostics-event-aggregation-wad.md) veya [Linux kümesi](service-fabric-diagnostics-event-aggregation-lad.md) olaylarını toplayıp birleştirme ve küme olaylarını izlemek üzere [Log Analytics kurma](service-fabric-diagnostics-oms-setup.md) hakkında bilgi edinin.
