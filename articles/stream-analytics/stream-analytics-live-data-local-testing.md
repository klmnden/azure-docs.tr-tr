---
title: Visual Studio için Azure Stream Analytics ile canlı verileri test etme
description: Azure Stream Analytics işinizi canlı akış verileri kullanarak yerel olarak test etme hakkında bilgi edinin.
services: stream-analytics
author: mamccrea
ms.author: mamccrea
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 12/07/2018
ms.custom: seodec18
ms.openlocfilehash: ea55d2f96a87503d43a69d288ce85dcff32a39ce
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61479839"
---
# <a name="test-live-data-locally-using-azure-stream-analytics-tools-for-visual-studio-preview"></a>Azure Stream Analytics araçları Visual Studio (Önizleme) kullanarak yerel olarak test canlı verileri

Visual Studio için Azure Stream Analytics araçları sayesinde işlerini IDE üzerinden yerel olarak test etmek canlı olay akışları Azure Event Hub, IOT hub'ı ve Blob Depolama kullanma. Canlı verileri yerel test edilemez değiştirin [performans ve ölçeklenebilirlik testi](stream-analytics-streaming-unit-consumption.md) bulutta gerçekleştirebilirsiniz ancak, Stream test etmek istediğiniz her zaman buluta göndermek zorunda işlevsel test sırasında zamandan tasarruf edebilirsiniz Analytics işi. Bu özellik Önizleme aşamasındadır ve üretim iş yükleri için kullanılmamalıdır.

## <a name="testing-options"></a>Test seçenekleri

Aşağıdaki yerel test seçenekleri desteklenir:

|**Girdi**  |**Çıktı**  |**İş türü**  |
|---------|---------|---------|
|Yerel statik veriler   |  Yerel statik veriler   |   Bulut/Edge |
|Canlı giriş verileri   |  Yerel statik veriler   |   Bulut |
|Canlı giriş verileri   |  Canlı çıktı verileri   |   Bulut |

## <a name="local-testing-with-live-data"></a>Yerel Canlı verilerle test

1. Oluşturduktan sonra bir [Azure Stream Analytics bulut projesini Visual Studio'da](stream-analytics-quick-create-vs.md)açın **script.asaql**. Yerel test yerel giriş ve çıkış yerel varsayılan olarak kullanır.

   ![Azure Stream Analytics Visual Studio yerel giriş ve çıkış yerel](./media/stream-analytics-live-data-local-testing/stream-analytics-local-testing-local-input-output.png)

2. Canlı verileri test etmek için seçin **kullanım bulut giriş** açılan kutusundan.

   ![Azure Stream Analytics Visual Studio Canlı bulut giriş](./media/stream-analytics-live-data-local-testing/stream-analytics-local-testing-cloud-input.png)


3. Ayarlama **başlattığınızda** giriş verilerini işlemeye işin ne zaman başlar tanımlamak için. İş giriş verileri doğru sonuçlar emin olmak için önceden okuması gerekebileceğini. Varsayılan süre 30 dakika geçerli vaktinden ayarlanır.

   ![Azure Stream Analytics Visual Studio canlı verileri başlangıç saati](./media/stream-analytics-live-data-local-testing/stream-analytics-local-testing-cloud-input-start-time.png)

4. Tıklayın **yerel olarak çalıştırma**. Bir konsol penceresi, çalışan ilerleme ve iş Ölçümleriyle görünür. İşlemi durdurmak istiyorsanız, bunu el ile yapabilirsiniz. 

   ![Azure Stream Analytics Visual Studio canlı verileri işlem penceresi](./media/stream-analytics-live-data-local-testing/stream-analytics-local-testing-cloud-input-process-window.png)

   Çıktı sonuçları yerel çalıştırma sonucu penceresinde ilk 500 çıkış sütunları içeren üç saniyede bir yenilenir ve çıkış dosyalarının proje yolunuzda yerleştirilir **ASALocalRun** klasör. Tıklayarak Çıkış dosyalarını açabilirsiniz **sonuçlar klasörünü Aç** yerel çalıştırma sonucu penceresinde düğmesine.

   ![Azure Stream Analytics Visual Studio canlı veri sonuçlar klasörünü açın](./media/stream-analytics-live-data-local-testing/stream-analytics-local-testing-cloud-input-open-results-folder.png)

5. Çıktı, bulut çıktı havuzlarından sonuçları istiyorsanız belirleyin **buluta çıkış** ikinci açılan kutusundan. Power BI ve Azure Data Lake Storage desteklenen çıktı havuzlarından değildir.

   ![Azure Stream Analytics Visual Studio canlı verileri buluta çıkış](./media/stream-analytics-live-data-local-testing/stream-analytics-local-testing-cloud-output.png)
 
## <a name="limitations"></a>Sınırlamalar

* Power BI ve Azure Data Lake Storage desteklenen çıkış havuzlarının kimlik doğrulama modeli sınırlamaları nedeniyle değildir.

* Yalnızca bulut giriş seçeneklerine sahip [zaman ilkeleri](stream-analytics-out-of-order-and-late-events.md) desteklerken yerel giriş seçenekleri destekler.

## <a name="next-steps"></a>Sonraki adımlar

* [Visual Studio için Azure Stream Analytics araçları kullanarak bir Stream Analytics işi oluşturma](stream-analytics-quick-create-vs.md)
* [Visual Studio için Azure Stream Analytics araçları yükleme](stream-analytics-tools-for-visual-studio-install.md)
* [Stream Analytics sorguları Visual Studio ile yerel olarak test etme](stream-analytics-vs-tools-local-run.md)
* [Azure Stream Analytics işleri görüntülemek için Visual Studio](stream-analytics-vs-tools.md)