---
title: Azure Data Factory sorunlarını giderme | Microsoft Docs
description: Azure Data Factory sorunlarını giderme. Ortak belge tüm dış, Denetim etkinlikleri.
services: data-factory
author: abnarain
manager: craigg
ms.service: data-factory
ms.topic: troubleshoot
ms.subservice: troubleshoot
ms.date: 6/26/2019
ms.author: abnarain
ms.reviewer: craigg
ms.openlocfilehash: 8d6ab565098e1ea40ede5c650f05e670a1edc7f6
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67452689"
---
# <a name="troubleshooting-azure-data-factory"></a>Azure Data Factory sorunlarını giderme
Bu makalede, genel sorun giderme soruları listelenmektedir.

- [Azure Databricks (Not Defteri, jar dosyaları dışındaki, python)](#azure-databricks)
- [Azure Data Lake Analytics (U-SQL)](#azure-data-lake-analytics-u-sql)
- [Azure İşlevleri](#azure-functions)
- [Özel (Azure yığını için)](#custom-azure-batch)
- [HDInsight (Spark, Hive, MapReduce, Pig, Hadoop akış)](#hdinsight-spark-hive-mapreduce-pig-hadoop-streaming)
- [Web etkinliği](#web-activity)



## <a name="azure-databricks"></a>Azure Databricks
| Hata Kodu | Hata İletisi                                          | Sorun açıklaması                             | Olası düzeltme / önerilen eylem                            |
| -------------- | ----------------------------------------------------- | --------------------------------------------------------------| :----------------------------------------------------------- |
| 3200           | 403 hatası                                                    | Databricks erişim belirtecinin süresi doldu.                         | Varsayılan olarak, Databricks erişim belirteci 90 gün boyunca geçerlidir.  Lütfen yeni bir belirteç oluşturun ve bağlı hizmeti güncelleştirin. |
| 3201           | Gerekli alan eksik: settings.task.notebook_task.notebook_path | Bozuk yazma: Not defteri yolu doğru biçimde belirtilmemiş. | Lütfen Databricks etkinlik not defteri yolu belirtin. |
| 3201           | Küme... mevcut değil                                 | Yazma hatası: Databricks kümesini yok veya silinmiş olabilir. | Databricks kümesini var olduğunu doğrulayın. |
| 3201           | Geçersiz bir python dosyası URI:... Lütfen desteklenen URI düzenleri için Databricks kullanıcı kılavuzunu ziyaret edin. | Bozuk yazma                                                | Çalışma alanı adresleme düzeni için mutlak yollar veya DBFS depolanan dosyalar için "dbfs:/folder/subfolder/foo.py" belirtin. |
| 3201           | {0}   LinkedService etki alanı ve accessToken gerekli özelliklere sahip olmalıdır | Bozuk yazma                                                | Lütfen doğrulama [bağlantılı hizmet tanımı](compute-linked-services.md#azure-databricks-linked-service). |
| 3201           | {0}   LinkedService, mevcut küme kimliği ya da yeni küme oluşturma bilgilerini belirtmeniz gerekir | Bozuk yazma                                                | Lütfen doğrulama [bağlantılı hizmet tanımı](compute-linked-services.md#azure-databricks-linked-service). |
| 3201           | Düğüm türü Standard_D16S_v3 desteklenmiyor. Düğüm türleri desteklenir:   Standard_DS3_v2 Standard_DS4_v2, Standard_DS5_v2, Standard_D8s_v3, Standard_D16s_v3, Standard_D32s_v3, Standard_D64s_v3, işler için standart_d3_v2, standart_d8_v3, standart_d16_v3, standart_d32_v3, standart_d64_v3, işler için standart_d12_v2, işler için standart_d13_v2, İşler için standart_d14_v2 işler için standart_d15_v2, Standard_DS12_v2, Standard_DS13_v2, Standard_DS14_v2, Standard_DS15_v2, Standard_E8s_v3, Standard_E16s_v3, Standard_E32s_v3, Standard_E64s_v3, Standard_L4s, Standard_L8s, Standard_L16s, Standard_L32s, Standard_F4s Standard_F8s, Standard_F16s, işler için standart_h16, Standard_F4s_v2, Standard_F8s_v2, Standard_F16s_v2, Standard_F32s_v2, Standard_F64s_v2, Standard_F72s_v2, işler için standart_nc12, işler için standart_nc24, Standard_NC6s_v3, Standard_NC12s_v3, Standard_ NC24s_v3, Standard_L8s_v2, Standard_L16s_v2, Standard_L32s_v2, Standard_L64s_v2, Standard_L80s_v2 | Bozuk yazma                                                | Hata iletisi bakın                                          |
| 3201           | Geçersiz notebook_path:... Mutlak yollar yalnızca şu anda desteklenmiyor. Yolları başlamalıdır '/'. | Bozuk yazma                                                | Hata iletisi bakın                                          |
| 3202           | Son 3600 hız sınırı aşan saniye içinde oluşturulan 1000 işler zaten vardı:   3600 saniye başına 1000 işi oluşturma. | Çok fazla Databricks bir saat içinde çalışır.                         | İş oluşturma hızı için bu Databricks çalışma alanı kullanan tüm işlem hatları denetleyin.  İşlem hatları başlatılan toplam çok fazla Databricks çalışır, bazı ardışık düzenleri, yeni bir çalışma alanına geçirin. |
| 3202           | İstek nesnesi ayrıştırılamadı: Beklenen 'key' ve 'JSON harita alanı base_parameters için ayarlanacak değer' var ' anahtar: "..." ' Bunun yerine. | Yazma hatası: Parametresi için sağlanan değer         | İşlem hattı json inceleyin ve tüm dizüstü baseParameters parametrelerinde belirtilen bir boş olmayan değere sahip olun. |
| 3202           | Kullanıcı: SimpleUserContext {UserID = '... ' adı =user@company.com, Orgıd =...} kümeye erişmek için yetkili değil | Erişim belirteci oluşturan kullanıcı bağlı hizmette belirtilen Databricks kümesine erişmek için izin verilmiyor. | Kullanıcı çalışma alanında gerekli izinlere sahip olduğundan emin olun.   |
| 3203           | Küme kesildi, işlerine erişmek kullanılamaz durumda. Lütfen küme düzeltin veya daha sonra yeniden deneyin. | Küme sonlandırıldı.    Etkileşimli küme için bu bir yarış durumu olabilir. | Bunu önlemek için en iyi yolu, işlem kümeleri kullanmaktır.             |
| 3204           | İş yürütme başarısız oldu. Herhangi bir sayıda etkinliğe özgü iletiyi küme beklenmeyen durumundan hata iletileri olabilir.  En yaygın hata iletisi olmaz. | Yok                                                          | Yok                                                          |



## <a name="azure-data-lake-analytics-u-sql"></a>Azure Data Lake Analytics'i (U-SQL)

| Hata kodu         | Hata İletisi                                                | Sorun açıklaması                                          | Olası düzeltme / önerilen eylem                             |
| -------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 2709                 | Erişim belirteci yanlış kiracıdan.                    | Yanlış bir AAD kiracısı                                         | ADLA erişmek için kullanılan hizmet sorumlusu, başka bir AAD kiracısına ait. ADLA hesabı ile aynı kiracıda Lütfen yeni bir hizmet sorumlusu oluşturun. |
| 2711,   2705,   2704 | Yasak. ACL doğrulaması başarısız oldu. Kaynak mevcut değil veya kullanıcının istenen işlemi gerçekleştirmek için yetkili değil<br/><br/>Kullanıcı datalake deposu için erişmesi mümkün değil  <br/><br/>Data lake analytics'e kullanıcı yetkili değil | Hizmet sorumlusu veya sağlanan sertifika depolama alanındaki bir dosyaya erişimi yok | Hizmet sorumlusu emin olun veya ADLA işleri için sağladıkları sertifika ADLA hesabı hem de varsayılan ADLS depolama erişimi için kök klasöründen vardır. |
| 2711                 | 'Azure Data Lake Store' dosya veya klasör bulunamıyor       | Yanlış ya da bağlı hizmet kimlik bilgilerini erişiminiz yok USQL dosyasının yolu olan | Lütfen yolun ve bağlı hizmette sağlanan kimlik bilgilerini doğrulayın |
| 2707                 | AzureDataLakeAnalytics hesabını çözümlenemiyor. Lütfen 'AccountName' ve 'DataLakeAnalyticsUri' denetleyin. | ADLA hesabı bağlı hizmette bir sorun var                  | Lütfen doğru hesap sağlandığını doğrulayın.             |
| 2703                 | Hata Kimliği: E_CQO_SYSTEM_INTERNAL_ERROR veya herhangi bir hata ile başlar "hata kodu:" | Hata ADLA'dan geliyor                                    | ADLA ve betik vardır, örneğin iş anlamına gelir gibi görünen herhangi bir hata başarısız gönderildi. ADLA üzerinde araştırma yapılması gerekir. Portalını açın ve ADLA hesabına gidin, iş için ADF etkinlik kimliği (çalıştırma kimliği değil işlem hattı) Çalıştır'ı kullanarak bakın. İş var. hata hakkında daha fazla bilgi olacaktır ve sorun giderme için yardımcı olur. Çözüm açık değilse, lütfen ADLA Destek ekibine başvurun ve hesabınızın adını ve iş kimliğini içeren proje URL'sini sağlayın |
| 2709                 | Biz, şu anda işiniz kabul edemez. Hesabınız için sıraya alınmış işlerin sayısı 200'dür. | ADLA üzerinde azaltma                                           | ADF tetikleyiciler ve etkinliklerde eşzamanlılık ayarları değiştirerek ADLA için gönderilen işlerin sayısını azaltın veya ADLA sınırları artırın |
| 2709                 | 24 gerektirdiğinden bu işi reddedildi AU ve bu hesabı olan bir işi 5'ten fazla kullanmasını önler yönetici tarafından tanımlanan bir ilke AU. | ADLA üzerinde azaltma                                           | ADF tetikleyiciler ve etkinliklerde eşzamanlılık ayarları değiştirerek ADLA için gönderilen işlerin sayısını azaltın veya ADLA sınırları artırın |



## <a name="azure-functions"></a>Azure İşlevleri

| Hata kodu | Hata İletisi                           | Açıklama                                                  | Olası düzeltme / önerilen eylem                           |
| ------------ | --------------------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 3600         | Yanıt içeriği geçerli bir JObject değil | Başka bir deyişle, Azure işlevini yanıtta bir JSON yükü döndürmedi. ADF Azure işlev etkinliği, JSON yanıt içeriği yalnızca destekler. | Azure işlevi, geçerli bir JSON yükü ör döndürmek için güncelleştirme bir C# işlevi döndürebilirsiniz (ActionResult) yeni < OkObjectResult ("{`\"Id\":\"123\"`}"); |
| 3600         | Geçersiz HttpMethod: '..'.               | Başka bir deyişle, etkinlik yükte belirtilen Http yöntemini Azure işlev etkinliği tarafından desteklenmiyor. | Desteklenen Http yöntemleri şunlardır:  <br/>PUT, POST, AL, DELETE, HEAD, İZLEME SEÇENEKLERİ |



## <a name="custom-azure-batch"></a>Özel (Azure yığını için)
| Hata kodu | Hata İletisi                                                | Açıklama                                                  | Olası düzeltme / önerilen eylem                           |
| ------------ | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 2500         | İsabet beklenmeyen bir özel durum ve yürütme başarısız oldu.             | Komut veya bir hata kodu döndürdü program başlatılamıyor. | Yürütülebilir dosya olup olmadığını kontrol edin. Program başlatıldı, stderr.txt depolama hesabına yüklediniz ve stdout.txt denetleyin. Hata ayıklama için kodunuzda copious günlükleri yaymak için iyi bir uygulamadır. |
| 2501         | Kullanıcı batch hesabı erişilemiyor batch hesabı ayarları gözden geçirin. | Yanlış Batch erişim anahtarı veya havuz adı sağlandı.            | Havuz adı ve Batch erişim anahtarı bağlantılı hizmetteki doğrulamanız gerekir. |
| 2502         | Erişim kullanıcı depolama hesabı, lütfen depolama hesabı ayarlarını denetleyin. | Sağlanan geçersiz depolama hesabı adı veya erişim anahtarı.       | Depolama hesabı adını ve erişim anahtarı bağlı hizmette doğrulamanız gerekir. |
| 2504         | İşlem, bir geçersiz durum kodu 'BadRequest' döndürdü.     | FolderPath çok fazla dosya varsa özel etkinlik.  (Birden fazla 32.768 karakter resourceFiles toplam boyutu olamaz.) | Gereksiz dosyaları kaldırın veya bunları zip ve ayıklamak, örneğin bir unzip komutu ekleyin: powershell.exe - nologo - noprofile-komut "& {Add-Type - A 'System.IO.Compression.FileSystem';   [IO.Compression.ZipFile]::ExtractToDirectory ($zipFile, $folder); }" ;  $folder\yourProgram.exe |
| 2505         | Hesap anahtarı kimlik bilgileri kullanılmadığı sürece, paylaşılan erişim imzası oluşturulamıyor. | Özel etkinlikler, yalnızca bir erişim anahtarı kullanan depolama hesaplarını destekler. | Açıklama bakın                                            |
| 2507         | Klasör yolu yok veya boş:...            | Depolama hesabında belirtilen yolda bir dosya yok.       | FolderPath çalıştırmak istediğiniz yürütülebilir dosyaları içermelidir. |
| 2508         | Kaynak klasörde yinelenen dosyalar demektir.               | FolderPath farklı alt klasörlerinde aynı ada sahip birden çok dosya vardır. | Özel etkinlikler, folderPath altında klasör yapısını düzleştirecek.  Klasör yapısı korunması gerekiyorsa, ZIP dosyaları ve bunları Azure Batch'te bir unzip komutuyla örneğin ayıklayın: powershell.exe - nologo - noprofile-komut "& {Add-Type - A 'System.IO.Compression.FileSystem';   [IO.Compression.ZipFile]::ExtractToDirectory ($zipFile, $folder); }" ;   $folder\yourProgram.exe |
| 2509         | Batch URL'si... olduğundan geçersizse, bu URI biçiminde olmalıdır.         | Batch URL'leri benzer olmalıdır. https://mybatchaccount.eastus.batch.azure.com | Açıklama bakın                                            |
| 2510         | İstek gönderilirken bir hata oluştu.               | Toplu iş URL'si geçersiz                                         | Batch URL'yi doğrulayın.                                            |

## <a name="hdinsight-spark-hive-mapreduce-pig-hadoop-streaming"></a>HDInsight (Spark, Hive, MapReduce, Pig, Hadoop akış)

| Hata kodu | Hata İletisi                                                | Açıklama                                                  | Olası düzeltme / önerilen eylem                           |
| ------------ | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 2300,   2310 | Hadoop işi gönderilemedi. Hata: Uzak ad çözümlenemedi. <br/><br/>Küme bulunamadı. | Belirtilen küme URI'si geçersiz.                              | Lütfen küme silinmez ve belirtilen URI doğru olduğundan emin olun. URI herhangi bir tarayıcıda açın ve Ambari UI görmeniz gerekir. Küme sanal ağ içinde ise, ardından, URI özel URI olmalıdır ve aynı sanal ağda bir parçası olan bir sanal makineden açmayı denemelisiniz. Daha fazla bilgi için [sanal ağda HDInsight](https://docs.microsoft.com/azure/hdinsight/hdinsight-extend-hadoop-virtual-network#directly-connect-to-apache-hadoop-services). |
| 2300         | Hadoop işi gönderilemedi. İş:..., küme:... /. Hata: Bir görev iptal edildi. | İş gönderme zaman aşımına uğradı.                         | Bu genel HDInsight bağlantısı sorunu veya ağ bağlantısı sorunu olabilir. İlk HDInsight Ambari UI, herhangi bir tarayıcı kullanılabilir ve kimlik bilgileriniz hala geçerli olduğundan emin olun. Kendinden konak IR kendinden konak IR kullanıyorsanız yüklendiği VM/makineden bunu emin olun ADF işten göndermeyi yeniden deneyin. Yine başarısız olursa ADF takım için desteğe başvurun. |
| 2300         | Yetkisiz:   Ambari kullanıcı adı veya parola yanlış  <br/><br/>Yetkisiz:   Ambari kullanıcı yönetici kilitli   <br/><br/>403 - Yasak: Erişim reddedildi | HDInsight için sağlanan kimlik bilgileri yanlış veya süresi dolmuş | Lütfen bunları düzeltin ve bağlı hizmeti yeniden dağıtın. Kimlik bilgilerini HDInsight üzerinde ilk küme URI'si dilediğiniz tarayıcıda açma ve oturum açmaya çalışırken çalıştığından emin olun. İşe yaramazsa, Azure portalından sıfırlayabilirsiniz. |
| 2300,   2310 | 502 - web sunucusu ağ geçidi veya proxy sunucusu olarak çalışırken geçersiz bir yanıt aldı.       <br/>Hatalı Ağ Geçidi | Hata HDInsight geliyor                               | Bu hata, HDInsight kümesinden kullanıma sunulacaktır. Başvuru [HDInsight sorun giderici](https://hdinsight.github.io/ambari/ambari-ui-502-error.html) sık karşılaşılan ile.    <br/>Spark kümeleri için ayrıca nedeniyle kaynaklanabilir [bu](https://hdinsight.github.io/spark/spark-thriftserver-errors.html). <br/><br/>[Ek bağlantısı](https://docs.microsoft.com/azure/application-gateway/application-gateway-troubleshooting-502) |
| 2300         | Hadoop işi gönderilemedi. İş:..., küme:... Hata: {\"hata\":\"templeton da hizmet çok fazla gönderme iş isteklerle meşgul olduğundan proje istek Gönder hizmeti oluşturulamıyor. Lütfen işlemi yeniden denemeden önce bir süre bekleyin. Eş zamanlı istekleri yapılandırmak için yapılandırma templeton.parallellism.job.submit başvurun. \  <br/><br/>Hadoop işi gönderilemedi. İş: 161da5d4-6fa8-4ef4-a240-6b6428c5ae2f, küme: https://abc-analytics-prod-hdi-hd-trax-prod01.azurehdinsight.net/.   Hata: {\"hata\":\"java.io.IOException: org.apache.hadoop.yarn.exceptions.YarnException: YARN için application_1561147195099_3730 gönderilemedi: org.apache.hadoop.security.AccessControlException: Kuyruk root.joblauncher zaten 500 uygulamalar içeren, uygulama teslimini kabul edemez: application_1561147195099_3730\ | Aynı anda çok fazla işleri için HDInsight gönderilen | HDI için gönderilen eşzamanlı iş sayısını sınırlandırmayı göz önünde bulundurun. Aynı etkinlik tarafından gönderilen varsa lütfen ADF etkinlik eşzamanlılık için bakın. Tetikleyiciler, zaman içinde eşzamanlı bir işlem hattı çalıştırmaları yayılır şekilde değiştirin. Ayrıca hata anlaşılacağı gibi "templeton.parallellism.job.submit" ince ayar için HDInsight belgeleri için bakın. |
| 2303,   2347 | Hadoop işi '5' çıkış kodu ile başarısız oldu. Bkz. 'wasbs://adfjobs@adftrialrun.blob.core.windows.net/StreamingJobs/da4afc6d-7836-444e-bbd5-635fce315997/18_06_2019_05_36_05_050/stderr' daha fazla ayrıntı için.  <br/><br/>Hive yürütme 'UserErrorHiveOdbcCommandExecutionFailure' hata koduyla başarısız oldu.   Bkz. 'wasbs://adfjobs@eclsupplychainblobd.blob.core.windows.net/HiveQueryJobs/16439742-edd5-4efe-adf6-9b8ff5770beb/18_06_2019_07_37_50_477/Status/hive.out' daha fazla ayrıntı için | HDInsight için işi gönderildi ve HDInsight üzerinde başarısız | İş için HDInsight başarıyla gönderildi. Bu, küme üzerinde başarısız oldu. Lütfen HDInsight Ambari UI, işin açın ve Günlükler var. açabilir veya hata iletisi noktaları olarak depolama biriminden dosyayı açın. Hatanın ayrıntıları bu dosyada olacaktır. |
| 2328         | İstek işlenirken bir iç sunucu hatası oluştu. Lütfen isteği yeniden deneyin veya desteğe başvurun | İsteğe bağlı HDInsight üzerinde gerçekleşir.                              | HDInsight sağlama başarısız olduğunda bu hata, HDInsight hizmetinden kullanıma sunulacaktır. Lütfen HDInsight ekibine başvurun ve bunları üzerinde isteğe bağlı küme adı sağlayın. |
| 2310         | java.lang.NullPointerException                               | Spark kümesi işe gönderilirken hata oluştu      | Bu özel durum HDInsight geliyor ve ise asıl soruna gizleme.   Lütfen HDInsight takım desteğine başvurun ve küme adı ve etkinlik zaman aralığı çalıştırma verin. |
|              | Diğer tüm hatalar                                             |                                                              | Lütfen [HDInsight sorun giderici](../hdinsight/hdinsight-troubleshoot-guide.md) ve [HDInsight SSS](https://hdinsight.github.io/) |



## <a name="web-activity"></a>Web Etkinliği

| Hata kodu | Hata İletisi                                                | Açıklama                                                  | Olası düzeltme / önerilen eylem                           |
| ------------ | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 2108         | Geçersiz HttpMethod: '..'.                                    | Başka bir deyişle, etkinlik yükte belirtilen Http yöntemini Web etkinliği tarafından desteklenmiyor. | Desteklenen Http yöntemleri şunlardır: <br/>PUT, POST, AL, SİL |
| 2108         | Geçersiz Sunucu Hatası 500                                     | İç hata: uç nokta                               | URL (ile Fiddler/Postman) işlevselliği kontrol edin: [Bir HTTP oturumu oluşturmak için Fiddler'ı kullanma](#how-to-use-fiddler-to-create-an-http-session-of-the-monitored-web-application) |
| 2108         | 401 Yetkisiz                                             | Geçerli bir kimlik doğrulama isteğini eksik                      | Geçerli kimlik doğrulama yöntemini belirtin (belirtecin süresi dolmuş olabilir).   <br/><br/>URL (ile Fiddler/Postman) işlevselliği kontrol edin: [Bir HTTP oturumu oluşturmak için Fiddler'ı kullanma](#how-to-use-fiddler-to-create-an-http-session-of-the-monitored-web-application) |
| 2108         | 403 Yasak                                                | Gerekli izinler eksik                                 | Erişilen kaynak kullanıcı izinlerini denetleyin.   <br/><br/>URL (ile Fiddler/Postman) işlevselliği kontrol edin: [Bir HTTP oturumu oluşturmak için Fiddler'ı kullanma](#how-to-use-fiddler-to-create-an-http-session-of-the-monitored-web-application) |
| 2108         | 400 Hatalı istek                                              | Geçersiz bir Http isteği                                         | URL'yi ve fiili istek gövdesi denetleyin.   <br/><br/>Fiddler/Postman isteği doğrulamak için kullanın: [Bir HTTP oturumu oluşturmak için Fiddler'ı kullanma](#how-to-use-fiddler-to-create-an-http-session-of-the-monitored-web-application) |
| 2108         | 404 bulunamadı                                                | Kaynak bulunamadı                                       | Fiddler/Postman isteği doğrulamak için kullanın: [Bir HTTP oturumu oluşturmak için Fiddler'ı kullanma](#how-to-use-fiddler-to-create-an-http-session-of-the-monitored-web-application) |
| 2108         | Hizmet kullanılamıyor                                          | Hizmet kullanılamıyor                                       | Fiddler/Postman isteği doğrulamak için kullanın: [Bir HTTP oturumu oluşturmak için Fiddler'ı kullanma](#how-to-use-fiddler-to-create-an-http-session-of-the-monitored-web-application) |
| 2108         | Desteklenmeyen medya türü                                       | Web etkinliği gövde ile eşleşmeyen Content-Type           | Doğru içerik kullanım Fiddler/Postman isteği doğrulama yükü biçimiyle eşleşen türü belirtin: [Bir HTTP oturumu oluşturmak için Fiddler'ı kullanma](#how-to-use-fiddler-to-create-an-http-session-of-the-monitored-web-application) |
| 2108         | Aradığınız kaynak kaldırılmış, adı değiştirilmiş veya geçici olarak kullanılamıyor. | Kaynak kullanılamıyor                                | Uç nokta kontrol etmek için Fiddler/Postman kullanın: [Bir HTTP oturumu oluşturmak için Fiddler'ı kullanma](#how-to-use-fiddler-to-create-an-http-session-of-the-monitored-web-application) |
| 2108         | Geçersiz bir yöntem (HTTP fiili) kullanıldığından aradığınız sayfa görüntülenemiyor. | Web etkinliği yöntemi hatalı istekte belirtildi   | Uç nokta kontrol etmek için Fiddler/Postman kullanın: [Bir HTTP oturumu oluşturmak için Fiddler'ı kullanma](#how-to-use-fiddler-to-create-an-http-session-of-the-monitored-web-application) |
| 2108         | invalid_payload                                              | Web etkinliği için gövdesi yanlış                       | Uç nokta kontrol etmek için Fiddler/Postman kullanın: [Bir HTTP oturumu oluşturmak için Fiddler'ı kullanma](#how-to-use-fiddler-to-create-an-http-session-of-the-monitored-web-application) |

#### <a name="how-to-use-fiddler-to-create-an-http-session-of-the-monitored-web-application"></a>İzlenen web uygulamasının bir HTTP oturumu oluşturmak için Fiddler'ı kullanma

1. İndirme ve yükleme [Fiddler](https://www.telerik.com/download/fiddler)

2. Web uygulamanızı HTTPS kullanıyorsa:

   1. Fiddler'ı açıp

   2. Git **Araçlar > Fiddler seçenekleri** ve ekran aşağıdaki gibi seçin. 

      ![fiddler seçenekleri](media/data-factory-troubleshoot-guide/fiddler-options.png)

3. Uygulamanız için SSL sertifikalarını kullanıyorsa, cihazınıza Fiddler sertifika da eklemeniz gerekir.

4. Cihazınız için Fiddler sertifika eklemek için Git **Araçları** > **Fiddler seçenekleri** > **HTTPS**  >   **Eylemler** > **masaüstüne kök sertifikayı dışarı aktar** Fiddler sertifika elde edilir.

5. Böylece yeni bir izleme başlatılabilmesi için tarayıcının önbelleğini temizlenebilir yakalamayı devre dışı bırakın.

6. 1. Git **dosya** > **trafik yakalama** veya basın **F12**.
   2. Tüm önbelleğe alınmış öğeleri kaldırılır ve yeniden indirilen tarayıcınızın önbelleğini temizleyin.

7. İsteği oluşturun: 

8. 1. Composer sekmesine tıklayın
   2. Http yöntemi ve URL'yi ayarlayın
   3. Üst bilgileri ekleyin ve gerekirse istek gövdesi
   4. Çalıştır seçeneğini tıklatın

9. Trafik yeniden Yakalamayı Başlat ve sorunlu işlem sayfanızda tamamlayın.

10. Tamamlandığında, Git **dosya** > **Kaydet** > **tüm oturumları**.

Fiddler'ı hakkında daha fazla bilgi [burada](https://docs.telerik.com/fiddler/Configure-Fiddler/Tasks/ConfigureFiddler)

## <a name="next-steps"></a>Sonraki adımlar

Sorununuz için çözüm bulma daha fazla yardım için deneyebileceğiniz bazı kaynaklar aşağıda verilmiştir.

*  [Bloglar](https://azure.microsoft.com/blog/tag/azure-data-factory/)
*  [Özellik istekleri](https://feedback.azure.com/forums/270578-data-factory)
*  [Videolar](https://azure.microsoft.com/resources/videos/index/?sort=newest&services=data-factory)
*  [MSDN Forumu](https://social.msdn.microsoft.com/Forums/home?sort=relevancedesc&brandIgnore=True&searchTerm=data+factory)
*  [Stack Overflow](https://stackoverflow.com/questions/tagged/azure-data-factory)
*  [Twitter](https://twitter.com/hashtag/DataFactory)



