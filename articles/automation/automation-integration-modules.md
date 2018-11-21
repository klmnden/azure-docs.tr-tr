---
title: Azure Automation tümleştirme modülü oluşturma
description: Bu eğiticide, Azure Automation’da tümleştirme modüllerinin oluşturulması, test edilmesi ve kullanım örnekleri gösterilir.
services: automation
ms.service: automation
ms.component: shared-capabilities
author: georgewallace
ms.author: gwallace
ms.date: 03/16/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: e4bd6a3e39fbb5d1eea4d7770d8940f801aecd43
ms.sourcegitcommit: 8d88a025090e5087b9d0ab390b1207977ef4ff7c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/21/2018
ms.locfileid: "52276503"
---
# <a name="azure-automation-integration-modules"></a>Azure Automation Tümleştirme Modülleri
PowerShell, Azure Automation’un ardındaki temel teknolojidir. Azure Automation PowerShell üzerine kurulu olduğundan, PowerShell modülleri Azure Automation’un genişletilmesinde önemlidir. Bu makalede, biz size, azure'da tümleştirme modülü olarak çalıştığından emin olmak için "Tümleştirme modülleri" ve kendi PowerShell modüllerinizi oluştururken için en iyi yöntemler olarak adlandırılır, PowerShell modülleri Azure Otomasyonu'nun kullanımını ayrıntılarını yol Otomasyon. 

