---
title: Kaynak denetimi tümleştirmesi Azure automation'da - eski
description: Bu makalede, Azure automation'da GitHub ile kaynak denetimi tümleştirmesi açıklanır.
services: automation
ms.service: automation
ms.subservice: process-automation
author: georgewallace
ms.author: gwallace
ms.date: 04/01/2019
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: c95af40c3fa3f9dad2bfb5ea4a1b9f585c636928
ms.sourcegitcommit: 3341598aebf02bf45a2393c06b136f8627c2a7b8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/01/2019
ms.locfileid: "58807257"
---
# <a name="source-control-integration-in-azure-automation---legacy"></a>Azure Otomasyonu - eski kaynak denetimi tümleştirmesi

> [!NOTE]
> Kaynak denetimi için yeni bir deneyimi vardır. Yeni deneyim hakkında daha fazla bilgi için bkz. [kaynak denetimi (Önizleme)](source-control-integration.md).

Kaynak denetimi tümleştirmesi runbook'ların GitHub kaynak denetim deposu için Otomasyon hesabınızdaki ilişkilendirmenizi sağlar. Kaynak denetimi kolayca takımınızla işbirliği yapmanıza, değişiklikleri izlemek ve runbook'larınızı önceki sürümleri geri alma sağlar. Örneğin, kaynak denetimi Otomasyonu üretim geliştirme ortamınızda test kod yükseltmek kolaylaştıran geliştirme, test veya üretim Otomasyon hesaplarınız, kaynak denetimine farklı dalları eşitleme sağlar hesabı.

Kaynak denetimi, kod, kaynak denetimi için Azure Otomasyonu anında iletme veya runbook'larınızı kaynak denetiminden Azure automation'da çekme olanak tanır. Bu makalede, Azure Otomasyonu ortamınızdaki kaynak denetimi ayarlama açıklar. Azure Automation'ın GitHub deponuza erişmek ve aracılığıyla gerçekleştirilebilecek farklı işlemler açıklaması için yapılandırarak Başlat kaynak denetimi tümleştirmesini kullanarak. 

