---
title: Bir örnek ile kullanmaya başlama
description: Bu makalede, biz, Power BI çalışma koleksiyonları get başlatılan örnek eklemeniz.
services: power-bi-embedded
documentationcenter: ''
author: markingmyname
manager: kfile
editor: ''
tags: ''
ROBOTS: NOINDEX
ms.assetid: d8a9ef78-ad4e-4bc7-9711-89172dc5c548
ms.service: power-bi-embedded
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: powerbi
ms.date: 09/25/2017
ms.author: maghan
ms.openlocfilehash: 4daa675092bed1788025e7e3f7e7e11d02ef1671
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
ms.locfileid: "31414890"
---
# <a name="get-started-with-power-bi-workspace-collections-sample"></a>Power BI çalışma koleksiyonları örneği kullanmaya başlama

İle **Microsoft Power BI çalışma koleksiyonları**, Power BI raporları sağ web veya mobil uygulamaları tümleştirebilirsiniz. Bu makalede, biz size tanıtmak **Power BI çalışma koleksiyonları** get başlatılan örnek.

> [!IMPORTANT]
> Power BI Çalışma Alanı Koleksiyonları kullanım dışı bırakılmıştır ve Haziran 2018'e kadar veya anlaşmanızda belirtilen süre boyunca kullanılabilecektir. Uygulamanızda kesinti yaşanmaması için Power BI Embedded'a geçirmeyi planlamanız önerilir. Verilerinizi Power BI Embedded'a nasıl taşıyacağınızı öğrenmek için bkz. [Power BI Çalışma Alanı Koleksiyonları'nı Power BI Embedded'a geçirme](https://powerbi.microsoft.com/documentation/powerbi-developer-migrate-from-powerbi-embedded/).

Biz daha ilerlemeden önce aşağıdaki kaynaklara kaydetmek istediğiniz: Power BI raporları örnek uygulaması ve kendi uygulamalarınızı halinde çok tümleştirdiğinizde yardımcı olurlar.

