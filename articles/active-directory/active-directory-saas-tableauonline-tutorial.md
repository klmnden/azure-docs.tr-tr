---
title: "Öğretici: Azure Active Directory Tümleştirme Tableau Online ile | Microsoft Docs"
description: "Çoklu oturum açma Azure Active Directory ve Tableau çevrimiçi arasında yapılandırmayı öğrenin."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 1d4b1149-ba3b-4f4e-8bce-9791316b730d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/13/2017
ms.author: jeedes
ms.openlocfilehash: 443fab1198a91a4d5749e6421f7b8603fc75a81e
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-tableau-online"></a>Öğretici: Azure Active Directory Tümleştirme Tableau Online ile

Bu öğreticide, Tableau çevrimiçi Azure Active Directory (Azure AD) ile tümleştirme öğrenin.

Tableau çevrimiçi Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Tableau çevrimiçi erişimi, Azure AD'de kontrol edebilirsiniz
- Azure AD hesaplarına otomatik olarak Tableau Online'a (çoklu oturum açma) açan kullanıcılarınıza etkinleştirebilirsiniz
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirme Tableau Online ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir Tableau çevrimiçi çoklu oturum açma abonelik etkin

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Tableau çevrimiçi galeriden ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-tableau-online-from-the-gallery"></a>Tableau çevrimiçi galeriden ekleme
Azure AD Tableau Online tümleştirilmesi yapılandırmak için Tableau çevrimiçi galeriden yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

**Tableau çevrimiçi Galeriden eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Uygulamalar][3]

4. Arama kutusuna **Tableau çevrimiçi**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_search.png)

5. Sonuçlar panelinde seçin **Tableau çevrimiçi**ve ardından **Ekle** uygulama eklemek için düğmesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Çoklu oturum açmayı yapılandırma ve Azure AD sınama
Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma Tableau "Britta Simon." olarak adlandırılan bir test kullanıcı tabanlı Online ile test etme

Tekli çalışmaya oturum için Azure AD ne karşılık gelen Tableau çevrimiçi bir kullanıcı için Azure AD içinde olduğu bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının ve ilgili kullanıcı Tableau çevrimiçi arasında bir bağlantı ilişkisi kurulması gerekir.

Çevrimiçi Tableau olarak değerini atayın **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** bağlantı ilişkisi oluşturmak için.

Yapılandırma ve Azure AD çoklu oturum açma Tableau Online ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Tableau çevrimiçi bir test kullanıcısı oluşturma](#creating-a-tableau-online-test-user)**  - Tableau kullanıcı Azure AD gösterimini bağlantılı çevrimiçi Britta Simon, karşılık gelen sağlamak için.
4. **[Azure AD test kullanıcısı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açmayı test](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma Tableau çevrimiçi uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma Tableau Online ile yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **Tableau çevrimiçi** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_samlbase.png)

3. Üzerinde **Tableau çevrimiçi etki alanı ve URL'leri** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_url.png)
    
    a. İçinde **oturum açma URL'si** metin kutusuna, URL'yi yazın:`https://sso.online.tableau.com`

    b. İçinde **tanımlayıcısı** metin kutusuna, URL'yi yazın:`https://sso.online.tableau.com/public/sp/<instancename>`

4. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **meta veri XML** ve meta veri dosyası, bilgisayarınıza kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_certificate.png) 

5. Tıklatın **kaydetmek** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-tableauonline-tutorial/tutorial_general_400.png)

6. Farklı bir tarayıcı penceresinde Tableau çevrimiçi uygulamanıza oturum. Git **ayarları** ve ardından **kimlik doğrulama**.
   
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_09.png)
    
7. SAML, altında etkinleştirmek için **kimlik doğrulama türleri** bölümü. Denetleme **çoklu oturum açma SAML ile** onay kutusu.
   
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_12.png)

