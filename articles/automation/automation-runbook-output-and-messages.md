---
title: Runbook çıkışı ve iletileri Azure automation'da
description: Desribes oluşturmak ve çıktı ve hata almak nasıl Azure automation'daki runbook'lar gelen iletiler.
services: automation
ms.service: automation
ms.subservice: process-automation
author: bobbytreed
ms.author: robreed
ms.date: 12/04/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 27dd9888d83e01ea522b2532fc1d65284f2fe8d1
ms.sourcegitcommit: f811238c0d732deb1f0892fe7a20a26c993bc4fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/29/2019
ms.locfileid: "67476935"
---
# <a name="runbook-output-and-messages-in-azure-automation"></a>Runbook çıkışı ve iletileri Azure Otomasyonu
Çoğu Azure Automation runbook'ları, bir çıkış biçimine sahip. Bu çıkış, bir hata iletisi, başka bir runbook ile kullanmak istediğiniz kullanıcı veya karmaşık bir nesne olabilir. Windows PowerShell sağlar [birden çok akış](/powershell/module/microsoft.powershell.core/about/about_redirection) bir betik veya iş akışı, çıkış göndermek için. Azure Otomasyonu bu akışları her biriyle farklı biçimde çalışır. Bir runbook oluştururken her kullanmak için en iyi uygulamaları izlemelisiniz.

