---
title: Kaynak kilitlenmesi anlama
description: Bir şema atamasını yaparken kaynakları korumak için kilitleme seçenekleri hakkında bilgi edinin.
author: DCtheGeek
ms.author: dacoulte
ms.date: 03/28/2019
ms.topic: conceptual
ms.service: blueprints
manager: carmonm
ms.custom: seodec18
ms.openlocfilehash: 232d823f364f9f98d1da1bade50ba369b898a57d
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "59788639"
---
# <a name="understand-resource-locking-in-azure-blueprints"></a>Kaynak Azure şemaları kilitleme anlama

Uygun ölçekte tutarlı ortamların oluşturulması yalnızca o tutarlılık sağlamak için bir mekanizma ise gerçekten değerlidir. Bu makalede, Azure planlar içinde kaynak kilitleme nasıl çalıştığı açıklanmaktadır. Kaynak kilitleme ve uygulamanın bir örneğini görmek için _atamaları Reddet_, bkz: [yeni kaynakları koruma](../tutorials/protect-new-resources.md) öğretici.

## <a name="locking-modes-and-states"></a>Kilitleme modları ve durumlar

Şema atamasını için kilitleme modunu uygular ve üç seçenek vardır: **Kilitleme**, **salt okunur**, veya **silmeyin**. Kilitleme modu blueprint ataması sırasında yapıt dağıtımı sırasında yapılandırılır. Şema atamasını güncelleştirerek farklı kilit modu ayarlanabilir.
Modları kilitleme, ancak dışında bir Blueprint'i değiştirilemez.

Şema atamasını yapıları tarafından oluşturulan kaynakları dört durumlarına sahiptir: **Kilitli**, **salt okunur**, **silme / yapamazsınız düzenlemek**, veya **Nelze Odstranit**. Her bir yapıt türü olabilir **kilitli** durumu. Aşağıdaki tabloda, bir kaynak durumunu belirlemek için kullanılabilir:

|Mod|Yapıt kaynağı türü|Durum|Açıklama|
|-|-|-|-|
|Kilitleme|*|Kilitli değil|Kaynakları planlar tarafından korunmayan. Bu durum ayrıca eklenen kaynaklar için kullanılan bir **salt okunur** veya **silmeyin** şema atamasını dışında kaynak grubu yapıt.|
|Salt Okunur|Kaynak grubu|Cannot Edit / Delete|Kaynak grubu salt okunur ve kaynak grubunda etiketlerin değiştirilemez. **Kilitli** kaynakları eklenebilir, taşınabilir, değiştirildi veya bu kaynak grubundan silindi.|
|Salt Okunur|Olmayan bir kaynak grubu|Salt Okunur|Kaynak hiçbir şekilde değiştirilemez--herhangi bir değişiklik ve silinemez.|
|Silmeyin|*|Silinemiyor|Kaynakları değiştirilebilir, ancak silinemez. **Kilitli** kaynakları eklenebilir, taşınabilir, değiştirildi veya bu kaynak grubundan silindi.|

## <a name="overriding-locking-states"></a>Kilitleme durumları geçersiz kılma

Genellikle birisi uygun mümkündür [rol tabanlı erişim denetimi](../../../role-based-access-control/overview.md) (RBAC) 'Owner' rolü gibi aboneliğinizin olmasını alter veya tüm kaynakları silmek için izin verilen. Bu erişim şemaları geçerli olduğu durumlarda dağıtılan bir atama işleminin bir parçası olarak kilitleme durum geçerli değildir. Atama ile ayarlandıysa **salt okunur** veya **silmeyin** seçeneğinde, abonelik bile korumalı kaynağın sahibi engellenen eylem gerçekleştirebilir.

Bu güvenlik önlemi tanımlanan şema ve programlı şekilde veya yanlışlıkla silinmeye veya değiştirmenin oluşturmak için tasarlandığı ortamı tutarlılığı korur.

## <a name="removing-locking-states"></a>Kilitleme durumları kaldırılıyor

