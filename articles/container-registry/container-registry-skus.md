---
title: Azure Container Registry SKU'ları
description: Azure Container Registry'de farklı hizmet katmanlarını karşılaştırın.
services: container-registry
author: dlepow
ms.service: container-registry
ms.topic: article
ms.date: 08/30/2018
ms.author: danlep
ms.openlocfilehash: 80ee1033ef7a5fee849126ab2b614de18b71e051
ms.sourcegitcommit: 67abaa44871ab98770b22b29d899ff2f396bdae3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/08/2018
ms.locfileid: "48855859"
---
# <a name="azure-container-registry-skus"></a>Azure Container Registry SKU'ları

Azure Container Registry (ACR), SKU'ları bilinen birden çok hizmet katmanlarında kullanılabilir. Bu SKU'ları, tahmin edilebilir fiyatlandırma ve azure'da özel Docker kayıt defterinizin kapasite ve kullanım desenlerini çizerek için çeşitli seçenekler sağlar.

| SKU | Yönetilen | Açıklama |
| --- | :-------: | ----------- |
| **Temel** | Evet | Azure Container Registry hakkında bilgi edinen geliştiriciler için düşük maliyetli bir giriş noktası. Temel kayıt defterleri, standart ve Premium (Azure Active Directory kimlik doğrulaması tümleştirmesi, görüntü silme ve web kancaları) aynı programlama özellikleri vardır. Ancak, görüntü işleme ve dahil edilen depolama en düşük kullanım senaryoları için uygun değildir. |
| **Standart** | Evet | Standart kayıt defterleri temel, daha fazla dahil edilen depolama ve görüntü işleme ile aynı özellikleri sunar. Standart kayıt defterleri, çoğu üretim senaryosu gereksinimlerini karşılayabilir. |
| **Premium** | Evet | Premium kayıt defterleri dahil edilen depolama ve yüksek hacimli senaryolar etkinleştirme eşzamanlı işlemlerin en yüksek tutarı sağlar. Ek olarak daha yüksek görüntü işleme, Premium gibi özellikler ekler [coğrafi çoğaltma] [ container-registry-geo-replication] tek bir kayıt defteri, birden çok bölgede yönetmek için ve [içerik güven (Önizleme)](container-registry-content-trust.md)görüntü etiketi imzalamak için. |
| Klasik<sup>1</sup> | Hayır | Bu SKU, Azure Azure Container Registry hizmetinde'nin ilk sürümünden etkin. Klasik kayıt defterleri, artan iş hacmi ve coğrafi çoğaltma gibi daha üst düzey özellikler sağlamak ACR becerisini sınırlandırır, aboneliğinizdeki Azure oluşturan bir depolama hesabı tarafından desteklenir. |

<sup>1</sup> Klasik SKU olacaktır **kullanım dışı** içinde **Mart 2019**. Temel, standart veya Premium tüm yeni kapsayıcı kayıt defterleri için kullanın.

Üst düzey bir SKU daha fazla performans ve ölçek sağlar seçme, ancak tüm yönetilen SKU'lar aynı programlama özelliklerini sağlar. Birden çok hizmet katmanı ile temel ile çalışmaya başlama sonra standart ve Premium, kayıt defteri kullanım arttıkça dönüştürün.

## <a name="managed-vs-unmanaged"></a>Yönetilmeyen ve yönetilen

Temel, standart ve Premium SKU'larının toplu olarak da bilinir *yönetilen* kayıt defterleri ve klasik kayıt defterleri olarak *yönetilmeyen*. İkisi arasındaki birincil fark, kapsayıcı görüntülerinizi nasıl saklandığı yerdir.

### <a name="managed-basic-standard-premium"></a>Yönetilen (temel, standart, Premium)

Resim depolama tamamen Azure tarafından yönetilen kayıt defterleri avantajı yönetilen. Diğer bir deyişle, görüntüleri depolayan bir depolama hesabı, Azure aboneliğinizde görünmüyor. Çeşitli avantajları vardır yönetilen kayıt defteri SKU'ları kullanılarak elde edilen, ayrıntılı olarak ele alınan [kapsayıcı görüntüsü Azure Container Registry depolamada][container-registry-storage]. Bu makalede, yönetilen kayıt defteri SKU'ları ve yeteneklerini odaklanır.

