---
title: Azure Otomasyonu GitHub Enterprise ile kaynak denetimi tümleştirmesi
description: Otomasyon runbook'ları kaynak denetimi için GitHub Enterprise ile tümleştirmeyi yapılandırma ayrıntılarını açıklar.
services: automation
ms.service: automation
author: georgewallace
ms.author: gwallace
ms.date: 04/17/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 8c7dc256b92252793545336ffc45a987054a5509
ms.sourcegitcommit: c29d7ef9065f960c3079660b139dd6a8348576ce
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/13/2018
ms.locfileid: "35650857"
---
# <a name="azure-automation-scenario---automation-source-control-integration-with-github-enterprise"></a>Azure otomasyonu senaryosu - GitHub Enterprise ile Otomasyon kaynak denetimi tümleştirmesi

Otomasyon runbook'ların GitHub kaynak denetim deposu için Otomasyon hesabınızdaki ilişkilendirmenizi sağlar kaynak denetimi tümleştirmesi şu anda destekler. Ancak, dağıtan müşteriler [GitHub Enterprise](https://enterprise.github.com/home) kendi DevOps uygulamalarını desteklemek için ayrıca iş süreçlerini otomatikleştirin ve hizmet yönetimi işlemleri için geliştirilen runbook'ları ömrünü yönetmek için kullanmak istiyorsanız.

Bu senaryoda, veri merkezindeki karma Runbook çalışanı ile Azure Resource Manager modüllerini ve Git araçlarının yüklü olarak yapılandırılmış bir Windows bilgisayara sahip. Karma çalışan makinenin bir kopyası yerel Git deposu vardır. Karma çalışanında runbook çalıştırıldığında, Git dizin eşitlenir ve runbook dosya içeriğini Otomasyon hesabına aktarılır.

Bu makalede, Azure Otomasyonu ortamınızda bu yapılandırmayı ayarlamak açıklar. Otomasyon runbook'ları veri merkezinizde runbook'ları çalıştırmak ve GitHub Enterprise deponuzda runbook'ları eşitleyen erişmek için bu senaryo ve karma Runbook çalışanı dağıtımını desteklemek için gereken güvenlik kimlik bilgileri ile yapılandırarak Başlat Otomasyon hesabınıza.

## <a name="getting-the-scenario"></a>Senaryoyu alma

Bu senaryo, doğrudan aktarabileceğiniz iki PowerShell runbook'ları oluşan [Runbook Galerisi](automation-runbook-gallery.md) Azure portalında veya indirilerek [PowerShell Galerisi](https://www.powershellgallery.com).

### <a name="runbooks"></a>Runbook'lar

Runbook | Açıklama|
--------|------------|
Dışarı aktarma RunAsCertificateToHybridWorker | Böylece çalışan runbook'ları runbook'ları Otomasyon hesabına aktarın için Azure kimlik doğrulaması Runbook için bir karma çalışanı bir Otomasyon hesabının RunAs sertifika verir.|
Eşitleme LocalGitFolderToAutomationAccount | Runbook karma makine üzerinde yerel Git klasör eşitler ve ardından runbook dosyaları (*.ps1) Otomasyon hesabına aktarın.|

### <a name="credentials"></a>Kimlik Bilgileri

Kimlik Bilgisi | Açıklama|
-----------|------------|
GitHRWCredential | Kimlik bilgisi varlığı kullanıcı adı ve karma çalışanı izinlerine sahip bir kullanıcı için parolayı içerecek şekilde oluşturun.|

## <a name="installing-and-configuring-this-scenario"></a>Bu senaryoyu yükleme ve yapılandırma

### <a name="prerequisites"></a>Önkoşullar

1. Eşitleme LocalGitFolderToAutomationAccount runbook kullanarak kimliğini doğrular [Azure farklı çalıştır hesabı](automation-sec-configure-azure-runas-account.md).

2. Bir Log Analytics çalışma alanıyla etkinleştirilmiş ve yapılandırılmış bir Azure Otomasyon çözümü de gereklidir. Yükleme ve yapılandırma bu senaryo için kullanılan Otomasyon hesabı ile ilişkili olan bir yoksa, oluşturulur ve yürüttüğünüzde sizin için yapılandırılmış **yeni OnPremiseHybridWorker.ps1** karma runbook betiği alt.

    > [!NOTE]
    > Aşağıdaki bölgelerde yalnızca - Log Analytics ile Otomasyon tümleştirme desteklemekte **Avustralya Güneydoğu**, **Doğu ABD 2**, **Güneydoğu Asya**, ve  **Batı Avrupa**.

3. Adanmış karma Runbook GitHub yazılım barındıran çalışanı görev yapar ve runbook dosyaların bakımını yapar bir bilgisayar (*runbook*.ps1), GitHub ve Otomasyon eşitlemek için dosya sistemindeki bir kaynak dizin hesabı.

### <a name="import-and-publish-the-runbooks"></a>İçeri aktarma ve runbook'ları yayımlama

