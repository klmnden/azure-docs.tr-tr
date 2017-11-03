---
title: "Azure Data Lake için U-SQL Programlama Kılavuzu | Microsoft Docs"
description: "Bir bulut tabanlı büyük veri platformu oluşturmanıza olanak sağlayan bir hizmetler kümesi olan Azure Data Lake içinde hakkında bilgi edinin."
services: data-lake-analytics
documentationcenter: 
author: saveenr
manager: saveenr
ms.assetid: 63be271e-7c44-4d19-9897-c2913ee9599d
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/30/2017
ms.author: saveenr
ms.openlocfilehash: bba8fff7997340e563c604f571604ee8d06eb719
ms.sourcegitcommit: 804db51744e24dca10f06a89fe950ddad8b6a22d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/30/2017
---
# <a name="u-sql-programmability-guide"></a>U-SQL Programlama Kılavuzu

U-SQL, büyük veri türü için iş yüklerinin tasarlanmış bir sorgu dildir. U-SQL benzersiz özellikler SQL benzeri tanımlayıcı dili genişletilebilirlik ve C# tarafından sağlanan programlama ile birleşimidir. Bu kılavuzda, biz genişletilebilirlik ve C# kullanarak etkin U-SQL dili ile programlama yoğunlaşabilirsiniz.

## <a name="requirements"></a>Gereksinimler

