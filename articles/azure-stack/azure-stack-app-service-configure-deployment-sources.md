---
title: "Dağıtım kaynakları Azure yığın uygulama hizmetleri için yapılandırma | Microsoft Docs"
description: "Hizmet Yöneticisi Azure yığın uygulama hizmeti için dağıtım kaynakları (Git, GitHub, BitBucket, DropBox ve OneDrive) nasıl yapılandırabileceğiniz"
services: azure-stack
documentationcenter: 
author: apwestgarth
manager: stefsch
editor: 
ms.assetid: 
ms.service: azure-stack
ms.workload: app-service
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/10/2017
ms.author: anwestg
ms.openlocfilehash: dc341d872a3b8943a934217ace21537f45bafd10
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/08/2017
---
# <a name="configure-deployment-sources"></a>Dağıtım kaynaklarını yapılandırma

Azure yığın uygulama hizmeti birden çok kaynak denetim sağlayıcılarından isteğe bağlı dağıtımını destekler. Bu özellik uygulama geliştiricileri kendi kaynak denetimi havuzların doğrudan dağıtmanıza olanak sağlar. Kullanıcılar kendi depoları bağlanmak için uygulama hizmeti yapılandırmak istiyorsanız, bir bulut işleci öncelikle Azure yığın uygulama hizmeti ve kaynak denetimi sağlayıcısı arasında tümleştirme yapılandırmanız gerekir.  

Kaynak denetimi sağlayıcısı, yerel Git yanı sıra, desteklenen şunlardır:

* GitHub
* BitBucket
* OneDrive
* Açılan kutu

## <a name="view-deployment-sources-in-app-service-administration"></a>Uygulama Hizmeti Yönetim görünümü dağıtım kaynakları

1. Azure yığın Yönetim Portalı'na (https://adminportal.local.azurestack.external) Hizmet Yöneticisi olarak oturum açın.
2. Gözat **kaynak sağlayıcıları** seçip **uygulama hizmeti kaynak sağlayıcısı yönetici**.  ![Uygulama hizmeti kaynak sağlayıcısı yönetici][1]
3. Tıklatın **kaynak denetimini yapılandırma**.  Burada yapılandırılan tüm dağıtım kaynaklar listesine bakın.
    ![Uygulama hizmeti kaynak sağlayıcısı yönetici kaynak denetimini yapılandırma][2]

## <a name="configure-github"></a>GitHub yapılandırın

Bu görevi tamamlamak için GitHub hesabı olması gerekir. Kişisel hesabı yerine, kuruluşunuz için bir hesap kullanmak isteyebilirsiniz.

1. GitHub için oturum açma, https://www.github.com/settings/developers için göz atın ve tıklayın **yeni uygulamayı Kaydet**.
    ![GitHub - kayıt yeni bir uygulama][3]
2. Girin bir **uygulama adı** örneğin - Azure yığın uygulama hizmeti.
3. Girin **giriş sayfası URL'si**. Giriş sayfası URL'si Azure yığın Portal adresi olmalıdır. Örneğin, https://portal.local.azurestack.external.
4. Girin bir **uygulama açıklaması**.
5. Girin **yetkilendirme geri çağırma URL'si**.  Etki alanınız azurestack.local için farklı bir etki alanı yerine altında çalıştırıyorsanız, varsayılan Azure yığın dağıtımında, form https://portal.local.azurestack.external/tokenauthorize içinde URL'dir.
    ![GitHub - kayıt doldurulmuş değerlerle yeni bir uygulama][4]
6. Tıklatın **kaydetmek uygulama**.  Artık bir sayfa listeyle sunulur **istemci kimliği** ve **gizli** uygulama için.
    ![GitHub - tamamlanan uygulama kaydı][5]
