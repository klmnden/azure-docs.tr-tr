---
title: Azure kapsayıcı kayıt defteri SKU'ları
description: Azure kapsayıcı kayıt defterinde farklı hizmet katmanlarındaki karşılaştırın.
services: container-registry
author: mmacy
manager: timlt
ms.service: container-registry
ms.topic: article
ms.date: 03/15/2018
ms.author: marsma
ms.openlocfilehash: c9b8e072b5ccd89c27d9c46407e472d6bf1e1e84
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/23/2018
---
# <a name="azure-container-registry-skus"></a>Azure kapsayıcı kayıt defteri SKU'ları

Azure kapsayıcı kayıt defteri (ACR) SKU'ları bilinen birden çok hizmet katmanları mevcuttur. Bu SKU'ları, tahmin edilebilir fiyatlandırma ve özel Docker kayıt defterinizde Azure kapasite ve kullanım desenlerini hizalama için çeşitli seçenekler sağlar.

| SKU | Yönetilen | Açıklama |
| --- | :-------: | ----------- |
| **Temel** | Evet | Azure kapsayıcı kayıt defteri hakkında öğrenme geliştiriciler için maliyet iyileştirilmiş giriş noktası. Temel kayıt defterleri standart ve Premium (Azure Active Directory kimlik doğrulama tümleştirme, görüntü silme ve web kancaları) aynı programlama özelliklerine sahip, ancak, boyutu ve kullanım kısıtlamaları vardır. |
| **Standart** | Evet | Standart kayıt defterleri daha yüksek depolama sınırları ve görüntü işleme ile temel olarak aynı yetenekleri sunar. Standart kayıt defterleri, çoğu üretim senaryosu gereksinimlerini karşılaması gerekir. |
| **Premium** | Evet | Premium kayıt defterleri, depolama ve eşzamanlı işlem, yüksek hacimli senaryolar etkinleştirme gibi kısıtlamaları hakkında daha yüksek sınırları sağlar. Ek olarak, daha yüksek görüntü işleme kapasitesi, Premium gibi özellikler ekler [coğrafi çoğaltma] [ container-registry-geo-replication] tek bir kayıt defteri birden çok bölgeler arasında yönetmek için her bir ağ Kapat kayıt defteri koruma dağıtımı. |
| Klasik | Hayır | Klasik kayıt defteri SKU ilk sürüm Azure Azure kapsayıcı kayıt defteri hizmetinin etkinleştirilmiş. Klasik kayıt defterleri, artan iş hacmi ve coğrafi çoğaltma gibi daha üst düzey özellikleri sağlamak üzere ACR becerisini sınırlandırır aboneliğinizde Azure oluşturduğu bir depolama hesabı tarafından desteklenir. Sınırlı yeteneklerini nedeniyle biz Klasik SKU gelecekte alanı onaylanamadı planlayın. |

Daha fazla performans ve ölçek üst düzey bir SKU sağlar seçme, Bununla birlikte, tüm yönetilen SKU'ları aynı programlı yetenekleri sağlar. Birden çok hizmet katmanı ile Basic ile çalışmaya başlama sonra standart ve Premium kayıt defteri kullanım arttıkça dönüştürün.

> [!NOTE]
> Planlanan Klasik kayıt defteri SKU kullanımdan nedeniyle, tüm yeni kayıt defterleri için temel, standart veya Premium kullanmanızı öneririz. Varolan Klasik kaydınız dönüştürme hakkında daha fazla bilgi için bkz: [Klasik kayıt defteri yükseltme][container-registry-upgrade].
>

## <a name="managed-vs-unmanaged"></a>Yönetilmeyen-yönetilen

Temel, standart ve Premium SKU'ları topluca olarak bilinen *yönetilen* registries ve klasik kayıt defterleri olarak *yönetilmeyen*. İkisi arasındaki birincil fark nasıl kapsayıcı görüntülerinizi depolanır.

### <a name="managed-basic-standard-premium"></a>Yönetilen (temel, standart, Premium)

Görüntü depolama tamamen Azure tarafından yönetilen kayıt defterleri avantajı yönetilen. Diğer bir deyişle, görüntüleri depolayan bir depolama hesabı Azure aboneliğinizi içinde görünmez. Çeşitli avantajları vardır yönetilen kayıt defteri SKU'ları birini kullanarak elde edilen, ayrıntılı olarak ele alınan [kapsayıcı görüntü depolama Azure kapsayıcı kayıt defterinde][container-registry-storage]. Bu makalede, yönetilen kayıt defteri SKU'ları ve yeteneklerini odaklanır.

### <a name="unmanaged-classic"></a>Yönetilmeyen (Klasik)

