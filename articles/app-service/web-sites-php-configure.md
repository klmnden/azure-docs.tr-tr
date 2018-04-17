---
title: Azure App Service Web Apps PHP yapılandırma | Microsoft Docs
description: Varsayılan PHP yüklemesini yapılandırmak veya Azure App Service'deki Web uygulamaları için özel bir PHP yükleme nasıl ekleneceğini öğrenin.
services: app-service
documentationcenter: php
author: cephalin
manager: cfowler
editor: ''
ms.assetid: 95c4072b-8570-496b-9c48-ee21a223fb60
ms.service: app-service
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: PHP
ms.topic: article
ms.date: 04/25/2017
ms.author: cephalin
ms.openlocfilehash: a9059be43a6188b81b57f1503bba241b3138e114
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
---
# <a name="configure-php-in-azure-app-service-web-apps"></a>Azure App Service Web Apps PHP yapılandırma

## <a name="introduction"></a>Giriş

Bu kılavuz, Web uygulamaları için yerleşik PHP çalışma zamanı yapılandırma göstermektedir [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714), özel bir PHP çalışma zamanı sağlamak ve uzantılarını etkinleştirme. Uygulama hizmeti kullanmak için kaydolun [ücretsiz deneme sürümü]. Bu Rehberde en almak için bir PHP web uygulaması App Service'te oluşturmanız gerekir.

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="how-to-change-the-built-in-php-version"></a>Nasıl yapılır: yerleşik PHP sürümünü değiştirme

Bir App Service web uygulaması oluşturduğunuzda varsayılan olarak, PHP 5.6 yüklü ve kullanmak için hemen kullanılabilir durumdadır. Kullanılabilir sürüm düzeltme, varsayılan yapılandırması ve etkin uzantıları görmek için en iyi yolu çağıran bir komut dosyası dağıtmaktır [phpinfo()] işlevi.

PHP 7.0 ve PHP 7.2 sürümleri de kullanılabilir, ancak varsayılan olarak etkin değildir. PHP sürümünü güncelleştirmek için aşağıdaki yöntemlerden birini izleyin:

### <a name="azure-portal"></a>Azure portalına

