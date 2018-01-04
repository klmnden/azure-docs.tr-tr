---
title: "Azure mikro güvenlik ilkeleri hakkında bilgi edinin | Microsoft Docs"
description: "Bir Service Fabric uygulaması sistem ve yerel güvenlik hesaplarını, burada bir uygulama başlatılmadan önce bazı ayrıcalıklı eylemleri gerçekleştirmek için gereken SetupEntry noktası dahil altında çalıştırmak nasıl bir genel bakış"
services: service-fabric
documentationcenter: .net
author: msfussell
manager: timlt
editor: 
ms.assetid: 4242a1eb-a237-459b-afbf-1e06cfa72732
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/30/2017
ms.author: mfussell
ms.openlocfilehash: b2ff715d8225bd0a9c7f6108f8804cdfa3189cc8
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
---
# <a name="configure-security-policies-for-your-application"></a>Uygulamanıza yönelik güvenlik ilkeleri yapılandırma
Azure Service Fabric kullanarak, kümedeki farklı kullanıcı hesabı altında çalışan uygulamaları güvenliğini sağlayabilirsiniz. Service Fabric uygulamaları tarafından kullanıcı hesapları altında--dağıtım zamanında örneğin kullanılan kaynaklar, dosyaları, dizinleri ve sertifikaları güvenli yardımcı olur. Bu çalışan uygulamaları bile paylaşılan bir barındırılan ortamda, diğerinden daha güvenli hale getirir.

Varsayılan olarak, Service Fabric uygulamaları Fabric.exe işlemin altında çalıştığı hesap altında çalışır. Service Fabric de bir yerel kullanıcı hesabı veya uygulama bildirimi içinde belirtilen yerel sistem hesabı altında uygulamaları çalıştırmak için yeteneği sağlar. Desteklenen yerel sistem hesabı türleri **YerelKullanıcı**, **NetworkService**, **Yerelhizmet**, ve **LocalSystem**.

 Tek başına yükleyici kullanarak, veri merkezinizde Windows Server'da Service Fabric çalıştırıyorsanız, Grup yönetilen hizmet hesapları dahil olmak üzere Active Directory etki alanı hesaplarını kullanabilirsiniz.

Tanımlayabilir ve kullanıcı grupları oluşturun, böylece bir veya daha fazla kullanıcı birlikte yönetilecek her gruba eklenebilir. Bu, farklı hizmet giriş noktaları için birden çok kullanıcı ve grup düzeyinde kullanılabilen bazı ortak ayrıcalıklarına sahip olmanız gerekir yararlıdır.

## <a name="configure-the-policy-for-a-service-setup-entry-point"></a>Bir hizmet Kurulum giriş noktası için ilkesi yapılandırma
Bölümünde açıklandığı gibi [uygulama ve hizmet bildirimlerini](service-fabric-application-and-service-manifests.md), Kurulum giriş noktası **SetupEntryPoint**, Service Fabric kimlik bilgileriyle çalışır ayrıcalıklı giriş noktasıdır (genellikle *NetworkService* hesabı) önce başka bir giriş noktası. Tarafından belirtilen yürütülebilir dosya **EntryPoint** genellikle uzun süre çalışan hizmet yöneticisidir. Bu nedenle ayrı Kurulum giriş noktası sahip hizmet ana bilgisayarı yürütülebilir yüksek ayrıcalıklara sahip genişletilmiş süre için çalıştırmanız gereğini ortadan kaldırır. Yürütülebilir dosya, **EntryPoint** belirtir çalıştırıldıktan **SetupEntryPoint** başarıyla çıkar. Sonuçta elde edilen işlem izlenen ve yeniden başlatılabilir ve yeniden ile başlayan **SetupEntryPoint** hiç sonlandırır veya çöküyor.

SetupEntryPoint ve hizmet için ana EntryPoint gösteren basit hizmeti bildirim bir örnek verilmiştir.

