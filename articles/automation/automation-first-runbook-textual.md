---
title: Azure automation'da ilk PowerShell iş akışı runbook Uygulamam
description: PowerShell İş Akışı kullanarak basit bir runbook oluşturma, test etme ve yayımlama adımlarını anlatan öğretici.
keywords: powershell iş akışı, powershell iş akışı örnekleri, iş akışı powershell
services: automation
ms.service: automation
ms.subservice: process-automation
author: georgewallace
ms.author: gwallace
ms.date: 09/24/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 1697f479cf013f2ef94dd5a8a2fc637d72e6e18a
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60740016"
---
# <a name="my-first-powershell-workflow-runbook"></a>İlk PowerShell İş Akışı runbook uygulamam

> [!div class="op_single_selector"]
> * [Grafik](automation-first-runbook-graphical.md)
> * [PowerShell](automation-first-runbook-textual-powershell.md)
> * [PowerShell İş Akışı](automation-first-runbook-textual.md)
> * [Python](automation-first-runbook-textual-python2.md)

Bu öğretici, Azure Automation’da bir [PowerShell İş Akışı runbook](automation-runbook-types.md#powershell-workflow-runbooks) oluşturulmasını adım adım göstermektedir. Test ve yayımlama açıklarken runbook işinin durumunu izlemek nasıl basit bir runbook ile başlayın. Ardından, bir Azure sanal makinesini başlatmayı içeren bir örnekle, bu runbook’u gerçekten Azure kaynaklarını yönetmek üzere değiştirin. Son olarak, runbook'u daha sağlam runbook parametreleri ekleyerek olun.

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi tamamlamak için aşağıdakiler gerekir:

* Azure aboneliği. Henüz bir aboneliğiniz yoksa [MSDN abone avantajlarınızı etkinleştirebilir](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) ya da [ücretsiz hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) için kaydolabilirsiniz.
* Runbook’u tutacak ve Azure kaynaklarında kimlik doğrulamasını yapacak bir [Automation hesabı](automation-offering-get-started.md).  Bu hesabın sanal makineyi başlatma ve durdurma izni olmalıdır.
* Azure sanal makinesi. Üretime yönelik bir VM olmaması için bu makineyi durdurup.

## <a name="step-1---create-new-runbook"></a>1. Adım - Yeni runbook oluşturma

Metnini veren basit bir runbook oluşturmaya başlayın *Hello World*.

1. Azure portalında, Otomasyon hesabınızı açın.

   Automation hesabı sayfası size bu hesaptaki kaynakların hızlı bir görünümünü sağlar. Birkaç varlığınız zaten olmalıdır. Bu varlıkların çoğu, yeni bir Otomasyon hesabına otomatik olarak dahil edilen modüllerdir. Burada ayrıca [önkoşullarda](#prerequisites) belirtilen Kimlik Bilgileri varlığınız da bulunmalıdır.

1. Tıklayın **runbook'ları** altında **süreç otomasyonu** runbook'ların listesini açmak için.
1. Üzerine tıklayarak yeni bir runbook oluşturmak **+ runbook Ekle** düğmesine ve ardından **yeni bir runbook oluşturmak**.
1. Runbook’a *MyFirstRunbook-Workflow* adını verin.
1. Bu durumda, oluşturacağız bir [PowerShell iş akışı runbook'u](automation-runbook-types.md#powershell-workflow-runbooks) seçin **Powershell iş akışı** için **Runbook türü**.
1. Runbook’u oluşturmak için **Oluştur**’a tıklayın ve metin düzenleyicisini açın.

## <a name="step-2---add-code-to-the-runbook"></a>2. Adım - Runbook'a kod ekleme

Kodu doğrudan runbook’a yazabilir veya Kitaplık denetiminde cmdlet’leri, runbook’ları ve varlıkları seçebilir ve ilgili parametrelerle bunların runbook’a eklenmesini sağlayabilirsiniz. Bu kılavuzda doğrudan runbook'a yazın.

1. Runbook uygulamanızı yalnızca gerekli şu anda boş *iş akışı* anahtar sözcüğü, iş akışının tamamını encases runbook'unuzu ve küme ayraçları adını.

   ```powershell-interactive
   Workflow MyFirstRunbook-Workflow
   {
   }
   ```

1. Ayraçlar arasına *Write-Output "Hello World."* yazın.

   ```powershell-interactive
   Workflow MyFirstRunbook-Workflow
   {
   Write-Output "Hello World"
   }
   ```

1. **Kaydet**’e tıklayarak runbook’u kaydedin.

## <a name="step-3---test-the-runbook"></a>3. Adım - Runbook'u test etme

Runbook’u üretimde kullanılabilir hale getirmek üzere yayımlamadan önce düzgün çalıştığından emin olmak için test etmeniz gerekir. Bir runbook'u test ettiğinizde, bunun **Taslak** sürümünü çalıştırır ve çıktısını etkileşimli olarak görüntülersiniz.

1. Test bölmesini açmak için **Test bölmesi**’ne tıklayın.
1. Testi başlatmak için **Başlat**’a tıklayın. Bu seçenek, etkinleştirilen tek seçenek olmalıdır.
1. Bir [runbook işi](automation-runbook-execution.md) oluşturulur ve durumu görüntülenir.

   İş durumu olarak başlar *sıraya alınan* başlatılabilmek bir runbook çalışanının bulutta gelmesinin beklendiğini gösteren. Bu taşınır *başlangıç* bir çalışan işi talep ettiğinde ve ardından *çalıştıran* runbook gerçekten çalışmaya başladığında.  

1. Runbook işi tamamlandığında çıktısı görüntülenir. Sizin durumunuzda görmelisiniz *Hello World*.

   ![Hello World](media/automation-first-runbook-textual/test-output-hello-world.png)

1. Tuvale geri dönmek için Test bölmesini kapatın.

## <a name="step-4---publish-and-start-the-runbook"></a>4. Adım - Runbook’u yayımlama ve başlatma

Oluşturduğunuz runbook hala Taslak modundadır. Üretimde çalıştırılabilmesi için önce yayınlamanız gerekir. Bir runbook yayımladığınızda, Taslak sürümü mevcut Yayımlanmış sürümün üzerine yazarsınız. Henüz runbook yalnızca oluşturduğundan sizin durumunuzda, yayımlanmış sürümünüz yok.

1. Runbook’u yayımlamak için **Yayımla**’ya tıklayın ve sorulduğunda **Evet**’e tıklayın.
1. Runbook'ta görüntülemek için sola kaydırırsanız **runbook'ları** bölmesi artık gösterir bir **yazma durumu** , **yayımlanan**.
1. **MyFirstRunbook-Workflow** için bölmeyi görüntülemek üzere geri sağa kaydırın.  
   Üst kısımdaki seçenekler runbook’u başlatmamıza, gelecekte bir zamanda başlatmak üzere zamanlamamıza ya da bir HTTP çağrısıyla başlatılabilmesi için [web kancası](automation-webhooks.md) oluşturmamıza olanak tanır.
1. runbook'u başlatmak istediğiniz yaptığımıza **Başlat** ardından **Evet** istendiğinde.

   ![Runbook’u başlatma](media/automation-first-runbook-textual/automation-runbook-controls-start.png)

1. Oluşturduğunuz runbook işi için bir iş bölmesi açılır. Bu bölme kapatılabilir, ancak işin ilerleme durumunu izleyebilmek için bu durumda, açık bırakın.
1. İş durumu gösterilen **iş özeti** ve ne zaman, runbook test gördüğümüz durumların aynısıdır.

   ![İş Özeti](media/automation-first-runbook-textual/job-pane-status-blade-jobsummary.png)

1. Runbook durumu olarak *Tamamlandı* gösterilince **Çıktı**’ya tıklayın. Çıktı bölmesi açılır ve gördüğünüz, *Hello World*.

   ![İş Özeti](media/automation-first-runbook-textual/job-pane-status-blade-outputtile.png)  

1. Çıktı bölmesini kapatın.
1. Runbook işine ait Akışlar bölmesini açmak için **Tüm Günlükler**’e tıklayın. yalnızca görmelisiniz *Hello World* çıkış akışı, ancak bu görünüm ayrıntılı ve hata gibi runbook işine yönelik diğer akışlar runbook bunlara yazıyorsa gösterilebilir.

   ![İş Özeti](media/automation-first-runbook-textual/job-pane-status-blade-alllogstile.png)

1. Akışları sayfası ve MyFirstRunbook sayfasına geri dönmek için iş sayfasına kapatın.
1. Tıklayın **işleri** bu runbook için işleri sayfasını açın. Bu sayfada tüm bu runbook tarafından oluşturulan işler listelenir. Yalnızca bir işin, yalnızca bir kez çalıştırılan işe listelendiğini görmeniz gerekir.

   ![İşler](media/automation-first-runbook-textual/runbook-control-job-tile.png)

1. Runbook'u başlattığımızda, görüntülediğiniz iş sayfasını açmak için bu işe tıklayabilirsiniz. Bu eylem, zaman içinde geri dönün ve belirli bir runbook için oluşturulan herhangi bir işin ayrıntılarını görüntülemek sağlar.

## <a name="step-5---add-authentication-to-manage-azure-resources"></a>5. Adım- Azure kaynaklarını yönetmek için kimlik doğrulaması ekleme

Runbook uygulamanızı test ettiniz ve yayımladınız, ancak şu ana kadar faydalı bir şey yapmadı. Bu runbook’un Azure kaynaklarını yönetmesini istiyorsunuz. Başvurulan kimlik bilgilerini kullanarak kimlik doğrulaması yaptınız sürece, ancak bunu yapamazsınız [önkoşulları](#prerequisites). Bunu şu şekilde **Connect-AzureRmAccount** cmdlet'i.

1. MyFirstRunbook İş Akışı bölmesinde **Düzenle**’ye tıklayarak metin düzenleyicisini açın.
2. İhtiyacınız olmayan **Write-Output** satırı artık, bu nedenle bir tane silin.
3. İmleci ayraçlar arasında boş bir satıra getirin.
4. Automation Farklı Çalıştır hesabınızla kimlik doğrulamasını işleyecek aşağıdaki kodu yazın veya kopyalayıp yapıştırın:

   ```powershell-interactive
   # Ensures you do not inherit an AzureRMContext in your runbook
   Disable-AzureRmContextAutosave –Scope Process

   $Conn = Get-AutomationConnection -Name AzureRunAsConnection
   Connect-AzureRmAccount -ServicePrincipal -Tenant $Conn.TenantID `
   -ApplicationId $Conn.ApplicationID -CertificateThumbprint $Conn.CertificateThumbprint

   $AzureContext = Select-AzureRmSubscription -SubscriptionId $Conn.SubscriptionID
   ```

   > [!IMPORTANT]
   > **Add-AzureRmAccount** ve **Login-AzureRmAccount** artık diğer adlarıdır **Connect-AzureRMAccount**. Varsa **Connect-AzureRMAccount** cmdlet'i mevcut değil, kullanabileceğiniz **Add-AzureRmAccount** veya **Login-AzureRmAccount**, veya [modüllerinizi güncelleştir ](automation-update-azure-modules.md) Otomasyon hesabınızda en son sürümlerine.

> [!NOTE]
> Gerekebilir [modüllerinizi güncelleştirme](automation-update-azure-modules.md) karşın yalnızca yeni bir Otomasyon hesabı oluşturdunuz.

1. Tıklayın **Test bölmesi** böylece runbook'u test edebilirsiniz.
1. Testi başlatmak için **Başlat**’a tıklayın. Tamamlandığında, hesabınızdaki temel bilgileri görüntüleyen bir aşağıdakine benzer bir çıktı almalısınız. Bu eylem, kimlik bilgisinin geçerli olduğunu onaylar.

   ![Kimlik doğrulaması](media/automation-first-runbook-textual/runbook-auth-output.png)

## <a name="step-6---add-code-to-start-a-virtual-machine"></a>6. Adım - Sanal makineyi başlatmak için kod ekleme

Azure aboneliğinize, runbook kimlik doğrulaması, kaynakları yönetebilir. Bir sanal makineyi başlatmak için bir komut ekleyin. Azure aboneliğinizdeki herhangi bir sanal makineyi seçebilir ve şimdilik bu adı cmdlet'e kod işiniz. Kaynakları birden çok farklı abonelikler arasında yönetiyorsanız, kullanmanız gerekir **- AzureRmContext** parametresi ile birlikte [Get-AzureRmContext](/powershell/module/azurerm.profile/get-azurermcontext).

1. Sonra *Connect-AzureRmAccount*, türü *Start-AzureRmVM-Name 'VMName' - ResourceGroupName 'NameofResourceGroup'* adı ve sanal makinenin kaynak grubu adı sağlar.  

   ```powershell-interactive
   workflow MyFirstRunbook-Workflow
   {
   # Ensures you do not inherit an AzureRMContext in your runbook
   Disable-AzureRmContextAutosave –Scope Process

   $Conn = Get-AutomationConnection -Name AzureRunAsConnection
   Connect-AzureRmAccount -ServicePrincipal -Tenant $Conn.TenantID -ApplicationId $Conn.ApplicationID -CertificateThumbprint $Conn.CertificateThumbprint

   $AzureContext = Select-AzureRmSubscription -SubscriptionId $Conn.SubscriptionID

   Start-AzureRmVM -Name 'VMName' -ResourceGroupName 'ResourceGroupName' -AzureRmContext $AzureContext
   }
   ```

1. Runbook'u kaydedin ve ardından **Test bölmesi** böylece test edebilirsiniz.
1. Testi başlatmak için **Başlat**’a tıklayın. Tamamlandığında, sanal makinenin başlatıldığını kontrol edin.

## <a name="step-7---add-an-input-parameter-to-the-runbook"></a>7. Adım - Runbook'a girdi parametresi ekleme

runbook'unuzda sanal makine şu anda runbook'ta o, kodlanmış başlatmaz, bunun runbook başlatılırken sanal makineyi belirtebilirsiniz daha kullanışlı olurdu. Bu işlevi sağlamak için runbook'a girdi parametreleri ekleyin.

1. Runbook’a *VMName* ve *ResourceGroupName* parametrelerini ekleyin ve bu değişkenleri aşağıdaki örnekteki gibi **Start-AzureRmVM** cmdlet’i ile kullanın.

   ```powershell-interactive
   workflow MyFirstRunbook-Workflow
   {
    Param(
     [string]$VMName,
     [string]$ResourceGroupName
    )  
   # Ensures you do not inherit an AzureRMContext in your runbook
   Disable-AzureRmContextAutosave –Scope Process

   $Conn = Get-AutomationConnection -Name AzureRunAsConnection
   Connect-AzureRmAccount -ServicePrincipal -Tenant $Conn.TenantID -ApplicationId $Conn.ApplicationID -CertificateThumbprint $Conn.CertificateThumbprint
   Start-AzureRmVM -Name $VMName -ResourceGroupName $ResourceGroupName
   }
   ```

2. Runbook'u kaydedin ve Test bölmesini açın. Şimdi, testte olan iki girdi değişkeni için değerleri sağlayabilirsiniz.
3. Test bölmesini kapatın.
4. Runbook’un yeni sürümünü yayımlamak için **Yayımla**’ya tıklayın.
5. Önceki adımda başlattığınız sanal makineyi durdurun.
6. Runbook'u başlatmak için **Başlat**’a tıklayın. Başlatacağınız sanal makinenin **VMName** ve **ResourceGroupName** bilgilerini yazın.

   ![Runbook’u Başlatma](media/automation-first-runbook-textual/automation-pass-params.png)

7. Runbook tamamlandığında, sanal makinenin başladığından emin olun.  

## <a name="next-steps"></a>Sonraki adımlar

* Grafik runbook'ları kullanmaya başlamak için bkz. [İlk grafik runbook uygulamam](automation-first-runbook-graphical.md)
* PowerShell runbook'ları kullanmaya başlamak için bkz. [İlk PowerShell runbook uygulamam](automation-first-runbook-textual-powershell.md)
* Runbook türleri, avantajları ve sınırlamaları hakkında daha fazla bilgi için, bkz. [Azure Automation runbook türleri](automation-runbook-types.md)
* PowerShell betik desteği özelliği hakkında daha fazla bilgi için bkz. [Azure Automation’da Yerel PowerShell betik desteği](https://azure.microsoft.com/blog/announcing-powershell-script-support-azure-automation-2/)
