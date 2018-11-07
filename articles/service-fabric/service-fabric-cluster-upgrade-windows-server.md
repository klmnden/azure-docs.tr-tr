---
title: Windows Server'da bir tek başına Azure Service Fabric kümesini yükseltme | Microsoft Docs
description: Azure Service Fabric kodu ve/veya küme güncelleştirme modunu ayarlama dahil olmak üzere bir tek başına Service Fabric kümesi çalıştıran yapılandırma yükseltin.
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: ''
ms.assetid: 66296cc6-9524-4c6a-b0a6-57c253bdf67e
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/15/2017
ms.author: dekapur
ms.openlocfilehash: 2190978b2583b2f5d8a1b10431c65fd24fe6842d
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/07/2018
ms.locfileid: "51228156"
---
# <a name="upgrade-your-standalone-azure-service-fabric-cluster-on-windows-server"></a>Windows Server'da tek başına Azure Service Fabric kümenizi yükseltme 
> [!div class="op_single_selector"]
> * [Azure kümesine](service-fabric-cluster-upgrade.md)
> * [Tek başına küme](service-fabric-cluster-upgrade-windows-server.md)
>
>

Herhangi bir modern sistemi için yükseltme olanağı ürününüzden uzun süreli başarının anahtarıdır. Bir Azure Service Fabric kümesine sahip olduğunuz bir kaynaktır. Bu makalede, kümenin her zaman, Service Fabric kodu ve yapılandırmaları desteklenen sürümleri çalıştıran nasıl emin olabileceğiniz açıklanır.

## <a name="control-the-service-fabric-version-that-runs-on-your-cluster"></a>Kümenizde çalışan Service Fabric sürüm denetimi
Kümenizin Microsoft yeni bir sürümü yayımlandığında, Service Fabric Güncelleştirmeleri indirmek için ayarlanacak fabricClusterAutoupgradeEnabled küme yapılandırmasını ayarlayın *true*. Kümenizi olmasını istediğiniz Service Fabric desteklenen bir sürümünü seçmek için fabricClusterAutoupgradeEnabled küme yapılandırmasını ayarlayın *false*.

