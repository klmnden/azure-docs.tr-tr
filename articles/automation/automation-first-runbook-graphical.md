---
title: Azure automation'da ilk bir grafik runbook Uygulamam
description: Basit bir grafik runbook uygulaması oluşturma, test etme ve yayımlama adımlarını anlatan öğretici.
keywords: runbook, runbook şablonu, runbook otomasyonu, azure runbook
services: automation
ms.service: automation
ms.subservice: process-automation
author: georgewallace
ms.author: gwallace
ms.date: 04/13/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: be811d0dc2ce2eca0b20ca12165eaf0799bd6b5d
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61078097"
---
# <a name="my-first-graphical-runbook"></a>İlk grafik runbook uygulamam

> [!div class="op_single_selector"]
> * [Grafik](automation-first-runbook-graphical.md)
> * [PowerShell](automation-first-runbook-textual-powershell.md)
> * [PowerShell İş Akışı](automation-first-runbook-textual.md)
> * [Python](automation-first-runbook-textual-python2.md)
> 

Bu öğretici, Azure Automation’da bir [grafik runbook uygulaması](automation-runbook-types.md#graphical-runbooks) oluşturulmasını adım adım göstermektedir. Runbook işi durumunun nasıl izleneceğini öğrenirken test edip yayımlayan basit bir runbook ile başlarsınız. Ardından, bir Azure sanal makinesini başlatmayı içeren bir örnekle, bu runbook’u gerçekten Azure kaynaklarını yönetmek üzere değiştirin. Daha sonra, runbook parametreleri ve koşullu bağlantılar ekleme yoluyla runbook’u daha sağlam hale getirerek öğreticiyi tamamlayın.

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi tamamlamak için aşağıdakiler gerekir:

* Azure aboneliği. Henüz bir aboneliğiniz yoksa [MSDN abone avantajlarınızı etkinleştirebilir](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) ya da [ücretsiz hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) için kaydolabilirsiniz.
* Runbook’u tutacak ve Azure kaynaklarında kimlik doğrulamasını yapacak bir [Automation hesabı](automation-offering-get-started.md). Bu hesabın sanal makineyi başlatma ve durdurma izni olmalıdır.
* Azure sanal makinesi. Bu makineyi durdurup başlatacağınız için makinenin üretime yönelik bir VM olmaması gerekir.

## <a name="create-runbook"></a>Runbook oluşturma

Çıktı olarak *Merhaba Dünya* metnini veren basit bir runbook oluşturun.

1. Azure portalında, Otomasyon hesabınızı açın.

   Automation hesabı sayfası size bu hesaptaki kaynakların hızlı bir görünümünü sağlar. Birkaç Varlığınız zaten olmalıdır. Bu varlıkların çoğu, yeni bir Otomasyon hesabına otomatik olarak dahil edilen modüllerdir. Burada ayrıca [önkoşullarda](#prerequisites) belirtilen Kimlik Bilgileri varlığınız da bulunmalıdır.

2. Runbook’ların listesini açmak için **İŞLEM YÖNETİMİ** altında **Runbooklar**’ı seçin.
3. Seçerek yeni bir runbook oluşturmak **+ runbook Ekle**, ardından **yeni bir runbook oluşturmak**.
4. Runbook’a *MyFirstRunbook-Graphical* adını verin.
5. Bu örneğimizde bir [grafik runbook uygulaması](automation-graphical-authoring-intro.md) oluşturacaksınız, bu nedenle **Runbook türü** olarak **Grafik**’i seçin.<br> ![Yeni runbook](media/automation-first-runbook-graphical/create-new-runbook.png)<br>
6. Runbook’u oluşturmak için **Oluştur**’a tıklayın ve grafik düzenleyicisini açın.

## <a name="add-activities"></a>Etkinlik ekleme

Düzenleyicinin sol tarafındaki Kitaplık denetimi runbook uygulamanıza eklenecek etkinlikleri seçmenizi sağlar. Runbook uygulamasından çıktı metnine **Write-Output** cmdlet’ini ekleyin.

1. Kitaplık denetiminde, arama metin kutusuna tıklayın ve **Write-Output** yazın. Aşağıdaki görüntüde arama sonuçları gösterilir: <br> ![Microsoft.PowerShell.Utility](media/automation-first-runbook-graphical/search-powershell-cmdlet-writeoutput.png)
1. Listenin aşağısına kaydırın. **Write-Output**’a sağ tıklayıp **Tuvale ekle**’yi seçebilir ya da cmdlet’in yanındaki üç noktaya tıklayıp ardından **Tuvale ekle**’yi seçebilirsiniz.
1. Tuvalde **Write-Output** etkinliğine tıklayın. Bu eylem, etkinliği yapılandırmanızı sağlayan yapılandırma denetimi sayfası açılır.
1. **Etiket** cmdlet’in adını varsayılan olarak alır ancak bunu daha kolay bir şeyle değiştirebilirsiniz. Bunu *çıkışa Hello World yazmak* üzere değiştirin.
1. Cmdlet parametreleri değerlerini sağlamak için **Parametreler**’e tıklayın. 

   Bazı cmdlet’ler birden fazla parametre kümesine sahiptir ve kullanacağınız cmdlet’i seçmeniz gerekir. Bu durumda, **Write-Output** yalnızca bir parametre kümesine sahip olur, bu nedenle seçmeniz gerekmez.

1. **InputObject** parametresini seçin. Bu, çıkış akışına göndermek üzere metin belirttiğiniz parametredir.
1. **Veri kaynağı** açılır listesinde, **PowerShell ifadesi**’nı seçin. **Veri kaynağı** açılır penceresi bir parametre değerini doldurmak için kullandığınız farklı kaynaklar sağlar.

   Başka bir etkinlik, Automation varlığı ya da PowerShell ifadesi gibi böyle kaynaklardan alınan çıktıları kullanabilirsiniz. Bu durumda, çıkış yalnızca *Merhaba Dünya*’dır. Bir PowerShell ifadesi kullanabilir ve dize belirtebilirsiniz.<br>

1. **İfade** kutusuna *"Hello World"* yazın ve ardından tuvale döndürmek için iki kez **Tamam**’a tıklayın.
1. **Kaydet**’e tıklayarak runbook’u kaydedin.

## <a name="test-the-runbook"></a>Runbook'u test etme

Runbook’u üretimde kullanılabilir hale getirmek üzere yayımlamadan önce düzgün çalıştığından emin olmak için test etmeniz gerekir. Bir runbook'u test ettiğinizde, bunun **Taslak** sürümünü çalıştırır ve çıktısını etkileşimli olarak görüntülersiniz.

1. Seçin **Test bölmesi** Test sayfasını açın.
1. Testi başlatmak için **Başlat**’a tıklayın. Etkinleştirilen tek seçenek bu olmalıdır.
1. Bölmede bir [runbook işi](automation-runbook-execution.md) oluşturulur ve durumu görüntülenir.

   İş durumu, bulutta bir runbook çalışanının kullanılabilir hale gelmesinin beklendiğini gösteren şekilde *Sırada* olarak başlar. Ardından, bir çalışan işi talep ettiğinde *Başlatılıyor* olarak ve runbook gerçekten çalışmaya başladığında *Çalışıyor* olarak değiştirilir.

1. Runbook işi tamamlandığında çıktısı görüntülenir. Bu durumda, *Merhaba Dünya* ifadesini görürsünüz.<br> ![Hello World](media/automation-first-runbook-graphical/runbook-test-results.png)
1. Tuvale geri dönmek için Test sayfayı kapatın.

## <a name="publish-and-start-the-runbook"></a>Yayımlama ve runbook'u Başlat

Oluşturduğunuz runbook hala Taslak modundadır. Üretimde çalıştırılabilmesi için yayımlanması gerekir. Bir runbook yayımladığınızda, Taslak sürümü mevcut Yayımlanmış sürümün üzerine yazarsınız. Örneğimizde, runbook’u henüz oluşturduğunuzdan, Yayımlanmış sürümünüz yok.

1. Seçin **Yayımla** runbook'u yayımlayamadı ve ardından **Evet** istendiğinde.
1. Runbook'ta görüntülemek için sola kaydırırsanız **runbook'ları** sayfasında, bu gösterir bir **yazma durumu** , **yayımlanan**.
1. İçin sayfayı görüntülemek üzere geri sağa kaydırın **MyFirstRunbook-Graphical**.

   Üst kısımdaki seçenekler runbook’u başlatmamıza, gelecekte bir zamanda başlatmak üzere zamanlamamıza ya da bir HTTP çağrısıyla başlatılabilmesi için [web kancası](automation-webhooks.md) oluşturmamıza olanak tanır.

1. Seçin **Başlat** ardından **Evet** runbook'u başlatmak için istendiğinde.
1. Oluşturulan runbook işi için iş sayfası açılır. **İş durumu**’nun **Tamamlandı** olduğunu doğrulayın.
1. Runbook durumu olarak *Tamamlandı* gösterilince **Çıktı**’ya tıklayın. **Çıkış** sayfası açılır ve gördüğünüz *Hello World* bölmesinde.
1. Çıkış sayfasını kapatın.
1. Tıklayın **tüm günlükler** runbook işi için akışlar sayfasını açın. Çıktı akışında yalnızca *Merhaba Dünya* metnini görmelisiniz, ancak bu bölmede, runbook bunlara yazıyorsa Ayrıntılı ve Hata gibi runbook işine yönelik diğer akışlar da gösterilebilir.
1. Tüm günlükleri sayfasında ve MyFirstRunbook-Graphical sayfasına geri dönmek için iş sayfasına kapatın.
1. Tüm runbook için iş kapatmak görüntülemek için **iş** sayfasından seçim yapıp **işleri** altında **kaynakları**. Bu bölmede, bu runbook tarafından oluşturulan tüm işler listelenir. İşi yalnızca bir kez çalıştırdığınız için sadece bir işin listelendiğini görmeniz gerekir.
1. Runbook’u başlattığınızda, görüntülediğiniz iş bölmesini açmak için bu işe tıklayabilirsiniz. Böylece zaman içinde geri dönerek, belirli bir runbook için oluşturulan herhangi bir işin ayrıntılarını görüntüleyebilirsiniz.

## <a name="create-variable-assets"></a>Değişken varlıkları oluşturma

Runbook uygulamanızı test ettiniz ve yayımladınız, ancak şu ana kadar faydalı bir şey yapmadı. Bu runbook’un Azure kaynaklarını yönetmesini istiyorsunuz. Kimlik doğrulaması için runbook uygulamanızı yapılandırmadan önce, aşağıda 6. adımda kimlik doğrulamak üzere abonelik kimliğini tutmak için bir değişken oluşturun ve etkinliği ayarladıktan sonra buna başvurun. Abonelik bağlamına başvuru eklemek birden fazla abonelik arasından kolayca çalışmanızı sağlar. Devam etmeden önce, Gezinti bölmesindeki Abonelik seçeneği kapalı’daki abonelik kimliğinizi kopyalayın.

1. Otomasyon hesapları sayfasında seçin **değişkenleri** altında **paylaşılan kaynakları**.
1. Seçin **değişken Ekle**.
1. Yeni değişken sayfasında içinde **adı** kutusuna **Azuresubscriptionıd** ve **değer** kutusuna abonelik kimliğinizi girin **Tür** için *dizeyi* **Şifreleme** için değeri koruyun.
1. Değişkeni oluşturmak için **Oluştur**’a tıklayın.

## <a name="add-authentication"></a>Kimlik doğrulaması ekleme

Abonelik kimliğinizi tutmak üzere bir değişkene sahip olduğunuza göre, runbook uygulamanızı [ön koşullarda](#prerequisites) başvurulan Farklı Çalıştır kimlik bilgileri ile kimlik doğrulamak üzere yapılandırabilirsiniz. Azure farklı çalıştır bağlantısı ekleyerek bunu **varlık** ve **Connect-AzureRmAccount** tuvaline cmdlet'i.

1. Geri, runbook Seç gidip **Düzenle** MyFirstRunbook-Graphical sayfasında.
1. İhtiyacınız olmayan **çıkışa Hello World** artık, bu nedenle üç nokta (...) tıklayıp **Sil**.
1. Kitaplık denetiminde, **VARLIKLAR**, **Bağlantılar**’ı genişletin ve **Tuvale Ekle**’yi seçerek **AzureRunAsConnection**’ı ekleyin.
1. Kitaplık denetiminde, yazın **Connect-AzureRmAccount** arama metin kutusuna.

   > [!IMPORTANT]
   > **Add-AzureRmAccount** için bir diğer ad sunulmuştur **Connect-AzureRMAccount**. Ne zaman kitaplığınızı arama öğe görmüyorsanız, **Connect-AzureRMAccount**, kullanabileceğiniz **Add-AzureRmAccount**, veya Otomasyon hesabınızda modüllerinizi güncelleştirebilirsiniz.

1. Ekleme **Connect-AzureRmAccount** tuvaline.
1. Şeklin altında bir daire görünene kadar **Farklı Çalıştır Bağlantısını Al** üzerinde bekleyin. Daireye tıklayın ve oku sürükleyin **Connect-AzureRmAccount**. Oluşturduğunuz ok bir *bağlantıdır*. Runbook ile başlayan **Çalıştır bağlantısını Al** ve ardından çalıştırın **Connect-AzureRmAccount**.<br> ![Etkinlikler arasında bağlantı oluşturma](media/automation-first-runbook-graphical/runbook-link-auth-activities.png)
1. Tuvalinde **Connect-AzureRmAccount** ve yapılandırma denetim bölmesinde türü **Azure'da oturum** içinde **etiket** metin.
1. Tıklayın **parametreleri** ve etkinlik parametresi yapılandırma sayfası görüntülenir.
1. **Connect-AzureRmAccount** birden fazla parametre kümesine sahiptir, bu nedenle parametre değerleri sağlamadan önce birini seçmeniz gerekir. **Parametre Kümesi**’ne tıklayın ve ardından **ServicePrincipalCertificate** parametre kümesine tıklayın.
1. Parametre kümesini seçtiğinizde, parametreleri Etkinlik parametresi Yapılandırması sayfasında görüntülenir. **APPLICATIONID**’ye tıklayın.<br> ![Azure RM hesabı parametreleri ekleme](media/automation-first-runbook-graphical/Add-AzureRmAccount-params.png)
1. Parametre değeri sayfasında seçin **etkinlik çıkışı** için **veri kaynağı** seçip **Çalıştır bağlantısını Al** listesinden de **alanyolu** metin kutusuna **ApplicationId**ve ardından **Tamam**. Etkinlik birden fazla özelliğe sahip bir nesne çıkardığından, Alan yolu için özellik adını belirtiyorsunuz.
1. Tıklayın **CERTIFICATETHUMBPRINT**, parametre değeri sayfanın seçip **etkinlik çıkışı** için **veri kaynağı**. Listede **Farklı Çalıştır Bağlantısını Al**’ı seçin **Alan yolu** metin kutusuna **CertificateThumbprint** yazın ve ardından **Tamam**’a tıklayın.
1. Tıklayın **SERVICEPRINCIPAL**, parametre değeri sayfanın seçip **ConstantValue** için **veri kaynağı**, seçeneğini **True**, ve ardından **Tamam**.
1. Tıklayın **TENANTID**, parametre değeri sayfanın seçip **etkinlik çıkışı** için **veri kaynağı**. Listede **Farklı Çalıştır Bağlantısını Al**’ı seçin **Alan yolu** metin kutusuna **TenantId** yazın ve ardından iki kez **Tamam**’a tıklayın.
1. Kitaplık denetiminde, arama metin kutusuna **Set-AzureRmContext** yazın.
1. Tuvale **Set-AzureRmContext** ekleme
1. Tuvalde, **Set-AzureRmContext**’i seçin ve Yapılandırma denetim bölmesinde **Etiket** metin kutusuna **Abonelik Kimliği Belirt** yazın.
1. Tıklayın **parametreleri** ve etkinlik parametresi yapılandırma sayfası görüntülenir.
1. **Set-AzureRmContext** birden fazla parametre kümesine sahiptir, bu nedenle parametre değerleri sağlamadan önce birini seçmelisiniz. **Parametre Kümesi**’ne tıklayın ve ardından **SubscriptionId** parametre kümesini seçin.
1. Parametre kümesini seçtiğinizde, parametreleri Etkinlik parametresi Yapılandırması sayfasında görüntülenir. **SubscriptionID**’e tıklayın.
1. Parametre değeri sayfasında seçin **değişken varlığı** için **veri kaynağı** seçip **Azuresubscriptionıd** listesini ve ardından **Tamam** iki kez.
1. Şeklin altında bir daire görünene kadar **Azure’da Oturum Aç** üzerinde bekleyin. Daireye tıklayın ve oku **Abonelik Kimliği Belirt**’e sürükleyin.

Runbook'unuzda bu noktada aşağıdakine benzer: <br>![Runbook kimlik doğrulama yapılandırması](media/automation-first-runbook-graphical/runbook-auth-config.png)

## <a name="add-activity-to-start-a-vm"></a>Bir VM başlatmak üzere etkinlik ekleme

Burada bir sanal makineyi başlatmak için **Start-AzureRmVM** etkinliği ekleyin. Azure aboneliğinizdeki herhangi bir sanal makineyi seçebilirsiniz, şimdilik bu adı cmdlet’e kod olarak ekleyin.

1. Kitaplık denetiminde, arama metin kutusuna **Start-AzureRm** yazın.
2. Tuvale **Start-AzureRmVM** ekleyin ve ardından **Abonelik Kimliği Belirt** altına tıklayarak sürükleyin.
1. Şeklin altında bir daire görünene kadar **Abonelik Kimliği Belirt** üzerinde bekleyin. Daireye tıklayın ve oku **Start-AzureRmVM**’ye sürükleyin.
1. **Start-AzureRmVM**’yi seçin. **Start-AzureRmVM** için kümeleri görüntülemek üzere **Parametreler**’i ve ardından **Parametre kümesi**’ni seçin. **ResourceGroupNameParameterSetName** parametre kümesini seçin. **ResourceGroupName** ve **Ad**’ın yanında ünlem işareti vardır. Bu, bunların gerekli parametreler olduğunu gösterir. Ayrıca, her ikisinin de dize değerleri beklediğini unutmayın.
1. **Ad**’ı seçin. **Veri Kaynağı** için **PowerShell ifadesi**’ni seçin ve çift tırnakların arasına, bu runbook uygulamasını başlatacağınız sanal makine adını yazın. **Tamam** düğmesine tıklayın.
1. **ResourceGroupName**’i seçin. **Veri Kaynağı** için **PowerShell ifadesi**’ni seçin ve çift tırnakların arasına kaynak grubu adını yazın. **Tamam** düğmesine tıklayın.
1. Runbook’u test edebilmek için Test bölmesine tıklayın.
1. Testi başlatmak için **Başlat**’a tıklayın. Tamamlandığında, sanal makinenin başlatıldığını kontrol edin.

Runbook'unuzda bu noktada aşağıdakine benzer: <br>![Runbook kimlik doğrulama yapılandırması](media/automation-first-runbook-graphical/runbook-startvm.png)

## <a name="add-additional-input-parameters"></a>Ek giriş parametreleri ekleme

Runbook’umuz şu anda **Start-AzureRmVM** cmdlet’inde belirttiğiniz kaynak grubunda sanal makineyi başlatır. Runbook, runbook başlatıldığında her ikisini de belirtmemiz durumunda daha kullanışlı olur. Şimdi bu işlevi sağlamak için runbook’a girdi parametreleri ekleyebilirsiniz.

1. Tıklayarak grafik düzenleyicisini açın **Düzenle** üzerinde **MyFirstRunbook-Graphical** bölmesi.
1. Seçin **giriş ve çıkış** ardından **Girişi Ekle** Runbook giriş parametresi bölmesini açmak için.
1. **Ad** için *VMName* belirtin. **Tür** *dizesini* koruyun ancak, **Zorunlu**’yu *Evet* olarak değiştirin. **Tamam** düğmesine tıklayın.
1. *ResourceGroupName* adlı ikinci bir zorunlu giriş parametresi oluşturun ve ardından **Giriş ve Çıkış** bölmesini kapatmak için **Tamam**’a tıklayın.<br> ![Runbook Giriş Parametreleri](media/automation-first-runbook-graphical/start-azurermvm-params-outputs.png)
1. **Start-AzureRmVM** etkinliğini seçin ve ardından **Parametreler**’e tıklayın.
1. **Ad** için **Veri kaynağı**’nı, **Runbook girişi** olarak değiştirin ve ardından **VMName**’i seçin.
1. **ResourceGroupName** için **Veri kaynağı**’nı, **Runbook girişi** olarak değiştirin ve ardından **ResourceGroupName**’i seçin.<br> ![Start-AzureVM Parametreleri](media/automation-first-runbook-graphical/start-azurermvm-params-runbookinput.png)
1. Runbook'u kaydedin ve Test bölmesini açın. Şimdi testte kullanılacak olan iki girdi değişkeni için değerleri sağlayabilirsiniz.
1. Test bölmesini kapatın.
1. Runbook’un yeni sürümünü yayımlamak için **Yayımla**’ya tıklayın.
1. Önceki adımda başlattığınız sanal makineyi durdurun.
1. Runbook'u başlatmak için **Başlat**’a tıklayın. Başlatacağınız sanal makinenin **VMName** ve **ResourceGroupName** bilgilerini yazın.
1. Runbook tamamlandığında, sanal makinenin başladığından emin olun.

## <a name="create-a-conditional-link"></a>Koşullu bağlantı oluşturma

Hala başlatılmamışsa, yalnızca sanal makineyi başlatmayı deneyecek şekilde runbook’u değiştirin. Bunu, runbook’a sanal makinenin örnek düzeyi durumunu alan **Get-AzureRmVM** cmdlet’ini ekleyerek yapabilirsiniz. Ardından, sanal makine durumunun çalışıyor veya durduruldu olduğunu belirlemek amacıyla PowerShell kodu kod parçacığıyla birlikte **Durumu Al** adlı PowerShell İş Akışı kodu modülü ekleyin. **Durumu Al** modülünden alınan bir koşullu bağlantı yalnızca, geçerli çalışma durumu durduruldu ise, **Start-AzureRmVM** cmdlet’ini çalıştırır. Son olarak, sanal makinin başarıyla başlatılıp başlatılmadığını veya PowerShell Write-Output cmdlet’ini kullanmadığını size bildirmek üzere bir çıktı mesajı gönderilir.

1. Açık **MyFirstRunbook-Graphical** grafik düzenleyicisinde.
1. Üzerine tıklayarak ve ardından *Sil* tuşuna basarak **Specify Subscription Id** ve **Start-AzureRmVM** arasındaki bağlantıyı kaldırın.
1. Kitaplık denetiminde, arama metin kutusuna **Get-AzureRm** yazın.
1. Tuvale **Get-AzureRmVM** ekleyin.
1. **Get-AzureRmVM** için kümeleri görüntülemek üzere **Get-AzureRmVM**’yi ve ardından **Parametre kümesi**’ni seçin. **GetVirtualMachineInResourceGroupNameParamSet** parametre kümesini seçin. **ResourceGroupName** ve **Ad**’ın yanında ünlem işareti vardır. Bu, bunların gerekli parametreler olduğunu gösterir. Ayrıca, her ikisinin de dize değerleri beklediğini unutmayın.
1. **Ad** için **Veri kaynağı** altında, **Runbook girişi**’ni ve ardından **VMName**’i seçin. **Tamam**’a tıklayın.
1. **ResourceGroupName** için **Veri kaynağı** altında, **Runbook girişi**’ni ve ardından **ResourceGroupName**’i seçin. **Tamam** düğmesine tıklayın.
1. **Durum** için **Veri kaynağı** altında, **Sabit değer**’i seçin ve ardından **True** öğesine tıklayın. **Tamam** düğmesine tıklayın.
1. **Abonelik Kimliği Belirt**’ten **Get-AzureRmVM**’ye bir bağlantı oluşturun.
1. Kitaplık denetiminde, **Runbook Denetimi**’ni genişletin ve tuvale **Kod** ekleyin.  
1. **Get-AzureRmVM**’den **Kod**’a bir bağlantı oluşturun.  
1. **Kod**’a tıklayın ve Yapılandırma bölmesinde etiketi **Durumu Al** olarak değiştirin.
1. Seçin **kod** parametresi ve **Kod Düzenleyicisi** sayfası görüntülenir.  
1. Kod düzenleyicisine, aşağıdaki kod parçacığını yapıştırın:

    ```powershell-interactive
     $StatusesJson = $ActivityOutput['Get-AzureRmVM'].StatusesText
     $Statuses = ConvertFrom-Json $StatusesJson
     $StatusOut =""
     foreach ($Status in $Statuses){
     if($Status.Code -eq "Powerstate/running"){$StatusOut = "running"}
     elseif ($Status.Code -eq "Powerstate/deallocated") {$StatusOut = "stopped"}
     }
     $StatusOut
     ```

1. **Durumu Al**’dan **Start-AzureRmVM**’ye bir bağlantı oluşturun.<br> ![Kod Modülü ile Runbook](media/automation-first-runbook-graphical/runbook-startvm-get-status.png)  
1. Bağlantıyı seçin ve Yapılandırma bölmesinde, **Koşul uygula**’yı **Evet** olarak değiştirin. Bağlantının, hedef etkinliğin yalnızca koşulun true olarak çözümlemesi halinde çalıştırılacağını belirten kesikli çizgiye döndüğüne dikkat edin.  
1. **Koşul ifadesi** için *$ActivityOutput['Get Status'] -eq "Stopped"* yazın. **Start-AzureRmVM** artık yalnızca sanal makine durursa çalışır.
1. Kitaplık denetiminde, **Cmdlet'leri** ve ardından **Microsoft.PowerShell.Utility**’yi genişletin.
1. Tuvale iki kez **Write-Output** ekleyin.
1. İlk **Write-Output** denetiminde, **Parametreler**’e tıklayın ve **Etiket** değerini *VM Başlatıldığında Bildir* olarak değiştirin.
1. **InputObject** için, **Veri kaynağını** **PowerShell ifadesi** olarak değiştirin ve ifadeye *"$VMName successfully started."* yazın.
1. Birinci **Write-Output** denetiminde, **Parametreler**’e tıklayın ve **Etiket** değerini *VM Başlatılamadığında Bildir* olarak değiştirin
1. **InputObject** için, **Veri kaynağını** **PowerShell ifadesi** olarak değiştirin ve ifadeye *"$VMName could not start."* yazın.
1. **Start-AzureRmVM**’den **VM Başlatıldığında Bildir** ve **VM Başlatma Başarısız Olduğunda Bildir**’e bir bağlantı oluşturun.
1. **VM Başlatıldığında Bildir** bağlantısını seçin ve **Koşul uygula**’yı **True** olarak değiştirin.
1. **Koşul ifadesi** için, *$ActivityOutput['Start-AzureRmVM'].IsSuccessStatusCode -eq $true* yazın. Write-Output denetimi artık yalnızca sanal makine başarıyla başlatıldığında çalışır.
1. **VM Başlatma Başarısız Olduğunda Bildir** bağlantısını seçin ve **Koşul uygula**’yı **True** olarak değiştirin.
1. **Koşul ifadesi** için, *$ActivityOutput['Start-AzureRmVM'].IsSuccessStatusCode -ne $true* yazın. Write-Output denetimi artık yalnızca sanal makine başarıyla başlatılmadığında çalışır. Runbook'unuzda, aşağıdaki resimdeki gibi görünmelidir: <br> ![Write-Output ile Runbook](media/automation-first-runbook-graphical/runbook-startazurermvm-complete.png)
1. Runbook'u kaydedin ve Test bölmesini açın.
1. Sanal makine kapalı iken runbook’u çalıştırın, başlamalıdır.

## <a name="next-steps"></a>Sonraki adımlar

* Grafik Yazma hakkında daha fazla bilgi için bkz. [Azure Automation’da grafik yazma](automation-graphical-authoring-intro.md)
* PowerShell runbook'ları kullanmaya başlamak için bkz. [İlk PowerShell runbook uygulamam](automation-first-runbook-textual-powershell.md)
* PowerShell iş akışı runbook'larını kullanmaya başlamak için bkz. [İlk PowerShell iş akışı runbook uygulamam](automation-first-runbook-textual.md)


