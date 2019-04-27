---
title: Bir Azure Service Fabric hizmeti yeniden başlatıldığında betik çalıştırma | Microsoft Docs
description: Bir Service Fabric Hizmet Kurulumu giriş noktası için bir ilke yapılandırın ve hizmeti başlatma süresi, betik çalıştırma hakkında bilgi edinin.
services: service-fabric
documentationcenter: .net
author: athinanthny
manager: chackdan
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 03/21/2018
ms.author: atsenthi
ms.openlocfilehash: 76be814e0dd4c054fc3a873716dbfe395eeeb2dc
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60837795"
---
# <a name="run-a-service-startup-script-as-a-local-user-or-system-account"></a>Yerel kullanıcı veya sistem hesabı olarak bir hizmet başlangıcı komut dosyası çalıştırma
Yürütülebilir bir Service Fabric hizmeti başlatıldığında önce bazı yapılandırma ve kurulum için bir iş çalıştırmak gerekli olabilir.  Örneğin, ortam değişkenlerini yapılandırma. Hizmeti için hizmet bildiriminde yürütülebilir hizmeti başlatıldığında önce çalıştırılacak bir betik belirtebilirsiniz. Hangi hesabı değiştirebilirsiniz Hizmet Kurulumu giriş noktası için bir RunAs ilkesini yapılandırarak yürütülebilir Kurulum altında çalışır.  Ayrı bir Kurulum giriş noktası yürütülebilir hizmet konağı uzun sürelerle yüksek ayrıcalıklarla çalıştırmanız gerekmez, kısa bir süre için yüksek ayrıcalıklı yapılandırması çalıştırmanıza olanak tanır.

Kurulum Giriş noktasına (**SetupEntryPoint** içinde [hizmet bildirimi](service-fabric-application-and-service-manifests.md)) varsayılan olarak, Service Fabric ile aynı kimlik bilgileri ile çalışan bir ayrıcalıklı giriş noktası (genellikle  *NetworkService* hesabı) önce başka bir giriş noktası. Tarafından belirtilen yürütülebilir **EntryPoint** genellikle uzun süre çalışan hizmet yöneticisidir. **EntryPoint** yürütülebilir dosyayı çalıştırmak **SetupEntryPoint** yürütülebilir başarıyla çıkılıyor. Sonuçta elde edilen işlem izlenen ve yeniden başlatılabilir ve yeniden ile başlayan **SetupEntryPoint** hiç olmadığı kadar sonlandırır veya kilitleniyor. 

## <a name="configure-the-service-setup-entry-point"></a>Hizmet kurulumu giriş noktasını yapılandırma
Kurulum betiği belirten bir durum bilgisi olmayan hizmet için bir basit hizmeti bildirim örneği verilmiştir *MySetup.bat* hizmetinde **SetupEntryPoint**.  **Bağımsız değişkenler** çalıştırıldığında, bağımsız değişkenleri komut dosyasına geçirmek için kullanılır.

```xml
<?xml version="1.0" encoding="utf-8"?>
<ServiceManifest Name="MyStatelessServicePkg"
                 Version="1.0.0"
                 xmlns="http://schemas.microsoft.com/2011/01/fabric"
                 xmlns:xsd="https://www.w3.org/2001/XMLSchema"
                 xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance">
  <Description>An example service manifest.</Description>
  <ServiceTypes>
    <StatelessServiceType ServiceTypeName="MyStatelessServiceType" />
  </ServiceTypes>

  <!-- Code package is your service executable. -->
  <CodePackage Name="Code" Version="1.0.0">
    <SetupEntryPoint>
      <ExeHost>
        <Program>MySetup.bat</Program>
        <Arguments>MyValue</Arguments>
        <WorkingFolder>Work</WorkingFolder>        
      </ExeHost>
    </SetupEntryPoint>
    <EntryPoint>
      <ExeHost>
        <Program>MyStatelessService.exe</Program>
      </ExeHost>
    </EntryPoint>
  </CodePackage>

  <ConfigPackage Name="Config" Version="1.0.0" />

  <Resources>
    <Endpoints>
      <Endpoint Name="ServiceEndpoint" />
    </Endpoints>
  </Resources>
</ServiceManifest>
```
## <a name="configure-the-policy-for-a-service-setup-entry-point"></a>Bir hizmet Kurulumu giriş noktası için ilke yapılandırma
Varsayılan olarak, hizmet Kurulumu giriş noktası yürütülebilir dosyası, Service Fabric ile aynı kimlik bilgileri altında çalışır (genellikle *NetworkService* hesabı).  Uygulama bildiriminde yerel sistem hesabı veya bir yönetici hesabı altında başlangıç betiği çalıştırmak için güvenlik izinleri değiştirebilirsiniz.

