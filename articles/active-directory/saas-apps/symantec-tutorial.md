---
title: 'Öğretici: Azure Active Directory Tümleştirmesi Symantec Web güvenlik hizmetini (WSS) | Microsoft Docs'
description: Azure Active Directory ve Symantec Web güvenlik hizmetini (WSS) arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: barbkess
ms.assetid: d6e4d893-1f14-4522-ac20-0c73b18c72a5
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 12/25/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 95ce68547ca13d2395fcd447990c42c48c04eb5f
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67089382"
---
# <a name="tutorial-azure-active-directory-integration-with-symantec-web-security-service-wss"></a>Öğretici: Azure Active Directory Tümleştirmesi Symantec Web güvenlik hizmetini (WSS)

Bu öğreticide, böylece WSS SAML kimlik doğrulaması kullanarak Azure AD'de sağlanan bir son kullanıcı kimlik doğrulaması ve kullanıcı Symantec Web güvenlik hizmetini (WSS) hesabınızın Azure Active Directory (Azure AD) hesabınızla tümleştirmek hakkında bilgi edineceksiniz veya grubu düzeyi ilkesi kuralları.

Symantec Web güvenlik hizmetini (WSS) Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Tüm son kullanıcılar ve grupları Azure AD portalından WSS hesabınız tarafından kullanılan yönetin.

- Son kullanıcıların, Azure AD kimlik bilgilerini kullanarak WSS içindeki doğrulatmak izin verin.

- Kullanıcının zorlamayı etkinleştir ve WSS hesabınızdaki tanımlanan düzeyi ilke kuralları gruplayın.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

Symantec Web güvenlik hizmetini (WSS) ile Azure AD tümleştirmesini yapılandırmak için aşağıdaki öğeler gerekir:

* Azure AD aboneliğiniz. Bir Azure AD ortamını yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/)
* Symantec Web güvenlik hizmetini (WSS) tek oturum açma etkin abonelik

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, yapılandırma ve Azure AD çoklu oturum açma bir test ortamında test edin.

* Symantec Web güvenlik hizmetini (WSS) destekleyen **IDP** tarafından başlatılan

## <a name="adding-symantec-web-security-service-wss-from-the-gallery"></a>Galeriden Symantec Web güvenlik hizmetini (WSS) ekleme

