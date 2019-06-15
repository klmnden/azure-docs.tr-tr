---
title: PHP için Azure web ve çalışan rolleri oluşturma
description: Bir Azure bulut hizmetinde PHP web ve çalışan rolleri oluşturma ve PHP çalışma zamanı yapılandırma için bir kılavuz.
services: ''
documentationcenter: php
author: msangapu
manager: cfowler
ms.assetid: 9f7ccda0-bd96-4f7b-a7af-fb279a9e975b
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: PHP
ms.topic: article
ms.date: 04/11/2018
ms.author: msangapu
ms.openlocfilehash: 83834104dd73e4381947903196ad35c3497b64a1
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60337570"
---
# <a name="create-php-web-and-worker-roles"></a>PHP web ve çalışan rolleri oluşturma

## <a name="overview"></a>Genel Bakış

Bu kılavuz, Windows geliştirme ortamında PHP web veya çalışan rolleri oluşturma, belirli bir PHP sürümünü kullanılabilir "yerleşik" sürümlerinden birini seçin, PHP yapılandırmasını değiştirmek, uzantılarını etkinleştirme ve son olarak, Azure'a dağıtma gösterilmektedir. Ayrıca, sağlayan bir PHP çalışma zamanı (ile özel yapılandırma ve uzantıları) kullanmak için bir web veya çalışan rolünün nasıl yapılandırılacağı açıklanır.

Azure, uygulamaları çalıştırmak için üç bilgi işlem modeli sağlar: Azure App Service, Azure sanal makineleri ve Azure bulut Hizmetleri. PHP üç modeli destekler. Web ve çalışan rollerini içeren bulut hizmetleri sunan *(PaaS) hizmet olarak platform*. Bir bulut hizmetinde ön uç web uygulamalarını barındırmak için adanmış bir Internet Information Services (IIS) web sunucusu bir web rolü sağlar. Bir çalışan rolü, kullanıcı etkileşimi veya girişi bağımsız zaman uyumsuz, uzun süreli veya kalıcı görevleri çalıştırabilir.

Bu seçenekler hakkında daha fazla bilgi için bkz. [işlem barındırma seçenekleri Azure tarafından sağlanan](cloud-services/cloud-services-choose-me.md).

## <a name="download-the-azure-sdk-for-php"></a>PHP için Azure SDK'sını indirme

