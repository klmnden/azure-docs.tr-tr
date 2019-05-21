---
title: 'Öğretici: Cisco Webex toplantıları ile Azure Active Directory Tümleştirme | Microsoft Docs'
description: Azure Active Directory ve Cisco Webex toplantılar arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: barbkess
ms.assetid: 26704ca7-13ed-4261-bf24-fd6252e2072b
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/01/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 308f745489fba2e2b539a2f2615b65228565dcf9
ms.sourcegitcommit: 67625c53d466c7b04993e995a0d5f87acf7da121
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2019
ms.locfileid: "65900118"
---
# <a name="tutorial-azure-active-directory-integration-with-cisco-webex-meetings"></a>Öğretici: Cisco Webex toplantıları ile Azure Active Directory Tümleştirme

Bu öğreticide, Cisco Webex toplantıları Azure Active Directory (Azure AD) ile tümleştirmeyi öğrenin.
Cisco Webex toplantıları Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

* Cisco Webex toplantıları erişimi, Azure AD'de kontrol edebilirsiniz.
* Azure AD hesaplarına otomatik olarak (çoklu oturum açma) Cisco Webex toplantıları oturum açmış, kullanıcıların etkinleştirebilirsiniz.
* Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile Cisco Webex toplantı yapılandırmak için aşağıdaki öğeler gerekir:

* Azure AD aboneliğiniz. Bir Azure AD ortamını yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/)
* Cisco Webex toplantıları tek oturum açma etkin abonelik

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, yapılandırma ve Azure AD çoklu oturum açma bir test ortamında test edin.

* Cisco Webex toplantıları destekler **SP** tarafından başlatılan

* Cisco Webex toplantıları destekler **zamanında** kullanıcı sağlama

## <a name="adding-cisco-webex-meetings-from-the-gallery"></a>Cisco Webex toplantıları galeri ekleme

Azure AD'de Cisco Webex toplantıları tümleştirmesini yapılandırmak için Cisco Webex toplantıları Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Cisco Webex toplantıları Galeriden eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. Gidin **kurumsal uygulamalar** seçip **tüm uygulamaları** seçeneği.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **Cisco Webex toplantıları**seçin **Cisco Webex toplantıları** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

     ![Sonuç listesinde Cisco Webex toplantıları](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma Cisco Webex adlı bir test kullanıcı göre toplantı test **Britta Simon**.
Tek iş için oturum açma için bir Azure AD kullanıcısı ile ilgili kullanıcı Cisco Webex toplantılarda arasında bir bağlantı ilişki kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma Cisco Webex toplantıları ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Cisco Webex toplantıları çoklu oturum açmayı yapılandırma](#configure-cisco-webex-meetings-single-sign-on)**  - uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
3. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Cisco Webex toplantıları test kullanıcısı oluşturma](#create-cisco-webex-meetings-test-user)**  - Cisco Webex toplantılarda, kullanıcının Azure AD gösterimini bağlı Britta simon'un bir karşılığı vardır.
6. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin.

Azure AD çoklu oturum açma ile Cisco Webex toplantı yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. İçinde [Azure portalında](https://portal.azure.com/), **Cisco Webex toplantıları** uygulama tümleştirme sayfasında **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma](common/select-sso.png)

2. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** iletişim kutusunda, **SAML/WS-Federasyon** modu, çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma seçim modu](common/select-saml-option.png)

3. Azure portalında, üzerinde **yukarı çoklu oturum açma SAML ile ayarlayın** sayfasında **Düzenle** açmak için simgeyi **temel SAML yapılandırma** iletişim.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. Üzerinde **temel SAML yapılandırma** bölümünde, karşıya yüklenen **hizmet sağlayıcısı meta verileri** dosyasını açıp aşağıdaki adımları uygulayarak uygulamayı yapılandırın:

    >[!Note]
    >Daha sonra açıklanan hizmet sağlayıcısı meta verileri dosyası alırsınız **yapılandırma Cisco Webex toplantıları çoklu oturum açma** öğreticinin bölümünde. 

    a. Tıklayın **meta veri dosyasını karşıya yükleme**.

    ![Meta veri dosyasını yükleyin](common/upload-metadata.png)

    b. Tıklayarak **klasör logosu** meta veri dosyası seçin ve **karşıya**.

    ![meta veri dosyası seçin](common/browse-upload-metadata.png)

    c. Hizmet sağlayıcısı meta veri dosyasını karşıya yükleme işlemin başarıyla tamamlanmasından sonra **tanımlayıcı** ve **yanıt URL'si** değerlerini alma otomatik olarak doldurulmuş **temel SAML yapılandırma** Bölüm:

    ![Cisco Webex toplantıları etki alanı ve URL'ler tek oturum açma bilgileri](common/sp-identifier-reply.png)

    İçinde **oturum açma URL'si** metin değerini yapıştırın **yanıt URL'si** otomatik olarak doldurulan SP meta veri dosyası karşıya yüklenmesiyle alır.

5. Cisco Webex toplantıları uygulama, özel öznitelik eşlemelerini SAML belirteci öznitelikleri yapılandırmanıza ekleyin gerektiren belirli bir biçimde SAML onaylamalarını bekler. Aşağıdaki ekran görüntüsünde, varsayılan öznitelikler listesinde gösterilmiştir. Tıklayarak **Düzenle** öznitelikleri eklemek için simge.

    ![image](common/edit-attribute.png)

6. Varsayılan öznitelikleri Sil **kullanıcı taleplerini** bölümü ve Cisco Webex toplantıları uygulama geçirilecek birkaç daha fazla öznitelik bekliyor SAML yanıtta yedekleme. İçinde **kullanıcı taleplerini** bölümünde **kullanıcı öznitelikleri** iletişim kutusunda gösterildiği gibi SAML belirteci özniteliği eklemek için aşağıdaki adımları gerçekleştirin tablonun altındaki:
    
    | Ad | Kaynak özniteliği|
    | ---------------|  --------- |
    |   firstName    | User.givenName |
    |   Soyadı    | User.surname |
    |   email       | User.Mail |
    |   Kullanıcı Kimliği    | User.Mail |

    a. Tıklayın **Ekle yeni talep** açmak için **yönetmek, kullanıcı talepleri** iletişim.

    ![image](./media/cisco-webex-tutorial/tutorial-cisco-webex-addnewclaim.png)

    ![image](common/new-attribute-details.png)

    b. İçinde **adı** metin kutusuna, bu satır için gösterilen öznitelik adı yazın.

    c. Bırakın **Namespace** boş.

    d. Kaynağı olarak **özniteliği**.

    e. Gelen **kaynak özniteliği** listesinde, ilgili satır için gösterilen öznitelik değeri yazın.

    f. Tıklayın **Tamam**

    g. **Kaydet**’e tıklayın.

7. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **SAML imzalama sertifikası** bölümünde **indirme** indirmek için **Federasyon meta veri XML**  bilgisayarınızdaki belirli seçenekler ihtiyacınıza göre ve kaydedin.

    ![Sertifika indirme bağlantısı](common/metadataxml.png)

8. Üzerinde **Cisco Webex toplantıları ' ayarlamak** bölümünde, ihtiyacınıza göre uygun URL'lerini kopyalayın.

    ![Yapılandırma URL'leri kopyalayın](common/copy-configuration-urls.png)

    a. Oturum Açma URL'si:

    b. Azure AD Tanımlayıcısı

    c. Oturum Kapatma URL'si

### <a name="configure-cisco-webex-meetings-single-sign-on"></a>Cisco Webex toplantıları çoklu oturum açmayı yapılandırın

1. Git [Cisco bulut işbirliği Yönetimi](https://www.webex.com/go/connectadmin) yönetim kimlik bilgilerinizle.

2. Git **güvenlik ayarları** gidin **Federe Web SSO Yapılandırması**.
 
    ![Çoklu oturum açmayı yapılandırın](./media/cisco-webex-tutorial/tutorial-cisco-webex-10.png)

3. Üzerinde **Federe Web SSO Yapılandırması** aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/cisco-webex-tutorial/tutorial-cisco-webex-11.png)

    a. Federation Protokolü metin kutusunda protokolünüzü adını yazın.

    b. Tıklayarak **SAML meta verileri içeri aktarma** Azure portalından indirdiğiniz meta veri dosyası karşıya yüklemek için bağlantı.

    c. Tıklayarak **dışarı** hizmet sağlayıcısı meta verileri dosyasını indirin ve bunu karşıya yükleme düğmesini **temel SAML yapılandırma** bölümü Azure portalı.

    d. İçinde **AuthContextClassRef** metin kutusuna `urn:oasis:names:tc:SAML:2.0:ac:classes:unspecified` ve Azure AD kullanarak mfa'yı etkinleştirmek istiyorsanız, iki değer gibi yazın `urn:oasis:names:tc:SAML:2.0:ac:classes:PasswordProtectedTransport;urn:oasis:names:tc:SAML:2.0:ac:classes:X509`

    e. Seçin **otomatik hesap oluşturma**.

    >[!NOTE]
    >Etkinleştirmek için **just-ın-time** kullanıcı hazırlanması gereken denetlemek **otomatik hesap oluşturma**. Buna ek olarak, SAML belirteci öznitelikleri SAML yanıt olarak geçilmesi gerekir.

    f. **Kaydet**’e tıklayın. 

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

Bu bölümde, Cisco Webex toplantılar için erişim izni verdiğinizde, Azure çoklu oturum açma kullanılacak Britta Simon etkinleştirin.

1. Azure portalında **kurumsal uygulamalar**seçin **tüm uygulamaları**, ardından **Cisco Webex toplantıları**.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde **Cisco Webex toplantıları**.

    ![Uygulamalar listesini Cisco Webex toplantıları bağlantıdaki](common/all-applications.png)

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

4. Tıklayın **Kullanıcı Ekle** düğmesine ve ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** 'a tıklayın kullanıcı listesinde **seçin** ekranın alt kısmındaki düğmesi.

6. SAML onaylaması ardından içinde herhangi bir rolü değer bekleniyor durumunda **rolü Seç** 'a tıklayın listeden bir kullanıcı için uygun rolü Seç iletişim kutusu **seçin** ekranın alt kısmındaki düğmesi.

7. İçinde **atama Ekle** iletişim tıklatın **atama** düğmesi.

### <a name="create-cisco-webex-meetings-test-user"></a>Cisco Webex toplantıları test kullanıcısı oluşturma

Bu bölümün amacı, Cisco Webex toplantılarda Britta Simon adlı bir kullanıcı oluşturmaktır. Cisco Webex toplantıları destekler **just-ın-time** sağlama, olan varsayılan olarak etkin. Bu bölümde, hiçbir eylem öğesini yoktur. Cisco Webex toplantılara bir kullanıcı zaten mevcut değilse, Cisco Webex toplantıları erişmeye çalıştığında yeni bir tane oluşturulur.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Cisco Webex toplantılar kutucuğunda erişim Paneli'nde tıklattığınızda, otomatik olarak Cisco Webex SSO'yu ayarlama toplantılar için oturum açmanız. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

