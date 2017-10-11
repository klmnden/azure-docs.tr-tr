---
title: "Öğretici: Azure Active Directory Tümleştirme ile AirWatch | Microsoft Docs"
description: "Çoklu oturum açma Azure Active Directory ile AirWatch arasında yapılandırmayı öğrenin."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 96a3bb1c-96c6-40dc-8ea0-060b0c2a62e5
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: jeedes
ms.reviewer: jeedes
ms.openlocfilehash: 1996ec97e7c0d94c5606ca43bb5956548f1f3712
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-airwatch"></a>Öğretici: Azure Active Directory Tümleştirme AirWatch ile

Bu öğreticide, Azure Active Directory (Azure AD) ile AirWatch tümleştirmek öğrenin.

AirWatch Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- AirWatch erişimi, Azure AD'de kontrol edebilirsiniz
- Otomatik olarak için AirWatch (çoklu oturum açma) ile Azure AD hesaplarına açan kullanıcılarınıza etkinleştirebilirsiniz
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirme AirWatch ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir AirWatch çoklu oturum açma etkin abonelik

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden AirWatch ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-airwatch-from-the-gallery"></a>Galeriden AirWatch ekleme
Azure AD AirWatch tümleştirilmesi yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden AirWatch eklemeniz gerekir.

**Galeriden AirWatch eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Uygulamalar][3]

4. Arama kutusuna **AirWatch**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-airwatch-tutorial/tutorial_airwatch_search.png)

5. Sonuçlar panelinde seçin **AirWatch**ve ardından **Ekle** uygulama eklemek için düğmesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-airwatch-tutorial/tutorial_airwatch_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Çoklu oturum açmayı yapılandırma ve Azure AD sınama
Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon." olarak adlandırılan bir test kullanıcı tabanlı AirWatch ile test etme

Tekli çalışmaya oturum için Azure AD AirWatch karşılık gelen kullanıcı için bir kullanıcı Azure AD'de nedir bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının AirWatch ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Bu bağlantı değeri atayarak ilişkisi **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** AirWatch içinde.

Yapılandırma ve Azure AD çoklu oturum açma AirWatch ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[AirWatch test kullanıcısı oluşturma](#creating-a-airwatch-test-user)**  - Britta Simon, karşılık gelen kullanıcı Azure AD gösterimini bağlı AirWatch sağlamak için.
4. **[Azure AD test kullanıcısı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açmayı test](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma AirWatch uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma ile AirWatch yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **AirWatch** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-airwatch-tutorial/tutorial_airwatch_samlbase.png)

3. Üzerinde **AirWatch etki alanı ve URL'leri** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-airwatch-tutorial/tutorial_airwatch_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın:`https://<subdomain>.awmdm.com/AirWatch/Login?gid=companycode`

    b. İçinde **tanımlayıcısı** metin değeri olarak yazın`AirWatch`

    > [!NOTE] 
    > Bu değer gerçek değil. Bu değer gerçek oturum açma URL'si ile güncelleştirin. Kişi [AirWatch istemci destek ekibi](http://www.air-watch.com/company/contact-us/) bu değeri alınamıyor. 
 
4. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **meta veri XML** ve XML dosyayı bilgisayarınıza kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-airwatch-tutorial/tutorial_airwatch_certificate.png) 

5. Üzerinde **AirWatch yapılandırma** 'yi tıklatın **yapılandırma AirWatch** açmak için **yapılandırma oturum açma** penceresi. Kopya **SAML çoklu oturum açma hizmet URL'si** gelen **hızlı başvuru bölümü.**

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-airwatch-tutorial/tutorial_airwatch_configure.png) 

6. Tıklatın **kaydetmek** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-airwatch-tutorial/tutorial_general_400.png)
<CS>
7. Farklı web tarayıcısı penceresinde AirWatch şirket sitenize yönetici olarak oturum açın.

8. Sol gezinti bölmesinde **hesapları**ve ardından **Yöneticiler**.
   
   ![Yöneticiler](./media/active-directory-saas-airwatch-tutorial/ic791920.png "yöneticileri")

9. Genişletme **ayarları** menüsüne ve ardından **Dizin Hizmetleri**.
   
   ![Ayarları](./media/active-directory-saas-airwatch-tutorial/ic791921.png "ayarları")

10. Tıklatın **kullanıcı** sekmesinde **temel DN** metin kutusuna, etki alanı adınızı yazın ve ardından **kaydetmek**.
   
   ![Kullanıcı](./media/active-directory-saas-airwatch-tutorial/ic791922.png "kullanıcı")

11. Tıklatın **Server** sekmesi.
   
   ![Sunucu](./media/active-directory-saas-airwatch-tutorial/ic791923.png "sunucu")

