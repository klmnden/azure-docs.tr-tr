---
title: Azure App Service'te Python ve PostgreSQL web uygulaması oluşturma | Microsoft Docs
description: Azure'da bir PostgreSQL veritabanına bağlantısı olan veri temelli bir Python uygulamasını nasıl çalıştıracağınızı öğrenin.
services: app-service\web
documentationcenter: python
author: berndverst
manager: jeconnoc
ms.service: app-service-web
ms.workload: web
ms.devlang: python
ms.topic: tutorial
ms.date: 07/13/2018
ms.author: beverst;cephalin
ms.custom: mvc
ms.openlocfilehash: ce84498ab89891bd7b96cfcc6b0c7ac029c93cbd
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/02/2018
ms.locfileid: "39423088"
---
# <a name="build-a-docker-python-and-postgresql-web-app-in-azure"></a>Azure'da Docker Python ve PostgreSQL web uygulaması oluşturma

Kapsayıcılar için Web App yüksek oranda ölçeklenebilen, kendi kendine düzeltme eki uygulayan bir web barındırma hizmeti sunar. Bu öğreticide veritabanı arka ucu olarak PostgreSQL kullanan veri temelli bir Python web uygulamasının nasıl oluşturulacağı gösterilmektedir. İşiniz bittiğinde, [Linux üzerinde App Service](app-service-linux-intro.md)'te bir Docker kapsayıcısı içinde çalışan bir Python Flask uygulamanız olur.

![Linux üzerinde App Service’te Docker Python Flask uygulaması](./media/tutorial-docker-python-postgresql-app/docker-flask-in-azure.png)

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Azure’da PostgreSQL veritabanı oluşturma
> * Python uygulamasını PostgreSQL’e bağlama
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

Yerel bir terminal penceresinde `psql` komutunu çalıştırarak yerel PostgreSQL sunucunuza bağlanın.

```bash
sudo -u postgres psql
```