```xml
<?xml version="1.0" encoding="utf-8" ?>
<ServiceManifest Name="MyServiceManifest" Version="SvcManifestVersion1" xmlns="http://schemas.microsoft.com/2011/01/fabric" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <Description>An example service manifest</Description>
  <ServiceTypes>
    <StatelessServiceType ServiceTypeName="MyServiceType" />
  </ServiceTypes>
  <CodePackage Name="Code" Version="1.0.0">
    <SetupEntryPoint>
      <ExeHost>
        <Program>MySetup.bat</Program>
        <WorkingFolder>CodePackage</WorkingFolder>
      </ExeHost>
    </SetupEntryPoint>
    <EntryPoint>
      <ExeHost>
        <Program>MyServiceHost.exe</Program>
      </ExeHost>
    </EntryPoint>
  </CodePackage>
  <ConfigPackage Name="Config" Version="1.0.0" />
</ServiceManifest>
```

### <a name="configure-the-policy-by-using-a-local-account"></a>Yerel bir hesap kullanarak ilkeyi yapılandırın
Kurulum giriş noktası için hizmet yapılandırdıktan sonra uygulama bildiriminde altında çalıştığı güvenlik izinlerini değiştirebilirsiniz. Aşağıdaki örnekte, kullanıcı yönetici hesabının ayrıcalıklarını altında çalışacağı hizmetin yapılandırma gösterilmektedir.

```xml
<?xml version="1.0" encoding="utf-8"?>
<ApplicationManifest xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" ApplicationTypeName="MyApplicationType" ApplicationTypeVersion="1.0.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">
   <ServiceManifestImport>
      <ServiceManifestRef ServiceManifestName="MyServiceTypePkg" ServiceManifestVersion="1.0.0" />
      <ConfigOverrides />
      <Policies>
         <RunAsPolicy CodePackageRef="Code" UserRef="SetupAdminUser" EntryPointType="Setup" />
      </Policies>
   </ServiceManifestImport>
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

İlk olarak, oluşturma bir **sorumluları** SetupAdminUser gibi bir kullanıcı adı bölümü. Bu, kullanıcının yöneticiler sistem grubunun bir üyesi olduğunu gösterir.

Sonraki altında **ServiceManifestImport** bölümünde, bu asıl uygulamak için bir ilke yapılandırın **SetupEntryPoint**. Bu Service Fabric gerektiğini bildirir **MySetup.bat** dosyasını çalıştırın, olmalıdır `RunAs` yönetici ayrıcalıklarına sahip. Sahip olduğunuz o *değil* kodda ana giriş noktası için bir ilke uygulanmış **MyServiceHost.exe** sisteminde çalışıyor **NetworkService** hesabı. Bu, tüm hizmet giriş noktaları olarak çalıştırılan varsayılan hesaptır.

Artık dosya MySetup.bat yönetici ayrıcalıklarını test etmek için Visual Studio projesi ekleyelim. Visual Studio'da hizmet projesine sağ tıklayın ve MySetup.bat adlı yeni bir dosya ekleyin.

Ardından, MySetup.bat dosya hizmet paketinde bulunduğundan emin olun. Varsayılan olarak, bu değil. Dosya seçin, bağlam menüsü almak için sağ tıklatın ve **özellikleri**. Özellikler iletişim kutusunda emin **çıktı dizinine Kopyala** ayarlanır **yeniyse Kopyala**. Aşağıdaki ekran görüntüsüne bakın.

![SetupEntryPoint toplu iş dosyası için Visual Studio CopyToOutput][image1]

Şimdi MySetup.bat dosyasını açın ve aşağıdaki komutları ekleyin:

```
REM Set a system environment variable. This requires administrator privilege
setx -m TestVariable "MyValue"
echo System TestVariable set to > out.txt
echo %TestVariable% >> out.txt

