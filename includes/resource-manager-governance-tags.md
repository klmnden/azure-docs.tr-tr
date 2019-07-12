---
title: include dosyası
description: include dosyası
services: azure-resource-manager
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: include
ms.date: 07/11/2019
ms.author: tomfitz
ms.custom: include file
ms.openlocfilehash: 099bca7483100da1a4ee2f8f10057c416ad145b0
ms.sourcegitcommit: 64798b4f722623ea2bb53b374fb95e8d2b679318
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67841472"
---
Azure kaynaklarınızı bir taksonomi mantıksal olarak düzenlemek için meta verileri vererek etiketler. Her etiket bir ad ve değer çifti oluşur. Örneğin, "Ortam" adını ve "Üretim" değerini üretimdeki tüm kaynaklara uygulayabilirsiniz.

Etiketleri uyguladıktan sonra aboneliğinizde bu etiket adını ve değerini taşıyan tüm kaynakları alabilirsiniz. Etiketler farklı kaynak grupları ilgili kaynakları almanızı sağlar. Bu yaklaşım, faturalama veya yönetim için kaynakları düzenlemeniz gerektiğinde yararlıdır.

Taksonominizi stratejisi kullanıcılar üzerindeki yükü azaltmak ve doğruluğu artırmak için otomatik olarak etiketleme stratejisi yanı sıra etiketleme bir Self Servis meta verileri dikkate almanız gerekir.

Etiketler için aşağıdaki sınırlamalar geçerlidir:

* Tüm kaynak türleri etiketleri destekler. Bir kaynak türü için bir etiket uygulamak, belirlemek için bkz: [etiket Azure kaynakları için destek](../articles/azure-resource-manager/tag-support.md).
* Her kaynak veya kaynak grubu en fazla 50 etiket adı/değer çiftleri olabilir. Şu anda 15 etiket yalnızca destek depolama hesapları, ancak bu sınırı 50 gelecek sürümlerden birinde gerçekleştirilecektir. İzin verilen en yüksek sayıdan daha fazla etiket uygulamak ihtiyacınız varsa etiket değeri için bir JSON dizesi kullanın. JSON dizesi, tek etiket adına uygulanan birden fazla değer içerebilir. Bir kaynak grubu, her 50 etiket adı/değer çiftine sahip çok sayıda kaynak içerebilir.
* Etiket adı 512 karakter ile sınırlıdır ve etiket değeri 256 karakter ile sınırlıdır. Depolama hesapları için etiket adı 128 karakter ile sınırlıdır ve etiket değeri 256 karakter ile sınırlıdır.
* Etiketleri genelleştirilmiş sanal makineleri desteklemez.
* Kaynak grubuna uygulanan etiketler, bu kaynak grubundaki kaynaklar tarafından devralınmaz.
* Bulut Hizmetleri gibi Klasik kaynakları için etiketler uygulanamaz.
* Etiket adları şu karakterleri içeremez: `<`, `>`, `%`, `&`, `\`, `?`, `/`
