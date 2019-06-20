---
title: Geçici hataları - Azure SQL veritabanı ile çalışma | Microsoft Docs
description: Sorun giderme, tanılama ve bir SQL bağlantı hatası veya Azure SQL veritabanında geçici hata önleme konusunda bilgi edinin.
keywords: SQL bağlantı, bağlantı dizesi, bağlantı sorunları, geçici bir hata oluştu, bağlantı hatası
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: dalechen
ms.author: ninarn
ms.reviewer: carlrab
manager: craigg
ms.date: 06/14/2019
ms.openlocfilehash: adbe8dfd41725c11516f820656b0476ed1aa8881
ms.sourcegitcommit: 22c97298aa0e8bd848ff949f2886c8ad538c1473
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/14/2019
ms.locfileid: "67144045"
---
# <a name="working-with-sql-database-connection-issues-and-transient-errors"></a>SQL veritabanı bağlantı sorunlarını ve geçici hatalar ile çalışma

Bu makalede, engellemek, sorun giderme, tanılama ve bağlantı hatalarını ve Azure SQL veritabanı ile etkileşim kurduğunda, istemci uygulamanızın karşılaştığı geçici hataları etkisini açıklar. Yeniden deneme mantığını yapılandırın, bağlantı dizesi oluşturma ve diğer bağlantı ayarlarını öğrenin.

<a id="i-transient-faults" name="i-transient-faults"></a>

## <a name="transient-errors-transient-faults"></a>Geçici hatalar (geçici hatalar)

Geçici bir hata olarak da bilinen bir geçici hata kendisini yakında gideren bir temel nedeni vardır. Azure sistem hızla donanım kaynaklarını daha iyi yük dengelemek için çeşitli iş yükleri geçtiğinde bir zaman geçici hataların nedeni. Çoğu bu yeniden yapılandırma olayları, 60 saniyeden kısa bir süre içinde tamamlayın. Bu yeniden yapılandırma zaman aralığı sırasında SQL veritabanı bağlantısı sorunları olabilir. SQL veritabanına bağlanan uygulamalar, bu geçici hatalara beklenir oluşturulmalıdır. Bunları işlemek için bunları kullanıcılara uygulama hataları olarak görünmesini yerine kendi kodunda yeniden deneme mantığı uygulayın.

İstemci programınız ADO.NET kullanıyorsa, programınız tarafından throw ve geçici bir hatayla ilgili bildirilir **SqlException**. Karşılaştırma **numarası** özelliği makalenin üst kısımda bulunan geçici hataları listesine karşı [SQL Database istemci uygulamaları için SQL hata kodları](sql-database-develop-error-messages.md).

<a id="connection-versus-command" name="connection-versus-command"></a>

### <a name="connection-vs-command"></a>Bağlantı komutu karşılaştırması

SQL bağlantısı yeniden deneme veya yeniden bağlı olarak aşağıdaki kurmak:

- **Bir bağlantı try sırasında geçici bir hata oluşur**

Birkaç saniyelik bir gecikmenin ardından bağlantıyı yeniden deneyin.

- **Bir SQL sorgu komutu sırasında geçici bir hata oluşuyor**

Hemen komutu yeniden denemeyin. Bunun yerine, bir gecikmeden sonra yeni bağlantı kurun. Ardından komutu yeniden deneyin.

<a id="j-retry-logic-transient-faults" name="j-retry-logic-transient-faults"></a>

## <a name="retry-logic-for-transient-errors"></a>Geçici hatalar için yeniden deneme mantığı

Bazen geçici bir hatayla karşılaşırsanız istemci programları daha sağlam alındığında bunlar yeniden deneme mantığı içerir. Programınızı üçüncü taraf ara yazılım SQL veritabanı ile iletişim kurduğunda, ara yazılım geçici hatalar için yeniden deneme mantığı içerip içermediğini satıcı isteyin.

<a id="principles-for-retry" name="principles-for-retry"></a>

### <a name="principles-for-retry"></a>Yeniden deneme ilkelerini

