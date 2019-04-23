---
title: Bir blueprint'in yaşam döngüsünü anlama
description: Her aşamanın ayrıntılarını ve bir şema geçtiği yaşam döngüsü hakkında bilgi edinin.
author: DCtheGeek
ms.author: dacoulte
ms.date: 02/01/2019
ms.topic: conceptual
ms.service: blueprints
manager: carmonm
ms.custom: seodec18
ms.openlocfilehash: a57085fa37efd56a46b740d8cbc4278dc53cf39f
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "59795432"
---
# <a name="understand-the-lifecycle-of-an-azure-blueprint"></a>Bir Azure blueprint'in yaşam döngüsünü anlama

Azure içinde birçok kaynaklar gibi Azure şemaları blueprint'te tipik ve doğal bir yaşam döngüsü vardır. Bunlar oluşturulan, dağıtılan ve son olarak, artık gerekli veya ilgili silindi.
Blueprint standart yaşam döngüsü işlemleri destekler. Daha sonra bunları kod – DevOps anahtar bir öğe olarak kendi altyapısını yönetme kuruluşlar için yaygın bir sürekli tümleştirme ve sürekli dağıtım işlem hatları destekleyen ek durumu düzeyleri sağlamak için temel oluşturur.

Bir şema ve aşamalar tam olarak anlamak için standart bir yaşam döngüsü değineceğiz:

> [!div class="checklist"]
> - Oluşturma ve bir şema düzenleme
> - Blueprint yayımlama
> - Oluşturma ve düzenleme, yeni bir şema sürümü
> - Yeni bir şema sürümünü yayımlama
> - Blueprint belirli bir sürümü siliniyor
> - Şema silme

## <a name="creating-and-editing-a-blueprint"></a>Oluşturma ve bir şema düzenleme

Bir şema oluşturma eklediğinizde, yapılara bir yönetim grubuna veya aboneliğe kaydedin ve benzersiz bir ad ve benzersiz bir sürümü sağlanmaktadır. Blueprint artık bulunduğu bir **taslak** modu ve henüz atanamaz. İçinde çalışırken **taslak** modu da devam edebilirsiniz güncelleştirilen ve değiştirildi.

A hiçbir zaman blueprint'te yayımlanan **taslak** modu üzerinde farklı bir simge görüntüler **şema tanımları** olan olanları sayfadan **yayımlanan**. **En son sürümü** olarak görüntülenen **taslak** hiçbir zaman yayımlanan bu planlar için.

