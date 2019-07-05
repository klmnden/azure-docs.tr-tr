---
title: Azure Otomasyonu PowerShell iş akışını öğrenme
description: Bu makalede PowerShell ve PowerShell iş akışı ve Otomasyon runbook'ları için ilgili kavramları arasındaki farkları anlamak için hızlı bir Ders yazarlar PowerShell ile tanıdık yöneliktir.
services: automation
ms.service: automation
ms.subservice: process-automation
author: bobbytreed
ms.author: robreed
ms.date: 12/14/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 085fcd6269663cb0055aaefe11ddc9434e8da7a1
ms.sourcegitcommit: f811238c0d732deb1f0892fe7a20a26c993bc4fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/29/2019
ms.locfileid: "67476991"
---
# <a name="learning-key-windows-powershell-workflow-concepts-for-automation-runbooks"></a>Otomasyon runbook'ları için temel Windows PowerShell iş akışı kavramları öğrenme

Azure automation'daki Runbook'lar Windows PowerShell iş akışları olarak uygulanır.  Bir Windows PowerShell iş akışı bir Windows PowerShell betiğiyle aynıdır, ancak yeni bir kullanıcı için kafa karıştırıcı olabilir bazı önemli farklılıklar vardır.  Bu makalede PowerShell iş akışı kullanarak runbook'ları yazmanıza yardımcı olmak için tasarlanmıştır ancak denetim noktaları gerekmedikçe PowerShell kullanarak runbook'ları yazma öneririz.  PowerShell iş akışı runbook'ları yazma sırasında birkaç söz dizimi farkları olan ve bu farklar etkin iş akışları yazmak için biraz daha fazla iş gerektirir.

İş akışı uzun süre çalışan görevleri gerçekleştiren veya birden çok aygıttaki veya yönetilen düğümlerdeki birden çok adımın koordinasyonu için gereken programlanmış, bağlantılı adımlardan oluşan bir dizidir. İş akışının normal betiğe göre avantajları bir eylemi birden çok aygıtta aynı anda gerçekleştirebilme ve hatadan otomatik olarak kurtarma yetenekleridir. Bir Windows PowerShell iş akışı Windows Workflow Foundation kullanan bir Windows PowerShell betiğidir. İş akışı Windows PowerShell sözdizimi kullanılarak yazılsa ve Windows PowerShell tarafından başlatılsa da, Windows Workflow Foundation tarafından işlenir.

