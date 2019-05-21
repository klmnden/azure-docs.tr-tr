---
title: 'Öğretici: Azure Active Directory Tümleştirmesi ile AnswerHub | Microsoft Docs'
description: Azure Active Directory ve AnswerHub arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 818b91d7-01df-4b36-9706-f167c710a73c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 01/17/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 52c49bdd51bda7876d19a681bde79c9dbeeb4ea7
ms.sourcegitcommit: 67625c53d466c7b04993e995a0d5f87acf7da121
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2019
ms.locfileid: "65901289"
---
# <a name="tutorial-azure-active-directory-integration-with-answerhub"></a>Öğretici: AnswerHub ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile AnswerHub tümleştirme konusunda bilgi edinin.
AnswerHub Azure AD ile tümleştirme şu avantajları sağlar:

* Azure AD AnswerHub erişimi denetlemek için kullanabilirsiniz.
* (Çoklu oturum açma) ile kendi Azure AD hesapları için AnswerHub otomatik olarak oturum açın, kullanıcılarınızın izin verebilirsiniz.
* Hesaplarınız bir merkezi konumdan yönetebilirsiniz: Azure portalı.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla bilgi için bkz. [Azure Active Directory'de uygulamalar için çoklu oturum açma](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile AnswerHub yapılandırmak için aşağıdakiler gerekir:

* Azure AD aboneliğiniz. Bir Azure AD ortamını yoksa başlayabilirsiniz bir [bir aylık deneme](https://azure.microsoft.com/pricing/free-trial/).
* Tekli etkin oturum sahip bir AnswerHub aboneliği.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, yapılandırma ve Azure AD çoklu oturum açma bir test ortamında test edin.

* AnswerHub SP tarafından başlatılan SSO'yu destekler.

## <a name="add-answerhub-from-the-gallery"></a>Galeriden AnswerHub Ekle

Azure AD'de AnswerHub tümleştirmesini ayarlamak için AnswerHub Galeriden yönetilen SaaS uygulamalarınıza eklemeniz gerekir.

**Galeriden AnswerHub eklemek için:**

1. İçinde [Azure portalında](https://portal.azure.com), sol bölmede seçin **Azure Active Directory**.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. Git **kurumsal uygulamalar**ve ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Bir uygulama eklemek için seçin **yeni uygulama** pencerenin üst kısmındaki.

    ![Yeni Uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **AnswerHub**. Seçin **AnswerHub** sonuç listesini ve ardından **Ekle**.

     ![Sonuç listesinde AnswerHub](common/search-new-app.png)

## <a name="set-up-and-test-azure-ad-single-sign-on"></a>Ayarlama ve Azure AD çoklu oturum açma testi

Bu bölümde, yapılandırın ve Azure AD çoklu oturum açma ile AnswerHub Britta Simon adlı bir test kullanıcısı kullanarak test edin.
Çoklu oturum açma için AnswerHub içinde bir Azure AD kullanıcısı ile ilgili kullanıcı arasında bir bağlantı yapmanız gerekir.

Yapılandırma ve Azure AD çoklu oturum açma AnswerHub ile test etmek için bu görevi tamamlamanız gerekir:

1. [Azure AD çoklu oturum açmayı yapılandırma](#configure-azure-ad-single-sign-on) kullanıcılarınızın özelliği kullanmak etkinleştirmek için.
2. [AnswerHub çoklu oturum açmayı yapılandırma](#configure-answerhub-single-sign-on) çoklu oturum açma ayarları uygulama tarafında ayarlamak için.
3. [Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user) Britta Simon adlı.
4. [Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user) Britta Simon, Azure AD çoklu oturum açma kullanmak üzere etkinleştirmek için.
5. Karşılık gelir ve Azure AD test kullanıcıya bağlı AnswerHub test kullanıcısı oluşturun.
6. [Çoklu oturum açmayı test](#test-single-sign-on) yapılandırma çalıştığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalındaki ayarlarsınız.

**Azure AD çoklu oturum açma ile AnswerHub yapılandırmak için:**

1. İçinde [Azure portalında](https://portal.azure.com/), **AnswerHub** uygulama tümleştirme sayfasında **çoklu oturum açma**.

    ![Çoklu oturum açma düğmesi](common/select-sso.png)

2. İçinde **tek bir oturum açma yönteminizi seçmeniz** Seç iletişim kutusunda **SAML/WS-Federasyon** modu, çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma seçin yöntemi iletişim kutusu](common/select-saml-option.png)

3. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlayın** sayfasında, açmak için Düzenle simgesine **temel SAML yapılandırma** iletişim kutusu.

    ![SAML sayfası ile çoklu oturum açmayı ayarlayın](common/edit-urls.png)

4. İçinde **temel SAML yapılandırma** bölümünde, aşağıdaki adımları tamamlayın:

    ![Temel bir SAML yapılandırma bölümü](common/sp-identifier.png)

    a. İçinde **oturum açma URL'si** kutusuna, bu düzen bir URL girin: `https://<company>.answerhub.com`

    b. İçinde **tanımlayıcı (varlık kimliği)** kutusuna, bu düzen bir URL girin: `https://<company>.answerhub.com`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerleri tanımlayıcısı ve gerçek oturum açma URL'si ile güncelleştirin. İlgili kişi [AnswerHub Destek ekibine](mailto:success@answerhub.com) değerleri alabilirsiniz. Gösterilen desenleri de başvurabilirsiniz **temel SAML yapılandırma** bölümünde Azure portalında.

5. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **SAML imzalama sertifikası** bölümünden **indirme** yanındaki bağlantı **sertifika (Base64)** , gereksinimlerinize göre ve sertifikayı bilgisayarınızda kaydedin.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

6. İçinde **AnswerHub kümesi** bölümünde, uygun URL'yi veya URL'leri, gereksinimlerinize göre kopyalayın.

    ![Yapılandırma URL'leri kopyalayın](common/copy-configuration-urls.png)

   Bu URL'ler kopyalayabilirsiniz:
    - Oturum Açma URL'si:

    - Azure AD Tanımlayıcısı

    - Oturum Kapatma URL'si

### <a name="configure-answerhub-single-sign-on"></a>AnswerHub çoklu oturum açmayı yapılandırın

Bu bölümde, AnswerHub için çoklu oturum açmayı ayarlayın.  

**AnswerHub çoklu oturum açmayı yapılandırmak için:**

1. Farklı bir web tarayıcı penceresinde AnswerHub şirketinizin sitesi için bir yönetici olarak oturum açın

    > [!NOTE]
    > AnswerHub yapılandırırken Yardım gerekirse başvurun [AnswerHub Destek ekibine](mailto:success@answerhub.com.).

2. Git **Yönetim**.

3. Üzerinde **kullanıcı ve grupları** , sol bölmede sekmesinde **sosyal ayarları** bölümünden **SAML Kurulumu**.

4. Üzerinde **IDP yapılandırma** sekmesinde, aşağıdaki adımları tamamlayın:

    ![Kullanıcılar ve Gruplar sekmesinde](./media/answerhub-tutorial/ic785172.png "SAML Kurulumu")  
  
    a. İçinde **IDP'nin oturum açma URL'si** kutusu, yapıştırma **oturum açma URL'si** Azure portaldan kopyaladığınız.
  
    b. İçinde **IDP oturum kapatma URL'si** kutusu, yapıştırma **oturum kapatma URL'si** Azure portaldan kopyaladığınız.

    c. İçinde **IDP ad tanımlayıcı biçimi** kutusuna **tanımlayıcı** seçili değer **kullanıcı öznitelikleri** bölümünde Azure portalında.
  
    d. Seçin **anahtarlara ve sertifikalara**.

5. İçinde **anahtarlara ve sertifikalara** bölümünde, aşağıdaki adımları tamamlayın:

    ![Anahtarlara ve sertifikalara bölüm](./media/answerhub-tutorial/ic785173.png "anahtarlar ve sertifikalar")  

    a. Not Defteri'nde Azure portalından indirdiğiniz Base64 kodlu bir sertifika açın, içeriğini kopyalayın ve ardından içeriği yapıştırın **IDP ortak anahtarı (x 509 biçimi)** kutusu.
  
    b. **Kaydet**’i seçin.

6. Üzerinde **IDP yapılandırma** sekmesinde **Kaydet** yeniden.

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümde, Azure portalında Britta Simon adlı bir test kullanıcısı oluşturun.

**Bir Azure AD test kullanıcısı oluşturmak için:**

1. Azure portalında, sol bölmede seçin **Azure Active Directory**seçin **kullanıcılar**ve ardından **tüm kullanıcılar**.

    ![Azure Active Directory, kullanıcılar, tüm kullanıcılar'ı seçin](common/users.png)

2. Seçin **yeni kullanıcı** ekranın üstünde.

    ![Yeni kullanıcı düğmesi](common/new-user.png)

3. Kullanıcı özellikleri, aşağıdaki adımları tamamlayın.

    ![Kullanıcı özellikleri](common/user-properties.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.
  
    b. İçinde **kullanıcı adı** kutusuna **brittasimon\@< yourcompanydomain.extension >**.  
    Örneğin, BrittaSimon@contoso.com.

    c. Seçin **Show parola** onay kutusunu işaretleyin ve ardından görüntülenen değeri yazın **parola** kutusu.

    d. **Oluştur**’u seçin.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Azure AD çoklu oturum açma için AnswerHub erişim vererek kullanmak için Britta Simon ayarlayın.

**Azure AD test kullanıcı atamak için:**

1. Azure portalında **kurumsal uygulamalar**seçin **tüm uygulamaları**ve ardından **AnswerHub**.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde seçin **AnswerHub**.

    ![Uygulamalar listesi](common/all-applications.png)

3. Soldaki menüde **kullanıcılar ve gruplar**.

    ![Kullanıcıları ve grupları seçin](common/users-groups-blade.png)

4. Seçin **Kullanıcı Ekle**ve ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim kutusu.

    ![Atama bölmesi ekleme](common/add-assign-user.png)

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** içinde **kullanıcılar** listeleyin ve ardından **seçin** kısmındaki düğmesi ekranı.

6. SAML onaylaması rol değeri de beklediğiniz varsa **rolü Seç** iletişim kutusunda, listeden bir kullanıcı için uygun rolü seçin. 

7. Seçin **seçin** ekranın alt kısmındaki düğmesi.

8. İçinde **atama Ekle** iletişim kutusunda **atama**.

### <a name="create-an-answerhub-test-user"></a>Bir AnswerHub test kullanıcısı oluşturma

Azure AD kullanıcıları için AnswerHub oturum açmak etkinleştirmek için AnswerHub eklemeniz gerekir. Bu görev AnswerHub içinde el ile gerçekleştirilir.

**Bir kullanıcı hesabı ayarlamak için:**

1. Oturum açın, **AnswerHub** yönetici olarak şirketin site

2. Git **Yönetim**.

3. Seçin **kullanıcıları ve grupları** sekmesi.

4. Sol bölmede, **Kullanıcıları Yönet** bölümünden **oluşturun veya içeri aktarma kullanıcı**ve ardından **kullanıcıları ve grupları**.

   ![Kullanıcılar ve Gruplar sekmesinde](./media/answerhub-tutorial/ic785175.png "kullanıcıları ve grupları")

5. Uygun kutulara girin **e-posta adresi**, **kullanıcıadı**, ve **parola** ekleyin ve ardından istediğiniz geçerli bir Azure AD hesabı **Kaydet** .

> [!NOTE]
> Azure AD kullanıcı hesaplarını ayarlamak için AnswerHub API sağladığı ya da diğer kullanıcı hesabı oluşturma araçları kullanabilirsiniz.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test.

Erişim Paneli'nde AnswerHub kutucuğu seçtiğinizde, otomatik olarak SSO'yu ayarlama AnswerHub için oturum açmanız. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek kaynaklar

- [Azure Active Directory ile SaaS uygulamalarını tümleştirme konusundaki öğreticiler](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

