---
title: "Öğretici: Azure Active Directory Tümleştirme ile Teamphoria | Microsoft Docs"
description: "Çoklu oturum açma Azure Active Directory ile Teamphoria arasında yapılandırmayı öğrenin."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: d569c705-6f0f-4ec1-b485-ba82526b5d32
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/07/2017
ms.author: jeedes
ms.openlocfilehash: 2a35efb04d7fe22abc6894c149caf090666ce016
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-teamphoria"></a>Öğretici: Azure Active Directory Tümleştirme Teamphoria ile

Bu öğreticide, Azure Active Directory (Azure AD) ile Teamphoria tümleştirmek öğrenin.

Teamphoria Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Teamphoria erişimi, Azure AD'de kontrol edebilirsiniz
- Otomatik olarak için Teamphoria (çoklu oturum açma) ile Azure AD hesaplarına açan kullanıcılarınıza etkinleştirebilirsiniz
- Hesaplarınızı bir merkezi konumda - Azure Yönetim Portalı'nı yönetme

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](active-directory-appssoaccess-whatis.md).

<!--## Overview

To enable single sign-on with Teamphoria, it must be configured to use Azure Active Directory as an identity provider. This guide provides information and tips on how to perform this configuration in Teamphoria.

>[!Note]: 
>This embedded guide is brand new in the new Azure portal, and we’d love to hear your thoughts. Use the Feedback ? button at the top of the portal to provide feedback. The older guide for using the [Azure classic portal](https://manage.windowsazure.com) to configure this application can be found [here](https://github.com/Azure/AzureAD-App-Docs/blob/master/articles/en-us/_/sso_configure.md).-->


## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirme Teamphoria ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir Teamphoria çoklu oturum açma etkin abonelik

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Bu gerekli olmadığı sürece, üretim ortamınızın kullanmamanız gerekir.
- Bir Azure AD deneme ortam yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden Teamphoria ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-teamphoria-from-the-gallery"></a>Galeriden Teamphoria ekleme
Azure AD Teamphoria tümleştirilmesi yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden Teamphoria eklemeniz gerekir.

**Galeriden Teamphoria eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure Yönetim Portalı](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

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

Bu bağlantı değeri atayarak ilişkisi **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** Teamphoria içinde.

Yapılandırma ve Azure AD çoklu oturum açma Teamphoria ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Teamphoria test kullanıcısı oluşturma](#creating-a-teamphoria-test-user)**  - Britta Simon, karşılık gelen her, Azure AD gösterimine bağlı Teamphoria sağlamak için.
4. **[Azure AD test kullanıcısı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açmayı test](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure Yönetim Portalı'nda etkinleştirin ve çoklu oturum açma Teamphoria uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma ile Teamphoria yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure Yönetim Portalı'nda üzerinde **Teamphoria** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda, olarak **modu** seçin **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-teamphoria-tutorial/tutorial_teamphoria_samlbase.png)

3. Üzerinde **Teamphoria etki alanı ve URL'leri** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-teamphoria-tutorial/tutorial_teamphoria_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna, şu biçimi kullanarak URL'yi yazın:`https://<sub-domain>.teamphoria.com/login`    

    > [!NOTE] 
    > Lütfen bu gerçek değerlerin olmadığına dikkat edin. Gerçek oturum açma URL'si ile bu değerleri güncelleştirmeniz gerekir. Kişi [Teamphoria istemci destek ekibi](https://www.teamphoria.com/) oturum açma URL'si alınamadı. 

4. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **sertifika (Base64)** ve sertifikayı bilgisayarınıza kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-teamphoria-tutorial/tutorial_teamphoria_certificate.png) 

5. Tıklatın **kaydetmek** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-teamphoria-tutorial/tutorial_general_400.png)

6. Üzerinde **Teamphoria yapılandırma** 'yi tıklatın **yapılandırma Teamphoria** açmak için **yapılandırma oturum açma** penceresi. Kopya **SAML çoklu oturum açma hizmet URL'si** gelen **hızlı başvuru bölümü.**

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-teamphoria-tutorial/tutorial_teamphoria_configure.png) 

7. Çoklu oturum açma yapılandırmak için **Teamphoria** yan, Teamphoria uygulamanızı yönetici olarak oturum açın.

8. Git **yönetici ayarları** sol araç ve altında seçeneği yapılandırma sekmesini üzerinde **tek oturum açma** SSO yapılandırma penceresini açmak için.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-teamphoria-tutorial/admin_sso_configure.png)

9. Tıklayın **yeni kimlik sağlayıcı Ekle** SSO ayarlarını ekleme formunu açmak için sağ üst köşedeki seçeneği.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-teamphoria-tutorial/add_new_identity_provider.png)

10. Aşağıdaki - açıklandığı gibi alanlara ayrıntılarını girin

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-teamphoria-tutorial/Teamphoria_sso_save.png)

    a. **GÖRÜNEN adı** : Yönetim sayfasında eklenti görünen adını girin.

    b. **DÜĞME adı** : SSO oturum açma için oturum açma sayfasında görüntüler sekmesini adı.

    c. **Sertifika** : sertifika indirilen daha önce Not Defteri'nde, Azure portalından Aç aynı içeriğini kopyalayın ve buraya kutusuna yapıştırın.

    d. **Giriş noktası** : Yapıştır **SAML çoklu oturum açma hizmet URL'si** önceki form Azure portalı kopyalanır.

    e. Seçeneğini geçiş **ON** ve tıklayın **KAYDETMEK**.   

<!--### Next steps

To ensure users can sign-in to Teamphoria after it has been configured to use Azure Active Directory, review the following tasks and topics:

- User accounts must be pre-provisioned into Teamphoria prior to sign-in. To set this up, see Provisioning.
 
- Users must be assigned access to Teamphoria in Azure AD to sign-in. To assign users, see Users.
 
- To configure access polices for Teamphoria users, see Access Policies.
 
- For additional information on deploying single sign-on to users, see [this article](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-appssoaccess-whatis#deploying-azure-ad-integrated-applications-to-users).-->


### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure Yönetim Portalı'nda bir test kullanıcı oluşturmaktır.

![Azure AD Kullanıcı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure Yönetim Portalı**, sol gezinti bölmesinde tıklatın **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-teamphoria-tutorial/create_aaduser_01.png) 

2. Git **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar** kullanıcıların listesini görüntülemek için.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-teamphoria-tutorial/create_aaduser_02.png) 

3. İletişim kutusunun üstündeki **Ekle** açmak için **kullanıcı** iletişim.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-teamphoria-tutorial/create_aaduser_03.png) 

4. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-teamphoria-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna, türü **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna, türü **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**'a tıklayın.
 
### <a name="creating-a-teamphoria-test-user"></a>Teamphoria test kullanıcısı oluşturma

Azure AD kullanıcıların Teamphoria oturum etkinleştirmek için bunların Teamphoria sağlanmalıdır. Teamphoria söz konusu olduğunda, sağlama bir el ile bir görevdir.

**Kullanıcı hesaplarını sağlamak için aşağıdaki adımları gerçekleştirin:**

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

Bu bölümde, Britta Teamphoria için kendi erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı atama][200] 

**Teamphoria için Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure Yönetim Portalı'nda uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

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

Çoklu oturum açma ayarlarınızı test etmek isterseniz, erişim paneli açın. Erişim paneli hakkında daha fazla ayrıntı için bkz: [erişim Paneli'ne giriş](https://msdn.microsoft.com/library/dn308586). 

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](active-directory-saas-tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](active-directory-appssoaccess-whatis.md)



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

