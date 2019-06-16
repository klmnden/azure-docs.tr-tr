---
title: Dönüştürme Azure Cloud Services uygulamalarını Service fabric'e | Microsoft Docs
description: Bu kılavuz, Cloud Services'tan Service Fabric'e geçme yardımcı olmak için Cloud Services Web ve çalışan rolleri ve Service Fabric durum bilgisi olmayan hizmetler karşılaştırılmaktadır.
services: service-fabric
documentationcenter: .net
author: vturecek
manager: chackdan
editor: ''
ms.assetid: 5880ebb3-8b54-4be8-af4b-95a1bc082603
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/02/2017
ms.author: vturecek
ms.openlocfilehash: 10fb44b0e76282ad78e7687beaa2e50e819e5cd9
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "62110021"
---
# <a name="guide-to-converting-web-and-worker-roles-to-service-fabric-stateless-services"></a>Web ve çalışan rolleri için Service Fabric durum bilgisi olmayan hizmetler dönüştürme Kılavuzu
Bu makalede, Cloud Services Web ve çalışan rolleri için Service Fabric durum bilgisi olmayan hizmetler geçirmeyi açıklar. Bu en basit geçiş bulut hizmetlerinden Service fabric'e genel, mimarisi, yaklaşık aynı kalmasını geçiyor uygulamalar için yoludur.

## <a name="cloud-service-project-to-service-fabric-application-project"></a>Service Fabric uygulaması projesi için bulut hizmeti projesi
 Bir bulut hizmeti projesini ve Service Fabric uygulaması projesini benzer bir yapıya sahiptir ve hem temsil uygulamanızı çalıştırmak için dağıtılan tam paketi uygulamanız için - diğer bir deyişle, bunlar her dağıtım birimi tanımlayın. Bir bulut hizmeti projesi, bir veya daha fazla Web veya çalışan rolleri içerir. Benzer şekilde, bir Service Fabric uygulaması projesi bir veya daha fazla hizmet içeriyor. 

Service Fabric uygulaması projesi için bir dizi dağıtılacak bir uygulama yalnızca tanımlar. oysa bulut hizmeti projesi bir VM dağıtımı ile uygulama dağıtımı couples ve bu nedenle, VM yapılandırma ayarlarını içeren fark vardır bir Service Fabric kümesinde var olan VM'ler. Service Fabric kümesi yalnızca bir kez bir Resource Manager şablonu aracılığıyla veya Azure Portalı aracılığıyla dağıtılır ve birden çok Service Fabric uygulamaları dağıtılabilir.

![Service Fabric ve Cloud Services projesi karşılaştırması][3]

## <a name="worker-role-to-stateless-service"></a>Çalışan rolü için durum bilgisi olmayan hizmet
Kavramsal olarak, bir çalışan rolü istekleri herhangi bir zamanda herhangi bir örneğine yönlendirilebilir ve iş yükü'nin her örneğinin aynı anlamı durum bilgisi olmayan bir iş yükünü temsil eder. Her örnek, önceki isteği hatırlamak beklenmiyor. İş yükü çalışır duruma Azure tablo depolama veya Azure Document DB gibi bir dış durumlarını mağaza tarafından yönetilir. Service Fabric'te, iş yükü bu tür bir durum bilgisi olmayan hizmet tarafından temsil edilir. Bir çalışan rolü Service Fabric'e geçiş için en kolay yaklaşım, durum bilgisi olmayan hizmete çalışan rolü kodunu dönüştürerek yapılabilir.

![Çalışan rolü için durum bilgisi olmayan hizmet][4]

## <a name="web-role-to-stateless-service"></a>Durum bilgisi olmayan hizmet için Web rolü
Benzer şekilde çalışan rolü, Web rolü de durum bilgisi olmayan bir iş yükünü temsil eder ve bu nedenle, kavramsal olarak, çok bir Service Fabric durum bilgisi olmayan hizmet eşlenebilir. Ancak, Web rolünden farklı olarak, IIS Service Fabric desteklemez. Bir web geçirmek için IIS veya ASP.NET Core 1 gibi bir System.Web bağlı değildir ve şirket içinde barındırılan bir web çerçevesi ilk taşıma Web rolü bir uygulamadan bir durum bilgisi olmayan hizmete gerektirir.

| **Uygulama** | **Destekleniyor** | **Geçiş yolu** |
| --- | --- | --- |
| ASP.NET Web formları |Hayır |1 ASP.NET Core MVC için Dönüştür |
| ASP.NET MVC |İle geçiş |Yükseltme ASP.NET Core MVC 1 |
| ASP.NET Web API'si |İle geçiş |Şirket içinde barındırılan bir sunucu veya ASP.NET Core 1 kullanın |
| ASP.NET Core 1 |Evet |Yok |