REM To delete this system variable us
REM REG delete "HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Session Manager\Environment" /v TestVariable /f
```

Ardından, yapı ve bir yerel geliştirme kümeye çözümü dağıtın. Service Fabric Explorer'da gösterildiği gibi hizmet başladıktan sonra MySetup.bat dosyasını iki yolla başarılı olduğunu görebilirsiniz. Bir PowerShell komut istemi açıp türü:

```
PS C:\ [Environment]::GetEnvironmentVariable("TestVariable","Machine")
MyValue
```

Ardından, burada hizmet dağıtıldı ve Service Fabric Explorer'da--Örneğin, düğüm 2 başlatılan düğüm adını not edin. Ardından, değerini gösterir out.txt dosyayı bulmak için uygulama örnek iş klasöre gidin **TestVariable**. Bu hizmet düğümü 2'ye dağıttıysanız, örneğin, ardından bu yol için gidebilirsiniz **MyApplicationType**:

```
C:\SfDevCluster\Data\_App\Node.2\MyApplicationType_App\work\out.txt
```

### <a name="configure-the-policy-by-using-local-system-accounts"></a>Yerel Sistem hesapları kullanarak ilkeyi yapılandırın
Genellikle, bir yönetici hesabı yerine yerel sistem hesabı kullanarak başlangıç komut dosyasını çalıştırmak için tercih edilir. İyi makineler kullanıcı erişim denetimi (UAC) varsayılan olarak etkin olduğundan, genellikle RunAs ilke Yöneticiler grubunun bir üyesi olarak çalışan işe yaramaz. Böyle durumlarda, **SetupEntryPoint LocalSystem olarak yerine Yöneticiler grubuna eklenmiş yerel bir kullanıcı olarak çalıştırmak için önerilir**. Aşağıdaki örnek, LocalSystem olarak çalışacak şekilde SetupEntryPoint ayarını gösterir:

```xml
<?xml version="1.0" encoding="utf-8"?>
<ApplicationManifest xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" ApplicationTypeName="MyApplicationType" ApplicationTypeVersion="1.0.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">
   <ServiceManifestImport>
      <ServiceManifestRef ServiceManifestName="MyServiceTypePkg" ServiceManifestVersion="1.0.0" />
      <ConfigOverrides />
      <Policies>
         <RunAsPolicy CodePackageRef="Code" UserRef="SetupLocalSystem" EntryPointType="Setup" />
      </Policies>
   </ServiceManifestImport>
   <Principals>
      <Users>
         <User Name="SetupLocalSystem" AccountType="LocalSystem" />
      </Users>
   </Principals>
</ApplicationManifest>
```

Linux kümeleri için bir hizmet veya kurulumu çalıştırmak için giriş noktası olarak **kök**, belirleyebileceğiniz **AccountType** olarak **LocalSystem**.

## <a name="start-powershell-commands-from-a-setup-entry-point"></a>PowerShell komutlarını bir Kurulum giriş noktasından Başlat
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

## <a name="use-console-redirection-for-local-debugging"></a>Yerel hata ayıklama için konsolu yeniden yönlendirmeyi kullanma
Bazen, bir komut dosyası hata ayıklama amacıyla çalışmasını konsol çıkışı görmek yararlıdır. Bunu yapmak için çıktıyı bir dosyaya yazar konsol yeniden yönlendirme ilkesi ayarlayabilirsiniz. Dosya çıktısı adlı uygulama klasörüne yazılır **günlük** düğümünde burada uygulamanın dağıtıldığı ve çalıştırın. (Bu önceki örnekte nerede bulacağını bakın.)

> [!WARNING]
> Hiçbir zaman bu uygulamayı yük devretme etkileyebileceğinden üretimde dağıtılan bir uygulamada konsol yeniden yönlendirme ilkesi kullanın. *Yalnızca* bunu yerel geliştirme ve hata ayıklama amacıyla kullanın.  
> 
> 

Aşağıdaki örnek, konsol yeniden yönlendirmesi FileRetentionCount değeriyle ayarını gösterir:

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

**Bu konsol yeniden yönlendirme ilkesi, komut dosyası hata ayıklama sonra hemen kaldırın**.

## <a name="configure-a-policy-for-service-code-packages"></a>Hizmet kodu paketleri için bir ilke yapılandırın
Önceki adımlarda bir RunAs ilkesi uygulamak için SetupEntryPoint öğrendiniz. Hizmeti ilkeleri uygulanabilecek farklı ilkeleri oluşturmak nasıl içine biraz daha derin bir göz atalım.

### <a name="create-local-user-groups"></a>Yerel kullanıcı grupları oluşturun
Bir gruba eklenecek bir veya daha fazla kullanıcıların kullanıcı grupları oluşturun ve tanımlayın. Bu, farklı hizmet giriş noktaları için birden çok kullanıcı ve grup düzeyinde kullanılabilen bazı ortak ayrıcalıklara sahip olması gerekir yararlıdır. Aşağıdaki örnek adlı bir yerel Grup gösterir **LocalAdminGroup** yönetici ayrıcalıklarına sahip. İki kullanıcılar, Customer1 ve Customer2, bu yerel grubun üyesi yapılır.

```xml
<Principals>
 <Groups>
   <Group Name="LocalAdminGroup">
     <Membership>
       <SystemGroup Name="Administrators"/>
     </Membership>
   </Group>
 </Groups>
  <Users>
     <User Name="Customer1">
        <MemberOf>
           <Group NameRef="LocalAdminGroup" />
        </MemberOf>
     </User>
    <User Name="Customer2">
      <MemberOf>
        <Group NameRef="LocalAdminGroup" />
      </MemberOf>
    </User>
  </Users>
