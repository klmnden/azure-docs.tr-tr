---
title: Mevcut bir Azure şema atamasını güncelleştirme
description: Azure şemaları, mevcut bir atamanın güncelleştirilmesi mekanizması hakkında bilgi edinin.
services: blueprints
author: DCtheGeek
ms.author: dacoulte
ms.date: 09/18/2018
ms.topic: conceptual
ms.service: blueprints
manager: carmonm
ms.openlocfilehash: ecac0fb21a6691874d5e8db49eadd7114d41845f
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "46956209"
---
# <a name="how-to-update-an-existing-blueprint-assignment"></a>Var olan bir şema atamasını güncelleştirme

Bir şema atandığında atama güncelleştirilebilir. Mevcut bir atamanın güncelleştirilmesi için birkaç nedeni vardır dahil olmak üzere:

- Ekleme veya kaldırma [kaynak kilitleme](../concepts/resource-locking.md)
- Değiştirin [dinamik parametreler](../concepts/parameters.md#dynamic-parameters)
- Atama yeni bir yükseltme **yayımlanan** şema sürümü

## <a name="updating-assignments"></a>Atamaları güncelleştiriliyor

1. Tıklayarak Azure portalında Azure şemaları hizmet başlatma **tüm hizmetleri** arama ve seçme **ilke** sol bölmesinde. Üzerinde **ilke** sayfasında, tıklayarak **şemaları**.

1. Seçin **atanan şemalar** sol sayfasında.

1. Planlar listesinden, şema atamasını tıklatın ve ardından **güncelleştirme ataması** düğmesini veya şema atamasını sağ tıklayıp **güncelleştirme ataması**.

   ![Güncelleştirme ataması](../media/update-existing-assignments/update-assignment.png)

1. **Atama şema** sayfa özgün atama tüm değerlerle önceden doldurulmuş olarak yüklenecektir. Değiştirebileceğiniz **şema tanımı sürümü**, **kilit atama** durumunu ve herhangi bir şema tanımı üzerinde mevcut dinamik parametre. Tıklayın **atama** değişiklikleriniz bittiğinde.

1. Yeni durum güncelleştirilmiş atama Ayrıntıları sayfasında bakın. Bu örnekte, eklediğimiz **kilitleme** atama.

   ![Güncelleştirilmiş atama - kilitli](../media/update-existing-assignments/updated-assignment.png)

1. Diğer ayrıntılarını keşfetmek **atama işlemleri** açılan listeyi kullanarak. İçindekiler tablosunu **yönetilen kaynaklar** güncelleştirilerek seçilen atama işlemi.

   ![Atama işlemleri](../media/update-existing-assignments/assignment-operations.png)

## <a name="rules-for-updating-assignments"></a>Kuralları atamaları güncelleştiriliyor

Güncelleştirilmiş atamaları dağıtımını birkaç önemli kurallara uygun olmalıdır. Bu kurallar, istenen değişikliği ve yapıt kaynağı olan türüne bağlı olarak var olan bir kaynak dağıtılan veya güncelleştirilmiş ne belirler.

- Rol Atamaları
  - Rol veya (kullanıcı, Grup veya uygulama) rolü atanan değişirse, yeni bir rol ataması oluşturulur. Daha önceden dağıtılan rol ataması yerinde kalır.
- İlke Atamaları
  - İlke ataması parametrelerinin değiştirilirse mevcut atamanın güncelleştirilir.
  - İlke ataması tanımı değiştirilirse, yeni ilke ataması oluşturulur. Daha önce dağıtılan ilke ataması yerinde kalır.
  - Daha önce dağıtılan ilke ataması, ilke atama yapıtındaki blueprint kaldırılırsa, yerinde kalır.
- Azure Resource Manager şablonları
  - Şablon olarak Resource Manager üzerinden işlenir bir **PUT**. Her kaynak türü bu farklı işler, şemalar tarafından çalıştırıldığında bu eylem etkisini belirlemek dahil edilen her kaynak için belgeleri gözden geçirin.

## <a name="possible-errors-on-updating-assignments"></a>Olası hataları üzerinde atamaları güncelleştiriliyor

Atamalar güncelleştirilirken yürütüldüğünde sonu değişiklik yapmak mümkündür. Buna örnek olarak, zaten dağıtıldıktan sonra bir kaynak grubu konumunu değişiyor. Tarafından desteklenen herhangi bir değişiklik [Azure Resource Manager](../../../azure-resource-manager/resource-group-overview.md) yapılması olabilir, ancak herhangi Azure Resource Manager aracılığıyla bir hata sonucunda da neden olur atama hatası değiştirebilirsiniz.

Atama bir kaç kez güncelleştirilebilir bir sınır yoktur. Bu nedenle, ya da hatalı bir parametre, varolan bir nesneye veya Azure Resource Manager tarafından izin verilmeyen bir değişiklik nedeniyle bir hata oluşursa hatayı belirlemek ve başka bir güncelleştirme için atama yapın.

## <a name="next-steps"></a>Sonraki adımlar

- Hakkında bilgi edinin [blueprint yaşam döngüsü](../concepts/lifecycle.md)
- Nasıl kullanılacağını anlamak [statik ve dinamik parametreleri](../concepts/parameters.md)
- Özelleştirme öğrenin [blueprint sıralama sırası](../concepts/sequencing-order.md)
- Öğrenin yapmak kullanım [blueprint kaynak kilitleme](../concepts/resource-locking.md)
- İle bir blueprint ataması sırasında sorunları gidermek [genel sorun giderme](../troubleshoot/general.md)
