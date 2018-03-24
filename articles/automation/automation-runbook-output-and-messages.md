---
title: Runbook çıkışı ve iletileri Azure automation'da
description: Azure Automation runbook'lardan nasıl oluşturulacağını ve çıkış ve hata iletileri Desribes.
services: automation
ms.service: automation
author: georgewallace
ms.author: gwallace
ms.date: 03/16/2018
ms.topic: article
manager: carmonm
ms.openlocfilehash: d4b8d485906701b4f05e057996bc31232a29e620
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/23/2018
---
# <a name="runbook-output-and-messages-in-azure-automation"></a>Runbook çıkışı ve iletileri Azure Automation
Çoğu Azure Automation runbook kullanıcıya bir hata iletisi gibi bir çıkış sahiptir veya karmaşık bir nesne başka bir iş akışı tarafından kullanılması amaçlanan. Windows PowerShell sağlar [birden çok akış](http://blogs.technet.com/heyscriptingguy/archive/2014/03/30/understanding-streams-redirection-and-write-host-in-powershell.aspx) bir komut dosyası veya iş akışı çıkış göndermek için. Azure Otomasyonu her bu akışları farklı şekilde çalışır ve bir runbook oluştururken her kullanımı için en iyi uygulamaları izlemelisiniz.

Aşağıdaki tabloda hem yayımlanan bir runbook çalıştırılırken hem de her akışlar ve Azure Portalı'ndaki davranışları kısa bir açıklaması verilmiştir [bir runbook'u test](automation-testing-runbook.md). Her akış hakkında ek ayrıntılar sonraki bölümlerde verilmiştir.

| Akış | Açıklama | Yayımlanma | Test etme |
|:--- |:--- |:--- |:--- |
| Çıktı |Diğer runbook'lar tarafından kullanılması amaçlanan nesneler. |İş geçmişine yazılır. |Test çıkış Bölmesi'nde görüntülenir. |
| Uyarı |Kullanıcıya yönelik uyarı iletisi. |İş geçmişine yazılır. |Test çıkış Bölmesi'nde görüntülenir. |
| Hata |Kullanıcıya yönelik hata iletisi. Bir özel durumun aksine, runbook varsayılan olarak bir hata iletisinden sonra devam eder. |İş geçmişine yazılır. |Test çıkış Bölmesi'nde görüntülenir. |
| Ayrıntılı |Genel ya da hata ayıklama bilgileri sağlayan iletiler. |Yalnızca runbook için ayrıntılı günlük kaydı etkin değilse iş geçmişine yazılır. |Yalnızca $VerbosePreference devam et runbook'ta ayarlandıysa Test çıkışı bölmesinde görüntülenir. |
| İlerleme Durumu |Önce ve sonra runbook'taki her etkinlik otomatik olarak oluşturulan kayıtlar. Runbook için etkileşimli bir kullanıcı amaçlandığından kendi ilerleme durumu kayıtlarını oluşturmaya çalışmamalıdır. |Yalnızca runbook için ilerleme durumu günlük kaydı etkin değilse iş geçmişine yazılır. |Test çıkış Bölmesi'nde görüntülenmez. |
| Hata ayıklama |Etkileşimli bir kullanıcıya yönelik iletiler. Runbook'larda kullanılmamalıdır. |İş geçmişine yazılmaz. |Test çıkış Bölmesi'ne yazılmaz. |

