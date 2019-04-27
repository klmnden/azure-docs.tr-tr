---
title: Tahmin desenleri Yardım
titleSuffix: Language Understanding - Azure Cognitive Services
description: Bir desen çok daha fazla konuşma sağlamadan bir amaç için daha yüksek doğruluk derecesi elde etmek sağlar.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 04/01/2019
ms.author: diberry
ms.openlocfilehash: 2a160ab7447304dc6eb14f76a723df4e8a4d9f46
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60813583"
---
# <a name="patterns-improve-prediction-accuracy"></a>Desenlerini tahmin doğruluğunu artırmak
Desenler, çeşitli konuşma çok benzer olduğunda doğruluğunu artırmak için tasarlanmıştır.  Bir desen çok daha fazla konuşma sağlamadan bir amaç için daha yüksek doğruluk derecesi elde etmek sağlar. 

## <a name="patterns-solve-low-intent-confidence"></a>Düşük hedefi güvenle desenleri çözün
Kuruluş şeması ile ilgili olarak çalışan bir rapor bir insan kaynakları uygulamasında göz önünde bulundurun. Bir çalışanın adı ve ilişki LUIS çalışanlar ilgili döndürür. Tom, çalışanın sahip bir yönetici adı Alice ve adlı Astları ekibi göz önünde bulundurun: Michael Rebecca ve Carl.

![Kuruluş Şeması görüntüsü](./media/luis-concept-patterns/org-chart.png)

|Konuşmalar|Tahmin hedefi|Intent puanı|
|--|--|--|
|Tom'ın alt kimdir?|GetOrgChart|.30|
|Tom alt kimdir?|GetOrgChart|.30|

LUIS, bir uygulamanın farklı uzunluktaki cümle, farklı sözcük sırasını ve hatta farklı sözcük (eş anlamlılar "alt", "manage", "rapor") ile 10 ile 20 konuşma arasında varsa, düşük güvenilirlik puanı döndürebilir. LUIS sözcük sırasını önemini anlamanıza yardımcı olması için bir desen oluşturun. 

Desen aşağıdaki durumlarda çözer: 

* Intent puanı düşük
* Doğru amaç en çok puan değil, ancak üst puanına çok yakın. 

## <a name="patterns-are-not-a-guarantee-of-intent"></a>Desenler hedefi garantisi değildir.
Desenlerini tahmin teknolojilerinin bir karışımını kullanın. Bir desende bir amaç için bir şablon utterance ayarı hedefi tahmin garantisi değil ancak güçlü bir sinyaldir. 

<a name="patterns-do-not-improve-entity-detection"/></a>

## <a name="patterns-do-not-improve-machine-learned-entity-detection"></a>Desenler makine öğrenilen varlık algılama geliştirmek değil

Bir desen, öncelikli olarak tahmin hedefleri ve rol için tasarlanmıştır. Pattern.any varlık, serbest biçimli varlıkları ayıklamak için kullanılır. Desenler varlıkları kullanırken, bir desen bir makine öğrenilen varlık algılamak yardımcı olmaz.  

Tek bir düzen birden çok konuşma daralttığınızda geliştirilmiş varlık tahmin görmek beklemiyoruz. Ateşlenmesine basit varlıklar için Konuşma ekleme veya başka desen değil ateşlenir listesi varlıkları kullanın gerekir.

## <a name="patterns-use-entity-roles"></a>Varlık rolleri desenleri kullanın
İki veya daha fazla varlık desenindeki bağlamsal ilişkiliyse, varlık desenleri kullanın [rolleri](luis-concept-roles.md) varlıklarla ilgili bağlamsal bilgi ayıklamak için.  

## <a name="prediction-scores-with-and-without-patterns"></a>Desenler olmadan ve tahmin puanları
Yeterli örnek konuşma göz önünde bulundurulduğunda, LUIS tahmin olasılık desensiz imkanımız olacaktır. Desen çok konuşma sağlamaya gerek kalmadan güvenilirlik puanı artırın.  

## <a name="pattern-matching"></a>Desen eşleştirme
Bir düzeni desen içinde varlıkları ilk algılama ve ardından bir kelimelerin rest ve sözcük sırasını deseninin doğrulama göre eşleştirilir. Varlıklar, deseni eşleştirmek için bir desen için gereklidir. Desen karakter düzeyinde değil belirteci düzeyinde uygulanır. 

## <a name="pattern-syntax"></a>Desen sözdizimi
Desen, bir şablon için bir utterance sözdizimidir. Şablon, sözcük ve sözcükler yanı sıra eşleştirmek istediğiniz varlıkları ve yok saymak için istediğiniz noktalama içermelidir. Bu **değil** normal bir ifade. 

