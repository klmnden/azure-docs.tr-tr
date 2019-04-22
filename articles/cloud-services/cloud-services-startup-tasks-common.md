---
title: Bulut Hizmetleri için genel başlangıç görevleri | Microsoft Docs
description: Genel başlangıç görevleri, cloud services web rolü veya çalışan rolü gerçekleştirmek isteyebileceğiniz bazı örnekler sağlar.
services: cloud-services
documentationcenter: ''
author: jpconnock
manager: timlt
editor: ''
ms.assetid: a7095dad-1ee7-4141-bc6a-ef19a6e570f1
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/18/2017
ms.author: jeconnoc
ms.openlocfilehash: 0a2e2a3d817140a6ab15dab0093b4025a3bfd76c
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "58916668"
---
# <a name="common-cloud-service-startup-tasks"></a>Bulut hizmeti genel başlangıç görevleri
Bu makalede, bulut hizmetinizin gerçekleştirmek isteyebileceğiniz genel başlangıç görevleri bazı örnekler sağlar. Başlangıç görevleri rol başlamadan önce işlemleri gerçekleştirmek için kullanabilirsiniz. Gerçekleştirmek isteyebileceğiniz işlemler, bir bileşeni yükleniyor, COM bileşenleri kaydediliyor, kayıt defteri anahtarlarını ayarlamak veya uzun süre çalışan bir işlem başlatılıyor içerir. 

Bkz: [bu makalede](cloud-services-startup-tasks.md) başlangıç görevleri nasıl çalıştığını ve bir başlangıç görevi tanımlayan girişleri oluşturmak özel olarak nasıl anlamak için.

> [!NOTE]
> Başlangıç görevleri, sanal makineler, bulut hizmeti Web ve çalışan rolleri için geçerli değildir.
> 

## <a name="define-environment-variables-before-a-role-starts"></a>Bir rol başlamadan önce ortam değişkenleri tanımlayın
Belirli bir görev için tanımlanan ortam değişkenleri ihtiyacınız varsa, [ortam] öğe içinde [görev] öğesi.

```xml
<ServiceDefinition name="MyService" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition">
    <WorkerRole name="WorkerRole1">
        ...
        <Startup>
            <Task commandLine="Startup.cmd" executionContext="limited" taskType="simple">
                <Environment>
                    <Variable name="MyEnvironmentVariable" value="MyVariableValue" />
                </Environment>
            </Task>
        </Startup>
    </WorkerRole>
</ServiceDefinition>
```

Değişkenleri de kullanabilirsiniz bir [geçerli Azure XPath değeri](cloud-services-role-config-xpath.md) dağıtım hakkında bir şey başvurmak için. Yerine `value` tanımlayın, öznitelik bir [RoleInstanceValue] alt öğesi.

```xml
<Variable name="PathToStartupStorage">
    <RoleInstanceValue xpath="/RoleEnvironment/CurrentInstance/LocalResources/LocalResource[@name='StartupLocalStorage']/@path" />
</Variable>
```


