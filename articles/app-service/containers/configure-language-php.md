---
title: PHP uygulamaları - Azure App Service'ı yapılandırma | Microsoft Docs
description: PHP uygulamalarını Azure App Service'te çalışacak şekilde yapılandırma hakkında bilgi edinin
services: app-service
documentationcenter: ''
author: cephalin
manager: jpconnock
editor: ''
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 03/28/2019
ms.author: cephalin
ms.openlocfilehash: 279660d903b3b0e893c3ccddb89da7c6dc42fa09
ms.sourcegitcommit: b7a44709a0f82974578126f25abee27399f0887f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2019
ms.locfileid: "67205068"
---
# <a name="configure-a-linux-php-app-for-azure-app-service"></a>Bir Linux PHP uygulamasını Azure App Service için yapılandırma

Bu kılavuzda, Azure App Service'te web uygulamaları, mobil arka uçlar ve API apps için yerleşik PHP çalışma zamanı yapılandırma işlemini göstermektedir.

Bu kılavuzu temel kavramları ve yerleşik bir Linux kapsayıcı kullanan App Service'te PHP geliştiricilerine yönelik yönergeler sağlar. Azure App Service daha önce kullanmadıysanız izleyin [PHP hızlı](quickstart-php.md) ve [öğretici MySQL ile PHP](tutorial-php-mysql-app.md) ilk.

## <a name="show-php-version"></a>PHP sürümünü Göster

