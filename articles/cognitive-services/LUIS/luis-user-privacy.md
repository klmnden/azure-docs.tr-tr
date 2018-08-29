---
title: Dışarı aktarma ve silme, müşteri verilerini - LUIS - Azure Bilişsel hizmetler || Microsoft Docs
description: Language Understanding hizmeti (LUIS) dışarı aktarma ve müşteri verilerini silme işlemi için başvuru.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.technology: luis
ms.topic: article
ms.date: 05/23/2018
ms.author: diberry
ms.openlocfilehash: c6fba28ea3a4b24f02b62b6c3f124569378e5bc8
ms.sourcegitcommit: 2ad510772e28f5eddd15ba265746c368356244ae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/28/2018
ms.locfileid: "43130552"
---
# <a name="export-and-delete-your-customer-data-in-language-understanding-luis-in-cognitive-services"></a>Language Understanding (LUIS) Bilişsel hizmetler, müşteri verilerini silebilir ve dışarı aktarma

## <a name="summary-of-customer-data-request-features"></a>Müşteri verilerini talep özelliklerin özeti
Language Understanding Intelligent Service (LUIS) hizmeti çalıştırmak için müşteri içerik korur, ancak LUIS kullanıcı görüntüleme, verme ve verileri silme üzerinde tam denetime sahiptir. Bu LUIS web aracılığıyla yapılabilir [portalı](luis-reference-regions.md) veya [LUIS program tabalı API'lerle](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c2f).

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-intro-sentence.md)]

Müşteri içeriği, Microsoft bölgesel Azure depolamada şifrelenmiş olarak depolanır ve içerir:

- Kayıt sırasında toplanan kullanıcı hesabı içerik
- Eğitim verilerini (yani hedefi & varlıklar) modelleri oluşturmak için gerekli
- Çalışma zamanında kullanıcı modelleri geliştirmeye yardımcı olmak için oturum açmış kullanıcı sorguları
  - Kullanıcılar, sorgu günlüğünü devre dışı ekleyerek kapatabilir `&log=false` isteğine ayrıntıları [burada](luis-resources-faq.md#how-can-i-disable-the-logging-of-utterances)

## <a name="deleting-customer-data"></a>Müşteri verileri silme
LUIS kullanıcılar LUIS web portalı veya LUIS programlı API'leri aracılığıyla, tüm kullanıcı içeriği silmek için tam denetime sahiptir. Aşağıdaki tabloda, her ikisi de Yardım bağlantıları görüntüler:

| | **Kullanıcı hesabı** | **Uygulama** | **Utterance(s)** | **Son kullanıcı sorguları** |
| --- | --- | --- | --- | --- |
| **Portal** | [Bağlantı](luis-how-to-account-settings.md) | [Bağlantı](luis-how-to-start-new-app.md#delete-app) | [Bağlantı](luis-how-to-start-new-app.md#delete-app) | [Bağlantı](luis-how-to-start-new-app.md#delete-app) |
| **API'ler** | [Bağlantı](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c4c) | [Bağlantı](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c39) | [Bağlantı](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c0b) | [Bağlantı](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/58b6f32139e2bb139ce823c9) |


## <a name="exporting-customer-data"></a>Müşteri verilerini dışarı aktarma
LUIS programlı API'ler aracılığıyla dışarı gerekir ancak LUIS kullanıcılar portalı, verileri görüntülemek için tam denetime sahiptir. Aşağıdaki tabloda bağlantıları ile verileri dışarı aktarmaları LUIS program tabalı API'lerle Yardım görüntüler:

| | **Kullanıcı hesabı** | **Uygulama** | **Utterance(s)** | **Son kullanıcı sorguları** |
| --- | --- | --- | --- | --- |
| **API'ler** | [Bağlantı](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c48) | [Bağlantı](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c40) | [Bağlantı](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c0a) | [Bağlantı](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c36) |


## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [LUIS bölgeleri başvurusu](./luis-reference-regions.md)
