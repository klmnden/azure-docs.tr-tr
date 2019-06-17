---
title: Uygulamaları atama kullanıcılara nasıl | Microsoft Docs
description: Kullanıcılar kiracınızdaki bir uygulamaya nasıl atanacağını ' ı Anlama
services: active-directory
documentationcenter: ''
author: msmimart
manager: CelesteDG
ms.assetid: ''
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: mimart
ms.collection: M365-identity-device-management
ms.openlocfilehash: b818fe1d8b6bbc9d2d8c5b460b4d71dccdd39366
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65825983"
---
# <a name="how-to-assign-users-to-applications"></a>Kullanıcılar uygulamaları atama

Bu makalede kullanıcıların kiracınızdaki bir uygulamaya nasıl atanacağını anlamanıza yardımcı olur.

## <a name="how-do-users-get-assigned-to-an-application-in-azure-ad"></a>Nasıl kullanıcıların Azure AD'de bir uygulama için atanır?

Bir kullanıcı bir uygulamaya erişmek, bunlar ilk için herhangi bir şekilde atanması gerekir. Atama bir yönetici, bir iş temsilci veya kullanıcı tarafından bazen kendileri gerçekleştirilebilir. Aşağıdaki kullanıcıların uygulamalarına atanan yolları açıklanır:

1.  Yönetici [bir kullanıcıya atar](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal) uygulamaya doğrudan

2.  Yönetici [bir grup atar](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal) kullanıcının uygulamaya, üyesi olduğunu da dahil olmak üzere:

    * Şirket içi ad'nizden eşitlenmiş bir grup

    * Bulutta oluşturulan bir statik güvenlik grubu

    * A [dinamik güvenlik grubu](https://docs.microsoft.com/azure/active-directory/active-directory-groups-dynamic-membership-azure-portal) bulutta oluşturulan

    * Bulutta oluşturulan bir Office 365 grubu

    * [Tüm kullanıcılar](https://docs.microsoft.com/azure/active-directory/active-directory-accessmanagement-dedicated-groups) grubu

3.  Yöneticinin sağlayan [Self Servis uygulama erişimini](https://docs.microsoft.com/azure/active-directory/active-directory-self-service-application-access) kullanarak bir uygulama eklemek bir kullanıcı izin vermek için [uygulama erişim panelinde](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) **uygulama Ekle** özellik **işletme onayı olmadan**

4.  Yöneticinin sağlayan [Self Servis uygulama erişimini](https://docs.microsoft.com/azure/active-directory/active-directory-self-service-application-access) kullanarak bir uygulama eklemek bir kullanıcı izin vermek için [uygulama erişim panelinde](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) **uygulama Ekle** özellik, ancak yalnızca w**i. önceki onay Seçili iş onaylayanlar kümesi**

5.  Yöneticinin sağlayan [Self Servis Grup Yönetimi](https://docs.microsoft.com/azure/active-directory/active-directory-accessmanagement-self-service-group-management) uygulamaya atanmış bir gruba katılma olanağı sağlamak için **işletme onayı olmadan**

6.  Yöneticinin sağlayan [Self Servis Grup Yönetimi](https://docs.microsoft.com/azure/active-directory/active-directory-accessmanagement-self-service-group-management) bir kullanıcı, bir uygulama için ancak yalnızca atandığı bir gruba katılma izni **önceki onayını seçili bir iş onaylayanlar kümesi ile**

7.  Yönetici bir lisans doğrudan bir birinci taraf uygulaması için bir kullanıcı gibi atar [Microsoft Office 365](https://products.office.com/)

8.  Bir yönetici kullanıcı gibi bir birinci taraf uygulaması için bir üyesi olan bir lisans grubuna atar [Microsoft Office 365](https://products.office.com/)

9.  Bir [yönetici uygulamaya onay](https://docs.microsoft.com/azure/active-directory/develop/active-directory-devhowto-multi-tenant-overview) tüm kullanıcılar ve ardından bir kullanıcı tarafından kullanılacak oturum açtığında uygulamaya

10. Bir kullanıcı [uygulamaya onay](https://docs.microsoft.com/azure/active-directory/develop/active-directory-devhowto-multi-tenant-overview) başlarına uygulamada oturum açarken

## <a name="next-steps"></a>Sonraki adımlar
[Uygulamaları Azure Active Directory ile yönetme](what-is-application-management.md)
