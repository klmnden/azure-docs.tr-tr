---
title: Azure Automation tümleştirme modülü oluşturma
description: Bu eğiticide, Azure Automation’da tümleştirme modüllerinin oluşturulması, test edilmesi ve kullanım örnekleri gösterilir.
services: automation
ms.service: automation
ms.subservice: shared-capabilities
author: georgewallace
ms.author: gwallace
ms.date: 01/24/2019
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 9122cf5cc908d578d8b781c6fdc49d7b04b0ab58
ms.sourcegitcommit: e69fc381852ce8615ee318b5f77ae7c6123a744c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/11/2019
ms.locfileid: "55990351"
---
# <a name="azure-automation-integration-modules"></a>Azure Automation Tümleştirme Modülleri

PowerShell, Azure Automation’un ardındaki temel teknolojidir. Azure Automation PowerShell üzerine kurulu olduğundan, PowerShell modülleri Azure Automation’un genişletilmesinde önemlidir. Bu makalede, PowerShell modülleri için tümleştirme modülleri adlandırılır, Azure Otomasyonu'nun kullanımını ayrıntılarını öğreneceksiniz. Ayrıca, Azure automation'da tümleştirme modülü olarak çalıştığından emin olmak için kendi PowerShell modüllerinizi oluştururken için önerilen uygulamaların öğreneceksiniz. 

## <a name="what-is-a-powershell-module"></a>PowerShell Modülü nedir?

Bir PowerShell modülü, PowerShell cmdlet'leri gibi grubudur **Get-Date** veya **Copy-Item**, gelen kullanılabilir:

* PowerShell Konsolu
* betikleri
* İş akışları
* Runbook'ları
* DSC kaynakları

