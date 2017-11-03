---
title: "Öğretici: Azure Active Directory Tümleştirme ile Zscaler özel erişim (ZPA) | Microsoft Docs"
description: "Çoklu oturum açma Azure Active Directory ile Zscaler özel erişim (ZPA) arasında yapılandırmayı öğrenin."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 83711115-1c4f-4dd7-907b-3da24b37c89e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/06/2017
ms.author: jeedes
ms.openlocfilehash: 5c598bfa5b6725d21a89df54dbcb3314cc631d80
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-zscaler-private-access-zpa"></a>Öğretici: Azure Active Directory Tümleştirme ile Zscaler özel erişim (ZPA)

Bu öğreticide, Azure Active Directory (Azure AD) ile Zscaler özel erişim (ZPA) tümleştirme öğrenin.

Zscaler özel erişim (ZPA) Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Erişimi için Zscaler özel erişim (ZPA), Azure AD'de kontrol edebilirsiniz
- Azure AD hesaplarına otomatik olarak Zscaler özel erişim (ZPA için) (çoklu oturum açma) açan kullanıcılarınıza etkinleştirebilirsiniz
- Hesaplarınızı bir merkezi konumda - Azure Yönetim Portalı'nı yönetme

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirme Zscaler özel erişim (ZPA ile) yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir Zscaler özel erişim (ZPA) çoklu oturum açma etkin abonelik


> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.


Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Bu gerekli olmadığı sürece, üretim ortamınızın kullanmamanız gerekir.
- Bir Azure AD deneme ortam yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden Zscaler özel erişim (ZPA) ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama


## <a name="adding-zscaler-private-access-zpa-from-the-gallery"></a>Galeriden Zscaler özel erişim (ZPA) ekleme
Azure AD tümleştirilmesi, Zscaler özel erişim (ZPA) yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden Zscaler özel erişim (ZPA) eklemeniz gerekir.

**Galeriden Zscaler özel erişim (ZPA) eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure Yönetim Portalı](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
3. Tıklatın **Ekle** iletişim kutusunun üst kısmında düğmesi.

    ![Uygulamalar][3]

4. Arama kutusuna **Zscaler özel erişim (ZPA)**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_zscalerprivateaccess_001.png)

5. Sonuçlar panelinde seçin **Zscaler özel erişim (ZPA)**ve ardından **Ekle** uygulama eklemek için düğmesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_zscalerprivateaccess_0001.png)


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Çoklu oturum açmayı yapılandırma ve Azure AD sınama
Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma ile Zscaler özel erişim ("Britta Simon" adlı bir test kullanıcı tabanlı ZPA) test etme.

Tekli çalışmaya oturum için Azure AD içinde Zscaler özel erişim (ZPA) karşılık gelen kullanıcı için bir kullanıcı Azure AD'de nedir bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının ve ilgili kullanıcı içinde Zscaler özel erişim (ZPA) arasında bir bağlantı ilişkisi kurulması gerekir.

Bu bağlantı değeri atayarak ilişkisi **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** içinde Zscaler özel erişim (ZPA).

Yapılandırmak ve Azure AD çoklu oturum açma ile Zscaler özel erişim (ZPA) sınamak için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Zscaler özel erişim (ZPA) test kullanıcısı oluşturma](#creating-a-zscaler-private-access-(zpa)-test-user)**  - Britta Simon, karşılık gelen içinde Zscaler özel erişim (Azure AD gösterimini her için bağlantılı ZPA) sahip.
4. **[Azure AD test kullanıcısı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açmayı test](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure Yönetim Portalı'nda etkinleştirin ve çoklu oturum açma Zscaler özel erişim (ZPA) uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma Zscaler özel erişim (ZPA ile) yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure Yönetim Portalı'nda üzerinde **Zscaler özel erişim (ZPA)** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda, olarak **modu** seçin **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_general_300.png)
    
3. Üzerinde **Zscaler özel erişim (ZPA) etki alanı ve URL'leri** bölümünde, aşağıdaki adımları gerçekleştirin:
    
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_zscalerprivateaccess_01.png)

    a. İçinde **oturum üzerinde URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın:`https://samlsp.private.zscaler.com/auth/login?domain=<your-domain-name>`

    b. İçinde **tanımlayıcısı** metin kutusuna, türü:`https://samlsp.private.zscaler.com/auth/metadata`

    > [!NOTE] 
    > Lütfen bu gerçek değerlerin olmadığına dikkat edin. Bu değerler gerçek oturum üzerinde URL ve tanımlayıcıdır ile güncelleştirmeniz gerekir. Burada URL benzersiz değeri tanımlayıcıda kullanmanızı öneririz. Kişi [Zscaler özel erişim (ZPA) destek ekibi](https://help.zscaler.com/zpa-submit-ticket) bu değerleri almak için.

4. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **yeni sertifika oluştur**.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_general_400.png)   

