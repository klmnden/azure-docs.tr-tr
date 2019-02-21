---
title: Azure Stack'te uygulama hizmetleri için dağıtım kaynaklarını yapılandırma | Microsoft Docs
description: Hizmet Yöneticisi Azure Stack'te uygulama hizmetleri için dağıtım kaynakları (Git, GitHub, BitBucket, DropBox ve OneDrive) nasıl yapılandıracağınız
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: app-service
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/15/2018
ms.author: jeffgilb
ms.reviewer: anwestg
ms.lastreviewed: 10/15/2018
ms.openlocfilehash: f2101c685ff7b3820f826da1d2e1d52b687d26c6
ms.sourcegitcommit: 6cab3c44aaccbcc86ed5a2011761fa52aa5ee5fa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/20/2019
ms.locfileid: "56446640"
---
# <a name="configure-deployment-sources"></a>Dağıtım kaynaklarını yapılandırma

*Uygulama hedefi: Azure Stack tümleşik sistemleri ve Azure Stack Geliştirme Seti*

Azure Stack'te App Service, isteğe bağlı dağıtım birden çok kaynak denetim sağlayıcılarından destekler. Bu özellik, uygulama geliştiricilerin kendi kaynak denetim depolarından doğrudan dağıtma olanak sağlar. Kullanıcılar App Service'ı için depolardaki bağlanmak için yapılandırmak istiyorsanız, bir bulut işleci Azure Stack'te App Service ve kaynak denetimi sağlayıcısı arasındaki tümleştirmeden önce yapılandırmanız gerekir.  

Yerel Git ek olarak, aşağıdaki kaynak denetimi sağlayıcıları desteklenir:

* GitHub
* BitBucket
* OneDrive
* DropBox

## <a name="view-deployment-sources-in-app-service-administration"></a>App Service yönetim dağıtım kaynakları görüntüle

