---
title: Stream Analytics'ten Azure Application ınsights'ı kullanarak dışarı aktarma | Microsoft Docs
description: Stream Analytics sürekli dönüştürme, filtreleyebilir ve rota verilerini Application Insights'tan dışarı aktar.
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.assetid: 31594221-17bd-4e5e-9534-950f3b022209
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 01/08/2019
ms.author: mbullwin
ms.openlocfilehash: d4a4196aa601fc8da79da3962faec026eff5ec87
ms.sourcegitcommit: c0419208061b2b5579f6e16f78d9d45513bb7bbc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/08/2019
ms.locfileid: "67625068"
---
# <a name="use-stream-analytics-to-process-exported-data-from-application-insights"></a>Application ınsights'tan dışarı aktarılan verileri işlemek için Stream Analytics'i kullanma
[Azure Stream Analytics](https://azure.microsoft.com/services/stream-analytics/) veri işleme için ideal araçtır [Application Insights'tan dışarı aktarılan](export-telemetry.md). Stream Analytics, çeşitli kaynaklardan veri çekebilirsiniz. Bu dönüştürme ve verileri filtreleyin ve ardından çeşitli havuzlarını yönlendirmek.

Bu örnekte, yeniden adlandırma uygulama anlayışları'ndan verileri alır ve bazı alanlar işler ve Power BI'a kanallar bir bağdaştırıcı oluşturacağız.

> [!WARNING]
> Çok daha iyi ve daha kolay [Application Insights verilerini Power BI'da görüntülemek için önerilen yollar](../../azure-monitor/app/export-power-bi.md ). Burada gösterilen yolu dışarı aktarılan verileri işlemek nasıl göstermek için yalnızca bir örnektir.
> 
> 

![Blok diyagramını PBI için SA aracılığıyla dışarı aktarma](./media/export-stream-analytics/020.png)

## <a name="create-storage-in-azure"></a>Azure'da depolama oluşturma
Depolama oluşturmanız gerekir. böylece sürekli dışarı aktarmayı her zaman bir Azure depolama hesabı'ına verileri çıkarır.

1. Aboneliğinizde "Klasik" depolama hesabı oluşturma [Azure portalında](https://portal.azure.com).
   
   ![Azure portalında yeni, veri, depolama Seç](./media/export-stream-analytics/030.png)
2. Bir kapsayıcı oluşturma
   
    ![Yeni depolama kapsayıcıları seçin, kapsayıcıları kutucuk tıklatın ve ardından Ekle](./media/export-stream-analytics/040.png)
3. Depolama erişim tuşunu kopyalamak
   
    Stream analytics hizmetine giriş yakında ayarlamanız gerekir.
   
    ![Depolama ayarları, anahtarları, açın ve bir kopya birincil erişim anahtarını Al](./media/export-stream-analytics/045.png)

## <a name="start-continuous-export-to-azure-storage"></a>Azure depolama alanına sürekli dışarı aktarmayı Başlat
[Sürekli dışarı aktarma](export-telemetry.md) Application Insights'a Azure depolama alanından verileri taşır.

1. Azure portalında, uygulamanız için oluşturduğunuz Application Insights kaynağına göz atın.
   
    ![Application Insights, uygulamanızın Gözat'ı seçin](./media/export-stream-analytics/050.png)
2. Sürekli dışarı aktarma oluşturun.
   
    ![Sürekli dışarı aktarma ayarları Ekle öğesini seçin](./media/export-stream-analytics/060.png)

    Daha önce oluşturduğunuz depolama hesabını seçin:

    ![Dışarı aktarma hedefi ayarlama](./media/export-stream-analytics/070.png)

    Görmek istediğiniz olay türlerini ayarlayın:

    ![Olay türü seçin](./media/export-stream-analytics/080.png)

1. Accumulate bazı veriler sağlar. Arkanıza yaslanın ve insanların uygulamanızı bir süredir kullanın. Telemetri vardır ve istatistiksel grafikte gördüğünüz [ölçüm Gezgini](../../azure-monitor/app/metrics-explorer.md) ve olayları tek tek [tanılama araması](../../azure-monitor/app/diagnostic-search.md). 
   
    Ve ayrıca, veri depolama dışarı aktarır. 
2. Dışarı aktarılan verileri inceleyin. Visual Studio'da **görüntülemek / Cloud Explorer**ve Azure / depolama. (Bu menü seçeneği yoksa, Azure SDK'yı yüklemeniz gerekir: Yeni Proje iletişim kutusunu açın ve görseli açmak C# / bulut / .NET için Microsoft Azure SDK'sını edinme.)
   
    ![](./media/export-stream-analytics/04-data.png)
   
    Uygulama adı ve izleme anahtarından türetilen yol adı ortak kısmını not edin. 

Olayları JSON biçimindeki dosyaları blob yazılır. Her dosya, bir veya daha fazla olaylar içerebilir. Bu nedenle olay verileri okumak ve filtrelemek istediğimiz alanları istiyoruz. Her türden veri ile yapabileceğimiz şeyler vardır ancak planımız bugün Stream Analytics Power BI'a veri kanal kullanmaktır.

## <a name="create-an-azure-stream-analytics-instance"></a>Azure Stream Analytics örnek oluşturma
Gelen [Azure portalında](https://portal.azure.com/), Azure Stream Analytics hizmeti seçin ve yeni bir Stream Analytics işi oluşturun:

![](./media/export-stream-analytics/SA001.png)

![](./media/export-stream-analytics/SA002.png)

Yeni proje oluşturulduğunda seçin **kaynağa Git**.

![](./media/export-stream-analytics/SA003.png)

### <a name="add-a-new-input"></a>Yeni giriş Ekle

![](./media/export-stream-analytics/SA004.png)

Sürekli dışarı aktarma blobunuz giriş gerçekleştirecek şekilde ayarlayın:

![](./media/export-stream-analytics/SA0005.png)

Şimdi, depolama, daha önce not ettiğiniz hesabınızdan, birincil erişim anahtarı gerekir. Bu depolama hesabı anahtarı ayarlayın.

### <a name="set-path-prefix-pattern"></a>Set yol ön eki deseni

**YYYY-AA-GG tarih biçimini (kısa çizgi ile) ayarladığınızdan emin olun.**

Yol ön eki deseni, Stream Analytics giriş dosyaları depolamada yeri bulur belirtir. Sürekli dışarı aktarma verileri nasıl depoladı karşılık olarak ayarlamanız gerekir. Şöyle ayarlayın:

    webapplication27_12345678123412341234123456789abcdef0/PageViews/{date}/{time}

Bu örnekte:

* `webapplication27` Application Insights kaynak adı **küçük harflerle**.
* `1234...` Application Insights kaynağına ait izleme anahtarı **tire atlama**. 
* `PageViews` Analiz etmek istediğiniz veri türüdür. Kullanılabilir türler, sürekli dışarı aktarma ayarladığınız filtre bağlıdır. Diğer kullanılabilir türlerini görmek ve görmek için dışarı aktarılan verileri incelemek [veri modelini dışarı aktarma](export-data-model.md).
* `/{date}/{time}` bir desen tam anlamıyla yazılır.

> [!NOTE]
> Yolu doğru almak emin olmak için depolama inceleyin.
> 

## <a name="add-new-output"></a>Yeni çıkış Ekle
Artık işinizi seçin > **çıkışları** > **Ekle**.

![](./media/export-stream-analytics/SA006.png)


![Yeni bir kanal seçin, çıkışlar, Ekle, Power BI'ı tıklatın](./media/export-stream-analytics/SA010.png)

Sağlayın, **iş veya Okul hesabı** Stream Analytics, Power BI kaynağınıza erişmek için yetkilendirmek üzere. Ardından, çıkış ve tablo ve hedef Power BI veri kümesi için bir ad oluşturun.

## <a name="set-the-query"></a>Sorguyu Ayarla
Sorgu çevirisi çıkış girişten alınan yönetir.

Doğru çıkış Al denetlemek için Test işlevini kullanın. Bu giriş sayfasından gerçekleştirdiğiniz örnek verileri verin. 

### <a name="query-to-display-counts-of-events"></a>Olay sayısını görüntülemek için sorgu
Bu sorguyu yapıştırın:

```SQL

    SELECT
      flat.ArrayValue.name,
      count(*)
    INTO
      [pbi-output]
    FROM
      [export-input] A
    OUTER APPLY GetElements(A.[event]) as flat
    GROUP BY TumblingWindow(minute, 1), flat.ArrayValue.name
```

* dışarı aktarma girişi biz giriş akışa verdiğiniz diğer adı olan
* pbı çıkış tanımladığımız çıkış diğer adı olan
* Kullandığımız [dış uygulama GetElements](https://docs.microsoft.com/stream-analytics-query/apply-azure-stream-analytics) olay adı iç içe bir JSON dizisi olduğundan. Ardından Select olay adı, bir zaman dönemi içindeki bu ada sahip örneklerinin sayısı ile birlikte seçer. [Group By](https://docs.microsoft.com/stream-analytics-query/group-by-azure-stream-analytics) yan tümcesi, bir dakikalık zaman dönemlerine öğeleri gruplandırır.

### <a name="query-to-display-metric-values"></a>Ölçüm değerleri görüntülemek için sorgu
```SQL

    SELECT
      A.context.data.eventtime,
      avg(CASE WHEN flat.arrayvalue.myMetric.value IS NULL THEN 0 ELSE  flat.arrayvalue.myMetric.value END) as myValue
    INTO
      [pbi-output]
    FROM
      [export-input] A
    OUTER APPLY GetElements(A.context.custom.metrics) as flat
    GROUP BY TumblingWindow(minute, 1), A.context.data.eventtime

``` 

* Bu sorgu, etkinlik ve ölçü değerini almak için ölçümleri telemetrisine ayrıntılı açıklanmıştır. Ölçüm değerleri bir dizi içinde olduğundan, dış uygulama GetElements deseni satırları ayıklamak için kullanırız. "myMetric" ölçüm bu durumda adıdır. 

### <a name="query-to-include-values-of-dimension-properties"></a>Boyut özelliklerinin değerlerini dahil etmek için sorgu
```SQL

    WITH flat AS (
    SELECT
      MySource.context.data.eventTime as eventTime,
      InstanceId = MyDimension.ArrayValue.InstanceId.value,
      BusinessUnitId = MyDimension.ArrayValue.BusinessUnitId.value
    FROM MySource
    OUTER APPLY GetArrayElements(MySource.context.custom.dimensions) MyDimension
    )
    SELECT
     eventTime,
     InstanceId,
     BusinessUnitId
    INTO AIOutput
    FROM flat

```

* Bu sorgu, bir sabit boyutlu bir dizi dizini olan belirli bir boyut bağlı olarak boyut özelliklerinin değerlerini içerir.

## <a name="run-the-job"></a>İşi çalıştırma
İşten başlatmak için geçmiş bir tarih seçebilirsiniz. 

![İşi seçin ve sorguyu tıklayın. Aşağıdaki örnekte yapıştırın.](./media/export-stream-analytics/SA008.png)

İş çalışıyor kadar bekleyin.

## <a name="see-results-in-power-bi"></a>Sonuçları Power bı'da görün
> [!WARNING]
> Çok daha iyi ve daha kolay [Application Insights verilerini Power BI'da görüntülemek için önerilen yollar](../../azure-monitor/app/export-power-bi.md ). Burada gösterilen yolu dışarı aktarılan verileri işlemek nasıl göstermek için yalnızca bir örnektir.
> 
> 

Power BI ile işinizi açın veya Okul hesabı ve veri kümesini ve Stream Analytics işi çıktı olarak tanımlı bir tablo seçin.

![Power BI'da veri kümesi ve alanları seçin.](./media/export-stream-analytics/200.png)

Artık bu veri kümesi raporlarında ve panolarında [Power BI](https://powerbi.microsoft.com).

![Power BI'da veri kümesi ve alanları seçin.](./media/export-stream-analytics/210.png)

## <a name="no-data"></a>Veri yok mu?
* Denetleme, [tarih biçimini ayarlama](#set-path-prefix-pattern) doğru olarak YYYY-AA-GG (çizgilerle).

## <a name="video"></a>Video
Noam Ben Zeev Stream Analytics'i kullanarak dışarı aktarılan verilerin nasıl işleneceğini gösterir.

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Export-to-Power-BI-from-Application-Insights/player]
> 
> 

## <a name="next-steps"></a>Sonraki adımlar
* [Sürekli dışarı aktarma](export-telemetry.md)
* [Ayrıntılı veri özellik türleri ve değerleri için başvuru model.](export-data-model.md)
* [Application Insights](../../azure-monitor/app/app-insights-overview.md)

