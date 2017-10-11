---
title: "Bulut Hizmetleri için genel başlangıç görevleri | Microsoft Docs"
description: "Bulut Hizmetleri web rolü ya da çalışan rolü gerçekleştirmek istediğinizi düşünelim ortak başlangıç görevleri, bazı örnekler sağlar."
services: cloud-services
documentationcenter: 
author: Thraka
manager: timlt
editor: 
ms.assetid: a7095dad-1ee7-4141-bc6a-ef19a6e570f1
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/18/2017
ms.author: adegeo
ms.openlocfilehash: cee23da5b089b02bfc0ef10afd60f0f2272585b1
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/03/2017
---
# <a name="common-cloud-service-startup-tasks"></a>Genel bulut hizmeti başlangıç görevleri
Bu makale, bulut hizmetinizin gerçekleştirmek istediğinizi düşünelim ortak başlangıç görevleri, bazı örnekler sağlar. Başlangıç görevi, bir rol başlamadan önce işlemlerini gerçekleştirmek için kullanabilirsiniz. Bir bileşeni yüklemek, COM bileşenlerini kaydetme, kayıt defteri anahtarlarını ayarlamak veya uzun süre çalışan bir işlem başlatılıyor gerçekleştirmek isteyebileceğiniz işlemleri içerir. 

Bkz: [bu makalede](cloud-services-startup-tasks.md) nasıl iş başlangıç görevleri ve bir başlangıç görevi tanımlayan girişinin özellikle nasıl oluşturulacağını öğrenmek için.

> [!NOTE]
> Başlangıç görevi sanal makineler için yalnızca bulut hizmeti Web ve çalışan rolleri için geçerli değildir.
> 

## <a name="define-environment-variables-before-a-role-starts"></a>Bir rolü başlatılmadan önce ortamı değişkenleri tanımlayın
Belirli bir görev için tanımlanan ortam değişkenlerini gerekiyorsa kullanın [ortam] öğesi içinde [görev] öğesi.

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

Değişkenleri de kullanabilir bir [geçerli Azure XPath değeri](cloud-services-role-config-xpath.md) dağıtımı hakkında bir şey başvurmak için. Yerine `value` özniteliği, tanımlayan bir [RoleInstanceValue] alt öğesi.

```xml
<Variable name="PathToStartupStorage">
    <RoleInstanceValue xpath="/RoleEnvironment/CurrentInstance/LocalResources/LocalResource[@name='StartupLocalStorage']/@path" />
</Variable>
```