[PHP için Azure SDK'sı](php-download-sdk.md) birkaç bileşenden oluşur. Bu makalede, iki tanesi kullanın: Azure PowerShell ve Azure öykünücüleri. Bu iki bileşen Microsoft Web Platformu yükleyicisi aracılığıyla yüklenebilir. Daha fazla bilgi için bkz. [Azure PowerShell’i yükleme ve yapılandırma](/powershell/azure/overview).

## <a name="create-a-cloud-services-project"></a>Cloud Services projesi oluşturma

Bir PHP web veya çalışan rolü oluşturmanın ilk adımı, bir Azure hizmeti projesi oluşturmaktır. bir Azure hizmeti projesi web ve çalışan rolleri için mantıksal kapsayıcı görevi görür ve projenin içerdiği [hizmet tanımı (.csdef)] ve [hizmet yapılandırma (.cscfg)] dosyaları.

Yeni bir Azure hizmet projesi oluşturmak için Azure PowerShell'i yönetici olarak çalıştırın ve aşağıdaki komutu yürütün:

    PS C:\>New-AzureServiceProject myProject

Bu komut yeni bir dizin oluşturur (`myProject`) web ve çalışan rolleri ekleme yapabilir.

## <a name="add-php-web-or-worker-roles"></a>PHP web veya çalışan rolleri ekleme

Bir projeye bir PHP web rolü eklemek için projenin kök dizininden aşağıdaki komutu çalıştırın:

    PS C:\myProject> Add-AzurePHPWebRole roleName

Bir çalışan rolü için bu komutu kullanın:

    PS C:\myProject> Add-AzurePHPWorkerRole roleName

> [!NOTE]
> `roleName` Parametresi isteğe bağlıdır. Belirtilmezse, rol adı otomatik olarak oluşturulur. Oluşturulan ilk web rolü `WebRole1`, ikinci olacaktır `WebRole2`ve benzeri. Oluşturulan ilk çalışan rolü olacaktır `WorkerRole1`, ikinci olacaktır `WorkerRole2`ve benzeri.
>
>

## <a name="specify-the-built-in-php-version"></a>Yerleşik PHP sürümünü belirtin

Bir PHP web veya çalışan rolü için bir proje eklediğinizde, projenin yapılandırma dosyalarını dağıtıldığında, PHP, uygulamanızın her web veya çalışan örneğinde yüklenir, böylece değiştirilir. Varsayılan olarak yüklenen PHP sürümünü görmek için aşağıdaki komutu çalıştırın:

    PS C:\myProject> Get-AzureServiceProjectRoleRuntime

Yukarıdaki komut çıktısı, aşağıda gösterilene için benzer görünecektir. Bu örnekte, `IsDefault` bayrağı ayarlandığında `true` PHP 5.3.17, yüklü varsayılan PHP sürümünü olacağını belirten için.

```
Runtime Version     PackageUri                      IsDefault
------- -------     ----------                      ---------
Node 0.6.17         http://nodertncu.blob.core...   False
Node 0.6.20         http://nodertncu.blob.core...   True
Node 0.8.4          http://nodertncu.blob.core...   False
IISNode 0.1.21      http://nodertncu.blob.core...   True
Cache 1.8.0         http://nodertncu.blob.core...   True
PHP 5.3.17          http://nodertncu.blob.core...   True
PHP 5.4.0           http://nodertncu.blob.core...   False
```

PHP çalışma zamanı sürümü sürümlerinin herhangi birinin listelenen PHP için ayarlayabilirsiniz. Örneğin, PHP sürümünü ayarlamak için (ada sahip bir rol için `roleName`) 5.4.0 için aşağıdaki komutu kullanın:

    PS C:\myProject> Set-AzureServiceProjectRole roleName php 5.4.0

> [!NOTE]
> PHP sürümlerini gelecekte değişebilir.
>
>

## <a name="customize-the-built-in-php-runtime"></a>Yerleşik PHP çalışma zamanı özelleştirme

Yukarıdaki değişikliği de dahil olmak üzere adımları izlediğinizde yüklenen PHP çalışma zamanı yapılandırması üzerinde tam denetime sahip `php.ini` ayarları ve uzantılarını etkinleştirme.

Yerleşik PHP çalışma zamanını özelleştirmek için aşağıdaki adımları izleyin:

1. Adlı yeni bir klasör ekleme `php`, `bin` web rolü dizin. Bir çalışan rolü için rol kök dizinine ekleyin.
2. İçinde `php` klasörü, adlı başka bir klasör oluşturun `ext`. Tüm put `.dll` uzantılarını (örneğin, `php_mongo.dll`) bu klasörde etkinleştirmek istediğiniz.
3. Ekleme bir `php.ini` dosyasını `php` klasör. Herhangi bir özel uzantıyı etkinleştirmek ve bu dosyayı herhangi bir PHP yönergeleri ayarlayın. Örneğin, açmak istiyorsanız `display_errors` üzerinde ve etkinleştirme `php_mongo.dll` uzantısı, içeriğini, `php.ini` dosya şu şekilde olacaktır:

        display_errors=On
        extension=php_mongo.dll

> [!NOTE]
> Açıkça ayarlamamanız herhangi bir ayarı `php.ini` dosya otomatik olarak sağlar, varsayılan değerlerine ayarlanmış olması. Ancak, bir tam ekleyebilirsiniz akılda tutulması `php.ini` dosya.
>
>

## <a name="use-your-own-php-runtime"></a>Kendi PHP çalışma zamanı kullanın

Yerleşik PHP çalışma zamanının seçilmesi ve yukarıda açıklandığı gibi yapılandırmak yerine, bazı durumlarda, kendi PHP çalışma zamanı sağlamak isteyebilirsiniz. Örneğin, geliştirme ortamınızda kullanan bir web veya çalışan rolü aynı PHP çalışma zamanı kullanabilirsiniz. Bu, uygulamayı üretim ortamınızda davranışı değişmez emin olmak kolaylaştırır.

### <a name="configure-a-web-role-to-use-your-own-php-runtime"></a>Bir web rolü, kendi PHP çalışma zamanı kullanmak için yapılandırma

Bir web rolü sağlayan bir PHP çalışma zamanı kullanmak üzere yapılandırmak için aşağıdaki adımları izleyin:

1. Bir Azure hizmet projesi oluşturun ve bu konuda daha önce açıklandığı gibi bir PHP web rolü ekleyin.
2. Oluşturma bir `php` klasöründe `bin` web rolün kök dizininde ise, PHP çalışma zamanı (tüm ikili dosyaları, yapılandırma dosyalarını, alt klasörler, vb.) sonra ekleyin klasörü `php` klasör.
3. (İSTEĞE BAĞLI) PHP çalışma zamanı kullanıyorsa [SQL Server için PHP için Microsoft Drivers][sqlsrv drivers], yüklemek için web rolünüz yapılandırmanız gerekecektir [SQL Server Native Client 2012] [ sql native client] bunu ne zaman hazırlanır. Bunu yapmak için ekleme [sqlncli.msi x64 yükleyici] için `bin` klasöründe web rolün kök dizini. Rol sağlandığında sonraki adımda açıklanan başlangıç betiği sessizce yükleyiciyi çalıştırın. PHP çalışma zamanı, SQL Server için PHP için Microsoft Drivers kullanmıyorsa, sonraki adımda gösterilen betikten şu satırı kaldırın:

        msiexec /i sqlncli.msi /qn IACCEPTSQLNCLILICENSETERMS=YES
4. Yapılandıran bir başlangıç görevi tanımlayın [Internet Information Services (IIS)] [ iis.net] isteklerini işlemek için PHP çalışma zamanı kullanmak için `.php` sayfaları. Bunu yapmak için açık `setup_web.cmd` dosyası (içinde `bin` web rolün kök dizin dosyası) bir metin düzenleyicisinde ve içeriğini aşağıdaki betikle değiştirin:

    ```cmd
    @ECHO ON
    cd "%~dp0"

    if "%EMULATED%"=="true" exit /b 0

    msiexec /i sqlncli.msi /qn IACCEPTSQLNCLILICENSETERMS=YES

    SET PHP_FULL_PATH=%~dp0php\php-cgi.exe
    SET NEW_PATH=%PATH%;%RoleRoot%\base\x86

    %WINDIR%\system32\inetsrv\appcmd.exe set config -section:system.webServer/fastCgi /+"[fullPath='%PHP_FULL_PATH%',maxInstances='12',idleTimeout='60000',activityTimeout='3600',requestTimeout='60000',instanceMaxRequests='10000',protocol='NamedPipe',flushNamedPipe='False']" /commit:apphost
    %WINDIR%\system32\inetsrv\appcmd.exe set config -section:system.webServer/fastCgi /+"[fullPath='%PHP_FULL_PATH%'].environmentVariables.[name='PATH',value='%NEW_PATH%']" /commit:apphost
    %WINDIR%\system32\inetsrv\appcmd.exe set config -section:system.webServer/fastCgi /+"[fullPath='%PHP_FULL_PATH%'].environmentVariables.[name='PHP_FCGI_MAX_REQUESTS',value='10000']" /commit:apphost
    %WINDIR%\system32\inetsrv\appcmd.exe set config -section:system.webServer/handlers /+"[name='PHP',path='*.php',verb='GET,HEAD,POST',modules='FastCgiModule',scriptProcessor='%PHP_FULL_PATH%',resourceType='Either',requireAccess='Script']" /commit:apphost
    %WINDIR%\system32\inetsrv\appcmd.exe set config -section:system.webServer/fastCgi /"[fullPath='%PHP_FULL_PATH%'].queueLength:50000"
    ```
5. Uygulama dosyalarınızı web rolün kök dizinine ekleyin. Bu web sunucusu sertifikasının kök dizini olacaktır.
6. Açıklanan şekilde uygulamanızı yayımlayın [uygulamanızı yayımlayın](#publish-your-application) bölümüne bakın.

> [!NOTE]
> `download.ps1` Betik (içinde `bin` web rolün kök dizininin klasör), kendi PHP çalışma zamanı kullanmak için yukarıda anlatılan adımları izledikten sonra silinebilir.
>
>

### <a name="configure-a-worker-role-to-use-your-own-php-runtime"></a>Bir çalışan rolü, kendi PHP çalışma zamanı kullanmak için yapılandırma

Bir çalışan rolü sağlayan bir PHP çalışma zamanı kullanmak üzere yapılandırmak için aşağıdaki adımları izleyin:

1. Bir Azure hizmet projesi oluşturun ve bu konuda daha önce açıklandığı gibi bir PHP çalışan rolü ekleyin.
2. Oluşturma bir `php` çalışan rolün kök dizininde, klasör ve, PHP çalışma zamanı (tüm ikili dosyaları, yapılandırma dosyalarını, alt klasörler, vb.) eklemek `php` klasör.
3. (İSTEĞE BAĞLI) PHP çalışma zamanı kullanıyorsa [SQL Server için PHP için Microsoft Drivers][sqlsrv drivers], yüklemek için çalışan rolü yapılandırmanız gerekecektir [SQL Server Native Client 2012] [ sql native client] bunu ne zaman hazırlanır. Bunu yapmak için ekleme [sqlncli.msi x64 yükleyici] çalışan rolün kök dizini. Rol sağlandığında sonraki adımda açıklanan başlangıç betiği sessizce yükleyiciyi çalıştırın. PHP çalışma zamanı, SQL Server için PHP için Microsoft Drivers kullanmıyorsa, sonraki adımda gösterilen betikten şu satırı kaldırın:

        msiexec /i sqlncli.msi /qn IACCEPTSQLNCLILICENSETERMS=YES
4. Ekleyen bir başlangıç görevi tanımlayın, `php.exe` rol sağlandığında çalışan rolün yol ortam değişkenine yürütülebilir. Bunu yapmak için açık `setup_worker.cmd` (çalışan rolün kök dizinine) bir metin Düzenleyicisi'nde dosya ve içeriğini aşağıdaki betikle değiştirin:

    ```cmd
    @echo on

    cd "%~dp0"

    echo Granting permissions for Network Service to the web root directory...
    icacls ..\ /grant "Network Service":(OI)(CI)W
    if %ERRORLEVEL% neq 0 goto error
    echo OK

    if "%EMULATED%"=="true" exit /b 0

    msiexec /i sqlncli.msi /qn IACCEPTSQLNCLILICENSETERMS=YES

    setx Path "%PATH%;%~dp0php" /M

    if %ERRORLEVEL% neq 0 goto error

    echo SUCCESS
    exit /b 0

    :error

    echo FAILED
    exit /b -1
    ```
5. Uygulama dosyalarınızı çalışan rolün kök dizinine ekleyin.
6. Açıklanan şekilde uygulamanızı yayımlayın [uygulamanızı yayımlayın](#publish-your-application) bölümüne bakın.

## <a name="run-your-application-in-the-compute-and-storage-emulators"></a>İşlem ve depolama öykünücüleri uygulamanızı çalıştırın

Azure öykünücüleri buluta dağıtmadan önce Azure uygulamanızı sınayabilir yerel bir ortam sağlar. Öykünücü ve Azure ortamı arasında bazı farklar vardır. Bu daha iyi anlamak için bkz: [geliştirme ve test için Azure depolama öykünücüsü kullanma](storage/common/storage-use-emulator.md).

PHP işlem öykünücüsü yerel olarak yüklü olması gerektiğini unutmayın. İşlem öykünücüsü, uygulamanızı çalıştırmak için yerel PHP yüklemenizi kullanır.

Projenizi öykünücüleri çalıştırmak için projenizin kök dizininden aşağıdaki komutu yürütün:

    PS C:\MyProject> Start-AzureEmulator

Şuna benzer bir çıktı görürsünüz:

    Creating local package...
    Starting Emulator...
    Role is running at http://127.0.0.1:81
    Started

Bir web tarayıcısı açıp çıktıda gösterilen yerel adres için gözatma öykünücüsünde çalışan uygulamanızı görebilirsiniz (`http://127.0.0.1:81` Yukarıdaki örnek çıktıda).

Öykünücüler durdurmak için bu komutu çalıştırın:

    PS C:\MyProject> Stop-AzureEmulator

## <a name="publish-your-application"></a>Uygulamanızı yayımlama

Uygulamanızı yayımlamak için ilk kez içeri aktarmanız gerekir, yayımlama ayarları kullanarak [Import-AzurePublishSettingsFile](https://docs.microsoft.com/powershell/module/servicemanagement/azure/import-azurepublishsettingsfile) cmdlet'i. Uygulamanızı kullanarak yayımlayabilirsiniz sonra [Publish-AzureServiceProject](https://docs.microsoft.com/powershell/module/servicemanagement/azure/publish-azureserviceproject) cmdlet'i. Oturum açma hakkında daha fazla bilgi için bkz: [Azure PowerShell'i yükleme ve yapılandırma işlemini](/powershell/azure/overview).

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi için [PHP Geliştirici Merkezi](https://azure.microsoft.com/develop/php/).

[install ps and emulators]: https://go.microsoft.com/fwlink/p/?linkid=320376&clcid=0x409
[Hizmet tanımı (.csdef)]: https://msdn.microsoft.com/library/windowsazure/ee758711.aspx
[Hizmet yapılandırma (.cscfg)]: https://msdn.microsoft.com/library/windowsazure/ee758710.aspx
[iis.net]: https://www.iis.net/
[sql native client]: https://docs.microsoft.com/sql/sql-server/sql-server-technical-documentation
[sqlsrv drivers]: https://php.net/sqlsrv
[sqlncli.msi x64 yükleyici]: https://go.microsoft.com/fwlink/?LinkID=239648
