---
title: 'Öğretici: GmbH çözünürlüğün Jıra için SAML SSO ile Azure Active Directory Tümleştirme | Microsoft Docs'
description: Çoklu oturum açma SAML SSO Jıra için Azure Active Directory arasındaki GmbH çözünürlüğün yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 20e18819-e330-4e40-bd8d-2ff3b98e035f
ms.service: Azure-Active-Directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 01/03/2018
ms.author: jeedes
ms.openlocfilehash: 3e527965782cc951553a5b5721955d4d3cfe67c6
ms.sourcegitcommit: 3ab534773c4decd755c1e433b89a15f7634e088a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/07/2019
ms.locfileid: "54065486"
---
# <a name="tutorial-azure-active-directory-integration-with-saml-sso-for-jira-by-resolution-gmbh"></a>Öğretici: GmbH çözünürlüğün Jıra için SAML SSO ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile GmbH çözünürlüğün Jıra için SAML SSO tümleştirme konusunda bilgi edinin.
SAML SSO için Jıra çözünürlüğün GmbH Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

* SAML SSO için Jıra GmbH çözünürlüğün erişimi, Azure AD'de kontrol edebilirsiniz.
* Otomatik olarak Jıra için SAML SSO (çoklu oturum açma) GmbH çözünürlüğüyle kendi Azure AD hesapları ile oturum, kullanıcıların etkinleştirebilirsiniz.
* Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirmesi GmbH çözünürlüğüyle Jıra için SAML SSO ile yapılandırmak için aşağıdaki öğeler gerekir:

* Azure AD aboneliğiniz. Bir Azure AD ortamını yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/)
* Abonelik Jıra çözünürlüğün GmbH çoklu oturum açma için SAML SSO etkin

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, yapılandırma ve Azure AD çoklu oturum açma bir test ortamında test edin.

* SAML SSO için Jıra GmbH destekler çözünürlüğün **SP** ve **IDP**tarafından başlatılan

## <a name="adding-saml-sso-for-jira-by-resolution-gmbh-from-the-gallery"></a>SAML SSO için Jıra GmbH çözünürlüğüyle galeri ekleme

Jıra için SAML SSO, Azure AD'de tümleştirmesini GmbH çözünürlüğün yapılandırmak için Jıra için SAML SSO GmbH çözünürlüğüyle Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**SAML SSO için Jıra GmbH çözünürlüğüyle Galeriden eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. Gidin **kurumsal uygulamalar** seçip **tüm uygulamaları** seçeneği.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **GmbH çözünürlüğün Jıra için SAML SSO**seçin **GmbH çözünürlüğün Jıra için SAML SSO** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine .

     ![SAML SSO için sonuç listesinde GmbH çözünürlüğün Jıra](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma SAML SSO ile Jıra GmbH çözünürlüğün adlı bir test kullanıcı tabanlı için test **Britta Simon**.
Tek iş için oturum açma için bir Azure AD kullanıcısı ve SAML SSO Jıra için ilgili kullanıcı çözünürlüğün arasında bir bağlantı ilişki GmbH kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma SAML SSO için Jıra ile GmbH çözünürlüğün sınamak için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[SAML SSO için Jıra çözünürlüğüyle GmbH çoklu oturum açmayı yapılandırma](#configure-saml-sso-for-jira-by-resolution-gmbh-single-sign-on)**  - uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
3. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[SAML SSO için çözüm GmbH test kullanıcısı tarafından Jıra oluşturma](#create-saml-sso-for-jira-by-resolution-gmbh-test-user)**  - kullanıcı Azure AD gösterimini bağlı GmbH çözünürlüğün Jıra için SAML SSO içinde bir karşılığı Britta simon'un sağlamak için.
6. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin.

Azure AD çoklu oturum açma SAML SSO için Jıra ile GmbH çözünürlüğüyle yapılandırmak için aşağıdaki adımları uygulayın:

1. İçinde [Azure portalında](https://portal.azure.com/), **GmbH çözünürlüğün Jıra için SAML SSO** uygulama tümleştirme sayfasında **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma](common/select-sso.png)

2. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** iletişim kutusunda, **SAML/WS-Federasyon** modu, çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma seçim modu](common/select-saml-option.png)

3. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için simgeyi **temel SAML yapılandırma** iletişim.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. Üzerinde **temel SAML yapılandırma** uygulamada yapılandırmak isterseniz, bölümü **IDP** başlatılan modu, aşağıdaki adımları gerçekleştirin:

    ![Oturum açma bilgileri tek bir SAML SSO için Jıra çözünürlüğün GmbH etki alanı ve URL'ler](common/idp-intiated.png)

    a. İçinde **tanımlayıcı** metin kutusuna şu biçimi kullanarak bir URL yazın: `https://<server-base-url>/plugins/servlet/samlsso`

    b. İçinde **yanıt URL'si** metin kutusuna şu biçimi kullanarak bir URL yazın: `https://<server-base-url>/plugins/servlet/samlsso`

    c. Tıklayın **ek URL'lerini ayarlayın** ve uygulamada yapılandırmak istiyorsanız aşağıdaki adımı gerçekleştirmeniz **SP** başlatılan modu:

    ![Oturum açma bilgileri tek bir SAML SSO için Jıra çözünürlüğün GmbH etki alanı ve URL'ler](common/metadata-upload-additional-signon.png)

    İçinde **oturum açma URL'si** metin kutusuna şu biçimi kullanarak bir URL yazın:  `https://<server-base-url>/plugins/servlet/samlsso`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerler gerçek tanımlayıcısı, yanıt URL'si ve oturum açma URL'si ile güncelleştirin. İlgili kişi [SAML SSO Jıra çözünürlüğün GmbH istemci için Destek ekibine](https://www.resolution.de/go/support) bu değerleri almak için. Gösterilen desenleri de başvurabilirsiniz **temel SAML yapılandırma** bölümünde Azure portalında.

4. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **SAML imzalama sertifikası** bölümünde **indirme** indirmek için **Federasyon meta veri XML**  bilgisayarınızdaki belirli seçenekler ihtiyacınıza göre ve kaydedin.

    ![Sertifika indirme bağlantısı](common/metadataxml.png)

6. Üzerinde **GmbH çözünürlüğüyle Jıra için SAML SSO ayarlama** bölümünde, ihtiyacınıza göre uygun URL'lerini kopyalayın.

    ![Yapılandırma URL'leri kopyalayın](common/copy-configuration-urls.png)

    a. Oturum Açma URL'si:

    b. Azure Ad tanımlayıcısı

    c. Oturum Kapatma URL'si

### <a name="configure-saml-sso-for-jira-by-resolution-gmbh-single-sign-on"></a>SAML SSO için Jıra çözünürlüğüyle GmbH çoklu oturum açmayı yapılandırın

1. Farklı bir web tarayıcı penceresinde oturum açın, **SAML SSO için çözüm GmbH Yönetici portalı tarafından Jıra** yönetici olarak.

2. Dişlisine gelin ve tıklayın **eklentileri**.
    
    ![Çoklu oturum açmayı yapılandırın](./media/samlssojira-tutorial/addon1.png)

3. Yönetici erişimi sayfasına yönlendirilirsiniz. Girin **parola** tıklatıp **Onayla** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/samlssojira-tutorial/addon2.png)

4. Eklentiler sekmesi bölümü altında **yeni eklentileri bulma**. Arama **SAML çoklu oturum açma (SSO) için JIRA** tıklatıp **yükleme** yeni SAML eklentisini yüklemek için düğme.

    ![Çoklu oturum açmayı yapılandırın](./media/samlssojira-tutorial/addon7.png)

5. Eklenti yüklemesi başlatılır. **Kapat**’a tıklayın.

    ![Çoklu oturum açmayı yapılandırın](./media/samlssojira-tutorial/addon8.png)

    ![Çoklu oturum açmayı yapılandırın](./media/samlssojira-tutorial/addon9.png)

6.  **Yönet**'e tıklayın.

    ![Çoklu oturum açmayı yapılandırın](./media/samlssojira-tutorial/addon10.png)
    
8. Tıklayın **yapılandırma** yeni eklentiyi yapılandırmak için.

    ![Çoklu oturum açmayı yapılandırın](./media/samlssojira-tutorial/addon11.png)

9. Üzerinde **SAML SingleSignOn eklentisi yapılandırma** sayfasında **yeni IDP ekleme** kimlik sağlayıcısı ayarlarını yapılandırmak için düğmeye.

    ![Çoklu oturum açmayı yapılandırın](./media/samlssojira-tutorial/addon4.png)

10. Üzerinde **SAML kimlik sağlayıcınızı seçin** sayfasında, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/samlssojira-tutorial/addon5a.png)
 
    a. Ayarlama **Azure AD'ye** IDP türü.
    
    b. Ekleme **adı** kimlik sağlayıcısının (ör. Azure AD).
    
    c. Ekleme **açıklama** kimlik sağlayıcısının (ör. Azure AD).
    
    d. **İleri**’ye tıklayın.
    
11. Üzerinde **kimlik sağlayıcı Yapılandırması** sayfasında **sonraki** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/samlssojira-tutorial/addon5b.png)

12. Üzerinde **meta verileri içeri aktarma SAML IDP** sayfasında, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/samlssojira-tutorial/addon5c.png)

    a. Tıklayın **yük dosyası** düğmesi ve 5. adımda indirdiğiniz meta veri XML dosyasını seçin.

    b. Tıklayın **alma** düğmesi.
    
    c. İçeri aktarma başarılı olana kadar kısa bir süre bekleyin.
    
    d. Tıklayın **sonraki** düğmesi.
    
13. Üzerinde **kullanıcı kimliği öznitelik ve dönüştürme** sayfasında **sonraki** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/samlssojira-tutorial/addon5d.png)
    
14. Üzerinde **kullanıcı oluşturma ve güncelleştirme** sayfasında **sonraki & Kaydet** ayarları kaydedin.   
    
    ![Çoklu oturum açmayı yapılandırın](./media/samlssojira-tutorial/addon6a.png)
    
15. Üzerinde **ayarlarınızı test etme** sayfasında **test atlayın ve el ile yapılandırma** kullanıcı test şimdilik atlamak için. Bu, sonraki bölümde gerçekleştirilir ve bazı ayarlar Azure portalında gerektirir. 
    
    ![Çoklu oturum açmayı yapılandırın](./media/samlssojira-tutorial/addon6b.png)
    
16. Apprearing iletişim okuma **test yolları atlanıyor...** , tıklayın **Tamam**.
    
    ![Çoklu oturum açmayı yapılandırın](./media/samlssojira-tutorial/addon6c.png)

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma 

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

1. Azure portalında, sol bölmede seçin **Azure Active Directory**seçin **kullanıcılar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](common/users.png)

