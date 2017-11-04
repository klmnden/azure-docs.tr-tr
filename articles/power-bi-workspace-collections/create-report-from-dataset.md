---
title: "Power BI çalışma koleksiyonlarda kümesinden yeni rapor oluşturma | Microsoft Docs"
description: "Power BI çalışma alanı koleksiyonu raporları, kendi uygulamanızda kümesinden şimdi oluşturulabilir."
services: power-bi-embedded
documentationcenter: 
author: guyinacube
manager: erikre
editor: 
tags: 
ROBOTS: NOINDEX
ms.assetid: 
ms.service: power-bi-embedded
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: powerbi
ms.date: 09/20/2017
ms.author: asaxton
ms.openlocfilehash: aa902cbc4992292420948b36d85e52fafc7224de
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="create-a-new-report-from-a-dataset-in-power-bi-workspace-collections"></a>Power BI çalışma koleksiyonlarda kümesinden yeni rapor oluşturma

Power BI çalışma alanı koleksiyonu raporları, kendi uygulamanızda kümesinden şimdi oluşturulabilir.

> [!IMPORTANT]
> Power BI Çalışma Alanı Koleksiyonları kullanım dışı bırakılmıştır ve Haziran 2018'e kadar veya anlaşmanızda belirtilen süre boyunca kullanılabilecektir. Uygulamanızda kesinti yaşanmaması için Power BI Embedded'a geçirmeyi planlamanız önerilir. Verilerinizi Power BI Embedded'a nasıl taşıyacağınızı öğrenmek için bkz. [Power BI Çalışma Alanı Koleksiyonları'nı Power BI Embedded'a geçirme](https://powerbi.microsoft.com/documentation/powerbi-developer-migrate-from-powerbi-embedded/).

Kimlik doğrulama yöntemi, bir rapor katıştırma benzer. Belirli bir veri kümesine erişim belirteçleri dayanır. Belirteçleri PowerBI.com için kullanılan Azure Active Directory (AAD tarafından) verilir. Power BI çalışma alanı koleksiyonu belirteçleri, kendi uygulama tarafından verilir.

Katıştırılmış Rapor oluştururken, yayınlanan belirteçleri belirli bir veri kümesi için ' dir. Belirteçleri her benzersiz belirtece sahip olmak için embed URL ile aynı öğede ilişkili olmalıdır. Ekli raporu oluşturmak için *Dataset.Read ve Workspace.Report.Create* kapsamları erişim belirtecinde yer sağlanmalıdır.

## <a name="create-access-token-needed-to-create-new-report"></a>Yeni bir rapor oluşturmak için gereken erişim belirteci oluşturma

Power BI çalışma koleksiyonları kullanın bir ekleme belirteci, HMAC olduğu JSON Web belirteçlerini imzalanmış. Belirteçleri, Power BI çalışma alanı koleksiyonu erişim anahtarla imzalanmıştır. Varsayılan olarak belirteçleri, ekleme, bir uygulamaya eklemek için bir rapor salt okunur erişim sağlamak için kullanılır. Embed belirteçleri için belirli bir rapor verilir ve bir embed URL'si ile ilişkili olmalıdır.