12. Aşağıdaki adımları gerçekleştirin:
    
    ![Karşıya yükleme](./media/active-directory-saas-airwatch-tutorial/ic791924.png "karşıya yükle")   
    
    a. Olarak **Directory türü**seçin **hiçbiri**.

    b. Seçin **SAML kimlik doğrulaması için kullanmak**.

    c. İndirilen sertifika karşıya yüklemek için tıklayın **karşıya**.

13. İçinde **isteği** bölümünde, aşağıdaki adımları gerçekleştirin:
    
    ![İstek](./media/active-directory-saas-airwatch-tutorial/ic791925.png "isteği")  

    a. Olarak **bağlama türü isteği**seçin **POST**.

    b. Azure portalında üzerinde **çoklu oturum açma sırasında Airwatch yapılandırma** iletişim sayfası, kopya **SAML çoklu oturum açma hizmet URL'si** değer ve ardından yapıştırın **kimlik sağlayıcısı çoklu oturum açma URL** metin kutusu.

    c. Olarak **NameID biçimi**seçin **e-posta adresi**.

    d. **Kaydet** düğmesine tıklayın.

14. Tıklatın **kullanıcı** yeniden sekmesinde.
    
    ![Kullanıcı](./media/active-directory-saas-airwatch-tutorial/ic791926.png "kullanıcı")

15. İçinde **özniteliği** bölümünde, aşağıdaki adımları gerçekleştirin:
    
    ![Öznitelik](./media/active-directory-saas-airwatch-tutorial/ic791927.png "özniteliği")

    a. İçinde **nesne tanımlayıcısı** metin kutusuna, türü **http://schemas.microsoft.com/identity/claims/objectidentifier**.

    b. İçinde **kullanıcıadı** metin kutusuna, türü **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**.

    c. İçinde **görünen adı** metin kutusuna, türü **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname**.

    d. İçinde **ad** metin kutusuna, türü **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname**.

    e. İçinde **Soyadı** metin kutusuna, türü **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname**.

    f. İçinde **e-posta** metin kutusuna, türü **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**.

    g. **Kaydet** düğmesine tıklayın.

<CE>

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

![Azure AD Kullanıcı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portal**, sol gezinti bölmesinde tıklatın **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-airwatch-tutorial/create_aaduser_01.png) 

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-airwatch-tutorial/create_aaduser_02.png) 

3. Açmak için **kullanıcı** iletişim kutusunda, tıklatın **Ekle** iletişim kutusunun üst kısmında.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-airwatch-tutorial/create_aaduser_03.png) 

4. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-airwatch-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna, türü **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna, türü **e-posta adresi** Britta Simon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**'a tıklayın.
 
### <a name="creating-a-airwatch-test-user"></a>AirWatch test kullanıcısı oluşturma

AirWatch için oturum açmak Azure AD kullanıcıları etkinleştirmek için bunlar içinde AirWatch için hazırlanması gerekir.

* AirWatch, sağlama el ile bir görev olduğunda.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1. Oturum, **AirWatch** yönetici olarak şirket site.
2. Sol taraftaki gezinti bölmesinde tıklayın **hesapları**ve ardından **kullanıcılar**.
   
   ![Kullanıcıların](./media/active-directory-saas-airwatch-tutorial/ic791929.png "kullanıcılar")
3. İçinde **kullanıcılar** menüsünde tıklatın **liste görünümü**ve ardından **Ekle \> Kullanıcı Ekle**.
   
   ![Kullanıcı ekleme](./media/active-directory-saas-airwatch-tutorial/ic791930.png "kullanıcı ekleme")
4. Üzerinde **Ekle / Düzenle kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

   ![Kullanıcı ekleme](./media/active-directory-saas-airwatch-tutorial/ic791931.png "kullanıcı ekleme")   
   1. Tür **kullanıcıadı**, **parola**, **parolayı onaylayın**, **ad**, **Soyadı**,  **E-posta adresi** istediğiniz ilgili metin kutularına sağlamayı geçerli bir Azure Active Directory hesabı.
   2. **Kaydet** düğmesine tıklayın.

>[!NOTE]
>API sağlama AAD kullanıcı hesaplarına AirWatch tarafından sağlanan veya herhangi diğer AirWatch kullanıcı hesabı oluşturma araçlarını kullanabilirsiniz.
>  

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atama

Bu bölümde, Britta AirWatch için erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı atama][200] 

**AirWatch için Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **AirWatch**.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-airwatch-tutorial/tutorial_airwatch_app.png) 

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    ![Kullanıcı atama][202] 

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Kullanıcı atama][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açmayı test etme

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Çoklu oturum açma ayarlarınızı test etmek isterseniz, erişim paneli açın. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](active-directory-saas-access-panel-introduction.md).


## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](active-directory-saas-tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-airwatch-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-airwatch-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-airwatch-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-airwatch-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-airwatch-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-airwatch-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-airwatch-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-airwatch-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-airwatch-tutorial/tutorial_general_203.png

