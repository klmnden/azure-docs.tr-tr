---
title: "SQL bağlantı hatası, geçici hata düzeltme | Microsoft Docs"
description: "Sorun giderme, tanılama ve SQL bağlantı hatası veya Azure SQL veritabanındaki geçici hata önlemek öğrenin."
keywords: "SQL bağlantısı, bağlantı dizesi, bağlantı sorunları, geçici bir hata oluştu, bağlantı hatası"
services: sql-database
author: dalechen
manager: craigg
ms.service: sql-database
ms.custom: develop apps
ms.topic: article
ms.date: 11/29/2017
ms.author: daleche
ms.openlocfilehash: f6b5f825d7f8111075fe37b5dc29d174928d913e
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/16/2018
---
# <a name="troubleshoot-diagnose-and-prevent-sql-connection-errors-and-transient-errors-for-sql-database"></a>SQL Database için SQL bağlantı hatalarını ve geçici hataları giderme, tanılama ve önleme
Bu makalede, engellemek, sorun giderme, tanılama ve bağlantı hataları ve Azure SQL veritabanı ile etkileşim kurarken, istemci uygulamanız karşılaştığında geçici hataları etkisini açıklar. Yeniden deneme mantığı yapılandırmak, bağlantı dizesi oluşturma ve diğer bağlantı ayarlarını öğrenin.

<a id="i-transient-faults" name="i-transient-faults"></a>

## <a name="transient-errors-transient-faults"></a>Geçici hataları geçici
Geçici bir hata olarak da bilinen, geçici bir hata yakında kendisini çözümleyen bir temel neden vardır. Azure sistem hızla donanım kaynakları daha iyi yük dengelemek için çeşitli iş yükleri geçtiğinde geçici hataları zaman zaman bir nedenidir. Bu yeniden yapılandırma olayları çoğunu 60 saniyeden daha kısa bir süre içinde tamamlayın. Bu yeniden yapılandırma zaman aralığı sırasında SQL veritabanı için bağlantı sorunu olabilir. SQL veritabanına bağlama uygulamalar, bu geçici hataları beklenir oluşturulmalıdır. Bunları işlemek için bunları kullanıcılara uygulama hataları olarak görünmesini yerine kendi kodunda yeniden deneme mantığı uygular.

İstemci programınız ADO.NET kullanıyorsa, programınızı throw tarafından geçici hata hakkında işe **SqlException**. Karşılaştırma **numarası** makalenin üst kısmından bulunan geçici hataları listesine karşı özelliği [SQL Database istemci uygulamaları için SQL hata kodları](sql-database-develop-error-messages.md).

<a id="connection-versus-command" name="connection-versus-command"></a>

### <a name="connection-vs-command"></a>Bağlantı komutu karşılaştırması
SQL bağlantısı yeniden deneyin veya yeniden bağlı olarak aşağıdaki oluşturun:

* **Bir bağlantı try sırasında geçici bir hata oluşur**: birkaç saniye bir gecikmeden sonra bağlantı yeniden deneyin.
* **Bir SQL sorgu komutu sırasında geçici bir hata oluşur**: komutu hemen yeniden denemeyin. Bunun yerine, bir gecikmeden sonra istemcinin yeniden bağlantı kurun. Sonra komutu yeniden deneyin.


<a id="j-retry-logic-transient-faults" name="j-retry-logic-transient-faults"></a>

## <a name="retry-logic-for-transient-errors"></a>Geçici hatalar için yeniden deneme mantığı
Yeniden deneme mantığı içerdiğinde bazen geçici bir hatayla karşılaşırsanız istemci programları daha sağlamdır.

Programınızı üçüncü taraf ara yazılımı SQL veritabanıyla iletişim kurduğunda, satıcı ara yazılım geçici hataları yeniden deneme mantığı içerip içermediğini isteyin.

<a id="principles-for-retry" name="principles-for-retry"></a>

