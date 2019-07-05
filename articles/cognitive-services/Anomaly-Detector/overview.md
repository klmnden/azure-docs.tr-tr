---
title: Anomali Algılayıcısı API'si nedir? | Microsoft Docs
titleSuffix: Azure Cognitive Services
description: Zaman serisi verilerinizdeki anormallikleri belirlemek için gelişmiş algoritmalar Anomali algılayıcısı API'nin kullanın.
services: cognitive-services
author: aahill
ms.service: cognitive-services
ms.subservice: anomaly-detector
ms.topic: article
ms.date: 03/26/2019
ms.author: aahi
ms.openlocfilehash: 38b23ee4bfa8a1dbcc11615425ccd580c23eb3e1
ms.sourcegitcommit: ccb9a7b7da48473362266f20950af190ae88c09b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2019
ms.locfileid: "67593081"
---
# <a name="what-is-the-anomaly-detector-api"></a>Anomali Algılayıcısı API'si nedir?

Anomali algılayıcısı API'si, izleme ve machine learning ile zaman serisi verilerinizdeki prosesler algılamak sağlar. Anomali algılayıcısı API tanımlayarak ve sektör, senaryo veya veri hacmi bağımsız olarak verilerinize en sığdırma modelleri uygulama otomatik olarak uyum sağlar. Zaman serisi verilerinizi kullanarak, API sınırları anomali algılama, beklenen değerleri belirler ve anomalileri hangi veri noktalarıdır.

![Hizmet isteklerini desen değişiklikleri algılama](./media/anomaly_detection2.png)

Tüm önceki deneyime machine learning Anomali algılayıcısı kullanarak gerektirmez ve RESTful API hizmeti işlemleri ve uygulamaları kolayca tümleştirmenizi sağlar.

## <a name="features"></a>Özellikler

Anomali algılayıcısı ile otomatik olarak zaman serisi verilerinizle anormal durumları algılayabilirsiniz veya gerçek zamanlı olarak göründüklerinde. 

|Özellik  |Açıklama  |
|---------|---------|
|Gerçek zamanlı olarak göründüklerinde anomalileri algılayın. | Anomalileri, son bir anomali olup olmadığını belirlemek için önceden görülen veri noktalarını kullanarak akış verilerinizi algılayın. Bu işlem, göndermek ve hedef noktasına bir anomali olup olmadığını belirler ve veri noktalarını kullanarak bir model oluşturur. Oluşturduğunuz her yeni bir veri noktasının API'SİYLE çağırarak, verilerinizi oluşturulduğu şekilde izleyebilirsiniz. |
|Toplu olarak veri kümenizi boyunca anomalileri algılayın. | Zaman serisi verilerinizi bulunmayabilir anomalileri algılamak için kullanın. Bu işlem, tüm zaman serisi verilerinizi her noktasıyla aynı modeliyle analiz kullanarak bir model oluşturur.         |
| Verileriniz hakkında ek bilgi alın. | Verilerinizi ve beklenen değerleri, anomali sınırları ve konumlar gibi anomalileri gözlemlenen ilgili yararlı ayrıntıları alın. |
| Anomali algılama sınırları ayarlayın. | Anomali algılayıcısı API, anomali algılama için sınırları otomatik olarak oluşturur. Artırmak veya veri anomalileri API'nin hassasiyet azaltmak için bu sınırlarını ayarlama ve verilerinizi daha iyi uyum sağlamak. |

## <a name="demo"></a>Tanıtım

Anomali algılayıcısı API'ı kullanmaya hızlıca başlamak için deneyin bir [çevrimiçi Tanıtımı](https://notebooks.azure.com/AzureAnomalyDetection/projects/anomalydetector) tarayıcınızdan çalıştırılabilir. Bu Tanıtım, bir web barındırılan Jupyter not defteri çalıştırır ve bir API isteği göndermek ve sonucun görselleştirilmesi gösterilmektedir.

Tanıtım çalıştırmak için aşağıdaki adımları tamamlayın:

1. Geçerli bir abonelik Anomali algılayıcısı API anahtarı ve bir API uç noktası edinin. Aşağıdaki bölümde kaydolduğunuz için yönergeler içerir. 
2. Oturum açın ve sağ üst köşedeki kopyalama,'a tıklayın.
3. Tıklayın **ücretsiz işlem üzerinde çalıştırın**
4. Bu örnek için not defterlerini birini seçin.
5. Geçerli Anomali algılayıcısı API abonelik anahtarınızı ekleme `subscription_key` değişkeni. Değişiklik `endpoint` uç noktanıza değişken. Örneğin, `https://westus2.api.cognitive.microsoft.com`
1. Üst menü çubuğunda **hücre**, ardından **tümünü Çalıştır**.

## <a name="workflow"></a>İş akışı

Anomali algılayıcısı API'si bir RESTful web, HTTP istekleri ve JSON Ayrıştır tüm programlama dilinden çağrı kolaylaştırma hizmetidir.

[!INCLUDE [cognitive-services-anomaly-detector-data-requirements](../../../includes/cognitive-services-anomaly-detector-data-requirements.md)]

[!INCLUDE [cognitive-services-anomaly-detector-signup-requirements](../../../includes/cognitive-services-anomaly-detector-signup-requirements.md)]

Kaydolduktan sonra:

1. Zaman serisi verilerinizi alın ve geçerli bir JSON biçimine dönüştürün. Kullanım [en iyi uygulamalar](concepts/anomaly-detection-best-practices.md) verilerinizi en iyi sonuçları almak için hazırlarken.
1. Verilerinizle Anomali algılayıcısı API'sine bir istek gönderin.
1. Döndürülen JSON iletisini ayrıştırarak API yanıtını işleyin.

## <a name="next-steps"></a>Sonraki adımlar

* [Hızlı Başlangıç: Anomali algılayıcısı REST API kullanarak, zaman serisi verilerinde görülen anomalileri algılayın](quickstarts/detect-data-anomalies-csharp.md)
* Anomali algılayıcısı API [çevrimiçi Tanıtımı](https://notebooks.azure.com/AzureAnomalyDetection/projects/anomalydetector)
* Anomali algılayıcısı [REST API Başvurusu](https://westus2.dev.cognitive.microsoft.com/docs/services/AnomalyDetector/operations/post-timeseries-entire-detect)