İçeri aktarmak için *dışarı aktarma RunAsCertificateToHybridWorker* ve *eşitleme LocalGitFolderToAutomationAccount* Runbook Galerisi, Azure portalında Otomasyon hesabınızdan runbook'ları izleyin yordamları [Runbook'u Galeriden Runbook alma](automation-runbook-gallery.md#to-import-a-runbook-from-the-runbook-gallery-with-the-azure-portal). Runbook'ları, Otomasyon hesabınıza başarıyla alındıktan sonra yayımlayın.

### <a name="deploy-and-configure-hybrid-runbook-worker"></a>Karma Runbook çalışanı'nı yapılandırma ve dağıtma

Veri merkezinizde zaten dağıtılmış bir karma Runbook çalışanı yoksa gereksinimlerini gözden geçirin ve Azure Otomasyon karma Runbook çalışanları - otomatikleştirmek yükleme ve yapılandırma için bu yordamı kullanarak otomatik yükleme adımları gerekir [Windows](automation-windows-hrw-install.md#automated-deployment) veya [Linux](automation-linux-hrw-install.md#installing-a-linux-hybrid-runbook-worker). Karma çalışanı bir bilgisayarda başarıyla yükledikten sonra bu senaryoyu desteklemek için yapılandırmasını tamamlamak için aşağıdaki adımları gerçekleştirin.

1. Yerel yönetici haklarına sahip bir hesapla karma Runbook çalışanı rolü barındıran bilgisayarda oturum açın, Git runbook dosyaları tutmak için bir dizin oluşturun. İç Git deposunu dizinine kopyalayın.
1. Sizin oluşturduğunuz bir farklı çalıştır hesabı yok veya yeni bir adanmış bu amaç için oluşturmak istediğiniz Azure portalından Automation hesapları gidin, Otomasyon hesabınızı seçin ve oluşturma bir [kimlik bilgisi varlığı](automation-credentials.md) , Kullanıcı adı ve parola karma çalışanı izinlerine sahip bir kullanıcı için içerir.
1. Otomasyon hesabınızdan [runbook'u düzenlemek](automation-edit-textual-runbook.md)**dışarı aktarma RunAsCertificateToHybridWorker** ve değişkeninin değerini değiştirme *$Password* güçlü bir parola ile.    Değer değiştirdikten sonra tıklayın **Yayımla** yayımlanan runbook'un taslak sürümünü sağlamak için.
1. Runbook'u başlatmak **dışarı aktarma RunAsCertificateToHybridWorker**hem de **Runbook'u Başlat** dikey penceresinde seçeneğinin altında **çalıştırma ayarları** seçeneğini  **Karma çalışanı** ve aşağı açılan listesinde, daha önce oluşturduğunuz bu senaryo için karma çalışanı grubu seçin.

    (Bu senaryo - Otomasyon hesabı runbook'ları içeri aktarma için belirli), Azure kaynaklarını yönetmek için farklı çalıştır bağlantısı kullanarak Azure ile çalışan can runbook'larda kimlik doğrulamasından geçmesini Bu karma çalışanı için bir sertifika verir.

1. Otomasyon hesabınızdan, daha önce oluşturduğunuz karma çalışanı grubu seçin ve [bir farklı çalıştır hesabı belirtebilirsiniz](automation-hrw-run-runbooks.md#runas-account) karma çalışanı grubu ve seçtiğiniz yalnızca veya önceden oluşturduğunuz kimlik bilgisi varlığı. Bu, eşitleme runbook Git komutlarını çalıştırabilirsiniz sağlar. 
1. Runbook'u başlatmak **eşitleme LocalGitFolderToAutomationAccount**, aşağıdaki gerekli giriş parametre değerlerini sağlayın ve **Runbook'u Başlat** dikey penceresinde seçeneğinin altında **çalıştırma ayarları**  seçeneğini **karma çalışanı** ve aşağı açılan listesinde, daha önce oluşturduğunuz bu senaryo için karma çalışanı grubu seçin:

   * *ResourceGroup* -kaynak grubunuzun adını, Otomasyon hesabı ile ilişkili
   * *AutomationAccountName* -Otomasyon hesabınızın adı
   * *GitPath* -yerel klasör veya dosya karma Runbook çalışanında nerede Git en son değişiklikler çekme üzere ayarlandı

    Bu karma çalışan bilgisayarda yerel Git klasörün eşitler ve Otomasyon hesabına'da kaynak dizinden .ps1 dosyaları alır.

1. Menüsünden seçim yaparak runbook işi Özet ayrıntılarını görüntüleyin **runbook'ları** dikey penceresinde, Otomasyon hesabı ve ardından **işleri** Döşe. Tamamlanmış başarıyla seçerek onaylayın **tüm günlükler** kutucuk ve ayrıntılı günlük akışı gözden geçirme.

## <a name="next-steps"></a>Sonraki adımlar

* Runbook türleri, avantajları ve sınırlamaları hakkında daha fazla bilgi için bkz. [Azure Automation runbook türleri](automation-runbook-types.md)
* PowerShell betik desteği özelliği hakkında daha fazla bilgi için bkz. [Azure Automation’da Yerel PowerShell betik desteği](https://azure.microsoft.com/blog/announcing-powershell-script-support-azure-automation-2/)
