---
title: Azure Media Services v3 SDK - Azure
description: Bu makalede, SDK'larını kullanarak Media Services v3 API'si ile geliştirmeye başlamak nasıl bir genel bakış sağlar.
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
ms.date: 04/11/2019
ms.author: juliako
ms.custom: ''
ms.openlocfilehash: 9fb4d1561a661387f759aada9e776d43a95aa5c7
ms.sourcegitcommit: b8a8d29fdf199158d96736fbbb0c3773502a092d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/15/2019
ms.locfileid: "59564518"
---
# <a name="develop-against-media-services-v3-api-using-sdks"></a>Media Services v3 API'si SDK'larını kullanarak geliştirin

Bir geliştirici olarak, Media Services kullanabileceğiniz [REST API](https://aka.ms/ams-v3-rest-ref) veya kolayca oluşturmak için REST API ile etkileşimde bulunmanızı sağlayan istemci kütüphaneleri, yönetmek ve özel medya iş akışları korumak. [Media Services v3](https://aka.ms/ams-v3-rest-sdk) API (eski adıyla bir swagger dosyası da bilinir) Openapı belirtimini temel alır.

> [!NOTE]
> Azure Media Services v3 SDK iş parçacığı açısından güvenli olmasını garanti edilmez. Çok iş parçacıklı bir uygulama geliştirirken, istemci korumak veya iş parçacığı başına yeni bir AzureMediaServicesClient nesnesini kullanmak için kendi iş parçacığı eşitleme mantığı eklemeniz gerekir. Çoklu iş parçacığı oluşturma sorunları (örneğin,. NET'te HttpClient örneği) istemci kodunuz tarafından sağlanan isteğe bağlı nesneler tarafından sunulan, dikkatli olmalıdır.

Bu konuda bağlantılar SDK'ları, araçları, nasıl yapılır kılavuzları sağlar.

## <a name="prerequisites"></a>Önkoşullar

Media Services karşı geliştirmeye başlamak için ihtiyacınız vardır:

- Etkin bir Azure aboneliği. Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) oluşturun.
- [Temel kavramlar hakkında bilgi edinin](concepts-overview.md)
- Gözden geçirme [ortamla geliştirme Hizmetleri v3 API'ler](media-services-apis-overview.md)
- [Bir Media Services hesabı oluşturma - CLI](create-account-cli-how-to.md)

## <a name="start-developing-with-sdks"></a>SDK'larla geliştirmeye başlama

### <a name="net"></a>.NET

Kullanım [.NET SDK'sı](https://aka.ms/ams-v3-dotnet-sdk) için [Media Services'e bağlanmak](configure-connect-dotnet-howto.md).

Medya Hizmetleri'ni keşfedin [.NET başvurusu](https://aka.ms/ams-v3-dotnet-ref) belgeleri.

### <a name="java"></a>Java

Kullanım [Java SDK'sı](https://aka.ms/ams-v3-java-sdk) için [Media Services'e bağlanmak](configure-connect-java-howto.md).

Media Services gözden [Java başvurusu](https://aka.ms/ams-v3-java-ref) belgeleri.

### <a name="nodejs"></a>Node.js

Kullanım [Node.js SDK'sı](https://aka.ms/ams-v3-nodejs-sdk) için [Media Services'e bağlanmak](configure-connect-nodejs-howto.md).

Medya Hizmetleri'ni keşfedin [Node.js başvurusu](https://aka.ms/ams-v3-nodejs-ref) belgeleri ve kullanıma [örnekleri](https://github.com/Azure-Samples/media-services-v3-node-tutorials) node.js ile Media Services API'sine kullanmayı gösterir.

### <a name="python"></a>Python

Kullanım [Python SDK'sı](https://aka.ms/ams-v3-python-sdk).

Media Services gözden [Python başvurusu](https://aka.ms/ams-v3-python-ref) belgeleri.

### <a name="go"></a>Başlayın

Kullanım [Go SDK](https://aka.ms/ams-v3-go-sdk).

Media Services gözden [Git başvurusu](https://aka.ms/ams-v3-go-ref) belgeleri.

### <a name="ruby"></a>Ruby

Kullanım [Ruby SDK'sı](https://aka.ms/ams-v3-ruby-sdk).

## <a name="azure-media-services-explorer"></a>Azure Media Services Gezgini

[Azure Media Services Gezgini](https://github.com/Azure/Azure-Media-Services-Explorer) (AMSE), Media Services hakkında bilgi edinmek istiyorsanız Windows müşteriler için kullanılabilir bir araçtır. AMSE olduğu bir Winforms /C# karşıya yükleyin, indirin, kodlama, VOD akışı ve Media Services ile içerik Canlı uygulama. Media Services, herhangi bir kod yazmadan test etmek istediğiniz istemciler için AMSE aracıdır. AMSE kod, Media Services ile geliştirmek isteyen müşteriler için bir kaynak olarak sağlanır.

AMSE açık kaynak bir proje, destek, topluluk tarafından sağlanır (sorunları rapor https://github.com/Azure/Azure-Media-Services-Explorer/issues). Bu proje [Microsoft Open Source Code of Conduct](https://opensource.microsoft.com/codeofconduct/) (Microsoft Açık Kaynak Kullanım Kuralları) belgesinde listelenen kurallara uygundur. Daha fazla bilgi için [kullanım kuralları SSS](https://opensource.microsoft.com/codeofconduct/faq/) veya başvurun opencode@microsoft.com herhangi bir ek sorularınız ya da yorumlarınız ile.

## <a name="next-steps"></a>Sonraki adımlar

[Genel Bakış](media-services-overview.md)
