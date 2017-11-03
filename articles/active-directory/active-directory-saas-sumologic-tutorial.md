---
title: "Öğretici: Azure Active Directory Tümleştirme ile SumoLogic | Microsoft Docs"
description: "Çoklu oturum açma Azure Active Directory ile SumoLogic arasında yapılandırmayı öğrenin."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: fbb76765-92d7-4801-9833-573b11b4d910
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/08/2017
ms.author: jeedes
ms.openlocfilehash: e739106472ccf930b2942eb810dd844f2b1ade7c
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-sumologic"></a>Öğretici: Azure Active Directory Tümleştirme SumoLogic ile

Bu öğreticide, Azure Active Directory (Azure AD) ile SumoLogic tümleştirmek öğrenin.

SumoLogic Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- SumoLogic erişimi, Azure AD'de kontrol edebilirsiniz
- Otomatik olarak için SumoLogic (çoklu oturum açma) ile Azure AD hesaplarına açan kullanıcılarınıza etkinleştirebilirsiniz
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirme SumoLogic ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir SumoLogic çoklu oturum açma abonelik etkin

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden SumoLogic ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-sumologic-from-the-gallery"></a>Galeriden SumoLogic ekleme
Azure AD SumoLogic tümleştirilmesi yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden SumoLogic eklemeniz gerekir.

**Galeriden SumoLogic eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Uygulamalar][3]

4. Arama kutusuna **SumoLogic**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-sumologic-tutorial/tutorial_sumologic_search.png)

5. Sonuçlar panelinde seçin **SumoLogic**ve ardından **Ekle** uygulama eklemek için düğmesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-sumologic-tutorial/tutorial_sumologic_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Çoklu oturum açmayı yapılandırma ve Azure AD sınama
Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı SumoLogic sınayın.

Tekli çalışmaya oturum için Azure AD SumoLogic karşılık gelen kullanıcı için bir kullanıcı Azure AD'de nedir bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının SumoLogic ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

SumoLogic içinde değerini atayın **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** bağlantı ilişkisi oluşturmak için.

Yapılandırma ve Azure AD çoklu oturum açma SumoLogic ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[SumoLogic test kullanıcısı oluşturma](#creating-a-sumologic-test-user)**  - Britta Simon, karşılık gelen kullanıcı Azure AD gösterimini bağlı SumoLogic sağlamak için.
4. **[Azure AD test kullanıcısı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açmayı test](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma SumoLogic uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma ile SumoLogic yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **SumoLogic** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-sumologic-tutorial/tutorial_sumologic_samlbase.png)

3. Üzerinde **SumoLogic etki alanı ve URL'leri** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-sumologic-tutorial/tutorial_sumologic_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın:`https://<tenantname>.SumoLogic.com`

    b. İçinde **tanımlayıcısı** metin kutusuna, URL şu biçimi kullanarak bir yazın:
    | |
    |--|
    | `https://<tenantname>.us2.sumologic.com` |
    | `https://<tenantname>.sumologic.com` |
    | `https://<tenantname>.us4.sumologic.com` |
    | `https://<tenantname>.eu.sumologic.com` |
    | `https://<tenantname>.au.sumologic.com` |

    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler gerçek oturum açma URL'si ve tanımlayıcı ile güncelleştirin. Kişi [SumoLogic istemci destek ekibi](https://www.sumologic.com/contact-us/) bu değerleri almak için. 
 
4. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **sertifika (Base64)** ve sertifika dosyayı bilgisayarınıza kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-sumologic-tutorial/tutorial_sumologic_certificate.png) 

5. Tıklatın **kaydetmek** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-sumologic-tutorial/tutorial_general_400.png)

6. Üzerinde **SumoLogic yapılandırma** 'yi tıklatın **yapılandırma SumoLogic** açmak için **yapılandırma oturum açma** penceresi. Kopya **SAML varlık kimliği ve SAML çoklu oturum açma hizmet URL'si** gelen **hızlı başvuru bölümü.**

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-sumologic-tutorial/tutorial_sumologic_configure.png) 

7. Farklı web tarayıcısı penceresinde SumoLogic şirket sitenize yönetici olarak oturum açın.

8. Git **yönetmek \> güvenlik**.
   
    ![Yönetme](./media/active-directory-saas-sumologic-tutorial/ic778556.png "yönetme")

9. Tıklatın **SAML**.
   
    ![Genel güvenlik ayarları](./media/active-directory-saas-sumologic-tutorial/ic778557.png "genel güvenlik ayarları")

