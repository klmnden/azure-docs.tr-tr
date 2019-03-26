---
title: Kaynak denetimi tümleştirmesi Azure automation'da - eski
description: Bu makalede, Azure automation'da GitHub ile kaynak denetimi tümleştirmesi açıklanır.
services: automation
ms.service: automation
ms.subservice: process-automation
author: georgewallace
ms.author: gwallace
ms.date: 09/25/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: da9b82b1e17a62aa9b3d606b0b16295acf04eb85
ms.sourcegitcommit: 72cc94d92928c0354d9671172979759922865615
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/25/2019
ms.locfileid: "58418763"
---
# <a name="source-control-integration-in-azure-automation---legacy"></a>Azure Otomasyonu - eski kaynak denetimi tümleştirmesi

> [!NOTE]
> Kaynak denetimi için yeni bir deneyimi vardır. Yeni deneyim hakkında daha fazla bilgi için bkz. [kaynak denetimi (Önizleme)](source-control-integration.md).

Kaynak denetimi tümleştirmesi runbook'ların GitHub kaynak denetim deposu için Otomasyon hesabınızdaki ilişkilendirmenizi sağlar. Kaynak denetimi kolayca takımınızla işbirliği yapmanıza, değişiklikleri izlemek ve runbook'larınızı önceki sürümleri geri alma sağlar. Örneğin, kaynak denetimi Otomasyonu üretim geliştirme ortamınızda test kod yükseltmek kolaylaştıran geliştirme, test veya üretim Otomasyon hesaplarınız, kaynak denetimine farklı dalları eşitleme sağlar hesabı.

Kaynak denetimi, kod, kaynak denetimi için Azure Otomasyonu anında iletme veya runbook'larınızı kaynak denetiminden Azure automation'da çekme olanak tanır. Bu makalede, Azure Otomasyonu ortamınızdaki kaynak denetimi ayarlama açıklar. Azure Automation'ın GitHub deponuza erişmek ve aracılığıyla gerçekleştirilebilecek farklı işlemler açıklaması için yapılandırarak Başlat kaynak denetimi tümleştirmesini kullanarak. 

