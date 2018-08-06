---
title: Azure App Service Web uygulamalarında PHP yapılandırma
description: Varsayılan PHP yüklemesini yapılandırmak veya Azure App service'taki Web Apps için özel bir PHP yükleme nasıl ekleneceğini öğrenin.
services: app-service
documentationcenter: php
author: msangapu
manager: cfowler
ms.assetid: 95c4072b-8570-496b-9c48-ee21a223fb60
ms.service: app-service
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: PHP
ms.topic: article
ms.date: 04/11/2018
ms.author: msangapu
ms.openlocfilehash: 6e7de0a7b580c0028982895487117ab98d0cd612
ms.sourcegitcommit: 9222063a6a44d4414720560a1265ee935c73f49e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/03/2018
ms.locfileid: "39503460"
---
# <a name="configure-php-in-azure-app-service-web-apps"></a>Azure App Service Web uygulamalarında PHP yapılandırma

## <a name="introduction"></a>Giriş

Bu kılavuz için Web Apps'te yerleşik PHP çalışma zamanı yapılandırma işlemi gösterilmektedir [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714)özel bir PHP çalışma zamanı sağlama ve genişletmeleri etkinleştirme. App Service'ı kullanmak için kaydolun [ücretsiz deneme sürümü]. Bu kılavuzu en almak için ilk App Service'te PHP web uygulaması oluşturmanız gerekir.

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="how-to-change-the-built-in-php-version"></a>Nasıl yapılır: yerleşik PHP sürümünü değiştirme

Bir App Service web uygulaması oluşturduğunuzda varsayılan olarak, PHP 5.6 yüklenir ve kullanmak için hemen kullanılabilir. Kullanılabilir sürüm düzeltme, varsayılan yapılandırması ve uzantılar görmek için en iyi yolu, çağıran bir komut dosyası dağıtmaktır [phpinfo()] işlevi.

PHP 7.0 ve PHP 7.2 sürümleri de kullanılabilir, ancak varsayılan olarak etkin değildir. PHP sürümünü güncelleştirmek için aşağıdaki yöntemlerden birini izleyin:

### <a name="azure-portal"></a>Azure portal