PowerShell işlevlerinin tüm cmdlet'ler ve DSC kaynakları gösterilir. Her bir cmdlet'i ve DSC kaynağı bir PowerShell modülü tarafından desteklenir, PowerShell ile sevk çoğu. Örneğin, **Get-Date** cmdlet'tir parçası `Microsoft.PowerShell.Utility` PowerShell modülü ve **Copy-Item** cmdlet'tir parçası `Microsoft.PowerShell.Management` PowerShell modülü ve paket DSC kaynağı bir parçası değildir Da PSDesiredStateConfiguration PowerShell modülünün. Bu modüllerin her ikisi de PowerShell ile birlikte verilir. Çok sayıda PowerShell modülü, PowerShell bir parçası olarak gönderilen yok. Bu modüller, ilk ya da üçüncü taraf ürünleriyle veya PowerShell Galerisi gibi yerlerde dağıtılır. Modüller, kapsüllenmiş işlevler aracılığıyla karmaşık görevleri daha basit hale getirdiklerinden kullanışlıdır.  Daha fazla bilgiyi [MSDN'de PowerShell modülleri](https://msdn.microsoft.com/library/dd878324%28v=vs.85%29.aspx) konusunda bulabilirsiniz. 

## <a name="what-is-an-azure-automation-integration-module"></a>Azure Automation Tümleştirme Modülü nedir?

Tümleştirme modülü PowerShell modülünden farklı değildir. Bu, isteğe bağlı olarak ek bir dosyanın bulunduğu bir PowerShell modülüdür. Bu dosya, runbook'larda modülün cmdlet'leriyle kullanılacak Azure Automation bağlantı türünü belirten bir meta veri dosyasıdır. PowerShell modülleri, içerdikleri cmdlet'ler runbook'larda ve DSC kaynakları kullanıma DSC yapılandırmaları içinde kullanmak için kullanılabilir hale getirmek için Azure Otomasyonu içine aktarılabilir. Planda, Azure Otomasyonu bu modülleri depolar ve runbook işi ve DSC derleme işi yürütme süresinde, bunları burada runbook'ları çalıştırmak ve DSC yapılandırmaları derleme Azure Otomasyonu korumalı alanlarına yükler. Tüm modüllerdeki DSC kaynakları da otomatik olarak Automation DSC çekme sunucusuna da yerleştirilir. DSC yapılandırmaları geçerli olduğu durumlarda makineler tarafından çekilebilir.  

Azure Otomasyonu yepyeni bir dizi Azure PowerShell modüllerini gönderin. Ancak, hangi sistem, hizmet veya araç ile tümleştirmek istediğiniz PowerShell modüllerini içeri aktarabilirsiniz.

> [!NOTE]
> Bazı modüller olarak gönderilir ve **genel modüller** Otomasyon hizmetinde. Bu genel modüller bir Otomasyon hesabı oluşturmak ve bazen, olduğunda sizi otomatik olarak bunları Otomasyon hesabınıza gönderir, kullanılabilir. Bunları otomatik olarak güncelleştirilmesini istemiyorsanız, her zaman aynı modülde kendiniz alabileceğiniz ve bu hizmette sevk Bu modülün genel modül sürümünden önceliklidir.

Tümleştirme Modülü paketini içeri aktardığınız biçim modülle aynı adda, .zip uzantılı sıkıştırılmış bir dosyadır. Windows PowerShell modülünü ve modülde varsa bir bildirim dosyası da (.psd1) dahil olmak üzere destek dosyalarını içerir.

Modülde bir Azure Otomasyonu bağlantı türü varsa, bağlantı türü özelliklerini belirten `<ModuleName>-Automation.json` adlı bir dosya da olmalıdır. Sıkıştırılmış .zip dosyanızın modül klasöründe içinde yerleştirilen bir json dosyası budur. Bu dosyayı alanlarını içeren bir **bağlantı** sistem veya modülü temsil eden hizmete bağlanmak için gerekli olmasıdır. Bu yapılandırma, Azure Automation'da bağlantı türü oluşturma yukarı sona erer. Bu dosyayı kullanarak, modülün bağlantı türü için alan adlarını, türlerini ve alanların şifreli veya isteğe bağlı olup olmayacağını ayarlayabilirsiniz. Aşağıdaki örnek json dosyası biçiminde bir şablondur:

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

Service Management Automation dağıttıysanız ve Otomasyon runbook'larınız için tümleştirme modülü paketleri oluşturulan bu şablon size tanıdık gelecektir.

## <a name="authoring-best-practices"></a>Yazma En İyi Uygulamaları

Tümleştirme modülleri PowerShell modülleri olsa da devam etmenizi öneririz, Azure automation'da kullanmak için bir PowerShell modülü yazarken, göz önünde bulundurun yoktur. Bazı öneriler modüllerinizi da PowerShell iş akışı içinde çalışır hale getirmek yararlıdır.

1. Modüldeki her cmdlet için bir özet, açıklama ve yardım URI’sı vardır. PowerShell'de, kullanıcının **Get-Help** cmdlet'ini kullanarak cmdlet'ler hakkında yardım almasını sağlayacak bazı bilgiler tanımlayabilirsiniz. Örneğin, nasıl bir özeti tanımlayabilir ve bir .psm1 dosyasında yazılan PowerShell modülü için Yardım URI aşağıdadır. 
   
    ```powershell
    <#
        .SYNOPSIS
         Gets all outgoing phone numbers for this Twilio account 
    #>
    function Get-TwilioPhoneNumbers {
    [CmdletBinding(DefaultParameterSetName='SpecifyConnectionFields', `
    HelpUri='https://www.twilio.com/docs/api/rest/outgoing-caller-ids')]
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

   Bu bilgiler verildiğinde gösterir kullanarak bu Yardım **Get-Help** PowerShell konsolundaki cmdlet'i. Bu bilgiler ayrıca Azure Otomasyonu içinde kullanıma sunulur.  Örneğin, runbook yazma sırasında etkinlik eklenirken. 'Ayrıntılı Yardımı Görüntüle' ı tıklatarak Yardım URI'sini Azure Automation'a erişmek için kullandığınız web tarayıcısını başka bir sekmede açılır.

   ![Tümleştirme Modülü Yardımı](media/automation-integration-modules/automation-integration-module-activitydesc.png)

