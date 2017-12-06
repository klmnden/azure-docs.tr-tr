---
title: "Azure kapsayıcı kayıt defteri SKU'ları"
description: "Azure kapsayıcı kayıt defterinde farklı hizmet katmanları arasındaki karşılaştırmaları"
services: container-registry
author: stevelas
manager: timlt
ms.service: container-registry
ms.topic: article
ms.date: 10/16/2017
ms.author: stevelas
ms.openlocfilehash: cf7724dd9e3e870cfd7ec0b5d2e4ea1d0694e9de
ms.sourcegitcommit: a48e503fce6d51c7915dd23b4de14a91dd0337d8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/05/2017
---
# <a name="azure-container-registry-skus"></a>Azure kapsayıcı kayıt defteri SKU'ları

Azure kapsayıcı kayıt defteri (ACR) SKU'ları bilinen birden çok hizmet katmanları mevcuttur. Bu SKU'ları, tahmin edilebilir fiyatlandırma ve özel Docker kayıt defterinizde Azure kullanmak istediğiniz nasıl için çeşitli seçenekler sağlar. Üst düzey bir SKU seçerek daha fazla ölçek ve performans sağlar. Ancak, tüm SKU'ları, Basic ile başlayın ve standart ve Premium için kayıt defteri kullanım arttıkça dönüştürmek bir geliştirici etkinleştirme aynı programlı özellikleri sağlar.

## <a name="basic"></a>Temel
Azure kapsayıcı kayıt defteri hakkında öğrenme geliştiriciler için maliyet iyileştirilmiş giriş noktası. Temel kayıt defterleri standart ve Premium (Azure Active Directory kimlik doğrulama tümleştirme, görüntü silme ve web kancaları) aynı programlama özelliklerine sahip, ancak, boyutu ve kullanım kısıtlamaları vardır.

## <a name="standard"></a>Standart
Standart kayıt defterleri daha yüksek depolama sınırları ve görüntü işleme ile temel olarak aynı yetenekleri sunar. Standart kayıt defterleri, çoğu üretim senaryosu gereksinimlerini karşılaması gerekir.

## <a name="premium"></a>Premium
Premium kayıt defterleri, depolama ve eşzamanlı işlem, yüksek hacimli senaryolar etkinleştirme gibi kısıtlamaları hakkında daha yüksek sınırları sağlar. Ek olarak, daha yüksek görüntü işleme kapasitesi, Premium gibi özellikler ekler [coğrafi çoğaltma](container-registry-geo-replication.md) tek bir kayıt defteri birden çok bölgeler arasında yönetmek için her bir dağıtım için bir ağ Kapat kayıt defteri koruma.

## <a name="classic"></a>Klasik
Klasik kayıt defteri SKU ilk sürüm Azure Azure kapsayıcı kayıt defteri hizmetinin etkinleştirilmiş. Klasik kayıt defterleri, artan iş hacmi ve coğrafi çoğaltma gibi daha üst düzey özellikleri sağlamak üzere ACR becerisini sınırlandırır aboneliğinizde Azure oluşturduğu bir depolama hesabı tarafından desteklenir. Sınırlı yeteneklerini nedeniyle biz Klasik SKU gelecekte alanı onaylanamadı planlayın.

