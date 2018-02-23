---
title: "Öğretici: Azure Active Directory Tümleştirme katılımcı Yönetim Hizmetleri ile | Microsoft Docs"
description: "Çoklu oturum açma Azure Active Directory katılım Yönetim Hizmetleri arasındaki yapılandırmayı öğrenin."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 1f56e612-728b-4203-a545-a81dc5efda00
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/13/2018
ms.author: jeedes
ms.openlocfilehash: 1fcbbabe80c3ff4b5a18904637cb227499da6829
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/21/2018
---
# <a name="tutorial-azure-active-directory-integration-with-attendance-management-services"></a>Öğretici: Azure Active Directory katılım Yönetim Hizmetleri ile tümleştirme

Bu öğreticide, katılım Yönetim Hizmetleri'ni Azure Active Directory (Azure AD) ile tümleştirme öğrenin.

Katılım Yönetim Hizmetleri Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Katılım Yönetim Hizmetleri erişimi, Azure AD'de kontrol edebilirsiniz.
- Azure AD hesaplarına otomatik olarak (çoklu oturum açma) katılımını Yönetim hizmetlerine açan kullanıcılarınıza etkinleştirebilirsiniz.
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirme katılımcı Yönetim Hizmetleri ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir katılım Yönetim Hizmetleri çoklu oturum açma abonelik etkin

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, şunları yapabilirsiniz [bir aylık deneme sürümünü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden katılımcı Yönetim Hizmetleri ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-attendance-management-services-from-the-gallery"></a>Galeriden katılımcı Yönetim Hizmetleri ekleme
Azure AD katılımını Yönetim Hizmetleri tümleştirilmesi yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden katılımcı Yönetim Hizmetleri eklemeniz gerekir.

**Galeriden katılımcı Yönetim Hizmetleri eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna yazın **katılımcı Yönetim Hizmetleri**seçin **katılımcı Yönetim Hizmetleri** sonuç panelinden ardından **Ekle** uygulama eklemek için düğmeyi.

    ![Sonuçlar listesinde katılımcı Yönetim Hizmetleri](./media/active-directory-saas-attendancemanagementservices-tutorial/tutorial_attendancemanagementservices_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı katılımcı Yönetim Hizmetleri ile test etme.

Tekli çalışmaya oturum için Azure AD katılımını yönetim Hizmetleri'nde karşılık gelen kullanıcının bir kullanıcı Azure AD'de nedir bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının ve ilgili kullanıcı katılımı yönetim Hizmetleri'nde arasında bir bağlantı ilişkisi kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma katılımcı Yönetim Hizmetleri ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Katılım Yönetim Hizmetleri test kullanıcısı oluşturma](#create-an-attendance-management-service-test-user)**  - Britta Simon, karşılık gelen kullanıcı Azure AD gösterimini bağlı katılımcı yönetim hizmetleri sağlamak için.
4. **[Azure AD test kullanıcısı atayın](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Test çoklu oturum açma](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma katılımcı Yönetim Hizmetleri uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma katılımcı Yönetim Hizmetleri ile yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **katılımcı Yönetim Hizmetleri** uygulama tümleştirmesi sayfasında, tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/active-directory-saas-attendancemanagementservices-tutorial/tutorial_attendancemanagementservices_samlbase.png)

3. Üzerinde **katılımcı Yönetim Hizmetleri etki alanı ve URL'leri** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Katılım Yönetim Hizmetleri etki alanı ve URL'leri tek oturum açma bilgileri](./media/active-directory-saas-attendancemanagementservices-tutorial/tutorial_attendancemanagementservices_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://id.obc.jp/<tenant information >/`

    b. İçinde **tanımlayıcısı** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://id.obc.jp/<tenant information >/`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerler gerçek oturum açma URL'si ve tanımlayıcı ile güncelleştirin. Kişi [katılımcı Management Services İstemcisi destek ekibi](http://www.obcnet.jp/) bu değerleri almak için.

4. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **Certificate(Base64)** ve sertifika dosyayı bilgisayarınıza kaydedin.

    ![Sertifika indirme bağlantısı](./media/active-directory-saas-attendancemanagementservices-tutorial/tutorial_attendancemanagementservices_certificate.png) 

5. Tıklatın **kaydetmek** düğmesi.

    ![Oturum açma tek Kaydet düğmesi yapılandırın](./media/active-directory-saas-attendancemanagementservices-tutorial/tutorial_general_400.png)

6. Üzerinde **katılımcı Yönetim Hizmetleri Yapılandırması** 'yi tıklatın **katılımcı yönetim hizmetlerini yapılandır** açmak için **yapılandırma oturum açma** penceresi. Kopya **SAML varlık kimliği ve SAML çoklu oturum açma hizmet URL'si** gelen **hızlı başvuru bölümü.**

    ![Katılım Yönetim Hizmetleri Yapılandırması](./media/active-directory-saas-attendancemanagementservices-tutorial/tutorial_attendancemanagementservices_configure.png) 

7. Farklı bir tarayıcı penceresinde katılımcı Yönetim Hizmetleri şirket sitenize yönetici olarak oturum.

8. Tıklayın **SAML kimlik doğrulaması** altında **güvenlik yönetimi bölümünde**.

    ![Katılım Yönetim Hizmetleri Yapılandırması](./media/active-directory-saas-attendancemanagementservices-tutorial/user1.png)

9. Aşağıdaki adımları gerçekleştirin:

    ![Katılım Yönetim Hizmetleri Yapılandırması](./media/active-directory-saas-attendancemanagementservices-tutorial/user2.png)

    a. Seçin **kullanım SAML kimlik doğrulaması**.

    b. İçinde **tanımlayıcısı** metin değerini yapıştırın **SAML varlık kimliği**, Azure portalından kopyalanan. 

    c. İçinde **kimlik doğrulama uç noktası URL'si** metin değerini yapıştırın **SAML çoklu oturum açma hizmet URL'si**, Azure portalından kopyalanan.

    d. Tıklatın **bir dosya seçin** Azure AD'den indirilen sertifikayı karşıya yüklemek için.

    e. Seçin **devre dışı bırak parola kimlik doğrulaması**.

    f. Tıklatın **kayıt**

> [!TIP]
> Şimdi bu yönergeleri içinde kısa bir sürümünü okuyabilirsiniz [Azure portal](https://portal.azure.com)uygulaması kuruluyor yaparken! Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** sekmesinde ve aracılığıyla katıştırılmış belgelere erişebilir **yapılandırma** alt bölüm. Daha fazla bilgiyi burada embedded belgeler özelliği hakkında: [Azure AD embedded belgeler]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Sol bölmede, Azure portal'ı tıklatın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/active-directory-saas-attendancemanagementservices-tutorial/create_aaduser_01.png)

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantılar](./media/active-directory-saas-attendancemanagementservices-tutorial/create_aaduser_02.png)

3. Açmak için **kullanıcı** iletişim kutusu, tıklatın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/active-directory-saas-attendancemanagementservices-tutorial/create_aaduser_03.png)

4. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/active-directory-saas-attendancemanagementservices-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** kullanıcı Britta Simon e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değer aşağı yazma **parola** kutusu.

    d. **Oluştur**’a tıklayın.
 
### <a name="create-an-attendance-management-services-test-user"></a>Katılım Yönetim Hizmetleri test kullanıcısı oluşturma

Azure AD kullanıcıların katılımcı yönetim hizmetlerinde oturum açmasına olanak tanımak için bunlar katılımcı Yönetim Hizmetleri içine sağlanmalıdır. Katılım Yönetim Hizmetleri söz konusu olduğunda, sağlama bir el ile bir görevdir.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1. Katılım Yönetim Hizmetleri şirket sitenize yönetici olarak oturum açın.

2. Tıklayın **kullanıcı yönetimi** altında **güvenlik yönetimi bölümünde**.

    ![Çalışanı ekleyin](./media/active-directory-saas-attendancemanagementservices-tutorial/user5.png)

3. Tıklatın **yeni kurallar oturum açma**.

    ![Çalışanı ekleyin](./media/active-directory-saas-attendancemanagementservices-tutorial/user3.png)

4. İçinde **OBCiD bilgi** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çalışanı ekleyin](./media/active-directory-saas-attendancemanagementservices-tutorial/user4.png)

    a. İçinde **OBCiD** metin kutusu, kullanıcı e-posta türünü ister  **BrittaSimon@contoso.com** .

    b. İçinde **parola** metin kutusuna, kullanıcının parolasını yazın.

    c. Tıklatın **kayıt**


### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Britta katılımcı Yönetim hizmetlerine erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**Katılım yönetim hizmetlere Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **katılımcı Yönetim Hizmetleri**.

    ![Uygulamalar listesinde katılımcı Yönetim Hizmetleri bağlantı](./media/active-directory-saas-attendancemanagementservices-tutorial/tutorial_attendancemanagementservices_app.png)  

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantı][202]

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Ekleme atama bölmesi][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açmayı test edin

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli katılımcı yönetim hizmetler kutucuğunda tıklattığınızda, otomatik olarak katılımcı Management Services uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](active-directory-saas-tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-attendancemanagementservices-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-attendancemanagementservices-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-attendancemanagementservices-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-attendancemanagementservices-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-attendancemanagementservices-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-attendancemanagementservices-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-attendancemanagementservices-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-attendancemanagementservices-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-attendancemanagementservices-tutorial/tutorial_general_203.png