## <a name="entry-point-api-and-lifecycle"></a>Giriş noktası API ve yaşam döngüsü
Çalışan rolü ve Service Fabric API'leri teklif benzer giriş noktaları hizmeti: 

| **Giriş noktası** | **Çalışan rolü** | **Service Fabric hizmeti** |
| --- | --- | --- |
| İşleniyor |`Run()` |`RunAsync()` |
| VM Başlat |`OnStart()` |Yok |
| VM durdurma |`OnStop()` |Yok |
| İstemci istekleri için açık dinleyici |Yok |<ul><li> `CreateServiceInstanceListener()` için durum bilgisi olmayan</li><li>`CreateServiceReplicaListener()` durum bilgisi olan için</li></ul> |

### <a name="worker-role"></a>Çalışan rolü
```csharp

using Microsoft.WindowsAzure.ServiceRuntime;

namespace WorkerRole1
{
    public class WorkerRole : RoleEntryPoint
    {
        public override void Run()
        {
        }

        public override bool OnStart()
        {
        }

        public override void OnStop()
        {
        }
    }
}

```

### <a name="service-fabric-stateless-service"></a>Service Fabric durum bilgisi olmayan hizmet
```csharp

using System.Collections.Generic;
using System.Threading;
using System.Threading.Tasks;
using Microsoft.ServiceFabric.Services.Communication.Runtime;
using Microsoft.ServiceFabric.Services.Runtime;

namespace Stateless1
{
    public class Stateless1 : StatelessService
    {
        protected override IEnumerable<ServiceInstanceListener> CreateServiceInstanceListeners()
        {
        }

        protected override Task RunAsync(CancellationToken cancelServiceInstance)
        {
        }
    }
}

```

Her ikisi de, işleme başlamak bir birincil "Çalıştır" geçersiz kılma sahiptir. Service Fabric Hizmetleri birleştirme `Run`, `Start`, ve `Stop` tek Giriş noktasına `RunAsync`. Hizmetinizi ne zaman çalışmaya başlayacak `RunAsync` başlar ve ne zaman çalışmayı durdurmasına `RunAsync` yöntemin CancellationToken sinyal. 

Yaşam döngüsü ve çalışan rolleri ve Service Fabric Hizmetleri ömrünü arasında bazı önemli farklılıklar vardır:

* **Yaşam döngüsü:** En büyük fark, bir çalışan rolü bir vm'dir ve bu nedenle yaşam döngüsü için VM'yi başlatır ve durdurur, olayları içeren VM bağlıdır ' dir. Bir Service Fabric hizmeti, ilgili olmayan konak VM veya makine ne zaman başlar ve durdurmak için olayları içermez, VM yaşam döngüsü ' ayrı bir yaşam döngüsüne sahiptir.
* **Yaşam süresi:** Bir çalışan rolü örneği, dönüşüm `Run` yöntemi çıkar. `RunAsync` Yöntemi bir Service Fabric hizmetinde ancak tamamlanana kadar çalışması ve hizmet örneği oluşturan kalır. 

Service Fabric Hizmetleri için istemci isteklerini dinlemek için bir isteğe bağlı iletişim Kurulumu giriş noktası sağlar. RunAsync ve iletişimi giriş noktası olan RunAsync yönteminde başlatmadan çıkmak için izin verilir neden olan Service Fabric Hizmetleri - hizmetinizi yalnızca istemci isteklerini dinleyecek veya yalnızca bir işleme döngüsü veya her ikisini de çalıştırın seçebilir - isteğe bağlı bir geçersiz kılma Hizmet örneği, çünkü istemci isteklerini dinlemek devam edebilir.

## <a name="application-api-and-environment"></a>Uygulama API ve ortam
Bulut Hizmetleri ortamı API, bilgi ve diğer VM rol örnekleriyle ilgili bilgilerin yanı sıra geçerli sanal makine örneği için işlevsellik sağlar. Service Fabric, çalışma zamanı için ilgili bilgiler sağlar ve bir hizmet düğümü hakkında bazı bilgiler şu anda çalışıyor. 

| **Ortam görevi** | **Cloud Services** | **Service Fabric** |
| --- | --- | --- |
| Yapılandırma ayarlarını ve değişiklik bildirimi |`RoleEnvironment` |`CodePackageActivationContext` |
| Yerel Depolama |`RoleEnvironment` |`CodePackageActivationContext` |
| Uç nokta bilgileri |`RoleInstance` <ul><li>Geçerli örneğin: `RoleEnvironment.CurrentRoleInstance`</li><li>Diğer roller ve örneği: `RoleEnvironment.Roles`</li> |<ul><li>`NodeContext` Geçerli düğüm adresi</li><li>`FabricClient` ve `ServicePartitionResolver` hizmet uç noktası bulma</li> |
| Ortam öykünme |`RoleEnvironment.IsEmulated` |Yok |
| Eş zamanlı değişiklik olayı |`RoleEnvironment` |Yok |

