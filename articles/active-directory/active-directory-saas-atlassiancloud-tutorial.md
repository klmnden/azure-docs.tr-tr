---
title: "Öğretici: Azure Active Directory Tümleştirme Atlassian bulut ile | Microsoft Docs"
description: "Çoklu oturum açma Azure Active Directory Atlassian bulut arasındaki yapılandırmayı öğrenin."
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: 729b8eb6-efc4-47fb-9f34-8998ca2c9545
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/06/2017
ms.author: jeedes
ms.openlocfilehash: fff906913b637c47d394f9127983a08f96d568e4
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-atlassian-cloud"></a>Öğretici: Azure Active Directory Tümleştirme Atlassian bulut ile

Bu öğreticide, Azure Active Directory (Azure AD) ile Atlassian bulut tümleştirme öğrenin.

Atlassian bulut Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Atlassian bulut erişimi olan Azure AD'de kontrol edebilirsiniz.
- Azure AD hesaplarına otomatik olarak Atlassian buluta (çoklu oturum açma) açan kullanıcılarınıza etkinleştirebilirsiniz.
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirme Atlassian bulut ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- SAML çoklu oturum açma Atlassian bulut ürünleri için etkinleştirmek için Identity Manager ayarlamanız gerekir. Daha fazla bilgi edinmek [Identity Manager]( https://www.atlassian.com/enterprise/cloud/identity-manager)

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, şunları yapabilirsiniz [bir aylık deneme sürümünü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden Atlassian bulut ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-atlassian-cloud-from-the-gallery"></a>Galeriden Atlassian bulut ekleme
Azure AD Atlassian bulut tümleştirilmesi yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden Atlassian bulut eklemeniz gerekir.

**Galeriden Atlassian bulut eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **Atlassian bulut**seçin **Atlassian bulut** sonuç panelinden ardından **Ekle** uygulama eklemek için düğmeyi.

    ![Sonuçlar listesinde Atlassian bulut](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırmanız ve Atlassian bulut ile Azure AD çoklu oturum açmayı test "Britta Simon" adlı bir test kullanıcı tabanlı.

Tekli çalışmaya oturum için Azure AD ne karşılık gelen Atlassian bulutta bir kullanıcı için Azure AD içinde olduğu bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının ve ilgili kullanıcı Atlassian bulutta arasında bir bağlantı ilişkisi kurulması gerekir.

Değeri Atlassian buluta atayın **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** bağlantı ilişkisi oluşturmak için.

Yapılandırma ve Azure AD çoklu oturum açma Atlassian bulut ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Bir Atlassian bulut test kullanıcısı oluşturma](#create-an-atlassian-cloud-test-user)**  - Britta Simon, karşılık gelen kullanıcı Azure AD gösterimini bağlı Atlassian bulut sağlamak için.
4. **[Azure AD test kullanıcısı atayın](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Test çoklu oturum açma](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma Atlassian bulut uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma Atlassian bulut ile yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **Atlassian bulut** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_samlbase.png)

3. Üzerinde **Atlassian bulut etki alanı ve URL'leri** bölümünde, uygulamada yapılandırmak istiyorsanız aşağıdaki adımları gerçekleştirin **IDP** modu tarafından başlatılan:

    ![Atlassian bulut etki alanı ve URL'leri tek oturum açma bilgileri](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_url_new.png)
    
    a. İçinde **tanımlayıcısı** metin kutusuna, URL'yi yazın:`https://id.atlassian.com/login`
    
    b. İçinde **yanıt URL'si** metin kutusuna, URL'yi yazın:`https://id.atlassian.com/login/saml/acs`

    c. İçinde **geçiş durumunu** metin kutusuna, URL şu biçimi kullanarak bir yazın:`https://<instancename>.atlassian.net`

4. Denetleme **Göster Gelişmiş URL ayarları** ve uygulamada yapılandırmak istiyorsanız aşağıdaki adımı gerçekleştirin **SP** modunda başlatılan:

    ![Atlassian bulut etki alanı ve URL'leri tek oturum açma bilgileri](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_url1.png)

    İçinde **oturum açma URL'si metin kutusuna**, şu biçimi kullanarak bir URL yazın:`https://<instancename>.atlassian.net`

    > [!NOTE] 
    > Bu değerler gerçek değildir. Öğreticide daha sonra açıklanan Atlassian bulut SAML Yapılandırma ekranından bu değerleri alabilirsiniz.

5. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **Certificate(Base64)** ve sertifika dosyayı bilgisayarınıza kaydedin.

    ![Sertifika indirme bağlantısı](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_certificate.png) 

6. Tıklatın **kaydetmek** düğmesi.

    ![Oturum açma tek Kaydet düğmesi yapılandırın](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_400.png)

7. Üzerinde **Atlassian bulut Yapılandırması** 'yi tıklatın **Atlassian bulut yapılandırma** açmak için **yapılandırma oturum açma** penceresi. Kopya **SAML varlık kimliği ve SAML çoklu oturum açma hizmet URL'si** gelen **hızlı başvuru bölümü.**

    ![Atlassian bulut yapılandırması](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_configure.png) 

8. Yönetici hakları kullanarak Atlassian portalı oturum açma, uygulamanız için yapılandırılmış SSO almak için.

9. Gidin **Atlassian Site Yönetimi** > **kuruluşlar ve güvenlik**. Henüz yapmadıysanız oluşturabilir ve kuruluşunuzun adını. Sol gezinti bölmesinde, ardından **etki alanları**.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_06.png)

10. Etki alanınızı - doğrulamak istediğiniz yolu seçin **DNS** veya **HTTPS**.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_17.png)

11. DNS doğrulamak üzere seçtiğiniz **DNS** sekmesi **etki alanları** sayfasında ve aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_18.png)

    a. Tıklatın **kopyalama** TXT kaydı değeri kopyalanacak.

    b. Yeni bir kayıt eklemek için ayarları sayfası, DNS'den Bul

    c. Yeni bir kayıt ekleme seçeneğini seçin ve kopyaladığınız değeri yapıştırın **etki alanları** sayfasına **değeri** alan. DNS sunucunuzun olarak da başvurabilir **yanıt** veya **açıklama**.

    d. DNS kaydı ayrıca aşağıdaki alanları içerebilir:
    
    * **Kayıt türü**: girin **TXT**
    * **Ana/ad/diğer**: (@ veya boş) varsayılan adı bırakın
    * **Yaşam süresi (TTL)**: girin **86400**
    
    e.  Kaydı kaydedin.

12. Geri dönüp **etki alanları sayfasına** kuruluş yönetimi ve tıklatın **etki alanını doğrula** düğmesi. Açılır ve tıklatın, etki alanı adınızı girin **etki alanını doğrula** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_19.png)
    
    > [!NOTE]
    > Etki alanı doğrulama başarılı olup olmadığını TXT kaydı değişikliklerin etkili olması için 72 saat kadar sürebilir, hemen bilemezsiniz. Denetleyin, **etki alanları** yakında doğrulama durumunuz için bu adımları gerçekleştirdikten sonra sayfa. Aşağıdaki ekran güncelleştirilmiş durumundaki bkz **doğrulandı**.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_20.png)

