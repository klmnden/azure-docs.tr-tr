---
title: 'Öğretici: Azure Active Directory Tümleştirme G Suite ile | Microsoft Docs'
description: Çoklu oturum açma G Suite ile Azure Active Directory arasındaki yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 38a6ca75-7fd0-4cdc-9b9f-fae080c5a016
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/02/2018
ms.author: jeedes
ms.openlocfilehash: aaa42283f44bc090d54db85ff5062edffa1b4b02
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
---
# <a name="tutorial-azure-active-directory-integration-with-g-suite"></a>Öğretici: Azure Active Directory Tümleştirme G Suite ile

Bu öğreticide, Azure Active Directory (Azure AD) ile G Suite tümleştirmek öğrenin.

G Suite Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- G Suite erişimi, Azure AD'de kontrol edebilirsiniz.
- Azure AD hesaplarına otomatik olarak G paketine (çoklu oturum açma) açan kullanıcılarınıza etkinleştirebilirsiniz.
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirme G paketiyle yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir G Suite çoklu oturum açma abonelik etkin
- Bir Google Apps aboneliği veya Google Cloud Platform'un abonelik.

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, şunları yapabilirsiniz [bir aylık deneme sürümünü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="frequently-asked-questions"></a>Sık Sorulan Sorular
1.  **S: Azure AD ile Google bulut platformu SSO tümleştirme bu tümleştirme desteğine mu?**
    
    C: Evet. Google Cloud Platform'un ve Google Apps aynı kimlik doğrulama platformu paylaşır. Bu nedenle GCP tümleştirme yapmak için SSO Google Apps ile yapılandırmanız gerekir.


2. **S: Azure AD çoklu oturum açma ile uyumlu Chromebooks ve diğer Chrome aygıtları misiniz?**
   
    A: Evet kullanıcıları Azure AD kimlik bilgilerini kullanarak Chromebook cihazlarını imzalayabilirsiniz. Bu bkz [G Suite Destek makalesine](https://support.google.com/chrome/a/answer/6060880) neden hakkında bilgi için kimlik bilgilerini iki kez kullanıcılardan.

3. **S: ı çoklu oturum açma etkinleştirirseniz, kullanıcıların Google sınıf, GMail, Google sürücü, YouTube vb. gibi herhangi bir Google ürünü oturumu açmak için Azure AD kimlik bilgilerini kullanmak erişebilecek mi?**
   
    A: Evet hangisini seçtiğinize bağlı [hangi G Suite](https://support.google.com/a/answer/182442?hl=en&ref_topic=1227583) etkinleştirmek veya kuruluşunuz için devre dışı bırakmak seçin.

4. **S: yalnızca bir alt kümesini G Suite Kullanıcılarım için çoklu oturum açmayı etkinleştir?**
   
    A: Hayır çoklu oturum açma üzerinde hemen kapatma kendi Azure AD kimlik bilgileriyle kimlik doğrulaması tüm G paketi kullanıcılarınızın gerektirir. Birden çok kimlik sağlayıcısı sahip G Suite desteklemediğinden G Suite ortamınız için kimlik sağlayıcısı ya da Azure AD olabilir veya Google--ancak ikisini aynı anda.

5. **S: bir kullanıcının Windows oturum açtığı, bunlar otomatik olarak G paketine bir parola girmesi istenir alma olmadan kimlik doğrulaması olur mu?**
   
    Y: Bu senaryoyu etkinleştirmek için iki seçenek vardır. İlk olarak, kullanıcılar Windows 10 cihazlara üzerinden oturum [Azure Active Directory katılım](active-directory-azureadjoin-overview.md). Alternatif olarak, kullanıcıların etki alanına katılmış bir şirket içi Active Directory'ye Azure ad çoklu oturum açma için etkinleştirilen Windows cihazları oturum bir [Active Directory Federasyon Hizmetleri (AD FS)](active-directory-aadconnect-user-signin.md) dağıtım. Her iki seçenek Azure AD arasında çoklu oturum açmayı etkinleştirmek için aşağıdaki öğreticide adımları gerçekleştirmek ihtiyaç duyduğunuz ve G Suite.

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden G paketi ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-g-suite-from-the-gallery"></a>Galeriden G paketi ekleme
Azure AD G Suite tümleştirilmesi yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden G paketi eklemeniz gerekir.

**Galeriden G paketi eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **G Suite**seçin **G Suite** sonuç panelinden ardından **Ekle** uygulama eklemek için düğmeyi.

    ![Sonuçlar listesinde G paketi](./media/active-directory-saas-googleapps-tutorial/tutorial_googleapps_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma G "Britta Simon" adlı bir test kullanıcı tabanlı Suite ile test etme.

Tekli çalışmaya oturum için Azure AD ne karşılık gelen G grubundaki bir kullanıcı için Azure AD içinde olduğu bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının G paketindeki ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Değeri G paketindeki atayın **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** bağlantı ilişkisi oluşturmak için.

Yapılandırma ve Azure AD çoklu oturum açma G Suite ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[G Suite test kullanıcısı oluşturma](#create-a-g-suite-test-user)**  - Britta Simon, karşılık gelen kullanıcı Azure AD gösterimini bağlı G Suite sağlamak için.
4. **[Azure AD test kullanıcısı atayın](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Test çoklu oturum açma](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma G Suite uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma G paketiyle yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **G Suite** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma iletişim kutusu](./media/active-directory-saas-googleapps-tutorial/tutorial_googleapps_samlbase.png)

3. Üzerinde **G Suite etki alanı ve URL'leri** için yapılandırmak istiyorsanız, bölüm **Gmail** aşağıdaki adımları gerçekleştirin:

    ![G Suite etki alanı ve URL'leri tek oturum açma bilgileri](./media/active-directory-saas-googleapps-tutorial/tutorial_googleapps_urlgmail.png)

    a. İçinde **oturum açma URL'si** metin kutusuna, URL şu biçimi kullanarak URL'sini yazın: `https://www.google.com/a/<yourdomain.com>/ServiceLogin?continue=https://mail.google.com`

    b. İçinde **tanımlayıcısı** metin kutusuna, URL şu biçimi kullanarak bir yazın: 
    | |
    |--|
    | `google.com/a/<yourdomain.com>` |
    | `google.com` |
    | `http://google.com` |
    | `http://google.com/a/<yourdomain.com>` |
 
    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler gerçek oturum açma URL'si ve tanımlayıcı ile güncelleştirin. Kişi [G Suite istemci destek ekibi](https://www.google.com/contact/) bu değerleri almak için.

4. Üzerinde **G Suite etki alanı ve URL'leri** için yapılandırmak istiyorsanız, bölüm **Google Cloud Platform'un** aşağıdaki adımları gerçekleştirin:

    ![G Suite etki alanı ve URL'leri tek oturum açma bilgileri](./media/active-directory-saas-googleapps-tutorial/tutorial_googleapps_url1.png)

    a. İçinde **oturum açma URL'si** metin kutusuna, URL şu biçimi kullanarak URL'sini yazın: `https://www.google.com/a/<yourdomain.com>/ServiceLogin?continue=https://console.cloud.google.com `

    b. İçinde **tanımlayıcısı** metin kutusuna, URL şu biçimi kullanarak bir yazın: 
    | |
    |--|
    | `google.com/a/<yourdomain.com>` |
    | `google.com` |
    | `http://google.com` |
    | `http://google.com/a/<yourdomain.com>` |
    
    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler gerçek oturum açma URL'si ve tanımlayıcı ile güncelleştirin. Kişi [G Suite istemci destek ekibi](https://www.google.com/contact/) bu değerleri almak için. 

5. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **sertifika** ve sertifika dosyayı bilgisayarınıza kaydedin.

    ![Sertifika indirme bağlantısı](./media/active-directory-saas-googleapps-tutorial/tutorial_googleapps_certificate.png) 

6. Tıklatın **kaydetmek** düğmesi.

    ![Oturum açma tek Kaydet düğmesi yapılandırın](./media/active-directory-saas-googleapps-tutorial/tutorial_general_400.png)

7. Üzerinde **G paketi yapılandırma** 'yi tıklatın **yapılandırma G Suite** açmak için **yapılandırma oturum açma** penceresi. Kopya **Sign-Out URL, SAML çoklu oturum açma hizmet URL'si ve değişiklik parola URL'si** gelen **hızlı başvuru bölümü.**

    ![G paketi yapılandırma](./media/active-directory-saas-googleapps-tutorial/tutorial_googleapps_configure.png) 

8. Tarayıcınızda yeni bir sekme açın ve oturum [G Suite Yönetici Konsolu](http://admin.google.com/) yönetici hesabını kullanarak.

9. Tıklatın **güvenlik**. Bağlantıyı görmüyorsanız, altında gizli olabilir **daha fazla denetim** ekranın altındaki menüsü.
   
    ![Güvenlik'e tıklayın.][10]

10. Üzerinde **güvenlik** sayfasında, **çoklu oturum açmayı kurduğunuzda (SSO).**
   
    ![SSO'ı tıklatın.][11]

11. Aşağıdaki yapılandırma değişikliklerini gerçekleştirin:
   
    ![SSO yapılandırın][12]
   
    a. Seçin **üçüncü taraf kimlik sağlayıcısı ile Kurulum SSO**.

    b. İçinde **oturum açma sayfası URL'si** alan G paketinde, değerini yapıştırın **çoklu oturum açma hizmet URL'si** Azure portalından kopyalanan.

    c. İçinde **oturum kapatma sayfası URL'si** alan G paketinde, değerini yapıştırın **Sign-Out URL** Azure portalından kopyalanan. 

    d. İçinde **değiştirmek parola URL'si** alan G paketinde, değerini yapıştırın **değiştirmek parola URL'si** Azure portalından kopyalanan. 

    e. G paketindeki için **doğrulama sertifikası**, Azure portalından indirdiğiniz sertifikasını karşıya yükleyin.

    f. Seçin **belirli bir etki alanı gönderici kullanmak**.

    g. Tıklatın **değişiklikleri kaydetmek**.

> [!TIP]
> Şimdi bu yönergeleri içinde kısa bir sürümünü okuyabilirsiniz [Azure portal](https://portal.azure.com)uygulaması kuruluyor yaparken!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** sekmesinde ve aracılığıyla katıştırılmış belgelere erişebilir **yapılandırma** alt bölüm. Daha fazla bilgiyi burada embedded belgeler özelliği hakkında: [Azure AD embedded belgeler]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Sol bölmede, Azure portal'ı tıklatın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/active-directory-saas-googleapps-tutorial/create_aaduser_01.png)

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantılar](./media/active-directory-saas-googleapps-tutorial/create_aaduser_02.png)

3. Açmak için **kullanıcı** iletişim kutusu, tıklatın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/active-directory-saas-googleapps-tutorial/create_aaduser_03.png)

4. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/active-directory-saas-googleapps-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** kullanıcı Britta Simon e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değer aşağı yazma **parola** kutusu.

    d. **Oluştur**’a tıklayın.
 
### <a name="create-a-g-suite-test-user"></a>G Suite test kullanıcısı oluşturma

Bu bölümün amacı Britta Simon G Suite yazılım adlı bir kullanıcı oluşturmaktır. G Suite otomatik sağlama, varsayılan olarak etkin olduğu destekler. Bu bölümdeki hiçbir şey yoktur. Bir kullanıcı zaten G Suite yazılımda yoksa, G Suite yazılım erişmeyi denediğinde yeni bir tane oluşturulur.

>[!NOTE] 
>Bir kullanıcı el ile oluşturmanız gerekiyorsa, kişi [Google destek ekibi](https://www.google.com/contact/).

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Britta G paketine erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**G paketine Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **G Suite**.

    ![Uygulamalar listesinde G Suite bağlantısı](./media/active-directory-saas-googleapps-tutorial/tutorial_googleapps_app.png)  

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantı][202]

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Ekleme atama bölmesi][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açmayı test edin

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli G Suite parçasında tıklattığınızda, otomatik olarak G Suite uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](active-directory-saas-tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-googleapps-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-googleapps-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-googleapps-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-googleapps-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-googleapps-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-googleapps-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-googleapps-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-googleapps-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-googleapps-tutorial/tutorial_general_203.png
[10]: ./media/active-directory-saas-googleapps-tutorial/gapps-security.png
[11]: ./media/active-directory-saas-googleapps-tutorial/security-gapps.png
[12]: ./media/active-directory-saas-googleapps-tutorial/gapps-sso-config.png

