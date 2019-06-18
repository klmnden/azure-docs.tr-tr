---
title: Dışarı aktarma ve veri Sil
titleSuffix: Azure Cognitive Services
description: Gizlilik ve uyumluluk sağlamak için müşteri verilerini silin.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 04/02/2019
ms.author: diberry
ms.openlocfilehash: a82452f4b41aee9c4ea6f269d92fbc91a5697d16
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "64916949"
---
# <a name="export-and-delete-your-customer-data-in-language-understanding-luis-in-cognitive-services"></a>Language Understanding (LUIS) Bilişsel hizmetler, müşteri verilerini silebilir ve dışarı aktarma

Gizlilik ve uyumluluk sağlamak için müşteri verilerini silin. 

## <a name="summary-of-customer-data-request-features"></a>Müşteri verilerini talep özelliklerin özeti
Language Understanding Intelligent Service (LUIS) hizmeti çalıştırmak için müşteri içerik korur, ancak LUIS kullanıcı görüntüleme, verme ve verileri silme üzerinde tam denetime sahiptir. Bu LUIS web aracılığıyla yapılabilir [portalı](luis-reference-regions.md) veya [LUIS yazma (programlı olarak da bilinir) API'leri](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c2f).

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-intro-sentence.md)]

Müşteri içeriği, Microsoft bölgesel Azure depolamada şifrelenmiş olarak depolanır ve içerir:

- Kayıt sırasında toplanan kullanıcı hesabı içerik
- Eğitim veri modelleri oluşturmak için gerekli
- Tarafından kullanılan kullanıcı sorguları günlüğe [etkin olarak öğrenmeye](luis-concept-review-endpoint-utterances.md) modeli iyileştirmeye yardımcı olmak için
  - Kullanıcılar, sorgu günlüğünü devre dışı ekleyerek kapatabilir `&log=false` isteğine ayrıntıları [burada](troubleshooting.md#how-can-i-disable-the-logging-of-utterances)

## <a name="deleting-customer-data"></a>Müşteri verileri silme
LUIS kullanıcıların içerik LUIS web portalı veya LUIS (programlama da bilinir) yazma API'leri aracılığıyla herhangi bir kullanıcı silmek için tam denetime sahiptir. Aşağıdaki tabloda, her ikisi de Yardım bağlantıları görüntüler:

| | **Kullanıcı hesabı** | **Uygulama** | **Örnek Utterance(s)** | **Son kullanıcı sorguları** |
| --- | --- | --- | --- | --- |
| **Portal** | [Bağlantı](luis-concept-data-storage.md#delete-an-account) | [Bağlantı](luis-how-to-start-new-app.md#delete-app) | [Bağlantı](luis-concept-data-storage.md#utterances-in-an-intent) | [Etkin öğrenme konuşma](luis-how-to-review-endpoint-utterances.md#disable-active-learning)<br>[Oturum uzunluğu](luis-concept-data-storage.md#disable-logging-utterances) |
| **API'ler** | [Bağlantı](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c4c) | [Bağlantı](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c39) | [Bağlantı](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c0b) | [Bağlantı](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/58b6f32139e2bb139ce823c9) |


## <a name="exporting-customer-data"></a>Müşteri verilerini dışarı aktarma
LUIS yazma (programlı olarak da bilinir) API'leri aracılığıyla dışarı gerekir ancak LUIS kullanıcılar portalı, verileri görüntülemek için tam denetime sahiptir. Aşağıdaki tabloda LUIS yazma (programlı olarak da bilinir) API'leri aracılığıyla verileri dışarı aktarma ile Yardım bağlantıları görüntüler:

| | **Kullanıcı hesabı** | **Uygulama** | **Utterance(s)** | **Son kullanıcı sorguları** |
| --- | --- | --- | --- | --- |
| **API'ler** | [Bağlantı](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c48) | [Bağlantı](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c40) | [Bağlantı](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c0a) | [Bağlantı](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c36) |

## <a name="location-of-active-learning"></a>Etkin öğrenme konumu

Etkinleştirmek için [etkin olarak öğrenmeye](luis-how-to-review-endpoint-utterances.md#enable-active-learning), kullanıcıların oturum konuşma yayımlanan LUIS uç noktaları alınan, aşağıdaki Azure coğrafi bölgelerde depolanır:

* [Avrupa](#europe)
* [Avustralya](#australia)
* [Amerika Birleşik Devletleri](#united-states)

LUIS (aşağıda açıklanmıştır) etkin olarak öğrenmeye verileri hariç olmak üzere aşağıdaki [bölgesel hizmetler için veri depolama uygulamaları](https://azuredatacentermap.azurewebsites.net/). 

### <a name="europe"></a>Avrupa

[Eu.luis.ai](https://eu.luis.ai) portalı ve Avrupa yazma (program tabalı API'lerle olarak da bilinir) Azure'nın Avrupa Coğrafya barındırılır. Eu.luis.ai portalı ve Avrupa (program tabalı API'lerle olarak da bilinir) yazma aşağıdaki Azure coğrafyaları uç noktalarına dağıtımını destekler:

* Avrupa
* Fransa
* Birleşik Krallık

Bu Azure coğrafyaları için dağıtırken, son kullanıcıların uygulama uç noktası tarafından alınan konuşma etkin olarak öğrenmeye için Azure'nın Avrupa coğrafi depolanır. Etkin öğrenme devre dışı bırakmak, bkz: [etkin olarak öğrenmeye devre dışı](luis-how-to-review-endpoint-utterances.md#disable-active-learning). Saklı konuşma yönetmek için bkz: [Sil utterance](luis-how-to-review-endpoint-utterances.md#delete-utterance). 

### <a name="australia"></a>Avustralya

[Au.luis.ai](https://au.luis.ai) portalı ve Avustralya yazma (program tabalı API'lerle olarak da bilinir) Azure'nın Avustralya Coğrafya barındırılır. Au.luis.ai portalı ve Avustralya yazma (program tabalı API'lerle olarak da bilinir), aşağıdaki Azure coğrafyaları uç noktalarına dağıtımını destekler:

* Avustralya

Bu Azure coğrafyaları için dağıtırken, son kullanıcıların uygulama uç noktası tarafından alınan konuşma etkin olarak öğrenmeye için Azure'nın Avustralya coğrafi depolanır. Etkin öğrenme devre dışı bırakmak, bkz: [etkin olarak öğrenmeye devre dışı](luis-how-to-review-endpoint-utterances.md#disable-active-learning). Saklı konuşma yönetmek için bkz: [Sil utterance](luis-how-to-review-endpoint-utterances.md#delete-utterance). 

### <a name="united-states"></a>Amerika Birleşik Devletleri

[Luis.ai](https://www.luis.ai) portalı ve Amerika Birleşik Devletleri yazma (program tabalı API'lerle olarak da bilinir) Azure'nın ABD Coğrafya barındırılır. Luis.ai portalı ve Amerika Birleşik Devletleri (program tabalı API'lerle olarak da bilinir) yazma aşağıdaki Azure coğrafyaları uç noktalarına dağıtımını destekler:

* Azure coğrafyaları yazma bölgeleri, Avustralya ve Avrupa desteklenmiyor

Bu Azure coğrafyaları için dağıtırken, son kullanıcıların uygulama uç noktası tarafından alınan konuşma etkin olarak öğrenmeye için Azure'nın ABD coğrafi depolanır. Etkin öğrenme devre dışı bırakmak, bkz: [etkin olarak öğrenmeye devre dışı](luis-how-to-review-endpoint-utterances.md#disable-active-learning). Saklı konuşma yönetmek için bkz: [Sil utterance](luis-how-to-review-endpoint-utterances.md#delete-utterance). 


## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [LUIS bölgeleri başvurusu](./luis-reference-regions.md)
