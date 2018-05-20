---
title: 'Öğretici: Azure Active Directory Tümleştirme ile görüntü geçiş | Microsoft Docs'
description: Çoklu oturum açma Azure Active Directory ve görüntü geçişi arasındaki yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 65bb5990-07ef-4244-9f41-cd28fc2cb5a2
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/22/2017
ms.author: jeedes
ms.openlocfilehash: b4ed73ca669ede9c206abd653a0b991edc2b1063
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2018
---
# <a name="tutorial-azure-active-directory-integration-with-image-relay"></a>Öğretici: Azure Active Directory Tümleştirme ile görüntü geçişi

Bu öğreticide, görüntü geçiş Azure Active Directory (Azure AD) ile tümleştirme öğrenin.

Görüntü geçiş Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Görüntü geçiş erişimi, Azure AD'de kontrol edebilirsiniz
- Azure AD hesaplarına otomatik olarak görüntü geçişi (çoklu oturum açma) açan kullanıcılarınıza etkinleştirebilirsiniz
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile görüntü geçişi yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir görüntü geçiş çoklu oturum açma abonelik etkin

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden görüntü geçiş ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-image-relay-from-the-gallery"></a>Galeriden görüntü geçiş ekleme
Azure AD görüntü geçiş tümleştirilmesi yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden görüntü geçiş eklemeniz gerekir.

**Galeriden görüntü geçiş eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Uygulamalar][3]

4. Arama kutusuna **görüntü geçiş**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_search.png)

5. Sonuçlar panelinde seçin **görüntü geçiş**ve ardından **Ekle** uygulama eklemek için düğmesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Çoklu oturum açmayı yapılandırma ve Azure AD sınama
Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma görüntü "Britta Simon" adlı bir test kullanıcı tabanlı geçiş ile test etme.

Tekli çalışmaya oturum için Azure AD görüntü geçiş karşılık gelen kullanıcı için bir kullanıcı Azure AD'de nedir bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının görüntü geçiş ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Görüntü geçiş değerini atayın **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** bağlantı ilişkisi oluşturmak için.

Yapılandırma ve Azure AD çoklu oturum açma görüntü geçiş ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Bir görüntü geçiş test kullanıcısı oluşturma](#creating-an-image-relay-test-user)**  - Britta Simon, karşılık gelen kullanıcı Azure AD gösterimini bağlantılı görüntü geçişi sağlamak için.
4. **[Azure AD test kullanıcısı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açmayı test](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma görüntü geçiş uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma ile görüntü geçişi yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **görüntü geçiş** uygulama tümleştirmesi sayfasında, tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_samlbase.png)

3. Üzerinde **görüntü geçiş etki alanı ve URL'leri** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://<companyname>.imagerelay.com/`

    b. İçinde **tanımlayıcısı** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://<companyname>.imagerelay.com/sso/metadata`

    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler gerçek oturum açma URL'si ve tanımlayıcı ile güncelleştirin. Kişi [görüntü geçiş istemci destek ekibi](http://support.imagerelay.com/) bu değerleri almak için. 
 


4. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **Certificate(Base64)** ve sertifika dosyayı bilgisayarınıza kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_certificate.png) 

5. Tıklatın **kaydetmek** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-imagerelay-tutorial/tutorial_general_400.png)

6. Üzerinde **görüntü geçiş yapılandırma** 'yi tıklatın **yapılandırma görüntü geçiş** açmak için **yapılandırma oturum açma** penceresi. Kopya **Sign-Out hizmet URL'sini ve SAML çoklu oturum açma hizmet URL'si** gelen **hızlı başvuru bölümü.**

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_configure.png) 

7. Başka bir tarayıcı penceresinde görüntü geçiş şirket sitenize yönetici olarak oturum açın.

8. Üstteki araç çubuğunda tıklatın **kullanıcılar ve izinler** iş yükü.
   
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_06.png) 

9. Tıklatın **oluşturma yeni izni**.
   
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_08.png)

10. İçinde **tek oturum açma ayarları** iş yükü, select **bu grup için yalnızca oturum açma aracılığıyla çoklu oturum açma** onay kutusunu işaretleyin ve ardından **kaydetmek**.
   
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_09.png) 

