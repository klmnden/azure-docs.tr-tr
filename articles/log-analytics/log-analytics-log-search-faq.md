---
title: "Günlük analizi yeni günlük arama sık sorulan sorular | Microsoft Docs"
description: "Bu makale sık sorulan soruları yükseltilmesiyle ilgili günlük analizi, yeni sorgu dili sağlar."
services: log-analytics
documentationcenter: 
author: bwren
manager: carmonm
editor: 
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/19/2018
ms.author: bwren
ms.openlocfilehash: 6dfee26d7585c8ec295a1f0ea1bd0bc14a34cc5a
ms.sourcegitcommit: 1fbaa2ccda2fb826c74755d42a31835d9d30e05f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/22/2018
---
# <a name="log-analytics-new-log-search-faq-and-known-issues"></a>Yeni günlük analizi oturum arama SSS ve bilinen sorunlar

Bu makalede sık sorulan sorular ve yükseltilmesiyle ilgili bilinen sorunları içerir [yeni sorgu dili için günlük analizi](log-analytics-log-search-upgrade.md).  Çalışma alanınızı yükseltmeye karar vermeden önce bu makalenin tümünü okumanız gerekir.


## <a name="alerts"></a>Uyarılar

### <a name="question-i-have-a-lot-of-alert-rules-do-i-need-to-create-them-again-in-the-new-language-after-i-upgrade"></a>Soru: çok sayıda uyarı kuralları sahibim. I yükselttikten sonra bunları yeniden yeni dilde oluşturmanız gerekiyor mu?  
Hayır, uyarı kurallarınızı yükseltme sırasında otomatik olarak yeni arama dili dönüştürülür.  

### <a name="question-i-have-alert-rules-with-webhook-and-runbook-actions-will-these-continue-to-work-when-i-upgrade"></a>Soru: Web kancası ve runbook eylemleri olan uyarı kuralları sahibim. Bunlar yükselttiğimde çalışmaya devam eder?

Hayır, bazı değişiklikler vardır yükü işleminin nasıl değişiklik yapmanızı gerektirebilir Web kancası ve runbook eylemleri. Çeşitli çıktı biçimleri standartlaştırmak ve yükü boyutunu azaltmak için bu değişiklikler yaptık. Bu biçimler hakkında ayrıntılar olan [günlük analizi uyarı kurallarında eylemleri eklemek](log-analytics-alerts-actions.md).


## <a name="computer-groups"></a>Bilgisayar grupları

### <a name="question-im-getting-errors-when-trying-to-use-computer-groups--has-their-syntax-changed"></a>Soru: hatalar bilgisayar gruplarını kullanmak çalışırken alıyorum.  Kendi sözdizimi değişti mi?
Evet, çalışma alanınızı yükseltildiğinde kullanarak bilgisayar söz dizimi değişiklikleri gruplandırır.  Bkz: [günlük analizi bilgisayar gruplarında oturum aramaları](log-analytics-computer-groups.md) Ayrıntılar için.


## <a name="dashboards"></a>Panolar

### <a name="question-can-i-still-use-dashboards-in-an-upgraded-workspace"></a>Soru: hala panolar yükseltilen bir çalışma alanında kullanabilir miyim?
Yükseltme işlemine biz depracating işlemi başlıyor **My Pano**.  Pano için çalışma alanınızı yükseltilmişse ancak bu kutucuklar düzenleyemez veya yenilerini ekleyebileceğiniz önce eklenen tüm kutucukları kullanmaya devam edebilirsiniz.  Oluşturma ve düzenleme görünümlerle devam edebilirsiniz [Görünüm Tasarımcısı](log-analytics-view-designer.md), ayarlayın ve aynı zamanda Azure portalında panoları oluşturmak daha zengin bir özellik vardır.


## <a name="log-searches"></a>Günlük aramalar

### <a name="question-i-have-saved-searches-outside-of-my-upgraded-workspace-can-i-convert-them-to-the-new-search-language-automatically"></a>Soru: t dışında yükseltilmiş çalışma Alanım aramaları kaydettiniz. I bunları yeni arama dili otomatik olarak dönüştürebilir miyim?
Her biri dönüştürmek için günlük arama sayfasını dil dönüştürücü aracını kullanın.  Birden çok aramaları çalışma yükseltmeden otomatik olarak dönüştürmek için bir yöntem yoktur.

