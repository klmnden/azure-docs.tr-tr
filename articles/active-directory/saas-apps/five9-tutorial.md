---
title: 'Öğretici: Five9 artı bağdaştırıcısı (CTI, ilgili Center aracıları) ile Azure Active Directory Tümleştirme | Microsoft Docs'
description: Azure Active Directory ve Five9 artı bağdaştırıcısı (CTI, ilgili Center aracıları) arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 88dc82ab-be0b-4017-8335-c47d00775d7b
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 04/04/2019
ms.author: jeedes
ms.openlocfilehash: f42e58ff7bf76af561dd49169c73838499718168
ms.sourcegitcommit: 031e4165a1767c00bb5365ce9b2a189c8b69d4c0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/13/2019
ms.locfileid: "59547293"
---
# <a name="tutorial-azure-active-directory-integration-with-five9-plus-adapter-cti-contact-center-agents"></a>Öğretici: Five9 artı bağdaştırıcısı (CTI, ilgili Center aracıları) ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile nasıl tümleştireceğinizi Five9 artı bağdaştırıcısı (CTI, ilgili Center aracıları) öğrenin.
Five9 artı bağdaştırıcısı (CTI, ilgili Center aracıları) Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

* Five9 artı bağdaştırıcısı (CTI, ilgili Center aracıları) erişimi, Azure AD'de kontrol edebilirsiniz.
* Otomatik olarak Five9 artı bağdaştırıcısına (CTI, ilgili Center aracıları) oturum açmış, kullanıcıların etkinleştirebilirsiniz (çoklu oturum açma) ile Azure AD hesaplarına.
* Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirmesi Five9 artı bağdaştırıcısıyla (CTI, ilgili Center aracıları) yapılandırmak için aşağıdaki öğeler gerekir:

* Azure AD aboneliğiniz. Bir Azure AD ortamını yoksa alabileceğiniz bir [ücretsiz bir hesap](https://azure.microsoft.com/free/).
* Abonelik Five9 artı bağdaştırıcısı (CTI, ilgili Center aracıları) çoklu oturum açma etkin

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, yapılandırma ve Azure AD çoklu oturum açma bir test ortamında test edin.

* Five9 artı bağdaştırıcısını (CTI, ilgili Center aracıları) destekleyen **IDP** tarafından başlatılan

## <a name="adding-five9-plus-adapter-cti-contact-center-agents-from-the-gallery"></a>Galeriden Five9 artı bağdaştırıcısı (CTI, ilgili Center aracıları) ekleme

Azure AD'ye Five9 artı bağdaştırıcısı (CTI, ilgili Center aracıları) tümleştirmesini yapılandırmak için Five9 artı bağdaştırıcısı (CTI, ilgili Center aracıları) eklemek galerideki yönetilen SaaS uygulamaları listenize gerekir.

**Galeriden Five9 artı bağdaştırıcısı (CTI, ilgili Center aracıları) eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. Gidin **kurumsal uygulamalar** seçip **tüm uygulamaları** seçeneği.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **Five9 artı bağdaştırıcısı (CTI, ilgili Center aracıları)** seçin **Five9 artı bağdaştırıcısı (CTI, ilgili Center aracıları)** sonucu panelinden ardından **Ekle** uygulama ekleme düğmesi.

     ![Five9 artı bağdaştırıcısı (CTI, ilgili Center aracıları) sonuç listesinde](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD'ye tek temelinde oturum açma adlı bir test kullanıcı Five9 artı bağdaştırıcısı (CTI, ilgili Center aracıları) ile test etme **Britta Simon**.
Tek iş için oturum açma için bir Azure AD kullanıcısı ve ilgili kullanıcı Five9 artı bağdaştırıcısında (CTI, ilgili Center aracıları) arasında bir bağlantı ilişki kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma Five9 artı bağdaştırıcısı (CTI, ilgili Center aracıları) ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Five9 artı bağdaştırıcısı (CTI, ilgili Center aracıları) çoklu oturum açmayı yapılandırma](#configure-five9-plus-adapter-cti-contact-center-agents-single-sign-on)**  - uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
3. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Five9 artı bağdaştırıcısı (CTI, ilgili Center aracıları) test kullanıcısı oluşturma](#create-five9-plus-adapter-cti-contact-center-agents-test-user)**  - bir karşılığı Britta simon'un Five9 yanı sıra kullanıcı Azure AD gösterimini bağlı bağdaştırıcı (CTI, ilgili Center aracıları) sağlamak için.
6. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin.

Azure AD çoklu oturum açma Five9 artı bağdaştırıcısıyla (CTI, ilgili Center aracıları) yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. İçinde [Azure portalında](https://portal.azure.com/), **Five9 artı bağdaştırıcısı (CTI, ilgili Center aracıları)** uygulama tümleştirme sayfasında **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma](common/select-sso.png)

2. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** iletişim kutusunda, **SAML/WS-Federasyon** modu, çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma seçim modu](common/select-saml-option.png)

3. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için simgeyi **temel SAML yapılandırma** iletişim.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında, aşağıdaki adımları gerçekleştirin:

    ![Five9 artı bağdaştırıcısı (CTI, ilgili Center aracıları) etki alanı ve URL'ler tek oturum açma bilgileri](common/idp-intiated.png)

    a. İçinde **tanımlayıcı** metin kutusuna şu biçimi kullanarak bir URL yazın:
    
    |    Ortam      |       URL'si      |
    | :-- | :-- |
    | "Five9 Plus için Microsoft Dynamics CRM bağdaştırıcısı" | `https://app.five9.com/appsvcs/saml/metadata/alias/msdc` |
    | "Five9 Plus bağdaştırıcısı Zendesk için" | `https://app.five9.com/appsvcs/saml/metadata/alias/zd` |
    | "Five9 Plus bağdaştırıcısı aracı Masaüstü araç seti için" | `https://app.five9.com/appsvcs/saml/metadata/alias/adt` |

    b. İçinde **yanıt URL'si** metin kutusuna şu biçimi kullanarak bir URL yazın:

    |      Ortam     |      URL'si      |
    | :--                  | :--           |
    | "Five9 Plus için Microsoft Dynamics CRM bağdaştırıcısı" | `https://app.five9.com/appsvcs/saml/SSO/alias/msdc` |
    | "Five9 Plus bağdaştırıcısı Zendesk için" | `https://app.five9.com/appsvcs/saml/SSO/alias/zd` |
    | "Five9 Plus bağdaştırıcısı aracı Masaüstü araç seti için" | `https://app.five9.com/appsvcs/saml/SSO/alias/adt` |

6. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **SAML imzalama sertifikası** bölümünde **indirme** indirmek için **sertifika (Base64)** bilgisayarınızdaki belirli seçenekler ihtiyacınıza göre ve kaydedin.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

7. Üzerinde **Five9 artı bağdaştırıcısını kurma (CTI, ilgili Center aracıları)** bölümünde, ihtiyacınıza göre uygun URL'lerini kopyalayın.

    ![Yapılandırma URL'leri kopyalayın](common/copy-configuration-urls.png)

    a. Oturum Açma URL'si:

    b. Azure AD Tanımlayıcısı

    c. Oturum Kapatma URL'si

### <a name="configure-five9-plus-adapter-cti-contact-center-agents-single-sign-on"></a>Five9 artı bağdaştırıcısı (CTI, ilgili Center aracıları) çoklu oturum açmayı yapılandırın

1. Çoklu oturum açmayı yapılandırma **Five9 artı bağdaştırıcısı (CTI, ilgili Center aracıları)** tarafı, indirilen göndermek için ihtiyacınız **Certificate(Base64)** ve uygun şekilde kopyalanan URL'lerini [Five9 artı Bağdaştırıcı (CTI, ilgili Center aracıları) destek ekibi](https://www.five9.com/about/contact). Ayrıca, SSO daha fazla yapılandırmak için lütfen izleyin aşağıdaki adımları bağdaştırıcısı göre:

    a. "Five9 artı bağdaştırıcısı aracı Masaüstü araç seti için" Yönetici Kılavuzu: [https://webapps.five9.com/assets/files/for_customers/documentation/integrations/agent-desktop-toolkit/plus-agent-desktop-toolkit-administrators-guide.pdf](https://webapps.five9.com/assets/files/for_customers/documentation/integrations/agent-desktop-toolkit/plus-agent-desktop-toolkit-administrators-guide.pdf)
    
    b. "Five9 yanı sıra Microsoft Dynamics CRM için bağdaştırıcı" Yönetici Kılavuzu: [https://webapps.five9.com/assets/files/for_customers/documentation/integrations/microsoft/microsoft-administrators-guide.pdf](https://webapps.five9.com/assets/files/for_customers/documentation/integrations/microsoft/microsoft-administrators-guide.pdf)
    
    c. "Five9 artı bağdaştırıcısı Zendesk için" Yönetici Kılavuzu: [https://webapps.five9.com/assets/files/for_customers/documentation/integrations/zendesk/zendesk-plus-administrators-guide.pdf](https://webapps.five9.com/assets/files/for_customers/documentation/integrations/zendesk/zendesk-plus-administrators-guide.pdf)

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

Bu bölümde, Five9 artı bağdaştırıcısına (CTI, ilgili Center aracıları) erişim izni verdiğinizde, Azure çoklu oturum açma kullanılacak Britta Simon etkinleştirin.

1. Azure portalında **kurumsal uygulamalar**seçin **tüm uygulamaları**, ardından **Five9 artı bağdaştırıcısı (CTI, ilgili Center aracıları)**.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde **Five9 artı bağdaştırıcısı (CTI, ilgili Center aracıları)**.

    ![Uygulamalar listesinde Five9 artı bağdaştırıcı (CTI, ilgili Center aracıları) bağlantısı](common/all-applications.png)

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

4. Tıklayın **Kullanıcı Ekle** düğmesine ve ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** 'a tıklayın kullanıcı listesinde **seçin** ekranın alt kısmındaki düğmesi.

6. SAML onaylaması ardından içinde herhangi bir rolü değer bekleniyor durumunda **rolü Seç** 'a tıklayın listeden bir kullanıcı için uygun rolü Seç iletişim kutusu **seçin** ekranın alt kısmındaki düğmesi.

7. İçinde **atama Ekle** iletişim tıklatın **atama** düğmesi.

### <a name="create-five9-plus-adapter-cti-contact-center-agents-test-user"></a>Five9 artı bağdaştırıcısı (CTI, ilgili Center aracıları) test kullanıcısı oluşturma

Bu bölümde, Britta Simon Five9 artı bağdaştırıcısı (CTI, ilgili Center aracıları) adlı bir kullanıcı oluşturun. Çalışmak [Five9 artı bağdaştırıcısı (CTI, ilgili Center aracıları) destek ekibi](https://www.five9.com/about/contact) Five9 artı bağdaştırıcısı (CTI, ilgili Center aracıları) platform kullanıcıları eklemek için. Kullanıcı oluşturulmalı ve çoklu oturum açma kullanmadan önce etkinleştirildi. 

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi 

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Five9 artı bağdaştırıcısı tıkladığınızda, (CTI, ilgili Center aracıları kutucuğuna erişim panelinde, otomatik olarak Five9 artı SSO'yu ayarladığınız bağdaştırıcısına (CTI, ilgili Center aracıları) açmış olmanız. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek Kaynaklar

- [ SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi ](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir? ](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

