---
title: 'Öğretici: Azure Active Directory Procore SSO ile tümleştirme | Microsoft Docs'
description: Azure Active Directory ve Procore SSO arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 9818edd3-48c0-411d-b05a-3ec805eafb2e
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/07/2017
ms.author: jeedes
ms.openlocfilehash: bd84224f4c3a8a498a296ff50190713111895472
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/14/2018
ms.locfileid: "39051624"
---
# <a name="tutorial-azure-active-directory-integration-with-procore-sso"></a>Öğretici: Azure Active Directory Procore SSO ile tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile Procore SSO tümleştirme konusunda bilgi edinin.

Procore SSO, Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Procore SSO erişimi, Azure AD'de denetleyebilirsiniz
- Azure AD hesaplarına otomatik olarak imzalanan Procore SSO (çoklu oturum açma) açma, kullanıcılarınızın etkinleştirebilirsiniz
- Bir merkezi konumda - Azure Yönetim Portalı hesaplarınızı yönetebilirsiniz.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirmesi Procore SSO ile yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliğiniz
- Bir Procore SSO çoklu oturum açma etkin aboneliği

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Bu gerekli olmadığı sürece üretim ortamınızı kullanmamanız gerekir.
- Azure AD deneme ortamı yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden Procore SSO ekleme
2. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-procore-sso-from-the-gallery"></a>Galeriden Procore SSO ekleme
Azure AD'de Procore SSO tümleştirmesini yapılandırmak için Procore SSO Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden Procore SSO eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure Yönetim Portalı](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
3. Tıklayın **Ekle** iletişim kutusunun üst kısmındaki düğmesi.

    ![Uygulamalar][3]

4. Arama kutusuna **Procore SSO**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/procoresso-tutorial/tutorial_procoresso_search.png)

5. Sonuçlar panelinde seçin **Procore SSO**ve ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/procoresso-tutorial/tutorial_procoresso_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Yapılandırma ve test Azure AD çoklu oturum açma
Bu bölümde, yapılandırın ve Azure AD çoklu oturum açma Procore "Britta Simon" adlı bir test kullanıcı tabanlı SSO ile test edin.

Tek iş için oturum açma için Azure AD ne Procore SSO karşılık gelen kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısının Procore SSO ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Değerini atayarak bu bağlantı ilişki kurulduktan **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** Procore SSO içinde.

Yapılandırma ve Azure AD çoklu oturum açma Procore SSO ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Procore SSO test kullanıcısı oluşturma](#creating-a-procore-sso-test-user)**  - Azure AD gösterimini her için bağlı Procore SSO Britta simon'un bir karşılığı vardır.
4. **[Azure AD test kullanıcı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açma testi](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure yönetim portalında etkinleştirin ve Procore SSO uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma Procore SSO ile yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure Yönetim Portalı'nda üzerinde **Procore SSO** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** için çoklu oturum açmayı etkinleştirme.
 
    ![Çoklu oturum açmayı yapılandırın](./media/procoresso-tutorial/tutorial_procoresso_samlbase.png)

3. Üzerinde **Procore SSO etki alanı ve URL'ler** bölümü, kullanıcı gerekmez uygulama zaten Azure ile önceden tümleştirilmiştir gibi tüm adımları gerçekleştirin.

    ![Çoklu oturum açmayı yapılandırın](./media/procoresso-tutorial/tutorial_procoresso_url.png)

4. Üzerinde **SAML imzalama sertifikası** bölümünde **meta veri XML** ve bilgisayarınızda XML dosyasını kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/procoresso-tutorial/tutorial_procoresso_certificate.png) 

5. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/procoresso-tutorial/tutorial_general_400.png)

6. Üzerinde **Procore SSO yapılandırma** bölümünde **yapılandırma Procore SSO** açmak için **yapılandırma oturum açma** penceresi. Kopyalama **SAML varlık kimliği ve SAML çoklu oturum açma hizmeti URL'si** gelen **hızlı başvuru bölümü.**

    ![Çoklu oturum açmayı yapılandırın](./media/procoresso-tutorial/tutorial_procoresso_configure.png) 

7. Çoklu oturum açmayı yapılandırma **Procore SSO** tarafı, procore şirketinizin sitesi yönetici olarak oturum açın.

8. Araç kutusu açılan listeden, aşağı, tıklayarak **yönetici** SSO ayarlar sayfasını açın.

    ![Çoklu oturum açmayı yapılandırın](./media/procoresso-tutorial/procore_tool_admin.png)

9. Aşağıdaki - açıklandığı kutularında değerleri yapıştırın

    ![Çoklu oturum açmayı yapılandırın](./media/procoresso-tutorial/procore_setting_admin.png)  

    a. İçinde **tek oturum üzerinde veren URL'si** kutusu, yapıştırma SAML varlık kimliği, Azure portalından kopyalanır.

    b. İçinde **hedef URL üzerinde SAML oturum** kutusu, yapıştırma ve SAML çoklu oturum açma hizmeti URL'si Azure portalından kopyalanır.

    c. Artık **meta veri XML** yukarıda Azure portalından indirilen ve adlı etiketinde sertifikası kopyalamak **X509Certificate**. Kopyalanan değer içine yapıştırın **çoklu oturum açma x509 sertifika** kutusu.

