---
title: Azure hizmet veri yolu SQLFilter söz dizimi başvurusu | Microsoft Docs
description: SQLFilter dilbilgisi hakkında ayrıntılar.
services: service-bus-messaging
documentationcenter: na
author: sethmanheim
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/05/2018
ms.author: sethm
ms.openlocfilehash: ec9d728eb31eb979e82bfb53cf619f823750e65c
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/09/2018
ms.locfileid: "29132176"
---
# <a name="sqlfilter-syntax"></a>SQLFilter sözdizimi

A *SqlFilter* nesnesidir örneği [SqlFilter sınıfı](/dotnet/api/microsoft.servicebus.messaging.sqlfilter)ve karşı hesaplanan bir SQL dil temelli bir filtre ifadesi temsil eden bir [BrokeredMessage](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage). Bir SqlFilter SQL 92 standart kümesini destekler.  
  
 Bu konu SqlFilter dilbilgisi ayrıntılarını listeler.  
  
```  
<predicate ::=  
      { NOT <predicate> }  
      | <predicate> AND <predicate>  
      | <predicate> OR <predicate>  
      | <expression> { = | <> | != | > | >= | < | <= } <expression>  
      | <property> IS [NOT] NULL  
      | <expression> [NOT] IN ( <expression> [, ...n] )  
      | <expression> [NOT] LIKE <pattern> [ESCAPE <escape_char>]  
      | EXISTS ( <property> )  
      | ( <predicate> )  
  
```  
  
```  
<expression> ::=  
      <constant>   
      | <function>  
      | <property>  
      | <expression> { + | - | * | / | % } <expression>  
      | { + | - } <expression>  
      | ( <expression> )  
  
```  
  
```  
<property> :=   
       [<scope> .] <property_name>  
  
```  
  
## <a name="arguments"></a>Bağımsız Değişkenler  
  
-   `<scope>`kapsamını belirten isteğe bağlı bir dize `<property_name>`. Geçerli değerler `sys` veya `user`. `sys` Değeri gösterir sistemi kapsamı nerede `<property_name>` bir ortak özellik adı [BrokeredMessage sınıfı](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage). `user`Kullanıcı kapsam gösterir nerede `<property_name>` , bir anahtar [BrokeredMessage sınıfı](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage) sözlük. `user`Kapsam ise varsayılan kapsamı `<scope>` belirtilmedi.  
  
## <a name="remarks"></a>Açıklamalar

Mevcut olmayan kullanıcı özelliği erişme denemesi bir hata olduğundan mevcut olmayan sistem özelliği erişme denemesi bir hata var. Bunun yerine, mevcut olmayan kullanıcı özelliği bilinmeyen bir değere dahili olarak değerlendirilir. Bilinmeyen bir değere özel işleci değerlendirme sırasında kabul edilir.  
  
## <a name="propertyname"></a>property_name  
  
```  
<property_name> ::=  
     <identifier>  
     | <delimited_identifier>  
  
<identifier> ::=  
     <regular_identifier> | <quoted_identifier> | <delimited_identifier>  
  
```  
  
### <a name="arguments"></a>Bağımsız Değişkenler  

 `<regular_identifier>`bir dize aşağıdaki normal ifade tarafından temsil edilen:  
  
```  
[[:IsLetter:]][_[:IsLetter:][:IsDigit:]]*  
```  
  
Bu dilbilgisi bir harf ile başlayıp bir veya daha fazla alt çizgi/harf/basamaklı tarafından izlenen herhangi bir dize anlamına gelir.  
  
`[:IsLetter:]`bir Unicode harf kategorilere herhangi bir Unicode karakter anlamına gelir. `System.Char.IsLetter(c)`döndürür `true` varsa `c` bir Unicode harf.  
  
`[:IsDigit:]`ondalık bir sayı kategorilere herhangi bir Unicode karakter anlamına gelir. `System.Char.IsDigit(c)`döndürür `true` varsa `c` Unicode sayıdır.  
  
A `<regular_identifier>` ayrılmış bir anahtar sözcük olamaz.  
  
`<delimited_identifier>`sol/sağ köşeli ayraç ([]) içine herhangi bir dize değil. Sağ köşeli ayraç iki sağ köşeli temsil edilir. Örnekleri aşağıda verilmiştir `<delimited_identifier>`:  
  
