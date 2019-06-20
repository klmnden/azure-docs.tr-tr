---
title: Ruby uygulamaları - Azure App Service'ı yapılandırma
description: Bu öğreticide, yazma ve Linux üzerinde Azure App Service için Ruby uygulamaları yapılandırma seçenekleri açıklanmaktadır.
services: app-service\web
documentationcenter: ''
author: cephalin
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 03/28/2019
ms.author: astay;cephalin;kraigb
ms.custom: seodec18
ms.openlocfilehash: aec85b16ab0357b28a564f75509e147e5555137b
ms.sourcegitcommit: b7a44709a0f82974578126f25abee27399f0887f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2019
ms.locfileid: "67205057"
---
# <a name="configure-a-linux-ruby-app-for-azure-app-service"></a>Bir Linux Ruby uygulamasını Azure App Service için yapılandırma

Bu makalede nasıl [Azure App Service](app-service-linux-intro.md) Ruby uygulamaları ve gerektiğinde App Service'in davranışını nasıl özelleştirebileceğiniz çalışır. Ruby uygulamaları dağıtılması gerekir tüm gerekli [pip](https://pypi.org/project/pip/) modüller.

Bu kılavuzu temel kavramları ve App Service'te yerleşik bir Linux kapsayıcı kullanan Ruby geliştiricileri için yönergeler sağlar. Azure App Service daha önce kullanmadıysanız izlemelidir [Ruby hızlı](quickstart-ruby.md) ve [PostgreSQL öğreticisiyle Ruby](tutorial-ruby-postgres-app.md) ilk.

## <a name="show-ruby-version"></a>Ruby sürümü göster

