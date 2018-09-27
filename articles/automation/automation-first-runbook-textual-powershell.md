---
title: Azure automation'da ilk PowerShell runbook'um
description: Basit bir PowerShell runbook oluşturma, test etme ve yayımlama adımlarını anlatan öğretici.
keywords: azure powershell, powershell betik öğreticisi, powershell otomasyonu
services: automation
ms.service: automation
ms.component: process-automation
author: georgewallace
ms.author: gwallace
ms.date: 03/16/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 8f3185a2c7633ba0cb5a9b266bcddf023d3c36e1
ms.sourcegitcommit: 51a1476c85ca518a6d8b4cc35aed7a76b33e130f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/25/2018
ms.locfileid: "47166461"
---
# <a name="my-first-powershell-runbook"></a>İlk PowerShell runbook’um

> [!div class="op_single_selector"]
> * [Grafik](automation-first-runbook-graphical.md)
> * [PowerShell](automation-first-runbook-textual-powershell.md)
> * [PowerShell İş Akışı](automation-first-runbook-textual.md)
> * [Python](automation-first-runbook-textual-python2.md)

Bu öğretici, Azure Automation’da bir [PowerShell runbook](automation-runbook-types.md#powershell-runbooks) oluşturulmasını adım adım göstermektedir. Test ve runbook işinin durumunu izlemeyi öğrenin yayımlayacağımız basit bir runbook ile başlayacağız. Ardından, bir Azure sanal makinesini başlatmayı içeren bir örnekle, bu runbook’u gerçekten Azure kaynaklarını yönetmek üzere değiştirin. Son olarak, runbook'u daha sağlam runbook parametreleri ekleyerek yapmanızı ister.

## <a name="prerequisites"></a>Önkoşullar
Bu öğreticiyi tamamlamak için aşağıdakiler gerekir:

* Azure aboneliği. Henüz bir aboneliğiniz yoksa [MSDN abone avantajlarınızı etkinleştirebilir](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) ya da [ücretsiz hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) için kaydolabilirsiniz.
* Runbook’u tutacak ve Azure kaynaklarında kimlik doğrulamasını yapacak bir [Automation hesabı](automation-offering-get-started.md). Bu hesabın sanal makineyi başlatma ve durdurma izni olmalıdır.
* Azure sanal makinesi. Bu makineyi durdurup başlatacağınız için makinenin üretime yönelik bir VM olmaması gerekir.

## <a name="step-1---create-new-runbook"></a>1. Adım - Yeni runbook oluşturma
Metnini veren basit bir runbook oluşturmaya başlayın *Hello World*.

1. Azure portalında, Otomasyon hesabınızı açın.

   Automation hesabı sayfası size bu hesaptaki kaynakların hızlı bir görünümünü sağlar. Birkaç varlığınız zaten olmalıdır. Bunların çoğu, yeni bir Automation hesabına otomatik olarak dahil edilen modüllerdir. Burada ayrıca [önkoşullarda](#prerequisites) belirtilen Kimlik Bilgileri varlığınız da bulunmalıdır.

1. Tıklayın **runbook'ları** altında **süreç otomasyonu** runbook'ların listesini açmak için.
2. Tıklayarak yeni bir runbook oluşturun **+ runbook Ekle** düğmesine ve ardından **yeni bir runbook oluşturmak**.
3. Runbook’a *MyFirstRunbook-PowerShell* adını verin.
4. Bu durumda, oluşturacağız bir [PowerShell runbook'u](automation-runbook-types.md#powershell-runbooks) seçin **Powershell** için **Runbook türü**.
5. Runbook’u oluşturmak için **Oluştur**’a tıklayın ve metin düzenleyicisini açın.

## <a name="step-2---add-code-to-the-runbook"></a>2. Adım - Runbook'a kod ekleme
Kodu doğrudan runbook’a yazabilir veya Kitaplık denetiminde cmdlet’leri, runbook’ları ve varlıkları seçebilir ve ilgili parametrelerle bunların runbook’a eklenmesini sağlayabilirsiniz. Bu kılavuzda doğrudan runbook'a yazın.

1. Şu anda boş, runbook türü *Write-Output "Hello World."* yazın.<br><br> ![Hello World](media/automation-first-runbook-textual-powershell/automation-helloworld.png)  
2. **Kaydet**’e tıklayarak runbook’u kaydedin.

## <a name="step-3---test-the-runbook"></a>3. Adım - Runbook'u test etme
Runbook’u üretimde kullanılabilir hale getirmek üzere yayımlamadan önce düzgün çalıştığından emin olmak için test etmeniz gerekir. Bir runbook'u test ettiğinizde, bunun **Taslak** sürümünü çalıştırır ve çıktısını etkileşimli olarak görüntülersiniz.

1. Test bölmesini açmak için **Test bölmesi**’ne tıklayın.
2. Testi başlatmak için **Başlat**’a tıklayın. Etkinleştirilen tek seçenek bu olmalıdır.
3. Bir [runbook işi](automation-runbook-execution.md) oluşturulur ve durumu görüntülenir.

   İş durumu, bulutta bir runbook çalışanının kullanılabilir hale gelmesinin beklendiğini gösteren şekilde *Sırada* olarak başlar. Bu taşınır *başlangıç* bir çalışan işi talep ettiğinde ve ardından *çalıştıran* runbook gerçekten çalışmaya başladığında.  

1. Runbook işi tamamlandığında çıktısı görüntülenir. Sizin durumunuzda görmelisiniz *Hello World*.<br><br> ![Test Bölmesi Çıktısı](media/automation-first-runbook-textual-powershell/automation-testpane-output.png)  
2. Tuvale geri dönmek için Test bölmesini kapatın.

## <a name="step-4---publish-and-start-the-runbook"></a>4. Adım - Runbook’u yayımlama ve başlatma
Oluşturduğunuz runbook hala Taslak modundadır. Üretimde çalıştırılabilmesi için önce yayımlanması gerekir.  Bir runbook yayımladığınızda, Taslak sürümü mevcut Yayımlanmış sürümün üzerine yazarsınız. Henüz runbook yalnızca oluşturduğundan sizin durumunuzda, yayımlanmış sürümünüz yok.

1. Runbook’u yayımlamak için **Yayımla**’ya tıklayın ve sorulduğunda **Evet**’e tıklayın.
2. Runbook'ta görüntülemek için sola kaydırırsanız **runbook'ları** bölmesi artık gösterir bir **yazma durumu** , **yayımlanan**.
3. **MyFirstRunbook-PowerShell**’e ait bölmeyi görüntülemek üzere geri sağa kaydırın.  
   Üst kısımdaki seçenekler runbook’u başlatmamıza, görüntülememize, gelecek bir zamanda başlatılmak üzere zamanlamamıza ya da bir HTTP çağrısıyla başlatılabilmesi için [web kancası](automation-webhooks.md) oluşturmamıza olanak tanır.
4. Runbook'u başlatmak için bu nedenle tıklatın istediğiniz **Başlat** ve ardından **Tamam** zaman Runbook'u Başlat sayfasını açar.
5. Oluşturduğunuz runbook işi için iş sayfası açılır. Bu bölme kapatılabilir, ancak işin ilerleme durumunu izleyebilmek için bu durumda, açık bırakın.
6. İş durumu gösterilen **iş özeti** ve ne zaman, runbook test gördüğümüz durumların aynısıdır.<br><br> ![İş Özeti](media/automation-first-runbook-textual-powershell/job-pane-status-blade-jobsummary.png)<br>  
7. Bir kez runbook durumu gösterir *tamamlandı*altında **genel bakış** tıklayın **çıkış**. Çıktı bölmesi açılır ve gördüğünüz, *Hello World*.<br><br> ![İş Çıktısı](media/automation-first-runbook-textual-powershell/job-pane-status-blade-outputtile.png)<br> 
8. Çıkış sayfasını kapatın.
9. Runbook işine ait Akışlar bölmesini açmak için **Tüm Günlükler**’e tıklayın. Çıktı akışında yalnızca *Merhaba Dünya* metnini görmelisiniz, ancak bu bölmede, runbook bunlara yazıyorsa Ayrıntılı ve Hata gibi runbook işine yönelik diğer akışlar da gösterilebilir.<br><br> ![Tüm Günlükler](media/automation-first-runbook-textual-powershell/job-pane-status-blade-alllogstile.png)<br>   
10. Akışları sayfası ve MyFirstRunbook-PowerShell sayfasına geri dönmek için iş sayfasına kapatın.
11. Altında **ayrıntıları**, tıklayın **işleri** bu runbook'a ait işler bölmesini açmak için. Bu bölmede, bu runbook tarafından oluşturulan tüm işler listelenir. İşi yalnızca bir kez çalıştırdığınız için sadece bir işin listelendiğini görmeniz gerekir.<br><br> ![İş Listesi](media/automation-first-runbook-textual-powershell/runbook-control-job-tile.png)  
12. Runbook’u başlattığınızda, görüntülediğiniz iş bölmesini açmak için bu işe tıklayabilirsiniz. Böylece zaman içinde geri dönerek, belirli bir runbook için oluşturulan herhangi bir işin ayrıntılarını görüntüleyebilirsiniz.

## <a name="step-5---add-authentication-to-manage-azure-resources"></a>5. Adım- Azure kaynaklarını yönetmek için kimlik doğrulaması ekleme
Runbook uygulamanızı test ettiniz ve yayımladınız, ancak şu ana kadar faydalı bir şey yapmadı. Bu runbook’un Azure kaynaklarını yönetmesini istiyorsunuz. Sahip olduğunuz sürece kimlik doğrulaması karşın başvurulan kimlik bilgilerini kullanarak bunu mümkün değil [önkoşulları](#prerequisites). Bunu şu şekilde **Connect-AzureRmAccount** cmdlet'i. Kaynakları birden çok farklı abonelikler arasında yönetiyorsanız, kullanmanız gereken **- AzureRmContext** parametresi ile birlikte [Get-AzureRmContext](/powershell/module/azurerm.profile/get-azurermcontext).

   ```powershell
   $Conn = Get-AutomationConnection -Name AzureRunAsConnection
   Connect-AzureRmAccount -ServicePrincipal -Tenant $Conn.TenantID `
-ApplicationID $Conn.ApplicationID -CertificateThumbprint $Conn.CertificateThumbprint

   $AzureContext = Select-AzureRmSubscription -SubscriptionId $ServicePrincipalConnection.SubscriptionID

   Get-AzureRmVM -ResourceGroupName myResourceGroup -AzureRmContext $AzureContext
   ```

1. Tıklayarak metin düzenleyicisini açın **Düzenle** MyFirstRunbook-PowerShell sayfasında.
2. İhtiyacınız olmayan **Write-Output** satırı artık, bu nedenle bir tane silin.
3. Otomasyon Farklı Çalıştır hesabınızla kimlik doğrulamasını işleyen aşağıdaki kodu yazın veya kopyalayıp yapıştırın:

   ```powershell
   $Conn = Get-AutomationConnection -Name AzureRunAsConnection
   Connect-AzureRmAccount -ServicePrincipal -Tenant $Conn.TenantID `
   -ApplicationId $Conn.ApplicationID -CertificateThumbprint $Conn.CertificateThumbprint
   ```

   > [!IMPORTANT]
   > **Add-AzureRmAccount** ve **Login-AzureRmAccount** artık diğer adlarıdır **Connect-AzureRMAccount**. Varsa **Connect-AzureRMAccount** cmdlet'i mevcut değil, kullanabileceğiniz **Add-AzureRmAccount** veya **Login-AzureRmAccount**, veya Otomasyon modüllerinizi güncelleştirebilirsiniz En son sürümlerine hesap.

4. Tıklayın **Test bölmesi** böylece runbook'u test edebilirsiniz.
5. Testi başlatmak için **Başlat**’a tıklayın. Tamamlandığında, hesabınızdaki temel bilgileri görüntüleyen bir aşağıdakine benzer bir çıktı almalısınız. Böylece kimlik bilgisinin geçerli olduğunu doğrulanmış olur.<br><br> ![Kimlik Doğrulaması](media/automation-first-runbook-textual-powershell/runbook-auth-output.png)

## <a name="step-6---add-code-to-start-a-virtual-machine"></a>6. Adım - Sanal makineyi başlatmak için kod ekleme
Azure aboneliğinize, runbook kimlik doğrulaması, kaynakları yönetebilir. Bir sanal makineyi başlatmak için bir komut ekleyin. Artık, şimdilik bu adı için ve Azure aboneliğinizdeki herhangi bir sanal makine seçebilirsiniz.

1. Sonra *Connect-AzureRmAccount*, türü *Start-AzureRmVM-Name 'VMName' - ResourceGroupName 'NameofResourceGroup'* adı ve sanal makinenin kaynak grubu adı sağlar.  
   
   ```
   $Conn = Get-AutomationConnection -Name AzureRunAsConnection
   Connect-AzureRmAccount -ServicePrincipal -Tenant $Conn.TenantID `
   -ApplicationID $Conn.ApplicationID -CertificateThumbprint $Conn.CertificateThumbprint
   Start-AzureRmVM -Name 'VMName' -ResourceGroupName 'ResourceGroupName'
   ```
   <br>
2. Runbook'u kaydedin ve ardından **Test bölmesi** böylece test edebilirsiniz.
3. Testi başlatmak için **Başlat**’a tıklayın. Tamamlandığında, sanal makinenin başlatıldığını kontrol edin.

## <a name="step-7---add-an-input-parameter-to-the-runbook"></a>7. Adım - Runbook'a girdi parametresi ekleme
Runbook'unuzda sanal makine şu anda runbook'ta o, kodlanmış başlatmaz, bunun runbook başlatılırken sanal makineyi belirtmeniz durumunda daha kullanışlı olurdu. Bu işlevi sağlamak için runbook'a girdi parametreleri ekleyin.

1. Parametrelerini ekleyin *VMName* ve *ResourceGroupName* runbook ve bu değişkenleri ile **Start-AzureRmVM** cmdlet'i aşağıdaki örnekte olduğu gibi.

   ```
   Param(
    [string]$VMName,
    [string]$ResourceGroupName
   )
   $Conn = Get-AutomationConnection -Name AzureRunAsConnection
   Connect-AzureRmAccount -ServicePrincipal -Tenant $Conn.TenantID `
   -ApplicationID $Conn.ApplicationID -CertificateThumbprint $Conn.CertificateThumbprint
   Start-AzureRmVM -Name $VMName -ResourceGroupName $ResourceGroupName
   ```
   <br>
1. Runbook'u kaydedin ve Test bölmesini açın. Şimdi testte kullanılan iki girdi değişkeni için değerleri sağlayabilirsiniz.
2. Test bölmesini kapatın.
3. Runbook’un yeni sürümünü yayımlamak için **Yayımla**’ya tıklayın.
4. Önceki adımda başlattığınız sanal makineyi durdurun.
5. Tıklayın **Tamam** runbook'u başlatın. Başlatacağınız sanal makinenin **VMName** ve **ResourceGroupName** bilgilerini yazın.<br><br> ![Parametre Geçirme](media/automation-first-runbook-textual-powershell/automation-pass-params.png)<br>  
6. Runbook tamamlandığında, sanal makinenin başladığından emin olun.

## <a name="differences-from-powershell-workflow"></a>PowerShell İş Akışı ile arasındaki farklar
PowerShell runbook'ları, PowerShell İş Akışı runbook'ları ile aynı yaşam döngüsü, yetenekler ve yönetim özelliklerine sahiptir, ancak arada bazı farklar ve sınırlamalar vardır:

1. PowerShell runbook'ları, bir derleme adımı içermediklerinden PowerShell İş Akışı runbook'larına göre daha hızlı çalışır.
2. PowerShell İş Akışı runbook'ları, kontrol noktalarını destekler. Kontrol noktalarının kullanılması sayesinde PowerShell İş Akışı runbook'ları, runbook içerisinde herhangi bir noktadan sürdürülebilirken, PowerShell runbook'ları yalnızca başlangıç noktasından sürdürülebilir.
3. PowerShell İş Akışı runbook'ları paralel ve seri yürütmeyi desteklerken, PowerShell runbook'ları yalnızca komutların seri olarak yürütülmesini destekler.
4. PowerShell İş Akışı runbook'unda bir etkinliğin, komutun veya betik bloğunun kendi çalışma alanı bulunabilirken, PowerShell runbook'unda betikteki her şey tek bir çalışma alanında çalıştırılır. Ayrıca yerel bir PowerShell runbook'u ile bir PowerShell İş Akışı runbook'u arasında bazı [söz dizimi farklılıkları](https://technet.microsoft.com/magazine/dn151046.aspx) vardır.

## <a name="next-steps"></a>Sonraki adımlar
* Grafik runbook'ları kullanmaya başlamak için bkz. [İlk grafik runbook uygulamam](automation-first-runbook-graphical.md)
* PowerShell iş akışı runbook'larını kullanmaya başlamak için bkz. [İlk PowerShell iş akışı runbook uygulamam](automation-first-runbook-textual.md)
* Runbook türleri, avantajları ve sınırlamaları hakkında daha fazla bilgi için bkz. [Azure Automation runbook türleri](automation-runbook-types.md)
* PowerShell betik desteği özelliği hakkında daha fazla bilgi için bkz. [Azure Automation’da Yerel PowerShell betik desteği](https://azure.microsoft.com/blog/announcing-powershell-script-support-azure-automation-2/)

