---
title: 'Öğretici: Azure Active Directory Tümleştirme ile Marketo | Microsoft Docs'
description: Çoklu oturum açma Azure Active Directory ile Marketo arasında yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: b88c45f5-d288-4717-835c-ca965add8735
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/09/2017
ms.author: jeedes
ms.openlocfilehash: 48aee4565548fd57ef9925a41299b06660df2718
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2018
ms.locfileid: "34348137"
---
# <a name="tutorial-azure-active-directory-integration-with-marketo"></a>Öğretici: Marketo Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile Marketo tümleştirmek öğrenin.

Marketo Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Marketo erişimi, Azure AD'de kontrol edebilirsiniz
- Otomatik olarak için Marketo (çoklu oturum açma) ile Azure AD hesaplarına açan kullanıcılarınıza etkinleştirebilirsiniz
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirme Marketo ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir Marketo çoklu oturum açma etkin abonelik

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden Marketo ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-marketo-from-the-gallery"></a>Galeriden Marketo ekleme
Azure AD Marketo tümleştirilmesi yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden Marketo eklemeniz gerekir.

**Galeriden Marketo eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Uygulamalar][3]

4. Arama kutusuna **Marketo**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_search.png)

5. Sonuçlar panelinde seçin **Marketo**ve ardından **Ekle** uygulama eklemek için düğmesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Çoklu oturum açmayı yapılandırma ve Azure AD sınama
Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon." olarak adlandırılan bir test kullanıcı tabanlı Marketo ile test etme

Tekli çalışmaya oturum için Azure AD Marketo karşılık gelen kullanıcı için bir kullanıcı Azure AD'de nedir bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının Marketo ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Marketo içinde değerini atayın **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** bağlantı ilişkisi oluşturmak için.

Yapılandırma ve Azure AD çoklu oturum açma Marketo ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Marketo test kullanıcısı oluşturma](#creating-a-marketo-test-user)**  - Britta Simon, karşılık gelen kullanıcı Azure AD gösterimini bağlı Marketo sağlamak için.
4. **[Azure AD test kullanıcısı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açmayı test](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma Marketo uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma ile Marketo yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **Marketo** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_samlbase.png)

3. Üzerinde **Marketo etki alanı ve URL'leri** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_url.png)

    a. İçinde **tanımlayıcısı** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://saml.marketo.com/sp`

    b. İçinde **yanıt URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://login.marketo.com/saml/assertion/\<munchkinid\>`

    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler gerçek tanımlayıcısı ve yanıt URL'si ile güncelleştirin. Kişi [Marketo destek ekibi](http://investors.marketo.com/contactus.cfm) bu değerleri almak için.
 
4. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **sertifika (Base64)** ve sertifika dosyayı bilgisayarınıza kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_certificate.png) 

5. Tıklatın **kaydetmek** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-marketo-tutorial/tutorial_general_400.png)

6. Üzerinde **Marketo yapılandırma** 'yi tıklatın **yapılandırma Marketo** açmak için **yapılandırma oturum açma** penceresi. Kopya **Sign-Out URL, SAML varlık kimliği ve SAML çoklu oturum açma hizmet URL'si** gelen **hızlı başvuru bölümü.**

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_configure.png) 

7. Uygulamanızı Munchkin kimliğini almak için Marketo için yönetici kimlik bilgilerinizi kullanarak oturum açın ve aşağıdaki işlemleri gerçekleştirin:
   
    a. Marketo uygulamasına yönetici kimlik bilgilerini kullanarak oturum açın.
   
    b. Tıklatın **yönetici** üst gezinti bölmesindeki düğmesi.
   
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_06.png) 
   
    c. Tümleştirme menüsüne gidin ve tıklayın **Munchkin bağlantı**.
   
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_11.png)
   
    d. Ekranda gösterilen Munchkin kimliğini kopyalayın ve yanıt URL'nizi Azure AD Yapılandırma Sihirbazı tamamlayın.
   
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_12.png) 