### <a name="configure-the-policy-by-using-a-local-system-account"></a>Yerel Sistem hesabı kullanarak ilke yapılandırma
Aşağıdaki uygulama bildirim örnek (SetupAdminUser) kullanıcı yönetici hesabı altında çalışacak şekilde Hizmet Kurulumu giriş noktasını yapılandırma gösterilmektedir.

```xml
<?xml version="1.0" encoding="utf-8"?>
<ApplicationManifest xmlns:xsd="https://www.w3.org/2001/XMLSchema" xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance" ApplicationTypeName="MyApplicationType" ApplicationTypeVersion="1.0.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">
  <Parameters>
    <Parameter Name="MyStatelessService_InstanceCount" DefaultValue="-1" />
  </Parameters>
  <ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName="MyStatelessServicePkg" ServiceManifestVersion="1.0.0" />
    <ConfigOverrides />
    <Policies>
      <RunAsPolicy CodePackageRef="Code" UserRef="SetupAdminUser" EntryPointType="Setup" />
    </Policies>
  </ServiceManifestImport>
  <DefaultServices>
    <Service Name="MyStatelessService" ServicePackageActivationMode="ExclusiveProcess">
      <StatelessService ServiceTypeName="MyStatelessServiceType" InstanceCount="[MyStatelessService_InstanceCount]">
        <SingletonPartition />
      </StatelessService>
    </Service>
  </DefaultServices>
  <Principals>
    <Users>
      <User Name="SetupAdminUser">
        <MemberOf>
          <SystemGroup Name="Administrators" />
        </MemberOf>
      </User>
    </Users>
  </Principals>
</ApplicationManifest>
```

İlk olarak, oluşturun bir **sorumluları** SetupAdminUser gibi bir kullanıcı adı bölümü. SetupAdminUser kullanıcı hesabını Administrators sistem grubuna bir üyesidir.

Sonraki altında **Servicemanifestımport** bölümünde, bu sorumluya uygulanacağını bir ilke yapılandırın **SetupEntryPoint**. Bu ilke, Service Fabric gerektiğini söyler. **MySetup.bat** dosyası, çalıştırılmadan SetupAdminUser olarak (yönetici ayrıcalıklarıyla) çalıştırmanız gerekir. Elinizde bu yana *değil* ana girdi noktası kodda bir ilke uygulanır **MyServiceHost.exe** sisteminde çalışan **NetworkService** hesabı. Bu, tüm hizmet giriş noktaları olarak çalıştırılan varsayılan hesaptır.

### <a name="configure-the-policy-by-using-local-system-accounts"></a>Yerel Sistem hesapları kullanarak ilke yapılandırma
Genellikle, bir yönetici hesabı yerine yerel sistem hesabı kullanarak başlangıç betiği çalıştırmak için tercih edilir. İyi bilgisayarlar kullanıcı erişim denetimi (UAC) varsayılan olarak etkin olduğundan RunAs ilke genellikle Yöneticiler grubunun bir üyesi olarak çalışan işe yaramaz. Böyle durumlarda, SetupEntryPoint, yerine Yöneticiler grubuna eklenmiş bir yerel kullanıcı olarak LocalSystem olarak kullanılması önerilir. Aşağıdaki örnek, LocalSystem olarak çalışacak şekilde SetupEntryPoint ayarı gösterir:

```xml
<?xml version="1.0" encoding="utf-8"?>
<ApplicationManifest xmlns:xsd="https://www.w3.org/2001/XMLSchema" xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance" ApplicationTypeName="MyApplicationType" ApplicationTypeVersion="1.0.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">
  <Parameters>
    <Parameter Name="MyStatelessService_InstanceCount" DefaultValue="-1" />
  </Parameters>
  <ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName="MyStatelessServicePkg" ServiceManifestVersion="1.0.0" />
    <ConfigOverrides />
    <Policies>
         <RunAsPolicy CodePackageRef="Code" UserRef="SetupLocalSystem" EntryPointType="Setup" />
      </Policies>
  </ServiceManifestImport>
  <DefaultServices>
    <Service Name="MyStatelessService" ServicePackageActivationMode="ExclusiveProcess">
      <StatelessService ServiceTypeName="MyStatelessServiceType" InstanceCount="[MyStatelessService_InstanceCount]">
        <SingletonPartition />
      </StatelessService>
    </Service>
  </DefaultServices>
  <Principals>
      <Users>
         <User Name="SetupLocalSystem" AccountType="LocalSystem" />
      </Users>
   </Principals>
</ApplicationManifest>
```

> [!NOTE]
> Linux kümelerine yönelik bir hizmet veya kurulumu çalıştırmak için giriş noktası olarak **kök**, belirtebileceğiniz **AccountType** olarak **LocalSystem**.

## <a name="run-a-script-from-the-setup-entry-point"></a>Bir betiği Kurulum giriş noktasından çalıştırabilirsiniz
Şimdi projeye yönetici ayrıcalıklarıyla çalıştırmak için bir başlangıç betiği ekleyin. 

Visual Studio'da hizmet projesine sağ tıklayın ve adlı yeni bir dosya ekleme *MySetup.bat*.

Ardından, emin *MySetup.bat* dosya hizmet paketinde yer almaktadır. Varsayılan olarak, bu değildir. Dosyayı seçin, bağlam menüsünü almak için sağ tıklatın ve seçin **özellikleri**. Özellikler iletişim kutusunda, emin **çıkış dizinine Kopyala** ayarlanır **yeniyse Kopyala**. Aşağıdaki ekran görüntüsüne bakın.

![Visual Studio CopyToOutput SetupEntryPoint toplu iş dosyası için][image1]

Şimdi Düzenle *MySetup.bat* dosya ve aşağıdaki komutları bir sistem ortam değişkenini ayarlamak ve çıkış bir metin dosyasına ekleyin:

```
REM Set a system environment variable. This requires administrator privilege
setx -m TestVariable %*
echo System TestVariable set to > out.txt
echo %TestVariable% >> out.txt

REM To delete this system variable us
REM REG delete "HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Session Manager\Environment" /v TestVariable /f
```

Ardından, derleme ve çözüm için bir yerel geliştirme kümesi dağıtın. Gösterildiği gibi hizmet başlatıldıktan sonra [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md), MySetup.bat dosya iki yolla başarılı olduğunu görebilirsiniz. Bir PowerShell komut istemi açıp türü:

```
PS C:\ [Environment]::GetEnvironmentVariable("TestVariable","Machine")
MyValue
```

Daha sonra başlatılan hizmet nerede dağıtıldığını ve düğümün adını Not [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md). Örneğin, düğüm 2. Ardından, değerini gösteren out.txt dosyayı bulmak için uygulama örneği iş klasöre gidin **TestVariable**. Bu hizmet düğümü 2 olarak dağıttıysanız, örneğin, ardından bu yol için giderek **MyApplicationType**:

```
C:\SfDevCluster\Data\_App\Node.2\MyApplicationType_App\work\out.txt
```

