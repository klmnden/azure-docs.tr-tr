---
title: 'Öğretici: Azure Active Directory Tümleştirmesi ile QPrism | Microsoft Docs'
description: Azure Active Directory ve QPrism arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: joflore
ms.assetid: 72ab75ba-132b-4f83-a34b-d28b81b6d7bc
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/23/2018
ms.author: jeedes
ms.openlocfilehash: b59efe3faa00a172e5948f18b06337eedd2a1f6b
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2019
ms.locfileid: "55184810"
---
# <a name="tutorial-azure-active-directory-integration-with-qprism"></a>Öğretici: QPrism ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile QPrism tümleştirme konusunda bilgi edinin.

Azure AD ile QPrism tümleştirme ile aşağıdaki avantajları sağlar:

- QPrism erişimi, Azure AD'de kontrol edebilirsiniz.
- Otomatik olarak için QPrism (çoklu oturum açma) ile Azure AD hesaplarına açmış, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınızı tek bir merkezi konumda yönetebilir: Azure portalı.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı için bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile QPrism yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliği
- Abonelik QPrism çoklu oturum açma etkin

Bu öğreticideki adımları test etmek için aşağıdaki önerileri uygulayın:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, şunları yapabilirsiniz [bir aylık deneme sürümü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden QPrism ekleme
1. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="add-qprism-from-the-gallery"></a>Galeriden QPrism Ekle
Azure AD'de QPrism tümleştirmesini yapılandırmak için QPrism Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden QPrism eklemek için:**

1. İçinde [Azure portalında](https://portal.azure.com), sol bölmede seçin **Azure Active Directory**. 

    ![Azure Active Directory düğmesi][1]

1. Gidin **kurumsal uygulamalar** > **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
1. İletişim kutusunun en üstünde yeni bir uygulama eklemek için seçin **yeni uygulama**.

    ![Yeni Uygulama düğmesi][3]

1. Arama kutusuna **QPrism**seçip **QPrism** sonucu panelinden. Ardından **Ekle** uygulama eklemek için.

    ![Sonuç listesinde QPrism](./media/qprism-tutorial/tutorial_qprism_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon." adlı bir test kullanıcı tabanlı QPrism ile test etme

Tek iş için oturum açma için Azure AD QPrism karşılığı kullanıcının Azure AD'de bir kullanıcı için olan bilmesi gerekir. Diğer bir deyişle, bir Azure AD kullanıcısının QPrism ilgili kullanıcı arasında bağlı bir ilişki olmalıdır.

Bu ilişki, içinde QPrism kurmak için değerini atayın **kullanıcı adı** değerini Azure AD'de **kullanıcı adı**.

Yapılandırma ve Azure AD çoklu oturum açma QPrism ile test etmek için aşağıdaki yapı taşlarını tamamlayın:

1. [Azure AD çoklu oturum açmayı yapılandırma](#configure-azure-ad-single-sign-on) kullanıcılarınız bu özelliği kullanmak etkinleştirmek için.
1. [Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user) Azure AD çoklu oturum açma Britta Simon ile test etmek için.
1. [QPrism test kullanıcısı oluşturma](#create-a-qprism-test-user) bir karşılığı Britta simon'un kullanıcı Azure AD gösterimini bağlı QPrism sağlamak için.
1. [Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user) Britta Simon, Azure AD çoklu oturum açma kullanmak üzere etkinleştirmek için.
1. [Çoklu oturum açmayı test](#test-single-sign-on) yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve QPrism uygulamanızda çoklu oturum açmayı yapılandırın.

1. Azure portalında, üzerinde **QPrism** uygulama tümleştirme sayfasında **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

1. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/qprism-tutorial/tutorial_qprism_samlbase.png)

1. İçinde **QPrism etki alanı ve URL'ler** bölümünde, aşağıdakileri yapın:

    ![QPrism etki alanı ve URL'ler tek oturum açma bilgileri](./media/qprism-tutorial/tutorial_qprism_url.png)

    a. İçinde **oturum açma URL'si** metin kutusunda, aşağıdaki desen kullanan bir URL yazın: `https://<customer domain>.qmyzone.com/login`

    b. İçinde **tanımlayıcı** metin kutusunda, aşağıdaki desen kullanan bir URL yazın: `https://<customer domain>.qmyzone.com/metadata.php`
         
    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler gerçek tanımlayıcısıyla güncelleştirin ve oturum açma URL'si. İlgili kişi [QPrism istemci Destek ekibine](mailto:qsupport-ce@quatrro.com) bu değerleri almak için. 

1. Üzerinde **SAML imzalama sertifikası** bölümünde, kopyalamak için Kopyala düğmesine **uygulama Federasyon meta verileri URL'sini** kopyalayıp Not Defteri'ne yapıştırın.

     ![Sertifika indirme bağlantısı](./media/qprism-tutorial/tutorial_qprism_certificate.png)

1. **Kaydet**’i seçin.

    ![Kaydet düğmesi çoklu oturum açmayı yapılandırın](./media/qprism-tutorial/tutorial_general_400.png)
    
1. Çoklu oturum açmayı yapılandırma **QPrism** tarafını göndermek için ihtiyacınız **uygulama Federasyon meta verileri URL'sini** için [QPrism Destek ekibine](mailto:qsupport-ce@quatrro.com). Bunlar, her iki kenarı da düzgün ayarlandığından SAML SSO bağlantı sağlamak için bu ayarı ayarlayın.

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcısı oluşturmak için:**

1. Azure portalında, sol bölmede seçin **Azure Active Directory**.

    ![Azure Active Directory düğmesi](./media/qprism-tutorial/create_aaduser_01.png)

1. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](./media/qprism-tutorial/create_aaduser_02.png)

1. Açmak için **kullanıcı** iletişim kutusu, üst kısmındaki **tüm kullanıcılar** iletişim kutusunda **Ekle**.

    ![Ekle düğmesi](./media/qprism-tutorial/create_aaduser_03.png)

1. İçinde **kullanıcı** iletişim kutusunda, aşağıdakileri yapın:

    ![Kullanıcı iletişim kutusu](./media/qprism-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** Britta Simon kullanıcı e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değeri yazın **parola** kutusu.

    d. **Oluştur**’u seçin.
 
### <a name="create-a-qprism-test-user"></a>QPrism test kullanıcısı oluşturma

Bu bölümde, Britta Simon QPrism içinde adlı bir kullanıcı oluşturun. Çalışmak [QPrism Destek ekibine](mailto:qsupport-ce@quatrro.com) QPrism platform kullanıcıları eklemek için. Kullanıcı oluşturulmalı ve çoklu oturum açma kullanmadan önce etkinleştirildi. 

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Azure çoklu oturum açma kullanmak için QPrism erişim vererek Britta Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**Britta Simon QPrism için atamak için:**

1. Azure portalında uygulama görünümü açın ve ardından dizin görünümüne gidin. Git **kurumsal uygulamalar**seçip **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

1. Uygulamalar listesinde **QPrism**.

    ![Uygulamalar listesinde QPrism bağlantı](./media/qprism-tutorial/tutorial_qprism_app.png)  

1. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı][202]

1. **Add (Ekle)** seçeneğini belirleyin. Ardından, altında **atama Ekle**seçin **kullanıcılar ve gruplar**.

    ![Atama Ekle bölmesi][203]

1. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** içinde **kullanıcılar** listesi.

1. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **seçin**.

1. Altında **atama Ekle**seçin **atama**.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test.

QPrism kutucuğu seçtiğinizde erişim panelinde, otomatik olarak QPrism uygulamanıza oturum.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/qprism-tutorial/tutorial_general_01.png
[2]: ./media/qprism-tutorial/tutorial_general_02.png
[3]: ./media/qprism-tutorial/tutorial_general_03.png
[4]: ./media/qprism-tutorial/tutorial_general_04.png

[100]: ./media/qprism-tutorial/tutorial_general_100.png

[200]: ./media/qprism-tutorial/tutorial_general_200.png
[201]: ./media/qprism-tutorial/tutorial_general_201.png
[202]: ./media/qprism-tutorial/tutorial_general_202.png
[203]: ./media/qprism-tutorial/tutorial_general_203.png

