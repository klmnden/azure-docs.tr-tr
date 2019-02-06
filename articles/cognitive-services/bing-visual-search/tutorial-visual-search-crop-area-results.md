---
title: 'Öğretici: Bing görsel arama SDK ile görüntü kırpma'
description: Bing görsel arama SDK, bir görüntüye belirli ares Öngörüler alma kullanın.
services: cognitive-services
author: mikedodaro
manager: cgronlun
ms.service: cognitive-services
ms.subservice: bing-visual-search
ms.topic: article
ms.date: 06/20/2018
ms.author: rosh
ms.openlocfilehash: 66012e22b4e3e512751711ea399b8add006fb717
ms.sourcegitcommit: 039263ff6271f318b471c4bf3dbc4b72659658ec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/06/2019
ms.locfileid: "55756402"
---
# <a name="tutorial-crop-an-image-with-the-bing-visual-search-sdk-for-c"></a>Öğretici: Bing görsel arama için SDK ile görüntü kırpmaC#

Bing görsel arama SDK benzer çevrimiçi görüntüleri bulma önce görüntü kırpma olanak tanır. Bu uygulama, bir görüntüden birçok kişi içeren tek bir kişi kırpar ve çevrimiçi bulunan benzer resimler içeren arama sonuçlarını döndürür.

Bu uygulama için tam kaynak kodunu ek hata işleme ve ek açıklamalar ile kullanılabilir [GitHub](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/blob/master/Tutorials/Bing-Visual-Search/BingVisualSearchCropImage.cs).

Bu öğreticide gösterilmiştir nasıl yapılır:

> [!div class="checklist"]
> * Bing görsel arama SDK'sını kullanarak bir istek gönderin
> * Bir alanı Bing görsel arama aramak için görüntü kırpma
> * Yanıta işlemek ve alma
> * Eylem öğeleri URL'lerini resposne içinde Bul

## <a name="prerequisites"></a>Önkoşullar

