---
title: Azure Otomasyonu'nda yazma grafik
description: Grafik yazma olmadan kodu ile çalışma için Azure Otomasyonu runbook'ları oluşturmanıza olanak sağlar. Bu makalede, grafik yazma giriş ve bir grafik runbook oluşturmaya başlamak için gereken tüm ayrıntıları sağlar.
services: automation
ms.service: automation
ms.subservice: process-automation
author: bobbytreed
ms.author: robreed
ms.date: 03/16/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 6d7626706951cc522dce9c6d70251455e64300bc
ms.sourcegitcommit: f811238c0d732deb1f0892fe7a20a26c993bc4fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/29/2019
ms.locfileid: "67476684"
---
# <a name="graphical-authoring-in-azure-automation"></a>Azure Otomasyonu'nda yazma grafik

Grafik yazma için Azure Otomasyonu runbook'ları temel Windows PowerShell veya PowerShell iş akışı kodu karmaşıklığını oluşturmanızı sağlar. Birbirine bağlamak ve bir iş akışı oluşturmak için yapılandırma etkinlikler, cmdlet'leri ve runbook'ları kitaplığından tuvale Ekle. System Center Orchestrator'ı veya Service Management Automation (SMA) ile sürekli çalıştıysa, ardından bu size tanıdık gelecektir

Bu makalede, grafik yazma giriş ve bir grafik runbook oluşturma başlamak için gereken kavramlar sağlar.

## <a name="graphical-runbooks"></a>Grafik runbook'ları

Tüm runbook'ları Azure automation'da Windows PowerShell iş akışlarıdır. Otomasyon İşçileri tarafından çalıştırılan PowerShell kodu grafik ve grafik PowerShell iş akışı runbook'ları oluşturmak, ancak görüntüleme veya doğrudan değiştirmek mümkün değildir. Grafik runbook için bir grafik PowerShell iş akışı runbook ve tersi dönüştürülebilir, ancak bir metinsel runbook'a dönüştürülemez. Var olan bir metinsel runbook grafik düzenleyicisini aktarılamaz.

## <a name="overview-of-graphical-editor"></a>Grafik Düzenleyicisi'ne genel bakış

Azure portalında oluşturma veya düzenleme grafik runbook grafik düzenleyicisini açabilirsiniz.

![Grafik çalışma](media/automation-graphical-authoring-intro/runbook-graphical-editor.png)

Aşağıdaki bölümlerde, grafik düzenleyicisini denetimleri açıklanmaktadır.

### <a name="canvas"></a>Canvas

Burada runbook'unuzu tasarım tuvalidir. Kitaplık denetiminde düğümlerden runbook'a etkinlikler ekleme ve bunları runbook mantığını tanımlamak için bağlantılar ile bağlayın.

Yakınlaştırma ve uzaklaştırma için tuvalin alt kısmında denetimleri kullanın.

### <a name="library-control"></a>Denetim Kitaplığı

