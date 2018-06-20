---
title: 'Öğretici: Azure Active Directory Tümleştirme ile Intacct | Microsoft Docs'
description: Çoklu oturum açma Azure Active Directory ile Intacct arasında yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 92518e02-a62c-4b1b-a8e9-2803eb2b49ac
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: jeedes
ms.openlocfilehash: 1fb8a1ac4474e91671710ce1f84980f296ecd8df
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/19/2018
ms.locfileid: "36224275"
---
# <a name="tutorial-azure-active-directory-integration-with-intacct"></a>Öğretici: Azure Active Directory Tümleştirme Intacct ile

Bu öğreticide, Azure Active Directory (Azure AD) ile Intacct tümleştirmek öğrenin.

Intacct Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Intacct erişimi, Azure AD'de kontrol edebilirsiniz
- Otomatik olarak için Intacct (çoklu oturum açma) ile Azure AD hesaplarına açan kullanıcılarınıza etkinleştirebilirsiniz
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirme Intacct ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir Intacct çoklu oturum açma abonelik etkin

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden Intacct ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-intacct-from-the-gallery"></a>Galeriden Intacct ekleme
Azure AD Intacct tümleştirilmesi yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden Intacct eklemeniz gerekir.

**Galeriden Intacct eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Uygulamalar][3]

4. Arama kutusuna **Intacct**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/intacct-tutorial/tutorial_intacct_search.png)

5. Sonuçlar panelinde seçin **Intacct**ve ardından **Ekle** uygulama eklemek için düğmesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/intacct-tutorial/tutorial_intacct_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Çoklu oturum açmayı yapılandırma ve Azure AD sınama
Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı Intacct sınayın.

Tekli çalışmaya oturum için Azure AD Intacct karşılık gelen kullanıcı için bir kullanıcı Azure AD'de nedir bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının Intacct ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Intacct içinde değerini atayın **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** bağlantı ilişkisi oluşturmak için.

Yapılandırma ve Azure AD çoklu oturum açma Intacct ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Bir Intacct test kullanıcısı oluşturma](#creating-an-intacct-test-user)**  - Britta Simon, karşılık gelen kullanıcı Azure AD gösterimini bağlı Intacct sağlamak için.
4. **[Azure AD test kullanıcısı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açmayı test](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma Intacct uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma ile Intacct yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **Intacct** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/intacct-tutorial/tutorial_intacct_samlbase.png)

3. Üzerinde **Intacct etki alanı ve URL'leri** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/intacct-tutorial/tutorial_intacct_url.png)

    İçinde **yanıt URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın:
    | |
    |--|
    | `https://<companyname>.intacct.com/ia/acct/sso_response.phtml`|
    | `https://www.intacct.com/ia/acct/sso_response.phtml` |

    > [!NOTE] 
    > Bu değer gerçek değil. Bu değer ile gerçek yanıt URL'si güncelleştirin. Kişi [Intacct destek ekibi](https://us.intacct.com/support) bu değeri alınamıyor.

4. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **Certificate(Base64)** ve sertifika dosyayı bilgisayarınıza kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/intacct-tutorial/tutorial_intacct_certificate.png) 

5. Tıklatın **kaydetmek** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/intacct-tutorial/tutorial_general_400.png)

6. Üzerinde **Intacct yapılandırma** 'yi tıklatın **yapılandırma Intacct** açmak için **yapılandırma oturum açma** penceresi. Kopya **SAML varlık kimliği ve SAML çoklu oturum açma hizmet URL'si** gelen **hızlı başvuru bölümü.**

    ![Çoklu oturum açmayı yapılandırın](./media/intacct-tutorial/tutorial_intacct_configure.png) 

7. Farklı web tarayıcısı penceresinde Intacct şirket sitenize yönetici olarak oturum açın.

8. Tıklatın **şirket** sekmesini ve ardından **şirket bilgisi**.

    ![Şirket](./media/intacct-tutorial/ic790037.png "şirket")

9. Tıklatın **güvenlik** sekmesini ve ardından **Düzenle**.

    ![Güvenlik](./media/intacct-tutorial/ic790038.png "güvenlik")