### <a name="principles-for-retry"></a>Yeniden deneme ilkelerini
* Geçici bir hatadır, bir bağlantı açmak için yeniden deneyin.
* Doğrudan geçici bir hata ile başarısız oldu SQL SELECT deyimi yeniden denemeyin.
  * Bunun yerine, yeni bir bağlantı kurarak Seç yeniden deneyin.
* Bir SQL UPDATE deyimi geçici bir hata ile başarısız olduğunda, GÜNCELLEŞTİRMEYİ yeniden denemeden önce yeni bir bağlantı kurun.
  * Yeniden deneme mantığı, tüm veritabanı ya da bir işlemin tamamlandığını veya tüm işlem geri alındı emin olmalısınız.

### <a name="other-considerations-for-retry"></a>Yeniden deneme ile diğer değerlendirmeler
* Otomatik olarak çalışma saatlerinden sonra başlayan ve sabah çok uzun zaman aralıklarına kendi yeniden deneme girişimleri arasındaki ile hasta destekleyebilir önce tamamlanır toplu program.
* Bir kullanıcı arabirimi programı sonra çok uzun bekleme vermek İnsan eğilimi hesabı.
  * Bu ilke istekleri sistemiyle bölgesini doldurmak için çözüm her birkaç saniyede yeniden gerekir.

### <a name="interval-increase-between-retries"></a>Denemeler arasındaki aralığı artırma
İlk, yeniden denemeden önce 5 saniye beklemenizi öneririz. Bulut hizmeti aşırı 5 saniye riskleri kısa bir gecikme sonrasında yeniden deneniyor. Sonraki denemeler için gecikme büyümesini katlanarak, en çok 60 saniye.

