---
title: Kullanıcı oturum açma işlemlerine Azure Active Directory'de bir kurumsal uygulama için devre dışı bırakma | Microsoft Docs
description: Böylece hiçbir kullanıcılar için Azure Active Directory'de oturum Kurumsal uygulama devre dışı bırakma
services: active-directory
documentationcenter: ''
author: barbkess
manager: mtillman
editor: ''
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/28/2017
ms.author: barbkess
ms.reviewer: asteen
ms.custom: it-pro
ms.openlocfilehash: 86ff34bb0780811663d437a9183d0b688b69073d
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/11/2018
ms.locfileid: "35303921"
---
# <a name="disable-user-sign-ins-for-an-enterprise-app-in-azure-active-directory"></a>Kullanıcı oturum açma işlemlerine Azure Active Directory'de bir kurumsal uygulama için devre dışı bırak
Böylece hiçbir kullanıcılar için Azure Active Directory (Azure AD) oturum Kurumsal uygulama devre dışı bırakmak kolaydır. Kuruluş uygulama yönetmek için uygun izinlere sahip olmalıdır ve dizin için genel yönetici olmanız gerekir.

## <a name="how-do-i-disable-user-sign-ins"></a>Kullanıcı oturum açma işlemleri nasıl devre dışı bırakabilirim?
1. Oturum [Azure portal](https://portal.azure.com) dizini için genel yönetici olan bir hesapla.
2. Seçin **tüm hizmetleri**, girin **Azure Active Directory** metin kutusuna ve ardından **Enter**.
3. Üzerinde **Azure Active Directory** -  ***directoryname*** bölmesi (diğer bir deyişle, Azure AD yönettiğiniz dizin için), seçin **kurumsal uygulamalar**.

    ![Açılış Kurumsal uygulamaları](./media/disable-user-sign-in-portal/open-enterprise-apps.png)
4. Üzerinde **kurumsal uygulamalar** bölmesinde, **tüm uygulamaları**. Yönetebileceğiniz uygulamaların bir listesini görürsünüz.
5. Üzerinde **kurumsal uygulamalar - tüm uygulamaları** bölmesinde, bir uygulama seçin.
6. Üzerinde ***appname*** bölmesi (diğer bir deyişle, başlık seçilen uygulamanın adını), seçin **özellikleri**.

    ![Tüm uygulamalar komutu seçme](./media/disable-user-sign-in-portal/select-app.png)
7. Üzerinde ***appname*** - **özellikleri** bölmesinde, **Hayır** için **kullanıcıların oturum açma için etkinleştirilen?**.
8. Seçin **kaydetmek** komutu.

## <a name="next-steps"></a>Sonraki adımlar
* [My tüm grupları görmek](../active-directory-groups-view-azure-portal.md)
* [Bir kullanıcı veya grup için bir kuruluş uygulama atama](assign-user-or-group-access-portal.md)
* [Bir kullanıcı veya grup ataması Kurumsal uygulamadan Kaldır](remove-user-or-group-access-portal.md)
* [Adını veya bir kuruluş uygulama logosunu değiştirme](change-name-or-logo-portal.md)
