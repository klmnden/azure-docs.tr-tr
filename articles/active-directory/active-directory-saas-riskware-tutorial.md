---
title: 'Öğretici: Azure Active Directory Tümleştirme ile Riskware | Microsoft Docs'
description: Çoklu oturum açma Azure Active Directory ile Riskware arasında yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 81866167-b163-4695-8978-fd29a25dac7a
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/15/2018
ms.author: jeedes
ms.openlocfilehash: bc3b4c75875661de65f559df9a83c3c92e34304e
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34655280"
---
# <a name="tutorial-azure-active-directory-integration-with-riskware"></a>Öğretici: Azure Active Directory Tümleştirme Riskware ile

Bu öğreticide, Azure Active Directory (Azure AD) ile Riskware tümleştirmek öğrenin.

Riskware Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Riskware erişimi, Azure AD'de kontrol edebilirsiniz.
- Otomatik olarak için Riskware (çoklu oturum açma) ile Azure AD hesaplarına açan kullanıcılarınıza etkinleştirebilirsiniz.
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirme Riskware ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir Riskware çoklu oturum açma abonelik etkin

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, şunları yapabilirsiniz [bir aylık deneme sürümünü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden Riskware ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-riskware-from-the-gallery"></a>Galeriden Riskware ekleme
Azure AD Riskware tümleştirilmesi yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden Riskware eklemeniz gerekir.

**Galeriden Riskware eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **Riskware**seçin **Riskware** sonuç panelinden ardından **Ekle** uygulama eklemek için düğmeyi.

    ![Sonuçlar listesinde Riskware](./media/active-directory-saas-riskware-tutorial/tutorial_riskware_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı Riskware sınayın.

Tekli çalışmaya oturum için Azure AD Riskware karşılık gelen kullanıcı için bir kullanıcı Azure AD'de nedir bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının Riskware ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma Riskware ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Riskware test kullanıcısı oluşturma](#create-a-riskware-test-user)**  - Britta Simon, karşılık gelen kullanıcı Azure AD gösterimini bağlı Riskware sağlamak için.
4. **[Azure AD test kullanıcısı atayın](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Test çoklu oturum açma](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma Riskware uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma ile Riskware yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **Riskware** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/active-directory-saas-riskware-tutorial/tutorial_riskware_samlbase.png)

3. Üzerinde **Riskware etki alanı ve URL'leri** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Riskware etki alanı ve URL'leri tek oturum açma bilgileri](./media/active-directory-saas-riskware-tutorial/tutorial_riskware_url.png)

    a. İçinde **URL üzerinde oturum** metin kutusuna, URL şu biçimi kullanarak bir yazın:
    | Ortam| URL deseni|
    |--|--|
    | UAT|  `https://riskcloud.net/uat?ccode=<COMPANYCODE>` |
    | ÜRETİM| `https://riskcloud.net/prod?ccode=<COMPANYCODE>` |
    | TANITIMI| `https://riskcloud.net/demo?ccode=<COMPANYCODE>` | 
    |||

    b. İçinde **tanımlayıcısı (varlık kimliği)** metin kutusuna, bir URL yazın:
    | Ortam| URL deseni|
    |--|--|
    | UAT| `https://riskcloud.net/uat` |
    | ÜRETİM| `https://riskcloud.net/prod` |
    | TANITIMI| `https://riskcloud.net/demo` | 
    |||

    > [!NOTE] 
    > URL değeri oturum gerçek değil. Değerin gerçek oturum açma URL'si ile güncelleştirin. Kişi [Riskware istemci destek ekibi](mailto:support@pansoftware.com.au) değeri alınamıyor.

4. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **meta veri XML** ve meta veri dosyası, bilgisayarınıza kaydedin.

    ![Sertifika indirme bağlantısı](./media/active-directory-saas-riskware-tutorial/tutorial_riskware_certificate.png) 

5. Tıklatın **kaydetmek** düğmesi.

    ![Oturum açma tek Kaydet düğmesi yapılandırın](./media/active-directory-saas-riskware-tutorial/tutorial_general_400.png)

6. Üzerinde **Riskware yapılandırma** 'yi tıklatın **yapılandırma Riskware** açmak için **yapılandırma oturum açma** penceresi. Kopya **Sign-Out URL ve SAML çoklu oturum açma hizmet URL'si** gelen **hızlı başvuru bölümü.**

    ![Riskware yapılandırma](./media/active-directory-saas-riskware-tutorial/tutorial_riskware_configure.png) 

7. Farklı web tarayıcısı penceresinde Riskware şirket sitenize yönetici olarak oturum açın.

8. Sağ üstte tıklatın **Bakım** bakım sayfasını açın. 

    ![Riskware yapılandırmaları tutmanız](./media/active-directory-saas-riskware-tutorial/tutorial_riskware_maintain.png)

9. Bakım sayfasında tıklatın **kimlik doğrulama**.

    ![Riskware yapılandırma authen](./media/active-directory-saas-riskware-tutorial/tutorial_riskware_authen.png)

10. İçinde **kimlik doğrulama Yapılandırması** sayfasında, aşağıdaki adımları gerçekleştirin:

    ![Riskware yapılandırma authenconfig](./media/active-directory-saas-riskware-tutorial/tutorial_riskware_config.png)

    a. Seçin **türü** olarak **SAML** kimlik doğrulaması için.

    b. İçinde **kod** metin kutusuna, kodunuzu AZURE_UAT gibi yazın.

    c. İçinde **açıklama** metin kutusuna, SSO için AZURE yapılandırma gibi bir açıklama yazın.

    d. İçinde **tek oturum açma sayfasına** metin değerini yapıştırın **SAML çoklu oturum açma hizmet URL'si**, Azure portalından kopyalanan.

    e. İçinde **sayfa oturum** metin değerini yapıştırın **Sign-Out URL**, Azure portalından kopyalanan.

    f. İçinde **Post Form alanı** metin kutusuna, SAML SamlResponse gibi içeren Post yanıt mevcut alan adını yazın

    g. İçinde **XML kimlik etiket adı** metin kutusuna, SAML yanıt NameID gibi benzersiz tanımlayıcısını içeren türü özniteliği.

    h. İndirilen açmak **meta veri Xml** meta veri dosyası sertifikadan Not Defteri'nde Azure Portal'dan kopyalayın ve yapıştırın **sertifika** metin kutusu
    
    i. İçinde **tüketici URL** metin değerini yapıştırın **yanıt URL'si**, hangi destek ekibinden alın.

    j. İçinde **veren** metin değerini yapıştırın **tanımlayıcısı**, hangi destek ekibinden aldığınız.

    > [!Note]
    > Kişi [Riskware istemci destek ekibi](mailto:support@pansoftware.com.au) bu değerleri almak için

    k. Seçin **kullanım sonrası** isteği SAML post parametre olarak geçirmek için.

    l. Seçin **kullanım isteği SAML** geçirmek için SSO izin isteği SAML SP tarafından başlatılan için.

    m. **Kaydet**’e tıklayın.

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Sol bölmede, Azure portal'ı tıklatın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/active-directory-saas-riskware-tutorial/create_aaduser_01.png)

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantılar](./media/active-directory-saas-riskware-tutorial/create_aaduser_02.png)

