---
title: "Azure'da Docker Python ve PostgreSQL bir web uygulaması oluşturma | Microsoft Docs"
description: "Bir PostgreSQL veritabanına bağlantı ile Azure içinde çalışma Docker Python uygulaması alma hakkında bilgi."
services: app-service\web
documentationcenter: python
author: berndverst
manager: erikre
ms.service: app-service-web
ms.workload: web
ms.devlang: python
ms.topic: tutorial
ms.date: 05/03/2017
ms.author: beverst
ms.custom: mvc
ms.openlocfilehash: 55d6f1d10ff08fd8f0ea4aba775a96549192cef2
ms.sourcegitcommit: 310748b6d66dc0445e682c8c904ae4c71352fef2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/28/2017
---
# <a name="build-a-docker-python-and-postgresql-web-app-in-azure"></a>Azure'da Docker Python ve PostgreSQL bir web uygulaması oluşturma

Web uygulaması kapsayıcıları için yüksek düzeyde ölçeklenebilir, otomatik olarak düzeltme eki uygulama web barındırma hizmeti sağlar. Bu öğretici, temel bir Docker Python web uygulaması oluşturma gösterilmektedir. Bu uygulama bir PostgreSQL veritabanına bağlanın. İşiniz bittiğinde, Docker kapsayıcısı içinde çalışan bir Python Flask uygulamasına sahip [Linux uygulama hizmeti](app-service-linux-intro.md).

![Docker Python Flask uygulama Linux uygulama hizmeti](./media/tutorial-docker-python-postgresql-app/docker-flask-in-azure.png)

MacOS üzerinde aşağıdaki adımları izleyebilirsiniz. Linux ve Windows yönergeleri çoğu durumda aynıdır, ancak bu öğreticide farklar ayrıntılı değil.
 
## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamak için:

1. [Git'i yükleyin](https://git-scm.com/)
1. [Python'ı yükleyin](https://www.python.org/downloads/)
1. [Yükleme ve PostgreSQL çalıştırma](https://www.postgresql.org/download/)
1. [Docker Community Edition'ı yükleme](https://www.docker.com/community-edition)

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

CLI'yi yerel olarak yükleyip kullanmayı tercih ederseniz bu konu başlığı için Azure CLI 2.0 veya sonraki bir sürümünü kullanmanız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI 2.0 yükleme]( /cli/azure/install-azure-cli). 

## <a name="test-local-postgresql-installation-and-create-a-database"></a>Yerel PostgreSQL yüklemeyi sınamak ve bir veritabanı oluştur

Terminal penceresi açın ve Çalıştır `psql postgres` yerel PostgreSQL sunucunuza bağlanmak için.

```bash
psql postgres
```

Bağlantı başarılı olursa, PostgreSQL veritabanınız çalışıyor. Aksi takdirde, yerel PostgresQL veritabanınızı verilen adımları izleyerek başlatıldığından emin olun [yüklemeleri - PostgreSQL çekirdek dağıtım](https://www.postgresql.org/download/).

Adlı bir veritabanı oluşturmak *eventregistration* ve adlı bir ayrı veritabanı kullanıcı ayarlama *Yöneticisi* parolayla *supersecretpass*.

```bash
CREATE DATABASE eventregistration;
CREATE USER manager WITH PASSWORD 'supersecretpass';
GRANT ALL PRIVILEGES ON DATABASE eventregistration TO manager;
```
Tür *\q* PostgreSQL istemci çıkmak için. 

<a name="step2"></a>

## <a name="create-local-python-flask-application"></a>Yerel Python Flask uygulaması oluşturma

Bu adımda, yerel Python Flask projesi ayarlayın.

### <a name="clone-the-sample-application"></a>Örnek uygulamayı kopyalama

Terminal penceresi açın ve `CD` bir çalışma dizini için.

Örnek depoyu kopyalayın ve Git için aşağıdaki komutları çalıştırın *0,1 initialapp* serbest bırakın.

```bash
git clone https://github.com/Azure-Samples/docker-flask-postgres.git
cd docker-flask-postgres
git checkout tags/0.1-initialapp
```

Bu örnek depo içeren bir [Flask](http://flask.pocoo.org/) uygulama. 

### <a name="run-the-application"></a>Uygulamayı çalıştırma

> [!NOTE] 
> Bir sonraki adımda üretim veritabanı ile kullanmak için Docker kapsayıcısı oluşturarak bu işlemi basitleştirir.

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

Uygulamanın tam olarak yüklendiğinde, aşağıdaki iletiye benzer bir şey görürsünüz:

```bash
INFO  [alembic.runtime.migration] Context impl PostgresqlImpl.
INFO  [alembic.runtime.migration] Will assume transactional DDL.
INFO  [alembic.runtime.migration] Running upgrade  -> 791cd7d80402, empty message
 * Serving Flask app "app"
 * Running on http://127.0.0.1:5000/ (Press CTRL+C to quit)
```

Bir tarayıcıda `http://127.0.0.1:5000` sayfasına gidin. Tıklatın **kaydedin!** ve bir test kullanıcısı oluşturun.

![Yerel olarak çalışan Python Flask uygulama](./media/tutorial-docker-python-postgresql-app/local-app.png)

Flask örnek uygulamanın kullanıcı verilerini veritabanında depolar. Bir kullanıcı kaydetme sırasında başarılı olursa, veri uygulamanızı yerel PostgreSQL veritabanına yazma.

Flask sunucu zaman durdurmak için Ctrl + C terminale yazın. 

## <a name="create-a-production-postgresql-database"></a>Bir üretim PostgreSQL veritabanı oluşturma

Bu adımda, Azure PostgreSQL veritabanında oluşturun. Uygulamanızın Azure'a dağıtıldığında, bu bulut veritabanını kullanır.

### <a name="log-in-to-azure"></a>Azure'da oturum açma

Şimdi kapsayıcıları için Web uygulamasında Python uygulamanızı barındırmak için gereken kaynakları oluşturmak için Azure CLI 2.0 kullanmayı adımıdır.  [az login](/cli/azure/#login) komutuyla Azure aboneliğinizde oturum açın ve ekrandaki yönergeleri izleyin.

```azurecli
az login
```

### <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

[az group create](/cli/azure/group#create) ile bir [kaynak grubu](../../azure-resource-manager/resource-group-overview.md) oluşturun.

[!INCLUDE [Resource group intro](../../../includes/resource-group.md)]

Aşağıdaki örnek Batı ABD bölgesinde bir kaynak grubu oluşturur:

```azurecli-interactive
az group create --name myResourceGroup --location "West US"
```

Kullanım [az appservice listesi-konumları](/cli/azure/appservice#list-locations) listesi kullanılabilir konumlar için Azure CLI komutu.

### <a name="create-an-azure-database-for-postgresql-server"></a>PostgreSQL için Azure Veritabanı sunucusu oluşturma

Bir PostgreSQL sunucusuyla [az postgres sunucusu oluşturmak](/cli/azure/postgres/server#az_postgres_server_create) komutu.

Aşağıdaki komutta için bir benzersiz sunucu adı yerine  *\<postgresql_name >* yer tutucu ve bir kullanıcı adı için  *\<admin_username >* yer tutucu. Sunucu adı PostgreSQL uç noktanızı bir parçası olarak kullanılır (`https://<postgresql_name>.postgres.database.azure.com`), adının Azure içindeki tüm sunucular arasında benzersiz olması gerekir. İlk veritabanı yönetici kullanıcı hesabı için kullanıcı adıdır. Bu kullanıcı için bir parola çekme istenir.

```azurecli-interactive
az postgres server create --resource-group myResourceGroup --name <postgresql_name> --admin-user <admin_username>
```

Azure CLI PostgreSQL sunucu için Azure veritabanı oluşturulduğunda, bilgileri aşağıdaki örneğe benzer şekilde gösterir:

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

### <a name="create-a-firewall-rule-for-the-azure-database-for-postgresql-server"></a>PostgreSQL sunucu için Azure veritabanı için bir güvenlik duvarı kuralı oluşturma

Tüm IP adreslerinden veritabanına erişim için aşağıdaki Azure CLI komutunu çalıştırın.

```azurecli-interactive
az postgres server firewall-rule create --resource-group myResourceGroup --server-name <postgresql_name> --start-ip-address=0.0.0.0 --end-ip-address=255.255.255.255 --name AllowAllIPs
```

Azure CLI aşağıdaki örneğe benzer bir çıktı ile güvenlik duvarı kuralı oluşturmayı onaylar:

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

## <a name="connect-your-python-flask-application-to-the-database"></a>Python Flask uygulamanız veritabanına bağlan

Bu adımda, Python Flask örnek uygulamanızı oluşturduğunuz PostgreSQL server için Azure veritabanına bağlanın.

### <a name="create-an-empty-database-and-set-up-a-new-database-application-user"></a>Boş bir veritabanı oluşturun ve yeni bir veritabanı uygulama kullanıcı ayarlama

Yalnızca tek bir veritabanına erişimi olan bir veritabanı kullanıcısı oluşturmalıdır. Sunucuya uygulama tam erişim verip önlemek için bu kimlik bilgileri kullanın.

(Yönetici parolanızı girmeniz istenir) veritabanına bağlanın.

```bash
psql -h <postgresql_name>.postgres.database.azure.com -U <my_admin_username>@<postgresql_name> postgres
```

Veritabanı ve kullanıcı PostgreSQL CLI oluşturun.

```bash
CREATE DATABASE eventregistration;
CREATE USER manager WITH PASSWORD 'supersecretpass';
GRANT ALL PRIVILEGES ON DATABASE eventregistration TO manager;
```

Tür *\q* PostgreSQL istemci çıkmak için.

### <a name="test-the-application-locally-against-the-azure-postgresql-database"></a>Uygulamayı yerel olarak Azure PostgreSQL veritabanına karşı test etme

Geri şimdi gidip *uygulama* klasörü kopyalanan Github depo veritabanı ortam değişkenleri güncelleştirerek Python Flask uygulamayı çalıştırabilirsiniz.

```bash
FLASK_APP=app.py DBHOST="<postgresql_name>.postgres.database.azure.com" DBUSER="manager@<postgresql_name>" DBNAME="eventregistration" DBPASS="supersecretpass" flask db upgrade
FLASK_APP=app.py DBHOST="<postgresql_name>.postgres.database.azure.com" DBUSER="manager@<postgresql_name>" DBNAME="eventregistration" DBPASS="supersecretpass" flask run
```

Uygulamanın tam olarak yüklendiğinde, aşağıdaki iletiye benzer bir şey görürsünüz:

```bash
INFO  [alembic.runtime.migration] Context impl PostgresqlImpl.
INFO  [alembic.runtime.migration] Will assume transactional DDL.
INFO  [alembic.runtime.migration] Running upgrade  -> 791cd7d80402, empty message
 * Serving Flask app "app"
 * Running on http://127.0.0.1:5000/ (Press CTRL+C to quit)
```

Bir tarayıcıda http://127.0.0.1:5000 gidin. Tıklatın **kaydedin!** ve bir test kaydı oluşturun. Şimdi, Azure veritabanına veri yazma.

![Yerel olarak çalışan Python Flask uygulama](./media/tutorial-docker-python-postgresql-app/local-app.png)

### <a name="running-the-application-from-a-docker-container"></a>Docker kapsayıcıdan uygulamayı çalıştırmayı

Docker kapsayıcısı görüntüsünü oluşturabilirsiniz.

```bash
cd ..
docker build -t flask-postgresql-sample .
```

Docker başarıyla kapsayıcı oluşturulduğu bir onay görüntüler.

```bash
Successfully built 7548f983a36b
```

Bir ortam değişkeni dosyasına veritabanı ortam değişkenleri eklemek *db.env*. Uygulama Azure PostgreSQL üretim veritabanına bağlanır.

```text
DBHOST="<postgresql_name>.postgres.database.azure.com"
DBUSER="manager@<postgresql_name>"
DBNAME="eventregistration"
DBPASS="supersecretpass"
```

Docker kapsayıcısı içinde uygulamadan çalıştırın. Aşağıdaki komutu ortam değişken dosyası belirtir ve yerel bağlantı noktası 5000 için varsayılan Flask bağlantı noktası 5000 eşler.

```bash
docker run -it --env-file db.env -p 5000:5000 flask-postgresql-sample
```

Çıktı ne daha önce gördüğünüzle için benzer. Ancak, ilk veritabanı geçiş artık gerçekleştirilmesi gerekir ve bu nedenle atlanır.

```bash
INFO  [alembic.runtime.migration] Context impl PostgresqlImpl.
INFO  [alembic.runtime.migration] Will assume transactional DDL.
 * Serving Flask app "app"
 * Running on http://0.0.0.0:5000/ (Press CTRL+C to quit)
```

Veritabanı zaten daha önce oluşturduğunuz kayıt içeriyor.

![Docker kapsayıcısı tabanlı Python Flask uygulama yerel olarak çalıştırma](./media/tutorial-docker-python-postgresql-app/local-docker.png)

## <a name="upload-the-docker-container-to-a-container-registry"></a>Docker kapsayıcısı bir kapsayıcı kayıt defterine karşıya yükle

Bu adımda, bir kapsayıcı kayıt defterine Docker kapsayıcısı yükleyin. Kullanım Azure kapsayıcı kayıt defteri, ancak Docker Hub gibi diğer popüler olanlar da kullanabilirsiniz.

### <a name="create-an-azure-container-registry"></a>Azure Container Registry oluşturma

Bir kapsayıcı kayıt defteri oluşturmak için aşağıdaki komutu yerine  *\<registry_name >* tercih ettiğiniz bir benzersiz Azure kapsayıcı kayıt defteri adı.

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

### <a name="retrieve-the-registry-credentials-for-pushing-and-pulling-docker-images"></a>Docker görüntüleri çekme ve itme için kayıt defteri kimlik bilgilerini alma

Kayıt defteri kimlik bilgilerini göstermek için önce yönetici modunu etkinleştirin.

```azurecli-interactive
az acr update --name <registry_name> --admin-enabled true
az acr credential show -n <registry_name>
```

İki parola bakın. Kullanıcı adı ve ilk parola not edin.

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

### <a name="upload-your-docker-container-to-azure-container-registry"></a>Azure kapsayıcı kayıt defterine Docker kapsayıcısı karşıya yükle

```bash
docker login <registry_name>.azurecr.io -u <registry_name> -p "<registry_password>"
docker tag flask-postgresql-sample <registry_name>.azurecr.io/flask-postgresql-sample
docker push <registry_name>.azurecr.io/flask-postgresql-sample
```

## <a name="deploy-the-docker-python-flask-application-to-azure"></a>Docker Python Flask uygulamayı Azure'a dağıtma

Bu adımda, Docker kapsayıcısı tabanlı Python Flask uygulamanızı Azure App Service'e dağıtın.

### <a name="create-an-app-service-plan"></a>App Service planı oluşturma

[az appservice plan create](/cli/azure/appservice/plan#create) komutu ile bir App Service planı oluşturun.

[!INCLUDE [app-service-plan](../../../includes/app-service-plan-linux.md)]

Aşağıdaki örnek, adlandırılmış bir Linux tabanlı uygulama hizmeti planı oluşturur *myAppServicePlan* kullanarak S1 fiyatlandırma katmanı:

```azurecli-interactive
az appservice plan create --name myAppServicePlan --resource-group myResourceGroup --sku S1 --is-linux
```

Uygulama hizmeti planı oluşturulduğunda, Azure CLI bilgileri aşağıdaki örneğe benzer şekilde gösterir:

```json
{
  "adminSiteName": null,
  "appServicePlanName": "myAppServicePlan",
  "geoRegion": "West US",
  "hostingEnvironmentProfile": null,
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Web/serverfarms/myAppServicePlan", 
  "kind": "linux",
  "location": "West US",
  "maximumNumberOfWorkers": 10,
  "name": "myAppServicePlan",
  "numberOfSites": 0,
  "perSiteScaling": false,
  "provisioningState": "Succeeded",
  "reserved": true,
  "resourceGroup": "myResourceGroup",
  "sku": {
    "capabilities": null,
    "capacity": 1,
    "family": "S",
    "locations": null,
    "name": "S1",
    "size": "S1",
    "skuCapacity": null,
    "tier": "Standard"
  },
  "status": "Ready",
  "subscription": "00000000-0000-0000-0000-000000000000",
  "tags": null,
  "targetWorkerCount": 0,
  "targetWorkerSizeId": 0,
  "type": "Microsoft.Web/serverfarms",
  "workerTierName": null
}
```

### <a name="create-a-web-app"></a>Web uygulaması oluşturma

Bir web uygulaması oluşturma *myAppServicePlan* uygulama hizmeti planıyla [az webapp oluşturmak](/cli/azure/webapp#create) komutu.

Web uygulaması kodunuzu dağıtmaya barındırma bir alan sağlar ve dağıtılmış uygulamanın görüntülemek bir URL sağlar. Web uygulaması oluşturmak için kullanın.

Aşağıdaki komutta,  *\<app_name >* yer tutucu benzersiz bir uygulama adına sahip. Bu ad web uygulaması için varsayılan URL parçası kitabıdır adının tüm Azure uygulama hizmetinde uygulamalar arasında benzersiz olması gerekir.

```azurecli
az webapp create --name <app_name> --resource-group myResourceGroup --plan myAppServicePlan
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

### <a name="configure-the-database-environment-variables"></a>Veritabanı ortam değişkenlerini yapılandırın

Öğreticide daha önce PostgreSQL veritabanına bağlanmak için ortam değişkenleri tanımlı.

Ortam değişkenleri olarak ayarladığınız App Service'te _uygulama ayarları_ kullanarak [az webapp config appsettings kümesi](/cli/azure/webapp/config#set) komutu.

Aşağıdaki örnek veritabanı bağlantı ayrıntıları uygulama ayarlarını belirtir. Ayrıca kullanır *bağlantı noktası* değişkeni eşlemesine bağlantı noktası 5000 bağlantı noktası 80 üzerinde HTTP trafiği almak için Docker kapsayıcısı.

```azurecli-interactive
az webapp config appsettings set --name <app_name> --resource-group myResourceGroup --settings DBHOST="<postgresql_name>.postgres.database.azure.com" DBUSER="manager@<postgresql_name>" DBPASS="supersecretpass" DBNAME="eventregistration" PORT=5000
```

### <a name="configure-docker-container-deployment"></a>Docker kapsayıcısı dağıtımını yapılandırma

Uygulama hizmeti otomatik olarak karşıdan yükle ve Docker kapsayıcısı çalıştırın.

```azurecli
az webapp config container set --resource-group myResourceGroup --name <app_name> --docker-registry-server-user "<registry_name>" --docker-registry-server-password "<registry_password>" --docker-custom-image-name "<registry_name>.azurecr.io/flask-postgresql-sample" --docker-registry-server-url "https://<registry_name>.azurecr.io"
```

Docker kapsayıcısı güncelleştirmek veya ayarlarını değiştirmek olduğunda, uygulamayı yeniden başlatın. Yeniden başlatma, tüm ayarları uygulanır ve en son kapsayıcı kayıt defterinden çekilir sağlar.

```azurecli-interactive
az webapp restart --resource-group myResourceGroup --name <app_name>
```

### <a name="browse-to-the-azure-web-app"></a>Azure web uygulaması'na göz atın 

Web tarayıcınız üzerinden dağıtılan web uygulamasının göz atın. 

```bash 
http://<app_name>.azurewebsites.net 
```
> [!NOTE]
> Web uygulaması kapsayıcı indirilir ve kapsayıcı yapılandırma değiştirildikten sonra başlatıldı gerektiğinden yük daha uzun sürer.

Önceki adımda Azure üretim veritabanına kaydedildi önceden kaydedilmiş konuklar bakın.

![Docker kapsayıcısı tabanlı Python Flask uygulama yerel olarak çalıştırma](./media/tutorial-docker-python-postgresql-app/docker-app-deployed.png)

**Tebrikler!** Azure App Service'te bir Docker kapsayıcısı tabanlı bir Python Flask uygulamanın çalıştırdığınız.

## <a name="update-data-model-and-redeploy"></a>Güncelleştirme veri modeli ve yeniden dağıtın

Bu adımda, Konuk modeli güncelleştirerek her olay kaydı katılanların sayısı ekleyin.

Kullanıma *0.2 geçiş* yayın aşağıdaki git komutu ile:

```bash
git checkout tags/0.2-migration
```

Bu sürüm zaten gerekli görünümleri, denetleyicileri ve modeli için yapılan değişiklikler. Ayrıca aracılığıyla oluşturulan bir veritabanı geçiş içerir *alembic* (`flask db migrate`). Aşağıdaki git komutu yapılan tüm değişiklikleri görebilirsiniz:

```bash
git diff 0.1-initialapp 0.2-migration
```

### <a name="test-your-changes-locally"></a>Yaptığınız değişiklikler yerel olarak test etme

Değişikliklerinizi flask sunucunun yerel olarak çalıştırarak test etmek için aşağıdaki komutları çalıştırın.

Mac / Linux:
```bash
source venv/bin/activate
cd app
FLASK_APP=app.py DBHOST="localhost" DBUSER="manager" DBNAME="eventregistration" DBPASS="supersecretpass" flask db upgrade
FLASK_APP=app.py DBHOST="localhost" DBUSER="manager" DBNAME="eventregistration" DBPASS="supersecretpass" flask run
```

Değişiklikleri görmek için tarayıcınızda http://127.0.0.1:5000 gidin. Bir test kaydı oluşturun.

![Docker kapsayıcısı tabanlı Python Flask uygulama yerel olarak çalıştırma](./media/tutorial-docker-python-postgresql-app/local-app-v2.png)

### <a name="publish-changes-to-azure"></a>Değişiklikler için Azure yayımlama

Yeni docker görüntüsünü oluşturabilirsiniz, kapsayıcı kayıt defterine gönderme ve uygulamayı yeniden başlatın.

```bash
docker build -t flask-postgresql-sample .
docker tag flask-postgresql-sample <registry_name>.azurecr.io/flask-postgresql-sample
docker push <registry_name>.azurecr.io/flask-postgresql-sample
az appservice web restart --resource-group myResourceGroup --name <app_name>
```

Azure web uygulamasına gidin ve yeni işlevselliği yeniden deneyin. Başka bir olay kaydı oluşturun.

```bash 
http://<app_name>.azurewebsites.net 
```

![Docker Python Flask uygulamada Azure uygulama hizmeti](./media/tutorial-docker-python-postgresql-app/docker-flask-in-azure.png)

## <a name="manage-your-azure-web-app"></a>Azure web uygulamanızı yönetme

Git [Azure portal](https://portal.azure.com) oluşturduğunuz web uygulaması görmek için.

Sol menüden **Uygulama Hizmetleri**’ne ve ardından Azure web uygulamanızın adına tıklayın.

![Portaldan Azure web uygulamasına gitme](./media/tutorial-docker-python-postgresql-app/app-resource.png)

Varsayılan olarak, portal, web uygulamanızın gösterir **genel bakış** sayfası. Bu sayfa, uygulamanızın nasıl çalıştığını gösterir. Buradan ayrıca göz atma, durdurma, başlatma, yeniden başlatma ve silme gibi temel yönetim görevlerini gerçekleştirebilirsiniz. Sayfanın sol tarafında sekmeleri açabilir farklı yapılandırma sayfalarında gösterilir.

![Azure portalında App Service sayfası](./media/tutorial-docker-python-postgresql-app/app-mgmt.png)

## <a name="next-steps"></a>Sonraki adımlar

Web uygulamanıza özel bir DNS adı eşleme öğrenmek için sonraki öğretici ilerleyin.

> [!div class="nextstepaction"]
> [Mevcut bir özel DNS adını Azure Web Apps ile eşleme](../app-service-web-tutorial-custom-domain.md)
