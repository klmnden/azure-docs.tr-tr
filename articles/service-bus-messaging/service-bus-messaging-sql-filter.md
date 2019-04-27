---
title: Azure Service Bus SQLFilter söz dizimi başvurusu | Microsoft Docs
description: SQLFilter dilbilgisi hakkında ayrıntılar.
services: service-bus-messaging
documentationcenter: na
author: spelluru
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/05/2018
ms.author: spelluru
ms.openlocfilehash: e490c7c24ed38e2988c1f097b09b508746f08178
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60591798"
---
# <a name="sqlfilter-syntax"></a>SQLFilter söz dizimi

A *SqlFilter* nesnedir örneği [SqlFilter sınıfı](/dotnet/api/microsoft.servicebus.messaging.sqlfilter), karşı değerlendirilir bir SQL dil temelli bir filtre ifadesi temsil eder bir [BrokeredMessage](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage). Bir SQL 92 standart kümesini destekler.  
  
 Bu konu, SqlFilter dilbilgisi ayrıntılarını listeler.  
  
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
  
-   `<scope>` kapsamını belirten isteğe bağlı bir dize `<property_name>`. Geçerli değerler `sys` veya `user`. `sys` Değeri gösterir sistem kapsamı burada `<property_name>` ortak özelliği adıdır [BrokeredMessage sınıfı](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage). `user` Kullanıcı kapsamını belirtir burada `<property_name>` bir anahtarı [BrokeredMessage sınıfı](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage) sözlüğü. `user` Kapsam ise varsayılan kapsam `<scope>` belirtilmedi.  
  
## <a name="remarks"></a>Açıklamalar

Mevcut olmayan kullanıcı özelliği erişme denemesi bir hata değildir ancak mevcut olmayan sistem özelliği erişme denemesi bir hata var. Bunun yerine, mevcut olmayan kullanıcı özelliği, bilinmeyen bir değer olarak dahili olarak değerlendirilir. Bilinmeyen bir değere işleci değerlendirmesi sırasında özel olarak kabul edilir.  
  
## <a name="propertyname"></a>property_name  
  
```  
<property_name> ::=  
     <identifier>  
     | <delimited_identifier>  
  
<identifier> ::=  
     <regular_identifier> | <quoted_identifier> | <delimited_identifier>  
  
```  
  
### <a name="arguments"></a>Bağımsız Değişkenler  

 `<regular_identifier>` bir dize, aşağıdaki normal ifade tarafından temsil edilir:  
  
```  
[[:IsLetter:]][_[:IsLetter:][:IsDigit:]]*  
```  
  
Bu dil bilgisi, bir harf ile başlayan ve bir veya daha fazla alt çizgi/harf/basamak tarafından izlenen herhangi bir dize anlamına gelir.  
  
`[:IsLetter:]` bir Unicode harf kategorilere ayrılır herhangi bir Unicode karakter anlamına gelir. `System.Char.IsLetter(c)` döndürür `true` varsa `c` Unicode harfidir.  
  
`[:IsDigit:]` bir ondalık basamak kategorilere ayrılır herhangi bir Unicode karakter anlamına gelir. `System.Char.IsDigit(c)` döndürür `true` varsa `c` bir Unicode basamak.  
  
A `<regular_identifier>` ayrılmış bir anahtar sözcük olamaz.  
  
`<delimited_identifier>` sol/sağ köşeli ayraç ([]) içine bir dizedir. Bir sağ köşeli ayraç iki sağ köşeli ayraç temsil edilir. Aşağıdaki örnekler `<delimited_identifier>`:  
  
```  
[Property With Space]  
[HR-EmployeeID]  
  
```  
  
`<quoted_identifier>` çift tırnak işareti ile herhangi bir karakter dizisidir. Çift tırnak işareti tanımlayıcı iki çift tırnak işareti temsil edilir. Kolayca bir dize sabitine ile çakışabilir tırnak işaretli tanımlayıcılar kullanmanız önerilmez. Mümkünse, sınırlandırılmış bir kimlik kullanın. Aşağıdaki örneğidir `<quoted_identifier>`:  
  
```  
"Contoso & Northwind"  
```  
  
## <a name="pattern"></a>Düzeni  
  
```  
<pattern> ::=  
      <expression>  
```  
  
### <a name="remarks"></a>Açıklamalar
  
`<pattern>` bir dize olarak değerlendirilen bir ifade olmalıdır. LIKE işleci için bir desen olarak kullanılır.      Bu joker karakterleri içerebilir:  
  
-   `%`:  Sıfır veya daha fazla karakter dizesi.  
  
-   `_`: Herhangi bir tek karakter.  
  
## <a name="escapechar"></a>escape_char  
  
```  
<escape_char> ::=  
      <expression>  
```  
  
### <a name="remarks"></a>Açıklamalar  

`<escape_char>` dize uzunluğu 1 olarak değerlendirilen bir ifade olmalıdır. LIKE işleci için bir kaçış karakteri kullanılır.  
  
 Örneğin, `property LIKE 'ABC\%' ESCAPE '\'` eşleşen `ABC%` ile başlayan bir dize yerine `ABC`.  
  
## <a name="constant"></a>Sabit  
  
```  
<constant> ::=  
      <integer_constant> | <decimal_constant> | <approximate_number_constant> | <boolean_constant> | NULL  
```  
  
### <a name="arguments"></a>Bağımsız Değişkenler  
  