10. İçinde **tek oturum açma (SSO)** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı](./media/intacct-tutorial/ic790039.png "çoklu oturum açmayı")

    a. Seçin **etkinleştirmek çoklu oturum açma**.

    b. Olarak **kimlik sağlayıcısı türü**seçin **SAML 2.0**.

    c. İçinde **veren URL'si** metin değerini yapıştırın **SAML varlık kimliği** Azure portalından kopyalanan.
   
    d. İçinde **oturum açma URL'si** metin değerini yapıştırın **SAML çoklu oturum açma hizmet URL'si** Azure portalından kopyalanan.

    e. Açık, **base-64** kodlanmış sertifika Not Defteri'nde, içeriğini, panoya kopyalayın ve yapıştırın kendisine **sertifika** kutusu.
   
    f. **Kaydet**’e tıklayın.

> [!TIP]
> Şimdi bu yönergeleri içinde kısa bir sürümünü okuyabilirsiniz [Azure portal](https://portal.azure.com)uygulaması kuruluyor yaparken!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** sekmesinde ve aracılığıyla katıştırılmış belgelere erişebilir **yapılandırma** alt bölüm. Daha fazla bilgiyi burada embedded belgeler özelliği hakkında: [Azure AD embedded belgeler]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

![Azure AD Kullanıcı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portal**, sol gezinti bölmesinde tıklatın **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/intacct-tutorial/create_aaduser_01.png) 

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/intacct-tutorial/create_aaduser_02.png) 

3. Açmak için **kullanıcı** iletişim kutusunda, tıklatın **Ekle** iletişim kutusunun üst kısmında.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/intacct-tutorial/create_aaduser_03.png) 

4. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/intacct-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna, türü **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna, türü **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.
 
### <a name="creating-an-intacct-test-user"></a>Bir Intacct test kullanıcısı oluşturma

Intacct için oturum açabilmeniz bunlar için Azure AD kullanıcıları ayarlamak için bunların Intacct sağlanmalıdır. Intacct için sağlama bir el ile bir görevdir.

**Kullanıcı hesaplarını sağlamak için aşağıdaki adımları gerçekleştirin:**

1. Oturum açın, **Intacct** Kiracı.

2. Tıklatın **şirket** sekmesini ve ardından **kullanıcılar**.

    ![Kullanıcıların](./media/intacct-tutorial/ic790041.png "kullanıcılar")
3. Tıklatın **Ekle** sekmesi.

    ![Ekleme](./media/intacct-tutorial/ic790042.png "ekleme")
4. İçinde **kullanıcı bilgilerini** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı bilgilerini](./media/intacct-tutorial/ic790043.png "kullanıcı bilgileri")

    a. Girin **kullanıcı kimliği**, **Soyadı**, **ad**, **e-posta adresi**, **başlık**ve **Telefon** sağlamayı istediğiniz bir Azure AD hesabının **kullanıcı bilgilerini** bölümü.

    b. Seçin **yönetici ayrıcalıkları** sağlamak istediğiniz bir Azure AD hesabının.
   
    c. **Kaydet**’e tıklayın. Azure AD hesap sahibi bir e-posta alır ve bunu etkinleştirilmeden önce kendi hesabı onaylamak için bir bağlantı izler.

>[!NOTE]
>Azure AD kullanıcı hesaplarını sağlamak için diğer Intacct kullanıcı hesabı oluşturma araçlarını veya Intacct tarafından sağlanan API'leri kullanabilirsiniz.
        
### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atama

Bu bölümde, Britta Intacct için erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı atama][200] 

**Intacct için Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **Intacct**.

    ![Çoklu oturum açmayı yapılandırın](./media/intacct-tutorial/tutorial_intacct_app.png) 

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    ![Kullanıcı atama][202] 

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Kullanıcı atama][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açmayı test etme

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test.

Erişim paneli Intacct parçasında tıklattığınızda, otomatik olarak Intacct uygulamanıza oturum açmanız.

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/intacct-tutorial/tutorial_general_01.png
[2]: ./media/intacct-tutorial/tutorial_general_02.png
[3]: ./media/intacct-tutorial/tutorial_general_03.png
[4]: ./media/intacct-tutorial/tutorial_general_04.png

[100]: ./media/intacct-tutorial/tutorial_general_100.png

[200]: ./media/intacct-tutorial/tutorial_general_200.png
[201]: ./media/intacct-tutorial/tutorial_general_201.png
[202]: ./media/intacct-tutorial/tutorial_general_202.png
[203]: ./media/intacct-tutorial/tutorial_general_203.png

