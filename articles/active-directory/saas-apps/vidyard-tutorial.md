---
title: 'Öğretici: Azure Active Directory tümleştirmesiyle Vidyard | Microsoft Docs'
description: Azure Active Directory ve Vidyard arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: bed7df23-6e13-4e7c-b4cc-53ed4804664d
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/22/2018
ms.author: jeedes
ms.openlocfilehash: d796ebf6e30476d766a0d9b6c78ba4b5cf577b47
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/14/2018
ms.locfileid: "39056234"
---
# <a name="tutorial-azure-active-directory-integration-with-vidyard"></a>Öğretici: Azure Active Directory Vidyard ile tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile Vidyard tümleştirme konusunda bilgi edinin.

Azure AD ile Vidyard tümleştirme ile aşağıdaki avantajları sağlar:

- Vidyard erişimi, Azure AD'de kontrol edebilirsiniz.
- Otomatik olarak imzalanan için Vidyard (çoklu oturum açma) ile Azure AD hesaplarına açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile Vidyard yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliğiniz
- Abonelik Vidyard çoklu oturum açma etkin

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, şunları yapabilirsiniz [bir aylık deneme sürümü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden Vidyard ekleme
2. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-vidyard-from-the-gallery"></a>Galeriden Vidyard ekleme
Azure AD'de Vidyard tümleştirmesini yapılandırmak için Vidyard Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden Vidyard eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **Vidyard**seçin **Vidyard** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Sonuç listesinde Vidyard](./media/vidyard-tutorial/tutorial_vidyard_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırın ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı Vidyard sınayın.

Tek iş için oturum açma için Azure AD ne Vidyard karşılığı kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısının Vidyard ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma Vidyard ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Vidyard test kullanıcısı oluşturma](#create-a-vidyard-test-user)**  - kullanıcı Azure AD gösterimini bağlı Vidyard Britta simon'un bir karşılığı vardır.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve Vidyard uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma ile Vidyard yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **Vidyard** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/vidyard-tutorial/tutorial_vidyard_samlbase.png)

3. Üzerinde **Vidyard etki alanı ve URL'ler** bölümünde, uygulamada yapılandırmak istiyorsanız aşağıdaki adımları gerçekleştirin **IDP** başlatılan modu:

    ![Vidyard etki alanı ve URL'ler tek oturum açma bilgileri](./media/vidyard-tutorial/tutorial_vidyard_url2.png)

    a. İçinde **tanımlayıcı** metin kutusuna bir URL şu biçimi kullanarak: `https://secure.vidyard.com/sso/saml/<unique id>/metadata`

    b. İçinde **yanıt URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://secure.vidyard.com/sso/saml/<unique id>/consume`

4. Denetleme **Gelişmiş URL ayarlarını göster** ve uygulamada yapılandırmak istiyorsanız, aşağıdaki adımı uygulayın **SP** başlatılan modu:

    ![Vidyard etki alanı ve URL'ler tek oturum açma bilgileri](./media/vidyard-tutorial/tutorial_vidyard_url1.png)

    İçinde **oturum açma URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://secure.vidyard.com/sso/saml/<unique id>/login`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerler gerçek tanımlayıcısı, yanıt URL'si ve oturum açma, öğreticinin ilerleyen bölümlerinde açıklanan URL ile güncelleştirir

1. Üzerinde **SAML imzalama sertifikası** bölümünde **Certificate(Base64)** ve bilgisayarınızdaki sertifika dosyasını kaydedin.

    ![Sertifika indirme bağlantısı](./media/vidyard-tutorial/tutorial_vidyard_certificate.png) 

6. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açma Kaydet düğmesi yapılandırın](./media/vidyard-tutorial/tutorial_general_400.png)

7. Üzerinde **Vidyard yapılandırma** bölümünde **yapılandırma Vidyard** açmak için **yapılandırma oturum açma** penceresi. Kopyalama **SAML çoklu oturum açma hizmeti URL'si** gelen **hızlı başvuru bölümü.**

    ![Vidyard yapılandırma](./media/vidyard-tutorial/tutorial_vidyard_configure.png)

8. Farklı bir web tarayıcı penceresinde Vidyard yazılım şirketinizin sitesi için bir yönetici olarak oturum açın.

9. Vidyard Panoda **grubu** > **güvenlik**

    ![Vidyard yapılandırma](./media/vidyard-tutorial/configure1.png)

1. Tıklayın **yeni profili** sekmesi.

    ![Vidyard yapılandırma](./media/vidyard-tutorial/configure2.png)

11. İçinde **SAML yapılandırma** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Vidyard yapılandırma](./media/vidyard-tutorial/configure3.png)

    a. Lütfen genel profil adı girin **profil adı** metin.

    b. Kopyalama **SSO kullanıcı oturum açma sayfasına** yapıştırın ve değer **oturum açma URL'si** metin kutusunda **Vidyard etki alanı ve URL'ler bölüm** Azure portalında.

    c. Kopyalama **ACS URL** yapıştırın ve değer **yanıt URL'si** metin kutusunda **Vidyard etki alanı ve URL'ler bölüm** Azure portalında.

    d. Kopyalama **veren/meta veri URL'si** yapıştırın ve değer **tanımlayıcı** metin kutusunda **Vidyard etki alanı ve URL'ler bölüm** Azure portalında.

    e. Not Defteri'nde Azure portalından indirilen sertifika dosyanızı açın ve ardından yapıştırın **X.509 sertifikası** metin.

    f. İçinde **SAML uç nokta URL'si** metin değerini yapıştırın **SAML çoklu oturum açma hizmeti URL'si** Azure portaldan kopyaladığınız.

    g. Tıklayın **onaylayın**.

12. Çoklu oturum açma sekmesinden seçin **atama** yanındaki mevcut bir profili

    ![Vidyard yapılandırma](./media/vidyard-tutorial/configure4.png)

    > [!NOTE]
    > SSO profili oluşturduktan sonra kullanıcılar Azure üzerinden erişim gerektiren herhangi bir gruplarına atayın. Kullanıcı Grup içinde atanmış olan mevcut değilse Vidyard otomatik olarak bir kullanıcı hesabı oluşturun ve gerçek zamanlı rollerine atayın.

13. Görünür olan Kuruluş grubunuzu seçin **kullanılabilir gruplar atanacak**.

    ![Vidyard yapılandırma](./media/vidyard-tutorial/configure5.png)

14. Atanan gruplar altında gördüğünüz **şu an atanan grup**. Bir rol için grubu tıklatın ve kuruluş göre seçin **Onayla**.

    ![Vidyard yapılandırma](./media/vidyard-tutorial/configure6.png)

    > [!NOTE]
    > Daha fazla bilgi için [bu belge](https://knowledge.vidyard.com/saml-single-sign-on-authentication/saml-based-single-sign-on-sso-in-vidyard).

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, sol bölmede, tıklayın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/vidyard-tutorial/create_aaduser_01.png)

2. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](./media/vidyard-tutorial/create_aaduser_02.png)