13. HTTPS doğrulamak üzere seçtiğiniz **HTTPS** sekmesi **etki alanları** sayfasında ve şu adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_21.png)

    a.  Tıklatın **karşıdan yükleme dosyası** HTML dosyası indirilemedi.

    b.  HTML dosyası, etki alanının kök dizinine yükleyin.

14. Geri dönüp **etki alanları** sayfasında Kuruluş Yönetimi'nde ve tıklayın **etki alanını doğrula** düğmesi. Girin, **etki alanı adı** tıklayın ve açılan **etki alanını doğrula** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_22.png)

15. Doğrulama işlemi kök dizininde karşıya dosya bulabiliyorsa, etki alanının durumu güncelleştirmeleri için **doğrulandı**.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_23.png)

    > [!NOTE]
    > Daha fazla bilgi için [Atlassian'ın etki alanı doğrulama belgeleri](https://confluence.atlassian.com/cloud/domain-verification-873871234.html)

16. Sol gezinti çubuğunda **SAML çoklu oturum açma**. Henüz yapmadıysanız, Atlassian'ın Identity Manager abone olun.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_11.png)

17. İçinde **eklemek SAML Yapılandırması** Ekle iletişim kutusu aşağıdaki gibi kimlik sağlayıcı ayarları:

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_12.png)

    a. İçinde **kimlik sağlayıcısı varlık kimliği** metin kutusunda, değerini yapıştırın **SAML varlık kimliği** Azure portalından kopyalanan.

    b. İçinde **kimlik sağlayıcısı SSO URL** metin kutusunda, değerini yapıştırın **SAML çoklu oturum açma hizmet URL'si** Azure portalından kopyalanan.

    c. İndirilen sertifika Not Defteri'nde açın ve başlangıç ve bitiş satırları olmadan değerleri kopyalamak ve yapıştırmak **ortak X509 sertifika** kutusu.
    
    d. Tıklatın **yapılandırmayı Kaydet** ayarları kaydetmek için.
     
