---
title: Azure automation'da ilk PowerShell iş akışı runbook Uygulamam
description: PowerShell İş Akışı kullanarak basit bir runbook oluşturma, test etme ve yayımlama adımlarını anlatan öğretici.
keywords: powershell iş akışı, powershell iş akışı örnekleri, iş akışı powershell
services: automation
ms.service: automation
ms.component: process-automation
author: georgewallace
ms.author: gwallace
ms.date: 03/16/2018
ms.topic: article
manager: carmonm
ms.openlocfilehash: cd57c6be9d962899bb3468fb86323af3fe01bc5b
ms.sourcegitcommit: d28bba5fd49049ec7492e88f2519d7f42184e3a8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/11/2018
---
# <a name="my-first-powershell-workflow-runbook"></a>İlk PowerShell İş Akışı runbook uygulamam

> [!div class="op_single_selector"]
> * [Grafik](automation-first-runbook-graphical.md)
> * [PowerShell](automation-first-runbook-textual-powershell.md)
> * [PowerShell İş Akışı](automation-first-runbook-textual.md)
> * [Python](automation-first-runbook-textual-python2.md)
> 
> 

Bu öğretici, Azure Automation’da bir [PowerShell İş Akışı runbook](automation-runbook-types.md#powershell-workflow-runbooks) oluşturulmasını adım adım göstermektedir. Test ve runbook işinin durumunu izlemek nasıl açıklayan sırasında yayımlama basit bir runbook ile başlayın. Ardından, bir Azure sanal makinesini başlatmayı içeren bir örnekle, bu runbook’u gerçekten Azure kaynaklarını yönetmek üzere değiştirin. Son olarak, daha sağlam runbook parametreleri ekleyerek runbook'u hale.

## <a name="prerequisites"></a>Önkoşullar
Bu öğreticiyi tamamlamak için aşağıdakiler gerekir:

* Azure aboneliği. Henüz bir aboneliğiniz yoksa [MSDN abone avantajlarınızı etkinleştirebilir](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) ya da [ücretsiz hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) için kaydolabilirsiniz.
* Runbook’u tutacak ve Azure kaynaklarında kimlik doğrulamasını yapacak bir [Automation hesabı](automation-offering-get-started.md).  Bu hesabın sanal makineyi başlatma ve durdurma izni olmalıdır.
* Azure sanal makinesi. Durdurun ve üretim VM olmamalıdır şekilde bu makineyi başlatın.

## <a name="step-1---create-new-runbook"></a>1. Adım - Yeni runbook oluşturma
Metnini veren basit bir runbook oluşturarak başlayın *Hello World*.

1. Azure portalında, Otomasyon hesabınızı açın.

   Automation hesabı sayfası size bu hesaptaki kaynakların hızlı bir görünümünü sağlar. Birkaç varlığınız zaten olmalıdır. Bunların çoğu, yeni bir Automation hesabına otomatik olarak dahil edilen modüllerdir. Burada ayrıca [önkoşullarda](#prerequisites) belirtilen Kimlik Bilgileri varlığınız da bulunmalıdır.

1. Tıklatın **Runbook'lar** altında **işlem Otomasyonu** listesini açmak için.
2. Tıklayarak yeni bir runbook oluşturmak **+ runbook Ekle** düğmesine ve ardından **yeni bir runbook oluşturmak**.
3. Runbook’a *MyFirstRunbook-Workflow* adını verin.
4. Bu durumda, oluşturacağız bir [PowerShell iş akışı runbook'u](automation-runbook-types.md#powershell-workflow-runbooks) şekilde select **Powershell iş akışı** için **Runbook türü**.
5. Runbook’u oluşturmak için **Oluştur**’a tıklayın ve metin düzenleyicisini açın.

## <a name="step-2---add-code-to-the-runbook"></a>2. Adım - Runbook'a kod ekleme
Kodu doğrudan runbook’a yazabilir veya Kitaplık denetiminde cmdlet’leri, runbook’ları ve varlıkları seçebilir ve ilgili parametrelerle bunların runbook’a eklenmesini sağlayabilirsiniz. Bu kılavuzda doğrudan runbook'a yazın.

1. Runbook'unuzda yalnızca gerekli şu anda boş *iş akışı* anahtar sözcüğü, tüm iş akışının encases runbook'unuz ve küme ayraçları adını.

   ```powershell-interactive
   Workflow MyFirstRunbook-Workflow
   {
   }
   ```
2. Ayraçlar arasına *Write-Output "Hello World."* yazın.

   ```powershell-interactive
   Workflow MyFirstRunbook-Workflow
   {
   Write-Output "Hello World"
   }
   ```
3. **Kaydet**’e tıklayarak runbook’u kaydedin.

## <a name="step-3---test-the-runbook"></a>3. Adım - Runbook'u test etme
Runbook’u üretimde kullanılabilir hale getirmek üzere yayımlamadan önce düzgün çalıştığından emin olmak için test etmeniz gerekir. Bir runbook'u test ettiğinizde, bunun **Taslak** sürümünü çalıştırır ve çıktısını etkileşimli olarak görüntülersiniz.

1. Test bölmesini açmak için **Test bölmesi**’ne tıklayın.
2. Testi başlatmak için **Başlat**’a tıklayın. Etkinleştirilen tek seçenek bu olmalıdır.
3. Bir [runbook işi](automation-runbook-execution.md) oluşturulur ve durumu görüntülenir.

   İş durumu, bulutta bir runbook çalışanının kullanılabilir hale gelmesinin beklendiğini gösteren şekilde *Sırada* olarak başlar. Bu taşınır *başlangıç* bir çalışan işi talep ettiğinde ve ardından *çalıştıran* runbook gerçekten çalışmaya başladığında.  

1. Runbook işi tamamlandığında çıktısı görüntülenir. Sizin durumunuzda görmelisiniz *Hello World*.<br><br> ![Hello World](media/automation-first-runbook-textual/test-output-hello-world.png)
2. Tuvale geri dönmek için Test bölmesini kapatın.

## <a name="step-4---publish-and-start-the-runbook"></a>4. Adım - Runbook’u yayımlama ve başlatma
Oluşturduğunuz runbook hala Taslak modundadır. Üretimde çalıştırılabilmesi için önce yayımlanması gerekir. Bir runbook yayımladığınızda, Taslak sürümü mevcut Yayımlanmış sürümün üzerine yazarsınız. Henüz runbook oluşturduğunuz olduğundan sizin durumunuzda, bir yayımlanmış sürümü yok.

1. Runbook’u yayımlamak için **Yayımla**’ya tıklayın ve sorulduğunda **Evet**’e tıklayın.
2. Runbook'ta görüntülemek için sola kaydırırsanız, **Runbook'lar** Bölmesi şimdi gösterir bir **yazma durumu** , **yayımlanan**.
3. **MyFirstRunbook-Workflow** için bölmeyi görüntülemek üzere geri sağa kaydırın.  
   Üst kısımdaki seçenekler runbook’u başlatmamıza, gelecekte bir zamanda başlatmak üzere zamanlamamıza ya da bir HTTP çağrısıyla başlatılabilmesi için [web kancası](automation-webhooks.md) oluşturmamıza olanak tanır.
4. yalnızca runbook'u başlatmak istediğiniz şekilde tıklatın **Başlat** ve ardından **Evet** istendiğinde.<br><br> ![Runbook’u başlatma](media/automation-first-runbook-textual/automation-runbook-controls-start.png)
5. Oluşturduğunuz runbook işi için bir iş bölmesi açıldı. Bu bölme kapatılabilir, ancak işin ilerleme durumunu izleyebilmek için bu durumda, bu açık bırakın.
6. İş durumu gösterilen **iş özeti** ve runbook test zaman gördüğümüz durumların aynısıdır.<br><br> ![İş Özeti](media/automation-first-runbook-textual/job-pane-status-blade-jobsummary.png)
7. Runbook durumu olarak *Tamamlandı* gösterilince **Çıktı**’ya tıklayın. Çıktı bölmesi açılır ve görebilirsiniz, *Hello World*.<br><br> ![İş Özeti](media/automation-first-runbook-textual/job-pane-status-blade-outputtile.png)  
8. Çıktı bölmesini kapatın.
9. Runbook işine ait Akışlar bölmesini açmak için **Tüm Günlükler**’e tıklayın. Yalnızca görmelisiniz *Hello World* çıkış akışı, ancak bu ayrıntılı ve hata gibi runbook işine yönelik diğer akışlar runbook bunlara yazıyorsa gösterebilir.<br><br> ![İş Özeti](media/automation-first-runbook-textual/job-pane-status-blade-alllogstile.png)
10. MyFirstRunbook bölmesine dönmek için Akışlar bölmesini ve İş bölmesini kapatın.
11. Bu runbook’a ait İşler bölmesini açmak için **İşler**’e tıklayın. Bu bölmede, bu runbook tarafından oluşturulan tüm işler listelenir. Yalnızca bir işin, yalnızca bir kez çalıştırılan işe beri listelendiğini görmeniz gerekir.<br><br> ![İşler](media/automation-first-runbook-textual/runbook-control-job-tile.png)
12. Runbook başlatıldığında görüntülediğiniz iş bölmesini açmak için bu işe tıklayabilirsiniz. Böylece zaman içinde geri dönerek, belirli bir runbook için oluşturulan herhangi bir işin ayrıntılarını görüntüleyebilirsiniz.

## <a name="step-5---add-authentication-to-manage-azure-resources"></a>5. Adım- Azure kaynaklarını yönetmek için kimlik doğrulaması ekleme
Test ve runbook'unuzu yayımlanan, ancak şu ana kadar faydalı bir şey değil. Azure kaynaklarınızı yönetmek istediğiniz. Kimlik doğrulaması ancak bölümünde bahsedilen kimlik bilgilerini kullanarak bunu açamayacaksınız [Önkoşullar](#prerequisites). Bunun **Connect-AzureRmAccount** cmdlet'i.

1. MyFirstRunbook İş Akışı bölmesinde **Düzenle**’ye tıklayarak metin düzenleyicisini açın.
2. gerekmeyen **Write-Output** satırı artık, bu nedenle bir tane silin.
3. İmleci ayraçlar arasında boş bir satıra getirin.
4. Automation Farklı Çalıştır hesabınızla kimlik doğrulamasını işleyecek aşağıdaki kodu yazın veya kopyalayıp yapıştırın:

   ```powershell-interactive
   $Conn = Get-AutomationConnection -Name AzureRunAsConnection
   Connect-AzureRmAccount -ServicePrincipal -Tenant $Conn.TenantID `
   -ApplicationId $Conn.ApplicationID -CertificateThumbprint $Conn.CertificateThumbprint
   ```
5. Tıklatın **Test bölmesi** böylece runbook'u test edebilirsiniz.
6. Testi başlatmak için **Başlat**’a tıklayın. Tamamlandığında, hesabınızdaki temel bilgileri görüntüleyen bir aşağıdakine benzer bir çıktı almalısınız. Böylece kimlik bilgisinin geçerli olduğunu doğrulanmış olur.<br><br> ![Kimlik Doğrulaması](media/automation-first-runbook-textual/runbook-auth-output.png)

## <a name="step-6---add-code-to-start-a-virtual-machine"></a>6. Adım - Sanal makineyi başlatmak için kod ekleme
Runbook'unuzda, Azure aboneliğinizin kimlik doğrulaması, kaynakları yönetebilir. bir sanal makineyi başlatmak için bir komut ekleyin. Azure aboneliğinizde herhangi bir sanal makine seçin ve şu an için runbook adı cmdlet'e kod demektir.

1. Sonra *Connect-AzureRmAccount*, türü *Start-AzureRmVM-Name 'VMName' - ResourceGroupName 'NameofResourceGroup'* adını ve kaynak grubu adını sanal makinenin, sağlama.  

   ```powershell-interactive
   workflow MyFirstRunbook-Workflow
   {
   $Conn = Get-AutomationConnection -Name AzureRunAsConnection
   Connect-AzureRmAccount -ServicePrincipal -Tenant $Conn.TenantID -ApplicationId $Conn.ApplicationID -CertificateThumbprint $Conn.CertificateThumbprint
   Start-AzureRmVM -Name 'VMName' -ResourceGroupName 'ResourceGroupName'
   }
   ```
2. Runbook'u kaydedin ve ardından **Test bölmesi** böylece test edebilirsiniz.
3. Testi başlatmak için **Başlat**’a tıklayın. Tamamlandığında, sanal makinenin başlatıldığını kontrol edin.

## <a name="step-7---add-an-input-parameter-to-the-runbook"></a>7. Adım - Runbook'a girdi parametresi ekleme
runbook'unuzda sanal makine şu anda runbook'ta Bu, sabit kodlanmış başlatır, ancak runbook başlatılırken sanal makineyi belirtebilirsiniz, daha kullanışlı olurdu. Bu işlevsellik sağlamak için runbook'a girdi parametreleri ekleyin.

1. Runbook’a *VMName* ve *ResourceGroupName* parametrelerini ekleyin ve bu değişkenleri aşağıdaki örnekteki gibi **Start-AzureRmVM** cmdlet’i ile kullanın.

   ```powershell-interactive
   workflow MyFirstRunbook-Workflow
   {
    Param(
     [string]$VMName,
     [string]$ResourceGroupName
    )  
   $Conn = Get-AutomationConnection -Name AzureRunAsConnection
   Connect-AzureRmAccount -ServicePrincipal -Tenant $Conn.TenantID -ApplicationId $Conn.ApplicationID -CertificateThumbprint $Conn.CertificateThumbprint
   Start-AzureRmVM -Name $VMName -ResourceGroupName $ResourceGroupName
   }
   ```
2. Runbook'u kaydedin ve Test bölmesini açın. Test olan iki girdi değişkeni için değerleri şimdi sağlayabilir.
3. Test bölmesini kapatın.
4. Runbook’un yeni sürümünü yayımlamak için **Yayımla**’ya tıklayın.
5. Önceki adımda başlattığınız sanal makineyi durdurun.
6. Runbook'u başlatmak için **Başlat**’a tıklayın. Başlatacağınız sanal makinenin **VMName** ve **ResourceGroupName** bilgilerini yazın.<br><br> ![Runbook’u başlatma](media/automation-first-runbook-textual/automation-pass-params.png)<br>  
7. Runbook tamamlandığında, sanal makinenin başladığından emin olun.  

## <a name="next-steps"></a>Sonraki adımlar
* Grafik runbook'ları kullanmaya başlamak için bkz. [İlk grafik runbook uygulamam](automation-first-runbook-graphical.md)
* PowerShell runbook'ları kullanmaya başlamak için bkz. [İlk PowerShell runbook uygulamam](automation-first-runbook-textual-powershell.md)
* Runbook türleri, avantajları ve sınırlamaları hakkında daha fazla bilgi için, bkz. [Azure Automation runbook türleri](automation-runbook-types.md)
* PowerShell betik desteği özelliği hakkında daha fazla bilgi için bkz. [Azure Automation’da Yerel PowerShell betik desteği](https://azure.microsoft.com/blog/announcing-powershell-script-support-azure-automation-2/)
