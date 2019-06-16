---
title: Bir Azure Service Fabric tek başına küme sürümünü yükseltin | Microsoft Docs
description: Bir tek başına Service Fabric kümesinde çalışan Azure Service Fabric kodu yükseltin.
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: chackdan
editor: ''
ms.assetid: 66296cc6-9524-4c6a-b0a6-57c253bdf67e
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/09/2018
ms.author: dekapur
ms.openlocfilehash: 29d034be5999d0bc3f0a244cfa7a5658a4ecce32
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60711377"
---
# <a name="upgrade-the-service-fabric-version-that-runs-on-your-cluster"></a>Kümenizde çalışan Service Fabric sürümüne yükseltin 

Herhangi bir modern sistemi için yükseltme olanağı ürününüzden uzun süreli başarının anahtarıdır. Bir Azure Service Fabric kümesine sahip olduğunuz bir kaynaktır. Bu makalede, Service Fabric, tek başına küme üzerinde çalışan sürümünü yükseltmek açıklar.

> [!NOTE]
> Kümeniz her zaman desteklenen bir Service Fabric sürümü çalıştığından emin olun. Microsoft, Service Fabric, yeni bir sürüm olduğunu bildirir, önceki sürümü en az Duyurunun tarihinden 60 gün sonra destek sonu için işaretlenir. Yeni yayınlar duyurulan [Service Fabric Ekibi blogunda](https://blogs.msdn.microsoft.com/azureservicefabric/). Bu noktada seçmek yeni sürüm kullanılabilir.
>
>

Her bir Service Fabric düğüm ayrı fiziksel veya sanal makine üzerinde nereye ayrılan bir üretim stili düğüm yapılandırması yalnızca kullanıyorsanız, kümenizi en yeni sürüme yükseltebilirsiniz. Birden fazla Service Fabric düğümü tek bir fiziksel veya sanal makine üzerinde olduğu bir geliştirme kümesi varsa kümenin yeni sürümü ile yeniden oluşturmanız gerekir.

İki ayrı iş akışları, kümenizin en son sürümü veya desteklenen bir Service Fabric sürüme yükseltebilirsiniz. Bir iş akışı otomatik olarak en son sürümü indirmek için bağlantıya sahip kümeler için kullanılıyor. Diğer iş akışı, en son Service Fabric sürümü indirmek için bağlantınız kümeler için kullanılıyor.

## <a name="enable-auto-upgrade-of-the-service-fabric-version-of-your-cluster"></a>Kümenizin Service Fabric sürümü otomatik yükseltmeyi etkinleştir
Microsoft yeni bir sürümü yayımlandığında, Service Fabric Güncelleştirmeleri indirmek için küme için Ayarla `fabricClusterAutoupgradeEnabled` küme yapılandırması *true*. El ile kümenizi olmasını istediğiniz Service Fabric desteklenen bir sürümünü seçmek için ayarlanmış `fabricClusterAutoupgradeEnabled` küme yapılandırması *false*.

## <a name="upgrade-clusters-that-have-connectivity-to-download-the-latest-code-and-configuration"></a>En son kod ve yapılandırma indirmek için bağlantıya sahip kümelerini yükseltme
Küme düğümlerinizi internet bağlantısı varsa, desteklenen bir sürüme kümenizi yükseltmek için bu adımları kullanın [Microsoft Download Center](https://download.microsoft.com).

Bağlantısına sahip kümeler [Microsoft Download Center](https://download.microsoft.com), Microsoft, yeni Service Fabric sürümleri kullanılabilirliği için düzenli olarak denetler.

Yeni bir Service Fabric sürümü kullanılabilir olduğunda, paket kümeye yerel olarak indirilir ve yükseltme için hazırlandı. Ayrıca, müşteri bu yeni sürümün bilgilendirmek için sistem aşağıdakine benzer bir açık küme sistem durumu uyarısı gösterir:

"Geçerli küme sürümü [version #] desteği [date] sona erer."

En son sürümü küme çalışmaya başladıktan sonra uyarı kaybolduktan.

Küme sistem durumu uyarısı gördüğünüzde, Küme yükseltme:

1. Kümedeki düğümler olarak listelenen tüm makineler için yönetici erişimi olan herhangi bir makineden kümeye bağlanın. Bu betiğin çalıştırıldığı makine kümesinin parçası olmak zorunda değildir.

    ```powershell
    ###### connect to the secure cluster using certs
    $ClusterName= "mysecurecluster.something.com:19000"
    $CertThumbprint= "70EF5E22ADB649799DA3C8B6A6BF7FG2D630F8F3"
    Connect-serviceFabricCluster -ConnectionEndpoint $ClusterName -KeepAliveIntervalInSec 10 `
        -X509Credential `
        -ServerCertThumbprint $CertThumbprint  `
        -FindType FindByThumbprint `
        -FindValue $CertThumbprint `
        -StoreLocation CurrentUser `
        -StoreName My
    ```

2. Yükseltmeden Service Fabric sürümleri listesini alın.

    ```powershell
    ###### Get the list of available Service Fabric versions
    Get-ServiceFabricRegisteredClusterCodeVersion
    ```

    Buna benzer bir çıktı almalısınız:

    ![Service Fabric sürümlerini edinme][getfabversions]
3. Kullanarak bir küme yükseltmesi mevcut bir sürüm başlatın [başlangıç ServiceFabricClusterUpgrade](https://docs.microsoft.com/powershell/module/servicefabric/start-servicefabricclusterupgrade) Windows PowerShell komutu.

    ```powershell
    Start-ServiceFabricClusterUpgrade -Code -CodePackageVersion <codeversion#> -Monitored -FailureAction Rollback

    ###### Here is a filled-out example

    Start-ServiceFabricClusterUpgrade -Code -CodePackageVersion 5.3.301.9590 -Monitored -FailureAction Rollback
    ```
   Yükseltmenin ilerleme durumunu izlemek için Service Fabric Explorer'ı kullanın veya aşağıdaki PowerShell komutunu çalıştırın:

    ```powershell
    Get-ServiceFabricClusterUpgrade
    ```

    Küme sistem durumu ilkeleri karşılanmadığı takdirde yükseltme geri alınır. Başlangıç ServiceFabricClusterUpgrade komutu için özel sistem durumu ilkeleri belirtmek için belgelerine bakın. [başlangıç ServiceFabricClusterUpgrade](https://docs.microsoft.com/powershell/module/servicefabric/start-servicefabricclusterupgrade).

    Geri almaya sonuçlanan sorunları giderdikten sonra daha önce açıklandığı gibi aynı adımları izleyerek yükseltmeyi yeniden başlatın.

## <a name="upgrade-clusters-that-have-no-connectivity-to-download-the-latest-code-and-configuration"></a>Sahip kümelerini yükseltme *bağlantı* en son kod ve yapılandırma indirilemedi
Küme düğümlerinizi internet bağlantısı yoksa, kümenizi desteklenen bir sürüme yükseltmek için bu adımları kullanın [Microsoft Download Center](https://download.microsoft.com).

> [!NOTE]
> İnternet'e bağlı olmayan bir küme çalıştırıyorsanız izlemek zorunda [Service Fabric ekibi blogu](https://blogs.msdn.microsoft.com/azureservicefabric/) yeni yayınlar hakkında bilgi edinmek için. Sistem, yeni sürümler sizi uyarmak için küme sistem durumu uyarısı göstermez.  
>
>

### <a name="auto-provisioning-vs-manual-provisioning"></a>Otomatik ve el ile sağlama sağlama
Otomatik olarak indirilip ve en son kod sürümü kaydı etkinleştirmek için Service Fabric güncelleştirme hizmeti ayarlayın. Yönergeler için *Tools\ServiceFabricUpdateService.zip\Readme_InstructionsAndHowTos.txt* içinde [tek başına paketin](service-fabric-cluster-standalone-package-contents.md).

El ile işlem için bu yönergeleri izleyin.

Şu özellik ayarlamak için küme yapılandırmanızı değiştirme *false* bir yapılandırma yükseltme işlemine başlamadan önce:

```json
"fabricClusterAutoupgradeEnabled": false,
```

Kullanım ayrıntıları için bkz. [başlangıç ServiceFabricClusterConfigurationUpgrade](https://docs.microsoft.com/powershell/module/servicefabric/start-servicefabricclusterconfigurationupgrade) PowerShell komutu. JSON ' clusterConfigurationVersion' yapılandırmasını yükseltme işlemine başlamadan önce güncelleştirdiğinizden emin olun.

```powershell
    Start-ServiceFabricClusterConfigurationUpgrade -ClusterConfigPath <Path to Configuration File>
```

### <a name="cluster-upgrade-workflow"></a>Küme yükseltme iş akışı

1. Çalıştırma [Get-ServiceFabricClusterUpgrade](https://docs.microsoft.com/powershell/module/servicefabric/get-servicefabricclusterupgrade) Not ve küme düğümlerinin birinden *TargetCodeVersion*.

2. Geçerli sürümle tüm yükseltme uyumlu sürümler listesinde ve ilişkili indirme bağlantıları karşılık gelen paketi indirmek için İnternet'e bağlı makineden aşağıdaki komutu çalıştırın:

    ```powershell
    ###### Get list of all upgrade compatible packages  
    Get-ServiceFabricRuntimeUpgradeVersion -BaseVersion <TargetCodeVersion as noted in Step 1> 
    ```

3. Kümedeki düğümler olarak listelenen tüm makineler için yönetici erişimi olan herhangi bir makineden kümeye bağlanın. Bu betiğin çalıştırıldığı makine kümesinin parçası olmak zorunda değildir.

    ```powershell
    ###### Get the list of available Service Fabric versions
    Copy-ServiceFabricClusterPackage -Code -CodePackagePath <name of the .cab file including the path to it> -ImageStoreConnectionString "fabric:ImageStore"

    ###### Here is a filled-out example
    Copy-ServiceFabricClusterPackage -Code -CodePackagePath .\MicrosoftAzureServiceFabric.5.3.301.9590.cab -ImageStoreConnectionString "fabric:ImageStore"
    ```
4. İndirilen paketi kümenin görüntü deposuna kopyalayın.

5. Kopyalanan paket kaydedin.

    ```powershell
    ###### Get the list of available Service Fabric versions
    Register-ServiceFabricClusterPackage -Code -CodePackagePath <name of the .cab file>

    ###### Here is a filled-out example
    Register-ServiceFabricClusterPackage -Code -CodePackagePath .\MicrosoftAzureServiceFabric.5.3.301.9590.cab
    ```
6. Bir küme yükseltmesi mevcut bir sürüm başlatın.

    ```powershell
    Start-ServiceFabricClusterUpgrade -Code -CodePackageVersion <codeversion#> -Monitored -FailureAction Rollback

    ###### Here is a filled-out example
    Start-ServiceFabricClusterUpgrade -Code -CodePackageVersion 5.3.301.9590 -Monitored -FailureAction Rollback
    ```
    Service Fabric Explorer yükseltmesinin ilerleme durumunu izleyebilir veya aşağıdaki PowerShell komutunu çalıştırabilirsiniz:

    ```powershell
    Get-ServiceFabricClusterUpgrade
    ```

    Küme sistem durumu ilkeleri karşılanmadığı takdirde yükseltme geri alınır. Başlangıç ServiceFabricClusterUpgrade komutu için özel sistem durumu ilkeleri belirtmek için belgelerine bakın. [başlangıç ServiceFabricClusterUpgrade](https://docs.microsoft.com/powershell/module/servicefabric/start-servicefabricclusterupgrade).

    Geri almaya sonuçlanan sorunları giderdikten sonra daha önce açıklandığı gibi aynı adımları izleyerek yükseltmeyi yeniden başlatın.

## <a name="next-steps"></a>Sonraki adımlar
* [Tek başına küme yapılandırmasını Yükselt](service-fabric-cluster-config-upgrade-windows-server.md)
* Bazı özelleştirme [Service Fabric küme ayarlarını](service-fabric-cluster-fabric-settings.md).
* [Kümenizin ölçeğini daraltma ve genişletme](service-fabric-cluster-scale-up-down.md).

<!--Image references-->
[getfabversions]: ./media/service-fabric-cluster-upgrade-windows-server/getfabversions.PNG
