---
title: 'Öğretici: Azure Active Directory kutusuyla tümleştirme | Microsoft Docs'
description: Çoklu oturum açma kutusunu ve Azure Active Directory arasında yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 3b565c8d-35e2-482a-b2f4-bf8fd7d8731f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 1/8/2017
ms.author: jeedes
ms.openlocfilehash: daad9104798dc02b479b4e022287c3630e4a67a0
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2018
---
# <a name="integrate-azure-active-directory-with-box"></a>Azure Active Directory kutusuyla tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) tümleştirme kutusuyla öğrenin.

Azure AD kutusuyla tümleştirerek, aşağıdaki faydaları alın:

- Kutusuna erişimi olan Azure AD'de kontrol edebilirsiniz.
- Otomatik olarak kutusuna (çoklu oturum açma veya SSO) ile Azure AD hesaplarına oturum, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınızı bir merkezi konumda, Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında bilgi edinmek için [uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirme kutusuyla yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Kutusunu SSO etkin bir abonelik

> [!NOTE]
> Bu öğreticide adımları test ettiğinizde, bunu yapmanızı öneririz *değil* bir üretim ortamında kullanın.

Bu öğreticide adımları test etmek için aşağıdaki önerileri uygulayın:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, şunları yapabilirsiniz [bir aylık deneme sürümünü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. 

Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden kutusu ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="add-box-from-the-gallery"></a>Galeriden kutusu ekleme
Azure ad tümleştirme kutusuyla yapılandırmak için kutusunda aşağıdakileri yaparak Galeriden yönetilen SaaS uygulamaları listenize ekleyin:

1. İçinde [Azure portal](https://portal.azure.com), sol bölmede seçin **Azure Active Directory**. 

    ![Azure Active Directory düğmesi][1]

2. Seçin **kurumsal uygulamalar** > **tüm uygulamaları**.

    !["Kurumsal uygulamalar" penceresi][2]
    
3. Yeni bir uygulama eklemek için seçin **yeni uygulama** pencerenin üstündeki düğmesi.

    !["Yeni uygulama" düğmesi][3]

4. Arama kutusuna **kutusunu**seçin **kutusunu** sonuçları listesi ve ardından **Ekle**.

    ![Sonuçlar listesinde kutusu](./media/active-directory-saas-box-tutorial/tutorial_box_search.png)
### <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon." olarak adlandırılan bir test kullanıcı tabanlı kutusuyla test etme

Tekli çalışmaya oturum için Azure AD'de kutusunu kullanıcı ve kendisine karşılık gelen tanımlamak Azure AD gerekiyor. Diğer bir deyişle, bir Azure AD kullanıcısının kutusunda aynı kullanıcı arasında bir bağlantı ilişkisi kurulmalıdır.

Bağlantı ilişkisi oluşturmak için bir kutu olarak atamak *kullanıcıadı* değerini *kullanıcı adı* Azure AD'de.

Yapılandırmak ve Azure AD çoklu oturum açma kutusuyla sınamak için sonraki beş bölümlerde yapı taşları tamamlayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma aşağıdakileri yaparak kutusunu uygulamanızda yapılandırın:

1. Azure portalında içinde **kutusunu** uygulama tümleştirmesi penceresinde, seçin **çoklu oturum açma**.

    !["Çoklu oturum açmayı" bağlantı][4]

2. İçinde **çoklu oturum açma** penceresi, **tek oturum açma modu** kutusunda **SAML tabanlı oturum açma**.
 
    !["Çoklu oturum açmayı" penceresi](./media/active-directory-saas-box-tutorial/tutorial_box_samlbase.png)

3. Altında **kutusunu etki alanı ve URL'leri**, aşağıdakileri yapın:

    !["Etki alanı ve URL'ler kutusuna" tek oturum açma bilgileri](./media/active-directory-saas-box-tutorial/url3.png)

    a. İçinde **oturum açma URL'si** kutusuna bir URL aşağıdaki biçimde yazın: *https://\<alt etki alanı >. box.com*.

    b. İçinde **tanımlayıcısı** metin kutusuna, türü **box.net**.
     
    > [!NOTE] 
    > Yukarıdaki değerleri gerçek değildir. Bunları tanımlayıcısı ve gerçek oturum açma URL'si ile güncelleştirin. Değerleri almak için başvurun [kutusunu istemci destek ekibi](https://community.box.com/t5/custom/page/page-id/submit_sso_questionaire). 

4. Altında **SAML imzalama sertifikası**seçin **meta veri XML**ve meta veri dosyası, bilgisayarınıza kaydedin.

    ![Sertifika indirme bağlantısı](./media/active-directory-saas-box-tutorial/tutorial_box_certificate.png) 

5. **Kaydet**’i seçin.

    ![Oturum açma tek Kaydet düğmesi yapılandırın](./media/active-directory-saas-box-tutorial/tutorial_general_400.png)
    
6. Uygulamanız için SSO yapılandırmak için yordamı izleyin [SSO'yu kendi Ayarla](https://community.box.com/t5/How-to-Guides-for-Admins/Setting-Up-Single-Sign-On-SSO-for-your-Enterprise/ta-p/1263#ssoonyourown).

> [!NOTE] 
> Kutusunu hesabınız için SSO ayarları etkinleştiremezsiniz, başvurmanız gerekebilir [kutusunu istemci destek ekibi](https://community.box.com/t5/custom/page/page-id/submit_sso_questionaire) ve indirilen XML dosyası belirtin.

> [!TIP]
> Uygulaması kuruluyor gibi önceki yönergeleri kısa bir sürümünü okuyabilirsiniz [Azure portal](https://portal.azure.com). Uygulamada ekledikten sonra **Active Directory** > **kurumsal uygulamalar** bölümünde, select **çoklu oturum açma** sekmesini tıklatın ve ardından erişim belgelerde katıştırılmış **yapılandırma** alt bölüm. Embedded belgeler özelliği hakkında daha fazla bilgi için bkz: [Azure AD embedded belgeler]( https://go.microsoft.com/fwlink/?linkid=845985).
>

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümde, aşağıdakileri yaparak Azure portalında test kullanıcısı Britta Simon oluşturun:

![Bir Azure AD test kullanıcısı oluşturma][100]

1. Azure portalında sol bölmede seçin **Azure Active Directory**.

    ![Azure Active Directory bağlantısı](./media/active-directory-saas-box-tutorial/create_aaduser_01.png)

2. Geçerli kullanıcıların bir listesini görüntülemek için seçin **kullanıcılar ve gruplar** > **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantılar](./media/active-directory-saas-box-tutorial/create_aaduser_02.png)

3. Üstündeki **tüm kullanıcılar** penceresinde, seçin **Ekle**.

    ![Ekle düğmesi](./media/active-directory-saas-box-tutorial/create_aaduser_03.png)

    **Kullanıcı** penceresi açılır.

4. İçinde **kullanıcı** penceresinde aşağıdakileri yapın:

    ![Kullanıcı penceresi](./media/active-directory-saas-box-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** kullanıcı Britta Simon e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değer aşağı yazma **parola** kutusu.

    d. **Oluştur**’u seçin.
 
### <a name="create-a-box-test-user"></a>Kutusunu test kullanıcısı oluşturma

Bu bölümde, test kullanıcısı Britta Simon kutusunda oluşturun. Kutusu yalnızca zaman sağlama, varsayılan olarak etkin olduğu destekler. Bir kullanıcı zaten yoksa, kutusunu erişmeyi denediğinde yeni bir tane oluşturulur. Hiçbir eylem sizden kullanıcı oluşturmak için gereklidir.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, kullanıcı Britta kutusuna erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin. Bunu yapmak için aşağıdakileri yapın:

![Kullanıcı rolü atayın][200]

1. Azure portalında açmak **uygulamaları** görünümü, Git **dizin** görüntülemek ve ardından **kurumsal uygulamalar** > **tüm uygulamaları**.

    !["Kurumsal uygulamalar" ve "Tüm uygulamaları" bağlantılar][201] 

2. İçinde **uygulamaları** listesinde **kutusunu**.

    ![Kutusu bağlantısı](./media/active-directory-saas-box-tutorial/tutorial_box_app.png)  

3. Sol bölmede seçin **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantı][202]

4. Seçin **Ekle** , daha sonra **eklemek atama** bölmesinde, **kullanıcılar ve gruplar**.

    ![Ekleme atama bölmesi][203]

5. İçinde **kullanıcılar ve gruplar** penceresi, **kullanıcılar** listesinde **Britta Simon**.

6. Seçin **seçin** düğmesi.

7. İçinde **eklemek atama** penceresinde, seçin **atamak**.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test.

Seçtiğinizde, **kutusunu** kutusunu uygulamanıza oturum açmak için oturum açma sayfası erişim panelinde, açık döşeme.

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](active-directory-saas-tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](manage-apps/what-is-single-sign-on.md)
* [Kullanıcı sağlamayı Yapılandır](active-directory-saas-box-userprovisioning-tutorial.md)



<!--Image references-->

[1]: ./media/active-directory-saas-box-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-box-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-box-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-box-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-box-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-box-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-box-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-box-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-box-tutorial/tutorial_general_203.png

