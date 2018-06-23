---
title: Nasıl desenleri tahmin doğruluğunu artırmak öğrenin | Microsoft Docs
titleSuffix: Azure
description: Tasarım desenleri hedefi tahmin puanları artırmak ve varlıkları bulmak için öğrenin.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.technology: luis
ms.topic: article
ms.date: 06/08/2018
ms.author: v-geberr
ms.openlocfilehash: 58bfae51fda10d14d9b1c4ea34cc10345d9a90ac
ms.sourcegitcommit: 65b399eb756acde21e4da85862d92d98bf9eba86
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/22/2018
ms.locfileid: "36318067"
---
# <a name="patterns-improve-prediction-accuracy"></a>Desenler tahmin doğruluğunu artırmak
Desenler birkaç utterances çok benzer olduğunda doğruluğunu artırmak için tasarlanmıştır. Utterance için bir desen sağlayarak HALUK yüksek güvenilirlik tahmin sahip olabilir. 

## <a name="patterns-solve-low-intent-confidence"></a>Düşük hedefi güvenirlik desenleri çözmek
Kuruluş Şeması bir çalışan ile ilgili raporları bir insan kaynakları uygulaması göz önünde bulundurun. Çalışanın adı ilişkisi verilen, HALUK çalışanları dahil edilen döndürür. Zel, çalışanın sahip bir yönetici adı Alice ve adlı ast ekibi dikkate alın: Gamze, Rebecca ve Carl.

![Kuruluş Şeması görüntüsü](./media/luis-concept-patterns/org-chart.png)

|Konuşmalar|Tahmin hedefi|Hedefi puanı|
|--|--|--|
|Can'ın alt kim?|GetOrgChart|.30|
|Zel astı kim?|GetOrgChart|.30|

Bir uygulamanın farklı uzunlukta cümle, farklı sözcük sırasını ve hatta farklı sözcükler (anlamlıları "alt", "manage", "rapor") ile 10 ile 20 utterances arasında varsa, düşük güvenilirlik puan HALUK döndürebilir. HALUK sözcük sırasını önemini anlamanıza yardımcı olması için bir desen oluşturun. 

Desenler aşağıdaki durumlarda çözün: 

* Hedefi puanı düşük olduğunda
* Ne zaman doğru hedefi üst puan değil, ancak çok üst puan kapatın. 

## <a name="patterns-are-not-a-guarantee-of-intent"></a>Desenler bir garantisi hedefinin değildir
Desenler tahmin teknolojileri bir karışımını kullanın. Amacına şablonu utterance için bir desen ayarı hedefi tahmin garanti değil ancak güçlü bir sinyal değil. 

## <a name="patterns-do-not-improve-entity-detection"></a>Desenler varlık algılama artırmak değil
Desenler varlıklar gerektirirken, bir desen varlık algılamak korumaz. Bir desen yalnızca amaçları ve roller ile tahmin yardımcı olmak için tasarlanmıştır.  

## <a name="prediction-scores-with-and-without-patterns"></a>Tahmin puanları ile ve desenler olmadan
Yeterli örnek utterances verildiğinde, HALUK tahmin güvenirlik desenleri olmadan artırmak gerçekleştirebilir. Desenler sayıda utterances sağlamasına gerek kalmadan güvenirlik puan artırın.  

## <a name="pattern-matching"></a>Desen eşleştirme
Bir desen varlıklar düzeni içinde ilk algılama sonra sözcükleri kalan ve desen word sırasını doğrulama göre eşleşir. Varlıkları düzeni eşleştirmek için bir desen için gereklidir. 

## <a name="pattern-syntax"></a>Desen sözdizimi
Desen sözdizimi bir utterance yönelik bir şablondur. Şablon sözcükler ve sözcüklerin yanı sıra eşleştirmek istediğiniz varlıkları ve noktalama yoksaymak istediğiniz içermelidir. Bu **değil** normal bir ifade. 

Desenler varlıklarda süslü ayraç tarafından çevrelenen `{}`. Desenler varlıkları ve varlıkları rolleriyle içerebilir. Pattern.Any yalnızca düzenleri kullanılan bir varlıktır. Sözdizimi aşağıdaki bölümlerde açıklanmıştır.

### <a name="syntax-to-add-an-entity-to-a-pattern-template"></a>Bir desen şablon için bir varlık eklemek için sözdizimi
Surround gibi süslü ayraçlar varlık adıyla düzeni şablonuna bir varlık eklemek için `Who does {Employee} manage?`. 

```
Who does {Employee} manage?
```

