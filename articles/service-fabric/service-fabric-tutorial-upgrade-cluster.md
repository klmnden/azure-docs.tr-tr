---
title: "Azure Service Fabric çalışma zamanını yükseltme | Microsoft Docs"
description: "Bu öğreticide, PowerShell kullanarak Azure’da barındırılan bir Service Fabric kümesinin çalışma zamanını yükseltmeyi öğrenirsiniz."
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
ms.custom: mvc
ms.openlocfilehash: 49211a88e004bbcbcc41b6674a34934db39513c7
ms.sourcegitcommit: fbba5027fa76674b64294f47baef85b669de04b7
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/24/2018
---
# <a name="tutorial-upgrade-the-runtime-of-a-service-fabric-cluster"></a>Öğretici: Bir Service Fabric kümesinin çalışma zamanını yükseltme

Bu öğretici, bir serinin üçüncü bölümüdür ve bir Azure Service Fabric kümesinde Service Fabric çalışma zamanının nasıl yükseltileceğini gösterir. Bu öğretici bölümü, Azure’da çalışan Service Fabric kümeleri için yazılmıştır ve tek başına Service Fabric kümeleri için geçerli değildir.

> [!WARNING]
> Öğreticinin bu bölümü PowerShell gerektirir. Henüz Azure CLI araçları tarafından küme çalışma zamanını yükseltme desteği sağlanmamaktadır. Alternatif olarak, kümeler portalda da yükseltilebilir. Daha fazla bilgi için bkz. [Bir Azure Service Fabric kümesini yükseltme](service-fabric-cluster-upgrade.md).

Kümeniz zaten en son Service Fabric çalışma zamanında çalışıyorsa bu adımı gerçekleştirmeniz gerekmez. Bununla birlikte, bir Azure Service Fabric kümesinde desteklenen herhangi bir çalışma zamanının yüklenmesi için bu makale kullanılabilir.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Küme sürümünü okuma
> * Küme sürümünü ayarlama

Bu öğretici dizisinde şunların nasıl yapıldığını öğrenirsiniz:
> [!div class="checklist"]
> * Şablon kullanarak Azure'da güvenli bir [Windows kümesi](service-fabric-tutorial-create-vnet-and-windows-cluster.md) veya [Linux kümesi](service-fabric-tutorial-create-vnet-and-linux-cluster.md) oluşturma
> * [Bir kümenin ölçeğini daraltma veya genişletme](service-fabric-tutorial-scale-cluster.md)
> * Bir kümenin çalışma zamanını yükseltme
> * [Service Fabric ile API Management dağıtma](service-fabric-tutorial-deploy-api-management.md)

## <a name="prerequisites"></a>Ön koşullar
Bu öğreticiye başlamadan önce:
- Azure aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun
- [Azure Powershell modülü sürüm 4.1 veya üzerini](https://docs.microsoft.com/powershell/azure/install-azurerm-ps) ya da [Azure CLI 2.0](/cli/azure/install-azure-cli)'ı yükleme
- Azure'da güvenli bir [Windows kümesi](service-fabric-tutorial-create-vnet-and-windows-cluster.md) veya [Linux kümesi](service-fabric-tutorial-create-vnet-and-linux-cluster.md) oluşturma
- Windows kümesi dağıtıyorsanız, bir Windows dağıtım ortamı ayarlayın. [Visual Studio 2017](http://www.visualstudio.com)'yi ve **Azure geliştirme**, **ASP.NET ve web geliştirme**, ayrıca **.NET Core çoklu platform geliştirme** iş yüklerini yükleyin.  Ardından bir [.NET dağıtım ortamı](service-fabric-get-started.md) ayarlayın.
- Linux kümesi dağıtıyorsanız, [Linux](service-fabric-get-started-linux.md) veya [MacOS](service-fabric-get-started-mac.md) üzerinde bir Java dağıtım ortamı ayarlayın.  [Service Fabric CLI](service-fabric-cli.md)'yı yükleyin. 

### <a name="sign-in-to-azure"></a>Azure'da oturum açma
Azure komutlarını yürütmeden önce Azure hesabınızda oturum açıp aboneliğinizi seçin.

```powershell
Login-AzureRmAccount
Get-AzureRmSubscription
Set-AzureRmContext -SubscriptionId <guid>
```

## <a name="get-the-runtime-version"></a>Çalıştırma sürümünü alma

Azure'a bağlanıp Service Fabric kümesini içeren aboneliği seçtiğinizde kümenin çalışma zamanı sürümünü öğrenebilirsiniz.

```powershell
Get-AzureRmServiceFabricCluster -ResourceGroupName SFCLUSTERTUTORIALGROUP -Name aztestcluster `
    | Select-Object ClusterCodeVersion
```

Dilerseniz de aşağıdaki kodla aboneliğinizdeki tüm kümelerin listesini alabilirsiniz:

```powershell
Get-AzureRmServiceFabricCluster | Select-Object Name, ClusterCodeVersion
```

**ClusterCodeVersion** değerini not alın. Bu değer bir sonraki bölümde kullanılacaktır.

## <a name="upgrade-the-runtime"></a>Çalışma zamanını yükseltme

Hangi sürümlere yükseltebileceğinizi öğrenmek için `Get-ServiceFabricRuntimeUpgradeVersion` cmdlet’i ile bir önceki bölümdeki **ClusterCodeVersion** değerini kullanın. Bu cmdlet yalnızca İnternet'e bağlı bir bilgisayardan çalıştırılabilir. Örneğin, `5.7.198.9494` sürümünden hangi çalışma zamanı sürümlerine yükseltebileceğinizi görmek istiyorsanız aşağıdaki komutu kullanın:

```powershell
Get-ServiceFabricRuntimeUpgradeVersion -BaseVersion "5.7.198.9494"
```

Sürüm listesiyle Azure Service Fabric kümesine daha yeni bir çalışma zamanına yükseltmesini söyleyebilirsiniz. Örneğin, `6.0.219.9494` sürümüne yükseltilebiliyorsa kümenizi yükseltmek için aşağıdaki komutu kullanın.

```powershell
Set-AzureRmServiceFabricUpgradeType -ResourceGroupName SFCLUSTERTUTORIALGROUP `
                                    -Name aztestcluster `
                                    -UpgradeMode Manual `
                                    -Version "6.0.219.9494"
```

> [!IMPORTANT]
> Küme çalışma zamanını yükseltme işleminin tamamlanması uzun sürebilir. Yükseltme çalıştığı sırada PowerShell engellenir. Yükseltme durumunu denetlemek için başka bir PowerShell oturumu kullanabilirsiniz.

Yükseltme durumu PowerShell veya `sfctl` CLI ile izlenebilir.

İlk olarak öğreticinin birinci bölümünde oluşturulan SSL sertifikası ile kümeye bağlanın. `Connect-ServiceFabricCluster` cmdlet’ini veya `sfctl cluster upgrade-status` komutunu kullanın.

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

Sonra durumu görüntülemek için `Get-ServiceFabricClusterUpgrade` veya `sfctl cluster upgrade-status` komutunu kullanın. Aşağıdakine benzer bir sonuç gösterilir.

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
> * Küme çalışma zamanının sürümünü öğrenme
> * Küme çalışma zamanını yükseltme
> * Yükseltme işlemini izleme

Ardından, bir Service Fabric kümesi ile API Management dağıtmayı öğrenmek için aşağıdaki öğreticiye ilerleyin.
> [!div class="nextstepaction"]
> [Service Fabric ile API Management dağıtma](service-fabric-tutorial-deploy-api-management.md)
