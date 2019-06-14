---
title: 'Azure AD Connect eşitleme: İşletimsel görevler ve önemli noktalar | Microsoft Docs'
description: Bu konuda, Azure AD Connect eşitleme ve bu bileşen çalışma için hazırlama için işletimsel görevler açıklanmaktadır.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: ''
ms.assetid: b29c1790-37a3-470f-ab69-3cee824d220d
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/27/2018
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 176b8509892ef16b631697a686471e7fa52bb380
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60381595"
---
# <a name="azure-ad-connect-staging-server-and-disaster-recovery"></a>Azure AD Connect: Hazırlama sunucusu ve olağanüstü durum kurtarma
Hazırlama modunda bir sunucuyla yapılandırmada değişiklik yapmak ve sunucunun etkin hale getirmeden önce değişiklikleri Önizleme. Ayrıca tam içeri aktarma ve üretim ortamınıza bu değişiklikleri yapmadan önce tüm değişiklikleri beklendiğini doğrulamak için tam eşitleme çalıştırmanızı sağlar.

## <a name="staging-mode"></a>Hazırlama modu
Hazırlama modu gibi çeşitli senaryolar için kullanılabilir:

* Yüksek kullanılabilirlik.
* Test ve yeni yapılandırma değişiklikleri dağıtın.
* Yeni bir sunucu tanıtır ve eski yetkisini alma.

Yükleme sırasında sunucunun olması seçebileceğiniz **hazırlama modunda**. Bu işlem sunucunun içeri aktarma ve eşitleme için etkin hale getirir, ancak hiçbir dışarı aktarma işlemini çalıştırmaz. Yükleme sırasında bu özellikleri seçmiş olsanız parola eşitleme veya parola geri yazma, bir sunucu hazırlama modunda çalışmıyor. Hazırlama modunu devre dışı bıraktığınızda, sunucunun verme başlar, parola eşitleme sağlar ve parola geri yazma özelliğini etkinleştirir.

> [!NOTE]
> Bir Azure AD Connect parola karması eşitleme özelliği etkin olduğunu varsayalım. Hazırlama modunu, eşitleme parola değişiklikleri sunucusu vermiyor etkinleştirdiğinizde şirket içi AD. Hazırlama modunu devre dışı bıraktığınızda, sunucunun son kaldığı gelen parola değişikliklerinin eşitlemeyi sürdürür. Sunucu, uzun bir süre için hazırlama modunda bırakılırsa, zaman diliminde oluşmadı tüm parola değişiklikleri eşitlemek için sunucuya biraz sürebilir.
>
>

Eşitleme Hizmeti Yöneticisi'ni kullanarak, bir dışarı aktarma hala zorlayabilirsiniz.

Hazırlama modundaki bir sunucu, Active Directory ve Azure AD değişiklikleri almaya devam eder. Her zaman en son değişiklikleri ve çok hızlı can take bir kopyasını başka bir sunucuya sorumluluklarını sahiptir. Birincil sunucunuza yapılandırma değişiklikleri yaparsanız, bu sunucuyu hazırlama modunda aynı değişiklik yapmak için sizin sorumluluğunuzdur.

Kendi SQL veritabanı sunucusu olduğundan bu, eski eşitleme teknolojilerinin bilen hazırlama modunu farklılık gösterir. Bu mimari, farklı bir veri merkezinde yer alması hazırlık modu sunucusu sağlar.

### <a name="verify-the-configuration-of-a-server"></a>Bir sunucunun yapılandırmasını doğrulama
Bu yöntem uygulamak için aşağıdaki adımları izleyin:

