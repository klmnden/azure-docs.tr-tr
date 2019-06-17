---
title: Video Indexer - Azure bir kişi model özelleştirme
titlesuffix: Azure Media Services
description: Bu makalede, Video Indexer bir kişi modeli nedir ve nasıl özelleştireceğinizi genel bir bakış sağlar.
services: media-services
author: anikaz
manager: johndeu
ms.service: media-services
ms.subservice: video-indexer
ms.topic: article
ms.date: 05/15/2019
ms.author: anzaman
ms.openlocfilehash: c74b913fc3ac35039d914fc97c9c438d2e4a3069
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65799437"
---
# <a name="customize-a-person-model-in-video-indexer"></a>Video Indexer bir kişi model özelleştirme

Video Indexer, videolarınızdaki ünlü tanıma destekler. Ünlü tanıma özelliği, yaklaşık IMDB Wikipedia ve üst LinkedIn öğrenilenler gibi sık istenen bir veri kaynağına göre bir milyon yüzleri kapsar. Video Indexer tarafından tanınmayan yüzleri yine de algılandı ancak bırakılır adlandırılmamış. Müşteriler, özel kişi modelleri oluşturabilir ve Video Indexer'ın varsayılan olarak tanınmıyorsunuz yüz tanımaya etkinleştirin. Müşteriler, bu kişi modeli kişinin yüz tanıma, resim dosyalarıyla bir kişinin adını eşleştirerek oluşturabilirsiniz.  

Farklı kullanım örnekleri için hesabınızı oluşturabilmesine olanak sağlar, hesap başına birden çok kişi modeller oluşturmak airdrop yararlı olabilir. Örneğin, farklı kanallar sıralanacak hesabınızda içeriği geliyorsa, her kanal için ayrı bir kişi model oluşturmak isteyebilirsiniz. 

> [!NOTE]
> Her bir kişi modeli 1 milyona kadar kişi destekler ve her hesap 50 kişi modelleri sınırı vardır. 

Bir model oluşturulduktan sonra karşıya yükleme/dizin ya da bir video ölçeklemek belirli bir kişi modelin model kimliği sağlayarak kullanabilirsiniz. Video için yeni bir yüz eğitim, video ile ilişkilendirildi belirli özel model güncelleştirir. 

Birden çok kişi modeli desteği gerekmiyorsa, bir kişinin karşıya yükleme/dizin oluşturma veya yeniden dizin oluşturmaya videonuza model kimliği atamayın. Bu durumda, Video Indexer, hesabınızdaki varsayılan Kişi modelini kullanır. 

Video Indexer Web sitesi bulunan bir videoyu algılanan yüzeylere düzenlemek ve hesabınızı özel birden çok kişi modeli yönetmek için açıklandığı gibi kullanabileceğiniz [bir Web sitesini kullanarak bir kişi model özelleştirme](customize-person-model-with-website.md) konu. Bölümünde anlatıldığı gibi API de kullanabilirsiniz [API'leri kullanarak bir kişi model özelleştirme](customize-person-model-with-api.md).