---
title: Azure Media Services’a genel bakış | Microsoft Docs
description: Bu konu Azure Media Services’e genel bir bakış sağlar.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 04/19/2019
ms.author: juliako
ms.openlocfilehash: efdc3f657712794325f8af6f1b0178915e7a8e6b
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61463506"
---
# <a name="azure-media-services-overview"></a>Azure Media Services’a genel bakış 

> [!div class="op_single_selector" title1="Select the version of Media Services that you are using:"]
> * [Sürüm 3](../latest/media-services-overview.md)
> * [Sürüm 2](media-services-overview.md)

> [!NOTE]
> Media Services v2’ye herhangi bir yeni özellik veya işlevsellik eklenmemektedir. <br/>En son sürüm olan [Media Services v3](https://docs.microsoft.com/azure/media-services/latest/)’ü inceleyin. Ayrıca bkz [geçiş kılavuzuna v2'den v3](../latest/migrate-from-v2-to-v3.md)

Microsoft Azure Media Services (AMS), geliştiricilerin ölçeklenebilir medya yönetimi ve teslimi uygulamaları oluşturmalarına olanak tanıyan genişletilebilir bir bulut tabanlı platformdur. Media Services, çeşitli istemcilere (TV, PC ve mobil cihazlar gibi) isteğe bağlı olarak veya canlı akış halinde teslim amacıyla video ve ses içeriklerini güvenli bir şekilde yüklemenizi, depolamanızı, kodlamanızı ve paketlemenizi sağlayan REST API'lerini temel alır.

Yalnızca Media Services’i kullanarak uçtan uca iş akışları oluşturabilirsiniz. Ayrıca, iş akışınızın bazı bölümleri için üçüncü taraf bileşenleri kullanmayı da tercih edebilirsiniz. Örneğin, bir üçüncü taraf kodlayıcısı kullanarak kodlayın. Daha sonra Media Services’i kullanarak yükleyin, koruyun, paketleyin ve teslim edin. İçeriğinizi canlı akışla aktarmayı veya isteğe bağlı içerik teslimini tercih edebilirsiniz. 

## <a name="prerequisites"></a>Önkoşullar

Azure Media Services’i kullanmaya başlamak için aşağıdakilerin bulunması gerekir:

* Bir Azure hesabı. Bir hesabınız yoksa, yalnızca birkaç dakika içinde ücretsiz bir deneme hesabı oluşturabilirsiniz. Ayrıntılar için bkz. [Azure Ücretsiz Deneme Sürümü](https://azure.microsoft.com).
* Bir Azure Media Services hesabı. Daha fazla bilgi için bkz. [Hesap Oluşturma](media-services-portal-create-account.md).
* (İsteğe bağlı) Geliştirme ortamı ayarlayın. Geliştirme ortamınız için .NET veya REST API’yi seçin. Daha fazla bilgi için bkz. [Ortam Ayarlama](media-services-dotnet-how-to-use.md).

    Ayrıca, [AMS API’ye programlama aracılığıyla nasıl bağlanılacağını öğrenin](media-services-use-aad-auth-to-access-ams-api.md).
* Standart veya premium akış uç noktası başlatılmış durumda.  Daha fazla bilgi için bkz. [Akış uç noktalarını yönetme](media-services-portal-manage-streaming-endpoints.md)

## <a name="sdks-and-tools"></a>SDK’lar ve araçlar

Media Services çözümleri oluşturmak için şunları kullanabilirsiniz:

* [Media Services REST API'si](https://docs.microsoft.com/rest/api/media/operations/azure-media-services-rest-api-reference)
* Kullanılabilir istemci SDK'larından biri:
    * [.NET için Azure Media Services SDK](https://github.com/Azure/azure-sdk-for-media-services),
    * [Java için Azure SDK](https://github.com/Azure/azure-sdk-for-java),
    * [Azure PHP SDK](https://github.com/Azure/azure-sdk-for-php),
    * [Node.js için Azure Media Services](https://github.com/michelle-becker/node-ams-sdk/blob/master/lib/request.js) (Bu, Microsoft dışı bir Node.js SDK sürümüdür. Bir topluluğun gözetimi altındadır ve şu anda AMS API'lerinin %100’ünü kapsamamaktadır).
* Mevcut araçlar:
    * [Azure portal](https://portal.azure.com/)
    * [Azure-Media-Services-Gezgini](https://github.com/Azure/Azure-Media-Services-Explorer) (Azure Media Services Gezgini (AMSE), Windows için bir Winforms/C# uygulamasıdır)

> [!NOTE]
> Java SDK'ın en son sürümünü almak ve Java ile geliştirmeye başlamak için bkz. [Media Services için Java istemcisi SDK’sı ile çalışmaya başlama](https://docs.microsoft.com/azure/media-services/media-services-java-how-to-use). <br/>
> Media Services için en yeni PHP SDK'sını indirmek üzere, [Packagist deposunda](https://packagist.org/packages/microsoft/windowsazure#v0.5.7) Microsoft/WindowAzure paketinin 0.5.7 sürümünü arayın.  

## <a name="code-samples"></a>Kod örnekleri

Birden fazla kod örneği **Azure Kod örnekleri** Galerisi: [Azure Media Services kod örnekleri](https://azure.microsoft.com/resources/samples/?service=media-services&sort=0).

## <a name="concepts"></a>Kavramlar

Azure Media Services kavramları hakkında bilgi edinmek için bkz. [Kavramlar](media-services-concepts.md).

## <a name="supported-scenarios-and-availability-of-media-services-across-data-centers"></a>Desteklenen senaryolar ve veri merkezleri arasında Media Services kullanılabilirliği

Ayrıntılı bilgi için bkz. [AMS senaryoları ve veri merkezleri arasında özelliklerin ve hizmetlerin kullanılabilirliği](scenarios-and-availability.md).

## <a name="service-level-agreement-sla"></a>Hizmet Düzeyi Sözleşmesi (SLA)

Daha fazla bilgi için bkz. [Microsoft Azure SLA](https://azure.microsoft.com/support/legal/sla/).

Veri merkezlerinde kullanılabilirlik hakkında bilgi için [Kullanılabilirlik](scenarios-and-availability.md#availability) bölümüne bakın.

## <a name="support"></a>Destek

[Azure Desteği](https://azure.microsoft.com/support/options/), Media Services de dahil olmak üzere Azure için destek seçenekleri sağlar.

## <a name="provide-feedback"></a>Geri bildirimde bulunma

[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]
