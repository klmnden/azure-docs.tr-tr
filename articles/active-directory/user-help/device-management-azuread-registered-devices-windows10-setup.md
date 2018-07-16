---
title: Azure Active Directory'ye kayıtlı cihazları ayarlama | Microsoft Docs
description: Azure Active Directory'ye kayıtlı cihazları ayarlama konusunda bilgi edinin.
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
ms.openlocfilehash: 7ce632c76a86fb00101db6664e9e79615484f9a1
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/14/2018
ms.locfileid: "39059798"
---
# <a name="set-up-azure-active-directory-registered-windows-10-devices"></a>Azure Active Directory ayarlama kayıtlı Windows 10 cihazları

İle cihaz Yönetimi Azure Active Directory'de (Azure AD) güvenlik ve uyumluluğa yönelik standartlarınızı karşılayan cihazlardan kullanıcılarınızın kaynaklarınızı eriştiğiniz emin olabilirsiniz. Daha fazla bilgi için [Azure Active Directory'de cihaz yönetimine giriş](../device-management-introduction.md).

Etkinleştirmek istiyorsanız **kendi cihazını getir (KCG)** senaryosu görevi bu Azure AD kayıtlı cihazları yapılandırarak. Azure AD'de kayıtlı Azure AD cihazları Windows 10, iOS, Android ve macOS için yapılandırabilirsiniz. Bu makalede, Windows 10 cihazları için ile ilgili adımları sağlar. 


## <a name="before-you-begin"></a>Başlamadan önce

Windows 10 cihazı kaydetmek için cihaz Kayıt Hizmeti'ni aygıtlarını kaydetmesini sağlamak için yapılandırılmalıdır. Ayrıca, daha az cihazları yapılandırılmış en fazla kayıtlı olması gerekir. Daha fazla bilgi için [cihaz ayarlarını yapılandırma](../device-management-azure-portal.md#configure-device-settings).

## <a name="what-you-should-know"></a>Bilmeniz gerekenler

Bir cihaz kaydederken şunları aklınızda tutun:

- Windows Azure AD'de kuruluşunuzun dizininde bu cihazı kaydeder

- Çok faktörlü kimlik doğrulaması sınaması gitmek için gerekli olabilir. Bu sınama, BT yöneticiniz tarafından yapılandırılabilir.

- Azure AD cihaz mobil cihaz Yönetimi kaydı gerektirir ve uygunsa kaydettiği denetler.

- Yönetilen bir kullanıcı varsa, Windows, masaüstüne otomatik oturum açma alır.

- Bir Federasyon kullanıcısı varsa, bir Windows oturum açma ekranı için kimlik bilgilerinizi girmeniz için alınır.


## <a name="registering-a-device"></a>Bir cihazı kaydetme

Bu bölümde, Azure ad Windows 10 Cihazınızı kaydetmek için adımları sağlar. İle başarıyla kayıtlı bir cihaz görünür bir **iş veya Okul hesabı** girişi.

![Kaydolma](./media/device-management-azuread-registered-devices-windows10-setup/08.png)


**Windows 10 Cihazınızı kaydetmek için:**

1. İçinde **Başlat** menüsünde tıklatın **ayarları**.

    ![Ayarlar](./media/device-management-azuread-registered-devices-windows10-setup/01.png)

2. Tıklayın **hesapları**.

    ![Hesaplar](./media/device-management-azuread-registered-devices-windows10-setup/02.png)


3. Tıklayın **işe veya okula erişim**.

    ![İşe veya okula erişim](./media/device-management-azuread-registered-devices-windows10-setup/03.png)

4. Üzerinde **işe veya okula erişim** iletişim kutusunda, tıklayın **Connect**.

    ![Bağlan](./media/device-management-azuread-registered-devices-windows10-setup/04.png)


5. Üzerinde **bir iş veya Okul hesabı ayarlama** iletişim kutusunda hesap adınızı girin (örneğin, someone@example.com) ve ardından **sonraki**.

    ![Bağlan](./media/device-management-azuread-registered-devices-windows10-setup/06.png)


6. Üzerinde **parola gir** iletişim kutusunda, parolanızı girin ve ardından **sonraki**.

    ![Bağlan](./media/device-management-azuread-registered-devices-windows10-setup/05.png)


7. Üzerinde **hazırsınız** iletişim kutusunda, tıklayın **Bitti**.

    ![Bağlan](./media/device-management-azuread-registered-devices-windows10-setup/07.png)

## <a name="verification"></a>Doğrulama

Bir cihaz için bir Azure AD alanına olup olmadığını doğrulamak için gözden geçirebilirsiniz **işe veya okula erişim** Cihazınızda iletişim.

![Kaydolma](./media/device-management-azuread-registered-devices-windows10-setup/08.png)

Alternatif olarak, Azure AD portalında cihaz ayarları gözden geçirebilirsiniz.

![Kaydolma](./media/device-management-azuread-registered-devices-windows10-setup/09.png)





## <a name="next-steps"></a>Sonraki adımlar

- Daha fazla bilgi için [Azure Active Directory'de cihaz yönetimine giriş](../device-management-introduction.md)

- Azure AD portalında cihazları yönetme hakkında daha fazla bilgi için bkz. [Azure portalını kullanarak cihazları yönetme ](../device-management-azure-portal.md).




