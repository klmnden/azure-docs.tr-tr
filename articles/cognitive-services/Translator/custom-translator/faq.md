---
title: Sık sorulan sorular - özel Translator
titleSuffix: Azure Cognitive Services
description: Özel Translator hakkında sık sorulan soruların yanıtlarını sağlar.
author: rajdeep-in
manager: christw
ms.service: cognitive-services
ms.subservice: translator-text
ms.date: 02/21/2019
ms.author: v-pawal
ms.topic: reference
ms.openlocfilehash: 79dc5baacb19143f2b31de7f3822af382c2e6cf4
ms.sourcegitcommit: 3d4121badd265e99d1177a7c78edfa55ed7a9626
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/30/2019
ms.locfileid: "66384049"
---
# <a name="frequently-asked-questions"></a>Sık sorulan sorular

Bu makale, hakkında sık sorulan soruların yanıtlarını içerir. [özel Translator](https://portal.customtranslator.azure.ai).

## <a name="what-are-the-current-restrictions-in-custom-translator"></a>Geçerli özel Translator sınırlamaları nelerdir?

Kısıtlamaları ve dosya boyutu, model eğitiminin ve model dağıtımı ile ilgili sınırları vardır. Oluşturan eğitim özel Translator bir model oluşturmak için ayarlarken, bu kısıtlamaları göz önünde bulundurun.

- Gönderilen dosya boyutu 100 MB'den az olmalıdır.
- Tek dilli Kinsoku'ya veri desteklenmiyor.

## <a name="when-should-i-request-deployment-for-a-translation-system-that-has-been-trained"></a>Dağıtım için eğitim vermiş bir çeviri sistemi istediğinizde?

Bu, projeniz için en uygun çeviri sistemi oluşturmak için birkaç eğitimleri sürebilir. BLEU puanı ve / veya test sonuçlarını kabul edilebilir değilse, daha fazla eğitim verileri veya daha dikkatli bir şekilde filtrelenmiş verileri kullanarak denemek isteyebilirsiniz. Katı olmalıdır ve dikkatli tasarlama, kümenizi ayarlama ve testinizi, terminolojisi ve malzeme çevirmek istediğiniz stili tam temsili olarak ayarlanmalıdır. Eğitim verilerinizi oluşturma içinde daha serbest ve farklı seçeneklerle denemeler yapın. Sistem test sonuçlarında çevirilerinizi memnun olduğunuzda, bir sistem dağıtım talep eğitilen sisteminizi geliştirmek için eğitim eklemek için daha fazla veri içermeyen ve eğitilen model API'leri aracılığıyla erişmesini istediğiniz.

## <a name="how-many-trained-systems-can-be-deployed-in-a-project"></a>Kaç eğitilen sistemleri bir proje içinde dağıtılabilir mi?

Proje yalnızca bir eğitilen sistem dağıtılabilir. Projeniz için bir uygun çeviri sistemi oluşturmak için birkaç eğitimleri alabilir ve en iyi sonuç veren bir eğitim dağıtımını istemenizi öneririz. Eğitim kalitesini BLEU puana göre belirleyebilirsiniz (yüksek değer daha iyi) ve çevirilerin kalitesini dağıtımına uygun olduğuna karar vermeyle ilgili önce gözden geçirenlerle danışmanlık.

## <a name="when-can-i-expect-my-trainings-to-be-deployed"></a>Dağıtılacak my eğitimleri ne beklemeliyim?

Dağıtım, genellikle bir saatten az sürer.

## <a name="how-do-you-access-a-deployed-system"></a>Dağıtılmış bir sistemde nasıl erişim sağlanır?

Dağıtılmış sistemler CategoryID belirterek Microsoft Translator Text API v3 sürümüne erişilebilir. Translator metin çevirisi API'si hakkında daha fazla bilgi bulunabilir [API Başvurusu](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-reference) Web sayfası.

## <a name="how-do-i-skip-alignment-and-sentence-breaking-if-my-data-is-already-sentence-aligned"></a>Nasıl miyim hizalama ve verilerimi hizalı cümle ise bozucu cümle atlamaz?

Tümce hizalama ve cümle bozucu metin dosyalarını ve TMX dosyaları için özel Translator atlar `.align` uzantısı. `.align` dosyaları kullanıcılar bozucu özel Translator'ın cümle ve mükemmel bir şekilde hizalı olup ve başka hiçbir işleme gerek dosyalar için hizalama işlemi atlamak için bir seçenek sunuyoruz. Kullanmanızı öneririz `.align` mükemmel bir şekilde hizalı dosyalarının uzantısı.

Ayıklanan cümleler sayısı aynı temel ada sahip iki dosya eşleşmiyorsa, özel Translator hala cümle aligner çalıştıracağı `.align` dosyaları.

## <a name="i-tried-uploading-my-tmx-but-it-says-document-processing-failed"></a>My TMX karşıya çalıştım, ancak "Belge işleme başarısız oldu" diyor.

TMX TMX 1.4b uymasını sağlamak belirtimi, <https://www.gala-global.org/tmx-14b>.
