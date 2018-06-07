---
title: Yayın Application Insights için ek açıklamalar | Microsoft Docs
description: Dağıtım ekleme veya Application Insights, ölçümleri explorer grafiklerde işaretleyicileri oluşturabilirsiniz.
services: application-insights
documentationcenter: .net
author: mrbullwinkle
manager: carmonm
ms.assetid: 23173e33-d4f2-4528-a730-913a8fd5f02e
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 11/16/2016
ms.author: mbullwin
ms.openlocfilehash: a479fa553d64f3820ae8513353484e72b57d30e4
ms.sourcegitcommit: 6cf20e87414dedd0d4f0ae644696151e728633b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/06/2018
ms.locfileid: "34807807"
---
# <a name="annotations-on-metric-charts-in-application-insights"></a>Application ınsights'ta ölçüm grafiklerde ek açıklamaları
Ek açıklamalar [ölçüm Gezgini](app-insights-metrics-explorer.md) grafikleri Göster yeni bir yapı veya diğer önemli olay dağıtıldığı. Bunlar değişikliklerinizi uygulamanızın performansı üzerinde hiçbir etkisi sahip olup olmadığını görmek kolaylaştırır. Tarafından otomatik olarak oluşturulabilir [Visual Studio Team Services yapı sistem](https://www.visualstudio.com/en-us/get-started/build/build-your-app-vs). Tarafından gibi herhangi bir olay bayrak için ek açıklama oluşturabilirsiniz [Powershell'den oluşturma](#create-annotations-from-powershell).

![Sunucu yanıt süresi ile görünür bağıntı ile ek açıklama örneği](./media/app-insights-annotations/00.png)



## <a name="release-annotations-with-vsts-build"></a>VSTS derleme ile yayın ek açıklama

Yayın ek açıklamalar, bulut tabanlı derleme özelliğidir ve Visual Studio Team Services hizmeti bırakın. 

### <a name="install-the-annotations-extension-one-time"></a>Ek açıklamalar uzantısını (bir kez) yükleyin
Yayın ek açıklamaları oluşturabilecek için Visual Studio Market'te bulunabilir birçok takım hizmet uzantılardan birine yüklemeniz gerekir.

1. Oturum açın, [Visual Studio Team Services](https://www.visualstudio.com/en-us/get-started/setup/sign-up-for-visual-studio-online) projesi.
2. Visual Studio pazarında [yayın ek açıklamaları uzantı alınmaya](https://marketplace.visualstudio.com/items/ms-appinsights.appinsightsreleaseannotations)ve Team Services hesabınıza ekleyin.

![AT Team Services web sayfasının açık Market sağ üst. Visual Team Services seçin ve ardından derleme ve sürüm altında daha bkz belirtin.](./media/app-insights-annotations/10.png)

Yalnızca bu kez Visual Studio Team Services hesabınız için yapmanız gerekir. Yayın ek açıklamaları artık hesabınıza herhangi bir projede için yapılandırılabilir. 

### <a name="configure-release-annotations"></a>Yayın ek açıklamaları yapılandırın

Her VSTS yayın şablonu için ayrı bir API anahtarı edinmeniz gerekir.

1. Oturum [Microsoft Azure portalı](https://portal.azure.com) ve uygulamanızı izler Application Insights kaynağı açın. (Veya [şimdi oluşturmak](app-insights-overview.md), henüz yapmadıysanız.)
2. Açık **API erişimini**, **uygulama Öngörüler kimliği**.
   
    ![Portal.Azure.com adresinde Application Insights kaynağınıza açın ve Ayarlar'ı seçin. API erişimini açın. Uygulama Kimliğini kopyalayın](./media/app-insights-annotations/20.png)

4. Ayrı bir tarayıcı penceresi açın (veya oluşturun) Visual Studio Team Services dağıtımlarınızı yönetir yayın şablonu. 
   
    Bir görev eklemek ve uygulama Öngörüler yayın ek açıklama görev menüsünden seçin.
   
    Yapıştır **uygulama kimliği** API erişimini dikey penceresinden kopyaladığınız.
   
    ![Visual Studio Team Services sürüm açın, bir yayın tanımı seçin ve Düzenle'yi seçin. Görev Ekle'yi tıklatın ve uygulama Öngörüler yayın ek açıklama seçin. Uygulama Öngörüler kimliği yapıştırın](./media/app-insights-annotations/30.png)
4. Ayarlama **apikey ile yapılan** bir değişkene alan `$(ApiKey)`.

5. Azure penceresine geri döndüğünüzde yeni bir API anahtar oluşturun ve bir kopyasını alın.
   
    ![Azure penceresinde API erişimini dikey penceresinde API anahtarı oluştur'a tıklayın. Bir açıklama sağlayın, yazma ek açıklamaları denetleyin ve anahtar oluştur'a tıklayın. Yeni anahtarı kopyalayın.](./media/app-insights-annotations/40.png)

6. Yayın şablonu yapılandırma sekmesini açın.
   
    İçin bir değişken tanımı oluşturun `ApiKey`.
   
    API anahtarınıza apikey ile yapılan değişken tanımını yapıştırın.
   
    ![Team Services penceresinde yapılandırma sekmesini seçin ve değişken Ekle'yi tıklatın. Adı apikey ile yapılan ve değerine ayarlayın, az önce oluşturulan anahtarını yapıştırın ve kilit simgesini tıklatın.](./media/app-insights-annotations/50.png)
7. Son olarak, **kaydetmek** yayın tanımı.


## <a name="view-annotations"></a>Görünüm ek açıklamaları
Artık, yeni bir sürüm dağıtmak için yayın şablonu kullandığınızda, ek açıklamanın Application Insights'a gönderilir. Ek açıklamalar ölçüm Gezgini grafiklerinde görüntülenir.

İstek sahibi, kaynak denetimi dalı dahil olmak üzere, yayın ayrıntılarını açmak için hiçbir ek açıklama işaretçisi tıklayın, tanımı, ortam ve daha fazlasını bırakın.

![Tüm yayın ek açıklama işaretçisi'ı tıklatın.](./media/app-insights-annotations/60.png)

## <a name="create-custom-annotations-from-powershell"></a>Özel ek açıklama Powershell'den oluşturma
(VS Team System kullanmadan) gibi herhangi bir işlem ek açıklamaları da oluşturabilirsiniz. 


1. Yerel bir kopyasını [Powershell betiği github'dan](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/API/CreateReleaseAnnotation.ps1).

2. Uygulama Kimliği alın ve API erişimini dikey penceresinden bir API anahtarı oluşturun.

3. Bu gibi komut dosyası çağırın:

```PS

     .\CreateReleaseAnnotation.ps1 `
      -applicationId "<applicationId>" `
      -apiKey "<apiKey>" `
      -releaseName "<myReleaseName>" `
      -releaseProperties @{
          "ReleaseDescription"="a description";
          "TriggerBy"="My Name" }
```

Ek açıklamalar için geçmiş oluşturmak örnek için betik değiştirmek çok kolaydır.

## <a name="next-steps"></a>Sonraki adımlar

* [İş öğeleri oluşturma](app-insights-diagnostic-search.md#create-work-item)
* [PowerShell ile Automation](app-insights-powershell.md)
