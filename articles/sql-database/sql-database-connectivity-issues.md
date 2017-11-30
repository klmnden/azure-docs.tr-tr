---
title: "SQL bağlantı hatası, geçici hata düzeltme | Microsoft Docs"
description: "Sorun giderme, tanılama ve SQL bağlantı hatası veya Azure SQL veritabanındaki geçici hata önlemek öğrenin. "
keywords: "SQL bağlantısı, bağlantı dizesi, bağlantı sorunları, geçici bir hata oluştu, bağlantı hatası"
services: sql-database
documentationcenter: 
author: dalechen
manager: cshepard
editor: 
ms.assetid: efb35451-3fed-4264-bf86-72b350f67d50
ms.service: sql-database
ms.custom: develop apps
ms.workload: On Demand
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: troubleshooting
ms.date: 11/29/2017
ms.author: daleche
ms.openlocfilehash: 1db0dee597ffe60c587e7bacd00640a308d04e99
ms.sourcegitcommit: cfd1ea99922329b3d5fab26b71ca2882df33f6c2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/30/2017
---
# <a name="troubleshoot-diagnose-and-prevent-sql-connection-errors-and-transient-errors-for-sql-database"></a>SQL Database için SQL bağlantı hatalarını ve geçici hataları giderme, tanılama ve önleme
Bu makalede, engellemek, sorun giderme, tanılama ve bağlantı hataları ve Azure SQL veritabanı ile etkileşim kurarken, istemci uygulamanız karşılaştığında geçici hataları etkisini açıklar. Yeniden deneme mantığı yapılandırmak, bağlantı dizesi oluşturma ve diğer bağlantı ayarlarını öğrenin.

<a id="i-transient-faults" name="i-transient-faults"></a>

## <a name="transient-errors-transient-faults"></a>Geçici hataları geçici
Geçici bir hata - Ayrıca, geçici hata - kendisini yakında çözümleyecek bir temel nedeni vardır. Azure sistem hızla donanım kaynakları daha iyi yük dengelemek için çeşitli iş yükleri geçtiğinde geçici hataları zaman zaman bir nedenidir. Bu yeniden yapılandırma olayları çoğunu genellikle 60 saniyeden daha kısa bir süre içinde tamamlayın. Bu yeniden yapılandırma zaman aralığı sırasında Azure SQL veritabanı için bağlantı sorunu olabilir. Azure SQL veritabanına bağlanma uygulamaları yerleşik bu geçici hataları beklediğiniz, bunları kullanıcılara uygulama hataları olarak görünmesini yerine kendi kodunda yeniden deneme mantığı uygulayarak işlemek için.

İstemci programınızı ADO.NET kullanarak, programınızı throw tarafından geçici hata hakkında işe bir **SqlException**. **Numarası** özelliği, üst konu kısmına yakın geçici hataları listesine karşı karşılaştırılabilir: [SQL Database istemci uygulamaları için SQL hata kodları](sql-database-develop-error-messages.md).

<a id="connection-versus-command" name="connection-versus-command"></a>

### <a name="connection-versus-command"></a>Bağlantı komutu karşılaştırması
SQL bağlantısı yeniden deneme veya yeniden bağlı olarak aşağıdaki kurmak:

* **Bir bağlantı try sırasında geçici bir hata oluşur**: bağlantı geciktirme birkaç saniye sonra yeniden denenmesi gerekiyor.
* **Bir SQL sorgu komutu sırasında geçici bir hata oluşur**: komutu değil hemen yeniden denenmesi gerekiyor. Bunun yerine, bir gecikmeden sonra istemcinin yeniden bağlantı. Sonra komutu yeniden denenebilir.


<a id="j-retry-logic-transient-faults" name="j-retry-logic-transient-faults"></a>

## <a name="retry-logic-for-transient-errors"></a>Geçici hatalar için yeniden deneme mantığı
Yeniden deneme mantığı içerdiğinde bazen geçici bir hatayla karşılaşırsanız istemci programları daha sağlamdır.

Programınızı 3 bir taraf ara yazılımı Azure SQL veritabanı ile iletişim kurduğunda, geçici hataları yeniden deneme mantığı ara yazılım içerip içermediğini satıcıyla sorgulayın.

