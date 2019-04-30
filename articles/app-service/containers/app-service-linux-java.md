---
title: Linux'ta - Azure App Service için Java Geliştirici Kılavuzu | Microsoft Docs
description: Linux üzerinde Azure App Service'te çalışan Java uygulamalarını yapılandırmayı öğrenin.
keywords: Azure app service, web uygulaması, linux, oss, java
services: app-service
author: rloutlaw
manager: angerobe
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: java
ms.topic: article
ms.date: 12/10/2018
ms.author: routlaw
ms.custom: seodec18
ms.openlocfilehash: 5c9f70650f518c72a75d9a7826e7cbc30a95a00c
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60852718"
---
# <a name="java-developers-guide-for-app-service-on-linux"></a>Linux'ta App Service için Java Geliştirici Kılavuzu

Linux üzerinde Azure App Service'te Java geliştiricilerinin kolayca oluşturmanızı, dağıtmanızı ve bunların Tomcat ölçeklendirme sağlar veya Linux tabanlı tam olarak yönetilen bir hizmet üzerinde web uygulamaları Java Standard Edition (SE) paketlenir. Komut satırından veya Intellij, Eclipse veya Visual Studio Code gibi düzenleyicilerde uygulamalarını Maven eklentileri ile dağıtın.

Bu kılavuzu temel kavramları ve Linux için App Service kullanarak Java geliştiricileri için yönergeler sağlar. Azure App Service Linux için kullanmadıysanız, okumanız gereken [Java Hızlı Başlangıç](quickstart-java.md) ilk. Java geliştirme belirli olmayan Linux için App Service'ı kullanma hakkında genel soruları [App Service Linux SSS](app-service-linux-faq.md).

## <a name="deploying-your-app"></a>Uygulamanızı dağıtma

Kullanabileceğiniz [Azure App Service için Maven Plugin](/java/api/overview/azure/maven/azure-webapp-maven-plugin/readme) .jar hem .war dosyaları dağıtmak için. Popüler ıde'lerle dağıtım ile desteklenen ayrıca [Intellij için Azure Araç Seti](/java/azure/intellij/azure-toolkit-for-intellij) veya [Eclipse için Azure Araç Seti](/java/azure/eclipse/azure-toolkit-for-eclipse).

Aksi takdirde, dağıtım yöntemini, arşiv türüne bağlıdır:

