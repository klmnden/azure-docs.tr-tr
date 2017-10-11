---
title: "Azure Cloud Services uygulamalar için mikro Dönüştür | Microsoft Docs"
description: "Bu kılavuz bulut hizmetlerinden Service Fabric geçirme yardımcı olmak için bulut Hizmetleri Web ve çalışan rolleri ve Service Fabric durum bilgisi olmayan hizmetler karşılaştırır."
services: service-fabric
documentationcenter: .net
author: vturecek
manager: timlt
editor: 
ms.assetid: 5880ebb3-8b54-4be8-af4b-95a1bc082603
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/29/2017
ms.author: vturecek
ms.openlocfilehash: 4ab1f83e88b262b1752300b2786340d9abca8154
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="guide-to-converting-web-and-worker-roles-to-service-fabric-stateless-services"></a>Web ve çalışan rolleri Service Fabric durum bilgisi olmayan hizmetler için dönüştürme Kılavuzu
Bu makalede, bulut Hizmetleri Web ve çalışan rolleri Service Fabric durum bilgisi olmayan hizmetler için nasıl geçirileceği açıklanmaktadır. Bu en basit geçiş bulut hizmetlerinden Service Fabric, genel mimarisi kabaca aynı kalmasını gittiği uygulamalar için yoludur.

## <a name="cloud-service-project-to-service-fabric-application-project"></a>Service Fabric uygulaması projesi için bulut hizmeti projesi
 Bir bulut hizmeti projesi ve bir Service Fabric uygulaması projesi benzer bir yapıya sahip ve her iki temsil dağıtım uygulamanız için - diğer bir deyişle, bunların her uygulamanızı çalıştırmak için dağıtılan eksiksiz paket tanımlamak. Bir bulut hizmeti projesi, bir veya daha fazla Web veya çalışan rolleri içerir. Benzer şekilde, bir Service Fabric uygulaması projesi bir veya daha fazla hizmet içeriyor. 

Service Fabric uygulaması proje için bir dizi dağıtılan bir uygulamayı yalnızca tanımlar ancak bulut hizmeti projesi VM dağıtımı ile uygulama dağıtımı couples ve bu nedenle, VM yapılandırma ayarlarını içeren farktır var olan sanal makineleri bir Service Fabric kümesindeki. Service Fabric kümesi yalnızca bir kez bir Resource Manager şablonu veya Azure portal aracılığıyla dağıtılır ve birden çok Service Fabric uygulamaları dağıtılabilir.

![Service Fabric ve Cloud Services proje karşılaştırma][3]

## <a name="worker-role-to-stateless-service"></a>Durum bilgisiz hizmetine çalışan rolü
Kavramsal olarak, bir çalışan rolü iş yükü, her örneği aynıdır ve istekleri herhangi bir zamanda herhangi bir örneğine yönlendirilebilir anlamına durum bilgisiz iş yükünü temsil eder. Her bir örnek, önceki isteği anımsaması beklenmiyor. İş yükü çalıştırır durumu, Azure Table Storage veya Azure belge DB gibi bir dış durum depolama alanını tarafından yönetilir. Service Fabric iş yükü bu tür bir durum bilgisiz hizmeti tarafından temsil edilir. Çalışan rolü için Service Fabric geçiş için en kolay yaklaşım, durum bilgisi olmayan bir hizmete çalışan rolü kodunu dönüştürerek yapılabilir.

![Durum bilgisiz hizmetine çalışan rolü][4]

## <a name="web-role-to-stateless-service"></a>Durum bilgisiz hizmetine Web rolü
Benzer şekilde çalışan rolü, Web rolü ayrıca durum bilgisiz iş yükünü temsil eder ve böylece kavramsal, çok bir Service Fabric durum bilgisiz hizmetine eşlenebilir. Ancak, Web rolünden farklı olarak, IIS Service Fabric desteklemez. Bir web geçirmek için kendi kendini barındırır ve IIS veya ASP.NET Core 1 gibi System.Web bağlı olmayan bir web çerçevesidir ilk taşıma Web rolünden Hizmeti'ne uygulama için bir durum bilgisiz gerektirir.

