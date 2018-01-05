---
title: "Linux üzerinde Azure App Service'te bir Ruby ve MySQL web uygulaması oluşturma | Microsoft Docs"
description: "Azure MySQL veritabanında bağlantı ile Azure içinde çalışma Söyleniş bir uygulamayı alma hakkında bilgi."
services: app-service\web
documentationcenter: nodejs
author: cephalin
manager: cfowler
ms.service: app-service-web
ms.workload: web
ms.devlang: nodejs
ms.topic: tutorial
ms.date: 12/21/2017
ms.author: cephalin
ms.custom: mvc
ms.openlocfilehash: 03b2b4e8f8827a08e1414512d848bd5bed48d674
ms.sourcegitcommit: df4ddc55b42b593f165d56531f591fdb1e689686
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/04/2018
---
# <a name="build-a-ruby-and-mysql-web-app-in-azure-app-service-on-linux"></a>Linux üzerinde Azure App Service'te bir Ruby ve MySQL web uygulaması oluşturma

[Linux’ta App Service](app-service-linux-intro.md) Linux işletim sistemini kullanan yüksek oranda ölçeklenebilir, otomatik olarak düzeltme eki uygulayan bir web barındırma hizmeti sağlar. Bu öğretici, bir Söyleniş web uygulaması oluşturmak ve MySQL veritabanına bağlanmak gösterilmiştir. İşiniz bittiğinde, gerekir bir [rayları üzerinde Söyleniş](http://rubyonrails.org/) uygulama hizmeti Linux üzerinde çalışan uygulama.

![Azure uygulama Hizmeti'nde çalışan rayları uygulamada Ruby](./media/tutorial-ruby-mysql-app/complete-checkbox-published.png)

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Azure üzerinde MySQL veritabanı oluşturma
> * Ruby rayları uygulamada Mysql'e bağlanmak
> * Uygulamayı Azure'a dağıtma
> * Veri modeli güncelleştirme ve uygulamayı yeniden dağıtın
> * Azure Stream tanılama günlükleri
> * Azure portalında uygulama yönetme

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi tamamlamak için:

* [Git'i yükleyin](https://git-scm.com/)
* [Ruby 2.3 yükleyin](https://www.ruby-lang.org/documentation/installation/)
* [Ruby 5.1 rayları üzerinde yükle](http://guides.rubyonrails.org/v5.1/getting_started.html)
* [Yükleyin ve MySQL başlatın](https://dev.mysql.com/doc/refman/5.7/en/installing.html) 

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prepare-local-mysql"></a>Yerel MySQL hazırlama

Bu adımda, bir veritabanı yerel MySQL sunucunuzu Bu öğreticide, kullanımınız için oluşturun.

### <a name="connect-to-local-mysql-server"></a>Yerel MySQL sunucuya bağlanın

Bir terminal penceresi, yerel MySQL sunucusuna bağlanın. Bu öğreticide tüm komutları çalıştırmak için bu bir terminal penceresi kullanabilirsiniz.

```bash
mysql -u root -p
```

İçin bir parola istenirse parolayı girin `root` hesabı. Kök hesap parolanızı hatırlamıyorsanız bkz [MySQL: kök parolasını sıfırlamak nasıl](https://dev.mysql.com/doc/refman/5.7/en/resetting-permissions.html).

Komutunuzu başarıyla çalışırsa, MySQL sunucunuzu çalışıyor. Aksi takdirde, yerel MySQL server'ınızdaki izleyerek başlatıldığından emin olun [MySQL yükleme sonrası adımları](https://dev.mysql.com/doc/refman/5.7/en/postinstallation.html).

Sunucu bağlantısı yazarak çıkmak `quit`.

```sql
quit
```

<a name="step2"></a>

## <a name="create-a-ruby-on-rails-app-locally"></a>Yerel olarak rayları uygulaması üzerinde bir Ruby oluşturma
Bu adımda, rayları örnek uygulama üzerinde bir Ruby almak, veritabanı bağlantısını yapılandırın ve yerel olarak çalıştırın. 

### <a name="clone-the-sample"></a>Örnek kopyalama

Terminal penceresinde `cd` bir çalışma dizini için.

Örnek depoyu kopyalamak için aşağıdaki komutu çalıştırın.

```bash
git clone https://github.com/Azure-Samples/rubyrails-tasks.git
```

`cd`Kopyalanan dizininize. Gerekli paketleri yükleyeceksiniz.

```bash
cd rubyrails-tasks
bundle install --path vendor/bundle
```

### <a name="configure-mysql-connection"></a>MySQL bağlantısını yapılandırın

Depoda açmak _config/database.yml_ ve yerel MySQL kök kullanıcı adı ve parola (satır 13) girin. MySQL gibi bir araç kullanarak yüklediyseniz [Homebrew](https://brew.sh/), kimlik bilgileri dosyasında zaten varsayılan değerlere ayarlayın, sonra da (olduğu `root` ve boş bir parola).

```txt
default: &default
  adapter: mysql2
  encoding: utf8
  pool: <%= ENV.fetch("RAILS_MAX_THREADS") { 5 } %>
  username: root
  password:
  socket: /tmp/mysql.sock
```

### <a name="run-the-sample-locally"></a>Örnek yerel olarak çalıştırma

Çalıştırma [rayları geçişler](http://guides.rubyonrails.org/active_record_migrations.html#running-migrations) tabloları uygulama oluşturması gerekir. Hangi tablolar geçişleri oluşturulur görmek için konum _db ve geçirmek_ Git deposunda dizin.

```bash
rake db:create
rake db:migrate
```

Uygulamayı çalıştırın.

```bash
rails server
```

Bir tarayıcıda `http://localhost:3000` sayfasına gidin. Bazı görevler sayfasında ekleyin.

![MySQL için Ruby rayları üzerinde başarıyla bağlandığını](./media/tutorial-ruby-mysql-app/mysql-connect-success.png)

Rayları sunucusunu durdurmak için şunu yazın `Ctrl + C` Terminal.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## <a name="create-mysql-in-azure"></a>MySQL oluşturma

Bu adımda oluşturduğunuz MySQL veritabanında [Azure veritabanı için MySQL (Önizleme)](/azure/mysql). Daha sonra bu veritabanına bağlanmak için rayları uygulama üzerinde Söyleniş yapılandırın.

### <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

[!INCLUDE [Create resource group](../../../includes/app-service-web-create-resource-group-no-h.md)] 

### <a name="create-a-mysql-server"></a>MySQL sunucusu oluşturun

MySQL (Önizleme) Azure veritabanındaki bir sunucu oluşturmak [az mysql sunucusu oluşturun](/cli/azure/mysql/server?view=azure-cli-latest#az_mysql_server_create) komutu.

Aşağıdaki komutta, gördüğünüz MySQL server adınızı alternatif  _&lt;mysql_server_name >_ yer tutucu (geçerli karakterler `a-z`, `0-9`, ve `-`). Bu ad MySQL sunucunun ana bilgisayar adı bir parçasıdır (`<mysql_server_name>.mysql.database.azure.com`), genel olarak benzersiz olması gerekir.

```azurecli-interactive
az mysql server create --name <mysql_server_name> --resource-group myResourceGroup --location "North Europe" --admin-user adminuser --admin-password My5up3r$tr0ngPa$w0rd!
```

MySQL sunucusu oluşturulduğunda, Azure CLI bilgileri aşağıdaki örneğe benzer şekilde gösterir:

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

### <a name="configure-server-firewall"></a>Sunucu Güvenlik Duvarı'nı yapılandırma

MySQL sunucunuzu kullanarak istemci bağlantılarına izin verecek şekilde için güvenlik duvarı kuralını [az mysql server güvenlik duvarı kuralı oluşturmak](/cli/azure/mysql/server/firewall-rule?view=azure-cli-latest#az_mysql_server_firewall_rule_create) komutu.

```azurecli-interactive
az mysql server firewall-rule create --name allIPs --server <mysql_server_name> --resource-group myResourceGroup --start-ip-address 0.0.0.0 --end-ip-address 255.255.255.255
```

> [!NOTE]
> Azure veritabanı için MySQL (Önizleme) şu anda yalnızca Azure hizmetlerine bağlantı sayısı sınırı değil. Dinamik olarak atanmış IP adresleri azure'da gibi tüm IP adresleri etkinleştirmek daha iyidir. Hizmet önizlemede değil. Veritabanınızın güvenliğini sağlamak için daha iyi yöntemleri aşağıda verilmiştir.
>

### <a name="connect-to-production-mysql-server-locally"></a>Yerel olarak üretim MySQL sunucusuna bağlan

Terminal penceresinde Azure MySQL sunucusuna bağlanın. Daha önce için belirttiğiniz değerini kullanmak  _&lt;mysql_server_name >_.

```bash
mysql -u adminuser@<mysql_server_name> -h <mysql_server_name>.mysql.database.azure.com -P 3306 -p
```

Kullanmak için bir parola istendiğinde, _My5up3r tr0ngPa$ $w0rd!_, veritabanı sunucusu oluşturduğunuzda belirttiğiniz.

### <a name="create-a-production-database"></a>Bir üretim veritabanı oluşturma

Konumundaki `mysql` isteminde, bir veritabanı oluşturun.

```sql
CREATE DATABASE sampledb;
```

### <a name="create-a-user-with-permissions"></a>İzinleri olan bir kullanıcı oluşturun

Adlı bir veritabanı kullanıcısı oluşturmalıdır _railsappuser_ ve tüm ayrıcalıkları verin `sampledb` veritabanı.

```sql
CREATE USER 'railsappuser' IDENTIFIED BY 'MySQLAzure2017'; 
GRANT ALL PRIVILEGES ON sampledb.* TO 'railsappuser';
```

Sunucu bağlantısı yazarak çıkmak `quit`.

```sql
quit
```

## <a name="connect-app-to-azure-mysql"></a>Azure MySQL uygulamaya Bağlan

Bu adımda, Azure veritabanı'nda (Önizleme) MySQL için oluşturduğunuz MySQL veritabanını rayları uygulamaya üzerinde Ruby bağlayın.

<a name="devconfig"></a>

### <a name="configure-the-database-connection"></a>Veritabanı bağlantısını yapılandır

Depoda açmak _config/database.yml_. Dosyanın sonunda üretim değişkenleri aşağıdaki kodla değiştirin. İçin  _&lt;mysql_server_name >_ yer tutucu, oluşturduğunuz MySQL server adını kullanın.

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
> `sslca` Eklenir ve işaret varolan bir _.pem_ örnek Depo dosyasında. Varsayılan olarak, Azure veritabanı için MySQL istemcilerden gelen SSL bağlantıları zorlar. Bu `.pem` sertifikasıdır nasıl SSL bağlantıları için Azure veritabanı için MySQL yaptığınız. Daha ayrıntılı bilgi için bkz. [MySQL için Azure Veritabanı'na güvenli bir şekilde bağlanmak üzere uygulamanızda SSL bağlantısını yapılandırma](../../mysql/howto-configure-ssl.md).
>

### <a name="test-the-application-locally"></a>Uygulamayı yerel olarak test etme

Yerel terminale aşağıdaki ortam değişkenleri ayarlayın:

```bash
export DB_HOST=<mysql_server_name>.mysql.database.azure.com
export DB_DATABASE=sampledb 
export DB_USERNAME=railsappuser@<mysql_server_name>
export DB_PASSWORD=MySQLAzure2017
```

MySQL (Önizleme), MySQL veritabanınızı Azure veritabanı tabloları oluşturmak için yapılandırdığınız üretim değerlerle rayları veritabanı geçişler çalıştırın. 

```bash
rake db:migrate RAILS_ENV=production
```

Üretim ortamında çalıştırırken rayları uygulamanın önceden derlenmiş varlıklar gerekir. Aşağıdaki komutla gerekli varlıklar oluşturun:

```bash
rake assets:precompile
```

Rayları üretim ortamına gizli güvenliği yönetmek için de kullanır. Gizli bir anahtar oluşturun.

```bash
rails secret
```

Gizli anahtar rayları üretim ortamı tarafından kullanılan ilgili değişkenlerini kaydedin. Kolaylık olması için her iki değişken için aynı anahtar kullanılır.

```bash
export RAILS_MASTER_KEY=<output_of_rails_secret>
export SECRET_KEY_BASE=<output_of_rails_secret>
```

JavaScript ve CSS dosyalara hizmet rayları üretim ortamı sağlar.

```bash
export RAILS_SERVE_STATIC_FILES=true
```

Üretim ortamında örnek uygulamayı çalıştırın.

```bash
rails server -e production
```

Gidin `http://localhost:3000`. Sayfa hatasız yüklerse, Ruby rayları uygulama üzerinde Azure MySQL veritabanına bağlanıyor.

Bazı görevler sayfasında ekleyin.

![Ruby rayları üzerinde Azure veritabanına başarıyla MySQL (Önizleme) bağlanır.](./media/tutorial-ruby-mysql-app/azure-mysql-connect-success.png)

Rayları sunucusunu durdurmak için şunu yazın `Ctrl + C` Terminal.

### <a name="commit-your-changes"></a>Değişikliklerinizi uygulamak

Değişikliklerinizi kaydetmek için aşağıdaki Git komutları çalıştırın:

```bash
git add .
git commit -m "database.yml updates"
```

Uygulamanız dağıtılmaya hazırdır.

## <a name="deploy-to-azure"></a>Azure’a dağıtma

Bu adımda, Azure App Service MySQL bağlı rayları uygulamayı dağıtın.

### <a name="configure-a-deployment-user"></a>Dağıtım kullanıcısı yapılandırma

[!INCLUDE [Configure deployment user](../../../includes/configure-deployment-user-no-h.md)]

### <a name="create-an-app-service-plan"></a>App Service planı oluşturma

[!INCLUDE [Create app service plan no h](../../../includes/app-service-web-create-app-service-plan-linux-no-h.md)]

### <a name="create-a-web-app"></a>Web uygulaması oluşturma

Cloud Shell’de, [az webapp create](/cli/azure/webapp?view=azure-cli-latest#az_webapp_create) komutuyla `myAppServicePlan` App Service planında bir web uygulaması oluşturun. 

Aşağıdaki örnekte `<app_name>` kısmını genel olarak benzersiz bir uygulama adıyla değiştirin (geçerli karakterler `a-z`, `0-9` ve `-` şeklindedir). Çalışma zamanı kümesine `RUBY|2.3`, dağıtan [Söyleniş görüntü varsayılan](https://hub.docker.com/r/appsvc/ruby/). Desteklenen tüm çalışma zamanlarını görmek için, [az webapp list-runtimes](/cli/azure/webapp?view=azure-cli-latest#az_webapp_list_runtimes) komutunu çalıştırın. 

```azurecli-interactive
az webapp create --resource-group myResourceGroup --plan myAppServicePlan --name <app_name> --runtime "RUBY|2.3" --deployment-local-git
```

Web uygulaması oluşturulduğunda Azure CLI aşağıda yer alan çıktıdaki gibi bilgiler gösterir:

```json
Local git is configured with url of 'https://<username>@<app_name>.scm.azurewebsites.net/<app_name>.git'
{
  "availabilityState": "Normal",
  "clientAffinityEnabled": true,
  "clientCertEnabled": false,
  "cloningInfo": null,
  "containerSize": 0,
  "dailyMemoryTimeQuota": 0,
  "defaultHostName": "<app_name>.azurewebsites.net",
  "deploymentLocalGitUrl": "https://<username>@<app_name>.scm.azurewebsites.net/<app_name>.git",
  "enabled": true,
  < JSON data removed for brevity. >
}
```

Git dağıtımı etkin boş bir yeni web uygulaması oluşturdunuz.

> [!NOTE]
> Git uzak URL’si `deploymentLocalGitUrl` özelliği içinde `https://<username>@<app_name>.scm.azurewebsites.net/<app_name>.git` biçiminde gösterilir. Bu URL’ye daha sonra ihtiyacınız olacağı için URL’yi kaydedin.
>

### <a name="configure-database-settings"></a>Veritabanı ayarlarını yapılandırma

App Service'te olarak ortam değişkenlerini ayarlama _uygulama ayarları_ kullanarak [az webapp config appsettings kümesi](/cli/azure/webapp/config/appsettings?view=azure-cli-latest#az_webapp_config_appsettings_set) bulut Kabuğu'nda komutu.

Aşağıdaki bulut Kabuk komut uygulama ayarlarını yapılandırır `DB_HOST`, `DB_DATABASE`, `DB_USERNAME`, ve `DB_PASSWORD`. Yer tutucuları değiştirmek  _&lt;uygulamaadı >_ ve  _&lt;mysql_server_name >_.

```azurecli-interactive
az webapp config appsettings set --name <app_name> --resource-group myResourceGroup --settings DB_HOST="<mysql_server_name>.mysql.database.azure.com" DB_DATABASE="sampledb" DB_USERNAME="railsappuser@<mysql_server_name>" DB_PASSWORD="MySQLAzure2017"
```

### <a name="configure-rails-environment-variables"></a>Rayları ortam değişkenlerini yapılandırın

Yerel terminale Azure rayları üretim ortamında için yeni bir gizli anahtar oluşturur.

```bash
rails secret
```

Rayları üretim ortamı tarafından gerek değişkenleri yapılandırın.

Aşağıdaki bulut Kabuk iki komutta,  _&lt;output_of_rails_secret >_ yer tutucularını yerel terminale oluşturulan yeni gizli anahtar.

```azurecli-interactive
az webapp config appsettings set --name <app_name> --resource-group myResourceGroup --settings RAILS_MASTER_KEY="<output_of_rails_secret>" SECRET_KEY_BASE="<output_of_rails_secret>" RAILS_SERVE_STATIC_FILES="true" ASSETS_PRECOMPILE="true"
```

`ASSETS_PRECOMPILE="true"`Her Git dağıtımı varlıklar derleneceği Söyleniş kapsayıcı varsayılan söyler.

### <a name="push-to-azure-from-git"></a>Git üzerinden Azure'a gönderme

Yerel terminale Azure uzaktan yerel Git deponuzu ekleyin.

```bash
git remote add azure <paste_copied_url_here>
```

Azure'a Ruby rayları uygulama dağıtmak için uzaktan gönderin. Daha önce dağıtım kullanıcı oluşturmanın bir parçası sağlanan parola istenir.

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

### <a name="browse-to-the-azure-web-app"></a>Azure web uygulaması'na göz atın

Gözat `http://<app_name>.azurewebsites.net` ve birkaç görevi listesine ekleyin.

![Azure uygulama Hizmeti'nde çalışan rayları uygulamada Ruby](./media/tutorial-ruby-mysql-app/ruby-mysql-in-azure.png)

Tebrikler, Azure App Service'in rayları uygulamasında veri güdümlü bir Ruby çalıştırıyorsanız.

## <a name="update-model-locally-and-redeploy"></a>Modeli yerel olarak güncelleştirin ve yeniden dağıtın

Bu adımda, bir basit değişiklik `task` veri modeli ve webapp ve güncelleştirme Azure'a yayımlayabilirsiniz.

Görevler senaryo için bir görev tamamlandı olarak işaretleyebilirsiniz şekilde uygulama değiştirin.

### <a name="add-a-column"></a>Sütun ekle

Terminale Git deposu kök dizinine gidin.

Adlı bir boolean sütunu ekler yeni bir geçiş oluşturmayı `Done` için `Tasks` tablosu:

```bash
rails generate migration add_Done_to_Tasks Done:boolean
```

Bu komut, yeni bir geçiş dosyada oluşturur _db ve geçirmek_ dizin.


Terminale yerel veritabanında değişiklik yapmak için rayları veritabanı geçiş çalıştırın.

```bash
rake db:migrate
```

### <a name="update-application-logic"></a>Uygulama mantığını güncelleştir

Açık *app/controllers/tasks_controller.rb* dosya. Dosyanın sonunda, aşağıdaki satırı bulun:

```rb
params.require(:task).permit(:Description)
```

Yeni dahil etmek için bu satırı değiştirin `Done` parametresi.

```rb
params.require(:task).permit(:Description, :Done)
```

### <a name="update-the-views"></a>Görünümleri güncelleştirme

Açık *app/views/tasks/_form.html.erb* düzenleme form dosya.

Satırı bulun `<%=f.error_span(:Description) %>` ve hemen altına aşağıdaki kodu ekleyin:

```erb
<%= f.label :Done, :class => 'control-label col-lg-2' %>
<div class="col-lg-10">
  <%= f.check_box :Done, :class => 'form-control' %>
</div>
```

Açık *app/views/tasks/show.html.erb* tek kayıtlı görünüm sayfası dosyası. 

Satırı bulun `<dd><%= @task.Description %></dd>` ve hemen altına aşağıdaki kodu ekleyin:

```erb
  <dt><strong><%= model_class.human_attribute_name(:Done) %>:</strong></dt>
  <dd><%= check_box "task", "Done", {:checked => @task.Done, :disabled => true}%></dd>
```

Açık *app/views/tasks/index.html.erb* tüm kayıtlar için dizin sayfası dosyası.

Satırı bulun `<th><%= model_class.human_attribute_name(:Description) %></th>` ve hemen altına aşağıdaki kodu ekleyin:

```erb
<th><%= model_class.human_attribute_name(:Done) %></th>
```

Aynı dosyada satırı bulun `<td><%= task.Description %></td>` ve hemen altına aşağıdaki kodu ekleyin:

```erb
<td><%= check_box "task", "Done", {:checked => task.Done, :disabled => true} %></td>
```

### <a name="test-the-changes-locally"></a>Değişiklikleri yerel olarak test etme

Yerel terminale rayları sunucunun çalıştırın.

```bash
rails server
```

Değiştirme, gidin görev durumunu görmek için `http://localhost:3000` ve öğeleri ekleme veya düzenleme.

![Görev için eklenen onay kutusu](./media/tutorial-ruby-mysql-app/complete-checkbox.png)

Rayları sunucusunu durdurmak için şunu yazın `Ctrl + C` Terminal.

### <a name="publish-changes-to-azure"></a>Değişiklikler için Azure yayımlama

Terminale rayları veritabanı geçişler Azure veritabanında değişiklik yapmak üretim ortamı için çalıştırın.

```bash
rake db:migrate RAILS_ENV=production
```

Git tüm değişiklikleri uygulayın ve ardından kod değişiklikleri Azure'a gönderin.

```bash
git add .
git commit -m "added complete checkbox"
git push azure master
```

Bir kez `git push` tamamlamak, Azure web uygulaması'na gidin ve yeni işlevselliğini test etmek içindir.

![Azure için yayımlanan modeli ve veritabanı değişiklikleri](media/tutorial-ruby-mysql-app/complete-checkbox-published.png)

Herhangi bir görevi eklediyseniz bunlar veritabanında tutulur. Veri şema güncelleştirmeleri varolan veriler olduğu gibi bırakın.

## <a name="manage-the-azure-web-app"></a>Azure web uygulamasını yönetme

Oluşturduğunuz web uygulamasını yönetmek için [Azure portalına](https://portal.azure.com) gidin.

Sol menüden **Uygulama Hizmetleri**'ne ve ardından Azure web uygulamanızın adına tıklayın.

![Portaldan Azure web uygulamasına gitme](./media/tutorial-php-mysql-app/access-portal.png)

Web uygulamanızın Genel Bakış sayfasını görürsünüz. Burada, Durdur, Başlat, yeniden başlatma, Gözat ve silme gibi temel yönetim görevlerini gerçekleştirebilirsiniz.

Soldaki menüden, uygulamanızı yapılandırmak için sayfaları sağlar.

![Azure portalında App Service sayfası](./media/tutorial-php-mysql-app/web-app-blade.png)

[!INCLUDE [cli-samples-clean-up](../../../includes/cli-samples-clean-up.md)]

<a name="next"></a>

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Azure üzerinde MySQL veritabanı oluşturma
> * Ruby rayları uygulamada Mysql'e bağlanmak
> * Uygulamayı Azure'a dağıtma
> * Veri modeli güncelleştirme ve uygulamayı yeniden dağıtın
> * Azure Stream tanılama günlükleri
> * Azure portalında uygulama yönetme

Bir web uygulaması için özel bir DNS adı eşleme öğrenmek için sonraki öğretici ilerleyin.

> [!div class="nextstepaction"]
> [Mevcut bir özel DNS adını Azure Web Apps ile eşleme](../app-service-web-tutorial-custom-domain.md)
