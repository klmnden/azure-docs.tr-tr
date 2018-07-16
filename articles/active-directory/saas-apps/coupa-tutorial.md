---
title: 'Öğretici: Azure Active Directory tümleştirmesiyle Coupa | Microsoft Docs'
description: Azure Active Directory ve Coupa arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 47f27746-9057-4b9c-991e-3abf77710f73
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/28/2018
ms.author: jeedes
ms.openlocfilehash: adad60611f1447b78173368ed137205f077cb8b7
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/14/2018
ms.locfileid: "39047819"
---
# <a name="tutorial-azure-active-directory-integration-with-coupa"></a>Öğretici: Azure Active Directory Coupa ile tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile Coupa tümleştirme konusunda bilgi edinin.

Azure AD ile Coupa tümleştirme ile aşağıdaki avantajları sağlar:

- Coupa erişimi, Azure AD'de kontrol edebilirsiniz.
- Otomatik olarak imzalanan için Coupa (çoklu oturum açma) ile Azure AD hesaplarına açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile Coupa yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliğiniz
- Abonelik Coupa çoklu oturum açma etkin

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, şunları yapabilirsiniz [bir aylık deneme sürümü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden Coupa ekleme
2. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-coupa-from-the-gallery"></a>Galeriden Coupa ekleme
Azure AD'de Coupa tümleştirmesini yapılandırmak için Coupa Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden Coupa eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi.

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **Coupa**seçin **Coupa** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Sonuç listesinde Coupa](./media/coupa-tutorial/tutorial_coupa_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırın ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı Coupa sınayın.

Tek iş için oturum açma için Azure AD ne Coupa karşılığı kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısının Coupa ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Coupa içinde değerini atayın **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** bağlantı kurmak için.

Yapılandırma ve Azure AD çoklu oturum açma Coupa ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Coupa test kullanıcısı oluşturma](#create-a-coupa-test-user)**  - kullanıcı Azure AD gösterimini bağlı Coupa Britta simon'un bir karşılığı vardır.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve Coupa uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma ile Coupa yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **Coupa** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma iletişim kutusu](./media/coupa-tutorial/tutorial_coupa_samlbase.png)

3. Üzerinde **Coupa etki alanı ve URL'ler** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Coupa etki alanı ve URL'ler tek oturum açma bilgileri](./media/coupa-tutorial/tutorial_coupa_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://<companyname>.coupahost.com`

    > [!NOTE]
    > Oturum açma URL değeri, gerçek değil. Bu değer, gerçek oturum açma URL'si ile güncelleştirin. İlgili kişi [Coupa istemci Destek ekibine](https://success.coupa.com/Support/Contact_Us?) bu değeri alınamıyor.

    b. İçinde **tanımlayıcı** metin kutusuna URL'yi yazın:

    | Ortam  | URL'si |
    |:-------------|----|
    | Korumalı Alan | `devsso35.coupahost.com`|
    | Üretim | `prdsso40.coupahost.com`|
    | | |

    c. İçinde **yanıt URL'si** metin kutusuna URL'yi yazın:

    | Ortam | URL'si |
    |------------- |----|
    | Korumalı Alan | `https://devsso35.coupahost.com/sp/ACS.saml2`|
    | Üretim | `https://prdsso40.coupahost.com/sp/ACS.saml2`|
    | | |

4. Üzerinde **SAML imzalama sertifikası** bölümünde **meta veri XML** ve bilgisayarınızda meta veri dosyasını kaydedin.

    ![Sertifika indirme bağlantısı](./media/coupa-tutorial/tutorial_coupa_certificate.png) 

5. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açma Kaydet düğmesi yapılandırın](./media/coupa-tutorial/tutorial_general_400.png)

6. Coupa şirketinizin sitesi için bir yönetici olarak oturum açın.

7. Git **Kurulum \> güvenlik denetimi**.

   ![Güvenlik denetimleri](./media/coupa-tutorial/ic791900.png "güvenlik denetimleri")

8. İçinde **Coupa kimlik bilgilerini kullanarak oturum** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Coupa SP meta verileri](./media/coupa-tutorial/ic791901.png "Coupa SP meta verileri")

    a. Seçin **SAML kullanarak oturum**.

    b. Tıklayın **Gözat** Azure portalından indirilen meta verilerini karşıya yüklemek için.

    c. **Kaydet**’e tıklayın.

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, sol bölmede, tıklayın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/coupa-tutorial/create_aaduser_01.png)

2. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](./media/coupa-tutorial/create_aaduser_02.png)

3. Açmak için **kullanıcı** iletişim kutusu, tıklayın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/coupa-tutorial/create_aaduser_03.png)

4. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/coupa-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** Britta Simon kullanıcı e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değeri yazın **parola** kutusu.

    d. **Oluştur**’a tıklayın.

### <a name="create-a-coupa-test-user"></a>Coupa test kullanıcısı oluşturma

Coupa açarken Azure AD kullanıcılarının etkinleştirmek için bunların Coupa sağlanması gerekir.  

* Coupa söz konusu olduğunda, sağlama bir el ile gerçekleştirilen bir görevdir.

**Kullanıcı sağlamayı yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Oturum açın, **Coupa** şirketinizin sitesi yöneticisi olarak.

2. Üstteki menüden **Kurulum**ve ardından **kullanıcılar**.

   ![Kullanıcılar](./media/coupa-tutorial/ic791908.png "kullanıcılar")

3. **Oluştur**’a tıklayın.

   ![Kullanıcılar oluşturma](./media/coupa-tutorial/ic791909.png "kullanıcılar oluşturma")

4. İçinde **oluşturacağı** bölümünde, aşağıdaki adımları gerçekleştirin:

   ![Kullanıcı ayrıntılarını](./media/coupa-tutorial/ic791910.png "kullanıcı ayrıntıları")

   a. Tür **oturum açma**, **ad**, **Soyadı**, **tek oturum açma kimliği**, **e-posta** özniteliklerinin bir Geçerli Azure Active Directory hesabı ilgili metin kutularına zbilgisayarlar istiyorsunuz.

   b. **Oluştur**’a tıklayın.

   >[!NOTE]
   >Azure Active Directory hesap sahibi bu etkinleştirilmeden önce hesabı onaylamak için bir bağlantı içeren bir e-posta alırsınız.
   >

>[!NOTE]
>Herhangi diğer Coupa kullanıcı hesabı oluşturma araçları kullanabilir veya API'leri için AAD kullanıcı hesapları sağlamak Coupa tarafından sağlanan.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Azure çoklu oturum açma kullanmak için Coupa erişim vererek Britta Simon etkinleştirin.

![Kullanıcı rolü atayın][200]

**Britta Simon Coupa için atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201]

2. Uygulamalar listesinde **Coupa**.

    ![Uygulamalar listesinde Coupa bağlantı](./media/coupa-tutorial/tutorial_coupa_app.png)  

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı][202]

4. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklayın **atama** düğmesini **atama Ekle** iletişim.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde Coupa kutucuğa tıkladığınızda, otomatik olarak Coupa uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/coupa-tutorial/tutorial_general_01.png
[2]: ./media/coupa-tutorial/tutorial_general_02.png
[3]: ./media/coupa-tutorial/tutorial_general_03.png
[4]: ./media/coupa-tutorial/tutorial_general_04.png

[100]: ./media/coupa-tutorial/tutorial_general_100.png

[200]: ./media/coupa-tutorial/tutorial_general_200.png
[201]: ./media/coupa-tutorial/tutorial_general_201.png
[202]: ./media/coupa-tutorial/tutorial_general_202.png
[203]: ./media/coupa-tutorial/tutorial_general_203.png
