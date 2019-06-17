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
ms.date: 05/31/2019
ms.author: mbullwin
ms.openlocfilehash: 6567d7f2ebaab5bd7b5bc8fb7b5a62970f169161
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66476178"
---
# <a name="annotations-on-metric-charts-in-application-insights"></a>Application ınsights ölçüm grafikleri ek açıklamalar

Ek açıklamalar [ölçüm Gezgini](../../azure-monitor/app/metrics-explorer.md) grafikleri, yeni bir derleme veya diğer önemli olay dağıtıldığı gösterir. Bunlar, değişikliklerinizi uygulamanızın performansını herhangi bir etkisi sahip olup olmadığını görmek kolaylaştırır. Tarafından otomatik olarak oluşturulabilir [Azure DevOps Hizmetleri derleme sistemi](https://docs.microsoft.com/azure/devops/pipelines/tasks/). Powershell'den oluşturarak gibi herhangi bir olay bayrağı için ek açıklamaları da oluşturabilirsiniz.

> [!NOTE]
> Bu makalede kullanım dışı yansıtır **Klasik ölçüm deneyimi**. Ek açıklamalara sahip Klasik deneyim ve şu anda kullanılabilir yalnızca  **[çalışma kitapları](../../azure-monitor/app/usage-workbooks.md)** . Geçerli hakkında daha fazla bilgi için ölçümler deneyimi, size başvurabilirsiniz [bu makalede](../../azure-monitor/platform/metrics-charts.md).

![Ek Açıklama örneği](./media/annotations/0-example.png)

## <a name="release-annotations-with-azure-devops-services-build"></a>Sürüm ek açıklamaları Azure DevOps Hizmetleri yapı ile

Sürüm ek açıklamaları, Azure DevOps Services bulut tabanlı Azure işlem hatları hizmetin bir özelliğidir.

### <a name="install-the-annotations-extension-one-time"></a>Ek Açıklamalar (bir kez) uzantıyı yükleme
Sürüm ek açıklamalarını oluşturabilmek için birçok Azure DevOps Hizmetleri uzantılara Visual Studio Market'te yüklemeniz gerekir.

1. Oturum açın, [Azure DevOps Hizmetleri](https://azure.microsoft.com/services/devops/) proje.
2. Visual Studio Market'te [sürüm ek açıklamalarını uzantısını Al](https://marketplace.visualstudio.com/items/ms-appinsights.appinsightsreleaseannotations), Azure DevOps Hizmetleri kuruluşunuza ekleyin.

![Azure DevOps kuruluş seçin ve yükleyin.](./media/annotations/1-install.png)

Azure DevOps Hizmetleri kuruluşunuz için bu kez yapmanız yeterlidir. Sürüm ek açıklamalarını kuruluşunuzdaki herhangi bir proje için artık yapılandırılabilir.

### <a name="configure-release-annotations"></a>Sürüm ek açıklamalarını yapılandırın

Her Azure DevOps hizmet yayın şablonu için ayrı bir API anahtarı alma gerekir.

1. Oturum [Microsoft Azure Portal'da](https://portal.azure.com) ve uygulamanızı izler Application Insights kaynağını açın. (Veya [şimdi oluşturmak](../../azure-monitor/app/app-insights-overview.md), siz henüz yapmadıysanız.)
2. Açık **API erişimi** sekmesi ve kopyalama **Application Insights kimliği**.
   
    ![Portal.Azure.com adresinde Application Insights kaynağınızı açın ve Ayarlar'ı seçin. API erişimi'ni açın. Uygulama Kimliğini kopyalama](./media/annotations/2-app-id.png)

4. Ayrı bir tarayıcı penceresi açın (veya oluşturun) Azure DevOps Services'den dağıtımlarınızı yöneten sürüm şablonu.
   
    Bir görev ekleyin ve Application Insights sürüm ek açıklamasının görev menüden seçim yapın.

   ![Görev eklemek için artı işaretine tıklayın ve Application Insights sürüm ek açıklamasının'ı seçin. Application Insights kimliğini yapıştırın](./media/annotations/3-add-task.png)

    Yapıştırma **uygulama kimliği** API erişimi sekmesinden kopyaladığınız.
   
    ![Application Insights kimliği yapıştırın](./media/annotations/4-paste-app-id.png)

5. Azure penceresinde geri döndüğünüzde yeni bir API anahtarı oluşturma ve bir kopyasını alın.
   
    ![Azure penceresinde API erişimi sekmede API anahtarı oluştur'a tıklayın.](./media/annotations/5-create-api-key.png)

    ![Oluşturma API anahtar sekmesi bir açıklama sağlayın, yazma ek açıklamalarını denetleyin ve anahtar oluştur'a tıklayın. Yeni anahtarı kopyalayın.](./media/annotations/6-create-api-key.png)

6. Yayın şablonu yapılandırma sekmesini açın.
   
    Bir değişken tanımı oluşturma `ApiKey`.
   
    ApiKey değişken tanımı için API anahtarınızı yapıştırın.
   
    ![Azure DevOps Hizmetleri penceresindeki değişken sekmesini seçin ve Ekle'ye tıklayın. ApiKey ve değerine ayarlayın, oluşturduğunuz anahtarı yapıştırın ve kilit simgesine tıklayın.](./media/annotations/7-paste-api-key.png)
1. Son olarak, **Kaydet** yayın ardışık düzeni.


## <a name="view-annotations"></a>Ek açıklamaları görüntüle
Şimdi, yeni bir yayın dağıtmak için yayın şablonunu kullandığınızda, bir ek açıklama Application Insights'a gönderilir. Ek açıklamalar, grafiklerde ölçüm Gezgini'nde görünür.

İstek sahibi, kaynak denetimi dal dahil olmak üzere sürüm hakkındaki ayrıntıları açmak için hiçbir ek açıklama işaretçisi üzerinde (açık gri ok) tıklayın, yayın işlem hattı, ortam ve daha fazla.

![Herhangi bir sürüm ek açıklama işaretçisi tıklayın.](./media/annotations/8-release.png)

## <a name="create-custom-annotations-from-powershell"></a>Powershell'den özel ek açıklamaları oluşturma
(Bir Azure DevOps Services'ı kullanmadan) gibi herhangi bir işlem ek açıklamaları da oluşturabilirsiniz. 


1. Yerel bir kopyasını [Powershell betiği github'dan](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/API/CreateReleaseAnnotation.ps1).

2. Uygulama Kimliğini alın ve API erişimi sekmesinden bir API anahtarı oluşturun.

3. Bunun gibi bir betik çağırın:

```PS

     .\CreateReleaseAnnotation.ps1 `
      -applicationId "<applicationId>" `
      -apiKey "<apiKey>" `
      -releaseName "<myReleaseName>" `
      -releaseProperties @{
          "ReleaseDescription"="a description";
          "TriggerBy"="My Name" }
```

Örneğin son ek açıklamalarını oluşturmak komut dosyasını değiştirmek kolay bir işlemdir.

## <a name="next-steps"></a>Sonraki adımlar

* [İş öğeleri oluşturma](../../azure-monitor/app/diagnostic-search.md#create-work-item)
* [PowerShell ile Automation](../../azure-monitor/app/powershell.md)