Bir ADO.NET kullanan istemciler için engelleme süresi tartışma için bkz [SQL Server bağlantı (ADO.NET) havuzu](http://msdn.microsoft.com/library/8xx3tyca.aspx).

Program otomatik olarak kapatılmadan önce yeniden deneme sayısı üst ayarlamak isteyebilirsiniz.

### <a name="code-samples-with-retry-logic"></a>Yeniden deneme mantığı ile kod örnekleri
Kod örnekleri yeniden deneme mantığı ile şu konumda mevcuttur:

- [SQL ADO.NET ile resiliently bağlanma][step-4-connect-resiliently-to-sql-with-ado-net-a78n]
- [PHP ile SQL resiliently bağlanma][step-4-connect-resiliently-to-sql-with-php-p42h]

<a id="k-test-retry-logic" name="k-test-retry-logic"></a>

### <a name="test-your-retry-logic"></a>Yeniden deneme mantığı test
Yeniden deneme mantığı sınamak için benzetimini yapmak veya programınız çalışmaya devam ederken, düzeltilebilir hataya neden gerekir.

#### <a name="test-by-disconnecting-from-the-network"></a>Ağ bağlantısını keserek test
Yeniden deneme mantığı sınayabilirsiniz bir program çalışırken, istemci bilgisayar ağ bağlantısını kesmek için yoludur. Hata.:

* **SqlException.Number** 11001 =
* İleti: "gibi bir ana makine bilinmiyor"

İlk yeniden deneme girişimi bir parçası olarak programınızı yazım hatası düzeltin ve bağlanmayı dener.

Bu test pratik hale getirmek için program başlamadan önce ağ bilgisayarınızdan çıkarın. Ardından, programı programa neden olan bir çalışma zamanı parametresi tanır:

* Geçici olarak 11001 geçici dikkate alınması gereken hataları listesine ekleyin.
* İlk bağlantı her zamanki gibi çalışır.
* Hata belirledi sonra 11001 listeden kaldırın.
* Bu bilgisayarın ağınıza bağlama kullanıcıya belirten bir ileti görüntüler.
   * Daha fazla yürütme kullanarak duraklatmak **Console.ReadLine** yöntemi veya bir iletişim kutusunda Tamam düğmesi. Kullanıcı, bilgisayar ağa takılı sonra Enter tuşuna basar.
* Yeniden bağlanma, başarı bekleniyor girişimi.

#### <a name="test-by-misspelling-the-database-name-when-connecting"></a>Veritabanı adı bağlanırken yazım hatası ile test
Programınızı kasıtlı olarak, hata ilk bağlantı denemesine önce kullanıcı adı hatalı. Hata.:

* **SqlException.Number** 18456 =
* İleti: "'WRONG_MyUserName' kullanıcı için oturum açma başarısız."

İlk yeniden deneme girişimi bir parçası olarak programınızı yazım hatası düzeltin ve bağlanmayı dener.

Bu test pratik hale getirmek için programa neden olan bir çalışma zamanı parametresi programınızı tanır:

* Geçici olarak 18456 geçici dikkate alınması gereken hataları listesine ekleyin.
* Kasıtlı olarak 'WRONG_' için kullanıcı adını ekleyin.
* Hata belirledi sonra 18456 listeden kaldırın.
* 'WRONG_' kullanıcı adını kaldırın.
* Yeniden bağlanma, başarı bekleniyor girişimi.


<a id="net-sqlconnection-parameters-for-connection-retry" name="net-sqlconnection-parameters-for-connection-retry"></a>

## <a name="net-sqlconnection-parameters-for-connection-retry"></a>Bağlantı yeniden deneme için .NET SqlConnection parametreleri
.NET Framework sınıf kullanarak istemci programınız SQL veritabanına bağlanır, **System.Data.SqlClient.SqlConnection**, .NET 4.6.1 kullanın veya daha sonra (veya .NET Core) böylece kendi bağlantısı yeniden deneme özelliğini kullanabilirsiniz. Bu özellik hakkında daha fazla bilgi için bkz: [bu Web sayfası](http://go.microsoft.com/fwlink/?linkid=393996).

<!--
2015-11-30, FwLink 393996 points to dn632678.aspx, which links to a downloadable .docx related to SqlClient and SQL Server 2014.
-->

Oluşturduğunuzda [bağlantı dizesi](http://msdn.microsoft.com/library/System.Data.SqlClient.SqlConnection.connectionstring.aspx) için **SqlConnection** nesne, değerler aşağıdaki parametreleri arasında koordine:

* **ConnectRetryCount**:&nbsp;&nbsp;1 varsayılandır. Aralık 0 ile 255'dır.
* **ConnectRetryInterval**:&nbsp;&nbsp;1 saniye varsayılandır. 1 ile 60 aralıktır.
* **Bağlantı zaman aşımı**:&nbsp;&nbsp;15 saniye varsayılandır. 0 ile 2147483647 arasındadır.

Özellikle, seçilen değerlerinizi aşağıdaki eşitlik true olmanız gerekir:

Bağlantı zaman aşımı ConnectRetryCount = * ConnectionRetryInterval

Örneğin, sayısı 3'e eşittir ve aralığı 10 saniye eşittir, yalnızca 29 saniye cinsinden zaman aşımı sistemin, üçüncü ve son yeniden bağlanmak yeterli süre vermez: 29 < 3 * 10.

<a id="connection-versus-command" name="connection-versus-command"></a>

## <a name="connection-vs-command"></a>Bağlantı komutu karşılaştırması
**ConnectRetryCount** ve **ConnectRetryInterval** parametreleri sağlar, **SqlConnection** nesne söyleyen veya rahatsız etme bağlanma işlemi yeniden deneyin, program, programınıza denetimi döndürme gibi. Yeniden deneme aşağıdaki durumlarda oluşabilir:

* mySqlConnection.Open yöntem çağrısı
* mySqlConnection.Execute yöntem çağrısı

Bir subtlety yoktur. Geçici bir hata oluşursa karşın, *sorgu* yürütülmekte olan, **SqlConnection** nesnesi değil bağlanma işlemi yeniden deneyin. Kesinlikle sorgunuzu yeniden değil. Ancak, **SqlConnection** çok hızlı bir şekilde sorgu yürütme için göndermeden önce bağlantıyı denetler. Bir bağlantı sorunu hızlı onay algılarsa, **SqlConnection** bağlanma işlemini yeniden dener. Yeniden deneme başarılı olursa, sorgu yürütme için gönderilir.

### <a name="should-connectretrycount-be-combined-with-application-retry-logic"></a>Uygulama yeniden deneme mantığı ile ConnectRetryCount birleştirilmelidir?
Güçlü özel yeniden deneme mantığı uygulamanız olduğunu varsayalım. Bağlanma işlemi dört kez yeniden deneyebilirler. Eklerseniz **ConnectRetryInterval** ve **ConnectRetryCount** = 3 bağlantı dizenizi 4 * 3 = 12 yeniden deneme sayısı artacaktır yeniden deneme sayısı. Yeniden deneme yüksek sayı düşündüğünüz değil.


<a id="a-connection-connection-string" name="a-connection-connection-string"></a>

## <a name="connections-to-sql-database"></a>SQL veritabanı bağlantıları
<a id="c-connection-string" name="c-connection-string"></a>

### <a name="connection-connection-string"></a>Bağlantı: Bağlantı dizesi
SQL veritabanına bağlanmak gerekli olan bağlantı dizesi, SQL Server'a bağlanmak için kullanılan dize biraz farklıdır. Veritabanınızdan için bağlantı dizesini kopyalayabilirsiniz [Azure portal](https://portal.azure.com/).

[!INCLUDE [sql-database-include-connection-string-20-portalshots](../../includes/sql-database-include-connection-string-20-portalshots.md)]

<a id="b-connection-ip-address" name="b-connection-ip-address"></a>

### <a name="connection-ip-address"></a>Bağlantı: IP adresi
SQL veritabanı sunucusu, istemci programınızı barındıran bilgisayarın IP adresinden gelen iletişimi kabul edecek şekilde yapılandırmanız gerekir. Bu yapılandırmayı ayarlamak için Güvenlik Duvarı Ayarları'nda Düzenle [Azure portal](https://portal.azure.com/).

IP adresini yapılandırmak unutursanız, programınızı gerekli IP adresi bildiren bir kullanışlı hata iletisi ile başarısız olur.

[!INCLUDE [sql-database-include-ip-address-22-portal](../../includes/sql-database-include-ip-address-22-v12portal.md)]

Daha fazla bilgi için bkz: [SQL veritabanında güvenlik duvarı ayarlarını yapılandırma](sql-database-configure-firewall-settings.md).
<a id="c-connection-ports" name="c-connection-ports"></a>

### <a name="connection-ports"></a>Bağlantı: bağlantı noktaları
Genellikle, yalnızca bağlantı noktası 1433 istemci programınızı barındıran bilgisayarda giden iletişim için açık olduğundan emin olmanız gerekir.

İstemci programınızı bir Windows bilgisayarda barındırılıyorsa, örneğin, Windows Güvenlik Duvarı konakta 1433 numaralı bağlantı noktasını açmak için kullanabilirsiniz.

1. Denetim Masası'nı açın.

2. Seçin **tüm Denetim Masası öğeleri** > **Windows Güvenlik Duvarı** > **Gelişmiş ayarları** > **giden kuralları**   >  **Eylemler** > **yeni kural**.

İstemci programınızı bir Azure sanal makine (VM) üzerinde barındırılıyorsa okuma [ADO.NET 4.5 ve SQL veritabanı için 1433 dışındaki bağlantı noktaları](sql-database-develop-direct-route-ports-adonet-v12.md).

Arka plan bağlantı noktaları ve IP adreslerini yapılandırma hakkında bilgi için [Azure SQL veritabanı Güvenlik Duvarı'nı](sql-database-firewall-configure.md).

<a id="d-connection-ado-net-4-5" name="d-connection-ado-net-4-5"></a>

### <a name="connection-adonet-461"></a>Bağlantı: ADO.NET 4.6.1
ADO.NET sınıflarını gibi programınızı kullanıyorsa, **System.Data.SqlClient.SqlConnection** SQL veritabanına bağlanmak için .NET Framework 4.6.1 sürümü kullanmanızı öneririz veya sonraki bir sürümü.

ADO.NET 4.6.1:

* Kullanarak bir bağlantıyı açmak için SQL Database, güvenilirliği artırıldı **SqlConnection.Open** yöntemi. **Açık** yöntemi şimdi bağlantı zaman aşımı süresi içinde belirli hataları için en yüksek çaba yeniden deneme mekanizmaları yanıt geçici hataları içerir.
* Bağlantı havuzu, programınızı verir bağlantı nesnesi çalıştığından verimli bir doğrulama içeren desteklenir.

Bir bağlantı havuzundan bir bağlantı nesnesi kullandığınızda, bunu hemen kullanımda değilse programınız geçici olarak bağlantı kapatmanızı öneririz. Bir bağlantıyı yeniden pahalı değildir, ancak yeni bir bağlantı oluşturmaktır.

ADO.NET 4.0 kullanmak isterseniz daha önce en son ADO.NET yükseltmenizi öneririz. Kasım 2015'ten itibaren yapabilecekleriniz [ADO.NET 4.6.1 karşıdan](http://blogs.msdn.com/b/dotnet/archive/2015/11/30/net-framework-4-6-1-is-now-available.aspx).

<a id="e-diagnostics-test-utilities-connect" name="e-diagnostics-test-utilities-connect"></a>

## <a name="diagnostics"></a>Tanılama
<a id="d-test-whether-utilities-can-connect" name="d-test-whether-utilities-can-connect"></a>

### <a name="diagnostics-test-whether-utilities-can-connect"></a>Tanılama: yardımcı programlar bağlanıp bağlanamadığınızı Test
SQL veritabanına bağlanmak, program başarısız olursa bir tanılama yardımcı programı ile bağlanmayı denemek için bir seçenektir. İdeal olarak, yardımcı program programınızın kullandığı aynı kitaplığı kullanarak bağlanır.

Herhangi bir Windows bilgisayarda bu yardımcı programları deneyebilirsiniz:

* SQL Server Management ADO.NET kullanarak bağlayan Studio (ssms.exe)
* kullanarak bağlanan sqlcmd.exe [ODBC](http://msdn.microsoft.com/library/jj730308.aspx)

Programınızı bağlandıktan sonra kısa bir SQL SELECT sorgusu çalışır olup olmadığını test edin.

<a id="f-diagnostics-check-open-ports" name="f-diagnostics-check-open-ports"></a>

### <a name="diagnostics-check-the-open-ports"></a>Tanılama: açık bağlantı noktalarını denetleyin
Bağlantı denemeleri bağlantı sorunları nedeniyle başarısız şüpheleniyorsanız, bağlantı noktası yapılandırmalarını raporları, bilgisayarınızdaki bir yardımcı programı çalıştırabilirsiniz.

Linux üzerinde aşağıdaki yardımcı programlar yararlı olabilir:

* `netstat -nap`
* `nmap -sS -O 127.0.0.1`
  * Örnek değeri, IP adresi olacak şekilde değiştirin.

Windows, [PortQry.exe](http://www.microsoft.com/download/details.aspx?id=17148) yardımcı programı yararlı olabilir. Bir SQL veritabanı sunucusu bağlantı noktası durumunuza sorgulanan ve bir dizüstü bilgisayar üzerinde çalıştırıldığı bir örnek yürütme şöyledir:

```
[C:\Users\johndoe\]
>> portqry.exe -n johndoesvr9.database.windows.net -p tcp -e 1433

Querying target system called:
 johndoesvr9.database.windows.net

Attempting to resolve name to IP address...
Name resolved to 23.100.117.95

querying...
TCP port 1433 (ms-sql-s service): LISTENING

[C:\Users\johndoe\]
>>
```


<a id="g-diagnostics-log-your-errors" name="g-diagnostics-log-your-errors"></a>

### <a name="diagnostics-log-your-errors"></a>Tanılama: hatalarınızı günlük
Aralıklı bir sorunun bazen en iyi genel bir desen algılanması gün veya hafta üzerinden tanı koydu.

İstemci, bulduğu tüm hataları günlüğü tarafından bir tanılama yardımcı olabilir. Günlük girişlerini dahili olarak kendi SQL veritabanı günlük hata verileri ile ilişkilendirmek kullanabilir.

Kurumsal kitaplığı 6 (EntLib60) günlük kaydıyla yardımcı olmak için yönetilen .NET sınıfları sağlar. Daha fazla bilgi için bkz: [5 - bir oturum kapatma dönmeden olarak kadar kolay: günlük uygulama bloğu kullanmak](http://msdn.microsoft.com/library/dn440731.aspx).

<a id="h-diagnostics-examine-logs-errors" name="h-diagnostics-examine-logs-errors"></a>

### <a name="diagnostics-examine-system-logs-for-errors"></a>Tanılama: hatalar için sistem günlüklerini inceleyin
Burada, sorgu hata günlükleri ve diğer bilgileri bazı Transact-SQL SELECT deyimleri bulunmaktadır.

| Sorgu günlüğü | Açıklama |
|:--- |:--- |
| `SELECT e.*`<br/>`FROM sys.event_log AS e`<br/>`WHERE e.database_name = 'myDbName'`<br/>`AND e.event_category = 'connectivity'`<br/>`AND 2 >= DateDiff`<br/>&nbsp;&nbsp;`(hour, e.end_time, GetUtcDate())`<br/>`ORDER BY e.event_category,`<br/>&nbsp;&nbsp;`e.event_type, e.end_time;` |[Sys.event_log](http://msdn.microsoft.com/library/dn270018.aspx) görünümü geçici hataları veya bağlantısı hataları neden olabilir. bazı içeren tek tek olaylar hakkında bilgi sağlar.<br/><br/>İdeal olarak, ilişkilendirebilirsiniz **start_time** veya **end_time** ne zaman istemci programınız sorunlarla karşılaştınız hakkında bilgi değerlerle.<br/><br/>Bağlanmalısınız *ana* bu sorguyu çalıştırmak için veritabanı. |
| `SELECT c.*`<br/>`FROM sys.database_connection_stats AS c`<br/>`WHERE c.database_name = 'myDbName'`<br/>`AND 24 >= DateDiff`<br/>&nbsp;&nbsp;`(hour, c.end_time, GetUtcDate())`<br/>`ORDER BY c.end_time;` |[Sys.database_connection_stats](http://msdn.microsoft.com/library/dn269986.aspx) görünümü olay türleri için ek tanılama toplanmış sayısını sağlar.<br/><br/>Bağlanmalısınız *ana* bu sorguyu çalıştırmak için veritabanı. |

<a id="d-search-for-problem-events-in-the-sql-database-log" name="d-search-for-problem-events-in-the-sql-database-log"></a>

### <a name="diagnostics-search-for-problem-events-in-the-sql-database-log"></a>Tanılama: SQL veritabanı günlük sorun olayları arama
SQL veritabanı günlüğüne sorun olaylarıyla ilgili girdileri arayabilirsiniz. Aşağıdaki Transact-SQL SELECT deyiminde deneyin *ana* veritabanı:

```
SELECT
   object_name
  ,CAST(f.event_data as XML).value
      ('(/event/@timestamp)[1]', 'datetime2')                      AS [timestamp]
  ,CAST(f.event_data as XML).value
      ('(/event/data[@name="error"]/value)[1]', 'int')             AS [error]
  ,CAST(f.event_data as XML).value
      ('(/event/data[@name="state"]/value)[1]', 'int')             AS [state]
  ,CAST(f.event_data as XML).value
      ('(/event/data[@name="is_success"]/value)[1]', 'bit')        AS [is_success]
  ,CAST(f.event_data as XML).value
      ('(/event/data[@name="database_name"]/value)[1]', 'sysname') AS [database_name]
FROM
  sys.fn_xe_telemetry_blob_target_read_file('el', null, null, null) AS f
WHERE
  object_name != 'login_event'  -- Login events are numerous.
  and
  '2015-06-21' < CAST(f.event_data as XML).value
        ('(/event/@timestamp)[1]', 'datetime2')
ORDER BY
  [timestamp] DESC
;
```


#### <a name="a-few-returned-rows-from-sysfnxetelemetryblobtargetreadfile"></a>Sys.fn_xe_telemetry_blob_target_read_file birkaç döndürülen satırları
Aşağıdaki örnek, döndürülen satır aşağıdaki gibi görünmelidir gösterir. Gösterilen null değerleri diğer satırlardaki null değildir.

```
object_name                   timestamp                    error  state  is_success  database_name

database_xml_deadlock_report  2015-10-16 20:28:01.0090000  NULL   NULL   NULL        AdventureWorks
```


<a id="l-enterprise-library-6" name="l-enterprise-library-6"></a>

## <a name="enterprise-library-6"></a>Kurumsal kitaplığı 6
Kurumsal kitaplığı 6 (EntLib60) biri SQL veritabanı hizmeti olan bulut Hizmetleri, sağlam istemcileri uygulamanıza yardımcı olacak bir .NET sınıfları çerçevesidir. İçinde EntLib60 yardımcı olabilir her alanına ayrılmış konuları bulmak için bkz: [Kurumsal kitaplığı 6 - Nisan 2013](http://msdn.microsoft.com/library/dn169621%28v=pandp.60%29.aspx).

Geçici hataları işlemek için yeniden deneme mantığı EntLib60 yardımcı olabilecek bir alandır. Daha fazla bilgi için bkz: [4 - Perseverance, tüm triumphs sırrını: geçici hata işleme uygulama bloğu kullanmak](http://msdn.microsoft.com/library/dn440719%28v=pandp.60%29.aspx).

> [!NOTE]
> Ortak Merkezi'nden EntLib60 için kaynak kodunu kullanılabilir [Yükleme Merkezi'nden](http://go.microsoft.com/fwlink/p/?LinkID=290898). Microsoft, daha fazla özellik güncelleştirmeleri veya bakım güncelleştirmeleri için EntLib yapmak için herhangi bir plan sahiptir.
> 
> 

<a id="entlib60-classes-for-transient-errors-and-retry" name="entlib60-classes-for-transient-errors-and-retry"></a>

### <a name="entlib60-classes-for-transient-errors-and-retry"></a>Geçici hataları ve yeniden deneme için EntLib60 sınıfları
Aşağıdaki EntLib60 sınıfları yeniden deneme mantığı için özellikle yararlıdır. Bu sınıf veya ad alanı altında bulunur **Microsoft.Practices.EnterpriseLibrary.TransientFaultHandling**.

Ad alanında **Microsoft.Practices.EnterpriseLibrary.TransientFaultHandling**:

* **RetryPolicy** sınıfı
  
  * **ExecuteAction** yöntemi
* **ExponentialBackoff** sınıfı
* **SqlDatabaseTransientErrorDetectionStrategy** class
* **ReliableSqlConnection** sınıfı
  
  * **ExecuteCommand** yöntemi

Ad alanında **Microsoft.Practices.EnterpriseLibrary.TransientFaultHandling.TestSupport**:

* **AlwaysTransientErrorDetectionStrategy** class
* **NeverTransientErrorDetectionStrategy** sınıfı

Bazı bağlantılar EntLib60 hakkında bilgi için aşağıda verilmiştir:

* Boş kitap indirme: [Geliştirici Kılavuzu Microsoft Enterprise kitaplığa 2 sürümü](http://www.microsoft.com/download/details.aspx?id=41145).
* En iyi uygulamalar: [yeniden genel rehberlik](../best-practices-retry-general.md) mükemmel ayrıntılı incelemesi yeniden deneme mantığı vardır.
* NuGet indirme: [Kurumsal Library - geçici hata işleme uygulama bloğu 6.0](http://www.nuget.org/packages/EnterpriseLibrary.TransientFaultHandling/).

<a id="entlib60-the-logging-block" name="entlib60-the-logging-block"></a>

### <a name="entlib60-the-logging-block"></a>EntLib60: Günlük bloğu
* Günlük bloğu için kullanabileceğiniz bir yüksek oranda esnek ve yapılandırılabilir çözümüdür:
  
  * Oluşturabilir ve çok çeşitli konumları günlük iletilerini depolayabilir.
  * Kategorilere ayırmak ve iletilerini filtreleyebilirsiniz.
  * Hata ayıklama ve izlemenin yanı sıra denetlemek için kullanışlı ve genel günlüğü gereksinimleri bağlamsal bilgi toplayın.
* Uygulama kodu, konum ve türünü hedef günlük deposu bağımsız olarak tutarlı olmasını sağlamak günlük bloğu günlük hedefi günlüğe kaydetme işlevlerini soyutlar.

Daha fazla bilgi için bkz: [5 - bir oturum kapatma dönmeden olarak kadar kolay: günlük uygulama bloğu kullanmak](https://msdn.microsoft.com/library/dn440731%28v=pandp.60%29.aspx).

<a id="entlib60-istransient-method-source-code" name="entlib60-istransient-method-source-code"></a>

### <a name="entlib60-istransient-method-source-code"></a>EntLib60 IsTransient yöntemi kaynak kodu
Öğesinden sonraki **SqlDatabaseTransientErrorDetectionStrategy** sınıfı, C# kaynak kodu **IsTransient** yöntemi. Kaynak kodu hangi hataları geçici ve worthy Nisan 2013'dan sonra yeniden deneyin, kabul açıklar.

```csharp
public bool IsTransient(Exception ex)
{
  if (ex != null)
  {
    SqlException sqlException;
    if ((sqlException = ex as SqlException) != null)
    {
      // Enumerate through all errors found in the exception.
      foreach (SqlError err in sqlException.Errors)
      {
        switch (err.Number)
        {
            // SQL Error Code: 40501
            // The service is currently busy. Retry the request after 10 seconds.
            // Code: (reason code to be decoded).
          case ThrottlingCondition.ThrottlingErrorNumber:
            // Decode the reason code from the error message to
            // determine the grounds for throttling.
            var condition = ThrottlingCondition.FromError(err);

            // Attach the decoded values as additional attributes to
            // the original SQL exception.
            sqlException.Data[condition.ThrottlingMode.GetType().Name] =
              condition.ThrottlingMode.ToString();
            sqlException.Data[condition.GetType().Name] = condition;

            return true;

          case 10928:
          case 10929:
          case 10053:
          case 10054:
          case 10060:
          case 40197:
          case 40540:
          case 40613:
          case 40143:
          case 233:
          case 64:
            // DBNETLIB Error Code: 20
            // The instance of SQL Server you attempted to connect to
            // does not support encryption.
          case (int)ProcessNetLibErrorCode.EncryptionNotSupported:
            return true;
        }
      }
    }
    else if (ex is TimeoutException)
    {
      return true;
    }
    else
    {
      EntityException entityException;
      if ((entityException = ex as EntityException) != null)
      {
        return this.IsTransient(entityException.InnerException);
      }
    }
  }

  return false;
}
```


## <a name="next-steps"></a>Sonraki adımlar
* Diğer ortak SQL veritabanı bağlantı sorunlarını giderme hakkında daha fazla bilgi için bkz: [Azure SQL veritabanı bağlantı sorunlarını giderme](sql-database-troubleshoot-common-connection-issues.md).
* [SQL Database ve SQL Server için bağlantı kitaplıkları](sql-database-libraries.md)
* [SQL Server bağlantı havuzu (ADO.NET)](https://docs.microsoft.com/dotnet/framework/data/adonet/sql-server-connection-pooling)
* [*Yeniden deneniyor* lisanslı bir Apache 2.0 Python içinde yazılmış kitaplığı yeniden deneniyor genel amaçlı olduğundan getirin](https://pypi.python.org/pypi/retrying) hemen her şey için yeniden deneme davranışı ekleme görevini kolaylaştırmak için.


<!-- Link references. -->

[step-4-connect-resiliently-to-sql-with-ado-net-a78n]: https://docs.microsoft.com/sql/connect/ado-net/step-4-connect-resiliently-to-sql-with-ado-net

[step-4-connect-resiliently-to-sql-with-php-p42h]: https://docs.microsoft.com/sql/connect/php/step-4-connect-resiliently-to-sql-with-php

