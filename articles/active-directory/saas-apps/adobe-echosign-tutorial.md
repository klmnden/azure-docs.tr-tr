---
title: 'Öğretici: Azure Active Directory Tümleştirme ile Adobe Sign | Microsoft Docs'
description: Azure Active Directory ve Adobe Sign arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: f9385723-8fe7-4340-8afb-1508dac3e92b
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2018
ms.author: jeedes
ms.openlocfilehash: d5cdc2ec0c6cfcf52f84629485d0dd879fbf6fa2
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/14/2018
ms.locfileid: "39054007"
---
# <a name="tutorial-azure-active-directory-integration-with-adobe-sign"></a>Öğretici: Azure Active Directory Adobe Sign ile'tümleştirme

Bu öğreticide, Adobe Sign'ı Azure Active Directory (Azure AD) ile tümleştirme konusunda bilgi edinin.

Adobe Sign'ı Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Adobe Sign erişimi, Azure AD'de kontrol edebilirsiniz.
- Otomatik olarak imzalanan için Adobe Sign (çoklu oturum açma) ile Azure AD hesaplarına açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda--Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı için bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile Adobe Sign yapılandırmak için gerekir:

- Azure AD aboneliğiniz
- Bir Adobe Sign çoklu oturum açma abonelik etkin.