18. Doğru URL'leri Kurulum sahip olduğunuzdan emin olmak için Azure AD ayarlarını güncelleştirin.
  
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_13.png)

    a. Kopya **SP kimlik kimliği** SAML ekranında ve değeri yapıştırın **tanımlayıcısı** Atlassian bulut altında Azure portalında kutusunda **etki alanı ve URL'leri** bölüm.
    
    b. Kopya **SP onaylama tüketici hizmeti URL'si** SAML ekranında ve değeri yapıştırın **yanıt URL'si** Atlassian bulut altında Azure portalında kutusunda **etki alanı ve URL'leri** bölüm.
    
    c. Oturum üzerinde Kiracı Atlassian bulutun URL'dir. 
    
19. Azure portalında tıklatın **kaydetmek** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_400.png)

> [!TIP]
> Şimdi bu yönergeleri içinde kısa bir sürümünü okuyabilirsiniz [Azure portal](https://portal.azure.com)uygulaması kuruluyor yaparken!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** sekmesinde ve aracılığıyla katıştırılmış belgelere erişebilir **yapılandırma** alt bölüm. Daha fazla bilgiyi burada embedded belgeler özelliği hakkında: [Azure AD embedded belgeler]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Sol bölmede, Azure portal'ı tıklatın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/active-directory-saas-atlassian-cloud-tutorial/create_aaduser_01.png)

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantılar](./media/active-directory-saas-atlassian-cloud-tutorial/create_aaduser_02.png)

3. Açmak için **kullanıcı** iletişim kutusu, tıklatın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/active-directory-saas-atlassian-cloud-tutorial/create_aaduser_03.png)

4. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/active-directory-saas-atlassian-cloud-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** kullanıcı Britta Simon e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değer aşağı yazma **parola** kutusu.

    d. **Oluştur**'a tıklayın.
  
### <a name="create-an-atlassian-cloud-test-user"></a>Bir Atlassian bulut test kullanıcısı oluşturma

Azure AD kullanıcılarının Atlassian bulut oturum açmayı etkinleştirmek için bunlar Atlassian bulutunu sağlanmalıdır. Atlassian bulut durumunda sağlama bir el ile bir görevdir.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1. Site Yönetim bölümünde Atlassian portalı tıklayın **kullanıcılar** düğmesi

    ![Atlassian bulut kullanıcı oluştur](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_14.png) 

2. Tıklatın **davet kullanıcı** Atlassian bulutta bir kullanıcı oluşturmak için düğmesi.

    ![Atlassian bulut kullanıcı oluştur](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_15.png) 

3. Kullanıcının girin **e-posta adresi** ve uygulama erişimi atayın. 

    ![Atlassian bulut kullanıcı oluştur](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_16.png)
 
4. Tıklatın **kullanıcıları davet** düğmesi, kullanıcı için e-posta daveti gönderir ve daveti kabul ettikten sonra kullanıcı sistemde etkin olacaktır. 

>[!NOTE] 
>Toplu kullanıcılar tıklatarak da oluşturabilirsiniz **Toplu oluşturma** kullanıcılar bölümünde düğmesi.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Britta Atlassian buluta erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**Britta Simon Atlassian buluta atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **Atlassian bulut**.

    ![Uygulamalar listesinde Atlassian bulut bağlantı](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_app.png)  

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantı][202]

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Ekleme atama bölmesi][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açmayı test edin

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli Atlassian bulut parçasında tıklattığınızda, otomatik olarak Atlassian bulut uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](active-directory-saas-tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_203.png

