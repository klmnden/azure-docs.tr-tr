---
title: Azure SQL veritabanı özellik kısıtlamaları | Microsoft Docs
description: Azure SQL veritabanı özellik kısıtlamaları içlerindeki bilgileri erişim kazanmak için saldırganlar tarafından olabilen veritabanı özelliklerini kısıtlayarak, veritabanı güvenliği artırır.
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: vainolo
ms.author: arib
ms.reviewer: vanto
manager: craigg
ms.date: 03/22/2019
ms.openlocfilehash: ac7a074e78def504a10b4daa07971f919f414a88
ms.sourcegitcommit: 25a60179840b30706429c397991157f27de9e886
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/28/2019
ms.locfileid: "66259460"
---
# <a name="azure-sql-database-feature-restrictions"></a>Azure SQL veritabanı özellik kısıtlamaları

Bir ortak SQL Server saldırıları veritabanına erişen web uygulamaları bı'yi veritabanı hakkında bilgi için SQL ekleme saldırılarına karşı çeşitli biçimlerdeki kullanıldığı kaynağıdır.  İdeal olarak, SQL ekleme işlemi için izin vermeyecek şekilde uygulama kodu geliştirilmiştir.  Bu nedenle SQL eklemelerini bir olgu karşı korumak için sahip olduğumuz yaşam ancak, büyük kod-içeren eski ve dış kod tabanlarında, bir hiçbir zaman tüm durumlarda giderilmiş olduğunu, emin olabilirsiniz.  Özellik kısıtlamaları amacı, SQL ekleme, SQL ekleme başarılı olsa bile veritabanıyla ilgili bilgi sızıntısı gelen bazı formları engellemektir.

## <a name="enabling-feature-restrictions"></a>Özellik kısıtlamaları etkinleştirme

Özellik kısıtlamaları etkinleştirme yapılır kullanarak `sp_add_feature_restriction` saklı yordamı şu şekilde:

```sql
EXEC sp_add_feature_restriction <feature>, <object_class>, <object_name>
```

Aşağıdaki özellikler kısıtlanmış olabilir:

