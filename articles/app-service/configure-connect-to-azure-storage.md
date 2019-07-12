---
title: Azure dosyaları'nı kullanarak depolama yapılandırma
description: Yapılandırma ve Azure App Service'te Windows kapsayıcı dosyalarında bağlamayla.
author: msangapu-msft
manager: gwallace
ms.service: app-service
ms.workload: web
ms.topic: article
ms.date: 7/01/2019
ms.author: msangapu-msft
ms.openlocfilehash: ea6555e8459b8f372a73d7f943e9a96523d4c791
ms.sourcegitcommit: 1572b615c8f863be4986c23ea2ff7642b02bc605
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2019
ms.locfileid: "67789052"
---
# <a name="configure-azure-files-in-a-windows-container-on-app-service"></a>App Service üzerindeki bir Windows kapsayıcısında Azure dosyaları'nı yapılandırma

> [!NOTE]
> Bu makalede, özel Windows kapsayıcılarına uygulanır. App Service dağıtmak için _Linux_, bkz: [Azure Depolama'dan içerik hizmet](./containers/how-to-serve-content-from-azure-storage.md).
>

Bu kılavuz, Azure Depolama'da Windows kapsayıcıları nasıl gösterir. Yalnızca [Azure dosya paylaşımlarını](https://docs.microsoft.com/azure/storage/files/storage-how-to-use-files-cli) ve [Premium dosya paylaşımlarını](https://docs.microsoft.com/azure/storage/files/storage-how-to-create-premium-fileshare) desteklenir. Bu nasıl yapılır makalesinde Azure dosya paylaşımlarını kullanırsınız. Güvenli içerik, içerik taşıma, birden fazla uygulama ve birden çok aktarma yöntemlere erişim avantajına sahip olur.

## <a name="prerequisites"></a>Önkoşullar

- [Azure CLI](/cli/azure/install-azure-cli) (2.0.46 veya üzeri).
- [Azure App Service'te mevcut bir Windows kapsayıcı uygulaması](https://docs.microsoft.com/azure/app-service/app-service-web-get-started-windows-container)
- [Azure dosya paylaşımı oluşturma](https://docs.microsoft.com/azure/storage/files/storage-how-to-use-files-cli)
- [Azure dosya paylaşımına dosya yükleme](https://docs.microsoft.com/azure/storage/files/storage-files-deployment-guide)

> [!NOTE]
> Azure dosyaları, varsayılan olmayan depolama ve ayrı olarak faturalandırılır, web uygulamasına dahil değildir. Güvenlik duvarı yapılandırması nedeniyle altyapı sınırlamaları kullanarak desteklemiyor.
>

## <a name="link-storage-to-your-web-app-preview"></a>Web uygulamanıza (Önizleme) depolama Bağla

 App Service uygulamanızda bir dizine Azure dosya paylaşımını bağlayabilmeniz için kullandığınız [ `az webapp config storage-account add` ](https://docs.microsoft.com/cli/azure/webapp/config/storage-account?view=azure-cli-latest#az-webapp-config-storage-account-add) komutu. Depolama türü depolamasını Azure dosyalarına olması gerekir.

```azurecli
az webapp config storage-account add --resource-group <group_name> --name <app_name> --custom-id <custom_id> --storage-type AzureFiles --share-name <share_name> --account-name <storage_account_name> --access-key "<access_key>" --mount-path <mount_path_directory of form c:<directory name> >
```

Bir Azure dosyaları'na bağlanmasını istediğiniz diğer dizinleri paylaşmak için bunu yapmanız gerekir.

## <a name="verify"></a>Doğrulama

Azure dosyaları paylaşımına bir web uygulamasına bağlandığında, aşağıdaki komutu çalıştırarak bunu doğrulayabilirsiniz:

```azurecli
az webapp config storage-account list --resource-group <resource_group> --name <app_name>
```


## <a name="next-steps"></a>Sonraki adımlar

- [Bir ASP.NET uygulamasını bir Windows kapsayıcı (Önizleme) kullanarak Azure App Service'e geçirme](app-service-web-tutorial-windows-containers-custom-fonts.md).