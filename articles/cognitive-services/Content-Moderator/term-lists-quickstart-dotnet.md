---
title: Azure içerik denetleyicinin özel terim listeleriyle Orta | Microsoft Docs
description: .NET için Azure içerik denetleyici SDK'sını kullanarak özel terimiyle Orta nasıl listeler.
services: cognitive-services
author: sanjeev3
manager: mikemcca
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: article
ms.date: 01/11/2018
ms.author: sajagtap
ms.openlocfilehash: 6da72ad070d9c3a6be38e24626dff77b52fed852
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35351845"
---
# <a name="moderate-with-custom-term-lists-in-net"></a>.NET özel terim listeleriyle Orta

Varsayılan Genel Azure içeriği denetleyici koşullarını birçok içerik yönetimini ihtiyaçları için yeterli listesidir. Ancak, kuruluşunuz için belirli koşullar için ekran gerekebilir. Örneğin, daha fazla inceleme için etiket rakip adları isteyebilirsiniz. 

.NET için içerik denetleyici SDK metin denetleme API ile kullanmak için koşulları özel listesini oluşturmak için kullanabilirsiniz.

> [!NOTE]
> Maksimum sınırı yoktur **5 terim listeler** her listesine ile **10.000 koşulları aşmaması**.
>

Bu makalede bilgiler sağlanmaktadır ve yardımcı olması için kod örnekleri için .NET için içerik denetleyici SDK'sını kullanarak kullanmaya başlama:
- Bir liste oluşturur.
- Koşulları bir listesine ekleyin.
- Bir liste koşullarını karşı ekran koşulları.
- Koşulları listeden silin.
- Bir listeyi silin.
- Liste bilgilerini düzenleyin.
- Yeni bir tarama listeye yapılan değişiklikler dahil edilmesini dizini yenileyin.

Bu makalede, Visual Studio ve C# ile bilginiz olduğunu varsayar.

## <a name="sign-up-for-content-moderator-services"></a>İçerik denetleyici Hizmetleri için kaydolun

REST API veya SDK üzerinden içerik denetleyici Hizmetleri kullanabilmeniz için önce bir abonelik anahtarı gerekir.

İçerik denetleyici panosunda abonelik anahtarınızı bulabilirsiniz **ayarları** > **kimlik bilgileri** > **API**  >  **Deneme Ocp-Apim-Subscription-Key**. Daha fazla bilgi için bkz: [genel bakış](overview.md).

## <a name="create-your-visual-studio-project"></a>Visual Studio projesi oluşturma

1. Yeni bir ekleme **konsol uygulaması (.NET Framework)** çözümünüzü projeye.

1. Proje adı **TermLists**. Bu proje çözüme yönelik tek başlangıç projesi olarak seçin.

1. Bir başvuru ekleyin **ModeratorHelper** proje oluşturduğunuz derleme [içerik denetleyici istemci Yardımcısı quickstart](content-moderator-helper-quickstart-dotnet.md).

### <a name="install-required-packages"></a>Gerekli paketleri yükleme

TermLists proje için aşağıdaki NuGet paketlerini yükleyin:

- Microsoft.Azure.CognitiveServices.ContentModerator
- Microsoft.Rest.ClientRuntime
- Microsoft.Rest.ClientRuntime.Azure
- Newtonsoft.Json

### <a name="update-the-programs-using-statements"></a>Güncelleştirme program using deyimleri

Değiştirme program using deyimleri kullanıcının.

    using System;
    using System.Threading;
    using Microsoft.CognitiveServices.ContentModerator;
    using Microsoft.CognitiveServices.ContentModerator.Models;
    using ModeratorHelper;

### <a name="add-private-properties"></a>Özel Özellikler ekleme

Aşağıdaki özel özellikleri ad alanına TermLists, sınıf Program ekleyin.

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

## <a name="create-a-term-list"></a>Bir terim listesi oluşturma