| Özellik          | Açıklama |
|------------------|-------------|
| N'ErrorMessages | Kısıtlı olduğunda, hata iletisi içindeki tüm kullanıcı verilerini maskelenir. Bkz: [hata iletileri, kısıtlama özelliği](#error-messages-feature-restriction) |
| N'Waitfor'       | Kısıtlı olduğunda komut herhangi bir gecikme olmadan hemen döndürür. Bkz: [WAITFOR özellik kısıtlama](#waitfor-feature-restriction) |

Değerini `object_class` olabilir `N'User'` veya `N'Role'` belirtmek için olup olmadığını `object_name` bir kullanıcı adı veya bir rolü adı veritabanında.

Aşağıdaki örnek, kullanıcı için tüm hata iletilerini açacak `MyUser` gizlenmeye:

```sql
EXEC sp_add_feature_restriction N'ErrorMessages', N'User', N'MyUser'
```

## <a name="disabling-feature-restrictions"></a>Özellik kısıtlamaları devre dışı bırakma

Özellik kısıtlamaları devre dışı bırakma yapılır kullanarak `sp_drop_feature_restriction` saklı yordamı şu şekilde:

```sql
EXEC sp_drop_feature_restriction <feature>, <object_class>, <object_name>
```

Aşağıdaki örnek, kullanıcı için hata iletisi maskeleme devre dışı bırakır `MyUser`:

```sql
EXEC sp_drop_feature_restriction N'ErrorMessages', N'User', N'MyUser'
```

## <a name="viewing-feature-restrictions"></a>Görüntüleme özellik kısıtlamaları

`sys.sql_feature_restrictions` Görünümü, veritabanında tüm şu anda tanımlı bir özellik kısıtlamaları gösterir. Şu sütunları içerir:

| Sütun adı | Veri türü | Açıklama |
|-------------|-----------|-------------|
| sınıf       | nvarchar(128) | Kısıtlamanın uygulandığı nesne sınıfı |
| object      | nvarchar(256) | Kısıtlamanın uygulandığı nesnesinin adı |
| Özelliği     | nvarchar(128) | Kısıtlı özelliği |

## <a name="feature-restrictions"></a>Özellik kısıtlamaları

### <a name="error-messages-feature-restriction"></a>Hata iletileri özellik kısıtlama

Bir ortak SQL ekleme saldırısına yöntemi hataya neden olan kod bulunmaktır.  Hata iletisi inceleyerek, bir saldırganın ek daha hedefe saldırılar yaşanabileceğini gösterir sistem hakkında bilgi öğrenebilirsiniz.  Bu saldırı uygulama bir sorgunun sonuçlarını görüntülemez ancak hata iletilerini görüntülemek özellikle yararlı olabilir.

Bir istek biçiminde olan bir web uygulamasını göz önünde bulundurun:

```html
http://www.contoso.com/employee.php?id=1
```

Aşağıdaki veritabanı sorgusu yürütür:

```sql
SELECT Name FROM EMPLOYEES WHERE Id=$EmpId
```

Değer olarak aktarılırsa `id` web uygulaması istek parametresi veritabanı sorgusunda $EmpId değiştirilecek kopyalanır, bir saldırganın aşağıdaki isteği yapabilir:

```html
http://www.contoso.com/employee.php?id=1 AND CAST(DB_NAME() AS INT)=0
```

Ve şu hata, veritabanı adını öğrenmek, saldırganın döndürülür:

```sql
Conversion failed when converting the nvarchar value 'HR_Data' to data type int.
```

Kısıtlama veritabanında uygulama kullanıcı için etkinleştirme hata iletileri özellik sonra böylece hiçbir iç veritabanı bilgilerini sızmış döndürülen hata iletisi maskelenir:

```sql
Conversion failed when converting the ****** value '******' to data type ******.
```

Benzer şekilde, saldırgan aşağıdaki isteği yapabilirsiniz:

```html
http://www.contoso.com/employee.php?id=1 AND CAST(Salary AS TINYINT)=0
```

Ve şu hata, çalışanın maaş öğrenmek, saldırganın döndürülür:

```sql
Arithmetic overflow error for data type tinyint, value = 140000.
```

Hata iletileri özellik kısıtlama kullanarak, veritabanı döndürür:

```sql
Arithmetic overflow error for data type ******, value = ******.
```

### <a name="waitfor-feature-restriction"></a>WAITFOR özellik kısıtlama

Bir uygulamanın eklenen SQL sonuçlarını veya hata iletisi ile bir saldırganın sağlar değil, ancak saldırgan, koşullu bir sorgu oluşturarak veritabanından bilgi çıkarabilir, bir eposta SQL ekleme, iki koşullu dalları yürütmek için farklı bir süre alır. Yanıt süresi karşılaştırarak, hangi dalın yürütülmesi ve böylece sistem hakkında bilgi edinin saldırgan bilebilirsiniz. Bu saldırı basit değişken kullanarak `WAITFOR` gecikme tanıtmak için deyimi.

Bir istek biçiminde olan bir web uygulamasını göz önünde bulundurun:

```html
http://www.contoso.com/employee.php?id=1
```

Aşağıdaki veritabanı sorgusu yürütür:

```sql
SELECT Name FROM EMPLOYEES WHERE Id=$EmpId
```

Web uygulama isteklerini kimliği parametre olarak geçirilen değer $EmpId veritabanı sorgusu, değiştirilecek kopyalanırsa, bir saldırganın aşağıdaki isteği yapabilir:

```html
http://www.contoso.com/employee.php?id=1; IF SYSTEM_USER='sa' WAITFOR DELAY '00:00:05'
```

Ve sorgu varsa ek bir 5 saniye götürecek `sa` hesabı kullanılan. Varsa `WAITFOR` özellik kısıtlama devre dışı veritabanında `WAITFOR` ifadesi yoksayılacak ve bilgileri değil sızmasına Bu saldırının kullanarak.