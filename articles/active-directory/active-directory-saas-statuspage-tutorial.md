---
title: 'Öğretici: Azure Active Directory Tümleştirme ile StatusPage | Microsoft Docs'
description: Çoklu oturum açma Azure Active Directory ile StatusPage arasında yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: f6ee8bb3-df43-4c0d-bf84-89f18deac4b9
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: jeedes
ms.openlocfilehash: 4b28e9c90da60aa9fb2113c5a179095156d16e02
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2018
ms.locfileid: "34352829"
---
# <a name="tutorial-azure-active-directory-integration-with-statuspage"></a>Öğretici: Azure Active Directory Tümleştirme StatusPage ile

Bu öğreticide, Azure Active Directory (Azure AD) ile StatusPage tümleştirmek öğrenin.

StatusPage Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- StatusPage erişimi, Azure AD'de kontrol edebilirsiniz
- Otomatik olarak için StatusPage (çoklu oturum açma) ile Azure AD hesaplarına açan kullanıcılarınıza etkinleştirebilirsiniz
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirme StatusPage ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir StatusPage çoklu oturum açma abonelik etkin

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden StatusPage ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-statuspage-from-the-gallery"></a>Galeriden StatusPage ekleme
Azure AD StatusPage tümleştirilmesi yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden StatusPage eklemeniz gerekir.

**Galeriden StatusPage eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Uygulamalar][3]

4. Arama kutusuna **StatusPage**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-statuspage-tutorial/tutorial_statuspage_search.png)

5. Sonuçlar panelinde seçin **StatusPage**ve ardından **Ekle** uygulama eklemek için düğmesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-statuspage-tutorial/tutorial_statuspage_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Çoklu oturum açmayı yapılandırma ve Azure AD sınama
Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı StatusPage sınayın.

Tekli çalışmaya oturum için Azure AD StatusPage karşılık gelen kullanıcı için bir kullanıcı Azure AD'de nedir bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının StatusPage ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

StatusPage içinde değerini atayın **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** bağlantı ilişkisi oluşturmak için.

Yapılandırma ve Azure AD çoklu oturum açma StatusPage ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[StatusPage test kullanıcısı oluşturma](#creating-a-statuspage-test-user)**  - Britta Simon, karşılık gelen kullanıcı Azure AD gösterimini bağlı StatusPage sağlamak için.
4. **[Azure AD test kullanıcısı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açmayı test](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma StatusPage uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma ile StatusPage yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **StatusPage** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-statuspage-tutorial/tutorial_statuspage_samlbase.png)

3. Üzerinde **StatusPage etki alanı ve URL'leri** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-statuspage-tutorial/tutorial_statuspage_url.png)

    a. İçinde **tanımlayıcısı** metin kutusuna, URL şu biçimi kullanarak bir yazın:
    | |
    |--|
    | `https://<subdomain>.statuspagestaging.com/` |
    | `https://<subdomain>.statuspage.io/` |

    b. İçinde **yanıt URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın: 
    | |
    |--|
    | `https://<subdomain>.statuspagestaging.com/sso/saml/consume` |
    | `https://<subdomain>.statuspage.io/sso/saml/consume` |

    > [!NOTE]
    > StatusPage destek ekibi ile iletişime geçin [ SupportTeam@statuspage.io ](mailto:SupportTeam@statuspage.io)çoklu oturum açma yapılandırmak için gerekli meta veri istemek için. 
    >
    >a. Meta verileri, veren değerini kopyalayın ve ardından yapıştırın **tanımlayıcısı** metin kutusu.
    >
    >b. Meta veri yanıt URL'yi kopyalayın ve ardından yapıştırın **yanıt URL'si** metin kutusu.

4. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **sertifika (Base64)** ve sertifika dosyayı bilgisayarınıza kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-statuspage-tutorial/tutorial_statuspage_certificate.png) 

5. Tıklatın **kaydetmek** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-statuspage-tutorial/tutorial_general_400.png)

6. Üzerinde **StatusPage yapılandırma** 'yi tıklatın **yapılandırma StatusPage** açmak için **yapılandırma oturum açma** penceresi. Kopya **SAML çoklu oturum açma hizmet URL'si** gelen **hızlı başvuru bölümü.**

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-statuspage-tutorial/tutorial_statuspage_configure.png) 

