---
title: "Azure Active Directory ayarlama alanına katılmış aygıtlar | Microsoft Docs"
description: "Azure Active Directory'ye katılmış cihazları öğrenin."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: mtillman
editor: 
ms.assetid: 54e1b01b-03ee-4c46-bcf0-e01affc0419d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/15/2018
ms.author: markvi
ms.reviewer: jairoc
ms.openlocfilehash: ac6c9224925e5bfd3cb056c6c8d9cf2a96b0eb2b
ms.sourcegitcommit: 384d2ec82214e8af0fc4891f9f840fb7cf89ef59
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/16/2018
---
# <a name="set-up-azure-active-directory-joined-devices"></a>Azure Active Directory ayarlama katılmış cihazlarda

Azure Active Directory'de (Azure AD) ile cihaz yönetimi, güvenlik ve uyumluluğa yönelik standartlarınızı karşılamak aygıtlardan kullanıcılarınızın kaynaklarınızı eriştiğiniz emin olabilirsiniz. Daha fazla bilgi için bkz: [Azure Active Directory'de cihaz yönetimine giriş](device-management-introduction.md).

Azure AD denetiminde çalışma şirkete ait Windows 10 cihazları getirmek istiyorsanız, bunu Azure AD alanına katılmış cihazları yapılandırarak gerçekleştirebilirsiniz. Bu konu ile ilgili adımları sağlar. 


## <a name="prerequisites"></a>Önkoşullar

Windows 10 cihazına katılmak için cihaz Kayıt Hizmeti'ni aygıtlarını kaydetmesini sağlamak için yapılandırılmalıdır. Azure AD kiracınızda aygıtları katılma iznine sahip ek olarak, daha az aygıtlarının yapılandırılmış en büyük değerden daha kayıtlı olmalıdır. Daha fazla bilgi için bkz: [aygıt ayarlarını yapılandır](device-management-azure-portal.md#configure-device-settings).



## <a name="what-you-should-know"></a>Bilmeniz gerekenler


- Windows Azure AD içinde kuruluşunuzun dizininde cihaz birleştirir.

- Çok faktörlü kimlik doğrulaması sınaması gitmesi gerekli. Bu sorunu, BT yöneticiniz tarafından yapılandırılabilir.

- Azure AD cihaz mobil cihaz Yönetimi kayıt gerektirir ve uygulanabilirse kaydettiği olup olmadığını denetler.

- Yönetilen bir kullanıcı varsa, Windows, masaüstüne otomatik oturum açma alır.

- Bir Federasyon kullanıcısı varsa, kimlik bilgilerinizi kullanarak oturum açın gerekir.


## <a name="joining-a-device"></a>Bir aygıtı birleştirme

Bu bölümde, Azure AD ile Windows 10 Cihazınızı eklemek için adımları sağlar. Azure AD ile başarıyla Cihazınızı katıldıysanız, **erişim iş veya Okul** iletişim bunu belirten bir **bağlı \<Azure AD\>**  girişi.

![Bağlı](./media/device-management-azuread-joined-devices-setup/13.png)


**Windows 10 Cihazınızı katılmak için:**

1. İçinde **Başlat** menüsünde tıklatın **ayarları**.

    ![Ayarlar](./media/device-management-azuread-joined-devices-setup/01.png)

2. Tıklatın **hesapları**.

    ![Hesaplar](./media/device-management-azuread-joined-devices-setup/02.png)


3. Tıklatın **erişim iş veya Okul**.

    ![Erişim iş veya Okul](./media/device-management-azuread-joined-devices-setup/03.png)

4. Üzerinde **erişim iş veya Okul** iletişim kutusunda, tıklatın **Bağlan**.

    ![Bağlan](./media/device-management-azuread-joined-devices-setup/04.png)


5. Üzerinde **bir iş veya Okul hesabı kurmak** iletişim kutusunda, tıklatın **bu cihaz Azure Active Directory'ye katılmasını**.

    ![Bağlan](./media/device-management-azuread-joined-devices-setup/08.png)


6. Üzerinde **şimdi oturum elde** iletişim kutusunda, hesap adınızı girin (örneğin, someone@example.com) ve ardından **sonraki**.

    ![Şimdi, oturumunuzu Al](./media/device-management-azuread-joined-devices-setup/10.png)


6. Üzerinde **parola gir** iletişim kutusunda, parolanızı girin ve ardından **oturum**.

    ![Parola girin](./media/device-management-azuread-joined-devices-setup/05.png)


7. Üzerinde **kuruluşunuzun bu olduğundan emin olun** iletişim kutusunda, tıklatın **katılma**.

    ![Bu, kuruluşunuzun olduğundan emin olun](./media/device-management-azuread-joined-devices-setup/11.png)


8. Üzerinde **tamamen hazırsınız** iletişim kutusunda, tıklatın **Bitti**.

    ![İşinizi tamamlandı](./media/device-management-azuread-joined-devices-setup/12.png)

## <a name="verification"></a>Doğrulama

Bir aygıt için Azure AD alanına katılıp katılmadığını doğrulamak için gözden geçirebilirsiniz **erişim iş veya Okul** aygıtınızda iletişim.

![Bağlı](./media/device-management-azuread-joined-devices-setup/13.png)

Alternatif olarak, aşağıdaki komutu çalıştırabilirsiniz:`dsregcmd /status`  
Başarıyla alanına katılmış bir cihazda **AzureAdJoined** olan **Evet**.

![Bağlı](./media/device-management-azuread-joined-devices-setup/14.png)

Ayrıca, Azure AD portalı aygıt ayarlarını gözden geçirebilirsiniz.

![Bağlı](./media/device-management-azuread-joined-devices-setup/15.png)

Daha fazla bilgi için bkz: [aygıtları bulmak](device-management-azure-portal.md#locate-devices).


## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi için bkz. 

- [Azure Active Directory'de cihaz yönetimine giriş](device-management-introduction.md)
- [Azure portalını kullanarak cihazları yönetme](device-management-azure-portal.md)
- 