Değiştirmek veya atama tarafından korunan bir kaynağa silmek gerekli hale gelirse, bunu yapmak için iki yolu vardır.

- Blueprint ataması için bir kilitleme modunu **kilit yok**
- Şema atamasını silme

Şemalar tarafından oluşturulan kilitler atama kaldırıldığında kaldırılır. Ancak, kaynak geride bıraktığı ve normal yollarla silinmesi gerekir.

## <a name="how-blueprint-locks-work"></a>Blueprint iş nasıl kilitler

Bir RBAC [atamaları Reddet](../../../role-based-access-control/deny-assignments.md) reddetme eylemi uygulanan yapıt kaynaklarına blueprint ataması sırasında atama seçtiyseniz **salt okunur** veya **silmeyin** seçeneği. Reddetme eylemi şema atamasını yönetilen kimlik eklenir ve yalnızca yapıt kaynakları aynı yönetilen kimlik tarafından kaldırılabilir. Bu güvenlik önlemi kilitleme mekanizması uygular ve şemaları dışında şema kilidi kaldırılıyor engeller.

![Blueprint kaynak grubu atamasını Reddet](../media/resource-locking/blueprint-deny-assignment.png)

> [!IMPORTANT]
> Azure Resource Manager rol atama ayrıntıları 30 dakikaya kadar önbelleğe alır. Sonuç olarak, eylemin şema kaynaklar üzerinde hemen tam etkili olmayabilir atamaları Reddet reddet. Bu süre boyunca, şema kilitleri tarafından korunacak amaçlanan bir kaynağı silmek mümkün olabilir.

## <a name="exclude-a-principal-from-a-deny-assignment"></a>Bir sorumlunun bir reddetme atamasından dışlama

Tasarım ya da güvenlik bazı senaryolarda bir sorumlusundan hariç tutmak gerekli olabilir [atamasını Reddet](../../../role-based-access-control/deny-assignments.md) şema atamasını oluşturur. Bu REST API için en fazla beş değerleri ekleyerek yapılır **excludedPrincipals** içindeki dizi **kilitleri** özelliği olduğunda [ataması oluşturma](/rest/api/blueprints/assignments/createorupdate).
Bir örnek içeren bir istek gövdesi **excludedPrincipals**:

```json
{
  "identity": {
    "type": "SystemAssigned"
  },
  "location": "eastus",
  "properties": {
    "description": "enforce pre-defined simpleBlueprint to this XXXXXXXX subscription.",
    "blueprintId": "/providers/Microsoft.Management/managementGroups/{mgId}/providers/Microsoft.Blueprint/blueprints/simpleBlueprint",
    "locks": {
        "mode": "AllResourcesDoNotDelete",
        "excludedPrincipals": [
            "7be2f100-3af5-4c15-bcb7-27ee43784a1f",
            "38833b56-194d-420b-90ce-cff578296714"
        ]
    },
    "parameters": {
      "storageAccountType": {
        "value": "Standard_LRS"
      },
      "costCenter": {
        "value": "Contoso/Online/Shopping/Production"
      },
      "owners": {
        "value": [
          "johnDoe@contoso.com",
          "johnsteam@contoso.com"
        ]
      }
    },
    "resourceGroups": {
      "storageRG": {
        "name": "defaultRG",
        "location": "eastus"
      }
    }
  }
}
```

## <a name="next-steps"></a>Sonraki adımlar

- İzleyin [yeni kaynakları korumak](../tutorials/protect-new-resources.md) öğretici.
- [Şema yaşam döngüsü](lifecycle.md) hakkında bilgi edinin.
- [Statik ve dinamik parametrelerin](parameters.md) kullanımını anlayın.
- [Şema sıralama düzenini](sequencing-order.md) özelleştirmeyi öğrenin.
- [Mevcut atamaları güncelleştirmeyi](../how-to/update-existing-assignments.md) öğrenin.
- [Genel sorun giderme](../troubleshoot/general.md) adımlarıyla şema atama sorunlarını giderin.