<properties
   pageTitle="Azure'da Office 365 aboneliğinize yönelik dizini yönetme | Microsoft Azure"
   description="Azure Active Directory'yi ve klasik Azure portalını kullanarak bir Office 365 aboneliği dizinini yönetme"
   services="active-directory"
   documentationCenter=""
   authors="curtand"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="08/23/2016"
   ms.author="curtand"/>

# Azure'da Office 365 aboneliğinize yönelik dizini yönetme

Bu makalede klasik Azure portalı kullanılarak bir Office 365 aboneliği için oluşturulmuş bir dizinin nasıl yönetileceği açıklanmaktadır. Klasik Azure portalında oturum açmak için Hizmet Yöneticisi veya bir Azure aboneliğinin ortak yöneticisi olmanız gerekir. Henüz bir Azure aboneliğiniz yoksa hemen [30 günlük ücretsiz deneme sürümüne](https://azure.microsoft.com/trial/get-started-active-directory/) kaydolabilir ve bu bağlantıyı kullanarak ilk bulut çözümünüzü 5 dakikadan kısa bir sürede dağıtabilirsiniz. Office 365'te oturum açmak için kullandığınız iş veya okul hesabını kullandığınızdan emin olun.

Azure aboneliğini tamamladığınızda, klasik Azure portalında oturum açabilir ve Azure hizmetlerine erişebilirsiniz. Office 365 kullanıcılarınızın kimliğini doğrulayan dizini yönetmek için Active Directory uzantısına tıklayın.

Zaten bir Azure aboneliğiniz varsa ek dizin yönetme işlemi de son derece basittir. Örneğin, Michael Smith, Contoso.com için bir Office 365 aboneliğine sahip olabilir. Ayrıca msmith@hotmail.com şeklindeki Microsoft hesabını kullanarak kaydolduğu bir Azure aboneliği de mevcuttur. Bu durumda, iki dizini yönetir.

  Abonelik |  Office 365  |  Azure
  -------------- | ------------- | -------------------------------
  Görünen ad |  Contoso  |     Varsayılan Azure Active Directory (Azure AD) dizini
  Etki alanı adı  |  contoso.com  | msmithhotmail.onmicrosoft.com

Çok faktörlü kimlik doğrulaması gibi Azure AD özelliklerini etkinleştirebilmek için, Microsoft hesabıyla Azure'da oturum açmış durumdayken Contoso dizindeki kullanıcı kimliklerini yönetmek istiyor. Aşağıdaki diyagram, işlemin açıklanmasına yardımcı olabilir.

![İki bağımsız dizini yönetmeye yönelik diyagram](./media/active-directory-manage-o365-subscription/AAD_O365_03.png)

Bu durumda, iki dizin birbirinden bağımsızdır.

## İki bağımsız dizini yönetmek için
Michael Smith'in Azure'da msmith@hotmail.com olarak oturum açmış durumdayken her iki dizini de yönetebilmek için aşağıdaki adımları tamamlaması gerekir:

> [AZURE.NOTE]
> Bu adımlar, yalnızca kullanıcı bir Microsoft hesabıyla oturum açmış durumdayken tamamlanabilir. Kullanıcı bir iş veya okul hesabıyla kaydolursa **Var olan dizini kullan** seçeneği mevcut olmaz. Bir iş veya okul hesabının kimlik doğrulaması yalnızca kendi giriş dizini (diğer bir deyişle, iş veya okul hesabının depolandığı ve işletme ya da okulun sahip olduğu dizin) ile yapılabilir.

1.  [Klasik Azure portalı](https://manage.windowsazure.com) üzerinde msmith@hotmail.com olarak oturum açın.
2.  **New (Yeni)** > **App services (Uygulama hizmetleri)** > **Active Directory** > **Directory (Dizin)** > **Custom Create (Özel Oluştur)** düğmesine tıklayın.
3.  Use existing directory (Var olan dizini kullan) seçeneğine tıklayın ve **I am ready to be signed out now (Şimdi oturumumun kapatılması için hazırım)** seçeneğini işaretleyin.
4.  Klasik Azure portalında Contoso.onmicrosoft.com adresinin genel yöneticisi (örneğin, msmith@contoso.com) olarak oturum açın.
5.  **Use the Contoso directory with Azure? (Contoso dizini Azure ile kullanılsın mı?)** sorusu sorulduğunda, **Continue (Devam)** seçeneğine tıklayın.
6.  **Sign out now (Şimdi oturumu kapat)** seçeneğine tıklayın.
7.  Klasik Azure portalı üzerinde msmith@hotmail.com olarak oturum açın. Contoso dizini ve Varsayılan dizin Active Directory uzantısında görünür.

Bu adımları tamamladıktan sonra msmith@hotmail.com Contoso dizininde genel yönetici olacaktır.

## Kaynakları genel yönetici olarak yönetmek için
Şimdi Jane Doe'nun msmith@hotmail.com için Azure aboneliği ile ilişkili olan web sitelerini ve veritabanı kaynaklarını yönetmesi gerektiğini varsayalım. Bunu yapabilmesi için ilk olarak Michael Smith'in şu ek adımları tamamlaması gerekir:

1.  [Klasik Azure portalı](https://manage.windowsazure.com) üzerinde Azure aboneliğine yönelik Hizmet Yöneticisi hesabını (bu örnekte msmith@hotmail.com) kullanarak oturum açın.
2.  Aboneliği Contoso dizinine aktarma: **Settings (Ayarlar)** > **Subscriptions (Abonelikler)** düğmesine tıklayın > aboneliği seçin > **Edit Directory (Dizini Düzenle)** seçeneğini belirleyin > **Contoso'yu (Contoso.com) seçin**. Aktarma işleminin bir parçası olarak, aboneliğin ortak yöneticisi olan tüm iş veya okul hesapları kaldırılır.
3.  Jane Doe'yu aboneliğin ortak yöneticisi olarak ekleme: **Settings (Ayarlar)** > **Administrators (Yöneticiler)** seçeneğine tıklayın > aboneliği seçin > **Add (Ekle)** seçeneğini belirleyin > **JohnDoe@Contoso.com** yazın.

## Sonraki adımlar
Abonelikler ve dizinler arasındaki ilişki hakkında daha fazla bilgi için bkz. [Bir aboneliğin bir dizinle olan ilişkisi nedir?](active-directory-how-subscriptions-associated-directory.md).



<!--HONumber=Aug16_HO4-->


