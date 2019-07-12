---
title: Azure App Service'te .NET uygulamaları için Snapshot Debugger'ı etkinleştirme | Microsoft Docs
description: Azure App Service'te .NET uygulamaları için Snapshot Debugger'ı etkinleştir
services: application-insights
documentationcenter: ''
author: brahmnes
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.reviewer: mbullwin
ms.date: 03/07/2019
ms.author: bfung
ms.openlocfilehash: 3e8ce3c2eff7b1f7184bb37f141e62563d4fe714
ms.sourcegitcommit: 6a42dd4b746f3e6de69f7ad0107cc7ad654e39ae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/07/2019
ms.locfileid: "67612676"
---
# <a name="enable-snapshot-debugger-for-net-apps-in-azure-app-service"></a>Azure App Service'te .NET uygulamaları için Snapshot Debugger'ı etkinleştir

Anlık görüntü hata ayıklayıcısı, şu anda Windows service planları üzerinde Azure App Service üzerinde çalışan ASP.NET ve ASP.NET Core uygulamaları için çalışır.

## <a id="installation"></a> Anlık görüntü hata ayıklayıcısı etkinleştir
Bir uygulama için Snapshot Debugger'ı etkinleştirmek için aşağıdaki yönergeleri izleyin. Farklı türde bir Azure hizmeti çalıştırıyorsanız, desteklenen platformlarda Snapshot Debugger'ı etkinleştirmek için yönergeler şunlardır:
* [Azure Cloud Services](snapshot-debugger-vm.md?toc=/azure/azure-monitor/toc.json)
* [Azure Service Fabric Hizmetleri](snapshot-debugger-vm.md?toc=/azure/azure-monitor/toc.json)
* [Azure sanal makineler ve sanal makine ölçek kümeleri](snapshot-debugger-vm.md?toc=/azure/azure-monitor/toc.json)
* [Şirket içi sanal veya fiziksel makineler](snapshot-debugger-vm.md?toc=/azure/azure-monitor/toc.json)

.NET Core önizleme sürümünü kullanıyorsanız, lütfen yönergelerini izleyin [diğer ortamlar için Snapshot Debugger'ı etkinleştirme](snapshot-debugger-vm.md?toc=/azure/azure-monitor/toc.json) ilk içerecek şekilde [Microsoft.applicationınsights.snapshotcollectya da](https://www.nuget.org/packages/Microsoft.ApplicationInsights.SnapshotCollector) NuGet ile uygulama paketini ve ardından aşağıdaki yönergeleri tamamlayın. 

Application Insights Snapshot Debugger uygulama hizmetleri çalışma zamanı bir parçası olarak önceden yüklenmiş, ancak get anlık görüntüleri için App Service uygulamanızı açın duruma getirmeniz gerekir. Kaynak kodunda Application Insights SDK'sı dahil olsa bile bir uygulamayı dağıttıktan sonra snapshot debugger'ı etkinleştirmek için aşağıdaki adımları izleyin.

1. Git **uygulama hizmetleri** bölmesinde Azure portalında.
2. Gidin **Ayarları > Application Insights** bölmesi.

   ![Uygulama Hizmetleri portalında App ınsights'ı etkinleştirme](./media/snapshot-debugger/applicationinsights-appservices.png)

3. Ya da bölmede uygulamanızı izlemek için var olan App Insights kaynağı seçin veya yeni bir kaynak oluşturmak için yönergeleri izleyin. Ayrıca Snapshot Debugger için her iki anahtar olduğundan emin olun **üzerinde**.

   ![App Insights site uzantısı Ekle][Enablement UI]

4. Anlık görüntü hata ayıklayıcısı, artık bir uygulama hizmetleri uygulama ayarı kullanılarak etkinleştirilir.

    ![Uygulama ayarı için anlık görüntü hata ayıklayıcısı][snapshot-debugger-app-setting]

## <a name="disable-snapshot-debugger"></a>Anlık görüntü hata ayıklayıcısı devre dışı bırak

Olarak için aynı adımları izleyerek **Snapshot Debugger'ı etkinleştirme**, ancak her iki anahtarlar için Snapshot Debugger için geçiş **kapalı**.
Snapshot Debugger, uygulama özel durumlarını tanılama kolaylaştırmak için tüm uygulamalarınızı etkin olmasını öneririz.

## <a name="next-steps"></a>Sonraki adımlar

- Bir özel durum tetikleyebileceğiniz uygulama trafiği oluşturur. Ardından, Application Insights örneğine gönderilmek için anlık görüntüleri 10-15 dakika bekleyin.
- Bkz: [anlık görüntüleri](snapshot-debugger.md?toc=/azure/azure-monitor/toc.json#view-snapshots-in-the-portal) Azure portalında.
- Snapshot Debugger sorunlarını giderme konusunda yardım için bkz: [Snapshot Debugger sorun giderme](snapshot-debugger-troubleshoot.md?toc=/azure/azure-monitor/toc.json).

[Enablement UI]: ./media/snapshot-debugger/enablement-ui.png
[snapshot-debugger-app-setting]:./media/snapshot-debugger/snapshot-debugger-app-setting.png

