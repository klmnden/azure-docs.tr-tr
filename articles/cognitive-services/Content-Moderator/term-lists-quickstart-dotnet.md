---
title: Azure Content Moderator, özel terim listeleri ile orta | Microsoft Docs
description: .NET için Azure Content Moderator SDK'sını kullanarak nasıl özel terimiyle Orta listeler.
services: cognitive-services
author: sanjeev3
manager: mikemcca
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: article
ms.date: 09/10/2018
ms.author: sajagtap
ms.openlocfilehash: d111f4eeea6a7cd630e550b0f57ad1617eb2bcd0
ms.sourcegitcommit: c29d7ef9065f960c3079660b139dd6a8348576ce
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/12/2018
ms.locfileid: "44719350"
---
# <a name="moderate-with-custom-term-lists-in-net"></a>. NET'te özel terim listeleri ile orta

Varsayılan Genel Azure Content Moderator koşullarını çoğu içerik denetimi ihtiyaçları için yeterli listesidir. Ancak, kuruluşunuz için belirli koşulların ekran gerekebilir. Örneğin, daha fazla inceleme için rakip etiketlerden isteyebilirsiniz. 

Kullanabileceğiniz [Content Moderator SDK'sı .NET için](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.ContentModerator/) özel metin denetimi API'si ile kullanmak için koşullarını listesini oluşturmak için.

> [!NOTE]
> Bir maksimum sınırı **5 terim listeleri** her listesine ile **10.000 koşulları aşmayacak**.
>

Bu makalede bilgiler sağlanmaktadır ve yardımcı olması için kod örnekleri için .NET için Content Moderator SDK'sını kullanarak kullanmaya başlayın:
- Bir liste oluşturur.
- Koşulları bir listesine ekleyin.
- Bir liste koşullarını karşı ekran koşulları.
- Koşulları listeden silin.
- Bir listeyi silin.
- Liste bilgileri düzenleyin.
- Listeye yeni bir tarama dahil edilmesini dizini yenileyin.

Bu makalede, zaten Visual Studio ve C# ile ilgili bilgi sahibi olduğunuz varsayılır.

## <a name="sign-up-for-content-moderator-services"></a>Content Moderator Hizmetleri için kaydolun

Content Moderator Hizmetleri REST API veya SDK aracılığıyla kullanabilmeniz için önce bir abonelik anahtarı gerekir.

Content Moderator panosunda abonelik anahtarınızı bulabilirsiniz **ayarları** > **kimlik bilgilerini** > **API**  >  **Deneme Ocp-Apim-Subscription-Key**. Daha fazla bilgi için [genel bakış](overview.md).

## <a name="create-your-visual-studio-project"></a>Visual Studio projenizi oluşturun

1. Yeni bir **konsol uygulaması (.NET Framework)** çözümünüze bir proje.

1. Projeyi adlandırın **TermLists**. Bu proje, çözüm için tek bir başlangıç projesi olarak seçin.

### <a name="install-required-packages"></a>Gerekli paketleri yükleme

TermLists projesi için aşağıdaki NuGet paketlerini yükleyin:

- Microsoft.Azure.CognitiveServices.ContentModerator
- Microsoft.Rest.ClientRuntime
- Microsoft.Rest.ClientRuntime.Azure
- Newtonsoft.Json

### <a name="update-the-programs-using-statements"></a>Deyimleri kullanarak program güncelleştirme

Değiştirme deyimleri kullanarak program.

    using Microsoft.Azure.CognitiveServices.ContentModerator;
    using Microsoft.CognitiveServices.ContentModerator;
    using Microsoft.CognitiveServices.ContentModerator.Models;
    using Newtonsoft.Json;
    using System;
    using System.Collections.Generic;
    using System.IO;
    using System.Threading;

### <a name="create-the-content-moderator-client"></a>Content Moderator istemcisi oluşturma

Aboneliğiniz için bir Content Moderator istemcisi oluşturmak için aşağıdaki kodu ekleyin.

> [!IMPORTANT]
> Güncelleştirme **AzureRegion** ve **CMSubscriptionKey** bölge tanımlayıcısı ve abonelik anahtarınızın değerlerini.


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
        private static readonly string AzureRegion = "YOUR API REGION";

        /// <summary>
        /// The base URL fragment for Content Moderator calls.
        /// </summary>
        private static readonly string AzureBaseURL =
            $"https://{AzureRegion}.api.cognitive.microsoft.com";

        /// <summary>
        /// Your Content Moderator subscription key.
        /// </summary>
        private static readonly string CMSubscriptionKey = "YOUR API KEY";

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

### <a name="add-private-properties"></a>Özel Özellikler ekleme

Aşağıdaki özel özellikleri ad TermLists, Program sınıfı ekleyin.

    /// <summary>
    /// The language of the terms in the term lists.
    /// </summary>
    private const string lang = "eng";

    /// <summary>
    /// The minimum amount of time, in milliseconds, to wait between calls
    /// to the Content Moderator APIs.
    /// </summary>
    private const int throttleRate = 3000;

    /// <summary>
    /// The number of minutes to delay after updating the search index before
    /// performing image match operations against a the list.
    /// </summary>
    private const double latencyDelay = 0.5;

## <a name="create-a-term-list"></a>Terim listesi oluşturma

Bir terim listesi ile oluşturduğunuz **ContentModeratorClient.ListManagementTermLists.Create**. İlk parametre olarak **Oluştur** "application/json" olması gereken bir MIME türü içeren bir dizedir. Daha fazla bilgi için [API Başvurusu](https://westus2.dev.cognitive.microsoft.com/docs/services/57cf755e3f9b070c105bd2c2/operations/57cf755e3f9b070868a1f67f). İkinci parametre bir **gövdesi** bir ad ve açıklama yeni terim listesi içeren nesne.

Aşağıdaki yöntem tanımını ad alanına TermLists, Program sınıfı ekleyin.

> [!NOTE]
> Content Moderator hizmet anahtarınız bir istek başına ikinci (RP'ler) hız sınırı vardır ve sınırını aşarsanız, SDK'sı, 429 hata koduna sahip bir özel durum oluşturur. 
>
> Ücretsiz katmanı anahtarı bir RPS oranı sınırı vardır.

    /// <summary>
    /// Creates a new term list.
    /// </summary>
    /// <param name="client">The Content Moderator client.</param>
    /// <returns>The term list ID.</returns>
    static string CreateTermList (ContentModeratorClient client)
    {
        Console.WriteLine("Creating term list.");

        Body body = new Body("Term list name", "Term list description");
        TermList list = client.ListManagementTermLists.Create("application/json", body);
        if (false == list.Id.HasValue)
        {
            throw new Exception("TermList.Id value missing.");
        }
        else
        {
            string list_id = list.Id.Value.ToString();
            Console.WriteLine("Term list created. ID: {0}.", list_id);
            Thread.Sleep(throttleRate);
            return list_id;
        }
    }

## <a name="update-term-list-name-and-description"></a>Güncelleştirme terim listesi adı ve açıklaması

Terim listesi bilgilerle güncelleştirme **ContentModeratorClient.ListManagementTermLists.Update**. İlk parametre olarak **güncelleştirme** terim listesi kimliğidir. Bir MIME türü "application/json" olması gereken ikinci parametredir. Daha fazla bilgi için [API Başvurusu](https://westus2.dev.cognitive.microsoft.com/docs/services/57cf755e3f9b070c105bd2c2/operations/57cf755e3f9b070868a1f685). Üçüncü parametre bir **gövdesi** yeni bir ad ve açıklama içeren bir nesne.

Aşağıdaki yöntem tanımını ad alanına TermLists, Program sınıfı ekleyin.

    /// <summary>
    /// Update the information for the indicated term list.
    /// </summary>
    /// <param name="client">The Content Moderator client.</param>
    /// <param name="list_id">The ID of the term list to update.</param>
    /// <param name="name">The new name for the term list.</param>
    /// <param name="description">The new description for the term list.</param>
    static void UpdateTermList (ContentModeratorClient client, string list_id, string name = null, string description = null)
    {
        Console.WriteLine("Updating information for term list with ID {0}.", list_id);
        Body body = new Body(name, description);
        client.ListManagementTermLists.Update(list_id, "application/json", body);
        Thread.Sleep(throttleRate);
    }

## <a name="add-a-term-to-a-term-list"></a>Bir terimi listesine bir terim ekleyin

Aşağıdaki yöntem tanımını ad alanına TermLists, Program sınıfı ekleyin.

    /// <summary>
    /// Add a term to the indicated term list.
    /// </summary>
    /// <param name="client">The Content Moderator client.</param>
    /// <param name="list_id">The ID of the term list to update.</param>
    /// <param name="term">The term to add to the term list.</param>
    static void AddTerm (ContentModeratorClient client, string list_id, string term)
    {
        Console.WriteLine("Adding term \"{0}\" to term list with ID {1}.", term, list_id);
        client.ListManagementTerm.AddTerm(list_id, term, lang);
        Thread.Sleep(throttleRate);
    }

## <a name="get-all-terms-in-a-term-list"></a>Tüm terimler terimi listesini alın

Aşağıdaki yöntem tanımını ad alanına TermLists, Program sınıfı ekleyin.

    /// <summary>
    /// Get all terms in the indicated term list.
    /// </summary>
    /// <param name="client">The Content Moderator client.</param>
    /// <param name="list_id">The ID of the term list from which to get all terms.</param>
    static void GetAllTerms(ContentModeratorClient client, string list_id)
    {
        Console.WriteLine("Getting terms in term list with ID {0}.", list_id);
        Terms terms = client.ListManagementTerm.GetAllTerms(list_id, lang);
        TermsData data = terms.Data;
        foreach (TermsInList term in data.Terms)
        {
            Console.WriteLine(term.Term);
        }
        Thread.Sleep(throttleRate);
    }

## <a name="add-code-to-refresh-the-search-index"></a>Arama dizini yenilemek için kod ekleyin

Bir terimi listesine değişiklikler yaptıktan sonra değişikliklerin terimi listeye ekrana metnin bir sonraki kullandığınızda dahil edilmesi arama dizinini yenileyin. Bu, nasıl bir arama motoru (etkinse) masaüstü veya bir web arama motoru sürekli olarak yeni dosyalar veya sayfalar eklemek için dizinini yeniler için benzer.

Terim listesi arama dizini ile yenileme **ContentModeratorClient.ListManagementTermLists.RefreshIndexMethod**.

Aşağıdaki yöntem tanımını ad alanına TermLists, Program sınıfı ekleyin.

    /// <summary>
    /// Refresh the search index for the indicated term list.
    /// </summary>
    /// <param name="client">The Content Moderator client.</param>
    /// <param name="list_id">The ID of the term list to refresh.</param>
    static void RefreshSearchIndex (ContentModeratorClient client, string list_id)
    {
        Console.WriteLine("Refreshing search index for term list with ID {0}.", list_id);
        client.ListManagementTermLists.RefreshIndexMethod(list_id, lang);
        Thread.Sleep((int)(latencyDelay * 60 * 1000));
    }

## <a name="screen-text-using-a-term-list"></a>Terim listesi kullanarak ekranı metni

Terim listesini kullanarak metin ekran **ContentModeratorClient.TextModeration.ScreenText**, şu parametreleri alır.

- Terim listesi koşullarını dili.
- "Text/html", "metin/xml", "metin/markdown" veya "text/plain" bir MIME türü.
- Ekrana metin.
- Bir Boole değeri. Bu alan kümesine **true** düzeltme bu filtreleme önce metin için.
- Bir Boole değeri. Bu alan kümesine **true** kişisel bilgilerin (PII) metinde algılamak için.
- Terim liste kimliği.

Daha fazla bilgi için [API Başvurusu](https://westus2.dev.cognitive.microsoft.com/docs/services/57cf753a3f9b070c105bd2c1/operations/57cf753a3f9b070868a1f66f).

**ScreenText** döndürür bir **ekran** olan nesne bir **koşulları** herhangi listeler özellik koşulları, Content Moderator, filtreleme algılandı. Content Moderator filtreleme sırasında herhangi bir hüküm algılamadı gerçekleştiriyorsanız **koşulları** özellik değerine sahip **null**.

Aşağıdaki yöntem tanımını ad alanına TermLists, Program sınıfı ekleyin.

    /// <summary>
    /// Screen the indicated text for terms in the indicated term list.
    /// </summary>
    /// <param name="client">The Content Moderator client.</param>
    /// <param name="list_id">The ID of the term list to use to screen the text.</param>
    /// <param name="text">The text to screen.</param>
    static void ScreenText (ContentModeratorClient client, string list_id, string text)
    {
        Console.WriteLine("Screening text: \"{0}\" using term list with ID {1}.", text, list_id);
        Screen screen = client.TextModeration.ScreenText(lang, "text/plain", text, false, false, list_id);
        if (null == screen.Terms)
        {
            Console.WriteLine("No terms from the term list were detected in the text.");
        }
        else
        {
            foreach (DetectedTerms term in screen.Terms)
            {
                Console.WriteLine(String.Format("Found term: \"{0}\" from list ID {1} at index {2}.", term.Term, term.ListId, term.Index));
            }
        }
        read.Sleep(throttleRate);
    }

## <a name="delete-terms-and-lists"></a>Hüküm ve listeleri sil

Bir terimi veya ayrılmış bir listeyi silmek oldukça basittir. SDK'sı, aşağıdaki görevleri gerçekleştirmek için kullanın:

- Bir terimi silin. (**ContentModeratorClient.ListManagementTerm.DeleteTerm**)
- Bir listedeki tüm koşulları, listenin silmeden silin. (**ContentModeratorClient.ListManagementTerm.DeleteAllTerms**)
- Bir liste ve tüm içeriğini silin. (**ContentModeratorClient.ListManagementTermLists.Delete**)

### <a name="delete-a-term"></a>Bir terimi Sil

Aşağıdaki yöntem tanımını ad alanına TermLists, Program sınıfı ekleyin.

    /// <summary>
    /// Delete a term from the indicated term list.
    /// </summary>
    /// <param name="client">The Content Moderator client.</param>
    /// <param name="list_id">The ID of the term list from which to delete the term.</param>
    /// <param name="term">The term to delete.</param>
    static void DeleteTerm (ContentModeratorClient client, string list_id, string term)
    {
        Console.WriteLine("Removed term \"{0}\" from term list with ID {1}.", term, list_id);
        client.ListManagementTerm.DeleteTerm(list_id, term, lang);
        Thread.Sleep(throttleRate);
    }

### <a name="delete-all-terms-in-a-term-list"></a>Bir terimi listesindeki tüm koşulları Sil

Aşağıdaki yöntem tanımını ad alanına TermLists, Program sınıfı ekleyin.

    /// <summary>
    /// Delete all terms from the indicated term list.
    /// </summary>
    /// <param name="client">The Content Moderator client.</param>
    /// <param name="list_id">The ID of the term list from which to delete all terms.</param>
    static void DeleteAllTerms (ContentModeratorClient client, string list_id)
    {
        Console.WriteLine("Removing all terms from term list with ID {0}.", list_id);
        client.ListManagementTerm.DeleteAllTerms(list_id, lang);
        Thread.Sleep(throttleRate);
    }

### <a name="delete-a-term-list"></a>Terim listesi Sil

Aşağıdaki yöntem tanımını ad alanına TermLists, Program sınıfı ekleyin.

    /// <summary>
    /// Delete the indicated term list.
    /// </summary>
    /// <param name="client">The Content Moderator client.</param>
    /// <param name="list_id">The ID of the term list to delete.</param>
    static void DeleteTermList (ContentModeratorClient client, string list_id)
    {
        Console.WriteLine("Deleting term list with ID {0}.", list_id);
        client.ListManagementTermLists.Delete(list_id);
        Thread.Sleep(throttleRate);
    }

## <a name="putting-it-all-together"></a>Hepsini birleştirme

Ekleme **ana** yöntem tanımını ad alanına TermLists, Program sınıfı. Son olarak, Program sınıfına ve TermLists ad alanı kapatın.

    static void Main(string[] args)
    {
        using (var client = Clients.NewClient())
        {
            string list_id = CreateTermList(client);

            UpdateTermList(client, list_id, "name", "description");
            AddTerm(client, list_id, "term1");
            AddTerm(client, list_id, "term2");

            GetAllTerms(client, list_id);

            // Always remember to refresh the search index of your list
            RefreshSearchIndex(client, list_id);

            string text = "This text contains the terms \"term1\" and \"term2\".";
            ScreenText(client, list_id, text);

            DeleteTerm(client, list_id, "term1");

            // Always remember to refresh the search index of your list
            RefreshSearchIndex(client, list_id);

            text = "This text contains the terms \"term1\" and \"term2\".";
            ScreenText(client, list_id, text);

            DeleteAllTerms(client, list_id);
            DeleteTermList(client, list_id);

            Console.WriteLine("Press ENTER to close the application.");
            Console.ReadLine();
        }
    }

## <a name="run-the-application-to-see-the-output"></a>Çıktıyı görmek için uygulamayı çalıştırma

Çıkışınız aşağıdaki satırda olacaktır, ancak Veri farklılık gösterebilir.

    Creating term list.
    Term list created. ID: 252.
    Updating information for term list with ID 252.
    
    Adding term "term1" to term list with ID 252.
    Adding term "term2" to term list with ID 252.
    
    Getting terms in term list with ID 252.
    term1
    term2
    
    Refreshing search index for term list with ID 252.
    
    Screening text: "This text contains the terms "term1" and "term2"." using term list with ID 252.
    Found term: "term1" from list ID 252 at index 32.
    Found term: "term2" from list ID 252 at index 46.
    
    Removed term "term1" from term list with ID 252.
    
    Refreshing search index for term list with ID 252.
    
    Screening text: "This text contains the terms "term1" and "term2"." using term list with ID 252.
    Found term: "term2" from list ID 252 at index 46.
    
    Removing all terms from term list with ID 252.
    Deleting term list with ID 252.
    Press ENTER to close the application.
    
## <a name="next-steps"></a>Sonraki adımlar

Alma [Content Moderator .NET SDK'sı](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.ContentModerator/) ve [Visual Studio çözümü](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/ContentModerator) bu ve diğer Content Moderator hızlı başlangıçlar, .NET için ve tümleştirmenizi üzerinde çalışmaya başlayın.
