---
title: Hızlı Başlangıç - Azure Active Directory koşullu erişimiyle belirli uygulamalar için çok faktörlü kimlik doğrulaması (MFA) gerekli | Microsoft Docs
description: Bu hızlı başlangıçta, Azure Active Directory (Azure AD) koşullu erişim kullanarak erişilen bulut uygulaması türü için kimlik doğrulaması gereksinimlerinizi nasıl bağlayabilirsiniz öğrenin.
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: quickstart
ms.date: 01/30/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: calebb
ms.collection: M365-identity-device-management
ms.openlocfilehash: cd5ab513034d6e2946dcb31f3a31dbf86f14873e
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60413173"
---
# <a name="quickstart-require-mfa-for-specific-apps-with-azure-active-directory-conditional-access"></a>Hızlı Başlangıç: Azure Active Directory koşullu erişimiyle belirli uygulamalar için MFA gerektirme

Kullanıcıların oturum açma deneyimini basitleştirmek için bulut uygulamalarınızdaki kullanıcı adı ve parola kullanarak oturum açmak izin vermek isteyebilirsiniz. Ancak, birçok ortamda, daha güçlü bir form çok faktörlü kimlik doğrulaması (MFA) gibi hesap doğrulama gerektirecek şekilde tavsiye olduğu en az birkaç uygulama vardır. Bu, örneğin true, kuruluşunuzun e-posta sisteminize veya ik uygulamalarınıza erişim için olabilir. Azure Active Directory'de (Azure AD), bir koşullu erişim ilkesi ile bu hedefe gerçekleştirebilirsiniz.

Bu hızlı başlangıçta nasıl yapılandırılacağını gösteren bir [Azure AD koşullu erişim ilkesi](../active-directory-conditional-access-azure-portal.md) , seçili bulut uygulaması ortamınızda çok faktörlü kimlik doğrulaması gerektirir.

![Azure portalında koşullu erişim ilkesi örneği](./media/app-based-mfa/32.png)

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="prerequisites"></a>Önkoşullar

Bu hızlı başlangıçta senaryoyu tamamlamak için gerekir:

- **Bir Azure AD Premium sürümü için erişim** -Azure AD koşullu erişimi olan bir Azure AD Premium özelliği.

