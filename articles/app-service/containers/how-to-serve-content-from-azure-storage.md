---
title: Linux'ta - App Service, Azure Depolama'dan içerik sunma
description: Yapılandırma ve Linux üzerinde Azure App Service'te Azure Depolama'dan içerik sunma hakkında.
author: msangapu
manager: jeconnoc
ms.service: app-service
ms.workload: web
ms.topic: article
ms.date: 2/04/2019
ms.author: msangapu-msft
ms.openlocfilehash: 6b4e145a693aabbf1a00d732e2fd602e7c887a03
ms.sourcegitcommit: 24fd3f9de6c73b01b0cee3bcd587c267898cbbee
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2019
ms.locfileid: "65956019"
---
# <a name="serve-content-from-azure-storage-in-app-service-on-linux"></a>Linux üzerinde App Service'te Azure Depolama'dan içerik sunma

Bu kılavuzu kullanarak Linux üzerinde App Service'te statik içerik sunmak gösterilmektedir [Azure depolama](/azure/storage/common/storage-introduction). Güvenli içerik, içerik taşıma, birden fazla uygulama ve birden çok aktarma yöntemlere erişim avantajına sahip olur. 

## <a name="prerequisites"></a>Önkoşullar

- Mevcut bir web uygulamasını (kapsayıcılar için Web uygulaması ya da Linux üzerinde App Service'te).
- [Azure CLI](/cli/azure/install-azure-cli) (2.0.46 veya üzeri).

## <a name="create-azure-storage"></a>Azure depolama oluşturma

> [!NOTE]
> Azure depolama, varsayılan olmayan depolama ve ayrı olarak faturalandırılır, web uygulamasına dahil değildir.
>

Azure oluşturma [Azure depolama hesabı](https://docs.microsoft.com/azure/storage/common/storage-quickstart-create-account?tabs=azure-cli).

```azurecli
#Create Storage Account
az storage account create --name <storage_account_name> --resource-group myResourceGroup

#Create Storage Container
az storage container create --name <storage_container_name> --account-name <storage_account_name>
```

## <a name="upload-files-to-azure-storage"></a>Azure Depolama'ya dosya yükleme

Depolama hesabı için yerel bir dizine yüklemek için kullandığınız [ `az storage blob upload-batch` ](https://docs.microsoft.com/cli/azure/storage/blob?view=azure-cli-latest#az-storage-blob-upload-batch) komutunu aşağıdaki örneğe benzer:

```azurecli
az storage blob upload-batch -d <full_path_to_local_directory> --account-name <storage_account_name> --account-key "<access_key>" -s <source_location_name>
```

## <a name="link-storage-to-your-web-app-preview"></a>Web uygulamanıza (Önizleme) depolama Bağla

> [!CAUTION]
> Varolan bir dizin bir web uygulamasında bir depolama hesabına bağlama dizin içeriğinin silinmesine neden olur. Mevcut bir uygulamaya geçirme dosyası yoksa, başlamadan önce uygulamanız ve içeriğinin bir yedeği olun.
>

App Service uygulamanız bir dizinde bir depolama hesabına bağlamak için kullandığınız [ `az webapp config storage-account add` ](https://docs.microsoft.com/cli/azure/webapp/config/storage-account?view=azure-cli-latest#az-webapp-config-storage-account-add) komutu. Depolama türü AzureBlob veya depolamasını Azure dosyalarına olabilir. Bu kapsayıcı için AzureBlob kullanırsınız.

```azurecli
az webapp config storage-account add --resource-group <group_name> --name <app_name> --custom-id <custom_id> --storage-type AzureBlob --share-name <share_name> --account-name <storage_account_name> --access-key "<access_key>" --mount-path <mount_path_directory>
```

Bir depolama hesabına bağlanması için istediğiniz herhangi bir dizin için bunu yapmalısınız.

## <a name="verify"></a>Doğrula

Bir depolama kapsayıcısı bir web uygulamasına bağlandığında, aşağıdaki komutu çalıştırarak bunu doğrulayabilirsiniz:

```azurecli
az webapp config storage-account list --resource-group <resource_group> --name <app_name>
```

## <a name="use-custom-storage-in-docker-compose"></a>Docker Compose özel depolama kullanma

Azure depolama ile çok kapsayıcılı uygulamaları özel kimliğini kullanarak bağlanabilir. Özel kimlik adı görüntülemek için çalıştırın [ `az webapp config storage-account list --name <app_name> --resource-group <resource_group>` ](/cli/azure/webapp/config/storage-account?view=azure-cli-latest#az-webapp-config-storage-account-list).

İçinde *docker-compose.yml* dosya, harita `volumes` seçeneğini `custom-id`. Örneğin:

```yaml
wordpress:
  image: wordpress:latest
  volumes:
  - <custom-id>:<path_in_container>
```

## <a name="next-steps"></a>Sonraki adımlar

- [Azure App Service'te Web uygulamalarını yapılandırma](../configure-common.md).
