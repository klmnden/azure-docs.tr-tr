---
title: 'Öğretici: SAP HANA ile Azure Active Directory Tümleştirme | Microsoft Docs'
description: Azure Active Directory ile SAP HANA arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: barbkess
ms.assetid: cef4a146-f4b0-4e94-82de-f5227a4b462c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 12/27/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 231b9b6d217a9ad1fe5f4a6478f5e8799257b92b
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67091619"
---
# <a name="tutorial-azure-active-directory-integration-with-sap-hana"></a>Öğretici: SAP HANA ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile SAP HANA tümleştirme konusunda bilgi edinin.
Azure AD ile SAP HANA tümleştirme ile aşağıdaki avantajları sağlar:

* SAP HANA erişimi, Azure AD'de kontrol edebilirsiniz.
* Azure AD hesaplarına otomatik olarak (çoklu oturum açma) SAP HANA için oturum açmış, kullanıcıların etkinleştirebilirsiniz.
* Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

SAP HANA ile Azure AD tümleştirmesini yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliği
- Çoklu oturum etkin açma (SSO) bir SAP HANA abonelik
- Ortak bir Iaas üzerinde çalışan bir HANA örneği şirket içi, Azure VM veya Azure büyük örneklerde SAP
- HANA Studio HANA örneğinde yüklü yanı sıra XSA Yönetim web arabirimi

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamında SAP hana kullanarak önermiyoruz. Tümleşik geliştirme ya da hazırlık ortamı uygulamanın önce test ve üretim ortamına kullanın.

Bu öğreticideki adımları test etmek için aşağıdaki önerileri uygulayın:

* Azure AD aboneliğiniz. Bir Azure AD ortamını yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/)
* SAP HANA çoklu oturum açma abonelik etkin.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, yapılandırma ve Azure AD çoklu oturum açma bir test ortamında test edin.

* SAP HANA destekler **IDP** tarafından başlatılan
* SAP HANA destekler **just-ın-time** kullanıcı sağlama

## <a name="adding-sap-hana-from-the-gallery"></a>SAP HANA galeri ekleme

