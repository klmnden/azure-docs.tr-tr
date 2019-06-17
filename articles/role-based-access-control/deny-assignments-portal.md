---
title: Azure portalını kullanarak Azure kaynakları için atamaları izin verilmeyenler listesi | Microsoft Docs
description: Kullanıcılar, gruplar, hizmet sorumluları ve Azure portalını kullanarak belirli kapsamları, belirli bir Azure kaynak eylemlerine erişim reddedildi yönetilen kimlikleri listesi öğrenin.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.assetid: 8078f366-a2c4-4fbb-a44b-fc39fd89df81
ms.service: role-based-access-control
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 06/10/2019
ms.author: rolyon
ms.reviewer: bagovind
ms.openlocfilehash: 21ffb1a2539a2e724a91dd3b2818270a5e573ef8
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67127482"
---
# <a name="list-deny-assignments-for-azure-resources-using-the-azure-portal"></a>Azure portalını kullanarak Azure kaynakları için atamaları izin verilmeyenler listesi

[Atamalar Reddet](deny-assignments.md) bir rol ataması bunları erişim verse bile kullanıcıların belirli bir Azure kaynak eylemler gerçekleştirme. Bu makalede nasıl listelemek Azure portalını kullanarak atamaları reddet.

> [!NOTE]
> Doğrudan kendi oluşturamazsınız atamaları reddet. Hakkında bilgi reddetmek için atamaları oluşturulur bkz [atamaları Reddet](deny-assignments.md).

## <a name="prerequisites"></a>Önkoşullar

Bir reddetme atama hakkında bilgi edinmek için şunlara sahip olmalısınız:

- `Microsoft.Authorization/denyAssignments/read` çoğu dahil izni [Azure kaynakları için yerleşik roller](built-in-roles.md).

## <a name="list-deny-assignments"></a>Atamalar izin verilmeyenler listesi

Aşağıdaki adımları listelemek için adımları atamalarını abonelik veya yönetim grubu kapsamda reddet.

1. Azure portalında **tüm hizmetleri** ardından **Yönetim grupları** veya **abonelikleri**.

1. Yönetim grubu veya listelemek istediğiniz aboneliğe tıklayın.

1. Tıklayın **erişim denetimi (IAM)** .

1. Tıklayın **atamaları Reddet** sekme (veya **görünümü** Reddet düğmesi görünümünde atamaları kutucuğuna).

    Varsa tüm atamalarını bu kapsamda reddetme ya da bu kapsama devralınan, bunlar listelenir.

    ![Erişim denetimi - atamalar sekmesinde Reddet](./media/deny-assignments-portal/access-control-deny-assignments.png)

1. Ek sütunları görüntülemek için tıklayın **sütunları Düzenle**.

    ![Sütunları atamaları - Reddet](./media/deny-assignments-portal/deny-assignments-columns.png)

    |  |  |
    | --- | --- |
    | **Ad** | Reddetme atama adı. |
    | **Sorumlu türü** | Kullanıcı, Grup, sistem tarafından tanımlanan bir grup veya hizmet sorumlusu. |
    | **Reddedildi**  | Reddetme atamasını dahil güvenlik sorumlusunun adı. |
    | **Kimlik** | Reddetme atama için benzersiz tanımlayıcı. |
    | **Hariç tutulan ilkeleri** | Reddetme atamadan hariç tutulan güvenlik sorumluları olup olmadığı. |
    | **Alt öğeler için geçerli değildir** | Reddetme atama için subscopes olup devralındı. |
    | **Korumalı sistemi** | İster reddetme atama Azure tarafından yönetilir. Şu anda, her zaman Evet. |
    | **Kapsam** | Yönetim grubu, abonelik, kaynak grubu veya kaynak. |

1. Herhangi bir etkin öğeyi bir onay işareti ekleyin ve ardından **Tamam** Seçili sütunları görüntülemek için.

## <a name="list-details-about-a-deny-assignment"></a>Liste ayrıntıları Reddet ataması hakkında

Bir reddetme ataması hakkındaki ek ayrıntıları listelemek için aşağıdaki adımları izleyin.

1. Açık **atamaları Reddet** bölmesinde önceki bölümde açıklandığı gibi.

1. Açmak için izin verme atama adına tıklayın **kullanıcılar** dikey penceresi.

    ![Kullanıcı atama - Reddet](./media/deny-assignments-portal/deny-assignment-users.png)

    **Kullanıcılar** dikey penceresinde, aşağıdaki iki bölümü içerir.

    |  |  |
    | --- | --- |
    | **Atama uygulandığı Reddet**  | Güvenlik sorumluları Reddet atama için geçerlidir. |
    | **Atama dışlar Reddet** | Güvenlik sorumluları Reddet atamadan hariç tutulur. |

    **Sistem tarafından tanımlanan asıl** tüm kullanıcıları, grupları, hizmet sorumluları ve yönetilen bir Azure AD dizini kimliklerini temsil eder.

1. Reddedilen izinlerin bir listesi görmek için tıklayın **izinler reddedildi**.

    ![Reddedilen izinleri atama - Reddet](./media/deny-assignments-portal/deny-assignment-denied-permissions.png)

    | Eylem türü | Açıklama |
    | --- | --- |
    | **Eylemler**  | Yönetim işlemlerini reddedildi. |
    | **NotActions** | Yönetim işlemlerini dışında yönetim işlemi reddetti. |
    | **DataActions**  | Veri işlemleri engellendi. |
    | **NotDataActions** | Veri işlemleri dışında veri işlem engellendi. |

    Örneğin, önceki ekran görüntüsünde gösterilen etkili izinler şunlardır:

    - İşlemler işlem tüm depolama alanı dışında veri düzlemi işlemleri engellenir.

1. Bir reddetme ataması özelliklerini görmek için tıklayın **özellikleri**.

    ![Özellikler atama - Reddet](./media/deny-assignments-portal/deny-assignment-properties.png)

    Üzerinde **özellikleri** dikey penceresinde, reddetme atama adı, kimliği, açıklama ve kapsam görebilirsiniz. **Alt öğeleri için geçerli değildir** anahtar gösterir Reddet atama için subscopes devralınır. **Korumalı sistemi** anahtar belirtir olup olmadığını reddet Bu atama, Azure tarafından yönetilir. Şu anda bu, **Evet** tüm durumlarda.

## <a name="next-steps"></a>Sonraki adımlar

* [Anlamak Azure kaynakları için atamaları Reddet](deny-assignments.md)
* [Azure PowerShell kullanarak Azure kaynakları için atamaları izin verilmeyenler listesi](deny-assignments-powershell.md)