2. Modül uzak bir sistemle bağlantılı olarak çalışıyorsa:

   1. Bu uzak sisteme bağlanmak için gereken bilgileri tanımlayan Tümleştirme Modülü meta veri dosyasını içermesi gerekir. Bu bilgilere bağlantı türü de denir.
   2. Modüldeki her cmdlet, parametre olarak bir bağlantı nesnesi (ilgili bağlantı türünün bir örneği) alabilmelidir.  

    Modüldeki cmdlet'ler, bağlantı türü alanlarının bulunduğu nesneyi cmdlet’e parametre olarak geçirme izni verirseniz Azure Automation’da daha kolay kullanılır. Bu, kullanıcıların bir cmdlet'i her çağırdığında bağlantı varlığı parametrelerinin cmdlet'in ilgili parametreleriyle eşlenecek sağlar.

    Yukarıdaki runbook örneğinde, Twilio’ya erişmek ve tüm telefon numaralarını hesaba getirmek için CorpTwilio adlı bir Twilio bağlantı varlığı kullanılmıştır.  Bu bağlantı alanlarının cmdlet parametreleri nasıl eşliyor fark ettiniz mi?

    ```powershell
    workflow Get-CorpTwilioPhones
    {
      $CorpTwilio = Get-AutomationConnection -Name 'CorpTwilio'

      Get-TwilioPhoneNumbers 
        -AccountSid $CorpTwilio.AccountSid  
        -AuthToken $CorptTwilio.AuthToken
    }
    ```
  
    Bu davranış yaklaşım daha kolay ve daha iyi bir şekilde doğrudan bağlantı nesnesi cmdlet'e geçirdiğini-

    ```powershell
    workflow Get-CorpTwilioPhones
    {
      $CorpTwilio = Get-AutomationConnection -Name 'CorpTwilio'

      Get-TwilioPhoneNumbers -Connection $CorpTwilio
    }
    ```

    Bağlantı nesnesini doğrudan parametrelerin bağlantı alanları yerine bir parametre olarak kabul etmelerine izin vererek, önceki örnekte olduğu gibi davranış cmdlet'lerinizi için etkinleştirebilirsiniz. Azure Automation kullanmayan kullanıcılar cmdlet'lerinizi bağlantı nesnesi gibi davranacak bir Hashtable yapılandırmadan çağırabilir, genellikle bir parametre kümesi her biri için kullanmanız gerekir. Parametre kümesi **SpecifyConnectionFields**, bağlantı alanı özelliklerini tek tek geçirmek için kullanılır. **UseConnectionObject** bağlantıyı doğrudan geçirmenizi sağlar. Gördüğünüz gibi, [Twilio PowerShell modülündeki](https://gallery.technet.microsoft.com/scriptcenter/Twilio-PowerShell-Module-8a8bfef8) Send-TwilioSMS cmdlet’i iki şekilde de geçirmenizi sağlar:

    ```powershell
    function Send-TwilioSMS {
      [CmdletBinding(DefaultParameterSetName='SpecifyConnectionFields', `
      HelpUri='https://www.twilio.com/docs/api/rest/sending-sms')]
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

3. Modüldeki tüm cmdlet’ler için çıktı türünü tanımlayın. Cmdlet için bir çıktı türünün tanımlanması tasarım zamanında IntelliSense’in, cmdlet’in yazma sırasında kullanılan çıktı özelliklerini belirlemenize yardımcı olmasını sağlar. Otomasyon runbook grafik, tasarım zamanı bilgisinin bir kolayca kullanıcı deneyimi modülünüzde anahtarına olduğu yazma sırasında bu özellikle yararlı olur.

   ![Grafik Runbook’u Çıktı Türü](media/automation-integration-modules/runbook-graphical-module-output-type.png)

   Bu davranış çalıştırmak zorunda kalmadan PowerShell ıse'de cmdlet çıktısının "İleri tür" işlevine benzer.

   ![POSH IntelliSense](media/automation-integration-modules/automation-posh-ise-intellisense.png)

