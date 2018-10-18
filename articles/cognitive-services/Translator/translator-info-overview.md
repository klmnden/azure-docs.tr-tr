---
title: Translator Metin Çevirisi API’si nedir?
titlesuffix: Azure Cognitive Services
description: Çok dilli kullanıcı deneyimleri sağlamak için Translator Metin Çevirisi API’si ile uygulamalarınızı, web sitelerinizi, araçlarınızı ve diğer çözümlerinizi tümleştirin.
services: cognitive-services
author: noellelacharite
manager: cgronlun
ms.service: cognitive-services
ms.component: translator-text
ms.topic: overview
ms.date: 05/10/2018
ms.author: nolachar
ms.openlocfilehash: c0004960b7a234d374ec0ae1bdc2f6576b197705
ms.sourcegitcommit: f20e43e436bfeafd333da75754cd32d405903b07
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/17/2018
ms.locfileid: "49385035"
---
# <a name="what-is-translator-text-api"></a>Translator Metin Çevirisi API’si nedir?

Translator Metin Çevirisi API’si, [60’tan fazla dilde](languages.md) çok dilli kullanıcı deneyimleri sunmak üzere uygulamalarınızla, web sitelerinize, araçlarınızla veya diğer çözümlerinize sorunsuz bir şekilde tümleştirilebilir. Metinler arası dil çevirisi gerçekleştirmek için herhangi bir donanım platformu üzerinde herhangi bir işletim sistemi ile kullanılabilir.

Translator Metin Çevirisi API’si, buluttaki Azure [Bilişsel Hizmetler API’si](https://docs.microsoft.com/azure/#pivot=products&panel=ai) makine öğrenimi ve yapak zeka algoritmaları koleksiyonunun parçasıdır ve geliştirme projelerinizde kullanılmaya hazırdır.

## <a name="about-microsoft-translator"></a>Microsoft Translator Hakkında

Microsoft Translator, bulut tabanlı bir makine çevirisi hizmetidir. Bu hizmetin temelinde, çeşitli Microsoft ürünlerini ve hizmetlerini desteklemenin yanı sıra dünya çapında binlerce işletmenin uygulamalarında ve iş akışlarında kullanılıp bu işletmelerin içeriklerini tüm dünyadaki hedef kitleye ulaştırmasını sağlayan Translator Metin Çevirisi API'si ve [Translator Konuşma Çevirisi API'si](https://docs.microsoft.com/azure/cognitive-services/speech-service/speech-translation) yer alır.

Mevcut Translator Konuşma Çevirisi API'sini, Bing Konuşma API'sini ve Özel Konuşma Tanıma Hizmeti'ni (önizleme) birleştirilmiş ve tamamen özelleştirilebilir bir hizmet haline getiren [Bilişsel Hizmetler Konuşma önizlemesi](https://docs.microsoft.com/azure/cognitive-services/speech-service/) aracılığıyla Konuşma çevirisi de sunulur.  

[Microsoft Translator hizmeti](https://www.microsoft.com/en-us/translator/home.aspx) hakkında daha fazla bilgi edinin

## <a name="language-customization"></a>Dil özelleştirmesi

Temel Microsoft Translator hizmetinin bir uzantısı olan Custom Translator, sinirsel çeviri sistemini özelleştirmenize ve çeviriyi, belirlediğiniz terminoloji ve stile göre iyileştirmenize yardımcı olması için Translator Metin Çevirisi API'si ile birlikte kullanılabilir.

Custom Translator sayesinde, kendi işletmenizde veya sektörünüzde kullanılan terminolojiyi işleyebilen çeviri sistemleri oluşturabilirsiniz. Ardından, özelleştirilmiş çeviri sisteminiz mevcut uygulamalarınızın, iş akışlarınızın ve web sitelerinizin yanı sıra çeşitli cihaz türleriyle, normal Microsoft Translator Metin Çevirisi API'si aracılığıyla kategori parametresi kullanılarak kolayca tümleştirilebilir. 

[Dil özelleştirmesi](customization.md) hakkında daha fazla bilgi edinin

## <a name="microsoft-translator-neural-machine-translation"></a>Microsoft Translator Sinirsel Makine Çevirisi

Sinirsel Makine Çevirisi (NMT), yüksek kaliteli yapay zeka destekli makine çevirileri için yeni standarttır. 2010'un ortalarında kalite konusunda duraklama noktasına ulaşan eski İstatistiksel Makine Çevirisi (SMT) teknolojisinin yerini almıştır.

NMT, yalnızca ham çeviri kalitesi puanı değil, daha akıcı olması ve insan diline daha çok benzemesi açısından da daha iyi çeviriler sunar. Bu akıcılığın temel nedeni, NMT'nin sözcükleri çevirmek için cümlenin bağlamından bütünüyle yararlanmasıdır. SMT ise her bir sözcüğün hemen birkaç sözcük öncesindeki ve sonrasındaki anlık bağlamı dikkate alıyordu.

NMT modelleri API'nin temelindedir ve son kullanıcılara görünür değildir. Gözle görülür tek fark, özellikle de Çince, Japonca ve Arapça gibi diller için geliştirilmiş çeviri kalitesidir. 

[NMT'nin nasıl çalıştığı](https://www.microsoft.com/en-us/translator/mt.aspx#nnt) hakkında daha fazla bilgi edinin

## <a name="next-steps"></a>Sonraki adımlar

- [Fiyatlandırma ayrıntıları](https://azure.microsoft.com/pricing/details/cognitive-services/translator-text-api/) hakkında bilgi edinin.

- Erişim anahtarı için [kaydolun](translator-text-how-to-signup.md).

- [Hızlı Başlangıç](quickstarts/csharp.md), C# dilinde yazılan REST API çağrılarına ilişkin bir adım adım kılavuzdur. Olabildiğince az kod ile metinleri bir dilden diğerine nasıl çevireceğinizi öğrenin.

- [API başvuru belgeleri](https://docs.microsoft.com/azure/cognitive-services/Translator/reference/v3-0-reference) API'ler için teknik belgeler sağlar.

## <a name="see-also"></a>Ayrıca bkz.

- [Bilişsel Hizmetler Belgeleri sayfası](https://docs.microsoft.com/azure/#pivot=products&panel=ai)
- [Bilişsel Hizmetler Ürün sayfası](https://azure.microsoft.com/services/cognitive-services/)
- [Çözüm ve fiyatlandırma bilgileri](https://www.microsoft.com/en-us/translator/default.aspx)
