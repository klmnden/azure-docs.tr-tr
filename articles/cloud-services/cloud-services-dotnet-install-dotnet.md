---
title: "Azure Cloud Services rollerinde .NET yükleme | Microsoft Docs"
description: "Bu makalede, .NET Framework üzerinde bulut hizmeti web ve çalışan rolleri el ile yüklemek açıklar"
services: cloud-services
documentationcenter: .net
author: thraka
manager: timlt
editor: 
ms.assetid: 8d1243dc-879c-4d1f-9ed0-eecd1f6a6653
ms.service: cloud-services
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/31/2017
ms.author: adegeo
ms.openlocfilehash: cc4b62bc554757e6e394b78334f52f45aa08efe8
ms.sourcegitcommit: 43c3d0d61c008195a0177ec56bf0795dc103b8fa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/01/2017
---
# <a name="install-net-on-azure-cloud-services-roles"></a>Azure Cloud Services rollerinde .NET yükleme
Bu makalede, Azure konuk işletim Sistemleri ile gelir yok .NET Framework sürümleri yüklemeyi açıklar. Bulut hizmeti web ve çalışan rolleri yapılandırmak için konuk işletim sisteminde .NET kullanın.

Örneğin, herhangi bir .NET 4.6 sürümünden ile gelmez konuk işletim sistemi ailesi 4, .NET 4.6.1 yükleyebilirsiniz. (5 konuk işletim sistemi ailesi .NET 4.6 ile gelir.) En son Azure konuk işletim sistemi sürümleri hakkında bilgi için [Azure konuk işletim sistemi sürüm haber](cloud-services-guestos-update-matrix.md). 

