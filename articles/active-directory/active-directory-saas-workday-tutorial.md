---
title: "Öğretici: Azure Active Directory Tümleştirme ile Workday | Microsoft Docs"
description: "Çoklu oturum açma Azure Active Directory ile iş günü arasında yapılandırmayı öğrenin."
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: e9da692e-4a65-4231-8ab3-bc9a87b10bca
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: jeedes
ms.openlocfilehash: 5e4d46f9a3954698fbbe3c80fd8a95f4cd87465b
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-workday"></a>Öğretici: Azure Active Directory Tümleştirme ile iş günü

Bu öğreticide, Azure Active Directory (Azure AD) ile Workday tümleştirmek öğrenin.

Workday Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Workday erişimi, Azure AD'de kontrol edebilirsiniz
- Otomatik olarak için Workday (çoklu oturum açma) ile Azure AD hesaplarına açan kullanıcılarınıza etkinleştirebilirsiniz
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirme Workday ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir iş günü çoklu oturum açma etkin abonelik

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden Workday ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-workday-from-the-gallery"></a>Galeriden Workday ekleme
Azure AD iş günü tümleştirilmesi yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden Workday eklemeniz gerekir.

**Galeriden Workday eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Uygulamalar][3]

4. Arama kutusuna **Workday**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-workday-tutorial/tutorial_workday_search.png)

5. Sonuçlar panelinde seçin **Workday**ve ardından **Ekle** uygulama eklemek için düğmesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-workday-tutorial/tutorial_workday_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Çoklu oturum açmayı yapılandırma ve Azure AD sınama
Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon." olarak adlandırılan bir test kullanıcı tabanlı Workday ile test etme

Tekli çalışmaya oturum için Azure AD ne karşılık gelen iş günü içinde bir kullanıcı için Azure AD içinde olduğu bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının Workday ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Bu bağlantı değeri atayarak ilişkisi **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** iş günü içinde.

Yapılandırma ve Azure AD çoklu oturum açma Workday ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Bir iş günü test kullanıcısı oluşturma](#creating-a-workday-test-user)**  - Britta Simon, karşılık gelen kullanıcı Azure AD gösterimini bağlı Workday sağlamak için.
4. **[Azure AD test kullanıcısı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açmayı test](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma Workday uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma ile Workday yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **Workday** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-workday-tutorial/tutorial_workday_samlbase.png)

3. Üzerinde **Workday etki alanı ve URL'leri** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-workday-tutorial/tutorial_workday_url.png)

    a. İçinde **oturum açma URL'si** metin değeri olarak yazın:`https://impl.workday.com/<tenant>/login-saml2.htmld`

    b. İçinde **yanıt URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın:`https://impl.workday.com/<tenant>/login-saml.htmld`

    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler gerçek oturum açma URL'si ve yanıt URL'si ile güncelleştirin. Yanıt URL'si bir alt etki alanı gibi olmalıdır: www, wd2, wd3, wd3 impl, wd5, wd5 impl). Aşağıdakine benzer kullanarak "*http://www.myworkday.com*" çalışır, ancak "*http://myworkday.com*" desteklemez. Kişi [Workday istemci destek ekibi](https://www.workday.com/en-us/partners-services/services/support.html) bu değerleri almak için. 
 

4. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **sertifika (Base64)** ve sertifika dosyayı bilgisayarınıza kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-workday-tutorial/tutorial_workday_certificate.png) 

5. Tıklatın **kaydetmek** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-workday-tutorial/tutorial_general_400.png)

6. Üzerinde **Workday yapılandırma** 'yi tıklatın **yapılandırma Workday** açmak için **yapılandırma oturum açma** penceresi. Kopya **Sign-Out URL, SAML varlık kimliği ve SAML çoklu oturum açma hizmet URL'si** gelen **hızlı başvuru bölümü.**

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-workday-tutorial/tutorial_workday_configure.png) 
<CS>
7. Farklı web tarayıcısı penceresinde Workday şirket sitenize yönetici olarak oturum açın.

8. Git **menü \> çalışma ekranı**.
   
    ![Çalışma ekranı](./media/active-directory-saas-workday-tutorial/IC782923.png "çalışma ekranı")