* [Visual Studio 2017](https://www.visualstudio.com/downloads/)’nin herhangi bir sürümü.
* Linux/MacOS kullanıyorsanız bu uygulama, [Mono](http://www.mono-project.com/) kullanılarak çalıştırılabilir.
- [NuGet Özel Arama](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Search.CustomSearch/1.2.0) paketinin yüklenmiş olması. 
    - Visual Studio'daki Çözüm Gezgini'nde projenize sağ tıklayıp menüden `Manage NuGet Packages` öğesini seçin. `Microsoft.Azure.CognitiveServices.Search.CustomSearch` paketini yükleyin. NuGet Özel Arama paketini yüklediğinizde aşağıdaki derlemeler de yüklenir:
        - Microsoft.Rest.ClientRuntime
        - Microsoft.Rest.ClientRuntime.Azure
        - Newtonsoft.Json

[!INCLUDE [cognitive-services-bing-image-search-signup-requirements](../../../includes/cognitive-services-bing-visual-search-signup-requirements.md)]

## <a name="specify-the-image-crop-area"></a>Görüntü kırpma alanını belirtin

Bu uygulama, bu görüntünün Microsoft üst düzey yönetim kadrosu ekip bir alan kırpar. Bu kırpma alanı görüntünün tamamını bir yüzdesi olarak temsil edilen, sol ve sağ alt koordinat kullanılarak tanımlanır.  

![Microsoft Üst Düzey Liderlik Ekibi](./media/MS_SrLeaders.jpg)

Bu görüntü oluşturarak kırpılır bir `ImageInfo` nesne kırpma alanından ve yükleme `ImageInfo` içine nesnesi bir `VisualSearchRequest`. `ImageInfo` Nesne görüntünün URL'sini de içerir.

```csharp
CropArea CropArea = new CropArea(top: (float)0.01, bottom: (float)0.30, left: (float)0.01, right: (float)0.20);
string imageURL = "https://docs.microsoft.com/azure/cognitive-services/bing-visual-search/media/ms_srleaders.jpg;
ImageInfo imageInfo = new ImageInfo(cropArea: CropArea, url: imageURL);

VisualSearchRequest visualSearchRequest = new VisualSearchRequest(imageInfo: imageInfo);
```

## <a name="search-for-images-similar-to-the-crop-area"></a>Görüntüleri kırpma alan benzer arayın

değişken `VisualSearchRequest` görüntü kırpma alan ve URL'sini hakkında bilgiler içerir. `VisualSearchMethodAsync()` yöntemi sonuçları alır.

```csharp
Console.WriteLine("\r\nSending visual search request with knowledgeRequest that contains URL and crop area");
var visualSearchResults = client.Images.VisualSearchMethodAsync(knowledgeRequest: visualSearchRequest).Result; 

```

## <a name="get-the-url-data-from-imagemoduleaction"></a>ImageModuleAction öğesinden URL verilerini alma

Bing görsel arama sonuçları `ImageTag` nesneleri.  Her etiket bir `ImageAction` nesneleri listesi içerir.  Her `ImageAction` içeren bir `Data` eylem türüne bağlı olan değerler listesini alanı.

Aşağıdaki kod ile çeşitli türleri yazdırabilirsiniz:

```csharp
Console.WriteLine("\r\n" + "ActionType: " + i.ActionType + " -> WebSearchUrl: " + i.WebSearchUrl);
```

Tam uygulama şunları döndürür:


|EylemTürü  |URL'si  | |
|---------|---------|---------|
|PagesIncluding WebSearchURL     |         |         
|WebSearchURL moreSizes     |         |         
|VisualSearch WebSearchURL    |         |         
|ImageById WebSearchURL     |         |         
|RelatedSearches WebSearchURL     |         |         
|Varlık WebSearchUrl ->     | https://www.bing.com/cr?IG=E40D0E1A13404994ACB073504BC937A4&CID=03DCF882D7386A442137F49BD6596BEF&rd=1&h=BvvDoRtmZ35Xc_UZE4lZx6_eg7FHgcCkigU1D98NHQo&v=1&r=https%3a%2f%2fwww.bing.com%2fsearch%3fq%3dSatya%2bNadella&p=DevEx,5380.1        |         
|TopicResults -> WebSearchUrl    |  https://www.bing.com/cr?IG=E40D0E1A13404994ACB073504BC937A4&CID=03DCF882D7386A442137F49BD6596BEF&rd=1&h=3QGtxPb3W9LemuHRxAlW4CW7XN4sPkUYCUynxAqI9zQ&v=1&r=https%3a%2f%2fwww.bing.com%2fdiscover%2fnadella%2bsatya&p=DevEx,5382.1        |         
|ImageResults -> WebSearchUrl    |  https://www.bing.com/cr?IG=E40D0E1A13404994ACB073504BC937A4&CID=03DCF882D7386A442137F49BD6596BEF&rd=1&h=l-WNHO89Kkw69AmIGe2MhlUp6MxR6YsJszgOuM5sVLs&v=1&r=https%3a%2f%2fwww.bing.com%2fimages%2fsearch%3fq%3dSatya%2bNadella&p=DevEx,5384.1        |         

Yukarıda gösterildiği gibi `Entity` ActionType tanınabilir bir kişi, yer veya şey hakkında bilgi döndüren Bing arama sorgusu içerir.  `TopicResults` ve `ImageResults` türleri, ilgili görüntülerin sorgularını içerir. Listedeki URL’ler, Bing arama sonuçlarına yönlendirir.


## <a name="get-urls-for-pagesincluding-actiontype-images"></a>Görüntü PagesIncluding ActionType için URL'leri alma

Gerçek görüntü URL’lerini almak için, bir `ActionType` türünü `ImageModuleAction` olarak okuyan bir tür dönüştürme gerekir. Bu tür ise değerler listesine sahip bir `Data` öğesi içerir.  Her değer, bir görüntünün URL’sidir.  Aşağıdaki, `PagesIncluding` eylem türünü `ImageModuleAction` türüne dönüştürür ve değerleri okur.

```csharp
    if (i.ActionType == "PagesIncluding")
    {
        foreach(ImageObject o in (i as ImageModuleAction).Data.Value)
        {
            Console.WriteLine("ContentURL: " + o.ContentUrl);
        }
    }
```

## <a name="next-steps"></a>Sonraki adımlar
> [!div class="nextstepaction"]
> [Tek sayfa uygulaması oluşturma](tutorial-bing-visual-search-single-page-app.md)

[Görsel Arama yanıtı](https://docs.microsoft.com/azure/cognitive-services/bing-visual-search/overview)
