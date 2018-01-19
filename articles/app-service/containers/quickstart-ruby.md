---
title: "Ruby Uygulaması oluşturma ve Linux’ta App Service’e dağıtma | Microsoft Docs"
description: "Linux’ta App Service ile Ruby uygulamaları oluşturmayı öğrenin."
keywords: azure app service, linux, oss, ruby
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
ms.openlocfilehash: f160a3291357387fcef75d8c2257e6e37274b0e7
ms.sourcegitcommit: df4ddc55b42b593f165d56531f591fdb1e689686
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/04/2018
---
# <a name="create-a-ruby-app-in-app-service-on-linux"></a>Linux’ta App Service’te Ruby Uygulaması oluşturma

[Linux’ta App Service](app-service-linux-intro.md) yüksek oranda ölçeklenebilen, kendi kendine düzeltme eki uygulayan bir web barındırma hizmeti sunar. Bu hızlı başlangıçta, basit bir uygulaması oluşturup Linux’ta bir Web App olarak Azure’a nasıl dağıtabileceğiniz açıklanır.

![Hello-world](./media/quickstart-ruby/hello-world-updated.png)

## <a name="prerequisites"></a>Ön koşullar

* <a href="https://www.ruby-lang.org/en/documentation/installation/#rubyinstaller" target="_blank">Ruby 2.4.1 veya üzerini yükleyin</a>
* <a href="https://git-scm.com/" target="_blank">Git'i yükleyin</a>

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="download-the-sample"></a>Örneği indirme

Bir terminal penceresinde, örnek uygulama deposunu yerel makinenize kopyalamak için aşağıdaki komutu çalıştırın:

```bash
git clone https://github.com/Azure-Samples/ruby-docs-hello-world
```

## <a name="run-the-application-locally"></a>Uygulamayı yerel olarak çalıştırma

Uygulamanın çalışması için Rails sunucusunu çalıştırın. *Hello-world* dizinine geçin; `rails server` komutu sunucuyu başlatır.

```bash
cd hello-world\bin
rails server
```

Web tarayıcınızı kullanarak uygulamayı yerel olarak test etmek için `http://localhost:3000` yoluna gidin.

![Hello-world](./media/quickstart-ruby/hello-world.png)

## <a name="modify-app-to-display-welcome-message"></a>Uygulamayı karşılama iletisi görüntüleyecek şekilde değiştirme

Uygulamayı bir karşılama iletisi görüntüleyecek şekilde değiştirin. İlk olarak *~/workspace/ruby-docs-hello-world/config/routes.rb* dosyasını `hello` adlı bir rota içerecek şekilde değiştirerek bir rota ayarlamanız gerekir.

  ```ruby
  Rails.application.routes.draw do
      #For details on the DSL available within this file, see http://guides.rubyonrails.org/routing.html
      root 'application#hello'
  end
  ```

Uygulamanın denetleyicisini, iletiyi tarayıcıya HTML olarak döndürecek şekilde değiştirin. 

*~/workspace/hello-world/app/controllers/application_controller.rb* dosyasını düzenlemek üzere açın. `ApplicationController` sınıfını şu kod örneğinde görülen şekilde değiştirin:

  ```ruby
  class ApplicationController > ActionController :: base
    protect_from_forgery with: :exception
    def hello
      render html: "Hello, world from Azure Web App on Linux!"
    end
  end
  ```

Uygulamanız artık yapılandırılmıştır. Kök giriş sayfasını doğrulamak için web tarayıcınızı kullanarak `http://localhost:3000` yoluna gidin.

![Hello World yapılandırıldı](./media/quickstart-ruby/hello-world-configured.png)

[!INCLUDE [Try Cloud Shell](../../../includes/cloud-shell-try-it.md)]

[!INCLUDE [Configure deployment user](../../../includes/configure-deployment-user.md)]

## <a name="create-a-ruby-web-app-on-azure"></a>Azure’da Ruby web uygulaması oluşturma

Web uygulamanız için gerekli varlıkları içeren bir kaynak grubu gerekir. Kaynak grubu oluşturmak için [az group create]() komutunu kullanın.

```azurecli-interactive
az group create --location westeurope --name myResourceGroup
```

[az appservice plan create](/cli/azure/appservice/plan?view=azure-cli-latest#az_appservice_plan_create) komutunu kullanarak web uygulamanız için bir uygulama hizmeti planı oluşturun.

```azurecli-interactive
az appservice plan create --name myAppServicePlan --resource-group myResourceGroup --is-linux
```

Sonra, [az webapp create](/cli/azure/webapp?view=azure-cli-latest#az_webapp_create) komutunu uygulayarak yeni oluşturulan hizmet planını kullanan web uygulamasını oluşturun. Çalışma zamanının `ruby|2.3` olarak ayarlandığına dikkat edin. `<app name>` değerini benzersiz bir uygulama adıyla değiştirmeyi unutmayın.

```azurecli-interactive
az webapp create --resource-group myResourceGroup --plan myAppServicePlan --name <app name> \
--runtime "ruby|2.3" --deployment-local-git
```

Komut çıktısı, yeni oluşturulan web uygulamasıyla ilgili bilgilerin yanı sıra dağıtım URL’sini gösterir. Aşağıda yer alan örnekteki gibi görünmelidir. Bu öğreticide daha sonra kullanmak üzere URL’yi kopyalayın.

```bash
https://<deployment user name>@<app name>.scm.azurewebsites.net/<app name>.git
```

Web uygulaması oluşturulduğunda bir **Genel Bakış** sayfası görüntülenebilir. Bu sayfaya gidin. Aşağıdaki karşılama sayfası görüntülenir:

![Karşılama sayfası](./media/quickstart-ruby/splash-page.png)


## <a name="deploy-your-application"></a>Uygulamanızı dağıtma

Yerel uygulamayı Azure web sitenize dağıtmak için aşağıdaki komutları çalıştırın:

```bash
git remote add azure <Git deployment URL from above>
git add -A
git commit -m "Initial deployment commit"
git push azure master
```

Uzaktan dağıtım işlemlerinin başarılı olarak bildirildiğini doğrulayın. Komutlar aşağıdaki metne benzer bir çıktı oluşturur:

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

Dağıtım tamamlandığında, dağıtımın etkili olması için burada gösterildiği gibi [az webapp restart](/cli/azure/webapp?view=azure-cli-latest#az_webapp_restart) komutunu kullanarak web uygulamanızı yeniden başlatın:

```azurecli-interactive
az webapp restart --name <app name> --resource-group myResourceGroup
```

Sitenize gidin ve sonuçları doğrulayın.

```bash
http://<app name>.azurewebsites.net
```

![güncelleştirilen web uygulaması](./media/quickstart-ruby/hello-world-updated.png)

> [!NOTE]
> Uygulama yeniden başlatıldığı sırada siteye göz atmaya çalışılması, `Error 503 Server unavailable` şeklinde bir HTTP durum koduna yol açar. Tamamen yeniden başlatılması birkaç dakika sürebilir.
>

[!INCLUDE [Clean-up section](../../../includes/cli-script-clean-up.md)]

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [MySQL ile Ruby on Rails](tutorial-ruby-mysql-app.md)