> [!NOTE]
> Kaynak denetimi destekler çekerek [PowerShell iş akışı runbook'ları](automation-runbook-types.md#powershell-workflow-runbooks) yanı [PowerShell runbook'ları](automation-runbook-types.md#powershell-runbooks). [Grafik runbook'ları](automation-runbook-types.md#graphical-runbooks) henüz desteklenmemektedir.

Bir GitHub hesabı zaten varsa, Otomasyon hesabınızı ve yalnızca biri için kaynak denetimi yapılandırmak için gereken iki basit adım vardır. Bunlar:

## <a name="step-1--create-a-github-repository"></a>1. adım – GitHub deposu oluşturma

Bir GitHub hesabı ve Azure Otomasyonu bağlamak istediğiniz bir depo zaten varsa, daha sonra mevcut hesabınızda oturum açın ve aşağıdaki 2. adımdaki başlatın. Aksi takdirde gidin [GitHub](https://github.com/), yeni bir hesap için kaydolun ve [yeni depo Oluştur](https://help.github.com/articles/create-a-repo/).

## <a name="step-2--set-up-source-control-in-azure-automation"></a>Adım 2 – Azure Otomasyonu'nda kaynak denetimi ayarlama

1. Otomasyon hesabı sayfasında Azure Portalı'nda altında **hesap ayarları**, tıklayın **kaynak denetimi.**

2. **Kaynak denetimi** sayfası açılır, GitHub hesabınızın ayrıntıları yapılandırabileceğiniz. Yapılandırılacak parametrelerin listesi aşağıdadır:  

   | **Parametre** | **Açıklama** |
   |:--- |:--- |
   | Kaynak Seç |Kaynağı seçin. Şu anda yalnızca **GitHub** desteklenir. |
   | Yetkilendirme |Tıklayın **Authorize** GitHub deponuza Azure Otomasyonu erişim vermek düğmesi. Ardından, zaten başka bir pencerede GitHub hesabınızda oturum açtıysanız, o hesabın kimlik bilgileri kullanılır. Yetkilendirme başarılı olduktan sonra sayfanın altında GitHub kullanıcı adınızı gösterecektir **yetkilendirme özelliği**. |
   | Depo seçin |Bir GitHub deposuna kullanılabilir depoları listesinden seçin. |
   | Şube seçin |Bir dal kullanılabilir dalların listesinden seçin. Yalnızca **ana** dal, herhangi bir dal oluşturmadıysanız, gösterilir. |
   | Runbook klasörünün yolu |Runbook klasörünün yolu İtme veya çekme kodunuzu istediğiniz GitHub deposunda yolunu belirtir. Şu biçimde girilmelidir **/KlasörAdı/altklasöradı**. Yalnızca runbook klasörünün yolu runbook Otomasyon hesabınıza eşitlenecektir. Runbook klasörü yolunun alt klasörlerdeki runbook'ları olacak **değil** senkronize edilir. Kullanım **/** deposu altındaki tüm runbook'lar eşitlenecek. |
3. Örneğin, adında bir havuz varsa **PowerShellScripts** adlı bir klasör içeren **RootFolder**, adlı bir klasör içeren **alt**. Aşağıdaki dizelerden her klasör düzeyinde eşitlemek için kullanabilirsiniz:

   1. Eşitleme runbook'ları için **depo**, runbook klasörü yolu */*
   2. Eşitleme runbook'ları için **RootFolder**, runbook klasörü yolu   */RootFolder*
   3. Eşitleme runbook'ları için **alt**, runbook klasörü yolu */RootFolder/alt*.
4. Parametreleri yapılandırdıktan sonra bunlar üzerinde görüntülenen **kaynak denetimi ayarlama** sayfası.  

    ![Kaynak denetimi sayfası ayarlar gösteriliyor](media/source-control-integration-legacy/automation-SourceControlConfigure.png)
5. ' A tıkladığınızda **Tamam**, kaynak denetimi tümleştirmesi artık Automation hesabınız için yapılandırılan ve GitHub bilgilerinizle güncelleştirilmelidir. Artık tüm kaynak denetimini eşitleme iş geçmişini görüntülemek için bu bölümü de tıklayabilirsiniz.  

    ![Geçerli yapılandırılmış kaynak denetimi yapılandırması için değerler](media/source-control-integration-legacy/automation-RepoValues.png)
6. Kaynak denetimi, ayarladıktan sonra iki [değişken varlıkları](automation-variables.md) Otomasyon hesabınızda oluşturulur. Buna ek olarak, yetkili uygulama GitHub hesabınıza eklenir.

   * Değişken **Microsoft.Azure.Automation.SourceControl.Connection** aşağıda gösterildiği gibi bağlantı dizesi değerlerini içerir.  

     | **Parametre** | **Değer** |
     |:--- |:--- |
     | `Name`  |Microsoft.Azure.Automation.SourceControl.Connection |
     | `Type`  |String |
     | `Value` |{"Dal":\<*dal adınızı*>, "RunbookFolderPath":\<*Runbook klasörünün yolu*>, "Sağlayıcı türü":\<*1'için ' e kadar bir değere sahip GitHub*>, "Depo":\<*deponuzun adını*>, "Username":\<*bilgisayarınızı GitHub kullanıcı adı*>} |

   * Değişken **Microsoft.Azure.Automation.SourceControl.OAuthToken**, sizin OAuthToken güvenli şifreli değerini içerir.  

     |**Parametre**            |**Değer** |
     |:---|:---|
     | `Name`  | Microsoft.Azure.Automation.SourceControl.OAuthToken |
     | `Type`  | Unknown(Encrypted) |
     | `Value` | <*Şifrelenmiş OAuthToken*> |  

     ![Kaynak Denetim değişkenleri gösteren bir pencere](media/source-control-integration-legacy/automation-Variables.png)  

   * **Kaynak denetimi Otomasyonu** GitHub hesabınıza yetkili bir uygulama olarak eklenir. Uygulamayı görüntülemek için: GitHub giriş sayfanızdan gidin, **profili** > **ayarları** > **uygulamaları**. Bu uygulama, bir Otomasyon hesabı GitHub deponuza eşitleme Azure Otomasyonu sağlar.  

     ![Github'da uygulama ayarları](media/source-control-integration-legacy/automation-GitApplication.png)

## <a name="using-source-control-in-automation"></a>Kaynak denetimi Otomasyonu kullanma

### <a name="check-in-a-runbook-from-azure-automation-to-source-control"></a>Azure Otomasyonu runbook kaynak denetimine iade edin

Runbook iade, Azure automation'da bir runbook için kaynak denetimi deponuzun yaptığınız değişiklikleri göndermek sağlar. Bir runbook'u iade için adımları aşağıda verilmiştir:

1. Otomasyon hesabınızdan [metinsel yeni runbook Oluştur](automation-first-runbook-textual.md), veya [var olan ve metinsel bir runbook'u düzenlemek](automation-edit-textual-runbook.md). Bu runbook, PowerShell iş akışı veya bir PowerShell Betiği runbook olabilir.  
2. Runbook'unuzda düzenledikten sonra dosyayı kaydedin ve tıklayın **iade** gelen **Düzenle** sayfası.  

    ![İade etme GitHub düğmesini gösteren bir pencere](media/source-control-integration-legacy/automation-CheckinButton.png)

     > [!NOTE] 
     > Azure Otomasyonu iadesi, kaynak denetiminde var olan kod üzerine yazar. Git iade için komut satırı eşdeğer yönergesi **git Ekle + git işlemesi + git itme**  

3. Tıkladığınızda **iade**, bir onay iletisi istenir, tıklayın **Evet** devam etmek için.  

    ![Kaynak denetimine iade onaylayan bir iletişim kutusu](media/source-control-integration-legacy/automation-CheckinMessage.png)
4. İade kaynak denetimi runbook başlatır: **Sync-MicrosoftAzureAutomationAccountToGitHubV1**. Bu runbook Github'a bağlanır ve değişiklikleri Azure Otomasyonu deponuza gönderir. İşaretli iş geçmişinde görüntülemek üzere geri dön **kaynak denetimi tümleştirmesi** sekmesini ve depo eşitleme sayfasını açmak için tıklayın. Bu sayfayı tüm kaynak denetim işlerinizi gösterir.  Ayrıntılarını görüntülemek için tıklayın ve görüntülemek istediğiniz işi seçin.  

    ![Bir eşitleme işi sonuçlarını gösteren bir pencere](media/source-control-integration-legacy/automation-CheckinRunbook.png)

   > [!NOTE]
   > Kaynak denetimi runbook'ları görüntülemek veya düzenlemek silemeyeceğiniz özel Otomasyon runbook'ları var. Bunlar, runbook listede gösterilmez, ancak işler listesi Eşitleme işleri bakın.

5. Değiştirilen runbook'un adına iade edilmiş runbook için bir giriş parametresi olarak gönderilir. Yapabilecekleriniz [iş ayrıntılarını görüntüleme](automation-runbook-execution.md#viewing-job-status-from-the-azure-portal) runbook'ta genişleterek **depo eşitleme** sayfası.  

    ![Bir eşitleme işi için giriş gösteren bir pencere](media/source-control-integration-legacy/automation-CheckinInput.png)
6. Değişiklikleri görüntülemek için iş tamamlandıktan sonra GitHub deponuzda yenileyin.  Bir işleme iletisi deponuzdaki bir işleme olmalıdır: **Güncelleştirilmiş *Runbook adı* Azure automation'da.**  

### <a name="sync-runbooks-from-source-control-to-azure-automation"></a>Kaynak denetiminden eşitleme runbook'lar için Azure Otomasyonu

Depo eşitleme sayfasında Eşitle düğmesi, tüm runbook'ları runbook klasörünün yolu deponuzun Otomasyon hesabınıza çekme sağlar. Aynı depodaki birden fazla Otomasyon hesabına eşitlenebilir. Bir runbook eşitleme adımlarını aşağıda verilmiştir:

1. Kaynak denetimi ayarladığınız Otomasyon hesabından **kaynak denetimi tümleştirmesi/depo eşitleme** sayfasında ve tıklayın **eşitleme**.  Bir onay iletisi ile istenir tıklayın **Evet** devam etmek için.  

    ![Tüm runbook'ları onaylayan bir ileti ile Eşitle düğmesi eşitlenir](media/source-control-integration-legacy/automation-SyncButtonwithMessage.png)

2. Eşitleme, bir runbook başlatır: **Eşitleme MicrosoftAzureAutomationAccountFromGitHubV1**. Bu runbook Github'a bağlanır ve değişiklikleri deponuzdan Azure Otomasyonu'na ekleme çeker. Yeni bir iş görmeniz gerekir **depo eşitleme** Bu eylem için sayfa. Eşitleme işi hakkındaki ayrıntıları görüntülemek için iş Ayrıntılar sayfasını açmak için tıklayın.  

    ![Bir GitHub deposuna bir eşitleme işi eşitleme sonuçlarını gösteren bir pencere](media/source-control-integration-legacy/automation-SyncRunbook.png)

    > [!NOTE]
    > Kaynak denetiminden bir eşitleme şu anda Automation hesabınız için mevcut runbook'ları taslak sürümünü üzerine yazar **tüm** olan runbook'ları kaynak denetimi. Git eşitlemek için komut satırı eşdeğer yönergesi **git çekme**

![Tüm bekleyen kaynak denetim eşitleme işi günlüklerinden gösteren bir pencere](media/source-control-integration-legacy/automation-AllLogs.png)

## <a name="disconnecting-source-control"></a>Kaynak denetiminin bağlantısı kesiliyor

GitHub hesabınızın bağlantısını kesmek için depo eşitleme sayfasını açın ve **Bağlantıyı Kes**. Kaynak denetiminin bağlantısını kesmek sonra runbook'ları önceden eşitlenmiş hala Otomasyon hesabınızda kalır, ancak depo eşitleme sayfasında etkin olmayacak.  

  ![Kaynak denetimi bağlantısını kesmek için bağlantıyı kes düğmesi gösteren bir pencere](media/source-control-integration-legacy/automation-Disconnect.png)

## <a name="next-steps"></a>Sonraki adımlar

Kaynak denetimi tümleştirmesi hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:  

* [Azure Otomasyonu: Azure Otomasyonu'nda kaynak denetimi tümleştirmesi](https://azure.microsoft.com/blog/azure-automation-source-control-13/)  
* [Sık kullandığınız kaynak denetimi sisteminiz için oy verin](https://www.surveymonkey.com/r/?sm=2dVjdcrCPFdT0dFFI8nUdQ%3d%3d)  
* [Azure Otomasyonu: Azure DevOps kullanarak Runbook kaynak denetimine tümleştirme](https://azure.microsoft.com/blog/azure-automation-integrating-runbook-source-control-using-visual-studio-online/)  
