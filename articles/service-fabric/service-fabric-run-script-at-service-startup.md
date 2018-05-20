---
title: Azure Service Fabric hizmeti başladığında bir komut dosyası çalıştırma | Microsoft Docs
description: Bir Service Fabric hizmeti Kurulum giriş noktası için bir ilke yapılandırın ve hizmet başlangıç süresi bir komut dosyası çalıştırma hakkında bilgi edinin.
services: service-fabric
documentationcenter: .net
author: msfussell
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 03/21/2018
ms.author: mfussell
ms.openlocfilehash: 3fe22d8bb52fa5f45ce5f1cdc7b860d1ce295a71
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2018
---
# <a name="run-a-service-startup-script-as-a-local-user-or-system-account"></a>Yerel kullanıcı veya sistem hesabı olarak bir hizmet başlangıcı komut dosyası çalıştırma
Service Fabric hizmeti yürütülebilir dosyası başlatıldığında önce bazı yapılandırma ya da kurulum iş çalıştırmak gerekli olabilir.  Örneğin, ortam değişkenleri yapılandırılıyor. Hizmeti için hizmet bildiriminde yürütülebilir hizmeti başlar önce çalıştırılacak bir komut dosyası belirtebilirsiniz. Hangi hesabı değiştirebilirsiniz hizmet Kurulum giriş noktası için bir RunAs ilkesini yapılandırarak yürütülebilir Kurulum altında çalışır.  Ayrı Kurulum giriş noktası hizmeti ana bilgisayarı yürütülebilir yüksek ayrıcalıklara sahip uzun süre için çalıştırmanız gerekmez için kısa bir süre için yüksek privilged yapılandırma çalıştırmanıza olanak sağlar.

Kurulum giriş noktası (**SetupEntryPoint** içinde [hizmet bildirimi](service-fabric-application-and-service-manifests.md)) çalıştırılan varsayılan Service Fabric kimlik bilgileriyle bir ayrıcalıklı giriş noktasıdır (genellikle  *NetworkService* hesabı) önce başka bir giriş noktası. Tarafından belirtilen yürütülebilir dosya **EntryPoint** genellikle uzun süre çalışan hizmet yöneticisidir. **EntryPoint** yürütülebilir dosyayı çalıştırmak **SetupEntryPoint** yürütülebilir başarıyla çıkar. Sonuçta elde edilen işlem izlenen ve yeniden başlatılabilir ve yeniden ile başlayan **SetupEntryPoint** hiç sonlandırır veya çöküyor. 

## <a name="configure-the-service-setup-entry-point"></a>Hizmet kurulumu giriş noktasını yapılandırma
Kurulum komut dosyasını belirten bir durum bilgisiz hizmet basit hizmeti bildirim örneğin aşağıdadır *MySetup.bat* hizmet **SetupEntryPoint**.  **Bağımsız değişkenler** çalıştırıldığında, bağımsız değişkenleri komut dosyasına geçirmek için kullanılır.

```xml
<?xml version="1.0" encoding="utf-8"?>
<ServiceManifest Name="MyStatelessServicePkg"
                 Version="1.0.0"
                 xmlns="http://schemas.microsoft.com/2011/01/fabric"
                 xmlns:xsd="http://www.w3.org/2001/XMLSchema"
                 xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
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
## <a name="configure-the-policy-for-a-service-setup-entry-point"></a>Bir hizmet Kurulum giriş noktası için ilkesi yapılandırma
Varsayılan olarak, hizmet Kurulum giriş noktası yürütülebilir Service Fabric olarak aynı kimlik altında çalışır (genellikle *NetworkService* hesabı).  Uygulama bildiriminde başlangıç komut dosyasını bir yerel sistem hesabı veya bir yönetici hesabı altında çalıştırmak için güvenlik izinleri değiştirebilirsiniz.

### <a name="configure-the-policy-by-using-a-local-system-account"></a>Yerel Sistem hesabını kullanarak ilkeyi yapılandırın
Aşağıdaki uygulama bildirim örnek kullanıcı yönetici hesabı (SetupAdminUser) altında çalıştırmak için hizmeti Kurulum giriş noktasının nasıl yapılandırılacağı gösterir.

```xml
<?xml version="1.0" encoding="utf-8"?>
<ApplicationManifest xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" ApplicationTypeName="MyApplicationType" ApplicationTypeVersion="1.0.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">
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