1. Azure Stack Yönetici portalında oturum açın (https://adminportal.local.azurestack.external) Hizmet Yöneticisi olarak.
2. Gözat **tüm hizmetleri** seçip **App Service**.
    ![App Service kaynak sağlayıcısı yönetici][1]
3. Tıklayın **kaynak denetimi Yapılandırması**. Tüm yapılandırılan dağıtım kaynakları listesini görebilirsiniz.
    ![App Service kaynak sağlayıcısı yönetici kaynak denetimi yapılandırması][2]

## <a name="configure-github"></a>GitHub'ı yapılandırma

Bu görevi tamamlamak için bir GitHub hesabınızın olması gerekir. Kişisel hesap yerine kuruluşunuz için bir hesap kullanmak isteyebilirsiniz.

1. Github'da oturum açın, Gözat https://www.github.com/settings/developersve ardından **yeni bir uygulamayı kaydetme**.
    ![GitHub - yeni uygulama kaydı][3]
2. Girin bir **uygulama adı**; Örneğin, **Azure Stack üzerinde App Service'te**.
3. Girin **giriş sayfası URL'si**. Giriş sayfası URL'si, Azure Stack portal adresi olmalıdır. Örneğin, https://portal.local.azurestack.external.
4. Girin bir **uygulama açıklaması**.
5. Girin **yetkilendirme geri çağırma URL'si**. Varsayılan bir Azure Stack dağıtımı biçiminde URL'dir https://portal.local.azurestack.external/TokenAuthorize. Farklı bir etki alanı altında çalışıyorsa, local.azurestack.external için etki alanı adınızla değiştirin.
6. Tıklayın **kaydetme uygulama**. Bir sayfa listesi görüntülenir **istemci kimliği** ve **gizli** uygulama için.
    ![GitHub - tamamlanan uygulama kaydı][5]
7.  Yeni bir tarayıcı sekmesinde veya penceresinde, Azure Stack Yönetici portalında oturum açın (https://adminportal.local.azurestack.external) Hizmet Yöneticisi olarak.
8.  Gözat **kaynak sağlayıcıları**seçip **App Service kaynak sağlayıcısı yönetici**.
9. Tıklayın **kaynak denetimi Yapılandırması**.
10. Kopyalama ve yapıştırma **istemci kimliği** ve **gizli** kutuları için GitHub karşılık gelen bir giriş ekleyin.
11. **Kaydet**’e tıklayın.

## <a name="configure-bitbucket"></a>BitBucket yapılandırın

Bu görevi tamamlamak için bir BitBucket hesabına ihtiyacınız vardır. Kişisel hesap yerine kuruluşunuz için bir hesap kullanmak isteyebilirsiniz.

1. BitBucket için oturum açın ve gidin **tümleştirmeler** hesabınız kapsamında.
    ![BitBucket Pano - tümleştirmeleri][7]
2. Tıklayın **OAuth** erişim yönetimi altında ve **Ekle tüketici**.
    ![BitBucket OAuth tüketicisi ekleyin][8]
3. Girin bir **adı** için tüketici; Örneğin, **Azure Stack üzerinde App Service'te**.
4. Girin bir **açıklama** uygulama için.
5. Girin **geri çağırma URL'si**. Varsayılan bir Azure Stack dağıtımı geri çağırma URL'si biçimindedir https://portal.local.azurestack.external/TokenAuthorize. Farklı bir etki alanı altında çalışıyorsa, azurestack.local için etki alanı adınızla değiştirin. BitBucket tümleştirme başarılı olması, URL'sini burada listelenen büyük/küçük harf izlemeniz gerekir.
6. Girin **URL**. Bu URL, Azure Stack portalı olmalıdır; URL Örneğin, https://portal.local.azurestack.external.
7. Seçin **izinleri** gerekli:
    - **Depoları**: *Okuma*
    - **Web kancaları**: *Okuma ve yazma*
8. **Kaydet**’e tıklayın. İle birlikte bu yeni uygulama artık bkz **anahtarı** ve **gizli**altında **OAuth tüketiciler**.
    ![BitBucket uygulama listesi][9]
9.  Yeni bir tarayıcı sekmesinde veya penceresinde, Azure Stack Yönetici portalında oturum açın (https://adminportal.local.azurestack.external) Hizmet Yöneticisi olarak.
10.  Gözat **kaynak sağlayıcıları** seçip **App Service kaynak sağlayıcısı yönetici**.
11. Tıklayın **kaynak denetimi Yapılandırması**.
12. Kopyalama ve yapıştırma **anahtarı** içine **istemci kimliği** giriş kutusu ve **gizli** içine **gizli** BitBucket için giriş kutusu.
13. **Kaydet**’e tıklayın.

## <a name="configure-onedrive"></a>OneDrive'ı yapılandırma

Bu görevi tamamlamak için Microsoft OneDrive hesabınıza bağlı Account olması gerekir.  Kişisel hesap yerine kuruluşunuz için bir hesap kullanmak isteyebilirsiniz.

> [!NOTE]
> OneDrive iş hesapları için henüz desteklenmemektedir.

1. Gözat https://apps.dev.microsoft.com/?referrer=https%3A%2F%2Fdev.onedrive.com%2Fapp-registration.htm Microsoft Account kullanarak oturum açın.
2. Altında **uygulamalarım**, tıklayın **uygulama ekleme**.
![OneDrive uygulamaları][10]
3. Girin bir **adı** yeni uygulama kaydı için: girin **Azure Stack üzerinde App Service'te**ve ardından **uygulama oluşturma**
4. Sonraki ekranda yeni uygulamanızın özelliklerini listeler. Kaydet **uygulama kimliği** geçici bir konuma.
![OneDrive uygulaması özellikleri][11]
5. Altında **uygulama gizli dizilerini**, tıklayın **yeni parola oluştur**. Not **yeni parola oluşturuldu**. Bu uygulama gizli anahtarı ve tıkladığınızda alınabilir değil **Tamam**.
6. Altında **platformları**, tıklayın **Platform Ekle**ve ardından **Web**.
7. Girin **yeniden yönlendirme URI'si**. Varsayılan bir Azure Stack dağıtımı yeniden yönlendirme URI'si biçimindedir https://portal.local.azurestack.external/TokenAuthorize. Farklı bir etki alanı altında çalışıyorsa, azurestack.local için etki alanı adınızla değiştirin.
![OneDrive uygulaması - Web Platformu Ekle][12]
8. Ekleme **Microsoft Graph izinleri** - **temsilci izinleri**
    - **Files.ReadWrite.AppFolder**
    - **User.Read**  
      ![OneDrive uygulaması - Graph izinleri][13]
9. **Kaydet**’e tıklayın.
10.  Yeni bir tarayıcı sekmesinde veya penceresinde, Azure Stack Yönetici portalında oturum açın (https://adminportal.local.azurestack.external) Hizmet Yöneticisi olarak.
11.  Gözat **kaynak sağlayıcıları** seçip **App Service kaynak sağlayıcısı yönetici**.
12. Tıklayın **kaynak denetimi Yapılandırması**.
13. Kopyalama ve yapıştırma **uygulama kimliği** içine **istemci kimliği** giriş kutusu ve **parola** içine **gizli** OneDrive için giriş kutusu.
14. **Kaydet**’e tıklayın.

## <a name="configure-dropbox"></a>DropBox'ı yapılandırma

> [!NOTE]
> Bu görevi tamamlamak için bir DropBox hesabı olması gerekir. Kişisel hesap yerine kuruluşunuz için bir hesap kullanmak isteyebilirsiniz.

1. Gözat https://www.dropbox.com/developers/apps DropBox hesabı kimlik bilgilerinizi kullanarak oturum açın.
2. Tıklayın **uygulaması oluşturma**.

    ![Dropbox uygulamalar][14]

3. Seçin **DropBox API**.
4. Erişim düzeyini ayarlayın **uygulama klasörü**.
5. Girin bir **adı** uygulamanız için.
![Dropbox uygulama kaydı][15]
6. Tıklayın **uygulaması oluşturma**. Uygulama ayarlarını listelendiği bir sayfa ile sunulan dahil olmak üzere **uygulama anahtarı** ve **uygulama gizli anahtarı**.
7. Emin olun **uygulama klasörü adı** ayarlanır **Azure Stack üzerinde App Service'te**.
8. Ayarlama **OAuth 2 yeniden yönlendirme URI'si** ve ardından **Ekle**. Varsayılan bir Azure Stack dağıtımı yeniden yönlendirme URI'si biçimindedir https://portal.local.azurestack.external/TokenAuthorize. Farklı bir etki alanı altında çalışıyorsa, etki alanınız için azurestack.local değiştirin.
![Dropbox uygulama yapılandırması][16]
9.  Yeni bir tarayıcı sekmesinde veya penceresinde, Azure Stack Yönetici portalında oturum açın (https://adminportal.local.azurestack.external) Hizmet Yöneticisi olarak.
10.  Gözat **kaynak sağlayıcıları** seçip **App Service kaynak sağlayıcısı yönetici**.
11. Tıklayın **kaynak denetimi Yapılandırması**.
12. Kopyalama ve yapıştırma **uygulama anahtarı** içine **istemci kimliği** giriş kutusu ve **uygulama gizli anahtarı** içine **gizli** DropBox için giriş kutusu.
13. **Kaydet**’e tıklayın.

## <a name="next-steps"></a>Sonraki adımlar

Kullanıcılar artık kullanabilir dağıtım kaynakları gibi şeyler için [sürekli dağıtım](https://docs.microsoft.com/azure/app-service/deploy-continuous-deployment), [yerel Git dağıtımı](https://docs.microsoft.com/azure/app-service/deploy-local-git), ve [bulut klasör eşitleme](https://docs.microsoft.com/azure/app-service/deploy-content-sync).

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
