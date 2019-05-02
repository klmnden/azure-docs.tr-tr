---
title: Azure Cloud Services rollerinde .NET yükleme | Microsoft Docs
description: Bu makalede .NET Framework, bulut hizmeti web ve çalışan rollerinde el ile yükleme
services: cloud-services
documentationcenter: .net
author: jpconnock
manager: timlt
editor: ''
ms.assetid: 8d1243dc-879c-4d1f-9ed0-eecd1f6a6653
ms.service: cloud-services
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/22/2018
ms.author: jeconnoc
ms.openlocfilehash: bc861b6730e8bf9db6ba2ab005496914f7b9ed89
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64699673"
---
# <a name="install-net-on-azure-cloud-services-roles"></a>Azure Cloud Services rollerinde .NET yükleme
Bu makalede, Azure konuk işletim sistemi ile birlikte gelmeyen .NET Framework sürümlerini yüklemek açıklar. Konuk işletim sisteminde .NET bulut hizmeti web ve çalışan rolleri yapılandırmak için kullanabilirsiniz.

Örneğin, herhangi bir .NET 4.6 sürümünü ile gelmez konuk işletim sistemi ailesi 4, .NET 4.6.2 yükleyebilirsiniz. (Konuk işletim sistemi ailesi 5, .NET 4.6 ile geliyor.) En son Azure konuk işletim sistemi sürümleri hakkında bilgi için [Azure konuk işletim sistemi sürüm haberleri](cloud-services-guestos-update-matrix.md). 

