---
title: Çoklu oturum açmayı yapılandırma - Azure Active Directory | Microsoft Docs
description: Bu öğreticide Azure portal kullanılarak bir uygulama için Azure Active Directory (Azure AD) ile SAML tabanlı çoklu oturum açma yapılandırması yapılmaktadır.
services: active-directory
author: barbkess
manager: mtillman
ms.service: active-directory
ms.component: app-mgmt
ms.topic: tutorial
ms.workload: identity
ms.date: 08/09/2018
ms.author: barbkess
ms.reviewer: arvinh,luleon
ms.openlocfilehash: b0180f162996c5fc4647071feaf02d42320b7c9a
ms.sourcegitcommit: 387d7edd387a478db181ca639db8a8e43d0d75f7
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/10/2018
ms.locfileid: "40036512"
---
# <a name="tutorial-configure-saml-based-single-sign-on-for-an-application-with-azure-active-directory"></a>Öğretici: Azure Active Directory ile bir uygulama için SAML tabanlı çoklu oturum açmayı yapılandırma

Bu öğreticide [Azure portal](https://portal.azure.com) kullanılarak bir uygulama için Azure Active Directory (Azure AD) ile SAML tabanlı çoklu oturum açma yapılandırması yapılmaktadır. Bu öğreticiyi kullanarak [uygulamaya özgü öğreticisi](../saas-apps/tutorial-list.md) olmayan uygulamaları yapılandırabilirsiniz. 


Bu öğreticide Azure portalda aşağıdaki işlemler gerçekleştirilmektedir:

> [!div class="checklist"]
> * SAML tabanlı çoklu oturum açma modunu seçme
> * Uygulamaya özgü etki alanını ve URL'leri yapılandırma
> * Kullanıcı özniteliklerini yapılandırma
> * SAML imzalama sertifikası oluşturma
> * Uygulamaya kullanıcı atama
> * Uygulamada SAML tabanlı oturum açma yapılandırması gerçekleştirme
> * SAML ayarlarını test etme

## <a name="before-you-begin"></a>Başlamadan önce

1. Uygulama Azure AD kiracınıza eklenmediyse bkz. [Hızlı başlangıç: Azure AD kiracınıza uygulama ekleme](add-application-portal.md).

2. Uygulama satıcınızdan [Etki alanını ve URL'leri yapılandırma](#configure-domain-and-urls) bölümünde belirtilen bilgileri isteyin.

3. Bu öğreticideki adımları test etmek için üretim ortamı harici bir ortam kullanmanızı öneririz. Üretim ortamı dışında bir Azure AD ortamınız yoksa [bir aylık deneme](https://azure.microsoft.com/pricing/free-trial/) aboneliği oluşturabilirsiniz.

4. [Azure portalda](https://portal.azure.com) Azure AD kiracınızın genel yönetici, bulut uygulaması yöneticisi veya uygulama yöneticisi hesabıyla oturum açın.

## <a name="select-a-single-sign-on-mode"></a>Çoklu oturum açma modunu seçme

Uygulamayı Azure AD kiracınıza ekledikten sonra çoklu oturum açma yapılandırmasını gerçekleştirebilirsiniz.

Çoklu oturum açma ayarlarını açmak için:

1. [Azure portalda](https://portal.azure.com) sol taraftaki gezinti panelinden **Azure Active Directory**’ye tıklayın. 

2. **Azure Active Directory** dikey penceresinde **Kurumsal uygulamalar**’a tıklayın. Açılan **Tüm uygulamalar** dikey penceresinde Azure AD kiracınızdaki uygulamalardan rastgele seçilmiş olanlar gösterilir. 

3. **Uygulama Türü** menüsünden **Tüm uygulamalar**'ı seçin ve **Uygula**'ya tıklayın.

4. Çoklu oturum açmayı yapılandırmak istediğiniz uygulamanın adını seçin. Uygulamanızı seçin veya [uygulama ekleme](add-application-portal.md) hızlı başlangıcında eklenen GitHub-test uygulamasını kullanın.

5. **Çoklu oturum açma**'ya tıklayın. **Çoklu Oturum Açma Modu** bölümünde varsayılan **SAML Tabanlı Oturum Açma** seçeneğini belirleyin. 

    ![Yapılandırma seçenekleri](media/configure-single-sign-on-portal/config-options.png)

6. Dikey pencerenin en üstündeki **Kaydet**'e tıklayın. 

## <a name="configure-domain-and-urls"></a>Etki alanını ve URL'leri yapılandırma

Etki alanını ve URL'leri yapılandırmak için:

1. Uygulama satıcısıyla iletişime geçerek aşağıdaki ayarlar için doğru bilgileri alın:

    | Yapılandırma ayarı | SP ile başlatılan | idP ile başlatılan | Açıklama |
    |:--|:--|:--|:--|
    | Oturum açma URL'si | Gerekli | Belirtmeyin | Kullanıcı bu URL'yi açtığında hizmet sağlayıcısı kimlik doğrulaması ve oturum açma için Azure AD'ye yönlendirir. Azure AD, URL'yi kullanarak Office 365 ve Azure AD Erişim Panelinden uygulamayı başlatır. Boş bırakıldığında, bir kullanıcı uygulamayı Office 365, Azure AD Erişim Paneli veya Azure AD çoklu oturum açma URL'sinden başlattığında Azure AD, idP ile başlatılan çoklu oturum açma işlemi gerçekleştirir.|
    | Tanımlayıcı (Varlık Kimliği) | Bazı uygulamalar için gereklidir | Bazı uygulamalar için gereklidir | Çoklu oturum açma yapılandırmasının yapıldığı uygulamayı benzersiz olarak tanımlar. Azure AD, tanımlayıcıyı uygulamaya SAML belirtecinin Audience parametresi olarak geri gönderir ve uygulamanın bunu doğrulaması beklenir. Bu değer ayrıca uygulama tarafından sağlanan SAML meta verilerinde Varlık Kimliği olarak da görünür.|
    | Yanıt URL'si | İsteğe bağlı | Gerekli | Uygulamanın SAML belirtecini almayı beklediği konumu belirtir. Yanıt URL'si, Onay Belgesi Tüketici Hizmeti (ACS) URL'si olarak da bilinir. |
    | Geçiş Durumu | İsteğe bağlı | İsteğe bağlı | Uygulamaya kimlik doğrulaması tamamlandıktan sonra kullanıcının yönlendirileceği yeri belirtir. Değer genellikle uygulama için geçerli bir URL'dir ancak bazı uygulamalar bu alanı farklı bir şekilde kullanır. Daha fazla bilgi için uygulama satıcısına danışın.

2. Bilgileri girin. Tüm ayarları görmek için **Gelişmiş URL ayarlarını göster**'e tıklayın.

    ![Yapılandırma seçenekleri](media/configure-single-sign-on-portal/config-urls.png)

3. Dikey pencerenin en üstündeki **Kaydet**'e tıklayın.

4. Bu bölümde bir **SAML Ayarlarını Test Edin** düğmesi bulunur. Bu testi öğreticinin [Çoklu oturum açma testi](#test-single-sign-on) bölümünde çalıştırın.

## <a name="configure-user-attributes"></a>Kullanıcı özniteliklerini yapılandırma

Kullanıcı öznitelikleri, Azure AD'nin uygulamaya gönderdiği bilgileri denetlemenizi sağlar. Örneğin Azure AD uygulamaya kullanıcının adını, e-posta adresini ve çalışan kimliğini gönderebilir. Azure AD, kullanıcı her oturum açtığında kullanıcı özniteliklerini uygulamaya SAML belirteci biçiminde gönderir. 

Çoklu oturum açma işlevinin düzgün çalışması için bu öznitelikler gerekli veya isteğe bağlı olabilir. Daha fazla bilgi için [uygulamaya özgü öğreticiye](../saas-apps/tutorial-list.md) bakın veya uygulama satıcısına sorun.

1. Tüm seçenekleri görüntülemek için **Diğer tüm kullanıcı özniteliklerini görüntüleyin ve düzenleyin**'e tıklayın.

    ![Kullanıcı özniteliklerini yapılandırma](media/configure-single-sign-on-portal/config-user-attributes.png)

2. **Kullanıcı Tanımlayıcısı**'nı girin.

    Kullanıcı tanımlayıcısı, uygulama içinde her kullanıcıyı benzersiz olarak tanımlar. Örneğin e-posta adresi hem kullanıcı adı hem de benzersiz tanıtıcı olarak kullanılıyorsa değeri *user.mail* olarak ayarlayın.

3. Daha fazla SAML belirteci özniteliği için **Diğer tüm kullanıcı özniteliklerini görüntüleyin ve düzenleyin**'e tıklayın.

4. **SAML Belirteci Öznitelikleri**'ne öznitelik eklemek için **Öznitelik ekle**'ye tıklayın. **Ad** belirtin ve menüden **Değer** seçin.

5. **Kaydet**’e tıklayın. Yeni özniteliği tabloda görürsünüz.
 
## <a name="create-a-saml-signing-certificate"></a>SAML imzalama sertifikası oluşturma

Azure AD, uygulamaya gönderdiği SAML belirteçlerini imzalamak için bir sertifika kullanır. 

1. Tüm seçenekleri görmek için **Gelişmiş sertifika imzalama seçeneklerini göster**'e tıklayın.

    ![Sertifikaları yapılandırma](media/configure-single-sign-on-portal/config-certificate.png)

2. Sertifika yapılandırmak için **Yeni sertifika oluştur**'a tıklayın.

3. **Yeni Sertifika Oluştur** dikey penceresinde sona erme tarihini belirleyin ve **Kaydet**'e tıklayın.

4. **Yeni sertifikayı etkinleştir**'e tıklayın.

5. Daha fazla bilgi için bkz. [Gelişmiş sertifika imzalama seçenekleri](certificate-signing-options.md).

6. Şu ana kadar yaptığınız değişiklikleri saklamak için **Çoklu oturum açma** dikey penceresinin en üstündeki **Kaydet**'e tıklamayı unutmayın. 

## <a name="assign-users-to-the-application"></a>Uygulamaya kullanıcı atama

Microsoft, uygulamayı kuruluşunuzda kullanıma sunmadan önce çoklu oturum açmayı birden fazla kullanıcı veya grupla test etmenizi önerir.

Uygulamaya kullanıcı veya grup atamak için:

1. Açık değilse portaldan uygulamayı açın.
2. Sol uygulama dikey penceresinde **Kullanıcılar ve gruplar**'a tıklayın.
3. **Kullanıcı ekle**'ye tıklayın.
4. **Atama Ekle** dikey penceresinde **Kullanıcılar ve gruplar**'a tıklayın.
5. Belirli bir kullanıcıyı bulmak için kullanıcının adını **Seç** kutusuna yazın, kullanıcının profil fotoğrafının veya logosunun yanındaki onay kutusunu işaretleyin ve **Seç**'e tıklayın. 
6. Geçerli kullanıcı adınızı bulup seçin. İsterseniz daha fazla kullanıcı seçebilirsiniz.
7. **Atama Ekle** dikey penceresinde **Ata**'ya tıklayın. İşlem tamamlandığında seçilen kullanıcılar, **Kullanıcılar ve gruplar** listesinde görünür.

## <a name="configure-the-application-to-use-azure-ad"></a>Uygulamayı Azure AD kullanacak şekilde yapılandırma

Neredeyse bitti.  Son adım olarak uygulamayı Azure AD'yi SAML kimliği sağlayıcısı olarak kullanacak şekilde yapılandırmanız gerekir. 

1. Uygulamanızın **Çoklu oturum açma** dikey penceresinin en altına kaydırın. 

    ![Uygulama yapılandırma](media/configure-single-sign-on-portal/configure-app.png)

2. Portalda **Uygulamayı yapılandır**'a tıklayın ve yönergeleri izleyin.
3. Çoklu oturum açma testi için uygulamada kullanıcı adlarını el ile oluşturun. [Önceki bölümde](#assign-users-to-the-application) uygulamaya atadığınız kullanıcı hesaplarını oluşturun.   Uygulamayı kuruluşta kullanıma sunmaya hazır olduğunuzda kullanıcı hesaplarını uygulamada otomatik olarak oluşturmak için otomatik kullanıcı sağlama özelliğini kullanmanızı öneririz.

## <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Ayarlarınızı test etmeye hazırsınız.  

1. Uygulamanızın çoklu oturum açma ayarlarını açın. 
2. **Etki alanını ve URL'leri yapılandır** bölümüne gidin.
2. **SAML Ayarlarını Test Edin**'e tıklayın. Test seçenekleri açılır.

    ![Çoklu oturum açma testi seçenekleri](media/configure-single-sign-on-portal/test-single-sign-on.png) 

3. **Geçerli kullanıcı olarak oturum aç**'a tıklayın. Bu işlev çoklu oturum açmanın yönetici kullanıcınızda çalışıp çalışmadığını kontrol etmenizi sağlar.
4. Hata olursa bir hata iletisi görüntülenir. Hatayla ilgili bilgileri kopyalayıp **Hata neye benziyor?** kutusuna yapıştırın.

    ![Çözüm rehberliği alın](media/configure-single-sign-on-portal/error-guidance.png)

5. **Çözüm rehberliği alın**'a tıklayın. Kök neden ve çözüm kılavuzu görüntülenir.  Bu örnekte kullanıcı, uygulamaya atanmamıştır.

    ![Hatayı düzeltme](media/configure-single-sign-on-portal/fix-error.png)

6. Çözüm kılavuzunu okuyun ve gerekirse **Düzelt**'e tıklayın.

7. Testi başarıyla tamamlana kadar tekrarlayın.



## <a name="next-steps"></a>Sonraki adımlar
Bu öğreticide Azure portalı kullanarak bir uygulamada Azure AD ile çoklu oturum açma yapılandırması gerçekleştirdiniz. Çoklu oturum açma yapılandırma sayfasını buldunuz ve çoklu oturum açma ayarlarını yapılandırdınız. Yapılandırmayı tamamladıktan sonra uygulamaya kullanıcı atadınız ve uygulamayı SAML tabanlı çoklu oturum açmayı kullanacak şekilde yapılandırdınız. Bu işlerin tümünü tamamladığınızda SAML oturum açma işlevinin düzgün çalıştığını doğruladınız.

Şu işlemleri yaptınız:
> [!div class="checklist"]
> * Çoklu oturum açma modu olarak SAML seçtiniz
> * Etki alanını ve URL'leri yapılandırmak için uygulama satıcısıyla iletişime geçtiniz
> * Kullanıcı özniteliklerini yapılandırdınız
> * SAML imzalama sertifikası oluşturdunuz
> * Uygulamaya el ile kullanıcı veya grup atadınız
> * Uygulamada oturum açma yapılandırması gerçekleştirdiniz
> * SAML tabanlı çoklu oturum açmayı test ettiniz

Uygulamayı kuruluşunuzda daha fazla kullanıcıya sunmak için otomatik sağlamayı kullanmanızı öneririz.

> [!div class="nextstepaction"]
>[Otomatik sağlama ile kullanıcı atamayı öğrenin](configure-automatic-user-provisioning-portal.md)


