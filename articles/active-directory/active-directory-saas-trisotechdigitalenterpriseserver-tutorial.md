---
title: 'Öğretici: Azure Active Directory Tümleştirme Trisotech dijital Kurumsal sunucusuyla | Microsoft Docs'
description: Çoklu oturum açma Azure Active Directory ile Trisotech dijital Enterprise Server arasında yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 6d54d20c-eca1-4fa6-b56a-4c3ed0593db0
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/16/2018
ms.author: jeedes
ms.openlocfilehash: 8be41cbdbea237d2075523f22caf52238d921f24
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2018
---
# <a name="tutorial-azure-active-directory-integration-with-trisotech-digital-enterprise-server"></a>Öğretici: Azure Active Directory Tümleştirme Trisotech dijital kuruluş sunucusu

Bu öğreticide, Azure Active Directory (Azure AD) ile Trisotech dijital Enterprise Server tümleştirmek öğrenin.

Trisotech dijital Enterprise Server Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Trisotech dijital kuruluş sunucusuna erişimi olan Azure AD'de kontrol edebilirsiniz.
- Azure AD hesaplarına otomatik olarak Trisotech dijital kuruluş sunucusuna (çoklu oturum açma) açan kullanıcılarınıza etkinleştirebilirsiniz.
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirme Trisotech dijital Kurumsal sunucusuyla yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir Trisotech dijital Enterprise Server Çoklu oturum açma abonelik etkin

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, şunları yapabilirsiniz [bir aylık deneme sürümünü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden Trisotech dijital Enterprise Server ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-trisotech-digital-enterprise-server-from-the-gallery"></a>Galeriden Trisotech dijital Enterprise Server ekleme
Azure AD Trisotech dijital Enterprise Server tümleştirilmesi yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden Trisotech dijital kuruluş sunucusu eklemeniz gerekir.

**Galeriden Trisotech dijital Enterprise sunucu eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna yazın **Trisotech dijital Enterprise Server**seçin **Trisotech dijital Enterprise Server** sonuç panelinden ardından **Ekle** Ekle düğmesi uygulama.

    ![Sonuçlar listesinde Trisotech dijital kuruluş sunucusu](./media/active-directory-saas-trisotechdigitalenterpriseserver-tutorial/tutorial_trisotechdigitalenterpriseserver_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma Trisotech dijital Kurumsal "Britta Simon" adlı bir test kullanıcı tabanlı sunucu ile test etme.

Tekli çalışmaya oturum için Azure AD ne karşılık gelen Trisotech dijital Enterprise Server'daki bir kullanıcı için Azure AD içinde olduğu bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının Trisotech dijital Enterprise Server ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma Trisotech dijital kuruluş sunucusu ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Trisotech dijital Enterprise Server test kullanıcısı oluşturma](#create-a-trisotech-digital-enterprise-server-test-user)**  - Britta Simon, karşılık gelen Trisotech dijital Kurumsal kullanıcı Azure AD gösterimini bağlı olan sunucusu sağlamak için.
4. **[Azure AD test kullanıcısı atayın](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Test çoklu oturum açma](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma Trisotech dijital Enterprise Server uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma Trisotech dijital Kurumsal sunucusuyla yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **Trisotech dijital Enterprise Server** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/active-directory-saas-trisotechdigitalenterpriseserver-tutorial/tutorial_trisotechdigitalenterpriseserver_samlbase.png)

3. Üzerinde **Trisotech dijital kurumsal sunucu etki alanı ve URL'leri** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Trisotech dijital kurumsal sunucu etki alanı ve URL'leri tek oturum açma bilgileri](./media/active-directory-saas-trisotechdigitalenterpriseserver-tutorial/tutorial_trisotechdigitalenterpriseserver_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://<companyname>.trisotech.com`

    b. İçinde **tanımlayıcısı** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://<companyname>.trisotech.com`

    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler gerçek oturum açma URL'si ve tanımlayıcı ile güncelleştirin. Kişi [Trisotech dijital Enterprise Server istemci destek ekibi](mailto:support@trisotech.com) bu değerleri almak için.

4. Üzerinde **SAML imzalama sertifikası** bölümünde, kopyalamak için Kopyala düğmesini tıklatın **uygulama Federasyon meta veri URL'sini** ve Not Defteri'ne yapıştırın. 

    ![Sertifika indirme bağlantısı](./media/active-directory-saas-trisotechdigitalenterpriseserver-tutorial/tutorial_trisotechdigitalenterpriseserver_certificate.png)

5. Tıklatın **kaydetmek** düğmesi.

    ![Oturum açma tek Kaydet düğmesi yapılandırın](./media/active-directory-saas-trisotechdigitalenterpriseserver-tutorial/tutorial_general_400.png)

6. Farklı web tarayıcısı penceresinde Trisotech dijital Enterprise sunucu yapılandırması şirket sitenize yönetici olarak oturum açın.

7. Tıklayın **menüsü simgesini** ve ardından **Yönetim**.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-trisotechdigitalenterpriseserver-tutorial/user1.png)

8. Seçin **kullanıcı sağlayıcısı**.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-trisotechdigitalenterpriseserver-tutorial/user2.png)

