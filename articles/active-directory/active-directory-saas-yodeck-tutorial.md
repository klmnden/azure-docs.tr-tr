---
title: 'Öğretici: Azure Active Directory Tümleştirme ile Yodeck | Microsoft Docs'
description: Çoklu oturum açma Azure Active Directory ile Yodeck arasında yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: b2c8dccb-eeb0-4f4d-a24d-8320631ce819
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/20/2018
ms.author: jeedes
ms.openlocfilehash: ba48c5d14775004e22dd67a2932c8969e6dfdd6d
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2018
---
# <a name="tutorial-azure-active-directory-integration-with-yodeck"></a>Öğretici: Azure Active Directory Tümleştirme Yodeck ile

Bu öğreticide, Azure Active Directory (Azure AD) ile Yodeck tümleştirmek öğrenin.

Yodeck Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Yodeck erişimi, Azure AD'de kontrol edebilirsiniz.
- Otomatik olarak için Yodeck (çoklu oturum açma) ile Azure AD hesaplarına açan kullanıcılarınıza etkinleştirebilirsiniz.
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirme Yodeck ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir Yodeck çoklu oturum açma abonelik etkin

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, şunları yapabilirsiniz [bir aylık deneme sürümünü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden Yodeck ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-yodeck-from-the-gallery"></a>Galeriden Yodeck ekleme
Azure AD Yodeck tümleştirilmesi yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden Yodeck eklemeniz gerekir.

**Galeriden Yodeck eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **Yodeck**seçin **Yodeck** sonuç panelinden ardından **Ekle** uygulama eklemek için düğmeyi.

    ![Sonuçlar listesinde Yodeck](./media/active-directory-saas-yodeck-tutorial/tutorial_yodeck_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı Yodeck sınayın.

Tekli çalışmaya oturum için Azure AD Yodeck karşılık gelen kullanıcı için bir kullanıcı Azure AD'de nedir bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının Yodeck ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma Yodeck ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Yodeck test kullanıcısı oluşturma](#create-a-yodeck-test-user)**  - Britta Simon, karşılık gelen kullanıcı Azure AD gösterimini bağlı Yodeck sağlamak için.
4. **[Azure AD test kullanıcısı atayın](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Test çoklu oturum açma](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma Yodeck uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma ile Yodeck yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **Yodeck** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma iletişim kutusu](./media/active-directory-saas-yodeck-tutorial/tutorial_yodeck_samlbase.png)

3. Üzerinde **Yodeck etki alanı ve URL'leri** bölümünde, uygulamada yapılandırmak istiyorsanız aşağıdaki adımları gerçekleştirin **IDP** modu tarafından başlatılan:

    ![Yodeck etki alanı ve URL'leri tek oturum açma bilgileri](./media/active-directory-saas-yodeck-tutorial/tutorial_yodeck_url.png)

    İçinde **tanımlayıcısı (varlık kimliği)** metin kutusuna, URL'yi yazın: `https://app.yodeck.com/api/v1/account/metadata/`

4. Denetleme **Göster Gelişmiş URL ayarları** ve uygulamada yapılandırmak istiyorsanız aşağıdaki adımı gerçekleştirin **SP** modunda başlatılan:

    ![Yodeck etki alanı ve URL'leri tek oturum açma bilgileri](./media/active-directory-saas-yodeck-tutorial/tutorial_yodeck_url1.png)

    İçinde **oturum açma URL'si** metin kutusuna, URL'yi yazın: `https://app.yodeck.com/login`

5. Üzerinde **SAML imzalama sertifikası** bölümünde, kopyalamak için Kopyala düğmesini tıklatın **uygulama Federasyon meta veri URL'sini** ve Not Defteri'ne yapıştırın.

    ![Sertifika indirme bağlantısı](./media/active-directory-saas-yodeck-tutorial/tutorial_yodeck_certificate.png)

6. Tıklatın **kaydetmek** düğmesi.

    ![Oturum açma tek Kaydet düğmesi yapılandırın](./media/active-directory-saas-yodeck-tutorial/tutorial_general_400.png)
    
7. Farklı web tarayıcısı penceresinde Yodeck şirket sitenize yönetici olarak oturum açın.

8. Tıklayın **kullanıcı ayarları** seçeneğini seçin ve sayfanın sağ üst köşedeki form **hesap ayarlarını**.

    ![Yodeck yapılandırma](./media/active-directory-saas-yodeck-tutorial/configure1.png)

9. Seçin **SAML** ve aşağıdaki adımları gerçekleştirin:

    ![Yodeck yapılandırma](./media/active-directory-saas-yodeck-tutorial/configure2.png)

    a. Seçin **alma URL'den**.

    b. İçinde **URL** metin kutusuna, Yapıştır **uygulama Federasyon meta veri URL'sini** Azure portal ve tıklatın kopyaladığınız değeri **alma**.
    
    c. Aldıktan sonra **uygulama Federasyon meta veri URL'sini**, geri kalan alanları otomatik olarak doldurur.

    d. **Kaydet**’e tıklayın.

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Sol bölmede, Azure portal'ı tıklatın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/active-directory-saas-yodeck-tutorial/create_aaduser_01.png)

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantılar](./media/active-directory-saas-yodeck-tutorial/create_aaduser_02.png)

3. Açmak için **kullanıcı** iletişim kutusu, tıklatın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/active-directory-saas-yodeck-tutorial/create_aaduser_03.png)

4. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/active-directory-saas-yodeck-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** kullanıcı Britta Simon e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değer aşağı yazma **parola** kutusu.

    d. **Oluştur**’a tıklayın.
 
### <a name="create-a-yodeck-test-user"></a>Yodeck test kullanıcısı oluşturma

Azure AD kullanıcıları için Yodeck oturum açmak etkinleştirmek için bunların Yodeck sağlanmalıdır.
Yodeck söz konusu olduğunda, sağlama bir el ile bir görevdir.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1. Yodeck şirket sitenize yönetici olarak oturum açın.

2. Tıklayın **kullanıcı ayarları** seçeneğini seçin ve sayfanın sağ üst köşedeki form **kullanıcılar**.

    ![Çalışanı ekleyin](./media/active-directory-saas-yodeck-tutorial/user1.png)

3. Tıklayın **+ kullanıcı** açmak için **kullanıcı ayrıntıları** sekmesi.

    ![Çalışanı ekleyin](./media/active-directory-saas-yodeck-tutorial/user2.png)

4. Üzerinde **kullanıcı ayrıntıları** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:

    ![Çalışanı ekleyin](./media/active-directory-saas-yodeck-tutorial/user3.png)

    a. İçinde **ad** kullanıcı türü adı metin kutusuna, ister **Britta**.

    b. İçinde **Soyadı** metin kutusuna, kullanıcının soyadını türü ister **Simon**.

    c. İçinde **e-posta** metin kutusuna, kullanıcının e-posta adresi türü ister **brittasimon@contoso.com**.

    d. Select uygun **hesap izinlerini** seçeneği kuruluş gereksinimi göredir.
    
    e. **Kaydet**’e tıklayın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Britta Yodeck için erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı rolü atayın][200]

**Yodeck için Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201]

2. Uygulamalar listesinde **Yodeck**.

    ![Uygulamalar listesinde Yodeck bağlantı](./media/active-directory-saas-yodeck-tutorial/tutorial_yodeck_app.png)  

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantı][202]

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Ekleme atama bölmesi][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli Yodeck parçasında tıklattığınızda, otomatik olarak Yodeck uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](active-directory-saas-tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/active-directory-saas-yodeck-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-yodeck-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-yodeck-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-yodeck-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-yodeck-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-yodeck-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-yodeck-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-yodeck-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-yodeck-tutorial/tutorial_general_203.png