> [!NOTE]
> Planlanan Klasik kayıt defteri SKU kullanımdan nedeniyle, tüm yeni kayıt defterleri için temel, standart veya Premium kullanmanızı öneririz. Varolan Klasik kaydınız dönüştürme hakkında daha fazla bilgi için bkz: [değiştirme SKU'ları](#changing-skus).
>

## <a name="registry-sku-feature-matrix"></a>Kayıt defteri SKU özelliği Matrisi

Aşağıdaki tabloda temel, standart ve Premium hizmet katmanları sınırları ve özellikleri ayrıntılı olarak açıklanmaktadır.

[!INCLUDE [container-instances-limits](../../includes/container-registry-limits.md)]

## <a name="manage-registry-size"></a>Kayıt defteri boyutunu Yönet
Her SKU depolama kısıtlamaları tipik bir senaryo ile hizalamak için tasarlanmıştır: Başlarken için temel, üretim uygulamaları ve Premium çoğunluğu hiper ölçekli performans için standart ve [coğrafi çoğaltma](container-registry-geo-replication.md). Kayıt defteri kullanım ömrü düzenli aralıklarla kullanılmayan içeriği silerek boyutuna yönetmeniz gerekir.

Kapsayıcı kayıt defterinde geçerli bir kayıt defteri kullanımını bulabilirsiniz **genel bakış** Azure portalında:

![Kayıt defteri kullanım bilgileri Azure portalında](media/container-registry-skus/registry-overview-quotas.png)

Azure portalında depoları silerek, kayıt defteri boyutunu yönetebilirsiniz.

Altında **Hizmetleri**seçin **depoları**, silin ve ardından istediğiniz depo sağ **silmek**.

![Azure portalında deposunu Sil](media/container-registry-skus/delete-repository-portal.png)

## <a name="changing-skus"></a>SKU'ları değiştirme

Azure portalında bir kayıt defterindeki SKU değiştirebilirsiniz.

Kayıt defterinde **genel bakış** Azure portalında seçin **güncelleştirme**, yeni bir seçin **SKU** gelen SKU açılır.

![Azure portalında güncelleştirme kapsayıcı kayıt defteri SKU](media/container-registry-skus/update-registry-sku.png)

## <a name="changing-from-classic"></a>Klasikten değiştirme
Klasik kayıt defteri için temel, standart veya Premium, Azure kopyaları aboneliğinizi ilişkili depolama hesabında kapsayıcı görüntülerden Azure tarafından yönetilen bir depolama hesabı varolan değiştirdiğinizde. Bu işlem biraz zaman alabilir.

Dönüştürme sırasında `docker pull` çalışmaya, ancak devam `docker push` dönüştürme işlemi tamamlanana kadar engellendi.

Tamamlandığında, abonelik depolama hesabı artık ACR tarafından kullanılır.

### <a name="why-change-from-classic-to-basic-standard-or-premium"></a>Neden Klasikten temel, standart veya Premium değiştirilsin mi?

Klasik kayıt defterleri sınırlı özellikleri nedeniyle, temel, standart veya Premium katmanları için Klasik kayıt defterleri değiştirmenizi öneririz. Bu üst düzey SKU'ları kayıt defterinde Azure özelliklerini daha kapsamlı tümleştirin. Bu özelliklerden bazıları şunlardır:

* Tek tek kimlik doğrulaması için Azure Active Directory Tümleştirme (bkz [az acr oturum açma](/cli/azure/acr?view=azure-cli-latest#az_acr_login))
* Görüntü ve etiket silme desteği
* [Coğrafi çoğaltma](container-registry-geo-replication.md)
* [Web kancaları](container-registry-webhook.md)

En önemlisi, Klasik kayıt defteri kayıt oluşturduğunuzda otomatik olarak Azure aboneliğinizde sağlanan bu Azure depolama hesabında bağlıdır. Bunun aksine, temel, standart ve Premium SKU'ları yararlanmak *yönetilen depolama*. Diğer bir deyişle, Azure depolama saydam yönetir,--görüntülerinizin ayrı bir depolama hesabı kendi abonelikte oluşturulmaz.

Bazı temel, standart ve Premium kayıt defterleri tarafından sağlanan yönetilen depolama yararları:

* Kapsayıcı görüntüleri [şifrelenen](../storage/common/storage-service-encryption.md).
* Görüntüleri kullanılarak depolanmış olan [coğrafi olarak yedekli depolama](../storage/common/storage-redundancy.md#geo-redundant-storage), modemlerin yedekleme bölgeli çoğaltma ile görüntüler.
* Yeteneği [SKU'ları arasında taşıyın](#changing-skus), üst düzey bir SKU seçtiğinizde daha yüksek verimlilik etkinleştirme. Gereksinimlerinize arttıkça her SKU ile üretilen iş gereksinimlerinizi ACR karşılayabilir. İstenen işleme ACR nasıl ulaşır, temel uygulaması olarak ifade edilen *hedefi* olmadan uygulama ayrıntılarını yönetmek zorunda (seçme daha yüksek SKU tarafından),.

## <a name="pricing"></a>Fiyatlandırma

Fiyatlandırma bilgileri her Azure kapsayıcı kayıt defteri SKU'ları için bkz: [kapsayıcı kayıt defteri fiyatlandırma](https://azure.microsoft.com/pricing/details/container-registry/).

## <a name="next-steps"></a>Sonraki adımlar

**Azure kapsayıcı kayıt defteri yol haritası**

Ziyaret [ACR yol haritası](https://aka.ms/acr/roadmap) hizmetinde yaklaşan özellikleri hakkında bilgi bulmak için github'da.

**Azure kapsayıcı kayıt defteri UserVoice**

Gönderir ve yeni özellik önerileri içinde oylamak [ACR UserVoice](https://feedback.azure.com/forums/903958-azure-container-registry).