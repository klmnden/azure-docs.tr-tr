---
title: 'Öğretici: Azure Active Directory Tümleştirme kutusu | Microsoft Docs'
description: Azure Active Directory ve kutusu arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 3b565c8d-35e2-482a-b2f4-bf8fd7d8731f
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/12/2018
ms.author: jeedes
ms.openlocfilehash: f5aa724e9848c9794eef093aef15b0aaed9cae97
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/02/2018
ms.locfileid: "39435769"
---
# <a name="integrate-azure-active-directory-with-box"></a>Azure Active Directory kutusu ile tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) tümleştirme kutusuyla öğrenin.

Azure AD tümleştirdiğinizde kutusuyla aşağıdaki avantajlardan yararlanabilirsiniz:

- Box'a erişimi, Azure AD'de kontrol edebilirsiniz.
- Oturumunuz otomatik olarak kutusuna (çoklu oturum açma veya SSO) ile Azure AD hesaplarına kullanıcılarınıza etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda, Azure portalında yönetebilir.

Azure AD ile SaaS uygulama tümleştirmesi hakkında bilgi edinmek için bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirmesi kutusuyla yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliğiniz
- Kutusu SSO etkin bir abonelik

> [!NOTE]
> Bu öğreticideki adımları test ettiğinizde, bunu yapmanızı öneririz *değil* bir üretim ortamı kullanın.

