---
title: Bilgisayar görme API JavaScript Öğreticisi | Microsoft Docs
description: Microsoft Bilişsel Hizmetleri'nde bilgisayar görme API'sini kullanan basit bir JavaScript uygulaması keşfedin. OCR gerçekleştirmek, küçük resim oluşturma ve görüntüdeki visual özellikleriyle çalışma.
services: cognitive-services
author: KellyDF
manager: corncar
ms.service: cognitive-services
ms.component: computer-vision
ms.topic: article
ms.date: 09/19/2017
ms.author: kefre
ms.openlocfilehash: 89bdc0524e07c1cb6a1473e0a52791fe20271e06
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35351911"
---
# <a name="computer-vision-api-javascript-tutorial"></a>Bilgisayar görme API JavaScript Öğreticisi

Bu öğretici, Microsoft Bilişsel hizmetler bilgisayar görme REST API özelliklerini gösterir.

Optik karakter tanıma gerçekleştirmek, akıllı kırpılmış küçük resimleri, oluşturun artı algılamak, etiket, kategorilere ayırmak ve yüz, görüntüyü içeren visual özellikleri açıklamak bilgisayar görme REST API'si kullanan bir JavaScript uygulaması keşfedin. Bu örnek, çözümleme veya işlem için bir resim URL'si gönderme olanak tanır. Bu açık kaynak örnek şablon olarak JavaScript'te kendi uygulamanızı oluşturmak için bilgisayar görme REST API kullanmak için kullanabilirsiniz.

JavaScript form uygulaması zaten yazıldı, ancak hiçbir bilgisayar görme işlevselliğine sahiptir. Bu öğreticide uygulamanın işlevselliğini tamamlamak için bilgisayarı görme REST API için belirli kodu ekleyin.

## <a name="prerequisites"></a>Önkoşullar

### <a name="platform-requirements"></a>Platform gereksinimleri

Bu öğreticide, basit bir metin düzenleyicisi kullanarak geliştirilmiştir.

### <a name="subscribe-to-computer-vision-api-and-get-a-subscription-key"></a>Bilgisayar görme API'sine abone olma ve aboneliği anahtarı alma 

Örnek oluşturmadan önce bilgisayarı görme API'sine Microsoft Bilişsel hizmetler parçası olan abone olmalısınız. Abonelik ve anahtar yönetimi Ayrıntılar için bkz: [abonelikleri](https://azure.microsoft.com/try/cognitive-services/). Bu öğreticide kullanmak üzere birincil ve ikincil anahtarlar geçerlidir. 

## <a name="download-the-tutorial-project"></a>Eğitmen projenizi indirin

