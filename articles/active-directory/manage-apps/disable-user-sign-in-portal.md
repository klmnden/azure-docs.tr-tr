---
title: Kullanıcı oturum açma işlemleri için kurumsal bir uygulamayı Azure Active Directory'de devre dışı bırakma | Microsoft Docs
description: Hiçbir kullanıcı için Azure Active Directory'de oturum böylece kurumsal bir uygulamanın devre dışı bırakma
services: active-directory
documentationcenter: ''
author: msmimart
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 04/12/2019
ms.author: mimart
ms.reviewer: asteen
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 90000f34ff247fdd5939dc19971c170aa4b70386
ms.sourcegitcommit: be9fcaace62709cea55beb49a5bebf4f9701f7c6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/17/2019
ms.locfileid: "65824652"
---
# <a name="disable-user-sign-ins-for-an-enterprise-app-in-azure-active-directory"></a>Kullanıcı oturum açma işlemleri için kurumsal bir uygulamayı Azure Active Directory'de devre dışı bırak
Hiçbir kullanıcı için Azure Active Directory (Azure AD) oturum açabilmeniz kurumsal bir uygulamanın devre dışı bırakmak kolay bir işlemdir. Kurumsal uygulama Yönetme iznine ihtiyacınız var. Ve dizin için genel yönetici olması gerekir.

## <a name="how-do-i-disable-user-sign-ins"></a>Kullanıcı oturum açma işlemleri nasıl devre dışı bırakabilirim?
1. Dizin için genel yönetici olan bir hesapla [Azure portalda](https://portal.azure.com) oturum açın.
1. Seçin **tüm hizmetleri**, girin **Azure Active Directory** metin kutusuna ve ardından **Enter**.
1. Üzerinde **Azure Active Directory** -  ***directoryname*** bölmesi (diğer bir deyişle, Azure AD dizini yönettiğiniz için), seçin **kurumsal uygulamalar**.
1. Üzerinde **kurumsal uygulamalar - tüm uygulamalar** bölmesinde yönetebileceğiniz uygulamaların listesini görürsünüz. Bir uygulama seçin.
1. Üzerinde ***appname*** bölmesi (diğer bir deyişle, başlık seçilen uygulamanın adını), seçin **özellikleri**.
1. Üzerinde ***appname*** - **özellikleri** bölmesinde **Hayır** için **kullanıcıların oturum açması etkinleştirildi mi?**.
1. Seçin **Kaydet** komutu.

## <a name="next-steps"></a>Sonraki adımlar
* [Tüm Gruplarım bakın](../fundamentals/active-directory-groups-view-azure-portal.md)
* [Kurumsal bir uygulamayı kullanıcı veya grup atama](assign-user-or-group-access-portal.md)
* [Bir kullanıcı veya grup ataması Kurumsal uygulamadan Kaldır](remove-user-or-group-access-portal.md)
* [Adını veya kurumsal bir uygulamanın logoyu değiştirme](change-name-or-logo-portal.md)
