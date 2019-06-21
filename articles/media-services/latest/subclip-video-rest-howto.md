---
title: Alt klip Azure Media Services REST API'si ile kodlarken video
description: Bu konuda bir video REST kullanarak Azure Media Services ile kodlama, alt klip açıklar
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: ne
ms.topic: article
ms.date: 06/10/2019
ms.author: juliako
ms.openlocfilehash: df8c8a4040b4aae4379b4bfe0e9a16337588dd1b
ms.sourcegitcommit: 82efacfaffbb051ab6dc73d9fe78c74f96f549c2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67305242"
---
# <a name="subclip-a-video-when-encoding-with-media-services---rest"></a>Alt klip bir video kodlama medya Hizmetleri - REST

Trim ya da bir video kullanarak kodlama, alt klip bir [iş](https://docs.microsoft.com/rest/api/media/jobs). Bu işlev ile çalışır [dönüştürme](https://docs.microsoft.com/rest/api/media/transforms) kullanarak oluşturulan [BuiltInStandardEncoderPreset](https://docs.microsoft.com/rest/api/media/transforms/createorupdate#builtinstandardencoderpreset) hazır veya [StandardEncoderPreset](https://docs.microsoft.com/rest/api/media/transforms/createorupdate#standardencoderpreset) hazır. 

Bu konudaki REST örnek bir kodlama işi gönderir gibi bir video kırpar bir iş oluşturur. 

## <a name="prerequisites"></a>Önkoşullar

Bu konu başlığı altında açıklanan adımları tamamlamak için için gerekenler:

- [Azure Media Services hesabı oluşturma](create-account-cli-how-to.md).
- [Azure Media Services REST API çağrıları için Postman yapılandırma](media-rest-apis-with-postman.md).
    
    Konunun son adımı takip edin [Azure AD belirteci Al](media-rest-apis-with-postman.md#get-azure-ad-token). 
- Dönüşüm ve çıktı oluşturma varlıklar. Dönüşüm ve çıktı oluşturma gördüğünüz varlıkları [URL'sini temel alarak bir uzak dosya kodlama ve akışını video - REST](stream-files-tutorial-with-rest.md) öğretici.
- Gözden geçirme [kodlama kavramı](encoding-concept.md) konu.

## <a name="create-a-subclipping-job"></a>Klip iş oluşturma

1. İndirdiğiniz Postman koleksiyonu seçin **dönüşümler ve işler** -> **oluşturma işlemiyle alt kırpma**.
    
    **PUT** istek aşağıdaki gibi görünür:
    
    ```
    https://management.azure.com/subscriptions/:subscriptionId/resourceGroups/:resourceGroupName/providers/Microsoft.Media/mediaServices/:accountName/transforms/:transformName/jobs/:jobName?api-version={{api-version}}
    ```
1. "TransformName" ortam değişkeninin değerini, dönüştürme adıyla güncelleştirin. 
1. Seçin **gövdesi** sekmesi ve "myOutputAsset" çıkışınızı ile güncelleştirme varlık adı.

    ```
    {
      "properties": {
        "description": "A Job with transform cb9599fb-03b3-40eb-a2ff-7ea909f53735 and single clip.",
       
        "input": {
          "@odata.type": "#Microsoft.Media.JobInputHttp",
          "baseUri": "https://nimbuscdn-nimbuspm.streaming.mediaservices.windows.net/2b533311-b215-4409-80af-529c3e853622/",
          "files": [
                "Ignite-short.mp4"
            ],
          "start": {
            "@odata.type": "#Microsoft.Media.AbsoluteClipTime",
            "time": "PT10S"
          },
          "end": {
            "@odata.type": "#Microsoft.Media.AbsoluteClipTime",
            "time": "PT40S"
          }
        },
      
        "outputs": [
          {
            "@odata.type": "#Microsoft.Media.JobOutputAsset",
            "assetName": "myOutputAsset"
          }
        ],
        "priority": "Normal"
      }
    }
    ```
1. **Gönder**’e basın.

    Gördüğünüz **yanıt** ile oluşturulan ve gönderilen işi hakkında bilgileri ve iş durumu. 

## <a name="next-steps"></a>Sonraki adımlar

[Özel bir dönüşüm ile kodlama](custom-preset-rest-howto.md) 
