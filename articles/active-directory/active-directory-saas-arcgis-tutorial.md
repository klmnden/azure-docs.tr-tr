---
title: 'Öğretici: Azure Active Directory Tümleştirme ArcGIS Online ile | Microsoft Docs'
description: Azure Active Directory ArcGIS Online arasında tek oturum açma yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: a9e132a4-29e7-48bf-beb9-4148e617c8a9
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/13/2017
ms.author: jeedes
ms.openlocfilehash: fc6bf53e379fec266062753e550ef1c613816443
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2018
---
# <a name="tutorial-azure-active-directory-integration-with-arcgis-online"></a>Öğretici: Azure Active Directory Tümleştirme ArcGIS Online ile

Bu öğreticide, Azure Active Directory (Azure AD) ile ArcGIS çevrimiçi tümleştirmek öğrenin.

ArcGIS çevrimiçi Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- ArcGIS çevrimiçi erişimi, Azure AD'de kontrol edebilirsiniz.
- Azure AD hesaplarına otomatik olarak ArcGIS Online'a (çoklu oturum açma) açan kullanıcılarınıza etkinleştirebilirsiniz.
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirme ArcGIS Online ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir ArcGIS çevrimiçi çoklu oturum açma abonelik etkin

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, şunları yapabilirsiniz [bir aylık deneme sürümünü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. ArcGIS çevrimiçi galeriden ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-arcgis-online-from-the-gallery"></a>ArcGIS çevrimiçi galeriden ekleme
Azure AD ArcGIS Online tümleştirilmesi yapılandırmak için ArcGIS çevrimiçi galeriden yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

**ArcGIS çevrimiçi Galeriden eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **ArcGIS çevrimiçi**seçin **ArcGIS çevrimiçi** sonuç panelinden ardından **Ekle** uygulama eklemek için düğmeyi.

    ![ArcGIS çevrimiçi sonuçlar listesinde](./media/active-directory-saas-arcgis-tutorial/tutorial_arcgisonline_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma ArcGIS "Britta Simon" adlı bir test kullanıcı tabanlı Online ile test etme.

Tekli çalışmaya oturum için Azure AD ne karşılık gelen ArcGIS çevrimiçi bir kullanıcı için Azure AD içinde olduğu bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının ve ilgili kullanıcı ArcGIS çevrimiçi arasında bir bağlantı ilişkisi kurulması gerekir.

Çevrimiçi ArcGIS olarak değerini atayın **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** bağlantı ilişkisi oluşturmak için.

Yapılandırma ve Azure AD çoklu oturum açma ArcGIS Online ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[ArcGIS çevrimiçi test kullanıcısı oluşturma](#create-a-arcgis-online-test-user)**  - ArcGIS kullanıcı Azure AD gösterimini bağlantılı çevrimiçi Britta Simon, karşılık gelen sağlamak için.
4. **[Azure AD test kullanıcısı atayın](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Test çoklu oturum açma](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma ArcGIS çevrimiçi uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma ArcGIS Online ile yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **ArcGIS çevrimiçi** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/active-directory-saas-arcgis-tutorial/tutorial_arcgisonline_samlbase.png)

3. Üzerinde **ArcGIS çevrimiçi etki alanı ve URL'leri** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![ArcGIS çevrimiçi etki alanı ve URL'leri tek oturum açma bilgileri](./media/active-directory-saas-arcgis-tutorial/tutorial_arcgisonline_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://<companyname>.maps.arcgis.com`

    b. İçinde **tanımlayıcısı** metin kutusuna, URL şu biçimi kullanarak bir yazın: `<companyname>.maps.arcgis.com`

    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler gerçek oturum açma URL'si ve tanımlayıcı ile güncelleştirin. Kişi [ArcGIS çevrimiçi istemci destek ekibi](http://support.esri.com/en/) bu değerleri almak için. 
 


4. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **meta veri XML** ve meta veri dosyası, bilgisayarınıza kaydedin.

    ![Sertifika indirme bağlantısı](./media/active-directory-saas-arcgis-tutorial/tutorial_arcgisonline_certificate.png) 

5. Tıklatın **kaydetmek** düğmesi.

    ![Oturum açma tek Kaydet düğmesi yapılandırın](./media/active-directory-saas-arcgis-tutorial/tutorial_general_400.png)

6. Farklı web tarayıcısı penceresinde ArcGIS şirket sitenize yönetici olarak oturum açın.

7. Tıklatın **ayarları düzenleme**.

    ![Ayarları Düzenle](./media/active-directory-saas-arcgis-tutorial/ic784742.png "ayarlarını Düzenle")

8. Tıklatın **güvenlik**.

    ![Güvenlik](./media/active-directory-saas-arcgis-tutorial/ic784743.png "güvenlik")

9. Altında **Kurumsal oturum açma bilgileri**, tıklatın **AYARLANMIŞ kimlik sağlayıcı**.

    ![Kurumsal oturum açma bilgileri](./media/active-directory-saas-arcgis-tutorial/ic784744.png "Kurumsal oturum açmalar")

10. Üzerinde **ayarlanmış kimlik sağlayıcı** yapılandırma sayfasında, aşağıdaki adımları gerçekleştirin:
   
    ![Kimlik sağlayıcısı ayarlamak](./media/active-directory-saas-arcgis-tutorial/ic784745.png "ayarlamak kimlik sağlayıcısı")
   
    a. İçinde **adı** metin kutusuna, kuruluşunuzun adını yazın.

    b. İçin **Kurumsal kimlik sağlayıcısı meta verileri sağlanan kullanarak**seçin **dosya**.

    c. İndirilen meta veri dosyanızı karşıya yüklemek için tıklayın **dosya**.

    d. Tıklatın **KÜMESİ kimlik SAĞLAYICISI**.

> [!TIP]
> Şimdi bu yönergeleri içinde kısa bir sürümünü okuyabilirsiniz [Azure portal](https://portal.azure.com)uygulaması kuruluyor yaparken!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** sekmesinde ve aracılığıyla katıştırılmış belgelere erişebilir **yapılandırma** alt bölüm. Daha fazla bilgiyi burada embedded belgeler özelliği hakkında: [Azure AD embedded belgeler]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Sol bölmede, Azure portal'ı tıklatın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/active-directory-saas-arcgis-tutorial/create_aaduser_01.png)

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantılar](./media/active-directory-saas-arcgis-tutorial/create_aaduser_02.png)

3. Açmak için **kullanıcı** iletişim kutusu, tıklatın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/active-directory-saas-arcgis-tutorial/create_aaduser_03.png)

4. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/active-directory-saas-arcgis-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** kullanıcı Britta Simon e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değer aşağı yazma **parola** kutusu.

    d. **Oluştur**’a tıklayın.
 
### <a name="create-a-arcgis-online-test-user"></a>ArcGIS çevrimiçi test kullanıcısı oluşturma

Azure AD kullanıcıların ArcGIS çevrimiçi oturum etkinleştirmek için bunlar ArcGIS çevrimiçi sağlanmalıdır.  
ArcGIS söz konusu olduğunda, sağlama el ile bir görev çevrimiçidir.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1. Oturum, **ArcGIS** Kiracı.

2. Tıklatın **üye davet et**.
   
    ![Üye davet](./media/active-directory-saas-arcgis-tutorial/ic784747.png "üye davet")

3. Seçin **üye otomatik olarak bir e-posta göndermeden eklemek**ve ardından **sonraki**.
   
    ![Üyeler otomatik olarak Ekle](./media/active-directory-saas-arcgis-tutorial/ic784748.png "üyeleri otomatik olarak Ekle")

4. Üzerinde **üyeleri** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
   
     ![Ekleme ve gözden](./media/active-directory-saas-arcgis-tutorial/ic784749.png "ekleme ve gözden geçirme")
    
     a. Girin **e-posta**, **ad**, ve **Soyadı** sağlamak istediğiniz geçerli bir AAD hesabının.
  
     b. Tıklatın **ekleme ve gözden geçirme**.
5. Girdiğiniz ve ardından verileri gözden **ADD MEMBERS**.
   
    ![Üye Ekle](./media/active-directory-saas-arcgis-tutorial/ic784750.png "Üye Ekle")
        
    > [!NOTE]
    > Azure Active Directory hesap sahibi bir e-posta alır ve onu etkinleştirilmeden önce kendi hesabı onaylamak için bağlantıyı izleyin.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Britta ArcGIS Online'a erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**Britta Simon ArcGIS Online'a atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **ArcGIS çevrimiçi**.

    ![Uygulamalar listesinde ArcGIS çevrimiçi bağlantı](./media/active-directory-saas-arcgis-tutorial/tutorial_arcgisonline_app.png)  

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantı][202]

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Ekleme atama bölmesi][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde ArcGIS çevrimiçi kutucuğa tıkladığınızda, otomatik olarak ArcGIS çevrimiçi uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](active-directory-saas-tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/active-directory-saas-arcgis-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-arcgis-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-arcgis-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-arcgis-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-arcgis-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-arcgis-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-arcgis-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-arcgis-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-arcgis-tutorial/tutorial_general_203.png

