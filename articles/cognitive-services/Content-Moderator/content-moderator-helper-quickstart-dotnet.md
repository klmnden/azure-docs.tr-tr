---
title: "Hızlı Başlangıç: .NET yardımcı yöntemi için Content Moderator SDK'sı"
titlesuffix: Azure Cognitive Services
description: .NET için Azure Content Moderator SDK'sını kullanarak bir Content Moderator istemcisi döndürme
services: cognitive-services
author: sanjeev3
manager: cgronlun
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: quickstart
ms.date: 01/04/2018
ms.author: sajagtap
ms.openlocfilehash: 0c167de3af8aea6820962e7cc8d0c21f5ba61807
ms.sourcegitcommit: ad08b2db50d63c8f550575d2e7bb9a0852efb12f
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/26/2018
ms.locfileid: "47223279"
---
# <a name="quickstart-helper-code-to-return-a-content-moderator-client"></a>Hızlı Başlangıç: Content Moderator istemcisi döndürmek için yardımcı kodu

Bu makalede, aboneliğiniz için bir Content Moderator istemcisi oluşturmak üzere .NET için Content Moderator SDK'sını kullanmaya başlamanıza yardımcı olacak bilgi ve kod örnekleri sağlanmaktadır.

Kitaplık, bu bölümdeki diğer hızlı başlangıçlar tarafından kullanılmaktadır.

Bu makale, Visual Studio ve C# hakkında bilgi sahibi olduğunuzu varsayar.

> [!IMPORTANT]
> Bu sınıf kitaplığı yalnızca gösterime yönelik olarak tasarlanmış kod içerir.
> Bu kodu üretim aşamasında kullanmak üzere uyarlarsanız, Content Moderator abonelik anahtarınızı depolamak ve kullanmak için güvenli bir yöntem kullanın.

## <a name="sign-up-for-content-moderator-services"></a>Content Moderator hizmetleri için kaydolma

Content Moderator hizmetlerini REST API veya SDK aracılığıyla kullanabilmeniz için önce bir abonelik anahtarınız olması gerekir.
Anahtarı nasıl edinebileceğinizi öğrenmek için bkz [Hızlı Başlangıç](quick-start.md).

## <a name="create-your-visual-studio-project"></a>Visual Studio projenizi oluşturun

1. Yeni bir **Sınıf Kitaplığı (.NET Framework)** projesi oluşturun.

   Örnek kodda projeye **ModeratorHelper** adı verildi.

1. **System.Configuration** Framework bütünleştirilmiş koduna bir başvuru ekleyin.

### <a name="install-required-packages"></a>Gerekli paketleri yükleme

Aşağıdaki NuGet paketlerini yükleyin:

- Microsoft.Azure.CognitiveServices.ContentModerator
- Microsoft.Rest.ClientRuntime
- Newtonsoft.Json

### <a name="create-the-content-moderator-client"></a>Content Moderator istemcisini oluşturma

ModeratorHelper.cs dosyasının içeriğini aşağıdaki kodla değiştirin:

    using Microsoft.CognitiveServices.ContentModerator;

    namespace ModeratorHelper
    {
    /// <summary>
    /// Wraps the creation and configuration of a Content Moderator client.
    /// </summary>
    /// <remarks>This class library contains insecure code. If you adapt this 
    /// code for use in production, use a secure method of storing and using
    /// your Content Moderator subscription key.</remarks>
    public static class Clients
    {
        /// <summary>
        /// The region/location for your Content Moderator account, 
        /// for example, westus.
        /// </summary>
        private static readonly string AzureRegion = "myRegion";

        /// <summary>
        /// The base URL fragment for Content Moderator calls.
        /// </summary>
        private static readonly string AzureBaseURL =
            $"{AzureRegion}.api.cognitive.microsoft.com";

        /// <summary>
        /// Your Content Moderator subscription key.
        /// </summary>
        private static readonly string CMSubscriptionKey = "myKey";

        /// <summary>
        /// Returns a new Content Moderator client for your subscription.
        /// </summary>
        /// <returns>The new client.</returns>
        /// <remarks>The <see cref="ContentModeratorClient"/> is disposable.
        /// When you have finished using the client,
        /// you should dispose of it either directly or indirectly. </remarks>
        public static ContentModeratorClient NewClient()
        {
            // Create and initialize an instance of the Content Moderator API wrapper.
            ContentModeratorClient client = new ContentModeratorClient(new ApiKeyServiceClientCredentials(CMSubscriptionKey));

            client.BaseUrl = AzureBaseURL;
            return client;
        }
    }
    }


> [!IMPORTANT]
> **AzureRegion** ve **CMSubscriptionKey** alanlarını bölge tanımlayıcınız ve abonelik anahtarınız ile değiştirin.

Artık aboneliğiniz için hemen bir Content Moderator istemcisi oluşturabilecek durumdasınız.

## <a name="next-steps"></a>Sonraki adımlar

Bu ve diğer .NET için Content Moderator hızlı başlangıçları için [Visual Studio çözümünü indirin](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/ContentModerator) ve tümleştirmeniz üzerinde çalışmaya başlayın.