11. Git **hesap ayarları**.
   
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_10.png) 

12. Git **tek oturum açma ayarları** iş yükü.
    
     ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_11.png)

13. Üzerinde **SAML ayarları** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:
    
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_12.png)
    
    a. İçinde **oturum açma URL'si** metin değerini yapıştırın **çoklu oturum açma hizmet URL'si** Azure portalından kopyalanan.

    b. İçinde **oturum kapatma URL'si** metin değerini yapıştırın **tek Sign-Out hizmeti URL'si** Azure portalından kopyalanan.

    c. Olarak **ad kimliği biçimi**seçin **urn: OASIS: adları: tc: SAML:1.1:nameid-biçimi: emailAddress**.

    d. Olarak **bağlama seçenekleri hizmet sağlayıcısı (görüntü geçiş) gelen istekleri için**seçin **POST bağlama**.

    e. Altında **x.509 sertifikası**, tıklatın **güncelleştirme sertifika**.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_17.png)

    f. İndirilen sertifika Not Defteri'nde açın, içeriği Kopyala ve x.509 sertifikası metin kutusuna yapıştırın.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_18.png)

    g. İçinde **Just-In-Time kullanıcı sağlamayı** bölümünde, select **Just-In-Time kullanıcı sağlamayı etkinleştirin**.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_19.png)

    h. İzin grubu seçin (örneğin, **SSO temel**) yalnızca çoklu oturum açma oturum açmaya izin.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_20.png)

    i. **Kaydet**’e tıklayın.

> [!TIP]
> Şimdi bu yönergeleri içinde kısa bir sürümünü okuyabilirsiniz [Azure portal](https://portal.azure.com)uygulaması kuruluyor yaparken!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** sekmesinde ve aracılığıyla katıştırılmış belgelere erişebilir **yapılandırma** alt bölüm. Daha fazla bilgiyi burada embedded belgeler özelliği hakkında: [Azure AD embedded belgeler]( https://go.microsoft.com/fwlink/?linkid=845985)


### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

![Azure AD Kullanıcı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portal**, sol gezinti bölmesinde tıklatın **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-imagerelay-tutorial/create_aaduser_01.png) 

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-imagerelay-tutorial/create_aaduser_02.png) 

3. Açmak için **kullanıcı** iletişim kutusunda, tıklatın **Ekle** iletişim kutusunun üst kısmında.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-imagerelay-tutorial/create_aaduser_03.png) 

4. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-imagerelay-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna, türü **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna, türü **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.
 
### <a name="creating-an-image-relay-test-user"></a>Bir görüntü geçiş test kullanıcısı oluşturma

Bu bölümün amacı Britta Simon de görüntü geçişi adlı bir kullanıcı oluşturmaktır.

**Görüntü geçiş Britta Simon adlı bir kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Görüntü geçiş şirket sitenize yönetici olarak oturum.

2. Git **kullanıcılar ve izinler** seçip **SSO kullanıcı oluştur**.
   
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_21.png) 

3. Girin **e-posta**, **ad**, **Soyadı**, ve **şirket** sağlamak ve izin grubu (seçmek istediğiniz kullanıcının Örneğin, SSO temel) yalnızca çoklu oturum açma oturum açarak Grup olduğu.
   
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_22.png) 

4. **Oluştur**’a tıklayın.

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atama

Bu bölümde, Britta görüntü geçişi erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı atama][200] 

**Görüntü geçişi Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **görüntü geçiş**.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_app.png) 

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    ![Kullanıcı atama][202] 

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Kullanıcı atama][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açmayı test etme

Bu bölümün amacı erişim paneli kullanılarak Azure AD çoklu oturum açma yapılandırmanızı test etmektir.    

Erişim paneli görüntü geçiş parçasında tıklattığınızda, otomatik olarak görüntü geçiş uygulamanıza açan.


## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](active-directory-saas-tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/active-directory-saas-imagerelay-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-imagerelay-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-imagerelay-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-imagerelay-tutorial/tutorial_general_04.png


[100]: ./media/active-directory-saas-imagerelay-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-imagerelay-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-imagerelay-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-imagerelay-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-imagerelay-tutorial/tutorial_general_203.png