Aşağıdaki tabloda her akışları ve yayımlanan runbook'lar için Azure Portalı'ndaki davranışları kısa bir açıklamasını sağlar ve ne zaman [bir runbook'u test etme](automation-testing-runbook.md). Her akış hakkındaki ek ayrıntılar sonraki bölümlerde verilmiştir.

| Akış | Açıklama | Yayımlanmış | Test |
|:--- |:--- |:--- |:--- |
| Çıkış |Diğer runbook'lar tarafından kullanılması amaçlanan nesneler. |İş geçmişine yazılır. |Test Çıkış Bölmesi'nde görüntülenir. |
| Uyarı |Kullanıcıya yönelik uyarı iletisi. |İş geçmişine yazılır. |Test Çıkış Bölmesi'nde görüntülenir. |
| Hata |Kullanıcıya yönelik hata iletisi. Bir özel durumun aksine, runbook varsayılan olarak bir hata iletisinden sonra devam eder. |İş geçmişine yazılır. |Test Çıkış Bölmesi'nde görüntülenir. |
| Ayrıntılı |Genel ya da hata ayıklama bilgileri vermek iletileri. |Yalnızca runbook için ayrıntılı günlük kaydı açıksa iş geçmişine yazılır. |$VerbosePreference runbook'ta devam et yalnızca ayarlanırsa, Test çıkış Bölmesi'nde görüntülenir. |
| İlerleme durumu |Runbook'taki her etkinlikten önce ve sonra otomatik oluşturulan kayıtlar. Runbook kendi ilerleme durumu kayıtlarını etkileşimli bir kullanıcı için hedeflenen oluşturmanız çalışmaması gerekir. |Yalnızca runbook için ilerleme durumu günlük kaydı açıksa iş geçmişine yazılır. |Test Çıkış Bölmesi'nde görüntülenmez. |
| Hata ayıklama |Etkileşimli bir kullanıcıya yönelik iletiler. Runbook'larda kullanılmamalıdır. |İş geçmişine yazılmaz. |Test Çıkış Bölmesi'ne yazılmaz. |

## <a name="output-stream"></a>Çıkış akışı
Çıkış akışına, doğru çalıştığı zaman, bir komut dosyası ya da iş akışı tarafından oluşturulan nesnelerin çıkışı için tasarlanmıştır. Azure Otomasyonu'nda Bu akış öncelikle tarafından tüketilmesi amaçlanan nesneler için kullanılır [runbook'ları geçerli runbook'u çağıran üst](automation-child-runbooks.md). Üst runbook'tan [runbook'u satır içi olarak çağırdığınızda](automation-child-runbooks.md#invoking-a-child-runbook-using-inline-execution) , üst öğeye çıkış akışından verileri döndürür. Yalnızca runbook'un hiçbir zaman başka bir runbook tarafından çağrılır biliyorsanız geri kullanıcıya genel bilgiler iletmek için çıkış akışı kullanın. Ancak, en iyi uygulama olarak kullanıcıya genel bilgiler iletmek için [Verbose Stream](#verbose-stream) kullanmalısınız.

[Write-Output](https://technet.microsoft.com/library/hh849921.aspx) kullanarak veya nesneyi runbook'ta kendi satırına koyarak çıkış akışına veri yazabilirsiniz.

```powershell
#The following lines both write an object to the output stream.
Write-Output –InputObject $object
$object
```

### <a name="output-from-a-function"></a>Bir işlevden çıkış
Runbook'unuza dahil bir işlevde çıkış akışına yazdığınızda, çıkış runbook'a geri gönderilir. Runbook bu çıkışı bir değişkene atarsa, çıkış akışına yazılmaz. Diğer akışlara işlevi içinde yazmak runbook için buna karşılık gelen akışa yazar.

Aşağıdaki örnek runbook'u göz önünde bulundurun:

```powershell
Workflow Test-Runbook
{
  Write-Verbose "Verbose outside of function" -Verbose
  Write-Output "Output outside of function"
  $functionOutput = Test-Function
  $functionOutput

  Function Test-Function
  {
    Write-Verbose "Verbose inside of function" -Verbose
    Write-Output "Output inside of function"
  }
}
```

Runbook işi için çıkış akışı:

```output
Output inside of function
Output outside of function
```

Runbook işi için ayrıntılı akış:

```output
Verbose outside of function
Verbose inside of function
```

Runbook yayımladıktan sonra ve, başlamadan önce de ayrıntılı Verbose stream çıktısını almak için runbook ayarlarında günlüğe yazmayı etkinleştirmeniz gerekir.

### <a name="declaring-output-data-type"></a>Bildirim çıkış veri türü
Bir iş akışı [OutputType özniteliğini](https://technet.microsoft.com/library/hh847785.aspx)kullanarak çıkışının veri türünü belirtebilir. Bu özniteliğin çalışma zamanı sırasında bir etkisi yoktur, ancak tasarım zamanında runbook yazarına runbook'un beklenen çıkışı hakkında bir gösterge sağlar. Runbook'lar için araç kümesi gelişmeye devam ettikçe, çıkış veri türlerinin tasarım zamanında bildirilmesinin önemi önemi artırır. Sonuç olarak, oluşturduğunuz tüm runbook'lara bu bildirimi dahil en iyi uygulamadır.

Çıktı türleri örnek listesi aşağıdadır:

* System.String
* System.Int32
* System.Collections.Hashtable
* Microsoft.Azure.Commands.Compute.Models.PSVirtualMachine

Aşağıdaki örnek runbook bir dize nesnesi çıkışı yapar ve çıkış türü bildirimini içerir. Runbook'unuz belirli bir türde dizi çıkışı yapıyorsa, yine de o türdeki dizi yerine, türü belirtmelisiniz.

```powershell
Workflow Test-Runbook
{
  [OutputType([string])]

  $output = "This is some string output."
  Write-Output $output
}
 ```

Grafik veya grafik PowerShell iş akışı runbook'ları, bir çıkış türü bildirmek için seçebileceğiniz **giriş ve çıkış** menü seçeneği ve çıkış türü adını yazın. Tam .NET sınıf adı, kolayca tanımlanabilen üst runbook'tan başvuru yapmak için kullanmanız önerilir. Bu, runbook'taki veri yolu için bu sınıfın tüm özellikleri gösterir ve bunları koşullu mantığı, günlüğe kaydetme ve runbook'taki diğer etkinlikler için değerler olarak başvuran kullanırken çok esneklik sağlar.<br> ![Runbook giriş ve çıkış seçeneği](media/automation-runbook-output-and-messages/runbook-menu-input-and-output-option.png)

Aşağıdaki örnekte, bu özellik göstermek için iki grafik runbook'lar sahip. Modüler bir runbook tasarım modeli uygularsanız, görevi gören bir runbook sahip *kimlik doğrulaması Runbook şablonu* farklı çalıştır hesabını kullanarak Azure ile kimlik doğrulamasını yönetme. Normalde, belirli bir senaryo otomatik hale getirmek için çekirdek mantığını gerçekleştirecek, bizim ikinci runbook yürütmek için bu durumda geçiyor *kimlik doğrulaması Runbook şablonu* ve sonuçları görüntülemek, **Test** Çıkış bölmesi. Normal koşullar altında bu runbook alt runbook'un çıktısını yararlanarak bir kaynağa karşı bir şey yoktur.

Temel mantığı işte **AuthenticateTo Azure** runbook.<br> ![Runbook şablonu örnek kimliğini](media/automation-runbook-output-and-messages/runbook-authentication-template.png).  

Çıktı türü içeren *Microsoft.Azure.Commands.Profile.Models.PSAzureContext*, profil özelliklerini kimlik doğrulaması döndürür.<br> ![Runbook'u çıktı türü örneği](media/automation-runbook-output-and-messages/runbook-input-and-output-add-blade.png) 

Bu runbook oldukça kolaydır ancak burada bir yapılandırma öğesini yoktur. Son Etkinlik yürütme **Write-Output** cmdlet'i ve profil verileri için bir PowerShell ifadesi kullanarak $_ değişkeni yazan **Inputobject** parametresini, bu cmdlet'i için gereklidir.  

Bu örnekte ikinci runbook için adlı *Test ChildOutputType*, yalnızca iki etkinlik vardır.<br> ![Örnek alt türü Runbook çıkış](media/automation-runbook-output-and-messages/runbook-display-authentication-results-example.png) 

İlk etkinlik çağrıları **AuthenticateTo Azure** runbook ve ikinci etkinlik çalışıyor **Write-Verbose** cmdlet'iyle **veri kaynağı** ,  **Etkinlik çıkışı** ve değeri **alan yolu** olduğu **Context.Subscription.SubscriptionName**, bağlam çıkışını belirtme  **Azure AuthenticateTo** runbook.<br> ![Write-Verbose cmdlet'i parametre veri kaynağı](media/automation-runbook-output-and-messages/runbook-write-verbose-parameters-config.png)    

Elde edilen çıkış abonelik adıdır.<br> ![Test-ChildOutputType Runbook sonuçları](media/automation-runbook-output-and-messages/runbook-test-childoutputtype-results.png)

Çıkış türü denetiminin davranışını hakkında bir not. Çıkış türü alanına giriş ve çıkış özellikleri dikey penceresinde bir değer yazdığınızda, denetim tarafından tanınması giriş sırayla, yazdıktan sonra denetimi dışında tıklatın gerekir.  

## <a name="message-streams"></a>İleti akışları
Çıkış akışından farklı olarak, ileti akışlarının kullanıcıya bilgi iletmesi amaçlanır. Farklı türde bilgileri için birden çok ileti akışı vardır ve her Azure Otomasyonu tarafından farklı şekilde ele alınır.

### <a name="warning-and-error-streams"></a>Uyarı ve hata akışları
Uyarı ve Hata akışlarının bir runbook'ta oluşan sorunları günlüğe kaydetmesi amaçlanır. Bir runbook yürütülür ve bir runbook test edildiğinde Test çıkış Bölmesi'nde Azure portalı içinde yer bunlar iş geçmişine yazılır. Varsayılan olarak, runbook bir uyarı veya hatadan sonra yürütülmeye devam eder. İletiyi oluşturmadan önce runbook'ta bir [tercih değişkeni](#preference-variables) ayarlayarak bir uyarı veya hatada runbook'un askıya alınması gerektiğini belirtebilirsiniz. Örneğin, bir runbook'un bir hatayla bir özel durumda olduğu gibi askıya almak için ayarlanmış **$ErrorActionPreference** durdurma için.

[Write-Warning](https://technet.microsoft.com/library/hh849931.aspx) veya [Write-Error](https://technet.microsoft.com/library/hh849962.aspx) cmdlet'ini kullanarak bir uyarı veya hata iletisi oluşturun. Etkinlikler de bu akışlara yazabilir.

```powershell
#The following lines create a warning message and then an error message that will suspend the runbook.

$ErrorActionPreference = "Stop"
Write-Warning –Message "This is a warning message."
Write-Error –Message "This is an error message that will stop the runbook because of the preference variable."
```

### <a name="verbose-stream"></a>Verbose stream
Ayrıntılı ileti akışı, runbook işlemi hakkında genel bilgiler için kullanılır. Bu yana [hata ayıklama Stream](#debug-stream) kullanılabilir olmayan bir runbook'ta hata ayıklama bilgileri için ayrıntılı iletiler kullanılmalıdır. Varsayılan olarak, yayımlanan runbook'lardan ayrıntılı iletiler iş geçmişinde depolanmaz. Ayrıntılı iletileri depolamak için Azure Portalı'nda runbook'un yapılandırma sekmesinde ayrıntılı kayıtları günlüğe yayımlanan runbook'ları yapılandırın. Çoğu durumda, performans nedenleriyle runbook için ayrıntılı kayıtları günlüğe kaydetmeme varsayılan ayarını korumalısınız. Bu seçeneği yalnızca sorun gidermek veya bir runbook'ta hata ayıklamak için açın.

Zaman [bir runbook'u test etme](automation-testing-runbook.md), runbook ayrıntılı kayıtları günlüğe kaydetmek üzere yapılandırılmış bile olsa ayrıntılı iletiler görüntülenmiyor. Ederken ayrıntılı iletileri görüntülemek için [bir runbook'u test etme](automation-testing-runbook.md), devam et $VerbosePreference değişkenini ayarlamalıdır. Bu değişken ayarlandığında, ayrıntılı iletiler, Azure portalında Test çıkışı bölmesinde görüntülenir.

[Write-Verbose](https://technet.microsoft.com/library/hh849951.aspx) cmdlet'ini kullanarak bir ayrıntılı ileti oluşturun.

```powershell
#The following line creates a verbose message.

Write-Verbose –Message "This is a verbose message."
```

### <a name="debug-stream"></a>Hata ayıklama akışı
Hata Ayıklama akışının etkileşimli kullanıcı tarafından kullanılması amaçlanır ve runbook'larda kullanılmamalıdır.

## <a name="progress-records"></a>İlerleme durumu kayıtlarını
Yapılandırırsanız (Azure Portalı'nda runbook'un yapılandırma sekmesinde), bir runbook'u ilerleme durumu günlük kayıtları ve önce ve her etkinliğin çalıştıktan sonra iş geçmişine bir kayıt yazılır. Çoğu durumda, performansı en üst düzeye çıkarmak için ilerleme durumu kayıtlarını günlüğe kaydetmeme varsayılan ayarını korumalısınız. Bu seçeneği yalnızca sorun gidermek veya bir runbook'ta hata ayıklamak için açın. Bir runbook'u test ederken, runbook ilerleme kaydı kayıtlarını günlüğe kaydetmek üzere yapılandırılmış bile olsa ilerleme durumu iletileri görüntülenmez.

[Write-Progress](https://technet.microsoft.com/library/hh849902.aspx) cmdlet bir runbook'ta geçerli değildir Bu cmdlet etkileşimli bir kullanıcıyla kullanılmak üzere tasarlanmıştır.

## <a name="preference-variables"></a>Tercih değişkenleri
Windows PowerShell farklı çıkış akışlarına gönderilen verilerin nasıl yanıtlanacağını belirlemek için [tercih değişkenleri](https://technet.microsoft.com/library/hh847796.aspx) kullanır. Bir runbook'ta nasıl farklı akışlara gönderilen verilerin yanıt denetlemek için bu değişkenleri ayarlayabilirsiniz.

Aşağıdaki tabloda runbook'larda kullanılabilecek tercih değişkenleriyle geçerli ve varsayılan değerleri listelenmektedir. Bu tabloda yalnızca bir runbook'ta geçerli olan değerleri içerir. Windows PowerShell ile Azure Otomasyonu dışında kullanıldığında tercih değişkenleri için ek değerler geçerlidir.

| Değişken | Varsayılan Değer | Geçerli Değerler |
|:--- |:--- |:--- |
| WarningPreference |Devam et |Durdur<br>Devam et<br>SilentlyContinue |
| ErrorActionPreference |Devam et |Durdur<br>Devam et<br>SilentlyContinue |
| VerbosePreference |SilentlyContinue |Durdur<br>Devam et<br>SilentlyContinue |

Aşağıdaki tabloda runbook'larda geçerli olan tercih değişkeni değerleri için davranışlar listelenmektedir.

| Değer | Davranış |
|:--- |:--- |
| Devam et |İletiyi günlüğe kaydeder ve runbook'u yürütmeye devam eder. |
| SilentlyContinue |İletiyi günlüğe kaydetmeden runbook'u yürütmeye devam eder. Bu değer, iletiyi yoksaymakla etkisi vardır. |
| Durdur |İletiyi günlüğe kaydeder ve runbook'u askıya alır. |

## <a name="runbook-output"></a>Runbook çıkışlarını ve iletilerini alma
### <a name="azure-portal"></a>Azure portal
Bir runbook işinin ayrıntılarını Azure portalında bir runbook'un işler sekmesinden görüntüleyebilirsiniz. İş özetinde giriş parametreleri görüntüler ve [çıkış Stream](#output-stream) işi ve bunlar oluştuysa hakkındaki genel bilgileri. Gelen iletileri geçmişini içeren [çıkış Stream](#output-stream) ve [uyarı ve hata akışları](#warning-and-error-streams) yanı sıra [Verbose Stream](#verbose-stream) ve [ilerleme durumu kayıtlarını](#progress-records) runbook günlük ayrıntılı kayıtlarla ilerleme durumu kayıtlarını için yapılandırılmışsa.

### <a name="windows-powershell"></a>Windows PowerShell
Windows PowerShell'de, çıkışı ve iletileri kullanarak bir runbook'tan alabileceğiniz [Get-AzureAutomationJobOutput](https://docs.microsoft.com/powershell/module/servicemanagement/azure/get-azureautomationjoboutput) cmdlet'i. Bu cmdlet iş Kimliğini gerektirir ve hangi akışın döndürüleceğini belirttiğiniz Stream adlı bir parametreye sahiptir. Bir işin tüm akışlarını döndürmek için **Tümü** değerini belirtebilirsiniz.

Aşağıdaki örnek, örnek bir runbook başlatır ve tamamlanmasını bekler. Tamamlandığında, çıkış akışı işten toplanır.

```powershell
$job = Start-AzureRmAutomationRunbook -ResourceGroupName "ResourceGroup01" `
  –AutomationAccountName "MyAutomationAccount" –Name "Test-Runbook"

$doLoop = $true
While ($doLoop) {
  $job = Get-AzureRmAutomationJob -ResourceGroupName "ResourceGroup01" `
    –AutomationAccountName "MyAutomationAccount" -Id $job.JobId
  $status = $job.Status
  $doLoop = (($status -ne "Completed") -and ($status -ne "Failed") -and ($status -ne "Suspended") -and ($status -ne "Stopped"))
}

Get-AzureRmAutomationJobOutput -ResourceGroupName "ResourceGroup01" `
  –AutomationAccountName "MyAutomationAccount" -Id $job.JobId –Stream Output

# For more detailed job output, pipe the output of Get-AzureRmAutomationJobOutput to Get-AzureRmAutomationJobOutputRecord
Get-AzureRmAutomationJobOutput -ResourceGroupName "ResourceGroup01" `
  –AutomationAccountName "MyAutomationAccount" -Id $job.JobId –Stream Any | Get-AzureRmAutomationJobOutputRecord
``` 

### <a name="graphical-authoring"></a>Grafik yazma
Grafik runbook'ları için fazladan günlük kaydı etkinlik düzeyinde izleme biçiminde kullanıma sunulmuştur. İzlemenin iki düzeyi vardır: Temel ve ayrıntılı. Temel izleme runbook yanı sıra herhangi bir etkinlik yeniden deneme için ilgili bilgiler her etkinliğin başlangıç ve bitiş zamanı görebilirsiniz. Bazı örnekler olan, deneme sayısı ve başlangıç saati etkinliği. Ayrıntılı izleme, her etkinlik için temel izleme artı giriş ve çıkış veri alın. Şu anda izleme kayıtları izlemeyi etkinleştirdiğinizde, ayrıntılı günlük kaydı etkinleştirmeniz gerekir verbose stream kullanarak yazılır. Etkin izleme ile grafik runbook'lar için ilerleme durumu kayıtlarını günlüğe kaydetmek için gerek yoktur. Temel izleme aynı amaca hizmet eder ve daha bilgilendirici.

![Görünümü grafik yazma iş akışları](media/automation-runbook-output-and-messages/job-streams-view-blade.png)

Önceki ekran görüntüsünden çok daha fazla bilgi, ayrıntılı günlük kaydı ve grafik runbook'lar için izlemeyi etkinleştirdiğinizde, iş akışları görünümü üretimde kullanılabilir olduğunu görebilirsiniz. Bu ek bilgiler bir runbook ile üretim sorunlarını gidermek için gerekli olabilir ve bu nedenle yalnızca genel bir uygulama değil de, bu amaç için etkinleştirmeniz. İzleme kayıtları, özellikle çok sayıda olabilir. Grafik runbook izleme ile etkinlik basit veya ayrıntılı izleme mi yapılandırdığınıza bağlı olarak her iki ila dört kayıt alabilirsiniz. Sorun giderme için runbook ilerlemesini izlemek için bu bilgileri gerekmiyorsa, izleme tutmak kapalı isteyebilirsiniz.

**Etkinlik düzeyinde izlemeyi etkinleştirmek için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, Otomasyon hesabınızı açın.
2. Altında **süreç otomasyonu**seçin **runbook'ları** runbook'ların listesini açmak için.
3. Runbook'ları sayfasında, bir grafik runbook'ların listenizden seçmek için tıklayın.
4. Altında **ayarları**, tıklayın **günlüğe kaydetme ve izleme**.
5. Günlüğe kaydetme ve izleme sayfası, ayrıntılı kayıtları günlüğe tıklayın **üzerinde** ayrıntılı günlüğe yazmayı etkinleştir ve etkinlik düzeyinde izleme altında için izleme seviyesini değiştirmek için **temel** veya **ayrıntılı** ihtiyaç duyduğunuz izleme düzeyini temel alan.<br>
   
   ![Grafik yazma günlüğe kaydetme ve izleme sayfası](media/automation-runbook-output-and-messages/logging-and-tracing-settings-blade.png)

### <a name="microsoft-azure-monitor-logs"></a>Microsoft Azure izleme günlükleri
Log Analytics çalışma alanınıza Automation runbook iş durumunu ve iş akışları yeniden gönderebilirsiniz. Azure İzleyici günlüklerine ile şunları yapabilirsiniz:

* Otomasyon işleriniz hakkında bilgi edinme 
* Bir e-posta veya uyarı (örneğin, başarısız olan veya askıya alınmış), runbook işi durumuna dayalı tetikleyici 
* İş akışlarınız arasında gelişmiş sorguları yazma 
* Otomasyon hesapları arasında işleri ilişkilendirin 
* Zaman içinde iş geçmişinizi görselleştirin    

Toplamanıza, ilişkilendirmenize ve işi veri üzerinde oynama yapmak için Azure İzleyici günlükleri ile tümleştirmeyi yapılandırma hakkında daha fazla bilgi için bkz. [Otomasyon iş durumunu ve iş akışları için Azure İzleyici günlüklerine iletmek](automation-manage-send-joblogs-log-analytics.md).

## <a name="next-steps"></a>Sonraki adımlar
* Runbook yürütme, runbook işlerini izleme ve diğer teknik ayrıntılar hakkında daha fazla bilgi edinmek için bkz. [Runbook işi izleme](automation-runbook-execution.md)
* Tasarım ve alt runbook'ları kullanma hakkında bilgi almak için bkz: [Azure automation'da alt runbook'lar](automation-child-runbooks.md)


