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
ms.devlang: na
ms.topic: conceptual
ms.date: 11/16/2016
ms.author: mbullwin
ms.openlocfilehash: 660080a629e00884dd61a49bc0950ebe25b6a0c5
ms.sourcegitcommit: 744747d828e1ab937b0d6df358127fcf6965f8c8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "42057727"
---
# <a name="annotations-on-metric-charts-in-application-insights"></a>Application ınsights ölçüm grafikleri ek açıklamalar
Ek açıklamalar [ölçüm Gezgini](app-insights-metrics-explorer.md) grafikleri, yeni bir derleme veya diğer önemli olay dağıtıldığı gösterir. Bunlar, değişikliklerinizi uygulamanızın performansını herhangi bir etkisi sahip olup olmadığını görmek kolaylaştırır. Tarafından otomatik olarak oluşturulabilir [Visual Studio Team Services derleme sistemi](https://docs.microsoft.com/vsts/pipelines/tasks/). Bayrak olarak istediğiniz herhangi bir olay için ek açıklamaları da oluşturabilirsiniz [Powershell'den oluşturarak](#create-annotations-from-powershell).

![Sunucu yanıt süresi ile görünür bağıntı açıklamalarla örneği](./media/app-insights-annotations/00.png)



## <a name="release-annotations-with-vsts-build"></a>VSTS derleme ile yayın ek açıklamaları

Sürüm ek açıklamaları, bulut tabanlı derleme özelliğidir ve Visual Studio Team Services'ın hizmet bırakın. 

### <a name="install-the-annotations-extension-one-time"></a>Ek Açıklamalar (bir kez) uzantıyı yükleme
Sürüm ek açıklamalarını oluşturabilmek için birçok Team Service uzantılara Visual Studio Market'te yüklemeniz gerekir.

1. Oturum açın, [Visual Studio Team Services](https://visualstudio.microsoft.com/vso/) proje.
2. Visual Studio Market'te [sürüm ek açıklamalarını uzantısını Al](https://marketplace.visualstudio.com/items/ms-appinsights.appinsightsreleaseannotations), Team Services hesabınıza ekleyin.

![AT sağ üst köşesinde Team Services web sayfası, açık Market. Visual Team Services'ı seçin ve ardından derleme ve yayın altında daha fazla seçin.](./media/app-insights-annotations/10.png)

Yalnızca bu kez Visual Studio Team Services hesabınız için yapmanız gerekir. Sürüm ek açıklamaları, hesabınızdaki herhangi bir proje için artık yapılandırılabilir. 

### <a name="configure-release-annotations"></a>Sürüm ek açıklamalarını yapılandırın

Her VSTS yayın şablonu için ayrı bir API anahtarı alma gerekir.

1. Oturum [Microsoft Azure Portal](https://portal.azure.com) ve uygulamanızı izler Application Insights kaynağını açın. (Veya [şimdi oluşturmak](app-insights-overview.md), siz henüz yapmadıysanız.)
2. Açık **API erişimi**, **Application Insights kimliği**.
   
    ![Portal.Azure.com adresinde Application Insights kaynağınızı açın ve Ayarlar'ı seçin. API erişimi'ni açın. Uygulama Kimliğini kopyalama](./media/app-insights-annotations/20.png)

4. Ayrı bir tarayıcı penceresi açın (veya oluşturun) Visual Studio Team Services dağıtımlarınız yöneten sürüm şablonu. 
   
    Bir görev ekleyin ve Application Insights sürüm ek açıklamasının görev menüden seçim yapın.
   
    Yapıştırma **uygulama kimliği** API erişimi dikey penceresinden kopyaladığınız.
   
    ![Visual Studio Team Services, yayın açın, bir yayın tanımı seçin ve Düzenle'yi seçin. Görev Ekle tıklayın ve Application Insights sürüm ek açıklamasının'ı seçin. Application Insights kimliğini yapıştırın](./media/app-insights-annotations/30.png)
4. Ayarlama **APIKey** alan bir değişkene `$(ApiKey)`.

5. Azure penceresinde geri döndüğünüzde yeni bir API anahtarı oluşturma ve bir kopyasını alın.
   
    ![Azure penceresinde API erişimi dikey pencerede API anahtarı oluştur'a tıklayın. Bir açıklama sağlayın, yazma ek açıklamalarını denetleyin ve anahtar oluştur'a tıklayın. Yeni anahtarı kopyalayın.](./media/app-insights-annotations/40.png)

6. Yayın şablonu yapılandırma sekmesini açın.
   
    Bir değişken tanımı oluşturma `ApiKey`.
   
    ApiKey değişken tanımı için API anahtarınızı yapıştırın.
   
    ![Team Services penceresinde, yapılandırma sekmesini seçin ve değişken Ekle'a tıklayın. ApiKey ve değerine ayarlayın, yalnızca oluşturulan anahtarı yapıştırın ve kilit simgesine tıklayın.](./media/app-insights-annotations/50.png)
7. Son olarak, **Kaydet** yayın tanımı.


## <a name="view-annotations"></a>Ek açıklamaları görüntüle
Şimdi, yeni bir yayın dağıtmak için yayın şablonunu kullandığınızda, bir ek açıklama Application Insights'a gönderilir. Ek açıklamalar, grafiklerde ölçüm Gezgini'nde görünür.

İstek sahibi, kaynak denetimi dal dahil olmak üzere, yayın ayrıntılarını açmak için herhangi bir ek açıklama işaretçisi tıklayın, sürüm tanımı, ortam ve daha fazlası.

![Herhangi bir sürüm ek açıklama işaretçisi tıklayın.](./media/app-insights-annotations/60.png)

## <a name="create-custom-annotations-from-powershell"></a>Powershell'den özel ek açıklamaları oluşturma
(Bir VS Team System'ı kullanmadan) gibi herhangi bir işlem ek açıklamaları da oluşturabilirsiniz. 


1. Yerel bir kopyasını [Powershell betiği github'dan](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/API/CreateReleaseAnnotation.ps1).

2. Uygulama Kimliğini alın ve API erişimi dikey penceresinden bir API anahtarı oluşturun.

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

* [İş öğeleri oluşturma](app-insights-diagnostic-search.md#create-work-item)
* [PowerShell ile Automation](app-insights-powershell.md)
