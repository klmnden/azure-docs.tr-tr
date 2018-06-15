---
title: 'Öğretici: Azure Active Directory Tümleştirme ile AnswerHub | Microsoft Docs'
description: Çoklu oturum açma Azure Active Directory ile AnswerHub arasında yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 818b91d7-01df-4b36-9706-f167c710a73c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/16/2017
ms.author: jeedes
ms.openlocfilehash: c4d63b5a51a6cf6626c42b0ef39587f1e760d238
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2018
ms.locfileid: "34338610"
---
# <a name="tutorial-azure-active-directory-integration-with-answerhub"></a>Öğretici: Azure Active Directory Tümleştirme AnswerHub ile

Bu öğreticide, Azure Active Directory (Azure AD) ile AnswerHub tümleştirmek öğrenin.

AnswerHub Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- AnswerHub erişimi, Azure AD'de kontrol edebilirsiniz
- Otomatik olarak için AnswerHub (çoklu oturum açma) ile Azure AD hesaplarına açan kullanıcılarınıza etkinleştirebilirsiniz
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirme AnswerHub ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir AnswerHub çoklu oturum açma abonelik etkin

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden AnswerHub ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-answerhub-from-the-gallery"></a>Galeriden AnswerHub ekleme
Azure AD AnswerHub tümleştirilmesi yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden AnswerHub eklemeniz gerekir.

**Galeriden AnswerHub eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Uygulamalar][3]

4. Arama kutusuna **AnswerHub**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-answerhub-tutorial/tutorial_answerhub_search.png)

5. Sonuçlar panelinde seçin **AnswerHub**ve ardından **Ekle** uygulama eklemek için düğmesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-answerhub-tutorial/tutorial_answerhub_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Çoklu oturum açmayı yapılandırma ve Azure AD sınama
Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı AnswerHub sınayın.

Tekli çalışmaya oturum için Azure AD AnswerHub karşılık gelen kullanıcı için bir kullanıcı Azure AD'de nedir bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının AnswerHub ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

AnswerHub içinde değerini atayın **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** bağlantı ilişkisi oluşturmak için.

Yapılandırma ve Azure AD çoklu oturum açma AnswerHub ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Bir AnswerHub test kullanıcısı oluşturma](#creating-an-answerhub-test-user)**  - Britta Simon, karşılık gelen kullanıcı Azure AD gösterimini bağlı AnswerHub sağlamak için.
4. **[Azure AD test kullanıcısı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açmayı test](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma AnswerHub uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma ile AnswerHub yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **AnswerHub** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-answerhub-tutorial/tutorial_answerhub_samlbase.png)

3. Üzerinde **AnswerHub etki alanı ve URL'leri** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-answerhub-tutorial/tutorial_answerhub_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://<company>.answerhub.com`

    b. İçinde **tanımlayıcısı** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://<company>.answerhub.com`

    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler gerçek oturum açma URL'si ve tanımlayıcı ile güncelleştirin. Kişi [AnswerHub istemci destek ekibi](mailto:success@answerhub.com) bu değerleri almak için. 
 
4. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **Certificate(Base64)** ve sertifika dosyayı bilgisayarınıza kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-answerhub-tutorial/tutorial_answerhub_certificate.png) 

5. Tıklatın **kaydetmek** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-answerhub-tutorial/tutorial_general_400.png)

6. Üzerinde **AnswerHub yapılandırma** 'yi tıklatın **yapılandırma AnswerHub** açmak için **yapılandırma oturum açma** penceresi. Kopya **Sign-Out URL ve SAML çoklu oturum açma hizmet URL'si** gelen **hızlı başvuru bölümü.**

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-answerhub-tutorial/tutorial_answerhub_configure.png) 

