---
title: "Uygulamalar için atama kullanıcılara nasıl | Microsoft Docs"
description: "Uygulamaya kiracınızda kullanıcılara nasıl atanacağını anlama"
services: active-directory
documentationcenter: 
author: ajamess
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: asteen
ms.openlocfilehash: 916238ba402a2555bac620d7f08e02799d981ae0
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/03/2017
---
# <a name="how-to-assign-users-to-applications"></a>Kullanıcılar uygulamalara atama

Bu makalede uygulamaya kiracınızda kullanıcılara nasıl atanacağını anlamanıza yardımcı olur.

## <a name="how-do-users-get-assigned-to-an-application-in-azure-ad"></a>Nasıl kullanıcılar Azure AD'de bir uygulamaya atanır?

Bir kullanıcı bir uygulamaya erişmek öncelikle ona herhangi bir yolla atanmaları gerekir. Atama bir yönetici, bir iş temsilci veya kullanıcı tarafından bazı durumlarda, kendilerini gerçekleştirilebilir. Aşağıda kullanıcılar uygulamalarına atanan yolları açıklanır:

1.  Bir yönetici [kullanıcı atar](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal) uygulamaya doğrudan

2.  Bir yönetici [bir grup atar](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal) kullanıcının uygulamaya bir üyesi olduğunu da dahil olmak üzere:

  * Şirket içi eşitlendiği bir grup

  * Bulutta oluşturulan bir statik güvenlik grubu

  * A [dinamik güvenlik grubu](https://docs.microsoft.com/azure/active-directory/active-directory-groups-dynamic-membership-azure-portal) bulutta oluşturulan

  * Bulutta oluşturulan bir Office 365 Grup

  * [Tüm kullanıcılar](https://docs.microsoft.com/azure/active-directory/active-directory-accessmanagement-dedicated-groups) grubu

3.  Yöneticinin paylaşılan [Self Servis uygulamaya erişim](https://docs.microsoft.com/azure/active-directory/active-directory-self-service-application-access) kullanarak bir uygulama eklemek bir kullanıcı izin vermek için [uygulama erişim Paneli'ne](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) **uygulama Ekle** özelliği **iş onayı olmadan**

4.  Yöneticinin paylaşılan [Self Servis uygulamaya erişim](https://docs.microsoft.com/azure/active-directory/active-directory-self-service-application-access) kullanarak bir uygulama eklemek bir kullanıcı izin vermek için [uygulama erişim Paneli'ne](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) **uygulama Ekle** özellik, ancak yalnızca w**i önceki onay iş onaylayanlar seçili kümesinden**

5.  Yöneticinin paylaşılan [Self Servis Grup Yönetimi](https://docs.microsoft.com/azure/active-directory/active-directory-accessmanagement-self-service-group-management) uygulamanın atandığı bir gruba katılması bir kullanıcı izin vermek için **iş onayı olmadan**

6.  Yöneticinin paylaşılan [Self Servis Grup Yönetimi](https://docs.microsoft.com/azure/active-directory/active-directory-accessmanagement-self-service-group-management) bir kullanıcı bir uygulama için ancak yalnızca atanmış bir gruba katılmak izin vermek için **iş onaylayanlar seçili kümesinden önceki onay ile**

7.  Yönetici bir lisans doğrudan birinci taraf uygulama için bir kullanıcı gibi atar [Microsoft Office 365](http://products.office.com/)

8.  Bir yönetici, kullanıcı gibi birinci taraf uygulama için bir üyesi olan bir gruba bir lisans atar [Microsoft Office 365](http://products.office.com/)

9.  Bir [yönetici uygulamaya izin](https://docs.microsoft.com/azure/active-directory/develop/active-directory-devhowto-multi-tenant-overview#understanding-user-and-admin-consent) tüm kullanıcılar ve sonra bir kullanıcı tarafından kullanılmak üzere oturum açtığında uygulamaya

10. Bir kullanıcı [izin uygulamaya](https://docs.microsoft.com/azure/active-directory/develop/active-directory-devhowto-multi-tenant-overview#understanding-user-and-admin-consent) tek başına uygulamayı oturum açma

## <a name="next-steps"></a>Sonraki adımlar
[Azure Active Directory ile uygulamaları yönetme](active-directory-enable-sso-scenario.md)
