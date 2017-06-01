---
title: "Beş dakika içinde Azure’da statik HTML web uygulaması oluşturma | Microsoft Docs"
description: "Örnek bir uygulama dağıtarak App Service&quot;te web uygulamaları çalıştırmanın ne kadar kolay olduğunu öğrenin."
services: app-service\web
documentationcenter: 
author: rick-anderson
manager: wpickett
editor: 
ms.assetid: 60495cc5-6963-4bf0-8174-52786d226c26
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 05/08/2017
ms.author: riande
ms.custom: mvc
ms.translationtype: Human Translation
ms.sourcegitcommit: 71fea4a41b2e3a60f2f610609a14372e678b7ec4
ms.openlocfilehash: 895906e1ab4bc50093ed3b18f043c3dd515ca054
ms.contentlocale: tr-tr
ms.lasthandoff: 05/10/2017

---
# <a name="create-a-static-html-web-app-in-azure-in-five-minutes"></a>Beş dakika içinde Azure’da statik HTML web uygulaması oluşturma

Bu hızlı başlangıç kılavuzunda, bir HTML+CSS sitesinin Azure’a nasıl dağıtılacağı gösterilir. Uygulamayı bir [Azure App Service planı](https://docs.microsoft.com/azure/app-service/azure-web-sites-web-hosting-plans-in-depth-overview) kullanarak çalıştıracak ve [Azure CLI](https://docs.microsoft.com/en-us/cli/azure/get-started-with-azure-cli) aracılığıyla uygulamanın içinde yeni bir web uygulaması oluşturacaksınız. Uygulamayı Azure’a dağıtmak için Git’i kullanırsınız. Önkoşullar yüklendikten sonra öğreticinin tamamlanması yaklaşık beş dakika sürer.

![hello-world-in-browser](media/app-service-web-get-started-html/hello-world-in-browser-az.png)

## <a name="prerequisites"></a>Ön koşullar

Bu örneği oluşturmadan önce aşağıdaki bileşenleri indirip yükleyin:

- [Git](https://git-scm.com/)
- [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli)

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="download-the-sample"></a>Örneği indirme

Bir terminal penceresinde, örnek uygulama deposunu yerel makinenize kopyalayın:

```bash
git clone https://github.com/Azure-Samples/html-docs-hello-world.git
```

## <a name="view-the-html"></a>HTML’yi görüntüleme

Örnek HTML’yi içeren dizine gidin. *index.html* dosyasını tarayıcınızda açın.

![hello-world-in-browser](media/app-service-web-get-started-html/hello-world-in-browser.png)

[!INCLUDE [login-to-azure](../../includes/login-to-azure.md)] 
[!INCLUDE [configure-deployment-user](../../includes/configure-deployment-user.md)] 

[!INCLUDE [app-service-web-quickstart1](../../includes/app-service-web-quickstart1.md)] 

`quickStartPlan` App Service planında bir [Web Uygulaması](app-service-web-overview.md) oluşturun. Web uygulaması, kodunuz için bir barındırma alanı ve dağıtılan uygulamayı görüntülemek için bir URL sağlar.

[!INCLUDE [app-service-web-quickstart2](../../includes/app-service-web-quickstart2.md)] 

Sayfa bir Azure App Service web uygulaması çalışıyor:

![hello-world-in-browser](media/app-service-web-get-started-html/hello-world-in-browser-az.png)

## <a name="update-and-redeploy-the-app"></a>Uygulamayı güncelleştirme ve yeniden dağıtma

*index.html* dosyasını açın. İşaretlemede bir değişiklik yapın. Örneğin, `Hello world!` öğesini `Hello Azure!` olarak değiştirin

Değişikliklerinizi Git’e işleyin ve ardından kod değişikliklerini Azure’a gönderin.

```bash
git commit -am "updated HTML"
git push azure master
```

Dağıtım tamamlandıktan sonra değişiklikleri görmek için tarayıcınızı yenileyin.

[!INCLUDE [manage-azure-web-app](../../includes/manage-azure-web-app.md)]


[!INCLUDE [cli-samples-clean-up](../../includes/cli-samples-clean-up.md)]

## <a name="next-steps"></a>Sonraki adımlar

- Örnek [Web Apps CLI betiklerini](app-service-cli-samples.md) keşfedin.
- Bir [App Service uygulamasına](app-service-web-tutorial-custom-domain.md) contoso.com gibi [özel bir etki alanı adını eşleme](app-service-web-tutorial-custom-domain.md) işlemini öğrenin.