> [!NOTE]
> Kaynak denetimi destekler çekerek [PowerShell iş akışı runbook'ları](automation-runbook-types.md#powershell-workflow-runbooks) yanı [PowerShell runbook'ları](automation-runbook-types.md#powershell-runbooks). [Grafik runbook'ları](automation-runbook-types.md#graphical-runbooks) henüz desteklenmemektedir.<br><br>
> 
> 

Bir GitHub hesabı zaten varsa, Otomasyon hesabınızı ve yalnızca biri için kaynak denetimi yapılandırmak için gereken iki basit adım vardır. Bunlar:

## <a name="step-1--create-a-github-repository"></a>1. adım – GitHub deposu oluşturma
Bir GitHub hesabı ve Azure Otomasyonu bağlamak istediğiniz bir depo zaten varsa, daha sonra mevcut hesabınızda oturum açın ve aşağıdaki 2. adımdaki başlatın. Aksi takdirde gidin [GitHub](https://github.com/), yeni bir hesap için kaydolun ve [yeni depo Oluştur](https://help.github.com/articles/create-a-repo/).

## <a name="step-2--set-up-source-control-in-azure-automation"></a>Adım 2 – Azure Otomasyonu'nda kaynak denetimi ayarlama
1. Otomasyon hesabı sayfasında Azure Portalı'nda altında **hesap ayarları**, tıklayın **kaynak denetimi.** 
   
1. **Kaynak denetimi** sayfası açılır, GitHub hesabınızın ayrıntıları yapılandırabileceğiniz. Yapılandırılacak parametrelerin listesi aşağıdadır:  
   
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
   
    ![Kaynak denetimi sayfası yapılandırma](media/automation-source-control-integration-legacy/automation_02_SourceControlConfigure.png)
5. ' A tıkladığınızda **Tamam**, kaynak denetimi tümleştirmesi artık Automation hesabınız için yapılandırılan ve GitHub bilgilerinizle güncelleştirilmelidir. Artık tüm kaynak denetimini eşitleme iş geçmişini görüntülemek için bu bölümü de tıklayabilirsiniz.  
   
    ![Depo değerleri](media/automation-source-control-integration-legacy/automation_03_RepoValues.png)
6. Kaynak denetimi ayarladıktan sonra Otomasyon hesabınızda aşağıdaki Otomasyon kaynakları oluşturulur:  
   İki [değişken varlıkları](automation-variables.md) oluşturulur.  
   
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

     ![Değişkenler](media/automation-source-control-integration-legacy/automation_04_Variables.png)  

     * **Kaynak denetimi Otomasyonu** GitHub hesabınıza yetkili bir uygulama olarak eklenir. Uygulamayı görüntülemek için: GitHub giriş sayfanızdan gidin, **profili** > **ayarları** > **uygulamaları**. Bu uygulama, bir Otomasyon hesabı GitHub deponuza eşitleme Azure Otomasyonu sağlar.  

     ![Git uygulama](media/automation-source-control-integration-legacy/automation_05_GitApplication.png)


## <a name="using-source-control-in-automation"></a>Kaynak denetimi Otomasyonu kullanma
### <a name="check-in-a-runbook-from-azure-automation-to-source-control"></a>Azure Otomasyonu runbook kaynak denetimine iade edin
Runbook iade, Azure automation'da bir runbook için kaynak denetimi deponuzun yaptığınız değişiklikleri göndermek sağlar. Bir runbook'u iade için adımları aşağıda verilmiştir:

1. Otomasyon hesabınızdan [metinsel yeni runbook Oluştur](automation-first-runbook-textual.md), veya [var olan ve metinsel bir runbook'u düzenlemek](automation-edit-textual-runbook.md). Bu runbook, PowerShell iş akışı veya bir PowerShell Betiği runbook olabilir.  
2. Runbook'unuzda düzenledikten sonra dosyayı kaydedin ve tıklayın **iade** gelen **Düzenle** sayfası.  
   
    ![İade Et düğmesi](media/automation-source-control-integration-legacy/automation_06_CheckinButton.png)

     > [!NOTE] 
     > Azure Otomasyonu iadesi, kaynak denetiminde var olan kod üzerine yazar. Git iade için komut satırı eşdeğer yönergesi **git Ekle + git işlemesi + git itme**  

1. Tıkladığınızda **iade**, bir onay iletisi istenir, tıklayın **Evet** devam etmek için.  
   
    ![İade Etme İletisi](media/automation-source-control-integration-legacy/automation_07_CheckinMessage.png)
2. İade kaynak denetimi runbook başlatır: **Sync-MicrosoftAzureAutomationAccountToGitHubV1**. Bu runbook Github'a bağlanır ve değişiklikleri Azure Otomasyonu deponuza gönderir. İşaretli iş geçmişinde görüntülemek üzere geri dön **kaynak denetimi tümleştirmesi** sekmesini ve depo eşitleme sayfasını açmak için tıklayın. Bu sayfayı tüm kaynak denetim işlerinizi gösterir.  Ayrıntılarını görüntülemek için tıklayın ve görüntülemek istediğiniz işi seçin.  
   
    ![Runbook iade etme](media/automation-source-control-integration-legacy/automation_08_CheckinRunbook.png)
   
   > [!NOTE]
   > Kaynak denetimi runbook'ları görüntülemek veya düzenlemek silemeyeceğiniz özel Otomasyon runbook'ları var. Bunlar, runbook listede gösterilmez, ancak işler listesi Eşitleme işleri bakın.
   > 
   > 
3. Değiştirilen runbook'un adına iade edilmiş runbook için bir giriş parametresi olarak gönderilir. Yapabilecekleriniz [iş ayrıntılarını görüntüleme](automation-runbook-execution.md#viewing-job-status-from-the-azure-portal) runbook'ta genişleterek **depo eşitleme** sayfası.  
   
    ![İade etme giriş](media/automation-source-control-integration-legacy/automation_09_CheckinInput.png)
4. Değişiklikleri görüntülemek için iş tamamlandıktan sonra GitHub deponuzda yenileyin.  Bir işleme iletisi deponuzdaki bir işleme olmalıdır: **Güncelleştirilmiş *Runbook adı* Azure automation'da.**  

### <a name="sync-runbooks-from-source-control-to-azure-automation"></a>Kaynak denetiminden eşitleme runbook'lar için Azure Otomasyonu
Depo eşitleme sayfasında Eşitle düğmesi, tüm runbook'ları runbook klasörünün yolu deponuzun Otomasyon hesabınıza çekme sağlar. Aynı depodaki birden fazla Otomasyon hesabına eşitlenebilir. Bir runbook eşitleme adımlarını aşağıda verilmiştir:

1. Kaynak denetimi ayarladığınız Otomasyon hesabından **kaynak denetimi tümleştirmesi/depo eşitleme** sayfasında ve tıklayın **eşitleme**.  Bir onay iletisi ile istenir tıklayın **Evet** devam etmek için.  
   
    ![Eşitleme düğmesi](media/automation-source-control-integration-legacy/automation_10_SyncButtonwithMessage.png)
2. Eşitleme, bir runbook başlatır: **Eşitleme MicrosoftAzureAutomationAccountFromGitHubV1**. Bu runbook Github'a bağlanır ve değişiklikleri deponuzdan Azure Otomasyonu'na ekleme çeker. Yeni bir iş görmeniz gerekir **depo eşitleme** Bu eylem için sayfa. Eşitleme işi hakkındaki ayrıntıları görüntülemek için iş Ayrıntılar sayfasını açmak için tıklayın.  
   
    ![Eşitleme Runbook](media/automation-source-control-integration-legacy/automation_11_SyncRunbook.png)

    > [!NOTE] 
    > Kaynak denetiminden bir eşitleme şu anda Automation hesabınız için mevcut runbook'ları taslak sürümünü üzerine yazar **tüm** olan runbook'ları kaynak denetimi. Git eşitlemek için komut satırı eşdeğer yönergesi **git çekme**

![AllLogs görüntüsü](media/automation-source-control-integration-legacy/automation_13_AllLogs.png)

## <a name="disconnecting-source-control"></a>Kaynak denetiminin bağlantısı kesiliyor
GitHub hesabınızın bağlantısını kesmek için depo eşitleme sayfasını açın ve **Bağlantıyı Kes**. Kaynak denetiminin bağlantısını kesmek sonra runbook'ları önceden eşitlenmiş hala Otomasyon hesabınızda kalır, ancak depo eşitleme sayfasında etkin olmayacak.  

  ![Düğme bağlantısını kes](media/automation-source-control-integration-legacy/automation_12_Disconnect.png)

## <a name="next-steps"></a>Sonraki adımlar
Kaynak denetimi tümleştirmesi hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:  

* [Azure Otomasyonu: Azure Otomasyonu'nda kaynak denetimi tümleştirmesi](https://azure.microsoft.com/blog/azure-automation-source-control-13/)  
* [Sık kullandığınız kaynak denetimi sisteminiz için oy verin](https://www.surveymonkey.com/r/?sm=2dVjdcrCPFdT0dFFI8nUdQ%3d%3d)  
* [Azure Otomasyonu: Azure DevOps kullanarak Runbook kaynak denetimine tümleştirme](https://azure.microsoft.com/blog/azure-automation-integrating-runbook-source-control-using-visual-studio-online/)  


