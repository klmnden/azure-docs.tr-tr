---
title: 'Öğretici: Azure Active Directory Tümleştirme Procore SSO | Microsoft Docs'
description: Çoklu oturum açma Azure Active Directory ile Procore SSO arasında yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 9818edd3-48c0-411d-b05a-3ec805eafb2e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/07/2017
ms.author: jeedes
ms.openlocfilehash: 4cba8aa046b84fe63b17a662990824b1823c1572
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/10/2018
---
# <a name="tutorial-azure-active-directory-integration-with-procore-sso"></a>Öğretici: Azure Active Directory Tümleştirme Procore SSO

Bu öğreticide, Azure Active Directory (Azure AD) ile Procore SSO tümleştirmek öğrenin.

Procore SSO Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Procore SSO erişimi, Azure AD'de kontrol edebilirsiniz
- Azure AD hesaplarına otomatik olarak için Procore SSO (çoklu oturum açma) açan kullanıcılarınıza etkinleştirebilirsiniz
- Hesaplarınızı bir merkezi konumda - Azure Yönetim Portalı'nı yönetme

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirme Procore SSO'su yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir Procore SSO çoklu oturum açma etkin abonelik

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Bu gerekli olmadığı sürece, üretim ortamınızın kullanmamanız gerekir.
- Bir Azure AD deneme ortam yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden Procore SSO ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-procore-sso-from-the-gallery"></a>Galeriden Procore SSO ekleme
Azure AD Procore SSO tümleştirilmesi yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden Procore SSO eklemeniz gerekir.

**Galeriden Procore SSO eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure Yönetim Portalı](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
3. Tıklatın **Ekle** iletişim kutusunun üst kısmında düğmesi.

    ![Uygulamalar][3]

4. Arama kutusuna **Procore SSO**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-procoresso-tutorial/tutorial_procoresso_search.png)

5. Sonuçlar panelinde seçin **Procore SSO**ve ardından **Ekle** uygulama eklemek için düğmesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-procoresso-tutorial/tutorial_procoresso_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Çoklu oturum açmayı yapılandırma ve Azure AD sınama
Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma Procore "Britta Simon" adlı bir test kullanıcı tabanlı SSO ile test etme.

Tekli çalışmaya oturum için Azure AD ne karşılık gelen Procore SSO bir kullanıcı için Azure AD içinde olduğu bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının ve ilgili kullanıcı Procore SSO arasında bir bağlantı ilişkisi kurulması gerekir.

Bu bağlantı değeri atayarak ilişkisi **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** Procore sso.

Yapılandırmak ve Azure AD çoklu oturum açma Procore SSO sınamak için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Procore SSO test kullanıcısı oluşturma](#creating-a-procore-sso-test-user)**  - Britta Simon, karşılık gelen her, Azure AD gösterimine bağlı Procore SSO sağlamak için.
4. **[Azure AD test kullanıcısı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açmayı test](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure Yönetim Portalı'nda etkinleştirin ve çoklu oturum açma Procore SSO uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma Procore SSO'su yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure Yönetim Portalı'nda üzerinde **Procore SSO** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-procoresso-tutorial/tutorial_procoresso_samlbase.png)

3. Üzerinde **Procore SSO etki alanı ve URL'leri** bölümü, kullanıcı gerekmez uygulama zaten Azure ile önceden tümleştirilmiş gibi tüm adımları gerçekleştirin.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-procoresso-tutorial/tutorial_procoresso_url.png)

4. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **meta veri XML** ve XML dosyayı bilgisayarınıza kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-procoresso-tutorial/tutorial_procoresso_certificate.png) 

5. Tıklatın **kaydetmek** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-procoresso-tutorial/tutorial_general_400.png)

6. Üzerinde **Procore SSO yapılandırma** 'yi tıklatın **yapılandırma Procore SSO** açmak için **yapılandırma oturum açma** penceresi. Kopya **SAML varlık kimliği ve SAML çoklu oturum açma hizmet URL'si** gelen **hızlı başvuru bölümü.**

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-procoresso-tutorial/tutorial_procoresso_configure.png) 

7. Çoklu oturum açma yapılandırmak için **Procore SSO** tarafı, procore şirket siteniz bir yönetici olarak oturum açın.

8. Araç kutusu açılır, aşağı, tıklayın **yönetici** SSO ayarları sayfasını açın.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-procoresso-tutorial/procore_tool_admin.png)