| **Uygulama** | **Destekleniyor** | **Geçiş yolu** |
| --- | --- | --- |
| ASP.NET Web formları |Hayır |ASP.NET Core 1 MVC Dönüştür |
| ASP.NET MVC |İle geçiş |ASP.NET yükseltmeye 1 MVC çekirdek |
| ASP.NET Web API'si |İle geçiş |Kendini barındıran sunucu veya ASP.NET Core 1 kullanın |
| ASP.NET Core 1 |Evet |Yok |

## <a name="entry-point-api-and-lifecycle"></a>Giriş noktası API ve yaşam döngüsü
Çalışan rolü ve Service Fabric API'leri teklif benzer giriş noktaları hizmeti: 

| **Giriş noktası** | **Çalışan rolü** | **Service Fabric hizmeti** |
| --- | --- | --- |
| İşleme |`Run()` |`RunAsync()` |
| VM Başlat |`OnStart()` |Yok |
| VM'yi Durdur |`OnStop()` |Yok |
| İstemci istekleri için açık dinleyicisi |Yok |<ul><li> `CreateServiceInstanceListener()`için durum bilgisiz</li><li>`CreateServiceReplicaListener()`durum bilgisi için</li></ul> |

### <a name="worker-role"></a>Çalışan rolü
```C#

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

### <a name="service-fabric-stateless-service"></a>Doku durum bilgisiz hizmeti
```C#

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

Hem birincil "Çalıştır" geçersiz kılma işlemeye başlamak üzere sahiptir. Service Fabric Hizmetleri birleştirme `Run`, `Start`, ve `Stop` tek bir giriş noktasına `RunAsync`. Hizmetinizi ne zaman çalışmaya başlaması gereken `RunAsync` başlar ve ne zaman çalışma durması gerektiğini `RunAsync` yöntemin CancellationToken durdurulma. 

Yaşam döngüsü ve çalışan rolleri ve Service Fabric Hizmetleri ömrü arasında birkaç temel farklılıklar vardır:

* **Yaşam döngüsü:** büyük bir VM çalışan rolü ise ve böylece kendi yaşam döngüsü bağlı zaman VM başlatır ve durdurur olaylarını içerir VM'ye farktır. Service Fabric hizmeti, birbiriyle ilgili olmayan olarak ne zaman konağı VM veya makine başlar ve durdurun, olayları içermez böylece VM döngüsü ayrı bir yaşam döngüsü sahiptir.
* **Yaşam süresi:** çalışan rolü örneği varsa geri `Run` yöntemi çıkar. `RunAsync` Bir Service Fabric hizmeti yönteminde ancak çalıştırabilirsiniz tamamlanması ve hizmet örneği oluşturan kalır. 

Service Fabric, istemci isteklerini dinlemek Hizmetleri için isteğe bağlı iletişim Kurulum giriş noktası sağlar. RunAsync ve iletişimi giriş noktası olan RunAsync yönteminde yeniden başlatmadan çıkmak için izin verilir - hizmetiniz yalnızca istemci isteklerini dinlemek veya yalnızca bir işleme döngüsü ya da her ikisini de çalıştırmak için seçebilirsiniz - Service Fabric hizmetlerini isteğe bağlı bir geçersiz kılma Hizmet örneği, çünkü istemci isteklerini dinlemek devam edebilir.

## <a name="application-api-and-environment"></a>Uygulama API ve ortam
Bulut Hizmetleri ortam API bilgileri ve geçerli VM örneği yanı sıra diğer VM rolü örneklerini hakkında bilgi için işlevsellik sağlar. Service Fabric, çalışma zamanı için ilgili bilgiler sağlar ve bir hizmet düğüm hakkında bazı bilgiler şu anda çalışıyor. 

