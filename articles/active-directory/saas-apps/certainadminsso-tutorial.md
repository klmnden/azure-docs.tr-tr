---
title: 'Öğretici: Azure Active Directory Tümleştirme belirli yönetim SSO | Microsoft Docs'
description: Çoklu oturum açma Azure Active Directory ve belirli yönetim SSO arasında yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 98ba0174-be02-408a-8634-c8113b12dedb
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/21/2018
ms.author: jeedes
ms.openlocfilehash: cf23620eb3a3b0a9cb8e5877f517281a9c21809c
ms.sourcegitcommit: 65b399eb756acde21e4da85862d92d98bf9eba86
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/22/2018
ms.locfileid: "36320621"
---
# <a name="tutorial-azure-active-directory-integration-with-certain-admin-sso"></a>Öğretici: Azure Active Directory Tümleştirme belirli yönetim SSO

Bu öğreticide, Azure Active Directory (Azure AD) ile belirli yönetim SSO tümleştirmek öğrenin.

Belirli yönetim SSO Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Belirli yönetim SSO erişimi, Azure AD'de kontrol edebilirsiniz.
- Azure AD hesaplarına otomatik olarak (çoklu oturum açma) için belirli yönetim SSO açan kullanıcılarınıza etkinleştirebilirsiniz.
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirme belirli yönetim SSO'su yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir belirli yönetim SSO çoklu oturum açma abonelik etkin

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, şunları yapabilirsiniz [bir aylık deneme sürümünü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Belirli yönetim SSO Galeriden ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-certain-admin-sso-from-the-gallery"></a>Belirli yönetim SSO Galeriden ekleme
Azure AD belirli yönetim SSO tümleştirilmesi yapılandırmak için belirli yönetim SSO Galeriden yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

**Belirli yönetim SSO Galeriden eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **belirli yönetim SSO**seçin **belirli yönetim SSO** sonuç panelinden ardından **Ekle** uygulama eklemek için düğmeyi.

    ![Sonuçlar listesinde belirli yönetim SSO](./media/certainadminsso-tutorial/tutorial_certainadminsso_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcıyı temel alarak belirli yönetim SSO ile test etme.

Tekli çalışmaya oturum için Azure AD ne karşılık gelen belirli yönetim SSO bir kullanıcı için Azure AD içinde olduğu bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının ve belirli yönetim SSO ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Yapılandırmak ve Azure AD çoklu oturum açma belirli yönetim SSO sınamak için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Belirli yönetim SSO test kullanıcısı oluşturma](#create-a-certain-admin-sso-test-user)**  - Britta Simon, karşılık gelen kullanıcı Azure AD gösterimini bağlı belirli yönetim SSO sağlamak için.
4. **[Azure AD test kullanıcısı atayın](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Test çoklu oturum açma](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma belirli yönetim SSO uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma belirli yönetim SSO'su yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **belirli yönetim SSO** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/certainadminsso-tutorial/tutorial_certainadminsso_samlbase.png)

3. Üzerinde **belirli yönetim SSO etki alanı ve URL'leri** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Belirli yönetim SSO etki alanı ve URL'leri tek oturum açma bilgileri](./media/certainadminsso-tutorial/tutorial_certainadminsso_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://<YOUR DOMAIN URL>/svcs/sso_admin_login/handleRequest/<ID>`

    b. İçinde **tanımlayıcısı** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://<SUBDOMAIN>.certain.com`

    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler gerçek oturum açma URL'si ve tanımlayıcı ile güncelleştirin. Kişi [belirli yönetim SSO istemci destek ekibi](mailto:integrations@certain.com) bu değerleri almak için. 
 
4. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **sertifika (ham)** ve sertifika dosyayı bilgisayarınıza kaydedin.

    ![Sertifika indirme bağlantısı](./media/certainadminsso-tutorial/tutorial_certainadminsso_certificate.png) 

5. Tıklatın **kaydetmek** düğmesi.

    ![Oturum açma tek Kaydet düğmesi yapılandırın](./media/certainadminsso-tutorial/tutorial_general_400.png)

6. Üzerinde **belirli yönetici SSO Yapılandırması** 'yi tıklatın **yapılandırma belirli yönetim SSO** açmak için **yapılandırma oturum açma** penceresi. Kopya **Sign-Out URL, SAML varlık kimliği ve SAML çoklu oturum açma hizmet URL'si** gelen **hızlı başvuru bölümü.**

    ![Belirli yönetim SSO yapılandırma](./media/certainadminsso-tutorial/tutorial_certainadminsso_configure.png) 

7. Çoklu oturum açma yapılandırmak için **belirli yönetim SSO** yan, indirilen göndermek için ihtiyacınız **sertifika (ham)**, **Sign-Out URL, SAML varlık kimliği ve SAML çoklu oturum açma hizmet URL'si**için [belirli yönetim SSO destek ekibi](mailto:integrations@certain.com). Bunlar, her iki tarafta da ayarlamanızı SAML SSO bağlantı sağlamak için bu ayarı ayarlayın.

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Sol bölmede, Azure portal'ı tıklatın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/certainadminsso-tutorial/create_aaduser_01.png)

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantılar](./media/certainadminsso-tutorial/create_aaduser_02.png)

3. Açmak için **kullanıcı** iletişim kutusu, tıklatın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/certainadminsso-tutorial/create_aaduser_03.png)

4. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/certainadminsso-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** kullanıcı Britta Simon e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değer aşağı yazma **parola** kutusu.

    d. **Oluştur**’a tıklayın.
 
### <a name="create-a-certain-admin-sso-test-user"></a>Belirli yönetim SSO test kullanıcısı oluşturma

Bu bölümde, belirli yönetim SSO içinde Britta Simon adlı bir kullanıcı oluşturun. Çalışmak [belirli yönetim SSO destek ekibi](mailto:integrations@certain.com) belirli yönetim SSO platform kullanıcıları eklemek için. Kullanıcıların oluşturulan ve çoklu oturum açma kullanmadan önce etkinleştirilmelidir.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, belirli yönetim SSO erişim vererek, Azure çoklu oturum açma kullanılacak Britta Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**Belirli yönetim SSO Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **belirli yönetim SSO**.

    ![Uygulamalar listesinde belirli yönetim SSO bağlantı](./media/certainadminsso-tutorial/tutorial_certainadminsso_app.png)  

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantı][202]

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Ekleme atama bölmesi][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli belirli yönetim SSO parçasında tıklattığınızda, otomatik olarak belirli yönetim SSO uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/certainadminsso-tutorial/tutorial_general_01.png
[2]: ./media/certainadminsso-tutorial/tutorial_general_02.png
[3]: ./media/certainadminsso-tutorial/tutorial_general_03.png
[4]: ./media/certainadminsso-tutorial/tutorial_general_04.png

[100]: ./media/certainadminsso-tutorial/tutorial_general_100.png

[200]: ./media/certainadminsso-tutorial/tutorial_general_200.png
[201]: ./media/certainadminsso-tutorial/tutorial_general_201.png
[202]: ./media/certainadminsso-tutorial/tutorial_general_202.png
[203]: ./media/certainadminsso-tutorial/tutorial_general_203.png

