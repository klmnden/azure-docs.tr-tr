---
title: Azure erişim paneli uzantısı için IE sorunlarını giderme | Microsoft Docs
description: My uygulamaları portal için Internet Explorer eklentisi dağıtmak için Grup İlkesi kullanma
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: mtillman
editor: ''
ms.assetid: f56b3230-26fd-42ec-9e3d-2c12daf15479
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 10/31/2017
ms.author: markvi
ms.reviewer: asteen
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: a4f1538cf598da8b5b9aa19def2d5f86ceaca0a0
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2018
---
# <a name="troubleshooting-the-access-panel-extension-for-internet-explorer"></a>Erişim paneli uzantısı Internet Explorer için sorun giderme
Bu makalede aşağıdaki sorunları gidermenize yardımcı olur:

* Internet Explorer'ı kullanırken, uygulamalarınızı My Apps portalınızda erişemez.
* Yazılımını zaten yüklemiş olsa bile "Yazılımı yükle" iletisini görürsünüz.

Bir yönetici, ayrıca bkz: [Grup İlkesi'ni kullanarak Internet Explorer için erişim paneli uzantısı dağıtma](active-directory-saas-ie-group-policy.md)

## <a name="run-the-diagnostic-tool"></a>Tanı Aracı'nı çalıştırın
Yükleme ve erişim paneli Tanı Aracı'nı çalıştırma erişim paneli uzantılı yükleme sorunlarını tanılamanıza:

1. [Tanı Aracı'nı yüklemek için burayı tıklatın.](https://account.activedirectory.windowsazure.com/applications/AccessPanelExtensionDiagnosticTool/AccessPanelExtensionDiagnosticTool.zip)
2. Dosyasını açın ve basın **tümünü Ayıkla** düğmesi.
   
    ![Tuşuna tümünü Ayıkla](./media/active-directory-saas-ie-troubleshooting/extract1.png)
3. Tuşuna basarak **ayıklamak** devam etmek için düğmesini.
   
    ![Extract tuşuna basın](./media/active-directory-saas-ie-troubleshooting/extract2.png)
4. Aracı çalıştırmak için adlı dosyaya sağ **AccessPanelExtensionDiagnosticTool**seçeneğini belirleyip **birlikte Aç > Microsoft Windows tabanlı komut dosyası ana bilgisayarı**.
   
    ![Birlikte Aç > Microsoft Windows tabanlı komut dosyası ana bilgisayarı](./media/active-directory-saas-ie-troubleshooting/open_tool.png)
5. Sonra ne yüklemenizle birlikte sorun olabilir açıklar aşağıdaki tanı pencere görürsünüz.
   
    ![Tanılama penceresi örneği](./media/active-directory-saas-ie-troubleshooting/tool_preview.png)
6. Tıklayın "**Evet**" bulundu sorunları giderin programının verin.
7. Bu değişiklikleri kaydetmek için her Internet Explorer penceresini kapatın ve Internet Explorer'ı yeniden açın.<br />Uygulamalarınızı hala erişemiyorsanız, aşağıdaki adımları deneyin.

## <a name="check-that-the-access-panel-extension-is-enabled"></a>Erişim paneli uzantısı etkin olup olmadığını denetleyin
Erişim paneli uzantısı Internet Explorer'da etkin olduğunu doğrulamak için:

1. Internet Explorer'daki **dişli simgesi** pencerenin sağ üst köşesinde üzerinde. Ardından **Internet Seçenekleri**.<br />(Internet Explorer'ın daha eski sürümlerinde bu altında bulabilirsiniz **Araçlar > Internet Seçenekleri**.
   
    ![İçin Araçlar > Internet Seçenekleri](./media/active-directory-saas-ie-troubleshooting/internetoptions.png)
2. Tıklatın **programları** sekmesini ve ardından **eklentileri yönetme** düğmesi.
   
    ![Eklentileri Yönet'e tıklayın](./media/active-directory-saas-ie-troubleshooting/internetoptions_programs.png)
3. Bu iletişim kutusunda seçin **erişim paneli uzantısı** ve ardından **etkinleştirmek** düğmesi.
   
    ![Etkinleştir'i tıklatın](./media/active-directory-saas-ie-troubleshooting/enableaddon.png)
4. Bu değişiklikleri kaydetmek için her Internet Explorer penceresini kapatın ve Internet Explorer'ı yeniden açın.

## <a name="enable-extensions-for-inprivate-browsing"></a>InPrivate Gözatma için uzantılarını etkinleştirme
InPrivate Gözatma modu kullanıyorsanız:

1. Internet Explorer'daki **dişli simgesi** pencerenin sağ üst köşesinde üzerinde. Ardından **Internet Seçenekleri**.<br />(Internet Explorer'ın daha eski sürümlerinde bu altında bulabilirsiniz **Araçlar > Internet Seçenekleri**.
   
    ![Tanılama penceresi örneği](./media/active-directory-saas-ie-troubleshooting/inprivateoptions.png)
2. Git **gizlilik** sekmesinde, ardından **işaretini** etiketli onay kutusunu **devre dışı araç çubukları ve uzantıları InPrivate Gözatma başladığında**</p>
   
    ![InPrivate Gözatma başladığında devre dışı bırak araç çubukları ve uzantıları seçeneğinin işaretini kaldırın](./media/active-directory-saas-ie-troubleshooting/enabletoolbars.png)
3. Bu değişiklikleri kaydetmek için her Internet Explorer penceresini kapatın ve Internet Explorer'ı yeniden açın.

## <a name="uninstall-the-access-panel-extension"></a>Erişim paneli uzantısını Kaldır
Erişim paneli uzantısı bilgisayarınızdan kaldırmak için:

1. Klavyenizde, basın **Windows tuşu** Başlat menüsünden açmak için. Menü açık olduğunda, bir arama yapmak için herhangi bir şey yazabilirsiniz. "Denetim Masası" yazın ve ardından açın **Denetim Masası** arama sonuçlarında görüntülendiğinde.
   
    ![Denetim Masası arayın](./media/active-directory-saas-ie-troubleshooting/search_sm.png)
2. Denetim Masası'nı sağ üst köşesinde değiştirme **görüntülemek** için seçenek **büyük simgeler**. Ardından bulun ve tıklatın **programlar ve Özellikler** düğmesi.
   
    ![Büyük simgeleri göstermek için izleme görünümü](./media/active-directory-saas-ie-troubleshooting/control_panel.png)
3. Listesinden **erişim paneli uzantısı**ve ardından **kaldırma** düğmesi.
   
    ![Kaldır'ı tıklatın](./media/active-directory-saas-ie-troubleshooting/uninstall.png)
4. Daha sonra yeniden sorun çözümlenmiş olup olmadığını görmek için uzantıyı yüklemek deneyebilirsiniz.

Uzantı kaldırma sorunlarla karşılaşırsanız, ayrıca kullanarak kaldırabilirsiniz [Microsoft düzeltme IT](https://go.microsoft.com/?linkid=9779673) aracı.

## <a name="related-articles"></a>İlgili makaleler
* [Azure Active Directory'de Uygulama Yönetimi için Makale Dizini](active-directory-apps-index.md)
* [Uygulama erişimi ve Azure Active Directory ile çoklu oturum açma](manage-apps/what-is-single-sign-on.md)
* [Grup İlkesi'ni kullanarak Internet Explorer için erişim paneli uzantısı dağıtma](active-directory-saas-ie-group-policy.md)