<a id="principles-for-retry" name="principles-for-retry"></a>

### <a name="principles-for-retry"></a>Yeniden deneme ilkelerini
* Geçici bir hatadır, bir bağlantı açmak için girişiminde denenmeli.
* Geçici bir hata ile başarısız olan bir SQL SELECT deyimi doğrudan denenmeli değil.
  
  * Bunun yerine, yeni bir bağlantı kurarak Seç yeniden deneyin.
* Bir SQL UPDATE deyimi geçici bir hata ile başarısız olduğunda, güncelleştirme denenen önce yeni bir bağlantı kurulmalıdır.
  
  * Yeniden deneme mantığı, tüm veritabanı işlem tamamlandı ya da, tüm işlem geri alındı emin olmalısınız.

### <a name="other-considerations-for-retry"></a>Yeniden deneme ile diğer değerlendirmeler
* Çalışma saatlerinden sonra otomatik olarak başlatılır ve sabah önce tamamlanır toplu iş programı uzun zaman aralıklarına kendi yeniden deneme girişimleri arasındaki ile çok hasta destekleyebilir.
* Bir kullanıcı arabirimi programı sonra çok uzun bekleme vermek İnsan eğilimi hesabı.
  
  * Ancak, bu ilkeyi istekleri sistemiyle bölgesini doldurmak için birkaç saniyede yeniden denemek için çözüm olmamalıdır.

### <a name="interval-increase-between-retries"></a>Denemeler arasındaki aralığı artırma
İlk, yeniden denemeden önce 5 saniye gecikme öneririz. Bulut hizmeti aşırı 5 saniye riskleri kısa bir gecikme sonrasında yeniden deneniyor. Gecikme büyümesine katlanarak, her sonraki yeniden deneme için en fazla 60 saniye.