>[!IMPORTANT]
>Azure SDK 2.9 sürümünü, .NET 4.6 konuk işletim sistemi ailesi 4 veya önceki bir sürümü dağıtma konusunda bir kısıtlama içerir. Kısıtlama için bir düzeltme kullanılabilir [Microsoft Docs](https://github.com/MicrosoftDocs/azure-cloud-services-files/tree/master/Azure%20Targets%20SDK%202.9) site.

.NET üzerinde web ve çalışan rollerini yüklemek için bulut hizmeti projenizi bir parçası olarak .NET web yükleyicisini içerir. Yükleyici rolün başlangıç görevleri bir parçası olarak başlatın. 

## <a name="add-the-net-installer-to-your-project"></a>.NET yükleyici projenize ekleyin.
.NET Framework web yükleyicisini indirmek için yüklemek istediğiniz sürümü seçin:

* [.NET 4.8 web yükleyicisi](https://dotnet.microsoft.com/download/thank-you/net48)
* [.NET 4.7.2 web yükleyicisi](https://go.microsoft.com/fwlink/?LinkId=863262)
* [.NET 4.6.2 web yükleyicisi](https://www.microsoft.com/download/details.aspx?id=53345)

Yükleyicisi eklemek için bir *web* rolü:
  1. İçinde **Çözüm Gezgini**altında **rolleri** , bulut hizmeti projesini sağ tıklayın, *web* rolü seçip alt **Ekle**  >  **Yeni klasör**. Adlı bir klasör oluşturun **bin**.
  2. Bin klasörüne sağ tıklayıp **Ekle** > **var olan öğe**. .NET Yükleyicisi'ni seçin ve bin klasörüne erişmeye ekleyin.
  
Yükleyicisi eklemek için bir *çalışan* rolü:
* Sağ tıklayın, *çalışan* rolü seçip alt **Ekle** > **var olan öğe**. .NET Yükleyicisi'ni seçin ve role ekleyin. 

Bu şekilde rol içerik klasöre dosyalar eklendiğinde bunlar otomatik olarak bulut hizmeti paketinizi eklenir. Dosyaları, ardından sanal makinede tutarlı bir konuma dağıtılır. Böylece tüm rolleri bir kopyasını yükleyici, bulut hizmeti içindeki her web ve çalışan rolü için bu işlemi yineleyin.

> [!NOTE]
> Uygulamanız .NET 4.6 bile hedefliyorsa, .NET 4.6.2, bulut hizmeti rolünde yüklemeniz gerekir. Bilgi Bankası konuk işletim sistemi içerir [3098779 güncelleştirme](https://support.microsoft.com/kb/3098779) ve [3097997 güncelleştirme](https://support.microsoft.com/kb/3097997). .NET 4.6 üzerinde Bilgi Bankası güncelleştirmeler yüklediyseniz, .NET uygulamaları çalıştırdığınızda, sorunlar ortaya çıkabilir. Bu sorunlarla karşılaşmamak için .NET 4.6.2 sürümü 4.6 yazmak yerine yükleyin. Daha fazla bilgi için [Bilgi Bankası makalesi 3118750](https://support.microsoft.com/kb/3118750) ve [4340191](https://support.microsoft.com/kb/4340191).
> 
> 

![Rol içeriğiyle Installer dosyaları][1]

## <a name="define-startup-tasks-for-your-roles"></a>Başlangıç görevleri, rolleriniz için tanımlayın
Başlangıç görevleri rol başlamadan önce işlemleri gerçekleştirmek için kullanabilirsiniz. Başlangıç görevinin bir parçası olarak .NET Framework yükleme, herhangi bir uygulama kodu çalıştırmadan önce framework yüklü olduğunu sağlar. Başlangıç görevleri hakkında daha fazla bilgi için bkz. [Azure'da başlangıç görevleri çalıştırma](cloud-services-startup-tasks.md). 

1. ServiceDefinition.csdef dosyasının altında aşağıdaki içeriği ekleyin **WebRole** veya **WorkerRole** düğüm tüm roller için:
   
    ```xml
    <LocalResources>
      <LocalStorage name="NETFXInstall" sizeInMB="1024" cleanOnRoleRecycle="false" />
    </LocalResources>    
    <Startup>
      <Task commandLine="install.cmd" executionContext="elevated" taskType="simple">
        <Environment>
          <Variable name="PathToNETFXInstall">
            <RoleInstanceValue xpath="/RoleEnvironment/CurrentInstance/LocalResources/LocalResource[@name='NETFXInstall']/@path" />
          </Variable>
          <Variable name="ComputeEmulatorRunning">
            <RoleInstanceValue xpath="/RoleEnvironment/Deployment/@emulated" />
          </Variable>
        </Environment>
      </Task>
    </Startup>
    ```
   
    Önceki yapılandırma Konsolu komutu çalıştırır `install.cmd` .NET Framework'ü yüklemek için yönetici ayrıcalıklarına sahip. Yapılandırma ayrıca oluşturur bir **LocalStorage** adlı bir öğe **NETFXInstall**. Başlangıç betiği temp klasörü bu yerel depolama kaynağı kullanılacak ayarlar. 
    
    > [!IMPORTANT]
    > Doğru framework yüklenmesini sağlamak için bu kaynak boyutu en az 1024 MB olarak ayarlayın.
    
    Başlangıç görevleri hakkında daha fazla bilgi için bkz: [yaygın Azure Cloud Services başlangıç görevleri](cloud-services-startup-tasks-common.md).

2. Adlı bir dosya oluşturun **Install.cmd** ve aşağıdaki yükleme betik dosyasına ekleyin.

   Betik, belirtilen .NET Framework sürümünü makine üzerinde kayıt defterini sorgulayarak yüklü olup olmadığını denetler. .NET sürüm yüklü değilse, ardından .NET web yükleyici açılır. Herhangi bir sorunu gidermeye yardımcı olmak için depolanan dosya startuptasklog-(geçerli tarih ve saat) .txt için tüm etkinlik betik günlüklerini **InstallLogs** yerel depolama.
   
   > [!IMPORTANT]
   > Install.cmd dosyasını oluşturmak için Windows Not Defteri gibi bir temel metin düzenleyici kullanın. Bir metin dosyası oluşturun ve uzantı için .cmd değiştirmek için Visual Studio kullanıyorsanız, dosyayı yine de bir UTF-8 bayt sırası işareti içerebilir. Komut dosyası ilk satırını çalıştırdığınızda bu işaretin bir hataya neden olabilir. Bu hatayı önlemek için komut dosyasının ilk satırı tarafından bayt sipariş işleme atlandı bir REM deyimi olun. 
   > 
   >
   
   ```cmd
   REM Set the value of netfx to install appropriate .NET Framework. 
   REM ***** To install .NET 4.5.2 set the variable netfx to "NDP452" *****
   REM ***** To install .NET 4.6 set the variable netfx to "NDP46" *****
   REM ***** To install .NET 4.6.1 set the variable netfx to "NDP461" ***** https://go.microsoft.com/fwlink/?LinkId=671729
   REM ***** To install .NET 4.6.2 set the variable netfx to "NDP462" ***** https://www.microsoft.com/download/details.aspx?id=53345
   REM ***** To install .NET 4.7 set the variable netfx to "NDP47" ***** 
   REM ***** To install .NET 4.7.1 set the variable netfx to "NDP471" ***** https://go.microsoft.com/fwlink/?LinkId=852095
   REM ***** To install .NET 4.7.2 set the variable netfx to "NDP472" ***** https://go.microsoft.com/fwlink/?LinkId=863262
   set netfx="NDP472"
   REM ***** To install .NET 4.8 set the variable netfx to "NDP48" ***** https://dotnet.microsoft.com/download/thank-you/net48
      
   REM ***** Set script start timestamp *****
   set timehour=%time:~0,2%
   set timestamp=%date:~-4,4%%date:~-10,2%%date:~-7,2%-%timehour: =0%%time:~3,2%
   set "log=install.cmd started %timestamp%."
   
   REM ***** Exit script if running in Emulator *****
   if "%ComputeEmulatorRunning%"=="true" goto exit
   
   REM ***** Needed to correctly install .NET 4.6.1, otherwise you may see an out of disk space error *****
   set TMP=%PathToNETFXInstall%
   set TEMP=%PathToNETFXInstall%
   
   REM ***** Setup .NET filenames and registry keys *****
   if %netfx%=="NDP472" goto NDP472
   if %netfx%=="NDP471" goto NDP471
   if %netfx%=="NDP47" goto NDP47
   if %netfx%=="NDP462" goto NDP462
   if %netfx%=="NDP461" goto NDP461
   if %netfx%=="NDP46" goto NDP46
   
   set "netfxinstallfile=NDP452-KB2901954-Web.exe"
   set netfxregkey="0x5cbf5"
   goto logtimestamp
   
   :NDP46
   set "netfxinstallfile=NDP46-KB3045560-Web.exe"
   set netfxregkey="0x6004f"
   goto logtimestamp
   
   :NDP461
   set "netfxinstallfile=NDP461-KB3102438-Web.exe"
   set netfxregkey="0x6040e"
   goto logtimestamp
   
   :NDP462
   set "netfxinstallfile=NDP462-KB3151802-Web.exe"
   set netfxregkey="0x60632"
   goto logtimestamp
   
   :NDP47
   set "netfxinstallfile=NDP47-KB3186500-Web.exe"
   set netfxregkey="0x707FE"
   goto logtimestamp
   
   :NDP471
   set "netfxinstallfile=NDP471-KB4033344-Web.exe"
   set netfxregkey="0x709fc"
   goto logtimestamp
   
   :NDP472
   set "netfxinstallfile=NDP472-KB4054531-Web.exe"
   set netfxregkey="0x70BF6"
   goto logtimestamp
   
   :logtimestamp
   REM ***** Setup LogFile with timestamp *****
   md "%PathToNETFXInstall%\log"
   set startuptasklog="%PathToNETFXInstall%log\startuptasklog-%timestamp%.txt"
   set netfxinstallerlog="%PathToNETFXInstall%log\NetFXInstallerLog-%timestamp%"
   echo %log% >> %startuptasklog%
   echo Logfile generated at: %startuptasklog% >> %startuptasklog%
   echo TMP set to: %TMP% >> %startuptasklog%
   echo TEMP set to: %TEMP% >> %startuptasklog%
   
   REM ***** Check if .NET is installed *****
   echo Checking if .NET (%netfx%) is installed >> %startuptasklog%
   set /A netfxregkeydecimal=%netfxregkey%
   set foundkey=0
   FOR /F "usebackq skip=2 tokens=1,2*" %%A in (`reg query "HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\NET Framework Setup\NDP\v4\Full" /v Release 2^>nul`) do @set /A foundkey=%%C
   echo Minimum required key: %netfxregkeydecimal% -- found key: %foundkey% >> %startuptasklog%
   if %foundkey% GEQ %netfxregkeydecimal% goto installed
   
   REM ***** Installing .NET *****
   echo Installing .NET with commandline: start /wait %~dp0%netfxinstallfile% /q /serialdownload /log %netfxinstallerlog%  /chainingpackage "CloudService Startup Task" >> %startuptasklog%
   start /wait %~dp0%netfxinstallfile% /q /serialdownload /log %netfxinstallerlog% /chainingpackage "CloudService Startup Task" >> %startuptasklog% 2>>&1
   if %ERRORLEVEL%== 0 goto installed
       echo .NET installer exited with code %ERRORLEVEL% >> %startuptasklog%    
       if %ERRORLEVEL%== 3010 goto restart
       if %ERRORLEVEL%== 1641 goto restart
       echo .NET (%netfx%) install failed with Error Code %ERRORLEVEL%. Further logs can be found in %netfxinstallerlog% >> %startuptasklog%
       goto exit
       
   :restart
   echo Restarting to complete .NET (%netfx%) installation >> %startuptasklog%
   shutdown.exe /r /t 5 /c "Installed .NET framework" /f /d p:2:4
   
   :installed
   echo .NET (%netfx%) is installed >> %startuptasklog%
   
   :end
   echo install.cmd completed: %date:~-4,4%%date:~-10,2%%date:~-7,2%-%timehour: =0%%time:~3,2% >> %startuptasklog%
   
   :exit
   EXIT /B 0
   ```

3. Install.cmd dosyasını kullanarak her role ekleme **Ekle** > **var olan öğe** içinde **Çözüm Gezgini** bu konunun önceki kısımlarında açıklandığı gibi. 

    Bu adım tamamlandıktan sonra tüm roller Install.cmd dosyasını ve .NET yükleyici dosyası olmalıdır.

   ![Rol içeriğiyle tüm dosyalar][2]

## <a name="configure-diagnostics-to-transfer-startup-logs-to-blob-storage"></a>Tanılama günlükleri başlangıç Blob depolamaya aktarmak için Yapılandır
Yükleme sorunlarını giderme basitleştirmek için Azure başlangıç betiği veya Azure Blob Depolama için .NET yükleyici tarafından oluşturulan günlük dosyalarını aktarmak için tanılama yapılandırabilirsiniz. Bu yaklaşımı kullanarak Blob depolama alanından günlük dosyaları indirme yerine Uzak Masaüstü Bağlantısı rolü tarafından günlüklerini görüntüleyebilirsiniz.


Tanılama yapılandırmak için diagnostics.wadcfgx dosyasını açın ve aşağıdaki içeriği altında ekleyin **dizinleri** düğüm: 

```xml 
<DataSources>
 <DirectoryConfiguration containerName="netfx-install">
  <LocalResource name="NETFXInstall" relativePath="log"/>
 </DirectoryConfiguration>
</DataSources>
```

Tanılama Günlüğü alt dizinindeki dosyaları aktarmak için bu XML yapılandırır **NETFXInstall** tanılama depolama hesabı için kaynak **netfx yükleme** blob kapsayıcısı.

## <a name="deploy-your-cloud-service"></a>Bulut hizmetinize dağıtın
Bulut hizmetinizin dağıttığınızda, başlangıç görevleri zaten yüklü değilse .NET Framework yükleyin. Bulut hizmeti rollerinizi bulunan *meşgul* framework yüklenirken belirtin. Framework yüklemesi yeniden başlatma gerektirirse, hizmet rolleri de yeniden başlatılabilir. 

## <a name="additional-resources"></a>Ek kaynaklar
* [.NET Framework yükleme][Installing the .NET Framework]
* [Hangi .NET Framework sürümlerinin yüklü olduğunu belirleme][How to: Determine Which .NET Framework Versions Are Installed]
* [.NET Framework yüklemelerinin sorunlarını giderme][Troubleshooting .NET Framework Installations]

[How to: Determine Which .NET Framework Versions Are Installed]: /dotnet/framework/migration-guide/how-to-determine-which-versions-are-installed
[Installing the .NET Framework]: /dotnet/framework/install/guide-for-developers
[Troubleshooting .NET Framework Installations]: /dotnet/framework/install/troubleshoot-blocked-installations-and-uninstallations

<!--Image references-->
[1]: ./media/cloud-services-dotnet-install-dotnet/rolecontentwithinstallerfiles.png
[2]: ./media/cloud-services-dotnet-install-dotnet/rolecontentwithallfiles.png
