---
title: 'Öğretici: Görüntünüzün Labs - mobil ve Web testi ile Azure Active Directory Tümleştirme | Microsoft Docs'
description: Azure Active Directory ve Tariflerinizden Labs - mobil ve Web testi arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 3142d947-70e5-4345-8a30-b92d8715fac9
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/23/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: e99f794c4eb9db78f50f63f14ccfad08c65ddb07
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60590822"
---
# <a name="tutorial-azure-active-directory-integration-with-sauce-labs---mobile-and-web-testing"></a>Öğretici: Görüntünüzün Labs - mobil ve Web testi ile Azure Active Directory Tümleştirme

Bu öğreticide, Tariflerinizden Laboratuvarlarla - mobil ve Web testi Azure Active Directory (Azure AD) ile tümleştirme konusunda bilgi edinin.

Görüntünüzün Labs - mobil ve Web testi Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Görüntünüzün Labs - mobil ve Web testi erişimi, Azure AD'de kontrol edebilirsiniz.
- Otomatik olarak imzalanan için Sauce Labs - mobil ve Web testi (çoklu oturum açma) ile Azure AD hesaplarına açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Görüntünüzün Laboratuvarlarla - mobil ve Web testi, Azure AD tümleştirmesi yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliği
- Abonelik bir adım öne Geçiren sır Labs - mobil ve Web çoklu oturum açmayı test etkin

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, şunları yapabilirsiniz [bir aylık deneme sürümü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Görüntünüzün Labs - mobil ve Web testi galerisinden ekleme
2. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-sauce-labs---mobile-and-web-testing-from-the-gallery"></a>Görüntünüzün Labs - mobil ve Web testi galerisinden ekleme
Görüntünüzün Labs - mobil ve Web testi Azure AD'ye tümleştirmesini yapılandırmak için Sauce Labs - mobil ve uygulamalarınızın listesini yönetilen SaaS uygulamaları için Web testi galerisinden eklemeniz gerekir.

**Görüntünüzün Labs - mobil ve Web testi galerisinden eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **Tariflerinizden Labs - mobil ve Web testi**seçin **Tariflerinizden Labs - mobil ve Web testi** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Görüntünüzün Labs - mobil ve Web testi sonuç listesinde](./media/saucelabs-mobileandwebtesting-tutorial/tutorial_saucelabs_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma Tariflerinizden Labs ile test etme - mobil ve Web testi "Britta Simon" adlı bir test kullanıcı tabanlı.

Tek çalışmak için oturum açma için Azure AD ne karşılık gelen kullanıcı Tariflerinizden Labs - mobil ve Web testi için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısı ve ilgili kullanıcı Tariflerinizden Labs - mobil ve Web testi arasında bir bağlantı ilişki kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma Tariflerinizden laboratuvarlarla - test etme için mobil ve Web testi, aşağıdaki yapı taşlarını izlemeniz gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Bir adım öne Geçiren sır Laboratuvarları oluşturma - kullanıcı mobil ve Web testi test](#create-a-sauce-labs---mobile-and-web-testing-test-user)**  : Sauce Labs - mobil ve Web kullanıcı Azure AD gösterimini bağlantılı test Britta simon'un bir karşılığı vardır.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma, Tariflerinizden Labs'de - mobil ve Web uygulama testi yapılandırın.

**Görüntünüzün Laboratuvarlarla - mobil ve Web testi, Azure AD çoklu oturum açmayı yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **Tariflerinizden Labs - mobil ve Web testi** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma iletişim kutusu](./media/saucelabs-mobileandwebtesting-tutorial/tutorial_saucelabs_samlbase.png)

3. Üzerinde **Tariflerinizden Labs - mobil ve Web testi etki alanı ve URL'ler** bölümü, kullanıcı gerekmez uygulama zaten Azure ile önceden tümleştirilmiştir gibi tüm adımları gerçekleştirin.

    ![Görüntünüzün Labs - mobil ve Web testi etki alanı ve oturum açma URL tek bilgi](./media/saucelabs-mobileandwebtesting-tutorial/tutorial_saucelabs_url.png)

4. Üzerinde **SAML imzalama sertifikası** bölümünde **meta veri XML** ve bilgisayarınızda meta veri dosyasını kaydedin.

    ![Sertifika indirme bağlantısı](./media/saucelabs-mobileandwebtesting-tutorial/tutorial_saucelabs_certificate.png)

5. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açma Kaydet düğmesi yapılandırın](./media/saucelabs-mobileandwebtesting-tutorial/tutorial_general_400.png)

