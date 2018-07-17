---
title: 'Öğretici: Azure Active Directory tümleştirmesiyle SmartLPA | Microsoft Docs'
description: Azure Active Directory ve SmartLPA arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: ed5f6580-bee6-4f44-aceb-f8f7feda3fb8
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2018
ms.author: jeedes
ms.openlocfilehash: eedec7de74f3bdd7ae43d1b1eb0decd5fe83ebf6
ms.sourcegitcommit: 0b05bdeb22a06c91823bd1933ac65b2e0c2d6553
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/17/2018
ms.locfileid: "39076498"
---
# <a name="tutorial-azure-active-directory-integration-with-smartlpa"></a>Öğretici: Azure Active Directory SmartLPA ile tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile SmartLPA tümleştirme konusunda bilgi edinin.

Azure AD ile SmartLPA tümleştirme ile aşağıdaki avantajları sağlar:

- SmartLPA erişimi, Azure AD'de kontrol edebilirsiniz.
- Otomatik olarak imzalanan için SmartLPA (çoklu oturum açma) ile Azure AD hesaplarına açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile SmartLPA yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliğiniz
- Abonelik SmartLPA çoklu oturum açma etkin

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, şunları yapabilirsiniz [bir aylık deneme sürümü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden SmartLPA ekleme
2. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-smartlpa-from-the-gallery"></a>Galeriden SmartLPA ekleme
Azure AD'de SmartLPA tümleştirmesini yapılandırmak için SmartLPA Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden SmartLPA eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **SmartLPA**seçin **SmartLPA** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Sonuç listesinde SmartLPA](./media/smartlpa-tutorial/tutorial_smartlpa_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırın ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı SmartLPA sınayın.

Tek iş için oturum açma için Azure AD ne SmartLPA karşılığı kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısının SmartLPA ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma SmartLPA ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[SmartLPA test kullanıcısı oluşturma](#create-a-smartlpa-test-user)**  - kullanıcı Azure AD gösterimini bağlı SmartLPA Britta simon'un bir karşılığı vardır.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve SmartLPA uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma ile SmartLPA yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **SmartLPA** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/smartlpa-tutorial/tutorial_smartlpa_samlbase.png)

3. Üzerinde **SmartLPA etki alanı ve URL'ler** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![SmartLPA etki alanı ve URL'ler tek oturum açma bilgileri](./media/smartlpa-tutorial/tutorial_smartlpa_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://<TENANTNAME>.smartlpa.com/`

    b. İçinde **tanımlayıcı** metin kutusuna bir URL şu biçimi kullanarak: `https://<TENANTNAME>.smartlpa.com/<UNIQUE ID>`

    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler gerçek oturum açma URL'si ve tanımlayıcı ile güncelleştirin. İlgili kişi [SmartLPA istemci Destek ekibine](mailto:support@smartlpa.com) bu değerleri almak için.

4. Üzerinde **SAML imzalama sertifikası** bölümünde **sertifika (Base64)** ve bilgisayarınızdaki sertifika dosyasını kaydedin.

    ![Sertifika indirme bağlantısı](./media/smartlpa-tutorial/tutorial_smartlpa_certificate.png) 

5. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açma Kaydet düğmesi yapılandırın](./media/smartlpa-tutorial/tutorial_general_400.png)

6. Üzerinde **SmartLPA yapılandırma** bölümünde **yapılandırma SmartLPA** açmak için **yapılandırma oturum açma** penceresi. Kopyalama **oturum kapatma URL'si, SAML varlık kimliği ve SAML çoklu oturum açma hizmeti URL'si** gelen **hızlı başvuru bölümü.**

    ![SmartLPA yapılandırma](./media/smartlpa-tutorial/tutorial_smartlpa_configure.png) 

7. Çoklu oturum açmayı yapılandırma **SmartLPA** tarafı, indirilen göndermek için ihtiyacınız **sertifika (Base64)**, **oturum kapatma URL'si, SAML varlık kimliği ve SAML çoklu oturum açma hizmeti URL'si**için [SmartLPA Destek ekibine](mailto:support@smartlpa.com). Bunlar, her iki kenarı da düzgün ayarlandığından SAML SSO bağlantı sağlamak için bu ayarı ayarlayın.

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, sol bölmede, tıklayın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/smartlpa-tutorial/create_aaduser_01.png)

2. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](./media/smartlpa-tutorial/create_aaduser_02.png)

3. Açmak için **kullanıcı** iletişim kutusu, tıklayın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/smartlpa-tutorial/create_aaduser_03.png)

4. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/smartlpa-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** Britta Simon kullanıcı e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değeri yazın **parola** kutusu.

    d. **Oluştur**’a tıklayın.
 
### <a name="create-a-smartlpa-test-user"></a>SmartLPA test kullanıcısı oluşturma

Bu bölümde, Britta Simon SmartLPA içinde adlı bir kullanıcı oluşturun. Çalışmak [SmartLPA Destek ekibine](mailto:support@smartlpa.com) SmartLPA platform kullanıcıları eklemek için. Kullanıcı oluşturulmalı ve çoklu oturum açma kullanmadan önce etkinleştirildi.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Azure çoklu oturum açma kullanmak için SmartLPA erişim vererek Britta Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**Britta Simon SmartLPA için atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

2. Uygulamalar listesinde **SmartLPA**.

    ![Uygulamalar listesinde SmartLPA bağlantı](./media/smartlpa-tutorial/tutorial_smartlpa_app.png)  

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı][202]

4. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde SmartLPA kutucuğa tıkladığınızda, otomatik olarak SmartLPA uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/smartlpa-tutorial/tutorial_general_01.png
[2]: ./media/smartlpa-tutorial/tutorial_general_02.png
[3]: ./media/smartlpa-tutorial/tutorial_general_03.png
[4]: ./media/smartlpa-tutorial/tutorial_general_04.png

[100]: ./media/smartlpa-tutorial/tutorial_general_100.png

[200]: ./media/smartlpa-tutorial/tutorial_general_200.png
[201]: ./media/smartlpa-tutorial/tutorial_general_201.png
[202]: ./media/smartlpa-tutorial/tutorial_general_202.png
[203]: ./media/smartlpa-tutorial/tutorial_general_203.png

