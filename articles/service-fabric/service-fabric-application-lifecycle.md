---
title: Service fabric'te uygulama yaşam döngüsü | Microsoft Docs
description: Geliştirme, dağıtma, test, yükseltme, bakımını yapma ve Service Fabric uygulamaları kaldırma açıklar.
services: service-fabric
documentationcenter: .net
author: athinanthny
manager: chackdan
editor: ''
ms.assetid: 08837cca-5aa7-40da-b087-2b657224a097
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 1/19/2018
ms.author: atsenthi
ms.openlocfilehash: 53cab3591ea11721e36b48438f35df016e2a9f3a
ms.sourcegitcommit: c6dc9abb30c75629ef88b833655c2d1e78609b89
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2019
ms.locfileid: "58664990"
---
# <a name="service-fabric-application-lifecycle"></a>Service Fabric uygulama yaşam döngüsü
Diğer platformlar ile Azure Service fabric'te uygulama genellikle aşağıdaki aşamaları geçtikçe: tasarım, geliştirme, test, dağıtım, yükseltme, Bakım ve kaldırma. Service Fabric, nihai yetkisinin alınması için bulut uygulamaları, geliştirme, dağıtım, günlük yönetim ve Bakım tam uygulama yaşam döngüsü için birinci sınıf destek sağlar. Hizmet modeli, uygulama yaşam döngüsü içinde bağımsız olarak katılmak birkaç farklı rol sağlar. Bu makalede, API'leri ve Service Fabric uygulama yaşam döngüsünün aşamaları boyunca farklı rolleri tarafından nasıl kullanıldıkları hakkında genel bir bakış sağlar.

[!INCLUDE [links to azure cli and service fabric cli](../../includes/service-fabric-sfctl.md)]

## <a name="service-model-roles"></a>Hizmet modeli rolleri
Hizmet modeli rolü şunlardır:

* **Servis Geliştirici**: Başka bir amaçla kullanılması ve aynı veya farklı türleri, birden çok uygulamada kullanılan modüler ve genel Hizmetleri geliştirir. Örneğin, bir kuyruk hizmeti, bir bilet oluşturma uygulaması (Yardım Masası) veya bir e-ticaret uygulaması (alışveriş sepetini) oluşturmak için kullanılabilir.
* **Uygulama geliştiricisi**: Uygulamaları, belirli özel gereksinimler ya da senaryoları karşılamak için hizmetler koleksiyonu tümleştirerek oluşturur. Örneğin, bir e-ticaret sitesi "JSON durum bilgisi olmayan ön uç hizmeti," tümleşebilen "Artırma durum bilgisi olan hizmet" ve "Sıra durum bilgisi olan hizmet" auctioning çözümünü oluşturmak için.
* **Uygulama Yöneticisi**: Uygulama Yapılandırma (şablon parametrelerinden yapılandırma doldurma), dağıtım (kullanılabilir kaynaklara eşleme) ve hizmet kalitesi kararlarını verir. Örneğin, bir uygulama Yöneticisi (Amerika Birleşik Devletleri İngilizce) ya da örneğin, Japonya için Japonca dil yerel uygulamanın karar verir. Farklı bir dağıtılmış uygulama farklı ayarlara sahip olabilir.
* **İşleç**: Uygulama yapılandırmasını temel alan uygulamalar ve uygulama yönetici tarafından belirtilen gereksinimleri dağıtır. Örneğin, operatör sağlar ve uygulamayı dağıtır ve Azure'da çalıştığını sağlar. İşleçler, uygulama sistem durumu ve performans bilgilerini izlemek ve gerektiğinde fiziksel altyapı Bakımı.

## <a name="develop"></a>Geliştirme
1. A *servis Geliştirici* hizmetlerini kullanarak farklı türlerde geliştiren [Reliable Actors](service-fabric-reliable-actors-introduction.md) veya [Reliable Services](service-fabric-reliable-services-introduction.md) programlama modeli.
2. A *servis Geliştirici* bildirimli olarak bir veya daha fazla kod, yapılandırma ve veri paketlerini oluşan bir hizmet bildirimi dosyasında geliştirilen hizmet türlerini açıklar.
3. Bir *uygulama geliştiricisi* sonra farklı hizmet türleri kullanarak bir uygulama oluşturur.
4. Bir *uygulama geliştiricisi* bildirimli olarak başvuran bağlı hizmetin hizmet bildirimleri ve uygun şekilde geçersiz kılma ve farklı kümesini parametreleştirme tarafından uygulamanın türü uygulama bildiriminde açıklanmaktadır bağlı hizmetler, yapılandırma ve dağıtım ayarları.

