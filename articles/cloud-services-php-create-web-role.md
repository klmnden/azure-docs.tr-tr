---
title: PHP için Azure web ve çalışan rolleri oluşturun
description: PHP web ve çalışan rolleri bir Azure bulut hizmeti oluşturma ve PHP çalışma zamanı yapılandırma için bir kılavuz.
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
ms.openlocfilehash: b9f350870dde71666d269aaae9cb7c14aaac5aad
ms.sourcegitcommit: 1362e3d6961bdeaebed7fb342c7b0b34f6f6417a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2018
---
# <a name="how-to-create-php-web-and-worker-roles"></a>PHP web ve çalışan rolleri oluşturma
## <a name="overview"></a>Genel Bakış
Bu kılavuzda Windows geliştirme ortamında PHP web veya çalışan rolleri oluşturmak, PHP belirli bir sürümü kullanılabilir "yerleşik" sürümlerinden birini seçin, PHP yapılandırmasını değiştirmek, uzantılarını etkinleştirme ve son olarak, Azure'a dağıtmak nasıl yapacağınızı gösterir. Ayrıca, sağladığınız PHP çalışma zamanı (özel yapılandırma ve birlikte uzantıları) kullanmak için bir web veya çalışan rolünün nasıl yapılandırılacağı açıklanmaktadır.

## <a name="what-are-php-web-and-worker-roles"></a>PHP web ve çalışan rolleri nelerdir?
Azure sağlayan üç işlem çalışan uygulamalar için modeli: Azure App Service, Azure sanal makineler ve Azure Cloud Services. Üç modeli PHP destekler. Web ve çalışan rolleri içerir, bulut hizmetleri sağlar *(PaaS) hizmet olarak platform*. Bir bulut hizmetinde ön uç web uygulamalarını barındırmak için adanmış bir Internet Information Services (IIS) web sunucusu bir web rolü sağlar. Çalışan rolü kullanıcı etkileşimi ve girişinden bağımsız zaman uyumsuz, uzun süre çalışan veya kalıcı görevleri çalıştırabilir.

Bu seçenekler hakkında daha fazla bilgi için bkz: [Azure tarafından sağlanan seçenekleri barındırma işlem](cloud-services/cloud-services-choose-me.md).

## <a name="download-the-azure-sdk-for-php"></a>PHP için Azure SDK'sını indirme
[PHP için Azure SDK] birçok bileşenden oluşur. Bu makalede iki tanesi kullanır: Azure PowerShell ve Azure öykünücüsünü. Bu iki bileşen Microsoft Web Platformu yükleyicisi aracılığıyla yüklenebilir. Daha fazla bilgi için bkz. [Azure PowerShell’i yükleme ve yapılandırma](/powershell/azure/overview).

## <a name="create-a-cloud-services-project"></a>Bulut Hizmetleri projesi oluşturma
Bir PHP web veya çalışan rolü oluşturmanın ilk adımı, bir Azure hizmeti projesi oluşturmaktır. bir Azure hizmeti projesi web ve çalışan rolleri için mantıksal bir kapsayıcı görevi görür ve projenin içeren [hizmet açıklaması (.csdef)] ve [hizmet yapılandırma (.cscfg)] dosyaları.

Yeni bir Azure hizmeti projesi oluşturmak için Azure PowerShell'i yönetici olarak çalıştırın ve aşağıdaki komutu yürütün:

    PS C:\>New-AzureServiceProject myProject

Bu komut yeni bir dizin oluşturur (`myProject`) web ve çalışan rolleri ekleme yapabilir.

## <a name="add-php-web-or-worker-roles"></a>PHP web veya çalışan rolleri Ekle
Bir PHP web rolü için bir proje eklemek için projenin kök dizini içinde aşağıdaki komutu çalıştırın:

    PS C:\myProject> Add-AzurePHPWebRole roleName