6. Farklı bir web tarayıcı penceresinde Tariflerinizden Labs - mobil ve Web şirket site yönetici olarak sınama için oturum açın.

7. Tıklayarak **kullanıcı simgesi** seçip **Ekip Yönetimi** sekmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/saucelabs-mobileandwebtesting-tutorial/configure1.png)

8. Girin, **etki alanı adı** metin kutusuna.

    ![Çoklu oturum açmayı yapılandırın](./media/saucelabs-mobileandwebtesting-tutorial/configure2.png)

9. Tıklayın **yapılandırma** sekmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/saucelabs-mobileandwebtesting-tutorial/configure3.png)

10. İçinde **çoklu oturum yapılandırma açma aktar** bölümünde, aşağıdaki adımları gerçekleştirin.

    ![Çoklu oturum açmayı yapılandırın](./media/saucelabs-mobileandwebtesting-tutorial/configure4.png)

    a. Tıklayın **Gözat** ve Azure AD'den indirilen meta veri dosyasını karşıya yükleyin.

    b. Seçin **izin tam zamanında sağlama** onay kutusu.

    c. **Kaydet**’e tıklayın.

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, sol bölmede, tıklayın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/saucelabs-mobileandwebtesting-tutorial/create_aaduser_01.png)

2. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](./media/saucelabs-mobileandwebtesting-tutorial/create_aaduser_02.png)

3. Açmak için **kullanıcı** iletişim kutusu, tıklayın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/saucelabs-mobileandwebtesting-tutorial/create_aaduser_03.png)

4. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/saucelabs-mobileandwebtesting-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** Britta Simon kullanıcı e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değeri yazın **parola** kutusu.

    d. **Oluştur**’a tıklayın.
  
### <a name="create-a-sauce-labs---mobile-and-web-testing-test-user"></a>Bir adım öne Geçiren sır Laboratuvarları oluşturma - kullanıcı, mobil ve Web testi test

Bu bölümün amacı, Britta Simon Tariflerinizden Labs - mobil ve Web testi adlı bir kullanıcı oluşturmaktır. Labs - Mobile tariflerinizden ve Web testi varsayılan olarak etkin olan tam zamanında sağlama, destekler. Bu bölümde, hiçbir eylem öğesini yoktur. Yeni bir kullanıcı Tariflerinizden Labs - mobil ve henüz mevcut değilse Web testi erişme denemesi sırasında oluşturulur.
>[!Note]
>Bir kullanıcı el ile oluşturmanız gerekiyorsa, kişi [Tariflerinizden Labs - mobil ve Web testi Destek ekibine](mailto:support@saucelabs.com).

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Azure çoklu oturum açma Tariflerinizden Labs kullanarak - mobil ve Web testi erişim vererek kullanmak Britta Simon etkinleştirin.

![Kullanıcı rolü atayın][200]

**Britta Simon Tariflerinizden Labs kullanarak - mobil ve Web testi, atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201]

2. Uygulamalar listesinde **Tariflerinizden Labs - mobil ve Web testi**.

    ![Uygulamalar listesinde Tariflerinizden Labs - mobil ve Web testi bağlantı](./media/saucelabs-mobileandwebtesting-tutorial/tutorial_saucelabs_app.png)  

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı][202]

4. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklayın **atama** düğmesini **atama Ekle** iletişim.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Tariflerinizden Labs tıklayın - mobil ve Web testi, erişim Paneli'nde döşeme sonra otomatik olarak Tariflerinizden Laboratuvarlarınızı için - mobil ve Web uygulaması sınama açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/saucelabs-mobileandwebtesting-tutorial/tutorial_general_01.png
[2]: ./media/saucelabs-mobileandwebtesting-tutorial/tutorial_general_02.png
[3]: ./media/saucelabs-mobileandwebtesting-tutorial/tutorial_general_03.png
[4]: ./media/saucelabs-mobileandwebtesting-tutorial/tutorial_general_04.png

[100]: ./media/saucelabs-mobileandwebtesting-tutorial/tutorial_general_100.png

[200]: ./media/saucelabs-mobileandwebtesting-tutorial/tutorial_general_200.png
[201]: ./media/saucelabs-mobileandwebtesting-tutorial/tutorial_general_201.png
[202]: ./media/saucelabs-mobileandwebtesting-tutorial/tutorial_general_202.png
[203]: ./media/saucelabs-mobileandwebtesting-tutorial/tutorial_general_203.png