İlk olarak, oluşturma bir **sorumluları** SetupAdminUser gibi bir kullanıcı adı bölümü. SetupAdminUser kullanıcı hesabı, yöneticiler sistem grubunun üyesidir.

Sonraki altında **ServiceManifestImport** bölümünde, bu asıl uygulamak için bir ilke yapılandırın **SetupEntryPoint**. Bu ilke Service Fabric gerektiğini söyler **MySetup.bat** dosyasını çalıştırın, SetupAdminUser (yönetici ayrıcalıklarıyla) çalıştırmanız gerekir. Sahip olduğu *değil* kodda ana giriş noktası için bir ilke uygulanmış **MyServiceHost.exe** sisteminde çalışıyor **NetworkService** hesabı. Bu, tüm hizmet giriş noktaları olarak çalıştırılan varsayılan hesaptır.

### <a name="configure-the-policy-by-using-local-system-accounts"></a>Yerel Sistem hesapları kullanarak ilkeyi yapılandırın
Genellikle, bir yönetici hesabı yerine yerel sistem hesabını kullanarak başlangıç komut dosyasını çalıştırmak için tercih edilir. İyi bilgisayarlarda varsayılan olarak etkin kullanıcı erişim denetimi (UAC) yüklü olduğu RunAs ilke genellikle Yöneticiler grubunun bir üyesi olarak çalışan işe yaramaz. Böyle durumlarda, SetupEntryPoint LocalSystem olarak yerine Yöneticiler grubuna eklenmiş yerel bir kullanıcı olarak çalıştırmak için önerilir. Aşağıdaki örnek, LocalSystem olarak çalışacak şekilde SetupEntryPoint ayarını gösterir:

```xml
<?xml version="1.0" encoding="utf-8"?>
<ApplicationManifest xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" ApplicationTypeName="MyApplicationType" ApplicationTypeVersion="1.0.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">
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
> Linux kümeleri için bir hizmet veya kurulumu çalıştırmak için giriş noktası olarak **kök**, belirleyebileceğiniz **AccountType** olarak **LocalSystem**.

## <a name="run-a-script-from-the-setup-entry-point"></a>Bir komut dosyası Kurulum giriş noktasından Çalıştır
Şimdi bir başlangıç betiği yönetici ayrıcalıklarıyla çalıştırmak için proje ekleyin. 

Visual Studio'da hizmet projesine sağ tıklayın ve adlı yeni bir dosya ekleme *MySetup.bat*.

Ardından, emin *MySetup.bat* dosya hizmet paketinde yer almaktadır. Varsayılan olarak, bu değil. Dosya seçin, bağlam menüsü almak için sağ tıklatın ve **özellikleri**. Özellikler iletişim kutusunda emin **çıktı dizinine Kopyala** ayarlanır **yeniyse Kopyala**. Aşağıdaki ekran görüntüsüne bakın.

![SetupEntryPoint toplu iş dosyası için Visual Studio CopyToOutput][image1]

Şimdi Düzenle *MySetup.bat* dosya ve aşağıdaki komutları bir sistem ortam değişkenini ayarlamak ve çıkış bir metin dosyası ekleyin:

```
REM Set a system environment variable. This requires administrator privilege
setx -m TestVariable %*
echo System TestVariable set to > out.txt
echo %TestVariable% >> out.txt

