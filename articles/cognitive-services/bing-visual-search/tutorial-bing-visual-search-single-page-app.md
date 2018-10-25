---
title: 'Öğretici: Tek sayfalı Web uygulaması geliştirme - Bing Görsel Arama'
titleSuffix: Azure Cognitive Services
description: Bing Görsel Arama API'sinin tek sayfalı bir Web uygulamasında kullanılmasını gösterir.
services: cognitive-services
author: brapel
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-visual-search
ms.topic: tutorial
ms.date: 10/04/2017
ms.author: bking
ms.openlocfilehash: a63107b86e82bf0bcd89523588414b45e6e21d5a
ms.sourcegitcommit: 62759a225d8fe1872b60ab0441d1c7ac809f9102
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/19/2018
ms.locfileid: "49465206"
---
# <a name="tutorial-visual-search-single-page-web-app"></a>Öğretici: Tek sayfalı Web uygulamasında Görsel Arama

Bing Görsel Arama API'si, Bing.com/images sayfasında gösterilen resim ayrıntılarına benzer bir deneyim sağlar. Görsel Arama ile bir görüntü belirtebilir ve görüntü hakkında görsel olarak benzer görüntüler, alışveriş kaynakları ve görüntüyü içeren web sayfaları gibi içgörüler alabilirsiniz. 

Bu öğretici Bing Görüntü Arama öğreticisindeki tek sayfalı web uygulamasını (bkz. [Tek sayfalı Web uygulaması](../Bing-Image-Search/tutorial-bing-image-search-single-page-app.md)) genişletir. Bu öğreticinin kaynak kodunun ilk halinin tamamı için bkz: [Tek sayfalı Web uygulaması (kaynak kodu)](../Bing-Image-Search/tutorial-bing-image-search-single-page-app-source.md). Bu öğreticinin kaynak kodunun son hali için bkz: [Görsel Arama Tek sayfalı Web uygulaması](tutorial-bing-visual-search-single-page-app-source.md).

Ele alınan görevler şunlardır:

> [!div class="checklist"]
> * Bing Görsel Arama API'sini bir görüntü içgörü belirteciyle arama
> * Benzer resimler görüntüleme

## <a name="call-bing-visual-search"></a>Bing Görsel Aramayı çağırma
Bing Görüntü Arama öğreticisini düzenleyin ve aşağıdaki kodu 409. satırdaki betik öğesinin sonuna ekleyin. Bu kod Bing Görsel Arama API'sini çağırır ve sonuçları görüntüler.

``` javascript
function handleVisualSearchResponse(){
    if(this.status !== 200){
        console.log(this.responseText);
        return;
    }
    let json = this.responseText;
    let response = JSON.parse(json);
    for (let i =0; i < response.tags.length; i++) {
        let tag = response.tags[i];
        if (tag.displayName === '') {
            for (let j = 0; j < tag.actions.length; j++) {
                let action = tag.actions[j];
                if (action.actionType === 'VisualSearch') {
                    let html = '';
                    for (let k = 0; k < action.data.value.length; k++) {
                        let item = action.data.value[k];
                        let height = 120;
                        let width = Math.max(Math.round(height * item.thumbnail.width / item.thumbnail.height), 120);
                        html += "<img src='"+ item.thumbnailUrl + "&h=" + height + "&w=" + width + "' height=" + height + " width=" + width + "'>";
                    }
                    showDiv("insights", html);
                    window.scrollTo({top: document.getElementById("insights").getBoundingClientRect().top, behavior: "smooth"});
                }
            }
        }
    }
}

function bingVisualSearch(insightsToken){
    let visualSearchBaseURL = 'https://api.cognitive.microsoft.com/bing/v7.0/images/visualsearch',
        boundary = 'boundary_ABC123DEF456',
        startBoundary = '--' + boundary,
        endBoundary = '--' + boundary + '--',
        newLine = "\r\n",
        bodyHeader = 'Content-Disposition: form-data; name="knowledgeRequest"' + newLine + newLine;

    let postBody = {
        imageInfo: {
            imageInsightsToken: insightsToken
        }
    }
    let requestBody = startBoundary + newLine;
    requestBody += bodyHeader;
    requestBody += JSON.stringify(postBody) + newLine + newLine;
    requestBody += endBoundary + newLine;       
    
    let request = new XMLHttpRequest();

    try {
        request.open("POST", visualSearchBaseURL);
    } 
    catch (e) {
        renderErrorMessage("Error performing visual search: " + e.message);
    }
    request.setRequestHeader("Ocp-Apim-Subscription-Key", getSubscriptionKey());
    request.setRequestHeader("Content-Type", "multipart/form-data; boundary=" + boundary);
    request.addEventListener("load", handleVisualSearchResponse);
    request.send(requestBody);
}
```

## <a name="capture-insights-token"></a>İçgörü elde etme belirteci
Aşağıdaki kodu 151. satırdaki `searchItemsRenderer` nesnesine ekleyin. Bu kod, tıklandığında `bingVisualSearch` işlevini çağıran bir **benzerlerini bulma** bağlantısı ekler. İşlev imageInsightsToken değerini bağımsız değişken olarak alır.

``` javascript
html.push("<a href='javascript:bingVisualSearch(\"" + item.imageInsightsToken + "\");'>find similar</a><br>");
```

## <a name="display-similar-images"></a>Benzer resimler görüntüleme
Aşağıdaki HTML kodunu 601. satıra ekleyin. Bu işaretleme kodu Bing Görsel Arama API'si çağrısının sonuçlarını görüntülemek için kullanılan bir öğe ekler.

``` html
<div id="insights">
    <h3><a href="#" onclick="return toggleDisplay('_insights')">Similar</a></h3>
    <div id="_insights" style="display: none"></div>
</div>
```

Yeni JavaScript kodu ve HTML öğeleri yerleştirildikten sonra arama sonuçları bir **benzerlerini bul** bağlantısıyla görüntülenir. **Benzerleri** bölümünü seçtiğinize benzer görüntülerle doldurmak için bağlantıya tıklayın. Görüntüleri göstermek için **Benzerleri** bölümünü genişletmeniz gerekebilir.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Tek Sayfalı Web uygulaması kaynağında Görsel Arama](tutorial-bing-visual-search-single-page-app-source.md)
> [Bing Görsel Arama API'si başvurusu](https://aka.ms/bingvisualsearchreferencedoc)