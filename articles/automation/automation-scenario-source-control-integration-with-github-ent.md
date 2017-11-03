---
title: "Azure Otomasyonu kaynak denetimi tümleştirmesinin GitHub kuruluş ile | Microsoft Docs"
description: "Otomasyon runbook'ları kaynak denetimi için GitHub Enterprise ile tümleştirme yapılandırma ayrıntılarını açıklar."
services: automation
documentationCenter: 
authors: eslesar
manager: jwhit
editor: 
ms.assetid: e01d817c-7d38-421c-adf5-647a4b526eb4
ms.service: automation
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/26/2017
ms.author: magoedte
ms.openlocfilehash: 2b3ca90382a1c4fb6446799ad539ab8ee603aefc
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="azure-automation-scenario---automation-source-control-integration-with-github-enterprise"></a>Azure otomasyonu senaryosu - GitHub kuruluş ile Otomasyon kaynak denetimi tümleştirmesi

Otomasyon runbook'ları GitHub kaynak denetim deponuza Otomasyon hesabınızda ilişkilendirmenizi sağlar kaynak denetimi tümleştirmesi, şu anda destekler.  Ancak, dağıtan müşteriler [GitHub Kurumsal](https://enterprise.github.com/home) kendi DevOps uygulamalarını desteklemek için ayrıca iş süreçlerini otomatikleştirmek ve hizmet yönetimi işlemleri için geliştirilen runbook'ları ömrünü yönetmek için kullanmak istiyorsanız.  

Bu senaryoda, veri merkezinizdeki karma Runbook çalışanı Azure Resource Manager modüllerini ve Git araçlarının yüklü olarak yapılandırılmış bir Windows bilgisayarı vardır.  Karma çalışanı makinenin bir kopyasını yerel Git deposu vardır.  Karma çalışanında runbook çalıştırıldığında, Git dizin eşitlenir ve runbook dosyası içeriği Otomasyon dikkate alınır.

Bu makalede, bu yapılandırma, Azure Automation ortamınızdaki ayarlama açıklar. Otomasyon runbook'ları runbook'ları çalıştırmak ve runbook'ları eşitlemek için GitHub Kurumsal deponuza erişmek için veri merkezinizdeki bu senaryo ve karma Runbook Worker dağıtımını desteklemek için gereken güvenlik tanıtım bilgilerini yapılandırarak Başlat Automation hesabınız ile.  


## <a name="getting-the-scenario"></a>Senaryoyu alma

