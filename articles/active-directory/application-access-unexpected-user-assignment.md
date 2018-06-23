---
title: Uygulamalar için atama kullanıcılara nasıl | Microsoft Docs
description: Uygulamaya kiracınızda kullanıcılara nasıl atanacağını anlama
services: active-directory
documentationcenter: ''
author: barbkess
manager: mtillman
ms.assetid: ''
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: barbkess
ms.openlocfilehash: 0c6755bc8990c75c696c6df7e95132f2dd97d313
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "36335171"
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

4.  Yöneticinin paylaşılan [Self Servis uygulamaya erişim](https://docs.microsoft.com/azure/active-directory/active-directory-self-service-application-access) kullanarak bir uygulama eklemek bir kullanıcı izin vermek için [uygulama erişim Paneli'ne](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) **uygulama Ekle** özellik, ancak yalnızca w**iş onaylayanlar seçili kümesinden i önceki onayı**

5.  Yöneticinin paylaşılan [Self Servis Grup Yönetimi](https://docs.microsoft.com/azure/active-directory/active-directory-accessmanagement-self-service-group-management) uygulamanın atandığı bir gruba katılması bir kullanıcı izin vermek için **iş onayı olmadan**

6.  Yöneticinin paylaşılan [Self Servis Grup Yönetimi](https://docs.microsoft.com/azure/active-directory/active-directory-accessmanagement-self-service-group-management) bir kullanıcı bir uygulama için ancak yalnızca atanmış bir gruba katılmak izin vermek için **iş onaylayanlar seçili kümesinden önceki onay ile**

7.  Yönetici bir lisans doğrudan birinci taraf uygulama için bir kullanıcı gibi atar [Microsoft Office 365](http://products.office.com/)

8.  Bir yönetici, kullanıcı gibi birinci taraf uygulama için bir üyesi olan bir gruba bir lisans atar [Microsoft Office 365](http://products.office.com/)

9.  Bir [yönetici uygulamaya izin](https://docs.microsoft.com/azure/active-directory/develop/active-directory-devhowto-multi-tenant-overview#understanding-user-and-admin-consent) tüm kullanıcılar ve sonra bir kullanıcı tarafından kullanılmak üzere oturum açtığında uygulamaya

10. Bir kullanıcı [izin uygulamaya](https://docs.microsoft.com/azure/active-directory/develop/active-directory-devhowto-multi-tenant-overview#understanding-user-and-admin-consent) tek başına uygulamayı oturum açma

## <a name="next-steps"></a>Sonraki adımlar
[Uygulamaları Azure Active Directory ile yönetme](manage-apps/what-is-application-management.md)
