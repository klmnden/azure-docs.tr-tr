---
title: Translator Konuşma Çevirisi hizmeti nedir?
titleSuffix: Azure Cognitive Services
description: Translator Konuşma Çevirisi hizmeti API'sini kullanarak uygulamalarınıza konuşmayı konuşmaya ve konuşmayı metne çevirme özelliklerini ekleyebilirsiniz.
services: cognitive-services
author: Jann-Skotdal
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-speech
ms.topic: overview
ms.date: 3/5/2018
ms.author: v-jansko
ROBOTS: NOINDEX,NOFOLLOW
ms.openlocfilehash: 24014bb06a779c214f18f966dfb1d26d61adee8d
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/27/2019
ms.locfileid: "60827536"
---
# <a name="what-is-translator-speech-api"></a>Translator Konuşma Çevirisi API'si nedir?

[!INCLUDE [Deprecation note](../../../includes/cognitive-services-translator-speech-deprecation-note.md)]

Translator Konuşma Çevirisi API'si, hedef işletim sistemi veya geliştirme dilinden bağımsız olarak, birden çok dilde konuşma çevirisine ihtiyaç duyan uygulamalara, araçlara veya çözümlere uçtan uca ve gerçek zamanlı konuşma çevirileri eklemek için kullanılabilir. API hem konuşmayı konuşmaya hem de konuşmayı metne çevirmek için kullanılabilir.

Bir Azure hizmeti olan Translator Metin Çevirisi API'si, buluttaki [Azure Bilişsel Hizmetler API'si](https://docs.microsoft.com/azure/) makine öğrenimi ve yapak zeka algoritmaları koleksiyonunun parçasıdır ve geliştirme projelerinizde kullanılmaya hazırdır.

Translator Konuşma Çevirisi API'si ile, istemci uygulamaları hizmete konuşma sesi akışı yapar ve kaynak dilde tanınan metni ve hedef dilde bu metnin çevirisini içeren metin ve ses tabanlı sonuçların akışını alır. Metin sonuçları, gelen ses akışına derin sinir ağlarıyla desteklenen Otomatik Konuşma Tanıma (ASR) uygulanarak oluşturulur. Ham ASR çıkışı kullanıcının amacını daha iyi yansıtması için TrueText olarak adlandırılan yeni bir teknikle geliştirilir. Örneğin, TrueText konuşmadaki istemsiz bozuklukları (hmm'lar ve öksürükler gibi), yinelenen sözcükleri kaldırır ve düzgün noktalama ve büyük harf kullanımını geri yükler. Küfürleri maskeleme veya çıkarma olanağı da sağlanır. Tanıma ve çeviri altyapıları, konuşmaları işlemek için özel olarak eğitilmiştir.

Translator Konuşma Çevirisi hizmeti, konuşmanın sonunu saptamak için sessizlik algılamasını kullanır. Ses etkinliğinde bir duraklama olduktan sonra, hizmet tamamlanan konuşma için nihai sonucun geri akışını yapar. Ayrıca hizmet kısmi sonuçları da geri gönderebilir; bu yolla devam eden konuşma için ara tanıma ve çeviriler sağlanır.

Konuşmanın konuşmaya çevirisi için, hizmet konuşulan metinden hedef dillerle sentezlenmiş konuşma (metin konuşmaya dönüştürme) olanağı sağlar. Metni konuşmaya dönüştürme sesi, istemci tarafından belirtilen biçimde oluşturulur. WAV ve MP3 biçimleri kullanılabilir.

Translator Konuşma Çevirisi API'si istemciyle sunucu arasında tam çift yönlü bir iletişim kanalı sağlamak için WebSocket protokolünü kullanır.

## <a name="about-microsoft-translator"></a>Microsoft Translator Hakkında
Microsoft Translator, bulut tabanlı bir makine çevirisi hizmetidir. Bu hizmetin temelinde, çeşitli Microsoft ürünlerini ve hizmetlerini desteklemenin yanı sıra dünya çapında binlerce işletmenin uygulamalarında ve iş akışlarında kullanılıp bu işletmelerin içeriklerini tüm dünyadaki hedef kitleye ulaştırmasını sağlayan [Translator Metin Çevirisi API'si](https://www.microsoft.com/en-us/translator/translatorapi.aspx) ve Translator Konuşma Çevirisi API'si yer alır.

[Microsoft Translator hizmeti](https://www.microsoft.com/en-us/translator/home.aspx) hakkında daha fazla bilgi edinin

## <a name="microsoft-translator-neural-machine-translation-nmt"></a>Microsoft Translator Sinirsel Makine Çevirisi (NMT)
Translator Konuşma Çevirisi API'si çevirileri sağlamak için hem eski istatistiksel makine çevirisini (SMT) hem de yeni sinirsel makine çevirisini (NMT) kullanır.

İstatistiksel makine çevirisi performans geliştirme açısından hareketsiz bir aşamaya ulaşmıştır. SMT ile genel sistemler için çeviri kalitesinde artık kayda değer bir geliştirme sağlanmamaktadır. Yeni AI tabanlı çeviri teknolojisi Sinir Ağları (NN) temelinde ivme kazanmaktadır.

NMT'nin sağladığı çeviriler SMT'ninkilere göre yalnızca ham çeviri kalitesi puanı açısından değil, daha akıcı olması ve insan diline daha çok benzemesi açısından da daha iyidir.
Bu akıcılığın temel nedeni, NMT'nin sözcükleri çevirmek için cümlenin bağlamından bütünüyle yararlanmasıdır. SMT ise her sözcüğün hemen birkaç sözcük öncesindeki ve sonrasındaki anlık bağlamı dikkate alır.

NMT modelleri API'nin temelindedir ve son kullanıcılara görünür değildir. Gözle görülür farklılıklar yalnızca şunlardır:
* Özellikle de Çince, Japonca ve Arapça gibi diller için geliştirilmiş çeviri kalitesi
* Mevcut Hub özelleştirme özellikleriyle uyumsuzluk (Microsoft Translator Metin Çevirisi API'siyle kullanım için)

Desteklenen konuşma çevirisi dillerinin hepsi NMT tarafından desteklenir. Dolayısıyla, tüm konuşmanın konuşmaya çevirilerinde NMT kullanılır.

Metnin konuşmaya çevirilerinde, dil çiftine bağlı olarak NMT ile SMT'nin bir bileşimi kullanılabilir. Hedef dil NMT tarafından destekleniyorsa, çevirinin tamamı NMT destekli olur. Hedef dil NMT tarafından desteklenmiyorsa, çeviri NMT ile SMT karması olur ve iki dil arasında "geçiş" için İngilizce kullanılır.

Desteklenen dilleri [Microsoft.com](https://www.microsoft.com/en-us/translator/languages.aspx)'da görüntüleyebilirsiniz.

[NMT'nin nasıl çalıştığı](https://www.microsoft.com/en-us/translator/mt.aspx#nnt) hakkında daha fazla bilgi edinin

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Kaydolma](translator-speech-how-to-signup.md)

> [!div class="nextstepaction"]
> [Kod yazmaya başlama](quickstarts/csharp.md)

## <a name="see-also"></a>Ayrıca bkz.
- [Bilişsel Hizmetler Belgeleri sayfası](https://docs.microsoft.com/azure/)
- [Bilişsel Hizmetler Ürün sayfası](https://azure.microsoft.com/services/cognitive-services/)
- [Çözüm ve fiyatlandırma bilgileri](https://www.microsoft.com/en-us/translator/home.aspx)
