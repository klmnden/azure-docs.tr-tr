---
title: 'Öğretici: Azure Active Directory Tümleştirmesi ile Vodeclic | Microsoft Docs'
description: Azure Active Directory ve Vodeclic arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: joflore
ms.assetid: d77a0f53-e3a3-445e-ab3e-119cef6e2e1d
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/06/2017
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: d3dcd39d58089b202d9e9d61cfc5d25e12ff7a6b
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/13/2019
ms.locfileid: "56217777"
---
# <a name="tutorial-azure-active-directory-integration-with-vodeclic"></a>Öğretici: Vodeclic ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile Vodeclic tümleştirme konusunda bilgi edinin.

Azure AD ile Vodeclic tümleştirme ile aşağıdaki avantajları sağlar:

- Vodeclic erişimi, Azure AD'de kontrol edebilirsiniz.
- Otomatik olarak Vodeclic (çoklu oturum açma veya SSO) ile Azure AD hesaplarına açmış, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda--Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile Vodeclic yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliği
- Vodeclic SSO etkin bir abonelik

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için aşağıdaki önerileri uygulayın:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Bir Azure AD deneme ortamı yoksa [bir aylık ücretsiz deneme sürümü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden Vodeclic ekleme
1. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="add-vodeclic-from-the-gallery"></a>Galeriden Vodeclic Ekle
Azure AD'de Vodeclic tümleştirmesini yapılandırmak için Vodeclic Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden Vodeclic eklemek için aşağıdaki adımları uygulayın:**

1. İçinde [Azure portalında](https://portal.azure.com), sol bölmede seçin **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

1. Git **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
1. Yeni bir uygulama eklemek için seçin **yeni uygulama** iletişim kutusunun üstündeki düğmesi.

    ![Yeni Uygulama düğmesi][3]

1. Arama kutusuna **Vodeclic**. Seçin **Vodeclic** sonuçlar paneli ve ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Sonuç listesinde Vodeclic](./media/vodeclic-tutorial/tutorial_vodeclic_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon." adlı bir test kullanıcı tabanlı Vodeclic ile test etme

Tek iş için oturum açma için Azure AD Vodeclic karşılığı kullanıcının Azure AD'de bir kullanıcı için olan bilmesi gerekir. Diğer bir deyişle, Vodeclic içinde bir Azure AD kullanıcısı ile ilgili kullanıcı arasında bir bağlantı kurmak gerekir.

Vodeclic içinde değeri vermek **kullanıcıadı** aynı değer olarak **kullanıcı adı** Azure AD'de. Şimdi iki kullanıcı arasındaki bağlantı kurduğunuz.

Yapılandırma ve Azure AD çoklu oturum açma Vodeclic ile test etmek için aşağıdaki yapı taşlarını tamamlayın:

1. [Azure AD çoklu oturum açmayı yapılandırma](#configure-azure-ad-single-sign-on) kullanıcılarınız bu özelliği kullanmak etkinleştirmek için.
1. [Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user) Azure AD çoklu oturum açma Britta Simon ile test etmek için.
1. [Vodeclic test kullanıcısı oluşturma](#create-a-vodeclic-test-user) bir karşılığı Britta simon'un kullanıcı Azure AD gösterimini bağlı Vodeclic sağlamak için.
1. [Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user) Britta Simon, Azure AD çoklu oturum açma kullanmak üzere etkinleştirmek için.
1. [Çoklu oturum açmayı test](#test-single-sign-on) yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve Vodeclic uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma ile Vodeclic yapılandırmak için aşağıdaki adımları uygulayın:**

1. Azure portalında, üzerinde **Vodeclic** uygulama tümleştirme sayfasında **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

1. İçinde **çoklu oturum açma** iletişim kutusunun **çoklu oturum açma modu**seçin **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/vodeclic-tutorial/tutorial_vodeclic_samlbase.png)

1. Uygulamada yapılandırmak istiyorsanız **IDP** modunda başlatılan **Vodeclic etki alanı ve URL'ler** bölümünde, aşağıdaki adımları uygulayın:

    ![Oturum açma bilgileri çoklu Vodeclic etki alanı ve URL'ler](./media/vodeclic-tutorial/tutorial_vodeclic_url.png)

    a. İçinde **tanımlayıcı** kutusuna aşağıdaki desene sahip bir URL yazın: `https://<companyname>.lms.vodeclic.net/auth/saml`

    b. İçinde **yanıt URL'si** kutusuna aşağıdaki desene sahip bir URL yazın: `https://<companyname>.lms.vodeclic.net/auth/saml/callback`

1. Uygulamada yapılandırmak istiyorsanız **SP** başlatılan modu, select **Gelişmiş URL ayarlarını göster** onay kutusunu işaretleyip aşağıdaki adımları izleyin:

    ![Oturum açma bilgileri çoklu Vodeclic etki alanı ve URL'ler](./media/vodeclic-tutorial/tutorial_vodeclic_url1.png)

    İçinde **oturum açma URL'si** kutusuna aşağıdaki desene sahip bir URL yazın: `https://<companyname>.lms.vodeclic.net/auth/saml`
     
    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler gerçek tanımlayıcısıyla güncelleştirme, yanıt URL'si ve oturum açma URL'si. İlgili kişi [Vodeclic istemci Destek ekibine](mailto:hotline@vodeclic.com) bu değerleri almak için.

1. İçinde **SAML imzalama sertifikası** bölümünden **meta veri XML**. Bilgisayarınızda meta verileri dosyayı kaydedin.

    ![Sertifika indirme bağlantısı](./media/vodeclic-tutorial/tutorial_vodeclic_certificate.png) 

1. **Kaydet**’i seçin.

    ![Çoklu oturum açma Kaydet düğmesi yapılandırın](./media/vodeclic-tutorial/tutorial_general_400.png)
    
1. Çoklu oturum açmayı yapılandırma **Vodeclic** yan, indirilen Gönder **meta veri XML** için [Vodeclic Destek ekibine](mailto:hotline@vodeclic.com). Bunlar, her iki kenarı da düzgün ayarlandığından SAML SSO bağlantı sağlamak için bu ayarı ayarlayın.

> [!TIP]
> İçindeki bu yönergeleri kısa bir sürümünü artık okuyabilir [Azure portalında](https://portal.azure.com) uygulamasını ayarladığınız sırada. Bu uygulamadan ekledikten sonra **Active Directory** > **kurumsal uygulamalar** bölümünden **çoklu oturum açma** sekme ve katıştırılmış erişin belgelerin **yapılandırma** alttaki bölümü. Daha fazla bilgi edinebilirsiniz embedded belgeleri özelliği hakkında [Azure AD belgeleri katıştırılmış]( https://go.microsoft.com/fwlink/?linkid=845985).

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları uygulayın:**

1. Azure portalında, sol bölmede seçin **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/vodeclic-tutorial/create_aaduser_01.png)

1. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar**. Ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](./media/vodeclic-tutorial/create_aaduser_02.png)

1. Açmak için **kullanıcı** iletişim kutusunda **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/vodeclic-tutorial/create_aaduser_03.png)

1. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları uygulayın:

    ![Kullanıcı iletişim kutusu](./media/vodeclic-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** Britta Simon kullanıcı e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değeri yazın **parola** kutusu.

    d. **Oluştur**’u seçin.
 
### <a name="create-a-vodeclic-test-user"></a>Vodeclic test kullanıcısı oluşturma

Bu bölümde, Britta Simon Vodeclic içinde adlı bir kullanıcı oluşturun. Çalışmak [Vodeclic Destek ekibine](mailto:hotline@vodeclic.com) Vodeclic platform kullanıcıları eklemek için. Kullanıcı oluşturulmalı ve çoklu oturum açma kullanmadan önce etkinleştirildi.

> [!NOTE]
> Uygulama gereksinimlerine göre makine beyaz listeye alma gerekebilir. Genel IP adresi ile paylaşmak, gerçekleşmesi gerekir [Vodeclic Destek ekibine](mailto:hotline@vodeclic.com).

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Azure çoklu oturum açma kullanmak için Vodeclic erişim vererek Britta Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**Britta Simon Vodeclic için atamak için aşağıdaki adımları uygulayın:**

1. Azure portalında uygulama görünümü açın ve ardından dizin görünümüne gidin. Ardından, Git **kurumsal uygulamalar**ve ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

1. Uygulamalar listesinde **Vodeclic**.

    ![Uygulamalar listesinde Vodeclic bağlantı](./media/vodeclic-tutorial/tutorial_vodeclic_app.png)  

1. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı][202]

1. Seçin **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim kutusu.

    ![Atama Ekle bölmesi][203]

1. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** içinde **kullanıcılar** listesi.

1. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **seçin** düğmesi.

1. İçinde **atama Ekle** iletişim kutusunda **atama** düğmesi.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test.

Erişim Paneli'nde Vodeclic kutucuğu seçtiğinizde, otomatik olarak Vodeclic uygulamanıza oturum açmış.

Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/vodeclic-tutorial/tutorial_general_01.png
[2]: ./media/vodeclic-tutorial/tutorial_general_02.png
[3]: ./media/vodeclic-tutorial/tutorial_general_03.png
[4]: ./media/vodeclic-tutorial/tutorial_general_04.png

[100]: ./media/vodeclic-tutorial/tutorial_general_100.png

[200]: ./media/vodeclic-tutorial/tutorial_general_200.png
[201]: ./media/vodeclic-tutorial/tutorial_general_201.png
[202]: ./media/vodeclic-tutorial/tutorial_general_202.png
[203]: ./media/vodeclic-tutorial/tutorial_general_203.png

