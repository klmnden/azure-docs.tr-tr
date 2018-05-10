---
title: Azure Application Insights gelen akış analizi kullanarak dışarı aktarma | Microsoft Docs
description: Akış analizi sürekli dönüştürme, filtre uygulayabilir ve rota verilerini Application Insights ' dışarı aktar.
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.assetid: 31594221-17bd-4e5e-9534-950f3b022209
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 01/04/2018
ms.author: mbullwin
ms.openlocfilehash: c898e43cb1334bf7fb1836554fb92708033d3f7d
ms.sourcegitcommit: 870d372785ffa8ca46346f4dfe215f245931dae1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/08/2018
---
# <a name="use-stream-analytics-to-process-exported-data-from-application-insights"></a>Application Insights dışarı aktarılan verileri işlemek için Stream Analytics kullanın
[Azure Stream Analytics](https://azure.microsoft.com/services/stream-analytics/) veri işleme için ideal araçtır [Application Insights ' dışarı](app-insights-export-telemetry.md). Akış analizi, çeşitli kaynaklardan veri çeker. Bu dönüştürme ve verilere filtre ve ardından havuzlarını çeşitli için rota.

Bu örnekte, yeniden adlandırır Application Insights ' verileri alır ve bazı alanlar işler ve Power BI kanallar bir bağdaştırıcıya oluşturacağız.

> [!WARNING]
> Çok daha iyi ve daha kolay [Power BI'da Application Insights verileri görüntülemek için önerilen yollar](app-insights-export-power-bi.md). Burada gösterilen yol dışarı aktarılan verileri işlemek nasıl göstermek üzere yalnızca bir örnektir.
> 
> 

![Blok Diyagramı PBI SA aracılığıyla dışarı aktarma](./media/app-insights-export-stream-analytics/020.png)

## <a name="create-storage-in-azure"></a>Depolama oluşturma
Depolama oluşturmanız gerekir böylece sürekli verme verileri Azure depolama hesabı her zaman çıkarır.

1. Aboneliğinizde bir "Klasik" depolama hesabı oluşturun [Azure portal](https://portal.azure.com).
   
   ![Azure portalında yeni, verileri depolama seçin](./media/app-insights-export-stream-analytics/030.png)
2. Bir kapsayıcı oluşturma
   
    ![Yeni depolama kapsayıcıları seçin, kapsayıcıları döşeme tıklatın ve ardından Ekle](./media/app-insights-export-stream-analytics/040.png)
3. Depolama erişim tuşu kopyalama
   
    Akış analizi hizmetine giriş yakında ayarlamak için gerekir.
   
    ![Depolama ayarlarını, anahtarları, açın ve birincil erişim tuşunu bir kopyasını alın](./media/app-insights-export-stream-analytics/045.png)

## <a name="start-continuous-export-to-azure-storage"></a>Azure depolama alanına sürekli verme Başlat
[Sürekli verme](app-insights-export-telemetry.md) Application Insights Azure depolama alanına gelen verileri taşır.

1. Azure portalında, uygulamanız için oluşturduğunuz Application Insights kaynağı göz atın.
   
    ![Gözat, Application Insights, uygulamanızı seçin](./media/app-insights-export-stream-analytics/050.png)
2. Sürekli verme oluşturun.
   
    ![Ayarları, sürekli verme ekleme seçin](./media/app-insights-export-stream-analytics/060.png)

    Daha önce oluşturduğunuz depolama hesabı seçin:

    ![Dışa aktarma hedefi ayarlama](./media/app-insights-export-stream-analytics/070.png)

    Görmek istediğiniz olay türlerini ayarlayın:

    ![Olay türlerini seçin](./media/app-insights-export-stream-analytics/080.png)

1. Accumulate bazı veriler sağlar. Arkanıza yaslanın ve kişilere uygulamanızın biraz kullanın. Telemetri geldikçe ve istatistiksel grafiklerde görürsünüz [ölçüm Gezgini](app-insights-metrics-explorer.md) ve olayları tek tek [tanılama arama](app-insights-diagnostic-search.md). 
   
    Ve ayrıca, verileri depolama alanınızın dışa aktarır. 
2. Dışarı aktarılan verileri inceleyin. Visual Studio'da, **görüntülemek / Cloud Explorer**ve Azure açın / depolama. (Bu menü seçeneği yoksa, Azure SDK'yı yüklemeniz gerekir: Yeni Proje iletişim kutusunu açın ve Visual C# açın / bulut / .NET için Microsoft Azure SDK'sını alın.)
   
    ![](./media/app-insights-export-stream-analytics/04-data.png)
   
    Uygulama adı ve araçları anahtarından türetilen yol adı, ortak parçası not edin. 

JSON biçiminde dosyaları blob yazılır. Her dosya bir veya daha fazla olaylar içerebilir. Bu nedenle olay verileri okumak ve istiyoruz alanları filtrelemek isteriz. Her türlü verilerle yapabileceğimiz bir şey vardır, ancak bizim planı bugün Stream Analytics Power BI veri kanal kullanmaktır.

## <a name="create-an-azure-stream-analytics-instance"></a>Bir Azure akış analizi örneği oluşturma
Gelen [Azure portal](https://portal.azure.com/), Azure Stream Analytics hizmeti seçin ve yeni bir Stream Analytics işi oluştur:

![](./media/app-insights-export-stream-analytics/SA001.png)

![](./media/app-insights-export-stream-analytics/SA002.png)

Yeni iş oluşturulduğunda seçin **kaynağa gidin**.

![](./media/app-insights-export-stream-analytics/SA003.png)

### <a name="add-a-new-input"></a>Yeni giriş Ekle

![](./media/app-insights-export-stream-analytics/SA004.png)

Sürekli verme blobundan giriş gerçekleştirecek şekilde ayarlayın:

![](./media/app-insights-export-stream-analytics/SA005.png)

Şimdi, depolama, daha önce not ettiğiniz hesabınızdan, birincil erişim anahtarı gerekir. Bu depolama hesabı anahtarı ayarlayın.

### <a name="set-path-prefix-pattern"></a>Set yol önek deseni

**Tarih biçimi YYYY-AA-GG (tire ile) ayarladığınızdan emin olun.**

Stream Analytics girdi dosyaları depolama burada bulur önek yol deseni belirtir. Bunu sürekli verme verileri nasıl depolar karşılık gelecek şekilde ayarlamanız gerekir. Aşağıdaki gibi ayarlayın:

    webapplication27_12345678123412341234123456789abcdef0/PageViews/{date}/{time}

Bu örnekte:

* `webapplication27` Application Insights kaynağı adı **tüm küçük**.
* `1234...` Application Insights kaynağı izleme anahtarını olan **tire atlama**. 
* `PageViews` Analiz etmek istediğiniz veri türüdür. Kullanılabilir türler filtresi sürekli dışarı aktarma ile Ayarla bağlıdır. Diğer kullanılabilir türleri görmek ve görmek için dışarı aktarılan verileri incelemek [veri modeli verme](app-insights-export-data-model.md).
* `/{date}/{time}` bir desen tam anlamıyla yazılır.

> [!NOTE]
> Yolun sağ alma emin olmak için depolama inceleyin.
> 

## <a name="add-new-output"></a>Yeni çıkış ekleme
Şimdi, işi seçin > **çıkışları** > **Ekle**.

![](./media/app-insights-export-stream-analytics/SA006.png)


![Yeni kanal seçin, çıkışları, Ekle, Power BI'ı tıklatın](./media/app-insights-export-stream-analytics/SA010.png)

Sağlayın, **iş veya Okul hesabı** akış analizi, Power BI kaynağa erişmek için yetki vermek için. Ardından çıktı için ve tablo ve hedef Power BI veri kümesi için bir ad oluşturun.

## <a name="set-the-query"></a>Sorgu ayarlayın
Sorgu çıktısını almak için giriş çevrilmesi yönetir.

Doğru çıkış Al denetlemek için Test işlevini kullanın. Giriş sayfasından sürdü örnek verileri verin. 

### <a name="query-to-display-counts-of-events"></a>Olayların sayısını görüntülemek için sorgu
Bu sorgu yapıştırın:

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

* dışarı aktarma girişi biz Giriş akışı verdiğiniz diğer adıdır
* Biz tanımlanan çıkış diğer pbı çıkış adıdır
* Kullanırız [dış uygulamak GetElements](https://msdn.microsoft.com/library/azure/dn706229.aspx) olay adı bir iç içe geçmiş JSON dizisi olduğundan. Sonra Seç süre ad örnekleriyle sayısını birlikte olay adı seçer. [Group By](https://msdn.microsoft.com/library/azure/dn835023.aspx) yan tümcesi süreleri bir dakika içinde öğeleri gruplandırır.

### <a name="query-to-display-metric-values"></a>Ölçü değerlerini görüntülemek için sorgu
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

* Bu sorgu olay süresi ve ölçüm değeri almak için ölçümleri telemetri ayrıntılı açıklanmıştır. Dış uygulamak GetElements düzeni satırları ayıklamak için kullanırız şekilde ölçüm bir dizi içinde değerlerdir. "myMetric" ölçüm bu durumda adıdır. 

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

* Bu sorgu boyut dizisindeki sabit dizinindeki olan belirli bir boyut bağlı olarak boyut özelliklerinin değerlerini içerir.

## <a name="run-the-job"></a>İşini çalıştır
İşten başlatmak için geçmiş bir tarih seçin. 

![İşi seçin ve sorguyu tıklayın. Aşağıdaki örnek yapıştırın.](./media/app-insights-export-stream-analytics/SA008.png)

İş çalışıyor kadar bekleyin.

## <a name="see-results-in-power-bi"></a>Power BI sonuçlarına bakın
> [!WARNING]
> Çok daha iyi ve daha kolay [Power BI'da Application Insights verileri görüntülemek için önerilen yollar](app-insights-export-power-bi.md). Burada gösterilen yol dışarı aktarılan verileri işlemek nasıl göstermek üzere yalnızca bir örnektir.
> 
> 

Power BI ile iş açın veya Okul hesabı ve veri kümesi ve Stream Analytics işi çıkış olarak tanımlanan tablo seçin.

![Power BI'da veri kümesi ve alanları seçin.](./media/app-insights-export-stream-analytics/200.png)

Artık bu veri kümesi raporlarında ve panolarında kullanarak [Power BI](https://powerbi.microsoft.com).

![Power BI'da veri kümesi ve alanları seçin.](./media/app-insights-export-stream-analytics/210.png)

## <a name="no-data"></a>Veri yok mu?
* Denetleyin, [tarih biçimini ayarladıktan](#set-path-prefix-pattern) doğru olarak YYYY-AA-GG (çizgilerle ile).

## <a name="video"></a>Video
Noam Ben Zeev kullanarak Stream Analytics dışarı aktarılan verileri işlemek nasıl gösterir.

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Export-to-Power-BI-from-Application-Insights/player]
> 
> 

## <a name="next-steps"></a>Sonraki adımlar
* [Sürekli dışarı aktarma](app-insights-export-telemetry.md)
* [Ayrıntılı veri özellik türleri ve değerleri için başvuru modeli.](app-insights-export-data-model.md)
* [Application Insights](app-insights-overview.md)