## <a name="configure-iis-startup-with-appcmdexe"></a>IIS Başlangıç AppCmd.exe ile yapılandırma
[AppCmd.exe](https://technet.microsoft.com/library/jj635852.aspx) komut satırı aracı, Azure üzerinde başlangıçta IIS ayarlarını yönetmek için kullanılabilir. *AppCmd.exe* yapılandırma ayarlarına başlangıç görevleri Azure ile ilgili kullanım için kullanışlı, komut satırı erişim sağlar. Kullanarak *AppCmd.exe*, Web sitesi ayarlarını eklenemez, değiştiren veya uygulamaları ve sitelerin kaldırılamaz.

Ancak, kullanımda dikkat edilmesi gereken birkaç şey vardır *AppCmd.exe* bir başlangıç görevi olarak:

* Başlangıç görevi birden çok kez yeniden başlatmalar arasında çalıştırabilirsiniz. Örneğin, ne zaman bir rol geri dönüştürür.
* Varsa bir *AppCmd.exe* eylem birden çok kez gerçekleştirilir, hataya neden. Örneğin, bir bölüme eklenmeye çalışılıyor *Web.config* iki kez hataya neden.
* Başlangıç görevleri, sıfır olmayan çıkış kodu döndürmesi durumunda başarısız veya **errorlevel**. Örneğin, *AppCmd.exe* bir hata oluşturur.

Denetlemek için iyi bir uygulamadır **errorlevel** çağırdıktan sonra *AppCmd.exe*, çağrısı kaydırma yapın kolay olduğu *AppCmd.exe* ile bir *.cmd* dosya. Bilinen bir algılama **errorlevel** yanıtı yok sayın veya için geri geçirin.

Tarafından döndürülen errorlevel *AppCmd.exe* Winerror.h'de dosyasında listelenir ve ayrıca görülebilir [MSDN](https://msdn.microsoft.com/library/windows/desktop/ms681382.aspx).

### <a name="example-of-managing-the-error-level"></a>Örnek hata düzeyi yönetme
Bu örnek için JSON için sıkıştırma bölüm ve bir sıkıştırma girişi ekler *Web.config* dosyasıyla hata işleme ve günlüğe kaydetme.

İlgili bölümleri [ServiceDefinition.csdef] dosya burada gösterilen, ayar içeren [executionContext](https://msdn.microsoft.com/library/azure/gg557552.aspx#Task) özniteliğini `elevated` vermek için *AppCmd.exe* ayarları değiştirmek için yeterli izinlere *Web.config* dosyası:

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

*Startup.cmd* toplu iş dosyası kullanır *AppCmd.exe* için JSON için bir sıkıştırma bölümü ve sıkıştırma giriş eklemek için *Web.config* dosya. Beklenen **errorlevel** 183 sıfır doğrula kullanmaya ayarlanır. EXE komut satırı programı. Beklenmeyen errorlevels StartupErrorLog.txt için günlüğe kaydedilir.

```cmd
REM   *** Add a compression section to the Web.config file. ***
%windir%\system32\inetsrv\appcmd set config /section:urlCompression /doDynamicCompression:True /commit:apphost >> "%TEMP%\StartupLog.txt" 2>&1

REM   ERRORLEVEL 183 occurs when trying to add a section that already exists. This error is expected if this
REM   batch file were executed twice. This can occur and must be accounted for in a Azure startup
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
Azure üzerinde etkili bir şekilde iki güvenlik duvarı vardır. Güvenlik Duvarı'nı ilk sanal makine ve dış dünya arasındaki bağlantıları denetler. Bu güvenlik duvarı tarafından denetlenen [uç noktaları] öğesinde [ServiceDefinition.csdef] dosya.

Güvenlik Duvarı'nı ikinci sanal makine ve sanal makinenin süreçlerinde arasındaki bağlantıları denetler. Bu güvenlik duvarı tarafından denetlenen `netsh advfirewall firewall` komut satırı aracı.

Azure güvenlik duvarı kuralları rollerinizi içinde başlatılan işlemleri oluşturur. Örneğin, bir hizmet veya programın başlattığınızda, Azure Internet ile iletişim kurmak bu hizmetin izin vermek için gerekli güvenlik duvarı kuralları otomatik olarak oluşturur. Ancak, rolünüze (örneğin, bir COM + hizmet ya da Windows zamanlanmış bir görev) dışında bir işlem tarafından başlatılan bir hizmet oluşturursanız, bu hizmete erişmesine izin vermek için bir güvenlik duvarı kuralı el ile oluşturmanız gerekir. Bu güvenlik duvarı kuralları, bir başlangıç görevi kullanılarak oluşturulabilir.

Bir güvenlik duvarı kuralı oluşturan bir başlangıç görevi olmalıdır bir [executionContext][görev] , **yükseltilmiş**. Aşağıdaki başlangıç görevi eklemek [ServiceDefinition.csdef] dosya.

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

Güvenlik duvarı kuralı eklemek için uygun kullanmalısınız `netsh advfirewall firewall` başlangıç toplu iş dosyası komutları. Bu örnekte, başlangıç görevi güvenlik ve şifreleme'yi TCP bağlantı noktası 80 gerektirir.

```cmd
REM   Add a firewall rule in a startup task.

REM   Add an inbound rule requiring security and encryption for TCP port 80 traffic.
netsh advfirewall firewall add rule name="Require Encryption for Inbound TCP/80" protocol=TCP dir=in localport=80 security=authdynenc action=allow >> "%TEMP%\StartupLog.txt" 2>&1

REM   If an error occurred, return the errorlevel.
EXIT /B %errorlevel%
```

## <a name="block-a-specific-ip-address"></a>Belirli bir IP adres bloğu
IIS değiştirerek belirtilen IP adresleri kümesi için bir Azure web rolü erişimi kısıtlayabilirsiniz **web.config** dosya. Ayrıca kilidini açan bir komut dosyası kullanmanıza gerek **IPSecurity** bölümünü **ApplicationHost.config** dosya.

Kilidini açmak için **IPSecurity** bölümünü **ApplicationHost.config** dosya, rol başlangıcında çalışan bir komut dosyası oluşturun. Adlı web rolünüz kök düzeyinde bir klasör oluşturun **başlangıç** ve bu klasör içinde adlı bir toplu iş dosyası oluşturmak **startup.cmd**. Bu dosyayı Visual Studio projenize ekleyin ve özelliklerini ayarlamak **her zaman Kopyala** paketinize bulunduğundan emin olmak için.

Aşağıdaki başlangıç görevi eklemek [ServiceDefinition.csdef] dosya.

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

Bu komut eklemek **startup.cmd** dosyası:

```cmd
@echo off
@echo Installing "IPv4 Address and Domain Restrictions" feature 
powershell -ExecutionPolicy Unrestricted -command "Install-WindowsFeature Web-IP-Security"
@echo Unlocking configuration for "IPv4 Address and Domain Restrictions" feature 
%windir%\system32\inetsrv\AppCmd.exe unlock config -section:system.webServer/security/ipSecurity
```

Bu görev neden **startup.cmd** gerekli sağlanarak web rolü başlatılmadan her zaman çalıştırılmak üzere toplu iş dosyası **IPSecurity** bölümdür kilidi.

Son olarak, değişiklik [system.webServer bölümü](http://www.iis.net/configreference/system.webserver/security/ipsecurity#005) web rolün **web.config** dosya erişim izni IP adresleri listesi aşağıdaki örnekte gösterildiği gibi ekleyin:

Bu örnek yapılandırma **sağlayan** tanımlanan iki dışında sunucuya erişmek için tüm IP'ler

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

Bu örnek yapılandırma **engellediği** sunucu tanımlanan iki dışında erişimini tüm IP'ler.

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

## <a name="create-a-powershell-startup-task"></a>PowerShell başlangıç görev oluşturma
Windows PowerShell komut dosyalarını, doğrudan çağrılamaz [ServiceDefinition.csdef] dosyası, ancak bunlar çağrılabilir gelen bir başlangıç toplu iş dosyası içinde.

PowerShell (varsayılan), imzalanmamış komut dosyalarının çalışmaz. Kodunuzu oturum sürece, imzalanmamış komut dosyalarını çalıştırmak üzere yapılandırmanız gerekir. İmzasız betikleri çalıştırmak için **ExecutionPolicy** ayarlanmalıdır **Kısıtlanmamış**. **ExecutionPolicy** ayarına kullanım üzerindeki Windows PowerShell sürümü temel alır.

```cmd
REM   Run an unsigned PowerShell script and log the output
PowerShell -ExecutionPolicy Unrestricted .\startup.ps1 >> "%TEMP%\StartupLog.txt" 2>&1

REM   If an error occurred, return the errorlevel.
EXIT /B %errorlevel%
```

Çalıştırır PowerShell 2.0 olan bir konuk işletim veya sürüm 2 çalıştırmak için zorlayabilirsiniz 1.0 kullanıyorsanız ve kullanılamaz durumdaysa, sürüm 1 kullanın.

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

## <a name="create-files-in-local-storage-from-a-startup-task"></a>Yerel depolama biriminden bir başlangıç görevi dosyaları oluşturma
Daha sonra uygulamanız tarafından erişilen, başlangıç görevi tarafından oluşturulan dosyaları depolamak için bir yerel depolama kaynağı kullanabilirsiniz.

Yerel depolama kaynağı oluşturmak için Ekle bir [LocalResources] için bölüm [ServiceDefinition.csdef] dosya ve ardından ekleyin [LocalStorage] alt öğesi. Yerel depolama kaynağı başlangıç görev için benzersiz bir ad ve uygun bir boyut verin.

Yerel depolama kaynağı başlangıç görevinde kullanmak için yerel depolama kaynak konumu başvurmak için bir ortam değişkeni oluşturmanız gerekir. Ardından başlangıç görev ve uygulama dosyaları okuma ve yerel depolama kaynağı yazma imkanınız olur.

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

Örnek olarak, bu **Startup.cmd** toplu iş dosyası kullanır **PathToStartupStorage** dosyası oluşturmak için ortam değişkeni **MyTest.txt** yerel depolama konumunda.

```cmd
REM   Create a simple text file.

ECHO This text will go into the MyTest.txt file which will be in the    >  "%PathToStartupStorage%\MyTest.txt"
ECHO path pointed to by the PathToStartupStorage environment variable.  >> "%PathToStartupStorage%\MyTest.txt"
ECHO The contents of the PathToStartupStorage environment variable is   >> "%PathToStartupStorage%\MyTest.txt"
ECHO "%PathToStartupStorage%".                                          >> "%PathToStartupStorage%\MyTest.txt"

REM   Exit the batch file with ERRORLEVEL 0.

EXIT /b 0
```

Azure SDK kullanarak yerel depolama klasörüne erişebilecek [GetLocalResource](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.getlocalresource.aspx) yöntemi.

```csharp
string localStoragePath = Microsoft.WindowsAzure.ServiceRuntime.RoleEnvironment.GetLocalResource("StartupLocalStorage").RootPath;

string fileContent = System.IO.File.ReadAllText(System.IO.Path.Combine(localStoragePath, "MyTestFile.txt"));
```

## <a name="run-in-the-emulator-or-cloud"></a>Öykünücü veya Bulut çalıştırın
İşlem öykünücüsü olduğunda karşılaştırılan bulutta çalışırken farklı adımları gerçekleştirmeniz, başlangıç görevi olabilir. Örneğin, yalnızca öykünücüsünde çalışırken SQL verilerinizi yeni bir kopyasını kullanmak isteyebilirsiniz. Veya öykünücüsünde çalıştırırken yapmanıza gerek yoktur bulut için bazı performans iyileştirmelerini yapmak isteyebilirsiniz.

İşlem öykünücüsü ve bulut üzerinde farklı eylemler gerçekleştirmek için bu özelliği bir ortam değişkeni oluşturarak gerçekleştirilebilir [ServiceDefinition.csdef] dosya. Ardından, başlangıç görevi bu ortam değişkenine bir değer için sınayın.

Ortam değişkeni oluşturmak için Ekle [değişkeni]/[RoleInstanceValue] öğesi ve bir XPath değeri oluşturun `/RoleEnvironment/Deployment/@emulated`. Değeri **ComputeEmulatorRunning %** ortam değişkenidir `true` işlem öykünücüsü üzerinde çalışırken ve `false` bulut üzerinde çalışırken.

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

Görev şimdi denetleyebilirsiniz **ComputeEmulatorRunning %** farklı eylemler gerçekleştirmek için ortam değişkeni tabanlı rol Bulut veya öykünücü çalıştıran. Bu ortam değişkeninin denetler .cmd Kabuk betiği'dir.

```cmd
REM   Check if this task is running on the compute emulator.

IF "%ComputeEmulatorRunning%" == "true" (
    REM   This task is running on the compute emulator. Perform tasks that must be run only in the compute emulator.
) ELSE (
    REM   This task is running on the cloud. Perform tasks that must be run only in the cloud.
)
```


## <a name="detect-that-your-task-has-already-run"></a>Görevinizi zaten çalıştırılıp çalıştırılmadığını Algıla
Rolü yeniden çalıştırmak, başlangıç görevleri neden başlatmadan geri dönüşüm. Bir görev, barındırma VM üzerinde zaten çalıştırılıp çalıştırılmadığını gösteren bayrak yoktur. Birden çok kez çalıştırmak olduğu önemli değildir, bazı görevler de sahip olabilir. Ancak, burada bir görevin birden çok kez çalışmasını engellemek için gereken bir durumla içine çalışabilir.

Bir görev zaten çalıştırılıp çalıştırılmadığını belirlemek için basit bir dosyada oluşturmak için yoldur **% TEMP %** görev başarılı olduğunda klasörü ve başlangıç görevinin de bakın. Sizin için yapar bir örnek cmd Kabuk betiği'dir.

```cmd
REM   If Task1_Success.txt exists, then Application 1 is already installed.
IF EXIST "%RoleRoot%\Task1_Success.txt" (
  ECHO Application 1 is already installed. Exiting. >> "%TEMP%\StartupLog.txt" 2>&1
  GOTO Finish
)

REM   Run your real exe task
ECHO Running XYZ >> "%TEMP%\StartupLog.txt" 2>&1
"%PathToApp1Install%\setup.exe" >> "%TEMP%\StartupLog.txt" 2>&1

IF %ERRORLEVEL% EQU 0 (
  REM   The application installed without error. Create a file to indicate that the task
  REM   does not need to be run again.

  ECHO This line will create a file to indicate that Application 1 installed correctly. > "%RoleRoot%\Task1_Success.txt"

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
Görev web veya çalışan rolü için yapılandırırken izlemeniz gereken bazı en iyi uygulamalar şunlardır.

### <a name="always-log-startup-activities"></a>Her zaman günlük başlangıç etkinlikleri
Visual Studio için toplu iş dosyaları işlemi mümkün olduğunca kadar veri almak üzere iyi olacak şekilde toplu iş dosyaları, adım için bir hata ayıklayıcısı sağlamaz. Toplu iş dosyaları çıktısı günlüğü her ikisi de **stdout** ve **stderr**, hata ayıklama ve toplu iş dosyaları düzeltme çalışılırken önemli bilgileri verebilirsiniz. Hem günlük için **stdout** ve **stderr** StartupLog.txt dizinindeki dosyasına işaret için tarafından **% TEMP %** ortam değişkeni, bir metin eklemek `>>  "%TEMP%\\StartupLog.txt" 2>&1` belirli satırları oturum istediğiniz sonuna. Örneğin, setup.exe yürütmek için **PathToApp1Install %** dizini:

    "%PathToApp1Install%\setup.exe" >> "%TEMP%\StartupLog.txt" 2>&1

Xml dosyanızı basitleştirmek için bir sarmalayıcı oluşturabilirsiniz *cmd* tüm, başlangıç çağrılar dosya görevlerin yanı sıra günlüğe kaydetme ve her alt görev aynı ortam değişkenlerini paylaşır sağlar.

Bu rahatsız edici kullanmak olsa bulabilirsiniz `>> "%TEMP%\StartupLog.txt" 2>&1` her başlangıç görevi ucunda. Görev günlüğü, günlük kaydı işleyen bir sarmalayıcı oluşturarak uygulayabilir. Bu sarmalayıcı çalıştırmak istediğiniz gerçek toplu iş dosyasını çağırır. Hedef toplu iş dosyasından herhangi bir çıktı yönlendirilecek *Startuplog.txt* dosya.

Aşağıdaki örnek, başlangıç toplu iş dosyasından tüm çıktı yeniden yönlendirme gösterilmektedir. Bu örnekte ServerDefinition.csdef dosyasını çağıran bir başlangıç görevi oluşturur *logwrap.cmd*. *logwrap.cmd* çağrıları *Startup2.cmd*, tüm çıktı yeniden yönlendirme **% TEMP %\\StartupLog.txt**.

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

Örnek çıkış **StartupLog.txt** dosyası:

```txt
[Mon 10/17/2016 20:24:46.75] == START logwrap.cmd ============================================== 
[Mon 10/17/2016 20:24:46.75] Running command1.cmd 
[Mon 10/17/2016 20:24:46.77] Some log information about this task
[Mon 10/17/2016 20:24:46.77] Some more log information about this task
[Mon 10/17/2016 20:24:46.77] Done 
[Mon 10/17/2016 20:24:46.77] == END logwrap.cmd ================================================ 
```

> [!TIP]
> **StartupLog.txt** dosyasının bulunduğu *C:\Resources\temp\\{rol tanımlayıcısı} \RoleTemp* klasör.
> 
> 

### <a name="set-executioncontext-appropriately-for-startup-tasks"></a>Başlangıç görevi için uygun şekilde executionContext ayarlayın
Başlangıç görevi için uygun şekilde ayrıcalıkları ayarlayın. Bazen normal ayrıcalıklara sahip rolünü çalıştıran olsa bile başlangıç görevleri yükseltilmiş ayrıcalıklarla çalıştırmanız gerekir.

[ExecutionContext][görev] öznitelik başlangıç görevi öncelik düzeyi ayarlar. Kullanarak `executionContext="limited"` başlangıç görevi rolle aynı ayrıcalık düzeyi olduğu anlamına gelir. Kullanarak `executionContext="elevated"` başlangıç görevi rolünüz için yönetici ayrıcalıkları vermeden yönetici görevlerini gerçekleştirmek başlangıç görevi sağlayan yönetici ayrıcalıklarına sahip anlamına gelir.

Yükseltilmiş ayrıcalıklar gerektiren bir başlangıç görevi kullanan bir başlangıç görevi örneğidir **AppCmd.exe** IIS'yi yapılandırmak için. **AppCmd.exe** gerektirir `executionContext="elevated"`.

### <a name="use-the-appropriate-tasktype"></a>Uygun taskType kullanın
[TaskType][görev] özniteliği belirler başlangıç görevi şekilde gerçekleştirilir. Üç değer vardır: **basit**, **arka plan**, ve **ön plan**. Arka plan ve ön plan görevleri zaman uyumsuz olarak başlatılır ve ardından basit görevler eşzamanlı olarak yürütülen bir kerede.

İle **basit** başlangıç görevleri, görevlerin çalıştığı sırada görevleri ServiceDefinition.csdef dosyasında listelenen sıraya göre ayarlayabilirsiniz. Varsa bir **basit** görev sıfır olmayan çıkış kodu ile sona erer ve ardından başlangıç yordamı durur ve rol başlatılmaz.

Arasındaki farkı **arka plan** başlangıç görevleri ve **ön plan** başlangıç görevleri olan **ön plan** görevleri tutmak kadar çalışan rolü **ön plan** görev sona erer. Bu aynı zamanda olması durumunda gelir **ön plan** görevin askıda kalmasına ya da çökme (Crash), rol değil geri dönüşüm kadar **ön plan** görev zorunlu kapalı. Bu nedenle, **arka plan** görevleri bu özelliği gerekmedikçe zaman uyumsuz başlangıç görevleri için önerilen **ön plan** görev.

### <a name="end-batch-files-with-exit-b-0"></a>Çık /B 0 ile son toplu iş dosyaları
Rol, yalnızca başlatacak **errorlevel** her basit başlangıç sıfır görevdir. Tüm programları Ayarla **errorlevel** (çıkış kodu), doğru şekilde toplu dosya bitmelidir bir `EXIT /B 0` her şeyin doğru şekilde çalıştırdıysanız.

Eksik `EXIT /B 0` başlatılmaz rolleri ortak bir nedeni bir başlangıç toplu işlemin sonunda dosyasıdır.

> [!NOTE]
> İç içe geçmiş toplu fark ettim dosyalar bazen askıda kullanırken `/B` parametresi. Başka bir toplu iş dosyası, geçerli toplu iş dosyasını çağırıyorsa ister kullanırsanız, bu yanıt vermemesine sorun gerçekleşmez emin olmak isteyebilirsiniz [günlük sarmalayıcı](#always-log-startup-activities). Atlayabilirsiniz `/B` parametresi bu durumda.
> 
> 

### <a name="expect-startup-tasks-to-run-more-than-once"></a>Birden çok kez çalıştırmak için başlangıç görevleri beklediğiniz
Yeniden başlatma tüm rol dönüştürür içerir, ancak çalışan tüm başlangıç görevleri tüm rol dönüştürür içerir. Bu, başlangıç görevleri herhangi bir sorun olmadan yeniden başlatmalar arasında birden çok kez çalıştırmanız mümkün olması gerektiği anlamına gelir. Bu konu ele alınmıştır [bölüm önceki](#detect-that-your-task-has-already-run).

### <a name="use-local-storage-to-store-files-that-must-be-accessed-in-the-role"></a>Roldeki erişilmelidir dosyalarını depolamak için yerel depolama kullanın
Kopyalama veya rolünüz için erişilebilir olduğundan, başlangıç görev sırasında bir dosya oluşturmak istiyorsanız, bu dosya yerel depolama alanına yerleştirilmelidir. Bkz: [bölüm önceki](#create-files-in-local-storage-from-a-startup-task).

## <a name="next-steps"></a>Sonraki adımlar
Bulut gözden [hizmet modeli ve paket](cloud-services-model-and-package.md)

Hakkında daha fazla bilgi [görevleri](cloud-services-startup-tasks.md) çalışır.

[Oluşturma ve dağıtma](cloud-services-how-to-create-deploy-portal.md) , bulut hizmet paketi.

[ServiceDefinition.csdef]: cloud-services-model-and-package.md#csdef
[görev]: https://msdn.microsoft.com/library/azure/gg557552.aspx#Task
[Startup]: https://msdn.microsoft.com/library/azure/gg557552.aspx#Startup
[Runtime]: https://msdn.microsoft.com/library/azure/gg557552.aspx#Runtime
[ortam]: https://msdn.microsoft.com/library/azure/gg557552.aspx#Environment
[değişkeni]: https://msdn.microsoft.com/library/azure/gg557552.aspx#Variable
[RoleInstanceValue]: https://msdn.microsoft.com/library/azure/gg557552.aspx#RoleInstanceValue
[RoleEnvironment]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.aspx
[Uç noktaları]: https://msdn.microsoft.com/library/azure/gg557552.aspx#Endpoints
[LocalStorage]: https://msdn.microsoft.com/library/azure/gg557552.aspx#LocalStorage
[LocalResources]: https://msdn.microsoft.com/library/azure/gg557552.aspx#LocalResources
[RoleInstanceValue]: https://msdn.microsoft.com/library/azure/gg557552.aspx#RoleInstanceValue
