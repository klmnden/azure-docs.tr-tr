---
title: Azure CDN kural altyapısı başvurusu | Microsoft Docs
description: Azure CDN için başvuru belgeleri altyapısı eşleştirme koşulları ve özellikleri kuralları.
services: cdn
author: mdgattuso
ms.service: azure-cdn
ms.topic: article
ms.date: 05/31/2019
ms.author: magattus
ms.openlocfilehash: 5fc611af75a7f733576f9343a4375fb56cacc030
ms.sourcegitcommit: ccb9a7b7da48473362266f20950af190ae88c09b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2019
ms.locfileid: "67593158"
---
# <a name="azure-cdn-from-verizon-premium-rules-engine-reference"></a>Azure CDN Verizon Premium kural altyapısı başvurusu

Bu makalede ayrıntılı açıklamaları için Azure Content Delivery Network (CDN) kullanılabilir eşleştirme koşulları ve özellikleri listelenmektedir [kurallar altyapısı](cdn-verizon-premium-rules-engine.md).

Kural altyapısı, belirli türde istekler son yetkilisinde CDN tarafından işlenen olacak şekilde tasarlanmıştır.

**Yaygın kullanımları**:

- Geçersiz kılabilir veya özel önbellek ilkesini tanımlayın.
- Güvenli veya hassas içerik isteklerini reddedin.
- Yeniden yönlendirme istekleri.
- Özel günlük verilerini Store.

## <a name="terminology"></a>Terminoloji

Bir kural tarafından tanımlanan [ **koşullu ifadeler**](cdn-verizon-premium-rules-engine-reference-conditional-expressions.md), [ **koşullara uyan**](cdn-verizon-premium-rules-engine-reference-match-conditions.md), ve [ **özellikleri**](cdn-verizon-premium-rules-engine-reference-features.md). Bu öğeleri aşağıdaki şekilde vurgulanmıştır:

 ![CDN eşleşme koşulu](./media/cdn-rules-engine-reference/cdn-rules-engine-terminology.png)

## <a name="syntax"></a>Sözdizimi

İçinde özel karakterler kabul edilir bir şekilde nasıl bir eşleşme koşulu veya özellik metin değerlerini işler göre değişir. Bir eşleşme koşulu veya özellik metin aşağıdaki yollardan biriyle yorumlayabilir:

1. [**Değişmez değerler**](#literal-values)
2. [**Değerleri joker karakter**](#wildcard-values)
3. [**Normal ifadeler**](#regular-expressions)

### <a name="literal-values"></a>Değişmez değerler

Değişmez değer yorumlanır metin % sembol hariç tüm özel karakterler eşlenmesi gereken değer bir parçası olarak değerlendirir. Diğer bir deyişle, koşul kümesi eşleşen bir sabit değer `\'*'\` değer, tam zaman yalnızca uyulmuş olur (diğer bir deyişle, `\'*'\`) bulunur.

URL kodlaması belirtmek için kullanılan bir yüzde sembolü (örneğin, `%20`).

### <a name="wildcard-values"></a>Değerleri joker karakter

Joker karakter değer olarak yorumlanır metin için özel karakterler ek anlamlar atar. Aşağıdaki tabloda, şu karakter kümesini nasıl yorumlanacağını açıklanmaktadır:

Karakter | Açıklama
----------|------------
\ | Bu tabloda belirtilen karakterlerden herhangi birini kaçış ters eğik çizgi kullanılır. Ters eğik çizgi kaçış doğrudan özel karakter önce belirtilmelidir.<br/>Örneğin, aşağıdaki söz dizimini yıldız çıkışları: `\*`
% | URL kodlaması belirtmek için kullanılan bir yüzde sembolü (örneğin, `%20`).
\* | Bir yıldız işareti, bir veya daha fazla karakterleri temsil eden bir joker karakterdir.
Uzay | Bir boşluk karakteri bir eşleşme koşulu ya da belirtilen değerleri veya desenleri tarafından karşılanması olduğunu gösterir.
'value' | Tek tırnak, özel bir anlamı yok. Ancak, tek tırnak bir dizi, değer değişmez değer değerlendirilmesi gerektiğini belirtmek için kullanılır. Aşağıdaki şekillerde kullanılabilir:<br><br/>-Bir eşleşme koşulu herhangi bir bölümünü karşılaştırma değeri belirtilen değerle eşleşen her memnun olmasını sağlar.  Örneğin, `'ma'` aşağıdaki dizelerden birini eşleşir: <br/><br/>/business/**ma**rathon/asset.htm<br/>**ma**p.gif<br/>/ İş/şablonu. **ma**p<br /><br />-Bu, bir değişmez değer olarak belirtilmesi için özel bir karakter sağlar. Örneğin, bir boşluk karakteri tek tırnak bir dizi kapsayan tarafından bir hazır bilgi boşluk karakteri belirtmek (diğer bir deyişle, `' '` veya `'sample value'`).<br/>-Boş bir değer belirtilmesini sağlar. Tek tırnak bir dizi belirterek boş bir değer belirtin (diğer bir deyişle, '').<br /><br/>**Önemli:**<br/>-Ardından belirtilen değer bir joker karakter içermiyorsa, tek tırnak kümesi belirtmek gerekli değildir, yani bir değişmez değer otomatik olarak değerlendirilir.<br/>-Bir ters eğik çizgi, bu tablodaki başka bir karakter kaçırmaz ise tek tırnak bir dizi içinde belirtildiğinde yoksayılır.<br/>-Değişmez bir karakter kaçış ters eğik çizgi kullanarak olarak özel bir karakter belirtmek için başka bir yol (diğer bir deyişle, `\`).

### <a name="regular-expressions"></a>Normal ifadeler

Normal ifadeler için bir metin değeri içinde aranır bir desen tanımlayın. Normal ifade gösterimi belirli anlamlara çeşitli sembolleri tanımlar. Aşağıdaki tabloda, nasıl özel karakterler gösteren eşleştirme koşulları ve normal ifadeler destekleyen özellikler tarafından kabul edilir.

Özel karakter | Açıklama
------------------|------------
\ | Ters eğik çizgi karakteri aşağıdaki çıkışları BT, normal ifade anlamını almak yerine değişmez değer olarak değerlendirilmesi bu karakteri neden olur. Örneğin, aşağıdaki söz dizimini yıldız çıkışları: `\*`
% | The meaning of a percentage symbol depends on its usage.<br/><br/> `%{HTTPVariable}`: This syntax identifies an HTTP variable.<br/>`%{HTTPVariable%Pattern}`: This syntax uses a percentage symbol to identify an HTTP variable and as a delimiter.<br />`\%`: Escaping a percentage symbol allows it to be used as a literal value or to indicate URL encoding (for example, `\%20`).
\* | An asterisk allows the preceding character to be matched zero or more times.
Space | A space character is typically treated as a literal character.
'value' | Single quotes are treated as literal characters. A set of single quotes does not have special meaning.

## Next steps

- [Rules engine match conditions](cdn-verizon-premium-rules-engine-reference-match-conditions.md)
- [Rules engine conditional expressions](cdn-verizon-premium-rules-engine-reference-conditional-expressions.md)
- [Rules engine features](cdn-verizon-premium-rules-engine-reference-features.md)
- [Override HTTP behavior using the rules engine](cdn-verizon-premium-rules-engine.md)
- [Azure CDN overview](cdn-overview.mdBir yüzde sembolü anlamını kendi kullanıma bağlıdır. Docs
descr`%{HTTPVariable}`: Bu sözdizimi, bir HTTP değişkenini tanımlar.match`%{HTTPVariable%Pattern}`: Bu söz dizimi bir yüzde sembolü HTTP değişken ve ayırıcı olarak tanımlamak için kullanır.1/2019`\``: Escaping a percentage symbol allows it to be used as a literal value or to indicate URL encoding (for example, `\`20`).f the available match conditions and features for the Azure Content Delivery Network (CDN) [rules engine](cdn-verizon-premium-rules-engine.md).

The rules engine is designed to be the final authority on how specific types of requests are processed by the CDN.

**Common uses**:

- Override or define a custom cache policy.
- Secure or deny requests for sensitive content.
- Redirect requests.
- Store custom log data.

## Terminology

A rule is defined through the use of [**conditional expressions**](cdn-verizon-premium-rules-engine-reference-conditional-expressions.md), [**match conditions**](cdn-verizon-premium-rules-engine-reference-match-conditions.md), and [**features**](cdn-verizon-premium-rules-engine-reference-features.md). These elements are highlighted in the following illustration:

 ![CDN match condition](./media/cdn-rules-engine-reference/cdn-rules-engine-terminology.png)

## Syntax

The manner in which special characters are treated varies according to how a match condition or feature handles text values. A match condition or feature may interpret text in one of the following ways:

1. [**Literal values**](#literal-values)
2. [**Wildcard values**](#wildcard-values)
3. [**Regular expressions**](#regular-expressions)

### Literal values

Text that is interpreted as a literal value treats all special characters, with the exception of the % symbol, as a part of the value that must be matched. In other words, a literal match condition set to `\'*'\` is only satisfied when that exact value (that is, `\'*'\`) is found.

A percentage symbol is used to indicate URL encoding (for example, `%20`).

### Wildcard values

Text that is interpreted as a wildcard value assigns additional meaning to special characters. The following table describes how the following set of characters is interpreted:

Character | Description
----------|------------
\ | A backslash is used to escape any of the characters specified in this table. A backslash must be specified directly before the special character that should be escaped.<br/>For example, the following syntax escapes an asterisk: `\*`
% | A percentage symbol is used to indicate URL encoding (for example, `%20`).
\* | An asterisk is a wildcard that represents one or more characters.
Space | A space character indicates that a match condition may be satisfied by either of the specified values or patterns.
'value' | A single quote does not have special meaning. However, a set of single quotes is used to indicate that a value should be treated as a literal value. It can be used in the following ways:<br><br/>- It allows a match condition to be satisfied whenever the specified value matches any portion of the comparison value.  For example, `'ma'` would match any of the following strings: <br/><br/>/business/**ma**rathon/asset.htm<br/>**ma**p.gif<br/>/business/template.**ma**p<br /><br />- It allows a special character to be specified as a literal character. For example, you may specify a literal space character by enclosing a space character within a set of single quotes (that is, `' '` or `'sample value'`).<br/>- It allows a blank value to be specified. Specify a blank value by specifying a set of single quotes (that is, '').<br /><br/>**Important:**<br/>- If the specified value does not contain a wildcard, then it is automatically considered a literal value, which means that it is not necessary to specify a set of single quotes.<br/>- If a backslash does not escape another character in this table, it is ignored when it is specified within a set of single quotes.<br/>- Another way to specify a special character as a literal character is to escape it using a backslash (that is, `\`).

### Regular expressions

Regular expressions define a pattern that is searched for within a text value. Regular expression notation defines specific meanings to a variety of symbols. The following table indicates how special characters are treated by match conditions and features that support regular expressions.

Special Character | Description
------------------|------------
\ | A backslash escapes the character the follows it, which causes that character to be treated as a literal value instead of taking on its regular expression meaning. For example, the following syntax escapes an asterisk: `\*`
% | The meaning of a percentage symbol depends on its usage.<br/><br/> `%{HTTPVariable}`: This syntax identifies an HTTP variable.<br/>`%{HTTPVariable%Pattern}`: This syntax uses a percentage symbol to identify an HTTP variable and as a delimiter.<br />`\%`: Escaping a percentage symbol allows it to be used as a literal value or to indicate URL encoding (for example, `\%20`).
\* | Sıfır veya daha fazla kez eşleştirilecek önceki karakteri bir yıldız işareti sağlar.
Uzay | Bir boşluk karakteri, genellikle bir değişmez değer olarak kabul edilir.
'value' | Tek tırnak işaretleri, sabit karakter olarak kabul edilir. Tek tırnak birtakım özel bir anlamı yok.

## <a name="next-steps"></a>Sonraki adımlar

- [Kural altyapısı eşleştirme koşulları](cdn-verizon-premium-rules-engine-reference-match-conditions.md)
- [Kural altyapısı koşullu ifadeleri](cdn-verizon-premium-rules-engine-reference-conditional-expressions.md)
- [Kural altyapısı özellikleri](cdn-verizon-premium-rules-engine-reference-features.md)
- [Kural altyapısı kullanarak HTTP davranışı geçersiz kılma](cdn-verizon-premium-rules-engine.md)
- [Azure CDN'ye genel bakış](cdn-overview.md)