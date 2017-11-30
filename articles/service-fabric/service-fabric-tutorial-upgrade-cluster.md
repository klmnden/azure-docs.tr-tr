---
title: "Azure Service Fabric çalışma zamanı yükseltme | Microsoft Docs"
description: "Bir Azure barındırılan Service Fabric kümesi çalışma zamanı yükseltmek için PowerShell kullanmayı öğrenin."
services: service-fabric
documentationcenter: .net
author: Thraka
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: tutorial
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/28/2017
ms.author: adegeo
ms.openlocfilehash: 15acfbce3bde585ed2b39762b08733901133a3dd
ms.sourcegitcommit: 29bac59f1d62f38740b60274cb4912816ee775ea
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/29/2017
---
# <a name="upgrade-the-runtime-of-a-service-fabric-cluster"></a>Yükseltme Service Fabric kümesi çalışma zamanı

Bu öğretici dört bir serinin bir parçasıdır ve bir Azure Service Fabric kümesi üzerinde Service Fabric çalışma zamanını yükseltme gösterir. Bu öğretici bölümü Azure üzerinde çalışan Service Fabric kümeleri için yazılmış ve kendi kendini barındıran Service Fabric kümeleri için geçerli değil.

> [!WARNING]
> Öğreticinin bu bölümü, PowerShell gerektiriyor. Küme çalışma zamanı yükseltme desteği Azure CLI araçları tarafından henüz desteklenmiyor. Alternatif olarak, bir küme portalda yükseltilebilir. Daha fazla bilgi için bkz: [Azure Service Fabric kümesi yükseltme](service-fabric-cluster-upgrade.md).

Kümenizi en son Service Fabric çalışma zamanı zaten çalışıyorsa, bu adımı gerekmez. Ancak, bu makalede, Azure Service Fabric kümesi üzerinde desteklenen tüm çalışma zamanı yükleme için kullanılabilir.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Küme sürümü okuyun
> * Küme sürümü Ayarla

