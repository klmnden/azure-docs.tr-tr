---
title: Azure Media Services v3 API'sine - Python bağlanma
description: Media Services v3 API Python ile bağlanma hakkında bilgi edinin.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/15/2019
ms.author: juliako
ms.openlocfilehash: 971e36b600a2c6be516e39ce84ca5780a2f23bbd
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60733105"
---
# <a name="connect-to-media-services-v3-api---python"></a>Media Services v3 API'sine - Python bağlanma

Bu makale için Azure Media Services v3 Python SDK'sına nasıl hizmet sorumlusu oturum açma yöntemiyle gösterir.

## <a name="prerequisites"></a>Önkoşullar

- Python'dan indirme [python.org](https://www.python.org/downloads/)
- Ayarladığınızdan emin olun `PATH` ortam değişkeni
- [Bir Media Services hesabı oluşturma](create-account-cli-how-to.md). Kaynak grubu adı ve Media Services hesap adını hatırlamak emin olun.
- Bağlantısındaki [erişimi API'leri](access-api-cli-how-to.md) konu. Sonraki adımda, abonelik kimliği, uygulama kimliği (istemci kimliği), kimlik doğrulama anahtarı (gizli) ve gereken Kiracı kimliği kaydedin.

## <a name="install-the-modules"></a>Modülleri yükleme

Python kullanarak Azure Media Services ile çalışmak için bu modül yüklemeniz gerekir.

* `azure-mgmt-resource` Modülü için Active Directory Azure modüllerini içerir.
* `azure-mgmt-media` Modülü Media Services varlıkları içerir.

Bir komut satırı aracı'nı açın ve modülleri yüklemek için aşağıdaki komutları kullanın.

```
pip3 install azure-mgmt-resource
pip3 install azure-mgmt-media==1.1.1
```

## <a name="connect-to-the-python-client"></a>Python istemciye bağlanma

1. Bir dosya oluşturun bir `.py` uzantısı
1. Sık kullandığınız düzenleyicinizde dosyasını açın
1. Dosyaya aşağıdaki kodu ekleyin. Kod gerekli modülleri içeri aktarır ve Media Services'e bağlanmak için gereken Active Directory kimlik bilgileri nesnesi oluşturur.

      Değişkenleri değerleri, aradığınızı değerleri belirlemek [erişimi API'leri](access-api-cli-how-to.md)

      ```
      import adal
      from msrestazure.azure_active_directory import AdalAuthentication
      from msrestazure.azure_cloud import AZURE_PUBLIC_CLOUD
      from azure.mgmt.media import AzureMediaServices
      from azure.mgmt.media.models import MediaService

      RESOURCE = 'https://management.core.windows.net/'
      # Tenant ID for your Azure Subscription
      TENANT_ID = '00000000-0000-0000-000000000000'
      # Your Service Principal App ID
      CLIENT = '00000000-0000-0000-000000000000'
      # Your Service Principal Password
      KEY = '00000000-0000-0000-000000000000'
      # Your Azure Subscription ID
      SUBSCRIPTION_ID = '00000000-0000-0000-000000000000'
      # Your Azure Media Service (AMS) Account Name
      ACCOUNT_NAME = 'amsv3account'
      # Your Resource Group Name
      RESOUCE_GROUP_NAME = 'AMSv3ResourceGroup'

      LOGIN_ENDPOINT = AZURE_PUBLIC_CLOUD.endpoints.active_directory
      RESOURCE = AZURE_PUBLIC_CLOUD.endpoints.active_directory_resource_id

      context = adal.AuthenticationContext(LOGIN_ENDPOINT + '/' + TENANT_ID)
      credentials = AdalAuthentication(
          context.acquire_token_with_client_credentials,
          RESOURCE,
          CLIENT,
          KEY
      )

      # The AMS Client
      # You can now use this object to perform different operations to your AMS account.
      client = AzureMediaServices(credentials, SUBSCRIPTION_ID)

      print("signed in")

      # Now that you are authenticated, you can manipulate the entities.
      # For example, list assets in your AMS account
      print (client.assets.list(RESOUCE_GROUP_NAME, ACCOUNT_NAME).get(0))
      ```

1. Dosyayı çalıştırın

## <a name="next-steps"></a>Sonraki adımlar

- Kullanım [Python SDK'sı](https://aka.ms/ams-v3-python-sdk).
- Media Services gözden [Python ref](https://aka.ms/ams-v3-python-ref) belgeleri.