Seçtiğiniz kitaplık denetimidir [etkinlikleri](#activities) runbook uygulamanıza eklenecek. Bunları diğer etkinlikler için bunları eriştiğiniz tuvaline ekleyin. Aşağıdaki tabloda açıklanan dört bölüm içerir:

| `Section` | Description |
|:--- |:--- |
| Cmdlet'ler |Runbook'ta kullanılabilecek tüm cmdlet'leri içerir. Cmdlet modülü tarafından düzenlenir. Tüm otomasyon hesabınızda yüklü modülleri mevcuttur. |
| Runbook'lar |Runbook'ları, Otomasyon hesabınızda içerir. Bu runbook'ları, tuvalin alt runbook'lar olarak kullanılacak eklenebilir. Yalnızca düzenlenmekte olan runbook olarak aynı temel türündeki runbook'lar gösterilir. Grafik PowerShell iş akışı runbook'ları için yalnızca PowerShell iş akışı tabanlı runbook'ları gösterilmekte iken için grafik runbook'ları yalnızca PowerShell tabanlı runbook'ları gösterilir. |
| Varlıklar |İçerir [Otomasyon varlıklarından](/previous-versions/azure/dn939988(v=azure.100)) Otomasyon hesabınızda, bir runbook'ta kullanılabilir. Bir varlığı bir runbook'a eklediğinizde, seçili varlığı alır bir iş akışı etkinlik ekler. Değişken varlıklar söz konusu olduğunda, bir değişkeni almak veya değişkeni ayarlamak için bir etkinlik eklenip eklenmeyeceğini seçebilirsiniz. |
| Runbook denetimi |Geçerli bir runbook'ta kullanılabilir runbook denetimi etkinlikleri içerir. A *birleşim* birden çok girdiyi alır ve iş akışı devam etmeden önce tüm tamamlanana kadar bekler. A *kod* etkinlik çalıştırmalarını PowerShell ya da PowerShell iş akışı kodu grafik runbook türüne bağlı olarak bir veya daha fazla satır. Bu etkinlik diğer etkinlikler ile elde etmek zor işlevleri veya özel kod için kullanabilirsiniz. |

### <a name="configuration-control"></a>Yapılandırma denetimi

Tuvalde seçili bir nesne için Ayrıntılar sağlarsınız yapılandırma denetimidir. Bu denetimde kullanılabilir özellikleri, seçili nesnenin türüne bağlıdır. Yapılandırma denetimi bir seçeneği belirlediğinizde, ek bilgi sağlamak için ek dikey pencereleri açılır.

### <a name="test-control"></a>Test denetimi

Grafik düzenleyicisini ilk başlatıldığında Test denetimi görüntülenmez. Ne zaman açıldığında, etkileşimli olarak [grafiksel bir runbook test](#graphical-runbook-procedures).

## <a name="graphical-runbook-procedures"></a>Grafik runbook yordamları

### <a name="exporting-and-importing-a-graphical-runbook"></a>Grafik runbook'u alma ve verme

Yalnızca bir grafik runbook yayımlanmış sürümü dışarı aktarabilirsiniz. Runbook henüz yayımlanmamış, ardından **dışarı** düğmesi devre dışıdır. Tıkladığınızda **dışarı** düğmesi, runbook'u yerel bilgisayarınıza indirilir. Runbook adı dosya adıyla eşleşen bir *graphrunbook* uzantısı.

Seçerek bir grafik veya grafik PowerShell iş akışı runbook dosyası içeri aktarabilirsiniz **alma** runbook eklerken seçenek. İçeri aktarılacak dosyasını seçtiğinizde, aynı tutabilirsiniz **adı** veya yeni bir tane sağlayın. Runbook türü alan runbook türü seçilen dosyanın değerlendirir ve doğru değil, farklı bir tür seçmek çalışırsanız, bir ileti olası çakışmaları vardır ve dönüştürme sırasında olabilir söz dizimi hatalarının ayıklanabileceğini sunulacak sonra görüntüler hataları.

![Runbook'u İçeri Aktar](media/automation-graphical-authoring-intro/runbook-import-revised20165.png)

### <a name="testing-a-graphical-runbook"></a>Grafik runbook'u test etme

Runbook'un yayımlanan sürümünü değişmeden ya da önce yayımlanmış olan yeni bir runbook test edebilirsiniz Azure portalında bir runbook'un taslak sürümünü test edebilirsiniz. Bu, yayımlanan sürümü değiştirmeden önce runbook'un doğru çalıştığını doğrulamanızı sağlar. Bir runbook'u test ettiğinizde, Taslak runbook yürütülür ve gerçekleştirdiği tüm işlemler tamamlanır. Hiçbir iş geçmişi oluşturulmaz, ancak çıkış Test çıkışı bölmesinde görüntülenir.

Bir runbook için Test denetimi düzenleme için runbook açarak açın ve ardından **Test bölmesi** düğmesi.

Giriş parametreleri ve tıklayarak runbook'u başlatabilirsiniz Test denetimi ister **Başlat** düğmesi.

### <a name="publishing-a-graphical-runbook"></a>Grafik runbook yayımlama

Azure Otomasyonu içindeki her runbook'un bir taslak ve bir yayımlanmış sürümü vardır. Yalnızca Yayımlanan sürüm çalıştırılabilir ve yalnızca Taslak sürüm düzenlenebilir. Yayımlanan sürüm Taslak sürümdeki herhangi bir değişiklikten etkilenmez. Taslak sürümü kullanılabilir olması hazır olduğunda size, yayımlanan sürümü taslak sürümle değiştirebilirsiniz yayımlamalısınız.

Grafik runbook'u düzenlemek ve ardından tıklayarak runbook'u açarak yayımlayabilirsiniz **Yayımla** düğmesi.

Bir runbook henüz yayımlanmamış, durumuna sahip **yeni**. Yayımlandığında, durumuna sahip **yayımlanan**. Runbook yayımlandıktan sonra taslak ve yayımlanan sürümleri farklı düzenlerseniz, runbook durumuna sahip **düzenleme**.

![Runbook durumları](media/automation-graphical-authoring-intro/runbook-statuses-revised20165.png)

Ayrıca, bir runbook yayımlanan sürümüne geri seçeneğiniz de vardır. Bu runbook son yayımlandığı ve runbook'un taslak sürümünü yayımlanan sürüm ile değiştirir. bu yana yapılan tüm değişiklikler hemen oluşturur.

## <a name="activities"></a>Etkinlikler

Etkinlikleri bir runbook'un yapı taşlarıdır. Bir etkinlik, bir PowerShell cmdlet'i, bir alt runbook veya iş akışı etkinlik olabilir. Kitaplık denetiminde sağ tıklatıp seçerek bir etkinliği runbook'a ekleyin **tuvale Ekle**. ' A tıklayın ve etkinlik istediğiniz tuvalde herhangi bir yere yerleştirmek için sürükleyin. Etkinlik tuval üzerindeki konumunu işlemi runbook'un herhangi bir şekilde etkilemez. Runbook'u kullanıma alın, ancak bunu kendi işlemi görselleştirmek en uygun bulduğunuz düzenleyebilirsiniz.

![Tuvale Ekle](media/automation-graphical-authoring-intro/add-to-canvas-revised20165.png)

Özelliklerini ve parametre yapılandırma dikey penceresinde yapılandırmak için tuvalde etkinliği seçin. Değiştirebileceğiniz **etiket** etkinliğin için açıklayıcı bir şey. Özgün cmdlet'i hala çalıştırıldığı, grafik Düzenleyicisi içinde kullanılan görünen adını değiştirmeniz yeterlidir. Etiketi runbook içinde benzersiz olmalıdır.

### <a name="parameter-sets"></a>Parametre kümeleri

Parametre kümesi, belirli bir cmdlet için değerleri kabul zorunlu ve isteğe bağlı parametreleri tanımlar. Tüm cmdlet'ler kümesi en az bir parametreye sahip ve birden çok vardır. Bir cmdlet birden fazla parametre kümesine sahiptir, parametrelerini yapılandırmadan önce kullandığınız hangisini seçmelisiniz. Yapılandırabileceğiniz parametreleri seçtiğiniz parametre kümesi bağlıdır. Parametre kümesi seçerek bir etkinlik tarafından kullanılan değiştirebilirsiniz **parametre** ve başka bir kümesini seçme. Bu durumda, yapılandırdığınız tüm parametre değerlerini kaybolur.

Aşağıdaki örnekte, Get-AzureRmVM cmdlet'i, üç parametre kümesine sahiptir. Parametre değerleri, parametre kümeleri birini kadar yapılandıramazsınız. ListVirtualMachineInResourceGroupParamSet parametre kümesi, bir kaynak grubundaki tüm sanal makineler döndürmek için olan ve tek bir isteğe bağlı parametreye sahiptir. The **GetVirtualMachineInResourceGroupParamSet** is for specifying the virtual machine you want to return and has two mandatory and one optional parameter.

![Parametre kümesi](media/automation-graphical-authoring-intro/get-azurermvm-parameter-sets.png)

#### <a name="parameter-values"></a>Parametre değerleri

Bir parametre için değer belirttiğinizde, değerin nasıl belirtildiğine belirlemek için bir veri kaynağı seçin. Bu parametre için geçerli değerleri belirli bir parametre bağımlı için kullanılabilen veri kaynakları. Örneğin, Null, null değerlere izin vermiyor bir parametre için kullanılabilir bir seçenek değil.

| Veri Kaynağı | Description |
|:--- |:--- |
| Sabit değer |Parametre için bir değer yazın. Bu yalnızca aşağıdaki veri türleri için kullanılabilir. Int32, Int64, String, Boolean, DateTime, geçiş yapın. |
| Etkinlik çıkışı |İş akışında geçerli etkinliği önündeki bir etkinliğin çıkışı. Tüm geçerli etkinlikler listelenir. Çıktısını için parametre değeri kullanmak için yalnızca etkinliği seçin. Birden fazla özelliğe sahip bir nesne etkinlik çıkışı etkinlik seçtikten sonra özelliğin adını yazabilirsiniz. |
| Runbook giriş |Bir runbook girdi parametreniz etkinlik parametresinde girdi olarak seçin. |
| Değişken varlığı |Bir Otomasyon değişken giriş olarak seçin. |
| Kimlik bilgisi varlığı |Otomasyon kimlik bilgileri, giriş olarak seçin. |
| Sertifika varlığı |Bir Otomasyon sertifikası giriş olarak seçin. |
| Bağlantı varlığı |Otomasyon bağlantısı, giriş olarak seçin. |
| PowerShell ifadesi |Basit belirtin [PowerShell ifadesi](#powershell-expressions). İfade, etkinlik ve parametre değeri için kullanılan sonucu önce değerlendirilir. Bir etkinlik veya bir runbook girdi parametreniz çıkışına başvurmak için değişkenleri kullanabilirsiniz. |
| Yapılandırılmadı |Önceden yapılandırılmış herhangi bir değer temizler. |

#### <a name="optional-additional-parameters"></a>İsteğe bağlı ek parametreler

Tüm cmdlet'ler ek parametreleri sağlamak için seçeneğiniz vardır. PowerShell genel parametreleri veya diğer özel Parametreler şunlardır. Bir metin kutusu PowerShell söz dizimini kullanarak parametrelerini nerede sağlayabilir sunulur. Örneğin, kullanılacak **ayrıntılı** ortak parametresi belirtirsiniz **"-Verbose: $True"** .

### <a name="retry-activity"></a>Etkinliği yeniden deneyin

**Yeniden deneme davranışı** belirli bir koşul yerine getirilene kadar birden çok kez çalıştırılması bir etkinliği sağlar benzer şekilde; bir döngü. Birden çok kez çalıştırmanız gerekir; hataya açık alanlardır etkinlikler için bu özelliği kullanabilir ve ihtiyaç duyabileceğiniz birden fazla başarı için çalışır, veya geçerli veriler bir etkinliğin çıktı bilgilerinin test.

Bir etkinlik için yeniden deneme etkinleştirdiğinizde, gecikme ve bir koşul ayarlayabilirsiniz. Gecikme (saniyeler veya dakikalar içinde ölçülür) zamandır yeniden etkinlik çalışmadan önce runbook bekler. Daha sonra gecikme belirtilmişse tamamlandıktan hemen sonra etkinlik yeniden çalışır.

![Etkinlik yeniden deneme gecikmesi](media/automation-graphical-authoring-intro/retry-delay.png)

Yeniden deneme koşulu etkinliği her çalıştığında sonra yürütülecek bir PowerShell ifadesidir. İfade True olarak çözümlenirse, etkinlik yeniden çalıştırır. İfade False çözümlenirse, ardından etkinlik yeniden çalıştırmaz ve sonraki etkinliğe runbook geçer.

![Etkinlik yeniden deneme gecikmesi](media/automation-graphical-authoring-intro/retry-condition.png)

Yeniden deneme koşulu, etkinlik yeniden deneme işlemleri ile ilgili bilgilere erişim sağlayan $RetryData adında bir değişkene kullanabilirsiniz. Bu değişken aşağıdaki tabloda özelliklere sahiptir:

| Özellik | Description |
|:--- |:--- |
| NumberOfAttempts |Etkinliği çalıştırmak kez sayısı. |
| Output |Son çalıştırma etkinliğin çıkışı. |
| TotalDuration |Zaman aşımına etkinliği ilk kez başlatıldığından beri geçen. |
| StartedAt |UTC biçiminde etkinliğin saat önce başlatıldı. |

Aşağıda örnekler etkinlik yeniden deneme koşulları.

```powershell-interactive
# Run the activity exactly 10 times.
$RetryData.NumberOfAttempts -ge 10
```

```powershell-interactive
# Run the activity repeatedly until it produces any output.
$RetryData.Output.Count -ge 1
```

```powershell-interactive
# Run the activity repeatedly until 2 minutes has elapsed.
$RetryData.TotalDuration.TotalMinutes -ge 2
```

Bir etkinlik için bir yeniden deneme koşulu yapılandırdıktan sonra etkinlik anımsatması için iki görsel ipuçları içerir. Bir etkinliğin sunulur ve etkinliğin yapılandırmasını gözden geçirin, diğeri ise.

![Etkinlik yeniden deneme Visual göstergeleri](media/automation-graphical-authoring-intro/runbook-activity-retry-visual-cue.png)

### <a name="workflow-script-control"></a>İş akışı betiği denetimi

PowerShell veya PowerShell iş akışı betiği, aksi takdirde kullanılamayabilir işlevselliği sağlamak için yazılan grafik runbook türüne bağlı olarak kabul eden özel bir aktivite kod denetimidir. Parametreleri kabul edemez ancak etkinlik çıkışı ve runbook giriş parametreleri için değişkenleri kullanabilirsiniz. Hiçbir bağlantı giden runbook çıkışına durumda eklenir sahip olmadığı sürece herhangi bir etkinliğin çıktı veri yoluna eklenir.

Örneğin, aşağıdaki kodu $NumberOfDays adlı runbook giriş değişkeni kullanarak bir tarih hesaplamaları gerçekleştirir. Daha sonra runbook'taki izleyen etkinlikler tarafından kullanılmak üzere çıktı olarak bir hesaplanan tarih saat gönderir.

```powershell-interactive
$DateTimeNow = (Get-Date).ToUniversalTime()
$DateTimeStart = ($DateTimeNow).AddDays(-$NumberOfDays)}
$DateTimeStart
```

## <a name="links-and-workflow"></a>Bağlantılar ve iş akışı

A **bağlantı** grafik bir runbook'ta iki etkinlik bağlanır. Tuvalde, kaynak etkinliğinden hedef etkinliği işaret eden bir ok olarak görüntülenir. Etkinlikler, kaynak etkinliği tamamlandıktan sonra Başlangıç hedef etkinlik ile oku yönde çalıştırın.

### <a name="create-a-link"></a>Bir bağlantı oluşturun

Kaynak etkinliği'ni seçip şeklin alt kısmındaki daire tıklayarak iki etkinlik arasında bir bağlantı oluşturun. Hedef etkinlik ve yayın için oku sürükleyin.

![Bir bağlantı oluşturun](media/automation-graphical-authoring-intro/create-link-revised20165.png)

Yapılandırma dikey penceresinde özelliklerini yapılandırmak için bağlantıyı seçin. Bu aşağıdaki tabloda açıklanan bağlantı türü içerir:

| Bağlantı türü | Description |
|:--- |:--- |
| İşlem hattı |Hedef etkinlik, kaynak etkinliğinden her nesne çıkış için bir kez çalıştırılır. Kaynak etkinliği hiçbir çıkış sonuçlanırsa, hedef etkinlik çalıştırmaz. Kaynak etkinliği çıktısını bir nesne olarak kullanılabilir. |
| Dizisi |Hedef etkinlik yalnızca bir kez çalışır. Bu kaynak etkinliğinden nesnelerinin bir dizisini alır. Kaynak etkinliği çıktısını bir nesne dizisi kullanılabilir. |

### <a name="starting-activity"></a>Başlangıç etkinliği

Grafik runbook'u gelen bir bağlantısı olmayan tüm etkinlikleri ile başlar. Runbook için başlangıç etkinliği olarak davranır yalnızca bir etkinlik genellikle budur. Birden çok etkinlik bir gelen bağlantı yoksa, paralel şekilde yürüterek runbook başlatır. Bu, diğer etkinlikler her tamamlandığında çalıştırılacak bağlantıları izler.

### <a name="conditions"></a>Koşullar

Bir bağlantıya koşul belirttiğinizde, hedef etkinlik döndüğüne ise yalnızca çalıştırılır true. Genellikle bir $ActivityOutput değişkeni bir koşul kaynak etkinliğinden çıktısını almak için kullandığınız

Bir işlem hattı bağlantısı için tek bir nesne için bir koşul belirtin ve koşul, her nesne çıkış için kaynak etkinliği tarafından değerlendirilir. Hedef etkinlik, koşulu karşılayan her bir nesne için ardından çalıştırın. Örneğin, Get-AzureRmVm ile bir kaynak etkinliği, aşağıdaki söz dizimini koşullu işlem hattı bağlantısı yalnızca sanal makineleri adlı kaynak grubunda kullanılabilir *Group1*.

```powershell-interactive
$ActivityOutput['Get Azure VMs'].Name -match "Group1"
```

Kaynak etkinliği tüm nesnelerini çıkışından içeren tek bir dizi döndürüldüğünden dizisi bağlantı için koşulu yalnızca bir kez değerlendirilir. Bu nedenle, bir sıra bağlantı gibi bir ardışık düzen bağlantısına filtreleme için kullanılamaz, ancak yalnızca bir sonraki etkinlik çalıştırılır olup olmadığını belirler. Örneğin aşağıdaki etkinlikler kümesini VM Başlat runbook'umuz yararlanın.

![Dizileri ile koşullu bağlantı](media/automation-graphical-authoring-intro/runbook-conditional-links-sequence.png)

Değerleri belirlemek için VM adını ve kaynak grubu adını temsil eden iki runbook giriş parametreleri için gerçekleştirilecek - uygun eylemi olan sağlandı doğrulanıyor üç farklı sıra bağlantı tek bir VM'yi başlatma, tüm sanal makinelerin kaynak Başlat vardır Grup veya bir Abonelikteki tüm sanal makineler. Azure'a bağlanma ve Get tek VM arasında dizisi bağlantısını için koşul mantığı şöyledir:

```powershell-interactive
<#
Both VMName and ResourceGroupName runbook input parameters have values
#>
(
(($VMName -ne $null) -and ($VMName.Length -gt 0))
) -and (
(($ResourceGroupName -ne $null) -and ($ResourceGroupName.Length -gt 0))
)
```

Bir koşullu bağlantı kullandığınızda, veri yok kaynak etkinliğinden Bu dalda diğer etkinliklere koşul tarafından filtrelenir. Bir etkinlik birden çok bağlantı kaynağına ise, bu dala bağlanan bağlantı koşulunda etkinliklere her dalda kullanılabilir verileri bağlıdır.

Örneğin, **Start-AzureRmVm** tüm sanal makineler aşağıdaki runbook etkinliğinde başlatır. İki koşullu bağlantılar var. İlk koşullu bağlantı ifade kullanır *$ActivityOutput ['Start-AzureRmVM']. IsSuccessStatusCode - eq $true* Start-AzureRmVm etkinliği başarıyla tamamlandıysa filtrelemek için. İkinci deyim kullanan *$ActivityOutput ['Start-AzureRmVM']. IsSuccessStatusCode - ne $true* sanal makineyi başlatmak Start-AzureRmVm etkinliği başarısız olursa filtrelemek için.

![Koşullu bağlantı örneği](media/automation-graphical-authoring-intro/runbook-conditional-links.png)

İlk bağlantı ve Get-AzureVM etkinlik çıkışı Get-AzureVM çalıştırıldığı zaman başlatılan sanal makinelerin yalnızca get kullanır izleyen etkinlik. İkinci bağlantıya takip eden herhangi bir etkinliği yalnızca Get-AzureVM çalıştırıldığı zaman durdurulan sanal makinelerin alır. Üçüncü bağlantı sonraki tüm etkinlikler, kendi çalışma durumuna bakılmaksızın tüm sanal makineler alır.

### <a name="junctions"></a>Merkezleriyle

Bir birleşim gelen tüm dalları tamamlanana kadar bekler özel bir etkinliktir. Bu, tüm geçmeden önce tamamladığınızdan emin olun ve paralel olarak birden çok etkinliği çalıştırmak sağlar.

Bir birleşim sınırsız sayıda gelen bağlantılar varken bu bağlantıları fazla biri bir işlem hattı olabilir. Gelen dizisi bağlantılarının sayısı sınırlı değildir. Birleşim ile birden çok gelen işlem hattı bağlantısı oluşturmanız ve runbook'u kaydetmek için izin verilir, ancak başarısız olduğunda da çalıştırın.

Aşağıdaki örnek, bir sanal makine kümesi, bu makinelere uygulanacak düzeltme ekleri aynı anda indirilirken başlayan bir runbook bir parçasıdır. Bir birleşim, runbook devam etmeden önce her iki işlem tamamlandığından emin olmak için kullanılır.

![Birleşim](media/automation-graphical-authoring-intro/runbook-junction.png)

### <a name="cycles"></a>Döngüleri

Hedef etkinlik bağlantıları geri kendi kaynak etkinliği veya sonunda kaynağına bağlantıları geri başka bir etkinlik olduğunda bir döngüsü elde edilir. Döngüleri grafik yazma şu anda izin verilmiyor. Bir döngü runbook'unuz varsa düzgün şekilde kaydeder ancak çalıştığında bir hata alır.

![Döngüsü](media/automation-graphical-authoring-intro/runbook-cycle.png)

### <a name="sharing-data-between-activities"></a>Etkinlikler arasında veri paylaşımı

Çıktı giden bir bağlantı içeren bir etkinlik tarafından herhangi bir veri yazılır *veri* runbook. Herhangi bir etkinliği runbook parametre değerleri doldurmak veya betik kodunda dahil veri verileri kullanabilirsiniz. Bir etkinlik, iş akışındaki herhangi bir önceki etkinliğin çıktısını erişebilirsiniz.

Veriler veri yoluna nasıl yazılır bağlantı üzerinde etkinlik türünü bağlıdır. İçin bir **işlem hattı**, veri çıkış katları nesnedir. İçin bir **dizisi** bağlantı verileri ise çıktı olarak bir dizi. Yalnızca bir değer ise, tek öğeli bir dizi çıktı olur.

İki yöntemden birini kullanarak veri çubuğunda verilere erişebilir. İlk olarak kullanarak bir **etkinlik çıkışı** parametre başka bir etkinliğin doldurmak için veri kaynağı. Çıktı bir nesne ise, tek bir özellik belirtebilirsiniz.

![Etkinlik çıkışı](media/automation-graphical-authoring-intro/activity-output-datasource-revised20165.png)

Bir etkinlik çıktısı da alabilirsiniz bir **PowerShell ifadesi** veri kaynağı veya bir **iş akışı betiği** ActivityOutput değişkeniyle etkinlik. Çıktı bir nesne ise, tek bir özellik belirtebilirsiniz. ActivityOutput değişkenler, aşağıdaki sözdizimini kullanın.

```powershell-interactive
$ActivityOutput['Activity Label']
$ActivityOutput['Activity Label'].PropertyName
```

### <a name="checkpoints"></a>Kontrol noktaları

Ayarlayabileceğiniz [kontrol noktaları](automation-powershell-workflow.md#checkpoints) seçerek bir grafik PowerShell iş akışı runbook'ta *denetim noktası runbook'u* herhangi bir etkinlik. Bu etkinliğin çalıştıktan sonra ayarlamak bir denetim noktası neden olur.

![Checkpoint](media/automation-graphical-authoring-intro/set-checkpoint.png)

Kontrol noktaları, grafik PowerShell iş akışı runbook'ları yalnızca etkinleştirilen, grafik runbook'larında kullanılamaz. Runbook, Azure cmdlet'lerini kullanıyorsa, runbook'u askıya alındı ve yeniden durumunda Connect-AzureRmAccount belirttiğinizde herhangi bir etkinliği izlemelidir farklı bir çalışan üzerinde bu kontrol noktasından.

## <a name="authenticating-to-azure-resources"></a>Azure kaynaklarında kimlik doğrulaması

Azure kaynaklarını yöneten Azure automation'daki Runbook'lar, Azure kimlik doğrulamasını gerektirir. [Run As hesabı](automation-create-runas-account.md) (hizmet sorumlusu olarak da bilinir) Automation runbook'larıyla aboneliğinizdeki Azure Resource Manager kaynaklarına erişmek için varsayılan yöntemdir. Ekleyerek bu işlevler için bir grafik runbook ekleyebilirsiniz **AzureRunAsConnection** PowerShell kullanarak bağlantı varlığı, [Get-AutomationConnection](https://technet.microsoft.com/library/dn919922%28v=sc.16%29.aspx) cmdlet'ini ve [ Connect-AzureRmAccount](/powershell/module/azurerm.profile/connect-azurermaccount) tuvaline cmdlet'i. Bu, aşağıdaki örnekte gösterilmiştir:

![Kimlik doğrulama etkinlikleri çalıştırma](media/automation-graphical-authoring-intro/authenticate-run-as-account.png)

Çalıştır bağlantısını Al etkinliği (diğer bir deyişle, Get-AutomationConnection) AzureRunAsConnection adlı bir sabit değer veri kaynağı ile yapılandırılır.

![Farklı Çalıştır bağlantısı yapılandırması](media/automation-graphical-authoring-intro/authenticate-runas-parameterset.png)

Connect-AzureRmAccount sonraki etkinliğe kimliği doğrulanmış farklı çalıştır hesabı kullanmak için runbook'ta ekler.

![Connect-AzureRmAccount parametre kümesi](media/automation-graphical-authoring-intro/authenticate-conn-to-azure-parameter-set.png)

> [!IMPORTANT]
> **Add-AzureRmAccount** için bir diğer ad sunulmuştur **Connect-AzureRMAccount**. Ne zaman kitaplığınızı arama öğe görmüyorsanız, **Connect-AzureRMAccount**, kullanabileceğiniz **Add-AzureRmAccount**, veya Otomasyon hesabınızda modüllerinizi güncelleştirebilirsiniz.

Parametreler için **APPLİCATİONID**, **CERTIFICATETHUMBPRINT**, ve **TENANTID** olduğundan alan yolu için özellik adını belirtmenize gerek etkinliği birden fazla özelliğe sahip bir nesne çıkarır. Aksi takdirde runbook yürütme, kimlik doğrulaması girişimi başarısız olur. En azından, runbook farklı çalıştır hesabıyla kimlik doğrulaması için ihtiyacınız olanlar budur.

Geriye dönük bir Otomasyon hesabı kullanarak oluşturduğunuz aboneler için uyumluluğu korumak için bir [Azure AD kullanıcı hesabını](automation-create-aduser-account.md) Azure Klasik dağıtımında yönetmek için veya Azure Resource Manager kaynaklarını için kimlik doğrulaması yöntemi Ekle-AzureAccount cmdlet'iyle bir [kimlik bilgisi varlığı](automation-credentials.md) erişimi olan bir Active Directory kullanıcı Azure hesabında temsil eder.

Bir kimlik bilgisi varlığı bir Add-AzureAccount etkinliği tarafından izlenen tuval ekleyerek bu işlevselliği için bir grafik runbook ekleyebilirsiniz. Ekle-AzureAccount kimlik bilgisi etkinliği, girdi için kullanır. Bu, aşağıdaki örnekte gösterilmiştir:

![Kimlik doğrulama etkinlikleri](media/automation-graphical-authoring-intro/authentication-activities.png)

Runbook'un sonra her bir denetim noktası başlatma sırasında kimlik doğrulaması gerekir. Bu, herhangi bir Checkpoint-Workflow etkinliği sonra ayrıca Add-AzureAccount etkinlik ekleme anlamına gelir. Aynı kullanabileceğinden, ek bir kimlik bilgisi etkinlik gerekmez

![Etkinlik çıkışı](media/automation-graphical-authoring-intro/authentication-activity-output.png)

## <a name="runbook-input-and-output"></a>Runbook giriş ve çıkış

### <a name="runbook-input"></a>Runbook giriş

Geçerli bir alt öğesi olarak kullanılıyorsa, giriş, Azure portalı üzerinden bir runbook başlattığınızda bir kullanıcı ya da başka bir runbook'tan bir runbook'u gerektirebilir.
Örneğin, bir sanal makine oluşturan bir runbook'unuz varsa, runbook'u başlatmak gibi bilgiler adı sanal makine ve diğer özellikleri her zaman sağlamanız gerekebilir.

Bir tanımlayarak bir runbook için giriş kabul edin veya daha fazla giriş parametreleri. Runbook her başlatıldığında bu parametreler için değerler sağlayın. Azure portalıyla bir runbook'u başlattığınızda her runbook'un giriş parametreleri için değerleri girmenizi ister.

Bir runbook'un giriş parametreleri tıklayarak erişebilirsiniz **giriş ve çıkış** runbook araç çubuğunda.

Bu açılır **giriş ve çıkış** nerede var olan bir giriş parametresi düzenleyebilir veya tıklayarak yeni bir denetim **Girişi Ekle**.

![Girişi Ekle](media/automation-graphical-authoring-intro/runbook-edit-add-input.png)

Her giriş parametresi, aşağıdaki tabloda özellikleri tarafından tanımlanır:

| Özellik | Description |
|:--- |:--- |
| Name |Parametrenin benzersiz adı. Bu, yalnızca alfa sayısal karakterler içerebilir ve boşluk içeremez. |
| Description |Giriş parametresi için isteğe bağlı bir açıklama. |
| Type |Veri türü için parametre değeri bekleniyor. Azure portalında uygun bir denetim için her parametresinin veri türü için giriş isterken sağlar. |
| Mandatory |Parametresi için bir değer sağlanmalıdır olup olmadığını belirtir. Tanımlanan varsayılan değeri olmayan zorunlu her parametre için bir değer belirtmezseniz, runbook başlatılamıyor. |
| Default Value |Bir sağlanmazsa, parametresi için hangi değerin kullanıldığını belirtir. Bu Null ya da belirli bir değer olabilir. |

### <a name="runbook-output"></a>Runbook çıkışı

Giden bir bağlantıya sahip olmayan herhangi bir etkinliği tarafından oluşturulan veriler için kaydedildiğinde [runbook'un çıktı](https://docs.microsoft.com/azure/automation/automation-runbook-output-and-messages). Çıkış, runbook işi ile kaydedilir ve runbook bir alt öğesi olarak kullanıldığında, bir üst runbook için kullanılabilir.

## <a name="powershell-expressions"></a>PowerShell ifadeleri

Grafik yazma avantajlarından biri, PowerShell ilişkin minimum bilgi ile bir runbook oluşturma olanağı sunuyor. Şu anda, ancak belirli doldurmak için biraz PowerShell bilmeniz gerekmez [parametre değerlerini](#activities) ve ayarın [bağlantı koşulları](#links-and-workflow). Bu bölümde PowerShell ifadeleri ile tanıdık olmayabilir kullanıcılar için hızlı bir giriş sağlar. PowerShell tam ayrıntıları şurada bulunabilir [Windows PowerShell ile betik oluşturma](https://technet.microsoft.com/library/bb978526.aspx).

### <a name="powershell-expression-data-source"></a>PowerShell ifadesi veri kaynağı
Değerini doldurmak için veri kaynağı olarak bir PowerShell ifadesi kullanabilirsiniz bir [Etkinlik parametresi](#activities) sonuçlarıyla ilgili bazı PowerShell kodu. Bu, tek satırlık bir bazı basit bir işlevi veya karmaşık bir mantık gerçekleştiren çok satırlı gerçekleştiren kod olabilir. Herhangi bir değişkene atanmış bir komutun çıktısını parametre değeri çıkışı yapılır.

Örneğin, aşağıdaki komutu, geçerli tarihi çıkış.

Örneğin, aşağıdaki komutu, geçerli tarihi çıkış.

```powershell-interactive
Get-Date
```

Aşağıdaki komut, geçerli tarihten itibaren bir dize oluşturmak ve bir değişkene atayın. Değişken içeriğini ardından çıkışa gönderilir

```powershell-interactive
$string = "The current date is " + (Get-Date)
$string
```

Aşağıdaki komutlar, geçerli tarihi değerlendirin ve geçerli gün, hafta veya haftanın günü olup olmadığını gösteren bir dize döndürür.

```powershell-interactive
$date = Get-Date
if (($date.DayOfWeek = "Saturday") -or ($date.DayOfWeek = "Sunday")) { "Weekend" }
else { "Weekday" }
```

### <a name="activity-output"></a>Etkinlik çıkışı

Bir önceki etkinliğin çıkışı runbook'ta kullanmak için şu sözdizimini $ActivityOutput değişkeni kullanın.

```powershell-interactive
$ActivityOutput['Activity Label'].PropertyName
```

Örneğin, bir sanal makinenin adını gerektiren özelliğine sahip bir etkinlik olabilir. Bu durumda, aşağıdaki ifade kullanabilirsiniz:

```powershell-interactive
$ActivityOutput['Get-AzureVm'].Name
```

Ardından bir özelliği yerine sanal makinesi için gerekli özellik nesne, aşağıdaki sözdizimini kullanarak tüm nesnesi döndürür.

```powershell-interactive
$ActivityOutput['Get-AzureVm']
```

Bir etkinliğin çıktısını, sanal makine adı için metin birleştiren aşağıdaki gibi daha karmaşık bir ifadede de kullanabilirsiniz.

```powershell-interactive
"The computer name is " + $ActivityOutput['Get-AzureVm'].Name
```

### <a name="conditions"></a>Koşullar

Kullanım [Karşılaştırma işleçleri](https://technet.microsoft.com/library/hh847759.aspx) değerleri karşılaştırmak veya değer belirtilen desenle eşleşip eşleşmediğini belirlemek için. Bir karşılaştırma $true veya $false değerini döndürür.

Örneğin, aşağıdaki iki koşul etkinlik sanal makineden adlı olup olmadığını belirler. *Get-AzureVM* şu anda *durduruldu*.

```powershell-interactive
$ActivityOutput["Get-AzureVM"].PowerState –eq "Stopped"
```

Aynı sanal makinenin dışında herhangi bir durumda olup olmadığını denetler aşağıdaki koşul *durduruldu*.

```powershell-interactive
$ActivityOutput["Get-AzureVM"].PowerState –ne "Stopped"
```

Birden çok koşulu kullanarak katılabilirsiniz bir [mantıksal işleç](https://technet.microsoft.com/library/hh847789.aspx) gibi **- ve** veya **- veya**. Örneğin, önceki örnekte aynı sanal makineye bir durumda olup olmadığını aşağıdaki denetimleri koşul *durduruldu* veya *durdurma*.

```powershell-interactive
($ActivityOutput["Get-AzureVM"].PowerState –eq "Stopped") -or ($ActivityOutput["Get-AzureVM"].PowerState –eq "Stopping")
```

### <a name="hashtables"></a>Hashtable'da

[Hashtable'da](https://technet.microsoft.com/library/hh847780.aspx) , bir dizi döndürmek için yararlı olan ad/değer çiftleridir. Özellik belirli etkinlikler için basit bir değer yerine bir hashtable bekleyebilir. Bir sözlük olarak başvurulan bir karma tablosu olarak görebilirsiniz.

Aşağıdaki sözdizimi ile karma tablosu oluşturun. Bir karma tablo girişleri herhangi bir sayıda içerebilir, ancak her bir ad ve değer tarafından tanımlanır.

```powershell-interactive
@{ <name> = <value>; [<name> = <value> ] ...}
```

Örneğin, aşağıdaki ifade, veri kaynağında bir hashtable için bir internet araması değerlerle beklenen bir etkinlik parametresi için kullanılan bir karma tablo oluşturur.

```powershell-interactive
$query = "Azure Automation"
$count = 10
$h = @{'q'=$query; 'lr'='lang_ja';  'count'=$Count}
$h
```

Aşağıdaki örnekte adlı bir etkinliğin çıktısını *Twitter bağlantısını Al* bir hashtable doldurmak için.

```powershell-interactive
@{'ApiKey'=$ActivityOutput['Get Twitter Connection'].ConsumerAPIKey;
    'ApiSecret'=$ActivityOutput['Get Twitter Connection'].ConsumerAPISecret;
    'AccessToken'=$ActivityOutput['Get Twitter Connection'].AccessToken;
    'AccessTokenSecret'=$ActivityOutput['Get Twitter Connection'].AccessTokenSecret}
```

## <a name="next-steps"></a>Sonraki Adımlar

* PowerShell iş akışı runbook'larını kullanmaya başlamak için bkz. [İlk PowerShell iş akışı runbook uygulamam](automation-first-runbook-textual.md)
* Grafik runbook'ları kullanmaya başlamak için bkz. [İlk grafik runbook uygulamam](automation-first-runbook-graphical.md)
* Runbook türleri, avantajları ve sınırlamaları hakkında daha fazla bilgi için bkz. [Azure Automation runbook türleri](automation-runbook-types.md)
* Otomasyon farklı çalıştır hesabını kullanarak kimlik doğrulaması yapmayı anlamak için bkz: [yapılandırma Azure farklı çalıştır hesabı](automation-sec-configure-azure-runas-account.md)
