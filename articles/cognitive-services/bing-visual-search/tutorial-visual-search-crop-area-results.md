---
title: Bing Visual arama SDK kırpma alanı sonuçları Öğreticisi | Microsoft Docs
description: Bing Visual arama SDK görüntüleri URL'lerini karşıya yüklenen resmin kırpma benzeyen almak için nasıl kullanılacağını.
services: cognitive-services
author: mikedodaro
manager: ronakshah
ms.service: cognitive-services
ms.component: bing-visual-search
ms.topic: article
ms.date: 06/20/2018
ms.author: rosh
ms.openlocfilehash: 9bc3c180f108025f442343d8c5356982a83826a6
ms.sourcegitcommit: 0408c7d1b6dd7ffd376a2241936167cc95cfe10f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/26/2018
ms.locfileid: "36958413"
---
# <a name="tutorial-bing-visual-search-sdk-image-crop-area-and-results"></a>Öğretici: Bing Visual arama SDK görüntü kırpma alanını ve sonuçları
Görsel arama SDK'sı bir görüntü alanını seçin ve daha büyük görüntü kırpma alanına benzer resimlerini çevrimiçi bulmak için bir seçenek içerir.  Bu örnek, bir kişiden birkaç kişi içeren bir görüntü gösteren kırpma alanı belirtir.  Kod kırpma alanının ve büyük görüntü URL'sini gönderir ve Bing arama ve çevrimiçi bulunan benzer görüntülerinin URL'leri içeren sonuçları döndürür.

## <a name="prerequisites"></a>Önkoşullar