### <a name="unmanaged-classic"></a>Yönetilmeyen (Klasik)

> [!IMPORTANT]
> Klasik SKU kullanımdan kaldırılıyor ve Mart 2019 sonra kullanılamaz. Temel, standart veya Premium tüm yeni kayıt defterleri için kullanın.

Klasik kayıt defterleri "yönetilmeyen" Klasik kayıt defterini yedekleyen depolama hesabı içinde bulunduğu anlamında *,* Azure aboneliği. Bu nedenle, kapsayıcı görüntülerinizin depolandığı depolama hesabının yönetim için sorumlu olursunuz. Yönetilmeyen kayıt defterleri ile gereksinimleriniz değiştikçe SKU'ları arasında geçiş yapamazsınız (dışında [yükseltme] [ container-registry-upgrade] yönetilen kayıt defteri için), ve yönetilen kayıt defterleri'nin birtakım özelliklerini (örneğin, kullanılamıyor kapsayıcı görüntüsünü silme [coğrafi çoğaltma][container-registry-geo-replication], ve [Web kancaları][container-registry-webhook]).

Klasik kayıt defterini yönetilen SKU'lar birine yükseltme hakkında daha fazla bilgi için bkz. [Klasik kayıt defterini yükseltme][container-registry-upgrade].

## <a name="sku-feature-matrix"></a>SKU özellik Matrisi

Aşağıdaki tabloda temel, standart ve Premium hizmet katmanları sınırlamaları ve özellikleri ayrıntılı olarak açıklanmaktadır.

[!INCLUDE [container-instances-limits](../../includes/container-registry-limits.md)]

## <a name="changing-skus"></a>SKU'ları değiştirme

Azure CLI veya Azure portalında bir kayıt defteri SKU değiştirebilirsiniz. Gerekli en fazla depolama kapasitesi için geçiş yapıyorsanız SKU sahip olduğu sürece yönetilen SKU'lar arasında serbestçe taşınabildiğini. Klasikten yönetilen SKU'lar birine geçiş yapıyorsanız, taşıyamazsınız geri Klasik'ten--tek yönlü bir dönüştürmedir.

### <a name="azure-cli"></a>Azure CLI

Azure CLI SKU'ları arasında taşımak için kullanın [az acr update] [ az-acr-update] komutu. Örneğin, Premium'a geçiş yapmak için şunu yazın:

```azurecli
az acr update --name myregistry --sku Premium
```

### <a name="azure-portal"></a>Azure portal

Kapsayıcı kayıt defterinde **genel bakış** Azure portalında **güncelleştirme**, yeni bir seçip **SKU** SKU açılır listeden.

![Azure portalındaki kapsayıcı kayıt defteri SKU güncelleştirme][update-registry-sku]

Klasik kayıt defterini varsa, Azure portalındaki yönetilen SKU seçemezsiniz. Bunun yerine, önce [yükseltme] [ container-registry-upgrade] yönetilen kayıt defteri için (bkz [Klasikten değiştirme](#changing-from-classic)).

## <a name="changing-from-classic"></a>Klasik modelden değiştirme

Yönetilmeyen bir Klasik kayıt defterini yönetilen temel, standart veya Premium SKU'ları, birine geçiş yaparken dikkate gereken ek noktalar vardır. Klasik kayıt defterinizin çok sayıda görüntü içerir ve çok gigabayta, geçiş işlemi biraz zaman alabilir. Ayrıca, `docker push` geçişi tamamlanana kadar işlemleri devre dışıdır.

Klasik kayıt defterinizin yönetilen SKU'lar birine yükseltme hakkında daha fazla bilgi için bkz [Klasik kapsayıcı kayıt defterini yükseltme][container-registry-upgrade].

## <a name="pricing"></a>Fiyatlandırma

Fiyatlandırma bilgileri her bir Azure Container Registry SKU'ları için bkz: [kapsayıcı kayıt defteri fiyatlandırması][container-registry-pricing].

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
[container-registry-webhook]: container-registry-webhook.md
