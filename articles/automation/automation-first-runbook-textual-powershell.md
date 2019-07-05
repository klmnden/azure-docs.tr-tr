---
title: Azure automation'da ilk PowerShell runbook'um
description: Basit bir PowerShell runbook oluşturma, test etme ve yayımlama adımlarını anlatan öğretici.
keywords: azure powershell, powershell betik öğreticisi, powershell otomasyonu
services: automation
ms.service: automation
ms.subservice: process-automation
author: bobbytreed
ms.author: robreed
ms.date: 11/27/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 10b93e54bc3f13c72889ab7c75b0e4f6e280e7d8
ms.sourcegitcommit: f811238c0d732deb1f0892fe7a20a26c993bc4fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/29/2019
ms.locfileid: "67476732"
---
# <a name="my-first-powershell-runbook"></a>İlk PowerShell runbook’um

> [!div class="op_single_selector"]
> * [Grafik](automation-first-runbook-graphical.md)
> * [PowerShell](automation-first-runbook-textual-powershell.md)
> * [PowerShell İş Akışı](automation-first-runbook-textual.md)
> * [Python](automation-first-runbook-textual-python2.md)

Bu öğretici, Azure Automation’da bir [PowerShell runbook](automation-runbook-types.md#powershell-runbooks) oluşturulmasını adım adım göstermektedir. Test ve runbook işinin durumunu izlemeyi öğrenin yayımlayacağımız basit bir runbook ile başlayacağız. Ardından, bir Azure sanal makinesini başlatmayı içeren bir örnekle, bu runbook’u gerçekten Azure kaynaklarını yönetmek üzere değiştirin. Son olarak, runbook'u daha sağlam runbook parametreleri ekleyerek yapmanızı ister.

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi tamamlamak için aşağıdaki önkoşulları karşılamanız gerekir:

* Azure aboneliği. Henüz bir aboneliğiniz yoksa [MSDN abone avantajlarınızı etkinleştirebilir](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) ya da [ücretsiz hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) için kaydolabilirsiniz.
* Runbook’u tutacak ve Azure kaynaklarında kimlik doğrulamasını yapacak bir [Automation hesabı](automation-quickstart-create-account.md). Bu hesabın sanal makineyi başlatma ve durdurma izni olmalıdır.
* Azure sanal makinesi. Üretime yönelik bir VM olmaması için bu makineyi durdurup.
* Gerekebilir [Azure modüllerini](automation-update-azure-modules.md) kullandığınız cmdlet'lerini temel alır.

## <a name="create-new-runbook"></a>Yeni runbook oluştur

Metnini veren basit bir runbook oluşturmaya başlayın *Hello World*.

1. Azure portalında, Otomasyon hesabınızı açın.
2. Tıklayın **runbook'ları** altında **süreç otomasyonu** runbook'ların listesini açmak için.
3. Tıklayarak yeni bir runbook oluşturun **+ runbook Ekle** düğmesine ve ardından **yeni bir runbook oluşturmak**.
4. Runbook’a *MyFirstRunbook-PowerShell* adını verin.
5. Bu durumda, oluşturacağız bir [PowerShell runbook'u](automation-runbook-types.md#powershell-runbooks) seçin **Powershell** için **Runbook türü**.
6. Runbook’u oluşturmak için **Oluştur**’a tıklayın ve metin düzenleyicisini açın.

## <a name="add-code-to-the-runbook"></a>Runbook'a kod ekleme

Kodu doğrudan runbook’a yazabilir veya Kitaplık denetiminde cmdlet’leri, runbook’ları ve varlıkları seçebilir ve ilgili parametrelerle bunların runbook’a eklenmesini sağlayabilirsiniz. Bu kılavuzda doğrudan runbook'a yazın.

1. Şu anda boş, runbook türü *Write-Output "Hello World."* yazın.

   ![Hello World](media/automation-first-runbook-textual-powershell/automation-helloworld.png)  

2. **Kaydet**’e tıklayarak runbook’u kaydedin.

## <a name="step-3---test-the-runbook"> </a> Runbook'u test etme

Runbook’u üretimde kullanılabilir hale getirmek üzere yayımlamadan önce düzgün çalıştığından emin olmak için test etmeniz gerekir. Bir runbook'u test ettiğinizde, bunun **Taslak** sürümünü çalıştırır ve çıktısını etkileşimli olarak görüntülersiniz.

1. Test bölmesini açmak için **Test bölmesi**’ne tıklayın.
2. Testi başlatmak için **Başlat**’a tıklayın. Etkinleştirilen tek seçenek bu olmalıdır.
3. Bir [runbook işi](automation-runbook-execution.md) oluşturulur ve durumu görüntülenir.

   İş durumu olarak başlar *sıraya alınan* başlatılabilmek bir runbook çalışanının bulutta gelmesinin beklendiğini gösteren. Bu taşınır *başlangıç* bir çalışan işi talep ettiğinde ve ardından *çalıştıran* runbook gerçekten çalışmaya başladığında.  

4. Runbook işi tamamlandığında çıktısı görüntülenir. Sizin durumunuzda görmelisiniz *Hello World*.

   ![Test Bölmesi Çıktısı](media/automation-first-runbook-textual-powershell/automation-testpane-output.png)  

5. Tuvale geri dönmek için Test bölmesini kapatın.

## <a name="publish-and-start-the-runbook"></a>Yayımlama ve runbook'u Başlat

Oluşturduğunuz runbook hala Taslak modundadır. Üretimde çalıştırılabilmesi için önce yayımlanması gerekir.  Bir runbook yayımladığınızda, Taslak sürümü mevcut Yayımlanmış sürümün üzerine yazarsınız. Henüz runbook yalnızca oluşturduğundan sizin durumunuzda, yayımlanmış sürümünüz yok.

1. Runbook’u yayımlamak için **Yayımla**’ya tıklayın ve sorulduğunda **Evet**’e tıklayın.
1. Runbook'ta görüntülemek için sola kaydırırsanız **runbook'ları** bölmesi artık gösterir bir **yazma durumu** , **yayımlanan**.
1. **MyFirstRunbook-PowerShell**’e ait bölmeyi görüntülemek üzere geri sağa kaydırın.  
   Üst kısımdaki seçenekler runbook’u başlatmamıza, görüntülememize, gelecek bir zamanda başlatılmak üzere zamanlamamıza ya da bir HTTP çağrısıyla başlatılabilmesi için [web kancası](automation-webhooks.md) oluşturmamıza olanak tanır.
1. Runbook'u başlatmak için bu nedenle tıklatın istediğiniz **Başlat** ve ardından **Tamam** zaman Runbook'u Başlat sayfasını açar.
1. Oluşturduğunuz runbook işi için iş sayfası açılır. Bu bölme kapatılabilir, ancak işin ilerleme durumunu izleyebilmek için bu durumda, açık bırakın.
1. İş durumu gösterilen **iş özeti** ve ne zaman, runbook test gördüğümüz durumların aynısıdır.

   ![İş Özeti](media/automation-first-runbook-textual-powershell/job-pane-status-blade-jobsummary.png)

1. Bir kez runbook durumu gösterir *tamamlandı*altında **genel bakış** tıklayın **çıkış**. Çıktı bölmesi açılır ve gördüğünüz, *Hello World*.

   ![İş Çıktısı](media/automation-first-runbook-textual-powershell/job-pane-status-blade-outputtile.png)

1. Çıkış sayfasını kapatın.
1. Runbook işine ait Akışlar bölmesini açmak için **Tüm Günlükler**’e tıklayın. Yalnızca görmelisiniz *Hello World* çıktısında stream, ancak bu Çıktıyı ayrıntılı ve hata gibi runbook işine yönelik diğer akışlar runbook bunlara yazıyorsa gösterilebilir.

   ![Tüm Günlükler](media/automation-first-runbook-textual-powershell/job-pane-status-blade-alllogstile.png)

1. Akışları sayfası ve MyFirstRunbook-PowerShell sayfasına geri dönmek için iş sayfasına kapatın.
1. Altında **ayrıntıları**, tıklayın **işleri** bu runbook'a ait işler bölmesini açmak için. Bu sayfada tüm bu runbook tarafından oluşturulan işler listelenir. İşi yalnızca bir kez çalıştırdığınız için sadece bir işin listelendiğini görmeniz gerekir.

   ![İş Listesi](media/automation-first-runbook-textual-powershell/runbook-control-job-tile.png)  

1. Runbook’u başlattığınızda, görüntülediğiniz iş bölmesini açmak için bu işe tıklayabilirsiniz. Bu eylem, zaman içinde geri dönün ve belirli bir runbook için oluşturulan herhangi bir işin ayrıntılarını görüntülemek sağlar.

## <a name="add-authentication-to-manage-azure-resources"></a>Azure kaynaklarını yönetmek için kimlik doğrulaması ekleme

Runbook uygulamanızı test ettiniz ve yayımladınız, ancak şu ana kadar faydalı bir şey yapmadı. Bu runbook’un Azure kaynaklarını yönetmesini istiyorsunuz. Sahip olduğunuz sürece kimlik doğrulaması olsa da, Otomasyon hesabı oluşturduğunuzda, otomatik olarak oluşturulan bir farklı çalıştır bağlantısı kullanarak bunu yapmak mümkün değildir. Farklı Çalıştır bağlantısı ile kullandığınız **Connect-AzureRmAccount** cmdlet'i. Kaynakları birden çok farklı abonelikler arasında yönetiyorsanız, kullanmanız gerekir **- AzureRmContext** parametresi ile birlikte [Get-AzureRmContext](/powershell/module/azurerm.profile/get-azurermcontext).

   ```powershell
   # Ensures you do not inherit an AzureRMContext in your runbook
   Disable-AzureRmContextAutosave –Scope Process
   
   $connection = Get-AutomationConnection -Name AzureRunAsConnection
   Connect-AzureRmAccount -ServicePrincipal -Tenant $connection.TenantID `
-ApplicationID $connection.ApplicationID -CertificateThumbprint $connection.CertificateThumbprint

   $AzureContext = Select-AzureRmSubscription -SubscriptionId $connection.SubscriptionID

   Get-AzureRmVM -ResourceGroupName myResourceGroup -AzureRmContext $AzureContext
   ```

1. Tıklayarak metin düzenleyicisini açın **Düzenle** MyFirstRunbook-PowerShell sayfasında.
1. İhtiyacınız olmayan **Write-Output** satırı artık, bu nedenle bir tane silin.
1. Otomasyon Farklı Çalıştır hesabınızla kimlik doğrulamasını işleyen aşağıdaki kodu yazın veya kopyalayıp yapıştırın:

   ```powershell
   # Ensures you do not inherit an AzureRMContext in your runbook
   Disable-AzureRmContextAutosave –Scope Process

   $connection = Get-AutomationConnection -Name AzureRunAsConnection
   Connect-AzureRmAccount -ServicePrincipal -Tenant $connection.TenantID `
   -ApplicationId $connection.ApplicationID -CertificateThumbprint $connection.CertificateThumbprint
   ```

   > [!IMPORTANT]
   > **Add-AzureRmAccount** ve **Login-AzureRmAccount** artık diğer adlarıdır **Connect-AzureRMAccount**. Varsa **Connect-AzureRMAccount** cmdlet'i mevcut değil, kullanabileceğiniz **Add-AzureRmAccount** veya **Login-AzureRmAccount**, veya Otomasyon modüllerinizi güncelleştirebilirsiniz En son sürümlerine hesap.

1. Tıklayın **Test bölmesi** böylece runbook'u test edebilirsiniz.
1. Testi başlatmak için **Başlat**’a tıklayın. Tamamlandığında, hesabınızdaki temel bilgileri görüntüleyen bir aşağıdakine benzer bir çıktı almalısınız. Bu çıkış, farklı çalıştır hesabı geçerli olduğunu onaylar.

   ![Kimlik doğrulaması](media/automation-first-runbook-textual-powershell/runbook-auth-output.png)

## <a name="add-code-to-start-a-virtual-machine"></a>Bir sanal makineyi başlatmak için kod ekleyin

Azure aboneliğinize, runbook kimlik doğrulaması, kaynakları yönetebilir. Bir sanal makineyi başlatmak için bir komut ekleyin. Artık, şimdilik bu adı için ve Azure aboneliğinizdeki herhangi bir sanal makine seçebilirsiniz.

1. Sonra *Connect-AzureRmAccount*, türü *Start-AzureRmVM-Name 'VMName' - ResourceGroupName 'NameofResourceGroup'* adı ve sanal makinenin kaynak grubu adı sağlar.  

   ```powershell
   # Ensures you do not inherit an AzureRMContext in your runbook
   Disable-AzureRmContextAutosave –Scope Process

   $connection = Get-AutomationConnection -Name AzureRunAsConnection
   Connect-AzureRmAccount -ServicePrincipal -Tenant $connection.TenantID `
   -ApplicationID $connection.ApplicationID -CertificateThumbprint $connection.CertificateThumbprint
   Start-AzureRmVM -Name 'VMName' -ResourceGroupName 'ResourceGroupName'
   ```

1. Runbook'u kaydedin ve ardından **Test bölmesi** böylece test edebilirsiniz.
1. Testi başlatmak için **Başlat**’a tıklayın. Tamamlandığında, sanal makinenin başlatıldığını kontrol edin.

## <a name="add-an-input-parameter"></a>Giriş parametresi Ekle

Runbook'unuzda sanal makine şu anda runbook'ta o, kodlanmış başlatmaz, bunun runbook başlatılırken sanal makineyi belirtmeniz durumunda daha kullanışlı olurdu. Bu işlevi sağlamak için runbook'a girdi parametreleri ekleyin.

1. Parametrelerini ekleyin *VMName* ve *ResourceGroupName* runbook ve bu değişkenleri ile **Start-AzureRmVM** cmdlet'i aşağıdaki örnekte olduğu gibi.

   ```powershell
   Param(
    [string]$VMName,
    [string]$ResourceGroupName
   )
   # Ensures you do not inherit an AzureRMContext in your runbook
   Disable-AzureRmContextAutosave –Scope Process

   $connection = Get-AutomationConnection -Name AzureRunAsConnection
   Connect-AzureRmAccount -ServicePrincipal -Tenant $connection.TenantID `
   -ApplicationID $connection.ApplicationID -CertificateThumbprint $connection.CertificateThumbprint
   Start-AzureRmVM -Name $VMName -ResourceGroupName $ResourceGroupName
   ```

1. Runbook'u kaydedin ve Test bölmesini açın. Şimdi testte kullanılan iki girdi değişkeni için değerleri sağlayabilirsiniz.
1. Test bölmesini kapatın.
1. Runbook’un yeni sürümünü yayımlamak için **Yayımla**’ya tıklayın.
1. Önceki adımda başlattığınız sanal makineyi durdurun.
1. Tıklayın **Tamam** runbook'u başlatın. Başlatacağınız sanal makinenin **VMName** ve **ResourceGroupName** bilgilerini yazın.<br><br> ![Parametre Geçirme](media/automation-first-runbook-textual-powershell/automation-pass-params.png)<br>  
1. Runbook tamamlandığında, sanal makinenin başladığından emin olun.

## <a name="differences-from-powershell-workflow"></a>PowerShell İş Akışı ile arasındaki farklar

PowerShell runbook'ları, PowerShell İş Akışı runbook'ları ile aynı yaşam döngüsü, yetenekler ve yönetim özelliklerine sahiptir, ancak arada bazı farklar ve sınırlamalar vardır:

1. PowerShell runbook'ları, bir derleme adımı içermediklerinden PowerShell İş Akışı runbook'larına göre daha hızlı çalışır.
2. Kontrol noktaları kullanılması sayesinde PowerShell iş akışı runbook'ları desteği, PowerShell iş akışı runbook'ları, runbook içindeki herhangi bir noktasından devam edebilir. PowerShell runbook'ları yalnızca en baştan devam edebilir.
3. PowerShell iş akışı runbook'ları paralel ve seri yürütmeyi destekler. PowerShell runbook'ları yalnızca komutların seri olarak yürütür.
4. PowerShell iş akışı runbook'u, etkinlik, bir komut veya betik bloğu, kendi çalışma alanı olabilir. Bir PowerShell runbook'unda betikteki her şey tek bir çalışma alanında çalıştırılır. Ayrıca yerel bir PowerShell runbook'u ile bir PowerShell İş Akışı runbook'u arasında bazı [söz dizimi farklılıkları](https://technet.microsoft.com/magazine/dn151046.aspx) vardır.

## <a name="next-steps"></a>Sonraki adımlar

* Grafik runbook'ları kullanmaya başlamak için bkz. [İlk grafik runbook uygulamam](automation-first-runbook-graphical.md)
* PowerShell iş akışı runbook'larını kullanmaya başlamak için bkz. [İlk PowerShell iş akışı runbook uygulamam](automation-first-runbook-textual.md)
* Runbook türleri, avantajları ve sınırlamaları hakkında daha fazla bilgi için bkz. [Azure Automation runbook türleri](automation-runbook-types.md)
* PowerShell betik desteği özelliği hakkında daha fazla bilgi için bkz. [Azure Automation’da Yerel PowerShell betik desteği](https://azure.microsoft.com/blog/announcing-powershell-script-support-azure-automation-2/)
