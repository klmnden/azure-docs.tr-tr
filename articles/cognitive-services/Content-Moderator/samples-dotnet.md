---
title: Azure içerik denetleyici kod örnekleri | Microsoft Docs
description: İçerik yöneticiyi uygulamalarınızda kullanmak
services: cognitive-services
author: sanjeev3
manager: mikemcca
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: article
ms.date: 01/10/2018
ms.author: sajagtap
ms.openlocfilehash: a7f5b0a7433631e303de47667871cc1354053c08
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35352570"
---
# <a name="net-sdk-samples"></a>.NET SDK'sı örneği

Aşağıdaki listede, .NET için Azure içerik denetleyici SDK kullanılarak oluşturulan kod örnekleri bağlantılarını içerir.

- **Yardımcı kitaplık**: [kullanmak için bir içerik denetleyici istemcisi diğer örnekleri oluşturma](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/ContentModerator/ModeratorHelper/Clients.cs). Bkz: [Hızlı Başlangıç](content-moderator-helper-quickstart-dotnet.md).

## <a name="moderation"></a>Denetleme

- **Görüntü yönetimini**: [yazıtipleri, metin ve içeriği yetişkin ve saldırganlardan resmi değerlendirmek](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/ContentModerator/ImageModeration/Program.cs). Bkz: [Hızlı Başlangıç](image-moderation-quickstart-dotnet.md).
- **Özel resimler**: [özel görüntü listeleriyle orta](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/ContentModerator/ImageListManagement/Program.cs). Bkz: [Hızlı Başlangıç](image-lists-quickstart-dotnet.md).

> [!NOTE]
> Maksimum sınırı yoktur **5 görüntü listeleri** her listesine ile **10.000 görüntüleri aşmaması**.
>

- **Metin denetleme**: [ekran uygunsuz metin ve kişisel bilgileri (PII) için metin](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/ContentModerator/TextModeration/Program.cs). Bkz: [Hızlı Başlangıç](text-moderation-quickstart-dotnet.md).
- **Özel terimler**: [özel terim listeleriyle orta](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/ContentModerator/TermListManagement/Program.cs). Bkz: [Hızlı Başlangıç](term-lists-quickstart-dotnet.md).

> [!NOTE]
> Maksimum sınırı yoktur **5 terim listeler** her listesine ile **10.000 koşulları aşmaması**.
>

- **Video yönetimini**: [yetişkin ve saldırganlardan içerik için bir video tarama ve sonuçları elde](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/ContentModerator/VideoModeration/Program.cs). Bkz: [Hızlı Başlangıç](video-moderation-api.md).

## <a name="review"></a>İncele

- **Görüntü işleri**: [tarayan ve incelemeler oluşturan bir denetleme işi başlatmak](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/ContentModerator/ImageJobs/Program.cs). Bkz: [Hızlı Başlangıç](moderation-jobs-quickstart-dotnet.md).
- **Görüntü incelemeler**: [oluşturma incelemeler İnsan-içinde--döngü için](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/ContentModerator/ImageReviews/Program.cs). Bkz: [Hızlı Başlangıç](moderation-reviews-quickstart-dotnet.md).
- **Video incelemeleri**: [oluşturma video incelemeleri İnsan-içinde--for döngüsü](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/ContentModerator/VideoReviews/Program.cs). Bkz: [hızlı başlangıç](video-reviews-quickstart-dotnet.md)
- **Video Dökümü incelemeleri**: [oluşturma video dökümü incelemeleri İnsan-içinde--for döngüsü](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/ContentModerator/VideoTranscriptReviews/Program.cs) görmek [hızlı başlangıç](video-reviews-quickstart-dotnet.md)

Konumundaki tüm .NET örnekleri görmek [içerik denetleyici .NET örnekleri github'da](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/ContentModerator).
