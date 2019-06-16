---
title: 'Öğretici: N2F - Azure Active Directory tümleştirmesiyle gider raporlarını | Microsoft Docs'
description: Azure Active Directory ve N2F - arasında çoklu oturum açmayı yapılandırma, Gider raporlarını öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: f56d53d7-5a08-490a-bfb9-78fefc2751ec
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/01/2019
ms.author: jeedes
ms.openlocfilehash: 0a7b38f26261625c6db8acb6653b3cd9353fdcc2
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67096513"
---
# <a name="tutorial-azure-active-directory-integration-with-n2f---expense-reports"></a>Öğretici: Harcama N2F - Azure Active Directory tümleştirmesiyle raporları

Bu öğreticide, Azure Active Directory (Azure AD) ile gider raporlarını N2F - tümleştirmek nasıl öğrenin.
Tümleştirme N2F - Azure AD ile gider raporlarını ile aşağıdaki avantajları sağlar:

* N2F - erişimi, Azure AD'de denetleyebilirsiniz gider bildirir.
* Otomatik olarak N2F - gider raporlarını (çoklu oturum açma) ile Azure AD hesaplarına oturum açmış, kullanıcıların etkinleştirebilirsiniz.
* Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirmesi N2F - gider raporlarını yapılandırmak için aşağıdaki öğeler gerekir:

* Azure AD aboneliğiniz. Bir Azure AD ortamını yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/)
* N2F - gider raporlarını tek oturum açma etkin abonelik

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, yapılandırma ve Azure AD çoklu oturum açma bir test ortamında test edin.

* N2F - gider raporlarını destekler **SP** ve **IDP** tarafından başlatılan

## <a name="adding-n2f---expense-reports-from-the-gallery"></a>N2F - gider raporlarını galeri ekleme

N2F - eklemenize gerek N2F - Azure AD'de, Gider raporlarını tümleştirmesini yapılandırmak için yönetilen SaaS listenizden galerisinden raporlar gider.

