---
title: Azure Stream Analytics sorgu SELECT INTO kullanarak hata ayıklama
description: Bu makalede, Azure Stream Analytics işinde Orta sorgu veri sorgu söz dizimi içinde SELECT INTO deyimleri kullanarak örnek açıklar.
services: stream-analytics
author: jseb225
ms.author: jeanb
manager: kfile
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 04/20/2017
ms.openlocfilehash: b056d4c29464451d3dc0ef62437f934535820489
ms.sourcegitcommit: cb61439cf0ae2a3f4b07a98da4df258bfb479845
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/05/2018
ms.locfileid: "43698001"
---
# <a name="debug-queries-by-using-select-into-statements"></a>Sorgular SELECT INTO deyimleri kullanarak hata ayıklama

Gerçek zamanlı veri işleme verileri sorgu ortasında nasıl göründüğünü bilmek yararlı olabilir. Giriş veya bir Azure Stream Analytics işi adımları birden çok kez okunduğu için ek SELECT INTO deyimleri yazabilirsiniz. Bunun yapılması ve Ara verileri depolama alanına, veri doğruluğunu gibi incelemenize olanak tanır *değişkenleri izleyin* yapmak için hata ayıklaması yaptığınızda bir program.

## <a name="use-select-into-to-check-the-data-stream"></a>SELECT INTO veri akışını denetlemek için kullanın:

Azure Stream Analytics işi aşağıdaki örnek sorguda bir akış girişi, iki başvuru verisi girişleri ve Azure tablo depolama için bir çıktı sahiptir. Sorgu adı ve kategori bilgilerini almak için iki başvuru BLOB'ları ve olay hub'ı verileri birleştirir:

![SELECT INTO örnek sorgu](./media/stream-analytics-select-into/stream-analytics-select-into-query1.png)

İş çalışıyor, ancak çıkış olay üretilen unutmayın. Üzerinde **izleme** kutucuk, giriş, veri üretir, ancak hangi adımında tanımadığınız görürsünüz, burada gösterilen **katılın** tüm olayları bırakılmasına neden oldu.

![İzleme kutucuğu](./media/stream-analytics-select-into/stream-analytics-select-into-monitor.png)
 
Bu durumda, "Ara sonuçları birleştirme ve girdiden okunan verileri günlüğe kaydetmek için" birkaç ek SELECT INTO deyimleri ekleyebilirsiniz.

Bu örnekte, iki yeni "geçici çıktı." ekledik İstediğiniz herhangi bir havuza olabilirler. Burada Azure depolama örnek olarak kullanırız:

![Ek SELECT INTO deyimleri ekleme](./media/stream-analytics-select-into/stream-analytics-select-into-outputs.png)

Ardından, böyle bir sorgu yazabilirsiniz:

![SELECT INTO sorgusunu yeniden](./media/stream-analytics-select-into/stream-analytics-select-into-query2.png)

Artık işi yeniden başlatın ve birkaç dakikalığına çalışmasına olanak tanır. Ardından sorgu temp1 ve temp2 Gezgini'yle Visual Studio bulut aşağıdaki tablolar oluşturmak için:

**temp1 tablo**
![SELECT INTO temp1 tablo](./media/stream-analytics-select-into/stream-analytics-select-into-temp-table-1.png)

**temp2 tablo**
![SELECT INTO temp2 tablo](./media/stream-analytics-select-into/stream-analytics-select-into-temp-table-2.png)

Gördüğünüz gibi temp1 ve temp2 veriniz varsa ve Ad sütununda doğru temp2 doldurulur. Ancak, olduğundan hala veri çıkışında, bir şey yanlış:

![SELECT INTO output1 tabloyla veri yok](./media/stream-analytics-select-into/stream-analytics-select-into-out-table-1.png)

Verileri yeniden örnekleyerek, sorunu ikinci birleşim ile neredeyse emin olabilirsiniz. Başvuru verileri blob indirmek ve bir göz atalım:

![SELECT INTO başvuru tablosu](./media/stream-analytics-select-into/stream-analytics-select-into-ref-table-1.png)

Gördüğünüz gibi bu başvuru verilerinde GUID'sinin biçimi biçiminden farklıdır [sütundan] temp2. İşte bu verileri içinde output1 beklendiği gibi geldiğinde kaydetmedi.

Veri biçimi düzeltin, bunu blob başvurusu ve yeniden deneyin:

![SELECT INTO geçici tablo](./media/stream-analytics-select-into/stream-analytics-select-into-ref-table-2.png)

Bu kez, çıktı verileri biçimlendirilmiş ve beklendiği şekilde doldurulur.

![SELECT INTO tablonun son halini](./media/stream-analytics-select-into/stream-analytics-select-into-final-table.png)


## <a name="get-help"></a>Yardım alın

Daha fazla yardım için deneyin bizim [Azure Stream Analytics forumumuzu](https://social.msdn.microsoft.com/Forums/azure/home?forum=AzureStreamAnalytics).

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Stream analytics'e giriş](stream-analytics-introduction.md)
* [Azure Akış Analizi'ni kullanmaya başlama](stream-analytics-real-time-fraud-detection.md)
* [Azure Akış Analizi işlerini ölçeklendirme](stream-analytics-scale-jobs.md)
* [Azure Akış Analizi Sorgu Dili Başvurusu](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Azure Akış Analizi Yönetimi REST API'si Başvurusu](https://msdn.microsoft.com/library/azure/dn835031.aspx)

