---
title: 'Öğretici: Müşteri için SAP Cloud ile Azure Active Directory Tümleştirme | Microsoft Docs'
description: Çoklu oturum açma SAP bulut ile Azure Active Directory arasında müşteri için yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: barbkess
ms.assetid: 90154dab-eba2-4563-bcf0-f2acc797ea97
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 12/19/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 669dfaa40cfe1bc65618d8706910e19d72c233ad
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67092045"
---
# <a name="tutorial-azure-active-directory-integration-with-sap-cloud-for-customer"></a>Öğretici: Müşteri için SAP Cloud ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile müşteri için SAP bulut tümleştirme konusunda bilgi edinin.
Azure AD ile müşteri için SAP bulut tümleştirme ile aşağıdaki avantajları sağlar:

* Müşteri için SAP Cloud erişebilir, Azure AD'de kontrol edebilirsiniz.
* Otomatik olarak SAP Cloud (çoklu oturum açma) müşteri için kendi Azure AD hesapları ile oturum açmış, kullanıcıların etkinleştirebilirsiniz.
* Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirmesi, müşteri için SAP bulut ile yapılandırmak için aşağıdaki öğeler gerekir:

* Azure AD aboneliğiniz. Bir Azure AD ortamını yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/)
* Müşteri çoklu oturum açma için SAP Cloud abonelik etkin

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, yapılandırma ve Azure AD çoklu oturum açma bir test ortamında test edin.

* Müşteri desteklediği için SAP Cloud **SP** tarafından başlatılan

## <a name="adding-sap-cloud-for-customer-from-the-gallery"></a>SAP Cloud müşteri için Galeri ekleme