Bu makaledeki konular hakkında tüm ayrıntılar için bkz. [Windows PowerShell iş akışı ile çalışmaya başlama](https://technet.microsoft.com/library/jj134242.aspx).

## <a name="basic-structure-of-a-workflow"></a>Bir iş akışı temel yapısı

Bir PowerShell iş akışı için bir PowerShell Betiği dönüştürme ilk adımı ile kapsayan **iş akışı** anahtar sözcüğü.  Bir iş akışı ile başlayan **iş akışı** ayraçlar içinde betiğin gövdesi ardından anahtar sözcüğü. İş akışının adı **iş akışı** aşağıdaki sözdiziminde gösterildiği gibi anahtar sözcüğü:

```powershell
Workflow Test-Workflow
{
    <Commands>
}
```

İş akışı adını, Otomasyon runbook'u adıyla eşleşmelidir. Runbook içeri sonra dosya adı iş akışının adıyla eşleşmelidir ve sonunda *.ps1*.

İş akışına parametreleri eklemek için **Param** anahtar sözcüğü bir betik için yaptığınız gibi.

## <a name="code-changes"></a>Kod değişiklikleri

PowerShell iş akışı kod PowerShell komut dosyası kodu dışında birkaç önemli değişiklikler için neredeyse aynı görünür.  Aşağıdaki bölümlerde, bir iş akışında çalıştırmak için bir PowerShell Betiği için bunu yapmanız gereken değişiklikler açıklanmaktadır.

### <a name="activities"></a>Etkinlikler

Etkinlik bir iş akışındaki belirli bir görevdir. Betiğin bir veya daha çok komuttan oluşmasına benzer şekilde, iş akışı da belirli bir sırayla yürütülen bir veya daha çok etkinlikten oluşur. Windows PowerShell Workflow birçok Windows PowerShell cmdlet'ini bir iş akışı içinde çalıştıklarında otomatik olarak etkinliklere dönüştürür. Biri, bu cmdlet'leri runbook'unuza belirttiğinizde, Windows Workflow Foundation tarafından karşılık gelen etkinlik çalıştırılır. Karşılık gelen bir etkinliği olmayan cmdlet'ler için, Windows PowerShell İş Akışı cmdlet'i bir [InlineScript](#inlinescript) etkinliğinde otomatik olarak çalıştırır. Hariç tutulan ve bir iş akışında açıkça bunları bir Inlinescript bloğunda eklemediğiniz sürece kullanılamaz cmdlet'ler kümesi yok. Bu kavramlarla ilgili daha fazla bilgi için bkz. [Betik İş Akışlarında Etkinlikleri Kullanma](https://technet.microsoft.com/library/jj574194.aspx).

İş akışı etkinlikleri çalışmalarını yapılandıran ortak parametreler kümesini paylaşır. Ortak iş akışı parametreleriyle ilgili daha ayrıntılı bilgi için bkz. [about_WorkflowCommonParameters](https://technet.microsoft.com/library/jj129719.aspx).

### <a name="positional-parameters"></a>Konumsal Parametreler

Konumsal parametreler etkinliği ve bir iş akışında bir cmdlet ile kullanamazsınız.  Bunun anlamı tüm parametre adları kullanmasıdır.

Örneğin, tüm çalışan hizmetler alır aşağıdaki kodu düşünün.

```azurepowershell-interactive
Get-Service | Where-Object {$_.Status -eq "Running"}
```

Bu aynı kod bir iş akışında çalıştırmayı denerseniz, "parametre kümesi belirtilen adlandırılmış parametreler kullanılarak çözümlenemez."gibi bir ileti alırsınız  Bunu düzeltmek için parametre adı olduğu aşağıdaki gibi sağlayın.

```powershell
Workflow Get-RunningServices
{
    Get-Service | Where-Object -FilterScript {$_.Status -eq "Running"}
}
```

### <a name="deserialized-objects"></a>Seri durumdan çıkarılmış nesne

İş akışlarında nesneleri seri durumdan.  Bunun anlamı özelliklerini hala kullanılabilir, ancak kendi yöntemlerini.  Örneğin, hizmet nesnesinin durdurma yöntemi kullanarak hizmeti durduran şu PowerShell kodunu göz önünde bulundurun.

```azurepowershell-interactive
$Service = Get-Service -Name MyService
$Service.Stop()
```

Bu bir iş akışında çalıştırmayı denerseniz, "bir Windows PowerShell iş akışında yöntem çağırma desteklenmiyor." belirten bir hata alırsanız

Bir seçenek olduğu iki Bu kod satırlarını sarmak için bir [Inlinescript](#inlinescript) block $Service durumda bloğu içinde bir hizmet nesnesi olması.

```powershell
Workflow Stop-Service
{
    InlineScript {
        $Service = Get-Service -Name MyService
        $Service.Stop()
    }
}
```

Başka bir seçenek varsa yöntem aynı işlevi gerçekleştiren başka bir cmdlet'ini kullanmaktır.  Bizim örnek Stop-Service cmdlet durdurma yöntemi aynı işlevselliği sağlar ve bir iş akışı için aşağıdakileri kullanabilirsiniz.

```powershell
Workflow Stop-MyService
{
    $Service = Get-Service -Name MyService
    Stop-Service -Name $Service.Name
}
```

## <a name="inlinescript"></a>InlineScript

**Inlinescript** etkinlik, bir veya daha fazla komut yerine PowerShell iş akışı gibi geleneksel PowerShell betiğini çalıştırmak ihtiyacınız olduğunda yararlıdır.  Bir iş akışındaki komutlar işlenmek üzere Windows Workflow Foundation'a gönderilirken, bir InlineScript bloğundaki komutlar Windows PowerShell tarafından işlenir.

Inlinescript, aşağıda gösterilen aşağıdaki sözdizimini kullanır.

```powershell
InlineScript
{
    <Script Block>
} <Common Parameters>
```

Çıkışı bir değişkene atayarak bir Inlinescript çıkış döndürebilir. Aşağıdaki örnek, bir hizmetini durdurur ve ardından hizmet adı çıkarır.

```powershell
Workflow Stop-MyService
{
    $Output = InlineScript {
        $Service = Get-Service -Name MyService
        $Service.Stop()
        $Service
    }

    $Output.Name
}
```

Değer bir Inlinescript bloğunun içine geçirebilirsiniz, ancak kullanmalısınız **$Using** kapsam değiştiricisi.  Aşağıdaki örnek, hizmet adı değişkeni tarafından sağlanan dışında önceki örnekle aynıdır.

```powershell
Workflow Stop-MyService
{
    $ServiceName = "MyService"

    $Output = InlineScript {
        $Service = Get-Service -Name $Using:ServiceName
        $Service.Stop()
        $Service
    }

    $Output.Name
}
```

Inlinescript etkinlikleri belirli iş akışlarında kritik olabilir, ancak iş akışı yapıları desteklemez ve yalnızca aşağıdaki nedenlerle gerekli olduğunda kullanılmalıdır:

* Kullanamazsınız [kontrol noktaları](#checkpoints) Inlinescript bloğu içinde. Blok içinde bir hata oluşursa, bloğun baştan devam gerekir.
* Kullanamazsınız [Paralel yürütme](#parallel-processing) bir InlineScriptBlock içinde.
* Inlinescript bloğunun tüm uzunluğu için Windows PowerShell oturumunu tuttuğu Inlinescript iş akışı'nın ölçeklenebilirliğini etkiler.

Inlinescript kullanma hakkında daha fazla bilgi için bkz. [bir iş akışında Windows PowerShell komutlarını çalıştırma](https://technet.microsoft.com/library/jj574197.aspx) ve [about_ınlinescript](https://technet.microsoft.com/library/jj649082.aspx).

## <a name="parallel-processing"></a>Paralel işleme

Windows PowerShell İş Akışlarının bir avantajı da bir komutlar kümesini tipik bir betikteki gibi sırayla yürütmek yerine paralel olarak gerçekleştirebilmesidir.

Kullanabileceğiniz **paralel** eşzamanlı olarak çalışan birden çok komut içeren bir betik bloğu oluşturmak için anahtar sözcüğü. Bu, aşağıda gösterilen aşağıdaki sözdizimini kullanır. Bu durumda, Activity1 ve Activity2 aynı anda başlatır. Activity3 ancak Activity1 ve Activity2 yalnızca tamamladıktan sonra başlar.

```powershell
Parallel
{
    <Activity1>
    <Activity2>
}
<Activity3>
```

Örneğin, ağ hedefe birden çok dosya kopyalamayı aşağıdaki PowerShell komutlarını göz önünde bulundurun.  Bir dosya sonraki başlatılmadan önce kopyalama bitmesi gereken şekilde bu komutlar sırayla çalıştırılır.

```azurepowershell-interactive
Copy-Item -Path C:\LocalPath\File1.txt -Destination \\NetworkPath\File1.txt
Copy-Item -Path C:\LocalPath\File2.txt -Destination \\NetworkPath\File2.txt
Copy-Item -Path C:\LocalPath\File3.txt -Destination \\NetworkPath\File3.txt
```

Bunların tümü aynı anda kopyalama işlemini başlatmak için aşağıdaki iş akışı aynı bu komutları paralel olarak çalıştırır.  Yalnızca tüm sonra kopyalanan tamamlama iletisi görüntülenir.

```powershell
Workflow Copy-Files
{
    Parallel
    {
        Copy-Item -Path "C:\LocalPath\File1.txt" -Destination "\\NetworkPath"
        Copy-Item -Path "C:\LocalPath\File2.txt" -Destination "\\NetworkPath"
        Copy-Item -Path "C:\LocalPath\File3.txt" -Destination "\\NetworkPath"
    }

    Write-Output "Files copied."
}
```

Kullanabileceğiniz **ForEach-Parallel** eşzamanlı olarak bir koleksiyondaki her öğe için komutları işlemek üzere yapısı. Betik bloğundaki komutlar sırayla yürütülürken koleksiyondaki öğeler paralel olarak işlenir. Bu, aşağıda gösterilen aşağıdaki sözdizimini kullanır. Bu durumda, Activity1 koleksiyondaki tüm öğeler için aynı anda başlatır. Her öğe için Activity2, Activity1 tamamlandıktan sonra başlar. Activity3 ancak yalnızca tüm öğeler için Activity1 ve Activity2 tamamlandığında başlar. Kullandığımız `ThrottleLimit` sınırlamak için parametre. Çok yüksek bir `ThrottleLimit` sorunlara neden olabilir. İdeal değeri `ThrottleLimit` parametresi, ortamınızdaki birçok faktöre bağlıdır. Düşük bir değerle başlangıç deneyin ve farklı artan değerleri için belirli durumda çalışan bulana kadar deneyin gerekir.

```powershell
ForEach -Parallel -ThrottleLimit 10 ($<item> in $<collection>)
{
    <Activity1>
    <Activity2>
}
<Activity3>
```

Aşağıdaki örnek, paralel olarak dosya kopyalama önceki örneğe benzerdir.  Bu durumda, bunu kopyaladıktan sonra her dosya için bir ileti görüntülenir.  Yalnızca tüm sonra tamamen kopyalanamaz son tamamlanma iletisi görüntülenir.

```powershell
Workflow Copy-Files
{
    $files = @("C:\LocalPath\File1.txt","C:\LocalPath\File2.txt","C:\LocalPath\File3.txt")

    ForEach -Parallel -ThrottleLimit 10 ($File in $Files)
    {
        Copy-Item -Path $File -Destination \\NetworkPath
        Write-Output "$File copied."
    }

    Write-Output "All files copied."
}
```

> [!NOTE]
> Bu güvenilir olmayan sonuçlar gösterilmiştir olduğundan, alt runbook'ları paralel olarak çalışan önermiyoruz. Bazen alt runbook'un çıkışı gösterilmez ve bir alt runbook ayarlarında diğer paralel alt runbook'lar etkileyebilir. $VerbosePreference $WarningPreference ve diğerleri gibi değişkenleri alt runbook'larına yayılan değil. Ve alt runbook'un bu değerleri değişirse, bunlar düzgün çağırma sonra geri yüklenebilir değil.

## <a name="checkpoints"></a>Kontrol noktaları

A *denetim noktası* değişkenlerin geçerli değerlerini ve bu noktaya kadar üretilen herhangi bir çıktı içeren iş akışının geçerli durumu anlık görüntüsüdür. Bir iş akışı hata sona erer veya askıya alındı, ardından sonraki çalıştırıldığında, iş akışının başlangıç yerine en son denetim noktasından başlayacaktır.  **Checkpoint-Workflow** etkinliğini kullanarak bir iş akışında denetim noktası ayarlayabilirsiniz. Azure Otomasyonu denilen bir özelliği olan [adil paylaşımı](automation-runbook-execution.md#fair-share), diğer runbook'ların çalışmasına izin vermek için 3 saatte bir çalışan herhangi bir runbook burada kaldırıldı. Sonuç olarak, kaldırılan runbook yeniden yüklenmesi ve olduğunda, bir runbook'ta yapılan en son kontrol noktasından yürütmeyi devam ettirir. Runbook eninde sonunda tamamlanır garantisi için kontrol noktaları 3 saatten kısa bir süre için çalışan aralıklarla eklemeniz gerekir. Her çalıştırma sırasında yeni bir kontrol noktası eklenir ve bir hata nedeniyle 3 saat sonra runbook çıkarılacak, ardından runbook süresiz olarak devam edecek.

Aşağıdaki örnek kodda Activity2 bir özel durum oluşur. sonlandırmak iş akışı neden olur. İş akışını yeniden çalıştırdığınızda, yalnızca son denetim noktasından ayarladıktan sonra bu yana Activity2 çalıştırarak başlatır.

```powershell
<Activity1>
Checkpoint-Workflow
<Activity2>
<Exception>
<Activity3>
```

Özel durum yatkın olabilir ve gereken etkinlikleri iş akışı devam ettirildiğinde tekrarlanmaması sonra bir iş akışında denetim noktası ayarlamanız gerekir. Örneğin, iş akışınızı bir sanal makine oluşturabilirsiniz. Sanal makine oluşturma komutlarının öncesinde ve sonrasında bir denetim noktası ayarlayabilirsiniz. Oluşturulması başarısız olursa, iş akışını yeniden başlatılırsa, ardından komutları yinelenmesi. Oluşturma başarılı olduktan sonra iş akışı başarısız olursa, iş akışı sürdürüldüğünde ardından sanal makineyi yeniden oluşturulmaz.

Aşağıdaki örnek, birden çok dosyalarını bir ağ konumuna kopyalar ve sonra her bir dosyayı bir denetim noktası ayarlar.  Ağ konumu kaybolursa, iş akışı hata sona erer.  Yeniden başlatıldığında, yalnızca şimdiye kadar kopyalanmış olan dosyaları atlanır anlamına son denetim noktasından devam edecek.

```powershell
Workflow Copy-Files
{
    $files = @("C:\LocalPath\File1.txt","C:\LocalPath\File2.txt","C:\LocalPath\File3.txt")

    ForEach ($File in $Files)
    {
        Copy-Item -Path $File -Destination \\NetworkPath
        Write-Output "$File copied."
        Checkpoint-Workflow
    }

    Write-Output "All files copied."
}
```

Çağırdıktan sonra kullanıcı adı kimlik bilgilerini kalıcı değildir çünkü [Suspend-Workflow](https://technet.microsoft.com/library/jj733586.aspx) etkinlik veya son denetim noktasından sonra null ve daha sonra bunları yeniden sonra varlık Mağazası'ndan almak için kimlik bilgilerini ayarlamanız gerekir.  **Suspend-Workflow** veya denetim noktası çağrılır.  Aksi takdirde, aşağıdaki hata iletisini alabilirsiniz: *İş akışı işini devam ettirilemiyor, ya da kalıcı veri tamamen kaydedilmeyecek, veya Kalıcılık verileri kaydedildi nedeniyle bozulmuş. İş akışını yeniden başlatmanız gerekir.*

Aşağıdaki aynı kod bu PowerShell iş akışı runbook'larınızı nasıl ele alınacağını gösterir.

```powershell
workflow CreateTestVms
{
    $Cred = Get-AzureAutomationCredential -Name "MyCredential"
    $null = Connect-AzureRmAccount -Credential $Cred

    $VmsToCreate = Get-AzureAutomationVariable -Name "VmsToCreate"

    foreach ($VmName in $VmsToCreate)
        {
        # Do work first to create the VM (code not shown)

        # Now add the VM
        New-AzureRmVm -VM $Vm -Location "WestUs" -ResourceGroupName "ResourceGroup01"

        # Checkpoint so that VM creation is not repeated if workflow suspends
        $Cred = $null
        Checkpoint-Workflow
        $Cred = Get-AzureAutomationCredential -Name "MyCredential"
        $null = Connect-AzureRmAccount -Credential $Cred
        }
}
```

> [!IMPORTANT]
> **Add-AzureRmAccount** için bir diğer ad sunulmuştur **Connect-AzureRMAccount**. Ne zaman kitaplığınızı arama öğe görmüyorsanız, **Connect-AzureRMAccount**, kullanabileceğiniz **Add-AzureRmAccount**, veya Otomasyon hesabınızda modüllerinizi güncelleştirebilirsiniz.

Hizmet sorumlusuyla yapılandırılmış bir farklı çalıştır hesabını kullanarak kimlik doğrulama yapıyorsunuz, bu gerekli değildir.

Denetim noktalarıyla ilgili daha fazla bilgi için bkz. [Betik İş Akışına Denetim Noktaları Ekleme](https://technet.microsoft.com/library/jj574114.aspx).

## <a name="next-steps"></a>Sonraki adımlar

* PowerShell iş akışı runbook'larını kullanmaya başlamak için bkz. [İlk PowerShell iş akışı runbook uygulamam](automation-first-runbook-textual.md)