### <a name="question-why-are-my-query-results-not-sorted"></a>Soru: Neden my sorgu sonuçları sıralanır değil mi?
Varsayılan olarak yeni sorgu dili sonuçları sıralanmış değil.  Kullanım [sıralama işleci](https://go.microsoft.com/fwlink/?linkid=856079) sonuçlarınızı bir veya daha fazla özelliğe göre sıralamak için.

### <a name="question-where-did-the-metrics-view-go-after-i-upgraded"></a>Soru: t yükselttikten sonra ölçümleri görünüm nerede?
Ölçümleri görünüm performans verilerini grafik gösterimi günlük aramadan verdi.  Bu görünüm yükseltmeden sonra artık mevcut değil.  Kullanabileceğiniz [işleci işleme](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/render-operator) bir timechart sorguda çıktısını biçimi.

### <a name="question-where-did-minify-go-after-i-upgraded"></a>Soru: Burada ı yükselttikten sonra Git minify?
Minify arama sonuçlarınızı özetlenen bir görünümünü sağlayan bir özelliktir.  Yükseltmeden sonra artık Minify seçeneği günlük arama Portalı'nda görünür.  Yeni arama dili kullanarak ile benzer işlevselliği elde edebilirsiniz [azaltmak](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/reduce-operator) veya [autocluster_v2](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/evaluate-operator/autocluster). 

    Event
    | where TimeGenerated > ago(10h)
    | reduce by RenderedDescription

    Event
    | where TimeGenerated > ago(10h)
    | evaluate autocluster_v2()



## <a name="log-search-api"></a>Log Arama API’si

### <a name="question-does-the-log-search-api-get-updated-after-i-upgrade"></a>Soru: t yükselttikten sonra günlük arama API güncelleştirilmiş olur mu?
Eski [günlük arama API](log-analytics-log-search-api.md) çalışma alanınızı yükseltildiğinde artık çalışmaz.  Bkz: [Azure günlük analizi REST API](https://dev.loganalytics.io/) yeni API hakkında ayrıntılı bilgi için.


## <a name="portals"></a>Portallar

### <a name="question-should-i-use-the-new-advanced-analytics-portal-or-keep-using-the-log-search-portal"></a>Soru: I yeni Advanced Analytics Portalı'nı kullanın ya da günlük arama portal'ı kullanmaya devam?
İki portalları karşılaştırması görebilirsiniz [oluşturma ve Azure günlük analizi günlük sorgularda düzenleme için portalları](log-analytics-log-search-portals.md).  Gereksinimleriniz için en iyisi seçebilmeniz her ayrı avantajları vardır.  Advanced Analytics Portalı'nda sorguları yazmanız ve Görünüm Tasarımcısı gibi diğer yerler yapıştırmak için yaygın bir durumdur.  İlgili bilgiyi okumalısınız [dikkate alınacak konular](log-analytics-log-search-portals.md#advanced-analytics-portal) yaparken.


### <a name="question--after-upgrade-i-get-an-error-trying-to-run-queries-and-am-also-seeing-errors-in-my-views"></a>Soru: Yükseltmeden sonra sorguları çalıştırmak çalışırken bir hata alabilir ve hataları my görünümlerde de görmesini.

Tarayıcınızı yükseltmeden sonra günlük analizi sorguları çalıştırmak için aşağıdaki adresi erişim gerektirir.  Tarayıcınız bir güvenlik duvarı üzerinden Azure portalına erişim varsa, bu adresleri erişimi etkinleştirmeniz gerekir.

| Uri | IP | Bağlantı Noktaları |
|:---|:---|:---|
| portal.loganalytics.io | Dinamik | 80,443 |
| api.loganalytics.io    | Dinamik | 80,443 |
| docs.loganalytics.io   | Dinamik | 80,443 |



## <a name="power-bi"></a>Power BI

### <a name="question-does-anything-change-with-powerbi-integration"></a>Soru: hiçbir şey Powerbı tümleşme değişiyor mu?
Evet.  Çalışma alanınızı yükseltildikten sonra Power BI için günlük analizi verileri dışarı aktarma işlemi artık çalışmaz.  Yükseltmeden önce oluşturulan tüm var olan zamanlamalar devre dışı bırakılacaktır.  

Yükseltme sonrasında Azure günlük analizi aynı platformu Application Insights kullanır ve günlük analizi sorguları Power BI dışarı aktarmak için aynı işlemi kullanmak [Power BI için Application Insights sorguları dışarı aktarma işlemi](../application-insights/app-insights-export-power-bi.md#export-analytics-queries).  Power BI Excel'e şimdi API uç noktası doğrudan çağırır. Bu sayede en fazla 500.000 satırları veya 64,000,000 bayt veri almak, uzun sorguları dışarı aktarma ve sorgu zaman aşımı özelleştirmek (varsayılan zaman aşımı süresi 3 dakika ve en uzun zaman aşımı 10 dakika).

## <a name="powershell-cmdlets"></a>PowerShell cmdlet'leri

### <a name="question-does-the-log-search-powershell-cmdlet-get-updated-after-i-upgrade"></a>Soru: t yükselttikten sonra günlük arama PowerShell cmdlet güncelleştirilmiş olur mu?
The [Get-AzureRmOperationalInsightsSearchResults](https://docs.microsoft.com/powershell/module/azurerm.operationalinsights/Get-AzureRmOperationalInsightsSearchResults) will be deprecated when the upgrade of all workspaces is complete.  Kullanım [Invoke-LogAnalyticsQuery cmdlet'i](https://dev.loganalytics.io/documentation/Tools/PowerShell-Cmdlets) yükseltilmiş çalışma alanlarında günlük aramalar gerçekleştirmek için.




## <a name="resource-manager-templates"></a>Resource Manager şablonları

### <a name="question-can-i-create-an-upgraded-workspace-with-a-resource-manager-template"></a>Soru: Resource Manager şablonu ile yükseltilen bir çalışma alanı oluşturabilirim?
Evet.  2017-03-15-Önizleme API sürümünü kullanın ve içeren bir **özellikleri** şablonunuzu aşağıdaki örnekteki gibi bölümünde.

    "resources": [
        {
            "type": "Microsoft.OperationalInsights/workspaces",
            "apiVersion": "2017-03-15-preview",
            "name": "[parameters('workspaceName')]",
            "location": "[parameters('workspaceRegion')]",
            "properties": {
                "sku": {
                    "name": "Free"
                },
                "features": {
                    "legacy": 0,
                    "searchVersion": 1
                }
            }
        }
    ],



## <a name="solutions"></a>Çözümler

### <a name="question-will-my-solutions-continue-to-work"></a>Soru: my çözümleri çalışmaya devam eder?
Yeni sorgu dili dönüştürülürse kendi performansını artırır ancak tüm çözümleri yükseltilen bir çalışma alanında, çalışmaya devam eder.  Bu bölümde açıklanan bazı varolan çözümleri ile ilgili sorunları bilinen vardır.

### <a name="known-issue-perspectives-in-application-insights-connector"></a>Bilinen sorun: Perspektifler Application Insights Bağlayıcısı
Açılardan [uygulama Öngörüler Bağlayıcısı çözüm](log-analytics-app-insights-connector.md) Application Insights Bağlayıcısı çözümde artık desteklenmemektedir.  Görünüm Tasarımcısı Application Insights verilerle özel görünümler oluşturmak için kullanabilirsiniz.

### <a name="known-issue-backup-solution"></a>Bilinen sorun: yedekleme çözümü
Yedekleme çözümü veri bir çalışma alanı yükseltmeden önce yüklenmişse, toplayabilir değil. Çözüm kaldırın ve en son sürümünü yükleyin.  Çözüm kullanmaya devam etmek için kurtarma Hizmetleri kasalarının yükseltmeniz gerekir böylece çözümü yeni sürümü Klasik yedekleme kasaları desteklemiyor.

## <a name="upgrade-process"></a>Yükseltme işlemi

### <a name="question-i-have-several-workspaces-can-i-upgrade-all-workspaces-at-the-same-time"></a>Soru: birkaç çalışma alanları sahibim. Aynı anda tüm çalışma alanları yükseltebilir miyim?  
Hayır.  Yükseltme için tek bir çalışma alanı her zaman geçerlidir. Şu anda çok sayıda çalışma alanları aynı anda yükseltme yolu yoktur. Yükseltilen çalışma alanının diğer kullanıcılar da etkileneceğini unutmayın.  

### <a name="question-will-existing-log-data-collected-in-my-workspace-be-modified-if-i-upgrade"></a>Soru: t yükseltirseniz, çalışma Alanım toplanan mevcut günlük verilerini değiştirilecek?  
Hayır. Çalışma alanı aramalarınız için kullanılabilir günlük verilerini yükseltme tarafından etkilenmez. Kayıtlı aramalar, uyarılara ve görünümlere yeni arama dili otomatik olarak dönüştürülür.  

### <a name="question-what-happens-if-i-dont-upgrade-my-workspace"></a>Soru: çalışma Alanım yükseltmediyseniz ne olur?  
Eski günlük arama önümüzdeki aylarda kullanım dışı kalacaktır. O zamana dek yükseltilmemiş çalışma alanları otomatik olarak yükseltilir.

### <a name="question-can-i-revert-back-after-i-upgrade"></a>Soru: geri ı yükselttikten sonra ı döndürebilirsiniz?
Genel kullanıma açılmadan önce yükselttikten sonra çalışma alanınızda geri döndürülemedi.  Yeni dil genel kullanılabilirlik ulaştı, biz eski platform devre dışı bırakmak başlangıç olarak bu özelliği kaldırılmıştır.


## <a name="views"></a>Görünümler

### <a name="question-how-do-i-create-a-new-view-with-view-designer"></a>Soru: Nasıl yeni bir görünüm Görünüm Tasarımcısı ile oluşturulur?
Yükseltmeden önce OMS portalı ana Panoda bir kutucuğu öğesinden Görünüm Tasarımcısı ile yeni bir görünüm oluşturabilirsiniz.  Çalışma alanınızı yükseltildiğinde bu kutucuğu kaldırılır.  Yeşil + düğmesini soldaki menüde üzerinde tıklatarak yararlı OMS portalında Görünüm Tasarımcısı ile yeni bir görünüm oluşturabilirsiniz.  Görünüm Tasarımcısı kutucuğa tıklayarak Azure portalıyla yeni bir görünüm oluşturmaya devam edin.


## <a name="next-steps"></a>Sonraki adımlar

- Daha fazla bilgi edinmek [çalışma alanınızda yeni yükseltme günlük analizi sorgu dili](log-analytics-log-search-upgrade.md).
