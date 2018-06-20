---
title: 'Öğretici: Azure Active Directory Tümleştirme Adobe işaretiyle | Microsoft Docs'
description: Çoklu oturum açma Azure Active Directory ve Adobe oturum arasında yapılandırmayı öğrenin.
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
ms.openlocfilehash: 9a1a1f1a1866e5221775d583a9bafe86eef17131
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/19/2018
ms.locfileid: "36221232"
---
# <a name="tutorial-azure-active-directory-integration-with-adobe-sign"></a>Öğretici: Azure Active Directory Tümleştirme Adobe oturum

Bu öğreticide, Azure Active Directory (Azure AD) ile Adobe oturum tümleştirmek öğrenin.

Adobe oturum Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Adobe oturum erişimi, Azure AD'de kontrol edebilirsiniz.
- Otomatik olarak (çoklu oturum açma) Adobe imzalamak için Azure AD hesaplarına açan kullanıcılarınıza etkinleştirebilirsiniz.
- Hesaplarınızı bir merkezi konumda--Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı için bkz: [uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirme Adobe işaretiyle yapılandırmak için gerekir:

- Bir Azure AD aboneliği
- Bir Adobe oturum çoklu oturum açma abonelik etkin

Bu öğreticide adımları test etmek için aşağıdaki önerileri uygulayın:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, şunları yapabilirsiniz [bir aylık deneme sürümünü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Adobe oturum Galeriden ekleniyor.
2. Yapılandırma ve Azure AD sınama çoklu oturum açmayı.

## <a name="add-adobe-sign-from-the-gallery"></a>Galeriden Adobe ekleme
Azure AD Adobe oturum tümleştirilmesi yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden Adobe oturum eklemeniz gerekir.

1. İçinde [Azure portal](https://portal.azure.com), sol bölmede seçin **Azure Active Directory** simgesi. 

    ![Azure Active Directory simgesine ekran görüntüsü][1]

2. Gözat **kurumsal uygulamalar** > **tüm uygulamaları**.

    ![Ekran görüntüsü, Azure Active Directory menülerle Kurumsal uygulamaları ve vurgulanan tüm uygulamalar][2]
    
3. Yeni bir uygulama eklemek için seçin **yeni uygulama** iletişim kutusunun üstündeki.

    ![İletişim kutusunun üstündeki yeni ekran uygulama seçeneği][3]

4. Arama kutusuna **Adobe oturum**.

    ![Arama kutusunun ekran görüntüsü](./media/adobe-echosign-tutorial/tutorial_adobesign_search.png)

5. Sonuçlar panelinde seçin **Adobe oturum**ve ardından **Ekle**.

    ![Sonuçlar bölmesinin ekran görüntüsü](./media/adobe-echosign-tutorial/tutorial_adobesign_addfromgallery.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme
Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma Adobe "Britta Simon." olarak adlandırılan bir test kullanıcı tabanlı işaretiyle test etme

Tekli çalışmaya oturum için bir Azure AD kullanıcısının ve Adobe oturum ilgili kullanıcı arasında bağlı bir ilişki tanımak Azure AD gerekiyor.

Adobe oturum açma bağlantılı ilişkisi oluşturmak için değeri atayın **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı**.

Yapılandırmak ve Azure AD çoklu oturum açma Adobe işaretiyle sınamak için aşağıdaki yapı taşları tamamlayın:

1. [Azure AD çoklu oturum açma yapılandırma](#configuring-azure-ad-single-sign-on) bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. [Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user) Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. [Adobe oturum test kullanıcısı oluşturma](#creating-an-adobe-sign-test-user) Britta Simon, karşılık gelen kimin kullanıcı Azure AD gösterimini bağlı Adobe oturum sağlamak için.
4. [Azure AD test kullanıcısı atayın](#assigning-the-azure-ad-test-user) Britta Azure AD çoklu oturum açma kullanmak Simon etkinleştirmek için.
5. [Test çoklu oturum açma](#testing-single-sign-on) yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma Adobe oturum uygulamanızda yapılandırın.

1. Azure portalında üzerinde **Adobe oturum** uygulama tümleştirmesi sayfasında, **çoklu oturum açma**.

    ![Çoklu oturum açma vurgulanmış özelliğini ile Adobe oturum ekran uygulama tümleştirme sayfası][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusu için **modu**seçin **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.

    ![Vurgulanan modu kutusuyla tek ekran oturum açma iletişim kutusu](./media/adobe-echosign-tutorial/tutorial_adobesign_samlbase.png)

3. İçinde **Adobe oturum etki alanı ve URL'leri** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Adobe oturum etki ekran ve URL'ler bölümü](./media/adobe-echosign-tutorial/tutorial_adobesign_url.png)

    a. İçinde **oturum açma URL'si** metin kutusunda, aşağıdaki desen kullanan bir URL yazın: `https://<companyname>.echosign.com/`

    b. İçinde **tanımlayıcısı** metin kutusunda, aşağıdaki desen kullanan bir URL yazın: `https://<companyname>.echosign.com`

    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerleri tanımlayıcısı ve gerçek oturum açma URL'si ile güncelleştirin. Kişi [Adobe oturum istemci destek ekibi](https://helpx.adobe.com/in/contact/support.html) bu değerleri almak için.

4. İçinde **SAML imzalama sertifikası** bölümünde, select **Certificate(Base64)** ve ardından sertifika dosyayı bilgisayarınıza kaydedin.

    ![Ekran görüntüsü, SAML imzalama sertifikası bölümü](./media/adobe-echosign-tutorial/tutorial_adobesign_certificate.png) 

5. **Kaydet**’i seçin.

    ![Ekran Kaydet düğmesi](./media/adobe-echosign-tutorial/tutorial_general_400.png)

6. İçinde **Adobe oturum yapılandırma** bölümünde, select **yapılandırma Adobe oturum** açmak için **yapılandırma oturum açma** penceresi. Kopya **Sign-Out URL**, **SAML varlık kimliği**, ve **SAML çoklu oturum açma hizmet URL'si** gelen **hızlı başvuru** bölümü.

    ![Adobe oturum yapılandırması ekran bölümünde, Adobe vurgulanan oturum yapılandırma](./media/adobe-echosign-tutorial/tutorial_adobesign_configure.png)

7. Yapılandırma önce başvurun [Adobe oturum istemci destek ekibi](https://helpx.adobe.com/in/contact/support.html) güvenilir listeye Adobe oturum etki alanınızda. Etki alanı ekleme şöyledir:

    a. [Adobe oturum istemci destek ekibi](https://helpx.adobe.com/in/contact/support.html) rastgele oluşturulmuş bir belirteç gönderir. Etki alanınız için belirteç şu şekilde olacaktır: **adobe oturum doğrulama xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx =**

    b. Doğrulama belirteci bir DNS metin kayıtta yayımlama ve bildirim [Adobe oturum istemci destek ekibi](https://helpx.adobe.com/in/contact/support.html).
    
    > [!NOTE]
    > Bu birkaç gün veya daha uzun sürebilir. Bir saat veya daha fazla bilgi için bir değer DNS'de yayımlanan DNS yayma gecikmeleri anlamına Not görünmeyebilir. BT yöneticiniz bu belirteci bir DNS metin kayıtta yayımlama hakkında bilgi sahibi olmalıdır.
    
    c. Ne zaman sizi uyarır [Adobe oturum istemci destek ekibi](https://helpx.adobe.com/in/contact/support.html) destek bileti aracılığıyla belirteç yayımlandıktan sonra bunlar etki alanı doğrulanamıyor ve hesabınıza ekleme.
    
    d. Genellikle, bir DNS kaydı belirteçte yayımlama şöyledir:

    * Etki alanı hesabınızla oturum açın
    * DNS kaydını güncelleştirmek için sayfasını bulun. Bu sayfayı DNS Yönetimi, ad sunucusu yönetimi veya Gelişmiş ayarları olarak da adlandırılabilir.
    * TXT kayıtlarının etki alanınız için bulun.
    * Adobe tarafından sağlanan tam belirteç değeri ile bir TXT kaydı ekleyin.
    * Yaptığınız değişiklikleri kaydedin.

8. Farklı web tarayıcısı penceresinde Adobe oturum şirket sitenize yönetici olarak oturum açın.

9. SAML menüde seçin **hesap ayarlarını** > **SAML ayarları**.
   
    ![Ekran Adobe oturum SAML Ayarları sayfası](./media/adobe-echosign-tutorial/ic789520.png "hesabı")

10. İçinde **SAML ayarları** bölümünde, aşağıdaki adımları gerçekleştirin:
  
    ![SAML ayarlarının ekran görüntüsüne](./media/adobe-echosign-tutorial/ic789521.png "SAML ayarları")
   
    a. Altında **SAML modu**seçin **SAML zorunlu**.
   
    b. Seçin **Echosign kimlik bilgilerini kullanarak oturum açması Echosign hesap yöneticileri izin**.
   
    c. Altında **kullanıcı oluşturma**seçin **otomatik olarak SAML kimliği doğrulanmış kullanıcılar eklemek**.

    d. Yapıştır **SAML varlık kimliği**, Azure portalından kopyalanan **varlık kimliği/sertifika veren URL** metin kutusu.
    
    e. Yapıştır **SAML çoklu oturum açma hizmet URL'si**, Azure portalından kopyalanan **oturum açma URL'si/SSO Endpoint** metin kutusu.
   
    f. Yapıştır **Sign-Out URL**, Azure portalından kopyalanan **oturum kapatma URL'si/SLO Endpoint** metin kutusu.

    g. İndirilen açmak **Certificate(Base64)** dosyasını Not Defteri'nde. İçeriğini, panoya kopyalayın ve yapıştırın kendisine **IDP sertifika** metin kutusu.

    h. Seçin **değişiklikleri kaydetmek**.

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Azure portalında Britta Simon adlı bir test kullanıcı oluşturmaktır.

![Azure portalında test kullanıcı adının ekran görüntüsü][100]

1. İçinde **Azure portal**, sol bölmede seçin **Azure Active Directory** simgesi.

    ![Azure AD simgesinin ekran görüntüsü](./media/adobe-echosign-tutorial/create_aaduser_01.png) 

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar**seçip **tüm kullanıcılar**.
    
    ![Azure ekran AD menülerle kullanıcılarını, gruplarını ve vurgulanan tüm kullanıcılar](./media/adobe-echosign-tutorial/create_aaduser_02.png) 

3. Açmak için **kullanıcı** iletişim kutusunda **Ekle**.
 
    ![Vurgulanan Ekle seçeneği ile tüm kullanıcılar iletişim kutusunun üstündeki ekran görüntüsü](./media/adobe-echosign-tutorial/create_aaduser_03.png) 

4. Üzerinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:
 
    ![Ekran görüntüsü, kullanıcı iletişim kutusu](./media/adobe-echosign-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusunda, **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusunda, BrittaSimon e-posta adresini yazın.

    c. Seçin **Göster parola**ve değerini yazma **parola**.

    d. **Oluştur**’u seçin.
 
### <a name="create-an-adobe-sign-test-user"></a>Adobe oturum test kullanıcısı oluşturma

Azure AD Adobe imzalamak için oturum açmalarını etkinleştirmek için bunlar Adobe oturum açın sağlanmalıdır. Bu el ile bir görevdir.

>[!NOTE]
>Azure AD kullanıcı hesaplarını sağlamak için herhangi bir Adobe oturum kullanıcı hesabı oluşturma araçlarını veya Adobe oturum tarafından sağlanan API'leri kullanabilirsiniz. 

1. Oturum açın, **Adobe oturum** yönetici olarak şirket site.

2. Üstteki menüde seçin **hesap**. Sol bölmede, ardından **kullanıcıları ve grupları** > **yeni bir kullanıcı oluşturmak**.
   
    ![Adobe oturum ekran vurgulanmış yeni bir kullanıcı oluşturmak ve şirket hesabı, kullanıcılar ve gruplar ile site](./media/adobe-echosign-tutorial/ic789524.png "hesabı")
   
3. İçinde **yeni kullanıcı oluştur** bölümünde, aşağıdaki adımları gerçekleştirin:
   
    ![Ekran, yeni kullanıcı oluştur bölüm](./media/adobe-echosign-tutorial/ic789525.png "kullanıcı oluştur")
   
    a. Türü **e-posta adresi**, **ad**, ve **Soyadı** geçerli bir Azure AD hesabının istediğiniz ilgili metin kutularına sağlamak için.
   
    b. Seçin **kullanıcı oluşturma**.

>[!NOTE]
>Azure Active Directory hesap sahibi etkin duruma gelmesi hesabı onaylamak için bir bağlantı içeren bir e-posta alır. 

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Adobe imzalamak için erişim vererek, Azure çoklu oturum açma, kullanılacak Britta Simon etkinleştirin.

![Ekran görüntüsü, Azure portal çoklu oturum açma][200] 

1. Azure portalında uygulamaları görünümünü açın. Dizin görünümüne göz atın, Git **kurumsal uygulamalar**seçip **tüm uygulamaları**.

    ![Kurumsal uygulamaları ve vurgulanan tüm uygulamaları ile ekran görüntüsü, Azure portal uygulamaları görüntüle][201] 

2. Uygulamalar listesinde **Adobe oturum**.

    ![Adobe vurgulanan oturum uygulamalar listesinin ekran görüntüsü](./media/adobe-echosign-tutorial/tutorial_adobesign_app.png) 

3. Soldaki menüde seçin **kullanıcılar ve gruplar**.

    ![Kullanıcılar ve gruplarla vurgulanmış menüsünün ekran görüntüsü][202] 

4. **Add (Ekle)** seçeneğini belirleyin. Ardından **eklemek atama** bölümünde, select **kullanıcılar ve gruplar**.

    ![Ekran görüntüsü, kullanıcılar ve Gruplar sayfasında ve atama Bölüm Ekle][203]

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda, kullanıcılar listesinde **Britta Simon**.

6. İçinde **kullanıcılar ve gruplar** iletişim kutusu, tıklatın **seçin**.

7. İçinde **eklemek atama** iletişim kutusunda **atamak**.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Erişim panelinde Adobe oturum döşeme seçtiğinizde, oturumunuz otomatik olarak Adobe oturum uygulamanıza. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](../manage-apps/what-is-single-sign-on.md)

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