* [Örnek çalışma web uygulaması](http://go.microsoft.com/fwlink/?LinkId=761493)
* [Power BI çalışma koleksiyonları API Başvurusu](https://msdn.microsoft.com/library/azure/mt711507.aspx)
* [Power BI .NET SDK'sı ](http://go.microsoft.com/fwlink/?LinkId=746472) (NuGet aracılığıyla kullanılabilir)
* [JavaScript rapor örnek ekleme](https://microsoft.github.io/PowerBI-JavaScript/demo)

> [!NOTE]
> En az bir oluşturmanıza gerek yapılandırabilirsiniz ve Power BI çalışma koleksiyonları alma çalışma başlatıldı örnek önce **çalışma alanı koleksiyonu** Azure aboneliğinizde. Nasıl oluşturulacağını öğrenmek için bir **çalışma alanı koleksiyonu** Azure portalında görmelerini [Power BI çalışma koleksiyonları ile çalışmaya başlama](get-started.md).

## <a name="configure-the-sample-app"></a>Örnek uygulamayı yapılandırma

Şimdi örnek uygulamayı çalıştırmak için gerekli bileşenleri erişmek için Visual Studio geliştirme ortamını ayarlama aracılığıyla yol.

1. İndirip sıkıştırmasını [Power BI çalışma koleksiyonlar - bir web uygulamasına bir rapor tümleştirmek](http://go.microsoft.com/fwlink/?LinkId=761493) github'da örnek.
2. Açık **Powerbı embedded.sln** Visual Studio. Yürütme gerekebilir **güncelleştirme paketini** bu çözümde kullanılan paketler güncelleştirmek için NuGet Paket Yöneticisi konsolunda komutu.
3. Çözümü derleyin.
4. Çalıştırma **ProvisionSample** konsol uygulaması. Örnek konsol uygulamasındaki bir çalışma alanı sağlamak ve PBIX dosyasını içeri aktarın.
5. Yeni bir sağlamak için **çalışma**, 1 seçeneğini belirleyin **koleksiyonu Yönetimi**ve ardından seçeneğini 6, **yeni bir çalışma alanı sağlanamadı**
6. Yeni bir almak için **rapor**, 2, seçeneğini belirleyin **rapor Yönetim**ve seçenek 3, ardından **PBIX Masaüstü içeri aktarma dosyası bir çalışma alanına**.

7. Girin, **çalışma alanı koleksiyonu** adı ve **erişim tuşu**. Bunlar alabileceğiniz **Azure portal**. Nasıl alınacağı hakkında daha fazla bilgi için **erişim tuşu**, bkz: [görünümü Power BI API'si erişim anahtarlarını](get-started.md#view-power-bi-api-access-keys) içinde Microsoft Power BI Embedded ile çalışmaya başlama.

    ![Azure portalındaki erişim tuşları](media/get-started-sample/azure-portal.png)
8. Kopyala ve yeni oluşturulan kaydedin **çalışma alanı kimliği** bu makalenin sonraki bölümlerinde kullanılacak. Sonra **çalışma alanı kimliği** olan oluşturulan, onu bulabilirsiniz **Azure portal**.

    ![Azure portalındaki çalışma alanı kimliği](media/get-started-sample/workspace-id.png)
9. Bir PBIX aktarmak için **çalışma**, seçeneğini belirleyin **6. İçeri aktarma PBIX Masaüstü dosyası var olan bir çalışma alanına**. Kullanışlı dosya PBIX yoksa, indirebilirsiniz [Retail Analysis Sample PBIX](http://go.microsoft.com/fwlink/?LinkID=780547).
10. İstenirse, için kolay bir ad girin, **Dataset**.

Benzer bir yanıt görmeniz gerekir:

```
Checking import state... Publishing
Checking import state... Succeeded
```

> [!NOTE]
> PBIX dosyanızın tüm doğrudan sorgu bağlantıları içeriyorsa, bağlantı dizelerini güncelleştirmek için 7 seçeneği çalıştırın.

Bu noktada, içeri aktarılan bir Power BI PBIX rapor vardır, **çalışma**. Şimdi çalıştırmak ne bakalım **Power BI çalışma koleksiyonları** başlatılan örnek web uygulamasını edinin.

## <a name="run-the-sample-web-app"></a>Örnek web uygulamasını çalıştırma

Web uygulaması örneği içeri aktarılan raporlar işleyen örnek bir uygulama olduğundan, **çalışma**. Web uygulaması örneği yapılandırma bırakılır.

1. İçinde **Powerbı katıştırılmış** Visual Studio çözümü sağ **EmbedSample** web uygulaması ve seçin **başlangıç projesi olarak ayarla**.
2. İçinde **web.config**, **EmbedSample** web uygulaması, düzenleme **appSettings**: **AccessKey**, **WorkspaceCollection** adı ve **Workspaceıd**.

    ```
    <appSettings>
        <add key="powerbi:AccessKey" value="" />
        <add key="powerbi:ApiUrl" value="https://api.powerbi.com" />
        <add key="powerbi:WorkspaceCollection" value="" />
        <add key="powerbi:WorkspaceId" value="" />
    </appSettings>
    ```
3. Çalıştırma **EmbedSample** web uygulaması.

Çalıştırdığınız sonra **EmbedSample** web uygulaması, sol gezinti bölmesinin içermelidir bir **raporları** menüsü. İçeri aktardığınız raporunu görüntülemek için Genişlet **raporları**ve bir rapora tıklayın. İçeri aktardığınız varsa [Retail Analysis Sample PBIX](http://go.microsoft.com/fwlink/?LinkID=780547), örnek web uygulaması şuna benzer:

![Örnek uygulama içinde örnek sol gezinti](media/get-started-sample/sample-left-nav.png)

Bir raporu tıklattıktan sonra **EmbedSample** web uygulaması görünmelidir bir şey:

![Örnek rapor uygulama içinde görüntüleme](media/get-started-sample/sample-web-app.png)

## <a name="explore-the-sample-code"></a>Örnek kod keşfetme

**Microsoft Power BI çalışma koleksiyonları** örnektir nasıl tümleştirileceği gösteren örnek bir web uygulaması **Power BI** uygulamanıza raporlar. Bir Model-View-Controller (MVC) tasarım deseni en iyi yöntemleri göstermek için kullanır. Bu bölüm içinde keşfedebilirsiniz örnek kod parçalarını vurgular **Powerbı katıştırılmış** web uygulaması çözümü. Etki alanı, sunu ve üç ayrı sınıfları kullanıcı girişine bağlı eylemleri modelleme Model-View-Controller (MVC) deseni ayırır: Model, Görünüm ve denetim. MVC hakkında daha fazla bilgi için bkz: [ASP.NET hakkında bilgi edinin](http://www.asp.net/mvc).

**Microsoft Power BI çalışma koleksiyonları** örnek kod gibi ayrılmış. Örnek kod kolayca bulabilmeniz için her bölüm Powerbı embedded.sln çözümde dosya adını içerir.

> [!NOTE]
> Bu bölümde, kodu nasıl yazılmıştır gösteren örnek kod bir özetidir. Tam örnek görüntülemek için lütfen Visual Studio Powerbı embedded.sln çözümde yükleyin.

### <a name="model"></a>Model

Örnek sahip bir **ReportsViewModel** ve **ReportViewModel**.

**ReportsViewModel.cs**: Power BI raporları temsil eder.

    public class ReportsViewModel
    {
        public List<Report> Reports { get; set; }
    }

**ReportViewModel.cs**: Power BI raporu temsil eder.

    public classReportViewModel
    {
        public IReport Report { get; set; }

        public string AccessToken { get; set; }
    }

### <a name="connection-string"></a>Bağlantı dizesi

Bağlantı dizesi şu biçimde olmalıdır:

```
Data Source=tcp:MyServer.database.windows.net,1433;Initial Catalog=MyDatabase
```

Ortak sunucu ve veritabanı kullanarak başarısız öznitelikleri. Örneğin: Server=tcp:MyServer.database.windows.net,1433;Database=MyDatabase,

### <a name="view"></a>Görünüm

**Görünüm** Power BI görüntüsünü yönetir **raporları** ve Power BI **rapor**.

**Reports.cshtml**: üzerinden yineleme **Model.Reports** oluşturmak için bir **ActionLink**. **ActionLink** şu şekilde oluşur:

| Bölümü | Açıklama |
| --- | --- |
| Başlık |Raporun adı. |
| Sorgu dizesi |Rapor Kimliği Bağla |

    <div id="reports-nav" class="panel-collapse collapse">
        <div class="panel-body">
            <ul class="nav navbar-nav">
                @foreach (var report in Model.Reports)
                {
                    var reportClass = Request.QueryString["reportId"] == report.Id ? "active" : "";
                    <li class="@reportClass">
                        @Html.ActionLink(report.Name, "Report", new { reportId = report.Id })
                    </li>
                }
            </ul>
        </div>
    </div>

Report.cshtml: Ayarlamak **Model.AccessToken**ve Lambda ifadesi **PowerBIReportFor**.

    @model ReportViewModel

    ...

    <div class="side-body padding-top">
        @Html.PowerBIAccessToken(Model.AccessToken)
        @Html.PowerBIReportFor(m => m.Report, new { style = "height:85vh" })
    </div>

### <a name="controller"></a>Denetleyici

**DashboardController.cs**: PowerBIClient geçirme oluşturur bir **uygulama belirteci**. Bir JSON Web Token (JWT) oluşturulur **imzalama anahtarı** almak için **kimlik bilgileri**. **Kimlik bilgileri** bir örneğini oluşturmak için kullanılan **PowerBIClient**. Örneği olduktan sonra **PowerBIClient**, GetReports() ve GetReportsAsync() çağırabilirsiniz.

CreatePowerBIClient()

    private IPowerBIClient CreatePowerBIClient()
    {
        var credentials = new TokenCredentials(accessKey, "AppKey");
        var client = new PowerBIClient(credentials)
        {
            BaseUri = new Uri(apiUrl)
        };

        return client;
    }

ActionResult Reports()

    public ActionResult Reports()
    {
        using (var client = this.CreatePowerBIClient())
        {
            var reportsResponse = client.Reports.GetReports(this.workspaceCollection, this.workspaceId);

            var viewModel = new ReportsViewModel
            {
                Reports = reportsResponse.Value.ToList()
            };

            return PartialView(viewModel);
        }
    }


Görev<ActionResult> rapor (dize reportId)

    public async Task<ActionResult> Report(string reportId)
    {
        using (var client = this.CreatePowerBIClient())
        {
            var reportsResponse = await client.Reports.GetReportsAsync(this.workspaceCollection, this.workspaceId);
            var report = reportsResponse.Value.FirstOrDefault(r => r.Id == reportId);
            var embedToken = PowerBIToken.CreateReportEmbedToken(this.workspaceCollection, this.workspaceId, report.Id);

            var viewModel = new ReportViewModel
            {
                Report = report,
                AccessToken = embedToken.Generate(this.accessKey)
            };

            return View(viewModel);
        }
    }

### <a name="integrate-a-report-into-your-app"></a>Uygulamanıza bir rapor tümleştirme

Bulduktan sonra bir **rapor**, kullandığınız bir **IFRAME** Power BI katıştırmak için **rapor**. İçinde powerbi.js gelen kod parçacığı aşağıda verilmiştir **Microsoft Power BI çalışma koleksiyonları** örnek.

```
init: function() {
    var embedUrl = this.getEmbedUrl();
    var iframeHtml = '<igrame style="width:100%;height:100%;" src="' + embedUrl + 
        '" scrolling="no" allowfullscreen="true"></iframe>';
    this.element.innerHTML = iframeHtml;
    this.iframe = this.element.childNodes[0];
    this.iframe.addEventListener('load', this.load.bind(this), false);
}
```

## <a name="filter-reports-embedded-in-your-application"></a>Uygulamanıza filtre raporları

Bir URL söz dizimini kullanarak eklenmiş bir raporu filtreleyebilirsiniz. Bunu yapmak için eklediğiniz bir **$filter** sorgu dizesi parametresi ile bir **eq** belirterek iFrame src URL'nize belirtilen filtre ile işleci. Filtre sorgu sözdizimi şöyledir:

```
https://app.powerbi.com/reportEmbed
?reportId=d2a0ea38-...-9673-ee9655d54a4a&
$filter={tableName/fieldName}%20eq%20'{fieldValue}'
```

> [!NOTE]
> {tableName/fieldName} boşluk ya da özel karakter içeremez. {fieldValue} tek bir kategorik değer kabul eder.  

## <a name="see-also"></a>Ayrıca bkz.

[Microsoft Power BI çalışma alanı koleksiyonu senaryoları](scenarios.md)  
[Power BI Çalışma Alanı Koleksiyonları ile kimlik doğrulaması ve yetkilendirme](app-token-flow.md)  
[Rapor ekleme](embed-report.md)  
[Veri kümesinden yeni rapor oluşturma](create-report-from-dataset.md)  
[Power BI Desktop](https://powerbi.microsoft.com/documentation/powerbi-desktop-get-the-desktop/)  
[JavaScript Örnek Ekleme](https://microsoft.github.io/PowerBI-JavaScript/demo/)  

Başka sorunuz mu var? [Power BI Topluluğu'nu deneyin](http://community.powerbi.com/)
