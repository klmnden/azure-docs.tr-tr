---
title: Anomali algılama Java uygulaması - Microsoft Bilişsel hizmetler | Microsoft Docs
description: Microsoft Bilişsel hizmetler Anomali algılama API'sini kullanan bir Java uygulaması keşfedin. Özgün veri noktaları API'ye gönderin ve beklenen değerini ve anomali noktalarını alın.
services: cognitive-services
author: wenya
manager: bix
ms.service: cognitive-services
ms.technology: anomaly-detection
ms.topic: article
ms.date: 05/01/2018
ms.author: wenya
ms.openlocfilehash: ac26e29f4a839f69b123489600c2c83fe395c48a
ms.sourcegitcommit: 609c85e433150e7c27abd3b373d56ee9cf95179a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/03/2018
ms.locfileid: "48247914"
---
# <a name="anomaly-detection-java-application"></a>Anomali algılama Java uygulaması

[!INCLUDE [PrivatePreviewNote](../../../../../includes/cognitive-services-anomaly-finder-private-preview-note.md)]

Bu makalede, Anomali algılama API'sini çağırmak için basit bir Java uygulaması kullanmayı gösterir.  
Örnek abonelik anahtarınız ile zaman serisi verilerini Anomali algılama API'sine gönderir, ardından tüm anomali noktaları ve beklenen değer, her veri noktası için API'den alır.

## <a name="prerequisites"></a>Önkoşullar

### <a name="platform-requirements"></a>Platform gereksinimleri

Bu öğretici ile geliştirilmiş [Intellij Idea](https://www.jetbrains.com/idea). Ve de yüklememiz gerekir [Java Development Kit (JDK)](http://www.oracle.com/technetwork/java/javase/downloads/index.html) sürüm 1.8 + ve güncel bir [Apache'nın Maven](http://maven.apache.org/) oluşturma aracı.

### <a name="subscribe-to-anomaly-detection-and-get-a-subscription-key"></a>Anomali algılama için abone ve bir abonelik anahtarı edinirler 

[!INCLUDE [GetSubscriptionKey](../includes/get-subscription-key.md)]
 

## <a name="download-the-tutorial-project"></a>Öğretici projesinin indirin

1. Git MicrosoftAnomalyDetection [Java depo](https://github.com/MicrosoftAnomalyDetection/java-sample).
2. Kopya tıklayın veya indir düğmesi.
3. Öğretici projesinin bir .zip dosyasını indirmek için ZIP'i indir'e tıklayın.

<a name="Step1"></a>
### <a name="open-the-tutorial-project"></a>Öğretici projesinin açın

1. Öğretici projesinin .zip dosyasını çıkartın.
2. Intellij Idea ' tıklatın **Dosya > Aç**, açık dosya ya da proje iletişim kutusu görüntülenir.
3. Ayıklanan projesinin kök yolunu seçin ve ardından Tamam'a tıklayın.
4. Projeleri panelinde genişletin **src > ana > java**.
5. Dosya Düzenleyicisi'ne yüklemek için com.microsoft.cognitiveservice.anomalydetection.Main.java çift tıklayın.

<a name="Step2"></a>
### <a name="replace-subscriptionkey-and-uri-region"></a>SubscriptionKey ve URI bölge değiştirin

```
// **********************************************
// *** Update or verify the following values. ***
// **********************************************

// Replace the subscriptionKey string value with your valid subscription key.
public static final String subscriptionKey = "<Subscription Key>";

public static final String uriBase = "https://api.labs.cognitive.microsoft.com/anomalyfinder/v1.0/anomalydetection";

```

<a name="Step3"></a>
### <a name="build-and-run-the-tutorial-project"></a>Derleme ve öğretici projesinin çalıştırma

1. Menüyü com.microsoft.cognitiveservice.anomalydetection.Main.java kaynak kod sekmesinde herhangi bir yere sağ tıklayarak getirin. 
2. Çalıştır 'Main.main() ''ı seçin
3. Örnek isteğinin sonucunu döndürdü ve terminal içinde gösterilir.

### <a name="result-of-the-tutorial-project"></a>Öğretici projesinin sonucu

[!INCLUDE [diagrams](../includes/diagrams.md)]

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [REST API başvurusu](https://dev.labs.cognitive.microsoft.com/docs/services/anomaly-detection/operations/post-anomalydetection)
