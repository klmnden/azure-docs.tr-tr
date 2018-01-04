---
title: "Windows Server tek başına Azure Service Fabric kümede yükseltme | Microsoft Docs"
description: "Azure Service Fabric kod ve/veya küme güncelleştirme modu ayarı dahil olmak üzere bir tek başına Service Fabric kümesi çalıştıran yapılandırma yükseltin."
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: 
ms.assetid: 66296cc6-9524-4c6a-b0a6-57c253bdf67e
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/15/2017
ms.author: dekapur
ms.openlocfilehash: c95c1827d0433dcb61eace34e7a905a5610c7781
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/21/2017
---
# <a name="upgrade-your-standalone-azure-service-fabric-cluster-on-windows-server"></a>Windows Server, tek başına Azure Service Fabric kümede yükseltme 
> [!div class="op_single_selector"]
> * [Azure küme](service-fabric-cluster-upgrade.md)
> * [Tek başına küme](service-fabric-cluster-upgrade-windows-server.md)
>
>

Tüm modern, yükseltme olanağı ürününüzün uzun vadeli başarı anahtarına sistemidir. Azure Service Fabric kümesi sahip olduğunuz bir kaynaktır. Bu makalede, kümenin her zaman Service Fabric kod ve yapılandırmaları desteklenen sürümleri çalıştıran nasıl emin olabileceğiniz açıklanır.

## <a name="control-the-service-fabric-version-that-runs-on-your-cluster"></a>Kümenizde çalışan Service Fabric sürüm denetimi
Microsoft yeni bir sürümü yayımlandığında Service Fabric güncelleştirmeleri karşıdan yüklemek için küme ayarlamak için fabricClusterAutoupgradeEnabled küme yapılandırmasını ayarlayın *doğru*. FabricClusterAutoupgradeEnabled küme yapılandırması, küme üzerinde olmasını istediğiniz Service Fabric desteklenen bir sürümünü seçmek için kümesine *false*.

