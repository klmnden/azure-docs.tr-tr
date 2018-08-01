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
ms.date: 07/30/2018
ms.author: barbkess
ms.reviewer: asteen
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 682973e6781a1de2c8d9628e39347650a3852b81
ms.sourcegitcommit: f86e5d5b6cb5157f7bde6f4308a332bfff73ca0f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/31/2018
ms.locfileid: "39364786"
---
# <a name="troubleshooting-the-access-panel-extension-for-internet-explorer"></a>Erişim paneli uzantısını Internet Explorer için sorun giderme
Bu makalede aşağıdaki sorunları gidermenize yardımcı olur:

* Internet Explorer'ı kullanırken uygulamalarınızı uygulamalarım portalından erişemez.
* Yazılımı yüklemiş olduğunuz olsa da "Yazılım Yükle" iletisi görüntülenir.

Bir yöneticiyseniz, ayrıca bkz: [Grup İlkesi'ni kullanarak Internet Explorer için erişim paneli uzantısını dağıtma](active-directory-saas-ie-group-policy.md)

## <a name="run-the-diagnostic-tool"></a>Tanılama aracını çalıştırın
İndiriliyor ve erişim paneli Tanı Aracı'nı çalıştırarak erişim paneli uzantısını yükleme sorunları tanılayabilirsiniz:

1. [Tanılama Aracı indirmek için buraya tıklayın.](https://account.activedirectory.windowsazure.com/applications/AccessPanelExtensionDiagnosticTool/AccessPanelExtensionDiagnosticTool.zip)
2. Dosyasını açın ve basın **tümünü Ayıkla** düğmesi.
   
    ![Tuşuna tümünü Ayıkla](./media/active-directory-saas-ie-troubleshooting/extract1.png)
3. Tuşuna basarak **ayıklamak** devam etmek için düğme.
   
    ![Extract tuşuna basın](./media/active-directory-saas-ie-troubleshooting/extract2.png)
4. Aracı çalıştırmak için adlı dosyaya sağ tıklayın **AccessPanelExtensionDiagnosticTool**, ardından **birlikte Aç > Microsoft Windows tabanlı komut dosyası ana bilgisayarı**.
   
    ![Birlikte Aç > Microsoft Windows tabanlı komut dosyası ana bilgisayarı](./media/active-directory-saas-ie-troubleshooting/open_tool.png)
5. Ardından, neyin yüklemenizle birlikte yanlış olabileceğini açıklayan aşağıdaki tanılama penceresinde görürsünüz.
   
    ![Tanılama penceresinde örneği](./media/active-directory-saas-ie-troubleshooting/tool_preview.png)
6. Tıklayın "**Evet**" bulundu sorunları giderin programın sağlamak için.
7. Bu değişiklikleri kaydetmek için her Internet Explorer penceresini kapatın ve Internet Explorer'ı yeniden açın.<br />Uygulamalarınızı hala erişemiyorsanız, aşağıdaki adımları deneyin.

## <a name="check-that-the-access-panel-extension-is-enabled"></a>Erişim paneli uzantısını etkin olup olmadığını denetleyin
Erişim paneli uzantısını Internet Explorer'da etkin olduğunu doğrulamak için:

1. Internet Explorer'da tıklayın **dişli simgesini** penceresinin sağ üst köşesinde. Ardından **Internet Seçenekleri**.<br />(Internet Explorer'ın eski sürümlerinde bu altında bulabilirsiniz **Araçlar > Internet Seçenekleri**.
   
    ![Araçlar'a gidin > Internet Seçenekleri](./media/active-directory-saas-ie-troubleshooting/internetoptions.png)
2. Tıklayın **programlar** sekmesine ve ardından tıklayın **eklentileri yönetme** düğmesi.
   
    ![Eklentileri Yönet'e tıklayın](./media/active-directory-saas-ie-troubleshooting/internetoptions_programs.png)
3. Bu iletişim kutusunda seçin **erişim paneli uzantısını** ve ardından **etkinleştirme** düğmesi.
   
    ![Etkinleştir'i tıklatın](./media/active-directory-saas-ie-troubleshooting/enableaddon.png)
4. Bu değişiklikleri kaydetmek için her Internet Explorer penceresini kapatın ve Internet Explorer'ı yeniden açın.

## <a name="enable-extensions-for-inprivate-browsing"></a>InPrivate Gözatma uzantılarını etkinleştir
InPrivate Gözatma modu kullanıyorsanız:

1. Internet Explorer'da tıklayın **dişli simgesini** penceresinin sağ üst köşesinde. Ardından **Internet Seçenekleri**.<br />(Internet Explorer'ın eski sürümlerinde bu altında bulabilirsiniz **Araçlar > Internet Seçenekleri**.
   
    ![Tanılama penceresinde örneği](./media/active-directory-saas-ie-troubleshooting/inprivateoptions.png)
2. Git **gizlilik** sekmesini, ardından **işaretini kaldırın** etiketli onay kutusunu **devre dışı araç çubukları ve uzantıları InPrivate Gözatma başladığında**</p>
   
    ![InPrivate Gözatma başladığında devre dışı bırakma araç çubukları ve uzantıları seçeneğinin işaretini kaldırın](./media/active-directory-saas-ie-troubleshooting/enabletoolbars.png)
3. Bu değişiklikleri kaydetmek için her Internet Explorer penceresini kapatın ve Internet Explorer'ı yeniden açın.

## <a name="uninstall-the-access-panel-extension"></a>Erişim paneli uzantısını kaldırma
Erişim paneli uzantıyı bilgisayarınızdan kaldırmak için:

1. Klavyenizdeki basın **Windows anahtar** Başlat menüsünden açmak için. Menü açıldığında bir arama yapmak için herhangi bir şey yazabilirsiniz. "Denetim Masası" yazın ve ardından açın **Denetim Masası** arama sonuçlarında görüntülendiğinde.
   
    ![Denetim Masası'nı arayın](./media/active-directory-saas-ie-troubleshooting/search_sm.png)
2. Denetim Masası'nı sağ üst köşesinde değiştirme **görüntülemek** seçeneğini **büyük simgeler**. Ardından bulun ve tıklatın **programlar ve Özellikler** düğmesi.
   
    ![Büyük simgeleri göster Chang görünümü](./media/active-directory-saas-ie-troubleshooting/control_panel.png)
3. Listesinden **erişim paneli uzantısını**ve ardından **kaldırma** düğmesi.
   
    ![Kaldır'ı tıklatın](./media/active-directory-saas-ie-troubleshooting/uninstall.png)
4. Daha sonra yeniden sorun çözümlenmiş olup olmadığını görmek için uzantıyı yüklemek deneyebilirsiniz.

Uzantıyı kaldırma sorunlarla karşılaşırsanız, ayrıca kullanarak kaldırabilirsiniz [Microsoft düzeltin,](https://go.microsoft.com/?linkid=9779673) aracı.

## <a name="related-articles"></a>İlgili makaleler
* [Azure Active Directory'de Uygulama Yönetimi için Makale Dizini](active-directory-apps-index.md)
* [Uygulama erişimi ve Azure Active Directory ile çoklu oturum açma](manage-apps/what-is-single-sign-on.md)
* [Grup İlkesi'ni kullanarak Internet Explorer için erişim paneli uzantısını dağıtma](active-directory-saas-ie-group-policy.md)

