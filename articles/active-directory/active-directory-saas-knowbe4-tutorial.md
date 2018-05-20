---
title: 'Öğretici: Azure Active Directory Tümleştirme KnowBe4 güvenlik tanıma eğitim ile | Microsoft Docs'
description: Çoklu oturum açma Azure Active Directory ve KnowBe4 güvenlik tanıma eğitim arasında yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: b80d2212-cc5f-4adb-836c-570640810c39
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/31/2017
ms.author: jeedes
ms.openlocfilehash: cfbcd48b1cb49f2e042ab0d4afa71d1d20226a46
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2018
---
# <a name="tutorial-azure-active-directory-integration-with-knowbe4-security-awareness-training"></a>Öğretici: Azure Active Directory Tümleştirme KnowBe4 güvenlik tanıma eğitim ile

Bu öğreticide, Azure Active Directory (Azure AD) ile KnowBe4 güvenlik tanıma eğitim tümleştirmek öğrenin.

KnowBe4 güvenlik tanıma eğitim Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- KnowBe4 güvenlik tanıma eğitim erişimi, Azure AD'de kontrol edebilirsiniz.
- Azure AD hesaplarına otomatik olarak Itanium tabanlı sistemler için KnowBe4 güvenlik tanıma eğitim için (çoklu oturum açma) açan kullanıcılarınıza etkinleştirebilirsiniz.
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirme KnowBe4 güvenlik tanıma eğitim ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir KnowBe4 güvenlik tanıma eğitim çoklu oturum açma abonelik etkin

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, şunları yapabilirsiniz [bir aylık deneme sürümünü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden KnowBe4 güvenlik tanıma eğitim ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-knowbe4-security-awareness-training-from-the-gallery"></a>Galeriden KnowBe4 güvenlik tanıma eğitim ekleme
Azure AD KnowBe4 güvenlik tanıma eğitim tümleştirilmesi yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden KnowBe4 güvenlik tanıma eğitim eklemeniz gerekir.

**Galeriden KnowBe4 güvenlik tanıma eğitim eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna yazın **KnowBe4 güvenlik tanıma eğitim**seçin **KnowBe4 güvenlik tanıma eğitim** sonuç panelinden ardından **Ekle** Ekle düğmesi uygulama.

    ![Sonuçlar listesinde KnowBe4 güvenlik tanıma eğitim](./media/active-directory-saas-knowbe4-tutorial/tutorial_knowbe4_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma KnowBe4 güvenlik tanıma "Britta Simon" adlı bir test kullanıcı tabanlı eğitim ile test etme.

Tekli çalışmaya oturum için Azure AD ne karşılık gelen KnowBe4 güvenlik tanıma Eğitim'deki bir kullanıcı için Azure AD içinde olduğu bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının KnowBe4 güvenlik tanıma eğitim ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

KnowBe4 güvenlik tanıma eğitim değerini atayın **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** bağlantı ilişkisi oluşturmak için.

Yapılandırma ve Azure AD çoklu oturum açma KnowBe4 güvenlik tanıma eğitim ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[KnowBe4 güvenlik tanıma eğitim test kullanıcısı oluşturma](#create-a-knowbe4-security-awareness-training-test-user)**  - Britta Simon, karşılık gelen kullanıcı Azure AD gösterimini bağlı KnowBe4 güvenlik tanıma eğitim sağlamak için.
4. **[Azure AD test kullanıcısı atayın](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Test çoklu oturum açma](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma KnowBe4 güvenlik tanıma eğitim uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma KnowBe4 güvenlik tanıma eğitim ile yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **KnowBe4 güvenlik tanıma eğitim** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/active-directory-saas-knowbe4-tutorial/tutorial_knowbe4_samlbase.png)

3. Üzerinde **KnowBe4 güvenlik tanıma eğitim etki alanı ve URL'leri** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![KnowBe4 güvenlik tanıma eğitim etki alanı ve URL'leri tek oturum açma bilgileri](./media/active-directory-saas-knowbe4-tutorial/tutorial_knowbe4_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://<companyname>.KnowBe4.com/auth/saml/<instancename>`

    > [!NOTE] 
    > Oturum açma URL değeri gerçek değil. Bu değer gerçek oturum açma URL'si ile güncelleştirin. Kişi [KnowBe4 güvenlik tanıma eğitim istemci destek ekibi](mailto:support@KnowBe4.com) bu değeri alınamıyor. 

    b. İçinde **tanımlayıcısı** metin kutusuna, dize değeri yazın: `KnowBe4`

    > [!NOTE]
    >Bu küçük harf duyarlıdır

4. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **sertifika (ham)** ve sertifika dosyayı bilgisayarınıza kaydedin.

    ![Sertifika indirme bağlantısı](./media/active-directory-saas-knowbe4-tutorial/tutorial_knowbe4_certificate.png) 

5. Tıklatın **kaydetmek** düğmesi.

    ![Oturum açma tek Kaydet düğmesi yapılandırın](./media/active-directory-saas-knowbe4-tutorial/tutorial_general_400.png)

6. Üzerinde **KnowBe4 güvenlik tanıma eğitim Yapılandırması** 'yi tıklatın **KnowBe4 güvenlik tanıma eğitim yapılandırma** açmak için **yapılandırma oturum açma** penceresi. Kopya **Sign-Out URL, SAML varlık kimliği ve SAML çoklu oturum açma hizmet URL'si** gelen **hızlı başvuru bölümü.**

    ![KnowBe4 güvenlik tanıma eğitim yapılandırması](./media/active-directory-saas-knowbe4-tutorial/tutorial_knowbe4_configure.png) 

7. Çoklu oturum açma yapılandırmak için **KnowBe4 güvenlik tanıma eğitim** yan, indirilen göndermek için ihtiyacınız **sertifika (ham)**, **Sign-Out URL, SAML varlık kimliği ve SAML çoklu oturum açma hizmet URL'si** için [KnowBe4 güvenlik tanıma eğitim istemci destek ekibi](mailto:support@KnowBe4.com).

> [!TIP]
> Şimdi bu yönergeleri içinde kısa bir sürümünü okuyabilirsiniz [Azure portal](https://portal.azure.com)uygulaması kuruluyor yaparken!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** sekmesinde ve aracılığıyla katıştırılmış belgelere erişebilir **yapılandırma** alt bölüm. Daha fazla bilgiyi burada embedded belgeler özelliği hakkında: [Azure AD embedded belgeler]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Sol bölmede, Azure portal'ı tıklatın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/active-directory-saas-knowbe4-tutorial/create_aaduser_01.png)

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantılar](./media/active-directory-saas-knowbe4-tutorial/create_aaduser_02.png)

3. Açmak için **kullanıcı** iletişim kutusu, tıklatın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/active-directory-saas-knowbe4-tutorial/create_aaduser_03.png)

4. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/active-directory-saas-knowbe4-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** kullanıcı Britta Simon e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değer aşağı yazma **parola** kutusu.

    d. **Oluştur**’a tıklayın.
 
### <a name="create-a-knowbe4-security-awareness-training-test-user"></a>KnowBe4 güvenlik tanıma eğitim test kullanıcısı oluşturma

Bu bölümün amacı Britta Simon KnowBe4 güvenlik tanıma eğitim adlı bir kullanıcı oluşturmaktır. KnowBe4 güvenlik tanıma eğitim yalnızca zaman sağlama, varsayılan olarak etkin olduğu destekler.

Bu bölümde, eylem öğe yok. Yeni bir kullanıcı henüz yoksa KnowBe4 güvenlik tanıma eğitim erişme denemesi sırasında oluşturulur. 

>[!NOTE]
>Bir kullanıcı el ile oluşturmanız gerekiyorsa, başvurmanız gerekir [KnowBe4 güvenlik tanıma eğitim destek ekibi](mailto:support@KnowBe4.com).

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Britta KnowBe4 güvenlik tanıma eğitim için erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**KnowBe4 güvenlik tanıma eğitim Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **KnowBe4 güvenlik tanıma eğitim**.

    ![Uygulamalar listesinde KnowBe4 güvenlik tanıma eğitim bağlantı](./media/active-directory-saas-knowbe4-tutorial/tutorial_knowbe4_app.png)  

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantı][202]

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Ekleme atama bölmesi][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümün amacı erişim paneli kullanılarak Azure AD çoklu oturum açma yapılandırmanızı test etmektir.
  
Erişim paneli KnowBe4 güvenlik tanıma eğitim parçasında tıklattığınızda, otomatik olarak KnowBe4 güvenlik tanıma eğitim uygulamanıza açan. 

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](active-directory-saas-tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/active-directory-saas-knowbe4-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-knowbe4-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-knowbe4-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-knowbe4-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-knowbe4-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-knowbe4-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-knowbe4-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-knowbe4-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-knowbe4-tutorial/tutorial_general_203.png