-   `<integer_constant>` tırnak işaretleri arasına değil ve ondalık basamak içermeyen sayıdan oluşan bir dizedir. Değerleri olarak depolanır `System.Int64` dahili olarak, aynı aralık izleyin.  
  
     Uzun sabitleri örnekleri şunlardır:  
  
    ```  
    1894  
    2  
    ```  
  
-   `<decimal_constant>` sayıların tırnak işaretleri arasına değil ve ondalık nokta içeren bir dizedir. Değerleri olarak depolanır `System.Double` dahili olarak, aynı aralık/duyarlık izleyin.  
  
     Gelecekte yayımlanacak bir sürümde bu sayının tam sayı semantiği desteklemek için farklı veri türü depolanabilir, arka plandaki olgu üzerinde doğrulamamalısınız veri türü olduğundan `System.Double` için `<decimal_constant>`.  
  
     Ondalık sabitler örnekleri şunlardır:  
  
    ```  
    1894.1204  
    2.0  
    ```  
  
-   `<approximate_number_constant>` bir sayı ile yazılmış bilimsel gösterim olur. Değerleri olarak depolanır `System.Double` dahili olarak, aynı aralık/duyarlık izleyin. Yaklaşık sayı sabitleri örnekleri şunlardır:  
  
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

Boole sabit anahtar sözcüklere göre temsil edilir **TRUE** veya **FALSE**. Değerleri olarak depolanır `System.Boolean`.  
  
## <a name="stringconstant"></a>string_constant  
  
```  
<string_constant>  
```  
  
### <a name="remarks"></a>Açıklamalar  

Dize sabitleri tek tırnak işaretleri içine alınır ve geçerli Unicode karakterlerini içerir. İki tek tırnak işareti gibi bir dize sabiti katıştırılmış tek tırnak işareti temsil edilir.  
  
## <a name="function"></a>işlev  
  
```  
<function> :=  
      newid() |  
      property(name) | p(name)  
```  
  
### <a name="remarks"></a>Açıklamalar
  
`newid()` İşlevinin döndürdükleriyle bir **System.Guid** tarafından oluşturulan `System.Guid.NewGuid()` yöntemi.  
  
`property(name)` İşlevi tarafından başvurulan özelliğin değerini döndürür `name`. `name` Değeri bir dize değeri döndüren herhangi bir geçerli ifade olabilir.  
  
## <a name="considerations"></a>Dikkat edilmesi gerekenler
  
Aşağıdakileri göz önünde bulundurun [SqlFilter](/dotnet/api/microsoft.servicebus.messaging.sqlfilter) semantiği:  
  
-   Özellik adları büyük/küçük harfe duyarsızdır.  
  
-   İşleçler, C# örtük dönüştürme semantiği mümkün olduğunca izleyin.  
  
-   Sistem özellikleri de kullanıma sunulan ortak özellikler olan [BrokeredMessage](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage) örnekleri.  
  
    Aşağıdakileri göz önünde bulundurun `IS [NOT] NULL` semantiği:  
  
    -   `property IS NULL` değerlendirmesinde `true` özelliği yok ya da özelliğin değeri `null`.  
  
### <a name="property-evaluation-semantics"></a>Özellik değerlendirmesi semantiği  
  
- Mevcut olmayan sistem özelliği değerlendirilecek girişiminde oluşturur bir [FilterException](/dotnet/api/microsoft.servicebus.messaging.filterexception) özel durum.  
  
- Var olmayan bir özellik olarak dahili olarak değerlendirilir **bilinmeyen**.  
  
  Bilinmeyen değerlendirme aritmetik işleçler içinde:  
  
- İkili işleçleri sol ve sağ tarafında işlenen olarak değerlendirilir, **bilinmeyen**, sonuç ise **bilinmeyen**.  
  
- Bir işlenen olarak değerlendirildiği taktirde birli işleçler için **bilinmeyen**, sonuç ise **bilinmeyen**.  
  
  Bilinmeyen değerlendirme ikili Karşılaştırma işleçleri içinde:  
  
- Sol ve sağ tarafında işlenen olarak değerlendirilir, **bilinmeyen**, sonuç ise **bilinmeyen**.  
  
  Bilinmeyen değerlendirme `[NOT] LIKE`:  
  
- Herhangi bir işlenen olarak değerlendirildiği taktirde **bilinmeyen**, sonuç ise **bilinmeyen**.  
  
  Bilinmeyen değerlendirme `[NOT] IN`:  
  
- Sol işlenen olarak değerlendirildiği taktirde **bilinmeyen**, sonuç ise **bilinmeyen**.  
  
  Bilinmeyen değerlendirme **ve** işleci:  
  
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
  
 Bilinmeyen değerlendirme **veya** işleci:  
  
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
  
-   Karşılaştırma işleçleri gibi `>`, `>=`, `<`, `<=`, `!=`, ve `=` veri türü promosyonlar ve örtük dönüştürmelerin bağlama C# işleci ile aynı semantiğe izleyin.  
  
-   Aritmetik işleçler gibi `+`, `-`, `*`, `/`, ve `%` veri türü promosyonlar ve örtük dönüştürmelerin bağlama C# işleci ile aynı semantiğe izleyin.

## <a name="next-steps"></a>Sonraki adımlar

- [SQLFilter class (.NET Framework)](/dotnet/api/microsoft.servicebus.messaging.sqlfilter)
- [SQLFilter class (.NET Standard)](/dotnet/api/microsoft.azure.servicebus.sqlfilter)
- [SQLRuleAction sınıfı](/dotnet/api/microsoft.servicebus.messaging.sqlruleaction)
