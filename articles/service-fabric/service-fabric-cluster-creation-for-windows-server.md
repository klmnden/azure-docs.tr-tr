---
title: "Tek başına Azure Service Fabric kümesi oluştur | Microsoft Docs"
description: "Bir Azure Service Fabric kümesi oluşturmayı herhangi bir makinede (fiziksel veya sanal) şirket içi olup olmadığını veya tüm bulut Windows Server'ı çalıştıran."
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: 
ms.assetid: 31349169-de19-4be6-8742-ca20ac41eb9e
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 8/10/2017
ms.author: chackdan;maburlik;dekapur
ms.openlocfilehash: 6aa2905a97ec6b8c125f2ab9572a8e40bf525b27
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/18/2017
---
# <a name="create-a-standalone-cluster-running-on-windows-server"></a>Windows Server çalıştıran tek başına kümesi oluşturma
Azure Service Fabric, tüm sanal makineler veya Windows Server çalıştıran bilgisayarlarda Service Fabric kümeleri oluşturmak için kullanabilirsiniz. Yani, dağıtmak ve Service Fabric uygulamaları birbirine bağlı bir Windows Server bilgisayarlar kümesi içeren herhangi bir ortamda çalıştırılabilir, şirket içinde veya herhangi bir bulut sağlayıcısına sahip olabilir. Service Fabric tek başına Windows Server paket adlı Service Fabric kümeleri oluşturmak için Kurulum paketini sağlar.

Bu makalede, tek başına Service Fabric kümesi oluşturma adımları açıklanmaktadır.

