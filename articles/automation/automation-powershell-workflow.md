---
title: "Azure otomasyonu için PowerShell iş akışı öğrenme | Microsoft Docs"
description: "Bu makalede PowerShell ve PowerShell iş akışı ve kavramları Automation runbook'larına geçerli arasındaki belirli farkları anlamak için hızlı Ders yazarlar PowerShell ile tanıdık yöneliktir."
services: automation
documentationcenter: 
author: eslesar
manager: carmonm
editor: tysonn
ms.assetid: 84bf133e-5343-4e0e-8d6c-bb14304a70db
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/21/2017
ms.author: magoedte;bwren
ms.openlocfilehash: 6dce88bdd85a28ce05e1621b08a0f4b148b02627
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="learning-key-windows-powershell-workflow-concepts-for-automation-runbooks"></a>Otomasyon runbook'ları için temel Windows PowerShell iş akışı kavramları öğrenme 
Azure Otomasyonu runbook'ları Windows PowerShell iş akışları olarak uygulanır.  Bir Windows PowerShell iş akışı, bir Windows PowerShell komut dosyası için benzer ancak yeni bir kullanıcıya kafa karıştırıcı olabilir önemli bazı farklar vardır.  Bu makale, PowerShell iş akışı kullanarak runbook'ları yazmanıza yardımcı olmak için tasarlanmıştır, ancak denetim noktaları gerekmedikçe PowerShell kullanarak runbook'ları yazma öneririz.  PowerShell iş akışı runbook'ları yazarken birkaç söz dizimi farkları yüklenir ve bu farklılıklar etkin iş akışları yazmak için biraz daha fazla iş gerektirmez.  

Bir iş akışı, uzun süre çalışan görevler gerçekleştiren veya birden fazla cihazda veya yönetilen düğümler arasında birden fazla adımın eşgüdümünü gerektiren programlı ve bağlı adımlar dizisidir. Bir iş akışının normal betiğe avantajları aynı anda birden çok aygıt karşı bir eylem gerçekleştirme yeteneğini ve hatadan otomatik olarak kurtarma yeteneğini içerir. Bir Windows PowerShell iş akışı Windows Workflow Foundation kullanan bir Windows PowerShell komut dosyasıdır. İş akışı Windows PowerShell sözdizimi kullanılarak yazılsa ve Windows PowerShell tarafından başlatılsa olsa da, Windows Workflow Foundation tarafından işlenir.