SAP HANA'ın Azure AD'ye tümleştirmesini yapılandırmak için SAP HANA Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**SAP HANA Galeriden eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)** , sol gezinti panelinde tıklayın **Azure Active Directory** simgesi.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. Gidin **kurumsal uygulamalar** seçip **tüm uygulamaları** seçeneği.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **SAP HANA**seçin **SAP HANA** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

     ![Sonuç listesinde SAP HANA](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma adlı bir test kullanıcı dayalı SAP HANA ile test etme **Britta Simon**.
Tek iş için oturum açma için bir Azure AD kullanıcısı ile SAP hana ilgili kullanıcı arasında bir bağlantı ilişki kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma SAP HANA ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[SAP HANA çoklu oturum açmayı yapılandırma](#configure-sap-hana-single-sign-on)**  - uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
3. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[SAP HANA test kullanıcısı oluşturma](#create-sap-hana-test-user)**  - kullanıcı Azure AD gösterimini bağlı SAP hana Britta simon'un bir karşılığı vardır.
6. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin.

Azure AD çoklu oturum açma ile SAP HANA yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. İçinde [Azure portalında](https://portal.azure.com/), **SAP HANA** uygulama tümleştirme sayfasında **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma](common/select-sso.png)

2. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** iletişim kutusunda, **SAML/WS-Federasyon** modu, çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma seçim modu](common/select-saml-option.png)

3. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için simgeyi **temel SAML yapılandırma** iletişim.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında, aşağıdaki adımları gerçekleştirin:

    ![SAP HANA etki alanı ve URL'ler tek oturum açma bilgileri](common/idp-intiated.png)

    a. İçinde **tanımlayıcı** metin kutusunda, aşağıdaki komutu yazın: `HA100`

    b. İçinde **yanıt URL'si** metin kutusuna şu biçimi kullanarak bir URL yazın: `https://<Customer-SAP-instance-url>/sap/hana/xs/saml/login.xscfunc`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerler gerçek tanımlayıcısı ve yanıt URL'si ile güncelleştirin. İlgili kişi [SAP HANA istemci Destek ekibine](https://cloudplatform.sap.com/contact.html) bu değerleri almak için. Gösterilen desenleri de başvurabilirsiniz **temel SAML yapılandırma** bölümünde Azure portalında.

5. SAP HANA uygulama belirli bir biçimde SAML onaylamalarını bekler. Bu uygulama için aşağıdaki talepleri yapılandırın. Bu öznitelikleri değerlerini yönetebilirsiniz **kullanıcı öznitelikleri** uygulama tümleştirme sayfasında bölümü. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için düğmeyi **kullanıcı öznitelikleri** iletişim.

    ![image](common/edit-attribute.png)

6. İçinde **kullanıcı öznitelikleri** bölümünde **kullanıcı öznitelikleri ve talepler** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:
 
    a. Tıklayın **düzenleme simgesi** açmak için **yönetmek, kullanıcı talepleri** iletişim.

    ![image](./media/saphana-tutorial/tutorial_usermail.png)

    ![image](./media/saphana-tutorial/tutorial_usermailedit.png)

    b. Gelen **dönüştürme** listesinden **ExtractMailPrefix()** .

    c. Gelen **parametresi 1** listesinden **user.mail**.

    d. **Kaydet**’e tıklayın.

7. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **SAML imzalama sertifikası** bölümünde **indirme** indirmek için **Federasyon meta veri XML**  bilgisayarınızdaki belirli seçenekler ihtiyacınıza göre ve kaydedin.

    ![Sertifika indirme bağlantısı](common/metadataxml.png)

### <a name="configure-sap-hana-single-sign-on"></a>SAP HANA çoklu oturum açmayı yapılandırın

1. Çoklu oturum açma SAP HANA tarafta yapılandırmak için oturum açın, **HANA XSA Web Konsolu** ilgili HTTPS uç noktasına giderek.

    > [!NOTE]
    > Varsayılan yapılandırmasında URL'sini, istek kimliği doğrulanmış bir SAP HANA veritabanı kullanıcının kimlik bilgileri gerektiren bir oturum açma ekranına yönlendirir. Kullanıcı oturum açtığında SAML yönetim görevlerini gerçekleştirmek için izinleri olmalıdır.

2. XSA Web arabiriminde Git **SAML kimlik sağlayıcısı**. Buradan seçin **+** düğmesini görüntülemek için ekranın alt kısmındaki **kimlik sağlayıcı bilgileri ekleme** bölmesi. Ardından aşağıdaki adımları uygulayın:

    ![Kimlik sağlayıcısı Ekle](./media/saphana-tutorial/sap1.png)

    a. İçinde **kimlik sağlayıcı bilgileri ekleyin** bölmesi (Bu, Azure portalından indirdiğiniz) meta verileri XML içeriğini yapıştırın **meta verileri** kutusu.

    ![Kimlik sağlayıcı ayarları Ekle](./media/saphana-tutorial/sap2.png)

    b. XML belgesinin içeriğini geçerliyse, ayrıştırma işlemi için gerekli olan bilgileri ayıklayan **konu, varlık kimliği ve sertifikayı veren** alanlarını **genel veri** ekran alanı. Ayrıca, URL alanları için gereken bilgileri ayıklar **hedef** ekran alanı, örneğin, **temel URL ve SingleSignOn URL (*)** alanları.

    ![Kimlik sağlayıcı ayarları Ekle](./media/saphana-tutorial/sap3.png)

    c. İçinde **adı** kutusunun **genel veri** ekran alanı, yeni SAML SSO kimlik sağlayıcısı için bir ad girin.

    > [!NOTE]
    > SAML IDP adı zorunludur ve benzersiz olmalıdır. Kullanılacak SAP HANA XS uygulamalar için kimlik doğrulama yöntemi olarak SAML seçeneğini belirlediğinizde görüntülenen kullanılabilir SAML Idp'yi listesinde görünür. Örneğin, bunu yapabilirsiniz **kimlik doğrulaması** ekran alanı XS Yapıt yönetim aracı.

3. Seçin **Kaydet** SAML kimlik sağlayıcısı ile ilgili ayrıntıları kaydetmeyi ve yeni SAML IDP bilinen SAML IDP listesine eklemek için.

    ![Kaydet düğmesi](./media/saphana-tutorial/sap4.png)

4. Sistem Özellikleri içinde HANA Studio **yapılandırma** sekmesinde, filtre tarafından ayarları **saml**. Ardından ayarlamak **assertion_timeout** gelen **10 sn** için **120 saniye**.

    ![assertion_timeout ayarı](./media/saphana-tutorial/sap7.png)

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

Bu bölümde, SAP HANA için erişim izni verdiğinizde, Azure çoklu oturum açma kullanılacak Britta Simon etkinleştirin.

1. Azure portalında **kurumsal uygulamalar**seçin **tüm uygulamaları**, ardından **SAP HANA**.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde yazın ve **SAP HANA**.

    ![Uygulamalar listesinde SAP HANA bağlantısı](common/all-applications.png)

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

4. Tıklayın **Kullanıcı Ekle** düğmesine ve ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** 'a tıklayın kullanıcı listesinde **seçin** ekranın alt kısmındaki düğmesi.

6. SAML onaylaması ardından içinde herhangi bir rolü değer bekleniyor durumunda **rolü Seç** 'a tıklayın listeden bir kullanıcı için uygun rolü Seç iletişim kutusu **seçin** ekranın alt kısmındaki düğmesi.

7. İçinde **atama Ekle** iletişim tıklatın **atama** düğmesi.

### <a name="create-sap-hana-test-user"></a>SAP HANA test kullanıcısı oluşturma

SAP HANA için oturum açmak Azure AD kullanıcılarının etkinleştirmek için SAP HANA'da hazırlamanız gerekir.
SAP HANA destekler **tam zamanında sağlama**, hangi tarafından varsayılan olarak etkindir.

Bir kullanıcı el ile oluşturmanız gerekiyorsa, aşağıdaki adımları uygulayın:

>[!NOTE]
>Kullanıcının kullandığı Dış kimlik doğrulama değiştirebilirsiniz. Kerberos gibi bir dış sistemle doğrulayabilir. Dış kimlikler hakkında ayrıntılı bilgi için kişi, [etki alanı yöneticisi](https://cloudplatform.sap.com/contact.html).

1. Açık [SAP HANA Studio](https://help.sap.com/viewer/a2a49126a5c546a9864aae22c05c3d0e/2.0.01/en-us) bir Yöneticiyseniz ve ardından etkinleştir DB kullanıcı SAML SSO olarak.

    ![Kullanıcı oluştur](./media/saphana-tutorial/sap5.png)

2. Görünmeyen sol tarafındaki onay kutusunu işaretleyin **SAML**ve ardından **yapılandırma** bağlantı.

3. Seçin **Ekle** SAML IDP eklemek için.  Uygun SAML IDP seçmek ve ardından **Tamam**.

4. Ekleme **Dış kimlik** (Bu durumda, BrittaSimon) ya da seçin **herhangi**. Sonra **Tamam**’ı seçin.

   > [!Note]
   > Varsa **herhangi** onay kutusu seçilmez ve HANA kullanıcı adı UPN etki alanı soneki önce kullanıcı adı tam olarak eşleşmesi gerekir. (Örneğin, BrittaSimon@contoso.com hana BrittaSimon olur.)

5. Test amacıyla, tüm Ata **XS** kullanıcı rolleri.

    ![Rol atama](./media/saphana-tutorial/sap6.png)

    > [!TIP]
    > Yalnızca, kullanım durumları için uygun izinleri vermeniz gerekir.

6. Kullanıcı kaydedin.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi 

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli SAP HANA kutucuğa tıkladığınızda, size otomatik olarak SAP HANA için SSO'yu ayarlamak için oturum açmanız. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