9. İçinde **kullanıcı sağlayıcısı yapılandırmaları** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-trisotechdigitalenterpriseserver-tutorial/user3.png)

    a. Seçin **güvenli onaylama işlemi biçimlendirme dili 2 (SAML 2)** açılır menüde gelen **kimlik doğrulama yöntemini**.

    b. İçinde **meta veri URL'sini** metin kutusuna, Yapıştır **uygulama Federasyon meta veri URL'sini** kopyaladığınız değeri form Azure portalı.

    c. İçinde **uygulama kimliği** metin kutusuna, URL şu biçimi kullanarak girin: `https://<companyname>.trisotech.com`.

    d. **Kaydet**’e tıklayın

    e. Etki alanı adını girin **(boş gelir herkes) etki alanlarına izin** metin kutusuna, otomatik olarak atar izin etki alanları ile eşleşen kullanıcıları için lisans

    f. **Kaydet**’e tıklayın

 ### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Sol bölmede, Azure portal'ı tıklatın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/active-directory-saas-trisotechdigitalenterpriseserver-tutorial/create_aaduser_01.png)

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantılar](./media/active-directory-saas-trisotechdigitalenterpriseserver-tutorial/create_aaduser_02.png)

3. Açmak için **kullanıcı** iletişim kutusu, tıklatın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/active-directory-saas-trisotechdigitalenterpriseserver-tutorial/create_aaduser_03.png)

4. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/active-directory-saas-trisotechdigitalenterpriseserver-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** kullanıcı Britta Simon e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değer aşağı yazma **parola** kutusu.

    d. **Oluştur**’a tıklayın.
 
### <a name="create-a-trisotech-digital-enterprise-server-test-user"></a>Trisotech dijital Enterprise Server test kullanıcısı oluşturma

Bu bölümün amacı Britta Simon Trisotech dijital Enterprise Server'da adlı bir kullanıcı oluşturmaktır. Trisotech dijital Enterprise Server yalnızca zaman sağlama, varsayılan olarak etkin olduğu destekler. Bu bölümde, eylem öğe yok. Yeni bir kullanıcı henüz yoksa Trisotech dijital Enterprise Server erişme denemesi sırasında oluşturulur.
>[!Note]
>Bir kullanıcı el ile oluşturmanız gerekiyorsa, kişi [Trisotech dijital Enterprise Server destek ekibi](mailto:support@trisotech.com).

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Britta Trisotech dijital kuruluş sunucusuna erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**Britta Simon Trisotech dijital kuruluş sunucusuna atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **Trisotech dijital Enterprise Server**.

    ![Uygulamalar listesinde Trisotech dijital kuruluş sunucusuna bağlantı](./media/active-directory-saas-trisotechdigitalenterpriseserver-tutorial/tutorial_trisotechdigitalenterpriseserver_app.png)  

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantı][202]

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Ekleme atama bölmesi][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli Trisotech dijital Enterprise Server parçasında tıklattığınızda, otomatik olarak Trisotech dijital Enterprise Server uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](active-directory-saas-tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/active-directory-saas-trisotechdigitalenterpriseserver-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-trisotechdigitalenterpriseserver-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-trisotechdigitalenterpriseserver-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-trisotechdigitalenterpriseserver-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-trisotechdigitalenterpriseserver-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-trisotechdigitalenterpriseserver-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-trisotechdigitalenterpriseserver-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-trisotechdigitalenterpriseserver-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-trisotechdigitalenterpriseserver-tutorial/tutorial_general_203.png

