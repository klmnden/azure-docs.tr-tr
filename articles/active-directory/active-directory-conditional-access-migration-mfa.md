---
title: "Azure portalında çok faktörlü kimlik doğrulaması gerektiren bir Klasik ilke geçirme | Microsoft Docs"
description: "Bu makalede, Azure portalında çok faktörlü kimlik doğrulaması gerektiren bir Klasik ilke geçirmek gösterilmiştir."
services: active-directory
keywords: "uygulamaları, Azure AD ile koşullu erişim, koşullu erişim ilkeleri, şirket kaynaklarına güvenli erişim için koşullu erişim"
documentationcenter: 
author: MarkusVi
manager: mtillman
editor: 
ms.assetid: 8c1d978f-e80b-420e-853a-8bbddc4bcdad
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/11/2017
ms.author: markvi
ms.reviewer: nigu
ms.openlocfilehash: 77484dc3773736ea15c39ede5f9d49b6b694d960
ms.sourcegitcommit: aaba209b9cea87cb983e6f498e7a820616a77471
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/12/2017
---
# <a name="migrate-a-classic-policy-that-requires-multi-factor-authentication-in-the-azure-portal"></a>Azure portalında çok faktörlü kimlik doğrulaması gerektiren bir Klasik ilke geçirme 

Bu makalede gerektiren klasik bir ilke geçirmek nasıl gösterilmektedir **çok faktörlü kimlik doğrulaması** bir bulut uygulaması için. Bir önkoşul olmamasına karşın, okumanızı öneririz [Azure portalında Klasik ilkelerine](active-directory-conditional-access-migration.md) Klasik ilkelerinizi geçişi başlatmadan önce.


 
## <a name="overview"></a>Genel Bakış 

Bu makalede senaryoda gerektiren klasik bir ilke geçirmek nasıl gösterir **çok faktörlü kimlik doğrulaması** bir bulut uygulaması için. 

![Azure Active Directory](./media/active-directory-conditional-access-migration/33.png)


Geçiş işlemi aşağıdaki adımlardan oluşur:

1. [Klasik İlkesi](#open-a-classic-policy) almak için yapılandırma ayarları.
2. Yeni bir Klasik ilkeniz değiştirmek için Azure AD koşullu erişim ilkesi. 
3. Klasik ilkeyi devre dışı bırakır.



## <a name="open-a-classic-policy"></a>Klasik İlkesi'ni açın

1. İçinde [Azure portal](https://portal.azure.com), sol gezinti çubuğu üzerinde tıklatın **Azure Active Directory**.

    ![Azure Active Directory](./media/active-directory-conditional-access-migration-mfa/01.png)

2. Üzerinde **Azure Active Directory** sayfasında **Yönet** 'yi tıklatın **koşullu erişim**.

    ![Koşullu erişim](./media/active-directory-conditional-access-migration-mfa/02.png)

3. İçinde **Yönet** 'yi tıklatın **Klasik ilkeleri (Önizleme)**.

    ![Klasik ilkeler](./media/active-directory-conditional-access-migration-mfa/12.png)

4. Klasik ilkeleri listesinde gerektirir İlkesi'ni **çok faktörlü kimlik doğrulaması** bir bulut uygulaması için.

    ![Klasik ilkeler](./media/active-directory-conditional-access-migration-mfa/13.png)


## <a name="create-a-new-conditional-access-policy"></a>Yeni bir koşullu erişim ilkesi oluşturma


1. İçinde [Azure portal](https://portal.azure.com), sol gezinti çubuğu üzerinde tıklatın **Azure Active Directory**.

    ![Azure Active Directory](./media/active-directory-conditional-access-migration/01.png)

2. Üzerinde **Azure Active Directory** sayfasında **Yönet** 'yi tıklatın **koşullu erişim**.

    ![Koşullu erişim](./media/active-directory-conditional-access-migration/02.png)



3. Üzerinde **koşullu erişim** sayfasını açmak için **yeni** üstteki araç çubuğunda sayfasını tıklatın **Ekle**.

    ![Koşullu erişim](./media/active-directory-conditional-access-migration/03.png)

4. Üzerinde **yeni** sayfasında **adı** metin kutusuna, ilkeniz için bir ad yazın.

    ![Koşullu erişim](./media/active-directory-conditional-access-migration/29.png)

5. İçinde **atamaları** 'yi tıklatın **kullanıcılar ve gruplar**.

    ![Koşullu erişim](./media/active-directory-conditional-access-migration/05.png)

    a. Klasik ilkede seçilen tüm kullanıcılar varsa, tıklatın **tüm kullanıcılar**. 

    ![Koşullu erişim](./media/active-directory-conditional-access-migration/35.png)

    b. Klasik ilkenizde seçili gruplarınız varsa, tıklatın **kullanıcıları ve grupları seçin**ve ardından gerekli kullanıcılar ve Gruplar'ı seçin.

    ![Koşullu erişim](./media/active-directory-conditional-access-migration/36.png)

    c. Dışlanan gruplarınız varsa, tıklatın **hariç** sekmesini ve ardından gerekli kullanıcılar ve Gruplar'ı seçin. 

    ![Koşullu erişim](./media/active-directory-conditional-access-migration/37.png)

6. Üzerinde **yeni** sayfasını açmak için **bulut uygulamaları** sayfasında **atama** 'yi tıklatın **bulut uygulamaları**.

    ![Koşullu erişim](./media/active-directory-conditional-access-azure-portal-get-started/07.png)

8. Üzerinde **bulut uygulamaları** sayfasında, aşağıdaki adımları gerçekleştirin:

    ![Koşullu erişim](./media/active-directory-conditional-access-migration/08.png)

    a. Tıklatın **uygulamaları**.

    b. **Seç**'e tıklayın.

    c. Üzerinde **seçin** sayfasında, bulut uygulamanızı seçin ve ardından **seçin**.

    d. Üzerinde **bulut uygulamaları** sayfasında, **Bitti**.



9. Varsa **çok faktörlü kimlik doğrulaması gerektiren** seçili:

    ![Koşullu erişim](./media/active-directory-conditional-access-migration/26.png)

    a. İçinde **erişim denetimleri** 'yi tıklatın **Grant**.

    ![Koşullu erişim](./media/active-directory-conditional-access-migration/27.png)

    b. Üzerinde **Grant** sayfasında, **erişim izni verme**ve ardından **çok faktörlü kimlik doğrulaması gerektiren**.

    c. **Seç**'e tıklayın.


10. Tıklatın **üzerinde** ilkeniz etkinleştirmek için.

    ![Koşullu erişim](./media/active-directory-conditional-access-migration/30.png)



## <a name="disable-the-classic-policy"></a>Klasik Bu ilkeyi devre dışı

Klasik ilkeniz devre dışı bırakmak için **devre dışı** içinde **ayrıntıları** görünümü.

![Klasik ilkeler](./media/active-directory-conditional-access-migration-mfa/14.png)



## <a name="next-steps"></a>Sonraki adımlar

- Klasik ilke geçişi hakkında daha fazla bilgi için bkz: [Azure portalında Klasik ilkelerine](active-directory-conditional-access-migration.md).


- Koşullu erişim ilkesini yapılandırma hakkında bilmek istiyorsanız [Azure Active Directory'de koşullu erişimi kullanmaya başlama](active-directory-conditional-access-azure-portal-get-started.md).

- Ortamınız için koşullu erişim ilkelerini yapılandırma için hazır olup olmadığını görmek [Azure Active Directory'de koşullu erişim için en iyi uygulamaları](active-directory-conditional-access-best-practices.md). 