Bu senaryo, doğrudan aktarabilirsiniz iki PowerShell runbook'ların oluşur [Runbook Galerisi](automation-runbook-gallery.md) Azure portalında veya Merkezi'nden [PowerShell Galerisi](https://www.powershellgallery.com).

### <a name="runbooks"></a>Runbook'lar

Runbook | Açıklama| 
--------|------------|
Dışarı aktarma RunAsCertificateToHybridWorker | Çalışan runbook'ları Azure ile runbook Otomasyonu dikkate almak için doğrulanabilmesi Runbook için bir karma çalışanı bir Otomasyon hesabının RunAs sertifika verir.| 
Eşitleme LocalGitFolderToAutomationAccount | Runbook karma makinede yerel Git klasörü eşitlenir ve ardından runbook dosyaları (*.ps1) Otomasyon dikkate alın.|

### <a name="credentials"></a>Kimlik Bilgileri

Kimlik Bilgisi | Açıklama|
-----------|------------|
GitHRWCredential | Kimlik bilgisi varlığı kullanıcı adı ve parola karma çalışanı izinlerine sahip bir kullanıcı için içerecek şekilde oluşturun.|

## <a name="installing-and-configuring-this-scenario"></a>Bu senaryoyu yükleme ve yapılandırma

### <a name="prerequisites"></a>Ön koşullar

1. Eşitleme LocalGitFolderToAutomationAccount runbook kullanarak kimlik doğrulaması [Azure farklı çalıştır hesabı](automation-sec-configure-azure-runas-account.md). 

2. Microsoft Operations Management Suite (OMS) çalışma alanında etkin ve yapılandırılmış Azure Otomasyon çözümünü ile de gereklidir.  Yükleyin ve bu senaryo yapılandırmak için kullanılan Otomasyon hesabı ile ilişkili olan bir yoksa, oluşturulur ve yürüttüğünüzde sizin için yapılandırılmış **yeni OnPremiseHybridWorker.ps1** karma runbook betikten çalışan.        

    > [!NOTE]
    > Şu anda aşağıdaki bölgeler yalnızca Otomasyon OMS ile-tümleştirmeyi **Avustralya Güneydoğu**, **Doğu ABD 2**, **Güneydoğu Asya**, ve **Batı Avrupa**. 

3. Bir adanmış karma Runbook de GitHub yazılım barındırmak ve runbook dosyaları korumak çalışan hizmet verebilir bir bilgisayar (*runbook*.ps1) GitHub arasında eşitlemek için dosya sisteminde bir kaynak dizinde ve Otomasyon hesabı.

### <a name="import-and-publish-the-runbooks"></a>İçeri aktarma ve runbook'ları yayımlama

İçeri aktarmak için *verme RunAsCertificateToHybridWorker* ve *eşitleme LocalGitFolderToAutomationAccount* Azure portalındaki Otomasyon hesabınız Runbook'u Galeriden runbook'lardan izleyin yordamlarda [Runbook'u Galeriden içeri aktarma Runbook](automation-runbook-gallery.md#to-import-a-runbook-from-the-runbook-gallery-with-the-azure-portal). Otomasyon hesabınızda başarıyla içeri aktarıldıktan sonra runbook'ları yayımlayın.

### <a name="deploy-and-configure-hybrid-runbook-worker"></a>Dağıtma ve karma Runbook çalışanı yapılandırma

Veri merkezinizde zaten dağıtılmış bir karma Runbook çalışanı yoksa gereksinimleri gözden geçirmeli ve konusundaki yordamı kullanarak otomatik yükleme adımları [Azure Otomasyon karma Runbook çalışanları - otomatikleştirmek yüklemek ve Yapılandırma](automation-hybrid-runbook-worker.md#automated-deployment).  Bir bilgisayarda karma çalışanı başarıyla yüklendikten sonra bu senaryoyu desteklemek için yapılandırmasını tamamlamak için aşağıdaki adımları gerçekleştirin.

1. Yerel yönetici haklarına sahip bir hesapla karma Runbook çalışanı rolü barındıran bilgisayarda oturum açma ve Git runbook dosyalarını barındıracak bir dizin oluşturun.  İç Git deposu dizinine kopyalayın.
2. Önceden oluşturulmuş bir farklı çalıştır hesabı yok veya yeni bir ayrılmış bu amaçla oluşturmak isterseniz Azure portalından Automation hesaplarına gidin, Otomasyon hesabınızı seçin ve oluşturma bir [kimlik bilgisi varlığı](automation-credentials.md) , Kullanıcı adı ve parola karma çalışanı izinlerine sahip bir kullanıcı için içerir.  
3. Otomasyon hesabınızdan [runbook'u düzenlemek](automation-edit-textual-runbook.md)**verme RunAsCertificateToHybridWorker** ve değişkeninin değerini değiştirme *$Password* güçlü bir parola ile.  Değer değiştirdikten sonra tıklatın **Yayımla** yayımlanan runbook'un taslak sürümünü sağlamak için. 
5. Runbook'u başlatmak **verme RunAsCertificateToHybridWorker**hem de **Runbook'u Başlat** seçeneği altında dikey **çalışma ayarları** seçeneğini  **Karma çalışanı** ve aşağı açılan listede daha önce oluşturduğunuz bu senaryo için karma çalışanı grubu seçin.  

    Böylece çalışan can runbook'larda (içindeki bu senaryo - alma runbook Otomasyon hesabı için belirli) Azure kaynaklarınızı yönetmek için farklı çalıştır bağlantısı kullanarak Azure ile kimlik doğrulaması Bu karma çalışanı bir sertifika verir.

4. Otomasyon hesabınızdan daha önce oluşturduğunuz karma çalışanı grubu seçin ve [bir RunAs hesabı belirtin](automation-hrw-run-runbooks.md#runas-account) karma çalışanı grubu ve seçtiğiniz yalnızca veya önceden oluşturduğunuz kimlik bilgisi varlığı.  Bu, eşitleme runbook Git komutları çalıştırabilirsiniz sağlar. 
5. Runbook'u başlatmak **eşitleme LocalGitFolderToAutomationAccount**, aşağıdaki gereken giriş parametre değerlerini sağlayın ve **Runbook'u Başlat** seçeneği altında dikey **çalışma ayarları**  seçeneğini **karma çalışanı** ve daha önce oluşturduğunuz bu senaryo için karma çalışanı grubu aşağı açılan listeden seçin:
    * *Kaynak grubu* -kaynak grubunuzun adını Automation hesabınız ile ilişkili
    * *AutomationAccountName* -Otomasyon hesabınızın adını
    * *GitPath* -yerel klasör veya dosya Git ayarlandığı yukarı en son değişiklikleri çekmesini karma Runbook çalışanı üzerinde

    Bu karma çalışan bilgisayarda yerel Git klasörü eşitlemek ve sonra .ps1 dosyaları kaynak dizinden Otomasyon hesabı alın.

    ![Eşitleme LocalGitFolderToAutomationAccount Runbook başlatın](media/automation-scenario-source-control-integration-with-github-ent/start-runbook-synclocalgitfoldertoautoacct.png)<br>

7. Buradan seçerek runbook için iş Özet ayrıntıları görüntüleyin **Runbook'lar** dikey penceresinde, Automation hesabı ve ardından **işleri** döşeme.  Tamamlanmış başarıyla seçerek onaylayın **tüm günlükleri** döşeme ve ayrıntılı günlük akışı gözden geçirme.  

## <a name="next-steps"></a>Sonraki adımlar

-  Runbook türleri, avantajları ve sınırlamaları hakkında daha fazla bilgi için bkz. [Azure Automation runbook türleri](automation-runbook-types.md)
-  PowerShell betik desteği özelliği hakkında daha fazla bilgi için bkz. [Azure Automation’da Yerel PowerShell betik desteği](https://azure.microsoft.com/blog/announcing-powershell-script-support-azure-automation-2/)
