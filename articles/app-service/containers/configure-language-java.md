---
title: Linux Java uygulamaları - Azure App Service'ı yapılandırma | Microsoft Docs
description: Linux üzerinde Azure App Service'te çalışan Java uygulamalarını yapılandırmayı öğrenin.
keywords: Azure app service, web uygulaması, linux, oss, java, java ee jee, javaee
services: app-service
author: bmitchell287
manager: douge
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: java
ms.topic: article
ms.date: 06/26/2019
ms.author: brendm
ms.custom: seodec18
ms.openlocfilehash: af6fd7b99147396a70fccc7b2b11dfef3def15a8
ms.sourcegitcommit: 1572b615c8f863be4986c23ea2ff7642b02bc605
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2019
ms.locfileid: "67786305"
---
# <a name="configure-a-linux-java-app-for-azure-app-service"></a>Azure App Service için Linux Java uygulaması yapılandırma

Linux üzerinde Azure App Service'te Java geliştiricilerinin kolayca oluşturmanızı, dağıtmanızı ve bunların Tomcat ölçeklendirme sağlar veya Linux tabanlı tam olarak yönetilen bir hizmet üzerinde web uygulamaları Java Standard Edition (SE) paketlenir. Komut satırından veya Intellij, Eclipse veya Visual Studio Code gibi düzenleyicilerde uygulamalarını Maven eklentileri ile dağıtın.

Bu kılavuzu temel kavramları ve App Service'te yerleşik bir Linux kapsayıcı kullanan Java geliştiricileri için yönergeler sağlar. Azure App Service daha önce kullanmadıysanız izleyin [Java Hızlı Başlangıç](quickstart-java.md) ve [Java PostgreSQL öğreticisiyle](tutorial-java-enterprise-postgresql-app.md) ilk.

## <a name="deploying-your-app"></a>Uygulamanızı dağıtma

Kullanabileceğiniz [Azure App Service için Maven Plugin](/java/api/overview/azure/maven/azure-webapp-maven-plugin/readme) .jar hem .war dosyaları dağıtmak için. Popüler ıde'lerle dağıtım ile desteklenen ayrıca [Intellij için Azure Araç Seti](/java/azure/intellij/azure-toolkit-for-intellij) veya [Eclipse için Azure Araç Seti](/java/azure/eclipse/azure-toolkit-for-eclipse).

Aksi takdirde, dağıtım yöntemini, arşiv türüne bağlıdır:

