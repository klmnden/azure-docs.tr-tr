---
title: SQL Azure Application Insights'tan dışarı aktarma | Microsoft Docs
description: Stream Analytics kullanarak SQL'ye sürekli olarak Application Insights verilerini dışarı aktarın.
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.assetid: 48903032-2c99-4987-9948-d6e4559b4a63
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 09/11/2017
ms.author: mbullwin
ms.openlocfilehash: eecd2a50607fa42562a9ae6a7fb950a253655a45
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65872714"
---
# <a name="walkthrough-export-to-sql-from-application-insights-using-stream-analytics"></a>Çözüm: Stream Analytics kullanarak Application Insights'tan SQL'e aktarma
Bu makale, telemetri verilerini taşıma [Azure Application Insights] [ start] kullanarak bir Azure SQL veritabanı'na [sürekli dışarı aktarma] [ export] ve [Azure Stream Analytics](https://azure.microsoft.com/services/stream-analytics/). 

Sürekli dışarı aktarma, telemetri verilerinizi Azure Depolama'ya JSON biçiminde taşır. Biz, Azure Stream Analytics'i kullanarak JSON nesneleri ve bir veritabanı tablosundaki satırları oluşturun.

(Daha genel olarak, sürekli dışarı aktarma kendi uygulamalarınızı Application Insights'a gönderdiği telemetriyi çözümlemesi yoludur. Veri toplama gibi dışarı aktarılan telemetri ile diğer işlemler yapmak üzere bu kod örneği uyarlayabilirsiniz.)

İzlemek istediğiniz uygulamanın sahip olduğunuz varsayılarak da başlayacağız.

Bu örnekte, biz sayfanın görünüm verilerini kullanarak, ancak aynı düzeni diğer veri türlerine özel olayları ve özel durumlar gibi kolayca genişletilebilir. 

## <a name="add-application-insights-to-your-application"></a>Uygulamanıza Application Insights ekleyin
Kullanmaya başlamak için:

1. [Web sayfalarınız için Application ınsights'ı ayarlama](../../azure-monitor/app/javascript.md). 
   
    (Bu örnekte, istemci tarayıcıları uygulamanızdan sayfa görüntüleme verileri işleme odaklanacağız, ancak Application ınsights'ı için sunucu tarafı ayarlayabilirsiniz, [Java](../../azure-monitor/app/java-get-started.md) veya [ASP.NET](../../azure-monitor/app/asp-net.md) uygulama ve işlem isteği bağımlılık ve diğer sunucu telemetrisi.)
2. Uygulamanızı yayımlayın ve telemetri verilerini Application Insights kaynağınıza görüntülenmesini izleyin.

## <a name="create-storage-in-azure"></a>Azure'da depolama oluşturma
Depolama oluşturmanız gerekir. böylece sürekli dışarı aktarmayı her zaman bir Azure depolama hesabı'ına verileri çıkarır.

1. Aboneliğinize bir depolama hesabı oluşturun [Azure portalında][portal].
   
    ![Azure portalında yeni, veri, depolama birimi seçin. Klasik seçin, oluşturun. Depolama adı sağlayın.](./media/code-sample-export-sql-stream-analytics/040-store.png)
2. Bir kapsayıcı oluşturma
   
    ![Yeni depolama kapsayıcıları seçin, kapsayıcıları kutucuk tıklatın ve ardından Ekle](./media/code-sample-export-sql-stream-analytics/050-container.png)
3. Depolama erişim tuşunu kopyalamak
   
    Stream analytics hizmetine giriş yakında ayarlamanız gerekir.
   
    ![Depolama ayarları, anahtarları, açın ve bir kopya birincil erişim anahtarını Al](./media/code-sample-export-sql-stream-analytics/21-storage-key.png)

## <a name="start-continuous-export-to-azure-storage"></a>Azure depolama alanına sürekli dışarı aktarmayı Başlat
1. Azure portalında, uygulamanız için oluşturduğunuz Application Insights kaynağına göz atın.
   
    ![Application Insights, uygulamanızın Gözat'ı seçin](./media/code-sample-export-sql-stream-analytics/060-browse.png)
2. Sürekli dışarı aktarma oluşturun.
   
    ![Sürekli dışarı aktarma ayarları Ekle öğesini seçin](./media/code-sample-export-sql-stream-analytics/070-export.png)

    Daha önce oluşturduğunuz depolama hesabını seçin:

    ![Dışarı aktarma hedefi ayarlama](./media/code-sample-export-sql-stream-analytics/080-add.png)

    Görmek istediğiniz olay türlerini ayarlayın:

    ![Olay türü seçin](./media/code-sample-export-sql-stream-analytics/085-types.png)


1. Accumulate bazı veriler sağlar. Arkanıza yaslanın ve insanların uygulamanızı bir süredir kullanın. Telemetri vardır ve istatistiksel grafikte gördüğünüz [ölçüm Gezgini](../../azure-monitor/app/metrics-explorer.md) ve olayları tek tek [tanılama araması](../../azure-monitor/app/diagnostic-search.md). 
   
    Ve ayrıca, veri depolama dışarı aktarır. 
2. Dışarı aktarılan verileri, portaldaki ya da inceleyin - seçme **Gözat**, depolama hesabınızı seçin ve ardından **kapsayıcıları** - ya da Visual Studio'da. Visual Studio'da **görüntülemek / Cloud Explorer**ve Azure / depolama. (Bu menü seçeneği yoksa, Azure SDK'yı yüklemeniz gerekir: Yeni Proje iletişim kutusunu açın ve görseli açmak C# / bulut / .NET için Microsoft Azure SDK'sını edinme.)
   
    ![Visual Studio'da açık sunucu tarayıcısı, Azure, depolama](./media/code-sample-export-sql-stream-analytics/087-explorer.png)
   
    Uygulama adı ve izleme anahtarından türetilen yol adı ortak kısmını not edin. 

Olayları JSON biçimindeki dosyaları blob yazılır. Her dosya, bir veya daha fazla olaylar içerebilir. Bu nedenle olay verileri okumak ve filtrelemek istediğimiz alanları istiyoruz. Her türden veri ile yapabileceğimiz şeyler vardır ancak planımız bugün Stream Analytics verileri bir SQL veritabanı'na taşımak için kullanmaktır. Bu çok ilgi çekici sorguları çalıştırmak kolaylaştıracak.

## <a name="create-an-azure-sql-database"></a>Bir Azure SQL veritabanı oluşturma
Aboneliğinize yeniden başlayarak [Azure portalı][portal], veritabanını oluşturmak (ve yeni bir sunucu zaten bir kendinizi sürece) için veri yazacaksınız.

![Yeni, veri, SQL](./media/code-sample-export-sql-stream-analytics/090-sql.png)

Veritabanı sunucusu, Azure hizmetlerine erişime izin verdiğinden emin olun:

![Göz atma, sunucuları, sunucunuzun, ayarlar, güvenlik duvarı, Azure için erişime izin ver](./media/code-sample-export-sql-stream-analytics/100-sqlaccess.png)

## <a name="create-a-table-in-azure-sql-db"></a>Azure SQL DB'de tablosu oluşturma
Tercih edilen Yönetim aracınızla önceki bölümde oluşturduğunuz veritabanına bağlanın. Bu kılavuzda, biz kullanacaklardır [SQL Server yönetim araçlarını](https://msdn.microsoft.com/ms174173.aspx) (SSMS).

![](./media/code-sample-export-sql-stream-analytics/31-sql-table.png)

Yeni bir sorgu oluşturun ve aşağıdaki T-SQL yürütme:

```SQL

CREATE TABLE [dbo].[PageViewsTable](
    [pageName] [nvarchar](max) NOT NULL,
    [viewCount] [int] NOT NULL,
    [url] [nvarchar](max) NULL,
    [urlDataPort] [int] NULL,
    [urlDataprotocol] [nvarchar](50) NULL,
    [urlDataHost] [nvarchar](50) NULL,
    [urlDataBase] [nvarchar](50) NULL,
    [urlDataHashTag] [nvarchar](max) NULL,
    [eventTime] [datetime] NOT NULL,
    [isSynthetic] [nvarchar](50) NULL,
    [deviceId] [nvarchar](50) NULL,
    [deviceType] [nvarchar](50) NULL,
    [os] [nvarchar](50) NULL,
    [osVersion] [nvarchar](50) NULL,
    [locale] [nvarchar](50) NULL,
    [userAgent] [nvarchar](max) NULL,
    [browser] [nvarchar](50) NULL,
    [browserVersion] [nvarchar](50) NULL,
    [screenResolution] [nvarchar](50) NULL,
    [sessionId] [nvarchar](max) NULL,
    [sessionIsFirst] [nvarchar](50) NULL,
    [clientIp] [nvarchar](50) NULL,
    [continent] [nvarchar](50) NULL,
    [country] [nvarchar](50) NULL,
    [province] [nvarchar](50) NULL,
    [city] [nvarchar](50) NULL
)

CREATE CLUSTERED INDEX [pvTblIdx] ON [dbo].[PageViewsTable]
(
    [eventTime] ASC
)WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, SORT_IN_TEMPDB = OFF, DROP_EXISTING = OFF, ONLINE = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON)

```

![](./media/code-sample-export-sql-stream-analytics/34-create-table.png)

Bu örnekte, sayfa görüntüleme verileri kullanıyoruz. Kullanılabilir diğer verileri görmek için uygulamanızın JSON çıktısını inceleyin ve bkz [veri modelini dışarı aktarma](../../azure-monitor/app/export-data-model.md).

## <a name="create-an-azure-stream-analytics-instance"></a>Azure Stream Analytics örnek oluşturma
Gelen [Azure portalında](https://portal.azure.com/), Azure Stream Analytics hizmeti seçin ve yeni bir Stream Analytics işi oluşturun:

![Stream analytics ayarları](./media/code-sample-export-sql-stream-analytics/SA001.png)

![](./media/code-sample-export-sql-stream-analytics/SA002.png)

Yeni proje oluşturulduğunda seçin **kaynağa Git**.

![Stream analytics ayarları](./media/code-sample-export-sql-stream-analytics/SA003.png)

#### <a name="add-a-new-input"></a>Yeni giriş Ekle

![Stream analytics ayarları](./media/code-sample-export-sql-stream-analytics/SA004.png)

Sürekli dışarı aktarma blobunuz giriş gerçekleştirecek şekilde ayarlayın:

![Stream analytics ayarları](./media/code-sample-export-sql-stream-analytics/SA0005.png)

Şimdi, depolama, daha önce not ettiğiniz hesabınızdan, birincil erişim anahtarı gerekir. Bu depolama hesabı anahtarı ayarlayın.

#### <a name="set-path-prefix-pattern"></a>Set yol ön eki deseni

**YYYY-AA-GG tarih biçimini (kısa çizgi ile) ayarladığınızdan emin olun.**

Yol ön eki deseni Stream Analytics'e giriş dosyaları depolamada nasıl bulacağını belirler. Sürekli dışarı aktarma verileri nasıl depoladı karşılık olarak ayarlamanız gerekir. Şöyle ayarlayın:

    webapplication27_12345678123412341234123456789abcdef0/PageViews/{date}/{time}

Bu örnekte:

* `webapplication27` Application Insights kaynak adı **küçük da**. 
* `1234...` Application Insights kaynağına ait izleme anahtarı **kaldırıldı çizgilerle**. 
* `PageViews` veri çözümleme için istediğimiz türüdür. Kullanılabilir türler, sürekli dışarı aktarma ayarladığınız filtre bağlıdır. Diğer kullanılabilir türlerini görmek ve görmek için dışarı aktarılan verileri incelemek [veri modelini dışarı aktarma](../../azure-monitor/app/export-data-model.md).
* `/{date}/{time}` bir desen tam anlamıyla yazılır.

Application Insights kaynağınıza ait iKey ve adını almak için kendi genel bakış sayfasında Essentials açın veya ayarlarını açın.

> [!TIP]
> Örnek işlevi, Giriş yolu doğru şekilde ayarladığınızdan emin denetlemek için kullanın. Başarısız olursa: İlgili veri depolama alanı için seçtiğiniz örnek zaman aralığı içinde emin olun. Giriş tanımını düzenleyin ve depolama hesabı, yol ön eki ayarlayın ve tarih biçimi doğru denetleyin.

 
## <a name="set-query"></a>Sorgu kümesi
Sorgu bölümünü açın:

Varsayılan sorguyla değiştirin:

```SQL

    SELECT flat.ArrayValue.name as pageName
    , flat.ArrayValue.count as viewCount
    , flat.ArrayValue.url as url
    , flat.ArrayValue.urlData.port as urlDataPort
    , flat.ArrayValue.urlData.protocol as urlDataprotocol
    , flat.ArrayValue.urlData.host as urlDataHost
    , flat.ArrayValue.urlData.base as urlDataBase
    , flat.ArrayValue.urlData.hashTag as urlDataHashTag
      ,A.context.data.eventTime as eventTime
      ,A.context.data.isSynthetic as isSynthetic
      ,A.context.device.id as deviceId
      ,A.context.device.type as deviceType
      ,A.context.device.os as os
      ,A.context.device.osVersion as osVersion
      ,A.context.device.locale as locale
      ,A.context.device.userAgent as userAgent
      ,A.context.device.browser as browser
      ,A.context.device.browserVersion as browserVersion
      ,A.context.device.screenResolution.value as screenResolution
      ,A.context.session.id as sessionId
      ,A.context.session.isFirst as sessionIsFirst
      ,A.context.location.clientip as clientIp
      ,A.context.location.continent as continent
      ,A.context.location.country as country
      ,A.context.location.province as province
      ,A.context.location.city as city
    INTO
      AIOutput
    FROM AIinput A
    CROSS APPLY GetElements(A.[view]) as flat


```

İlk birkaç özelliği sayfa görüntüleme verileri için belirli olduğuna dikkat edin. Dışarı aktarmalar diğer telemetri türleri farklı özelliklere sahip olur. Bkz: [ayrıntılı özellik türleri ve değerleri için veri modeli başvurusu.](../../azure-monitor/app/export-data-model.md)

## <a name="set-up-output-to-database"></a>Çıkış veritabanı ayarlama
SQL çıktı olarak seçin.

![Stream analytics'te çıkışları seçin](./media/code-sample-export-sql-stream-analytics/SA006.png)

SQL veritabanı belirtin.

![Veritabanınızın ayrıntıları girin](./media/code-sample-export-sql-stream-analytics/SA007.png)

Sihirbazı kapatmak ve çıkış ayarlanmış olan bir bildirim bekler.

## <a name="start-processing"></a>İşlemini Başlat
Eylem çubuğu'ndan işi başlatın:

![Stream analytics'te Başlat'a tıklayın.](./media/code-sample-export-sql-stream-analytics/SA008.png)

Şimdi veya başlamak önceki veri başlatma verilerini işlemeye başlamak seçebilirsiniz. İkinci bir süre çalışmakta sürekli dışarı aktarma olsaydı yararlı olur.

Birkaç dakika sonra SQL Server Yönetim Araçları için geri dönün ve veri içinde akışını izleyin. Örneğin, bu gibi bir sorguda kullanın:

    SELECT TOP 100 *
    FROM [dbo].[PageViewsTable]


## <a name="related-articles"></a>İlgili makaleler
* [Stream Analytics kullanarak Power BI için dışarı aktarma](../../azure-monitor/app/export-power-bi.md )
* [Ayrıntılı veri özellik türleri ve değerleri için başvuru model.](../../azure-monitor/app/export-data-model.md)
* [Application Insights içinde sürekli dışarı aktarma](../../azure-monitor/app/export-telemetry.md)
* [Application Insights](https://azure.microsoft.com/services/application-insights/)

<!--Link references-->

[diagnostic]: ../../azure-monitor/app/diagnostic-search.md
[export]: ../../azure-monitor/app/export-telemetry.md
[metrics]: ../../azure-monitor/app/metrics-explorer.md
[portal]: https://portal.azure.com/
[start]: ../../azure-monitor/app/app-insights-overview.md

