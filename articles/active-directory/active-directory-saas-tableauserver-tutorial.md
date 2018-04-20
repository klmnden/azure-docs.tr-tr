---
title: 'Öğretici: Azure Active Directory Tümleştirme Tableau sunucusuyla | Microsoft Docs'
description: Çoklu oturum açma Azure Active Directory ve Tableau sunucusu arasındaki yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: c1917375-08aa-445c-a444-e22e23fa19e0
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/18/2017
ms.author: jeedes
ms.openlocfilehash: 3b0390c8b95a46b2c134252532bef118ea4df52d
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/20/2018
---
# <a name="tutorial-azure-active-directory-integration-with-tableau-server"></a>Öğretici: Azure Active Directory Tümleştirme Tableau sunucusuyla

Bu öğreticide, Azure Active Directory (Azure AD) ile Tableau Server Tümleştirme öğrenin.

Tableau Server Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Tableau sunucu erişimi, Azure AD'de kontrol edebilirsiniz
- Azure AD hesaplarına otomatik olarak (çoklu oturum açma) Tableau sunucuya açan kullanıcılarınıza etkinleştirebilirsiniz
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirme Tableau sunucusuyla yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir Tableau Server Çoklu oturum açma abonelik etkin

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden Tableau sunucu ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-tableau-server-from-the-gallery"></a>Galeriden Tableau sunucu ekleme
Azure AD Tableau sunucu tümleştirilmesi yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden Tableau sunucusu eklemeniz gerekir.

**Galeriden Tableau sunucusu eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Uygulamalar][3]

4. Arama kutusuna **Tableau Server**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-tableauserver-tutorial/tutorial_tableauserver_search.png)

5. Sonuçlar panelinde seçin **Tableau Server**ve ardından **Ekle** uygulama eklemek için düğmesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-tableauserver-tutorial/tutorial_tableauserver_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Çoklu oturum açmayı yapılandırma ve Azure AD sınama
Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma Tableau "Britta Simon." olarak adlandırılan bir test kullanıcı tabanlı sunucu ile test etme

Tekli çalışmaya oturum için Azure AD ne karşılık gelen Tableau Server'daki bir kullanıcı için Azure AD içinde olduğu bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının ve Tableau Server'daki ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Değeri Tableau Server'da atayın **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** bağlantı ilişkisi oluşturmak için.

Yapılandırma ve Azure AD çoklu oturum açma Tableau sunucusu ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Tableau Server test kullanıcısı oluşturma](#creating-a-tableau-server-test-user)**  - Britta Simon, karşılık gelen kullanıcı Azure AD gösterimini bağlı Tableau sunucusu sağlamak için.
4. **[Azure AD test kullanıcısı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açmayı test](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma Tableau Server uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma Tableau sunucusuyla yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **Tableau sunucu** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-tableauserver-tutorial/tutorial_tableauserver_samlbase.png)

3. Üzerinde **Tableau sunucu etki alanı ve URL'leri** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-tableauserver-tutorial/tutorial_tableauserver_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://azure.<domain name>.link`
    
    b. İçinde **tanımlayıcısı** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://azure.<domain name>.link`

    c. İçinde **yanıt URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://azure.<domain name>.link/wg/saml/SSO/index.html`
     
    > [!NOTE] 
    > Yukarıdaki değerleri gerçek değerler değildir. Daha sonra gerçek URL ve Tableau sunucu yapılandırma sayfasından tanımlayıcı değerlerini güncelleştirin. 

4. Tableau sunucu uygulaması SAML onaylar belirli bir biçimde bekliyor. Bu uygulama için aşağıdaki talep yapılandırın. Bu öznitelik değerlerini yönetebilirsiniz **"Kullanıcı öznitelikleri"** uygulama tümleştirmesi sayfasında bölüm. Aşağıdaki ekran görüntüsünde bir örnek için aynı gösterir.
    
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-tableauserver-tutorial/3.png)
    
5. İçinde **kullanıcı öznitelikleri** bölümünde **çoklu oturum açma** iletişim kutusunda, yukarıdaki resimde gösterildiği gibi SAML belirteci özniteliği yapılandırın ve aşağıdaki adımları gerçekleştirin:
    
    | Öznitelik Adı | Öznitelik Değeri |
    | ---------------| --------------- |    
    | kullanıcı adı | *user.mailnickname* |

    a. Tıklatın **Ekle özniteliği** açmak için **özniteliği eklemek** iletişim.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-tableauserver-tutorial/tutorial_officespace_04.png)

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-tableauserver-tutorial/tutorial_officespace_05.png)
    
    b. İçinde **adı** metin kutusuna, ilgili satır için gösterilen öznitelik adı yazın.
    
    c. Gelen **değeri** listesinde, ilgili satır için gösterilen öznitelik değeri yazın.
    
    d. Tıklatın **Tamam**


6. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **meta veri XML** ve meta veri dosyası, bilgisayarınıza kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-tableauserver-tutorial/tutorial_tableauserver_certificate.png) 

7. Tıklatın **kaydetmek** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-tableauserver-tutorial/tutorial_general_400.png)
<CS>
8. Uygulamanız için yapılandırılmış SSO almak için Tableau sunucu kiracınız yönetici olarak oturum gerekir.
   
   a. Tableau sunucu yapılandırmasında tıklatın **SAML** sekmesi.
  
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-tableauserver-tutorial/tutorial_tableauserver_001.png) 
  
   b. Onay kutusunu işaretleyin **kullanım SAML çoklu oturum açma için**.
   
   c. Tableau sunucu dönüşü URL — Tableau Server kullanıcıları, gibi erişecek URL http://tableau_server. Kullanarak http://localhost önerilmez. URL eğik ile kullanarak (örneğin, http://tableau_server/) desteklenmiyor. Kopya **Tableau sunucu dönüşü URL** ve Azure AD ile yapıştırma **oturum üzerinde URL'si** metin kutusuna **Tableau sunucu etki alanı ve URL'leri** bölümü.
   
   d. SAML varlık kimliği — varlık kimliği Tableau Server yüklemenize IDP benzersiz olarak tanımlar. Tableau sunucu URL'si yeniden burada isterseniz girebilirsiniz, ancak Tableau sunucu URL'si olması gerekmez. Kopya **SAML varlık kimliği** ve Azure AD ile yapıştırma **tanımlayıcısı** metin kutusuna **Tableau sunucu etki alanı ve URL'leri** bölümü.
     
   e. Tıklatın **meta veri dosyası ver** ve metin düzenleyici uygulamada açın. Onaylama işlemi tüketici hizmet URL'si Http Post ile bulup 0 dizin ve URL'yi kopyalayın. Artık Azure AD ile yapıştırma **yanıt URL'si** metin kutusuna **Tableau sunucu etki alanı ve URL'leri** bölümü.
   
   f. Azure portalından indirdiğiniz, Federasyon meta veri dosyasını bulun ve bunu karşıya **SAML IDP meta veri dosyası**.
   
   g. Tıklatın **Tamam** Tableau sunucu yapılandırması sayfasında düğmesini.
   
    >[!NOTE] 
    >Müşteriniz Tableau Server SAML SSO yapılandırmasında herhangi bir sertifikayı karşıya yüklemek ve SSO akışında dikkate.
    >SAML Tableau sunucuda yapılandırmaya yardımcı sonra lütfen bu makalesine başvurun [yapılandırma SAML](http://onlinehelp.tableau.com/current/server/en-us/config_saml.htm).
    >
<CE>

> [!TIP]
> Şimdi bu yönergeleri içinde kısa bir sürümünü okuyabilirsiniz [Azure portal](https://portal.azure.com)uygulaması kuruluyor yaparken!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** sekmesinde ve aracılığıyla katıştırılmış belgelere erişebilir **yapılandırma** alt bölüm. Daha fazla bilgiyi burada embedded belgeler özelliği hakkında: [Azure AD embedded belgeler]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

![Azure AD Kullanıcı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portal**, sol gezinti bölmesinde tıklatın **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-tableauserver-tutorial/create_aaduser_01.png) 

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-tableauserver-tutorial/create_aaduser_02.png) 

3. Açmak için **kullanıcı** iletişim kutusunda, tıklatın **Ekle** iletişim kutusunun üst kısmında.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-tableauserver-tutorial/create_aaduser_03.png) 

4. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-tableauserver-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna, türü **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna, türü **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.
 
### <a name="creating-a-tableau-server-test-user"></a>Tableau Server test kullanıcısı oluşturma

Bu bölümün amacı Britta Simon Tableau Server'da adlı bir kullanıcı oluşturmaktır. Tableau sunucusundaki tüm kullanıcılar sağlamak gerekir. 

Bu kullanıcı adını Azure AD özel özniteliğinde yapılandırdığınız değeri eşleşmelidir **kullanıcıadı**. Doğru eşlemesi ile tümleştirme çalışmalıdır [yapılandırma Azure AD çoklu oturum açma](#configuring-azure-ad-single-sign-on).

>[!NOTE]
>Bir kullanıcı el ile oluşturmanız gerekiyorsa, kuruluşunuzdaki Tableau sunucu yöneticisine başvurmanız gerekir.
> 
> 

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atama

Bu bölümde, Britta Tableau sunucuya erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı atama][200] 

**Britta Simon Tableau sunucusuna atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **Tableau Server**.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-tableauserver-tutorial/tutorial_tableauserver_app.png) 

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    ![Kullanıcı atama][202] 

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Kullanıcı atama][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açmayı test etme

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde Tableau sunucu bölmesi tıklattığınızda, otomatik olarak Tableau sunucu uygulamanızı açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](https://msdn.microsoft.com/library/dn308586). 

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](active-directory-saas-tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_203.png