### <a name="syntax-to-add-an-entity-and-role-to-a-pattern-template"></a>Bir varlık ve rol bir desen şablonuna eklemek için sözdizimi
Bir varlık rolü olarak belirtilir `{entity:role}` iki nokta ardından rol adı varlık adıyla. Bir role sahip bir varlık düzeni şablonuna eklemek için süslü ayraçlar ile rol adını ve varlık adı gibi çevreleyen `Book a ticket from {Location:Origin} to {Location:Destination}`. 

```
Book a ticket from {Location:Origin} to {Location:Destination}
```

### <a name="syntax-to-add-a-patternany-to-pattern-template"></a>Bir pattern.any düzeni şablonuna eklemek için sözdizimi
Pattern.any varlık desen değişken uzunlukta bir varlık eklemenizi sağlar. Desen şablonu ve ardından sürece pattern.any herhangi bir uzunlukta olabilir. 

Eklemek için bir **Pattern.any** varlık bir desen şablonda çevreleyen süslü ayraçlar Pattern.any varlıkla gibi `How much does {Booktitle} cost and what format is it available in?`.  

```
How much does {Booktitle} cost and what format is it available in?
```

|Düzeninde kitap başlıkları|
|--|
|Ne kadar **bu kitap çalabilir** maliyet ve hangi biçimi, kullanılabilir?|
|Ne kadar **isteyin** maliyet ve hangi biçimi, kullanılabilir?|
|Ne kadar **köpek gece saat içinde merak olay** maliyet ve hangi biçimi, kullanılabilir?| 

Bu kitap başlık örneklerde kitap başlığı bağlamsal sözcükleri HALUK için kafa karıştırıcı değildir. İçindeki bir desenle olduğundan ve bir Pattern.any varlık ile işaretlenmiş kitap başlığı sona ereceği HALUK bilir.

### <a name="explicit-lists"></a>Açık listeler
Desen bir Pattern.any içeriyorsa ve desen sözdizimi için olasılığı verir yanlış varlık ayıklama tabanlı üzerinde utterance, oluşturma bir [açık listesi](https://aka.ms/ExplicitList) özel durumuna izin geliştirme API'si aracılığıyla. 

Örneğin, her iki isteğe bağlı sözdizimi içeren bir düzeni olduğunu varsayalım `[]`ve varlık sözdizimi `{}`, birleştirilmiş veri yanlış ayıklamak için bir şekilde.

Desen '[Bul] e-posta {konuda} [kişiden {}]'. Aşağıdaki utterances içinde **konu** ve **kişi** doğru ve yanlış varlık ayıklanır:

|utterance|Varlık|Doğru ayıklama|
|--|--|:--:|
|Chris köpekler hakkında e-posta|Konu köpekler =<br>kişi Chris =|✔|
|ADAM La Mancha gelen e-posta|Konu adam =<br>kişi La Mancha =|X|

Önceki tabloda, utterance `email about the man from La Mancha`, konusu olmalıdır `the man from La Mancha` (bir kitap başlığı) ancak konu isteğe bağlı word içerdiğinden `from`, başlığı yanlış tahmin. 

Desen bu özel durumu düzeltmek için add `the man from la mancha` {konu} varlık kullanmak için bir açık listesi eşleşme olarak [API için açık listesi yazma](https://aka.ms/ExplicitList).

### <a name="syntax-to-mark-optional-text-in-a-template-utterance"></a>İsteğe bağlı metin şablonu utterance olarak işaretlemek için sözdizimi
İşaretlemek normal ifade köşeli ayraç sözdizimini kullanarak utterance isteğe bağlı metinde `[]`. İsteğe bağlı bir metin köşeli yalnızca iki köşeli kadar yerleştirebilirsiniz.

```
[find] email about {subject} [from {person}]
```

Noktalama işaretleri gibi `.`, `!`, ve `?` köşeli ayraç kullanarak göz ardı edilebilir. Bu işaretler yoksaymak için her işareti ayrı bir düzende olması gerekir. İsteğe bağlı sözdizimi şu anda birkaç öğelerin bir listedeki bir öğe yok sayılıyor desteklemiyor.

## <a name="patterns-only"></a>Yalnızca desenleri
HALUK amacı bir uygulamanın tüm örnek utterances olmadan sağlar. Bu kullanım desenlerini yalnızca kullanılan izin verilir. Desenler her düzeni en az bir varlık gerektirir. Bu örnek utterances gerektirdiğinden yalnızca desen uygulaması için makine öğrenilen varlıklar düzeni içeremez. 

## <a name="best-practices"></a>En iyi uygulamalar
Bilgi [en iyi uygulamalar](luis-concept-best-practices.md).

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Bu öğreticide desenlerini uygulama öğrenin](luis-tutorial-pattern.md)

[LUIS]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions
