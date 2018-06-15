---
title: 'Öğretici: SAP Business nesnesi bulut Azure Active Directory Tümleştirme | Microsoft Docs'
description: Çoklu oturum açma Azure Active Directory ve SAP Business nesnesi bulut arasında yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: 6c5e44f0-4e52-463f-b879-834d80a55cdf
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/14/2017
ms.author: jeedes
ms.openlocfilehash: 5a56a892ac3b28c4e90ec2ea6360da3d2eff2581
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2018
ms.locfileid: "34349497"
---
# <a name="tutorial-azure-active-directory-integration-with-sap-business-object-cloud"></a>Öğretici: SAP Business nesnesi bulut Azure Active Directory Tümleştirme

Bu öğreticide, SAP Business nesnesi bulut Azure Active Directory (Azure AD) ile tümleştirme öğrenin.

SAP Business nesnesi bulut Azure AD ile tümleştirdiğinizde aşağıdaki yararları alın:

- Azure AD'de SAP Business nesnesi buluta kimlerin erişimi olduğunu kontrol edebilirsiniz.
- Otomatik olarak SAP Business nesnesi bulut kullanıcılarınıza, çoklu oturum açma ve bir kullanıcının Azure AD hesabı kullanarak oturum.
- Hesaplarınızı tek, merkezi bir konumda, Azure portalında yönetebilir.

Azure AD ile hizmet (SaaS) uygulaması tümleştirme olarak yazılım hakkında daha fazla bilgi edinmek için [uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

SAP Business nesnesi bulut ile Azure AD tümleştirmeyi ayarlamak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- SAP Business nesnesi olan bir buluta, tekli açık oturum

> [!NOTE]
> Bu öğreticide adımları test ederseniz, bunları bir üretim ortamında sınamanızı yok öneririz.

Bu öğreticide adımları test etmek için öneriler:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, şunları yapabilirsiniz [bir aylık ücretsiz deneme sürümü alma](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. 

Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. SAP Business nesnesi bulut Galeriden ekleyin.
2. Ayarlama ve Azure AD çoklu oturum açmayı test edin.

## <a name="add-sap-business-object-cloud-from-the-gallery"></a>SAP Business nesnesi bulut Galerisi'nden ekleme
Galeride SAP Business nesnesi bulut Azure AD ile tümleştirmeyi ayarlamak için SAP Business nesnesi bulut yönetilen SaaS uygulamaları listenize ekleyin.

SAP Business nesnesi bulut Galeriden eklemek için:

1. İçinde [Azure portal](https://portal.azure.com), soldaki menüde seçin **Azure Active Directory**. 

    ![Azure Active Directory düğmesi][1]

2. Seçin **kurumsal uygulamalar**ve ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar sayfası][2]
    
3. Yeni bir uygulama eklemek için seçin **yeni uygulama**.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **SAP Business nesnesi bulut**.

    ![Arama kutusu](./media/active-directory-saas-sapboc-tutorial/tutorial_sapboc_search.png)

5. Sonuçlar panelinde seçin **SAP Business nesnesi bulut**ve ardından **Ekle**.

    ![SAP Business nesnesi bulut sonuçlar listesinde](./media/active-directory-saas-sapboc-tutorial/tutorial_sapboc_addfromgallery.png)

##  <a name="set-up-and-test-azure-ad-single-sign-on"></a>Ayarlama ve Azure AD çoklu oturum açmayı test edin

Bu bölümde, ayarladığınız ve SAP Business nesnesi bulut ile Azure AD çoklu oturum açmayı test adlı bir test kullanıcı tabanlı *Britta Simon*.

Tekli çalışmaya oturum için Azure AD SAP Business nesnesi bulutta Azure AD karşılık gelen kullanıcı bilmek ister. Bir Azure AD kullanıcısının ve SAP Business nesnesi bulutta ilgili kullanıcı arasındaki bağlantıyı ilişkisi kurulmalıdır.

İçin SAP Business nesnesi bulutta bağlantı ilişkisi oluşturmak için **kullanıcıadı**, değeri atayın **kullanıcı adı** Azure AD'de.

Yapılandırma ve Azure AD çoklu oturum açma SAP Business nesnesi bulut ile test etmek için aşağıdaki görevleri tamamlayın:

1. [Azure AD çoklu oturum açmayı ayarlama](#set-up-azure-ad-single-sign-on). Bir kullanıcı bu özelliği kullanmak için ayarlar.
2. [Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user). Testleri Azure AD çoklu oturum açma Britta Simon kullanıcıyla.
3. [SAP Business nesnesi bulut test kullanıcısı oluşturma](#create-an-sap-business-object-cloud-test-user). Karşılık gelen Britta Simon, kullanıcının Azure AD gösterimini bağlı SAP Business nesnesi bulut oluşturur.
4. [Azure AD test kullanıcısı atayın](#assign-the-azure-ad-test-user). Çoklu oturum açmayı Britta Azure AD kullanılacak Simon ayarlar.
5. [Test çoklu oturum açma](#test-single-sign-on). Yapılandırma çalıştığını doğrular.

### <a name="set-up-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı ayarlayın

Bu bölümde, üzerinde Azure AD çoklu Azure portalında oturum açın. Sonra tek oturum açma SAP Business nesnesi bulut uygulamanızda ayarlayın.

Azure AD çoklu oturum açma SAP Business nesnesi bulut ile ayarlamak için:

1. Azure portalında üzerinde **SAP Business nesnesi bulut** uygulama tümleştirmesi sayfasında, **çoklu oturum açma**.

    ![Çoklu oturum açma seçin][4]

2. Üzerinde **çoklu oturum açma** sayfası için **modu**seçin **SAML tabanlı oturum açma**.
 
    ![SAML tabanlı oturum açma seçin](./media/active-directory-saas-sapboc-tutorial/tutorial_sapboc_samlbase.png)

3. Altında **SAP Business nesnesi bulut etki alanı ve URL'leri**, aşağıdaki adımları tamamlayın:

    1. İçinde **oturum açma URL'si** kutusunda, aşağıdaki desen bir URL girin: 
    | |
    |-|-|
    | `https://<sub-domain>.sapanalytics.cloud/` |
    | `https://<sub-domain>.sapbusinessobjects.cloud/` |

    2. İçinde **tanımlayıcısı** kutusunda, aşağıdaki desen bir URL girin:
    | |
    |-|-|
    | `<sub-domain>.sapbusinessobjects.cloud` |
    | `<sub-domain>.sapanalytics.cloud` |

    ![SAP Business nesnesi bulut etki alanı ve URL'leri sayfa URL'leri](./media/active-directory-saas-sapboc-tutorial/tutorial_sapboc_url.png)
 
    > [!NOTE] 
    > Bu URL'leri yalnızca tanıtım değerler. Değerlerini tanımlayıcı URL'sini ve gerçek oturum açma URL'si ile güncelleştirin. Oturum açma URL'si almak için başvurun [SAP Business nesnesi bulut istemci destek ekibi](https://www.sap.com/product/analytics/cloud-analytics.support.html). Yönetici konsolundan SAP Business nesnesi bulut meta verileri yükleyerek tanımlayıcı URL'sini alabilirsiniz. Bu öğreticinin ilerleyen bölümlerinde açıklanmıştır. 

4. Altında **SAML imzalama sertifikası**seçin **meta veri XML**. Ardından, meta veri dosyası, bilgisayarınıza kaydedin.

    ![Meta veri XML seçin](./media/active-directory-saas-sapboc-tutorial/tutorial_sapboc_certificate.png) 

5. **Kaydet**’i seçin.

    ![Kaydet'i seçin](./media/active-directory-saas-sapboc-tutorial/tutorial_general_400.png)

6. Farklı web tarayıcısı penceresinde SAP Business nesnesi bulut şirket sitenize yönetici olarak oturum açın.

7. Seçin **menü** > **sistem** > **Yönetim**.
    
    ![Menüsünü, sonra sistem ve yönetim seçin](./media/active-directory-saas-sapboc-tutorial/config1.png)

8. Üzerinde **güvenlik** sekmesine **Düzenle** (Kalem) simgesi.
    
    ![Güvenlik sekmesinde Düzenle simgesine seçin](./media/active-directory-saas-sapboc-tutorial/config2.png)  

9. İçin **kimlik doğrulama yöntemini**seçin **SAML çoklu oturum açma (SSO)**.

    ![SAML çoklu oturum açma kimlik doğrulama yöntemini seçin](./media/active-directory-saas-sapboc-tutorial/config3.png)  

10. Hizmet sağlayıcısı meta verileri (1. adım) yüklemek üzere seçin **karşıdan**. Meta veri dosyasını bulun ve kopyalama **Entityıd** değeri. Azure portalında altında **SAP Business nesnesi bulut etki alanı ve URL'leri**, değeri yapıştırın **tanımlayıcısı** kutusu.

    ![Kopyalama ve yapıştırma Entityıd değeri](./media/active-directory-saas-sapboc-tutorial/config4.png)  

11. Hizmet sağlayıcısı meta verileri (2. adım) altında Azure portalından indirdiğiniz dosyayı karşıya yükleme için **karşıya kimlik sağlayıcısı meta verileri**seçin **karşıya**.  

    ![Karşıya yükleme karşıya kimlik sağlayıcısı meta verileri altında seçin](./media/active-directory-saas-sapboc-tutorial/config5.png)

12. İçinde **kullanıcı özniteliği** listesinde, uygulamanız için kullanmak istediğiniz kullanıcı özniteliği (adım 3) seçin. Bu kullanıcı özniteliği kimlik sağlayıcısı eşler. Özel bir öznitelik kullanıcının sayfasında girmek için **özel SAML eşleme** seçeneği. Ya da her ikisini seçebilirsiniz **e-posta** veya **kullanıcı kimliği** kullanıcı özniteliği olarak. Bizim örneğimizde, biz seçili **e-posta** biz kullanıcı tanımlayıcısı talebi eşlenen çünkü **userprincipalname** özniteliğini **kullanıcı öznitelikleri** Azure portalı bölümünde. Bu, her başarılı SAML yanıt SAP Business nesnesi bulut uygulamaya gönderilen bir benzersiz kullanıcı e-posta, sağlar.

    ![Kullanıcı özniteliğini seçin](./media/active-directory-saas-sapboc-tutorial/config6.png)

13. Kimlik sağlayıcısı (4. adım), hesabı doğrulamak için **oturum açma kimlik bilgileri (e-posta)** kutusuna, kullanıcının e-posta adresi girin. Ardından, seçin **hesabı doğrula**. Sistem kullanıcı hesabı ile oturum açma kimlik bilgilerini ekler.

    ![E-posta girin ve Doğrula hesabı seçin](./media/active-directory-saas-sapboc-tutorial/config7.png)

14. Seçin **kaydetmek** simgesi.

    ![Kaydet simgesine](./media/active-directory-saas-sapboc-tutorial/save.png)

> [!TIP]
> Bu yönergelerde kısa bir sürümünü okuyabilirsiniz [Azure portal](https://portal.azure.com)uygulamanızı ayarlama yaparken! Seçerek uygulama ekledikten sonra **Active Directory** > **kurumsal uygulamalar**seçin **çoklu oturum açma** sekmesi. Katıştırılmış belgelerde erişebilirsiniz **yapılandırma** bölümünde, sayfanın sonundaki. Daha fazla bilgi için bkz: [Azure AD embedded belgeler]( https://go.microsoft.com/fwlink/?linkid=845985).

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümde, Azure portalında Britta Simon adlı bir test kullanıcısı oluşturun.

Azure AD'de bir test kullanıcı oluşturmak için:

1. Azure portalında soldaki menüde seçin **Azure Active Directory**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-sapboc-tutorial/create_aaduser_01.png) 

2. Kullanıcıların listesini görüntülemek için seçin **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-sapboc-tutorial/create_aaduser_02.png) 

3. Açmak için **kullanıcı** iletişim kutusunda **Ekle**.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-sapboc-tutorial/create_aaduser_03.png) 

4. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları tamamlayın:
 
    1. İçinde **adı** kutusuna **BrittaSimon**.

    2. İçinde **kullanıcı adı** kutusuna, Britta Simon kullanıcının e-posta adresi girin.

    3. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değer aşağı yazma **parola** kutusu.

    4. **Oluştur**’u seçin.

        ![Kullanıcı iletişim kutusu](./media/active-directory-saas-sapboc-tutorial/create_aaduser_04.png) 

    ![Azure AD Kullanıcı oluşturma][100]

### <a name="create-an-sap-business-object-cloud-test-user"></a>SAP Business nesnesi bulut test kullanıcısı oluşturma

Bunlar SAP Business nesnesi buluta oturum açabilmeniz için önce azure AD kullanıcıları SAP Business nesnesi bulutta sağlanmalıdır. SAP Business nesnesi bulutta sağlama bir el ile bir görevdir.

Bir kullanıcı hesabı sağlamak için:

1. SAP Business nesnesi bulut şirket sitenize yönetici olarak oturum açın.

2. Seçin **menü** > **güvenlik** > **kullanıcılar**.

    ![Çalışanı ekleyin](./media/active-directory-saas-sapboc-tutorial/user1.png)

3. Üzerinde **kullanıcılar** seçin sayfasında, yeni kullanıcı ayrıntıları eklemek için **+**. 

    ![Kullanıcılar Sayfası Ekle](./media/active-directory-saas-sapboc-tutorial/user4.png)

    Ardından, aşağıdaki adımları tamamlayın:

    1. İçinde **kullanıcı kimliği** kutusuna, kullanıcının kullanıcı kimliği gibi girin **Britta**.

    2. İçinde **ad** kutusuna, kullanıcının ilk adını gibi girin **Britta**.

    3. İçinde **SOYADI** kutusuna, kullanıcının soyadını gibi girin **Simon**.

    4. İçinde **GÖRÜNEN adı** kutusuna, kullanıcının tam adını gibi girin **Britta Simon**.

    5. İçinde **e-posta** kutusuna, kullanıcının e-posta adresi gibi girin **brittasimon@contoso.com**.

    6. Üzerinde **Rollerini Seç** sayfasında, kullanıcı için uygun rolü seçin ve ardından **Tamam**.

      ![Rol seç](./media/active-directory-saas-sapboc-tutorial/user3.png)

    7. Seçin **kaydetmek** simgesi.    


### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, SAP Business nesnesi bulut için kullanıcı hesabı erişimi verilmesi tarafından Azure AD çoklu oturum açma kullanılacak Britta Simon kullanıcı izin verin.

SAP Business nesnesi buluta Britta Simon atamak için:

1. Azure Portalı'ndaki uygulamaları görünümünü açın ve dizin görünümüne gidin. Seçin **kurumsal uygulamalar**ve ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **SAP Business nesnesi bulut**.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-sapboc-tutorial/tutorial_sapboc_app.png) 

3. Soldaki menüde seçin **kullanıcılar ve gruplar**.

    ![Kullanıcıları ve grupları seçin][202] 

4. **Add (Ekle)** seçeneğini belirleyin. Ardından **eklemek atama** sayfasında, **kullanıcılar ve gruplar**.

    ![Atama Ekle sayfası][203]

5. Üzerinde **kullanıcılar ve gruplar** sayfasında, kullanıcıların, seçim listesinde **Britta Simon**.

6. Üzerinde **kullanıcılar ve gruplar** sayfasında, **seçin**.

7. Üzerinde **eklemek atama** sayfasında, **atamak**.

![Kullanıcı rolü atayın][200] 
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test.

SAP Business nesnesi bulut döşeme erişim panelinde seçtiğinizde, otomatik olarak SAP Business nesnesi bulut uygulamanıza oturum açmanız.

Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](active-directory-saas-tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](manage-apps/what-is-single-sign-on.md)


<!--Image references-->

[1]: ./media/active-directory-saas-sapboc-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-sapboc-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-sapboc-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-sapboc-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-sapboc-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-sapboc-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-sapboc-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-sapboc-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-sapboc-tutorial/tutorial_general_203.png

