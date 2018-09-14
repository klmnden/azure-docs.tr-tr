---
title: Bing görsel arama SDK kırpma alanı sonuçları Öğreticisi | Microsoft Docs
description: Karşıya yüklenen görüntünün alanını kırpmak için benzer görüntülerin URL'lerini almak için Bing görsel arama SDK kullanma
services: cognitive-services
author: mikedodaro
manager: ronakshah
ms.service: cognitive-services
ms.component: bing-visual-search
ms.topic: article
ms.date: 06/20/2018
ms.author: rosh
ms.openlocfilehash: dd51ed7c710cc51a9fe0e63e55aa0d2c4ea24bee
ms.sourcegitcommit: e2ea404126bdd990570b4417794d63367a417856
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/14/2018
ms.locfileid: "45574498"
---
# <a name="tutorial-bing-visual-search-sdk-image-crop-area-and-results"></a>Öğretici: Görüntü kırpma alanı Bing görsel arama SDK'sı ve sonuçları
Görüntünün bir alan seçin ve daha büyük görüntü kırpma alanına benzer resimler çevrimiçi bulmak için bir seçenek Visual Search SDK'sı içerir.  Bu örnek, bir kişiden birden çok kişi içeren bir görüntü gösteren kırpma alanı belirtir.  Kod, kırpma alan ve büyük görüntünün URL'sini gönderir ve Bing arama URL'leri ve çevrimiçi bulunan benzer görüntülerin URL'lerini içeren sonuçları döndürür.

## <a name="prerequisites"></a>Önkoşullar

