---
title: 'Öğretici: Azure Active Directory tümleştirmesiyle AnswerHub | Microsoft Docs'
description: Azure Active Directory ve AnswerHub arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 818b91d7-01df-4b36-9706-f167c710a73c
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/16/2017
ms.author: jeedes
ms.openlocfilehash: 447a3911bc1f021fb1ca2658716de1910b5379b6
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/14/2018
ms.locfileid: "39044093"
---
# <a name="tutorial-azure-active-directory-integration-with-answerhub"></a>Öğretici: Azure Active Directory AnswerHub ile tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile AnswerHub tümleştirme konusunda bilgi edinin.

Azure AD ile AnswerHub tümleştirme ile aşağıdaki avantajları sağlar:

- AnswerHub erişimi, Azure AD'de denetleyebilirsiniz
- Otomatik olarak imzalanan için AnswerHub (çoklu oturum açma) ile Azure AD hesaplarına açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilirsiniz.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile AnswerHub yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliğiniz
- Abonelik bir AnswerHub çoklu oturum açma etkin

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden AnswerHub ekleme
2. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-answerhub-from-the-gallery"></a>Galeriden AnswerHub ekleme
Azure AD'de AnswerHub tümleştirmesini yapılandırmak için AnswerHub Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden AnswerHub eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Uygulamalar][3]

4. Arama kutusuna **AnswerHub**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/answerhub-tutorial/tutorial_answerhub_search.png)

5. Sonuçlar panelinde seçin **AnswerHub**ve ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/answerhub-tutorial/tutorial_answerhub_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Yapılandırma ve test Azure AD çoklu oturum açma
Bu bölümde, yapılandırın ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı AnswerHub sınayın.

Tek iş için oturum açma için Azure AD ne AnswerHub karşılığı kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısının AnswerHub ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

AnswerHub içinde değerini atayın **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** bağlantı kurmak için.

Yapılandırma ve Azure AD çoklu oturum açma AnswerHub ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Bir AnswerHub test kullanıcısı oluşturma](#creating-an-answerhub-test-user)**  - kullanıcı Azure AD gösterimini bağlı AnswerHub Britta simon'un bir karşılığı vardır.
4. **[Azure AD test kullanıcı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açma testi](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve AnswerHub uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma ile AnswerHub yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **AnswerHub** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/answerhub-tutorial/tutorial_answerhub_samlbase.png)

3. Üzerinde **AnswerHub etki alanı ve URL'ler** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/answerhub-tutorial/tutorial_answerhub_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://<company>.answerhub.com`

    b. İçinde **tanımlayıcı** metin kutusuna bir URL şu biçimi kullanarak: `https://<company>.answerhub.com`

    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler gerçek oturum açma URL'si ve tanımlayıcı ile güncelleştirin. İlgili kişi [AnswerHub istemci Destek ekibine](mailto:success@answerhub.com) bu değerleri almak için. 
 
4. Üzerinde **SAML imzalama sertifikası** bölümünde **Certificate(Base64)** ve bilgisayarınızdaki sertifika dosyasını kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/answerhub-tutorial/tutorial_answerhub_certificate.png) 

5. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/answerhub-tutorial/tutorial_general_400.png)

6. Üzerinde **AnswerHub yapılandırma** bölümünde **yapılandırma AnswerHub** açmak için **yapılandırma oturum açma** penceresi. Kopyalama **oturum kapatma URL'si ve SAML çoklu oturum açma hizmeti URL'si** gelen **hızlı başvuru bölümü.**

    ![Çoklu oturum açmayı yapılandırın](./media/answerhub-tutorial/tutorial_answerhub_configure.png) 