```  
[Property With Space]  
[HR-EmployeeID]  
  
```  
  
`<quoted_identifier>`ile çift tırnak işaretleri arasına herhangi bir dize değil. Çift tırnak işareti tanımlayıcıda iki çift tırnak işareti temsil edilir. Bir dize sabiti ile kolayca çakışabilir çünkü tırnak işaretli tanımlayıcılar kullanmak için önerilmez. Sınırlandırılmış bir kimlik mümkünse kullanın. Aşağıdaki örneğidir `<quoted_identifier>`:  
  
```  
"Contoso & Northwind"  
```  
  
## <a name="pattern"></a>düzeni  
  
```  
<pattern> ::=  
      <expression>  
```  
  
### <a name="remarks"></a>Açıklamalar
  
`<pattern>`bir dize olarak değerlendirilen bir ifade olmalıdır. LIKE işleci için bir desen olarak kullanılır.      Aşağıdaki joker karakterleri içerebilir:  
  
-   `%`: Herhangi bir dize sıfır veya daha fazla karakter.  
  
-   `_`: Herhangi bir tek karakteri.  
  
## <a name="escapechar"></a>escape_char  
  
```  
<escape_char> ::=  
      <expression>  
```  
  
### <a name="remarks"></a>Açıklamalar  

`<escape_char>`dize uzunluğu 1 olarak değerlendirilen bir ifade olmalıdır. LIKE işleci için bir kaçış karakteri olarak kullanılır.  
  
 Örneğin, `property LIKE 'ABC\%' ESCAPE '\'` eşleşen `ABC%` ile başlayan bir dize yerine `ABC`.  
  
## <a name="constant"></a>sabiti  
  
```  
<constant> ::=  
      <integer_constant> | <decimal_constant> | <approximate_number_constant> | <boolean_constant> | NULL  
```  
  
### <a name="arguments"></a>Bağımsız Değişkenler  
  
-   `<integer_constant>`yalnızca tırnak işaretleri içine değil ve ondalık basamak içeren değil sayı dizesidir. Değerleri olarak depolanan `System.Int64` dahili olarak, aynı aralık izleyin.  
  
     Bu, uzun sabitleri örnekleri şunlardır:  
  
    ```  
    1894  
    2  
    ```  
  
-   `<decimal_constant>`yalnızca tırnak işaretleri içine değil ve ondalık içeren sayı dizesidir. Değerleri olarak depolanan `System.Double` dahili olarak, aynı aralık/duyarlık izleyin.  
  
     Sonraki bir sürümde tam sayı semantiğini desteklemek için farklı bir veri türü bu sayı depolanabilir, arka plandaki olgu üzerinde doğrulamamalısınız veri türü olduğundan `System.Double` için `<decimal_constant>`.  
  
     Ondalık sabitleri örnekleri verilmiştir:  
  
    ```  
    1894.1204  
    2.0  
    ```  
  
-   `<approximate_number_constant>`bir sayı yazılmış bilimsel gösterim şeklindedir. Değerleri olarak depolanan `System.Double` dahili olarak, aynı aralık/duyarlık izleyin. Yaklaşık sayı sabitleri örnekleri verilmiştir:  
  
    ```  
    101.5E5  
    0.5E-2  
    ```  
  
## <a name="booleanconstant"></a>boolean_constant  
  
```  
<boolean_constant> :=  
      TRUE | FALSE  
```  
  
### <a name="remarks"></a>Açıklamalar  

Boole sabitleri anahtar sözcükleri tarafından temsil edilen **TRUE** veya **FALSE**. Değerleri olarak depolanan `System.Boolean`.  
  
## <a name="stringconstant"></a>string_constant  
  
```  
<string_constant>  
```  
  
### <a name="remarks"></a>Açıklamalar  

Dize sabitleri tek tırnak işaretleri içine ve geçerli Unicode karakterler içerir. Bir dize sabitine katıştırılmış tek tırnak işareti, iki tırnak işaretleri tek olarak temsil edilir.  
  
## <a name="function"></a>işlev  
  
```  
<function> :=  
      newid() |  
      property(name) | p(name)  
```  
  
### <a name="remarks"></a>Açıklamalar
  
