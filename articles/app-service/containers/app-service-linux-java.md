---
title: Linux üzerinde Azure App Service için Java dili desteği | Microsoft Docs
description: Linux üzerinde Azure App Service ile Java uygulaması dağıtma için Geliştirici Kılavuzu.
keywords: Azure app service, web uygulaması, linux, oss, java
services: app-service
author: rloutlaw
manager: angerobe
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: java
ms.topic: article
ms.date: 08/29/2018
ms.author: routlaw
ms.openlocfilehash: 2b2256ef5802160dbaa66e2a098a798fcdc653d2
ms.sourcegitcommit: cc4fdd6f0f12b44c244abc7f6bc4b181a2d05302
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/25/2018
ms.locfileid: "47065120"
---
# <a name="java-developers-guide-for-app-service-on-linux"></a>Linux'ta App Service için Java Geliştirici Kılavuzu

Linux üzerinde Azure App Service'te Java geliştiricilerinin kolayca oluşturmanızı, dağıtmanızı ve bunların Tomcat ölçeklendirme sağlar veya Linux tabanlı tam olarak yönetilen bir hizmet üzerinde web uygulamaları Java Standard Edition (SE) paketlenir. Komut satırından veya Intellij, Eclipse veya Visual Studio Code gibi düzenleyicilerde uygulamalarını Maven eklentileri ile dağıtın.

Bu kılavuzu temel kavramları ve Linux için App Service kullanarak Java geliştiricileri için yönergeler sağlar. Azure App Service Linux için kullanmadıysanız, okumanız gereken [Java Hızlı Başlangıç](quickstart-java.md) ilk. Java geliştirme belirli olmayan Linux için App Service'ı kullanma hakkında genel soruları [App Service Linux SSS](app-service-linux-faq.md).

## <a name="logging-and-debugging-apps"></a>Günlüğe kaydetme ve hata ayıklama uygulamaları

Performans raporları, trafiği görselleştirmeler ve sistem durumu denetlemeler Azure portalı üzerinden eeach uygulama için kullanılabilir. Bkz: [Azure App Service tanılama genel bakış](/azure/app-service/app-service-diagnostics) erişmek ve bu tanılama araçları kullanma hakkında daha fazla bilgi için.

### <a name="ssh-console-access"></a>SSH konsol erişimi 

SSH bağlantısı, uygulamanızı çalıştıran Linux ortamı tarafından oynatılan ' dir. Bkz: [Linux'ta Azure App Service için SSH desteği](/azure/app-service/containers/app-service-linux-ssh-support) web tarayıcınızı veya yerel terminalde aracılığıyla Linux sistemine bağlanmak tam yönergeler için.

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