Geçerli Ruby sürümü göstermek için aşağıdaki komutu çalıştırın [Cloud Shell](https://shell.azure.com):

```azurecli-interactive
az webapp config show --resource-group <resource-group-name> --name <app-name> --query linuxFxVersion
```

Tüm desteklenen sürümleri Ruby göstermek için aşağıdaki komutu çalıştırın [Cloud Shell](https://shell.azure.com):

```azurecli-interactive
az webapp list-runtimes --linux | grep RUBY
```

Ruby desteklenmeyen bir sürümünü, bunun yerine kendi kapsayıcı görüntünüzü oluşturarak çalıştırabilirsiniz. Daha fazla bilgi için [özel bir Docker görüntüsü kullanma](tutorial-custom-docker-image.md).

## <a name="set-ruby-version"></a>Ruby sürümünü ayarlama

Aşağıdaki komutu çalıştırın [Cloud Shell](https://shell.azure.com) 2.3 için Ruby sürümü ayarlamak için:

```azurecli-interactive
az webapp config set --resource-group <resource-group-name> --name <app-name> --linux-fx-version "RUBY|2.3"
```

> [!NOTE]
> Dağıtım süre boyunca aşağıdakilere benzer hatalar görürseniz:
> ```
> Your Ruby version is 2.3.3, but your Gemfile specified 2.3.1
> ```
> or
> ```
> rbenv: version `2.3.1' is not installed
> ```
> Projenizde yapılandırılmış Ruby sürümü çalıştırdığınızdan kapsayıcısında yüklü olan sürümden farklı olduğu anlamına gelir (`2.3.3` Yukarıdaki örnekteki). Yukarıdaki örnekte, her ikisini de denetlemek *Gemfile* ve *.ruby sürüm* ve Ruby sürümü ayarlanmadı çalıştırdığınızdan kapsayıcısında yüklü sürümü olarak ayarlanmış olduğunu doğrulayın veya (`2.3.3` içinde Yukarıdaki örnekte).

## <a name="access-environment-variables"></a>Ortam değişkenlerine erişme

Uygulama hizmetinde [uygulama ayarlarını belirlemek](../configure-common.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json#configure-app-settings) , uygulama kodunuz dışında. Standardını kullanarak bunlar erişebilir [ENV ['< yol-name >']](https://ruby-doc.org/core-2.3.3/ENV.html) deseni. Örneğin, bir uygulama ayarı erişmeye adlı `WEBSITE_SITE_NAME`, aşağıdaki kodu kullanın:

```ruby
ENV['WEBSITE_SITE_NAME']
```

## <a name="customize-deployment"></a>Dağıtım özelleştirme

Dağıtırken bir [Git deposu](../deploy-local-git.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json), veya bir [Zip paketini](../deploy-zip.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json) açık yapı işlemleri ile dağıtım altyapısı (Kudu) otomatik olarak aşağıdaki dağıtım sonrası adımları çalıştırır:

1. Kontrol bir *Gemfile* bulunmaktadır.
1. `bundle clean` öğesini çalıştırın. 
1. `bundle install --path "vendor/bundle"` öğesini çalıştırın.
1. Çalıştırma `bundle package` paket toprağa değerli taşlar satıcı/önbellek klasörüne için.

### <a name="use---without-flag"></a>--Bayrağı olmadan kullanın

Çalıştırılacak `bundle install` ile [--olmadan](https://bundler.io/man/bundle-install.1.html) bayrak olarak ayarlayın `BUNDLE_WITHOUT` [uygulama ayarı](../configure-common.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json#configure-app-settings) grupları virgülle ayrılmış listesi. Örneğin, aşağıdaki komut, ayarlar `development,test`.

```azurecli-interactive
az webapp config appsettings set --name <app-name> --resource-group <resource-group-name> --settings BUNDLE_WITHOUT="development,test"
```

Bu ayar tanımlanan sonra dağıtım altyapısında çalışan `bundle install` ile `--without $BUNDLE_WITHOUT`.

### <a name="precompile-assets"></a>Varlıkları önceden derleme

Dağıtım sonrası adımları varlıkları varsayılan olarak önceden derleme yok. Varlık ön derleme üzerinde etkinleştirmek için ayarlanmış `ASSETS_PRECOMPILE` [uygulama ayarı](../configure-common.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json#configure-app-settings) için `true`. Komut `bundle exec rake --trace assets:precompile` dağıtım sonrası adımları sonunda çalıştırın. Örneğin:

```azurecli-interactive
az webapp config appsettings set --name <app-name> --resource-group <resource-group-name> --settings ASSETS_PRECOMPILE=true
```

Daha fazla bilgi için [hizmet statik varlıklar](#serve-static-assets).

## <a name="customize-start-up"></a>Başlatmayı özelleştirme

Varsayılan olarak, Ruby kapsayıcısına Rails sunucusunu şu sırayla başlatılır (daha fazla bilgi için [başlatma betiği](https://github.com/Azure-App-Service/ruby/blob/master/2.3.8/startup.sh)):

1. Oluşturmak bir [secret_key_base](https://edgeguides.rubyonrails.org/security.html#environmental-security) değer zaten yoksa. Uygulamayı üretim modunda çalıştırmak için bu değer gereklidir.
1. Ayarlama `RAILS_ENV` ortam değişkenine `production`.
1. Silmek *.pid* dosyası *tmp/pids* tarafından daha önce çalışan bir Rails sunucusunu sol dizin.
1. Tüm bağımlılıkların yüklü olup olmadığını denetleyin. Aksi takdirde, yerel toprağa değerli taşlar yükleme deneyin *satıcı/önbellek* dizin.
1. `rails server -e $RAILS_ENV` öğesini çalıştırın.

Başlatma işlemi aşağıdaki şekillerde özelleştirebilirsiniz:

- [Statik varlıklar hizmet](#serve-static-assets)
- [Üretim dışı modunda çalıştır](#run-in-non-production-mode)
- [Secret_key_base el ile ayarlama](#set-secret_key_base-manually)

### <a name="serve-static-assets"></a>Statik varlıklar hizmet

Rails sunucusunu Ruby kapsayıcısında, varsayılan olarak, üretim modunda çalışır ve [varlıklar önceden derlenmiş ve web sunucunuz tarafından sunulan varsayar](https://guides.rubyonrails.org/asset_pipeline.html#in-production). Rails sunucusunu statik varlıklarından görmesi için iki işlem yapmanız gerekir:

- **Varlıkları ön derleme** - [statik varlıkları yerel olarak ön derleme](https://guides.rubyonrails.org/asset_pipeline.html#local-precompilation) ve bunları el ile dağıtabilirsiniz. Veya bunun yerine işlemek dağıtım altyapısı sağlar (bkz [varlıklar ön derleme](#precompile-assets).
- **Statik dosyaları sunma etkinleştirme** - Ruby kapsayıcısına statik varlıklarından sunmak için ayarlayın `RAILS_SERVE_STATIC_FILES` [ayarlamak `RAILS_SERVE_STATIC_FILES` uygulama ayarı](../configure-common.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json#configure-app-settings) için `true`. Örneğin:

    ```azurecli-interactive
    az webapp config appsettings set --name <app-name> --resource-group <resource-group-name> --settings RAILS_SERVE_STATIC_FILES=true
    ```

### <a name="run-in-non-production-mode"></a>Üretim dışı modunda çalıştır

Rails sunucusunu üretim modunda çalıştırır. Örneğin, geliştirme modunda çalışacak şekilde ayarlamak `RAILS_ENV` [uygulama ayarı](../configure-common.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json#configure-app-settings) için `development`.

```azurecli-interactive
az webapp config appsettings set --name <app-name> --resource-group <resource-group-name> --settings RAILS_ENV="development"
```

Ancak, bu ayar tek başına localhost yalnızca ister ve kapsayıcı dışında erişilebilir değildir kabul geliştirme modunda başlatmak Rails sunucusunu neden olur. Uzak istemci isteklerini kabul etmek için ayarlama `APP_COMMAND_LINE` [uygulama ayarı](../configure-common.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json#configure-app-settings) için `rails server -b 0.0.0.0`. Bu uygulama ayarı, özel komut Ruby kapsayıcısında çalıştırmanıza olanak tanır. Örneğin:

```azurecli-interactive
az webapp config appsettings set --name <app-name> --resource-group <resource-group-name> --settings APP_COMMAND_LINE="rails server -b 0.0.0.0"
```

### <a name="set-secret_key_base-manually"></a> Secret_key_base el ile ayarlama

Kendi kullanılacak `secret_key_base` ayarlayın, sizin için oluşturur. App Service izin vermek yerine değer `SECRET_KEY_BASE` [uygulama ayarı](../configure-common.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json#configure-app-settings) istediğiniz değer. Örneğin:

```azurecli-interactive
az webapp config appsettings set --name <app-name> --resource-group <resource-group-name> --settings SECRET_KEY_BASE="<key-base-value>"
```

## <a name="access-diagnostic-logs"></a>Tanılama günlüklerine erişim

[!INCLUDE [Access diagnostic logs](../../../includes/app-service-web-logs-access-no-h.md)]

## <a name="open-ssh-session-in-browser"></a>Tarayıcıda SSH oturum aç

[!INCLUDE [Open SSH session in browser](../../../includes/app-service-web-ssh-connect-builtin-no-h.md)]

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Öğretici: PostgreSQL ile Rails uygulaması](tutorial-ruby-postgres-app.md)

> [!div class="nextstepaction"]
> [App Service Linux SSS](app-service-linux-faq.md)
