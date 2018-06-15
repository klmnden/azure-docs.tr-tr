---
title: Azure Active Directory'ye kayıtlı cihazları | Microsoft Docs
description: Azure Active Directory'ye kayıtlı cihazları öğrenin.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: mtillman
editor: ''
ms.assetid: 54e1b01b-03ee-4c46-bcf0-e01affc0419d
ms.service: active-directory
ms.component: devices
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/15/2018
ms.author: markvi
ms.reviewer: jairoc
ms.openlocfilehash: 541f37bb11c604567b356716db282d0e4e2f629b
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2018
ms.locfileid: "34713533"
---
# <a name="set-up-azure-active-directory-registered-windows-10-devices"></a>Azure Active Directory ayarlama kayıtlı Windows 10 cihazları

Azure Active Directory'de (Azure AD) ile cihaz yönetimi, güvenlik ve uyumluluğa yönelik standartlarınızı karşılamak aygıtlardan kullanıcılarınızın kaynaklarınızı eriştiğiniz emin olabilirsiniz. Daha fazla bilgi için bkz: [Azure Active Directory'de cihaz yönetimine giriş](device-management-introduction.md).

Etkinleştirmek istiyorsanız, **kendi cihazını getir (KCG)** senaryosunda, gerçekleştirmek bu Azure AD kayıtlı cihazları yapılandırarak. Azure AD'de kayıtlı Azure AD cihazlar Windows 10, iOS, Android ve macOS için yapılandırabilirsiniz. Bu makalede Windows 10 cihazlar için ile ilgili adımları sağlar. 


## <a name="before-you-begin"></a>Başlamadan önce

Bir Windows 10 cihazı kaydetmek için cihaz Kayıt Hizmeti'ni aygıtlarını kaydetmesini sağlamak için yapılandırılmalıdır. Ayrıca, daha az aygıtları yapılandırılmış en büyük değerden daha kayıtlı olması gerekir. Daha fazla bilgi için bkz: [aygıt ayarlarını yapılandır](device-management-azure-portal.md#configure-device-settings).

## <a name="what-you-should-know"></a>Bilmeniz gerekenler

Bir cihaz kaydedilirken, aşağıdakileri göz önünde bulundurmalıdır:

- Windows aygıt kuruluşunuzun dizininde Azure AD'de kaydeder

- Çok faktörlü kimlik doğrulaması sınaması gitmesi gerekli. Bu sorunu, BT yöneticiniz tarafından yapılandırılabilir.

- Azure AD cihaz mobil cihaz Yönetimi kayıt gerektirir ve uygulanabilirse kaydettiği olup olmadığını denetler.

- Yönetilen bir kullanıcı varsa, Windows, masaüstüne otomatik oturum açma alır.

- Bir Federasyon kullanıcısı varsa, bir Windows ekrana oturum açma kimlik bilgilerinizi girmeniz için alınır.


## <a name="registering-a-device"></a>Bir cihaz kaydetme

Bu bölümde, Azure AD için Windows 10 Cihazınızı kaydetmek için adımları sağlar. İle başarıyla kayıtlı bir cihazla görünür bir **iş veya Okul hesabı** girişi.

![Kaydolma](./media/device-management-azuread-registered-devices-windows10-setup/08.png)


**Windows 10 Cihazınızı kaydetmek için:**

1. İçinde **Başlat** menüsünde tıklatın **ayarları**.

    ![Ayarlar](./media/device-management-azuread-registered-devices-windows10-setup/01.png)

2. Tıklatın **hesapları**.

    ![Hesaplar](./media/device-management-azuread-registered-devices-windows10-setup/02.png)


3. Tıklatın **erişim iş veya Okul**.

    ![Erişim iş veya Okul](./media/device-management-azuread-registered-devices-windows10-setup/03.png)

4. Üzerinde **erişim iş veya Okul** iletişim kutusunda, tıklatın **Bağlan**.

    ![Bağlan](./media/device-management-azuread-registered-devices-windows10-setup/04.png)


5. Üzerinde **bir iş veya Okul hesabı ayarlamanız** iletişim kutusunda, hesap adınızı girin (örneğin, someone@example.com) ve ardından **sonraki**.

    ![Bağlan](./media/device-management-azuread-registered-devices-windows10-setup/06.png)


6. Üzerinde **parola gir** iletişim kutusunda, parolanızı girin ve ardından **sonraki**.

    ![Bağlan](./media/device-management-azuread-registered-devices-windows10-setup/05.png)


7. Üzerinde **tamamen hazırsınız** iletişim kutusunda, tıklatın **Bitti**.

    ![Bağlan](./media/device-management-azuread-registered-devices-windows10-setup/07.png)

## <a name="verification"></a>Doğrulama

Bir aygıt için Azure AD alanına katılıp katılmadığını doğrulamak için gözden geçirebilirsiniz **erişim iş veya Okul** aygıtınızda iletişim.

![Kaydolma](./media/device-management-azuread-registered-devices-windows10-setup/08.png)

Alternatif olarak, Azure AD portalı aygıt ayarlarını gözden geçirebilirsiniz.

![Kaydolma](./media/device-management-azuread-registered-devices-windows10-setup/09.png)





## <a name="next-steps"></a>Sonraki adımlar

- Daha fazla bilgi için bkz: [Azure Active Directory'de cihaz yönetimine giriş](device-management-introduction.md)

- Azure AD portalında cihazları yönetme hakkında daha fazla bilgi için bkz: [Azure portalını kullanarak cihazları yönetme ](device-management-azure-portal.md).