Bağlantınız başarılı olursa, PostgreSQL veritabanınız çalışır. Aksi takdirde, yerel PostgresQL veritabanınızın [İndirmeler - PostgreSQL Çekirdek Dağıtım](https://www.postgresql.org/download/) konusundaki adımlar izlenilerek başlatıldığından emin olun.

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

Örnek depoyu kopyalamak ve *0.1-initialapp* sürümüne gitmek için aşağıdaki komutları çalıştırın.

```bash
git clone https://github.com/Azure-Samples/docker-flask-postgres.git
cd docker-flask-postgres
git checkout tags/0.1-initialapp
```

Bu örnek depo, bir [Flask](http://flask.pocoo.org/) uygulaması içerir. 

### <a name="run-the-app-locally"></a>Uygulamayı yerel olarak çalıştırma

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

[!INCLUDE [Create resource group](../../../includes/app-service-web-create-resource-group-linux-no-h.md)] 

### <a name="create-an-azure-database-for-postgresql-server"></a>PostgreSQL için Azure Veritabanı sunucusu oluşturma

Cloud Shell'de [`az postgres server create`](/cli/azure/postgres/server?view=azure-cli-latest#az-postgres-server-create) komutuyla bir PostgreSQL sunucusu oluşturun.

Aşağıdaki örnek komutta *\<postgresql_name>* yerine benzersiz bir sunucu adı, *\<admin_username>* ve *\<admin_password>* yerine de kullanmak istediğiniz kullanıcı bilgilerini yazın. Sunucu adı, PostgreSQL uç noktasının bir parçası olan `https://<postgresql_name>.postgres.database.azure.com` olarak kullanıldığından, adın Azure’daki tüm sunucularda benzersiz olması gerekir. Kullanıcı kimlik bilgileri, veritabanı yönetici kullanıcısı için geçerli olacaktır. 

```azurecli-interactive
az postgres server create --resource-group myResourceGroup --name <postgresql_name> --location "West Europe" --admin-user <admin_username> --admin-password <admin_password> --sku-name GP_Gen4_2
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

### <a name="create-a-firewall-rule-for-the-postgresql-server"></a>PostgreSQL için bir güvenlik duvarı kuralı oluşturma

Tüm IP adreslerinden veritabanına erişim izni vermek için Cloud Shell'de aşağıdaki Azure CLI komutunu çalıştırın. Hem başlangıç hem bitiş IP’si `0.0.0.0` olarak ayarlandığında, güvenlik duvarı yalnızca diğer Azure kaynakları için açılır. 

```azurecli-interactive
az postgres server firewall-rule create --resource-group myResourceGroup --server-name <postgresql_name> --start-ip-address=0.0.0.0 --end-ip-address=0.0.0.0 --name AllowAzureIPs
```

> [!TIP] 
> [Yalnızca uygulamanızın kullandığı giden IP adreslerini kullanarak](../app-service-ip-addresses.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json#find-outbound-ips) güvenlik duvarı kurallarınızda daha da kısıtlayıcı olabilirsiniz.
>

Cloud Shell'de *\<you_ip_address>* yerine [yerel IPv4 IP adresinizi](https://whatismyipaddress.com/) yazdıktan sonra komutu tekrar çalıştırarak yerel bilgisayarınızdan veritabanına erişim izni verin. 

```azurecli-interactive
az postgres server firewall-rule create --resource-group myResourceGroup --server-name <postgresql_name> --start-ip-address=<you_ip_address> --end-ip-address=<you_ip_address> --name AllowLocalClient
```

## <a name="connect-python-app-to-production-database"></a>Python uygulamasını üretim veritabanına bağlama

Bu adımda, örnek Flask uygulamanızı oluşturduğunuz PostgreSQL için Azure Veritabanı sunucusuna bağlarsınız.

### <a name="create-empty-database-and-user-access"></a>Boş veritabanı oluşturma ve kullanıcı erişimi sağlama

Cloud Shell'de `psql` komutunu çalıştırarak veritabanına bağlanın. Yönetici parolanızı girmeniz istendiğinde [PostgreSQL için Azure Veritabanı sunucusu oluşturma](#create-an-azure-database-for-postgresql-server) bölümünde belirttiğiniz parolayı kullanın.

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

![Yerel olarak çalışan Python Flask uygulaması](./media/tutorial-docker-python-postgresql-app/local-app.png)

## <a name="upload-app-to-a-container-registry"></a>Uygulamayı kapsayıcı kayıt defterine yükleme

Bu adımda bir Docker görüntüsü oluşturacak ve bunu Azure Container Registry'ye yükleyeceksiniz. Docker Hub gibi popüler kayıt defterlerini de kullanabilirsiniz.

### <a name="build-the-docker-image-and-test-it"></a>Docker görüntüsü oluşturma ve test etme

Yerel terminal penceresinde Docker görüntüsünü oluşturun.

```bash
cd ..
docker build -t flask-postgresql-sample .
```

Docker, görüntünün başarıyla oluşturulduğuna ilişkin bir onay görüntüler.

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

Görüntüyü yerel ortamdaki bir Docker kapsayıcısında çalıştırın. Aşağıdaki komut ortam değişkeni dosyasını belirtir ve varsayılan 5000 numaralı Flask bağlantı noktasını yerel 5000 numaralı bağlantı noktasına eşler.

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

Kapsayıcının yerel ortamda çalıştığını doğruladığınıza göre _db.env_ dosyasını silebilirsiniz. Azure App Service'te ortam değişkenlerini tanımlamak için uygulama ayarlarını kullanacaksınız.  

### <a name="create-an-azure-container-registry"></a>Azure Container Registry oluşturma

Cloud Shell'de aşağıdaki komutu kullanarak Azure Container Registry'de bir kayıt defteri oluşturun. *\<registry_name>* yerine benzersiz bir kayıt defteri adı yazın.

```azurecli-interactive
az acr create --name <registry_name> --resource-group myResourceGroup --location "West US" --sku Basic
```

### <a name="retrieve-registry-credentials"></a>Kayıt defteri kimlik bilgilerini alma

Kayıt defteri kimlik bilgilerini almak için Cloud Shell'de aşağıdaki komutları çalıştırın. Görüntüleri çekmek ve göndermek için bu bilgilere ihtiyacınız olacak.

```azurecli-interactive
az acr update --name <registry_name> --admin-enabled true
az acr credential show -n <registry_name>
```

Çıkışta iki parola göreceksiniz. Kullanıcı adını (varsayılan olarak kayıt defteri adıdır) ve ilk parolayı not edin.

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

### <a name="upload-docker-image-to-registry"></a>Docker görüntüsünü kayıt defterine yükleme

Yerel terminal penceresinde `docker` komutunu kullanarak yeni kayıt defterinizde oturum açın. İstendiğinde, aldığınız parolayı girin.

```bash
docker login <registry_name>.azurecr.io -u <registry_name>
```

Docker görüntünüzü kayıt defterine gönderin.

```bash
docker tag flask-postgresql-sample <registry_name>.azurecr.io/flask-postgresql-sample
docker push <registry_name>.azurecr.io/flask-postgresql-sample
```

## <a name="create-web-app-with-uploaded-image"></a>Yüklenen görüntüyle web uygulaması oluşturma

Bu adımda Azure App Service'te bir uygulama oluşturacak ve Azure Container Registry'ye yüklediğiniz Docker görüntüsünü kullanacak şekilde yapılandıracaksınız.

### <a name="create-an-app-service-plan"></a>App Service planı oluşturma

[!INCLUDE [Create app service plan](../../../includes/app-service-web-create-app-service-plan-linux-no-h.md)]

### <a name="create-a-web-app"></a>Web uygulaması oluşturma

Cloud Shell'de *myAppServicePlan* App Service planında [`az webapp create`](/cli/azure/webapp?view=azure-cli-latest#az-webapp-create) komutuyla bir web uygulaması oluşturun.

Aşağıdaki komutta, *\<app_name>* yer tutucusunu benzersiz bir uygulama adıyla değiştirin. Bu ad web uygulamasına ilişkin varsayılan URL'nin bir parçasıdır; dolayısıyla, Azure App Service'teki tüm uygulamalar arasında benzersiz olmalıdır.

```azurecli-interactive
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

### <a name="configure-environment-variables"></a>Ortam değişkenlerini yapılandırma

Öğreticinin önceki bölümlerinde, PostgreSQL veritabanınıza bağlanmak üzere ortam değişkenleri tanımladınız.

App Service'te, [`az webapp config appsettings set`](/cli/azure/webapp/config/appsettings?view=azure-cli-latest#az-webapp-config-appsettings-set) komutunu kullanıp ortam değişkenlerini _uygulama ayarları_ olarak belirlersiniz.

Şu örnek, veritabanı bağlantı ayrıntılarını uygulama ayarları olarak belirtir. Ayrıca kapsayıcının 5000 numaralı bağlantı noktası için *WEBSITES_PORT* değişkenini kullanarak kapsayıcının 80 numaralı bağlantı noktasından HTTP trafiği almasına izin verir.

```azurecli-interactive
az webapp config appsettings set --name <app_name> --resource-group myResourceGroup --settings DBHOST="<postgresql_name>.postgres.database.azure.com" DBUSER="manager@<postgresql_name>" DBPASS="supersecretpass" DBNAME="eventregistration" WEBSITES_PORT=5000
```

### <a name="configure-custom-container-deployment"></a>Özel kapsayıcı dağıtımını yapılandırma

Kapsayıcı görüntüsü adını belirtilmiş olmanıza rağmen özel kayıt defteri URL'sini ve kullanıcı kimlik bilgilerini de belirtmeniz gerekir. Cloud Shell'de [az webapp config container set](/cli/azure/webapp/config/container?view=azure-cli-latest#az-webapp-config-container-set) komutunu çalıştırın.

```azurecli-interactive
az webapp config container set --resource-group myResourceGroup --name <app_name> --docker-registry-server-user "<registry_name>" --docker-registry-server-password "<registry_password>" --docker-registry-server-url "https://<registry_name>.azurecr.io"
```

Cloud Shell'de uygulamayı yeniden başlatın. Yeniden başlatma, tüm ayarların uygulandığından ve kayıt defterinden en son kapsayıcının alındığından emin olmanızı sağlar.

```azurecli-interactive
az webapp restart --resource-group myResourceGroup --name <app_name>
```

### <a name="browse-to-the-azure-web-app"></a>Azure web uygulamasına göz atma 

Dağıtılan web uygulamasına göz atın. 

```bash 
http://<app_name>.azurewebsites.net 
```

> [!NOTE]
> Web uygulaması ilk kez çağrıldığında kapsayıcının indirilmesi ve çalışması gerektiğinden başlaması biraz uzun sürebilir. Uzun bir süre bekledikten sonra hata görürseniz sayfayı yenilemeniz yeterlidir.

Önceki adımda önceden kaydedilmiş konukların Azure üretim veritabanına kaydedildiğini görürsünüz.

![Yerel olarak çalışan Docker kapsayıcısı tabanlı Python Flask uygulaması](./media/tutorial-docker-python-postgresql-app/docker-app-deployed.png)

**Tebrikler!** Kapsayıcılar için Web App'te bir Python uygulaması çalıştırıyorsunuz.

## <a name="update-data-model-and-redeploy"></a>Veri modelini güncelleştirme ve yeniden dağıtma

Bu adımda, `Guest` modelini güncelleştirerek etkinlik kayıtlarına katılanların sayısını ekliyorsunuz.

Yerel terminal penceresinde aşağıdaki git komutunu kullanarak *0.2-migration* sürümünü kullanıma alın:

```bash
git checkout tags/0.2-migration
```

Bu sürüm modelde, görünümlerde ve denetleyicilerde gerekli değişiklikleri zaten yapmıştır. Ayrıca *alembic* (`flask db migrate`) aracılığıyla oluşturulan bir veritabanı geçişi içerir. Aşağıdaki git komutu aracılığıyla yapılan tüm değişiklikleri görebilirsiniz:

```bash
git diff 0.1-initialapp 0.2-migration
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

![Yerel olarak çalışan Docker kapsayıcısı tabanlı Python Flask uygulaması](./media/tutorial-docker-python-postgresql-app/local-app-v2.png)

### <a name="publish-changes-to-azure"></a>Değişiklikleri Azure’da yayımlama

Yerel terminal penceresinde yeni Docker görüntüsünü oluşturun ve kayıt defterinize gönderin.

```bash
cd ..
docker build -t flask-postgresql-sample .
docker tag flask-postgresql-sample <registry_name>.azurecr.io/flask-postgresql-sample
docker push <registry_name>.azurecr.io/flask-postgresql-sample
```

Cloud Shell'de uygulamayı yeniden başlatarak kayıt defterinden en son kapsayıcının çekilmesini sağlayın.

```azurecli-interactive
az webapp restart --resource-group myResourceGroup --name <app_name>
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