## <a name="run-powershell-commands-from-a-setup-entry-point"></a>Bir Kurulum giriş noktasından PowerShell komutlarını çalıştırın
Powershell'den çalıştırmak **SetupEntryPoint** noktası çalıştırabilirsiniz **PowerShell.exe** bir toplu iş dosyasında bir PowerShell dosyasını işaret eder. İlk olarak, bir PowerShell dosyasını hizmet projesine--örneğin ekleyin **MySetup.ps1**. Ayarlamayı unutmayın *yeniyse Kopyala* özelliği böylece dosya da hizmet paketine dahildir. Adlı bir sistem ortam değişkeni ayarlayan MySetup.ps1 adlı bir PowerShell dosyasını başlatan bir örnek toplu iş dosyası aşağıdaki örnekte **TestVariable**.

Bir PowerShell dosyasını başlatmak için MySetup.bat:

```
powershell.exe -ExecutionPolicy Bypass -Command ".\MySetup.ps1"
```

Sistem ortam değişkenini ayarlamak için aşağıdaki PowerShell dosyasına ekleyin:

```
[Environment]::SetEnvironmentVariable("TestVariable", "MyValue", "Machine")
[Environment]::GetEnvironmentVariable("TestVariable","Machine") > out.txt
```

> [!NOTE]
> Toplu iş dosyası çalıştığında, varsayılan olarak, adlı uygulama klasörü görünüyor **çalışma** dosyaları. MySetup.bat çalıştığında, bu durumda, bu uygulama aynı klasörde MySetup.ps1 dosyayı bulmak için istediğimiz **kod paketi** klasör. Bu klasörü değiştirmek için çalışma klasörünü ayarlayın:
> 
> 

```xml
<SetupEntryPoint>
    <ExeHost>
    <Program>MySetup.bat</Program>
    <WorkingFolder>CodePackage</WorkingFolder>
    </ExeHost>
</SetupEntryPoint>
```

## <a name="debug-a-startup-script-locally-using-console-redirection"></a>Konsol yönlendirmesi kullanarak yerel olarak başlatma komut dosyası hata ayıklama
Bazen, hata ayıklama amacıyla bir kurulum betik çalışmasını konsol çıkışını görmek için kullanışlıdır. Kurulum Giriş noktasına çıktıyı bir dosyaya yazar hizmet bildirimindeki bir konsol yeniden yönlendirme ilkesi ayarlayabilirsiniz. Dosya çıktısı adlı uygulama klasörüne yazılır **günlük** küme düğümünde uygulama burada dağıtılan ve çalıştırın. 

> [!WARNING]
> Hiçbir zaman konsol yeniden yönlendirme ilkesi, bu uygulamanın yük devretme etkileyebileceğinden, üretimde dağıtılan bir uygulamada kullanın. *Yalnızca* bunu yerel geliştirme ve hata ayıklama amacıyla kullanın.  
> 
> 

Konsol yönlendirmesi FileRetentionCount değeri ayarı aşağıdaki hizmet bildirimi örnek gösterilmektedir:

```xml
<SetupEntryPoint>
    <ExeHost>
    <Program>MySetup.bat</Program>
    <WorkingFolder>CodePackage</WorkingFolder>
    <ConsoleRedirection FileRetentionCount="10"/>
    </ExeHost>
</SetupEntryPoint>
```

Yazılacak MySetup.ps1 dosya artık değiştirirseniz bir **Yankı** komutu, bu hata ayıklama amacıyla çıkış dosyasının yazacak:

```
Echo "Test console redirection which writes to the application log folder on the node that the application is deployed to"
```

> [!WARNING]
> Hemen betiğinizin hata ayıklama sonra bu konsolu yeniden yönlendirme ilkesi kaldırın.



<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Sonraki adımlar
* [Uygulama ve hizmet güvenliği hakkında bilgi edinin](service-fabric-application-and-service-security.md)
* [Uygulama modelini anlama](service-fabric-application-model.md)
* [Bir hizmet bildiriminde kaynakları belirtme](service-fabric-service-manifest-resources.md)
* [Uygulama dağıtma](service-fabric-deploy-remove-applications.md)

[image1]: ./media/service-fabric-application-runas-security/copy-to-output.png
