---
title: 'Öğretici: Azure Active Directory Tümleştirme ile IdeaScale | Microsoft Docs'
description: Çoklu oturum açma Azure Active Directory ile IdeaScale arasında yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: e16dda6b-fdf9-43cc-9bbb-a523f085a8af
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/16/2017
ms.author: jeedes
ms.openlocfilehash: 29fecfb9a93934fd12492ee34ab619198c341c51
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2018
ms.locfileid: "34344703"
---
# <a name="tutorial-azure-active-directory-integration-with-ideascale"></a>Öğretici: Azure Active Directory Tümleştirme IdeaScale ile

Bu öğreticide, Azure Active Directory (Azure AD) ile IdeaScale tümleştirmek öğrenin.

IdeaScale Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- IdeaScale erişimi, Azure AD'de kontrol edebilirsiniz
- Otomatik olarak için IdeaScale (çoklu oturum açma) ile Azure AD hesaplarına açan kullanıcılarınıza etkinleştirebilirsiniz
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirme IdeaScale ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir IdeaScale çoklu oturum açma etkin abonelik

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden IdeaScale ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-ideascale-from-the-gallery"></a>Galeriden IdeaScale ekleme
Azure AD IdeaScale tümleştirilmesi yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden IdeaScale eklemeniz gerekir.

**Galeriden IdeaScale eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Uygulamalar][3]

4. Arama kutusuna **IdeaScale**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-ideascale-tutorial/tutorial_ideascale_search.png)

5. Sonuçlar panelinde seçin **IdeaScale**ve ardından **Ekle** uygulama eklemek için düğmesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-ideascale-tutorial/tutorial_ideascale_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Çoklu oturum açmayı yapılandırma ve Azure AD sınama
Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon." olarak adlandırılan bir test kullanıcı tabanlı IdeaScale ile test etme

Tekli çalışmaya oturum için Azure AD IdeaScale karşılık gelen kullanıcı için bir kullanıcı Azure AD'de nedir bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının IdeaScale ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

IdeaScale içinde değerini atayın **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** bağlantı ilişkisi oluşturmak için.

Yapılandırma ve Azure AD çoklu oturum açma IdeaScale ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Bir IdeaScale test kullanıcısı oluşturma](#creating-an-ideascale-test-user)**  - Britta Simon, karşılık gelen kullanıcı Azure AD gösterimini bağlı IdeaScale sağlamak için.
4. **[Azure AD test kullanıcısı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açmayı test](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma IdeaScale uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma ile IdeaScale yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **IdeaScale** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-ideascale-tutorial/tutorial_ideascale_samlbase.png)

3. Üzerinde **IdeaScale etki alanı ve URL'leri** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-ideascale-tutorial/tutorial_ideascale_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://<companyname>.ideascale.com`

    b. İçinde **tanımlayıcısı** metin kutusuna, URL şu biçimi kullanarak bir yazın:
    | |
    |--|
    | `http://<companyname>.ideascale.com`  |
    | `https://<companyname>.ideascale.com` |

    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler gerçek oturum açma URL'si ve tanımlayıcı ile güncelleştirin. Kişi [IdeaScale istemci destek ekibi](http://support.ideascale.com/) bu değerleri almak için. 
 
4. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **meta veri XML** ve meta veri dosyası, bilgisayarınıza kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-ideascale-tutorial/tutorial_ideascale_certificate.png) 

5. Tıklatın **kaydetmek** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-ideascale-tutorial/tutorial_general_400.png)

6. Üzerinde **IdeaScale yapılandırma** 'yi tıklatın **yapılandırma IdeaScale** açmak için **yapılandırma oturum açma** penceresi. Kopya **Sign-Out URL ve SAML varlık kimliği** gelen **hızlı başvuru bölümünde**.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-ideascale-tutorial/tutorial_ideascale_configure.png) 

7. Farklı web tarayıcısı penceresinde IdeaScale şirket sitenize yönetici olarak oturum açın.

8. Git **topluluk ayarları**.
   
    ![Topluluk ayarları](./media/active-directory-saas-ideascale-tutorial/ic790847.png "topluluk ayarları")

9. Git **güvenlik \> tek oturum açma ayarları**.
   
    ![Çoklu oturum açma ayarları](./media/active-directory-saas-ideascale-tutorial/ic790848.png "tek oturum açma ayarları")