7. Farklı web tarayıcısı penceresinde AnswerHub şirket sitenize yönetici olarak oturum açın.
   
    >[!NOTE]
    >AnswerHub yapılandırmada yardıma gereksinim duyarsanız başvurun [AnswerHub'in destek ekibi](mailto:success@answerhub.com.).
   
8. Git **Yönetim**.

9. Tıklatın **kullanıcı ve grup** sekmesi.

10. Sol taraftaki gezinti bölmesinde de **sosyal ayarları** 'yi tıklatın **SAML Kurulumu**.

11. Tıklatın **IDP Config** sekmesi.

12. Üzerinde **IDP Config** sekmesinde, aşağıdaki adımları gerçekleştirin:

     ![SAML Kurulumu](./media/active-directory-saas-answerhub-tutorial/ic785172.png "SAML Kurulumu")  
  
     a. İçinde **IDP oturum açma URL'si** metin kutusuna, Yapıştır **SAML çoklu oturum açma hizmet URL'si** Azure portalından kopyalanan.
  
     b. İçinde **IDP oturum kapatma URL'si** metin kutusuna, Yapıştır **Sign-Out URL** Azure portalından kopyaladığınız değeri.
     
     c. İçinde **IDP ad tanımlayıcısı biçimi** metin kutusu, kullanıcı tanımlayıcısı olarak seçili Azure Portalı'nda aynı değeri girin **kullanıcı öznitelikleri** bölümü.
  
     d. Tıklatın **anahtarlar ve Sertifikalar**.

13. Anahtarlar ve sertifikalar sekmesinde, aşağıdaki adımları gerçekleştirin:
    
     ![Anahtarlar ve Sertifikalar](./media/active-directory-saas-answerhub-tutorial/ic785173.png "anahtarlar ve sertifikalar")  
 
     a. Not Defteri'nde, Azure Portalı'ndan indirilen, base-64 kodlanmış sertifika açmak içeriğini, panoya kopyalayın ve yapıştırın kendisine **IDP ortak anahtarı (x 509 biçimi)** metin kutusu.
  
     b. **Kaydet**’e tıklayın.

14. Üzerinde **IDP Config** sekmesini tıklatın, **kaydetmek**.

> [!TIP]
> Şimdi bu yönergeleri içinde kısa bir sürümünü okuyabilirsiniz [Azure portal](https://portal.azure.com)uygulaması kuruluyor yaparken!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** sekmesinde ve aracılığıyla katıştırılmış belgelere erişebilir **yapılandırma** alt bölüm. Daha fazla bilgiyi burada embedded belgeler özelliği hakkında: [Azure AD embedded belgeler]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

![Azure AD Kullanıcı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portal**, sol gezinti bölmesinde tıklatın **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-answerhub-tutorial/create_aaduser_01.png) 

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-answerhub-tutorial/create_aaduser_02.png) 

3. Açmak için **kullanıcı** iletişim kutusunda, tıklatın **Ekle** iletişim kutusunun üst kısmında.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-answerhub-tutorial/create_aaduser_03.png) 

4. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-answerhub-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna, türü **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna, türü **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.
 
### <a name="creating-an-answerhub-test-user"></a>Bir AnswerHub test kullanıcısı oluşturma

Azure AD kullanıcıları için AnswerHub oturum açmak etkinleştirmek için bunların AnswerHub sağlanmalıdır.  
AnswerHub söz konusu olduğunda, sağlama bir el ile bir görevdir.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1. Oturum, **AnswerHub** yönetici olarak şirket site.

2. Git **Yönetim**.

3. Tıklatın **kullanıcıları ve grupları** sekmesi.

4. Sol taraftaki gezinti bölmesinde de **kullanıcıları yönetme** 'yi tıklatın **oluşturma veya içeri aktarma kullanıcı**.
   
   ![Kullanıcıları ve grupları](./media/active-directory-saas-answerhub-tutorial/ic785175.png "kullanıcıları ve grupları")

5. Tür **e-posta adresi**, **kullanıcıadı** ve **parola** ilgili metin kutularına sağlamak ve ardından istediğiniz geçerli bir Azure Active Directory hesap **kaydetmek**.

>[!NOTE]
>API sağlama AAD kullanıcı hesaplarına AnswerHub tarafından sağlanan veya herhangi diğer AnswerHub kullanıcı hesabı oluşturma araçlarını kullanabilirsiniz.

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atama

Bu bölümde, Britta AnswerHub için erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı atama][200] 

**AnswerHub için Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **AnswerHub**.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-answerhub-tutorial/tutorial_answerhub_app.png) 

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    ![Kullanıcı atama][202] 

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Kullanıcı atama][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açmayı test etme

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli AnswerHub parçasında tıklattığınızda, otomatik olarak AnswerHub uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](active-directory-saas-tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/active-directory-saas-answerhub-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-answerhub-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-answerhub-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-answerhub-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-answerhub-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-answerhub-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-answerhub-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-answerhub-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-answerhub-tutorial/tutorial_general_203.png

