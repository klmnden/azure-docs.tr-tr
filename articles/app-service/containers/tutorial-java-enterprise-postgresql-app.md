---
title: Linux üzerinde - Azure App Service Kurumsal Java web uygulaması derleme | Microsoft Docs
description: Wildfly Linux üzerinde Azure App Service'te çalışan bir Java Enterprise uygulamasını nasıl edinebileceğinizi öğrenin.
author: JasonFreeberg
manager: routlaw
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: java
ms.topic: tutorial
ms.date: 11/13/2018
ms.author: jafreebe
ms.custom: seodec18
ms.openlocfilehash: 472ff85adaf72f91948c4072b12cca3ff8e59f37
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60769998"
---
# <a name="tutorial-build-a-java-ee-and-postgres-web-app-in-azure"></a>Öğretici: Azure'da bir Java EE ve Postgres web uygulaması oluşturma

Bu öğreticide Azure App Service'te bir Java Enterprise Edition (EE) web uygulaması oluşturma ve bir Postgres veritabanı'na bağlanma gösterilmektedir. İşlemi tamamladığınızda, olacaktır bir [WildFly](https://www.wildfly.org/about/) veri depolarken uygulama [Postgres için Azure veritabanı](https://azure.microsoft.com/services/postgresql/) Azure üzerinde çalışan [Linux üzerinde App Service'te](app-service-linux-intro.md).

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:
> [!div class="checklist"]
> * Maven kullanarak Azure'a bir Java EE uygulama dağıtma
> * Azure'da Postgres veritabanı oluşturma
> * WildFly sunucusunu Postgres kullanacak şekilde yapılandırma
> * Uygulamayı güncelleştirme ve yeniden dağıtma
> * WildFly üzerinde birim testleri çalıştırma

## <a name="prerequisites"></a>Önkoşullar

1. [Git'i indirin ve yükleyin](https://git-scm.com/)
2. [Maven 3'ü yükleyin ve indirin](https://maven.apache.org/install.html)
3. [Azure CLI'yı yükleme ve indirme](https://docs.microsoft.com/cli/azure/install-azure-cli)

## <a name="clone-and-edit-the-sample-app"></a>Kopyalamak ve örnek uygulamayı Düzenle

Bu adımda, örnek uygulamayı kopyalayın ve Maven proje nesne modeli (POM veya pom.xml) dağıtımı için yapılandırın.

### <a name="clone-the-sample"></a>Örneği kopyalama

Terminal penceresinde, bir çalışma dizini ve kopya Git [örnek depoyu](https://github.com/Azure-Samples/wildfly-petstore-quickstart).

```bash
git clone https://github.com/Azure-Samples/wildfly-petstore-quickstart.git
```

### <a name="update-the-maven-pom"></a>Maven POM güncelleştir

Maven Azure eklentisi, istenen adı ve kaynak grubu, App Service ile güncelleştirin. App Service planı veya örnek önceden oluşturmanız gerekmez. Maven plugin, zaten yoksa, App Service ve kaynak grubu oluşturur. 

Aşağı kaydırarak `<plugins>` bölümünü _pom.xml_, değişiklik yapmak için 200 satır. 

```xml
<!-- Azure App Service Maven plugin for deployment -->
<plugin>
  <groupId>com.microsoft.azure</groupId>
  <artifactId>azure-webapp-maven-plugin</artifactId>
  <version>${version.maven.azure.plugin}</version>
  <configuration>
    <appName>YOUR_APP_NAME</appName>
    <resourceGroup>YOUR_RESOURCE_GROUP</resourceGroup>
    <linuxRuntime>wildfly 14-jre8</linuxRuntime>
  ...
</plugin>  
```
Değiştirin `YOUR_APP_NAME` ve `YOUR_RESOURCE_GROUP` App Service ve kaynak grubunuzun adlarına sahip.

## <a name="build-and-deploy-the-application"></a>Uygulama derleme ve dağıtma

Şimdi Maven App Service'e dağıtma ve uygulamamızı oluşturmak için kullanacağız.

### <a name="build-the-war-file"></a>Derleme .war dosyası

Bu projede POM, bir Web Arşivi (WAR) dosyasına uygulamayı paketlemek için yapılandırılır. Maven kullanarak uygulama oluşturun:

```bash
mvn clean install -DskipTests
```

Bu uygulamanın test çalışmalarını WildFly uygulama dağıtıldığında çalıştırılmak üzere tasarlanmıştır. Biz yerel olarak derlemek için testleri atlar ve App Service üzerinde uygulama dağıtıldıktan sonra testleri çalıştırın.

### <a name="deploy-to-app-service"></a>App Service’e dağıtma

WAR hazır olduğuna göre Azure eklentisi için App Service'e dağıtım yapmak kullanabiliriz:

```bash
mvn azure-webapp:deploy
```

Dağıtım tamamlandığında, sonraki adıma devam edin.

### <a name="create-a-record"></a>Bir kayıt oluşturun

Bir tarayıcıyı açın ve `https://<your_app_name>.azurewebsites.net/` dizinine gidin. Tebrikler, Azure App Service'e bir Java EE uygulama dağıttığınız!

Bu noktada, uygulamayı bir bellek içi H2 veritabanını kullanıyor. Gezinti çubuğunda bulunan "Yönetici"'ye tıklayın ve yeni bir kategori oluşturun. App Service örneğinizin yeniden başlatırsanız, bellek içi veritabanı kaydı kaybolur. Aşağıdaki adımlarda, Azure üzerinde Postgres veritabanı sağlayarak bu sorunu gidermek ve kullanılacağını WildFly yapılandırın.

![Yönetici düğmesi konumu](media/tutorial-java-enterprise-postgresql-app/admin_button.JPG)

## <a name="provision-a-postgres-database"></a>Postgres veritabanı sağlama

Postgres veritabanı sunucusu sağlamak için bir terminal açın ve sunucu adı, kullanıcı adı, parola ve konum için istenen değerleri ile aşağıdaki komutu çalıştırın. App Service'inizin bulunduğu aynı kaynak grubunu kullanın. Daha sonra kullanmak üzere parolanızı not bırakın!

```bash
az postgres server create -n <desired-name> -g <same-resource-group> --sku-name GP_Gen4_2 -u <desired-username> -p <desired-password> -l <location>
```

Portal ve arama Postgres veritabanı göz atın. Yedekleme dikey penceresinde olduğunda, "Sunucu adı" ve "Sunucu Yöneticisi oturum açma adı" değerlerini kopyalayın, daha sonra gerekecektir.

### <a name="allow-access-to-azure-services"></a>Azure hizmetlerine erişime izin ver

İçinde **bağlantı güvenliği** paneli Azure veritabanı dikey pencerenin "Azure hizmetlerine erişime izin ver" düğmesine geçiş **ON** konumu.

![Azure hizmetlerine erişime izin verme](media/tutorial-java-enterprise-postgresql-app/postgress_button.JPG)

## <a name="update-your-java-app-for-postgres"></a>Postgres için Java uygulamanızı güncelleştirin

Şimdi bazı değişiklikler bizim Postgres veritabanı kullanmak üzere etkinleştirmek için Java uygulaması yapacağız.

### <a name="add-postgres-credentials-to-the-pom"></a>İçin POM Postgres kimlik bilgilerini ekleyin

İçinde _pom.xml_, büyük harflerle yazılan ifadeler yer tutucu değerlerini Postgres sunucu adı, yönetici oturum açma adı ve parola ile değiştirin. Bu alanlar Azure Maven Plugin içinde olur. (Değiştirdiğinizden emin olun `YOUR_SERVER_NAME`, `YOUR_PG_USERNAME`, ve `YOUR_PG_PASSWORD` içinde `<value>` ... etiketleri içinde değil `<name>` etiketleri!)

```xml
<plugin>
      ...
      <appSettings>
      <property>
        <name>POSTGRES_CONNECTIONURL</name>
        <value>jdbc:postgresql://YOUR_SERVER_NAME:5432/postgres?ssl=true</value>
      </property>
      <property>
        <name>POSTGRES_USERNAME</name>
        <value>YOUR_PG_USERNAME</value>
      </property>
      <property>
        <name>POSTGRES_PASSWORD</name>
        <value>YOUR_PG_PASSWORD</value>
      </property>
    </appSettings>
  </configuration>
</plugin>
```

### <a name="update-the-java-transaction-api"></a>Java API işlem güncelleştir

Ardından, böylece daha önce kullandığımız bellek içi H2 veritabanı yerine Java uygulamamız Postgres ile iletişim kurar, bizim Java işlem API (JPA) yapılandırmasını düzenlemek ihtiyacımız var. Bir düzenleyici açık _src/main/resources/META-INF/persistence.xml_. `<jta-data-source>` değerini `java:jboss/datasources/postgresDS` ile değiştirin. JTA XML dosyanızı, şimdi bu ayarı sahip olmalıdır:

```xml
...
<jta-data-source>java:jboss/datasources/postgresDS</jta-data-source>
...
```

## <a name="configure-the-wildfly-application-server"></a>WildFly uygulama sunucusunu yapılandırma

Yeniden yapılandırılan uygulamamız dağıtmadan önce biz WildFly uygulama sunucusu Postgres modülü ve bağımlılıkları ile güncelleştirmeniz gerekir. Daha fazla yapılandırma bilgileri şu adreste bulunabilir: [WildFly yapılandırma sunucusu](configure-language-java.md#configure-wildfly-server).

Sunucuyu yapılandırmak için şu dört dosyalarında gerekir `wildfly_config/` dizini:

- **postgresql-42.2.5.jar**: Bu JAR Dosyası Postgres için JDBC sürücüsü içindir. Daha fazla bilgi için [resmi Web sitesi](https://jdbc.postgresql.org/index.html).
- **postgres module.xml**: Bu XML dosyası (org.postgres) Postgres modül için bir ad bildirir. Ayrıca, kullanılacak modülü için gerekli olan bağımlılıklar ve kaynakları belirtir.
- **jboss_cli_commands.cl**: Bu dosya, JBoss CLI tarafından yürütülecek yapılandırma komutları içerir. Komutlar Postgres modülü WildFly uygulama sunucusuna ekleyin, kimlik bilgilerini sağlayın, JNDI adı bildirmek, vb. zaman aşımı eşiği. JBoss CLI ile alışkın değilseniz bkz [resmi belgelerine](https://access.redhat.com/documentation/red_hat_jboss_enterprise_application_platform/7.0/html-single/management_cli_guide/#how_to_cli).
- **startup_script.sh**: Son olarak, App Service örneği başlatıldığında bu kabuk betiği yürütülür. Komut dosyası, yalnızca tek bir işlevi gerçekleştirir: komutlar akışkan `jboss_cli_commands.cli` JBoss CLI için.

Bu dosyaların içeriğini özellikle okuma önerisi _jboss_cli_commands.cli_.

### <a name="ftp-the-configuration-files"></a>FTP yapılandırma dosyaları

FTP içeriği ihtiyacımız `wildfly_config/` bizim App Service örneğine. FTP kimlik bilgilerinizi almak için tıklayın **yayımlama profili Al** düğme Azure portalında App Service dikey penceresinde. FTP kullanıcı adı ve parola indirilen XML belgesinde olacaktır. Yayımlama profili hakkında daha fazla bilgi için bkz. [bu belgeyi](https://docs.microsoft.com/azure/app-service/deploy-configure-credentials).

Tercih ettiğiniz bir FTP aracını kullanarak, dört dosyaları aktarma `wildfly_config/` için `/home/site/deployments/tools/`. (, Dizin dosyaları kendilerinin yalnızca Aktarım değil unutmayın.)

### <a name="finalize-app-service"></a>App Service Sonlandır

App Service "Uygulama ayarları" panele dikey penceresine gidin. "Çalışma zamanı altında", "Başlangıç dosyası" alanın ayarlanacağı `/home/site/deployments/tools/startup_script.sh`. Bu, sonra App Service örneği oluşturulur, ancak önce WildFly sunucuyu başlatır Kabuk betiği çalıştırılır garanti eder.

Son olarak, App service'inizi yeniden başlatın. "Genel bakış" panelinde düğmesidir.

## <a name="redeploy-the-application"></a>Uygulamayı yeniden dağıtma

Bir terminal penceresinde, yeniden oluşturun ve uygulamanızı yeniden dağıtın.

```bash
mvn clean install -DskipTests azure-webapp:deploy
```

Tebrikler! Uygulamanız artık Postgres veritabanı kullanıyor ve uygulama içinde oluşturulan herhangi bir kayıt Postgres yerine önceki H3 bellek içi veritabanına depolanır. Bunu doğrulamak için bir kayıt olun ve App service'inizi yeniden başlatın. Uygulamanızı yeniden başlatırken kayıtları kalmaya devam eder.

## <a name="clean-up"></a>Temizleme

Bu kaynaklara başka bir öğretici için gereksinim duymuyorsanız (sonraki adımlara bakın), aşağıdaki komutu çalıştırarak silebilirsiniz:

```bash
az group delete --name <your-resource-group>
```

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Maven kullanarak Azure'a bir Java EE uygulama dağıtma
> * Azure'da Postgres veritabanı oluşturma
> * WildFly sunucusunu Postgres kullanacak şekilde yapılandırma
> * Uygulamayı güncelleştirme ve yeniden dağıtma
> * WildFly üzerinde birim testleri çalıştırma

Uygulamanıza özel bir DNS adı eşlemeyle ilgili bilgi edinmek için sonraki öğreticiye ilerleyin.

> [!div class="nextstepaction"]
> [Öğretici: Uygulamanıza özel DNS adı eşleme](../app-service-web-tutorial-custom-domain.md)

Ya da diğer kaynaklara göz atın:

> [!div class="nextstepaction"]
> [Java uygulamasını yapılandırma](configure-language-java.md)