İhtiyacınız olacak [Visual Studio 2017](https://www.visualstudio.com/downloads/) Windows üzerinde çalışan bu kodu alın. (Ücretsiz Community sürümü çalışır.)

Bilmeniz gereken bir [Bilişsel Hizmetleri API hesap](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) Bing arama API'leri ile. [Ücretsiz deneme sürümü](https://azure.microsoft.com/try/cognitive-services/?api=bing-web-search-api) Bu Hızlı Başlangıç için yeterlidir. Ücretsiz deneme sürümünüzü etkinleştirmek ya da Ücretli abonelik anahtarı Azure panonuza kullanabilir sağlanan erişim anahtarı gerekir.

## <a name="application-dependencies"></a>Uygulama bağımlılıkları
Bing Web arama SDK'yı kullanarak bir konsol uygulaması ayarlamak için Visual Studio'daki Çözüm Gezgini'nden NuGet paketlerini Yönet seçeneğini göz atın. Microsoft.Azure.CognitiveServices.Search.VisualSearch paketini ekleyin.

Web ara SDK'sı NuGet paketi yükleniyor dahil olmak üzere bağımlılıkları yükler:

* Microsoft.Rest.ClientRuntime
* Microsoft.Rest.ClientRuntime.Azure
* Newtonsoft.Json

## <a name="image-and-crop-area"></a>Görüntü ve kırpma alanı
Aşağıdaki resimde Microsoft Kıdemli liderlik ekibindeki gösterir.  Visual arama SDK'yı kullanarak, biz kırpma alanı görüntünün karşıya yükleme ve diğer görüntüleri ve büyük görüntü seçili alanında varlığı içeren Web sayfaları bulabilirsiniz.  Bu durumda, bir kişinin varlıktır.

![Microsoft Kıdemli liderlik ekibindeki](./media/MS_SrLeaders.jpg)

## <a name="specify-the-crop-area-as-imageinfo-in-visualsearchrequest"></a>VisualSearchRequest ImageInfo olarak kırpma alanı belirtin
Bu örnek önceki görüntünün doğru koordinatları görüntünün tamamını yüzdeyle alt ve sol üst belirtir kırpma alanı kullanır.  Aşağıdaki kod oluşturur bir `ImageInfo` yükler ve kırpma alanı nesnesinden `ImageInfo` içine nesne bir `VisualSearchRequest`.  `ImageInfo` Nesnesi çevrimiçi görüntünün URL'si de içerir.

```
CropArea CropArea = new CropArea(top: (float)0.01, bottom: (float)0.30, left: (float)0.01, right: (float)0.20);
string imageURL = "https://docs.microsoft.com/en-us/azure/cognitive-services/bing-visual-search/media/ms_srleaders.jpg;
ImageInfo imageInfo = new ImageInfo(cropArea: CropArea, url: imageURL);

VisualSearchRequest visualSearchRequest = new VisualSearchRequest(imageInfo: imageInfo);
```
## <a name="search-for-images-similar-to-crop-area"></a>Görüntüleri kırpma alanı benzer arayın
`VisualSearchRequest` Görüntü ve URL'sini kırpma alanı bilgileri içermektedir.  `VisualSearchMethodAsync` Yöntemi sonuçları alır.
```
Console.WriteLine("\r\nSending visual search request with knowledgeRequest that contains URL and crop area");
var visualSearchResults = client.Images.VisualSearchMethodAsync(knowledgeRequest: visualSearchRequest).Result; 

```

## <a name="get-the-url-data-from-imagemoduleaction"></a>ImageModuleAction URL Veri Al
Görsel arama sonuçları `ImageTag` nesneleri.  Her etiket bir listesini içeren `ImageAction` nesneleri.  Her `ImageAction` içeren bir `Data` eylem türüne bağlı olan bir değer listesi alanı:

Aşağıdaki kod ile çeşitli türleri alabilirsiniz:
```
Console.WriteLine("\r\n" + "ActionType: " + i.ActionType + " -> WebSearchUrl: " + i.WebSearchUrl);

```
Tam uygulama döndürür:

* ActionType: PagesIncluding WebSearchURL:
* ActionType: WebSearchURL MoreSizes:
* ActionType: VisualSearch WebSearchURL:
* ActionType: ImageById WebSearchURL: 
* ActionType: RelatedSearches WebSearchURL:
* ActionType: Varlık -> WebSearchUrl: https://www.bing.com/cr?IG=E40D0E1A13404994ACB073504BC937A4&CID=03DCF882D7386A442137F49BD6596BEF&rd=1&h=BvvDoRtmZ35Xc_UZE4lZx6_eg7FHgcCkigU1D98NHQo&v=1&r=https%3a%2f%2fwww.bing.com%2fsearch%3fq%3dSatya%2bNadella&p=DevEx, 5380.1
* ActionType: TopicResults WebSearchUrl ->: https://www.bing.com/cr?IG=E40D0E1A13404994ACB073504BC937A4&CID=03DCF882D7386A442137F49BD6596BEF&rd=1&h=3QGtxPb3W9LemuHRxAlW4CW7XN4sPkUYCUynxAqI9zQ&v=1&r=https%3a%2f%2fwww.bing.com%2fdiscover%2fnadella%2bsatya&p=DevEx, 5382.1
* ActionType: ImageResults WebSearchUrl ->: https://www.bing.com/cr?IG=E40D0E1A13404994ACB073504BC937A4&CID=03DCF882D7386A442137F49BD6596BEF&rd=1&h=l-WNHO89Kkw69AmIGe2MhlUp6MxR6YsJszgOuM5sVLs&v=1&r=https%3a%2f%2fwww.bing.com%2fimages%2fsearch%3fq%3dSatya%2bNadella&p=DevEx, 5384.1

Yukarıdaki listede gösterildiği gibi `Entity` `ActionType` tanınabilir kişi, yer veya şey hakkındaki bilgileri döndürür bir Bing arama sorgusu içerir.  `TopicResults` Ve `ImageResults` türlerini içeren ilgili görüntüleri için sorgular. Bing arama sonuçları listesinde bağlantısını URL'lerinde.


## <a name="pagesincluding-actiontype-urls-of-images-found-by-visual-search"></a>PagesIncluding ActionType URL'leri görüntüsü Visual arama sonucu bulunamadı

Gerçek görüntü URL'leri alma okuyan bir cast gerektiren bir `ActionType` olarak `ImageModuleAction`, içeren bir `Data` değerleri listesi öğesiyle.  Her değer bir görüntü URL'dir.  Aşağıdaki atamalar `PagesIncluding` eylem türü `ImageModuleAction` ve değerleri okur.
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

Aşağıdaki kod, önceki örneklerin çalışır. Bir görüntü post isteğinin gövdesinde ikili gönderir, URL'leri cropArea nesne ve Bing yazdırır yanı sıra her ActionType için arama yapın. ActionType PagesIncluding ise, kod ImageObject öğeleri ImageObject verileri alır.  Verileri Web sayfalarındaki görüntülerin URL'leri değerler listesini içerir.  Kopyalayın ve sonuçta elde edilen Visual arama sonuçları göstermek için URL'leri tarayıcıya yapıştırın. Görüntüleri göstermek için tarayıcı ContentUrl öğeleri kopyalayıp yeniden açın.

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
                string imageURL = "https://docs.microsoft.com/en-us/azure/cognitive-services/bing-visual-search/media/ms_srleaders.jpg";
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
[Görsel arama yanıtı](https://docs.microsoft.com/en-us/azure/cognitive-services/bing-visual-search/overview#the-response)