Kopya [Bilişsel hizmetler JavaScript bilgisayar görme öğretici](https://github.com/Azure-Samples/cognitive-services-javascript-computer-vision-tutorial), .zip dosyasını indirin ve boş bir dizine ayıklayın.

Eklenen tüm Eğitmen kodu ile tamamlandı öğretici kullanılacak tercih ediyorsanız, dosyalarında kullanabilirsiniz **tamamlandı** klasör.

## <a name="add-the-tutorial-code"></a>Eğitmen kodu ekleyin

JavaScript uygulama altı .html dosyaları her bir özellik için bir tane ile ayarlanır. Her dosya bilgisayar vizyonu farklı bir işlev gösterir (Analiz, OCR, vb.). Eğitmen kodu bir dosya, tüm altı dosyaları ya da yalnızca birkaç dosyaların ekleyebilmek altı öğretici bölümleri bağımlılıklarını sahip değilsiniz. Ve herhangi bir sırada dosyalara Eğitmen kodu ekleyin.

Haydi başlayalım.

## <a name="analyze-an-image"></a>Bir resmi çözümleme

Bilgisayar vizyonu Çözümle özelliğini 2. 000'den fazla tanınabilir nesneleri, yaşam önemlidir, manzara ve Eylemler için görüntü analiz eder. Analiz tamamlandığında Çözümle açıklayıcı etiketleri, renk analiz, resim yazıları ve daha fazlasını görüntüsüyle açıklayan bir JSON nesnesi döndürür.

Eğitmen uygulamasının Çözümle özelliği tamamlamak için aşağıdaki adımları gerçekleştirin:

### <a name="analyze-step-1-add-the-event-handler-code-for-the-form-button"></a>Analiz 1. adım: form düğmesi olay işleyicisi kodunu ekleyin

Açık **analyze.html** dosyasını bir metin düzenleyicisinde açıp bulun **analyzeButtonClick** dosyasının altına işlevi.

**AnalyzeButtonClick** olay işleyici işlevi formun temizler, URL'de belirtilen görüntü görüntüler sonra çağırır **AnalyzeImage** görüntü çözümlemek için işlevi.

Aşağıdaki kodu kopyalayıp **analyzeButtonClick** işlevi.

```javascript
function analyzeButtonClick() {

    // Clear the display fields.
    $("#sourceImage").attr("src", "#");
    $("#responseTextArea").val("");
    $("#captionSpan").text("");
    
    // Display the image.
    var sourceImageUrl = $("#inputImage").val();
    $("#sourceImage").attr("src", sourceImageUrl);
    
    AnalyzeImage(sourceImageUrl, $("#responseTextArea"), $("#captionSpan"));
}
```

### <a name="analyze-step-2-add-the-wrapper-for-the-rest-api-call"></a>2. adım analiz: REST API çağrısı için sarmalayıcı ekleme

**AnalyzeImage** görüntü çözümlemek için REST API çağrısı işlevi sarmalar. Başarılı bir geri döndürme bağlı biçimlendirilmiş JSON analiz belirtilen textarea görüntülenir ve resim yazısını belirtilen aralık içinde görüntülenir.

Kopyalama ve yapıştırma **AnalyzeImage** işlev altındaki Yeni kod **analyzeButtonClick** işlevi.

```javascript
/* Analyze the image at the specified URL by using Microsoft Cognitive Services Analyze Image API.
 * @param {string} sourceImageUrl - The URL to the image to analyze.
 * @param {<textarea> element} responseTextArea - The text area to display the JSON string returned
 *                             from the REST API call, or to display the error message if there was 
 *                             an error.
 * @param {<span> element} captionSpan - The span to display the image caption.
 */
function AnalyzeImage(sourceImageUrl, responseTextArea, captionSpan) {
    // Request parameters.
    var params = {
        "visualFeatures": "Categories,Description,Color",
        "details": "",
        "language": "en",
    };
    
    // Perform the REST API call.
    $.ajax({
        url: common.uriBasePreRegion + 
             $("#subscriptionRegionSelect").val() + 
             common.uriBasePostRegion + 
             common.uriBaseAnalyze +
             "?" + 
             $.param(params),
                    
        // Request headers.
        beforeSend: function(jqXHR){
            jqXHR.setRequestHeader("Content-Type","application/json");
            jqXHR.setRequestHeader("Ocp-Apim-Subscription-Key", 
                encodeURIComponent($("#subscriptionKeyInput").val()));
        },
        
        type: "POST",
        
        // Request body.
        data: '{"url": ' + '"' + sourceImageUrl + '"}',
    })
    
    .done(function(data) {
        // Show formatted JSON on webpage.
        responseTextArea.val(JSON.stringify(data, null, 2));
        
        // Extract and display the caption and confidence from the first caption in the description object.
        if (data.description && data.description.captions) {
            var caption = data.description.captions[0];
            
            if (caption.text && caption.confidence) {
                captionSpan.text("Caption: " + caption.text +
                    " (confidence: " + caption.confidence + ").");
            }
        }
    })
    
    .fail(function(jqXHR, textStatus, errorThrown) {
        // Prepare the error string.
        var errorString = (errorThrown === "") ? "Error. " : errorThrown + " (" + jqXHR.status + "): ";
        errorString += (jqXHR.responseText === "") ? "" : (jQuery.parseJSON(jqXHR.responseText).message) ? 
            jQuery.parseJSON(jqXHR.responseText).message : jQuery.parseJSON(jqXHR.responseText).error.message;
        
        // Put the error JSON in the response textarea.
        responseTextArea.val(JSON.stringify(jqXHR, null, 2));
        
        // Show the error message.
        alert(errorString);
    });
}
```

### <a name="analyze-step-3-run-the-application"></a>Analiz 3. adım: uygulamayı çalıştırma

Kaydet **analyze.html** dosyası ve bir Web tarayıcısında açın. Abonelik anahtarınızı put **abonelik anahtarı** alan ve doğru bölgede kullandığınızdan emin olun **abonelik bölge**. Analiz'ye tıklayın, bir görüntüye bir URL girin **analiz görüntü** görüntüyü çözümlemek ve sonuçları görmek için düğmesi.

## <a name="recognize-a-landmark"></a>Bir yer işareti tanı

Bilgisayar vizyonu önemli özellik dağlar veya ünlü binalar gibi doğal ve yapay işaretleri resmi analiz eder. Analiz tamamlandığında yer işareti görüntüde bulunan bilinen yerler tanımlayan bir JSON nesnesi döndürür.

Eğitmen uygulamanın önemli özelliği tamamlamak için aşağıdaki adımları gerçekleştirin:

### <a name="landmark-step-1-add-the-event-handler-code-for-the-form-button"></a>Yer işareti adım 1: formu düğmesi olay işleyicisi kodunu ekleyin

Açık **landmark.html** dosyasını bir metin düzenleyicisinde açıp bulun **landmarkButtonClick** dosyasının altına işlevi.

**LandmarkButtonClick** olay işleyici işlevi formun temizler, URL'de belirtilen görüntü görüntüler sonra çağırır **IdentifyLandmarks** görüntü çözümlemek için işlevi.

Aşağıdaki kodu kopyalayıp **landmarkButtonClick** işlevi.

```javascript
function landmarkButtonClick() {

    // Clear the display fields.
    $("#sourceImage").attr("src", "#");
    $("#responseTextArea").val("");
    $("#captionSpan").text("");
    
    // Display the image.
    var sourceImageUrl = $("#inputImage").val();
    $("#sourceImage").attr("src", sourceImageUrl);
    
    IdentifyLandmarks(sourceImageUrl, $("#responseTextArea"), $("#captionSpan"));
}
```

### <a name="landmark-step-2-add-the-wrapper-for-the-rest-api-call"></a>Yer işareti adım 2: REST API çağrısı için sarmalayıcı ekleme

**IdentifyLandmarks** görüntü çözümlemek için REST API çağrısı işlevi sarmalar. Başarılı bir geri döndürme bağlı biçimlendirilmiş JSON analiz belirtilen textarea görüntülenir ve resim yazısını belirtilen aralık içinde görüntülenir.

Kopyalama ve yapıştırma **IdentifyLandmarks** işlev altındaki Yeni kod **landmarkButtonClick** işlevi.

```javascript
/* Identify landmarks in the image at the specified URL by using Microsoft Cognitive Services 
 * Landmarks API.
 * @param {string} sourceImageUrl - The URL to the image to analyze for landmarks.
 * @param {<textarea> element} responseTextArea - The text area to display the JSON string returned
 *                             from the REST API call, or to display the error message if there was 
 *                             an error.
 * @param {<span> element} captionSpan - The span to display the image caption.
 */
function IdentifyLandmarks(sourceImageUrl, responseTextArea, captionSpan) {
    // Request parameters.
    var params = {
        "model": "landmarks"
    };
    
    // Perform the REST API call.
    $.ajax({
        url: common.uriBasePreRegion + 
             $("#subscriptionRegionSelect").val() + 
             common.uriBasePostRegion + 
             common.uriBaseLandmark +
             "?" + 
             $.param(params),
                    
        // Request headers.
        beforeSend: function(jqXHR){
            jqXHR.setRequestHeader("Content-Type","application/json");
            jqXHR.setRequestHeader("Ocp-Apim-Subscription-Key", 
                encodeURIComponent($("#subscriptionKeyInput").val()));
        },
        
        type: "POST",
        
        // Request body.
        data: '{"url": ' + '"' + sourceImageUrl + '"}',
    })
    
    .done(function(data) {
        // Show formatted JSON on webpage.
        responseTextArea.val(JSON.stringify(data, null, 2));
        
        // Extract and display the caption and confidence from the first caption in the description object.
        if (data.result && data.result.landmarks) {
            var landmark = data.result.landmarks[0];
            
            if (landmark.name && landmark.confidence) {
                captionSpan.text("Landmark: " + landmark.name +
                    " (confidence: " + landmark.confidence + ").");
            }
        }
    })
    
    .fail(function(jqXHR, textStatus, errorThrown) {
        // Prepare the error string.
        var errorString = (errorThrown === "") ? "Error. " : errorThrown + " (" + jqXHR.status + "): ";
        errorString += (jqXHR.responseText === "") ? "" : (jQuery.parseJSON(jqXHR.responseText).message) ? 
            jQuery.parseJSON(jqXHR.responseText).message : jQuery.parseJSON(jqXHR.responseText).error.message;
        
        // Put the error JSON in the response textarea.
        responseTextArea.val(JSON.stringify(jqXHR, null, 2));
        
        // Show the error message.
        alert(errorString);
    });
}
```

### <a name="landmark-step-3-run-the-application"></a>Yer işareti 3. adım: uygulamayı çalıştırma

Kaydet **landmark.html** dosyası ve bir Web tarayıcısında açın. Abonelik anahtarınızı put **abonelik anahtarı** alan ve doğru bölgede kullandığınızdan emin olun **abonelik bölge**. Analiz'ye tıklayın, bir görüntüye bir URL girin **analiz görüntü** görüntüyü çözümlemek ve sonuçları görmek için düğmesi.

## <a name="recognize-celebrities"></a>Ünlüleri tanıma

Bilgisayar vizyonu çok ünlüler özelliği bir görüntü ünlü kişi için çözümler. Analiz tamamlandığında çok ünlüler görüntüde bulunan çok ünlüler tanımlayan bir JSON nesnesi döndürür.

Eğitmen uygulamanın çok ünlüler özelliği tamamlamak için aşağıdaki adımları gerçekleştirin:

### <a name="celebrities-step-1-add-the-event-handler-code-for-the-form-button"></a>Çok ünlüler adım 1: formu düğmesi olay işleyicisi kodunu ekleyin

Açık **celebrities.html** dosyasını bir metin düzenleyicisinde açıp bulun **celebritiesButtonClick** dosyasının altına işlevi.

**CelebritiesButtonClick** olay işleyici işlevi formun temizler, URL'de belirtilen görüntü görüntüler sonra çağırır **IdentifyCelebrities** görüntü çözümlemek için işlevi.

Aşağıdaki kodu kopyalayıp **celebritiesButtonClick** işlevi.

```javascript
function celebritiesButtonClick() {

    // Clear the display fields.
    $("#sourceImage").attr("src", "#");
    $("#responseTextArea").val("");
    $("#captionSpan").text("");
    
    // Display the image.
    var sourceImageUrl = $("#inputImage").val();
    $("#sourceImage").attr("src", sourceImageUrl);
    
    IdentifyCelebrities(sourceImageUrl, $("#responseTextArea"), $("#captionSpan"));
}
```

### <a name="celebrities-step-2-add-the-wrapper-for-the-rest-api-call"></a>Çok ünlüler adım 2: REST API çağrısı için sarmalayıcı ekleme

```javascript
/* Identify celebrities in the image at the specified URL by using Microsoft Cognitive Services 
 * Celebrities API.
 * @param {string} sourceImageUrl - The URL to the image to analyze for celebrities.
 * @param {<textarea> element} responseTextArea - The text area to display the JSON string returned
 *                             from the REST API call, or to display the error message if there was 
 *                             an error.
 * @param {<span> element} captionSpan - The span to display the image caption.
 */
function IdentifyCelebrities(sourceImageUrl, responseTextArea, captionSpan) {
    // Request parameters.
    var params = {
        "model": "celebrities"
    };
    
    // Perform the REST API call.
    $.ajax({
        url: common.uriBasePreRegion + 
             $("#subscriptionRegionSelect").val() + 
             common.uriBasePostRegion + 
             common.uriBaseCelebrities +
             "?" + 
             $.param(params),
                    
        // Request headers.
        beforeSend: function(jqXHR){
            jqXHR.setRequestHeader("Content-Type","application/json");
            jqXHR.setRequestHeader("Ocp-Apim-Subscription-Key", 
                encodeURIComponent($("#subscriptionKeyInput").val()));
        },
        
        type: "POST",
        
        // Request body.
        data: '{"url": ' + '"' + sourceImageUrl + '"}',
    })
    
    .done(function(data) {
        // Show formatted JSON on webpage.
        responseTextArea.val(JSON.stringify(data, null, 2));
        
        // Extract and display the caption and confidence from the first caption in the description object.
        if (data.result && data.result.celebrities) {
            var celebrity = data.result.celebrities[0];
            
            if (celebrity.name && celebrity.confidence) {
                captionSpan.text("Celebrity name: " + celebrity.name +
                    " (confidence: " + celebrity.confidence + ").");
            }
        }
    })
    
    .fail(function(jqXHR, textStatus, errorThrown) {
        // Prepare the error string.
        var errorString = (errorThrown === "") ? "Error. " : errorThrown + " (" + jqXHR.status + "): ";
        errorString += (jqXHR.responseText === "") ? "" : (jQuery.parseJSON(jqXHR.responseText).message) ? 
            jQuery.parseJSON(jqXHR.responseText).message : jQuery.parseJSON(jqXHR.responseText).error.message;
        
        // Put the error JSON in the response textarea.
        responseTextArea.val(JSON.stringify(jqXHR, null, 2));
        
        // Show the error message.
        alert(errorString);
    });
}
```

### <a name="celebrities-step-3-run-the-application"></a>Çok ünlüler adım 3: uygulamayı çalıştırma

Kaydet **celebrities.html** dosyası ve bir Web tarayıcısında açın. Abonelik anahtarınızı put **abonelik anahtarı** alan ve doğru bölgede kullandığınızdan emin olun **abonelik bölge**. Analiz'ye tıklayın, bir görüntüye bir URL girin **analiz görüntü** görüntüyü çözümlemek ve sonuçları görmek için düğmesi.

## <a name="intelligently-generate-a-thumbnail"></a>Akıllıca küçük resim oluşturma

Küçük resim özelliği bilgisayar vizyonu görüntüden bir küçük resim oluşturur. Kullanarak **akıllı kırpma** özelliği, küçük resim özelliği tanımlar ve bir görüntü ve Merkezi'nde daha aesthetically güzel küçük resim görüntüleri oluşturmak için bu alan, küçük resmi ilgi alanı.

Eğitmen uygulamasının küçük resim özelliği tamamlamak için aşağıdaki adımları gerçekleştirin:

### <a name="thumbnail-step-1-add-the-event-handler-code-for-the-form-button"></a>Küçük resim adım 1: formu düğmesi olay işleyicisi kodunu ekleyin

Açık **thumbnail.html** dosyasını bir metin düzenleyicisinde açıp bulun **thumbnailButtonClick** dosyasının altına işlevi.

**ThumbnailButtonClick** olay işleyici işlevi formun temizler, URL'de belirtilen görüntü görüntüler sonra çağırır **getThumbnail** iki kez oluşturmak için iki küçük resim kırpılmış bir akıllı işlevi ve bir akıllı kırpma olmadan.

Aşağıdaki kodu kopyalayıp **thumbnailButtonClick** işlevi.

```javascript
function thumbnailButtonClick() {

    // Clear the display fields.
    document.getElementById("sourceImage").src = "#";
    document.getElementById("thumbnailImageSmartCrop").src = "#";
    document.getElementById("thumbnailImageNonSmartCrop").src = "#";
    document.getElementById("responseTextArea").value = "";
    document.getElementById("captionSpan").text = "";
    
    // Display the image.
    var sourceImageUrl = document.getElementById("inputImage").value;
    document.getElementById("sourceImage").src = sourceImageUrl;
    
    // Get a smart cropped thumbnail.
    getThumbnail (sourceImageUrl, true, document.getElementById("thumbnailImageSmartCrop"), 
        document.getElementById("responseTextArea"));
    
    // Get a non-smart-cropped thumbnail.
    getThumbnail (sourceImageUrl, false, document.getElementById("thumbnailImageNonSmartCrop"),
        document.getElementById("responseTextArea"));
}
```

### <a name="thumbnail-step-2-add-the-wrapper-for-the-rest-api-call"></a>Küçük resim adım 2: REST API çağrısı için sarmalayıcı ekleme

**GetThumbnail** görüntü çözümlemek için REST API çağrısı işlevi sarmalar. Başarılı bir geri döndürme küçük resim belirtilen img öğesi içinde görüntülenir.

Aşağıdakileri kopyalayıp **getThumbnail** altında henüz işlevi **thumbnailButtonClick** işlevi.

```javascript
/* Get a thumbnail of the image at the specified URL by using Microsoft Cognitive Services
 * Thumbnail API.
 * @param {string} sourceImageUrl URL to image.
 * @param {boolean} smartCropping Set to true to use the smart cropping feature which crops to the
 *                                more interesting area of an image; false to crop for the center
 *                                of the image.
 * @param {<img> element} imageElement The img element in the DOM which will display the thumnail image.
 * @param {<textarea> element} responseTextArea - The text area to display the Response Headers returned
 *                             from the REST API call, or to display the error message if there was 
 *                             an error.
 */
function getThumbnail (sourceImageUrl, smartCropping, imageElement, responseTextArea) {
    // Create the HTTP Request object.
    var xhr = new XMLHttpRequest();
    
    // Request parameters.
    var params = "width=100&height=150&smartCropping=" + smartCropping.toString();

    // Build the full URI.
    var fullUri = common.uriBasePreRegion + 
                  document.getElementById("subscriptionRegionSelect").value + 
                  common.uriBasePostRegion + 
                  common.uriBaseThumbnail +
                  "?" + 
                  params;
    
    // Identify the request as a POST, with the URI and parameters.
    xhr.open("POST", fullUri);
    
    // Add the request headers.
    xhr.setRequestHeader("Content-Type","application/json");
    xhr.setRequestHeader("Ocp-Apim-Subscription-Key", 
        encodeURIComponent(document.getElementById("subscriptionKeyInput").value));
    
    // Set the response type to "blob" for the thumbnail image data.
    xhr.responseType = "blob";
    
    // Process the result of the REST API call.
    xhr.onreadystatechange = function(e) {
        if(xhr.readyState === XMLHttpRequest.DONE) {
            
            // Thumbnail successfully created.
            if (xhr.status === 200) {
                // Show response headers.
                var s = JSON.stringify(xhr.getAllResponseHeaders(), null, 2);
                responseTextArea.value = JSON.stringify(xhr.getAllResponseHeaders(), null, 2);
                
                // Show thumbnail image.
                var urlCreator = window.URL || window.webkitURL;
                var imageUrl = urlCreator.createObjectURL(this.response);
                imageElement.src = imageUrl;
            } else {
                // Display the error message. The error message is the response body as a JSON string. 
                // The code in this code block extracts the JSON string from the blob response.
                var reader = new FileReader();
                
                // This event fires after the blob has been read.
                reader.addEventListener('loadend', (e) => {
                    responseTextArea.value = JSON.stringify(JSON.parse(e.srcElement.result), null, 2);
                });
                
                // Start reading the blob as text.
                reader.readAsText(xhr.response);
            }
        }
    }
    
    // Execute the REST API call.
    xhr.send('{"url": ' + '"' + sourceImageUrl + '"}');
}
```

### <a name="thumbnail-step-3-run-the-application"></a>Küçük resim adım 3: uygulamayı çalıştırma

Kaydet **thumbnail.html** dosyası ve bir Web tarayıcısında açın. Abonelik anahtarınızı put **abonelik anahtarı** alan ve doğru bölgede kullandığınızdan emin olun **abonelik bölge**. Analiz'ye tıklayın, bir görüntüye bir URL girin **küçük resimler oluşturma** görüntüyü çözümlemek ve sonuçları görmek için düğmesi.

## <a name="read-printed-text-ocr"></a>Yazdırılan metin (OCR) okuma

Bilgisayar vizyonu optik karakter tanıma özelliği yazdırılan metin görüntüsü analiz eder. Analiz tamamlandığında OCR metin ve görüntü metnin konumunu içeren bir JSON nesnesi döndürür.

Eğitmen uygulamasının OCR özelliği tamamlamak için aşağıdaki adımları gerçekleştirin:

### <a name="ocr-step-1-add-the-event-handler-code-for-the-form-button"></a>OCR adım 1: formu düğmesi olay işleyicisi kodunu ekleyin

Açık **ocr.html** dosyasını bir metin düzenleyicisinde açıp bulun **ocrButtonClick** dosyasının altına işlevi.

**OcrButtonClick** olay işleyici işlevi formun temizler, URL'de belirtilen görüntü görüntüler sonra çağırır **ReadOcrImage** görüntü çözümlemek için işlevi.

Aşağıdaki kodu kopyalayıp **ocrButtonClick** işlevi.

```javascript
function ocrButtonClick() {

    // Clear the display fields.
    $("#sourceImage").attr("src", "#");
    $("#responseTextArea").val("");
    $("#captionSpan").text("");
    
    // Display the image.
    var sourceImageUrl = $("#inputImage").val();
    $("#sourceImage").attr("src", sourceImageUrl);
    
    ReadOcrImage(sourceImageUrl, $("#responseTextArea"));
}
```

### <a name="ocr-step-2-add-the-wrapper-for-the-rest-api-call"></a>OCR adım 2: REST API çağrısı için sarmalayıcı ekleme

**ReadOcrImage** görüntü çözümlemek için REST API çağrısı işlevi sarmalar. Başarılı bir üzerine dönün, metin açıklayan biçimlendirilmiş JSON ve metin konumunu belirtilen textarea görüntülenir.

Aşağıdakileri kopyalayıp **ReadOcrImage** altında henüz işlevi **ocrButtonClick** işlevi.

```javascript
/* Recognize and read printed text in an image at the specified URL by using Microsoft Cognitive 
 * Services OCR API.
 * @param {string} sourceImageUrl - The URL to the image to analyze for printed text.
 * @param {<textarea> element} responseTextArea - The text area to display the JSON string returned
 *                             from the REST API call, or to display the error message if there was 
 *                             an error.
 */
function ReadOcrImage(sourceImageUrl, responseTextArea) {
    // Request parameters.
    var params = {
        "language": "unk",
        "detectOrientation ": "true",
    };

    // Perform the REST API call.
    $.ajax({
        url: common.uriBasePreRegion + 
             $("#subscriptionRegionSelect").val() + 
             common.uriBasePostRegion + 
             common.uriBaseOcr +
             "?" + 
             $.param(params),
        
        // Request headers.
        beforeSend: function(jqXHR){
            jqXHR.setRequestHeader("Content-Type","application/json");
            jqXHR.setRequestHeader("Ocp-Apim-Subscription-Key", 
                encodeURIComponent($("#subscriptionKeyInput").val()));
        },
        
        type: "POST",
        
        // Request body.
        data: '{"url": ' + '"' + sourceImageUrl + '"}',
    })
    
    .done(function(data) {
        // Show formatted JSON on webpage.
        responseTextArea.val(JSON.stringify(data, null, 2));
    })
    
    .fail(function(jqXHR, textStatus, errorThrown) {
        // Put the JSON description into the text area.
        responseTextArea.val(JSON.stringify(jqXHR, null, 2));
        
        // Display error message.
        var errorString = (errorThrown === "") ? "Error. " : errorThrown + " (" + jqXHR.status + "): ";
        errorString += (jqXHR.responseText === "") ? "" : (jQuery.parseJSON(jqXHR.responseText).message) ? 
            jQuery.parseJSON(jqXHR.responseText).message : jQuery.parseJSON(jqXHR.responseText).error.message;
        alert(errorString);
    });
}
```

### <a name="ocr-step-3-run-the-application"></a>OCR 3. adım: uygulamayı çalıştırma

Kaydet **ocr.html** dosyası ve bir Web tarayıcısında açın. Abonelik anahtarınızı put **abonelik anahtarı** alan ve doğru bölgede kullandığınızdan emin olun **abonelik bölge**. Metni okuyun ve ardından görüntüye bir URL girin **okuma görüntü** görüntüyü çözümlemek ve sonuçları görmek için düğmesi.

## <a name="read-handwritten-text-handwriting-recognition"></a>El yazısı metni (el yazısı tanıma) okuyun

El yazısı tanıma özelliği bilgisayar vizyonu el yazısı metin görüntüsü analiz eder. Analiz tamamlandığında, el yazısı tanıma metin ve görüntü metnin konumunu içeren bir JSON nesnesi döndürür.

Eğitmen uygulamasının el yazısı tanıma özelliği tamamlamak için aşağıdaki adımları gerçekleştirin:

### <a name="handwriting-recognition-step-1-add-the-event-handler-code-for-the-form-button"></a>El yazısı tanıma adım 1: formu düğmesi olay işleyicisi kodunu ekleyin

Açık **handwriting.html** dosyasını bir metin düzenleyicisinde açıp bulun **handwritingButtonClick** dosyasının altına işlevi.

**HandwritingButtonClick** olay işleyici işlevi formun temizler, URL'de belirtilen görüntü görüntüler sonra çağırır **HandwritingImage** görüntü çözümlemek için işlevi.

Aşağıdaki kodu kopyalayıp **handwritingButtonClick** işlevi.

```javascript
function handwritingButtonClick() {

    // Clear the display fields.
    $("#sourceImage").attr("src", "#");
    $("#responseTextArea").val("");
    
    // Display the image.
    var sourceImageUrl = $("#inputImage").val();
    $("#sourceImage").attr("src", sourceImageUrl);
    
    ReadHandwrittenImage(sourceImageUrl, $("#responseTextArea"));
}
```

### <a name="handwriting-recognition-step-2-add-the-wrapper-for-the-rest-api-call"></a>El yazısı tanıma adım 2: REST API çağrısı için sarmalayıcı ekleme

**ReadHandwrittenImage** işlevi bir görüntü çözümlemek için gerekli iki REST API çağrılarını sarmalar. El yazısı tanıma zaman alan bir işlem olduğundan, iki aşamalı işlemi kullanılır. İlk çağrıda işleme için resim gönderir; İşlem tamamlandığında, ikinci çağrı algılanan metni alır.

Metin aldıktan sonra metin ve metin konumunu açıklayan biçimlendirilmiş JSON içinde belirtilen textarea görüntülenir.

Aşağıdakileri kopyalayıp **ReadHandwrittenImage** altında henüz işlevi **handwritingButtonClick** işlevi.

```javascript
/* Recognize and read text from an image of handwriting at the specified URL by using Microsoft 
 * Cognitive Services Recognize Handwritten Text API.
 * @param {string} sourceImageUrl - The URL to the image to analyze for handwriting.
 * @param {<textarea> element} responseTextArea - The text area to display the JSON string returned
 *                             from the REST API call, or to display the error message if there was 
 *                             an error.
 */
function ReadHandwrittenImage(sourceImageUrl, responseTextArea) {
    // Request parameters.
    var params = {
        "handwriting": "true",
    };

    // This operation requrires two REST API calls. One to submit the image for processing,
    // the other to retrieve the text found in the image. 
    //
    // Perform the first REST API call to submit the image for processing.
    $.ajax({
        url: common.uriBasePreRegion + 
             $("#subscriptionRegionSelect").val() + 
             common.uriBasePostRegion + 
             common.uriBaseHandwriting +
             "?" + 
             $.param(params),
        
        // Request headers.
        beforeSend: function(jqXHR){
            jqXHR.setRequestHeader("Content-Type","application/json");
            jqXHR.setRequestHeader("Ocp-Apim-Subscription-Key", 
                encodeURIComponent($("#subscriptionKeyInput").val()));
        },
        
        type: "POST",
        
        // Request body.
        data: '{"url": ' + '"' + sourceImageUrl + '"}',
    })
    
    .done(function(data, textStatus, jqXHR) {
        // Show progress.
        responseTextArea.val("Handwritten image submitted.");
        
        // Note: The response may not be immediately available. Handwriting Recognition is an
        // async operation that can take a variable amount of time depending on the length
        // of the text you want to recognize. You may need to wait or retry this GET operation.
        //
        // Try once per second for up to ten seconds to receive the result.
        var tries = 10;
        var waitTime = 100;
        var taskCompleted = false;
        
        var timeoutID = setInterval(function () { 
            // Limit the number of calls.
            if (--tries <= 0) {
                window.clearTimeout(timeoutID);
                responseTextArea.val("The response was not available in the time allowed.");
                return;
            }

            // The "Operation-Location" in the response contains the URI to retrieve the recognized text.
            var operationLocation = jqXHR.getResponseHeader("Operation-Location");
            
            // Perform the second REST API call and get the response.
            $.ajax({
                url: operationLocation,
                
                // Request headers.
                beforeSend: function(jqXHR){
                    jqXHR.setRequestHeader("Content-Type","application/json");
                    jqXHR.setRequestHeader("Ocp-Apim-Subscription-Key",
                        encodeURIComponent($("#subscriptionKeyInput").val()));
                },
                
                type: "GET",
            })
            
            .done(function(data) {
                // If the result is not yet available, return.
                if (data.status && (data.status === "NotStarted" || data.status === "Running")) {
                    return;
                }
                
                // Show formatted JSON on webpage.
                responseTextArea.val(JSON.stringify(data, null, 2));
                
                // Indicate the task is complete and clear the timer.
                taskCompleted = true;
                window.clearTimeout(timeoutID);
            })
            
            .fail(function(jqXHR, textStatus, errorThrown) {
                // Indicate the task is complete and clear the timer.
                taskCompleted = true;
                window.clearTimeout(timeoutID);
                
                // Display error message.
                var errorString = (errorThrown === "") ? "Error. " : errorThrown + " (" + jqXHR.status + "): ";
                errorString += (jqXHR.responseText === "") ? "" : (jQuery.parseJSON(jqXHR.responseText).message) ? 
                    jQuery.parseJSON(jqXHR.responseText).message : jQuery.parseJSON(jqXHR.responseText).error.message;
                alert(errorString);
            });
        }, waitTime);
    })
    
    .fail(function(jqXHR, textStatus, errorThrown) {
        // Put the JSON description into the text area.
        responseTextArea.val(JSON.stringify(jqXHR, null, 2));
        
        // Display error message.
        var errorString = (errorThrown === "") ? "Error. " : errorThrown + " (" + jqXHR.status + "): ";
        errorString += (jqXHR.responseText === "") ? "" : (jQuery.parseJSON(jqXHR.responseText).message) ? 
            jQuery.parseJSON(jqXHR.responseText).message : jQuery.parseJSON(jqXHR.responseText).error.message;
        alert(errorString);
    });
}
```

### <a name="handwriting-recognition-step-3-run-the-application"></a>El yazısı tanıma adım 3: uygulamayı çalıştırma

Kaydet **handwriting.html** dosyası ve bir Web tarayıcısında açın. Abonelik anahtarınızı put **abonelik anahtarı** alan ve doğru bölgede kullandığınızdan emin olun **abonelik bölge**. Metni okuyun ve ardından görüntüye bir URL girin **okuma görüntü** görüntüyü çözümlemek ve sonuçları görmek için düğmesi.

## <a name="next-steps"></a>Sonraki adımlar

- [Bilgisayar görme API C&#35; Öğreticisi](CSharpTutorial.md)
- [Bilgisayar görme API Python Eğitmen](PythonTutorial.md)
