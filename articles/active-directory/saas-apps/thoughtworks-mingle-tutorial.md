---
title: 'Öğretici: Azure Active Directory Tümleştirme Thoughtworks Mingle ile | Microsoft Docs'
description: Çoklu oturum açma Azure Active Directory arasındaki Thoughtworks Mingle yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: 69d859d9-b7f7-4c42-bc8c-8036138be586
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: jeedes
ms.openlocfilehash: f4729440bf523cef3f2721111852f4e5f445b5fb
ms.sourcegitcommit: c851842d113a7078c378d78d94fea8ff5948c337
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2018
ms.locfileid: "35979872"
---
# <a name="tutorial-azure-active-directory-integration-with-thoughtworks-mingle"></a>Öğretici: Azure Active Directory Tümleştirme Thoughtworks Mingle ile

Bu öğreticide, Azure Active Directory (Azure AD) ile tümleştirme Thoughtworks Mingle öğrenin.

Azure AD ile tümleştirme Thoughtworks Mingle ile aşağıdaki avantajları sağlar:

- Thoughtworks Mingle erişimi, Azure AD'de kontrol edebilirsiniz
- Azure AD hesaplarına otomatik olarak (çoklu oturum açma) için Thoughtworks Mingle açan kullanıcılarınıza etkinleştirebilirsiniz
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirme Thoughtworks Mingle ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir Thoughtworks Mingle çoklu oturum açma abonelik etkin

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, şunları yapabilirsiniz [bir aylık deneme sürümünü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden Thoughtworks Mingle ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-thoughtworks-mingle-from-the-gallery"></a>Galeriden Thoughtworks Mingle ekleme
Thoughtworks Mingle tümleştirilmesi Azure AD'ye yapılandırmak için Thoughtworks Mingle Galeriden yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

**Thoughtworks Mingle Galeriden eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **Thoughtworks Mingle**seçin **Thoughtworks Mingle** sonuç panelinden ardından **Ekle** uygulama eklemek için düğmeyi.

    ![Thoughtworks Mingle sonuçlar listesinde](./media/thoughtworks-mingle-tutorial/tutorial_thoughtworksmingle_addfromgallery.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme
Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı Thoughtworks Mingle ile test etme.

Tekli çalışmaya oturum için Azure AD Thoughtworks Mingle karşılık gelen kullanıcı için bir kullanıcı Azure AD'de nedir bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının ilgili Thoughtworks Mingle kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Thoughtworks Mingle içinde değerini atayın **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** bağlantı ilişkisi oluşturmak için.

Yapılandırma ve Azure AD çoklu oturum açma Thoughtworks Mingle ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Thoughtworks Mingle test kullanıcısı oluşturma](#create-a-thoughtworks-mingle-test-user)**  - Thoughtworks kullanıcı Azure AD gösterimini bağlantılı Mingle Britta Simon, karşılık gelen sağlamak için.
4. **[Azure AD test kullanıcısı atayın](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Test çoklu oturum açma](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma Thoughtworks Mingle uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma Thoughtworks Mingle ile yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **Thoughtworks Mingle** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/thoughtworks-mingle-tutorial/tutorial_thoughtworksmingle_samlbase.png)

3. Üzerinde **Thoughtworks Mingle etki alanı ve URL'leri** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Thoughtworks Mingle etki alanı ve URL'leri tek oturum açma bilgilerini](./media/thoughtworks-mingle-tutorial/tutorial_thoughtworksmingle_url.png)

    İçinde **oturum açma URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://<companyname>.mingle.thoughtworks.com`

    > [!NOTE] 
    > Değer gerçek değil. Değerin gerçek oturum açma URL'si ile güncelleştirin. Kişi [Thoughtworks Mingle istemci destek ekibi](https://support.thoughtworks.com/hc/categories/201743486-Mingle-Community-Support) değeri alınamıyor. 
 
4. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **meta veri XML** ve meta veri dosyası, bilgisayarınıza kaydedin.

    ![Sertifika indirme bağlantısı](./media/thoughtworks-mingle-tutorial/tutorial_thoughtworksmingle_certificate.png) 

5. Tıklatın **kaydetmek** düğmesi.

    ![Oturum açma tek Kaydet düğmesi yapılandırın](./media/thoughtworks-mingle-tutorial/tutorial_general_400.png)

6. Oturum, **Thoughtworks Mingle** yönetici olarak şirket site.

7. Tıklatın **yönetici** sekmesini ve ardından **SSO Config**.
   
    ![Yönetici sekmesine](./media/thoughtworks-mingle-tutorial/ic785157.png "SSO yapılandırma")

8. İçinde **SSO Config** bölümünde, aşağıdaki adımları gerçekleştirin:
   
    ![SSO Config](./media/thoughtworks-mingle-tutorial/ic785158.png "SSO yapılandırma")
    
    a. Meta veri dosyası karşıya yüklemek için tıklayın **dosya**. 

    b. Tıklatın **değişiklikleri kaydetmek**.

> [!TIP]
> Şimdi bu yönergeleri içinde kısa bir sürümünü okuyabilirsiniz [Azure portal](https://portal.azure.com)uygulaması kuruluyor yaparken!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** sekmesinde ve aracılığıyla katıştırılmış belgelere erişebilir **yapılandırma** alt bölüm. Daha fazla bilgiyi burada embedded belgeler özelliği hakkında: [Azure AD embedded belgeler]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portal**, sol gezinti bölmesinde tıklatın **Azure Active Directory** simgesi.

    ![Azure Active Directory düğmesi](./media/thoughtworks-mingle-tutorial/create_aaduser_01.png) 

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantılar](./media/thoughtworks-mingle-tutorial/create_aaduser_02.png) 

3. Açmak için **kullanıcı** iletişim kutusunda, tıklatın **Ekle** iletişim kutusunun üst kısmında.
 
    ![Ekle düğmesi](./media/thoughtworks-mingle-tutorial/create_aaduser_03.png) 

4. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Kullanıcı iletişim kutusu](./media/thoughtworks-mingle-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna, türü **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna, türü **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.
 
### <a name="create-a-thoughtworks-mingle-test-user"></a>Thoughtworks Mingle test kullanıcısı oluşturma

Azure AD kullanıcıların oturum açabilmesi için bunlar Azure Active Directory kullanıcı adlarını kullanarak Thoughtworks Mingle uygulama sağlanmalıdır. Thoughtworks Mingle söz konusu olduğunda, sağlama bir el ile bir görevdir.

**Kullanıcı sağlamayı yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Thoughtworks Mingle şirket sitenize yönetici olarak oturum açın.

2. tıklatın **profil**.
   
    ![İlk projenizi](./media/thoughtworks-mingle-tutorial/ic785160.png "ilk projenizi")

3. Tıklatın **yönetici** sekmesini ve ardından **kullanıcılar**.
   
    ![Kullanıcıların](./media/thoughtworks-mingle-tutorial/ic785161.png "kullanıcılar")

4. Tıklatın **yeni kullanıcı**.
   
    ![Yeni kullanıcı](./media/thoughtworks-mingle-tutorial/ic785162.png "yeni kullanıcı")

5. Üzerinde **yeni kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
   
    ![Yeni kullanıcı iletişim kutusu](./media/thoughtworks-mingle-tutorial/ic785163.png "yeni kullanıcı")  
 
    a. Tür **oturum açma adı**, **görünen adı**, **Seç parola**, **parolayı onayla** geçerli bir Azure AD hesabının sağlamak istediğiniz ilgili metin kutuları. 

    b. Olarak **kullanıcı türü**seçin **tam kullanıcı**.

    c. Tıklatın **bu profili oluşturma**.

>[!NOTE]
>API Thoughtworks Mingle tarafından sağlanan sağlama AAD kullanıcı hesaplarına veya herhangi diğer Thoughtworks Mingle kullanıcı hesabı oluşturma araçlarını kullanabilirsiniz.
> 

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Britta Thoughtworks Mingle için erişim izni verme, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**Thoughtworks Mingle için Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **Thoughtworks Mingle**.

    ![Uygulamalar listesinde Thoughtworks Mingle bağlantı](./media/thoughtworks-mingle-tutorial/tutorial_thoughtworksmingle_app.png) 

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantı][202] 

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Ekleme atama bölmesi][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümün amacı erişim paneli kullanılarak Azure AD çoklu oturum açma yapılandırmanızı test etmektir.

Erişim paneli Thoughtworks Mingle parçasında tıklattığınızda, otomatik olarak Thoughtworks Mingle uygulamanıza açan.

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/thoughtworks-mingle-tutorial/tutorial_general_01.png
[2]: ./media/thoughtworks-mingle-tutorial/tutorial_general_02.png
[3]: ./media/thoughtworks-mingle-tutorial/tutorial_general_03.png
[4]: ./media/thoughtworks-mingle-tutorial/tutorial_general_04.png

[100]: ./media/thoughtworks-mingle-tutorial/tutorial_general_100.png

[200]: ./media/thoughtworks-mingle-tutorial/tutorial_general_200.png
[201]: ./media/thoughtworks-mingle-tutorial/tutorial_general_201.png
[202]: ./media/thoughtworks-mingle-tutorial/tutorial_general_202.png
[203]: ./media/thoughtworks-mingle-tutorial/tutorial_general_203.png

