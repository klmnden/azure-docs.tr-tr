---
title: Azure şema yaşam döngüsünü anlama
description: Her aşamanın ayrıntılarını ve bir şema geçtiği yaşam döngüsü hakkında bilgi edinin.
services: blueprints
author: DCtheGeek
ms.author: dacoulte
ms.date: 09/18/2018
ms.topic: conceptual
ms.service: blueprints
manager: carmonm
ms.openlocfilehash: e0790168a8b9590aaa440a04cd99f26c2ece2818
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "46991553"
---
# <a name="understand-the-life-cycle-of-an-azure-blueprint"></a>Azure şema yaşam döngüsünü anlama

Azure içinde birçok kaynaklar gibi Azure şemaları blueprint'te bir tipik ve doğal yaşam döngüsü vardır. Bunlar oluşturulan, dağıtılan ve son olarak, artık gerekli veya ilgili silindi.
Blueprint geleneksel CRUD (oluşturma/okuma/güncelleştirme/silme) yaşam döngüsü işlemleri destekler, ancak bunlar üzerine ortak sürekli tümleştirmeyi ek durum düzeylerini sağlamak için ayrıca oluşturur / sürekli dağıtım (CI/CD) işlem hatları kullanıma göre kod – temel bir DevOps öğesinde aracılığıyla altyapılarını yönetme kod olarak altyapı (Iac) denir.

Bir şema ve aşamalar tam olarak anlamak için standart bir yaşam döngüsü değineceğiz:

> [!div class="checklist"]
> - Oluşturma ve bir şema düzenleme
> - Blueprint yayımlama
> - Oluşturma ve düzenleme, yeni bir şema sürümü
> - Yeni bir şema sürümünü yayımlama
> - Blueprint belirli bir sürümü siliniyor
> - Şema silme

## <a name="creating-and-editing-a-blueprint"></a>Oluşturma ve bir şema düzenleme

Bir şema oluştururken, yapılara bir yönetim grubuna kaydetme ve benzersiz bir ad ve benzersiz bir sürüm sağlanan ekleyin. Blueprint bulunduğu bu noktada, bir **taslak** modu ve henüz atanamaz. Ancak, while **taslak** modu devam güncelleştirildi ve değiştirildi.

Bir blueprint'te **taslak** hiç yayımlanmadı modu üzerinde farklı bir simge görüntülenir **şema tanımları** olan bu sayfadan **yayımlanan**. **En son sürümü** olarak görüntülenecek **taslak** hiçbir zaman yayımlanan bu planlar için.

