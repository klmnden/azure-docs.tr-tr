---
title: Adını veya bir Azure Active Directory'de Kurumsal uygulama logosunu değiştirme | Microsoft Docs
description: Adı veya özel kuruluş uygulaması Azure Active Directory'de logosunu değiştirme
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
ms.date: 04/05/2019
ms.author: mimart
ms.reviewer: asteen
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 30da8d6843c27c42d4d99adef50b9ad98a131c95
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2019
ms.locfileid: "65780915"
---
# <a name="change-the-name-or-logo-of-an-enterprise-application-in-azure-active-directory"></a>Adı veya Azure Active Directory'de bir kurumsal uygulamanın logoyu değiştirme

Adı veya logosu özel kuruluş uygulaması Azure Active Directory'de (Azure AD) değiştirmek kolay bir işlemdir. Bu değişiklikleri yapmak için uygun izinlere sahip olmalıdır ve özel uygulama oluşturucusu olmalıdır.

## <a name="how-do-i-change-an-enterprise-applications-name-or-logo"></a>Bir kurumsal uygulamanın adı veya logosu nasıl değiştirebilirim?

1. Oturum [Azure Active Directory portalında](https://aad.portal.azure.com/) dizin için genel yönetici olan bir hesapla. **Azure Active Directory Yönetim Merkezi** sayfası görüntülenir.
2. Sol bölmede **Kurumsal uygulamalar**’ı seçin. Kurumsal uygulamalarınızı listesi görüntülenir.
3. Bir uygulama seçin. Uygulama genel bakış sayfası görüntülenir.
4. Uygulama genel bakış bölmesinde altında **Yönet** başlığı seçin **özellikleri**. **Özellikleri** sayfası görüntülenir.
5. Adı değiştirmek istiyorsanız, seçin **adı** kutusunda, yeni adı yazın ve ENTER tuşuna **Enter**.
6. Logo değiştirmek istiyorsanız, bulma **logosu** alan ve klasör simgesini seçin **bir dosya seçin** uygulamanın geçerli logo resmi olan kutusu.

   ![Özellikler komutu seçme](./media/change-name-or-logo-portal/change-logo.png)

   Logo değişiyorsa değil, aksi takdirde 8. adımına geçin.
7. Dosya Seçici'de yeni logo olarak istediğiniz dosyayı seçin. Dosyanın adı, geçerli logo resmi altındaki kutuya görünür.

   > [!NOTE]
   > Azure logosu resmi bir PNG dosyası gerektirir ve genişlik, yükseklik ve dosya boyutu sınırları geçerlidir.
8. **Kaydet**’i seçin. Yeni bir logo seçerseniz **logosu** yeni logo dosyası yansıtacak şekilde alanın görüntü değişiklikler.

## <a name="next-steps"></a>Sonraki adımlar

* [Hızlı Başlangıç: Kuruluşunuzun gruplar ve üyeler, Azure Active Directory'de görüntüle](../fundamentals/active-directory-groups-view-azure-portal.md)
* [Kurumsal bir uygulamayı kullanıcı veya grup atama](assign-user-or-group-access-portal.md)
* [Bir kullanıcı veya grup ataması Kurumsal uygulamadan Kaldır](remove-user-or-group-access-portal.md)
* [Kullanıcı oturum açma Kurumsal uygulama için devre dışı bırak](disable-user-sign-in-portal.md)
