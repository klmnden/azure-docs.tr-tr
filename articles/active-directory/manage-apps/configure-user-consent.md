---
title: Bir uygulamaya - Azure Active Directory kullanıcı onayı yapılandırın | Microsoft Docs
description: Kullanıcılar uygulama izinleri onay şekilde yönetmeyi öğrenin. Yönetici onayı vererek kullanıcı deneyimini kolaylaştırabilirsiniz. Bu yöntemler, Azure Active Directory (Azure AD) kiracınız içindeki tüm son kullanıcılar için geçerlidir.
services: active-directory
author: msmimart
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.topic: conceptual
ms.date: 10/22/2018
ms.author: mimart
ms.reviewer: arvindh
ms.collection: M365-identity-device-management
ms.openlocfilehash: 4951984d05e75b0271cf6592c77c54ad13678994
ms.sourcegitcommit: f811238c0d732deb1f0892fe7a20a26c993bc4fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/29/2019
ms.locfileid: "67476547"
---
# <a name="configure-the-way-end-users-consent-to-an-application-in-azure-active-directory"></a>Son kullanıcılar Azure Active Directory'de bir uygulamaya onay şekilde yapılandırın
Kullanıcılar uygulama izinleri onay şekilde yapılandırmayı öğrenin. Yönetici onayı vererek kullanıcı deneyimini kolaylaştırabilirsiniz. Bu makalede, kullanıcı onayı yapılandırabileceğiniz farklı yollar sunar. Azure Active Directory (Azure AD) kiracınız içindeki tüm son kullanıcılar için yöntemleri uygulayın. 

Uygulamalara yönelik daha fazla bilgi için bkz: [Azure Active Directory onay çerçevesine](../develop/consent-framework.md).

## <a name="prerequisites"></a>Önkoşullar

Yönetici onayı verme genel yönetici, uygulama Yöneticisi veya bir bulut uygulaması Yöneticisi oturum açmanız gerekir.

Uygulamalara erişimi kısıtlamak için kullanıcı atama isteme ve ardından kullanıcıları veya grupları uygulamaya atama gerekir.  Daha fazla bilgi için [kullanıcıları ve grupları atama yöntemleri](methods-for-assigning-users-and-groups.md).

## <a name="grant-admin-consent-to-enterprise-apps-in-the-azure-portal"></a>Azure portalının kurumsal uygulamalar için yönetici izni verme

Kurumsal uygulama için yönetici onayı vermek için:

1. Oturum [Azure portalında](https://portal.azure.com) genel yönetici, uygulama Yöneticisi veya bir bulut uygulaması Yöneticisi olarak.
2. Tıklayın **tüm hizmetleri** sol gezinti menüsünün üstünde. **Azure Active Directory uzantısını** açılır.
3. Filtre Arama kutusuna **"Azure Active Directory"** seçip **Azure Active Directory** öğesi.
4. Gezinti menüden **kurumsal uygulamalar**.
5. Onay için uygulamayı seçin.
6. Seçin **izinleri** ve ardından **yönetici onayı vermek**. Uygulamayı yönetmek oturum açmanız istenir.
7. Uygulama için yönetici onayı vermek için izinleri olan bir hesapla oturum açın. 
8. Uygulama izni vermiş olursunuz.

Bu seçenek, yalnızca uygulama ise çalışır: 

- Kiracınızda kayıtlı veya
- Başka bir Azure AD kiracısına kayıtlı ve en az bir son kullanıcı tarafından onaylı. Son kullanıcı bir uygulamaya onay verdi sonra Azure AD uygulama listeler **Kurumsal uygulamaları** Azure portalında.

## <a name="grant-admin-consent-when-registering-an-app-in-the-azure-portal"></a>Azure portalında bir uygulama kaydı sırasında verme yönetici onayı

Bir uygulama kaydı sırasında yönetici onayı vermek için: 

1. [Azure portalında](https://portal.azure.com) genel yönetici olarak oturum açın.
2. Gidin **uygulama kayıtları** dikey penceresi.
3. Uygulama onayı için seçin.
4. Seçin **API izinleri**.
5. Tıklayın **yönetici onayı vermek**.


## <a name="grant-admin-consent-through-a-url-request"></a>Bir URL isteği aracılığıyla yönetici izni verme

Bir URL isteği aracılığıyla yönetici onayı vermek için:

1. Bir istek oluşturmak *login.microsoftonline.com* , uygulama yapılandırmaları olan ve üzerinde ekleme `&prompt=admin_consent`. 
2. Yönetici kimlik bilgilerinizle oturum sonra uygulamayı tüm kullanıcılar için izin verildi.


## <a name="force-user-consent-through-a-url-request"></a>Kullanıcı onayı bir URL isteği ile zorla

Son kullanıcıların bir uygulama kimlik doğrulaması her zaman onayı istemek için ekleme `&prompt=consent` kimlik doğrulaması istek URL'si.

## <a name="next-steps"></a>Sonraki adımlar

[Onay ve AzureAD uygulamaları ile tümleştirme](../develop/quickstart-v1-integrate-apps-with-azure-ad.md)

[Uygulama onayı ve AzureAD v2.0 için kullanarak yakınsanmış](../develop/active-directory-v2-scopes.md)

[AzureAD StackOverflow](https://stackoverflow.com/questions/tagged/azure-active-directory)
