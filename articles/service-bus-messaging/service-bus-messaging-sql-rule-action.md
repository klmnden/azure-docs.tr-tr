---
title: "Azure'da SQLRuleAction söz dizimi başvurusu | Microsoft Docs"
description: "SQLRuleAction dilbilgisi hakkında ayrıntılar."
services: service-bus-messaging
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/28/2017
ms.author: sethm
ms.openlocfilehash: 7379b7f58563675f28d77928d933c0d9c7992e71
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="sqlruleaction-syntax"></a>SQLRuleAction sözdizimi

A *SqlRuleAction* örneği [SqlRuleAction](/dotnet/api/microsoft.servicebus.messaging.sqlruleaction) sınıfı ve SQL dilinde yazılmış eylemleri temsil kümesi göre karşı gerçekleştirilen sözdizimi bir [BrokeredMessage](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage).   
  
Bu konu, SQL kural eylemi dilbilgisi ayrıntılarını listeler.  
  
```  
<statements> ::=
    <statement> [, ...n]  
  
```  
  
```  
<statement> ::=
    <action> [;]
    Remarks
    -------
    Semicolon is optional.  
  
```  
  
```  
<action> ::=
    SET <property> = <expression>
    REMOVE <property>  
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
  
### <a name="remarks"></a>Açıklamalar  

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
  
 Bu, bir harf ile başlayıp bir veya daha fazla alt çizgi/harf/basamaklı tarafından izlenen herhangi bir dize anlamına gelir.  
  
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
  
     Uzun sabitleri örnekleri verilmiştir:  
  
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
  
Boole sabitleri anahtar sözcükleri tarafından temsil edilen `TRUE` veya `FALSE`. Değerleri olarak depolanan `System.Boolean`.  
  
## <a name="stringconstant"></a>string_constant  
  
```  
<string_constant>  
```  
  
### <a name="remarks"></a>Açıklamalar
  
Dize sabitleri tek tırnak işaretleri içine ve geçerli Unicode karakterler içerir. Bir dize sabitine katıştırılmış tek tırnak işareti, iki tırnak işaretleri tek olarak temsil edilir.  
  
## <a name="function"></a>İşlevi  
  
```  
<function> :=  
      newid() |  
      property(name) | p(name)  
```  
  
### <a name="remarks"></a>Açıklamalar  

`newid()` İşlev döndürür bir **System.Guid** tarafından oluşturulan `System.Guid.NewGuid()` yöntemi.  
  
`property(name)` İşlevi tarafından başvurulan özelliğinin değerini döndürür `name`. `name` Değeri bir string değeri döndürür geçerli bir ifade olabilir.  
  
## <a name="considerations"></a>Dikkat edilmesi gerekenler

- Küme, yeni bir özellik oluşturmak veya mevcut bir özellik değerini güncelleştirmek için kullanılır.
- Kaldır, bir özelliği kaldırmak için kullanılır.
- İfade türü ve var olan özellik türü farklı olduğunda örtük dönüşüm mümkünse gerçekleştirir.
- Mevcut olmayan Sistem özellikleri başvurulan eylem başarısız olur.
- Mevcut olmayan kullanıcı özelliklerini başvurulan, eylem başarısız olmaz.
- Mevcut olmayan kullanıcı özelliği "Bilinmiyor" olarak dahili olarak, aynı topluca aşağıdaki değerlendirilir [SQLFilter](/dotnet/api/microsoft.servicebus.messaging.sqlfilter) işleçleri değerlendirirken.

## <a name="next-steps"></a>Sonraki adımlar

- [SQLRuleAction sınıfı](/dotnet/api/microsoft.servicebus.messaging.sqlruleaction)
- [SQLFilter sınıfı](/dotnet/api/microsoft.servicebus.messaging.sqlfilter)