**N2F - galerisinden, Gider raporlarını eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)** , sol gezinti panelinde tıklayın **Azure Active Directory** simgesi.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. Gidin **kurumsal uygulamalar** seçip **tüm uygulamaları** seçeneği.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **N2F - gider raporlarını**seçin **N2F - gider raporlarını** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

     ![N2F - gider raporlarını sonuçları listesi](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve test Azure AD çoklu oturum açma ile N2F - gider raporlarını bir test kullanıcı tabanlı olarak adlandırılan **Britta Simon**.
Çoklu oturum açma iş, bir Azure AD kullanıcısının N2F - ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekiyorsa gider bildirir.

Yapılandırma ve Azure AD çoklu oturum açma ile N2F - gider raporlarını, test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[N2F yapılandırma - gider raporlarını çoklu oturum açma](#configure-n2f---expense-reports-single-sign-on)**  - uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
3. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[N2F Oluştur - gider raporlarını kullanıcı test](#create-n2f---expense-reports-test-user)**  - N2F içinde bir karşılığı Britta simon'un sağlamak için - gider kullanıcı Azure AD gösterimini bağlantılı raporlar.
6. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin.

Azure AD yapılandırmak için çoklu oturum açma ile N2F - gider raporlarını, aşağıdaki adımları gerçekleştirin:

1. İçinde [Azure portalında](https://portal.azure.com/), **N2F - gider raporlarını** uygulama tümleştirme sayfasında **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma](common/select-sso.png)

2. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** iletişim kutusunda, **SAML/WS-Federasyon** modu, çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma seçim modu](common/select-saml-option.png)

3. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için simgeyi **temel SAML yapılandırma** iletişim.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. Üzerinde **temel SAML yapılandırma** uygulamada yapılandırmak isterseniz, bölümü **IDP** başlatılan modu, kullanıcı uygulamaya zaten Azure ile önceden tümleştirilmiş olduğu gibi tüm adımları gerçekleştirmek sahip değil.

    ![Çoklu oturum açma bilgileri N2F - gider raporlarını etki alanı ve URL'ler](common/preintegrated.png)

5. Tıklayın **ek URL'lerini ayarlayın** ve uygulamada yapılandırmak istiyorsanız, aşağıdaki adımı uygulayın **SP** başlatılan modu:

    ![Çoklu oturum açma bilgileri N2F - gider raporlarını etki alanı ve URL'ler](common/metadata-upload-additional-signon.png)

    İçinde **oturum açma URL'si** metin kutusuna bir URL yazın:  `https://www.n2f.com/app/`

6. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlayın** sayfasında **SAML imzalama sertifikası** bölümünde, kopyalamak için Kopyala düğmesine **uygulama Federasyon meta verileri URL'sini** ve üzerinde kaydedin, bilgisayar.

    ![Sertifika indirme bağlantısı](common/copy-metadataurl.png)

7. Üzerinde **myPolicies kümesi** bölümünde, ihtiyacınıza göre uygun URL'lerini kopyalayın.

    ![Yapılandırma URL'leri kopyalayın](common/copy-configuration-urls.png)

    a. Oturum Açma URL'si:

    b. Azure AD Tanımlayıcısı

    c. Oturum Kapatma URL'si

### <a name="configure-n2f---expense-reports-single-sign-on"></a>N2F - gider raporlarını çoklu oturum açmayı yapılandırın

1. Farklı bir web tarayıcı penceresinde N2F - gider raporlarını şirket site yönetici olarak oturum açın.

2. Tıklayarak **ayarları** seçip **Gelişmiş ayarlar** açılır listeden.

    ![N2F - gider raporlarını yapılandırma](./media/n2f-expensereports-tutorial/configure1.png)

3. Seçin **hesap ayarları** sekmesi.

    ![N2F - gider raporlarını yapılandırma](./media/n2f-expensereports-tutorial/configure2.png)

4. Seçin **kimlik doğrulaması** seçip **+ kimlik doğrulama yöntemi Ekle** sekmesi.

    ![N2F - gider raporlarını yapılandırma](./media/n2f-expensereports-tutorial/configure3.png)

5. Seçin **SAML Microsoft Office 365** kimlik doğrulama yöntemi olarak.

    ![N2F - gider raporlarını yapılandırma](./media/n2f-expensereports-tutorial/configure4.png)

6. Üzerinde **kimlik doğrulama yöntemi** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![N2F - gider raporlarını yapılandırma](./media/n2f-expensereports-tutorial/configure5.png)

    a. İçinde **varlık kimliği** metin kutusu, yapıştırma **Azure AD tanımlayıcısı** Azure portaldan kopyaladığınız değeri.

    b. İçinde **meta veri URL'si** metin kutusu, yapıştırma **uygulama Federasyon meta verileri URL'sini** Azure portaldan kopyaladığınız değeri.

    c. **Kaydet**’e tıklayın.

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma 

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

1. Azure portalında, sol bölmede seçin **Azure Active Directory**seçin **kullanıcılar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](common/users.png)

2. Seçin **yeni kullanıcı** ekranın üstünde.

    ![Yeni kullanıcı düğmesi](common/new-user.png)

3. Kullanıcı özellikleri, aşağıdaki adımları gerçekleştirin.

    ![Kullanıcı iletişim kutusu](common/user-properties.png)

    a. İçinde **adı** alana **BrittaSimon**.
  
    b. İçinde **kullanıcı adı** alan türü **brittasimon\@yourcompanydomain.extension**  
    Örneğin, BrittaSimon@contoso.com

    c. Seçin **Show parola** onay kutusunu işaretleyin ve ardından parola kutusunda görüntülenen değeri yazın.

    d. **Oluştur**’a tıklayın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Azure çoklu oturum açma kullanmak için N2F erişim vererek Britta Simon enable - gider bildirir.

1. Azure portalında **kurumsal uygulamalar**seçin **tüm uygulamaları**, ardından **N2F - gider raporlarını**.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde **N2F - gider raporlarını**.

    ![-N2F gider bağlantıyı uygulamalar listesini bildirir.](common/all-applications.png)

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

4. Tıklayın **Kullanıcı Ekle** düğmesine ve ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** 'a tıklayın kullanıcı listesinde **seçin** ekranın alt kısmındaki düğmesi.

6. SAML onaylaması ardından içinde herhangi bir rolü değer bekleniyor durumunda **rolü Seç** 'a tıklayın listeden bir kullanıcı için uygun rolü Seç iletişim kutusu **seçin** ekranın alt kısmındaki düğmesi.

7. İçinde **atama Ekle** iletişim tıklatın **atama** düğmesi.

### <a name="create-n2f---expense-reports-test-user"></a>N2F oluşturma - kullanıcı gider raporlarını test

Azure AD kullanıcılarının N2F - gider raporlarını, oturum açmayı etkinleştirmek için bunların N2F - gider raporlarını sağlanması gerekir. N2F - gider raporlarını, söz konusu olduğunda sağlama bir el ile gerçekleştirilen bir görevdir.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1. N2F - gider raporlarını şirket site yönetici olarak oturum açın.

2. Tıklayarak **ayarları** seçip **Gelişmiş ayarlar** açılır listeden.

    ![N2F - kullanıcı gider Ekle](./media/n2f-expensereports-tutorial/configure1.png)

3. Seçin **kullanıcılar** sol gezinti bölmesi sekmesinden.

    ![N2F - gider raporlarını yapılandırma](./media/n2f-expensereports-tutorial/user1.png)

4. Seçin **+ yeni kullanıcı** sekmesi.

    ![N2F - gider raporlarını yapılandırma](./media/n2f-expensereports-tutorial/user2.png)

5. Üzerinde **kullanıcı** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![N2F - gider raporlarını yapılandırma](./media/n2f-expensereports-tutorial/user3.png)

    a. İçinde **e-posta adresi** metin gibi kullanıcı e-posta adresini girin **brittasimon\@contoso.com**.

    b. İçinde **ad** metin gibi kullanıcı adını girin **Britta**.

    c. İçinde **adı** metin gibi kullanıcı adını girin **BrittaSimon**.

    d. Seçin **doğrudan Yöneticisi (N + 1) rolü,** , ve **bölme** kuruluş ihtiyacınıza göre.

    e. Tıklayın **doğrula ve gönderme davet**.

    > [!NOTE]
    > Kullanıcı eklenirken herhangi bir sorunla karşılaşıyorsanız Lütfen başvurun [N2F - gider raporlarını Destek ekibine](mailto:support@n2f.com)

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi 

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

N2F tıklayın - erişim Paneli'nde gider raporlarını döşeme sonra otomatik olarak N2F - gider raporlarını SSO'yu ayarlamak için oturum açmanız. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

