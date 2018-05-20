---
title: Rapor ve Azure Service Fabric ile sistem durumu denetimi | Microsoft Docs
description: Hizmet kodunuzdan sistem durumu raporları göndermek nasıl Azure Service Fabric sağlayan sistem durumu izleme araçları kullanarak Hizmetinizin durumunu denetlemek nasıl öğrenin.
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: mfussell
editor: ''
ms.assetid: 7c712c22-d333-44bc-b837-d0b3603d9da8
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/2/2017
ms.author: dekapur
ms.openlocfilehash: 82ee3cbca40713d527f64ae4698cb9ce64a10215
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2018
---
# <a name="report-and-check-service-health"></a>Hizmet durumunu raporlama ve denetleme
Hizmetlerinizin sorunlarla yanıt ve olayları ve kesintileri düzeltme yeteneğinizi sorunları hızla algılamak için yeteneğinizi bağlıdır. Sorunları ve hataları Azure Service Fabric sistem durumu hizmeti kodunuzdan yöneticiye, standart sistem durumu izleme sistem durumunu denetlemek için Service Fabric sağlayan araçları kullanabilirsiniz.

Sistem durumu hizmetinden raporu olan üç yolu vardır:

* Kullanım [bölüm](https://docs.microsoft.com/dotnet/api/system.fabric.istatefulservicepartition) veya [CodePackageActivationContext](https://docs.microsoft.com/dotnet/api/system.fabric.codepackageactivationcontext) nesneleri.  
  Kullanabileceğiniz `Partition` ve `CodePackageActivationContext` geçerli bağlamı parçası olan öğeleri durumunu bildirmek için nesneleri. Örneğin, bir çoğaltma bir parçası olarak çalışan bir kod yalnızca bu çoğaltma, ait olduğu bölüm ve bir parçası olan uygulama sistem durumu bildirebilirsiniz.
* Kullanım `FabricClient`.   
  Kullanabileceğiniz `FabricClient` rapor durumu kümeyi değilse, hizmet kodundan için [güvenli](service-fabric-cluster-security.md) veya yönetici ayrıcalıkları olan hizmeti çalışıyorsa. Çoğu gerçek dünya senaryoları değil güvenli olmayan kümeler kullanın veya yönetici ayrıcalıkları sağlayın. İle `FabricClient`, sistem durumu kümesinin parçası olan herhangi bir varlıkta bildirebilirsiniz. İdeal olarak, ancak hizmet kod yalnızca kendi sağlığı ile ilgili raporları göndermesi gerekir.
* Küme, uygulama, dağıtılan bir uygulama, hizmet, hizmet paketi, bölüm, çoğaltma veya düğüm düzeyleri REST API'leri kullanın. Bu sistem durumu bir kapsayıcıdaki raporlamak için kullanılabilir.

Bu makalede, hizmet kodundan durumu raporları bir örnek adım adım anlatılmaktadır. Bu örnek ayrıca sistem durumunu denetlemek için Service Fabric tarafından sağlanan araçları nasıl kullanılabileceğini gösterir. Bu makale, Service Fabric yeteneklerini izleme sistem durumu bir giriş olması amaçlanmıştır. Daha ayrıntılı bilgi için bu makalenin sonunda bağlantıyla Başlat sistem durumu hakkında kapsamlı makaleleri bir dizi okuyabilir.

## <a name="prerequisites"></a>Önkoşullar
Aşağıdakilerin yüklü olması gerekir:

* Visual Studio 2015 veya Visual Studio 2017
* Service Fabric SDK

## <a name="to-create-a-local-secure-dev-cluster"></a>Yerel güvenli geliştirme küme oluşturmak için
* PowerShell'i yönetici ayrıcalıklarıyla açın ve aşağıdaki komutları çalıştırın:

![Güvenli geliştirme kümesi oluşturmayı gösteren komutları](./media/service-fabric-diagnostics-how-to-report-and-check-service-health/create-secure-dev-cluster.png)

## <a name="to-deploy-an-application-and-check-its-health"></a>Bir uygulamayı dağıtmak ve durumunu denetlemek için
1. Visual Studio'yu yönetici olarak açın.
2. Kullanarak bir proje oluşturma **durum bilgisi olan hizmet** şablonu.
   
    ![Durum bilgisi olan hizmeti ile bir Service Fabric uygulaması oluşturma](./media/service-fabric-diagnostics-how-to-report-and-check-service-health/create-stateful-service-application-dialog.png)
3. Tuşuna **F5** uygulamayı hata ayıklama modunda çalıştırın. Uygulamayı yerel kümeye dağıtılır.
4. Uygulama çalışmaya başladıktan sonra bildirim alanında yerel Küme Yöneticisi simgesine sağ tıklayın ve seçin **yerel kümeyi Yönet** Service Fabric Explorer açmak için kısayol menüsünden.
   
    ![Bildirim alanından Service Fabric Explorer'ı açın](./media/service-fabric-diagnostics-how-to-report-and-check-service-health/LaunchSFX.png)
5. Uygulama durumunu olduğu gibi bu görüntüyü görüntülenmesi gerekir. Şu anda uygulama hatasız sağlıklı olmalıdır.
   
    ![Service Fabric Explorer'da sağlıklı uygulama](./media/service-fabric-diagnostics-how-to-report-and-check-service-health/sfx-healthy-app.png)
6. Ayrıca, sistem durumu PowerShell kullanarak da kontrol edebilirsiniz. Kullanabileceğiniz ```Get-ServiceFabricApplicationHealth``` uygulamaya ait sağlık ve denetlemek için kullanabileceğiniz ```Get-ServiceFabricServiceHealth``` bir hizmetin sistem durumu denetlemek için. Sistem Durumu raporu PowerShell'de aynı uygulama için bu görüntüyü kullanılır.
   
    ![PowerShell'de sağlıklı uygulama](./media/service-fabric-diagnostics-how-to-report-and-check-service-health/ps-healthy-app-report.png)

## <a name="to-add-custom-health-events-to-your-service-code"></a>Özel durum olayları hizmet kodunuzun eklemek için
Visual Studio Service Fabric proje şablonlarını örnek kod içerir. Aşağıdaki adımlar, nasıl özel durum olayları hizmet kodunuzdan bildirebilirsiniz gösterir. Bu raporlar otomatik olarak Service Fabric, Service Fabric Explorer, Azure portal durum görünümü ve PowerShell gibi sağladığı sistem durumu izleme için standart Araçları'nda görünür.

1. Visual Studio'da daha önce oluşturduğunuz uygulamayı kapatıp yeniden açmanız veya kullanarak yeni bir uygulama oluşturma **durum bilgisi olan hizmet** Visual Studio şablonu.
2. Stateful1.cs dosyasını açın ve Bul `myDictionary.TryGetValueAsync` Çağır `RunAsync` yöntemi. Bu yöntem döndürdüğünü gördüğünüz bir `result` çalışan sayısı tutmak için bu uygulamayı anahtar mantık olduğu için geçerli sayaç değerini tutan. Bu gerçek bir uygulamada olsaydı ve bir hata sonucu eksikliği temsil varsa, bu olay bayrak istersiniz.
3. Bir hata sonucu eksikliği temsil eden sistem durumu olayı bildirmek için aşağıdaki adımları ekleyin.
   
    a. Ekleme `System.Fabric.Health` Stateful1.cs dosyaya ad alanı.
   
    ```csharp
    using System.Fabric.Health;
    ```
   
    b. Sonra aşağıdaki kodu ekleyin `myDictionary.TryGetValueAsync` çağırın
   
    ```csharp
    if (!result.HasValue)
    {
        HealthInformation healthInformation = new HealthInformation("ServiceCode", "StateDictionary", HealthState.Error);
        this.Partition.ReportReplicaHealth(healthInformation);
    }
    ```
    Bir durum bilgisi olan hizmetinden raporlandığını çünkü biz çoğaltma sistem durumu raporu. `HealthInformation` Parametre rapor edilen sistem durumu sorun hakkında bilgi depolar.
   
    Durum bilgisiz hizmet oluşturduysanız, aşağıdaki kodu kullanın
   
    ```csharp
    if (!result.HasValue)
    {
        HealthInformation healthInformation = new HealthInformation("ServiceCode", "StateDictionary", HealthState.Error);
        this.Partition.ReportInstanceHealth(healthInformation);
    }
    ```
4. Hizmetinizin yönetici ayrıcalıklarıyla çalıştırıyorsa veya küme değilse [güvenli](service-fabric-cluster-security.md), aynı zamanda `FabricClient` aşağıdaki adımlarda gösterildiği gibi sistem durumu raporu için.  
   
    a. Oluşturma `FabricClient` sonra örnek `var myDictionary` bildirimi.
   
    ```csharp
    var fabricClient = new FabricClient(new FabricClientSettings() { HealthReportSendInterval = TimeSpan.FromSeconds(0) });
    ```
   
    b. Sonra aşağıdaki kodu ekleyin `myDictionary.TryGetValueAsync` çağırın.
   
    ```csharp
    if (!result.HasValue)
    {
       var replicaHealthReport = new StatefulServiceReplicaHealthReport(
            this.Context.PartitionId,
            this.Context.ReplicaId,
            new HealthInformation("ServiceCode", "StateDictionary", HealthState.Error));
        fabricClient.HealthManager.ReportHealth(replicaHealthReport);
    }
    ```
5. Şimdi bu arızanın benzetimini gerçekleştirin ve bu sistem durumu izleme araçları görünmesini bakın. Hata benzetimi için sistem durumu, daha önce eklediğiniz kod raporlama ilk satırı açıklama. İlk satırını açıklama sonra kod aşağıdaki gibi görünecektir.
   
    ```csharp
    //if(!result.HasValue)
    {
        HealthInformation healthInformation = new HealthInformation("ServiceCode", "StateDictionary", HealthState.Error);
        this.Partition.ReportReplicaHealth(healthInformation);
    }
    ```
   Bu kodu her zaman sistem durumu raporu harekete `RunAsync` yürütür. Değişikliği yaptıktan sonra basın **F5** uygulamayı çalıştırın.
6. Uygulamayı çalıştırdıktan sonra uygulama durumunu denetlemek için Service Fabric Explorer açın. Bu süre, Service Fabric Explorer uygulamanın sağlıksız olduğunu gösterir. Daha önce eklediğimiz koddan bildirilen hata nedeniyle budur.
   
    ![Service Fabric Explorer'da düzgün çalışmayan uygulama](./media/service-fabric-diagnostics-how-to-report-and-check-service-health/sfx-unhealthy-app.png)
7. Service Fabric Explorer ağaç görünümünde birincil çoğaltma seçerseniz, görürsünüz **sistem durumu** çok bir hata gösterir. Service Fabric Explorer de eklenmiştir sistem durumu rapor ayrıntıları görüntüler `HealthInformation` kodu parametresi. PowerShell ve Azure portalı aynı sistem durumu raporları görebilirsiniz.
   
    ![Service Fabric Explorer'da çoğaltma durumu](./media/service-fabric-diagnostics-how-to-report-and-check-service-health/replica-health-error-report-sfx.png)

Bu çoğaltma silinene kadar veya başka bir raporu tarafından değiştirilene kadar bu rapor sistem durumu Yöneticisi'nde kalır. Biz ayarlanmamış olduğundan `TimeToLive` bu sistem durumu raporu için `HealthInformation` nesne, rapor her zaman geçerli olsun.

Sistem durumu çoğaltma bu durumda olan en ayrıntılı düzeyi, bildirilen olduğunu öneririz. Üzerindeki sistem durumu bildirebilirsiniz `Partition`.

```csharp
HealthInformation healthInformation = new HealthInformation("ServiceCode", "StateDictionary", HealthState.Error);
this.Partition.ReportPartitionHealth(healthInformation);
```

Rapor sistem durumunu için `Application`, `DeployedApplication`, ve `DeployedServicePackage`, kullanmak `CodePackageActivationContext`.

```csharp
HealthInformation healthInformation = new HealthInformation("ServiceCode", "StateDictionary", HealthState.Error);
var activationContext = FabricRuntime.GetActivationContext();
activationContext.ReportApplicationHealth(healthInformation);
```

## <a name="next-steps"></a>Sonraki adımlar
* [Service Fabric sistem üzerinde derin Dalış](service-fabric-health-introduction.md)
* [Hizmet durumu raporlama için REST API](https://docs.microsoft.com/rest/api/servicefabric/report-the-health-of-a-service)
* [Uygulama durumu raporlama için REST API](https://docs.microsoft.com/rest/api/servicefabric/report-the-health-of-an-application)

