---
title: Hızlı Başlangıç - Azure Active Directory kimlik koruması ile oturum risk algılandığında erişimi engelleme | Microsoft Docs
description: Bu hızlı başlangıçta, oturum açma oturumu riskler için temel engellemek için bir Azure Active Directory (Azure AD) kimlik koruması oturum açma riski koşullu erişim ilkesini nasıl yapılandırabileceğinizi öğrenin.
services: active-directory
keywords: kimlik koruması, uygulamalar, Azure AD ile koşullu erişim, koşullu erişim ilkeleri, şirket kaynaklarına güvenli erişim için koşullu erişim
documentationcenter: ''
author: MarkusVi
manager: mtillman
ms.assetid: ''
ms.service: active-directory
ms.component: identity-protection
ms.topic: article
ms.devlang: na
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/13/2018
ms.author: markvi
ms.reviewer: raluthra
ms.openlocfilehash: cadcc806b9aaeea4f2fc68c911e09c7e35926623
ms.sourcegitcommit: f983187566d165bc8540fdec5650edcc51a6350a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/13/2018
ms.locfileid: "45552464"
---
# <a name="quickstart-block-access-when-a-session-risk-is-detected-with-azure-active-directory-identity-protection"></a>Hızlı Başlangıç: Azure Active Directory kimlik koruması ile oturum risk algılandığında erişimi engelle  

Ortamınızın korumasını sürdürün için şüpheli oturum açarken kullanıcıların engellemek isteyebilirsiniz. Azure Active Directory (Azure AD) kimlik koruması, her oturum açma analiz eder ve bir kullanıcı hesabının meşru sahibi tarafından bir oturum açma denemesi olasılığını gerçekleştirilmedi hesaplar. (Düşük, Orta, yüksek) olasılığını oturum açma risk düzeyini adlı hesaplanan değeri biçiminde belirtilir. Oturum açma riski koşuluna ayarlayarak, belirli oturum açma risk düzeyleri için yanıt oturum açma riski koşullu erişim ilkesi yapılandırabilirsiniz. 

Bu hızlı başlangıçta bir oturum açma riski engelleyen koşullu erişim ilkesini yapılandırmak bir oturum açma, bir orta gösterilmektedir ve oturum açma riski düzeyi algılandı. 

![İlke oluşturma](./media/quickstart-sign-in-risk-policy/1003.png)


Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.



## <a name="prerequisites"></a>Önkoşullar 

Bu öğreticide senaryoyu tamamlamak için gerekir:

- **Bir Azure AD Premium P2 sürümünü erişimi** -Azure AD kimlik koruması, bir Azure AD Premium P2 özelliğidir. 

- **Kimlik koruması** -Bu hızlı başlangıçta senaryoda kimlik korumasının etkinleştirilmesini gerektirir. Kimlik Koruması'nı etkinleştirme bilmiyorsanız, bkz. [etkinleştirme Azure Active Directory kimlik koruması](../identity-protection/enable.md).

- **Tor tarayıcı** - [Tor tarayıcı](https://www.torproject.org/projects/torbrowser.html.en) çevrimiçi gizliliğinizi korumak amacıyla tasarlanmıştır. Kimlik Koruması'nın algıladığı bir oturum açma bir Tor tarayıcıdan **anonim IP adreslerinden oturum açma**, bir orta düzeyde risk düzeyine sahip. Daha fazla bilgi için bkz. [Azure Active Directory risk olayları](../reports-monitoring/concept-risk-events.md).  

- **Alain Charon adlı bir test hesabı** - bir test hesabı oluşturmak için bkz bilmiyorsanız [bulut tabanlı kullanıcılar eklemek](../fundamentals/add-users-azure-active-directory.md#add-cloud-based-users).


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

2. Azure portalında sol gezinti çubuğunda tıklatın **tüm hizmetleri**. 

4. İçinde **filtre** metin kutusuna **kimlik koruması**.

5. Tıklayın **Azure AD kimlik koruması**.   
 
6. Üzerinde **Azure AD kimlik koruması** sayfasında **yapılandırma** bölümünde **oturum açma riski İlkesi**.
 
5. İlke sayfasında içinde **atamaları** bölümünde **kullanıcılar**.

6. Üzerinde **kullanıcılar** sayfasında **Seçili kullanıcılar**.

7. Üzerinde **Seçili kullanıcılar** sayfasında **Alain Charon**ve ardından **seçin**.

8. Üzerinde **kullanıcılar** sayfasında **Bitti**. 

9. İlke sayfasında içinde **atamaları** bölümünde **koşullar**.

10. Üzerinde **koşullar** sayfasında **oturum açma riski**.

11. Üzerinde **oturum açma riski** sayfasında **Orta ve yukarıdaki**ve ardından **seçin**. 

12. Üzerinde **koşullar** sayfasında **Bitti**.

13. İlke sayfasında içinde **denetimleri** bölümünde **erişim**.

14. Üzerinde **erişim** sayfasında **erişime izin ver**seçin **çok faktörlü kimlik doğrulaması gerektiren**ve ardından **seçin**.

15. İlke sayfasında tıklayın **Kaydet**.  


## <a name="test-your-conditional-access-policy"></a>Koşullu erişim ilkenizi test

İlkenizi test etmek için oturum açın deneyin, [Azure portalında](https://portal.azure.com) olarak **Alan Charon** Tor tarayıcıyı kullanarak. Oturum açma denemesi, koşullu erişim ilkesi tarafından engellenmesi gerekir.

![Multi-factor authentication](./media/quickstart-sign-in-risk-policy/203.png)


## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli değilse, test kullanıcısı, Tor tarayıcı silin ve oturum açma riski koşullu erişim ilkesini devre dışı bırakın:

- Bir Azure AD kullanıcı silme işlemini bilmiyorsanız, bkz. [Azure AD'den kullanıcı silme](../fundamentals/add-users-azure-active-directory.md#delete-users-from-azure-ad).

- Tor tarayıcı kaldırma yönergeleri için bkz: [kaldırma](https://tb-manual.torproject.org/en-US/uninstalling.html).


