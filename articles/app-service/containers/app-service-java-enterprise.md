---
title: Linux - Azure App Service üzerinde Java Enterprise desteği | Microsoft Docs
description: Linux üzerinde Azure App Service ile Wildfly kullanarak Java Kurumsal uygulamaları dağıtmak için Geliştirici Kılavuzu.
keywords: Azure app service, web uygulaması, linux, oss, java, wildfly, enterprise, java ee jee, javaee
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
ms.custom: seodec18
ms.openlocfilehash: 8db65fd9a1f271aea4ceb345f4d9dfbb6b9ff8a6
ms.sourcegitcommit: a60a55278f645f5d6cda95bcf9895441ade04629
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2019
ms.locfileid: "58877389"
---
# <a name="java-enterprise-guide-for-app-service-on-linux"></a>Linux'ta App Service için Java Enterprise Kılavuzu

> [!NOTE] 
> App Service Linux üzerinde Java Enterprise Edition, şu anda Önizleme aşamasındadır. Bu yığın **değil** üretim yönelik iş için önerilir. Lütfen [Java Geliştirici Kılavuzu](app-service-linux-java.md) bizim Java SE ve Tomcat yığınları hakkında bilgi için.

Linux üzerinde Azure App Service'te Java geliştiricilerinin oluşturmanızı, dağıtmanızı ve Linux tabanlı tam olarak yönetilen bir hizmet üzerinde Java Enterprise (Java EE) uygulama ölçeklendirme sağlar.  Temel Kurumsal Java Çalışma zamanı ortamı olan açık kaynaklı [Wildfly](https://wildfly.org/) uygulama sunucusu.

Bu kılavuzu temel kavramları ve Linux için App Service kullanarak kurumsal Java geliştiricilerine yönelik yönergeler sağlar. Linux için Azure App Service ile Java uygulamalarını hiçbir zaman dağıttıysanız, tamamlamanız gereken [Java Hızlı Başlangıç](quickstart-java.md) ilk. Linux için App Service için Java Enterprise özgü olmayan soruları yanıtlanır [Java Geliştirici Kılavuzu](app-service-linux-java.md) ve [App Service Linux SSS](app-service-linux-faq.md).

## <a name="scale-with-app-service"></a>App Service ile ölçeklendirme 

