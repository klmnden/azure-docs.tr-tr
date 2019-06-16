---
title: Açık kaynak teknolojileri hakkında SSS - Azure App Service | Microsoft Docs
description: Azure App Service'in Web Apps özelliği, açık kaynak teknolojileri hakkında sık sorulan soruların yanıtlarını alın.
services: app-service\web
documentationcenter: ''
author: genlin
manager: cshepard
editor: ''
tags: top-support-issue
ms.assetid: 2fa5ee6b-51a6-4237-805f-518e6c57d11b
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 10/31/2018
ms.author: genli
ms.custom: seodec18
ms.openlocfilehash: 7831e5e989835b2c9432dbd61a242584a7b6244d
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61270211"
---
# <a name="open-source-technologies-faqs-for-web-apps-in-azure"></a>Açık kaynak teknolojilerini azure'daki Web uygulamaları için SSS

Bu makale için açık kaynak teknolojileri ile ilgili sorunlar hakkında sık sorulan sorular (SSS) yanıtlarını sahip [Azure App Service'in Web Apps özelliği](https://azure.microsoft.com/services/app-service/web/).

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="how-do-i-turn-on-php-logging-to-troubleshoot-php-issues"></a>PHP'nin PHP sorunlarını gidermek için günlüğü nasıl kapatırım?

PHP günlüğü etkinleştirmek için:

1. Oturum açın, [Kudu Web sitesi](https://*yourwebsitename*.scm.azurewebsites.net).
2. Üst menüde **hata ayıklama konsolunu** > **CMD**.
3. Seçin **Site** klasör.
4. Seçin **wwwroot** klasör.
5. Seçin **+** simgesine tıklayın ve ardından **yeni dosya**.
6. Dosya adı kümesine **. user.ini**.
7. Kalem simgesini seçin **. user.ini**.
8. Bu kod dosyasına ekleyin: `log_errors=on`
9. **Kaydet**’i seçin.
10. Kalem simgesini seçin **wp-config.php**.
11. Metin, aşağıdaki kodla değiştirin:
    ```php
    //Enable WP_DEBUG modedefine('WP_DEBUG', true);//Enable debug logging to /wp-content/debug.logdefine('WP_DEBUG_LOG', true);
    //Suppress errors and warnings to screendefine('WP_DEBUG_DISPLAY', false);//Suppress PHP errors to screenini_set('display_errors', 0);
    ```
12. Azure portalında web uygulama menüsünde, web uygulamanızı yeniden başlatın.

Daha fazla bilgi için [etkinleştirme WordPress Hata günlüklerini](https://blogs.msdn.microsoft.com/azureossds/2015/10/09/logging-php-errors-in-wordpress-2/).

## <a name="how-do-i-log-python-application-errors-in-apps-that-are-hosted-in-app-service"></a>App Service'te barındırılan uygulamalarda Python uygulama hatalarını nasıl oturum açarım?
[!INCLUDE [web-sites-python-troubleshooting-wsgi-error-log](../../includes/web-sites-python-troubleshooting-wsgi-error-log.md)]

## <a name="how-do-i-change-the-version-of-the-nodejs-application-that-is-hosted-in-app-service"></a>App Service'te barındırılan Node.js uygulamanın sürümünü nasıl değiştirebilirim?

Node.js uygulaması sürümünü değiştirmek için aşağıdaki seçeneklerden birini kullanabilirsiniz:

* Azure Portalı'nda **uygulama ayarları**.
  1. Azure portalında web uygulamanıza gidin.
  2. Üzerinde **ayarları** dikey penceresinde **uygulama ayarları**.
  3. İçinde **uygulama ayarları**, anahtar ve değer olarak istediğiniz Node.js sürümünü WEBSITE_NODE_DEFAULT_VERSION içerebilir.
  4. Git, [Kudu konsolunu](https://*yourwebsitename*.scm.azurewebsites.net).
  5. Node.js sürümü denetlemek için aşağıdaki komutu girin:  
     ```
     node -v
     ```
* İisnode.yml dosyasını değiştirin. Node.js sürümü iisnode.yml dosyasını değiştirerek yalnızca ayarlar çalışma zamanı ortamı bu iisnode kullanır. Uygulamanızın Kudu cmd ve diğerleri tarafından ayarlanmış olan Node.js sürümünü kullanmaya devam **uygulama ayarları** Azure portalında.

  İisnode.yml el ile ayarlamak için uygulama kök klasöründe bir iisnode.yml dosyası oluşturun. Dosyasında aşağıdaki satırı ekleyin:
  ```yml
  nodeProcessCommandLine: "D:\Program Files (x86)\nodejs\5.9.1\node.exe"
  ```
   
* Kaynak denetimi dağıtım sırasında package.json kullanarak iisnode.yml dosyasını ayarlayın.
  Azure kaynak denetimi dağıtım işlemi aşağıdaki adımları içerir:
  1. Azure web uygulamasına içerik taşınır.
  2. Hiç yoksa (deploy.cmd, .deployment dosyaları) web uygulaması kök klasöründe varsayılan dağıtım betiğini oluşturur.
  3. Hangi oluşturduğu bir iisnode.yml dosyası Node.js sürümünü package.json dosyasına bahsetmeniz durumunda bir dağıtım betiği çalıştıran > altyapısı `"engines": {"node": "5.9.1","npm": "3.7.3"}`
  4. Aşağıdaki kod satırını iisnode.yml dosyasını sahiptir:
      ```yml
      nodeProcessCommandLine: "D:\Program Files (x86)\nodejs\5.9.1\node.exe"
      ```

## <a name="i-see-the-message-error-establishing-a-database-connection-in-my-wordpress-app-thats-hosted-in-app-service-how-do-i-troubleshoot-this"></a>App Service'te barındırılan WordPress uygulamamı de "veritabanı bağlantısı kurulurken hata" iletisini görüyorum. Bu nasıl giderebilirim?

Azure WordPress uygulamanızı bu hatayı görürseniz, php_errors.log ve debug.log, etkinleştirmek için tüm adımları ayrıntılı [etkinleştirme WordPress Hata günlüklerini](https://blogs.msdn.microsoft.com/azureossds/2015/10/09/logging-php-errors-in-wordpress-2/).

Günlükleri etkinleştirildiğinde, hatayı yeniden oluşturmaya ve ardından bağlantılar dışında çalışıp çalışmadığını görmek için günlüklere bakın:
```
[09-Oct-2015 00:03:13 UTC] PHP Warning: mysqli_real_connect(): (HY000/1226): User ‘abcdefghijk79' has exceeded the ‘max_user_connections’ resource (current value: 4) in D:\home\site\wwwroot\wp-includes\wp-db.php on line 1454
```

Debug.log veya php_errors.log dosyalarınızda bu hatayı görürseniz, uygulamanızın bağlantı sayısını aşıyor. ClearDB üzerinde barındırıyorsanız, kullanılabilen bağlantı sayısını doğrulayın, [hizmet planı](https://www.cleardb.com/pricing.view).

## <a name="how-do-i-debug-a-nodejs-app-thats-hosted-in-app-service"></a>App Service'ta barındırılan bir Node.js uygulamasını nasıl hata ayıklama?

1.  Git, [Kudu konsolunu](https://*yourwebsitename*.scm.azurewebsites.net/DebugConsole).
2.  Uygulama günlükleri klasörünüze (D:\home\LogFiles\Application) gidin.
3.  Logging_errors.txt dosyanın içeriğini kontrol edin.

## <a name="how-do-i-install-native-python-modules-in-an-app-service-web-app-or-api-app"></a>Bir App Service web uygulaması veya API uygulaması yerel Python modüllerini nasıl yüklerim?

Bazı paketler pip Azure'da kullanarak yüklemeyebilir. Paket Python paket dizini kullanılamıyor olabilir veya bir derleyici gerekli olabilir (Derleyici, App Service'te web uygulaması çalıştıran bir bilgisayarda kullanılabilir değildir). App Service web apps ve API uygulamalarında yerel modülleri yükleme hakkında daha fazla bilgi için bkz: [App Service yüklemek Python modüllerde](https://blogs.msdn.microsoft.com/azureossds/2015/06/29/install-native-python-modules-on-azure-web-apps-api-apps/).

## <a name="how-do-i-deploy-a-django-app-to-app-service-by-using-git-and-the-new-version-of-python"></a>Nasıl bir Django uygulaması App Service'e Git ve yeni Python sürümünü kullanarak dağıtabilirim?

Django yükleme hakkında daha fazla bilgi için bkz: [bir Django uygulaması App Service'e dağıtma](https://blogs.msdn.microsoft.com/azureossds/2016/08/25/deploying-django-app-to-azure-app-services-using-git-and-new-version-of-python/).

## <a name="where-are-the-tomcat-log-files-located"></a>Tomcat günlük dosyalarını nerede bulunuyor?

Azure Market ve özel dağıtımlar için:

* Klasör konumu: D:\home\site\wwwroot\bin\apache-tomcat-8.0.33\logs
* İlgilendiğiniz dosyaları:
    * catalina. *yyyy-aa-gg*.log
    * ana bilgisayar yöneticisi. *yyyy-aa-gg*.log
    * localhost. *yyyy-aa-gg*.log
    * Yöneticisi. *yyyy-aa-gg*.log
    * site_access_log. *yyyy-aa-gg*.log


Portal **uygulama ayarları** dağıtımları:

* Klasör konumu: D:\home\LogFiles
* İlgilendiğiniz dosyaları:
    * catalina. *yyyy-aa-gg*.log
    * ana bilgisayar yöneticisi. *yyyy-aa-gg*.log
    * localhost. *yyyy-aa-gg*.log
    * Yöneticisi. *yyyy-aa-gg*.log
    * site_access_log. *yyyy-aa-gg*.log

## <a name="how-do-i-troubleshoot-jdbc-driver-connection-errors"></a>JDBC sürücüsü bağlantı hatalarını nasıl giderebilirim?

Tomcat günlükleri şu iletiyi görebilirsiniz:

```
The web application[ROOT] registered the JDBC driver [com.mysql.jdbc.Driver] but failed to unregister it when the web application was stopped. To prevent a memory leak,the JDBC Driver has been forcibly unregistered
```

Hatayı gidermek için:

1. Uygulama/lib klasörünüzden sqljdbc*.jar dosyayı kaldırın.
2. Özel Tomcat veya Azure Market Tomcat web sunucusu kullanıyorsanız, bu .jar dosyasını Tomcat lib klasörüne kopyalayın.
3. Azure portalından Java etkinleştiriyorsanız (seçin **Java 1.8** > **Tomcat sunucusu**), uygulamanıza paralel klasörde sqljdbc.* jar dosyasını kopyalayın. Ardından, aşağıdaki sınıf ayarları web.config dosyasına ekleyin:

    ```xml
    <httpPlatform>
    <environmentVariables>
    <environmentVariablename ="JAVA_OPTS" value=" -Djava.net.preferIPv4Stack=true
    -Xms128M -classpath %CLASSPATH%;[Path to the sqljdbc*.jarfile]" />
    </environmentVariables>
    </httpPlatform>
    ```

## <a name="why-do-i-see-errors-when-i-attempt-to-copy-live-log-files"></a>Dinamik günlük dosyalarını kopyalamak denediğinizde hataları neden görüyorum?

Bir Java uygulaması (örneğin, Tomcat) için dinamik günlük dosyalarını kopyalamak çalışırsanız, bu FTP hatayı görebilirsiniz:

```
Error transferring file [filename] Copying files from remote side failed.
    
The process cannot access the file because it is being used by another process.
```

Hata iletisi, FTP istemcisi bağlı olarak değişebilir.

Tüm Java uygulamaları bu bir kilitleme sorunu var. Kudu yalnızca uygulama çalışırken, bu dosyayı indirmeyi destekler.

Uygulamayı durdurma, bu dosyalara FTP erişim sağlar.

Başka bir geçici çözüm, bu dosyaları farklı bir dizine kopyalar ve bir zamanlamaya göre çalışan bir WebJob yazmaktır. Örnek proje için bkz: [CopyLogsJob](https://github.com/kamilsykora/CopyLogsJob) proje.

## <a name="where-do-i-find-the-log-files-for-jetty"></a>Jetty için günlük dosyalarını nerede bulabilirim?

Market ve özel dağıtımlar için günlük dosyasına D:\home\site\wwwroot\bin\jetty-distribution-9.1.2.v20140210\logs klasöründe bulunur. Klasör konumunu kullanmakta olduğunuz Jetty sürümünde bağlı olduğunu unutmayın. Örneğin, yol burada 9.1.2 Jetty için sağlanır. Aramak için jetty_*YYYY_MM_DD*. stderrout.log.

Portal uygulama ayarı dağıtımları için D:\home\LogFiles günlük dosyasıdır. Aramak için jetty_*YYYY_MM_DD*. stderrout.log

## <a name="can-i-send-email-from-my-azure-web-app"></a>My Azure web uygulamasından e-posta gönderebilir miyim?

App Service, yerleşik e-posta özelliği yoktur. Uygulamanızdan e-posta göndermek için bazı iyi seçenekler görmek için bu [Stack Overflow tartışma](https://stackoverflow.com/questions/17666161/sending-email-from-azure).

## <a name="why-does-my-wordpress-site-redirect-to-another-url"></a>WordPress sitemi başka bir URL'ye neden yönlendiriyor mu?

Azure'da kısa süre önce yaptıysanız, WordPress için eski etki alanı URL'si yönlendirebilir. Bu MySQL veritabanı ayarı nedeniyle oluşur.

WordPress arkadaş + yeniden yönlendirme URL'sini doğrudan veritabanında güncelleştirmek için kullanabileceğiniz bir Azure Site uzantısı'dır. WordPress arkadaş + kullanma hakkında daha fazla bilgi için bkz. [WordPress araçları ve geçiş MySQL ile WordPress arkadaş +](https://blogs.msdn.microsoft.com/azureossds/2016/12/21/wordpress-tools-and-mysql-migration-with-wordpress-buddy/).

Alternatif olarak, el ile yeniden yönlendirmeyi güncelleştirmek tercih ettiğiniz SQL sorguları veya PHPMyAdmin, kullanarak URL'sini görmek [WordPress: Yanlış URL'ye yeniden yönlendirerek](https://blogs.msdn.microsoft.com/azureossds/2016/07/12/wordpress-redirecting-to-wrong-url/).

## <a name="how-do-i-change-my-wordpress-sign-in-password"></a>Oturum açma WordPress parolamı nasıl değiştirebilirim?

WordPress oturum açma parolanızı unuttuysanız, WordPress arkadaş + güncelleştirmek için kullanabilirsiniz. Parolanızı sıfırlamak için WordPress arkadaş + Azure Site uzantısını yükleyin ve ardından açıklanan bu adımları tamamlayın [WordPress araçları ve geçiş MySQL ile WordPress arkadaş +](https://blogs.msdn.microsoft.com/azureossds/2016/12/21/wordpress-tools-and-mysql-migration-with-wordpress-buddy/).

## <a name="i-cant-sign-in-to-wordpress-how-do-i-resolve-this"></a>WordPress için oturum açamıyorum. Bu nasıl giderebilirim?

Kendinize yakın zamanda bir eklenti yükledikten sonra WordPress dışında kilitli görürseniz, hatalı bir eklenti olabilir. WordPress arkadaş + yardımcı olabilecek bir Azure Site uzantısı, WordPress eklentileri devre dışı bırak'dır. Daha fazla bilgi için [WordPress araçları ve geçiş MySQL ile WordPress arkadaş +](https://blogs.msdn.microsoft.com/azureossds/2016/12/21/wordpress-tools-and-mysql-migration-with-wordpress-buddy/).

## <a name="how-do-i-migrate-my-wordpress-database"></a>WordPress veritabanımı nasıl geçirebilirim?

WordPress sitenize bağlı MySQL veritabanını geçirme için birçok seçeneğiniz vardır:

* Geliştiriciler: Kullanım [komut istemi veya PHPMyAdmin](https://blogs.msdn.microsoft.com/azureossds/2016/03/02/migrating-data-between-mysql-databases-using-kudu-console-azure-app-service/)
* Non-geliştiricileri: Kullanım [WordPress arkadaş +](https://blogs.msdn.microsoft.com/azureossds/2016/12/21/wordpress-tools-and-mysql-migration-with-wordpress-buddy/)

## <a name="how-do-i-help-make-wordpress-more-secure"></a>WordPress daha güvenli hale getirmenize nasıl yardımcı?

WordPress için en iyi güvenlik uygulamaları hakkında bilgi edinmek için [WordPress güvenliği için en iyi yöntemler](https://blogs.msdn.microsoft.com/azureossds/2016/12/26/best-practices-for-wordpress-security-on-azure/).

## <a name="i-am-trying-to-use-phpmyadmin-and-i-see-the-message-access-denied-how-do-i-resolve-this"></a>PHPMyAdmin kullanılacak getirmeye çalışıyorum ve "Erişim engellendi." iletisini görüyorum Bu nasıl giderebilirim?

MySQL uygulama içi özelliği bu App Service örneğinde henüz çalışmıyorsa bu sorunla karşılaşabilirsiniz. Bu sorunu çözmek için Web sitenize güvenle erişin deneyin. Bu MySQL uygulama içi işlemi dahil olmak üzere gerekli işlemleri başlatır. Uygulama içi MySQL, işlem Gezgini içinde çalıştığını doğrulamak için bu mysqld.exe işlemlerinde listelendiğinden emin olun.

Bu MySQL uygulama içi çalıştığından emin olduktan sonra PHPMyAdmin kullanmayı deneyin.

## <a name="i-get-an-http-403-error-when-i-try-to-import-or-export-my-mysql-in-app-database-by-using-phpmyadmin-how-do-i-resolve-this"></a>İçeri veya PHPMyadmin aracılığıyla MySQL uygulama içi veritabanını dışarı aktarma çalıştığınızda bir HTTP 403 hatası alıyorum. Bu nasıl giderebilirim?

Chrome daha eski bir sürümünü kullanıyorsanız, bilinen bir hatayı yaşıyor olabilir. Bu sorunu çözmek için Chrome, yeni bir sürüme yükseltin. Ayrıca, Internet Explorer veya Microsoft Edge, nerede sorun oluşmaz gibi farklı bir tarayıcı kullanmayı deneyin.