Azure AD'de müşteri için SAP Cloud tümleştirmesini yapılandırmak için SAP Cloud müşteri için Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**SAP Cloud müşteri için Galeriden eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)** , sol gezinti panelinde tıklayın **Azure Active Directory** simgesi.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. Gidin **kurumsal uygulamalar** seçip **tüm uygulamaları** seçeneği.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **müşteri için SAP Cloud**seçin **müşteri için SAP Cloud** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

     ![Sonuç listesinde müşteri için SAP Cloud](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve müşteri adlı bir test kullanıcı tabanlı için Azure AD çoklu oturum açma SAP Cloud ile test etme **Britta Simon**.
Tek iş için oturum açma için bir Azure AD kullanıcısı ve müşteri için SAP Cloud ilgili kullanıcı arasında bir bağlantı ilişki kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma SAP bulut ile müşteri için test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[SAP Cloud müşteri çoklu oturum açma için yapılandırma](#configure-sap-cloud-for-customer-single-sign-on)**  - uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
3. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Müşteri test kullanıcısı için SAP Cloud oluşturma](#create-sap-cloud-for-customer-test-user)**  - kullanıcı Azure AD gösterimini bağlı müşteri için SAP bulutta Britta simon'un bir karşılığı vardır.
6. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin.

Azure AD çoklu oturum açma müşteri için SAP bulut ile yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. İçinde [Azure portalında](https://portal.azure.com/), **müşteri için SAP Cloud** uygulama tümleştirme sayfasında **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma](common/select-sso.png)

2. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** iletişim kutusunda, **SAML/WS-Federasyon** modu, çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma seçim modu](common/select-saml-option.png)

3. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için simgeyi **temel SAML yapılandırma** iletişim.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. Üzerinde **temel SAML yapılandırma** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Müşteri etki alanı ve URL'ler tek oturum açma bilgileri için SAP Cloud](common/sp-identifier.png)

    a. İçinde **oturum açma URL'si** metin kutusuna şu biçimi kullanarak bir URL yazın: `https://<server name>.crm.ondemand.com`

    b. İçinde **tanımlayıcı (varlık kimliği)** metin kutusuna şu biçimi kullanarak bir URL yazın: `https://<server name>.crm.ondemand.com`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerler gerçek oturum açma URL ve tanımlayıcıdır ile güncelleştirin. İlgili kişi [müşteri istemci destek ekibi için SAP Cloud](https://www.sap.com/about/agreements.sap-cloud-services-customers.html) bu değerleri almak için. Gösterilen desenleri de başvurabilirsiniz **temel SAML yapılandırma** bölümünde Azure portalında.

5. Müşteri uygulaması için SAP Cloud, belirli bir biçimde SAML onaylamalarını bekliyor. Bu uygulama için aşağıdaki talepleri yapılandırın. Bu öznitelikleri değerlerini yönetebilirsiniz **kullanıcı öznitelikleri** uygulama tümleştirme sayfasında bölümü. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için düğmeyi **kullanıcı öznitelikleri** iletişim.

    ![image](common/edit-attribute.png)

6. İçinde **kullanıcı öznitelikleri** bölümünde **kullanıcı öznitelikleri ve talepler** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    a. Tıklayın **düzenleme simgesi** açmak için **yönetmek, kullanıcı talepleri** iletişim.

    ![image](./media/sap-customer-cloud-tutorial/tutorial_usermail.png)

    ![image](./media/sap-customer-cloud-tutorial/tutorial_usermailedit.png)

    b. Seçin **dönüştürme** olarak **kaynak**.

    c. Gelen **dönüştürme** listesinden **ExtractMailPrefix()** .

    d. Gelen **parametresi 1** listesinde, uygulamanız için kullanmak istediğiniz kullanıcı özniteliğini seçin.
    Örneğin, EmployeeID benzersiz kullanıcı tanımlayıcısı kullanmak istediğiniz ve öznitelik değeri içinde ExtensionAttribute2 depoladığınız seçerseniz, user.extensionattribute2 seçin.

    e. **Kaydet**’e tıklayın.

7. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **SAML imzalama sertifikası** bölümünde **indirme** indirmek için **Federasyon meta veri XML**  bilgisayarınızdaki belirli seçenekler ihtiyacınıza göre ve kaydedin.

    ![Sertifika indirme bağlantısı](common/metadataxml.png)

8. Üzerinde **müşteri için SAP bulut kümesi** bölümünde, ihtiyacınıza göre uygun URL'lerini kopyalayın.

    ![Yapılandırma URL'leri kopyalayın](common/copy-configuration-urls.png)

    a. Oturum Açma URL'si:

    b. Azure Ad tanımlayıcısı

    c. Oturum Kapatma URL'si

### <a name="configure-sap-cloud-for-customer-single-sign-on"></a>SAP bulut müşteri çoklu oturum açma için yapılandırma
   
1. Yönetici haklarıyla müşteri portalı için SAP Cloud oturum açın.
   
2. Gidin **uygulama ve kullanıcı yönetimi görevinin** tıklatıp **kimlik sağlayıcısı** sekmesi.
   
3. Tıklayın **yeni kimlik sağlayıcısı** ve Azure portalından indirilen meta veri XML dosyasını seçin. Meta verileri içeri aktararak, sistem otomatik olarak gerekli imza sertifikası ve şifreleme sertifikasını karşıya yükler.
   
    ![Çoklu oturum açmayı yapılandırın](./media/sap-customer-cloud-tutorial/tutorial_sapcloudforcustomer_54.png)
   
4. Azure Active Directory gerektiren ' % s'öğesi onay belgesi tüketici hizmeti URL'si seçin SAML isteğindeki **onay belgesi tüketici hizmeti URL'si dahil** onay kutusu.
   
5. Tıklayın **çoklu oturum açmayı etkinleştirme**.
   
6. Yaptığınız değişiklikleri kaydedin.
   
7. Tıklayın **My sistem** sekmesi.
   
    ![Çoklu oturum açmayı yapılandırın](./media/sap-customer-cloud-tutorial/tutorial_sapcloudforcustomer_52.png)
   
8. İçinde **Azure AD oturum açma URL'si** metin kutusu, yapıştırma **oturum açma URL'si** , Azure Portalı'ndan kopyaladığınız.
   
    ![Çoklu oturum açmayı yapılandırın](./media/sap-customer-cloud-tutorial/tutorial_sapcloudforcustomer_53.png)
   
9. Kullanıcı kimliği ve parola veya SSO seçerek oturum açma arasında çalışan el ile seçip seçemeyeceğini belirtin **el ile kimlik sağlayıcısı seçim**.
   
10. İçinde **SSO URL** bölümünde, çalışanlarınız tarafından sisteme oturum açmak için kullanılması gereken URL'yi belirtin. 
    İçinde **URL'ye gönderilen çalışana** listesinde, aşağıdaki seçenekler arasından seçim yapabilirsiniz:
   
    **SSO URL**
   
    Sistem, yalnızca normal sistem URL çalışana gönderir. Çalışan olamaz, SSO kullanarak oturum açın ve parolayı kullanın veya gerekir bunun yerine sertifika.
   
    **SSO URL'Sİ** 
   
    Sistem SSO URL çalışana gönderir. Çalışan SSO kullanarak oturum açabilir. Kimlik doğrulama isteği, IDP yönlendirilir.
   
    **Otomatik Seçim**
   
    SSO etkin değilse, sistemin normal sistem URL çalışana gönderir. SSO etkin olursa, sistem çalışan bir parolaya sahip olup olmadığını denetler. Bir parolası varsa, hem SSO URL hem de olmayan SSO URL çalışana gönderilir. Ancak, çalışan parolası yoksa, yalnızca SSO URL çalışana gönderilir.
   
11. Yaptığınız değişiklikleri kaydedin.

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

Bu bölümde, müşteri için SAP Cloud erişim vererek, Azure çoklu oturum açma kullanılacak Britta Simon etkinleştirin.

1. Azure portalında **kurumsal uygulamalar**seçin **tüm uygulamaları**, ardından **müşteri için SAP Cloud**.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde yazın ve **müşteri için SAP Cloud**.

    ![Uygulamalar listesindeki bağlantılardan müşteri için SAP Cloud](common/all-applications.png)

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

4. Tıklayın **Kullanıcı Ekle** düğmesine ve ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** 'a tıklayın kullanıcı listesinde **seçin** ekranın alt kısmındaki düğmesi.

6. SAML onaylaması ardından içinde herhangi bir rolü değer bekleniyor durumunda **rolü Seç** 'a tıklayın listeden bir kullanıcı için uygun rolü Seç iletişim kutusu **seçin** ekranın alt kısmındaki düğmesi.

7. İçinde **atama Ekle** iletişim tıklatın **atama** düğmesi.

### <a name="create-sap-cloud-for-customer-test-user"></a>SAP Cloud için müşteri test kullanıcısı oluşturma

Bu bölümde, Britta Simon müşteri için SAP bulutta adlı bir kullanıcı oluşturun. Çalışmak [müşteri destek ekibi için SAP Cloud](https://www.sap.com/about/agreements.sap-cloud-services-customers.html) müşteri platform için SAP bulut kullanıcıları eklemek için. Kullanıcı oluşturulmalı ve çoklu oturum açma kullanmadan önce etkinleştirildi.

> [!NOTE]
> Nameıd değer SAP Cloud platform müşteri için kullanıcı adı alanıyla eşleşmelidir emin olun.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi 

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde kutucuk müşteri için SAP Cloud tıklattığınızda, otomatik olarak için SAP Cloud SSO'yu ayarlama Müşteri için oturum açmanız. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

