---
title: 'Öğretici: Azure Active Directory Tümleştirme ile Temizle gözden geçirme | Microsoft Docs'
description: Çoklu oturum açma Azure Active Directory Temizle gözden geçirme arasındaki yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 8264159a-11a2-4a8c-8285-4efea0adac8c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/12/2018
ms.author: jeedes
ms.openlocfilehash: aae5504e593902768ba6e9576a09f3cff1911eb6
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2018
ms.locfileid: "34341806"
---
# <a name="tutorial-azure-active-directory-integration-with-clear-review"></a>Öğretici: Azure Active Directory Tümleştirme ile Temizle gözden geçirme

Bu öğreticide, Temizle gözden geçirme Azure Active Directory (Azure AD) ile tümleştirme öğrenin.

Clear gözden geçirme Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Clear gözden geçirme erişimi, Azure AD'de kontrol edebilirsiniz.
- Azure AD hesaplarına otomatik olarak Temizle incelemeye (çoklu oturum açma) açan kullanıcılarınıza etkinleştirebilirsiniz.
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirme Temizle gözden geçirme ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir açık gözden geçirme çoklu oturum açma abonelik etkin

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, şunları yapabilirsiniz [bir aylık deneme sürümünü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden Temizle gözden geçirme ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-clear-review-from-the-gallery"></a>Galeriden Temizle gözden geçirme ekleme
Azure AD Temizle gözden geçirme tümleştirilmesi yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden Temizle gözden geçirme eklemeniz gerekir.

**Galeriden Temizle yorum eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **Temizle gözden geçirme**seçin **Temizle gözden geçirme** sonuç panelinden ardından **Ekle** uygulama eklemek için düğmeyi.

    ![Sonuçlar listesinde Temizle gözden geçirme](./media/active-directory-saas-clearreview-tutorial/tutorial_clearreview_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma Temizle "Britta Simon" adlı bir test kullanıcı tabanlı gözden geçirme ile test etme.

Tekli çalışmaya oturum için Azure AD Temizle gözden geçirme karşılık gelen kullanıcı için bir kullanıcı Azure AD'de nedir bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının Temizle gözden geçirme ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Değerini Temizle incelemede atamak **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** bağlantı ilişkisi oluşturmak için.

Yapılandırma ve Azure AD çoklu oturum açma Temizle gözden geçirme ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Clear gözden geçirme test kullanıcısı oluşturma](#create-a-clear-review-test-user)**  - Britta Simon, karşılık gelen kullanıcı Azure AD gösterimini bağlı Temizle gözden geçirme sağlamak için.
4. **[Azure AD test kullanıcısı atayın](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Test çoklu oturum açma](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma Temizle gözden geçirme uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma Temizle gözden geçirme ile yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **Temizle gözden geçirme** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/active-directory-saas-clearreview-tutorial/tutorial_clearreview_samlbase.png)

3. Üzerinde **Temizle gözden geçirme etki alanı ve URL'leri** bölümünde, uygulamada yapılandırmak istiyorsanız aşağıdaki adımları gerçekleştirin **başlatılan IDP** modu:

    ![NET gözden geçirme etki alanı ve URL'leri tek oturum açma bilgileri](./media/active-directory-saas-clearreview-tutorial/tutorial_clearreview_url.png)

    a. İçinde **tanımlayıcısı** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://<customer name>.clearreview.com/sso/metadata/`

    b. İçinde **yanıt URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://<customer name>.clearreview.com/sso/acs/`

4. Denetleme **Göster Gelişmiş URL ayarları** ve uygulamada yapılandırmak istiyorsanız aşağıdaki adımı gerçekleştirin **SP** modunda başlatılan:

    ![NET gözden geçirme etki alanı ve URL'leri tek oturum açma bilgileri](./media/active-directory-saas-clearreview-tutorial/tutorial_clearreview_url_sp.png)

    İçinde **oturum açma URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın:`https://<customer name>.clearreview.com`

    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler, gerçek oturum açma URL'si, tanımlayıcı ve yanıt URL'si ile güncelleştirin. Kişi [Temizle gözden geçirme destek ekibi](https://clearreview.com/contact/) bu değerleri almak için.

5. Clear gözden geçirme uygulama adı tanımlayıcısı talep kümesinde benzersiz kullanıcı kimliği değeri bekler. Kullanıcı tanımlayıcısı değerine eşlemelisiniz **user.mail**.

    ![Öznitelik bölümü](./media/active-directory-saas-clearreview-tutorial/attribute.png)


6. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **sertifika (Base64)** ve sertifika dosyayı bilgisayarınıza kaydedin.

    ![Sertifika indirme bağlantısı](./media/active-directory-saas-clearreview-tutorial/tutorial_clearreview_certificate.png)

7. Tıklatın **kaydetmek** düğmesi.

    ![Oturum açma tek Kaydet düğmesi yapılandırın](./media/active-directory-saas-clearreview-tutorial/tutorial_general_400.png)

8. Üzerinde **gözden geçirme yapılandırmayı Temizle** 'yi tıklatın **yapılandırma Temizle gözden** açmak için **yapılandırma oturum açma** penceresi. Kopya **Sign-Out URL, SAML varlık kimliği ve SAML çoklu oturum açma hizmet URL'si** gelen **hızlı başvuru bölümü.**

    ![Yapılandırmayı Temizle gözden geçir](./media/active-directory-saas-clearreview-tutorial/tutorial_clearreview_configure.png) 

9. Çoklu oturum açma yapılandırmak için **Temizle gözden geçirme** yan, açık **Temizle gözden geçirme** yönetici kimlik bilgileriyle portal.

10. Seçin **yönetici** sol gezinti gelen.

    ![Oturum açma tek Kaydet düğmesi yapılandırın](./media/active-directory-saas-clearreview-tutorial/tutorial_clearreview_app_admin1.png)

11. Seçin **değişiklik** sayfanın sonundaki.

    ![Oturum açma tek Kaydet düğmesi yapılandırın](./media/active-directory-saas-clearreview-tutorial/tutorial_clearreview_app_admin2.png)

12. Şu adımları gerçekleştirin **çoklu oturum açma ayarları** sayfası

    ![Oturum açma tek Kaydet düğmesi yapılandırın](./media/active-directory-saas-clearreview-tutorial/tutorial_clearreview_app_admin3.png)

    a. İçinde **veren URL'si** metin değerini yapıştırın **SAML varlık kimliği** Azure portalından kopyalanan.

    b. İçinde **SAML Endpoint** metin değerini yapıştırın **SAML çoklu oturum açma hizmet URL'si** Azure portalından kopyalanan.    

    c. İçinde **SLO Endpoint** metin değerini yapıştırın **oturum açma hizmet URL'si** Azure portalından kopyalanan. 

    d. İndirilen sertifika Not Defteri'nde açın ve içeriği yapıştırmak **X.509 sertifikası** metin kutusu.   

13. **Kaydet**’e tıklayın.

> [!TIP]
> Şimdi bu yönergeleri içinde kısa bir sürümünü okuyabilirsiniz [Azure portal](https://portal.azure.com)uygulaması kuruluyor yaparken!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** sekmesinde ve aracılığıyla katıştırılmış belgelere erişebilir **yapılandırma** alt bölüm. Daha fazla bilgiyi burada embedded belgeler özelliği hakkında: [Azure AD embedded belgeler]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Sol bölmede, Azure portal'ı tıklatın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/active-directory-saas-clearreview-tutorial/create_aaduser_01.png)

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantılar](./media/active-directory-saas-clearreview-tutorial/create_aaduser_02.png)

3. Açmak için **kullanıcı** iletişim kutusu, tıklatın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/active-directory-saas-clearreview-tutorial/create_aaduser_03.png)

4. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/active-directory-saas-clearreview-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** kullanıcı Britta Simon e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değer aşağı yazma **parola** kutusu.

    d. **Oluştur**’a tıklayın.
  
### <a name="create-a-clear-review-test-user"></a>Clear gözden geçirme test kullanıcısı oluşturma

Bu bölümde, Britta Simon Temizle incelemede adlı bir kullanıcı oluşturun. Lütfen çalışmak [Temizle gözden geçirme destek ekibi](https://clearreview.com/contact/) Temizle gözden geçirme platform kullanıcıları eklemek için.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Britta Temizle gözden geçirme için erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**Temizle incelemeye Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **Temizle gözden**.

    ![Uygulamalar listesinde Temizle gözden geçirme bağlantı](./media/active-directory-saas-clearreview-tutorial/tutorial_clearreview_app.png)  

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantı][202]

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Ekleme atama bölmesi][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli Temizle gözden geçirme parçasında tıklattığınızda, otomatik olarak Temizle gözden geçirme uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](active-directory-saas-tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/active-directory-saas-clearreview-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-clearreview-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-clearreview-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-clearreview-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-clearreview-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-clearreview-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-clearreview-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-clearreview-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-clearreview-tutorial/tutorial_general_203.png
