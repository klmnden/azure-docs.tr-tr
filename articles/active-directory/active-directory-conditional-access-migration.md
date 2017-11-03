---
title: "Azure portalında Klasik ilkelerine | Microsoft Docs"
description: "Azure portalında Klasik ilkelerini geçirin."
services: active-directory
keywords: "uygulamaları, Azure AD ile koşullu erişim, koşullu erişim ilkeleri, şirket kaynaklarına güvenli erişim için koşullu erişim"
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: 8c1d978f-e80b-420e-853a-8bbddc4bcdad
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 10/23/2017
ms.author: markvi
ms.reviewer: nigu
ms.openlocfilehash: c584eddb5542c2c49d08d35bcaf8e7acb5c5b83a
ms.sourcegitcommit: e6029b2994fa5ba82d0ac72b264879c3484e3dd0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/24/2017
---
# <a name="migrate-classic-policies-in-the-azure-portal"></a>Azure portalında Klasik ilkelerine 


[Koşullu erişim](active-directory-conditional-access-azure-portal.md) olduğunu denetlemek için nasıl sağlayan Azure Active directory (Azure AD) yeteneğini yetkili kullanıcılara erişimi, bulut uygulamalarınızı. Amacı hala aynı olsa da, yeni Azure portalına sürümü de koşullu erişimin nasıl çalıştığı için önemli geliştirmeler anlatılmıştır. Azure portal dışında yapılandırdığınız koşullu erişim ilkeleri, Azure portalında oluşturduğunuz yeni ilkeleriyle bulunabilir. Değil devre dışı bırakma veya bunları kaldırma sürece, bunlar hala ortamınızda uygulanır. Ancak, Klasik ilkelerinizin yeni Azure AD için koşullu geçirmek öneririz çünkü erişim ilkeleri:

- Yeni ilkeler Klasik ilkeleriyle işleyemedi adresi senaryoları için etkinleştirin.

- Bunları birleştirerek yönetmek zorunda ilkeleri sayısını azaltabilirsiniz.   

Bu konu, varolan Klasik ilkelerinizi yeni geçiş ile yeni Azure yönetmenize yardımcı olan AD koşullu erişim ilkeleri.


## <a name="classic-policies"></a>Klasik ilkeleri

Azure AD için koşullu erişim ilkeleri ve değil oluşturduğunuz Azure portalında Intune olarak da bilinen **Klasik ilkeleri**. Klasik ilkelerinizi geçirmek için Klasik Azure portalına erişiminiz olması gerekmez. Azure portal ile sağlayan bir [ **Klasik ilkeleri (Önizleme)** Görünüm](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ConditionalAccessBlade/ClassicPolicies) Klasik ilkelerinizi inceleyin olanak sağlar.

![Azure Active Directory](./media/active-directory-conditional-access-migration/33.png)


### <a name="open-a-classic-policy"></a>Klasik İlkesi'ni açın

**Klasik İlkesi'ni açmak için:**

1. İçinde [Azure portal](https://portal.azure.com), sol gezinti çubuğu üzerinde tıklatın **Azure Active Directory**.

    ![Azure Active Directory](./media/active-directory-conditional-access-migration/01.png)

2. Üzerinde **Azure Active Directory** sayfasında **Yönet** 'yi tıklatın **koşullu erişim**.

    ![Koşullu erişim](./media/active-directory-conditional-access-migration/02.png)
 
2. Üzerinde **koşullu erişim - ilkeleri** sayfasında **Yönet** 'yi tıklatın **Klasik ilkeleri (Önizleme)**.

3. Klasik ilkeleri listesinden önem verdiğiniz ilkesini seçin.   

    ![Koşullu erişim](./media/active-directory-conditional-access-migration/34.png)



## <a name="azure-ad-conditional-access-policies"></a>Azure AD koşullu erişim ilkeleri

Bu konu, etkinleştirmeniz Klasik ilkelerinizi Azure AD ile koşullu erişim hakkında bilgi sahibi olmadan geçirmek ayrıntılı adımlar ilkeleri sağlar. Ancak, temel kavramlar ve terimler hakkında bilginiz olması Azure AD koşullu erişim geçiş deneyiminizi artırmaya yardımcı olur.

Bkz.:

- [Azure Active Directory'de koşullu erişim](active-directory-conditional-access-azure-portal.md) temel kavramları ve terminolojiyi hakkında bilgi edinmek için

- [Azure Active Directory'de koşullu erişimi kullanmaya başlama](active-directory-conditional-access-azure-portal-get-started.md) Azure portalında kullanıcı arabirimi tanımak için


 





## <a name="multi-factor-authentication-policy"></a>Çok faktörlü kimlik doğrulama İlkesi 

Bu örnek, çok faktörlü kimlik doğrulaması ** bulut uygulaması için gerektiren klasik bir ilke geçirmek nasıl gösterir. 

![Azure Active Directory](./media/active-directory-conditional-access-migration/33.png)


**Klasik bir ilke geçirmek için:**

1. [Klasik İlkesi](#open-a-classic-policy) almak için yapılandırma ayarları.
2. Yeni bir Klasik ilkeniz değiştirmek için Azure AD koşullu erişim ilkesi. 


### <a name="create-a-new-conditional-access-policy"></a>Yeni bir koşullu erişim ilkesi oluşturma


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

11. Klasik ilkeyi devre dışı bırakır. 

    ![Koşullu erişim](./media/active-directory-conditional-access-migration/38.png)



 


## <a name="next-steps"></a>Sonraki adımlar

- Koşullu erişim ilkesini yapılandırma hakkında bilmek istiyorsanız [Azure Active Directory'de koşullu erişimi kullanmaya başlama](active-directory-conditional-access-azure-portal-get-started.md).

- Ortamınız için koşullu erişim ilkelerini yapılandırma için hazır olup olmadığını görmek [Azure Active Directory'de koşullu erişim için en iyi uygulamaları](active-directory-conditional-access-best-practices.md). 
