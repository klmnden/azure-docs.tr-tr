---
title: "Azure kapsayıcı kayıt defterinde en iyi uygulamalar"
description: "Bu en iyi uygulamaları izleyerek, Azure kapsayıcı kayıt defteri etkili bir şekilde kullanmayı öğrenin."
services: container-registry
documentationcenter: 
author: mmacy
manager: timlt
editor: mmacy
tags: 
keywords: 
ms.assetid: 
ms.service: container-registry
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/05/2017
ms.author: marsma
ms.custom: 
ms.openlocfilehash: 9ec5573082dbf9de1288e1511f5041f8c20b416e
ms.sourcegitcommit: d41d9049625a7c9fc186ef721b8df4feeb28215f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/02/2017
---
# <a name="best-practices-for-azure-container-registry"></a>Azure kapsayıcı kayıt defteri için en iyi yöntemler

Bu en iyi uygulamaları izleyerek, performans ve düşük maliyetli, özel Docker kayıt defterinde Azure kullanımını en üst düzeye çıkarmanıza yardımcı olabilir.

## <a name="network-close-deployment"></a>Ağ Kapat dağıtım

Kapsayıcı kaydınız kapsayıcıları dağıtın aynı Azure bölgesinde oluşturun. Ağ Kapat bir bölgede kaydınız yerleştirme gecikme süresi ve maliyet düşürmek konakları, kapsayıcıya yardımcı olabilir.

Ağ Kapat dağıtım özel kapsayıcı Kayıt Defteri'ni kullanarak başlıca nedenlerinden biridir. Docker görüntülerine sahip mükemmel [katmanlama yapı](https://docs.docker.com/engine/userguide/storagedriver/imagesandcontainers/) artımlı dağıtımları için sağlar. Ancak, belirli bir görüntü için gerekli tüm katmanlara çıkarmak yeni düğümler gerekir. Bu ilk `docker pull` kadar birden çok gigabayt kolayca ekleyebilirsiniz. Özel bir kayıt defteri dağıtımınızı yakın sahip ağ gecikmesini en aza indirir.
Ayrıca, tüm genel Bulutlar, Azure dahil, ağ çıkışı ücretleri uygulayın. Bir veri merkezi görüntülerden başka bir çekme gecikme yanı sıra ağ çıkış ücretleri ekler.

## <a name="geo-replicate-multi-region-deployments"></a>Coğrafi replicate bölgeli dağıtımlar

Azure kapsayıcı kayıt defterindeki kullanmak [coğrafi çoğaltma](container-registry-geo-replication.md) kapsayıcıları için birden çok bölgeye dağıtıyorsanız özelliği. Yerel veri merkezlerinde genel müşterilerden hizmet veren veya Geliştirme ekibiniz farklı konumlarda olup olmadığını, kayıt defteri yönetimi basitleştirmek ve gecikmeyi coğrafi çoğaltma tarafından kaydınız en aza indir. Şu anda Önizleme'de, bu özellik ile kullanılabilir [Premium](container-registry-skus.md#premium) kayıt defterleri.

Coğrafi çoğaltma, üç bölümlü öğretici bkz öğrenmek için [coğrafi çoğaltma Azure kapsayıcı kayıt defterinde](container-registry-tutorial-prepare-registry.md).

## <a name="repository-namespaces"></a>Depo ad alanları

Depo ad alanları yararlanarak, kuruluşunuzdaki birden çok grupları içindeki tek bir kayıt defteri paylaşımı izin verebilirsiniz. Kayıt defterleri, dağıtımlar ve takımlar arasında paylaşılabilir. Azure kapsayıcı kayıt iç içe geçmiş ad alanları, Grup yalıtımı etkinleştirildiğinde destekler.

Örneğin, aşağıdaki kapsayıcıyı görüntü etiketleri göz önünde bulundurun. Şirket çapındaki gibi kullanılan görüntüleri `aspnetcore`, kapsayıcı görüntüleri üretim ve pazarlama grupları tarafından her kullanım kendi ad alanları ait sırada kök ad alanı yerleştirilir.

```
contoso.azurecr.io/aspnetcore:2.0
contoso.azurecr.io/products/widget/web:1
contoso.azurecr.io/products/bettermousetrap/refundapi:12.3
contoso.azurecr.io/marketing/2017-fall/concertpromotions/campaign:218.42
```

## <a name="dedicated-resource-group"></a>Ayrılmış kaynak grubu

Kapsayıcı kayıt defterleri, birden çok kapsayıcı konak genelinde kullanılan kaynaklar olduğundan, bir kayıt defteri, kendi kaynak grubu bulunmalıdır.

Azure kapsayıcı örnekleri gibi bir özel ana bilgisayar türü denemeniz ancak büyük olasılıkla işiniz bittiğinde kapsayıcı örneğini silin istersiniz. Ancak, ayrıca Azure kapsayıcı kayıt defterine gönderilen resimler koleksiyonunu tutmak isteyebilirsiniz. Kendi kaynak grubunda kaydınız koyarak kapsayıcı örnek kaynak grubu sildiğinizde, kayıt defterinde görüntüleri koleksiyonunu yanlışlıkla silme riskini en aza indirin.

## <a name="authentication"></a>Kimlik Doğrulaması

Azure kapsayıcı kayıt defteri ile kimlik doğrulamasını yaparken, iki birincil senaryo vardır: tek tek kimlik doğrulama ve hizmet (veya "gözetimsiz") kimlik doğrulaması. Aşağıdaki tabloda, bu senaryoların kısa bir genel bakış ve önerilen yöntem, her kimlik doğrulama sağlar.

| Tür | Örnek senaryo | Önerilen yöntem |
|---|---|---|
| Tek tek kimlik | Görüntüleri çekme veya kendi geliştirme makineden görüntüleri Ftp'den Geliştirici. | [az acr oturum açma](/cli/azure/acr?view=azure-cli-latest#az_acr_login) |
| Gözetimsiz/hizmet kimliği | Derleme ve dağıtım ardışık düzen burada kullanıcı ile doğrudan ilgili değildir. | [Hizmet sorumlusu](container-registry-authentication.md#service-principal) |

Azure kapsayıcı kayıt defteri kimlik doğrulaması hakkında ayrıntılı bilgi için bkz: [Azure kapsayıcı kayıt defteri ile kimlik doğrulama](container-registry-authentication.md).

## <a name="next-steps"></a>Sonraki adımlar

Azure kapsayıcı kayıt defteri SKU'ları, her farklı özellikleri sağlıyor adlı birkaç katmanda kullanılabilir. Kullanılabilir SKU'lar hakkında daha fazla bilgi için bkz: [Azure kapsayıcı kayıt defteri SKU'ları](container-registry-skus.md).