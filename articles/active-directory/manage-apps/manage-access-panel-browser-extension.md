---
title: Azure erişim paneli uzantısını IE için sorun giderme | Microsoft Docs
description: Internet Explorer eklenti için uygulamalarım portalında dağıtmak için Grup İlkesi kullanma
services: active-directory
documentationcenter: ''
author: msmimart
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/11/2019
ms.author: mimart
ms.reviewer: asteen
ms.custom: H1Hack27Feb2017
ms.collection: M365-identity-device-management
ms.openlocfilehash: f08a02842c97b0f4076a1b311aa918df6d83c592
ms.sourcegitcommit: be9fcaace62709cea55beb49a5bebf4f9701f7c6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/17/2019
ms.locfileid: "65824511"
---
# <a name="troubleshoot-the-access-panel-extension-for-internet-explorer"></a>Erişim paneli uzantısını Internet Explorer için sorun giderme

Bu makalede aşağıdaki sorunları gidermenize yardımcı olur:

* Internet Explorer'ı kullanırken uygulamalarınızı uygulamalarım portalından erişemez.
* Yazılımı yüklemiş olduğunuz olsa da "Yazılım Yükle" iletisi görüntülenir.

Bir yönetici değilseniz bkz [Grup İlkesi'ni kullanarak Internet Explorer için erişim paneli uzantısını dağıtma](deploy-access-panel-browser-extension.md).

## <a name="run-the-diagnostic-tool"></a>Tanılama aracını çalıştırın

İndiriliyor ve erişim paneli Tanı Aracı'nı çalıştırarak erişim paneli uzantısını yükleme sorunları tanılayabilirsiniz. 

İndirin ve Tanılama Aracı yüklemek için:

1. [Tanılama Aracı indirmek için bu bağlantıyı seçin.](https://account.activedirectory.windowsazure.com/applications/AccessPanelExtensionDiagnosticTool/AccessPanelExtensionDiagnosticTool.zip)

2. Dosyasını açın ve içeriği bilgisayarınıza ayıklayın.
   
3. Aracı çalıştırmak için adlı dosyaya sağ tıklayın *AccessPanelExtensionDiagnosticTool.js* seçip **birlikte Aç** > **Microsoft Windows tabanlı komut dosyası ana bilgisayarı** .
   
    ![Birlikte Aç > Microsoft Windows tabanlı komut dosyası ana bilgisayarı](./media/manage-access-panel-browser-extension/open-access-panel-extension-diagnostic-tool.png)

4. Görüntülenen ve seçin Tanılama sonuçları gözden **Evet** sorunlarını düzeltmek için. **Denetleme sonuçları** uzantı işe yaramazsa yapmanız gerekenler hakkında bilgi içeren iletişim kutusu görüntülenir.  

5. İleti okumak ve seçin **Tamam**.

## <a name="check-that-the-access-panel-extension-is-enabled"></a>Erişim paneli uzantısını etkin olup olmadığını denetleyin

Erişim paneli uzantısını Internet Explorer'da etkinleştirdikten doğrulamak için:

1. Internet Explorer'da seçin **dişli simgesini** penceresini seçin ve sağ üst köşedeki **Internet Seçenekleri**.
   
2. Git **programlar** sekmenize **eklentileri yönetme**.
   
3. Seçin **erişim paneli uzantısını** içinde **Microsoft Corporation** seçin ve bölüm **etkinleştirme**.
   
4. Değişiklikleri Kaydet, tüm Internet Explorer tarayıcı pencerelerini kapatmak için açık olması gerekir. Değişiklik, Internet Explorer bir sonraki açışınızda etkinleşir.

## <a name="enable-extensions-for-inprivate-browsing"></a>InPrivate Gözatma uzantılarını etkinleştir

InPrivate Gözatma için uzantıları etkinleştirmek için:

1. Internet Explorer'da seçin **dişli simgesini** penceresini seçin ve sağ üst köşedeki **Internet Seçenekleri**.
   
2. Git **gizlilik** doğrulayın ve sekme **devre dışı araç çubukları ve uzantıları InPrivate Gözatma başladığında** onay kutusunu temizleyin.
   
3.  Değişiklikleri Kaydet, tüm Internet Explorer tarayıcı pencerelerini kapatmak için açık olması gerekir. Değişiklik, Internet Explorer bir sonraki açışınızda etkinleşir.

## <a name="uninstall-the-access-panel-extension"></a>Erişim paneli uzantısını kaldırma

Erişim paneli uzantısını bilgisayarınızdan kaldırmak için:

1. Denetim Masası'ndaki arama *kaldırma*. 

2. Arama sonuçlarında seçin **program Kaldır**.
   
    ![Programı Kaldır arayın.](./media/manage-access-panel-browser-extension/uninstall-program-control-panel.png)

3. Listesinden **erişim paneli uzantısını** seçip **kaldırma**.

    ![Erişim paneli uzantısını kaldırın.](./media/manage-access-panel-browser-extension/uninstall-access-panel-extension.png)
   
4. Daha sonra yeniden sorun çözümlenmiş olup olmadığını görmek için uzantıyı yüklemek deneyebilirsiniz.

Uzantıyı kaldırma sorun yaşarsanız, ayrıca kullanarak kaldırabilirsiniz [Microsoft düzeltin,](https://go.microsoft.com/?linkid=9779673) aracı.

## <a name="related-articles"></a>İlgili makaleler
* [Uygulama erişimi ve Azure Active Directory ile çoklu oturum açma](what-is-single-sign-on.md)
* [Grup İlkesi'ni kullanarak Internet Explorer için erişim paneli uzantısını dağıtma](deploy-access-panel-browser-extension.md)

