---
title: Özel Çevirmen nedir?
titleSuffix: Azure Cognitive Services
description: Özel Translator Microsoft Translator Hub için istatistiksel makine çevirisi (SMT) ancak sinirsel makine çevirisi (NMT) sistemleri için özel olarak yaptığı için benzer özellikleri sunar.
author: rajdeep-in
manager: christw
ms.service: cognitive-services
ms.subservice: translator-text
ms.date: 02/21/2019
ms.author: v-pawal
ms.topic: overview
ms.openlocfilehash: 5ce73e28d25f0e517da08895764adc5a31740a7e
ms.sourcegitcommit: 3d4121badd265e99d1177a7c78edfa55ed7a9626
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/30/2019
ms.locfileid: "66389649"
---
# <a name="what-is-custom-translator"></a>Özel Çevirmen nedir?

[Özel Translator](https://portal.customtranslator.azure.ai) Translator kuruluşlar, uygulama geliştiricilerinin sağlayan Microsoft Translator hizmeti özelliğidir ve özelleştirilmiş sinirsel makine çevirisi (NMT) sistemleri oluşturmak için dil hizmeti sağlayıcıları. Özelleştirilmiş çeviri sistemleri sorunsuzca mevcut uygulamalarınızın, iş akışları ve Web siteleri ile tümleştirin. [Özel Translator](https://portal.customtranslator.azure.ai/) ne benzer olanağı sunan [Microsoft Translator Hub](https://hub.microsofttranslator.com/) için istatistiksel makine çevirisi (SMT) ancak sinirsel makine çevirisi (NMT) sistemleri için özel olarak yapar.

Çevirisi sistemleri ile oluşturulmuş [özel Translator](https://portal.customtranslator.azure.ai) aynı bulut tabanlı kullanılabilir [güvenli](https://cognitive.uservoice.com/knowledgebase/articles/1147537-api-and-customization-confidentiality), yüksek performans, yüksek oranda ölçeklenebilir Microsoft Translator [Text API v3 sürümüne](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-translate?tabs=curl), her gün milyarlarca çevirileri güçlendirir.

Özel Translator düzine üç'den fazla dili destekleyen ve NMT için kullanılabilen dilleri için doğrudan eşler. Tam bir listesi için bkz. [Microsoft Translator diller](https://docs.microsoft.com/azure/cognitive-services/translator/language-support#customization).

## <a name="features"></a>Özellikler

Özel Translator özel çeviri sistemi oluşturun ve daha sonra erişmek için farklı özellikler sağlar.

|Özellik  |Açıklama  |
|---------|---------|
|[Sinirsel makine çevirisi teknolojisini kullanır](https://blogs.msdn.microsoft.com/translation/2016/11/15/microsoft-translator-launching-neural-network-based-translations-for-all-its-speech-languages/)     |  Sinirsel makine özel translator tarafından sağlanan çevirisi (NMT) yararlanarak, çeviri geliştirin.       |
|[İş terminolojisi bilir sistemleri oluşturun](what-are-parallel-documents.md)     |  Özelleştirme ve kendi iş ve sektör kullanılan terimler anlayan paralel belgeleri kullanarak çevirisi sistemleri oluşturun.       |
|[Modellerinizi oluşturmak için Sözlük kullanma](what-is-dictionary.md)     |   Eğitim veri kümesi yoksa, yalnızca sözlük veri modeliyle eğitebilirsiniz.       |
|[Başkalarıyla işbirliği yapın](how-to-manage-settings.md#share-your-workspace)     |   Çalışmanızı başka kişilerle paylaşarak takımınızla birlikte çalışın.     |
|[Kendi özel çeviri modeline erişme](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-translate?tabs=curl)     |  Özel çeviri modelinizi istediğiniz zaman, var olan uygulamalarınız tarafından erişilebilir / program Microsoft Translator Text API v3 sürümüne.       |

## <a name="get-better-translations"></a>Daha iyi Çeviriler Al

Microsoft Translator yayımlanan [sinirsel makine çevirisi (NMT)](https://blogs.msdn.microsoft.com/translation/2016/11/15/microsoft-translator-launching-neural-network-based-translations-for-all-its-speech-languages/) 2016. Endüstri standardında NMT sağlanan çeviri kalitesini önemli gelişmeler [istatistiksel makine çevirisi (SMT)](https://en.wikipedia.org/wiki/Statistical_machine_translation) teknoloji. NMT daha iyi tam cümlelerden bağlamında çevirme önce yakaladığından, daha yüksek kalitede, daha fazla insan görünen ve daha fluent çevirileri sağlar. [Özel Translator](https://portal.customtranslator.azure.ai) NMT özel modeller elde edilen daha iyi bir çeviri kalitesi sağlanır.

Bir çeviri sistem oluşturmak için önceden çevrilmiş belgelerin kullanabilirsiniz. Bu belgeler etki alanına özgü terminoloji ve stil, daha iyi bir genel çeviri sistemi içerir. Kullanıcılar, hizalama, PDF, LCL, HTML, HTM, XLF, TMX, XLIFF, TXT, DOCX ve XLSX belgeler karşıya yükleyebilirsiniz.

Özel Translator, veri toplama ve hazırlık daha etkili yapmak için belge düzeyinde paralel verileri de kabul eder. Kullanıcılar birden çok dilde ancak ayrı belgelerde aynı içerik sürümleri erişimi varsa, özel Translator cümleler belgeler arasında otomatik olarak eşleşecek şekilde mümkün olacaktır.

Uygun türde ve eğitim veri miktarını sağlanmazsa, bu görmek için yaygın görülen bir durumdur [BLEU puanı](what-is-bleu-score.md) kazançlar özel Translator'ı kullanarak 5 ve 10 noktaları arasında.

## <a name="be-productive-and-cost-effective"></a>Üretken ve uygun maliyetli

İle [özel Translator](https://portal.customtranslator.azure.ai), eğitim ve özel bir sistem dağıtma herhangi bir programlama becerileri gerektirmez.

Güvenli kullanarak [özel Translator](https://portal.customtranslator.azure.ai) portal, kullanıcılar eğitim verilerini karşıya yükleme, eğitim sistemleri, test sistemleri ve sezgisel kullanıcı arabirimi aracılığıyla bir üretim ortamında dağıtın. Sistem sonra (gerçek zaman veri boyutu eğitim üzerinde bağlıdır) birkaç saat içinde uygun ölçekte kullanmak için kullanılabilir.

[Özel Translator](https://portal.customtranslator.azure.ai) ayrıca programlı olarak erişilebilir bir [API ayrılmış](https://custom-api.cognitive.microsofttranslator.com/swagger/) (şu anda önizlemede). API oluşturma veya güncelleştirme Eğitim düzenli olarak, kendi uygulama veya Web hizmeti aracılığıyla yönetmek kullanıcılar sağlar.

İçeriği çevirmek için özel bir modeli kullanma maliyetini kullanıcının Translator metin çevirisi API'si fiyatlandırma katmanını temel alır. Bilişsel Hizmetler'e bakın [Translator Text API Web sayfası fiyatlandırma](https://azure.microsoft.com/pricing/details/cognitive-services/translator-text-api/) için fiyatlandırma katmanı ayrıntıları.

## <a name="securely-translate-anytime-anywhere-on-all-your-apps-and-services"></a>Tüm uygulamaları ve hizmetleri üzerinde dilediğiniz zaman, istediğiniz yerden güvenli bir şekilde Çevir

Özel sistemlere sorunsuz bir şekilde erişilebilen ve ürün ya da iş akışlarınızın ve standart REST teknolojisi aracılığıyla Microsoft Translator Text API aracılığıyla herhangi bir cihazda tümleşik.

## <a name="next-steps"></a>Sonraki adımlar

- [Fiyatlandırma ayrıntıları](https://azure.microsoft.com/pricing/details/cognitive-services/translator-text-api/) hakkında bilgi edinin.

- İle [hızlı](quickstart-build-deploy-custom-model.md) bir çeviri modeline özel Translator oluşturmayı öğrenin.