Çalışan rolü için bu komutu kullanın:

    PS C:\myProject> Add-AzurePHPWorkerRole roleName

> [!NOTE]
> `roleName` Parametresi isteğe bağlıdır. Belirtilmezse, rol adı otomatik olarak oluşturulur. Oluşturulan ilk web rolü olacaktır `WebRole1`, ikinci olacaktır `WebRole2`ve benzeri. Oluşturulan ilk çalışan rolü olacaktır `WorkerRole1`, ikinci olacaktır `WorkerRole2`ve benzeri.
>
>

## <a name="specify-the-built-in-php-version"></a>Yerleşik PHP sürümünü belirtin
Bir projeye bir PHP web veya çalışan rolü eklediğinizde, böylece her uygulamanızın web veya çalışan örneği üzerinde dağıtıldığında PHP yüklenecek projenin yapılandırma dosyalarını değiştirilmiştir. Varsayılan olarak yüklenen PHP sürümünü görmek için aşağıdaki komutu çalıştırın:

    PS C:\myProject> Get-AzureServiceProjectRoleRuntime

Yukarıdaki komut çıktısı, aşağıda gösterilen öğeleri için benzer görünecektir. Bu örnekte, `IsDefault` bayrağı ayarlanmış `true` yüklü varsayılan PHP sürümü olacağını belirten php 5.3.17,.

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

PHP çalıştırma sürümünü sürümlerinin herhangi birinin listelenen PHP için ayarlayabilirsiniz. Örneğin, PHP sürümünü ayarlamak için (ada sahip bir rol için `roleName`) 5.4.0 için aşağıdaki komutu kullanın:

    PS C:\myProject> Set-AzureServiceProjectRole roleName php 5.4.0

> [!NOTE]
> Kullanılabilir PHP sürümlerin gelecekte değişebilir.
>
>

## <a name="customize-the-built-in-php-runtime"></a>Yerleşik PHP çalışma zamanı özelleştirme
Yukarıdaki değiştirilmesini dahil olmak üzere adımları izlediğinizde yüklediğiniz PHP çalışma zamanına ilişkin yapılandırmayı üzerinde tam denetime sahip `php.ini` ayarları ve uzantılarını etkinleştirme.

Yerleşik PHP çalışma zamanı özelleştirmek için aşağıdaki adımları izleyin:

1. Adlı yeni bir klasör ekleyin `php`, `bin` web rolünüz dizininde. Çalışan rolü için rol kök dizinine ekleyin.
2. İçinde `php` klasörünü adlı başka bir klasör oluşturun `ext`. Herhangi bir yerleştirme `.dll` uzantılarını (örneğin, `php_mongo.dll`) bu klasörde etkinleştirmek istediğiniz.
3. Ekleme bir `php.ini` dosya `php` klasör. Tüm özel uzantılar etkinleştirin ve bu dosyadaki tüm PHP yönergeleri ayarlayın. Örneğin, etkinleştirmek istiyorsanız `display_errors` üzerinde ve etkinleştirme `php_mongo.dll` uzantısı, içeriğini, `php.ini` dosya şu şekilde olacaktır:

        display_errors=On
        extension=php_mongo.dll

> [!NOTE]
> Açıkça ayarlamazsanız herhangi bir ayarı `php.ini` dosya otomatik olarak sağlamak için varsayılan değerleri ayarlanabilir. Ancak, bir tam ekleyebilirsiniz göz önünde bulundurmanız `php.ini` dosya.
>
>

## <a name="use-your-own-php-runtime"></a>Kendi PHP çalışma zamanı kullanın
Yerleşik bir PHP çalışma zamanı seçme ve yukarıda açıklandığı gibi yapılandırmak yerine bazı durumlarda, kendi PHP çalışma zamanı sağlamak isteyebilirsiniz. Örneğin, geliştirme ortamınızı kullandığınız web veya çalışan rolü aynı PHP çalışma zamanı kullanabilirsiniz. Bu uygulamayı üretim ortamınıza davranış değişmez emin olmak kolaylaştırır.

