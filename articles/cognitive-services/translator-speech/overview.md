---
title: Konuşma çeviri API belgelerine | Microsoft Docs
titleSuffix: Cognitive Services
description: Konuşma için konuşma eklemek için Microsoft Çeviricisi konuşma çeviri API ve konuşma metin çeviri uygulamalarınız için kullanın.
services: cognitive-services
author: Jann-Skotdal
manager: chriswendt1
ms.service: cognitive-services
ms.component: translator-speech
ms.topic: article
ms.date: 3/5/2018
ms.author: v-jansko
ms.openlocfilehash: 15f27e6b5b2fd7384958a660156855fc65f4e558
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35352432"
---
# <a name="microsoft-translator-speech-api"></a>Microsoft Translator Konuşma Tanıma API’si
Microsoft Çeviricisi konuşma API uygulamaları, Araçlar ya da birden fazla dil konuşma Çeviri hedef işletim sistemi veya geliştirme dilinden bağımsız olarak gerektiren herhangi bir çözüm uçtan uca, gerçek zamanlı konuşma çevirileri eklemek için kullanılabilir. API, konuşma ve konuşma metin çeviri için her iki konuşma için kullanılabilir.

Microsoft Çeviricisi metin API'dir, bir Azure hizmeti parçası [Microsoft Bilişsel Hizmetleri API koleksiyonu](https://docs.microsoft.com/azure/#pivot=products&panel=cognitive) makine öğrenme ve AI algoritmaları bulutta, geliştirme projelerinizi taşımalarına tüketilebilir.

Microsoft Çeviricisi konuşma API ile istemci uygulamaları hizmetine konuşma ses akışı ve bir akış kaynağı dil ve hedef dilde kendi çevirisi tanınan metni içeren metin ve ses tabanlı sonuçları geri alabilirsiniz. Metin sonuçlarını, otomatik konuşma tanıma (ASR) gelen ses akışına derin sinir ağları tarafından uygulayarak oluşturulur. Ham ASR çıktı daha fazla kullanıcının istediğinden daha yakından yansıtacak şekilde DoğruMetin adında yeni bir teknik tarafından geliştirilmiştir. Örneğin, DoğruMetin disfluencies (hmms ve coughs), yinelenen sözcükler ve uygun noktalama geri yükler ve büyük/küçük harf kaldırır. Maskeleme veya profanities hariç özelliği de dahil edilmiştir. Tanıma ve çeviri motorları özellikle konuşma konuşma işlemek için eğitilmiş olup. 

Konuşma çeviri hizmeti sessizlik algılama bir utterance sonuna belirlemek için kullanır. Sesli etkinliğinde bir duraklamadan sonra hizmeti yeniden tamamlanmış utterance nihai sonucu akışı sağlanacak. Hizmet ayrıca Ara teşekkürler ve bir utterance için çeviriler ediyor vermek geri kısmi sonuçlar gönderebilirsiniz. 

Konuşma için konuşma çevirisi hizmeti konuşma (okuma) hedef dillerde konuşulan metinden sentezlemek olanağı sağlar. Metin okuma ses, istemci tarafından belirtilen biçimde oluşturulur. WAV ve MP3 biçimleri kullanılabilir.

Konuşma çeviri API WebSocket Protokolü istemci ve sunucu arasında bir tam çift yönlü iletişim kanalı sağlamak için kullanır. 

## <a name="about-microsoft-translator"></a>Microsoft Çeviricisi hakkında
Microsoft Translator makine çevirisi bulut tabanlı bir hizmettir. Bu hizmet özünde [Çeviricisi metin API] olan (https://www.microsoft.com/en-us/translator/translatorapi.aspx) ve Çeviricisi konuşma çeşitli Microsoft ürünleri ve Hizmetleri güç ve işletmeler binlerce tarafından kendi uygulamaları ve iş akışlarında, dünya çapında kullanılan kendi içeriğinin API'si Dünya çapında bir hedef kitleye ulaşabilirsiniz.

Daha fazla bilgi edinmek [Microsoft Translator hizmeti](https://www.microsoft.com/en-us/translator/home.aspx)

## <a name="microsoft-translator-neural-machine-translation-nmt"></a>Microsoft Çeviricisi sinir makine çevirisi (NMT)
Microsoft Çeviricisi konuşma API çevirileri sağlamak için eski istatistiksel makine çevirisi (SMT) ve yeni sinir makine çevirisi (NMT) kullanır.

İstatistiksel makine çevirisi Plato performans geliştirme açısından sınırına ulaştı. Çeviri kalitesini artık SMT genel sistemler için herhangi bir önemli şekilde artırma. Yeni bir çeviri AI tabanlı teknoloji sinir ağları (NN üzerinde) temel satışlarının veriyor.

Daha iyi Çeviriler yalnızca açısından aynı zamanda çünkü Puanlama bir ham çeviri kalitesini'den fazla fluent, SMT daha fazla insan olanları ses NMT sağlar. Bu ortadan anahtar nedeni NMT tam bir cümle bağlamında kelimeleri kullanmasıdır. SMT önce ve sonra her sözcüğün yalnızca birkaç sözcük hemen bağlamını alır.

NMT modeller API özünde ve son kullanıcılar için görünür değildir. Yalnızca belirgin farklılıklar şunlardır:
* Çince, Japonca ve Arapça gibi diller için özellikle geliştirilmiş çeviri kalitesini
* Uyumsuzluk mevcut Hub özelleştirme özellikleriyle (kullanmak için Microsoft Çeviricisi metin API ile)

Tüm desteklenen konuşma çeviri dillerini tarafından NMT güç sağlar. Bu nedenle, tüm konuşma için konuşma çeviri NMT kullanır. 

Metin çeviri konuşma NMT ve SMT bileşimini dil çifti bağlı olarak kullanabilir. Hedef Dil NMT tarafından destekleniyorsa, tam çeviri NMT sağlanmıştır. Hedef Dil NMT tarafından desteklenmiyorsa, çeviri NMT ve iki dilleri arasında "Özet" olarak İngilizce kullanarak SMT karma ' dir. 

Görünüm desteklenen dillerden [Microsoft.com](https://www.microsoft.com/en-us/translator/languages.aspx). 

Daha fazla bilgi edinmek [NMT nasıl çalışır?](https://www.microsoft.com/en-us/translator/mt.aspx#nnt)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Kaydol](translator-speech-how-to-signup.md)

> [!div class="nextstepaction"]
> [Kod yazmaya başlayın](quickstarts/csharp.md)

## <a name="see-also"></a>Ayrıca bkz.
- [Bilişsel hizmetler belge sayfası](https://docs.microsoft.com/azure/#pivot=products&panel=cognitive)
- [Bilişsel hizmetler ürün sayfası](https://azure.microsoft.com/services/cognitive-services/)
- [Çözüm ve fiyatlandırma bilgileri](https://www.microsoft.com/en-us/translator/home.aspx) 
