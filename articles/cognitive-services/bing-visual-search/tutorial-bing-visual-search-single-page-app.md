---
title: Bing görüntü arama tek sayfa Web uygulaması | Microsoft Docs
titleSuffix: Bing Web Search APIs - Cognitive Services
description: Bir tek sayfalı Web uygulamasında Bing görüntü arama API kullanmayı gösterir.
services: cognitive-services
author: brapel
manager: ehansen
ms.service: cognitive-services
ms.component: bing-image-search
ms.topic: article
ms.date: 10/04/2017
ms.author: v-brapel
ms.openlocfilehash: 303d7745167d2ea25fda083ed99881ac4e0a7ec7
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35354137"
---
# <a name="tutorial-visual-search-single-page-web-app"></a>Öğretici: Visual arama tek sayfalı Web uygulaması

Bing Visual arama API Bing.com/images üzerinde gösterilen görüntü ayrıntıları benzer bir deneyim sağlar. Visual arama ile birlikte bir görüntü belirtin ve görsel olarak benzer görüntüleri, alışveriş kaynakları, görüntü ve daha fazlasını içeren Web sayfalarının görüntü ile ilgili Öngörüler ulaşırsınız. 

Bu öğretici tek sayfa web uygulaması Bing görüntü arama öğretici genişletir (bkz [tek sayfalı Web uygulaması](../Bing-Image-Search/tutorial-bing-image-search-single-page-app.md)). Tam kaynak kodu Bu öğretici başlatmak, bkz: [tek sayfalı Web uygulaması (kaynak kodu)](../Bing-Image-Search/tutorial-bing-image-search-single-page-app-source.md). Bu öğreticinin son kaynak kodunu bkz [Visual arama tek sayfalı Web uygulaması](tutorial-bing-visual-search-single-page-app-source.md).

Kapsanan görevleri şunlardır:

> [!div class="checklist"]
> * Bir görüntü Öngörüler belirteciyle Bing Visual arama API çağrısı
> * Benzer resimleri görüntüleme

## <a name="call-bing-visual-search"></a>Bing Visual arama çağırın
Bing görüntü arama öğretici düzenleyin ve komut dosyası öğesi satırı 409 sonuna aşağıdaki kodu ekleyin. Bu kod Bing Visual arama API çağrılarının ve sonuçları görüntüler.

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

## <a name="capture-insights-token"></a>Öngörüler belirteci yakalama
Aşağıdaki kodu ekleyin `searchItemsRenderer` 151 satırında nesne. Bu kod ekleyen bir **benzer bulmak** çağırır bağlantı `bingVisualSearch` tıklatıldığında işlev. İşlev bağımsız değişken olarak imageInsightsToken alır.

``` javascript
html.push("<a href='javascript:bingVisualSearch(\"" + item.imageInsightsToken + "\");'>find similar</a><br>");
```

## <a name="display-similar-images"></a>Benzer resimleri görüntüleme
Aşağıdaki HTML kod satırında 601 ekleyin. Bu biçimlendirme kodu Bing Visual arama API çağrısının sonuçlarını görüntülemek için kullanılan bir öğe ekler.

``` html
<div id="insights">
    <h3><a href="#" onclick="return toggleDisplay('_insights')">Similar</a></h3>
    <div id="_insights" style="display: none"></div>
</div>
```

Tüm yeni JavaScript kodu ve yerinde HTML öğeleri ile arama sonuçları ile görüntülenir bir **benzer bulmak** bağlantı. Doldurmak için bağlantıyı tıklatın **benzer** bölüm görüntülerle seçtiğiniz benzer. Genişletmeniz gerekebilir **benzer** görüntüleri göstermek için bölümü.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Görsel arama tek sayfalı Web Uygulama kaynağı](tutorial-bing-visual-search-single-page-app-source.md)
> [Bing Visual arama API Başvurusu](https://aka.ms/bingvisualsearchreferencedoc)