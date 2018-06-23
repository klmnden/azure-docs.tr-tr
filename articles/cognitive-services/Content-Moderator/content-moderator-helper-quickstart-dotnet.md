---
title: Azure içerik denetleyici için SDK'sı .NET yardımcı yöntem | Microsoft Docs
description: .NET için Azure içerik denetleyici SDK'sını kullanarak bir içerik denetleyici istemci iade etme
services: cognitive-services
author: sanjeev3
manager: mikemcca
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: article
ms.date: 01/04/2018
ms.author: sajagtap
ms.openlocfilehash: 36f2124708731f78f34849d8210ed39ea8f59140
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35351467"
---
# <a name="helper-code-to-return-a-content-moderator-client"></a>Bir içerik denetleyici istemci döndürmek için yardımcı kodu

Bu makalede bilgiler sağlar ve yardımcı olması için kod örnekleri, aboneliğiniz için bir içerik denetleyici istemci oluşturmak için .NET için içerik denetleyici SDK ile çalışmaya başlamak.

Kitaplığı, bu bölümdeki diğer quickstarts tarafından kullanılır.

Bu makalede, Visual Studio ve C# ile bilginiz olduğunu varsayar.

> [!IMPORTANT]
> Bu sınıf kitaplığı yalnızca tanıtım amacıyla hedeflenen kodunu içerir.
> Üretimde kullanım için bu kodu uyum, depolama ve içerik denetleyici abonelik anahtarınızı kullanarak güvenli bir yöntem kullanın.

## <a name="sign-up-for-content-moderator-services"></a>İçerik denetleyici Hizmetleri için kaydolun

REST API veya SDK üzerinden içerik denetleyici Hizmetleri kullanabilmeniz için önce bir abonelik anahtarı gerekir.
Başvurmak [Hızlı Başlangıç](quick-start.md) anahtarı nasıl edinebilirsiniz öğrenin.

## <a name="create-your-visual-studio-project"></a>Visual Studio projesi oluşturma

1. Yeni bir **sınıf kitaplığı (.NET Framework)** projesi.

   Örnek kodda ı proje adı **ModeratorHelper**.

1. Bir başvuru ekleyin **System.Configuration** Framework derleme.

### <a name="install-required-packages"></a>Gerekli paketleri yükleme

Aşağıdaki NuGet paketlerini yükleyin:

- Microsoft.Azure.CognitiveServices.ContentModerator
- Microsoft.Rest.ClientRuntime
- Newtonsoft.Json

### <a name="create-the-content-moderator-client"></a>İçerik denetleyici istemcisi oluşturma

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
> Güncelleştirme **AzureRegion** ve **CMSubscriptionKey** bölge tanımlayıcısı ve abonelik anahtarı değerlerini içeren alanlar.

Artık, aboneliğiniz için bir içerik denetleyici istemci oluşturmanın hızlı bir yolu vardır.

## <a name="next-steps"></a>Sonraki adımlar

[Visual Studio çözümü indirme](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/ContentModerator) bu ve diğer içerik denetleyici hızlı başlangıç ipuçları için .NET için ve tümleştirme üzerinde başlayın.