</Principals>
```

### <a name="create-local-users"></a>Yerel kullanıcılar oluşturma
Uygulamasında bir hizmeti güvenli hale getirmek için kullanılan yerel bir kullanıcı oluşturabilirsiniz. Zaman bir **YerelKullanıcı** hesap türü belirtilen uygulama bildirimi ilkeleri bölümünde Service Fabric, uygulamanın dağıtıldığı makinelerde yerel kullanıcı hesaplarını oluşturur. Varsayılan olarak, bu hesapları (örneğin, aşağıdaki örnekte Customer3) uygulama bildiriminde belirtilenlerle aynı adı yok. Bunun yerine, dinamik olarak oluşturulur ve rastgele parolalara sahip.

```xml
<Principals>
  <Users>
     <User Name="Customer3" AccountType="LocalUser" />
  </Users>
</Principals>
```

Uygulamanın kullanıcı hesabı ve parolası (örneğin, için NTLM kimlik doğrulamasını etkinleştir) tüm makinelerde aynı gerektiriyorsa, küme bildiriminde NTLMAuthenticationEnabled true olarak ayarlanmalıdır. Küme bildiriminde tüm makinelerde aynı parola oluşturmak için kullanılan bir NTLMAuthenticationPasswordSecret de belirtmeniz gerekir.

```xml
<Section Name="Hosting">
      <Parameter Name="EndpointProviderEnabled" Value="true"/>
      <Parameter Name="NTLMAuthenticationEnabled" Value="true"/>
      <Parameter Name="NTLMAuthenticationPassworkSecret" Value="******" IsEncrypted="true"/>
 </Section>
```

### <a name="assign-policies-to-the-service-code-packages"></a>Hizmet kodu paketler ilke ataması
**RunAsPolicy** bölümünde bir **ServiceManifestImport** kod paketi çalıştırmak için kullanılması gereken sorumluları bölümünden hesabını belirtir. Ayrıca ilkeleri bölümünde kullanıcı hesapları olan hizmet bildirimi kod paketi ilişkilendirir. Bu kurulum veya ana giriş noktaları için belirtebilirsiniz veya belirtebilirsiniz `All` her ikisini de uygulamak için. Aşağıdaki örnek, uygulanmakta olan farklı ilkeleri gösterir:

```xml
<Policies>
<RunAsPolicy CodePackageRef="Code" UserRef="LocalAdmin" EntryPointType="Setup"/>
<RunAsPolicy CodePackageRef="Code" UserRef="Customer3" EntryPointType="Main"/>
</Policies>
```

Varsa **EntryPointType** belirtilmezse, varsayılan olarak EntryPointType için ayarlanmış "Ana" =. Belirtme **SetupEntryPoint** belirli yüksek ayrıcalıklı kurulum işlemlerini sistem hesabı altında çalıştırmak istediğinizde kullanışlıdır. Gerçek servis kodu düşük ayrıcalıklı hesap altında çalışır.

### <a name="apply-a-default-policy-to-all-service-code-packages"></a>Varsayılan bir ilke tüm hizmet kod paketleri için geçerlidir
Kullandığınız **DefaultRunAsPolicy** bölüm tüm kodları için varsayılan bir kullanıcı hesabı paketleri belirtmek için belirli bir sahip değilseniz **RunAsPolicy** tanımlanmış. Bir uygulama tarafından kullanılan hizmet bildiriminde belirtilen kod paketleri çoğunu aynı kullanıcı altında çalıştırmanız gerekiyorsa, uygulamayı yalnızca bu kullanıcı hesabıyla varsayılan bir RunAs ilkesi tanımlayabilirsiniz. Kod paketi yoksa belirleyen aşağıdaki örnekte bir **RunAsPolicy** belirtilen kod paketi altında çalışması gereken **MyDefaultAccount** ilkeleri bölümünde belirtilen.

```xml
<Policies>
  <DefaultRunAsPolicy UserRef="MyDefaultAccount"/>