İndirme ve yükleme [Visual Studio için Azure Data Lake Araçları](https://www.microsoft.com/download/details.aspx?id=49504).

## <a name="get-started-with-u-sql"></a>U-SQL ile çalışmaya başlama  

Aşağıdaki U-SQL betiği bakın:

```
@a  = 
  SELECT * FROM 
    (VALUES
       ("Contoso",   1500.0, "2017-03-39"),
       ("Woodgrove", 2700.0, "2017-04-10")
    ) AS D( customer, amount );

@results =
  SELECT
    customer,
    amount,
    date
  FROM @a;    
```

Bu komut dosyası iki satır kümeleri tanımlar: `@a` ve `@results`. Satır kümesi `@results` gelen tanımlanan `@a`.

## <a name="c-types-and-expressions-in-u-sql-script"></a>C# türleri ve U-SQL betiği ifadeleri

Bir C# ifadesi gibi U-SQL mantıksal işlemleriyle birlikte bir U-SQL ifadesidir `AND`, `OR`, ve `NOT`. U-SQL deyimleri seçin, ayıklama, kullanılabilir nerede, otomatik olarak sahip olmak ve GROUP BY BİLDİRİN. Örneğin, aşağıdaki komut dosyasını bir dize bir DateTime değeri olarak ayrıştırır.

```
@results =
  SELECT
    customer,
    amount,
    DateTime.Parse(date) AS date
  FROM @a;    
```

Aşağıdaki kod parçacığını bir dizeyi DateTime değeri DECLARE deyimi olarak ayrıştırır.

```
DECLARE @d = DateTime.Parse("2016/01/01");
```

### <a name="use-c-expressions-for-data-type-conversions"></a>Veri türü dönüştürmeleri için C# ifadeleri kullanma

Aşağıdaki örnek, C# ifadeler kullanarak bir datetime veri dönüştürme nasıl yapabilirsiniz gösterir. Bu belirli bir senaryoda, dize TarihSaat veri gece 00:00:00 saat gösterimi standart DateTime değerine dönüştürülür.

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

### <a name="use-c-expressions-for-todays-date"></a>Bugünün tarihini C# ifadeleri kullanma

Bugünün tarihini çıkarmak için aşağıdaki C# ifade kullanabilirsiniz:`DateTime.Now.ToString("M/d/yyyy")`

Bu ifade bir komut dosyası kullanma örneği şöyledir:

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
## <a name="using-net-assemblies"></a>.NET derlemelerini kullanma

U-SQL'nin genişletilebilirlik modeli yoğun .NET derlemelerden özel kod ekleme olanağı kullanır. 

### <a name="register-a-net-assembly"></a>Bir .NET derlemesi kaydetme

Kullanım `CREATE ASSEMBLY` deyimi bir .NET derlemesi bir U-SQL veritabanı içine koyun. U-SQL betikleri bu derlemeler kullanarak daha sonra kullanabileceğiniz `REFERENCE ASSEMBLY` deyimi. 

Aşağıdaki kod, bir derlemeyi kaydetmeye gösterilmektedir:

```
CREATE ASSEMBLY MyDB.[MyAssembly]
   FROM "/myassembly.dll";
```

Aşağıdaki kod, bir derleme başvurusu gösterilmektedir:

```
REFERENCE ASSEMBLY MyDB.[MyAssembly];
```

Başvurun [derleme kayıt yönergeleri](https://blogs.msdn.microsoft.com/azuredatalake/2016/08/26/how-to-register-u-sql-assemblies-in-your-u-sql-catalog/) , bu konuda daha ayrıntılı ele alınmaktadır.


### <a name="use-assembly-versioning"></a>Derleme sürümü oluşturma kullanın
Şu anda, U-SQL .NET Framework sürüm 4.5 kullanır. Bu nedenle, kendi derlemeleri çalışma zamanı bu sürümü ile uyumlu olduğundan emin olun.

U-SQL kodun bir 64-bit (x 64) biçiminde daha önce belirtildiği gibi. Bu nedenle, kodunuzu x64 üzerinde çalıştırmak için derlendiğinden emin olun. Aksi takdirde, daha önce gösterilen yanlış biçim hatası alırsınız.

Her derleme dll dosyasını karşıya ve farklı bir çalışma zamanı, yerel bir derleme veya bir yapılandırma dosyası gibi bir kaynak dosya en fazla 400 MB olabilir. Kaynak dağıtma aracılığıyla ya da derlemeler ve ek dosyalarına başvuruları yoluyla dağıtılan kaynakları toplam boyutu 3 GB aşamaz.

Son olarak, her U-SQL veritabanı herhangi verilen derleme bir sürümü yalnızca içerebileceğini unutmayın. Örneğin, sürüm 7 ve sürüm 8 NewtonSoft Json.Net kitaplığının ihtiyacınız varsa, bunları iki farklı veritabanlarında kaydetmeniz gerekir. Ayrıca, her komut dosyası yalnızca bir verilen derleme DLL sürümüne başvurabilir. Bu bakımdan, U-SQL C# derleme yönetimi ve sürüm oluşturma semantiğini izler.

## <a name="use-user-defined-functions-udf"></a>Kullanıcı tanımlı işlevler kullanın: UDF
U-SQL kullanıcı tanımlı işlevler veya UDF, parametreleri kabul, (örneğin, karmaşık bir hesaplama) bir eylem gerçekleştirmek ve bu eylem bir değer olarak sonuç yordamları programlama. UDF dönüş değerini, yalnızca tek bir skaler olabilir. U-SQL UDF temel U-SQL betiği diğer C# skaler bir işlev gibi çağrılabilir.

U-SQL kullanıcı tanımlı işlevler olarak başlatma öneririz **ortak** ve **statik**.

```
public static string MyFunction(string param1)
{
    return "my result";
}
```

İlk bir UDF oluşturma basit örneğe bakalım.

Bu kullanım örneği senaryosu biz mali ayı ilk oturum açma için belirli kullanıcı ve Mali Çeyrek yıl de dahil olmak üzere mali dönemi belirlemeniz gerekir. Senaryomuzda yılın ilk mali ayı Haziran ' dir.

Mali dönemi hesaplamak için size aşağıdaki C# işlevi tanıtmaktadır:

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

Mali ayı ve üç aylık dönem hesaplar ve bir string değeri döndürür. Haziran için ilk mali Çeyreğin ilk ayı "Q1:P1" kullanırız. Temmuz, biz "Q1:P2" kullanın ve benzeri.

Bu, biz U-SQL Projemizin kullanacaksanız bir normal C# işlevdir.

Arka plan kodu bölüm bu senaryoda, nasıl göründüğünü aşağıda verilmiştir:

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

Şimdi Biz bu işlev temel U-SQL komut dosyasından çağıracaksınız. Bunu yapmak için bu durumda NameSpace.Class.Function(parameter) olduğu ad alanı dahil işlevi tam olarak nitelenmiş bir ad vermek sahibiz.

```
USQL_Programmability.CustomFunctions.GetFiscalPeriod(dt)
```

Gerçek U-SQL temel betiği aşağıdadır:

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

Çıkış dosyası komut dosyası yürütme aşağıdadır:

```
0d8b9630-d5ca-11e5-8329-251efa3a2941,2016-02-11T07:04:17.2630000-08:00,2016-06-01T00:00:00.0000000,"Q3:8","User1",""

20843640-d771-11e5-b87b-8b7265c75a44,2016-02-11T07:04:17.2630000-08:00,2016-06-01T00:00:00.0000000,"Q3:8","User2",""

301f23d2-d690-11e5-9a98-4b4f60a1836f,2016-02-11T09:01:33.9720000-08:00,2016-06-01T00:00:00.0000000,"Q3:8","User3",""
```

Bu örnek, basit bir satır içi U-SQL UDF'de kullanımını gösterir.

### <a name="keep-state-between-udf-invocations"></a>UDF çağrılarını arasında durumu tutun
U-SQL C# programlama nesnelerini daha, arka plan kodu Genel değişkenler aracılığıyla etkileşim kullanan gelişmiş. Aşağıdaki iş kullanım örneği senaryosu bakalım.

Büyük kuruluşlarda, kullanıcıların iç uygulamaları çeşitleri arasında geçiş yapabilirsiniz. Bunlar, Microsoft Dynamics CRM, Powerbı vb. içerebilir. Müşteriler kullanıcılar kullanım eğilimleri nelerdir, farklı uygulamalar arasında nasıl geçiş, bir telemetri analizi uygulamak istediğiniz ve benzeri. Uygulama kullanımı en iyi duruma getirme iş için belirtilir. Farklı uygulamalar veya oturum açma belirli yordamları birleştirmek de isteyebilirsiniz.

Bu hedefe ulaşmak için biz oturum kimliklerini belirlemek ve öteleme arasında oluştu son oturumun süresi gerekir.

Bir önceki oturum açma bulma ve ardından bu oturum açma aynı uygulama için oluşturulan tüm oturumları atamak gerekir. U-SQL temel betiği bize ÖTELEME işlevi zaten hesaplanmış sütunlar üzerinde hesaplamalar uygulamak izin vermeyen ilk iştir. Belirli bir oturum için aynı süre içinde tüm oturumları tutmak sahibiz ikinci iştir.

İçinde bir arka plan kodu bölümü genel değişkeni kullanırız bu sorunu çözmek için: `static public string globalSession;`.

Bu genel değişkeni tüm satır kümesi için komut dosyası yürütme sırasında uygulanır.

U-SQL programımız arka plan kodu bölümünü şöyledir:

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

Bu örnek, genel değişkeni gösterir `static public string globalSession;` içinde kullanılan `getStampUserSession` işlevi ve oturum parametresi değiştirildiğinde her zaman yeniden.

U-SQL temel komut aşağıdaki gibidir:

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

İşlev `USQLApplication21.UserSession.getStampUserSession(UserSessionTimestamp)` burada ikinci bellek satır kümesi hesaplama sırasında çağrılır. Bunu geçirir `UserSessionTimestamp` sütun ve kadar değeri döndürür `UserSessionTimestamp` değişti.

Çıktı dosyası aşağıdaki gibidir:

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

Bu örnek bir genel değişkeni tüm bellek satır kümesi için uygulanan bir arka plan kodu bölüm içinde kullandığımız daha karmaşık bir kullanım örneği senaryosu gösterilmektedir.

## <a name="use-user-defined-types-udt"></a>Kullanıcı tanımlı türler kullanın: UDT
Kullanıcı tanımlı türler veya UDT, başka bir programlama, U-SQL özelliğidir. U-SQL UDT bir normal C# kullanıcı tanımlı tür gibi davranır. C# yerleşik ve özel kullanıcı tanımlı türler kullanılmasına kesin türü belirtilmiş bir dil değil.

U-SQL örtük olarak serileştirmek veya satır kümeleri tepe arasında UDT geçirildiğinde rasgele atama anlayabileceği olamaz. Bu, kullanıcının açık bir biçimlendirici IFormatter arabirimini kullanarak sağlamak sahip anlamına gelir. Bu, U-SQL ile serileştirme sağlar ve yöntemleri için UDT anlayabileceği.

> [!NOTE]
> U-SQL'nin yerleşik ayıklayıcıları ve outputters şu anda serileştirmek veya seri durumdan UDT veri ya da hatta IFormatter kümesiyle dosyalarından. Bu nedenle çıktı ifadesiyle bir dosyaya UDT veri yazma veya bir ayıklayıcısı ile okuma, bir string veya byte dizisi olarak geçirmeniz gerekir. Ardından serileştirme çağırın ve kod (diğer bir deyişle, UDT'ın ToString() yöntemini) açıkça seri durumundan çıkarma. Kullanıcı tanımlı ayıklayıcıları ve outputters, diğer yandan okuyabilir ve atama yazma.

Biz UDT AYIKLAYICISI veya OUTPUTTER (dışında önceki seçin), aşağıda gösterildiği gibi kullanırsanız:

```
@rs1 =
    SELECT 
        MyNameSpace.Myfunction_Returning_UDT(filed1) AS myfield
    FROM @rs0;

OUTPUT @rs1 
    TO @output_file 
    USING Outputters.Text();
```

Biz, şu hata iletisini alıyorsunuz:

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

İçinde outputter UDT ile çalışmak için ya da ToString() yöntemiyle dize veya bir özel outputter oluşturmak için seri hale getirmek sahibiz.

Atama, grup tarafından şu anda kullanılamaz. UDT grubu tarafından kullanılıyorsa, aşağıdaki hata oluşturulur:

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

UDT tanımlamak için biz gerekir:

* Şu ad alanlarından ekleyin:

```
using Microsoft.Analytics.Interfaces
using System.IO;
```

* Ekleme `Microsoft.Analytics.Interfaces`, UDT arabirimler için gerekli olduğu. Ayrıca, `System.IO` IFormatter arabirimi tanımlamak için gerekli.

* Kullanılan tanımlı bir tür SqlUserDefinedType özniteliğiyle tanımlayın.

**SqlUserDefinedType** bir tür tanımı bir kullanıcı tanımlı tür (UDT) U-SQL'de bir bütünleştirilmiş işaretlemek için kullanılır. Öznitelik özellikleri UDT fiziksel özelliklerini yansıtır. Bu sınıf devralınan olamaz.

SqlUserDefinedType UDT tanımı için gerekli bir özniteliktir.

Sınıf oluşturucu:  

* SqlUserDefinedTypeAttribute (türü biçimlendirici)

* Biçimlendirici yazın: gerekli bir UDT biçimlendirici--özellikle tanımlamak için parametre türü `IFormatter` arabirimi gerekir bayraklarıdır burada.

```
[SqlUserDefinedType(typeof(MyTypeFormatter))]
public class MyType
{ … }
```

* Tipik UDT, ayrıca aşağıdaki örnekte gösterildiği gibi IFormatter arabirimi tanımını gerektirir:

```
public class MyTypeFormatter : IFormatter<MyType>
{
    public void Serialize(MyType instance, IColumnWriter writer, ISerializationContext context)
    { … }

    public MyType Deserialize(IColumnReader reader, ISerializationContext context)
    { … }
}
```

`IFormatter` Arabirimi serileştirir ve kök türüne sahip bir nesne grafiğinin XML'deki serileştiren \<typeparamref name = "T" >.

\<typeparam name = "T" > serileştirme ve seri durumdan kök türü için Nesne grafiği.

* **Seri durumdan**: Sağlanan akış verilerini XML'deki serileştirir ve grafik nesnelerinin reconstitutes.

* **Seri hale**: bir nesne ya da nesneleriyle verilen kök sağlanan akış grafiği Serileştirir.

`MyType`Örnek: türünün örneği.  
`IColumnWriter`yazıcı / `IColumnReader` okuyucu: temel sütun akış.  
`ISerializationContext`Bağlam: seri hale getirme sırasında akış kaynak veya hedef bağlamının belirten bayrakları kümesini tanımlayan Enum.

* **Ara**: kaynak veya hedef bağlamı kalıcı depolama olmadığını belirtir.

* **Kalıcılığı**: kaynak veya hedef bağlamı kalıcı depolama alanının olduğunu belirtir.

Bir normal C# türü olarak bir U-SQL UDT tanımı geçersiz kılmaları işleçlerini gibi içerebilir +/ == /! =. Statik yöntemler de içerir. Biz bu UDT U-SQL MIN toplama işlevi için parametre olarak kullanacaksanız, örneğin, biz tanımlamak varsa < işleci geçersiz kılma.

Bu kılavuzda daha önce şu biçimde belirli tarihten mali dönem tanımlaması için bir örnek gösterilen `Qn:Pn (Q1:P10)`. Aşağıdaki örnek, mali dönemi değerlerini için özel bir tür tanımlamak gösterilmiştir.

Aşağıdaki özel UDT ve IFormatter arabirimi arka plan kodu bölümle örneğidir:

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

İki sayı türü tanımlanmış içerir: üç aylık dönem ve ay. İşleçler `==/!=/>/<` ve statik yöntemi `ToString()` burada tanımlanır.

Daha önce belirtildiği gibi UDT SELECT ifadelerde kullanılabilir, ancak OUTPUTTER/AYIKLAYICISI özel serileştirme kullanılamaz. Ya da bir dize olarak serileştirilmesi sahip `ToString()` veya özel bir OUTPUTTER/AYIKLAYICISI kullanılır.

Şimdi şimdi UDT kullanımını tartışın. Arka plan kodu bölümünde, biz bizim GetFiscalPeriod işlevi için değiştirilmiştir:

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

Gördüğünüz gibi bizim FiscalPeriod türü değeri döndürür.

Daha fazla U-SQL temel komut dosyasında kullanmak nasıl bir örneği burada sunuyoruz. Bu örnek, farklı UDT çağırma formlardan U-SQL betiği gösterir.

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

Bir tam arka plan kodu bölümü bir örneği burada verilmiştir:

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
Kullanıcı tanımlı toplamlarda, toplama ile ilgili olan tüm işlevler out-of--box U-SQL ile gönderilmeyen ' dir. Örneğin, özel matematik hesaplamaları, dize birleştirmeler, işlemeleri dizeler vb. ile gerçekleştirmek için bir toplama olabilir.

Kullanıcı tanımlı toplama temel sınıf tanımı aşağıdaki gibidir:

```c#
    [SqlUserDefinedAggregate]
    public abstract class IAggregate<T1, T2, TResult> : IAggregate
    {
        protected IAggregate();

        public abstract void Accumulate(T1 t1, T2 t2);
        public abstract void Init();
        public abstract TResult Terminate();
    }
```

**SqlUserDefinedAggregate** türü kullanıcı tanımlı toplama olarak kayıtlı olduğunu belirtir. Bu sınıf devralınan olamaz.

SqlUserDefinedType özniteliği **isteğe bağlı** UDAGG tanımı.


Üç soyut parametreleri geçirmek temel sınıf sağlar: iki giriş parametreleri ve bir sonucu olarak. Veri türleri değişkendir ve sınıf devralma sırasında tanımlanması gerekir.

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

* **Init** hesaplaması sırasında her grup için bir kez çağırır. Bu, her toplama grubu için bir başlatma yordamının sağlar.  
* **Accumulate** her bir değer için bir kez çalıştırılır. Toplama algoritması ana işlevsellik sağlar. Sınıf devralma sırasında tanımlanan çeşitli veri türlerine sahip toplama değerlerini için kullanılabilir. Değişken veri türünde iki parametre kabul edebilir.
* **Sonlandırma** her grup için sonucu çıkarmak için işlem sonunda toplama grubu başına bir kez çalıştırılır.

Doğru giriş ve çıkış veri türlerinin bildirmek için sınıf tanımını aşağıdaki şekilde kullanın:

```
public abstract class IAggregate<T1, T2, TResult> : IAggregate
```

* T1: ilk parametre birikmesine
* T2: ilk parametre birikmesine
* TResult: dönüş türü Sonlandır

Örneğin:

```
public class GuidAggregate : IAggregate<string, int, int>
```

or

```
public class GuidAggregate : IAggregate<string, string, string>
```

### <a name="use-udagg-in-u-sql"></a>U-SQL UDAGG kullanın
UDAGG kullanmak için ilk kod arkasında tanımlayın veya daha önce bahsedildiği gibi varolan programlama DLL başvuru.

Ardından aşağıdaki sözdizimini kullanın:

```
AGG<UDAGG_functionname>(param1,param2)
```

UDAGG bir örneği burada verilmiştir:

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

Bu kullanım örneği senaryosu sınıfı GUID'leri belirli kullanıcılar için birleştirme.

## <a name="use-user-defined-objects-udo"></a>Kullanıcı tanımlı nesneler kullanın: UDO
U-SQL kullanıcı tanımlı nesneler veya UDO adlı özel programlama nesneleri tanımlamanıza olanak sağlar.

U-SQL UDO listesi aşağıdadır:

* Kullanıcı tanımlı ayıklayıcıları
    * Extract satır satır
    * Özel yapılandırılmış dosyalarından veri ayıklama gerçekleştirmek için kullanılır

* Kullanıcı tanımlı outputters
    * Çıkış satır satır
    * Çıktı özel veri türleri veya özel dosya biçimleri kullanılan

* Kullanıcı tanımlı işlemcileri
    * Bir satır almak ve bir satır üretir
    * Sütun sayısını azaltın veya varolan bir sütun kümesinden türetilmiş değerlerle yeni sütun oluşturmak için kullanılır

* Kullanıcı tanımlı appliers
    * Bir satır alın ve n satır 0 üretir
    * Dış/ARASI UYGULA ile kullanılan

* Kullanıcı tanımlı combiners
    * Satır kümeleri--kullanıcı tanımlı birleştirmeler birleştirir

* Kullanıcı tanımlı reducers
    * N satırları almak ve bir satır üretir
    * Satır sayısını azaltmak için kullanılan

UDO genellikle açıkça U-SQL komut dosyasında aşağıdaki U-SQL deyimlerini bir parçası olarak adlandırılır:

* EXTRACT
* ÇIKTI
* İŞLEM
* BİRLEŞTİRME
* AZALTMA

> [!NOTE]  
> UDO'ın 0,5 Gb bellek tüketmesine sınırlıdır.  Bu bellek kısıtlaması yerel yürütmeleri için geçerli değildir.

## <a name="use-user-defined-extractors"></a>Kullanıcı tanımlı ayıklayıcıları kullanın
U-SQL EXTRACT deyimi kullanarak dış veri almanıza izin verir. Bir ayıklama deyimi yerleşik UDO ayıklayıcıları kullanabilirsiniz:  

* *Extractors.Text()*: farklı kodlamaları sınırlandırılmış metin dosyalarından ayıklama sağlar.

* *Extractors.Csv()*: ayıklama virgülle ayrılmış değer (CSV) dosyaları farklı kodlamaları sağlar.

* *Extractors.Tsv()*: ayıklama sekmeyle ayrılmış değerinden farklı kodlamaları (TSV) dosyaları sağlar.

Özel ayıklayıcısı geliştirmek yararlı olabilir. Biz aşağıdaki görevlerden herhangi birini yapmak istiyorsanız, bu veri içe aktarma sırasında yararlı olabilir:

* Giriş verisi sütunları bölme ve tek tek değerleri değiştirerek değiştirin. İŞLEMCİ işlevselliğini sütunları birleştirmek için daha iyi olur.
* Web sayfaları ve e-posta gibi yapılandırılmamış veriler veya XML/JSON gibi yarı yapılandırılmamış veriler ayrıştırılamadı.
* Desteklenmeyen kodlama verileri ayrıştırılamıyor.

Bir kullanıcı tarafından tanımlanan ayıklayıcısı veya Opyalanan tanımlamak için oluşturmamız gerekir bir `IExtractor` arabirimi. Tüm giriş parametreleri için sütun/satır sınırlayıcıları gibi ayıklayıcısı ve kodlama, sınıf oluşturucuda tanımlanması gerekir. `IExtractor` Arabirimi için bir tanım da içermelidir `IEnumerable<IRow>` gibi geçersiz kıl:

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

**SqlUserDefinedExtractor** öznitelik türü kullanıcı tanımlı bir ayıklayıcısı kayıtlı olduğunu gösterir. Bu sınıf devralınan olamaz.

SqlUserDefinedExtractor Opyalanan tanımı için isteğe bağlı bir özniteliktir. Opyalanan nesnesi için AtomicFileProcessing özelliği tanımlamak için kullanılır.

* bool AtomicFileProcessing   

* **doğru** bu ayıklayıcısı atomik giriş dosyaları (JSON, XML,...) gerektirdiğini belirtir =
* **yanlış** bu ayıklayıcısı bölünmüş / dağıtılmış dosyalarla (CSV, SEQ,...) başa belirtir =

Ana Opyalanan programlamasına nesneler **giriş** ve **çıkış**. Giriş nesnesi girdi verisi olarak numaralandırmak için kullanılan `IUnstructuredReader`. Çıkış nesnesi ayıklayıcısı etkinlik sonucunda çıkış veri kümesi için kullanılır.

Giriş verisi üzerinden erişilen `System.IO.Stream` ve `System.IO.StreamReader`.

Giriş sütunları numaralandırması için biz ilk giriş akışı satır ayırıcı kullanarak ayırın.

```
foreach (Stream current in input.Split(my_row_delimiter))
{
…
}
```

Ardından, daha fazla giriş satır sütun bölün.

```
foreach (Stream current in input.Split(my_row_delimiter))
{
…
    string[] parts = line.Split(my_column_delimiter);
    foreach (string part in parts)
    { … }
}
```

Çıktı veri kümesi için kullanırız `output.Set` yöntemi.

Özel ayıklayıcısı yalnızca sütunlar ve tanımlanmış değerler ile çıkış çıktıları anlamak önemlidir. Yöntem çağrısının ayarlayın.

```
output.Set<string>(count, part);
```

Gerçek ayıklayıcısı çıkış çağırarak tetiklenir `yield return output.AsReadOnly();`.

Ayıklayıcısı örnek aşağıda verilmiştir:

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

Bu kullanım örneği senaryosu ayıklayıcısı "guid" sütunu için GUID oluşturur ve "kullanıcı" sütundaki değerleri büyük harflere dönüştürür. Özel ayıklayıcıları giriş verilerini ayrıştırma ve onu düzenleme daha karmaşık sonuçlara yol açabilir.

Özel ayıklayıcısı kullanan temel U-SQL komut dosyası aşağıdadır:

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

## <a name="use-user-defined-outputters"></a>Kullanıcı tanımlı outputters kullanın
Kullanıcı tanımlı outputter yerleşik U-SQL işlevselliğini genişletmek izin veren başka bir U-SQL UDO ' dir. Benzer şekilde ayıklayıcısı vardır birkaç yerleşik outputters.

* *Outputters.Text()*: veri farklı kodlamaları sınırlandırılmış metin dosyasına yazar.
* *Outputters.Csv()*: veri farklı kodlamaları virgülle ayrılmış değer (CSV) dosyasına yazar.
* *Outputters.Tsv()*: veri farklı kodlamaları sekmesini ayrılmış değerler (TSV) dosyasına yazar.

Özel outputter özel tanımlanmış bir biçimde veri yazmak sağlar. Aşağıdaki görevler için yararlı olabilir:

* Veri yarı yapılandırılmış veya yapılandırılmamış dosyalara yazma.
* Veri yazma değil Kodlamalar desteklenir.
* Çıktı verilerini değiştirme veya özel öznitelikleri ekleme.

Kullanıcı tanımlı outputter tanımlamak için oluşturmamız gerekir `IOutputter` arabirimi.

Aşağıdadır temel `IOutputter` sınıfı uygulama:

```
public abstract class IOutputter : IUserDefinedOperator
{
    protected IOutputter();

    public virtual void Close();
    public abstract void Output(IRow input, IUnstructuredWriter output);
}
```

Sütun/satır sınırlayıcıları, kodlama ve vb. gibi outputter tüm giriş parametreleri sınıfı oluşturucuda tanımlanması gerekir. `IOutputter` Arabirimi için bir tanım da içermelidir `void Output` geçersiz kılar. Öznitelik `[SqlUserDefinedOutputter(AtomicFileProcessing = true)` atomik dosya işleme için isteğe bağlı olarak ayarlanabilir. Daha fazla bilgi için aşağıdaki ayrıntılarına bakın.

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

* `Output`Giriş her satır için çağrılır. Döndürdüğü `IUnstructuredWriter output` satır kümesi.
* Oluşturucu sınıfı için kullanıcı tanımlı outputter parametreleri geçirmek için kullanılır.
* `Close`İsteğe bağlı olarak pahalı durumunu serbest bırakmak veya son satırını zaman yazıldı belirlemek için geçersiz kılmak için kullanılır.

**SqlUserDefinedOutputter** öznitelik türü kullanıcı tanımlı bir outputter kayıtlı olduğunu gösterir. Bu sınıf devralınan olamaz.

SqlUserDefinedOutputter, kullanıcı tanımlı outputter tanımı için isteğe bağlı bir özniteliktir. AtomicFileProcessing özelliği tanımlamak için kullanılır.

* bool AtomicFileProcessing   

* **doğru** bu outputter atomik çıktı dosyaları (JSON, XML,...) gerektirdiğini belirtir =
* **yanlış** bu outputter bölünmüş / dağıtılmış dosyalarla (CSV, SEQ,...) başa belirtir =

Ana programlamasına nesneler **satır** ve **çıkış**. **Satır** nesne çıktı verisi olarak numaralandırmak için kullanılan `IRow` arabirimi. **Çıktı** hedef dosyasına çıkış veri kümesi için kullanılır.

Çıktı verilerini üzerinden erişilen `IRow` arabirimi. Çıktı verilerini bir satır aynı anda geçirilir.

Değerlerini ayrı ayrı IRow arabiriminin Get yöntemini çağırarak numaralandırılmıştır:

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

Çıktı verilerini kullanarak dosyaya yazılan `System.IO.StreamWriter`. Akış parametre kümesine `output.BaseStrea` parçası olarak `IUnstructuredWriter output`.

Dosyaya veri arabelleği sonra her bir satır yineleme temizlemek önemli olduğunu unutmayın. Ayrıca, `StreamWriter` nesne özniteliğiyle etkin atılabilir (varsayılan) ve birlikte kullanılmalıdır **kullanarak** anahtar sözcüğü:

```
using (StreamWriter streamWriter = new StreamWriter(output.BaseStream, this._encoding))
{
…
}
```

Aksi takdirde Flush() yöntemini her yinelemeden sonra açıkça çağırın. Bu aşağıdaki örnekte gösteriyoruz.

### <a name="set-headers-and-footers-for-user-defined-outputter"></a>Üstbilgiler ve altbilgiler kullanıcı tanımlı outputter için ayarlama
Üstbilgi ayarlamak için tek yineleme yürütme akışı kullanın.

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

İlk kodda `if (isHeaderRow)` blok yalnızca bir kez gerçekleştirilir.

Altbilgi için örneğine başvuru kullanın `System.IO.Stream` nesne (`output.BaseStream`). Altbilgi çağrısının yönteminde yazma `IOutputter` arabirimi.  (Daha fazla bilgi için aşağıdaki örneğe bakın.)

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
    public override void Close().
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

Ve U-SQL temel betiği:

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

Tablo verisi bir HTML dosyası oluşturur bir HTML outputter budur.

### <a name="call-outputter-from-u-sql-base-script"></a>U-SQL temel betikten outputter çağırın
Özel outputter temel U-SQL komut dosyasından çağırmak için outputter nesne yeni bir örneğini oluşturulması gerekir.

```sql
OUTPUT @rs0 TO @output_file USING new USQL_Programmability.HTMLOutputter(isHeader: true);
```

Nesnesinin bir örneği temel komut dosyasında oluşturmamak için size bir işlev sarmalayıcı bizim önceki örnekte gösterildiği gibi oluşturabilirsiniz:

```c#
        // Define the factory classes
        public static class Factory
        {
            public static HTMLOutputter HTMLOutputter(bool isHeader = false, Encoding encoding = null)
            {
                return new HTMLOutputter(isHeader, encoding);
            }
        }
```

Bu durumda, özgün çağrısı aşağıdaki gibi görünür:

```
OUTPUT @rs0 
TO @output_file 
USING USQL_Programmability.Factory.HTMLOutputter(isHeader: true);
```

## <a name="use-user-defined-processors"></a>Kullanıcı tanımlı işlemcilerini kullanıyor.
Kullanıcı tanımlı işlemci veya UDP, U-SQL programlama özelliklerine uygulayarak gelen satırların işlemenize olanak sağlayan UDO türüdür. UDP sütunu birleştirme, değerleri değiştirmek ve gerekirse, yeni sütun eklemek etkinleştirir. Temel olarak, gerekli veri öğeleri oluşturmak için bir satır kümesi işlem yardımcı olabilir.

Bir UDP tanımlamak için oluşturmamız gerekir bir `IProcessor` ile arabirim `SqlUserDefinedProcessor` için UDP isteğe bağlı öznitelik.

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

**SqlUserDefinedProcessor** türü kullanıcı tanımlı bir işlemci olarak kayıtlı olduğunu belirtir. Bu sınıf devralınan olamaz.

SqlUserDefinedProcessor özniteliği **isteğe bağlı** UDP tanımı.

Ana programlamasına nesneler **giriş** ve **çıkış**. Giriş nesnesi giriş sütunları ve çıkış numaralandırır ve çıktı verilerini işlemci etkinliğinin sonucu olarak ayarlamak için kullanılır.

Giriş sütunları numaralandırması için kullandığımız `input.Get` yöntemi.

```
string column_name = input.Get<string>("column_name");
```

Parametresi için `input.Get` yöntemdir parçası olarak geçirilen bir sütun `PRODUCE` yan tümcesi `PROCESS` U-SQL temel komut dosyası ifadesi. Burada doğru veri türünü kullanmak gerekir.

Çıkış için kullan `output.Set` yöntemi.

Bu özel üretici yalnızca çıkarır sütunlar ve ile tanımlanmış değerler dikkate almak önemlidir `output.Set` yöntem çağrısı.

```
output.Set<string>("mycolumn", mycolumn);
```

Gerçek işlemci çıktı çağırarak tetiklenir `return output.AsReadOnly();`.

Bir işlemci örneği aşağıda verilmiştir:

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

Bu kullanım örneği senaryosu, var olan sütunlar--bu durumda, "kullanıcı" için büyük harf ve "des" birleştirerek "full_description" adlı yeni bir sütun işlemci oluşturuyor. Ayrıca, bir GUID oluşturur ve özgün ve yeni GUID değerleri döndürür.

Önceki örnekte görebildiğiniz gibi C# sırasında yöntemi çağırabilirsiniz `output.Set` yöntem çağrısı.

Özel bir işlemci kullanan temel U-SQL komut dosyası örneği verilmiştir:

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
U-SQL kullanıcı tanımlı bir applier, özel bir C# işlev Sorguda dış tablo ifadesi tarafından döndürülen her satır için çağrılacak sağlar. Sağ giriş sol girdisinden her satır için hesaplanan ve üretilen satırları son çıktı için birleştirilir. Sol ve sağ giriş sütunları kümesi birleşimi UYGULA operatör tarafından üretilen sütun listesi var.

Kullanıcı tanımlı applier USQL seçin ifadenin bir parçası çağrılır.

Kullanıcı tanımlı applier tipik çağrısı aşağıdaki gibi görünür:

```
SELECT …
FROM …
CROSS APPLYis used to pass parameters
new MyScript.MyApplier(param1, param2) AS alias(output_param1 string, …);
```

Bir SELECT ifadesinde appliers kullanma hakkında daha fazla bilgi için bkz: [seçin, U-SQL seçme ARASI uygulamak ve OUTER APPLY](https://msdn.microsoft.com/library/azure/mt621307.aspx).

Kullanıcı tanımlı applier temel sınıf tanımı aşağıdaki gibidir:

```
public abstract class IApplier : IUserDefinedOperator
{
protected IApplier();

public abstract IEnumerable<IRow> Apply(IRow input, IUpdatableRow output);
}
```

Kullanıcı tanımlı bir applier tanımlamak için oluşturmamız gerekir `IApplier` ile Arabirimi [`SqlUserDefinedApplier`] özniteliği için kullanıcı tanımlı applier tanım isteğe bağlıdır.

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

* Uygulama dış tablonun her satırı için çağrılır. Döndürdüğü `IUpdatableRow` satır kümesi çıktı.
* Oluşturucu sınıfı için kullanıcı tanımlı applier parametreleri geçirmek için kullanılır.

**SqlUserDefinedApplier** türü kullanıcı tanımlı bir applier kayıtlı olduğunu belirtir. Bu sınıf devralınan olamaz.

**SqlUserDefinedApplier** olan **isteğe bağlı** kullanıcı tanımlı applier tanımı.


Ana programlamasına nesneleri aşağıdaki gibidir:

```
public override IEnumerable<IRow> Apply(IRow input, IUpdatableRow output)
```

Giriş satır kümeleri olarak geçirilir `IRow` giriş. Çıktı satırları olarak oluşturulan `IUpdatableRow` çıkış arabirimi.

Tek tek sütun adları çağırarak belirlenebilir `IRow` şema yöntemi.

```
ISchema schema = row.Schema;
var col = schema[i];
string val = row.Get<string>(col.Name)
```

Gelen gerçek veri değerleri almak için `IRow`, Get() yöntemi kullanırız `IRow` arabirimi.

```
mycolumn = row.Get<int>("mycolumn")
```

Veya şema sütun adı kullanırız:

```
row.Get<int>(row.Schema[0].Name)
```

Çıkış değerleri ile ayarlanmalıdır `IUpdatableRow` çıktı:

```
output.Set<int>("mycolumn", mycolumn)
```

Özel appliers sütunlar ve ile tanımlanmış değerler yalnızca çıktı anlamak önemlidir `output.Set` yöntem çağrısı.

Gerçek çıkış çağırarak tetiklenir `yield return output.AsReadOnly();`.

Kullanıcı tanımlı applier parametreleri oluşturucuya geçirilebilir. Applier temel U-SQL betiği applier çağrısında sırasında tanımlanması gerektiğini sütunlar değişken sayıda geri dönebilirsiniz.

```
new USQL_Programmability.ParserApplier ("all") AS properties(make string, model string, year string, type string, millage int);
```

Kullanıcı tanımlı applier örneği aşağıdadır:

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

Bu kullanıcı tarafından tanımlanan applier için temel U-SQL betiği aşağıdadır:

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

Bu kullanım örnek senaryoda, kullanıcı tanımlı applier araba yakıt özellikleri için virgülle ayrılmış değer ayrıştırıcı gibi davranır. Giriş dosyası satırları şuna benzer:

```
103 Z1AB2CD123XY45889   Ford,Explorer,2005,SUV,152345
303 Y0AB2CD34XY458890   Shevrolet,Cruise,2010,4Dr,32455
210 X5AB2CD45XY458893   Nissan,Altima,2011,4Dr,74000
```

Normal bir sekmeyle ayrılmış TSV dosya marka ve model gibi araba özellikleri içeren özellikleri sütunu var. Bu özellikler Ayrıştırılmış tablo sütunları için gerekir. Sağlanan applier geçirilen parametresine bağlı olarak sonuç kümesinde özellikleri sayısını dinamik oluşturmanıza olanak sağlar. Tüm özellikleri veya özellikler yalnızca belirli bir grup oluşturabilirsiniz.

    …USQL_Programmability.ParserApplier ("all")
    …USQL_Programmability.ParserApplier ("make")
    …USQL_Programmability.ParserApplier ("make&model")

Kullanıcı tanımlı applier applier nesnesinin yeni bir örneğini çağrılabilir:

```
CROSS APPLY new MyNameSpace.MyApplier (parameter: “value”) AS alias([columns types]…);
```

Veya bir sarmalayıcı fabrika yöntemi çağrıldı:

```c#
    CROSS APPLY MyNameSpace.MyApplier (parameter: “value”) AS alias([columns types]…);
```

## <a name="use-user-defined-combiners"></a>Kullanıcı tanımlı combiners kullanın
Kullanıcı tanımlı Birleştirici veya UDC, sol ve sağ satır kümeleri özel mantığına göre satırları birleştirmek sağlar. Kullanıcı tanımlı birleştirici birleştirme ifadesiyle kullanılır.

Bir Birleştirici hem giriş satır kümeleri, gruplandırma sütunlarında, beklenen sonucu şema ve ek bilgiler hakkında gerekli bilgileri sağlar birleştirme ifade ile çağrılan.

Temel bir U-SQL komut dosyasında bir Birleştirici çağırmak için size aşağıdaki sözdizimini kullanın:

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

Daha fazla bilgi için bkz: [BİRLEŞTİRMEK ifade (U-SQL)](https://msdn.microsoft.com/library/azure/mt621339.aspx).

Kullanıcı tanımlı bir Birleştirici tanımlamak için oluşturmamız gerekir `ICombiner` ile Arabirimi [`SqlUserDefinedCombiner`] özniteliği için bir kullanıcı tarafından tanımlanan birleştirici tanım isteğe bağlıdır.

Temel `ICombiner` sınıf tanımı:

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

Özel uygulanması bir `ICombiner` arabirim tanımı içermelidir bir `IEnumerable<IRow>` geçersiz kılma birleştirin.

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

**SqlUserDefinedCombiner** öznitelik türü kullanıcı tanımlı bir Birleştirici kayıtlı olduğunu gösterir. Bu sınıf devralınan olamaz.

**SqlUserDefinedCombiner** Birleştirici modu özelliği tanımlamak için kullanılır. Bir kullanıcı tarafından tanımlanan birleştirici tanımı için isteğe bağlı bir özniteliktir.

CombinerMode modu

CombinerMode enum, şu değerleri alabilir:

* Tam (0) her çıktı satır olası tüm giriş satırları sol ve sağ ile aynı anahtar değerine bağlıdır.

* Sol soldan (ve büyük olasılıkla tüm satırların aynı anahtar değeriyle sağdan) tek bir giriş satır her çıktı satır bağlıdır (1).

* Sağa (2) sağa (ve büyük olasılıkla tüm satırların aynı anahtar değeriyle soldan) tek bir giriş satır her çıktı satır bağlıdır.

* İç (3) tek bir giriş satır sol ve sağda aynı değere sahip her çıktı satır bağlıdır.

Örnek: [`SqlUserDefinedCombiner(Mode=CombinerMode.Left)`]


Ana programlamasına nesneler şunlardır:

```c#
    public override IEnumerable<IRow> Combine(IRowset left, IRowset right,
        IUpdatableRow output
```

Giriş satır kümeleri olarak geçirilir **sol** ve **sağ** `IRowset` arabirimi türü. Her iki satır kümeleri işleme için numaralandırılmış gerekir. Böylece biz numaralandırır ve gerekirse, önbelleğe zorunda kez, her bir arabirime yalnızca sıralayabilirsiniz.

Amacıyla önbelleğe alma işlemi için bir liste oluşturabilir\<T\> tür bellek yapısı sonuç olarak bir LINQ Sorgu yürütme, özellikle listesi <`IRow`>. Anonim veri türü, numaralandırma sırasında da kullanılabilir.

Bkz: [LINQ sorgularını (C#) giriş](https://msdn.microsoft.com/library/bb397906.aspx) LINQ sorguları hakkında daha fazla bilgi ve [IEnumerable\<T\> arabirimi](https://msdn.microsoft.com/library/9eekhta0(v=vs.110).aspx) IEnumerablehakkındadahafazlabilgiiçin\<T\> arabirimi.

Gelen gerçek veri değerleri almak için `IRowset`, Get() yöntemi kullanırız `IRow` arabirimi.

```
mycolumn = row.Get<int>("mycolumn")
```

Tek tek sütun adları çağırarak belirlenebilir `IRow` şema yöntemi.

```
ISchema schema = row.Schema;
var col = schema[i];
string val = row.Get<string>(col.Name)
```

Veya şema sütun adı kullanarak:

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

Her iki satır kümeleri numaralandırma sonra biz tüm satırları döngü alınacaktır. Sol satır kümesindeki her satır için biz bizim Birleştirici koşulu karşılıyor tüm satırları bulmak için adımıdır.

Çıkış değerleri ile ayarlanmalıdır `IUpdatableRow` çıktı.

```
output.Set<int>("mycolumn", mycolumn)
```

Arama için gerçek çıktı tetiklediği `yield return output.AsReadOnly();`.

Birleştirici örnek aşağıda verilmiştir:

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

Bu kullanım örneği senaryosu biz için satıcıya bir analiz raporu oluşturmakta olduğunuz. Birden fazla $20.000 maliyet ve, belirli bir zaman çerçevesi içinde normal perakende üzerinden daha hızlı Web sitesi aracılığıyla satmak tüm ürünleri bulmak için belirtilir.

Burada, temel U-SQL betiği verilmiştir. Normal bir birleştirme ve bir Birleştirici arasında mantığı karşılaştırabilirsiniz:

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

Kullanıcı tanımlı bir Birleştirici applier nesnesinin yeni bir örneğini çağrılabilir:

```
USING new MyNameSpace.MyCombiner();
```


Veya bir sarmalayıcı fabrika yöntemi çağrıldı:

```
USING MyNameSpace.MyCombiner();
```

## <a name="use-user-defined-reducers"></a>Kullanıcı tanımlı reducers kullanın

U-SQL özel satır kümesi reducers C# ile kullanıcı tanımlı işleci Genişletilebilirlik Çerçevesi'ni kullanıp IReducer arabirimi uygulama yazmanızı sağlar.

Kullanıcı tanımlı reducer veya UDR, gereksiz satırları (içe aktarma) veri ayıklama sırasında ortadan kaldırmak için kullanılabilir. Ayrıca, yönetmek ve satırları ve sütunları değerlendirmek için de kullanılabilir. Programlanabilirlik mantığına göre onu da hangi satırların ayıklanması gereken tanımlayabilirsiniz.

UDR sınıf tanımlamak için oluşturmamız gerekir bir `IReducer` isteğe bağlı bir arabirimiyle `SqlUserDefinedReducer` özniteliği.

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

**SqlUserDefinedReducer** öznitelik türü kullanıcı tanımlı bir reducer kayıtlı olduğunu gösterir. Bu sınıf devralınan olamaz.
**SqlUserDefinedReducer** kullanıcı tanımlı reducer tanımı için isteğe bağlı bir özniteliktir. IsRecursive özelliği tanımlamak için kullanılır.

* bool IsRecursive    
* **doğru** bu Reducer ilişkilendirilebilir ve yer değiştirebilme olup olmadığını gösterir =

Ana programlamasına nesneler **giriş** ve **çıkış**. Giriş nesnesi giriş satırları numaralandırmak için kullanılır. Çıkış, etkinlik azaltma sonucunda çıktı satırları ayarlamak için kullanılır.

Giriş satırları numaralandırması için kullandığımız `Row.Get` yöntemi.

```
foreach (IRow row in input.Rows)
{
    row.Get<string>("mycolumn");
}
```

Parametresi için `Row.Get` yöntemdir parçası olarak geçirilen bir sütun `PRODUCE` sınıfının `REDUCE` U-SQL temel komut dosyası ifadesi. Biz de doğru veri türünde burada kullanmanız gerekir.

Çıkış için kullan `output.Set` yöntemi.

Bu özel reducer yalnızca ile tanımlanmış değerleri çıkarır anlamak önemlidir `output.Set` yöntem çağrısı.

```
output.Set<string>("mycolumn", guid);
```

Gerçek reducer çıkış çağırarak tetiklenir `yield return output.AsReadOnly();`.

Reducer örnek aşağıda verilmiştir:

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

Bu kullanım örneği senaryosu reducer boş kullanıcı adı satırlarla atlıyor. Her satır kümesinde için gereken her sütun okur, ardından, kullanıcı adının uzunluğu değerlendirir. Kullanıcı adı değer uzunluğu 0'dan ise gerçek satır çıkarır.

Özel reducer kullanan temel U-SQL komut dosyası aşağıdadır:

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
