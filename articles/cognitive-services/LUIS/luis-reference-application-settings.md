---
title: Uygulama ayarları
titleSuffix: Azure Cognitive Services
description: Dil anlama uygulamalar için uygulama ayarları anlayın.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: article
ms.date: 05/29/2019
ms.author: diberry
ms.openlocfilehash: c19d328c3a5f4dd6dbe14ca94809e42c5655ea72
ms.sourcegitcommit: 3d4121badd265e99d1177a7c78edfa55ed7a9626
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/30/2019
ms.locfileid: "66391151"
---
# <a name="application-settings"></a>Uygulama ayarları

Bu uygulama ayarlarını depolanan [dışarı](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c40) uygulama ve [güncelleştirilmiş](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/versions-update-application-version-settings) REST API'leri ile. Uygulama sürümü ayarlarınızı değiştirme, uygulama eğitim durumunuz için deneyimsiz sıfırlar.

|Ayar|Varsayılan değer|Notlar|
|--|--|--|
|NormalizePunctuation|True|Noktalama işaretleri kaldırır.|
|NormalizeDiacritics|True|Vurgu kaldırır.|

## <a name="diacritics-normalization"></a>Vurgu normalleştirme 

Utterance normalleştirme LUIS JSON uygulama dosyanıza Aksanlar için açma `settings` parametresi.

```JSON
"settings": [
    {"name": "NormalizeDiacritics", "value": "true"}
] 
```

Aşağıdaki konuşma Aksanları normalleştirme konuşma nasıl etkilediğini gösterir:

|İle Aksanları false olarak ayarlayın|Vurgu kümesiyle true|
|--|--|
|`quiero tomar una piña colada`|`quiero tomar una pina colada`|
|||

### <a name="language-support-for-diacritics"></a>Aksanlar için dil desteği

#### <a name="brazilian-portuguese-pt-br-diacritics"></a>Portekizce (Brezilya) `pt-br` Vurgu

|Vurgu false olarak ayarlayın.|Vurgu false olarak ayarlayın.|
|-|-|
|`á`|`a`|
|`â`|`a`|
|`ã`|`a`|
|`à`|`a`|
|`ç`|`c`|
|`é`|`e`|
|`ê`|`e`|
|`í`|`i`|
|`ó`|`o`|
|`ô`|`o`|
|`õ`|`o`|
|`ú`|`u`| 
|||

#### <a name="dutch-nl-nl-diacritics"></a>Felemenkçe `nl-nl` Vurgu

|Vurgu false olarak ayarlayın.|Vurgu false olarak ayarlayın.|
|-|-|
|`á`|`a`|
|`à`|`a`|
|`é`|`e`|
|`ë`|`e`|
|`è`|`e`|
|`ï`|`i`|
|`í`|`i`|
|`ó`|`o`|
|`ö`|`o`|
|`ú`|`u`| 
|`ü`|`u`|
|||

#### <a name="french-fr--diacritics"></a>Fransızca `fr-` Vurgu

Bu, Fransızca ve Kanada subcultures içerir.

|Vurgu false olarak ayarlayın.|Vurgu false olarak ayarlayın.|
|--|--|
|`é`|`e`|
|`à`|`a`|
|`è`|`e`|
|`ù`|`u`|
|`â`|`a`| 
|`ê`|`e`| 
|`î`|`i`| 
|`ô`|`o`| 
|`û`|`u`| 
|`ç`|`c`| 
|`ë`|`e`| 
|`ï`|`i`| 
|`ü`|`u`| 
|`ÿ`|`y`| 

#### <a name="german-de-de-diacritics"></a>Almanca `de-de` Vurgu

|Vurgu false olarak ayarlayın.|Vurgu false olarak ayarlayın.|
|--|--|
|`ä`|`a`|
|`ö `|`o`| 
|`ü `|`u`| 

#### <a name="italian-it-it-diacritics"></a>İtalyanca `it-it` Vurgu

|Vurgu false olarak ayarlayın.|Vurgu false olarak ayarlayın.|
|--|--|
|`à`|`a`|
|`è`|`e`|
|`é`|`e`|
|`ì`|`i`| 
|`í`|`i`| 
|`î`|`i`| 
|`ò`|`o`| 
|`ó `|`o`| 
|`ù`|`u`|
|`ú`|`u`|

#### <a name="spanish-es--diacritics"></a>İspanyolca `es-` Vurgu

Bu, İspanyolca ve Kanada Meksika içerir.

|Vurgu false olarak ayarlayın.|Vurgu false olarak ayarlayın.|
|-|-|
|`á`|`a`|
|`é`|`e`|
|`í`|`i`| 
|`ó `|`o`| 
|`ú`|`u`|
|`ü`|`u`|
|`ñ`|`u`|


## <a name="punctuation-normalization"></a>Noktalama işaretleri normalleştirme

LUIS JSON uygulama dosyanıza noktalama utterance normalleştirme açma `settings` parametresi.

```JSON
"settings": [
    {"name": "NormalizePunctuation", "value": "true"}
] 
```

Aşağıdaki konuşma Aksanları konuşma nasıl etkilediğini gösterir:

|İle Aksanları false olarak ayarlayın|İle Aksanları true olarak ayarlayın|
|--|--|
|`Hmm..... I will take the cappuccino`|`Hmm I will take the cappuccino`|
|||

### <a name="punctuation-removed"></a>Noktalama işaretleri kaldırılmış

Aşağıdaki noktalama işareti ile kaldırılır `NormalizePunctuation` ayarlanır true.

|Noktalama işaretleri|
|--|
|`-`| 
|`.`| 
|`'`|
|`"`|
|`\`|
|`/`|
|`?`|
|`!`|
|`_`|
|`,`|
|`;`|
|`:`|
|`(`|
|`)`|
|`[`|
|`]`|
|`{`|
|`}`|
|`+`|
|`¡`|
