---
title: Azure Stream Analytics özellik karşılaştırması
description: Bu makalede, Azure Stream Analytics Bulut ve Azure portalı, Visual Studio ve Visual Studio Code IOT Edge işler için desteklenen özellikler karşılaştırılmaktadır.
services: stream-analytics
author: mamccrea
ms.author: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 06/27/2019
ms.openlocfilehash: 22d7ef90ee0cf4d09467516b7bb0664327b7dabe
ms.sourcegitcommit: 79496a96e8bd064e951004d474f05e26bada6fa0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2019
ms.locfileid: "67509787"
---
# <a name="azure-stream-analytics-feature-comparison"></a>Azure Stream Analytics özellik karşılaştırması

Azure Stream Analytics ile akış çözümleri kullanarak IOT Edge ve bulutta oluşturabilirsiniz [Azure portalında](stream-analytics-quick-create-portal.md), [Visual Studio](stream-analytics-quick-create-vs.md), ve [Visual Studio Code](quick-create-vs-code.md). Bu makaledeki tablolar, her platform için her iki proje türü tarafından hangi özelliklerin desteklendiği göster.

## <a name="cloud-job-features"></a>Bulut işi özellikleri


|Özellik  |Portal  |Visual Studio  |Visual Studio Code  |
|---------|---------|---------|---------|
|Platformlar arası     |Mac</br>Linux</br>Windows         |Windows        |Mac</br>Linux</br>Windows          |
|Komut dosyası yazma     |Evet         |Evet         |Evet         |
|Betik IntelliSense     |Söz dizimi vurgulama         |Söz dizimi vurgulama</br>Kod tamamlama</br>Hata işaretçisi         |Söz dizimi vurgulama</br>Kod tamamlama</br>Hata işaretçisi         |
|Girişler, çıkışlar ve iş yapılandırmalarını tanımlayın     |Evet         |Evet         |Evet         |
|Çıktı BLOB bölümleme     |Evet         |Evet         |Evet         |
|Çıktı olarak Power BI     |Evet         |Evet         |Hayır         |
|SQL veritabanı başvuru verileri     |Evet         |Evet         |Evet         |
|Özel ileti özellikleri     |Evet         |Hayır         |Hayır         |
|Giriş ve çıkışları birden çok sorgu arasında paylaşma     |Hayır         |Evet         |Evet         |
|JavaScript UDF ve UDA     |Evet         |Evet         |Yalnızca Windows         |
|Machine Learning belirtme     |Evet, ancak sorguyu test edilemiyor        |Evet, ancak yerel olarak test edilemiyor         |Hayır         |
|Uyumluluk düzeyi     |1.0</br>1.1</br>1.2         |1.0</br>1.1</br>1.2          |1.0</br>1.1</br>1.2          |
|Yerleşik ML tabanlı Anomali algılama işlevleri     |Evet         |Evet         |Evet         |
|Yerleşik Jeo-uzamsal İşlevler     |Evet         |Evet         |Evet         |
|Sorgu örnek bir dosya ile test etme     |Evet         |Evet         |Evet         |
|Canlı verileri yerel testi     |Hayır         |Evet         |Hayır         |
|İşleri listelemek ve iş varlıkları görüntüleyin     |Evet         |Evet         |Evet         |
|Bir işi dışarı aktarma için yerel bir proje     |Hayır         |Evet         |Evet         |
|İşleri durdurmak gönderin ve Başlat     |Evet         |Evet         |Evet         |
|Kaynak denetimi     |Hayır         |Evet         |Evet         |
|CI/CD desteği     |Kısmi         |Evet         |Evet         |
|İş ölçümlerini görüntüleme ve diyagramı     |Evet         |Evet         |Portalda açma         |
|Proje çalışma zamanı hataları görüntüle     |Evet         |Evet         |Hayır         |
|Tanılama günlükleri     |Evet         |Hayır         |Hayır         |


## <a name="iot-edge-job-features"></a>IOT Edge işi özellikleri

|Özellik  |Portal  |Visual Studio  |Visual Studio Code  |
|---------|---------|---------|---------|
|İşi geliştirme     |Evet         |Evet         |Hayır         |
|Kaynak denetimi     |Hayır         |Evet         |Hayır         |
|Bir işi dışarı aktarma için yerel bir proje     |Hayır         |Evet         |Hayır         |
|Sorgu örnek bir dosya ile test etme     |Evet         |Evet         |Hayır         |
|Giriş ve çıkışları birden çok sorgu arasında paylaşma     |Hayır         |Evet         |Hayır         |
|C#UDF     |Hayır         |Evet         |Hayır         |
|İşleri durdurmak gönderin ve Başlat     |Evet         |Evet         |Hayır         |
|İşleri listelemek ve iş varlıkları görüntüleyin     |Evet         |Evet         |Hayır         |
|İş ölçümlerini görüntüleme ve diyagramı     |Evet         |Kısmi         |Hayır         |
|Proje çalışma zamanı hataları görüntüle     |Evet         |Kısmi         |Hayır         |
|CI/CD desteği     |Hayır         |Hayır         |Hayır         |


## <a name="next-steps"></a>Sonraki adımlar

* [IOT Edge üzerinde Azure Stream Analytics](stream-analytics-edge.md)
* [Öğretici: Yazma bir C# Azure Stream Analytics IOT Edge işi (Önizleme) için kullanıcı tanımlı işlevi](stream-analytics-edge-csharp-udf.md)
* [Visual Studio Araçları'nı kullanarak Stream Analytics IOT Edge işlerini geliştirme](stream-analytics-tools-for-visual-studio-edge-jobs.md)
* [Azure Stream Analytics işleri görüntülemek için Visual Studio](stream-analytics-vs-tools.md)
* [Visual Studio Code (Önizleme) ile Azure Stream Analytics'i keşfedin](vscode-explore-jobs.md)


