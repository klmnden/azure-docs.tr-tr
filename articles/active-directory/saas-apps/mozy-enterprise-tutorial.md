---
title: 'Öğretici: Azure Active Directory Tümleştirme Mozy kuruluş ile | Microsoft Docs'
description: Çoklu oturum açma Azure Active Directory ile Mozy kuruluş arasında yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 489b5e62-85c2-45c9-8766-326632d48114
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2017
ms.author: jeedes
ms.openlocfilehash: 6c814de45fe91990af61bb47c10a43e81e6c4159
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/19/2018
ms.locfileid: "36224725"
---
# <a name="tutorial-azure-active-directory-integration-with-mozy-enterprise"></a>Öğretici: Azure Active Directory Tümleştirme Mozy kuruluş ile

Bu öğreticide, Azure Active Directory (Azure AD) ile Mozy Kurumsal tümleştirme öğrenin.

Mozy kuruluş Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Mozy Kurumsal erişimi, Azure AD'de kontrol edebilirsiniz
- Azure AD hesaplarına otomatik olarak Mozy kuruluş (çoklu oturum açma) açan kullanıcılarınıza etkinleştirebilirsiniz
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirme Mozy Enterprise ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir Mozy Kurumsal çoklu oturum açma abonelik etkin

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden Mozy Kurumsal ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-mozy-enterprise-from-the-gallery"></a>Galeriden Mozy Kurumsal ekleme
Azure AD Mozy Kurumsal tümleştirilmesi yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden Mozy Kurumsal eklemeniz gerekir.

**Galeriden Mozy Kurumsal eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Uygulamalar][3]

4. Arama kutusuna **Mozy Kurumsal**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/mozy-enterprise-tutorial/tutorial_mozyenterprise_search.png)

5. Sonuçlar panelinde seçin **Mozy Kurumsal**ve ardından **Ekle** uygulama eklemek için düğmesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/mozy-enterprise-tutorial/tutorial_mozyenterprise_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Çoklu oturum açmayı yapılandırma ve Azure AD sınama
Bu bölümde, yapılandırmak ve Mozy kuruluş ile Azure AD çoklu oturum açmayı test "Britta Simon" adlı bir test kullanıcı tabanlı.

Tekli çalışmaya oturum için Azure AD ne karşılık gelen Mozy kuruluştaki bir kullanıcı için Azure AD içinde olduğu bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının ve ilgili kullanıcı Mozy kuruluştaki arasında bir bağlantı ilişkisi kurulması gerekir.

Değeri Mozy kuruluşta atayın **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** bağlantı ilişkisi oluşturmak için.

Yapılandırma ve Azure AD çoklu oturum açma Mozy kuruluş ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Mozy Kurumsal test kullanıcısı oluşturma](#creating-a-mozy-enterprise-test-user)**  - Britta Simon, karşılık gelen kullanıcı Azure AD gösterimini bağlı Mozy Kurumsal sağlamak için.
4. **[Azure AD test kullanıcısı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açmayı test](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma Mozy Kurumsal uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma Mozy Enterprise ile yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **Mozy Kurumsal** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/mozy-enterprise-tutorial/tutorial_mozyenterprise_samlbase.png)

3. Üzerinde **Mozy kuruluş etki alanı ve URL'leri** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/mozy-enterprise-tutorial/tutorial_mozyenterprise_url.png)

    İçinde **oturum açma URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://<tenantname>.Mozyenterprise.com`

    > [!NOTE] 
    > Bu değer gerçek değil. Bu değer gerçek oturum açma URL'si ile güncelleştirin. Kişi [Mozy Kurumsal İstemci destek ekibi](http://support.mozy.com/) bu değeri alınamıyor.

4. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **Certificate(Base64)** ve sertifika dosyayı bilgisayarınıza kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/mozy-enterprise-tutorial/tutorial_mozyenterprise_certificate.png) 

5. Tıklatın **kaydetmek** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/mozy-enterprise-tutorial/tutorial_general_400.png)

6. Üzerinde **Mozy Kurumsal yapılandırma** 'yi tıklatın **Mozy Kurumsal yapılandırma** açmak için **yapılandırma oturum açma** penceresi. Kopya **SAML varlık kimliği ve SAML çoklu oturum açma hizmet URL'si** gelen **hızlı başvuru bölümü.**

    ![Çoklu oturum açmayı yapılandırın](./media/mozy-enterprise-tutorial/tutorial_mozyenterprise_configure.png) 

7. Farklı web tarayıcısı penceresinde Mozy Kurumsal şirket sitenize yönetici olarak oturum açın.

8. İçinde **yapılandırma** 'yi tıklatın **kimlik doğrulama İlkesi**.
   
   ![Kimlik doğrulama İlkesi](./media/mozy-enterprise-tutorial/ic777314.png "kimlik doğrulama İlkesi")

