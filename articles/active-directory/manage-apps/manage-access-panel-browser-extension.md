---
title: Azure erişim paneli uzantısını IE için sorun giderme | Microsoft Docs
description: Internet Explorer eklenti için uygulamalarım portalında dağıtmak için Grup İlkesi kullanma
services: active-directory
documentationcenter: ''
author: barbkess
manager: mtillman
ms.service: active-directory
ms.component: app-mgmt
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/11/2018
ms.author: barbkess
ms.reviewer: asteen
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 9ebd949460f826c9529b9f392bc4a7f4918ee170
ms.sourcegitcommit: c29d7ef9065f960c3079660b139dd6a8348576ce
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/12/2018
ms.locfileid: "44715049"
---
# <a name="troubleshooting-the-access-panel-extension-for-internet-explorer"></a>Erişim paneli uzantısını Internet Explorer için sorun giderme
Bu makalede aşağıdaki sorunları gidermenize yardımcı olur:

* Internet Explorer'ı kullanırken uygulamalarınızı uygulamalarım portalından erişemez.
* Yazılımı yüklemiş olduğunuz olsa da "Yazılım Yükle" iletisi görüntülenir.

Bir yöneticiyseniz, ayrıca bkz: [Grup İlkesi'ni kullanarak Internet Explorer için erişim paneli uzantısını dağıtma](deploy-access-panel-browser-extension.md)

## <a name="run-the-diagnostic-tool"></a>Tanılama aracını çalıştırın
İndiriliyor ve erişim paneli Tanı Aracı'nı çalıştırarak erişim paneli uzantısını yükleme sorunları tanılayabilirsiniz:

1. [Tanılama Aracı indirmek için buraya tıklayın.](https://account.activedirectory.windowsazure.com/applications/AccessPanelExtensionDiagnosticTool/AccessPanelExtensionDiagnosticTool.zip)
2. Dosyasını açın ve basın **tümünü Ayıkla** düğmesi.
   
    ![Tuşuna tümünü Ayıkla](./media/manage-access-panel-browser-extension/extract1.png)
3. Tuşuna basarak **ayıklamak** devam etmek için düğme.
   
    ![Extract tuşuna basın](./media/manage-access-panel-browser-extension/extract2.png)
4. Aracı çalıştırmak için adlı dosyaya sağ tıklayın **AccessPanelExtensionDiagnosticTool**, ardından **birlikte Aç > Microsoft Windows tabanlı komut dosyası ana bilgisayarı**.
   
    ![Birlikte Aç > Microsoft Windows tabanlı komut dosyası ana bilgisayarı](./media/manage-access-panel-browser-extension/open_tool.png)
5. Ardından, neyin yüklemenizle birlikte yanlış olabileceğini açıklayan aşağıdaki tanılama penceresinde görürsünüz.
   
    ![Tanılama penceresinde örneği](./media/manage-access-panel-browser-extension/tool_preview.png)
6. Tıklayın "**Evet**" bulundu sorunları giderin programın sağlamak için.
7. Bu değişiklikleri kaydetmek için her Internet Explorer penceresini kapatın ve Internet Explorer'ı yeniden açın.<br />Uygulamalarınızı hala erişemiyorsanız, aşağıdaki adımları deneyin.

## <a name="check-that-the-access-panel-extension-is-enabled"></a>Erişim paneli uzantısını etkin olup olmadığını denetleyin
Erişim paneli uzantısını Internet Explorer'da etkin olduğunu doğrulamak için:

1. Internet Explorer'da tıklayın **dişli simgesini** penceresinin sağ üst köşesinde. Ardından **Internet Seçenekleri**.<br />(Internet Explorer'ın eski sürümlerinde bu altında bulabilirsiniz **Araçlar > Internet Seçenekleri**.
   
    ![Araçlar'a gidin > Internet Seçenekleri](./media/manage-access-panel-browser-extension/internetoptions.png)
2. Tıklayın **programlar** sekmesine ve ardından tıklayın **eklentileri yönetme** düğmesi.
   
    ![Eklentileri Yönet'e tıklayın](./media/manage-access-panel-browser-extension/internetoptions_programs.png)
3. Bu iletişim kutusunda seçin **erişim paneli uzantısını** ve ardından **etkinleştirme** düğmesi.
   
    ![Etkinleştir'i tıklatın](./media/manage-access-panel-browser-extension/enableaddon.png)
4. Bu değişiklikleri kaydetmek için her Internet Explorer penceresini kapatın ve Internet Explorer'ı yeniden açın.

## <a name="enable-extensions-for-inprivate-browsing"></a>InPrivate Gözatma uzantılarını etkinleştir
InPrivate Gözatma modu kullanıyorsanız:

1. Internet Explorer'da tıklayın **dişli simgesini** penceresinin sağ üst köşesinde. Ardından **Internet Seçenekleri**.<br />(Internet Explorer'ın eski sürümlerinde bu altında bulabilirsiniz **Araçlar > Internet Seçenekleri**.
   
    ![Tanılama penceresinde örneği](./media/manage-access-panel-browser-extension/inprivateoptions.png)
2. Git **gizlilik** sekmesini, ardından **işaretini kaldırın** etiketli onay kutusunu **devre dışı araç çubukları ve uzantıları InPrivate Gözatma başladığında**</p>
   
    ![InPrivate Gözatma başladığında devre dışı bırakma araç çubukları ve uzantıları seçeneğinin işaretini kaldırın](./media/manage-access-panel-browser-extension/enabletoolbars.png)
3. Bu değişiklikleri kaydetmek için her Internet Explorer penceresini kapatın ve Internet Explorer'ı yeniden açın.

## <a name="uninstall-the-access-panel-extension"></a>Erişim paneli uzantısını kaldırma
Erişim paneli uzantıyı bilgisayarınızdan kaldırmak için:

1. Klavyenizdeki basın **Windows anahtar** Başlat menüsünden açmak için. Menü açıldığında bir arama yapmak için herhangi bir şey yazabilirsiniz. "Denetim Masası" yazın ve ardından açın **Denetim Masası** arama sonuçlarında görüntülendiğinde.
   
    ![Denetim Masası'nı arayın](./media/manage-access-panel-browser-extension/search_sm.png)
2. Denetim Masası'nı sağ üst köşesinde değiştirme **görüntülemek** seçeneğini **büyük simgeler**. Ardından bulun ve tıklatın **programlar ve Özellikler** düğmesi.
   
    ![Büyük simgeleri göster Chang görünümü](./media/manage-access-panel-browser-extension/control_panel.png)
3. Listesinden **erişim paneli uzantısını**ve ardından **kaldırma** düğmesi.
   
    ![Kaldır'ı tıklatın](./media/manage-access-panel-browser-extension/uninstall.png)
4. Daha sonra yeniden sorun çözümlenmiş olup olmadığını görmek için uzantıyı yüklemek deneyebilirsiniz.

Uzantıyı kaldırma sorunlarla karşılaşırsanız, ayrıca kullanarak kaldırabilirsiniz [Microsoft düzeltin,](https://go.microsoft.com/?linkid=9779673) aracı.

## <a name="related-articles"></a>İlgili makaleler
* [Uygulama erişimi ve Azure Active Directory ile çoklu oturum açma](what-is-single-sign-on.md)
* [Grup İlkesi'ni kullanarak Internet Explorer için erişim paneli uzantısını dağıtma](deploy-access-panel-browser-extension.md)

