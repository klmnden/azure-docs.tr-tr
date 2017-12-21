---
title: "Azure'da PHP ve MySQL bir web uygulaması oluşturma | Microsoft Docs"
description: "Azure MySQL veritabanında bağlantısı olan Azure üzerinde çalışan bir PHP uygulaması alma hakkında bilgi."
services: app-service\web
documentationcenter: nodejs
author: cephalin
manager: erikre
editor: 
ms.assetid: 14feb4f3-5095-496e-9a40-690e1414bd73
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: tutorial
ms.date: 10/20/2017
ms.author: cephalin
ms.custom: mvc
ms.openlocfilehash: be3fba0c148cc7ee07fa3d50e0783916790e7cab
ms.sourcegitcommit: 3fca41d1c978d4b9165666bb2a9a1fe2a13aabb6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/15/2017
---
# <a name="build-a-php-and-mysql-web-app-in-azure"></a>Azure'da PHP ve MySQL bir web uygulaması oluşturma

> [!NOTE]
> Bu makalede Windows App Service'e bir uygulama dağıtır. Uygulama hizmeti dağıtım _Linux_, bakın [Linux üzerinde bir PHP ve MySQL web uygulamasını Azure App Service'te yapı](./containers/tutorial-php-mysql-app.md).
>

[Azure Web Apps](app-service-web-overview.md) yüksek oranda ölçeklenebilen, kendi kendine düzeltme eki uygulayan bir web barındırma hizmeti sunar. Bu öğretici, Azure üzerinde bir PHP web uygulaması oluşturma ve MySQL veritabanına bağlanmak gösterilmiştir. İşiniz bittiğinde, gerekir bir [Laravel](https://laravel.com/) Azure App Service Web Apps üzerinde çalışan uygulama.

![Azure uygulama Hizmeti'nde çalışan PHP uygulaması](./media/app-service-web-tutorial-php-mysql/complete-checkbox-published.png)

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Azure üzerinde MySQL veritabanı oluşturma
> * MySQL için PHP uygulamaya Bağlan
> * Uygulamayı Azure'a dağıtma
> * Veri modeli güncelleştirme ve uygulamayı yeniden dağıtın
> * Azure Stream tanılama günlükleri
> * Azure portalında uygulama yönetme

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamak için:

* [Git'i yükleyin](https://git-scm.com/)
* [PHP 5.6.4 yükleme veya üstü](http://php.net/downloads.php)
* [Oluşturucu yükleyin](https://getcomposer.org/doc/00-intro.md)
* Aşağıdaki PHP uzantılarını Laravel gereksinimlerini etkinleştirme: OpenSSL, PDO MySQL, Mbstring, belirteç Oluşturucu, XML
* [Yükleyin ve MySQL başlatın](https://dev.mysql.com/doc/refman/5.7/en/installing.html) 

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prepare-local-mysql"></a>Yerel MySQL hazırlama

Bu adımda, bir veritabanı yerel MySQL sunucunuzu Bu öğreticide, kullanımınız için oluşturun.

### <a name="connect-to-local-mysql-server"></a>Yerel MySQL sunucuya bağlanın

Bir terminal penceresi, yerel MySQL sunucusuna bağlanın. Bu öğreticide tüm komutları çalıştırmak için bu bir terminal penceresi kullanabilirsiniz.

```bash
mysql -u root -p
```

İçin bir parola istenirse parolayı girin `root` hesabı. Kök hesap parolanızı hatırlamıyorsanız bkz [MySQL: kök parolasını sıfırlamak nasıl](https://dev.mysql.com/doc/refman/5.7/en/resetting-permissions.html).

Komutunuzu başarıyla çalışırsa, MySQL sunucunuzu çalışıyor. Aksi takdirde, yerel MySQL server'ınızdaki izleyerek başlatıldığından emin olun [MySQL yükleme sonrası adımları](https://dev.mysql.com/doc/refman/5.7/en/postinstallation.html).

### <a name="create-a-database-locally"></a>Yerel bir veritabanı oluşturun

Konumundaki `mysql` isteminde, bir veritabanı oluşturun.

```sql 
CREATE DATABASE sampledb;
```

Sunucu bağlantısı yazarak çıkmak `quit`.

```sql
quit
```

<a name="step2"></a>

## <a name="create-a-php-app-locally"></a>Yerel olarak bir PHP uygulaması oluşturma
Bu adımda, Laravel örnek bir uygulama almak, veritabanı bağlantısını yapılandırın ve yerel olarak çalıştırın. 

### <a name="clone-the-sample"></a>Örnek kopyalama

Terminal penceresinde `cd` bir çalışma dizini için.

Örnek depoyu kopyalamak için aşağıdaki komutu çalıştırın.

```bash
git clone https://github.com/Azure-Samples/laravel-tasks
```

`cd`Kopyalanan dizininize.
Gerekli paketleri yükleyeceksiniz.

```bash
cd laravel-tasks
composer install
```

### <a name="configure-mysql-connection"></a>MySQL bağlantısını yapılandırın

Depo kök dizininde adlı bir metin dosyası oluşturun *.env*. Aşağıdaki değişkenler içine kopyalamak *.env* dosya. Değiştir  _&lt;root_password >_ yer tutucusunu MySQL kök kullanıcının parolası ile.

```
APP_ENV=local
APP_DEBUG=true
APP_KEY=

DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_DATABASE=sampledb
DB_USERNAME=root
DB_PASSWORD=<root_password>
```

Laravel nasıl kullandığı hakkında bilgi için _.env_ dosya için bkz: [Laravel ortamı Yapılandırması](https://laravel.com/docs/5.4/configuration#environment-configuration).

### <a name="run-the-sample-locally"></a>Örnek yerel olarak çalıştırma

Çalıştırma [Laravel veritabanı geçişler](https://laravel.com/docs/5.4/migrations) tabloları uygulama oluşturması gerekir. Hangi tablolar geçişleri oluşturulur görmek için konum _veritabanı/geçişler_ Git deposunda dizin.

```bash
php artisan migrate
```

Yeni bir Laravel uygulama anahtarı oluşturur.

```bash
php artisan key:generate
```

Uygulamayı çalıştırın.

```bash
php artisan serve
```

Bir tarayıcıda `http://localhost:8000` sayfasına gidin. Bazı görevler sayfasında ekleyin.

![MySQL için PHP başarıyla bağlandığını](./media/app-service-web-tutorial-php-mysql/mysql-connect-success.png)

PHP sunucusu durdurmak için şunu yazın `Ctrl + C` Terminal.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="create-mysql-in-azure"></a>MySQL oluşturma

Bu adımda oluşturduğunuz MySQL veritabanında [Azure veritabanı için MySQL (Önizleme)](/azure/mysql). Daha sonra bu veritabanına bağlanmak için PHP uygulaması yapılandırın.

### <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

[!INCLUDE [Create resource group](../../includes/app-service-web-create-resource-group-no-h.md)] 

### <a name="create-a-mysql-server"></a>MySQL sunucusu oluşturun

Bulut Kabuğu'nda Azure veritabanındaki bir sunucu ile MySQL (Önizleme) oluşturmak [az mysql sunucusu oluşturun](/cli/azure/mysql/server?view=azure-cli-latest#az_mysql_server_create) komutu.

Aşağıdaki komutta, gördüğünüz MySQL server adınızı alternatif  _&lt;mysql_server_name >_ yer tutucu (geçerli karakterler `a-z`, `0-9`, ve `-`). Bu ad MySQL sunucunun ana bilgisayar adı bir parçasıdır (`<mysql_server_name>.database.windows.net`), genel olarak benzersiz olması gerekir.

```azurecli-interactive
az mysql server create --name <mysql_server_name> --resource-group myResourceGroup --location "North Europe" --admin-user adminuser --admin-password MySQLAzure2017
```

> [!NOTE]
> Karışıklığı önlemek için Bu öğreticide, dikkat etmeniz gereken birkaç kimlik olduğundan `--admin-user` ve `--admin-password` kukla değerlere ayarlanır. Bir üretim ortamında bir iyi kullanıcı adı ve parola MySQL sunucunuz için Azure'da seçerken en iyi güvenlik uygulamalarını izleyin.
>
>

MySQL sunucusu oluşturulduğunda, Azure CLI bilgileri aşağıdaki örneğe benzer şekilde gösterir:

```json
{
  "administratorLogin": "adminuser",
  "administratorLoginPassword": null,
  "fullyQualifiedDomainName": "<mysql_server_name>.database.windows.net",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.DBforMySQL/servers/<mysql_server_name>",
  "location": "northeurope",
  "name": "<mysql_server_name>",
  "resourceGroup": "myResourceGroup",
  ...
}
```

### <a name="configure-server-firewall"></a>Sunucu Güvenlik Duvarı'nı yapılandırma

Bulut Kabuğu'nda kullanarak istemci bağlantılarına izin verecek şekilde MySQL sunucunuz için bir güvenlik duvarı kuralı oluşturmak [az mysql server güvenlik duvarı kuralı oluşturmak](/cli/azure/mysql/server/firewall-rule?view=azure-cli-latest#az_mysql_server_firewall_rule_create) komutu.

```azurecli-interactive
az mysql server firewall-rule create --name allIPs --server <mysql_server_name> --resource-group myResourceGroup --start-ip-address 0.0.0.0 --end-ip-address 255.255.255.255
```

> [!NOTE]
> Azure veritabanı için MySQL (Önizleme) şu anda yalnızca Azure hizmetlerine bağlantı sayısı sınırı değil. Dinamik olarak atanmış IP adresleri azure'da gibi tüm IP adresleri etkinleştirmek daha iyidir. Hizmet önizlemede değil. Veritabanınızın güvenliğini sağlamak için daha iyi yöntemleri aşağıda verilmiştir.
>
>

### <a name="connect-to-production-mysql-server-locally"></a>Yerel olarak üretim MySQL sunucusuna bağlan

Yerel terminal penceresinde Azure MySQL sunucusuna bağlanın. Daha önce için belirttiğiniz değerini kullanmak  _&lt;mysql_server_name >_. Kullanmak için bir parola istendiğinde, _MySQLAzure2017_, Azure'da veritabanı oluşturduğunuzda belirttiğiniz.

```bash
mysql -u adminuser@<mysql_server_name> -h <mysql_server_name>.database.windows.net -P 3306 -p
```

### <a name="create-a-production-database"></a>Bir üretim veritabanı oluşturma

Konumundaki `mysql` isteminde, bir veritabanı oluşturun.

```sql
CREATE DATABASE sampledb;
```

### <a name="create-a-user-with-permissions"></a>İzinleri olan bir kullanıcı oluşturun

Adlı bir veritabanı kullanıcısı oluşturmalıdır _phpappuser_ ve tüm ayrıcalıkları verin `sampledb` veritabanı. Öğreticinin Basitlik için yeniden kullanmak _MySQLAzure2017_ ve parola olarak.

```sql
CREATE USER 'phpappuser' IDENTIFIED BY 'MySQLAzure2017'; 
GRANT ALL PRIVILEGES ON sampledb.* TO 'phpappuser';
```

Sunucu bağlantısı yazarak çıkmak `quit`.

```sql
quit
```

## <a name="connect-app-to-azure-mysql"></a>Azure MySQL uygulamaya Bağlan

Bu adımda, PHP uygulaması, Azure veritabanı'nda (Önizleme) MySQL için oluşturduğunuz MySQL veritabanına bağlanın.

<a name="devconfig"></a>

### <a name="configure-the-database-connection"></a>Veritabanı bağlantısını yapılandır

Depo kök dizininde oluşturma bir _. env.production_ dosya ve aşağıdaki değişkenleri buraya kopyalayın. Yer tutucu Değiştir  _&lt;mysql_server_name >_ hem de *DB_HOST* ve *DB_USERNAME*.

```
APP_ENV=production
APP_DEBUG=true
APP_KEY=

DB_CONNECTION=mysql
DB_HOST=<mysql_server_name>.database.windows.net
DB_DATABASE=sampledb
DB_USERNAME=phpappuser@<mysql_server_name>
DB_PASSWORD=MySQLAzure2017
MYSQL_SSL=true
```

Değişiklikleri kaydedin.

> [!TIP]
> MySQL bağlantı bilgilerinizin güvenliğini sağlamak için bu dosya zaten Git deposundan dışarıda bırakılmış (bkz _.gitignore_ depo kök içinde). Daha sonra MySQL (Önizleme) Azure veritabanı veritabanınızda bağlanmak için App Service'te ortam değişkenleri yapılandırma konusunda bilgi edinin. Ortam değişkenleri ile gerekmeyen *.env* uygulama hizmeti dosyasında.
>

### <a name="configure-ssl-certificate"></a>SSL sertifikası yapılandırma

Varsayılan olarak, Azure veritabanı için MySQL istemcilerden gelen SSL bağlantıları zorlar. Azure, MySQL veritabanınızı bağlanmak için kullanmanız gerekir bir _.pem_ SSL sertifikası.

Açık _config/database.php_ ve ekleme _sslmode_ ve _seçenekleri_ parametreleri `connections.mysql`aşağıdaki kodda gösterildiği gibi.

```php
'mysql' => [
    ...
    'sslmode' => env('DB_SSLMODE', 'prefer'),
    'options' => (env('MYSQL_SSL')) ? [
        PDO::MYSQL_ATTR_SSL_KEY    => '/ssl/certificate.pem', 
    ] : []
],
```

Bu oluşturma konusunda bilgi almak için _certificate.pem_, bkz: [uygulamanızda güvenli bir şekilde MySQL için Azure veritabanına bağlanmak için SSL yapılandırma bağlantısı](../mysql/howto-configure-ssl.md).

> [!TIP]
> Yolun _/ssl/certificate.pem_ noktaları var olan _certificate.pem_ Git deposu dosyasında. Bu dosya, bu öğreticide kolaylık sağlaması açısından sunulmuştur. En iyi yöntem yürüttükten değil, _.pem_ kaynak denetimine sertifikalar. 
>

### <a name="test-the-application-locally"></a>Uygulamayı yerel olarak test etme

Laravel veritabanı geçişler ile Çalıştır _. env.production_ MySQL (Önizleme), MySQL veritabanınızı Azure veritabanı tabloları oluşturmak için ortam dosyası olarak. Unutmayın _. env.production_ Azure üzerinde MySQL veritabanı için bağlantı bilgilerini içeren.

```bash
php artisan migrate --env=production --force
```

_. env.production_ geçerli uygulama anahtarı henüz yok. Terminale daha yeni bir tane oluşturun.

```bash
php artisan key:generate --env=production --force
```

Örnek uygulama ile Çalıştır _. env.production_ ortam dosyası olarak.

```bash
php artisan serve --env=production
```

Gidin `http://localhost:8000`. Sayfa hatasız yüklerse, PHP uygulaması Azure MySQL veritabanına bağlanıyor.

Bazı görevler sayfasında ekleyin.

![PHP, Azure veritabanına başarıyla MySQL (Önizleme) bağlanır.](./media/app-service-web-tutorial-php-mysql/mysql-connect-success.png)

PHP durdurmak için aşağıdakileri yazın `Ctrl + C` Terminal.

### <a name="commit-your-changes"></a>Değişikliklerinizi uygulamak

Değişikliklerinizi kaydetmek için aşağıdaki Git komutları çalıştırın:

```bash
git add .
git commit -m "database.php updates"
```

Uygulamanız dağıtılmaya hazırdır.

## <a name="deploy-to-azure"></a>Azure’a Dağıt

Bu adımda, Azure App Service'e MySQL bağlı PHP uygulaması dağıtın.

### <a name="configure-a-deployment-user"></a>Dağıtım kullanıcısı yapılandırma

[!INCLUDE [Configure deployment user](../../includes/configure-deployment-user-no-h.md)]

### <a name="create-an-app-service-plan"></a>App Service planı oluşturma

[!INCLUDE [Create app service plan no h](../../includes/app-service-web-create-app-service-plan-no-h.md)]

<a name="create"></a>
### <a name="create-a-web-app"></a>Web uygulaması oluşturma

[!INCLUDE [Create web app no h](../../includes/app-service-web-create-web-app-php-no-h.md)] 

### <a name="configure-database-settings"></a>Veritabanı ayarlarını yapılandırma

Daha önce işaret edildiği gibi Azure MySQL veritabanınızı App Service'te ortam değişkenleri kullanarak bağlanabilir.

Ortam değişkenleri olarak ayarladığınız bulut Kabuğu'nda _uygulama ayarları_ kullanarak [az webapp config appsettings kümesi](/cli/azure/webapp/config/appsettings?view=azure-cli-latest#az_webapp_config_appsettings_set) komutu.

Aşağıdaki komut uygulama ayarlarını yapılandırır `DB_HOST`, `DB_DATABASE`, `DB_USERNAME`, ve `DB_PASSWORD`. Yer tutucuları değiştirmek  _&lt;uygulamaadı >_ ve  _&lt;mysql_server_name >_.

```azurecli-interactive
az webapp config appsettings set --name <app_name> --resource-group myResourceGroup --settings DB_HOST="<mysql_server_name>.database.windows.net" DB_DATABASE="sampledb" DB_USERNAME="phpappuser@<mysql_server_name>" DB_PASSWORD="MySQLAzure2017" MYSQL_SSL="true"
```

PHP kullanabilirsiniz [getenv](http://www.php.net/manual/function.getenv.php) ayarlarına erişmek için yöntem. Laravel kodu kullanan bir [env](https://laravel.com/docs/5.4/helpers#method-env) sarmalayıcı PHP üzerinden `getenv`. Örneğin, MySQL yapılandırmasında _config/database.php_ aşağıdaki kod gibi görünüyor:

```php
'mysql' => [
    'driver'    => 'mysql',
    'host'      => env('DB_HOST', 'localhost'),
    'database'  => env('DB_DATABASE', 'forge'),
    'username'  => env('DB_USERNAME', 'forge'),
    'password'  => env('DB_PASSWORD', ''),
    ...
],
```

### <a name="configure-laravel-environment-variables"></a>Laravel ortam değişkenlerini yapılandırın

Laravel App Service'te bir uygulama anahtarı gerekir. Uygulama ayarları ile yapılandırabilirsiniz.

Yerel terminal penceresinde kullanmak `php artisan` için kaydetmeden yeni bir uygulama anahtarı oluşturmak için _.env_.

```bash
php artisan key:generate --show
```

Bulut Kabuğu'nda uygulama App Service'te web uygulaması kullanarak ayarlayan [az webapp config appsettings kümesi](/cli/azure/webapp/config/appsettings?view=azure-cli-latest#az_webapp_config_appsettings_set) komutu. Yer tutucuları değiştirmek  _&lt;uygulamaadı >_ ve  _&lt;outputofphpartisankey: Oluştur >_.

```azurecli-interactive
az webapp config appsettings set --name <app_name> --resource-group myResourceGroup --settings APP_KEY="<output_of_php_artisan_key:generate>" APP_DEBUG="true"
```

`APP_DEBUG="true"`dağıtılan web uygulamasının hatalarla karşılaştığında hata ayıklama bilgileri döndürmek için Laravel söyler. Bir üretim uygulaması çalıştırırken kümesine `false`, daha güvenli olduğu.

### <a name="set-the-virtual-application-path"></a>Sanal uygulama yolu ayarla

Web uygulaması için sanal uygulama yolu ayarlayın. Bu adım gereklidir çünkü [Laravel uygulama yaşam döngüsü](https://laravel.com/docs/5.4/lifecycle) içinde başlar _ortak_ uygulamanızın kök dizininde yerine dizin. İçinde yaşam döngüsü başlatmak diğer PHP çerçeveleri kök dizini sanal uygulama yolu el ile yapılandırma olmadan çalışabilir.

Bulut Kabuğu'nda, sanal uygulama yolu kullanarak ayarlamak [az kaynak güncelleştirme](/cli/azure/resource#update) komutu. Değiştir  _&lt;uygulamaadı >_ yer tutucu.

```azurecli-interactive
az resource update --name web --resource-group myResourceGroup --namespace Microsoft.Web --resource-type config --parent sites/<app_name> --set properties.virtualApplications[0].physicalPath="site\wwwroot\public" --api-version 2015-06-01
```

Varsayılan olarak, Azure App Service kök sanal uygulama yolu işaret (_/_) dağıtılan uygulama dosyalarını kök dizinine (_sites\wwwroot_).

### <a name="push-to-azure-from-git"></a>Git üzerinden Azure'a gönderme

Yerel terminal penceresinde yerel Git deponuza bir Azure uzak deposu ekleyin. Değiştir  _&lt;Yapıştır\_kopyalanan\_url\_burada >_ , kaydedilen Git uzak URL'si ile [bir web uygulaması oluşturma](#create).

```bash
git remote add azure <paste_copied_url_here>
```

Azure'a PHP uygulama dağıtmak için uzaktan gönderin. Daha önce dağıtım kullanıcı oluşturmanın bir parçası sağlanan parola istenir.

```bash
git push azure master
```

Dağıtım sırasında Azure App Service, ilerleme durumunu Git ile iletişim kurar.

```bash
Counting objects: 3, done.
Delta compression using up to 8 threads.
Compressing objects: 100% (3/3), done.
Writing objects: 100% (3/3), 291 bytes | 0 bytes/s, done.
Total 3 (delta 2), reused 0 (delta 0)
remote: Updating branch 'master'.
remote: Updating submodules.
remote: Preparing deployment for commit id 'a5e076db9c'.
remote: Running custom deployment command...
remote: Running deployment command...
...
< Output has been truncated for readability >
```

> [!NOTE]
> Dağıtım işlemi yükleyeceğini karşılaşabilirsiniz [Oluşturucu](https://getcomposer.org/) sonunda paketler. Uygulama hizmeti varsayılan dağıtımı sırasında bu otomasyonlara çalışmaz, bu örnek depo etkinleştirmek için kök dizindeki üç ek dosyaların sahiptir:
>
> - `.deployment`-Bu dosyayı çalıştırmak için uygulama hizmeti söyler `bash deploy.sh` özel dağıtım komut dosyası olarak.
> - `deploy.sh`-Özel dağıtım komut dosyası. Dosyayı gözden geçirirseniz, çalıştığını görürsünüz `php composer.phar install` sonra `npm install`.
> - `composer.phar`-Oluşturucu Paket Yöneticisi.
>
> Git tabanlı dağıtımınız App Service için herhangi bir adımı eklemek için bu yaklaşımı kullanın. Daha fazla bilgi için bkz: [özel dağıtım betiği](https://github.com/projectkudu/kudu/wiki/Custom-Deployment-Script).
>

### <a name="browse-to-the-azure-web-app"></a>Azure web uygulaması'na göz atın

Gözat `http://<app_name>.azurewebsites.net` ve birkaç görevi listesine ekleyin.

![Azure uygulama Hizmeti'nde çalışan PHP uygulaması](./media/app-service-web-tutorial-php-mysql/php-mysql-in-azure.png)

Tebrikler, Azure App Service'te bir veri güdümlü PHP uygulaması çalıştırıyorsanız.

## <a name="update-model-locally-and-redeploy"></a>Modeli yerel olarak güncelleştirin ve yeniden dağıtın

Bu adımda, bir basit değişiklik `task` veri modeli ve webapp ve güncelleştirme Azure'a yayımlayabilirsiniz.

Görevler senaryo için bir görev tamamlandı olarak işaretleyebilirsiniz şekilde uygulama değiştirin.

### <a name="add-a-column"></a>Sütun ekle

Yerel terminal penceresine Git deposu kök dizinine gidin.

İçin yeni bir veritabanı geçiş oluşturmayı `tasks` tablosu:

```bash
php artisan make:migration add_complete_column --table=tasks
```

Bu komut oluşturulur geçiş dosyasının adını gösterir. Bu dosyada Bul _veritabanı/geçişler_ ve açın.

Değiştir `up` aşağıdaki kod ile yöntemi:

```php
public function up()
{
    Schema::table('tasks', function (Blueprint $table) {
        $table->boolean('complete')->default(False);
    });
}
```

Önceki kod bir boolean sütunundaki ekler `tasks` adlı bir tablo `complete`.

Değiştir `down` geri alma eylemi için aşağıdaki kod ile yöntemi:

```php
public function down()
{
    Schema::table('tasks', function (Blueprint $table) {
        $table->dropColumn('complete');
    });
}
```

Yerel terminal penceresinde yerel veritabanında değişiklik yapmak için Laravel veritabanı geçiş çalıştırın.

```bash
php artisan migrate
```

Temel [Laravel adlandırma kuralı](https://laravel.com/docs/5.4/eloquent#defining-models), model `Task` (bkz _app/Task.php_) eşlendiği `tasks` varsayılan olarak tablo.

### <a name="update-application-logic"></a>Uygulama mantığını güncelleştir

Açık *routes/web.php* dosya. Uygulama kendi yollar ve iş mantığı buraya tanımlar.

Dosyanın sonunda bir yol ile aşağıdaki kodu ekleyin:

```php
/**
 * Toggle Task completeness
 */
Route::post('/task/{id}', function ($id) {
    error_log('INFO: post /task/'.$id);
    $task = Task::findOrFail($id);

    $task->complete = !$task->complete;
    $task->save();

    return redirect('/');
});
```

Önceki kod, değerini değiştirerek veri modelini basit bir güncelleştirme yapar `complete`.

### <a name="update-the-view"></a>Görünümü güncelleştirmek

Açık *resources/views/tasks.blade.php* dosya. Arama `<tr>` açma etiketi ve ile değiştirin:

```html
<tr class="{{ $task->complete ? 'success' : 'active' }}" >
```

Önceki kod görev tam olup olmamasına bağlı olarak satır rengi değişir.

Sonraki satırında, aşağıdaki kodu kullanabilirsiniz:

```html
<td class="table-text"><div>{{ $task->name }}</div></td>
```

Tüm satırı aşağıdaki kodla değiştirin:

```html
<td>
    <form action="{{ url('task/'.$task->id) }}" method="POST">
        {{ csrf_field() }}

        <button type="submit" class="btn btn-xs">
            <i class="fa {{$task->complete ? 'fa-check-square-o' : 'fa-square-o'}}"></i>
        </button>
        {{ $task->name }}
    </form>
</td>
```

Önceki kod daha önce tanımlanan rota başvuran Gönder düğmesi ekler.

### <a name="test-the-changes-locally"></a>Değişiklikleri yerel olarak test etme

Yerel terminal penceresinde geliştirme sunucusu Git deposu kök dizininden çalıştırın.

```bash
php artisan serve
```

Değiştirme, gidin görev durumunu görmek için `http://localhost:8000` ve onay kutusunu seçin.

![Görev için eklenen onay kutusu](./media/app-service-web-tutorial-php-mysql/complete-checkbox.png)

PHP durdurmak için aşağıdakileri yazın `Ctrl + C` Terminal.

### <a name="publish-changes-to-azure"></a>Değişiklikler için Azure yayımlama

Yerel terminal penceresinde Laravel veritabanı geçişler üretim bağlantı dizesini içeren Azure veritabanında değişiklik yapmak için çalıştırın.

```bash
php artisan migrate --env=production --force
```

Git tüm değişiklikleri uygulayın ve ardından kod değişiklikleri Azure'a gönderin.

```bash
git add .
git commit -m "added complete checkbox"
git push azure master
```

Bir kez `git push` tamamlamak, Azure web uygulaması'na gidin ve yeni işlevselliğini test etmek içindir.

![Azure için yayımlanan modeli ve veritabanı değişiklikleri](media/app-service-web-tutorial-php-mysql/complete-checkbox-published.png)

Herhangi bir görevi eklediyseniz bunlar veritabanında tutulur. Veri şema güncelleştirmeleri varolan veriler olduğu gibi bırakın.

## <a name="stream-diagnostic-logs"></a>Akış tanılama günlükleri

PHP uygulaması Azure App Service'te çalışırken, terminal yöneltilen konsol günlükleri alabilirsiniz. Böylece, uygulama hatalarını hata ayıklama yardımcı olmak için aynı tanılama iletileri alabilirsiniz.

Günlük akış başlatmak için kullanmak [az webapp günlük tail](/cli/azure/webapp/log?view=azure-cli-latest#az_webapp_log_tail) bulut Kabuğu'nda komutu.

```azurecli-interactive
az webapp log tail --name <app_name> --resource-group myResourceGroup
```

Günlük akış başladıktan sonra Azure web uygulaması bazı web trafiği almak için tarayıcıda yenileyin. Konsol günlükleri terminale yöneltilen şimdi görebilirsiniz. Konsol günlükleri hemen görmüyorsanız, 30 saniye içinde yeniden kontrol edin.

Dilediğiniz zaman oturum sırasında akış durdurmak için aşağıdakileri yazın `Ctrl` + `C`.

> [!TIP]
> Bir PHP uygulaması standart kullanabilir [error_log()](http://php.net/manual/function.error-log.php) konsola çıktı için. Örnek uygulama bu yaklaşımı kullanır _app/Http/routes.php_.
>
> Bir web çerçevesidir olarak [Laravel kullanan Monolog](https://laravel.com/docs/5.4/errors) olarak oturum açma sağlayıcısı. Çıkış iletilerini konsola Monolog alınacağı hakkında bilgi için bkz: [PHP: monolog Konsolu'na (php://out) oturum için nasıl kullanılacağını](http://stackoverflow.com/questions/25787258/php-how-to-use-monolog-to-log-to-console-php-out).
>
>

## <a name="manage-the-azure-web-app"></a>Azure web uygulamasını yönetme

Oluşturduğunuz web uygulamasını yönetmek için [Azure portalına](https://portal.azure.com) gidin.

Sol menüden **Uygulama Hizmetleri**'ne ve ardından Azure web uygulamanızın adına tıklayın.

![Portaldan Azure web uygulamasına gitme](./media/app-service-web-tutorial-php-mysql/access-portal.png)

Web uygulamanızın Genel Bakış sayfasını görürsünüz. Burada, Durdur, Başlat, yeniden başlatma, Gözat ve silme gibi temel yönetim görevlerini gerçekleştirebilirsiniz.

Soldaki menüden, uygulamanızı yapılandırmak için sayfaları sağlar.

![Azure portalında App Service sayfası](./media/app-service-web-tutorial-php-mysql/web-app-blade.png)

[!INCLUDE [cli-samples-clean-up](../../includes/cli-samples-clean-up.md)]

<a name="next"></a>

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Azure üzerinde MySQL veritabanı oluşturma
> * MySQL için PHP uygulamaya Bağlan
> * Uygulamayı Azure'a dağıtma
> * Veri modeli güncelleştirme ve uygulamayı yeniden dağıtın
> * Azure Stream tanılama günlükleri
> * Azure portalında uygulama yönetme

Bir web uygulaması için özel bir DNS adı eşleme öğrenmek için sonraki öğretici ilerleyin.

> [!div class="nextstepaction"]
> [Mevcut bir özel DNS adını Azure Web Apps ile eşleme](app-service-web-tutorial-custom-domain.md)
