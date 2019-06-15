---
title: Video Indexer - Azure içinde bir dil modelini özelleştirin
titlesuffix: Azure Media Services
description: Bu makalede, Video Indexer, dil modeli nedir ve nasıl özelleştireceğinizi genel bir bakış sağlar.
services: media-services
author: anikaz
manager: johndeu
ms.service: media-services
ms.subservice: video-indexer
ms.topic: article
ms.date: 05/15/2019
ms.author: anzaman
ms.openlocfilehash: 516ecd8842e7b673201cc640b283c081a02d2b2f
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65799555"
---
# <a name="customize-a-language-model-with-video-indexer"></a>Video Indexer ile bir dil modelini özelleştirin

Video Indexer, Microsoft ile tümleştirilmesiyle otomatik konuşma tanıma destekler [özel konuşma hizmeti](https://azure.microsoft.com/services/cognitive-services/custom-speech-service/). Dil modeli uyarlama metinle, yani sözlük uyum sağlamak üzere altyapısı istediğiniz etki alanından karşıya yükleyerek özelleştirebilirsiniz. Modelinizi eğitin sonra varsayılan telaffuz varsayılarak yeni sözcük uyarlama metinli tanınır ve dil modeli, sözcük yeni olası dizileri öğreneceksiniz. Özel dil modelleri, İngilizce, İspanyolca, Fransızca, Almanca, İtalyanca, Çince (Basitleştirilmiş), Japonca, Rusça, Portekizce (Brezilya), Hintçe ve Kore dili için desteklenir. 

Örnek olarak (bağlamında Azure Kubernetes hizmeti), "Kubernetes" gibi yüksek oranda belirli bir sözcük ele alalım. Word'ün yeni Video Indexer olduğundan, "topluluklarına" kabul edilir. "Kubernetes" tanımak için modeli eğitmek gerekir. Diğer durumlarda, sözcükleri var, ancak dil modelini belirli bir bağlamda görünmesini beklediği değil. Örneğin, "container service" belirli bir sözcükler kümesini özel olmayan dil modeli tanımlayabileceği bir 2 sözcük dizisi değil.

Sözcüklerin listesini bir metin dosyasına bağlama olmadan yüklemek için seçeneği var. Bu, kısmi uyarlama olarak kabul edilir. Alternatif olarak, belgeleri veya cümleler daha iyi uyarlaması için içeriğinizin ilgili metin dosyaları karşıya yükleyebilirsiniz.

Video Indexer API veya Web sitesi özel dil modelleri oluşturup konularında anlatıldığı gibi kullanabileceğiniz [sonraki adımlar](#next-steps) bu konudaki.

## <a name="best-practices-for-custom-language-models"></a>Özel dil modelleri için en iyi uygulamalar

Video Indexer word birleşimleri, en iyi bilgi edinmek için olasılıklar göre öğrenir:

* Bunlar konuşulan gibi gerçek yeterli cümlelerden örnekler verin.
* Daha fazla satır başına yalnızca bir cümle yerleştirin. Aksi takdirde sistem cümleler olasılıklar öğreneceksiniz.
* Bir sözcük diğerleri karşı word artırmak için bir cümle olarak yerleştirmek kabul edilebilir, ancak sistem, tam cümlelerden gelen en iyi öğrenir.
* Mümkünse, yeni bir sözcük veya kısaltmalar uygulandığında, çok kullanım örneklerini sisteme mümkün olduğu kadar bağlam vermek için tam bir cümle verin.
* Çeşitli uyarlama seçenekler yerleştirin ve bunlar sizin için nasıl çalıştığını görmek bu seçeneği deneyin.
* Birden çok kez aynı tam cümle tekrarını kaçının. Bu giriş rest karşı sapması oluşturabilir.
* Genel olmayan simgeler eklemekten kaçınır (~, # % @ &) atılmış şekilde. Göründükleri cümleler ayrıca atılan.
* Bunun yapılması kadar artırma etkisini dilute çünkü yüz binlerce cümle, gibi çok büyük girişlerdeki yerleştirmeyin.

## <a name="next-steps"></a>Sonraki adımlar

[Dil modeli API'lerini kullanarak özelleştirme](customize-language-model-with-api.md)

[Web sitesini kullanarak dil modelini özelleştirin](customize-language-model-with-website.md)
