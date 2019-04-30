---
title: Azure Service Fabric uygulama parolalarını yönetme | Microsoft Docs
description: Gizli değer bir Service Fabric uygulamasında (platformdan) güvenli hale getirmeyi öğrenin.
services: service-fabric
documentationcenter: .net
author: vturecek
manager: chackdan
editor: ''
ms.assetid: 94a67e45-7094-4fbd-9c88-51f4fc3c523a
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 01/04/2019
ms.author: vturecek
ms.openlocfilehash: d151dbf20e68a2152e9d886a74e51786bb8fbfa6
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60614474"
---
# <a name="manage-encrypted-secrets-in-service-fabric-applications"></a>Service Fabric uygulamaları şifrelenmiş gizli dizileri Yönet
Bu kılavuzda, Service Fabric uygulaması gizli yönetme adımlarında size kılavuzluk eder. Gizli dizileri depolama bağlantı dizeleri, parolalar veya düz metin olarak işleneceğini değil diğer değerleri gibi herhangi bir önemli bilgi olabilir.

Bir Service Fabric uygulamasında şifrelenmiş gizli dizileri kullanma üç adımdan oluşur:
* Bir şifreleme sertifikası ayarlayın ve parolaları şifrelemek.
* Şifrelenmiş parolalar uygulamayı belirtin.
* Şifrelenmiş parolalar hizmet kodundan şifresini çözemez.

## <a name="set-up-an-encryption-certificate-and-encrypt-secrets"></a>Bir şifreleme sertifikası ayarlayın ve parolaları şifrelemek
Bir şifreleme sertifikasını ayarlama ve parolaları şifrelemek için kullanarak Windows ile Linux arasında farklılık gösterir.
* [Bir şifreleme sertifikası ayarlayın ve Windows Küme parolaları şifrelemek.][secret-management-windows-specific-link]
* [Bir şifreleme sertifikası ayarlayın ve Linux kümelerinde parolaları şifrelemek.][secret-management-linux-specific-link]

## <a name="specify-encrypted-secrets-in-an-application"></a>Bir uygulamada şifrelenmiş parolalar belirtin
Önceki adımda, bir sertifika ile şifrelemek ve bir uygulamada kullanmak için base-64 kodlu bir dize üreten açıklar. Bu base-64 kodlama dizesi belirtilen bir şifrelenmiş olarak [parametre] [ parameters-link] bir hizmetin Settings.xml veya bir şifrelenmiş olarak [ortam değişkeni] [ environment-variables-link] bir hizmetin ServiceManifest.xml içinde.

Bir şifrelenmiş belirtin [parametre] [ parameters-link] hizmetinizin Settings.xml yapılandırma dosyası ile `IsEncrypted` özniteliğini `true`:

```xml
<?xml version="1.0" encoding="utf-8" ?>
<Settings xmlns:xsd="https://www.w3.org/2001/XMLSchema" xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/2011/01/fabric">
  <Section Name="MySettings">
    <Parameter Name="MySecret" IsEncrypted="true" Value="I6jCCAeYCAxgFhBXABFxzAt ... gNBRyeWFXl2VydmjZNwJIM=" />
  </Section>
</Settings>
```
Bir şifrelenmiş belirtin [ortam değişkeni] [ environment-variables-link] ile hizmetinizin ServiceManifest.xml dosyasındaki `Type` özniteliğini `Encrypted`:
```xml
<CodePackage Name="Code" Version="1.0.0">
  <EnvironmentVariables>
    <EnvironmentVariable Name="MyEnvVariable" Type="Encrypted" Value="I6jCCAeYCAxgFhBXABFxzAt ... gNBRyeWFXl2VydmjZNwJIM=" />
  </EnvironmentVariables>
</CodePackage>
```

### <a name="inject-application-secrets-into-application-instances"></a>Uygulama örnekleri ile uygulama gizli dizilerini ekleme
İdeal olarak, farklı ortamlar için dağıtım olarak otomatik olmalıdır. Bu, bir derleme ortamında gizli şifreleme gerçekleştirmek ve uygulama örnekleri oluşturulurken parametre olarak şifrelenmiş gizli dizileri sağlayarak gerçekleştirilebilir.

#### <a name="use-overridable-parameters-in-settingsxml"></a>Geçersiz kılınabilir parametrelere Settings.xml dosyasında kullanın
Settings.xml yapılandırma dosyası uygulama oluşturma zamanında sağlandığı geçersiz kılınabilir parametrelere izin verir. Kullanım `MustOverride` parametresi için bir değer sağlamak yerine özniteliği:

