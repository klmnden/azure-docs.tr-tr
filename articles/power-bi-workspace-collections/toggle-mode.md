---
title: "Görünüm arasında geçiş yapmak ve düzenleme moduna raporlar Power BI çalışma koleksiyonlarda | Microsoft Docs"
description: "Görünüm arasında geçiş yapmak ve düzenleme moduna raporlarınız Power BI çalışma alanı koleksiyonu içinde hakkında bilgi edinin."
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
ms.openlocfilehash: e66778697f3f4f2f065d2757b3b60ac2699c0334
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="toggle-between-view-and-edit-mode-for-reports-in-power-bi-workspace-collections"></a>Görünüm arasında geçiş yapmak ve düzenleme moduna Power BI çalışma koleksiyonlarda raporlar için

Görünüm arasında geçiş yapmak ve düzenleme moduna raporlarınız Power BI çalışma alanı koleksiyonu içinde hakkında bilgi edinin.

> [!IMPORTANT]
> Power BI Çalışma Alanı Koleksiyonları kullanım dışı bırakılmıştır ve Haziran 2018'e kadar veya anlaşmanızda belirtilen süre boyunca kullanılabilecektir. Uygulamanızda kesinti yaşanmaması için Power BI Embedded'a geçirmeyi planlamanız önerilir. Verilerinizi Power BI Embedded'a nasıl taşıyacağınızı öğrenmek için bkz. [Power BI Çalışma Alanı Koleksiyonları'nı Power BI Embedded'a geçirme](https://powerbi.microsoft.com/documentation/powerbi-developer-migrate-from-powerbi-embedded/).

## <a name="creating-an-access-token"></a>Bir erişim belirteci oluşturma

Hem görüntülemek ve bir rapor düzenleme olanağı sağlayan bir erişim belirteci oluşturmanız gerekir. Düzenle ve raporu kaydetmek için ihtiyacınız **Report.ReadWrite** belirteci izni. Daha fazla bilgi için bkz: [Authenticating ve Power BI çalışma koleksiyonlarda yetkilendirme](app-token-flow.md).

> [!NOTE]
> Bu, düzenlemek ve var olan bir raporu için değişiklikleri kaydetmek sağlar. Ayrıca destekleme işlevi isterseniz **Kaydet**, ek izinler sağlamanız gerekir. Daha fazla bilgi için bkz: [kapsamları](app-token-flow.md#scopes).

```
using Microsoft.PowerBI.Security;

// rlsUsername and roles are optional
string scopes = "Report.ReadWrite";
PowerBIToken embedToken = PowerBIToken.CreateReportEmbedTokenForCreation(workspaceCollectionName, workspaceId, datasetId, null, null, scopes);

var token = embedToken.Generate("{access key}");
```

## <a name="embed-configuration"></a>Yapılandırma katıştırma

İzinler ve bir viewMode kaydetme görmeniz için sağlamanız gereken düğmesini düzenleme modunda. Daha fazla bilgi için bkz: [yapılandırma ayrıntılarını katıştırmak](https://github.com/Microsoft/PowerBI-JavaScript/wiki/Embed-Configuration-Details).

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
        permissions: models.Permissions.ReadWrite /*both save & save as buttons will be visible*/,
        viewMode: models.ViewMode.View,
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

Bu temel görünüm modunda rapor eklemek için gösterir **viewMode** ayarlamak **modeller. ViewMode.View**.

## <a name="view-mode"></a>Görünüm modu

Görünüm moduna geçmek için içinde olup olmadığını düzenleme modu aşağıdaki JavaScript kullanabilirsiniz.

```
// Get a reference to the embedded report HTML element
var reportContainer = $('#reportContainer')[0];

// Get a reference to the embedded report.
report = powerbi.get(reportContainer);

// Switch to view mode.
report.switchMode("view");

```

## <a name="edit-mode"></a>Düzenleme modu

Görünüm modu değilseniz düzenleme moduna geçmek için aşağıdaki JavaScript kullanabilirsiniz.

```
// Get a reference to the embedded report HTML element
var reportContainer = $('#reportContainer')[0];

// Get a reference to the embedded report.
report = powerbi.get(reportContainer);

// Switch to edit mode.
report.switchMode("edit");

```

## <a name="see-also"></a>Ayrıca bkz.

[Bir örnek ile kullanmaya başlama](get-started-sample.md)  
[Rapor ekleme](embed-report.md)  
[Power BI Çalışma Alanı Koleksiyonları ile kimlik doğrulaması ve yetkilendirme](app-token-flow.md)  
[CreateReportEmbedToken](https://docs.microsoft.com/dotnet/api/microsoft.powerbi.security.powerbitoken?redirectedfrom=MSDN#methods_)  
[JavaScript Örnek Ekleme](https://microsoft.github.io/PowerBI-JavaScript/demo/)  
[Powerbı CSharp Git deposu](https://github.com/Microsoft/PowerBI-CSharp)  
[Powerbı düğümlü Git deposu](https://github.com/Microsoft/PowerBI-Node)  

Başka sorunuz mu var? [Power BI Topluluğu'nu deneyin](http://community.powerbi.com/)