10. Gelen **bir yapılandırma seçin veya yeni bir tane oluşturun** listesinde, seçin **Azure AD**ve ardından **yapılandırma**.
   
    ![SAML 2.0 yapılandırma](./media/active-directory-saas-sumologic-tutorial/ic778558.png "SAML 2.0 yapılandırma")

11. Üzerinde **yapılandırma SAML 2.0** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:
   
    ![SAML 2.0 yapılandırma](./media/active-directory-saas-sumologic-tutorial/ic778559.png "SAML 2.0 yapılandırma")
   
    a. İçinde **yapılandırma adı** metin kutusuna, türü **Azure AD**. 

    b. Seçin **hata ayıklama modu**.

    c. İçinde **veren** metin değerini yapıştırın **SAML varlık kimliği**, Azure portalından kopyalanan. 

    d. İçinde **Authn istek URL'si** metin değerini yapıştırın **SAML çoklu oturum açma hizmet URL'si**, Azure portalından kopyalanan.

    e. Base-64 kodlanmış sertifikanızı Not Defteri'nde açın, içeriğini, panoya kopyalayın ve sonra tüm sertifika içine yapıştırabilirsiniz **X.509 sertifikası** metin kutusu.

    f. Olarak **e-posta özniteliği**seçin **kullanım SAML konu**.  

    g. Seçin **SP tarafından başlatılan oturum açma yapılandırması**.

    h. İçinde **oturum açma yolunu** metin kutusuna, türü **Azure** tıklatıp **kaydetmek**.

> [!TIP]
> Şimdi bu yönergeleri içinde kısa bir sürümünü okuyabilirsiniz [Azure portal](https://portal.azure.com)uygulaması kuruluyor yaparken!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** sekmesinde ve aracılığıyla katıştırılmış belgelere erişebilir **yapılandırma** alt bölüm. Daha fazla bilgiyi burada embedded belgeler özelliği hakkında: [Azure AD embedded belgeler]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

![Azure AD Kullanıcı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portal**, sol gezinti bölmesinde tıklatın **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-sumologic-tutorial/create_aaduser_01.png) 

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-sumologic-tutorial/create_aaduser_02.png) 

3. Açmak için **kullanıcı** iletişim kutusunda, tıklatın **Ekle** iletişim kutusunun üst kısmında.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-sumologic-tutorial/create_aaduser_03.png) 

4. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-sumologic-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna, türü **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna, türü **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**'a tıklayın.
 
### <a name="creating-a-sumologic-test-user"></a>SumoLogic test kullanıcısı oluşturma

Azure AD kullanıcıları için SumoLogic oturum açmak etkinleştirmek için bunlar için SumoLogic sağlanmalıdır.  

* SumoLogic söz konusu olduğunda, sağlama bir el ile bir görevdir.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1. Oturum, **SumoLogic** Kiracı.

2. Git **yönetmek \> kullanıcılar**.
   
    ![Kullanıcıların](./media/active-directory-saas-sumologic-tutorial/ic778561.png "kullanıcılar")

3. **Ekle**'ye tıklayın.
   
    ![Kullanıcıların](./media/active-directory-saas-sumologic-tutorial/ic778562.png "kullanıcılar")

4. Üzerinde **yeni kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:
   
    ![Yeni kullanıcı](./media/active-directory-saas-sumologic-tutorial/ic778563.png "yeni kullanıcı") 
 
    a. İlgili bilgi sağlamayı istediğiniz Azure AD hesabının yazın **ad**, **Soyadı**, ve **e-posta** metin kutuları.
  
    b. Bir rol seçin.
  
    c. Olarak **durum**seçin **etkin**.
  
    d. **Kaydet** düğmesine tıklayın.

>[!NOTE]
>API sağlama AAD kullanıcı hesaplarına SumoLogic tarafından sağlanan veya herhangi diğer SumoLogic kullanıcı hesabı oluşturma araçlarını kullanabilirsiniz. 
> 

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atama

Bu bölümde, Britta SumoLogic için erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı atama][200] 

**SumoLogic için Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **SumoLogic**.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-sumologic-tutorial/tutorial_sumologic_app.png) 

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    ![Kullanıcı atama][202] 

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Kullanıcı atama][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açmayı test etme

Bu bölümün amacı erişim paneli kullanılarak Azure AD çoklu oturum açma yapılandırmanızı test etmektir.

Erişim paneli SumoLogic parçasında tıklattığınızda, otomatik olarak SumoLogic uygulamanıza açan.

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](active-directory-saas-tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-sumologic-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-sumologic-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-sumologic-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-sumologic-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-sumologic-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-sumologic-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-sumologic-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-sumologic-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-sumologic-tutorial/tutorial_general_203.png