Daha fazla bilgi için [akış günlükleri Azure CLI ile](/azure/app-service/web-sites-enable-diagnostic-log#streaming-with-azure-command-line-interface).

### <a name="app-logging"></a>Uygulama günlüğü

Etkinleştirme [uygulama günlüğü](/azure/app-service/web-sites-enable-diagnostic-log#enablediag) Azure portalından veya [Azure CLI](/cli/azure/webapp/log#az-webapp-log-config) App Service'nın yerel, uygulamanızın standart konsol çıkışı ve standart konsol hatası akış yazmak için yapılandırmak için dosya sistemi veya Azure Blob Depolama. Yerel App Service dosya sisteminde oturum örnek 12 saat yapılandırıldıktan sonra devre dışı bırakıldı. Daha uzun bekletme süresi gerekiyorsa, bir Blob Depolama kapsayıcısına çıkışını yazmak için uygulamayı yapılandırma.

Uygulamanız kullanıyorsa [Logback](https://logback.qos.ch/) veya [Log4j](https://logging.apache.org/log4j) izleme için günlüğekaydetmeçerçevesiyapılandırmayönergelerikullanarakgözdengeçirmeiçinAzureApplicationInsightsiçindebuizlemeleriniletebilir[Keşfedin Java izleme günlükleri Application Insights'ta](/azure/application-insights/app-insights-java-trace-logs). 

## <a name="customization-and-tuning"></a>Özelleştirme ve ayarlama

Linux için Azure App Service kutusu ayarlama ve Azure portalı ve CLI aracılığıyla özelleştirmeyi destekler. Belirli olmayan bir Java web uygulama yapılandırması için aşağıdaki makaleleri inceleyin:

- [App Service ayarlarını yapılandırma](/azure/app-service/web-sites-configure?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json)
- [Özel etki alanını ayarlama](/azure/app-service/app-service-web-tutorial-custom-domain?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json)
- [SSL'yi etkinleştirme](/azure/app-service/app-service-web-tutorial-custom-ssl?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json)
- [CDN ekleme](/azure/cdn/cdn-add-to-web-app?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json)

### <a name="set-java-runtime-options"></a>Java Çalışma zamanı seçenekleri

Tomcat ve Java SE ortamlarında ayrılan bellek veya diğer JVM çalışma zamanı seçenekleri ayarlamak için aşağıda gösterildiği gibi JAVA_OPTS Ayarla bir [uygulama ayarı](/azure/app-service/web-sites-configure#app-settings). Başlatıldığında app Service Linux Java Çalışma zamanı için bu ayarı bir ortam değişkeni geçirir.

Azure portalında altında **uygulama ayarları** adlı yeni bir uygulama ayarı için web app oluşturmak `JAVA_OPTS` gibi ek ayarlar içeren `$JAVA_OPTS -Xms512m -Xmx1204m`.

Azure App Service Linux Maven plugin uygulama ayarlarını yapılandırmak için Azure eklentisi bölümünde ayarı/değer etiketler ekleyin. Aşağıdaki örnek, belirli bir minimum ve maksimum Java heapsıze ayarlar:

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
az webapp config set -n ${WEBAPP_NAME} -g ${WEBAPP_RESOURCEGROUP_NAME} --web-sockets-enabled true 
```

Ardından, uygulamanızı yeniden başlatın:

```azurecli-interactive
az webapp stop -n ${WEBAPP_NAME} -g ${WEBAPP_RESOURCEGROUP_NAME} 
az webapp start -n ${WEBAPP_NAME} -g ${WEBAPP_RESOURCEGROUP_NAME}  
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

## <a name="secure-application"></a>Güvenli uygulama

Linux için App Service'te çalışan Java uygulamalarını kümesinin aynısına sahip [en iyi güvenlik uygulamaları](/azure/security/security-paas-applications-using-app-services) diğer uygulamalar olarak. 

### <a name="authenticate-users"></a>Kullanıcıların kimliklerini doğrulama

Uygulama kimlik doğrulaması ile Azure portalında ayarlama **kimlik doğrulama ve yetkilendirme** seçeneği. Burada, Azure Active Directory veya Facebook, Google veya GitHub gibi sosyal oturum açma bilgilerini kullanarak kimlik doğrulamasını etkinleştirebilirsiniz. Azure portal yapılandırması yalnızca tek bir kimlik doğrulama sağlayıcısı yapılandırırken çalışır.  Daha fazla bilgi için [App Service uygulamanızı Azure Active Directory oturum açma bilgilerini kullanacak şekilde yapılandırma](/azure/app-service/app-service-mobile-how-to-configure-active-directory-authentication) ve diğer kimlik sağlayıcıları için ilgili makaleler.

Birden çok oturum açma sağlayıcısı etkinleştirmeniz gerekirse, yönergeleri izleyin [App Service kimlik doğrulaması özelleştirme](https://docs.microsoft.com/azure/app-service/app-service-authentication-how-to) makalesi.

 Spring önyükleme geliştiriciler [Azure Active Directory Spring Boot Başlatıcı](/java/azure/spring-framework/configure-spring-boot-starter-java-app-with-azure-active-directory?view=azure-java-stable) tanıdık Spring güvenlik açıklamalarını ve API'leri kullanarak uygulamaların güvenliğini sağlamak için.

### <a name="configure-tlsssl"></a>TLS/SSL'yi yapılandırma

Bölümündeki yönergeleri [var olan özel bir SSL sertifikası bağlama](/azure/app-service/app-service-web-tutorial-custom-ssl) mevcut bir SSL sertifikasını karşıya yüklemek ve uygulamanızın etki alanı adı için bağlama için. Varsayılan olarak, uygulamanızın HTTP bağlantıları-özel SSL ve TLS zorlamak için öğreticinin adımlarını yine de sağlar.

## <a name="tomcat"></a>Tomcat 

### <a name="connecting-to-data-sources"></a>Veri kaynaklarına bağlanma

>[!NOTE]
> Uygulamanız Spring Framework veya Spring Boot kullanıyorsa, veritabanı bağlantısı bilgilerini Spring veri JPA [uygulama özellikleri dosyanızda] ortam değişkenleri olarak ayarlayabilirsiniz. Ardından [uygulama ayarları](/azure/app-service/web-sites-configure#app-settings) bu değerler uygulamanız için Azure portal veya CLI tanımlamak için.

Tomcat veritabanlarına Java veritabanı bağlantısı (JDBC) veya Java Kalıcılık API (JPA) kullanarak yönetilen bağlantıları kullanacak şekilde yapılandırmak için önce başlangıç Tomcat tarafından okunan CATALINA_OPTS ortam değişkeni özelleştirin. Bu değerler bir uygulama ayarı ile App Service Maven eklentisi ayarlayın:

```xml
<appSettings> 
    <property> 
        <name>CATALINA_OPTS</name> 
        <value>"$CATALINA_OPTS -Dmysqluser=${mysqluser} -Dmysqlpass=${mysqlpass} -DmysqlURL=${mysqlURL}"</value> 
    </property> 
</appSettings> 
```

Veya eşdeğer bir App Service, Azure portalından ayarlama.

Ardından, veri kaynağının yalnızca bir uygulama veya tüm App Service planıyla çalışan uygulamalar için kullanılabilir hale gerekip gerekmediğini belirleyin.

Uygulama düzeyi veri kaynakları için: 

1. Ekleme bir `context.xml` , web uygulamanız için mevcut değil, eklemek, dosya `META-INF` WAR dosyanızı proje oluşturulduğunda, dizin.

2. Bu dosyadaki Ekle bir `Context` JNDI adresine veri kaynağına bağlamak için yol giriş. ,

    ```xml
    <Context>
        <Resource
            name="jdbc/mysqldb" type="javax.sql.DataSource"
            url="${mysqlURL}"
            driverClassName="com.mysql.jdbc.Driver"
            username="${mysqluser}" password="${mysqlpass}"
        />
    </Context>
    ```

3. Uygulamanızın güncelleştirme `web.xml` uygulamanızdaki veri kaynağını kullanmak için.

    ```xml
    <resource-env-ref>
        <resource-env-ref-name>jdbc/mysqldb</resource-env-ref-name>
        <resource-env-ref-type>javax.sql.DataSource</resource-env-ref-type>
    </resource-env-ref>
    ```

Paylaşılan sunucu düzeyinde kaynaklar için:

1. İçeriğini kopyalayın `/usr/local/tomcat/conf` içine `/home/tomcat` , App Service Linux üzerinde örnek, bir yapılandırma var. henüz yoksa SSH kullanarak.

2. Bağlamına ekleyin, `server.xml`

    ```xml
    <Context>
        <Resource
            name="jdbc/mysqldb" type="javax.sql.DataSource"
            url="${mysqlURL}"
            driverClassName="com.mysql.jdbc.Driver"
            username="${mysqluser}" password="${mysqlpass}"
        />
    </Context>
    ```

3. Uygulamanızın güncelleştirme `web.xml` uygulamanızdaki veri kaynağını kullanmak için.

    ```xml
    <resource-env-ref>
        <resource-env-ref-name>jdbc/mysqldb</resource-env-ref-name>
        <resource-env-ref-type>javax.sql.DataSource</resource-env-ref-type>
    </resource-env-ref>
    ```

4. Bunları yerleştirerek JDBC sürücüsü dosyaları için Tomcat classloader kullanılabilir olduğundan emin olun `/home/tomcat/lib` dizin. Bu dosyalar, App Service örneğine karşıya yüklemek için aşağıdaki adımları gerçekleştirin:  
    1. Azure App Service webpp uzantıyı yükleyin:
      ```azurecli-interactive
      az extension add –name webapp
      ```
    2. App Service için yerel sisteminizden SSH tüneli oluşturmaya aşağıdaki CLI komutunu çalıştırın:
      ```azurecli-interactive
      az webapp remote-connection create –g [resource group] -n [app name] -p [local port to open]
      ```
    3. SFTP istemcinizi ile yerel tünel bağlantı noktasına bağlanmak ve dosyaları karşıya yükleme `/home/tomcat/lib`.

5. App Service Linux uygulamayı yeniden başlatın. Tomcat sıfırlanır `CATALINA_HOME` için `/home/tomcat` ve sınıfları ve güncelleştirilmiş yapılandırmayı kullanın.

## <a name="docker-containers"></a>Docker kapsayıcıları

Azure tarafından desteklenen Zulu kapsayıcılarınızı App Service'te çalışan JDK kullanılacak, uygulamanızın emin `Dockerfile` görüntüleri kullanan [Java App Service Docker görüntü deposu](https://github.com/Azure-App-Service/java).

## <a name="runtime-availability-and-statement-of-support"></a>Çalışma zamanı kullanılabilirlik ve Destek bildirimi

Linux için App Service, yönetilen Java web uygulamalarını barındırmak için iki çalışma zamanlarını destekler:

- [Tomcat servlet kapsayıcı](http://tomcat.apache.org/) paketlenmiş uygulamaları çalıştırmak için farklı web arşivi (WAR) dosyaları. Desteklenen sürümler şunlardır: 8,5 ve 9.0 sürümlerine ait.
- Uygulamaları çalıştırmak için Java SE çalışma zamanı ortamı (JAR) dosyaları Java arşiv paketlendi. Yalnızca desteklenen ana sürüm Java 8 ' dir.

## <a name="java-runtime-statement-of-support"></a>Java Çalışma zamanı destek bildirimi 

### <a name="jdk-versions-and-maintenance"></a>JDK sürümleri ve Bakım

Azure'nın desteklenen Java Development Kit (JDK) olan [Zulu](https://www.azul.com/products/zulu-and-zulu-enterprise/) aracılığıyla sağlanan [Azul Systems](https://www.azul.com/).

Ana sürüm güncelleştirmeleri Linux için Azure App service'taki yeni çalışma zamanı seçenekleri aracılığıyla sağlanır. Müşteriler, App Service dağıtımı yapılandırarak Java daha yeni sürümleri için güncelleştirme ve test etmeden sorumlu ve ihtiyaçlarını karşılayan önemli güncelleştirme sağlama.

Desteklenen JDK otomatik olarak üç aylık olarak Ocak, Nisan, Temmuz ve Ekim her yıl düzeltme eki.

### <a name="security-updates"></a>Güvenlik güncelleştirmeleri

Azul sistemlerden kullanılabilir olduklarında hemen sonra yamaları ve düzeltmeler önemli güvenlik açıkları için kullanıma sunulacaktır. "Büyük" bir güvenlik açığı 9.0 temel puanı ile tanımlanan ya da üzerinde daha büyük [NIST ortak güvenlik açığı Puanlama sistemi, sürüm 2](https://nvd.nist.gov/cvss.cfm).

### <a name="deprecation-and-retirement"></a>Kullanımdan kaldırma ve devre dışı bırakma

Desteklenen Java Çalışma zamanı kullanımdan kaldırılacak, çalışma zamanı kullanımdan önce en az altı ay etkilenen çalışma zamanı kullanan Azure geliştiricileri, kullanımdan kaldırma bildirimi verilir.

### <a name="local-development"></a>Yerel geliştirme

Geliştiriciler, üretim sürümü, Azul Zulu Enterprise JDK yerel geliştirme için indirebileceği [Azul'ın indirme sitesinde](https://www.azul.com/downloads/zulu/).

### <a name="development-support"></a>Geliştirme desteği

Azure için geliştirirken Azul Zulu Enterprise JDK ürün desteği aracılığıyla veya [Azure Stack](https://azure.microsoft.com/overview/azure-stack/) ile bir [Azure destek planı koşullu](https://azure.microsoft.com/support/plans/).

### <a name="runtime-support"></a>Çalışma zamanı desteği

Geliştiriciler şunları yapabilir [bir sorun açın](/azure/azure-supportability/how-to-create-azure-support-request) oluşturulduysa Azure desteği aracılığıyla App Service Linux Java Çalışma zamanı ile bir [tam destek planı](https://azure.microsoft.com/support/plans/).

## <a name="next-steps"></a>Sonraki adımlar

Ziyaret [Java geliştiricileri için Azure](/java/azure/) Azure hızlı başlangıç kılavuzlarımız, öğreticilerimiz ve Java başvuru belgeleri bulmak için merkezi.
