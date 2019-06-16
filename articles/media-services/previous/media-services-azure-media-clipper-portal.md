---
title: Portalda Azure Media Clipper'ı kullanma | Microsoft Docs
description: Azure Portalı'ndan Azure Media Clipper'ı kullanarak küçük resimleri oluşturun
services: media-services
keywords: clip;subclip;encoding;media
author: dbgeorge
manager: jasonsue
ms.author: dwgeo
ms.date: 03/14/2019
ms.topic: article
ms.service: media-services
ms.openlocfilehash: 8c88caefb0909da55de87116a23fa520c1679cc2
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61465856"
---
# <a name="create-clips-with-azure-media-clipper-in-the-portal"></a>Portalda Azure Media Clipper'ı ile klip oluşturma  

Portalda Azure Media Clipper'ı klipleri varlıklarından, media services hesapları oluşturmak için kullanabilirsiniz. Başlamak için portalında media services hesabınıza gidin. Ardından, **alt klip** sekmesi.

Üzerinde **alt klip** sekmesinde, klipleri oluşturmaya başlayabilir. Portalda, tek bit hızlı MP4 Çoklu bit hızına sahip MP4'ler göndermenizi ve geçerli bir akış Bulucusu ile yayımlanan Canlı arşivlerinizin Clipper'ı yükler. Yayımlanmamış varlıklar yüklü değil.

Clipper'ı, şu anda genel Önizleme aşamasındadır. Azure portalında Clipper erişmek için şuna Git [genel Önizleme sayfası](https://portal.azure.com/?feature.subclipper=true).

Aşağıdaki resimde media services hesabınızda Clipper giriş sayfası gösterilmektedir: ![Azure Media Clipper'ı Azure portalında](media/media-services-azure-media-clipper-portal/media-services-azure-media-clipper-portal.png)

## <a name="producing-clips"></a>Küçük resimleri üretme
Küçük resim oluşturmak için sürükleyin ve bir varlık küçük arabirimi bırakın. İşareti kez biliniyorsa, bunları arabirimine el ile girebilirsiniz. Aksi durumda, kayıttan yürütme varlık veya istenen işareti açma bulma ve işareti genişletme zaman oynatma kafasını sürükleyin. İşaretleme veya işaretini genişletme zaman sağlanmazsa, küçük başından başlar veya sırasıyla Giriş varlığı sonuna dek devam eder.

Çerçeve-doğruluğu/GOP-doğruluğa sahip gitmek için kare ileri/GOP İleri veya çerçeveyi geriye dönük/GOP-dönük düğmeleri kullanın. Kırpma karşı birden çok varlığı için sürükleyin ve birden çok varlığı varlık seçim paneli küçük arabiriminden bırakın. Seçin ve yeniden sıralamak istediğiniz arabiriminde varlıklar. Varlık seçim paneli her varlık için varlık süresi, türü ve çözüm meta veri sağlar. Birden çok varlığı birlikte birleştirme, her giriş dosyasının kaynak çözme göz önünde bulundurun. Kaynak çözümler farklıysa, en yüksek çözünürlüğü varlık çözünürlüğünü karşılamak için daha düşük bir çözümleme girişi upscaled. Kırpma tanımlı işlemin çıktısını önizlemek için seçilen işareti sürelerinden Önizleme düğmesi ve klibi yürütür'ı seçin.

## <a name="producing-dynamic-manifest-filters"></a>Dinamik bildirim filtreleri üretme
[Dinamik bildirim filtreleri](https://azure.microsoft.com/blog/dynamic-manifest/) bildirim öznitelikleri ve varlık zaman çizelgesi dayalı kurallar açıklanmaktadır. Bu kurallar, akış uç noktanızı çıkış çalma listesi (manifest) nasıl yönetir belirler. Filtre, hangi segmentlerin kayıttan yürütme için akışı değiştirmek için kullanılabilir. Clipper tarafından üretilen filtreleri yerel filtreler ve kaynak varlığa özgüdür. İşlenmiş klipleri aksine filtreleri yeni varlıkları değildir ve oluşturmak için bir kodlama işi gerektirmez. Aracılığıyla kolayca oluşturulabilir [.NET SDK'sı](https://docs.microsoft.com/azure/media-services/media-services-dotnet-dynamic-manifest) veya [REST API](https://docs.microsoft.com/azure/media-services/media-services-rest-dynamic-manifest), ancak yalnızca GOP doğru değildir. Genellikle, akış için kodlanmış varlıklar iki saniye GOP boyutuna sahiptir.

Bir dinamik bildirim filtresi oluşturmak için gidin **varlıklar** sekmesinde ve istediğiniz varlığı seçin. Seçin **alt klip** üst menü düğmesi. Dinamik bildirim filtresi Gelişmiş Ayarlar menüsünden kırpma modu seçin. Ardından filtreyi oluşturmak için bir işlenen küçük üretmek için aynı işlem takip edebilirsiniz. Filtreler yalnızca tek bir varlığından üretilebilir.

Aşağıdaki görüntüde, Azure portalında dinamik bildirim Filtresi modunda Clipper gösterilmektedir: ![Azure Media Clipper'ı Azure portalında dinamik bildirim Filtresi modunda](media/media-services-azure-media-clipper-portal/media-services-azure-media-clipper-filter.PNG)

## <a name="submitting-clipping-jobs"></a>Kırpma işlerini gönderme
Küçük resim oluşturma işlemini tamamladığınızda, dinamik bildirim çağrı ve karşılık gelen kırpma iş başlatma için gönderme iş düğmesi seçin.

## <a name="next-steps"></a>Sonraki adımlar
Azure Media Clipper'ı kullanmaya başlamak için okuma [Başlarken](media-services-azure-media-clipper-getting-started.md) pencere dağıtma hakkında daha fazla ayrıntı için makaleyi.
