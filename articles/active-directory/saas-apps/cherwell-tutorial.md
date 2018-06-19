---
title: 'Öğretici: Azure Active Directory Tümleştirme ile Cherwell | Microsoft Docs'
description: Çoklu oturum açma Azure Active Directory ile Cherwell arasında yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: ad891f99-179e-4487-834d-35f3bc01c1ec
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/03/2017
ms.author: jeedes
ms.openlocfilehash: 5cb36ccafd18247e75519c6e38449b850d1a06b1
ms.sourcegitcommit: c851842d113a7078c378d78d94fea8ff5948c337
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2018
ms.locfileid: "35971888"
---
# <a name="tutorial-azure-active-directory-integration-with-cherwell"></a>Öğretici: Azure Active Directory Tümleştirme Cherwell ile

Bu öğreticide, Azure Active Directory (Azure AD) ile Cherwell tümleştirmek öğrenin.

Cherwell Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Cherwell erişimi, Azure AD'de kontrol edebilirsiniz
- Otomatik olarak için Cherwell (çoklu oturum açma) ile Azure AD hesaplarına açan kullanıcılarınıza etkinleştirebilirsiniz
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirme Cherwell ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir Cherwell çoklu oturum açma etkin abonelik

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden Cherwell ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-cherwell-from-the-gallery"></a>Galeriden Cherwell ekleme
Azure AD Cherwell tümleştirilmesi yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden Cherwell eklemeniz gerekir.

**Galeriden Cherwell eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Uygulamalar][3]

4. Arama kutusuna **Cherwell**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/cherwell-tutorial/tutorial_cherwell_search.png)

5. Sonuçlar panelinde seçin **Cherwell**ve ardından **Ekle** uygulama eklemek için düğmesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/cherwell-tutorial/tutorial_cherwell_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Çoklu oturum açmayı yapılandırma ve Azure AD sınama
Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon." olarak adlandırılan bir test kullanıcı tabanlı Cherwell ile test etme

Tekli çalışmaya oturum için Azure AD Cherwell karşılık gelen kullanıcı için bir kullanıcı Azure AD'de nedir bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının Cherwell ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Cherwell içinde değerini atayın **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** bağlantı ilişkisi oluşturmak için.

Yapılandırma ve Azure AD çoklu oturum açma Cherwell ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Cherwell test kullanıcısı oluşturma](#creating-a-cherwell-test-user)**  - Britta Simon, karşılık gelen kullanıcı Azure AD gösterimini bağlı Cherwell sağlamak için.
4. **[Azure AD test kullanıcısı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açmayı test](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma Cherwell uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma ile Cherwell yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **Cherwell** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/cherwell-tutorial/tutorial_cherwell_samlbase.png)

3. Üzerinde **Cherwell etki alanı ve URL'leri** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/cherwell-tutorial/tutorial_cherwell_url.png)

    İçinde **oturum açma URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://<companyname>.cherwellondemand.com/cherwellclient`

    > [!NOTE] 
    > Bu değer gerçek değil. Bu değer gerçek oturum açma URL'si ile güncelleştirin. Kişi [Cherwell destek ekibi](https://csm.cherwell.com/contact) bu değeri alınamıyor.
 
4. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **sertifika (Base64)** ve sertifika dosyayı bilgisayarınıza kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/cherwell-tutorial/tutorial_cherwell_certificate.png) 

5. Tıklatın **kaydetmek** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/cherwell-tutorial/tutorial_general_400.png)

6. Üzerinde **Cherwell yapılandırma** 'yi tıklatın **yapılandırma Cherwell** açmak için **yapılandırma oturum açma** penceresi. Kopya **SAML varlık kimliği ve SAML çoklu oturum açma hizmet URL'si** gelen **hızlı başvuru bölümü.**

    ![Çoklu oturum açmayı yapılandırın](./media/cherwell-tutorial/tutorial_cherwell_configure.png) 

7. Çoklu oturum açma yapılandırmak için **Cherwell** yan, indirilen göndermek için ihtiyacınız **sertifika (Base64)**, **SAML çoklu oturum açma hizmet URL'si**, ve **SAML varlık kimliği** için [Cherwell destek ekibi](https://csm.cherwell.com/contact). 

    >[!NOTE]
    >Gerçek SSO yapılandırmasını yapmak Cherwell destek ekibinize sahiptir. SSO, aboneliğiniz için etkinleştirildiğinde, bir bildirim alırsınız.
    > 
    
> [!TIP]
> Şimdi bu yönergeleri içinde kısa bir sürümünü okuyabilirsiniz [Azure portal](https://portal.azure.com)uygulaması kuruluyor yaparken!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** sekmesinde ve aracılığıyla katıştırılmış belgelere erişebilir **yapılandırma** alt bölüm. Daha fazla bilgiyi burada embedded belgeler özelliği hakkında: [Azure AD embedded belgeler]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

![Azure AD Kullanıcı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portal**, sol gezinti bölmesinde tıklatın **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/cherwell-tutorial/create_aaduser_01.png) 

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/cherwell-tutorial/create_aaduser_02.png) 

3. Açmak için **kullanıcı** iletişim kutusunda, tıklatın **Ekle** iletişim kutusunun üst kısmında.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/cherwell-tutorial/create_aaduser_03.png) 

4. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/cherwell-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna, türü **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna, türü **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.
 
### <a name="creating-a-cherwell-test-user"></a>Cherwell test kullanıcısı oluşturma

Azure AD kullanıcıları için Cherwell oturum açmak etkinleştirmek için bunların Cherwell sağlanmalıdır.

Cherwell söz konusu olduğunda, kullanıcı hesapları tarafından oluşturulması gerektiğini, [Cherwell destek ekibi](https://csm.cherwell.com/contact).

>[!NOTE]
>API tarafından Cherwell sağlamak için Azure Active Directory kullanıcı hesapları sağlanan veya herhangi diğer Cherwell kullanıcı hesabı oluşturma araçlarını kullanabilirsiniz.

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atama

Bu bölümde, Britta Cherwell için erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı atama][200] 

**Cherwell için Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **Cherwell**.

    ![Çoklu oturum açmayı yapılandırın](./media/cherwell-tutorial/tutorial_cherwell_app.png) 

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    ![Kullanıcı atama][202] 

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Kullanıcı atama][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açmayı test etme

Çoklu oturum açma ayarlarınızı test etmek isterseniz, erişim paneli açın. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/cherwell-tutorial/tutorial_general_01.png
[2]: ./media/cherwell-tutorial/tutorial_general_02.png
[3]: ./media/cherwell-tutorial/tutorial_general_03.png
[4]: ./media/cherwell-tutorial/tutorial_general_04.png

[100]: ./media/cherwell-tutorial/tutorial_general_100.png

[200]: ./media/cherwell-tutorial/tutorial_general_200.png
[201]: ./media/cherwell-tutorial/tutorial_general_201.png
[202]: ./media/cherwell-tutorial/tutorial_general_202.png
[203]: ./media/cherwell-tutorial/tutorial_general_203.png

