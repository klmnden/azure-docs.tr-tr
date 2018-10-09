---
title: 'Öğretici: Görüntü kırpma alanı ve sonuçlar - Bing Görsel Arama'
description: Karşıya yüklenen görüntünün kırpma alanına benzer görüntülerin URL’lerini almak için Bing Görsel Arama SDK’sını kullanma.
services: cognitive-services
author: mikedodaro
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-visual-search
ms.topic: tutorial
ms.date: 06/20/2018
ms.author: rosh
ms.openlocfilehash: 66e17c00da898e575bb858dbe16a35d1c44a2780
ms.sourcegitcommit: ad08b2db50d63c8f550575d2e7bb9a0852efb12f
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/26/2018
ms.locfileid: "47226919"
---
# <a name="tutorial-bing-visual-search-sdk-image-crop-area-and-results"></a>Öğretici: Bing Görsel Arama SDK’sı görüntü kırpma alanı ve sonuçları
Görsel Arama SDK’sı, görüntünün alanını seçip daha büyük görüntünün kırpma alanına benzer görüntüleri çevrimiçi bulma seçeneği içerir.  Bu örnek, birkaç kişinin bulunduğu bir görüntüden bir kişiyi gösteren kırpma alanını belirtir.  Kod, kırpma alanını ve büyük görüntünün URL’sini gönderir ve Bing Arama URL’leri ile çevrimiçi bulunan benzer görüntülerin URL’lerini içeren sonuçları döndürür.

## <a name="prerequisites"></a>Ön koşullar