Bir tartışma *engelleme süresi* ADO.NET kullanan istemciler için kullanılabilir [SQL Server bağlantı havuzu (ADO.NET)](http://msdn.microsoft.com/library/8xx3tyca.aspx).

Program otomatik olarak kapatılmadan önce yeniden deneme sayısı üst ayarlamak isteyebilirsiniz.

### <a name="code-samples-with-retry-logic"></a>Yeniden deneme mantığı ile kod örnekleri
Kod örnekleri yeniden deneme mantığı ile şu konumda mevcuttur:

- [SQL ADO.NET ile resiliently bağlanma][step-4-connect-resiliently-to-sql-with-ado-net-a78n]
- [PHP ile SQL resiliently bağlanma][step-4-connect-resiliently-to-sql-with-php-p42h]

<a id="k-test-retry-logic" name="k-test-retry-logic"></a>

### <a name="test-your-retry-logic"></a>Yeniden deneme mantığı test
Yeniden deneme mantığı sınamak için benzetimini yapmak veya programınız çalışmaya devam ederken düzeltilebilir daha hataya neden gerekir.

#### <a name="test-by-disconnecting-from-the-network"></a>Ağ bağlantısını keserek test
Yeniden deneme mantığı sınayabilirsiniz bir program çalışırken, istemci bilgisayar ağ bağlantısını kesmek için yoludur. Hata olacaktır:

* **SqlException.Number** 11001 =
* İleti: "gibi bir ana makine bilinmiyor"

İlk yeniden deneme girişimi bir parçası olarak programınızı yazım hatası düzeltin ve bağlanmayı dener.

Bu pratik yapmak için program başlamadan önce ağ bilgisayarınızdan çıkarın. Ardından programınızı programa neden olan bir çalışma zamanı parametre tanır:

1. Geçici olarak 11001 geçici dikkate alınması gereken hataları listesine ekleyin.
2. İlk bağlantı her zamanki gibi çalışır.
3. Hata belirledi sonra 11001 listeden kaldırın.
4. Bilgisayar ağa takın kullanıcıya söyleyen bir ileti görüntüler.
   * Daha fazla yürütme kullanarak duraklatmak **Console.ReadLine** yöntemi veya bir iletişim kutusunda Tamam düğmesi. Kullanıcı, bilgisayar ağa takılı sonra Enter tuşuna basar.
5. Yeniden bağlanma, başarı bekleniyor girişimi.

#### <a name="test-by-misspelling-the-database-name-when-connecting"></a>Veritabanı adı bağlanırken yazım hatası ile test
Programınızı kasıtlı olarak, hata ilk bağlantı denemesine önce kullanıcı adı hatalı. Hata olacaktır:

* **SqlException.Number** 18456 =
* İleti: "'WRONG_MyUserName' kullanıcı için oturum açma başarısız."

İlk yeniden deneme girişimi bir parçası olarak programınızı yazım hatası düzeltin ve bağlanmayı dener.

Bu pratik yapmak için programınızı programa neden olan bir çalışma zamanı parametre tanı:

1. Geçici olarak 18456 geçici dikkate alınması gereken hataları listesine ekleyin.
2. Kasıtlı olarak 'WRONG_' için kullanıcı adını ekleyin.
3. Hata belirledi sonra 18456 listeden kaldırın.
4. 'WRONG_' kullanıcı adını kaldırın.
5. Yeniden bağlanma, başarı bekleniyor girişimi.


<a id="net-sqlconnection-parameters-for-connection-retry" name="net-sqlconnection-parameters-for-connection-retry"></a>

## <a name="net-sqlconnection-parameters-for-connection-retry"></a>Bağlantı yeniden deneme için .NET SqlConnection parametreleri
İstemci programınız Azure SQL veritabanı için .NET Framework sınıf kullanarak bağlanırsa **System.Data.SqlClient.SqlConnection**, .NET 4.6.1 kullanması gereken veya daha sonra (veya .NET Core), bağlantı yeniden deneme özelliğinden yararlanabilirsiniz şekilde. Özelliğin ayrıntıları [burada](http://go.microsoft.com/fwlink/?linkid=393996).

<!--
2015-11-30, FwLink 393996 points to dn632678.aspx, which links to a downloadable .docx related to SqlClient and SQL Server 2014.
-->

Oluşturduğunuzda [bağlantı dizesi](http://msdn.microsoft.com/library/System.Data.SqlClient.SqlConnection.connectionstring.aspx) için **SqlConnection** nesne değerler aşağıdaki parametreleri arasında koordine:

* ConnectRetryCount &nbsp; &nbsp; *(varsayılan değer 1. Aralık 0 ile 255'dır.)*
* ConnectRetryInterval &nbsp; &nbsp; *(varsayılan değer 1 saniye. 1 ile 60 aralıktır.)*
* Bağlantı zaman aşımı &nbsp; &nbsp; *(varsayılan değer 15 saniye. Aralık 0 ile 2147483647'dır)*

Özellikle, seçilen değerlerinizi aşağıdaki eşitlik true olmanız gerekir:

* Bağlantı zaman aşımı ConnectRetryCount = * ConnectionRetryInterval

Örneğin, varsa sayısı = 3 ve aralığı 29 saniye oldukça sistem kendi 3 ve son yeniden bağlanma sırasında yeterli süre verirsiniz değil yalnızca = 10 saniye, bir zaman aşımı süresi: 29 < 3 * 10.

<a id="connection-versus-command" name="connection-versus-command"></a>

## <a name="connection-versus-command"></a>Bağlantı komutu karşılaştırması
**ConnectRetryCount** ve **ConnectRetryInterval** parametreleri sağlar, **SqlConnection** nesne söyleyen veya rahatsız etme bağlanma işlemi yeniden deneyin, program, programınıza denetimi döndürme gibi. Yeniden deneme aşağıdaki durumlarda oluşabilir:

* mySqlConnection.Open yöntem çağrısı
* mySqlConnection.Execute yöntem çağrısı

Bir subtlety yoktur. Geçici bir hata oluşursa karşın, *sorgu* yürütülmekte olan, **SqlConnection** nesnesi yeniden bağlanma işlemi ve bunu kesinlikle sorgunuzu yeniden denemez. Ancak, **SqlConnection** çok hızlı bir şekilde sorgu yürütme için göndermeden önce bağlantıyı denetler. Bir bağlantı sorunu hızlı onay algılarsa, **SqlConnection** bağlanma işlemini yeniden dener. Yeniden deneme başarılı olursa, sorgu yürütme için gönderilir.

### <a name="should-connectretrycount-be-combined-with-application-retry-logic"></a>Uygulama yeniden deneme mantığı ile ConnectRetryCount birleştirilmelidir?
Güçlü özel yeniden deneme mantığı uygulamanız olduğunu varsayalım. Bağlanma işlemi 4 kez yeniden deneyebilirler. Eklerseniz **ConnectRetryInterval** ve **ConnectRetryCount** = 3 bağlantı dizenizi 4 * 3 = 12 yeniden deneme sayısı artacaktır yeniden deneme sayısı. Yeniden deneme yüksek sayı düşündüğünüz değil.


<a id="a-connection-connection-string" name="a-connection-connection-string"></a>

## <a name="connections-to-azure-sql-database"></a>Azure SQL veritabanı bağlantıları
<a id="c-connection-string" name="c-connection-string"></a>

### <a name="connection-connection-string"></a>Bağlantı: Bağlantı dizesi
Azure SQL veritabanına bağlanmak için gerekli bağlantı dizesi, Microsoft SQL Server veritabanına bağlanmak için dize biraz farklıdır. Veritabanınızdan için bağlantı dizesini kopyalayabilirsiniz [Azure portal](https://portal.azure.com/).

[!INCLUDE [sql-database-include-connection-string-20-portalshots](../../includes/sql-database-include-connection-string-20-portalshots.md)]

<a id="b-connection-ip-address" name="b-connection-ip-address"></a>

### <a name="connection-ip-address"></a>Bağlantı: IP adresi
SQL veritabanı sunucusu, istemci programınızı barındıran bilgisayarın IP adresinden gelen iletişimi kabul edecek şekilde yapılandırmanız gerekir. Güvenlik Duvarı Ayarları'nda düzenleyerek bunu [Azure portal](https://portal.azure.com/).

IP adresini yapılandırmak unutursanız, programınızı gerekli IP adresi bildiren bir kullanışlı hata iletisi ile başarısız olur.

[!INCLUDE [sql-database-include-ip-address-22-portal](../../includes/sql-database-include-ip-address-22-v12portal.md)]

Daha fazla bilgi için bkz: [nasıl yapılır: SQL veritabanında güvenlik duvarı ayarlarını yapılandırma](sql-database-configure-firewall-settings.md)

<a id="c-connection-ports" name="c-connection-ports"></a>

### <a name="connection-ports"></a>Bağlantı: bağlantı noktaları
Genellikle, yalnızca bağlantı noktası 1433 istemci programı barındıran bilgisayarda giden iletişim için açık olduğundan emin olmanız gerekir.

İstemci programınızı bir Windows bilgisayarda barındırılıyorsa, örneğin, ana bilgisayardaki Windows Güvenlik Duvarı'nı bağlantı noktası 1433 açmanızı sağlar:

1. Denetim Masası'nı açın
2. &gt;Tüm Denetim Masası öğeleri
3. &gt;Windows Güvenlik Duvarı
4. &gt;Gelişmiş ayarlar
5. &gt;Giden kuralları
6. &gt;Eylemler
7. &gt;Yeni Kural

İstemci programınız Azure sanal makine (VM) barındırılıp barındırılmadığını, okumanız gerekir:<br/>[ADO.NET 4.5 ve SQL veritabanı için 1433 dışındaki bağlantı noktaları](sql-database-develop-direct-route-ports-adonet-v12.md).

Arka plan IP adresi ve bağlantı noktaları hakkında bilgi için cofiguration, bkz: [Azure SQL veritabanı güvenlik duvarı](sql-database-firewall-configure.md)

<a id="d-connection-ado-net-4-5" name="d-connection-ado-net-4-5"></a>

### <a name="connection-adonet-461"></a>Bağlantı: ADO.NET 4.6.1
ADO.NET sınıflarını gibi programınızı kullanıyorsa, **System.Data.SqlClient.SqlConnection** Azure SQL veritabanına bağlanmak için .NET Framework 4.6.1 sürümü kullanmanızı öneririz ya da daha yüksek.

ADO.NET 4.6.1:

* Azure SQL veritabanı için yapıldığında geliştirilmiş güvenilirlik kullanarak bir bağlantı açmak **SqlConnection.Open** yöntemi. **Açık** yöntemi şimdi en iyi çaba yeniden deneme geçici hataları için mekanizmaları bağlantı zaman aşımı süresi içinde belirli hataları içerir.
* Bağlantı havuzu destekler. Bu, program sunar bağlantı nesnesi çalıştığından verimli bir doğrulama içerir.

Bir bağlantı havuzundan bir bağlantı nesnesi kullandığınızda, programınızı geçici olarak bağlantı hemen kullanırken, kapatmanızı öneririz. Bir bağlantı yeniden açarak yeni bir bağlantı oluşturma yolu pahalı değil.

ADO.NET 4.0 kullanıyorsanız isterseniz daha önce en son ADO.NET yükseltmenizi öneririz.

* Kasım 2015'ten itibaren yapabilecekleriniz [ADO.NET 4.6.1 karşıdan](http://blogs.msdn.com/b/dotnet/archive/2015/11/30/net-framework-4-6-1-is-now-available.aspx).

<a id="e-diagnostics-test-utilities-connect" name="e-diagnostics-test-utilities-connect"></a>

## <a name="diagnostics"></a>Tanılama
<a id="d-test-whether-utilities-can-connect" name="d-test-whether-utilities-can-connect"></a>

### <a name="diagnostics-test-whether-utilities-can-connect"></a>Tanılama: yardımcı programlar bağlanıp bağlanamadığınızı Test
Azure SQL veritabanına bağlanmak, program başarısız olduysa, bir tanılama yardımcı programı ile bağlanmayı denemek için seçenektir. İdeal olarak yardımcı programı programınızın kullandığı aynı kitaplığı kullanarak bağlanabilir.

Herhangi bir Windows bilgisayarda bu yardımcı programları deneyebilirsiniz:

* SQL Server Management ADO.NET kullanarak bağlayan Studio (ssms.exe).
* kullanarak bağlanan sqlcmd.exe [ODBC](http://msdn.microsoft.com/library/jj730308.aspx).

Kısa bir SQL SELECT sorgusu çalışır olup olmadığını bağlandıktan sonra test edin.

<a id="f-diagnostics-check-open-ports" name="f-diagnostics-check-open-ports"></a>

### <a name="diagnostics-check-the-open-ports"></a>Tanılama: açık bağlantı noktalarını denetleyin
Bağlantı sorunları nedeniyle bağlantı girişimleri başarısız olduğunu düşündüğünüz varsayalım. Bilgisayarınızda, bağlantı noktası yapılandırmalarını raporları bir yardımcı programı çalıştırabilirsiniz.

Linux üzerinde aşağıdaki yardımcı programlar yararlı olabilir:

* `netstat -nap`
* `nmap -sS -O 127.0.0.1`
  * (Örnek değeri, IP adresi olacak şekilde değiştirin.)

Windows [PortQry.exe](http://www.microsoft.com/download/details.aspx?id=17148) yardımcı programı yararlı olabilir. Azure SQL Database sunucusu bağlantı noktası durumunuza sorgulanan ve, bir dizüstü bilgisayar üzerinde çalıştırıldığı bir örnek yürütme şöyledir:

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

İstemci, bulduğu tüm hataları günlüğü tarafından bir tanılama yardımcı olabilir. Günlük girişlerini dahili olarak kendisini Azure SQL veritabanı günlük hata verileri ile ilişkilendirmek kullanabilir.

Kurumsal kitaplığı 6 (EntLib60) günlük kaydıyla yardımcı olmak için yönetilen .NET sınıfları sunar:

* [5 - Oturumu Kapat dönmeden kadar kolay: günlük uygulama bloğu kullanma](http://msdn.microsoft.com/library/dn440731.aspx)

<a id="h-diagnostics-examine-logs-errors" name="h-diagnostics-examine-logs-errors"></a>

### <a name="diagnostics-examine-system-logs-for-errors"></a>Tanılama: hatalar için sistem günlüklerini inceleyin
Burada, hata, sorgu günlükleri ve diğer bilgileri bazı Transact-SQL SELECT deyimleri bulunmaktadır.

| Sorgu günlüğü | Açıklama |
|:--- |:--- |
| `SELECT e.*`<br/>`FROM sys.event_log AS e`<br/>`WHERE e.database_name = 'myDbName'`<br/>`AND e.event_category = 'connectivity'`<br/>`AND 2 >= DateDiff`<br/>&nbsp;&nbsp;`(hour, e.end_time, GetUtcDate())`<br/>`ORDER BY e.event_category,`<br/>&nbsp;&nbsp;`e.event_type, e.end_time;` |[Sys.event_log](http://msdn.microsoft.com/library/dn270018.aspx) görünümü geçici hataları veya bağlantısı hataları neden olabilir. bazı dahil olmak üzere ayrı ayrı olaylar hakkında bilgi sağlar.<br/><br/>İdeal olarak ilişkilendirebilirsiniz **start_time** veya **end_time** ne zaman istemci programınız sorunlarla karşılaştınız hakkında bilgi değerlerle.<br/><br/>**İpucu:** bağlanmalısınız **ana** bunu çalıştırmak için veritabanı. |
| `SELECT c.*`<br/>`FROM sys.database_connection_stats AS c`<br/>`WHERE c.database_name = 'myDbName'`<br/>`AND 24 >= DateDiff`<br/>&nbsp;&nbsp;`(hour, c.end_time, GetUtcDate())`<br/>`ORDER BY c.end_time;` |[Sys.database_connection_stats](http://msdn.microsoft.com/library/dn269986.aspx) görünümü olay türleri, ek tanılama için toplanan sayısını sağlar.<br/><br/>**İpucu:** bağlanmalısınız **ana** bunu çalıştırmak için veritabanı. |

<a id="d-search-for-problem-events-in-the-sql-database-log" name="d-search-for-problem-events-in-the-sql-database-log"></a>

### <a name="diagnostics-search-for-problem-events-in-the-sql-database-log"></a>Tanılama: SQL veritabanı günlük sorun olayları arama
Azure SQL veritabanı'nın günlükteki sorun olaylarıyla ilgili girdileri arayabilirsiniz. Aşağıdaki Transact-SQL SELECT deyiminde deneyin **ana** veritabanı:

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
Sonraki döndürülen satır aşağıdaki gibi görünmelidir olduğu. Gösterilen null değerleri diğer satırlardaki null değildir.

```
object_name                   timestamp                    error  state  is_success  database_name

database_xml_deadlock_report  2015-10-16 20:28:01.0090000  NULL   NULL   NULL        AdventureWorks
```


<a id="l-enterprise-library-6" name="l-enterprise-library-6"></a>

## <a name="enterprise-library-6"></a>Kurumsal kitaplığı 6
Kurumsal kitaplığı 6 (EntLib60) biri Azure SQL veritabanı hizmeti olan bulut Hizmetleri, sağlam istemcileri uygulamanıza yardımcı olacak bir .NET sınıfları çerçevesidir. İlk ziyaret ederek EntLib60 yardımcı olabilecek her alanına ayrılmış konuları bulabilirsiniz:

* [Kurumsal kitaplığı 6 - Nisan 2013](http://msdn.microsoft.com/library/dn169621%28v=pandp.60%29.aspx)

Geçici hataları işlemek için yeniden deneme mantığı EntLib60 yardımcı olabilecek bir alandır:

* [4 - perseverance, tüm Triumphs sırrını: geçici hata işleme uygulama blok kullanma](http://msdn.microsoft.com/library/dn440719%28v=pandp.60%29.aspx)

> [!NOTE]
> Kaynak kodu EntLib60 için kullanılabilir ortak için [karşıdan](http://go.microsoft.com/fwlink/p/?LinkID=290898). Microsoft, daha fazla özellik güncelleştirmeleri veya bakım güncelleştirmeleri için EntLib yapmak için herhangi bir plan sahiptir.
> 
> 

<a id="entlib60-classes-for-transient-errors-and-retry" name="entlib60-classes-for-transient-errors-and-retry"></a>

### <a name="entlib60-classes-for-transient-errors-and-retry"></a>Geçici hataları ve yeniden deneme için EntLib60 sınıfları
Aşağıdaki EntLib60 sınıfları yeniden deneme mantığı için özellikle yararlıdır. Tüm bunlar içinde bulunan veya ad alanı altında daha fazla **Microsoft.Practices.EnterpriseLibrary.TransientFaultHandling**:

*Ad alanında **Microsoft.Practices.EnterpriseLibrary.TransientFaultHandling**:*

* **RetryPolicy** sınıfı
  
  * **ExecuteAction** yöntemi
* **ExponentialBackoff** sınıfı
* **SqlDatabaseTransientErrorDetectionStrategy** sınıfı
* **ReliableSqlConnection** sınıfı
  
  * **ExecuteCommand** yöntemi

Ad alanında **Microsoft.Practices.EnterpriseLibrary.TransientFaultHandling.TestSupport**:

* **AlwaysTransientErrorDetectionStrategy** sınıfı
* **NeverTransientErrorDetectionStrategy** sınıfı

Aşağıda, EntLib60 hakkında bilgilere bağlantılar verilmiştir:

* Ücretsiz [kitap indirme: Microsoft Enterprise kitaplığa 2 sürümü Geliştirici Kılavuzu](http://www.microsoft.com/download/details.aspx?id=41145)
* En iyi uygulamalar: [yeniden genel rehberlik](../best-practices-retry-general.md) mükemmel ayrıntılı incelemesi yeniden deneme mantığı vardır.
* NuGet yüklenmesini [Kurumsal Library - uygulama bloğu 6.0 geçici hata işleme](http://www.nuget.org/packages/EnterpriseLibrary.TransientFaultHandling/)

<a id="entlib60-the-logging-block" name="entlib60-the-logging-block"></a>

### <a name="entlib60-the-logging-block"></a>EntLib60: Günlük bloğu
* Günlük bloğu izin veren oldukça esnek ve yapılandırılabilir bir çözümdür:
  
  * Oluşturabilir ve çok çeşitli konumları günlük iletilerini depolayabilir.
  * Kategorilere ayırmak ve iletilerini filtreleyebilirsiniz.
  * Hata ayıklama ve izlemenin yanı sıra denetlemek için kullanışlı ve genel günlüğü gereksinimleri bağlamsal bilgi toplayın.
* Uygulama kodu, konum ve türünü hedef günlük deposu bağımsız olarak tutarlı olmasını sağlamak günlük bloğu günlük hedefi günlüğe kaydetme işlevlerini soyutlar.

Ayrıntılar için bkz: [5 - olarak kolay olarak dönmeden kapalı bir günlük: günlük uygulama bloğu kullanma](https://msdn.microsoft.com/library/dn440731%28v=pandp.60%29.aspx)

<a id="entlib60-istransient-method-source-code" name="entlib60-istransient-method-source-code"></a>

### <a name="entlib60-istransient-method-source-code"></a>EntLib60 IsTransient yöntemi kaynak kodu
Öğesinden sonraki **SqlDatabaseTransientErrorDetectionStrategy** sınıfı, C# kaynak kodu **IsTransient** yöntemi. Kaynak kodu hangi hataları geçici ve Yeniden Dene'yi, Nisan 2013'ten itibaren worthy olarak kabul açıklar.

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
* Diğer ortak Azure SQL veritabanı bağlantı sorunlarını gidermek için ziyaret [Azure SQL veritabanı bağlantı sorunlarını giderme](sql-database-troubleshoot-common-connection-issues.md).
* [SQL Database ve SQL Server için bağlantı kitaplıkları](sql-database-libraries.md)
* [SQL Server bağlantı (ADO.NET) havuzu](https://docs.microsoft.com/dotnet/framework/data/adonet/sql-server-connection-pooling)
* [*Yeniden deneniyor* lisanslı bir Apache 2.0 yazılan kitaplığı yeniden deneniyor genel amaçlı olduğundan getirin **Python**, yeniden deneme davranışı için neredeyse her şeyi ekleme görevini kolaylaştırmak için.](https://pypi.python.org/pypi/retrying)


<!-- Link references. -->

[step-4-connect-resiliently-to-sql-with-ado-net-a78n]: https://docs.microsoft.com/sql/connect/ado-net/step-4-connect-resiliently-to-sql-with-ado-net

[step-4-connect-resiliently-to-sql-with-php-p42h]: https://docs.microsoft.com/sql/connect/php/step-4-connect-resiliently-to-sql-with-php

