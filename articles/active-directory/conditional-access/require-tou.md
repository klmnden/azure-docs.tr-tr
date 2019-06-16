---
title: Hızlı Başlangıç - Azure Active Directory koşullu erişim tarafından korunan bulut uygulamaları erişmeden önce kabul edilmesi için kullanım koşullarını gerektiren | Microsoft Docs
description: Bu hızlı başlangıçta, Azure Active Directory koşullu erişim tarafından seçilen bulut uygulamalarına erişim sağlanmadan önce kullanım koşullarınızı kabul edildiğini nasıl gerektirebilir öğrenin.
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: quickstart
ms.date: 12/14/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: calebb
ms.collection: M365-identity-device-management
ms.openlocfilehash: 2a3523a050a021f3a98c144efe14d692704fba63
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67112207"
---
# <a name="quickstart-require-terms-of-use-to-be-accepted-before-accessing-cloud-apps"></a>Hızlı Başlangıç: Bulut uygulamaları erişmeden önce kabul edilmesi için kullanım koşullarını gerektirir

Ortamınızdaki belirli bulut uygulamalarına erişmeden önce kullanıcılardan (ToU) kullanım koşullarınızı kabul eden bir formun onay almak isteyebilirsiniz. Azure Active Directory (Azure AD) koşullu erişim aşağıdakileri sağlar:

- Kullanım koşullarını yapılandırmak için basit bir yöntem
- Koşullu erişim ilkesi aracılığıyla kullanım koşullarınızı kabul etmesini Gerektir seçeneği  

Bu hızlı başlangıçta nasıl yapılandırılacağını gösteren bir [Azure AD koşullu erişim ilkesi](../active-directory-conditional-access-azure-portal.md) seçili bulut uygulaması ortamınızda kabul edilmesi bir ToU gerektirir.

![İlke oluşturma](./media/require-tou/5555.png)

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="prerequisites"></a>Önkoşullar

Bu hızlı başlangıçta senaryoyu tamamlamak için gerekir:

- **Bir Azure AD Premium sürümü için erişim** -Azure AD koşullu erişim, bir Azure AD Premium özelliği.
- **Adlı bir test hesabı Isabella Simonsen** - bir test hesabı oluşturmak için bkz bilmiyorsanız [bulut tabanlı kullanıcılar eklemek](../fundamentals/add-users-azure-active-directory.md#add-a-new-user).

## <a name="test-your-sign-in"></a>Oturum açma testi

Bu adımın amacı, bir koşullu erişim ilkesi olmadan oturum açma deneyimi izlenimi almaktır.

**Oturum açma işleminizi test etmek için:**

1. Oturum açın, [Azure portalında](https://portal.azure.com/) Isabella Simonsen olarak.
1. Oturumunuzu kapatın.

## <a name="create-your-terms-of-use"></a>Kullanım koşullarınızın oluşturma

Bu bölümde örnek bir ToU oluşturma adımlarını sağlar. İçin bir değer bir ToU oluşturduğunuzda, seçtiğiniz **zorla koşullu erişim ilkesi şablonlarıyla**. Seçme **özel İlkesi** , ToU oluşturulduktan hemen sonra yeni bir koşullu erişim ilkesi oluşturmak için iletişim kutusu açılır.

**Kullanım koşullarınızın oluşturmak için:**

1. Microsoft Word'de yeni bir belge oluşturun.

1. Tür **My kullanım koşullarını**ve belge olarak bilgisayarınıza kaydedin **mytou.pdf**.

1. Oturum açın, [Azure portalında](https://portal.azure.com) genel yönetici, güvenlik yöneticisi veya koşullu erişim Yöneticisi olarak.

1. Azure portalında sol gezinti çubuğunda tıklatın **Azure Active Directory**.

   ![Azure Active Directory](./media/require-tou/02.png)

1. Üzerinde **Azure Active Directory** sayfasında **güvenlik** bölümünde **koşullu erişim**.

   ![Koşullu Erişim](./media/require-tou/03.png)

1. İçinde **Yönet** bölümünde **kullanım koşullarını**.

   ![Kullanım koşulları](./media/require-tou/04.png)

1. Üstteki menüden **yeni terimler**.

   ![Kullanım koşulları](./media/require-tou/05.png)

1. Üzerinde **yeni kullanım koşulları** sayfası:

   ![Kullanım koşulları](./media/require-tou/112.png)

   1. İçinde **adı** metin kutusuna **My TOU**.

   1. İçinde **görünen ad** metin kutusuna **My TOU**.

   1. PDF dosyasını kullanma koşullarınızın karşıya yükleyin.

   1. Olarak **dil**seçin **İngilizce**.

   1. Olarak **kullanıcıların kullanım koşullarını genişletmesini gerekli kıl**seçin **üzerinde**.

   1. Olarak **zorla koşullu erişim ilkesi şablonlarıyla**seçin **özel İlkesi**.

   1. **Oluştur**’a tıklayın.

## <a name="create-your-conditional-access-policy"></a>Koşullu erişim ilkenizi oluşturun

Bu bölümde, gerekli koşullu erişim ilkesi oluşturma işlemi gösterilmektedir. Bu hızlı başlangıçta bir senaryo kullanır:

- Azure portalı, kullanım koşullarını kabul edilmesini gerektiren bir bulut uygulaması için yer tutucu olarak. 
- Koşullu erişim ilkesini test etmek için örnek kullanıcı.  

İlkenizde, ayarlayın:

| Ayar | Değer |
| --- | --- |
| Kullanıcılar ve gruplar | Isabella Simonsen |
| Bulut uygulamaları | Microsoft Azure Yönetimi |
| Erişim verme | My TOU |

![İlke oluşturma](./media/require-tou/1234.png)

**Koşullu erişim ilkenizi yapılandırmak için:**

1. Üzerinde **yeni** sayfasında **adı** metin kutusuna **gerektiren TOU Isabella için**.

   ![Ad](./media/require-tou/71.png)

1. İçinde **atama** bölümünde **kullanıcılar ve gruplar**.

   ![Kullanıcılar ve gruplar](./media/require-tou/06.png)

1. Üzerinde **kullanıcılar ve gruplar** sayfası:

   ![Kullanıcılar ve gruplar](./media/require-tou/24.png)

   1. Tıklayın **kullanıcıları ve grupları seçin**ve ardından **kullanıcılar ve gruplar**.

   1. **Seç**'e tıklayın.

   1. Üzerinde **seçin** sayfasında **Isabella Simonsen**ve ardından **seçin**.

   1. Üzerinde **kullanıcılar ve gruplar** sayfasında **Bitti**.

1. Tıklayın **bulut uygulamaları**.

   ![Bulut uygulamaları](./media/require-tou/08.png)

1. Üzerinde **bulut uygulamaları** sayfası:

   ![Bulut uygulamalarını seçin](./media/require-tou/26.png)

   1. Tıklayın **uygulamaları Seç**.

   1. **Seç**'e tıklayın.

   1. Üzerinde **seçin** sayfasında **Microsoft Azure Management**ve ardından **seçin**.

   1. Üzerinde **bulut uygulamaları** sayfasında **Bitti**.

1. İçinde **erişim denetimleri** bölümünde **Grant**.

   ![Erişim denetimleri](./media/require-tou/10.png)

1. Üzerinde **Grant** sayfası:

   ![Verme](./media/require-tou/111.png)

   1. Seçin **erişim ver**.

   1. Seçin **My TOU**.

   1. **Seç**'e tıklayın.

1. İçinde **ilkesini etkinleştir** bölümünde **üzerinde**.

   ![İlkeyi etkinleştirme](./media/require-tou/18.png)

1. **Oluştur**’a tıklayın.

## <a name="evaluate-a-simulated-sign-in"></a>Bir sanal oturum değerlendir

Koşullu erişim ilkenizi yapılandırdığınıza göre büyük olasılıkla beklenen şekilde çalışıp çalışmadığını bilmek ister. İlk adım, koşullu erişim ilkesi durum aracı bir oturum açma, test kullanıcının benzetimini yapmak için kullanın. Benzetim, bu oturum açma işleminin ilkeleriniz üzerindeki etkisini tahmin eder ve bir benzetim raporu oluşturur.  

İlke değerlendirmesi aracı, ayarlarsanız ne başlatmak için:

- **Isabella Simonsen** kullanıcı olarak
- **Microsoft Azure Management** bulut uygulaması olarak

Tıklayarak **ne** gösteren bir simülasyon raporu oluşturur:

- **İçin Isabella TOU gerektiren** altında **uygulanacak ilkeler**
- **My TOU** olarak **verme denetimleri**.

![Durum ilkesi aracı](./media/require-tou/79.png)

**Koşullu erişim ilkenizi değerlendirmek için:**

1. Üzerinde [koşullu erişim - ilkeleri](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ConditionalAccessBlade/Policies) sayfasında, üstteki menüde **ne**.  

   ![Ya](./media/require-tou/14.png)

1. Tıklayın **kullanıcılar**seçin **Isabella Simonsen**ve ardından **seçin**.

   ![Kullanıcı](./media/require-tou/15.png)

1. Bulut uygulaması seçmek için:

   ![Bulut uygulamaları](./media/require-tou/16.png)

   1. Tıklayın **bulut uygulamaları**.

   1. Üzerinde **bulut uygulamaları sayfası**, tıklayın **uygulamaları Seç**.

   1. **Seç**'e tıklayın.

   1. Üzerinde **seçin** sayfasında **Microsoft Azure Management**ve ardından **seçin**.

   1. Bulut uygulamaları sayfasında tıklayın **Bitti**.

1. Tıklayın **ne**.

## <a name="test-your-conditional-access-policy"></a>Koşullu erişim ilkenizi test

Önceki bölümde, bir sanal oturum değerlendirilecek öğrendiniz. Bir simülasyon ek olarak, koşullu erişim ilkenizi, beklendiği gibi çalıştığından emin olmak için sınamalısınız.

İlkenizi test etmek için oturum açın deneyin, [Azure portalında](https://portal.azure.com) kullanarak, **Isabella Simonsen** test hesabı. Kullanım koşullarınızı kabul etmenizi gerektiren bir iletişim kutusu görüntülenir.

![Kullanım koşulları](./media/require-tou/57.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli değilse, test kullanıcısı ve koşullu erişim ilkesini Sil:

- Bir Azure AD kullanıcı silme işlemini bilmiyorsanız, bkz. [Azure AD'den kullanıcı silme](../fundamentals/add-users-azure-active-directory.md#delete-a-user).

- İlkeniz silinecek ilkenizi seçin ve ardından **Sil** hızlı erişim araç çubuğu.

    ![Multi-factor authentication](./media/require-tou/33.png)

- Kullanım koşullarınızın silmek için onu seçin ve ardından **koşulları Sil** araç çubuğunda üstte.

    ![Multi-factor authentication](./media/require-tou/29.png)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Belirli uygulamalar için mfa'yı gerekli](app-based-mfa.md)
> [oturumu risk algılandığında erişimi engelle](app-sign-in-risk.md)
