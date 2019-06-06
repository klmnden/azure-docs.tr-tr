---
title: Azure Service Fabric uygulama dağıtımı | Microsoft Docs
description: FabricClient API'leri dağıtma ve Service Fabric uygulamaları kaldırmak için kullanın.
services: service-fabric
documentationcenter: .net
author: aljo-microsoft
manager: chackdan
editor: ''
ms.assetid: b120ffbf-f1e3-4b26-a492-347c29f8f66b
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 01/19/2018
ms.author: aljo
ms.openlocfilehash: 4b2d88004696515169ffde96b50d2771bcc1a669
ms.sourcegitcommit: ef06b169f96297396fc24d97ac4223cabcf9ac33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66428137"
---
# <a name="deploy-and-remove-applications-using-fabricclient"></a>Dağıtma ve FabricClient kullanarak uygulamaları kaldırma
> [!div class="op_single_selector"]
> * [Resource Manager](service-fabric-application-arm-resource.md)
> * [PowerShell](service-fabric-deploy-remove-applications.md)
> * [Service Fabric CLI](service-fabric-application-lifecycle-sfctl.md)
> * [FabricClient API’leri](service-fabric-deploy-remove-applications-fabricclient.md)
> 
> 

<br/>

Bir kez bir [uygulama türü paketlenmiş][10], bir Azure Service Fabric kümesine dağıtım için hazırdır. Dağıtım, aşağıdaki üç adımdan oluşur:

1. Görüntü deposu için uygulama paketini karşıya yükleyin
2. Uygulama türünü kaydedin
3. Uygulama paketini görüntü deposundan kaldırmak
4. Uygulama örneğini oluşturma

Bir uygulamayı dağıtırsanız ve kümedeki bir örneğini çalıştıran sonra uygulama örneğini ve uygulama türünü silebilirsiniz. Tamamen bir uygulama, aşağıdaki adımları izleyerek kümeden kaldırın:

1. Çalışan Kaldır (veya sildiğinizde) uygulama örneği
2. Artık ihtiyacınız yoksa, uygulama türünün kaydını

Dağıtma ve hata ayıklama, yerel geliştirme kümenizde uygulamaları için Visual Studio kullanıyorsanız, tüm önceki adımları otomatik olarak bir PowerShell Betiği işlenir.  Bu betik bulunan *betikleri* uygulama projesinin klasörü. Bu makalede, böylece Visual Studio dışında aynı işlemleri de yapabilirsiniz, betik yaptığı hakkında arka plan sağlar. 
 
