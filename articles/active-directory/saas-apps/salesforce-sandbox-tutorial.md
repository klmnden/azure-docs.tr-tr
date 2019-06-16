---
title: 'Öğretici: Salesforce korumalı alanı ile Azure Active Directory Tümleştirmesi | Microsoft Docs'
description: Azure Active Directory ve Salesforce korumalı alan arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: ee54c39e-ce20-42a4-8531-da7b5f40f57c
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 04/17/2019
ms.author: jeedes
ms.openlocfilehash: 1e303485a03edcd9ba3d3e7380aa4c7ae8b1a4b0
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67092609"
---
# <a name="tutorial-azure-active-directory-integration-with-salesforce-sandbox"></a>Öğretici: Salesforce korumalı alanı ile Azure Active Directory Tümleştirmesi

Bu öğreticide, Azure Active Directory (Azure AD) ile Salesforce korumalı alan tümleştirme konusunda bilgi edinin.

Sanal amaçları, geliştirme, test ve eğitim, verilere ve uygulamalara, Salesforce üretimde ödün vermeden gibi çeşitli ayrı ortamlarda kuruluşunuz birden çok kopyasını oluşturma olanağı sunar Kuruluş.
Daha fazla ayrıntı için [korumalı alan genel bakış](https://help.salesforce.com/articleView?id=create_test_instance.htm&language=en_us&type=5).

Salesforce korumalı alan Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

* Salesforce korumalı alan erişimi, Azure AD'de kontrol edebilirsiniz.
* Azure AD hesaplarına otomatik olarak (çoklu oturum açma) Salesforce korumalı alan için oturum açmış, kullanıcıların etkinleştirebilirsiniz.
* Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile Salesforce korumalı alan yapılandırmak için aşağıdaki öğeler gerekir:

* Azure AD aboneliğiniz. Bir Azure AD ortamını yoksa alabileceğiniz bir [ücretsiz hesap](https://azure.microsoft.com/free/)
* Salesforce korumalı alan çoklu oturum açma abonelik etkin.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, yapılandırma ve Azure AD çoklu oturum açma bir test ortamında test edin.

* Salesforce korumalı alan destekler **SP ve IDP** tarafından başlatılan
* Salesforce korumalı alan destekler **zamanında** kullanıcı sağlama
* Salesforce korumalı alan destekler [ **otomatik** kullanıcı sağlama](salesforce-sandbox-provisioning-tutorial.md)

## <a name="adding-salesforce-sandbox-from-the-gallery"></a>Galeriden Salesforce korumalı alan ekleme

Azure AD'de Salesforce korumalı alan tümleştirmesini yapılandırmak için Salesforce korumalı alan Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden Salesforce korumalı alan eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)** , sol gezinti panelinde tıklayın **Azure Active Directory** simgesi.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. Gidin **kurumsal uygulamalar** seçip **tüm uygulamaları** seçeneği.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **Salesforce korumalı alan**seçin **Salesforce korumalı alan** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Sonuç listesinde Salesforce korumalı alan](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırmanız ve Salesforce korumalı alanı ile Azure AD çoklu oturum açmayı test adlı bir test kullanıcı tabanlı **Britta Simon**.

Tek çalışmak için oturum açma için Azure AD ne karşılık gelen kullanıcı Salesforce korumalı alan içinde bir kullanıcının Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısı ve Salesforce korumalı alan ilgili kullanıcı arasında bir bağlantı ilişki kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma Salesforce korumalı alanı ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Salesforce korumalı alan çoklu oturum açmayı yapılandırma](#configure-salesforce-sandbox-single-sign-on)**  - uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
3. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Salesforce korumalı alan test kullanıcısı oluşturma](#create-salesforce-sandbox-test-user)**  - kullanıcı Azure AD gösterimini bağlı Salesforce korumalı Britta simon'un bir karşılığı vardır.
6. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin.

Azure AD çoklu oturum açma ile Salesforce korumalı alan yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. İçinde [Azure portalında](https://portal.azure.com/), **Salesforce korumalı alan** uygulama tümleştirme sayfasında **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma](common/select-sso.png)

2. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** iletişim kutusunda, **SAML/WS-Federasyon** modu, çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma seçim modu](common/select-saml-option.png)

3. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için simgeyi **temel SAML yapılandırma** iletişim.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. Üzerinde **temel SAML yapılandırma** varsa, bölüm **hizmet sağlayıcısı meta veri dosyası** yapılandırmak isterseniz **IDP** başlatılan modu, aşağıdaki adımları gerçekleştirin:

    a. Tıklayın **meta veri dosyasını karşıya yükleme**.

    ![Meta veri dosyasını karşıya yükleyin](common/upload-metadata.png)

    b. Tıklayarak **klasör logosu** meta veri dosyası seçin ve **karşıya**.

    ![meta veri dosyası seçin](common/browse-upload-metadata.png)

    > [!NOTE]
    > Hizmet sağlayıcısı, öğreticinin ilerleyen bölümlerinde açıklanan Salesforce korumalı alan Yönetim Portalı'ndan meta veri dosyası alırsınız.

    c. Meta veri dosyası başarıyla karşıya yüklendikten sonra **yanıt URL'si** değer otomatik olarak doldurulmuş elde **yanıt URL'si** metin.

    ![image](common/both-replyurl.png)

    > [!Note]
    > Varsa **yanıt URL'si** değer otomatik polulated elde değil, değerin ihtiyacınıza göre el ile doldurun.

5. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **SAML imzalama sertifikası** bölümünde **indirme** indirmek için **meta veri XML**bilgisayarınızdaki belirli seçenekler ihtiyacınıza göre ve kaydedin.

    ![Sertifika indirme bağlantısı](common/metadataxml.png)

6. Üzerinde **Salesforce korumalı alan kümesi** bölümünde, ihtiyacınıza göre uygun URL'lerini kopyalayın.

    ![Yapılandırma URL'leri kopyalayın](common/copy-configuration-urls.png)

    a. Oturum Açma URL'si:

    b. Azure AD Tanımlayıcısı

    c. Oturum Kapatma URL'si

### <a name="configure-salesforce-sandbox-single-sign-on"></a>Salesforce korumalı alan çoklu oturum açmayı yapılandırın

1. Tarayıcınızda yeni bir sekme açın ve Salesforce korumalı alan yönetici hesabınızda oturum açın.

2. Tıklayarak **Kurulum** altında **ayarlar simgesine** sayfanın sağ üst köşesinde.

    ![Çoklu oturum açmayı yapılandırın](./media/salesforce-sandbox-tutorial/configure1.png)

3. Ekranı aşağı kaydırarak **ayarları** sol gezinti bölmesinden **kimlik** ilgili bölümü genişletin. Ardından **çoklu oturum açma ayarları**.

    ![Çoklu oturum açmayı yapılandırın](./media/salesforce-sandbox-tutorial/sf-admin-sso.png)

4. Üzerinde **çoklu oturum açma ayarları** sayfasında **Düzenle** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/salesforce-sandbox-tutorial/configure3.png)

5. Seçin **SAML etkin**ve ardından **Kaydet**.

    ![Çoklu oturum açmayı yapılandırın](./media/salesforce-sandbox-tutorial/sf-enable-saml.png)

6. SAML çoklu oturum açma ayarlarınızı yapılandırmak için tıklayın **yeni meta veri dosyasından**.

    ![Çoklu oturum açmayı yapılandırın](./media/salesforce-sandbox-tutorial/sf-admin-sso-new.png)

7. Tıklayın **Dosya Seç** Azure portal'ı seçin ve indirilen meta veri XML dosyasını karşıya yüklemek için **Oluştur**.

    ![Çoklu oturum açmayı yapılandırın](./media/salesforce-sandbox-tutorial/xmlchoose.png)

8. Üzerinde **SAML çoklu oturum açma ayarları** sayfasında alanları otomatik olarak doldurur ve Kaydet'e tıklayın.

    ![Çoklu oturum açmayı yapılandırın](./media/salesforce-sandbox-tutorial/salesforcexml.png)

9. Üzerinde **çoklu oturum açma ayarları** sayfasında, **meta verileri indirme** düğmesi, hizmet sağlayıcısı meta verileri dosyası indirilemedi. Bu dosyada kullanmak **temel SAML yapılandırma** yukarıda açıklandığı gibi gerekli URL'leri yapılandırmak için Azure portalında bölümü.

    ![Çoklu oturum açmayı yapılandırın](./media/salesforce-sandbox-tutorial/configure4.png)

10. Uygulamada yapılandırmak istiyorsanız **SP** başlatılan modu, aşağıdaki ilgili Önkoşullar şunlardır:

    a. Doğrulanmış bir etki alanı olmalıdır.

    b. Salesforce korumalı alan etki alanınızda etkinleştirmeniz ve yapılandırmanız gerekir, bu adımlar, bu öğreticinin ilerleyen bölümlerinde açıklanmıştır.

    c. Azure portalında, üzerinde **temel SAML yapılandırma** bölümünde **ek URL'lerini ayarlayın** ve aşağıdaki adımı uygulayın:
  
    ![Salesforce korumalı alan etki alanı ve URL'ler tek oturum açma bilgileri](common/both-signonurl.png)

    İçinde **oturum açma URL'si** metin kutusuna şu biçimi kullanarak değeri yazın: `https://<instancename>--Sandbox.<entityid>.my.salesforce.com`

    > [!NOTE]
    > Etki alanı etkinleştirdikten sonra bu değer Salesforce korumalı alan portaldan kopyalanmalıdır.

11. Üzerinde **SAML imzalama sertifikası** bölümünde **Federasyon meta verileri XML** ve bilgisayarınızda xml dosyasını kaydedin.

    ![Sertifika indirme bağlantısı](common/metadataxml.png)

12. Tarayıcınızda yeni bir sekme açın ve Salesforce korumalı alan yönetici hesabınızda oturum açın.

13. Tıklayarak **Kurulum** altında **ayarlar simgesine** sayfanın sağ üst köşesinde.

    ![Çoklu oturum açmayı yapılandırın](./media/salesforce-sandbox-tutorial/configure1.png)

14. Ekranı aşağı kaydırarak **ayarları** sol gezinti bölmesinden **kimlik** ilgili bölümü genişletin. Ardından **çoklu oturum açma ayarları**.

    ![Çoklu oturum açmayı yapılandırın](./media/salesforce-sandbox-tutorial/sf-admin-sso.png)

15. Üzerinde **çoklu oturum açma ayarları** sayfasında **Düzenle** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/salesforce-sandbox-tutorial/configure3.png)

16. Seçin **SAML etkin**ve ardından **Kaydet**.

    ![Çoklu oturum açmayı yapılandırın](./media/salesforce-sandbox-tutorial/sf-enable-saml.png)

17. SAML çoklu oturum açma ayarlarınızı yapılandırmak için tıklayın **yeni meta veri dosyasından**.

    ![Çoklu oturum açmayı yapılandırın](./media/salesforce-sandbox-tutorial/sf-admin-sso-new.png)

18. Tıklayın **Dosya Seç** meta veri XML dosyasını karşıya yükleyin ve tıklayın **Oluştur**.

    ![Çoklu oturum açmayı yapılandırın](./media/salesforce-sandbox-tutorial/xmlchoose.png)

19. Üzerinde **SAML çoklu oturum açma ayarları** sayfasında alanları otomatik olarak doldurmak, yapılandırma adını yazın (örneğin: *SPSSOWAAD_Test*), **adı** metin kutusu ve Kaydet'i tıklatın.

    ![Çoklu oturum açmayı yapılandırın](./media/salesforce-sandbox-tutorial/sf-saml-config.png)

20. Salesforce korumalı alan etki alanınızda etkinleştirmek için aşağıdaki adımları gerçekleştirin:

    > [!NOTE]
    > Etki alanı etkinleştirmeden önce aynı Salesforce korumalı alan üzerinde oluşturmanız gerekir. Daha fazla bilgi için [etki alanı adınız tanımlama](https://help.salesforce.com/HTViewHelpDoc?id=domain_name_define.htm&language=en_US). Etki alanı oluşturulduktan sonra Lütfen doğru yapılandırıldığından emin olun.

21. Sol gezinti bölmesinde Salesforce korumalı alan içinde tıklatın **şirket ayarları** ilgili bölümü genişletin ve ardından **My Domain**.

    ![Çoklu oturum açmayı yapılandırın](./media/salesforce-sandbox-tutorial/sf-my-domain.png)

22. İçinde **kimlik doğrulama Yapılandırması** bölümünde **Düzenle**.

    ![Çoklu oturum açmayı yapılandırın](./media/salesforce-sandbox-tutorial/sf-edit-auth-config.png)

23. İçinde **kimlik doğrulama Yapılandırması** bölümünde olarak **kimlik doğrulama hizmeti**, SAML çoklu oturum açma Salesforce korumalı alan SSO yapılandırması sırasında ayarlanan ayar adını seçin ve tıklayın **Kaydet**.

    ![Çoklu oturum açmayı yapılandırın](./media/salesforce-sandbox-tutorial/configure2.png)

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

1. Azure portalında, sol bölmede seçin **Azure Active Directory**seçin **kullanıcılar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](common/users.png)

2. Seçin **yeni kullanıcı** ekranın üstünde.

    ![Yeni kullanıcı düğmesi](common/new-user.png)

3. Kullanıcı özellikleri, aşağıdaki adımları gerçekleştirin.

    ![Kullanıcı iletişim kutusu](common/user-properties.png)

    a. İçinde **adı** alana **BrittaSimon**.
  
    b. İçinde **kullanıcı adı** alan türü `brittasimon@yourcompanydomain.extension`. Örneğin, BrittaSimon@contoso.com.

    c. Seçin **Show parola** onay kutusunu işaretleyin ve ardından parola kutusunda görüntülenen değeri yazın.

    d. **Oluştur**’a tıklayın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Azure çoklu oturum açma kullanmak için Salesforce korumalı alan erişimi vererek Britta Simon etkinleştirin.

1. Azure portalında **kurumsal uygulamalar**seçin **tüm uygulamaları**, ardından **Salesforce korumalı alan**.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde **Salesforce korumalı alan**.

    ![Uygulamalar listesinde Salesforce korumalı alan bağlantı](common/all-applications.png)

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

4. Tıklayın **Kullanıcı Ekle** düğmesine ve ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** 'a tıklayın kullanıcı listesinde **seçin** ekranın alt kısmındaki düğmesi.

6. SAML onaylaması ardından içinde herhangi bir rolü değer bekleniyor durumunda **rolü Seç** 'a tıklayın listeden bir kullanıcı için uygun rolü Seç iletişim kutusu **seçin** ekranın alt kısmındaki düğmesi.

7. İçinde **atama Ekle** iletişim tıklatın **atama** düğmesi.

### <a name="create-salesforce-sandbox-test-user"></a>Salesforce korumalı alan test kullanıcısı oluşturma

Bu bölümde, Britta Simon adlı bir kullanıcı, Salesforce korumalı alanında oluşturulur. Salesforce korumalı alan tam zamanında sağlama, varsayılan olarak etkin olduğu destekler. Bu bölümde, hiçbir eylem öğesini yoktur. Salesforce korumalı alan erişmeye çalıştığında, Salesforce korumalı alan içinde bir kullanıcı zaten mevcut değilse yeni bir tane oluşturulur. Salesforce korumalı alan da otomatik kullanıcı sağlamayı destekler, daha fazla ayrıntı bulabilirsiniz [burada](salesforce-sandbox-provisioning-tutorial.md) otomatik kullanıcı sağlamayı yapılandırma.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli Salesforce korumalı alan kutucuğa tıkladığınızda, size otomatik olarak SSO'yu ayarlama Salesforce korumalı oturum açmanız. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

* [Kullanıcı sağlamayı yapılandırma](salesforce-sandbox-provisioning-tutorial.md)
