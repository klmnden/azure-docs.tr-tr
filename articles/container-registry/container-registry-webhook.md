---
title: "Azure kapsayıcı kayıt defteri Web kancaları"
description: "Web kancası belirli eylemleri, kayıt defteri depoları oluştuğunda tetikleyici olayları kullanmayı öğrenin."
services: container-registry
author: neilpeterson
manager: timlt
ms.service: container-registry
ms.topic: article
ms.date: 12/02/2017
ms.author: nepeters
ms.openlocfilehash: 915f90fd5d969d5544d56e5bec754b799f349015
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/01/2018
---
# <a name="using-azure-container-registry-webhooks"></a>Azure kapsayıcı kayıt defteri Web kancalarını kullanarak

Azure kapsayıcı kayıt defteri depolar ve özel Docker kapsayıcısı görüntüleri, benzer Docker hub'a genel Docker görüntüleri depolayan şekilde yönetir. Web kancası belirli eylemleri, kayıt defteri depoları biri gerçekleştiğinde tetikleyici olayları kullanabilirsiniz. Web kancası kayıt defteri düzeyinde olaylara yanıt verebilir veya belirli depo etiketi kadar kapsamlı.

Web kancası istekleri hakkında daha fazla bilgi için bkz: [Azure kapsayıcı kayıt defteri Web kancası şema başvurusu](container-registry-webhook-reference.md).

## <a name="prerequisites"></a>Önkoşullar

* Azure kapsayıcı kayıt defteri - Azure aboneliğinizde bir kapsayıcı kayıt oluşturun. Örneğin, [Azure portal](container-registry-get-started-portal.md) veya [Azure CLI](container-registry-get-started-azure-cli.md).
* Docker CLI - yerel bilgisayarınıza Docker ana bilgisayar olarak ayarlayabilir ve Docker CLI komutlara erişmek için yükleme [Docker altyapısına](https://docs.docker.com/engine/installation/).

## <a name="create-webhook-azure-portal"></a>Azure portal Web kancası oluşturma

1. Oturum [Azure portalı](https://portal.azure.com)
1. Bir Web kancası oluşturmak istediğiniz kapsayıcı kayıt defterine gidin.
1. Altında **Hizmetleri**seçin **Kancalarını**.
1. Seçin **Ekle** Web kancası araç.
1. Tamamlamak *Web kancası oluşturma* form aşağıdaki bilgilerle:

| Değer | Açıklama |
|---|---|
| Ad | Web kancası için vermek istediğiniz adı. Yalnızca küçük harfler ve sayılar içerebilir ve 5-50 karakter uzunluğunda olmalıdır. |
| Hizmet URI'si | Web kancası POST bildirimleri burada göndermesi gereken URI. |
| Özel üst bilgiler | Üstbilgiler POST istekle birlikte geçirmek isteyebilirsiniz. İçinde olmalıdır "anahtar: değer" biçimi. |
| Tetikleyici eylemleri | Web kancası tetiklemek eylemler. Web kancası görüntü itme tarafından şu anda tetiklenebilir ve/veya eylemlerini silme. |
| Durum | Web kancası oluşturulduktan sonra durumu. Varsayılan olarak etkindir. |
| Kapsam | Web kancası çalıştığı kapsamı. Varsayılan olarak, kayıt defterinde tüm olayları kapsamı içindir. Bu depo veya bir etiket için "deposu: Etiket" biçimini kullanarak belirtilebilir. |

Örnek Web kancası form:

![Azure portalında ACR Web kancası oluşturma kullanıcı Arabirimi](./media/container-registry-webhook/webhook.png)

## <a name="create-webhook-azure-cli"></a>Web kancası Azure CLI oluşturma

Azure CLI kullanarak bir Web kancası oluşturmak üzere kullanmanız [az acr Web kancası oluşturma](/cli/azure/acr/webhook#az_acr_webhook_create) komutu.

```azurecli-interactive
az acr webhook create --registry mycontainerregistry --name myacrwebhook01 --actions delete --uri http://webhookuri.com
```

## <a name="test-webhook"></a>Test Web kancası

### <a name="azure-portal"></a>Azure portalına

Kullanarak önceki kapsayıcısı üzerinde Web kancası görüntü gönderme ve silme eylemlerini, kendisiyle test **Ping** düğmesi. Ping belirtilen uç nokta için genel bir POST isteği gönderir ve yanıt günlüğe kaydeder. Ping özelliğini kullanarak, Web kancası doğru şekilde yapılandırdığınız doğrulamanıza yardımcı olabilir.

1. Test etmek istediğiniz Web kancası seçin.
2. Üst araç çubuğunda seçin **Ping**.
3. Uç noktanın yanıt iade **HTTP durum** sütun.

![Azure portalında ACR Web kancası oluşturma kullanıcı Arabirimi](./media/container-registry-webhook/webhook-02.png)

### <a name="azure-cli"></a>Azure CLI

Azure CLI ile bir ACR Web kancası sınamak için kullanın [az acr Web kancası ping](/cli/azure/acr/webhook#az_acr_webhook_ping) komutu.

```azurecli-interactive
az acr webhook ping --registry mycontainerregistry --name myacrwebhook01
```

Sonuçları görmek için [az acr Web kancası listesi-olayları](/cli/azure/acr/webhook#list-events) komutu.

```azurecli-interactive
az acr webhook list-events --registry mycontainerregistry08 --name myacrwebhook01
```

## <a name="delete-webhook"></a>Web kancasını sil

### <a name="azure-portal"></a>Azure portalına

Web kancası seçerek her Web kancası silinebilir ve ardından **silmek** düğme Azure portalında.

### <a name="azure-cli"></a>Azure CLI

```azurecli-interactive
az acr webhook delete --registry mycontainerregistry --name myacrwebhook01
```

## <a name="next-steps"></a>Sonraki adımlar

[Azure kapsayıcı kayıt defteri Web kancası şema başvurusu](container-registry-webhook-reference.md)