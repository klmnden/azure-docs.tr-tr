---
title: "Kullanıcı oturum açma işlemlerine Azure Active Directory'de bir kurumsal uygulama için devre dışı bırakma | Microsoft Docs"
description: "Böylece hiçbir kullanıcılar için Azure Active Directory'de oturum Kurumsal uygulama devre dışı bırakma"
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: a27562f9-18dc-42e8-9fee-5419566f8fd7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/28/2017
ms.author: curtand
ms.reviewer: asteen
ms.custom: it-pro
ms.openlocfilehash: 4f587eb96aaa41ba2034eb9eaeb904f72c501abe
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="disable-user-sign-ins-for-an-enterprise-app-in-azure-active-directory"></a>Kullanıcı oturum açma işlemlerine Azure Active Directory'de bir kurumsal uygulama için devre dışı bırak
Böylece hiçbir kullanıcılar için Azure Active Directory (Azure AD) oturum Kurumsal uygulama devre dışı bırakmak kolaydır. Kuruluş uygulama yönetmek için uygun izinlere sahip olmalıdır ve dizin için genel yönetici olmanız gerekir.

## <a name="how-do-i-disable-user-sign-ins"></a>Kullanıcı oturum açma işlemleri nasıl devre dışı bırakabilirim?
1. Oturum [Azure portal](https://portal.azure.com) dizini için genel yönetici olan bir hesapla.
2. Seçin **daha fazla hizmet**, girin **Azure Active Directory** metin kutusuna ve ardından **Enter**.
3. Üzerinde **Azure Active Directory** -  ***directoryname*** (diğer bir deyişle, Azure AD dikey yönettiğiniz dizin için), dikey penceresinde seçin **kurumsal uygulamalar**.

    ![Açılış Kurumsal uygulamaları](./media/active-directory-coreapps-disable-app-azure-portal/open-enterprise-apps.png)
4. Üzerinde **kurumsal uygulamalar** dikey penceresinde, select **tüm uygulamaları**. Yönetebileceğiniz uygulamaların bir listesini görürsünüz.
5. Üzerinde **kurumsal uygulamalar - tüm uygulamaları** dikey penceresinde, bir uygulama seçin.
6. Üzerinde ***appname*** dikey penceresinde (diğer bir deyişle, dikey penceresinde seçili uygulamasının başlık adı ile) seçin **özellikleri**.

    ![Tüm uygulamalar komutu seçme](./media/active-directory-coreapps-disable-app-azure-portal/select-app.png)
7. Üzerinde ***appname*** - **özellikleri** dikey penceresinde, select **Hayır** için **kullanıcıların oturum açma için etkinleştirilen?**.
8. Seçin **kaydetmek** komutu.

## <a name="next-steps"></a>Sonraki adımlar
* [My tüm grupları görmek](active-directory-groups-view-azure-portal.md)
* [Bir kullanıcı veya grup için bir kuruluş uygulama atama](active-directory-coreapps-assign-user-azure-portal.md)
* [Bir kullanıcı veya grup ataması Kurumsal uygulamadan Kaldır](active-directory-coreapps-remove-assignment-azure-portal.md)
* [Adını veya bir kuruluş uygulama logosunu değiştirme](active-directory-coreapps-change-app-logo-user-azure-portal.md)
