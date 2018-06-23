---
title: Dışarı aktarma ve müşteri verilerini - HALUK - silinmesini Azure Bilişsel hizmetler || Microsoft Docs
description: Dışarı aktarma ve Müşteri verilerinin silinmesini referansı dil anlama hizmetinden (HALUK).
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.technology: luis
ms.topic: article
ms.date: 05/23/2018
ms.author: v-geberr;
ms.openlocfilehash: f684b8ab875e2fbb774dc4a29bce25be41b24e6d
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35355445"
---
# <a name="export-and-delete-your-customer-data-in-language-understanding-luis-in-cognitive-services"></a>Dışarı aktarma ve içinde dil anlama (HALUK) Bilişsel Hizmetleri'ndeki müşteri verilerini sil

## <a name="summary-of-customer-data-request-features"></a>Müşteri verileri isteği özelliklerinin özeti
Dil anlama akıllı hizmet (HALUK) hizmeti çalıştırmak için müşteri içeriği korur, ancak HALUK kullanıcı verilerini silme görüntüleme ve dışarı aktarma üzerinde tam denetime sahiptir. Bu HALUK web yapılabilir [portal](https://docs.microsoft.com/en-us/azure/cognitive-services/luis/luis-reference-regions) veya [HALUK programlı API'leri](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c2f).

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-intro-sentence.md)]

Müşteri içerik Microsoft bölgesel Azure depolama alanında şifrelenmiş olarak depolanır ve içerir:

- Kullanıcı hesabı içeriği kayıt sırasında toplanan
- Eğitim verileri (yani hedefi & varlıkları) modelleri oluşturmak için gerekli
- Kullanıcı modeller geliştirmeye yardımcı olmak için çalışma zamanında oturum açmış kullanıcı sorguları
  - Kullanıcılar kapatma sorgu günlüğünü devre dışı ekleyerek `&log=false` isteğine ayrıntıları [burada](https://docs.microsoft.com/en-us/azure/cognitive-services/luis/luis-resources-faq#how-can-i-disable-the-logging-of-utterances)

## <a name="deleting-customer-data"></a>Müşteri verileri silme
HALUK kullanıcıların herhangi bir kullanıcı içeriği, HALUK web portalı veya HALUK programlı API'leri yoluyla silmek için tam denetime sahiptir. Aşağıdaki tabloda her ikisi de Yardım bağlantıları görüntüler:

| | **Kullanıcı hesabı** | **Uygulama** | **Utterance(s)** | **Son kullanıcı sorguları** |
| --- | --- | --- | --- | --- |
| **Portal** | [Bağlantı](https://docs.microsoft.com/en-us/azure/cognitive-services/luis/luis-how-to-account-settings) | [Bağlantı](https://docs.microsoft.com/en-us/azure/cognitive-services/luis/create-new-app#delete-app) | [Bağlantı](https://docs.microsoft.com/en-us/azure/cognitive-services/luis/create-new-app#delete-app) | [Bağlantı](https://docs.microsoft.com/en-us/azure/cognitive-services/luis/create-new-app#delete-app) |
| **API'ler** | [Bağlantı](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c4c) | [Bağlantı](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c0b) | [Bağlantı](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c0b) | [Bağlantı](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/58b6f32139e2bb139ce823c9) |


## <a name="exporting-customer-data"></a>Müşteri verileri dışarı aktarma
HALUK programlı API'leri aracılığıyla verilmelidir ancak HALUK kullanıcılar portalında verileri görüntülemek için tam denetime sahiptir. Aşağıdaki tabloda HALUK programlı API'leri aracılığıyla veri dışarı aktarma ile Yardım bağlantıları görüntüler:

| | **Kullanıcı hesabı** | **Uygulama** | **Utterance(s)** | **Son kullanıcı sorguları** |
| --- | --- | --- | --- | --- |
| **API'ler** | [Bağlantı](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c48) | [Bağlantı](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c40) | [Bağlantı](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c0a) | [Bağlantı](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c36) |


## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [HALUK bölgeler başvurusu](./luis-reference-regions.md)
