---
title: 'Öğretici: SAP iş nesnesi bulut ile Azure Active Directory Tümleştirme | Microsoft Docs'
description: Azure Active Directory ve SAP iş nesnesi bulut arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: 6c5e44f0-4e52-463f-b879-834d80a55cdf
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/14/2017
ms.author: jeedes
ms.openlocfilehash: ffd4480a13549caba17becff27a43f51fcaa1988
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/14/2018
ms.locfileid: "39041747"
---
# <a name="tutorial-azure-active-directory-integration-with-sap-business-object-cloud"></a>Öğretici: SAP iş nesnesi bulut ile Azure Active Directory Tümleştirme

Bu öğreticide, SAP iş nesnesi bulut Azure Active Directory (Azure AD) ile tümleştirmeyi öğrenin.

SAP Business nesne bulut Azure AD ile tümleştirdiğinizde aşağıdaki avantajlardan yararlanabilirsiniz:

- Azure AD'de, SAP iş nesnesi buluta kimlerin erişebileceğini kontrol edebilirsiniz.
- Otomatik olarak SAP iş nesnesi bulut kullanıcılarınıza, çoklu oturum açma ve bir kullanıcının Azure AD hesabı kullanarak oturum açabilirsiniz.
- Hesaplarınızı tek, merkezi bir konumda, Azure portalında yönetebilir.