- Tomcat için .war dosyaları dağıtmak için kullanın `/api/wardeploy/` Arşiv dosyasının göndermek için uç nokta. Bu API hakkında daha fazla bilgi için lütfen bkz [bu belgeleri](https://docs.microsoft.com/azure/app-service/deploy-zip#deploy-war-file).
- Java SE görüntülerindeki .jar dosyalarını dağıtmak için `/api/zipdeploy/` Kudu sitesi uç noktası. Bu API hakkında daha fazla bilgi için lütfen bkz [bu belgeleri](https://docs.microsoft.com/azure/app-service/deploy-zip#rest).

.War veya FTP kullanarak bir .jar dağıtmayın. FTP aracı başlatma komut dosyaları, bağımlılıklar veya diğer çalışma zamanı dosyalarını karşıya yüklemek için tasarlanmıştır. Web uygulamaları dağıtmak için en uygun bir seçenek değil.

## <a name="logging-and-debugging-apps"></a>Günlüğe kaydetme ve hata ayıklama uygulamaları

Performans raporları, trafiği görselleştirmeler ve sistem durumu denetlemeler Azure portalı üzerinden her uygulama için kullanılabilir. Bkz: [Azure App Service tanılama genel bakış](/azure/app-service/overview-diagnostics) erişmek ve bu tanılama araçları kullanma hakkında daha fazla bilgi için.

## <a name="application-performance-monitoring"></a>Uygulama performansı izleme

Bkz: [uygulama performansı izleme ile Linux üzerinde Azure App Service'te Java uygulamaları Araçları](how-to-java-apm-monitoring.md) New Relic ve Appdynamics'in ile Linux üzerinde App Service'te çalışan Java uygulamalarını yapılandırmak nasıl yapılır yönergeleri için.

### <a name="ssh-console-access"></a>SSH konsol erişimi

SSH bağlantısı, uygulamanızı çalıştıran Linux ortamı için kullanılabilir. Bkz: [Linux'ta Azure App Service için SSH desteği](/azure/app-service/containers/app-service-linux-ssh-support) web tarayıcınızı veya yerel terminalde aracılığıyla Linux sistemine bağlanmak tam yönergeler için.

### <a name="streaming-logs"></a>Akış günlükleri

Hızlı hata ayıklama ve sorun giderme için Azure CLI kullanarak konsolunuza günlüklerin akışını. CLI ile yapılandırma `az webapp log config` günlük kaydını etkinleştirmek için:

```azurecli-interactive
az webapp log config --name ${WEBAPP_NAME} \
 --resource-group ${RESOURCEGROUP_NAME} \
 --web-server-logging filesystem
```

Konsolu kullanarak günlükleri akışını `az webapp log tail`:

```azurecli-interactive
az webapp log tail --name webappname --resource-group myResourceGroup
```

Daha fazla bilgi için [akış günlükleri Azure CLI ile](../troubleshoot-diagnostic-logs.md#streaming-with-azure-cli).

### <a name="app-logging"></a>Uygulama günlüğü

Etkinleştirme [uygulama günlüğü](/azure/app-service/troubleshoot-diagnostic-logs#enablediag) Azure portalından veya [Azure CLI](/cli/azure/webapp/log#az-webapp-log-config) App Service'nın yerel, uygulamanızın standart konsol çıkışı ve standart konsol hatası akış yazmak için yapılandırmak için dosya sistemi veya Azure Blob Depolama. Yerel App Service dosya sisteminde oturum örnek 12 saat yapılandırıldıktan sonra devre dışı bırakıldı. Daha uzun bekletme süresi gerekiyorsa, bir Blob Depolama kapsayıcısına çıkışını yazmak için uygulamayı yapılandırma. Java ve Tomcat uygulama günlüklerinizi bulunabilir `/home/LogFiles/Application/` dizin.

Uygulamanız kullanıyorsa [Logback](https://logback.qos.ch/) veya [Log4j](https://logging.apache.org/log4j) izleme için günlüğekaydetmeçerçevesiyapılandırmayönergelerikullanarakgözdengeçirmeiçinAzureApplicationInsightsiçindebuizlemeleriniletebilir[Keşfedin Java izleme günlükleri Application Insights'ta](/azure/application-insights/app-insights-java-trace-logs).

### <a name="troubleshooting-tools"></a>Sorun giderme araçları

Yerleşik Java görüntüleri temel alan [Alpine Linux](https://alpine-linux.readthedocs.io/en/latest/getting_started.html) işletim sistemi. Kullanım `apk` sorun giderme yüklemek için Paket Yöneticisi Araçları veya komutları.

## <a name="customization-and-tuning"></a>Özelleştirme ve ayarlama

Linux için Azure App Service kutusu ayarlama ve Azure portalı ve CLI aracılığıyla özelleştirmeyi destekler. Belirli olmayan bir Java web uygulama yapılandırması için aşağıdaki makaleleri inceleyin:

- [App Service ayarlarını yapılandırma](/azure/app-service/web-sites-configure?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json)
- [Özel etki alanını ayarlama](/azure/app-service/app-service-web-tutorial-custom-domain?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json)
- [SSL'yi etkinleştirme](/azure/app-service/app-service-web-tutorial-custom-ssl?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json)
- [CDN ekleme](/azure/cdn/cdn-add-to-web-app?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json)
- [Kudu sitesi yapılandırma](https://github.com/projectkudu/kudu/wiki/Configurable-settings#linux-on-app-service-settings)

### <a name="set-java-runtime-options"></a>Java Çalışma zamanı seçenekleri

Tomcat ve Java SE ortamlarında ayrılan bellek veya diğer JVM çalışma zamanı seçenekleri ayarlamak için oluşturun bir [uygulama ayarı](/azure/app-service/web-sites-configure#app-settings) adlı `JAVA_OPTS` seçenekleri. Başlatıldığında app Service Linux Java Çalışma zamanı için bu ayarı bir ortam değişkeni geçirir.

Azure portalında altında **uygulama ayarları** adlı yeni bir uygulama ayarı için web app oluşturmak `JAVA_OPTS` gibi ek ayarlar içeren `-Xms512m -Xmx1204m`.

Maven plugin uygulama ayarlarını yapılandırmak için Azure eklentisi bölümünde ayarı/değer etiketler ekleyin. Aşağıdaki örnek, belirli bir en düşük ve en çok Java yığın boyutu ayarlar:

```xml
<appSettings>
    <property>
        <name>JAVA_OPTS</name>
        <value>-Xms512m -Xmx1204m</value>
    </property>
</appSettings>
```

Tek bir uygulama, App Service planında bir dağıtım yuvası ile çalışan geliştiriciler, aşağıdaki seçenekleri kullanabilirsiniz:

- B1 ve S1 örnekleri: `-Xms1024m -Xmx1024m`
- B2 ve S2 örnekleri: `-Xms3072m -Xmx3072m`
- B3 ve S3 örnekleri: `-Xms6144m -Xmx6144m`

Ne zaman ayar uygulama yığın ayarları, App Service planı bilgilerinizi gözden geçirin ve birden çok uygulama ve dağıtım yuvası dikkate en iyi bellek ayırma bulması gerekir.

Bir JAR uygulama dağıtıyorsanız, adında `app.jar` yerleşik görüntü uygulamanızın doğru şekilde belirleyebilirsiniz. (Maven plugin, bu otomatik olarak yeniden adlandırma işlemi yapar.) Yeniden adlandırmak için JAR istemiyorsanız `app.jar`, jar dosyasını çalıştırmak için komutu ile bir kabuk betiği karşıya yükleyebilirsiniz. Bu komut dosyasında tam yolu yapıştırın [başlangıç dosyası](https://docs.microsoft.com/en-us/azure/app-service/containers/app-service-linux-faq#startup-file) metin kutusunda portalının yapılandırma bölümü.

### <a name="turn-on-web-sockets"></a>Web yuvaları üzerinde Aç

Azure portalında web yuvaları için desteği etkinleştirmek **uygulama ayarları** uygulama için. Ayarın etkili olması uygulamayı yeniden başlatmanız gerekir.

Web yuvası desteği Azure CLI kullanarak aşağıdaki komutla açın:

```azurecli-interactive
az webapp config set -n ${WEBAPP_NAME} -g ${WEBAPP_RESOURCEGROUP_NAME} --web-sockets-enabled true
```

Ardından, uygulamanızı yeniden başlatın:

```azurecli-interactive
az webapp stop -n ${WEBAPP_NAME} -g ${WEBAPP_RESOURCEGROUP_NAME}
az webapp start -n ${WEBAPP_NAME} -g ${WEBAPP_RESOURCEGROUP_NAME}  
```

### <a name="set-default-character-encoding"></a>Varsayılan karakter kodlamasını ayarlayın

Azure portalında altında **uygulama ayarları** adlı yeni bir uygulama ayarı için web app oluşturmak `JAVA_OPTS` değerle `-Dfile.encoding=UTF-8`.

Alternatif olarak, App Service Maven plugin kullanarak uygulama ayarı yapılandırabilirsiniz. Ayar adı ve değeri etiketleri eklentisi yapılandırmasına ekleyin:

```xml
<appSettings>
    <property>
        <name>JAVA_OPTS</name>
        <value>-Dfile.encoding=UTF-8</value>
    </property>
</appSettings>
```

### <a name="adjust-startup-timeout"></a>Başlangıç zaman aşımını ayarlama

Java uygulamanızı özellikle büyük ise, başlama süresi sınırını artırmanız gerekir. Bunu yapmak için bir uygulama ayarı oluşturmak `WEBSITES_CONTAINER_START_TIME_LIMIT` ve App Service, zaman aşımına uğramadan önce beklemesi gereken saniye sayısını ayarlayın. En yüksek değer `1800` saniye.

## <a name="secure-applications"></a>Uygulamaları güvenli hale getirin

Linux için App Service'te çalışan Java uygulamalarını kümesinin aynısına sahip [en iyi güvenlik uygulamaları](/azure/security/security-paas-applications-using-app-services) diğer uygulamalar olarak.

### <a name="authenticate-users"></a>Kullanıcıların kimliklerini doğrulama

Azure portalında uygulama kimlik doğrulamasını ayarlama **kimlik doğrulama ve yetkilendirme** seçeneği. Burada, Azure Active Directory veya Facebook, Google veya GitHub gibi sosyal oturum açma bilgilerini kullanarak kimlik doğrulamasını etkinleştirebilirsiniz. Azure portal yapılandırması yalnızca tek bir kimlik doğrulama sağlayıcısı yapılandırırken çalışır.  Daha fazla bilgi için [App Service uygulamanızı Azure Active Directory oturum açma bilgilerini kullanacak şekilde yapılandırma](/azure/app-service/configure-authentication-provider-aad) ve diğer kimlik sağlayıcıları için ilgili makaleler.

Birden çok oturum açma sağlayıcısı etkinleştirmeniz gerekirse, yönergeleri izleyin [App Service kimlik doğrulaması özelleştirme](https://docs.microsoft.com/azure/app-service/app-service-authentication-how-to) makalesi.

Spring önyükleme geliştiriciler [Azure Active Directory Spring Boot Başlatıcı](/java/azure/spring-framework/configure-spring-boot-starter-java-app-with-azure-active-directory?view=azure-java-stable) tanıdık Spring güvenlik açıklamalarını ve API'leri kullanarak uygulamaların güvenliğini sağlamak için. Maksimum boyut olarak artırıldığından emin olun, `application.properties` dosya. Değerini öneririz `16384`.

### <a name="configure-tlsssl"></a>TLS/SSL'yi yapılandırma

Bölümündeki yönergeleri [var olan özel bir SSL sertifikası bağlama](/azure/app-service/app-service-web-tutorial-custom-ssl) mevcut bir SSL sertifikasını karşıya yüklemek ve uygulamanızın etki alanı adı için bağlama için. Varsayılan olarak, uygulamanızın HTTP bağlantıları-özel SSL ve TLS zorlamak için öğreticinin adımlarını yine de sağlar.

### <a name="use-keyvault-references"></a>KeyVault başvuruları kullanın

[Azure anahtar kasası](../../key-vault/key-vault-overview.md) erişim ilkeleri ve denetim geçmişi ile gizli merkezi yönetim sağlar. Gizli anahtarları (parolaları veya bağlantı dizeleri için gibi) Keyvault'ta depolayabilir ve ortam değişkenlerini, uygulamanızda bu gizliliklerin erişin.

İlk olarak, yönergelerini izleyin [Key Vault'a erişim verme](../app-service-key-vault-references.md#granting-your-app-access-to-key-vault) ve [bir uygulama ayarı gizli bir anahtar kasası başvurusu yapmak](../app-service-key-vault-references.md#reference-syntax). App Service terminal uzaktan erişirken yazdırma ortam değişkeni tarafından başvuru gizli çözdüğünü doğrulayabilirsiniz.

Bu gizli dizileri Spring veya Tomcat yapılandırma dosyanızdaki eklemesine ortam değişkeni ekleme söz dizimini kullanın (`${MY_ENV_VAR}`). Spring yapılandırma dosyaları için lütfen bu edinmek [yapılandırmaları te dış](https://docs.spring.io/spring-boot/docs/current/reference/html/boot-features-external-config.html).

## <a name="data-sources"></a>Veri kaynakları

### <a name="tomcat"></a>Tomcat

Bu yönergeler, tüm veritabanı bağlantıları için geçerlidir. Yer tutucuları seçilen veritabanınızın sürücü sınıf adını doldurun ve JAR dosyası gerekir. Sağlanan bir sınıf adları ve genel veritabanları için sürücü indirmeleri tablodur.

| Database   | Sürücü sınıfı adı                             | JDBC Sürücüsü                                                                      |
|------------|-----------------------------------------------|------------------------------------------------------------------------------------------|
| PostgreSQL | `org.postgresql.Driver`                        | [İndir](https://jdbc.postgresql.org/download.html)                                    |
| MySQL      | `com.mysql.jdbc.Driver`                        | [İndirme](https://dev.mysql.com/downloads/connector/j/) (Seç "Platform bağımsız") |
| SQL Server | `com.microsoft.sqlserver.jdbc.SQLServerDriver` | [İndir](https://docs.microsoft.com/sql/connect/jdbc/download-microsoft-jdbc-driver-for-sql-server?view=sql-server-2017#available-downloads-of-jdbc-driver-for-sql-server)                                                           |

Tomcat, Java veritabanı bağlantısı (JDBC) veya Java Kalıcılık API (JPA) kullanacak şekilde yapılandırmak için öncelikle özelleştirmek `CATALINA_OPTS` Tomcat başlangıçta tarafından okunan yukarı ortam değişkeni. Bu değerleri bir uygulama ayarı aracılığıyla ayarlamak [App Service Maven plugin](https://github.com/Microsoft/azure-maven-plugins/blob/develop/azure-webapp-maven-plugin/README.md):

```xml
<appSettings>  
    <property>  
        <name>CATALINA_OPTS</name>  
        <value>"$CATALINA_OPTS -Ddbuser=${DBUSER} -Ddbpassword=${DBPASSWORD} -DconnURL=${CONNURL}"</value>  
    </property>  
</appSettings>  
```

Veya "Uygulama ayarlar" dikey penceresinde Azure portalında ortam değişkenlerini ayarlayın.

Ardından, veri kaynağı bir uygulama veya Tomcat servlet üzerinde çalışan tüm uygulamalar için kullanılabilir olup olmayacağını belirler.

#### <a name="application-level-data-sources"></a>Uygulama düzeyi veri kaynakları

1. Oluşturma bir `context.xml` dosyası `META-INF/` projenizin dizin. Oluşturma `META-INF/` henüz yoksa dizin.

2. İçinde `context.xml`, ekleme bir `Context` JNDI adresine veri kaynağına bağlamak için öğesi. Değiştirin `driverClassName` Yukarıdaki tablodaki sürücünüzün sınıf adı ile yer tutucu.

    ```xml
    <Context>
        <Resource
            name="jdbc/dbconnection"
            type="javax.sql.DataSource"
            url="${dbuser}"
            driverClassName="<insert your driver class name>"
            username="${dbpassword}"
            password="${connURL}"
        />
    </Context>
    ```

3. Uygulamanızın güncelleştirme `web.xml` uygulamanızdaki veri kaynağını kullanmak için.

    ```xml
    <resource-env-ref>
        <resource-env-ref-name>jdbc/dbconnection</resource-env-ref-name>
        <resource-env-ref-type>javax.sql.DataSource</resource-env-ref-type>
    </resource-env-ref>
    ```

#### <a name="shared-server-level-resources"></a>Paylaşılan, sunucu düzeyinde kaynakları

1. İçeriğini kopyalayın `/usr/local/tomcat/conf` içine `/home/tomcat/conf` , App Service Linux üzerinde örnek, bir yapılandırma var. henüz yoksa SSH kullanarak.

    ```bash
    mkdir -p /home/tomcat
    cp -a /usr/local/tomcat/conf /home/tomcat/conf
    ```

2. Bir bağlam öğesine ekleyin, `server.xml` içinde `<Server>` öğesi.

    ```xml
    <Server>
    ...
    <Context>
        <Resource
            name="jdbc/dbconnection"
            type="javax.sql.DataSource"
            url="${dbuser}"
            driverClassName="<insert your driver class name>"
            username="${dbpassword}"
            password="${connURL}"
        />
    </Context>
    ...
    </Server>
    ```

3. Uygulamanızın güncelleştirme `web.xml` uygulamanızdaki veri kaynağını kullanmak için.

    ```xml
    <resource-env-ref>
        <resource-env-ref-name>jdbc/dbconnection</resource-env-ref-name>
        <resource-env-ref-type>javax.sql.DataSource</resource-env-ref-type>
    </resource-env-ref>
    ```

#### <a name="finally-place-the-driver-jars-in-the-tomcat-classpath-and-restart-your-app-service"></a>Son olarak, sürücü jar dosyaları dışındaki Tomcat sınıf getirin ve App service'inizi yeniden başlatın

1. Bunları yerleştirerek JDBC sürücüsü dosyaları için Tomcat classloader kullanılabilir olduğundan emin olun `/home/tomcat/lib` dizin. (Zaten yoksa, bu dizin oluşturun.) Bu dosyalar, App Service örneğine karşıya yüklemek için aşağıdaki adımları gerçekleştirin:  
   1. Azure App Service webpp uzantıyı yükleyin:

      ```azurecli-interactive
      az extension add –name webapp
      ```

   1. App Service için yerel sisteminizden SSH tüneli oluşturmaya aşağıdaki CLI komutunu çalıştırın:

      ```azurecli-interactive
      az webapp remote-connection create –g [resource group] -n [app name] -p [local port to open]
      ```

   1. SFTP istemcinizi ile yerel tünel bağlantı noktasına bağlanmak ve dosyaları karşıya yükleme `/home/tomcat/lib` klasör.

      Alternatif olarak, JDBC sürücüsünü yüklenecek bir FTP istemcisi kullanabilirsiniz. Aşağıdaki adımları [FTP kimlik bilgilerinizi almak için yönergeler](https://docs.microsoft.com/azure/app-service/deploy-configure-credentials).

2. Sunucu düzeyinde veri kaynağı oluşturduysanız, App Service Linux uygulamayı yeniden başlatın. Tomcat sıfırlanır `CATALINA_HOME` için `/home/tomcat/conf` ve güncelleştirilmiş yapılandırmayı kullanın.

### <a name="spring-boot"></a>Spring Boot

Spring Boot uygulamalarda veri kaynaklarına bağlanmak için bağlantı dizeleri oluşturmak ve bunları ekleme öneririz, `application.properties` dosya.

1. App Service dikey penceresinde "Uygulama ayarları" bölümünde, dize için bir ad ayarlayın, JDBC bağlantı dizesi değer alanına yapıştırın ve türü "Özel" olarak ayarlayın. İsteğe bağlı olarak, bu bağlantı dizesini yuva ayarı olarak ayarlayabilirsiniz.

    ![Portalda bir bağlantı dizesi oluşturma.][1]

    Bu bağlantı dizesini uygulamamız adlı bir ortam değişkeni olarak erişilebilir `CUSTOMCONNSTR_<your-string-name>`. Örneğin, yukarıda oluşturduğumuz bağlantı dizesini adlandırılacağını `CUSTOMCONNSTR_exampledb`.

2. İçinde `application.properties` dosya, bu bağlantı dizesi ortam değişkeni adı ile başvuru. Bizim örneğimizde, biz kullanırsınız.

    ```yml
    app.datasource.url=${CUSTOMCONNSTR_exampledb}
    ```

Lütfen [veri erişimi üzerinde Spring Boot belgeleri](https://docs.spring.io/spring-boot/docs/current/reference/html/howto-data-access.html
) ve [yapılandırmaları te dış](https://docs.spring.io/spring-boot/docs/current/reference/html/boot-features-external-config.html) Bu konu hakkında daha fazla bilgi için.

## <a name="docker-containers"></a>Docker kapsayıcıları

Azure tarafından desteklenen Zulu JDK kapsayıcılarınızı içinde kullanmak için çekme ve gelen belgelendiği gibi önceden oluşturulmuş görüntüleri kullanmayı emin olun [Azul Zulu Enterprise Azure indirme sayfası için desteklenen](https://www.azul.com/downloads/azure-only/zulu/) veya `Dockerfile` örneklerden[Microsoft Java GitHub deposu](https://github.com/Microsoft/java/tree/master/docker).

## <a name="runtime-availability-and-statement-of-support"></a>Çalışma zamanı kullanılabilirlik ve Destek bildirimi

Linux için App Service, yönetilen Java web uygulamalarını barındırmak için iki çalışma zamanlarını destekler:

- [Tomcat servlet kapsayıcı](https://tomcat.apache.org/) paketlenmiş uygulamaları çalıştırmak için farklı web arşivi (WAR) dosyaları. Desteklenen sürümler şunlardır: 8,5 ve 9.0 sürümlerine ait.
- Uygulamaları çalıştırmak için Java SE çalışma zamanı ortamı (JAR) dosyaları Java arşiv paketlendi. Yalnızca desteklenen ana sürüm Java 8 ' dir.

## <a name="java-runtime-statement-of-support"></a>Java Çalışma zamanı destek bildirimi

### <a name="jdk-versions-and-maintenance"></a>JDK sürümleri ve Bakım

Azure'nın desteklenen Java Development Kit (JDK) olan [Zulu](https://www.azul.com/downloads/azure-only/zulu/) aracılığıyla sağlanan [Azul Systems](https://www.azul.com/).

Ana sürüm güncelleştirmeleri Linux için Azure App service'taki yeni çalışma zamanı seçenekleri aracılığıyla sağlanır. Müşteriler, App Service dağıtımı yapılandırarak Java daha yeni sürümleri için güncelleştirme ve test etmeden sorumlu ve ihtiyaçlarını karşılayan önemli güncelleştirme sağlama.

Desteklenen JDK otomatik olarak üç aylık olarak Ocak, Nisan, Temmuz ve Ekim her yıl düzeltme eki.

### <a name="security-updates"></a>Güvenlik güncelleştirmeleri

Azul sistemlerden kullanılabilir olduklarında hemen sonra yamaları ve düzeltmeler önemli güvenlik açıkları için kullanıma sunulacaktır. "Büyük" bir güvenlik açığı 9.0 temel puanı ile tanımlanan ya da üzerinde daha büyük [NIST ortak güvenlik açığı Puanlama sistemi, sürüm 2](https://nvd.nist.gov/cvss.cfm).

### <a name="deprecation-and-retirement"></a>Kullanımdan kaldırma ve devre dışı bırakma

Desteklenen Java Çalışma zamanı kullanımdan kaldırılacak, çalışma zamanı kullanımdan önce en az altı ay etkilenen çalışma zamanı kullanan Azure geliştiricileri, kullanımdan kaldırma bildirimi verilir.

### <a name="local-development"></a>Yerel geliştirme

Geliştiriciler, üretim sürümü, Azul Zulu Enterprise JDK yerel geliştirme için indirebileceği [Azul'ın indirme sitesinde](https://www.azul.com/downloads/azure-only/zulu/).

### <a name="development-support"></a>Geliştirme desteği

Ürün desteği [Azure tarafından desteklenen Azul Zulu JDK](https://www.azul.com/downloads/azure-only/zulu/) Azure için geliştirmeye aracılığıyla kullanılabilir veya [Azure Stack](https://azure.microsoft.com/overview/azure-stack/) ile bir [Azure destek planı koşullu](https://azure.microsoft.com/support/plans/).

### <a name="runtime-support"></a>Çalışma zamanı desteği

Geliştiriciler şunları yapabilir [bir sorun açın](/azure/azure-supportability/how-to-create-azure-support-request) oluşturulduysa Azure desteği aracılığıyla Azul Zulu JDK ile bir [tam destek planı](https://azure.microsoft.com/support/plans/).

## <a name="next-steps"></a>Sonraki adımlar

Ziyaret [Java geliştiricileri için Azure](/java/azure/) Azure hızlı başlangıç kılavuzlarımız, öğreticilerimiz ve Java başvuru belgeleri bulmak için merkezi.

<!--Image references-->
[1]: ./media/app-service-linux-java/connection-string.png