</Policies>
```
### <a name="use-an-active-directory-domain-group-or-user"></a>Bir Active Directory etki alanı grubu veya kullanıcı kullanın
Tek başına yükleyici kullanarak Windows sunucusuna yüklenen Service Fabric bir örneği için bir Active Directory kullanıcı veya grup hesabı için kimlik bilgileri altında hizmeti çalıştırabilirsiniz. Bu içi etki alanınızda Active Directory ve Azure Active Directory (Azure AD) değil. Bir etki alanı kullanıcısı veya grubu kullanarak izinleri verilmiş olması (örneğin, dosya paylaşımları) etki alanındaki diğer kaynaklara erişebilir.

Aşağıdaki örnek, bir Active Directory kullanıcı olarak adlandırılır ve gösterir *TestUser* kendi etki alanı ile bir sertifika kullanılarak şifrelenmiş parola adlı *MyCert*. Kullanabileceğiniz `Invoke-ServiceFabricEncryptText` gizli şifreli metin oluşturmak için PowerShell komutu. Bkz: [Service Fabric uygulamaları parolalarında yönetme](service-fabric-application-secret-management.md) Ayrıntılar için.

Bir bant dışı yöntem kullanarak yerel makineye parolasının şifresi sertifikanın özel anahtarı dağıtmanız gerekir (Azure'da, Azure Resource Manager aracılığıyla budur). Ardından, Service Fabric makineye hizmet paketini dağıtırken, gizli şifresini çözmek ve bu kimlik bilgileri altında çalıştırmak için Active Directory ile (kullanıcı adı ile birlikte) kimlik doğrulaması yapmak kullanabilirsiniz.

```xml
<Principals>
  <Users>
    <User Name="TestUser" AccountType="DomainUser" AccountName="Domain\User" Password="[Put encrypted password here using MyCert certificate]" PasswordEncrypted="true" />
  </Users>
</Principals>
<Policies>
  <DefaultRunAsPolicy UserRef="TestUser" />
  <SecurityAccessPolicies>
    <SecurityAccessPolicy ResourceRef="MyCert" PrincipalRef="TestUser" GrantRights="Full" ResourceType="Certificate" />
  </SecurityAccessPolicies>
