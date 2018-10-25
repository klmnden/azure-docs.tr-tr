---
title: Linux üzerinde Azure App Service’te bir PHP ve MySQL web uygulaması derleme | Microsoft Docs
description: Azure’da çalışan ve bir MySQL veritabanı ile bağlantısı olan PHP uygulamasını nasıl edinebileceğinizi öğrenin.
services: app-service\web
author: cephalin
manager: erikre
ms.service: app-service-web
ms.workload: web
ms.devlang: nodejs
ms.topic: tutorial
ms.date: 11/28/2017
ms.author: cephalin
ms.custom: mvc
ms.openlocfilehash: 9dbdcc9eb09ff137b32225e83e42ec5baca39396
ms.sourcegitcommit: f6050791e910c22bd3c749c6d0f09b1ba8fccf0c
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50024554"
---
# <a name="build-a-php-and-mysql-web-app-in-azure-app-service-on-linux"></a>Linux üzerinde Azure App Service’te bir PHP ve MySQL web uygulaması derleme

> [!NOTE]
> Bu makalede bir uygulamanın Linux üzerinde App Service'e dağıtımı yapılır. _Windows_'da App Service dağıtmak için bkz. [Azure'da PHP ve MySQL web uygulaması derleme](../app-service-web-tutorial-php-mysql.md).
>

[Linux’ta App Service](app-service-linux-intro.md) Linux işletim sistemini kullanan yüksek oranda ölçeklenebilir, otomatik olarak düzeltme eki uygulayan bir web barındırma hizmeti sağlar. Bu öğreticide, bir PHP web uygulaması oluşturma ve bu uygulamayı bir MySQL veritabanına bağlamayla ilgili yönergeler verilmiştir. İşiniz bittiğinde, Linux üzerinde App Service’te çalışan bir [Laravel](https://laravel.com/) uygulamasına sahip olacaksınız.

![Azure App Service’te çalışan PHP uygulaması](./media/tutorial-php-mysql-app/complete-checkbox-published.png)

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Azure’da MySQL veritabanı oluşturma
> * PHP uygulamasını MySQL’e bağlama
> * Uygulamayı Azure’da dağıtma
> * Veri modelini güncelleştirme ve uygulamayı yeniden dağıtma
> * Azure’daki tanılama günlüklerinin akışını sağlama
> * Uygulamayı Azure portalında yönetme

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamak için:

* [Git'i yükleyin](https://git-scm.com/)
* [PHP 5.6.4 veya sonraki sürümü yükleme](http://php.net/downloads.php)
* [Oluşturucu Yükleme](https://getcomposer.org/doc/00-intro.md)
* Laravel için gereken şu PHP uzantılarını etkinleştirin: OpenSSL, PDO-MySQL, Mbstring, Tokenizer, XML
* [MySQL yükleme ve başlatma](https://dev.mysql.com/doc/refman/5.7/en/installing.html) 

## <a name="prepare-local-mysql"></a>Yerel MySQL hazırlama

Bu adımda, bu öğreticide kullanmak üzere yerel MySQL sunucunuzda bir veritabanı oluşturursunuz.

### <a name="connect-to-local-mysql-server"></a>Yerel MySQL sunucusuna bağlanma

Bir terminal penceresinde yerel MySQL sunucunuza bağlanın. Bu öğreticideki tüm komutları çalıştırmak için bu terminal penceresini kullanabilirsiniz.

```bash
mysql -u root -p
```

Parola istenirse `root` hesabının parolasını girin. Kök hesap parolanızı hatırlamıyorsanız bkz [MySQL: Kök Parolayı Sıfırlama](https://dev.mysql.com/doc/refman/5.7/en/resetting-permissions.html).

Komutunuz başarıyla çalışırsa, MySQL sunucunuz çalışıyor demektir. Çalışmıyorsa, yerel MySQL sunucunuzun aşağıdaki [MySQL yükleme sonrası adımları](https://dev.mysql.com/doc/refman/5.7/en/postinstallation.html) kullanılarak başlatıldığından emin olun.

### <a name="create-a-database-locally"></a>Yerel olarak veritabanı oluşturma

`mysql` isteminde bir veritabanı oluşturun.

```sql 
CREATE DATABASE sampledb;
```

`quit` yazarak sunucu bağlantınızdan çıkış yapın.

```sql
quit
```

<a name="step2"></a>

## <a name="create-a-php-app-locally"></a>Yerel olarak PHP uygulaması oluşturma
Bu adımda bir Laravel örnek uygulaması edinir, veritabanı bağlantısını yapılandırır ve yerel olarak çalıştırırsınız. 

### <a name="clone-the-sample"></a>Örneği kopyalama

Terminal penceresinde, `cd` ile bir çalışma dizinine gidin.

Örnek depoyu kopyalamak için aşağıdaki komutu çalıştırın.

```bash
git clone https://github.com/Azure-Samples/laravel-tasks
```

`cd` komutuyla kopyalanmış dizininize geçin.
Gereken paketleri yükleyin.

```bash
cd laravel-tasks
composer install
```

### <a name="configure-mysql-connection"></a>MySQL bağlantısını yapılandırma

Depo kökünde *.env* adlı bir dosya oluşturun. Aşağıdaki değişkenleri *.env* dosyasına kopyalayın. _&lt;root_password>_ yer tutucusunu MySQL kök kullanıcı parolası ile değiştirin.

```txt
APP_ENV=local
APP_DEBUG=true
APP_KEY=SomeRandomString

DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_DATABASE=sampledb
DB_USERNAME=root
DB_PASSWORD=<root_password>
```

Laravel’in _.env_ dosyasını nasıl kullandığı hakkında bilgi için bkz. [Laravel Ortamı Yapılandırması](https://laravel.com/docs/5.4/configuration#environment-configuration).

### <a name="run-the-sample-locally"></a>Örneği yerel olarak çalıştırma

Uygulama için gereken tabloları oluşturmak üzere [Laravel veritabanı geçişlerini](https://laravel.com/docs/5.4/migrations) çalıştırın. Geçişlerde hangi tabloların oluşturulduğunu görmek için, Git deposundaki _veritabanı/geçişler_ dizinine bakın.

```bash
php artisan migrate
```

Yeni bir Laravel uygulama anahtarı oluşturun.

```bash
php artisan key:generate
```

Uygulamayı çalıştırın.

```bash
php artisan serve
```

Bir tarayıcıda `http://localhost:8000` sayfasına gidin. Sayfaya birkaç görev ekleyin.

![PHP başarıyla MySQL’e bağlanır](./media/tutorial-php-mysql-app/mysql-connect-success.png)

PHP’yi durdurmak için terminale `Ctrl + C` yazın.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## <a name="create-mysql-in-azure"></a>Azure’da MySQL oluşturma

Bu adımda, [MySQL için Azure Veritabanı](/azure/mysql) içinde bir MySQL veritabanı oluşturursunuz. Daha sonra, PHP uygulamasını bu veritabanına bağlanacak şekilde yapılandırırsınız.

### <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

[!INCLUDE [Create resource group](../../../includes/app-service-web-create-resource-group-linux-no-h.md)] 

### <a name="create-a-mysql-server"></a>MySQL sunucusu oluşturma

MySQL için Azure Veritabanı içinde [`az mysql server create`](/cli/azure/mysql/server?view=azure-cli-latest#az-mysql-server-create) komutu ile bir sunucu oluşturun.

Aşağıdaki komutta, _&lt;mysql_server_name>_ yer tutucusunu gördüğünüz yerde MySQL sunucunuzun adını değiştirin (geçerli karakterler `a-z`, `0-9` ve `-`). Bu ad, MySQL sunucusu ana bilgisayar adının (`<mysql_server_name>.database.windows.net`) bir parçasıdır ve genel olarak benzersiz olması gerekir.

```azurecli-interactive
az mysql server create --name <mysql_server_name> --resource-group myResourceGroup --location "North Europe" --admin-user adminuser --admin-password My5up3r$tr0ngPa$w0rd!
```

MySQL sunucusu oluşturulduğunda Azure CLI, aşağıdaki örneğe benzer bilgiler gösterir:

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

### <a name="configure-server-firewall"></a>Sunucu güvenlik duvarını yapılandırma

[`az mysql server firewall-rule create`](/cli/azure/mysql/server/firewall-rule?view=azure-cli-latest#az-mysql-server-firewall-rule-create) komutunu kullanarak MySQL sunucunuzun istemci bağlantılarına izin vermesi için bir güvenlik duvarı kuralı oluşturun. Hem başlangıç hem bitiş IP’si 0.0.0.0 olarak ayarlandığında, güvenlik duvarı yalnızca diğer Azure kaynakları için açılır. 

```azurecli-interactive
az mysql server firewall-rule create --name allAzureIPs --server <mysql_server_name> --resource-group myResourceGroup --start-ip-address 0.0.0.0 --end-ip-address 0.0.0.0
```

> [!TIP] 
> [Yalnızca uygulamanızın kullandığı giden IP adreslerini kullanarak](../app-service-ip-addresses.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json#find-outbound-ips) güvenlik duvarı kurallarınızda daha da kısıtlayıcı olabilirsiniz.
>

### <a name="connect-to-production-mysql-server-locally"></a>Üretim MySQL sunucusuna yerel olarak bağlanma

Terminal penceresinde, Azure’da MySQL sunucusuna bağlanın. Daha önce _&lt;mysql_server_name>_ için belirttiğiniz değeri kullanın.

```bash
mysql -u adminuser@<mysql_server_name> -h <mysql_server_name>.database.windows.net -P 3306 -p
```

Parola sorulduğunda, veritabanı sunucusunu oluştururken belirttiğiniz _$tr0ngPa$w0rd!_ parolasını kullanın.

### <a name="create-a-production-database"></a>Üretim veritabanı oluşturma

`mysql` isteminde bir veritabanı oluşturun.

```sql
CREATE DATABASE sampledb;
```

### <a name="create-a-user-with-permissions"></a>İzinleri olan bir kullanıcı oluşturma

_phpappuser_ adlı bir veritabanı kullanıcısı oluşturun ve bu kullanıcıya `sampledb` veritabanındaki tüm ayrıcalıkları verin.

```sql
CREATE USER 'phpappuser' IDENTIFIED BY 'MySQLAzure2017'; 
GRANT ALL PRIVILEGES ON sampledb.* TO 'phpappuser';
```

`quit` yazarak sunucu bağlantısından çıkış yapın.

```sql
quit
```

## <a name="connect-app-to-azure-mysql"></a>Uygulamayı Azure MySQL’e bağlama

Bu adımda, PHP uygulamasını MySQL için Azure Veritabanı içinde oluşturduğunuz MySQL veritabanına bağlarsınız.

<a name="devconfig"></a>

### <a name="configure-the-database-connection"></a>Veritabanı bağlantısını yapılandırma

Depo kökünde bir _.env.production_ dosyası oluşturun ve içine aşağıdaki değişkenleri kopyalayın. _&lt;mysql_server_name>_ yer tutucusunu değiştirin.

```txt
APP_ENV=production
APP_DEBUG=true
APP_KEY=SomeRandomString

DB_CONNECTION=mysql
DB_HOST=<mysql_server_name>.mysql.database.azure.com
DB_DATABASE=sampledb
DB_USERNAME=phpappuser@<mysql_server_name>
DB_PASSWORD=MySQLAzure2017
MYSQL_SSL=true
```

Değişiklikleri kaydedin.

> [!TIP]
> MySQL bağlantı bilgilerinizin güvenliğini sağlamak için bu dosya zaten Git deposunun dışında bırakılmıştır (Depo kökünde _.gitignore_ dosyasına bakın). Daha sonra, App Service’teki ortam değişkenlerini, MySQL için Azure Veritabanı içinde veritabanınıza bağlanmak üzere nasıl yapılandıracağınızı öğreneceksiniz. Ortam değişkenlerini kullandığınızda App Service içinde *.env* dosyası gerekli değildir.
>

### <a name="configure-ssl-certificate"></a>SSL sertifikası yapılandırma

Varsayılan olarak, MySQL için Azure Veritabanı, istemcilerden gelen SSL bağlantılarını zorlar. Azure’da MySQL veritabanınıza bağlanmak üzere MySQL için Azure Veritabanı tarafından sağlanan [_.pem_ sertifikasını kullanmanız gerekir](../../mysql/howto-configure-ssl.md).

_config/database.php_ dosyasını açın ve aşağıdaki kodda gösterildiği gibi _sslmode_ ile _options_ parametrelerini `connections.mysql` içine ekleyin.

```php
'mysql' => [
    ...
    'sslmode' => env('DB_SSLMODE', 'prefer'),
    'options' => (env('MYSQL_SSL')) ? [
        PDO::MYSQL_ATTR_SSL_KEY    => '/ssl/BaltimoreCyberTrustRoot.crt.pem', 
    ] : []
],
```

`BaltimoreCyberTrustRoot.crt.pem` sertifikası, bu öğreticide kolaylık sağlaması açısından depoda sunulmuştur. 

### <a name="test-the-application-locally"></a>Uygulamayı yerel olarak test etme

MySQL için Azure Veritabanı içinde tablolar oluşturmak için, _.env.production_ ile Laravel veritabanı geçişlerini ortam dosyası olarak çalıştırın. _.env.production_ dosyasının, Azure’da MySQL veritabanınızla bağlantı bilgilerini içerdiğini unutmayın.

```bash
php artisan migrate --env=production --force
```

_.env.production_ henüz geçerli bir uygulama anahtarına sahip değildir. Terminalde bu dosya için yeni bir tane oluşturun.

```bash
php artisan key:generate --env=production --force
```

Örnek uygulamayı _.env.production_ ortam dosyası ile birlikte çalıştırın.

```bash
php artisan serve --env=production
```

`http://localhost:8000` sayfasına gidin. Sayfa hatasız yüklenirse, PHP uygulaması Azure’da MySQL veritabanına bağlanıyor demektir.

Sayfaya birkaç görev ekleyin.

![PHP, MySQL için Azure Veritabanı’na başarıyla bağlanır](./media/tutorial-php-mysql-app/mysql-connect-success.png)

PHP’yi durdurmak için terminale `Ctrl + C` yazın.

### <a name="commit-your-changes"></a>Değişikliklerinizi kaydetme

Değişikliklerinizi kaydetmek için aşağıdaki Git komutlarını çalıştırın:

```bash
git add .
git commit -m "database.php updates"
```

Uygulamanız dağıtılmaya hazırdır.

## <a name="deploy-to-azure"></a>Azure’a dağıtma

Bu adımda, MySQL’e bağlı PHP uygulamasını Azure App Service'e dağıtırsınız.

Laravel uygulaması _/public_ dizininde başlar. App Service için varsayılan PHP Docker görüntüsü, Apache kullanır ve Laravel için `DocumentRoot` öğesini özelleştirmenize izin vermez. Bununla birlikte, `.htaccess` kullanarak tüm istekleri kök dizin yerine _/public_ dizinini işaret edecek şekilde yeniden yazabilirsiniz. Depo köküne bu amaçla bir `.htaccess` zaten eklenmiştir. Bununla, Laravel uygulamanız dağıtılmaya hazırdır.

> [!NOTE] 
> _.htaccess_ yeniden yazma işlemini kullanmazsanız, Laravel uygulamanızı bunun yerine bir [özel Docker görüntüsü](quickstart-docker-go.md) ile dağıtabilirsiniz.
>
>

### <a name="configure-a-deployment-user"></a>Dağıtım kullanıcısı yapılandırma

[!INCLUDE [Configure deployment user](../../../includes/configure-deployment-user-no-h.md)]

### <a name="create-an-app-service-plan"></a>App Service planı oluşturma

[!INCLUDE [Create app service plan no h](../../../includes/app-service-web-create-app-service-plan-linux-no-h.md)]

### <a name="create-a-web-app"></a>Web uygulaması oluşturma

[!INCLUDE [Create web app](../../../includes/app-service-web-create-web-app-php-linux-no-h.md)] 

### <a name="configure-database-settings"></a>Veritabanı ayarlarını yapılandırma

App Service’te [`az webapp config appsettings set`](/cli/azure/webapp/config/appsettings?view=azure-cli-latest#az-webapp-config-appsettings-set) komutunu kullanarak ortam değişkenlerini _uygulama ayarları_ olarak ayarlayabilirsiniz.

Aşağıdaki komut `DB_HOST`, `DB_DATABASE`, `DB_USERNAME` ve `DB_PASSWORD` uygulama ayarlarını yapılandırır. _&lt;appname>_ ve _&lt;mysql_server_name>_ yer tutucularını değiştirin.

```azurecli-interactive
az webapp config appsettings set --name <app_name> --resource-group myResourceGroup --settings DB_HOST="<mysql_server_name>.database.windows.net" DB_DATABASE="sampledb" DB_USERNAME="phpappuser@<mysql_server_name>" DB_PASSWORD="MySQLAzure2017" MYSQL_SSL="true"
```

Ayarlara erişmek için PHP [getenv](http://php.net/manual/en/function.getenv.php) yöntemini kullanabilirsiniz. Laravel kodu, PHP `getenv` üzerinde bir [env](https://laravel.com/docs/5.4/helpers#method-env) sarmalayıcı kullanır. Örneğin, _config/database.php_ içindeki MySQL yapılandırması aşağıdaki kod gibi görünür:

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

### <a name="configure-laravel-environment-variables"></a>Laravel ortam değişkenlerini yapılandırma

Laravel, App Service'te bir uygulama anahtarı gerektirir. Uygulama anahtarını uygulama ayarları ile yapılandırabilirsiniz.

Uygulama anahtarını _.env_ dosyasına kaydetmeden yeni bir uygulama anahtarı oluşturmak için `php artisan` seçeneğini kullanın.

```bash
php artisan key:generate --show
```

[`az webapp config appsettings set`](/cli/azure/webapp/config/appsettings?view=azure-cli-latest#az-webapp-config-appsettings-set) komutunu kullanarak App Service web uygulamasında uygulama anahtarını ayarlayın. _&lt;appname>_ ve _&lt;outputofphpartisankey:generate>_ yer tutucularını değiştirin.

```azurecli-interactive
az webapp config appsettings set --name <app_name> --resource-group myResourceGroup --settings APP_KEY="<output_of_php_artisan_key:generate>" APP_DEBUG="true"
```

`APP_DEBUG="true"`, dağıtılan web uygulaması hatalarla karşılaştığında Laravel’den hata ayıklama bilgilerini döndürmesini ister. Bir üretim uygulaması çalıştırırken daha güvenli olan `false` seçeneğine ayarlayın.

### <a name="push-to-azure-from-git"></a>Git üzerinden Azure'a gönderme

Yerel Git deponuza bir Azure uzak deposu ekleyin.

```bash
git remote add azure <paste_copied_url_here>
```

PHP uygulamasını dağıtmak için Azure uzak deposuna gönderin. Daha önce dağıtım kullanıcısı oluştururken belirttiğiniz parola istenir.

```bash
git push azure master
```

Dağıtım sırasında Azure App Service, ilerleme durumunu Git'e iletir.

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
> Dağıtım işleminin sonunda [Composer](https://getcomposer.org/) paketleri yüklediğini fark edebilirsiniz. App Service, varsayılan dağıtım sırasında bu otomasyonları çalıştırmadığı için bu örnek depoyu etkinleştirmek üzere kök dizinde üç ek dosya bulunur:
>
> - `.deployment` - Bu dosya, App Service’ten özel dağıtım betiği olarak `bash deploy.sh` komutunu çalıştırmasını ister.
> - `deploy.sh` - Özel dağıtım betiği. Dosyayı gözden geçirirseniz, `npm install` komutundan sonra `php composer.phar install` çalıştırdığını görürsünüz.
> - `composer.phar` - Composer paket yöneticisi.
>
> App Service’e Git tabanlı dağıtımınıza herhangi bir adım eklemek için bu yaklaşımı kullanabilirsiniz. Daha fazla bilgi için bkz. [Özel Dağıtım Betiği](https://github.com/projectkudu/kudu/wiki/Custom-Deployment-Script).
>

### <a name="browse-to-the-azure-web-app"></a>Azure web uygulamasına göz atma

`http://<app_name>.azurewebsites.net` listesine göz atın ve listeye birkaç görev ekleyin.

![Azure App Service’te çalışan PHP uygulaması](./media/tutorial-php-mysql-app/php-mysql-in-azure.png)

Tebrikler, Azure App Service'te veri temelli bir PHP uygulaması çalıştırıyorsunuz.

## <a name="update-model-locally-and-redeploy"></a>Modeli yerel olarak güncelleştirme ve yeniden dağıtma

Bu adımda, `task` veri modeli ve web uygulamasında basit bir değişiklik yaptıktan sonra güncelleştirmeyi Azure'da yayımlarsınız.

Görevler senaryosu için, görevi tamamlandı olarak işaretleyebileceğiniz şekilde uygulamayı değiştirirsiniz.

### <a name="add-a-column"></a>Sütun ekleme

Terminalde Git deposunun kök dizinine gidin.

`tasks` tablosu için yeni bir veritabanı geçişi oluşturun:

```bash
php artisan make:migration add_complete_column --table=tasks
```

Bu komut, oluşturulan geçiş dosyasının adını gösterir. _database/migrations_ içinde bu dosyayı bulup açın.

`up` yöntemini aşağıdaki kod ile değiştirin:

```php
public function up()
{
    Schema::table('tasks', function (Blueprint $table) {
        $table->boolean('complete')->default(False);
    });
}
```

Yukarıdaki kod, `tasks` tablosuna `complete` adlı bir boole sütunu ekler.

Geri alma eylemi için `down` yöntemini aşağıdaki kod ile değiştirin:

```php
public function down()
{
    Schema::table('tasks', function (Blueprint $table) {
        $table->dropColumn('complete');
    });
}
```

Terminalde, Laravel veritabanı geçişlerini çalıştırarak yerel veritabanında değişikliği yapın.

```bash
php artisan migrate
```

[Laravel adlandırma kuralına](https://laravel.com/docs/5.4/eloquent#defining-models) göre `Task` modeli (bkz. _app/Task.php_) varsayılan olarak `tasks` tablosu ile eşlenir.

### <a name="update-application-logic"></a>Uygulama mantığını güncelleştirme

*routes/web.php* dosyasını açın. Uygulama, yollarını ve iş mantığını burada tanımlar.

Dosyanın sonuna aşağıdaki kod ile bir yol ekleyin:

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

Yukarıdaki kod, `complete` değerini değiştirerek veri modelinde basit bir güncelleştirme yapar.

### <a name="update-the-view"></a>Görünümü güncelleştirme

*resources/views/tasks.blade.php* dosyasını açın. `<tr>` açma etiketini bulup şununla değiştirin:

```html
<tr class="{{ $task->complete ? 'success' : 'active' }}" >
```

Yukarıdaki kod, görevin tamamlanıp tamamlanmamasına bağlı olarak satır rengini değiştirir.

Sonraki satırda şu kodu görürsünüz:

```html
<td class="table-text"><div>{{ $task->name }}</div></td>
```

Tüm satırı aşağıdaki kod ile değiştirin:

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

Yukarıdaki kod, daha önce tanımladığınız yola başvuran Gönder düğmesini ekler.

### <a name="test-the-changes-locally"></a>Değişiklikleri yerel olarak test etme

Git deposunun kök dizininden geliştirme sunucusunu çalıştırın.

```bash
php artisan serve
```

Görev durumu değişikliğini görmek için `http://localhost:8000` öğesine gidip onay kutusunu işaretleyin.

![Göreve eklenen onay kutusu](./media/tutorial-php-mysql-app/complete-checkbox.png)

PHP’yi durdurmak için terminale `Ctrl + C` yazın.

### <a name="publish-changes-to-azure"></a>Değişiklikleri Azure’da yayımlama

Terminalde, Laravel veritabanı geçişlerini üretim bağlantı dizesi ile birlikte çalıştırarak değişikliği Azure veritabanında yapın.

```bash
php artisan migrate --env=production --force
```

Tüm değişiklikleri Git’e kaydedin ve ardından kod değişikliklerini Azure’a gönderin.

```bash
git add .
git commit -m "added complete checkbox"
git push azure master
```

`git push` tamamlandığında, Azure web uygulamasına gidin ve yeni işlevleri test edin.

![Azure’da yayımlanan model ve veritabanı değişiklikleri](media/tutorial-php-mysql-app/complete-checkbox-published.png)

Herhangi bir görevi eklediyseniz veritabanında tutulur. Veri şemasında yapılan güncelleştirmeler var olan verileri olduğu gibi bırakır.

## <a name="manage-the-azure-web-app"></a>Azure web uygulamasını yönetme

Oluşturduğunuz web uygulamasını yönetmek için [Azure portalına](https://portal.azure.com) gidin.

Sol menüden **Uygulama Hizmetleri**'ne ve ardından Azure web uygulamanızın adına tıklayın.

![Portaldan Azure web uygulamasına gitme](./media/tutorial-php-mysql-app/access-portal.png)

Web uygulamanızın Genel Bakış sayfasını görürsünüz. Buradan durdurma, başlatma, yeniden başlatma, göz atma ve silme gibi temel yönetim görevlerini gerçekleştirebilirsiniz.

Soldaki menü, uygulamanızı yapılandırmaya yönelik sayfalar sağlar.

![Azure portalında App Service sayfası](./media/tutorial-php-mysql-app/web-app-blade.png)

[!INCLUDE [cli-samples-clean-up](../../../includes/cli-samples-clean-up.md)]

<a name="next"></a>

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Azure’da MySQL veritabanı oluşturma
> * PHP uygulamasını MySQL’e bağlama
> * Uygulamayı Azure’da dağıtma
> * Veri modelini güncelleştirme ve uygulamayı yeniden dağıtma
> * Azure’daki tanılama günlüklerinin akışını sağlama
> * Uygulamayı Azure portalında yönetme

Bir web uygulamasına DNS adı eşlemeyle ilgili bilgi edinmek için sonraki öğreticiye geçin.

> [!div class="nextstepaction"]
> [Mevcut bir özel DNS adını Azure Web Apps ile eşleme](../app-service-web-tutorial-custom-domain.md)
