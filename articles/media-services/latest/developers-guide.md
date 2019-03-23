---
title: Azure Media Services v3 SDK - Azure
description: Bu makalede, SDK'ları / araçlarını kullanarak Media Services v3 API'si ile geliştirmeye başlamak nasıl bir genel bakış sağlar.
services: media-services
documentationcenter: na
author: Juliako
manager: femila
editor: ''
tags: ''
keywords: ''
ms.service: media-services
ms.devlang: multiple
ms.topic: overview
ms.tgt_pltfrm: multiple
ms.workload: media
ms.date: 03/20/2019
ms.author: juliako
ms.custom: ''
ms.openlocfilehash: cda6c9cd1f9c8b9305349f0904aeb744ba373711
ms.sourcegitcommit: 87bd7bf35c469f84d6ca6599ac3f5ea5545159c9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/22/2019
ms.locfileid: "58350258"
---
# <a name="start-developing-with-media-services-v3-api-using-sdkstools"></a>Media Services v3 API SDK'ları / araçlarla geliştirmeye başlayın

Bir geliştirici olarak, Media Services kullanabileceğiniz [REST API](https://aka.ms/ams-v3-rest-ref) veya kolayca oluşturmak için REST API ile etkileşimde bulunmanızı sağlayan istemci kütüphaneleri, yönetmek ve özel medya iş akışları korumak. [Media Services v3](https://aka.ms/ams-v3-rest-sdk) API (eski adıyla bir swagger dosyası da bilinir) Openapı belirtimini temel alır.

Bu konuda bağlantılar SDK'ları, araçları, belgeleri sağlar. Ayrıca, farklı Geliştirici env bazı yararlı bilgiler sağlar.

## <a name="prerequisites"></a>Önkoşullar

Media Services karşı geliştirmeye başlamak için ihtiyacınız vardır:

- Etkin bir Azure aboneliği. Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) oluşturun.
- [Temel kavramlar hakkında bilgi edinin](concepts-overview.md)
- [Bir Media Services hesabı oluşturma](create-account-cli-how-to.md)

## <a name="start-developing"></a>Geliştirmeye başlama

Azure Media Services şu SDK'ları / tools destekler 

