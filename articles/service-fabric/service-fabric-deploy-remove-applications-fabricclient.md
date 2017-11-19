---
title: "Azure Service Fabric uygulama dağıtımı | Microsoft Docs"
description: "FabricClient API'ları dağıtmak ve Service Fabric uygulamaları kaldırmak için kullanın."
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: 
ms.assetid: b120ffbf-f1e3-4b26-a492-347c29f8f66b
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 10/05/2017
ms.author: ryanwi
ms.openlocfilehash: 6d737e354f5e7ee57c2e2c3d9b5599d4ba2b09af
ms.sourcegitcommit: 732e5df390dea94c363fc99b9d781e64cb75e220
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/14/2017
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

Bir kez bir [uygulama türü paketlenmiş][10], Azure Service Fabric kümesi içine dağıtımı için hazırdır. Dağıtım aşağıdaki üç adımdan oluşur:

1. Uygulama paketi görüntü deposuna karşıya yükleme
2. Uygulama türünü kaydetme
3. Uygulama paketi görüntü deposundan kaldırır
4. Uygulama örneği oluşturma

Bir uygulamanın dağıtıldığını ve bir örnek kümede çalışan sonra uygulama örneği ve uygulama türünü silebilirsiniz. Bir uygulamayı kümeden tamamen kaldırmak için aşağıdaki adımları içerir:

1. Çalışan Kaldır (veya Sil) uygulama örneği
2. Artık ihtiyacınız varsa uygulama türü kaydını kaldırma

Dağıtma ve yerel geliştirme kümenizde uygulamalarında hata ayıklama için Visual Studio kullanıyorsanız, yukarıdaki adımların tümünü otomatik olarak bir PowerShell komut dosyası gerçekleştirilir.  Bu komut dosyası içinde bulunur *betikleri* uygulama projesi klasörü. Bu makalede, Visual Studio dışında aynı işlemleri gerçekleştirebilmeleri için komut dosyası yaptıklarını üzerinde arka plan sağlar. 
 