Bkz: [Reliable Actors hizmetini kullanmaya başlama](service-fabric-reliable-actors-get-started.md) ve [Reliable Services ile çalışmaya başlama](service-fabric-reliable-services-quick-start.md) örnekler.

## <a name="deploy"></a>Dağıt
1. Bir *Uygulama Yöneticisi* uygulama türünü uygun parametrelerini belirterek, bir Service Fabric kümesine dağıtılması için belirli bir uygulamaya belirlenmek **ApplicationType** uygulama bildiriminde öğesi.
2. Bir *işleci* kullanarak uygulama paketini kümenin görüntü deposuna yükler [ **CopyApplicationPackage** yöntemi](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.applicationmanagementclient) veya [  **Kopyalama ServiceFabricApplicationPackage** cmdlet'i](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps). Uygulama paketi, uygulama bildiriminin ve hizmet paketleri koleksiyonunu içerir. Service Fabric uygulamalarını bir Azure blob depolama veya Service Fabric sistem hizmeti görüntü deposunda saklanan uygulama paketinden dağıtır.
3. *İşleci* sonra uygulama türünü kullanarak karşıya yüklenen uygulama paketinin hedef kümedeki sağlar [ **ProvisionApplicationAsync** yöntemi](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.applicationmanagementclient),  [ **Register-ServiceFabricApplicationType** cmdlet'i](https://docs.microsoft.com/powershell/module/servicefabric/register-servicefabricapplicationtype), veya [ **uygulama sağlama** REST işlemini](https://docs.microsoft.com/rest/api/servicefabric/provision-an-application).
4. Uygulama sağlama sonra bir *işleci* tarafından sağlanan parametrelerle uygulama başladığında *Uygulama Yöneticisi* kullanarak [  **CreateApplicationAsync** yöntemi](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.applicationmanagementclient), [ **yeni ServiceFabricApplication** cmdlet'i](https://docs.microsoft.com/powershell/module/servicefabric/new-servicefabricapplication), veya [ **uygulama oluşturma**  REST işlemini](https://docs.microsoft.com/rest/api/servicefabric/create-an-application).
5. Uygulama dağıtıldıktan sonra bir *işleci* kullanan [ **CreateServiceAsync** yöntemi](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.servicemanagementclient), [ **yeni ServiceFabricService**  cmdlet'i](https://docs.microsoft.com/powershell/module/servicefabric/new-servicefabricservice), veya [ **hizmet Oluştur** REST işlemini](https://docs.microsoft.com/rest/api/servicefabric/create-a-service) kullanılabilir hizmet türlerine göre uygulama için yeni hizmet örnekleri oluşturmak için.
6. Uygulama artık Service Fabric kümesinde çalışıyor.

Bkz: [uygulama dağıtma](service-fabric-deploy-remove-applications.md) örnekler.

## <a name="test"></a>Test et
1. Yerel geliştirme kümesi veya bir test kümesine dağıttıktan sonra bir *servis Geliştirici* yerleşik yük devretme testi senaryosu kullanarak çalıştırır [ **FailoverTestScenarioParameters** ](https://docs.microsoft.com/dotnet/api/system.fabric.testability.scenario.failovertestscenarioparameters) ve [ **FailoverTestScenario** ](https://docs.microsoft.com/dotnet/api/system.fabric.testability.scenario.failovertestscenario) sınıfları veya [ **Invoke-ServiceFabricFailoverTestScenario** cmdlet'i](/powershell/module/servicefabric/invoke-servicefabricfailovertestscenario?view=azureservicefabricps). Yük devretme testi senaryosu, önemli geçişleri ve hala kullanılabilir ve çalışır olduğundan emin olmak için yük devretme işlemleri belirtilen bir hizmeti çalışır.
2. *Servis Geliştirici* ardından yerleşik chaos test senaryosu kullanarak çalıştırır [ **ChaosTestScenarioParameters** ](https://docs.microsoft.com/dotnet/api/system.fabric.testability.scenario.chaostestscenarioparameters) ve [  **ChaosTestScenario** ](https://docs.microsoft.com/dotnet/api/system.fabric.testability.scenario.chaostestscenario) sınıfları veya [ **Invoke-ServiceFabricChaosTestScenario** cmdlet'i](/powershell/module/servicefabric/invoke-servicefabricchaostestscenario?view=azureservicefabricps). Kaos test senaryosu, kümeye birden çok düğüm, kod paketi ve çoğaltma hataları rastgele sevk.
3. *Servis Geliştirici* [hizmetten hizmete iletişimi test](service-fabric-testability-scenarios-service-communication.md) birincil çoğaltmalara küme çevresinde hareket test senaryoları yazma tarafından.

Bkz: [hata analizi hizmeti giriş](service-fabric-testability-overview.md) daha fazla bilgi için.

## <a name="upgrade"></a>Yükselt
1. A *servis Geliştirici* bağlı Hizmetleri örneklenmiş uygulamanın güncelleştirir ve/veya hataları düzeltir ve hizmet bildiriminin yeni bir sürümü sağlar.
2. Bir *uygulama geliştiricisi* parametreleştiren tutarlı hizmetlerin yapılandırma ve dağıtım ayarları geçersiz kılar ve yeni bir sürümünü uygulama bildirim sağlar. Uygulama geliştiricisi uygulamasına hizmet bildirimleri yeni sürümlerini içerir ve güncelleştirilmiş uygulama paketi uygulama türünde yeni bir sürümü sağlar.
3. Bir *Uygulama Yöneticisi* uygun parametreleri güncelleştirerek hedef uygulamasına yeni uygulama türü sürümünü içerir.
4. Bir *işleci* güncelleştirilmiş uygulama paketini kümenin görüntü kullanarak mağaza yükler [ **CopyApplicationPackage** yöntemi](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.applicationmanagementclient) veya [ **Kopyalama ServiceFabricApplicationPackage** cmdlet'i](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps). Uygulama paketi, uygulama bildiriminin ve hizmet paketleri koleksiyonunu içerir.
5. Bir *işleci* kullanarak hedef kümedeki uygulamanın yeni sürümünü sağlar [ **ProvisionApplicationAsync** yöntemi](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.applicationmanagementclient), [  **Register-ServiceFabricApplicationType** cmdlet'i](https://docs.microsoft.com/powershell/module/servicefabric/register-servicefabricapplicationtype), veya [ **uygulama sağlama** REST işlemini](https://docs.microsoft.com/rest/api/servicefabric/provision-an-application).
6. Bir *işleci* yeni sürümü kullanarak hedef uygulama yükseltmeleri [ **UpgradeApplicationAsync** yöntemi](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.applicationmanagementclient), [  **Başlangıç ServiceFabricApplicationUpgrade** cmdlet'i](https://docs.microsoft.com/powershell/module/servicefabric/start-servicefabricapplicationupgrade), veya [ **uygulama yükseltme** REST işlemini](https://docs.microsoft.com/rest/api/servicefabric/upgrade-an-application).
7. Bir *işleci* yükseltme kullanarak ilerleme durumunu denetler [ **GetApplicationUpgradeProgressAsync** yöntemi](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.applicationmanagementclient), [  **Get-ServiceFabricApplicationUpgrade** cmdlet'i](https://docs.microsoft.com/powershell/module/servicefabric/get-servicefabricapplicationupgrade), veya [ **uygulama yükseltme ilerleme durumunu alma** REST işlemini](https://docs.microsoft.com/rest/api/servicefabric/get-the-progress-of-an-application-upgrade1).
8. Gerekirse, *işleci* değiştirir ve geçerli uygulamanın parametreleri yeniden uygular kullanarak yükseltme [ **UpdateApplicationUpgradeAsync** yöntemi](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.applicationmanagementclient), [ **Güncelleştirme ServiceFabricApplicationUpgrade** cmdlet'i](https://docs.microsoft.com/powershell/module/servicefabric/update-servicefabricapplicationupgrade), veya [ **güncelleştirme uygulama yükseltme** REST işlemini](https://docs.microsoft.com/rest/api/servicefabric/update-an-application-upgrade).
9. Gerekirse, *işleci* geçerli uygulamanın geri alır kullanarak yükseltme [ **RollbackApplicationUpgradeAsync** yöntemi](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.applicationmanagementclient), [ **Başlangıç ServiceFabricApplicationRollback** cmdlet'i](https://docs.microsoft.com/powershell/module/servicefabric/start-servicefabricapplicationrollback), veya [ **uygulama geri alma yükseltme** REST işlemini](https://docs.microsoft.com/rest/api/servicefabric/rollback-an-application-upgrade).
10. Service Fabric, bağlı hizmetlerinden herhangi birinin kullanılabilirliğini kaybetmeden kümede çalışan hedef uygulama yükseltir.

Bkz: [uygulaması yükseltme Öğreticisi](service-fabric-application-upgrade-tutorial.md) örnekler.

## <a name="maintain"></a>Koru
1. İşletim sistemi yükseltmeleri ve düzeltme ekleri için Service Fabric kümesinde çalışan tüm uygulamaların kullanılabilirliğini sağlamak için Azure altyapı ile arabirimleri.
2. Yükseltmeleri ve düzeltme ekleri için Service Fabric platform için Service Fabric kendisi küme üzerinde çalışan uygulamalardan herhangi birinin kullanılabilirliğini kaybetmeden yükseltir.
3. Bir *Uygulama Yöneticisi* eklenmesi veya geçmiş kapasite kullanımı verileri ve tahmini gelecek talebi incelendikten sonra kümeden düğüm kaldırma onaylar.
4. Bir *işleci* ekler ve tarafından belirtilen düğümleri kaldırır *Uygulama Yöneticisi*.
5. Yeni bir düğüm eklenir veya var olan düğümleri kümeden kaldırılır, Service Fabric otomatik olarak Yük Dengeleme çalışan uygulamaları en iyi performans elde etmek için kümedeki tüm düğümlerde gerçekleştirir.

## <a name="remove"></a>Kaldır
1. Bir *işleci* kümede çalışan bir hizmetin belirli bir örneğini kullanarak tüm uygulamayı kaldırmadan silebilirsiniz [ **DeleteServiceAsync** yöntemi](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.servicemanagementclient), [ **Remove-ServiceFabricService** cmdlet'i](https://docs.microsoft.com/powershell/module/servicefabric/remove-servicefabricservice), veya [ **hizmet silme** REST işlemini](https://docs.microsoft.com/rest/api/servicefabric/delete-a-service).  
2. Bir *işleci* uygulama örneğini ve tüm hizmetlerinin kullanarak silebilirsiniz [ **DeleteApplicationAsync** yöntemi](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.applicationmanagementclient), [  **Remove-ServiceFabricApplication** cmdlet'i](https://docs.microsoft.com/powershell/module/servicefabric/remove-servicefabricapplication), veya [ **uygulama Sil** REST işlemini](https://docs.microsoft.com/rest/api/servicefabric/delete-an-application).
3. Uygulama ve Hizmetleri durdurduktan sonra *işleci* uygulama türünü kullanarak sağlamasını [ **UnprovisionApplicationAsync** yöntemi](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.applicationmanagementclient), [ **Unregister-ServiceFabricApplicationType** cmdlet'i](https://docs.microsoft.com/powershell/module/servicefabric/unregister-servicefabricapplicationtype), veya [ **uygulamanın sağlamasını** REST işlemini](https://docs.microsoft.com/rest/api/servicefabric/unprovision-an-application). Uygulama türünün sağlamasını kaldırma işlemini uygulama paketi ImageStore kaldırmaz. Uygulama paketini el ile kaldırmanız gerekir.
4. Bir *işleci* ImageStore kullanarak uygulama paketi kaldırır [ **RemoveApplicationPackage** yöntemi](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.applicationmanagementclient) veya [  **Remove-ServiceFabricApplicationPackage** cmdlet'i](/powershell/module/servicefabric/remove-servicefabricapplicationpackage?view=azureservicefabricps).

Bkz: [uygulama dağıtma](service-fabric-deploy-remove-applications.md) örnekler.

## <a name="next-steps"></a>Sonraki adımlar
Geliştirme ile ilgili daha fazla bilgi için test ve Service Fabric uygulamaları ve hizmetleri yönetmek bakın:

* [Reliable Actors](service-fabric-reliable-actors-introduction.md)
* [Reliable Services](service-fabric-reliable-services-introduction.md)
* [Uygulama dağıtma](service-fabric-deploy-remove-applications.md)
* [Uygulama yükseltme](service-fabric-application-upgrade.md)
* [Test edilebilirliğe genel bakış](service-fabric-testability-overview.md)