> [!NOTE]
> Kümeniz her zaman desteklenen bir Service Fabric sürümü çalıştığından emin olun. Microsoft, Service Fabric, yeni bir sürüm olduğunu bildirir, önceki sürümü en az Duyurunun tarihinden 60 gün sonra destek sonu için işaretlenir. Yeni yayınlar duyurulan [Service Fabric Ekibi blogunda](https://blogs.msdn.microsoft.com/azureservicefabric/). Bu noktada seçmek yeni sürüm kullanılabilir.
>
>

Her bir Service Fabric düğüm ayrı fiziksel veya sanal makine üzerinde nereye ayrılan bir üretim stili düğüm yapılandırması yalnızca kullanıyorsanız, kümenizi en yeni sürüme yükseltebilirsiniz. Birden fazla Service Fabric düğümü tek bir fiziksel veya sanal makine üzerinde olduğu bir geliştirme kümesi varsa kümenin yeni sürümü ile yeniden oluşturmanız gerekir.

İki ayrı iş akışları, kümenizin en son sürümü veya desteklenen bir Service Fabric sürüme yükseltebilirsiniz. Bir iş akışı otomatik olarak en son sürümü indirmek için bağlantıya sahip kümeler için kullanılıyor. Diğer iş akışı, en son Service Fabric sürümü indirmek için bağlantınız kümeler için kullanılıyor.

### <a name="upgrade-clusters-that-have-connectivity-to-download-the-latest-code-and-configuration"></a>En son kod ve yapılandırma indirmek için bağlantıya sahip kümelerini yükseltme
Küme düğümlerinizi internet bağlantısı varsa, desteklenen bir sürüme kümenizi yükseltmek için bu adımları kullanın [Microsoft Download Center](https://download.microsoft.com).

Bağlantısına sahip kümeler [Microsoft Download Center](https://download.microsoft.com), Microsoft, yeni Service Fabric sürümleri kullanılabilirliği için düzenli olarak denetler.

Yeni bir Service Fabric sürümü kullanılabilir olduğunda, paket kümeye yerel olarak indirilir ve yükseltme için hazırlandı. Ayrıca, müşteri bu yeni sürümün bilgilendirmek için sistem aşağıdakine benzer bir açık küme sistem durumu uyarısı gösterir:

"Geçerli küme sürümü [version #] desteği [date] sona erer."

En son sürümü küme çalışmaya başladıktan sonra uyarı kaybolduktan.

#### <a name="cluster-upgrade-workflow"></a>Küme yükseltme iş akışı
Küme sistem durumu uyarısı gördüğünüzde, aşağıdakileri yapın:

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

    ```Powershell

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

### <a name="upgrade-clusters-that-have-no-connectivity-to-download-the-latest-code-and-configuration"></a>Sahip kümelerini yükseltme *bağlantı* en son kod ve yapılandırma indirilemedi
Küme düğümlerinizi internet bağlantısı yoksa, kümenizi desteklenen bir sürüme yükseltmek için bu adımları kullanın [Microsoft Download Center](https://download.microsoft.com).

> [!NOTE]
> İnternet'e bağlı olmayan bir küme çalıştırıyorsanız, yeni yayınlar hakkında bilgi edinmek için Service Fabric ekibi blogu izlemek zorunda. Sistem, yeni sürümler sizi uyarmak için küme sistem durumu uyarısı göstermez.  
>
>

#### <a name="auto-provisioning-vs-manual-provisioning"></a>Otomatik ve el ile sağlama sağlama
Otomatik olarak indirilip ve en son kod sürümü kaydı etkinleştirmek için Service Fabric güncelleştirme hizmeti ayarlayın. İçinde Tools\ServiceFabricUpdateService.zip\Readme_InstructionsAndHowTos.txt yönergeler için bkz [tek başına paketin](service-fabric-cluster-standalone-package-contents.md).
El ile işlem için bu yönergeleri izleyin.

Şu özellik ayarlamak için küme yapılandırmanızı değiştirme *false* bir yapılandırma yükseltme işlemine başlamadan önce:

        "fabricClusterAutoupgradeEnabled": false,

Kullanım ayrıntıları için bkz. [başlangıç ServiceFabricClusterConfigurationUpgrade](https://docs.microsoft.com/powershell/module/servicefabric/start-servicefabricclusterconfigurationupgrade) PowerShell komutu. JSON ' clusterConfigurationVersion' yapılandırmasını yükseltme işlemine başlamadan önce güncelleştirdiğinizden emin olun.

```powershell

    Start-ServiceFabricClusterConfigurationUpgrade -ClusterConfigPath <Path to Configuration File>

```

#### <a name="cluster-upgrade-workflow"></a>Küme yükseltme iş akışı

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

    ```Powershell

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


## <a name="upgrade-the-cluster-configuration"></a>Küme yapılandırmasını Yükselt
Yapılandırma Yükseltmeyi başlatmadan önce yeni küme yapılandırma JSON tek başına paketteki aşağıdaki PowerShell betiğini çalıştırarak test edebilirsiniz:

```powershell

    TestConfiguration.ps1 -ClusterConfigFilePath <Path to the new Configuration File> -OldClusterConfigFilePath <Path to the old Configuration File>

```
Veya bu betiği kullanın:

```powershell

    TestConfiguration.ps1 -ClusterConfigFilePath <Path to the new Configuration File> -OldClusterConfigFilePath <Path to the old Configuration File> -FabricRuntimePackagePath <Path to the .cab file which you want to test the configuration against>

```

Bazı yapılandırmalar olamaz, uç noktaları gibi küme adı, düğüm IP yükseltilmiş vb. Yeni küme yapılandırma JSON karşı eski bir test edilir ve bir sorun olduğunda hataları PowerShell penceresinde oluşturur.

Küme yapılandırması yükseltme yükseltmek için çalıştırmanız [başlangıç ServiceFabricClusterConfigurationUpgrade](https://docs.microsoft.com/powershell/module/servicefabric/start-servicefabricclusterconfigurationupgrade). Yükseltme etki alanı tarafından işlenen yükseltme etki alanı yapılandırma yükseltmedir.

```powershell

    Start-ServiceFabricClusterConfigurationUpgrade -ClusterConfigPath <Path to Configuration File>

```

### <a name="cluster-certificate-config-upgrade"></a>Küme sertifikası yapılandırma yükseltmesi  
Bir küme sertifikası, küme düğümleri arasında kimlik doğrulaması için kullanılır. Küme düğümler arasında iletişim hatası engellediği için sertifika geçişi ile çok dikkatli gerçekleştirilmelidir.

Teknik olarak, dört seçenekten desteklenir:  

* Tek sertifika yükseltme: yükseltme yolu (birincil) -> sertifika B (birincil) sertifikasıdır sertifika C (birincil) -> ->....

* Double sertifika yükseltme: (birincil) -> sertifika sertifika (birincil) ve B (ikincil) -> sertifika B (birincil) yükseltme yolu olan sertifika B (birincil) -> ve C (ikincil) sertifika C (birincil) -> ->....

* Sertifika türü yükseltme: sertifika parmak izi tabanlı yapılandırma <> – CommonName tabanlı sertifika yapılandırması. Örneğin, sertifika parmak izi (birincil) ve parmak izi B (ikincil) -> sertifika CommonName C.

* Sertifika verenin parmak izi yükseltme: yükseltme yoludur sertifika CN = A, IssuerThumbprint IT1 = (birincil) sertifika CN -> = A, IssuerThumbprint IT1, IT2 = (birincil) sertifika CN -> = A, IssuerThumbprint IT2 = (birincil).


## <a name="next-steps"></a>Sonraki adımlar
* Bazı özelleştirmeyi öğrenin [Service Fabric küme ayarlarını](service-fabric-cluster-fabric-settings.md).
* Bilgi edinmek için nasıl [kümenizin ölçeğini daraltma ve genişletme](service-fabric-cluster-scale-up-down.md).
* Hakkında bilgi edinin [uygulama yükseltmeleri](service-fabric-application-upgrade.md).

<!--Image references-->
[getfabversions]: ./media/service-fabric-cluster-upgrade-windows-server/getfabversions.PNG