- **Adlı bir test hesabı Isabella Simonsen** - bir test hesabı oluşturmak için bkz bilmiyorsanız [bulut tabanlı kullanıcılar eklemek](../fundamentals/add-users-azure-active-directory.md#add-a-new-user).

Bu hızlı başlangıçta bir senaryoda, kullanıcı test hesabınız için mfa'yı etkinleştirilmedi gerektirir. Daha fazla bilgi için [bir kullanıcı için iki aşamalı doğrulama gerektirme](../authentication/howto-mfa-userstates.md).

## <a name="test-your-sign-in"></a>Oturum açma testi

Bu adımın amacı, bir koşullu erişim ilkesi olmadan oturum açma deneyimi izlenimi almaktır.

**Ortamınızı başlatmak için:**

1. Isabella Simonsen, Azure portalında oturum açın.
1. Oturumunuzu kapatın.

## <a name="create-your-conditional-access-policy"></a>Koşullu erişim ilkenizi oluşturun

Bu bölümde, gerekli koşullu erişim ilkesi oluşturma işlemi gösterilmektedir. Bu hızlı başlangıçta bir senaryo kullanır:

- Azure portalı MFA gerektiren bir bulut uygulaması için yer tutucu olarak. 
- Koşullu erişim ilkesini test etmek için örnek kullanıcı.  

İlkenizde, ayarlayın:

| Ayar | Değer |
| --- | --- |
| Kullanıcılar ve gruplar | Isabella Simonsen |
| Bulut uygulamaları | Microsoft Azure Yönetimi |
| Erişim verme | Çok faktörlü kimlik doğrulaması gerektir |

![Koşullu erişim ilkesi Genişletilmiş](./media/app-based-mfa/31.png)

**Koşullu erişim ilkenizi yapılandırmak için:**

1. [Azure portalınızda](https://portal.azure.com) genel yönetici, güvenlik yöneticisi veya koşullu erişim yöneticisi olarak oturum açın.

1. Azure portalında sol gezinti çubuğunda tıklatın **Azure Active Directory**.

   ![Azure Active Directory](./media/app-based-mfa/02.png)

1. Üzerinde **Azure Active Directory** sayfasında **güvenlik** bölümünde **koşullu erişim**.

   ![Koşullu erişim](./media/app-based-mfa/03.png)

1. Üzerinde **koşullu erişim** sayfasında, üstteki araç çubuğunda tıklatın **yeni ilke**.

   ![Ekle](./media/app-based-mfa/04.png)

1. Üzerinde **yeni** sayfasında **adı** metin kutusuna **Azure portal erişimi için MFA gerektiren**.

   ![Ad](./media/app-based-mfa/05.png)

1. İçinde **atama** bölümünde **kullanıcılar ve gruplar**.

   ![Kullanıcılar ve gruplar](./media/app-based-mfa/06.png)

1. Üzerinde **kullanıcılar ve gruplar** sayfasında, aşağıdaki adımları gerçekleştirin:

   ![Kullanıcılar ve gruplar](./media/app-based-mfa/24.png)

   1. Tıklayın **kullanıcıları ve grupları seçin**ve ardından **kullanıcılar ve gruplar**.

   1. **Seç**'e tıklayın.

   1. Üzerinde **seçin** sayfasında **Isabella Simonsen**ve ardından **seçin**.

   1. Üzerinde **kullanıcılar ve gruplar** sayfasında **Bitti**.

1. Tıklayın **bulut uygulamaları**.

   ![Bulut uygulamaları](./media/app-based-mfa/08.png)

1. Üzerinde **bulut uygulamaları** sayfasında, aşağıdaki adımları gerçekleştirin:

   ![Bulut uygulamalarını seçin](./media/app-based-mfa/26.png)

   1. Tıklayın **uygulamaları Seç**.

   1. **Seç**'e tıklayın.

   1. Üzerinde **seçin** sayfasında **Microsoft Azure Management**ve ardından **seçin**.

   1. Üzerinde **bulut uygulamaları** sayfasında **Bitti**.

1. İçinde **erişim denetimleri** bölümünde **Grant**.

   ![Erişim denetimleri](./media/app-based-mfa/10.png)

1. Üzerinde **Grant** sayfasında, aşağıdaki adımları gerçekleştirin:

   ![Erişim İzni Verme](./media/app-based-mfa/11.png)

   1. Seçin **erişim ver**.

   1. Seçin **çok faktörlü kimlik doğrulaması gerektiren**.

   1. **Seç**'e tıklayın.

1. İçinde **ilkesini etkinleştir** bölümünde **üzerinde**.

   ![İlkeyi etkinleştir](./media/app-based-mfa/18.png)

1. **Oluştur**’a tıklayın.

## <a name="evaluate-a-simulated-sign-in"></a>Bir sanal oturum değerlendir

Koşullu erişim ilkenizi yapılandırdığınıza göre şimdi beklenen şekilde çalışıp çalışmadığını kontrol etmek isteyebilirsiniz. İlk adım, koşullu erişim ilkesi durum aracı bir oturum açma, test kullanıcının benzetimini yapmak için kullanın. Benzetim, bu oturum açma işleminin ilkeleriniz üzerindeki etkisini tahmin eder ve bir benzetim raporu oluşturur.  

İlke değerlendirmesi aracı, ayarlarsanız ne başlatmak için:

- **Isabella Simonsen** kullanıcı olarak
- **Microsoft Azure Management** bulut uygulaması olarak

Tıklayarak **ne** gösteren bir simülasyon raporu oluşturur:

- **Azure portala erişim için mfa'yı gerekli** altında **uygulanacak ilkeler**
- **Çok faktörlü kimlik doğrulaması gerektiren** olarak **denetimleri ver**.

![Durum ilkesi aracı](./media/app-based-mfa/23.png)

**Koşullu erişim ilkenizi değerlendirmek için:**

1. Üzerinde [koşullu erişim - ilkeleri](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ConditionalAccessBlade/Policies) sayfasında, üstteki menüde **ne**.  

   ![What If](./media/app-based-mfa/14.png)

1. Tıklayın **kullanıcılar**seçin **Isabella Simonsen**ve ardından **seçin**.

   ![Kullanıcı](./media/app-based-mfa/15.png)

1. Bulut uygulaması seçmek için aşağıdaki adımları gerçekleştirin:

   ![Bulut uygulamaları](./media/app-based-mfa/16.png)

   1. Tıklayın **bulut uygulamaları**.

   1. Üzerinde **bulut uygulamaları sayfası**, tıklayın **uygulamaları Seç**.

   1. **Seç**'e tıklayın.

   1. Üzerinde **seçin** sayfasında **Microsoft Azure Management**ve ardından **seçin**.

   1. Bulut uygulamaları sayfasında tıklayın **Bitti**.

1. Tıklayın **ne**.

## <a name="test-your-conditional-access-policy"></a>Koşullu erişim ilkenizi test etme

Önceki bölümde, bir sanal oturum değerlendirilecek öğrendiniz. Bir simülasyon ek olarak, koşullu erişim ilkenizi, beklendiği gibi çalıştığından emin olmak için sınamalısınız.

İlkenizi test etmek için oturum açın deneyin, [Azure portalında](https://portal.azure.com) kullanarak, **Isabella Simonsen** test hesabı. Hesabınızı ek güvenlik doğrulaması için ayarlamanızı gerektiren bir iletişim kutusu görüntülenir.

![Multi-factor authentication](./media/app-based-mfa/22.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli değilse, test kullanıcısı ve koşullu erişim ilkesini Sil:

- Bir Azure AD kullanıcı silme işlemini bilmiyorsanız, bkz. [Azure AD'den kullanıcı silme](../fundamentals/add-users-azure-active-directory.md#delete-a-user).
- İlkeniz silinecek ilkenizi seçin ve ardından **Sil** hızlı erişim araç çubuğu.

    ![Multi-factor authentication](./media/app-based-mfa/33.png)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Kabul edilmesi için kullanım koşullarını gerektiren](require-tou.md)
> [oturumu risk algılandığında erişimi engelle](app-sign-in-risk.md)
