---
title: "Portalda Azure medya Kırpıcıyı kullanın | Microsoft Docs"
description: "Azure Portal'da Azure medya Kırpıcıyı kullanarak Klip Oluştur"
services: media-services
keywords: "küçük; subclip; kodlama; ortam"
author: dbgeorge
manager: jasonsue
ms.author: dwgeo
ms.date: 11/10/2017
ms.topic: article
ms.service: media-services
ms.openlocfilehash: faaae7edbc2fb62ae219dd963f405e7246c982d4
ms.sourcegitcommit: 9a61faf3463003375a53279e3adce241b5700879
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/15/2017
---
# <a name="create-clips-with-azure-media-clipper-in-the-portal"></a>Küçük resimleri portalda Azure medya Kırpıcıyı ile oluşturma
Portalda Azure medya Kırpıcıyı klipleri varlıklarından media services hesaplarınızı oluşturmak için kullanabilirsiniz. Başlamak için Portalı'nda media services hesabınıza gidin. Ardından, **Subclip** sekmesi.

Üzerinde **Subclip** sekmesinde, klipleri oluşturmaya başlamak kullanabilirsiniz. Portalda, tek bit hızında MP4s, Çoklu bit hızlı MP4s ve geçerli akış Bulucusu ile yayımlanan Canlı arşivler Kırpıcıyı yükler. Yayımdan varlıklar yüklü değil.

## <a name="producing-clips"></a>Küçük resimleri üretme
Küçük resim oluşturmak için sürükleyip bir varlık küçük arabirim. İşareti kez biliniyorsa, bunları arabirimine el ile girebilirsiniz. Aksi takdirde kayıttan yürütme varlık veya istenen işareti bileşenini Bul ve işareti çıkış saati oynatma sürükleyin. İşaretleme veya işaretini out birer sağlanmazsa, küçük baştan başlatır veya sırasıyla Giriş varlık sonuna kadar devam eder.

Çerçeve-doğruluğu/GOP-doğruluk ile gezinmek için çerçeve İleri/GOP iletme veya çerçeve-geriye dönük/GOP-geriye dönük düğmelerini kullanın. Kırpma birden çok varlığı karşı için sürükleyin ve varlık Seçim Bölmesi küçük arabiriminden içine birden çok varlığı bırakın. Seçin ve yeniden sıralamak istediğiniz sırayı arabiriminde varlıklar. Varlık seçim bölmesi, her varlık için varlık süresi, türü ve çözümleme meta veriler sağlar. Birden çok varlığı birlikte birleştirme, her girdi dosyası kaynak çözünürlüğü göz önünde bulundurun. Kaynak çözümleri farklıysa, en yüksek çözünürlük varlık çözümleme karşılamak üzere daha düşük çözünürlük giriş upscaled. Kırpma iş çıktısı önizlemek için seçilen işareti sürelerinden Önizleme düğmesini ve küçük yürütür'ı seçin.

## <a name="producing-dynamic-manifest-filters"></a>Dinamik bildirim filtreleri üretme
[Dinamik bildirim filtreleri](https://azure.microsoft.com/blog/dynamic-manifest/) bildirim öznitelikleri ve varlık zaman çizelgesi dayalı kurallar açıklanmaktadır. Akış uç noktanızı çıkış çalma listesi (bildirim) nasıl değiştirdiğinde bu kurallar belirler. Filtre, hangi kesimleri kayıttan yürütme için akışı değiştirmek için kullanılabilir. Kırpıcıyı tarafından üretilen filtreler yerel filtreler ve kaynak varlık özeldir. İşlenmiş klip, farklı filtreler yeni varlıklar değildir ve oluşturmak için bir kodlama işi gerektirmez. Aracılığıyla hızlı oluşturulabilir [.NET SDK'sı](https://docs.microsoft.com/azure/media-services/media-services-dotnet-dynamic-manifest) veya [REST API](https://docs.microsoft.com/azure/media-services/media-services-rest-dynamic-manifest), ancak yalnızca GOP doğru olduğundan. Genellikle, akış için kodlanmış varlıklar iki saniye GOP boyutuna sahiptir.

Dinamik bir bildirim filtresi oluşturmak için Gelişmiş Ayarlar menüsünden kırpma modu olarak dinamik bildirim filtreyi seçin. Filtre oluşturmak için bir küçük üretmek için aynı işlemi izleyebilirsiniz. Filtreler yalnızca tek bir varlık karşı üretilebilir.

## <a name="submitting-clipping-jobs"></a>Kırpma işlerini gönderme
Küçük resim oluşturma işiniz bittiğinde, başlatma için gönderme iş düğmesi karşılık gelen kırpma iş veya dinamik bildirim çağrısı seçin.