- Bir bağlantı açmak için geçici bir hatadır, yeniden deneyin.
- Bir SQL doğrudan yeniden denemeyin `SELECT` deyimi bir geçici hata ile başarısız oldu. Bunun yerine, yeni bir bağlantı kurmak ve sonra yeniden deneyin `SELECT`.
- Bir SQL zaman `UPDATE` deyimi bir geçici hata ile başarısız oluyor, GÜNCELLEŞTİRMEYİ yeniden denemeden önce yeni bir bağlantı kurun. Yeniden deneme mantığı, tüm veritabanı ya da bir işlemin tamamlandığını veya işlemin tümü geri alınır emin olmalısınız.

### <a name="other-considerations-for-retry"></a>Yeniden deneme ile diğer değerlendirmeler

- Otomatik olarak çalışma saatlerinden sonra başlar ve sabah çok uzun aralıklarla kendi yeniden deneme girişimleri arasındaki hasta edilebildiği önce bittikten batch program.
- Bir kullanıcı arabirimi program sonra uzun bekleme vermek İnsan eğilimi hesabı. Bu ilke isteklerinin sistemiyle doldurmak çünkü çözümü her birkaç saniyede yeniden gerekir.

### <a name="interval-increase-between-retries"></a>Yeniden denemeler arasındaki aralığı artış

İlk uygulamanızı yeniden denemeden önce 5 saniye bekleyin öneririz. Bulut hizmeti aşırı yüklenilmesini 5 saniye riskleri kısa bir gecikmeden sonra yeniden deneniyor. Sonraki her yeniden deneme için gecikme büyümesini katlanarak, en fazla 60 saniye kadar.

