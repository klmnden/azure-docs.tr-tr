---
title: Sürüm ek açıklamaları Application ınsights | Microsoft Docs
description: Dağıtım ekleyebilir veya, Application ınsights ölçüm Gezgini grafiklerini işaretleyicileri oluşturun.
services: application-insights
documentationcenter: .net
author: mrbullwinkle
manager: carmonm
ms.assetid: 23173e33-d4f2-4528-a730-913a8fd5f02e
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 07/01/2019
ms.author: mbullwin
ms.openlocfilehash: e3ec202ba6126b150fb78c76591682f163018661
ms.sourcegitcommit: f10ae7078e477531af5b61a7fe64ab0e389830e8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2019
ms.locfileid: "67604553"
---
# <a name="annotations-on-metric-charts-in-application-insights"></a>Application ınsights ölçüm grafikleri ek açıklamalar

Ek açıklamalar [ölçüm Gezgini](../../azure-monitor/app/metrics-explorer.md) grafikleri, yeni bir derleme veya diğer önemli olayların dağıtıldığı gösterir. Ek açıklamalar, değişikliklerinizi uygulamanızın performansını herhangi bir etkisi sahip olup olmadığını görmek kolaylaştırır. Tarafından otomatik olarak oluşturulabilir [Azure işlem hatları](https://docs.microsoft.com/azure/devops/pipelines/tasks/) sistem oluşturun. Powershell'den oluşturarak gibi herhangi bir olay bayrağı için ek açıklamaları da oluşturabilirsiniz.

> [!NOTE]
> Bu makalede kullanım dışı yansıtır **Klasik ölçüm deneyimi**. Ek açıklamalara sahip Klasik deneyim ve şu anda kullanılabilir yalnızca  **[çalışma kitapları](../../azure-monitor/app/usage-workbooks.md)** . Geçerli ölçüm deneyimi hakkında daha fazla bilgi için bkz: [Gelişmiş Özellikler, Azure ölçüm Gezgini](../../azure-monitor/platform/metrics-charts.md).

![Ek Açıklama örneği](./media/annotations/0-example.png)

## <a name="release-annotations-with-azure-pipelines-build"></a>Sürüm ek açıklamaları ile Azure işlem hatları oluşturma

Sürüm ek açıklamaları, Azure DevOps bulut tabanlı Azure işlem hatları hizmetin bir özelliğidir.

### <a name="install-the-annotations-extension-one-time"></a>Ek Açıklamalar (bir kez) uzantıyı yükleme
Sürüm ek açıklamalarını oluşturabilmek için birçok Azure DevOps uzantılara Visual Studio Market'te yüklemeniz gerekir.

1. Oturum açın, [Azure DevOps](https://azure.microsoft.com/services/devops/) proje.
   
1. Visual Studio Market'te [sürüm ek açıklamalarını uzantısı](https://marketplace.visualstudio.com/items/ms-appinsights.appinsightsreleaseannotations) sayfa, Azure DevOps kuruluşunuzu seçin ve ardından **yükleme** uzantısı, Azure DevOps kuruluşa eklemek için.
   
   ![Azure DevOps kuruluş seçin ve ardından Yükle'yi seçebilirsiniz.](./media/annotations/1-install.png)
   
Yalnızca Azure DevOps kuruluşunuz için bir kez uzantıyı yüklemeniz gerekir. Artık kuruluşunuzdaki herhangi bir proje için sürüm ek açıklamaları da yapılandırabilirsiniz.

### <a name="configure-release-annotations"></a>Sürüm ek açıklamalarını yapılandırın

Her biri, Azure işlem hatları sürüm şablonları için ayrı bir API anahtarı oluşturun.

1. Oturum [Azure portalında](https://portal.azure.com) ve uygulamanızı izler Application Insights kaynağını açın. Veya, yoksa [yeni bir Application Insights kaynağı oluşturun](../../azure-monitor/app/app-insights-overview.md).
   
1. Açık **API erişimi** sekmesi ve kopyalama **Application Insights kimliği**.
   
   ![API erişimi altında uygulama kimliğini kopyalama](./media/annotations/2-app-id.png)

1. Ayrı bir tarayıcı penceresinde açın veya Azure işlem hatları dağıtımlarınızı yöneten sürüm şablonu oluşturun.
   
1. Seçin **Görev Ekle**ve ardından **Application Insights sürüm ek açıklamasının** görev menüsünde.
   
   ![Görev Ekle ve Application Insights sürüm ek açıklamasının'ı seçin.](./media/annotations/3-add-task.png)
   
1. Altında **uygulama kimliği**, Application Insights kopyaladığınız kimliği yapıştırın **API erişimi** sekmesi.
   
   ![Application Insights kimliği yapıştırın](./media/annotations/4-paste-app-id.png)
   
1. Application ınsights'ta geri **API erişimi** penceresinde **API anahtarı oluştur**. 
   
   ![API erişimi sekmede API anahtarı oluştur'ı seçin.](./media/annotations/5-create-api-key.png)
   
1. İçinde **API anahtarı oluştur** penceresi, açıklaması, bir tür seçin **yazma ek açıklamaları**ve ardından **anahtar üret**. Yeni anahtarı kopyalayın.
   
   ![API oluşturma anahtar penceresinde, bir açıklama yazın, yazma ek açıklamalar'ı seçin ve anahtar Üret'ı seçin.](./media/annotations/6-create-api-key.png)
   
1. Yayın Şablonu penceresinde üzerinde **değişkenleri** sekmesinde **Ekle** yeni API anahtarı için bir değişken tanımı oluşturmak için.

1. Altında **adı**, girin `ApiKey`, altında **değer**, kopyaladığınız API anahtarını yapıştırın **API erişimi** sekmesi.
   
   ![Azure DevOps değişkenleri sekmesinde, ekleme, adı ' % s'değişkeni ApiKey seçin ve değeri altında API anahtarını yapıştırın.](./media/annotations/7-paste-api-key.png)
   
1. Seçin **Kaydet** ana yayın şablonu penceresinde şablonu kaydedin.

## <a name="view-annotations"></a>Ek açıklamaları görüntüle
Şimdi, yeni bir yayın dağıtmak için yayın şablonunu kullandığınızda, bir ek açıklama Application Insights'a gönderilir. Grafiklerde ek açıklamaları görünür **ölçüm Gezgini**.

İstek sahibi, kaynak denetim dalı, yayın işlem hattı ve ortam gibi yayın ayrıntılarını açmak için hiçbir ek açıklama işaretçisi (açık gri ok) seçin.

![Sürüm ek açıklama işaretçisi seçin.](./media/annotations/8-release.png)

## <a name="create-custom-annotations-from-powershell"></a>Powershell'den özel ek açıklamaları oluşturma
Kullanabileceğiniz [CreateReleaseAnnotation](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/API/CreateReleaseAnnotation.ps1) ek açıklamaları istediğiniz, Azure DevOps kullanmadan herhangi bir işlem oluşturmak için github, PowerShell Betiği. 

1. Yerel bir kopyasını [CreateReleaseAnnotation.ps1](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/API/CreateReleaseAnnotation.ps1).
   
1. Application Insights Kimliğinizi alın ve uygulama anlayışları'ndan bir API anahtarı oluşturmak için önceki yordamda adımları kullanın **API erişimi** sekmesi.
   
1. PowerShell betiğini aşağıdaki kodla açılı köşeli parantez içindeki yer tutucuları değerleriniz ile değiştirerek çağırın. `-releaseProperties` İsteğe bağlıdır. 
   
   ```powershell
   
        .\CreateReleaseAnnotation.ps1 `
         -applicationId "<applicationId>" `
         -apiKey "<apiKey>" `
         -releaseName "<releaseName>" `
         -releaseProperties @{
             "ReleaseDescription"="<a description>";
             "TriggerBy"="<Your name>" }
   ```

Örneğin son ek açıklamalarını oluşturmak komut dosyasını değiştirebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

* [İş öğeleri oluşturma](../../azure-monitor/app/diagnostic-search.md#create-work-item)
* [PowerShell ile Automation](../../azure-monitor/app/powershell.md)