`newid()` İşlev döndürür bir **System.Guid** tarafından oluşturulan `System.Guid.NewGuid()` yöntemi.  
  
`property(name)` İşlevi tarafından başvurulan özelliğinin değerini döndürür `name`. `name` Değeri bir string değeri döndürür geçerli bir ifade olabilir.  
  
## <a name="considerations"></a>Dikkat edilmesi gerekenler
  
Aşağıdakileri göz önünde bulundurun [SqlFilter](/dotnet/api/microsoft.servicebus.messaging.sqlfilter) semantiği:  
  
-   Özellik adları büyük/küçük harfe duyarsızdır.  
  
-   C# örtük dönüşüm semantiği mümkün olduğunca işleçleri izleyin.  
  
-   Sistem özellikleri olan ortak özellikler, gösterilen [BrokeredMessage](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage) örnekleri.  
  
    Aşağıdakileri göz önünde bulundurun `IS [NOT] NULL` semantiği:  
  
    -   `property IS NULL`olarak değerlendirilir `true` özelliği yok veya özelliğin değeri, `null`.  
  
### <a name="property-evaluation-semantics"></a>Özellik değerlendirme semantiği  
  
-   Mevcut olmayan sistem özelliği değerlendirmek için girişiminde oluşturur bir [FilterException](/dotnet/api/microsoft.servicebus.messaging.filterexception) özel durum.  
  
-   Var olmayan bir özellik olarak dahili olarak değerlendirilir **bilinmeyen**.  
  
 Aritmetik işleçler bilinmeyen hesaplanmasında:  
  
-   İkili işleçler varsa sol ve sağ tarafındaki işlenenleri olarak değerlendirildiği için **bilinmeyen**, sonuç sonra **bilinmeyen**.  
  
-   Birli işleçleri işleneni olarak değerlendirildiği taktirde **bilinmeyen**, sonuç sonra **bilinmeyen**.  
  
 İkili Karşılaştırma işleçleri bilinmeyen hesaplanmasında:  
  
-   Varsa sol ve sağ tarafındaki işlenenleri olarak değerlendirildiği **bilinmeyen**, sonuç sonra **bilinmeyen**.  
  
 Bilinmeyen hesaplanmasında `[NOT] LIKE`:  
  
-   Tüm işleneni olarak değerlendirildiği taktirde **bilinmeyen**, sonuç sonra **bilinmeyen**.  
  
 Bilinmeyen hesaplanmasında `[NOT] IN`:  
  
-   Sol işleneni olarak değerlendirildiği taktirde **bilinmeyen**, sonuç sonra **bilinmeyen**.  
  
 Bilinmeyen hesaplanmasında **ve** işleci:  
  
```  
+---+---+---+---+  
|AND| T | F | U |  
+---+---+---+---+  
| T | T | F | U |  
+---+---+---+---+  
| F | F | F | F |  
+---+---+---+---+  
| U | U | F | U |  
+---+---+---+---+  
```  
  
 Bilinmeyen hesaplanmasında **veya** işleci:  
  
```  
+---+---+---+---+  
|OR | T | F | U |  
+---+---+---+---+  
| T | T | T | T |  
+---+---+---+---+  
| F | T | F | U |  
+---+---+---+---+  
| U | T | U | U |  
+---+---+---+---+  
```  
  
### <a name="operator-binding-semantics"></a>İşleç bağlama semantiği
  
-   Karşılaştırma işleçleri gibi `>`, `>=`, `<`, `<=`, `!=`, ve `=` veri türü promosyonlar ve örtük dönüşümler bağlama C# işleci aynı topluca izleyin.  
  
-   Aritmetik işleçler gibi `+`, `-`, `*`, `/`, ve `%` veri türü promosyonlar ve örtük dönüşümler bağlama C# işleci aynı topluca izleyin.

## <a name="next-steps"></a>Sonraki adımlar

- [SQLFilter sınıfı (.NET Framework)](/dotnet/api/microsoft.servicebus.messaging.sqlfilter)
- [SQLFilter sınıfı (.NET standart)](/dotnet/api/microsoft.azure.servicebus.filters.sqlfilter)
- [SQLRuleAction sınıfı](/dotnet/api/microsoft.servicebus.messaging.sqlruleaction)