Geçerli PHP sürümünü görüntülemek için aşağıdaki komutu çalıştırın [Cloud Shell](https://shell.azure.com):

```azurecli-interactive
az webapp config show --resource-group <resource-group-name> --name <app-name> --query linuxFxVersion
```

Desteklenen tüm PHP sürümleri göstermek için aşağıdaki komutu çalıştırın [Cloud Shell](https://shell.azure.com):

```azurecli-interactive
az webapp list-runtimes --linux | grep PHP
```

## <a name="set-php-version"></a>PHP sürümü Ayarla

Aşağıdaki komutu çalıştırın [Cloud Shell](https://shell.azure.com) 7.2 için PHP sürümünü ayarlamak için:

```azurecli-interactive
az webapp config set --name <app-name> --resource-group <resource-group-name> --linux-fx-version "PHP|7.2"
```

## <a name="run-composer"></a>Oluşturucusu çalıştırın

Varsayılan olarak, Kudu çalıştırmaz [Oluşturucusu](https://getcomposer.org/). Kudu dağıtım sırasında Composer otomasyonunu etkinleştirme için sağlamanız gereken bir [özel dağıtım betiği](https://github.com/projectkudu/kudu/wiki/Custom-Deployment-Script).

Yerel terminal penceresinden dizini depo kökünüzde değiştirin. İzleyin [komut satırı yükleme adımlarını](https://getcomposer.org/download/) indirmek için *composer.phar*.

Aşağıdaki komutları çalıştırın:

```bash
npm install kuduscript -g
kuduscript --php --scriptType bash --suppressPrompt
```

Depo kökünüzde artık iki yeni dosyalarına ek olarak sahip *composer.phar*: *.deployment* ve *deploy.sh*. Bu dosyalar Windows ve Linux özellikleri App Service'in için hem de çalışır.

Açık *deploy.sh* ve bulma `Deployment` bölümü. Tüm bölüm, aşağıdaki kodla değiştirin:

```bash
##################################################################################################################################
# Deployment
# ----------

echo PHP deployment

# 1. KuduSync
if [[ "$IN_PLACE_DEPLOYMENT" -ne "1" ]]; then
  "$KUDU_SYNC_CMD" -v 50 -f "$DEPLOYMENT_SOURCE" -t "$DEPLOYMENT_TARGET" -n "$NEXT_MANIFEST_PATH" -p "$PREVIOUS_MANIFEST_PATH" -i ".git;.hg;.deployment;deploy.sh"
  exitWithMessageOnError "Kudu Sync failed"
fi

# 3. Initialize Composer Config
initializeDeploymentConfig

# 4. Use composer
echo "$DEPLOYMENT_TARGET"
if [ -e "$DEPLOYMENT_TARGET/composer.json" ]; then
  echo "Found composer.json"
  pushd "$DEPLOYMENT_TARGET"
  php composer.phar install $COMPOSER_ARGS
  exitWithMessageOnError "Composer install failed"
  popd
fi
##################################################################################################################################
```

Tüm değişikliklerinizi işleyin ve kodunuzu yeniden dağıtın. Oluşturucusu şimdi dağıtım otomasyonunu bir parçası olarak çalıştırıyor olmalıdır.

## <a name="customize-start-up"></a>Başlatmayı özelleştirme

Yerleşik PHP kapsayıcı varsayılan olarak, Apache server çalıştırın. Başlatma sırasında çalıştığı `apache2ctl -D FOREGROUND"`. İsterseniz, başka bir komut başlangıç sırasında aşağıdaki komutu çalıştırarak çalıştırabileceğiniz [Cloud Shell](https://shell.azure.com):

```azurecli-interactive
az webapp config set --resource-group <resource-group-name> --name <app-name> --startup-file "<custom-command>"
```

## <a name="access-environment-variables"></a>Ortam değişkenlerine erişme

Uygulama hizmetinde [uygulama ayarlarını belirlemek](../configure-common.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json#configure-app-settings) , uygulama kodunuz dışında. Standardını kullanarak bunlar erişebilir [getenv()](https://secure.php.net/manual/function.getenv.php) deseni. Örneğin, bir uygulama ayarı erişmeye adlı `DB_HOST`, aşağıdaki kodu kullanın:

```php
getenv("DB_HOST")
```

## <a name="change-site-root"></a>Bir site kökünde değiştirme

Tercih ettiğiniz bir web çerçevesi bir alt site kökü olarak kullanabilir. Örneğin, [Laravel](https://laravel.com/), kullandığı `public/` alt site kökü.

App Service için varsayılan PHP görüntüsü, Apache kullanır ve uygulamanız için bir site kökünde özelleştirmenize izin vermez. Bu sınırlara yakın çalışmak için ekleme bir *.htaccess* depo kökünüzde aşağıdaki içeriğe sahip bir dosyaya:

```
<IfModule mod_rewrite.c>
    RewriteEngine on

    RewriteRule ^.*$ /public/$1 [NC,L,QSA]
</IfModule>
```

*.htaccess* yeniden yazma işlemini kullanmazsanız, Laravel uygulamanızı bunun yerine bir [özel Docker görüntüsü](quickstart-docker-go.md) ile dağıtabilirsiniz.

## <a name="detect-https-session"></a>HTTPS oturumu algılayın

App Service'te [SSL sonlandırma](https://wikipedia.org/wiki/TLS_termination_proxy) tüm HTTPS isteklerini, şifrelenmemiş HTTP istekleri olarak uygulamanızı ulaşmak için Ağ Yük Dengeleyiciler, olur. Uygulama mantığı ihtiyaçlarınızı veya değil, kullanıcı isteklerini şifreli olup olmadığı denetlenecek incelemek, `X-Forwarded-Proto` başlığı.

```php
if (isset($_SERVER['X-Forwarded-Proto']) && $_SERVER['X-Forwarded-Proto'] === 'https') {
  // Do something when HTTPS is used
}
```

Popüler web çerçeveleri erişmenizi `X-Forwarded-*` bilgileri, standart uygulama deseni. İçinde [CodeIgniter](https://codeigniter.com/), [is_https()](https://github.com/bcit-ci/CodeIgniter/blob/master/system/core/Common.php#L338-L365) değerini denetler `X_FORWARDED_PROTO` varsayılan olarak.

## <a name="customize-phpini-settings"></a>PHP.ini ayarlarını özelleştirme

PHP yüklemenizi değişiklikler yapmanız gerekirse, herhangi birini değiştirebilirsiniz [php.ini yönergeleri](https://www.php.net/manual/ini.list.php) aşağıdaki adımları izleyerek.

> [!NOTE]
> PHP sürümünü ve geçerli görmek için en iyi yolu *php.ini* yapılandırmadır çağrılacak [phpinfo()](https://php.net/manual/function.phpinfo.php) uygulamanızda.
>

### <a name="Customize-non-PHP_INI_SYSTEM directives"></a>Özelleştirme-olmayan-PHP_INI_SYSTEM yönergeleri

PHP_INI_USER PHP_INI_PERDIR ve PHP_INI_ALL yönergeleri özelleştirmek için (bkz [php.ini yönergeleri](https://www.php.net/manual/ini.list.php)), ekleme bir *.htaccess* uygulamanızın kök dizinine dosya.

İçinde *.htaccess* kullanma yönergelerini ekleyin `php_value <directive-name> <value>` söz dizimi. Örneğin:

```
php_value upload_max_filesize 1000M
php_value post_max_size 2000M
php_value memory_limit 3000M
php_value max_execution_time 180
php_value max_input_time 180
php_value display_errors On
php_value upload_max_filesize 10M
```

Uygulamanızı değişikliklerle yeniden dağıtın ve yeniden başlatın. Kudu ile dağıtma varsa (örneğin, [Git](../deploy-local-git.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json)), dağıtımdan sonra otomatik olarak yeniden başlatılır.

Kullanmaya alternatif olarak *.htaccess*, kullanabileceğiniz [ini_set()](https://www.php.net/manual/function.ini-set.php) uygulamanızda bu PHP_INI_SYSTEM olmayan yönergeleri özelleştirebilirsiniz.

### <a name="customize-php_ini_system-directives"></a>PHP_INI_SYSTEM yönergeleri özelleştirme

PHP_INI_SYSTEM yönergeleri özelleştirmek için (bkz [php.ini yönergeleri](https://www.php.net/manual/ini.list.php)), kullanamazsınız *.htaccess* yaklaşım. App Service kullanarak ayrı bir mekanizma sağlar `PHP_INI_SCAN_DIR` uygulama ayarı.

İlk olarak, aşağıdaki komutu çalıştırın [Cloud Shell](https://shell.azure.com) çağrılan ayarlama uygulama ekleme `PHP_INI_SCAN_DIR`:

```azurecli-interactive
az webapp config appsettings set --name <app-name> --resource-group <resource-group-name> --settings PHP_INI_SCAN_DIR="/usr/local/etc/php/conf.d:/home/site/ini"
```

`/usr/local/etc/php/conf.d` Varsayılan dizin, burada *php.ini* bulunmaktadır. `/home/site/ini` hangi eklediğiniz özel bir özel dizin *.ini* dosya. Değerlerle ayrı bir `:`.

Linux kapsayıcınızı web SSH oturumundan gidin (`https://cephalin-container.scm.azurewebsites.net/webssh/host`).

Bir dizin oluşturma `/home/site` adlı `ini`, oluşturup bir *.ini* dosyası `/home/site/ini` dizin (örneğin, *settings.ini)* özelleştirmek istediğiniz yönergeleri ile. İçinde kullandığınız aynı sözdizimini kullanan bir *php.ini* dosya. 

> [!TIP]
> App Service'te yerleşik Linux kapsayıcılarında */home* kalıcı paylaşılan depolama alanı olarak kullanılır. 
>

Örneğin, değerini değiştirmek için [expose_php](https://php.net/manual/ini.core.php#ini.expose-php) aşağıdaki komutları çalıştırın:

```bash
cd /home/site
mkdir ini
echo "expose_php = Off" >> ini/setting.ini
```

Değişikliklerin etkili olması için uygulamayı yeniden başlatın.

## <a name="enable-php-extensions"></a>PHP uzantılarını etkinleştir

Yerleşik PHP yüklemeleri en yaygın olarak kullanılan uzantıları içerir. Ek uzantı aynı etkinleştirebilirsiniz şekilde [php.ini yönergeleri özelleştirme](#customize-php_ini_system-directives).

> [!NOTE]
> PHP sürümünü ve geçerli görmek için en iyi yolu *php.ini* yapılandırmadır çağrılacak [phpinfo()](https://php.net/manual/function.phpinfo.php) uygulamanızda.
>

Aşağıdaki adımları izleyerek ek uzantıları etkinleştirmek için:

Ekleme bir `bin` uygulama ve put kök dizininin dizin `.so` uzantısı dosyalarında (örneğin, *mongodb.so*). Uzantıları Azure olan VC9 ve iş parçacığı güvenli olmayan (nts) uyumlu PHP sürümüyle uyumlu olduğundan emin olun.

Yaptığınız değişiklikleri dağıtın.

Bağlantısındaki [PHP_INI_SYSTEM özelleştirme yönergeleri](#customize-php_ini_system-directives), özel uzantıları ekleyin *.ini* ile dosya [uzantısı](https://www.php.net/manual/ini.core.php#ini.extension) veya [zend_extension ](https://www.php.net/manual/ini.core.php#ini.zend-extension) yönergeleri.

```ini
extension=/home/site/wwwroot/bin/mongodb.so
zend_extension=/home/site/wwwroot/bin/xdebug.so
```

Değişikliklerin etkili olması için uygulamayı yeniden başlatın.

## <a name="access-diagnostic-logs"></a>Tanılama günlüklerine erişim

[!INCLUDE [Access diagnostic logs](../../../includes/app-service-web-logs-access-no-h.md)]

## <a name="open-ssh-session-in-browser"></a>Tarayıcıda SSH oturum aç

[!INCLUDE [Open SSH session in browser](../../../includes/app-service-web-ssh-connect-builtin-no-h.md)]

## <a name="troubleshooting"></a>Sorun giderme

Çalışan PHP uygulaması App Service'te farklı davranır ya da hatalı aşağıdakileri deneyin:

- [Günlük akışı erişim](#access-diagnostic-logs).
- Uygulamayı yerel olarak üretim modunda test edin. App Service, Node.js uygulamaları üretim modunda çalışır ve böylece projeniz yerel olarak üretim modunda beklendiği gibi çalıştığından emin olmanız gerekir. Örneğin:
    - Yapılandırmanıza bağlı olarak, *composer.json*, farklı paketleri üretim modu için yüklü (`require` karşılaştırması `require-dev`).
    - Bazı web çerçeveleri, statik dosyalar farklı üretim modunda dağıtabilirsiniz.
    - Bazı web çerçeveleri, üretim modunda çalışırken özel başlatma komut dosyaları kullanabilirsiniz.
- Uygulamanızı App Service'te hata ayıklama modunda çalıştırın. Örneğin, [Laravel](https://meanjs.org/), uygulamanız tarafından üretimde hata ayıklama iletilerinin de çıkışını almak yapılandırabileceğiniz [ayarı `APP_DEBUG` uygulama ayarının `true` ](../configure-common.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json#configure-app-settings).

### <a name="robots933456"></a>robots933456

Kapsayıcı günlüklerini, şu iletiyi görebilirsiniz:

```
2019-04-08T14:07:56.641002476Z "-" - - [08/Apr/2019:14:07:56 +0000] "GET /robots933456.txt HTTP/1.1" 404 415 "-" "-"
```

Bu iletiyi güvenle yok sayabilirsiniz. `/robots933456.txt` App Service, kapsayıcı hizmet istekleri olup olmadığını denetlemek için kullandığı bir işlevsiz URL yoludur. 404 yanıtı basit yol mevcut, ancak App Service, kapsayıcı sağlıklı ve isteklere yanıt vermeye hazır olduğunu bilmesini sağlar gösterir.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Öğretici: MySQL ile PHP uygulaması](tutorial-php-mysql-app.md)

> [!div class="nextstepaction"]
> [App Service Linux SSS](app-service-linux-faq.md)
