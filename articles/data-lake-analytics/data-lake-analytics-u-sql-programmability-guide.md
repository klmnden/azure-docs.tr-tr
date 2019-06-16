---
title: Azure Data Lake için U-SQL Programlama Kılavuzu
description: Bulut tabanlı büyük veri platformunun oluşturmanıza olanak sağlayan bir Azure Data Lake Analytics hizmetleri hakkında bilgi edinin.
services: data-lake-analytics
ms.service: data-lake-analytics
author: saveenr
ms.author: saveenr
ms.reviewer: jasonwhowell
ms.assetid: 63be271e-7c44-4d19-9897-c2913ee9599d
ms.topic: conceptual
ms.date: 06/30/2017
ms.openlocfilehash: d1b230b40d1f880787334ebfd39e704e3a650baa
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60811607"
---
# <a name="u-sql-programmability-guide"></a>U-SQL Programlama Kılavuzu

U-SQL, büyük veri türü için iş yüklerinin tasarlanmış bir sorgu dilidir. U-SQL benzersiz özelliklerden biri, genişletilebilirlik ve C# tarafından sağlanan programlama SQL benzeri tanımlayıcı dil birleşimidir. Bu kılavuzda, genişletilebilirlik ve C# kullanarak etkin U-SQL dili ile programlama, biz yoğunlaşır.

## <a name="requirements"></a>Gereksinimler