1. [Hazırlama](#prepare)
2. [Yapılandırma](#configuration)
3. [İçeri aktarma ve eşitleme](#import-and-synchronize)
4. [Doğrulayın](#verify)
5. [Etkin sunucu anahtarı](#switch-active-server)

#### <a name="prepare"></a>Hazırlama
1. Azure AD Connect'i yüklemek, seçin **hazırlama modunda**, seçimini temizleyin **eşitleme başlatma** Yükleme Sihirbazı'nda son sayfasında. Bu mod, el ile eşitleme altyapısı çalıştırmanıza olanak sağlar.
   ![ReadyToConfigure](./media/how-to-connect-sync-staging-server/readytoconfigure.png)
2. Oturum kapatma/oturum içinde ve başlangıç menüsünde seçin **eşitleme hizmeti**.

#### <a name="configuration"></a>Yapılandırma
Birincil sunucuya özel değişiklikler yaptınız ve hazırlık sunucusu yapılandırmasıyla karşılaştırmak istediğinizde, ardından kullanmak [Azure AD Connect yapılandırma Belgeleyici'yi](https://github.com/Microsoft/AADConnectConfigDocumenter).

#### <a name="import-and-synchronize"></a>İçeri aktarma ve eşitleme
1. Seçin **Bağlayıcılar**ve ilk bağlayıcı türü olan seçin **Active Directory Domain Services**. Tıklayın **çalıştırma**seçin **tam içeri aktarma**, ve **Tamam**. Bu türün tüm bağlayıcıları için bu adımları uygulayın.
2. Bağlayıcı türü olan seçin **Azure Active Directory (Microsoft)** . Tıklayın **çalıştırma**seçin **tam içeri aktarma**, ve **Tamam**.
3. Bağlayıcılar sekmesi seçili olduğundan emin olun. Her bağlayıcı türü olan **Active Directory Domain Services**, tıklayın **çalıştırmak**seçin **Delta eşitlemesi**, ve **Tamam**.
4. Bağlayıcı türü olan seçin **Azure Active Directory (Microsoft)** . Tıklayın **çalıştırma**seçin **Delta eşitlemesi**, ve **Tamam**.

Artık aşamalı dışa aktarma değişiklikler Azure AD'ye ve AD (Exchange karma dağıtımı kullanıyorsanız) şirket. Sonraki adımlar ne gerçekten dizinleri ver başlamadan önce değişmek üzere olduğunu denetlemek sağlar.

#### <a name="verify"></a>Doğrulama
1. Bir komut istemi açın ve gidin `%ProgramFiles%\Microsoft Azure AD Sync\bin`
2. Çalıştırın: `csexport "Name of Connector" %temp%\export.xml /f:x` Bağlayıcısının eşitleme hizmetinde bulunamadı. "Contoso.com – AAD" benzer bir adı varsa Azure AD için.
3. Çalıştırın: `CSExportAnalyzer %temp%\export.xml > %temp%\export.csv` Microsoft Excel'de incelenebilir export.csv adlı % temp % içinde bir dosya var. Bu dosya, dışarı aktarılacak olan tüm değişiklikleri içerir.
4. Veriler veya yapılandırma için gerekli değişiklikleri yapın ve şu adımları yeniden (içeri aktarma ve eşitleme ve doğrulama) çalıştırın, dışarı aktarılacak olan değişiklikleri beklendiği kadar.

**C:\Export.csv anlama** dosya çoğu açıklayıcıdır. İçerik anlamak için bazı kısaltmalar:
* OMODT – nesne değişiklik türü. Bir ekleme, güncelleştirme veya silme işlemi bir nesne düzeyinde olup olmadığını gösterir.
* AMODT – öznitelik değişiklik türü. Bir ekleme, güncelleştirme veya silme işlemi düzeyinde bir öznitelik olup olmadığını gösterir.

**Ortak tanımlayıcıları almak** c:\Export.csv dışarı aktarılacak olan tüm değişiklikleri içerir. Her satır bağlayıcı alanında bir nesne için bir değişiklik karşılık gelir ve nesne DN özniteliği tarafından tanımlanır. DN öznitelik bağlayıcı alanında bir nesneye atanmış benzersiz bir tanımlayıcıdır. Analiz etmek için export.csv birçok satır/değişiklik olduğunda, kullanıma nesneleri DN özniteliği tek başına göre değişiklikler tahmin etmek zor olabilir. Değişiklikleri çözümleme sürecini basitleştirmek için csanalyzer.ps1 PowerShell betiğini kullanın. Komut nesneleri ortak tanımlayıcıları (örneğin, displayName, userPrincipalName) alır. Betiği kullanmak için:
1. PowerShell Betiği bölümünden kopyalayın [CSAnalyzer](#appendix-csanalyzer) adlı bir dosyaya `csanalyzer.ps1`.
2. Bir PowerShell penceresi açın ve PowerShell komut dosyasını oluşturduğunuz klasöre göz atın.
3. Çalıştır: `.\csanalyzer.ps1 -xmltoimport %temp%\export.xml`.
4. Adlı bir dosya artık sahip **processedusers1.csv** Microsoft Excel'de incelenebilir. Dosyanın ortak tanımlayıcıları (örneğin, displayName ve userPrincipalName seçeneği) DN özniteliği bir eşleme sağladığını unutmayın. Şu anda dışarı aktarılacak olan gerçek öznitelik değişiklikleri içermez.

#### <a name="switch-active-server"></a>Etkin sunucu anahtarı
1. Şu anda etkin sunucu üzerinde değil verme için sunucudan (FIM/DirSync/Azure AD eşitleme) Azure AD'ye açma veya hazırlama modu (Azure AD Connect) olarak ayarlayın.
2. Sunucuda yükleme sihirbazını çalıştırın **hazırlama modunda** ve devre dışı bırakma **hazırlama modunda**.
   ![ReadyToConfigure](./media/how-to-connect-sync-staging-server/additionaltasks.png)

## <a name="disaster-recovery"></a>Olağanüstü durum kurtarma
Uygulama tasarımının parçasını ne durumunda olağanüstü bir durum eşitleme sunucusu kaybetmek nerede yapılacağını planlamaktır. Ve hangisinin kullanılacağını dahil olmak üzere çeşitli etkenlere bağlıdır farklı modeli vardır:

* Nesneler değişiklik yapabilir yapma kapalı kalma süresinde Azure AD'de olmaması, dayanıklılık nedir?
* Parola Eşitleme kullanırsanız, kullanıcılar şirket içi değiştirdiğinden durumunda Azure AD'de eski parolayı kullanmak sahip oldukları kabul ediyor musunuz?
* Parola geri yazma gibi gerçek zamanlı işlemleri üzerinde bir bağımlılık gerekiyor?

Bu soruları ve kuruluşunuzun ilkesini yanıtlarını bağlı olarak, aşağıdaki stratejilerden birini uygulanabilir:

* Gerektiğinde yeniden oluşturun.
* Sahip olarak bilinen bir yedek bir bekleme sunucusunun **hazırlama modunda**.
* Sanal makineler kullanır.

Yerleşik SQL Express veritabanı kullanmayın sonra da gözden geçirmelisiniz [SQL yüksek kullanılabilirlik](#sql-high-availability) bölümü.

### <a name="rebuild-when-needed"></a>Gerektiğinde yeniden oluşturun
Gerektiğinde bir sunucusu yeniden oluşturma için uygun bir strateji planlamaktır. Genellikle, ilk içeri aktarma ve eşitleme birkaç saat içinde tamamlanabilir yapın ve eşitleme altyapısını yükleme. Kullanılabilir yedek bir sunucu değilse geçici olarak eşitleme altyapısı barındırmak için bir etki alanı denetleyicisi kullanmak da mümkündür.

Veritabanı verileri Active Directory ve Azure AD kullanarak yeniden bu nedenle eşitleme altyapısı sunucusuna nesneler hakkında herhangi bir durumu depolamaz. **SourceAnchor** özniteliği, şirket içi ve bulut nesneleri birleştirmek için kullanılır. Var olan nesneleri şirket içi ve bulut sunucuyla yeniden oluşturursanız, ardından eşitleme altyapısı nesnelerle birlikte yeniden üzerinde yeniden eşleşir. Belge ve kaydetmek için ihtiyaç duyduğunuz filtreleme ve eşitleme kuralları gibi sunucusunda yapılan yapılandırma değişiklikleri noktalardır. Eşitlemeye başlamadan önce bu özel yapılandırmaları yeniden gerekir.

### <a name="have-a-spare-standby-server---staging-mode"></a>Hazırlama modu yedek bir bekleme sunucusunun - yüklü
Daha karmaşık bir ortam varsa, ardından bir veya daha fazla yedek sunucular olması önerilir. Yükleme sırasında bir sunucu olması etkinleştirebilirsiniz **hazırlama modunda**.

Daha fazla bilgi için [hazırlama modunda](#staging-mode).

### <a name="use-virtual-machines"></a>Sanal makineler kullanın
Bir ortak ve desteklenen yöntem, bir sanal makinede eşitleme altyapısı çalıştırmaktır. Konak bir sorun olması durumunda, eşitleme altyapısı sunucusuna sahip bir görüntü başka bir sunucuya geçirilebilir.

### <a name="sql-high-availability"></a>SQL yüksek kullanılabilirlik
Azure AD Connect ile birlikte sunulan SQL Server Express kullanmıyorsanız, yüksek kullanılabilirlik için SQL Server ayrıca düşünülmelidir. Desteklenen yüksek kullanılabilirlik çözümleri SQL Kümeleme ve AOA (Always On kullanılabilirlik grupları) içerir. Yansıtma desteklenmeyen çözümleri içerir.

Azure AD Connect sürüm 1.1.524.0 SQL AOA için destek eklendi. Azure AD Connect'i yüklemeden önce SQL AOA etkinleştirmeniz gerekir. Yükleme sırasında Azure AD Connect veya sağlanan SQL örneğinin SQL AOA için etkin olup olmadığını algılar. SQL AOA etkinleştirilirse, daha fazla Azure AD Connect SQL AOA zaman uyumlu veya zaman uyumsuz çoğaltma kullanacak şekilde yapılandırıldı, rakamları. Kullanılabilirlik grubu dinleyicisi Kur ayarlarken RegisterAllProvidersIP özelliğini 0 olarak ayarlayın, önerilir. Bu Azure AD Connect, SQL Native Client SQL'e bağlanmak için şu anda kullanır ve SQL Native Client MultiSubNetFailover özelliğinin kullanılmasını desteklemiyor olmasıdır.

## <a name="appendix-csanalyzer"></a>Ek CSAnalyzer
Bölümüne bakın [doğrulayın](#verify) nasıl bu betiği kullanın.

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
$result=$reader = [System.Xml.XmlReader]::Create($resolvedXMLtoimport) 
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

        #export the collection of users as a CSV
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
#export the collection of users as CSV
Write-Host Writing processedusers${outputfilecount}.csv -ForegroundColor Yellow
$objOutputUsers | Export-Csv -path processedusers${outputfilecount}.csv -NoTypeInformation
```

## <a name="next-steps"></a>Sonraki adımlar
**Genel bakış konuları**  

* [Azure AD Connect eşitlemesi: Anlama ve eşitleme özelleştirme](how-to-connect-sync-whatis.md)  
* [Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](whatis-hybrid-identity.md)  