3. Açmak için **kullanıcı** iletişim kutusu, tıklayın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/vidyard-tutorial/create_aaduser_03.png)

4. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/vidyard-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** Britta Simon kullanıcı e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değeri yazın **parola** kutusu.

    d. **Oluştur**’a tıklayın.
 
### <a name="create-a-vidyard-test-user"></a>Vidyard test kullanıcısı oluşturma

Bu bölümün amacı Vidyard Britta Simon adlı bir kullanıcı oluşturmaktır. Vidyard tam zamanında sağlama, varsayılan olarak etkin olan destekler. Bu bölümde, hiçbir eylem öğesini yoktur. Yeni bir kullanıcı, henüz yoksa Vidyard erişme denemesi sırasında oluşturulur.
>[!Note]
>Bir kullanıcı el ile oluşturmanız gerekiyorsa, kişi [Vidyard Destek ekibine](mailto:support@vidyard.com).

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Azure çoklu oturum açma kullanmak için Vidyard erişim vererek Britta Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**Britta Simon Vidyard için atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

2. Uygulamalar listesinde **Vidyard**.

    ![Uygulamalar listesinde Vidyard bağlantı](./media/vidyard-tutorial/tutorial_vidyard_app.png)  

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı][202]

4. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde Vidyard kutucuğa tıkladığınızda, otomatik olarak Vidyard uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/vidyard-tutorial/tutorial_general_01.png
[2]: ./media/vidyard-tutorial/tutorial_general_02.png
[3]: ./media/vidyard-tutorial/tutorial_general_03.png
[4]: ./media/vidyard-tutorial/tutorial_general_04.png

[100]: ./media/vidyard-tutorial/tutorial_general_100.png

[200]: ./media/vidyard-tutorial/tutorial_general_200.png
[201]: ./media/vidyard-tutorial/tutorial_general_201.png
[202]: ./media/vidyard-tutorial/tutorial_general_202.png
[203]: ./media/vidyard-tutorial/tutorial_general_203.png