>[!IMPORTANT]
>Azure SDK 2.9 .NET 4.6 konuk işletim sistemi ailesi 4 veya önceki bir sürümü dağıtma üzerine bir kısıtlama var. Kısıtlama için bir düzeltme edinilebilir [Microsoft Docs](https://github.com/MicrosoftDocs/azure-cloud-services-files/tree/master/Azure%20Targets%20SDK%202.9) site.

.NET web ve çalışan rollerinde yüklemek için bulut hizmeti projenizi bir parçası olarak .NET web yükleyicisi içerir. Yükleyici rolün başlangıç görevleri bir parçası olarak başlatın. 

## <a name="add-the-net-installer-to-your-project"></a>.NET yükleyici projenize ekleyin
.NET Framework için web yükleyici indirmek için yüklemek istediğiniz sürümü seçin:

* [.NET 4.7.1 web yükleyicisi](http://go.microsoft.com/fwlink/?LinkId=852095)
* [.NET 4.6.1 web yükleyicisi](http://go.microsoft.com/fwlink/?LinkId=671729)

Yükleyicisi eklemek için bir *web* rol:
  1. İçinde **Çözüm Gezgini**altında **rolleri** , bulut hizmet projenize sağ tıklayın, *web* rolü ve select **Ekle**  >  **Yeni klasör**. Adlı bir klasör oluşturun **bin**.
  2. Bin klasörü sağ tıklatın ve seçin **Ekle** > **varolan öğeyi**. .NET Yükleyicisi'ni seçin ve bin klasörüne ekleyin.
  
Yükleyicisi eklemek için bir *çalışan* rol:
* Sağ tıklayın, *çalışan* rolü ve select **Ekle** > **varolan öğeyi**. .NET Yükleyicisi'ni seçin ve role ekleyin. 

Bu şekilde rol içerik klasörüne dosyalar eklendiğinde bunlar otomatik olarak, bulut hizmet paketi eklenir. Dosyaları daha sonra sanal makinede tutarlı bir konumuna dağıtılır. Böylece tüm rolleri yükleyici kopyasını, bulut hizmeti içindeki her web ve çalışan rolü için bu işlemi yineleyin.

> [!NOTE]
> Uygulamanız .NET 4.6 hedefliyorsa bile, bulut hizmeti rolü'nde .NET 4.6.1 yüklemeniz gerekir. Bilgi Bankası konuk işletim sistemi içeren [3098779 güncelleştirme](https://support.microsoft.com/kb/3098779) ve [3097997 güncelleştirme](https://support.microsoft.com/kb/3097997). .NET 4.6 üstünde Bilgi Bankası güncelleştirmeler yüklediyseniz, .NET uygulamalarınızın çalıştırdığınızda sorunlar oluşabilir. Bu sorunları önlemek için sürümü 4.6 yerine .NET 4.6.1 yükleyin. Daha fazla bilgi için bkz: [Bilgi Bankası makalesi 3118750](https://support.microsoft.com/kb/3118750).
> 
> 

![Rol içeriğiyle Installer dosyaları][1]

## <a name="define-startup-tasks-for-your-roles"></a>Başlangıç görevleri, rolleri tanımlama
Başlangıç görevi, bir rol başlamadan önce işlemlerini gerçekleştirmek için kullanabilirsiniz. Başlangıç görevinin bir parçası .NET Framework yükleme herhangi bir uygulama kodu çalıştırmadan önce framework yüklendiğini sağlar. Başlangıç görevleri hakkında daha fazla bilgi için bkz: [Azure'da başlangıç görevleri çalıştırma](cloud-services-startup-tasks.md). 

1. ServiceDefinition.csdef dosyasında altına aşağıdaki içeriği ekleyin **WebRole** veya **örn** düğüm tüm rolleri için:
   
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
   
    Önceki yapılandırma Konsolu komutu çalıştırır `install.cmd` .NET Framework'ü yüklemek için yönetici ayrıcalıklarına sahip. Yapılandırma da oluşturur bir **LocalStorage** adlı öğe **NETFXInstall**. Başlangıç betiği bu yerel depolama kaynağı kullanmak için geçici klasörü ayarlar. 
    
    > [!IMPORTANT]
    > Framework'ün doğru yükleme sağlamak için bu kaynak boyutu en az 1024 MB olarak ayarlayın.
    
    Başlangıç görevleri hakkında daha fazla bilgi için bkz: [ortak Azure Cloud Services başlangıç görevleri](cloud-services-startup-tasks-common.md).

2. Adlı bir dosya oluşturun **Install.cmd** ve aşağıdaki yükleme betik dosyasına ekleme.

    Komut dosyası belirtilen .NET Framework sürümünü makinede kayıt sorgulayarak yüklü olup olmadığını denetler. .NET sürüm yüklü değilse .NET web yükleyicisi açılmış. Sorunları gidermenize yardımcı olması için betik tüm etkinlik depolanan startuptasklog-(geçerli tarih ve saat) .txt dosya oturum **InstallLogs** yerel depolama.
  
    > [!IMPORTANT]
    > Install.cmd dosyasını oluşturmak için Windows Not Defteri gibi bir temel metin düzenleyicisi kullanın. Bir metin dosyası oluşturun ve uzantı için .cmd değiştirmek için Visual Studio kullanıyorsanız, dosya UTF-8 bayt sırası işareti içeriyor olabilir. Komut dosyasının ilk satırını çalıştırdığınızda bu işareti hataya neden olabilir. Bu hatayı önlemek için komut dosyasının ilk satırı bayt sırası işlemesi atlanabilir REM deyimi olun. 
    > 
    >
   
    ```cmd
    REM Set the value of netfx to install appropriate .NET Framework. 
    REM ***** To install .NET 4.5.2 set the variable netfx to "NDP452" *****
    REM ***** To install .NET 4.6 set the variable netfx to "NDP46" *****
    REM ***** To install .NET 4.6.1 set the variable netfx to "NDP461" *****
    REM ***** To install .NET 4.6.2 set the variable netfx to "NDP462" *****
    REM ***** To install .NET 4.7 set the variable netfx to "NDP47" *****
    REM ***** To install .NET 4.7.1 set the variable netfx to "NDP47" *****
    set netfx="NDP471"
    
    REM ***** Set script start timestamp *****
    set timehour=%time:~0,2%
    set timestamp=%date:~-4,4%%date:~-10,2%%date:~-7,2%-%timehour: =0%%time:~3,2%
    set "log=install.cmd started %timestamp%."
    
    REM ***** Exit script if running in Emulator *****
    if %ComputeEmulatorRunning%=="true" goto exit
    
    REM ***** Needed to correctly install .NET 4.6.1, otherwise you may see an out of disk space error *****
    set TMP=%PathToNETFXInstall%
    set TEMP=%PathToNETFXInstall%
    
    REM ***** Setup .NET filenames and registry keys *****
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
    
    :NPD47
    set "netfxinstallfile=NDP47-KB3186500-Web.exe"
    set netfxregkey="0x707FE"
    goto logtimestamp
    
    :NDP471
    set "netfxinstallfile=NDP471-KB4033344-Web.exe"
    set netfxregkey="0x709fc"
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
    
    :restart
    echo Restarting to complete .NET (%netfx%) installation >> %startuptasklog%
    EXIT /B %ERRORLEVEL%
    
    :installed
    echo .NET (%netfx%) is installed >> %startuptasklog%
    
    :end
    echo install.cmd completed: %date:~-4,4%%date:~-10,2%%date:~-7,2%-%timehour: =0%%time:~3,2% >> %startuptasklog%
    
    :exit
    EXIT /B 0
    ```

3. Install.cmd dosyasını kullanarak her role eklemek **Ekle** > **varolan öğeyi** içinde **Çözüm Gezgini** bu konunun önceki kısımlarında açıklandığı gibi. 

    Bu adım tamamlandıktan sonra tüm rolleri Install.cmd dosyasını ve .NET yükleyici dosyası olmalıdır.

   ![Rol içeriğe sahip tüm dosyalar][2]

## <a name="configure-diagnostics-to-transfer-startup-logs-to-blob-storage"></a>Blob depolama alanına başlangıç günlükleri aktarmak için tanılama Yapılandır
Yükleme sorunlarını giderme basitleştirmek için başlangıç betiği veya Azure Blob depolama alanına .NET yükleyici tarafından oluşturulan tüm günlük dosyaları aktarmak için Azure tanılama yapılandırabilirsiniz. Bu yaklaşımı kullanarak, rol içine, Uzak Masaüstü'nü gerek kalmadan yerine Blob depolama alanından günlük dosyaları indirme günlükleri görüntüleyebilirsiniz.

Tanılama yapılandırmak için diagnostics.wadcfgx dosyasını açın ve aşağıdaki içeriği altında eklemek **dizinleri** düğümü: 

```xml 
<DataSources>
 <DirectoryConfiguration containerName="netfx-install">
  <LocalResource name="NETFXInstall" relativePath="log"/>
 </DirectoryConfiguration>
</DataSources>
```

Tanılama günlük dizininde dosya aktarmak için bu XML yapılandırır **NETFXInstall** kaynak tanılama depolama hesabı **netfx yükleme** blob kapsayıcısı.

## <a name="deploy-your-cloud-service"></a>Bulut hizmetinize dağıtın
Bulut hizmetinizi dağıttığınızda, başlangıç görevleri henüz yüklü değilse .NET Framework yükleyin. Bulut hizmeti rollerinizi bulunan *meşgul* framework yüklenirken durum. Framework yüklemesi bir yeniden başlatma gerektirirse, hizmet rolleri de yeniden başlatılabilir. 

## <a name="additional-resources"></a>Ek kaynaklar
* [.NET Framework yükleme][Installing the .NET Framework]
* [Hangi .NET Framework sürümlerinin yüklü olduğunu belirleme][How to: Determine Which .NET Framework Versions Are Installed]
* [.NET Framework yükleme sorunlarını giderme][Troubleshooting .NET Framework Installations]

[How to: Determine Which .NET Framework Versions Are Installed]: /dotnet/framework/migration-guide/how-to-determine-which-versions-are-installed
[Installing the .NET Framework]: /dotnet/framework/install/guide-for-developers
[Troubleshooting .NET Framework Installations]: /dotnet/framework/install/troubleshoot-blocked-installations-and-uninstallations

<!--Image references-->
[1]: ./media/cloud-services-dotnet-install-dotnet/rolecontentwithinstallerfiles.png
[2]: ./media/cloud-services-dotnet-install-dotnet/rolecontentwithallfiles.png
