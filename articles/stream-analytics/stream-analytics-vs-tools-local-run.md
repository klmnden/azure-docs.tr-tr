---
title: Azure Stream Analytics sorguları Visual Studio ile yerel olarak test etme
description: Bu makalede, Visual Studio için Azure Stream Analytics araçları sorgularla yerel olarak test etmek açıklar.
services: stream-analytics
author: su-jie
ms.author: sujie
manager: kfile
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 07/10/2018
ms.openlocfilehash: 1b86085a76f5ff87147db9dbd0a584784f5e4a2e
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60308485"
---
# <a name="test-stream-analytics-queries-locally-with-visual-studio"></a>Stream Analytics sorguları Visual Studio ile yerel olarak test etme

Visual Studio için Azure Stream Analytics araçları, Stream Analytics işleri örnek veriler içeren yerel olarak test etmek için kullanabilirsiniz.

Bunu kullanın [hızlı](stream-analytics-quick-create-vs.md) Visual Studio kullanarak Stream Analytics işi oluşturma hakkında bilgi edinmek için.

## <a name="test-your-query"></a>Sorgunuzu test etme

Azure Stream Analytics projenizde, çift **Script.asaql** betik Düzenleyicisi'nde açın. Sözdizimi hataları olup olmadığını görmek için sorguyu yeniden derleyebilirsiniz. Sorgu Düzenleyicisi, IntelliSense, söz dizimi renklendirme ve bir hata işaret destekler.

![Sorgu Düzenleyicisi](./media/stream-analytics-vs-tools-local-run/stream-analytics-tools-for-vs-query-01.png)
 
### <a name="add-local-input"></a>Yerel giriş ekle

Sorgunuz yerel statik veri karşı doğrulamak için seçin ve giriş sağ **yerel giriş Ekle**.
   
![Yerel giriş ekle](./media/stream-analytics-vs-tools-local-run/stream-analytics-tools-for-vs-add-local-input-01.png)
   
Açılır pencerede, örnek verileri, yerel yolu seçin ve **Kaydet**.
   
![Yerel giriş ekle](./media/stream-analytics-vs-tools-local-run/stream-analytics-tools-for-vs-add-local-input-02.png)
   
Adlı bir dosya **local_EntryStream.json** girişleri klasörünüze otomatik olarak eklenir.
   
![Yerel giriş klasörü dosya listesi](./media/stream-analytics-vs-tools-local-run/stream-analytics-tools-for-vs-add-local-input-03.png)
   
Seçin **yerel olarak çalıştırma** sorgu Düzenleyicisi'nde. Veya F5 tuşuna basabilirsiniz.
   
![Yerel Olarak Çalıştır](./media/stream-analytics-vs-tools-local-run/stream-analytics-tools-for-vs-local-run-01.png)
   
Doğrudan Visual Studio'dan bir tablo biçiminde çıktı görüntülenebilir.

![Tablo biçiminde çıktı](./media/stream-analytics-vs-tools-local-run/stream-analytics-for-vs-local-result.png)

Konsol çıktısı çıktı yolundan bulabilirsiniz. Sonuç klasörü açmak için herhangi bir tuşa basın.
   
![Yerel çalıştırma](./media/stream-analytics-vs-tools-local-run/stream-analytics-tools-for-vs-local-run-02.png)
   
Yerel klasör içinde sonuçlarını denetleyin.
   
![Yerel klasör sonucu](./media/stream-analytics-vs-tools-local-run/stream-analytics-tools-for-vs-local-run-03.png)
   

### <a name="sample-input"></a>Örnek Giriş
Yerel bir dosyaya, giriş kaynaklarından örnek giriş verileri de toplayabilirsiniz. Giriş yapılandırma dosyasını sağ tıklatın ve seçin **örnek verileri**. 

![Örnek Veri](./media/stream-analytics-vs-tools-local-run/stream-analytics-tools-for-vs-sample-data-01.png)

Event hubs'ı veya IOT hub'ları akış örnek verileri kullanabilirsiniz. Diğer giriş kaynakları desteklenmez. Açılan iletişim kutusunda, yerel yol seçin ve örnek verileri kaydetmek için dolgu **örnek**.

![Örnek verileri yapılandırma](./media/stream-analytics-vs-tools-local-run/stream-analytics-tools-for-vs-sample-data-02.png)
 
İlerleme durumunu görebilirsiniz **çıkış** penceresi. 

![Örnek veri çıkışı](./media/stream-analytics-vs-tools-local-run/stream-analytics-tools-for-vs-sample-data-03.png)

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Stream Analytics işleri görüntülemek için Visual Studio](stream-analytics-vs-tools.md)
* [Hızlı Başlangıç: Visual Studio kullanarak Stream Analytics işi oluşturma](stream-analytics-quick-create-vs.md)
* [Öğretici: Azure Stream Analytics işi dağıtma ile CI/Azure DevOps kullanarak CD](stream-analytics-tools-visual-studio-cicd-vsts.md)
* [Stream Analytics araçlarıyla sürekli tümleştirme ve geliştirme](stream-analytics-tools-for-visual-studio-cicd.md)