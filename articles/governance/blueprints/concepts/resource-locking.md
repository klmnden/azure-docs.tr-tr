---
title: Kaynak Azure şemaları kilitleme anlama
description: Bir şema atamasını yaparken kaynakları korumak için kilitleme seçenekleri hakkında bilgi edinin.
services: blueprints
author: DCtheGeek
ms.author: dacoulte
ms.date: 10/25/2018
ms.topic: conceptual
ms.service: blueprints
manager: carmonm
ms.openlocfilehash: 4e71797837927fe5f5233bcf88d35fef98f504e9
ms.sourcegitcommit: 0f54b9dbcf82346417ad69cbef266bc7804a5f0e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/26/2018
ms.locfileid: "50139451"
---
# <a name="understand-resource-locking-in-azure-blueprints"></a>Kaynak Azure şemaları kilitleme anlama

Uygun ölçekte tutarlı ortamların oluşturulması yalnızca o tutarlılık sağlamak için bir mekanizma ise gerçekten değerlidir. Bu makalede, Azure planlar içinde kaynak kilitleme nasıl çalıştığı açıklanmaktadır.

## <a name="locking-modes-and-states"></a>Kilitleme modları ve durumlar

Kilitleme modu için şema atamasını geçerlidir ve yalnızca iki seçenek vardır: **hiçbiri** veya **tüm kaynakları**. Kilitleme modu blueprint ataması sırasında yapılandırılır ve atama aboneliğine başarıyla uygulandıktan sonra değiştirilemez.

Şema atamasını yapıları tarafından oluşturulan kaynakları üç durumu vardır: **kilitli**, **salt okunur**, veya **silme / yapamazsınız düzenlemek**. Her bir yapıt olabilir **kilitli** durumu. Bununla birlikte, olmayan bir kaynak grubu yapıtlar vardır **salt okunur** ve kaynak gruplarınız **silme / yapamazsınız düzenlemek** durumları. Bu kaynakların nasıl yönetileceğini de önemli bir ayrımdır farktır.

**Salt okunur** durumunda, tam olarak tanımlandığı şekilde: kaynak hiçbir şekilde değiştirilemez--herhangi bir değişiklik ve silinemez. **Silme / yapamazsınız düzenlemek** kaynak grupları "container" yapısı nedeniyle daha inceliklidir. Kaynak Grup nesnesi salt okunur, ancak kilitli olmayan kaynakların kaynak grubunda değişiklik mümkündür.

## <a name="overriding-locking-states"></a>Kilitleme durumları geçersiz kılma

Genellikle birisi uygun mümkündür [rol tabanlı erişim denetimi](../../../role-based-access-control/overview.md) (RBAC) 'Owner' rolü gibi aboneliğinizin olmasını alter veya tüm kaynakları silmek için izin verilen. Bu erişim şemaları geçerli olduğu durumlarda dağıtılan bir atama işleminin bir parçası olarak kilitleme durum geçerli değildir. Atama ile ayarlandıysa **kilit** seçeneğinde, abonelik bile sahibi eklenen kaynaklar değiştirebilirsiniz.

Bu güvenlik önlemi tanımlanan şema ve programlı şekilde veya yanlışlıkla silinmeye veya değiştirmenin oluşturmak için tasarlandığı ortamı tutarlılığı korur.

## <a name="removing-locking-states"></a>Kilitleme durumları kaldırılıyor

Atama tarafından oluşturulan kaynakları silmek gerekli hale gelirse, bunları silmek için ilk Kaldır atama yoludur. Şemalar tarafından oluşturulan kilitler atama kaldırıldığında kaldırılır. Ancak, kaynak geride bıraktığı ve normal yollarla silinmesi gerekir.

## <a name="how-blueprint-locks-work"></a>Blueprint iş nasıl kilitler

Bir RBAC rolü `denyAssignments` atama seçtiyseniz yapıt kaynaklarına blueprint ataması sırasında uygulanan **kilit** seçeneği. Rol şema atamasını yönetilen kimlik eklenir ve yalnızca yapıt kaynakları aynı yönetilen kimlik tarafından kaldırılabilir. Bu güvenlik önlemi kilitleme mekanizması uygular ve şemaları dışında şema kilidi kaldırılıyor engeller. Rol ve kilit kaldırma işlemi yalnızca yalnızca uygun haklara sahip kişiler tarafından gerçekleştirilebilir şema atamasını kaldırarak mümkündür.

> [!IMPORTANT]
> Azure Resource Manager rol atama ayrıntıları 30 dakikaya kadar önbelleğe alır. Sonuç olarak, `denyAssignments` şema üzerinde kaynakların tam aslında hemen olmayabilir. Bu süre boyunca, şema kilitleri tarafından korunacak amaçlanan bir kaynağı silmek mümkün olabilir.

## <a name="next-steps"></a>Sonraki adımlar

- [Şema yaşam döngüsü](lifecycle.md) hakkında bilgi edinin
- [Statik ve dinamik parametreleri](parameters.md) kullanmayı anlayın
- [Şema sıralamasını](sequencing-order.md) özelleştirmeyi öğrenin
- [Var olan atamaları güncelleştirmeyi](../how-to/update-existing-assignments.md) öğrenin
- [Genel sorun giderme](../troubleshoot/general.md) adımlarıyla şema atama sorunlarını giderin