Oluşturma ve düzenleme ile bir şema [Azure portalında](../create-blueprint-portal.md#create-a-blueprint) veya [REST API](../create-blueprint-rest-api.md#create-a-blueprint).

## <a name="publishing-a-blueprint"></a>Blueprint yayımlama

Tüm istenen değişiklikleri bir blueprint'te yaptıktan sonra **taslak** modunda olabilir **yayımlanan** ve atama için kullanılabilir. **Yayımlanan** şema sürümü olamaz değiştirilebilir.
Bir kez **yayımlanan**, farklı bir simge içeren şema görüntüler **taslak** Blueprint ve sağlanan sürüm numarasını görüntüler **en son sürümü** sütun.

Bir şema ile yayımlama [Azure portalında](../create-blueprint-portal.md#publish-a-blueprint) veya [REST API](../create-blueprint-rest-api.md#publish-a-blueprint).

## <a name="creating-and-editing-a-new-version-of-the-blueprint"></a>Oluşturma ve düzenleme, yeni bir şema sürümü

Ancak bir **yayımlanan** bir şema sürümü olamaz değiştirilebilir, şema yeni bir sürümü mevcut blueprint'e eklenebilir ve gerektiği şekilde değiştirdi. Bu işlem için var olan bir şema değişiklikleri yaparak gerçekleştirilir. Şema zaten olduysa **yayımlanan**, bu değişiklikler kaydedildiğinde, bunlar gösterme **yayımdan kaldırılmış değişikliklerinizi** şema tanımları listesinde. Değişiklikleri kaydeder kaydedilirken bir **taslak** şema sürümü.

Bir şema ile düzenleme [Azure portalında](../create-blueprint-portal.md#edit-a-blueprint).

## <a name="publishing-a-new-version-of-the-blueprint"></a>Yeni bir şema sürümünü yayımlama

Yalnızca bir şema ilk sürümü gibi **yayımlanan** atamak için sonraki her sürümü aynı bu blueprint'in olmalıdır **yayımlanan** atanabilmesi için önce. Zaman **yayımdan kaldırılmış değişikliklerinizi** bir şema için yapıldı ancak henüz etkinleştirilmemiş **yayımlanan**, **Blueprint Yayımla** düğmesi düzenleme şema sayfasında kullanılabilir. Düğmenin görünür değilse, şema zaten yüklenmişse **yayımlanan**, ancak hiçbir **yayımdan kaldırılmış değişikliklerinizi**.

> [!NOTE]
> Tek bir şema, birden çok olabilir **yayımlanan** birbirlerinden sürümleri aboneliğe atanabilir.

Bir şema ile yayınlama adımlarını **yayımdan kaldırılmış değişikliklerinizi** yeni bir sürümü var olan bir şema yeni şemayı Yayımla adımları aynıdır.

## <a name="deleting-a-specific-version-of-the-blueprint"></a>Blueprint belirli bir sürümü siliniyor

Her bir şema sürümü benzersiz bir nesnedir ve ayrı ayrı olabilir **yayımlanan**. Bu, aynı zamanda her bir şema sürümü silinebilir anlamına gelir. Bir şema sürümü siliniyor o diğer sürümleri üzerinde hiçbir etkisi yok.

> [!NOTE]
> Etkin bir ataması yok bir şema silmek mümkün değildir. Atamaları silin ve ardından kaldırmak istediğiniz sürümü silin.

1. Tıklayarak Azure portalında Azure şemaları hizmet başlatma **tüm hizmetleri** arama ve seçme **ilke** sol bölmesinde. Üzerinde **ilke** sayfasında, tıklayarak **şemaları**.

1. Seçin **şema tanımları** sayfasında sol ve bir sürümünü silmek istediğiniz şema bulmak için filtre seçeneklerini kullanın. Düzen sayfasını açmak için tıklayın.

1. Tıklayın **sürümleri yayımlanan** sekmesini ve silmek istediğiniz sürümü bulun.

1. Sürümü silin ve seçmek için sağ **bu sürümü Sil**.

## <a name="deleting-the-blueprint"></a>Şema silme

Çekirdek blueprint da silinebilir. Çekirdek blueprint da silmek bu blueprint'in tüm şema sürümleri bakılmaksızın **taslak** veya **yayımlanan** durumu. Olarak bir şema sürümünü silme işlemine çekirdek şema silme şema sürümlerinin herhangi birinin mevcut atamaları kaldırmaz.

> [!NOTE]
> Etkin bir ataması yok bir şema silmek mümkün değildir. Atamaları silin ve ardından kaldırmak istediğiniz sürümü silin.

Bir şema ile Sil [Azure portalında](../create-blueprint-portal.md#delete-a-blueprint) veya [REST API](../create-blueprint-rest-api.md#delete-a-blueprint).

## <a name="assignments"></a>Atamalar

Blueprint bir aboneliğe atanabilir yaşam döngüsü sırasında birkaç nokta vardır.
Her bir şema sürümünü moddur **yayımlanan**, sonra da bu sürüm bir aboneliğe atanabilir. Olduğunda bile bir **taslak** olması durumunda bir veya daha fazla şema sürümlerinde şema sürümü bir **yayımlanan** modu, ardından bunların her biri **yayımlanan** sürüm atamak kullanılabilir. Bu, kullanılan ve yeni bir sürüme geliştirilen etkin olarak atanmış bir şema sürümleri sağlar.

Blueprint sürümlerini atandığından, burada atanmış oldukları ve hangi parametrelerle bunların ile atanan anlamak önemlidir. Parametreleri ya da statik veya dinamik olabilir. Daha fazla bilgi için bkz. [statik ve dinamik parametreleri](parameters.md).

### <a name="updating-assignments"></a>Atamaları güncelleştiriliyor

Bir şema atandığında atama güncelleştirilebilir. Mevcut bir atamanın güncelleştirilmesi için birkaç nedeni vardır dahil olmak üzere:

- Ekleme veya kaldırma [kaynak kilitleme](resource-locking.md)
- Değiştirin [dinamik parametreler](parameters.md#dynamic-parameters)
- Atama yeni bir yükseltme **yayımlanan** şema sürümü

Bilgi edinmek için bkz [mevcut Atamaları Güncelleştir](../how-to/update-existing-assignments.md).

## <a name="next-steps"></a>Sonraki adımlar

- Nasıl kullanılacağını anlamak [statik ve dinamik parametreleri](parameters.md)
- Özelleştirme öğrenin [blueprint sıralama sırası](sequencing-order.md)
- Öğrenin yapmak kullanım [blueprint kaynak kilitleme](resource-locking.md)
- Bilgi edinmek için nasıl [mevcut Atamaları Güncelleştir](../how-to/update-existing-assignments.md)
- İle bir blueprint ataması sırasında sorunları gidermek [genel sorun giderme](../troubleshoot/general.md)