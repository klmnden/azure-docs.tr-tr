---
title: "Öğretici: Azure Active Directory Tümleştirme ile çalışma alanına Facebook tarafından | Microsoft Docs"
description: "Çoklu oturum açma Azure Active Directory ve Facebook ile çalışma alanına arasında yapılandırmayı öğrenin."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 30f2ee64-95d3-44ef-b832-8a0a27e2967c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: jeedes
ms.openlocfilehash: 1590a66f215f0c093d24ff602c0ad951ba1e1eea
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-workplace-by-facebook"></a>Öğretici: Facebook ile çalışma alanına Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile çalışma alanına Facebook ile tümleştirmek öğrenin.

Çalışma alanına Facebook ile Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Facebook ile çalışma alanına erişimi olan Azure AD'de kontrol edebilirsiniz
- Otomatik olarak çalışma alanına göre Facebook (çoklu oturum açma) ile Azure AD hesaplarına açan kullanıcılarınıza etkinleştirebilirsiniz
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Ön koşullar

Facebook ile çalışma alanına ile Azure AD tümleştirme yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Abonelik çalışma alanına Facebook çoklu oturum açma tarafından etkin

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden Facebook ile çalışma alanına ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-workplace-by-facebook-from-the-gallery"></a>Galeriden Facebook ile çalışma alanına ekleme
Azure AD çalışma alanına Facebook ile tümleştirilmesi yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden Facebook ile çalışma alanına eklemeniz gerekir.

**Galeriden Facebook ile çalışma alanına eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Uygulamalar][3]

4. Arama kutusuna **Facebook ile çalışma alanına**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-workplacebyfacebook-tutorial/tutorial_workplacebyfacebook_search.png)

5. Sonuçlar panelinde seçin **Facebook ile çalışma alanına**ve ardından **Ekle** uygulama eklemek için düğmesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-workplacebyfacebook-tutorial/tutorial_workplacebyfacebook_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Çoklu oturum açmayı yapılandırma ve Azure AD sınama
Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma ile çalışma alanına Facebook "Britta Simon." olarak adlandırılan bir test kullanıcıya bağlı olarak test etme

Tekli çalışmaya oturum için Azure AD ne karşılık gelen çalışma Facebook tarafından bir kullanıcı için Azure AD içinde olduğu bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının çalışma Facebook ile ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Bu bağlantı değeri atayarak ilişkisi **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** Facebook ile çalışma.

Yapılandırmak ve Azure AD çoklu oturum açma ile çalışma alanına Facebook tarafından sınamak için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Yeniden kimlik doğrulamanın sıklığını yapılandırma](#configuring-reauthentication-frequency)**  - çalışma alanı için bir SAML denetimi isteyecek şekilde yapılandırmak için.
3. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
4. **[Facebook test kullanıcı tarafından bir çalışma alanı oluşturma](#creating-a-workplace-by-facebook-test-user)**  - karşılık gelen Britta Simon, çalışma alanına kullanıcı Azure AD gösterimini bağlı Facebook tarafından sağlamak için.
5. **[Azure AD test kullanıcısı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
6. **[Çoklu oturum açmayı test](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma Facebook uygulama tarafından çalışma alanınızda yapılandırın.

**Facebook ile çalışma alanına ile Azure AD çoklu oturum açma yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **Facebook ile çalışma alanına** uygulama tümleştirmesi sayfasında, tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-workplacebyfacebook-tutorial/tutorial_workplacebyfacebook_samlbase.png)

3. Üzerinde **Facebook etki alanı ve URL'ler ile çalışma alanına** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-workplacebyfacebook-tutorial/tutorial_workplacebyfacebook_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın:`https://<instancename>.facebook.com`

    b. İçinde **tanımlayıcısı** metin kutusuna, URL şu biçimi kullanarak bir yazın:`https://www.facebook.com/company/<instancename>`

    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler gerçek oturum açma URL'si ve tanımlayıcı ile güncelleştirin. Kişi [Facebook istemcisini destek ekibi ile çalışma alanına](https://workplace.fb.com/faq/) bu değerleri almak için. 

4. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **sertifika (Base64)** ve sertifika dosyayı bilgisayarınıza kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-workplacebyfacebook-tutorial/tutorial_workplacebyfacebook_certificate.png) 

5. Tıklatın **kaydetmek** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-workplacebyfacebook-tutorial/tutorial_general_400.png)

6. Üzerinde **Facebook yapılandırmaya göre çalışma alanına** 'yi tıklatın **yapılandırma çalışma Facebook tarafından** açmak için **yapılandırma oturum açma** penceresi. Kopya **Sign-Out URL, SAML varlık kimliği ve SAML çoklu oturum açma hizmet URL'si** gelen **hızlı başvuru bölümü.**

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-workplacebyfacebook-tutorial/config.png) 

7. Bir farklı web tarayıcısı penceresinde, işyeriniz Facebook şirket sitenin yönetici olarak oturum açın.
  
   > [!NOTE] 
   > SAML kimlik doğrulama işleminin bir parçası olarak, Azure AD ile parametreleri geçirmek için sorgu dizeleri 2,5 kilobayt boyutu en fazla çalışma alanına değerlendirebilir.

8. İçinde **şirket Pano**gidin **kimlik doğrulaması** sekmesi.

9. Altında **SAML kimlik doğrulaması**seçin **yalnızca SSO** aşağı açılan listeden.

