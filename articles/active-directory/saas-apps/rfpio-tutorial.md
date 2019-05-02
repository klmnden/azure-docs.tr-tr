---
title: 'Öğretici: Azure Active Directory Tümleştirmesi ile RFPIO | Microsoft Docs'
description: Azure Active Directory ve RFPIO arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 87187076-7b50-4247-814f-f217b052703f
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 04/14/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 176e0c80b4b1c70c3f49a35ae04d6233bc080f43
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64687801"
---
# <a name="tutorial-azure-active-directory-integration-with-rfpio"></a>Öğretici: RFPIO ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile RFPIO tümleştirme konusunda bilgi edinin.
Azure AD ile RFPIO tümleştirme ile aşağıdaki avantajları sağlar:

* RFPIO erişimi, Azure AD'de kontrol edebilirsiniz.
* Otomatik olarak (çoklu oturum açma) RFPIO için kendi Azure AD hesapları ile oturum açmış, kullanıcıların etkinleştirebilirsiniz.
* Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile RFPIO yapılandırmak için aşağıdaki öğeler gerekir:

* Azure AD aboneliğiniz. Bir Azure AD ortamını yoksa alabileceğiniz bir [ücretsiz hesap](https://azure.microsoft.com/free/)
* Abonelik RFPIO çoklu oturum açma etkin

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, yapılandırma ve Azure AD çoklu oturum açma bir test ortamında test edin.

* RFPIO destekler **SP ve IDP** tarafından başlatılan

## <a name="adding-rfpio-from-the-gallery"></a>Galeriden RFPIO ekleme

Azure AD'de RFPIO tümleştirmesini yapılandırmak için RFPIO Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden RFPIO eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. Gidin **kurumsal uygulamalar** seçip **tüm uygulamaları** seçeneği.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **RFPIO**seçin **RFPIO** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Sonuç listesinde RFPIO](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma RFPIO adlı bir test kullanıcı tabanlı test **Britta Simon**.
Tek iş için oturum açma için bir Azure AD kullanıcısının RFPIO ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma RFPIO ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[RFPIO çoklu oturum açmayı yapılandırma](#configure-rfpio-single-sign-on)**  - uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
3. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[RFPIO test kullanıcısı oluşturma](#create-rfpio-test-user)**  - kullanıcı Azure AD gösterimini bağlı RFPIO Britta simon'un bir karşılığı vardır.
6. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin.

Azure AD çoklu oturum açma ile RFPIO yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. İçinde [Azure portalında](https://portal.azure.com/), **RFPIO** uygulama tümleştirme sayfasında **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma](common/select-sso.png)

2. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** iletişim kutusunda, **SAML/WS-Federasyon** modu, çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma seçim modu](common/select-saml-option.png)

3. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için simgeyi **temel SAML yapılandırma** iletişim.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. Üzerinde **temel SAML yapılandırma** uygulamada yapılandırmak isterseniz, bölümü **IDP** modunda başlatılan aşağıdaki adımı uygulayın:

    ![RFPIO etki alanı ve URL'ler tek oturum açma bilgileri](common/idp-identifier.png)

    a. İçinde **tanımlayıcı** metin kutusuna şu biçimi kullanarak bir URL yazın: `https://www.rfpio.com`

    b. Tıklayın **ek URL'lerini ayarlayın**.

    c. İçinde **geçiş durumu** metin kutusuna bir dize değeri. İlgili kişi [RFPIO Destek ekibine](https://www.rfpio.com/contact/) bu değeri alınamıyor.

    ![RFPIO etki alanı ve URL'ler tek oturum açma bilgileri](common/idp-preintegrated-relay.png)

5. Tıklayın **ek URL'lerini ayarlayın** ve uygulamada yapılandırmak istiyorsanız, aşağıdaki adımı uygulayın **SP** başlatılan modu:

    ![image](common/both-preintegrated-signon.png)

    İçinde **oturum açma URL'si** metin kutusuna şu biçimi kullanarak bir URL yazın:  `https://www.app.rfpio.com`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerler gerçek tanımlayıcısı ve oturum açma URL'si ile güncelleştirin. İlgili kişi [RFPIO istemci Destek ekibine](https://www.rfpio.com/contact/) bu değerleri almak için. Gösterilen desenleri de başvurabilirsiniz **temel SAML yapılandırma** bölümünde Azure portalında.

6. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **SAML imzalama sertifikası** bölümünde **indirme** indirmek için **Federasyon meta veri XML**  bilgisayarınızdaki belirli seçenekler ihtiyacınıza göre ve kaydedin.

    ![Sertifika indirme bağlantısı](common/metadataxml.png)

7. Üzerinde **RFPIO kümesi** bölümünde, ihtiyacınıza göre uygun URL'lerini kopyalayın.

    ![Yapılandırma URL'leri kopyalayın](common/copy-configuration-urls.png)

    a. Oturum Açma URL'si:

    b. Azure AD Tanımlayıcısı

    c. Oturum Kapatma URL'si

### <a name="configure-rfpio-single-sign-on"></a>RFPIO çoklu oturum açmayı yapılandırın

1. Farklı bir web tarayıcı penceresinde oturum açın **RFPIO** yönetici olarak Web sitesi.

1. Üzerindeki alt sol üst köşedeki açılır menüye tıklayın.

    ![Çoklu oturum açmayı yapılandırın](./media/rfpio-tutorial/app1.png)

1. Tıklayarak **kuruluş ayarlarına**. 

    ![Çoklu oturum açmayı yapılandırın](./media/rfpio-tutorial/app2.png)

1. Tıklayarak **özellikler ve tümleştirme**.

    ![Çoklu oturum açmayı yapılandırın](./media/rfpio-tutorial/app4.png)

1. İçinde **SAML SSO yapılandırma** tıklayın **Düzenle**.

    ![Çoklu oturum açmayı yapılandırın](./media/rfpio-tutorial/app3.png)

1. Bu bölümde, eylemleri gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/rfpio-tutorial/app5.png)
    
    a. İçeriğini kopyalayın **indirilen meta veri XML** yapıştırın **kimlik Yapılandırması** alan.

    > [!NOTE]
    >İçeriği kopyalamak için indirilen **Federasyon meta verileri XML** kullanım **not defteri ++** veya uygun **XML Düzenleyicisi**.

    b. Tıklayın **doğrulama**.

    c. ' I tıklattıktan sonra **doğrulama**Çevir, **SAML(Enabled)** açık.

    d. **Gönder**'e tıklayın.

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

1. Azure portalında, sol bölmede seçin **Azure Active Directory**seçin **kullanıcılar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](common/users.png)

2. Seçin **yeni kullanıcı** ekranın üstünde.

    ![Yeni kullanıcı düğmesi](common/new-user.png)

3. Kullanıcı özellikleri, aşağıdaki adımları gerçekleştirin.

    ![Kullanıcı iletişim kutusu](common/user-properties.png)

    a. İçinde **adı** alana **BrittaSimon**.
  
    b. İçinde **kullanıcı adı** alan türü `brittasimon@yourcompanydomain.extension`. Örneğin, BrittaSimon@contoso.com

    c. Seçin **Show parola** onay kutusunu işaretleyin ve ardından parola kutusunda görüntülenen değeri yazın.

    d. **Oluştur**’a tıklayın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Azure çoklu oturum açma kullanmak için RFPIO erişim vererek Britta Simon etkinleştirin.

1. Azure portalında **kurumsal uygulamalar**seçin **tüm uygulamaları**, ardından **RFPIO**.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde **RFPIO**.

    ![Uygulamalar listesinde RFPIO bağlantı](common/all-applications.png)

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

4. Tıklayın **Kullanıcı Ekle** düğmesine ve ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** 'a tıklayın kullanıcı listesinde **seçin** ekranın alt kısmındaki düğmesi.

6. SAML onaylaması ardından içinde herhangi bir rolü değer bekleniyor durumunda **rolü Seç** 'a tıklayın listeden bir kullanıcı için uygun rolü Seç iletişim kutusu **seçin** ekranın alt kısmındaki düğmesi.

7. İçinde **atama Ekle** iletişim tıklatın **atama** düğmesi.

### <a name="create-rfpio-test-user"></a>RFPIO test kullanıcısı oluşturma

1. RFPIO şirketinizin sitesi için bir yönetici olarak oturum açın.

1. Üzerindeki alt sol üst köşedeki açılır menüye tıklayın.

    ![Çoklu oturum açmayı yapılandırın](./media/rfpio-tutorial/app1.png)

1. Tıklayarak **kuruluş ayarlarına**. 

    ![Çoklu oturum açmayı yapılandırın](./media/rfpio-tutorial/app2.png)

1. Tıklayın **TAKIM ÜYELERİ**.

    ![Çoklu oturum açmayı yapılandırın](./media/rfpio-tutorial/app6.png)

1. Tıklayarak **ÜYELER Ekle**.

    ![Çoklu oturum açmayı yapılandırın](./media/rfpio-tutorial/app7.png)

1. İçinde **eklediğiniz yeni üyeler** bölümü. Eylemleri gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/rfpio-tutorial/app8.png)

    a. ENTER **e-posta adresi** içinde **her satırda bir e-posta girin** alan.

    b. Lütfen seçin **rol** gereksinimlerinize göre.

    c. Tıklayın **ÜYELER Ekle**.

    > [!NOTE]
    > Azure Active Directory hesap sahibinin e-posta alır ve etkin hale gelir önce hesabını onaylamak için bir bağlantı izler.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli RFPIO kutucuğa tıkladığınızda, size otomatik olarak SSO'yu ayarlama RFPIO için oturum açmanız. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

