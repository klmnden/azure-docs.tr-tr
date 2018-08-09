---
title: Ruby Uygulaması oluşturma ve Linux’ta App Service’e dağıtma | Microsoft Docs
description: Linux’ta App Service ile Ruby uygulamaları oluşturmayı öğrenin.
keywords: azure app service, linux, oss, ruby
services: app-service
documentationcenter: ''
author: SyntaxC4
manager: cfowler
editor: ''
ms.assetid: 6d00c73c-13cb-446f-8926-923db4101afa
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 10/10/2017
ms.author: cfowler
ms.custom: mvc
ms.openlocfilehash: 67ecb7c253d7968057b27c5b37b5ed4da12c8d05
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/02/2018
ms.locfileid: "39441505"
---
# <a name="create-a-ruby-app-in-app-service-on-linux"></a>Linux’ta App Service’te Ruby Uygulaması oluşturma

[Linux’ta Azure App Service](app-service-linux-intro.md) yüksek oranda ölçeklenebilen, kendi kendine düzeltme eki uygulayan bir web barındırma hizmeti sunar. Bu hızlı başlangıçta, basit bir [Ruby on Rails](https://rubyonrails.org/) uygulaması oluşturup Linux’ta bir Web App olarak Azure’a nasıl dağıtabileceğiniz açıklanır.

![Hello-world](./media/quickstart-ruby/hello-world-updated.png)

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Ön koşullar

* <a href="https://www.ruby-lang.org/en/documentation/installation/#rubyinstaller" target="_blank">Ruby 2.3 veya üzerini yükleyin</a>
* <a href="https://git-scm.com/" target="_blank">Git'i yükleyin</a>

## <a name="download-the-sample"></a>Örneği indirme

Bir terminal penceresinde, örnek uygulama deposunu yerel makinenize kopyalamak için aşağıdaki komutu çalıştırın:

```bash
git clone https://github.com/Azure-Samples/ruby-docs-hello-world
```

## <a name="run-the-application-locally"></a>Uygulamayı yerel olarak çalıştırma

Uygulamanın çalışması için Rails sunucusu çalışır durumda olmalıdır. `hello-world` dizinine geçin ve `rails server` komutunu kullanarak sunucuyu başlatın.

```bash
cd hello-world\bin
rails server
```

Web tarayıcınızı kullanarak uygulamayı yerel olarak test etmek için `http://localhost:3000` yoluna gidin.

![Hello World yapılandırıldı](./media/quickstart-ruby/hello-world-configured.png)

[!INCLUDE [Try Cloud Shell](../../../includes/cloud-shell-try-it.md)]

[!INCLUDE [Configure deployment user](../../../includes/configure-deployment-user.md)]

[!INCLUDE [Create resource group](../../../includes/app-service-web-create-resource-group-linux.md)]

[!INCLUDE [Create app service plan](../../../includes/app-service-web-create-app-service-plan-linux.md)]

## <a name="create-a-web-app"></a>Web uygulaması oluşturma

[!INCLUDE [Create web app](../../../includes/app-service-web-create-web-app-ruby-linux-no-h.md)] 

Yerleşik görüntü ile yeni oluşturduğunuz web uygulamasını görmek için siteye göz atın. _&lt;app name>_ değerini kendi web uygulamanızın adıyla değiştirin.

```bash
http://<app_name>.azurewebsites.net
```

Yeni web uygulamanız aşağıdaki gibi görünmelidir:

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

Dağıtım tamamlandığında, dağıtımın etkili olması için burada gösterildiği gibi [`az webapp restart`](/cli/azure/webapp?view=azure-cli-latest#az-webapp-restart) komutunu kullanarak web uygulamanızı yeniden başlatın:

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
> [MySQL ile Ruby on Rails](tutorial-ruby-postgres-app.md)
