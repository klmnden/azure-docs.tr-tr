---
title: "Service Fabric, uygulama yaşam döngüsü | Microsoft Docs"
description: "Geliştirme, dağıtma, test, yükseltme, koruma ve Service Fabric uygulamaları kaldırma açıklar."
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: 
ms.assetid: 08837cca-5aa7-40da-b087-2b657224a097
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 1/19/2018
ms.author: ryanwi
ms.openlocfilehash: 923778e54a1ae5967d681751841c3a2b3fb45130
ms.sourcegitcommit: 817c3db817348ad088711494e97fc84c9b32f19d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/20/2018
---
# <a name="service-fabric-application-lifecycle"></a>Service Fabric uygulama yaşam döngüsü
Diğer platformlar ile Azure Service Fabric bir uygulamada genellikle aşağıdaki aşamaları geçtikçe: tasarım, geliştirme, test, dağıtım, yükseltme, Bakım ve kaldırma. Service Fabric, nihai yetkisini için bulut uygulamalarından, geliştirme aşamasından dağıtım, günlük yönetim ve Bakım tam uygulama yaşam döngüsü için birinci sınıf destek sağlar. Hizmet modeli bağımsız olarak uygulama yaşam döngüsü katılmak birçok farklı rol sağlar. Bu makalede API'ler ve Service Fabric uygulama yaşam döngüsü aşamaları boyunca farklı rolleri tarafından nasıl kullanıldıkları hakkında genel bir bakış sağlar.

[!INCLUDE [links to azure cli and service fabric cli](../../includes/service-fabric-sfctl.md)]

Aşağıdaki Microsoft Virtual Academy video, uygulama yaşam döngüsü açıklar:<center><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/building-microservices-applications-on-azure-service-fabric-16747?l=My3Ka56yC_6106218965">
<img src="./media/service-fabric-application-lifecycle/AppLifecycleVid.png" WIDTH="360" HEIGHT="244">
</a></center>

## <a name="service-model-roles"></a>Hizmet modeli rolleri
Hizmet modeli rolü şunlardır:

* **Hizmet Geliştirici**: başka bir amaçla ve aynı tür ya da farklı birçok uygulamada kullanılan modüler ve genel hizmetler geliştirir. Örneğin, bir sıra hizmeti gişe uygulaması (Yardım Masası) veya bir e-ticaret uygulaması (alışveriş sepeti) oluşturmak için kullanılabilir.
* **Uygulama geliştiricisi**: belirli özel gereksinimler ya da senaryoları karşılamak için hizmetler koleksiyonu tümleştirerek uygulamaları oluşturur. "Örneğin, bir e-ticaret Web sitesi JSON durum bilgisi olmayan ön uç hizmeti," tümleştirmek "Artırma durum bilgisi olan hizmeti" ve "Sırası durum bilgisi olan hizmeti" auctioning çözümü oluşturmak için.
* **Uygulama Yöneticisi**: kararları uygulama yapılandırması (yapılandırma şablonu parametreleri doldurma), dağıtım (kullanılabilir kaynaklar için bir eşleme) ve hizmet kalitesi sağlar. Örneğin, bir uygulama Yöneticisi (Amerika Birleşik Devletleri İngilizce) veya örneğin, Japonya için Japonca dil yerel uygulamanın karar verir. Farklı bir dağıtılmış uygulama farklı ayarlara sahip olabilir.
* **İşleç**: uygulama yapılandırmasına bağlı olarak uygulamalar ve uygulama Yöneticisi tarafından belirtilen gereksinimler dağıtır. Örneğin, bir işleç sağlar ve uygulamayı dağıtır ve Azure'da çalıştığını sağlar. İşleçler uygulama sağlık ve performans bilgilerini izlemek ve gerektiği gibi fiziksel altyapı sürdürün.