## <a name="connect-to-the-cluster"></a>Kümeye bağlanma
Oluşturarak kümeye bağlanın bir [FabricClient](/dotnet/api/system.fabric.fabricclient) bu makaledeki kod örnekleri birini çalıştırmadan önce örneği. Yerel bir geliştirme kümesi veya bir uzak küme veya Azure Active Directory, X509 kullanılarak güvenlik altına kümeye bağlanma örnekleri için sertifikalar veya Windows Active Directory bkz [güvenli kümeye Bağlan](service-fabric-connect-to-secure-cluster.md#connect-to-a-cluster-using-the-fabricclient-apis). Yerel geliştirme kümeye bağlanmak için şu komutu çalıştırın:

```csharp
// Connect to the local cluster.
FabricClient fabricClient = new FabricClient();
```

## <a name="upload-the-application-package"></a>Uygulama paketi yükleme
Derleme ve adlı bir uygulama paketi varsayalım *MyApplication* Visual Studio. Varsayılan olarak, ApplicationManifest.xml listelenen uygulama türü adı "MyApplicationType" bağlıdır.  Gerekli uygulama bildirimini, hizmet bildirimlerini ve kod/config/veri paketleri içeren uygulama paketi bulunan *C:\Users\&lt; kullanıcı adı&gt;\Documents\Visual Studio 2017\Projects\ MyApplication\MyApplication\pkg\Debug*.

Uygulama paketini karşıya yükleme, iç Service Fabric bileşenleri tarafından erişilebilen bir konuma koyan. Service Fabric uygulama paketi kaydı sırasında uygulama paketini doğrular. Ancak, uygulama paketi yerel olarak (yani, yüklemeden önce) doğrulamak istiyorsanız, kullanmak [Test ServiceFabricApplicationPackage](/powershell/module/servicefabric/test-servicefabricapplicationpackage?view=azureservicefabricps) cmdlet'i.

[CopyApplicationPackage](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.copyapplicationpackage) API küme görüntü deposu için uygulama paketi yükler. 

Uygulama paketi büyük ve/veya birçok dosyaları yoksa, şunları yapabilirsiniz [onu Sıkıştır](service-fabric-package-apps.md#compress-a-package) ve PowerShell kullanarak görüntü deposuna kopyalama. Sıkıştırma, boyutu ve dosya sayısını azaltır.

Bkz: [görüntü deposu bağlantı dizesi anlamak](service-fabric-image-store-connection-string.md) resim ve görüntü deposu hakkında ek bilgi için bağlantı dizesi depolar.

## <a name="register-the-application-package"></a>Uygulama paketi Kaydet
Uygulama türü ve sürümü, uygulama paketi kaydedildikten sonra kullanılabilir hale uygulama bildiriminde bildirildi. Sistem önceki adımda karşıya yüklenen paket okur, paket doğrular, paket içeriğini işler ve işlenen paket bir iç sistem konumuna kopyalar.  

[ProvisionApplicationAsync](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.provisionapplicationasync) uygulama kümede yazın ve dağıtım kullanımına API kaydeder.

[GetApplicationTypeListAsync](/dotnet/api/system.fabric.fabricclient.queryclient.getapplicationtypelistasync) API tüm başarıyla kayıtlı uygulama türleri hakkında bilgi sağlar. Bu API, kaydı yapıldığında belirlemek için kullanabilirsiniz.

## <a name="remove-an-application-package-from-the-image-store"></a>Bir uygulama paketi görüntü deposundan kaldırır
Uygulama başarıyla kaydedildikten sonra uygulama paketini kaldırmanız önerilir.  Uygulama paketleri görüntü deposundan silme sistem kaynakları serbest bırakır.  Kullanılmayan uygulama paketleri tutma disk depolama alanı kullanır ve uygulama performans sorunlarına yol açar. Kullanarak görüntü deposu uygulama paketini silmeniz [RemoveApplicationPackage](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.removeapplicationpackage) API.

## <a name="create-an-application-instance"></a>Uygulama örneğini oluşturma
Bir uygulama kullanarak başarıyla kaydettirildi herhangi bir uygulama türü örneği [CreateApplicationAsync](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.createapplicationasync) API. Her bir uygulama adı ile başlamalıdır *"fabric:"* düzen ve her uygulama örneği (küme içinde) benzersiz olmalıdır. Hedef uygulama türü uygulama bildiriminde tanımlanan varsayılan hizmetlerin de oluşturulur.

Verilen herhangi bir kayıtlı uygulama türü sürümü için birden çok uygulama örneği oluşturulabilir. Her uygulama örneği yalıtımı, kendi çalışma dizini ve işlemleri kümesi ile çalışır.

Hangi adlı görmek için uygulamalar ve hizmetler çalıştırmak kümede çalışan [GetApplicationListAsync](/dotnet/api/system.fabric.fabricclient.queryclient.getapplicationlistasync) ve [GetServiceListAsync](/dotnet/api/system.fabric.fabricclient.queryclient.getservicelistasync) API'leri.

## <a name="create-a-service-instance"></a>Bir hizmet örneği oluşturma
Bir hizmet türünü kullanarak bir hizmet örneği [CreateServiceAsync](/dotnet/api/system.fabric.fabricclient.servicemanagementclient.createserviceasync) API.  Hizmet, varsayılan hizmet uygulama bildiriminde olarak bildirilirse, uygulama örneği oluşturulduğunda hizmet örneği.  Çağırma [CreateServiceAsync](/dotnet/api/system.fabric.fabricclient.servicemanagementclient.createserviceasync) API zaten örneği bir hizmet için bir hata kodu FabricErrorCode.ServiceAlreadyExists değerini içeren FabricException türünde bir özel durum döndürür.

## <a name="remove-a-service-instance"></a>Bir hizmet örneği Kaldır
Bir hizmet örneği artık gerekli olmadığında çalışmasını kaldırabilirsiniz çağırarak uygulama örneği [DeleteServiceAsync](/dotnet/api/system.fabric.fabricclient.servicemanagementclient.deleteserviceasync) API.  

> [!WARNING]
> Bu işlem geri alınamaz ve hizmet durumu kurtarılamıyor.

## <a name="remove-an-application-instance"></a>Bir uygulama örneğini kaldırma
Uygulama örneğini artık gerekli olmadığında kalıcı olarak adını kullanarak kaldırabilirsiniz [DeleteApplicationAsync](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.deleteapplicationasync) API. [DeleteApplicationAsync](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.deleteapplicationasync) tüm hizmet durumunu da, kalıcı olarak kaldırma uygulamaya ait tüm hizmetleri otomatik olarak kaldırır.

> [!WARNING]
> Bu işlem geri alınamaz ve uygulama durumu kurtarılamıyor.

## <a name="unregister-an-application-type"></a>Bir uygulama türü kaydını kaldırma
Belirli bir uygulama türü sürümü artık gerekli olduğunda, uygulama türünü kullanarak bu belirli sürümü kaydı [Unregister-ServiceFabricApplicationType](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.unprovisionapplicationasync) API. Kullanılmayan uygulama türleri sürümleri kaydını Image store tarafından kullanılan depolama alanını serbest bırakır. Uygulama türü sürümü, uygulama bu uygulama türü sürümü karşı örneği ve hiçbir bekleyen uygulama yükseltmesi bu uygulama türü sürümü başvurduğunuzdan sürece kaydı olabilir.

## <a name="troubleshooting"></a>Sorun giderme
### <a name="copy-servicefabricapplicationpackage-asks-for-an-imagestoreconnectionstring"></a>Kopya ServiceFabricApplicationPackage bir ImageStoreConnectionString için sorar
Service Fabric SDK ortam zaten ayarlanmış doğru Varsayılanları olması gerekir. Ancak gerekirse, tüm komutlar için ImageStoreConnectionString Service Fabric kümesi kullanarak değer eşleşmesi gerekir. Küme bildiriminde ImageStoreConnectionString bulabilirsiniz kullanarak alınan [Get-ServiceFabricClusterManifest](/powershell/module/servicefabric/get-servicefabricclustermanifest?view=azureservicefabricps) ve Get-ImageStoreConnectionStringFromClusterManifest komutlar:

```powershell
PS C:\> Get-ImageStoreConnectionStringFromClusterManifest(Get-ServiceFabricClusterManifest)
```

**Get-ImageStoreConnectionStringFromClusterManifest** Service Fabric SDK PowerShell modülünün bir parçası olan cmdlet, görüntü deposu bağlantı dizesini almak için kullanılır.  SDK modülü içeri aktarmak için aşağıdakini çalıştırın:

```powershell
Import-Module "$ENV:ProgramFiles\Microsoft SDKs\Service Fabric\Tools\PSModule\ServiceFabricSDK\ServiceFabricSDK.psm1"
```


ImageStoreConnectionString küme bildiriminde bulunur:

```xml
<ClusterManifest xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" Name="Server-Default-SingleNode" Version="1.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">

    [...]

    <Section Name="Management">
      <Parameter Name="ImageStoreConnectionString" Value="file:D:\ServiceFabric\Data\ImageStore" />
    </Section>

    [...]
```

Bkz: [görüntü deposu bağlantı dizesi anlamak](service-fabric-image-store-connection-string.md) resim ve görüntü deposu hakkında ek bilgi için bağlantı dizesi depolar.

### <a name="deploy-large-application-package"></a>Büyük uygulama paketini dağıtma
Sorun: [CopyApplicationPackage](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.copyapplicationpackage) API zaman aşımına uğruyor büyük uygulama paketi (GB sırasını).
Deneyin:
- Daha büyük zaman aşımını belirtmek [CopyApplicationPackage](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.copyapplicationpackage) yöntemi ile `timeout` parametresi. Varsayılan olarak, zaman aşımı 30 dakikadır.
- Kaynak makine ve küme arasındaki ağ bağlantısını denetleyin. Bağlantı yavaş ise, bir makine daha iyi bir ağ bağlantısı ile kullanmayı düşünün.
İstemci makine küme başka bir bölgede ise, bir istemci makine bir daha yakından ya da aynı bölgede küme olarak düşünün.
- Dış azaltma devreyi olmadığını denetleyin. Örneğin, görüntü deposu azure depolama kullanacak şekilde yapılandırıldığında, karşıya yükleme kısıtlanan.

Sorun: karşıya yükleme paketi başarıyla tamamlandı ancak [ProvisionApplicationAsync](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.provisionapplicationasync) API zaman aşımına uğradı. Deneyin:
- [Paket Sıkıştır](service-fabric-package-apps.md#compress-a-package) görüntü deposuna kopyalama önce.
Sıkıştırma küçültür ve hangi sırayla trafik miktarını azaltır ve bu Service Fabric çalışma dosyaları sayısı gerçekleştirmeniz gerekir. Karşıya yükleme işlemi (özellikle sıkıştırma zaman eklerseniz) daha yavaş olabilir, ancak kaydedin ve kaydı uygulama türü daha hızlı.
- Daha büyük zaman aşımını belirtmek [ProvisionApplicationAsync](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.provisionapplicationasync) API'si ile `timeout` parametresi.

### <a name="deploy-application-package-with-many-files"></a>Birçok dosyalarıyla uygulama paketini dağıtma
Sorun: [ProvisionApplicationAsync](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.provisionapplicationasync) zaman aşımına uğraması için uygulama paketi birçok dosyalarla (binlerce sırasını).
Deneyin:
- [Paket Sıkıştır](service-fabric-package-apps.md#compress-a-package) görüntü deposuna kopyalama önce. Sıkıştırma dosyalarının sayısını azaltır.
- Daha büyük zaman aşımını belirtmek [ProvisionApplicationAsync](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.provisionapplicationasync) ile `timeout` parametresi.

## <a name="code-example"></a>Kod örneği
Aşağıdaki örnek bir uygulama paketi görüntü deposuna kopyalar, uygulama türü sağlar, uygulama örneğini oluşturur, bir hizmet örneği oluşturur, uygulama türü kaydını hükümleri uygulama örneği kaldırır ve siler Uygulama paketi görüntü deposundan.

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
    string packagePath = "C:\\Users\\username\\Documents\\Visual Studio 2017\\Projects\\MyApplication\\MyApplication\\pkg\\Debug";
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
[Service Fabric uygulama yükseltme](service-fabric-application-upgrade.md)

[Service Fabric sistem durumu giriş](service-fabric-health-introduction.md)

[Tanılama ve bir Service Fabric hizmeti sorun giderme](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)

[Service Fabric uygulamada modeli](service-fabric-application-model.md)

<!--Link references--In actual articles, you only need a single period before the slash-->
[10]: service-fabric-application-model.md
[11]: service-fabric-application-upgrade.md
