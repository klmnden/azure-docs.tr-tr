---
title: Grafik Azure Otomasyonu'nda yazma
description: "Grafik yazma kodu ile çalışma olmadan için Azure Automation runbook'ları oluşturmanızı sağlar. Bu makale, grafik yazma giriş bilgileri ve grafik runbook oluşturmaya başlamak için gerekli tüm ayrıntılar sağlar."
services: automation
ms.service: automation
author: georgewallace
ms.author: gwallace
ms.date: 03/16/2018
ms.topic: article
manager: carmonm
ms.devlang: na
ms.tgt_pltfrm: na
ms.openlocfilehash: 5bdb92378a72789918f40494bebb5faf68e64c84
ms.sourcegitcommit: a36a1ae91968de3fd68ff2f0c1697effbb210ba8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/17/2018
---
# <a name="graphical-authoring-in-azure-automation"></a>Grafik Azure Otomasyonu'nda yazma
## <a name="introduction"></a>Giriş
Grafik yazma temeldeki Windows PowerShell veya PowerShell iş akışı kodu karmaşıklık için Azure Automation runbook'ları oluşturmanızı sağlar. Etkinlikleri tuvale cmdlet'ler ve runbook'ları kitaplıktan ekleyin, birbirine bağlamak ve bir iş akışı oluşturmak üzere yapılandırın. System Center Orchestrator veya Service Management Automation (SMA) ile herhangi bir zamanda çalıştıysanız, ardından bu size tanıdık gelecektir

Bu makalede, grafik yazma giriş bilgileri ve grafik runbook oluşturmaya başlamak için gereken kavramlar sağlar.

## <a name="graphical-runbooks"></a>Grafik runbook'lar
Azure Otomasyonu'nda tüm runbook'lar Windows PowerShell iş akışlarıdır. Otomasyon çalışanları tarafından çalıştırılan PowerShell kodu grafik ve grafik PowerShell iş akışı runbook'ları oluşturmak, ancak görüntüleme veya doğrudan değiştirmek mümkün değildir. Bir grafik runbook bir grafik PowerShell iş akışı runbook'u ve tam tersini dönüştürülebilir, ancak bir metinsel runbook'a dönüştürülemiyor. Var olan bir metinsel runbook'un grafik Düzenleyicisi aktarılamaz.

## <a name="overview-of-graphical-editor"></a>Grafik Düzenleyicisi'ne genel bakış
Oluşturma veya grafik runbook düzenleme, grafik düzenleyicisini Azure portalında açabilirsiniz.

![Grafik çalışma](media/automation-graphical-authoring-intro/runbook-graphical-editor.png)

Aşağıdaki bölümlerde grafik düzenleyicisini denetimlerinde açıklanmaktadır.

### <a name="canvas"></a>Tuvale
Tuvale runbook'unuz tasarım burada ' dir. Kitaplık denetiminde düğümlerinden runbook'a etkinlikler ekleme ve bunları runbook mantığını tanımlamak için bağlantılar ile bağlayın.

Yakınlaştırma ve uzaklaştırma için tuvalin altındaki denetimleri kullanabilirsiniz.

