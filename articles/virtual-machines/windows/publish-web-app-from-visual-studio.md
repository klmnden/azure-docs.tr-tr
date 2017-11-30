---
title: "Bir Web uygulaması için bir Azure VM Visual Studio'dan yayımlamak | Microsoft Docs"
description: "Visual Studio'da bir Azure sanal makinesi için bir ASP.NET Web uygulaması yayımlama"
services: virtual-machines-windows
documentationcenter: 
author:
- kraigb
- justcla
manager: ghogen
editor: 
tags: azure-service-management
ms.assetid: 70267837-3629-41e0-bb58-2167ac4932b3
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: dotnet
ms.topic: article
ms.date: 11/03/2017
ms.author:
- kraigb
- justcla
ms.openlocfilehash: 5a0dd3d123cb0d580ea753cebc36ebcdb7084db9
ms.sourcegitcommit: 29bac59f1d62f38740b60274cb4912816ee775ea
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/29/2017
---
# <a name="publish-an-aspnet-web-app-to-an-azure-vm-from-visual-studio"></a>Visual Studio'dan bir Azure VM için bir ASP.NET Web uygulaması yayımlama

Bu belgede bir Azure sanal makine (VM) kullanarak bir ASP.NET web uygulaması yayımlama açıklanır **Microsoft Azure sanal makineleri** Yayımlama özelliği, Visual Studio 2017.  

## <a name="prerequisites"></a>Ön koşullar
Bir Azure VM için bir ASP.NET projesi yayımlamak için Visual Studio kullanabilmeniz için VM doğru şekilde ayarlanması gerekir.

- Bir ASP.NET web uygulamasını çalıştırmak ve WebDeploy yüklü olması için makine yeniden yapılandırılması gerekir.

- VM yapılandırılmış bir DNS adı olmalıdır. Daha fazla bilgi için bkz: [bir tam etki alanı adı için bir Windows VM Azure portalında oluşturma](portal-create-fqdn.md).

## <a name="publish-your-aspnet-web-app-to-the-azure-vm-using-visual-studio"></a>Visual Studio kullanarak Azure VM ASP.NET web uygulamanızı yayınlama
Aşağıdaki bölümde, bir Azure sanal makinesi için mevcut bir ASP.NET web uygulamasını yayımlama açıklanır.

1. Web uygulaması çözümünü Visual Studio 2017 açın.
2. Çözüm Gezgini'nde projeye sağ tıklayın ve seçin **Yayımla...**
3. Bulana kadar yayımlama seçenekler arasında gezinmek için sayfanın sağ tarafta oku kullanın **Microsoft Azure sanal makineleri**.  

   ![Yayımla Sayfası - sağ ok]

4. Seçin **Microsoft Azure sanal makineleri** simgesi ve select **Yayımla**.

   ![Yayımla Sayfası - Microsoft Azure sanal makinesi simgesi]

5. (İle Azure aboneliği, sanal makineye bağlı) uygun hesabı seçin.  
   - Visual Studio açmış durumdaysanız, hesap listesi tüm kimliği doğrulanmış hesaplarınızı ile doldurulur.  
   - "Ekle bir hesap...", oturumunuz açık değil veya gereksinim duyduğunuz hesap listelenmiyorsa, belirleyin ve oturum açmak için istemleri izleyin.  
   ![Azure hesabı Seçici]  

6. Uygun VM, mevcut sanal makineler listesinden seçin.

   > [!Note]
   > Bu liste doldurulurken biraz zaman alabilir.

   ![Azure VM Seçici]

7. Yayımlama başlatmak için Tamam'ı tıklatın.

8. Kimlik bilgileri istendiğinde, kullanıcı adı ve hak (genellikle yönetici kullanıcı adı ve parola VM oluşturulurken kullanılan) yayımlama ile yapılandırılmış VM hedefte kullanıcı hesabının parolasını sağlayın.  

   ![WebDeploy oturum açma]

9. Güvenlik sertifikası kabul edin.

   ![Sertifika hatası]

10. Yayımlama işlemi ilerleme durumunu denetlemek için çıkış penceresini izleyin.

    ![Çıktı penceresi]

11. Yayımlama başarılı olursa, yeni yayımlanan site URL'sini açmak için bir tarayıcı başlatılır.

**Başarılı!**

Şimdi başarıyla web uygulamanız için bir Azure sanal makinesi yayımladığınız.

## <a name="publish-page-options"></a>Sayfa Seçenekleri Yayımla