| **Ortam görev** | **Cloud Services** | **Service Fabric** |
| --- | --- | --- |
| Yapılandırma ayarları ve değişiklik bildirimi |`RoleEnvironment` |`CodePackageActivationContext` |
| Yerel Depolama |`RoleEnvironment` |`CodePackageActivationContext` |
| Uç nokta bilgileri |`RoleInstance` <ul><li>Geçerli örnek:`RoleEnvironment.CurrentRoleInstance`</li><li>Diğer roller ve örneği:`RoleEnvironment.Roles`</li> |<ul><li>`NodeContext`Geçerli düğüm adresi</li><li>`FabricClient`ve `ServicePartitionResolver` hizmet uç noktası bulma</li> |
| Ortam öykünmesi |`RoleEnvironment.IsEmulated` |Yok |
| Eşzamanlı değişiklik olayı |`RoleEnvironment` |Yok |

## <a name="configuration-settings"></a>Yapılandırma ayarları
Bulut hizmetleri yapılandırma ayarlarında bir VM rolü için ayarlanır ve bu VM rolü tüm örnekleri için geçerlidir. Bu ayarlar, anahtar-değer çiftleri ServiceConfiguration.*.cscfg dosyalarında ayarlamak ve doğrudan RoleEnvironment erişilebilir. Bir VM birden çok hizmet ve uygulamaları barındırmak için Service Fabric içinde ayarlarını ayrı ayrı her hizmet ve her uygulama yerine bir VM uygulayın. Bir hizmet üç paketlerini oluşur:

* **Kod:** hizmeti yürütülebilir dosyaları, ikili dosyaları, DLL'ler ve bir hizmetin ihtiyaç çalıştırmak için diğer dosyaları içerir.
* **Config:** tüm yapılandırma dosyalarını ve ayarlarını bir hizmet için.
* **Veri:** statik veri dosyalarını hizmetle ilişkilendirilmiş.

Bu paketleri her bağımsız olarak sürümlü hem de yükseltilmiş olabilir. Bulut Hizmetleri için benzer bir yapılandırma paketi program aracılığıyla bir API aracılığıyla erişilebilir ve olayları bir yapılandırma paketi değişikliği hizmetine bildirmek kullanılabilir. Settings.xml dosyası anahtar-değer yapılandırması ve benzer bir App.config dosyası uygulama ayarları bölümüne program erişimi için kullanılabilir. Ancak, XML, JSON, YAML veya özel bir ikili biçimi olup bulut Hizmetleri, herhangi bir biçimdeki tüm yapılandırma dosyalarını bir Service Fabric yapılandırma paketi içerebilir. 

### <a name="accessing-configuration"></a>Erişimi yapılandırma
#### <a name="cloud-services"></a>Cloud Services
Yapılandırma ayarları ServiceConfiguration.*.cscfg üzerinden erişilebilir `RoleEnvironment`. Bu ayarlar, tüm rol örneklerinin aynı bulut hizmeti dağıtım genel olarak kullanılabilir.

```C#

string value = RoleEnvironment.GetConfigurationSettingValue("Key");

```

#### <a name="service-fabric"></a>Service Fabric
Her hizmetin kendi tek tek bir yapılandırma paketi vardır. Hiçbir yerleşik mekanizması olduğunu genel yapılandırma ayarları için erişilebilir bir kümedeki tüm uygulamalar tarafından. Service Fabric'ın özel Settings.xml yapılandırma dosyasının bir yapılandırma paketi içinde kullanırken, uygulama düzeyinde yapılandırma ayarlarını edinerek uygulama düzeyinde Settings.xml değerlerde üzerine yazılabilir.

Yapılandırma ayarları her hizmet örneği hizmetin aracılığıyla içinde erişimleri olan `CodePackageActivationContext`.