Azure AD ile bir hizmet (SaaS) uygulamasını tümleştirme olarak yazılım hakkında daha fazla bilgi edinmek için [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

SAP Business nesne bulut ile Azure AD tümleştirmeyi ayarlamak için aşağıdaki öğeler gerekir:

- Azure AD aboneliğiniz
- SAP iş nesnesi ile bir bulutta, çoklu açık oturum

> [!NOTE]
> Bu öğreticideki adımları test varsa, bunları bir üretim ortamında da test etmezseniz, öneririz.

Bu öğreticideki adımları test etmek için öneriler:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, şunları yapabilirsiniz [bir aylık ücretsiz deneme sürümü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. 

Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. SAP Business nesne bulut Galeriden ekleyin.
2. Ayarlama ve Azure AD çoklu oturum açmayı test edin.

## <a name="add-sap-business-object-cloud-from-the-gallery"></a>SAP Business nesne bulut Galeriden Ekle
Galeride SAP iş nesnesi bulut Azure AD ile tümleştirmeyi ayarlamak için SAP iş nesnesi bulut yönetilen SaaS uygulamaları listenize ekleyin.

SAP Business nesne bulut Galeriden eklemek için:

1. İçinde [Azure portalında](https://portal.azure.com), soldaki menüde **Azure Active Directory**. 

    ![Azure Active Directory düğmesi][1]

2. Seçin **kurumsal uygulamalar**ve ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar sayfası][2]
    
3. Yeni bir uygulama eklemek için seçin **yeni uygulama**.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **SAP iş nesnesi bulut**.

    ![Arama kutusu](./media/sapboc-tutorial/tutorial_sapboc_search.png)

5. Sonuçlar panelinde seçin **SAP iş nesnesi bulut**ve ardından **Ekle**.

    ![Sonuç listesinde SAP iş nesnesi bulut](./media/sapboc-tutorial/tutorial_sapboc_addfromgallery.png)

##  <a name="set-up-and-test-azure-ad-single-sign-on"></a>Ayarlama ve Azure AD çoklu oturum açma testi

Bu bölümde, ayarladığınız ve SAP iş nesnesi bulut ile Azure AD çoklu oturum açmayı test adlı bir test kullanıcı tabanlı *Britta Simon*.

Tek iş için oturum açma için Azure AD SAP iş nesnesi bulut Azure AD karşılık gelen kullanıcı bilmesi gerekir. Bir Azure AD kullanıcısı ve SAP iş nesnesi bulutta ilgili kullanıcı arasında bir bağlantı ilişki kurulması gerekir.

İçin SAP iş nesnesi bulutta bağlantı ilişki kurmak için **kullanıcıadı**, değerini atayın **kullanıcı adı** Azure AD'de.

Yapılandırma ve Azure AD çoklu oturum açma SAP iş nesnesi Cloud ile test etmek için aşağıdaki görevleri tamamlayın:

1. [Azure AD çoklu oturum açmayı ayarlamak](#set-up-azure-ad-single-sign-on). Bir kullanıcı bu özelliği kullanmak için ayarlar.
2. [Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user). Testleri Azure AD çoklu oturum açma kullanıcı Britta Simon.
3. [Bir SAP iş nesnesi bulut test kullanıcısı oluşturma](#create-an-sap-business-object-cloud-test-user). Buluttaki SAP iş nesne kullanıcı Azure AD gösterimini bağlı bir karşılığı Britta simon'un oluşturur.
4. [Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user). Çoklu oturum açmayı Britta Simon, Azure AD'yi kullanacak şekilde ayarlar.
5. [Çoklu oturum açmayı test](#test-single-sign-on). Yapılandırma çalıştığını doğrular.

### <a name="set-up-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı ayarlayın

Bu bölümde, Azure AD'ye tek Azure portalında oturum açın. Ardından, çoklu oturum açmayı SAP iş nesnesi bulut uygulamanıza ayarlayın.

Azure AD çoklu oturum açma SAP iş nesnesi bulutla ayarlamak için:

1. Azure portalında, üzerinde **SAP iş nesnesi bulut** uygulama tümleştirme sayfasında **çoklu oturum açma**.

    ![Çoklu oturum açma seçin][4]

2. Üzerinde **çoklu oturum açma** sayfası için **modu**seçin **SAML tabanlı oturum açma**.
 
    ![SAML tabanlı oturum açma'yı seçin](./media/sapboc-tutorial/tutorial_sapboc_samlbase.png)

3. Altında **SAP iş nesnesi bulut etki alanı ve URL'ler**, aşağıdaki adımları tamamlayın:

    1. İçinde **oturum açma URL'si** kutusunda, aşağıdaki desenin bir URL girin: 
    | |
    |-|-|
    | `https://<sub-domain>.sapanalytics.cloud/` |
    | `https://<sub-domain>.sapbusinessobjects.cloud/` |

    2. İçinde **tanımlayıcı** kutusunda, aşağıdaki desenin bir URL girin:
    | |
    |-|-|
    | `<sub-domain>.sapbusinessobjects.cloud` |
    | `<sub-domain>.sapanalytics.cloud` |

    ![SAP Business nesne bulut etki alanı ve URL'ler sayfa URL'leri](./media/sapboc-tutorial/tutorial_sapboc_url.png)
 
    > [!NOTE] 
    > Bu URL'ler gösterimi için değerler. Tanımlayıcı URL'sini ve gerçek oturum açma URL'si ile güncelleştirin. Oturum açma URL'si almak için iletişime geçin [SAP iş nesnesi bulut istemci Destek ekibine](https://help.sap.com/viewer/product/SAP_BusinessObjects_Cloud/release/en-US). SAP Business nesne bulut meta verilerini yönetici konsolundan indirerek tanımlayıcı URL'sini alabilirsiniz. Bu öğreticinin ilerleyen bölümlerinde açıklanmıştır. 

4. Altında **SAML imzalama sertifikası**seçin **meta veri XML**. Ardından, bilgisayarınızda meta veri dosyasını kaydedin.

    ![Meta veri XML seçin](./media/sapboc-tutorial/tutorial_sapboc_certificate.png) 

5. **Kaydet**’i seçin.

    ![Kaydet'i seçin](./media/sapboc-tutorial/tutorial_general_400.png)

6. Farklı bir web tarayıcı penceresinde bir SAP iş nesnesi bulut şirketinizin sitesi için bir yönetici olarak oturum açın.

7. Seçin **menü** > **sistem** > **Yönetim**.
    
    ![Menü, sonra sistem ve yönetim seçin](./media/sapboc-tutorial/config1.png)

8. Üzerinde **güvenlik** sekmesinde **Düzenle** (Kalem) simgesi.
    
    ![Güvenlik sekmesinde düzenleme simgesini seçin.](./media/sapboc-tutorial/config2.png)  

9. İçin **kimlik doğrulama yöntemi**seçin **SAML çoklu oturum açma (SSO)**.

    ![SAML çoklu oturum açma için kimlik doğrulama yöntemini seçin.](./media/sapboc-tutorial/config3.png)  

10. Hizmet sağlayıcısı meta verileri (adım 1) indirmek için seçin **indirme**. Meta veri dosyasında bulup kopyalayabilirsiniz **Entityıd** değeri. Azure portalında altında **SAP iş nesnesi bulut etki alanı ve URL'ler**, değeri olarak yapıştırın **tanımlayıcı** kutusu.

    ![Kopyalama ve yapıştırma Entityıd değeri](./media/sapboc-tutorial/config4.png)  

11. Hizmet sağlayıcısı meta verileri (Adım 2) altında Azure portalından indirilen dosyayı karşıya yüklemek için **karşıya kimlik sağlayıcısı meta verileri**seçin **karşıya**.  

    ![Karşıya yükleme kimlik sağlayıcısı meta verileri altında seçin](./media/sapboc-tutorial/config5.png)

12. İçinde **kullanıcı özniteliği** listesinde, uygulamanız için kullanmak istediğiniz kullanıcı özniteliği (adım 3) seçin. Bu kullanıcı özniteliğini kimlik sağlayıcısına eşler. Özel bir öznitelik kullanıcının sayfasında girmek için kullanın. **özel SAML eşleme** seçeneği. Ya da her ikisini seçebilirsiniz **e-posta** veya **kullanıcı kimliği** kullanıcı özniteliği. Bizim örneğimizde, biz seçili **e-posta** biz kullanıcı tanımlayıcısı talebi ile eşlenmiş çünkü **userprincipalname** özniteliğini **kullanıcı öznitelikleri** Azure bölümünde Portalı. Bu, her başarılı SAML yanıtını SAP iş nesnesi bulut uygulamasına gönderdiği bir benzersiz kullanıcı e-posta, sağlar.

    ![Kullanıcı özniteliğini seçin](./media/sapboc-tutorial/config6.png)

13. Kimlik sağlayıcısı (adım 4) hesabı doğrulamak için **oturum açma kimlik bilgileri (e-posta)** kutusuna, kullanıcının e-posta adresi girin. Ardından, **hesabı doğrula**. Sistem kullanıcı hesabı ile oturum açma kimlik bilgilerini ekler.

    ![E-posta girin ve doğrulayın hesabı seçin](./media/sapboc-tutorial/config7.png)

14. Seçin **Kaydet** simgesi.

    ![Kaydet simgesine](./media/sapboc-tutorial/save.png)

> [!TIP]
> Bu yönergelerde kısa bir sürümünü edinebilirsiniz [Azure portalında](https://portal.azure.com), uygulamanızı ayarladığınız sırada! Seçerek uygulama ekledikten sonra **Active Directory** > **kurumsal uygulamalar**seçin **çoklu oturum açma** sekmesi. Katıştırılmış belgelerinde erişebileceğiniz **yapılandırma** sayfanın alt kısmındaki bölümde. Daha fazla bilgi için [Azure AD belgeleri katıştırılmış]( https://go.microsoft.com/fwlink/?linkid=845985).

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümde, Azure portalında Britta Simon adlı bir test kullanıcısı oluşturun.

Azure AD'de bir test kullanıcısı oluşturmak için:

1. Azure portalında soldaki menüde seçin **Azure Active Directory**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/sapboc-tutorial/create_aaduser_01.png) 

2. Kullanıcıların listesini görüntülemek için seçin **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/sapboc-tutorial/create_aaduser_02.png) 

3. Açmak için **kullanıcı** iletişim kutusunda **Ekle**.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/sapboc-tutorial/create_aaduser_03.png) 

4. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları tamamlayın:
 
    1. İçinde **adı** kutusuna **BrittaSimon**.

    2. İçinde **kullanıcı adı** kutusuna, Britta Simon kullanıcının e-posta adresi girin.

    3. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değeri yazın **parola** kutusu.

    4. **Oluştur**’u seçin.

        ![Kullanıcı iletişim kutusu](./media/sapboc-tutorial/create_aaduser_04.png) 

    ![Azure AD kullanıcısı oluşturun][100]

### <a name="create-an-sap-business-object-cloud-test-user"></a>Bir SAP iş nesnesi bulut test kullanıcısı oluşturma

SAP Business nesne buluta oturum önce azure AD kullanıcıları SAP iş nesnesi bulutta sağlanması gerekir. SAP iş nesnesi bulutta sağlama bir el ile gerçekleştirilen bir görevdir.

Bir kullanıcı hesabı sağlamak için:

1. SAP Business nesne bulut şirketinizin sitesi için bir yönetici olarak oturum açın.

2. Seçin **menü** > **güvenlik** > **kullanıcılar**.

    ![Çalışan Ekle](./media/sapboc-tutorial/user1.png)

3. Üzerinde **kullanıcılar** seçin sayfasında, yeni kullanıcı ayrıntıları eklemek için **+**. 

    ![Kullanıcılar sayfasına ekleme](./media/sapboc-tutorial/user4.png)

    Ardından, aşağıdaki adımları tamamlayın:

    1. İçinde **kullanıcı kimliği** kutusuna, kullanıcının kullanıcı kimliği gibi girin **Britta**.

    2. İçinde **ad** kutusunda, kullanıcı adı gibi girin **Britta**.

    3. İçinde **SOYADI** kutusunda, son kullanıcı adı gibi girin **Simon**.

    4. İçinde **GÖRÜNEN ad** kutusuna, kullanıcının tam adı gibi girin **Britta Simon**.

    5. İçinde **e-posta** kutusuna, kullanıcının e-posta adresi gibi girin **brittasimon@contoso.com**.

    6. Üzerinde **Rollerini Seç** sayfasında kullanıcı için uygun rolü seçin ve ardından **Tamam**.

      ![Rol seç](./media/sapboc-tutorial/user3.png)

    7. Seçin **Kaydet** simgesi.    


### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, kullanıcının Britta Simon, SAP iş nesnesi buluta kullanıcı hesabı erişimi verilmesi tarafından Azure AD çoklu oturum açmayı kullanmak için izin verin.

Britta Simon SAP iş nesnesi buluta atamak için:

1. Azure portalında uygulama görünümü açın ve ardından dizin görünümüne gidin. Seçin **kurumsal uygulamalar**ve ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

2. Uygulamalar listesinde **SAP iş nesnesi bulut**.

    ![Çoklu oturum açmayı yapılandırın](./media/sapboc-tutorial/tutorial_sapboc_app.png) 

3. Sol menüde **kullanıcılar ve gruplar**.

    ![Kullanıcıları ve grupları seçin][202] 

4. **Add (Ekle)** seçeneğini belirleyin. Ardından **atama Ekle** sayfasında **kullanıcılar ve gruplar**.

    ![Atama Ekle sayfası][203]

5. Üzerinde **kullanıcılar ve gruplar** sayfasında, kullanıcıların, select listesinde **Britta Simon**.

6. Üzerinde **kullanıcılar ve gruplar** sayfasında **seçin**.

7. Üzerinde **atama Ekle** sayfasında **atama**.

![Kullanıcı rolü atayın][200] 
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test.

Erişim Paneli'nde SAP iş nesnesi bulut kutucuğu seçtiğinizde, otomatik olarak SAP iş nesnesi bulut uygulamanız için oturum açmanız.

Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir?](../manage-apps/what-is-single-sign-on.md)


<!--Image references-->

[1]: ./media/sapboc-tutorial/tutorial_general_01.png
[2]: ./media/sapboc-tutorial/tutorial_general_02.png
[3]: ./media/sapboc-tutorial/tutorial_general_03.png
[4]: ./media/sapboc-tutorial/tutorial_general_04.png

[100]: ./media/sapboc-tutorial/tutorial_general_100.png

[200]: ./media/sapboc-tutorial/tutorial_general_200.png
[201]: ./media/sapboc-tutorial/tutorial_general_201.png
[202]: ./media/sapboc-tutorial/tutorial_general_202.png
[203]: ./media/sapboc-tutorial/tutorial_general_203.png

