---
title: "Azure kapsayıcı kayıt defteri Web kancaları"
description: "Web kancası belirli eylemleri, kayıt defteri depoları oluştuğunda tetikleyici olayları kullanmayı öğrenin."
services: container-registry
documentationcenter: 
author: neilpeterson
manager: timlt
editor: 
tags: acr, azure-container-registry
keywords: "Docker, kapsayıcıları, ACR"
ms.assetid: 
ms.service: container-registry
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/08/2017
ms.author: nepeters
ms.openlocfilehash: 5a9dab91aafb92f944b473f05144242143e36477
ms.sourcegitcommit: ccb84f6b1d445d88b9870041c84cebd64fbdbc72
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/14/2017
---
# <a name="using-azure-container-registry-webhooks"></a>Azure kapsayıcı kayıt defteri Web kancalarını kullanarak

Azure kapsayıcı kayıt defteri depolar ve özel Docker kapsayıcısı görüntüleri, benzer Docker hub'a genel Docker görüntüleri depolayan şekilde yönetir. Web kancalarını belirli eylemleri, kayıt defteri depoları biri gerçekleştiğinde tetikleyici olaylar için kullanın. Web kancası kayıt defteri düzeyinde olaylara yanıt verebilir veya belirli depo etiketi kadar kapsamlı.

Daha fazla arka plan ve kavramları için bkz: [hakkında Azure kapsayıcı kayıt defteri](./container-registry-intro.md).

## <a name="prerequisites"></a>Ön koşullar

- Azure kapsayıcı kayıt defteri - yönetilen Azure aboneliğinizde bir yönetilen kapsayıcı kayıt oluşturun. Örneğin, [Azure portal](container-registry-get-started-portal.md) veya [Azure CLI](container-registry-get-started-azure-cli.md).
- Docker CLI - yerel bilgisayarınıza Docker ana bilgisayar olarak ayarlayabilir ve Docker CLI komutlara erişmek için yükleme [Docker altyapısına](https://docs.docker.com/engine/installation/).

## <a name="create-webhook-azure-portal"></a>Azure portal Web kancası oluşturma

1. Oturum [Azure portal](https://portal.azure.com) ve Web kancalarını oluşturmak istediğiniz kayıt defteri gidin.

2. Kapsayıcı dikey penceresinde, seçin **Kancalarını** altında **Hizmetleri**.

3. Seçin **Ekle** Web kancası dikey araç.

4. Tamamlamak *Web kancası oluşturma* form aşağıdaki bilgilerle:

| Değer | Açıklama |
|---|---|
| Ad | Web kancası için vermek istediğiniz adı. Yalnızca küçük harfler ve sayılar içerebilir ve 5-50 karakter uzunluğunda olmalıdır. |
| Hizmet URI'si | Web kancası POST bildirimleri burada göndermesi gereken URI. |
| Özel Üstbilgileri | Üstbilgiler POST istekle birlikte geçirmek isteyebilirsiniz. İçinde olmalıdır "anahtar: değer" biçimi. |
| Tetikleyici eylemleri | Web kancası tetiklemek eylemler. Web kancası görüntü itme tarafından şu anda tetiklenebilir ve/veya eylemlerini silme. |
| Durum | Web kancası oluşturulduktan sonra durumu. Varsayılan olarak etkindir. |
| Kapsam | Web kancası çalıştığı kapsamı. Varsayılan olarak, kayıt defterinde tüm olayları kapsamı içindir. Bu depo veya bir etiket için "deposu: Etiket" biçimini kullanarak belirtilebilir. |

Örnek Web kancası form:

![Azure portalında ACR Web kancası oluşturma kullanıcı Arabirimi](./media/container-registry-webhook/webhook.png)

## <a name="create-webhook-azure-cli"></a>Web kancası Azure CLI oluşturma

Azure CLI kullanarak bir Web kancası oluşturmak üzere kullanmanız [az acr Web kancası oluşturma](/cli/azure/acr/webhook#create) komutu.

```azurecli-interactive
az acr webhook create --registry mycontainerregistry --name myacrwebhook01 --actions delete --uri http://webhookuri.com
```

## <a name="test-webhook"></a>Test Web kancası

### <a name="azure-portal"></a>Azure portalına

Kullanarak önceki kapsayıcısı üzerinde Web kancası görüntü gönderme ve silme eylemlerini, kendisiyle test **Ping** düğmesi. Ping belirtilen uç nokta için genel bir POST isteği gönderir ve yanıt günlüğe kaydeder. Bu, Web kancası doğru şekilde yapılandırdığınız doğrulamanıza yardımcı olabilir.

1. Test etmek istediğiniz Web kancası seçin.
2. Üst araç çubuğunda seçin **Ping**.
3. Uç noktanın yanıt iade **HTTP durum** sütun.

![Azure portalında ACR Web kancası oluşturma kullanıcı Arabirimi](./media/container-registry-webhook/webhook-02.png)

### <a name="azure-cli"></a>Azure CLI

Azure CLI ile bir ACR Web kancası sınamak için kullanın [az acr Web kancası ping](/cli/azure/acr/webhook#ping) komutu.

```azurecli-interactive
az acr webhook ping --registry mycontainerregistry --name myacrwebhook01
```

Sonuçları görmek için [az acr Web kancası listesi-olayları](/cli/azure/acr/webhook#list-events) komutu.

```azurecli-interactive
az acr webhook list-events --registry mycontainerregistry08 --name myacrwebhook01
```

## <a name="delete-webhook"></a>Web kancası silme

### <a name="azure-portal"></a>Azure portalına

Web kancası seçerek her Web kancası silinebilir ve ardından **silmek** düğme Azure portalında.

### <a name="azure-cli"></a>Azure CLI

```azurecli-interactive
az acr webhook delete --registry mycontainerregistry --name myacrwebhook01
```
