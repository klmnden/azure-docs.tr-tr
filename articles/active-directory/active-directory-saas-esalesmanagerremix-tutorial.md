---
title: "Öğretici: Azure Active Directory Tümleştirme E satış yöneticisi Remix ile | Microsoft Docs"
description: "Çoklu oturum açma Azure Active Directory arasındaki E satış yöneticisi Remix yapılandırmayı öğrenin."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 89b5022c-0d5b-4103-9877-ddd32b6e1c02
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/21/2018
ms.author: jeedes
ms.openlocfilehash: 56c2860b6aeac9fdc66f2b1fdd1ed9126bf77d3f
ms.sourcegitcommit: d1f35f71e6b1cbeee79b06bfc3a7d0914ac57275
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/22/2018
---
# <a name="tutorial-azure-active-directory-integration-with-e-sales-manager-remix"></a>Öğretici: Azure Active Directory Tümleştirme E satış yöneticisi Remix ile

Bu öğreticide, Azure Active Directory (Azure AD) ile tümleştirme E satış yöneticisi Remix öğrenin.

Azure AD ile tümleştirme E satış yöneticisi Remix ile aşağıdaki avantajları sağlar:

- E satış yöneticisi Remix erişimi, Azure AD'de kontrol edebilirsiniz.
- Azure AD hesaplarına otomatik olarak E satış yöneticisi Remix için (çoklu oturum açma) açan kullanıcılarınıza etkinleştirebilirsiniz.
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirme E satış yöneticisi Remix ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir E satış yöneticisi Remix çoklu oturum açma abonelik etkin

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, şunları yapabilirsiniz [bir aylık deneme sürümünü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden E satış yöneticisi Remix ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-e-sales-manager-remix-from-the-gallery"></a>Galeriden E satış yöneticisi Remix ekleme
Azure AD E satış yöneticisi Remix, tümleştirilmesi yapılandırmak için E satış yöneticisi Remix Galeriden yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

**Galeriden E satış yöneticisi Remix eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **E satış yöneticisi Remix**seçin **E satış yöneticisi Remix** sonuç panelinden ardından **Ekle** uygulama eklemek için düğmeyi.

    ![Sonuçlar listesinde Satış Yöneticisi Remix E](./media/active-directory-saas-esalesmanagerremix-tutorial/tutorial_esalesmanagerremix_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma E "Britta Simon" adlı bir test kullanıcı dayalı satış yöneticisi Remix ile test etme.

Tekli çalışmaya oturum için Azure AD E satış yöneticisi Remix karşılık gelen kullanıcı için bir kullanıcı Azure AD'de nedir bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının E satış yöneticisi Remix ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma E satış yöneticisi Remix ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Bir E satış yöneticisi Remix test kullanıcısı oluşturma](#create-an-e-sales-manager-remix-test-user)**  - Britta Simon, karşılık gelen satış E Yöneticisi kullanıcı Azure AD gösterimini bağlantılı Remix sağlamak için.
4. **[Azure AD test kullanıcısı atayın](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Test çoklu oturum açma](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma E satış yöneticisi Remix uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma E satış yöneticisi Remix ile yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **E satış yöneticisi Remix** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/active-directory-saas-esalesmanagerremix-tutorial/tutorial_esalesmanagerremix_samlbase.png)

3. Üzerinde **E satış yöneticisi Remix etki ve URL'leri** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![E satış yöneticisi Remix etki alanı ve URL'leri tek oturum açma bilgilerini](./media/active-directory-saas-esalesmanagerremix-tutorial/tutorial_esalesmanagerremix_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://<Server-Based-URL>/<sub-domain>/esales-pc`

    b. İçinde **tanımlayıcısı** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://<Server-Based-URL>/<sub-domain>/`

    c. Kopya **tanımlayıcısı** Not Defteri'nde değeri. Daha sonra Bu öğreticide tanımlayıcı değeri kullanır.
    
    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler gerçek oturum açma URL'si ve tanımlayıcı ile güncelleştirin. Kişi [E satış yöneticisi Remix istemci destek ekibi](mailto:esupport@softbrain.co.jp) bu değerleri almak için.

4. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **Certificate(Base64)** ve sertifika dosyayı bilgisayarınıza kaydedin.

    ![Sertifika indirme bağlantısı](./media/active-directory-saas-esalesmanagerremix-tutorial/tutorial_esalesmanagerremix_certificate.png) 

5. Seçin **Görünüm ve diğer tüm kullanıcı özniteliklerini düzenleme** ve tıklayın **emailaddress** özniteliği.
    
    ![E satış yöneticisi Remix yapılandırma](./media/active-directory-saas-esalesmanagerremix-tutorial/configure1.png)

6. Kopya **Namespace** ve **adı** textbox gelen talep değeri. Aşağıdaki desende - değeri üretmek `<Namespace>/<Name>`. Bu değeri daha sonra bu öğreticide kullanacaksınız.

    ![E satış yöneticisi Remix yapılandırma](./media/active-directory-saas-esalesmanagerremix-tutorial/configure2.png)

7. Üzerinde **E satış yöneticisi Remix Yapılandırması** 'yi tıklatın **yapılandırma E satış yöneticisi Remix** açmak için **yapılandırma oturum açma** penceresi. Kopya **Sign-Out URL ve SAML çoklu oturum açma hizmet URL'si** gelen **hızlı başvuru bölümü.**

    ![E satış yöneticisi Remix yapılandırma](./media/active-directory-saas-esalesmanagerremix-tutorial/tutorial_esalesmanagerremix_configure.png) 

8. Tıklatın **kaydetmek** düğmesi.

    ![Oturum açma tek Kaydet düğmesi yapılandırın](./media/active-directory-saas-esalesmanagerremix-tutorial/tutorial_general_400.png)

9. Uygulamanıza E satış yöneticisi Remix yönetici olarak oturum açma.

10. Seçin **yönetici menüsünde** sağ üst menüsünde.

    ![E satış yöneticisi Remix yapılandırma](./media/active-directory-saas-esalesmanagerremix-tutorial/configure4.png)

11. Seçin **sistem ayarlarını**>**Dış Sistem işbirliği**

    ![E satış yöneticisi Remix yapılandırma](./media/active-directory-saas-esalesmanagerremix-tutorial/configure5.png)
    
12. Seçin **SAML**

    ![E satış yöneticisi Remix yapılandırma](./media/active-directory-saas-esalesmanagerremix-tutorial/configure6.png)

13. İçinde **SAML kimlik doğrulama ayarını** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![E satış yöneticisi Remix yapılandırma](./media/active-directory-saas-esalesmanagerremix-tutorial/configure3.png)
    
    a. Seçin **PC sürümü**
    
    b. Seçin **e-posta** açılır listede işbirliği gelen bölüm öğesi.

    c. Birlikte çalışma öğesi metin kutusuna yapıştırın **talep değeri** Azure portal yani kopyaladığınız var `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`

    d. İçinde **veren (varlık kimliği)** metin kutusuna, Yapıştır **tanımlayıcısı** Azure portalından kopyaladığınız değeri **E satış yöneticisi Remix etki ve URL'leri** bölümü.

    e. İndirilen, karşıya yüklemek için **sertifika** Azure portalından seçin **dosya seçimi**.

    f. İçinde **kimlik sağlayıcısı oturum açma URL'si** metin kutusuna, Yapıştır **SAML çoklu oturum açma hizmet URL'si** Azure portalından kopyaladığınız.

    g. İçinde **kimlik sağlayıcısı oturum kapatma URL'si** metin kutusuna, Yapıştır **Sign-Out URL** Azure portalından kopyaladığınız değeri.

    h. Tıklatın **tam ayarlama**

> [!TIP]
> Şimdi bu yönergeleri içinde kısa bir sürümünü okuyabilirsiniz [Azure portal](https://portal.azure.com)uygulaması kuruluyor yaparken!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** sekmesinde ve aracılığıyla katıştırılmış belgelere erişebilir **yapılandırma** alt bölüm. Daha fazla bilgiyi burada embedded belgeler özelliği hakkında: [Azure AD embedded belgeler]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Sol bölmede, Azure portal'ı tıklatın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/active-directory-saas-esalesmanagerremix-tutorial/create_aaduser_01.png)

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantılar](./media/active-directory-saas-esalesmanagerremix-tutorial/create_aaduser_02.png)

3. Açmak için **kullanıcı** iletişim kutusu, tıklatın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/active-directory-saas-esalesmanagerremix-tutorial/create_aaduser_03.png)

4. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/active-directory-saas-esalesmanagerremix-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** kullanıcı Britta Simon e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değer aşağı yazma **parola** kutusu.

    d. **Oluştur**’a tıklayın.
 
### <a name="create-an-e-sales-manager-remix-test-user"></a>Bir E satış yöneticisi Remix test kullanıcısı oluşturma

1. Uygulamanıza E satış yöneticisi Remix yönetici olarak oturum açma.

2. Seçin **yönetici menüsünde** sağ üst menüsünde.

    ![E satış yöneticisi Remix yapılandırma](./media/active-directory-saas-esalesmanagerremix-tutorial/configure4.png)

3. Seçin **şirket ayarlarınızı**>**bakım Departmanlar ve çalışan** seçip **kaydedilmiş çalışanlar**.

    ![Kullanıcı](./media/active-directory-saas-esalesmanagerremix-tutorial/user1.png)

4. İçinde **yeni çalışan kayıt** bölümünde, aşağıdaki adımları gerçekleştirin:
    
    ![Kullanıcı](./media/active-directory-saas-esalesmanagerremix-tutorial/user2.png)

    a. İçinde **çalışan adı** metin kutusuna, Britta gibi kullanıcı adını yazın.

    b. İlgili tüm zorunlu alanlar kullanıcı bilgilerle doldurun.
    
    c. SAML etkinleştirirseniz, yönetici oturum açma ekranından oturum açın, böylece yönetici seçerek kullanıcı oturum açma ayrıcalıkları vermek mümkün olmayacak **yönetici oturum açma**

    d. Tıklatın **kayıt**

5. Gelecekte, yönetici olarak oturum açmak istiyorsanız, oturum yönetici izni verildi kullanıcıyla oturum ve tıklatın **yönetici menüsünde** sağ üst menüsünde.

    ![Kullanıcı](./media/active-directory-saas-esalesmanagerremix-tutorial/configure4.png)

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Britta E satış yöneticisi Remix için erişim izni verme, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**E satış yöneticisi Remix için Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **E satış yöneticisi Remix**.

    ![Uygulamalar listesinde E satış yöneticisi Remix bağlantı](./media/active-directory-saas-esalesmanagerremix-tutorial/tutorial_esalesmanagerremix_app.png)  

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantı][202]

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Ekleme atama bölmesi][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açmayı test edin

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli E satış yöneticisi Remix parçasında tıklattığınızda, otomatik olarak E satış yöneticisi Remix uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](active-directory-saas-tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-esalesmanagerremix-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-esalesmanagerremix-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-esalesmanagerremix-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-esalesmanagerremix-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-esalesmanagerremix-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-esalesmanagerremix-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-esalesmanagerremix-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-esalesmanagerremix-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-esalesmanagerremix-tutorial/tutorial_general_203.png

