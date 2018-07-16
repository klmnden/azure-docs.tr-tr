---
title: 'Öğretici: Azure Active Directory tümleştirmesiyle Riskware | Microsoft Docs'
description: Azure Active Directory ve Riskware arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 81866167-b163-4695-8978-fd29a25dac7a
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2018
ms.author: jeedes
ms.openlocfilehash: 7705baa0ba912f24d7859110c75d36703aeb4a77
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/14/2018
ms.locfileid: "39041968"
---
# <a name="tutorial-azure-active-directory-integration-with-riskware"></a>Öğretici: Azure Active Directory Riskware ile tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile Riskware tümleştirme konusunda bilgi edinin.

Azure AD ile Riskware tümleştirme ile aşağıdaki avantajları sağlar:

- Riskware erişimi, Azure AD'de kontrol edebilirsiniz.
- Otomatik olarak imzalanan için Riskware (çoklu oturum açma) ile Azure AD hesaplarına açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile Riskware yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliğiniz
- Abonelik Riskware çoklu oturum açma etkin

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, şunları yapabilirsiniz [bir aylık deneme sürümü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden Riskware ekleme
2. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-riskware-from-the-gallery"></a>Galeriden Riskware ekleme
Azure AD'de Riskware tümleştirmesini yapılandırmak için Riskware Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden Riskware eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **Riskware**seçin **Riskware** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Sonuç listesinde Riskware](./media/riskware-tutorial/tutorial_riskware_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırın ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı Riskware sınayın.

Tek iş için oturum açma için Azure AD ne Riskware karşılığı kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısının Riskware ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma Riskware ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Riskware test kullanıcısı oluşturma](#create-a-riskware-test-user)**  - kullanıcı Azure AD gösterimini bağlı Riskware Britta simon'un bir karşılığı vardır.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve Riskware uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma ile Riskware yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **Riskware** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma iletişim kutusu](./media/riskware-tutorial/tutorial_riskware_samlbase.png)

3. Üzerinde **Riskware etki alanı ve URL'ler** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Riskware etki alanı ve URL'ler tek oturum açma bilgileri](./media/riskware-tutorial/tutorial_riskware_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna bir URL şu biçimi kullanarak:
    | Ortam| URL deseni|
    |--|--|
    | UAT|  `https://riskcloud.net/uat?ccode=<COMPANYCODE>` |
    | ÜRÜN| `https://riskcloud.net/prod?ccode=<COMPANYCODE>` |
    | TANITIMI| `https://riskcloud.net/demo?ccode=<COMPANYCODE>` |
    |||

    b. İçinde **tanımlayıcı (varlık kimliği)** metin kutusuna bir URL:
    | Ortam| URL deseni|
    |--|--|
    | UAT| `https://riskcloud.net/uat` |
    | ÜRÜN| `https://riskcloud.net/prod` |
    | TANITIMI| `https://riskcloud.net/demo` |
    |||

    > [!NOTE]
    > Oturum açma URL değeri, gerçek değil. Değerini gerçek oturum açma URL'si ile güncelleştirin. İlgili kişi [Riskware istemci Destek ekibine](mailto:support@pansoftware.com.au) değeri alınamıyor.

4. Üzerinde **SAML imzalama sertifikası** bölümünde **meta veri XML** ve bilgisayarınızda meta veri dosyasını kaydedin.

    ![Sertifika indirme bağlantısı](./media/riskware-tutorial/tutorial_riskware_certificate.png) 

5. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açma Kaydet düğmesi yapılandırın](./media/riskware-tutorial/tutorial_general_400.png)

6. Üzerinde **Riskware yapılandırma** bölümünde **yapılandırma Riskware** açmak için **yapılandırma oturum açma** penceresi. Kopyalama **oturum kapatma URL'si ve SAML çoklu oturum açma hizmeti URL'si** gelen **hızlı başvuru bölümü.**

    ![Riskware yapılandırma](./media/riskware-tutorial/tutorial_riskware_configure.png)

7. Farklı bir web tarayıcı penceresinde Riskware şirketinizin sitesi için bir yönetici olarak oturum açın.

8. Sağ üst kısımdaki tıklayın **Bakım** bakım sayfasını açın.

    ![Riskware yapılandırmaları tutmanız](./media/riskware-tutorial/tutorial_riskware_maintain.png)

9. Bakım sayfasında tıklatın **kimlik doğrulaması**.

    ![Riskware yapılandırma authen](./media/riskware-tutorial/tutorial_riskware_authen.png)

