---
title: Tek başına Azure Service Fabric kümesi oluşturma | Microsoft Docs
description: Bir Azure Service Fabric kümesi oluşturma olsun şirket içi veya buluttaki herhangi bir makinede (fiziksel veya sanal) Windows Server çalıştıran.
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: chackdan
editor: ''
ms.assetid: 31349169-de19-4be6-8742-ca20ac41eb9e
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 2/21/2019
ms.author: dekapur
ms.openlocfilehash: 3e9e3afd5172783c6b5ed8e6342ce9927353d006
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60386860"
---
# <a name="create-a-standalone-cluster-running-on-windows-server"></a>Windows Server üzerinde çalışan tek başına küme oluşturma
Azure Service Fabric, tüm sanal makineleri veya Windows Server çalıştıran bilgisayarlarda, Service Fabric kümeleri oluşturmak için kullanabilirsiniz. Bu, dağıtmak ve Service Fabric uygulamaları Windows Server bilgisayarları birbirine bağlı bir dizi içeren herhangi bir ortamında çalıştırmak, şirket içinde veya tüm bulut sağlayıcıları ile de gösterir. Service Fabric, tek başına Windows Server paketi adlı Service Fabric kümeleri oluşturmak için bir kurulum paketi sağlar.

Bu makalede bir Service Fabric tek başına küme oluşturma adımlarında size kılavuzluk eder.