Bu makalede konularda tam Ayrıntılar için bkz [Windows PowerShell iş akışı ile çalışmaya başlama](http://technet.microsoft.com/library/jj134242.aspx).

## <a name="basic-structure-of-a-workflow"></a>Bir iş akışının temel yapısı
Bir PowerShell iş akışı için bir PowerShell Betiği dönüştürme ilk adımı ile kapsayan **iş akışı** anahtar sözcüğü.  Bir iş akışı ile başlayan **iş akışı** ayraçlar içinde betiğin gövdesi anahtar sözcüğünü. İş akışının adı **iş akışı** anahtar sözcüğünü aşağıdaki sözdiziminde gösterildiği gibi:

    Workflow Test-Workflow
    {
       <Commands>
    }

İş akışının adı Otomasyon runbook'u adı eşleşmelidir. Runbook içeri sonra dosya adı iş akışı adı ile eşleşmesi gerekir ve içinde bitmelidir *.ps1*.

İş akışına parametre eklemek için kullanın **Param** anahtar sözcüğü bir betik için olduğu gibi.

## <a name="code-changes"></a>Kod değişiklikleri
PowerShell iş akışı kodu birkaç önemli değişiklikler dışında bir kod PowerShell neredeyse aynı arar.  Aşağıdaki bölümlerde bir iş akışında çalıştırmak için bir PowerShell Betiği onun için yapmanız gereken değişiklikleri açıklanmaktadır.

### <a name="activities"></a>Etkinlikler
Bir etkinlik, bir iş akışındaki belirli bir görevdir. Yalnızca bir komut dosyası bir veya daha fazla komuttan oluşması gibi bir iş akışı etkinliklerinin sırayla gerçekleştirilen bir veya daha fazla oluşur. Bir iş akışı çalıştığında, Windows PowerShell iş akışı birçok Windows PowerShell cmdlet'leri otomatik olarak etkinliklere dönüştürür. Runbook'unuzda bu cmdlet'leri birini belirttiğinizde, Windows Workflow Foundation tarafından karşılık gelen etkinlik çalıştırılır. Karşılık gelen bir etkinliği olmayan cmdlet'ler için Windows PowerShell iş akışı cmdlet'i otomatik olarak çalışan bir [Inlinescript](#inlinescript) etkinlik. Hariç tutulan ve bir iş akışında açıkça bunları bir Inlinescript bloğunda eklemediğiniz sürece kullanılamaz cmdlet'leri kümesi yok. Bu kavramlarla ilgili daha ayrıntılı bilgi için bkz: [betik iş akışlarında etkinlikleri kullanma](http://technet.microsoft.com/library/jj574194.aspx).

İş akışı etkinlikleri çalışmalarını yapılandıran ortak parametreler kümesini paylaşır. İş akışı ortak parametreleri hakkında daha fazla ayrıntı için bkz: [about_WorkflowCommonParameters](http://technet.microsoft.com/library/jj129719.aspx).

### <a name="positional-parameters"></a>Konumsal Parametreler
Konumsal parametreler, etkinlikler ve iş akışı cmdlet'leri ile kullanamazsınız.  Bunun anlamı tüm parametre adları kullanmasıdır.

Örneğin, tüm çalışan hizmetler alır aşağıdaki kodu göz önünde bulundurun.

     Get-Service | Where-Object {$_.Status -eq "Running"}

Bir iş akışında aynı bu kodu çalıştırmak çalışırsanız, "parametre kümesi belirtilen adlandırılmış parametreler kullanılarak çözümlenemiyor."gibi bir ileti alırsınız  Bu sorunu gidermek için aşağıdaki gibi parametre adı sağlayın.

    Workflow Get-RunningServices
    {
        Get-Service | Where-Object -FilterScript {$_.Status -eq "Running"}
    }

### <a name="deserialized-objects"></a>Seri durumdan çıkarılmış nesneleri
İş akışlarında nesneleri serisi.  Özellikleri hala kullanılabilir anlamına gelir, ancak bunların yöntemleri.  Örneğin, hizmet nesnesinin Stop yöntemi kullanarak bir hizmet durdurur aşağıdaki PowerShell kodu göz önünde bulundurun.

    $Service = Get-Service -Name MyService
    $Service.Stop()

Bu bir iş akışında çalıştırmayı denerseniz, "bir Windows PowerShell iş akışında yöntem çağırma desteklenmiyor." bildiren bir hata alıyorsunuz  

Bir seçenektir bu iki satır kod sarmalamak için bir [Inlinescript](#inlinescript) engelle; Bu durumda $Service bloğu içinde bir hizmet nesnesi olması.

    Workflow Stop-Service
    {
        InlineScript {
            $Service = Get-Service -Name MyService
            $Service.Stop()
        }
    }

Başka bir seçenek varsa yöntemi aynı işlevi gerçekleştirir başka bir cmdlet kullanmaktır.  Bizim örnek Hizmeti Durdur cmdlet Stop yöntemi ile aynı işlevselliği sağlar ve bir iş akışı için aşağıdakileri kullanabilirsiniz.

    Workflow Stop-MyService
    {
        $Service = Get-Service -Name MyService
        Stop-Service -Name $Service.Name
    }


## <a name="inlinescript"></a>Inlinescript
**Inlinescript** etkinlik, bir veya daha fazla komutu yerine PowerShell iş akışı gibi geleneksel PowerShell betiğini çalıştırmak gerektiğinde kullanışlıdır.  Bir iş akışındaki komutları için Windows Workflow Foundation işleme için gönderilirken, bir Inlinescript bloğundaki komutlar Windows PowerShell tarafından işlenir.

Inlinescript aşağıdaki aşağıdaki sözdizimini kullanır.

    InlineScript
    {
      <Script Block>
    } <Common Parameters>

Çıkışı bir değişkene atayarak bir Inlinescript çıkış döndürebilir. Aşağıdaki örnek, bir hizmetini durdurur ve hizmet adı çıkarır.

    Workflow Stop-MyService
    {
        $Output = InlineScript {
            $Service = Get-Service -Name MyService
            $Service.Stop()
            $Service
        }

        $Output.Name
    }


Bir Inlinescript bloğu içine değerlerinin geçmesini sağlayabilirsiniz, ancak kullanmalısınız **$Using** kapsam değiştiricisi.  Aşağıdaki örnek, hizmet adı değişkeni tarafından sağlanan dışında önceki örnekle aynıdır.

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


Inlinescript etkinlikleri belirli iş akışlarında kritik olabilir, ancak iş akışı yapıları desteklemez ve yalnızca aşağıdaki nedenlerle gerekli olduğunda kullanılmalıdır:

* Kullanamazsınız [kontrol noktaları](#checkpoints) bir Inlinescript bloğunun. Blokta bir hata meydana gelirse, blok başından devam gerekir.
* Kullanamazsınız [Paralel yürütme](#parallel-processing) bir InlineScriptBlock içinde.
* Inlinescript bloğunun tüm uzunluğu için Windows PowerShell oturumunu tuttuğu beri Inlinescript iş akışı ölçeklenebilirliğini etkiler.

Inlinescript kullanma hakkında daha fazla bilgi için bkz: [bir iş akışında Windows PowerShell komutlarını çalıştırma](http://technet.microsoft.com/library/jj574197.aspx) ve [about_ınlinescript](http://technet.microsoft.com/library/jj649082.aspx).

## <a name="parallel-processing"></a>Paralel işleme
Windows PowerShell iş akışlarının bir avantajı, bir komut kümesi yerine paralel sıralı olarak tipik bir betikteki gibi ile gerçekleştirmek için yeteneğidir.

Kullanabileceğiniz **paralel** eşzamanlı olarak çalıştıran birden çok komut içeren bir betik bloğu oluşturmak için anahtar sözcüğü. Bu, aşağıdaki aşağıdaki sözdizimini kullanır. Bu durumda, Activity1 ve Activity2 aynı anda başlar. Activity3 ancak Activity1 ve Activity2 yalnızca tamamladıktan sonra başlar.

    Parallel
    {
      <Activity1>
      <Activity2>
    }
    <Activity3>


Örneğin, bir ağ hedefe birden çok dosya Kopyala aşağıdaki PowerShell komutlarını göz önünde bulundurun.  Sonraki başlatılmadan önce kopyalama, bir dosya bitmesi gerekir böylece bu komutlar sırayla çalışır.     

    Copy-Item -Path C:\LocalPath\File1.txt -Destination \\NetworkPath\File1.txt
    Copy-Item -Path C:\LocalPath\File2.txt -Destination \\NetworkPath\File2.txt
    Copy-Item -Path C:\LocalPath\File3.txt -Destination \\NetworkPath\File3.txt

Bunların tümü aynı anda kopyalama işlemini başlatmak için aşağıdaki iş akışı bu aynı komutları paralel olarak çalışır.  Yalnızca tüm sonra kopyalanan tamamlama iletisi görüntülenir.

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


Kullanabileceğiniz **ForEach-Parallel** aynı anda bir koleksiyondaki her öğe için komutları işlemek üzere yapısı. Betik bloğundaki komutlar sırayla yürütülürken koleksiyondaki öğeler paralel olarak işlenir. Bu, aşağıdaki aşağıdaki sözdizimini kullanır. Bu durumda, Activity1 koleksiyondaki tüm öğeleri aynı anda başlar. Activity1 tamamlandıktan sonra her öğe için Activity2 başlatır. Activity3 ancak yalnızca tüm öğeler için Activity1 ve Activity2 tamamlandığında başlar.

    ForEach -Parallel ($<item> in $<collection>)
    {
      <Activity1>
      <Activity2>
    }
    <Activity3>

Aşağıdaki örnek, paralel olarak dosyaları kopyalanıyor önceki örneğe benzerdir.  Bu durumda, onu kopyaladıktan sonra her dosya için bir ileti görüntülenir.  Yalnızca tüm sonra tamamen kopyaladıktan son tamamlama ileti görüntülenir.

    Workflow Copy-Files
    {
        $files = @("C:\LocalPath\File1.txt","C:\LocalPath\File2.txt","C:\LocalPath\File3.txt")

        ForEach -Parallel ($File in $Files)
        {
            Copy-Item -Path $File -Destination \\NetworkPath
            Write-Output "$File copied."
        }

        Write-Output "All files copied."
    }

> [!NOTE]
> Bu güvenilir olmayan sonuçlar vermek için göstermiştir bu yana çalışan alt runbook'ları paralel olarak önermiyoruz.  Bazen alt runbook'tan çıkış gösterilmez ve bir alt runbook ayarlarında diğer paralel alt runbook'lar etkileyebilir
>

## <a name="checkpoints"></a>Kontrol noktaları
A *denetim noktası* değişkenlerin geçerli değerlerini ve bu noktaya kadar üretilen çıktıyı içerir iş akışının geçerli durumuna anlık görüntüsüdür. Bir iş akışı hata sona erer veya askıya alındı, ardından İleri çalıştırıldığında, akışı başlangıcı yerine en son denetim noktasından başlayacaktır.  İle bir iş akışında bir denetim noktası ayarlayabilirsiniz **Checkpoint-Workflow** etkinlik.

Aşağıdaki örnek kodda bir özel durum activity2 sonrasında sona erdirmek iş akışı neden olur. İş akışını yeniden çalıştırdığınızda, yalnızca son denetim noktasının ayarlandığı sonra bu yana Activity2 çalıştırarak başlatır.

    <Activity1>
    Checkpoint-Workflow
    <Activity2>
    <Exception>
    <Activity3>

Özel durum olabilecek ve gereken etkinliklerin iş akışı devam ettirildiğinde tekrarlanmaması sonra bir iş akışında denetim noktaları ayarlamanız gerekir. Örneğin, iş akışınızı bir sanal makine oluşturabilir. Önce ve sonra sanal makine oluşturma komutlarının bir denetim noktası ayarlayabilirsiniz. Oluşturma başarısız olursa, iş akışını yeniden başlatılırsa, komutlar tekrarlar. Oluşturma başarılı olduktan sonra akışı başarısız olursa, iş akışı sürdürüldüğünde sonra sanal makineyi yeniden oluşturulmaz.

Aşağıdaki örnek, birden çok dosyalarını bir ağ konumuna kopyalar ve sonra her bir dosyanın bir denetim noktası ayarlar.  Ağ konumu kaybolursa, iş akışı hata sona erer.  Yeniden başlatıldığında, önceden kopyaladığınız dosyalar atlanır anlamı son denetim noktası devam eder.

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

Çağırdıktan sonra kullanıcı adı kimlik bilgilerini kalıcı değildir çünkü [Suspend-Workflow](https://technet.microsoft.com/library/jj733586.aspx) etkinlik veya null ve sonra bunları yeniden sonra varlık deposundan almak için kimlik bilgilerini ayarlamak gereken en son kontrol sonra  **Suspend-Workflow** ya da kontrol noktası çağrılır.  Aksi takdirde, aşağıdaki hata iletisini alabilirsiniz: *iş akışının devam ettirilemez, Kalıcılık veri bırakılamadı tamamen kaydedildi, veya kaydedilen çünkü kalıcı veri ya da bozulmuş. İş akışını yeniden başlatmanız gerekir.*

Aşağıdaki aynı kod bu PowerShell iş akışı larınızda nasıl ele alınacağını gösterir.

    workflow CreateTestVms
    {
       $Cred = Get-AzureAutomationCredential -Name "MyCredential"
       $null = Add-AzureRmAccount -Credential $Cred

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
          $null = Add-AzureRmAccount -Credential $Cred
         }
     }


Hizmet sorumlusu ile yapılandırılmış olan bir farklı çalıştır hesabını kullanarak kimlik doğrulaması yaptıklarını, bu gerekli değildir.  

Kontrol noktaları hakkında daha fazla bilgi için bkz: [betik iş akışına denetim noktaları ekleme](http://technet.microsoft.com/library/jj574114.aspx).

## <a name="next-steps"></a>Sonraki adımlar
* PowerShell iş akışı runbook'larını kullanmaya başlamak için bkz. [İlk PowerShell iş akışı runbook uygulamam](automation-first-runbook-textual.md)