10. Tıklayarak **değişiklikleri kaydetmek**.

11. Bu ayarlar sonra göndermesi gerekiyor. **etki alanı adı** (ör. **contoso.com**) için Procore içine, oturum açtığınız aracılığıyla [Procore Destek ekibine](https://support.procore.com/) ve bunlar etki alanı Federasyon SSO'yu etkinleştirin.

<!--### Next steps

To ensure users can sign-in to Procore SSO after it has been configured to use Azure Active Directory, review the following tasks and topics:

- User accounts must be pre-provisioned into Procore SSO prior to sign-in. To set this up, see Provisioning.
 
- Users must be assigned access to Procore SSO in Azure AD to sign-in. To assign users, see Users.
 
- To configure access polices for Procore SSO users, see Access Policies.
 
- For additional information on deploying single sign-on to users, see [this article](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#deploying-azure-ad-integrated-applications-to-users).-->


### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, bir test kullanıcısı Britta Simon adlı Azure Yönetim Portalı'nda oluşturmaktır.

![Azure AD kullanıcısı oluşturun][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure Yönetim Portalı**, sol gezinti bölmesinde **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/procoresso-tutorial/create_aaduser_01.png) 

2. Git **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar** kullanıcılar listesini görüntüleyin.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/procoresso-tutorial/create_aaduser_02.png) 

3. İletişim kutusunun en üstünde tıklayın **Ekle** açmak için **kullanıcı** iletişim.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/procoresso-tutorial/create_aaduser_03.png) 

4. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/procoresso-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.
 
### <a name="creating-a-procore-sso-test-user"></a>Procore SSO test kullanıcısı oluşturma

Lütfen izleyin kendi tarafında Procore test kullanıcısı oluşturmak için aşağıdaki adımları.

1. Procore şirketinizin sitesi yönetici olarak oturum açın.  

2. Araç kutusu açılan listeden, aşağı, tıklayarak **dizin** şirket dizin sayfasını açın.

    ![Çoklu oturum açmayı yapılandırın](./media/procoresso-tutorial/Procore_sso_directory.png)

3. Tıklayarak **bir kişi ekleyin** seçeneği formu açın ve girmek için seçenekler - gerçekleştirin

    ![Çoklu oturum açmayı yapılandırın](./media/procoresso-tutorial/Procore_user_add.png)

    a. İçinde **ad** metin türü kullanıcının adı gibi **Britta**.

    b. İçinde **Soyadı** metin türü kullanıcının soyadını gibi **Simon**.

    c. İçinde **e-posta adresi** metin türü kullanıcının e-posta adresi ister **BrittaSimon@contoso.com**.

    d. Seçin **izin şablonu** olarak **daha sonra izin şablonu Uygula**.

    e. **Oluştur**’a tıklayın.

4. Kontrol edin ve yeni eklenen kişi ayrıntılarını güncelleştirin.

    ![Çoklu oturum açmayı yapılandırın](./media/procoresso-tutorial/Procore_user_check.png)

5. Tıklayarak **Kaydet ve Gönder daveti** (davet e-posta aracılığıyla gerekliyse) veya **Kaydet** (doğrudan kullanıcı kaydı tamamlamak için Kaydet).
    
    ![Çoklu oturum açmayı yapılandırın](./media/procoresso-tutorial/Procore_user_save.png)  

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcı atama

Bu bölümde, Azure çoklu oturum açma kullanmak için Procore SSO erişim vererek Britta Simon etkinleştirin.

![Kullanıcı Ata][200] 

**Britta Simon Procore SSO atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure Yönetim Portalı'nda uygulamaları görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

2. Uygulamalar listesinde **Procore SSO**.

    ![Çoklu oturum açmayı yapılandırın](./media/procoresso-tutorial/tutorial_procoresso_app.png) 

3. Soldaki menüde **kullanıcılar ve gruplar**.

    ![Kullanıcı Ata][202] 

4. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Kullanıcı Ata][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Çoklu oturum açma ayarları test etmek isterseniz, erişim Paneli'nde açın. Erişim paneli hakkında daha fazla ayrıntı için bkz. [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md). Erişim panelinde Procore SSO kutucuğa tıkladığınızda, otomatik olarak Procore SSO uygulamanıza açan.

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/procoresso-tutorial/tutorial_general_01.png
[2]: ./media/procoresso-tutorial/tutorial_general_02.png
[3]: ./media/procoresso-tutorial/tutorial_general_03.png
[4]: ./media/procoresso-tutorial/tutorial_general_04.png

[100]: ./media/procoresso-tutorial/tutorial_general_100.png

[200]: ./media/procoresso-tutorial/tutorial_general_200.png
[201]: ./media/procoresso-tutorial/tutorial_general_201.png
[202]: ./media/procoresso-tutorial/tutorial_general_202.png
[203]: ./media/procoresso-tutorial/tutorial_general_203.png

