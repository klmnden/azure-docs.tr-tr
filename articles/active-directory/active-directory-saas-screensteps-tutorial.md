---
title: 'Öğretici: Azure Active Directory Tümleştirme ile ScreenSteps | Microsoft Docs'
description: Çoklu oturum açma Azure Active Directory ile ScreenSteps arasında yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: 4563fe94-a88f-4895-a07f-79df44889cf9
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/14/2017
ms.author: jeedes
ms.openlocfilehash: 444bc2d0724f8e7efc453c9efa577908822a4df2
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2018
ms.locfileid: "34352557"
---
# <a name="tutorial-azure-active-directory-integration-with-screensteps"></a>Öğretici: Azure Active Directory Tümleştirme ScreenSteps ile

Bu öğreticide, Azure Active Directory (Azure AD) ile ScreenSteps tümleştirmek öğrenin.

ScreenSteps Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- ScreenSteps erişimi, Azure AD'de kontrol edebilirsiniz.
- Otomatik olarak için ScreenSteps (çoklu oturum açma) ile Azure AD hesaplarına açan kullanıcılarınıza etkinleştirebilirsiniz.
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirme ScreenSteps ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir ScreenSteps çoklu oturum açma abonelik etkin

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, şunları yapabilirsiniz [bir aylık deneme sürümünü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden ScreenSteps ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-screensteps-from-the-gallery"></a>Galeriden ScreenSteps ekleme
Azure AD ScreenSteps tümleştirilmesi yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden ScreenSteps eklemeniz gerekir.

**Galeriden ScreenSteps eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **ScreenSteps**seçin **ScreenSteps** sonuç panelinden ardından **Ekle** uygulama eklemek için düğmeyi.

    ![Sonuçlar listesinde ScreenSteps](./media/active-directory-saas-screensteps-tutorial/tutorial_screensteps_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı ScreenSteps sınayın.

Tekli çalışmaya oturum için Azure AD ScreenSteps karşılık gelen kullanıcı için bir kullanıcı Azure AD'de nedir bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının ScreenSteps ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

ScreenSteps içinde değerini atayın **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** bağlantı ilişkisi oluşturmak için.

Yapılandırma ve Azure AD çoklu oturum açma ScreenSteps ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[ScreenSteps test kullanıcısı oluşturma](#create-a-screensteps-test-user)**  - Britta Simon, karşılık gelen kullanıcı Azure AD gösterimini bağlı ScreenSteps sağlamak için.
4. **[Azure AD test kullanıcısı atayın](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Test çoklu oturum açma](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma ScreenSteps uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma ile ScreenSteps yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **ScreenSteps** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/active-directory-saas-screensteps-tutorial/tutorial_screensteps_samlbase.png)

3. Üzerinde **ScreenSteps etki alanı ve URL'leri** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![ScreenSteps etki alanı ve URL'leri tek oturum açma bilgileri](./media/active-directory-saas-screensteps-tutorial/tutorial_screensteps_url.png)

    İçinde **oturum açma URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://<tenantname>.ScreenSteps.com`

    > [!NOTE] 
    > Bu değer gerçek değil. Bu değer, gerçek oturum açma, bu öğreticinin ilerleyen bölümlerinde açıklanan URL ile güncelleştirin. 

4. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **Certificate(Base64)** ve sertifika dosyayı bilgisayarınıza kaydedin.

    ![Sertifika indirme bağlantısı](./media/active-directory-saas-screensteps-tutorial/tutorial_screensteps_certificate.png) 

5. Tıklatın **kaydetmek** düğmesi.

    ![Oturum açma tek Kaydet düğmesi yapılandırın](./media/active-directory-saas-screensteps-tutorial/tutorial_general_400.png)

6. Üzerinde **ScreenSteps yapılandırma** 'yi tıklatın **yapılandırma ScreenSteps** açmak için **yapılandırma oturum açma** penceresi. Kopya **Sign-Out URL ve SAML çoklu oturum açma hizmet URL'si** gelen **hızlı başvuru bölümü.**

    ![ScreenSteps yapılandırma](./media/active-directory-saas-screensteps-tutorial/tutorial_screensteps_configure.png) 

7. Farklı web tarayıcısı penceresinde ScreenSteps şirket sitenize yönetici olarak oturum açın.

8. Tıklatın **hesap ayarları**.

    ![Hesap Yönetimi](./media/active-directory-saas-screensteps-tutorial/ic778523.png "hesap yönetimi")

9. Tıklatın **çoklu oturum açma**.

    ![Uzaktan kimlik doğrulama](./media/active-directory-saas-screensteps-tutorial/ic778524.png "uzaktan kimlik doğrulama")

10. Tıklatın **tek oturum açma uç noktası oluşturma**.

    ![Uzaktan kimlik doğrulama](./media/active-directory-saas-screensteps-tutorial/ic778525.png "uzaktan kimlik doğrulama")

11. İçinde **oluşturmak tek oturum açma uç noktası** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Bir kimlik doğrulama uç noktası oluşturma](./media/active-directory-saas-screensteps-tutorial/ic778526.png "bir kimlik doğrulama uç noktası oluşturma")
    
    a. İçinde **başlık** metin kutusuna, bir başlık yazın.
    
    b. Gelen **modu** listesinde **SAML**.
    
    c. **Oluştur**’a tıklayın.

12. **Düzen** yeni uç noktası.

    ![Uç noktayı Düzenle](./media/active-directory-saas-screensteps-tutorial/ic778528.png "Düzenle uç noktası")

13. İçinde **Düzenle tek oturum açma uç noktası** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Uzaktan kimlik doğrulama uç noktası](./media/active-directory-saas-screensteps-tutorial/ic778527.png "uzaktan kimlik doğrulama uç noktası")

    a. Tıklatın **yeni SAML sertifika dosyası karşıya yükleme**ve Azure portalından yüklediğiniz sertifikayı karşıya yükleyin.
    
    b. Yapıştır **SAML çoklu oturum açma hizmet URL'si** Azure portalından kopyaladığınız değeri **uzaktan oturum açma URL'si** metin kutusu.
    
    c. Yapıştır **Sign-Out URL** Azure portalından kopyaladığınız değeri **oturum kapatma URL'si** metin kutusu.
    
    d. Seçin bir **grup** olduğunda bunlar sağlanan için kullanıcılara atamak için.
    
    e. Tıklatın **güncelleştirme**.

    f. Kopya **SAML tüketici URL** Pano ve yapıştırma için **oturum açma URL'si** metin kutusuna **ScreenSteps etki alanı ve URL'leri** bölümü.
    
    g. Geri dönüp **Düzenle tek oturum açma uç noktası**.
    
    h. ' I tıklatın **olun hesabı için varsayılan** düğmesini ScreenSteps oturum tüm kullanıcılar için bu uç noktası kullan. Alternatif olarak tıklayabilirsiniz **eklemek için Site** belirli sitelerin Bu uç noktayı kullanmak için düğmesini **ScreenSteps**.

