---
title: "include dosyası"
description: "include dosyası"
services: azure-resource-manager
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: include
ms.date: 02/21/2018
ms.author: tomfitz
ms.custom: include file
ms.openlocfilehash: 4f5dbbd1da8a221fb5f8b9641e016829987db538
ms.sourcegitcommit: 12fa5f8018d4f34077d5bab323ce7c919e51ce47
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/23/2018
---
Tüm öğeleri oluşturmadan önce şimdi kapsam kavramı gözden geçirin. Azure yönetim dört düzeyleri sağlar: Yönetim grupları, abonelik, kaynak grubu ve kaynak. [Yönetim grupları](../articles/billing/billing-enterprise-mgmt-group-overview.md) Önizleme sürümünde olan. Aşağıdaki resimde bu katmanların bir örnek gösterilmektedir.

![Kapsam](./media/resource-manager-governance-scope/scope-levels.png)

Yönetim ayarlarda bu kapsam düzeyleri uygulayın. Seçtiğiniz düzeyi ayarı yaygın olarak nasıl uygulandığını belirler. Düşük düzeylerde yüksek düzeylerinden ayarlarını devralır. Abonelik için bir ayar uygulanırsa, bu ayarı tüm kaynak grupları ve aboneliğinizde kaynaklara uygulanır. Bir ayar uygulamak ayardır kaynak grubu, kaynak grubu ve tüm kaynaklarını uygulanır. Ancak, başka bir kaynak grubu bu ayarı yok.

Genellikle, yüksek düzeyde kritik ayarları ve daha düşük düzeylerdeki proje özgü gereksinimler uygulamak için mantıklıdır. Örneğin, kuruluşunuz için tüm kaynakların belirli bölgelere dağıtılan emin olmak isteyebilirsiniz. Bu gereksinim gerçekleştirmek için izin verilen konumların belirten abonelik için bir ilke uygulanır. Kuruluşunuzda bulunan diğer kullanıcıların yeni kaynak grubu ve kaynak eklediğinizde, izin verilen konumların otomatik olarak uygulanır. 
