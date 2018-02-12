---
title: "Azure'da Docker Python ve PostgreSQL web uygulaması oluşturma | Microsoft Docs"
description: "Azure'da çalışan ve bir PostgreSQL veritabanına bağlantısı olan Docker Python uygulamasını nasıl alabileceğinizi öğrenin."
services: app-service\web
documentationcenter: python
author: berndverst
manager: cfowler
ms.service: app-service-web
ms.workload: web
ms.devlang: python
ms.topic: tutorial
ms.date: 01/28/2018
ms.author: beverst;cephalin
ms.custom: mvc
ms.openlocfilehash: 070f69cab63525c3209380bc5f7121812be4a899
ms.sourcegitcommit: eeb5daebf10564ec110a4e83874db0fb9f9f8061
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/03/2018
---
# <a name="build-a-docker-python-and-postgresql-web-app-in-azure"></a>Azure'da Docker Python ve PostgreSQL web uygulaması oluşturma

Kapsayıcılar için Web App yüksek oranda ölçeklenebilen, kendi kendine düzeltme eki uygulayan bir web barındırma hizmeti sunar. Bu öğretici, Azure’da temel bir Docker Python web uygulamasının nasıl oluşturulacağını gösterir. Bu uygulamayı bir PostgreSQL veritabanına bağlarsınız. İşiniz bittiğinde, [Linux üzerinde App Service](app-service-linux-intro.md)'te bir Docker kapsayıcısı içinde çalışan bir Python Flask uygulamanız olur.

![Linux üzerinde App Service’te Docker Python Flask uygulaması](./media/tutorial-docker-python-postgresql-app/docker-flask-in-azure.png)

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Azure’da PostgreSQL veritabanı oluşturma
> * Python uygulamasını MySQL’e bağlama
> * Uygulamayı Azure’da dağıtma
> * Veri modelini güncelleştirme ve uygulamayı yeniden dağıtma
> * Uygulamayı Azure portalında yönetme

Bu makaledeki adımları macOS üzerinde izleyebilirsiniz. Linux ve Windows yönergeleri çoğu durumda aynıdır, ancak bu öğreticide farkları konusunda ayrıntıya girilmemiştir.
 