Yayımlama Sihirbazı'nı tamamladıktan sonra Yayımla Sayfası belgede seçilen iyi yeni yayımlama profili ile açılır.

### <a name="re-publish"></a>Yeniden yayımlayın

Web uygulamanıza güncelleştirmeleri yayımlamak için seçin **Yayımla** Yayımla Sayfası düğmesinde.  
- İstenirse, kullanıcı adı ve parola girin.  
- Yayımlama hemen başlar.

![Yayımla Sayfası - Yayımla düğmesi]

### <a name="modify-publish-profile-settings"></a>Değiştirme yayımlama profili ayarları

Görüntülemek ve yayımlama profili ayarlarını değiştirmek için seçin **ayarları...** .  

![Yayımla Sayfası - Ayarlar düğmesi]

Ayarlarınızı aşağıdakine benzer görünmelidir:  

![Yayımlama ayarları - bağlantı sayfası]

#### <a name="save-user-name-and-password"></a>Kullanıcı adı ve parola Kaydet
- Her yayımladığınızda kimlik doğrulama bilgilerini sağlama önlemek için doldurabilirsiniz **kullanıcı adı** ve **parola** alanları ve select **Parolayı Kaydet** kutusu.
- Kullanım **bağlantıyı doğrula** düğmesi doğru bilgileri girdiğinizden emin onaylayın.

#### <a name="deploy-to-clean-web-server"></a>Web sunucusu temizlemek için dağıtma

- Web sunucusu web uygulaması her karşıya yükleme sonra temiz bir kopyasına sahip olmak istiyorsanız (ve diğer hiçbir dosya önceki bir dağıtımın asılı bırakılır), göz atabilirsiniz **hedefteki ek dosyaları Kaldır** onay kutusu **ayarları** sekmesi.

- Uyarı: Bu ayar ile yayımlama web sunucusunda (wwwroot dizinini) mevcut tüm dosyaları siler. Bu seçenek etkinleştirildiğinde yayımlamadan önce makinenin durumunu bildiğinizden emin olun. 

![Yayımlama ayarları - Ayarları sayfası]

## <a name="next-steps"></a>Sonraki adımlar

### <a name="set-up-cicd-for-automated-deployment-to-azure-vm"></a>Azure VM için otomatik dağıtım için CI/CD Ayarla

Visual Studio Team hizmeti ile sürekli teslim ardışık ayarlamak için bkz: [dağıtma için bir Windows sanal makine](https://docs.microsoft.com/en-us/vsts/build-release/apps/cd/deploy-webdeploy-iis-deploygroups).

[VM Overview - DNS Name]: ../../../includes/media/publish-web-app-from-visual-studio/VMOverviewDNSName.png
[IP Address Config - DNS Name]: ../../../includes/media/publish-web-app-from-visual-studio/IPAddressConfigDNSName.png
[VM Overview - DNS Configured]: ../../../includes/media/publish-web-app-from-visual-studio/VMOverviewDNSConfigured.png
[Yayımla Sayfası - sağ ok]: ../../../includes/media/publish-web-app-from-visual-studio/PublishPageRightArrow.png
[Yayımla Sayfası - Microsoft Azure sanal makinesi simgesi]: ../../../includes/media/publish-web-app-from-visual-studio/PublishPageMicrosoftAzureVirtualMachineIcon.png
[Azure hesabı Seçici]: ../../../includes/media/publish-web-app-from-visual-studio/ChooseVM-SelectAccount.png
[Azure VM Seçici]: ../../../includes/media/publish-web-app-from-visual-studio/ChooseVM-SelectVM.png
[WebDeploy oturum açma]: ../../../includes/media/publish-web-app-from-visual-studio/WebDeployLogin.png
[Sertifika hatası]: ../../../includes/media/publish-web-app-from-visual-studio/CertificateError.png
[Çıktı penceresi]: ../../../includes/media/publish-web-app-from-visual-studio/OutputWindow.png
[Yayımla Sayfası - Yayımla düğmesi]: ../../../includes/media/publish-web-app-from-visual-studio/PublishPagePublishButton.png
[Yayımla Sayfası - Ayarlar düğmesi]: ../../../includes/media/publish-web-app-from-visual-studio/PublishPageSettingsButton.png
[Yayımlama ayarları - bağlantı sayfası]: ../../../includes/media/publish-web-app-from-visual-studio/PublishSettingsConnectionPage.png
[Yayımlama ayarları - Ayarları sayfası]: ../../../includes/media/publish-web-app-from-visual-studio/PublishSettingsSettingsPage.png
