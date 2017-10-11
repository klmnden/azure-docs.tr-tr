---
title: "Öğretici: Azure Active Directory Tümleştirme ile çalışma alanına Facebook tarafından | Microsoft Docs"
description: "Çoklu oturum açma Azure Active Directory ve Facebook ile çalışma alanına arasında yapılandırmayı öğrenin."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 30f2ee64-95d3-44ef-b832-8a0a27e2967c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/18/2017
ms.author: jeedes
ms.openlocfilehash: 27e62a00832484667117d8718db9f2fc05e2f4e2
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/29/2017
---
# <a name="tutorial-azure-active-directory-integration-with-workplace-by-facebook"></a>Öğretici: Facebook ile çalışma alanına Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile çalışma alanına Facebook ile tümleştirmek öğrenin.

Çalışma alanına Facebook ile Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Facebook ile çalışma alanına erişimi olan Azure AD'de kontrol edebilirsiniz.
- Otomatik olarak çalışma alanına (çoklu oturum açma) tarafından Facebook kendi Azure AD hesaplarıyla oturum, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınızı tek bir merkezi konumda yönetebilirsiniz: Azure portal.

Azure AD ile hizmet (SaaS) uygulaması tümleştirme olarak yazılım hakkında daha fazla ayrıntı için bkz: [uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Ön koşullar

Facebook ile çalışma alanına ile Azure AD tümleştirme yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Abonelik çalışma alanına Facebook çoklu oturum açma (SSO) tarafından etkin

Bu öğreticide adımları test etmek için aşağıdaki önerileri uygulayın:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, şunları yapabilirsiniz [bir aylık deneme sürümünü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD SSO bir test ortamında test. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Facebook ile çalışma alanına Galeriden ekleyin.
2. Yapılandırma ve Azure AD çoklu oturum açmayı sınayın.

## <a name="add-workplace-by-facebook-from-the-gallery"></a>Galeriden Facebook ile çalışma alanına ekleyin
Azure AD çalışma alanına Facebook ile tümleştirilmesi yapılandırmak için çalışma alanına Facebook tarafından Galeriden yönetilen SaaS uygulamaları listenize ekleyin.

1. İçinde [Azure portal](https://portal.azure.com), sol bölmede seçin **Azure Active Directory**. 

    ![Azure Active Directory düğmesi][1]

2. Gözat **kurumsal uygulamalar** > **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
3. Yeni bir uygulama eklemek için seçin **yeni uygulama** iletişim kutusunun üst kısmında.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **Facebook ile çalışma alanına**seçip **Facebook ile çalışma alanına** sonuçlarından. Ardından **Ekle**, bir uygulama eklemek için.

    ![Sonuçlar listesinde Facebook ile çalışma](./media/active-directory-saas-facebook-at-work-tutorial/tutorial_workplacebyfacebook_search.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme
Bu bölümde, yapılandırma ve Azure AD çalışma alanına "Britta Simon." olarak adlandırılan bir test kullanıcı tabanlı Facebook tarafından SSO'su test etme

Çalışmak SSO için Azure AD ne karşılık gelen çalışma Facebook tarafından bir kullanıcı için Azure AD içinde olduğu bilmesi gerekir. Diğer bir deyişle, bir Azure AD kullanıcısının çalışma Facebook ile ilgili kullanıcı arasında bağlantılı bir ilişkisi kurulmalıdır.

Bu ilişkiyi değerini atayarak kurmak **kullanıcı adı** değeri olarak Azure AD'de **kullanıcı adı** Facebook ile çalışma.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure portalında Azure AD SSO'yu etkinleştirmek ve SSO Facebook uygulama tarafından çalışma alanınızda yapılandırın.

1. Azure portalında üzerinde **Facebook ile çalışma alanına** uygulama tümleştirmesi sayfasında, **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. İçinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** SSO'yu etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/active-directory-saas-facebook-at-work-tutorial/tutorial_workplacebyfacebook_samlbase.png)

3. İçinde **Facebook etki alanı ve URL'ler ile çalışma alanına** bölümünde, aşağıdakileri yapın:

    a. İçinde **oturum açma URL'si** metin kutusunda, aşağıdaki desen kullanan bir URL yazın:`https://<company subdomain>.facebook.com`

    b. İçinde **tanımlayıcısı** metin kutusunda, aşağıdaki desen kullanan bir URL yazın:`https://www.facebook.com/company/<scim company id>`

    > [!NOTE]
    > Bu yalnızca bir örnek değerlerdir. Bu değerleri tanımlayıcısı ve gerçek oturum açma URL'si ile güncelleştirin. Kişi [Facebook istemcisini destek ekibi ile çalışma alanına](https://workplace.fb.com/faq/) bu değerleri almak için. 

4. İçinde **SAML imzalama sertifikası** bölümünde, select **sertifika (Base64)**ve ardından sertifika dosyayı bilgisayarınıza kaydedin.

    ![Sertifika indirme bağlantısı](./media/active-directory-saas-facebook-at-work-tutorial/tutorial_workplacebyfacebook_certificate.png) 

5. **Kaydet**’i seçin.

    ![Oturum açma tek Kaydet düğmesi yapılandırın](./media/active-directory-saas-facebook-at-work-tutorial/tutorial_general_400.png)

6. İçinde **Facebook yapılandırmaya göre çalışma alanına** bölümünde, select **yapılandırma çalışma Facebook tarafından** açmak için **yapılandırma oturum açma** penceresi. Kopya **Sign-Out URL, SAML varlık kimliği ve SAML çoklu oturum açma hizmet URL'si** gelen **hızlı başvuru** bölümü.

    ![Facebook yapılandırma ile çalışma](./media/active-directory-saas-facebook-at-work-tutorial/config.png) 

7. Farklı web tarayıcısı penceresinde işyeriniz Facebook şirket site tarafından bir yönetici olarak oturum açın.
  
   > [!NOTE] 
   > SAML kimlik doğrulama işleminin bir parçası olarak, çalışma alanına sorgu dizeleri kadar 2,5 kilobayt boyutunda Azure AD'ye parametreleri geçirmek için kullanabilirsiniz.

8. İçinde **şirket Pano**gidin **kimlik doğrulaması** sekmesi.

9. Altında **SAML kimlik doğrulaması**seçin **yalnızca SSO** aşağı açılan listeden.

10. Kopyalanan değerleri girin **Facebook yapılandırmaya göre çalışma alanına** bölümüne karşılık gelen alanlara Azure portalının:

    *   İçinde **SAML URL** metin kutusunda, değerini yapıştırın **çoklu oturum açma hizmet URL'si**, Azure portalından kopyalanan.
    *   İçinde **SAML veren URL'si** metin kutusunda, değerini yapıştırın **SAML varlık kimliği**, hangi Azure portalından kopyalanır.
    *   İçinde **SAML oturum kapatma yeniden yönlendir (isteğe bağlı)**, değerini yapıştırın **Sign-Out URL**, hangi Azure portalından kopyalanır.
    *   Açık, **base-64 kodlamalı sertifika** Not Defteri'nde, Azure portalından indirdiğiniz. İçeriğini, panoya kopyalayın ve yapıştırın kendisine **SAML sertifika** metin kutusu.

11. Hedef kitle URL girmeniz gerekebilir alıcı URL'si ve ACS (onaylama tüketici hizmeti) URL'si altında listelenen **SAML Yapılandırması** bölümü.

12. Bölümün sonuna kaydırın ve seçin **Test SSO**. Açılır pencere sahip Azure AD oturum açma sayfası görüntülenir. Kimlik doğrulaması için kimlik bilgilerinizi normal olarak girin. Azure AD geri döndürülen e-posta adresi ile oturum açmış kullanıcının iş yeri hesabını aynı olduğundan emin olun.

13. Test başarıyla tamamlandı, seçin ve sayfanın alt kısmına kaydırın **kaydetmek**.

14. Çalışma alanına kullanan herkesin artık kimlik doğrulaması için Azure AD oturum açma sayfası sunulur.

SAML oturum açma kapatma sırasında Azure AD oturum kapatma sayfasını işaret etmek için kullanılan URL'si yapılandırın seçebilirsiniz. Bu ayar etkin ve yapılandırılmış olduğunda, kullanıcı artık çalışma alanına oturum kapatma sayfasına yönlendirilir. Bunun yerine, kullanıcı SAML oturum kapatma yeniden yönlendirme ayarında eklenmiş olan URL'ye yeniden yönlendirilir.


> [!TIP]
> Şimdi bu yönergeleri içinde kısa bir sürümünü okuyabilirsiniz [Azure portal](https://portal.azure.com)uygulaması kuruluyor yaparken. Bu uygulamadan ekledikten sonra **Active Directory** > **kurumsal uygulamalar** bölüm, yalnızca select **çoklu oturum açma** sekmesini tıklatın ve erişim Belge aracılığıyla katıştırılmış **yapılandırma** alt bölüm. Daha fazla bilgiyi embedded belgeler özelliği hakkında [Azure AD embedded belgeler]( https://go.microsoft.com/fwlink/?linkid=845985).

### <a name="configure-reauthentication-frequency"></a>Yeniden kimlik doğrulamanın sıklığını yapılandırma

Bir SAML denetimi için üç gün, günde bir hafta, iki hafta, bir ay istemek için çalışma alanına yapılandırabilirsiniz ya da hiç.

> [!NOTE] 
>Mobil uygulamalar üzerinde SAML denetimi için en düşük değer için bir hafta ayarlanır.

Ayrıca, tüm kullanıcılar için sıfırlama SAML zorlayabilirsiniz. Bunu yapmak için kullanın **şimdi gerektiren SAML kimlik doğrulaması tüm kullanıcılar için** düğmesi.


### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

![Azure AD Kullanıcı oluşturma][100]

1. İçinde **Azure portal**, sol bölmede seçin **Azure Active Directory**.

    ![Azure Active Directory düğmesi](./media/active-directory-saas-facebook-at-work-tutorial/create_aaduser_01.png) 

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar**seçip **tüm kullanıcılar**.
    
    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantılar](./media/active-directory-saas-facebook-at-work-tutorial/create_aaduser_02.png) 

3. Açmak için **kullanıcı** iletişim kutusunda **Ekle**.
 
    ![Ekle düğmesi](./media/active-directory-saas-facebook-at-work-tutorial/create_aaduser_03.png) 

4. İçinde **kullanıcı** iletişim kutusunda, aşağıdakileri yapın:
 
    ![Kullanıcı iletişim kutusu](./media/active-directory-saas-facebook-at-work-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusunda, **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusunda, **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola**ve not edin.

    d. **Oluştur**’u seçin.
 
### <a name="create-a-workplace-by-facebook-test-user"></a>Facebook test kullanıcı tarafından bir çalışma alanı oluşturma

Bu bölümde, Britta Simon adlı bir kullanıcı tarafından Facebook çalışma alanında oluşturulur. Çalışma alanına Facebook tarafından yalnızca zaman sağlama, varsayılan olarak etkin olduğu destekler.

Bu bölümdeki hiçbir şey yoktur. Bir kullanıcı çalışma Facebook tarafından yoksa, çalışma alanına erişmeye çalıştığında yeni bir Facebook tarafından oluşturulur.

>[!Note]
>Bir kullanıcı el ile oluşturmanız gerekiyorsa, kişi [Facebook istemcisini destek ekibi ile çalışma alanına](https://workplace.fb.com/faq/).

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Facebook ile çalışma alanına erişim vererek Azure SSO kullanılacak Britta Simon etkinleştirin.

![Kullanıcı atama][200] 

1. Azure portalında uygulamaları görünümünü açın. Dizin görünümüne gidin, Git **kurumsal uygulamalar**ve ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **Facebook ile çalışma alanına**.

    ![Uygulamalar listesinde Facebook bağlantısıyla çalışma](./media/active-directory-saas-facebook-at-work-tutorial/tutorial_workplacebyfacebook_app.png) 

3. Soldaki menüde seçin **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantı][202] 

4. **Add (Ekle)** seçeneğini belirleyin. Ardından **eklemek atama** bölmesinde, **kullanıcılar ve gruplar**.

    ![Ekleme atama bölmesi][203]

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **seçin**.

7. İçinde **eklemek atama** iletişim kutusunda **atamak**.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açmayı test edin

SSO ayarlarınızı test etmek isterseniz, erişim paneli açın.
Daha fazla bilgi için bkz. [Erişim Paneli'ne Giriş](active-directory-saas-access-panel-introduction.md).


## <a name="next-steps"></a>Sonraki adımlar

* Bkz: [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](active-directory-saas-tutorial-list.md).
* Okuma [uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](active-directory-appssoaccess-whatis.md).
* Nasıl yapılır hakkında daha fazla bilgi [kullanıcı sağlamayı Yapılandır](active-directory-saas-facebook-at-work-provisioning-tutorial.md).



<!--Image references-->

[1]: ./media/active-directory-saas-facebook-at-work-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-facebook-at-work-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-facebook-at-work-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-facebook-at-work-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-facebook-at-work-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-facebook-at-work-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-facebook-at-work-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-facebook-at-work-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-facebook-at-work-tutorial/tutorial_general_203.png

