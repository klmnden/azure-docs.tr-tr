---
title: Azure Active Directory'de tek ve çok kiracılı uygulamalar
description: Azure AD'de tek kiracılı ve çok kiracılı uygulamaları arasındaki farklar ve özellikleri hakkında bilgi edinin.
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/24/2018
ms.author: celested
ms.reviewer: justhu
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 57ec1939dd1f5445583200776e489f7596f572f0
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60250315"
---
# <a name="tenancy-in-azure-active-directory"></a>Azure Active Directory'de kiralama

Azure Active Directory (Azure AD), kullanıcılar ve uygulamalar gibi nesneleri gruplar halinde düzenler adlı *kiracılar*. Kiracılar, kuruluş ve bunların güvenliği ve işletimsel ilkeleri karşılamak için kuruluşunuzun sahip uygulamalar içindeki kullanıcılar şirket ilkeleri ayarlamak bir yönetici izin verin. 

## <a name="who-can-sign-in-to-your-app"></a>Kimin uygulamanıza oturum?

Uygulamaları geliştirmeye geldiğinde, geliştiricilerin uygulamalarını tek kiracılı ya da çok kiracılı uygulama kaydı sırasında olması için yapılandırmayı seçebilirsiniz [Azure portalında](https://portal.azure.com).
* Tek kiracılı uygulamalar yalnızca kendi ana Kiracı olarak da bilinir, kayıtlı olan bir kiracı kullanılabilir.
* Çok kiracılı uygulamalar, kullanıcılar kendi giriş kiracısında hem de diğer kiracılar için kullanılabilir.

Azure portalında, İzleyici şu şekilde ayarlayarak tek kiracılı veya çok kiracılı uygulamanızı yapılandırabilirsiniz.

| Hedef kitle | Tek/birden çok-tenant | Kim oturum | 
|----------|--------| ---------|
| Yalnızca bu dizindeki hesapları | Tek kiracılı | Dizininizdeki tüm kullanıcı ve konuk hesapları uygulamanızı veya API'nizi kullanabilir.<br>*Hedef kitlenizi kuruluşunuz, bu seçeneği kullanın.* |
| Herhangi bir Azure AD dizinindeki hesaplar | Çok Kiracılı | Tüm kullanıcılar ve konuklar bir iş veya Okul hesabı Microsoft ile uygulamanız veya API kullanabilirsiniz. Bu, okullar ve Office 365 kullanan işletmelerin içerir.<br>*Hedef kitlenizi iş veya eğitim müşterileri, bu seçeneği kullanın.* |
| Herhangi bir Azure AD dizini ve kişisel Microsoft hesapları (örneğin, Skype, Xbox, Outlook.com) hesapları | Çok Kiracılı | Bir iş veya okul veya kişisel Microsoft hesabı olan tüm kullanıcılar, uygulama veya API kullanabilirsiniz. Okullar ve Xbox ve Skype gibi hizmetlerinde oturum açarken kullanılacak kişisel hesaplarının yanı sıra Office 365 kullanan işletmelerin de buna dahildir.<br>*Microsoft hesapları geniş kümesini hedeflemek için bu seçeneği kullanın.* | 

## <a name="best-practices-for-multi-tenant-apps"></a>Çok kiracılı uygulamalar için en iyi uygulamalar

Çok kiracılı harika uygulamalar geliştirmeye BT yöneticileri, kiracıda ayarlayabilirsiniz farklı ilke sayısına nedeniyle zor olabilir. Çok kiracılı bir uygulama oluşturmak tercih ederseniz, bu en iyi uygulamaları izleyin:

* Yapılandırılmış bir kiracıya uygulamanızı test [koşullu erişim ilkeleri](conditional-access-dev-guide.md).
* Uygulamanızı yalnızca gerçekten gerekli izinleri isteyen emin olmak için en az kullanıcı erişim ilkesini izleyin. Bazı kuruluşlar da uygulamanızı almalarını bu kullanıcıları engelleyebilir olarak yönetici onayı gerektiren izinler isteyen kaçının. 
* Uygun adları ve açıklamaları, uygulamanızın bir parçası olarak kullanıma tüm izinleri sağlar. Bu, kullanıcıların yardımcı olur ve uygulamanızın API'lerini kullanmayı denediğinizde ne oldukları kabul etmiş olursunuz yöneticileri bildirin. Daha fazla bilgi için en iyi yöntemler kısmına bakın [izinleri Kılavuzu](v1-permissions-and-consent.md).

## <a name="next-steps"></a>Sonraki adımlar

* [Uygulamanın çok kiracılı dönüştürme](howto-convert-app-to-be-multi-tenant.md)