5. Üzerinde **yeni sertifika oluştur** iletişim kutusunda, Takvim simgesine tıklayın ve bir **sona erme tarihi**. Ardından **kaydetmek** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_general_500.png)

6. Üzerinde **SAML imzalama sertifikası** bölümünde, select **yeni sertifika etkin hale getirin** tıklatıp **kaydetmek** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_zscalerprivateaccess_02.png)

7. Açılır pencere üzerinde **geçiş sertifikası** penceresinde tıklatın **Tamam**.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_general_600.png)

8. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **meta veri XML** ve meta veri dosyası, bilgisayarınıza kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_zscalerprivateaccess_03.png) 

9. Farklı web tarayıcısı penceresinde Zscaler özel erişim (ZPA) şirket sitenize yönetici olarak oturum açın.

10. Gidin **yönetici** ve ardından **IDP yapılandırma**.

    ![Çoklu oturum açma uygulama tarafında yapılandırma](./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_zscalerprivateaccess_04.png)

11. İçinde **IDP yapılandırma** 'yi tıklatın **yeni IDP Yapılandırması Ekle**.

    ![Çoklu oturum açma uygulama tarafında yapılandırma](./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_zscalerprivateaccess_05.png)

12. İçinde **yeni IDP yapılandırma** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açma uygulama tarafında yapılandırma](./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_zscalerprivateaccess_06.png)

    a. Tıklatın **Dosya Seç** ve indirilen meta veri dosyasını karşıya yükleyin.

    b. Tıklatın **kaydetmek** düğmesi.
    


### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure Yönetim Portalı'nda bir test kullanıcı oluşturmaktır.

![Azure AD Kullanıcı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure Yönetim Portalı**, sol gezinti bölmesinde tıklatın **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-zscalerprivateaccess-tutorial/create_aaduser_01.png) 

2. Git **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar** kullanıcıların listesini görüntülemek için.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-zscalerprivateaccess-tutorial/create_aaduser_02.png) 

3. İletişim kutusunun üstündeki **Ekle** açmak için **kullanıcı** iletişim.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-zscalerprivateaccess-tutorial/create_aaduser_03.png) 

4. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-zscalerprivateaccess-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna, türü **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna, türü **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**'a tıklayın. 



### <a name="creating-a-zscaler-private-access-zpa-test-user"></a>Zscaler özel erişim (ZPA) test kullanıcısı oluşturma

Bu bölümde, Britta Simon Zscaler özel erişim (ZPA içinde) adlı bir kullanıcı oluşturun. Lütfen çalışmak [Zscaler özel erişim (ZPA) destek ekibi](https://help.zscaler.com/zpa-submit-ticket) Zscaler özel erişim (ZPA) platform kullanıcıları eklemek için.


### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atama

Bu bölümde, Britta Zscaler özel erişim (ZPA) her erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı atama][200] 

**Britta Simon Zscaler özel erişim (ZPA) atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure Yönetim Portalı'nda uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **Zscaler özel erişim (ZPA)**.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_zscalerprivateaccess_50.png) 

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    ![Kullanıcı atama][202] 

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Kullanıcı atama][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    


### <a name="testing-single-sign-on"></a>Çoklu oturum açmayı test etme

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli Zscaler özel erişim (ZPA) parçasında tıklattığınızda, otomatik olarak Zscaler özel erişim (ZPA) uygulamanıza açan.


## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](active-directory-saas-tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_general_203.png