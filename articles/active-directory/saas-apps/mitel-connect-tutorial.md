---
title: 'Öğretici: Mitel Connect ile Azure Active Directory Tümleştirmesi | Microsoft Docs'
description: Azure Active Directory ve Mitel bağlanmak arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 204f540b-09f1-452b-a52f-78143710ef76
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 05/03/2019
ms.author: jeedes
ms.openlocfilehash: e03870322df94a4c9587a3395c70925d2d2e838d
ms.sourcegitcommit: ccb9a7b7da48473362266f20950af190ae88c09b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2019
ms.locfileid: "67588218"
---
# <a name="tutorial-azure-active-directory-integration-with-mitel-micloud-connect"></a>Öğretici: Mitel MiCloud Connect ile Azure Active Directory Tümleştirmesi

Bu öğreticide, Azure Active Directory (Azure AD) ile tümleştirme Mitel MiCloud bağlanmak öğreneceksiniz. Azure AD ile tümleştirme MiCloud bağlanma ile aşağıdaki avantajları sağlar:

* MiCloud, bağlandığınız uygulamaların, Kurumsal kimlik bilgilerini kullanarak erişebilir, Azure AD'de kontrol edebilirsiniz.
* Azure AD hesaplarına otomatik olarak (çoklu oturum açma) MiCloud bağlanmak için oturum açmış olarak hesabınızdaki kullanıcılar etkinleştirebilirsiniz.


Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirmesi MiCloud bağlantı ile yapılandırmak için aşağıdaki öğeler gerekir:

