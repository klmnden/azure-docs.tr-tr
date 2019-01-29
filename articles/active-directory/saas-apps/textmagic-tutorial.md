---
title: 'Öğretici: Azure Active Directory Tümleştirmesi ile TextMagic | Microsoft Docs'
description: Azure Active Directory ve TextMagic arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 3e5b49d2-7096-46bc-a9ce-90e09177ba28
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/06/2018
ms.author: jeedes
ms.openlocfilehash: ed5107d581c880d130901bfb31d34afb9e986635
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2019
ms.locfileid: "55190097"
---
# <a name="tutorial-azure-active-directory-integration-with-textmagic"></a>Öğretici: TextMagic ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile TextMagic tümleştirme konusunda bilgi edinin.

Azure AD ile TextMagic tümleştirme ile aşağıdaki avantajları sağlar:

- TextMagic erişimi, Azure AD'de kontrol edebilirsiniz.
- Otomatik olarak imzalanan için TextMagic (çoklu oturum açma) ile Azure AD hesaplarına açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md)

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile TextMagic yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliği
- Abonelik TextMagic çoklu oturum açma etkin

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, şunları yapabilirsiniz [bir aylık deneme sürümü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden TextMagic ekleme
2. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-textmagic-from-the-gallery"></a>Galeriden TextMagic ekleme

Azure AD'de TextMagic tümleştirmesini yapılandırmak için TextMagic Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden TextMagic eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **TextMagic**seçin **TextMagic** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Sonuç listesinde TextMagic](./media/textmagic-tutorial/tutorial_textmagic_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırın ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı TextMagic sınayın.

Tek iş için oturum açma için Azure AD ne TextMagic karşılığı kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısının TextMagic ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma TextMagic ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[TextMagic test kullanıcısı oluşturma](#creating-a-textmagic-test-user)**  - kullanıcı Azure AD gösterimini bağlı TextMagic Britta simon'un bir karşılığı vardır.
4. **[Azure AD test kullanıcı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açma testi](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve TextMagic uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma ile TextMagic yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **TextMagic** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** iletişim kutusunu tıklatın **seçin** için **SAML** modu, çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açmayı yapılandırın](common/tutorial_general_301.png)

3. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için simgeyi **temel SAML yapılandırma** iletişim.

    ![Çoklu oturum açmayı yapılandırın](common/editconfigure.png)

4. Üzerinde **temel SAML yapılandırma** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![TextMagic etki alanı ve URL'ler tek oturum açma bilgileri](./media/textmagic-tutorial/tutorial_textmagic_url.png)

    İçinde **tanımlayıcı** metin kutusuna bir URL: `https://my.textmagic.com/saml/metadata`

5. TextMagic uygulama belirli bir biçimde SAML onaylamalarını bekler. Bu uygulama için aşağıdaki talepleri yapılandırın. Bu öznitelikleri değerlerini yönetebilirsiniz **kullanıcı öznitelikleri ve talepler** uygulama tümleştirme sayfasında bölümü. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için düğmeyi **kullanıcı öznitelikleri ve talepler** iletişim.

    ![image](./media/textmagic-tutorial/i4-attribute.png)

6. İçinde **kullanıcı taleplerini** bölümünde **kullanıcı öznitelikleri ve talepler** iletişim kutusunda, SAML belirteci özniteliği yukarıdaki görüntüde gösterilen şekilde yapılandırın ve aşağıdaki adımları gerçekleştirin:

    | Name  | Kaynak özniteliği  | Ad alanı |
    | --------------- | --------------- | --------------- |
    | Şirket | User.CompanyName | http://schemas.xmlsoap.org/ws/2005/05/identity/claims |
    | FirstName               | User.givenName |  http://schemas.xmlsoap.org/ws/2005/05/identity/claims |
    | Soyadı            | User.surname |  http://schemas.xmlsoap.org/ws/2005/05/identity/claims |
    | telefon               | User.telephoneNumber |  http://schemas.xmlsoap.org/ws/2005/05/identity/claims |
    
    a. Tıklayarak **düzenleme simgesi** düzenlenecek **ad tanımlayıcı değeri** gelen **user.userprinicipalname** için **user.mail**.

    ![TextMagic özniteliği](./media/textmagic-tutorial/tutorial_textmagic_email.png)

    b. Tıklayarak **Ekle yeni talep** açmak için **yönetmek, kullanıcı talepleri** iletişim.

    ![image](./common/new_save_attribute.png)

    ![image](./common/new_attribute_details.png)

    c. İçinde **adı**metin kutusuna, bu satır için gösterilen öznitelik adı yazın.

    d. Girin **Namespace** değeri.

    e. Kaynağı olarak **özniteliği**.

    f. Gelen **kaynak özniteliği** listesinde, ilgili satır için gösterilen öznitelik değeri yazın.

    g. **Tamam**’a tıklayın.

    h. **Kaydet**’e tıklayın. 

7. Üzerinde **SAML imzalama sertifikası** sayfasında **SAML imzalama sertifikası** bölümünde **indirme** indirmek için **sertifika (Base64)** ve bilgisayarınızdaki sertifika dosyasını kaydedin.

    ![Sertifika indirme bağlantısı](./media/textmagic-tutorial/tutorial_textmagic_certificate.png) 

8. Üzerinde **TextMagic kümesi** bölümünde, ihtiyacınıza göre uygun URL'yi kopyalayın.

    a. Oturum Açma URL'si:

    b. Azure AD Tanımlayıcısı

    c. Oturum Kapatma URL'si

    ![TextMagic yapılandırma](common/configuresection.png)

9. Farklı bir web tarayıcı penceresinde TextMagic şirketinizin sitesi için bir yönetici olarak oturum açın.

10. Seçin **hesap ayarları** kullanıcı adı altında.

    ![TextMagic yapılandırma](./media/textmagic-tutorial/config1.png)

11. Tıklayın SEKMESİNDE **çoklu oturum açma (SSO)** ve aşağıdaki alanları doldurun:  
    
    ![TextMagic yapılandırma](./media/textmagic-tutorial/config2.png)

    a. İçinde **kimlik sağlayıcısı varlık Tanıtıcısı:** metin değerini yapıştırın **Azure AD tanımlayıcısı**, hangi Azure Portalı'ndan kopyaladığınız.

    b. İçinde **kimlik sağlayıcısı SSO URL:** metin değerini yapıştırın **oturum açma URL'si**, hangi Azure Portalı'ndan kopyaladığınız.

    c. İçinde **kimlik sağlayıcısı SLO URL:** metin değerini yapıştırın **oturum kapatma URL'si**, hangi Azure Portalı'ndan kopyaladığınız.

    d. Açık, **base-64 kodlamalı sertifika** Azure portalından indirdiğiniz Not Defteri'nde, içeriğini, panoya kopyalayın ve yapıştırın kendisine **x509 ortak sertifika:** metin.

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
  
### <a name="creating-a-textmagic-test-user"></a>TextMagic test kullanıcısı oluşturma

Uygulamanızın desteklediği **zaman kullanıcı hazırlama, sadece** ve sonra kimlik doğrulama kullanıcıları uygulama içinde otomatik olarak oluşturulur. Sisteme alt hesabını etkinleştirmek için bir kez ilk oturum açma bilgileri doldurmanız gerekir.
Bu bölümde, hiçbir eylem öğesini yoktur.

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcı atama

Bu bölümde, Azure çoklu oturum açma kullanmak için TextMagic erişim vererek Britta Simon etkinleştirin.

1. Azure portalında **kurumsal uygulamalar**seçin **tüm uygulamaları**.

    ![Kullanıcı Ata][201]

2. Uygulamalar listesinde **TextMagic**.

    ![Çoklu oturum açmayı yapılandırın](./media/textmagic-tutorial/tutorial_textmagic_app.png) 

3. Soldaki menüde **kullanıcılar ve gruplar**.

    ![Kullanıcı Ata][202]

4. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Kullanıcı Ata][203]

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** 'a tıklayın kullanıcı listesinde **seçin** ekranın alt kısmındaki düğmesi.

6. İçinde **atama Ekle** iletişim kutusunda **atama** düğmesi.

### <a name="testing-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde TextMagic kutucuğa tıkladığınızda, otomatik olarak TextMagic uygulamanıza açan.
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