> [!NOTE]
> Bu tek başına Windows Server paketi ticari olarak kullanılabilir ve üretim dağıtımları için kullanılabilir. Bu paket, "Önizleme"aşamasında olan yeni Service Fabric özellikler içerebilir. Ekranı aşağı kaydırarak "[Önizleme bu paketinde bulunan özellikler](#previewfeatures_anchor)." Önizleme özellikleri bölümüne listesi. Yapabilecekleriniz [EULA'yı bir kopyasını indirin](http://go.microsoft.com/fwlink/?LinkID=733084) şimdi.
> 
> 

<a id="getsupport"></a>

## <a name="get-support-for-the-service-fabric-for-windows-server-package"></a>Windows Server için Service Fabric paketi için destek alma
* Windows Server için Service Fabric tek başına paketi hakkında topluluğa sor [Azure Service Fabric Forumu](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=AzureServiceFabric?).
* İçin bilet [Service Fabric profesyonel desteği](http://support.microsoft.com/oas/default.aspx?prid=16146).  Microsoft Profesyonel desteği hakkında daha fazla bilgi [burada](https://support.microsoft.com/en-us/gp/offerprophone?wa=wsignin1.0).
* Ayrıca destek bu paket için bir parçası olarak alabilirsiniz [Microsoft Premier Destek](https://support.microsoft.com/en-us/premier).
* Daha fazla ayrıntı için lütfen bkz. [Azure Service Fabric destek seçenekleri](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-support).
* Destek amacıyla günlükleri toplamak için çalıştırın [Service Fabric tek başına günlük Toplayıcı](service-fabric-cluster-standalone-package-contents.md).

<a id="downloadpackage"></a>

## <a name="download-the-service-fabric-for-windows-server-package"></a>Windows Server için Service Fabric paketi yükle
Küme oluşturmak için Windows Server için Service Fabric paketi kullanın (Windows Server 2012 R2 ve daha yeni) burada bulundu: <br>
[Bağlantı - Service Fabric tek başına paketi - Windows Server'ı karşıdan yükle](http://go.microsoft.com/fwlink/?LinkId=730690)

Paket içeriğine ayrıntılarını bulun [burada](service-fabric-cluster-standalone-package-contents.md).

Service Fabric çalışma zamanı paketi küme oluşturma sırasında otomatik olarak yüklenir. Lütfen internet'e bağlı bir makineden dağıtma, bant dışı çalışma zamanı paketini buradan indirin: <br>
[Bağlantı - Service Fabric çalışma zamanı - Windows Server'ı karşıdan yükle](https://go.microsoft.com/fwlink/?linkid=839354)

Tek başına küme yapılandırma örnekleri adresindeki bulun: <br>
[Tek başına küme yapılandırma örnekleri](https://github.com/Azure-Samples/service-fabric-dotnet-standalone-cluster-configuration/tree/master/Samples)

<a id="createcluster"></a>

## <a name="create-the-cluster"></a>Kümeyi oluşturma
Service Fabric dağıtılabilir bir makine geliştirme kümeye kullanarak *ClusterConfig.Unsecure.DevCluster.json* dahil dosya [örnekleri](https://github.com/Azure-Samples/service-fabric-dotnet-standalone-cluster-configuration/tree/master/Samples).

Makinenize tek başına paketini açın, yerel makineye örnek yapılandırma dosyasını kopyalayın ve ardından Çalıştır *CreateServiceFabricCluster.ps1* tek başına paket klasörüne bir yönetici PowerShell oturumu aracılığıyla betikten :
### <a name="step-1a-create-an-unsecured-local-development-cluster"></a>Adım 1A: Güvenli olmayan yerel geliştirme kümesi oluşturun
```powershell
.\CreateServiceFabricCluster.ps1 -ClusterConfigFilePath .\ClusterConfig.Unsecure.DevCluster.json -AcceptEULA
```

Ortam Kurulumu kısmına bakın [planlama ve küme dağıtımınızı hazırlama](service-fabric-cluster-standalone-deployment-preparation.md) ayrıntıları sorun giderme.

Çalışan geliştirme senaryolarını tamamlandı verirseniz, Service Fabric kümesi makineden bölümünde açıklanan adımları izleyerek başvurarak kaldırabilirsiniz "[bir kümesini kaldırmak](#removecluster_anchor)". 

### <a name="step-1b-create-a-multi-machine-cluster"></a>Adım 1B: çoklu makine bir küme oluşturun
Planlama aracılığıyla ilerlemiş ve hazırlık adımları ayrıntılı adresindeki sonra aşağıdaki bağlantıda, küme yapılandırma dosyanızı kullanarak, üretim kümesi oluşturmak için hazır. <br>
[Planlama ve küme dağıtımınızı hazırlama](service-fabric-cluster-standalone-deployment-preparation.md)

1. Yapılandırma dosyasını çalıştırarak yazdığınız doğrulamak *TestConfiguration.ps1* tek başına paket klasörüne betikten:  

    ```powershell
    .\TestConfiguration.ps1 -ClusterConfigFilePath .\ClusterConfig.json
    ```

    Bir çıktı görmeniz gerekir aşağıdaki gibi. Alt alanın "Geçirilen" döndürülür "True" olarak, sağlamlık denetimleri geçtikten ve küme görünüyorsa dağıtılabilir için giriş yapılandırmasına bağlı olarak.

    ```
    Trace folder already exists. Traces will be written to existing trace folder: C:\temp\Microsoft.Azure.ServiceFabric.WindowsServer\DeploymentTraces
    Running Best Practices Analyzer...
    Best Practices Analyzer completed successfully.
    
    LocalAdminPrivilege        : True
    IsJsonValid                : True
    IsCabValid                 : True
    RequiredPortsOpen          : True
    RemoteRegistryAvailable    : True
    FirewallAvailable          : True
    RpcCheckPassed             : True
    NoConflictingInstallations : True
    FabricInstallable          : True
    Passed                     : True
    ```

2. Küme oluşturma: çalıştırmak *CreateServiceFabricCluster.ps1* Service Fabric kümesi yapılandırmasındaki her makine üzerinden dağıtmak için komut dosyası. 
    ```powershell
    .\CreateServiceFabricCluster.ps1 -ClusterConfigFilePath .\ClusterConfig.json -AcceptEULA
    ```

> [!NOTE]
> VM/CreateServiceFabricCluster.ps1 PowerShell Betiği çalıştırıldığı makineye dağıtım izlemeleri yazılır. Bunlar, komut dosyası çalıştırıldığı dizinde göre alt DeploymentTraces, bulunabilir. Service Fabric bir makineye doğru şekilde dağıtıldı olmadığını görmek için FabricDataRoot dizininde (göre varsayılan c:\ProgramData\SF) küme yapılandırma dosyasındaki FabricSettings bölümünde ayrıntılı olarak yüklenen dosyalar bulun. De FabricHost.exe ve Fabric.exe işlemleri Görev Yöneticisi'nde çalışan görülebilir.
> 
> 

### <a name="step-1c-create-an-offline-internet-disconnected-cluster"></a>Adım 1C: Çevrimdışı (internet kesilmiş) küme oluşturma
Service Fabric çalışma zamanı paketi küme oluşturma sırasında otomatik olarak yüklenir. İnternet'e bağlı olmayan makinelere bir küme dağıtımı sırasında Service Fabric çalışma zamanı paketi ayrı olarak indirebilir ve küme oluşturma sırasında bunu yolunu belirtmeniz gerekir.
Çalışma zamanı paketi, internet'e bağlı başka bir makineden ayrı ayrı, indirilebilir [karşıdan yükleme bağlantısı - Service Fabric çalışma zamanı - Windows Server](https://go.microsoft.com/fwlink/?linkid=839354). Burada, çevrimdışı kümeden dağıtıyorsanız ve çalıştırarak küme oluşturma çalışma zamanı paketi kopyalamak `CreateServiceFabricCluster.ps1` ile `-FabricRuntimePackagePath` dahil, aşağıda gösterildiği gibi parametre: 

```powershell
.\CreateServiceFabricCluster.ps1 -ClusterConfigFilePath .\ClusterConfig.json -FabricRuntimePackagePath .\MicrosoftAzureServiceFabric.cab
```
Burada `.\ClusterConfig.json` ve `.\MicrosoftAzureServiceFabric.cab` küme yapılandırmasını ve çalışma zamanı .cab dosyası için sırasıyla yollardır.


### <a name="step-2-connect-to-the-cluster"></a>2. adım: kümeye bağlanın
Güvenli bir kümeye bağlanmak için bkz: [Service fabric güvenli kümeye bağlanma](service-fabric-connect-to-secure-cluster.md).

Güvenli olmayan bir kümeye bağlanmak için aşağıdaki PowerShell komutunu çalıştırın:

```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint <*IPAddressofaMachine*>:<Client connection end point port>
```
Örnek:
```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint 192.13.123.2345:19000
```
### <a name="step-3-bring-up-service-fabric-explorer"></a>3. adım: Service Fabric Explorer Getir
Service Fabric Explorer ya da doğrudan http://localhost:19080/Explorer/index.html veya uzaktan http://< makinelerden biri ile kümeye bağlanabilir artık*IPAddressofaMachine*>: 19080/Explorer / index.HTML.

## <a name="add-and-remove-nodes"></a>Ekleme ve düğümlerini Kaldır
Ekleyebilir veya iş gereksinimleriniz değiştikçe, tek başına Service Fabric kümesi düğümleri kaldırın. Bkz: [ekleme veya kaldırma Service Fabric tek başına küme düğümlerine](service-fabric-cluster-windows-server-add-remove-nodes.md) ayrıntılı adımlar için.

<a id="removecluster" name="removecluster_anchor"></a>
## <a name="remove-a-cluster"></a>Bir küme Kaldır
Bir kümeyi kaldırmak için paket klasöründen *RemoveServiceFabricCluster.ps1* PowerShell betiğini çalıştırın ve yolu JSON yapılandırma dosyasına geçirin. İsteğe bağlı olarak silme işleminin günlüğü için bir konum belirtebilirsiniz.

Bu komut dosyası, küme yapılandırma dosyasındaki düğümler olarak listelenen tüm makineler yönetici erişimi olan herhangi bir makinede çalıştırılabilir. Bu komut dosyasını çalıştırmak makine kümesinin parçası olması gerekmez.

```
# Removes Service Fabric from each machine in the configuration
.\RemoveServiceFabricCluster.ps1 -ClusterConfigFilePath .\ClusterConfig.json -Force
```

```
# Removes Service Fabric from the current machine
.\CleanFabric.ps1
```

<a id="telemetry"></a>

## <a name="telemetry-data-collected-and-how-to-opt-out-of-it"></a>Toplanan telemetri verileri ve onu dışında kabul etme
Varsayılan olarak, ürün telemetri ürünü geliştirmek için Service Fabric kullanımı hakkında bilgi toplar. Bağlantı kurulumunun bir parçası çalıştıran en iyi yöntem Çözümleyicisi kontrol [https://vortex.data.microsoft.com/collect/v1](https://vortex.data.microsoft.com/collect/v1). Erişilebilir durumda değilse, telemetri dışında tercih sürece kurulum başarısız olur.

1. Aşağıdaki veriler karşıya yüklemek telemetri ardışık düzen çalışır [https://vortex.data.microsoft.com/collect/v1](https://vortex.data.microsoft.com/collect/v1) günde bir kez. En yüksek çaba karşıya yükleme ve küme işlevselliğini üzerinde hiçbir etkisi olmaz. Telemetriyi yalnızca yük devretme çalışan düğümü gönderilen birincil Yöneticisi. Başka bir düğüm telemetri gönderir.
2. Telemetri aşağıdakilerden oluşur:

* Hizmetlerin sayısı
* ServiceTypes sayısı
* Uygulama sayısı
* ApplicationUpgrades sayısı
* Sistemdeki yük devretme birimi sayısı
* Inbuildfailoverunits sayısı
* UnhealthyFailoverUnits sayısı
* Çoğaltmaların sayısı
* InBuildReplicas sayısı
* StandByReplicas sayısı
* OfflineReplicas sayısı
* CommonQueueLength
* QueryQueueLength
* FailoverUnitQueueLength
* CommitQueueLength
* Düğüm sayısı
* IsContextComplete: True/False
* ClusterId: Her küme için rastgele oluşturulmuş bir GUID budur
* ServiceFabricVersion
* Sanal makine ya da telemetri karşıya Makine IP adresi

Telemetri devre dışı bırakmak için aşağıdakileri ekleyin *özellikleri* yapılandırmada, küme: *enableTelemetry: yanlış*.

<a id="previewfeatures" name="previewfeatures_anchor"></a>

## <a name="preview-features-included-in-this-package"></a>Bu pakete dahil önizleme özellikleri
yok.


> [!NOTE]
> Yeni başlangıç [Windows Server için tek başına küme GA sürümü (sürüm 5.3.204.x)](https://azure.microsoft.com/blog/azure-service-fabric-for-windows-server-now-ga/), kümenizi el ile veya otomatik olarak gelecek sürümlerde için yükseltebilirsiniz. Başvurmak [bir tek başına Service Fabric kümesi sürümüne yükseltmenizi](service-fabric-cluster-upgrade-windows-server.md) Ayrıntılar için belge.
> 
> 

## <a name="next-steps"></a>Sonraki adımlar
* [Dağıtma ve PowerShell kullanarak uygulamaları kaldırma](service-fabric-deploy-remove-applications.md)
* [Tek başına Windows kümesi için yapılandırma ayarları](service-fabric-cluster-manifest.md)
* [Bir tek başına Service Fabric kümesi düğümlerine Ekle Kaldır](service-fabric-cluster-windows-server-add-remove-nodes.md)
* [Bir tek başına Service Fabric kümesi sürümüne yükseltme](service-fabric-cluster-upgrade-windows-server.md)
* [Azure VM'ler Windows çalıştıran bir tek başına Service Fabric kümesi oluştur](service-fabric-cluster-creation-with-windows-azure-vms.md)
* [Windows güvenliği kullanarak Windows'u bir tek başına kümede güvenli](service-fabric-windows-cluster-windows-security.md)
* [Tek başına küme X509 kullanarak Windows güvenli sertifikaları](service-fabric-windows-cluster-x509-security.md)

<!--Image references-->
[Trusted Zone]: ./media/service-fabric-cluster-creation-for-windows-server/TrustedZone.png