> [!TIP]
> Şimdi bu yönergeleri içinde kısa bir sürümünü okuyabilirsiniz [Azure portal](https://portal.azure.com)uygulaması kuruluyor yaparken!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** sekmesinde ve aracılığıyla katıştırılmış belgelere erişebilir **yapılandırma** alt bölüm. Daha fazla bilgiyi burada embedded belgeler özelliği hakkında: [Azure AD embedded belgeler]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Sol bölmede, Azure portal'ı tıklatın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/active-directory-saas-screensteps-tutorial/create_aaduser_01.png)

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantılar](./media/active-directory-saas-screensteps-tutorial/create_aaduser_02.png)

3. Açmak için **kullanıcı** iletişim kutusu, tıklatın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/active-directory-saas-screensteps-tutorial/create_aaduser_03.png)

4. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/active-directory-saas-screensteps-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** kullanıcı Britta Simon e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değer aşağı yazma **parola** kutusu.

    d. **Oluştur**’a tıklayın.
 
### <a name="create-a-screensteps-test-user"></a>ScreenSteps test kullanıcısı oluşturma

Bu bölümde, ScreenSteps içinde Britta Simon adlı bir kullanıcı oluşturun. Çalışmak [ScreenSteps istemci destek ekibi](https://www.screensteps.com/contact) ScreenSteps platform kullanıcıları eklemek için. Kullanıcıların oluşturulan ve çoklu oturum açma kullanmadan önce etkinleştirilmelidir. 

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Britta ScreenSteps için erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**ScreenSteps için Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **ScreenSteps**.

    ![Uygulamalar listesinde ScreenSteps bağlantı](./media/active-directory-saas-screensteps-tutorial/tutorial_screensteps_app.png)  

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantı][202]

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Ekleme atama bölmesi][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli ScreenSteps parçasında tıklattığınızda, otomatik olarak ScreenSteps uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](active-directory-saas-tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/active-directory-saas-screensteps-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-screensteps-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-screensteps-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-screensteps-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-screensteps-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-screensteps-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-screensteps-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-screensteps-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-screensteps-tutorial/tutorial_general_203.png