9. Üzerinde **kimlik doğrulama İlkesi** bölümünde, aşağıdaki adımları gerçekleştirin:
   
   ![Kimlik doğrulama İlkesi](./media/mozy-enterprise-tutorial/ic777315.png "kimlik doğrulama İlkesi")
   
   a. Seçin **dizin hizmeti** olarak **sağlayıcı**.
   
   b. Seçin **LDAP göndermeyi kullanmak**.
   
   c. Tıklatın **SAML kimlik doğrulaması** sekmesi.
   
   d. Yapıştır **SAML çoklu oturum açma hizmet URL'si**, Azure portalından kopyalanan **kimlik doğrulaması URL'sini** metin kutusu.
   
   e. Yapıştır **SAML varlık kimliği**, Azure portalından kopyalanan **SAML Endpoint** metin kutusu.
   
   f. İndirilen base-64 kodlanmış sertifika Not Defteri'nde açın, içeriğini, panoya kopyalayın ve sonra tüm sertifika içine yapıştırabilirsiniz **SAML sertifika** metin kutusu.
   
   g. Seçin **etkinleştirmek SSO kendi ağ kimlik bilgileriyle oturum yöneticileri için**.
   
   h. Tıklatın **değişiklikleri kaydetmek**.

> [!TIP]
> Şimdi bu yönergeleri içinde kısa bir sürümünü okuyabilirsiniz [Azure portal](https://portal.azure.com)uygulaması kuruluyor yaparken!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** sekmesinde ve aracılığıyla katıştırılmış belgelere erişebilir **yapılandırma** alt bölüm. Daha fazla bilgiyi burada embedded belgeler özelliği hakkında: [Azure AD embedded belgeler]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

![Azure AD Kullanıcı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portal**, sol gezinti bölmesinde tıklatın **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/mozy-enterprise-tutorial/create_aaduser_01.png) 

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/mozy-enterprise-tutorial/create_aaduser_02.png) 

3. Açmak için **kullanıcı** iletişim kutusunda, tıklatın **Ekle** iletişim kutusunun üst kısmında.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/mozy-enterprise-tutorial/create_aaduser_03.png) 

4. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/mozy-enterprise-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna, türü **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna, türü **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.
 
### <a name="creating-a-mozy-enterprise-test-user"></a>Mozy Kurumsal test kullanıcısı oluşturma

Azure AD kullanıcıların Mozy kuruma oturum etkinleştirmek için bunlar Mozy kuruma sağlanmalıdır. Mozy Enterprise söz konusu olduğunda, sağlama bir el ile bir görevdir.

>[!NOTE]
>API sağlama AAD kullanıcı hesaplarına Mozy kuruluş tarafından sağlanan veya herhangi diğer Mozy Kurumsal kullanıcı hesabı oluşturma araçlarını kullanabilirsiniz.

**Kullanıcı hesaplarını sağlamak için aşağıdaki adımları gerçekleştirin:**

1. Oturum, **Mozy Kurumsal** Kiracı.

2. Tıklatın **kullanıcılar**ve ardından **yeni kullanıcı Ekle**.
   
   ![Kullanıcıların](./media/mozy-enterprise-tutorial/ic777317.png "kullanıcılar")
   
   >[!NOTE]
   >**Yeni kullanıcı Ekle** seçeneği ise yalnızca görüntülenen **Mozy** altında sağlayıcı olarak seçilen **kimlik doğrulama İlkesi**. SAML kimlik doğrulaması yapılandırılmışsa, ardından kullanıcıları otomatik olarak kendi ilk oturum açma aracılığıyla çoklu oturum açma üzerinde üzerinde eklenir.
    
3. Yeni kullanıcı iletişim kutusunda aşağıdaki adımları gerçekleştirin:
   
   ![Kullanıcıları ekleme](./media/mozy-enterprise-tutorial/ic777318.png "kullanıcı ekleme")
   
   a. Gelen **bir grubu seçin** listesinde, bir grup seçin.
   
   b. Gelen **kullanıcı türüne** listesinde, bir tür seçin.
   
   c. İçinde **kullanıcıadı** metin kutusuna, Azure AD kullanıcı adını yazın.
   
   d. İçinde **e-posta** metin kutusuna, Azure AD kullanıcısının e-posta adresini yazın.
   
   e. Seçin **kullanıcı yönerge e-posta Gönder**.
   
   f. Tıklatın **kullanıcıları eklemek**.

     >[!NOTE]
     > Kullanıcı oluşturduktan sonra onu etkinleştirilmeden önce hesabı onaylamak için bir bağlantı içeren Azure AD kullanıcı bir e-posta gönderilir.

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atama

Bu bölümde, Britta Mozy kuruluş erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı atama][200] 

**Mozy kuruluş Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **Mozy Kurumsal**.

    ![Çoklu oturum açmayı yapılandırın](./media/mozy-enterprise-tutorial/tutorial_mozyenterprise_app.png) 

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    ![Kullanıcı atama][202] 

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Kullanıcı atama][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açmayı test etme

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli Mozy Kurumsal parçasında tıkladığınızda, Mozy Kurumsal uygulama oturum açma sayfasında almanız gerekir.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/mozy-enterprise-tutorial/tutorial_general_01.png
[2]: ./media/mozy-enterprise-tutorial/tutorial_general_02.png
[3]: ./media/mozy-enterprise-tutorial/tutorial_general_03.png
[4]: ./media/mozy-enterprise-tutorial/tutorial_general_04.png

[100]: ./media/mozy-enterprise-tutorial/tutorial_general_100.png

[200]: ./media/mozy-enterprise-tutorial/tutorial_general_200.png
[201]: ./media/mozy-enterprise-tutorial/tutorial_general_201.png
[202]: ./media/mozy-enterprise-tutorial/tutorial_general_202.png
[203]: ./media/mozy-enterprise-tutorial/tutorial_general_203.png