7. Farklı bir web tarayıcı penceresinde AnswerHub şirket sitenize yönetici olarak oturum.
   
    >[!NOTE]
    >AnswerHub yapılandırırken Yardım gerekirse başvurun [AnswerHub'ın Destek ekibine](mailto:success@answerhub.com.).
   
8. Git **Yönetim**.

9. Tıklayın **kullanıcı ve grup** sekmesi.

10. Sol taraftaki gezinti bölmesinde, **sosyal ayarları** bölümünde **SAML Kurulumu**.

11. Tıklayın **IDP yapılandırma** sekmesi.

12. Üzerinde **IDP yapılandırma** sekmesinde, aşağıdaki adımları gerçekleştirin:

     ![SAML Kurulumu](./media/answerhub-tutorial/ic785172.png "SAML Kurulumu")  
  
     a. İçinde **IDP'nin oturum açma URL'si** metin kutusu, yapıştırma **SAML çoklu oturum açma hizmeti URL'si** , Azure Portalı'ndan kopyaladığınız.
  
     b. İçinde **IDP oturum kapatma URL'si** metin kutusu, yapıştırma **oturum kapatma URL'si** Azure Portalı'ndan kopyaladığınız değeri.
     
     c. İçinde **IDP ad tanımlayıcı biçimi** metin kullanıcı tanımlayıcısı olarak seçili Azure portalında aynı değeri girin **kullanıcı öznitelikleri** bölümü.
  
     d. Tıklayın **anahtarlara ve sertifikalara**.

13. Anahtarlara ve sertifikalara sekmesinde aşağıdaki adımları gerçekleştirin:
    
     ![Anahtarlara ve sertifikalara](./media/answerhub-tutorial/ic785173.png "anahtarlar ve sertifikalar")  
 
     a. Not Defteri'nde, Azure portalından indirilen, base-64 kodlanmış sertifika açın içeriğini sizin panoya kopyalayın ve yapıştırın kendisine **IDP ortak anahtarı (x 509 biçimi)** metin.
  
     b. **Kaydet**’e tıklayın.

14. Üzerinde **IDP yapılandırma** sekmesinde **Kaydet**.

> [!TIP]
> İçindeki bu yönergeleri kısa bir sürümünü artık okuyabilir [Azure portalında](https://portal.azure.com), uygulamayı hazırlama ayarladığınız sırada!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** aracılığıyla katıştırılmış belgelere erişebilir ve sekmesinde  **Yapılandırma** alttaki bölümü. Daha fazla bilgi edinebilirsiniz embedded belgeleri özelliği hakkında: [Azure AD'ye embedded belgeleri]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

![Azure AD kullanıcısı oluşturun][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portalında**, sol gezinti bölmesinde **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/answerhub-tutorial/create_aaduser_01.png) 

2. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/answerhub-tutorial/create_aaduser_02.png) 

3. Açmak için **kullanıcı** iletişim kutusunda, tıklayın **Ekle** iletişim kutusunun üst kısmındaki.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/answerhub-tutorial/create_aaduser_03.png) 

4. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/answerhub-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.
 
### <a name="creating-an-answerhub-test-user"></a>Bir AnswerHub test kullanıcısı oluşturma

AnswerHub için oturum açmak Azure AD kullanıcılarının etkinleştirmek için bunların AnswerHub sağlanması gerekir.  
AnswerHub söz konusu olduğunda, sağlama bir el ile gerçekleştirilen bir görevdir.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1. Oturum açın, **AnswerHub** şirketinizin sitesi yöneticisi olarak.

2. Git **Yönetim**.

3. Tıklayın **kullanıcıları ve grupları** sekmesi.

4. Sol taraftaki gezinti bölmesinde, **Kullanıcıları Yönet** bölümünde **oluşturun veya içeri aktarma kullanıcı**.
   
   ![Kullanıcıları ve grupları](./media/answerhub-tutorial/ic785175.png "kullanıcıları ve grupları")

5. Tür **e-posta adresi**, **kullanıcıadı** ve **parola** ilgili metin kutularına sağlayın ve ardından istediğinizgeçerlibirAzureActiveDirectoryhesabı **Kaydet**.

>[!NOTE]
>Herhangi diğer AnswerHub kullanıcı hesabı oluşturma araçları kullanabilir veya API'leri için AAD kullanıcı hesapları sağlamak AnswerHub tarafından sağlanan.

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcı atama

Bu bölümde, Azure çoklu oturum açma kullanmak için AnswerHub erişim vererek Britta Simon etkinleştirin.

![Kullanıcı Ata][200] 

**Britta Simon AnswerHub için atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

2. Uygulamalar listesinde **AnswerHub**.

    ![Çoklu oturum açmayı yapılandırın](./media/answerhub-tutorial/tutorial_answerhub_app.png) 

3. Soldaki menüde **kullanıcılar ve gruplar**.

    ![Kullanıcı Ata][202] 

4. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Kullanıcı Ata][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde AnswerHub kutucuğa tıkladığınızda, otomatik olarak AnswerHub uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/answerhub-tutorial/tutorial_general_01.png
[2]: ./media/answerhub-tutorial/tutorial_general_02.png
[3]: ./media/answerhub-tutorial/tutorial_general_03.png
[4]: ./media/answerhub-tutorial/tutorial_general_04.png

[100]: ./media/answerhub-tutorial/tutorial_general_100.png

[200]: ./media/answerhub-tutorial/tutorial_general_200.png
[201]: ./media/answerhub-tutorial/tutorial_general_201.png
[202]: ./media/answerhub-tutorial/tutorial_general_202.png
[203]: ./media/answerhub-tutorial/tutorial_general_203.png