- Tomcat için .war dosyaları dağıtmak için kullanın `/api/wardeploy/` Arşiv dosyasının göndermek için uç nokta. Bu API hakkında daha fazla bilgi için lütfen bkz [bu belgeleri](https://docs.microsoft.com/azure/app-service/deploy-zip#deploy-war-file).
- Java SE görüntülerindeki .jar dosyalarını dağıtmak için `/api/zipdeploy/` Kudu sitesi uç noktası. Bu API hakkında daha fazla bilgi için lütfen bkz [bu belgeleri](https://docs.microsoft.com/azure/app-service/deploy-zip#rest).

.War veya FTP kullanarak bir .jar dağıtmayın. FTP aracı başlatma komut dosyaları, bağımlılıklar veya diğer çalışma zamanı dosyalarını karşıya yüklemek için tasarlanmıştır. Web uygulamaları dağıtmak için en uygun bir seçenek değil.

## <a name="logging-and-debugging-apps"></a>Günlüğe kaydetme ve hata ayıklama uygulamaları

Performans raporları, trafiği görselleştirmeler ve sistem durumu denetlemeler Azure portalı üzerinden her uygulama için kullanılabilir. Daha fazla bilgi için [Azure App Service tanılama genel bakış](../overview-diagnostics.md).

### <a name="ssh-console-access"></a>SSH konsol erişimi

[!INCLUDE [Open SSH session in browser](../../../includes/app-service-web-ssh-connect-builtin-no-h.md)]

### <a name="stream-diagnostic-logs"></a>Tanılama günlüklerini akışla aktarma

[!INCLUDE [Access diagnostic logs](../../../includes/app-service-web-logs-access-no-h.md)]

Daha fazla bilgi için [akış günlükleri Azure CLI ile](../troubleshoot-diagnostic-logs.md#streaming-with-azure-cli).

### <a name="app-logging"></a>Uygulama günlüğü

Etkinleştirme [uygulama günlüğü](../troubleshoot-diagnostic-logs.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json#enablediag) Azure portalından veya [Azure CLI](/cli/azure/webapp/log#az-webapp-log-config) App Service'nın yerel, uygulamanızın standart konsol çıkışı ve standart konsol hatası akış yazmak için yapılandırmak için dosya sistemi veya Azure Blob Depolama. Yerel App Service dosya sisteminde oturum örnek 12 saat yapılandırıldıktan sonra devre dışı bırakıldı. Daha uzun bekletme süresi gerekiyorsa, bir Blob Depolama kapsayıcısına çıkışını yazmak için uygulamayı yapılandırma. Java ve Tomcat uygulama günlüklerinizi bulunabilir */home/LogFiles/uygulama/* dizin.

Uygulamanız kullanıyorsa [Logback](https://logback.qos.ch/) veya [Log4j](https://logging.apache.org/log4j) izleme için günlüğekaydetmeçerçevesiyapılandırmayönergelerikullanarakgözdengeçirmeiçinAzureApplicationInsightsiçindebuizlemeleriniletebilir[Keşfedin Java izleme günlükleri Application Insights'ta](/azure/application-insights/app-insights-java-trace-logs).

### <a name="troubleshooting-tools"></a>Sorun giderme araçları

Yerleşik Java görüntüleri temel alan [Alpine Linux](https://alpine-linux.readthedocs.io/en/latest/getting_started.html) işletim sistemi. Kullanım `apk` sorun giderme yüklemek için Paket Yöneticisi Araçları veya komutları.

### <a name="flight-recorder"></a>Uçuş Kaydedicisi

App Service üzerindeki tüm Linux Java görüntülerini kolayca JVM için bağlanın ve kaydı bir profil oluşturucuyu başlatın veya bir yığın dökümü oluşturmak için yüklü olan Zulu uçuş Kaydedicisi vardır.

#### <a name="timed-recording"></a>Zamanlanmış kaydetme

Alma başlatıldı, uygulamanızı App Service'e SSH ve çalıştırma için `jcmd` çalışan tüm Java işlemlerin listesini görmek için komutu. Jcmd yanı sıra kendisini, çalışan bir işlem kimlik numarası (PID) ile Java uygulamanızı görmeniz gerekir.

```shell
078990bbcd11:/home# jcmd
Picked up JAVA_TOOL_OPTIONS: -Djava.net.preferIPv4Stack=true
147 sun.tools.jcmd.JCmd
116 /home/site/wwwroot/app.jar
```

JVM 30 saniyelik kaydını başlatmak için aşağıdaki komutu yürütün. Bu JVM profil ve adlı JFR dosyası oluşturma *jfr_example.jfr* giriş dizininde. (116 Java uygulamanızın PID ile değiştirin.)

```shell
jcmd 116 JFR.start name=MyRecording settings=profile duration=30s filename="/home/jfr_example.jfr"
```

30 ikinci aralığında kaydı doğrulamak için bir yerde çalıştırarak sürüyor `jcmd 116 JFR.check`. Bu işlem, belirli bir Java işlemi için tüm kayıtları gösterir.

#### <a name="continuous-recording"></a>Sürekli kaydetme

Çalışma zamanı performansı üzerinde en az etki ile Java uygulamanızı sürekli olarak profilini Zulu uçuş Kaydedicisi kullanabilirsiniz ([kaynak](https://assets.azul.com/files/Zulu-Mission-Control-data-sheet-31-Mar-19.pdf)). Bunu yapmak için gerekli yapılandırmayla JAVA_OPTS adlı bir uygulama ayarı oluşturmak için aşağıdaki Azure CLI komutunu çalıştırın. Uygulama ayarı JAVA_OPTS içeriğini geçirilen `java` , uygulamanız başlatıldığında komutu.

```azurecli
az webapp config appsettings set -g <your_resource_group> -n <your_app_name> --settings JAVA_OPTS=-XX:StartFlightRecording=disk=true,name=continuous_recording,dumponexit=true,maxsize=1024m,maxage=1d
```

Daha fazla bilgi için lütfen bkz [Jcmd komut başvurusu](https://docs.oracle.com/javacomponents/jmc-5-5/jfr-runtime-guide/comline.htm#JFRRT190).

### <a name="analyzing-recordings"></a>Kayıtları analiz etme

Kullanım [FTPS](../deploy-ftp.md) JFR dosyanızın yerel makinenize indirmek için. JFR dosyasını çözümlemek için indirme ve yükleme [Zulu görev kontrol merkezidir](https://www.azul.com/products/zulu-mission-control/). Zulu dili görev kontrol merkezidir hakkında yönergeler için bkz [Azul belgeleri](https://docs.azul.com/zmc/) ve [yükleme yönergeleri](https://docs.microsoft.com/java/azure/jdk/java-jdk-flight-recorder-and-mission-control).

## <a name="customization-and-tuning"></a>Özelleştirme ve ayarlama

Linux için Azure App Service kutusu ayarlama ve Azure portalı ve CLI aracılığıyla özelleştirmeyi destekler. Java'ya özgü web uygulama yapılandırması için aşağıdaki makaleleri inceleyin:

- [Uygulama ayarlarını yapılandırma](../configure-common.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json#configure-app-settings)
- [Özel etki alanını ayarlama](../app-service-web-tutorial-custom-domain.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json)
- [SSL'yi etkinleştirme](../app-service-web-tutorial-custom-ssl.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json)
- [CDN ekleme](../../cdn/cdn-add-to-web-app.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json)
- [Kudu sitesi yapılandırma](https://github.com/projectkudu/kudu/wiki/Configurable-settings#linux-on-app-service-settings)

### <a name="set-java-runtime-options"></a>Java Çalışma zamanı seçenekleri

Tomcat ve Java SE ortamlarında ayrılan bellek veya diğer JVM çalışma zamanı seçenekleri ayarlamak için oluşturun bir [uygulama ayarı](../configure-common.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json#configure-app-settings) adlı `JAVA_OPTS` seçenekleri. Başlatıldığında app Service Linux Java Çalışma zamanı için bu ayarı bir ortam değişkeni geçirir.

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

Bir JAR uygulama dağıtıyorsanız, adında *app.jar* yerleşik görüntü uygulamanızın doğru şekilde belirleyebilirsiniz. (Maven plugin, bu otomatik olarak yeniden adlandırma işlemi yapar.) Yeniden adlandırmak için JAR istemiyorsanız *app.jar*, jar dosyasını çalıştırmak için komutu ile bir kabuk betiği karşıya yükleyebilirsiniz. Bu komut dosyasında tam yolu yapıştırın [başlangıç dosyası](app-service-linux-faq.md#built-in-images) metin kutusunda portalının yapılandırma bölümü.

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

### <a name="pre-compile-jsp-files"></a>JSP dosyaları önceden derleme

Tomcat uygulama performansını artırmak için App Service'ı dağıtmadan önce JSP dosyalarınızı derleyebilirsiniz. Kullanabileceğiniz [Maven plugin](https://sling.apache.org/components/jspc-maven-plugin/plugin-info.html) Apache Sling veya bunu kullanarak tarafından sağlanan [Ant derleme dosyası](https://tomcat.apache.org/tomcat-9.0-doc/jasper-howto.html#Web_Application_Compilation).

## <a name="secure-applications"></a>Güvenli uygulamalar

Linux için App Service'te çalışan Java uygulamalarını kümesinin aynısına sahip [en iyi güvenlik uygulamaları](/azure/security/security-paas-applications-using-app-services) diğer uygulamalar olarak.

### <a name="authenticate-users"></a>Kullanıcıların kimliklerini doğrulama

Azure portalında uygulama kimlik doğrulamasını ayarlama **kimlik doğrulama ve yetkilendirme** seçeneği. Burada, Azure Active Directory veya Facebook, Google veya GitHub gibi sosyal oturum açma bilgilerini kullanarak kimlik doğrulamasını etkinleştirebilirsiniz. Azure portal yapılandırması yalnızca tek bir kimlik doğrulama sağlayıcısı yapılandırırken çalışır. Daha fazla bilgi için [App Service uygulamanızı Azure Active Directory oturum açma bilgilerini kullanacak şekilde yapılandırma](../configure-authentication-provider-aad.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json) ve diğer kimlik sağlayıcıları için ilgili makaleler. Birden çok oturum açma sağlayıcısı etkinleştirmeniz gerekirse, yönergeleri izleyin [App Service kimlik doğrulaması özelleştirme](../app-service-authentication-how-to.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json) makalesi.

#### <a name="tomcat"></a>Tomcat

Tomcat uygulama kullanıcının erişip talep doğrudan sorumlusu atama tarafından Tomcat servlet nesnesinden bir harita nesnesi. Eşlem nesnesine her talep türünü talep türü için bir koleksiyona eşler. Aşağıdaki kodda `request` örneğidir `HttpServletRequest`.

```java
Map<String, Collection<String>> map = (Map<String, Collection<String>>) request.getUserPrincipal();
```

Şimdi, inceleyebilirsiniz `Map` herhangi belirli bir talep nesnesi. Örneğin, aşağıdaki kod parçacığı tüm talep türleri yinelenir ve her koleksiyonun içeriğini yazdırır.

```java
for (Object key : map.keySet()) {
        Object value = map.get(key);
        if (value != null && value instanceof Collection {
            Collection claims = (Collection) value;
            for (Object claim : claims) {
                System.out.println(claims);
            }
        }
    }
```

Kullanıcıların oturumu kapatın ve diğer eylemleri gerçekleştirmek için lütfen belgelere bakın [App Service kimlik doğrulaması ve yetkilendirme kullanım](https://docs.microsoft.com/azure/app-service/app-service-authentication-how-to). Ayrıca Tomcat resmi belgelerine olan [HttpServletRequest arabirimi](https://tomcat.apache.org/tomcat-5.5-doc/servletapi/javax/servlet/http/HttpServletRequest.html) ve yöntemleri. Yöntemleri de hydrated aşağıdaki servlet App Service yapılandırmanızı temel alarak:

```java
public boolean isSecure()
public String getRemoteAddr()
public String getRemoteHost()
public String getScheme()
public int getServerPort()
```

Bu özellik devre dışı bırakmak için adlı bir uygulama ayarı oluşturmak `WEBSITE_AUTH_SKIP_PRINCIPAL` değeriyle `1`. App Service tarafından eklenen tüm servlet filtreleri devre dışı bırakmak için adlı bir ayar oluşturmak `WEBSITE_SKIP_FILTERS` değeriyle `1`.

#### <a name="spring-boot"></a>Spring Boot

Spring önyükleme geliştiriciler [Azure Active Directory Spring Boot Başlatıcı](/java/azure/spring-framework/configure-spring-boot-starter-java-app-with-azure-active-directory?view=azure-java-stable) tanıdık Spring güvenlik açıklamalarını ve API'leri kullanarak uygulamaların güvenliğini sağlamak için. Maksimum boyut olarak artırıldığından emin olun, *application.properties* dosya. Değerini öneririz `16384`.

### <a name="configure-tlsssl"></a>TLS/SSL'yi yapılandırma

Bölümündeki yönergeleri [var olan özel bir SSL sertifikası bağlama](../app-service-web-tutorial-custom-ssl.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json) mevcut bir SSL sertifikasını karşıya yüklemek ve uygulamanızın etki alanı adı için bağlama için. Varsayılan olarak, uygulamanızın HTTP bağlantıları-özel SSL ve TLS zorlamak için öğreticinin adımlarını yine de sağlar.

### <a name="use-keyvault-references"></a>KeyVault başvuruları kullanın

[Azure anahtar kasası](../../key-vault/key-vault-overview.md) erişim ilkeleri ve denetim geçmişi ile gizli merkezi yönetim sağlar. Gizli anahtarları (parolaları veya bağlantı dizeleri için gibi) Keyvault'ta depolayabilir ve ortam değişkenlerini, uygulamanızda bu gizliliklerin erişin.

İlk olarak, yönergelerini izleyin [Key Vault'a erişim verme](../app-service-key-vault-references.md#granting-your-app-access-to-key-vault) ve [bir uygulama ayarı gizli bir anahtar kasası başvurusu yapmak](../app-service-key-vault-references.md#reference-syntax). App Service terminal uzaktan erişirken yazdırma ortam değişkeni tarafından başvuru gizli çözdüğünü doğrulayabilirsiniz.

Bu gizli dizileri Spring veya Tomcat yapılandırma dosyanızdaki eklemesine ortam değişkeni ekleme söz dizimini kullanın (`${MY_ENV_VAR}`). Spring yapılandırma dosyaları için lütfen bu edinmek [yapılandırmaları te dış](https://docs.spring.io/spring-boot/docs/current/reference/html/boot-features-external-config.html).

## <a name="configure-apm-platforms"></a>APM platformları yapılandırma

Bu bölüm, Java uygulamaları (APM) platformları NewRelic ve AppDynamics uygulama performansı izleme ile Linux üzerinde Azure App Service'te dağıtılan bağlama işlemi gösterilmektedir.

[Yeni Relic yapılandırma](#configure-new-relic)
[AppDynamics yapılandırın](#configure-appdynamics)

### <a name="configure-new-relic"></a>Yeni Relic yapılandırın

1. NewRelic hesabında oluşturma [NewRelic.com](https://newrelic.com/signup)
2. Olan NewRelic Java agent yükleme, benzer bir dosya adı olacaktır *newrelic java x.x.x.zip*.
3. Lisans anahtarınızı kopyalayın, buna daha sonra aracıyı yapılandırmak için ihtiyacınız olacak.
4. [App Service örneğinizin içine SSH](app-service-linux-ssh-support.md) ve yeni bir dizin oluşturmak */home/site/wwwroot/apm*.
5. Paketten çıkarılan NewRelic Java aracı dosyalarını bir dizin altında dosya yükleme */home/site/wwwroot/apm*. Aracınızı dosyaları olmalıdır */home/site/wwwroot/apm/newrelic*.
6. YAML dosyasının değiştirme */home/site/wwwroot/apm/newrelic/newrelic.yml* ve yer tutucu lisans değerini kendi lisans anahtarı ile değiştirin.
7. Azure portalında App Service uygulamanızda gidin ve yeni bir uygulama ayarı oluştur.
    - Uygulamanızı kullanıyorsa **Java SE**, adlı bir ortam değişkenini oluşturmak `JAVA_OPTS` değerle `-javaagent:/home/site/wwwroot/apm/newrelic/newrelic.jar`.
    - Kullanıyorsanız **Tomcat**, adlı bir ortam değişkenini oluşturmak `CATALINA_OPTS` değerle `-javaagent:/home/site/wwwroot/apm/newrelic/newrelic.jar`.
    - Kullanıyorsanız **WildFly**, New Relic belgelerine bakın [burada](https://docs.newrelic.com/docs/agents/java-agent/additional-installation/wildfly-version-11-installation-java) JBoss yapılandırma ve Java agent yükleme hakkında yönergeler için.
    - Bir ortam değişkeni için zaten varsa `JAVA_OPTS` veya `CATALINA_OPTS`, ekleme `javaagent` seçeneği sonuna kadar geçerli değeri.

### <a name="configure-appdynamics"></a>AppDynamics yapılandırın

1. AppDynamics hesabınız oluşturma [AppDynamics.com](https://www.appdynamics.com/community/register/)
2. AppDynamics Web sitesinden Java agent yükleme, dosya adı şuna benzer olacaktır *AppServerAgent x.x.x.xxxxx.zip*
3. [App Service örneğinizin içine SSH](app-service-linux-ssh-support.md) ve yeni bir dizin oluşturmak */home/site/wwwroot/apm*.
4. Java aracı dosyalarını bir dizin altında dosya yükleme */home/site/wwwroot/apm*. Aracınızı dosyaları olmalıdır */home/site/wwwroot/apm/appdynamics*.
5. Azure portalında App Service uygulamanızda gidin ve yeni bir uygulama ayarı oluştur.
    - Kullanıyorsanız **Java SE**, adlı bir ortam değişkenini oluşturmak `JAVA_OPTS` değerle `-javaagent:/home/site/wwwroot/apm/appdynamics/javaagent.jar -Dappdynamics.agent.applicationName=<app-name>` burada `<app-name>` App Service adınız.
    - Kullanıyorsanız **Tomcat**, adlı bir ortam değişkenini oluşturmak `CATALINA_OPTS` değerle `-javaagent:/home/site/wwwroot/apm/appdynamics/javaagent.jar -Dappdynamics.agent.applicationName=<app-name>` burada `<app-name>` App Service adınız.
    - Kullanıyorsanız **WildFly**, AppDynamics belgelerine bakın [burada](https://docs.appdynamics.com/display/PRO45/JBoss+and+Wildfly+Startup+Settings) JBoss yapılandırma ve Java agent yükleme hakkında yönergeler için.

## <a name="configure-jar-applications"></a>JAR uygulamaları yapılandır

### <a name="starting-jar-apps"></a>JAR uygulamaları başlatılıyor

Varsayılan olarak adlandırılacak şekilde JAR uygulamanızı App Service bekliyor *app.jar*. Bu ada sahip, otomatik olarak çalıştırılır. Maven kullanıcılarına dahil ederek JAR adına ayarlayabilirsiniz `<finalName>app</finalName>` içinde `<build>` bölümünü, *pom.xml*. [Gradle aynı yapabileceğiniz](https://docs.gradle.org/current/dsl/org.gradle.api.tasks.bundling.Jar.html#org.gradle.api.tasks.bundling.Jar:archiveFileName) ayarlayarak `archiveFileName` özelliği.

Ayrıca, JAR için farklı bir ad kullanmak istiyorsanız, sağlamalısınız [başlangıç komutu](app-service-linux-faq.md#built-in-images) , JAR dosyasını yürütür. Örneğin: `java -jar my-jar-app.jar`. Portalında, yapılandırması, başlangıç komutu için değer ayarlayabilirsiniz > Genel ayarları veya adlı bir uygulama ayarı ile `STARTUP_COMMAND`.

### <a name="server-port"></a>Sunucu bağlantı noktası

Uygulama bağlantı noktası 80 üzerinde de dinleyecek şekilde app Service Linux gelen istekleri 80 numaralı bağlantı noktasına yönlendirir. Bunu, uygulamanızın yapılandırmasında yapabilirsiniz (Spring'ın gibi *application.properties* dosyası), veya başlangıç komutunuz (örneğin, `java -jar spring-app.jar --server.port=80`). Lütfen genel Java çerçeveler için aşağıdaki belgelere bakın:

- [Spring Boot](https://docs.spring.io/spring-boot/docs/current/reference/html/howto-properties-and-configuration.html#howto-use-short-command-line-arguments)
- [SparkJava](http://sparkjava.com/documentation#embedded-web-server)
- [Micronaut](https://docs.micronaut.io/latest/guide/index.html#runningSpecificPort)
- [Framework Yürüt](https://www.playframework.com/documentation/2.6.x/ConfiguringHttps#Configuring-HTTPS)
- [Vertx](https://vertx.io/docs/vertx-core/java/#_start_the_server_listening)
- [Quarkus](https://quarkus.io/guides/application-configuration-guide)

## <a name="data-sources"></a>Veri kaynakları

### <a name="tomcat"></a>Tomcat

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

Veya ortam değişkenlerini kümesinde **yapılandırma** > **uygulama ayarları** Azure portalında sayfası.

Ardından, veri kaynağı bir uygulama veya Tomcat servlet üzerinde çalışan tüm uygulamalar için kullanılabilir olup olmayacağını belirler.

#### <a name="application-level-data-sources"></a>Uygulama düzeyi veri kaynakları

1. Oluşturma bir *context.xml* dosyası *META INF /* projenizin dizin. Oluşturma *META INF /* henüz yoksa dizin.

2. İçinde *context.xml*, ekleme bir `Context` JNDI adresine veri kaynağına bağlamak için öğesi. Değiştirin `driverClassName` Yukarıdaki tablodaki sürücünüzün sınıf adı ile yer tutucu.

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

3. Uygulamanızın güncelleştirme *web.xml* uygulamanızdaki veri kaynağını kullanmak için.

    ```xml
    <resource-env-ref>
        <resource-env-ref-name>jdbc/dbconnection</resource-env-ref-name>
        <resource-env-ref-type>javax.sql.DataSource</resource-env-ref-type>
    </resource-env-ref>
    ```

#### <a name="shared-server-level-resources"></a>Paylaşılan sunucu düzeyinde kaynaklar

1. İçeriğini kopyalayın */usr/local/tomcat/conf* içine */home/tomcat/conf* , App Service Linux üzerinde örnek, bir yapılandırma var. henüz yoksa SSH kullanarak.

    ```bash
    mkdir -p /home/tomcat
    cp -a /usr/local/tomcat/conf /home/tomcat/conf
    ```

2. Bir bağlam öğesine ekleyin, *server.xml* içinde `<Server>` öğesi.

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

3. Uygulamanızın güncelleştirme *web.xml* uygulamanızdaki veri kaynağını kullanmak için.

    ```xml
    <resource-env-ref>
        <resource-env-ref-name>jdbc/dbconnection</resource-env-ref-name>
        <resource-env-ref-type>javax.sql.DataSource</resource-env-ref-type>
    </resource-env-ref>
    ```

#### <a name="finalize-configuration"></a>Yapılandırmayı son haline getir

Son olarak, sürücü jar dosyaları dışındaki Tomcat sınıf yerleştirin ve App service'inizi yeniden başlatın.

1. Bunları yerleştirerek JDBC sürücüsü dosyaları için Tomcat classloader kullanılabilir olduğundan emin olun */home/tomcat/lib* dizin. (Zaten yoksa, bu dizin oluşturun.) Bu dosyalar, App Service örneğine karşıya yüklemek için aşağıdaki adımları gerçekleştirin:

    1. İçinde [Cloud Shell](https://shell.azure.com), webapp uzantıyı yükleyin:

      ```azurecli-interactive
      az extension add -–name webapp
      ```

    2. App Service için yerel sisteminizden SSH tüneli oluşturma için aşağıdaki CLI komutunu çalıştırın:

      ```azurecli-interactive
      az webapp remote-connection create --resource-group <resource-group-name> --name <app-name> --port <port-on-local-machine>
      ```

    3. SFTP istemcinizi ile yerel tünel bağlantı noktasına bağlanmak ve dosyaları karşıya yükleme */home/tomcat/lib* klasör.

    Alternatif olarak, JDBC sürücüsünü yüklenecek bir FTP istemcisi kullanabilirsiniz. Aşağıdaki adımları [FTP kimlik bilgilerinizi almak için yönergeler](../deploy-configure-credentials.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json).

2. Sunucu düzeyinde veri kaynağı oluşturduysanız, App Service Linux uygulamayı yeniden başlatın. Tomcat sıfırlanır `CATALINA_HOME` için `/home/tomcat/conf` ve güncelleştirilmiş yapılandırmayı kullanın.

### <a name="spring-boot"></a>Spring Boot

Spring Boot uygulamalarda veri kaynaklarına bağlanmak için bağlantı dizeleri oluşturmak ve bunları ekleme öneririz, *application.properties* dosya.

1. App Service sayfası "Yapılandırma" bölümünde, dize için bir ad ayarlayın, JDBC bağlantı dizesi değer alanına yapıştırın ve türü "Özel" olarak ayarlayın. İsteğe bağlı olarak, bu bağlantı dizesini yuva ayarı olarak ayarlayabilirsiniz.

    Bu bağlantı dizesini uygulamamız adlı bir ortam değişkeni olarak erişilebilir `CUSTOMCONNSTR_<your-string-name>`. Örneğin, yukarıda oluşturduğumuz bağlantı dizesini adlandırılacağını `CUSTOMCONNSTR_exampledb`.

2. İçinde *application.properties* dosya, bu bağlantı dizesi ortam değişkeni adı ile başvuru. Bizim örneğimizde, biz kullanırsınız.

    ```yml
    app.datasource.url=${CUSTOMCONNSTR_exampledb}
    ```

Lütfen [veri erişimi üzerinde Spring Boot belgeleri](https://docs.spring.io/spring-boot/docs/current/reference/html/howto-data-access.html) ve [yapılandırmaları te dış](https://docs.spring.io/spring-boot/docs/current/reference/html/boot-features-external-config.html) Bu konu hakkında daha fazla bilgi için.

## <a name="configure-java-ee-wildfly"></a>Yapılandırma Java EE (WildFly)

> [!NOTE]
> App Service Linux üzerinde Java Enterprise Edition, şu anda Önizleme aşamasındadır. Bu yığın **değil** üretim yönelik iş için önerilir. Java SE ve Tomcat sunduğumuz yığınlar hakkında bilgiler.

Linux üzerinde Azure App Service'te Java geliştiricilerinin oluşturmanızı, dağıtmanızı ve Linux tabanlı tam olarak yönetilen bir hizmet üzerinde Java Enterprise (Java EE) uygulama ölçeklendirme sağlar.  Temel Kurumsal Java Çalışma zamanı ortamı olan açık kaynaklı [WildFly](https://wildfly.org/) uygulama sunucusu.

Bu bölüm aşağıdaki alt bölümleri içerir:

- [App Service ile ölçeklendirme](#scale-with-app-service)
- [Uygulama sunucusu yapılandırması özelleştirme](#customize-application-server-configuration)
- [Modüller ve bağımlılıkları yükleme](#install-modules-and-dependencies)
- [Veri kaynaklarını yapılandırma](#configure-data-sources)
- [Mesajlaşma sağlayıcıları etkinleştir](#enable-messaging-providers)
- [Yönetim oturumu önbelleğe almayı yapılandır](#configure-session-management-caching)

### <a name="scale-with-app-service"></a>App Service ile ölçeklendirme

Linux üzerinde App Service'te çalışan WildFly uygulama sunucusu bir etki alanı yapılandırmasında tek başına modunda çalışır. App Service planı ölçeğini daralttığınızda her WildFly örnek bir tek başına sunucu olarak yapılandırılır.

Uygulamanız ile yatay veya dikey olarak ölçeklendirme [ölçeklendirme kuralları](../../monitoring-and-diagnostics/monitoring-autoscale-get-started.md) ve [, örnek sayısını artırıyorsanız](../web-sites-scale.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json).

### <a name="customize-application-server-configuration"></a>Uygulama sunucusu yapılandırması özelleştirme

Web uygulaması örneği durum bilgisi olduğundan başlatılan yeni örneklerin başlangıçta uygulama tarafından gerek duyulan WildFly yapılandırmayı desteklemek için yapılandırılması gerekir.
Bir başlangıç WildFly CLI için çağrılacak Bash betiği yazabilirsiniz:

- Veri kaynakları ayarlayın
- Mesajlaşma sağlayıcılarını yapılandırma
- Diğer modüller ve bağımlılıkları WildFly sunucu yapılandırmasına ekleyin.

Betik WildFly hazır ve çalışır durumda olduğunda, ancak uygulama başlatılmadan önce çalışır. Betik kullanması gereken [JBOSS CLI](https://docs.jboss.org/author/display/WFLY/Command+Line+Interface) çağrılır */opt/jboss/wildfly/bin/jboss-cli.sh* herhangi bir yapılandırma veya sunucu başladıktan sonra gerekli değişiklikleri uygulama sunucusu yapılandırmak için.

CLI'ın etkileşimli mod WildFly yapılandırmak için kullanmayın. İçin JBoss CLI kullanarak bir komut dosyası komut bunun yerine, sağlayabilir `--file` komutu, örneğin:

```bash
/opt/jboss/wildfly/bin/jboss-cli.sh -c --file=/path/to/your/jboss_commands.cli
```

App Service Örneğinizdeki altında bir konuma başlangıç komut dosyasını karşıya yüklemek için FTP kullanma, */home* gibi dizin */home/site/deployments/tools*. Daha fazla bilgi için bkz. [uygulamanızı FTP/S kullanarak Azure App Service'e dağıtma](https://docs.microsoft.com/azure/app-service/deploy-ftp).

Ayarlama **başlangıç betiği** Azure portalında, başlangıç Kabuk betiği konumunu Örneğin alan */home/site/deployments/tools/your-startup-script.sh*.

Tedarik [uygulama ayarları](../configure-common.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json#configure-app-settings) kullanmak için ortam değişkenlerini betiğe geçirmek için uygulama yapılandırması. Uygulama ayarları, bağlantı dizeleri ve sürüm denetimi dışında Uygulamanızı yapılandırmak için gerekli diğer gizli dizileri koruyun.

### <a name="install-modules-and-dependencies"></a>Modüller ve bağımlılıkları yükleme

JBoss CLI aracılığıyla WildFly sınıf içine modülleri ve bağımlılıklarını yüklemek için kendi dizininde aşağıdaki dosyalar oluşturmanız gerekir. Çoğu durumda bir bağımlılık yapılandırmak gerekenler, minimum düzeyde bu liste, bu nedenle bazı modüller ve bağımlılıkları JNDI adlandırma gibi ek yapılandırma veya başka bir API özel yapılandırma gerekebilir.

- Bir [XML modülü tanımlayıcısı](https://jboss-modules.github.io/jboss-modules/manual/#descriptors). Bu XML dosya adı, öznitelikler ve bağımlılıkları modülünüzün tanımlar. Bu [örnek module.xml dosyası](https://access.redhat.com/documentation/en-us/jboss_enterprise_application_platform/6/html/administration_and_configuration_guide/example_postgresql_xa_datasource) Postgres modülü, JAR dosyasını JDBC bağımlılığı ve gerekli diğer modül bağımlılıklarının tanımlar.
- Gerekli JAR dosyası için tüm bağımlılıkların modülünüzde.
- Yeni modül yapılandırmak için JBoss CLI komutları ile bir komut dosyası. Bu dosya sunucusunu bağımlılık kullanacak şekilde yapılandırmak için JBoss CLI tarafından yürütülecek komutlarınızı içerir. Modüller, veri kaynakları ve mesajlaşma sağlayıcıları eklemek için komutlara ilişkin belgeleri için başvurmak [bu belgeyi](https://access.redhat.com/documentation/red_hat_jboss_enterprise_application_platform/7.0/html-single/management_cli_guide/#how_to_cli).
- JBoss CLI'yı arayın ve önceki adımda komut yürütmek için bir Bash başlangıç betiği. Bu dosya, App Service örneğinizin yeniden başlatıldığında veya ölçek genişletme sırasında yeni örnekleri sağlandığında yürütülür. Bu başlangıç betiği JBoss komutları JBoss CLI için geçirilen diğer tüm yapılandırmaları, uygulamanız için yapabileceğiniz gösterilmiştir. En azından, bu dosya için JBoss CLI JBoss CLI komut geçirilecek tek bir komut olabilir:

```bash
/opt/jboss/wildfly/bin/jboss-cli.sh -c --file=/path/to/your/jboss_commands.cli
```

Modülünüzün için içeriği ve dosyaları aldıktan sonra modülün WildFly uygulama sunucusuna eklemek için aşağıdaki adımları izleyin.

1. App Service Örneğinizdeki altında bir konuma dosyalarınızı karşıya yüklemek için FTP kullanın, */home* gibi dizin */home/site/deployments/tools*. Daha fazla bilgi için bkz. [uygulamanızı FTP/S kullanarak Azure App Service'e dağıtma](../deploy-ftp.md).
2. İçinde **yapılandırma** > **genel ayarlar** sayfasında Azure Portalı'nın ayarlamak **başlangıç betiği** için başlangıç Kabuk komut dosyanızın konumuna alan örnek */home/site/deployments/tools/startup.sh*.
3. App Service örneğinizin tuşlarına basarak yeniden **yeniden** düğmesine **genel bakış** bölümü portalı veya Azure CLI kullanarak.

### <a name="configure-data-sources"></a>Veri kaynaklarını yapılandırma

Bir veri kaynağına erişmek için WildFly/JBoss yapılandırmak için yukarıdaki "yükleme modüller ve bağımlılıkları" bölümünde ana hatlarıyla genel işlemi kullanın. Aşağıdaki bölümde, PostgreSQL, MySQL ve SQL Server veri kaynakları için bu işlemi belirli ayrıntılar sağlar.

Bu bölümde, bir uygulama, bir App Service örneği ve bir Azure veritabanı hizmeti örneğine zaten varsayılır. App Service adınız, kendi kaynak grubu ve veritabanı bağlantısı bilgilerinizi aşağıdaki yönergelere bakın. Bu bilgiler Azure portalında bulabilirsiniz.

Örnek bir uygulama kullanarak en baştan tüm sürecinde Git isterseniz bkz [Öğreticisi: Azure'da bir Java EE ve Postgres web uygulaması derleme](tutorial-java-enterprise-postgresql-app.md).

Aşağıdaki adımlar, mevcut App Service ve veritabanına bağlanmak için koşulları açıklamaktadır.

1. İçin JDBC sürücüsünü indir [PostgreSQL](https://jdbc.postgresql.org/download.html), [MySQL](https://dev.mysql.com/downloads/connector/j/), veya [SQL Server](https://docs.microsoft.com/sql/connect/jdbc/download-microsoft-jdbc-driver-for-sql-server). Sürücü .jar dosyasını almak için indirilen Arşiviniz ayıklayın.

2. Gibi bir ada sahip bir dosya oluşturun *module.xml* ve aşağıdaki işaretlemeyi ekleyin. Değiştirin `<module name>` (açılı ayraçlar dahil) yer tutucu ile `org.postgres` , PostgreSQL için `com.mysql` için MySQL veya `com.microsoft` SQL Server için. Değiştirin `<JDBC .jar file path>` .jar dosyasını önceki adımdan adıyla konumun tam yolunu dahil olmak üzere, dosya, App Service örneğinde yerleştirmeniz gerekir. Bu, altında herhangi bir konumda olabilir */home* dizin.

    ```xml
    <?xml version="1.0" ?>
    <module xmlns="urn:jboss:module:1.1" name="<module name>">
        <resources>
           <resource-root path="<JDBC .jar file path>" />
        </resources>
        <dependencies>
            <module name="javax.api"/>
            <module name="javax.transaction.api"/>
        </dependencies>
    </module>
    ```

3. Gibi bir ada sahip bir dosya oluşturun *datasource commands.cli* ve aşağıdaki kodu ekleyin. Değiştirin `<JDBC .jar file path>` önceki adımda kullanılan değerine sahip. Değiştirin `<module file path>` dosya adı ve örnek için bir önceki adımda, App Service yolundan */home/module.xml*.

    **PostgreSQL**

    ```console
    module add --name=org.postgres --resources=<JDBC .jar file path> --module-xml=<module file path>

    /subsystem=datasources/jdbc-driver=postgres:add(driver-name=postgres,driver-module-name=org.postgres,driver-class-name=org.postgresql.Driver,driver-xa-datasource-class-name=org.postgresql.xa.PGXADataSource)

    data-source add --name=postgresDS --driver-name=postgres --jndi-name=java:jboss/datasources/postgresDS --connection-url=$DATABASE_CONNECTION_URL --user-name=$DATABASE_SERVER_ADMIN_FULL_NAME --password=$DATABASE_SERVER_ADMIN_PASSWORD --use-ccm=true --max-pool-size=5 --blocking-timeout-wait-millis=5000 --enabled=true --driver-class=org.postgresql.Driver --exception-sorter-class-name=org.jboss.jca.adapters.jdbc.extensions.postgres.PostgreSQLExceptionSorter --jta=true --use-java-context=true --valid-connection-checker-class-name=org.jboss.jca.adapters.jdbc.extensions.postgres.PostgreSQLValidConnectionChecker

    reload --use-current-server-config=true
    ```

    **MySQL**

    ```console
    module add --name=com.mysql --resources=<JDBC .jar file path> --module-xml=<module file path>

    /subsystem=datasources/jdbc-driver=mysql:add(driver-name=mysql,driver-module-name=com.mysql,driver-class-name=com.mysql.cj.jdbc.Driver)

    data-source add --name=mysqlDS --jndi-name=java:jboss/datasources/mysqlDS --connection-url=$DATABASE_CONNECTION_URL --driver-name=mysql --user-name=$DATABASE_SERVER_ADMIN_FULL_NAME --password=$DATABASE_SERVER_ADMIN_PASSWORD --use-ccm=true --max-pool-size=5 --blocking-timeout-wait-millis=5000 --enabled=true --driver-class=com.mysql.cj.jdbc.Driver --jta=true --use-java-context=true --exception-sorter-class-name=com.mysql.cj.jdbc.integration.jboss.ExtendedMysqlExceptionSorter

    reload --use-current-server-config=true
    ```

    **SQL Server**

    ```console
    module add --name=com.microsoft --resources=<JDBC .jar file path> --module-xml=<module file path>

    /subsystem=datasources/jdbc-driver=sqlserver:add(driver-name=sqlserver,driver-module-name=com.microsoft,driver-class-name=com.microsoft.sqlserver.jdbc.SQLServerDriver,driver-datasource-class-name=com.microsoft.sqlserver.jdbc.SQLServerDataSource)

    data-source add --name=sqlDS --jndi-name=java:jboss/datasources/sqlDS --driver-name=sqlserver --connection-url=$DATABASE_CONNECTION_URL --validate-on-match=true --background-validation=false --valid-connection-checker-class-name=org.jboss.jca.adapters.jdbc.extensions.mssql.MSSQLValidConnectionChecker --exception-sorter-class-name=org.jboss.jca.adapters.jdbc.extensions.mssql.MSSQLExceptionSorter

    reload --use-current-server-config=true
    ```

    Bu dosya, sonraki adımda açıklanan başlangıç betiği çalıştırılır. JDBC sürücüsü WildFly modül olarak yükler, karşılık gelen WildFly veri kaynağını oluşturur ve değişiklikler etkili emin olmak için sunucuyu yeniden yükler.

4. Gibi bir ada sahip bir dosya oluşturun *startup.sh* ve aşağıdaki kodu ekleyin. Değiştirin `<JBoss CLI script>` önceki adımda oluşturduğunuz dosyanın adı. Tam yolu eklediğinizden emin olun konumu, dosya, App Service örneğinde örneğin yerleştireceğiniz */home/datasource-commands.cli*.

    ```bash
    #!/usr/bin/env bash
    /opt/jboss/wildfly/bin/jboss-cli.sh -c --file=<JBoss CLI script>
    ```

5. FTP, App Service örneğine JDBC .jar dosyasını, modül XML dosyası, JBoss CLI betiği ve başlangıç komut dosyasını karşıya yüklemek için kullanın. Bu dosyalar gibi önceki adımlarda belirtilen konumda put */home*. FTP hakkında daha fazla bilgi için bkz. [uygulamanızı FTP/S kullanarak Azure App Service'e dağıtma](https://docs.microsoft.com/azure/app-service/deploy-ftp).

6. Veritabanı bağlantısı bilgilerinizi tutun, App Service ayarları eklemek için Azure CLI'yı kullanın. Değiştirin `<resource group>` ve `<webapp name>` App Service'inizin değerleri kullanır. Değiştirin `<database server name>`, `<database name>`, `<admin name>`, ve `<admin password>` veritabanı bağlantısı bilgilerinizi ile. App Service ve veritabanı bilgilerinizi Azure portalından alabilirsiniz.

    **PostgreSQL:**

    ```bash
    az webapp config appsettings set \
        --resource-group <resource group> \
        --name <webapp name> \
        --settings \
            DATABASE_CONNECTION_URL=jdbc:postgresql://<database server name>:5432/<database name>?ssl=true \
            DATABASE_SERVER_ADMIN_FULL_NAME=<admin name> \
            DATABASE_SERVER_ADMIN_PASSWORD=<admin password>
    ```

    **MySQL:**

    ```bash
    az webapp config appsettings set \
        --resource-group <resource group> \
        --name <webapp name> \
        --settings \
            DATABASE_CONNECTION_URL=jdbc:mysql://<database server name>:3306/<database name>?ssl=true\&useLegacyDatetimeCode=false\&serverTimezone=GMT \
            DATABASE_SERVER_ADMIN_FULL_NAME=<admin name> \
            DATABASE_SERVER_ADMIN_PASSWORD=<admin password>
    ```

    **SQL sunucusu:**

    ```bash
    az webapp config appsettings set \
        --resource-group <resource group> \
        --name <webapp name> \
        --settings \
            DATABASE_CONNECTION_URL=jdbc:sqlserver://<database server name>:1433;database=<database name>;user=<admin name>;password=<admin password>;encrypt=true;trustServerCertificate=false;hostNameInCertificate=*.database.windows.net;loginTimeout=30;
    ```

    DATABASE_CONNECTION_URL değerler şunlardır: her bir veritabanı sunucusu için farklı ve Azure portalında değerler farklı. Burada (ve yukarıdaki kod parçacıkları) gösterilen URL biçimleri WildFly tarafından kullanım için gereklidir:

    * **PostgreSQL:** `jdbc:postgresql://<database server name>:5432/<database name>?ssl=true`
    * **MySQL:** `jdbc:mysql://<database server name>:3306/<database name>?ssl=true\&useLegacyDatetimeCode=false\&serverTimezone=GMT`
    * **SQL sunucusu:** `jdbc:sqlserver://<database server name>:1433;database=<database name>;user=<admin name>;password=<admin password>;encrypt=true;trustServerCertificate=false;hostNameInCertificate=*.database.windows.net;loginTimeout=30;`

7. Azure portalında App Service hizmetinize gidin ve bulmak **yapılandırma** > **genel ayarlar** sayfası. Ayarlama **başlangıç betiği** başlangıç betiğinizi konumunu ve adını için örneğin alan */home/startup.sh*.

App Service hizmetiniz bir sonraki başlatılmasında, başlangıç betiği çalıştırın ve gerekli yapılandırma adımlarını gerçekleştirin. Bu yapılandırmayı doğru oluştuğunu test etmek için SSH kullanarak uygulama hizmetinize erişmek ve sonra Başlangıç betiği kendiniz Bash komut isteminden çalıştırın. Ayrıca, App Service günlüklerini inceleyebilirsiniz. Bu seçenekler hakkında daha fazla bilgi için bkz. [günlüğe kaydetme ve hata ayıklama uygulamaları](#logging-and-debugging-apps).

Ardından, uygulamanız için WildFly yapılandırmasını güncelleştirin ve yeniden dağıtmanız gerekir. Aşağıdaki adımları kullanın:

1. Açık *src/main/resources/META-INF/persistence.xml* dosya uygulama ve bulma `<jta-data-source>` öğesi. Dosyanın içeriğini aşağıda gösterildiği gibi değiştirin:

    **PostgreSQL**

    ```xml
    <jta-data-source>java:jboss/datasources/postgresDS</jta-data-source>
    ```

    **MySQL**

    ```xml
    <jta-data-source>java:jboss/datasources/mysqlDS</jta-data-source>
    ```

    **SQL Server**

    ```xml
    <jta-data-source>java:jboss/datasources/postgresDS</jta-data-source>
    ```

2. Yeniden oluşturun ve Bash isteminde aşağıdaki komutu kullanarak uygulamanızı yeniden dağıtın:

    ```bash
    mvn package -DskipTests azure-webapp:deploy
    ```

3. App Service örneğinizin tuşlarına basarak yeniden **yeniden** düğmesine **genel bakış** bölümü Azure portalı veya Azure CLI kullanarak.

App Service örneğinizin veritabanınıza erişmek için yapılandırılmıştır.

Veritabanı bağlantısı WildFly ile yapılandırma hakkında daha fazla bilgi için bkz. [PostgreSQL](https://developer.jboss.org/blogs/amartin-blog/2012/02/08/how-to-set-up-a-postgresql-jdbc-driver-on-jboss-7), [MySQL](https://docs.jboss.org/jbossas/docs/Installation_And_Getting_Started_Guide/5/html/Using_other_Databases.html#Using_other_Databases-Using_MySQL_as_the_Default_DataSource), veya [SQL Server](https://docs.jboss.org/jbossas/docs/Installation_And_Getting_Started_Guide/5/html/Using_other_Databases.html#d0e3898).

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

Bir dış oturum deposu gibi kullanılacak WildFly yapılandırabileceğiniz [Azure önbelleği için Redis](/azure/azure-cache-for-redis/). Şunları yapmanız gerekir [mevcut ARR örnek benzeşimini devre dışı bırakma](https://azure.microsoft.com/blog/disabling-arrs-instance-affinity-in-windows-azure-web-sites/) oturum tanımlama bilgisi tabanlı yönlendirmesini devre dışı ve yapılandırılmış WildFly oturumu deposu girişim çalışmasına izin vermek için yapılandırma.

## <a name="docker-containers"></a>Docker kapsayıcıları

Azure tarafından desteklenen Zulu JDK kapsayıcılarınızı içinde kullanmak için çekme ve gelen belgelendiği gibi önceden oluşturulmuş görüntüleri kullanmayı emin olun [Azul Zulu Enterprise Azure indirme sayfası için desteklenen](https://www.azul.com/downloads/azure-only/zulu/) veya `Dockerfile` örneklerden[Microsoft Java GitHub deposu](https://github.com/Microsoft/java/tree/master/docker).

## <a name="statement-of-support"></a>Destek bildirimi

### <a name="runtime-availability"></a>Çalışma zamanı kullanılabilirlik

Linux için App Service, yönetilen Java web uygulamalarını barındırmak için iki çalışma zamanlarını destekler:

- [Tomcat servlet kapsayıcı](https://tomcat.apache.org/) paketlenmiş uygulamaları çalıştırmak için farklı web arşivi (WAR) dosyaları. Desteklenen sürümler şunlardır: 8,5 ve 9.0 sürümlerine ait.
- Uygulamaları çalıştırmak için Java SE çalışma zamanı ortamı (JAR) dosyaları Java arşiv paketlendi. Java 8 ve 11 desteklenen sürümleridir.

### <a name="jdk-versions-and-maintenance"></a>JDK sürümleri ve Bakım

OpenJDK olan Azul Zulu Enterprise yapılar OpenJDK Azure ve Microsoft ve Azul sistemleri tarafından desteklenen Azure Stack için ücretsiz, çok platformlu, üretime hazır bir dağıtımını ' dir. Bunlar, oluşturma ve Java SE uygulamaları çalıştırmak için tüm bileşenleri içerir. JDK'den yükleyebileceğiniz [Java JDK yükleme](https://aka.ms/azure-jdks).

Desteklenen JDK otomatik olarak üç aylık olarak Ocak, Nisan, Temmuz ve Ekim her yıl düzeltme eki.

### <a name="security-updates"></a>Güvenlik güncelleştirmeleri

Azul sistemlerden kullanılabilir olduklarında hemen sonra yamaları ve düzeltmeler önemli güvenlik açıkları için kullanıma sunulacaktır. "Büyük" bir güvenlik açığı 9.0 temel puanı ile tanımlanan ya da üzerinde daha büyük [NIST ortak güvenlik açığı Puanlama sistemi, sürüm 2](https://nvd.nist.gov/cvss.cfm).

### <a name="deprecation-and-retirement"></a>Kullanımdan kaldırma ve devre dışı bırakma

Desteklenen Java Çalışma zamanı kullanımdan kaldırılacak, çalışma zamanı kullanımdan önce en az altı ay etkilenen çalışma zamanı kullanan Azure geliştiricileri, kullanımdan kaldırma bildirimi verilir.

## <a name="next-steps"></a>Sonraki adımlar

Ziyaret [Java geliştiricileri için Azure](/java/azure/) Azure hızlı başlangıç kılavuzlarımız, öğreticilerimiz ve Java başvuru belgeleri bulmak için merkezi.

Java geliştirme belirli olmayan Linux için App Service'ı kullanma hakkında genel soruları [App Service Linux SSS](app-service-linux-faq.md).