Oluşturma ve düzenleme ile bir şema [Azure portalında](../create-blueprint-portal.md#create-a-blueprint) veya [REST API](../create-blueprint-rest-api.md#create-a-blueprint).

## <a name="publishing-a-blueprint"></a>Blueprint yayımlama

İçin bir şema tüm planlı değişiklikler yapıldıktan sonra **taslak** modunda olabilir **yayımlanan** ve atama için kullanılabilir. **Yayımlanan** şema sürümü olamaz değiştirilebilir.
Bir kez **yayımlanan**, farklı bir simge içeren şema görüntüler **taslak** Blueprint ve sağlanan sürüm numarasını görüntüler **en son sürümü** sütun.

Bir şema ile yayımlama [Azure portalında](../create-blueprint-portal.md#publish-a-blueprint) veya [REST API](../create-blueprint-rest-api.md#publish-a-blueprint).

## <a name="creating-and-editing-a-new-version-of-the-blueprint"></a>Oluşturma ve düzenleme, yeni bir şema sürümü

A **yayımlanan** bir şema sürümü olamaz değiştirilebilir. Ancak, yeni bir şema sürümü mevcut blueprint'e eklenebilen ve gerektiğinde değişiklik. Var olan bir şema için düzenleyerek değişiklik. Yeni değişiklikler kaydedildiğinde, şema sunuyor **yayımdan kaldırılmış değişikliklerinizi**. Bu yeni değişiklikler **taslak** şema sürümü.

Bir şema ile düzenleme [Azure portalında](../create-blueprint-portal.md#edit-a-blueprint).

## <a name="publishing-a-new-version-of-the-blueprint"></a>Yeni bir şema sürümünü yayımlama

Düzenlenen her bir şema sürümü olmalıdır **yayımlanan** atanabilmesi için önce. Zaman **yayımdan kaldırılmış değişikliklerinizi** bir şema için yapılan ama **yayımlanan**, **Blueprint Yayımla** düğmesi düzenleme şema sayfasında kullanılabilir. Düğmenin görünür değilse, şema zaten yüklenmişse **yayımlanan** ve hiçbir **yayımdan kaldırılmış değişikliklerinizi**.

> [!NOTE]
> Tek bir şema, birden çok olabilir **yayımlanan** birbirlerinden sürümleri aboneliğe atanabilir.

Bir şema ile yayımlamak için **yayımdan kaldırılmış değişikliklerinizi**, yeni şema yayımlamak için aynı adımları kullanın.

## <a name="deleting-a-specific-version-of-the-blueprint"></a>Blueprint belirli bir sürümü siliniyor

Her bir şema sürümü benzersiz bir nesnedir ve ayrı ayrı olabilir **yayımlanan**. Bu nedenle, her bir şema sürümü de silinebilir. Bir şema sürümü siliniyor o diğer sürümleri üzerinde hiçbir etkisi yok.

> [!NOTE]
> Etkin bir ataması yok bir şema silmek mümkün değildir. Atamaları silin ve ardından kaldırmak istediğiniz sürümü silin.

1. Seçin **tüm hizmetleri** sol bölmesinde. Arayın ve seçin **şemaları**.

1. Seçin **Blueprint tanımları** sayfasında sol ve bir sürümünü silmek istediğiniz şema bulmak için filtre seçeneklerini kullanın. Düzen sayfasını açmak için tıklayın.

1. Tıklayın **sürümleri yayımlanan** sekmesini ve silmek istediğiniz sürümü bulun.

1. Sürümü silin ve seçmek için sağ **bu sürümü Sil**.

## <a name="deleting-the-blueprint"></a>Şema silme

Çekirdek blueprint da silinebilir. Çekirdek blueprint silinmesi da her ikisi de dahil olmak üzere bu blueprint'in tüm şema sürümlerini siler **taslak** ve **yayımlanan** planlar. Olarak bir şema sürümünü silme işlemine çekirdek şema silme şema sürümlerinin herhangi birinin mevcut atamaları kaldırmaz.

> [!NOTE]
> Etkin bir ataması yok bir şema silmek mümkün değildir. Atamaları silin ve ardından kaldırmak istediğiniz sürümü silin.

Bir şema ile Sil [Azure portalında](../create-blueprint-portal.md#delete-a-blueprint) veya [REST API](../create-blueprint-rest-api.md#delete-a-blueprint).

## <a name="assignments"></a>Atamalar

Blueprint bir aboneliğe atanabilir yaşam döngüsü boyunca birkaç nokta vardır. Şema sürümü modunda olduğunda **yayımlanan**, sonra da bu sürüm bir aboneliğe atanabilir. Bu yaşam döngüsünü kullanılan ve yeni bir sürüme geliştirilen etkin olarak atanmış bir şema sürümleri sağlar.

Blueprint sürümlerini atandığından, burada atanmış oldukları ve hangi parametrelerle bunların ile atanan anlamak önemlidir. Parametreleri ya da statik veya dinamik olabilir. Daha fazla bilgi için bkz. [statik ve dinamik parametreleri](parameters.md).

### <a name="updating-assignments"></a>Atamaları güncelleştiriliyor

Bir şema atandığında atama güncelleştirilebilir. Mevcut bir atamanın güncelleştirilmesi için birkaç nedeni vardır dahil olmak üzere:

- Ekleme veya kaldırma [kaynak kilitleme](resource-locking.md)
- Değiştirin [dinamik parametreler](parameters.md#dynamic-parameters)
- Atama yeni bir yükseltme **yayımlanan** şema sürümü

Bilgi edinmek için bkz [mevcut Atamaları Güncelleştir](../how-to/update-existing-assignments.md).

## <a name="next-steps"></a>Sonraki adımlar

- [Statik ve dinamik parametrelerin](parameters.md) kullanımını anlayın.
- [Şema sıralama düzenini](sequencing-order.md) özelleştirmeyi öğrenin.
- [Şema kaynak kilitleme](resource-locking.md) özelliğini kullanmayı öğrenin.
- [Mevcut atamaları güncelleştirmeyi](../how-to/update-existing-assignments.md) öğrenin.
- [Genel sorun giderme](../troubleshoot/general.md) adımlarıyla şema atama sorunlarını giderin.