7.  Yeni bir tarayıcı sekmesinde veya penceresinde Azure yığın yönetim portalı (https://adminportal.local.azurestack.external) Hizmet Yöneticisi olarak oturum açın.
8.  Gözat **kaynak sağlayıcıları** seçip **uygulama hizmeti kaynak sağlayıcısı yönetici**.
9. Tıklatın **kaynak denetimini yapılandırma**.
10. Kopyalama ve yapıştırma **istemci kimliği** ve **gizli** karşılık gelen bir giriş için GitHub kutuları.
11. **Kaydet** düğmesine tıklayın.

## <a name="configure-bitbucket"></a>BitBucket yapılandırın

Bu görevi tamamlamak için bir BitBucket hesabınızın olması gerekir. Kişisel hesabı yerine, kuruluşunuz için bir hesap kullanmak isteyebilirsiniz.

1. BitBucket için oturum açın ve gidin **tümleştirmeler** hesabınızın altında.
    ![BitBucket Pano - tümleştirmeler][7]
2. Tıklatın **OAuth** erişim yönetimi altında ve **Ekle tüketici**.
    ![BitBucket OAuth tüketicisi ekleyin][8]
3. Girin bir **adı** tüketici, örneğin Azure yığın uygulama hizmeti için.
4. Girin bir **açıklama** uygulama için.
5. Girin **geri çağırma URL'si**.  Etki alanınız azurestack.local için farklı bir etki alanı yerine altında çalıştırıyorsanız, varsayılan Azure yığın dağıtımında, geri çağırma form https://portal.local.azurestack.external/TokenAuthorize içinde URL'dir.  Url, BitBucket tümleştirme başarılı olması burada listelenen gibi büyük harfe izlemeniz gerekir.
6. Girin **URL** -bu URL'nin Azure yığın portalı URL'si, örneğin https://portal.local.azurestack.external olması gerekir.
7. Seçin **izinleri** gerekir:
    - **Depoları**: *okuma*
    - **Web kancası**: *okuma ve yazma*
8. **Kaydet** düğmesine tıklayın.  Şimdi bu yeni uygulama ile birlikte göreceğiniz **anahtar** ve **gizli** altında **OAuth tüketicileri**.
    ![BitBucket uygulama listeleme][9]
9.  Yeni bir tarayıcı sekmesinde veya penceresinde Azure yığın yönetim portalı (https://adminportal.local.azurestack.external) Hizmet Yöneticisi olarak oturum açın.
10.  Gözat **kaynak sağlayıcıları** seçip **uygulama hizmeti kaynak sağlayıcısı yönetici**.
11. Tıklatın **kaynak denetimini yapılandırma**.
12. Kopyalama ve yapıştırma **anahtar** içine **istemci kimliği** giriş kutusu ve **gizli** içine **gizli** BitBucket için giriş kutusu.
13. **Kaydet** düğmesine tıklayın.


## <a name="configure-onedrive"></a>OneDrive yapılandırın

Bu görevi tamamlamak için Microsoft OneDrive hesabınıza bağlı Account olması gerekir.  Kişisel hesabı yerine, kuruluşunuz için bir hesap kullanmak isteyebilirsiniz.

> [!NOTE]
> OneDrive iş hesapları için şu anda desteklenmemektedir.

1. Microsoft Account kullanarak göz atın https://apps.dev.microsoft.com/?referrer=https%3A%2F%2Fdev.onedrive.com%2Fapp-registration.htm ve günlük.
2. Altında **uygulamalarım**, tıklatın **bir uygulama ekleyin**.
![OneDrive uygulamalar][10]
3. Girin bir **adı** yeni uygulama kaydı için girin **Azure yığın uygulama hizmeti**, tıklatıp **uygulaması oluştur**
4. Sonraki ekranda, yeni uygulamanızı özelliklerini listeler. Kayıt **uygulama kimliği**. ![OneDrive uygulama özellikleri][11]
5. Altında **uygulama parolaları**, tıklatın **yeni bir parola oluşturmak**. Not **oluşturulan yeni bir parola**. Bu, uygulama gizli anahtarı ve tıklattıktan sonra alınabilir değil **Tamam** bu aşamada.
6. Altında **platformları** tıklatın **eklemek Platform** seçip **Web**.
7. Girin **yeniden yönlendirme URI'si**.  Etki alanınız azurestack.local için farklı bir etki alanı yerine altında çalışan bir varsayılan Azure yığın dağıtımda, yeniden yönlendirme URI'si form https://portal.local.azurestack.external/tokenauthorize içinde ise ![OneDrive uygulama - Ekle Web Platformu][12]
8. Ekleme **Microsoft Graph izinleri** - **izinleri için temsilci**
    - **Files.ReadWrite.AppFolder**
    - **User.Read**  
      ![OneDrive uygulaması - grafik izinleri][13]
9. **Kaydet** düğmesine tıklayın.
10.  Yeni bir tarayıcı sekmesinde veya penceresinde Azure yığın yönetim portalı (https://adminportal.local.azurestack.external) Hizmet Yöneticisi olarak oturum açın.
11.  Gözat **kaynak sağlayıcıları** seçip **uygulama hizmeti kaynak sağlayıcısı yönetici**.
12. Tıklatın **kaynak denetimini yapılandırma**.
13. Kopyalama ve yapıştırma **uygulama kimliği** içine **istemci kimliği** giriş kutusu ve **parola** içine **gizli** OneDrive için giriş kutusu.
14. **Kaydet** düğmesine tıklayın.

## <a name="configure-dropbox"></a>DropBox yapılandırın

> [!NOTE]
> Bu görevi tamamlamak için DropBox hesabınızın olması gerekir.  Kişisel hesabı yerine, kuruluşunuz için bir hesap kullanmak isteyebilirsiniz.

1. DropBox hesabı kullanarak göz atın https://www.dropbox.com/developers/apps ve günlük.
2. Tıklatın **uygulaması oluşturma**.

    ![Dropbox uygulamalar][14]

3. Seçin **DropBox API**.
4. Erişim düzeyini ayarlamak **uygulama klasörü**.
5. Girin bir **adı** uygulamanız için.
![Dropbox uygulama kaydı][15]
6. Tıklatın **uygulaması oluşturma**.  Artık uygulama da dahil olmak üzere ayarları listelendiği bir sayfa görürsünüz **uygulama anahtarı** ve **uygulama gizli anahtarı**.
7. Denetleme **uygulama klasör adı** ayarlanır **Azure yığın uygulama hizmeti**.
8. Ayarlama **OAuth 2 yeniden yönlendirme URI'si** tıklatıp **Ekle**.  Etki alanınız azurestack.local için farklı bir etki alanı yerine altında çalışan bir varsayılan Azure yığın dağıtımda, yeniden yönlendirme URI'si form https://portal.local.azurestack.external/tokenauthorize içinde ise.
![Dropbox uygulama yapılandırması][16]
9.  Yeni bir tarayıcı sekmesinde veya penceresinde Azure yığın yönetim portalı (https://adminportal.local.azurestack.external) Hizmet Yöneticisi olarak oturum açın.
10.  Gözat **kaynak sağlayıcıları** seçip **uygulama hizmeti kaynak sağlayıcısı yönetici**.
11. Tıklatın **kaynak denetimini yapılandırma**.
12. Kopyalama ve yapıştırma **uygulama anahtarı** içine **istemci kimliği** giriş kutusu ve **uygulama gizli anahtarı** içine **gizli** dropbox giriş kutusu.
13. **Kaydet** düğmesine tıklayın.


<!--Image references-->
[1]: ./media/azure-stack-app-service-configure-deployment-sources/App-service-provider-admin.png
[2]: ./media/azure-stack-app-service-configure-deployment-sources/App-service-provider-admin-source-control-configuration.png
[3]: ./media/azure-stack-app-service-configure-deployment-sources/App-service-provider-admin-github-developer-applications.png
[4]: ./media/azure-stack-app-service-configure-deployment-sources/App-service-provider-admin-github-register-a-new-oauth-application-populated.png
[5]: ./media/azure-stack-app-service-configure-deployment-sources/App-service-provider-admin-github-register-a-new-oauth-application-complete.png
[6]: ./media/azure-stack-app-service-configure-deployment-sources/App-service-provider-admin-roles-management-server-repair-all.png
[7]: ./media/azure-stack-app-service-configure-deployment-sources/App-service-provider-admin-bitbucket-dashboard.png
[8]: ./media/azure-stack-app-service-configure-deployment-sources/App-service-provider-admin-bitbucket-access-management-add-oauth-consumer.png
[9]: ./media/azure-stack-app-service-configure-deployment-sources/App-service-provider-admin-bitbucket-access-management-add-oauth-consumer-complete.png
[10]: ./media/azure-stack-app-service-configure-deployment-sources/App-service-provider-admin-Onedrive-applications.png
[11]: ./media/azure-stack-app-service-configure-deployment-sources/App-service-provider-admin-Onedrive-application-registration.png
[12]: ./media/azure-stack-app-service-configure-deployment-sources/App-service-provider-admin-Onedrive-application-platform.png
[13]: ./media/azure-stack-app-service-configure-deployment-sources/App-service-provider-admin-Onedrive-application-graph-permissions.png
[14]: ./media/azure-stack-app-service-configure-deployment-sources/App-service-provider-admin-Dropbox-applications.png
[15]: ./media/azure-stack-app-service-configure-deployment-sources/App-service-provider-admin-Dropbox-application-registration.png
[16]: ./media/azure-stack-app-service-configure-deployment-sources/App-service-provider-admin-Dropbox-application-configuration.png

## <a name="next-steps"></a>Sonraki adımlar

Kullanıcılar artık kullanabilir dağıtım kaynakları şeyler ister için [sürekli dağıtım](https://docs.microsoft.com/azure/app-service-web/app-service-continuous-deployment), [yerel Git dağıtımı](https://docs.microsoft.com/azure/app-service-web/app-service-deploy-local-git), ve [bulut klasör eşitleme](https://docs.microsoft.com/azure/app-service-web/app-service-deploy-content-sync).