10. Olarak **çoklu oturum açma türü**seçin **SAML 2.0**.
   
    ![Oturum açma türü tek](./media/active-directory-saas-ideascale-tutorial/ic790849.png "tek oturum açma türü")

11. Üzerinde **çoklu oturum açma ayarları** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:
   
    ![Çoklu oturum açma ayarları](./media/active-directory-saas-ideascale-tutorial/ic790850.png "tek oturum açma ayarları")
   
    a. İçinde **SAML IDP varlık kimliği** metin değerini yapıştırın **SAML varlık kimliği** Azure portalından kopyalanan.

    b. Azure Portalı'ndan indirilen meta veri dosyasının içeriğini kopyalayın ve yapıştırın **SAML IDP meta veri** metin kutusu.

    c. İçinde **oturum kapatma başarı URL** metin değerini yapıştırın **Sign-Out URL** Azure portalından kopyalanan.

    d. Tıklatın **değişiklikleri kaydetmek**.

> [!TIP]
> Şimdi bu yönergeleri içinde kısa bir sürümünü okuyabilirsiniz [Azure portal](https://portal.azure.com)uygulaması kuruluyor yaparken!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** sekmesinde ve aracılığıyla katıştırılmış belgelere erişebilir **yapılandırma** alt bölüm. Daha fazla bilgiyi burada embedded belgeler özelliği hakkında: [Azure AD embedded belgeler]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

![Azure AD Kullanıcı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portal**, sol gezinti bölmesinde tıklatın **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-ideascale-tutorial/create_aaduser_01.png) 

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-ideascale-tutorial/create_aaduser_02.png) 

3. Açmak için **kullanıcı** iletişim kutusunda, tıklatın **Ekle** iletişim kutusunun üst kısmında.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-ideascale-tutorial/create_aaduser_03.png) 

4. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-ideascale-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna, türü **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna, türü **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.
 
### <a name="creating-an-ideascale-test-user"></a>Bir IdeaScale test kullanıcısı oluşturma

Azure AD kullanıcıların IdeaScale oturum etkinleştirmek için bunlar içinde IdeaScale için hazırlanması gerekir. IdeaScale söz konusu olduğunda, sağlama bir el ile bir görevdir.

**Kullanıcı sağlamayı yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Oturum, **IdeaScale** yönetici olarak şirket site.

2. Git **topluluk ayarları**.
   
    ![Topluluk ayarları](./media/active-directory-saas-ideascale-tutorial/ic790847.png "topluluk ayarları")

3. Git **temel ayarları \> üye yönetim**.

4. Tıklatın **üye ekleme**.
   
    ![Üye yönetim](./media/active-directory-saas-ideascale-tutorial/ic790852.png "üye Yönetimi")

5. Yeni Üye Ekle bölümünde, aşağıdaki adımları gerçekleştirin:
   
    ![Yeni üye eklemek](./media/active-directory-saas-ideascale-tutorial/ic790853.png "yeni üye ekleme")
   
    a. İçinde **e-posta adresleri** metin kutusuna, kullanmak istediğiniz sağlamak için geçerli bir AAD hesabıyla e-posta adresini yazın.
   
    b. Tıklatın **değişiklikleri kaydetmek**. 
   
    >[!NOTE]
    >Azure Active Directory hesap sahibi etkin duruma gelmesi hesabı onaylamak için bir bağlantı içeren bir e-posta alır.
      
>[!NOTE]
>API sağlama AAD kullanıcı hesaplarına IdeaScale tarafından sağlanan veya herhangi diğer IdeaScale kullanıcı hesabı oluşturma araçlarını kullanabilirsiniz.
 

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atama

Bu bölümde, Britta IdeaScale için erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı atama][200] 

**IdeaScale için Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **IdeaScale**.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-ideascale-tutorial/tutorial_ideascale_app.png) 

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    ![Kullanıcı atama][202] 

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Kullanıcı atama][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açmayı test etme


Bu bölümün amacı erişim paneli kullanılarak Azure AD çoklu oturum açma yapılandırmanızı test etmektir.

Erişim paneli IdeaScale parçasında tıklattığınızda, otomatik olarak IdeaScale uygulamanıza açan.

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](active-directory-saas-tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/active-directory-saas-ideascale-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-ideascale-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-ideascale-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-ideascale-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-ideascale-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-ideascale-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-ideascale-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-ideascale-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-ideascale-tutorial/tutorial_general_203.png

