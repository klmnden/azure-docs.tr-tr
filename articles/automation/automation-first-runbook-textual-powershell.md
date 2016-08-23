<properties
    pageTitle="Azure Automation’da ilk PowerShell runbook’um | Microsoft Azure"
    description="Basit bir PowerShell runbook oluşturma, test etme ve yayımlama adımlarını anlatan öğretici."
    services="automation"
    documentationCenter=""
    authors="mgoedtel"
    manager="jwhit"
    editor=""
    keywords="azure powershell, powershell betik öğreticisi, powershell otomasyonu"/>
<tags
    ms.service="automation"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="07/19/2016"
    ms.author="magoedte;sngun"/>

# İlk PowerShell runbook’um

> [AZURE.SELECTOR] - [Grafik](automation-first-runbook-graphical.md) - [PowerShell](automation-first-runbook-textual-PowerShell.md) - [PowerShell İş Akışı](automation-first-runbook-textual.md)  

Bu öğretici, Azure Automation’da bir [PowerShell runbook](automation-runbook-types.md#powershell-runbooks) oluşturulmasını adım adım göstermektedir. Runbook işi durumunun nasıl izleneceğini açıklarken test edip yayımlayacağımız basit bir runbook ile başlayacağız. Ardından, bu runbook’u gerçekten Azure kaynaklarını yönetmek üzere değiştireceğiz. Bu öğreticide gösterilen bir Azure sanal makinesini başlatmaktır. Daha sonra, runbook parametreleri ekleyerek runbook’u daha sağlam hale getireceğiz.

## Ön koşullar

Bu öğreticiyi tamamlamak için aşağıdakiler gerekir:

-   Azure aboneliği. Henüz bir aboneliğiniz yoksa [MSDN abone avantajlarınızı etkinleştirebilir](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) ya da <a href="/pricing/free-account/" target="_blank">[ücretsiz hesap için kaydolabilirsiniz](https://azure.microsoft.com/free/).
-   Runbook’u tutacak ve Azure kaynaklarında kimlik doğrulamasını yapacak bir [Automation hesabı](automation-security-overview.md).  Bu hesabın sanal makineyi başlatma ve durdurma izni olmalıdır.
-   Azure sanal makinesi. Bu makineyi durdurup başlatacağımız için makinenin üretime yönelik olmaması gerekir.

## 1. Adım - Yeni runbook oluşturma

Çıktı olarak *Hello World* metnini veren basit bir runbook oluşturacağız.

1.  Azure Portal’da, Automation hesabınızı açın.  
    Automation hesabı sayfası size bu hesaptaki kaynakların hızlı bir görünümünü sağlar. Birkaç Varlığınız zaten olmalıdır. Bunların çoğu, yeni bir Automation hesabına otomatik olarak dahil edilen modüllerdir. Burada ayrıca [önkoşullarda](#prerequisites) belirtilen Kimlik Bilgileri varlığınız da bulunmalıdır.
2.  Runbook'ların listesini açmak için **Runbook'lar** kutucuğuna tıklayın.  
    ![RunbooksControl](media/automation-first-runbook-textual-powershell/automation-runbooks-control.png)  
3.  **Runbook ekle** düğmesine ve ardından **Yeni bir runbook oluştur**’a tıklayarak yeni bir runbook oluşturun.
4.  Runbook’a *MyFirstRunbook-PowerShell* adını verin.
5.  Bu örneğimizde bir [PowerShell runbook’u](automation-runbook-types.md#powershell-runbooks) oluşturacağız, bu nedenle **Runbook türü** olarak **Powershell**’i seçin.  
    ![Runbook Türü](media/automation-first-runbook-textual-powershell/automation-runbook-type.png)  
6.  Runbook’u oluşturmak için **Oluştur**’a tıklayın ve metin düzenleyicisini açın.

## 2. Adım - Runbook'a kod ekleme

Kodu doğrudan runbook’a yazabilir veya Kitaplık denetiminde cmdlet’leri, runbook’ları ve varlıkları seçebilir ve ilgili parametrelerle bunların runbook’a eklenmesini sağlayabilirsiniz. Bu kılavuzda doğrudan runbook'a yazacağız.

1.  Runbook şu anda boş, *Write-Output "Hello World."* yazın.  
    ![Hello World](media/automation-first-runbook-textual-powershell/automation-helloworld.png)  
2.  **Kaydet**’e tıklayarak runbook’u kaydedin.  
    ![Kaydet Düğmesi](media/automation-first-runbook-textual-powershell/automation-save-button.png)  

## 3. Adım - Runbook'u test etme

Runbook’u üretimde kullanılabilir hale getirmek üzere yayımlamadan önce düzgün çalıştığından emin olmak için test etmek istiyoruz. Bir runbook'u test ettiğinizde, bunun **Taslak** sürümünü çalıştırır ve çıktısını etkileşimli olarak görüntülersiniz.

1.  Test bölmesini açmak için **Test bölmesi**’ne tıklayın.  
    ![Test Bölmesi](media/automation-first-runbook-textual-powershell/automation-testpane.png)  
2.  Testi başlatmak için **Başlat**’a tıklayın. Etkinleştirilen tek seçenek bu olmalıdır.
3.  Bir [runbook işi](automation-runbook-execution.md) oluşturulur ve durumu görüntülenir.  
    İş durumu, bulutta bir runbook çalışanının kullanılabilir hale gelmesinin beklendiğini gösteren şekilde *Sırada* olarak başlar. Ardından, bir çalışan işi talep ettiğinde *Başlatılıyor* olarak ve runbook gerçekten çalışmaya başladığında *Çalışıyor* olarak değiştirilir.  
4.  Runbook işi tamamlandığında çıktısı görüntülenir. Örneğimizde, *Hello World* metnini görmeliyiz.  
    ![Test Bölmesi Çıktısı](media/automation-first-runbook-textual-powershell/automation-testpane-output.png)  
5.  Tuvale geri dönmek için Test bölmesini kapatın.

## 4. Adım - Runbook’u yayımlama ve başlatma

Yeni oluşturduğumuz runbook hala Taslak modunda. Runbook’un üretimde çalıştırılabilmesi için önce yayımlanması gerekir. Bir runbook yayımladığınızda, Taslak sürümü mevcut Yayımlanmış sürümün üzerine yazarsınız. Örneğimizde, runbook’u henüz oluşturduğumuzdan, Yayımlanmış sürümümüz yok.

1.  Runbook’u yayımlamak için **Yayımla**’ya tıklayın ve sorulduğunda **Evet**’e tıklayın.  
    ![Yayımla düğmesi](media/automation-first-runbook-textual-powershell/automation-publish-button.png)  
2.  Şimdi runbook'u **Runbook'lar** bölmesinde görüntülemek için sola kaydırırsanız, **Yazma Durumu** olarak **Yayımlandı** gösterilir.
3.  **MyFirstRunbook-PowerShell**’e ait bölmeyi görüntülemek üzere geri sağa kaydırın.  
    Üst kısımdaki seçenekler runbook’u başlatmamıza, görüntülememize, gelecek bir zamanda başlatılmak üzere zamanlamamıza ya da bir HTTP çağrısıyla başlatılabilmesi için [web kancası](automation-webhooks.md) oluşturmamıza olanak tanır.
4.  Yalnızca runbook'u başlatmak istediğimizden **Başlat**’a tıklayın ve Runbook'u Başlat dikey penceresi açıldığında **Tamam**’a tıklayın.  
    ![Başlat düğmesi](media/automation-first-runbook-textual-powershell/automation-start-button.png)  
5.  Yeni oluşturduğumuz runbook için bir iş bölmesi açıldı. Bu bölme kapatılabilir, ancak bu kez işin ilerleme durumunu izleyebilmek için açık bırakacağız.
6.  İş durumu **İş Özeti**’nde gösterilir ve runbook’u test ettiğimizde gördüğümüz durumların aynısıdır.  
    ![İş Özeti](media/automation-first-runbook-textual-powershell/automation-job-summary.png)  
7.  Runbook durumu olarak *Tamamlandı* gösterilince **Çıktı**’ya tıklayın. Çıktı bölmesi açılır ve *Hello World* metnimizi görebiliriz.  
    ![İş Çıktısı](media/automation-first-runbook-textual-powershell/automation-job-output.png)
8.  Çıktı bölmesini kapatın.
9.  Runbook işine ait Akışlar bölmesini açmak için **Tüm Günlükler**’e tıklayın. Çıktı akışında yalnızca *Hello World* metnini görmeliyiz, ancak bu bölmede, runbook bunlara yazıyorsa Ayrıntılı ve Hata gibi runbook işine yönelik diğer akışlar da gösterilebilir.  
    ![Tüm Günlükler](media/automation-first-runbook-textual-powershell/automation-alllogs.png)  
10. MyFirstRunbook-PowerShell bölmesine dönmek için Akışlar bölmesini ve İş bölmesini kapatın.
11. Bu runbook’a ait İşler bölmesini açmak için **İşler**’e tıklayın. Bu bölmede, bu runbook tarafından oluşturulan tüm işler listelenir. İşi yalnızca bir kez çalıştırdığımız için sadece bir işin listelendiğini görmeliyiz.  
    ![İş Listesi](media/automation-first-runbook-textual-powershell/automation-job-list.png)  
12. Runbook’u başlattığımızda, görüntülediğimiz iş bölmesini açmak için bu işe tıklayabilirsiniz. Böylece zaman içinde geri dönerek, belirli bir runbook için oluşturulan herhangi bir işin ayrıntılarını görüntüleyebilirsiniz.

## 5. Adım- Azure kaynaklarını yönetmek için kimlik doğrulaması ekleme

Runbook uygulamamızı test ettik ve yayımladık, ancak şu ana kadar faydalı bir şey yapmadı. Bu runbook’un Azure kaynaklarını yönetmesini istiyoruz. Runbook‘un bunu gerçekleştirebilmesi için [önkoşullar](#prerequisites) bölümünde bahsedilen kimlik bilgilerini kullanarak kimlik doğrulaması yapması gerekir. Bunun için **Add-AzureRmAccount** cmdlet'ini kullanıyoruz.

1.  MyFirstRunbook-PowerShell bölmesinde **Düzenle**’ye tıklayarak metin düzenleyicisini açın.  
    ![Runbook’u Düzenleme](media/automation-first-runbook-textual-powershell/automation-edit-runbook.png)  
2.  **Write-Output** satırı artık gerekli değil, bu nedenle silebilirsiniz.
3.  Automation Farklı Çalıştır hesabınızla kimlik doğrulamasını işleyecek aşağıdaki kodu yazın veya kopyalayıp yapıştırın:

    ```
     $Conn = Get-AutomationConnection -Name AzureRunAsConnection 
     Add-AzureRMAccount -ServicePrincipal -Tenant $Conn.TenantID `
     -ApplicationId $Conn.ApplicationID -CertificateThumbprint $Conn.CertificateThumbprint
    ``` 
<br>
4.  Runbook’u test edebilmemiz için **Test bölmesine** tıklayın.
5.  Testi başlatmak için **Başlat**’a tıklayın. Tamamlandığında, hesabınızdaki temel bilgileri görüntüleyen bir aşağıdakine benzer bir çıktı almalısınız. Böylece kimlik bilgisinin geçerli olduğunu doğrulanmış olur. <br> ![Kimlik doğrulaması](media/automation-first-runbook-textual-powershell/runbook-auth-output.png)

## 6. Adım - Sanal makineyi başlatmak için kod ekleme

Runbook uygulamamız Azure aboneliğimiz için kimlik doğrulaması yaptığına göre, kaynakları yönetebiliriz. Sanal makineyi başlatmak için bir komut ekleyeceğiz. Azure aboneliğinizdeki herhangi bir sanal makineyi seçebilirsiniz, şimdilik bu adı cmdlet’e kod olarak ekleyeceğiz.

1.  *Add-AzureRmAccount* ’un ardından, başlatılacak sanal makinenin adını ve Kaynak Grubu adını girip *Start-AzureRmVM -Name 'VMName' -ResourceGroupName 'NameofResourceGroup'* yazın.  
    
    ```
     $Conn = Get-AutomationConnection -Name AzureRunAsConnection 
     Add-AzureRMAccount -ServicePrincipal -Tenant $Conn.TenantID `
     -ApplicationID $Conn.ApplicationID -CertificateThumbprint $Conn.CertificateThumbprint 
     Start-AzureRmVM -Name 'VMName' -ResourceGroupName 'ResourceGroupName'
     ```
<br>
2.  Runbook'u kaydedin ve ardından **Test bölmesi**’ne tıklayın böylece test edebiliriz.
3.  Testi başlatmak için **Başlat**’a tıklayın. Tamamlandığında, sanal makinenin başlatıldığını kontrol edin.

## 7. Adım - Runbook'a girdi parametresi ekleme

Runbook uygulamamız şu anda, runbook’a kod olarak eklediğimiz sanal makineyi başlatır, ancak runbook başlatılırken sanal makineyi belirtebilseydik daha kullanışlı olurdu. Şimdi bu işlevi sağlamak için runbook’a girdi parametreleri ekleyeceğiz.

1.  Runbook’a *VMName* ve *ResourceGroupName* parametrelerini ekleyin ve bu değişkenleri aşağıdaki örnekteki gibi **Start-AzureRmVM** cmdlet’i ile kullanın.  
    
    ```
    Param(
       [string]$VMName,
       [string]$ResourceGroupName
    )
     $Conn = Get-AutomationConnection -Name AzureRunAsConnection 
     Add-AzureRMAccount -ServicePrincipal -Tenant $Conn.TenantID `
     -ApplicationID $Conn.ApplicationID -CertificateThumbprint $Conn.CertificateThumbprint 
     Start-AzureRmVM -Name $VMName -ResourceGroupName $ResourceGroupName
     ```
<br> 
2.  Runbook'u kaydedin ve Test bölmesini açın. Şimdi testte kullanılacak olan iki girdi değişkeni için değerleri sağlayabileceğinizi unutmayın.
3.  Test bölmesini kapatın.
4.  Runbook’un yeni sürümünü yayımlamak için **Yayımla**’ya tıklayın.
5.  Önceki adımda başlattığınız sanal makineyi durdurun.
6.  Runbook'u başlatmak için **Başlat**’a tıklayın. Başlatacağınız sanal makinenin **VMName** ve **ResourceGroupName** bilgilerini yazın.  
    ![Parametre Geçir](media/automation-first-runbook-textual-powershell/automation-pass-params.png)  
7.  Runbook tamamlandığında, sanal makinenin başladığından emin olun.

## PowerShell İş Akışı ile arasındaki farklar

PowerShell runbook'ları, PowerShell İş Akışı runbook'ları ile aynı yaşam döngüsü, yetenekler ve yönetim özelliklerine sahiptir ancak arada bazı farklar ve sınırlamalar vardır:

1.  PowerShell runbook'ları, bir derleme adımı içermediklerinden PowerShell İş Akışı runbook'larına göre daha hızlı çalışır.
2.  PowerShell İş Akışı runbook'ları, kontrol noktalarını destekler. Kontrol noktalarının kullanılması sayesinde PowerShell İş Akışı runbook'ları, runbook içerisinde herhangi bir noktadan sürdürülebilirken, PowerShell runbook'ları yalnızca başlangıç noktasından sürdürülebilir.
3.  PowerShell İş Akışı runbook'ları paralel ve seri yürütmeyi desteklerken, PowerShell runbook'ları yalnızca komutların seri olarak yürütülmesini destekler.
4.  PowerShell İş Akışı runbook'unda bir etkinliğin, komutun veya betik bloğunun kendi çalışma alanı bulunabilirken, PowerShell runbook'unda betikteki her şey tek bir çalışma alanında çalıştırılır. Ayrıca yerel bir PowerShell runbook'u ile bir PowerShell İş Akışı runbook'u arasında bazı [söz dizimi farklılıkları](https://technet.microsoft.com/magazine/dn151046.aspx) vardır.

## Sonraki adımlar

-   Grafik runbook'ları kullanmaya başlamak için bkz. [İlk grafik runbook uygulamam](automation-first-runbook-graphical.md)
-   PowerShell iş akışı runbook'larını kullanmaya başlamak için bkz. [İlk PowerShell iş akışı runbook uygulamam](automation-first-runbook-textual.md)
-   Runbook türleri, avantajları ve sınırlamaları hakkında daha fazla bilgi için bkz. [Azure Automation runbook türleri](automation-runbook-types.md)
-   PowerShell betik desteği özelliği hakkında daha fazla bilgi için bkz. [Azure Automation’da Yerel PowerShell betik desteği](https://azure.microsoft.com/blog/announcing-powershell-script-support-azure-automation-2/)



<!--HONumber=Aug16_HO1-->