Varlıkları desenleri süslü ayraç tarafından çevrilmiş `{}`. Desenler, varlıkları ve varlıklar rolleriyle içerebilir. [Pattern.Any](luis-concept-entity-types.md#patternany-entity) yalnızca desenlerinde kullanılan bir varlık. 

Desen sözdizimi, aşağıdaki söz dizimini destekler:

|İşlev|Sözdizimi|İç içe geçme düzeyi|Örnek|
|--|--|--|--|
|varlık| {} -süslü ayraç|2|{Entity-adı} form nerede?|
|isteğe bağlı|[] - köşeli ayraç<BR><BR>Bir sınırı isteğe bağlıdır ve gruplandırma herhangi bir birleşimini iç içe geçme düzeyi 3 |2|Soru işareti olan isteğe bağlı [?]|
|Gruplandırma|() - parantez|2|olduğunu (bir \| b).|
|or| \| -dikey çubuk (kanal)<br><br>Dikey çubuk (veya) bir grubu 2'in bir sınır yoktur |-|Form olduğu ({form-adı-kısa} &#x7c; {form-adı-uzun} &#x7c; {form-number})| 
|Başlangıç ve/veya utterance sonu|^-şapka|-|^ utterance başlayın<br>utterance yapılır ^<br>^ {number} varlıkla tüm utterance katı değişmez değer eşleşme ^|

## <a name="nesting-syntax-in-patterns"></a>İç içe geçme söz dizimi desenleri

**İsteğe bağlı** sözdizimiyle köşeli parantez içinde iç içe iki düzey olabilir. Örneğin: `[[this]is] a new form`. Bu örnek için aşağıdaki konuşma sağlar: 

|İç içe geçmiş isteğe bağlı utterance örneği|Açıklama|
|--|--|
|Bu, yeni bir form|desenindeki tüm sözcüklerle eşleşir|
|Yeni form|Dış isteğe bağlı word ve isteğe bağlı olmayan sözcükler deseni ile eşleşir|
|Yeni bir form|yalnızca gerekli eşleşen sözcükler|

**Gruplandırma** sözdizimiyle parantezler, iç içe iki düzey olabilir. Örneğin: `(({Entity1.RoleName1} | {Entity1.RoleName2} ) | {Entity2} )`. Bu özellik, eşleştirilecek üç varlıkları sağlar. 

Entity1 roller (Seattle) kaynak ve hedef (Cairo) gibi bir konumdur ve liste varlığı (RedWest-C) bilinen yapı adı varlık 2 ise, aşağıdaki konuşma bu desene eşleme:

|İç içe gruplama utterance örneği|Açıklama|
|--|--|
|RedWest-C|Dış gruplandırma varlık eşleşir|
|Seattle|bir iç gruplandırma varlıkların eşleşir|
|Kahire|bir iç gruplandırma varlıkların eşleşir|

## <a name="nesting-limits-for-groups-with-optional-syntax"></a>İsteğe bağlı söz dizimi olan gruplar için iç içe geçme sınırı

Bir birleşimi **gruplandırma** ile **isteğe bağlı** sözdizimi 3 iç içe geçme düzeyi sınırı vardır.

|İzin Verilen|Örnek|
|--|--|
|Evet|([(test1 &#x7c; test2)] &#x7c; test3)|
|Hayır|([([test1] &#x7c; test2)] &#x7c; test3)|

## <a name="nesting-limits-for-groups-with-or-ing-syntax"></a>İç içe grupları veya-ing söz dizimi ile sınırları

Bir birleşimi **gruplandırma** ile **veya-ing** söz dizimi 2 dikey çubuklar sınırı vardır.

|İzin Verilen|Örnek|
|--|--|
|Evet|(test1 &#x7c; test2 &#x7c; (test3 &#x7c; test4))|
|Hayır|(test1 &#x7c; test2 &#x7c; test3 &#x7c; (test4 &#x7c; test5)) |

## <a name="syntax-to-add-an-entity-to-a-pattern-template"></a>Bir varlık için bir desen şablonu eklemek için söz dizimi
Surround gibi küme ayracı ile varlık adı deseni şablona bir varlık eklemek için `Who does {Employee} manage?`. 

|Varlık deseni|
|--|
|`Who does {Employee} manage?`|

## <a name="syntax-to-add-an-entity-and-role-to-a-pattern-template"></a>Bir varlık ve rol için bir desen şablonu eklemek için söz dizimi
Bir varlık rolü olarak gösterilir `{entity:role}` ile bir iki nokta üst üste ve ardından rol adı tarafından izlenen varlık adı. Desen şablona bir role sahip bir varlık eklemek için varlık adı ve küme ayracı ile rol adı gibi çevreleyen `Book a ticket from {Location:Origin} to {Location:Destination}`. 

|Varlık rolleriyle deseni|
|--|
|`Book a ticket from {Location:Origin} to {Location:Destination}`|

## <a name="syntax-to-add-a-patternany-to-pattern-template"></a>Bir pattern.any deseni şablonuna eklemek için söz dizimi
Pattern.any varlık varlığın değişen uzunluktaki desen eklemenizi sağlar. Düzen şablonu ve ardından sürece pattern.any herhangi bir uzunlukta olabilir. 

Eklemek için bir **Pattern.any** deseni şablon varlığa saran ve küme ayraçlarının Pattern.any varlıkla gibi `How much does {Booktitle} cost and what format is it available in?`.  

|Pattern.any varlık deseni|
|--|
|`How much does {Booktitle} cost and what format is it available in?`|

|Deseninde bir kitap adları|
|--|
|Ne kadardır **kitabın çalabilir** maliyet ve hangi biçimde, kullanılabilir?|
|Ne kadardır **isteyin** maliyet ve hangi biçimde, kullanılabilir?|
|Ne kadardır **gece zamanlı Mandal, merak olay** maliyet ve hangi biçimde, kullanılabilir?| 

LUIS sona ereceği kitap başlığı, bildiğinden sözcükler kitap başlığın Pattern.any varlığı temel alan LUIS için kafa karıştırıcı değildir.

## <a name="explicit-lists"></a>Açık listeler

oluşturma bir [açık listesi](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5ade550bd5b81c209ce2e5a8) özel durumuna izin yazma API aracılığıyla zaman:

* Desen içeren bir [Pattern.any](luis-concept-entity-types.md#patternany-entity)
* Ve deseni söz dizimi utterance üzerinde temel bir yanlış varlık ayıklama olasılığı sağlar. 

Örneğin, hem isteğe bağlı söz dizimi içeren bir düzeni olduğunu varsayalım `[]`ve varlık sözdizimi `{}`, birleştirilmiş bir şekilde hatalı verileri ayıklamak için.

Desen '[Bul] e-postadan hakkında {subject} [{person}]'.

Aşağıdaki konuşma içinde **konu** ve **kişi** varlık doğru ve hatalı bir şekilde ayıklanır:

|İfade|Varlık|Doğru ayıklama|
|--|--|:--:|
|Chris köpekler hakkında e-posta|Konu köpekler =<br>kişi Chris =|✔|
|ADAM La Mancha gelen e-posta|Konu man =<br>kişi La Mancha =|X|

Yukarıdaki tabloda, konu olmalıdır `the man from La Mancha` (bir kitap başlığı) ancak konu isteğe bağlı sözcüğünü içerdiğinden `from`, başlığı yanlış tahmin edildiğinde. 

Bu özel durumun desen için düzeltmek için ekleme `the man from la mancha` {subject} varlık kullanan bir açık listesi eşleşme olarak [açık bir listesi için API geliştirme](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5ade550bd5b81c209ce2e5a8).

## <a name="syntax-to-mark-optional-text-in-a-template-utterance"></a>İsteğe bağlı bir metin şablonu utterance olarak işaretlemek için söz dizimi
İsteğe bağlı normal ifade köşeli parantez sözdizimini kullanarak utterance metni işaretleme `[]`. İsteğe bağlı bir metin, köşeli ayraç yalnızca iki ayraçlar en fazla iç içe yerleştirebilirsiniz.

|İsteğe bağlı metin deseni|Anlamı|
|--|--|
|`[find] email about {subject} [from {person}]`|`find` ve `from {person}` isteğe bağlıdır|
|' Me [?] Yardım | Noktalama işareti isteğe bağlıdır.|

Noktalama işaretleri (`?`, `!`, `.`) yoksayılıp yoksayılmaması gerektiğini ve desenleri köşeli parantez sözdizimini kullanarak bunların yoksayılması gerekir. 

## <a name="pattern-only-apps"></a>Yalnızca desenini uygulama
Her hedefi için bir düzen var olduğu sürece hiçbir örnek konuşma olan hedefleri ile bir uygulama oluşturabilirsiniz. Bu örnek konuşma gerektirdiğinden deseni-yalnızca uygulama için makine öğrenilen varlıklar deseni içermelidir. 

## <a name="best-practices"></a>En iyi uygulamalar
Bilgi [en iyi uygulamalar](luis-concept-best-practices.md).

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Bu öğreticide desenlerini uygulama hakkında bilgi edinin](luis-tutorial-pattern.md)
