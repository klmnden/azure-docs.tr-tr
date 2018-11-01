---
title: 'Öğretici: Azure Active Directory 8x8lik sanal Office ile tümleştirme | Microsoft Docs'
description: Azure Active Directory ve 8x8lik sanal Office arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: b34a6edf-e745-4aec-b0b2-7337473d64c5
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/30/2018
ms.author: jeedes
ms.openlocfilehash: 53db637bf7ad47896747b491fcbe31123fdb104e
ms.sourcegitcommit: ae45eacd213bc008e144b2df1b1d73b1acbbaa4c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/01/2018
ms.locfileid: "50741819"
---
# <a name="tutorial-azure-active-directory-integration-with-8x8-virtual-office"></a>Öğretici: Azure Active Directory 8x8lik sanal Office ile tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile 8x8lik sanal Office tümleştirme konusunda bilgi edinin.

8x8lik tümleştirmek Azure AD ile sanal Office ile aşağıdaki avantajları sağlar:

- Kimlerin erişebildiğini 8x8lik sanal Office için Azure AD'de kontrol edebilirsiniz.
- Azure AD hesaplarına otomatik olarak imzalanan (çoklu oturum açma) 8x8lik sanal Office açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md)

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirmesi 8x8lik sanal Office ile yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliği
- Abonelik 8x8lik sanal Office çoklu oturum açma etkin

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, şunları yapabilirsiniz [bir aylık deneme sürümü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden 8x8lik sanal Office ekleme
2. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-8x8-virtual-office-from-the-gallery"></a>Galeriden 8x8lik sanal Office ekleme

Azure AD'de 8x8lik sanal Office tümleştirmesini yapılandırmak için 8x8lik sanal Office Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden 8x8lik sanal Office eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **8x8lik sanal Office**seçin **8x8lik sanal Office** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![sonuç listesinde 8x8lik sanal Office](./media/8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcısı üzerinde sanal tabanlı 8x8lik ile test edin.

Tek iş için oturum açma için Azure AD 8 x 8 sanal Office, Azure AD'de bir kullanıcı için olan karşılığı kullanıcının bilmesi gerekir. Diğer bir deyişle, bir Azure AD kullanıcısının 8x8lik ilgili kullanıcı arasında bir bağlantı ilişki sanal Office kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma 8x8lik sanal Office ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Bir 8 x 8 sanal Office test kullanıcısı oluşturma](#creating-a-8x8-virtual-office-test-user)**  - kullanıcı Azure AD gösterimini bağlı 8x8lik sanal Office Britta simon'un bir karşılığı vardır.
4. **[Azure AD test kullanıcı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açma testi](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve 8x8lik sanal Office uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma 8x8lik sanal Office ile yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **8x8lik sanal Office** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** iletişim kutusunu tıklatın **seçin** için **SAML** modu, çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açmayı yapılandırın](common/tutorial_general_301.png)

3. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için simgeyi **temel SAML yapılandırma** iletişim.

    ![Çoklu oturum açmayı yapılandırın](common/editconfigure.png)

4. Üzerinde **temel SAML yapılandırma** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![8x8lik sanal Office etki alanı ve URL'ler tek oturum açma bilgileri](./media/8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_url.png)

    a. İçinde **tanımlayıcı** metin kutusuna bir URL: `https://sso.8x8.com/saml2`

    b. İçinde **yanıt URL'si** metin kutusuna bir URL: `https://sso.8x8.com/saml2`

5. Üzerinde **SAML imzalama sertifikası** sayfasında **SAML imzalama sertifikası** bölümünde **indirme** indirmek için **sertifika (ham)** ve bilgisayarınızdaki sertifika dosyasını kaydedin.

    ![Sertifika indirme bağlantısı](./media/8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_certificate.png) 

6. Üzerinde **8x8lik sanal ofis Kur** bölümünde, ihtiyacınıza göre uygun URL'yi kopyalayın.

    a. Oturum Açma URL'si:

    b. Azure AD Tanımlayıcısı

    c. Oturum Kapatma URL'si

    ![8x8lik sanal Office yapılandırma](common/configuresection.png)

7. 8x8lik sanal Office kiracınıza yönetici olarak oturum.

8. Seçin **sanal Office hesabı Mgr** uygulama panosunda.

    ![Uygulama tarafında yapılandırma](./media/8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_001.png)

9. Seçin **iş** yönetmek ve hesap **oturum** düğmesi.

    ![Uygulama tarafında yapılandırma](./media/8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_002.png)

10. Tıklayın **HESAPLARI** menü listesi sekmesindedir.

    ![Uygulama tarafında yapılandırma](./media/8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_003.png)

11. Tıklayın **çoklu oturum açma** hesapları listesinde.
  
    ![Uygulama tarafında yapılandırma](./media/8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_004.png)

12. Seçin **çoklu oturum açma** kimlik doğrulama yöntemleri ve tıklatın altında **SAML**.

    ![Uygulama tarafında yapılandırma](./media/8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_005.png)

13. İçinde **SAML çoklu oturum açma** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Uygulama tarafında yapılandırma](./media/8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_006.png)

    a. İçinde **oturum açma URL'si** metin kutusu, yapıştırma **oturum açma URL'si** Azure portaldan kopyaladığınız değeri.

    b. İçinde **oturum kapatma URL'si** metin kutusu, yapıştırma **oturum kapatma URL'si** Azure portaldan kopyaladığınız değeri.

    c. İçinde **veren URL'si** metin kutusu, yapıştırma **Azure AD tanımlayıcısı** Azure portaldan kopyaladığınız değeri.

    d. Tıklayın **Gözat** Azure portalından indirilen sertifikayı karşıya yüklemek için düğme.

    e. **Kaydet** düğmesine tıklayın.

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

1. Azure portalında, sol bölmede seçin **Azure Active Directory**seçin **kullanıcılar**ve ardından **tüm kullanıcılar**.

    ![Azure AD kullanıcısı oluşturun][100]

2. Seçin **yeni kullanıcı** ekranın üstünde.

    ![Bir Azure AD test kullanıcısı oluşturma](common/create_aaduser_01.png) 

3. Kullanıcı özellikleri, aşağıdaki adımları gerçekleştirin.

    ![Bir Azure AD test kullanıcısı oluşturma](common/create_aaduser_02.png)

    a. İçinde **adı** alanına **BrittaSimon**.
  
    b. İçinde **kullanıcı adı** alanına **brittasimon@yourcompanydomain.extension**  
    Örneğin, BrittaSimon@contoso.com

    c. Seçin **özellikleri**seçin **Show parola** onay kutusunu işaretleyin ve ardından parola kutusunda görüntülenen değeri yazın.

    d. **Oluştur**’u seçin.
  
### <a name="creating-a-8x8-virtual-office-test-user"></a>Bir 8 x 8 sanal Office test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon 8x8lik sanal Office adlı bir kullanıcı oluşturmaktır. 8x8lik sanal Office tam zamanında sağlama, varsayılan olarak etkin olan destekler.

Bu bölümde, hiçbir eylem öğesini yoktur. Yeni bir kullanıcı, henüz yoksa 8x8lik sanal Office erişme denemesi sırasında oluşturulur.

> [!NOTE]
> Bir kullanıcı el ile oluşturmanız gerekiyorsa, iletişime geçmeniz [8x8lik sanal Office Destek ekibine](https://www.8x8.com/about-us/contact-us).

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcı atama

Bu bölümde, 8x8lik sanal Office için erişim izni verdiğinizde, Azure çoklu oturum açma kullanılacak Britta Simon etkinleştirin.

1. Azure portalında **kurumsal uygulamalar**seçin **tüm uygulamaları**.

    ![Kullanıcı Ata][201]

2. Uygulamalar listesinde **8x8lik sanal Office**.

    ![Çoklu oturum açmayı yapılandırın](./media/8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_app.png) 

3. Soldaki menüde **kullanıcılar ve gruplar**.

    ![Kullanıcı Ata][202]

4. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Kullanıcı Ata][203]

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** 'a tıklayın kullanıcı listesinde **seçin** ekranın alt kısmındaki düğmesi.

6. İçinde **atama Ekle** iletişim kutusunda **atama** düğmesi.

### <a name="testing-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli 8x8lik sanal Office kutucuğa tıkladığınızda, size otomatik olarak 8x8lik sanal Office uygulamanızın açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: common/tutorial_general_01.png
[2]: common/tutorial_general_02.png
[3]: common/tutorial_general_03.png
[4]: common/tutorial_general_04.png

[100]: common/tutorial_general_100.png

[201]: common/tutorial_general_201.png
[202]: common/tutorial_general_202.png
[203]: common/tutorial_general_203.png