[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamak için:

1. [Git'i yükleyin](https://git-scm.com/)
1. [Python'ı yükleyin](https://www.python.org/downloads/)
1. [PostgreSQL’i yükleyin ve çalıştırın](https://www.postgresql.org/download/)
1. [Docker Community Edition'ı yükleyin](https://www.docker.com/community-edition)

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

Örnek depoyu kopyalamak ve *0.1-initialapp* sürümüne gitmek için aşağıdaki komutları çalıştırın.

```bash
git clone https://github.com/Azure-Samples/docker-flask-postgres.git
cd docker-flask-postgres
git checkout tags/0.1-initialapp
```

Bu örnek depo, bir [Flask](http://flask.pocoo.org/) uygulaması içerir. 

### <a name="run-the-application"></a>Uygulamayı çalıştırma

> [!NOTE] 
> Sonraki adımlardan birinde üretim veritabanıyla kullanılacak bir Docker kapsayıcısı oluşturarak bu işlemi basitleştireceksiniz.

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

![Yerel olarak çalışan Python Flask uygulaması](./media/tutorial-docker-python-postgresql-app/local-app.png)

Flask örnek uygulaması, kullanıcı verilerini veritabanında depolar. Bir kullanıcı kaydetmede başarılı olursanız, uygulamanız verileri yerel PostgreSQL veritabanına yazar.

Flask sunucusunu istediğiniz zaman durdurmak için, terminale Ctrl+C yazın. 

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## <a name="create-a-production-postgresql-database"></a>Üretim PostgreSQL veritabanı oluşturma

Bu adımda, Azure’da bir SQL Veritabanı oluşturursunuz. Uygulamanız Azure’da dağıtıldığında bu bulut veritabanını kullanır.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

### <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

[!INCLUDE [Create resource group](../../../includes/app-service-web-create-resource-group-no-h.md)] 

### <a name="create-an-azure-database-for-postgresql-server"></a>PostgreSQL için Azure Veritabanı sunucusu oluşturma

[`az postgres server create`](/cli/azure/postgres/server?view=azure-cli-latest#az_postgres_server_create) komutuyla PostgreSQL sunucusu oluşturun.

Şu komutta *\<postgresql_name>* yer tutucusu yerine benzersiz bir sunucu adı ve *\<admin_username>* yer tutucusu yerine bir kullanıcı adı kullanın. Sunucu adı, PostgreSQL uç noktasının bir parçası olan `https://<postgresql_name>.postgres.database.azure.com` olarak kullanıldığından, adın Azure’daki tüm sunucularda benzersiz olması gerekir. Kullanıcı adı, ilk veritabanı yönetici kullanıcı hesabı içindir. Sizden bu kullanıcı için bir parola seçmeniz istenir.

```azurecli-interactive
az postgres server create --resource-group myResourceGroup --name <postgresql_name> --admin-user <admin_username>  --storage-size 51200
```

PostgreSQL sunucusu için Azure Veritabanı oluşturulduğunda Azure CLI, aşağıdaki örneğe benzer bilgiler gösterir:

```json
{
  "administratorLogin": "<my_admin_username>",
  "fullyQualifiedDomainName": "<postgresql_name>.postgres.database.azure.com",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.DBforPostgreSQL/servers/<postgresql_name>",
  "location": "westus",
  "name": "<postgresql_name>",
  "resourceGroup": "myResourceGroup",
  "sku": {
    "capacity": 100,
    "family": null,
    "name": "PGSQLS3M100",
    "size": null,
    "tier": "Basic"
  },
  "sslEnforcement": null,
  "storageMb": 2048,
  "tags": null,
  "type": "Microsoft.DBforPostgreSQL/servers",
  "userVisibleState": "Ready",
  "version": null
}
```

### <a name="create-a-firewall-rule-for-the-azure-database-for-postgresql-server"></a>PostgreSQL için Azure Veritabanı'na bir güvenlik duvarı kuralı oluşturma

Tüm IP adreslerinden veritabanına erişim izni vermek için şu Azure CLI komutunu çalıştırın.

```azurecli-interactive
az postgres server firewall-rule create --resource-group myResourceGroup --server-name <postgresql_name> --start-ip-address=0.0.0.0 --end-ip-address=255.255.255.255 --name AllowAllIPs
```

Azure CLI, şu örneğe benzer bir çıkışa sahip güvenlik duvarı kuralı oluşumunu onaylar:

```json
{
  "endIpAddress": "255.255.255.255",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.DBforPostgreSQL/servers/<postgresql_name>/firewallRules/AllowAllIPs",
  "name": "AllowAllIPs",
  "resourceGroup": "myResourceGroup",
  "startIpAddress": "0.0.0.0",
  "type": "Microsoft.DBforPostgreSQL/servers/firewallRules"
}
```

## <a name="connect-your-python-flask-application-to-the-database"></a>Python Flask uygulamanızı veritabanına bağlama

Bu adımda, örnek Python Flask uygulamanızı oluşturduğunuz PostgreSQL sunucusu için Azure Veritabanı'na bağlarsınız.

### <a name="create-an-empty-database-and-set-up-a-new-database-application-user"></a>Boş bir veritabanı oluşturun ve yeni veritabanı uygulama kullanıcısını ayarlayın

Yalnızca tek bir veritabanına erişimi olan bir veritabanı kullanıcısı oluşturun. Uygulamaya sunucuya tam erişim vermeyi önlemek için bu kimlik bilgilerini kullanın.

Veritabanına bağlanın (yönetici parolanızı girmeniz istenir).

```bash
psql -h <postgresql_name>.postgres.database.azure.com -U <my_admin_username>@<postgresql_name> postgres
```

PostgreSQL CLI’dan veritabanı ve kullanıcı oluşturun.

```bash
CREATE DATABASE eventregistration;
CREATE USER manager WITH PASSWORD 'supersecretpass';
GRANT ALL PRIVILEGES ON DATABASE eventregistration TO manager;
```

PostgreSQL istemcisinden çıkmak için `\q` yazın.

### <a name="test-the-application-locally-against-the-azure-postgresql-database"></a>Uygulamayı Azure PostgreSQL veritabanıyla yerel olarak test edin

Şimdi kopyalanan Github deposunun *app* klasörüne geri dönerek, veritabanı ortam değişkenlerini güncelleştirip Python Flask uygulamasını çalıştırabilirsiniz.

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

Bir tarayıcıda http://localhost:5000 konumuna gidin. **Kaydet!** öğesine tıklayın ve bir test kaydı oluşturun. Artık Azure’da veritabanına veri yazıyorsunuz.

![Yerel olarak çalışan Python Flask uygulaması](./media/tutorial-docker-python-postgresql-app/local-app.png)

### <a name="running-the-application-from-a-docker-container"></a>Uygulamayı Docker Kapsayıcısı'ndan çalıştırma

Docker kapsayıcı görüntüsünü oluşturun.

```bash
cd ..
docker build -t flask-postgresql-sample .
```

Docker kapsayıcının başarıyla oluşturulduğuna ilişkin bir onay görüntüler.

```bash
Successfully built 7548f983a36b
```

Depo kökünde, _db.env_ adlı ortam değişkeni dosyasını ekleyin ve ardından buna aşağıdaki veritabanı ortam değişkenlerini ekleyin. Uygulama, PostgreSQL üretim veritabanı için Azure Veritabanı'na bağlanır.

```text
DBHOST=<postgresql_name>.postgres.database.azure.com
DBUSER=manager@<postgresql_name>
DBNAME=eventregistration
DBPASS=supersecretpass
```

Uygulamayı Docker kapsayıcısının içinden çalıştırın. Aşağıdaki komut ortam değişkeni dosyasını belirtir ve varsayılan 5000 numaralı Flask bağlantı noktasını yerel 5000 numaralı bağlantı noktasına eşler.

```bash
docker run -it --env-file db.env -p 5000:5000 flask-postgresql-sample
```

Çıkış daha önce gördüklerinize benzer. Bununla birlikte, artık ilk veritabanı geçişinin yapılması gerekmez ve dolayısıyla bu geçiş atlanır.

```bash
INFO  [alembic.runtime.migration] Context impl PostgresqlImpl.
INFO  [alembic.runtime.migration] Will assume transactional DDL.
 * Serving Flask app "app"
 * Running on http://0.0.0.0:5000/ (Press CTRL+C to quit)
```

Veritabanı zaten daha önce oluşturduğunuz kaydı içermektedir.

![Yerel olarak çalışan Docker kapsayıcısı tabanlı Python Flask uygulaması](./media/tutorial-docker-python-postgresql-app/local-docker.png)

## <a name="upload-the-docker-container-to-a-container-registry"></a>Docker kapsayıcısını bir kapsayıcı kayıt defterine yükleme

Bu adımda, Docker kapsayıcısını bir kapsayıcı kayıt defterine yüklersiniz. Azure Container Registry'yi kullanın. Ama Docker Hub gibi diğer popüler kayıt defterlerini de kullanabilirsiniz.

### <a name="create-an-azure-container-registry"></a>Azure Container Registry oluşturma

Kapsayıcı kayıt defteri oluşturmaya yönelik aşağıdaki komutta *\<registry_name>* değerini kendi seçtiğiniz benzersiz bir Azure kapsayıcı kayıt defteri adıyla değiştirin.

```azurecli-interactive
az acr create --name <registry_name> --resource-group myResourceGroup --location "West US" --sku Basic
```

Çıktı

```json
{
  "adminUserEnabled": false,
  "creationDate": "2017-05-04T08:50:55.635688+00:00",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.ContainerRegistry/registries/<registry_name>",
  "location": "westus",
  "loginServer": "<registry_name>.azurecr.io",
  "name": "<registry_name>",
  "provisioningState": "Succeeded",
  "sku": {
    "name": "Basic",
    "tier": "Basic"
  },
  "storageAccount": {
    "name": "<registry_name>01234"
  },
  "tags": {},
  "type": "Microsoft.ContainerRegistry/registries"
}
```

### <a name="retrieve-the-registry-credentials-for-pushing-and-pulling-docker-images"></a>Docker görüntülerini gönderme ve çekme için kayıt defteri kimlik bilgilerini alma

Kayıt defteri kimlik bilgilerini görüntülemek için, önce yönetici modunu etkinleştirin.

```azurecli-interactive
az acr update --name <registry_name> --admin-enabled true
az acr credential show -n <registry_name>
```

İki parola göreceksiniz. Kullanıcı adını ve ilk parolayı not alın.

```json
{
  "passwords": [
    {
      "name": "password",
      "value": "<registry_password>"
    },
    {
      "name": "password2",
      "value": "<registry_password2>"
    }
  ],
  "username": "<registry_name>"
}
```

### <a name="upload-your-docker-container-to-azure-container-registry"></a>Docker kapsayıcınızı Azure Container Registry'ye yükleme

Kayıt defterinizde oturum açın. İstendiğinde, aldığınız parolayı girin.

```bash
docker login <registry_name>.azurecr.io -u <registry_name>
```

Docker görüntünüzü kayıt defterine gönderin.

```bash
docker tag flask-postgresql-sample <registry_name>.azurecr.io/flask-postgresql-sample
docker push <registry_name>.azurecr.io/flask-postgresql-sample
```

## <a name="deploy-the-docker-python-flask-application-to-azure"></a>Docker Python Flask uygulamasını Azure'a dağıtma

Bu adımda, Docker kapsayıcı tabanlı Python Flask uygulamanızı Azure App Service'e dağıtırsınız.

### <a name="create-an-app-service-plan"></a>App Service planı oluşturma

[!INCLUDE [Create app service plan](../../../includes/app-service-web-create-app-service-plan-linux-no-h.md)]

### <a name="create-a-web-app"></a>Web uygulaması oluşturma

*myAppServicePlan* App Service planında [`az webapp create`](/cli/azure/webapp?view=azure-cli-latest#az_webapp_create) komutuyla bir web uygulaması oluşturun.

Web uygulaması size kodunuzu dağıtmak için bir barındırma alanı getirir ve dağıtılan uygulamayı görüntüleyebilmeniz için bir URL sağlar. Web uygulamasını oluşturun.

Aşağıdaki komutta, *\<app_name>* yer tutucusunu benzersiz bir uygulama adıyla değiştirin. Bu ad web uygulamasına ilişkin varsayılan URL'nin bir parçasıdır; dolayısıyla, Azure App Service'teki tüm uygulamalar arasında benzersiz olmalıdır.

```azurecli
az webapp create --name <app_name> --resource-group myResourceGroup --plan myAppServicePlan --deployment-container-image-name "<registry_name>.azurecr.io/flask-postgresql-sample"
```

Web uygulaması oluşturulduğunda Azure CLI aşağıda yer alan örnekteki gibi bilgiler gösterir:

```json
{
  "availabilityState": "Normal",
  "clientAffinityEnabled": true,
  "clientCertEnabled": false,
  "cloningInfo": null,
  "containerSize": 0,
  "dailyMemoryTimeQuota": 0,
  "defaultHostName": "<app_name>.azurewebsites.net",
  "enabled": true,
  ...
  < Output has been truncated for readability >
}
```

### <a name="configure-the-database-environment-variables"></a>Veritabanı ortam değişkenlerini yapılandırma

Öğreticinin önceki bölümlerinde, PostgreSQL veritabanınıza bağlanmak üzere ortam değişkenleri tanımladınız.

App Service'te, [`az webapp config appsettings set`](/cli/azure/webapp/config/appsettings?view=azure-cli-latest#az_webapp_config_appsettings_set) komutunu kullanıp ortam değişkenlerini _uygulama ayarları_ olarak belirlersiniz.

Şu örnek, veritabanı bağlantı ayrıntılarını uygulama ayarları olarak belirtir. Ayrıca, Docker Kapsayıcınızdaki 5000 NUMARALI BAĞLANTI NOKTASINI HTTP trafiğini 80 NUMARALI BAĞLANTI NOKTASINDA alacak şekilde eşlemek için *PORT* değişkenini kullanır.

```azurecli-interactive
az webapp config appsettings set --name <app_name> --resource-group myResourceGroup --settings DBHOST="<postgresql_name>.postgres.database.azure.com" DBUSER="manager@<postgresql_name>" DBPASS="supersecretpass" DBNAME="eventregistration" PORT=5000
```

### <a name="configure-docker-container-deployment"></a>Docker kapsayıcı dağıtımını yapılandırma

AppService bir Docker kapsayıcısını otomatik olarak indirip çalıştırabilir.

```azurecli
az webapp config container set --resource-group myResourceGroup --name <app_name> --docker-registry-server-user "<registry_name>" --docker-registry-server-password "<registry_password>" --docker-registry-server-url "https://<registry_name>.azurecr.io"
```

Docker kapsayıcısını her güncelleştirdiğinizde veya ayarlarını değiştirdiğinizde, uygulamayı yeniden başlatın. Yeniden başlatma, tüm ayarların uygulandığından ve kayıt defterinden en son kapsayıcının alındığından emin olmanızı sağlar.

```azurecli-interactive
az webapp restart --resource-group myResourceGroup --name <app_name>
```

### <a name="browse-to-the-azure-web-app"></a>Azure web uygulamasına göz atma 

Web tarayıcınızı kullanarak dağıtılan web uygulamasına göz atın. 

```bash 
http://<app_name>.azurewebsites.net 
```
> [!NOTE]
> Web uygulamasının yüklenmesi daha uzun sürer çünkü kapsayıcı yapılandırması değiştirildikten sonra kapsayıcının indirilmesi ve başlatılması gerekir.

Önceki adımda önceden kaydedilmiş konukların Azure üretim veritabanına kaydedildiğini görürsünüz.

![Yerel olarak çalışan Docker kapsayıcısı tabanlı Python Flask uygulaması](./media/tutorial-docker-python-postgresql-app/docker-app-deployed.png)

**Tebrikler!** Azure App Service’te Docker kapsayıcı tabanlı bir Python Flask uygulaması çalıştırıyorsunuz.

## <a name="update-data-model-and-redeploy"></a>Veri modelini güncelleştirme ve yeniden dağıtma

Bu adımda, Konuk modelini güncelleştirerek etkinlik kayıtlarına katılanların sayısını ekliyorsunuz.

Aşağıdaki git komutuyla *0.2-migration* sürümünü kullanıma alın:

```bash
git checkout tags/0.2-migration
```

Bu sürüm görünümlere, denetleyicilere ve modele gerekli değişiklikleri zaten yapmıştır. Ayrıca *alembic* (`flask db migrate`) aracılığıyla oluşturulan bir veritabanı geçişi içerir. Aşağıdaki git komutu aracılığıyla yapılan tüm değişiklikleri görebilirsiniz:

```bash
git diff 0.1-initialapp 0.2-migration
```

### <a name="test-your-changes-locally"></a>Değişikliklerinizi yerel olarak test etme

Flask sunucusunu çalıştırarak değişikliklerinizi yerel olarak test etmek için şu komutları çalıştırın.

```bash
source venv/bin/activate
cd app
FLASK_APP=app.py DBHOST="localhost" DBUSER="manager" DBNAME="eventregistration" DBPASS="supersecretpass" flask db upgrade
FLASK_APP=app.py DBHOST="localhost" DBUSER="manager" DBNAME="eventregistration" DBPASS="supersecretpass" flask run
```

Değişiklikleri görüntülemek için tarayıcınızda http://localhost:5000 konumuna gidin. Test kaydı oluşturun.

![Yerel olarak çalışan Docker kapsayıcısı tabanlı Python Flask uygulaması](./media/tutorial-docker-python-postgresql-app/local-app-v2.png)

### <a name="publish-changes-to-azure"></a>Değişiklikleri Azure’da yayımlama

Yeni Docker görüntüsünü oluşturun, bunu kapsayıcı kayıt defterine gönderin ve uygulamayı yeniden başlatın.

```bash
cd ..
docker build -t flask-postgresql-sample .
docker tag flask-postgresql-sample <registry_name>.azurecr.io/flask-postgresql-sample
docker push <registry_name>.azurecr.io/flask-postgresql-sample
az appservice web restart --resource-group myResourceGroup --name <app_name>
```

Azure web uygulamanıza gidin ve yeni işlevleri yeniden deneyin. Başka bir etkinlik kaydı oluşturun.

```bash 
http://<app_name>.azurewebsites.net 
```

![Azure App Service’te Docker Python Flask uygulaması](./media/tutorial-docker-python-postgresql-app/docker-flask-in-azure.png)

## <a name="manage-your-azure-web-app"></a>Azure web uygulamanızı yönetme

Oluşturduğunuz web uygulamasını görmek için [Azure portalına](https://portal.azure.com) gidin.

Sol menüden **Uygulama Hizmetleri**’ne ve ardından Azure web uygulamanızın adına tıklayın.

![Portaldan Azure web uygulamasına gitme](./media/tutorial-docker-python-postgresql-app/app-resource.png)

Portal, varsayılan olarak web uygulamanızın **Genel Bakış** sayfasında görünür. Bu sayfa, uygulamanızın nasıl çalıştığını gösterir. Buradan ayrıca göz atma, durdurma, başlatma, yeniden başlatma ve silme gibi temel yönetim görevlerini gerçekleştirebilirsiniz. Sayfanın sol tarafındaki sekmeler, açabileceğiniz farklı yapılandırma sayfalarını gösterir.

![Azure portalında App Service sayfası](./media/tutorial-docker-python-postgresql-app/app-mgmt.png)

## <a name="next-steps"></a>Sonraki adımlar

Web uygulamanıza özel bir DNS adı eşlemeyle ilgili bilgi edinmek için sonraki öğreticiye geçin.

> [!div class="nextstepaction"]
> [Mevcut bir özel DNS adını Azure Web Apps ile eşleme](../app-service-web-tutorial-custom-domain.md)
