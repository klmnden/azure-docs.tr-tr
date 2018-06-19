---
title: 'Öğretici: Azure Active Directory Tümleştirme ile Vidyard | Microsoft Docs'
description: Çoklu oturum açma Azure Active Directory ile Vidyard arasında yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: bed7df23-6e13-4e7c-b4cc-53ed4804664d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/22/2018
ms.author: jeedes
ms.openlocfilehash: 1ad4259ef0e0ca948d677944855ce732b3de98fd
ms.sourcegitcommit: c851842d113a7078c378d78d94fea8ff5948c337
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2018
ms.locfileid: "35978307"
---
# <a name="tutorial-azure-active-directory-integration-with-vidyard"></a>Öğretici: Azure Active Directory Tümleştirme Vidyard ile

Bu öğreticide, Azure Active Directory (Azure AD) ile Vidyard tümleştirmek öğrenin.

Vidyard Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Vidyard erişimi, Azure AD'de kontrol edebilirsiniz.
- Otomatik olarak için Vidyard (çoklu oturum açma) ile Azure AD hesaplarına açan kullanıcılarınıza etkinleştirebilirsiniz.
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirme Vidyard ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir Vidyard çoklu oturum açma abonelik etkin

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, şunları yapabilirsiniz [bir aylık deneme sürümünü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden Vidyard ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-vidyard-from-the-gallery"></a>Galeriden Vidyard ekleme
Azure AD Vidyard tümleştirilmesi yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden Vidyard eklemeniz gerekir.

**Galeriden Vidyard eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **Vidyard**seçin **Vidyard** sonuç panelinden ardından **Ekle** uygulama eklemek için düğmeyi.

    ![Sonuçlar listesinde Vidyard](./media/vidyard-tutorial/tutorial_vidyard_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı Vidyard sınayın.

Tekli çalışmaya oturum için Azure AD Vidyard karşılık gelen kullanıcı için bir kullanıcı Azure AD'de nedir bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının Vidyard ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma Vidyard ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Vidyard test kullanıcısı oluşturma](#create-a-vidyard-test-user)**  - Britta Simon, karşılık gelen kullanıcı Azure AD gösterimini bağlı Vidyard sağlamak için.
4. **[Azure AD test kullanıcısı atayın](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Test çoklu oturum açma](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma Vidyard uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma ile Vidyard yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **Vidyard** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/vidyard-tutorial/tutorial_vidyard_samlbase.png)

3. Üzerinde **Vidyard etki alanı ve URL'leri** bölümünde, uygulamada yapılandırmak istiyorsanız aşağıdaki adımları gerçekleştirin **IDP** modu tarafından başlatılan:

    ![Vidyard etki alanı ve URL'leri tek oturum açma bilgileri](./media/vidyard-tutorial/tutorial_vidyard_url2.png)

    a. İçinde **tanımlayıcısı** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://secure.vidyard.com/sso/saml/<unique id>/metadata`

    b. İçinde **yanıt URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://secure.vidyard.com/sso/saml/<unique id>/consume`

4. Denetleme **Göster Gelişmiş URL ayarları** ve uygulamada yapılandırmak istiyorsanız aşağıdaki adımı gerçekleştirin **SP** modunda başlatılan:

    ![Vidyard etki alanı ve URL'leri tek oturum açma bilgileri](./media/vidyard-tutorial/tutorial_vidyard_url1.png)

    İçinde **oturum açma URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://secure.vidyard.com/sso/saml/<unique id>/login`

    > [!NOTE]
    > Bu değerler gerçek değildir. Gerçek tanımlayıcısı, yanıt URL'si ve oturum açma, öğreticide daha sonra açıklanan URL ile bu değerleri güncelleştirmek

1. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **Certificate(Base64)** ve sertifika dosyayı bilgisayarınıza kaydedin.

    ![Sertifika indirme bağlantısı](./media/vidyard-tutorial/tutorial_vidyard_certificate.png) 

6. Tıklatın **kaydetmek** düğmesi.

    ![Oturum açma tek Kaydet düğmesi yapılandırın](./media/vidyard-tutorial/tutorial_general_400.png)

7. Üzerinde **Vidyard yapılandırma** 'yi tıklatın **yapılandırma Vidyard** açmak için **yapılandırma oturum açma** penceresi. Kopya **SAML çoklu oturum açma hizmet URL'si** gelen **hızlı başvuru bölümü.**

    ![Vidyard yapılandırma](./media/vidyard-tutorial/tutorial_vidyard_configure.png)

8. Farklı web tarayıcısı penceresinde Vidyard yazılım şirket sitenize yönetici olarak oturum açın.

9. Vidyard panodan seçin **grup** > **güvenlik**

    ![Vidyard yapılandırma](./media/vidyard-tutorial/configure1.png)

1. Tıklatın **yeni bir profil** sekmesi.

    ![Vidyard yapılandırma](./media/vidyard-tutorial/configure2.png)

11. İçinde **SAML Yapılandırması** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Vidyard yapılandırma](./media/vidyard-tutorial/configure3.png)

    a. Lütfen genel profil adı girin **profil adı** metin kutusu.

    b. Kopya **SSO kullanıcı oturum açma sayfası** değer ve yapıştırın **URL üzerinde oturum** metin kutusuna **Vidyard etki alanı ve URL'leri bölümüne** Azure Portal'daki.

    c. Kopya **ACS URL** değer ve yapıştırın **yanıt URL'si** metin kutusuna **Vidyard etki alanı ve URL'leri bölümüne** Azure Portal'daki.

    d. Kopya **veren/meta veri URL'sini** değer ve yapıştırın **tanımlayıcısı** metin kutusuna **Vidyard etki alanı ve URL'leri bölümüne** Azure Portal'daki.

    e. Not Defteri'nde Azure Portalı'ndan indirilen sertifika dosyanızı açın ve ardından yapıştırın **X.509 sertifikası** metin kutusu.

    f. İçinde **SAML uç nokta URL'si** metin kutusuna, değerini yapıştırın **SAML çoklu oturum açma hizmet URL'si** Azure portalından kopyalanır.

    g. Tıklatın **onaylayın**.

12. Çoklu oturum açma sekmesinden seçin **atamak** bir profil yanındaki

    ![Vidyard yapılandırma](./media/vidyard-tutorial/configure4.png)

    > [!NOTE]
    > SSO profili oluşturduktan sonra kullanıcılar Azure üzerinden erişim gerektirecek gruplara atayın. Kullanıcı atanmış olan Grup içinde mevcut değilse Vidyard otomatik olarak bir kullanıcı hesabı oluşturun ve gerçek zamanlı rolleri atayın.

13. Görünür kuruluş grubunuzu seçin **bölgeyle atanacak**.

    ![Vidyard yapılandırma](./media/vidyard-tutorial/configure5.png)

14. Atanan gruplar altında görebilirsiniz **şu anda atanmış grup**. Kuruluş ve tıklatın göredir grup için bir rol seçin **Onayla**.

    ![Vidyard yapılandırma](./media/vidyard-tutorial/configure6.png)

    > [!NOTE]
    > Daha fazla bilgi için bkz [bu belge](https://knowledge.vidyard.com/saml-single-sign-on-authentication/saml-based-single-sign-on-sso-in-vidyard).

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Sol bölmede, Azure portal'ı tıklatın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/vidyard-tutorial/create_aaduser_01.png)

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantılar](./media/vidyard-tutorial/create_aaduser_02.png)

3. Açmak için **kullanıcı** iletişim kutusu, tıklatın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/vidyard-tutorial/create_aaduser_03.png)

4. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/vidyard-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** kullanıcı Britta Simon e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değer aşağı yazma **parola** kutusu.

    d. **Oluştur**’a tıklayın.
 
### <a name="create-a-vidyard-test-user"></a>Vidyard test kullanıcısı oluşturma

Bu bölümün amacı Britta Simon içinde Vidyard adlı bir kullanıcı oluşturmaktır. Vidyard yalnızca zaman sağlama, varsayılan olarak etkin olduğu destekler. Bu bölümde, eylem öğe yok. Yeni bir kullanıcı henüz yoksa Vidyard erişme denemesi sırasında oluşturulur.
>[!Note]
>Bir kullanıcı el ile oluşturmanız gerekiyorsa, kişi [Vidyard destek ekibi](mailto:support@vidyard.com).

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Britta Vidyard için erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**Vidyard için Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **Vidyard**.

    ![Uygulamalar listesinde Vidyard bağlantı](./media/vidyard-tutorial/tutorial_vidyard_app.png)  

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantı][202]

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Ekleme atama bölmesi][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli Vidyard parçasında tıklattığınızda, otomatik olarak Vidyard uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](../manage-apps/what-is-single-sign-on.md)

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