Bu öğreticideki adımları test etmek için aşağıdaki önerileri uygulayın:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, şunları yapabilirsiniz [bir aylık deneme sürümü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. 

Bu öğreticide açıklanan senaryo iki temel yapı taşları oluşur:

1. Galeriden kutusu ekleme
1. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="add-box-from-the-gallery"></a>Galeriden kutusu ekleme
Kutusuyla Azure AD'ye tümleştirmesini yapılandırmak için kutusunda aşağıdakileri yaparak Galeriden yönetilen SaaS uygulamaları listenize ekleyin:

1. İçinde [Azure portalında](https://portal.azure.com), sol bölmede seçin **Azure Active Directory**. 

    ![Azure Active Directory düğmesi][1]

1. Seçin **kurumsal uygulamalar** > **tüm uygulamaları**.

    !["Kurumsal uygulamalar" penceresi][2]
    
1. Yeni bir uygulama eklemek için seçin **yeni uygulama** penceresinin üstünde düğme.

    !["Yeni uygulama" düğmesi][3]

1. Arama kutusuna **kutusu**seçin **kutusu** sonuç listesini ve ardından **Ekle**.

    ![Sonuçlar listesinde kutusu](./media/box-tutorial/tutorial_box_search.png)
### <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma kutusunu "Britta Simon." adlı bir test kullanıcı ile test etme

Tek iş için oturum açma için Azure AD kutusuna kullanıcı ve kendisine karşılık gelen Azure AD'de tanımlaması gerekir. Diğer bir deyişle, bir Azure AD kullanıcısı ve aynı kullanıcı kutusunda arasında bir bağlantı ilişki kurulması gerekir.

Bağlantı kurmak için bir kutu olarak atama *kullanıcıadı* değerini *kullanıcı adı* Azure AD'de.

Yapılandırma ve Azure AD çoklu oturum açma kutusu test etmek için sonraki beş bölümlerde yapı taşlarını tamamlayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma aşağıdakileri yaparak kutusu uygulamanızda yapılandırın:

1. Azure portalında, **kutusu** uygulama tümleştirme penceresinde, seçin **çoklu oturum açma**.

    !["Çoklu oturum açma" bağlantısı][4]

1. İçinde **çoklu oturum açma** penceresi içinde **çoklu oturum açma modunu** kutusunda **SAML tabanlı oturum açma**.
 
    !["Çoklu oturum açma" penceresi](./media/box-tutorial/tutorial_box_samlbase.png)

1. Altında **kutusu etki alanı ve URL'ler**, aşağıdakileri yapın:

    !["Etki alanı ve URL'ler kutusuna" çoklu oturum açma bilgileri](./media/box-tutorial/url3.png)

    a. İçinde **oturum açma URL'si** kutusuna bir URL aşağıdaki biçimde yazın: *https://\<alt etki alanı >. box.com*.

    b. İçinde **tanımlayıcı** metin kutusuna **box.net**.
     
    > [!NOTE] 
    > Yukarıdaki değerleri gerçek değildir. Bunları tanımlayıcısı ve gerçek oturum açma URL'si ile güncelleştirin. Değerlerini almak için iletişime geçin [kutusu istemci Destek ekibine](https://community.box.com/t5/custom/page/page-id/submit_sso_questionaire). 

1. Altında **SAML imzalama sertifikası**seçin **meta veri XML**ve bilgisayarınızda meta veri dosyasını kaydedin.

    ![Sertifika indirme bağlantısı](./media/box-tutorial/tutorial_box_certificate.png) 

1. **Kaydet**’i seçin.

    ![Çoklu oturum açma Kaydet düğmesi yapılandırın](./media/box-tutorial/tutorial_general_400.png)
    
1. Uygulamanız için SSO'yu yapılandırmak için verilen yordamı izleyin [SSO'yu kendi ayarlamak](https://community.box.com/t5/How-to-Guides-for-Admins/Setting-Up-Single-Sign-On-SSO-for-your-Enterprise/ta-p/1263#ssoonyourown).

> [!NOTE] 
> Box hesabınıza SSO ayarlarını etkinleştiremiyor, başvurmanız gerekebilir [kutusu istemci Destek ekibine](https://community.box.com/t5/custom/page/page-id/submit_sso_questionaire) ve indirilen XML dosyasını sağlayın.

> [!TIP]
> Uygulamayı hazırlama tutunun gibi önceki yönergeleri kısa bir sürümünü edinebilirsiniz [Azure portalında](https://portal.azure.com). Uygulamayı ekledikten sonra **Active Directory** > **kurumsal uygulamalar** bölümünden **çoklu oturum açma** sekmesine ve ardından erişim belgelerinde katıştırılmış **yapılandırma** alttaki bölümü. Ekli belge özelliği hakkında daha fazla bilgi için bkz. [Azure AD belgeleri katıştırılmış]( https://go.microsoft.com/fwlink/?linkid=845985).
>

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümde, aşağıdakileri yaparak Azure portalında, Britta Simon test kullanıcısı oluşturun:

![Bir Azure AD test kullanıcısı oluşturma][100]

1. Azure portalında, sol bölmede seçin **Azure Active Directory**.

    ![Azure Active Directory bağlantısı](./media/box-tutorial/create_aaduser_01.png)

1. Geçerli kullanıcıların bir listesini görüntülemek için seçin **kullanıcılar ve gruplar** > **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](./media/box-tutorial/create_aaduser_02.png)

1. Üst kısmındaki **tüm kullanıcılar** penceresinde **Ekle**.

    ![Ekle düğmesi](./media/box-tutorial/create_aaduser_03.png)

    **Kullanıcı** penceresi açılır.

1. İçinde **kullanıcı** penceresinde aşağıdakileri yapın:

    ![Kullanıcı penceresi](./media/box-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** Britta Simon kullanıcı e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değeri yazın **parola** kutusu.

    d. **Oluştur**’u seçin.
 
### <a name="create-a-box-test-user"></a>Kutusunu test kullanıcısı oluşturma

Bu bölümde, Britta Simon kutusunda test kullanıcısı oluşturun. Kutusunu tam zamanında sağlama, varsayılan olarak etkin olduğu destekler. Bir kullanıcı zaten mevcut değilse kutusu erişmeye çalıştığında yeni bir tane oluşturulur. Herhangi bir eylemi, kullanıcı oluşturmak için gerekli değildir.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, kullanıcı Box'a erişim izni verdiğinizde, Azure çoklu oturum açma kullanılacak Britta Simon etkinleştirin. Bunu yapmak için aşağıdakileri yapın:

![Kullanıcı rolü atayın][200]

1. Azure portalında açın **uygulamaları** görünümü, Git **dizin** görüntüleyin ve ardından **kurumsal uygulamalar** > **tüm uygulamaları**.

    !["Kurumsal uygulamalar" ve "Tüm uygulamalar" bağlantıları][201] 

1. İçinde **uygulamaları** listesinden **kutusu**.

    ![Kutusu bağlantısı](./media/box-tutorial/tutorial_box_app.png)  

1. Sol bölmede seçin **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı][202]

1. Seçin **Ekle** ve daha sonra **atama Ekle** bölmesinde **kullanıcılar ve gruplar**.

    ![Atama Ekle bölmesi][203]

1. İçinde **kullanıcılar ve gruplar** penceresi içinde **kullanıcılar** listesinden **Britta Simon**.

1. Seçin **seçin** düğmesi.

1. İçinde **atama Ekle** penceresinde **atama**.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test.

Seçtiğinizde, **kutusu** kutusunu uygulamanıza oturum açma için oturum açma sayfasına erişim panelinde açık, kutucuğu.

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)
* [Kullanıcı sağlamayı yapılandırma](box-userprovisioning-tutorial.md)



<!--Image references-->

[1]: ./media/box-tutorial/tutorial_general_01.png
[2]: ./media/box-tutorial/tutorial_general_02.png
[3]: ./media/box-tutorial/tutorial_general_03.png
[4]: ./media/box-tutorial/tutorial_general_04.png

[100]: ./media/box-tutorial/tutorial_general_100.png

[200]: ./media/box-tutorial/tutorial_general_200.png
[201]: ./media/box-tutorial/tutorial_general_201.png
[202]: ./media/box-tutorial/tutorial_general_202.png
[203]: ./media/box-tutorial/tutorial_general_203.png