## <a name="configuration-settings"></a>Yapılandırma ayarları
Cloud Services yapılandırma ayarlarında bir VM rolü için ayarlanır ve bu VM rolü'nün tüm örnekleri için geçerlidir. Bu ayarlar, anahtar-değer çiftleridir ServiceConfiguration.*.cscfg dosyalarında ayarlayın ve doğrudan RoleEnvironment erişilebilir. Bir VM'nin birden çok hizmet ve uygulamaları barındırabilir, Service Fabric ayarları tek tek her hizmet ve her bir uygulama yerine bir VM geçerlidir. Hizmet üç paketlerini oluşur:

* **Kod:** hizmeti yürütülebilir dosyaları, ikili dosyaları, DLL'ler ve çalıştırmak için bir hizmet gereken dosyaları içerir.
* **Config:** tüm yapılandırma dosyalarını ve ayarlarını bir hizmet.
* **Veri:** statik veri dosyalarının hizmetle ilişkili.

Bu paketlerin her bağımsız olarak sürümü oluşturulabilir ve yükseltilebilir. Bulut hizmetlerine benzer bir yapılandırma paketi programlı bir API aracılığıyla erişilebilen ve olayları hizmeti, bir yapılandırma paketi değişikliği bildirmek kullanılabilir. Bir Settings.xml dosyasının, anahtar-değer yapılandırma ve programlı erişim bir App.config dosyası uygulama ayarları bölümüne benzer için kullanılabilir. Ancak, XML, YAML, JSON ya da özel bir ikili biçimi olup olmadığını bulut hizmetlerinin aksine, bir Service Fabric config paketi herhangi bir biçimdeki tüm yapılandırma dosyaları içerebilir. 

### <a name="accessing-configuration"></a>Erişimi yapılandırma
#### <a name="cloud-services"></a>Cloud Services
ServiceConfiguration.*.cscfg aracılığıyla yapılandırma ayarlarınızı üzerinden erişilebilir `RoleEnvironment`. Bu ayarlar, tüm rol örneklerinin aynı bulut hizmeti dağıtımı için genel olarak kullanılabilir.

```csharp

string value = RoleEnvironment.GetConfigurationSettingValue("Key");

```

#### <a name="service-fabric"></a>Service Fabric
Her hizmet kendi ayrı yapılandırma paketi içerir. Yerleşik bir mekanizma bulunmamaktadır genel yapılandırma ayarları için erişilebilir bir kümedeki tüm uygulamalar tarafından. Service Fabric'in özel Settings.xml yapılandırma dosyası içinde bir yapılandırma paketini kullanırken, uygulama düzeyinde yapılandırma ayarları edinerek uygulama düzeyinde Settings.xml değerleri üzerine yazılabilir.

Yapılandırma ayarları olan erişen her bir hizmet örneği aracılığıyla hizmetin içinde `CodePackageActivationContext`.

```csharp

ConfigurationPackage configPackage = this.Context.CodePackageActivationContext.GetConfigurationPackageObject("Config");

// Access Settings.xml
KeyedCollection<string, ConfigurationProperty> parameters = configPackage.Settings.Sections["MyConfigSection"].Parameters;

string value = parameters["Key"]?.Value;

// Access custom configuration file:
using (StreamReader reader = new StreamReader(Path.Combine(configPackage.Path, "CustomConfig.json")))
{
    MySettings settings = JsonConvert.DeserializeObject<MySettings>(reader.ReadToEnd());
}

```

### <a name="configuration-update-events"></a>Yapılandırma güncelleştirme olayları
#### <a name="cloud-services"></a>Cloud Services
`RoleEnvironment.Changed` Olay ortamında, bir yapılandırma değişikliği gibi bir değişiklik meydana geldiğinde, tüm rol örneklerine bildirmek için kullanılır. Bu yapılandırma güncelleştirmeleri rol örneklerine geri dönüştürerek ya da bir çalışan sürecin yeniden başlatılarak olmadan kullanmak için kullanılır.

```csharp

RoleEnvironment.Changed += RoleEnvironmentChanged;

private void RoleEnvironmentChanged(object sender, RoleEnvironmentChangedEventArgs e)
{
   // Get the list of configuration changes
   var settingChanges = e.Changes.OfType<RoleEnvironmentConfigurationSettingChange>();
foreach (var settingChange in settingChanges) 
   {
      Trace.WriteLine("Setting: " + settingChange.ConfigurationSettingName, "Information");
   }
}

```

