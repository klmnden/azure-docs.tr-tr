---
title: Raporları görüntüleme ve Azure AD hak yönetimi (Önizleme) - Azure Active Directory günlükleri
description: Kullanıcı atamalarını raporu görüntülemek ve Denetim günlükleri, Azure Active Directory hak yönetimi (Önizleme) hakkında bilgi edinin.
services: active-directory
documentationCenter: ''
author: rolyon
manager: mtillman
editor: jocastel-MSFT
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.subservice: compliance
ms.date: 04/19/2019
ms.author: rolyon
ms.reviewer: jocastel
ms.collection: M365-identity-device-management
ms.openlocfilehash: 60a61a581574c77a57939ea23fdadc7b060b82af
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "64541548"
---
# <a name="view-reports-and-logs-in-azure-ad-entitlement-management-preview"></a>Raporları görüntüleme ve günlükleri Azure AD hak yönetimi (Önizleme)

> [!IMPORTANT]
> Azure Active Directory (Azure AD) Yetkilendirme Yönetimi, şu anda genel Önizleme aşamasındadır.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir.
> Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="view-resources-a-user-has-access-to"></a>Bir kullanıcının erişebileceği kaynakları görüntüle

1. Tıklayın **Azure Active Directory** ve ardından **Kimlik Yönetimi**.

1. Sol menüde **kullanıcı atamaları raporu**.

1. Tıklayın **Seçili kullanıcılar** belirli kullanıcılar bölmesini açmak için.

1. Listede erişime sahip oldukları kaynakları görüntülemek istediğiniz kullanıcıyı bulun.

1. Kullanıcı'yı tıklatın ve ardından **seçin**.

    Kullanıcının erişimi olan kaynakların listesi görüntülenir. Bu erişim paketi, ilke ve tarihleri içerir.

    ![Kullanıcı atamalarını raporu](./media/entitlement-management-reports/user-assignments-report.png)

## <a name="determine-the-status-of-a-users-request"></a>Bir kullanıcı isteğinin durumunu belirleme

Kullanıcının istenen ve bir erişim paketine alınan hakkında ek ayrıntıları almak için Azure AD denetim günlüğünde kullanabilirsiniz. Özellikle, günlük kayıtları kullanabileceğiniz `EntitlementManagement` ve `UserManagement` her istek için işleme adımları ek ayrıntıları almak için kategoriler.  

1. Tıklayın **Azure Active Directory** ve ardından **denetim günlükleri**.

1. Sayfanın üstünde değiştirme **kategori** ya da `EntitlementManagement` veya `UserManagement`aradığınız denetim kaydını bağlı olarak.  

1. **Uygula**'ya tıklayın.

1. Günlükleri indirmek için tıklayın **indirme**.

Azure AD, yeni bir istek aldığında, bir denetim kaydı, Yazar **kategori** olduğu `EntitlementManagement` ve **etkinlik** genellikle `User requests access package assignment`.  Azure portalında oluşturulan bir doğrudan atama söz konusu olduğunda **etkinlik** Denetim kaydının alan `Administrator directly assigns user to access package`, ve atama gerçekleştiren kullanıcı tarafından tanımlanan **ActorUserPrincipalName**.

Azure AD, istek, devam ederken ek denetim kayıtlarını yazacak dahil olmak üzere:

| Kategori | Etkinlik | İstek durumu |
| :---- | :------------ | :------------ |
| `EntitlementManagement` | `Auto approve access package assignment request` | İstek onay gerektirmez |
| `UserManagement` | `Create request approval` | İstek onayı gerekir |
| `UserManagement` | `Add approver to request approval` | İstek onayı gerekir |
| `EntitlementManagement` | `Approve access package assignment request` | İstek Onaylandı |
| `EntitlementManagement` | `Ready to fulfill access package assignment request` |İstek Onaylandı veya onay gerektirmez |

Bir kullanıcının erişim atandığında, Azure AD için bir denetim kaydı yazar `EntitlementManagement` kategorisiyle **etkinlik** `Fulfill access package assignment`.  Erişim alınan kullanıcı tarafından tanımlanan **ActorUserPrincipalName** alan.

Erişim yok atanma sonra Azure AD için bir denetim kaydı yazar `EntitlementManagement` kategorisiyle **etkinlik** ya da `Deny access package assignment request`, istek bir onaylayan tarafından reddedildiğinde veya `Access package assignment request timed out (no approver action taken)`, önce istek zaman aşımına uğrarsa bir Onaylayan onaylarken.

Kullanıcının erişim paket atamasını süresi dolduğunda, kullanıcı tarafından iptal edildi veya Azure AD için bir denetim kaydı yazar sonra yönetici tarafından kaldırıldı `EntitlementManagement` kategorisiyle **etkinlik** , `Remove access package assignment`.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure AD hak yönetimi sorunlarını giderme](entitlement-management-troubleshoot.md)
- [Yaygın senaryolar](entitlement-management-scenarios.md)
