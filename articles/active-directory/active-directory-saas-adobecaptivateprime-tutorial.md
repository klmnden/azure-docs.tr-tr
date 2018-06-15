---
title: 'Öğretici: Azure Active Directory Tümleştirme ile Adobe büyüleyecektir asal | Microsoft Docs'
description: Çoklu oturum açma Azure Active Directory ve Adobe büyüleyecektir asal arasında yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 2f95b226-1465-47f4-b8b7-de4b0772abbc
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/02/2018
ms.author: jeedes
ms.openlocfilehash: 1cb31fa9a5c7fdcd4158c9c2d2ddccc7125ba3bd
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2018
ms.locfileid: "34337852"
---
# <a name="tutorial-azure-active-directory-integration-with-adobe-captivate-prime"></a>Öğretici: Adobe büyüleyecektir asal Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile Adobe büyüleyecektir asal tümleştirmek öğrenin.

Adobe büyüleyecektir asal Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Adobe büyüleyecektir asal erişimi, Azure AD'de kontrol edebilirsiniz.
- Azure AD hesaplarına otomatik olarak Adobe büyüleyecektir asal için (çoklu oturum açma) açan kullanıcılarınıza etkinleştirebilirsiniz.
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirme Adobe asal büyüleyecektir yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir Adobe büyüleyecektir asal çoklu oturum açma abonelik etkin

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, şunları yapabilirsiniz [bir aylık deneme sürümünü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden Adobe büyüleyecektir asal ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-adobe-captivate-prime-from-the-gallery"></a>Galeriden Adobe büyüleyecektir asal ekleme
Azure AD Adobe büyüleyecektir asal tümleştirilmesi yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden Adobe büyüleyecektir asal eklemeniz gerekir.

**Adobe büyüleyecektir asal Galeriden eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **Adobe büyüleyecektir asal**seçin **Adobe büyüleyecektir asal** sonuç panelinden ardından **Ekle** uygulama eklemek için düğmeyi.

    ![Adobe büyüleyecektir asal sonuçlar listesinde](./media/active-directory-saas-adobecaptivateprime-tutorial/tutorial_adobecaptivateprime_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırmak ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı Adobe büyüleyecektir asal sınayın.

Tekli çalışmaya oturum için Azure AD Adobe büyüleyecektir asal karşılık gelen kullanıcı için bir kullanıcı Azure AD'de nedir bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının Adobe büyüleyecektir asal ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma Adobe büyüleyecektir asal ile test etmek için aşağıdaki yapı taşları tamamlanması gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Adobe büyüleyecektir asal test kullanıcısı oluşturma](#create-an-adobe-captivate-prime-test-user)**  - karşılık gelen Britta Simon, Adobe büyüleyecektir asal kullanıcı Azure AD gösterimini bağlı sağlamak için.
4. **[Azure AD test kullanıcısı atayın](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Test çoklu oturum açma](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma Adobe büyüleyecektir asal uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma Adobe asal büyüleyecektir yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **Adobe büyüleyecektir asal** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/active-directory-saas-adobecaptivateprime-tutorial/tutorial_adobecaptivateprime_samlbase.png)

3. Üzerinde **Adobe büyüleyecektir asal etki alanı ve URL'leri** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Adobe büyüleyecektir asal etki alanı ve URL'leri tek oturum açma bilgileri](./media/active-directory-saas-adobecaptivateprime-tutorial/tutorial_adobecaptivateprime_url.png)

    a. İçinde **tanımlayıcısı** metin kutusuna, bir URL yazın: `https://captivateprime.adobe.com`

    b. İçinde **yanıt URL'si** metin kutusuna, bir URL yazın: `https://captivateprime.adobe.com/saml/SSO`

4. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **meta veri XML** ve meta veri dosyası, bilgisayarınıza kaydedin.

    ![Sertifika indirme bağlantısı](./media/active-directory-saas-adobecaptivateprime-tutorial/tutorial_adobecaptivateprime_certificate.png) 

5. Tıklatın **kaydetmek** düğmesi.

    ![Oturum açma tek Kaydet düğmesi yapılandırın](./media/active-directory-saas-adobecaptivateprime-tutorial/tutorial_general_400.png)

6. Git **özellikleri** sekmesinde, kopya **kullanıcı erişim URL'si** ve Not Defteri'ne yapıştırın.

    ![Kullanıcı erişim bağlantısı](./media/active-directory-saas-adobecaptivateprime-tutorial/tutorial_adobecaptivateprime_appprop.png)

7. Çoklu oturum açma yapılandırmak için **Adobe büyüleyecektir asal** yan, indirilen göndermek için ihtiyacınız **meta veri XML** ve kopyalanan **kullanıcı erişim URL'si** için [Adobe Asal destek ekibi büyüleyecektir](mailto:captivateprimesupport@adobe.com). Bunlar, her iki tarafta da ayarlamanızı SAML SSO bağlantı sağlamak için bu ayarı ayarlayın.

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Sol bölmede, Azure portal'ı tıklatın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/active-directory-saas-adobecaptivateprime-tutorial/create_aaduser_01.png)

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantılar](./media/active-directory-saas-adobecaptivateprime-tutorial/create_aaduser_02.png)

3. Açmak için **kullanıcı** iletişim kutusu, tıklatın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/active-directory-saas-adobecaptivateprime-tutorial/create_aaduser_03.png)

4. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/active-directory-saas-adobecaptivateprime-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** kullanıcı Britta Simon e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değer aşağı yazma **parola** kutusu.

    d. **Oluştur**’a tıklayın.
  
### <a name="create-an-adobe-captivate-prime-test-user"></a>Adobe büyüleyecektir asal test kullanıcısı oluşturma

Bu bölümde, Adobe büyüleyecektir asal içinde Britta Simon adlı bir kullanıcı oluşturun. Çalışmak [Adobe büyüleyecektir asal destek ekibi](mailto:captivateprimesupport@adobe.com) Adobe büyüleyecektir asal platform kullanıcıları eklemek için. Kullanıcıların gerekir ve çoklu oturum açma kullanmadan önce etkinleştirildi

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Adobe büyüleyecektir asal erişim vererek, Azure çoklu oturum açma kullanılacak Britta Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**Adobe büyüleyecektir asal Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **Adobe büyüleyecektir asal**.

    ![Uygulamalar listesinde Adobe büyüleyecektir asal bağlantı](./media/active-directory-saas-adobecaptivateprime-tutorial/tutorial_adobecaptivateprime_app.png)  

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantı][202]

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Ekleme atama bölmesi][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli Adobe büyüleyecektir asal parçasında tıklattığınızda, otomatik olarak Adobe büyüleyecektir asal uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](active-directory-saas-tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/active-directory-saas-adobecaptivateprime-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-adobecaptivateprime-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-adobecaptivateprime-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-adobecaptivateprime-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-adobecaptivateprime-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-adobecaptivateprime-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-adobecaptivateprime-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-adobecaptivateprime-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-adobecaptivateprime-tutorial/tutorial_general_203.png