Bu kodun Windows üzerinde çalıştırılması için [Visual Studio 2017](https://www.visualstudio.com/downloads/) gerekir. (Ücretsiz Community Edition’ı kullanabilirsiniz.)

Bing Arama API'lerine sahip bir [Bilişsel Hizmetler API hesabınız](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) olması gerekir. [Ücretsiz deneme](https://azure.microsoft.com/try/cognitive-services/?api=bing-web-search-api) bu hızlı başlangıç için yeterlidir. Ücretsiz denemenizi etkinleştirdiğinizde verilen erişim anahtarınız olması veya Azure panonuzdan ücretli bir abonelik anahtarı kullanmanız gerekir.

## <a name="application-dependencies"></a>Uygulama bağımlılıkları
Bing Web Araması SDK'sını kullanarak bir konsol uygulaması kurmak için Visual Studio’da Çözüm Gezgini'nde NuGet Paketlerini Yönet seçeneğine gidin. Microsoft.Azure.CognitiveServices.Search.VisualSearch paketini ekleyin.

NuGet Web Araması SDK'sı paketini yüklemek aşağıdakilerin dahil olduğu bağımlılıkları da yükler:

* Microsoft.Rest.ClientRuntime
* Microsoft.Rest.ClientRuntime.Azure
* Newtonsoft.Json

## <a name="image-and-crop-area"></a>Görüntü ve kırpma alanı
Aşağıdaki görüntüde, Microsoft üst düzey liderlik ekibi gösterilmektedir.  Görsel Arama SDK’sını kullanarak, görüntünün kırpma alanını karşıya yükleyip büyük görüntünün seçili alanında varlığı içeren diğer görüntü ve Web sayfalarını buluruz.  Bu durumda varlık bir kişidir.

![Microsoft Üst Düzey Liderlik Ekibi](./media/MS_SrLeaders.jpg)

## <a name="specify-the-crop-area-as-imageinfo-in-visualsearchrequest"></a>Kırpma alanını VisualSearchRequest öğesinde ImageInfo olarak belirtin
Bu örnekte, görüntünün tamamının yüzdesine göre sol üst ve sağ alt koordinatlarını belirten önceki görüntünün kırpma alanı kullanılır.  Aşağıdaki kod, kırpma alanından bir `ImageInfo` nesnesi oluşturur ve `ImageInfo` nesnesini `VisualSearchRequest` öğesine yükler.  `ImageInfo` nesnesi, görüntünün URL'sini de çevrimiçi olarak içerir.

```
CropArea CropArea = new CropArea(top: (float)0.01, bottom: (float)0.30, left: (float)0.01, right: (float)0.20);
string imageURL = "https://docs.microsoft.com/azure/cognitive-services/bing-visual-search/media/ms_srleaders.jpg;
ImageInfo imageInfo = new ImageInfo(cropArea: CropArea, url: imageURL);

VisualSearchRequest visualSearchRequest = new VisualSearchRequest(imageInfo: imageInfo);
```
## <a name="search-for-images-similar-to-crop-area"></a>Kırpma alanına benzer görüntüleri arama
`VisualSearchRequest`, görüntünün ve URL’sinin kırpma alanı bilgilerini içerir.  `VisualSearchMethodAsync` yöntemi sonuçları alır.
```
Console.WriteLine("\r\nSending visual search request with knowledgeRequest that contains URL and crop area");
var visualSearchResults = client.Images.VisualSearchMethodAsync(knowledgeRequest: visualSearchRequest).Result; 

```

## <a name="get-the-url-data-from-imagemoduleaction"></a>ImageModuleAction öğesinden URL verilerini alma
Görsel Arama sonuçları `ImageTag` nesneleridir.  Her etiket bir `ImageAction` nesneleri listesi içerir.  Her `ImageAction`, eylem türüne bağımlı olan değerlerin bir listesi olan `Data` alanını içerir:

Aşağıdaki kod ile çeşitli türleri alabilirsiniz:
```
Console.WriteLine("\r\n" + "ActionType: " + i.ActionType + " -> WebSearchUrl: " + i.WebSearchUrl);

```
Tam uygulama şunları döndürür:

* ActionType: PagesIncluding WebSearchURL:
* ActionType: MoreSizes WebSearchURL:
* ActionType: VisualSearch WebSearchURL:
* ActionType: ImageById WebSearchURL: 
* ActionType: RelatedSearches  WebSearchURL:
* ActionType: Entity -> WebSearchUrl: https://www.bing.com/cr?IG=E40D0E1A13404994ACB073504BC937A4&CID=03DCF882D7386A442137F49BD6596BEF&rd=1&h=BvvDoRtmZ35Xc_UZE4lZx6_eg7FHgcCkigU1D98NHQo&v=1&r=https%3a%2f%2fwww.bing.com%2fsearch%3fq%3dSatya%2bNadella&p=DevEx,5380.1
* ActionType: TopicResults -> WebSearchUrl: https://www.bing.com/cr?IG=E40D0E1A13404994ACB073504BC937A4&CID=03DCF882D7386A442137F49BD6596BEF&rd=1&h=3QGtxPb3W9LemuHRxAlW4CW7XN4sPkUYCUynxAqI9zQ&v=1&r=https%3a%2f%2fwww.bing.com%2fdiscover%2fnadella%2bsatya&p=DevEx,5382.1
* ActionType: ImageResults -> WebSearchUrl: https://www.bing.com/cr?IG=E40D0E1A13404994ACB073504BC937A4&CID=03DCF882D7386A442137F49BD6596BEF&rd=1&h=l-WNHO89Kkw69AmIGe2MhlUp6MxR6YsJszgOuM5sVLs&v=1&r=https%3a%2f%2fwww.bing.com%2fimages%2fsearch%3fq%3dSatya%2bNadella&p=DevEx,5384.1

Önceki listede gösterildiği gibi `Entity` `ActionType`, tanınabilir bir kişi, yer veya şey hakkında bilgi döndüren bir Bing Arama sorgusu içerir.  `TopicResults` ve `ImageResults` türleri, ilgili görüntülerin sorgularını içerir. Listedeki URL’ler Bing arama sonuçlarına yönlendirir.


## <a name="pagesincluding-actiontype-urls-of-images-found-by-visual-search"></a>Görsel Arama tarafından bulunan görüntülerin PagesIncluding ActionType URL’leri

Gerçek görüntü URL’lerini almak için, bir `ActionType` türünü `ImageModuleAction` olarak okuyan bir tür dönüştürme gerekir. Bu tür ise değerler listesine sahip bir `Data` öğesi içerir.  Her değer, bir görüntünün URL’sidir.  Aşağıdaki, `PagesIncluding` eylem türünü `ImageModuleAction` türüne dönüştürür ve değerleri okur.
```
    if (i.ActionType == "PagesIncluding")
    {
        foreach(ImageObject o in (i as ImageModuleAction).Data.Value)
        {
            Console.WriteLine("ContentURL: " + o.ContentUrl);
        }
    }
```

## <a name="complete-code"></a>Kodu tamamla

Aşağıdaki kod önceki örnekleri çalıştırır. Kod, cropArea nesnesiyle birlikte POST isteğinin gövdesinde bir görüntü ikili dosyası gönderir ve her ActionType için Bing arama URL’lerini yazdırır. ActionType PagesIncluding ise, kod ImageObject öğelerini ImageObject Verilerinde alır.  Veriler, Web sayfalarındaki görüntülerin URL'leri olan değerlerin bir listesini içerir.  Sonuçları göstermek için elde edilen Görsel Arama URL’lerini kopyalayıp tarayıcıya yapıştırın. Görüntüleri göstermek için ContentUrl öğelerini kopyalayıp tarayıcıya yapıştırın.

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
[Görsel Arama yanıtı](https://docs.microsoft.com/azure/cognitive-services/bing-visual-search/overview#the-response)