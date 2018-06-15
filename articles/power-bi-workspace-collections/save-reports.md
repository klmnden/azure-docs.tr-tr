---
title: Power BI çalışma koleksiyonlarda raporları kaydetmek | Microsoft Docs
description: Power BI çalışma alanı koleksiyonu içinde raporları kaydetmek öğrenin. Bu, başarılı bir şekilde çalışması için uygun izinleri gerektirir.
services: power-bi-embedded
documentationcenter: ''
author: markingmyname
manager: kfile
editor: ''
tags: ''
ROBOTS: NOINDEX
ms.assetid: ''
ms.service: power-bi-embedded
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: powerbi
ms.date: 09/20/2017
ms.author: maghan
ms.openlocfilehash: c5512584531c9f5c8a13e9a50161eb6b5a1f8a7b
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
ms.locfileid: "31411225"
---
# <a name="save-reports-in-power-bi-workspace-collections"></a>Raporlar Power BI çalışma koleksiyonlarda Kaydet

Power BI çalışma alanı koleksiyonu içinde raporları kaydetmek öğrenin. Raporları kaydetme başarılı bir şekilde çalışması için uygun izinleri gerektirir.

> [!IMPORTANT]
> Power BI Çalışma Alanı Koleksiyonları kullanım dışı bırakılmıştır ve Haziran 2018'e kadar veya anlaşmanızda belirtilen süre boyunca kullanılabilecektir. Uygulamanızda kesinti yaşanmaması için Power BI Embedded'a geçirmeyi planlamanız önerilir. Verilerinizi Power BI Embedded'a nasıl taşıyacağınızı öğrenmek için bkz. [Power BI Çalışma Alanı Koleksiyonları'nı Power BI Embedded'a geçirme](https://powerbi.microsoft.com/documentation/powerbi-developer-migrate-from-powerbi-embedded/).

Power BI çalışma alanı koleksiyonu içinde varolan raporları düzenleyebilir ve kaydedebilirsiniz. Ayrıca, yeni bir rapor oluşturmak ve oluşturmak için yeni bir rapor olarak kaydedin.

Bir raporu kaydetmek için önce belirli bir rapor için bir belirteç sağ kapsamlarla oluşturmanız gerekir:

* Report.ReadWrite etkinleştirmek için kapsam gereklidir
* Kaydetme etkinleştirmek için Report.Read ve Workspace.Report.Copy kapsamları gereklidir
* Report.ReadWrite ve Workspace.Report.Copy gerektiği şekilde, kaydetme ve kaydetme etkinleştirmek için

Sırasıyla sağ etkinleştirmek için raporun katıştırma Embed yapılandırmasında doğru iznin sağlamanız gereken dosya menüsü düğmeleri farklı kaydet/Kaydet:

* modeller. Permissions.ReadWrite
* modeller. Permissions.Copy
* modeller. Permissions.All

> [!NOTE]
> Erişim belirteci uygun kapsamları da gerekir. Daha fazla bilgi için bkz: [kapsamları](app-token-flow.md#scopes).

## <a name="embed-report-in-edit-mode"></a>Rapor düzenleme modunda ekleme

Bu nedenle yalnızca Embed yapılandırmasında hakkı özellikleri geçirmek ve powerbi.embed() çağırmak için uygulamanızın içinde düzenleme modunda bir rapor eklemek istediğiniz varsayalım. İzinler ve kaydetme bakın ve düğmeleri düzenleme modunda olarak kaydetmek için bir viewMode sağlayın. Daha fazla bilgi için bkz: [yapılandırma ayrıntılarını katıştırmak](https://github.com/Microsoft/PowerBI-JavaScript/wiki/Embed-Configuration-Details).

Örneğin, JavaScript içinde:

```
   <div id="reportContainer"></div>

    // Get models. Models, it contains enums that can be used.
    var models = window['powerbi-client'].models;

    // Embed configuration used to describe the what and how to embed.
    // This object is used when calling powerbi.embed.
    // This also includes settings and options such as filters.
    // You can find more information at https://github.com/Microsoft/PowerBI-JavaScript/wiki/Embed-Configuration-Details.
    var config= {
        type: 'report',
        accessToken: 'eyJ0eXAiO...Qron7qYpY9MI',
        embedUrl: 'https://embedded.powerbi.com/appTokenReportEmbed',
        id:  '5dac7a4a-4452-46b3-99f6-a25915e0fe55',
        permissions: models.Permissions.All /*both save & save as buttons will be visible*/,
        viewMode: models.ViewMode.Edit,
        settings: {
            filterPaneEnabled: true,
            navContentPaneEnabled: true
        }
    };

    // Get a reference to the embedded report HTML element
    var reportContainer = $('#reportContainer')[0];

    // Embed the report and display it within the div container.
    var report = powerbi.embed(reportContainer, config);
```

Şimdi bir rapor düzenleme modunda, uygulamanızda katıştırılır.

## <a name="save-report"></a>Raporu kaydetme

Rapor düzenleme modunda izinleri ve doğru belirteci ile katıştırma sonra raporu Dosya menüsünden veya javascript kaydedebilirsiniz:

```
 // Get a reference to the embedded report.
    report = powerbi.get(reportContainer);

 // Save report
    report.save();
```

## <a name="save-as"></a>Farklı kaydet

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
> Yalnızca sonra *Farklı Kaydet* oluşturulan yeni bir rapor. Kaydetme sonra tuvale hala eski rapor düzenleme modunu ve yeni rapor göstermez. Oluşturulan yeni rapor ekleme. Rapor oluşturuldukça yeni rapor katıştırma yeni bir erişim belirteci gerektirir.

Ardından yeni raporun sonra Yük gerekir bir *Farklı Kaydet*. Yeni rapor yüklenirken, herhangi bir raporu katıştırmak için benzer.

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

## <a name="see-also"></a>Ayrıca bkz.

[Bir örnek ile kullanmaya başlama](get-started-sample.md)  
[Rapor ekleme](embed-report.md)  
[Veri kümesinden yeni rapor oluşturma](create-report-from-dataset.md)  
[Power BI Çalışma Alanı Koleksiyonları ile kimlik doğrulaması ve yetkilendirme](app-token-flow.md)  
[Power BI Desktop](https://powerbi.microsoft.com/documentation/powerbi-desktop-get-the-desktop/)  
[JavaScript Örnek Ekleme](https://microsoft.github.io/PowerBI-JavaScript/demo/)  

Başka sorunuz mu var? [Power BI Topluluğu'nu deneyin](http://community.powerbi.com/)

