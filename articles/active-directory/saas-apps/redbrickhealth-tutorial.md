---
title: 'Öğretici: Azure Active Directory Tümleştirme RedBrick sistem durumu ile | Microsoft Docs'
description: Çoklu oturum açma Azure Active Directory ve RedBrick sistem durumu arasında yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 26290c65-9aa3-42ab-8ba5-901b14dc8e73
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/22/2018
ms.author: jeedes
ms.openlocfilehash: c30da2ecebd7c46e8396351f7e7c5ee69c130e29
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/19/2018
ms.locfileid: "36217043"
---
# <a name="tutorial-azure-active-directory-integration-with-redbrick-health"></a>Öğretici: Azure Active Directory Tümleştirme ile RedBrick sistem durumu

Bu öğreticide, Azure Active Directory (Azure AD) ile RedBrick sistem durumu tümleştirmek öğrenin.

RedBrick sistem durumu Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- RedBrick sistem erişimi, Azure AD'de kontrol edebilirsiniz.
- Azure AD hesaplarına otomatik olarak RedBrick sağlık (çoklu oturum açma) açan kullanıcılarınıza etkinleştirebilirsiniz.
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirme RedBrick Health ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir RedBrick sistem durumu çoklu oturum açma abonelik etkin

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, şunları yapabilirsiniz [bir aylık deneme sürümünü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden RedBrick sistem durumu ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-redbrick-health-from-the-gallery"></a>Galeriden RedBrick sistem durumu ekleme
Azure AD RedBrick sistem durumu tümleştirilmesi yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden RedBrick sistem durumu eklemeniz gerekir.

**Galeriden RedBrick sistem durumu eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **RedBrick sistem durumu**seçin **RedBrick sistem durumu** sonuç panelinden ardından **Ekle** uygulama eklemek için düğmeyi.

    ![Sonuçlar listesinde redBrick sistem durumu](./media/redbrickhealth-tutorial/tutorial_redbrickhealth_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı RedBrick durumu ile test etme.

Tekli çalışmaya oturum için Azure AD RedBrick sistem durumu karşılık gelen kullanıcı için bir kullanıcı Azure AD'de nedir bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının RedBrick sistem durumu ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

RedBrick durumu değeri atamak **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** bağlantı ilişkisi oluşturmak için.

Yapılandırma ve Azure AD çoklu oturum açma RedBrick sistem durumu ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[RedBrick sistem durumu test kullanıcısı oluşturma](#create-a-redbrick-health-test-user)**  - Britta Simon, karşılık gelen kullanıcı Azure AD gösterimini bağlı RedBrick sistem durumu sağlamak için.
4. **[Azure AD test kullanıcısı atayın](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Test çoklu oturum açma](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma RedBrick sistem durumu uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma RedBrick Health ile yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **RedBrick sistem durumu** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/redbrickhealth-tutorial/tutorial_redbrickhealth_samlbase.png)

3. Üzerinde **RedBrick sistem durumu etki alanı ve URL'leri** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Oturum açma bilgileri redBrick sistem durumu etki alanı ve URL'leri tek](./media/redbrickhealth-tutorial/tutorial_redbrickhealth_url.png)

    a. İçinde **tanımlayıcısı** metin kutusuna, bir URL yazın: `http://www.redbrickhealth.com`
    
    b. İçinde **yanıt URL'si** metin kutusuna, bir URL yazın: `https://sso-intg.redbrickhealth.com/sp/ACS.saml2`
    
    Üretim ortamı için: `https://sso.redbrickhealth.com/sp/ACS.saml2`

    c. Tıklatın **Göster Gelişmiş URL ayarları**.
    
    ![Oturum açma bilgileri redBrick sistem durumu etki alanı ve URL'leri tek](./media/redbrickhealth-tutorial/tutorial_redbrickhealth_url1.png)

    d. İçinde **geçiş durumunu** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://api-sso2.redbricktest.com/identity/sso/nbound?target=https://vanity9-sso2.redbrickdev.com/portal&connection=<companyname>conn1`
    
    > [!NOTE] 
    > Geçiş durumu değeri gerçek değil. Bu değer ile gerçek geçiş durumunu güncelleştirin. Kişi [RedBrick sistem durumu destek ekibi](https://home.redbrickhealth.com/contact/) bu değeri alınamıyor.

4. RedBrick sistem uygulaması SAML onaylar SAML belirteci öznitelikleri yapılandırmanıza özel öznitelik eşlemelerini ekleyin gerektiren belirli bir biçimde bekliyor. Bu talep belirli müşterisiyseniz ve gereksinimi temel bağlıdır. Aşağıdaki isteğe bağlı taleplerdir örneği yalnızca, uygulamanız için yapılandırabilirsiniz. Bu öznitelik değerlerini yönetebilirsiniz **kullanıcı öznitelikleri** uygulama tümleştirmesi sayfasında bölüm.

    ![Çoklu oturum açmayı yapılandırın](./media/redbrickhealth-tutorial/attribute.png)

5. İçinde **kullanıcı öznitelikleri** bölümünde **çoklu oturum açma** iletişim kutusunda, yukarıdaki resimde gösterildiği gibi SAML belirteci özniteliği yapılandırın ve aşağıdaki adımları gerçekleştirin:

    | Öznitelik Adı | Öznitelik Değeri |
    | ---------------| ----------------|
    | asıl adı | ********** |
    | istemci kimliği | ********** |
    | katılımcı kimliği | ********** |
    
    > [!NOTE]
    > Bu değerler yalnızca başvuru amaç için geçerlidir. Kuruluşunuzun gereksinim göredir öznitelikleri tanımlamanız gerekir. Temasa [RedBrick sistem durumu destek ekibi](https://home.redbrickhealth.com/contact/) gerekli talepler hakkında daha fazla bilgi almak için.
    
    a. Tıklatın **Ekle özniteliği** açmak için **özniteliği eklemek** iletişim.
    
    ![Çoklu oturum açmayı yapılandırın](./media/redbrickhealth-tutorial/tutorial_attribute_04.png)
    
    ![Çoklu oturum açmayı yapılandırın](./media/redbrickhealth-tutorial/tutorial_attribute_05.png)
    
    b. İçinde **adı** metin kutusuna, ilgili satır için gösterilen öznitelik adı yazın.
    
    c. Gelen **değeri** listesinde, ilgili satır için gösterilen öznitelik değeri yazın.

    d. Bırakın **Namespace** boş.
    
    e. **Tamam**’a tıklayın.

6. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **Certificate(Base64)** ve sertifika dosyayı bilgisayarınıza kaydedin.

    ![Sertifika indirme bağlantısı](./media/redbrickhealth-tutorial/tutorial_redbrickhealth_certificate.png) 

7. Tıklatın **kaydetmek** düğmesi.

    ![Oturum açma tek Kaydet düğmesi yapılandırın](./media/redbrickhealth-tutorial/tutorial_general_400.png)

8. Üzerinde **RedBrick sistem durumu Yapılandırması** 'yi tıklatın **yapılandırma RedBrick durumu** açmak için **yapılandırma oturum açma** penceresi. Kopya **SAML varlık kimliği** gelen **hızlı başvuru bölümü.**

    ![RedBrick sistem durumu yapılandırması](./media/redbrickhealth-tutorial/tutorial_redbrickhealth_configure.png) 

9. Çoklu oturum açma yapılandırmak için **RedBrick sistem durumu** yan, indirilen göndermek için ihtiyacınız **Certificate(Base64)** ve **SAML varlık kimliği** için [RedBrick sistem durumu Takım Destek](https://home.redbrickhealth.com/contact/). Bunlar, her iki tarafta da ayarlamanızı SAML SSO bağlantı sağlamak için bu ayarı ayarlayın.

> [!TIP]
> Şimdi bu yönergeleri içinde kısa bir sürümünü okuyabilirsiniz [Azure portal](https://portal.azure.com)uygulaması kuruluyor yaparken!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** sekmesinde ve aracılığıyla katıştırılmış belgelere erişebilir **yapılandırma** alt bölüm. Daha fazla bilgiyi burada embedded belgeler özelliği hakkında: [Azure AD embedded belgeler]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Sol bölmede, Azure portal'ı tıklatın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/redbrickhealth-tutorial/create_aaduser_01.png)

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantılar](./media/redbrickhealth-tutorial/create_aaduser_02.png)

3. Açmak için **kullanıcı** iletişim kutusu, tıklatın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/redbrickhealth-tutorial/create_aaduser_03.png)

4. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/redbrickhealth-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** kullanıcı Britta Simon e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değer aşağı yazma **parola** kutusu.

    d. **Oluştur**’a tıklayın.
  
### <a name="create-a-redbrick-health-test-user"></a>RedBrick sistem durumu test kullanıcısı oluşturma

Bu bölümde, Britta Simon RedBrick durumu adlı bir kullanıcı oluşturun. Çalışmak [RedBrick sistem durumu destek ekibi](https://home.redbrickhealth.com/contact/) RedBrick sistem durumu platform kullanıcıları eklemek için. Kullanıcıların oluşturulan ve çoklu oturum açma kullanmadan önce etkinleştirilmelidir. 

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Britta RedBrick sağlık erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**RedBrick sağlık Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **RedBrick sistem durumu**.

    ![Uygulamalar listesinde RedBrick sistem durumu bağlantı](./media/redbrickhealth-tutorial/tutorial_redbrickhealth_app.png)  

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantı][202]

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Ekleme atama bölmesi][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde RedBrick durumu kutucuğu tıklattığınızda, otomatik olarak RedBrick sistem durumu uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/redbrickhealth-tutorial/tutorial_general_01.png
[2]: ./media/redbrickhealth-tutorial/tutorial_general_02.png
[3]: ./media/redbrickhealth-tutorial/tutorial_general_03.png
[4]: ./media/redbrickhealth-tutorial/tutorial_general_04.png

[100]: ./media/redbrickhealth-tutorial/tutorial_general_100.png

[200]: ./media/redbrickhealth-tutorial/tutorial_general_200.png
[201]: ./media/redbrickhealth-tutorial/tutorial_general_201.png
[202]: ./media/redbrickhealth-tutorial/tutorial_general_202.png
[203]: ./media/redbrickhealth-tutorial/tutorial_general_203.png