Azure AD'de Symantec Web güvenlik hizmetini (WSS) tümleştirmesini yapılandırmak için Symantec Web güvenlik hizmetini (WSS) Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden Symantec Web güvenlik hizmetini (WSS) eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)** , sol gezinti panelinde tıklayın **Azure Active Directory** simgesi.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. Gidin **kurumsal uygulamalar** seçip **tüm uygulamaları** seçeneği.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **Symantec Web güvenlik hizmetini (WSS)** seçin **Symantec Web güvenlik hizmetini (WSS)** sonucu panelinden ardından **Ekle** düğme eklemek için uygulama.

     ![Sonuç listesinde Symantec Web güvenlik hizmetini (WSS)](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma ile Symantec Web güvenlik hizmeti (adlı bir test kullanıcı tabanlı WSS) test etme **Britta Simon**.
Tek iş için oturum açma için bir Azure AD kullanıcısı ile ilgili kullanıcı Symantec Web güvenlik hizmetini (WSS) arasında bir bağlantı ilişki kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma Symantec Web güvenlik hizmetini (WSS) test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **Symantec Web güvenlik hizmetini (WSS) çoklu oturum açmayı yapılandırma** - uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
3. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Symantec Web güvenlik hizmetini (WSS) test kullanıcısı oluşturma](#create-symantec-web-security-service-wss-test-user)**  - Symantec Web güvenlik hizmetini (WSS), kullanıcı Azure AD gösterimini bağlı Britta simon'un bir karşılığı vardır.
6. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin.

Azure AD çoklu oturum açma Symantec Web güvenlik hizmetini (WSS) yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. İçinde [Azure portalında](https://portal.azure.com/), **Symantec Web güvenlik hizmetini (WSS)** uygulama tümleştirme sayfasında **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma](common/select-sso.png)

2. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** iletişim kutusunda, **SAML/WS-Federasyon** modu, çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma seçim modu](common/select-saml-option.png)

3. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için simgeyi **temel SAML yapılandırma** iletişim.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. Üzerinde **temel SAML yapılandırma** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Symantec Web güvenlik hizmetini (WSS) etki alanı ve URL'ler tek oturum açma bilgileri](common/idp-intiated.png)

    a. İçinde **tanımlayıcı** metin kutusuna bir URL yazın: `https://saml.threatpulse.net:8443/saml/saml_realm`

    b. İçinde **yanıt URL'si** metin kutusuna bir URL yazın: `https://saml.threatpulse.net:8443/saml/saml_realm/bcsamlpost`

    > [!NOTE]
    > İlgili kişi [Symantec Web güvenlik hizmetini (WSS) istemci Destek ekibine](https://www.symantec.com/contact-us) f değerlerini **tanımlayıcı** ve **yanıt URL'si** herhangi bir nedenle çalışmıyor... Gösterilen desenleri de başvurabilirsiniz **temel SAML yapılandırma** bölümünde Azure portalında.

5. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **SAML imzalama sertifikası** bölümünde **indirme** indirmek için **Federasyon meta veri XML**  bilgisayarınızdaki belirli seçenekler ihtiyacınıza göre ve kaydedin.

    ![Sertifika indirme bağlantısı](common/metadataxml.png)

### <a name="configure-symantec-web-security-service-wss-single-sign-on"></a>Symantec Web güvenlik hizmetini (WSS) çoklu oturum açmayı yapılandırın

Çoklu oturum açma Symantec Web güvenlik hizmetini (WSS) tarafında yapılandırma WSS çevrimiçi belgelerine bakın. İndirilen **Federasyon meta verileri XML** WSS Portalı'na aktarılması gerekir. İlgili kişi [Symantec Web güvenlik hizmetini (WSS) Destek ekibine](https://www.symantec.com/contact-us) WSS portalındaki yapılandırmayla ilgili yardıma ihtiyacınız varsa.

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

Bu bölümde, Symantec Web güvenlik hizmetini (WSS) erişim izni verdiğinizde, Azure çoklu oturum açma kullanılacak Britta Simon etkinleştirin.

1. Azure portalında **kurumsal uygulamalar**seçin **tüm uygulamaları**, ardından **Symantec Web güvenlik hizmetini (WSS)** .

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde yazın ve **Symantec Web güvenlik hizmetini (WSS)** .

    ![Uygulamalar listesinde Symantec Web güvenlik hizmetini (WSS) bağlantısı](common/all-applications.png)

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

4. Tıklayın **Kullanıcı Ekle** düğmesine ve ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** 'a tıklayın kullanıcı listesinde **seçin** ekranın alt kısmındaki düğmesi.

6. SAML onaylaması ardından içinde herhangi bir rolü değer bekleniyor durumunda **rolü Seç** 'a tıklayın listeden bir kullanıcı için uygun rolü Seç iletişim kutusu **seçin** ekranın alt kısmındaki düğmesi.

7. İçinde **atama Ekle** iletişim tıklatın **atama** düğmesi.

### <a name="create-symantec-web-security-service-wss-test-user"></a>Symantec Web güvenlik hizmetini (WSS) test kullanıcısı oluşturma

Bu bölümde, Britta Simon Symantec Web güvenlik hizmetini (WSS) adlı bir kullanıcı oluşturun. Karşılık gelen uç kullanıcı adı WSS Portalı'nda el ile oluşturulabilir veya birkaç dakika sonra (yaklaşık 15 dakika) WSS portalına eşitlenecek Azure AD'de sağlanan kullanıcılar/gruplar bekleyebilirsiniz. Kullanıcı oluşturulmalı ve çoklu oturum açma kullanmadan önce etkinleştirildi. Web siteleri göz atmak için kullanılan son kullanıcı makinenin genel IP adresini de Symantec Web güvenlik hizmetini (WSS) portalında sağlanması gerekir.

> [!NOTE]
> Lütfen tıklayın [burada](https://www.bing.com/search?q=my+ip+address&qs=AS&pq=my+ip+a&sc=8-7&cvid=29A720C95C78488CA3F9A6BA0B3F98C5&FORM=QBLH&sp=1) makinenizin genel IP adresi günceller.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi 

WSS hesabınızı Azure AD'nize SAML kimlik doğrulaması için kullanmak üzere yapılandırdığınıza göre bu bölümde, çoklu oturum açma işlevlerini sınayacaksınız.

Ardından, web tarayıcınızı açın ve bir siteye göz atmayı deneyin WSS, proxy trafiğini web tarayıcınızda yapılandırdıktan sonra Azure oturum açma sayfasına yönlendirilirsiniz. Azure AD'de (diğer bir deyişle, BrittaSimon) sağlanan bir test son kullanıcı kimlik bilgilerini girin ve ilişkili parola. Kimlik doğrulandıktan sonra seçtiğiniz bir Web sitesine gözatmak mümkün olacaktır. WSS tarafında BrittaSimon kullanıcı olarak o siteye göz atmak denediğinizde WSS blok sayfası görmeniz gerekir sonra belirli bir siteye göz atma BrittaSimon engellemek için bir ilke kuralı oluşturmanız gerekir.

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