7. Başka bir tarayıcı penceresinde StatusPage şirket sitenize yönetici olarak oturum açma.

8. Ana araç çubuğunda tıklatın **hesabı Yönet**.
   
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-statuspage-tutorial/tutorial_statuspage_06.png) 

10. Tıklatın **çoklu oturum açma** sekmesi. 
   
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-statuspage-tutorial/tutorial_statuspage_07.png) 

11. SSO Kurulumu sayfasında, aşağıdaki adımları gerçekleştirin:
   
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-statuspage-tutorial/tutorial_statuspage_08.png) 

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-statuspage-tutorial/tutorial_statuspage_09.png) 
 
    a. İçinde **SSO hedef URL** metin değerini yapıştırın **SAML çoklu oturum açma hizmet URL'si**, Azure portalından kopyalanan.

    b. İndirilen sertifikanızı Not Defteri'nde açın, içeriği Kopyala ve ardından yapıştırın **sertifika** metin kutusu. 

    c. Tıklatın **yapılandırma kaydetme**.

> [!TIP]
> Şimdi bu yönergeleri içinde kısa bir sürümünü okuyabilirsiniz [Azure portal](https://portal.azure.com)uygulaması kuruluyor yaparken!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** sekmesinde ve aracılığıyla katıştırılmış belgelere erişebilir **yapılandırma** alt bölüm. Daha fazla bilgiyi burada embedded belgeler özelliği hakkında: [Azure AD embedded belgeler]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

![Azure AD Kullanıcı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portal**, sol gezinti bölmesinde tıklatın **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-statuspage-tutorial/create_aaduser_01.png) 

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-statuspage-tutorial/create_aaduser_02.png) 

3. Açmak için **kullanıcı** iletişim kutusunda, tıklatın **Ekle** iletişim kutusunun üst kısmında.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-statuspage-tutorial/create_aaduser_03.png) 

4. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-statuspage-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna, türü **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna, türü **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.
 
### <a name="creating-a-statuspage-test-user"></a>StatusPage test kullanıcısı oluşturma

Bu bölümün amacı Britta Simon içinde StatusPage adlı bir kullanıcı oluşturmaktır.

Yalnızca zaman sağlama StatusPage destekler. İçinde zaten etkinleştirdiğiniz [yapılandırma Azure AD çoklu oturum açma](#configuring-azure-ad-single-sign-on).

**İçinde StatusPage Britta Simon adlı bir kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. StatusPage şirket sitenize yönetici olarak oturum.

2. Üstteki menüde tıklatın **hesabı Yönet**.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-statuspage-tutorial/tutorial_statuspage_06.png)

3. Tıklatın **ekip üyelerinin** sekmesi. 
   
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-statuspage-tutorial/tutorial_statuspage_10.png) 

4. Tıklatın **Ekle ekip ÜYESİNE**. 
   
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-statuspage-tutorial/tutorial_statuspage_11.png) 

5. Tür **e-posta adresi**, **ad**, ve **soyad** istediğiniz ilgili metin kutularına sağlamayı geçerli bir kullanıcı. 
   
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-statuspage-tutorial/tutorial_statuspage_12.png) 

6. Olarak **rol**, seçin **İstemci Yöneticisi**.

7. Tıklatın **hesap oluştur**.

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atama

Bu bölümde, Britta StatusPage için erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı atama][200] 

**StatusPage için Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **StatusPage**.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-statuspage-tutorial/tutorial_statuspage_app.png) 

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    ![Kullanıcı atama][202] 

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Kullanıcı atama][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açmayı test etme

Bu bölümün amacı erişim paneli kullanılarak Azure AD çoklu oturum açma yapılandırmanızı test etmektir.

Erişim paneli StatusPage parçasında tıklattığınızda, otomatik olarak StatusPage uygulamanıza açan.

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](active-directory-saas-tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/active-directory-saas-statuspage-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-statuspage-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-statuspage-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-statuspage-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-statuspage-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-statuspage-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-statuspage-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-statuspage-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-statuspage-tutorial/tutorial_general_203.png

