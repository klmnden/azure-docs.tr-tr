---
title: 'Öğretici: Azure Active Directory Tümleştirme ile Teamphoria | Microsoft Docs'
description: Çoklu oturum açma Azure Active Directory ile Teamphoria arasında yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: d569c705-6f0f-4ec1-b485-ba82526b5d32
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/23/2018
ms.author: jeedes
ms.openlocfilehash: 89dea3114c502cbc726e48066138169dc4cc7e04
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34591897"
---
# <a name="tutorial-azure-active-directory-integration-with-teamphoria"></a>Öğretici: Azure Active Directory Tümleştirme Teamphoria ile

Bu öğreticide, Azure Active Directory (Azure AD) ile Teamphoria tümleştirmek öğrenin.

Teamphoria Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Teamphoria erişimi, Azure AD'de kontrol edebilirsiniz
- Otomatik olarak için Teamphoria (çoklu oturum açma) ile Azure AD hesaplarına açan kullanıcılarınıza etkinleştirebilirsiniz
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirme Teamphoria ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir Teamphoria çoklu oturum açma etkin abonelik

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, şunları yapabilirsiniz [bir aylık deneme sürümünü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin.
Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden Teamphoria ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-teamphoria-from-the-gallery"></a>Galeriden Teamphoria ekleme
Azure AD Teamphoria tümleştirilmesi yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden Teamphoria eklemeniz gerekir.

**Galeriden Teamphoria eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure Portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi.

    ![Active Directory][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
3. Tıklatın **Ekle** iletişim kutusunun üst kısmında düğmesi.

    ![Uygulamalar][3]

4. Arama kutusuna **Teamphoria**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-teamphoria-tutorial/tutorial_teamphoria_search.png)

5. Sonuçlar panelinde seçin **Teamphoria**ve ardından **Ekle** uygulama eklemek için düğmesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-teamphoria-tutorial/tutorial_teamphoria_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Çoklu oturum açmayı yapılandırma ve Azure AD sınama
Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı Teamphoria sınayın.

Tekli çalışmaya oturum için Azure AD Teamphoria karşılık gelen kullanıcı için bir kullanıcı Azure AD'de nedir bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının Teamphoria ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma Teamphoria ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Teamphoria test kullanıcısı oluşturma](#creating-a-teamphoria-test-user)**  - Britta Simon, karşılık gelen her, Azure AD gösterimine bağlı Teamphoria sağlamak için.
4. **[Azure AD test kullanıcısı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açmayı test](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma Teamphoria uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma ile Teamphoria yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **Teamphoria** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-teamphoria-tutorial/tutorial_teamphoria_samlbase.png)

3. Üzerinde **Teamphoria etki alanı ve URL'leri** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-teamphoria-tutorial/tutorial_teamphoria_url.png)

    İçinde **oturum açma URL'si** metin kutusuna, şu biçimi kullanarak URL'yi yazın: `https://<sub-domain>.teamphoria.com/login`   

    > [!NOTE] 
    > Oturum açma URL'si değeri gerçek değil. Bu değer gerçek oturum açma URL'si ile güncelleştirmeniz gerekir. Kişi [Teamphoria istemci destek ekibi](https://www.teamphoria.com/) oturum açma URL'si alınamadı.

4. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **sertifika (Base64)** ve sertifikayı bilgisayarınıza kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-teamphoria-tutorial/tutorial_teamphoria_certificate.png)

5. Tıklatın **kaydetmek** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-teamphoria-tutorial/tutorial_general_400.png)

6. Üzerinde **Teamphoria yapılandırma** 'yi tıklatın **yapılandırma Teamphoria** açmak için **yapılandırma oturum açma** penceresi. Kopya **SAML çoklu oturum açma hizmet URL'si** gelen **hızlı başvuru bölümü.**

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-teamphoria-tutorial/tutorial_teamphoria_configure.png)

7. Çoklu oturum açma yapılandırmak için **Teamphoria** yan, Teamphoria uygulamanızı yönetici olarak oturum açın.