### <a name="configure-a-web-role-to-use-your-own-php-runtime"></a>Bir web rolü, kendi PHP çalışma zamanı kullanmak için yapılandırma
Sağladığınız bir PHP çalışma zamanı kullanmak için bir web rolü yapılandırmak için aşağıdaki adımları izleyin:

1. Bir Azure hizmeti projesi oluşturun ve bu konuda daha önce açıklandığı gibi bir PHP web rolü ekleyin.
2. Oluşturma bir `php` klasöründe `bin` web rolün kök dizininde bulunan ve daha sonra PHP çalışma zamanı (tüm ikili dosyaları, yapılandırma dosyalarını, alt klasörler, vb.) ekleyin klasör `php` klasör.
3. (İSTEĞE BAĞLI) PHP çalışma zamanı kullanıyorsa [SQL Server için PHP için Microsoft Drivers][sqlsrv drivers], yüklemek için web rolünüz yapılandırmanız gerekecektir [SQL Server Native Client 2012] [ sql native client] , ne zaman hazırlanır. Bunu yapmak için ekleyin [sqlncli.msi x64 yükleyici] için `bin` web rolün kök dizininde klasör. Rol sağlandığında sonraki adımda açıklanan başlangıç betiği sessizce yükleyiciyi çalıştırın. PHP çalışma zamanı, SQL Server için PHP için Microsoft Drivers kullanmıyorsa, sonraki adımda gösterilen komut dosyasından aşağıdaki satırı kaldırabilirsiniz:

        msiexec /i sqlncli.msi /qn IACCEPTSQLNCLILICENSETERMS=YES