</Policies>
<Certificates>
```
### <a name="use-a-group-managed-service-account"></a>Grup yönetilen hizmet hesabı.
Tek başına yükleyici kullanarak Windows sunucusuna yüklenen Service Fabric bir örneği için bir grup olarak yönetilen hizmet hesabı (gMSA) hizmeti çalışabilir. Bu Active Directory olduğuna dikkat edin, etki alanı içinde şirket içi ve Azure Active Directory (Azure AD) ile değil. Bir gmsa'yı kullanarak bir parola olduğundan veya şifrelenmiş parola depolanır `Application Manifest`.

Aşağıdaki örnek olarak adlandırılan bir gMSA hesabının nasıl oluşturulacağını gösterir *svc Test$*; Bu yönetilen hizmet hesabı küme düğümlerine; dağıtma ve kullanıcı asıl adı yapılandırma.

##### <a name="prerequisites"></a>Önkoşullar.
- Etki alanı bir KDS kök anahtarı gerekir.
- Etki alanı bir Windows Server 2012 veya sonraki işlev düzeyinde olması gerekir.

##### <a name="example"></a>Örnek
1. Bir grup yönetilen hizmet hesabı kullanarak oluşturduğunuz active directory etki alanı yöneticisi olan `New-ADServiceAccount` komutunu ve emin `PrincipalsAllowedToRetrieveManagedPassword` service fabric küme düğümlerinin tümü içerir. Unutmayın `AccountName`, `DnsHostName`, ve `ServicePrincipalName` benzersiz olması gerekir.
```
New-ADServiceAccount -name svc-Test$ -DnsHostName svc-test.contoso.com  -ServicePrincipalNames http/svc-test.contoso.com -PrincipalsAllowedToRetrieveManagedPassword SfNode0$,SfNode1$,SfNode2$,SfNode3$,SfNode4$
```
2. Her service fabric küme düğümlerinin (örneğin, `SfNode0$,SfNode1$,SfNode2$,SfNode3$,SfNode4$`) yükleyin ve gMSA test.
```
Add-WindowsFeature RSAT-AD-PowerShell
Install-AdServiceAccount svc-Test$
Test-AdServiceAccount svc-Test$
```
3. Kullanıcı asıl adı yapılandırmak ve kullanıcı başvurmak için RunAsPolicy yapılandırın.
```xml
<?xml version="1.0" encoding="utf-8"?>
<ApplicationManifest xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" ApplicationTypeName="MyApplicationType" ApplicationTypeVersion="1.0.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">
   <ServiceManifestImport>
      <ServiceManifestRef ServiceManifestName="MyServiceTypePkg" ServiceManifestVersion="1.0.0" />
      <ConfigOverrides />
      <Policies>
         <RunAsPolicy CodePackageRef="Code" UserRef="DomaingMSA"/>
      </Policies>
   </ServiceManifestImport>
  <Principals>
    <Users>
      <User Name="DomaingMSA" AccountType="ManagedServiceAccount" AccountName="domain\svc-Test$"/>
    </Users>
  </Principals>
</ApplicationManifest>
```

## <a name="assign-a-security-access-policy-for-http-and-https-endpoints"></a>HTTP ve HTTPS uç noktaları için bir güvenlik erişim ilkesi atama
Bir hizmet için bir RunAs ilkesi uygulamak ve uç nokta kaynakları HTTP protokolü ile hizmet bildirimi bildirir, belirtmelisiniz bir **SecurityAccessPolicy** Bu uç noktaları için ayrılmış bağlantı noktaları doğru olduğundan emin olmak için Access-control hizmetinin altında çalıştığı RunAs kullanıcı hesabı için listelenen. Aksi takdirde, **http.sys** hizmetine erişiminiz yok ve istemciden çağrıları hatalarıyla alın. Aşağıdaki örnek, adlı bir uç nokta Customer1 hesabı uygular **EndpointName**, sağlayan, tam erişim hakları.

```xml
<Policies>
   <RunAsPolicy CodePackageRef="Code" UserRef="Customer1" />
   <!--SecurityAccessPolicy is needed if RunAsPolicy is defined and the Endpoint is http -->
   <SecurityAccessPolicy ResourceRef="EndpointName" PrincipalRef="Customer1" />
</Policies>
```

HTTPS uç noktası için aynı zamanda istemciye döndürmek için sertifikanın adı belirtmeniz gerekir. Kullanarak bunu yapabilirsiniz **EndpointBindingPolicy**, uygulama bildirimi sertifikaları bölümünde tanımlanan sertifika ile.

```xml
<Policies>
   <RunAsPolicy CodePackageRef="Code" UserRef="Customer1" />
  <!--SecurityAccessPolicy is needed if RunAsPolicy is defined and the Endpoint is http -->
   <SecurityAccessPolicy ResourceRef="EndpointName" PrincipalRef="Customer1" />
  <!--EndpointBindingPolicy is needed if the EndpointName is secured with https -->
  <EndpointBindingPolicy EndpointRef="EndpointName" CertificateRef="Cert1" />
