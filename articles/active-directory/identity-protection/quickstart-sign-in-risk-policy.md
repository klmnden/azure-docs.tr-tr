---
title: Hızlı Başlangıç - Azure Active Directory kimlik koruması ile oturum risk algılandığında erişimi engelleme | Microsoft Docs
description: Bu hızlı başlangıçta, oturum açma oturumu riskler için temel engellemek için bir Azure Active Directory (Azure AD) kimlik koruması oturum açma riski koşullu erişim ilkesini nasıl yapılandırabileceğinizi öğrenin.
services: active-directory
keywords: kimlik koruması, uygulamalar, Azure AD ile koşullu erişim, koşullu erişim ilkeleri, şirket kaynaklarına güvenli erişim için koşullu erişim
documentationcenter: ''
author: MicrosoftGuyJFlo
manager: daveba
ms.assetid: ''
ms.service: active-directory
ms.subservice: identity-protection
ms.topic: article
ms.devlang: na
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/13/2018
ms.author: joflore
ms.reviewer: sahandle
ms.collection: M365-identity-device-management
ms.openlocfilehash: 4f5127342f97a90103ef56efbd7465832440ec0f
ms.sourcegitcommit: 6da4959d3a1ffcd8a781b709578668471ec6bf1b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2019
ms.locfileid: "58521823"
---
# <a name="quickstart-block-access-when-a-session-risk-is-detected-with-azure-active-directory-identity-protection"></a>Hızlı Başlangıç: Azure Active Directory kimlik koruması ile oturum risk algılandığında erişimi engelleme  

Ortamınızın korumasını sürdürün için şüpheli oturum açarken kullanıcıların engellemek isteyebilirsiniz. Azure Active Directory (Azure AD) kimlik koruması, her oturum açma analiz eder ve bir kullanıcı hesabının meşru sahibi tarafından bir oturum açma denemesi olasılığını gerçekleştirilmedi hesaplar. (Düşük, Orta, yüksek) olasılığını oturum açma risk düzeyini adlı hesaplanan değeri biçiminde belirtilir. Oturum açma riski koşuluna ayarlayarak, belirli oturum açma risk düzeyleri için yanıt oturum açma riski koşullu erişim ilkesi yapılandırabilirsiniz. 

Bu hızlı başlangıçta bir oturum açma riski engelleyen koşullu erişim ilkesini yapılandırmak bir oturum açma, bir orta gösterilmektedir ve oturum açma riski düzeyi algılandı. 

![İlke oluşturma](./media/quickstart-sign-in-risk-policy/1004.png)


Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.



## <a name="prerequisites"></a>Önkoşullar 

Bu öğreticide senaryoyu tamamlamak için şunlar gereklidir:

- **Bir Azure AD Premium P2 sürümünü erişimi** -Azure AD kimlik koruması, bir Azure AD Premium P2 özelliğidir. 

- **Kimlik koruması** -Bu hızlı başlangıçta senaryoda kimlik korumasının etkinleştirilmesini gerektirir. Kimlik Koruması'nı etkinleştirme bilmiyorsanız, bkz. [etkinleştirme Azure Active Directory kimlik koruması](../identity-protection/enable.md).

- **Tor tarayıcı** - [Tor tarayıcı](https://www.torproject.org/projects/torbrowser.html.en) çevrimiçi gizliliğinizi korumak amacıyla tasarlanmıştır. Kimlik Koruması'nın algıladığı bir oturum açma bir Tor tarayıcıdan **anonim IP adreslerinden oturum açma**, bir orta düzeyde risk düzeyine sahip. Daha fazla bilgi için bkz. [Azure Active Directory risk olayları](../reports-monitoring/concept-risk-events.md).  

- **Alain Charon adlı bir test hesabı** - bir test hesabı oluşturmak için bkz bilmiyorsanız [yeni kullanıcı ekleme](../fundamentals/add-users-azure-active-directory.md#add-a-new-user).


## <a name="test-your-sign-in"></a>Oturum açma testi 

Bu adımın amacı, test hesabınızı Tor tarayıcı kullanarak kiracınızda erişebildiğinizden emin olmaktır.

**Oturum açma işleminizi test etmek için:**

1. Oturum açın, [Azure portalında](https://portal.azure.com) olarak **Alain Charon**.

2. Oturumunuzu kapatın. 


## <a name="create-your-conditional-access-policy"></a>Koşullu erişim ilkenizi oluşturun 

Algılanan bir oluşturmak için bir oturum açma Tor tarayıcısından Bu hızlı başlangıçta bir senaryo kullanır **anonim IP adreslerinden oturum açma** risk olayı. Bu risk olayı risk düzeyini Orta'dır. Bu risk olayına yanıt olarak oturum açma riski koşuluna Orta ayarlayın. 

Bu bölümde, gerekli oturum açma riski koşullu erişim ilkesi oluşturma işlemi gösterilmektedir. İlkenizde, ayarlayın:

|Ayar |Değer|
|---     | --- |
| Kullanıcılar  | Alain Charon  |
| Koşullar | Oturum açma riski, Orta ve üzeri |
| Denetimler | Erişimi engelle |
 

![İlke oluşturma](./media/quickstart-sign-in-risk-policy/201.png)

 


**Koşullu erişim ilkenizi yapılandırmak için:**

1. Oturum açın, [Azure portalında](https://portal.azure.com) genel yönetici olarak.

2. Git [Azure AD kimlik koruması sayfa](https://portal.azure.com/#blade/Microsoft_AAD_ProtectionCenter/IdentitySecurityDashboardMenuBlade/Overview).
 
3. Üzerinde **Azure AD kimlik koruması** sayfasında **yapılandırma** bölümünde **oturum açma riski İlkesi**.
 
4. İlke sayfasında içinde **atamaları** bölümünde **kullanıcılar**.

5. Üzerinde **kullanıcılar** sayfasında **Seçili kullanıcılar**.

6. Üzerinde **Seçili kullanıcılar** sayfasında **Alain Charon**ve ardından **seçin**.

7. Üzerinde **kullanıcılar** sayfasında **Bitti**. 

8. İlke sayfasında içinde **atamaları** bölümünde **koşullar**.

9. Üzerinde **koşullar** sayfasında **oturum açma riski**.

10. Üzerinde **oturum açma riski** sayfasında **Orta ve yukarıdaki**ve ardından **seçin**. 

11. Üzerinde **koşullar** sayfasında **Bitti**.

12. İlke sayfasında içinde **denetimleri** bölümünde **erişim**.

13. Üzerinde **erişim** sayfasında **erişime izin ver**seçin **çok faktörlü kimlik doğrulaması gerektiren**ve ardından **seçin**.

14. İlke sayfasında tıklayın **Kaydet**.  


## <a name="test-your-conditional-access-policy"></a>Koşullu erişim ilkenizi test etme

İlkenizi test etmek için oturum açın deneyin, [Azure portalında](https://portal.azure.com) olarak **Alan Charon** Tor tarayıcıyı kullanarak. Oturum açma denemesi, koşullu erişim ilkesi tarafından engellenmesi gerekir.

![Multi-factor authentication](./media/quickstart-sign-in-risk-policy/203.png)


## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli değilse, test kullanıcısı, Tor tarayıcı silin ve oturum açma riski koşullu erişim ilkesini devre dışı bırakın:

- Bir Azure AD kullanıcı silme işlemini bilmiyorsanız, bkz. [ekleme veya kullanıcıları silmek](../fundamentals/add-users-azure-active-directory.md#delete-a-user).

- Tor tarayıcı kaldırma yönergeleri için bkz: [kaldırma](https://tb-manual.torproject.org/uninstalling/).


