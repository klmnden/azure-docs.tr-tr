---
title: "Azure Active Directory'de koşullu erişimi kullanmaya başlama | Microsoft Docs"
description: "Koşullu erişim konumu koşulu kullanarak test edin."
services: active-directory
keywords: "uygulamaları, Azure AD ile koşullu erişim, koşullu erişim ilkeleri, şirket kaynaklarına güvenli erişim için koşullu erişim"
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: 
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 10/21/2017
ms.author: markvi
ms.reviewer: calebb
ms.openlocfilehash: b9a95cea57f4d0a4a927679cd4802bb56420887a
ms.sourcegitcommit: 4ed3fe11c138eeed19aef0315a4f470f447eac0c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/23/2017
---
# <a name="get-started-with-conditional-access-in-azure-active-directory"></a>Azure Active Directory'de koşullu erişimi kullanmaya başlama

Koşullu erişim, uygulamalarınızı yetkili kullanıcıların erişebilmeniz için koşullar tanımlamanızı sağlayan Azure Active Directory bir yetenektir. 

Bu konu, ortamınızdaki bir konum koşula göre koşullu bir erişim test etmek için yönergeler sağlar.  


## <a name="scenario-description"></a>Senaryo açıklaması

Yalnızca Kurumsal intranet bağlantısı gerçekleştirilmez erişim uygulamalar için çok faktörlü kimlik doğrulaması gerektirmek için birçok kuruluşta bir ortak gerekli değildir. Azure Active Directory ile konum temelli koşullu erişim ilkesini yapılandırarak bu amacı kolayca gerçekleştirebilirsiniz. Bu konu, ilgili ilke yapılandırmaya ilişkin ayrıntılı yönergeler sağlar. İlke yararlanır [güvenilen IP'leri](../multi-factor-authentication/multi-factor-authentication-whats-next.md#trusted-ips) şirkete yapılan erişim denemesi ayırt etmek için intranet ve diğer tüm konumlara kullanıcının.


## <a name="prerequisites"></a>Ön koşullar

Bu konuda açıklanan senaryo özetlenen kavramları hakkında bilgi sahibi olduğunu varsayar [Azure Active Directory koşullu erişim](active-directory-conditional-access-azure-portal.md).

Bu senaryoyu test etmek için gerekir:

- Test kullanıcısı oluşturma 

- Test kullanıcısı için bir Azure AD Premium lisansı atayın

- Yönetilen bir uygulama yapılandırma ve test kullanıcınız atayın

- Güvenilen IP'leri yapılandırmak

Güvenilen IP'ler hakkında daha fazla ayrıntı gerekirse bkz [güvenilen IP'ler](../multi-factor-authentication/multi-factor-authentication-whats-next.md#trusted-ips).


## <a name="policy-configuration-steps"></a>İlke yapılandırma adımları

**Koşullu erişim ilkesini yapılandırmak için aşağıdakileri yapın:**

1. Azure portalında sol gezinti çubuğu tıklatın **Azure Active Directory**. 

    ![Koşullu erişim](./media/active-directory-conditional-access-azure-portal-get-started/01.png)

2. Üzerinde **Azure Active Directory** dikey penceresindeki **Yönet** 'yi tıklatın **koşullu erişim**.

    ![Koşullu erişim](./media/active-directory-conditional-access-azure-portal-get-started/02.png)
 
3. Üzerinde **koşullu erişim** açmak için dikey penceresinde **yeni** üstteki araç çubuğunda dikey tıklayın **Ekle**.

    ![Koşullu erişim](./media/active-directory-conditional-access-azure-portal-get-started/03.png)

4. Üzerinde **yeni** dikey penceresindeki **adı** metin kutusuna, ilkeniz için bir ad yazın.

    ![Koşullu erişim](./media/active-directory-conditional-access-azure-portal-get-started/04.png)

5. İçinde **atama** 'yi tıklatın **kullanıcılar ve gruplar**.

    ![Koşullu erişim](./media/active-directory-conditional-access-azure-portal-get-started/05.png)

6. Üzerinde **kullanıcılar ve gruplar** dikey penceresinde, aşağıdaki adımları gerçekleştirin:

    ![Koşullu erişim](./media/active-directory-conditional-access-azure-portal-get-started/06.png)

    a. Tıklatın **kullanıcıları ve grupları seçin**.

    b. **Seç**'e tıklayın.

    c. Üzerinde **seçin** dikey penceresinde, test kullanıcınız seçin ve ardından **seçin**.

    d. Üzerinde **kullanıcılar ve gruplar** dikey penceresinde tıklatın **Bitti**.

7. Üzerinde **yeni** açmak için dikey penceresinde **bulut uygulamaları** dikey penceresinde, **atama** 'yi tıklatın **bulut uygulamaları**.

    ![Koşullu erişim](./media/active-directory-conditional-access-azure-portal-get-started/07.png)

8. Üzerinde **bulut uygulamaları** dikey penceresinde, aşağıdaki adımları gerçekleştirin:

    ![Koşullu erişim](./media/active-directory-conditional-access-azure-portal-get-started/08.png)

    a. Tıklatın **uygulamaları**.

    b. **Seç**'e tıklayın.

    c. Üzerinde **seçin** dikey penceresinde, bulut uygulamanızı seçin ve ardından **seçin**.

    d. Üzerinde **bulut uygulamaları** dikey penceresinde tıklatın **Bitti**.

9. Üzerinde **yeni** açmak için dikey penceresinde **koşullar** dikey penceresinde, **atama** 'yi tıklatın **koşullar**.

    ![Koşullu erişim](./media/active-directory-conditional-access-azure-portal-get-started/09.png)

10. Üzerinde **koşullar** açmak için dikey penceresinde **konumları** dikey penceresinde tıklatın **konumları**.

    ![Koşullu erişim](./media/active-directory-conditional-access-azure-portal-get-started/10.png)

11. Üzerinde **konumları** dikey penceresinde, aşağıdaki adımları gerçekleştirin:

    ![Koşullu erişim](./media/active-directory-conditional-access-azure-portal-get-started/11.png)

    a. Altında **yapılandırma**, tıklatın **Evet**.

    b. Altında **INCLUDE**, tıklatın **tüm konumları**.

    c. Tıklatın **hariç**ve ardından **tüm güvenilen IP'leri**.

    ![Koşullu erişim](./media/active-directory-conditional-access-azure-portal-get-started/12.png)

    d. **Bitti**’ye tıklayın.

12. Üzerinde **koşullar** dikey penceresinde tıklatın **Bitti**.

13. Üzerinde **yeni** açmak için dikey penceresinde **Grant** dikey penceresinde, **denetimleri** 'yi tıklatın **Grant**.

    ![Koşullu erişim](./media/active-directory-conditional-access-azure-portal-get-started/13.png)

14. Üzerinde **Grant** dikey penceresinde, aşağıdaki adımları gerçekleştirin:

    ![Koşullu erişim](./media/active-directory-conditional-access-azure-portal-get-started/14.png)

    a. Seçin **çok faktörlü kimlik doğrulaması gerektiren**.

    b. **Seç**'e tıklayın.

15. Üzerinde **yeni** dikey altında **ilkesini etkinleştir**, tıklatın **üzerinde**.

    ![Koşullu erişim](./media/active-directory-conditional-access-azure-portal-get-started/15.png)

16. Üzerinde **yeni** dikey penceresinde tıklatın **oluşturma**.


## <a name="testing-the-policy"></a>İlkeyi test etme

İlkeniz sınamak için uygulamanızı bir aygıttan erişim: 

1. İçinde yapılandırılmış güvenilen IP'ler bir IP adresi vardır 

1. Yapılandırılmış güvenilen IP'leri içinde olmayan bir IP adresi var.

Çok faktörlü kimlik doğrulaması yalnızca, yapılandırılmış güvenilen IP'leri içinde değil bir aygıttan yapılan bağlantı girişimi sırasında gerekli olması gerekir. 


## <a name="next-steps"></a>Sonraki adımlar

Koşullu erişim hakkında daha fazla bilgi edinmek istiyorsanız, bkz: [Azure Active Directory koşullu erişim](active-directory-conditional-access-azure-portal.md).