</Policies
```
## <a name="upgrading-multiple-applications-with-https-endpoints"></a>Https uç noktaları birden çok uygulama yükseltme
Kullanmamaya dikkat etmeniz gereken **aynı bağlantı noktasını** http kullanırken aynı uygulamanın farklı örnekleri için**s**. Service Fabric uygulaması örneklerden birini için sertifika yükseltmeniz mümkün olmayacaktır nedenidir. Örneğin, 1 veya uygulama 2 hem uygulaması kendi sertifikası 1 cert 2 yükseltmek istiyorsanız. Yükseltme gerçekleştiğinde, diğer uygulama hala kullanarak olsa bile Service Fabric sertifikası 1 kayıt http.sys ile yukarı temizlendi. Bunu önlemek için Service Fabric olmadığını zaten başka bir uygulama örneği bağlantı noktası (http.sys) nedeniyle sertifika ile kayıtlı ve işlem başarısız algılar.

Bu nedenle Service Fabric kullanan iki farklı hizmet yükseltmeyi desteklemez **aynı bağlantı noktasını** farklı uygulama durumlarda. Diğer bir deyişle, aynı bağlantı noktasında farklı Hizmetleri aynı sertifikayı kullanamazsınız. Aynı bağlantı noktasında paylaşılan bir sertifikanız gerekiyorsa, hizmetleri yerleştirme kısıtlamalarına sahip farklı makinelerde yerleştirilir emin olmak gerekir. Veya her uygulama örneği her hizmet için Service Fabric dinamik bağlantı noktaları mümkünse kullanmayı düşünün. 

Https, "Windows HTTP sunucu API birden çok sertifika bir bağlantı noktasını paylaşmak uygulamalar için desteklemiyor." bildiren uyarı bir hata ile yükseltme bir hata görürseniz

## <a name="a-complete-application-manifest-example"></a>Tam uygulama bildirim örneği
Aşağıdaki uygulama bildirimi birçok farklı ayarı gösterir:

```xml
<?xml version="1.0" encoding="utf-8"?>
<ApplicationManifest xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" ApplicationTypeName="Application3Type" ApplicationTypeVersion="1.0.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">
   <Parameters>
      <Parameter Name="Stateless1_InstanceCount" DefaultValue="-1" />
   </Parameters>
   <ServiceManifestImport>
      <ServiceManifestRef ServiceManifestName="Stateless1Pkg" ServiceManifestVersion="1.0.0" />
      <ConfigOverrides />
      <Policies>
         <RunAsPolicy CodePackageRef="Code" UserRef="Customer1" />
         <RunAsPolicy CodePackageRef="Code" UserRef="LocalAdmin" EntryPointType="Setup" />
        <!--SecurityAccessPolicy is needed if RunAsPolicy is defined and the Endpoint is http -->
         <SecurityAccessPolicy ResourceRef="EndpointName" PrincipalRef="Customer1" />
        <!--EndpointBindingPolicy is needed the EndpointName is secured with https -->
        <EndpointBindingPolicy EndpointRef="EndpointName" CertificateRef="Cert1" />
     </Policies>
   </ServiceManifestImport>
   <DefaultServices>
      <Service Name="Stateless1">
         <StatelessService ServiceTypeName="Stateless1Type" InstanceCount="[Stateless1_InstanceCount]">
            <SingletonPartition />
         </StatelessService>
      </Service>
   </DefaultServices>
   <Principals>
      <Groups>
         <Group Name="LocalAdminGroup">
            <Membership>
               <SystemGroup Name="Administrators" />
            </Membership>
         </Group>
      </Groups>
      <Users>
         <User Name="LocalAdmin">
            <MemberOf>
               <Group NameRef="LocalAdminGroup" />
            </MemberOf>
         </User>
         <!--Customer1 below create a local account that this service runs under -->
         <User Name="Customer1" />
         <User Name="MyDefaultAccount" AccountType="NetworkService" />
      </Users>
   </Principals>
   <Policies>
      <DefaultRunAsPolicy UserRef="LocalAdmin" />
   </Policies>
   <Certificates>
     <EndpointCertificate Name="Cert1" X509FindValue="FF EE E0 TT JJ DD JJ EE EE XX 23 4T 66 "/>
  </Certificates>
</ApplicationManifest>
```


<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Sonraki adımlar
* [Uygulama modeli anlama](service-fabric-application-model.md)
* [Bir hizmet bildirimi kaynakları belirtme](service-fabric-service-manifest-resources.md)
* [Bir uygulamayı dağıtma](service-fabric-deploy-remove-applications.md)

[image1]: ./media/service-fabric-application-runas-security/copy-to-output.png