8. Kadar aşağıya kaydırın **alma meta veri dosyasına Tableau çevrimiçi** bölümü.  Gözat'ı tıklatın ve Azure AD'den indirmiş meta veri dosyası içeri aktarın. Ardından **Uygula**.
   
   ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_13.png)

9. İçinde **eşleşen onaylar** bölümünde, karşılık gelen kimlik sağlayıcısı onaylama adını eklemek **e-posta adresi**, **ad**, ve **Soyadı**. Bu bilgiler Azure AD'den almak için: 
  
    a. Azure portalında geçin **Tableau çevrimiçi** uygulama tümleştirme sayfası.
    
    b. Öznitelikleri bölümünde **"görüntülemek ve diğer tüm kullanıcı öznitelikleri Düzenle"** onay kutusu. 
    
   ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-tableauonline-tutorial/attributesection.png)
      
    c. Bu öznitelikler için ad alanı değerini kopyalayın: givenname, e-posta ve aşağıdaki adımları kullanarak Soyadı:

   ![Azure AD çoklu oturum açma](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_10.png)
    
    d. Tıklatın **user.givenname** değeri 
    
    e. Değerinden kopyalama **ad alanı** metin kutusu.

   ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-tableauonline-tutorial/attributesection2.png)

    f. Namesapce kopyalamak için değerler soyadını ve e-posta için yukarıdaki adımları izleyin.

    g. Tableau çevrimiçi uygulamaya geçiş yapın ve ardından ayarlamak **çevrimiçi öznitelikleri Tableau** gibi bölümünde:
     * E-posta: **posta** veya **userprincipalname**
     * Ad: **givenname**
     * Soyadı: **Soyadı**
   
   ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_14.png)

> [!TIP]
> Şimdi bu yönergeleri içinde kısa bir sürümünü okuyabilirsiniz [Azure portal](https://portal.azure.com)uygulaması kuruluyor yaparken!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** sekmesinde ve aracılığıyla katıştırılmış belgelere erişebilir **yapılandırma** alt bölüm. Daha fazla bilgiyi burada embedded belgeler özelliği hakkında: [Azure AD embedded belgeler]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

![Azure AD Kullanıcı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portal**, sol gezinti bölmesinde tıklatın **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-tableauonline-tutorial/create_aaduser_01.png) 

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-tableauonline-tutorial/create_aaduser_02.png) 

3. Açmak için **kullanıcı** iletişim kutusunda, tıklatın **Ekle** iletişim kutusunun üst kısmında.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-tableauonline-tutorial/create_aaduser_03.png) 

4. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-tableauonline-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna, türü **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna, türü **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**'a tıklayın.
 
### <a name="creating-a-tableau-online-test-user"></a>Tableau çevrimiçi bir test kullanıcısı oluşturma

Bu bölümde, Britta Simon Tableau çevrimiçi olarak adlandırılan bir kullanıcı oluşturun.

1. Üzerinde **Tableau çevrimiçi**, tıklatın **ayarları** ve ardından **kimlik doğrulaması** bölümü. Ekranı aşağı kaydırarak **Kullanıcıları Seç** bölümü. Tıklatın **kullanıcıları eklemek** ve ardından **e-posta adreslerini girin**.
   
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_15.png)
2. Seçin **çoklu oturum açma (SSO) kimlik doğrulaması için kullanıcı ekleme**. İçinde **e-posta adreslerini girin** textbox ekleyinbritta.simon@contoso.com
   
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_11.png)
3. **Oluştur**'a tıklayın.

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atama

Bu bölümde, Britta Tableau Online'a erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı atama][200] 

**Britta Simon Tableau Online'a atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **Tableau çevrimiçi**.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_app.png) 

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    ![Kullanıcı atama][202] 

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Kullanıcı atama][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açmayı test etme

Bu bölümün amacı erişim paneli kullanılarak Azure AD SSO yapılandırmanızı test etmektir.

Erişim panelinde Tableau çevrimiçi kutucuğa tıkladığınızda, otomatik olarak Tableau çevrimiçi uygulamanıza açan.

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](active-directory-saas-tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_203.png

