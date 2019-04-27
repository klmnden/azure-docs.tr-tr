---
title: Linux Java uygulamaları - Azure App Service'ı yapılandırma | Microsoft Docs
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
ms.date: 03/28/2019
ms.author: routlaw
ms.custom: seodec18
ms.openlocfilehash: b659c076974b0659c645c9b6460e458dfac8974a
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60850469"
---
# <a name="configure-a-linux-java-app-for-azure-app-service"></a>Azure App Service için Linux Java uygulaması yapılandırma

Linux üzerinde Azure App Service'te Java geliştiricilerinin kolayca oluşturmanızı, dağıtmanızı ve bunların Tomcat ölçeklendirme sağlar veya Linux tabanlı tam olarak yönetilen bir hizmet üzerinde web uygulamaları Java Standard Edition (SE) paketlenir. Komut satırından veya Intellij, Eclipse veya Visual Studio Code gibi düzenleyicilerde uygulamalarını Maven eklentileri ile dağıtın.

Bu kılavuzu temel kavramları ve App Service'te yerleşik bir Linux kapsayıcı kullanan Java geliştiricileri için yönergeler sağlar. Azure App Service daha önce kullanmadıysanız izleyin [Java Hızlı Başlangıç](quickstart-java.md) ve [Java PostgreSQL öğreticisiyle](tutorial-java-enterprise-postgresql-app.md) ilk.

## <a name="logging-and-debugging-apps"></a>Günlüğe kaydetme ve hata ayıklama uygulamaları

Performans raporları, trafiği görselleştirmeler ve sistem durumu denetlemeler Azure portalı üzerinden her uygulama için kullanılabilir. Daha fazla bilgi için [Azure App Service tanılama genel bakış](../overview-diagnostics.md).

### <a name="ssh-console-access"></a>SSH konsol erişimi

[!INCLUDE [Open SSH session in browser](../../../includes/app-service-web-ssh-connect-builtin-no-h.md)]

### <a name="stream-diagnostic-logs"></a>Tanılama günlüklerini akışla aktarma

[!INCLUDE [Access diagnostic logs](../../../includes/app-service-web-logs-access-no-h.md)]

