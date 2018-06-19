---
title: 'Öğretici: Azure Active Directory Tümleştirme ile Envi MMIS | Microsoft Docs'
description: Çoklu oturum açma Azure Active Directory ile Envi MMIS arasında yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: ab89f8ee-2507-4625-94bc-b24ef3d5e006
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/03/2018
ms.author: jeedes
ms.openlocfilehash: 008ba69384cd52d4d2d75978aec1602e1c2cce79
ms.sourcegitcommit: c851842d113a7078c378d78d94fea8ff5948c337
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2018
ms.locfileid: "35979821"
---
# <a name="tutorial-azure-active-directory-integration-with-envi-mmis"></a>Öğretici: Azure Active Directory Tümleştirme Envi MMIS ile

Bu öğreticide, Azure Active Directory (Azure AD) ile Envi MMIS tümleştirmek öğrenin.

Envi MMIS Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Envi MMIS erişimi, Azure AD'de kontrol edebilirsiniz.
- Azure AD hesaplarına otomatik olarak (çoklu oturum açma) için Envi MMIS açan kullanıcılarınıza etkinleştirebilirsiniz.
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirme Envi MMIS ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir Envi MMIS çoklu oturum açma abonelik etkin

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, şunları yapabilirsiniz [bir aylık deneme sürümünü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden Envi MMIS ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-envi-mmis-from-the-gallery"></a>Galeriden Envi MMIS ekleme
Azure AD Envi MMIS tümleştirilmesi yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden Envi MMIS eklemeniz gerekir.

**Galeriden Envi MMIS eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **Envi MMIS**seçin **Envi MMIS** sonuç panelinden ardından **Ekle** uygulama eklemek için düğmeyi.

    ![Sonuçlar listesinde Envi MMIS](./media/envimmis-tutorial/tutorial_envimmis_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırmanız ve Envi MMIS ile Azure AD çoklu oturum açmayı test "Britta Simon" adlı bir test kullanıcı tabanlı.

Tekli çalışmaya oturum için Azure AD Envi MMIS karşılık gelen kullanıcı için bir kullanıcı Azure AD'de nedir bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının Envi MMIS ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma Envi MMIS ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Bir Envi MMIS test kullanıcısı oluşturma](#create-an-envi-mmis-test-user)**  - Britta Simon, karşılık gelen kullanıcı Azure AD gösterimini bağlı Envi MMIS sağlamak için.
4. **[Azure AD test kullanıcısı atayın](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Test çoklu oturum açma](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma Envi MMIS uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma Envi MMIS ile yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **Envi MMIS** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/envimmis-tutorial/tutorial_envimmis_samlbase.png)

3. Üzerinde **Envi MMIS etki alanı ve URL'leri** bölümünde, uygulamada yapılandırmak istiyorsanız aşağıdaki adımları gerçekleştirin **IDP** modu tarafından başlatılan:

    ![Envi MMIS etki alanı ve URL'leri tek oturum açma bilgileri](./media/envimmis-tutorial/tutorial_envimmis_url.png)

    a. İçinde **tanımlayıcısı** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://www.<CUSTOMER DOMAIN>.com/Account`

    b. İçinde **yanıt URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://www.<CUSTOMER DOMAIN>.com/Account/Acs`

4. Denetleme **Göster Gelişmiş URL ayarları** ve uygulamada yapılandırmak istiyorsanız aşağıdaki adımı gerçekleştirin **SP** modunda başlatılan:

    ![Envi MMIS etki alanı ve URL'leri tek oturum açma bilgileri](./media/envimmis-tutorial/tutorial_envimmis_url1.png)

    İçinde **oturum açma URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://www.<CUSTOMER DOMAIN>.com/Account`
     
    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerler, gerçek tanımlayıcı, yanıt URL'si ve oturum açma URL'si ile güncelleştirin. Kişi [Envi MMIS istemci destek ekibi](mailto:support@ioscorp.com) bu değerleri almak için.

5. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **meta veri XML** ve meta veri dosyası, bilgisayarınıza kaydedin.

    ![Sertifika indirme bağlantısı](./media/envimmis-tutorial/tutorial_envimmis_certificate.png) 

6. Tıklatın **kaydetmek** düğmesi.

    ![Oturum açma tek Kaydet düğmesi yapılandırın](./media/envimmis-tutorial/tutorial_general_400.png)

7. Farklı web tarayıcısı penceresinde Envi MMIS sitenize yönetici olarak oturum açın.

8. Tıklayın **My etki alanı** sekmesi.

    ![Oturum açma tek Kaydet düğmesi yapılandırın](./media/envimmis-tutorial/configure1.png)

9. **Düzenle**’ye tıklayın.

    ![Oturum açma tek Kaydet düğmesi yapılandırın](./media/envimmis-tutorial/configure2.png)

10. Seçin **uzaktan kimlik doğrulamasını kullan** onay kutusunu ve ardından **HTTP yeniden yönlendirme** gelen **kimlik doğrulama türü** açılır.

    ![Oturum açma tek Kaydet düğmesi yapılandırın](./media/envimmis-tutorial/configure3.png)

11. Seçin **kaynakları** sekmesini ve sonra **meta veriler karşıya**.

    ![Oturum açma tek Kaydet düğmesi yapılandırın](./media/envimmis-tutorial/configure4.png)

12. İçinde **meta veriler karşıya** açılan, aşağıdaki adımları gerçekleştirin:

    ![Oturum açma tek Kaydet düğmesi yapılandırın](./media/envimmis-tutorial/configure5.png)

    a. Seçin **dosya** gelen seçeneği **karşıya gelen** açılır.

    b. Azure Portalı'ndan indirilen meta veri dosyası seçerek karşıya **dosya simgesini seçin**.

    c. **Tamam**’a tıklayın.

13. İndirilen meta veri dosyası dosyalarını karşıya yükledikten sonra alanları otomatik olarak doldurulacak. Tıklatın **güncelleştirme**

    ![Oturum açma tek Kaydet düğmesi yapılandırın](./media/envimmis-tutorial/configure6.png)

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Sol bölmede, Azure portal'ı tıklatın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/envimmis-tutorial/create_aaduser_01.png)

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantılar](./media/envimmis-tutorial/create_aaduser_02.png)

3. Açmak için **kullanıcı** iletişim kutusu, tıklatın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/envimmis-tutorial/create_aaduser_03.png)

4. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/envimmis-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** kullanıcı Britta Simon e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değer aşağı yazma **parola** kutusu.

    d. **Oluştur**’a tıklayın.
 
### <a name="create-an-envi-mmis-test-user"></a>Bir Envi MMIS test kullanıcısı oluşturma

Azure AD kullanıcılarının Envi MMIS oturum açmayı etkinleştirmek için bunların Envi MMIS sağlanmalıdır.  
Envi MMIS söz konusu olduğunda, sağlama bir el ile bir görevdir.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1. Envi MMIS şirket sitenize yönetici olarak oturum açın.

2. Tıklayın **kullanıcı listesinde** sekmesi.

    ![Çalışanı ekleyin](./media/envimmis-tutorial/user1.png)

3. Tıklatın **Kullanıcı Ekle** düğmesi.

    ![Çalışanı ekleyin](./media/envimmis-tutorial/user2.png)

4. İçinde **Kullanıcı Ekle** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çalışanı ekleyin](./media/envimmis-tutorial/user3.png)

    a. İçinde **kullanıcı adı** Britta Simon hesap türü kullanıcı adı metin kutusuna, ister **brittasimon@contoso.com**.
    
    b. İçinde **ad** BrittaSimon türü adı metin kutusuna, ister **Britta**.

    c. İçinde **Soyadı** BrittaSimon Soyadı türünü metin kutusuna, ister **Simon**.

    d. Kullanıcının bir başlık girin **başlık** TextBox.
    
    e. İçinde **e-posta adresi** Britta Simon hesap türü e-posta adresi metin kutusuna, ister **brittasimon@contoso.com**.

    f. İçinde **SSO kullanıcı adı** Britta Simon hesap türü kullanıcı adı metin kutusuna, ister **brittasimon@contoso.com**.

    g. **Kaydet**’e tıklayın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Britta Envi MMIS erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**Envi MMIS Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **Envi MMIS**.

    ![Uygulamalar listesinde Envi MMIS bağlantı](./media/envimmis-tutorial/tutorial_envimmis_app.png)  

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantı][202]

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Ekleme atama bölmesi][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli Envi MMIS parçasında tıklattığınızda, otomatik olarak Envi MMIS uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/envimmis-tutorial/tutorial_general_01.png
[2]: ./media/envimmis-tutorial/tutorial_general_02.png
[3]: ./media/envimmis-tutorial/tutorial_general_03.png
[4]: ./media/envimmis-tutorial/tutorial_general_04.png

[100]: ./media/envimmis-tutorial/tutorial_general_100.png

[200]: ./media/envimmis-tutorial/tutorial_general_200.png
[201]: ./media/envimmis-tutorial/tutorial_general_201.png
[202]: ./media/envimmis-tutorial/tutorial_general_202.png
[203]: ./media/envimmis-tutorial/tutorial_general_203.png

