---
title: Hızlı Başlangıç - Azure Active Directory koşullu erişimi olan belirli uygulamalar için multi-Factor authentication (MFA) gerekli | Microsoft Docs
description: Bu Hızlı Başlangıç, Azure Active Directory (Azure AD) koşullu erişim kullanarak erişilen bulut uygulama türü için kimlik doğrulama gereksinimlerinizi nasıl bağlayabilirsiniz öğreneceksiniz.
services: active-directory
keywords: uygulamaları, Azure AD ile koşullu erişim, koşullu erişim ilkeleri, şirket kaynaklarına güvenli erişim için koşullu erişim
documentationcenter: ''
author: MarkusVi
manager: mtillman
ms.assetid: ''
ms.service: active-directory
ms.topic: article
ms.devlang: na
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/11/2018
ms.author: markvi
ms.reviewer: calebb
ms.openlocfilehash: 475b0229b9e627a56b02d2299ee2e5400aa0ede1
ms.sourcegitcommit: e14229bb94d61172046335972cfb1a708c8a97a5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/14/2018
---
# <a name="quickstart-require-mfa-for-specific-apps-with-azure-active-directory-conditional-access"></a>Hızlı Başlangıç: Azure Active Directory koşullu erişimi olan belirli uygulamalar için MFA gerekir. 

Kullanıcılarınız oturum açma deneyimini basitleştirmek için bunları bir kullanıcı adı ve parola kullanarak, bulut uygulamalarınızı oturum açmak izin vermek isteyebilirsiniz. Bununla birlikte, çoğu ortam için daha güçlü bir form çok faktörlü kimlik doğrulaması (MFA) gibi hesap doğrulama gerektirecek şekilde önerilir; en az birkaç uygulama var. Bu, kuruluşunuzun e-posta sisteminize veya HR uygulamalarınızı erişimi için örnek true için olabilir. Azure Active Directory (Azure AD), bu hedefi bir koşullu erişim ilkesi ile gerçekleştirebilirsiniz.    

Bu hızlı başlangıç nasıl yapılandırılacağını göstermektedir bir [Azure AD koşullu erişim ilkesi](active-directory-conditional-access-azure-portal.md) seçili bulut uygulamasının ortamınızda çok faktörlü kimlik doğrulaması gerektirir.

![İlke oluşturma](./media/active-directory-conditional-access-app-based-mfa/32.png)


Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.



## <a name="prerequisites"></a>Önkoşullar 

Bu hızlı başlangıç senaryoda tamamlamak için gerekir:

- **Bir Azure AD Premium edition erişimi** -Azure AD koşullu erişimi olan bir Azure AD Premium özelliği. 

