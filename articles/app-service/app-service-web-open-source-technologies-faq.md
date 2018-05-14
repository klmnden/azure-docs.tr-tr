---
title: Açık kaynak teknolojileri SSS Azure web uygulamaları | Microsoft Docs
description: Azure App Service Web Apps özelliğini açık kaynak teknolojileri hakkında sık sorulan soruların yanıtlarını alın.
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
ms.date: 05/11/2018
ms.author: genli
ms.openlocfilehash: 747ee61d2620e7f79353207c0e44bcea36df30ee
ms.sourcegitcommit: c52123364e2ba086722bc860f2972642115316ef
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/11/2018
---
# <a name="open-source-technologies-faqs-for-web-apps-in-azure"></a>Açık kaynak teknolojileri Azure Web uygulamaları için sık sorulan sorular

Bu makale için açık kaynak teknolojileri ile ilgili sorunlar hakkında sık sorulan sorular (SSS) yanıtlarını sahip [Azure App Service Web Apps özelliğini](https://azure.microsoft.com/services/app-service/web/).

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="my-cleardb-database-is-down-how-do-i-resolve-this"></a>ClearDB veritabanı kullanılamıyor. Bu nasıl giderebilirim?

Veritabanı ile ilgili sorunlar için başvurun [ClearDB Destek](https://www.cleardb.com/developers/help/support). 

ClearDB hakkında sık sorulan soruların yanıtları için bkz: [ClearDB SSS](https://docs.microsoft.com/azure/store-cleardb-faq/).

## <a name="why-wasnt-my-cleardb-database-migrated-during-my-subscription-migration"></a>Neden ClearDB veritabanı my abonelik geçişi sırasında geçirilen değildi?

Abonelikler arasında kaynak geçiş gerçekleştirdiğinizde, bazı sınırlamalar uygulanır. ClearDB MySQL veritabanı bir üçüncü taraf hizmeti ve Azure abonelik geçişi sırasında geçirilmez.

Azure kaynaklarınızı geçirmeden önce MySQL veritabanınızı geçişini yönetmiyorsanız ClearDB MySQL veritabanı kullanılamıyor olabilir. Bunu önlemek için ilk olarak, el ile ClearDB veritabanınızı geçirin ve sonra web uygulamanız için Azure aboneliği Taşı.

Daha fazla bilgi için bkz: [ClearDB MySQL veritabanları için SSS Azure uygulama hizmeti ile](https://docs.microsoft.com/azure/store-cleardb-faq/).

## <a name="how-do-i-turn-on-php-logging-to-troubleshoot-php-issues"></a>Üzerinde PHP sorunları gidermek için günlüğü PHP nasıl kapatırım?

PHP günlüğünü etkinleştirmek için:

1. Oturum açın, [Kudu Web sitesi](https://*yourwebsitename*.scm.azurewebsites.net).
2. Üst menüde seçin **hata ayıklama Konsolu'nda** > **CMD**.
3. Seçin **Site** klasör.
4. Seçin **wwwroot** klasör.
5. Seçin **+** simgesine ve ardından **yeni dosya**.
6. Dosya adı ayarlamak **. user.ini**.
7. Kalem simgesini seçin **. user.ini**.
8. Dosyasında bu kodu ekleyin: `log_errors=on`
9. **Kaydet**’i seçin.
10. Kalem simgesini seçin **wp-config.php**.
11. Metin aşağıdaki kodla değiştirin:
   ```
   //Enable WP_DEBUG modedefine('WP_DEBUG', true);//Enable debug logging to /wp-content/debug.logdefine('WP_DEBUG_LOG', true);
   //Supress errors and warnings to screendefine('WP_DEBUG_DISPLAY', false);//Supress PHP errors to screenini_set('display_errors', 0);
   ```
12. Azure portalında web uygulama menüsünde, web uygulamanızı yeniden başlatın.

Daha fazla bilgi için bkz: [etkinleştirmek WordPress Hata günlüklerini](https://blogs.msdn.microsoft.com/azureossds/2015/10/09/logging-php-errors-in-wordpress-2/).

## <a name="how-do-i-log-python-application-errors-in-apps-that-are-hosted-in-app-service"></a>App Service içinde barındırılan uygulamalar, Python uygulama hatalarını nasıl oturum?

Python uygulama hatalarını yakalamak için:

1. Azure portalında web uygulamanızı seçin **ayarları**.
2. Üzerinde **ayarları** sekmesine **uygulama ayarları**.
3. Altında **uygulama ayarları**, aşağıdaki anahtar/değer çifti girin:
    * Anahtar: WSGI_LOG
    * Değer: D:\home\site\wwwroot\logs.txt (dosya adı seçiminizi girin)

Şimdi wwwroot klasörüne logs.txt dosyasındaki hatalar görmeniz gerekir.

## <a name="how-do-i-change-the-version-of-the-nodejs-application-that-is-hosted-in-app-service"></a>App Service içinde barındırılan Node.js uygulaması sürümü nasıl değişiyor?

Node.js uygulaması sürümünü değiştirmek için aşağıdaki seçeneklerden birini kullanabilirsiniz:

*   Azure portalında kullanmak **uygulama ayarları**.
    1. Azure portalında web uygulamanıza gidin.
    2. Üzerinde **ayarları** dikey penceresinde, select **uygulama ayarları**.
    3. İçinde **uygulama ayarları**, anahtar ve değer olarak istediğiniz Node.js sürümünü olarak WEBSITE_NODE_DEFAULT_VERSION içerebilir.
    4. Gidin, [Kudu konsol](https://*yourwebsitename*.scm.azurewebsites.net).
    5. Node.js sürümünü denetlemek için aşağıdaki komutu girin:  
   ```
   node -v
   ```
*   İisnode.yml dosyasını değiştirin. İisnode.yml dosyasını Node.js sürümünü değiştirme yalnızca ayarlar çalışma zamanı ortamı bu iisnode kullanır. Kudu cmd ve diğerleri kümesinde Node.js sürümünü kullanmaya devam **uygulama ayarları** Azure portalında.

    İisnode.yml el ile ayarlamak için uygulama kök klasöründe bir iisnode.yml dosyasını oluşturun. Dosyasında aşağıdaki satırı ekleyin:
   ```
   nodeProcessCommandLine: "D:\Program Files (x86)\nodejs\5.9.1\node.exe"
   ```
   
*   Kaynak denetimi dağıtımı sırasında package.json kullanarak iisnode.yml dosyasını ayarlayın.
    Azure kaynak denetimi dağıtım işlemi aşağıdaki adımları içerir:
    1. İçerik Azure web uygulaması'na taşınır.
    2. Hiç yoksa (deploy.cmd, .deployment dosyaları) web uygulaması kök klasöründe bir varsayılan dağıtım betiğini oluşturur.
    3. Hangi oluşturduğu bir iisnode.yml dosyasını package.json dosyası Node.js sürümünü Bahsediyor, bir dağıtım betiği çalıştıran > altyapısı `"engines": {"node": "5.9.1","npm": "3.7.3"}`
    4. Aşağıdaki kod satırını iisnode.yml dosyasını sahiptir:
        ```
        nodeProcessCommandLine: "D:\Program Files (x86)\nodejs\5.9.1\node.exe"
        ```

## <a name="i-see-the-message-error-establishing-a-database-connection-in-my-wordpress-app-thats-hosted-in-app-service-how-do-i-troubleshoot-this"></a>App Service içinde barındırılan my WordPress uygulamasında "veritabanı bağlantısı kurma hatası" iletisini görüyorum. Bu nasıl giderebilirim?

Azure WordPress uygulamanız bu hatayı görürseniz, php_errors.log ve debug.log, etkinleştirmek için tam adımları ayrıntılı [etkinleştirmek WordPress Hata günlüklerini](https://blogs.msdn.microsoft.com/azureossds/2015/10/09/logging-php-errors-in-wordpress-2/).

Günlükleri etkinleştirildiğinde, hatayı yeniden oluşturmaya ve dışı bağlantılar çalışıp çalışmadığını görmek için günlüklere bakın:
```
[09-Oct-2015 00:03:13 UTC] PHP Warning: mysqli_real_connect(): (HY000/1226): User ‘abcdefghijk79' has exceeded the ‘max_user_connections’ resource (current value: 4) in D:\home\site\wwwroot\wp-includes\wp-db.php on line 1454
```

Debug.log veya php_errors.log dosyalarınızı bu hatayı görürseniz, uygulamanızı bağlantı sayısını aşıyor. ClearDB üzerinde koyduysanız kullanılabilir olan bağlantı sayısı doğrulayın, [hizmet planı](https://www.cleardb.com/pricing.view).

## <a name="how-do-i-debug-a-nodejs-app-thats-hosted-in-app-service"></a>App Service içinde barındırılan bir Node.js uygulaması nasıl hata?

1.  Gidin, [Kudu konsol](https://*yourwebsitename*.scm.azurewebsites.net/DebugConsole).
2.  Uygulama günlükleri klasörünüze (D:\home\LogFiles\Application) gidin.
3.  Logging_errors.txt dosyasında içerik için denetleyin.

## <a name="how-do-i-install-native-python-modules-in-an-app-service-web-app-or-api-app"></a>Yerel Python modüllerini nasıl bir App Service web uygulaması veya API uygulaması yükleme?

Bazı paketler, Azure'da PIP kullanarak yüklemeyebilir. Paket Python paket Dizini'nde olmayabilir veya bir derleyici gerekli olabilir (derleyici App Service'te web uygulaması çalıştıran bilgisayarda kullanılabilir değildir). App Service web apps ve API apps yerel modülleri yükleme hakkında daha fazla bilgi için bkz: [uygulama hizmeti yükleme Python modülleri](https://blogs.msdn.microsoft.com/azureossds/2015/06/29/install-native-python-modules-on-azure-web-apps-api-apps/).

## <a name="how-do-i-deploy-a-django-app-to-app-service-by-using-git-and-the-new-version-of-python"></a>Nasıl bir Django uygulaması App Service'e Git ve Python yeni sürümünü kullanarak dağıtırım?

Django yükleme hakkında daha fazla bilgi için bkz: [bir Django uygulaması App Service'e dağıtma](https://blogs.msdn.microsoft.com/azureossds/2016/08/25/deploying-django-app-to-azure-app-services-using-git-and-new-version-of-python/).

## <a name="where-are-the-tomcat-log-files-located"></a>Tomcat günlük dosyalarının bulunduğu?

Azure Market ve özel dağıtımlar için:

* Klasör konumu: D:\home\site\wwwroot\bin\apache-tomcat-8.0.33\logs
* İlgilenilen dosyalar:
    * catalina. *yyyy-aa-gg*.log
    * ana bilgisayar-yöneticisi. *yyyy-aa-gg*.log
    * localhost. *yyyy-aa-gg*.log
    * Yöneticisi. *yyyy-aa-gg*.log
    * site_access_log. *yyyy-aa-gg*.log


Portal için **uygulama ayarları** dağıtımlar:

* Klasör konumu: D:\home\LogFiles
* İlgilenilen dosyalar:
    * catalina. *yyyy-aa-gg*.log
    * ana bilgisayar-yöneticisi. *yyyy-aa-gg*.log
    * localhost. *yyyy-aa-gg*.log
    * Yöneticisi. *yyyy-aa-gg*.log
    * site_access_log. *yyyy-aa-gg*.log

## <a name="how-do-i-troubleshoot-jdbc-driver-connection-errors"></a>Nasıl JDBC sürücüsü bağlantı hatalarını giderme?

Tomcat günlüklerinizi aşağıdaki iletiyi görebilirsiniz:

```
The web application[ROOT] registered the JDBC driver [com.mysql.jdbc.Driver] but failed to unregister it when the web application was stopped. To prevent a memory leak,the JDBC Driver has been forcibly unregistered
```

Hatayı gidermek için:

1. Uygulama/lib klasörünüzden sqljdbc*.jar dosyayı kaldırın.
2. Özel Tomcat veya Azure Market Tomcat web sunucusu kullanıyorsanız, bu .jar dosyasına Tomcat lib klasörüne kopyalayın.
3. Azure portalından Java etkinleştiriyorsanız (seçin **Java 1.8** > **Tomcat sunucusunu**), uygulamanıza paralel klasöründe sqljdbc.* jar dosyasını kopyalayın. Ardından, aşağıdaki sınıf ayarları web.config dosyasına ekleyin:

    ```
    <httpPlatform>
    <environmentVariables>
    <environmentVariablename ="JAVA_OPTS" value=" -Djava.net.preferIPv4Stack=true
    -Xms128M -classpath %CLASSPATH%;[Path to the sqljdbc*.jarfile]" />
    </environmentVariables>
    </httpPlatform>
    ```

## <a name="why-do-i-see-errors-when-i-attempt-to-copy-live-log-files"></a>Dinamik günlük dosyalarını kopyalamak çalıştığınızda hataları neden görüyorum?

Java uygulama (örneğin, Tomcat) için dinamik günlük dosyaları kopyalamak çalışırsanız, bu FTP hata görebilirsiniz:

```
Error transferring file [filename] Copying files from remote side failed.
    
The process cannot access the file because it is being used by another process.
```

Hata iletisi, FTP istemcisi bağlı olarak değişebilir.

Tüm Java uygulamalarını bu bir kilitleme sorunu var. Yalnızca Kudu, uygulama çalışırken bu dosya indirme destekler.

Uygulamasının durdurulması bu dosyalara FTP erişim sağlar.

Bir zamanlamaya göre çalışır ve bu dosyalar farklı bir dizine kopyalar bir Webjob'un yazmak için başka bir geçici bir çözüm değildir. Örnek proje için bkz: [CopyLogsJob](https://github.com/kamilsykora/CopyLogsJob) projesi.

## <a name="where-do-i-find-the-log-files-for-jetty"></a>Günlük dosyaları için Jetty nerede bulabilirim?

Market ve özel dağıtımlar için günlük dosyası D:\home\site\wwwroot\bin\jetty-distribution-9.1.2.v20140210\logs klasöründe bulunur. Klasör konumu, kullanmakta olduğunuz Jetty sürümünde bağlı olduğunu unutmayın. Örneğin, yol burada 9.1.2 Jetty için sağlanır. Jetty_ için Ara*YYYY_MM_DD*. stderrout.log.

Portal uygulama ayarı dağıtımları için D:\home\LogFiles günlük dosyasıdır. Jetty_ için Ara*YYYY_MM_DD*. stderrout.log

## <a name="can-i-send-email-from-my-azure-web-app"></a>My Azure web uygulamasından e-posta gönderebilir miyim?

Uygulama hizmeti yerleşik e-posta özelliği yoktur. Uygulamanızdan, e-posta göndermek için bazı iyi alternatifleri görmek için bu [yığın taşması tartışma](http://stackoverflow.com/questions/17666161/sending-email-from-azure).

## <a name="why-does-my-wordpress-site-redirect-to-another-url"></a>Neden WordPress Sitem başka bir URL'ye yönlendirilmesini?

Azure için son geçiş yaptıysanız, WordPress eski etki alanı URL'ye yeniden yönlendirebilir. Bu MySQL veritabanı ayarında kaynaklanır.

WordPress arkadaş + yeniden yönlendirme URL'sini doğrudan veritabanında güncelleştirmek için kullanabileceğiniz bir Azure Site uzantısı'dır. WordPress arkadaş + kullanma hakkında daha fazla bilgi için bkz: [WordPress araçları ve MySQL geçiş ile WordPress arkadaş +](https://blogs.msdn.microsoft.com/azureossds/2016/12/21/wordpress-tools-and-mysql-migration-with-wordpress-buddy/).

Alternatif olarak, el ile yeniden yönlendirme güncelleştirmek tercih ettiğiniz SQL sorguları ya da PHPMyAdmin, kullanarak URL'i [WordPress: yanlış URL'ye yeniden yönlendirerek](https://blogs.msdn.microsoft.com/azureossds/2016/07/12/wordpress-redirecting-to-wrong-url/).

## <a name="how-do-i-change-my-wordpress-sign-in-password"></a>Oturum açma WordPress parolamı nasıl değişiyor?

Oturum açma WordPress parolanızı unuttuysanız, WordPress arkadaş + güncelleştirmek için kullanabilirsiniz. Parolanızı sıfırlamak için WordPress arkadaş + Azure Site uzantısını yükleyin ve ardından bölümünde açıklanan adımları tamamlayın [WordPress araçları ve MySQL geçiş ile WordPress arkadaş +](https://blogs.msdn.microsoft.com/azureossds/2016/12/21/wordpress-tools-and-mysql-migration-with-wordpress-buddy/).

## <a name="i-cant-sign-in-to-wordpress-how-do-i-resolve-this"></a>I WordPress için oturum açılamıyor. Bu nasıl giderebilirim?

Kendiniz WordPress dışında bir eklenti yükledikten sonra son kilitli bulursanız, hatalı bir eklenti olabilir. WordPress arkadaş + yardımcı olabilecek bir Azure Site uzantısı, WordPress eklentileri devre dışı bırak'dır. Daha fazla bilgi için bkz: [WordPress araçları ve MySQL geçiş ile WordPress arkadaş +](https://blogs.msdn.microsoft.com/azureossds/2016/12/21/wordpress-tools-and-mysql-migration-with-wordpress-buddy/).

## <a name="how-do-i-migrate-my-wordpress-database"></a>WordPress Veritabanım nasıl geçişini?

WordPress Web sitenize bağlı MySQL veritabanını geçirme için birçok seçeneğiniz vardır:

* : Geliştiricilerin [komut istemi veya PHPMyAdmin](https://blogs.msdn.microsoft.com/azureossds/2016/03/02/migrating-data-between-mysql-databases-using-kudu-console-azure-app-service/)
* Olmayan-Geliştiriciler: [WordPress arkadaş +](https://blogs.msdn.microsoft.com/azureossds/2016/12/21/wordpress-tools-and-mysql-migration-with-wordpress-buddy/)

## <a name="how-do-i-help-make-wordpress-more-secure"></a>WordPress daha güvenli hale getirmenize nasıl yardımcı?

WordPress için en iyi güvenlik yöntemleri hakkında bilgi için bkz [azure'da WordPress güvenlik için en iyi uygulamaları](https://blogs.msdn.microsoft.com/azureossds/2016/12/26/best-practices-for-wordpress-security-on-azure/).

## <a name="i-am-trying-to-use-phpmyadmin-and-i-see-the-message-access-denied-how-do-i-resolve-this"></a>PHPMyAdmin kullanmak çalışıyorum ve "Erişim engellendi." iletisini görüyorum Bu nasıl giderebilirim?

Bu uygulama hizmeti örnek MySQL uygulama özelliği henüz çalışmıyorsa bu sorunla karşılaşabilirsiniz. Sorunu gidermek için Web sitenizi erişmeyi deneyin. Bu MySQL uygulama işlemi dahil olmak üzere gerekli işlemleri başlatır. Uygulama MySQL, işlem Explorer'da çalıştığını doğrulamak için bu mysqld.exe işlemlerde listelendiğinden emin olmak.

Bu MySQL uygulama çalıştığından emin olduktan sonra PHPMyAdmin kullanmayı deneyin.

## <a name="i-get-an-http-403-error-when-i-try-to-import-or-export-my-mysql-in-app-database-by-using-phpmyadmin-how-do-i-resolve-this"></a>Alınacak veya MySQL uygulama Veritabanım PHPMyadmin kullanarak dışarı çalıştığınızda bir HTTP 403 hata iletisi. Bu nasıl giderebilirim?

Chrome eski bir sürümünü kullanıyorsanız, bilinen hata yaşıyor. Sorunu çözmek için Chrome daha yeni bir sürüme yükseltin. Ayrıca Internet Explorer veya Edge'i, burada sorunu oluşmaz gibi farklı bir tarayıcı kullanarak deneyin.
