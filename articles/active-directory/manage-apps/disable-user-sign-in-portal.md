---
title: Kullanıcı oturum açma işlemleri için kurumsal bir uygulamayı Azure Active Directory'de devre dışı bırakma | Microsoft Docs
description: Hiçbir kullanıcı için Azure Active Directory'de oturum böylece kurumsal bir uygulamanın devre dışı bırakma
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 08/28/2017
ms.author: celested
ms.reviewer: asteen
ms.custom: it-pro
ms.openlocfilehash: 00953911aa1dd2ffe05b78e5b63c45d67719193f
ms.sourcegitcommit: d1c5b4d9a5ccfa2c9a9f4ae5f078ef8c1c04a3b4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/08/2019
ms.locfileid: "55963004"
---
# <a name="disable-user-sign-ins-for-an-enterprise-app-in-azure-active-directory"></a>Kullanıcı oturum açma işlemleri için kurumsal bir uygulamayı Azure Active Directory'de devre dışı bırak
Hiçbir kullanıcı için Azure Active Directory (Azure AD) oturum, böylece kurumsal bir uygulamanın devre dışı bırakmak kolay bir işlemdir. Kurumsal uygulamasını yönetmek için uygun izinlere sahip olmalıdır ve dizin için genel yönetici olması gerekir.

## <a name="how-do-i-disable-user-sign-ins"></a>Kullanıcı oturum açma işlemleri nasıl devre dışı bırakabilirim?
1. Dizin için genel yönetici olan bir hesapla [Azure portalda](https://portal.azure.com) oturum açın.
2. Seçin **tüm hizmetleri**, girin **Azure Active Directory** metin kutusuna ve ardından **Enter**.
3. Üzerinde **Azure Active Directory** -  ***directoryname*** bölmesi (diğer bir deyişle, Azure AD dizini yönettiğiniz için), seçin **kurumsal uygulamalar**.

    ![Açılış kurumsal uygulamalar](./media/disable-user-sign-in-portal/open-enterprise-apps.png)
4. Üzerinde **kurumsal uygulamalar** bölmesinde **tüm uygulamaları**. Yönetebileceğiniz uygulamaların listesini görürsünüz.
5. Üzerinde **kurumsal uygulamalar - tüm uygulamalar** bölmesinde bir uygulama seçin.
6. Üzerinde ***appname*** bölmesi (diğer bir deyişle, başlık seçilen uygulamanın adını), seçin **özellikleri**.

    ![Tüm uygulamaları komutu seçme](./media/disable-user-sign-in-portal/select-app.png)
7. Üzerinde ***appname*** - **özellikleri** bölmesinde **Hayır** için **kullanıcıların oturum açması etkinleştirildi mi?**.
8. Seçin **Kaydet** komutu.

## <a name="next-steps"></a>Sonraki adımlar
* [Tüm Gruplarım bakın](../fundamentals/active-directory-groups-view-azure-portal.md)
* [Kurumsal bir uygulamayı kullanıcı veya grup atama](assign-user-or-group-access-portal.md)
* [Bir kullanıcı veya grup ataması Kurumsal uygulamadan Kaldır](remove-user-or-group-access-portal.md)
* [Adını veya kurumsal bir uygulamanın logoyu değiştirme](change-name-or-logo-portal.md)