1. Web uygulamanıza gidin [Azure portalında](https://portal.azure.com) tıklayın **ayarları** düğmesi.

    ![Web uygulaması ayarları][settings-button]
1. Gelen **ayarları** dikey penceresinde **uygulama ayarları** ve en yeni PHP sürümünü seçin.

    ![Uygulama Ayarları][application-settings]
1. Tıklayın **Kaydet** üst kısmındaki düğmeye **Web uygulaması ayarları** dikey penceresi.

    ![Yapılandırma ayarlarını Kaydet][save-button]

### <a name="azure-powershell-windows"></a>Azure PowerShell (Windows)

1. Azure PowerShell'i açın ve hesabınızda oturum açın:

        PS C:\> Connect-AzureRmAccount
1. Web uygulaması için PHP sürümünü ayarlayın.

        PS C:\> Set-AzureWebsite -PhpVersion {5.6 | 7.0 | 7.2} -Name {app-name}
1. Şimdi PHP sürümünü ayarlayın. Bu ayarlar doğrulayabilirsiniz:

        PS C:\> Get-AzureWebsite -Name {app-name} | findstr PhpVersion

### <a name="azure-cli-20-linux-mac-windows"></a>Azure CLI 2.0 (Linux, Mac, Windows)

Azure komut satırı arabirimini kullanmak için şunları yapmalısınız [Azure CLI 2.0 yükleme](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) bilgisayarınızda.

1. Açık Terminal ve hesabınızda oturum açın.

        az login

1. Desteklenen çalışma zamanları listesini görmek için kontrol edin.

        az webapp list-runtimes | grep php

1. Web uygulaması için PHP sürümünü ayarlayın.

        az webapp config set --php-version {5.6 | 7.0 | 7.1 | 7.2} --name {app-name} --resource-group {resource-group-name}

1. Şimdi PHP sürümünü ayarlayın. Bu ayarlar doğrulayabilirsiniz:

        az webapp show --name {app-name} --resource-group {resource-group-name}

## <a name="how-to-change-the-built-in-php-configurations"></a>Nasıl yapılır: yerleşik PHP yapılandırmalarını değiştirme

Tüm yerleşik PHP çalışma zamanı için yapılandırma seçeneklerinin aşağıdaki adımları izleyerek değiştirebilirsiniz. (Php.ini yönergeleri hakkında daha fazla bilgi için bkz. [php.ini yönergeleri listesi].)

### <a name="changing-phpiniuser-phpiniperdir-phpiniall-configuration-settings"></a>PHP değiştirme\_ını\_kullanıcı, PHP\_ını\_PERDIR, PHP\_ını\_tüm yapılandırma ayarları

1. Ekleme bir [. user.ini] kök dizininize dosya.
1. Yapılandırma ayarlarını eklemek `.user.ini` içinde kullandığınız aynı sözdizimini kullanılarak dosya bir `php.ini` dosya. Örneğin, açmak istediğiniz `display_errors` ayarı ve ayarlanmış `upload_max_filesize` 10 milyon için ayarlama, `.user.ini` bu metin dosyasını içerebilir:

        ; Example Settings
        display_errors=On
        upload_max_filesize=10M

        ; OPTIONAL: Turn this on to write errors to d:\home\LogFiles\php_errors.log
        ; log_errors=On
1. Web uygulamanızı dağıtın.
1. Web uygulamasını yeniden başlatın. (Yeniden başlatma gereklidir çünkü hangi PHP sıklığı okur `.user.ini` dosyaları tarafından yönetilir `user_ini.cache_ttl` sistem düzeyi ayarı ve 300 saniye (5 dakika) varsayılan olarak açıktır. ayarı. Yeni ayarları okumak için PHP web uygulaması yeniden başlatılıyor zorlar `.user.ini` dosyası.)

Kullanmaya alternatif olarak bir `.user.ini` kullanabileceğiniz dosyası [ini_set()] sistem düzeyindeki yönergeleri olmayan yapılandırma seçeneklerini ayarlamak için komut işlevi.

### <a name="changing-phpinisystem-configuration-settings"></a>PHP değiştirme\_ını\_sistemi yapılandırma ayarları

1. Bir uygulama ayarı, anahtarı ile Web uygulamanıza ekleme `PHP_INI_SCAN_DIR` ve değer `d:\home\site\ini`
1. Oluşturma bir `settings.ini` Kudu konsolunu kullanarak dosya (http://&lt;site adı&gt;. scm.azurewebsite.net) içinde `d:\home\site\ini` dizin.
1. Yapılandırma ayarlarını eklemek `settings.ini` içinde kullandığınız aynı sözdizimini kullanılarak dosya bir `php.ini` dosya. Örneğin, işaret edecek şekilde istediyseniz `curl.cainfo` ayarını bir `*.crt` dosya ve 512 K 'wincache.maxfilesize' ayarı, `settings.ini` bu metin dosyasını içerebilir:

        ; Example Settings
        curl.cainfo="%ProgramFiles(x86)%\Git\bin\curl-ca-bundle.crt"
        wincache.maxfilesize=512
1. Değişiklikleri tekrar yüklemek için Web uygulamanızı yeniden başlatın.

## <a name="how-to-enable-extensions-in-the-default-php-runtime"></a>Nasıl yapılır: varsayılan PHP çalışma zamanı uzantılarını etkinleştir

Önceki bölümde belirtildiği gibi varsayılan PHP sürümünü, varsayılan yapılandırması ve uzantılar görmek için en iyi yolu çağıran bir komut dosyası dağıtmaktır [phpinfo()]. Aşağıdaki adımları izleyerek ek uzantıları etkinleştirmek için:

### <a name="configure-via-ini-settings"></a>INI ayarları yapılandırın

1. Ekleme bir `ext` dizininden `d:\home\site` dizin.
1. PUT `.dll` dosyalar uzantısı `ext` dizin (örneğin, `php_xdebug.dll`). Uzantılar, PHP, olan VC9 ve iş parçacığı güvenli olmayan (nts) uyumlu varsayılan sürümü ile uyumlu olduğundan emin olun.
1. Bir uygulama ayarı, anahtarı ile Web uygulamanıza ekleme `PHP_INI_SCAN_DIR` ve değer `d:\home\site\ini`
1. Oluşturma bir `ini` dosyası `d:\home\site\ini` adlı `extensions.ini`.
1. Yapılandırma ayarlarını eklemek `extensions.ini` içinde kullandığınız aynı sözdizimini kullanılarak dosya bir `php.ini` dosya. Örneğin, MongoDB ve XDebug uzantıları etkinleştirmek istiyorsanız, `extensions.ini` bu metin dosyasını içerebilir:

        ; Enable Extensions
        extension=d:\home\site\ext\php_mongo.dll
        zend_extension=d:\home\site\ext\php_xdebug.dll
1. Değişiklikleri yüklemek için Web uygulamanızı yeniden başlatın.

### <a name="configure-via-app-setting"></a>Uygulama ayarı aracılığıyla yapılandırma

1. Ekleme bir `bin` kök dizinine dizin.
1. PUT `.dll` dosyalar uzantısı `bin` dizin (örneğin, `php_xdebug.dll`). Uzantılar, PHP, olan VC9 ve iş parçacığı güvenli olmayan (nts) uyumlu varsayılan sürümü ile uyumlu olduğundan emin olun.
1. Web uygulamanızı dağıtın.
1. Web uygulamanızı Azure portalında için bulun ve tıklayın **ayarları** düğmesi.

    ![Web uygulaması ayarları][settings-button]
1. Gelen **ayarları** dikey penceresinde **uygulama ayarları** kaydırın **uygulama ayarları** bölümü.
1. İçinde **uygulama ayarları** bölümünde, oluşturun bir **php_extensıons** anahtarı. Bu anahtarın değeri, Web sitesinin kök göreli bir yol olabilir: **bin\your ext dosya**.

    ![Uygulama Ayarları'nda uzantısını etkinleştirme][php-extensions]
1. Tıklayın **Kaydet** üst kısmındaki düğmeye **Web uygulaması ayarları** dikey penceresi.

    ![Yapılandırma ayarlarını Kaydet][save-button]

Zend uzantıları, kullanarak de desteklenir bir **PHP_ZENDEXTENSIONS** anahtarı. Birden çok uzantıyı etkinleştirmek için virgülle ayrılmış listesini içeren `.dll` dosyaları uygulama ayarı değeri.

## <a name="how-to-use-a-custom-php-runtime"></a>Nasıl yapılır: özel bir PHP çalışma zamanı kullanın

Varsayılan PHP çalışma zamanı yerine App Service Web Apps, PHP komut yürütmek için sağlayan bir PHP çalışma zamanı kullanabilirsiniz. Sağladığınız çalışma zamanı tarafından yapılandırılabilir bir `php.ini` ayrıca sağlayan dosya. Aşağıdaki adımları izleyerek Web Apps ile özel bir PHP çalışma zamanı kullanmak için.

1. Bir iş parçacığı güvenli olmayan, PHP için Windows VC9 veya VC11 uyumlu sürümü edinin. PHP için Windows son sürümleri şurada bulunabilir: [ http://windows.php.net/download/ ]. Eski sürümleri arşiv burada bulunabilir: [ http://windows.php.net/downloads/releases/archives/ ].
1. Değiştirme `php.ini` dosya, çalışma zamanı için. Sistem-düzey-yalnızca yönergeleridir herhangi bir yapılandırma ayarı Web uygulamaları tarafından göz ardı edilir. (Sistem düzeyi-yalnızca yönergeleri hakkında daha fazla bilgi için bkz. [php.ini yönergeleri listesi]).
1. İsteğe bağlı olarak, PHP çalışma zamanı uzantılarını ekleyin ve bunları etkinleştirmek `php.ini` dosya.
1. Ekleme bir `bin` kök dizini ve put PHP çalışma zamanınızı da içeren dizine dizin (örneğin, `bin\php`).
1. Web uygulamanızı dağıtın.
1. Web uygulamanızı Azure portalında için bulun ve tıklayın **ayarları** düğmesi.

    ![Web uygulaması ayarları][settings-button]
1. Gelen **ayarları** dikey penceresinde **uygulama ayarları** kaydırın **işleyici eşlemeleri** bölümü. Ekleme `*.php` uzantı alan ve yola ekleyin `php-cgi.exe` yürütülebilir. PHP çalışma zamanınızın koyarsanız `bin` uygulamanızın kök dizininde yoludur `D:\home\site\wwwroot\bin\php\php-cgi.exe`.

    ![İşleyici eşlemeleri işleyici belirtin][handler-mappings]
1. Tıklayın **Kaydet** üst kısmındaki düğmeye **Web uygulaması ayarları** dikey penceresi.

    ![Yapılandırma ayarlarını Kaydet][save-button]

<a name="composer" />

## <a name="how-to-enable-composer-automation-in-azure"></a>Nasıl yapılır: azure'daki Composer otomasyonunu etkinleştirme

PHP projenizi varsa varsayılan olarak, App Service ile composer.json, hiçbir şey yapmıyor. Kullanırsanız [Git dağıtımını](app-service-deploy-local-git.md), işleme sırasında composer.json etkinleştirebilirsiniz `git push` Composer uzantısını etkinleştirerek.

> [!NOTE]
> Yapabilecekleriniz [birinci sınıf oluşturucusu desteği için App Service burada oy](https://feedback.azure.com/forums/169385-web-apps-formerly-websites/suggestions/6477437-first-class-support-for-composer-and-pip)!
>

1. Uygulama dikey penceresinde, PHP ile web [Azure portalında](https://portal.azure.com), tıklayın **Araçları** > **uzantıları**.

    ![Azure'da Composer otomasyonunu etkinleştirme için azure portal ayarları dikey penceresi](./media/web-sites-php-configure/composer-extension-settings.png)
2. Tıklayın **Ekle**, ardından **Oluşturucusu**.

    ![Azure'da Composer otomasyonunu etkinleştirme için Composer uzantısını ekleyin](./media/web-sites-php-configure/composer-extension-add.png)
3. Tıklayın **Tamam** yasal koşulları kabul etmek için. Tıklayın **Tamam** yeniden uzantısı eklemek için.

    **Yüklü uzantıları** Composer uzantısını dikey pencerede gösterilir.
    ![Azure'da Composer otomasyonunu etkinleştirme için yasal koşulları kabul edin](./media/web-sites-php-configure/composer-extension-view.png)
4. Yerel makinenizde bir terminal penceresinde, artık gerçekleştirmek `git add`, `git commit`, ve `git push` Web uygulamanıza. Composer composer.json içinde tanımlanan bağımlılıklar yüklüyor dikkat edin.

    ![Composer otomasyonunu azure'da Git dağıtımı](./media/web-sites-php-configure/composer-extension-success.png)

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi için [PHP Geliştirici Merkezi](/develop/php/).

> [!NOTE]
> Azure hesabı için kaydolmadan önce Azure App Service’i kullanmaya başlamak isterseniz, App Service’te hemen kısa süreli bir başlangıç web uygulaması oluşturabileceğiniz [App Service’i Deneyin](https://azure.microsoft.com/try/app-service/) sayfasına gidin. Kredi kartı ve taahhüt gerekmez.
>

[ücretsiz deneme sürümü]: https://www.windowsazure.com/pricing/free-trial/
[phpinfo()]: http://php.net/manual/en/function.phpinfo.php
[select-php-version]: ./media/web-sites-php-configure/select-php-version.png
[Php.ini yönergeleri listesi]: http://www.php.net/manual/en/ini.list.php
[. user.ini]: http://www.php.net/manual/en/configuration.file.per-user.php
[ini_set()]: http://www.php.net/manual/en/function.ini-set.php
[application-settings]: ./media/web-sites-php-configure/application-settings.png
[settings-button]: ./media/web-sites-php-configure/settings-button.png
[save-button]: ./media/web-sites-php-configure/save-button.png
[php-extensions]: ./media/web-sites-php-configure/php-extensions.png
[handler-mappings]: ./media/web-sites-php-configure/handler-mappings.png
[http://windows.php.net/download/]: http://windows.php.net/download/
[http://windows.php.net/downloads/releases/archives/]: http://windows.php.net/downloads/releases/archives/
[SETPHPVERCLI]: ./media/web-sites-php-configure/ChangePHPVersion-XPlatCLI.png
[GETPHPVERCLI]: ./media/web-sites-php-configure/ShowPHPVersion-XplatCLI.png
[SETPHPVERPS]: ./media/web-sites-php-configure/ChangePHPVersion-PS.png
[GETPHPVERPS]: ./media/web-sites-php-configure/ShowPHPVersion-PS.png