10. İçinde **kimlik doğrulama Yapılandırması** sayfasında, aşağıdaki adımları gerçekleştirin:

    ![Riskware yapılandırma authenconfig](./media/riskware-tutorial/tutorial_riskware_config.png)

    a. Seçin **türü** olarak **SAML** kimlik doğrulaması için.

    b. İçinde **kod** metin AZURE_UAT gibi kod yazın.

    c. İçinde **açıklama** metin SSO için AZURE yapılandırma gibi bir açıklama yazın.

    d. İçinde **tek oturum açma sayfasına** metin kutusu, yapıştırma **SAML çoklu oturum açma hizmeti URL'si** Azure Portalı'ndan kopyaladığınız değeri.

    e. İçinde **sayfa oturum** metin kutusu, yapıştırma **oturum kapatma URL'si** Azure Portalı'ndan kopyaladığınız değeri.

    f. İçinde **Post Form alanı** metin SAMLResponse gibi SAML içeren bir gönderi yanıtı mevcut alan adını yazın

    g. İçinde **XML kimlik etiket adı** metin Nameıd gibi SAML yanıtını benzersiz tanımlayıcı içeren tür özniteliği.

    h. İndirilen açın **meta veri Xml** meta veri dosyası sertifikadan Defteri'nde Azure portalından kopyalayın ve yapıştırın **sertifika** metin kutusu

    i. İçinde **tüketici URL** metin değerini yapıştırın **yanıt URL'si**, hangi destek ekibinden alın.

    j. İçinde **veren** metin değerini yapıştırın **tanımlayıcı**, hangi destek ekibinden alın.

    > [!Note]
    > İlgili kişi [Riskware istemci Destek ekibine](mailto:support@pansoftware.com.au) bu değerleri almak için

    k. Seçin **kullanım POST** onay kutusu.

    m. Seçin **kullanım SAML isteği** onay kutusu.

    m. **Kaydet**’e tıklayın.

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, sol bölmede, tıklayın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/riskware-tutorial/create_aaduser_01.png)

2. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](./media/riskware-tutorial/create_aaduser_02.png)

3. Açmak için **kullanıcı** iletişim kutusu, tıklayın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/riskware-tutorial/create_aaduser_03.png)

4. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/riskware-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** Britta Simon kullanıcı e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değeri yazın **parola** kutusu.

    d. **Oluştur**’a tıklayın.

### <a name="create-a-riskware-test-user"></a>Riskware test kullanıcısı oluşturma

Riskware için oturum açmak Azure AD kullanıcılarının etkinleştirmek için bunların Riskware sağlanması gerekir. Riskware içinde sağlama bir el ile gerçekleştirilen bir görevdir.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1. İçin Riskware bir güvenlik yöneticisi olarak oturum açın.

2. Sağ üst kısımdaki tıklayın **Bakım** bakım sayfasını açın. 

    ![Riskware yapılandırma tutar](./media/riskware-tutorial/tutorial_riskware_maintain.png)

3. Bakım sayfasında tıklatın **kişiler**.

    ![Riskware yapılandırma kişiler](./media/riskware-tutorial/tutorial_riskware_people.png)

4. Seçin **ayrıntıları** sekmesinde ve aşağıdaki adımları gerçekleştirin:

    ![Riskware yapılandırma ayrıntıları](./media/riskware-tutorial/tutorial_riskware_details.png)

    a. Seçin **kişi türü** çalışana gibi.

    b. İçinde **ad** metin gibi kullanıcı adını girin **Britta**.

    c. İçinde **Soyadı** metin gibi kullanıcının soyadını girin **Simon**.

5. Üzerinde **güvenlik** sekmesinde, aşağıdaki adımları gerçekleştirin:

    ![Güvenlik Riskware yapılandırma](./media/riskware-tutorial/tutorial_riskware_security.png)

    a. Altında **kimlik doğrulaması** bölümünden **kimlik doğrulaması** sahip Kurulum modunu SSO için AZURE yapılandırma ister.

    b. Altında **oturum açma ayrıntıları** bölümünde **kullanıcı kimliği** metin gibi kullanıcının e-posta girin **brittasimon@contoso.com**.

    c. İçinde **parola** metin kutusu, kullanıcının parolasını girin.

6. Üzerinde **kuruluş** sekmesinde, aşağıdaki adımları gerçekleştirin:

    ![Kuruluş Riskware yapılandırma](./media/riskware-tutorial/tutorial_riskware_org.png)

    a. Olarak seçeneğini **Level1** kuruluş.

    b. Altında **kişinin birincil çalışma alanı** bölümünde **konumu** metin konumunuz yazın.

    c. Altında **çalışan** bölümünden **çalışan durumu** normal ister.

7. **Kaydet**’e tıklayın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Azure çoklu oturum açma kullanmak için Riskware erişim vererek Britta Simon etkinleştirin.

![Kullanıcı rolü atayın][200]

**Britta Simon Riskware için atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

2. Uygulamalar listesinde **Riskware**.

    ![Uygulamalar listesinde Riskware bağlantı](./media/riskware-tutorial/tutorial_riskware_app.png)  

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı][202]

4. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde Riskware kutucuğa tıkladığınızda, otomatik olarak Riskware uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/riskware-tutorial/tutorial_general_01.png
[2]: ./media/riskware-tutorial/tutorial_general_02.png
[3]: ./media/riskware-tutorial/tutorial_general_03.png
[4]: ./media/riskware-tutorial/tutorial_general_04.png

[100]: ./media/riskware-tutorial/tutorial_general_100.png

[200]: ./media/riskware-tutorial/tutorial_general_200.png
[201]: ./media/riskware-tutorial/tutorial_general_201.png
[202]: ./media/riskware-tutorial/tutorial_general_202.png
[203]: ./media/riskware-tutorial/tutorial_general_203.png