4. Modüldeki cmdlet'ler parametreler için karmaşık nesne türlerini sürmez. PowerShell İş Akışı, karmaşık türleri seri durumdan çıkarılmış biçimde depolaması nedeniyle PowerShell’den farklıdır. Temel türler temel olarak kalır, ancak temelde özellik paketi olurlar, seri durumdan çıkarılmış karmaşık türler Dönüştür. Örneğin, bir runbook’ta (veya bununla ilgili PowerShell İş Akışı) **Get-Process** cmdlet’i kullanırsanız, beklenen [System.Diagnostic.Process] türünü değil, [Deserialized.System.Diagnostic.Process] türü nesnesini döndürür. Bu tür, seri durumdan çıkarılmamış türle aynı özelliklere sahip olsa da, yöntemlerin hiçbiri yoktur. Ve bu değer, burada cmdlet Bu parametre için bir [System.Diagnostic.Process] değerini bekler cmdlet'e parametre olarak geçirmek çalışırsanız aşağıdaki hatayı alırsınız: *'Process' parametresinde bağımsız değişken dönüşümü işlenemiyor. Hata: "" "System.Diagnostics.Process" türüne Deserialized.System.Diagnostics.Process"türündeki"System.Diagnostics.Process (CcmExec)"değeri dönüştürülemiyor.*   Beklenen [System.Diagnostic.Process] türüyle verilen [Deserialized.System.Diagnostic.Process] türü arasındaki tür uyuşmazlığı olduğundan budur. Bu soruna geçici bir çözüm yolu cmdlet'lerin parametreler için karmaşık türler yakalayana sağlamaktır. Burada bunu yapmanın yanlış yolu gösterilmektedir.

    ```powershell
    function Get-ProcessDescription {
      param (
            [System.Diagnostic.Process] $process
      )
      $process.Description
    }
    ```

    Doğru yolu; karmaşık nesneyi alıp kullanmak için cmdlet tarafından dahili olarak kullanılan temel gerçekleşir. Cmdlet’ler PowerShell İş Akışı değil de PowerShell bağlamında yürütüldüğünden, cmdlet’in içinde $process doğru [System.Diagnostic.Process] türüne dönüşür.  

    ```powershell
    function Get-ProcessDescription {
      param (
            [String] $processName
      )
      $process = Get-Process -Name $processName
   
      $process.Description
    }
    ```

   Runbook'lardaki bağlantı varlıkları olan bir karmaşık tür olmalarına rağmen olan ve henüz cmdlet'lere için bu hashtable'da görünüyor, ' bağlantı parametresi sorunsuz yayın özel durumu olmadan. Teknik olarak, bazı PowerShell türleri, seri durumdaki biçimlerinden seri durumdan çıkarılmış biçimlerine düzgün bir şekilde yayınlanabilirler; bu nedenle de seri durumdan çıkarılmış türü kabul eden parametrelerde cmdlet'lere geçirilebilirler. Hashtable bunlardan biridir. Bir modül yazarının tanımlı türlerinin de seri durumdan bir şekilde uygulanması mümkündür, ancak dikkate alınması gereken bazı dengelemeleri vardır. Türün bir varsayılan oluşturucusu olmalı, özelliklerinin tümü genel olmalı ve bir PSTypeConverter’ı olması gerekir. Ancak, modül yazarına ait olmayan zaten tanımlı türler için ', bu nedenle düzeltmek için ' bir yolu yoktur parametrelerin tüm karmaşık türlerinden kaçınılması önerilir. Runbook yazma İpucu: Cmdlet'lerinizi bir karmaşık tür parametre alması gerekirse, PowerShell iş akışları, geçici çözüm olan karmaşık türü oluşturan cmdlet'i ve aynı Inlinescript etkinliğinde karmaşık türü tüketen cmdlet'i sarmalamak için. InlineScript, içeriklerini PowerShell İş Akışı değil de PowerShell olarak yürüttüğünden, karmaşık tür oluşturan cmdlet, seri durumdan çıkarılan karmaşık türü değil, doğru türü üretir.

5. Modüldeki tüm cmdlet’leri durum bilgisiz hale getirin. PowerShell İş Akışı, iş akışında çağrılan her cmdlet’i farklı bir oturumda çalıştırır. Başka bir deyişle, oluşturulan veya aynı modüldeki başka cmdlet'ler tarafından değiştirilen oturum durumuna bağlı cmdlet'lerin PowerShell iş akışı runbook'larında çalışmaz.  İşte yapılmaması gereken bir örnek.
   
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

6. Modülün tamamı Xcopy’ye uygun bir pakette yer almalıdır. Runbook'ların yürütülmesi gerektiğinde azure Automation modülleri Automation korumalı için distributd ' dir. Modüller ister çalışıyor üzerinde konak bağımsız olarak çalışması gerekir. Zip erişebiliyor olmalısınız ve modül paket taşıyın ve sahip başka bir ana bilgisayarın PowerShell ortamına içe aktarıldığında da normal çalışmasını işlev. Gerçekleşmesi için sırayla modülün modül klasörü dışında herhangi bir dosya bağlıdır olmamalıdır. Bu klasör modülü Azure Automation'a içeri aktarıldığında ayarlananlar klasördür. Bir ürün yüklü olduğunda, bu ayarları kümesi gibi modülü de herhangi bir konağa benzersiz kayıt defteri ayarlarını bağlı olmamalıdır. Bu en iyi yönteme uyulmazsa modül Azure Automation'da kullanılamaz.  

7. Başvuru değilse [Az Azure Powershell modülleri](/powershell/azure/new-azureps-module-az?view=azps-1.1.0) de başvuru olmayan, modülünüzde olun `AzureRM`. `Az` Modülü ile birlikte kullanılamaz `AzureRM` modüller. `Az` runbook'larında desteklenir, ancak varsayılan olarak içe aktarılmaz. Hakkında bilgi edinmek için `Az` modülleri ve hesaba katmanız gereken noktalar [Az Modül desteği, Azure automation'da](az-modules.md).

## <a name="next-steps"></a>Sonraki adımlar

* PowerShell iş akışı runbook'larını kullanmaya başlamak için bkz. [İlk PowerShell iş akışı runbook uygulamam](automation-first-runbook-textual.md)
* PowerShell Modülleri oluşturma hakkında daha fazla bilgi için bkz. [Windows PowerShell Modülü Yazma](https://msdn.microsoft.com/library/dd878310%28v=vs.85%29.aspx)
