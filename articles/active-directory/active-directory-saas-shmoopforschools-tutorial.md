---
title: 'Öğretici: Azure Active Directory ile tümleştirme için Shmoop okullar ile | Microsoft Docs'
description: Çoklu oturum açma Azure Active Directory için Shmoop okullar arasındaki yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: 1d75560a-55b3-42e9-bda1-92b01c572d8e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/23/2018
ms.author: jeedes
ms.openlocfilehash: 799a88344d6c348061af19bfbbd9022025d2d66b
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2018
---
# <a name="tutorial-azure-active-directory-integration-with-shmoop-for-schools"></a>Öğretici: Azure Active Directory ile tümleştirme için Shmoop okullar ile

Bu öğreticide, Azure Active Directory (Azure AD) ile Shmoop için okullar tümleştirmek öğrenin.

Shmoop için okullar Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- İçin Shmoop okullar erişimi olan Azure AD'de kontrol edebilirsiniz.
- Otomatik olarak için Shmoop okullar kendi Azure AD hesaplarıyla oturum, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınızı bir merkezi konumda--Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla bilgi için bkz: [uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirmesi için Shmoop okullar ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir Shmoop okullar için çoklu oturum açma abonelik etkin

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamında kullanmanızı öneririz yok.

Bu öğreticide adımları sınamak için önerilir:

- Yalnızca gerekli olduğunda, üretim ortamından kullanma.
- Başlarken bir [ücretsiz bir aylık deneme](https://azure.microsoft.com/pricing/free-trial/) bir Azure AD deneme ortam zaten yoksa.

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden Shmoop için okullar ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="add-shmoop-for-schools-from-the-gallery"></a>Galeriden Shmoop için okullar ekleme
Azure AD için Shmoop okullar tümleştirilmesi yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden Shmoop için okullar eklemeniz gerekir.

**Galeriden Shmoop için okullar eklemek için aşağıdaki adımları uygulayın:**

1. İçinde [Azure portal](https://portal.azure.com), sol bölmede seçin **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Git **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
3. Yeni bir uygulama eklemek için seçin **yeni uygulama** iletişim kutusunun üst kısmında düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **Shmoop için okullar**. Seçin **Shmoop için okullar** sonuçlarından seçip **Ekle** uygulama eklemek için düğmesi.

    ![Sonuçlar listesinde Shmoop için okullar](./media/active-directory-saas-shmoopforschools-tutorial/tutorial_shmoopforschools_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma Shmoop için "Britta Simon." olarak adlandırılan bir test kullanıcı göre okulların ile test etme

Tekli çalışmaya oturum için Azure AD için Shmoop okullar karşılık gelen kullanıcı için bir kullanıcı Azure AD'de olan bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının ve Shmoop için okullar ilgili kullanıcı arasında bir bağlantı oluşturmanız gerekir.

Yapılandırma ve Azure AD çoklu oturum açma için Shmoop okullar ile test etmek için aşağıdaki yapı taşları tamamlayın:

1. [Azure AD çoklu oturum açma yapılandırma](#configure-azure-ad-single-sign-on) bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. [Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user) Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. [Shmoop için okullar test kullanıcısı oluşturma](#create-a-shmoop-for-schools-test-user) Britta Simon, karşılık gelen Shmoop için kullanıcı Azure AD gösterimini bağlantılı okullar sağlamak için.
4. [Azure AD test kullanıcısı atayın](#assign-the-azure-ad-test-user) Britta Azure AD çoklu oturum açma kullanmak Simon etkinleştirmek için.
5. [Test çoklu oturum açma](#test-single-sign-on) yapılandırma çalıştığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma için Shmoop okullar uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma için Shmoop okullar ile yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **Shmoop için okullar** uygulama tümleştirmesi sayfasında, **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. İçinde **çoklu oturum açma** iletişim kutusunda aşağı açılan menüsünün altında **tek oturum açma modu**seçin **SAML tabanlı oturum açma**.
 
    ![Çoklu oturum açma iletişim kutusu](./media/active-directory-saas-shmoopforschools-tutorial/tutorial_shmoopforschools_samlbase.png)

3. İçinde **Shmoop için okullar etki alanı ve URL'leri** bölümünde, aşağıdaki adımları uygulayın:

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-shmoopforschools-tutorial/tutorial_shmoopforschools_url.png)

    a. İçinde **oturum açma URL'si** kutusunda, aşağıdaki desende bir URL yazın: `https://schools.shmoop.com/public-api/saml2/start/<uniqueid>`

    b. İçinde **tanımlayıcısı** kutusunda, aşağıdaki desende bir URL yazın: `https://schools.shmoop.com/<uniqueid>`

    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerleri tanımlayıcısı ve gerçek oturum açma URL'si ile güncelleştirin. Kişi [okullar Shmoop istemci destek ekibi](mailto:support@shmoop.com) bu değerleri almak için. 
 
4. Shmoop için okullar uygulaması SAML onaylar belirli bir biçimde bekliyor. Bu uygulama için aşağıdaki talep yapılandırın. Bu öznitelik değerlerini yönetebilirsiniz **kullanıcı öznitelikleri** bölümü uygulama tümleştirmesi sayfasında. Aşağıdaki ekran görüntüsünde nasıl onaylar yapılandırılacağını göstermektedir:

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-shmoopforschools-tutorial/tutorial_attribute.png)

    > [!NOTE]
    > Okul Shmoop kullanıcılar için iki rolünü destekler: **Öğretmen** ve **Öğrenci**. Bu rolleri Azure AD'de kullanıcıları uygun roller atanabilir şekilde ayarlayın. Azure AD'de rollerini yapılandırmak nasıl anlamak için bkz: [rol tabanlı erişim denetimi kullanarak Azure AD bulut uygulamalarında](http://www.dushyantgill.com/blog/2014/12/10/roles-based-access-control-in-cloud-applications-using-azure-ad/).
    
5. İçinde **kullanıcı öznitelikleri** bölümüne **çoklu oturum açma** iletişim kutusunda, SAML belirteci özniteliği önceki görüntüde gösterildiği gibi yapılandırın.  Ardından aşağıdaki adımları uygulayın:

    | Öznitelik adı | Öznitelik değeri |
    | -------------- | --------------- |
    | rol           | User.assignedroles |

    a. Açmak için **özniteliği eklemek** iletişim kutusunda **Ekle özniteliği**.
    
    ![Çoklu oturum açmayı yapılandırın ](./media/active-directory-saas-shmoopforschools-tutorial/tutorial_attribute_04.png)
    
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-shmoopforschools-tutorial/tutorial_attribute_05.png)
    
    b. İçinde **adı** ilgili satır için gösterilen öznitelik adı yazın.
    
    c. Gelen **değeri** listesinde, ilgili satır için gösterilen öznitelik değerini seçin.

    d. Bırakın **Namespace** kutusunu boş.
    
    e. Seçin **Tamam**.

6. **Kaydet** düğmesini seçin.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-shmoopforschools-tutorial/tutorial_general_400.png)

7. Üzerinde **SAML imzalama sertifikası** bölümünde, kopyalamak için Kopyala düğmesini tıklatın **uygulama Federasyon meta veri URL'sini** ve Not Defteri'ne yapıştırın.

    ![Sertifika indirme bağlantısı](./media/active-directory-saas-shmoopforschools-tutorial/tutorial_shmoopforschools_certificate.png)

8. Çoklu oturum açma yapılandırmak için **Shmoop için okullar** yan, ihtiyacınız göndermek **uygulama Federasyon meta veri URL'sini** için [Shmoop için okullar destek ekibi](mailto:support@shmoop.com).

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Azure portalında Britta Simon adlı bir test kullanıcı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları uygulayın:**

1. Azure portalında sol bölmede seçin **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/active-directory-saas-shmoopforschools-tutorial/create_aaduser_01.png)

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar**. Ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantılar](./media/active-directory-saas-shmoopforschools-tutorial/create_aaduser_02.png)

3. Açmak için **kullanıcı** iletişim kutusunda **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/active-directory-saas-shmoopforschools-tutorial/create_aaduser_03.png)

4. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları uygulayın:

    ![Kullanıcı iletişim kutusu](./media/active-directory-saas-shmoopforschools-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** kullanıcı Britta Simon e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değer aşağı yazma **parola** kutusu.

    d. **Oluştur**’u seçin.
 
### <a name="create-a-shmoop-for-schools-test-user"></a>Shmoop için okullar test kullanıcısı oluşturma

Bu bölümün amacı Britta Simon okullar için Shmoop adlı bir kullanıcı oluşturmaktır. Shmoop için okullar yalnızca zaman sağlama, varsayılan olarak etkin olduğu destekler. Bu bölümde, eylem öğe yok. Yeni bir kullanıcı henüz yoksa, Shmoop için okullar erişme denemesi sırasında oluşturulur.

>[!NOTE]
>Bir kullanıcı el ile oluşturmanız gerekiyorsa, kişi [Shmoop için okullar destek ekibi](mailto:support@shmoop.com).

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Britta için Shmoop okullar erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**İçin Shmoop okullar Britta Simon atamak için aşağıdaki adımları uygulayın:**

1. Azure portalında uygulamaları görünümünü açın. Ardından **kurumsal uygulamalar** dizin görünümünde.  Ardından, **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **Shmoop için okullar**.

    ![Uygulamalar listesinde Shmoop için okullar bağlantı](./media/active-directory-saas-shmoopforschools-tutorial/tutorial_shmoopforschools_app.png)  

3. Soldaki menüde seçin **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantı][202]

4. Seçin **Ekle** düğmesi. Ardından **eklemek atama** iletişim kutusunda **kullanıcılar ve gruplar**.

    ![Ekleme atama bölmesi][203]

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. İçinde **kullanıcılar ve gruplar** iletişim kutusu, tıklatın **seçin** düğmesi. 

7. İçinde **eklemek atama** iletişim kutusunda **atamak** düğmesi.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test.

Seçtiğinizde, **Shmoop için okullar** döşeme erişim panelinde oturumunuz otomatik olarak Shmoop için okullar uygulamanıza.

Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile yönelik öğreticiler listesi](active-directory-saas-tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/active-directory-saas-shmoopforschools-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-shmoopforschools-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-shmoopforschools-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-shmoopforschools-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-shmoopforschools-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-shmoopforschools-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-shmoopforschools-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-shmoopforschools-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-shmoopforschools-tutorial/tutorial_general_203.png