* Azure AD aboneliği

  Bir Azure AD ortamını yoksa alabileceğiniz bir [ücretsiz hesap](https://azure.microsoft.com/free/)
* Bir Mitel MiCloud bağlanmak hesabı

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, yapılandırma ve Azure AD çoklu oturum açma (SSO) test edin.

* Mitel bağlanmak destekler **SP** tarafından başlatılan

## <a name="adding-mitel-connect-from-the-gallery"></a>Galeriden Mitel bağlama ekleme

Azure AD Connect Mitel tümleştirme yapılandırmak için Mitel bağlanmak Galeriden yönetilen SaaS uygulamaları, Azure portalında listesine eklemeniz gerekir.

**Galeriden Ekle Mitel bağlanmak için aşağıdaki adımları uygulayın:**

1. **[Azure portalda](https://portal.azure.com)** sol taraftaki gezinti panelinden **Azure Active Directory**’ye tıklayın.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. Tıklayın **kurumsal uygulamalar** ve ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Tıklayın **yeni uygulama**.

    ![Yeni Uygulama düğmesi](common/add-new-app.png)

4. Tür **Mitel bağlanmak** arama alanını tıklatın **Mitel bağlanmak** sonuçlar paneli ve ardından **Ekle**.

     ![Sonuç listesinde Mitel bağlanma](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırın ve MiCloud Connect ile Azure AD çoklu oturum açmayı test adlı bir test kullanıcı tabanlı **Britta Simon**. Tek iş için oturum açma için bir Azure AD kullanıcısı ile ilgili MiCloud bağlama kullanıcı arasında bir bağlantı ilişki kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma MiCloud Connect ile test etme için aşağıdaki adımları tamamlamanız gerekir:

1. **[Azure AD ile SSO için MiCloud Bağlan'ı yapılandırma](#configure-micloud-connect-for-sso-with-azure-ad)**  - kullanıcılarınız bu özelliği kullanmak için ve uygulama tarafında SSO ayarlarını yapılandırmaya yönelik etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
4. **[Mitel MiCloud bağlanmak test kullanıcısı oluşturma](#create-a-mitel-micloud-connect-test-user)**  - kullanıcı Azure AD gösterimini bağlı MiCloud bağlanmak hesabınızda Britta simon'un bir karşılığı vardır.
5. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-micloud-connect-for-sso-with-azure-ad"></a>MiCloud yapılandırmak için Azure AD ile SSO bağlama

Bu bölümde, Azure AD çoklu oturum açma MiCloud bağlanmak için Azure portalında etkinleştirin ve MiCloud bağlanmak hesabınızı Azure AD kullanarak SSO izin verecek şekilde yapılandırın.

MiCloud bağlanmak için Azure AD SSO ile yapılandırmak için Azure portalı ve yan yana Mitel hesap Portalı'nı açmak en kolay yoldur. Bazı bilgiler Mitel hesap portalı ve bazı Mitel hesap portalından Azure portalında Azure portalından kopyalayın gerekecektir.


1. Yapılandırma sayfasını açmak için [Azure portalında](https://portal.azure.com/), aşağıdakileri yapın:

    a. Üzerinde **Mitel bağlanma** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma](common/select-sso.png)

    b. İçinde **tek bir oturum açma yönteminizi seçmeniz** iletişim kutusunda, tıklayın **SAML**.

    ![Çoklu oturum açma seçim modu](common/select-saml-option.png)
    
    SAML tabanlı oturum açma sayfası görüntülenir.

2. Yapılandırma iletişim kutusu Mitel hesap Portalı'nda açmak için aşağıdakileri yapın:

    a. Üzerinde **telefon sistemi** menüsünü tıklatın **eklenti özellikleri**.

    b. Sağındaki **çoklu oturum açma**, tıklayın **etkinleştirme** veya **ayarları**.
    
    Connect çoklu oturum açma ayarları iletişim kutusu görüntülenir.
    
3. Seçin **etkinleştirme çoklu oturum açma** onay kutusu.
    ![Görüntü](./media/mitel-connect-tutorial/Mitel_Connect_Enable.png)


4. Azure portalında **Düzenle** simgesini **temel SAML yapılandırma** bölümü.
    ![Görüntü](common/edit-urls.png)

    Temel bir SAML yapılandırma iletişim kutusu görüntülenir.

5.  URL'den kopyalama **Mitel tanımlayıcı (varlık kimliği)** yapıştırın ve alan Mitel hesap Portalı'nda **tanımlayıcı (varlık kimliği)** Azure portalında alan.

6. URL'den kopyalama **yanıt URL'si (onay belgesi tüketici hizmeti URL'si)** yapıştırın ve alan Mitel hesap Portalı'nda **yanıt URL'si (onay belgesi tüketici hizmeti URL'si)** Azure portalında alan.  
   ![Görüntü](./media/mitel-connect-tutorial/Mitel_Azure_BasicConfig.png)

7. İçinde **oturum açma URL'si** metin kutusunda, aşağıdaki URL'lerden birini yazın:

    * **https://portal.shoretelsky.com** -Varsayılan Mitel uygulamanızı Mitel hesap portalı kullanmak için
    * **https://teamwork.shoretel.com** -Teamwork varsayılan Mitel uygulamanızı kullanmak için

    **NOT**: Varsayılan Mitel uygulama erişim panelinde Mitel bağlanma kutucuğuna kullanıcı tıkladığında erişilen uygulamasıdır. Bu da bir test ayarı, Azure AD'den yaparken erişilen uygulamasıdır.

8. Tıklayın **Kaydet** içinde **temel SAML yapılandırma** Azure portalında iletişim kutusu.

9. İçinde **SAML imzalama sertifikası** bölümünde **SAML tabanlı oturum açma** sayfasında Azure Portalı'nda, **indirme** yanındaki **sertifika (Base64)** indirmek için **imzalama sertifikası** ve bilgisayarınıza kaydedin.
    ![Görüntü](./media/mitel-connect-tutorial/Azure_SigningCert.png)

10. İmzalama sertifikası dosyasını bir metin düzenleyicisinde açın, tüm veri dosyasına kopyalayın ve verileri yapıştırın **imzalama sertifikası** Mitel hesap portalındaki alan. 
    ![Görüntü](./media/mitel-connect-tutorial/Mitel_Connect_SigningCert.png)

11. İçinde **Kurulum Mitel bağlantı** bölümünde **SAML tabanlı oturum açma** sayfa Azure portalı, aşağıdakileri yapın:

    a. URL'den kopyalama **oturum açma URL'si** yapıştırın ve alan **oturum açma URL'si** Mitel hesap portalındaki alan.

    b. URL'den kopyalama **Azure AD tanımlayıcısı** yapıştırın ve alan **varlık kimliği** Mitel hesap portalındaki alan.
    ![Görüntü](./media/mitel-connect-tutorial/Mitel_Azure_SetupConnect.png)

12. Tıklayın **Kaydet** üzerinde **Connect çoklu oturum açma ayarları** Mitel hesap portalı iletişim kutusu.

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma 

Bu bölümde, Azure portalında Britta Simon adlı bir test kullanıcısı oluşturacaksınız.

1. Azure portalında, sol bölmede, tıklayın **Azure Active Directory**, tıklayın **kullanıcılar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](common/users.png)

2. Tıklayın **yeni kullanıcı** ekranın üstünde.

    ![Yeni kullanıcı düğmesi](common/new-user.png)

3. Kullanıcı Özellikleri iletişim kutusunda, aşağıdaki adımları uygulayın:

    ![Kullanıcı iletişim kutusu](common/user-properties.png)

    a. İçinde **adı** alanına **BrittaSimon**.
  
    b. İçinde **kullanıcı adı** alanında, @ brittasimon yazın\<yourcompanydomain\>.\< Uzantı\>.  
Örneğin, BrittaSimon@contoso.com.

    c. Seçin **Show parola** onay kutusunu işaretleyin ve ardından görüntülenen değeri yazın **parola** kutusu.

    d. **Oluştur**’a tıklayın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Azure çoklu oturum açma Mitel bağlanmak için erişim izni verme kullanmak Britta Simon tıklatmalarını sağlarsınız.

1. Azure portalında **kurumsal uygulamalar**ve ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde **Mitel bağlanma**.

    ![Uygulamalar listesinde Mitel bağlanın bağlantısı](common/all-applications.png)

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

4. Tıklayın **Kullanıcı Ekle**, ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** içinde **kullanıcılar** listesinde'a tıklayın **seçin** ekranın alt kısmındaki.

6. Listeden herhangi bir rolü değer SAML onayı bekleniyor, kullanıcı için uygun rolü seçin **rolü Seç** iletişim ve ardından **seçin** ekranın alt kısmındaki.

7. İçinde **atama Ekle** iletişim kutusunda, tıklayın **atama**.

### <a name="create-a-mitel-micloud-connect-test-user"></a>Mitel MiCloud bağlanmak test kullanıcısı oluşturma

Bu bölümde, Britta Simon MiCloud bağlanmak hesabınızdaki adlı bir kullanıcı oluşturun. Kullanıcılar oluşturulan ve çoklu oturum açma kullanılarak önce etkinleştirilmesi gerekir.

Mitel hesap Portalı'nda kullanıcı ekleme hakkında daha fazla ayrıntı için bkz. [kullanıcı ekleme](https://docs.shoretel.com/connectcloud/Account/Users/AddUser) Mitel Bilgi Bankası makalesine.

Bir kullanıcı MiCloud bağlanmak hesabınızda aşağıdaki ayrıntılarla oluşturun:

  * **Adı:** Britta Simon

* **İş e-posta adresi:** `brittasimon@<yourcompanydomain>.<extension>`   
(Örnek: [ brittasimon@contoso.com ](mailto:brittasimon@contoso.com))

* **Kullanıcı adı:** `brittasimon@<yourcompanydomain>.<extension>`  
(Örnek: [ brittasimon@contoso.com ](mailto:brittasimon@contoso.com); kullanıcının kullanıcı adı genellikle kullanıcının iş e-posta adresi ile aynıdır)

**NOT:** Kullanıcının MiCloud bağlama kullanıcı adı, kullanıcının e-posta adresine azure'da aynı olmalıdır.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak, Azure AD çoklu oturum açma yapılandırması test edeceğiz.

Erişim paneli Mitel bağlanma kutucuğa tıkladığınızda, otomatik olarak varsayılan olarak yapılandırılmış MiCloud Connect uygulama oturum açmak için yeniden yönlendirilmesi gereken **oturum açma URL'si** alan. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
