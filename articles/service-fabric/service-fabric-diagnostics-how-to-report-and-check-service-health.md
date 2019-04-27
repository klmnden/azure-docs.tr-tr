---
title: Azure Service Fabric ile durumunu raporlama ve denetleme | Microsoft Docs
description: Hizmet kodunuzdan sistem durumu raporlarını göndermek ve sağlayan Azure Service Fabric sistem durumu izleme araçları kullanarak, hizmet durumunu denetlemek öğrenin.
services: service-fabric
documentationcenter: .net
author: srrengar
manager: mfussell
editor: ''
ms.assetid: 7c712c22-d333-44bc-b837-d0b3603d9da8
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 02/25/2019
ms.author: srrengar
ms.openlocfilehash: 2126157f49bd978d2218986601245cae2e4157b6
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60322086"
---
# <a name="report-and-check-service-health"></a>Hizmet durumunu raporlama ve denetleme
Hizmetlerinizi sorunlarla, yanıt ve olayları ve kesintileri düzeltme olanağınız sorunları hızlı bir şekilde algılamak için yeteneğinizi bağlıdır. Sorunlar ve hatalar için Azure Service Fabric sistem durumu Yöneticisi hizmeti kodunuzdan raporu standart sistem durumu izleme Service Fabric sistem durumunu denetlemek için sağladığı araçları kullanabilirsiniz.

Sistem durumu hizmetinden rapor üç yolu vardır:

* Kullanım [bölüm](https://docs.microsoft.com/dotnet/api/system.fabric.istatefulservicepartition) veya [CodePackageActivationContext](https://docs.microsoft.com/dotnet/api/system.fabric.codepackageactivationcontext) nesneleri.  
  Kullanabileceğiniz `Partition` ve `CodePackageActivationContext` geçerli bağlam parçası olan öğeler durumunu bildirmek için nesne. Örneğin, bir çoğaltma bir parçası olarak çalışan kodu yalnızca bu çoğaltma, ait olduğu bölüm ve bir parçası olan uygulama sistem durumu bildirebilir.
* Kullanım `FabricClient`.   
  Kullanabileceğiniz `FabricClient` için rapor sistem durumu hizmet kodundan küme değilse [güvenli](service-fabric-cluster-security.md) veya yönetici ayrıcalıklarına sahip hizmet çalışıyorsa. Çoğu gerçek dünya senaryoları değil güvenli olmayan kümelerini kullanın veya yönetici ayrıcalıkları verin. İle `FabricClient`, sistem durumu kümesinin bir parçası olan herhangi bir varlıkta bildirebilirsiniz. İdeal olarak, ancak hizmet kodunu yalnızca kendi sağlığı ile ilgili raporları göndermesi gerekir.
* Küme, uygulama, dağıtılan uygulama, hizmet, hizmet paketi, bölüm, çoğaltma veya düğüm düzeylerinde REST API'lerini kullanın. Bu sistem durumu bir kapsayıcı içindeki bildirmek için kullanılabilir.

Bu makalede hizmet kodundan durumu raporları örneği size kılavuzluk eder. Örnek ayrıca sistem durumunu denetlemek için Service Fabric tarafından sağlanan araçları nasıl kullanılabileceğini gösterir. Bu makalede, Service Fabric yetenekleri izleme sistem durumu hızlı bir giriş olması amaçlanmıştır. Makale serisi, bu makalenin sonundaki bağlantıyı ile başlayan ayrıntılı sistem durumu hakkında ayrıntılı bilgi için edinebilirsiniz.

## <a name="prerequisites"></a>Önkoşullar
Aşağıdakilerin yüklü olması gerekir:

* Visual Studio 2015 veya Visual Studio 2017
* Service fabric SDK'sı

## <a name="to-create-a-local-secure-dev-cluster"></a>Güvenli yerel geliştirme kümesi oluşturmak için
* PowerShell'i yönetici ayrıcalıklarıyla açın ve aşağıdaki komutları çalıştırın:

![Güvenli bir geliştirme kümesi oluşturmayı gösteren komutları](./media/service-fabric-diagnostics-how-to-report-and-check-service-health/create-secure-dev-cluster.png)

## <a name="to-deploy-an-application-and-check-its-health"></a>Bir uygulamayı dağıtmak ve onun durumunu denetleme
1. Visual Studio'yu yönetici olarak açın.
1. Kullanarak bir proje oluşturma **durum bilgisi olan hizmet** şablonu.
   
    ![Durum bilgisi olan hizmet ile bir Service Fabric uygulaması oluşturma](./media/service-fabric-diagnostics-how-to-report-and-check-service-health/create-stateful-service-application-dialog.png)
1. Tuşuna **F5** uygulamayı hata ayıklama modunda çalıştırın. Uygulamayı yerel kümeye dağıtılır.
1. Uygulama çalışmaya başladıktan sonra bildirim alanında yerel Küme Yöneticisi simgesine sağ tıklayıp **yerel kümeyi Yönet** kısayol menüsünden Service Fabric Explorer'ı açın.
   
    ![Service Fabric Explorer, bildirim alanından açın](./media/service-fabric-diagnostics-how-to-report-and-check-service-health/LaunchSFX.png)
1. Bu resimde göründüğü gibi uygulama durumunun görüntülenmesi gerekir. Şu anda uygulama hatasız iyi durumda olması gerekir.
   
    ![Service Fabric Explorer'da sağlıklı uygulama](./media/service-fabric-diagnostics-how-to-report-and-check-service-health/sfx-healthy-app.png)
1. Ayrıca, PowerShell kullanarak durumu denetleyebilirsiniz. Kullanabileceğiniz ```Get-ServiceFabricApplicationHealth``` uygulamanın durumunu ve denetlemek için kullanabilir ```Get-ServiceFabricServiceHealth``` bir hizmetin durumu denetlemek için. Bu görüntüde PowerShell'de aynı uygulama için sistem durumu raporudur.
   
    ![PowerShell'de sağlıklı uygulama](./media/service-fabric-diagnostics-how-to-report-and-check-service-health/ps-healthy-app-report.png)

## <a name="to-add-custom-health-events-to-your-service-code"></a>Özel durum olayları hizmeti kodunuza eklemek için
Visual Studio'da Service Fabric proje şablonları, örnek kod içerir. Aşağıdaki adımlar, nasıl özel durum olayları hizmet kodunuzdan bildirebilirsiniz gösterir. Bu raporlar otomatik olarak Service Fabric, Service Fabric Explorer, Azure portal durum görünümü ve PowerShell gibi sağladığı sistem durumu izleme için standart Araçları'nda görüntülenir.

1. Visual Studio'da daha önce oluşturduğunuz uygulamayı kapatıp yeniden açmanız veya kullanarak yeni bir uygulama oluşturma **durum bilgisi olan hizmet** Visual Studio şablonu.
1. Stateful1.cs dosyasını açın ve bulma `myDictionary.TryGetValueAsync` Çağır `RunAsync` yöntemi. Bu yöntem döndürdüğünü gördüğünüz bir `result` anahtar mantığı bu uygulamada çalışan sayısını olduğundan geçerli sayaç değerini tutan. Bu gerçek bir uygulama varsa ve bir hata sonucu eksikliği temsil varsa, bu olay bayrak istersiniz.
1. Bir hata sonucu eksikliği temsil ettiğinde sistem durumu olayı bildirmek için aşağıdaki adımları ekleyin.
   
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
    Durum bilgisi olan bir hizmetten bildirildiğinden çünkü biz çoğaltma sistem durumu bildirir. `HealthInformation` Parametre bildirildiğinden sistem durumu sorunu hakkında bilgi depolar.
   
    Durum bilgisi olmayan hizmet oluşturduysanız aşağıdaki kodu kullanın.
   
    ```csharp
    if (!result.HasValue)
    {
        HealthInformation healthInformation = new HealthInformation("ServiceCode", "StateDictionary", HealthState.Error);
        this.Partition.ReportInstanceHealth(healthInformation);
    }
    ```
1. Hizmetinizin yönetici ayrıcalıklarına sahip olarak çalışıyorsa ya da küme değilse [güvenli](service-fabric-cluster-security.md), ayrıca `FabricClient` için rapor sistem durumu, aşağıdaki adımlarda gösterildiği gibi.  
   
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
1. Şimdi bu hata benzetimi yapma ve sistem durumu izleme araçları görünmesini çıksın. Hata benzetimi yapmak için sistem durumu, daha önce eklediğiniz kod raporlama ilk satırı açıklama satırı yapın. İlk satırı açıklama sonra kod aşağıdaki örnekteki gibi görünecektir.
   
    ```csharp
    //if(!result.HasValue)
    {
        HealthInformation healthInformation = new HealthInformation("ServiceCode", "StateDictionary", HealthState.Error);
        this.Partition.ReportReplicaHealth(healthInformation);
    }
    ```
   Bu kod, sistem durumu raporu her değişiminde tetikler `RunAsync` yürütür. Değişikliği yaptıktan sonra basın **F5** uygulamayı çalıştırın.
1. Uygulama çalışmaya başladıktan sonra uygulama durumunu denetlemek için Service Fabric Explorer'ı açın. Bu kez, Service Fabric Explorer, uygulamanın sağlıksız olduğunu gösterir. Daha önce eklediğimiz koddan bildirilen hata nedeniyle budur.
   
    ![Service Fabric Explorer'da iyi durumda olmayan uygulama](./media/service-fabric-diagnostics-how-to-report-and-check-service-health/sfx-unhealthy-app.png)
1. Service Fabric Explorer ağaç görünümünde birincil çoğaltma'yı seçerseniz, göreceksiniz **sistem durumu** çok bir hata gösterir. Service Fabric Explorer, aynı zamanda eklenen sistem durumu raporu ayrıntıları görüntüler `HealthInformation` kodda parametresi. PowerShell ve Azure Portalı'nda aynı sistem durumu raporlarının görebilirsiniz.
   
    ![Service Fabric Explorer'da çoğaltma durumu](./media/service-fabric-diagnostics-how-to-report-and-check-service-health/replica-health-error-report-sfx.png)

Bu rapor, başka bir rapora veya bu çoğaltma silinene kadar değiştirilene kadar sistem durumu Yöneticisi'nde kalır. Biz ayarlanmamış olduğundan `TimeToLive` bu sistem durumu raporu için `HealthInformation` nesnesi, raporu her zaman geçerli olsun.

Sistem durumu çoğaltma bu durumda, en ayrıntılı düzeyine bildirileceğini öneririz. Sistem durumu üzerinde raporlamadan `Partition`.

```csharp
HealthInformation healthInformation = new HealthInformation("ServiceCode", "StateDictionary", HealthState.Error);
this.Partition.ReportPartitionHealth(healthInformation);
```

Rapor Durumu'na için `Application`, `DeployedApplication`, ve `DeployedServicePackage`, kullanın `CodePackageActivationContext`.

```csharp
HealthInformation healthInformation = new HealthInformation("ServiceCode", "StateDictionary", HealthState.Error);
var activationContext = FabricRuntime.GetActivationContext();
activationContext.ReportApplicationHealth(healthInformation);
```

## <a name="next-steps"></a>Sonraki adımlar
* [Service Fabric sistem durumunu derinlemesine](service-fabric-health-introduction.md)
* [Hizmet durumunu raporlama için REST API](https://docs.microsoft.com/rest/api/servicefabric/report-the-health-of-a-service)
* [Uygulama durumunu raporlama için REST API](https://docs.microsoft.com/rest/api/servicefabric/report-the-health-of-an-application)