ADO.NET kullanan istemciler için engelleme süresi bir tartışma için bkz [SQL Server connection pooling (ADO.NET)](https://msdn.microsoft.com/library/8xx3tyca.aspx).

Programın kendini sonlandırılmadan önce bir yeniden deneme sayısı ayarlamak isteyebilirsiniz.

### <a name="code-samples-with-retry-logic"></a>Yeniden deneme mantığı ile kod örnekleri

Yeniden deneme mantığı ile kod örnekleri, şu konumda mevcuttur:

- [Dayanıklı ADO.NET ile SQL bağlantısı kurma][step-4-connect-resiliently-to-sql-with-ado-net-a78n]
- [Dayanıklı PHP ile SQL bağlantısı kurma][step-4-connect-resiliently-to-sql-with-php-p42h]

<a id="k-test-retry-logic" name="k-test-retry-logic"></a>

### <a name="test-your-retry-logic"></a>Yeniden deneme mantığınız test

Yeniden deneme mantığınız test etmek için benzetimini yapmak veya programınızın çalışmaya devam ederken düzeltilebilir hata neden.

#### <a name="test-by-disconnecting-from-the-network"></a>Ağ bağlantısını keserek test

Yeniden deneme mantığınız test edebilirsiniz bir program çalışırken, istemci bilgisayar ağ bağlantısını kesmek için yoludur. Hata oluşur:

- **SqlException.Number** 11001 =
- İleti: "Böyle bir ana makine bilinmiyor"

İlk yeniden deneme girişimi bir parçası olarak, istemci bilgisayar ağa yeniden ve bağlanma girişimi.

Bu test pratik hale getirmek için programınızı başlamadan önce ağ üzerinden bilgisayarınıza çıkarın. Ardından, programınızın programa neden olan bir çalışma zamanı parametre tanır:

- Geçici olarak 11001 geçici dikkate alınması gereken hataları listesine ekleyin.
- İlk bağlantısını her zaman olduğu gibi çalışır.
- Hata yakalandı sonra 11001 listeden kaldırın.
- Kullanıcı, bilgisayar ağa bağlamak için bildiren bir ileti görüntüler.
- Daha fazla yürütme kullanarak duraklatmak **Console.ReadLine** yöntemi veya bir iletişim kutusunda Tamam düğmesi. Kullanıcı, bilgisayar ağa takılı sonra Enter tuşuna basar.
- Yeniden bağlanmak başarı bekleniyor deneyin.

#### <a name="test-by-misspelling-the-database-name-when-connecting"></a>Veritabanı adı bağlanırken yazım hatası ile test

Programınızı bilerek kullanıcı adından önce ilk bağlantı girişimi yazsanız. Hata oluşur:

- **SqlException.Number** = 18456
- İleti: "Oturum açma 'WRONG_MyUserName' kullanıcı için başarısız oldu."

Programınızı ilk yeniden deneme girişimi bir parçası olarak, yazım hataları düzeltin ve bağlanma girişimi.

Bu test pratik hale getirmek için programa neden olan bir çalışma zamanı parametre programınızı tanır:

- Geçici olarak 18456 geçici dikkate alınması gereken hataları listesine ekleyin.
- Bilerek 'WRONG_' kullanıcı adını ekleyin.
- Hata yakalandı sonra 18456 listeden kaldırın.
- 'WRONG_' kullanıcı adından kaldırın.
- Yeniden bağlanmak başarı bekleniyor deneyin.

<a id="net-sqlconnection-parameters-for-connection-retry" name="net-sqlconnection-parameters-for-connection-retry"></a>

## <a name="net-sqlconnection-parameters-for-connection-retry"></a>Bağlantı yeniden deneme için .NET SqlConnection parametreleri

.NET Framework sınıfı kullanarak, istemci programınızın SQL veritabanı'na bağlanır, **System.Data.SqlClient.SqlConnection**, .NET 4.6.1 kullanın veya üzeri (veya .NET Core) böylece onun bağlantı yeniden deneme özelliği kullanabilirsiniz. Bu özellik hakkında daha fazla bilgi için bkz. [bu Web sayfasını](https://docs.microsoft.com/dotnet/api/system.data.sqlclient.sqlconnection).

<!--
2015-11-30, FwLink 393996 points to dn632678.aspx, which links to a downloadable .docx related to SqlClient and SQL Server 2014.
-->

Oluşturduğunuzda [bağlantı dizesi](https://msdn.microsoft.com/library/System.Data.SqlClient.SqlConnection.connectionstring.aspx) için **SqlConnection** nesne, aşağıdaki parametreleri arasında değerleri koordine:

- **ConnectRetryCount**:&nbsp;&nbsp;varsayılan 1'dir. 0 ile 255 arasındaki aralıktır.
- **ConnectRetryInterval**:&nbsp;&nbsp;10 saniye varsayılandır. 1 ila 60 aralığı.
- **Bağlantı zaman aşımı**:&nbsp;&nbsp;15 saniye varsayılandır. Aralığı 0 ile 2147483647 arasındadır.

Özellikle, seçilen değerlerinizin aşağıdaki eşitlik true yapmanız gerekir: Bağlantı zaman aşımı ConnectRetryCount = * ConnectionRetryInterval

Örneğin, 3 sayısı eşittir ve aralığı 10 saniye, yalnızca 29 saniyelik bir zaman aşımı sistemin, üçüncü ve son yeniden bağlanmak yeterli zaman vermez: 29 < 3 * 10.

<a id="connection-versus-command" name="connection-versus-command"></a>

## <a name="connection-vs-command"></a>Bağlantı komutu karşılaştırması

**ConnectRetryCount** ve **ConnectRetryInterval** parametreleri sağlar, **SqlConnection** nesne belirten veya rahatsız etme bağlanma işlemi yeniden deneyin, program, programınız için denetimi döndürme gibi. Yeniden deneme işlemleri, aşağıdaki durumlarda oluşabilir:

- mySqlConnection.Open yöntem çağrısı
- mySqlConnection.Execute yöntem çağrısı

Bir subtlety yoktur. Geçici bir hata oluşursa sırada, *sorgu* yürütüldüğü, uygulamanızın **SqlConnection** nesne değil bağlanma işlemi yeniden deneyin. Kesinlikle sorgunuzu yeniden değil. Ancak, **SqlConnection** çok hızlı bir şekilde sorgu yürütme için göndermeden önce bağlantıyı denetler. Bir bağlantı sorunu hızlı onay algılarsa **SqlConnection** bağlanma işlemini yeniden dener. Yeniden deneme başarılı olursa, sorgu yürütme için gönderilir.

### <a name="should-connectretrycount-be-combined-with-application-retry-logic"></a>Uygulama yeniden deneme mantığı ile ConnectRetryCount birleştirilmesi gerekir

Güçlü özel yeniden deneme mantığı uygulamanız olduğunu varsayalım. Dört kez yeniden bağlanma işlemi. Eklerseniz **ConnectRetryInterval** ve **ConnectRetryCount** = 3 bağlantı dizenizi 4 * 3 = 12 yeniden deneme sayısı artacaktır. yeniden deneme sayısı. Böyle çok sayıda yeniden deneme düşündüğünüz değil.

<a id="a-connection-connection-string" name="a-connection-connection-string"></a>

## <a name="connections-to-sql-database"></a>SQL veritabanı bağlantısı

<a id="c-connection-string" name="c-connection-string"></a>

### <a name="connection-connection-string"></a>Bağlantı: Bağlantı dizesi

SQL veritabanına bağlanmak gerekli olan bağlantı dizesi, SQL Server'a bağlanmak için kullanılan dize biraz farklıdır. Veritabanınızdan için bağlantı dizesini kopyalayabilirsiniz [Azure portalında](https://portal.azure.com/).

[!INCLUDE [sql-database-include-connection-string-20-portalshots](../../includes/sql-database-include-connection-string-20-portalshots.md)]

<a id="b-connection-ip-address" name="b-connection-ip-address"></a>

### <a name="connection-ip-address"></a>Bağlantı: IP adresi

SQL veritabanı sunucusu, istemci programınızı barındıran bilgisayarın IP adresinden gelen iletişimi kabul edecek şekilde yapılandırmanız gerekir. Bu yapılandırmayı ayarlamak için Güvenlik Duvarı Ayarları'nda Düzenle [Azure portalında](https://portal.azure.com/).

IP adresini yapılandırmak unutursanız, programınızı gerekli IP adresi belirten kullanışlı hata iletisiyle başarısız olur.

[!INCLUDE [sql-database-include-ip-address-22-portal](../../includes/sql-database-include-ip-address-22-v12portal.md)]

Daha fazla bilgi için [SQL Database'de güvenlik duvarı ayarlarını yapılandırma](sql-database-configure-firewall-settings.md).
<a id="c-connection-ports" name="c-connection-ports"></a>

### <a name="connection-ports"></a>Bağlantı: Bağlantı Noktaları

Genellikle, yalnızca bağlantı noktası 1433'ten istemci programınızı barındıran bilgisayarda giden iletişim için açık olduğundan emin olmak gerekir.

Bir Windows bilgisayarda, istemci programınızın barındırıldığında, örneğin, Windows Güvenlik Duvarı konakta 1433 numaralı bağlantı noktasını açmak için kullanabilirsiniz.

1. Denetim Masası'nı açın.
2. Seçin **tüm Denetim Masası öğeleri** > **Windows Güvenlik Duvarı** > **Gelişmiş ayarlar** > **giden kuralları**   >  **Eylemleri** > **yeni kural**.

İstemci programınız bir Azure sanal makine (VM) üzerinde barındırılıyorsa, okuma [ADO.NET 4.5 ve SQL veritabanı için 1433 dışındaki bağlantı noktaları](sql-database-develop-direct-route-ports-adonet-v12.md).

Arka plan bağlantı noktalarını ve IP adreslerinin yapılandırma hakkında bilgi için [Azure SQL veritabanı güvenlik duvarını](sql-database-firewall-configure.md).

<a id="d-connection-ado-net-4-5" name="d-connection-ado-net-4-5"></a>

### <a name="connection-adonet-462-or-later"></a>Bağlantı: ADO.NET 4.6.2 veya üzeri

Programınızı gibi ADO.NET sınıflarını kullanıyorsa **System.Data.SqlClient.SqlConnection** SQL veritabanına bağlanmak için .NET Framework sürüm 4.6.2'yi kullanmanızı öneririz veya üzeri.

#### <a name="starting-with-adonet-462"></a>ADO.NET ile 4.6.2 başlatılıyor

- Azure SQL veritabanları için böylece bulut özellikli uygulamalar performansını geliştirmeye hemen yeniden denenmesi bağlantı açık girişimi.

#### <a name="starting-with-adonet-461"></a>ADO.NET ile 4.6.1 başlatılıyor

- SQL veritabanı için bir bağlantı kullanarak açtığınızda güvenilirliği artırıldı **SqlConnection.Open** yöntemi. **Açık** yöntemi artık bağlantı zaman aşımı süresi içinde belirli hataları için en yüksek çaba yeniden deneme mekanizmaları geçici hatalar için yanıt içerir.
- Bağlantı havuzu, programınızı sağlar bağlantı nesnesi çalıştığını verimli bir doğrulama içeren desteklenir.

Bir bağlantı havuzundan bir bağlantı nesnesi kullandığınızda, bunu hemen kullanımda değilse, programınızın geçici olarak bağlantıyı kapatır öneririz. Bir bağlantıyı yeniden açmak pahalı değil, ancak yeni bir bağlantı oluşturmaktır.

ADO.NET 4.0 kullanın veya daha önce en son ADO.NET'e yükseltmenizi öneririz olur. Ağustos 2018'den itibaren yapabilecekleriniz [ADO.NET 4.6.2 indirme](https://blogs.msdn.microsoft.com/dotnet/20../../announcing-the-net-framework-4-7-2/).

<a id="e-diagnostics-test-utilities-connect" name="e-diagnostics-test-utilities-connect"></a>

## <a name="diagnostics"></a>Tanılama

<a id="d-test-whether-utilities-can-connect" name="d-test-whether-utilities-can-connect"></a>

### <a name="diagnostics-test-whether-utilities-can-connect"></a>Tanılama: Yardımcı programları bağlanıp bağlanamadığınızı test

SQL veritabanına bağlanmak, programınızın başarısız olursa, bir tanılama bir yardımcı program ile bağlanmayı denemek için seçenektir. İdeal olarak, yardımcı programınız kullanır aynı kitaplığını kullanarak bağlanır.

Herhangi bir Windows bilgisayarda, bu yardımcı programlar deneyebilirsiniz:

- ADO.NET kullanarak bağlayan Ssms'yi (ssms.exe)
- `sqlcmd.exe`, bağlayan kullanarak [ODBC](https://msdn.microsoft.com/library/jj730308.aspx)

Programınızı bağlandıktan sonra kısa bir SQL SELECT sorgusu çalışıp çalışmadığını test edin.

<a id="f-diagnostics-check-open-ports" name="f-diagnostics-check-open-ports"></a>

### <a name="diagnostics-check-the-open-ports"></a>Tanılama: Bağlantı noktalarını açma denetleyin

Bağlantı denemeleri bağlantı sorunları nedeniyle başarısız şüpheleriniz varsa, bağlantı noktası yapılandırmalarını raporları, bilgisayarınızdaki bir yardımcı çalıştırabilirsiniz.

Linux üzerinde aşağıdaki yardımcı programlar yararlı olabilir:

- `netstat -nap`
- `nmap -sS -O 127.0.0.1`: IP adresiniz için örnek değeri değiştirin.

Windows, şirket [PortQry.exe](https://www.microsoft.com/download/details.aspx?id=17148) yardımcı programı yararlı olabilir. SQL veritabanı sunucusu bağlantı noktası durumunuza sorgulanabilir ve, bir dizüstü bilgisayarda çalıştırılan örnek yürütme şu şekildedir:

```cmd
[C:\Users\johndoe\]
>> portqry.exe -n johndoesvr9.database.windows.net -p tcp -e 1433

Querying target system called: johndoesvr9.database.windows.net

Attempting to resolve name to IP address...
Name resolved to 23.100.117.95

querying...
TCP port 1433 (ms-sql-s service): LISTENING

[C:\Users\johndoe\]
>>
```

<a id="g-diagnostics-log-your-errors" name="g-diagnostics-log-your-errors"></a>

### <a name="diagnostics-log-your-errors"></a>Tanılama: Oturum, hataları

Aralıklı bir sorunun bazen en iyi bir genel düzen algılanması günler veya haftalar içinde tanı koydu.

İstemcinizi bulduğu tüm hataları oturum açarak tanılama aşamasında size yardımcı olabilir. Dahili olarak kendi SQL veritabanı günlük hata verilerle günlük girişlerini ilişkilendirebilmesi olabilir.

Kurumsal kitaplığı 6 (EntLib60) günlük kaydı ile yardımcı olmak için .NET yönetilen sınıflar sağlar. Daha fazla bilgi için [5 - kadar kolay bir oturum kapatma denk gelen olarak: Günlük uygulama bloğu kullanın](https://msdn.microsoft.com/library/dn440731.aspx).

<a id="h-diagnostics-examine-logs-errors" name="h-diagnostics-examine-logs-errors"></a>

### <a name="diagnostics-examine-system-logs-for-errors"></a>Tanılama: Hatalar için sistem günlüklerini inceleyin

Sorgu hata günlüklerini ve diğer bilgileri bazı Transact-SQL SELECT deyimi aşağıda verilmiştir.

| Sorgu günlüğü | Açıklama |
|:--- |:--- |
| `SELECT e.*`<br/>`FROM sys.event_log AS e`<br/>`WHERE e.database_name = 'myDbName'`<br/>`AND e.event_category = 'connectivity'`<br/>`AND 2 >= DateDiff`<br/>&nbsp;&nbsp;`(hour, e.end_time, GetUtcDate())`<br/>`ORDER BY e.event_category,`<br/>&nbsp;&nbsp;`e.event_type, e.end_time;` |[Sys.event_log](https://msdn.microsoft.com/library/dn270018.aspx) görünümü geçici hatalar ya da bağlantısı hataları neden olabilecek bazı içeren tek tek olayları hakkında bilgi sağlar.<br/><br/>İdeal olarak, ilişkilendirebilmek **start_time** veya **end_time** ne zaman istemci programınız sorunlarla karşılaştınız hakkında bilgi değerleri.<br/><br/>Bağlanmanız gerekir *ana* bu sorguyu çalıştırmak için veritabanı. |
| `SELECT c.*`<br/>`FROM sys.database_connection_stats AS c`<br/>`WHERE c.database_name = 'myDbName'`<br/>`AND 24 >= DateDiff`<br/>&nbsp;&nbsp;`(hour, c.end_time, GetUtcDate())`<br/>`ORDER BY c.end_time;` |[Sys.database_connection_stats](https://msdn.microsoft.com/library/dn269986.aspx) görünümü, toplanan olay türleri için ek tanılama sayısını sağlar.<br/><br/>Bağlanmanız gerekir *ana* bu sorguyu çalıştırmak için veritabanı. |

<a id="d-search-for-problem-events-in-the-sql-database-log" name="d-search-for-problem-events-in-the-sql-database-log"></a>

### <a name="diagnostics-search-for-problem-events-in-the-sql-database-log"></a>Tanılama: SQL veritabanı günlüğünde sorun olayları arayın

SQL veritabanı günlük sorun olaylar hakkında girdiler için arama yapabilirsiniz. Aşağıdaki Transact-SQL SELECT deyimi deneyin *ana* veritabanı:

```sql
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

Aşağıdaki örnek, döndürülen satır aşağıdaki gibi görünmelidir gösterir. Gösterilen null değerleri diğer satırlarda boş değildir.

```
object_name                   timestamp                    error  state  is_success  database_name

database_xml_deadlock_report  2015-10-16 20:28:01.0090000  NULL   NULL   NULL        AdventureWorks
```

<a id="l-enterprise-library-6" name="l-enterprise-library-6"></a>

## <a name="enterprise-library-6"></a>Kurumsal kitaplığı 6

Kurumsal kitaplığı 6 (EntLib60) biri olan SQL veritabanı hizmeti, bulut Hizmetleri, güçlü istemciler uygulamanıza yardımcı olan bir .NET sınıf çerçevedir. EntLib60 olabileceğine her alanı için ayrılmış konuları bulmak için bkz: [Kurumsal kitaplığı 6 - Nisan 2013](https://msdn.microsoft.com/library/dn169621%28v=pandp.60%29.aspx).

Geçici hataları işlemek için yeniden deneme mantığı EntLib60 size yardımcı olabilir bir alandır. Daha fazla bilgi için [4 - Perseverance, tüm triumphs gizliliği: Geçici hata işleme uygulama bloğu kullanın](https://msdn.microsoft.com/library/dn440719%28v=pandp.60%29.aspx).

> [!NOTE]
> Genel Merkezi'nden EntLib60 için kaynak kodu kullanılabilir [İndirme Merkezi](https://go.microsoft.com/fwlink/p/?LinkID=290898). Microsoft, daha fazla özellik güncelleştirmeleri veya bakım güncelleştirmeleri için EntLib yapmak için herhangi bir plan sahiptir.

<a id="entlib60-classes-for-transient-errors-and-retry" name="entlib60-classes-for-transient-errors-and-retry"></a>

### <a name="entlib60-classes-for-transient-errors-and-retry"></a>Geçici hataları ve yeniden deneme için EntLib60 sınıfları

Aşağıdaki EntLib60 sınıfları yeniden deneme mantığı için özellikle yararlıdır. Bu sınıf veya ad alanı altında bulunur **Microsoft.Practices.EnterpriseLibrary.TransientFaultHandling**.

Ad alanındaki **Microsoft.Practices.EnterpriseLibrary.TransientFaultHandling**:

- **RetryPolicy** sınıfı
  - **ExecuteAction** yöntemi
- **ExponentialBackoff** sınıfı
- **SqlDatabaseTransientErrorDetectionStrategy** class
- **ReliableSqlConnection** sınıfı
  - **ExecuteCommand** yöntemi

Ad alanındaki **Microsoft.Practices.EnterpriseLibrary.TransientFaultHandling.TestSupport**:

- **AlwaysTransientErrorDetectionStrategy** sınıfı
- **NeverTransientErrorDetectionStrategy** sınıfı

Bazı EntLib60 hakkındaki bilgilere bağlantılar aşağıda verilmiştir:

- Ücretsiz kitap indirme: [Microsoft Enterprise Library, 2 sürümü için Geliştirici Kılavuzu](https://www.microsoft.com/download/details.aspx?id=41145).
- En iyi uygulamalar: [Yeniden deneme genel Kılavuzu](../best-practices-retry-general.md) yeniden deneme mantığı mükemmel ilgili ayrıntılı bir tartışma sahiptir.
- NuGet indirin: [Enterprise Library - geçici hata işleme uygulama bloğu 6.0](https://www.nuget.org/packages/EnterpriseLibrary.TransientFaultHandling/).

<a id="entlib60-the-logging-block" name="entlib60-the-logging-block"></a>

### <a name="entlib60-the-logging-block"></a>EntLib60: Günlük bloğu

- Günlük bloğu için kullanabileceğiniz yüksek oranda esnek ve yapılandırılabilir bir çözümdür:
  - Oluşturma ve çok çeşitli konumlara günlük iletilerini depolayın.
  - Kategorilere ayırın ve iletileri filtreleyin.
  - Hata ayıklama ve izleme yanı sıra denetim için kullanışlı ve genel günlüğü gereksinimleri olan bağlamsal bilgiler toplayın.
- Uygulama kodu hedef günlük depolama türünü ve konumunu bağımsız olarak tutarlı olmasını sağlayın günlük hedef günlük işlevi günlük blok soyutlar.

Daha fazla bilgi için [5 - kadar kolay bir oturum kapatma denk gelen olarak: Günlük uygulama bloğu kullanın](https://msdn.microsoft.com/library/dn440731%28v=pandp.60%29.aspx).

<a id="entlib60-istransient-method-source-code" name="entlib60-istransient-method-source-code"></a>

### <a name="entlib60-istransient-method-source-code"></a>EntLib60 IsTransient yöntemi kaynak kodu

Öğesinden sonraki **SqlDatabaseTransientErrorDetectionStrategy** sınıfı, C# kaynak kodu **IsTransient** yöntemi. Kaynak kodu, hangi hataların geçici ve yeniden deneyin, Nisan 2013 itibarıyla, bu durum dikkate açıklar.

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

- Diğer ortak SQL veritabanı bağlantı sorunlarını giderme hakkında daha fazla bilgi için bkz. [Azure SQL veritabanı bağlantı sorunlarını giderme](sql-database-troubleshoot-common-connection-issues.md).
- [SQL veritabanı ve SQL Server için bağlantı kitaplıkları](sql-database-libraries.md)
- [SQL Server connection pooling (ADO.NET)](https://docs.microsoft.com/dotnet/framework/data/adonet/sql-server-connection-pooling)
- [*Yeniden deneme* bir Apache 2.0 lisansı kitaplığı, Python'da yazılmış, yeniden deneme genel amaçlı olan getirin](https://pypi.python.org/pypi/retrying) her şey için yeniden deneme davranışı ekleme görevini kolaylaştıran.

<!-- Link references. -->

[step-4-connect-resiliently-to-sql-with-ado-net-a78n]: https://docs.microsoft.com/sql/connect/ado-net/step-4-connect-resiliently-to-sql-with-ado-net

[step-4-connect-resiliently-to-sql-with-php-p42h]: https://docs.microsoft.com/sql/connect/php/step-4-connect-resiliently-to-sql-with-php