```xml
<?xml version="1.0" encoding="utf-8" ?>
<Settings xmlns:xsd="https://www.w3.org/2001/XMLSchema" xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/2011/01/fabric">
  <Section Name="MySettings">
    <Parameter Name="MySecret" IsEncrypted="true" Value="" MustOverride="true" />
  </Section>
</Settings>
```

Settings.xml değerleri geçersiz kılmak için bir geçersiz kılma parametresini ApplicationManifest.xml hizmetinin bildirin:

```xml
<ApplicationManifest ... >
  <Parameters>
    <Parameter Name="MySecret" DefaultValue="" />
  </Parameters>
  <ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName="Stateful1Pkg" ServiceManifestVersion="1.0.0" />
    <ConfigOverrides>
      <ConfigOverride Name="Config">
        <Settings>
          <Section Name="MySettings">
            <Parameter Name="MySecret" Value="[MySecret]" IsEncrypted="true" />
          </Section>
        </Settings>
      </ConfigOverride>
    </ConfigOverrides>
  </ServiceManifestImport>
 ```

Değer olarak belirtilebilir şimdi bir *parametr aplikace* uygulamanın bir örneğini oluştururken. Bir uygulama örneği oluşturma Script, PowerShell kullanarak veya ile yazılmış C#, derleme sürecinde kolay tümleştirme için.

PowerShell kullanarak, parametre için sağlanan `New-ServiceFabricApplication` olarak komutu bir [karma tablo](https://technet.microsoft.com/library/ee692803.aspx):

```powershell
New-ServiceFabricApplication -ApplicationName fabric:/MyApp -ApplicationTypeName MyAppType -ApplicationTypeVersion 1.0.0 -ApplicationParameter @{"MySecret" = "I6jCCAeYCAxgFhBXABFxzAt ... gNBRyeWFXl2VydmjZNwJIM="}
```

Kullanarak C#, uygulama parametreleri belirtilmiş bir `ApplicationDescription` olarak bir `NameValueCollection`:

```csharp
FabricClient fabricClient = new FabricClient();

NameValueCollection applicationParameters = new NameValueCollection();
applicationParameters["MySecret"] = "I6jCCAeYCAxgFhBXABFxzAt ... gNBRyeWFXl2VydmjZNwJIM=";

ApplicationDescription applicationDescription = new ApplicationDescription(
    applicationName: new Uri("fabric:/MyApp"),
    applicationTypeName: "MyAppType",
    applicationTypeVersion: "1.0.0",
    applicationParameters: applicationParameters)
);

await fabricClient.ApplicationManager.CreateApplicationAsync(applicationDescription);
```

## <a name="decrypt-encrypted-secrets-from-service-code"></a>Hizmet kodundan şifrelenmiş gizli dizilerin şifresini çözmek
Erişmek için API [parametreleri] [ parameters-link] ve [ortam değişkenlerini] [ environment-variables-link] için şifrelenmiş değer kolay şifre çözmesine izin verin. Şifreli dize şifreleme için kullanılan sertifikayla ilgili bilgileri içerdiğinden, sertifikayı el ile belirtmeniz gerekmez. Sertifika yalnızca hizmetin üzerinde çalıştığı düğüm üzerinde yüklü olması gerekir.

```csharp
// Access decrypted parameters from Settings.xml
ConfigurationPackage configPackage = FabricRuntime.GetActivationContext().GetConfigurationPackageObject("Config");
bool MySecretIsEncrypted = configPackage.Settings.Sections["MySettings"].Parameters["MySecret"].IsEncrypted;
if (MySecretIsEncrypted)
{
    SecureString MySecretDecryptedValue = configPackage.Settings.Sections["MySettings"].Parameters["MySecret"].DecryptValue();
}

// Access decrypted environment variables from ServiceManifest.xml
// Note: you do not have to call any explicit API to decrypt the environment variable.
string MyEnvVariable = Environment.GetEnvironmentVariable("MyEnvVariable");
```

## <a name="next-steps"></a>Sonraki adımlar
Daha fazla bilgi edinin [uygulama ve hizmet güvenliği](service-fabric-application-and-service-security.md)

<!-- Links -->
[parameters-link]:service-fabric-how-to-parameterize-configuration-files.md
[environment-variables-link]: service-fabric-how-to-specify-environment-variables.md
[secret-management-windows-specific-link]: service-fabric-application-secret-management-windows.md
[secret-management-linux-specific-link]: service-fabric-application-secret-management-linux.md
