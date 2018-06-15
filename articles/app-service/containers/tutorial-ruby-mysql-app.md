---
title: Linux üzerinde Azure App Service’te bir Ruby ve MySQL web uygulaması derleme | Microsoft Docs
description: Azure’da çalışan ve bir MySQL veritabanı ile bağlantısı olan Ruby uygulamasını nasıl edinebileceğinizi öğrenin.
services: app-service\web
documentationcenter: ''
author: cephalin
manager: cfowler
ms.service: app-service-web
ms.workload: web
ms.devlang: ruby
ms.topic: tutorial
ms.date: 12/21/2017
ms.author: cephalin
ms.custom: mvc
ms.openlocfilehash: a026eafeb71c67a2cb98c20c4fc5af16073be083
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2018
ms.locfileid: "31789104"
---
# <a name="build-a-ruby-and-mysql-web-app-in-azure-app-service-on-linux"></a>Linux üzerinde Azure App Service’te bir Ruby ve MySQL web uygulaması derleme

[Linux’ta App Service](app-service-linux-intro.md) Linux işletim sistemini kullanan yüksek oranda ölçeklenebilir, otomatik olarak düzeltme eki uygulayan bir web barındırma hizmeti sağlar. Bu öğreticide, bir Ruby web uygulaması oluşturma ve bu uygulamayı bir MySQL veritabanına bağlamayla ilgili yönergeler verilmiştir. İşiniz bittiğinde, Linux üzerinde App Service’te çalışan bir [Ruby on Rails](http://rubyonrails.org/) uygulamasına sahip olacaksınız.

![Azure App Service'te çalışan Ruby on Rails uygulaması](./media/tutorial-ruby-mysql-app/complete-checkbox-published.png)

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Azure’da MySQL veritabanı oluşturma
> * Ruby on Rails uygulamasını MySQL'e bağlama
> * Uygulamayı Azure’da dağıtma
> * Veri modelini güncelleştirme ve uygulamayı yeniden dağıtma
> * Azure’daki tanılama günlüklerinin akışını sağlama
> * Uygulamayı Azure portalında yönetme

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamak için:

* [Git'i yükleyin](https://git-scm.com/)
* [Ruby 2.3'ü yükleyin](https://www.ruby-lang.org/documentation/installation/)
* [Ruby on Rails 5.1'i yükleyin](http://guides.rubyonrails.org/v5.1/getting_started.html)
* [MySQL'i yükleyin ve başlatın](https://dev.mysql.com/doc/refman/5.7/en/installing.html) 

## <a name="prepare-local-mysql"></a>Yerel MySQL hazırlama

Bu adımda, bu öğreticide kullanmak üzere yerel MySQL sunucunuzda bir veritabanı oluşturursunuz.

### <a name="connect-to-local-mysql-server"></a>Yerel MySQL sunucusuna bağlanma

Bir terminal penceresinde yerel MySQL sunucunuza bağlanın. Bu öğreticideki tüm komutları çalıştırmak için bu terminal penceresini kullanabilirsiniz.

```bash
mysql -u root -p
```

Parola istenirse `root` hesabının parolasını girin. Kök hesap parolanızı hatırlamıyorsanız bkz [MySQL: Kök Parolayı Sıfırlama](https://dev.mysql.com/doc/refman/5.7/en/resetting-permissions.html).

Komutunuz başarıyla çalışırsa, MySQL sunucunuz çalışıyor demektir. Çalışmıyorsa, yerel MySQL sunucunuzun aşağıdaki [MySQL yükleme sonrası adımları](https://dev.mysql.com/doc/refman/5.7/en/postinstallation.html) kullanılarak başlatıldığından emin olun.

`quit` yazarak sunucu bağlantınızdan çıkış yapın.

```sql
quit
```

<a name="step2"></a>

## <a name="create-a-ruby-on-rails-app-locally"></a>Yerel olarak bir Ruby on Rails uygulaması oluşturma
Bu adımda Ruby on Rails örnek uygulaması edinir, veritabanı bağlantısını yapılandırır ve yerel olarak çalıştırırsınız. 

### <a name="clone-the-sample"></a>Örneği kopyalama

Terminal penceresinde, `cd` ile bir çalışma dizinine gidin.

Örnek depoyu kopyalamak için aşağıdaki komutu çalıştırın.

```bash
git clone https://github.com/Azure-Samples/rubyrails-tasks.git
```

`cd` komutuyla kopyalanmış dizininize geçin. Gereken paketleri yükleyin.

```bash
cd rubyrails-tasks
bundle install --path vendor/bundle
```

### <a name="configure-mysql-connection"></a>MySQL bağlantısını yapılandırma

Depoda, _config/database.yml_ dosyasını açın ve yerel MySQL kök kullanıcı adını ve parolasını sağlayın (13. satır). MySQL'i [Homebrew](https://brew.sh/) gibi bir araç kullanarak yüklediyseniz, dosyadaki kimlik bilgileri zaten varsayılan değerlere ayarlanmıştır (bunlar `root` ve boş bir paroladır).

```txt
default: &default
  adapter: mysql2
  encoding: utf8
  pool: <%= ENV.fetch("RAILS_MAX_THREADS") { 5 } %>
  username: root
  password:
  socket: /tmp/mysql.sock
```

### <a name="run-the-sample-locally"></a>Örneği yerel olarak çalıştırma

Uygulama için gereken tabloları oluşturmak üzere [Rails geçişlerini](http://guides.rubyonrails.org/active_record_migrations.html#running-migrations) çalıştırın. Geçişlerde hangi tabloların oluşturulduğunu görmek için, Git deposundaki _db/migrate_ dizinine bakın.

```bash
rake db:create
rake db:migrate
```

Uygulamayı çalıştırın.

```bash
rails server
```

Bir tarayıcıda `http://localhost:3000` sayfasına gidin. Sayfaya birkaç görev ekleyin.

![Ruby on Rails başarıyla MySQL’e bağlanır](./media/tutorial-ruby-mysql-app/mysql-connect-success.png)

Rails sunucusunu durdurmak için terminale `Ctrl + C` yazın.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## <a name="create-mysql-in-azure"></a>Azure’da MySQL oluşturma

Bu adımda, [MySQL için Azure Veritabanı (Önizleme)](/azure/mysql) içinde bir MySQL veritabanı oluşturursunuz. Daha sonra, Ruby on Rails uygulamasını bu veritabanına bağlanacak şekilde yapılandırırsınız.

### <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

[!INCLUDE [Create resource group](../../../includes/app-service-web-create-resource-group-linux-no-h.md)] 

### <a name="create-a-mysql-server"></a>MySQL sunucusu oluşturma

MySQL için Azure Veritabanı (Önizleme) içinde [`az mysql server create`](/cli/azure/mysql/server?view=azure-cli-latest#az_mysql_server_create) komutu ile bir sunucu oluşturun.

Aşağıdaki komutta, _&lt;mysql_server_name>_ yer tutucusunu gördüğünüz yerde MySQL sunucunuzun adını değiştirin (geçerli karakterler `a-z`, `0-9` ve `-`). Bu ad, MySQL sunucusu ana bilgisayar adının (`<mysql_server_name>.mysql.database.azure.com`) bir parçasıdır ve genel olarak benzersiz olması gerekir.

```azurecli-interactive
az mysql server create --name <mysql_server_name> --resource-group myResourceGroup --location "North Europe" --admin-user adminuser --admin-password My5up3r$tr0ngPa$w0rd!
```

MySQL sunucusu oluşturulduğunda Azure CLI, aşağıdaki örneğe benzer bilgiler gösterir:

```json
{
  "administratorLogin": "adminuser",
  "administratorLoginPassword": null,
  "fullyQualifiedDomainName": "<mysql_server_name>.database.mysql.database.azure.com",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.DBforMySQL/servers/<mysql_server_name>",
  "location": "northeurope",
  "name": "<mysql_server_name>",
  "resourceGroup": "myResourceGroup",
  ...
}
```

### <a name="configure-server-firewall"></a>Sunucu güvenlik duvarını yapılandırma

[`az mysql server firewall-rule create`](/cli/azure/mysql/server/firewall-rule?view=azure-cli-latest#az_mysql_server_firewall_rule_create) komutunu kullanarak MySQL sunucunuzun istemci bağlantılarına izin vermesi için bir güvenlik duvarı kuralı oluşturun. Hem başlangıç hem bitiş IP’si 0.0.0.0 olarak ayarlandığında, güvenlik duvarı yalnızca diğer Azure kaynakları için açılır. 

```azurecli-interactive
az mysql server firewall-rule create --name allAzureIPs --server <mysql_server_name> --resource-group myResourceGroup --start-ip-address 0.0.0.0 --end-ip-address 0.0.0.0
```

> [!TIP] 
> [Yalnızca uygulamanızın kullandığı giden IP adreslerini kullanarak](../app-service-ip-addresses.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json#find-outbound-ips) güvenlik duvarı kurallarınızda daha da kısıtlayıcı olabilirsiniz.
>

### <a name="connect-to-production-mysql-server-locally"></a>Üretim MySQL sunucusuna yerel olarak bağlanma

Terminal penceresinde, Azure’da MySQL sunucusuna bağlanın. Daha önce _&lt;mysql_server_name>_ için belirttiğiniz değeri kullanın.

```bash
mysql -u adminuser@<mysql_server_name> -h <mysql_server_name>.mysql.database.azure.com -P 3306 -p
```

Parola sorulduğunda, veritabanı sunucusunu oluştururken belirttiğiniz _My5up3r$tr0ngPa$w0rd!_ parolasını kullanın.

### <a name="create-a-production-database"></a>Üretim veritabanı oluşturma

`mysql` isteminde bir veritabanı oluşturun.

```sql
CREATE DATABASE sampledb;
```

### <a name="create-a-user-with-permissions"></a>İzinleri olan bir kullanıcı oluşturma

_railsappuser_ adlı bir veritabanı kullanıcısı oluşturun ve bu kullanıcıya `sampledb` veritabanındaki tüm ayrıcalıkları verin.

```sql
CREATE USER 'railsappuser' IDENTIFIED BY 'MySQLAzure2017'; 
GRANT ALL PRIVILEGES ON sampledb.* TO 'railsappuser';
```

`quit` yazarak sunucu bağlantısından çıkış yapın.

```sql
quit
```

## <a name="connect-app-to-azure-mysql"></a>Uygulamayı Azure MySQL’e bağlama

Bu adımda, Ruby on Rails uygulamasını MySQL için Azure Veritabanı (Önizleme) içinde oluşturduğunuz MySQL veritabanına bağlarsınız.

<a name="devconfig"></a>

### <a name="configure-the-database-connection"></a>Veritabanı bağlantısını yapılandırma

Depoda, _config/database.yml_ dosyasını açın. Dosyanın en altında, üretim değişkenlerini aşağıdaki kodla değiştirin. _&lt;mysql_server_name>_ yer tutucusu için, oluşturduğunuz MySQL sunucusunun adını kullanın.

```txt
production:
  <<: *default
  host: <%= ENV['DB_HOST'] %>
  port: 3306
  database: <%= ENV['DB_DATABASE'] %>
  username: <%= ENV['DB_USERNAME'] %>
  password: <%= ENV['DB_PASSWORD'] %>
  sslca: ssl/BaltimoreCyberTrustRoot.crt.pem
```

Değişiklikleri kaydedin.

> [!NOTE]
> `sslca` eklenir ve aynı depodaki mevcut bir _.pem_ dosyasına işaret eder. Varsayılan olarak, MySQL için Azure Veritabanı, istemcilerden gelen SSL bağlantılarını zorlar. MySQL için Azure Veritabanı'na SSL bağlantılarını bu `.pem` sertifikasıyla kurarsınız. Daha ayrıntılı bilgi için bkz. [MySQL için Azure Veritabanı'na güvenli bir şekilde bağlanmak üzere uygulamanızda SSL bağlantısını yapılandırma](../../mysql/howto-configure-ssl.md).
>

### <a name="test-the-application-locally"></a>Uygulamayı yerel olarak test etme

Yerel terminalde, aşağıdaki ortam değişkenlerini ayarlayın:

```bash
export DB_HOST=<mysql_server_name>.mysql.database.azure.com
export DB_DATABASE=sampledb 
export DB_USERNAME=railsappuser@<mysql_server_name>
export DB_PASSWORD=MySQLAzure2017
```

MySQL için Azure Veritabanı (Önizleme) içinde tablolar oluşturmak için, Rails veritabanı geçişlerini yeni yapılandırdığınız üretim değerleriyle çalıştırın. 

```bash
rake db:migrate RAILS_ENV=production
```

Üretim ortamında çalıştırırken, Rails uygulamasına önceden derlenmiş varlıklar gerekir. Aşağıdaki komutu kullanarak gerekli varlıkları oluşturun:

```bash
rake assets:precompile
```

Ayrıca Rails üretim ortamı güvenliği yönetmek için gizli diziler kullanır. Gizli anahtar oluşturun.

```bash
rails secret
```

Gizli anahtarı, Rails üretim ortamı tarafından kullanılan ilgili değişkenlere kaydedin. Kolaylık olması için, her iki değişken için de aynı anahtarı kullanırsınız.

```bash
export RAILS_MASTER_KEY=<output_of_rails_secret>
export SECRET_KEY_BASE=<output_of_rails_secret>
```

JavaScript ve CSS dosyalarına hizmet vermek için Rails üretim ortamını etkinleştirin.

```bash
export RAILS_SERVE_STATIC_FILES=true
```

Örnek uygulamayı üretim ortamında çalıştırın.

```bash
rails server -e production
```

`http://localhost:3000` sayfasına gidin. Sayfa hatasız yüklenirse, Ruby on Rails uygulaması Azure’da MySQL veritabanına bağlanıyor demektir.

Sayfaya birkaç görev ekleyin.

![Ruby on Rails, MySQL için Azure Veritabanına (Önizleme) başarıyla bağlanır](./media/tutorial-ruby-mysql-app/azure-mysql-connect-success.png)

Rails sunucusunu durdurmak için terminale `Ctrl + C` yazın.

### <a name="commit-your-changes"></a>Değişikliklerinizi kaydetme

Değişikliklerinizi kaydetmek için aşağıdaki Git komutlarını çalıştırın:

```bash
git add .
git commit -m "database.yml updates"
```

Uygulamanız dağıtılmaya hazırdır.

## <a name="deploy-to-azure"></a>Azure’a dağıtma

Bu adımda, MySQL’e bağlı Rails uygulamasını Azure App Service'e dağıtırsınız.

### <a name="configure-a-deployment-user"></a>Dağıtım kullanıcısı yapılandırma

[!INCLUDE [Configure deployment user](../../../includes/configure-deployment-user-no-h.md)]

### <a name="create-an-app-service-plan"></a>App Service planı oluşturma

[!INCLUDE [Create app service plan no h](../../../includes/app-service-web-create-app-service-plan-linux-no-h.md)]

### <a name="create-a-web-app"></a>Web uygulaması oluşturma

[!INCLUDE [Create web app](../../../includes/app-service-web-create-web-app-ruby-linux-no-h.md)] 

### <a name="configure-database-settings"></a>Veritabanı ayarlarını yapılandırma

App Service’te, Cloud Shell'de [`az webapp config appsettings set`](/cli/azure/webapp/config/appsettings?view=azure-cli-latest#az_webapp_config_appsettings_set) komutunu kullanarak ortam değişkenlerini _uygulama ayarları_ olarak ayarlayabilirsiniz.

Aşağıdaki Cloud Shell komutu `DB_HOST`, `DB_DATABASE`, `DB_USERNAME` ve `DB_PASSWORD` uygulama ayarlarını yapılandırır. _&lt;appname>_ ve _&lt;mysql_server_name>_ yer tutucularını değiştirin.

```azurecli-interactive
az webapp config appsettings set --name <app_name> --resource-group myResourceGroup --settings DB_HOST="<mysql_server_name>.mysql.database.azure.com" DB_DATABASE="sampledb" DB_USERNAME="railsappuser@<mysql_server_name>" DB_PASSWORD="MySQLAzure2017"
```

### <a name="configure-rails-environment-variables"></a>Rails ortam değişkenlerini yapılandırma

Yerel terminalde, Azure'da Rails üretim ortamı için yeni bir gizli anahtar oluşturun.

```bash
rails secret
```

Rails üretim ortamına gereken değişkenleri yapılandırın.

Aşağıdaki Cloud Shell komutunda, iki _&lt;output_of_rails_secret>_ yer tutucusunu yerel terminalde oluşturduğunuz yeni gizli anahtarla değiştirin.

```azurecli-interactive
az webapp config appsettings set --name <app_name> --resource-group myResourceGroup --settings RAILS_MASTER_KEY="<output_of_rails_secret>" SECRET_KEY_BASE="<output_of_rails_secret>" RAILS_SERVE_STATIC_FILES="true" ASSETS_PRECOMPILE="true"
```

`ASSETS_PRECOMPILE="true"`, varsayılan Ruby kapsayıcısına her Git dağıtımında varlıkları yeniden derlemesini bildirir.

### <a name="push-to-azure-from-git"></a>Git üzerinden Azure'a gönderme

Yerel terminalde yerel Git deponuza bir Azure uzak deposu ekleyin.

```bash
git remote add azure <paste_copied_url_here>
```

Ruby on Rails uygulamasını dağıtmak için Azure uzak deposuna gönderin. Daha önce dağıtım kullanıcısı oluştururken belirttiğiniz parola istenir.

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

### <a name="browse-to-the-azure-web-app"></a>Azure web uygulamasına göz atma

`http://<app_name>.azurewebsites.net` listesine göz atın ve listeye birkaç görev ekleyin.

![Azure App Service'te çalışan Ruby on Rails uygulaması](./media/tutorial-ruby-mysql-app/ruby-mysql-in-azure.png)

Tebrikler, Azure App Service'te veri temelli bir Ruby on Rails uygulaması çalıştırıyorsunuz.

## <a name="update-model-locally-and-redeploy"></a>Modeli yerel olarak güncelleştirme ve yeniden dağıtma

Bu adımda, `task` veri modeli ve web uygulamasında basit bir değişiklik yaptıktan sonra güncelleştirmeyi Azure'da yayımlarsınız.

Görevler senaryosu için, görevi tamamlandı olarak işaretleyebileceğiniz şekilde uygulamayı değiştirirsiniz.

### <a name="add-a-column"></a>Sütun ekleme

Terminalde Git deposunun kök dizinine gidin.

`Tasks` tablosuna `Done` adlı bir Boole sütunu ekleyen yeni bir geçiş oluşturun:

```bash
rails generate migration add_Done_to_Tasks Done:boolean
```

Bu komut yeni geçiş dosyasını _db/migrate_ dizininde oluşturur.


Terminalde, Rails veritabanı geçişlerini çalıştırarak yerel veritabanında değişikliği yapın.

```bash
rake db:migrate
```

### <a name="update-application-logic"></a>Uygulama mantığını güncelleştirme

*app/controllers/tasks_controller.rb* dosyasını açın. Dosyanın en sonunda şu satırı bulun:

```rb
params.require(:task).permit(:Description)
```

Bu satırı, yeni `Done` parametresini içerecek şekilde değiştirin.

```rb
params.require(:task).permit(:Description, :Done)
```

### <a name="update-the-views"></a>Görünümleri güncelleştirme

Düzenleme formu olan *app/views/tasks/_form.html.erb* dosyasını açın.

`<%=f.error_span(:Description) %>` satırını bulun ve bu satırın hemen altına aşağıdaki kodu ekleyin:

```erb
<%= f.label :Done, :class => 'control-label col-lg-2' %>
<div class="col-lg-10">
  <%= f.check_box :Done, :class => 'form-control' %>
</div>
```

Tek kayıtlı Görünüm sayfası olan *app/views/tasks/show.html.erb* dosyasını açın. 

`<dd><%= @task.Description %></dd>` satırını bulun ve bu satırın hemen altına aşağıdaki kodu ekleyin:

```erb
  <dt><strong><%= model_class.human_attribute_name(:Done) %>:</strong></dt>
  <dd><%= check_box "task", "Done", {:checked => @task.Done, :disabled => true}%></dd>
```

Tüm kayıtların dizin sayfası olan *app/views/tasks/index.html.erb* dosyasını açın.

`<th><%= model_class.human_attribute_name(:Description) %></th>` satırını bulun ve bu satırın hemen altına aşağıdaki kodu ekleyin:

```erb
<th><%= model_class.human_attribute_name(:Done) %></th>
```

Aynı dosyada, `<td><%= task.Description %></td>` satırını bulun ve bu satırın hemen altına aşağıdaki kodu ekleyin:

```erb
<td><%= check_box "task", "Done", {:checked => task.Done, :disabled => true} %></td>
```

### <a name="test-the-changes-locally"></a>Değişiklikleri yerel olarak test etme

Yerel terminalde Rails sunucusunu çalıştırın.

```bash
rails server
```

Görev durumu değişikliğini görmek için `http://localhost:3000` öğesine gidip öğeleri ekleyin veya düzenleyin.

![Göreve eklenen onay kutusu](./media/tutorial-ruby-mysql-app/complete-checkbox.png)

Rails sunucusunu durdurmak için terminale `Ctrl + C` yazın.

### <a name="publish-changes-to-azure"></a>Değişiklikleri Azure’da yayımlama

Terminalde, üretim ortamı için Rails veritabanı geçişlerini çalıştırarak değişikliği Azure veritabanında yapın.

```bash
rake db:migrate RAILS_ENV=production
```

Tüm değişiklikleri Git’e kaydedin ve ardından kod değişikliklerini Azure’a gönderin.

```bash
git add .
git commit -m "added complete checkbox"
git push azure master
```

`git push` tamamlandığında, Azure web uygulamasına gidin ve yeni işlevleri test edin.

![Azure’da yayımlanan model ve veritabanı değişiklikleri](media/tutorial-ruby-mysql-app/complete-checkbox-published.png)

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
> * Ruby on Rails uygulamasını MySQL'e bağlama
> * Uygulamayı Azure’da dağıtma
> * Veri modelini güncelleştirme ve uygulamayı yeniden dağıtma
> * Azure’daki tanılama günlüklerinin akışını sağlama
> * Uygulamayı Azure portalında yönetme

Bir web uygulamasına DNS adı eşlemeyle ilgili bilgi edinmek için sonraki öğreticiye geçin.

> [!div class="nextstepaction"]
> [Mevcut bir özel DNS adını Azure Web Apps ile eşleme](../app-service-web-tutorial-custom-domain.md)
