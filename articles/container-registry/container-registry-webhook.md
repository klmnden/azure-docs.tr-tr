---
title: Azure Container Registry Web kancaları
description: Kayıt defteri depolarınızda belirli eylemleri meydana geldiğinde olayları tetiklemeyi için Web kancalarını kullanma konusunda bilgi edinin.
services: container-registry
author: mmacy
manager: jeconnoc
ms.service: container-registry
ms.topic: article
ms.date: 12/02/2017
ms.author: marsma
ms.openlocfilehash: e85b69e452f4d76dfdf974698fa7d3b5cdbc0c30
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/02/2018
ms.locfileid: "39426097"
---
# <a name="using-azure-container-registry-webhooks"></a>Azure Container Registry Web kancalarını kullanma

Azure container registry, depolar ve özel Docker kapsayıcı görüntüleri, Docker Hub, genel Docker görüntülerini depolama benzer bir biçimde yönetir. Web kancaları belirli eylemleri kayıt defteri depolarınızı biri gerçekleştiğinde tetikleyici olayları kullanabilirsiniz. Web kancaları kayıt defteri düzeyinde olaylara yanıt verebilir veya belirli depo etiket aşağı kapsamlandırılabilir.

Web kancası isteklerden daha fazla ayrıntı için bkz. [Azure kapsayıcı kayıt defteri Web kancası şeması başvurusu](container-registry-webhook-reference.md).

## <a name="prerequisites"></a>Önkoşullar

* Azure kapsayıcı kayıt defteri - Azure aboneliğinizde bir kapsayıcı kayıt defteri oluşturun. Örneğin, [Azure portalında](container-registry-get-started-portal.md) veya [Azure CLI](container-registry-get-started-azure-cli.md).
* Docker CLI - yerel bilgisayarınızı bir Docker konağı olarak ayarlamak ve Docker CLI komutlarına erişmek için yükleme [Docker altyapısı](https://docs.docker.com/engine/installation/).

## <a name="create-webhook-azure-portal"></a>Azure portalında Web kancası oluştur

1. [Azure portalda](https://portal.azure.com) oturum açma
1. Bir Web kancası oluşturmak istediğiniz kapsayıcı kayıt defterine gidin.
1. Altında **Hizmetleri**seçin **Web kancaları**.
1. Seçin **Ekle** Web kancası araç.
1. Tamamlamak *Web kancası oluşturma* formunu aşağıdaki bilgilerle:

| Değer | Açıklama |
|---|---|
| Ad | Web kancası'na vermek istediğiniz adı. Yalnızca küçük harf ve rakam içerebilir ve 5-50 karakter uzunluğunda olmalıdır. |
| Hizmet URI'si | Web kancası posta bildirimleri gönderip burada URI'si. |
| Özel üst bilgiler | POST isteğini birlikte geçirmek istediğiniz üstbilgileri. İçinde olmalıdır "anahtar: değer" biçimi. |
| Eylem tetikleyici | Web kancası tetiklemenin eylemler. Web kancaları şu anda görüntü gönderme tarafından tetiklenebilir ve/veya eylemleri sil. |
| Durum | Web kancası oluşturulduktan sonra durumu. Bu, varsayılan olarak etkindir. |
| Kapsam | Web kancası çalıştığı kapsamı. Varsayılan olarak, kayıt defterini tüm olaylar için kapsamıdır. Onu bir depoya veya bir etiket için "depo: Etiket" biçimini kullanarak belirtilebilir. |

Örnek Web kancası formu:

![Azure portalında ACR Web kancası oluşturma kullanıcı Arabirimi](./media/container-registry-webhook/webhook.png)

## <a name="create-webhook-azure-cli"></a>Azure CLI Web kancası oluştur

Azure CLI kullanarak bir Web kancası oluşturmak için kullanın [az acr Web kancası oluşturma](/cli/azure/acr/webhook#az-acr-webhook-create) komutu.

```azurecli-interactive
az acr webhook create --registry mycontainerregistry --name myacrwebhook01 --actions delete --uri http://webhookuri.com
```

## <a name="test-webhook"></a>Web kancasını test et

### <a name="azure-portal"></a>Azure portal

Önceki kullanarak Web kancası kapsayıcısı üzerinde görüntü anında iletme ve silme eylemlerini, bunu test etmek **Ping** düğmesi. Ping belirtilen uç nokta için genel bir POST isteği gönderir ve yanıtını kaydeder. Ping özelliğini kullanarak, Web kancası doğru şekilde yapılandırdığınız doğrulamanıza yardımcı olabilir.

1. Test etmek istediğiniz Web kancasını seçin.
2. Üst araç çubuğunda, seçin **Ping**.
3. Uç noktanın yanıt iade **HTTP durum** sütun.

![Azure portalında ACR Web kancası oluşturma kullanıcı Arabirimi](./media/container-registry-webhook/webhook-02.png)

### <a name="azure-cli"></a>Azure CLI

Azure CLI ile bir ACR Web kancası test etmek için [az acr Web kancası ping](/cli/azure/acr/webhook#az-acr-webhook-ping) komutu.

```azurecli-interactive
az acr webhook ping --registry mycontainerregistry --name myacrwebhook01
```

Sonuçları görmek için [az acr Web kancası liste olayları](/cli/azure/acr/webhook#list-events) komutu.

```azurecli-interactive
az acr webhook list-events --registry mycontainerregistry08 --name myacrwebhook01
```

## <a name="delete-webhook"></a>Web kancasını sil

### <a name="azure-portal"></a>Azure portal

Web kancası seçerek her Web kancası silinebilir ve ardından **Sil** düğme Azure portalında.

### <a name="azure-cli"></a>Azure CLI

```azurecli-interactive
az acr webhook delete --registry mycontainerregistry --name myacrwebhook01
```

## <a name="next-steps"></a>Sonraki adımlar

[Azure kapsayıcı kayıt defteri Web kancası Şeması Başvurusu](container-registry-webhook-reference.md)