Bu öğreticideki adımları test etmek için aşağıdaki önerileri uygulayın:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, şunları yapabilirsiniz [bir aylık deneme sürümü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Adobe Sign Galeriden ekleniyor.
2. Yapılandırma ve test Azure AD çoklu oturum açma.

## <a name="add-adobe-sign-from-the-gallery"></a>Adobe Sign Galeriden Ekle
Azure AD'de Adobe Sign tümleştirmesini yapılandırmak için Adobe Sign Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

1. İçinde [Azure portalında](https://portal.azure.com), sol bölmede seçin **Azure Active Directory** simgesi. 

    ![Azure Active Directory simgesinin görüntüsü][1]

2. Gözat **kurumsal uygulamalar** > **tüm uygulamaları**.

    ![Kurumsal uygulamaları ve tüm uygulamaların vurgulandığı ekran görüntüsü, Azure Active Directory menüleri][2]
    
3. Yeni bir uygulama eklemek için seçin **yeni uygulama** iletişim kutusunun üst.

    ![İletişim kutusunun üst kısmındaki Yeni ekran uygulama seçeneği][3]

4. Arama kutusuna **Adobe Sign**.

    ![Arama kutusunun ekran görüntüsü](./media/adobe-echosign-tutorial/tutorial_adobesign_search.png)

5. Sonuçlar panelinde seçin **Adobe Sign**ve ardından **Ekle**.

    ![Sonuçlar bölmesinin ekran görüntüsü](./media/adobe-echosign-tutorial/tutorial_adobesign_addfromgallery.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme
Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon." adlı bir test kullanıcı tabanlı Adobe Sign'ile test etme

Tek çalışmak için oturum açma için Azure AD arasında bir Azure AD kullanıcısı ve Adobe Sign ilgili kullanıcı bağlı bir ilişki tanıması gerekir.

Adobe oturum açma bağlı ilişki kurmak için değerini atayın **kullanıcı adı** değerini Azure AD'de **Username**.

Yapılandırma ve Azure AD çoklu oturum açma Adobe Sign ile'test etmek için aşağıdaki yapı taşlarını tamamlayın:

1. [Azure AD çoklu oturum açmayı yapılandırma](#configuring-azure-ad-single-sign-on) kullanıcılarınız bu özelliği kullanmak etkinleştirmek için.
2. [Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user) Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. [Bir Adobe Sign test kullanıcısı oluşturma](#creating-an-adobe-sign-test-user) Britta simon'un bir karşılığı olan kullanıcı Azure AD gösterimini bağlı bir Adobe Sign sağlamak için.
4. [Azure AD test kullanıcı atama](#assigning-the-azure-ad-test-user) Britta Simon, Azure AD çoklu oturum açma kullanmak üzere etkinleştirmek için.
5. [Çoklu oturum açmayı test](#testing-single-sign-on) yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve Adobe Sign uygulamanızda çoklu oturum açmayı yapılandırın.

1. Azure portalında, üzerinde **Adobe Sign** uygulama tümleştirme sayfasında **çoklu oturum açma**.

    ![Çoklu oturum vurgulanmış açma Adobe Sign ekran uygulama tümleştirme sayfası][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusu için **modu**seçin **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.

    ![Vurgulanan modu kutusu tek ekran oturum açma iletişim kutusu](./media/adobe-echosign-tutorial/tutorial_adobesign_samlbase.png)

3. İçinde **Adobe oturum etki alanı ve URL'ler** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Ekran görüntüsü, Adobe oturum etki alanı ve URL'ler bölümü](./media/adobe-echosign-tutorial/tutorial_adobesign_url.png)

    a. İçinde **oturum açma URL'si** metin kutusunda, aşağıdaki desen kullanan bir URL yazın: `https://<companyname>.echosign.com/`

    b. İçinde **tanımlayıcı** metin kutusunda, aşağıdaki desen kullanan bir URL yazın: `https://<companyname>.echosign.com`

    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerleri tanımlayıcısı ve gerçek oturum açma URL'si ile güncelleştirin. İlgili kişi [Adobe oturum istemci Destek ekibine](https://helpx.adobe.com/in/contact/support.html) bu değerleri almak için.

4. İçinde **SAML imzalama sertifikası** bölümünden **Certificate(Base64)** ve bilgisayarınızdaki sertifika dosyasını kaydedin.

    ![Ekran görüntüsü, SAML imzalama sertifikası bölümü](./media/adobe-echosign-tutorial/tutorial_adobesign_certificate.png) 

5. **Kaydet**’i seçin.

    ![Ekran Kaydet düğmesi](./media/adobe-echosign-tutorial/tutorial_general_400.png)

6. İçinde **Adobe oturum yapılandırma** bölümünden **Adobe Sign'ı yapılandırma** açmak için **yapılandırma oturum açma** penceresi. Kopyalama **oturum kapatma URL'si**, **SAML varlık kimliği**, ve **SAML çoklu oturum açma hizmeti URL'si** gelen **hızlı başvuru** bölümü.

    ![Adobe oturum yapılandırması ekran görüntüsü bölümünde, Adobe vurgulanan oturum yapılandırma](./media/adobe-echosign-tutorial/tutorial_adobesign_configure.png)

7. Yapılandırma önce başvurun [Adobe oturum istemci Destek ekibine](https://helpx.adobe.com/in/contact/support.html) beyaz liste için Adobe Sign etki alanınızda. Etki alanı ekleme şöyledir:

    a. [Adobe oturum istemci Destek ekibine](https://helpx.adobe.com/in/contact/support.html) rastgele oluşturulmuş bir belirteç gönderir. Etki alanınız için belirteci aşağıdaki gibi olacaktır: **adobe oturum doğrulama xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx =**

    b. Doğrulama belirteci DNS metin kaydı yayımlama ve bildirim [Adobe oturum istemci Destek ekibine](https://helpx.adobe.com/in/contact/support.html).
    
    > [!NOTE]
    > Bu birkaç gün veya daha uzun sürebilir. Bir saat veya daha fazla bilgi için bir değer DNS'de yayımlanan DNS yayma gecikmeleri anlamına Not görünmeyebilir. BT yöneticiniz bu belirteci DNS metin kaydı yayımlama hakkında bilgi sahibi olması gerekir.
    
    c. Ne zaman size bildirim [Adobe oturum istemci Destek ekibine](https://helpx.adobe.com/in/contact/support.html) üzerinden destek bileti belirteç yayımlandıktan sonra etki alanını doğrulama ve hesabınıza ekleyin.
    
    d. Genellikle, bir DNS kaydı belirteçte yayımlama şöyledir:

    * Etki alanı hesabınızda oturum açın
    * DNS kaydı güncelleştirmek için sayfayı bulun. Bu sayfada, DNS Yönetimi, ad sunucusu yönetimi veya Gelişmiş ayarları çağrılabilir.
    * TXT kayıtlarının, etki alanınız için bulun.
    * Adobe tarafından sağlanan tam belirteç değeri içeren bir TXT kaydı ekleyin.
    * Yaptığınız değişiklikleri kaydedin.

8. Farklı bir web tarayıcı penceresinde bir Adobe Sign şirketinizin sitesi için bir yönetici olarak oturum açın.

9. SAML menüde **hesap ayarları** > **SAML ayarlarını**.
   
    ![Ekran Adobe oturum SAML Ayarları sayfası](./media/adobe-echosign-tutorial/ic789520.png "hesabı")

10. İçinde **SAML ayarlarını** bölümünde, aşağıdaki adımları gerçekleştirin:
  
    ![SAML ayarlarını görüntüsü](./media/adobe-echosign-tutorial/ic789521.png "SAML ayarları")
   
    a. Altında **SAML modu**seçin **SAML zorunlu**.
   
    b. Seçin **izin Echosign hesap yöneticileri Echosign kimlik bilgilerini kullanarak oturum açması**.
   
    c. Altında **kullanıcı oluşturma**seçin **SAML ile kimliği doğrulanmış kullanıcılar otomatik olarak Ekle**.

    d. Yapıştırma **SAML varlık kimliği**, hangi Azure portaldan kopyaladığınız **varlık kimliği/veren URL'si** metin kutusu.
    
    e. Yapıştırma **SAML çoklu oturum açma hizmeti URL'si**, Azure portalından kopyalanan **oturum açma URL'si/SSO'ya uç noktası** metin kutusu.
   
    f. Yapıştırma **oturum kapatma URL'si**, hangi Azure portaldan kopyaladığınız **oturum kapatma URL'si/SLO uç noktasına** metin kutusu.

    g. İndirilen açın **Certificate(Base64)** dosyasını Not Defteri'nde. İçeriğini sizin panoya kopyalayın ve yapıştırın kendisine **IDP sertifika** metin kutusu.

    h. Seçin **değişiklikleri kaydetmek**.

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Azure portalında Britta Simon adlı bir test kullanıcısı oluşturmaktır.

![Test kullanıcı adının Azure portalının ekran görüntüsü][100]

1. İçinde **Azure portalında**, sol bölmede seçin **Azure Active Directory** simgesi.

    ![Azure AD simgesinin görüntüsü](./media/adobe-echosign-tutorial/create_aaduser_01.png) 

2. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar**seçip **tüm kullanıcılar**.
    
    ![Kullanıcılar, gruplar ve vurgulanmış tüm kullanıcılar Azure ekran AD menüleri](./media/adobe-echosign-tutorial/create_aaduser_02.png) 

3. Açmak için **kullanıcı** iletişim kutusunda **Ekle**.
 
    ![Üst ekle seçeneğinin vurgulandığı ile tüm kullanıcılar iletişim kutusunun ekran görüntüsü](./media/adobe-echosign-tutorial/create_aaduser_03.png) 

4. Üzerinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:
 
    ![Ekran kullanıcı iletişim kutusu](./media/adobe-echosign-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusunda, **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusunda, BrittaSimon e-posta adresini yazın.

    c. Seçin **Göster parola**ve değerini yazma **parola**.

    d. **Oluştur**’u seçin.
 
### <a name="create-an-adobe-sign-test-user"></a>Bir Adobe Sign test kullanıcısı oluşturma

Adobe Sign'için oturum açmak Azure AD kullanıcılarının etkinleştirmek için Adobe oturum sağlanması gerekir. Bu el ile gerçekleştirilen bir görevdir.

>[!NOTE]
>Azure AD kullanıcı hesapları sağlamak için herhangi bir Adobe Sign kullanıcı hesabı oluşturma araçları veya Adobe Sign tarafından sağlanan API'leri kullanabilirsiniz. 

1. Oturum açın, **Adobe Sign** yönetici olarak şirketin site.

2. Üst menüde **hesabı**. Sol bölmede, ardından **kullanıcıları ve grupları** > **yeni kullanıcı oluşturma**.
   
    ![Adobe Sign ekran şirket hesabı, kullanıcıları ve grupları, site ve vurgulanmış yeni kullanıcı oluşturma](./media/adobe-echosign-tutorial/ic789524.png "hesabı")
   
3. İçinde **yeni kullanıcı oluşturma** bölümünde, aşağıdaki adımları gerçekleştirin:
   
    ![Ekran görüntüsü, oluşturma yeni kullanıcı bölümünün](./media/adobe-echosign-tutorial/ic789525.png "kullanıcı oluştur")
   
    a. Tür **e-posta adresi**, **ad**, ve **Soyadı** geçerli bir Azure AD hesabı ilgili metin kutularına sağlamak istediğiniz.
   
    b. Seçin **oluşturacağı**.

>[!NOTE]
>Azure Active Directory hesap sahibinin önce etkin hale gelir ve hesabı onaylamak için bir bağlantı içeren bir e-posta alır. 

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Adobe Sign'için erişim izni verdiğinizde, Azure çoklu oturum açma, kullanılacak Britta Simon etkinleştirin.

![Ekran görüntüsü, Azure portal çoklu oturum açma][200] 

1. Azure portalında uygulama görünümünü açın. Ardından dizin görünümüne gidin, Git **kurumsal uygulamalar**seçip **tüm uygulamaları**.

    ![Kurumsal uygulamaları ve tüm uygulamaların vurgulandığı ekran görüntüsü, Azure portal uygulamaları görüntüle][201] 

2. Uygulamalar listesinde **Adobe Sign**.

    ![Adobe Sign'ın vurgulandığı ekran görüntüsü uygulamalar listesi](./media/adobe-echosign-tutorial/tutorial_adobesign_app.png) 

3. Soldaki menüde **kullanıcılar ve gruplar**.

    ![Kullanıcılar ve gruplarla vurgulanmış menüsünün ekran görüntüsü][202] 

4. **Add (Ekle)** seçeneğini belirleyin. Ardından **atama Ekle** bölümünden **kullanıcılar ve gruplar**.

    ![Ekran görüntüsü, kullanıcılar ve Gruplar sayfasında ve atama bölümü ekleyin][203]

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda, kullanıcılar listesinden **Britta Simon**.

6. İçinde **kullanıcılar ve gruplar** iletişim kutusu, tıklayın **seçin**.

7. İçinde **atama Ekle** iletişim kutusunda **atama**.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Erişim Paneli'nde Adobe Sign kutucuğu seçtiğinizde, oturumunuz otomatik olarak Adobe Sign uygulamanıza. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/adobe-echosign-tutorial/tutorial_general_01.png
[2]: ./media/adobe-echosign-tutorial/tutorial_general_02.png
[3]: ./media/adobe-echosign-tutorial/tutorial_general_03.png
[4]: ./media/adobe-echosign-tutorial/tutorial_general_04.png

[100]: ./media/adobe-echosign-tutorial/tutorial_general_100.png

[200]: ./media/adobe-echosign-tutorial/tutorial_general_200.png
[201]: ./media/adobe-echosign-tutorial/tutorial_general_201.png
[202]: ./media/adobe-echosign-tutorial/tutorial_general_202.png
[203]: ./media/adobe-echosign-tutorial/tutorial_general_203.png
