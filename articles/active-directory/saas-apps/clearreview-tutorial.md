---
title: 'Öğretici: Azure Active Directory tümleştirmesiyle Temizle gözden geçirme | Microsoft Docs'
description: Azure Active Directory ve NET gözden arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 8264159a-11a2-4a8c-8285-4efea0adac8c
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/12/2018
ms.author: jeedes
ms.openlocfilehash: 6ce6661bf6d3841f7ade78a74d50a1d6eeefbdaf
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/14/2018
ms.locfileid: "39048003"
---
# <a name="tutorial-azure-active-directory-integration-with-clear-review"></a>Öğretici: Azure Active Directory tümleştirmesiyle Temizle gözden geçirme

Bu öğreticide, NET gözden geçirme Azure Active Directory (Azure AD) ile tümleştirmeyi öğrenin.

NET gözden geçirme, Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- NET gözden geçirme erişimi, Azure AD'de kontrol edebilirsiniz.
- Azure AD hesaplarına otomatik olarak imzalanan Temizle gözden geçirme (çoklu oturum açma) açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile NET gözden geçirme yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliğiniz
- Abonelik Temizle gözden geçirme çoklu oturum açma etkin

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, şunları yapabilirsiniz [bir aylık deneme sürümü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden Temizle incelemesi ekleniyor
2. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-clear-review-from-the-gallery"></a>Galeriden Temizle incelemesi ekleniyor
Azure AD'de Temizle gözden geçirme tümleştirmesini yapılandırmak için NET gözden geçirme Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden Temizle gözden geçirme eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **Temizle gözden geçirme**seçin **Temizle gözden geçirme** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Sonuç listesinde NET gözden geçirme](./media/clearreview-tutorial/tutorial_clearreview_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve gözden geçirmeyle Temizle "Britta Simon" adlı bir test kullanıcı tabanlı Azure AD çoklu oturum açmayı test.

Tek iş için oturum açma için Azure AD düz incelemesindeki karşılık gelen kullanıcı için bir kullanıcı Azure AD'de nedir bilmesi gerekir. Diğer bir deyişle, bir Azure AD kullanıcısı ve ilgili kullanıcı Temizle incelemesindeki arasında bir bağlantı ilişki kurulması gerekir.

Değeri Temizle incelemesindeki atayın **kullanıcı adı** değerini Azure AD'de **kullanıcı adı** bağlantı kurmak için.

Yapılandırma ve Azure AD çoklu oturum açma Temizle gözden geçirme ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[NET gözden geçirin test kullanıcısı oluşturma](#create-a-clear-review-test-user)**  - kullanıcı Azure AD gösterimini bağlı Temizle incelemesindeki Britta simon'un bir karşılığı vardır.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma Temizle gözden geçirme uygulamanızı yapılandırın.

**Azure AD çoklu oturum açma Temizle gözden geçirme ile yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **Temizle gözden geçirme** uygulama tümleştirme sayfası, tıklayın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/clearreview-tutorial/tutorial_clearreview_samlbase.png)

3. Üzerinde **Temizle gözden geçirme etki alanı ve URL'ler** bölümünde, uygulamada yapılandırmak istiyorsanız aşağıdaki adımları gerçekleştirin **IDP tarafından başlatılan** modu:

    ![Çoklu oturum açma bilgileri Temizle gözden geçirme etki alanı ve URL'leri](./media/clearreview-tutorial/tutorial_clearreview_url.png)

    a. İçinde **tanımlayıcı** metin kutusuna bir URL şu biçimi kullanarak: `https://<customer name>.clearreview.com/sso/metadata/`

    b. İçinde **yanıt URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://<customer name>.clearreview.com/sso/acs/`

4. Denetleme **Gelişmiş URL ayarlarını göster** ve uygulamada yapılandırmak istiyorsanız, aşağıdaki adımı uygulayın **SP** başlatılan modu:

    ![Çoklu oturum açma bilgileri Temizle gözden geçirme etki alanı ve URL'leri](./media/clearreview-tutorial/tutorial_clearreview_url_sp.png)

    İçinde **oturum açma URL'si** metin kutusuna bir URL şu biçimi kullanarak:`https://<customer name>.clearreview.com`

    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler gerçek oturum açma URL'si, tanımlayıcıya ve yanıt URL'si ile güncelleştirin. İlgili kişi [Temizle gözden geçirme Destek ekibine](https://clearreview.com/contact/) bu değerleri almak için.

5. Temizle gözden geçirme uygulama adı tanımlayıcısı talebi benzersiz kullanıcı tanıtıcı değeri bekler. Kullanıcı tanımlayıcısı değeri eşlemelisiniz **user.mail**.

    ![Öznitelik bölümü](./media/clearreview-tutorial/attribute.png)


6. Üzerinde **SAML imzalama sertifikası** bölümünde **sertifika (Base64)** ve bilgisayarınızdaki sertifika dosyasını kaydedin.

    ![Sertifika indirme bağlantısı](./media/clearreview-tutorial/tutorial_clearreview_certificate.png)

7. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açma Kaydet düğmesi yapılandırın](./media/clearreview-tutorial/tutorial_general_400.png)

8. Üzerinde **gözden geçirme yapılandırmayı Temizle** bölümünde **yapılandırma Temizle gözden geçirme** açmak için **yapılandırma oturum açma** penceresi. Kopyalama **oturum kapatma URL'si, SAML varlık kimliği ve SAML çoklu oturum açma hizmeti URL'si** gelen **hızlı başvuru bölümü.**

    ![Gözden geçirme yapılandırmasını Temizle](./media/clearreview-tutorial/tutorial_clearreview_configure.png) 

9. Çoklu oturum açmayı yapılandırmak için **Temizle gözden geçirme** yanı, açık **Temizle gözden geçirme** yönetici kimlik bilgileriyle portal.

10. Seçin **yönetici** sol gezinti bölmesinden.

    ![Çoklu oturum açma Kaydet düğmesi yapılandırın](./media/clearreview-tutorial/tutorial_clearreview_app_admin1.png)

11. Seçin **değişiklik** sayfanın alt kısmındaki.

    ![Çoklu oturum açma Kaydet düğmesi yapılandırın](./media/clearreview-tutorial/tutorial_clearreview_app_admin2.png)

12. Aşağıdaki adımları uygulayın **çoklu oturum açma ayarları** sayfası

    ![Çoklu oturum açma Kaydet düğmesi yapılandırın](./media/clearreview-tutorial/tutorial_clearreview_app_admin3.png)

    a. İçinde **veren URL'si** metin değerini yapıştırın **SAML varlık kimliği** , Azure Portalı'ndan kopyaladığınız.

    b. İçinde **SAML uç noktası** metin değerini yapıştırın **SAML çoklu oturum açma hizmeti URL'si** , Azure Portalı'ndan kopyaladığınız.    

    c. İçinde **SLO uç nokta** metin değerini yapıştırın **oturum açma hizmeti URL'si** , Azure Portalı'ndan kopyaladığınız. 

    d. İndirilen sertifikanın Not Defteri'nde açın ve içeriği yapıştırın **X.509 sertifikası** metin.   

13. **Kaydet**’e tıklayın.

> [!TIP]
> İçindeki bu yönergeleri kısa bir sürümünü artık okuyabilir [Azure portalında](https://portal.azure.com), uygulamayı hazırlama ayarladığınız sırada!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** aracılığıyla katıştırılmış belgelere erişebilir ve sekmesinde  **Yapılandırma** alttaki bölümü. Daha fazla bilgi edinebilirsiniz embedded belgeleri özelliği hakkında: [Azure AD'ye embedded belgeleri]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, sol bölmede, tıklayın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/clearreview-tutorial/create_aaduser_01.png)

2. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](./media/clearreview-tutorial/create_aaduser_02.png)

3. Açmak için **kullanıcı** iletişim kutusu, tıklayın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/clearreview-tutorial/create_aaduser_03.png)

4. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/clearreview-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** Britta Simon kullanıcı e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değeri yazın **parola** kutusu.

    d. **Oluştur**’a tıklayın.
  
### <a name="create-a-clear-review-test-user"></a>NET gözden geçirin test kullanıcısı oluşturma

Bu bölümde, Britta Simon Temizle incelemesindeki adlı bir kullanıcı oluşturun. Lütfen birlikte çalışarak [Temizle gözden geçirme Destek ekibine](https://clearreview.com/contact/) Temizle gözden geçirme platform kullanıcıları eklemek için.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, NET gözden geçirme için erişim izni verdiğinizde, Azure çoklu oturum açma kullanılacak Britta Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**Britta Simon Temizle gözden geçirmeye atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

2. Uygulamalar listesinde **Temizle gözden geçirme**.

    ![Uygulamalar listesinde NET gözden geçirme bağlantısı](./media/clearreview-tutorial/tutorial_clearreview_app.png)  

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı][202]

4. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli Temizle gözden geçirme kutucuğa tıkladığınızda, size otomatik olarak Temizle gözden geçirme uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/clearreview-tutorial/tutorial_general_01.png
[2]: ./media/clearreview-tutorial/tutorial_general_02.png
[3]: ./media/clearreview-tutorial/tutorial_general_03.png
[4]: ./media/clearreview-tutorial/tutorial_general_04.png

[100]: ./media/clearreview-tutorial/tutorial_general_100.png

[200]: ./media/clearreview-tutorial/tutorial_general_200.png
[201]: ./media/clearreview-tutorial/tutorial_general_201.png
[202]: ./media/clearreview-tutorial/tutorial_general_202.png
[203]: ./media/clearreview-tutorial/tutorial_general_203.png
