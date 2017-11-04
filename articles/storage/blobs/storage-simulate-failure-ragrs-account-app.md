---
title: "Azure'da olarak yedekli depolamaya okuma erişimi erişme hatası benzetimini | Microsoft Docs"
description: "Coğrafi olarak yedekli depolamaya okuma erişimi erişimde bir hata benzetimi"
services: storage
documentationcenter: 
author: georgewallace
manager: timlt
editor: 
ms.service: storage
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: csharp
ms.topic: tutorial
ms.date: 10/12/2017
ms.author: gwallace
ms.custom: mvc
ms.openlocfilehash: 2919eb0e301000b53f4f63799e9c65aad45ca9f2
ms.sourcegitcommit: a7c01dbb03870adcb04ca34745ef256414dfc0b3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/17/2017
---
# <a name="simulate-a-failure-in-accessing-read-access-redundant-storage"></a>Yedekli depolamaya okuma erişimi erişimde arızanın benzetimini gerçekleştirin

Bu öğretici iki serinin bir parçasıdır. Bu öğreticide, başarısız bir yanıt sahip Fiddler istekleri için ekleme bir [okuma erişimli coğrafi olarak yedekli](../common/storage-redundancy.md#read-access-geo-redundant-storage) arızanın benzetimini gerçekleştirin ve ikincil uç noktasından okumak uygulama sağlamak için depolama hesabı (RA-GRS).

![Senaryo uygulama](media/storage-simulate-failure-ragrs-account-app/scenario.png)

Bölümünde dizisinin iki bilgi nasıl yapılır:

> [!div class="checklist"]
> * Çalıştırın ve uygulama duraklatma
> * Arızanın benzetimini gerçekleştirin
> * Birincil uç noktası geri yükleme benzetimi

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamak için:

* İndirme ve yükleme [Fiddler](https://www.telerik.com/download/fiddler)

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

Bu öğreticiyi tamamlamak için önceki depolama öğretici tamamlamış olmanız gerekir: [uygulama verilerinizi Azure storage ile yüksek oranda kullanılabilir hale][previous-tutorial].

## <a name="launch-fiddler"></a>Fiddler'ı Başlat

Açık Fiddler, select **kuralları** ve **özelleştirme kuralları**.

![Fiddler kuralları özelleştirme](media/storage-simulate-failure-ragrs-account-app/figure1.png)

Fiddler Macro'yu gösteren başlatır **SampleRules.js** dosya. Bu dosya, Fiddler özelleştirmek için kullanılır. Aşağıdaki kod örneğinde Yapıştır `OnBeforeResponse` işlevi. Yeni kod oluşturduğu mantığı hemen uygulanmadı emin olmak için dışı bırakılmıştır. Select tamamlandıktan sonra **dosya** ve **kaydetmek** yaptığınız değişiklikleri kaydetmek için.

```javascript
    /*
        // Simulate data center failure
        // After it is successfully downloading the blob, pause the code in the sample,
        // uncomment these lines of script, and save the script.
        // It will intercept the (probably successful) responses and send back a 503 error. 
        // When you're ready to stop sending back errors, comment these lines of script out again 
        //     and save the changes.

        if ((oSession.hostname == "contosoragrs.blob.core.windows.net") 
            && (oSession.PathAndQuery.Contains("HelloWorld"))) {
            oSession.responseCode = 503;  
        }
    */
```

![Özelleştirilmiş kural yapıştırın](media/storage-simulate-failure-ragrs-account-app/figure2.png)

## <a name="start-and-pause-the-application"></a>Başlatın ve uygulamayı duraklatma

Visual Studio'da basın **F5** veya seçin **Başlat** uygulama hata ayıklama başlatılamıyor. Birincil uç noktasından okuma uygulama başladıktan sonra basın **herhangi bir tuşa** uygulamasını duraklatmak için konsol penceresinde.

## <a name="simulate-failure"></a>Arızanın benzetimini gerçekleştirin

Uygulama ile duraklatıldı özel şimdi açıklamadan çıkarın kaydettik Fiddler'da önceki adımı kural. Bu kod örneği istekleri RA-GRS depolama hesabı ve yolunu görüntü adı içeriyorsa, arar `HelloWorld`, bir yanıt kodu, döndürür `503 - Service Unavailable`.

Fiddler ve select **kuralları** -> **kuralları Özelleştir...** .  Aşağıdaki satırlardaki yorumları kaldırın, yerine `STORAGEACCOUNTNAME` depolama hesabınızın adı. Seçin **dosya** -> **kaydetmek** yaptığınız değişiklikleri kaydetmek için.

```javascript
         if ((oSession.hostname == "STORAGEACCOUNTNAME.blob.core.windows.net")
         && (oSession.PathAndQuery.Contains("HelloWorld"))) {
         oSession.responseCode = 503;
         }
```

Uygulama sürdürmek için basın **herhangi bir tuşa** .

Uygulama yeniden çalışmaya başladıktan sonra birincil uç istekleri başarısız başlar. Uygulama birincil uç noktasına 5 kez yeniden dener. Hata eşiğinin beş denemeleri sonra görüntüyü ikincil salt okunur uç noktasından ister. Uygulama görüntüsü başarıyla ikincil uç noktasından 20 kez aldıktan sonra uygulama birincil bitiş noktasına bağlanmaya çalışır. Birincil uç nokta hala ulaşılamaz durumda olduğunda uygulama ikincil uç noktaya okuma sürdürür. Bu deseni [devre kesici](/azure/architecture/patterns/circuit-breaker.md) önceki öğreticide açıklanan düzeni.

![Özelleştirilmiş kural yapıştırın](media/storage-simulate-failure-ragrs-account-app/figure3.png)

## <a name="simulate-primary-endpoint-restoration"></a>Birincil uç noktası geri yükleme benzetimi

Önceki adımda Fiddler özel kural kümesi ile birincil uç istekleri başarısız olur. Yeniden çalışıp birincil uç nokta benzetimini yapmak için eklemesine mantığı kaldırmanız `503` hata.

Uygulama duraklatmak için basın **herhangi bir tuşa**.

### <a name="remove-the-custom-rule"></a>Özel kural Kaldır

Fiddler ve select **kuralları** ve **kuralları Özelleştir...** .  Yorum yapmak veya Özel mantık kaldırmak `OnBeforeResponse` işlevi, varsayılan işlevi çıkılıyor. Seçin **dosya** ve **kaydetmek** değişiklikleri kaydedin.

![Özelleştirilmiş kuralını Kaldır](media/storage-simulate-failure-ragrs-account-app/figure5.png)

Tamamlandığında, basın **herhangi bir tuşa** uygulama sürdürmek için. 999 okuma isabetler kadar uygulama birincil uç noktasından okuma devam eder.

![Uygulama Sürdür](media/storage-simulate-failure-ragrs-account-app/figure4.png)

## <a name="next-steps"></a>Sonraki adımlar

Bölümünde seri iki, nasıl gibi coğrafi olarak yedekli depolamaya okuma erişimi test etmek için bir hata benzetimini yapma hakkında öğrenilen:

> [!div class="checklist"]
> * Çalıştırın ve uygulama duraklatma
> * Arızanın benzetimini gerçekleştirin
> * Birincil uç noktası geri yükleme benzetimi

Önceden oluşturulmuş depolama örnekleri görmek için bu bağlantıyı izleyin.

> [!div class="nextstepaction"]
> [Azure depolama kod örnekleri](storage-samples-blobs-cli.md)

[previous-tutorial]: storage-create-geo-redundant-storage.md