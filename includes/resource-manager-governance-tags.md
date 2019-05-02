---
title: include dosyası
description: include dosyası
services: azure-resource-manager
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: include
ms.date: 11/20/2018
ms.author: tomfitz
ms.custom: include file
ms.openlocfilehash: c426df2293cfb2d8ba4dc02e8fc5519c3d822168
ms.sourcegitcommit: e7d4881105ef17e6f10e8e11043a31262cfcf3b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2019
ms.locfileid: "64868787"
---
Azure kaynaklarınızı bir taksonomi mantıksal olarak düzenlemek için meta verileri vererek etiketler. Her etiket bir ad ve değer çifti oluşur. Örneğin, "Ortam" adını ve "Üretim" değerini üretimdeki tüm kaynaklara uygulayabilirsiniz.

Etiketleri uyguladıktan sonra aboneliğinizde bu etiket adını ve değerini taşıyan tüm kaynakları alabilirsiniz. Etiketler farklı kaynak grupları ilgili kaynakları almanızı sağlar. Bu yaklaşım, faturalama veya yönetim için kaynakları düzenlemeniz gerektiğinde yararlıdır.

Taksonominizi stratejisi kullanıcılar üzerindeki yükü azaltmak ve doğruluğu artırmak için otomatik olarak etiketleme stratejisi yanı sıra etiketleme bir Self Servis meta verileri dikkate almanız gerekir.

Etiketler için aşağıdaki sınırlamalar geçerlidir:

* Tüm kaynak türleri etiketleri destekler. Bir kaynak türü için bir etiket uygulamak, belirlemek için bkz: [etiket Azure kaynakları için destek](../articles/azure-resource-manager/tag-support.md).
* Her kaynak veya kaynak grubu en fazla 15 etiket adı/değer çifti içerebilir. Bu sınırlama yalnızca kaynak grubu veya kaynağa doğrudan uygulanan etiketler için geçerlidir. Kaynak grupları, her biri 15 etiket adı/değer çiftine sahip çok sayıda kaynak içerebilir. Bir kaynak ile ilişkilendirmeniz gereken 15'ten fazla değer varsa, etiket değeri için JSON dizesi kullanın. JSON dizesi, tek etiket adına uygulanan birden fazla değer içerebilir. Bu makalede, etikete bir JSON dizesi atama örneği gösterilmektedir.
* Etiket adı 512 karakter ile sınırlıdır ve etiket değeri 256 karakter ile sınırlıdır. Depolama hesapları için etiket adı 128 karakter ile sınırlıdır ve etiket değeri 256 karakter ile sınırlıdır.
* Sanal makineler ve sanal makine ölçek kümeleri için tüm etiket adları ve değerleri için 2048 karakter toplam sınırlıdır.
* Kaynak grubuna uygulanan etiketler, bu kaynak grubundaki kaynaklar tarafından devralınmaz.
* Bulut Hizmetleri gibi Klasik kaynakları için etiketler uygulanamaz.
* Etiket adları şu karakterleri içeremez: `<`, `>`, `%`, `&`, `\`, `?`, `/`