3. Açmak için **kullanıcı** iletişim kutusu, tıklatın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/active-directory-saas-riskware-tutorial/create_aaduser_03.png)

4. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/active-directory-saas-riskware-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** kullanıcı Britta Simon e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değer aşağı yazma **parola** kutusu.

    d. **Oluştur**’a tıklayın.
 
### <a name="create-a-riskware-test-user"></a>Riskware test kullanıcısı oluşturma

Azure AD kullanıcıları için Riskware oturum açmak etkinleştirmek için bunların Riskware sağlanmalıdır. Riskware içinde sağlama bir el ile bir görevdir.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1. İçin Riskware bir güvenlik yöneticisi olarak oturum açın.

2. Sağ üstte tıklatın **Bakım** bakım sayfasını açın. 

    ![Riskware yapılandırma tutar](./media/active-directory-saas-riskware-tutorial/tutorial_riskware_maintain.png)

3. Bakım sayfasında tıklatın **kişiler**.

    ![Riskware yapılandırma kişiler](./media/active-directory-saas-riskware-tutorial/tutorial_riskware_people.png)

4. Üzerinde **ayrıntıları** sekmesinde, aşağıdaki adımları gerçekleştirin:
    
    ![Riskware yapılandırma ayrıntıları](./media/active-directory-saas-riskware-tutorial/tutorial_riskware_details.png)

    a. Seçin **kişi türü** çalışana gibi.

    b. İçinde **ad** metin gibi kullanıcının ilk adını girin **Britta**.

    c. İçinde **Soyadı** metin kutusuna, son kullanıcı gibi adını **Simon**.

5. Üzerinde **güvenlik** sekmesinde, aşağıdaki adımları gerçekleştirin:    

    ![Riskware yapılandırma güvenlik](./media/active-directory-saas-riskware-tutorial/tutorial_riskware_security.png)

    a. Altında **kimlik doğrulaması** bölümünde, select **kimlik doğrulaması** sahip kurulum modu SSO için AZURE yapılandırma ister.

    b. Altında **oturum açma ayrıntıları** bölümünde **kullanıcı kimliği** metin kutusuna, bir kullanıcı gibi e-posta girin **brittasimon@contoso.com**.

    c. İçinde **parola** metin kutusuna, kullanıcının parolasını girin.

6. Üzerinde **prosedürlerini** sekmesinde, aşağıdaki adımları gerçekleştirin:

    ![Riskware yapılandırma kuruluş](./media/active-directory-saas-riskware-tutorial/tutorial_riskware_org.png)

    a. Altında **prosedürlerini** bölümünde, kuruluşunuzun olarak seçin **Level1** prosedürlerini.
    
    b. Altında **kişinin birincil çalışma alanına** bölümünde **konumu** metin kutusuna, konumunuz yazın.

    c. Altında **çalışan** bölümünde, select **çalışan durumu** normal ister.

7. **Kaydet**’e tıklayın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Britta Riskware için erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**Riskware için Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **Riskware**.

    ![Uygulamalar listesinde Riskware bağlantı](./media/active-directory-saas-riskware-tutorial/tutorial_riskware_app.png)  

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantı][202]

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Ekleme atama bölmesi][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli Riskware parçasında tıklattığınızda, otomatik olarak Riskware uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](active-directory-saas-tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-riskware-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-riskware-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-riskware-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-riskware-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-riskware-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-riskware-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-riskware-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-riskware-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-riskware-tutorial/tutorial_general_203.png