> [!NOTE]
> Bu tek başına Windows Server paketi ticari olarak satışta olduğu ve üretim dağıtımları için kullanılabilir. Bu paket "Önizleme"aşamasında olan yeni Service Fabric özellikler içerebilir. Ekranı aşağı kaydırarak "[Önizleme özellikleri bu pakete dahil](#previewfeatures_anchor)." Önizleme özellikleri bölümüne listesi. Yapabilecekleriniz [EULA'yı bir kopyasını indirin](https://go.microsoft.com/fwlink/?LinkID=733084) şimdi.
> 
> 

<a id="getsupport"></a>

## <a name="get-support-for-the-service-fabric-for-windows-server-package"></a>Windows Server için Service Fabric paketi için destek alma
* Windows Server için Service Fabric tek başına paketin hakkında topluluğa sorun [Azure Service Fabric Forumu](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=AzureServiceFabric?).
* İçin bilet [Service Fabric için profesyonel destek](https://support.microsoft.com/oas/default.aspx?prid=16146).  Microsoft profesyonel destek hakkında daha fazla bilgi [burada](https://support.microsoft.com/en-us/gp/offerprophone?wa=wsignin1.0).
* Ayrıca destek bu paket için bir parçası olarak alabilirsiniz [Microsoft Premier desteği](https://support.microsoft.com/en-us/premier).
* Daha fazla ayrıntı için lütfen bkz [Azure Service Fabric destek seçenekleri](https://docs.microsoft.com/azure/service-fabric/service-fabric-support).
* Destek amacıyla günlükleri toplamak için çalıştırma [Service Fabric tek başına günlük Toplayıcı](service-fabric-cluster-standalone-package-contents.md).

<a id="downloadpackage"></a>

## <a name="download-the-service-fabric-for-windows-server-package"></a>Windows Server paketi için Service Fabric indirme
Kümeyi oluşturmak için Windows Server için Service Fabric paket kullanın (Windows Server 2012 R2 ve üzeri sürümlerde) burada bulunabilir: <br>
[Bağlantı - Service Fabric tek başına paketin - Windows Server'ı indirin](https://go.microsoft.com/fwlink/?LinkId=730690)

Paket içeriğine ayarıntılarını bulun [burada](service-fabric-cluster-standalone-package-contents.md).

Service Fabric çalışma zamanı paketi küme oluşturulurken otomatik olarak indirilir. Lütfen internet'e bağlı olmayan bir makineden dağıtıyorsanız, bant dışı çalışma zamanı paketi buradan indirin: <br>
[Bağlantı - Service Fabric çalışma zamanı - Windows Server'ı indirin](https://go.microsoft.com/fwlink/?linkid=839354)

Tek başına küme yapılandırması örneği bulun: <br>
[Tek başına küme yapılandırma örnekleri](https://github.com/Azure-Samples/service-fabric-dotnet-standalone-cluster-configuration/tree/master/Samples)

<a id="createcluster"></a>

## <a name="create-the-cluster"></a>Kümeyi oluşturma
Kurulum paketiyle birlikte birkaç örnek küme yapılandırma dosyası yüklenir. Tek bir bilgisayarda çalışan korumasız ve üç düğümlü bir küme olan *ClusterConfig.Unsecure.DevCluster.json*, en basit küme yapılandırmasıdır.  Diğer yapılandırma dosyalarında X.509 sertifikalarıyla ya da Windows güvenliğiyle korunan tek veya çok makineli kümeler açıklanır.  Bu öğreticide varsayılan yapılandırma ayarlarından herhangi birini değiştirmeniz gerekmez, ancak yapılandırma dosyasını inceleyip ayarları tanıyın.  **Düğümler** bölümünde, kümedeki üç düğüm açıklanmaktadır: ad, IP adresi, [düğüm türü, hata etki alanı ve yükseltme etki alanı](service-fabric-cluster-manifest.md#nodes-on-the-cluster).  **Özellikler** bölümü kümenin [güvenlik, güvenilirlik düzeyi, tanılama koleksiyonu ve düğüm türlerini](service-fabric-cluster-manifest.md#cluster-properties) tanımlar.

Bu makalede oluşturduğunuz küme güvenli değildir.  Herkes anonim olarak bağlanıp yönetim işlemleri gerçekleştirebileceğinden, üretim kümeleri her zaman X.509 sertifikaları veya Windows güvenliği kullanılarak güvenli hale getirilmelidir.  Güvenlik yalnızca küme oluşturma sırasında yapılandırılır ve küme oluşturulduktan sonra güvenliği etkinleştirmek mümkün değildir. Yapılandırma dosyası etkinleştirme güncelleştirme [sertifika güvenlik](service-fabric-windows-cluster-x509-security.md) veya [Windows Güvenlik](service-fabric-windows-cluster-windows-security.md). Service Fabric küme güvenliği hakkında daha fazla bilgi edinmek için [Küme güvenliğini sağlama](service-fabric-cluster-security.md) makalesini okuyun.

### <a name="step-1-create-the-cluster"></a>1. Adım: Kümeyi oluşturma

#### <a name="scenario-a-create-an-unsecured-local-development-cluster"></a>Senaryo A: Güvenli olmayan yerel geliştirme kümesi oluşturun
Service Fabric dağıtılabilir bir makine geliştirme kümeye kullanarak *ClusterConfig.Unsecure.DevCluster.json* dahil dosya [örnekleri](https://github.com/Azure-Samples/service-fabric-dotnet-standalone-cluster-configuration/tree/master/Samples).

Tek başına paketini makinenize açın, örnek yapılandırma dosyası yerel makineye kopyalayın ve ardından çalıştırma *CreateServiceFabricCluster.ps1* tek başına paket klasöründen bir yönetici PowerShell oturumu aracılığıyla betiği .

```powershell
.\CreateServiceFabricCluster.ps1 -ClusterConfigFilePath .\ClusterConfig.Unsecure.DevCluster.json -AcceptEULA
```

Ortam Kurulumu kısmına bakın [planlama ve küme dağıtımınızı hazırlama](service-fabric-cluster-standalone-deployment-preparation.md) için sorun giderme ayrıntıları.

Çalışan geliştirme senaryolarını tamamlandıysa, Service Fabric kümesine makineden bölümdeki adımları başvurarak kaldırabilirsiniz "[küme kaldırma](#removecluster_anchor)". 

#### <a name="scenario-b-create-a-multi-machine-cluster"></a>Senaryo B: Çok makineli bir küme oluşturma
Planlama aracılığıyla gitti ve hazırlık adımları ayrıntılı sonra [planlama ve küme dağıtımınızı hazırlama](service-fabric-cluster-standalone-deployment-preparation.md), küme yapılandırma dosyanızı kullanarak üretim kümenizi oluşturmak hazır olursunuz.

Kümeyi dağıtan ve yapılandıran küme yöneticisinin bilgisayarda yönetici ayrıcalıklarına sahip olması gerekir. Service Fabric’i bir etki alanı denetleyicisine yükleyemezsiniz.

1. Tek başına paketteki *TestConfiguration.ps1* betiği, bir kümenin belirli bir ortamda dağıtılıp dağıtılamayacağını doğrulamak için bir en iyi yöntem çözümleyicisi olarak kullanılır. [Dağıtım hazırlığı](service-fabric-cluster-standalone-deployment-preparation.md) belgesinde, ön koşullar ve ortam gereksinimleri listelenir. Geliştirme kümesini oluşturabileceğinizi doğrulamak için betiği çalıştırın:  

    ```powershell
    .\TestConfiguration.ps1 -ClusterConfigFilePath .\ClusterConfig.json
    ```

    Aşağıdakine benzer bir çıktı görmeniz gerekir. Alt alan "Başarılı" döndürülür "True" olarak, sağlamlık denetimleri geçtikten ve küme görünüyorsa dağıtılabilir olması için giriş yapılandırmasına bağlı olarak.

    ```powershell
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

2. Kümeyi oluşturun:  Çalıştırma *CreateServiceFabricCluster.ps1* yapılandırmasındaki her makine üzerinde Service Fabric kümesi dağıtmayı betiği. 
    ```powershell
    .\CreateServiceFabricCluster.ps1 -ClusterConfigFilePath .\ClusterConfig.json -AcceptEULA
    ```

> [!NOTE]
> Dağıtım izlemeleri, üzerinde CreateServiceFabricCluster.ps1 PowerShell betiğini çalıştırabileceğiniz VM/makineye yazılır. Bunlar, betiğin çalıştırıldığı dizine göre DeploymentTraces alt klasöründe bulunabilir. Service Fabric’in bir makineye doğru şekilde dağıtılıp dağıtılmadığını görmek için, FabricSettings bölümündeki küme yapılandırma dosyasında (varsayılan olarak c:\ProgramData\SF) ayrıntılı olarak açıklandığı gibi FabricDataRoot dizinindeki yüklü dosyaları bulun. Ayrıca, FabricHost.exe ve Fabric.exe işlemlerinin Görev Yöneticisi’nde çalıştığı görülebilir.
> 
> 

#### <a name="scenario-c-create-an-offline-internet-disconnected-cluster"></a>C: senaryosu Çevrimdışı (Internet bağlantısı) kümesi oluşturma
Service Fabric çalışma zamanı paketi küme oluşturma sırasında otomatik olarak indirilir. İnternet'e bağlı olmayan makinelere bir küme dağıtılırken, Service Fabric çalışma zamanı paketi ayrı olarak indirebilir ve küme oluşturma sırasında yolu sağlamak gerekir.
Çalışma zamanı paket, internet'e bağlı başka bir makineden ayrı olarak indirilebilir [indirme bağlantısı - Service Fabric çalışma zamanı - Windows Server](https://go.microsoft.com/fwlink/?linkid=839354). Burada, çevrimdışı küme dağıtma ve çalıştırarak küme oluşturun çalışma zamanı paketini kopyalayın `CreateServiceFabricCluster.ps1` ile `-FabricRuntimePackagePath` dahil, bu örnekte gösterildiği gibi parametre: 

```powershell
.\CreateServiceFabricCluster.ps1 -ClusterConfigFilePath .\ClusterConfig.json -FabricRuntimePackagePath .\MicrosoftAzureServiceFabric.cab
```

*.\ClusterConfig.JSON* ve *.\MicrosoftAzureServiceFabric.cab* küme yapılandırmasını ve çalışma zamanı .cab dosyası için sırasıyla yollardır.

### <a name="step-2-connect-to-the-cluster"></a>2. Adım: Kümeye bağlanma
Kümenin çalışır ve kullanılabilir olup olmadığını doğrulamak için kümeye bağlanın. ServiceFabric PowerShell modülü çalışma zamanıyla birlikte yüklenir.  Küme düğümlerinin birinden veya Service Fabric çalışma zamanı ile uzak bir bilgisayardan, kümeye bağlanabilirsiniz.  [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster?view=azureservicefabricps) cmdlet’i, kümeyle bir bağlantı kurar.

Güvenli olmayan bir kümeye bağlanmak için aşağıdaki PowerShell komutunu çalıştırın:

```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint <*IPAddressofaMachine*>:<Client connection end point port>
```

Örneğin:
```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint 192.13.123.2345:19000
```

Bir kümeye bağlanmayla ilgili diğer örnekler için bkz. [Güvenli bir kümeye bağlanma](service-fabric-connect-to-secure-cluster.md). Kümeye bağlandıktan sonra, kümedeki düğümlerin listesini ve her bir düğümün durum bilgilerini görüntülemek için [Get-ServiceFabricNode](/powershell/module/servicefabric/get-servicefabricnode?view=azureservicefabricps) cmdlet’ini kullanın. Her düğüm için **HealthState** değerinin *OK* olması gerekir.

```powershell
PS C:\temp\Microsoft.Azure.ServiceFabric.WindowsServer> Get-ServiceFabricNode |Format-Table

NodeDeactivationInfo NodeName IpAddressOrFQDN NodeType  CodeVersion  ConfigVersion NodeStatus NodeUpTime NodeDownTime HealthState
-------------------- -------- --------------- --------  -----------  ------------- ---------- ---------- ------------ -----------
                     vm2      localhost       NodeType2 5.6.220.9494 0                     Up 00:03:38   00:00:00              OK
                     vm1      localhost       NodeType1 5.6.220.9494 0                     Up 00:03:38   00:00:00              OK
                     vm0      localhost       NodeType0 5.6.220.9494 0                     Up 00:02:43   00:00:00              OK
```

### <a name="step-3-visualize-the-cluster-using-service-fabric-explorer"></a>3. Adım: Service Fabric Explorer’ı kullanarak kümeyi görselleştirme
[Service Fabric Explorer](service-fabric-visualizing-your-cluster.md), kümenizi görselleştirmek ve uygulamaları yönetmek için iyi bir araçtır.  Service Fabric Explorer giderek bir tarayıcı kullanarak erişim kümede çalışan bir hizmet olan [ http://localhost:19080/Explorer ](http://localhost:19080/Explorer).

Küme panosu, kümenize uygulama ve düğüm durumunun özetini de içeren bir genel bakış sağlar. Düğüm görünümü, kümenin fiziksel düzenini gösterir. Belirli bir düğümde, hangi uygulamalara kod dağıtıldığını denetleyebilirsiniz.

![Service Fabric Explorer][service-fabric-explorer]

## <a name="add-and-remove-nodes"></a>Düğüm ekleme ve kaldırma
İş gereksinimleriniz değiştikçe tek başına Service Fabric kümenize düğüm ekleyebilir veya kaldırabilirsiniz. Ayrıntılı adımlar için bkz. [Service Fabric tek başına kümesine düğüm ekleme veya kaldırma](service-fabric-cluster-windows-server-add-remove-nodes.md).

<a id="removecluster" name="removecluster_anchor"></a>
## <a name="remove-a-cluster"></a>Bir kümeyi kaldırma
Bir kümeyi kaldırmak için paket klasöründen *RemoveServiceFabricCluster.ps1* PowerShell betiğini çalıştırın ve yolu JSON yapılandırma dosyasına geçirin. İsteğe bağlı olarak silme işleminin günlüğü için bir konum belirtebilirsiniz.

Bu betik, küme yapılandırma dosyasında düğümleri olarak listelenen tüm makineler için yönetici erişimi olan herhangi bir makinede çalıştırılabilir. Bu betiğin çalıştırıldığı makine kümesinin parçası olacak gerekmez.

```powershell
# Removes Service Fabric from each machine in the configuration
.\RemoveServiceFabricCluster.ps1 -ClusterConfigFilePath .\ClusterConfig.json -Force
```

```powershell
# Removes Service Fabric from the current machine
.\CleanFabric.ps1
```

<a id="telemetry"></a>

## <a name="telemetry-data-collected-and-how-to-opt-out-of-it"></a>Toplanan telemetri verilerini ve dışında nasıl
Varsayılan olarak, ürün ürünü geliştirmek için Service Fabric kullanım telemetri toplar. Bağlantı kurulumunun bir parçası çalıştırılan en iyi yöntem Çözümleyicisi denetler [ https://vortex.data.microsoft.com/collect/v1 ](https://vortex.data.microsoft.com/collect/v1). Telemetri dışında iyileştirilmiş sürece erişilebilir durumda değilse, kurulum başarısız olur.

1. Telemetri ardışık düzen için aşağıdaki Veril dener [ https://vortex.data.microsoft.com/collect/v1 ](https://vortex.data.microsoft.com/collect/v1) her gün. Bu en yüksek çaba karşıya yükleme ve küme işlevselliğini etkilemez. Telemetriyi yalnızca yük devretme çalışan düğümü gönderilen birincil Yöneticisi. Başka bir düğüm telemetri gönderin.
2. Telemetri aşağıdakilerden oluşur:

* Hizmet sayısı
* ServiceTypes sayısı
* Uygulama Sayısı
* ApplicationUpgrades sayısı
* FailoverUnits sayısı
* Inbuildfailoverunit sayısı
* UnhealthyFailoverUnits sayısı
* Çoğaltma Sayısı
* InBuildReplicas sayısı
* StandByReplicas sayısı
* OfflineReplicas sayısı
* CommonQueueLength
* QueryQueueLength
* FailoverUnitQueueLength
* CommitQueueLength
* Düğüm sayısı
* IsContextComplete: True/False
* Lclusterıd: Her küme için rastgele oluşturulmuş bir GUID budur.
* ServiceFabricVersion
* Sanal makine veya makinenin içinden telemetriyi karşıya IP adresi

Telemetri devre dışı bırakmak için aşağıdaki ekleme *özellikleri* , küme yapılandırmasını: *enableTelemetry: false*.

<a id="previewfeatures" name="previewfeatures_anchor"></a>

## <a name="preview-features-included-in-this-package"></a>Bu pakete dahil önizleme özellikleri
Yok.


> [!NOTE]
> Yeni başlangıç [Windows Server için tek başına küme GA sürümünü (sürüm 5.3.204.x)](https://azure.microsoft.com/blog/azure-service-fabric-for-windows-server-now-ga/), kümenizi el ile veya otomatik olarak sonraki sürümlere yükseltebilirsiniz. Başvurmak [bir tek başına Service Fabric kümesi sürümünden yükseltme](service-fabric-cluster-upgrade-windows-server.md) Ayrıntılar için belge.
> 
> 

## <a name="next-steps"></a>Sonraki adımlar
* [Dağıtma ve PowerShell kullanarak uygulamaları kaldırma](service-fabric-deploy-remove-applications.md)
* [Tek başına Windows Küme yapılandırma ayarları](service-fabric-cluster-manifest.md)
* [Tek başına Service Fabric küme düğümleri ekleyebilir veya kaldırabilirsiniz](service-fabric-cluster-windows-server-add-remove-nodes.md)
* [Tek başına Service Fabric küme sürümünü yükseltme](service-fabric-cluster-upgrade-windows-server.md)
* [Windows çalıştıran Azure Vm'leriyle bir tek başına Service Fabric kümesi oluşturma](service-fabric-cluster-creation-with-windows-azure-vms.md)
* [Windows güvenliğini kullanarak Windows üzerinde tek başına küme güvenliğini sağlama](service-fabric-windows-cluster-windows-security.md)
* [X509 kullanarak Windows üzerinde tek başına küme güvenliğini sağlama sertifikaları](service-fabric-windows-cluster-x509-security.md)

<!--Image references-->
[Trusted Zone]: ./media/service-fabric-cluster-creation-for-windows-server/TrustedZone.png
[service-fabric-explorer]: ./media/service-fabric-cluster-creation-for-windows-server/sfx.png