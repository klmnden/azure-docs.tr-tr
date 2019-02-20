---
title: Application Insights ile canlı Azure App Service uygulamalarının profilini | Microsoft Docs
description: Azure App Service Application Insights Profiler ile canlı uygulamaları profil.
services: application-insights
documentationcenter: ''
author: cweining
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.reviewer: mbullwin
ms.date: 08/06/2018
ms.author: cweining
ms.openlocfilehash: f3ec10a970406cbb1bb6a1a52ffa8508e37fc516
ms.sourcegitcommit: 79038221c1d2172c0677e25a1e479e04f470c567
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/19/2019
ms.locfileid: "56414176"
---
# <a name="profile-live-azure-app-service-apps-with-application-insights"></a>Application Insights ile canlı Azure App Service uygulamalarını profili

Profiler, şu anda Azure App Service üzerinde çalışan ASP.NET ve ASP.NET Core uygulamaları için çalışır. Temel Hizmet katmanını veya üzeri Profiler'ı kullanmak için gerekli değildir. Linux üzerinde Profiler'ı etkinleştirmek şu anda yalnızca aracılığıyla olası [bu yöntem](profiler-aspnetcore-linux.md).

## <a id="installation"></a> Uygulamanız için Profiler'ı etkinleştir
Bir uygulama için Profiler'ı etkinleştirmek için aşağıdaki yönergeleri izleyin. Farklı türde bir Azure hizmeti çalıştırıyorsanız, desteklenen platformlarda Profiler'ı etkinleştirmek için yönergeler şunlardır:
* [Cloud Services](../../azure-monitor/app/profiler-cloudservice.md?toc=/azure/azure-monitor/toc.json)
* [Service Fabric uygulamaları](../../azure-monitor/app/profiler-servicefabric.md?toc=/azure/azure-monitor/toc.json)
* [Sanal Makineler](../../azure-monitor/app/profiler-vm.md?toc=/azure/azure-monitor/toc.json)

Application Insights Profiler uygulama hizmetleri çalışma zamanı bir parçası olarak önceden yüklenir ancak get profilleri App Service uygulamanız için oturum açmak gerekir. Kaynak kodunda App Insights SDK'sı dahil olsa bile bir uygulamayı dağıttıktan sonra Profil Oluşturucu etkinleştirmek için aşağıdaki adımları izleyin.

1. Git **uygulama hizmetleri** bölmesinde Azure portalında.
2. Gidin **Ayarları > Application Insights** bölmesi.

   ![Uygulama Hizmetleri portalında App ınsights'ı etkinleştirme](./media/profiler/AppInsights-AppServices.png)

3. Ya da bölmede uygulamanızı izlemek için var olan App Insights kaynağı seçin veya yeni bir kaynak oluşturmak için yönergeleri izleyin. Ayrıca Profiler olduğundan emin olun **üzerinde**.

   ![App Insights site uzantısı Ekle][Enablement UI]

4. Profiler, artık bir uygulama hizmetleri uygulama ayarı kullanılarak etkinleştirilir.

    ![Profiler uygulama ayarı][profiler-app-setting]

## <a name="disable-profiler"></a>Profiler devre dışı bırak

Durdurmak veya Profiler altında tek tek bir uygulamanın örneği için yeniden **Web işleri**Uygulama kaynağı'na gidin. Profiler'ı silmek için Git **uzantıları**.

![Bir web işi için Profiler devre dışı bırak][disable-profiler-webjob]

Profiler herhangi bir performans sorunu mümkün olduğunca erken bulmak için uygulamalarınızı tüm etkin olmasını öneririz.

Değişiklikleri web uygulamanıza dağıtılacak WebDeploy kullanırsanız, dağıtım sırasında silindi, App_Data klasöründe hariç emin olun. Aksi takdirde, Profiler uzantının dosyaları web uygulamasını azure'a dağıtma başlatıldığında silinir.



## <a name="next-steps"></a>Sonraki adımlar

* [Visual Studio'da Application Insights ile çalışma](https://docs.microsoft.com/azure/application-insights/app-insights-visual-studio)

[Enablement UI]: ./media/profiler/Enablement_UI.png
[profiler-app-setting]:./media/profiler/profiler-app-setting.png
[disable-profiler-webjob]: ./media/profiler/disable-profiler-webjob.png