9. Aşağıdaki - açıklandığı gibi değerler kutularına yapıştırın

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-procoresso-tutorial/procore_setting_admin.png)    

    a. İçinde **tek oturum üzerinde veren URL'si** kutusu, SAML varlık kimliği kopyalanan Azure portalından Yapıştır.

    b. İçinde **hedef URL üzerinde SAML oturum** kutusu, SAML çoklu oturum açma hizmet URL'si kopyalanan Azure portalından Yapıştır.

    c. Şimdi açmak **meta veri XML** yukarıda Azure portalından indirdiğiniz ve sertifikası adlı etiketinde kopyalama **X509Certificate**. Kopyalanan değeri içine yapıştırabilirsiniz **çoklu oturum açma x509 sertifika** kutusu.

10. Tıklayın **değişiklikleri kaydetmek**.

11. Bu ayarları sonra göndermesi gerekir. **etki alanı adı** (örneğin **contoso.com**) için Procore uygulamasına, oturum açtığınız aracılığıyla [Procore destek ekibi](https://support.procore.com/) ve bunlar Bu etki alanı için Federasyon SSO etkinleştirin.

<!--### Next steps

To ensure users can sign-in to Procore SSO after it has been configured to use Azure Active Directory, review the following tasks and topics:

- User accounts must be pre-provisioned into Procore SSO prior to sign-in. To set this up, see Provisioning.
 
- Users must be assigned access to Procore SSO in Azure AD to sign-in. To assign users, see Users.
 
- To configure access polices for Procore SSO users, see Access Policies.
 
- For additional information on deploying single sign-on to users, see [this article](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#deploying-azure-ad-integrated-applications-to-users).-->


### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure Yönetim Portalı'nda bir test kullanıcı oluşturmaktır.

![Azure AD Kullanıcı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure Yönetim Portalı**, sol gezinti bölmesinde tıklatın **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-procoresso-tutorial/create_aaduser_01.png) 

2. Git **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar** kullanıcıların listesini görüntülemek için.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-procoresso-tutorial/create_aaduser_02.png) 

3. İletişim kutusunun üstündeki **Ekle** açmak için **kullanıcı** iletişim.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-procoresso-tutorial/create_aaduser_03.png) 

4. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-procoresso-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna, türü **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna, türü **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.
 
### <a name="creating-a-procore-sso-test-user"></a>Procore SSO test kullanıcısı oluşturma

Lütfen izleyin kendi tarafında Procore test kullanıcısı oluşturmak için aşağıdaki adımları.

1. Procore şirket siteniz bir yönetici olarak oturum açın.  

2. Araç kutusu açılır, aşağı, tıklayın **Directory** şirket dizin sayfasını açın.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-procoresso-tutorial/Procore_sso_directory.png)

3. Tıklayın **bir kişi Ekle** formunu açmak ve girmek için seçenek seçenekler - gerçekleştirin

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-procoresso-tutorial/Procore_user_add.png)

    a. İçinde **ad** metin kutusuna, türü kullanıcının ilk adını gibi **Britta**.

    b. İçinde **Soyadı** metin kutusuna, türü kullanıcının soyadını gibi **Simon**.

    c. İçinde **e-posta adresi** türü kullanıcının e-posta adresi metin kutusuna, ister **BrittaSimon@contoso.com**.

    d. Seçin **izin şablonu** olarak **izin şablonu daha sonra uygulamak**.

    e. **Oluştur**’a tıklayın.

4. Kontrol edin ve yeni eklenen kişi ayrıntılarını güncelleştirin.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-procoresso-tutorial/Procore_user_check.png)

5. Tıklayın **Kaydet ve Gönder daveti** (posta yoluyla bir davet gerekliyse) veya **kaydetmek** (kullanıcı kayıt işlemini tamamlamak için doğrudan Kaydet).
    
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-procoresso-tutorial/Procore_user_save.png)    

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atama

Bu bölümde, Britta Procore SSO kendi erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı atama][200] 

**Britta Simon Procore SSO atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure Yönetim Portalı'nda uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **Procore SSO**.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-procoresso-tutorial/tutorial_procoresso_app.png) 

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    ![Kullanıcı atama][202] 

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Kullanıcı atama][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açmayı test etme

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Çoklu oturum açma ayarlarınızı test etmek isterseniz, erişim paneli açın. Erişim paneli hakkında daha fazla ayrıntı için bkz: [erişim Paneli'ne giriş](https://msdn.microsoft.com/library/dn308586). Erişim paneli Procore SSO parçasında tıklattığınızda, otomatik olarak Procore SSO uygulamanıza açan.

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](active-directory-saas-tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-procoresso-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-procoresso-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-procoresso-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-procoresso-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-procoresso-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-procoresso-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-procoresso-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-procoresso-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-procoresso-tutorial/tutorial_general_203.png