Klasik kayıt defterleri "yönetilmeyen" içinde bir Klasik kayıt defteri yedekler depolama hesabının bulunduğu herkese açık *,* Azure aboneliği. Bu nedenle, kapsayıcı görüntülerinizi depolandığı depolama hesabını yönetimi için sorumlu. Yönetilmeyen kayıt defterleri ile gereksinimleriniz değiştikçe SKU'ları arasında geçiş yapamazsınız (dışında [yükseltme] [ container-registry-upgrade] yönetilen bir kayıt defteri), ve yönetilen kayıt defterleri birkaç özelliklerinin (örneğin, kullanılamaz kapsayıcı görüntü silme işlemi, [coğrafi çoğaltma][container-registry-geo-replication], ve [kancalarını][container-registry-webhook]).

Klasik kayıt defteri yönetilen SKU'ları birine yükseltme hakkında daha fazla bilgi için bkz: [Klasik kayıt defteri yükseltme][container-registry-upgrade].

## <a name="sku-feature-matrix"></a>SKU özelliği Matrisi

Aşağıdaki tabloda temel, standart ve Premium hizmet katmanları sınırları ve özellikleri ayrıntılı olarak açıklanmaktadır.

[!INCLUDE [container-instances-limits](../../includes/container-registry-limits.md)]

## <a name="changing-skus"></a>SKU'ları değiştirme

Bir kayıt defterindeki SKU Azure portalında veya Azure CLI ile değiştirebilirsiniz. Geçiş yapıyorsanız SKU gerekli maksimum depolama kapasitesi olduğu sürece, yönetilen SKU'ları arasında serbestçe taşıyabilirsiniz. Yönetilen SKU'ları birine Klasikten geçiş yapıyorsanız, taşıyamazsınız geri Klasik olarak--tek yönlü bir dönüştürme değildir.

### <a name="azure-cli"></a>Azure CLI

Azure CLI SKU arasında dolaşmak için kullanma [az acr güncelleştirme] [ az-acr-update] komutu. Örneğin, Premium geçiş yapmak için şunu yazın:

```azurecli
az acr update --name myregistry --sku Premium
```

### <a name="azure-portal"></a>Azure portalına

Kapsayıcı kayıt defterinde **genel bakış** Azure portalında seçin **güncelleştirme**, yeni bir seçin **SKU** gelen SKU açılır.

![Azure portalında güncelleştirme kapsayıcı kayıt defteri SKU][update-registry-sku]

Klasik kayıt varsa, Azure portalı içinde yönetilen bir SKU seçemezsiniz. Bunun yerine, öncelikle [yükseltme] [ container-registry-upgrade] yönetilen bir kayıt defteri (bkz [Klasikten değiştirme](#changing-from-classic)).

## <a name="changing-from-classic"></a>Klasikten değiştirme

Yönetilmeyen bir Klasik kayıt defteri yönetilen temel, standart veya Premium SKU'ları, birine geçirirken dikkate almanız gereken ek noktalar vardır. Klasik kaydınız çok sayıda görüntü içerir ve çok gigabayta ise, geçiş işlemi biraz zaman alabilir. Ayrıca, `docker push` geçişi tamamlanana kadar operations devre dışı.

Klasik kaydınız yönetilen SKU'ları birine yükseltme hakkında daha fazla bilgi için bkz [Klasik kapsayıcı kayıt defteri yükseltme][container-registry-upgrade].

## <a name="pricing"></a>Fiyatlandırma

Fiyatlandırma bilgileri her Azure kapsayıcı kayıt defteri SKU'ları için bkz: [kapsayıcı kayıt defteri fiyatlandırma][container-registry-pricing].

## <a name="next-steps"></a>Sonraki adımlar

**Azure kapsayıcı kayıt defteri yol haritası**

Ziyaret [ACR yol haritası] [ acr-roadmap] hizmetinde yaklaşan özellikleri hakkında bilgi bulmak için github'da.

**Azure kapsayıcı kayıt defteri UserVoice**

Gönderir ve yeni özellik önerileri içinde oylamak [ACR UserVoice][container-registry-uservoice].

<!-- IMAGES -->
[update-registry-sku]: ./media/container-registry-skus/update-registry-sku.png

<!-- LINKS - External -->
[acr-roadmap]: https://aka.ms/acr/roadmap
[container-registry-pricing]: https://azure.microsoft.com/pricing/details/container-registry/
[container-registry-uservoice]: https://feedback.azure.com/forums/903958-azure-container-registry

<!-- LINKS - Internal -->
[az-acr-update]: /cli/azure/acr#az_acr_update
[container-registry-geo-replication]: container-registry-geo-replication.md
[container-registry-upgrade]: container-registry-upgrade.md
[container-registry-storage]: container-registry-storage.md
[container-registry-webhook]: container-registry-webhook.md
