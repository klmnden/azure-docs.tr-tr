---
title: Azure Container Registry SKU'ları
description: Azure Container Registry'de farklı hizmet katmanlarını karşılaştırın.
services: container-registry
author: dlepow
ms.service: container-registry
ms.topic: article
ms.date: 05/06/2019
ms.author: danlep
ms.openlocfilehash: f36b206ff015511dea7369617febe9220282bbe5
ms.sourcegitcommit: 0ae3139c7e2f9d27e8200ae02e6eed6f52aca476
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65069052"
---
# <a name="azure-container-registry-skus"></a>Azure Container Registry SKU'ları

Azure Container Registry (ACR), SKU'ları bilinen birden çok hizmet katmanlarında kullanılabilir. Bu SKU'ları, tahmin edilebilir fiyatlandırma ve azure'da özel Docker kayıt defterinizin kapasite ve kullanım desenlerini çizerek için çeşitli seçenekler sağlar.

| SKU | Yönetilen | Açıklama |
| --- | :-------: | ----------- |
| **Temel** | Evet | Azure Container Registry hakkında bilgi edinen geliştiriciler için düşük maliyetli bir giriş noktası. Temel kayıt defterleri, standart ve Premium aynı programlama özellikleri sahip (Azure Active Directory gibi [kimlik doğrulaması tümleştirmesinde](container-registry-authentication.md#individual-login-with-azure-ad), [görüntü silme][container-registry-delete], ve [Web kancaları][container-registry-webhook]). Ancak, görüntü işleme ve dahil edilen depolama en düşük kullanım senaryoları için uygun değildir. |
| **Standart** | Evet | Standart kayıt defterleri temel, daha fazla dahil edilen depolama ve görüntü işleme ile aynı özellikleri sunar. Standart kayıt defterleri, çoğu üretim senaryosu gereksinimlerini karşılayabilir. |
| **Premium** | Evet | Premium kayıt defterleri dahil edilen depolama ve yüksek hacimli senaryolar etkinleştirme eşzamanlı işlemlerin en yüksek tutarı sağlar. Daha yüksek görüntü işleme ek Premium özellikler dahil olmak üzere ekler [coğrafi çoğaltma] [ container-registry-geo-replication] tek bir kayıt defteri, birden çok bölgede yönetmek için [içerik güven](container-registry-content-trust.md) görüntü etiketi imzalama için ve [güvenlik duvarları ve sanal ağlar (Önizleme)](container-registry-vnet.md) kayıt defterine erişimi kısıtlamak için. |
|  Klasik (*Nisan 2019 sonra kullanılamaz*) | Hayır | Bu SKU, Azure Azure Container Registry hizmetinde'nin ilk sürümünden etkin. Klasik kayıt defterleri, artan iş hacmi ve coğrafi çoğaltma gibi daha üst düzey özellikler sağlamak ACR becerisini sınırlandırır, aboneliğinizdeki Azure oluşturan bir depolama hesabı tarafından desteklenir. |

> [!IMPORTANT]
> Klasik kayıt defteri SKU okunuyor **kullanım dışı**ve sonra kullanılamaz **Nisan 2019**. Tüm yeni kayıt defterleri için temel, standart veya Premium kullanmanızı öneririz. Tüm mevcut Klasik kayıt defterleri Nisan 2019'öncesinde yükseltilmelidir. Yükseltme için bilgi [Klasik kayıt defterini yükseltme][container-registry-upgrade].

Temel, standart ve Premium SKU'ları (topluca çağrılan *yönetilen kayıt defterleri*) tüm aynı programlama özelliklerini sağlar. Bunlar ayrıca tüm yararlı [resim depolama] [ container-registry-storage] tamamen Azure tarafından yönetilir. Üst düzey bir SKU seçilmesi, daha fazla performans ve ölçek sağlar. Birden çok hizmet katmanı ile temel ile çalışmaya başlama sonra standart ve Premium, kayıt defteri kullanım arttıkça dönüştürün.

## <a name="sku-feature-matrix"></a>SKU özellik Matrisi

Aşağıdaki tabloda temel, standart ve Premium hizmet katmanları sınırlamaları ve özellikleri ayrıntılı olarak açıklanmaktadır.

[!INCLUDE [container-instances-limits](../../includes/container-registry-limits.md)]

## <a name="changing-skus"></a>SKU'ları değiştirme

Azure CLI veya Azure portalında bir kayıt defteri SKU değiştirebilirsiniz. Gerekli en fazla depolama kapasitesi için geçiş yapıyorsanız SKU sahip olduğu sürece yönetilen SKU'lar arasında serbestçe taşınabildiğini. Klasikten yönetilen SKU'lar birine geçiş yaptığınızda, taşıyamazsınız geri Klasik'ten--tek yönlü bir dönüştürmedir.

### <a name="azure-cli"></a>Azure CLI

Azure CLI SKU'ları arasında taşımak için kullanın [az acr update] [ az-acr-update] komutu. Örneğin, Premium'a geçiş yapmak için şunu yazın:

```azurecli
az acr update --name myregistry --sku Premium
```

### <a name="azure-portal"></a>Azure portal

Kapsayıcı kayıt defterinde **genel bakış** Azure portalında **güncelleştirme**, yeni bir seçip **SKU** SKU açılır listeden.

![Azure portalındaki kapsayıcı kayıt defteri SKU güncelleştirme][update-registry-sku]

Klasik kayıt defterini varsa, Azure portalındaki yönetilen SKU seçemezsiniz. Bunun yerine, önce [yükseltme] [ container-registry-upgrade] yönetilen kayıt defteri için.

## <a name="pricing"></a>Fiyatlandırma

Fiyatlandırma bilgileri her bir Azure Container Registry SKU'ları için bkz: [kapsayıcı kayıt defteri fiyatlandırması][container-registry-pricing].

Veri aktarımları için fiyatlandırma hakkında daha fazla ayrıntı için bkz [bant genişliği fiyatlandırma ayrıntıları](https://azure.microsoft.com/pricing/details/bandwidth/). 

## <a name="next-steps"></a>Sonraki adımlar

**Azure kapsayıcı kayıt defteri yol haritası**

Ziyaret [ACR yol haritası] [ acr-roadmap] hizmette yakında çıkacak özellikler hakkında bilgi için GitHub üzerindeki.

**Azure kapsayıcı kayıt defteri UserVoice**

Gönder ve yeni özellik öneri oylamak [ACR UserVoice][container-registry-uservoice].

<!-- IMAGES -->
[update-registry-sku]: ./media/container-registry-skus/update-registry-sku.png

<!-- LINKS - External -->
[acr-roadmap]: https://aka.ms/acr/roadmap
[container-registry-pricing]: https://azure.microsoft.com/pricing/details/container-registry/
[container-registry-uservoice]: https://feedback.azure.com/forums/903958-azure-container-registry

<!-- LINKS - Internal -->
[az-acr-update]: /cli/azure/acr#az-acr-update
[container-registry-geo-replication]: container-registry-geo-replication.md
[container-registry-upgrade]: container-registry-upgrade.md
[container-registry-storage]: container-registry-storage.md
[container-registry-delete]: container-registry-delete.md
[container-registry-webhook]: container-registry-webhook.md