## <a name="develop"></a>Geliştirme
1. A *Hizmet Geliştirici* hizmetlerini kullanarak farklı türlerde geliştirir [Reliable Actors](service-fabric-reliable-actors-introduction.md) veya [Reliable Services](service-fabric-reliable-services-introduction.md) programlama modeli.
2. A *Hizmet Geliştirici* bildirimli olarak bir veya daha fazla kod, yapılandırma ve veri paketlerini oluşan bir hizmet bildirim dosyasında geliştirilen hizmet türleri açıklanmaktadır.
3. Bir *uygulama geliştiricisi* farklı hizmet türlerini kullanarak bir uygulama oluşturur.
4. Bir *uygulama geliştiricisi* bildirimli olarak uygulama bildiriminde uygulama türü başvuran bağlı Hizmetleri hizmet bildirimlerini uygun şekilde geçersiz kılma ve bağlı hizmetlerin farklı yapılandırma ve dağıtım ayarlarını kümesini parametreleştirme tarafından açıklanır.

Bkz: [Reliable Actors ile çalışmaya başlama](service-fabric-reliable-actors-get-started.md) ve [Reliable Services ile çalışmaya başlama](service-fabric-reliable-services-quick-start.md) örnekleri için.

## <a name="deploy"></a>Dağıtma
1. Bir *Uygulama Yöneticisi* Service Fabric kümesi için uygun parametreleri belirterek dağıtılması için belirli bir uygulama için uygulama türü belirlenmek **ApplicationType** uygulama bildiriminde öğesi.
2. Bir *işleci* kullanarak uygulama paketi küme görüntü deposuna karşıya yükleme [ **CopyApplicationPackage** yöntemi](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.applicationmanagementclient#System_Fabric_FabricClient_ApplicationManagementClient_CopyApplicationPackage_System_String_System_String_System_String_) veya [ **kopya ServiceFabricApplicationPackage** cmdlet](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps). Uygulama paketi, uygulama bildirimi ve hizmet paketleri koleksiyonunu içerir. Service Fabric uygulamaları bir Azure blob deposu ya da Service Fabric sistem hizmeti Image store depolanan uygulama paketinden dağıtır.
3. *İşleci* sonra hazırlar karşıya yüklenen uygulama paketini kullanarak hedef kümedeki uygulama türü [ **ProvisionApplicationAsync** yöntemi](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.applicationmanagementclient#System_Fabric_FabricClient_ApplicationManagementClient_ProvisionApplicationAsync_System_String_System_TimeSpan_System_Threading_CancellationToken_), [ **Register-ServiceFabricApplicationType** cmdlet](https://docs.microsoft.com/powershell/servicefabric/vlatest/register-servicefabricapplicationtype), veya [ **bir uygulama sağlama** REST işlemini](https://docs.microsoft.com/rest/api/servicefabric/provision-an-application).
4. Uygulama sağlama sonra bir *işleci* uygulama tarafından sağlanan parametreleri ile başlayan *Uygulama Yöneticisi* kullanarak [ **CreateApplicationAsync** yöntemi](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.applicationmanagementclient#System_Fabric_FabricClient_ApplicationManagementClient_CreateApplicationAsync_System_Fabric_Description_ApplicationDescription_System_TimeSpan_System_Threading_CancellationToken_), [ **yeni ServiceFabricApplication** cmdlet](https://docs.microsoft.com/powershell/servicefabric/vlatest/new-servicefabricapplication), veya [ **uygulama oluştur** REST işlemini](https://docs.microsoft.com/rest/api/servicefabric/create-an-application).
5. Uygulama dağıtıldıktan sonra bir *işleci* kullanan [ **CreateServiceAsync** yöntemi](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.servicemanagementclient#System_Fabric_FabricClient_ServiceManagementClient_CreateServiceAsync_System_Fabric_Description_ServiceDescription_System_TimeSpan_System_Threading_CancellationToken_), [ **yeni ServiceFabricService** cmdlet](https://docs.microsoft.com/powershell/servicefabric/vlatest/new-servicefabricservice), veya [ **hizmet oluşturma** REST işlemini](https://docs.microsoft.com/rest/api/servicefabric/create-a-service) kullanılabilir hizmet türlerine göre uygulama için yeni hizmet örnekleri oluşturmak için.
6. Uygulama, Service Fabric kümesi şimdi çalışıyor.

Bkz: [bir uygulamayı dağıtmak](service-fabric-deploy-remove-applications.md) örnekleri için.

## <a name="test"></a>Test etme
1. Yerel geliştirme küme ya da bir test kümesi dağıttıktan sonra bir *Hizmet Geliştirici* kullanarak yerleşik yük devretme testi senaryosu çalıştıran [ **FailoverTestScenarioParameters** ](https://docs.microsoft.com/dotnet/api/system.fabric.testability.scenario.failovertestscenarioparameters#System_Fabric_Testability_Scenario_FailoverTestScenarioParameters) ve [ **FailoverTestScenario** ](https://docs.microsoft.com/dotnet/api/system.fabric.testability.scenario.failovertestscenario#System_Fabric_Testability_Scenario_FailoverTestScenario) sınıfları veya [ **Invoke-ServiceFabricFailoverTestScenario** cmdlet](/powershell/module/servicefabric/invoke-servicefabricfailovertestscenario?view=azureservicefabricps). Yük devretme testi senaryosu önemli geçişler ve hala kullanılabilir ve çalışır olduğundan emin olmak için yük devretme ile belirtilen bir hizmeti çalışır.
2. *Hizmet Geliştirici* yerleşik chaos test senaryoyu kullanarak çalışır [ **ChaosTestScenarioParameters** ](https://docs.microsoft.com/dotnet/api/system.fabric.testability.scenario.chaostestscenarioparameters#System_Fabric_Testability_Scenario_ChaosTestScenarioParameters) ve [ **ChaosTestScenario** ](https://docs.microsoft.com/dotnet/api/system.fabric.testability.scenario.chaostestscenario#System_Fabric_Testability_Scenario_ChaosTestScenario) sınıfları veya [ **Invoke-ServiceFabricChaosTestScenario** cmdlet](/powershell/module/servicefabric/invoke-servicefabricchaostestscenario?view=azureservicefabricps). Chaos test senaryosu, kümeye birden çok düğüm, kod paketi ve çoğaltma hataları rastgele uygulanmasını.
3. *Hizmet Geliştirici* [hizmeti hizmeti iletişimi testleri](service-fabric-testability-scenarios-service-communication.md) küme geçici birincil çoğaltmalara taşıma sınama senaryolarını yazarak.

Bkz: [hata analizi hizmetine giriş](service-fabric-testability-overview.md) daha fazla bilgi için.

## <a name="upgrade"></a>Yükseltme
1. A *Hizmet Geliştirici* başlatılan uygulamanın bağlı Hizmetleri güncelleştirir ve/veya hataları düzeltir ve hizmeti bildirimini yeni bir sürümünü sağlar.
2. Bir *uygulama geliştiricisi* geçersiz kılar ve tutarlı Hizmetleri Yapılandırma ve dağıtım ayarlarının parameterizes ve uygulama bildirimi yeni bir sürümünü sağlar. Uygulama geliştiricisi daha sonra uygulamaya hizmet bildirimlerini yeni sürümlerini içerir ve güncelleştirilmiş uygulama paketi uygulama türünde yeni bir sürümünü sağlar.
3. Bir *Uygulama Yöneticisi* uygun parametreleri güncelleştirerek hedef uygulamaya uygulama türü yeni sürümünü içerir.
4. Bir *işleci* küme görüntü kullanarak deposu güncelleştirilmiş uygulama paketi yükler [ **CopyApplicationPackage** yöntemi](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.applicationmanagementclient#System_Fabric_FabricClient_ApplicationManagementClient_CopyApplicationPackage_System_String_System_String_System_String_) veya [ **kopya ServiceFabricApplicationPackage** cmdlet](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps). Uygulama paketi, uygulama bildirimi ve hizmet paketleri koleksiyonunu içerir.
5. Bir *işleci* kullanarak hedef kümedeki uygulamanın yeni sürümü hazırlar [ **ProvisionApplicationAsync** yöntemi](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.applicationmanagementclient#System_Fabric_FabricClient_ApplicationManagementClient_ProvisionApplicationAsync_System_String_System_TimeSpan_System_Threading_CancellationToken_), [ **Register-ServiceFabricApplicationType** cmdlet](https://docs.microsoft.com/powershell/servicefabric/vlatest/register-servicefabricapplicationtype), veya [ **bir uygulama sağlama** REST işlemini](https://docs.microsoft.com/rest/api/servicefabric/provision-an-application).
6. Bir *işleci* kullanarak yeni sürümü için hedef uygulama yükseltmeleri [ **UpgradeApplicationAsync** yöntemi](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.applicationmanagementclient#System_Fabric_FabricClient_ApplicationManagementClient_UpgradeApplicationAsync_System_Fabric_Description_ApplicationUpgradeDescription_System_TimeSpan_System_Threading_CancellationToken_), [ **başlangıç ServiceFabricApplicationUpgrade** cmdlet](https://docs.microsoft.com/powershell/servicefabric/vlatest/start-servicefabricapplicationupgrade), veya [ **uygulama yükseltme** REST işlemini](https://docs.microsoft.com/rest/api/servicefabric/upgrade-an-application).
7. Bir *işleci* yükseltme kullanarak ilerlemesini denetler [ **GetApplicationUpgradeProgressAsync** yöntemi](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.applicationmanagementclient#System_Fabric_FabricClient_ApplicationManagementClient_GetApplicationUpgradeProgressAsync_System_Uri_System_TimeSpan_System_Threading_CancellationToken_), [ **Get-ServiceFabricApplicationUpgrade** cmdlet](https://docs.microsoft.com/powershell/servicefabric/vlatest/get-servicefabricapplicationupgrade), veya [ **uygulama yükseltme ilerleme durumunu alma** REST işlemini](https://docs.microsoft.com/rest/api/servicefabric/get-the-progress-of-an-application-upgrade1).
8. Gerekirse, *işleci* değiştirir ve geçerli uygulama parametrelerinin yeniden uygular kullanarak yükseltme [ **UpdateApplicationUpgradeAsync** yöntemi](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.applicationmanagementclient#System_Fabric_FabricClient_ApplicationManagementClient_UpdateApplicationUpgradeAsync_System_Fabric_Description_ApplicationUpgradeUpdateDescription_System_TimeSpan_System_Threading_CancellationToken_), [ **güncelleştirme ServiceFabricApplicationUpgrade** cmdlet](https://docs.microsoft.com/powershell/servicefabric/vlatest/update-servicefabricapplicationupgrade), veya [ **güncelleştirme uygulama yükseltme** REST işlemini](https://docs.microsoft.com/rest/api/servicefabric/update-an-application-upgrade).
9. Gerekirse, *işleci* geçerli uygulama geri alındığında kullanarak yükseltme [ **RollbackApplicationUpgradeAsync** yöntemi](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.applicationmanagementclient#System_Fabric_FabricClient_ApplicationManagementClient_RollbackApplicationUpgradeAsync_System_Uri_System_TimeSpan_System_Threading_CancellationToken_), [ **başlangıç ServiceFabricApplicationRollback** cmdlet](https://docs.microsoft.com/powershell/servicefabric/vlatest/start-servicefabricapplicationrollback), veya [ **geri alma uygulama yükseltme** REST işlemini](https://docs.microsoft.com/rest/api/servicefabric/rollback-an-application-upgrade).
10. Service Fabric herhangi bağlı hizmetlerinin kullanılabilirliğini kaybetmeden kümede çalışan hedef uygulama yükseltir.

Bkz: [uygulaması yükseltme Öğreticisi](service-fabric-application-upgrade-tutorial.md) örnekleri için.

## <a name="maintain"></a>Koru
1. İşletim sistemi yükseltildikten ve düzeltme ekleri için Service Fabric kümede çalışan tüm uygulamaların kullanılabilirliğini garanti etmek için Azure altyapı ile arabirimleri.
2. Yükseltmeleri ve düzeltme eklerini Service Fabric platformu için Service Fabric kendisini küme üzerinde çalışan uygulamalardan herhangi birinin kullanılabilirliğini kaybetmeden yükseltir.
3. Bir *Uygulama Yöneticisi* eklenmesi veya geçmiş kapasite kullanım verileri ve öngörülen ileri tarihli talebi çözümleme sonra bir küme düğümünden kaldırılması onaylar.
4. Bir *işleci* ekler ve tarafından belirtilen düğümleri kaldırır *Uygulama Yöneticisi*.
5. Yeni düğümler eklenir veya varolan düğümleri kümeden kaldırılır, Service Fabric otomatik olarak yük-çalışan uygulamaları en iyi performans elde etmek için kümedeki tüm düğümlere dengeler.

## <a name="remove"></a>Kaldır
1. Bir *işleci* kümede çalışan bir hizmeti belirli bir örneği kullanarak tüm uygulama kaldırmadan silebilirsiniz [ **DeleteServiceAsync** yöntemi](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.servicemanagementclient#System_Fabric_FabricClient_ServiceManagementClient_DeleteServiceAsync_System_Fabric_Description_DeleteServiceDescription_System_TimeSpan_System_Threading_CancellationToken_), [ **Kaldır ServiceFabricService** cmdlet](https://docs.microsoft.com/powershell/servicefabric/vlatest/remove-servicefabricservice), veya [ **hizmet silme** REST işlemini](https://docs.microsoft.com/rest/api/servicefabric/delete-a-service).  
2. Bir *işleci* uygulama örneğini ve tüm hizmetlerinin kullanarak silebilirsiniz [ **DeleteApplicationAsync** yöntemi](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.applicationmanagementclient#System_Fabric_FabricClient_ApplicationManagementClient_DeleteApplicationAsync_System_Fabric_Description_DeleteApplicationDescription_System_TimeSpan_System_Threading_CancellationToken_), [ **Kaldır ServiceFabricApplication** cmdlet](https://docs.microsoft.com/powershell/servicefabric/vlatest/remove-servicefabricapplication), veya [ **uygulama Sil** REST işlemini](https://docs.microsoft.com/rest/api/servicefabric/delete-an-application).
3. Uygulama ve Hizmetleri durdurduktan sonra *işleci* uygulama türünü kullanarak sağlamasını [ **UnprovisionApplicationAsync** yöntemi](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.applicationmanagementclient#System_Fabric_FabricClient_ApplicationManagementClient_UnprovisionApplicationAsync_System_String_System_String_System_TimeSpan_System_Threading_CancellationToken_), [ **Unregister-ServiceFabricApplicationType** cmdlet](https://docs.microsoft.com/powershell/servicefabric/vlatest/unregister-servicefabricapplicationtype), veya [ **uygulama sağlamasını** REST işlemini](https://docs.microsoft.com/rest/api/servicefabric/unprovision-an-application). Uygulama türü sağlamanın kaldırılması, uygulama paketi görüntü kaldırmaz. Uygulama paketi el ile kaldırmanız gerekir.
4. Bir *işleci* görüntü kullanarak uygulama paketi kaldırır [ **RemoveApplicationPackage** yöntemi](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.applicationmanagementclient#System_Fabric_FabricClient_ApplicationManagementClient_RemoveApplicationPackage_System_String_System_String_) veya [ **Kaldır ServiceFabricApplicationPackage** cmdlet](/powershell/module/servicefabric/remove-servicefabricapplicationpackage?view=azureservicefabricps).

Bkz: [bir uygulamayı dağıtmak](service-fabric-deploy-remove-applications.md) örnekleri için.

## <a name="next-steps"></a>Sonraki adımlar
Geliştirme hakkında daha fazla bilgi için test ve Service Fabric uygulamaları ve Hizmetleri yönetme bakın:

* [Reliable Actors](service-fabric-reliable-actors-introduction.md)
* [Reliable Services](service-fabric-reliable-services-introduction.md)
* [Bir uygulamayı dağıtma](service-fabric-deploy-remove-applications.md)
* [Uygulama yükseltme](service-fabric-application-upgrade.md)
* [Test Edilebilirlik genel bakış](service-fabric-testability-overview.md)