REM To delete this system variable us
REM REG delete "HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Session Manager\Environment" /v TestVariable /f
```

Ardından, yapı ve bir yerel geliştirme kümeye çözümü dağıtın. Gösterildiği gibi hizmet başladıktan sonra [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md), MySetup.bat dosyasını iki yolla başarılı olduğunu görebilirsiniz. Bir PowerShell komut istemi açıp türü:

```
PS C:\ [Environment]::GetEnvironmentVariable("TestVariable","Machine")
MyValue
```

Ardından, hizmet dağıtılır ve başlatılan burada düğümün adını not edin [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md). Örneğin, düğüm 2. Ardından, değerini gösterir out.txt dosyayı bulmak için uygulama örnek iş klasöre gidin **TestVariable**. Bu hizmet düğümü 2'ye dağıttıysanız, örneğin, ardından bu yol için gidebilirsiniz **MyApplicationType**:

```
C:\SfDevCluster\Data\_App\Node.2\MyApplicationType_App\work\out.txt
```

## <a name="run-powershell-commands-from-a-setup-entry-point"></a>Kurulum giriş noktasından PowerShell komutlarını çalıştırın
Powershell'den çalıştırmak için **SetupEntryPoint** noktası çalıştırabilirsiniz **PowerShell.exe** bir toplu iş dosyasında bir PowerShell dosyasını işaret eder. İlk olarak, bir PowerShell dosya hizmeti projesine--Örneğin, ekleyin **MySetup.ps1**. Ayarlamayı unutmayın *yeniyse Kopyala* özelliği böylece dosyayı Ayrıca hizmet paketinde dahil. Adlı Sistem ortam değişkenini ayarlar MySetup.ps1 adlı bir PowerShell dosya başlatır örnek bir toplu iş dosyası aşağıdaki örnekte **TestVariable**.

PowerShell dosyası başlatmak için MySetup.bat:

```
powershell.exe -ExecutionPolicy Bypass -Command ".\MySetup.ps1"
```

PowerShell dosyasında bir sistem ortam değişkenini ayarlamak üzere aşağıdakini ekleyin:

```
[Environment]::SetEnvironmentVariable("TestVariable", "MyValue", "Machine")
[Environment]::GetEnvironmentVariable("TestVariable","Machine") > out.txt
```

> [!NOTE]
> Varsayılan olarak, toplu iş dosyası çalıştığında, uygulama klasörü adlı göründüğünü **iş** dosyaları için. MySetup.bat çalıştığında, bu durumda, bu uygulama aynı klasörde MySetup.ps1 dosyasını bulmak için istiyoruz **kod paketi** klasör. Bu klasörü değiştirmek için çalışma klasörü ayarlayın:
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

## <a name="debug-a-startup-script-locally-using-console-redirection"></a>Konsol yeniden yönlendirme özelliğini kullanarak yerel olarak bir başlangıç komut dosyası hata ayıklama
Bazen, hata ayıklama amacıyla bir kurulum komut dosyası çalıştırılarak konsol çıkışı görmek için yararlıdır. Çıktıyı bir dosyaya yazar hizmet bildiriminde Kurulum Giriş noktasındaki bir konsol yeniden yönlendirme ilkesi ayarlayabilirsiniz. Dosya çıktısı adlı uygulama klasörüne yazılır **günlük** küme düğümünde burada uygulamanın dağıtıldığı ve çalıştırın. 

> [!WARNING]
> Hiçbir zaman bu uygulamayı yük devretme etkileyebileceğinden üretimde dağıtılan bir uygulamada konsol yeniden yönlendirme ilkesi kullanın. *Yalnızca* bunu yerel geliştirme ve hata ayıklama amacıyla kullanın.  
> 
> 

Aşağıdaki hizmet bildirimi örnek konsol yeniden yönlendirmesi FileRetentionCount değeri ile ayarı gösterir:

```xml
<SetupEntryPoint>
    <ExeHost>
    <Program>MySetup.bat</Program>
    <WorkingFolder>CodePackage</WorkingFolder>
    <ConsoleRedirection FileRetentionCount="10"/>
    </ExeHost>
</SetupEntryPoint>
```

Şimdi yazmak için MySetup.ps1 dosyasını değiştirirseniz, bir **Echo** komutu, bu hata ayıklama amacıyla çıktı dosyasına yazacak:

```
Echo "Test console redirection which writes to the application log folder on the node that the application is deployed to"
```

> [!WARNING]
> Kodunuzu hata ayıklama sonra bu konsol yeniden yönlendirme ilkesi hemen kaldırın.



<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Sonraki adımlar
* [Uygulama ve hizmet güvenliği hakkında bilgi edinin](service-fabric-application-and-service-security.md)
* [Uygulama modeli anlama](service-fabric-application-model.md)
* [Bir hizmet bildirimi kaynakları belirtme](service-fabric-service-manifest-resources.md)
* [Uygulama dağıtma](service-fabric-deploy-remove-applications.md)

[image1]: ./media/service-fabric-application-runas-security/copy-to-output.png
