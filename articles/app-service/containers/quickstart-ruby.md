---
title: "Bir Ruby uygulaması oluşturma ve Linux uygulama hizmetine dağıtma | Microsoft Docs"
description: "Uygulama hizmeti Linux'ta ile Söyleniş uygulamaları oluşturmayı öğrenin."
keywords: Azure uygulama hizmeti, linux, oss, ruby
services: app-service
documentationcenter: 
author: SyntaxC4
manager: cfowler
editor: 
ms.assetid: 6d00c73c-13cb-446f-8926-923db4101afa
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 10/10/2017
ms.author: cfowler
ms.custom: mvc
ms.openlocfilehash: 55ff4dc168ca6f8b2bdbb7c5743515691e8ac92d
ms.sourcegitcommit: 9c3150e91cc3075141dc2955a01f47040d76048a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/26/2017
---
# <a name="create-a-ruby-app-in-app-service-on-linux"></a>Linux üzerinde App Service'te bir Ruby uygulaması oluşturma

[Uygulama hizmeti Linux'ta](app-service-linux-intro.md) düzeyde ölçeklenebilir, otomatik olarak düzeltme eki uygulama web barındırma hizmeti sağlar. Bu Hızlı Başlangıç, temel bir Ruby oluşturmayı gösteren rayları uygulaması, Azure için Linux üzerinde bir Web uygulaması olarak dağıtırsınız.

![Merhaba Dünya](./media/quickstart-ruby/hello-world-updated.png)

## <a name="prerequisites"></a>Ön koşullar

* [Ruby 2.4.1 ya da daha yüksek](https://www.ruby-lang.org/en/documentation/installation/#rubyinstaller).
* [Git](https://git-scm.com/downloads).
* Bir [etkin Azure aboneliği](https://azure.microsoft.com/pricing/free-trial/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio).

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="download-the-sample"></a>Örneği indirme

Bir terminal penceresi örnek uygulama depoyu yerel makinenize kopyalamak için aşağıdaki komutu çalıştırın:

```bash
git clone https://github.com/Azure-Samples/ruby-docs-hello-world
```

## <a name="run-the-application-locally"></a>Uygulamayı yerel olarak çalıştırma

Uygulamanın çalışması sırayla rayları sunucunun çalıştırın. Değiştirin *Merhaba Dünya* dizini ve `rails server` komut sunucu başlatır.

```bash
cd hello-world\bin
rails server
```

Web tarayıcınız üzerinden gidin `http://localhost:3000` uygulama yerel olarak test etmek için.

![Merhaba Dünya](./media/quickstart-ruby/hello-world.png)

## <a name="modify-app-to-display-welcome-message"></a>Hoş Geldiniz iletisi görüntülenecek uygulama değiştirme

Hoş Geldiniz iletisi görüntüler için uygulama değiştirin. İlk olarak değiştirerek bir rota kurmanız gerekir *~/workspace/ruby-docs-hello-world/config/routes.rb* adlı bir rota eklenecek dosyası `hello`.

  ```ruby
  Rails.application.routes.draw do
      #For details on the DSL available within this file, see http://guides.rubyonrails.org/routing.html
      root 'application#hello'
  end
  ```

Bu iletiyi HTML olarak tarayıcıya döndürecek şekilde uygulamanın denetleyicisini değiştirin. 

Açık *~/workspace/hello-world/app/controllers/application_controller.rb* düzenlemek için. Değiştirme `ApplicationController` aramak için sınıf gibi aşağıdaki kod örneği:

  ```ruby
  class ApplicationController > ActionController :: base
    protect_from_forgery with: :exception
    def hello
      render html: "Hello, world from Azure Web App on Linux!"
    end
  end
  ```

Uygulamanız şimdi yapılandırıldı. Web tarayıcınız üzerinden gidin `http://localhost:3000` kök giriş sayfası onaylamak için.

![Yapılandırılmış Merhaba Dünya](./media/quickstart-ruby/hello-world-configured.png)

[!INCLUDE [Try Cloud Shell](../../../includes/cloud-shell-try-it.md)]

[!INCLUDE [Configure deployment user](../../../includes/configure-deployment-user.md)]

## <a name="create-a-ruby-web-app-on-azure"></a>Azure üzerinde bir Söyleniş web uygulaması oluşturma

Bir kaynak grubu web uygulamanız için gereken varlıklar içermesi gerekir. Bir kaynak grubu oluşturmak için kullanın [az grubu oluşturma]() komutu.

```azurecli-interactive
az group create --location westeurope --name myResourceGroup
```

Kullanım [az uygulama hizmeti planı oluşturma](https://docs.microsoft.com/cli/azure/appservice/plan#az_appservice_plan_create) web uygulamanız için bir app service planı oluşturmak için komutu.

```azurecli-interactive
az appservice plan create --name myAppServicePlan --resource-group myResourceGroup --is-linux
```

Ardından, sorun [az webapp oluşturmak](https://docs.microsoft.com/cli/azure/webapp) yeni oluşturulan hizmet planını kullanan web uygulaması oluşturmak için komutu. Çalışma zamanı kümesine bildirimi `ruby|2.3`. Değiştirmeyi unutmayın `<app name>` benzersiz bir uygulama adına sahip.

```azurecli-interactive
az webapp create --resource-group myResourceGroup --plan myAppServicePlan --name <app name> \
--runtime "ruby|2.3" --deployment-local-git
```

Komut çıktısı dağıtım URL'si yanı sıra yeni oluşturulan web uygulaması hakkında bilgi gösterir. Aşağıdaki örneğe benzemelidir. Bu öğreticide daha sonra kullanmak için URL'yi kopyalayın.

```bash
https://<deployment user name>@<app name>.scm.azurewebsites.net/<app name>.git
```

Web uygulaması oluşturulduktan sonra bir **genel bakış** sayfasını görüntülemek kullanılabilir. Kendisine gidin. Aşağıdaki karşılama sayfası görüntülenir:

![Giriş sayfası](./media/quickstart-ruby/splash-page.png)


## <a name="deploy-your-application"></a>Uygulamanızı dağıtmak

Azure Web sitenizi yerel uygulamayı dağıtmak için aşağıdaki komutları çalıştırın:

```bash
git remote add azure <Git deployment URL from above>
git add -A
git commit -m "Initial deployment commit"
git push azure master
```

Uzaktan dağıtım işlemlerini Başarı Raporu onaylayın. Komutları, aşağıdakine benzer bir çıktı üretir:

```bash
remote: Using sass-rails 5.0.6
remote: Updating files in vendor/cache
remote: Bundle gems are installed into ./vendor/bundle
remote: Updating files in vendor/cache
remote: ~site/repository
remote: Finished successfully.
remote: Running post deployment command(s)...
remote: Deployment successful.
To https://<your web app name>.scm.azurewebsites.net/<your web app name>.git
  579ccb....2ca5f31  master -> master
myuser@ubuntu1234:~workspace/<app name>$
```

Dağıtım tamamlandıktan sonra web uygulamanızı kullanarak etkili olması için dağıtım için yeniden [az webapp yeniden](https://docs.microsoft.com/cli/azure/webapp#az_webapp_restart) aşağıda gösterildiği gibi komut:

```azurecli-interactive
az webapp restart --name <app name> --resource-group myResourceGroup
```

Sitenize gidin ve sonuçlar doğrulayın.

```bash
http://<app name>.azurewebsites.net
```

![güncelleştirilmiş web uygulaması](./media/quickstart-ruby/hello-world-updated.png)

> [!NOTE]
> Uygulama yeniden başlatılırken bir HTTP durum kodu site sonuçlarında göz atmaya çalışırken `Error 503 Server unavailable`. Tam olarak yeniden başlatmak için birkaç dakika sürebilir.
>

[!INCLUDE [Clean-up section](../../../includes/cli-script-clean-up.md)]

## <a name="next-steps"></a>Sonraki adımlar

[Azure uygulama hizmeti Linux SSS](https://docs.microsoft.com/azure/app-service-web/app-service-linux-faq.md)