2. Seçin **yeni kullanıcı** ekranın üstünde.

    ![Yeni kullanıcı düğmesi](common/new-user.png)

3. Kullanıcı özellikleri, aşağıdaki adımları gerçekleştirin.

    ![Kullanıcı iletişim kutusu](common/user-properties.png)

    a. İçinde **adı** alana **BrittaSimon**.
  
    b. İçinde **kullanıcı adı** alan türü **brittasimon@yourcompanydomain.extension**  
    Örneğin, BrittaSimon@contoso.com

    c. Seçin **Show parola** onay kutusunu işaretleyin ve ardından parola kutusunda görüntülenen değeri yazın.

    d. **Oluştur**’a tıklayın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Azure çoklu oturum açma GmbH çözünürlüğüyle Jıra için SAML SSO için erişim vererek kullanmak Britta Simon etkinleştirin.

1. Azure portalında **kurumsal uygulamalar**seçin **tüm uygulamaları**, ardından **GmbH çözünürlüğün Jıra için SAML SSO**.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde yazın ve **GmbH çözünürlüğün Jıra için SAML SSO**.

    ![SAML SSO uygulamaları listesinde çözümleme GmbH bağlantısıyla Jıra için](common/all-applications.png)

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

4. Tıklayın **Kullanıcı Ekle** düğmesine ve ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** 'a tıklayın kullanıcı listesinde **seçin** ekranın alt kısmındaki düğmesi.

6. SAML onaylaması ardından içinde herhangi bir rolü değer bekleniyor durumunda **rolü Seç** 'a tıklayın listeden bir kullanıcı için uygun rolü Seç iletişim kutusu **seçin** ekranın alt kısmındaki düğmesi.

7. İçinde **atama Ekle** iletişim tıklatın **atama** düğmesi.

### <a name="create-saml-sso-for-jira-by-resolution-gmbh-test-user"></a>SAML SSO için çözüm GmbH test kullanıcısı tarafından Jıra oluşturma

SAML SSO için Jıra GmbH çözünürlüğüyle oturum açmak Azure AD kullanıcılarının etkinleştirmek için bunlar Jıra için SAML SSO içine GmbH çözüm tarafından sağlanması gerekir.  
SAML SSO GmbH çözünürlüğün Jıra, sağlama, el ile bir görev olur.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1. Yönetici olarak, SAML SSO Jıra çözümleme GmbH şirket site tarafından için oturum açın.

2. Dişlisine gelin ve tıklayın **kullanıcı yönetimi**.

    ![Çalışan Ekle](./media/samlssojira-tutorial/user1.png) 

3. Girmek için yönetici erişimi sayfasına yönlendirilirsiniz **parola** tıklatıp **Onayla** düğmesi.

    ![Çalışan Ekle](./media/samlssojira-tutorial/user2.png) 

4. Altında **kullanıcı yönetimi** sekmesinde bölüm **oluşturacağı**.

    ![Çalışan Ekle](./media/samlssojira-tutorial/user3.png) 

5. Üzerinde **"Yeni kullanıcı oluşturma"** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:

    ![Çalışan Ekle](./media/samlssojira-tutorial/user4.png) 

    a. İçinde **e-posta adresi** metin kutusuna kullanıcı e-posta adresi türünü ister Brittasimon@contoso.com.

    b. İçinde **tam adı** metin Britta Simon gibi kullanıcının tam adını yazın.

    c. İçinde **kullanıcıadı** metin kutusuna kullanıcı e-posta türünü ister Brittasimon@contoso.com.

    d. İçinde **parola** metin kutusu, kullanıcı parolasını yazın.

    e. Tıklayın **kullanıcı oluşturma**.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi 

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Çözüm GmbH kutucuk erişim Paneli'nde tarafından Jıra için SAML SSO'ye tıkladığınızda, otomatik olarak Jıra için SAML SSO için SSO'yu ayarlama çözümleme GmbH tarafından oturum açmanız. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek Kaynaklar

- [ SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi ](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir? ](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

