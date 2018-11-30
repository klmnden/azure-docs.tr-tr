---
title: Görünüm Reddet Azure portalını kullanarak atamaları | Microsoft Docs
description: Kullanıcılar, gruplar, hizmet sorumluları ve Azure portalını kullanarak belirli kapsamda belirli eylemler için erişim reddedildi yönetilen kimlikleri görüntülemeyi öğrenin.
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
ms.date: 11/30/2018
ms.author: rolyon
ms.reviewer: bagovind
ms.openlocfilehash: f5870ddbbb8be0ebbeae7656485521a327b86d5b
ms.sourcegitcommit: c8088371d1786d016f785c437a7b4f9c64e57af0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/30/2018
ms.locfileid: "52642812"
---
# <a name="view-deny-assignments-using-the-azure-portal"></a>Azure portalını kullanarak atamaları görünümü Reddet

[Atamalar Reddet](deny-assignments.md) bir rol ataması bunları erişim verse bile kullanıcıların belirli eylemleri gerçekleştirme. Kendi oluşturulamıyor olsa bile atamaları, reddetme görüntülemeye yine genel izinlerinizi etkileyebileceğinden atamaları reddet. Reddetme ataması hakkında bilgi almak için, [yerleşik rollerin](built-in-roles.md) çoğuna eklenmiş olan `Microsoft.Authorization/denyAssignments/read` izninizin olması gerekir.

Bu makalede görüntülemek için Azure portalını kullanmayı açıklar atamaları reddet.

> [!NOTE]
> Şu anda izin verme atamalar salt okunurdur ve yalnızca Azure tarafından ayarlanabilir.

## <a name="view-deny-assignments"></a>Reddetme atamalarını görüntüleyin

Reddetme atamalarını abonelik veya yönetim grubu kapsamda görüntülemek üzere şu adımları izleyin.

1. Azure portalında **tüm hizmetleri** ardından **Yönetim grupları** veya **abonelikleri**.

1. Yönetim grubu veya görüntülemek istediğiniz aboneliğe tıklayın.

1. Tıklayın **erişim denetimi (IAM)**.

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

## <a name="view-details-about-a-deny-assignment"></a>Bir reddetme ataması ayrıntılarını görüntüleme

Bir reddetme ataması hakkında ek ayrıntıları görüntülemek için aşağıdaki adımları izleyin.

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

* [Anlamak atamaları Reddet](deny-assignments.md)
* [RBAC ve REST API kullanarak atamaları izin verilmeyenler listesi](deny-assignments-rest.md)
