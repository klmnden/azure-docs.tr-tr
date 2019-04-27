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
ms.openlocfilehash: b8f6a2d12e1a9920421e6491432b516520ae110b
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60730199"
---
# <a name="profile-live-azure-app-service-apps-with-application-insights"></a>Application Insights ile canlı Azure App Service uygulamalarını profili

Profiler ASP.NET ve ASP.NET Core hakkında temel hizmet katmanı kullanarak Azure App Service üzerinde çalışan uygulamaların çalıştırabilirsiniz veya üzeri. Linux üzerinde Profiler'ı etkinleştirmek şu anda yalnızca aracılığıyla olası [bu yöntem](profiler-aspnetcore-linux.md).

## <a id="installation"></a> Uygulamanız için Profiler'ı etkinleştir
Bir uygulama için Profiler'ı etkinleştirmek için aşağıdaki yönergeleri izleyin. Farklı türde bir Azure hizmeti çalıştırıyorsanız, desteklenen platformlarda Profiler'ı etkinleştirmek için yönergeler şunlardır:
* [Cloud Services](../../azure-monitor/app/profiler-cloudservice.md?toc=/azure/azure-monitor/toc.json)
* [Service Fabric uygulamaları](../../azure-monitor/app/profiler-servicefabric.md?toc=/azure/azure-monitor/toc.json)
* [Sanal Makineler](../../azure-monitor/app/profiler-vm.md?toc=/azure/azure-monitor/toc.json)

Application Insights Profiler, uygulama hizmetleri çalışma zamanı bir parçası olarak önceden yüklenir. Aşağıdaki adımlar, uygulama hizmetiniz için etkinleştirme gösterilmektedir. App Insights SDK'sı, uygulamanızın derleme zamanında ekledik olsa bile bu adımları izleyin.

1. "Always On" ayarı, app service için etkinleştirin. App service'inizin altında genel ayarlar yapılandırma sayfasındaki ayarı güncelleştirebilirsiniz.
1. Git **uygulama hizmetleri** bölmesinde Azure portalında.
1. Gidin **Ayarları > Application Insights** bölmesi.

   ![Uygulama Hizmetleri portalında App ınsights'ı etkinleştirme](./media/profiler/AppInsights-AppServices.png)

1. Ya da bölmede uygulamanızı izlemek için var olan App Insights kaynağı seçin veya yeni bir kaynak oluşturmak için yönergeleri izleyin. Ayrıca Profiler olduğundan emin olun **üzerinde**. Uygulama hizmetinizde farklı bir abonelikte Application Insights kaynağınıza ise, Application Insights'ı yapılandırmak için bu sayfayı kullanamazsınız. Yine de el ile gerekli uygulama ayarları, ancak oluşturarak bunu yapabilirsiniz el ile. [Sonraki bölümde Profiler el ile etkinleştirmek için yönergeler içerir.](#enable-profiler-manually-or-with-azure-resource-manager) 

   ![App Insights site uzantısı Ekle][Enablement UI]

1. Profiler, artık bir uygulama hizmetleri uygulama ayarı kullanılarak etkinleştirilir.

    ![Profiler uygulama ayarı][profiler-app-setting]

## <a name="enable-profiler-manually-or-with-azure-resource-manager"></a>El ile veya Azure Resource Manager ile Profiler'ı etkinleştir
Application Insights Profiler, Azure App Service uygulama ayarları oluşturarak etkinleştirilebilir. Yukarıda gösterilen seçenekler sayfası, bu uygulama ayarları oluşturur. Ancak, bir şablon ya da başka araçlar kullanarak bu ayarları oluşturulmasını otomatik hale getirebilirsiniz. Application Insights kaynağınıza farklı bir abonelikte, Azure App Service'in ise bu ayarlar da çalışır.
Profiler'ı etkinleştirmek için gereken ayarları şunlardır:

|Uygulama Ayarı    | Değer    |
|---------------|----------|
|APPINSIGHTS_INSTRUMENTATIONKEY         | Application Insights kaynağınızın iKey    |
|APPINSIGHTS_PROFILERFEATURE_VERSION | 1.0.0 |
|DiagnosticServices_EXTENSION_VERSION | ~3 |


Kullanarak bu değerleri ayarlayabileceğiniz [Azure Resource Manager şablonları](../../azure-monitor/app/azure-web-apps.md#app-service-application-settings-with-azure-resource-manager), [Azure Powershell](https://docs.microsoft.com/powershell/module/az.websites/set-azwebapp), [Azure CLI](https://docs.microsoft.com/cli/azure/webapp/config/appsettings?view=azure-cli-latest).


## <a name="disable-profiler"></a>Profiler devre dışı bırak

Durdurmak veya Profiler altında tek tek bir uygulamanın örneği için yeniden **Web işleri**Uygulama kaynağı'na gidin. Profiler'ı silmek için Git **uzantıları**.

![Bir web işi için Profiler devre dışı bırak][disable-profiler-webjob]

Profiler herhangi bir performans sorunu mümkün olduğunca erken bulmak için uygulamalarınızı tüm etkin olmasını öneririz.

Profiler'ın dosyaları WebDeploy değişiklikleri web uygulamanıza dağıtmak için kullanırken silinebilir. Silme işlemi sırasında dağıtım silindi, App_Data klasöründe hariç tutarak engelleyebilirsiniz. 


## <a name="next-steps"></a>Sonraki adımlar

* [Visual Studio'da Application Insights ile çalışma](https://docs.microsoft.com/azure/application-insights/app-insights-visual-studio)

[Enablement UI]: ./media/profiler/Enablement_UI.png
[profiler-app-setting]:./media/profiler/profiler-app-setting.png
[disable-profiler-webjob]: ./media/profiler/disable-profiler-webjob.png