Linux üzerinde App Service'te çalışan WildFly uygulama sunucusu bir etki alanı yapılandırmasında tek başına modunda çalışır. App Service planı ölçeğini daralttığınızda her WildFly örnek bir tek başına sunucu olarak yapılandırılır.

 Uygulamanız ile yatay veya dikey olarak ölçeklendirme [ölçeklendirme kuralları](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-autoscale-get-started?toc=%2Fazure%2Fapp-service%2Fcontainers%2Ftoc.json) ve [, örnek sayısını artırıyorsanız](https://docs.microsoft.com/azure/app-service/web-sites-scale?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json). 

## <a name="customize-application-server-configuration"></a>Uygulama sunucusu yapılandırması özelleştirme

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

Başlangıç betiği için karşıya yükleme `/home/site/deployments/tools` App Service Örneğinizdeki. Bkz: [bu belgeyi](/azure/app-service/deploy-configure-credentials#userscope) FTP kimlik bilgilerinizi alma hakkında yönergeler için. 

Ayarlama **başlangıç betiği** Azure portalında, başlangıç Kabuk betiği konumunu Örneğin alan `/home/site/deployments/tools/your-startup-script.sh`.

Tedarik [uygulama ayarları](/azure/app-service/web-sites-configure#application-settings) kullanmak için ortam değişkenlerini betiğe geçirmek için uygulama yapılandırması. Uygulama ayarları, bağlantı dizeleri ve sürüm denetimi dışında Uygulamanızı yapılandırmak için gerekli diğer gizli dizileri koruyun.

## <a name="modules-and-dependencies"></a>Modüller ve bağımlılıkları

JBoss CLI aracılığıyla Wildfly sınıf içine modülleri ve bağımlılıklarını yüklemek için kendi dizininde aşağıdaki dosyalar oluşturmanız gerekir.  Çoğu durumda bir bağımlılık yapılandırmak gerekenler, minimum düzeyde bu liste, bu nedenle bazı modüller ve bağımlılıkları JNDI adlandırma gibi ek yapılandırma veya başka bir API özel yapılandırma gerekebilir.

- Bir [XML modülü tanımlayıcısı](https://jboss-modules.github.io/jboss-modules/manual/#descriptors). Bu XML dosya adı, öznitelikler ve bağımlılıkları modülünüzün tanımlar. Bu [örnek module.xml dosyası](https://access.redhat.com/documentation/en-us/jboss_enterprise_application_platform/6/html/administration_and_configuration_guide/example_postgresql_xa_datasource) Postgres modülü, JAR dosyasını JDBC bağımlılığı ve gerekli diğer modül bağımlılıklarının tanımlar.
- Gerekli JAR dosyası için tüm bağımlılıkların modülünüzde.
- Yeni modül yapılandırmak için JBoss CLI komutları ile bir komut dosyası. Bu dosya sunucusunu bağımlılık kullanacak şekilde yapılandırmak için JBoss CLI tarafından yürütülecek komutlarınızı içerir. Modüller, veri kaynakları ve mesajlaşma sağlayıcıları eklemek için komutlara ilişkin belgeleri için başvurmak [bu belgeyi](https://access.redhat.com/documentation/red_hat_jboss_enterprise_application_platform/7.0/html-single/management_cli_guide/#how_to_cli).
-  JBoss CLI'yı arayın ve önceki adımda komut yürütmek için bir Bash başlangıç betiği. Bu dosya, App Service örneğinizin yeniden başlatıldığında veya ölçek genişletme sırasında yeni örnekleri sağlandığında yürütülür.  Bu başlangıç betiği JBoss komutları JBoss CLI için geçirilen diğer tüm yapılandırmaları, uygulamanız için yapabileceğiniz gösterilmiştir. En azından, bu dosya için JBoss CLI JBoss CLI komut geçirilecek tek bir komut olabilir: 
   
```bash
`/opt/jboss/wildfly/bin/jboss-cli.sh -c --file=/path/to/your/jboss_commands.cli` 
``` 

Modülünüzün için içeriği ve dosyaları aldıktan sonra modülün Wildfly uygulama sunucusuna eklemek için aşağıdaki adımları izleyin. 

1. Dosyalarınıza FTP `/home/site/deployments/tools` App Service Örneğinizdeki. Yönergeler için bu belgede, FTP kimlik bilgilerinizi Forms'dan bakın. 
2. Örneğin, başlangıç Kabuk betiği konumuna Azure portal'ın uygulama ayarları dikey penceresinde "Başlangıç betiği" alanını ayarlayın `/home/site/deployments/tools/your-startup-script.sh` .
3. App Service örneğinizin tuşlarına basarak yeniden **yeniden** düğmesine **genel bakış** bölümü portalı veya Azure CLI kullanarak.

## <a name="data-sources"></a>Veri kaynakları

Wildfly için veri kaynağı bağlantısı yapılandırmak için yukarıdaki modülleri yükleme ve bağımlılıkları bölümünde ana hatlarıyla aynı süreci izleyin. Herhangi bir Azure veritabanı hizmeti için aynı adımları izleyebilirsiniz.

1. Veritabanı flavor için JDBC sürücüsünü yükleyin. Kolaylık olması için sürücüleri şunlardır [Postgres](https://jdbc.postgresql.org/download.html) ve [MySQL](https://dev.mysql.com/downloads/connector/j/). .Jar dosyasını almak için indirme ayıklayın.
2. "Modülleri ve oluşturup XML modülü tanımlayıcısı, JBoss CLI betiği, başlangıç betiği ve JDBC .jar bağımlılık karşıya bağımlılıklar" adımları anahattı izleyin.


Wildfly ile yapılandırma hakkında daha fazla bilgi [PostgreSQL](https://developer.jboss.org/blogs/amartin-blog/2012/02/08/how-to-set-up-a-postgresql-jdbc-driver-on-jboss-7) , [MySQL](https://docs.jboss.org/jbossas/docs/Installation_And_Getting_Started_Guide/5/html/Using_other_Databases.html#Using_other_Databases-Using_MySQL_as_the_Default_DataSource), ve [SQL veritabanı](https://docs.jboss.org/jbossas/docs/Installation_And_Getting_Started_Guide/5/html/Using_other_Databases.html#d0e3898) kullanılabilir. Veri kaynağı tanımlarının sunucunuza eklemek için bu özelleştirilmiş yönergeler genelleştirilmiş bir yaklaşım ile birlikte kullanabilirsiniz.

## <a name="messaging-providers"></a>Mesajlaşma sağlayıcıları

Service Bus Mesajlaşma mekanizması olarak kullanan temelli ileti Fasulye etkinleştirmek için:

1. Kullanım [Apache QPId JMS Mesajlaşma kitaplığına](https://qpid.apache.org/proton/index.html). Bu bağımlılık pom.xml (veya diğer derleme dosyası), uygulamayı içerir.

2.  Oluşturma [hizmet veri yolu kaynaklarını](/azure/service-bus-messaging/service-bus-java-how-to-use-jms-api-amqp). Gönderme ile bir Azure Service Bus ad alanı ve sıra içinde bu ad alanı ve bir paylaşılan erişim ilkesi oluşturun ve özelliklerini alır.

3. Paylaşılan Erişim İlkesi anahtarı kodunuza URL kodlaması ile geçirin ilkenizin birincil anahtar veya [hizmet veri yolu SDK'yı](/azure/service-bus-messaging/service-bus-java-how-to-use-jms-api-amqp#setup-jndi-context-and-configure-the-connectionfactory).

4. JMS sağlayıcı modülü XML tanımlayıcısı, .jar bağımlılıkları, JBoss CLI komutları ve başlangıç betiği ile yükleme modüller ve bağımlılıkları bölümünde ana hatlarıyla belirtilen adımları izleyin. Dört dosyalara ek olarak JMS kuyruk ve konu JNDI adını tanımlayan bir XML dosyası oluşturmanız gerekir. Bkz: [bu depo](https://github.com/JasonFreeberg/widlfly-server-configs/tree/master/appconfig) başvuru yapılandırma dosyaları için.


## <a name="configure-session-management-caching"></a>Yönetim oturumu önbelleğe almayı yapılandır

Varsayılan olarak, istemci istekleri oturumlarına ile yönlendirildiğinden emin olmak için oturum benzeşimi tanımlama Linux üzerinde App Service'te kullanır, uygulamanızın'ın aynı örneğine. Bu varsayılan davranışı, işlem yapılandırma gerektirmez, ancak bazı sınırlamaları vardır:

- Bir uygulama örneği yeniden başlatıldı veya ölçeklendirilebilir, uygulama sunucusu kullanıcı oturumu durumunda kaybolur.
- Uygulamalarınız için uzun oturum zaman aşımı ayarları veya sabit birkaç kullanıcıya varsa, yalnızca yeni oturumlar yeni başlatılan örneklerine yönlendirilir olduğundan yük almak için yeni örnekleri autoscaled biraz zaman alabilir.

Bir dış oturum deposu gibi kullanılacak Wildfly yapılandırabileceğiniz [Azure önbelleği için Redis](/azure/azure-cache-for-redis/). Şunları yapmanız gerekir [mevcut ARR örnek benzeşimini devre dışı bırakma](https://azure.microsoft.com/blog/disabling-arrs-instance-affinity-in-windows-azure-web-sites/) oturum tanımlama bilgisi tabanlı yönlendirmesini devre dışı ve yapılandırılmış Wildfly oturumu deposu girişim çalışmasına izin vermek için yapılandırma.

## <a name="enable-web-sockets"></a>Web yuvaları etkinleştir

Varsayılan olarak, App Service'te web yuvalarını etkinleştirilir. Uygulamanızda WebSockets ile çalışmaya başlamak için başvurmak [Bu hızlı başlangıçta](https://github.com/wildfly/quickstart/tree/master/websocket-hello).

## <a name="logs-and-troubleshooting"></a>Günlükleri ve sorun giderme

App Service, uygulamanızdaki sorunları gidermenize yardımcı olacak araçlar sağlar.

-   Tıklayarak günlük özelliğini açar **tanılama günlükleri** sol taraftaki gezinti bölmesinde. Tıklayın **dosya sistemi** depolama kotası ve Bekletme dönemi ayarlayın ve değişikliklerinizi kaydedin. Bu günlükleri altında bulabilirsiniz `/home/LogFiles/`.
-   [Uygulama örneğine bağlanmak için SSH kullanın](app-service-linux-ssh-support.md) uygulamaları çalıştırmaya yönelik günlükleri görüntülemek için.
-   Onay tanılama günlükleri **tanılama günlükleri** paneli Portal veya Azure CLI komutunu kullanarak:
`az webapp log tail --name <your-app-name> --resource-group <your-apps-resource-group>`