8. Gidin **yönetim ayarlarını** sol araç çubuğunda ve yapılandırma sekmesi altında seçeneğini üzerinde **tek oturum açma** SSO yapılandırma penceresini açın.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-teamphoria-tutorial/admin_sso_configure.png)

9. Tıklayın **yeni kimlik sağlayıcı Ekle** SSO ayarlarını ekleme formunu açmak için sağ üst köşedeki seçeneği.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-teamphoria-tutorial/add_new_identity_provider.png)

10. Aşağıdaki - açıklandığı gibi alanlara ayrıntılarını girin

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-teamphoria-tutorial/Teamphoria_sso_save.png)

    a. **GÖRÜNEN adı**: Yönetim sayfasında eklenti görünen adını girin.

    b. **DÜĞME adı**: SSO oturum açma için oturum açma sayfasında görüntüler sekmesini adı.

    c. **Sertifika**: sertifika indirilen daha önce Not Defteri'nde, Azure portalından Aç aynı içeriğini kopyalayın ve buraya kutusuna yapıştırın.

    d. **Giriş noktası**: Yapıştır **SAML çoklu oturum açma hizmet URL'si** daha önce Azure portaldan kopyalanır.

    e. Seçeneğini geçiş **ON** ve tıklayın **KAYDETMEK**.

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

![Azure AD Kullanıcı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portal**, sol gezinti bölmesinde tıklatın **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-teamphoria-tutorial/create_aaduser_01.png) 

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-teamphoria-tutorial/create_aaduser_02.png) 

3. İletişim kutusunun üstündeki **Ekle** açmak için **kullanıcı** iletişim.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-teamphoria-tutorial/create_aaduser_03.png)

4. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-teamphoria-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna, türü **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna, türü **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.

### <a name="creating-a-teamphoria-test-user"></a>Teamphoria test kullanıcısı oluşturma

Azure AD kullanıcıların Teamphoria oturum etkinleştirmek için bunların Teamphoria sağlanmalıdır. Teamphoria söz konusu olduğunda, sağlama bir el ile bir görevdir.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1. Teamphoria şirket sitenize yönetici olarak oturum açın.

2. Tıklayın **yönetici** ayarları sol araç çubuğunda ve altında **Yönet** tıklatın sekmesi **kullanıcılar** kullanıcılar Yönetim sayfasını açmak için.

    ![Çalışanı ekleyin](./media/active-directory-saas-teamphoria-tutorial/admin_manage_users.png)

3. Tıklayın **el ile davet** seçeneği.

    ![Kişileri davet edin](./media/active-directory-saas-teamphoria-tutorial/admin_manage_add_users.png)

4. Bu sayfada, eylemi gerçekleştirin.
    
    ![Kişileri davet edin](./media/active-directory-saas-teamphoria-tutorial/manual_user_invite.png)

    a. İçinde **e-posta adresi** metin kutusuna, **e-posta adresi** BrittaSimon biri.

    b. İçinde **ad** metin kutusuna, türü **Britta**.

    c. İçinde **SOYADI** metin kutusuna, türü **Simon**.

    d. Tıklatın **davet 1 kullanıcı**. Kullanıcı sistemde oluşturulmasına daveti kabul etmesi gerekir.

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atama

Bu bölümde, Britta Teamphoria için erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı atama][200]

**Teamphoria için Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201]

2. Uygulamalar listesinde **Teamphoria**.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-teamphoria-tutorial/tutorial_teamphoria_app.png) 

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    ![Kullanıcı atama][202]

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Kullanıcı atama][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.

### <a name="testing-single-sign-on"></a>Çoklu oturum açmayı test etme

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Çoklu oturum açma ayarlarınızı test etmek isterseniz, erişim paneli açın. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](https://msdn.microsoft.com/library/dn308586).

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](active-directory-saas-tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/active-directory-saas-teamphoria-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-teamphoria-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-teamphoria-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-teamphoria-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-teamphoria-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-teamphoria-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-teamphoria-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-teamphoria-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-teamphoria-tutorial/tutorial_general_203.png