4. Yapılandırır bir başlangıç görevi tanımlamak [Internet Information Services (IIS)] [ iis.net] isteklerini işlemek için PHP çalışma zamanı kullanmak için `.php` sayfaları. Bunu yapmak için açın `setup_web.cmd` dosyası (içinde `bin` web rolün kök dizin dosyası) bir metin düzenleyicisinde ve aşağıdaki komut dosyasının içeriğini değiştirin:

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
5. Uygulama dosyalarınızı web rolün kök dizinine ekleyin. Bu web sunucusunun kök dizininde olacaktır.
6. Bölümünde açıklandığı gibi uygulamanızı yayımlamak [uygulamanızı yayımlamak](#publish-your-application) bölümüne bakın.

> [!NOTE]
> `download.ps1` Komut dosyası (içinde `bin` web rolün kök dizininin klasör), kendi PHP çalışma zamanı kullanmak için yukarıda anlatılan adımları izledikten sonra silinebilir.
>
>

### <a name="configure-a-worker-role-to-use-your-own-php-runtime"></a>Çalışan rolü, kendi PHP çalışma zamanı kullanmak için yapılandırma
Sağladığınız bir PHP çalışma zamanı kullanmak için bir çalışan rolünü yapılandırmak için aşağıdaki adımları izleyin:

1. Bir Azure hizmeti projesi oluşturun ve bu konuda daha önce açıklandığı gibi PHP çalışan rolü ekleyin.
2. Oluşturma bir `php` çalışan rolün kök dizininde klasörü, PHP çalışma zamanı (tüm ikili dosyaları, yapılandırma dosyalarını, alt klasörler, vb.) sonra ekleyin `php` klasör.
3. (İSTEĞE BAĞLI) PHP çalışma zamanı kullanıyorsa [SQL Server için PHP için Microsoft Drivers][sqlsrv drivers], yüklemek için çalışan rolü yapılandırmanız gerekecektir [SQL Server Native Client 2012] [ sql native client] , ne zaman hazırlanır. Bunu yapmak için ekleyin [sqlncli.msi x64 yükleyici] çalışan rolün kök dizinine. Rol sağlandığında sonraki adımda açıklanan başlangıç betiği sessizce yükleyiciyi çalıştırın. PHP çalışma zamanı, SQL Server için PHP için Microsoft Drivers kullanmıyorsa, sonraki adımda gösterilen komut dosyasından aşağıdaki satırı kaldırabilirsiniz:

        msiexec /i sqlncli.msi /qn IACCEPTSQLNCLILICENSETERMS=YES
4. Ekler bir başlangıç görevi tanımlamak, `php.exe` rolü sağlandığında çalışan rolün PATH ortam değişkeni için çalıştırılabilir. Bunu yapmak için açık `setup_worker.cmd` (çalışan rolün kök dizininde) bir metin Düzenleyicisi'nde dosya ve aşağıdaki komut dosyasının içeriğini değiştirin:

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
6. Bölümünde açıklandığı gibi uygulamanızı yayımlamak [uygulamanızı yayımlamak](#publish-your-application) bölümüne bakın.

## <a name="run-your-application-in-the-compute-and-storage-emulators"></a>İşlem ve depolama öykünücüsünü uygulamanızı çalıştırma
Azure öykünücüsünü buluta dağıtmadan önce Azure uygulamanızı sınayabilir yerel bir ortam sağlar. Öykünücüler ve Azure ortamı arasındaki bazı farklar vardır. Bu daha iyi anlamak için bkz: [geliştirme ve sınama için Azure storage öykünücüsünü kullanma](storage/common/storage-use-emulator.md).

PHP işlem öykünücüsü yerel olarak yüklü olması gerektiğini unutmayın. İşlem öykünücüsü, uygulamanızı çalıştırmak için yerel PHP yüklemenizi kullanır.

Projenizi Öykünücüler çalıştırmak için projenin kök dizininden aşağıdaki komutu yürütün:

    PS C:\MyProject> Start-AzureEmulator

Aşağıdakine benzer bir çıktı görürsünüz:

    Creating local package...
    Starting Emulator...
    Role is running at http://127.0.0.1:81
    Started

Web tarayıcısında açarak ve çıktıda gösterilen yerel adres için gözatma öykünücüsünde çalıştıran uygulamanız görebilirsiniz (`http://127.0.0.1:81` Yukarıdaki örnek çıktıda).

Öykünücüler durdurmak için bu komutu çalıştırın:

    PS C:\MyProject> Stop-AzureEmulator

## <a name="publish-your-application"></a>Uygulamanızı yayımlama
Uygulamanızı yayımlamak için ilk içeri aktarmanız gerekir, yayımlama ayarları kullanarak [Import-AzurePublishSettingsFile](https://msdn.microsoft.com/library/azure/dn790370.aspx) cmdlet'i. Uygulamanızı kullanarak yayımlayabilirsiniz sonra [Publish-AzureServiceProject](https://msdn.microsoft.com/library/azure/dn495166.aspx) cmdlet'i. Oturum açma hakkında daha fazla bilgi için bkz: [Azure PowerShell'i yükleme ve yapılandırma nasıl](/powershell/azure/overview).

## <a name="next-steps"></a>Sonraki adımlar
Daha fazla bilgi için bkz: [PHP Geliştirici Merkezi](/develop/php/).

[PHP için Azure SDK]: /develop/php/common-tasks/download-php-sdk/
[install ps and emulators]: http://go.microsoft.com/fwlink/p/?linkid=320376&clcid=0x409
[hizmet açıklaması (.csdef)]: http://msdn.microsoft.com/library/windowsazure/ee758711.aspx
[hizmet yapılandırma (.cscfg)]: http://msdn.microsoft.com/library/windowsazure/ee758710.aspx
[iis.net]: http://www.iis.net/
[sql native client]: http://msdn.microsoft.com/sqlserver/aa937733.aspx
[sqlsrv drivers]: http://php.net/sqlsrv
[sqlncli.msi x64 yükleyici]: http://go.microsoft.com/fwlink/?LinkID=239648