8. Uygulamada SSO yapılandırmak için izleyin aşağıdaki adımları:
   
    a. Marketo uygulamasına yönetici kimlik bilgilerini kullanarak oturum açın.
   
    b. Tıklatın **yönetici** üst gezinti bölmesindeki düğmesi.
   
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_06.png) 
   
    c. Tümleştirme menüsüne gidin ve tıklayın **çoklu oturum açma**.
   
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_07.png) 
   
    d. SAML ayarlarını etkinleştirmek için **Düzenle** düğmesi.
   
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_08.png) 
   
    e. **Etkin** çoklu oturum açma ayarları.
   
    f. Yapıştır **SAML varlık kimliği**, **verenin kimliği** metin kutusu.
   
    g. İçinde **varlık kimliği** metin kutusuna, URL olarak girin `http://saml.marketo.com/sp`.
   
    h. Kullanıcı Kimliği konum olarak seçin **adı tanımlayıcı öğe**.
   
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_09.png)
   
    > [!NOTE]
    > Kullanıcı tanımlayıcısı UPN değeri sonra değişiklik özniteliği sekmesini değerinde değilse.
   
    i. Azure AD Yapılandırma Sihirbazı'ndan indirilen sertifikasını yükleyin. **Kaydet** ayarlar.
   
    j. Yeniden yönlendirme sayfası ayarlarını düzenleyin.
   
    k. Yapıştır **SAML çoklu oturum açma hizmet URL'si** içinde **oturum açma URL'si** metin kutusu.
   
    l. Yapıştır **Sign-Out URL** içinde **oturum kapatma URL'si** metin kutusu.
   
    m. İçinde **hata URL**, kopyalama, **Marketo örnek URL'si** tıklatıp **kaydetmek** ayarlarını kaydetmek için düğmesi.
   
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_10.png)

9. Kullanıcılar için SSO'yu etkinleştirmek için aşağıdaki eylemleri tamamlayın:
   
    a. Marketo uygulamasına yönetici kimlik bilgilerini kullanarak oturum açın.
   
    b. Tıklatın **yönetici** üst gezinti bölmesindeki düğmesi.
   
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_06.png) 
   
    c. Gidin **güvenlik** menüsüne ve ardından **oturum açma ayarları**.
   
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_13.png)
   
    d. Denetleme **gerektiren SSO** seçeneği ve **kaydetmek** ayarlar.
   
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_14.png)

> [!TIP]
> Şimdi bu yönergeleri içinde kısa bir sürümünü okuyabilirsiniz [Azure portal](https://portal.azure.com)uygulaması kuruluyor yaparken!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** sekmesinde ve aracılığıyla katıştırılmış belgelere erişebilir **yapılandırma** alt bölüm. Daha fazla bilgiyi burada embedded belgeler özelliği hakkında: [Azure AD embedded belgeler]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

![Azure AD Kullanıcı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portal**, sol gezinti bölmesinde tıklatın **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-marketo-tutorial/create_aaduser_01.png) 

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-marketo-tutorial/create_aaduser_02.png) 

3. Açmak için **kullanıcı** iletişim kutusunda, tıklatın **Ekle** iletişim kutusunun üst kısmında.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-marketo-tutorial/create_aaduser_03.png) 

4. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-marketo-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna, türü **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna, türü **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.
 
### <a name="creating-a-marketo-test-user"></a>Marketo test kullanıcısı oluşturma

Bu bölümde, Marketo içinde Britta Simon adlı bir kullanıcı oluşturun. Marketo platformunda bir kullanıcı oluşturmak için aşağıdaki adımları izleyin.

1. Marketo uygulamasına yönetici kimlik bilgilerini kullanarak oturum açın.

2. Tıklatın **yönetici** üst gezinti bölmesindeki düğmesi.
   
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_06.png) 

3. Gidin **güvenlik** menüsüne ve ardından **kullanıcıları ve rolleri**
   
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_19.png)  

4. Tıklatın **yeni kullanıcı davet** kullanıcılar sekmesinde bağlantı
   
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_15.png) 

5. Yeni kullanıcı davet Sihirbazı'nda aşağıdaki bilgileri girin
   
    a. Kullanıcının girmesi **e-posta** metin kutusuna adresi
   
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_16.png)
   
    b. Girin **ad** metin kutusuna
   
    c. Girin **Soyadı** metin kutusuna
   
    d. **İleri**’ye tıklayın

6. İçinde **izinleri** sekmesine **userRoles** tıklatıp **sonraki**
   
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_17.png)
7. Tıklatın **Gönder** Kullanıcı Davet Gönder düğmesi
   
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_18.png)

8. Kullanıcı e-posta bildirimi alır ve bağlantıya tıklayın ve hesabını etkinleştirmek için parolasını değiştirmek zorundadır. 

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atama

Bu bölümde, Britta Marketo için erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı atama][200] 

**Marketo için Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **Marketo**.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_app.png) 

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    ![Kullanıcı atama][202] 

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Kullanıcı atama][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açmayı test etme

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli Marketo parçasında tıklattığınızda, otomatik olarak Marketo uygulamanıza açan.

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](active-directory-saas-tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/active-directory-saas-marketo-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-marketo-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-marketo-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-marketo-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-marketo-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-marketo-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-marketo-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-marketo-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-marketo-tutorial/tutorial_general_203.png