### <a name="library-control"></a>Kitaplığı denetimi
Burada seçtiğiniz kitaplık denetimidir [etkinlikleri](#activities) runbook'a eklemek için. Bunları burada, bunları etkinliği başka etkinliklere bağlayın tuvale ekleyin. Aşağıdaki tabloda açıklanan dört bölüm içerir:

| Section | Açıklama |
|:--- |:--- |
| Cmdlet'leri |Kullanılabilir tüm cmdlet'leri runbook'unuza içerir. Cmdlet modülü tarafından düzenlenir. Tüm otomasyon hesabınızda yüklü modülleri kullanılabilir. |
| Runbook'lar |Runbook'ları Otomasyon hesabınızda içerir. Bu runbook'lar alt runbook'lar olarak kullanılacak tuvale eklenebilir. Yalnızca düzenlenmekte olan runbook aynı çekirdek türdeki runbook'lar gösterilir; Grafik PowerShell iş akışı runbook'ları yalnızca PowerShell iş akışı tabanlı runbook'ları gösterilmese için grafik runbook'ları yalnızca PowerShell tabanlı runbook'lar gösterilir. |
| Varlıklar |İçeren [Otomasyon varlıkları](http://msdn.microsoft.com/library/dn939988.aspx) Otomasyon hesabınızda runbook'unuzda kullanılabilir. Bir varlığı bir runbook'a eklediğinizde, seçili varlığı alır bir iş akışı etkinliği ekler. Değişken varlıkları söz konusu olduğunda, değişkeni almak veya değişkeni ayarlamak için bir etkinlik eklenip eklenmeyeceğini seçebilirsiniz. |
| Runbook denetimi |Kullanılabilir runbook denetimi etkinlikleri geçerli runbook'unuzda içerir. A *birleşim* birden çok girişi alır ve tüm iş akışı devam etmeden önce tamamlanana kadar bekler. A *kod* etkinlik bir veya daha fazla kod satırı bulunmaktadır PowerShell veya PowerShell iş akışı grafik runbook türüne bağlı olarak çalışır. Bu etkinlik için özel kod veya diğer etkinliklerle elde etmek zordur işlevselliği için kullanabilirsiniz. |

### <a name="configuration-control"></a>Yapılandırma denetimi
Yapılandırma ayrıntıları tuvalde seçili bir nesne için verdiğiniz denetimdir. Bu denetimde kullanılabilen özellikleri seçilen nesne türüne bağlıdır. Yapılandırma denetiminde bir seçeneği belirlediğinizde, ek bilgi sağlamak için ek dikey pencere açılır.

### <a name="test-control"></a>Test denetimi
Grafik Düzenleyicisi'ni ilk başlattığınızda Test denetimi görüntülenmez. Ne zaman açıldığında, etkileşimli olarak [bir grafik runbook'u test](#graphical-runbook-procedures). 

## <a name="graphical-runbook-procedures"></a>Grafik runbook yordamları
### <a name="exporting-and-importing-a-graphical-runbook"></a>Bir grafik runbook alma ve verme
Yalnızca bir grafik runbook yayımlanan sürümüne dışarı aktarabilirsiniz. Runbook henüz yayımlanmadı, sonra **verme** düğmesi devre dışıdır. Tıkladığınızda **verme** düğmesi, runbook'u yerel bilgisayara indirilir. Dosya adı ile runbook adıyla bir *graphrunbook* uzantısı.

Seçerek bir grafik veya grafik PowerShell iş akışı runbook dosyasını da içeri aktarabilirsiniz **alma** bir runbook eklerken seçeneği. Alınacak dosyayı seçtiğinizde, aynı tutabilirsiniz **adı** veya yeni bir tane girin. Runbook türü alan runbook türü seçilen dosyanın değerlendirir ve doğru değil farklı bir tür seçin çalışırsanız, bir ileti olası çakışmaları vardır ve dönüştürme sırasında olabilir sözdizimi sunulur sonra görüntüler hatalar. 

![Runbook'u içeri aktar](media/automation-graphical-authoring-intro/runbook-import-revised20165.png)

### <a name="testing-a-graphical-runbook"></a>Grafik runbook'u test etme
Runbook'un yayımlanan sürümünde değişiklik yapmadan ya da onu yayımlamadan önce yeni bir runbook test edebilirsiniz Azure portalında bir runbook'un taslak sürümünü test edebilirsiniz. Bu, yayımlanan sürümü değiştirmeden önce runbook'un doğru çalıştığını doğrulamanızı sağlar. Bir runbook'u test ettiğinizde taslak runbook yürütülür ve gerçekleştirdiği tüm işlemler tamamlanır. İş Geçmişi oluşturulmaz, ancak çıktı Test çıkışı bölmesinde görüntülenir. 

Bir runbook için Test denetimini runbook düzenleme için açarak açın ve tıklayın **Test bölmesi** düğmesi.

Herhangi bir giriş parametreleri ve tıklayarak runbook'u başlatmak için Test denetimi ister **Başlat** düğmesi.

### <a name="publishing-a-graphical-runbook"></a>Grafik runbook yayımlama
Azure Otomasyonu içindeki her runbook'un bir taslak ve bir yayımlanmış sürümü vardır. Yalnızca yayımlanan sürüm çalıştırılabilir ve yalnızca taslak sürüm düzenlenebilir. Yayımlanan sürüm taslak sürümdeki herhangi bir değişiklikten etkilenmez. Taslak sürümü kullanılabilir olması hazır olduğunda size, hangi yayımlanan sürüm taslak sürümle değiştirebilirsiniz yayımlayın.

Bir grafik runbook düzenleme ve ardından tıklayarak runbook'u açarak yayımlayabilirsiniz **Yayımla** düğmesi.

Bir runbook henüz yayımlanmadı zaman durumuna sahip **yeni**. Yayımlandığında, durumuna sahip **yayımlanan**. Runbook yayımlandıktan sonra taslak ve yayımlanan sürümleri farklı düzenlerseniz, runbook durumuna sahip **düzenleme**.

![Runbook durumları](media/automation-graphical-authoring-intro/runbook-statuses-revised20165.png) 

Ayrıca, bir runbook yayımlanan sürümüne geri seçeneğiniz de vardır. Bu runbook son yayımlandığı ve runbook'un taslak sürümünü yayımlanmış sürümüyle değiştirir beri yapılan değişiklikler hemen oluşturur.

## <a name="activities"></a>Etkinlikler
Etkinlikleri bir runbook'un yapı taşlarıdır. Bir etkinlik PowerShell cmdlet, bir alt runbook veya iş akışı etkinlik olabilir. Bir etkinliği runbook'a kitaplık denetiminde sağ tıklayıp seçerek eklemek **tuvale Ekle**. Ardından, tıklatın ve istediğiniz tuvalde herhangi bir yere koyun için etkinliğe sürükleyin. Tuvalde etkinlik konumunu runbook herhangi bir şekilde çalışmasını etkilemez. Ancak, bu işlemi görselleştirmek en uygun bulmak, runbook'u kullanıma düzenleyebilirsiniz. 

![Tuvale Ekle](media/automation-graphical-authoring-intro/add-to-canvas-revised20165.png)

Özelliklerini ve parametre yapılandırma dikey penceresinde yapılandırmak için tuval üzerinde etkinliği seçin. Değiştirebileceğiniz **etiket** için açıklayıcı bir etkinliğin. Özgün cmdlet'i hala çalıştırıldığı, grafik Düzenleyicisi'nde kullanılan görünen adını yalnızca değiştiriyorsunuz. Etiketi runbook içinde benzersiz olmalıdır. 

### <a name="parameter-sets"></a>Parametre kümeleri
Bir parametre kümesi belirli bir cmdlet için değerleri kabul edin zorunlu ve isteğe bağlı parametreleri tanımlar. Tüm cmdlet'ler, en az bir parametre kümesi ve bazı birden çok sahiptir. Bir cmdlet birden fazla parametre kümesine varsa, parametreleri yapılandırmadan önce kullanacağınız seçmelisiniz. Yapılandırabileceğiniz parametreleri, seçtiğiniz parametre kümesi bağlıdır. Seçerek bir etkinlik tarafından kullanılan parametre kümesi değiştirebileceğiniz **parametre** ve başka bir kümesini seçme. Bu durumda, yapılandırdığınız tüm parametre değerlerini kaybolur.

Aşağıdaki örnekte, Get-AzureRmVM cmdlet'i üç parametre kümesine sahiptir. Bir parametre kümeleri seçene kadar parametre değerlerini yapılandıramazsınız. The ListVirtualMachineInResourceGroupParamSet parameter set is for returning all virtual machines in a resource group and has a single optional parameter. The **GetVirtualMachineInResourceGroupParamSet** is for specifying the virtual machine you want to return and has two mandatory and one optional parameter.

![Parametre kümesi](media/automation-graphical-authoring-intro/get-azurermvm-parameter-sets.png)

#### <a name="parameter-values"></a>Parametre değerleri
Bir parametre için değer belirttiğinizde, değerin nasıl belirtilen belirlemek için bir veri kaynağı seçin. Bu parametre için geçerli değer belirli bir parametre bağlıdır için kullanılabilir olan veri kaynakları. Örneğin, Null, null değerlere izin vermiyor bir parametre için kullanılabilir bir seçenek değil.

| Veri Kaynağı | Açıklama |
|:--- |:--- |
| Sabit Değer |Parametresi için bir değer yazın. Bu sadece aşağıdaki veri türleri için kullanılabilir: Int32, Int64, dize, Boolean, DateTime, anahtarı. |
| Etkinlik çıkışı |İş akışında geçerli etkinlik önündeki bir etkinliğin çıkışı. Tüm geçerli etkinlikler listelenir. Yalnızca çıktısını için parametre değeri kullanmak için etkinliği seçin. Birden fazla özelliğe sahip bir nesne etkinlik çıkışı yapıyorsa etkinlik seçtikten sonra özellik adını yazabilirsiniz. |
| Runbook giriş |Etkinlik parametresi için giriş olarak bir runbook giriş parametresi seçin. |
| Değişken varlığı |Bir Otomasyon değişkeni giriş olarak seçin. |
| Kimlik bilgisi varlığı |Otomasyon kimlik bilgileri giriş olarak seçin. |
| Sertifika varlığı |Bir Otomasyon sertifikası giriş olarak seçin. |
| Bağlantı varlığı |Otomasyon bağlantısı giriş olarak seçin. |
| PowerShell ifadesi |Basit belirtin [PowerShell ifadesi](#powershell-expressions). İfade, etkinlik ve parametre değeri olarak kullanılan sonuç önce değerlendirilir. Bir etkinlik veya runbook giriş parametresi çıkışına başvurmak için değişkenleri kullanabilirsiniz. |
| Yapılandırılmadı |Önceden yapılandırılmış herhangi bir değer temizler. |

#### <a name="optional-additional-parameters"></a>İsteğe bağlı ek parametreler
Tüm cmdlet'ler ek parametreler sağlama seçeneğine sahip. Bunlar, PowerShell genel parametreleri veya diğer özel parametreler olan. PowerShell sözdizimini kullanarak parametreler burada sağlayabilir içeren bir metin kutusu sunulur. Örneğin, kullanılacak **ayrıntılı** ortak parametresi belirtirsiniz **"-Verbose: $True"**.

### <a name="retry-activity"></a>Etkinlik yeniden deneyin
**Yeniden deneme davranışını** belirli bir koşul yerine getirilene kadar birden çok kez çalıştırılması için bir etkinlik sağlayan çok benzer bir döngü şekilde. Birden çok kez çalıştırmanız, hata potansiyeli etkinlikler için bu özelliği kullanabilirsiniz ve ihtiyaç duyabileceğiniz birden fazla başarı için denemesi, veya geçerli veriler için etkinliğin çıktı bilgilerinin test. 

Bir etkinlik için retry etkinleştirdiğinizde, gecikme ve bir koşul ayarlayabilirsiniz. Gecikme süresi (saniyeler veya dakikalar içinde ölçülür) zamandır etkinlik yeniden çalıştırmadan önce runbook bekler. Ardından gecikme belirtilirse, hemen tamamlandıktan sonra etkinlik yeniden çalışır. 

![Etkinlik yeniden deneme gecikmesi](media/automation-graphical-authoring-intro/retry-delay.png)

Yeniden deneme etkinliği her çalıştığında sonra hesaplanan bir PowerShell ifadesi bir durumdur. İfadenin True olarak çözümlenirse, etkinliği yeniden çalıştırır. İfade yanlış olarak çözümlenirse, sonra etkinliği yeniden çalıştırmaz ve sonraki etkinliğe runbook geçer. 

![Etkinlik yeniden deneme gecikmesi](media/automation-graphical-authoring-intro/retry-condition.png)

Yeniden deneme koşulu etkinlik yeniden deneme işlemleri ile ilgili bilgilere erişim sağlayan $RetryData adında bir değişkeni kullanabilirsiniz. Bu değişken aşağıdaki tabloda özelliklere sahiptir:

| Özellik | Açıklama |
|:--- |:--- |
| NumberOfAttempts |Etkinliği çalıştırmak kez sayısı. |
| Çıktı |Son çalıştırma etkinliğin çıktı. |
| TotalDuration |Zaman aşımına etkinlik ilk kez başlatıldığından beri geçen. |
| StartedAt |Saati UTC biçiminde etkinliğin önce başlatıldı. |

Aşağıda verilmiştir etkinlik örnekleri koşulları yeniden deneyin.

    # Run the activity exactly 10 times.
    $RetryData.NumberOfAttempts -ge 10 

    # Run the activity repeatedly until it produces any output.
    $RetryData.Output.Count -ge 1 

    # Run the activity repeatedly until 2 minutes has elapsed. 
    $RetryData.TotalDuration.TotalMinutes -ge 2

Bir etkinlik için bir yeniden deneme koşulu yapılandırdıktan sonra etkinlik, anımsatmak için iki görsel ipuçları içerir. Etkinliğin sunulur ve etkinlik yapılandırmasını gözden geçirmek diğer durumdur.

![Etkinlik yeniden deneme görsel göstergeleri](media/automation-graphical-authoring-intro/runbook-activity-retry-visual-cue.png)

### <a name="workflow-script-control"></a>İş akışı komut dosyası denetimi
Kod PowerShell veya PowerShell iş akışı komut dosyası, aksi takdirde kullanılamayabilir işlevselliği sağlamak için yazılan grafik runbook türüne bağlı olarak kabul özel bir aktivite denetimdir. Parametreleri kabul edemez, ancak etkinlik çıkışı ve runbook giriş parametreleri için değişkenleri kullanabilirsiniz. Hiçbir bağlantı giden; bu durumda runbook çıkışına eklenmiş olan sürece etkinliğin herhangi bir çıktı veri yoluna eklenir.

Örneğin, aşağıdaki kodu tarih hesaplamaları $NumberOfDays adlı bir runbook giriş değişken kullanarak gerçekleştirir. Daha sonra runbook'taki izleyen etkinlikler tarafından kullanılacak çıkış olarak hesaplanan tarih saat gönderir.

    $DateTimeNow = (Get-Date).ToUniversalTime()
    $DateTimeStart = ($DateTimeNow).AddDays(-$NumberOfDays)}
    $DateTimeStart


## <a name="links-and-workflow"></a>Bağlantılar ve iş akışı
A **bağlantı** grafik bir runbook'ta iki etkinlik bağlanır. Tuvalde, kaynak etkinliğinden hedef etkinliğine işaret eden bir ok olarak görüntülenir. Etkinlikler oku yönde başlangıç kaynak etkinliği tamamlandıktan sonra hedef etkinlik ile çalıştırın. 

### <a name="create-a-link"></a>Bağlantı oluşturma
Kaynak etkinliği seçip şeklin altındaki daire tıklatarak iki etkinlik arasında bir bağlantı oluşturun. Hedef etkinlik ve yayın oku sürükleyin.

![Bağlantı oluşturma](media/automation-graphical-authoring-intro/create-link-revised20165.png)

Yapılandırma dikey penceresinde özelliklerini yapılandırmak için bağlantıyı seçin. Aşağıdaki tabloda açıklanan bağlantı türü içerir:

| Bağlantı türü | Açıklama |
|:--- |:--- |
| İşlem hattı |Hedef etkinlik, kaynak etkinliğinden her nesne çıktısı için bir kez çalıştırılır. Kaynak etkinliği hiçbir çıkış sonuçlanırsa, hedef etkinlik çalışmaz. Kaynak etkinliği çıktısını bir nesne olarak kullanılabilir. |
| Sequence |Hedef etkinlik yalnızca bir kez çalışır. Bunu kaynak etkinliğinden nesnelerinin bir dizisi alır. Kaynak etkinliği çıktısını nesnelerinin bir dizisi kullanılabilir. |

### <a name="starting-activity"></a>Başlangıç etkinliği
Bir grafik runbook gelen bağlantısına sahip olmayan tüm etkinlikleri ile başlatır. Bu runbook için başlangıç etkinlik olarak davranır yalnızca bir etkinlik görülür. Birden çok etkinliği bir gelen bağlantı yoksa, paralel olarak çalıştırarak runbook başlatır. Her tamamladıkça diğer etkinlikleri çalıştırmak için bağlantıları izler.

### <a name="conditions"></a>Koşullar
Bir bağlantı bir koşul belirttiğinizde, hedef etkinlik koşul çözümler ise yalnızca çalıştırılır true. Genellikle kaynak etkinliğinden çıkış almak için bir $ActivityOutput değişkeni koşul olarak kullanın. 

Ardışık Düzen bağlantısına için tek bir nesne için bir koşul belirtin ve koşul için her nesne çıkış kaynağı etkinlik tarafından değerlendirilir. Hedef etkinlik bir koşula uygun her bir nesne için daha sonra çalıştırın. Örneğin, Get-AzureRmVm bir kaynak etkinlikle aşağıdaki sözdizimini için koşullu ardışık düzen bağlantısına adlı kaynak grubunda yalnızca sanal makineleri almak için kullanılabilecek *Group1*. 

    $ActivityOutput['Get Azure VMs'].Name -match "Group1"

Tüm nesneleri çıktı ve kaynak etkinliğinden içeren tek bir dizi döndürdü beri dizisi bağlantı için koşulu yalnızca bir kez değerlendirilir. Bu nedenle, bir dizi bağlantı gibi bir ardışık düzen bağlantısına filtreleme için kullanılamaz, ancak yalnızca bir sonraki etkinlik çalıştırılır olup olmadığını belirler. Örneğin aşağıdaki etkinlikler kümesini bizim VM Başlat runbook'ta ele alın.<br> ![Dizileri ile koşullu bağlantı](media/automation-graphical-authoring-intro/runbook-conditional-links-sequence.png)<br>
Değerleri belirlemek için sanal makine adını ve kaynak grubu adı temsil eden iki runbook giriş parametreleri yapılacak - uygun eylemi olan sağlandı doğrulama üç farklı sıra bağlantı tek bir VM başlatmak için kaynak tüm sanal makineleri Başlat vardır Grup veya bir Abonelikteki tüm VM'ler. Azure Connect ve Get tek VM arasındaki dizisi bağlantı için koşul mantığı şöyledir:

    <# 
    Both VMName and ResourceGroupName runbook input parameters have values 
    #>
    (
    (($VMName -ne $null) -and ($VMName.Length -gt 0))
    ) -and (
    (($ResourceGroupName -ne $null) -and ($ResourceGroupName.Length -gt 0))
    )

Bir koşullu bağlantı kullandığınızda, o şubedeki diğer etkinlikler için kullanılabilir kaynak etkinliğinden veri koşul tarafından filtrelenir. Bir etkinlik birden çok bağlantı kaynağına ise, o şubedeki bağlanan bağlantı koşulunda etkinlikleri her dal için kullanılabilir veri bağlıdır.

Örneğin, **Start-AzureRmVm** runbook etkinliğinde tüm sanal makineleri başlatır. İki koşullu bağlantıları vardır. Deyim ilk koşullu bağlantı kullanan *$ActivityOutput ['Start-AzureRmVM']. IsSuccessStatusCode - eq $true* Start-AzureRmVm etkinliği başarıyla tamamlandı, filtre uygulamak için. İkinci ifade kullanan *$ActivityOutput ['Start-AzureRmVM']. IsSuccessStatusCode - ne $true* sanal makineyi başlatmak Start-AzureRmVm etkinlik başarısız olursa filtre uygulamak için. 

![Koşullu bağlantı örneği](media/automation-graphical-authoring-intro/runbook-conditional-links.png)

Herhangi bir etkinliği, ilk bağlantı ve Get-AzureVM etkinlik çıkışı Get-AzureVM çalıştırıldığı zaman başlatılan sanal makineler yalnızca get kullanır izler. İkinci bağlantıya izleyen herhangi bir etkinliği yalnızca Get-AzureVM çalıştırıldığı zaman durdurulan sanal makineleri alır. Üçüncü bağlantıyı izleyerek herhangi bir etkinlik çalışan durumlarına bakılmaksızın tüm sanal makineleri alır.

### <a name="junctions"></a>Kavşakları
Bir birleşim tüm gelen dalları tamamlanana kadar bekler özel bir etkinliktir. Bu, paralel olarak birden çok etkinliği çalıştırın ve tüm geçmeden önce tamamladığınızdan emin olun olanak sağlar.

Bir birleşim gelen bağlantıları sınırsız sayıda varken bu bağlantıları geçmeyen biri bir işlem hattı olabilir. Gelen sırası bağlantılarının sayısı sınırlı değildir. Birleşim ile birden çok gelen ardışık düzen bağlantılar oluşturma ve runbook'u kaydedin izin verilir, ancak ne zaman başarısız çalıştırıldığında.

Aşağıdaki örnek, bir dizi sanal makineyi aynı anda bu makinelere uygulanacak düzeltme ekleri indirilirken başlayan bir runbook bir parçasıdır. Bir birleşim runbook devam etmeden önce her iki işlem tamamlandığından emin olmak için kullanılır.

![Birleşim](media/automation-graphical-authoring-intro/runbook-junction.png)

### <a name="cycles"></a>Döngüler
Bir döngü bir hedef etkinlik bir bağlantı geri kendi kaynak etkinliği sonunda bağlantılar kaynağına geri başka bir etkinliğin durumdur. Döngüleri grafik yazma şu anda izin verilmiyor. Runbook'unuz bir döngü sahipse, düzgün şekilde kaydeder ancak çalışırken bir hata alır.

![Döngüsü](media/automation-graphical-authoring-intro/runbook-cycle.png)

### <a name="sharing-data-between-activities"></a>Etkinlikler arasında veri paylaşımı
Çıkış giden bir bağlantı içeren bir etkinlik tarafından herhangi bir veri yazılır *veri yoluna* runbook. Herhangi bir etkinliği runbook parametre değerleri doldurmak veya komut dosyası kodu içinde dahil üzerinde veri yoluna veri kullanabilirsiniz. Bir etkinliği iş akışındaki önceki tüm etkinlik çıkışı erişebilir. 

Veriler veri yoluna nasıl yazılır etkinlik bağlantı türüne bağlıdır. İçin bir **ardışık düzen**, katları nesneler olarak çıkış veridir. İçin bir **dizisi** bağlantı, veri çıkış dizisi olarak değil. Tek bir değer ise bir tek öğe dizisi olarak çıktı olur.

İki yöntemden birini kullanarak veri yoluna verilere erişebilir. İlk kullanarak bir **etkinlik çıkışı** başka bir etkinliğin bir parametreyi doldurmak için veri kaynağı. Çıktı bir nesne ise, tek bir özellik belirtebilirsiniz.

![Etkinlik çıkışı](media/automation-graphical-authoring-intro/activity-output-datasource-revised20165.png)

Bir etkinlik çıktısı da alabilir bir **PowerShell ifadesi** veri kaynağı veya bir **iş akışı betiği** ActivityOutput değişkeniyle etkinlik. Çıktı bir nesne ise, tek bir özellik belirtebilirsiniz. ActivityOutput değişkenleri aşağıdaki sözdizimini kullanın.

    $ActivityOutput['Activity Label']
    $ActivityOutput['Activity Label'].PropertyName 

### <a name="checkpoints"></a>Kontrol noktaları
Ayarlayabileceğiniz [kontrol noktaları](automation-powershell-workflow.md#checkpoints) seçerek bir grafik PowerShell iş akışı runbook'ta *denetim noktası runbook* herhangi bir etkinlik üzerinde. Bu etkinliğin çalıştıktan sonra ayarlamak bir denetim noktası neden olur.

![Kontrol noktası](media/automation-graphical-authoring-intro/set-checkpoint.png)

Kontrol noktaları grafik PowerShell iş akışı runbook'ları yalnızca etkinleştirilen, grafik runbook'larında kullanılabilir değildir. Runbook Azure cmdlet'lerini kullanıyorsa, runbook askıya alınır ve yeniden durumda Add-AzureRMAccount belirttiğinizde herhangi bir etkinliği izlemelisiniz farklı bir çalışan üzerinde bu kontrol noktasından. 

## <a name="authenticating-to-azure-resources"></a>Azure kaynaklarında kimlik doğrulaması
Azure kaynaklarını yöneten Azure automation'daki Runbook'lar Azure kimlik doğrulaması gerektirir. [Farklı çalıştır hesabı](automation-offering-get-started.md#creating-an-automation-account) (hizmet sorumlusu olarak da bilinir) Automation runbook'larıyla aboneliğinizdeki Azure Resource Manager kaynaklarına erişmek için varsayılan yöntemdir. Bu işlevselliği ekleyerek bir grafik runbook eklemek için **AzureRunAsConnection** PowerShell kullanarak bağlantı varlığı [Get-AutomationConnection](https://technet.microsoft.com/library/dn919922%28v=sc.16%29.aspx) cmdlet'ini ve [ Add-AzureRmAccount](https://msdn.microsoft.com/library/mt619267.aspx) tuvale cmdlet'i. Bu aşağıdaki örnekte gösterilmiştir:<br>![Kimlik doğrulama etkinlikler olarak çalıştırmak](media/automation-graphical-authoring-intro/authenticate-run-as-account.png)<br>
Farklı Çalıştır bağlantısını Al etkinliği (diğer bir deyişle, Get-AutomationConnection) AzureRunAsConnection adlı bir sabit değer veri kaynağı ile yapılandırılır.<br>![Bağlantı yapılandırma olarak çalıştır](media/automation-graphical-authoring-intro/authenticate-runas-parameterset.png)<br>
Sonraki etkinliği, Add-AzureRmAccount, kimliği doğrulanmış farklı çalıştır hesabı kullanmak için runbook'ta ekler.<br>
![Add-AzureRmAccount Parameter Set](media/automation-graphical-authoring-intro/authenticate-conn-to-azure-parameter-set.png)<br>
Parametreler için **APPLİCATİONID**, **CERTIFICATETHUMBPRINT**, ve **TENANTID** için alan yolu için özellik adını belirtmek zorunda etkinliği birden fazla özelliğe sahip bir nesne çıkarır. Aksi durumda, runbook yürüttüğünüzde kimliğini doğrulama denemesinde başarısız olur. En azından, runbook farklı çalıştır hesabıyla kimlik doğrulaması için gerekenler budur.

Bir Otomasyon hesabı kullanarak oluşturduğunuz aboneleri için geriye dönük uyumluluğu korumak için bir [Azure AD kullanıcı hesabı](automation-create-aduser-account.md) Azure Klasik dağıtım yönetmek için veya Azure Resource Manager kaynakları için kimlik doğrulaması yöntemi Add-AzureAccount cmdlet'iyle bir [kimlik bilgisi varlığı](automation-credentials.md) erişimi olan bir Active Directory kullanıcı Azure hesabına temsil eder.

Add-AzureAccount etkinlik tarafından izlenen tuvale bir kimlik bilgisi varlığı ekleyerek, bu işlev bir grafik runbook ekleyebilirsiniz. Add-AzureAccount kimlik bilgisi etkinlik kendi giriş için kullanır. Bu aşağıdaki örnekte gösterilmiştir:

![Kimlik doğrulama etkinlikleri](media/automation-graphical-authoring-intro/authentication-activities.png)

Runbook'un sonra her bir denetim noktası başlatma sırasında kimlik doğrulaması yapmalıdır. Bu, herhangi bir Checkpoint-Workflow etkinliği sonra ek Add-AzureAccount etkinlik ekleme anlamına gelir. Aynı kullanabileceğiniz bir ek kimlik bilgileri etkinlik gerekmez 

![Etkinlik çıkışı](media/automation-graphical-authoring-intro/authentication-activity-output.png)

## <a name="runbook-input-and-output"></a>Giriş ve çıkış Runbook
### <a name="runbook-input"></a>Runbook giriş
Geçerli bir alt öğesi olarak kullanılıyorsa, giriş Azure Portalı aracılığıyla runbook başlattığınızda bir kullanıcı ya da başka bir runbook'tan bir runbook'u gerektirebilir.
Örneğin, bir sanal makine oluşturur bir runbook'unuz varsa, runbook'u başlatmak adı gibi bilgileri sanal makine ve diğer özellikleri her zaman sağlamanız gerekebilir. 

Bir tanımlayarak bir runbook için giriş kabul edin veya daha fazla giriş parametreleri. Runbook her başlatıldığında bu parametreler için değerler sağlayın. Azure portalıyla bir runbook'u başlattığınızda her runbook'un giriş parametreleri için değerleri girmenizi ister.

Bir runbook'un giriş parametreleri tıklatarak erişebilirsiniz **giriş ve çıkış** runbook araç çubuğunda. 

Bu açılır **giriş ve çıkış** , var olan bir giriş parametresi düzenleyin veya tıklatarak yeni bir tane oluşturun denetim **giriş Ekle**. 

![Giriş Ekle](media/automation-graphical-authoring-intro/runbook-edit-add-input.png)

Her giriş parametresi, aşağıdaki tabloda özelliklerine göre tanımlanır:

| Özellik | Açıklama |
|:--- |:--- |
| Ad |Parametrenin benzersiz adı. Bu, yalnızca alfa sayısal karakterler içerebilir ve bir boşluk içeremez. |
| Açıklama |Giriş parametresi isteğe bağlı bir açıklama. |
| Tür |Veri türü için parametre değeri bekleniyor. Azure portalı uygun bir denetim için her parametre için veri türü için giriş isterken sağlar. |
| Zorunlu |Bir değer parametresi için sağlanan olup olmadığını belirtir. Tanımlanmış bir varsayılan değeri yok zorunlu her parametre için bir değer belirtmezseniz, runbook başlatılamıyor. |
| Varsayılan Değer |Bir sağlanmazsa, parametresi için hangi değerin kullanıldığını belirtir. Bu Null ya da belirli bir değer olabilir. |

### <a name="runbook-output"></a>Runbook çıkışı
Giden bir bağlantı yok herhangi bir etkinlik tarafından oluşturulan veri olduğu için [runbook'un çıktı](http://msdn.microsoft.com/library/azure/dn879148.aspx). Çıktı runbook işi ile kaydedilen ve runbook bir alt öğesi olarak kullanıldığında, üst runbook için kullanılabilir. 

## <a name="powershell-expressions"></a>PowerShell ifadeleri
Grafik yazma avantajlarından biri, PowerShell ilişkin minimum bilgi ile bir runbook oluşturma olanağı sağlamaktadır. Şu anda, ancak belirli doldurmak için biraz PowerShell bilmeniz gereken [parametre değerlerini](#activities) ve ayarı için [bağlantı koşulları](#links-and-workflow). Bu bölüm, PowerShell ifadeler ile tanıdık olmayabilir olan kullanıcılar için bir giriş sağlar. PowerShell tam ayrıntıları şurada bulunabilir [ile Windows PowerShell komut dosyası](http://technet.microsoft.com/library/bb978526.aspx). 

### <a name="powershell-expression-data-source"></a>PowerShell ifadesi veri kaynağı
Bir PowerShell ifadesi bir veri kaynağı olarak değerini doldurmak için kullanabileceğiniz bir [Etkinlik parametresi](#activities) sonuçları bazı PowerShell kodu ile. Bu tek satırlık bir bazı basit işlevi veya karmaşık bir mantık gerçekleştirmek birden çok satır gerçekleştirir kod olabilir. Herhangi bir değişkene atanmamış bir komut çıktısı parametre değeri çıkışı yapılır. 

Örneğin, aşağıdaki komutu, geçerli tarih çıktı. 

    Get-Date

Aşağıdaki komutlar, geçerli tarihten itibaren dizesi oluşturma ve bir değişkene atayın. Değişkeni içeriğini sonra çıkışı gönderilir 

    $string = "The current date is " + (Get-Date)
    $string

Aşağıdaki komutları geçerli tarih değerlendirmek ve geçerli gün, hafta sonu veya haftanın günü olup olmadığını gösteren bir dize döndürür. 

    $date = Get-Date
    if (($date.DayOfWeek = "Saturday") -or ($date.DayOfWeek = "Sunday")) { "Weekend" }
    else { "Weekday" }


### <a name="activity-output"></a>Etkinlik çıkışı
Önceki bir etkinliğin çıktısını runbook'ta kullanmak için aşağıdaki sözdizimi ile $ActivityOutput değişkeni kullanın.

    $ActivityOutput['Activity Label'].PropertyName

Örneğin, bir sanal makinenin adını gerektiren bir özellik olmayan bir etkinliği olabilir; bu durumda aşağıdaki ifadeyi kullanabilirsiniz:

    $ActivityOutput['Get-AzureVm'].Name

Ardından sanal makine gereken özelliği yalnızca bir özellik yerine nesne varsa, aşağıdaki sözdizimini kullanarak nesnenin tamamı döndürecektir.

    $ActivityOutput['Get-AzureVm']

Sanal makine adı metne art arda ekler aşağıdaki gibi daha karmaşık bir ifadede bir etkinlik çıkışı de kullanabilirsiniz.

    "The computer name is " + $ActivityOutput['Get-AzureVm'].Name


### <a name="conditions"></a>Koşullar
Kullanım [Karşılaştırma işleçleri](https://technet.microsoft.com/library/hh847759.aspx) değerleri karşılaştırmak ya da bir değeri belirtilen desenle eşleşip eşleşmediğini belirlemek için. Bir karşılaştırma $true veya $false değerini döndürür.

Örneğin, aşağıdaki iki koşul bir etkinlik sanal makineden adlı olup olmadığını belirler. *Get-AzureVM* şu anda *durduruldu*. 

    $ActivityOutput["Get-AzureVM"].PowerState –eq "Stopped"

Aynı sanal makine herhangi başka bir durumda olup olmadığını denetler şu koşul *durduruldu*.

    $ActivityOutput["Get-AzureVM"].PowerState –ne "Stopped"

Birden çok koşul kullanarak katılabilirsiniz bir [mantıksal işleç](https://technet.microsoft.com/library/hh847789.aspx) gibi **- ve** veya **- veya**. Örneğin, aynı sanal makine önceki örnekte bir durumda olup olmadığını denetler şu koşul *durduruldu* veya *durdurma*.

    ($ActivityOutput["Get-AzureVM"].PowerState –eq "Stopped") -or ($ActivityOutput["Get-AzureVM"].PowerState –eq "Stopping") 


### <a name="hashtables"></a>Hashtable'da
[Hashtable'da](http://technet.microsoft.com/library/hh847780.aspx) değer kümesini döndürmek için yararlı olan ad/değer çiftleri. Özellikler belirli etkinlikler için basit bir değer yerine bir hashtable bekleyebilir. Hashtable için bir sözlük olarak adlandırılan olarak görebilirsiniz. 

Aşağıdaki sözdizimi ile bir karma tablosu oluşturun. Bir karma tablosu girdileri herhangi bir sayıda içerebilir ancak her bir ad ve değer tarafından tanımlanır.

    @{ <name> = <value>; [<name> = <value> ] ...}

Örneğin, aşağıdaki ifade veri kaynağında bir internet arama değerlerini içeren bir hashtable beklenen bir etkinlik parametresi için kullanılan bir karma tablosu oluşturur.

    $query = "Azure Automation"
    $count = 10
    $h = @{'q'=$query; 'lr'='lang_ja';  'count'=$Count}
    $h

Aşağıdaki örnek, çıktı olarak adlandırılan bir etkinliğin kullanır *Twitter bağlantısını Al* bir hashtable doldurmak için.

    @{'ApiKey'=$ActivityOutput['Get Twitter Connection'].ConsumerAPIKey;
      'ApiSecret'=$ActivityOutput['Get Twitter Connection'].ConsumerAPISecret;
      'AccessToken'=$ActivityOutput['Get Twitter Connection'].AccessToken;
      'AccessTokenSecret'=$ActivityOutput['Get Twitter Connection'].AccessTokenSecret}



## <a name="next-steps"></a>Sonraki Adımlar
* PowerShell iş akışı runbook'larını kullanmaya başlamak için bkz. [İlk PowerShell iş akışı runbook uygulamam](automation-first-runbook-textual.md) 
* Grafik runbook'ları kullanmaya başlamak için bkz. [İlk grafik runbook uygulamam](automation-first-runbook-graphical.md)
* Runbook türleri, avantajları ve sınırlamaları hakkında daha fazla bilgi için bkz. [Azure Automation runbook türleri](automation-runbook-types.md)
* Automation farklı çalıştır hesabını kullanarak kimlik doğrulamasını nasıl anlamak için bkz: [yapılandırma Azure farklı çalıştır hesabı](automation-sec-configure-azure-runas-account.md)

