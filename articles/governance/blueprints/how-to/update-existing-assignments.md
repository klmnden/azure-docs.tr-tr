---
title: Mevcut bir atamanın portalından güncelleştirme
description: Mevcut bir Azure şemaları portalda atamadan güncelleştirme mekanizması hakkında bilgi edinin.
author: DCtheGeek
ms.author: dacoulte
ms.date: 10/25/2018
ms.topic: conceptual
ms.service: blueprints
manager: carmonm
ms.custom: seodec18
ms.openlocfilehash: c75bd8c3831bad0c8217f16315843cbe3824fe4d
ms.sourcegitcommit: 37343b814fe3c95f8c10defac7b876759d6752c3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/24/2019
ms.locfileid: "63766608"
---
# <a name="how-to-update-an-existing-blueprint-assignment"></a>Var olan bir şema atamasını güncelleştirme

Bir şema atandığında atama güncelleştirilebilir. Mevcut bir atamanın güncelleştirilmesi için birkaç nedeni vardır dahil olmak üzere:

- Ekleme veya kaldırma [kaynak kilitleme](../concepts/resource-locking.md)
- Değiştirin [dinamik parametreler](../concepts/parameters.md#dynamic-parameters)
- Atama yeni bir yükseltme **yayımlanan** şema sürümü

## <a name="updating-assignments"></a>Atamaları güncelleştiriliyor

1. Seçin **tüm hizmetleri** sol bölmesinde. Arayın ve seçin **şemaları**.

1. Seçin **şemaları atanan** sol sayfasında.

1. Şema atamasını şemaları listede sol. Ardından **güncelleştirme ataması** düğmesini veya şema atamasını sağ tıklayıp **güncelleştirme ataması**.

   ![Var olan bir şema atamasını güncelleştir](../media/update-existing-assignments/update-assignment.png)

1. **Ata şema** sayfa özgün atama tüm değerlerle önceden doldurulmuş olarak yüklenecektir. Değiştirebileceğiniz **şema tanımı sürümü**, **kilit atama** durumunu ve herhangi bir şema tanımı üzerinde mevcut dinamik parametre. Tıklayın **atama** değişiklikleriniz bittiğinde.

1. Yeni durum güncelleştirilmiş atama Ayrıntıları sayfasında bakın. Bu örnekte, eklediğimiz **kilitleme** atama.

   ![Var olan bir şema atamasını - kilit modu değiştirildi güncelleştirildi](../media/update-existing-assignments/updated-assignment.png)

1. Diğer ayrıntılarını keşfetmek **atama işlemleri** açılan listeyi kullanarak. İçindekiler tablosunu **yönetilen kaynakları** güncelleştirilerek seçilen atama işlemi.

   ![Şema atamasını atama işlemleri](../media/update-existing-assignments/assignment-operations.png)

## <a name="rules-for-updating-assignments"></a>Kuralları atamaları güncelleştiriliyor

Güncelleştirilmiş atamaları dağıtımını birkaç önemli kurallara uygun olmalıdır. Bu kurallar, zaten dağıtılmış kaynaklar için ne olacağını belirler. Hangi eylemler gerçekleştirildikçe istenen değişikliği ve dağıtılmış veya güncelleştirilmesi yapıt kaynak türünü belirler.

- Rol Atamaları
  - Rol veya (kullanıcı, Grup veya uygulama) rolü atanan değişirse, yeni bir rol ataması oluşturulur. Daha önce dağıttığınız rol atamaları yerinde kalır.
- İlke Atamaları
  - İlke ataması parametrelerinin değiştirilirse mevcut atamanın güncelleştirilir.
  - İlke ataması tanımı değiştirilirse, yeni ilke ataması oluşturulur. Daha önce dağıtılan ilke atamaları yerinde kalır.
  - İlke atama yapıtındaki blueprint kaldırılırsa, ilke atamaları yerinde kalır dağıtıldı.
- Azure Resource Manager şablonları
  - Şablon olarak Resource Manager üzerinden işlenir bir **PUT**. Her kaynak türü farklı bu eylemin işlediği gibi şemalar tarafından çalıştırıldığında bu eylem etkisini belirlemek dahil edilen her kaynak için belgeleri gözden geçirin.

## <a name="possible-errors-on-updating-assignments"></a>Olası hataları üzerinde atamaları güncelleştiriliyor

Atamalar güncelleştirilirken yürütüldüğünde sonu değişiklik yapmak mümkündür. Bir örnek, zaten dağıtıldıktan sonra bir kaynak grubu konumunu değişiyor. Tarafından desteklenen herhangi bir değişiklik [Azure Resource Manager](../../../azure-resource-manager/resource-group-overview.md) yapılması olabilir, ancak herhangi Azure Resource Manager aracılığıyla bir hata sonucunda da neden olur atama hatası değiştirebilirsiniz.

Atama bir kaç kez güncelleştirilebilir bir sınır yoktur. Bir hata oluşursa, hatayı belirlemek ve başka bir güncelleştirme için atama yapın.  Örnek hata senaryolar:

- Hatalı bir parametre
- Varolan bir nesneye
- Azure Resource Manager tarafından desteklenmeyen bir değişiklik

## <a name="next-steps"></a>Sonraki adımlar

- [Şema yaşam döngüsü](../concepts/lifecycle.md) hakkında bilgi edinin.
- [Statik ve dinamik parametrelerin](../concepts/parameters.md) kullanımını anlayın.
- [Şema sıralama düzenini](../concepts/sequencing-order.md) özelleştirmeyi öğrenin.
- [Şema kaynak kilitleme](../concepts/resource-locking.md) özelliğini kullanmayı öğrenin.
- [Genel sorun giderme](../troubleshoot/general.md) adımlarıyla şema atama sorunlarını giderin.