- [Azure CLI](https://aka.ms/ams-v3-cli) 
- [.NET SDK](https://aka.ms/ams-v3-dotnet-sdk)
- [Java SDK](https://aka.ms/ams-v3-java-sdk)
- [Node.js SDK’sı](https://aka.ms/ams-v3-nodejs-sdk)
- [Python SDK'sı](https://aka.ms/ams-v3-python-sdk)
- [Go SDK'sı](https://aka.ms/ams-v3-go-sdk)
- [Ruby SDK](https://aka.ms/ams-v3-ruby-sdk)

### <a name="azure-media-services-explorer"></a>Azure Media Services Gezgini

[Azure Media Services Gezgini](https://github.com/Azure/Azure-Media-Services-Explorer) (AMSE), Media Services hakkında bilgi edinmek istiyorsanız Windows müşteriler için kullanılabilir bir araçtır. AMSE olduğu bir Winforms /C# karşıya yükleyin, indirin, kodlama, VOD akışı ve Media Services ile içerik Canlı uygulama. Media Services, herhangi bir kod yazmadan test etmek istediğiniz istemciler için AMSE aracıdır. AMSE kod, Media Services ile geliştirmek isteyen müşteriler için bir kaynak olarak sağlanır.

AMSE açık kaynak bir proje, destek, topluluk tarafından sağlanır (sorunları rapor https://github.com/Azure/Azure-Media-Services-Explorer/issues). Bu proje [Microsoft Open Source Code of Conduct](https://opensource.microsoft.com/codeofconduct/) (Microsoft Açık Kaynak Kullanım Kuralları) belgesinde listelenen kurallara uygundur. Daha fazla bilgi için [kullanım kuralları SSS](https://opensource.microsoft.com/codeofconduct/faq/) veya başvurun opencode@microsoft.com herhangi bir ek sorularınız ya da yorumlarınız ile.

## <a name="rest"></a>REST

Media Services REST API'leri ile geliştirmeye başlamak için bir REST İstemcisi'ni yükleyin. Örneğin **Postman**, **Visual Studio Code** REST eklentisiyle veya **Telerik Fiddler**. Kullanıyoruz [Postman](media-rest-apis-with-postman.md) Media Services v3 örnekleri için.

Medya Hizmetleri'ni keşfedin [REST API başvuru](https://aka.ms/ams-v3-rest-ref) belgeler ve bu örnekler göz atın:

- [Öğretici: URL'sini temel alarak bir uzak dosya kodlama ve akışını video - REST](stream-files-tutorial-with-rest.md)
- [Media Services hesabına - REST dosya yükleme](upload-files-rest-how-to.md)
- [Medya Hizmetleri - REST ile filtre oluşturma](filters-dynamic-manifest-rest-howto.md)
- [Azure Resource Manager tabanlı REST API](https://github.com/Azure-Samples/media-services-v3-arm-templates)

<!-- ## CLI -->
[!INCLUDE [media-services-cli-instructions](../../../includes/media-services-cli-instructions.md)]

Medya Hizmetleri'ni keşfedin [Azure CLI başvuru](https://aka.ms/ams-v3-cli-ref) belgeler ve bu örnekler göz atın:

- [Ölçek medya ayrılmış birimleri - CLI](media-reserved-units-cli-how-to.md)
- [Bir Media Services hesabı oluşturma - CLI](./scripts/cli-create-account.md) 
- [Sıfırlama hesabı kimlik bilgileri - CLI](./scripts/cli-reset-account-credentials.md)
- [Varlıklar - CLI oluşturma](./scripts/cli-create-asset.md)
- [Karşıya dosya - CLI](./scripts/cli-upload-file-asset.md)
- [Oluşturma dönüşümleri - CLI](./scripts/cli-create-transform.md)
- [Oluşturma işleri - CLI](./scripts/cli-create-jobs.md)
- [CLI EventGrid - oluşturma](./scripts/cli-create-event-grid.md)
- [Bir varlığı - CLI yayımlayın](./scripts/cli-publish-asset.md)
- [Filtre - CLI](filters-dynamic-manifest-cli-howto.md)

## <a name="net"></a>.NET

Medya Hizmetleri'ni keşfedin [.NET ref](https://aka.ms/ams-v3-dotnet-ref) belgeler ve bu örnekler göz atın:

- [Öğretici: Karşıya yükleme, kodlama ve akışını videoları - .NET](stream-files-tutorial-with-api.md) 
- [Öğretici: Stream Canlı Media Services v3 - .NET](stream-live-tutorial-with-api.md)
- [Öğretici: Media Services v3 - .NET ile videoları analiz etme](analyze-videos-tutorial-with-api.md)
- [Yerel bir dosyadan - .NET iş girdisi oluşturma](job-input-from-local-file-how-to.md)
- [Bir HTTPS URL'si - .NET iş girdisi oluşturma](job-input-from-http-how-to.md)
- [-.NET gibi özel bir dönüşüm ile kodlayın](customize-encoder-presets-how-to.md)
- [AES-128 dinamik şifreleme ve anahtar teslim hizmeti - .NET](protect-with-aes128.md)
- [DRM dinamik şifreleme ve lisans teslimat hizmeti - .NET](protect-with-drm.md)
- [Mevcut ilkeden - .NET bir imzalama anahtarı alma](get-content-key-policy-dotnet-howto.md)
- [Medya Hizmetleri - .NET ile filtre oluşturma](filters-dynamic-manifest-dotnet-howto.md)
- [Azure işlevler v2 Media Services v3 ile talep üzerine örnekleri video Gelişmiş](https://aka.ms/ams3functions)

## <a name="java"></a>Java

Media Services gözden [Java ref](https://aka.ms/ams-v3-java-ref) belgeleri.

## <a name="nodejs"></a>Node.js

Medya Hizmetleri'ni keşfedin [Node.js ref](https://aka.ms/ams-v3-nodejs-ref) belgeleri ve kullanıma [örnekleri](https://github.com/Azure-Samples/media-services-v3-node-tutorials) node.js ile Media Services API'sine kullanmayı gösterir.

## <a name="python"></a>Python

Media Services gözden [Python ref](https://aka.ms/ams-v3-python-ref) belgeleri.

## <a name="go"></a>Başlayın

Media Services gözden [Git başvuru](https://aka.ms/ams-v3-go-ref) belgeleri.

## <a name="next-steps"></a>Sonraki adımlar

- [Bir hesap oluşturma - CLI](create-account-cli-how-to.md)
- [API'lere erişim - CLI](access-api-cli-how-to.md)

