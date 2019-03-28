---
title: Azure portalında çok faktörlü kimlik doğrulaması gerektiren bir Klasik ilke geçişi | Microsoft Docs
description: Bu makalede, Azure portalında çok faktörlü kimlik doğrulaması gerektiren bir Klasik ilke geçirme gösterilmektedir.
services: active-directory
keywords: uygulamalara koşullu erişim, Azure AD ile koşullu erişim, şirket kaynaklarına güvenli erişim, koşullu erişim ilkeleri
documentationcenter: ''
author: MicrosoftGuyJFlo
manager: daveba
editor: ''
ms.assetid: 8c1d978f-e80b-420e-853a-8bbddc4bcdad
ms.service: active-directory
ms.subservice: conditional-access
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 06/13/2018
ms.author: joflore
ms.reviewer: nigu
ms.collection: M365-identity-device-management
ms.openlocfilehash: e5e26f901b0170116a83663176a402b55d6d70b0
ms.sourcegitcommit: 6da4959d3a1ffcd8a781b709578668471ec6bf1b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2019
ms.locfileid: "58522154"
---
# <a name="migrate-a-classic-policy-that-requires-multi-factor-authentication-in-the-azure-portal"></a>Azure portalında çok faktörlü kimlik doğrulaması gerektiren bir Klasik ilke geçişi 

Bu makale gerektiren bir Klasik ilke geçirme **çok faktörlü kimlik doğrulaması** bulut uygulaması için. Bir önkoşul olmamasına karşın, okumanızı öneririz [Azure portalında Klasik ilkeleri geçirme](policy-migration.md) , Klasik ilkeleri geçirme işlemine başlamadan önce.


 
## <a name="overview"></a>Genel Bakış 

Bu makaledeki senaryoda gerektiren bir Klasik ilke geçirme gösterilmektedir **çok faktörlü kimlik doğrulaması** bulut uygulaması için. 

![Azure Active Directory](./media/policy-migration/33.png)


Geçiş işlemi aşağıdaki adımlardan oluşur:

1. [Klasik ilke açın](#open-a-classic-policy) yapılandırma ayarlarını almak için.
2. Yeni bir Azure AD koşullu erişim ilkesi Klasik ilkeniz değiştirilecek. 
3. Klasik ilke devre dışı bırakın.



## <a name="open-a-classic-policy"></a>Klasik ilke açın

1. İçinde [Azure portalında](https://portal.azure.com), sol gezinti tıklatın **Azure Active Directory**.

    ![Azure Active Directory](./media/policy-migration-mfa/01.png)

2. Üzerinde **Azure Active Directory** sayfasında **Yönet** bölümünde **koşullu erişim**.

    ![Koşullu erişim](./media/policy-migration-mfa/02.png)

3. İçinde **Yönet** bölümünde **Klasik ilkeleri (Önizleme)**.

    ![Klasik ilkeler](./media/policy-migration-mfa/12.png)

4. Klasik ilkeleri listesinde gerektiren ilkeye tıklayın **çok faktörlü kimlik doğrulaması** bulut uygulaması için.

    ![Klasik ilkeler](./media/policy-migration-mfa/13.png)


## <a name="create-a-new-conditional-access-policy"></a>Yeni bir koşullu erişim ilkesi oluşturma


1. İçinde [Azure portalında](https://portal.azure.com), sol gezinti tıklatın **Azure Active Directory**.

    ![Azure Active Directory](./media/policy-migration/01.png)

2. Üzerinde **Azure Active Directory** sayfasında **Yönet** bölümünde **koşullu erişim**.

    ![Koşullu erişim](./media/policy-migration/02.png)



3. Üzerinde **koşullu erişim** sayfasında açmak için **yeni** sayfasında, üstteki araç çubuğunda tıklatın **Ekle**.

    ![Koşullu erişim](./media/policy-migration/03.png)

4. Üzerinde **yeni** sayfasında **adı** metin ilkeniz için bir ad yazın.

    ![Koşullu erişim](./media/policy-migration/29.png)

5. İçinde **atamaları** bölümünde **kullanıcılar ve gruplar**.

    ![Koşullu erişim](./media/policy-migration/05.png)

    a. Tüm kullanıcıları seçtiyseniz Klasik ilkeniz varsa tıklayın **tüm kullanıcılar**. 

    ![Koşullu erişim](./media/policy-migration/35.png)

    b. Klasik ilkeniz seçili grupları varsa, tıklayın **kullanıcıları ve grupları seçin**ve ardından gerekli kullanıcılar ve Gruplar'ı seçin.

    ![Koşullu erişim](./media/policy-migration/36.png)

    c. Dışlanan grupları varsa, tıklayın **hariç** sekmesini ve sonra gerekli kullanıcıları ve grupları seçin. 

    ![Koşullu erişim](./media/policy-migration/37.png)

6. Üzerinde **yeni** sayfasında açmak için **bulut uygulamaları** sayfasında **atama** bölümünde **bulut uygulamaları**.

8. Üzerinde **bulut uygulamaları** sayfasında, aşağıdaki adımları gerçekleştirin:

    ![Koşullu erişim](./media/policy-migration/08.png)

    a. Tıklayın **uygulamaları Seç**.

    b. **Seç**'e tıklayın.

    c. Üzerinde **seçin** sayfasında bulut uygulamanızı seçin ve ardından **seçin**.

    d. Üzerinde **bulut uygulamaları** sayfasında **Bitti**.



9. Varsa **çok faktörlü kimlik doğrulaması gerektiren** seçili:

    ![Koşullu erişim](./media/policy-migration/26.png)

    a. İçinde **erişim denetimleri** bölümünde **Grant**.

    ![Koşullu erişim](./media/policy-migration/27.png)

    b. Üzerinde **Grant** sayfasında **erişim ver**ve ardından **çok faktörlü kimlik doğrulaması gerektiren**.

    c. **Seç**'e tıklayın.


10. Tıklayın **üzerinde** ilkenizi etkinleştirmek için.

    ![Koşullu erişim](./media/policy-migration/30.png)



## <a name="disable-the-classic-policy"></a>Klasik ilke devre dışı bırak

Klasik ilkeniz devre dışı bırakmak için **devre dışı** içinde **ayrıntıları** görünümü.

![Klasik ilkeler](./media/policy-migration-mfa/14.png)



## <a name="next-steps"></a>Sonraki adımlar

- Klasik ilke geçişi hakkında daha fazla bilgi için bkz. [Azure portalında Klasik ilkeleri geçirme](policy-migration.md).


- Koşullu erişim ilkesi yapılandırmak için bkz. nasıl bilmek istiyorsanız [Azure Active Directory koşullu erişimiyle belirli uygulamalar için MFA gerektiren](app-based-mfa.md).

- Ortamınızda koşullu erişim ilkelerini yapılandırmaya hazırsanız bkz. [Azure Active Directory'de koşullu erişim için en iyi yöntemler](best-practices.md). 
