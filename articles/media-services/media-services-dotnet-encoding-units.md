---
title: "Medya kodlama birimleri - Azure ekleyerek işleme ölçeğini |  Microsoft Docs"
description: "Bilgi nasıl .NET ile kodlama birimleri eklemek"
services: media-services
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.assetid: 33f7625a-966a-4f06-bc09-bccd6e2a42b5
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/09/2017
ms.author: juliako;milangada;
ms.openlocfilehash: 72a8729d22a9e76c8076d7a3347619a2163e4f09
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-scale-encoding-with-net-sdk"></a>.NET SDK ile kodlama ölçeklendirme
> [!div class="op_single_selector"]
> * [Portal](media-services-portal-scale-media-processing.md)
> * [.NET](media-services-dotnet-encoding-units.md)
> * [REST](https://docs.microsoft.com/rest/api/media/operations/encodingreservedunittype)
> * [Java](https://github.com/southworkscom/azure-sdk-for-media-services-java-samples)
> * [PHP](https://github.com/Azure/azure-sdk-for-php/tree/master/examples/MediaServices)
> 
> 

## <a name="overview"></a>Genel Bakış
> [!IMPORTANT]
> Gözden geçirdiğinizden emin olun [genel bakış](media-services-scale-media-processing-overview.md) konu işleme medya ölçeklendirme hakkında daha fazla bilgi için konu.
> 
> 

Ayrılmış birim türü ve .NET SDK kullanarak ayrılan birimler kodlama sayısını değiştirmek için aşağıdakileri yapın:

    IEncodingReservedUnit encodingS1ReservedUnit = _context.EncodingReservedUnits.FirstOrDefault();
    encodingS1ReservedUnit.ReservedUnitType = ReservedUnitType.Basic; // Corresponds to S1
    encodingS1ReservedUnit.Update();
    Console.WriteLine("Reserved Unit Type: {0}", encodingS1ReservedUnit.ReservedUnitType);

    encodingS1ReservedUnit.CurrentReservedUnits = 2;
    encodingS1ReservedUnit.Update();

    Console.WriteLine("Number of reserved units: {0}", encodingS1ReservedUnit.CurrentReservedUnits);

## <a name="opening-a-support-ticket"></a>Bir destek bileti açmadan
Varsayılan olarak her Media Services hesabı kodlama ve 5 isteğe bağlı akışa ayrılan birimler en fazla 25 ölçeklendirebilirsiniz. Bir destek bileti açılarak daha yüksek bir sınır isteyebilir.

### <a name="open-a-support-ticket"></a>Bir destek bileti açın
Bir destek açmak için bilet aşağıdakileri yapın:

1. Tıklatın [alma desteği](https://manage.windowsazure.com/?getsupport=true). Açmadıysanız, kimlik bilgilerinizi girmeniz istenir.
2. Aboneliğinizi seçin.
3. Destek türü'nün altında "Teknik" seçin.
4. "Bilet oluştur" seçeneğini tıklatın.
5. "Azure medya sonraki sayfada sunulan hizmetler" Ürün listesinde seçin.
6. "Sorun türü" seçin sorununuz için uygun olan.
7. Devam Et'i tıklatın.
8. Sonraki sayfasındaki yönergeleri izleyin ve sorunu ayrıntılarını girin.
9. Bilet için Gönder'e tıklayın.

## <a name="media-services-learning-paths"></a>Media Services’i öğrenme yolları
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

