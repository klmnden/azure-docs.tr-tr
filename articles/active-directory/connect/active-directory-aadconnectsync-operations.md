---
title: "Azure AD Connect eşitleme: işletimsel görevleri ve ilgili önemli noktalar | Microsoft Docs"
description: "Bu konuda, Azure AD Connect eşitleme ve bu bileşen işletim için hazırlamak nasıl işletimsel görevler açıklanmaktadır."
services: active-directory
documentationcenter: 
author: AndKjell
manager: femila
editor: 
ms.assetid: b29c1790-37a3-470f-ab69-3cee824d220d
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/13/2017
ms.author: billmath
ms.openlocfilehash: 89bfedd282d04569bcf873fd7a9082791a94376b
ms.sourcegitcommit: 62eaa376437687de4ef2e325ac3d7e195d158f9f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/22/2017
---
# <a name="azure-ad-connect-sync-operational-tasks-and-consideration"></a>Azure AD Connect eşitleme: işletimsel görevleri ve değerlendirme
Bu konunun amacı, Azure AD Connect eşitleme için işletimsel görevleri açıklar sağlamaktır.

## <a name="staging-mode"></a>Hazırlama modu
Hazırlama modu dahil olmak üzere çeşitli senaryoları için kullanılabilir:

* Yüksek kullanılabilirlik.
* Test ve yeni yapılandırma değişikliği dağıtın.
* Yeni bir sunucu getirir ve eski yetkisini alın.

Hazırlama modunda bir sunucuyla yapılandırma değişiklikleri yapın ve sunucunun etkin hale getirmeden önce önizleme değişiklikleri. Ayrıca, tam içeri aktarma ve üretim ortamınıza bu değişiklikleri yapmadan önce tüm değişiklikleri beklenen doğrulamak için tam eşitleme çalıştırmanızı sağlar.

Yükleme sırasında sunucuyu içinde olacak şekilde seçebileceğiniz **hazırlama modu**. Bu eylem sunucunun içeri aktarma ve eşitleme için etkin hale getirir, ancak hiçbir dışarı aktarma çalışmaz. Bu özellikler yüklemesi sırasında seçilen olsa bile sunucu hazırlama modunda bir parola eşitleme ya da parola geri yazma çalışmıyor. Hazırlama modunu devre dışı bıraktığınızda, sunucu dışarı aktarma başlatır, parola eşitleme sağlar ve parola geri yazma özelliğini etkinleştirir.

> [!NOTE]
> Parola karma Eşitlemesi özelliği etkin bir Azure AD Connect olduğunu varsayalım. Hazırlama modunu, eşitleme parola değişiklikleri sunucusu vermiyor etkinleştirdiğinizde şirket içi AD. Hazırlama modunu devre dışı bıraktığınızda, sunucunun son kaldığı yerden gelen parola değişikliklerini eşitlemeyi sürdürür. Sunucu uzun bir süre için hazırlama modunda bırakılırsa sunucusunun süre içinde oluşan tüm parola değişiklikleri eşitlemek biraz zaman alabilir.
>
>

Eşitleme Hizmeti Yöneticisi'ni kullanarak bir verme hala zorlayabilirsiniz.

Hazırlama modundaki bir sunucu, Active Directory ve Azure AD değişiklikleri almaya devam eder. Her zaman en son değişiklikleri ve can çok hızlı Al bir kopyasını başka bir sunucuya sorumlulukları vardır. Birincil sunucunuza yapılandırma değişikliklerini yapın, hazırlama modunda sunucunun aynı değişiklikler yapmak üzere sizin sorumluluğunuzdadır olur.

Sunucunun kendi SQL veritabanı olduğundan bu, eski eşitleme teknolojilerinin bilgi ile hazırlama modunu farklıdır. Bu mimari hazırlama modu sunucusunun farklı bir veri merkezinde bulunan izin verir.

### <a name="verify-the-configuration-of-a-server"></a>Bir sunucunun yapılandırmasını doğrulama
Bu yöntem uygulamak için aşağıdaki adımları izleyin:

1. [Hazırlama](#prepare)
2. [Yapılandırma](#configuration)
3. [İçeri aktarma ve eşitleme](#import-and-synchronize)
4. [Doğrulayın](#verify)
5. [Anahtar active server](#switch-active-server)

#### <a name="prepare"></a>Hazırlama
1. Azure AD Connect'i yüklemek, seçin **hazırlama modu**ve işaretini **Eşitlemeyi Başlat** Yükleme Sihirbazı'nda son sayfasında. Bu mod el ile eşitleme altyapısı çalıştırmanıza olanak sağlar.
   ![ReadyToConfigure](./media/active-directory-aadconnectsync-operations/readytoconfigure.png)
2. Oturum Kapat/oturum Başlat menüsünden öğesini seçin ve içinde **eşitleme hizmeti**.

#### <a name="configuration"></a>Yapılandırma
Özel değişiklikleri birincil sunucuya yapmış ve hazırlama sunucu yapılandırmasıyla karşılaştırmak istediğiniz, ardından kullanın [Azure AD Connect yapılandırma Belgeleyici'yi](https://github.com/Microsoft/AADConnectConfigDocumenter).

#### <a name="import-and-synchronize"></a>İçeri aktarma ve eşitleme
1. Seçin **Bağlayıcılar**ve ilk bağlayıcı türü olan seçin **Active Directory etki alanı Hizmetleri**. Tıklatın **çalıştırmak**seçin **tam alma**, ve **Tamam**. Bu türdeki tüm bağlayıcıları için bu adımları uygulayın.
2. Bağlayıcı türü olan seçin **Azure Active Directory (Microsoft)**. Tıklatın **çalıştırmak**seçin **tam alma**, ve **Tamam**.
3. Bağlayıcılar sekmesi seçili olduğundan emin olun. Her bağlayıcı türü olan **Active Directory etki alanı Hizmetleri**, tıklatın **çalıştırmak**seçin **Delta eşitlemesi**, ve **Tamam**.
4. Bağlayıcı türü olan seçin **Azure Active Directory (Microsoft)**. Tıklatın **çalıştırmak**seçin **Delta eşitlemesi**, ve **Tamam**.

Şimdi hazırlanan verme Azure AD ile değiştirir ve AD (Exchange karma dağıtımı kullanıyorsanız) şirket. Sonraki adımlar, aslında dizinleri ver başlamadan önce değiştirilmek nedir incelemek izin verin.

#### <a name="verify"></a>Doğrulama
1. Bir komut istemi açın ve gidin`%ProgramFiles%\Microsoft Azure AD Sync\bin`
2. Çalıştır: `csexport "Name of Connector" %temp%\export.xml /f:x` bağlayıcısının adını eşitleme hizmeti bulunamadı. "Contoso.com için – AAD" benzer bir ada sahip Azure AD için.
3. Çalıştır: `CSExportAnalyzer %temp%\export.xml > %temp%\export.csv` Microsoft Excel'de incelenebilir export.csv adlı % temp % içinde bir dosyanız. Bu dosya dışarı aktarılacak olan tüm değişiklikleri içerir.
4. Veriler veya yapılandırma gerekli değişiklikleri yapın ve bu adımları tekrar (içeri aktarma ve eşitleme ve doğrula) çalıştırın, dışarı aktarılacak olan değişiklikleri beklenen kadar.

**C:\Export.csv anlama** dosyasının çoğu kendinden açıklamalıdır. İçerik anlamak üzere bazı kısaltmalar:
* OMODT – nesne değişikliği türü. Nesne düzeyinde bir işlemi ekleme, güncelleştirme veya silme olup olmadığını gösterir.
* AMODT – öznitelik değişikliği türü. Bir öznitelik düzeyinde işlemi ekleme, güncelleştirme veya silme olup olmadığını gösterir.

**Ortak tanımlayıcılarının alınması** export.csv dosyası dışarı aktarılacak olan tüm değişiklikleri içerir. Her satır bir bağlayıcı alanı nesne için bir değişikliği karşılık gelir ve nesne DN özniteliği tarafından tanımlanır. Bağlayıcı alanı içindeki bir nesneye atanmış benzersiz bir tanımlayıcı DN özniteliğidir. Çözümlenecek export.csv fazla satır/değişiklik olduğunda, çıkışı nesneleri DN özniteliği tek başına göre değişir şekil zor olabilir. Değişiklikleri analiz etme işlemini basitleştirmek için csanalyzer.ps1 PowerShell komut dosyasını kullanın. Komut dosyası nesnelerin ortak tanımlayıcıları (örneğin, displayName, userPrincipalName) alır. Betik kullanmak için:
1. PowerShell Betiği bölümünden kopyalayın [CSAnalyzer](#appendix-csanalyzer) adlı bir dosyaya `csanalyzer.ps1`.
2. Bir PowerShell penceresi açın ve PowerShell komut dosyasını oluşturduğunuz klasöre göz atın.
3. Çalıştır: `.\csanalyzer.ps1 -xmltoimport %temp%\export.xml`.
4. Şimdi adlı bir dosyanız varsa **processedusers1.csv** , incelenmesi Microsoft Excel'de. Dosya DN özniteliği eşlemeyi ortak tanımlayıcıları (örneğin, displayName ve userPrincipalName) sağladığını unutmayın. Şu anda dışarı aktarılacak gerçek öznitelik değişikliklerini içermez.

#### <a name="switch-active-server"></a>Anahtar active server
1. Şu anda etkin sunucuda değil verme için Azure AD ile (FIM/DirSync/Azure AD eşitleme) sunucu etkinleştirmek ya da hazırlama modu (Azure AD Connect) ayarlayın.
2. Sunucuda Yükleme Sihirbazı'nı çalıştırın **hazırlama modu** ve devre dışı bırakma **hazırlama modu**.
   ![ReadyToConfigure](./media/active-directory-aadconnectsync-operations/additionaltasks.png)

## <a name="disaster-recovery"></a>Olağanüstü durum kurtarma
Uygulama tasarımı durumunda bir olağanüstü durum burada, eşitleme sunucusu kaybetmeniz yapmanız gerekenler için plana parçasıdır. Farklı modelleri ve hangisinin kullanılacağını gibi çeşitli etmenlere bağlıdır vardır:

* Nesnelere değişiklikler yapabilir yapabilir, kapalı kalma süresi sırasında Azure AD olmaması için dayanıklılık nedir?
* Parola Eşitleme kullanırsanız, kullanıcıların bunlar şirket içi değişiklik durumda eski parola Azure AD'de kullanmak zorunda olduğunu kabul ediyor musunuz?
* Parola geri yazma gibi gerçek zamanlı işlemler üzerinde bir bağımlılık var mı?

Bu sorular ve kuruluşunuzun ilkesini yanıtlar bağlı olarak, aşağıdaki stratejilerden birini uygulanabilir:

* Gerektiğinde yeniden oluşturun.
* Bilinen bir yedek bir bekleme sunucusunun **hazırlama modu**.
* Sanal makineler kullanır.

Yerleşik SQL Express veritabanı kullanmayın sonra da gözden geçirmelisiniz [SQL yüksek kullanılabilirlik](#sql-high-availability) bölümü.

### <a name="rebuild-when-needed"></a>Gerektiğinde yeniden oluşturma
Gerektiğinde bir sunucusu yeniden oluşturma için planlama uygun bir stratejidir. Genellikle, ilk içe aktarma ve eşitleme birkaç saat içinde tamamlanabilir yapın ve eşitleme altyapısı yükleme. Kullanılabilir bir yedek sunucu değilse geçici olarak eşitleme altyapısı barındırmak için bir etki alanı denetleyicisi kullanmak da mümkündür.

Active Directory ve Azure AD verilerden veritabanı yeniden şekilde eşitleme altyapısı sunucusu nesneler hakkında herhangi bir durum depolamaz. **SourceAnchor** özniteliği, şirket içi ve bulut nesnelerden katılmak için kullanılır. Varolan nesneleri şirket içi ve bulut sunucusuyla yeniden oluşturursanız, daha sonra eşitleme altyapısı nesnelerle birlikte yeniden üzerinde yeniden eşleşir. Belge ve kaydetmek için gereken filtreleme ve eşitleme kuralları gibi sunucusuna yapılan yapılandırma değişikliklerinin noktalardır. Eşitlemeye başlamadan önce bu özel yapılandırmalar yeniden gerekir.

### <a name="have-a-spare-standby-server---staging-mode"></a>Hazırlama modu yedek bir bekleme sunucusunun-
Daha karmaşık bir ortamınız varsa, bir veya daha fazla yedek sunucular olması önerilir. Yükleme sırasında bir sunucu olması etkinleştirebilirsiniz **hazırlama modu**.

Daha fazla bilgi için bkz: [hazırlama modu](#staging-mode).

### <a name="use-virtual-machines"></a>Sanal makineler kullanın
Bir ortak ve desteklenen yöntem, bir sanal makinede eşitleme altyapısı çalıştırmaktır. Konak bir sorun var. durumunda görüntünün eşitleme altyapısı sunucusu ile başka bir sunucuya geçirilemez.

### <a name="sql-high-availability"></a>SQL yüksek kullanılabilirlik
Azure AD Connect ile birlikte gelen SQL Server Express kullanmıyorsanız sonra SQL Server için yüksek kullanılabilirlik da dikkate alınmalıdır. Desteklenen yüksek kullanılabilirlik çözümleri SQL Kümeleme ve AOA (Always On kullanılabilirlik grupları) içerir. Yansıtma desteklenmeyen çözümleri içerir.

Azure AD Connect sürümünde 1.1.524.0 SQL AOA için destek eklendi. Azure AD Connect yüklemeden önce SQL AOA etkinleştirmeniz gerekir. Yükleme sırasında Azure AD Connect veya sağlanan SQL örneği için SQL AOA etkin olup olmadığını algılar. SQL AOA etkinleştirilirse, daha fazla Azure AD Connect SQL AOA zaman uyumlu veya zaman uyumsuz çoğaltma kullanacak şekilde yapılandırılıp yapılandırılmadığını rakamlar. Kullanılabilirlik grubu dinleyicisi Kur ayarlarken RegisterAllProvidersIP özelliği 0 olarak ayarlayın önerilir. SQL Native Client MultiSubNetFailover özelliğinin kullanımını desteklemez ve Azure AD Connect, SQL Native Client SQL bağlanmak için şu anda kullandığı nedeni budur.

## <a name="appendix-csanalyzer"></a>Ek CSAnalyzer
Bölümüne bakın [doğrulayın](#verify) bu komut dosyası kullanma.

```
Param(
    [Parameter(Mandatory=$true, HelpMessage="Must be a file generated using csexport 'Name of Connector' export.xml /f:x)")]
    [string]$xmltoimport="%temp%\exportedStage1a.xml",
    [Parameter(Mandatory=$false, HelpMessage="Maximum number of users per output file")][int]$batchsize=1000,
    [Parameter(Mandatory=$false, HelpMessage="Show console output")][bool]$showOutput=$false
)

#LINQ isn't loaded automatically, so force it
[Reflection.Assembly]::Load("System.Xml.Linq, Version=3.5.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089") | Out-Null

[int]$count=1
[int]$outputfilecount=1
[array]$objOutputUsers=@()

#XML must be generated using "csexport "Name of Connector" export.xml /f:x"
write-host "Importing XML" -ForegroundColor Yellow

#XmlReader.Create won't properly resolve the file location,
#so expand and then resolve it
$resolvedXMLtoimport=Resolve-Path -Path ([Environment]::ExpandEnvironmentVariables($xmltoimport))

#use an XmlReader to deal with even large files
$result=$reader = [System.Xml.XmlReader]::Create($resolvedXMLtoimport) 
$result=$reader.ReadToDescendant('cs-object')
do 
{
    #create the object placeholder
    #adding them up here means we can enforce consistency
    $objOutputUser=New-Object psobject
    Add-Member -InputObject $objOutputUser -MemberType NoteProperty -Name ID -Value ""
    Add-Member -InputObject $objOutputUser -MemberType NoteProperty -Name Type -Value ""
    Add-Member -inputobject $objOutputUser -MemberType NoteProperty -Name DN -Value ""
    Add-Member -inputobject $objOutputUser -MemberType NoteProperty -Name operation -Value ""
    Add-Member -inputobject $objOutputUser -MemberType NoteProperty -Name UPN -Value ""
    Add-Member -inputobject $objOutputUser -MemberType NoteProperty -Name displayName -Value ""
    Add-Member -inputobject $objOutputUser -MemberType NoteProperty -Name sourceAnchor -Value ""
    Add-Member -inputobject $objOutputUser -MemberType NoteProperty -Name alias -Value ""
    Add-Member -inputobject $objOutputUser -MemberType NoteProperty -Name primarySMTP -Value ""
    Add-Member -inputobject $objOutputUser -MemberType NoteProperty -Name onPremisesSamAccountName -Value ""
    Add-Member -inputobject $objOutputUser -MemberType NoteProperty -Name mail -Value ""

    $user = [System.Xml.Linq.XElement]::ReadFrom($reader)
    if ($showOutput) {Write-Host Found an exported object... -ForegroundColor Green}

    #object id
    $outID=$user.Attribute('id').Value
    if ($showOutput) {Write-Host ID: $outID}
    $objOutputUser.ID=$outID

    #object type
    $outType=$user.Attribute('object-type').Value
    if ($showOutput) {Write-Host Type: $outType}
    $objOutputUser.Type=$outType

    #dn
    $outDN= $user.Element('unapplied-export').Element('delta').Attribute('dn').Value
    if ($showOutput) {Write-Host DN: $outDN}
    $objOutputUser.DN=$outDN

    #operation
    $outOperation= $user.Element('unapplied-export').Element('delta').Attribute('operation').Value
    if ($showOutput) {Write-Host Operation: $outOperation}
    $objOutputUser.operation=$outOperation

    #now that we have the basics, go get the details

    foreach ($attr in $user.Element('unapplied-export-hologram').Element('entry').Elements("attr"))
    {
        $attrvalue=$attr.Attribute('name').Value
        $internalvalue= $attr.Element('value').Value

        switch ($attrvalue)
        {
            "userPrincipalName"
            {
                if ($showOutput) {Write-Host UPN: $internalvalue}
                $objOutputUser.UPN=$internalvalue
            }
            "displayName"
            {
                if ($showOutput) {Write-Host displayName: $internalvalue}
                $objOutputUser.displayName=$internalvalue
            }
            "sourceAnchor"
            {
                if ($showOutput) {Write-Host sourceAnchor: $internalvalue}
                $objOutputUser.sourceAnchor=$internalvalue
            }
            "alias"
            {
                if ($showOutput) {Write-Host alias: $internalvalue}
                $objOutputUser.alias=$internalvalue
            }
            "proxyAddresses"
            {
                if ($showOutput) {Write-Host primarySMTP: ($internalvalue -replace "SMTP:","")}
                $objOutputUser.primarySMTP=$internalvalue -replace "SMTP:",""
            }
        }
    }

    $objOutputUsers += $objOutputUser

    Write-Progress -activity "Processing ${xmltoimport} in batches of ${batchsize}" -status "Batch ${outputfilecount}: " -percentComplete (($objOutputUsers.Count / $batchsize) * 100)

    #every so often, dump the processed users in case we blow up somewhere
    if ($count % $batchsize -eq 0)
    {
        Write-Host Hit the maximum users processed without completion... -ForegroundColor Yellow

        #export the collection of users as as CSV
        Write-Host Writing processedusers${outputfilecount}.csv -ForegroundColor Yellow
        $objOutputUsers | Export-Csv -path processedusers${outputfilecount}.csv -NoTypeInformation

        #increment the output file counter
        $outputfilecount+=1

        #reset the collection and the user counter
        $objOutputUsers = $null
        $count=0
    }

    $count+=1

    #need to bail out of the loop if no more users to process
    if ($reader.NodeType -eq [System.Xml.XmlNodeType]::EndElement)
    {
        break
    }

} while ($reader.Read)

#need to write out any users that didn't get picked up in a batch of 1000
#export the collection of users as as CSV
Write-Host Writing processedusers${outputfilecount}.csv -ForegroundColor Yellow
$objOutputUsers | Export-Csv -path processedusers${outputfilecount}.csv -NoTypeInformation
```

## <a name="next-steps"></a>Sonraki adımlar
**Genel bakış konuları**  

* [Azure AD Connect eşitleme: anlamak ve eşitleme özelleştirme](active-directory-aadconnectsync-whatis.md)  
* [Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](active-directory-aadconnect.md)  