```C#

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
`RoleEnvironment.Changed` Olay ortamında, bir yapılandırma değişikliği gibi bir değişiklik meydana geldiğinde tüm rol örneklerini bildirmek için kullanılır. Bu, yapılandırma güncelleştirmelerini rol örnekleri geri dönüştürme veya bir çalışan işleminin yeniden kullanmak için kullanılır.

```C#

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
Bir hizmette - kod, yapılandırma ve verileri - üç paket türlerinin her biri bir paket güncelleştirildi, eklenen veya kaldırılan bir hizmet örneği bildir olayları vardır. Bir hizmet birden çok paket her tür içerebilir. Örneğin, bir hizmet birden çok yapılandırma paketleri, tek tek her sürümü tutulan ve yükseltilebilir olabilir. 

Bu olaylar, hizmet örneği başlatmadan hizmet paketleri değişiklikleri kullanmak kullanılabilir.

```C#

this.Context.CodePackageActivationContext.ConfigurationPackageModifiedEvent +=
                    this.CodePackageActivationContext_ConfigurationPackageModifiedEvent;

private void CodePackageActivationContext_ConfigurationPackageModifiedEvent(object sender, PackageModifiedEventArgs<ConfigurationPackage> e)
{
    this.UpdateCustomConfig(e.NewPackage.Path);
    this.UpdateSettings(e.NewPackage.Settings);
}

```

## <a name="startup-tasks"></a>Başlangıç görevi
Başlatma, bir uygulama başlatılmadan önce gerçekleştirilen eylemler görevlerdir. Bir başlangıç görevi, genellikle yükseltilmiş ayrıcalıklar kullanarak kurulum komut dosyalarını çalıştırmak için kullanılır. Bulut Hizmetleri ve Service Fabric başlangıç görevleri destekler. Bir rol örneği bir parçası olduğundan Service Fabric herhangi belirli VM bağlanmayan bir hizmet için bir başlangıç görevi bağlıdır ancak bulut Hizmetleri'nde bir başlangıç görevi bir VM'ye bağlı olduğunu ana farktır.

| Cloud Services | Service Fabric |
| --- | --- | --- |
| Yapılandırma konumu |ServiceDefinition.csdef |
| Ayrıcalıkları |"sınırlı" veya "yükseltilmiş" |
| Sıralama |"Basit", "arka plan", "ön" |

### <a name="cloud-services"></a>Cloud Services
Bulut Hizmetleri başlatma giriş noktası ServiceDefinition.csdef rol başına yapılandırılır. 

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
Bulut Hizmetleri ve Service Fabric ile Visual Studio Proje şablonları ve hata ayıklama, yapılandırma ve hem yerel hem de dağıtımı Azure desteği ile tümleştirilir. Service Fabric ve bulut hizmetlerini de bir yerel geliştirme çalışma zamanı ortamı sağlar. Farktır bulut hizmeti geliştirme çalışma zamanı, çalıştığı Azure ortamı öykünür olsa da, Service Fabric bir öykünücü kullanmaz - tam Service Fabric çalışma zamanını kullanır. Yerel geliştirme makinenizde çalıştırma Service Fabric üretimde çalışan aynı ortamda ortamıdır.

## <a name="next-steps"></a>Sonraki adımlar
Service Fabric Reliable Services hakkında daha fazla bilgi ve bulut Hizmetleri ve Service Fabric uygulama mimarisi tamamını Service Fabric özelliklerden yararlanmak nasıl anlamak için arasındaki temel farklar.

* [Service Fabric Reliable Services ile çalışmaya başlama](service-fabric-reliable-services-quick-start.md)
* [Bulut Hizmetleri ve Service Fabric arasındaki farklar kavramsal Kılavuzu](service-fabric-cloud-services-migration-differences.md)

<!--Image references-->
[3]: ./media/service-fabric-cloud-services-migration-worker-role-stateless-service/service-fabric-cloud-service-projects.png
[4]: ./media/service-fabric-cloud-services-migration-worker-role-stateless-service/worker-role-to-stateless-service.png
