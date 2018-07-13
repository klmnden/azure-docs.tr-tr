---
title: Azure'da Python ve PostgreSQL web uygulaması oluşturma | Microsoft Docs
description: Azure'da çalışan ve bir PostgreSQL veritabanına bağlantısı olan Python uygulamasını nasıl alabileceğinizi öğrenin.
services: app-service\web
documentationcenter: python
author: berndverst
manager: erikre
ms.service: app-service-web
ms.workload: web
ms.devlang: python
ms.topic: tutorial
ms.date: 01/25/2018
ms.author: beverst
ms.custom: mvc
ms.openlocfilehash: 49ec67d06446d6c48e45aef90e2bd528a1b541a9
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38473023"
---
# <a name="tutorial-build-a-python-and-postgresql-web-app-in-azure"></a>Öğretici: Azure’da Python ve PostgreSQL web uygulaması derleme

> [!NOTE]
> Bu makalede bir uygulamanın Windows üzerinde App Service'e dağıtımı yapılır. _Linux_ üzerinde App Service'e dağıtım yapmak için, bkz. [Azure’da Docker Python ve PostgreSQL web uygulaması oluşturma](./containers/tutorial-docker-python-postgresql-app.md).
>

[Azure App Service](app-service-web-overview.md), yüksek oranda ölçeklenebilen, kendi kendine düzeltme eki uygulayan bir web barındırma hizmeti sunar. Bu öğretici, Azure’da temel bir Python web uygulamasının nasıl oluşturulacağını gösterir. Bu uygulamayı bir PostgreSQL veritabanına bağlayın. İşiniz bittiğinde, App Service üzerinde çalışan bir Python Flask uygulamanız olur.

![Linux üzerinde App Service’te Python Flask uygulaması](./media/app-service-web-tutorial-python-postgresql/docker-flask-in-azure.png)

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Azure’da PostgreSQL veritabanı oluşturma
> * Python uygulamasını MySQL’e bağlama
> * Uygulamayı Azure’da dağıtma
> * Veri modelini güncelleştirme ve uygulamayı yeniden dağıtma
> * Uygulamayı Azure portalında yönetme

Bu öğreticideki adımları MacOS üzerinde izleyebilirsiniz. Linux ve Windows yönergeleri çoğu durumda aynıdır, ancak bu öğreticide farkları konusunda ayrıntıya girilmemiştir.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamak için:

1. [Git'i yükleyin](https://git-scm.com/)
1. [Python'ı yükleyin](https://www.python.org/downloads/)
1. [PostgreSQL’i yükleyin ve çalıştırın](https://www.postgresql.org/download/)

## <a name="test-local-postgresql-installation-and-create-a-database"></a>Yerel PostgreSQL yüklemesini test etme ve bir veritabanı oluşturma

Yerel PostgreSQL sunucunuza bağlanmak için terminal penceresini açın ve `psql` komutunu çalıştırın.

```bash
sudo -u postgres psql
```

Bağlantınız başarılı olursa, PostgreSQL veritabanınız çalışır. Aksi takdirde, yerel PostgresQL veritabanınızın [İndirmeler - PostgreSQL Çekirdek Dağıtım](https://www.postgresql.org/download/) konusundaki adımlar izlenilerek başlatıldığından emin olun.

*eventregistration* adlı bir veritabanı oluşturun ve *supersecretpass* parolasıyla *manager* adlı ayrı bir veritabanı kullanıcısı ayarlayın.

```bash
CREATE DATABASE eventregistration;
CREATE USER manager WITH PASSWORD 'supersecretpass';
GRANT ALL PRIVILEGES ON DATABASE eventregistration TO manager;
```
PostgreSQL istemcisinden çıkmak için `\q` yazın. 

<a name="step2"></a>

## <a name="create-local-python-flask-application"></a>Yerel Python Flask uygulaması oluşturma

Bu adımda, yerel Python Flask projesini ayarlayacaksınız.

### <a name="clone-the-sample-application"></a>Örnek uygulamayı kopyalama

Terminal penceresini açın ve bir çalışma dizinine `CD` yazın.

Örnek depoyu kopyalamak için şu komutları çalıştırın ve ardından ilk uygulamanın yürütmesine geri dönün (`modelChange` komutundan önce).

```bash
git clone https://github.com/Azure-Samples/flask-postgresql-app
cd flask-postgresql-app
git revert modelChange --no-edit
```

Bu örnek depo, bir [Flask](http://flask.pocoo.org/) uygulaması içerir. 

### <a name="run-the-application"></a>Uygulamayı çalıştırma

Gereken paketleri yükleyip uygulamayı başlatın.

```bash
pip install virtualenv
virtualenv venv
source venv/bin/activate
pip install -r requirements.txt
cd app
FLASK_APP=app.py DBHOST="localhost" DBUSER="manager" DBNAME="eventregistration" DBPASS="supersecretpass" flask db upgrade
FLASK_APP=app.py DBHOST="localhost" DBUSER="manager" DBNAME="eventregistration" DBPASS="supersecretpass" flask run
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

![Yerel olarak çalışan Python Flask uygulaması](./media/app-service-web-tutorial-python-postgresql/local-app.png)

Flask örnek uygulaması, kullanıcı verilerini veritabanında depolar. Bir kullanıcı kaydetmede başarılı olursanız, uygulamanız verileri yerel PostgreSQL veritabanına yazar.

Flask sunucusunu istediğiniz zaman durdurmak için, terminale Ctrl+C yazın. 

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="create-postgresql-in-azure"></a>Azure’da PostgreSQL oluşturma

Bu adımda, Azure’da bir SQL Veritabanı oluşturursunuz. Uygulamanız Azure’da dağıtıldığında bu bulut veritabanını kullanır.

### <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

[!INCLUDE [Create resource group](../../includes/app-service-web-create-resource-group-no-h.md)] 

### <a name="create-a-postgresql-server"></a>PostgreSQL sunucusu oluşturma

[`az postgres server create`](/cli/azure/postgres/server?view=azure-cli-latest#az_postgres_server_create) komutuyla PostgreSQL sunucusu oluşturun.

Aşağıdaki komutta *\<postgresql_name>* yer tutucusunu benzersiz bir sunucu ile değiştirin, *\<admin_username>* için bir kullanıcı adı ve *\<admin_password>* yer tutucusu için bir parola girin. Sunucu adı, PostgreSQL uç noktasının bir parçası olan `https://<postgresql_name>.postgres.database.azure.com` olarak kullanıldığından, adın Azure’daki tüm sunucularda benzersiz olması gerekir.

```azurecli-interactive
az postgres server create --resource-group myResourceGroup --name <postgresql_name> --location "West Europe" --admin-user <admin_username> --admin-password <server_admin_password> --sku-name GP_Gen4_2
```

PostgreSQL için Azure Veritabanı sunucusu oluşturulduğunda Azure CLI, aşağıdaki örneğe benzer bilgiler gösterir:

```json
{
  "location": "westeurope",
  "name": "<postgresql_server_name>",
  "resourceGroup": "myResourceGroup",
  "sku": {
    "additionalProperties": {},
    "capacity": 2,
    "family": "Gen4",
    "name": "GP_Gen4_2",
    "size": null,
    "tier": "GeneralPurpose"
  },
  "sslEnforcement": "Enabled",
  ...   +  
  -  < Output has been truncated for readability >
}
```

### <a name="configure-server-firewall"></a>Sunucu güvenlik duvarını yapılandırma

Tüm IP adreslerinden veritabanına erişim izni vermek için şu Azure CLI komutunu çalıştırın. Hem başlangıç hem bitiş IP’si 0.0.0.0 olarak ayarlandığında, güvenlik duvarı yalnızca diğer Azure kaynakları için açılır. 

```azurecli-interactive
az postgres server firewall-rule create --resource-group myResourceGroup --server-name <postgresql_name> --start-ip-address 0.0.0.0 --end-ip-address 0.0.0.0 --name AllowAzureIPs
```

Azure CLI, şu örneğe benzer bir çıkışa sahip güvenlik duvarı kuralı oluşumunu onaylar:

```json
{
  "additionalProperties": {},
  "endIpAddress": "255.255.255.255",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.DBforPostgreSQL/servers/<postgresql_name>/firewallRules/AllowAllIPs",
  "name": "AllowAllIPs",
 "resourceGroup": "myResourceGroup",
  "startIpAddress": "0.0.0.0",
  "type": "Microsoft.DBforPostgreSQL/servers/firewallRules"
}
```

> [!TIP] 
> [Yalnızca uygulamanızın kullandığı giden IP adreslerini kullanarak](app-service-ip-addresses.md#find-outbound-ips) güvenlik duvarı kurallarınızda daha da kısıtlayıcı olabilirsiniz.
>

### <a name="create-a-production-database-and-user"></a>Üretim veritabanı ve kullanıcı oluşturma

Yalnızca tek bir veritabanına erişimi olan bir veritabanı kullanıcısı oluşturun. Uygulamaya sunucuya tam erişim vermeyi önlemek için bu kimlik bilgilerini kullanın.

Veritabanına bağlanın (yönetici parolanızı girmeniz istenir).

```bash
psql -h <postgresql_name>.postgres.database.azure.com -U <admin_username>@<postgresql_name> postgres
```

PostgreSQL CLI’dan veritabanı ve kullanıcı oluşturun.

```bash
CREATE DATABASE eventregistration;
CREATE USER manager WITH PASSWORD 'supersecretpass';
GRANT ALL PRIVILEGES ON DATABASE eventregistration TO manager;
```

PostgreSQL istemcisinden çıkmak için `\q` yazın.

### <a name="test-app-locally-with-azure-postgresql"></a>Azure PostgreSQL ile uygulamayı yerel olarak test etme

Şimdi kopyalanan Github deposuna ait *uygulama* klasörüne geri döndüğünüzde, veritabanı ortam değişkenlerini güncelleştirerek Python Flask uygulamasını çalıştırabilirsiniz.

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

![Yerel olarak çalışan Python Flask uygulaması](./media/app-service-web-tutorial-python-postgresql/local-app.png)

## <a name="deploy-to-azure"></a>Azure’a dağıtma

Bu adımda, PostgreSQL’e bağlı Python uygulamasını Azure App Service'e dağıtırsınız.

Git deponuz, App Service'te Flask web uygulamasını çalıştırmak için gereken şu dosyaları zaten içerir:

- `.deployment`: çalıştırmak için özel dağıtım betiğini belirtir.
- `deploy.cmd`: dağıtım betiği. `pip install` komutunun çalıştırılacağı yer.
- `web.config`: IIS'te bir `httpPlatformHandler` içinde çalıştırmak için giriş noktası betiğini belirtir.
- `run_waitress_server.py`: giriş noktası betiği. Bir [`waitress`](https://docs.pylonsproject.org/projects/waitress) sunucusunda Flask web uygulamasını başlatır.

### <a name="configure-a-deployment-user"></a>Dağıtım kullanıcısı yapılandırma

[!INCLUDE [Configure deployment user](../../includes/configure-deployment-user-no-h.md)]

### <a name="create-an-app-service-plan"></a>App Service planı oluşturma

[!INCLUDE [Create app service plan](../../includes/app-service-web-create-app-service-plan-no-h.md)]

<a name="create"></a>
### <a name="create-a-web-app"></a>Web uygulaması oluşturma

[!INCLUDE [Create web app no h](../../includes/app-service-web-create-web-app-no-h.md)] 

### <a name="install-python"></a>Python'ı yükleme

Bu adımda, App Service'te [site uzantıları](https://www.siteextensions.net/packages?q=Tags%3A%22python%22) ile Python 3.6.2 yüklersiniz. REST uç noktasına karşı kimlik doğrulaması yapmak için [Dağıtım kullanıcısı yapılandırma](#configure-a-deployment-user) bölümünde yapılandırdığınız kimlik bilgilerini kullanacaksınız.

Cloud Shell’de, Python 3.6.2 paket bilgilerini almak için sonraki komutu çalıştırın. *\<deployment_user >* öğesini yapılandırdığınız dağıtım kullanıcı adı ve *\<app_name >* öğesini de uygulama adınız ile değiştirin. İstendiğinde, yapılandırdığınız dağıtım parolasını kullanın.

```bash
packageinfo=$(curl -u <deployment_user> https://<app_name>.scm.azurewebsites.net/api/extensionfeed/python362x86)
```

Cloud Shell’de, Python paketini yüklemek için sonraki komutu çalıştırın. *\<deployment_user >* öğesini yapılandırdığınız dağıtım kullanıcı adı ve *\<app_name >* öğesini de uygulama adınız ile değiştirin. İstendiğinde, yapılandırdığınız dağıtım parolasını kullanın.

```bash
curl -X PUT -u <deployment_user> -H "Content-Type: application/json" -d '$packageinfo' https://<app_name>.scm.azurewebsites.net/api/siteextensions/python362x86
```

Komut çıktısından, Python’un `D:\home\python362x86\python.exe` yolunda yüklenmiş olduğunu görebilirsiniz.

### <a name="configure-database-settings"></a>Veritabanı ayarlarını yapılandırma

Öğreticide daha önce PostgreSQL veritabanınıza bağlanmak için ortam değişkenleri tanımladınız.

App Service’te, [az webapp config appsettings set](/cli/azure/webapp/config/appsettings?view=azure-cli-latest#az_webapp_config_appsettings_set) komutunu kullanarak ortam değişkenlerini _uygulama ayarları_ olarak ayarlarsınız.

Şu örnek, veritabanı bağlantı ayrıntılarını uygulama ayarları olarak belirtir. *\<app_name >* öğesini uygulama adınızla ve *\<postgresql_name >* öğesini de PostgreSQL sunucu adınızla değiştirin.

```azurecli-interactive
az webapp config appsettings set --name <app_name> --resource-group myResourceGroup --settings DBHOST="<postgresql_name>.postgres.database.azure.com" DBUSER="manager@<postgresql_name>" DBPASS="supersecretpass" DBNAME="eventregistration"
```

### <a name="push-to-azure-from-git"></a>Git üzerinden Azure'a gönderme

[!INCLUDE [app-service-plan-no-h](../../includes/app-service-web-git-push-to-azure-no-h.md)]

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

### <a name="browse-to-the-azure-web-app"></a>Azure web uygulamasına göz atma 

Web tarayıcınızı kullanarak dağıtılan web uygulamasına göz atın. 

```bash 
http://<app_name>.azurewebsites.net 
```

Önceki adımda önceden kaydedilmiş konukların Azure üretim veritabanına kaydedildiğini görürsünüz.

![Yerel olarak çalışan Python Flask uygulaması](./media/app-service-web-tutorial-python-postgresql/docker-app-deployed.png)

**Tebrikler!** Azure App Service’te bir Python Flask uygulaması çalıştırıyorsunuz.

## <a name="update-data-model-and-redeploy"></a>Veri modelini güncelleştirme ve yeniden dağıtma

Bu adımda, `Guest` modelini güncelleştirerek etkinlik kayıtlarına katılanların sayısını ekliyorsunuz.

`modelChange` yürütmesi tarafından etiketlenmiş dosyalara göz atın:

```bash
git checkout modelChange -- *
```

Bu sürüm görünümlere, denetleyicilere ve modele gerekli değişiklikleri zaten yaptı. Ayrıca *alembic* (`flask db migrate`) aracılığıyla oluşturulan bir veritabanı geçişi içerir. Tüm dosyalardaki değişiklikleri [GitHub yürütme görünümünde](https://github.com/Azure-Samples/flask-postgresql-app/commit/139a53023688631c3cc2caefd70086f4722ecd7e) görebilirsiniz.

### <a name="test-your-changes-locally"></a>Değişikliklerinizi yerel olarak test etme

Flask sunucusunu çalıştırarak değişikliklerinizi yerel olarak test etmek için şu komutları çalıştırın.

```bash
source venv/bin/activate
cd app
FLASK_APP=app.py DBHOST="localhost" DBUSER="manager" DBNAME="eventregistration" DBPASS="supersecretpass" flask db upgrade
FLASK_APP=app.py DBHOST="localhost" DBUSER="manager" DBNAME="eventregistration" DBPASS="supersecretpass" flask run
```

Değişiklikleri görüntülemek için tarayıcınızda http://localhost:5000 adresine gidin. Test kaydı oluşturun.

![Yerel olarak çalışan Python Flask uygulaması](./media/app-service-web-tutorial-python-postgresql/local-app-v2.png)

### <a name="publish-changes-to-azure"></a>Değişiklikleri Azure’da yayımlama

Yerel terminal penceresinde, değişikliklerinizi Git’e işleyin ve ardından kod değişikliklerini Azure’a gönderin.

```bash
git add .
git commit -m "updated data model"
git push azure master
```

Azure web uygulamanıza gidin ve yeni işlevleri yeniden deneyin. Başka bir etkinlik kaydı oluşturun.

```bash 
http://<app_name>.azurewebsites.net 
```

![Azure App Service’te Python Flask uygulaması](./media/app-service-web-tutorial-python-postgresql/docker-flask-in-azure.png)

## <a name="manage-your-azure-web-app"></a>Azure web uygulamanızı yönetme

Oluşturduğunuz web uygulamasını görmek için [Azure portalına](https://portal.azure.com) gidin.

Sol menüden **Uygulama Hizmetleri**’ne ve ardından Azure web uygulamanızın adına tıklayın.

![Portaldan Azure web uygulamasına gitme](./media/app-service-web-tutorial-python-postgresql/app-resource.png)

Portal, varsayılan olarak web uygulamanızın **Genel Bakış** sayfasında görünür. Bu sayfa, uygulamanızın nasıl çalıştığını gösterir. Buradan ayrıca göz atma, durdurma, başlatma, yeniden başlatma ve silme gibi temel yönetim görevlerini gerçekleştirebilirsiniz. Sayfanın sol tarafındaki sekmeler, açabileceğiniz farklı yapılandırma sayfalarını gösterir.

![Azure portalında App Service sayfası](./media/app-service-web-tutorial-python-postgresql/app-mgmt.png)

[!INCLUDE [cli-samples-clean-up](../../includes/cli-samples-clean-up.md)]

## <a name="next-steps"></a>Sonraki adımlar

Web uygulamanıza özel bir DNS adı eşlemeyle ilgili bilgi edinmek için sonraki öğreticiye geçin.

> [!div class="nextstepaction"]
> [Mevcut bir özel DNS adını Azure Web Apps ile eşleme](app-service-web-tutorial-custom-domain.md)
