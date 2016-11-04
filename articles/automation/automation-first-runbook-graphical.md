---
title: Azure Automation’da ilk grafik runbook uygulamam | Microsoft Docs
description: Basit bir grafik runbook uygulaması oluşturma, test etme ve yayımlama adımlarını anlatan öğretici.
services: automation
documentationcenter: ''
author: mgoedtel
manager: jwhit
editor: ''
keywords: runbook, runbook şablonu, runbook otomasyonu, azure runbook

ms.service: automation
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/06/2016
ms.author: magoedte;bwren

---
# İlk grafik runbook uygulamam
> [AZURE.SELECTOR] - [Grafik](automation-first-runbook-graphical.md) - [PowerShell](automation-first-runbook-textual-PowerShell.md) - [PowerShell İş Akışı](automation-first-runbook-textual.md)
> 
> 

Bu öğretici, Azure Automation’da bir [grafik runbook uygulaması](automation-runbook-types.md#graphical-runbooks) oluşturulmasını adım adım göstermektedir.  Runbook işi durumunun nasıl izleneceğini açıklarken test edip yayımlayacağımız basit bir runbook ile başlayacağız.  Ardından, bu runbook’u gerçekten Azure kaynaklarını yönetmek üzere değiştireceğiz. Bu öğreticide gösterilen bir Azure sanal makinesini başlatmaktır.  Daha sonra, runbook parametreleri ve koşula bağlı bağlantılar ekleyerek runbook’u daha sağlam hale getireceğiz.

## Ön koşullar
Bu öğreticiyi tamamlamak için aşağıdakiler gerekir:

* Azure aboneliği.  Henüz bir aboneliğiniz yoksa [MSDN abone avantajlarınızı etkinleştirebilir](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) ya da <a href="/pricing/free-account/" target="_blank">[ücretsiz hesap için kaydolabilirsiniz](https://azure.microsoft.com/free/).
* Runbook’u tutacak ve Azure kaynaklarında kimlik doğrulamasını yapacak bir [Azure Farklı Çalıştır Hesabı](automation-sec-configure-azure-runas-account.md).  Bu hesabın sanal makineyi başlatma ve durdurma izni olmalıdır.
* Azure sanal makinesi.  Bu makineyi durdurup başlatacağımız için makinenin üretime yönelik olmaması gerekir.

## 1. Adım - Yeni runbook oluşturma
Çıktı olarak *Hello World* metnini veren basit bir runbook oluşturacağız.

1. Azure Portal’da, Automation hesabınızı açın.  
   Automation hesabı sayfası size bu hesaptaki kaynakların hızlı bir görünümünü sağlar.  Birkaç Varlığınız zaten olmalıdır.  Bunların çoğu, yeni bir Automation hesabına otomatik olarak dahil edilen modüllerdir.  Burada ayrıca [önkoşullarda](#prerequisites) belirtilen Kimlik Bilgileri varlığınız da bulunmalıdır.
2. Runbook'ların listesini açmak için **Runbook'lar** kutucuğuna tıklayın.<br> ![Runbook denetimi](media/automation-first-runbook-graphical/runbooks-control.png)
3. **Runbook ekle** düğmesine ve ardından **Yeni bir runbook oluştur**’a tıklayarak yeni bir runbook oluşturun.
4. Runbook’a *MyFirstRunbook-Graphical* adını verin.
5. Bu örneğimizde bir [grafik runbook uygulaması](automation-graphical-authoring-intro.md) oluşturacağız, bu nedenle **Runbook türü** olarak **Grafik**’i seçin.<br> ![Yeni runbook](media/automation-first-runbook-graphical/create-new-runbook.png)<br>
6. Runbook’u oluşturmak için **Oluştur**’a tıklayın ve grafik düzenleyicisini açın.

## 2. Adım - Runbook’a etkinlikler ekleme
Düzenleyicinin sol tarafındaki Kitaplık denetimi runbook uygulamanıza eklenecek etkinlikleri seçmenizi sağlar.  Runbook uygulamasından çıktı metnine **Write-Output** cmdlet’ini ekleyeceğiz.

1. Kitaplık denetiminde, arama metin kutusuna tıklayın ve **Write-Output** yazın.  Arama sonuçları altında görüntülenir. <br> ![Microsoft.PowerShell.Utility](media/automation-first-runbook-graphical/search-powershell-cmdlet-writeoutput.png)
2. Listenin aşağısına kaydırın.  **Write-Output**’a sağ tıklayıp **Tuvale ekle**’yi seçebilir ya da cmdlet’in yanındaki elipse tıklayıp ardından **Tuvale ekle**’yi seçebilirsiniz.
3. Tuvalde **Write-Output** etkinliğine tıklayın.  Bu, etkinliği yapılandırmanızı sağlayan Yapılandırma denetimi dikey penceresini açar.
4. **Etiket** cmdlet’in adını varsayılan olarak alır ancak bunu daha kolay bir şeyle değiştirebiliriz. Bunu *çıkışa Hello World yazmak* üzere değiştirin.
5. Cmdlet parametreleri değerlerini sağlamak için **Parametreler**’e tıklayın.   
   Bazı cmdlet’ler birden fazla parametre kümesine sahiptir ve kullanacağınızı seçmeniz gerekir. Bu durumda, **Write-Output** yalnızca bir parametre kümesine sahip olur, bu nedenle seçmeniz gerekmez. <br> ![Write-Output özellikleri](media/automation-first-runbook-graphical/write-output-properties-b.png)
6. **InputObject** parametresini seçin.  Bu, çıktı akışına göndermek üzere metni belirleyeceğimiz parametredir.
7. **Veri kaynağı** açılır listesinde, **PowerShell ifadesi**’nı seçin.  **Veri kaynağı** açılır penceresi bir parametre değerini doldurmak için kullandığınız farklı kaynaklar sağlar.  
   Başka bir etkinlik, Automation varlığı ya da PowerShell ifadesi gibi böyle kaynaklardan alınan çıktıları kullanabilirsiniz.  Bu durumda, yalnızca *Hello World* için metin çıktısı istiyoruz. Bir PowerShell ifadesi kullanabilir ve dize belirtebiliriz.
8. **İfade** kutusuna *"Hello World"* yazın ve ardından tuvale döndürmek için iki kez **Tamam**’a tıklayın.<br> ![PowerShell ifadesi](media/automation-first-runbook-graphical/expression-hello-world.png)
9. **Kaydet**’e tıklayarak runbook’u kaydedin.<br> ![Runbook'u kaydet](media/automation-first-runbook-graphical/runbook-toolbar-save-revised20165.png)

## 3. Adım - Runbook'u test etme
Runbook’u üretimde kullanılabilir hale getirmek üzere yayımlamadan önce düzgün çalıştığından emin olmak için test etmek istiyoruz.  Bir runbook'u test ettiğinizde, bunun **Taslak** sürümünü çalıştırır ve çıktısını etkileşimli olarak görüntülersiniz.

1. Test dikey penceresini açmak için **Test bölmesi**’ne tıklayın.<br> ![Test bölmesi](media/automation-first-runbook-graphical/runbook-toolbar-test-revised20165.png)
2. Testi başlatmak için **Başlat**’a tıklayın.  Etkinleştirilen tek seçenek bu olmalıdır.
3. Bölmede bir [runbook işi](automation-runbook-execution.md) oluşturulur ve durumu görüntülenir.  
   İş durumu, bulutta bir runbook çalışanının kullanılabilir hale gelmesinin beklendiğini gösteren şekilde *Sırada* olarak başlar.  Ardından, bir çalışan işi talep ettiğinde *Başlatılıyor* olarak ve runbook gerçekten çalışmaya başladığında *Çalışıyor* olarak değiştirilir.  
4. Runbook işi tamamlandığında çıktısı görüntülenir. Örneğimizde, *Hello World* metnini görmeliyiz.<br> ![Hello World](media/automation-first-runbook-graphical/runbook-test-results.png)
5. Tuvale geri dönmek için Test dikey penceresini kapatın.

## 4. Adım - Runbook’u yayımlama ve başlatma
Yeni oluşturduğumuz runbook hala Taslak modunda. Runbook’un üretimde çalıştırılabilmesi için önce yayımlanması gerekir.  Bir runbook yayımladığınızda, Taslak sürümü mevcut Yayımlanmış sürümün üzerine yazarsınız.  Örneğimizde, runbook’u henüz oluşturduğumuzdan, Yayımlanmış sürümümüz yok.

1. Runbook’u yayımlamak için **Yayımla**’ya tıklayın ve sorulduğunda **Evet**’e tıklayın.<br> ![Yayımlama](media/automation-first-runbook-graphical/runbook-toolbar-publish-revised20166.png)
2. Şimdi runbook'u **Runbook'lar** dikey penceresinde görüntülemek için sola kaydırırsanız, **Yazma Durumu** olarak **Yayımlandı** gösterilir.
3. **MyFirstRunbook** için dikey pencereyi görüntülemek üzere geri sağa kaydırın.  
   Üst kısımdaki seçenekler runbook’u başlatmamıza, gelecekte bir zamanda başlatmak üzere zamanlamamıza ya da bir HTTP çağrısıyla başlatılabilmesi için [web kancası](automation-webhooks.md) oluşturmamıza olanak tanır.
4. Yalnızca runbook'u başlatmak istiyoruz, bu nedenle **Başlat**’a ve ardından sorulduğunda **Evet**’e tıklayın.<br> ![Runbook’u başlatma](media/automation-first-runbook-graphical/runbook-controls-start-revised20165.png)
5. Yeni oluşturduğumuz runbook için bir iş dikey penceresi açıldı.  Bu dikey pencere kapatılabilir, ancak bu kez işin ilerleme durumunu izleyebilmek için açık bırakacağız.
6. İş durumu **İş Özeti**’nde gösterilir ve runbook’u test ettiğimizde gördüğümüz durumların aynısıdır.<br> ![İş Özeti](media/automation-first-runbook-graphical/runbook-job-summary.png)
7. Runbook durumu olarak *Tamamlandı* gösterilince **Çıktı**’ya tıklayın. **Çıktı** dikey penceresi açılır ve bölmede *Hello World* metnimizi görebiliriz.<br> ![İş Özeti](media/automation-first-runbook-graphical/runbook-job-output.png)  
8. Çıktı dikey penceresini kapatın.
9. Runbook işine ait Akışlar dikey penceresini açmak için **Tüm Günlükler**’e tıklayın.  Çıktı akışında yalnızca *Hello World* metnini görmeliyiz, ancak bu bölmede, runbook bunlara yazıyorsa Ayrıntılı ve Hata gibi runbook işine yönelik diğer akışlar da gösterilebilir.<br> ![İş Özeti](media/automation-first-runbook-graphical/runbook-job-AllLogs.png)
10. Tüm Günlükler dikey penceresini ve İş dikey penceresini kapatarak MyFirstRunbook dikey penceresine dönün.
11. Bu runbook’a ait İşler dikey penceresini açmak için **İşler**’e tıklayın.  Bu bölmede, bu runbook tarafından oluşturulan tüm işler listelenir. İşi yalnızca bir kez çalıştırdığımız için sadece bir işin listelendiğini görmeliyiz.<br> ![İşler](media/automation-first-runbook-graphical/runbook-control-jobs.png)
12. Runbook’u başlattığımızda, görüntülediğimiz iş bölmesini açmak için bu işe tıklayabilirsiniz.  Böylece zaman içinde geri dönerek, belirli bir runbook için oluşturulan herhangi bir işin ayrıntılarını görüntüleyebilirsiniz.

## 5. Adım - Değişken varlıkları oluşturma
Runbook uygulamamızı test ettik ve yayımladık, ancak şu ana kadar faydalı bir şey yapmadı. Bu runbook’un Azure kaynaklarını yönetmesini istiyoruz.  Kimlik doğrulaması için runbook uygulamamızı yapılandırmadan önce, aşağıda 6. adımda kimlik doğrulamak üzere abonelik kimliğini tutmak için bir değişken oluşturacağız ve etkinliği ayarladıktan sonra buna başvuracağız.  Abonelik bağlamına başvuru eklemek birden fazla abonelik arasından kolayca çalışmanızı sağlar.  Devam etmeden önce, Gezinti bölmesindeki Abonelik seçeneği kapalı’daki abonelik kimliğinizi kopyalayın.  

1. Automation hesapları dikey penceresinde, **Varlıklar** kutucuğuna tıklayın, **Varlıklar** dikey penceresi açılır.
2. Varlıklar dikey penceresinde, **Değişkenler** kutucuğuna tıklayın.
3. Değişkenleri dikey penceresinde, **Değişken ekle**’ye tıklayın.<br>![Automation Değişkeni](media/automation-first-runbook-graphical/create-new-subscriptionid-variable.png)
4. Yeni değişken dikey penceresinde, **Ad** kutusuna, **AzureSubscriptionId** girin ve **Değer** kutusuna Abonelik kimliğinizi yazın.  **Tür** için *dizeyi* **Şifreleme** için değeri koruyun.  
5. Değişkeni oluşturmak için **Oluştur**’a tıklayın.  

## 6. Adım- Azure kaynaklarını yönetmek için kimlik doğrulaması ekleme
Abonelik kimliğimizi tutmak üzere bir değişkene sahip olduğumuza göre, runbook uygulamamızı [ön koşullarda](#prerequisites) başvurulan Farklı Çalıştır kimlik bilgileri ile kimlik doğrulamak üzere yapılandırabiliriz.  Bunu tuvale Azure Farklı Çalıştır bağlantısı **Varlığı** ve **Add-AzureRMAccount** cmdlet’i ekleyerek yaparız.  

1. MyFirstRunbook dikey penceresinde **Düzenle**’ye tıklayarak grafik düzenleyicisini açın.<br> ![Runbook’u düzenleme](media/automation-first-runbook-graphical/runbook-controls-edit-revised20165.png)
2. Artık **çıkışa Hello World yazmamız** gerekmez, bu nedenle sağ tıklayın ve **Sil**’i seçin.
3. Kitaplık denetiminde, **Bağlantılar**’ı genişletin ve **Tuvale Ekle**’yi seçerek **AzureRunAsConnection**’ı ekleyin.
4. Tuvalde, **AzureRunAsConnection**’ı seçin ve Yapılandırma denetim bölmesinde **Etiket** metin kutusuna **Farklı Çalıştır Bağlantısını Al** yazın.  Bu bağlantıdır 
5. Kitaplık denetiminde, arama metin kutusuna **Add-AzureRmAccount** yazın.
6. Tuvale **Add-AzureRmAccount** ekleme<br> ![Add-AzureRMAccount](media/automation-first-runbook-graphical/search-powershell-cmdlet-addazurermaccount.png)
7. Şeklin altında bir daire görünene kadar **Farklı Çalıştır Bağlantısını Al** üzerinde bekleyin. Daireye tıklayın ve oku **Add-AzureRmAccount**’a sürükleyin.  Yeni oluşturduğunuz ok bir *bağlantıdır*.  Runbook **Farklı Çalıştır Bağlantısını Al** ile başlar ve ardından **Add-AzureRmAccount**’ı çalıştırır.<br> ![Etkinlikler arasında bağlantı oluşturma](media/automation-first-runbook-graphical/runbook-link-auth-activities.png)
8. Tuvalde, **Add-AzureRmAccount**’ı seçin ve Yapılandırma denetim bölmesinde **Etiket** metin kutusuna **Azure’da Oturum Aç** yazın.
9. **Parametreler**’e tıklayın, Parametre Yapılandırma dikey penceresi görünür. 
10. **Add-AzureRmAccount** birden fazla parametre kümesine sahiptir, bu nedenle parametre değerleri sağlamadan önce birini seçmeliyiz.  **Parametre Kümesi**’ne tıklayın ve ardından **ServicePrincipalCertificate** parametre kümesine tıklayın. 
11. Parametre kümesini seçtiğinizde, parametreleri Etkinlik Parametresi yapılandırma dikey penceresinde parametreler görüntülenir.  **APPLICATIONID**’ye tıklayın.<br> ![Azure RM hesabı parametreleri ekleme](media/automation-first-runbook-graphical/add-azurermaccount-parameterset.png)
12. Parametre değeri dikey penceresinde **Veri kaynağı** için **Etkinlik çıkışı**’nı seçin ve listede **Farklı Çalıştır Bağlantısını Al**’ı seçin, **Alan yolu** metin kutusuna **ApplicationId** yazın ve ardından **Tamam**’a tıklayın.  Etkinlik birden fazla özelliğe sahip bir nesne çıkardığından, Alan yolu için özellik adını belirtiyoruz.
13. **CERTIFICATETHUMBPRINT**’ tıklayın ve Parametre Değeri dikey penceresinde, **Veri kaynağı** için **Etkinlik çıkışı**’nı seçin.  Listede **Farklı Çalıştır Bağlantısını Al**’ı seçin **Alan yolu** metin kutusuna **CertificateThumbprint** yazın ve ardından **Tamam**’a tıklayın. 
14. **SERVICEPRINCIPAL**’e tıklayın ve Parametre Değeri dikey penceresinde, **Veri kaynağı** için **ConstantValue** ‘yu seçin, **True** seçeneğine tıklayın ve **Tamam**’a tıklayın.
15. **TENANTID**’ye tıklayın ve Parametre Değeri dikey penceresinde, **Veri kaynağı** için **Etkinlik çıkışı**’nı seçin.  Listede **Farklı Çalıştır Bağlantısını Al**’ı seçin **Alan yolu** metin kutusuna **TenantId** yazın ve ardından iki kez **Tamam**’a tıklayın.  
16. Kitaplık denetiminde, arama metin kutusuna **Set-AzureRmContext** yazın.
17. Tuvale **Set-AzureRmContext** ekleme
18. Tuvalde, **Set-AzureRmContext**’i seçin ve Yapılandırma denetim bölmesinde **Etiket** metin kutusuna **Abonelik Kimliği Belirt** yazın.
19. **Parametreler**’e tıklayın, Parametre Yapılandırma dikey penceresi görünür. 
20. **Set-AzureRmContext** birden fazla parametre kümesine sahiptir, bu nedenle parametre değerleri sağlamadan önce birini seçmeliyiz.  **Parametre Kümesi**’ne tıklayın ve ardından **SubscriptionId** parametre kümesini seçin.  
21. Parametre kümesini seçtiğinizde, parametreleri Etkinlik Parametresi yapılandırma dikey penceresinde parametreler görüntülenir.  **SubscriptionID**’e tıklayın.
22. Parametre Değeri dikey penceresinde, **Veri kaynağı** için **Değişken Varlığı**’nı seçin ve listede **AzureSubscriptionId**’yi seçin ve iki kez **Tamam**’a tıklayın.   
23. Şeklin altında bir daire görünene kadar **Azure’da Oturum Aç** üzerinde bekleyin. Daireye tıklayın ve oku **Abonelik Kimliği Belirt**’e sürükleyin.

Runbook'unuzda bu noktada aşağıdakine benzer: <br>![Runbook kimlik doğrulama yapılandırması](media/automation-first-runbook-graphical/runbook-auth-config.png)

## 7. Adım - Sanal makineyi başlatmak üzere etkinlik ekleme
Şimdi bir sanal makineyi başlatmak için **Start-AzureRmVM** etkinliği ekleyeceğiz.  Azure aboneliğinizdeki herhangi bir sanal makineyi seçebilirsiniz, şimdilik bu adı cmdlet’e kod olarak ekleyeceğiz.

1. Kitaplık denetiminde, arama metin kutusuna **Start-AzureRm** yazın.
2. Tuvale **Start-AzureRmVM** ekleyin ve ardından **Abonelik Kimliği Belirt** altına tıklayarak sürükleyin.
3. Şeklin altında bir daire görünene kadar **Abonelik Kimliği Belirt** üzerinde bekleyin.  Daireye tıklayın ve oku **Start-AzureRmVM**’ye sürükleyin. 
4. **Start-AzureRmVM**’yi seçin.  **Start-AzureRmVM** için kümeleri görüntülemek üzere **Parametreler**’i ve ardından **Parametre kümesi**’ni seçin.  **ResourceGroupNameParameterSetName** parametre kümesini seçin. **ResourceGroupName** ve **Ad**’ın yanında ünlem işareti olduğuna dikkat edin.  Bu, bunların gerekli parametreler olduğunu gösterir.  Ayrıca, her ikisinin de dize değerleri beklediğini unutmayın.
5. **Ad**’ı seçin.  **Veri Kaynağı** için **PowerShell ifadesi**’ni seçin ve çift tırnakların arasına, bu runbook uygulamasını başlatacağımız sanal makine adını yazın.  **Tamam**’a tıklayın.<br>![Start-AzureRmVM Adı Parametre Değeri](media/automation-first-runbook-graphical/runbook-startvm-nameparameter.png)
6. **ResourceGroupName**’i seçin. **Veri Kaynağı** için **PowerShell ifadesi**’ni seçin ve çift tırnakların arasına kaynak grubu adını yazın.  **Tamam**’a tıklayın.<br> ![Start-AzureRmVM Parametreleri](media/automation-first-runbook-graphical/startazurermvm-params.png)
7. Runbook’u test edebilmemiz için Test bölmesine tıklayın.
8. Testi başlatmak için **Başlat**’a tıklayın.  Tamamlandığında, sanal makinenin başlatıldığını kontrol edin.

Runbook'unuzda bu noktada aşağıdakine benzer: <br>![Runbook kimlik doğrulama yapılandırması](media/automation-first-runbook-graphical/runbook-startvm.png)

## 8. Adım - Runbook’a ek giriş parametreleri ekleme
Runbook uygulamamız şu anda **Start-AzureRmVM** cmdlet’inden belirttiğimiz kaynak grubundaki sanal makinede başlamış durumda, ancak runbook uygulamamız runbook uygulamamızın ne zaman başlatılacağını seçseydik daha faydalı olurdu.  Şimdi bu işlevi sağlamak için runbook’a girdi parametreleri ekleyeceğiz.

1. **MyFirstRunbook** bölmesinde **Düzenle**’ye tıklayarak grafik düzenleyicisini açın.
2. **Giriş ve çıkış**’a tıklayın ve ardından Runbook Giriş Parametresi bölmesini açmak için **Giriş ekle**’ye tıklayın.<br> ![Runbook Giriş ve Çıkış](media/automation-first-runbook-graphical/runbook-toolbar-InputandOutput-revised20165.png)
3. **Ad** için *VMName* belirtin.  **Tür** *dizesini* koruyun ancak, **Zorunlu**’yu *Evet* olarak değiştirin.  **Tamam**’a tıklayın.
4. *ResourceGroupName* adlı ikinci bir zorunlu giriş parametresi oluşturun ve ardından **Giriş ve Çıkış** bölmesini kapatmak için **Tamam**’a tıklayın.<br> ![Runbook Giriş Parametreleri](media/automation-first-runbook-graphical/start-azurermvm-params-outputs.png)
5. **Start-AzureRmVM** etkinliğini seçin ve ardından **Parametreler**’e tıklayın.
6. **Ad** için **Veri kaynağı**’nı, **Runbook girişi** olarak değiştirin ve ardından **VMName**’i seçin.<br>
7. **ResourceGroupName** için **Veri kaynağı**’nı, **Runbook girişi** olarak değiştirin ve ardından **ResourceGroupName**’i seçin.<br> ![Start-AzureVM Parametreleri](media/automation-first-runbook-graphical/start-azurermvm-params-runbookinput.png)
8. Runbook'u kaydedin ve Test bölmesini açın.  Şimdi testte kullanılacak olan iki girdi değişkeni için değerleri sağlayabileceğinizi unutmayın.
9. Test bölmesini kapatın.
10. Runbook’un yeni sürümünü yayımlamak için **Yayımla**’ya tıklayın.
11. Önceki adımda başlattığınız sanal makineyi durdurun.
12. Runbook'u başlatmak için **Başlat**’a tıklayın.  Başlatacağınız sanal makinenin **VMName** ve **ResourceGroupName** bilgilerini yazın.<br> ![Runbook’u Başlatma](media/automation-first-runbook-graphical/runbook-start-inputparams.png)
13. Runbook tamamlandığında, sanal makinenin başladığından emin olun.

## 9. Adım - Koşullu bağlantı oluşturma
Hala başlatılmamışsa, yalnızca sanal makineyi başlatmayı deneyecek şekilde runbook’u değiştireceğiz.  Bunu, runbook’a sanal makinenin örnek düzeyi durumunu alacak **Get-AzureRmVM** cmdlet’ini ekleyerek yapacağız. Ardından, sanal makine durumunun çalışıyor mu yoksa durduruldu mu olduğunu belirlemek amacıyla PowerShell kodu iş parçacığıyla birlikte **Durumu Al** adlı PowerShell İş Akışı kodu modülü ekleyeceğiz.  **Durumu Al** modülünden alınan bir koşullu bağlantı yalnızca, geçerli çalışma durumu durduruldu ise, **Start-AzureRmVM** cmdlet’ini çalıştıracak.  Son olarak, sanal makinin başarıyla başlatılıp başlatılmadığını veya PowerShell Write-Output cmdlet’ini kullanmadığını size bildirmek üzere bir çıktı mesajı göndereceğiz.

1. Grafik düzenleyicisinde **MyFirstRunbook**’u açın.
2. Üzerine tıklayarak ve ardından *Sil* tuşuna basarak **Specify Subscription Id** ve **Start-AzureRmVM** arasındaki bağlantıyı kaldırın.
3. Kitaplık denetiminde, arama metin kutusuna **Get-AzureRm** yazın.
4. Tuvale **Get-AzureRmVM** ekleyin.
5. **Get-AzureRmVM** için kümeleri görüntülemek üzere **Get-AzureRmVM**’yi ve ardından **Parametre kümesi**’ni seçin.  **GetVirtualMachineInResourceGroupNameParamSet** parametre kümesini seçin.  **ResourceGroupName** ve **Ad**’ın yanında ünlem işareti olduğuna dikkat edin.  Bu, bunların gerekli parametreler olduğunu gösterir.  Ayrıca, her ikisinin de dize değerleri beklediğini unutmayın.
6. **Ad** için **Veri kaynağı** altında, **Runbook girişi**’ni ve ardından **VMName**’i seçin.  **Tamam**’a tıklayın.
7. **ResourceGroupName** için **Veri kaynağı** altında, **Runbook girişi**’ni ve ardından **ResourceGroupName**’i seçin.  **Tamam**’a tıklayın.
8. **Durum** için **Veri kaynağı** altında, **Sabit değer**’i seçin ve ardından **True**’ya tıklayın.  **Tamam**’a tıklayın.  
9. **Abonelik Kimliği Belirt**’ten **Get-AzureRmVM**’ye bir bağlantı oluşturun.
10. Kitaplık denetiminde, **Runbook Denetimi**’ni genişletin ve tuvale **Kod** ekleyin.  
11. **Get-AzureRmVM**’den **Kod**’a bir bağlantı oluşturun.  
12. **Kod**’a tıklayın ve Yapılandırma bölmesinde etiketi **Durumu Al** olarak değiştirin.
13. **Kod** parametresini seçin, **Kod Düzenleyicisi** dikey penceresi görünür.  
14. Kod düzenleyicisine, aşağıdaki kod parçacığını yapıştırın:
    
     ```
     $StatusesJson = $ActivityOutput['Get-AzureRmVM'].StatusesText 
     $Statuses = ConvertFrom-Json $StatusesJson 
     $StatusOut ="" 
     foreach ($Status in $Statuses){ 
     if($Status.Code -eq "Powerstate/running"){$StatusOut = "running"} 
     elseif ($Status.Code -eq "Powerstate/deallocated") {$StatusOut = "stopped"} 
     } 
     $StatusOut 
     ```
15. **Durumu Al**’dan **Start-AzureRmVM**’ye bir bağlantı oluşturun.<br> ![Kod Modülü ile Runbook](media/automation-first-runbook-graphical/runbook-startvm-get-status.png)  
16. Bağlantıyı seçin ve Yapılandırma bölmesinde, **Koşul uygula**’yı **Evet** olarak değiştirin.   Bağlantının, hedef etkinliğin yalnızca koşulun true olarak çözümlemesi halinde çalıştırılacağını belirten kesikli çizgiye döndüğüne dikkat edin.  
17. **Koşul ifadesi** için *$ActivityOutput['Get Status'] -eq "Stopped"* yazın.  **Start-AzureRmVM** artık yalnızca sanal makine durursa çalışır.
18. Kitaplık denetiminde, **Cmdlet'leri** ve ardından **Microsoft.PowerShell.Utility**’yi genişletin.
19. Tuvale iki kez **Write-Output** ekleyin.<br> ![Write-Output ile Runbook](media/automation-first-runbook-graphical/runbook-startazurermvm-complete.png)
20. İlk **Write-Output** denetiminde, **Parametreler**’e tıklayın ve **Etiket** değerini *VM Başlatıldığında Bildir* olarak değiştirin.
21. **InputObject** için, **Veri kaynağını** **PowerShell ifadesi** olarak değiştirin ve ifadeye *"$VMName successfully started."* yazın.
22. Birinci **Write-Output** denetiminde, **Parametreler**’e tıklayın ve **Etiket** değerini *VM Başlatılamadığında Bildir* olarak değiştirin
23. **InputObject** için, **Veri kaynağını** **PowerShell ifadesi** olarak değiştirin ve ifadeye *"$VMName could not start."* yazın.
24. **Start-AzureRmVM**’den **VM Başlatıldığında Bildir** ve **VM Başlatma Başarısız Olduğunda Bildir**’e bir bağlantı oluşturun.
25. **VM Başlatıldığında Bildir** bağlantısını seçin ve **Koşul uygula**’yı **True** olarak değiştirin.
26. **Koşul ifadesi** için, *$ActivityOutput['Start-AzureRmVM'].IsSuccessStatusCode -eq $true* yazın.  Write-Output denetimi artık yalnızca sanal makine başarıyla başlatıldığında çalışır.
27. **VM Başlatma Başarısız Olduğunda Bildir** bağlantısını seçin ve **Koşul uygula**’yı **True** olarak değiştirin.
28. **Koşul ifadesi** için, *$ActivityOutput['Start-AzureRmVM'].IsSuccessStatusCode -ne $true* yazın.  Write-Output denetimi artık yalnızca sanal makine başarıyla başlatılmadığında çalışır.
29. Runbook'u kaydedin ve Test bölmesini açın.
30. Sanal makine kapalı iken runbook’u çalıştırın, başlamalıdır.

## Sonraki adımlar
* Grafik Yazma hakkında daha fazla bilgi için bkz. [Azure Automation’da grafik yazma](automation-graphical-authoring-intro.md)
* PowerShell runbook'ları kullanmaya başlamak için bkz. [İlk PowerShell runbook uygulamam](automation-first-runbook-textual-powershell.md)
* PowerShell iş akışı runbook'larını kullanmaya başlamak için bkz. [İlk PowerShell iş akışı runbook uygulamam](automation-first-runbook-textual.md)

<!--HONumber=Sep16_HO4-->