1. Web uygulamanız için Gözat [Azure portal](https://portal.azure.com) ve tıklayın **ayarları** düğmesi.

    ![Web uygulaması ayarları][settings-button]
1. Gelen **ayarları** dikey penceresinde, select **uygulama ayarları** ve yeni PHP sürümünü seçin.

    ![Uygulama Ayarları][application-settings]
1. Tıklatın **kaydetmek** en üstündeki düğmesi **Web uygulaması ayarları** dikey.

    ![Yapılandırma ayarlarını Kaydet][save-button]

### <a name="azure-powershell-windows"></a>Azure PowerShell'i (Windows)

1. Açık Azure PowerShell ve hesabınızda oturum açma:

        PS C:\> Login-AzureRmAccount
1. Web uygulaması için PHP sürümünü ayarlayın.

        PS C:\> Set-AzureWebsite -PhpVersion {5.6 | 7.0 | 7.2} -Name {app-name}
1. PHP sürümünü şimdi ayarlayın. Bu ayarları doğrulayabilirsiniz:

        PS C:\> Get-AzureWebsite -Name {app-name} | findstr PhpVersion

### <a name="azure-cli-20-linux-mac-windows"></a>Azure CLI 2.0 (Linux, Mac, Windows)

Azure komut satırı arabirimi kullanmak için şunları yapmalısınız [yükleme Azure CLI 2.0](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli?view=azure-cli-latest) bilgisayarınızda.

1. Açık Terminal ve hesabınızda oturum açın.

        az login

1. Desteklenen çalışma zamanları listesini görmek için kontrol edin.

        az webapp list-runtimes | grep php

1. Web uygulaması için PHP sürümünü ayarlayın.

        az webapp config set --php-version {5.6 | 7.0 | 7.1 | 7.2} --name {app-name} --resource-group {resource-group-name}

1. PHP sürümünü şimdi ayarlayın. Bu ayarları doğrulayabilirsiniz:

        az webapp show --name {app-name} --resource-group {resource-group-name}

## <a name="how-to-change-the-built-in-php-configurations"></a>Nasıl yapılır: yerleşik PHP yapılandırmalarını değiştirme

Tüm yerleşik PHP çalışma zamanı için yapılandırma seçeneklerinin aşağıdaki adımları izleyerek değiştirebilirsiniz. (Php.ini yönergeleri hakkında daha fazla bilgi için bkz: [php.ini yönergeleri listesi].)

### <a name="changing-phpiniuser-phpiniperdir-phpiniall-configuration-settings"></a>PHP değiştirme\_ını\_kullanıcı, PHP\_ını\_PERDIR, PHP\_ını\_tüm yapılandırma ayarları

1. Ekleme bir [. user.ini] kök dizinine dosya.
1. Yapılandırma ayarlarını eklemek `.user.ini` içinde kullandığınız aynı sözdizimini kullanarak dosya bir `php.ini` dosyası. Örneğin, etkinleştirmek istiyorsanız `display_errors` ayarı ve ayarlanmış `upload_max_filesize` 10 M için ayarlama, `.user.ini` dosya bu metin içerebilir:

        ; Example Settings
        display_errors=On
        upload_max_filesize=10M

        ; OPTIONAL: Turn this on to write errors to d:\home\LogFiles\php_errors.log
        ; log_errors=On
1. Web uygulamanızı dağıtın.
1. Web uygulaması yeniden başlatın. (Yeniden başlatma gereklidir çünkü hangi PHP sıklığı okur `.user.ini` dosyaları tarafından yönetilir `user_ini.cache_ttl` sistem düzeyi ayarı ve 300 saniye (5 dakika) varsayılan ayarı. Web uygulaması yeniden zorlar yeni ayarlarında okumak için PHP `.user.ini` dosyası.)

Kullanmaya alternatif olarak bir `.user.ini` kullanabileceğiniz dosyası [ini_set()] , sistem düzeyinde yönergeleri olmayan yapılandırma seçenekleri ayarlamak için komut dosyalarında işlevi.

### <a name="changing-phpinisystem-configuration-settings"></a>PHP değiştirme\_ını\_sistemi yapılandırma ayarları

1. Bir uygulama ayarı, Web uygulamanızın anahtarla eklemek `PHP_INI_SCAN_DIR` ve değeri `d:\home\site\ini`
1. Oluşturma bir `settings.ini` Kudu konsolunu kullanarak dosya (http://&lt;site adı&gt;. scm.azurewebsite.net) içinde `d:\home\site\ini` dizin.
1. Yapılandırma ayarlarını eklemek `settings.ini` php.ini dosyasında kullandığınız aynı sözdizimini kullanarak dosya. Örneğin işaret edecek şekilde istediyseniz `curl.cainfo` ayarını bir `*.crt` dosya ve 512 K 'wincache.maxfilesize' ayarı, `settings.ini` bu metin dosyasını içerebilir:

        ; Example Settings
        curl.cainfo="%ProgramFiles(x86)%\Git\bin\curl-ca-bundle.crt"
        wincache.maxfilesize=512
1. Değişiklikleri yeniden yüklemek için Web uygulamanızı yeniden başlatın.

## <a name="how-to-enable-extensions-in-the-default-php-runtime"></a>Nasıl yapılır: varsayılan PHP çalışma zamanında eklentilerini etkinleştir

Önceki bölümde belirtildiği gibi varsayılan PHP sürümü, varsayılan yapılandırması ve etkin uzantıları görmek için en iyi yolu çağıran bir komut dosyası dağıtmaktır [phpinfo()]. Aşağıdaki adımları izleyerek ek uzantıları etkinleştirmek için:

### <a name="configure-via-ini-settings"></a>Aracılığıyla INI ayarlarını yapılandırma

1. Ekleme bir `ext` dizininden `d:\home\site` dizin.
1. PUT `.dll` dosyalar uzantısı `ext` dizin (örneğin, `php_xdebug.dll`). Uzantılar, PHP, olan VC9 ve iş parçacığı güvenli olmayan (nts) uyumlu varsayılan sürümü ile uyumlu olduğundan emin olun.
1. Bir uygulama ayarı, Web uygulamanızın anahtarla eklemek `PHP_INI_SCAN_DIR` ve değeri `d:\home\site\ini`
1. Oluşturma bir `ini` dosyasını `d:\home\site\ini` adlı `extensions.ini`.
1. Yapılandırma ayarlarını eklemek `extensions.ini` php.ini dosyasında kullandığınız aynı sözdizimini kullanarak dosya. Örneğin, MongoDB ve XDebug uzantıları etkinleştirmek istiyorsanız, `extensions.ini` bu metin dosyasını içerebilir:

        ; Enable Extensions
        extension=d:\home\site\ext\php_mongo.dll
        zend_extension=d:\home\site\ext\php_xdebug.dll
1. Değişiklikleri yüklemek için Web uygulamanızı yeniden başlatın.

### <a name="configure-via-app-setting"></a>Uygulama ayarı aracılığıyla yapılandırın

1. Ekleme bir `bin` kök dizin için dizin.
1. PUT `.dll` dosyalar uzantısı `bin` dizin (örneğin, `php_xdebug.dll`). Uzantılar, PHP, olan VC9 ve iş parçacığı güvenli olmayan (nts) uyumlu varsayılan sürümü ile uyumlu olduğundan emin olun.
1. Web uygulamanızı dağıtın.
1. Web uygulamanızı Azure portalında bulun ve tıklayın **ayarları** düğmesi.

    ![Web uygulaması ayarları][settings-button]
1. Gelen **ayarları** dikey penceresinde, select **uygulama ayarları** ve kaydırma **uygulama ayarları** bölümü.
1. İçinde **uygulama ayarları** bölümünde, oluşturmak bir **php_extensıons** anahtarı. Bu anahtarın değeri, Web sitesi kök göreli bir yol olacaktır: **bin\your ext dosya**.

    ![Uygulama ayarları uzantı etkinleştirilemedi][php-extensions]
1. Tıklatın **kaydetmek** en üstündeki düğmesi **Web uygulaması ayarları** dikey.

    ![Yapılandırma ayarlarını Kaydet][save-button]

Zend uzantıları de desteklenir kullanarak bir **PHP_ZENDEXTENSIONS** anahtarı. Birden çok uzantıları etkinleştirmek için virgülle ayrılmış listesini içeren `.dll` dosyaları uygulama ayarının değeri.

## <a name="how-to-use-a-custom-php-runtime"></a>Nasıl yapılır: özel bir PHP çalışma zamanı kullanın

Varsayılan PHP çalışma zamanı yerine App Service Web Apps, PHP komut yürütmek için sağlayan bir PHP çalışma zamanı kullanabilirsiniz. Sağladığınız çalışma zamanı tarafından yapılandırılabilir bir `php.ini` de belirtmeniz dosya. Aşağıdaki adımları Web Apps ile özel bir PHP çalışma zamanı kullanmak için.

1. Bir iş parçacığı güvenli, VC9 veya VC11 uyumlu PHP için Windows sürümü edinin. Windows için PHP'nin en son sürümleri şurada bulunabilir: [ http://windows.php.net/download/ ]. Eski sürümleri arşive burada bulunabilir: [ http://windows.php.net/downloads/releases/archives/ ].
1. Değiştirme `php.ini` dosya, çalışma zamanı için. Sistem düzeyinde-yalnızca yönergeleri yapılandırma ayarları, Web uygulamaları tarafından göz ardı edilir. (Sistem düzeyinde-yalnızca yönergeleri hakkında daha fazla bilgi için bkz: [php.ini yönergeleri listesi]).
1. İsteğe bağlı olarak, PHP çalışma zamanına uzantıları ekleyin ve bunları etkinleştirmek `php.ini` dosya.
1. Ekleme bir `bin` kök dizini ve put, PHP çalışma zamanı da içeren dizin için dizin (örneğin, `bin\php`).
1. Web uygulamanızı dağıtın.
1. Web uygulamanızı Azure portalında bulun ve tıklayın **ayarları** düğmesi.

    ![Web uygulaması ayarları][settings-button]
1. Gelen **ayarları** dikey penceresinde, select **uygulama ayarları** ve kaydırma **işleyici eşlemeleri** bölümü. Ekleme `*.php` uzantısı alan ve yoluna ekleyin `php-cgi.exe` yürütülebilir. PHP çalışma zamanı yerleştirirseniz `bin` yolun, uygulamanızın kök dizininde olduğundan `D:\home\site\wwwroot\bin\php\php-cgi.exe`.

    ![İşleyici eşlemeleri işleyici belirtin][handler-mappings]
1. Tıklatın **kaydetmek** en üstündeki düğmesi **Web uygulaması ayarları** dikey.

    ![Yapılandırma ayarlarını Kaydet][save-button]

<a name="composer" />

## <a name="how-to-enable-composer-automation-in-azure"></a>Nasıl yapılır: Azure içinde Oluşturucu Otomasyonu

PHP projeniz varsa varsayılan olarak, uygulama hizmeti ile composer.json, hiçbir şey yapmaz. Kullanırsanız [Git dağıtımı](app-service-deploy-local-git.md), sırasında işleme composer.json etkinleştirebilirsiniz `git push` Composer uzantısını etkinleştirerek.

> [!NOTE]
> Yapabilecekleriniz [birinci sınıf oluşturucu desteği için uygulama hizmeti burada oy](https://feedback.azure.com/forums/169385-web-apps-formerly-websites/suggestions/6477437-first-class-support-for-composer-and-pip)!
>

1. Uygulamanızın dikey penceresinde, PHP ile web [Azure portal](https://portal.azure.com), tıklatın **Araçları** > **uzantıları**.

    ![Azure Otomasyonu'nda Oluşturucu etkinleştirmek için azure portal ayarlar dikey penceresi](./media/web-sites-php-configure/composer-extension-settings.png)
2. Tıklatın **Ekle**, ardından **Oluşturucu**.

    ![Azure Otomasyonu'nda Oluşturucu etkinleştirmek için Composer uzantısını Ekle](./media/web-sites-php-configure/composer-extension-add.png)
3. Tıklatın **Tamam** yasal koşulları kabul etmek için. Tıklatın **Tamam** uzantısı yeniden ekleyin.

    **Yüklü uzantıları** dikey Composer uzantısını gösterir.
    ![Azure Otomasyonu'nda Oluşturucu etkinleştirmek için yasal koşulları kabul](./media/web-sites-php-configure/composer-extension-view.png)
4. Şimdi, yerel makinenizde bir terminal penceresi içinde gerçekleştirmek `git add`, `git commit`, ve `git push` Web uygulamanız için. Oluşturucu composer.json içinde tanımlanan bağımlılıklar yüklüyor dikkat edin.

    ![Azure Otomasyonu'nda Oluşturucusu ile Git dağıtımı](./media/web-sites-php-configure/composer-extension-success.png)

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi için bkz: [PHP Geliştirici Merkezi](/develop/php/).

> [!NOTE]
> Azure hesabı için kaydolmadan önce Azure App Service’i kullanmaya başlamak isterseniz, App Service’te hemen kısa süreli bir başlangıç web uygulaması oluşturabileceğiniz [App Service’i Deneyin](https://azure.microsoft.com/try/app-service/) sayfasına gidin. Kredi kartı ve taahhüt gerekmez.
>

[ücretsiz deneme sürümü]: https://www.windowsazure.com/pricing/free-trial/
[phpinfo()]: http://php.net/manual/en/function.phpinfo.php
[select-php-version]: ./media/web-sites-php-configure/select-php-version.png
[php.ini yönergeleri listesi]: http://www.php.net/manual/en/ini.list.php
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