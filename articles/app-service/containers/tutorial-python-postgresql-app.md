---
title: Azure App Service'te Python ve PostgreSQL web uygulaması oluşturma | Microsoft Docs
description: Azure'da bir PostgreSQL veritabanına bağlantısı olan veri temelli bir Python uygulamasını nasıl çalıştıracağınızı öğrenin.
services: app-service\web
documentationcenter: python
author: cephalin
manager: jeconnoc
ms.service: app-service-web
ms.workload: web
ms.devlang: python
ms.topic: tutorial
ms.date: 09/28/2018
ms.author: beverst;cephalin
ms.custom: mvc
ms.openlocfilehash: f4ce197d541b8573e38fd85dcebb575c8ee99f59
ms.sourcegitcommit: 7c4fd6fe267f79e760dc9aa8b432caa03d34615d
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/28/2018
ms.locfileid: "47435809"
---
# <a name="build-a-docker-python-and-postgresql-web-app-in-azure"></a>Azure'da Docker Python ve PostgreSQL web uygulaması oluşturma

[Linux’ta App Service](app-service-linux-intro.md) yüksek oranda ölçeklenebilen, kendi kendine düzeltme eki uygulayan bir web barındırma hizmeti sunar. Bu öğreticide veritabanı arka ucu olarak PostgreSQL kullanan veri temelli bir Python web uygulamasının nasıl oluşturulacağı gösterilmektedir. İşiniz bittiğinde, Linux üzerinde App Service’te bir Docker kapsayıcısı içinde çalışan bir Python Flask uygulamanız olur.

![Linux üzerinde App Service’te Docker Python Flask uygulaması](./media/tutorial-python-postgresql-app/docker-flask-in-azure.png)

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Azure’da PostgreSQL veritabanı oluşturma
> * Python uygulamasını PostgreSQL’e bağlama
> * Uygulamayı Azure’da dağıtma
> * Tanılama günlüklerini görüntüleme
> * Veri modelini güncelleştirme ve uygulamayı yeniden dağıtma
> * Uygulamayı Azure portalında yönetme

Bu makaledeki adımları macOS üzerinde izleyebilirsiniz. Linux ve Windows yönergeleri çoğu durumda aynıdır, ancak bu öğreticide farkları konusunda ayrıntıya girilmemiştir.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamak için:

1. [Git'i yükleyin](https://git-scm.com/)
1. [Python'ı yükleyin](https://www.python.org/downloads/)
1. [PostgreSQL’i yükleyin ve çalıştırın](https://www.postgresql.org/download/)

## <a name="test-local-postgresql-installation-and-create-a-database"></a>Yerel PostgreSQL yüklemesini test etme ve bir veritabanı oluşturma

Yerel bir terminal penceresinde `psql` komutunu çalıştırarak yerel PostgreSQL sunucunuza bağlanın.

```bash
sudo -u postgres psql postgres
```

`unknown user: postgres` gibi bir hata iletisi alırsanız, PostgreSQL yüklemeniz oturum açmış kullanıcı adınız ile yapılandırılabilir. Bunun yerine aşağıdaki komutu deneyin.

```bash
psql postgres
```

Bağlantınız başarılı olursa, PostgreSQL veritabanınız çalışır. Aksi takdirde, yerel PostgresQL veritabanınızın [İndirmeler - PostgreSQL Çekirdek Dağıtım](https://www.postgresql.org/download/) konusunda işletim sisteminiz için yönergeler izlenilerek başlatıldığından emin olun.

*eventregistration* adlı bir veritabanı oluşturun ve *supersecretpass* parolasıyla *manager* adlı ayrı bir veritabanı kullanıcısı ayarlayın.

```sql
CREATE DATABASE eventregistration;
CREATE USER manager WITH PASSWORD 'supersecretpass';
GRANT ALL PRIVILEGES ON DATABASE eventregistration TO manager;
```

PostgreSQL istemcisinden çıkmak için `\q` yazın.

<a name="step2"></a>

## <a name="create-local-python-app"></a>Yerel Python uygulaması oluşturma

Bu adımda, yerel Python Flask projesini ayarlayacaksınız.

### <a name="clone-the-sample-app"></a>Örnek uygulamayı kopyalama

Terminal penceresini açın ve bir çalışma dizinine `CD` yazın.

Örnek depoyu kopyalamak için aşağıdaki komutları çalıştırın.

```bash
git clone https://github.com/Azure-Samples/flask-postgresql-app.git
cd flask-postgresql-app
```

Bu örnek depo, bir [Flask](http://flask.pocoo.org/) uygulaması içerir.

### <a name="run-the-app-locally"></a>Uygulamayı yerel olarak çalıştırma

Gereken paketleri yükleyip uygulamayı başlatın.

```bash
# Bash
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt
cd app
FLASK_APP=app.py DBHOST="localhost" DBUSER="manager" DBNAME="eventregistration" DBPASS="supersecretpass" flask db upgrade
FLASK_APP=app.py DBHOST="localhost" DBUSER="manager" DBNAME="eventregistration" DBPASS="supersecretpass" flask run

# PowerShell
pip install virtualenv
virtualenv venv
source venv/bin/activate
pip install -r requirements.txt
cd app
Set-Item Env:FLASK_APP ".\app.py"
DBHOST="localhost" DBUSER="manager" DBNAME="eventregistration" DBPASS="supersecretpass" flask db upgrade
DBHOST="localhost" DBUSER="manager" DBNAME="eventregistration" DBPASS="supersecretpass" flask run
```

Uygulama tam olarak yüklendiğinde, şu iletiye benzer bir şey görürsünüz:

```bash
INFO  [alembic.runtime.migration] Context impl PostgresqlImpl.
INFO  [alembic.runtime.migration] Will assume transactional DDL.
INFO  [alembic.runtime.migration] Running upgrade  -> 791cd7d80402, empty message
 * Serving Flask app "app"
 * Running on http://127.0.0.1:5000/ (Press CTRL+C to quit)
```

Bir tarayıcıda `http://localhost:5000` sayfasına gidin. **Kaydet!** öğesine tıklayın ve bir test kullanıcısı oluşturun.

![Yerel olarak çalışan Python Flask uygulaması](./media/tutorial-python-postgresql-app/local-app.png)

Flask örnek uygulaması, kullanıcı verilerini veritabanında depolar. Bir kullanıcı kaydetmede başarılı olursanız, uygulamanız verileri yerel PostgreSQL veritabanına yazar.

Flask sunucusunu istediğiniz zaman durdurmak için, terminale Ctrl+C yazın.

## <a name="create-a-production-postgresql-database"></a>Üretim PostgreSQL veritabanı oluşturma

Bu adımda, Azure’da bir SQL Veritabanı oluşturursunuz. Uygulamanız Azure’da dağıtıldığında bu bulut veritabanını kullanır.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

### <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

[!INCLUDE [Create resource group](../../../includes/app-service-web-create-resource-group-linux-no-h.md)]

### <a name="create-an-azure-database-for-postgresql-server"></a>PostgreSQL için Azure Veritabanı sunucusu oluşturma

Cloud Shell'de [`az postgres server create`](/cli/azure/postgres/server?view=azure-cli-latest#az-postgres-server-create) komutuyla bir PostgreSQL sunucusu oluşturun.

Aşağıdaki örnek komutta *\<postgresql_name>* yerine benzersiz bir sunucu adı, *\<admin_username>* ve *\<admin_password>* yerine de kullanmak istediğiniz kullanıcı bilgilerini yazın. Kullanıcı kimlik bilgileri, veritabanı yöneticisi hesabı için geçerli olacaktır. Sunucu adı, PostgreSQL uç noktasının bir parçası olan `https://<postgresql_name>.postgres.database.azure.com` olarak kullanıldığından, adın Azure’daki tüm sunucularda benzersiz olması gerekir.

```azurecli-interactive
az postgres server create --resource-group myResourceGroup --name <postgresql_name> --location "West Europe" --admin-user <admin_username> --admin-password <admin_password> --sku-name B_Gen4_1
```

PostgreSQL için Azure Veritabanı sunucusu oluşturulduğunda Azure CLI, aşağıdaki örneğe benzer bilgiler gösterir:

```json
{
  "administratorLogin": "<admin_username>",
  "fullyQualifiedDomainName": "<postgresql_name>.postgres.database.azure.com",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.DBforPostgreSQL/servers/<postgresql_name>",
  "location": "westus",
  "name": "<postgresql_name>",
  "resourceGroup": "myResourceGroup",
  "sku": {
    "capacity": 1,
    "family": "Gen4",
    "name": "B_Gen4_1",
    "size": null,
    "tier": "Basic"
  },
  < JSON data removed for brevity. >
}
```

> [!NOTE]
> \<admin_username> ve \<admin_password> değerlerini daha sonra kullanmak üzere aklınızda tutun. Postgre sunucusu ve veritabanlarında oturum açmak için bunlara ihtiyacınız vardır.


### <a name="create-firewall-rules-for-the-postgresql-server"></a>PostgreSQL sunucusu için güvenlik duvarı kuralları oluşturma

Azure kaynaklarından veritabanına erişim izni vermek için Cloud Shell'de aşağıdaki Azure CLI komutunu çalıştırın.

```azurecli-interactive
az postgres server firewall-rule create --resource-group myResourceGroup --server-name <postgresql_name> --start-ip-address=0.0.0.0 --end-ip-address=0.0.0.0 --name AllowAllAzureIPs
```

> [!NOTE]
> Bu ayar, Azure ağ içindeki tüm IP’lerden ağ bağlantılarına izin verir. Üretim kullanımı için, [yalnızca uygulamanızın kullandığı giden IP adreslerini kullanarak](../app-service-ip-addresses.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json#find-outbound-ips) en kısıtlayıcı güvenlik duvarı kurallarını yapılandırmayı deneyin.

Cloud Shell'de *\<your_ip_address>* yerine [yerel IPv4 IP adresinizi](http://www.whatsmyip.org/) yazdıktan sonra komutu tekrar çalıştırarak yerel bilgisayarınızdan erişim izni verin.

```azurecli-interactive
az postgres server firewall-rule create --resource-group myResourceGroup --server-name <postgresql_name> --start-ip-address=<your_ip_address> --end-ip-address=<your_ip_address> --name AllowLocalClient
```

## <a name="connect-python-app-to-production-database"></a>Python uygulamasını üretim veritabanına bağlama

Bu adımda, örnek Flask uygulamanızı oluşturduğunuz PostgreSQL için Azure Veritabanı sunucusuna bağlarsınız.

### <a name="create-empty-database-and-user-access"></a>Boş veritabanı oluşturma ve kullanıcı erişimi sağlama

Yerel terminal penceresinde, aşağıdaki komutu çalıştırarak veritabanına bağlanın. Yönetici parolanızı girmeniz istendiğinde [PostgreSQL için Azure Veritabanı sunucusu oluşturma](#create-an-azure-database-for-postgresql-server) bölümünde belirttiğiniz parolayı kullanın.

```bash
psql -h <postgresql_name>.postgres.database.azure.com -U <my_admin_username>@<postgresql_name> postgres
```

Yerel Postgres sunucunuzda olduğu gibi, Azure Postgres sunucusunda veritabanı ve kullanıcıyı oluşturun.

```bash
CREATE DATABASE eventregistration;
CREATE USER manager WITH PASSWORD 'supersecretpass';
GRANT ALL PRIVILEGES ON DATABASE eventregistration TO manager;
```

PostgreSQL istemcisinden çıkmak için `\q` yazın.

> [!NOTE]
> Yönetici kullanıcıyı kullanmak yenine belirli uygulamalar için kısıtlı izinlere sahip olan veritabanı kullanıcıları oluşturmak en iyi uygulamadır. Bu örnekte, `manager` kullanıcısı _yalnızca_ `eventregistration` veritabanı için tam ayrıcalıklara sahiptir.

### <a name="test-app-connectivity-to-production-database"></a>Uygulamanın üretim veritabanına bağlanıp bağlanmadığını test etme

Yerel terminal penceresine dönüp aşağıdaki komutları çalıştırarak Flask veritabanı geçişini ve Flask sunucusunu çalıştırın.

```bash
FLASK_APP=app.py DBHOST="<postgresql_name>.postgres.database.azure.com" DBUSER="manager@<postgresql_name>" DBNAME="eventregistration" DBPASS="supersecretpass" flask db upgrade
FLASK_APP=app.py DBHOST="<postgresql_name>.postgres.database.azure.com" DBUSER="manager@<postgresql_name>" DBNAME="eventregistration" DBPASS="supersecretpass" flask run
```

Uygulama tam olarak yüklendiğinde, şu iletiye benzer bir şey görürsünüz:

```bash
INFO  [alembic.runtime.migration] Context impl PostgresqlImpl.
INFO  [alembic.runtime.migration] Will assume transactional DDL.
INFO  [alembic.runtime.migration] Running upgrade  -> 791cd7d80402, empty message
 * Serving Flask app "app"
 * Running on http://127.0.0.1:5000/ (Press CTRL+C to quit)
```

Bir tarayıcıda http://localhost:5000 sayfasına gidin. **Kaydet!** öğesine tıklayın ve bir test kaydı oluşturun. Artık Azure’da veritabanına veri yazıyorsunuz.

![Yerel olarak çalışan Python Flask uygulaması](./media/tutorial-python-postgresql-app/local-app.png)

## <a name="deploy-to-azure"></a>Azure’a dağıtma

Bu adımda, Postgres’e bağlı Python uygulamasını Azure App Service'e dağıtırsınız.

### <a name="configure-repository"></a>Depoyu yapılandırma

App Service’te Git dağıtım altyapısı, depo kökünde bir _application.py_ olduğunda `pip` otomasyonunu çağırır. Bu öğreticide, dağıtım altyapısının otomasyonu sizin için çalıştırmasını sağlayacaksınız. Yerel terminal penceresinde, depo köküne gidin, bir kukla _application.py_ oluşturun ve değişikliklerinizi işleyin.

```bash
cd ..
touch application.py
git add .
git commit -m "ensure azure automation"
```

### <a name="configure-a-deployment-user"></a>Dağıtım kullanıcısı yapılandırma

[!INCLUDE [Configure deployment user](../../../includes/configure-deployment-user-no-h.md)]

### <a name="create-an-app-service-plan"></a>App Service planı oluşturma 

[!INCLUDE [Create app service plan](../../../includes/app-service-web-create-app-service-plan-linux-no-h.md)]

### <a name="create-a-web-app"></a>Web uygulaması oluşturma 

[!INCLUDE [Create web app](../../../includes/app-service-web-create-web-app-python-linux-no-h.md)]

### <a name="configure-environment-variables"></a>Ortam değişkenlerini yapılandırma

Öğreticinin önceki bölümlerinde, PostgreSQL veritabanınıza bağlanmak üzere ortam değişkenleri tanımladınız.

App Service’te, Cloud Shell'de [`az webapp config appsettings set`](/cli/azure/webapp/config/appsettings?view=azure-cli-latest#az-webapp-config-appsettings-set) komutunu kullanarak ortam değişkenlerini _uygulama ayarları_ olarak ayarlayabilirsiniz.

Şu örnek, veritabanı bağlantı ayrıntılarını uygulama ayarları olarak belirtir. 

```azurecli-interactive
az webapp config appsettings set --name <app_name> --resource-group myResourceGroup --settings DBHOST="<postgresql_name>.postgres.database.azure.com" DBUSER="manager@<postgresql_name>" DBPASS="supersecretpass" DBNAME="eventregistration"
```

### <a name="push-to-azure-from-git"></a>Git üzerinden Azure'a gönderme

[!INCLUDE [app-service-plan-no-h](../../../includes/app-service-web-git-push-to-azure-no-h.md)]

```bash 
Counting objects: 5, done. 
Delta compression using up to 4 threads. 
Compressing objects: 100% (5/5), done. 
Writing objects: 100% (5/5), 489 bytes | 0 bytes/s, done. 
Total 5 (delta 3), reused 0 (delta 0) 
remote: Updating branch 'master'. 
remote: Updating submodules. 
remote: Preparing deployment for commit id '6c7c716eee'. 
remote: Running custom deployment command... 
remote: Running deployment command... 
remote: Handling node.js deployment. 
. 
. 
. 
remote: Deployment successful. 
To https://<app_name>.scm.azurewebsites.net/<app_name>.git 
 * [new branch]      master -> master 
```  

### <a name="configure-entry-point"></a>Giriş noktasını yapılandırma

Varsayılan olarak, yerleşik görüntü kök dizinde giriş noktası olarak bir _wsgi.py_ veya _application.py_ dosyası arar ancak giriş noktanız _app/app.py_. Önceden eklediğiniz _application.py_ boştur ve herhangi bir işlevi yoktur.

Cloud Shell’de, özel bir başlatma betiği ayarlamak için [`az webapp config set`](/cli/azure/webapp/config?view=azure-cli-latest#az-webapp-config-set) komutunu çalıştırın.

```azurecli-interactive
az webapp config set --name <app_name> --resource-group myResourceGroup --startup-file "gunicorn '--bind=0.0.0.0' --chdir /home/site/wwwroot/app app:app"
```

`--startup-file` parametresi özel bir komutu veya özel komut içeren dosya yolunu alır. Özel komutunuz şu biçimde olmalıdır:

```
gunicorn '--bind=0.0.0.0' --chdir /home/site/wwwroot/<subdirectory> <module>:<variable>
```

Özel komutta, giriş noktanız kök dizinde değilse `--chdir` gereklidir ve `<subdirectory>` alt dizindir. `<module>`, _.py_ dosyasının adıdır ve `<variable>` modülde web uygulamanızı temsil eden değişkendir.

### <a name="browse-to-the-azure-web-app"></a>Azure web uygulamasına göz atma

Dağıtılan web uygulamasına göz atın. Uygulama ilk kez çağrıldığında kapsayıcının indirilmesi ve çalışması gerektiğinden başlatılması biraz uzun sürebilir. Sayfa zaman aşımına uğrar veya hata iletisi görüntülerse, birkaç dakika bekleyip sayfayı yenileyin.

```bash
http://<app_name>.azurewebsites.net
```

Önceki adımda önceden kaydedilmiş konukların Azure üretim veritabanına kaydedildiğini görürsünüz.

![Azure’da çalışan Python Flask uygulaması](./media/tutorial-python-postgresql-app/docker-app-deployed.png)

**Tebrikler!** Linux için App Service’te bir Python uygulaması çalıştırıyorsunuz.

## <a name="access-diagnostic-logs"></a>Tanılama günlüklerine erişim

Python uygulaması bir kapsayıcıda çalıştığından, Linux üzerinde App Service kapsayıcı içinden oluşturulan konsol günlüklerine erişmenize olanak sağlar. Günlükleri bulmak için şu URL’ye gidin:

```
https://<app_name>.scm.azurewebsites.net/api/logs/docker
```

Her biri bir `href` özelliğine sahip iki JSON nesnesi görmeniz gerekir. Bir `href` Docker konsol günlüklerine işaret eder (`_docker.log` ile biter) ve başka bir `href` Python kapsayıcısı içinden oluşturulan konsol günlüklerine işaret eder. 

```json
[  
   {  
      "machineName":"RD0003FF61ACD0_default",
      "lastUpdated":"2018-09-27T16:48:17Z",
      "size":4766,
      "href":"https://<app_name>.scm.azurewebsites.net/api/vfs/LogFiles/2018_09_27_RD0003FF61ACD0_default_docker.log",
      "path":"/home/LogFiles/2018_09_27_RD0003FF61ACD0_default_docker.log"
   },
   {  
      "machineName":"RD0003FF61ACD0",
      "lastUpdated":"2018-09-27T16:48:19Z",
      "size":2589,
      "href":"https://<app_name>.scm.azurewebsites.net/api/vfs/LogFiles/2018_09_27_RD0003FF61ACD0_docker.log",
      "path":"/home/LogFiles/2018_09_27_RD0003FF61ACD0_docker.log"
   }
]
```

Günlüklere gitmek için istediğiniz `href` değerini bir tarayıcı penceresine kopyalayın. Günlüklerin akışı yapılmaz, bu nedenle biraz gecikme olabilir. Yeni günlükleri görmek için tarayıcı sayfasını yenileyin.

## <a name="update-data-model-and-redeploy"></a>Veri modelini güncelleştirme ve yeniden dağıtma

Bu adımda, `Guest` modelini güncelleştirerek etkinlik kayıtlarına katılanların sayısını ekleyecek ve güncelleştirmeyi Azure’a yeniden dağıtacaksınız.

Yerel terminal penceresinden, aşağıdaki Git komutunu kullanarak `modelChange` dalından dosyaları kullanıma alın:

```bash
git checkout origin/modelChange -- .
```

Bu sonuçlandırma modelde, görünümlerde ve denetleyicilerde gerekli değişiklikleri zaten yapar. Ayrıca *alembic* (`flask db migrate`) aracılığıyla oluşturulan bir veritabanı geçişi içerir. Aşağıdaki git komutu aracılığıyla tüm değişiklikleri görebilirsiniz:

```bash
git diff master origin/modelChange
```

### <a name="test-your-changes-locally"></a>Değişikliklerinizi yerel olarak test etme

Flask sunucusunu çalıştırarak değişikliklerinizi yerel olarak test etmek için yerel terminal penceresinde aşağıdaki komutları çalıştırın.

```bash
source venv/bin/activate
cd app
FLASK_APP=app.py DBHOST="<postgresql_name>.postgres.database.azure.com" DBUSER="manager@<postgresql_name>" DBNAME="eventregistration" DBPASS="supersecretpass" flask db upgrade
FLASK_APP=app.py DBHOST="<postgresql_name>.postgres.database.azure.com" DBUSER="manager@<postgresql_name>" DBNAME="eventregistration" DBPASS="supersecretpass" flask run
```

Değişiklikleri görüntülemek için tarayıcınızda http://localhost:5000 adresine gidin. Test kaydı oluşturun.

![Yerel olarak çalışan Docker kapsayıcısı tabanlı Python Flask uygulaması](./media/tutorial-python-postgresql-app/local-app-v2.png)

### <a name="publish-changes-to-azure"></a>Değişiklikleri Azure’da yayımlama

Yerel terminal penceresinde, değişikliklerinizi Git’e işleyin ve ardından kod değişikliklerini Azure’a gönderin.

```bash 
git add . 
git commit -m "updated data model" 
git push azure master 
``` 

Azure web uygulamanıza gidin ve yeni işlevleri yeniden deneyin. Sayfayı yenilediğinizden emin olun.

```bash
http://<app_name>.azurewebsites.net
```

![Azure App Service’te Docker Python Flask uygulaması](./media/tutorial-python-postgresql-app/docker-flask-in-azure.png)

## <a name="manage-your-web-app-in-the-azure-portal"></a>Azure portalda web uygulamanızı yönetme

Oluşturduğunuz web uygulamasını görmek için [Azure portalına](https://portal.azure.com) gidin.

Sol menüden **Uygulama Hizmetleri**’ne ve ardından Azure web uygulamanızın adına tıklayın.

![Portaldan Azure web uygulamasına gitme](./media/tutorial-python-postgresql-app/app-resource.png)

Portal, varsayılan olarak web uygulamanızın **Genel Bakış** sayfasında görünür. Bu sayfa, uygulamanızın nasıl çalıştığını gösterir. Buradan ayrıca göz atma, durdurma, başlatma, yeniden başlatma ve silme gibi temel yönetim görevlerini gerçekleştirebilirsiniz. Sayfanın sol tarafındaki sekmeler, açabileceğiniz farklı yapılandırma sayfalarını gösterir.

![Azure portalında App Service sayfası](./media/tutorial-python-postgresql-app/app-mgmt.png)

[!INCLUDE [cli-samples-clean-up](../../../includes/cli-samples-clean-up.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Azure’da PostgreSQL veritabanı oluşturma
> * Python uygulamasını PostgreSQL’e bağlama
> * Uygulamayı Azure’da dağıtma
> * Tanılama günlüklerini görüntüleme
> * Veri modelini güncelleştirme ve uygulamayı yeniden dağıtma
> * Uygulamayı Azure portalında yönetme

Web uygulamanıza özel bir DNS adı eşlemeyle ilgili bilgi edinmek için sonraki öğreticiye geçin.

> [!div class="nextstepaction"]
> [Yerleşik Python görüntüsünü yapılandırma](how-to-configure-python.md)

> [!div class="nextstepaction"]
> [Mevcut bir özel DNS adını Azure Web Apps ile eşleme](../app-service-web-tutorial-custom-domain.md)