## <a name="what-is-a-powershell-module"></a>PowerShell Modülü nedir?
PowerShell modülü, PowerShell konsolundan kullanılabilen **Get-Date** veya **Copy-Item** gibi çeşitli PowerShell cmdlet’leri; PowerShell DSC kaynaklarından kullanılabilen WindowsFeature or File gibi PowerShell DSC kaynakları; betikler, iş akışları ve runbook’lardan oluşan bir gruptur. PowerShell işlevlerinin tümü, cmdlet'ler ve DSC kaynakları aracılığıyla sunulur ve her cmdlet/DSC kaynağı bir PowerShell modülü tarafından desteklenir. Bu modüllerin çoğu PowerShell ile birlikte gelir. Örneğin, **Get-Date** cmdlet’i Microsoft.PowerShell.Utility PowerShell modülünün bir parçası, **Copy-Item** cmdlet’i Microsoft.PowerShell.Management PowerShell modülünün bir parçası ve Paket DSC kaynağı da PSDesiredStateConfiguration PowerShell modülünün bir parçasıdır. Bu modüllerin her ikisi de PowerShell ile birlikte verilir. Ancak çok sayıda PowerShell modülü PowerShell ile birlikte gelmez; System Center 2012 Configuration Manager gibi birinci ve üçüncü taraf ürünleriyle birlikte veya geniş PowerShell topluluğu tarafından PowerShell Galerisi gibi yerlerde dağıtılır. Modüller, kapsüllenmiş işlevler aracılığıyla karmaşık görevleri daha basit hale getirdiklerinden kullanışlıdır.  Daha fazla bilgiyi [MSDN'de PowerShell modülleri](https://msdn.microsoft.com/library/dd878324%28v=vs.85%29.aspx) konusunda bulabilirsiniz. 

## <a name="what-is-an-azure-automation-integration-module"></a>Azure Automation Tümleştirme Modülü nedir?
Tümleştirme modülü PowerShell modülünden farklı değildir. Bu, isteğe bağlı olarak ek bir dosyanın bulunduğu bir PowerShell modülüdür. Bu dosya, runbook'larda modülün cmdlet'leriyle kullanılacak Azure Automation bağlantı türünü belirten bir meta veri dosyasıdır. İsteğe bağlı dosya olsun veya olmasın bu PowerShell modülleri, içerdikleri cmdlet’ler runbook’larda, DSC kaynakları da DSC yapılandırmalarında kullanılacak şekilde Azure Automation’a aktarılabilirler. Arka planda, Azure Otomasyonu bu modülleri depolar, runbook işi ve DSC derleme işi yürütme süresinde de bunları Azure Otomasyonu korumalı alanlarına yükler; bu korumalı alanlarda runbook’ların yürütüldüğü, DSC yapılandırmalarının derlenir. Modüllerdeki DSC kaynakları otomatik olarak Automation DSC çekme sunucusuna da yerleştirilir; böylece, DSC yapılandırmalarını uygulamaya çalışan makineler bunları çekebilir.  

Azure yönetimine hemen başlayabilmeniz için Azure Otomasyonu ile birlikte birkaç Azure PowerShell modülü sunuyoruz; ancak tümleştirmek istediğiniz sistem, hizmet veya araca göre diğer PowerShell modüllerini de içeri aktarabilirsiniz. 

> [!NOTE]
> Bazı modüller Otomasyon hizmetinde “genel modüller” olarak gönderilir. Bu genel modüller bir Otomasyon hesabı oluşturmak ve bazen, olduğunda sizi otomatik olarak bunları Otomasyon hesabınıza gönderir, kullanılabilir. Bunları otomatik olarak güncelleştirilmesini istemiyorsanız, her zaman aynı modülde kendiniz alabileceğiniz ve bu hizmette sevk Bu modülün genel modül sürümünden önceliklidir. 

Tümleştirme Modülü paketini içeri aktardığınız biçim modülle aynı adda, .zip uzantılı sıkıştırılmış bir dosyadır. Windows PowerShell modülünü ve modülde varsa bir bildirim dosyası da (.psd1) dahil olmak üzere destek dosyalarını içerir.

Modülde bir Azure Otomasyonu bağlantı türü varsa, bağlantı türü özelliklerini belirten `<ModuleName>-Automation.json` adlı bir dosya da olmalıdır. Sıkıştırılmış .zip dosyanızın modül klasöründe yer alan bu json dosyasında, sisteme veya modülü temsil eden hizmete bağlanmak için gereken "bağlantı" alanları bulunur. Bu, Azure Automation'da bağlantı türü oluşturma yukarı sona erer. Bu dosyayı kullanarak, modülün bağlantı türü için alan adlarını, türlerini ve alanların şifreli veya isteğe bağlı olup olmayacağını ayarlayabilirsiniz. Burada, json dosyası biçiminde bir şablon verilmiştir:

```json
{ 
   "ConnectionFields": [
   {
      "IsEncrypted":  false,
      "IsOptional":  false,
      "Name":  "ComputerName",
      "TypeName":  "System.String"
   },
   {
      "IsEncrypted":  false,
      "IsOptional":  true,
      "Name":  "Username",
      "TypeName":  "System.String"
   },
   {
      "IsEncrypted":  true,
      "IsOptional":  false,
      "Name":  "Password",
   "TypeName":  "System.String"
   }],
   "ConnectionTypeName":  "DataProtectionManager",
   "IntegrationModuleName":  "DataProtectionManager"
}
```

Service Management Automation dağıttıysanız ve Otomasyon runbook'larınız için tümleştirme modülü paketleri oluşturuldu, bunu size tanıdık gelecektir. 

## <a name="authoring-best-practices"></a>Yazma En İyi Uygulamaları
Tümleştirme Modülleri temelde birer PowerShell modülü olsa da PowerShell modülü yazarken, Azure Otomasyonu'nda en kullanışlı hale getirmek için bazı noktalara dikkat etmenizi öneririz. Automation kullanıp kullanmamanızdan bağımsız olarak, bunlardan bazıları Azure Automation’a özeldir, bazıları da yalnızca PowerShell İş Akışı’nda modüllerin iyi çalışmasını sağlar. 

1. Modüldeki her cmdlet için bir özet, açıklama ve yardım URI’sı vardır. PowerShell'de, kullanıcının **Get-Help** cmdlet'ini kullanarak cmdlet'ler hakkında yardım almasını sağlayacak bazı bilgiler tanımlayabilirsiniz. Örneğin, bir .psm1 dosyasında yazılan PowerShell modülü için bir yardım URI’sını ve özeti nasıl tanımlayacağınız aşağıda anlatılmıştır.<br>  
   
    ```powershell
    <#
        .SYNOPSIS
         Gets all outgoing phone numbers for this Twilio account 
    #>
    function Get-TwilioPhoneNumbers {
    [CmdletBinding(DefaultParameterSetName='SpecifyConnectionFields', `
    HelpUri='http://www.twilio.com/docs/api/rest/outgoing-caller-ids')]
    param(
       [Parameter(ParameterSetName='SpecifyConnectionFields', Mandatory=$true)]
       [ValidateNotNullOrEmpty()]
       [string]
       $AccountSid,
   
       [Parameter(ParameterSetName='SpecifyConnectionFields', Mandatory=$true)]
       [ValidateNotNullOrEmpty()]
       [string]
       $AuthToken,
   
       [Parameter(ParameterSetName='UseConnectionObject', Mandatory=$true)]
       [ValidateNotNullOrEmpty()]
       [Hashtable]
       $Connection
    )
   
    $cred = CreateTwilioCredential -Connection $Connection -AccountSid $AccountSid -AuthToken $AuthToken
   
    $uri = "$TWILIO_BASE_URL/Accounts/" + $cred.UserName + "/IncomingPhoneNumbers"
   
    $response = Invoke-RestMethod -Method Get -Uri $uri -Credential $cred
   
    $response.TwilioResponse.IncomingPhoneNumbers.IncomingPhoneNumber
    }
    ```
   <br> Bu bilgiler verildiğinde PowerShell konsolunda **Get-Help** cmdlet’i kullanılarak ilgili yardım görülebileceği gibi, Azure Otomasyonu'nda bu yardım işlevinin olduğu da gösterilir.  Örneğin, runbook yazma sırasında etkinlik eklenirken. "Ayrıntılı yardımı görüntüleyin" seçeneğinin tıklanması, yardım URI’sini Azure Automation’a erişmek için kullandığınız başka bir web tarayıcısı sekmesinde açar.<br>![Tümleştirme Modülü Yardımı](media/automation-integration-modules/automation-integration-module-activitydesc.png)
1. Modül uzak bir sistemle bağlantılı olarak çalışıyorsa,

    a. Bu uzak sisteme bağlanmak için gereken bilgileri tanımlayan Tümleştirme Modülü meta veri dosyasını içermesi gerekir. Bu bilgilere bağlantı türü de denir.  
    b. Modüldeki her cmdlet, parametre olarak bir bağlantı nesnesi (ilgili bağlantı türünün bir örneği) alabilmelidir.  

    Modüldeki cmdlet'ler, bağlantı türü alanlarının bulunduğu nesneyi cmdlet’e parametre olarak geçirme izni verirseniz Azure Automation’da daha kolay kullanılır. Bu şekilde, kullanıcılar bir cmdlet’i her çağırdığında bağlantı varlığı parametrelerinin cmdlet'in ilgili parametreleriyle eşlenmesi gerekmez. Yukarıdaki runbook örneğinde, Twilio’ya erişmek ve tüm telefon numaralarını hesaba getirmek için CorpTwilio adlı bir Twilio bağlantı varlığı kullanılmıştır.  Bağlantı alanlarının cmdlet parametreleriyle nasıl eşlendiğini fark ettiniz mi?<br>
   
    ```powershell
    workflow Get-CorpTwilioPhones
    {
      $CorpTwilio = Get-AutomationConnection -Name 'CorpTwilio'
   
      Get-TwilioPhoneNumbers 
        -AccountSid $CorpTwilio.AccountSid  
        -AuthToken $CorptTwilio.AuthToken
    }
    ```
  
    Buna daha iyi ve daha kolay bir yaklaşım, bağlantı nesnesini doğrudan cmdlet’e geçirmektir. 
   
    ```powershell
    workflow Get-CorpTwilioPhones
    {
      $CorpTwilio = Get-AutomationConnection -Name 'CorpTwilio'
   
      Get-TwilioPhoneNumbers -Connection $CorpTwilio
    }
    ```
   
    Cmdlet’lerinizin, bağlantı nesnesini parametrelerin bağlantı alanları yerine doğrudan bir parametre olarak kabul etmelerine izin vererek benzer bir yaklaşım elde edebilirsiniz. Azure Automation kullanmayan kullanıcılar cmdlet'lerinizi bağlantı nesnesi gibi davranacak bir Hashtable yapılandırmadan çağırabilir, genellikle bir parametre kümesi her biri için kullanmanız gerekir. Bağlantı alanı özelliklerini tek tek geçirmek için aşağıdaki **SpecifyConnectionFields** parametre kümesi kullanılmıştır. **UseConnectionObject** bağlantıyı doğrudan geçirmenizi sağlar. Gördüğünüz gibi, [Twilio PowerShell modülündeki](https://gallery.technet.microsoft.com/scriptcenter/Twilio-PowerShell-Module-8a8bfef8) Send-TwilioSMS cmdlet’i iki şekilde de geçirmenizi sağlar: 
   
    ```powershell
    function Send-TwilioSMS {
      [CmdletBinding(DefaultParameterSetName='SpecifyConnectionFields', `
      HelpUri='http://www.twilio.com/docs/api/rest/sending-sms')]
      param(
         [Parameter(ParameterSetName='SpecifyConnectionFields', Mandatory=$true)]
         [ValidateNotNullOrEmpty()]
         [string]
         $AccountSid,
   
         [Parameter(ParameterSetName='SpecifyConnectionFields', Mandatory=$true)]
         [ValidateNotNullOrEmpty()]
         [string]
         $AuthToken,
   
         [Parameter(ParameterSetName='UseConnectionObject', Mandatory=$true)]
         [ValidateNotNullOrEmpty()]
         [Hashtable]
         $Connection
   
       )
    }
    ```
   <br>
1. Modüldeki tüm cmdlet’ler için çıktı türünü tanımlayın. Cmdlet için bir çıktı türünün tanımlanması tasarım zamanında IntelliSense’in, cmdlet’in yazma sırasında kullanılan çıktı özelliklerini belirlemenize yardımcı olmasını sağlar. Bu işlem, Modülünüzde kolay bir kullanıcı deneyimi sağlamak için tasarım zamanı bilgisinin önemli olduğu Automation runbook grafik yazma işlemleri sırasında özellikle yardımcı olur.<br><br> ![Grafik Runbook’u Çıktı Türü](media/automation-integration-modules/runbook-graphical-module-output-type.png)<br> PowerShell ISE’de cmdlet çıktısının cmdlet’in çalıştırılmasını gerektirmeyen "ileri tür" işlevine benzer.<br><br> ![POSH IntelliSense](media/automation-integration-modules/automation-posh-ise-intellisense.png)<br>
1. Modüldeki cmdlet'ler parametreler için karmaşık nesne türlerini almamalıdır. PowerShell İş Akışı, karmaşık türleri seri durumdan çıkarılmış biçimde depolaması nedeniyle PowerShell’den farklıdır. Temel türler temel olarak kalır, ancak karmaşık türler temelde özellik paketi olurlar, seri durumdan çıkarılmış hale dönüştürülür. Örneğin, bir runbook’ta (veya bununla ilgili PowerShell İş Akışı) **Get-Process** cmdlet’i kullanırsanız, beklenen [System.Diagnostic.Process] türünü değil, [Deserialized.System.Diagnostic.Process] türü nesnesini döndürür. Bu tür, seri durumdan çıkarılmamış türle aynı özelliklere sahip olsa da, yöntemlerin hiçbiri yoktur. Ve bu değer, burada cmdlet Bu parametre için bir [System.Diagnostic.Process] değerini bekler cmdlet'e parametre olarak geçirmek çalışırsanız aşağıdaki hatayı alırsınız: *'işlem' parametresinde bağımsız değişken dönüşümü işlenemiyor. Hata: "Deserialized.System.Diagnostics.Process" türündeki "System.Diagnostics.Process (CcmExec)" değeri "System.Diagnostics.Process" türüne dönüştürülemez.*   Bunun nedeni, beklenen [System.Diagnostic.Process] türüyle verilen [Deserialized.System.Diagnostic.Process] türü arasında tür uyuşmazlığı olmasıdır. Bu sorunla karşılaşmamak için modülünüze ait cmdlet’lerin parametre olarak karmaşık tür kullanmaması gerekir. Burada bunu yapmanın yanlış yolu gösterilmektedir.
   
    ```powershell
    function Get-ProcessDescription {
      param (
            [System.Diagnostic.Process] $process
      )
      $process.Description
    }
    ``` 
    <br>
    Buradaysa doğru yolu; karmaşık nesneyi alıp kullanmak için cmdlet tarafından dahili olarak kullanılabilen temel nesne alınmaktadır. Cmdlet’ler PowerShell İş Akışı değil de PowerShell bağlamında yürütüldüğünden, cmdlet’in içinde $process doğru [System.Diagnostic.Process] türüne dönüşür.  
   
    ```powershell
    function Get-ProcessDescription {
      param (
            [String] $processName
      )
      $process = Get-Process -Name $processName
   
      $process.Description
    }
    ```
   <br>
   Runbook’lardaki bağlantı varlıkları hashtable’lardır. Bu hashtable’lar karmaşık tür olmalarına rağmen Bağlantı parametreleri nedeniyle yayın özel durumu olmadan cmdlet’lere sorunsuz geçebilirler. Teknik olarak, bazı PowerShell türleri, seri durumdaki biçimlerinden seri durumdan çıkarılmış biçimlerine düzgün bir şekilde yayınlanabilirler; bu nedenle de seri durumdan çıkarılmış türü kabul eden parametrelerde cmdlet'lere geçirilebilirler. Hashtable bunlardan biridir. Bir modül yazarının tanımlı türlerinin de seri durumdan çıkarılabilecek şekilde uygulanması mümkündür, ancak bu durumda bazı kısıtlamalar geçerli olur. Türün bir varsayılan oluşturucusu olmalı, özelliklerinin tümü genel olmalı ve bir PSTypeConverter’ı olması gerekir. Ancak, modül yazarına ait olmayan zaten tanımlı türler için, bunları “düzeltmenin” bir yolu yoktur; bu nedenle, parametrelerin tüm karmaşık türlerinden kaçınılması önerilir. Runbook yazma ipucu: Bazı cmdlet'lerinizin karmaşık tür parametre alması gerekirse ya da başka birinin karmaşık tür parametre gerektiren bir modülünü kullanıyorsanız, PowerShell İş Akışı runbook'larında ve yerel PowerShell'deki PowerShell İş Akışları’nda kullanabileceğiniz geçici çözüm karmaşık türü oluşturan cmdlet’i ve karmaşık türü tüketen cmdlet’i aynı InlineScript etkinliğinde sarmalamaktır. InlineScript, içeriklerini PowerShell İş Akışı değil de PowerShell olarak yürüttüğünden, karmaşık tür oluşturan cmdlet, seri durumdan çıkarılan karmaşık türü değil, doğru türü üretir.
1. Modüldeki tüm cmdlet’leri durum bilgisiz hale getirin. PowerShell İş Akışı, iş akışında çağrılan her cmdlet’i farklı bir oturumda çalıştırır. Bu nedenle aynı modüldeki başka cmdlet’ler tarafından oluşturulan veya değiştirilen oturum durumuna bağlı cmdlet'lerin PowerShell İş Akışı runbook'larında çalışmaz.  İşte yapılmaması gereken bir örnek.
   
    ```powershell
    $globalNum = 0
    function Set-GlobalNum {
       param(
           [int] $num
       )
   
       $globalNum = $num
    }
    function Get-GlobalNumTimesTwo {
       $output = $globalNum * 2
   
       $output
    }
    ```
   <br>
1. Modülün tamamı Xcopy’ye uygun bir pakette yer almalıdır. Runbook’ların yürütülmesi gerektiğinde Azure Automation modülleri Automation korumalı alanlarına dağıtıldığından, çalıştırıldıkları konaktan bağımsız çalışmaları gerekir. Bu durumda modül paketini sıkıştırabilir, aynı veya daha yeni bir PowerShell sürümünü kullanan başka konaklara taşıyabilir, bu konakta PowerShell ortamına içe aktarıldığında da normal çalışmasını sağlayabilirsiniz. Bunun gerçekleşmesi için, modülün modül klasörü dışında herhangi bir dosyaya (Azure Automation’a içeri aktarırken sıkıştırılan klasör) ya da konaktaki benzersiz kayıt defteri ayarlarına (ürün yüklemelerinde ayarlananlar gibi) bağlı olmaması gerekir. Bu en iyi yönteme uyulmazsa modül Azure Otomasyonu'nda kullanılamaz.  

## <a name="next-steps"></a>Sonraki adımlar

* PowerShell iş akışı runbook'larını kullanmaya başlamak için bkz. [İlk PowerShell iş akışı runbook uygulamam](automation-first-runbook-textual.md)
* PowerShell Modülleri oluşturma hakkında daha fazla bilgi için bkz. [Windows PowerShell Modülü Yazma](https://msdn.microsoft.com/library/dd878310%28v=vs.85%29.aspx)