10. Kopyalanan değerleri giriş **Facebook yapılandırmaya göre çalışma alanına** bölümüne karşılık gelen alanlara Azure portalının:

    *   İçinde **SAML URL** metin değerini yapıştırın **çoklu oturum açma hizmet URL'si**, Azure portalından kopyalanan.
    *   İçinde **SAML veren URL'si metin kutusuna**, değerini yapıştırın **SAML varlık kimliği**, hangi Azure portalından kopyalanır.
    *   İçinde **SAML oturum kapatma yönlendirme** (isteğe bağlı) değerini yapıştırın **Sign-Out URL**, Azure portalından kopyalanan.
    *   Açık, **base-64 kodlamalı sertifika** Azure portalından indirdiğiniz Not Defteri'nde, içeriğini, panoya kopyalayın ve yapıştırın kendisine **SAML sertifika** metin kutusu.

11. Hedef kitle URL, alıcı URL girmeniz gerekebilir ve ACS (onaylama tüketici hizmeti) URL altında listelenen **SAML Yapılandırması** bölümü.

12. Bölümün sonuna kaydırın ve tıklayın **Test SSO** düğmesi. Bu sonuç ile Azure AD oturum açma sayfası görünen açılır pencerede sunulur. Kimlik bilgilerinizi doğrulamak için normal olarak girin. 

    **Sorun giderme:** geri Azure AD'den aynıdır, oturum açarken iş yeri hesabı döndürülen e-posta adresi emin olun.

13. Test başarıyla tamamlandıktan sonra sayfanın alt kısmına kaydırın ve tıklayın **kaydetmek** düğmesi.

14. Çalışma alanına kullanan tüm kullanıcıların kimlik doğrulaması için Azure AD oturum açma sayfası artık sunulur.

15. **SAML oturum kapatma yeniden yönlendir (isteğe bağlı)** - 

    İsteğe bağlı olarak, Azure AD oturum kapatma sayfasını işaret etmek için kullanılan bir SAML oturum kapatma URL'si yapılandırın seçebilirsiniz. Bu ayar etkin ve yapılandırılmış olduğunda, kullanıcı artık çalışma alanına oturum kapatma sayfasına yönlendirilir. Bunun yerine, kullanıcı oturum kapatma SAML yönlendirmek ayarında eklendi URL'ye yönlendirilir.


> [!TIP]
> Şimdi bu yönergeleri içinde kısa bir sürümünü okuyabilirsiniz [Azure portal](https://portal.azure.com)uygulaması kuruluyor yaparken!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** sekmesinde ve aracılığıyla katıştırılmış belgelere erişebilir **yapılandırma** alt bölüm. Daha fazla bilgiyi burada embedded belgeler özelliği hakkında: [Azure AD embedded belgeler]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="configuring-reauthentication-frequency"></a>Yeniden kimlik doğrulamanın sıklığını yapılandırma

Her gün, üç gün, hafta, iki hafta, ay için bir SAML denetimi istemek için çalışma alanına yapılandırabilirsiniz ya da hiç.

> [!NOTE] 
>Mobil uygulamalar üzerinde SAML denetimi için en düşük değer için bir hafta ayarlanır.

Ayrıca, bir SAML Sıfırla düğmesini kullanarak tüm kullanıcılar için zorlayabilirsiniz: artık gerektiren SAML kimlik doğrulaması tüm kullanıcılar için.


### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

![Azure AD Kullanıcı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portal**, sol gezinti bölmesinde tıklatın **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-workplacebyfacebook-tutorial/create_aaduser_01.png) 

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-workplacebyfacebook-tutorial/create_aaduser_02.png) 

3. Açmak için **kullanıcı** iletişim kutusunda, tıklatın **Ekle** iletişim kutusunun üst kısmında.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-workplacebyfacebook-tutorial/create_aaduser_03.png) 

4. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-workplacebyfacebook-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna, türü **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna, türü **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**'a tıklayın.
 
### <a name="creating-a-workplace-by-facebook-test-user"></a>Facebook test kullanıcı tarafından bir çalışma alanı oluşturma

Bu bölümde, Britta Simon adlı bir kullanıcı tarafından Facebook çalışma alanında oluşturulur. Çalışma alanına Facebook tarafından yalnızca zaman sağlama, varsayılan olarak etkin olduğu destekler.

Bu bölümdeki hiçbir şey yoktur. Bir kullanıcı çalışma Facebook tarafından yoksa, çalışma alanına erişmeye çalıştığında yeni bir Facebook tarafından oluşturulur.

>[!Note]
>Bir kullanıcı el ile oluşturmanız gerekirse başvurun [Facebook istemcisini destek ekibi ile çalışma](https://workplace.fb.com/faq/)

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atama

Bu bölümde, Facebook ile çalışma alanına erişim vererek, Azure çoklu oturum açma kullanılacak Britta Simon etkinleştirin.

![Kullanıcı atama][200] 

**Çalışma alanına Facebook tarafından Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **Facebook ile çalışma alanına**.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-workplacebyfacebook-tutorial/tutorial_workplacebyfacebook_app.png) 

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    ![Kullanıcı atama][202] 

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Kullanıcı atama][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açmayı test etme

Çoklu oturum açma ayarlarınızı test etmek isterseniz, erişim paneli açın.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](active-directory-saas-access-panel-introduction.md).


## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](active-directory-saas-tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](active-directory-appssoaccess-whatis.md)
* [Kullanıcı sağlamayı Yapılandır](active-directory-saas-workplacebyfacebook-provisioning-tutorial.md)



<!--Image references-->

[1]: ./media/active-directory-saas-workplacebyfacebook-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-workplacebyfacebook-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-workplacebyfacebook-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-workplacebyfacebook-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-workplacebyfacebook-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-workplacebyfacebook-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-workplacebyfacebook-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-workplacebyfacebook-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-workplacebyfacebook-tutorial/tutorial_general_203.png

