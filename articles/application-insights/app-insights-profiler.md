---
title: Profil Canlı Azure web apps ile Application Insights | Microsoft Docs
description: Azure Application Insights Profiler ile canlı web apps'de profili.
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.reviewer: cawa
ms.date: 08/06/2018
ms.author: mbullwin
ms.openlocfilehash: 2f726c9c70e6e46ad2e82e9d6f15dae2c9d3d008
ms.sourcegitcommit: 333d4246f62b858e376dcdcda789ecbc0c93cd92
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/01/2018
ms.locfileid: "52724698"
---
# <a name="profile-live-azure-web-apps-with-application-insights"></a>Application Insights ile canlı Azure web apps profili

Profiler şu anda Web Apps üzerinde çalışan ASP.NET ve ASP.NET Core web uygulamaları için çalışır. Temel Hizmet katmanını veya üzeri Profiler'ı kullanmak için gerekli değildir.

## <a id="installation"></a> Web uygulamalarınız için Profiler'ı etkinleştir
Web uygulaması için Profiler'ı etkinleştirmek için aşağıdaki yönergeleri izleyin. Farklı türde bir Azure hizmeti çalıştırıyorsanız, desteklenen platformlarda Profiler'ı etkinleştirmek için yönergeler şunlardır:
* [Cloud Services](app-insights-profiler-cloudservice.md?toc=/azure/azure-monitor/toc.json)
* [Service Fabric uygulamaları](app-insights-profiler-servicefabric.md?toc=/azure/azure-monitor/toc.json)
* [Sanal Makineler](app-insights-profiler-vm.md?toc=/azure/azure-monitor/toc.json)


Application Insights Profiler uygulama hizmetleri çalışma zamanı bir parçası olarak önceden yüklenmiş, ancak Azure Web Apps için get profilleri açın duruma getirmeniz gerekir. Kaynak kodunda App Insights SDK'sı dahil olsa bile bir Web uygulamasını dağıttıktan sonra Profil Oluşturucu etkinleştirmek için aşağıdaki adımları izleyin.

1. Git **uygulama hizmetleri** bölmesinde Azure portalında.
1. Gidin **Ayarları > İzleme** bölmesi.

   ![Uygulama Hizmetleri portalında App ınsights'ı etkinleştirme](./media/app-insights-profiler/AppInsights-AppServices.png)

1. Ya da bölmesinde web uygulamanızı izlemek için var olan App Insights kaynağı seçin veya yeni bir kaynak oluşturmak için yönergeleri izleyin. Ayrıca Profiler olduğundan emin olun **üzerinde**.

   ![App Insights site uzantısı Ekle][Enablement UI]

1. Profiler, artık bir uygulama hizmetleri uygulama ayarı kullanılarak etkinleştirilir.

    ![Profiler uygulama ayarı][profiler-app-setting]

## <a name="disable-profiler"></a>Profiler devre dışı bırak

Durdurmak veya Profiler altında bir tek tek web uygulamasının örneği için yeniden **Web işleri**Web Apps kaynak gidin. Profiler'ı silmek için Git **uzantıları**.

![Bir web işi için Profiler devre dışı bırak][disable-profiler-webjob]

Profiler herhangi bir performans sorunu mümkün olduğunca erken bulmak için tüm web apps'de etkin olmasını öneririz.

Değişiklikleri web uygulamanıza dağıtılacak WebDeploy kullanırsanız, dağıtım sırasında silindi, App_Data klasöründe hariç emin olun. Aksi takdirde, Profiler uzantının dosyaları web uygulamasını azure'a dağıtma başlatıldığında silinir.



## <a name="next-steps"></a>Sonraki adımlar

* [Visual Studio'da Application Insights ile çalışma](https://docs.microsoft.com/azure/application-insights/app-insights-visual-studio)

[appinsights-in-appservices]:./media/app-insights-profiler/AppInsights-AppServices.png
[Enablement UI]: ./media/app-insights-profiler/Enablement_UI.png
[profiler-app-setting]:./media/app-insights-profiler/profiler-app-setting.png
[performance-blade]: ./media/app-insights-profiler/performance-blade.png
[performance-blade-examples]: ./media/app-insights-profiler/performance-blade-examples.png
[performance-blade-v2-examples]:./media/app-insights-profiler/performance-blade-v2-examples.png
[trace-explorer]: ./media/app-insights-profiler/trace-explorer.png
[trace-explorer-toolbar]: ./media/app-insights-profiler/trace-explorer-toolbar.png
[trace-explorer-hint-tip]: ./media/app-insights-profiler/trace-explorer-hint-tip.png
[trace-explorer-hot-path]: ./media/app-insights-profiler/trace-explorer-hot-path.png
[enable-profiler-banner]: ./media/app-insights-profiler/enable-profiler-banner.png
[disable-profiler-webjob]: ./media/app-insights-profiler/disable-profiler-webjob.png
[linked app services]: ./media/app-insights-profiler/linked-app-services.png