İhtiyacınız olacak [Visual Studio 2017](https://www.visualstudio.com/downloads/) Windows üzerinde çalışan bu kod alınamıyor. (Ücretsiz Community Edition’ı kullanabilirsiniz.)

Olmalıdır bir [Bilişsel hizmetler API hesabı](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) Bing arama API'leri ile. [Ücretsiz deneme sürümü](https://azure.microsoft.com/try/cognitive-services/?api=bing-web-search-api) Bu Hızlı Başlangıç için yeterlidir. Ücretsiz deneme sürümünüzü etkinleştirin ya da Ücretli abonelik anahtarı, Azure panosundan kullanabilir sağlanan erişim anahtarı gerekir.

## <a name="application-dependencies"></a>Uygulama bağımlılıkları
Bing Web araması SDK'sını kullanarak bir konsol uygulaması ayarlamak için NuGet paketlerini Yönet seçeneğini Visual Studio'da Çözüm Gezgini'nden göz atın. Microsoft.Azure.CognitiveServices.Search.VisualSearch paketini ekleyin.

Web arama SDK'sı NuGet paketi yükleniyor dahil olmak üzere, bağımlılıkları yükler:

* Microsoft.Rest.ClientRuntime
* Microsoft.Rest.ClientRuntime.Azure
* Newtonsoft.Json

## <a name="image-and-crop-area"></a>Görüntü ve kırpma alan
Aşağıdaki görüntüde, Microsoft üst düzey yönetim kadrosu ekibi gösterilmektedir.  Bir kırpma alanı görüntünün karşıya Visual Search SDK'sını kullanarak ve diğer resmi ve daha büyük resmi seçili alanında varlığı içeren bir Web sayfasını bulun.  Bu durumda, bir kişinin varlıktır.

![Microsoft üst düzey yönetim kadrosu ekibi](./media/MS_SrLeaders.jpg)

## <a name="specify-the-crop-area-as-imageinfo-in-visualsearchrequest"></a>Kırpma alanının ImageInfo VisualSearchRequest içinde olarak belirtin
Bu örnek, önceki görüntünün sol üst belirtir ve doğru koordinatları görüntünün tamamını bir yüzdesine göre daha düşük bir kırpma alanı kullanır.  Aşağıdaki kod oluşturur bir `ImageInfo` yükler ve kırpma alanı nesneden `ImageInfo` içine nesnesi bir `VisualSearchRequest`.  `ImageInfo` Nesne çevrimiçi görüntünün URL'sini de içerir.

```
CropArea CropArea = new CropArea(top: (float)0.01, bottom: (float)0.30, left: (float)0.01, right: (float)0.20);
string imageURL = "https://docs.microsoft.com/azure/cognitive-services/bing-visual-search/media/ms_srleaders.jpg;
ImageInfo imageInfo = new ImageInfo(cropArea: CropArea, url: imageURL);

VisualSearchRequest visualSearchRequest = new VisualSearchRequest(imageInfo: imageInfo);
```
## <a name="search-for-images-similar-to-crop-area"></a>Alan kırpmak için benzer resimler için arama yapın
`VisualSearchRequest` Görüntüsü ve URL'sini kırpma alanı bilgileri içermektedir.  `VisualSearchMethodAsync` Yöntemi sonuçlarını alır.
```
Console.WriteLine("\r\nSending visual search request with knowledgeRequest that contains URL and crop area");
var visualSearchResults = client.Images.VisualSearchMethodAsync(knowledgeRequest: visualSearchRequest).Result; 

```

## <a name="get-the-url-data-from-imagemoduleaction"></a>ImageModuleAction URL verilerini al
Görsel arama sonuçları `ImageTag` nesneleri.  Her etiket bir listesini içeren `ImageAction` nesneleri.  Her `ImageAction` içeren bir `Data` eylem türüne bağlı olan değerler listesini alanı:

Aşağıdaki kod ile çeşitli türleri alabilirsiniz:
```
Console.WriteLine("\r\n" + "ActionType: " + i.ActionType + " -> WebSearchUrl: " + i.WebSearchUrl);

```
Uygulamanın döndürür:

* ActionType: PagesIncluding WebSearchURL:
* ActionType: WebSearchURL MoreSizes:
* ActionType: VisualSearch WebSearchURL:
* ActionType: ImageById WebSearchURL: 
* ActionType: RelatedSearches WebSearchURL:
* ActionType: Varlık -> WebSearchUrl: https://www.bing.com/cr?IG=E40D0E1A13404994ACB073504BC937A4&CID=03DCF882D7386A442137F49BD6596BEF&rd=1&h=BvvDoRtmZ35Xc_UZE4lZx6_eg7FHgcCkigU1D98NHQo&v=1&r=https%3a%2f%2fwww.bing.com%2fsearch%3fq%3dSatya%2bNadella&p=DevEx, 5380.1
* ActionType: TopicResults WebSearchUrl ->: https://www.bing.com/cr?IG=E40D0E1A13404994ACB073504BC937A4&CID=03DCF882D7386A442137F49BD6596BEF&rd=1&h=3QGtxPb3W9LemuHRxAlW4CW7XN4sPkUYCUynxAqI9zQ&v=1&r=https%3a%2f%2fwww.bing.com%2fdiscover%2fnadella%2bsatya&p=DevEx, 5382.1
* ActionType: ImageResults WebSearchUrl ->: https://www.bing.com/cr?IG=E40D0E1A13404994ACB073504BC937A4&CID=03DCF882D7386A442137F49BD6596BEF&rd=1&h=l-WNHO89Kkw69AmIGe2MhlUp6MxR6YsJszgOuM5sVLs&v=1&r=https%3a%2f%2fwww.bing.com%2fimages%2fsearch%3fq%3dSatya%2bNadella&p=DevEx, 5384.1

Önceki listede gösterildiği `Entity` `ActionType` tanınabilir bir kişi, yer veya şey hakkında bilgi veren bir Bing arama sorgu içerir.  `TopicResults` Ve `ImageResults` sorguları ilgili görüntüleri için türler bulunur. Bing arama sonuçları listesinde bağlantısını URL'lerinde.


## <a name="pagesincluding-actiontype-urls-of-images-found-by-visual-search"></a>PagesIncluding ActionType URL'leri görüntülerin görsel arama sonucu bulundu

Gerçek görüntü URL'leri alma gerektirir okuyan bir cast bir `ActionType` olarak `ImageModuleAction`, içeren bir `Data` değerleri bir liste öğesi.  Her bir görüntünün URL'sini değerdir.  Aşağıdaki yayınları `PagesIncluding` eylem türü `ImageModuleAction` ve değerlerini okur.
```
    if (i.ActionType == "PagesIncluding")
    {
        foreach(ImageObject o in (i as ImageModuleAction).Data.Value)
        {
            Console.WriteLine("ContentURL: " + o.ContentUrl);
        }
    }
```

## <a name="complete-code"></a>Tam kod

Aşağıdaki kod, önceki örneklerde çalışır. Görüntü post isteğinin gövdesinde ikili gönderir, URL'leri cropArea nesne ve Bing yazdırır yanı sıra her ActionType için arama yapın. ActionType PagesIncluding ise, kod ImageObject verilerinde ImageObject öğeleri alır.  Veri Web sayfalarındaki görüntülerin URL'lerini olan değerlerin bir listesini içerir.  Sonuçta elde edilen görsel arama sonuçları göstermek için URL'leri tarayıcıya kopyalayıp yeniden açın. Görüntüleri göstermek için tarayıcı ContentUrl öğeleri kopyalayıp yeniden açın.

```
using System;
using System.IO;
using System.Linq;
using Microsoft.Azure.CognitiveServices.Search.VisualSearch;
using Microsoft.Azure.CognitiveServices.Search.VisualSearch.Models;
using Microsoft.Azure.CognitiveServices.Search.ImageSearch;

namespace VisualSearchFeatures
{
    class Program
    {
        static void Main(string[] args)
        {
            String subscriptionKey = "YOUR-ACCESS-KEY";

            VisualSearchImageBinaryWithCropArea(subscriptionKey);

            Console.WriteLine("Any key to quit...");
            Console.ReadKey();

        }

        public static void VisualSearchImageBinaryWithCropArea(string subscriptionKey)
        {
            var client = new VisualSearchAPI(new Microsoft.Azure.CognitiveServices.Search.VisualSearch.ApiKeyServiceClientCredentials(subscriptionKey));

            try
            {
                CropArea CropArea = new CropArea(top: (float)0.01, bottom: (float)0.30, left: (float)0.01, right: (float)0.20);
                
                // The ImageInfo struct specifies the crop area in the image and the URL of the larger image. 
                string imageURL = "https://docs.microsoft.com/azure/cognitive-services/bing-visual-search/media/ms_srleaders.jpg";
                ImageInfo imageInfo = new ImageInfo(cropArea: CropArea, url: imageURL);
                
                VisualSearchRequest visualSearchRequest = new VisualSearchRequest(imageInfo: imageInfo);

                var visualSearchResults = client.Images.VisualSearchMethodAsync(knowledgeRequest: visualSearchRequest).Result; 

                Console.WriteLine("\r\nVisual search request with image from URL and crop area combined in knowledgeRequest");

                if (visualSearchResults == null)
                {
                    Console.WriteLine("No visual search result data.");
                }
                else
                {
                    // List of tags
                    if (visualSearchResults.Tags.Count > 0)
                    {

                        foreach(ImageTag t in visualSearchResults.Tags)
                        {
                            foreach (ImageAction i in t.Actions)
                            { 
                                Console.WriteLine("\r\n" + "ActionType: " + i.ActionType + " -> WebSearchUrl: " + i.WebSearchUrl);

                                if (i.ActionType == "PagesIncluding")
                                {
                                    foreach(ImageObject o in (i as ImageModuleAction).Data.Value)
                                    {
                                        Console.WriteLine("ContentURL: " + o.ContentUrl);
                                    }
                                }
                            }

                        }

                    }
                    else
                    {
                        Console.WriteLine("Couldn't find image tags!");
                    }
                }
            }

            catch (Exception ex)
            {
                Console.WriteLine("Encountered exception. " + ex.Message);
            }
        }
    }
}

```
## <a name="next-steps"></a>Sonraki adımlar
[Görsel arama yanıt](https://docs.microsoft.com/azure/cognitive-services/bing-visual-search/overview#the-response)