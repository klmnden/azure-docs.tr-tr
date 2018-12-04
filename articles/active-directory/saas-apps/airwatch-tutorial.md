---
title: 'Öğretici: Azure Active Directory tümleştirmesiyle AirWatch | Microsoft Docs'
description: Azure Active Directory ve AirWatch arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 96a3bb1c-96c6-40dc-8ea0-060b0c2a62e5
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: jeedes
ms.reviewer: jeedes
ms.openlocfilehash: bf95b949d6fee4057f67d1e44ded36f363aa5e2b
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2018
ms.locfileid: "52848927"
---
# <a name="tutorial-azure-active-directory-integration-with-airwatch"></a>Öğretici: Azure Active Directory AirWatch ile tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile AirWatch tümleştirme konusunda bilgi edinin.

Azure AD ile AirWatch tümleştirme ile aşağıdaki avantajları sağlar:

- AirWatch erişimi, Azure AD'de denetleyebilirsiniz
- Otomatik olarak imzalanan için AirWatch (çoklu oturum açma) ile Azure AD hesaplarına açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilirsiniz.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile AirWatch yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliği
- Bir AirWatch çoklu oturum açma etkin aboneliği

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden AirWatch ekleme
2. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-airwatch-from-the-gallery"></a>Galeriden AirWatch ekleme
Azure AD'de AirWatch tümleştirmesini yapılandırmak için AirWatch Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden AirWatch eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Uygulamalar][3]

4. Arama kutusuna **AirWatch**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/airwatch-tutorial/tutorial_airwatch_search.png)

5. Sonuçlar panelinde seçin **AirWatch**ve ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/airwatch-tutorial/tutorial_airwatch_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Yapılandırma ve test Azure AD çoklu oturum açma
Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon." adlı bir test kullanıcı tabanlı AirWatch ile test etme

Tek iş için oturum açma için Azure AD ne AirWatch karşılığı kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısının AirWatch ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Değerini atayarak bu bağlantı ilişki kurulduktan **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** AirWatch içinde.

Yapılandırma ve Azure AD çoklu oturum açma AirWatch ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[AirWatch test kullanıcısı oluşturma](#creating-a-airwatch-test-user)**  - kullanıcı Azure AD gösterimini bağlı AirWatch Britta simon'un bir karşılığı vardır.
4. **[Azure AD test kullanıcı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açma testi](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve AirWatch uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma ile AirWatch yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **AirWatch** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/airwatch-tutorial/tutorial_airwatch_samlbase.png)

3. Üzerinde **AirWatch etki alanı ve URL'ler** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/airwatch-tutorial/tutorial_airwatch_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://<subdomain>.awmdm.com/AirWatch/Login?gid=companycode`

    b. İçinde **tanımlayıcı** metin değeri olarak yazın `AirWatch`

    > [!NOTE] 
    > Bu değer, gerçek değil. Bu değer, gerçek oturum açma URL'si ile güncelleştirin. İlgili kişi [AirWatch istemci Destek ekibine](https://www.air-watch.com/company/contact-us/) bu değeri alınamıyor. 
 
4. Üzerinde **SAML imzalama sertifikası** bölümünde **meta veri XML** ve bilgisayarınızda XML dosyasını kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/airwatch-tutorial/tutorial_airwatch_certificate.png) 

5. Üzerinde **AirWatch yapılandırma** bölümünde **yapılandırma AirWatch** açmak için **yapılandırma oturum açma** penceresi. Kopyalama **SAML çoklu oturum açma hizmeti URL'si** gelen **hızlı başvuru bölümü.**

    ![Çoklu oturum açmayı yapılandırın](./media/airwatch-tutorial/tutorial_airwatch_configure.png) 

6. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/airwatch-tutorial/tutorial_general_400.png)
<CS>
7. Farklı bir web tarayıcı penceresinde AirWatch şirketinizin sitesi için bir yönetici olarak oturum açın.

8. Sol gezinti bölmesinden **hesapları**ve ardından **Yöneticiler**.
   
   ![Yöneticiler](./media/airwatch-tutorial/ic791920.png "yöneticileri")

9. Genişletin **ayarları** menüsüne ve ardından **Dizin Hizmetleri**.
   
   ![Ayarları](./media/airwatch-tutorial/ic791921.png "ayarları")

10. Tıklayın **kullanıcı** sekmesinde **temel DN** metin etki alanı adınızı yazın ve ardından **Kaydet**.
   
   ![Kullanıcı](./media/airwatch-tutorial/ic791922.png "kullanıcı")

11. Tıklayın **sunucu** sekmesi.
   
   ![Sunucu](./media/airwatch-tutorial/ic791923.png "sunucusu")

12. Aşağıdaki adımları gerçekleştirin:
    
    ![Karşıya yükleme](./media/airwatch-tutorial/ic791924.png "karşıya yükleme")   
    
    a. Olarak **dizin türü**seçin **hiçbiri**.

    b. Seçin **SAML kimlik doğrulaması için kullanmak**.

    c. İndirilen sertifika karşıya yüklemek için tıklayın **karşıya**.

