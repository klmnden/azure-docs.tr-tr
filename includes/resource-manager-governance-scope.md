---
title: include dosyası
description: include dosyası
services: azure-resource-manager
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: include
ms.date: 02/21/2018
ms.author: tomfitz
ms.custom: include file
ms.openlocfilehash: dc4281c17b92e1720625764a52a34a94d6f296ab
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/23/2019
ms.locfileid: "66164584"
---
Bir öğe oluşturmadan önce kapsam kavramını gözden geçirelim. Azure dört yönetim düzeyi sunar: yönetim grupları, abonelik, kaynak grubu ve kaynak. [Yönetim grupları](../articles/billing/billing-enterprise-mgmt-group-overview.md) önizleme sürümündedir. Aşağıdaki resimde bu katmanlara ait bir örnek gösterilir.

![`Scope`](./media/resource-manager-governance-scope/scope-levels.png)

Yönetim ayarlarını bu kapsam düzeylerinden birinde uygularsınız. Seçtiğiniz düzey, ayarın ne kadar yaygın olarak uygulanacağını belirler. Düşük düzeyler, yüksek düzeylerdeki ayarları devralır. Aboneliğe bir ayar uyguladığınızda, bu ayar aboneliğinizdeki tüm kaynak gruplarına ve kaynaklara uygulanır. Kaynak grubunda bir ayar uyguladığınızda, bu ayar kaynak grubu ve tüm kaynaklarına uygulanır. Ancak başka bir kaynak grubunda bu ayar bulunmaz.

Genellikle, önemli ayarları yüksek düzeylerde, projeye özgü gereksinimleri ise düşük düzeylerde uygulamak en mantıklı yoldur. Örneğin, kuruluşunuza ait tüm kaynakların belirli bölgelere dağıtıldığından emin olmak isteyebilirsiniz. Bu gereksinimi gerçekleştirmek için, aboneliğe izin verilen konumları belirten bir ilke uygulayın. Kuruluşunuzda bulunan diğer kullanıcılar yeni kaynak grupları ve kaynaklar eklediğinde, izin verilen konumlar otomatik olarak uygulanır. 