- **Bir test hesabı olarak adlandırılan Isabella Simonsen** - sınama hesabı oluşturma, bkz: bilmiyorsanız [bulut tabanlı kullanıcılar eklemek](add-users-azure-active-directory.md#add-cloud-based-users).



## <a name="create-your-conditional-access-policy"></a>Koşullu erişim ilkesi oluşturma 

Bu bölümde gerekli koşullu erişim ilkesinin nasıl oluşturulacağını gösterir. Bu hızlı başlangıç senaryoda kullanır:

- Azure portalı MFA gerektiren bir bulut uygulama için yer tutucu olarak. 
- Koşullu erişim ilkesini test etmek için örnek kullanıcı.  

İlkenizde, ayarlayın:

|Ayar |Değer|
|---     | --- |
|Kullanıcılar ve gruplar | Isabella Simonsen |
|Bulut uygulamaları | Microsoft Azure Yönetimi |
|Erişim verme | Çok faktörlü kimlik doğrulaması gerektir |
 

![İlke oluşturma](./media/active-directory-conditional-access-app-based-mfa/31.png)

 


**Koşullu erişim ilkesini yapılandırmak için:**

1. Oturum açın, [Azure portal](https://portal.azure.com) genel yönetici olarak.

2. Azure portalında sol gezinti çubuğu tıklatın **Azure Active Directory**. 

    ![Azure Active Directory](./media/active-directory-conditional-access-app-based-mfa/02.png)

3. Üzerinde **Azure Active Directory** sayfasında **Yönet** 'yi tıklatın **koşullu erişim**.

    ![Koşullu erişim](./media/active-directory-conditional-access-app-based-mfa/03.png)
 
4. Üzerinde **koşullu erişim** üstteki araç çubuğunda sayfasını tıklatın **Ekle**.

    ![Ekle](./media/active-directory-conditional-access-app-based-mfa/04.png)

5. Üzerinde **yeni** sayfasında **adı** metin kutusuna, türü **Azure portal erişimi için MFA gerektiren**.

    ![Ad](./media/active-directory-conditional-access-app-based-mfa/05.png)

6. İçinde **atama** 'yi tıklatın **kullanıcılar ve gruplar**.

    ![Kullanıcılar ve gruplar](./media/active-directory-conditional-access-app-based-mfa/06.png)

7. Üzerinde **kullanıcılar ve gruplar** sayfasında, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcılar ve gruplar](./media/active-directory-conditional-access-app-based-mfa/24.png)

    a. Tıklatın **kullanıcıları ve grupları seçin**ve ardından **kullanıcılar ve gruplar**.

    b. **Seç**'e tıklayın.

    c. Üzerinde **seçin** sayfasında **Isabella Simonsen**ve ardından **seçin**.

    d. Üzerinde **kullanıcılar ve gruplar** sayfasında, **Bitti**.

8. Tıklatın **bulut uygulamaları**.

    ![Bulut uygulamaları](./media/active-directory-conditional-access-app-based-mfa/08.png)

9. Üzerinde **bulut uygulamaları** sayfasında, aşağıdaki adımları gerçekleştirin:

    ![Bulut uygulamaları seçin](./media/active-directory-conditional-access-app-based-mfa/26.png)

    a. Tıklatın **uygulamaları**.

    b. **Seç**'e tıklayın.

    c. Üzerinde **seçin** sayfasında **Microsoft Azure Management**ve ardından **seçin**.

    d. Üzerinde **bulut uygulamaları** sayfasında, **Bitti**.


10. İçinde **erişim denetimleri** 'yi tıklatın **Grant**.

    ![Erişim denetimleri](./media/active-directory-conditional-access-app-based-mfa/10.png)

11. Üzerinde **Grant** sayfasında, aşağıdaki adımları gerçekleştirin:

    ![Erişim İzni Verme](./media/active-directory-conditional-access-app-based-mfa/11.png)

    a. Seçin **erişim izni verme**.

    a. Seçin **çok faktörlü kimlik doğrulaması gerektiren**.

    b. **Seç**'e tıklayın.

12. İçinde **ilkesini etkinleştir** 'yi tıklatın **üzerinde**.

    ![İlkeyi etkinleştir](./media/active-directory-conditional-access-app-based-mfa/18.png)

13. **Oluştur**’a tıklayın.


## <a name="evaluate-a-simulated-sign-in"></a>Bir sanal oturum açma değerlendir

Koşullu erişim ilkenizi yapılandırdığınıza göre büyük olasılıkla beklendiği gibi çalışıp çalışmadığını bilmek ister. İlk adım olarak, koşullu erişim ilkesi ne aracı bir oturum açma, test kullanıcısının benzetimini yapmak için kullanın. Bu oturum açma etki ilkelerinizi sahiptir ve benzetimi raporunu oluşturur benzetimi tahmin eder.  

İlke Değerlendirme Aracı ayarlarsanız ne başlatmak için:

- **Isabella Simonsen** kullanıcı olarak 
- **Microsoft Azure Management** bulut uygulaması olarak

 Tıklatarak **ne** gösteren benzetimi raporunu oluşturur:

- **Azure portal erişimi için MFA'ya gerek** altında **uygulanacak ilkeleri** 
- **Çok faktörlü kimlik doğrulaması gerektiren** olarak **verme denetimleri**.

![Ne ilke aracı](./media/active-directory-conditional-access-app-based-mfa/23.png)



**Koşullu erişim ilkenizi değerlendirmek için:**

1. Üzerinde [koşullu erişim - ilkeleri](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ConditionalAccessBlade/Policies) sayfasında üst menüsünde tıklatın **ne**.  
 
    ![What If](./media/active-directory-conditional-access-app-based-mfa/14.png)

2. Tıklatın **kullanıcılar**seçin **Isabella Simonsen**ve ardından **seçin**.

    ![Kullanıcı](./media/active-directory-conditional-access-app-based-mfa/15.png)

2. Bir bulut uygulama seçmek için aşağıdaki adımları gerçekleştirin:

    ![Bulut uygulamaları](./media/active-directory-conditional-access-app-based-mfa/16.png)

    a. Tıklatın **bulut uygulamaları**.

    b. Üzerinde **bulut uygulamalar sayfası**, tıklatın **uygulamaları**.

    c. **Seç**'e tıklayın.

    d. Üzerinde **seçin** sayfasında, Microsoft Azure yönetim ** seçin ve ardından **seçin**.

    e. Bulut uygulamaları sayfasında, tıklatın **Bitti**.

3. Tıklatın **ne**.


## <a name="test-your-conditional-access-policy"></a>Koşullu erişim ilkenizi test

Önceki bölümde, bir sanal oturum açma değerlendirmek öğrendiniz. Benzetim yanı sıra, beklendiği gibi çalıştığından emin olmak için koşullu erişim ilkenizi test etmeniz gerekir. 

İlkeniz test etmek için oturum için açma deneyin, [Azure portal](https://portal.azure.com) kullanarak, **Isabella Simonsen** test hesabı. Hesabınızı ek güvenlik doğrulama kaydınızı ayarlamanızı gerektiren bir iletişim kutusu görmeniz gerekir.

![Multi-factor authentication](./media/active-directory-conditional-access-app-based-mfa/22.png)


## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli olduğunda, test kullanıcısı ve koşullu erişim ilkesini Sil:

- Bir Azure AD kullanıcısının silmek nasıl bilmiyorsanız, bkz: [kullanıcılar Azure AD'den silme](add-users-azure-active-directory.md#delete-users-from-azure-ad).

- İlkeniz silmek için ilkeyi seçin ve ardından **silmek** hızlı erişim araç.

    ![Multi-factor authentication](./media/active-directory-conditional-access-app-based-mfa/33.png)


## <a name="next-steps"></a>Sonraki adımlar

Koşullu erişim hakkında daha fazla bilgi edinmek istiyorsanız, bkz: [Azure Active Directory koşullu erişim](active-directory-conditional-access-azure-portal.md).

