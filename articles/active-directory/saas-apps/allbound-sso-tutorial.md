---
title: 'Öğretici: Allbound SSO ile Azure Active Directory Tümleştirme | Microsoft Docs'
description: Azure Active Directory ve Allbound SSO arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 15011ddf-941f-4da2-b993-40ad94a08e42
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/19/2018
ms.author: jeedes
ms.openlocfilehash: e0658841e121dc8ea5bd4c704d99fd6bac98a32f
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2019
ms.locfileid: "55171397"
---
# <a name="tutorial-azure-active-directory-integration-with-allbound-sso"></a>Öğretici: Allbound SSO ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile Allbound SSO tümleştirme konusunda bilgi edinin.

Allbound SSO, Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Allbound SSO erişimi, Azure AD'de kontrol edebilirsiniz.
- Azure AD hesaplarına otomatik olarak imzalanan Allbound SSO (çoklu oturum açma) açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md)

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirmesi Allbound SSO ile yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliği
- Abonelik bir Allbound SSO çoklu oturum açma etkin

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, şunları yapabilirsiniz [bir aylık deneme sürümü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden Allbound SSO ekleme
2. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-allbound-sso-from-the-gallery"></a>Galeriden Allbound SSO ekleme

Azure AD'de Allbound SSO tümleştirmesini yapılandırmak için Allbound SSO Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden Allbound SSO eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **Allbound SSO**seçin **Allbound SSO** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Sonuç listesinde Allbound SSO](./media/allbound-sso-tutorial/tutorial_allbound-sso_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırmanız ve Allbound SSO ile Azure AD çoklu oturum açmayı test "Britta Simon" adlı bir test kullanıcı tabanlı.

Tek iş için oturum açma için Azure AD ne Allbound SSO karşılık gelen kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısının Allbound SSO ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma Allbound SSO ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Bir Allbound SSO test kullanıcısı oluşturma](#creating-an-allbound-sso-test-user)**  - kullanıcı Azure AD gösterimini bağlı Allbound SSO Britta simon'un bir karşılığı vardır.
4. **[Azure AD test kullanıcı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açma testi](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve Allbound SSO uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma Allbound SSO ile yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **Allbound SSO** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** iletişim kutusunu tıklatın **seçin** için **SAML** modu, çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açmayı yapılandırın](common/tutorial_general_301.png)

3. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için simgeyi **temel SAML yapılandırma** iletişim.

    ![Çoklu oturum açmayı yapılandırın](common/editconfigure.png)

4. Üzerinde **temel SAML yapılandırma** bölümünde, uygulamada yapılandırmak istiyorsanız aşağıdaki adımları gerçekleştirmek **IDP** başlatılan modu:

    ![Allbound SSO etki alanı ve URL'ler tek oturum açma bilgileri](./media/allbound-sso-tutorial/tutorial_allbound-sso_url.png)

    a. İçinde **tanımlayıcı** metin kutusuna bir URL şu biçimi kullanarak: `https://<SUBDOMAIN>.allbound.com/`

    b. İçinde **yanıt URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://<SUBDOMAIN>.allbound.com/acs`

5. Tıklayın **ek URL'lerini ayarlayın** ve uygulamada yapılandırmak istiyorsanız, aşağıdaki adımı uygulayın **SP** başlatılan modu:

    ![Allbound SSO etki alanı ve URL'ler tek oturum açma bilgileri](./media/allbound-sso-tutorial/tutorial_allbound-sso_url1.png)

    İçinde **oturum açma URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://<SUBDOMAIN>.allbound.com/`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerler gerçek tanımlayıcısı, yanıt URL'si ve oturum açma URL'si ile güncelleştirin. İlgili kişi [Allbound SSO istemci Destek ekibine](mailto:engineering@allbound.com) bu değerleri almak için.

6. Üzerinde **SAML imzalama sertifikası** sayfasında **SAML imzalama sertifikası** bölümünde **indirme** indirmek için **Federasyon meta verileri XML** ve bilgisayarınızda meta veri dosyasını kaydedin.

    ![Sertifika indirme bağlantısı](./media/allbound-sso-tutorial/tutorial_allbound-sso_certificate.png) 

7. Üzerinde **Allbound SSO'yu ayarlama** bölümünde, ihtiyacınıza göre uygun URL'yi kopyalayın.

    a. Oturum Açma URL'si:

    b. Azure AD Tanımlayıcısı

    c. Oturum Kapatma URL'si

    ![Allbound SSO yapılandırma](common/configuresection.png)

8. Çoklu oturum açmayı yapılandırma **Allbound SSO** tarafı, indirilen göndermek için ihtiyacınız **Federasyon meta verileri XML** için [Allbound SSO Destek ekibine](mailto:engineering@allbound.com). Bunlar, her iki kenarı da düzgün ayarlandığından SAML SSO bağlantı sağlamak için bu ayarı ayarlayın.

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

1. Azure portalında, sol bölmede seçin **Azure Active Directory**seçin **kullanıcılar**ve ardından **tüm kullanıcılar**.

    ![Azure AD kullanıcısı oluşturun][100]

2. Seçin **yeni kullanıcı** ekranın üstünde.

    ![Bir Azure AD test kullanıcısı oluşturma](common/create_aaduser_01.png) 

3. Kullanıcı özellikleri, aşağıdaki adımları gerçekleştirin.

    ![Bir Azure AD test kullanıcısı oluşturma](common/create_aaduser_02.png)

    a. İçinde **adı** alanına **BrittaSimon**.
  
    b. İçinde **kullanıcı adı** alanına **brittasimon@yourcompanydomain.extension**  
    Örneğin, BrittaSimon@contoso.com

    c. Seçin **özellikleri**seçin **Show parola** onay kutusunu işaretleyin ve ardından parola kutusunda görüntülenen değeri yazın.

    d. **Oluştur**’u seçin.

### <a name="creating-an-allbound-sso-test-user"></a>Bir Allbound SSO test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon Allbound SSO'ya adlı bir kullanıcı oluşturmaktır. Allbound SSO tam zamanında sağlama, varsayılan olarak etkin olan destekler. Bu bölümde, hiçbir eylem öğesini yoktur. Yeni bir kullanıcı Allbound SSO henüz mevcut değilse erişme denemesi sırasında oluşturulur.
>[!Note]
>Bir kullanıcı el ile oluşturmanız gerekiyorsa, kişi [Allbound SSO Destek ekibine](mailto:engineering@allbound.com).

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcı atama

Bu bölümde, Azure çoklu oturum açma kullanmak için Allbound SSO erişimi vererek Britta Simon etkinleştirin.

1. Azure portalında **kurumsal uygulamalar**seçin **tüm uygulamaları**.

    ![Kullanıcı Ata][201]

2. Uygulamalar listesinde **Allbound SSO**.

    ![Çoklu oturum açmayı yapılandırın](./media/allbound-sso-tutorial/tutorial_allbound-sso_app.png) 

3. Soldaki menüde **kullanıcılar ve gruplar**.

    ![Kullanıcı Ata][202]

4. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Kullanıcı Ata][203]

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** 'a tıklayın kullanıcı listesinde **seçin** ekranın alt kısmındaki düğmesi.

6. İçinde **atama Ekle** iletişim kutusunda **atama** düğmesi.

### <a name="testing-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde Allbound SSO kutucuğa tıkladığınızda, otomatik olarak Allbound SSO uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: common/tutorial_general_01.png
[2]: common/tutorial_general_02.png
[3]: common/tutorial_general_03.png
[4]: common/tutorial_general_04.png

[100]: common/tutorial_general_100.png

[201]: common/tutorial_general_201.png
[202]: common/tutorial_general_202.png
[203]: common/tutorial_general_203.png