9. Git **hesap Yönetim**.
   
    ![Hesap Yönetimi](./media/active-directory-saas-workday-tutorial/IC782924.png "hesap yönetimi")

10. Git **Kiracı kurulumunu – güvenlik Düzenle**.
   
    ![Kiracı Güvenliği Düzenle](./media/active-directory-saas-workday-tutorial/IC782925.png "Kiracı güvenlik Düzenle")

11. İçinde **yönlendirme URL'lerini** bölümünde, aşağıdaki adımları gerçekleştirin:
   
    ![Yönlendirme URL'lerini](./media/active-directory-saas-workday-tutorial/IC7829581.png "yeniden yönlendirme URL'leri")
   
    a. Tıklatın **Satır Ekle**.
   
    b. İçinde **oturum açma yeniden yönlendirme URL'si** textbox ve **mobil yeniden yönlendirme URL'sini** metin kutusuna, türü **oturum açma URL'si** girmiş olduğunuz **Workday etki alanı ve URL'leri** Azure portalının bölümü.
   
    c. Azure portalında üzerinde **yapılandırma oturum açma** penceresinde, kopya **Sign-Out URL**ve ardından yapıştırın **oturumu kapatıp tekrar yönlendirme URL'sini** metin kutusu.
   
    d.  İçinde **ortam** metin kutusuna, ortamının adını yazın.  

    >[!NOTE]
    > Ortam özniteliğinin değeri Kiracı URL değerine bağlıdır:  
    >-Workday kiracısı URL'si etki alanı adı ile impl örneğin başlayıp başlamadığını: *https://impl.workday.com/\<Kiracı\>/login-saml2.htmld*), **ortam** uygulamasına özniteliği ayarlanmalıdır.  
    >-Etki alanı adı başka bir şey ile başlarsa başvurmanız gerekir [Workday istemci destek ekibi](https://www.workday.com/en-us/partners-services/services/support.html) eşleştirme almak için **ortam** değeri.

12. İçinde **SAML Kurulumu** bölümünde, aşağıdaki adımları gerçekleştirin:
   
    ![SAML Kurulumu](./media/active-directory-saas-workday-tutorial/IC782926.png "SAML Kurulumu")
   
    a.  Seçin **SAML kimlik doğrulamasını etkinleştirme**.
   
    b.  Tıklatın **Satır Ekle**.

13. SAML kimlik sağlayıcısı bölümünde, aşağıdaki adımları gerçekleştirin:
   
    ![SAML kimlik sağlayıcısı](./media/active-directory-saas-workday-tutorial/IC7829271.png "SAML kimlik sağlayıcısı")
   
    a. Kimlik sağlayıcı adı metin kutusuna bir sağlayıcı adı yazın (örneğin: *SPInitiatedSSO*).
   
    b. Azure portalında üzerinde **yapılandırma oturum açma** penceresinde, kopya **SAML varlık kimliği** değer ve ardından yapıştırın **veren** metin kutusu.
   
    c. Seçin **etkinleştirmek iş günü başlatılan oturum kapatma**.
   
    d. Azure portalında üzerinde **yapılandırma oturum açma** penceresinde, kopya **Sign-Out URL** değer ve ardından yapıştırın **oturum kapatma istek URL'si** metin kutusu.

    e. Tıklatın **kimlik sağlayıcısı ortak anahtar sertifikası**ve ardından **oluşturma**. 

    ![Oluşturma](./media/active-directory-saas-workday-tutorial/IC782928.png "oluşturma")

    f. Tıklatın **x509 oluşturma ortak anahtar**. 

    ![Oluşturma](./media/active-directory-saas-workday-tutorial/IC782929.png "oluşturma")


14. İçinde **görünüm x509 ortak anahtar** bölümünde, aşağıdaki adımları gerçekleştirin: 
   
    ![Görünüm x509 ortak anahtar](./media/active-directory-saas-workday-tutorial/IC782930.png "görünüm x509 ortak anahtar") 
   
    a. İçinde **adı** metin kutusuna, sertifikanızı için bir ad yazın (örneğin: *PPE\_SP*).
   
    b. İçinde **geçerlilik başlangıcı** metin kutusu, geçerli öznitelik değerinden sertifikanızın yazın.
   
    c.  İçinde **için geçerli** metin kutusu, geçerli sertifikanızın öznitelik değeri girin.
   
    > [!NOTE]
    > Çift tıklatarak indirilen sertifika bitiş tarihi ve geçerli geçerli alabilirsiniz.  Tarihleri altında listelenen **ayrıntıları** sekmesi.
    > 
    >
   
    d.  Base-64 kodlanmış sertifikanızı Not Defteri'nde açın ve içeriğini kopyalayın.
   
    e.  İçinde **sertifika** metin kutusuna, Pano içeriğini yapıştırın.
   
    f.  **Tamam** düğmesine tıklayın.

15. Aşağıdaki adımları gerçekleştirin: 
   
    ![SSO yapılandırma](./media/active-directory-saas-workday-tutorial/WorkdaySSOConfiguratio.png "SSO yapılandırma")
   
    a.  Etkinleştirme **x509 özel anahtar çifti**.
   
    b.  İçinde **hizmet sağlayıcı kimliği** metin kutusuna, türü **http://www.workday.com**.
   
    c.  Seçin **etkinleştir SP tarafından başlatılan SAML kimlik doğrulaması**.
   
    d.  Azure portalında üzerinde **yapılandırma oturum açma** penceresinde, kopya **SAML çoklu oturum açma hizmet URL'si** değer ve ardından yapıştırın **IDP SSO hizmet URL'si** metin kutusu.
   
    e. Seçin **SP tarafından başlatılan kimlik doğrulama isteği Deflate değil**.
   
    f. Olarak **kimlik doğrulama isteği imza yöntemi**seçin **SHA256**. 
   
    ![Kimlik doğrulama isteği imza yöntemi](./media/active-directory-saas-workday-tutorial/WorkdaySSOConfiguration.png "kimlik doğrulama isteği imza yöntemi") 
   
    g. **Tamam** düğmesine tıklayın. 
   
    ![TAMAM](./media/active-directory-saas-workday-tutorial/IC782933.png "TAMAM")
<CE>

> [!TIP]
> Şimdi bu yönergeleri içinde kısa bir sürümünü okuyabilirsiniz [Azure portal](https://portal.azure.com)uygulaması kuruluyor yaparken!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** sekmesinde ve aracılığıyla katıştırılmış belgelere erişebilir **yapılandırma** alt bölüm. Daha fazla bilgiyi burada embedded belgeler özelliği hakkında: [Azure AD embedded belgeler]( https://go.microsoft.com/fwlink/?linkid=845985)
>

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

![Azure AD Kullanıcı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portal**, sol gezinti bölmesinde tıklatın **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-workday-tutorial/create_aaduser_01.png) 

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-workday-tutorial/create_aaduser_02.png) 

3. Açmak için **kullanıcı** iletişim kutusunda, tıklatın **Ekle** iletişim kutusunun üst kısmında.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-workday-tutorial/create_aaduser_03.png) 

4. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-workday-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna, türü **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna, türü **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**'a tıklayın.
 
### <a name="creating-a-workday-test-user"></a>Bir iş günü test kullanıcısı oluşturma

İş günü içinde sağlanan bir sınama kullanıcısı almak için başvurmanız gerekir [Workday istemci destek ekibi](https://www.workday.com/en-us/partners-services/services/support.html).
[Workday istemci destek ekibi](https://www.workday.com/en-us/partners-services/services/support.html) kullanıcı sizin için oluşturur.

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atama

Bu bölümde, iş günü için erişim vererek, Azure çoklu oturum açma kullanılacak Britta Simon etkinleştirin.

![Kullanıcı atama][200] 

**İş günü için Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **Workday**.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-workday-tutorial/tutorial_workday_app.png) 

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    ![Kullanıcı atama][202] 

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Kullanıcı atama][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açmayı test etme

Çoklu oturum açma ayarlarınızı test etmek isterseniz, erişim paneli açın. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](active-directory-saas-tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](active-directory-appssoaccess-whatis.md)
* [Kullanıcı sağlamayı Yapılandır](active-directory-saas-workday-inbound-tutorial.md)



<!--Image references-->

[1]: ./media/active-directory-saas-workday-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-workday-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-workday-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-workday-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-workday-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-workday-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-workday-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-workday-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-workday-tutorial/tutorial_general_203.png

