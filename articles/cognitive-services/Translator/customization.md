---
title: Microsoft Çeviricisi metin API çeviri özelleştirme | Microsoft Docs
description: Tercih edilen terimleri ve stil kullanarak kendi makine çevirisi sistemi oluşturmak için Microsoft Çeviricisi hub'ı kullanın.
services: cognitive-services
author: Jann-Skotdal
manager: chriswendt1
ms.service: cognitive-services
ms.component: translator-text
ms.topic: article
ms.date: 05/10/2018
ms.author: v-jansko
ms.openlocfilehash: 1db22a414c41f338c4e7fd6ce9dc7ac739fa9237
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35354965"
---
# <a name="customize-your-text-translations"></a>Metin çevirileri özelleştirme

Microsoft özel Çeviricisi Önizleme Microsoft Translator Gelişmiş sinir makine çevirisi Microsoft Çeviricisi metin API (yalnızca sürüm 3) kullanarak metin çevirirken özelleştirme olanağı tanıyan Microsoft Translator hizmeti bir özelliğidir. 

Bu özellik kullanıldığında konuşma çeviri özelleştirmek için de kullanılabilir [Bilişsel hizmetler konuşma Önizleme](https://docs.microsoft.com/en-us/azure/cognitive-services/speech-service/).

## <a name="custom-translator"></a>Özel Çevirmen
Özel Çevirici ile kendi iş ve endüstri kullanılan terminolojiyi anladığınızdan sinir çeviri sistemleri oluşturabilirsiniz. Özelleştirilmiş çeviri sistem ardından var olan uygulamalar, iş akışları ve Web siteleri tümleştirir. 

### <a name="how-does-it-work"></a>Nasıl çalışır?
Daha önce çevrilen belgelerinizi (leaflets, Web sayfaları, belge, vb.) stili ve etki alanına özgü terminolojisi genel çeviri sistem daha iyi yansıtan bir çeviri sistem oluşturmak için kullanın. Kullanıcılar TMX, XLIFF, TXT, DOCX ve XLSX belgeleri karşıya yükleyebilir.  

Sistem Ayrıca belge düzeyinde paralel olan ancak henüz cümle düzeyinde uymayan verileri kabul eder. Kullanıcılar, birden çok dilde ancak ayrı belgelerde aynı içerik sürümleri erişimi varsa özel Çeviricisi cümleleri belgeler arasında otomatik olarak eşleşen kuramaz.  Sistem tek dilli Kinsoku'ya veri birini veya her ikisini dillerde çevirileri geliştirmek üzere paralel eğitim verileri tamamlamak için de kullanabilirsiniz. 

Özelleştirilmiş sistem sonra normal bir çağrı Microsoft Çeviricisi metin kategori parametresini kullanarak API aracılığıyla kullanılabilir.

Eğitim veri miktarı ve uygun türe, 5 ile 10 arasında kazanç beklediğiniz vermektir veya özel Çeviricisi kullanarak daha da fazla BLEU çeviri kalitesini işaret eder.

Kullanılabilir verilerine dayalı özelleştirme çeşitli düzeyleri hakkında daha fazla ayrıntı bulunabilir [özel Çeviricisi Kullanıcı Kılavuzu'na](http://aka.ms/CustomTranslatorDocs).


## <a name="microsoft-translator-hub"></a>Microsoft Çeviricisi Hub

Eski Microsoft Çeviricisi Hub istatistiksel makine çevirisi çevirmek için kullanılabilir. [Daha fazla bilgi](https://www.microsoft.com/en-us/translator/hub.aspx) 

## <a name="custom-translator-versus-hub"></a>Hub karşı özel Çevirmen

|   | **Hub** | **Özel Çevirmen**|
|:-----|:----:|:----:|
|Özelleştirme özellik durumu   | Genel Erişilebilirlik  | Önizleme |
| Metin API sürümü  | Yalnızca v2   | Yalnızca v3 |
| SMT özelleştirme | Evet   | Hayır | 
| NMT özelleştirme | Hayır    | Evet |
| Yeni birleşik konuşma Hizmetleri özelleştirme | Hayır    | Evet | 
| [Hiçbir izleme](http://www.aka.ms/notrace) | Evet   | Evet | 

## <a name="collaborative-translations-framework"></a>İşbirlikçi çevirileri Framework

> [!NOTE]
> 1 Şubat 2018 itibariyle AddTranslation() ve AddTranslationArray() artık Çevirici metin API V2.0 ile kullanılabilir. Bu yöntemler başarısız olur ve hiçbir şey yazılır. Çevirici metin API V3.0 bu yöntemleri desteklemez.

>Benzer işlevsellik Çeviricisi Hub API içinde kullanılabilir. Bkz: [ https://hub.microsofttranslator.com/swagger ](https://hub.microsofttranslator.com/swagger). 

## <a name="next-steps"></a>Sonraki adımlar
> [!div class="nextstepaction"]
> [Özel Çeviricisi kullanarak bir özelleştirilmiş dil sistemi kurulumu](http://aka.ms/CustomTranslatorDocs)