İndirme ve yükleme [Visual Studio için Azure Data Lake Araçları](https://www.microsoft.com/download/details.aspx?id=49504).

## <a name="get-started-with-u-sql"></a>U-SQL ile çalışmaya başlama  

Aşağıdaki U-SQL betiği konum:

```
@a  = 
  SELECT * FROM 
    (VALUES
       ("Contoso",   1500.0, "2017-03-39"),
       ("Woodgrove", 2700.0, "2017-04-10")
    ) AS D( customer, amount, date );

@results =
  SELECT
    customer,
    amount,
    date
  FROM @a;    
```

Bu betik iki satır kümeleri tanımlar: `@a` ve `@results`. Satır kümesi `@results` öğesinden tanımlanan `@a`.

## <a name="c-types-and-expressions-in-u-sql-script"></a>C# türleri ve U-SQL betiği ifadeleri

Bir C# ifadesi gibi U-SQL mantıksal işlemleri ile birlikte bir U-SQL ifadesi; `AND`, `OR`, ve `NOT`. U-SQL deyimleri, SELECT, EXTRACT, kullanılabilir, sahip ve GRUPLANDIRMA ölçütü BİLDİRİN. Örneğin, aşağıdaki betik, bir DateTime değeri olarak bir dizeyi ayrıştırır.

```
@results =
  SELECT
    customer,
    amount,
    DateTime.Parse(date) AS date
  FROM @a;    
```

Aşağıdaki kod parçacığı DECLARE deyimi içinde DateTime değeri olarak bir dizeyi ayrıştırır.

```
DECLARE @d = DateTime.Parse("2016/01/01");
```

### <a name="use-c-expressions-for-data-type-conversions"></a>Veri türü dönüştürmeleri için C# ifadeleri kullanma

Aşağıdaki örnek, C# ifadeleri ile bir datetime veri dönüştürme nasıl yapabileceğiniz gösterir. Buradaki senaryoda, standart tarih/saat gece yarısını 00:00:00 zaman gösterimiyle dize datetime veri dönüştürülür.

```
DECLARE @dt = "2016-07-06 10:23:15";

@rs1 =
  SELECT 
    Convert.ToDateTime(Convert.ToDateTime(@dt).ToString("yyyy-MM-dd")) AS dt,
    dt AS olddt
  FROM @rs0;

OUTPUT @rs1 
  TO @output_file 
  USING Outputters.Text();
```

### <a name="use-c-expressions-for-todays-date"></a>C# ifadelerini bugünün tarihini kullanın

Bugünün tarihini çekmek için aşağıdaki C# ifade kullanabilirsiniz: `DateTime.Now.ToString("M/d/yyyy")`

Bu ifade bir betikte kullanmaya ilişkin bir örnek aşağıda verilmiştir:

```
@rs1 =
  SELECT
    MAX(guid) AS start_id,
    MIN(dt) AS start_time,
    MIN(Convert.ToDateTime(Convert.ToDateTime(dt<@default_dt?@default_dt:dt).ToString("yyyy-MM-dd"))) AS start_zero_time,
    MIN(USQL_Programmability.CustomFunctions.GetFiscalPeriod(dt)) AS start_fiscalperiod,
    DateTime.Now.ToString("M/d/yyyy") AS Nowdate,
    user,
    des
  FROM @rs0
  GROUP BY user, des;
```
## <a name="using-net-assemblies"></a>.NET derlemeleri

U-SQL'nin genişletilebilirlik modeli yoğun .NET derlemeleri özel kod ekleme olanağı kullanır. 

### <a name="register-a-net-assembly"></a>Bir .NET bütünleştirilmiş kodu Kaydet

Kullanım `CREATE ASSEMBLY` deyimi bir .NET derlemesi bir U-SQL veritabanına yerleştirmek için. Kullanarak U-SQL betikleri bu derlemeleri daha sonra kullanabileceğiniz `REFERENCE ASSEMBLY` deyimi. 

Aşağıdaki kod, bir derlemeyi kaydettirmek gösterilmektedir:

```
CREATE ASSEMBLY MyDB.[MyAssembly]
   FROM "/myassembly.dll";
```

Aşağıdaki kod, bir derleme başvurusu gösterilmektedir:

```
REFERENCE ASSEMBLY MyDB.[MyAssembly];
```

Başvurun [derleme kayıt yönergeleri](https://blogs.msdn.microsoft.com/azuredatalake/2016/08/26/how-to-register-u-sql-assemblies-in-your-u-sql-catalog/) , bu konuda daha ayrıntılı ele alınmaktadır.


### <a name="use-assembly-versioning"></a>Derleme sürümlendirme kullanın
Şu anda, U-SQL .NET Framework 4.5 sürümünü kullanır. Bu nedenle, kendi derlemeleri çalışma zamanının bu sürümü ile uyumlu olduğundan emin olun.

U-SQL kodun bir 64-bit (x 64) biçiminde daha önce belirtildiği gibi. Bu nedenle x64 üzerinde çalıştırmak için kodunuzun derlendiğinden emin olun. Aksi takdirde, daha önce gösterilen yanlış biçim hatası alırsınız.

Her derleme DLL karşıya ve farklı bir çalışma zamanı, yerel bir derleme veya bir yapılandırma dosyası gibi bir kaynak dosyası, en fazla 400 MB olabilir. Kaynak dağıtma yoluyla veya başvuru derlemeleri ve bunların ek dosyaları aracılığıyla dağıtılan kaynaklar toplam boyutu 3 GB aşamaz.

Son olarak, her bir U-SQL veritabanı herhangi bir derleme bir sürümü yalnızca içerebileceğini unutmayın. Örneğin, sürüm 7 hem NewtonSoft Json.NET kitaplığın sürüm 8 ihtiyacınız varsa, bunları iki farklı veritabanlarında kaydetmeniz gerekir. Ayrıca, her komut dosyası yalnızca DLL verilen bir derlemeye bir sürümüne başvuruda bulunabilir. Bu bakımdan, U-SQL C# derleme yönetimi ve sürüm semantiği izler.

## <a name="use-user-defined-functions-udf"></a>Kullanıcı tanımlı işlevler kullanma: UDF
U-SQL kullanıcı tanımlı işlevleri veya UDF, parametreleri kabul eder (örneğin, karmaşık bir hesaplama) bir eylem gerçekleştirmek ve bir değer olarak, eylem sonucu döndürür yordamlarını programlama. UDF dönüş değeri yalnızca tek bir skaler olabilir. U-SQL UDF diğer C# skaler bir işlev gibi temel U-SQL betiğindeki çağrılabilir.

U-SQL kullanıcı tanımlı işlevler olarak başlatmak öneririz **genel** ve **statik**.

```
public static string MyFunction(string param1)
{
    return "my result";
}
```

İlk bir UDF oluşturmanın basit örneğe göz atalım.

Mali Çeyrek yıl ve mali ayı ilk oturum açmak için kullanıcıyı dahil olmak üzere dönemin mali belirlemek için bu kullanım örneği senaryosu oluşturmamız gerekir. Bizim senaryomuz yılın mali ayı ilk Haziran ' dir.

Mali Döneme hesaplamak için biz aşağıdaki C# işlevi sunar:

```
public static string GetFiscalPeriod(DateTime dt)
{
    int FiscalMonth=0;
    if (dt.Month < 7)
    {
    FiscalMonth = dt.Month + 6;
    }
    else
    {
    FiscalMonth = dt.Month - 6;
    }

    int FiscalQuarter=0;
    if (FiscalMonth >=1 && FiscalMonth<=3)
    {
    FiscalQuarter = 1;
    }
    if (FiscalMonth >= 4 && FiscalMonth <= 6)
    {
    FiscalQuarter = 2;
    }
    if (FiscalMonth >= 7 && FiscalMonth <= 9)
    {
    FiscalQuarter = 3;
    }
    if (FiscalMonth >= 10 && FiscalMonth <= 12)
    {
    FiscalQuarter = 4;
    }

    return "Q" + FiscalQuarter.ToString() + ":P" + FiscalMonth.ToString();
}
```

Çeyreğin mali ayı hesaplar ve bir string değeri döndürür. Haziran için ilk mali üç aylık dönemin ilk ay "Q1:P1" kullanırız. Temmuz, biz "Q1:P2" kullanın ve benzeri.

U-SQL Projemizin kullanacağız bir normal C# işlev budur.

Arka plan kod bölümünde bu senaryoda nasıl göründüğünü aşağıda verilmiştir:

```
using Microsoft.Analytics.Interfaces;
using Microsoft.Analytics.Types.Sql;
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;

namespace USQL_Programmability
{
    public class CustomFunctions
    {
        public static string GetFiscalPeriod(DateTime dt)
        {
            int FiscalMonth=0;
            if (dt.Month < 7)
            {
                FiscalMonth = dt.Month + 6;
            }
            else
            {
                FiscalMonth = dt.Month - 6;
            }

            int FiscalQuarter=0;
            if (FiscalMonth >=1 && FiscalMonth<=3)
            {
                FiscalQuarter = 1;
            }
            if (FiscalMonth >= 4 && FiscalMonth <= 6)
            {
                FiscalQuarter = 2;
            }
            if (FiscalMonth >= 7 && FiscalMonth <= 9)
            {
                FiscalQuarter = 3;
            }
            if (FiscalMonth >= 10 && FiscalMonth <= 12)
            {
                FiscalQuarter = 4;
            }

            return "Q" + FiscalQuarter.ToString() + ":" + FiscalMonth.ToString();
        }
    }
}
```

Temel bir U-SQL komut dosyasından bu işlevi çağırmak için artık kullanacağız. Bunu yapmak için şu ad alanı, bu durumda NameSpace.Class.Function(parameter) olduğu dahil olmak üzere işlev için tam bir ad sağlamanız gerekir.

```
USQL_Programmability.CustomFunctions.GetFiscalPeriod(dt)
```

Asıl U-SQL temel betiği aşağıda verilmiştir:

```
DECLARE @input_file string = @"\usql-programmability\input_file.tsv";
DECLARE @output_file string = @"\usql-programmability\output_file.tsv";

@rs0 =
    EXTRACT
            guid Guid,
        dt DateTime,
            user String,
            des String
    FROM @input_file USING Extractors.Tsv();

DECLARE @default_dt DateTime = Convert.ToDateTime("06/01/2016");

@rs1 =
    SELECT
        MAX(guid) AS start_id,
    MIN(dt) AS start_time,
        MIN(Convert.ToDateTime(Convert.ToDateTime(dt<@default_dt?@default_dt:dt).ToString("yyyy-MM-dd"))) AS start_zero_time,
        MIN(USQL_Programmability.CustomFunctions.GetFiscalPeriod(dt)) AS start_fiscalperiod,
        user,
        des
    FROM @rs0
    GROUP BY user, des;

OUTPUT @rs1 
    TO @output_file 
    USING Outputters.Text();
```

Betik yürütme çıktı dosyası aşağıda verilmiştir:

```
0d8b9630-d5ca-11e5-8329-251efa3a2941,2016-02-11T07:04:17.2630000-08:00,2016-06-01T00:00:00.0000000,"Q3:8","User1",""

20843640-d771-11e5-b87b-8b7265c75a44,2016-02-11T07:04:17.2630000-08:00,2016-06-01T00:00:00.0000000,"Q3:8","User2",""

301f23d2-d690-11e5-9a98-4b4f60a1836f,2016-02-11T09:01:33.9720000-08:00,2016-06-01T00:00:00.0000000,"Q3:8","User3",""
```

Bu örnek, basit bir satır içi U-SQL'de UDF kullanımını gösterir.

### <a name="keep-state-between-udf-invocations"></a>UDF çağrıları arasında durumu korumak
U-SQL C# programlama nesneleri daha, arka plan kod genel değişkenler zahmetle kullanan karmaşık. Aşağıdaki iş kullanım örneği senaryosunu göz atalım.

Büyük kuruluşlarda, kullanıcıların iç uygulamaları çeşitleri arasında geçiş yapabilirsiniz. Bunlar, Microsoft Dynamics CRM, Power BI vb. içerebilir. Müşteriler, kullanıcıların kullanım eğilimlerini nedir, farklı uygulamalar arasında nasıl geçiş, bir telemetri analizi uygulamak isteyebilirsiniz ve benzeri. İş için uygulama kullanımını en iyi duruma olmaktır. Farklı uygulamalarda ya da belirli oturum açma rutinleri birleştirmek de isteyebilirsiniz.

Bu hedefe ulaşmak için oturum kimliklerini belirlemek ve öteleme arasında gerçekleşen son oturumu süresi sahibiz.

Bir önceki oturum açma bulun ve ardından bu oturum açma aynı uygulama için oluşturulan tüm oturumlara atamak ihtiyacımız var. LAG işlevi ile önceden hesaplanmış sütun üzerinde hesaplamalar uygulamak bize temel U-SQL betiği izin vermez ilk zorluktur. Belirli bir oturum için aynı süre içinde tüm oturumları tutmak sahibiz ikinci zorluktur.

Arka plan kod bölümünün içinde genel değişken kullanıyoruz bu sorunu çözmek için: `static public string globalSession;`.

Bu genel değişken tüm satır kümesi için sunduğumuz betik yürütme sırasında uygulanır.

U-SQL programımız arka plan kod bölümünü şu şekildedir:

```
using Microsoft.Analytics.Interfaces;
using Microsoft.Analytics.Types.Sql;
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;

namespace USQLApplication21
{
    public class UserSession
    {
        static public string globalSession;
        static public string StampUserSession(string eventTime, string PreviousRow, string Session)
        {

            if (!string.IsNullOrEmpty(PreviousRow))
            {
                double timeGap = Convert.ToDateTime(eventTime).Subtract(Convert.ToDateTime(PreviousRow)).TotalMinutes;
                if (timeGap <= 60) {return Session;}
                else {return Guid.NewGuid().ToString();}
            }
            else {return Guid.NewGuid().ToString();}

        }

        static public string getStampUserSession(string Session)
        {
            if (Session != globalSession && !string.IsNullOrEmpty(Session)) { globalSession = Session; }
            return globalSession;
        }

    }
}
```

Bu örnek, genel değişken gösterir `static public string globalSession;` içinde kullanılan `getStampUserSession` işlevi ve oturumu parametresini değiştirilir her zaman yeniden.

U-SQL temel betiği aşağıdaki gibidir:

```
DECLARE @in string = @"\UserSession\test1.tsv";
DECLARE @out1 string = @"\UserSession\Out1.csv";
DECLARE @out2 string = @"\UserSession\Out2.csv";
DECLARE @out3 string = @"\UserSession\Out3.csv";

@records =
    EXTRACT DataId string,
            EventDateTime string,           
            UserName string,
            UserSessionTimestamp string

    FROM @in
    USING Extractors.Tsv();

@rs1 =
    SELECT 
        EventDateTime,
        UserName,
    LAG(EventDateTime, 1) 
        OVER(PARTITION BY UserName ORDER BY EventDateTime ASC) AS prevDateTime,          
        string.IsNullOrEmpty(LAG(EventDateTime, 1) 
        OVER(PARTITION BY UserName ORDER BY EventDateTime ASC)) AS Flag,           
        USQLApplication21.UserSession.StampUserSession
           (
            EventDateTime,
            LAG(EventDateTime, 1) OVER(PARTITION BY UserName ORDER BY EventDateTime ASC),
            LAG(UserSessionTimestamp, 1) OVER(PARTITION BY UserName ORDER BY EventDateTime ASC)
           ) AS UserSessionTimestamp
    FROM @records;

@rs2 =
    SELECT 
        EventDateTime,
        UserName,
        LAG(EventDateTime, 1) 
        OVER(PARTITION BY UserName ORDER BY EventDateTime ASC) AS prevDateTime,
        string.IsNullOrEmpty( LAG(EventDateTime, 1) OVER(PARTITION BY UserName ORDER BY EventDateTime ASC)) AS Flag,
        USQLApplication21.UserSession.getStampUserSession(UserSessionTimestamp) AS UserSessionTimestamp
    FROM @rs1
    WHERE UserName != "UserName";

OUTPUT @rs2
    TO @out2
    ORDER BY UserName, EventDateTime ASC
    USING Outputters.Csv();
```

İşlev `USQLApplication21.UserSession.getStampUserSession(UserSessionTimestamp)` burada ikinci bellek satır kümesi hesaplama sırasında çağrılır. Arabimini `UserSessionTimestamp` sütun ve değerine kadar `UserSessionTimestamp` değişti.

Çıkış dosyası aşağıdaki gibidir:

```
"2016-02-19T07:32:36.8420000-08:00","User1",,True,"72a0660e-22df-428e-b672-e0977007177f"
"2016-02-17T11:52:43.6350000-08:00","User2",,True,"4a0cd19a-6e67-4d95-a119-4eda590226ba"
"2016-02-17T11:59:08.8320000-08:00","User2","2016-02-17T11:52:43.6350000-08:00",False,"4a0cd19a-6e67-4d95-a119-4eda590226ba"
"2016-02-11T07:04:17.2630000-08:00","User3",,True,"51860a7a-1610-4f74-a9ea-69d5eef7cd9c"
"2016-02-11T07:10:33.9720000-08:00","User3","2016-02-11T07:04:17.2630000-08:00",False,"51860a7a-1610-4f74-a9ea-69d5eef7cd9c"
"2016-02-15T21:27:41.8210000-08:00","User3","2016-02-11T07:10:33.9720000-08:00",False,"4d2bc48d-bdf3-4591-a9c1-7b15ceb8e074"
"2016-02-16T05:48:49.6360000-08:00","User3","2016-02-15T21:27:41.8210000-08:00",False,"dd3006d0-2dcd-42d0-b3a2-bc03dd77c8b9"
"2016-02-16T06:22:43.6390000-08:00","User3","2016-02-16T05:48:49.6360000-08:00",False,"dd3006d0-2dcd-42d0-b3a2-bc03dd77c8b9"
"2016-02-17T16:29:53.2280000-08:00","User3","2016-02-16T06:22:43.6390000-08:00",False,"2fa899c7-eecf-4b1b-a8cd-30c5357b4f3a"
"2016-02-17T16:39:07.2430000-08:00","User3","2016-02-17T16:29:53.2280000-08:00",False,"2fa899c7-eecf-4b1b-a8cd-30c5357b4f3a"
"2016-02-17T17:20:39.3220000-08:00","User3","2016-02-17T16:39:07.2430000-08:00",False,"2fa899c7-eecf-4b1b-a8cd-30c5357b4f3a"
"2016-02-19T05:23:54.5710000-08:00","User3","2016-02-17T17:20:39.3220000-08:00",False,"6ca7ed80-c149-4c22-b24b-94ff5b0d824d"
"2016-02-19T05:48:37.7510000-08:00","User3","2016-02-19T05:23:54.5710000-08:00",False,"6ca7ed80-c149-4c22-b24b-94ff5b0d824d"
"2016-02-19T06:40:27.4830000-08:00","User3","2016-02-19T05:48:37.7510000-08:00",False,"6ca7ed80-c149-4c22-b24b-94ff5b0d824d"
"2016-02-19T07:27:37.7550000-08:00","User3","2016-02-19T06:40:27.4830000-08:00",False,"6ca7ed80-c149-4c22-b24b-94ff5b0d824d"
"2016-02-19T19:35:40.9450000-08:00","User3","2016-02-19T07:27:37.7550000-08:00",False,"3f385f0b-3e68-4456-ac74-ff6cef093674"
"2016-02-20T00:07:37.8250000-08:00","User3","2016-02-19T19:35:40.9450000-08:00",False,"685f76d5-ca48-4c58-b77d-bd3a9ddb33da"
"2016-02-11T09:01:33.9720000-08:00","User4",,True,"9f0cf696-c8ba-449a-8d5f-1ca6ed8f2ee8"
"2016-02-17T06:30:38.6210000-08:00","User4","2016-02-11T09:01:33.9720000-08:00",False,"8b11fd2a-01bf-4a5e-a9af-3c92c4e4382a"
"2016-02-17T22:15:26.4020000-08:00","User4","2016-02-17T06:30:38.6210000-08:00",False,"4e1cb707-3b5f-49c1-90c7-9b33b86ca1f4"
"2016-02-18T14:37:27.6560000-08:00","User4","2016-02-17T22:15:26.4020000-08:00",False,"f4e44400-e837-40ed-8dfd-2ea264d4e338"
"2016-02-19T01:20:31.4800000-08:00","User4","2016-02-18T14:37:27.6560000-08:00",False,"2136f4cf-7c7d-43c1-8ae2-08f4ad6a6e08"
```

Bu örnekte, genel değişken tüm bellek satır kümesi için uygulanan bir arka plan kod bölümü içinde kullandığımız daha karmaşık bir kullanım örneği senaryosu gösterilmiştir.

## <a name="use-user-defined-types-udt"></a>Kullanıcı tanımlı türler kullanın: UDT
Kullanıcı tanımlı türler veya UDT, başka bir programlama, U-SQL özelliğidir. U-SQL UDT bir normal C# kullanıcı tanımlı tür gibi davranır. C# yerleşik ve özel kullanıcı tanımlı türler kullanımına izin veren bir türü kesin belirlenmiş dildir.

U-SQL örtük olarak seri hale getirmek veya UDT satır kümeleri köşeler arasında iletildiğinde rastgele Udt'ler anlayabileceği değiştirilemez. Bu, kullanıcı açık bir biçimlendirici Iformatter arabirimi kullanarak sağlamak sahip olduğu anlamına gelir. Bu, U-SQL ile serileştirme sağlar ve yöntemleri için UDT seri durumdan çıkarılamıyor.

> [!NOTE]
> U-SQL'nin yerleşik ayıklayıcılar ve çıktı şu anda yapamazsınız serileştirmek veya seri durumdan çıkarılamıyor Iformatter kümesi dosyalarıyla bile UDT veri. Bu nedenle UDT veri çıkış bildirimi bir dosyaya yazma veya bir ayıklayıcısı ile okuma, bir dize veya bayt dizisi olarak geçirmek zorunda. Daha sonra seri hale getirme çağırın ve kod (diğer bir deyişle, UDT'ın ToString() yöntemini) açıkça seri durumdan çıkarma. Kullanıcı tanımlı ayıklayıcılar ve çıktı, diğer taraftan, okuyup Udt'ler yazabilir.

Biz UDT AYIKLAYICISI veya OUTPUTTER (dışında önceki seçin), burada gösterildiği gibi kullanırsanız:

```
@rs1 =
    SELECT 
        MyNameSpace.Myfunction_Returning_UDT(filed1) AS myfield
    FROM @rs0;

OUTPUT @rs1 
    TO @output_file 
    USING Outputters.Text();
```

Biz aşağıdaki hatayı alırsınız:

```
Error   1   E_CSC_USER_INVALIDTYPEINOUTPUTTER: Outputters.Text was used to output column myfield of type
MyNameSpace.Myfunction_Returning_UDT.

Description:

Outputters.Text only supports built-in types.

Resolution:

Implement a custom outputter that knows how to serialize this type, or call a serialization method on the type in
the preceding SELECT.   C:\Users\sergeypu\Documents\Visual Studio 2013\Projects\USQL-Programmability\
USQL-Programmability\Types.usql 52  1   USQL-Programmability
```

İçinde outputter UDT ile çalışmak için ya da dize ile ToString() yöntemini veya özel outputter oluşturmak için seri hale getirmek sahibiz.

Udt'ler, GROUP BY içinde şu anda kullanılamaz. GROUP BY içinde UDT kullandıysanız, aşağıdaki hata oluşturulur:

```
Error   1   E_CSC_USER_INVALIDTYPEINCLAUSE: GROUP BY doesn't support type MyNameSpace.Myfunction_Returning_UDT
for column myfield

Description:

GROUP BY doesn't support UDT or Complex types.

Resolution:

Add a SELECT statement where you can project a scalar column that you want to use with GROUP BY.
C:\Users\sergeypu\Documents\Visual Studio 2013\Projects\USQL-Programmability\USQL-Programmability\Types.usql
62  5   USQL-Programmability
```

Bir UDT tanımlamak için için sunuyoruz:

* Aşağıdaki ad alanlarını ekleyin:

```
using Microsoft.Analytics.Interfaces
using System.IO;
```

* Ekleme `Microsoft.Analytics.Interfaces`, UDT arabirimler için gerekli olan. Ayrıca, `System.IO` Iformatter arabirimi tanımlamak için gerekli.

* Türüyle kullanılan tanımlı SqlUserDefinedType özniteliği tanımlayın.

**SqlUserDefinedType** bir tür tanımı bir derlemede U-SQL kullanıcı tanımlı tür (UDT) olarak işaretlemek için kullanılır. Öznitelikte özellik UDT fiziksel özelliklerini yansıtır. Bu sınıf devralınamaz.

SqlUserDefinedType, UDT tanımından için gerekli bir özniteliktir.

Sınıfının Oluşturucusu:  

* SqlUserDefinedTypeAttribute (türü biçimlendiricisini)

* Biçimlendirici yazın: Gerekli bir UDT biçimlendirici--özellikle tanımlamak için parametre türü `IFormatter` arabirimi burada geçirilmelidir.

```
[SqlUserDefinedType(typeof(MyTypeFormatter))]
public class MyType
{ … }
```

* Tipik bir UDT Iformatter arabirimi tanımını aşağıdaki örnekte gösterildiği gibi ayrıca gerektirir:

```
public class MyTypeFormatter : IFormatter<MyType>
{
    public void Serialize(MyType instance, IColumnWriter writer, ISerializationContext context)
    { … }

    public MyType Deserialize(IColumnReader reader, ISerializationContext context)
    { … }
}
```

`IFormatter` Arabirimi serileştirir ve bir nesne grafiğinin kök türü ile serileştirir \<typeparamref name = "T" >.

\<typeparam name = "T" > Nesne grafiğinin seri hale getirmek ve seri durumdan çıkarılamıyor kök türü.

* **Seri durumdan**: Sağlanan akış verileri serileştirir ve grafik nesnelerinin reconstitutes.

* **Seri hale getirme**: Bir nesne veya graf ile sağlanan akışına verilen kök nesnelerin serileştirir.

`MyType` Örnek: Tür örneği.  
`IColumnWriter` yazıcı / `IColumnReader` okuyucu: Temel alınan sütun akış.  
`ISerializationContext` İçerik: Kaynak veya hedef içerik akışı için serileştirme sırasında belirten bayrakları kümesini tanımlayan sabit listesi.

* **Ara**: Kaynak veya hedef bağlam kalıcı bir depoya olmadığını belirtir.

* **Kalıcılık**: Kaynak veya hedef bağlam kalıcı bir depoya olduğunu belirtir.

Bir normal C# türü olarak bir U-SQL UDT tanımından geçersiz kılmaları işleçleri içerebilir +/ == /! =. Ayrıca, statik yöntemler de içerebilir. U-SQL en az bir toplama işlevi için parametre olarak bu UDT kullanılacak kullanacağız, örneğin, biz tanımlamak zorunda < işleci geçersiz kılma.

Örneği belirli bir tarihten biçimde mali dönem tanımlaması için Biz bu kılavuzda daha önce gösterilen `Qn:Pn (Q1:P10)`. Aşağıdaki örnek, mali nokta değerleri için özel bir tür tanımlamak gösterilmektedir.

Özel UDT ve Iformatter arabirimi ile arka plan kod bölümünün bir örneği verilmiştir:

```
[SqlUserDefinedType(typeof(FiscalPeriodFormatter))]
public struct FiscalPeriod
{
    public int Quarter { get; private set; }

    public int Month { get; private set; }

    public FiscalPeriod(int quarter, int month):this()
    {
    this.Quarter = quarter;
    this.Month = month;
    }

    public override bool Equals(object obj)
    {
    if (ReferenceEquals(null, obj))
    {
        return false;
    }

    return obj is FiscalPeriod && Equals((FiscalPeriod)obj);
    }

    public bool Equals(FiscalPeriod other)
    {
return this.Quarter.Equals(other.Quarter) && this.Month.Equals(other.Month);
    }

    public bool GreaterThan(FiscalPeriod other)
    {
return this.Quarter.CompareTo(other.Quarter) > 0 || this.Month.CompareTo(other.Month) > 0;
    }

    public bool LessThan(FiscalPeriod other)
    {
return this.Quarter.CompareTo(other.Quarter) < 0 || this.Month.CompareTo(other.Month) < 0;
    }

    public override int GetHashCode()
    {
    unchecked
    {
        return (this.Quarter.GetHashCode() * 397) ^ this.Month.GetHashCode();
    }
    }

    public static FiscalPeriod operator +(FiscalPeriod c1, FiscalPeriod c2)
    {
return new FiscalPeriod((c1.Quarter + c2.Quarter) > 4 ? (c1.Quarter + c2.Quarter)-4 : (c1.Quarter + c2.Quarter), (c1.Month + c2.Month) > 12 ? (c1.Month + c2.Month) - 12 : (c1.Month + c2.Month));
    }

    public static bool operator ==(FiscalPeriod c1, FiscalPeriod c2)
    {
    return c1.Equals(c2);
    }

    public static bool operator !=(FiscalPeriod c1, FiscalPeriod c2)
    {
    return !c1.Equals(c2);
    }
    public static bool operator >(FiscalPeriod c1, FiscalPeriod c2)
    {
    return c1.GreaterThan(c2);
    }
    public static bool operator <(FiscalPeriod c1, FiscalPeriod c2)
    {
    return c1.LessThan(c2);
    }
    public override string ToString()
    {
    return (String.Format("Q{0}:P{1}", this.Quarter, this.Month));
    }

}

public class FiscalPeriodFormatter : IFormatter<FiscalPeriod>
{
    public void Serialize(FiscalPeriod instance, IColumnWriter writer, ISerializationContext context)
    {
    using (var binaryWriter = new BinaryWriter(writer.BaseStream))
    {
        binaryWriter.Write(instance.Quarter);
        binaryWriter.Write(instance.Month);
        binaryWriter.Flush();
    }
    }

    public FiscalPeriod Deserialize(IColumnReader reader, ISerializationContext context)
    {
    using (var binaryReader = new BinaryReader(reader.BaseStream))
    {
var result = new FiscalPeriod(binaryReader.ReadInt16(), binaryReader.ReadInt16());
        return result;
    }
    }
}
```

İki sayıyı tanımlanan bir tür içerir: Çeyrek yıl ve ay. İşleçler `==/!=/>/<` ve statik yöntem `ToString()` burada tanımlanır.

Daha önce bahsedildiği gibi UDT SELECT ifadelerde kullanılabilir, ancak OUTPUTTER/AYIKLAYICISI özel seri hale getirme kullanılamaz. Ya da bir dize olarak serileştirilmesi sahip `ToString()` veya özel bir OUTPUTTER/AYIKLAYICISI kullanılır.

Şimdi github'dan UDT kullanımını tartışın. Arka plan kod bölümünde, bizim GetFiscalPeriod işlevi aşağıdaki değiştirdik:

```
public static FiscalPeriod GetFiscalPeriodWithCustomType(DateTime dt)
{
    int FiscalMonth = 0;
    if (dt.Month < 7)
    {
    FiscalMonth = dt.Month + 6;
    }
    else
    {
    FiscalMonth = dt.Month - 6;
    }

    int FiscalQuarter = 0;
    if (FiscalMonth >= 1 && FiscalMonth <= 3)
    {
    FiscalQuarter = 1;
    }
    if (FiscalMonth >= 4 && FiscalMonth <= 6)
    {
    FiscalQuarter = 2;
    }
    if (FiscalMonth >= 7 && FiscalMonth <= 9)
    {
    FiscalQuarter = 3;
    }
    if (FiscalMonth >= 10 && FiscalMonth <= 12)
    {
    FiscalQuarter = 4;
    }

    return new FiscalPeriod(FiscalQuarter, FiscalMonth);
}
```

Gördüğünüz gibi bizim FiscalPeriod türünün değerini döndürür.

Buradan daha fazla temel U-SQL betiği içinde kullanmaya ilişkin bir örnek sağlar. Bu örnekte, farklı UDT çağırma formlardan U-SQL betiği gösterilmiştir.

```
DECLARE @input_file string = @"c:\work\cosmos\usql-programmability\input_file.tsv";
DECLARE @output_file string = @"c:\work\cosmos\usql-programmability\output_file.tsv";

@rs0 =
    EXTRACT
        guid string,
        dt DateTime,
        user String,
        des String
    FROM @input_file USING Extractors.Tsv();

@rs1 =
    SELECT 
        guid AS start_id,
        dt,
        DateTime.Now.ToString("M/d/yyyy") AS Nowdate,
        USQL_Programmability.CustomFunctions.GetFiscalPeriodWithCustomType(dt).Quarter AS fiscalquarter,
        USQL_Programmability.CustomFunctions.GetFiscalPeriodWithCustomType(dt).Month AS fiscalmonth,
        USQL_Programmability.CustomFunctions.GetFiscalPeriodWithCustomType(dt) + new USQL_Programmability.CustomFunctions.FiscalPeriod(1,7) AS fiscalperiod_adjusted,
        user,
        des
    FROM @rs0;

@rs2 =
    SELECT 
           start_id,
           dt,
           DateTime.Now.ToString("M/d/yyyy") AS Nowdate,
           fiscalquarter,
           fiscalmonth,
           USQL_Programmability.CustomFunctions.GetFiscalPeriodWithCustomType(dt).ToString() AS fiscalperiod,

       // This user-defined type was created in the prior SELECT.  Passing the UDT to this subsequent SELECT would have failed if the UDT was not annotated with an IFormatter.
           fiscalperiod_adjusted.ToString() AS fiscalperiod_adjusted,
           user,
           des
    FROM @rs1;

OUTPUT @rs2 
    TO @output_file 
    USING Outputters.Text();
```

Tam arka plan kod bölümünün bir örnek aşağıda verilmiştir:

```
using Microsoft.Analytics.Interfaces;
using Microsoft.Analytics.Types.Sql;
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.IO;

namespace USQL_Programmability
{
    public class CustomFunctions
    {
        static public DateTime? ToDateTime(string dt)
        {
            DateTime dtValue;

            if (!DateTime.TryParse(dt, out dtValue))
                return Convert.ToDateTime(dt);
            else
                return null;
        }

        public static FiscalPeriod GetFiscalPeriodWithCustomType(DateTime dt)
        {
            int FiscalMonth = 0;
            if (dt.Month < 7)
            {
                FiscalMonth = dt.Month + 6;
            }
            else
            {
                FiscalMonth = dt.Month - 6;
            }

            int FiscalQuarter = 0;
            if (FiscalMonth >= 1 && FiscalMonth <= 3)
            {
                FiscalQuarter = 1;
            }
            if (FiscalMonth >= 4 && FiscalMonth <= 6)
            {
                FiscalQuarter = 2;
            }
            if (FiscalMonth >= 7 && FiscalMonth <= 9)
            {
                FiscalQuarter = 3;
            }
            if (FiscalMonth >= 10 && FiscalMonth <= 12)
            {
                FiscalQuarter = 4;
            }

            return new FiscalPeriod(FiscalQuarter, FiscalMonth);
        }



        [SqlUserDefinedType(typeof(FiscalPeriodFormatter))]
        public struct FiscalPeriod
        {
            public int Quarter { get; private set; }

            public int Month { get; private set; }

            public FiscalPeriod(int quarter, int month):this()
            {
                this.Quarter = quarter;
                this.Month = month;
            }

            public override bool Equals(object obj)
            {
                if (ReferenceEquals(null, obj))
                {
                    return false;
                }

                return obj is FiscalPeriod && Equals((FiscalPeriod)obj);
            }

            public bool Equals(FiscalPeriod other)
            {
return this.Quarter.Equals(other.Quarter) &&    this.Month.Equals(other.Month);
            }

            public bool GreaterThan(FiscalPeriod other)
            {
return this.Quarter.CompareTo(other.Quarter) > 0 || this.Month.CompareTo(other.Month) > 0;
            }

            public bool LessThan(FiscalPeriod other)
            {
return this.Quarter.CompareTo(other.Quarter) < 0 || this.Month.CompareTo(other.Month) < 0;
            }

            public override int GetHashCode()
            {
                unchecked
                {
                    return (this.Quarter.GetHashCode() * 397) ^ this.Month.GetHashCode();
                }
            }

            public static FiscalPeriod operator +(FiscalPeriod c1, FiscalPeriod c2)
            {
return new FiscalPeriod((c1.Quarter + c2.Quarter) > 4 ? (c1.Quarter + c2.Quarter)-4 : (c1.Quarter + c2.Quarter), (c1.Month + c2.Month) > 12 ? (c1.Month + c2.Month) - 12 : (c1.Month + c2.Month));
            }

            public static bool operator ==(FiscalPeriod c1, FiscalPeriod c2)
            {
                return c1.Equals(c2);
            }

            public static bool operator !=(FiscalPeriod c1, FiscalPeriod c2)
            {
                return !c1.Equals(c2);
            }
            public static bool operator >(FiscalPeriod c1, FiscalPeriod c2)
            {
                return c1.GreaterThan(c2);
            }
            public static bool operator <(FiscalPeriod c1, FiscalPeriod c2)
            {
                return c1.LessThan(c2);
            }
            public override string ToString()
            {
                return (String.Format("Q{0}:P{1}", this.Quarter, this.Month));
            }

        }

        public class FiscalPeriodFormatter : IFormatter<FiscalPeriod>
        {
public void Serialize(FiscalPeriod instance, IColumnWriter writer, ISerializationContext context)
            {
                using (var binaryWriter = new BinaryWriter(writer.BaseStream))
                {
                    binaryWriter.Write(instance.Quarter);
                    binaryWriter.Write(instance.Month);
                    binaryWriter.Flush();
                }
            }

public FiscalPeriod Deserialize(IColumnReader reader, ISerializationContext context)
            {
                using (var binaryReader = new BinaryReader(reader.BaseStream))
                {
var result = new FiscalPeriod(binaryReader.ReadInt16(), binaryReader.ReadInt16());
                    return result;
                }
            }
        }
    }
}
```

## <a name="use-user-defined-aggregates-udagg"></a>Kullanıcı tanımlı toplamlarda kullanın: UDAGG
Kullanıcı tanımlı toplamlarda, olan toplama ile ilgili işlevler,-hazır U-SQL ile birlikte gönderilmeyen ' dir. Örneğin, özel matematik hesaplamaları, dize birleştirmeleri, işlemeleri dizeleri vb. ile gerçekleştirmek için bir toplama olabilir.

Kullanıcı tanımlı toplama temel sınıf tanımına aşağıdaki gibidir:

```csharp
    [SqlUserDefinedAggregate]
    public abstract class IAggregate<T1, T2, TResult> : IAggregate
    {
        protected IAggregate();

        public abstract void Accumulate(T1 t1, T2 t2);
        public abstract void Init();
        public abstract TResult Terminate();
    }
```

**SqlUserDefinedAggregate** türü kullanıcı tanımlı bir toplam kaydedilmesi gerektiğini gösterir. Bu sınıf devralınamaz.

SqlUserDefinedType özniteliği **isteğe bağlı** UDAGG tanımı.


Üç soyut parametreleri geçirmek temel sınıf sağlar: iki giriş parametreleri ve bir sonucu olarak. Veri türlerini değişkendir ve sınıf devralma sırasında tanımlanmalıdır.

```
public class GuidAggregate : IAggregate<string, string, string>
{
    string guid_agg;

    public override void Init()
    { … }

    public override void Accumulate(string guid, string user)
    { … }

    public override string Terminate()
    { … }
}
```

* **Init** her grup için hesaplama sırasında bir kez çalıştırır. Bu, her bir toplama grubu için bir başlatma yordamı sağlar.  
* **Accumulate** her değer için bir defa yürütülür. Toplama algoritması ana işlevsellik sağlar. Toplama değerleri sınıf devralma sırasında tanımlanan çeşitli veri türleri ile kullanılabilir. Değişken veri türünde iki parametre kabul edebilirsiniz.
* **Sonlandırma** her grup için sonuç çıkış işleme sonunda toplama grubu başına bir kez yürütülür.

Doğru giriş ve çıkış veri türlerinin bildirmek için sınıf tanımına aşağıdaki gibi kullanın:

```
public abstract class IAggregate<T1, T2, TResult> : IAggregate
```

* T1: İlk parametresini Topla
* T2: Accumulate ikinci parametre
* TResult: Dönüş türünü Sonlandır

Örneğin:

```
public class GuidAggregate : IAggregate<string, int, int>
```

or

```
public class GuidAggregate : IAggregate<string, string, string>
```

### <a name="use-udagg-in-u-sql"></a>U-SQL UDAGG kullanın
UDAGG kullanmak için öncelikle gerideki kod tanımlayın veya daha önce bahsedildiği gibi mevcut programlama DLL başvurusu.

Ardından aşağıdaki sözdizimini kullanın:

```
AGG<UDAGG_functionname>(param1,param2)
```

UDAGG örneği aşağıda verilmiştir:

```
public class GuidAggregate : IAggregate<string, string, string>
{
    string guid_agg;

    public override void Init()
    {
        guid_agg = "";
    }

    public override void Accumulate(string guid, string user)
    {
        if (user.ToUpper()== "USER1")
        {
        guid_agg += "{" + guid + "}";
        }
    }

    public override string Terminate()
    {
        return guid_agg;
    }

}
```

Ve temel U-SQL betiği:

```
DECLARE @input_file string = @"\usql-programmability\input_file.tsv";
DECLARE @output_file string = @" \usql-programmability\output_file.tsv";

@rs0 =
    EXTRACT
            guid string,
        dt DateTime,
            user String,
            des String
    FROM @input_file 
    USING Extractors.Tsv();

@rs1 =
    SELECT
        user,
        AGG<USQL_Programmability.GuidAggregate>(guid,user) AS guid_list
    FROM @rs0
    GROUP BY user;

OUTPUT @rs1 TO @output_file USING Outputters.Text();
```

Bu kullanım örneği senaryosu, biz belirli kullanıcılar için sınıf GUID'leri art arda ekler.

## <a name="use-user-defined-objects-udo"></a>Kullanıcı tanımlı nesneler kullanın: UDO
U-SQL kullanıcı tanımlı nesneler veya UDO çağrılan özel programlama nesneleri tanımlamanızı sağlar.

U-SQL'de UDO listesi verilmiştir:

* Kullanıcı tanımlı ayıklayıcıları
    * Ayıklama satır satır
    * Yapılandırılmış özel dosyalarından veri ayıklama uygulamak için kullanılan

* Kullanıcı tanımlı çıktı
    * Çıkış satır satır
    * Çıktı özel veri türleri veya özel dosya biçimleri kullanılır.

* Kullanıcı tanımlı işlemcileri
    * Bir satır alın ve bir satır üretir.
    * Sütun sayısını azaltın veya mevcut bir sütun kümesinden türetilen değerlerle yeni sütun oluşturmak için kullanılır

* Kullanıcı tanımlı appliers
    * Bir satır alın ve 0, n sayıda satır üretir
    * Kullanılan ile dış/çapraz UYGULA

* Kullanıcı tanımlı combiners
    * Satır kümeleri--kullanıcı tanımlı birleştirmeler birleştirir

* Kullanıcı tanımlı genişletin
    * N sayıda satır alın ve bir satır üretir.
    * Satır sayısını azaltmak için kullanılan

UDO genellikle açıkça U-SQL komut dosyasında aşağıdaki U-SQL deyimleri bir parçası olarak adlandırılır:

* EXTRACT
* ÇIKIŞ
* SÜREÇ
* BİRLEŞTİRME
* AZALTIN

> [!NOTE]  
> UDO'ın 0,5 Gb bellek tüketmesine sınırlıdır.  Bu bellek sınırlama yerel yürütme için geçerli değildir.

## <a name="use-user-defined-extractors"></a>Kullanıcı tanımlı ayıklayıcıları kullanın
U-SQL, dış veri ayıklama deyimi kullanılarak içeri aktarmanıza olanak sağlar. Bir ayıklama deyimi yerleşik UDO ayıklayıcıları kullanabilirsiniz:  

* *Extractors.Text()* : Sınırlandırılmış metin dosyalarından farklı kodlamaları ayıklanmasıyla sağlar.

* *Extractors.Csv()* : Ayıklama virgülle ayrılmış değer (CSV) dosyaların farklı kodlamaları sağlar.

* *Extractors.Tsv()* : Ayıklama sekmeyle ayrılmış değer (TSV) dosyaların farklı kodlamaları sağlar.

Özel ayıklayıcı geliştirmek yararlı olabilir. Aşağıdaki görevlerden herhangi birini yapmak isterseniz bu verileri içeri aktarma sırasında yararlı olabilir:

* Giriş veri sütunları bölme ve tek tek değerleri değiştirerek değiştirin. İŞLEMCİ işlevselliğini sütunları birleştirmek için iyidir.
* Web sayfaları ve e-postalar gibi yapılandırılmamış verileri veya XML/JSON gibi yarı yapılandırılmamış verileri ayrıştırılamıyor.
* Desteklenmeyen kodlama verileri ayrıştırılamıyor.

Bir kullanıcı tanımlı ayıklayıcısı veya St tanımlamak için oluşturmamız gerekir bir `IExtractor` arabirimi. Tüm giriş parametreleri için sütun/satır sınırlayıcıları gibi ayıklayıcısı ve kodlama, sınıfın oluşturucusunda tanımlanması gerekir. `IExtractor` Arabirimi için bir tanım de içermelidir `IEnumerable<IRow>` gibi geçersiz kıl:

```
[SqlUserDefinedExtractor]
public class SampleExtractor : IExtractor
{
     public SampleExtractor(string row_delimiter, char col_delimiter)
     { … }

     public override IEnumerable<IRow> Extract(IUnstructuredReader input, IUpdatableRow output)
     { … }
}
```

**SqlUserDefinedExtractor** öznitelik türü kullanıcı tanımlı bir ayıklayıcısı kayıtlı olduğunu gösterir. Bu sınıf devralınamaz.

SqlUserDefinedExtractor Opyalanan tanımı için isteğe bağlı bir özniteliktir. St nesnesi için AtomicFileProcessing özelliği tanımlamak için kullanılır.

* bool AtomicFileProcessing   

* **doğru** = ayıklayıcının atomik giriş dosyaları (JSON, XML,...) gerektirdiğini belirtir
* **false** = ayıklayıcının bölünmüş / dağıtılmış dosyalarıyla (CSV, SEQ,...) giderebilirsiniz belirtir

Ana Opyalanan programlama nesneler **giriş** ve **çıkış**. Giriş nesnesi numaralandırma girdi verisi olarak kullanılan `IUnstructuredReader`. Çıkış nesnesi, çıktı verilerini ayıklayıcısı etkinliğin sonucu olarak ayarlamak için kullanılır.

Giriş verilerini üzerinden erişilen `System.IO.Stream` ve `System.IO.StreamReader`.

Giriş sütunları numaralandırma için biz ilk giriş akışında satır sınırlayıcı kullanarak ayırın.

```
foreach (Stream current in input.Split(my_row_delimiter))
{
…
}
```

Ardından, daha fazla giriş satırı sütun bölün.

```
foreach (Stream current in input.Split(my_row_delimiter))
{
…
    string[] parts = line.Split(my_column_delimiter);
    foreach (string part in parts)
    { … }
}
```

Çıkış veri kümesi için kullanırız `output.Set` yöntemi.

Özel ayıklayıcı yalnızca sütun ve tanımlanan değerleri ile çıktı çıktıları anlamak önemlidir. Yöntem çağrısının ayarlayın.

```
output.Set<string>(count, part);
```

Gerçek ayıklayıcısı çıkış çağırarak tetiklenir `yield return output.AsReadOnly();`.

Ayıklayıcısı örneği verilmiştir:

```
[SqlUserDefinedExtractor(AtomicFileProcessing = true)]
public class FullDescriptionExtractor : IExtractor
{
     private Encoding _encoding;
     private byte[] _row_delim;
     private char _col_delim;

    public FullDescriptionExtractor(Encoding encoding, string row_delim = "\r\n", char col_delim = '\t')
    {
         this._encoding = ((encoding == null) ? Encoding.UTF8 : encoding);
         this._row_delim = this._encoding.GetBytes(row_delim);
         this._col_delim = col_delim;

    }

    public override IEnumerable<IRow> Extract(IUnstructuredReader input, IUpdatableRow output)
    {
         string line;
         //Read the input line by line
         foreach (Stream current in input.Split(_encoding.GetBytes("\r\n")))
         {
        using (System.IO.StreamReader streamReader = new StreamReader(current, this._encoding))
         {
             line = streamReader.ReadToEnd().Trim();
             //Split the input by the column delimiter
             string[] parts = line.Split(this._col_delim);
             int count = 0; // start with first column
             foreach (string part in parts)
             {
    if (count == 0)
             {  // for column “guid”, re-generated guid
                 Guid new_guid = Guid.NewGuid();
                 output.Set<Guid>(count, new_guid);
             }
             else if (count == 2)
             {
                 // for column “user”, convert to UPPER case
                 output.Set<string>(count, part.ToUpper());

             }
             else
             {
                 // keep the rest of the columns as-is
                 output.Set<string>(count, part);
             }
             count += 1;
             }

         }
         yield return output.AsReadOnly();
         }
         yield break;
     }
}
```

Bu kullanım örneği senaryosu ayıklayıcısı "GUID" sütunu için bir GUID oluşturur ve "kullanıcı" sütun değerlerini büyük harfe dönüştürür. Özel ayıklayıcı, girdi verilerini ayrıştırma ve düzenleme, daha karmaşık sonuçlara neden olabilir.

Özel ayıklayıcı kullanan temel bir U-SQL komut dosyası aşağıda verilmiştir:

```
DECLARE @input_file string = @"\usql-programmability\input_file.tsv";
DECLARE @output_file string = @"\usql-programmability\output_file.tsv";

@rs0 =
    EXTRACT
            guid Guid,
        dt String,
            user String,
            des String
    FROM @input_file
        USING new USQL_Programmability.FullDescriptionExtractor(Encoding.UTF8);

OUTPUT @rs0 TO @output_file USING Outputters.Text();
```

## <a name="use-user-defined-outputters"></a>Kullanıcı tanımlı çıktı kullanın
Kullanıcı tanımlı outputter yerleşik bir U-SQL işlevlerini genişletmek izin veren başka bir U-SQL UDO ' dir. Benzer şekilde ayıklayıcısı, vardır birkaç yerleşik çıktı.

* *Outputters.Text()* : Verileri farklı kodlamaları sınırlandırılmış metin dosyasına yazar.
* *Outputters.Csv()* : Verileri farklı kodlamaları virgülle ayrılmış değer (CSV) dosyasına yazar.
* *Outputters.Tsv()* : Verileri farklı kodlamaları için sekmesinde ayrılmış değerler (TSV) dosyaları yazar.

Özel outputter özel tanımlanmış bir biçimde veri yazmanıza olanak sağlar. Bu, aşağıdaki görevler için yararlı olabilir:

* Veri, yarı yapılandırılmış veya yapılandırılmamış dosyalara yazma.
* Veri yazma olmayan Kodlamalar desteklenmiyor.
* Çıkış verileri değiştirme veya özel öznitelik ekleme.

Kullanıcı tanımlı outputter tanımlamak için oluşturmamız gerekir `IOutputter` arabirimi.

Temel aşağıdadır `IOutputter` sınıf uygulaması:

```
public abstract class IOutputter : IUserDefinedOperator
{
    protected IOutputter();

    public virtual void Close();
    public abstract void Output(IRow input, IUnstructuredWriter output);
}
```

Tüm giriş parametreleri sütun/satır sınırlayıcıları, kodlama ve benzeri gibi outputter sınıfın oluşturucusunda tanımlanması gerekir. `IOutputter` Arabirimi için bir tanım de içermelidir `void Output` geçersiz kılar. Öznitelik `[SqlUserDefinedOutputter(AtomicFileProcessing = true)` atomik dosya işleme için isteğe bağlı olarak ayarlanabilir. Daha fazla bilgi için aşağıdaki ayrıntılarına bakın.

```
[SqlUserDefinedOutputter(AtomicFileProcessing = true)]
public class MyOutputter : IOutputter
{

    public MyOutputter(myparam1, myparam2)
    {
      …
    }

    public override void Close()
    {
      …
    }

    public override void Output(IRow row, IUnstructuredWriter output)
    {
      …
    }
}
```

* `Output` Her giriş satırı için çağrılır. Döndürür `IUnstructuredWriter output` satır kümesi.
* Oluşturucu sınıf için kullanıcı tanımlı outputter parametreleri geçirmek için kullanılır.
* `Close` İsteğe bağlı olarak, pahalı durumunu serbest bırakmak veya son satırı zaman yazılmıştır belirlemek için geçersiz kılmak için kullanılır.

**SqlUserDefinedOutputter** öznitelik türü kullanıcı tanımlı bir outputter kaydedilmesi gerektiğini gösterir. Bu sınıf devralınamaz.

SqlUserDefinedOutputter, kullanıcı tanımlı outputter tanımı için isteğe bağlı bir özniteliktir. AtomicFileProcessing özelliği tanımlamak için kullanılır.

* bool AtomicFileProcessing   

* **doğru** = bu outputter atomik çıkış dosyaları (JSON, XML,...) gerektirdiğini belirtir
* **false** = bölünmüş / dağıtılmış dosyalarıyla (CSV, SEQ,...) bu outputter giderebilirsiniz belirtir

Ana programlama nesneler **satır** ve **çıkış**. **Satır** nesnesi olarak çıktı verilerini listeleme için kullanılır `IRow` arabirimi. **Çıkış** hedef dosya için çıktı veri kümesi için kullanılır.

Çıktı verilerini üzerinden erişilen `IRow` arabirimi. Çıktı verilerini bir satır aynı anda geçirilir.

Değerlerin IRow arabiriminin alma yöntemini çağırarak numaralandırılır:

```
row.Get<string>("column_name")
```

Tek tek sütun adları çağırarak belirlenebilir `row.Schema`:

```
ISchema schema = row.Schema;
var col = schema[i];
string val = row.Get<string>(col.Name)
```

Bu yaklaşım, herhangi bir meta veri şema için esnek bir outputter oluşturmanıza olanak sağlar.

Çıktı verilerini kullanarak dosyaya yazılır `System.IO.StreamWriter`. Akış parametre kümesine `output.BaseStream` parçası olarak `IUnstructuredWriter output`.

Her satır yinelemeden sonra dosyaya veri arabelleği temizlemek önemli olduğunu unutmayın. Ayrıca, `StreamWriter` nesne ile etkin tek kullanımlık özniteliğin (varsayılan) ile kullanılmalıdır **kullanarak** anahtar sözcüğü:

```
using (StreamWriter streamWriter = new StreamWriter(output.BaseStream, this._encoding))
{
…
}
```

Aksi takdirde, Flush() yöntemi her yinelemeden sonra açıkça çağırın. Bu aşağıdaki örnekte göstereceğiz.

### <a name="set-headers-and-footers-for-user-defined-outputter"></a>Kullanıcı tanımlı outputter için üstbilgiler ve altbilgiler ayarlayın
Bir başlık ayarlamak için tek bir yineleme yürütme akışı kullanın.

```
public override void Output(IRow row, IUnstructuredWriter output)
{
 …
if (isHeaderRow)
{
    …                
}

 …
if (isHeaderRow)
{
    isHeaderRow = false;
}
 …
}
}
```

İlk kod `if (isHeaderRow)` blok yalnızca bir kez yürütülür.

Alt bilgisi için başvuru örneğini kullanın `System.IO.Stream` nesne (`output.BaseStream`). Alt bilgi Close() yöntemi yazma `IOutputter` arabirimi.  (Daha fazla bilgi için aşağıdaki örneğe bakın.)

Kullanıcı tanımlı bir outputter örneği aşağıdadır:

```
[SqlUserDefinedOutputter(AtomicFileProcessing = true)]
public class HTMLOutputter : IOutputter
{
    // Local variables initialization
    private string row_delimiter;
    private char col_delimiter;
    private bool isHeaderRow;
    private Encoding encoding;
    private bool IsTableHeader = true;
    private Stream g_writer;

    // Parameters definition            
    public HTMLOutputter(bool isHeader = false, Encoding encoding = null)
    {
    this.isHeaderRow = isHeader;
    this.encoding = ((encoding == null) ? Encoding.UTF8 : encoding);
    }

    // The Close method is used to write the footer to the file. It's executed only once, after all rows
    public override void Close()
    {
    //Reference to IO.Stream object - g_writer
    StreamWriter streamWriter = new StreamWriter(g_writer, this.encoding);
    streamWriter.Write("</table>");
    streamWriter.Flush();
    streamWriter.Close();
    }

    public override void Output(IRow row, IUnstructuredWriter output)
    {
    System.IO.StreamWriter streamWriter = new StreamWriter(output.BaseStream, this.encoding);

    // Metadata schema initialization to enumerate column names
    ISchema schema = row.Schema;

    // This is a data-independent header--HTML table definition
    if (IsTableHeader)
    {
        streamWriter.Write("<table border=1>");
        IsTableHeader = false;
    }

    // HTML table attributes
    string header_wrapper_on = "<th>";
    string header_wrapper_off = "</th>";
    string data_wrapper_on = "<td>";
    string data_wrapper_off = "</td>";

    // Header row output--runs only once
    if (isHeaderRow)
    {
        streamWriter.Write("<tr>");
        for (int i = 0; i < schema.Count(); i++)
        {
        var col = schema[i];
        streamWriter.Write(header_wrapper_on + col.Name + header_wrapper_off);
        }
        streamWriter.Write("</tr>");
    }

    // Data row output
    streamWriter.Write("<tr>");                
    for (int i = 0; i < schema.Count(); i++)
    {
        var col = schema[i];
        string val = "";
        try
        {
        // Data type enumeration--required to match the distinct list of types from OUTPUT statement
        switch (col.Type.Name.ToString().ToLower())
        {
            case "string": val = row.Get<string>(col.Name).ToString(); break;
            case "guid": val = row.Get<Guid>(col.Name).ToString(); break;
            default: break;
        }
        }
        // Handling NULL values--keeping them empty
        catch (System.NullReferenceException)
        {
        }
        streamWriter.Write(data_wrapper_on + val + data_wrapper_off);
    }
    streamWriter.Write("</tr>");

    if (isHeaderRow)
    {
        isHeaderRow = false;
    }
    // Reference to the instance of the IO.Stream object for footer generation
    g_writer = output.BaseStream;
    streamWriter.Flush();
    }
}

// Define the factory classes
public static class Factory
{
    public static HTMLOutputter HTMLOutputter(bool isHeader = false, Encoding encoding = null)
    {
    return new HTMLOutputter(isHeader, encoding);
    }
}
```

Ve temel U-SQL betiği:

```
DECLARE @input_file string = @"\usql-programmability\input_file.tsv";
DECLARE @output_file string = @"\usql-programmability\output_file.html";

@rs0 =
    EXTRACT
            guid Guid,
        dt String,
            user String,
            des String
         FROM @input_file
         USING new USQL_Programmability.FullDescriptionExtractor(Encoding.UTF8);

OUTPUT @rs0 
    TO @output_file 
    USING new USQL_Programmability.HTMLOutputter(isHeader: true);
```

Tablo verilerini bir HTML dosyası oluşturur bir HTML outputter budur.

### <a name="call-outputter-from-u-sql-base-script"></a>U-SQL temel betikten outputter çağırın
Temel bir U-SQL komut dosyasından özel outputter çağırmak için yeni outputter nesnesi örneği oluşturulması gerekir.

```sql
OUTPUT @rs0 TO @output_file USING new USQL_Programmability.HTMLOutputter(isHeader: true);
```

Temel betikte nesnesinin bir örneğini oluşturmaktan kaçınmak için bir işlev sarmalayıcı bizim önceki örnekte gösterilen şekilde oluşturabiliriz:

```csharp
        // Define the factory classes
        public static class Factory
        {
            public static HTMLOutputter HTMLOutputter(bool isHeader = false, Encoding encoding = null)
            {
                return new HTMLOutputter(isHeader, encoding);
            }
        }
```

Bu durumda, özgün çağrı aşağıdaki gibi görünür:

```
OUTPUT @rs0 
TO @output_file 
USING USQL_Programmability.Factory.HTMLOutputter(isHeader: true);
```

## <a name="use-user-defined-processors"></a>Kullanıcı tanımlı işlemcileri kullanın
Kullanıcı tanımlı işlemci veya UDP, U-SQL programlama özellikleri uygulayarak gelen satır işleyecek şekilde sağlayan UDO türüdür. UDP sütunları birleştirin, değerleri değiştirebilir ve gerekirse, yeni sütun ekleme sağlar. Aslında, gerekli veri öğeleri oluşturmak için bir satır kümesi işlemeye yardımcı olur.

Bir UDP tanımlamak için oluşturmamız gerekir bir `IProcessor` ile arabirim `SqlUserDefinedProcessor` UDP için isteğe bağlı öznitelik.

Bu arabirim tanımı içermelidir `IRow` arabirimi satır kümesi geçersiz kılmak, aşağıdaki örnekte gösterildiği gibi:

```
[SqlUserDefinedProcessor]
public class MyProcessor: IProcessor
{
public override IRow Process(IRow input, IUpdatableRow output)
 {
        …
 }
}
```

**SqlUserDefinedProcessor** türü kullanıcı tanımlı bir işlemci kaydedilmesi gerektiğini gösterir. Bu sınıf devralınamaz.

SqlUserDefinedProcessor öznitelik **isteğe bağlı** UDP tanımı.

Ana programlama nesneler **giriş** ve **çıkış**. Giriş nesnesi sütunları giriş ve çıkış listeleme ve sonucunda işlemci etkinliğinin çıkış veri kümesi için kullanılır.

Giriş sütunları numaralandırma için kullandığımız `input.Get` yöntemi.

```
string column_name = input.Get<string>("column_name");
```

Parametresi için `input.Get` yöntemdir parçası olarak geçirilen bir sütun `PRODUCE` yan tümcesi `PROCESS` temel U-SQL komut dosyası ifadesi. Doğru veri türünü burada kullanmak ihtiyacımız var.

Çıkış için kullan `output.Set` yöntemi.

Bu özel üretici yalnızca çıkarır sütunlar ve değerler ile tanımlanan dikkat edin önemlidir `output.Set` yöntem çağrısı.

```
output.Set<string>("mycolumn", mycolumn);
```

Gerçek işlemci çıktı çağırarak tetiklenir `return output.AsReadOnly();`.

Bir işlemci örneği verilmiştir:

```
[SqlUserDefinedProcessor]
public class FullDescriptionProcessor : IProcessor
{
public override IRow Process(IRow input, IUpdatableRow output)
 {
     string user = input.Get<string>("user");
     string des = input.Get<string>("des");
     string full_description = user.ToUpper() + "=>" + des;
     output.Set<string>("dt", input.Get<string>("dt"));
     output.Set<string>("full_description", full_description);
     output.Set<Guid>("new_guid", Guid.NewGuid());
     output.Set<Guid>("guid", input.Get<Guid>("guid"));
     return output.AsReadOnly();
 }
}
```

Bu kullanım örneği senaryosu, var olan sütunları--bu durumda, büyük harf "des", "kullanıcı" birleştirerek "full_description" adlı yeni bir sütun işlemci oluşturuyor. Ayrıca, bir GUID oluşturur ve özgün ve yeni GUID değerleri döndürür.

Önceki örnekte görebileceğiniz gibi C# sırasında yöntemi çağırabilirsiniz `output.Set` yöntem çağrısı.

Özel bir işlemci kullanan temel bir U-SQL komut dosyası örneği aşağıdadır:

```
DECLARE @input_file string = @"\usql-programmability\input_file.tsv";
DECLARE @output_file string = @"\usql-programmability\output_file.tsv";

@rs0 =
    EXTRACT
            guid Guid,
        dt String,
            user String,
            des String
    FROM @input_file USING Extractors.Tsv();

@rs1 =
     PROCESS @rs0
     PRODUCE dt String,
             full_description String,
             guid Guid,
             new_guid Guid
     USING new USQL_Programmability.FullDescriptionProcessor();

OUTPUT @rs1 TO @output_file USING Outputters.Text();
```

## <a name="use-user-defined-appliers"></a>Kullanıcı tanımlı appliers kullanın
U-SQL kullanıcı tanımlı bir uygulayıcı, özel C# işlevi dış tablo ifade bir sorgu tarafından döndürülen her satır için çağrılacak sağlar. Sağ giriş sol girdisinden her satır için değerlendirilen ve üretilen satır son çıktısının birleşiminden oluşur. Sol ve sağ girişine sütun kümesinde birleşimi APPLY işleci tarafından üretilen sütun listesi var.

Kullanıcı tanımlı uygulayıcı USQL seçin ifadesinin bir parçası çağrılır.

Kullanıcı tanımlı uygulayıcı tipik çağrı aşağıdaki gibi görünür:

```
SELECT …
FROM …
CROSS APPLYis used to pass parameters
new MyScript.MyApplier(param1, param2) AS alias(output_param1 string, …);
```

Bir SELECT ifadesinde appliers kullanma hakkında daha fazla bilgi için bkz [seçin, U-SQL seçerek OUTER APPLY ve CROSS APPLY](/u-sql/statements-and-expressions/select/from/select-selecting-from-cross-apply-and-outer-apply).

Kullanıcı tanımlı applier temel sınıf tanımına aşağıdaki gibidir:

```
public abstract class IApplier : IUserDefinedOperator
{
protected IApplier();

public abstract IEnumerable<IRow> Apply(IRow input, IUpdatableRow output);
}
```

Kullanıcı tanımlı bir uygulayıcı tanımlamak için oluşturmamız gerekir `IApplier` ile arabirim [`SqlUserDefinedApplier`], kullanıcı tanımlı applier tanımı için isteğe bağlı öznitelik.

```
[SqlUserDefinedApplier]
public class ParserApplier : IApplier
{
    public ParserApplier()
    {
        …
    }

    public override IEnumerable<IRow> Apply(IRow input, IUpdatableRow output)
    {
        …
    }
}
```

* Uygulama dış tablodaki her satır için çağrılır. Döndürür `IUpdatableRow` satır kümesi çıktı.
* Oluşturucu sınıf için kullanıcı tanımlı uygulayıcı parametreleri geçirmek için kullanılır.

**SqlUserDefinedApplier** türü kullanıcı tanımlı bir uygulayıcı kaydedilmesi gerektiğini gösterir. Bu sınıf devralınamaz.

**SqlUserDefinedApplier** olduğu **isteğe bağlı** kullanıcı tanımlı applier tanımı için.


Ana programlama nesneleri aşağıdaki gibidir:

```
public override IEnumerable<IRow> Apply(IRow input, IUpdatableRow output)
```

Giriş satır kümeleri olarak geçirilir `IRow` giriş. Çıktı satırları olarak oluşturulan `IUpdatableRow` çıkışı arabirimi.

Tek tek sütun adları çağırarak belirlenebilir `IRow` şema yöntemi.

```
ISchema schema = row.Schema;
var col = schema[i];
string val = row.Get<string>(col.Name)
```

Gelen gerçek veri değerlerini almak için `IRow`, Get() yöntemini kullanırız `IRow` arabirimi.

```
mycolumn = row.Get<int>("mycolumn")
```

Veya şema sütun adını kullanıyoruz:

```
row.Get<int>(row.Schema[0].Name)
```

Çıkış değerleri ile ayarlanmalıdır `IUpdatableRow` çıktı:

```
output.Set<int>("mycolumn", mycolumn)
```

Özel appliers sütunları ve ile tanımlanan değerleri yalnızca çıktı anlaşılması önemlidir `output.Set` yöntem çağrısı.

Gerçek çıkış çağırarak tetiklenir `yield return output.AsReadOnly();`.

Kullanıcı tanımlı applier parametreleri oluşturucuya geçirilebilir. Uygulayıcı, değişken sayıda temel U-SQL betiği applier arama sırasında tanımlanması gerekir sütunları döndürebilir.

```
new USQL_Programmability.ParserApplier ("all") AS properties(make string, model string, year string, type string, millage int);
```

Kullanıcı tanımlı applier örnek aşağıda verilmiştir:

```
[SqlUserDefinedApplier]
public class ParserApplier : IApplier
{
private string parsingPart;

public ParserApplier(string parsingPart)
{
    if (parsingPart.ToUpper().Contains("ALL")
    || parsingPart.ToUpper().Contains("MAKE")
    || parsingPart.ToUpper().Contains("MODEL")
    || parsingPart.ToUpper().Contains("YEAR")
    || parsingPart.ToUpper().Contains("TYPE")
    || parsingPart.ToUpper().Contains("MILLAGE")
    )
    {
    this.parsingPart = parsingPart;
    }
    else
    {
    throw new ArgumentException("Incorrect parameter. Please use: 'ALL[MAKE|MODEL|TYPE|MILLAGE]'");
    }
}

public override IEnumerable<IRow> Apply(IRow input, IUpdatableRow output)
{

    string[] properties = input.Get<string>("properties").Split(',');

    //  only process with correct number of properties
    if (properties.Count() == 5)
    {

    string make = properties[0];
    string model = properties[1];
    string year = properties[2];
    string type = properties[3];
    int millage = -1;

    // Only return millage if it is number, otherwise, -1
    if (!int.TryParse(properties[4], out millage))
    {
        millage = -1;
    }

    if (parsingPart.ToUpper().Contains("MAKE") || parsingPart.ToUpper().Contains("ALL")) output.Set<string>("make", make);
    if (parsingPart.ToUpper().Contains("MODEL") || parsingPart.ToUpper().Contains("ALL")) output.Set<string>("model", model);
    if (parsingPart.ToUpper().Contains("YEAR") || parsingPart.ToUpper().Contains("ALL")) output.Set<string>("year", year);
    if (parsingPart.ToUpper().Contains("TYPE") || parsingPart.ToUpper().Contains("ALL")) output.Set<string>("type", type);
    if (parsingPart.ToUpper().Contains("MILLAGE") || parsingPart.ToUpper().Contains("ALL")) output.Set<int>("millage", millage);
    }
    yield return output.AsReadOnly();            
}
}
```

Bu kullanıcı tarafından tanımlanan uygulayıcı için temel U-SQL betiği aşağıda verilmiştir:

```
DECLARE @input_file string = @"c:\usql-programmability\car_fleet.tsv";
DECLARE @output_file string = @"c:\usql-programmability\output_file.tsv";

@rs0 =
    EXTRACT
        stocknumber int,
        vin String,
        properties String
    FROM @input_file USING Extractors.Tsv();

@rs1 =
    SELECT
        r.stocknumber,
        r.vin,
        properties.make,
        properties.model,
        properties.year,
        properties.type,
        properties.millage
    FROM @rs0 AS r
    CROSS APPLY
    new USQL_Programmability.ParserApplier ("all") AS properties(make string, model string, year string, type string, millage int);

OUTPUT @rs1 TO @output_file USING Outputters.Text();
```

Bu kullanım örneği senaryosu kullanıcı tanımlı uygulayıcı araba Filo özellikleri için bir virgülle ayrılmış değer ayrıştırıcısı gibi davranır. Giriş dosyası satırları aşağıdaki gibi görünür:

```
103 Z1AB2CD123XY45889   Ford,Explorer,2005,SUV,152345
303 Y0AB2CD34XY458890   Chevrolet,Cruise,2010,4Dr,32455
210 X5AB2CD45XY458893   Nissan,Altima,2011,4Dr,74000
```

Tipik sekmeyle TSV dosyası gibi marka ve model araba özellikleri içeren özellikleri sütunu var. Bu özellikler için tablo sütunları Ayrıştırılan gerekir. Sağlanan uygulayıcı geçirilen parametresine bağlı olarak sonuç kümesinde dinamik bir dizi özellikleri oluşturmanızı sağlar. Tüm özellikler veya özellikleri yalnızca belirli bir dizi oluşturabilirsiniz.

    …USQL_Programmability.ParserApplier ("all")
    …USQL_Programmability.ParserApplier ("make")
    …USQL_Programmability.ParserApplier ("make&model")

Kullanıcı tanımlı uygulayıcı applier nesnesinin yeni bir örneğini çağrılabilir:

```
CROSS APPLY new MyNameSpace.MyApplier (parameter: “value”) AS alias([columns types]…);
```

Veya bir sarmalayıcı Üreteç yöntemi çağırmayı:

```csharp
    CROSS APPLY MyNameSpace.MyApplier (parameter: “value”) AS alias([columns types]…);
```

## <a name="use-user-defined-combiners"></a>Kullanıcı tanımlı combiners kullanın
Kullanıcı tanımlı Birleştirici veya UDC, özel mantığı temelinde, sol ve sağ satır satır birleştirmenizi sağlar. Birleştirme ifadesi ile kullanıcı tanımlı Birleştirici kullanıldı.

Bir Birleştirici hem giriş satır kümeleri, gruplandırma sütunları, beklenen sonuç şema ve ek bilgiler hakkında gerekli bilgileri sağlayan birleştirme ifade ile çağrılan.

Temel bir U-SQL komut dosyası bir Birleştirici çağırmak için aşağıdaki söz dizimi kullanırız:

```
Combine_Expression :=
    'COMBINE' Combine_Input
    'WITH' Combine_Input
    Join_On_Clause
    Produce_Clause
    [Readonly_Clause]
    [Required_Clause]
    USING_Clause.
```

Daha fazla bilgi için [birleştirme ifadesi (U-SQL)](/u-sql/statements-and-expressions/combine-expression).

Bir kullanıcı tanımlı Birleştirici tanımlamak için oluşturmamız gerekir `ICombiner` ile arabirim [`SqlUserDefinedCombiner`] özniteliği için bir kullanıcı tanımlı Birleştirici tanım isteğe bağlıdır.

Temel `ICombiner` sınıf tanımını:

```
public abstract class ICombiner : IUserDefinedOperator
{
protected ICombiner();
public virtual IEnumerable<IRow> Combine(List<IRowset> inputs,
       IUpdatableRow output);
public abstract IEnumerable<IRow> Combine(IRowset left, IRowset right,
       IUpdatableRow output);
}
```

Özel uygulanışı bir `ICombiner` arabirimi tanımı içermelidir bir `IEnumerable<IRow>` geçersiz kılma birleştirin.

```
[SqlUserDefinedCombiner]
public class MyCombiner : ICombiner
{

public override IEnumerable<IRow> Combine(IRowset left, IRowset right,
    IUpdatableRow output)
{
    …
}
}
```

**SqlUserDefinedCombiner** öznitelik türü bir kullanıcı tanımlı Birleştirici kaydedilmesi gerektiğini gösterir. Bu sınıf devralınamaz.

**SqlUserDefinedCombiner** Birleştirici modu özelliği tanımlamak için kullanılır. Kullanıcı tanımlı Birleştirici tanımı için isteğe bağlı bir özniteliktir.

CombinerMode     Mode

CombinerMode enum, şu değerleri alabilir:

* Tam (0) her çıkış satır tüm giriş satırları sol ve sağda aynı anahtar değerine sahip olabilecek bağlıdır.

* Sol (1) sol (ve büyük olasılıkla tüm satırların aynı anahtar değerine sahip sağa) tek bir giriş satır her çıkış satır bağlıdır.

* Sağ (2) sağa (ve büyük olasılıkla tüm satırların aynı anahtar değerine sahip soldan) tek bir giriş satır her çıkış satır bağlıdır.

* İç (3) tek bir giriş satır sol ve sağda aynı değere sahip her çıkış satır bağlıdır.

Örnek: [`SqlUserDefinedCombiner(Mode=CombinerMode.Left)`]


Ana programlama nesneleri şunlardır:

```csharp
    public override IEnumerable<IRow> Combine(IRowset left, IRowset right,
        IUpdatableRow output
```

Giriş satır kümeleri olarak geçirilir **sol** ve **doğru** `IRowset` arabirimin türü. Her iki satır kümeleri işleme için öğrenmeniz gerekir. Biz listeleme ve gerekirse, önbelleğe alacak şekilde yalnızca her arabirim bir kez sıralayabilirsiniz.

Tarafından önbelleğe alma işlemleri için bir liste oluşturabiliriz\<T\> tür bellek yapısı sonuç olarak bir LINQ Sorgu yürütmesi, özellikle listesi <`IRow`>. Anonim veri türü de numaralandırma sırasında kullanılabilir.

Bkz: [(C#) LINQ sorgularına giriş](/dotnet/csharp/programming-guide/concepts/linq/introduction-to-linq-queries) LINQ sorguları hakkında daha fazla bilgi ve [IEnumerable\<T\> arabirimi](/dotnet/api/system.collections.generic.ienumerable-1) IEnumerablehakkındadahafazlabilgiiçin\<T\> arabirimi.

Gelen gerçek veri değerlerini almak için `IRowset`, Get() yöntemini kullanırız `IRow` arabirimi.

```
mycolumn = row.Get<int>("mycolumn")
```

Tek tek sütun adları çağırarak belirlenebilir `IRow` şema yöntemi.

```
ISchema schema = row.Schema;
var col = schema[i];
string val = row.Get<string>(col.Name)
```

Veya şema sütun adını kullanarak:

```
c# row.Get<int>(row.Schema[0].Name)
```

LINQ ile genel numaralandırma aşağıdaki gibi görünür:

```
var myRowset =
            (from row in left.Rows
                          select new
                          {
                              Mycolumn = row.Get<int>("mycolumn"),
                          }).ToList();
```

Her iki satır kümeleri numaralandırma sonra tüm satırlarda döngü kullanacağız. Sol satır kümesindeki her bir satır için sunduğumuz Birleştirici koşulu karşılayan tüm satırları bulmak için kullanacağız.

Çıkış değerleri ile ayarlanmalıdır `IUpdatableRow` çıktı.

```
output.Set<int>("mycolumn", mycolumn)
```

Gerçek çıkış için arama tarafından tetiklenen `yield return output.AsReadOnly();`.

Bir Birleştirici örneği verilmiştir:

```
[SqlUserDefinedCombiner]
public class CombineSales : ICombiner
{

public override IEnumerable<IRow> Combine(IRowset left, IRowset right,
    IUpdatableRow output)
{
    var internetSales =
    (from row in left.Rows
          select new
          {
              ProductKey = row.Get<int>("ProductKey"),
              OrderDateKey = row.Get<int>("OrderDateKey"),
              SalesAmount = row.Get<decimal>("SalesAmount"),
              TaxAmt = row.Get<decimal>("TaxAmt")
          }).ToList();

    var resellerSales =
    (from row in right.Rows
     select new
     {
     ProductKey = row.Get<int>("ProductKey"),
     OrderDateKey = row.Get<int>("OrderDateKey"),
     SalesAmount = row.Get<decimal>("SalesAmount"),
     TaxAmt = row.Get<decimal>("TaxAmt")
     }).ToList();

    foreach (var row_i in internetSales)
    {
    foreach (var row_r in resellerSales)
    {

        if (
        row_i.OrderDateKey > 0
        && row_i.OrderDateKey < row_r.OrderDateKey
        && row_i.OrderDateKey == 20010701
        && (row_r.SalesAmount + row_r.TaxAmt) > 20000)
        {
        output.Set<int>("OrderDateKey", row_i.OrderDateKey);
        output.Set<int>("ProductKey", row_i.ProductKey);
        output.Set<decimal>("Internet_Sales_Amount", row_i.SalesAmount + row_i.TaxAmt);
        output.Set<decimal>("Reseller_Sales_Amount", row_r.SalesAmount + row_r.TaxAmt);
        }

    }
    }
    yield return output.AsReadOnly();
}
}
```

Bu kullanım örneği senaryosu, biz için perakende analiz raporu oluşturuyorsunuz. Birden fazla 20.000 $ maliyet ve, belirli bir zaman çerçevesi içinde normal perakende aracılığıyla daha hızlı Web sitesi üzerinden satış tüm ürünleri bulmak için kullanılan hedeftir.

Temel U-SQL betiği aşağıda verilmiştir. Normal bir birleştirme ve bir Birleştirici arasındaki mantık karşılaştırabilirsiniz:

```sql
DECLARE @LocalURI string = @"\usql-programmability\";

DECLARE @input_file_internet_sales string = @LocalURI+"FactInternetSales.txt";
DECLARE @input_file_reseller_sales string = @LocalURI+"FactResellerSales.txt";
DECLARE @output_file1 string = @LocalURI+"output_file1.tsv";
DECLARE @output_file2 string = @LocalURI+"output_file2.tsv";

@fact_internet_sales =
EXTRACT
    ProductKey int ,
    OrderDateKey int ,
    DueDateKey int ,
    ShipDateKey int ,
    CustomerKey int ,
    PromotionKey int ,
    CurrencyKey int ,
    SalesTerritoryKey int ,
    SalesOrderNumber String ,
    SalesOrderLineNumber  int ,
    RevisionNumber int ,
    OrderQuantity int ,
    UnitPrice decimal ,
    ExtendedAmount decimal,
    UnitPriceDiscountPct float ,
    DiscountAmount float ,
    ProductStandardCost decimal ,
    TotalProductCost decimal ,
    SalesAmount decimal ,
    TaxAmt decimal ,
    Freight decimal ,
    CarrierTrackingNumber String,
    CustomerPONumber String
FROM @input_file_internet_sales
USING Extractors.Text(delimiter:'|', encoding: Encoding.Unicode);

@fact_reseller_sales =
EXTRACT
    ProductKey int ,
    OrderDateKey int ,
    DueDateKey int ,
    ShipDateKey int ,
    ResellerKey int ,
    EmployeeKey int ,
    PromotionKey int ,
    CurrencyKey int ,
    SalesTerritoryKey int ,
    SalesOrderNumber String ,
    SalesOrderLineNumber  int ,
    RevisionNumber int ,
    OrderQuantity int ,
    UnitPrice decimal ,
    ExtendedAmount decimal,
    UnitPriceDiscountPct float ,
    DiscountAmount float ,
    ProductStandardCost decimal ,
    TotalProductCost decimal ,
    SalesAmount decimal ,
    TaxAmt decimal ,
    Freight decimal ,
    CarrierTrackingNumber String,
    CustomerPONumber String
FROM @input_file_reseller_sales
USING Extractors.Text(delimiter:'|', encoding: Encoding.Unicode);

@rs1 =
SELECT
    fis.OrderDateKey,
    fis.ProductKey,
    fis.SalesAmount+fis.TaxAmt AS Internet_Sales_Amount,
    frs.SalesAmount+frs.TaxAmt AS Reseller_Sales_Amount
FROM @fact_internet_sales AS fis
     INNER JOIN @fact_reseller_sales AS frs
     ON fis.ProductKey == frs.ProductKey
WHERE
    fis.OrderDateKey < frs.OrderDateKey
    AND fis.OrderDateKey == 20010701
    AND frs.SalesAmount+frs.TaxAmt > 20000;

@rs2 =
COMBINE @fact_internet_sales AS fis
WITH @fact_reseller_sales AS frs
ON fis.ProductKey == frs.ProductKey
PRODUCE OrderDateKey int,
        ProductKey int,
        Internet_Sales_Amount decimal,
        Reseller_Sales_Amount decimal
USING new USQL_Programmability.CombineSales();

OUTPUT @rs1 TO @output_file1 USING Outputters.Tsv();
OUTPUT @rs2 TO @output_file2 USING Outputters.Tsv();
```

Bir kullanıcı tanımlı Birleştirici applier nesnesinin yeni bir örneğini çağrılabilir:

```
USING new MyNameSpace.MyCombiner();
```


Veya bir sarmalayıcı Üreteç yöntemi çağırmayı:

```
USING MyNameSpace.MyCombiner();
```

## <a name="use-user-defined-reducers"></a>Kullanıcı tanımlı genişletin kullanın

U-SQL özel satır kümesi genişletin C# ' de kullanıcı tanımlı işleç Genişletilebilirlik Çerçevesi'ni kullanıp IReducer arabirimi uygulama yazmanızı sağlar.

Kullanıcı tanımlı Azaltıcı veya UDR, gereksiz satırlar (alma) veri ayıklama sırasında ortadan kaldırmak için kullanılabilir. Ayrıca, işlemek ve satırları ve sütunları değerlendirmek için de kullanılabilir. Programlama mantığını bağlı olarak, bu da ayıklanacak hangi satırların gereksinim tanımlayabilirsiniz.

UDR sınıf tanımlamak için oluşturmamız gerekir bir `IReducer` isteğe bağlı bir arabirimle `SqlUserDefinedReducer` özniteliği.

Bu sınıf arabirimi için bir tanım içermelidir `IEnumerable` arabirimi satır kümesi geçersiz.

```
[SqlUserDefinedReducer]
public class EmptyUserReducer : IReducer
{

    public override IEnumerable<IRow> Reduce(IRowset input, IUpdatableRow output)
    {
        …
    }

}
```

**SqlUserDefinedReducer** öznitelik türü bir kullanıcı tanımlı Azaltıcı kaydedilmesi gerektiğini gösterir. Bu sınıf devralınamaz.
**SqlUserDefinedReducer** kullanıcı tanımlı Azaltıcı tanımı için isteğe bağlı bir özniteliktir. IsRecursive özelliği tanımlamak için kullanılır.

* bool IsRecursive    
* **doğru** ilişkilendirilebilir ve yer değiştirebilirlik bu azaltıcı olup olmadığını gösterir =

Ana programlama nesneler **giriş** ve **çıkış**. Giriş nesnesi, giriş satırları listeleme için kullanılır. Çıkış, azaltma etkinlik sonucunda çıktı satırları ayarlamak için kullanılır.

Girdi satırları numaralandırma için kullandığımız `Row.Get` yöntemi.

```
foreach (IRow row in input.Rows)
{
    row.Get<string>("mycolumn");
}
```

Parametresi için `Row.Get` yöntemdir parçası olarak geçirilen bir sütun `PRODUCE` sınıfının `REDUCE` temel U-SQL komut dosyası ifadesi. Biz burada doğru veri türünü de kullanmanız gerekir.

Çıkış için kullan `output.Set` yöntemi.

Bu özel Azaltıcı yalnızca ile tanımlanan değerleri çıkarır anlaşılması önemlidir `output.Set` yöntem çağrısı.

```
output.Set<string>("mycolumn", guid);
```

Gerçek Azaltıcı çıkış çağırarak tetiklenir `yield return output.AsReadOnly();`.

Bir azaltıcı örneği verilmiştir:

```
[SqlUserDefinedReducer]
public class EmptyUserReducer : IReducer
{

    public override IEnumerable<IRow> Reduce(IRowset input, IUpdatableRow output)
    {
    string guid;
    DateTime dt;
    string user;
    string des;

    foreach (IRow row in input.Rows)
    {
        guid = row.Get<string>("guid");
        dt = row.Get<DateTime>("dt");
        user = row.Get<string>("user");
        des = row.Get<string>("des");

        if (user.Length > 0)
        {
        output.Set<string>("guid", guid);
        output.Set<DateTime>("dt", dt);
        output.Set<string>("user", user);
        output.Set<string>("des", des);

        yield return output.AsReadOnly();
        }
    }
    }

}
```

Bu kullanım örneği senaryosu, bir boş kullanıcı adı ile satırları Azaltıcı atlıyor. Her bir satır kümesinde, her gerekli sütun okur, sonra kullanıcı adının uzunluğu değerlendirir. Yalnızca kullanıcı adı değer uzunluğu 0'dan büyük olması durumunda gerçek satır çıkarır.

Aşağıdaki özel Azaltıcı kullanan temel U-SQL betiğidir:

```
DECLARE @input_file string = @"\usql-programmability\input_file_reducer.tsv";
DECLARE @output_file string = @"\usql-programmability\output_file.tsv";

@rs0 =
    EXTRACT
            guid string,
        dt DateTime,
            user String,
            des String
    FROM @input_file 
    USING Extractors.Tsv();

@rs1 =
    REDUCE @rs0 PRESORT guid
    ON guid  
    PRODUCE guid string, dt DateTime, user String, des String
    USING new USQL_Programmability.EmptyUserReducer();

@rs2 =
    SELECT guid AS start_id,
           dt AS start_time,
           DateTime.Now.ToString("M/d/yyyy") AS Nowdate,
           USQL_Programmability.CustomFunctions.GetFiscalPeriodWithCustomType(dt).ToString() AS start_fiscalperiod,
           user,
           des
    FROM @rs1;

OUTPUT @rs2 
    TO @output_file 
    USING Outputters.Text();
```