## <a name="connect-to-the-cluster"></a>Kümeye bağlanma
Oluşturarak kümeye bağlanın. bir [FabricClient](/dotnet/api/system.fabric.fabricclient) bu makaledeki kod örnekleri birini çalıştırmadan önce örneği. Örnekleri bir yerel geliştirme kümesi veya bir uzak kümeye veya Azure Active Directory, X509 kullanılarak güvenli kümeye bağlanmak için sertifikaları veya Windows Active Directory bakın [güvenli bir kümeye bağlanma](service-fabric-connect-to-secure-cluster.md#connect-to-a-cluster-using-the-fabricclient-apis). Yerel geliştirme kümesine bağlanmak için aşağıdaki örneği çalıştırın:

```csharp
// Connect to the local cluster.
FabricClient fabricClient = new FabricClient();
```

## <a name="upload-the-application-package"></a>Uygulama paketini karşıya yükleyin
Derleme ve adlı bir uygulama paketi varsayalım *MyApplication* Visual Studio'da. Varsayılan olarak, "MyApplicationType" ApplicationManifest.xml içinde listelenen uygulama türü adı var.  Gerekli uygulama bildirimi, hizmet bildirimleri ve kodu/config/veri paketleri içeren uygulama paketini bulunan *C:\Users\&lt; kullanıcıadı&gt;\Documents\Visual Studio 2019\Projects\ MyApplication\MyApplication\pkg\Debug*.

Uygulama paketini karşıya yükleme, iç Service Fabric bileşenleri tarafından erişilebilen bir konuma yerleştirir. Service Fabric uygulama paketi kaydı sırasında uygulama paketini doğrular. Ancak, uygulama paketi yerel olarak doğrulamak istiyorsanız (diğer bir deyişle, önce karşıya), kullanın [Test ServiceFabricApplicationPackage](/powershell/module/servicefabric/test-servicefabricapplicationpackage?view=azureservicefabricps) cmdlet'i.

[CopyApplicationPackage](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.copyapplicationpackage) API uygulama paketini kümenin görüntü deposuna yükler. 

Uygulama paketi büyük ve/veya sahip çok sayıda dosya varsa [bu sıkıştırma](service-fabric-package-apps.md#compress-a-package) ve PowerShell kullanarak görüntü deposuna kopyalayın. Sıkıştırma, boyutu ve dosya sayısını azaltır.

Bkz: [görüntü deposu bağlantı dizesi anlamak](service-fabric-image-store-connection-string.md) resim ve görüntü deposu hakkında ek bilgi için bağlantı dizesi depolar.

## <a name="register-the-application-package"></a>Uygulama paketini kaydetme
Uygulama türü ve sürümü, uygulama paketi kaydedildiğinde kullanıma hazır hale uygulama bildirimde belirtilen. Sistem önceki adımda karşıya yüklenen paket okur, paket doğrular, paket içeriğini işler ve işlenen paket bir iç sistem konumuna kopyalar.  

[ProvisionApplicationAsync](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.provisionapplicationasync) uygulama türü kümedeki ve dağıtım için kullanılabilir hale getirmek API kaydeder.

[GetApplicationTypeListAsync](/dotnet/api/system.fabric.fabricclient.queryclient.getapplicationtypelistasync) API tüm başarıyla kayıtlı uygulama türleri hakkında bilgi sağlar. Kayıt tamamlandığında belirlemek için bu API'yi kullanabilirsiniz.

## <a name="remove-an-application-package-from-the-image-store"></a>Bir uygulama paketi görüntü deposundan kaldırmak
Uygulama başarıyla kaydedildikten sonra uygulama paketini kaldırmak önerilir.  Uygulama paketleri görüntü deposundan silme, sistem kaynakları serbest bırakır.  Kullanılmayan uygulama paketleri tutma disk depolama alanını kullanan ve uygulama performası sorunlarını için yol açar. Uygulama paketini kullanarak görüntü deposu Sil [RemoveApplicationPackage](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.removeapplicationpackage) API.

## <a name="create-an-application-instance"></a>Bir uygulama örneği oluşturma
Bir uygulamadan kullanarak başarıyla kaydedildi herhangi bir uygulama türü örneği oluşturabilir [CreateApplicationAsync](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.createapplicationasync) API. Her uygulamanın adı ile başlamalıdır *"fabric:"* şeması ve her uygulama örneği (küme içinde) benzersiz olmalıdır. Hedef uygulama türü uygulama bildiriminde tanımlanan varsayılan hizmetlerin de oluşturulur.

Birden fazla uygulama örneğinin belirtilen herhangi bir kayıtlı uygulama türü sürümü için oluşturulabilir. Her uygulama örneği yalıtım, kendi çalışma dizini ve işlemleri kümesi ile çalışır.

Hangi adlı görmek için uygulamalar ve hizmetler çalıştırmak, bir kümede çalışan [GetApplicationListAsync](/dotnet/api/system.fabric.fabricclient.queryclient.getapplicationlistasync) ve [GetServiceListAsync](/dotnet/api/system.fabric.fabricclient.queryclient.getservicelistasync) API'leri.

## <a name="create-a-service-instance"></a>Bir hizmet örneği oluşturma
Hizmet türü kullanan bir hizmet örneği oluşturabilir [CreateServiceAsync](/dotnet/api/system.fabric.fabricclient.servicemanagementclient.createserviceasync) API.  Hizmet, uygulama bildiriminde varsayılan hizmet olarak bildirilirse, uygulama örneği oluşturulduğunda hizmet örneği.  Çağırma [CreateServiceAsync](/dotnet/api/system.fabric.fabricclient.servicemanagementclient.createserviceasync) zaten örneği bir hizmet için API FabricException türünde bir özel durum döndürür. Özel durum FabricErrorCode.ServiceAlreadyExists değerini içeren bir hata kodu içerir.

## <a name="remove-a-service-instance"></a>Bir hizmet örneğini kaldırma
Bir hizmet örneği artık gerekli olmadığında çalışmasını kaldırabilirsiniz çağırarak uygulama örneği [DeleteServiceAsync](/dotnet/api/system.fabric.fabricclient.servicemanagementclient.deleteserviceasync) API.  

> [!WARNING]
> Bu işlem geri alınamaz ve hizmet durumu kurtarılamıyor.

## <a name="remove-an-application-instance"></a>Uygulama örneğini Kaldır
Bir uygulama örneği artık gerekli olmadığında, kalıcı olarak adını kullanarak kaldırabilirsiniz [DeleteApplicationAsync](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.deleteapplicationasync) API. [DeleteApplicationAsync](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.deleteapplicationasync) tüm hizmet durumunu da, kalıcı olarak kaldırma uygulamaya ait tüm hizmetleri otomatik olarak kaldırır.

> [!WARNING]
> Bu işlem geri alınamaz ve uygulama durumu kurtarılamıyor.

## <a name="unregister-an-application-type"></a>Bir uygulama türünün kaydını silmek
Belirli bir uygulama türü sürümü artık gerekli değilse, uygulama türünü kullanarak bu belirli sürümü kaydını kaldırmanız gerekir [Unregister-ServiceFabricApplicationType](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.unprovisionapplicationasync) API. Kullanılmayan uygulama türü sürümleri kaydı, görüntü deposu tarafından kullanılan depolama alanı serbest bırakır. Hiçbir uygulama bu uygulama türü sürümünü karşı örneği oluşturulur sürece bir uygulama türü sürümünü kaydı olabilir. Ayrıca, uygulama türünü bekleyen uygulama yok olabilir yükseltmeleri bu uygulama türü sürümünü başvuruyor.

## <a name="troubleshooting"></a>Sorun giderme
### <a name="copy-servicefabricapplicationpackage-asks-for-an-imagestoreconnectionstring"></a>İçin bir Imagestoreconnectionstring kopyalama ServiceFabricApplicationPackage sorar
Service Fabric SDK'sı ortamı zaten ayarlanmış doğru varsayılan değerleri olmalıdır. Ancak, gerekirse Imagestoreconnectionstring tüm komutlar için Service Fabric kümesi kullanan değer ile eşleşmelidir. Küme bildiriminde Imagestoreconnectionstring bulabilirsiniz kullanarak [Get-ServiceFabricClusterManifest](/powershell/module/servicefabric/get-servicefabricclustermanifest?view=azureservicefabricps) ve Get-ImageStoreConnectionStringFromClusterManifest komutları:

```powershell
PS C:\> Get-ImageStoreConnectionStringFromClusterManifest(Get-ServiceFabricClusterManifest)
```

**Get-ImageStoreConnectionStringFromClusterManifest** cmdlet'i, Service Fabric SDK PowerShell modülünü parçası olan resim depolama bağlantı dizesini almak için kullanılır.  SDK modülü içeri aktarmak için çalıştırın:

```powershell
Import-Module "$ENV:ProgramFiles\Microsoft SDKs\Service Fabric\Tools\PSModule\ServiceFabricSDK\ServiceFabricSDK.psm1"
```


Imagestoreconnectionstring küme bildiriminde bulunur:

```xml
<ClusterManifest xmlns:xsd="https://www.w3.org/2001/XMLSchema" xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance" Name="Server-Default-SingleNode" Version="1.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">

    [...]

    <Section Name="Management">
      <Parameter Name="ImageStoreConnectionString" Value="file:D:\ServiceFabric\Data\ImageStore" />
    </Section>

    [...]
```

Bkz: [görüntü deposu bağlantı dizesi anlamak](service-fabric-image-store-connection-string.md) resim ve görüntü deposu hakkında ek bilgi için bağlantı dizesi depolar.

### <a name="deploy-large-application-package"></a>Büyük uygulama paketini dağıtma
Sorun: [CopyApplicationPackage](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.copyapplicationpackage) API zaman aşımına büyük uygulama paketi (sipariş GB).
Deneyin:
- İçin daha büyük bir zaman belirtmek [CopyApplicationPackage](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.copyapplicationpackage) yöntemi ile `timeout` parametresi. Varsayılan olarak, zaman aşımını 30 dakikadır.
- Küme ve kaynak makine arasında ağ bağlantısını denetleyin. Bağlantısının yavaş olması daha iyi bir ağ bağlantısı ile bir makineyi kullanmayı düşünün.
İstemci makine kümesinden başka bir bölgede ise, bir istemci makine olarak aynı veya daha yakın bir bölgede kullanarak göz önünde bulundurun.
- Dış azaltma karşılaşırsınız olmadığını kontrol edin. Örneğin, görüntü deposu azure depolamanızı kullanmak için yapılandırıldığında, karşıya yüklemeyi azaltma.

Sorun: Karşıya yükleme paketi başarıyla tamamlandı ancak [ProvisionApplicationAsync](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.provisionapplicationasync) API zaman aşımına uğrar. Deneyin:
- [Paket sıkıştırma](service-fabric-package-apps.md#compress-a-package) görüntü deposuna kopyalamadan önce.
Sıkıştırma boyutunu azaltır ve hangi sırayla trafik miktarını azaltır ve bu Service Fabric çalışma dosyaları sayısı gerçekleştirmeniz gerekir. Karşıya yükleme işlemi (özellikle sıkıştırma zaman eklerseniz) daha yavaş olabilir, ancak kaydetme ve uygulama kaydı türü daha hızlı.
- İçin daha büyük bir zaman belirtmek [ProvisionApplicationAsync](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.provisionapplicationasync) API'SİYLE `timeout` parametresi.

### <a name="deploy-application-package-with-many-files"></a>Çok sayıda dosya içeren uygulama paketini dağıtma
Sorun: [ProvisionApplicationAsync](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.provisionapplicationasync) zaman aşımına uğraması için uygulama paketi (binlerce siparişi) çok sayıda dosya içeren.
Deneyin:
- [Paket sıkıştırma](service-fabric-package-apps.md#compress-a-package) görüntü deposuna kopyalamadan önce. Sıkıştırma dosyalarının sayısını azaltır.
- İçin daha büyük bir zaman belirtmek [ProvisionApplicationAsync](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.provisionapplicationasync) ile `timeout` parametresi.

## <a name="code-example"></a>Kod örneği
Aşağıdaki örnek, bir uygulama paketi görüntü deposuna kopyalar ve uygulama türünün sağlar. Ardından, örnek bir uygulama örneği oluşturur ve bir hizmet örneği oluşturur. Son olarak, örnek uygulama örneğini kaldırır, uygulama türünün sağlamasını kaldırır ve görüntü deposundan uygulama paketini siler.

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Reflection;
using System.Threading.Tasks;

using System.Fabric;
using System.Fabric.Description;
using System.Threading;

namespace ServiceFabricAppLifecycle
{
class Program
{
static void Main(string[] args)
{

    string clusterConnection = "localhost:19000";
    string appName = "fabric:/MyApplication";
    string appType = "MyApplicationType";
    string appVersion = "1.0.0";
    string serviceName = "fabric:/MyApplication/Stateless1";
    string imageStoreConnectionString = "file:C:\\SfDevCluster\\Data\\ImageStoreShare";
    string packagePathInImageStore = "MyApplication";
    string packagePath = "C:\\Users\\username\\Documents\\Visual Studio 2019\\Projects\\MyApplication\\MyApplication\\pkg\\Debug";
    string serviceType = "Stateless1Type";

    // Connect to the cluster.
    FabricClient fabricClient = new FabricClient(clusterConnection);

    // Copy the application package to a location in the image store
    try
    {
        fabricClient.ApplicationManager.CopyApplicationPackage(imageStoreConnectionString, packagePath, packagePathInImageStore);
        Console.WriteLine("Application package copied to {0}", packagePathInImageStore);
    }
    catch (AggregateException ae)
    {
        Console.WriteLine("Application package copy to Image Store failed: ");
        foreach (Exception ex in ae.InnerExceptions)
        {
            Console.WriteLine("HResult: {0} Message: {1}", ex.HResult, ex.Message);
        }
    }

    // Provision the application.  "MyApplicationV1" is the folder in the image store where the application package is located. 
    // The application type with name "MyApplicationType" and version "1.0.0" (both are found in the application manifest) 
    // is now registered in the cluster.            
    try
    {
        fabricClient.ApplicationManager.ProvisionApplicationAsync(packagePathInImageStore).Wait();

        Console.WriteLine("Provisioned application type {0}", packagePathInImageStore);
    }
    catch (AggregateException ae)
    {
        Console.WriteLine("Provision Application Type failed:");

        foreach (Exception ex in ae.InnerExceptions)
        {
            Console.WriteLine("HResult: {0} Message: {1}", ex.HResult, ex.Message);
        }
    }

    // Delete the application package from a location in the image store.
    try
    {
        fabricClient.ApplicationManager.RemoveApplicationPackage(imageStoreConnectionString, packagePathInImageStore);
        Console.WriteLine("Application package removed from {0}", packagePathInImageStore);
    }
    catch (AggregateException ae)
    {
        Console.WriteLine("Application package removal from Image Store failed: ");
        foreach (Exception ex in ae.InnerExceptions)
        {
            Console.WriteLine("HResult: {0} Message: {1}", ex.HResult, ex.Message);
        }
    }

    //  Create the application instance.
    try
    {
        ApplicationDescription appDesc = new ApplicationDescription(new Uri(appName), appType, appVersion);
        fabricClient.ApplicationManager.CreateApplicationAsync(appDesc).Wait();
        Console.WriteLine("Created application instance of type {0}, version {1}", appType, appVersion);
    }
    catch (AggregateException ae)
    {
        Console.WriteLine("CreateApplication failed.");
        foreach (Exception ex in ae.InnerExceptions)
        {
            Console.WriteLine("HResult: {0} Message: {1}", ex.HResult, ex.Message);
        }
    }

    // Create the stateless service description.  For stateful services, use a StatefulServiceDescription object.
    StatelessServiceDescription serviceDescription = new StatelessServiceDescription();
    serviceDescription.ApplicationName = new Uri(appName);
    serviceDescription.InstanceCount = 1;
    serviceDescription.PartitionSchemeDescription = new SingletonPartitionSchemeDescription();
    serviceDescription.ServiceName = new Uri(serviceName);
    serviceDescription.ServiceTypeName = serviceType;

    // Create the service instance.  If the service is declared as a default service in the ApplicationManifest.xml,
    // the service instance is already running and this call will fail.
    try
    {
        fabricClient.ServiceManager.CreateServiceAsync(serviceDescription).Wait();
        Console.WriteLine("Created service instance {0}", serviceName);
    }
    catch (AggregateException ae)
    {
        Console.WriteLine("CreateService failed.");
        foreach (Exception ex in ae.InnerExceptions)
        {
            Console.WriteLine("HResult: {0} Message: {1}", ex.HResult, ex.Message);
        }
    }

    // Delete a service instance.
    try
    {
        DeleteServiceDescription deleteServiceDescription = new DeleteServiceDescription(new Uri(serviceName));

        fabricClient.ServiceManager.DeleteServiceAsync(deleteServiceDescription);
        Console.WriteLine("Deleted service instance {0}", serviceName);
    }
    catch (AggregateException ae)
    {
        Console.WriteLine("DeleteService failed.");
        foreach (Exception ex in ae.InnerExceptions)
        {
            Console.WriteLine("HResult: {0} Message: {1}", ex.HResult, ex.Message);
        }
    }

    // Delete an application instance from the application type.
    try
    {
        DeleteApplicationDescription deleteApplicationDescription = new DeleteApplicationDescription(new Uri(appName));

        fabricClient.ApplicationManager.DeleteApplicationAsync(deleteApplicationDescription).Wait();
        Console.WriteLine("Deleted application instance {0}", appName);
    }
    catch (AggregateException ae)
    {
        Console.WriteLine("DeleteApplication failed.");
        foreach (Exception ex in ae.InnerExceptions)
        {
            Console.WriteLine("HResult: {0} Message: {1}", ex.HResult, ex.Message);
        }
    }

    // Un-provision the application type.
    try
    {
        fabricClient.ApplicationManager.UnprovisionApplicationAsync(appType, appVersion).Wait();
        Console.WriteLine("Un-provisioned application type {0}, version {1}", appType, appVersion);
    }
    catch (AggregateException ae)
    {
        Console.WriteLine("Un-provision application type failed: ");
        foreach (Exception ex in ae.InnerExceptions)
        {
            Console.WriteLine("HResult: {0} Message: {1}", ex.HResult, ex.Message);
        }
    }

    Console.WriteLine("Hit enter...");
    Console.Read();
}        
}
}

```

## <a name="next-steps"></a>Sonraki adımlar
[Service Fabric uygulaması yükseltme](service-fabric-application-upgrade.md)

[Service Fabric sistem durumu giriş](service-fabric-health-introduction.md)

[Tanılama ve sorun giderme bir Service Fabric hizmeti](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)

[Bir Service Fabric uygulamasında model](service-fabric-application-model.md)

<!--Link references--In actual articles, you only need a single period before the slash-->
[10]: service-fabric-package-apps.md
[11]: service-fabric-application-upgrade.md
