---
title: include dosyası
description: include dosyası
services: azure-resource-manager
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: include
ms.date: 06/18/2019
ms.author: tomfitz
ms.custom: include file
ms.openlocfilehash: 5358fe387d2a371d96d46d8546ce0f20b47ca54b
ms.sourcegitcommit: b7a44709a0f82974578126f25abee27399f0887f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2019
ms.locfileid: "67206477"
---
Azure kaynaklarınızı bir taksonomi mantıksal olarak düzenlemek için meta verileri vererek etiketler. Her etiket bir ad ve değer çifti oluşur. Örneğin, "Ortam" adını ve "Üretim" değerini üretimdeki tüm kaynaklara uygulayabilirsiniz.

Etiketleri uyguladıktan sonra aboneliğinizde bu etiket adını ve değerini taşıyan tüm kaynakları alabilirsiniz. Etiketler farklı kaynak grupları ilgili kaynakları almanızı sağlar. Bu yaklaşım, faturalama veya yönetim için kaynakları düzenlemeniz gerektiğinde yararlıdır.

Taksonominizi stratejisi kullanıcılar üzerindeki yükü azaltmak ve doğruluğu artırmak için otomatik olarak etiketleme stratejisi yanı sıra etiketleme bir Self Servis meta verileri dikkate almanız gerekir.

Etiketler için aşağıdaki sınırlamalar geçerlidir:

* Tüm kaynak türleri etiketleri destekler. Bir kaynak türü için bir etiket uygulamak, belirlemek için bkz: [etiket Azure kaynakları için destek](../articles/azure-resource-manager/tag-support.md).
* Her kaynak veya kaynak grubu en fazla 15 etiket adı/değer çifti içerebilir. Bu sınırlama yalnızca kaynak grubu veya kaynağa doğrudan uygulanan etiketler için geçerlidir. Kaynak grupları, her biri 15 etiket adı/değer çiftine sahip çok sayıda kaynak içerebilir. Bir kaynak ile ilişkilendirmeniz gereken 15'ten fazla değer varsa, etiket değeri için JSON dizesi kullanın. JSON dizesi, tek etiket adına uygulanan birden fazla değer içerebilir. Bu makalede, etikete bir JSON dizesi atama örneği gösterilmektedir.
* Etiket adı 512 karakter ile sınırlıdır ve etiket değeri 256 karakter ile sınırlıdır. Depolama hesapları için etiket adı 128 karakter ile sınırlıdır ve etiket değeri 256 karakter ile sınırlıdır.
* Sanal makineler ve sanal makine ölçek kümeleri için tüm etiket adları ve değerleri için 2048 karakter toplam sınırlıdır. Etiketleri genelleştirilmiş sanal makineleri desteklemez.
* Kaynak grubuna uygulanan etiketler, bu kaynak grubundaki kaynaklar tarafından devralınmaz.
* Bulut Hizmetleri gibi Klasik kaynakları için etiketler uygulanamaz.
* Etiket adları şu karakterleri içeremez: `<`, `>`, `%`, `&`, `\`, `?`, `/`