13. İçinde **istek** bölümünde, aşağıdaki adımları gerçekleştirin:
    
    ![İstek](./media/airwatch-tutorial/ic791925.png "iste")  

    a. Olarak **isteği bağlama türü**seçin **POST**.

    b. Azure portalında, üzerinde **çoklu oturum açma sırasında Airwatch yapılandırma** iletişim sayfası, kopyalama **SAML çoklu oturum açma hizmeti URL'si** değeri ve ardından yapıştırın **kimlik sağlayıcısının çoklu oturum açma URL** metin.

    c. Olarak **Nameıd biçimi**seçin **e-posta adresi**.

    d. **Kaydet**’e tıklayın.

14. Tıklayın **kullanıcı** yeniden sekmesi.
    
    ![Kullanıcı](./media/airwatch-tutorial/ic791926.png "kullanıcı")

15. İçinde **özniteliği** bölümünde, aşağıdaki adımları gerçekleştirin:
    
    ![Öznitelik](./media/airwatch-tutorial/ic791927.png "özniteliği")

    a. İçinde **nesne tanımlayıcısı** metin kutusuna **http://schemas.microsoft.com/identity/claims/objectidentifier**.

    b. İçinde **kullanıcıadı** metin kutusuna **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**.

    c. İçinde **görünen ad** metin kutusuna **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname**.

    d. İçinde **ad** metin kutusuna **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname**.

    e. İçinde **Soyadı** metin kutusuna **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname**.

    f. İçinde **e-posta** metin kutusuna **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**.

    g. **Kaydet**’e tıklayın.

<CE>

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

![Azure AD kullanıcısı oluşturun][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portalında**, sol gezinti bölmesinde **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/airwatch-tutorial/create_aaduser_01.png) 

2. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/airwatch-tutorial/create_aaduser_02.png) 

3. Açmak için **kullanıcı** iletişim kutusunda, tıklayın **Ekle** iletişim kutusunun üst kısmındaki.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/airwatch-tutorial/create_aaduser_03.png) 

4. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/airwatch-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna **e-posta adresi** Britta simon'un.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.
 
### <a name="creating-a-airwatch-test-user"></a>AirWatch test kullanıcısı oluşturma

Azure AD kullanıcıları için AirWatch oturum açmak etkinleştirmek için bunlar içinde AirWatch için sağlanması gerekir.

* AirWatch, sağlama elle bir görevin olduğunda.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1. Oturum açın, **AirWatch** şirketinizin sitesi yöneticisi olarak.
2. Sol taraftaki gezinti bölmesinde **hesapları**ve ardından **kullanıcılar**.
   
   ![Kullanıcılar](./media/airwatch-tutorial/ic791929.png "kullanıcılar")
3. İçinde **kullanıcılar** menüsünde tıklatın **liste görünümü**ve ardından **Ekle \> Kullanıcı Ekle**.
   
   ![Kullanıcı ekleme](./media/airwatch-tutorial/ic791930.png "kullanıcı ekleme")
4. Üzerinde **Ekle / Düzenle kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

   ![Kullanıcı ekleme](./media/airwatch-tutorial/ic791931.png "kullanıcı ekleme")   
   1. Tür **kullanıcıadı**, **parola**, **parolayı onaylayın**, **ad**, **Soyadı**,  **E-posta adresi** istediğiniz ilgili metin kutularına zbilgisayarlar geçerli bir Azure Active Directory hesabı.
   2. **Kaydet**’e tıklayın.

>[!NOTE]
>Herhangi diğer AirWatch kullanıcı hesabı oluşturma araçları kullanabilir veya API'leri için AAD kullanıcı hesapları sağlamak AirWatch tarafından sağlanan.
>  

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcı atama

Bu bölümde, Azure çoklu oturum açma kullanmak için AirWatch erişim vererek Britta Simon etkinleştirin.

![Kullanıcı Ata][200] 

**Britta Simon AirWatch için atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

2. Uygulamalar listesinde **AirWatch**.

    ![Çoklu oturum açmayı yapılandırın](./media/airwatch-tutorial/tutorial_airwatch_app.png) 

3. Soldaki menüde **kullanıcılar ve gruplar**.

    ![Kullanıcı Ata][202] 

4. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Kullanıcı Ata][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Çoklu oturum açma ayarları test etmek isterseniz, erişim Paneli'nde açın. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md).


## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/airwatch-tutorial/tutorial_general_01.png
[2]: ./media/airwatch-tutorial/tutorial_general_02.png
[3]: ./media/airwatch-tutorial/tutorial_general_03.png
[4]: ./media/airwatch-tutorial/tutorial_general_04.png

[100]: ./media/airwatch-tutorial/tutorial_general_100.png

[200]: ./media/airwatch-tutorial/tutorial_general_200.png
[201]: ./media/airwatch-tutorial/tutorial_general_201.png
[202]: ./media/airwatch-tutorial/tutorial_general_202.png
[203]: ./media/airwatch-tutorial/tutorial_general_203.png

