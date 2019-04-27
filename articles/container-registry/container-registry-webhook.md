---
title: Azure Container Registry Web kancaları
description: Kayıt defteri depolarınızda belirli eylemleri meydana geldiğinde olayları tetiklemeyi için Web kancalarını kullanma konusunda bilgi edinin.
services: container-registry
author: dlepow
ms.service: container-registry
ms.topic: article
ms.date: 03/14/2019
ms.author: danlep
ms.openlocfilehash: 0a3d2d0e858dc052095c0a58287970d10c06f0ba
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60787283"
---
# <a name="using-azure-container-registry-webhooks"></a>Azure Container Registry Web kancalarını kullanma

Azure container registry, depolar ve özel Docker kapsayıcı görüntüleri, Docker Hub, genel Docker görüntülerini depolama benzer bir biçimde yönetir. Depoları için barındırabilir [Helm grafikleri](container-registry-helm-repos.md) (Önizleme), bir paketleme biçimlendirme Kubernetes uygulamaları dağıtmak için. Web kancaları belirli eylemleri kayıt defteri depolarınızı biri gerçekleştiğinde tetikleyici olayları kullanabilirsiniz. Web kancaları kayıt defteri düzeyinde olaylara yanıt verebilir veya belirli depo etiket aşağı kapsamlandırılabilir.

Web kancası isteklerden daha fazla ayrıntı için bkz. [Azure kapsayıcı kayıt defteri Web kancası şeması başvurusu](container-registry-webhook-reference.md).

## <a name="prerequisites"></a>Önkoşullar

* Azure kapsayıcı kayıt defteri - Azure aboneliğinizde bir kapsayıcı kayıt defteri oluşturun. Örneğin, [Azure portalında](container-registry-get-started-portal.md) veya [Azure CLI](container-registry-get-started-azure-cli.md). [Azure Container Registry SKU'ları](container-registry-skus.md) farklı Web kancaları kotalar sahip.
* Docker CLI - yerel bilgisayarınızı bir Docker konağı olarak ayarlamak ve Docker CLI komutlarına erişmek için yükleme [Docker altyapısı](https://docs.docker.com/engine/installation/).

## <a name="create-webhook---azure-portal"></a>Web kancası - Azure portal'ı oluşturma

1. [Azure Portal](https://portal.azure.com) oturum açın.
1. Bir Web kancası oluşturmak istediğiniz kapsayıcı kayıt defterine gidin.
1. Altında **Hizmetleri**seçin **Web kancaları**.
1. Seçin **Ekle** Web kancası araç.
1. Tamamlamak *Web kancası oluşturma* formunu aşağıdaki bilgilerle:

| Değer | Açıklama |
|---|---|
| Ad | Web kancası'na vermek istediğiniz adı. Yalnızca harf ve rakam içerebilir ve 5-50 karakter uzunluğunda olmalıdır. |
| Hizmet URI'si | Web kancası posta bildirimleri gönderip burada URI'si. |
| Özel üst bilgiler | POST isteğini birlikte geçirmek istediğiniz üstbilgileri. İçinde olmalıdır "anahtar: değer" biçimi. |
| Eylem tetikleyici | Web kancası tetiklemenin eylemler. Görüntü gönderme, görüntü silme, Helm grafiği anında iletme, Helm grafiği silme ve görüntü karantina Eylemler içerir. Web kancası tetiklemenin bir veya daha fazla eylem seçebilirsiniz. |
| Durum | Web kancası oluşturulduktan sonra durumu. Bu, varsayılan olarak etkindir. |
| Kapsam | Web kancası çalıştığı kapsamı. Belirtilmezse, kayıt defterini tüm olaylar için kapsamıdır. Bu bir depo veya bir etiket için "depo: Etiket" biçimini kullanarak belirtilebilir veya "depo: *" bir depo altındaki tüm etiketlere yönelik. |

Örnek Web kancası formu:

![Azure portalında ACR Web kancası oluşturma kullanıcı Arabirimi](./media/container-registry-webhook/webhook.png)

## <a name="create-webhook---azure-cli"></a>Web kancası - Azure CLI oluşturma

Azure CLI kullanarak bir Web kancası oluşturmak için kullanın [az acr Web kancası oluşturma](/cli/azure/acr/webhook#az-acr-webhook-create) komutu. Aşağıdaki komut, bir Web kancası oluşturur, tüm olayları Sil'kayıt defterindeki görüntü için *mycontainerregistry*:

```azurecli-interactive
az acr webhook create --registry mycontainerregistry --name myacrwebhook01 --actions delete --uri http://webhookuri.com
```

## <a name="test-webhook"></a>Web kancasını test et

### <a name="azure-portal"></a>Azure portal

Web kancası kullanılmadan önce kendisiyle sınayabilirsiniz **Ping** düğmesi. Ping belirtilen uç nokta için genel bir POST isteği gönderir ve yanıtını kaydeder. Ping özelliğini kullanarak, Web kancası doğru şekilde yapılandırdığınız doğrulamanıza yardımcı olabilir.

1. Test etmek istediğiniz Web kancasını seçin.
2. Üst araç çubuğunda, seçin **Ping**.
3. Uç noktanın yanıt iade **HTTP durum** sütun.

![Azure portalında ACR Web kancası oluşturma kullanıcı Arabirimi](./media/container-registry-webhook/webhook-02.png)

### <a name="azure-cli"></a>Azure CLI

Azure CLI ile bir ACR Web kancası test etmek için [az acr Web kancası ping](/cli/azure/acr/webhook#az-acr-webhook-ping) komutu.

```azurecli-interactive
az acr webhook ping --registry mycontainerregistry --name myacrwebhook01
```

Sonuçları görmek için [az acr Web kancası liste olayları](/cli/azure/acr/webhook) komutu.

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

### <a name="webhook-schema-reference"></a>Web kancası Şeması Başvurusu

Web kancası Şeması Başvurusu biçimini ve Azure Container Registry tarafından yayılan JSON olay yükü özellikleri hakkında daha fazla bilgi için bkz:

[Azure kapsayıcı kayıt defteri Web kancası Şeması Başvurusu](container-registry-webhook-reference.md)

### <a name="event-grid-events"></a>Event Grid olaylarını

Bu makalede ele alınan yerel kayıt defteri Web kancası olaylarını ek olarak, Azure Container Registry olayları Event grid'e gönderebilir:

[Hızlı Başlangıç: Kapsayıcı kayıt defteri olayları Event Grid'e göndermek](container-registry-event-grid-quickstart.md)
