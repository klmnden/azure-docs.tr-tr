---
title: 'Öğretici: Azure Active Directory Tümleştirme ile TOPdesk - genel | Microsoft Docs'
description: Çoklu oturum açma Azure Active Directory ile TOPdesk - genel arasında yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: 0873299f-ce70-457b-addc-e57c5801275f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/25/2017
ms.author: jeedes
ms.openlocfilehash: 76b75ffde280047f8ed6c2d4173e905df1d7a658
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2018
---
# <a name="tutorial-azure-active-directory-integration-with-topdesk---public"></a>Öğretici: Azure Active Directory Tümleştirme ile TOPdesk - genel

Bu öğreticide, Azure Active Directory (Azure AD) ile ortak TOPdesk - tümleştirmek öğrenin.

TOPdesk - genel Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- TOPdesk - genel erişimi, Azure AD'de kontrol edebilirsiniz.
- Otomatik olarak TOPdesk - Azure AD hesaplarına sahip (çoklu oturum açma) ortak için açan kullanıcılarınıza etkinleştirebilirsiniz.
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile TOPdesk - yapılandırmak için genel, aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- TOPdesk - genel çoklu oturum açma etkin abonelik

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, şunları yapabilirsiniz [bir aylık deneme sürümünü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Ortak galerisinden TOPdesk - ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-topdesk---public-from-the-gallery"></a>Ortak galerisinden TOPdesk - ekleme
TOPdesk - Azure AD'ye ortak tümleştirmesini yapılandırmak için TOPdesk - galerisinden ortak yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**TOPdesk - Galerisi'nden ortak eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **TOPdesk - genel**seçin **TOPdesk - genel** sonuç panelinden ardından **Ekle** uygulama eklemek için düğmeyi.

    ![TOPdesk - Genel sonuçlar listesinde](./media/active-directory-saas-topdesk-public-tutorial/tutorial_topdesk-public_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma TOPdesk ile test etme - genel "Britta Simon" adlı bir test kullanıcı tabanlı.

Çoklu oturum açma çalışmak özelliğini için Azure AD TOPdesk ne karşılık gelen kullanıcı bilmek ister - genel bir kullanıcıya Azure AD'de. Diğer bir deyişle, bir Azure AD kullanıcısının TOPdesk - ilgili kullanıcı arasında bir bağlantı ilişkisi ortak kurulması gerekir.

TOPdesk içinde-ortak değerini atayın **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** bağlantı ilişkisi oluşturmak için.

Yapılandırma ve Azure AD çoklu oturum açma TOPdesk ile-test etmek için genel, aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[TOPdesk - genel test kullanıcısı oluşturma](#create-a-topdesk---public-test-user)**  - Britta Simon, karşılık gelen TOPdesk - kullanıcı Azure AD gösterimini bağlantılı ortak sağlamak için.
4. **[Azure AD test kullanıcısı atayın](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Test çoklu oturum açma](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma, TOPdesk - genel uygulama yapılandırın.

**Azure AD çoklu oturum açma ile TOPdesk - yapılandırmak için genel, aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **TOPdesk - genel** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/active-directory-saas-topdesk-public-tutorial/tutorial_topdesk-public_samlbase.png)

3. Üzerinde **TOPdesk - ortak etki alanı ve URL'leri** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![TOPdesk - ortak etki alanı ve URL'ler tek oturum açma bilgileri](./media/active-directory-saas-topdesk-public-tutorial/tutorial_topdesk-public_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://<companyname>.topdesk.net`
    
    b. İçinde **tanımlayıcısı** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://<companyname>.topdesk.net/tas/public/login/verify`

    c. İçinde **yanıt URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://<companyname>.topdesk.net/tas/public/login/saml`
     
    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler, gerçek tanımlayıcı, yanıt URL'si ve oturum açma URL'si ile güncelleştirin. Yanıt açıklaması daha sonra öğreticide URL'dir. Kişi [TOPdesk - ortak istemci destek ekibi](https://help.topdesk.com/saas/enterprise/user/) bu değerleri almak için.  

4. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **meta veri XML** ve meta veri dosyası, bilgisayarınıza kaydedin.

    ![Sertifika indirme bağlantısı](./media/active-directory-saas-topdesk-public-tutorial/tutorial_topdesk-public_certificate.png) 

5. Tıklatın **kaydetmek** düğmesi.

    ![Oturum açma tek Kaydet düğmesi yapılandırın](./media/active-directory-saas-topdesk-public-tutorial/tutorial_general_400.png)
    
6. Üzerinde **TOPdesk - genel yapılandırması** 'yi tıklatın **TOPdesk - ortak yapılandırma** açmak için **yapılandırma oturum açma** penceresi. Kopya **Sign-Out URL, SAML varlık kimliği ve SAML çoklu oturum açma hizmet URL'si** gelen **hızlı başvuru bölümü.**

    ![TOPdesk - genel yapılandırması](./media/active-directory-saas-topdesk-public-tutorial/tutorial_topdesk-public_configure.png) 

7. Oturum, **TOPdesk - genel** yönetici olarak şirket site.

8. İçinde **TOPdesk** menüsünde tıklatın **ayarları**.
   
    ![Ayarları](./media/active-directory-saas-topdesk-public-tutorial/ic790598.png "ayarları")

9. Tıklatın **oturum açma ayarları**.
   
    ![Oturum açma ayarları](./media/active-directory-saas-topdesk-public-tutorial/ic790599.png "oturum açma ayarları")

10. Genişletme **oturum açma ayarları** menüsüne ve ardından **genel**.
   
    ![Genel](./media/active-directory-saas-topdesk-public-tutorial/ic790600.png "genel")

11. İçinde **ortak** bölümünü **SAML oturum açma** yapılandırma bölümünde, aşağıdaki adımları gerçekleştirin:
   
    ![Teknik ayarları](./media/active-directory-saas-topdesk-public-tutorial/ic790601.png "teknik ayarları")
   
    a. Tıklatın **karşıdan** ortak meta veri dosyası indirip bilgisayarınıza yerel olarak kaydedin.
   
    b. İndirilen meta veri dosyasını açın ve ardından bulun **AssertionConsumerService** düğümü.

    ![AssertionConsumerService](./media/active-directory-saas-topdesk-public-tutorial/ic790619.png "AssertionConsumerService")
   
    c. Kopya **AssertionConsumerService** değeri, bu değeri yapıştırın **yanıt URL'si** metin kutusuna **TOPdesk - ortak etki alanı ve URL'leri** bölümü.      
   
12. Bir sertifika dosyası oluşturmak için aşağıdaki adımları gerçekleştirin:
    
    ![Sertifika](./media/active-directory-saas-topdesk-public-tutorial/ic790606.png "sertifika")
    
    a. Azure Portalı'ndan indirilen meta veri dosyası açın.
    
    b. Genişletme **RoleDescriptor** sahip düğümü bir **xsi: type** , **ssas'nin: ApplicationServiceType**.
    
    c. Değerini kopyalayın **X509Certificate** düğümü.
    
    d. Kopyalanan Kaydet **X509Certificate** yerel olarak bilgisayarınızda bir dosyada değeri.

13. İçinde **ortak** 'yi tıklatın **Ekle**.
    
    ![SAML oturum açma](./media/active-directory-saas-topdesk-public-tutorial/ic790625.png "SAML oturum açma")

14. Üzerinde **SAML Yapılandırması Yardımcısı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
    
    ![SAML Yapılandırması Yardımcısı](./media/active-directory-saas-topdesk-public-tutorial/ic790608.png "SAML Yapılandırması Yardımcısı")
    
    a. Azure Portalı'ndan indirilen meta veri dosyanızı altında karşıya yüklemek için **Federasyon meta verileri**, tıklatın **Gözat**.

    b. Sertifika dosyanızın altında karşıya yüklemek için **sertifika (RSA)**, tıklatın **Gözat**.

    c. Aldığınız TOPdesk destek ekibinden altında logosu dosyayı karşıya yüklemeyi **Logo simgesini**, tıklatın **Gözat**.

    d. İçinde **kullanıcı adı özniteliği** metin kutusuna, türü `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`.

    e. İçinde **görünen adı** metin kutusuna, yapılandırmanız için bir ad yazın.

    f. **Kaydet**’e tıklayın.

> [!TIP]
> Şimdi bu yönergeleri içinde kısa bir sürümünü okuyabilirsiniz [Azure portal](https://portal.azure.com)uygulaması kuruluyor yaparken!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** sekmesinde ve aracılığıyla katıştırılmış belgelere erişebilir **yapılandırma** alt bölüm. Daha fazla bilgiyi burada embedded belgeler özelliği hakkında: [Azure AD embedded belgeler]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Sol bölmede, Azure portal'ı tıklatın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/active-directory-saas-topdesk-public-tutorial/create_aaduser_01.png)

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantılar](./media/active-directory-saas-topdesk-public-tutorial/create_aaduser_02.png)

3. Açmak için **kullanıcı** iletişim kutusu, tıklatın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/active-directory-saas-topdesk-public-tutorial/create_aaduser_03.png)

4. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/active-directory-saas-topdesk-public-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** kullanıcı Britta Simon e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değer aşağı yazma **parola** kutusu.

    d. **Oluştur**’a tıklayın.
 
### <a name="create-a-topdesk---public-test-user"></a>TOPdesk - genel test kullanıcısı oluşturma

Azure AD kullanıcıların TOPdesk - oturum etkinleştirmek için ortak, bunlar sağlanmalıdır TOPdesk - genel.  
TOPdesk - söz konusu olduğunda ortak, sağlama olduğunu el ile bir görev.

### <a name="to-configure-user-provisioning-perform-the-following-steps"></a>Kullanıcı sağlamayı yapılandırmak için aşağıdaki adımları gerçekleştirin:
1. Oturum, **TOPdesk - genel** yönetici olarak şirket site.

2. Üstteki menüde tıklatın **TOPdesk \> yeni \> destek dosyalarını \> kişi**.
   
    ![Kişi](./media/active-directory-saas-topdesk-public-tutorial/ic790628.png "kişi")

3. Yeni bir kişiye iletişim kutusunda, aşağıdaki adımları gerçekleştirin:
   
    ![Yeni bir kişiye](./media/active-directory-saas-topdesk-public-tutorial/ic790629.png "yeni bir kişiye")
   
    a. Genel sekmesini tıklatın.

    b. İçinde **Soyadı** metin kutusuna, Simon gibi kullanıcının soyadı yazın
 
    c. Seçin bir **Site** hesabı.
 
    d. **Kaydet**’e tıklayın.

> [!NOTE]
> TOPdesk - Azure AD kullanıcı hesaplarını sağlamak için ortak API'ler sağlanan veya herhangi diğer TOPdesk - genel kullanıcı hesabı oluşturma araçlarını kullanabilirsiniz.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Britta TOPdesk - genel erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**TOPdesk için - Britta Simon atamak için ortak, aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **TOPdesk - genel**.

    ![TOPdesk - ortak bağlantı uygulamalar listesinde](./media/active-directory-saas-topdesk-public-tutorial/tutorial_topdesk-public_app.png)  

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantı][202]

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Ekleme atama bölmesi][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

TOPdesk tıklatın - genel kutucuğu erişim panelinde olduğunda, otomatik olarak, TOPdesk - genel uygulama açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](active-directory-saas-tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/active-directory-saas-topdesk-public-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-topdesk-public-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-topdesk-public-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-topdesk-public-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-topdesk-public-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-topdesk-public-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-topdesk-public-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-topdesk-public-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-topdesk-public-tutorial/tutorial_general_203.png