## <a name="configure-iis-startup-with-appcmdexe"></a>IIS başlatma AppCmd.exe ile yapılandırma
[AppCmd.exe](https://technet.microsoft.com/library/jj635852.aspx) komut satırı aracı, azure'da başlangıçta IIS ayarlarını yönetmek için kullanılabilir. *AppCmd.exe* başlangıç görevleri, Azure üzerinde kullanım için yapılandırma ayarları için kullanışlı, komut satırı erişim sağlar. Kullanarak *AppCmd.exe*, Web sitesi ayarları eklediyseniz, değiştirme veya uygulamalar ve siteler için kaldırıldı.

Ancak, kullanımında dikkat edilmesi gereken birkaç nokta vardır *AppCmd.exe* olarak başlangıç görevi:

* Başlangıç görevleri, yeniden başlatmalar arasında birden çok kez çalıştırılabilir. Örneğin, ne zaman bir rol geri dönüştürür.
* Varsa bir *AppCmd.exe* eylemi birden çok kez gerçekleştirilen, hataya neden. Örneğin, bir bölüme eklenmeye çalışılması *Web.config* iki kez hataya neden.
* Başlangıç görevleri, bunlar bir sıfır olmayan çıkış kodu döndürmesi durumunda başarısız veya **errorlevel**. Örneğin, *AppCmd.exe* bir hata oluşturur.

Denetlemek için iyi bir uygulamadır **errorlevel** arama sonra *AppCmd.exe*, çağrısını sarmalamak durumunda ne yapacaklarını kolay olduğu *AppCmd.exe* ile bir *.cmd* dosya. Bilinen bir algılama **errorlevel** yanıt, bunu yoksayabilir, veya geri geçirin.

Tarafından döndürülen errorlevel *AppCmd.exe* wınerror dosyasında listelenen ve üzerinde görülebilir [MSDN](/windows/desktop/Debug/system-error-codes--0-499-).

### <a name="example-of-managing-the-error-level"></a>Örnek hata düzeyini yönetme
Bu örnek için JSON için bir sıkıştırma bölümü ve bir sıkıştırma giriş ekler *Web.config* dosyasıyla hata işleme ve günlüğe kaydetme.

İlgili bölümleri [ServiceDefinition.csdef] dosya burada gösterilen, ayar içeren [executionContext](/previous-versions/azure/reference/gg557552(v=azure.100)#Task) özniteliğini `elevated` vermek *AppCmd.exe* ayarlarını değiştirmek için yeterli izinlere *Web.config* dosyası:

```xml
<ServiceDefinition name="MyService" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition">
    <WorkerRole name="WorkerRole1">
        ...
        <Startup>
            <Task commandLine="Startup.cmd" executionContext="elevated" taskType="simple" />
        </Startup>
    </WorkerRole>
</ServiceDefinition>
```

*Startup.cmd* toplu iş dosyası kullanan *AppCmd.exe* için JSON için bir sıkıştırma bölümü ve bir sıkıştırma giriş eklemek için *Web.config* dosya. Beklenen **errorlevel** 183 sıfır VERIFY kullanma için ayarlanır. EXE komut satırı programı. Beklenmeyen errorlevels StartupErrorLog.txt için günlüğe kaydedilir.

```cmd
REM   *** Add a compression section to the Web.config file. ***
%windir%\system32\inetsrv\appcmd set config /section:urlCompression /doDynamicCompression:True /commit:apphost >> "%TEMP%\StartupLog.txt" 2>&1

REM   ERRORLEVEL 183 occurs when trying to add a section that already exists. This error is expected if this
REM   batch file were executed twice. This can occur and must be accounted for in an Azure startup
REM   task. To handle this situation, set the ERRORLEVEL to zero by using the Verify command. The Verify
REM   command will safely set the ERRORLEVEL to zero.
IF %ERRORLEVEL% EQU 183 DO VERIFY > NUL

REM   If the ERRORLEVEL is not zero at this point, some other error occurred.
IF %ERRORLEVEL% NEQ 0 (
    ECHO Error adding a compression section to the Web.config file. >> "%TEMP%\StartupLog.txt" 2>&1
    GOTO ErrorExit
)

REM   *** Add compression for json. ***
%windir%\system32\inetsrv\appcmd set config  -section:system.webServer/httpCompression /+"dynamicTypes.[mimeType='application/json; charset=utf-8',enabled='True']" /commit:apphost >> "%TEMP%\StartupLog.txt" 2>&1
IF %ERRORLEVEL% EQU 183 VERIFY > NUL
IF %ERRORLEVEL% NEQ 0 (
    ECHO Error adding the JSON compression type to the Web.config file. >> "%TEMP%\StartupLog.txt" 2>&1
    GOTO ErrorExit
)

REM   *** Exit batch file. ***
EXIT /b 0

REM   *** Log error and exit ***
:ErrorExit
REM   Report the date, time, and ERRORLEVEL of the error.
DATE /T >> "%TEMP%\StartupLog.txt" 2>&1
TIME /T >> "%TEMP%\StartupLog.txt" 2>&1
ECHO An error occurred during startup. ERRORLEVEL = %ERRORLEVEL% >> "%TEMP%\StartupLog.txt" 2>&1
EXIT %ERRORLEVEL%
```

## <a name="add-firewall-rules"></a>Güvenlik duvarı kuralları ekleme
Azure'da, etkili bir şekilde iki güvenlik duvarı vardır. Sanal makine ve dış dünya arasındaki bağlantıları ilk Güvenlik Duvarı'nı denetler. Bu güvenlik duvarı tarafından denetlenir [uç noktaları] öğesinde [ServiceDefinition.csdef] dosya.

Güvenlik Duvarı'nı ikinci sanal makine ve sanal makinenin içinde işlemleri arasında bağlantılar denetler. Bu güvenlik duvarı tarafından denetlenebilir `netsh advfirewall firewall` komut satırı aracı.

Azure, rolleriniz içinde başlatılan işlemler için güvenlik duvarı kuralları oluşturur. Örneğin, Azure, otomatik olarak bir hizmet veya programın başlattığınızda, Internet ile iletişim kurmak bu hizmet izin vermek için gerekli güvenlik duvarı kuralları oluşturur. Ancak rolünüz (örneğin, bir COM + hizmet veya Windows zamanlanmış bir görev) dışında bir işlem tarafından başlatılan bir hizmet oluşturursanız, el ile ilgili hizmete erişmesine izin vermek için bir güvenlik duvarı kuralı oluşturmanız gerekir. Bu güvenlik duvarı kuralları, bir başlangıç görevi kullanılarak oluşturulabilir.

Bir güvenlik duvarı kuralı oluşturur bir başlangıç görevi olmalıdır bir [executionContext][görev] , **yükseltilmiş**. Aşağıdaki başlangıç görevi [ServiceDefinition.csdef] dosya.

```xml
<ServiceDefinition name="MyService" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition">
    <WorkerRole name="WorkerRole1">
        ...
        <Startup>
            <Task commandLine="AddFirewallRules.cmd" executionContext="elevated" taskType="simple" />
        </Startup>
    </WorkerRole>
</ServiceDefinition>
```

Güvenlik duvarı kuralı eklemek için uygun kullanmalısınız `netsh advfirewall firewall` başlangıç toplu iş dosyasında komutları. Bu örnekte, başlangıç görevinin TCP bağlantı noktası 80 için güvenlik ve şifreleme gerektirir.

```cmd
REM   Add a firewall rule in a startup task.

REM   Add an inbound rule requiring security and encryption for TCP port 80 traffic.
netsh advfirewall firewall add rule name="Require Encryption for Inbound TCP/80" protocol=TCP dir=in localport=80 security=authdynenc action=allow >> "%TEMP%\StartupLog.txt" 2>&1

REM   If an error occurred, return the errorlevel.
EXIT /B %errorlevel%
```

## <a name="block-a-specific-ip-address"></a>Belirli bir IP adres bloğu
IIS değiştirerek belirtilen IP adresleri kümesi için bir Azure web rolü erişimi kısıtlayabilirsiniz **web.config** dosya. Ayrıca kilidini açan bir komut dosyası kullanmanız gerekir **IPSecurity** bölümünü **ApplicationHost.config** dosya.

Kilidini açmak için **IPSecurity** bölümünü **ApplicationHost.config** dosya, rol başlangıcı sırasında çalıştırılan bir komut dosyası oluşturun. Bir klasör oluşturun web rolü olarak adlandırılan kök düzeyinde **başlangıç** ve bu klasördeki adlı bir toplu iş dosyasını oluşturun **startup.cmd**. Bu dosyayı Visual Studio projenize ekleyin ve özelliklerini ayarlamak **her zaman Kopyala** paketinize dahil emin olmak için.

Aşağıdaki başlangıç görevi [ServiceDefinition.csdef] dosya.

```xml
<ServiceDefinition name="MyService" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition">
    <WebRole name="WebRole1">
        ...
        <Startup>
            <Task commandLine="startup.cmd" executionContext="elevated" />
        </Startup>
    </WebRole>
</ServiceDefinition>
```

Bu komut ekleme **startup.cmd** dosyası:

```cmd
@echo off
@echo Installing "IPv4 Address and Domain Restrictions" feature 
powershell -ExecutionPolicy Unrestricted -command "Install-WindowsFeature Web-IP-Security"
@echo Unlocking configuration for "IPv4 Address and Domain Restrictions" feature 
%windir%\system32\inetsrv\AppCmd.exe unlock config -section:system.webServer/security/ipSecurity
```

Bu görev neden **startup.cmd** web rolü başlatılır, gerekli sağlanarak her zaman çalıştırılacak toplu iş dosyası **IPSecurity** bölüm kilidi.

Son olarak, değişiklik [system.webServer bölümü](https://www.iis.net/configreference/system.webserver/security/ipsecurity#005) web rolün **web.config** dosyaya erişim izni verilen IP adreslerinin bir listesi aşağıdaki örnekte gösterildiği gibi ekleyin:

Bu örnek yapılandırma **sağlayan** tanımlı iki dışında sunucuya erişmek için tüm IP'ler

```xml
<system.webServer>
    <security>
    <!--Unlisted IP addresses are granted access-->
    <ipSecurity>
        <!--The following IP addresses are denied access-->
        <add allowed="false" ipAddress="192.168.100.1" subnetMask="255.255.0.0" />
        <add allowed="false" ipAddress="192.168.100.2" subnetMask="255.255.0.0" />
    </ipSecurity>
    </security>
</system.webServer>
```

Bu örnek yapılandırma **engellediği** sunucu dışında tanımlanmış iki erişmesini tüm IP'lere.

```xml
<system.webServer>
    <security>
    <!--Unlisted IP addresses are denied access-->
    <ipSecurity allowUnlisted="false">
        <!--The following IP addresses are granted access-->
        <add allowed="true" ipAddress="192.168.100.1" subnetMask="255.255.0.0" />
        <add allowed="true" ipAddress="192.168.100.2" subnetMask="255.255.0.0" />
    </ipSecurity>
    </security>
</system.webServer>
```

## <a name="create-a-powershell-startup-task"></a>Bir PowerShell başlangıç görevi oluşturun
Windows PowerShell betiklerini doğrudan çağrılamaz [ServiceDefinition.csdef] dosyası, ancak bunlar ınvoked from içinde başlangıç toplu iş dosyası.

PowerShell (varsayılan), imzalanmamış komut dosyalarının çalıştırmaz. Betiğinizi oturum sürece, imzalanmamış komut dosyalarını çalıştırmak için PowerShell yapılandırmanız gerekir. İmzasız betikleri çalıştırmasına olanak **ExecutionPolicy** ayarlanmalıdır **Kısıtlanmamış**. **ExecutionPolicy** ayarı kullanmak Windows PowerShell sürümüne bağlıdır.

```cmd
REM   Run an unsigned PowerShell script and log the output
PowerShell -ExecutionPolicy Unrestricted .\startup.ps1 >> "%TEMP%\StartupLog.txt" 2>&1

REM   If an error occurred, return the errorlevel.
EXIT /B %errorlevel%
```

Çalıştırmalar PowerShell 2.0 bir konuk OS veya sürüm 2 çalıştırmaya zorlayabilirsiniz 1.0 kullanıyorsanız ve bu mümkün değilse, sürüm 1'ı kullanın.

```cmd
REM   Attempt to set the execution policy by using PowerShell version 2.0 syntax.
PowerShell -Version 2.0 -ExecutionPolicy Unrestricted .\startup.ps1 >> "%TEMP%\StartupLog.txt" 2>&1

REM   If PowerShell version 2.0 isn't available. Set the execution policy by using the PowerShell
IF %ERRORLEVEL% EQU -393216 (
   PowerShell -Command "Set-ExecutionPolicy Unrestricted" >> "%TEMP%\StartupLog.txt" 2>&1
   PowerShell .\startup.ps1 >> "%TEMP%\StartupLog.txt" 2>&1
)

REM   If an error occurred, return the errorlevel.
EXIT /B %errorlevel%
```

## <a name="create-files-in-local-storage-from-a-startup-task"></a>Başlangıç görevi yerel depolama dosyaları oluşturma
Daha sonra uygulamanız tarafından erişilen, başlangıç görevi tarafından oluşturulan dosyaları depolamak için bir yerel depolama kaynağı kullanabilirsiniz.

Yerel depolama kaynağı oluşturmak için bir [LocalResources] bölümünü [ServiceDefinition.csdef] dosya ve ardından eklemek [LocalStorage] alt öğesi. Yerel depolama kaynağı başlangıç göreviniz için benzersiz bir ad ve uygun bir boyut sağlar.

Başlangıç göreviniz yerel depolama kaynağı kullanmak için yerel depolama kaynak konumuna başvurmak için bir ortam değişkenini oluşturmak gerekir. Ardından başlangıç görevi ve uygulama dosyalarını okuma ve yerel depolama kaynak yazma olanağına sahip olursunuz.

İlgili bölümleri **ServiceDefinition.csdef** dosya burada gösterilir:

```xml
<ServiceDefinition name="MyService" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition">
  <WorkerRole name="WorkerRole1">
    ...

    <LocalResources>
      <LocalStorage name="StartupLocalStorage" sizeInMB="5"/>
    </LocalResources>

    <Startup>
      <Task commandLine="Startup.cmd" executionContext="limited" taskType="simple">
        <Environment>
          <Variable name="PathToStartupStorage">
            <RoleInstanceValue xpath="/RoleEnvironment/CurrentInstance/LocalResources/LocalResource[@name='StartupLocalStorage']/@path" />
          </Variable>
        </Environment>
      </Task>
    </Startup>
  </WorkerRole>
</ServiceDefinition>
```

Örneğin, bu **Startup.cmd** toplu iş dosyası kullanan **PathToStartupStorage** ortam değişkeni dosyasını oluşturmak için **MyTest.txt** yerel depolama konumunda.

```cmd
REM   Create a simple text file.

ECHO This text will go into the MyTest.txt file which will be in the    >  "%PathToStartupStorage%\MyTest.txt"
ECHO path pointed to by the PathToStartupStorage environment variable.  >> "%PathToStartupStorage%\MyTest.txt"
ECHO The contents of the PathToStartupStorage environment variable is   >> "%PathToStartupStorage%\MyTest.txt"
ECHO "%PathToStartupStorage%".                                          >> "%PathToStartupStorage%\MyTest.txt"

REM   Exit the batch file with ERRORLEVEL 0.

EXIT /b 0
```

Azure SDK'sından kullanarak yerel depolama klasörüne erişebilecek [GetLocalResource](/previous-versions/azure/reference/ee772845(v=azure.100)) yöntemi.

```csharp
string localStoragePath = Microsoft.WindowsAzure.ServiceRuntime.RoleEnvironment.GetLocalResource("StartupLocalStorage").RootPath;

string fileContent = System.IO.File.ReadAllText(System.IO.Path.Combine(localStoragePath, "MyTestFile.txt"));
```

## <a name="run-in-the-emulator-or-cloud"></a>Öykünücü veya bulutta çalıştırın
Başlangıç göreviniz işlem öykünücüsünde olduğunda karşılaştırıldığında bulutta çalışırken farklı adımları gerçekleştirmeniz olabilir. Örneğin, yalnızca öykünücüde çalışırken SQL verilerinizi yeni bir kopyasını kullanmak isteyebilirsiniz. Veya öykünücüde çalıştırırken yapmanız gerekmez bulut için bazı performans iyileştirmelerini yapmak isteyebilirsiniz.

İşlem öykünücüsü ve bulut üzerinde farklı eylemleri gerçekleştirmek için bu özelliği, bir ortam değişkeninde oluşturarak gerçekleştirilebilir [ServiceDefinition.csdef] dosya. Ardından başlangıç göreviniz için bir değer bu ortam değişkenini test.

Ortam değişkenini oluşturmak için Ekle [değişkeni]/[RoleInstanceValue] öğesi ve bir XPath değeri oluşturun `/RoleEnvironment/Deployment/@emulated`. Değerini **ComputeEmulatorRunning %** ortam değişkenidir `true` işlem öykünücüsü üzerinde çalışırken ve `false` bulutta çalışırken.

```xml
<ServiceDefinition name="MyService" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition">
  <WorkerRole name="WorkerRole1">

    ...

    <Startup>
      <Task commandLine="Startup.cmd" executionContext="limited" taskType="simple">
        <Environment>
          <Variable name="ComputeEmulatorRunning">
            <RoleInstanceValue xpath="/RoleEnvironment/Deployment/@emulated" />
          </Variable>
        </Environment>
      </Task>
    </Startup>

  </WorkerRole>
</ServiceDefinition>
```

Görev kontrol edebilirsiniz **ComputeEmulatorRunning %** farklı eylemler gerçekleştirmesini ortam değişkeni tabanlı rol Bulut veya öykünücü çalışan. O ortam değişkeni denetleyen .cmd bir kabuk betiği aşağıda verilmiştir.

```cmd
REM   Check if this task is running on the compute emulator.

IF "%ComputeEmulatorRunning%" == "true" (
    REM   This task is running on the compute emulator. Perform tasks that must be run only in the compute emulator.
) ELSE (
    REM   This task is running on the cloud. Perform tasks that must be run only in the cloud.
)
```


## <a name="detect-that-your-task-has-already-run"></a>Görev zaten çalıştırıldı algılayın
Başlangıç görevleri yeniden çalıştırmak neden yeniden başlatma geri dönüşüm rolü. Bir görev, barındırma VM üzerinde zaten çalıştırılıp çalıştırılmadığını gösteren bayrak yoktur. Bazı görevler burada birden çok kez çalıştırmak farketmez olabilir. Ancak, bir görevin birden çok kez çalışmasını engellemek için gereken bir durum halinde çalıştırabilirsiniz.

Bir görevi zaten çalıştırıldı algılamak için en basit yolu, bir dosyada oluşturmaktır **% TEMP %** görev başarılı olduğunda klasörünü ve bu görevin başlangıcında Ara. Sizin için yapan bir örnek cmd Kabuğu betiği aşağıda verilmiştir.

```cmd
REM   If Task1_Success.txt exists, then Application 1 is already installed.
IF EXIST "%PathToApp1Install%\Task1_Success.txt" (
  ECHO Application 1 is already installed. Exiting. >> "%TEMP%\StartupLog.txt" 2>&1
  GOTO Finish
)

REM   Run your real exe task
ECHO Running XYZ >> "%TEMP%\StartupLog.txt" 2>&1
"%PathToApp1Install%\setup.exe" >> "%TEMP%\StartupLog.txt" 2>&1

IF %ERRORLEVEL% EQU 0 (
  REM   The application installed without error. Create a file to indicate that the task
  REM   does not need to be run again.

  ECHO This line will create a file to indicate that Application 1 installed correctly. > "%PathToApp1Install%\Task1_Success.txt"

) ELSE (
  REM   An error occurred. Log the error and exit with the error code.

  DATE /T >> "%TEMP%\StartupLog.txt" 2>&1
  TIME /T >> "%TEMP%\StartupLog.txt" 2>&1
  ECHO  An error occurred running task 1. Errorlevel = %ERRORLEVEL%. >> "%TEMP%\StartupLog.txt" 2>&1

  EXIT %ERRORLEVEL%
)

:Finish

REM   Exit normally.
EXIT /B 0
```

## <a name="task-best-practices"></a>Görev en iyi uygulamalar
Görev, web veya çalışan rolü için yapılandırırken izlemelidir en iyi yöntemlerden bazıları aşağıda verilmiştir.

### <a name="always-log-startup-activities"></a>Her zaman günlük başlangıç etkinlikleri
Visual Studio için toplu iş dosyaları işlemi mümkün olduğunca olabildiğince fazla veri almak üzere iyi olacak şekilde toplu iş dosyaları hata ayıklayıcısı sağlamıyor. Toplu iş dosyaları, çıkışı günlük hem de **stdout** ve **stderr**, hata ayıklama ve toplu iş dosyaları düzeltmek çalışırken önemli bilgiler verir. Hem günlük için **stdout** ve **stderr** StartupLog.txt için dizinde dosya tarafından işaret edilen **% TEMP %** ortam değişkeni, metin ekleyin `>>  "%TEMP%\\StartupLog.txt" 2>&1` sonuna günlüğe kaydetmek istediğiniz belirli satırlar. Örneğin, setup.exe yürütülecek **PathToApp1Install %** dizini:

    "%PathToApp1Install%\setup.exe" >> "%TEMP%\StartupLog.txt" 2>&1

Xml dosyanızı basitleştirmek için bir sarmalayıcı oluşturabilirsiniz *cmd* tüm startup'ınız çağıran dosya görevleri ile birlikte günlüğe kaydetme ve her bir alt görev aynı ortam değişkenlerini paylaşımları sağlar.

Bu sinir kullanmanıza rağmen bulabilirsiniz `>> "%TEMP%\StartupLog.txt" 2>&1` ucundaki her başlangıç görevi. Görev günlüğü, günlük kaydı işleyen bir sarmalayıcı oluşturarak uygulayabilir. Bu kapsayıcı, çalıştırmak istediğiniz gerçek toplu iş dosyasını çağırır. Hedef toplu iş dosyasından herhangi bir çıktı yönlendirilecek *Startuplog.txt* dosya.

Aşağıdaki örnek, tüm çıktı başlangıç toplu iş dosyasından yönlendirmek gösterilmektedir. Bu örnekte, ServerDefinition.csdef dosya çağıran bir başlangıç görevi oluşturur. *logwrap.cmd*. *logwrap.cmd* çağrıları *Startup2.cmd*, tüm çıktı yeniden yönlendirme **% TEMP %\\StartupLog.txt**.

ServiceDefinition.cmd:

```xml
<Startup>
    <Task commandLine="logwrap.cmd startup2.cmd" executionContext="limited" taskType="simple" />
</Startup>
```

**logwrap.cmd:**

```cmd
@ECHO OFF

REM   logwrap.cmd calls passed in batch file, redirecting all output to the StartupLog.txt log file.

ECHO [%date% %time%] == START logwrap.cmd ============================================== >> "%TEMP%\StartupLog.txt" 2>&1
ECHO [%date% %time%] Running %1 >> "%TEMP%\StartupLog.txt" 2>&1

REM   Call the child command batch file, redirecting all output to the StartupLog.txt log file.
START /B /WAIT %1 >> "%TEMP%\StartupLog.txt" 2>&1

REM   Log the completion of child command.
ECHO [%date% %time%] Done >> "%TEMP%\StartupLog.txt" 2>&1

IF %ERRORLEVEL% EQU 0 (

   REM   No errors occurred. Exit logwrap.cmd normally.
   ECHO [%date% %time%] == END logwrap.cmd ================================================ >> "%TEMP%\StartupLog.txt" 2>&1
   ECHO.  >> "%TEMP%\StartupLog.txt" 2>&1
   EXIT /B 0

) ELSE (

   REM   Log the error.
   ECHO [%date% %time%] An error occurred. The ERRORLEVEL = %ERRORLEVEL%.  >> "%TEMP%\StartupLog.txt" 2>&1
   ECHO [%date% %time%] == END logwrap.cmd ================================================ >> "%TEMP%\StartupLog.txt" 2>&1
   ECHO.  >> "%TEMP%\StartupLog.txt" 2>&1
   EXIT /B %ERRORLEVEL%

)
```

**Startup2.cmd:**

```cmd
@ECHO OFF

REM   This is the batch file where the startup steps should be performed. Because of the
REM   way Startup2.cmd was called, all commands and their outputs will be stored in the
REM   StartupLog.txt file in the directory pointed to by the TEMP environment variable.

REM   If an error occurs, the following command will pass the ERRORLEVEL back to the
REM   calling batch file.

ECHO [%date% %time%] Some log information about this task
ECHO [%date% %time%] Some more log information about this task

EXIT %ERRORLEVEL%
```

Örnek çıktıda **StartupLog.txt** dosyası:

```txt
[Mon 10/17/2016 20:24:46.75] == START logwrap.cmd ============================================== 
[Mon 10/17/2016 20:24:46.75] Running command1.cmd 
[Mon 10/17/2016 20:24:46.77] Some log information about this task
[Mon 10/17/2016 20:24:46.77] Some more log information about this task
[Mon 10/17/2016 20:24:46.77] Done 
[Mon 10/17/2016 20:24:46.77] == END logwrap.cmd ================================================ 
```

> [!TIP]
> **StartupLog.txt** dosyası *C:\Resources\temp\\{role tanımlayıcısı} \RoleTemp* klasör.
> 
> 

### <a name="set-executioncontext-appropriately-for-startup-tasks"></a>Başlangıç görevleri için uygun şekilde executionContext ayarlayın
Başlangıç görevi ayrıcalıkları uygun şekilde ayarlayın. Bazı durumlarda normal ayrıcalıklara sahip rolünü çalıştıran olsa bile başlangıç görevleri yükseltilmiş ayrıcalıklarla çalıştırmanız gerekir.

[ExecutionContext][görev] özniteliği başlangıç görevinin ayrıcalık düzeyini ayarlar. Kullanarak `executionContext="limited"` başlangıç görevinin rolle aynı ayrıcalık düzeyi sahip olduğu anlamına gelir. Kullanarak `executionContext="elevated"` başlangıç görevinin rolünüz için yönetici ayrıcalıkları vermeden yönetici görevleri gerçekleştirmek başlangıç görevi sağlayan yönetici ayrıcalıklarına sahip olduğu anlamına gelir.

Yükseltilmiş ayrıcalıklar gerektiren bir başlangıç görevi, kullanan bir başlangıç görevi örneğidir **AppCmd.exe** IIS'yi yapılandırmak için. **AppCmd.exe** gerektirir `executionContext="elevated"`.

### <a name="use-the-appropriate-tasktype"></a>Uygun taskType kullanın
[TaskType][görev] özniteliği belirler başlangıç görevinin şekilde yürütülür. Üç değer vardır: **basit**, **arka plan**, ve **ön plan**. Arka plan ve ön plan görevler zaman uyumsuz olarak başlatılır ve ardından basit görevler eşzamanlı olarak yürütülen bir kerede.

İle **basit** başlangıç görevleri, görevler ServiceDefinition.csdef dosyasında listelenen sırayla sıralama görevleri çalıştırarak ayarlayabilirsiniz. Varsa bir **basit** görev sıfır olmayan çıkış kodu ile sona erer ve başlangıç yordamı durur ve rol başlatılmaz.

Arasındaki fark **arka plan** başlangıç görevleri ve **ön plan** başlangıç görevleri olan **ön plan** görevleri tutmak kadar çalışan rolü  **ön plan** görev sona erer. Bu olması durumunda da anlamına gelir **ön plan** görev yanıt vermemeye başlıyor veya kilitlenmesi değil geri dönüşüm rolü kadar **ön plan** görev zorla kapatıldı. Bu nedenle, **arka plan** görevleri bu özellik, gerekli olmadıkça, zaman uyumsuz başlangıç görevleri için önerilen **ön plan** görev.

### <a name="end-batch-files-with-exit-b-0"></a>Çıkış /B 0 ile son toplu iş dosyaları
Rol varsa yalnızca başlar **errorlevel** her basit startup'ınız sıfır görevdir. Tüm programları Ayarla **errorlevel** (çıkış kodu) doğru şekilde toplu iş dosyasını bitmelidir bir `EXIT /B 0` her şeyin doğru şekilde çalıştırdıysanız.

Eksik `EXIT /B 0` başlangıç batch sonunda yaygın bir nedeni, başlatma rolleri dosyasıdır.

> [!NOTE]
> İç içe geçmiş toplu fark ettim dosyaları bazen askıda kullanırken `/B` parametresi. Başka bir toplu iş dosyası, geçerli toplu işlem dosyanız çağırırsa, ister kullanırsanız bu yanıt vermemesine sorun gerçekleşmez emin olmak isteyebilirsiniz [günlük sarmalayıcı](#always-log-startup-activities). Atlayabilirsiniz `/B` parametresi bu durumda.
> 
> 

### <a name="expect-startup-tasks-to-run-more-than-once"></a>Birden çok kez çalıştırmak için başlangıç görevleri beklediğiniz
Tüm rol geri dönüşümlerine genel bir yeniden başlatma içerir, ancak tüm başlangıç görevleri çalıştıran tüm rol geri dönüşümlerine genel içerir. Bu, başlangıç görevleri herhangi bir sorun olmadan yeniden başlatmalar arasında birden çok kez çalıştırmak mümkün olması gerektiğini anlamına gelir. Bu içinde ele alınmıştır [önceki bölümde yer](#detect-that-your-task-has-already-run).

### <a name="use-local-storage-to-store-files-that-must-be-accessed-in-the-role"></a>Roldeki erişilmelidir dosyaları depolamak için yerel depolama kullanın
Kopyalayın veya rolünüz için erişilebilir ise başlangıç göreviniz sırasında bir dosya oluşturmak istiyorsanız, bu dosyayı yerel depolama alanına yerleştirilmelidir. Bkz: [önceki bölümde yer](#create-files-in-local-storage-from-a-startup-task).

## <a name="next-steps"></a>Sonraki adımlar
Bulut gözden [hizmet modeli ve paketi](cloud-services-model-and-package.md)

Hakkında daha fazla bilgi [görevleri](cloud-services-startup-tasks.md) çalışır.

[Oluşturma ve dağıtma](cloud-services-how-to-create-deploy-portal.md) , bulut hizmeti paketi.

[ServiceDefinition.csdef]: cloud-services-model-and-package.md#csdef
[Görev]: https://msdn.microsoft.com/library/azure/gg557552.aspx#Task
[Startup]: https://msdn.microsoft.com/library/azure/gg557552.aspx#Startup
[Runtime]: https://msdn.microsoft.com/library/azure/gg557552.aspx#Runtime
[Ortam]: https://msdn.microsoft.com/library/azure/gg557552.aspx#Environment
[Değişkeni]: https://msdn.microsoft.com/library/azure/gg557552.aspx#Variable
[RoleInstanceValue]: https://msdn.microsoft.com/library/azure/gg557552.aspx#RoleInstanceValue
[RoleEnvironment]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.aspx
[Uç noktaları]: https://msdn.microsoft.com/library/azure/gg557552.aspx#Endpoints
[LocalStorage]: https://msdn.microsoft.com/library/azure/gg557552.aspx#LocalStorage
[LocalResources]: https://msdn.microsoft.com/library/azure/gg557552.aspx#LocalResources
[RoleInstanceValue]: https://msdn.microsoft.com/library/azure/gg557552.aspx#RoleInstanceValue