#### <a name="service-fabric"></a>Service Fabric
-Kod, yapılandırma ve verileri - bir hizmette üç paket türlerinin her biri bir hizmet örneği bir paket güncelleştirildi, eklenen veya kaldırıldığında bildiren olaylar vardır. Bir hizmet birden çok paket her tür içerebilir. Örneğin, bir hizmet birden çok yapılandırma paketleri, tek tek her tutulan ve yükseltilebilir olabilir. 

Bu olaylar hizmet örneği yeniden başlatmanıza gerek kalmadan hizmet paketleri değişiklikleri kullanmak kullanılabilir.

```csharp

this.Context.CodePackageActivationContext.ConfigurationPackageModifiedEvent +=
                    this.CodePackageActivationContext_ConfigurationPackageModifiedEvent;

private void CodePackageActivationContext_ConfigurationPackageModifiedEvent(object sender, PackageModifiedEventArgs<ConfigurationPackage> e)
{
    this.UpdateCustomConfig(e.NewPackage.Path);
    this.UpdateSettings(e.NewPackage.Settings);
}

```

## <a name="startup-tasks"></a>Başlangıç görevleri
Başlangıç, bir uygulama başlatılmadan önce gerçekleştirilen eylemler görevlerdir. Başlangıç görevi, genellikle yükseltilmiş ayrıcalıklar kullanarak kurulum komut dosyalarını çalıştırmak için kullanılır. Hem bulut hizmetlerini hem de Service Fabric başlangıç görevleri destekler. İkisi arasındaki temel fark, bir rol örneğinin parçası olduğu için Service Fabric başlangıç görevi belirli bir sanal Makineye bağlı değildir bir hizmete bağlıdır ancak bulut Hizmetleri'nde bir başlangıç görevi bir VM'ye bağlı olduğunu ' dir.

| Service Fabric | Cloud Services |
| --- | --- |
| Yapılandırma konumu |ServiceDefinition.csdef |
| Ayrıcalıkları |"kısıtlı" veya "yükseltilmiş" |
| Sıralama |"Basit", "arka plan", "ön" |

### <a name="cloud-services"></a>Cloud Services
Bulut Hizmetleri'nde bir başlangıç giriş noktası ServiceDefinition.csdef rol başına yapılandırılır. 

```xml

<ServiceDefinition>
    <Startup>
        <Task commandLine="Startup.cmd" executionContext="limited" taskType="simple" >
            <Environment>
                <Variable name="MyVersionNumber" value="1.0.0.0" />
            </Environment>
        </Task>
    </Startup>
    ...
</ServiceDefinition>

```

### <a name="service-fabric"></a>Service Fabric
Service Fabric başlangıç giriş noktası ServiceManifest.xml hizmetinde başına yapılandırılır:

```xml

<ServiceManifest>
  <CodePackage Name="Code" Version="1.0.0">
    <SetupEntryPoint>
      <ExeHost>
        <Program>Startup.bat</Program>
      </ExeHost>
    </SetupEntryPoint>
    ...
</ServiceManifest>

``` 

## <a name="a-note-about-development-environment"></a>Geliştirme ortamı hakkında bir Not
Hem bulut hizmetlerini hem de Service Fabric ile Visual Studio Proje şablonları ve hata ayıklama, yapılandırma ve hem de yerel olarak ve dağıtımı için Azure desteği ile tümleştirilmiştir. Hem bulut hizmetlerini hem de Service Fabric aynı zamanda bir yerel geliştirme çalışma zamanı ortamı sağlar. Fark vardır: Bulut hizmeti geliştirme çalışma zamanı üzerinde çalışan Azure ortamına öykünür olsa da Service Fabric, bir öykünücü kullanmaz - tam Service Fabric çalışma zamanı kullanır. Yerel geliştirme makinenizde kurulmayı Service Fabric ortam üretim ortamında çalışan aynı ortamıdır.

## <a name="next-steps"></a>Sonraki adımlar
Service Fabric güvenilir hizmetler hakkında daha fazla bilgi ve eksiksiz bir Service Fabric özelliklerden yararlanmak nasıl anlamak için Cloud Services ve Service Fabric uygulama mimarisi arasındaki temel farklar.

* [Service Fabric güvenilir hizmetler ile çalışmaya başlama](service-fabric-reliable-services-quick-start.md)
* [Cloud Services ve Service Fabric arasındaki farklar için kavramsal Kılavuzu](service-fabric-cloud-services-migration-differences.md)

<!--Image references-->
[3]: ./media/service-fabric-cloud-services-migration-worker-role-stateless-service/service-fabric-cloud-service-projects.png
[4]: ./media/service-fabric-cloud-services-migration-worker-role-stateless-service/worker-role-to-stateless-service.png