Bir terim listesiyle oluşturduğunuz **ContentModeratorClient.ListManagementTermLists.Create**. İlk parametre olarak **oluşturma** "application/json" olması gereken bir MIME türü içeren bir dizedir. Daha fazla bilgi için bkz: [API Başvurusu](https://westus2.dev.cognitive.microsoft.com/docs/services/57cf755e3f9b070c105bd2c2/operations/57cf755e3f9b070868a1f67f). İkinci parametre bir **gövde** bir ad ve açıklama yeni terim listesi içeren nesne.

Aşağıdaki yöntem tanımını ad alanına TermLists, sınıf Program ekleyin.

> [!NOTE]
> İçerik denetleyici hizmeti anahtarınızı ikinci (RPS) hız sınırı başına bir istek varsa ve sınırı aşarsanız, SDK 429 hata kodunu içeren bir özel durum oluşturur. 
>
> Ücretsiz katmanı anahtarı bir RPS hızı sınırı vardır.

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

Terim listesi bilgilerle güncelleştirmek **ContentModeratorClient.ListManagementTermLists.Update**. İlk parametre olarak **güncelleştirme** terim listesi kimliğidir. İkinci parametre "application/json" olması gereken bir MIME türü değil. Daha fazla bilgi için bkz: [API Başvurusu](https://westus2.dev.cognitive.microsoft.com/docs/services/57cf755e3f9b070c105bd2c2/operations/57cf755e3f9b070868a1f685). Üçüncü parametre bir **gövde** yeni adı ve açıklamayı içeren nesne.

Aşağıdaki yöntem tanımını ad alanına TermLists, sınıf Program ekleyin.

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

## <a name="add-a-term-to-a-term-list"></a>Bir terim bir terim listesine ekle

Aşağıdaki yöntem tanımını ad alanına TermLists, sınıf Program ekleyin.

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

## <a name="get-all-terms-in-a-term-list"></a>Tüm koşulları terim listesini al

Aşağıdaki yöntem tanımını ad alanına TermLists, sınıf Program ekleyin.

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

## <a name="add-code-to-refresh-the-search-index"></a>Arama dizini yenilemek için kod ekleme

Bir terim listesine değişiklikleri yaptıktan sonra değişikliklerin ekran metni terim listesine bir sonraki kullanışınızda dahil edilmesi arama dizinini yenileyin. Bu, nasıl bir arama motoru (etkinse) masaüstü veya bir web arama motoru sürekli yeni dosyalar veya sayfaları içerecek şekilde dizinini yeniler için benzer.

Bir terim listesi arama dizini ile yenileme **ContentModeratorClient.ListManagementTermLists.RefreshIndexMethod**.

Aşağıdaki yöntem tanımını ad alanına TermLists, sınıf Program ekleyin.

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

## <a name="screen-text-using-a-term-list"></a>Terim listesini kullanarak ekran metni

Bir terim listesiyle kullanarak metin ekran **ContentModeratorClient.TextModeration.ScreenText**, aşağıdaki parametreleri alır.

- Terim listesi koşullarını dili.
- "Text/html", "text/xml", "metin/markdown" veya "metin/düz" bir MIME türü.
- Ekran metni.
- Bir Boole değeri. Bu alan kümesi'ne **true** düzeltme, filtreleme önce metni için.
- Bir Boole değeri. Bu alan kümesi'ne **true** kişisel bilgilerin (PII) metinde algılamak için.
- Terim liste kimliği

Daha fazla bilgi için bkz: [API Başvurusu](https://westus2.dev.cognitive.microsoft.com/docs/services/57cf753a3f9b070c105bd2c1/operations/57cf753a3f9b070868a1f66f).

**ScreenText** döndürür bir **ekran** sahip nesne, bir **koşulları** herhangi listeler özellik koşulları o içerik filtreleme algılanan denetleyici. İçerik denetleyici filtreleme sırasında tüm koşullar algılamadı gerçekleştiriyorsanız **koşulları** değerine **null**.

Aşağıdaki yöntem tanımını ad alanına TermLists, sınıf Program ekleyin.

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

Bir terim veya bir liste silme basittir. SDK, aşağıdaki görevleri gerçekleştirmek için kullanın:

- Bir terim silin. (**ContentModeratorClient.ListManagementTerm.DeleteTerm**)
- Bir listedeki tüm koşulları listesi silmeden silin. (**ContentModeratorClient.ListManagementTerm.DeleteAllTerms**)
- Bir listesi ve tüm içeriğini silin. (**ContentModeratorClient.ListManagementTermLists.Delete**)

### <a name="delete-a-term"></a>Bir terim Sil

Aşağıdaki yöntem tanımını ad alanına TermLists, sınıf Program ekleyin.

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

### <a name="delete-all-terms-in-a-term-list"></a>Bir terim listedeki tüm koşulları Sil

Aşağıdaki yöntem tanımını ad alanına TermLists, sınıf Program ekleyin.

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

### <a name="delete-a-term-list"></a>Terim listesini sil

Aşağıdaki yöntem tanımını ad alanına TermLists, sınıf Program ekleyin.

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

## <a name="putting-it-all-together"></a>Tüm bir araya getirme

Ekleme **ana** yöntem tanımını ad alanına TermLists, Program sınıfı. Son olarak, Program sınıfı ve TermLists ad kapatın.

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

## <a name="run-the-application-to-see-the-output"></a>Çıkışı görmek için uygulamayı çalıştırın

Aşağıdaki satırda, çıktı olacaktır, ancak verileri gösterebilir.

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

[Visual Studio çözümü indirme](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/ContentModerator) bu ve diğer içerik denetleyici hızlı başlangıç ipuçları için .NET için ve tümleştirme üzerinde başlayın.
