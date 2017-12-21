---
title: "Azure'da Java ve MySQL bir web uygulaması oluşturma"
description: "Azure uygulama hizmeti çalışan Azure MySQL veritabanı hizmeti bağlandığı bir Java uygulaması alma hakkında bilgi."
services: app-service\web
documentationcenter: Java
author: bbenz
manager: jeffsand
editor: jasonwhowell
ms.assetid: 
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: java
ms.topic: tutorial
ms.date: 05/22/2017
ms.author: bbenz
ms.custom: mvc
ms.openlocfilehash: ad53575b655ebec5a134c8d76b963708caf14334
ms.sourcegitcommit: 3fca41d1c978d4b9165666bb2a9a1fe2a13aabb6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/15/2017
---
# <a name="build-a-java-and-mysql-web-app-in-azure"></a>Azure'da Java ve MySQL bir web uygulaması oluşturma

> [!NOTE]
> Bu makalede Windows App Service'e bir uygulama dağıtır. Uygulama hizmeti dağıtım _Linux_, bakın [kapsayıcılı yay önyükleme uygulama Azure'a dağıtma](/java/azure/spring-framework/deploy-containerized-spring-boot-java-app-with-maven-plugin).
>

Bu öğretici bir Java web uygulaması oluşturma ve MySQL veritabanına bağlanmak nasıl gösterir. İşiniz bittiğinde, olacaktır bir [yay önyükleme](https://projects.spring.io/spring-boot/) veri depolarken uygulama [Azure veritabanı için MySQL](https://docs.microsoft.com/azure/mysql/overview) üzerinde çalışan [Azure App Service Web Apps](app-service-web-overview.md).

![Azure uygulama hizmeti çalışan Java uygulaması](./media/app-service-web-tutorial-java-mysql/appservice-web-app.png)

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Azure üzerinde MySQL veritabanı oluşturma
> * Örnek bir uygulama veritabanına bağlan
> * Uygulamayı Azure'a dağıtma
> * Uygulamayı güncelleştirme ve yeniden dağıtma
> * Azure Stream tanılama günlükleri
> * Azure portalında uygulamayı izleme


## <a name="prerequisites"></a>Ön koşullar

1. [Git yükleyip](https://git-scm.com/)
1. [Java 7 JDK yükleyip veya üstü](http://www.oracle.com/technetwork/java/javase/downloads/index.html)
1. [İndirme, yükleme ve MySQL Başlat](https://dev.mysql.com/doc/refman/5.7/en/installing.html) 

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prepare-local-mysql"></a>Yerel MySQL hazırlama 

Bu adımda, yerel bir MySQL Server uygulamayı makinenizde yerel olarak test kullanmak için bir veritabanı oluşturun.

### <a name="connect-to-mysql-server"></a>MySQL sunucusuna bağlan

Bir terminal penceresi, yerel MySQL sunucusuna bağlanın. Bu öğreticide tüm komutları çalıştırmak için bu bir terminal penceresi kullanabilirsiniz.

```bash
mysql -u root -p
```

İçin bir parola istenirse parolayı girin `root` hesabı. Kök hesap parolanızı hatırlamıyorsanız bkz [MySQL: kök parolasını sıfırlamak nasıl](https://dev.mysql.com/doc/refman/5.7/en/resetting-permissions.html).

Komutunuzu başarıyla çalışırsa, MySQL sunucunuzu zaten çalışıyor. Aksi takdirde, yerel MySQL server'ınızdaki izleyerek başlatıldığından emin olun [MySQL yükleme sonrası adımları](https://dev.mysql.com/doc/refman/5.7/en/postinstallation.html).

### <a name="create-a-database"></a>Veritabanı oluşturma 

İçinde `mysql` isteminde, bir veritabanı ve yapılacak iş öğeleri için bir tablo oluşturun.

```sql
CREATE DATABASE tododb;
```

Sunucu bağlantısı yazarak çıkmak `quit`.

```sql
quit
```

## <a name="create-and-run-the-sample-app"></a>Oluşturun ve örnek uygulamayı çalıştırma 

Bu adımda, örnek yay önyükleme uygulama kopyalama, yerel MySQL veritabanını kullanacak şekilde yapılandırın ve bilgisayarınızda çalıştırın. 

### <a name="clone-the-sample"></a>Örnek kopyalama

Terminal penceresinde, bir çalışma dizinine gidin ve örnek depoyu kopyalayın. 

```bash
git clone https://github.com/azure-samples/mysql-spring-boot-todo
```

### <a name="configure-the-app-to-use-the-mysql-database"></a>MySQL veritabanını kullanmak üzere uygulamayı yapılandırır

Güncelleştirme `spring.datasource.password` ve değer *spring-boot-mysql-todo/src/main/resources/application.properties* MySQL istemi açmak için kullanılan aynı kök parolayla:

```
spring.datasource.password=mysqlpass
```

### <a name="build-and-run-the-sample"></a>Derleme ve örnek çalıştırma

Derleme ve bağlantıların bulunması dahil Maven sarmalayıcı kullanarak örneği çalıştırın:

```bash
cd spring-boot-mysql-todo
mvnw package spring-boot:run
```

Tarayıcınızı açın `http://localhost:8080` eylem örnekte görmek için. Aşağıdaki SQL komutlarını MySQL isteminde görevleri listeye eklemek gibi MySQL içinde depolanan verileri görüntülemek için kullanın.

```SQL
use testdb;
select * from todo_item;
```

Devreyi tarafından uygulamayı durdurun `Ctrl` + `C` Terminal. 

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="create-an-azure-mysql-database"></a>Bir Azure MySQL veritabanı oluşturma

Bu adımda, oluşturduğunuz bir [Azure veritabanı için MySQL](../mysql/quickstart-create-mysql-server-database-using-azure-cli.md) kullanarak örnek [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli). Örnek uygulama bu veritabanını daha sonra öğreticide kullanmak üzere yapılandırın.

### <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Oluşturma bir [kaynak grubu](../azure-resource-manager/resource-group-overview.md) ile [az grubu oluşturma](/cli/azure/group#create) komutu. Bir Azure kaynak grubu web uygulamaları, veritabanları ve depolama hesapları gibi ilgili kaynaklar burada dağıtılan ve yönetilen mantıksal bir kapsayıcısıdır. 

Aşağıdaki örnek Kuzey Avrupa bölgesinde bir kaynak grubu oluşturur:

```azurecli-interactive
az group create --name myResourceGroup --location "North Europe"
```    

Olası değerler görmek için kullanabileceğiniz `--location`, kullanın [az appservice listesi-konumları](/cli/azure/appservice#list-locations) komutu.

### <a name="create-a-mysql-server"></a>MySQL sunucusu oluşturun

Bulut Kabuğu'nda Azure veritabanındaki bir sunucu ile MySQL (Önizleme) oluşturmak [az mysql sunucusu oluşturun](/cli/azure/mysql/server#create) komutu.    
Burada gördüğünüz kendi benzersiz MySQL sunucu adı yerine `<mysql_server_name>` yer tutucu. Bu ad, MySQL sunucunuzun ana bilgisayar adı, bir parçasıdır `<mysql_server_name>.mysql.database.azure.com`, genel olarak benzersiz olması gerekir. Ayrıca yerine `<admin_user>` ve `<admin_password>` kendi değerlere sahip.

```azurecli-interactive
az mysql server create --name <mysql_server_name> --resource-group myResourceGroup --location "North Europe" --admin-user <admin_user> --admin-password <admin_password>
```

MySQL sunucusu oluşturulduğunda, Azure CLI bilgileri aşağıdaki örneğe benzer şekilde gösterir:

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

### <a name="configure-server-firewall"></a>Sunucu Güvenlik Duvarı'nı yapılandırma

Bulut Kabuğu'nda kullanarak istemci bağlantılarına izin verecek şekilde MySQL sunucunuz için bir güvenlik duvarı kuralı oluşturmak [az mysql server güvenlik duvarı kuralı oluşturmak](/cli/azure/mysql/server/firewall-rule#create) komutu. 

```azurecli-interactive
az mysql server firewall-rule create --name allIPs --server <mysql_server_name> --resource-group myResourceGroup --start-ip-address 0.0.0.0 --end-ip-address 255.255.255.255
```

> [!NOTE]
> Azure veritabanı için MySQL (Önizleme) Azure Hizmetleri bağlantılarından şu anda otomatik olarak etkinleştirmez. Dinamik olarak atanmış IP adresleri azure'da gibi tüm IP adresleri için şimdi etkinleştirmek daha iyidir. Hizmet önizlemesini devam ettikçe, veritabanınızın güvenliğini sağlamak için daha iyi yöntemleri etkinleştirilecek.

## <a name="configure-the-azure-mysql-database"></a>Azure MySQL veritabanını yapılandırın

Yerel terminal penceresinde Azure MySQL sunucusuna bağlanın. Daha önce için belirttiğiniz değerini kullanmak `<admin_user>` ve `<mysql_server_name>`.

```bash
mysql -u <admin_user>@<mysql_server_name> -h <mysql_server_name>.mysql.database.azure.com -P 3306 -p
```

### <a name="create-a-database"></a>Veritabanı oluşturma 

İçinde `mysql` isteminde, bir veritabanı ve yapılacak iş öğeleri için bir tablo oluşturun.

```sql
CREATE DATABASE tododb;
```

### <a name="create-a-user-with-permissions"></a>İzinleri olan bir kullanıcı oluşturun

Bir veritabanı kullanıcısı oluşturun ve tüm ayrıcalıkları vermek `tododb` veritabanı. Yer tutucuları değiştirmek `<Javaapp_user>` ve `<Javaapp_password>` kendi benzersiz uygulama adıyla.

```sql
CREATE USER '<Javaapp_user>' IDENTIFIED BY '<Javaapp_password>'; 
GRANT ALL PRIVILEGES ON tododb.* TO '<Javaapp_user>';
```

Sunucu bağlantısı yazarak çıkmak `quit`.

```sql
quit
```

## <a name="deploy-the-sample-to-azure-app-service"></a>Örnek Azure App Service'e dağıtma

İle bir Azure uygulama hizmeti planı oluştur **serbest** fiyatlandırma katmanı kullanarak [az uygulama hizmeti planı oluşturma](/cli/azure/appservice/plan#create) CLI komutu. Uygulama hizmeti planı, uygulamalarınızı barındırmak için kullanılan fiziksel kaynakları tanımlar. Bir uygulama hizmeti planı atanan tüm uygulamalar maliyet birden fazla uygulama barındırdığında kaydetmenize izin vererek, bu kaynakları paylaşır. 

```azurecli-interactive
az appservice plan create --name myAppServicePlan --resource-group myResourceGroup --sku FREE
```

Plan hazır olduğunda, Azure CLI aşağıdaki örneğe benzer çıkış şunları gösterir:

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

### <a name="create-an-azure-web-app"></a>Bir Azure Web uygulaması oluşturma

 Bulut Kabuğu'nda kullanmak [az webapp oluşturma](/cli/azure/appservice/web#create) bir web uygulaması tanımı oluşturmak için CLI komutu `myAppServicePlan` uygulama hizmeti planı. Web uygulaması tanımı uygulamanızla erişmek için bir URL sağlar ve kodunuzu Azure'a dağıtmak için çeşitli seçenekler yapılandırır. 

```azurecli-interactive
az webapp create --name <app_name> --resource-group myResourceGroup --plan myAppServicePlan
```

Yedek `<app_name>` kendi benzersiz uygulama adıyla yer tutucu. Adının Azure içinde tüm uygulamalar arasında benzersiz olması gerekir böylece bu benzersiz bir ad web uygulaması için varsayılan etki alanı adı bir parçasıdır. Kullanıcılarınız için kullanıma önce web uygulaması için bir özel etki alanı adı girişi eşleyebilirsiniz.

Web uygulaması tanımı hazır olduğunda, Azure CLI bilgileri aşağıdaki örneğe benzer şekilde gösterir: 

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

### <a name="configure-java"></a>Java yapılandırın 

İle uygulamanızın gerektiğini Java Çalışma zamanı yapılandırma bulut Kabuğu'nda ayarlama [az appservice web yapılandırma güncelleştirmesi](/cli/azure/appservice/web/config#update) komutu.

Aşağıdaki komut, yeni bir Java 8 JDK üzerinde çalıştırmak için web uygulaması yapılandırır ve [Apache Tomcat](http://tomcat.apache.org/) 8.0.

```azurecli-interactive
az webapp config set --name <app_name> --resource-group myResourceGroup --java-version 1.8 --java-container Tomcat --java-container-version 8.0
```

### <a name="configure-the-app-to-use-the-azure-sql-database"></a>Azure SQL veritabanını kullanmak üzere uygulamayı yapılandırır

Örnek uygulamayı çalıştırmadan önce Azure üzerinde oluşturulan Azure MySQL veritabanını kullanmak için web uygulaması uygulama ayarları ayarlayın. Bu özellikler web uygulaması ortam değişkenleri olarak sunulur ve paketlenmiş web app içinde application.properties kümesindeki değerlerini geçersiz kılar. 

Bulut Kabuğu'nda kullanarak uygulama ayarlarını [az webapp config appsettings](https://docs.microsoft.com/cli/azure/appservice/web/config/appsettings) CLI içinde:

```azurecli-interactive
az webapp config appsettings set --settings SPRING_DATASOURCE_URL="jdbc:mysql://<mysql_server_name>.mysql.database.azure.com:3306/tododb?verifyServerCertificate=true&useSSL=true&requireSSL=false" --resource-group myResourceGroup --name <app_name>
```

```azurecli-interactive
az webapp config appsettings set --settings SPRING_DATASOURCE_USERNAME=Javaapp_user@mysql_server_name --resource-group myResourceGroup --name <app_name>
```

```azurecli-interactive
az webapp config appsettings set --settings SPRING_DATASOURCE_PASSWORD=Javaapp_password --resource-group myResourceGroup --name <app_name>
```

### <a name="get-ftp-deployment-credentials"></a>FTP dağıtımı kimlik bilgileri alma 
FTP, yerel Git, GitHub, Visual Studio Team Services ve BitBucket gibi çeşitli şekillerde Azure uygulama hizmeti uygulamanızı dağıtabilirsiniz. Dağıtmak için bu örnek için FTP. WAR dosyası daha önce Azure App Service'e yerel makinenizde oluşturuldu.

Bir ftp komutu boyunca Web uygulaması'na iletmek için hangi kimlik bilgileri belirlemek için kullanın [az appservice web dağıtım listesi-yayımlama-profilleri](https://docs.microsoft.com/cli/azure/appservice/web/deployment#az_appservice_web_deployment_list_publishing_profiles) komutunu bulut Kabuğu'nda: 

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

### <a name="upload-the-app-using-ftp"></a>FTP kullanarak uygulamayı karşıya yükle

Sık kullandığınız FTP aracı dağıtmak için kullanın. WAR dosyası */site/wwwroot/webapps* alınan sunucu adresi klasöründe `URL` alanındaki önceki komutu. Var olan varsayılan (kök) uygulama dizini kaldırın ve mevcut ROOT.war ile değiştirin. Öğreticinin önceki yerleşik WAR dosyası.

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

### <a name="test-the-web-app"></a>Web uygulaması sınama

Gözat `http://<app_name>.azurewebsites.net/` ve birkaç görevi listesine ekleyin. 

![Azure uygulama hizmeti çalışan Java uygulaması](./media/app-service-web-tutorial-java-mysql/appservice-web-app.png)

**Tebrikler!** Azure App Service'te bir veri güdümlü Java uygulaması kullanıyorsunuz.

## <a name="update-the-app-and-redeploy"></a>Uygulamayı güncelleştirme ve yeniden dağıtma

Ek bir sütun Yapılacaklar listesinde öğesinin oluşturulduğu hangi gün için içerecek şekilde uygulamayı güncelleştirin. Veritabanı şeması sizin için veri modeli değişiklikleri olarak, var olan veritabanı kayıtlarını değiştirmeden güncelleştirme yay önyükleme işler.

1. Yerel sisteminizde açık *src/main/java/com/example/fabrikam/TodoItem.java* ve sınıfına aşağıdaki içeri aktarmaları ekleyin:   

    ```java
    import java.text.SimpleDateFormat;
    import java.util.Calendar;
    ```

2. Ekleme bir `String` özelliği `timeCreated` için *src/main/java/com/example/fabrikam/TodoItem.java*, nesne oluşturma sırasında bir zaman damgasına sahip başlatılıyor. Alıcıları/ayarlayıcılar için yeni Ekle `timeCreated` bu dosyayı düzenlerken özelliği.

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

3. Güncelleştirme *src/main/java/com/example/fabrikam/TodoDemoController.java* bir çizgi `updateTodo` yöntemi zaman damgası ayarlamak için:

    ```java
    item.setComplete(requestItem.isComplete());
    item.setId(requestItem.getId());
    item.setTimeCreated(requestItem.getTimeCreated());
    repository.save(item);
    ```

4. Yeni alan için destek Thymeleaf şablonunda ekleyin. Güncelleştirme *src/main/resources/templates/index.html* zaman damgası ve her tablo veri satırında zaman damgası değerini görüntülemek için yeni bir alan için yeni bir tablo üstbilgiyle.

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

5. Uygulama yeniden oluşturun:

    ```bash
    mvnw clean package 
    ```

6. Güncelleştirilmiş FTP. Varolan kaldırma WAR olarak önce *site/wwwroot/webapps/ROOT* dizin ve *ROOT.war*, sonra da güncelleştirilmiş karşıya yükleme. WAR dosyası olarak ROOT.war. 

Uygulama yenilediğinizde bir **saat oluşturulan** sütundur artık görünür. Yeni bir görev eklediğinizde, uygulama zaman damgası otomatik olarak doldurur. Temel alınan veri modeli değişti olsa bile varolan görevlerinizi değişmeden ve uygulama ile çalışmak kalır. 

![Java uygulaması olan yeni bir sütun güncelleştirildi](./media/app-service-web-tutorial-java-mysql/appservice-updates-java.png)
      
## <a name="stream-diagnostic-logs"></a>Akış tanılama günlükleri 

Java uygulamanızı Azure App Service'te çalışırken terminalinizde doğrudan yöneltilen konsol günlükleri alabilirsiniz. Böylece, uygulama hatalarını hata ayıklama yardımcı olmak için aynı tanılama iletileri alabilirsiniz.

Günlük akış başlatmak için kullanmak [az webapp günlük tail](/cli/azure/webapp/log?view=azure-cli-latest#az_webapp_log_tail) bulut Kabuğu'nda komutu.

```azurecli-interactive 
az webapp log tail --name <app_name> --resource-group myResourceGroup 
``` 

## <a name="manage-your-azure-web-app"></a>Azure web uygulamanızı yönetme

Oluşturduğunuz web uygulaması görmek için Azure portalına gidin.

Bunu yapmak için [https://portal.azure.com](https://portal.azure.com) sayfasında oturum açın.

Sol menüden **App Service**’e ve ardından Azure web uygulamanızın adına tıklayın.

![Portaldan Azure web uygulamasına gitme](./media/app-service-web-tutorial-java-mysql/access-portal.png)

Varsayılan olarak, web uygulamanızın dikey penceresinde **Genel Bakış** sayfası gösterilir. Bu sayfa, uygulamanızın nasıl çalıştığını gösterir. Burada, Durdur, Başlat, yeniden başlatma ve silme gibi yönetim görevlerini gerçekleştirebilirsiniz. Dikey pencerenin sol tarafındaki sekmeler, açabileceğiniz farklı yapılandırma sayfalarını gösterir.

![Azure portalında App Service dikey penceresi](./media/app-service-web-tutorial-java-mysql/web-app-blade.png)

Dikey penceredeki bu sekmelerde web uygulamanıza ekleyebileceğiniz çok sayıda harika özellik gösterilir. Aşağıdaki listede yalnızca birkaç olasılık sunulmaktadır:
* Özel bir DNS adı eşleme
* Özel bir SSL sertifikası bağlama
* Sürekli dağıtımı yapılandırma
* Ölçeği artırma ve genişletme
* Kullanıcı kimlik doğrulaması ekleme

## <a name="clean-up-resources"></a>Kaynakları temizleme

Başka bir öğretici için bu kaynakları gerekmiyorsa (bkz [sonraki adımlar](#next)), bulut Kabuğu'nda aşağıdaki komutu çalıştırarak silebilirsiniz: 
  
```azurecli-interactive
az group delete --name myResourceGroup 
``` 

<a name="next"></a>

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="checklist"]
> * Azure üzerinde MySQL veritabanı oluşturma
> * Örnek bir Java uygulaması Mysql'e bağlanmak
> * Uygulamayı Azure'a dağıtma
> * Uygulamayı güncelleştirme ve yeniden dağıtma
> * Azure Stream tanılama günlükleri
> * Azure portalında uygulama yönetme

Uygulamaya özel bir DNS adı eşleme öğrenmek için sonraki öğretici ilerleyin.

> [!div class="nextstepaction"] 
> [Mevcut bir özel DNS adını Azure Web Apps ile eşleme](app-service-web-tutorial-custom-domain.md)
