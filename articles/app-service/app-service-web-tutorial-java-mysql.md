---
title: Azure'da Java ve MySQL web uygulaması oluşturma
description: Azure uygulama hizmetinde çalışan Azure MySQL veritabanına bağlanacak bir Java uygulaması edinmeyi öğrenin.
services: app-service\web
documentationcenter: Java
author: bbenz
manager: jeffsand
editor: jasonwhowell
ms.assetid: ''
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: java
ms.topic: tutorial
ms.date: 05/22/2017
ms.author: bbenz
ms.custom: mvc
ms.openlocfilehash: 0712035f317adb318d60285637526f951bf5bdec
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
# <a name="tutorial-build-a-java-and-mysql-web-app-in-azure"></a>Öğretici: Azure’da Java ve MySQL web uygulaması derleme

> [!NOTE]
> Bu makalede bir uygulamanın Windows üzerinde App Service'e dağıtımı yapılır. App Service'i _Linux_ üzerinde dağıtmak için bkz. [Kapsayıcılı bir Spring Boot uygulamasını Azure'a dağıtma](/java/azure/spring-framework/deploy-containerized-spring-boot-java-app-with-maven-plugin).
>

Bu öğreticide size, Azure’da bir Java web uygulaması oluşturma ve bu uygulamayı bir MySQL veritabanına bağlamayla ilgili yönergeler verilmiştir. İşiniz bittiğinde, [Azure App Service Web Apps](app-service-web-overview.md) üzerinde çalıştırılan [MySQL için Azure Veritabanı](https://docs.microsoft.com/azure/mysql/overview)'nda veri depolayan bir [Spring Boot](https://projects.spring.io/spring-boot/) uygulamanız olacaktır.

![Azure uygulama hizmetinde çalıştırılan Java uygulaması](./media/app-service-web-tutorial-java-mysql/appservice-web-app.png)

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Azure’da MySQL veritabanı oluşturma
> * Örnek uygulamayı veritabanına bağlama
> * Uygulamayı Azure’da dağıtma
> * Uygulamayı güncelleştirme ve yeniden dağıtma
> * Azure’daki tanılama günlüklerinin akışını sağlama
> * Azure Portal’da uygulamayı izleme

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Ön koşullar

1. [Git'i indirin ve yükleyin](https://git-scm.com/)
1. [Java 7 JDK veya üstünü indirin ve yükleyin](http://www.oracle.com/technetwork/java/javase/downloads/index.html)
1. [MySQL'i indirin, yükleyin ve başlatın](https://dev.mysql.com/doc/refman/5.7/en/installing.html) 

## <a name="prepare-local-mysql"></a>Yerel MySQL hazırlama 

Bu adımda, uygulamayı makinenizde yerel olarak test ederken kullanmak üzere yerel MySQL sunucusunda bir veritabanı oluşturursunuz.

### <a name="connect-to-mysql-server"></a>MySQL sunucusuna bağlanma

Bir terminal penceresinde yerel MySQL sunucunuza bağlanın. Bu öğreticideki tüm komutları çalıştırmak için bu terminal penceresini kullanabilirsiniz.

```bash
mysql -u root -p
```

Parola istenirse `root` hesabının parolasını girin. Kök hesap parolanızı hatırlamıyorsanız bkz [MySQL: Kök Parolayı Sıfırlama](https://dev.mysql.com/doc/refman/5.7/en/resetting-permissions.html).

Komutunuz başarıyla çalışırsa, MySQL sunucunuz zaten çalışıyor demektir. Çalışmıyorsa, yerel MySQL sunucunuzun aşağıdaki [MySQL yükleme sonrası adımları](https://dev.mysql.com/doc/refman/5.7/en/postinstallation.html) kullanılarak başlatıldığından emin olun.

### <a name="create-a-database"></a>Veritabanı oluşturma 

`mysql` isteminde, yapılacaklar öğeleri için bir veritabanı ve tablo oluşturun.

```sql
CREATE DATABASE tododb;
```

`quit` yazarak sunucu bağlantınızdan çıkış yapın.

```sql
quit
```

## <a name="create-and-run-the-sample-app"></a>Örnek uygulamayı oluşturma ve çalıştırma 

Bu adımda, örnek Spring Boot uygulamasını kopyalar, yerel MySQL veritabanını kullanacak şekilde yapılandırır ve bilgisayarınızda çalıştırırsınız. 

### <a name="clone-the-sample"></a>Örneği kopyalama

Terminal penceresinde, bir çalışma dizinine gidin ve örnek depoyu kopyalayın. 

```bash
git clone https://github.com/azure-samples/mysql-spring-boot-todo
```

### <a name="configure-the-app-to-use-the-mysql-database"></a>Uygulamayı MySQL veritabanını kullanacak şekilde yapılandırma

*spring-boot-mysql-todo/src/main/resources/application.properties* içindeki `spring.datasource.password` ve değeri, MySQL istemini açmak için kullanılan aynı kök parolayla güncelleştirin:

```
spring.datasource.password=mysqlpass
```

### <a name="build-and-run-the-sample"></a>Örneği derleme ve çalıştırma

Depoya eklenmiş olan Maven sarmalayıcısını kullanarak örneği derleyin ve çalıştırın:

```bash
cd spring-boot-mysql-todo
mvnw package spring-boot:run
```

Örneği iş başında görmek için tarayıcınızı `http://localhost:8080` adresinde açın. Listeye görevleri eklerken, MySQL'de depolanan verileri görüntülemek için MySQL komut isteminde aşağıdaki SQL komutlarını kullanın.

```SQL
use testdb;
select * from todo_item;
```

Terminalde `Ctrl`+`C` tuşlarına basarak uygulamayı durdurun. 

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="create-an-azure-mysql-database"></a>Azure MySQL veritabanı oluşturma

bu adımda, [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli) kullanarak [MySQL için Azure Veritabanı](../mysql/quickstart-create-mysql-server-database-using-azure-cli.md) oluşturursunuz. Öğreticinin devamında, örnek uygulamayı bu veritabanını kullanacak şekilde yapılandırırsınız.

### <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

[`az group create`](/cli/azure/group#az_group_create) komutuyla bir [kaynak grubu](../azure-resource-manager/resource-group-overview.md) oluşturun. Azure kaynak grubu; web uygulamaları, veritabanları ve depolama hesapları gibi ilgili kaynakların dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır. 

Aşağıdaki örnekte Kuzey Avrupa bölgesinde bir kaynak grubu oluşturulmaktadır:

```azurecli-interactive
az group create --name myResourceGroup --location "North Europe"
```    

`--location` için hangi olası değerleri kullanabileceğinizi görmek için [`az appservice list-locations`](/cli/azure/appservice#list-locations)omutunu kullanın.

### <a name="create-a-mysql-server"></a>MySQL sunucusu oluşturma

Cloud Shell’de, MySQL için Azure Veritabanı içinde [`az mysql server create`](/cli/azure/mysql/server#az_mysql_server_create) komutuyla bir sunucu oluşturun. `<mysql_server_name>` yer tutucusunu gördüğünüz yerde kendi benzersiz MySQL sunucunuzun adıyla değiştirin. Bu ad, MySQL sunucusu konak adınızın (`<mysql_server_name>.mysql.database.azure.com`) bir parçasıdır ve genel olarak benzersiz olması gerekir. Ayrıca `<admin_user>` ve `<admin_password>` değerlerini de kendi değerlerinizle değiştirin.

```azurecli-interactive
az mysql server create --name <mysql_server_name> --resource-group myResourceGroup --location "North Europe" --admin-user <admin_user> --admin-password <admin_password>
```

MySQL sunucusu oluşturulduğunda Azure CLI, aşağıdaki örneğe benzer bilgiler gösterir:

```json
{
  "administratorLogin": "admin_user",
  "administratorLoginPassword": null,
  "fullyQualifiedDomainName": "mysql_server_name.mysql.database.azure.com",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.DBforMySQL/servers/mysql_server_name",
  "location": "northeurope",
  "name": "mysql_server_name",
  "resourceGroup": "mysqlJavaResourceGroup",
  ...
  < Output has been truncated for readability >
}
```

### <a name="configure-server-firewall"></a>Sunucu güvenlik duvarını yapılandırma

Cloud Shell’de, [`az mysql server firewall-rule create`](/cli/azure/mysql/server/firewall-rule#az_mysql_server_firewall_rule_create) komutunu kullanarak MySQL sunucunuzun istemci bağlantılarına izin vermesi için bir güvenlik duvarı kuralı oluşturun. 

```azurecli-interactive
az mysql server firewall-rule create --name allIPs --server <mysql_server_name> --resource-group myResourceGroup --start-ip-address 0.0.0.0 --end-ip-address 255.255.255.255
```

> [!NOTE]
> MySQL için Azure Veritabanı şu anda Azure hizmetlerinden gelen bağlantıları otomatik olarak etkinleştirmemektedir. Azure’daki IP adresleri dinamik olarak atandığından, şimdilik tüm IP adreslerinin etkinleştirilmesi daha iyi olur. Veritabanınızın güvenliğini sağlamak için daha iyi yöntemler etkinleştirilecektir.

## <a name="configure-the-azure-mysql-database"></a>Azure MySQL veritabanını yapılandırma

Yerel terminal penceresinde, Azure’da MySQL sunucusuna bağlanın. Daha önce `<admin_user>` ve `<mysql_server_name>` için belirttiğiniz değeri kullanın.

```bash
mysql -u <admin_user>@<mysql_server_name> -h <mysql_server_name>.mysql.database.azure.com -P 3306 -p
```

### <a name="create-a-database"></a>Veritabanı oluşturma 

`mysql` isteminde, yapılacaklar öğeleri için bir veritabanı ve tablo oluşturun.

```sql
CREATE DATABASE tododb;
```

### <a name="create-a-user-with-permissions"></a>İzinleri olan bir kullanıcı oluşturma

Bir veritabanı kullanıcısı oluşturun ve bu kullanıcıya `tododb` veritabanındaki tüm ayrıcalıkları verin. `<Javaapp_user>` ve `<Javaapp_password>` yer tutucularını kendi benzersiz uygulama adınızla değiştirin.

```sql
CREATE USER '<Javaapp_user>' IDENTIFIED BY '<Javaapp_password>'; 
GRANT ALL PRIVILEGES ON tododb.* TO '<Javaapp_user>';
```

`quit` yazarak sunucu bağlantınızdan çıkış yapın.

```sql
quit
```

## <a name="deploy-the-sample-to-azure-app-service"></a>Örneği Azure App Service’e dağıtma

[`az appservice plan create`](/cli/azure/appservice/plan#az_appservice_plan_create) CLI komutunu kullanarak **ÜCRETSİZ** fiyatlandırma katmanıyla bir Azure App Service planı oluşturun. Uygulama hizmeti planı, uygulamalarınızı barındırmak için kullanılan fiziksel kaynakları tanımlar. Uygulama hizmeti planına atanan tüm uygulamalar bu kaynakları paylaşarak birden çok uygulamayı barındırırken, maliyetten tasarruf etmenize imkan sağlar. 

```azurecli-interactive
az appservice plan create --name myAppServicePlan --resource-group myResourceGroup --sku FREE
```

Plan hazır olduğunda, Azure CLI aşağıdaki örnekte gösterilene benzer bir çıkış görüntüler:

```json
{ 
  "adminSiteName": null,
  "appServicePlanName": "myAppServicePlan",
  "geoRegion": "North Europe",
  "hostingEnvironmentProfile": null,
  "id": "/subscriptions/0000-0000/resourceGroups/myResourceGroup/providers/Microsoft.Web/serverfarms/myAppServicePlan",
  "kind": "app",
  "location": "North Europe",
  "maximumNumberOfWorkers": 1,
  "name": "myAppServicePlan",
  ...
  < Output has been truncated for readability >
} 
``` 

### <a name="create-an-azure-web-app"></a>Azure Web uygulaması oluşturma

Cloud Shell'de, [`az webapp create`](/cli/azure/appservice/web#az_appservice_web_create) CLI komutunu kullanarak `myAppServicePlan` App Service planında bir web uygulaması tanımı oluşturun. Web uygulaması tanımı, uygulamanıza erişebilmek için bir URL sağlar ve çeşitli seçenekleri yapılandırarak kodunuzu Azure'a dağıtır. 

```azurecli-interactive
az webapp create --name <app_name> --resource-group myResourceGroup --plan myAppServicePlan
```

`<app_name>` yer tutucusunu kendi benzersiz uygulama adınızla değiştirin. Bu benzersiz ad, web uygulamasının varsayılan etki alanı adının bir parçası olduğundan, adın Azure’daki tüm uygulamalarda benzersiz olması gerekir. Uygulamanızı kullanıcılarınızın kullanımına sunmadan önce web uygulamasına bir özel etki alanı adı girdisi eşleyebilirsiniz.

Web uygulaması tanımı hazır olduğunda Azure CLI aşağıda yer alan örnekteki gibi bilgiler gösterir: 

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

### <a name="configure-java"></a>Java'yı yapılandırma 

Cloud Shell'de, [`az webapp config set`](/cli/azure/webapp/config#az_webapp_config_set) komutuyla uygulamanıza gereken Java çalışma zamanı yapılandırmasını ayarlayın.

Aşağıdaki komut web uygulamasını yeni Java 8 JDK ve [Apache Tomcat](http://tomcat.apache.org/) 8.0 üzerinde çalıştırılacak şekilde yapılandırır.

```azurecli-interactive
az webapp config set --name <app_name> --resource-group myResourceGroup --java-version 1.8 --java-container Tomcat --java-container-version 8.0
```

### <a name="configure-the-app-to-use-the-azure-sql-database"></a>Uygulamayı Azure SQL veritabanını kullanacak şekilde yapılandırma

Örnek uygulamayı çalıştırmadan önce, web uygulamasında uygulama ayarlarını Azure'da oluşturduğunuz Azure MySQL veritabanını kullanacak şekilde belirleyin. Bu özellikler web uygulamasına ortam değişkenleri olarak gösterilir ve paketli web uygulamasının içindeki application.properties'de ayarlanan değerleri geçersiz kılar. 

Cloud Shell'de, CLI'de [`az webapp config appsettings`](https://docs.microsoft.com/cli/azure/appservice/web/config/appsettings) komutunu kullanarak uygulama ayarlarını belirleyin:

```azurecli-interactive
az webapp config appsettings set --settings SPRING_DATASOURCE_URL="jdbc:mysql://<mysql_server_name>.mysql.database.azure.com:3306/tododb?verifyServerCertificate=true&useSSL=true&requireSSL=false" --resource-group myResourceGroup --name <app_name>
```

```azurecli-interactive
az webapp config appsettings set --settings SPRING_DATASOURCE_USERNAME=Javaapp_user@mysql_server_name --resource-group myResourceGroup --name <app_name>
```

```azurecli-interactive
az webapp config appsettings set --settings SPRING_DATASOURCE_PASSWORD=Javaapp_password --resource-group myResourceGroup --name <app_name>
```

### <a name="get-ftp-deployment-credentials"></a>FTP dağıtım kimlik bilgilerini alma 
Uygulamanızı FTP, yerel Git, GitHub, Visual Studio Team Services ve Bitbucket gibi çeşitli yollarla Azure uygulama hizmetine dağıtabilirsiniz. Bu örnekte, daha önce yerel makinenizde oluşturulan .WAR dosyasını Azure App Service'e dağıtmak için FTP kullanılır.

Ftp komutunda Web App'e hangi kimlik bilgilerinin geçirileceğini saptamak için, Cloud Shell'de [`az appservice web deployment list-publishing-profiles`](https://docs.microsoft.com/cli/azure/appservice/web/deployment#az_appservice_web_deployment_list_publishing_profiles) komutunu kullanın: 

```azurecli-interactive
az webapp deployment list-publishing-profiles --name <app_name> --resource-group myResourceGroup --query "[?publishMethod=='FTP'].{URL:publishUrl, Username:userName,Password:userPWD}" --output json
```

```JSON
[
  {
    "Password": "aBcDeFgHiJkLmNoPqRsTuVwXyZ",
    "URL": "ftp://waws-prod-blu-069.ftp.azurewebsites.windows.net/site/wwwroot",
    "Username": "app_name\\$app_name"
  }
]
```

### <a name="upload-the-app-using-ftp"></a>FTP kullanarak uygulamayı karşıya yükleme

Alışkın olduğunuz FTP aracını kullanarak, .WAR dosyasını önceki komutun `URL` alanından alınan sunucu adresindeki */site/wwwroot/webapps* klasöre dağıtın. Mevcut varsayılan (ROOT) uygulama dizinini kaldırın ve mevcut ROOT.war dosyasını öğreticinin önceki bölümlerinde oluşturulan .WAR dosyasıyla değiştirin.

```bash
ftp waws-prod-blu-069.ftp.azurewebsites.windows.net
Connected to waws-prod-blu-069.drip.azurewebsites.windows.net.
220 Microsoft FTP Service
Name (waws-prod-blu-069.ftp.azurewebsites.windows.net:raisa): app_name\$app_name
331 Password required
Password:
cd /site/wwwroot/webapps
mdelete -i ROOT/*
rmdir ROOT/
put target/TodoDemo-0.0.1-SNAPSHOT.war ROOT.war
```

### <a name="test-the-web-app"></a>Web uygulamasını test etme

`http://<app_name>.azurewebsites.net/` listesine göz atın ve listeye birkaç görev ekleyin. 

![Azure uygulama hizmetinde çalıştırılan Java uygulaması](./media/app-service-web-tutorial-java-mysql/appservice-web-app.png)

**Tebrikler!** Azure App Service’te veri temelli bir Java uygulaması çalıştırıyorsunuz.

## <a name="update-the-app-and-redeploy"></a>Uygulamayı güncelleştirme ve yeniden dağıtma

Uygulamayı güncelleştirerek, yapılacaklar listesine öğenin oluşturulduğu güne ilişkin bir sütun ekleyin. Spring Boot, veri modeli değiştikçe mevcut veritabanı kayıtlarınızda değişiklik yapmadan sizin için veritabanı şemasındaki güncelleştirmeyi işler.

1. Yerel sisteminizde, *src/main/java/com/example/fabrikam/TodoItem.java* konumunu açın ve aşağıdaki içeri aktarmaları sınıfa ekleyin:   

    ```java
    import java.text.SimpleDateFormat;
    import java.util.Calendar;
    ```

2. *src/main/java/com/example/fabrikam/TodoItem.java* dosyasına, nesne oluşturma sırasında onu bir zaman damgasıyla başlatacak bir `String` özelliği `timeCreated` ekleyin. Bu dosyayı düzenlerken yeni `timeCreated` özelliği için alıcılar/ayarlayıcılar ekleyin.

    ```java
    private String name;
    private boolean complete;
    private String timeCreated;
    ...

    public TodoItem(String category, String name) {
       this.category = category;
       this.name = name;
       this.complete = false;
       this.timeCreated = new SimpleDateFormat("MMMM dd, YYYY").format(Calendar.getInstance().getTime());
    }
    ...
    public void setTimeCreated(String timeCreated) {
       this.timeCreated = timeCreated;
    }

    public String getTimeCreated() {
        return timeCreated;
    }
    ```

3. *src/main/java/com/example/fabrikam/TodoDemoController.java* dosyasını `updateTodo` yöntemindeki bir satırla güncelleştirerek zaman damgasını ayarlayın:

    ```java
    item.setComplete(requestItem.isComplete());
    item.setId(requestItem.getId());
    item.setTimeCreated(requestItem.getTimeCreated());
    repository.save(item);
    ```

4. `Thymeleaf` şablonundaki yeni alan için destek ekleyin. *src/main/resources/templates/index.html* dosyasını zaman damgası için yeni tablo üst bilgisiyle ve her tablo veri satırında zaman damgasının değerini görüntülemek için yeni bir alanla güncelleştirin.

    ```html
    <th>Name</th>
    <th>Category</th>
    <th>Time Created</th>
    <th>Complete</th>
    ...
    <td th:text="${item.category}">item_category</td><input type="hidden" th:field="*{todoList[__${i.index}__].category}"/>
    <td th:text="${item.timeCreated}">item_time_created</td><input type="hidden" th:field="*{todoList[__${i.index}__].timeCreated}"/>
    <td><input type="checkbox" th:checked="${item.complete} == true" th:field="*{todoList[__${i.index}__].complete}"/></td>
    ```

5. Uygulamayı yeniden oluşturun:

    ```bash
    mvnw clean package 
    ```

6. Güncelleştirilmiş .WAR dosyasını daha önce olduğu gibi FTP yapın; bunun için mevcut *site/wwwroot/webapps/ROOT* dizinini ve *ROOT.war* dosyasını kaldırın, sonra da güncelleştirilmiş .WAR file dosyasını ROOT.war olarak karşıya yükleyin. 

Uygulamayı yenilediğinizde, **Oluşturulma Zamanı** sütunu artır görünür durumda olur. Yeni görev eklediğinizde, uygulama zaman damgasını otomatik olarak doldurur. Temelindeki veri modeli değiştirilmiş olsa da, mevcut görevleriniz olduğu gibi kalır ve uygulamayla çalışır. 

![Yeni sütunla güncelleştirilen Java uygulaması](./media/app-service-web-tutorial-java-mysql/appservice-updates-java.png)
      
## <a name="stream-diagnostic-logs"></a>Tanılama günlüklerini akışla aktarma 

Java uygulamanız Azure App Service'te çalışırken, doğrudan terminalinize yönlendirilen konsol günlüklerini alabilirsiniz. Böylece, uygulama hatalarını ayıklamanıza yardımcı olan tanılama iletilerinin aynısını alabilirsiniz.

Günlük akışını başlatmak için Cloud Shell’de [`az webapp log tail`](/cli/azure/webapp/log?view=azure-cli-latest#az_webapp_log_tail) komutunu kullanın.

```azurecli-interactive 
az webapp log tail --name <app_name> --resource-group myResourceGroup 
``` 

## <a name="manage-your-azure-web-app"></a>Azure web uygulamanızı yönetme

Oluşturduğunuz web uygulamasını görmek için [Azure portalına](https://portal.azure.com) gidin.

Sol menüden **App Service**’e ve ardından Azure web uygulamanızın adına tıklayın.

![Portaldan Azure web uygulamasına gitme](./media/app-service-web-tutorial-java-mysql/access-portal.png)

Varsayılan olarak, web uygulamanızın sayfasında **Genel Bakış** sayfası gösterilir. Bu sayfa, uygulamanızın nasıl çalıştığını gösterir. Buradan ayrıca durdurma, başlatma, yeniden başlatma ve silme gibi yönetim görevlerini gerçekleştirebilirsiniz. Sayfanın sol tarafındaki sekmeler, açabileceğiniz farklı yapılandırma sayfalarını gösterir.

![Azure portalında App Service sayfası](./media/app-service-web-tutorial-java-mysql/web-app-blade.png)

Sayfadaki bu sekmelerde web uygulamanıza ekleyebileceğiniz çok sayıda harika özellik gösterilir. Aşağıdaki listede yalnızca birkaç olasılık sunulmaktadır:
* Özel bir DNS adı eşleme
* Özel bir SSL sertifikası bağlama
* Sürekli dağıtımı yapılandırma
* Ölçeği artırma ve genişletme
* Kullanıcı kimlik doğrulaması ekleme

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu kaynaklara başka bir öğretici (bkz. [Sonraki adımlar](#next)) için gereksinim duymuyorsanız, Cloud Shell'de aşağıdaki komutu çalıştırarak bu kaynakları silebilirsiniz: 
  
```azurecli-interactive
az group delete --name myResourceGroup 
``` 

<a name="next"></a>

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="checklist"]
> * Azure’da MySQL veritabanı oluşturma
> * Örnek Java uygulamasını MySQL'e bağlama
> * Uygulamayı Azure’da dağıtma
> * Uygulamayı güncelleştirme ve yeniden dağıtma
> * Azure’daki tanılama günlüklerinin akışını sağlama
> * Uygulamayı Azure portalında yönetme

Uygulamaya özel bir DNS adı eşlemeyle ilgili bilgi edinmek için sonraki öğreticiye geçin.

> [!div class="nextstepaction"] 
> [Mevcut bir özel DNS adını Azure Web Apps ile eşleme](app-service-web-tutorial-custom-domain.md)
