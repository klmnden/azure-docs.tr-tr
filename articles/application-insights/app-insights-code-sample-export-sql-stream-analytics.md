---
title: Dışarı aktarmak için SQL Azure Application Insights | Microsoft Docs
description: Sürekli olarak Application Insights verileri kullanarak Stream Analytics SQL verin.
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.assetid: 48903032-2c99-4987-9948-d6e4559b4a63
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: conceptual
ms.date: 03/06/2015
ms.author: mbullwin
ms.openlocfilehash: 7d4bf0c5beeba22569e0000b28b007fb5b0ff68f
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/11/2018
ms.locfileid: "35294185"
---
# <a name="walkthrough-export-to-sql-from-application-insights-using-stream-analytics"></a>İzlenecek yol: Uygulama kullanarak Stream Analytics ilişkin bilgiler için SQL dışarı aktarma
Bu makalede, telemetri verilerini taşıma gösterilmektedir [Azure Application Insights] [ start] kullanarak bir Azure SQL veritabanına [sürekli verme] [ export] ve [Azure akış analizi](https://azure.microsoft.com/services/stream-analytics/). 

Sürekli verme telemetri verileri JSON biçiminde Azure depolama alanına taşır. Biz Azure akış analizi kullanarak JSON nesneleri ayrıştırabilir ve bir veritabanı tablosundaki satırları oluşturmak.

(Daha genellikle sürekli verme kendi uygulamalarınızı Application Insights'a gönderme telemetri analizi yapmak için yoludur. Dışarı aktarılan telemetriyle veri toplama gibi başka işlemler yapmak için bu kod örneği uyarlayabilirsiniz.)

İzlemek istediğiniz uygulamayı zaten sahip varsayımıyla başlayacağız.

Bu örnekte, biz sayfası görünüm verilerini kullanarak, ancak aynı düzeni özel etkinlikler ve özel durumlar gibi diğer veri türleri için kolayca genişletilebilir. 

## <a name="add-application-insights-to-your-application"></a>Uygulamanıza Application Insights ekleyin
Kullanmaya başlamak için:

1. [Web sayfalarınız için Application Insights'ı ayarlamak](app-insights-javascript.md). 
   
    (Bu örnekte, istemci tarayıcıları sayfa görünümü verileri işleme biz odaklanmak, ancak Application Insights'ı için sunucu tarafı ayarlayabilir, [Java](app-insights-java-get-started.md) veya [ASP.NET](app-insights-asp-net.md) uygulama ve istek işleme, bağımlılık ve diğer sunucu telemetri.)
2. Uygulamanızı yayımlayın ve telemetri verilerini Application Insights kaynağınıza görünmesini izleyin.

## <a name="create-storage-in-azure"></a>Depolama oluşturma
Depolama oluşturmanız gerekir böylece sürekli verme verileri Azure depolama hesabı her zaman çıkarır.

1. Aboneliğinizde depolama hesabı oluşturma [Azure portal][portal].
   
    ![Azure portalında yeni, verileri depolama birimi seçin. Klasik seçin, oluşturun. Depolama adı sağlayın.](./media/app-insights-code-sample-export-sql-stream-analytics/040-store.png)
2. Bir kapsayıcı oluşturma
   
    ![Yeni depolama kapsayıcıları seçin, kapsayıcıları döşeme tıklatın ve ardından Ekle](./media/app-insights-code-sample-export-sql-stream-analytics/050-container.png)
3. Depolama erişim tuşu kopyalama
   
    Akış analizi hizmetine giriş yakında ayarlamak için gerekir.
   
    ![Depolama ayarlarını, anahtarları, açın ve birincil erişim tuşunu bir kopyasını alın](./media/app-insights-code-sample-export-sql-stream-analytics/21-storage-key.png)

## <a name="start-continuous-export-to-azure-storage"></a>Azure depolama alanına sürekli verme Başlat
1. Azure portalında, uygulamanız için oluşturduğunuz Application Insights kaynağı göz atın.
   
    ![Gözat, Application Insights, uygulamanızı seçin](./media/app-insights-code-sample-export-sql-stream-analytics/060-browse.png)
2. Sürekli verme oluşturun.
   
    ![Ayarları, sürekli verme ekleme seçin](./media/app-insights-code-sample-export-sql-stream-analytics/070-export.png)

    Daha önce oluşturduğunuz depolama hesabı seçin:

    ![Dışa aktarma hedefi ayarlama](./media/app-insights-code-sample-export-sql-stream-analytics/080-add.png)

    Görmek istediğiniz olay türlerini ayarlayın:

    ![Olay türlerini seçin](./media/app-insights-code-sample-export-sql-stream-analytics/085-types.png)


1. Accumulate bazı veriler sağlar. Arkanıza yaslanın ve kişilere uygulamanızın biraz kullanın. Telemetri geldikçe ve istatistiksel grafiklerde görürsünüz [ölçüm Gezgini](app-insights-metrics-explorer.md) ve olayları tek tek [tanılama arama](app-insights-diagnostic-search.md). 
   
    Ve ayrıca, verileri depolama alanınızın dışa aktarır. 
2. Dışarı aktarılan verileri, ya da portal - Seç **Gözat**, depolama hesabınızı seçin ve ardından **kapsayıcıları** - ya da Visual Studio. Visual Studio'da, **görüntülemek / Cloud Explorer**ve Azure açın / depolama. (Bu menü seçeneği yoksa, Azure SDK'yı yüklemeniz gerekir: Yeni Proje iletişim kutusunu açın ve Visual C# açın / bulut / .NET için Microsoft Azure SDK'sını alın.)
   
    ![Visual Studio'da açın Server Browser, Azure, depolama](./media/app-insights-code-sample-export-sql-stream-analytics/087-explorer.png)
   
    Uygulama adı ve araçları anahtarından türetilen yol adı, ortak parçası not edin. 

JSON biçiminde dosyaları blob yazılır. Her dosya bir veya daha fazla olaylar içerebilir. Bu nedenle olay verileri okumak ve istiyoruz alanları filtrelemek isteriz. Her türlü verilerle yapabileceğimiz bir şey vardır, ancak bizim planı bugün Stream Analytics verileri bir SQL veritabanını taşımak için kullanmaktır. Bu, çok sayıda ilginç sorguları çalıştırmak kolay hale getirir.

## <a name="create-an-azure-sql-database"></a>Bir Azure SQL veritabanı oluşturma
Bir kez daha, aboneliğinizde başlayarak [Azure portal][portal], veritabanı oluşturma (ve yeni bir sunucu zaten bir olduğuna sürece) verileri yazacak şekilde.

![Yeni, verileri, SQL](./media/app-insights-code-sample-export-sql-stream-analytics/090-sql.png)

Veritabanı sunucusu Azure hizmetlerine erişime izin verdiğinden emin olun:

![Gözat, sunucuları, sunucunuz, ayarları, güvenlik duvarı, Azure erişime izin ver](./media/app-insights-code-sample-export-sql-stream-analytics/100-sqlaccess.png)

## <a name="create-a-table-in-azure-sql-db"></a>Azure SQL DB tablosu oluşturma
Tercih edilen Yönetim aracınızla önceki bölümde oluşturulan veritabanına bağlanın. Bu kılavuzda, biz kullanacağınız [SQL Server Yönetim Araçları](https://msdn.microsoft.com/ms174173.aspx) (SSMS).

![](./media/app-insights-code-sample-export-sql-stream-analytics/31-sql-table.png)

Yeni bir sorgu oluşturun ve aşağıdaki T-SQL Yürüt:

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

![](./media/app-insights-code-sample-export-sql-stream-analytics/34-create-table.png)

Bu örnekte, sayfa görünümleri verilerden kullanıyoruz. Kullanılabilir diğer verileri görmek için JSON çıktısını inceleyin ve bakın [veri modeli verme](app-insights-export-data-model.md).

## <a name="create-an-azure-stream-analytics-instance"></a>Bir Azure akış analizi örneği oluşturma
Gelen [Azure portal](https://portal.azure.com/), Azure Stream Analytics hizmeti seçin ve yeni bir Stream Analytics işi oluştur:

![](./media/app-insights-code-sample-export-sql-stream-analytics/SA001.png)

![](./media/app-insights-code-sample-export-sql-stream-analytics/SA002.png)

Yeni iş oluşturulduğunda seçin **kaynağa gidin**.

![](./media/app-insights-code-sample-export-sql-stream-analytics/SA003.png)

#### <a name="add-a-new-input"></a>Yeni giriş Ekle

![](./media/app-insights-code-sample-export-sql-stream-analytics/SA004.png)

Sürekli verme blobundan giriş gerçekleştirecek şekilde ayarlayın:

![](./media/app-insights-code-sample-export-sql-stream-analytics/SA005.png)

Şimdi, depolama, daha önce not ettiğiniz hesabınızdan, birincil erişim anahtarı gerekir. Bu depolama hesabı anahtarı ayarlayın.

#### <a name="set-path-prefix-pattern"></a>Set yol önek deseni

**Tarih biçimi YYYY-AA-GG (tire ile) ayarladığınızdan emin olun.**

Önek yol deseni Stream Analytics girdi dosyaları depolama alanına nasıl bulacağını belirler. Bunu sürekli verme verileri nasıl depolar karşılık gelecek şekilde ayarlamanız gerekir. Aşağıdaki gibi ayarlayın:

    webapplication27_12345678123412341234123456789abcdef0/PageViews/{date}/{time}

Bu örnekte:

* `webapplication27` Application Insights kaynağı adı **tüm alt durumda**. 
* `1234...` Application Insights kaynağı izleme anahtarını olan **kaldırılan çizgilerle**. 
* `PageViews` Analiz etmek istiyoruz veri türüdür. Kullanılabilir türler filtresi sürekli dışarı aktarma ile Ayarla bağlıdır. Diğer kullanılabilir türleri görmek ve görmek için dışarı aktarılan verileri incelemek [veri modeli verme](app-insights-export-data-model.md).
* `/{date}/{time}` bir desen tam anlamıyla yazılır.

Application Insights kaynağınıza iKey ve adını almak için genel bakış sayfasında Essentials açın veya ayarları'nı açın.

> [!TIP]
> Giriş yolu doğru şekilde ayarladığınızdan emin denetlemek için örnek işlevini kullanın. Başarısız olursa: olup olmadığını veri depolama alanı için seçtiğiniz örnek zaman aralığı içinde denetleyin. Giriş tanımı düzenleyin ve ayarlayın depolama hesabı, yol öneki ve tarih biçimi doğru denetleyin.
> 
> 
## <a name="set-query"></a>Set sorgu
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

İlk birkaç Özellikleri Sayfası görünüm verilerini belirli olduğuna dikkat edin. Dışarı aktarma diğer telemetri türlerinin farklı özelliklere sahip olur. Bkz: [ayrıntılı özellik türleri ve değerleri için veri modeli başvurusu.](app-insights-export-data-model.md)

## <a name="set-up-output-to-database"></a>Veritabanı için çıktı ayarlama
SQL çıktısı olarak seçin.

![Çıkış akış analizleri seçin](./media/app-insights-code-sample-export-sql-stream-analytics/SA006.png)

SQL veritabanı belirtin.

![Veritabanınızın ayrıntıları doldurun](./media/app-insights-code-sample-export-sql-stream-analytics/SA007.png)

Sihirbazı kapatmak ve çıktı ayarlanmış bir bildirim bekler.

## <a name="start-processing"></a>İşlemini Başlat
İş eylemi çubuğundan başlatın:

![Akış analizi Başlat'ı tıklatın.](./media/app-insights-code-sample-export-sql-stream-analytics/SA008.png)

Şimdi veya başlamak eski veri başlatmayı veri işlemeye başlaması seçebilirsiniz. İkincisi verme sürekli bir süredir çalışıyor olsaydı yararlıdır.

Birkaç dakika sonra SQL Server Yönetim Araçları için geri dönün ve içinde akan verilere izleyin. Örneğin, şöyle bir sorguyu kullanın:

    SELECT TOP 100 *
    FROM [dbo].[PageViewsTable]


## <a name="related-articles"></a>İlgili makaleler
* [Akış analizi kullanarak Powerbı dışarı aktarma](app-insights-export-power-bi.md)
* [Ayrıntılı veri özellik türleri ve değerleri için başvuru modeli.](app-insights-export-data-model.md)
* [Application ınsights'ta sürekli dışarı aktarma](app-insights-export-telemetry.md)
* [Application Insights](https://azure.microsoft.com/services/application-insights/)

<!--Link references-->

[diagnostic]: app-insights-diagnostic-search.md
[export]: app-insights-export-telemetry.md
[metrics]: app-insights-metrics-explorer.md
[portal]: http://portal.azure.com/
[start]: app-insights-overview.md