## <a name="prerequisites"></a>Ön koşullar
Bu öğreticiye başlamadan önce:
- Bir Azure aboneliğiniz yoksa, oluşturma bir [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)
- Yükleme [Azure Powershell modülü sürümü 4.1 veya üzerini](https://docs.microsoft.com/powershell/azure/install-azurerm-ps) veya [Azure CLI 2.0](/cli/azure/install-azure-cli).
- Güvenli oluşturma [Windows Küme](service-fabric-tutorial-create-vnet-and-windows-cluster.md) veya [Linux kümesi](service-fabric-tutorial-create-vnet-and-linux-cluster.md) Azure ile ilgili
- Windows Küme dağıtırsanız, bir Windows geliştirme ortamını ayarlama. Yükleme [Visual Studio 2017](http://www.visualstudio.com) ve **Azure geliştirme**, **ASP.NET ve web geliştirme**, ve **.NET Core platformlar arası geliştirme**iş yükleri.  Daha sonra ayarlanmış bir [.NET geliştirme ortamı](service-fabric-get-started.md).
- Linux kümesi dağıtırsanız, üzerinde bir Java geliştirme ortamını ayarlama [Linux](service-fabric-get-started-linux.md) veya [MacOS](service-fabric-get-started-mac.md).  Yükleme [doku CLI hizmet](service-fabric-cli.md). 

### <a name="sign-in-to-azure"></a>Azure'da oturum açma
Azure komutları çalıştırmadan önce Azure hesabınızda oturum açın, aboneliğinizi seçin.

```powershell
Login-AzureRmAccount
Get-AzureRmSubscription
Set-AzureRmContext -SubscriptionId <guid>
```

## <a name="get-the-runtime-version"></a>Çalışma zamanı sürümü Al

Azure'a bağlandıktan sonra seçili abonelik Service Fabric kümesi içeren, Küme çalışma zamanı sürümü elde edebilirsiniz.

```powershell
Get-AzureRmServiceFabricCluster -ResourceGroupName SFCLUSTERTUTORIALGROUP -Name aztestcluster `
    | Select-Object ClusterCodeVersion
```

Veya yalnızca aşağıdakilerle aboneliğinizdeki tüm kümelerin listesini alın:

```powershell
Get-AzureRmServiceFabricCluster | Select-Object Name, ClusterCodeVersion
```

Not **ClusterCodeVersion** değeri. Bu değer bir sonraki bölümde kullanılır.

## <a name="upgrade-the-runtime"></a>Yükseltme çalışma zamanı

Değerini kullanmak **ClusterCodeVersion** ile önceki bölümdeki `Get-ServiceFabricRuntimeUpgradeVersion` hangi sürümlerinin yükseltmek kullanılabilir olduğunu öğrenmek için cmdlet. Bu cmdlet yalnızca internet'e bağlı bir bilgisayardan çalıştırabilirsiniz. Örneğin, yükseltme sürümünden hangi çalışma zamanı sürümlerini görmek istiyorsanız `5.7.198.9494`, aşağıdaki komutu kullanın:

```powershell
Get-ServiceFabricRuntimeUpgradeVersion -BaseVersion "5.7.198.9494"
```

Sürümlerinin listesi ile daha yeni bir çalışma zamanı yükseltmek için Azure Service Fabric kümesi anlayabilirsiniz. Örneğin, varsa sürüm `6.0.219.9494` kümenizi yükseltmek için aşağıdaki komutu kullanın yükseltmek kullanılabilir.

```powershell
Set-AzureRmServiceFabricUpgradeType -ResourceGroupName SFCLUSTERTUTORIALGROUP `
                                    -Name aztestcluster `
                                    -UpgradeMode Manual `
                                    -Version "6.0.219.9494"
```

> [!IMPORTANT]
> Küme çalışma zamanı yükseltme tamamlanması uzun sürebilir. Yükseltme çalışırken PowerShell engellendi. Yükseltme durumunu denetlemek için başka bir PowerShell oturumu kullanın.

Yükseltme durumu ya da PowerShell ile izlenebilir veya `sfctl` CLI.

İlk öğreticinin ilk bölümünde oluşturulan SSL sertifikası ile kümeye bağlanın. Kullanım `Connect-ServiceFabricCluster` cmdlet'ini veya `sfctl cluster upgrade-status`.

```powershell
$endpoint = "<mycluster>.southcentralus.cloudapp.azure.com:19000"
$thumbprint = "63EB5BA4BC2A3BADC42CA6F93D6F45E5AD98A1E4"

Connect-ServiceFabricCluster -ConnectionEndpoint $endpoint `
                             -KeepAliveIntervalInSec 10 `
                             -X509Credential -ServerCertThumbprint $thumbprint `
                             -FindType FindByThumbprint -FindValue $thumbprint `
                             -StoreLocation CurrentUser -StoreName My
```

```azurecli
sfctl cluster select --endpoint https://aztestcluster.southcentralus.cloudapp.azure.com:19080 \
--pem ./aztestcluster201709151446.pem --no-verify
```

Ardından, kullanın `Get-ServiceFabricClusterUpgrade` veya `sfctl cluster upgrade-status` durumunu görüntülemek için. Aşağıdaki sonucu benzer bir şey gösterilir.

```powershell
Get-ServiceFabricClusterUpgrade

TargetCodeVersion                          : 6.0.219.9494
TargetConfigVersion                        : 3
StartTimestampUtc                          : 11/28/2017 3:09:48 AM
UpgradeState                               : RollingForwardPending
UpgradeDuration                            : 00:09:00
CurrentUpgradeDomainDuration               : 00:09:00
NextUpgradeDomain                          : 1
UpgradeDomainsStatus                       : { "0" = "Completed";
                                             "1" = "Pending";
                                             "2" = "Pending";
                                             "3" = "Pending";
                                             "4" = "Pending" }
UpgradeKind                                : Rolling
RollingUpgradeMode                         : Monitored
FailureAction                              : Rollback
ForceRestart                               : False
UpgradeReplicaSetCheckTimeout              : 37201.09:59:01
HealthCheckWaitDuration                    : 00:05:00
HealthCheckStableDuration                  : 00:05:00
HealthCheckRetryTimeout                    : 00:45:00
UpgradeDomainTimeout                       : 02:00:00
UpgradeTimeout                             : 12:00:00
ConsiderWarningAsError                     : False
MaxPercentUnhealthyApplications            : 0
MaxPercentUnhealthyNodes                   : 100
ApplicationTypeHealthPolicyMap             : {}
EnableDeltaHealthEvaluation                : True
MaxPercentDeltaUnhealthyNodes              : 0
MaxPercentUpgradeDomainDeltaUnhealthyNodes : 0
ApplicationHealthPolicyMap                 : {}
```

```azurecli
sfctl cluster upgrade-status

{
  "codeVersion": "6.0.219.9494",
  "configVersion": "3",

... item cut to save space ...

  },
  "upgradeDomains": [
    {
      "name": "0",
      "state": "Completed"
    },
    {
      "name": "1",
      "state": "Pending"
    },
    {
      "name": "2",
      "state": "Pending"
    },
    {
      "name": "3",
      "state": "Pending"
    },
    {
      "name": "4",
      "state": "Pending"
    }
  ],
  "upgradeDurationInMilliseconds": "PT1H2M4.63889S",
  "upgradeState": "RollingForwardPending"
}
```

## <a name="conclusion"></a>Sonuç
Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Küme çalışma zamanı sürümünü edinin
> * Yükseltme küme çalışma zamanı
> * İzleyici yükseltme