Erişim tuşları belirteçleri oturum/şifrelemek için kullanılan erişim belirteçleri sunucuda oluşturulması gerekir. Bir erişim belirteci oluşturma hakkında daha fazla bilgi için bkz: [Authenticating ve Power BI çalışma koleksiyonlarla yetkilendirme](app-token-flow.md). Ayrıca gözden geçirebilirsiniz [CreateReportEmbedToken](https://docs.microsoft.com/dotnet/api/microsoft.powerbi.security.powerbitoken?redirectedfrom=MSDN#methods_) yöntemi. Burada, ne için Power BI .NET SDK kullanarak gibi görünür bir örnek verilmiştir.

Bu örnekte, yeni raporu oluşturmak için istiyoruz bizim veri kümesi Tanıtıcısı sahip. Biz de eklemeniz gerekir *Dataset.Read ve Workspace.Report.Create*.

*PowerBIToken sınıfı* , yüklemenizi gerektirir [Power BI çekirdek NuGut paketi](https://www.nuget.org/packages/Microsoft.PowerBI.Core/).

**NuGet paketi yüklemesi**

```
Install-Package Microsoft.PowerBI.Core
```

**C# kodu**

```
using Microsoft.PowerBI.Security;

// rlsUsername and roles are optional
string scopes = "Dataset.Read Workspace.Report.Create";
PowerBIToken embedToken = PowerBIToken.CreateReportEmbedTokenForCreation(workspaceCollectionName, workspaceId, datasetId, null, null, scopes);

var token = embedToken.Generate("{access key}");
```

## <a name="create-a-new-blank-report"></a>Yeni boş bir rapor oluşturun

Yeni bir rapor oluşturmak için Oluştur yapılandırma sağlanmalıdır. Bu erişim belirteci, embedURL ve karşı raporu oluşturmak için istiyoruz Datasetıd içermelidir. Bu, nuget yüklemenizi gerektirir [Power BI JavaScript paket](https://www.nuget.org/packages/Microsoft.PowerBI.JavaScript/). EmbedUrl yalnızca https://embedded.powerbi.com/appTokenReportEmbed olacaktır.

> [!NOTE]
> Kullanabileceğiniz [JavaScript rapor katıştırmak örnek](https://microsoft.github.io/PowerBI-JavaScript/demo/) işlevselliğini test etmek için. Aynı zamanda kullanılabilir farklı işlemler için kod örnekleri sağlar.

**NuGet paketi yüklemesi**

```
Install-Package Microsoft.PowerBI.JavaScript
```

**JavaScript kodu**

```
<div id="reportContainer"></div>
  
var embedCreateConfiguration = {
        accessToken: 'eyJ0eXAiO...Qron7qYpY9MI',
        embedUrl: 'https://embedded.powerbi.com/appTokenReportEmbed',
        datasetId: '5dac7a4a-4452-46b3-99f6-a25915e0fe55',
    };
    
    // Grab the reference to the div HTML element that will host the report
    var reportContainer = $('#reportContainer')[0];

    // Create report
    var report = powerbi.createReport(reportContainer, embedCreateConfiguration);
```

Çağırma *powerbi.createReport()* içinde görünür boş bir tuval düzenleme modunda yapar *div* öğesi.

![Yeni boş raporu](media/create-report-from-dataset/create-new-report.png)

## <a name="save-new-reports"></a>Yeni raporlar Kaydet

Çağırmanız kadar rapor oluşturulmaz **Farklı Kaydet** işlemi. Bu dosya menüsünden veya JavaScript yapılabilir.

```
 // Get a reference to the embedded report.
    report = powerbi.get(reportContainer);
    
    var saveAsParameters = {
        name: "newReport"
    };

    // SaveAs report
    report.saveAs(saveAsParameters);
```

> [!IMPORTANT]
> Yeni bir rapor yalnızca sonra oluşturulan **Farklı Kaydet** olarak adlandırılır. Kaydettikten sonra tuvale düzenleme modunu ve rapor hala veri kümesini gösterir. Herhangi bir raporu gibi yeni raporu yeniden yüklemeniz gerekir.

![Dosya menüsü - Kaydet olarak](media/create-report-from-dataset/save-new-report.png)

## <a name="load-the-new-report"></a>Yeni rapor yükleyin

Yani normal bir rapor uygulama katıştırır aynı şekilde katıştırmak gerek yeni raporuyla etkileşim kurmak için yeni bir belirteç özellikle yeni rapor için verilmiş olması gerekir ve ardından ekleme yöntemini çağırın.

```
<div id="reportContainer"></div>
  
var embedConfiguration = {
        accessToken: 'eyJ0eXAiO...Qron7qYpY9MJ',
        embedUrl: 'https://embedded.powerbi.com/appTokenReportEmbed',
        reportId: '5dac7a4a-4452-46b3-99f6-a25915e0fe54',
    };
    
    // Grab the reference to the div HTML element that will host the report
    var reportContainer = $('#reportContainer')[0];

    // Embed report
    var report = powerbi.embed(reportContainer, embedConfiguration);
```

## <a name="automate-save-and-load-of-a-new-report-using-the-saved-event"></a>Kaydetme otomatikleştirmek ve "kaydedildi" olayını kullanarak yeni bir rapor yükleme

"Farklı Kaydet" sürecini otomatik hale getirmek için ve yeni rapor yüklenirken "kaydedildi" olayının kullanabilir. Bu olay zaman ateşlenir kaydetme işlemi tamamlandıktan ve yeni reportId, rapor adı, eski reportId (varsa) içeren bir Json nesnesi döndürür ve Farklı Kaydet işlemi ise veya kaydedin.

```
{
  "reportObjectId": "5dac7a4a-4452-46b3-99f6-a25915e0fe54",
  "reportName": "newReport",
  "saveAs": true,
  "originalReportObjectId": null
}
```

"Kaydedildi" olay üzerinde dinleme işlemini otomatikleştirmek için yeni reportId Al, yeni bir belirteç oluşturmak ve yeni rapor ile ekleme.

```
<div id="reportContainer"></div>
  
var embedCreateConfiguration = {
        accessToken: 'eyJ0eXAiO...Qron7qYpY9MI',
        embedUrl: 'https://embedded.powerbi.com/appTokenReportEmbed',
        datasetId: '5dac7a4a-4452-46b3-99f6-a25915e0fe55',
    };
    
    // Grab the reference to the div HTML element that will host the report
    var reportContainer = $('#reportContainer')[0];

    // Create report
    var report = powerbi.createReport(reportContainer, embedCreateConfiguration);


   var saveAsParameters = {
        name: "newReport"
    };

    // SaveAs report
    report.saveAs(saveAsParameters);

    // report.on will add an event handler which prints to Log window.
    report.on("saved", function(event) {
        
         // get new Token
         var newReportId =  event.detail.reportObjectId;

        // create new Token. This is a function that the application should provide
        var newToken = createAccessToken(newReportId,scopes /*provide the wanted scopes*/);
        
        
    var embedConfiguration = {
        accessToken: newToken ,
        embedUrl: 'https://embedded.powerbi.com/appTokenReportEmbed',
        reportId: newReportId,
    };

    // Embed report
    var report = powerbi.embed(reportContainer, embedConfiguration);
       
   // report.off removes a given event handler if it exists.
   report.off("saved");
    });
```

## <a name="see-also"></a>Ayrıca bkz.

[Bir örnek ile kullanmaya başlama](get-started-sample.md)  
[Raporları kaydetme](save-reports.md)  
[Rapor ekleme](embed-report.md)  
[Power BI Çalışma Alanı Koleksiyonları ile kimlik doğrulaması ve yetkilendirme](app-token-flow.md)  
[Power BI Desktop](https://powerbi.microsoft.com/documentation/powerbi-desktop-get-the-desktop/)  
[JavaScript Örnek Ekleme](https://microsoft.github.io/PowerBI-JavaScript/demo/)  
[Power BI çekirdek NuGut paketi](https://www.nuget.org/packages/Microsoft.PowerBI.Core/)  
[Power BI JavaScript paketi](https://www.nuget.org/packages/Microsoft.PowerBI.JavaScript/)  

Başka sorunuz mu var? [Power BI Topluluğu'nu deneyin](http://community.powerbi.com/)