---
title: Kaynak Azure şemaları kilitleme anlama
description: Bir şema atamasını yaparken kaynakları korumak için kilitleme seçenekleri hakkında bilgi edinin.
services: blueprints
author: DCtheGeek
ms.author: dacoulte
ms.date: 09/18/2018
ms.topic: conceptual
ms.service: blueprints
manager: carmonm
ms.openlocfilehash: 718c23b806da5058c042b961077e51ba0d4b6245
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "46973261"
---
# <a name="understand-resource-locking-in-azure-blueprints"></a>Kaynak Azure şemaları kilitleme anlama

Uygun ölçekte tutarlı ortamların oluşturulması yalnızca tutarlılık kalıcı olmasını sağlamak için bir mekanizma ise gerçekten değerlidir. Bu makalede, Azure planlar içinde kaynak kilitleme nasıl çalıştığı açıklanmaktadır.

## <a name="locking-modes-and-states"></a>Kilitleme modları ve durumlar

Kilitleme modu için şema atamasını geçerlidir ve yalnızca iki seçenek vardır: **hiçbiri** veya **tüm kaynakları**. Bu, blueprint ataması sırasında yapılandırılır ve atama aboneliğine başarıyla uygulandıktan sonra değiştirilemez.

Abonelik için atanan blueprint'e yapıt tanımlarını tarafından oluşturulan kaynakları üç durumu vardır: **kilitli**, **salt okunur**, veya **silme / yapamazsınız düzenlemek**. Yapıt herhangi bir türden olabilir **kilitli** durumu. Ancak, olarak olmayan bir kaynak grubu yapıtların durumda **salt okunur** ve kaynak grupları olarak **silme / yapamazsınız düzenlemek**. Bu kaynakların nasıl yönetileceğini de önemli bir fark budur.

**Salt okunur** durumunda, tam olarak tanımlandığı şekilde: kaynak hiçbir şekilde değiştirilemez--herhangi bir değişiklik ve silinemez. **Silme / yapamazsınız düzenlemek** kaynak grupları "container" yapısı nedeniyle daha inceliklidir. Kaynak Grup nesnesi, salt okunur ancak oluşturmak mümkündür, güncelleştirme ve silme kaynakları kaynak grubu--bunlar herhangi bir blueprint atama ile bir parçası olmayan sürece **salt okunur** kilitleme durumu.

## <a name="overriding-locking-states"></a>Kilitleme durumları geçersiz kılma

Uygun olan tipik olarak mümkün olsa [rol tabanlı erişim denetimi](../../../role-based-access-control/overview.md) (RBAC) 'Owner' rolü gibi aboneliğinizin değiştirme veya herhangi bir kaynağa silme yeteneğine sahip olacak şekilde bu durum geçerli değildir, şemalar dağıtılan bir atama işleminin bir parçası olarak kilitleme geçerlidir. Atama ile ayarlandıysa **kilit** seçeneğinde, abonelik bile sahibi eklenen kaynaklar değiştirebilirsiniz.

Bu, tanımlanan şema ve programlı şekilde veya yanlışlıkla silinmeye veya değiştirmenin oluşturmak için tasarlandığı ortamı tutarlılığı korur.

## <a name="removing-locking-states"></a>Kilitleme durumları kaldırılıyor

Atama tarafından oluşturulan kaynakları silmek gerekli hale gelirse, ardından silmek için yalnızca ilk Kaldır atama yoludur. Şemalar tarafından oluşturulan kilitler atama kaldırıldığında kaldırılır. Ancak, kaynak geride bıraktığı ve normal yollarla, uygun izinlere sahip biri tarafından silinmesi gerekir.

## <a name="how-blueprint-locks-work"></a>Blueprint iş nasıl kilitler

Bir RBAC rolü `denyAssignments` atama seçtiyseniz yapıt kaynaklarına blueprint ataması sırasında uygulanan **kilit** seçeneği. Rol şema atamasını yönetilen kimlik eklenir ve yalnızca yapıt kaynakları aynı yönetilen kimlik tarafından kaldırılabilir. Bu, kilitleme mekanizması uygular ve girişimlerini dışında şemalar şema kilidi kaldırın. Rol ve kilit kaldırma işlemi yalnızca yalnızca uygun haklara sahip kişiler tarafından gerçekleştirilebilir şema atamasını kaldırarak mümkündür.

## <a name="next-steps"></a>Sonraki adımlar

- Hakkında bilgi edinin [blueprint yaşam döngüsü](lifecycle.md)
- Nasıl kullanılacağını anlamak [statik ve dinamik parametreleri](parameters.md)
- Özelleştirme öğrenin [blueprint sıralama sırası](sequencing-order.md)
- Bilgi edinmek için nasıl [mevcut Atamaları Güncelleştir](../how-to/update-existing-assignments.md)
- İle bir blueprint ataması sırasında sorunları gidermek [genel sorun giderme](../troubleshoot/general.md)