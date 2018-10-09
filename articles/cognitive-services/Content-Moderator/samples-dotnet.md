---
title: Kod örnekleri - Content Moderator, .NET
description: Uygulamalarınızda Content Moderator kullanın
services: cognitive-services
author: sanjeev3
manager: cgronlun
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: sample
ms.date: 01/10/2018
ms.author: sajagtap
ms.openlocfilehash: c5465a2de0924037eb93b9de652e93e85f166737
ms.sourcegitcommit: ad08b2db50d63c8f550575d2e7bb9a0852efb12f
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/26/2018
ms.locfileid: "47223160"
---
# <a name="net-sdk-samples"></a>.NET SDK örnekleri

Aşağıdaki listede .NET için Azure Content Moderator SDK'sı kullanılarak derlenen kod örneklerine bağlantılar bulunmaktadır.

- **Yardımcı kitaplık**: [Diğer örneklerde kullanmak üzere bir Content Moderator istemcisi oluşturun](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/ContentModerator/ModeratorHelper/Clients.cs). Bkz: [hızlı başlangıç](content-moderator-helper-quickstart-dotnet.md).

## <a name="moderation"></a>Denetleme

- **Görüntü denetimi**: [Bir görüntüyü yetişkinlere yönelik ve müstehcen içerik, metin ve yüzler için değerlendirme](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/ContentModerator/ImageModeration/Program.cs). Bkz: [hızlı başlangıç](image-moderation-quickstart-dotnet.md).
- **Özel görüntüler**: [Özel görüntü listeleriyle denetim](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/ContentModerator/ImageListManagement/Program.cs). Bkz: [hızlı başlangıç](image-lists-quickstart-dotnet.md).

> [!NOTE]
> Listesi sayısı üst sınırı, her biri **10.000 görüntüyü aşmamak** kaydıyla **5 listedir**.
>

- **Metin denetimi**: [Metinde küfür ve kişisel bilgiler (PII) arama](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/ContentModerator/TextModeration/Program.cs). Bkz: [hızlı başlangıç](text-moderation-quickstart-dotnet.md).
- **Özel terimler**: [Özel terim listeleriyle denetim](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/ContentModerator/TermListManagement/Program.cs). Bkz: [hızlı başlangıç](term-lists-quickstart-dotnet.md).

> [!NOTE]
> Üst sınır, her biri **10.000 terimi aşmamak** kaydıyla **5 listedir**.
>

- **Video denetimi**: [Videoda yetişkinlere yönelik ve müstehcen içerik arama ve sonuçları getirme](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/ContentModerator/VideoModeration/Program.cs). Bkz: [hızlı başlangıç](video-moderation-api.md).

## <a name="review"></a>Gözden geçirme

- **Görüntü işleri**: [Tarama yapan ve gözden geçirmeler oluşturan bir denetim işi başlatma](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/ContentModerator/ImageJobs/Program.cs). Bkz: [hızlı başlangıç](moderation-jobs-quickstart-dotnet.md).
- **Görüntü incelemeleri**: [Devrede insan araştıran gözden geçirmeler oluşturma](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/ContentModerator/ImageReviews/Program.cs). Bkz: [hızlı başlangıç](moderation-reviews-quickstart-dotnet.md).
- **Görüntü incelemeleri**: [Devrede insan araştıran video gözden geçirmeleri oluşturma](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/ContentModerator/VideoReviews/Program.cs). Bkz: [hızlı başlangıç](video-reviews-quickstart-dotnet.md)
- **Videoyu metne dönüştürme gözden geçirmeleri**: [Devrede insan araştırma için videoyu metne dönüştürme gözden geçirmeleri oluşturun](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/ContentModerator/VideoTranscriptReviews/Program.cs) Bkz: [hızlı başlangıç](video-reviews-quickstart-dotnet.md)

[GitHub'daki Content Moderator .NET örnekleri](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/ContentModerator) arasında tüm .NET örneklerine bakın.