Daha fazla bilgi için [akış günlükleri Azure CLI ile](../troubleshoot-diagnostic-logs.md#streaming-with-azure-cli).

### <a name="app-logging"></a>Uygulama günlüğü

Etkinleştirme [uygulama günlüğü](../troubleshoot-diagnostic-logs.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json#enablediag) Azure portalından veya [Azure CLI](/cli/azure/webapp/log#az-webapp-log-config) App Service'nın yerel, uygulamanızın standart konsol çıkışı ve standart konsol hatası akış yazmak için yapılandırmak için dosya sistemi veya Azure Blob Depolama. Yerel App Service dosya sisteminde oturum örnek 12 saat yapılandırıldıktan sonra devre dışı bırakıldı. Daha uzun bekletme süresi gerekiyorsa, bir Blob Depolama kapsayıcısına çıkışını yazmak için uygulamayı yapılandırma.

Uygulamanız kullanıyorsa [Logback](https://logback.qos.ch/) veya [Log4j](https://logging.apache.org/log4j) izleme için günlüğekaydetmeçerçevesiyapılandırmayönergelerikullanarakgözdengeçirmeiçinAzureApplicationInsightsiçindebuizlemeleriniletebilir[Keşfedin Java izleme günlükleri Application Insights'ta](/azure/application-insights/app-insights-java-trace-logs).

## <a name="customization-and-tuning"></a>Özelleştirme ve ayarlama

Linux için Azure App Service kutusu ayarlama ve Azure portalı ve CLI aracılığıyla özelleştirmeyi destekler. Java'ya özgü web uygulama yapılandırması için aşağıdaki makaleleri inceleyin:

- [App Service ayarlarını yapılandırma](../web-sites-configure.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json)
- [Özel etki alanını ayarlama](../app-service-web-tutorial-custom-domain.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json)
- [SSL'yi etkinleştirme](../app-service-web-tutorial-custom-ssl.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json)
- [CDN ekleme](../../cdn/cdn-add-to-web-app.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json)

### <a name="set-java-runtime-options"></a>Java Çalışma zamanı seçenekleri

Tomcat ve Java SE ortamlarında ayrılan bellek veya diğer JVM çalışma zamanı seçenekleri ayarlamak için aşağıda gösterildiği gibi JAVA_OPTS Ayarla bir [uygulama ayarı](../web-sites-configure.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json#app-settings). Başlatıldığında app Service Linux Java Çalışma zamanı için bu ayarı bir ortam değişkeni geçirir.

Azure portalında altında **uygulama ayarları** adlı yeni bir uygulama ayarı için web app oluşturmak `JAVA_OPTS` gibi ek ayarlar içeren `$JAVA_OPTS -Xms512m -Xmx1204m`.

Azure App Service Linux Maven plugin uygulama ayarlarını yapılandırmak için Azure eklentisi bölümünde ayarı/değer etiketler ekleyin. Aşağıdaki örnek, belirli bir en düşük ve en çok Java yığın boyutu ayarlar:

```xml
<appSettings>
    <property>
        <name>JAVA_OPTS</name>
        <value>$JAVA_OPTS -Xms512m -Xmx1204m</value>
    </property>
</appSettings>
```

Tek bir uygulama, App Service planında bir dağıtım yuvası ile çalışan geliştiriciler, aşağıdaki seçenekleri kullanabilirsiniz:

- B1 ve S1 örnekleri:-Xms1024m-Xmx1024m
- B2 ve S2 örnekleri:-Xms3072m-Xmx3072m
- B3 ve S3 örnekleri:-Xms6144m-Xmx6144m


Ne zaman ayar uygulama yığın ayarları, App Service planı bilgilerinizi gözden geçirin ve birden çok uygulama ve dağıtım yuvası dikkate en iyi bellek ayırma bulması gerekir.

### <a name="turn-on-web-sockets"></a>Web yuvaları üzerinde Aç

Azure portalında web yuvaları için desteği etkinleştirmek **uygulama ayarları** uygulama için. Ayarın etkili olması uygulamayı yeniden başlatmanız gerekir.

Web yuvası desteği Azure CLI kullanarak aşağıdaki komutla açın:

```azurecli-interactive
az webapp config set --name <app-name> --resource-group <resource-group-name> --web-sockets-enabled true
```

Ardından, uygulamanızı yeniden başlatın:

```azurecli-interactive
az webapp stop --name <app-name> --resource-group <resource-group-name>
az webapp start --name <app-name> --resource-group <resource-group-name>
```

### <a name="set-default-character-encoding"></a>Varsayılan karakter kodlamasını ayarlayın

Azure portalında altında **uygulama ayarları** adlı yeni bir uygulama ayarı için web app oluşturmak `JAVA_OPTS` değerle `$JAVA_OPTS -Dfile.encoding=UTF-8`.

Alternatif olarak, App Service Maven plugin kullanarak uygulama ayarı yapılandırabilirsiniz. Ayar adı ve değeri etiketleri eklentisi yapılandırmasına ekleyin:

```xml
<appSettings>
    <property>
        <name>JAVA_OPTS</name>
        <value>$JAVA_OPTS -Dfile.encoding=UTF-8</value>
    </property>
</appSettings>
```

## <a name="secure-applications"></a>Uygulamaları güvenli hale getirin

Linux için App Service'te çalışan Java uygulamalarını kümesinin aynısına sahip [en iyi güvenlik uygulamaları](/azure/security/security-paas-applications-using-app-services) diğer uygulamalar olarak. 

### <a name="authenticate-users"></a>Kullanıcıların kimliklerini doğrulama

Azure portalında uygulama kimlik doğrulamasını ayarlama **kimlik doğrulama ve yetkilendirme** seçeneği. Burada, Azure Active Directory veya Facebook, Google veya GitHub gibi sosyal oturum açma bilgilerini kullanarak kimlik doğrulamasını etkinleştirebilirsiniz. Azure portal yapılandırması yalnızca tek bir kimlik doğrulama sağlayıcısı yapılandırırken çalışır. Daha fazla bilgi için [App Service uygulamanızı Azure Active Directory oturum açma bilgilerini kullanacak şekilde yapılandırma](../configure-authentication-provider-aad.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json) ve diğer kimlik sağlayıcıları için ilgili makaleler.

Birden çok oturum açma sağlayıcısı etkinleştirmeniz gerekirse, yönergeleri izleyin [App Service kimlik doğrulaması özelleştirme](../app-service-authentication-how-to.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json) makalesi.

 Spring önyükleme geliştiriciler [Azure Active Directory Spring Boot Başlatıcı](/java/azure/spring-framework/configure-spring-boot-starter-java-app-with-azure-active-directory?view=azure-java-stable) tanıdık Spring güvenlik açıklamalarını ve API'leri kullanarak uygulamaların güvenliğini sağlamak için.

### <a name="configure-tlsssl"></a>TLS/SSL'yi yapılandırma

Bölümündeki yönergeleri [var olan özel bir SSL sertifikası bağlama](../app-service-web-tutorial-custom-ssl.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json) mevcut bir SSL sertifikasını karşıya yüklemek ve uygulamanızın etki alanı adı için bağlama için. Varsayılan olarak, uygulamanızın HTTP bağlantıları-özel SSL ve TLS zorlamak için öğreticinin adımlarını yine de sağlar.

## <a name="configure-apm-platforms"></a>APM platformları yapılandırma

Bu bölüm, Java uygulamaları (APM) platformları NewRelic ve AppDynamics uygulama performansı izleme ile Linux üzerinde Azure App Service'te dağıtılan bağlama işlemi gösterilmektedir.

[Yeni Relic yapılandırma](#configure-new-relic)
[AppDynamics yapılandırın](#configure-appdynamics)

### <a name="configure-new-relic"></a>Yeni Relic yapılandırın

1. NewRelic hesabında oluşturma [NewRelic.com](https://newrelic.com/signup)
2. Olan NewRelic Java agent yükleme, benzer bir dosya adı olacaktır `newrelic-java-x.x.x.zip`.
3. Lisans anahtarınızı kopyalayın, buna daha sonra aracıyı yapılandırmak için ihtiyacınız olacak.
4. [App Service örneğinizin içine SSH](app-service-linux-ssh-support.md) ve yeni bir dizin oluşturmak `/home/site/wwwroot/apm`.
5. Paketten çıkarılan NewRelic Java aracı dosyalarını bir dizin altında dosya yükleme `/home/site/wwwroot/apm`. Aracınızı dosyaları olmalıdır `/home/site/wwwroot/apm/newrelic`.
6. YAML dosyasının değiştirme `/home/site/wwwroot/apm/newrelic/newrelic.yml` ve yer tutucu lisans değerini kendi lisans anahtarı ile değiştirin.
7. Azure portalında App Service uygulamanızda gidin ve yeni bir uygulama ayarı oluştur.
    - Uygulamanızı kullanıyorsa **Java SE**, adlı bir ortam değişkenini oluşturmak `JAVA_OPTS` değerle `-javaagent:/home/site/wwwroot/apm/newrelic/newrelic.jar`.
    - Kullanıyorsanız **Tomcat**, adlı bir ortam değişkenini oluşturmak `CATALINA_OPTS` değerle `-javaagent:/home/site/wwwroot/apm/newrelic/newrelic.jar`.
    - Kullanıyorsanız **WildFly**, New Relic belgelerine bakın [burada](https://docs.newrelic.com/docs/agents/java-agent/additional-installation/wildfly-version-11-installation-java) JBoss yapılandırma ve Java agent yükleme hakkında yönergeler için.
    - Bir ortam değişkeni için zaten varsa `JAVA_OPTS` veya `CATALINA_OPTS`, ekleme `javaagent` seçeneği sonuna kadar geçerli değeri.

### <a name="configure-appdynamics"></a>AppDynamics yapılandırın

1. AppDynamics hesabınız oluşturma [AppDynamics.com](https://www.appdynamics.com/community/register/)
1. AppDynamics Web sitesinden Java agent yükleme, dosya adı şuna benzer olacaktır. `AppServerAgent-x.x.x.xxxxx.zip`
1. [App Service örneğinizin içine SSH](app-service-linux-ssh-support.md) ve yeni bir dizin oluşturmak `/home/site/wwwroot/apm`.
1. Java aracı dosyalarını bir dizin altında dosya yükleme `/home/site/wwwroot/apm`. Aracınızı dosyaları olmalıdır `/home/site/wwwroot/apm/appdynamics`.
1. Azure portalında App Service uygulamanızda gidin ve yeni bir uygulama ayarı oluştur.
    - Kullanıyorsanız **Java SE**, adlı bir ortam değişkenini oluşturmak `JAVA_OPTS` değerle `-javaagent:/home/site/wwwroot/apm/appdynamics/javaagent.jar -Dappdynamics.agent.applicationName=<app-name>` burada `<app-name>` App Service adınız.
    - Kullanıyorsanız **Tomcat**, adlı bir ortam değişkenini oluşturmak `CATALINA_OPTS` değerle `-javaagent:/home/site/wwwroot/apm/appdynamics/javaagent.jar -Dappdynamics.agent.applicationName=<app-name>` burada `<app-name>` App Service adınız.
    - Kullanıyorsanız **WildFly**, AppDynamics belgelerine bakın [burada](https://docs.appdynamics.com/display/PRO45/JBoss+and+Wildfly+Startup+Settings) JBoss yapılandırma ve Java agent yükleme hakkında yönergeler için.

## <a name="configure-tomcat"></a>Tomcat yapılandırma

### <a name="connect-to-data-sources"></a>Veri kaynaklarına bağlanma

>[!NOTE]
> Uygulamanız Spring Framework veya Spring Boot kullanıyorsa, veritabanı bağlantısı bilgilerini Spring veri JPA [uygulama özellikleri dosyanızda] ortam değişkenleri olarak ayarlayabilirsiniz. Ardından [uygulama ayarları](../web-sites-configure.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json#app-settings) bu değerler uygulamanız için Azure portal veya CLI tanımlamak için.

Bu yönergeler, tüm veritabanı bağlantıları için geçerlidir. Yer tutucuları seçilen veritabanınızın sürücü sınıf adını doldurun ve JAR dosyası gerekir. Sağlanan bir sınıf adları ve genel veritabanları için sürücü indirmeleri tablodur.

| Database   | Sürücü sınıfı adı                             | JDBC Sürücüsü                                                                      |
|------------|-----------------------------------------------|------------------------------------------------------------------------------------------|
| PostgreSQL | `org.postgresql.Driver`                        | [İndir](https://jdbc.postgresql.org/download.html)                                    |
| MySQL      | `com.mysql.jdbc.Driver`                        | [İndirme](https://dev.mysql.com/downloads/connector/j/) (Seç "Platform bağımsız") |
| SQL Server | `com.microsoft.sqlserver.jdbc.SQLServerDriver` | [İndir](https://docs.microsoft.com/sql/connect/jdbc/download-microsoft-jdbc-driver-for-sql-server?view=sql-server-2017#available-downloads-of-jdbc-driver-for-sql-server)                                                           |

Tomcat, Java veritabanı bağlantısı (JDBC) veya Java Kalıcılık API (JPA) kullanacak şekilde yapılandırmak için öncelikle özelleştirmek `CATALINA_OPTS` tarafından Tomcat başlatma sırasında okunan ortam değişkeni. Bu değerleri bir uygulama ayarı aracılığıyla ayarlamak [App Service Maven plugin](https://github.com/Microsoft/azure-maven-plugins/blob/develop/azure-webapp-maven-plugin/README.md):

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

#### <a name="shared-server-level-resources"></a>Paylaşılan sunucu düzeyinde kaynaklar

1. İçeriğini kopyalayın `/usr/local/tomcat/conf` içine `/home/tomcat/conf` , App Service Linux üzerinde örnek, bir yapılandırma var. henüz yoksa SSH kullanarak.
    ```
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

#### <a name="finalize-configuration"></a>Yapılandırmayı son haline getir

Son olarak, sürücü jar dosyaları dışındaki Tomcat sınıf yerleştirin ve App service'inizi yeniden başlatın.

1. Bunları yerleştirerek JDBC sürücüsü dosyaları için Tomcat classloader kullanılabilir olduğundan emin olun `/home/tomcat/lib` dizin. (Zaten yoksa, bu dizin oluşturun.) Bu dosyalar, App Service örneğine karşıya yüklemek için aşağıdaki adımları gerçekleştirin:
    1. İçinde [Cloud Shell](https://shell.azure.com), webapp uzantıyı yükleyin:

      ```azurecli-interactive
      az extension add -–name webapp
      ```

    2. App Service için yerel sisteminizden SSH tüneli oluşturma için aşağıdaki CLI komutunu çalıştırın:

      ```azurecli-interactive
      az webapp remote-connection create --resource-group <resource-group-name> --name <app-name> --port <port-on-local-machine>
      ```

    3. SFTP istemcinizi ile yerel tünel bağlantı noktasına bağlanmak ve dosyaları karşıya yükleme `/home/tomcat/lib` klasör.

    Alternatif olarak, JDBC sürücüsünü yüklenecek bir FTP istemcisi kullanabilirsiniz. Aşağıdaki adımları [FTP kimlik bilgilerinizi almak için yönergeler](../deploy-configure-credentials.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json).

2. Sunucu düzeyinde veri kaynağı oluşturduysanız, App Service Linux uygulamayı yeniden başlatın. Tomcat sıfırlanır `CATALINA_HOME` için `/home/tomcat/conf` ve güncelleştirilmiş yapılandırmayı kullanın.

## <a name="configure-wildfly-server"></a>WildFly sunucusunu yapılandırma

[App Service ile ölçek](#scale-with-app-service)
[özelleştirme uygulama sunucusu yapılandırması](#customize-application-server-configuration)
[modüller ve bağımlılıkları](#modules-and-dependencies)
[veri Kaynakları](#data-sources)
[Mesajlaşma sağlayıcıları etkinleştir](#enable-messaging-providers)
[yönetim oturumu önbelleğe almayı yapılandır](#configure-session-management-caching)

### <a name="scale-with-app-service"></a>App Service ile ölçeklendirme

Linux üzerinde App Service'te çalışan WildFly uygulama sunucusu bir etki alanı yapılandırmasında tek başına modunda çalışır. App Service planı ölçeğini daralttığınızda her WildFly örnek bir tek başına sunucu olarak yapılandırılır.

 Uygulamanız ile yatay veya dikey olarak ölçeklendirme [ölçeklendirme kuralları](../../monitoring-and-diagnostics/monitoring-autoscale-get-started.md) ve [, örnek sayısını artırıyorsanız](../web-sites-scale.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json).

### <a name="customize-application-server-configuration"></a>Uygulama sunucusu yapılandırması özelleştirme

Web uygulaması örneği durum bilgisi olduğundan başlatılan yeni örneklerin başlangıçta uygulama tarafından gerek duyulan Wildfly yapılandırmayı desteklemek için yapılandırılması gerekir.
Bir başlangıç WildFly CLI için çağrılacak Bash betiği yazabilirsiniz:

- Veri kaynakları ayarlayın
- Mesajlaşma sağlayıcılarını yapılandırma
- Diğer modüller ve bağımlılıkları Wildfly sunucu yapılandırmasına ekleyin.

 Betik Wildfly hazır ve çalışır durumda olduğunda, ancak uygulama başlatılmadan önce çalışır. Betik kullanması gereken [JBOSS CLI](https://docs.jboss.org/author/display/WFLY/Command+Line+Interface) çağrılır `/opt/jboss/wildfly/bin/jboss-cli.sh` herhangi bir yapılandırma veya sunucu başladıktan sonra gerekli değişiklikleri uygulama sunucusu yapılandırmak için.

CLI'ın etkileşimli mod Wildfly yapılandırmak için kullanmayın. İçin JBoss CLI kullanarak bir komut dosyası komut bunun yerine, sağlayabilir `--file` komutu, örneğin:

```bash
/opt/jboss/wildfly/bin/jboss-cli.sh -c --file=/path/to/your/jboss_commands.cli
```

Başlangıç betiği için karşıya yükleme `/home/site/deployments/tools` App Service Örneğinizdeki. Bkz: [bu belgeyi](../deploy-configure-credentials.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json#userscope) FTP kimlik bilgilerinizi alma hakkında yönergeler için.

Ayarlama **başlangıç betiği** Azure portalında, başlangıç Kabuk betiği konumunu Örneğin alan `/home/site/deployments/tools/your-startup-script.sh`.

Tedarik [uygulama ayarları](../web-sites-configure.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json#app-settings) kullanmak için ortam değişkenlerini betiğe geçirmek için uygulama yapılandırması. Uygulama ayarları, bağlantı dizeleri ve sürüm denetimi dışında Uygulamanızı yapılandırmak için gerekli diğer gizli dizileri koruyun.

### <a name="modules-and-dependencies"></a>Modüller ve bağımlılıkları

JBoss CLI aracılığıyla Wildfly sınıf içine modülleri ve bağımlılıklarını yüklemek için kendi dizininde aşağıdaki dosyalar oluşturmanız gerekir. Çoğu durumda bir bağımlılık yapılandırmak gerekenler, minimum düzeyde bu liste, bu nedenle bazı modüller ve bağımlılıkları JNDI adlandırma gibi ek yapılandırma veya başka bir API özel yapılandırma gerekebilir.

- Bir [XML modülü tanımlayıcısı](https://jboss-modules.github.io/jboss-modules/manual/#descriptors). Bu XML dosya adı, öznitelikler ve bağımlılıkları modülünüzün tanımlar. Bu [örnek module.xml dosyası](https://access.redhat.com/documentation/en-us/jboss_enterprise_application_platform/6/html/administration_and_configuration_guide/example_postgresql_xa_datasource) Postgres modülü, JAR dosyasını JDBC bağımlılığı ve gerekli diğer modül bağımlılıklarının tanımlar.
- Gerekli JAR dosyası için tüm bağımlılıkların modülünüzde.
- Yeni modül yapılandırmak için JBoss CLI komutları ile bir komut dosyası. Bu dosya sunucusunu bağımlılık kullanacak şekilde yapılandırmak için JBoss CLI tarafından yürütülecek komutlarınızı içerir. Modüller, veri kaynakları ve mesajlaşma sağlayıcıları eklemek için komutlara ilişkin belgeleri için başvurmak [bu belgeyi](https://access.redhat.com/documentation/red_hat_jboss_enterprise_application_platform/7.0/html-single/management_cli_guide/#how_to_cli).
- JBoss CLI'yı arayın ve önceki adımda komut yürütmek için bir Bash başlangıç betiği. Bu dosya, App Service örneğinizin yeniden başlatıldığında veya ölçek genişletme sırasında yeni örnekleri sağlandığında yürütülür. Bu başlangıç betiği JBoss komutları JBoss CLI için geçirilen diğer tüm yapılandırmaları, uygulamanız için yapabileceğiniz gösterilmiştir. En azından, bu dosya için JBoss CLI JBoss CLI komut geçirilecek tek bir komut olabilir:

```bash
`/opt/jboss/wildfly/bin/jboss-cli.sh -c --file=/path/to/your/jboss_commands.cli`
```

Modülünüzün için içeriği ve dosyaları aldıktan sonra modülün Wildfly uygulama sunucusuna eklemek için aşağıdaki adımları izleyin.

1. Dosyalarınıza FTP `/home/site/deployments/tools` App Service Örneğinizdeki. Yönergeler için bu belgede, FTP kimlik bilgilerinizi Forms'dan bakın.
2. Örneğin, başlangıç Kabuk betiği konumuna Azure portal'ın uygulama ayarları dikey penceresinde "Başlangıç betiği" alanını ayarlayın `/home/site/deployments/tools/your-startup-script.sh` .
3. App Service örneğinizin tuşlarına basarak yeniden **yeniden** düğmesine **genel bakış** bölümü portalı veya Azure CLI kullanarak.

### <a name="data-sources"></a>Veri kaynakları

Wildfly için veri kaynağı bağlantısı yapılandırmak için yukarıdaki modülleri yükleme ve bağımlılıkları bölümünde ana hatlarıyla aynı süreci izleyin. Herhangi bir Azure veritabanı hizmeti için aynı adımları izleyebilirsiniz.

1. Veritabanı flavor için JDBC sürücüsünü yükleyin. Kolaylık olması için sürücüleri şunlardır [Postgres](https://jdbc.postgresql.org/download.html) ve [MySQL](https://dev.mysql.com/downloads/connector/j/). .Jar dosyasını almak için indirme ayıklayın.
2. "Modülleri ve oluşturup XML modülü tanımlayıcısı, JBoss CLI betiği, başlangıç betiği ve JDBC .jar bağımlılık karşıya bağımlılıklar" adımları anahattı izleyin.

Wildfly ile yapılandırma hakkında daha fazla bilgi [PostgreSQL](https://developer.jboss.org/blogs/amartin-blog/2012/02/08/how-to-set-up-a-postgresql-jdbc-driver-on-jboss-7) , [MySQL](https://docs.jboss.org/jbossas/docs/Installation_And_Getting_Started_Guide/5/html/Using_other_Databases.html#Using_other_Databases-Using_MySQL_as_the_Default_DataSource), ve [SQL veritabanı](https://docs.jboss.org/jbossas/docs/Installation_And_Getting_Started_Guide/5/html/Using_other_Databases.html#d0e3898) kullanılabilir. Veri kaynağı tanımlarının sunucunuza eklemek için bu özelleştirilmiş yönergeler genelleştirilmiş bir yaklaşım ile birlikte kullanabilirsiniz.

### <a name="enable-messaging-providers"></a>Mesajlaşma sağlayıcıları etkinleştir

Service Bus Mesajlaşma mekanizması olarak kullanan temelli ileti Fasulye etkinleştirmek için:

1. Kullanım [Apache QPId JMS Mesajlaşma kitaplığına](https://qpid.apache.org/proton/index.html). Bu bağımlılık pom.xml (veya diğer derleme dosyası), uygulamayı içerir.

2. Oluşturma [hizmet veri yolu kaynaklarını](/azure/service-bus-messaging/service-bus-java-how-to-use-jms-api-amqp). Gönderme ile bir Azure Service Bus ad alanı ve sıra içinde bu ad alanı ve bir paylaşılan erişim ilkesi oluşturun ve özelliklerini alır.

3. Paylaşılan Erişim İlkesi anahtarı kodunuza URL kodlaması ile geçirin ilkenizin birincil anahtar veya [hizmet veri yolu SDK'yı](/azure/service-bus-messaging/service-bus-java-how-to-use-jms-api-amqp#setup-jndi-context-and-configure-the-connectionfactory).

4. JMS sağlayıcı modülü XML tanımlayıcısı, .jar bağımlılıkları, JBoss CLI komutları ve başlangıç betiği ile yükleme modüller ve bağımlılıkları bölümünde ana hatlarıyla belirtilen adımları izleyin. Dört dosyalara ek olarak JMS kuyruk ve konu JNDI adını tanımlayan bir XML dosyası oluşturmanız gerekir. Bkz: [bu depo](https://github.com/JasonFreeberg/widlfly-server-configs/tree/master/appconfig) başvuru yapılandırma dosyaları için.

### <a name="configure-session-management-caching"></a>Yönetim oturumu önbelleğe almayı yapılandır

Varsayılan olarak, istemci istekleri oturumlarına ile yönlendirildiğinden emin olmak için oturum benzeşimi tanımlama Linux üzerinde App Service'te kullanır, uygulamanızın'ın aynı örneğine. Bu varsayılan davranışı, işlem yapılandırma gerektirmez, ancak bazı sınırlamaları vardır:

- Bir uygulama örneği yeniden başlatıldı veya ölçeklendirilebilir, uygulama sunucusu kullanıcı oturumu durumunda kaybolur.
- Uygulamalarınız için uzun oturum zaman aşımı ayarları veya sabit birkaç kullanıcıya varsa, yalnızca yeni oturumlar yeni başlatılan örneklerine yönlendirilir olduğundan yük almak için yeni örnekleri autoscaled biraz zaman alabilir.

Bir dış oturum deposu gibi kullanılacak Wildfly yapılandırabileceğiniz [Azure önbelleği için Redis](/azure/azure-cache-for-redis/). Şunları yapmanız gerekir [mevcut ARR örnek benzeşimini devre dışı bırakma](https://azure.microsoft.com/blog/disabling-arrs-instance-affinity-in-windows-azure-web-sites/) oturum tanımlama bilgisi tabanlı yönlendirmesini devre dışı ve yapılandırılmış Wildfly oturumu deposu girişim çalışmasına izin vermek için yapılandırma.

## <a name="docker-containers"></a>Docker kapsayıcıları

Azure tarafından desteklenen Zulu JDK kapsayıcılarınızı içinde kullanmak için çekme ve gelen belgelendiği gibi önceden oluşturulmuş görüntüleri kullanmayı emin olun [Azul Zulu Enterprise Azure indirme sayfası için desteklenen](https://www.azul.com/downloads/azure-only/zulu/) veya `Dockerfile` örneklerden[Microsoft Java GitHub deposu](https://github.com/Microsoft/java/tree/master/docker).

## <a name="statement-of-support"></a>Destek bildirimi

### <a name="runtime-availability"></a>Çalışma zamanı kullanılabilirlik

Linux için App Service, yönetilen Java web uygulamalarını barındırmak için iki çalışma zamanlarını destekler:

- [Tomcat servlet kapsayıcı](https://tomcat.apache.org/) paketlenmiş uygulamaları çalıştırmak için farklı web arşivi (WAR) dosyaları. Desteklenen sürümler şunlardır: 8,5 ve 9.0 sürümlerine ait.
- Uygulamaları çalıştırmak için Java SE çalışma zamanı ortamı (JAR) dosyaları Java arşiv paketlendi. Java 8 ve 11 desteklenen sürümleridir.

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

Java geliştirme belirli olmayan Linux için App Service'ı kullanma hakkında genel soruları [App Service Linux SSS](app-service-linux-faq.md).