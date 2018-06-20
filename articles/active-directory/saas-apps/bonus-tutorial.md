---
title: 'Öğretici: Azure Active Directory Tümleştirme ile Bonusly | Microsoft Docs'
description: Çoklu oturum açma Azure Active Directory ile Bonusly arasında yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: 29fea32a-fa20-47b2-9e24-26feb47b0ae6
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: jeedes
ms.openlocfilehash: adde08f7981e6e23ba5bf2601d2f019702d1d33c
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/19/2018
ms.locfileid: "36231653"
---
# <a name="tutorial-azure-active-directory-integration-with-bonusly"></a>Öğretici: Azure Active Directory Tümleştirme Bonusly ile

Bu öğreticide, Azure Active Directory (Azure AD) ile Bonusly tümleştirmek öğrenin.

Bonusly Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Bonusly erişimi, Azure AD'de kontrol edebilirsiniz
- Otomatik olarak için Bonusly (çoklu oturum açma) ile Azure AD hesaplarına açan kullanıcılarınıza etkinleştirebilirsiniz
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirme Bonusly ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir Bonusly çoklu oturum açma abonelik etkin

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, şunları yapabilirsiniz [bir aylık deneme sürümünü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden Bonusly ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-bonusly-from-the-gallery"></a>Galeriden Bonusly ekleme
Azure AD Bonusly tümleştirilmesi yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden Bonusly eklemeniz gerekir.

**Galeriden Bonusly eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **Bonusly**seçin **Bonusly** sonuç panelinden ardından **Ekle** uygulama eklemek için düğmeyi.

    ![Sonuçlar listesinde bonusly](./media/bonus-tutorial/tutorial_bonusly_addfromgallery.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme
Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı Bonusly sınayın.

Tekli çalışmaya oturum için Azure AD Bonusly karşılık gelen kullanıcı için bir kullanıcı Azure AD'de nedir bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının Bonusly ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Bonusly içinde değerini atayın **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** bağlantı ilişkisi oluşturmak için.

Yapılandırma ve Azure AD çoklu oturum açma Bonusly ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Bonusly test kullanıcısı oluşturma](#create-a-bonusly-test-user)**  - Britta Simon, karşılık gelen kullanıcı Azure AD gösterimini bağlı Bonusly sağlamak için.
4. **[Azure AD test kullanıcısı atayın](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Test çoklu oturum açma](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma Bonusly uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma ile Bonusly yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **Bonusly** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/bonus-tutorial/tutorial_bonusly_samlbase.png)

3. Üzerinde **Bonusly etki alanı ve URL'leri** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Oturum açma bilgileri bonusly etki alanı ve URL'leri tek](./media/bonus-tutorial/tutorial_bonusly_url.png)

    İçinde **yanıt URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://Bonus.ly/saml/<tenant-name>`

    > [!NOTE] 
    > Değer gerçek değil. Değerin gerçek yanıt URL'si ile güncelleştirin. Kişi [Bonusly destek ekibi](https://Bonusly/contact) değeri alınamıyor.
 
4. Üzerinde **SAML imzalama sertifikası** bölümünde, kopyalama **parmak İZİ** sertifika değeri.

    ![Sertifika indirme bağlantısı](./media/bonus-tutorial/tutorial_bonusly_certificate.png) 

5. Tıklatın **kaydetmek** düğmesi.

    ![Oturum açma tek Kaydet düğmesi yapılandırın](./media/bonus-tutorial/tutorial_general_400.png)

6. Üzerinde **Bonusly yapılandırma** 'yi tıklatın **yapılandırma Bonusly** açmak için **yapılandırma oturum açma** penceresi. Kopya **SAML varlık kimliği ve SAML çoklu oturum açma hizmet URL'si** gelen **hızlı başvuru bölümü.**

    ![Bonusly yapılandırma](./media/bonus-tutorial/tutorial_bonusly_configure.png) 

7. Farklı bir tarayıcı penceresinde oturum açın, **Bonusly** Kiracı.

8. Üstteki araç çubuğunda tıklatın **ayarları**ve ardından **tümleştirmeler ve uygulamaları**.
   
    ![Bonusly sosyal bölüm](./media/bonus-tutorial/ic773686.png "Bonusly")
9. Altında **çoklu oturum açma**seçin **SAML**.

10. Üzerinde **SAML** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
   
    ![Bonusly Saml iletişim sayfa](./media/bonus-tutorial/ic773687.png "Bonusly")
   
    a. İçinde **IDP SSO hedef URL** metin değerini yapıştırın **SAML çoklu oturum açma hizmet URL'si**, Azure portalından kopyalanan.
   
    b. İçinde **IDP veren** metin değerini yapıştırın **SAML varlık kimliği**, Azure portalından kopyalanan. 

    c. İçinde **IDP oturum açma URL'si** metin değerini yapıştırın **SAML çoklu oturum açma hizmet URL'si**, Azure portalından kopyalanan.

    d. Yapıştır **parmak izi** değeri kopyalanan Azure portalından **sertifika parmak izi** metin kutusu.
   
11. **Kaydet**’e tıklayın.

> [!TIP]
> Şimdi bu yönergeleri içinde kısa bir sürümünü okuyabilirsiniz [Azure portal](https://portal.azure.com)uygulaması kuruluyor yaparken!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** sekmesinde ve aracılığıyla katıştırılmış belgelere erişebilir **yapılandırma** alt bölüm. Daha fazla bilgiyi burada embedded belgeler özelliği hakkında: [Azure AD embedded belgeler]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portal**, sol gezinti bölmesinde tıklatın **Azure Active Directory** simgesi.

    ![Azure Active Directory düğmesi](./media/bonus-tutorial/create_aaduser_01.png) 

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantılar](./media/bonus-tutorial/create_aaduser_02.png) 

3. Açmak için **kullanıcı** iletişim kutusunda, tıklatın **Ekle** iletişim kutusunun üst kısmında.
 
    ![Ekle düğmesi](./media/bonus-tutorial/create_aaduser_03.png) 

4. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Kullanıcı iletişim kutusu](./media/bonus-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna, türü **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna, türü **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.
 
### <a name="create-a-bonusly-test-user"></a>Bonusly test kullanıcısı oluşturma

Azure AD kullanıcıları için Bonusly oturum açmak etkinleştirmek için bunların Bonusly sağlanmalıdır. Bonusly söz konusu olduğunda, sağlama bir el ile bir görevdir.

>[!NOTE]
>API sağlama AAD kullanıcı hesaplarına Bonusly tarafından sağlanan veya herhangi diğer Bonusly kullanıcı hesabı oluşturma araçlarını kullanabilirsiniz.
>  

**Kullanıcı sağlamayı yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Bir web tarayıcısı penceresinde Bonusly kiracınız için oturum açın.

2. Tıklatın **ayarları**.
 
    ![Ayarları](./media/bonus-tutorial/ic781041.png "ayarları")

3. Tıklatın **kullanıcılar ve İkramiyeler** sekmesi.
   
    ![Kullanıcılar ve İkramiyeler](./media/bonus-tutorial/ic781042.png "kullanıcılar ve İkramiyeler")

4. Tıklatın **kullanıcıları yönetme**.
   
    ![Kullanıcıları yönetme](./media/bonus-tutorial/ic781043.png "kullanıcıları yönetme")

5. Tıklatın **kullanıcı ekleme**.
   
    ![Kullanıcı ekleme](./media/bonus-tutorial/ic781044.png "kullanıcı ekleme")

6. Üzerinde **Kullanıcı Ekle** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:
   
    ![Kullanıcı ekleme](./media/bonus-tutorial/ic781045.png "kullanıcı ekleme")  

    a. İçinde **ad** metin gibi kullanıcının ilk adını girin **Britta**.

    b. İçinde **Soyadı** metin kutusuna, son kullanıcı gibi adını **Simon**.
 
    c. İçinde **e-posta** metin kutusuna, bir kullanıcı gibi e-posta girin **brittasimon@contoso.com**.

    d. **Kaydet**’e tıklayın.
   
     >[!NOTE]
     >Azure AD hesap sahibi etkin duruma gelmesi hesabı onaylamak için bir bağlantı içeren bir e-posta alır.
     >  

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Britta Bonusly için erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**Bonusly için Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **Bonusly**.

    ![Bonusly bağlantı uygulamalar listesinde](./media/bonus-tutorial/tutorial_bonusly_app.png) 

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantı][202] 

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Ekleme atama bölmesi][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümün amacı erişim paneli kullanılarak Azure AD çoklu oturum açma yapılandırmanızı test etmektir.

Bonusly kutucuğa tıkladığınızda erişim panelinde, otomatik olarak Bonusly uygulamanıza açan.

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/bonus-tutorial/tutorial_general_01.png
[2]: ./media/bonus-tutorial/tutorial_general_02.png
[3]: ./media/bonus-tutorial/tutorial_general_03.png
[4]: ./media/bonus-tutorial/tutorial_general_04.png

[100]: ./media/bonus-tutorial/tutorial_general_100.png

[200]: ./media/bonus-tutorial/tutorial_general_200.png
[201]: ./media/bonus-tutorial/tutorial_general_201.png
[202]: ./media/bonus-tutorial/tutorial_general_202.png
[203]: ./media/bonus-tutorial/tutorial_general_203.png

