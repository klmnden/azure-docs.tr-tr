---
title: Azure Media Services’a genel bakış | Microsoft Docs
description: Bu konu Azure Media Services’e genel bir bakış sağlar.
services: media-services
documentationcenter: ''
author: Juliako
manager: cfowler
editor: ''
ms.assetid: 7a5e9723-c379-446b-b4d6-d0e41bd7d31f
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 10/24/2017
ms.author: juliako;anilmur
ms.openlocfilehash: d79a8937c0190246218153e42ead74ada619b121
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/10/2018
---
# <a name="azure-media-services-overview"></a>Azure Media Services’a genel bakış 

> [!div class="op_single_selector" title1="Select the version of Media Services that you are using:"]
> * [Sürüm 2 - Genel Kullanım](media-services-overview.md)
> * [Sürüm 3 - Önizleme](../latest/media-services-overview.md)

Microsoft Azure Media Services (AMS), geliştiricilerin ölçeklenebilir medya yönetimi ve teslimi uygulamaları oluşturmalarına olanak tanıyan genişletilebilir bir bulut tabanlı platformdur. Media Services, çeşitli istemcilere (TV, PC ve mobil cihazlar gibi) isteğe bağlı olarak veya canlı akış halinde teslim amacıyla video ve ses içeriklerini güvenli bir şekilde yüklemenizi, depolamanızı, kodlamanızı ve paketlemenizi sağlayan REST API'lerini temel alır.

Yalnızca Media Services’i kullanarak uçtan uca iş akışları oluşturabilirsiniz. Ayrıca, iş akışınızın bazı bölümleri için üçüncü taraf bileşenleri kullanmayı da tercih edebilirsiniz. Örneğin, bir üçüncü taraf kodlayıcısı kullanarak kodlayın. Daha sonra Media Services’i kullanarak yükleyin, koruyun, paketleyin ve teslim edin.

İçeriğinizi canlı akışla aktarmayı veya isteğe bağlı içerik teslimini tercih edebilirsiniz. Konu, aynı zamanda ilgili diğer konulara bağlantılar içerir.

## <a name="media-services-learning-paths"></a>Media Services’i öğrenme yolları
AMS öğrenme yollarını burada görebilirsiniz:

* [AMS Canlı Akış İş Akışı](https://azure.microsoft.com/documentation/learning-paths/media-services-streaming-live/)
* [AMS İsteğe Bağlı Akış İş Akışı](https://azure.microsoft.com/documentation/learning-paths/media-services-streaming-on-demand/)

## <a name="prerequisites"></a>Ön koşullar

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
    * [Azure Portal](https://portal.azure.com/)
    * [Azure-Media-Services-Gezgini](https://github.com/Azure/Azure-Media-Services-Explorer) (Azure Media Services Gezgini (AMSE), Windows için bir Winforms/C# uygulamasıdır)

> [!NOTE]
> Java SDK'ın en son sürümünü almak ve Java ile geliştirmeye başlamak için bkz. [Media Services için Java istemcisi SDK’sı ile çalışmaya başlama](https://docs.microsoft.com/azure/media-services/media-services-java-how-to-use). <br/>
> Media Services için en yeni PHP SDK'sını indirmek üzere, [Packagist deposunda](https://packagist.org/packages/microsoft/windowsazure#v0.5.7) Microsoft/WindowAzure paketinin 0.5.7 sürümünü arayın.  

## <a name="code-samples"></a>Kod örnekleri

Birden fazla kod örneği için **Azure Kod Örnekleri** galerisine bakın: [Azure Media Services kod örnekleri](https://azure.microsoft.com/resources/samples/?service=media-services&sort=0).

## <a name="concepts"></a>Kavramlar

Azure Media Services kavramları hakkında bilgi edinmek için bkz. [Kavramlar](media-services-concepts.md).

## <a name="supported-scenarios-and-availability-of-media-services-across-data-centers"></a>Desteklenen senaryolar ve veri merkezleri arasında Media Services kullanılabilirliği

Ayrıntılı bilgi için bkz. [AMS senaryoları ve veri merkezleri arasında özelliklerin ve hizmetlerin kullanılabilirliği](scenarios-and-availability.md).

## <a name="service-level-agreement-sla"></a>Hizmet Düzeyi Sözleşmesi (SLA)

* Media Services Kodlama için, REST API işlemlerinin %99,9 kullanılabilirliğini garanti ediyoruz.
* Akış için, standart veya premium akış uç noktası satın alındığında mevcut medya içeriğinin %99,9 kullanılabilirliği garantisi ile isteklere başarıyla hizmet vereceğiz.
* Canlı Kanallar için, çalışan Kanalların en az %99,9 oranda dış bağlantıya sahip olacağını garanti ediyoruz.
* Content Protection için, önemli istekleri en az %99,9 oranda başarıyla karşılayacağımızı garanti ediyoruz.
* Dizin Oluşturucu için, bir Kodlamaya Ayrılan Birim ile işlenen Dizin Oluşturucu Görevi isteklerine %99,9 oranda başarıyla hizmet vereceğiz.

Daha fazla bilgi için bkz. [Microsoft Azure SLA](https://azure.microsoft.com/support/legal/sla/).

Veri merkezlerinde kullanılabilirlik hakkında bilgi için [Kullanılabilirlik](scenarios-and-availability.md#availability) bölümüne bakın.

## <a name="support"></a>Destek

[Azure Desteği](https://azure.microsoft.com/support/options/), Media Services de dahil olmak üzere Azure için destek seçenekleri sağlar.

## <a name="provide-feedback"></a>Geri bildirimde bulunma

[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]