## <a name="output-stream"></a>Çıkış akışı
Çıkış akışı, doğru çalıştığı zaman bir komut dosyası veya iş akışı tarafından oluşturulan nesnelerin çıkışı için tasarlanmıştır. Azure Otomasyonu'nda Bu akış öncelikle tarafından kullanılması amaçlanan nesneler için kullanılan [üst geçerli runbook'u çağıran runbook'ları](automation-child-runbooks.md). Olduğunda, [bir runbook'u satır içi olarak çağırdığınızda](automation-child-runbooks.md#invoking-a-child-runbook-using-inline-execution) üst runbook'tan, veri çıkış akışından üst öğeye döndürür. Yalnızca runbook'un hiçbir zaman başka bir runbook tarafından çağrılır biliyorsanız geri kullanıcıya genel bilgiler iletişim kurmak için çıkış akışı kullanmanız gerekir. En iyi uygulama, ancak genellikle kullanmanız [ayrıntılı akış](#Verbose) kullanıcıya genel bilgiler iletişim kurmak için.

Çıkış akışı kullanan veri yazabilirsiniz [Write-Output](http://technet.microsoft.com/library/hh849921.aspx) veya nesneyi runbook'ta kendi satırına koyarak kullanılabilir.

    #The following lines both write an object to the output stream.
    Write-Output –InputObject $object
    $object

### <a name="output-from-a-function"></a>Bir işlevden çıkış
Runbook'unuza dahil edilen bir işlevde çıkış akışına yazdığınızda, çıkış runbook'a geri gönderilir. Ardından runbook bu çıkışı bir değişkene atarsa, çıkış akışına yazılmaz. Diğer akışlara işlevi içinde yazmak runbook için buna karşılık gelen akışa yazar.

Aşağıdaki örnek runbook'u göz önünde bulundurun:

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


Runbook işi için çıkış akışı olacaktır:

    Output inside of function
    Output outside of function

Runbook işi için ayrıntılı akış olacaktır:

    Verbose outside of function
    Verbose inside of function

Runbook yayımlandıktan sonra ve, başlamadan önce ayrıca runbook ayarlarında ayrıntılı akış çıkış almak amacıyla ayrıntılı günlüğe yazmayı etkinleştirmeniz gerekir.

### <a name="declaring-output-data-type"></a>Bildiren çıktı veri türü
Bir iş akışı kullanarak çıkışının veri türünü belirtebilirsiniz [OutputType özniteliğini](http://technet.microsoft.com/library/hh847785.aspx). Bu özniteliğin çalışma zamanı sırasında herhangi bir etkisi olmaz ancak runbook yazarına runbook'un beklenen çıkışı tasarım zamanında bir gösterge sağlar. Runbook'lar için araç kümesi gelişmeye devam ettikçe, çıkış veri türlerinin tasarım zamanında bildirilmesinin önemi önemi artırır. Sonuç olarak, oluşturduğunuz tüm runbook'lara bu bildirimi dahil en iyi uygulamadır.

Çıktı türleri örnek listesi aşağıdadır:

* System.String
* System.Int32
* System.Collections.Hashtable
* Microsoft.Azure.Commands.Compute.Models.PSVirtualMachine

Aşağıdaki örnek runbook bir dize nesnesi çıkışı yapar ve çıkış türü bildirimini içerir. Ardından runbook'unuz belirli bir türde dizi çıkışı yapıyorsa türün bir dizi aksine türü belirtmelisiniz.

    Workflow Test-Runbook
    {
       [OutputType([string])]

       $output = "This is some string output."
       Write-Output $output
    }

Grafik veya grafik PowerShell iş akışı runbook'ları bir çıktı türünün bildirmek için seçebileceğiniz **giriş ve çıkış** menü seçeneğini ve çıkış türü adını yazın. Kolayca tanımlanabilen bir üst runbook'tan başvururken yapmak için tam .NET sınıf adı kullanmanız önerilir. Bu runbook'taki veri yolu o sınıfın tüm özelliklerini gösterir ve bunları günlüğe kaydetme ve runbook'taki diğer etkinlikler için değerler olarak başvuran koşullu mantık için kullanırken çok esneklik sağlar.<br> ![Runbook giriş ve çıkış seçeneği](media/automation-runbook-output-and-messages/runbook-menu-input-and-output-option.png)

Aşağıdaki örnekte, bu özellik göstermek için iki grafik runbook'lar sahip. Modüler runbook tasarım modeli uygularsanız, görevi gören bir runbook sahip *kimlik doğrulaması Runbook şablonu* kimlik doğrulama farklı çalıştır hesabını kullanarak Azure ile yönetme. Normal olarak belirli bir senaryo otomatikleştirmek için çekirdek mantığını gerçekleştireceği, bizim ikinci runbook, bu durumda yürütme yükleneceği *kimlik doğrulaması Runbook şablonu* ve sonuçları görüntülemek, **Test** çıkış bölmesi. Normal koşullar altında bu runbook alt runbook'un çıktısını yararlanan bir kaynak karşı bir şeyler gerekir.    

Temel mantığını işte **AuthenticateTo Azure** runbook.<br> ![Runbook şablonu örnek kimliğini](media/automation-runbook-output-and-messages/runbook-authentication-template.png).  

Çıkış türü içeren *Microsoft.Azure.Commands.Profile.Models.PSAzureContext*, profil özellikleri kimlik doğrulaması döndürür.<br> ![Runbook'u çıktı türü örneği](media/automation-runbook-output-and-messages/runbook-input-and-output-add-blade.png) 

Bu runbook düz İleri olmakla birlikte, burada duyurmak için bir yapılandırma öğesi yok. Son Etkinlik yürütme **Write-Output** cmdlet'i ve profil verileri için bir PowerShell ifadesi kullanan bir $_ değişkeni yazan **Inputobject** Bu cmdlet'i için gerekli olan parametre.  

Bu örnekte ikinci runbook için adlı *Test ChildOutputType*, yalnızca iki etkinlik vardır.<br> ![Örnek alt türü Runbook çıkış](media/automation-runbook-output-and-messages/runbook-display-authentication-results-example.png) 

İlk etkinlik çağrıları **AuthenticateTo Azure** runbook ve ikinci etkinlik çalışıyor **Write-Verbose** cmdlet'iyle **veri kaynağı** , **etkinlik çıkışı** ve değeri **alan yolu** olan **Context.Subscription.SubscriptionName**, bağlam çıkışı belirtme **AuthenticateTo Azure** runbook.<br> ![Write-Verbose cmdlet parametresi veri kaynağı](media/automation-runbook-output-and-messages/runbook-write-verbose-parameters-config.png)    

Sonuçta çıktı abonelik adıdır.<br> ![Test-ChildOutputType Runbook sonuçları](media/automation-runbook-output-and-messages/runbook-test-childoutputtype-results.png)

Çıktı türü denetimi davranışını hakkında bir not. Giriş ve çıkış özellikleri dikey penceresinde çıktı Tür alanında bir değer yazdığınızda, denetim tarafından kabul edilecek girişinizin sırada, yazdıktan sonra denetimi dışında tıklatın gerekir.  

## <a name="message-streams"></a>İleti akışları
Çıkış akışından farklı olarak, ileti akışlarının kullanıcıya bilgi iletişim kurmak için tasarlanmıştır. Farklı bilgi türleri için birden çok ileti akışı vardır ve her Azure Automation tarafından farklı şekilde ele alınır.

### <a name="warning-and-error-streams"></a>Uyarı ve hata akışları
Uyarı ve hata akışlarının bir runbook'ta oluşan sorunları günlüğe kaydetmesi amaçlanır. Bir runbook yürütülür ve bir runbook test edildiğinde Azure Portalı'nı Test çıkışı bölmesinde yer alan kullanıcılar iş geçmişine yazılır. Varsayılan olarak, runbook bir uyarı veya hatadan sonra yürütülmeye devam eder. Runbook bir uyarı veya hata ayarlayarak askıya alınması gerektiğini belirtebilirsiniz bir [tercih değişkeni](#PreferenceVariables) iletiyi oluşturmadan önce runbook'ta. Örneğin, bir özel durum gibi bir hata askıya almak bir runbook neden olmak için ayarlar **$ErrorActionPreference** Dur için.

Bir uyarı veya hata iletisini kullanarak oluşturma [Write-Warning](https://technet.microsoft.com/library/hh849931.aspx) veya [yazma hatası](http://technet.microsoft.com/library/hh849962.aspx) cmdlet'i. Etkinlikler de bu akışlara yazabilir.

    #The following lines create a warning message and then an error message that will suspend the runbook.

    $ErrorActionPreference = "Stop"
    Write-Warning –Message "This is a warning message."
    Write-Error –Message "This is an error message that will stop the runbook because of the preference variable."

### <a name="verbose-stream"></a>Ayrıntılı akış
Ayrıntılı ileti akışı runbook işlemi hakkında genel bilgi içindir. Bu yana [hata ayıklama akışı](#Debug) kullanılabilir olmayan bir runbook'ta ayrıntılı iletiler için hata ayıklama bilgileri kullanılmalıdır. Varsayılan olarak, yayımlanan runbook'lardan ayrıntılı iletiler iş geçmişinde depolanmaz. Ayrıntılı iletileri depolamak için Azure Portalı'nda runbook'un yapılandırma sekmesinde ayrıntılı kayıtları günlüğe yayımlanan runbook'ları yapılandırın. Çoğu durumda, performans nedenleriyle runbook için ayrıntılı kayıtları günlüğe kaydetme değil, varsayılan ayarını korumalısınız. Yalnızca sorun gidermek veya bir runbook'ta hata ayıklamak için bu seçeneği etkinleştirin.

Zaman [bir runbook'u test](automation-testing-runbook.md), runbook ayrıntılı kayıtları günlüğe kaydetmek üzere yapılandırılmış bile olsa ayrıntılı iletiler görüntülenmez. Ederken ayrıntılı iletileri görüntülemek için [bir runbook'u test](automation-testing-runbook.md), devam et $VerbosePreference değişkenini ayarlamalıdır. Bu değişken ayarlandığında, ayrıntılı iletiler Azure Portalı'nı Test çıkışı bölmesinde görüntülenir.

Kullanarak bir ayrıntılı ileti oluşturun [Write-Verbose](http://technet.microsoft.com/library/hh849951.aspx) cmdlet'i.

    #The following line creates a verbose message.

    Write-Verbose –Message "This is a verbose message."

### <a name="debug-stream"></a>Hata ayıklama akışı
Hata ayıklama akışının etkileşimli kullanıcı kullanılması amaçlanmıştır ve runbook'larda kullanılmamalıdır.

## <a name="progress-records"></a>İlerleme durumu kayıtları
Yapılandırırsanız, bir runbook'u ilerleme oturum (Azure Portalı'nda runbook'un yapılandırma sekmesinde) kaydeder ve sonra önce ve her etkinliğin çalıştıktan sonra iş geçmişine bir kayıt yazılır. Çoğu durumda, bir runbook için ilerleme durumu kayıtlarını performansını en üst düzeye çıkarmak için oturum değil varsayılan ayarını korumalısınız. Yalnızca sorun gidermek veya bir runbook'ta hata ayıklamak için bu seçeneği etkinleştirin. Runbook ilerleme durumu kayıtlarını günlüğe kaydetmek üzere yapılandırılmış olsa bile bir runbook'u test ederken ilerleme durumu iletileri görüntülenmez.

[Write-Progress](http://technet.microsoft.com/library/hh849902.aspx) cmdlet olmadığından bir runbook'ta geçerli bu etkileşimli bir kullanıcı ile kullanılmak üzere tasarlanmıştır.

## <a name="preference-variables"></a>Tercih değişkenleri
Windows PowerShell kullanan [tercih değişkenleri](http://technet.microsoft.com/library/hh847796.aspx) farklı çıkış akışlarına gönderilen verilerin yanıt vereceğinizi belirlemek için. Bir runbook'ta nasıl farklı akışlara gönderilen verilerin yanıt denetlemek için bu değişkenleri ayarlayabilirsiniz.

Aşağıdaki tabloda geçerli runbook'ları ve varsayılan değerleri kullanılabilir tercih değişkenleri listeler. Bu tablo yalnızca bir runbook'ta geçerli olan değerleri içerir. Windows PowerShell Azure Otomasyonu dışında kullanıldığında tercih değişkenleri için ek değerler geçerlidir.

| Değişken | Varsayılan Değer | Geçerli Değerler |
|:--- |:--- |:--- |
| WarningPreference |Devam |Durdur<br>Devam<br>SilentlyContinue |
| ErrorActionPreference |Devam |Durdur<br>Devam<br>SilentlyContinue |
| VerbosePreference |SilentlyContinue |Durdur<br>Devam<br>SilentlyContinue |

Aşağıdaki tabloda runbook'larda geçerli olan tercih değişkeni değerleri için davranışı listelenmektedir.

| Değer | Davranış |
|:--- |:--- |
| Devam |İletiyi günlüğe kaydeder ve runbook'u yürütmeye devam eder. |
| SilentlyContinue |İletiyi günlüğe kaydetmeden runbook'u yürütmeye devam eder. Bu, iletiyi yoksaymakla aynı etkiye sahiptir. |
| Durdur |İletiyi günlüğe kaydeder ve runbook'u askıya alır. |

## <a name="retrieving-runbook-output-and-messages"></a>Runbook çıkışlarını ve iletilerini alma
### <a name="azure-portal"></a>Azure portalına
Bir runbook'un işler sekmesinden Azure portalında bir runbook işi ayrıntılarını görüntüleyebilirsiniz. Giriş parametreleri iş özetini görüntüler ve [çıkış akışı](#Output) işi ve bunlar oluştuysa hakkındaki genel bilgileri. Gelen iletileri geçmişini içerir [çıkış akışı](#Output) ve [uyarı ve hata akışları](#WarningError) ek olarak [ayrıntılı akış](#Verbose) ve [ilerleme durumu kayıtlarını](#Progress) runbook günlük ayrıntılı ve ilerleme durumu kayıtlarını için yapılandırılmışsa.

### <a name="windows-powershell"></a>Windows PowerShell
Windows PowerShell'de, çıkışı ve iletileri kullanarak bir runbook'tan alabilirsiniz [Get-AzureAutomationJobOutput](https://msdn.microsoft.com/library/mt603476.aspx) cmdlet'i. Bu cmdlet iş Kimliğini gerektirir ve hangi akışın döndürüleceğini belirttiğiniz akış adlı bir parametreye sahiptir. Belirleyebileceğiniz **herhangi** işin tüm akışlarını döndürmek için.

Aşağıdaki örnek, örnek bir runbook başlatır ve tamamlanmasını bekler. Tamamlandığında, çıkış akışı işten toplanır.

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
    

### <a name="graphical-authoring"></a>Grafik yazma
Grafik runbook'lar için fazladan günlüğü etkinlik düzeyi izleme formunda kullanılabilir. İzleme iki düzeyi vardır: temel ve ayrıntılı. Temel izleme başlangıç görebilirsiniz ve runbook yanı sıra tüm etkinlik yeniden deneme sayısı gibi ilgili bilgileri her etkinliğin bitiş saati çalışır ve Etkinliğin başlangıç. Ayrıntılı izleme, her etkinlik için temel izleme artı girdi ve çıktı veri alın. Şu anda izleme kayıtları izlemeyi etkinleştirdiğinizde, ayrıntılı günlük etkinleştirmeniz gerekir böylece ayrıntılı akış kullanılarak yazılır. İzlemenin etkin ile grafik runbook'lar için temel izleme aynı amaca hizmet eder ve daha bilgilendirici olduğu için ilerleme durumu kayıtlarını günlüğe kaydetmek için gerek yoktur.

![Görünüm grafik yazma iş akışları](media/automation-runbook-output-and-messages/job-streams-view-blade.png)

Önceki ekran görüntüsünde, ayrıntılı günlük kaydı ve grafik runbook'lar için izlemeyi etkinleştirdiğinizde, çok daha fazla bilgi iş akışları görünüm üretimde kullanılabilir olduğunu görebilirsiniz. Bu ek bilgiler ile runbook üretim sorunlarını gidermeye yönelik temel olabilir ve bu nedenle, yalnızca bu genel bir uygulama değil de, bunun için etkinleştirmeniz gerekir. İzleme kayıtları, özellikle çok sayıda olabilir. Grafik runbook izleme ile etkinlik basit veya ayrıntılı izleme yapılandırmış olmanıza bağlı olarak her iki ila dört kayıt elde edebilirsiniz. Sorun giderme için bir runbook ilerlemesini izlemek için bu bilgileri gerekli olmadıkça, izleme tutmak kapalı isteyebilirsiniz.

**Etkinlik düzeyinde izlemeyi etkinleştirmek için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, Otomasyon hesabınızı açın.
2. Altında **işlem Otomasyonu**seçin **Runbook'lar** listesini açmak için.
3. Runbook'ları sayfasında, grafik runbook, runbook'ları listesinden seçmek için tıklatın.
4. Altında **ayarları**, tıklatın **günlüğe kaydetme ve izleme**.
5. Günlüğe kaydetme ve ayrıntılı kayıtları günlüğe altında sayfa izleme tıklayın **üzerinde** ayrıntılı günlük kaydını etkinleştir ve etkinlik düzeyinde izleme altında izleme düzeyini değiştirmek için **temel** veya **ayrıntılı** ihtiyaç duyduğunuz izleme düzeyi temelinde.<br>
   
   ![Grafik yazma günlüğe kaydetme ve dikey izleme](media/automation-runbook-output-and-messages/logging-and-tracing-settings-blade.png)

### <a name="microsoft-azure-log-analytics"></a>Microsoft Azure günlük analizi
Otomasyon runbook iş durumu ve iş akışları için günlük analizi çalışma alanınız gönderebilirsiniz. Günlük analizi ile şunları yapabilirsiniz,

* Otomasyon işleriniz hakkında bilgi edinme 
* Bir e-posta veya uyarı (örneğin, başarısız veya askıya alınmış), runbook işi durumlarına dayalı tetikleyici 
* Gelişmiş sorgular, iş akışları yazma 
* İşlerini Automation hesaplarında ilişkilendirmek 
* İş Geçmişi zaman içinde görselleştirin    

Toplamak, bağıntılı ve iş verilerde işlem yapmak için günlük analizi ile entegrasyonu yapılandırma hakkında daha fazla bilgi için bkz: [Otomasyon iş durumu ve iş akışları için günlük analizi iletmek](automation-manage-send-joblogs-log-analytics.md).

## <a name="next-steps"></a>Sonraki adımlar
* Runbook yürütme, runbook işlerini izleme ve diğer teknik ayrıntılar hakkında daha fazla bilgi edinmek için bkz. [Runbook işi izleme](automation-runbook-execution.md)
* Alt runbook'ları tasarlama ve nasıl kullanılacağını anlamak için bkz: [Azure Otomasyonu'nda alt runbook'lar](automation-child-runbooks.md)

