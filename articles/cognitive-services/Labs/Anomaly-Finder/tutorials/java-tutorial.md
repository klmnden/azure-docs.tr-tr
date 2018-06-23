---
title: Anomali algılama Java uygulaması - Microsoft Bilişsel hizmetler | Microsoft Docs
description: Microsoft Bilişsel Hizmetleri'nde Anomali algılama API'sini kullanan bir Java uygulaması keşfedin. API için özgün veri noktaları göndermek ve beklenen değer ve anomali noktaları alabilirsiniz.
services: cognitive-services
author: wenya
manager: bix
ms.service: cognitive-services
ms.technology: anomaly-detection
ms.topic: article
ms.date: 05/01/2018
ms.author: wenya
ms.openlocfilehash: 228d440da358eba1322e2228c54f21e925e36ecd
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35353321"
---
# <a name="anomaly-detection-java-application"></a>Anomali algılama Java uygulaması

Bu makalede, Anomali algılama API'sini çağırmak için basit bir Java uygulaması kullanarak gösterilmektedir.  
Örnek abonelik anahtarınızla Anomali algılama API için zaman serisi veri gönderir, ardından tüm anomali noktaları ve beklenen değer her veri noktası için API alır.

## <a name="prerequisites"></a>Önkoşullar

### <a name="platform-requirements"></a>Platform gereksinimleri

Bu öğretici kullanılarak geliştirilmiştir [Intellij Idea](https://www.jetbrains.com/idea). Ve yüklemenize gerek de [Java Geliştirme Seti (JDK)](http://www.oracle.com/technetwork/java/javase/downloads/index.html) 1.8 + ve güncel bir sürüm [Apache'nın Maven](http://maven.apache.org/) aracını yapılandırma.

### <a name="subscribe-to-anomaly-detection-and-get-a-subscription-key"></a>Anomali algılama abone olma ve aboneliği anahtarı alma 

[!INCLUDE [GetSubscriptionKey](../includes/get-subscription-key.md)]
 

## <a name="download-the-tutorial-project"></a>Eğitmen projenizi indirin

1. MicrosoftAnomalyDetection gidin [Java depo](https://github.com/MicrosoftAnomalyDetection/java-sample).
2. İndir düğmesi veya kopya tıklayın.
3. Eğitmen projesinin bir .zip dosyası yüklemek için ZIP'i indir'e tıklayın.

<a name="Step1"></a>
### <a name="open-the-tutorial-project"></a>Eğitmen projesini açın

1. Eğitmen projesinin .zip dosyasını ayıklayın.
2. Intellij Idea içinde tıklatın **Dosya > Aç**, açık bir dosya veya proje iletişim kutusu görüntülenir.
3. Ayıklanan projenin kök yolu seçin ve Tamam'ı tıklatın.
4. Projeleri panelinde genişletin **src > ana > java**.
5. Düzenleyiciye dosyasını yüklemek için com.microsoft.cognitiveservice.anomalydetection.Main.java çift tıklayın.

<a name="Step2"></a>
### <a name="replace-subscriptionkey-and-uri-region"></a>SubscriptionKey ve URI bölge Değiştir

```
// **********************************************
// *** Update or verify the following values. ***
// **********************************************

// Replace the subscriptionKey string value with your valid subscription key.
public static final String subscriptionKey = "<Subscription Key>";

public static final String uriBase = "https://api.labs.cognitive.microsoft.com/anomalyfinder/v1.0/anomalydetection";

```

<a name="Step3"></a>
### <a name="build-and-run-the-tutorial-project"></a>Eğitmen projesini derlemeyi ve çalıştırmayı

1. Menüsü com.microsoft.cognitiveservice.anomalydetection.Main.java kaynak kodu sekmesinde herhangi bir yere sağ tıklayarak getirin. 
2. Çalıştır 'Main.main() ''ı seçin
3. Örnek istek sonucu döndürdü ve terminale gösterilir.

### <a name="result-of-the-tutorial-project"></a>Eğitmen proje sonucu

[!INCLUDE [diagrams](../includes/diagrams.md)]

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [REST API başvurusu](https://dev.labs.cognitive.microsoft.com/docs/services/anomaly-detection/operations/post-anomalydetection)
