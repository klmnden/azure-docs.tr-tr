---
title: 'Öğretici: Azure Active Directory tümleştirmesiyle Envi MMIS | Microsoft Docs'
description: Azure Active Directory ve Envi MMIS arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: ab89f8ee-2507-4625-94bc-b24ef3d5e006
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/03/2018
ms.author: jeedes
ms.openlocfilehash: 70066f1c29849b77c67710eb908ef2a340cdc45f
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/14/2018
ms.locfileid: "39047669"
---
# <a name="tutorial-azure-active-directory-integration-with-envi-mmis"></a>Öğretici: Azure Active Directory Envi MMIS ile tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile Envi MMIS tümleştirme konusunda bilgi edinin.

Azure AD ile Envi MMIS tümleştirme ile aşağıdaki avantajları sağlar:

- Envi MMIS erişimi, Azure AD'de kontrol edebilirsiniz.
- Azure AD hesaplarına otomatik olarak imzalanan (çoklu oturum açma) için Envi MMIS açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile Envi MMIS yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliğiniz
- Abonelik bir Envi MMIS çoklu oturum açma etkin

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, şunları yapabilirsiniz [bir aylık deneme sürümü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden Envi MMIS ekleme
2. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-envi-mmis-from-the-gallery"></a>Galeriden Envi MMIS ekleme
Azure AD'de Envi MMIS tümleştirmesini yapılandırmak için Envi MMIS Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden Envi MMIS eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **Envi MMIS**seçin **Envi MMIS** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Sonuç listesinde Envi MMIS](./media/envimmis-tutorial/tutorial_envimmis_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırmanız ve Envi MMIS ile Azure AD çoklu oturum açmayı test "Britta Simon" adlı bir test kullanıcı tabanlı.

Tek iş için oturum açma için Azure AD ne Envi MMIS karşılığı kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısının Envi MMIS ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma Envi MMIS ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Bir Envi MMIS test kullanıcısı oluşturma](#create-an-envi-mmis-test-user)**  - kullanıcı Azure AD gösterimini bağlı Envi MMIS Britta simon'un bir karşılığı vardır.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve Envi MMIS uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma ile Envi MMIS yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **Envi MMIS** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/envimmis-tutorial/tutorial_envimmis_samlbase.png)

3. Üzerinde **Envi MMIS etki alanı ve URL'ler** bölümünde, uygulamada yapılandırmak istiyorsanız aşağıdaki adımları gerçekleştirin **IDP** başlatılan modu:

    ![Envi MMIS etki alanı ve URL'ler tek oturum açma bilgileri](./media/envimmis-tutorial/tutorial_envimmis_url.png)

    a. İçinde **tanımlayıcı** metin kutusuna bir URL şu biçimi kullanarak: `https://www.<CUSTOMER DOMAIN>.com/Account`

    b. İçinde **yanıt URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://www.<CUSTOMER DOMAIN>.com/Account/Acs`

4. Denetleme **Gelişmiş URL ayarlarını göster** ve uygulamada yapılandırmak istiyorsanız, aşağıdaki adımı uygulayın **SP** başlatılan modu:

    ![Envi MMIS etki alanı ve URL'ler tek oturum açma bilgileri](./media/envimmis-tutorial/tutorial_envimmis_url1.png)

    İçinde **oturum açma URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://www.<CUSTOMER DOMAIN>.com/Account`
     
    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerler gerçek tanımlayıcısı, yanıt URL'si ve oturum açma URL'si ile güncelleştirin. İlgili kişi [Envi MMIS istemci Destek ekibine](mailto:support@ioscorp.com) bu değerleri almak için.

5. Üzerinde **SAML imzalama sertifikası** bölümünde **meta veri XML** ve bilgisayarınızda meta veri dosyasını kaydedin.

    ![Sertifika indirme bağlantısı](./media/envimmis-tutorial/tutorial_envimmis_certificate.png) 

6. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açma Kaydet düğmesi yapılandırın](./media/envimmis-tutorial/tutorial_general_400.png)

7. Farklı bir web tarayıcı penceresinde Envi MMIS sitenize yönetici olarak oturum.

8. Tıklayarak **My Domain** sekmesi.

    ![Çoklu oturum açma Kaydet düğmesi yapılandırın](./media/envimmis-tutorial/configure1.png)

9. **Düzenle**’ye tıklayın.

    ![Çoklu oturum açma Kaydet düğmesi yapılandırın](./media/envimmis-tutorial/configure2.png)

10. Seçin **uzak kimlik doğrulaması kullan** onay kutusunu seçip **HTTP yeniden yönlendirme** gelen **kimlik doğrulama türü** açılır.

    ![Çoklu oturum açma Kaydet düğmesi yapılandırın](./media/envimmis-tutorial/configure3.png)

11. Seçin **kaynakları** sekmesine ve ardından **meta verilerini karşıya yükleme**.

    ![Çoklu oturum açma Kaydet düğmesi yapılandırın](./media/envimmis-tutorial/configure4.png)

12. İçinde **meta verilerini karşıya yükleme** açılan, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açma Kaydet düğmesi yapılandırın](./media/envimmis-tutorial/configure5.png)

    a. Seçin **dosya** seçeneğini **karşıya gelen** açılır.

    b. Azure portalından indirilen meta veri dosyası seçerek karşıya **dosya simgesini**.

    c. **Tamam**’a tıklayın.

13. İndirilen meta veri dosyası karşıya yüklendikten sonra alanları otomatik olarak doldurulacak. Tıklayın **güncelleştirme**

    ![Çoklu oturum açma Kaydet düğmesi yapılandırın](./media/envimmis-tutorial/configure6.png)

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, sol bölmede, tıklayın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/envimmis-tutorial/create_aaduser_01.png)

2. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](./media/envimmis-tutorial/create_aaduser_02.png)

3. Açmak için **kullanıcı** iletişim kutusu, tıklayın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/envimmis-tutorial/create_aaduser_03.png)

4. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/envimmis-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** Britta Simon kullanıcı e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değeri yazın **parola** kutusu.

    d. **Oluştur**’a tıklayın.
 
### <a name="create-an-envi-mmis-test-user"></a>Bir Envi MMIS test kullanıcısı oluşturma

Envi MMIS için oturum açmak Azure AD kullanıcılarının etkinleştirmek için bunların Envi MMIS sağlanması gerekir.  
Envi MMIS söz konusu olduğunda, sağlama bir el ile gerçekleştirilen bir görevdir.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1. Envi MMIS şirketinizin sitesi için bir yönetici olarak oturum açın.

2. Tıklayarak **kullanıcı listesi** sekmesi.

    ![Çalışan Ekle](./media/envimmis-tutorial/user1.png)

3. Tıklayın **Kullanıcı Ekle** düğmesi.

    ![Çalışan Ekle](./media/envimmis-tutorial/user2.png)

4. İçinde **Kullanıcı Ekle** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çalışan Ekle](./media/envimmis-tutorial/user3.png)

    a. İçinde **kullanıcı adı** Britta Simon hesap kullanıcı adı türü metin ister **brittasimon@contoso.com**.
    
    b. İçinde **ad** metin türü adı BrittaSimon ister **Britta**.

    c. İçinde **Soyadı** BrittaSimon Soyadı türünü metin ister **Simon**.

    d. Kullanıcının bir başlık girin **başlık** TextBox.
    
    e. İçinde **e-posta adresi** metin Britta Simon hesap türü e-posta adresi ister **brittasimon@contoso.com**.

    f. İçinde **SSO kullanıcı adı** Britta Simon hesap kullanıcı adı türü metin ister **brittasimon@contoso.com**.

    g. **Kaydet**’e tıklayın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Azure çoklu oturum açma kullanmak için Envi MMIS erişim vererek Britta Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**Britta Simon Envi MMIS için atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

2. Uygulamalar listesinde **Envi MMIS**.

    ![Uygulamalar listesinde Envi MMIS bağlantı](./media/envimmis-tutorial/tutorial_envimmis_app.png)  

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı][202]

4. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde Envi MMIS kutucuğa tıkladığınızda, otomatik olarak Envi MMIS uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir?](../manage-apps/what-is-single-sign-on.md)



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