> [!NOTE]
> Kümenizi desteklenen bir Service Fabric sürümü her zaman çalıştığından emin olun. Microsoft Service Fabric, yeni bir sürüm olduğunu bildirir, önceki sürümü en az Duyurunun 60 gün sonra destek sonu için işaretlenir. Yeni sürümler duyurdu [Service Fabric ekip blogunda](https://blogs.msdn.microsoft.com/azureservicefabric/). Bu noktada seçmek yeni sürümü kullanılabilir.
>
>

Her Service Fabric düğümü ayrı fiziksel veya sanal makine üzerinde nerede ayrılmış bir üretim stili düğüm yapılandırması yalnızca kullanıyorsanız, kümenizi yeni sürüme yükseltebilirsiniz. Birden fazla Service Fabric düğümü tek bir fiziksel veya sanal makine üzerinde olduğu bir geliştirme kümeniz varsa, yeni sürümü kümeye yeniden oluşturmanız gerekir.

İki ayrı iş akışları kümenizi en son sürümünü veya desteklenen bir Service Fabric sürüme yükseltebilirsiniz. Bir iş akışı otomatik olarak en son sürümünü indirmek için bağlantıya sahip kümeler için kullanılıyor. Diğer iş akışı en son Service Fabric sürümünü indirmek için bağlantı yok kümeleri için kullanılıyor.

### <a name="upgrade-clusters-that-have-connectivity-to-download-the-latest-code-and-configuration"></a>En son kod ve yapılandırma indirmek için bağlantınız kümeleri yükseltme
Küme düğümlerinizi için Internet bağlantınız varsa kümenizi desteklenen bir sürüme yükseltmek için aşağıdaki adımları kullanın [Microsoft Download Center](http://download.microsoft.com).

Bağlantınız kümeleri için [Microsoft Download Center](http://download.microsoft.com), Microsoft yeni Service Fabric sürümleri kullanılabilirlik için düzenli olarak denetler.

Yeni bir Service Fabric sürümü kullanılabilir olduğunda, paketi yerel olarak kümeye indirilir ve yükseltme için sağlanan. Ayrıca, bu yeni sürüm müşteri bildirmek için aşağıdakine benzer bir açık küme sistem durumu uyarısı sistem gösterir:

"Geçerli küme sürümü [version #] destek [date] sona erer."

En son sürümünü çalıştıran küme sonra uyarı kaybolduktan.

#### <a name="cluster-upgrade-workflow"></a>Küme yükseltme iş akışı
Küme sistem durumu uyarısı gördüğünüzde, aşağıdakileri yapın:

1. Kümedeki düğümler olarak listelenen tüm makineler yönetici erişimi olan herhangi makineden kümeye bağlanın. Bu komut dosyasını çalıştırmak makine kümesinin parçası olması gerekmez.

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

2. Yükseltmeden Service Fabric sürümlerinin listesini alın.

    ```powershell

    ###### Get the list of available Service Fabric versions
    Get-ServiceFabricRegisteredClusterCodeVersion
    ```

    Buna benzer bir çıktı almalısınız:

    ![Service Fabric sürümlerini alma][getfabversions]
3. Kullanılabilir bir sürümüne Küme yükseltme başlamayı [başlangıç ServiceFabricClusterUpgrade](https://msdn.microsoft.com/library/mt125872.aspx) Windows PowerShell komutu.

    ```Powershell

    Start-ServiceFabricClusterUpgrade -Code -CodePackageVersion <codeversion#> -Monitored -FailureAction Rollback

    ###### Here is a filled-out example

    Start-ServiceFabricClusterUpgrade -Code -CodePackageVersion 5.3.301.9590 -Monitored -FailureAction Rollback

    ```
   Yükseltmenin ilerleme durumunu izlemek için Service Fabric Explorer kullanın veya aşağıdaki PowerShell komutunu çalıştırın:

    ```powershell

    Get-ServiceFabricClusterUpgrade
    ```

    Küme sistem durumu ilkeleri karşılanmadığı takdirde yükseltme geri alınır. Başlangıç ServiceFabricClusterUpgrade komutu için özel sistem durumu ilkeleri belirtmek için belgelerine bakın [başlangıç ServiceFabricClusterUpgrade](https://msdn.microsoft.com/library/mt125872.aspx).

Geri alma sonuçlandı sorunları giderdikten sonra yükseltme daha önce açıklanan adımları izleyerek yeniden başlatın.

### <a name="upgrade-clusters-that-have-no-connectivity-to-download-the-latest-code-and-configuration"></a>Sahip kümeler yükseltme *hiç bağlantı* en son kod ve yapılandırma indirmek için
Küme düğümlerinizi internet bağlantısı yoksa, kümenizi desteklenen bir sürüme yükseltmek için aşağıdaki adımları kullanın [Microsoft Download Center](http://download.microsoft.com).

> [!NOTE]
> İnternet'e bağlı olmayan bir küme çalıştırıyorsanız, yeni sürümleri hakkında bilgi edinmek için Service Fabric ekip blogu izlemek zorunda. Sistem, yeni sürümlerin sizi uyarmak için bir küme sistem durumu uyarısı göstermez.  
>
>

#### <a name="auto-provisioning-vs-manual-provisioning"></a>Otomatik el ile sağlama ve sağlama
Otomatik karşıdan yükleniyor ve en son kod sürümü için kaydı etkinleştirmek için Service Fabric Güncelleştirme hizmetini ayarlayın. İçinde Tools\ServiceFabricUpdateService.zip\Readme_InstructionsAndHowTos.txt yönergeler için bkz [tek başına paket](service-fabric-cluster-standalone-package-contents.md).
El ile işlem için bu yönergeleri izleyin.

Aşağıdaki özelliği ayarlamak için küme yapılandırmanızı değiştirme *false* yapılandırma yükseltmeye başlamadan önce:

        "fabricClusterAutoupgradeEnabled": false,

Kullanım ayrıntıları için bkz: [başlangıç ServiceFabricClusterConfigurationUpgrade PowerShell komutunu](https://msdn.microsoft.com/en-us/library/mt788302.aspx). Yapılandırma yükseltmeye başlamadan önce JSON ' clusterConfigurationVersion' güncelleştirdiğinizden emin olun.

```powershell

    Start-ServiceFabricClusterConfigurationUpgrade -ClusterConfigPath <Path to Configuration File>

```

#### <a name="cluster-upgrade-workflow"></a>Küme yükseltme iş akışı

1. Get-ServiceFabricClusterUpgrade Kümedeki düğümlerden birinde çalıştırın ve TargetCodeVersion dikkat edin.

2. Geçerli sürümle tüm yükseltme uyumlu sürümlerini listeler ve karşılık gelen paket ilişkili indirme bağlantılarından indirmek için internet'e bağlı bir makineden aşağıdakini çalıştırın:

    ```powershell

    ###### Get list of all upgrade compatible packages  
    Get-ServiceFabricRuntimeUpgradeVersion -BaseVersion <TargetCodeVersion as noted in Step 1> 
    ```

3. Kümedeki düğümler olarak listelenen tüm makineler yönetici erişimi olan herhangi makineden kümeye bağlanın. Bu komut dosyasını çalıştırmak makine kümesinin parçası olması gerekmez.

    ```powershell

   ###### Get the list of available Service Fabric versions
    Copy-ServiceFabricClusterPackage -Code -CodePackagePath <name of the .cab file including the path to it> -ImageStoreConnectionString "fabric:ImageStore"

   ###### Here is a filled-out example
    Copy-ServiceFabricClusterPackage -Code -CodePackagePath .\MicrosoftAzureServiceFabric.5.3.301.9590.cab -ImageStoreConnectionString "fabric:ImageStore"

    ```
4. İndirilen paketi küme görüntü deposuna kopyalama.

5. Kopyalanan paketi kaydedin.

    ```powershell

    ###### Get the list of available Service Fabric versions
    Register-ServiceFabricClusterPackage -Code -CodePackagePath <name of the .cab file>

    ###### Here is a filled-out example
    Register-ServiceFabricClusterPackage -Code -CodePackagePath MicrosoftAzureServiceFabric.5.3.301.9590.cab

     ```
6. Kullanılabilir bir sürümüne Küme yükseltme başlatın.

    ```Powershell

    Start-ServiceFabricClusterUpgrade -Code -CodePackageVersion <codeversion#> -Monitored -FailureAction Rollback

    ###### Here is a filled-out example
    Start-ServiceFabricClusterUpgrade -Code -CodePackageVersion 5.3.301.9590 -Monitored -FailureAction Rollback

    ```
   Service Fabric Explorer üzerinde yükseltmesinin ilerleme durumunu izleyebilirsiniz veya aşağıdaki PowerShell komutunu çalıştırabilirsiniz:

    ```powershell

    Get-ServiceFabricClusterUpgrade
    ```

    Küme sistem durumu ilkeleri karşılanmadığı takdirde yükseltme geri alınır. Başlangıç ServiceFabricClusterUpgrade komutu için özel sistem durumu ilkeleri belirtmek için belgelerine bakın [başlangıç ServiceFabricClusterUpgrade](https://msdn.microsoft.com/library/mt125872.aspx).

Geri alma sonuçlandı sorunları giderdikten sonra yükseltme daha önce açıklanan adımları izleyerek yeniden başlatın.


## <a name="upgrade-the-cluster-configuration"></a>Yükseltme küme yapılandırması
Yapılandırma Yükseltmeyi başlatmadan önce tek başına paketinde aşağıdaki PowerShell betiğini çalıştırarak yeni küme yapılandırmanızı JSON test edebilirsiniz:

```powershell

    TestConfiguration.ps1 -ClusterConfigFilePath <Path to the new Configuration File> -OldClusterConfigFilePath <Path to the old Configuration File>

```
Veya bu komut dosyasını kullanın:

```powershell

    TestConfiguration.ps1 -ClusterConfigFilePath <Path to the new Configuration File> -OldClusterConfigFilePath <Path to the old Configuration File> -FabricRuntimePackagePath <Path to the .cab file which you want to test the configuration against>

```

Bazı yapılandırmalar olamaz, uç noktaları gibi küme adı, düğüm IP yükseltilmiş vb. Yeni küme yapılandırması JSON eskisinin karşı test ve bir sorun olduğunda hataları PowerShell penceresinde oluşturur.

Küme yapılandırması yükseltme yükseltmek için başlangıç ServiceFabricClusterConfigurationUpgrade çalıştırın. Yapılandırma yükseltme yükseltme etki alanı tarafından işlenen yükseltme etki alanıdır.

```powershell

    Start-ServiceFabricClusterConfigurationUpgrade -ClusterConfigPath <Path to Configuration File>

```

### <a name="cluster-certificate-config-upgrade"></a>Küme sertifikası config yükseltme  
Küme sertifikası, küme düğümleri arasında kimlik doğrulaması için kullanılır. Küme düğümleri arasında iletişim hatası engellediği için sertifika geçişine ek dikkatli gerçekleştirilmesi gerekir.

Teknik olarak, dört seçenekleri desteklenir:  

* Tek sertifika yükseltme: yükseltme yolu (birincil) -> sertifika B (birincil) sertifikasıdır sertifika C (birincil) -> ->....

* Double sertifika yükseltme: (birincil) -> sertifika sertifika (birincil) ve B (ikincil) -> sertifika B (birincil) yükseltme yolu olan sertifika B (birincil) -> ve C (ikincil) sertifika C (birincil) -> ->....

* Sertifika türü yükseltme: sertifika parmak izi tabanlı yapılandırma <> – CommonName tabanlı sertifika yapılandırması. Örneğin, sertifika parmak izi (birincil) ve parmak izi B (ikincil) sertifika CommonName c ->

* Sertifika verenin parmak izi yükseltme: sertifika CN yükseltme yoludur A, IssuerThumbprint = IT1 = (birincil) sertifika CN -> A, IssuerThumbprint = IT1, IT2 = (birincil) sertifika CN -> A, IssuerThumbprint = IT2 = (birincil).


## <a name="next-steps"></a>Sonraki adımlar
* Bazı özelleştirmeyi öğrenin [Service Fabric kümesi ayarlarını](service-fabric-cluster-fabric-settings.md).
* Bilgi edinmek için nasıl [kümenizi ölçeğini](service-fabric-cluster-scale-up-down.md).
* Hakkında bilgi edinin [uygulama yükseltmelerini](service-fabric-application-upgrade.md).

<!--Image references-->
[getfabversions]: ./media/service-fabric-cluster-upgrade-windows-server/getfabversions.PNG
