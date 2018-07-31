---
title: Alanına katılmış cihazların Azure Active Directory'yi ayarlama | Microsoft Docs
description: Azure Active Directory'ye katılmış cihazları ayarlama konusunda bilgi edinin.
services: active-directory
author: eross-msft
manager: mtillman
ms.assetid: 54e1b01b-03ee-4c46-bcf0-e01affc0419d
ms.service: active-directory
ms.component: user-help
ms.workload: identity
ms.topic: conceptual
ms.date: 01/15/2018
ms.author: lizross
ms.reviewer: jairoc
ms.openlocfilehash: 511580b95646677a231a68862cae8a6aa57abcb0
ms.sourcegitcommit: 30fd606162804fe8ceaccbca057a6d3f8c4dd56d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/30/2018
ms.locfileid: "39345540"
---
# <a name="set-up-azure-active-directory-joined-devices"></a>Alanına katılmış cihazları Azure Active Directory'yi ayarlama

İle cihaz Yönetimi Azure Active Directory'de (Azure AD) güvenlik ve uyumluluğa yönelik standartlarınızı karşılayan cihazlardan kullanıcılarınızın kaynaklarınızı eriştiğiniz emin olabilirsiniz. Daha fazla bilgi için [Azure Active Directory'de cihaz yönetimine giriş](../device-management-introduction.md).

Azure AD denetimi altında iş ait Windows 10 cihazları getirmek istiyorsanız, bunu Azure AD'ye katılmış cihazları yapılandırarak gerçekleştirebilirsiniz. Bu konu ile ilgili adımları sağlar. 


## <a name="prerequisites"></a>Önkoşullar

Windows 10 cihazı alanına katılmak için cihaz Kayıt Hizmeti'ni aygıtlarını kaydetmesini sağlamak için yapılandırılmalıdır. Azure AD kiracınıza cihazları katılma izni sahip olmaya ek olarak, daha az cihazları yapılandırılmış en fazla kayıtlı olması gerekir. Daha fazla bilgi için [cihaz ayarlarını yapılandırma](../device-management-azure-portal.md#configure-device-settings).



## <a name="what-you-should-know"></a>Bilmeniz gerekenler


- Windows, Azure AD'de cihaz kuruluşunuzun dizininde birleştirir.

- Çok faktörlü kimlik doğrulaması sınaması gitmek için gerekli olabilir. Bu sınama, BT yöneticiniz tarafından yapılandırılabilir.

- Azure AD cihaz mobil cihaz Yönetimi kaydı gerektirir ve uygunsa kaydettiği denetler.

- Yönetilen bir kullanıcı varsa, Windows, masaüstüne otomatik oturum açma alır.

- Bir Federasyon kullanıcısı varsa, kimlik bilgilerinizi kullanarak oturum gerekir.

- Birleştirildiyse, WS-Federasyon ve WS-Trust uç noktası kullanıcı adı/parola kimlik sağlayıcınız desteklemelidir. Bu sürüm, 1.3 veya 2005 olabilir. Bu protokol desteği, hem cihaz Azure AD'ye ve cihaza bir parola ile oturum açmak için gereklidir. 




## <a name="joining-a-device"></a>Bir cihaz katılma

Bu bölümde, Windows 10 Cihazınızı, Azure AD'ye katılmak için adımları sağlar. Başarıyla alanına katılmış bir cihaz olarak gösterilir **bağlı \<Azure AD'nize\>**.

![Bağlanıldı](./media/device-management-azuread-joined-devices-setup/13.png)


**Windows 10 Cihazınızı katılmak için:**

1. İçinde **Başlat** menüsünde tıklatın **ayarları**.

    ![Ayarlar](./media/device-management-azuread-joined-devices-setup/01.png)

2. Tıklayın **hesapları**.

    ![Hesaplar](./media/device-management-azuread-joined-devices-setup/02.png)


3. Tıklayın **işe veya okula erişim**.

    ![İşe veya okula erişim](./media/device-management-azuread-joined-devices-setup/03.png)

4. Üzerinde **işe veya okula erişim** iletişim kutusunda, tıklayın **Connect**.

    ![Bağlan](./media/device-management-azuread-joined-devices-setup/04.png)


5. Üzerinde **Kurulum bir iş veya Okul hesabı** iletişim kutusunda, tıklayın **bu cihazı Azure Active Directory'ye katılmasını**.

    ![Bağlan](./media/device-management-azuread-joined-devices-setup/08.png)


6. Üzerinde **geçelim, açan** iletişim kutusunda hesap adınızı girin (örneğin, someone@example.com) ve ardından **sonraki**.

    ![Şimdi, oturum Al](./media/device-management-azuread-joined-devices-setup/10.png)


6. Üzerinde **parola gir** iletişim kutusunda, parolanızı girin ve ardından **oturum**.

    ![Parolayı girin](./media/device-management-azuread-joined-devices-setup/05.png)


7. Üzerinde **kuruluşunuz bu olduğundan emin olun** iletişim kutusunda, tıklayın **katılın**.

    ![Bu, kuruluşunuzun olduğundan emin olun](./media/device-management-azuread-joined-devices-setup/11.png)


8. Üzerinde **hazırsınız** iletişim kutusunda, tıklayın **Bitti**.

    ![Her şey](./media/device-management-azuread-joined-devices-setup/12.png)

## <a name="verification"></a>Doğrulama

Bir cihaz için bir Azure AD alanına olup olmadığını doğrulamak için gözden geçirebilirsiniz **işe veya okula erişim** Cihazınızda iletişim.

![Bağlanıldı](./media/device-management-azuread-joined-devices-setup/13.png)

Alternatif olarak, aşağıdaki komutu çalıştırabilirsiniz: `dsregcmd /status`  
Başarıyla alanına katılmış bir cihazda **AzureAdJoined** olduğu **Evet**.

![Bağlanıldı](./media/device-management-azuread-joined-devices-setup/14.png)

Ayrıca, Azure AD portalında cihaz ayarları gözden geçirebilirsiniz.

![Bağlanıldı](./media/device-management-azuread-joined-devices-setup/15.png)

Daha fazla bilgi için [cihazlarını bulma](../device-management-azure-portal.md#locate-devices).


## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi için bkz. 

- [Azure Active Directory'de cihaz yönetimine giriş](../device-management-introduction.md)
- [Azure portalını kullanarak cihazları yönetme](../device-management-azure-portal.md)
- 



