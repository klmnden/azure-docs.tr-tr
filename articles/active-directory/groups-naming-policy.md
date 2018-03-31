---
title: Ad için Grup İlkesi ayarları Office 365 grupları Azure Active Directory'de (Önizleme) | Microsoft Docs
description: Office 365 grupları Azure Active Directory'de (Önizleme) için sona erme kurma
services: active-directory
documentationcenter: ''
author: curtand
manager: michael.tillman
editor: ''
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: article
ms.date: 03/29/2018
ms.author: curtand
ms.reviewer: kairaz.contractor
ms.custom: it-pro
ms.openlocfilehash: c21706a591d0e1aa00279edf7a5534ada95fd8c1
ms.sourcegitcommit: 34e0b4a7427f9d2a74164a18c3063c8be967b194
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/30/2018
---
# <a name="enforce-a-naming-policy-for-office-365-groups-in-azure-active-directory-preview"></a>Office 365 grupları Azure Active Directory'de (Önizleme) için adlandırma ilkesini zorunlu kılma

Oluşturulan veya kullanıcılarınız tarafından düzenlenmiş Office 365 grupları için tutarlı adlandırma kurallarını zorlamak için bir Grup İlkesi Azure Active Directory'de (Azure AD), kiracılar için adlandırma ayarlayın. Örneğin, bir grup üyeliği, coğrafi bölge işlevi iletişim kurmak için adlandırma ilkesini kullanabilir veya grup oluşturan kişiyi. Adres Defteri'ni gruplarında kategorilere ayırma yardımcı olmak için adlandırma İlkesi'ni de kullanabilirsiniz. Grup adları ve diğer adları kullanılmasını belirli kelimeleri engellemek için ilke kullanabilirsiniz.

> [!IMPORTANT]
> Office 365 grupları adlandırma ilkesi Preview sürümünü kullanarak bir veya daha fazla Office 365 grupların üyesi olan her benzersiz bir kullanıcı için Azure Active Directory Premium P1 lisansları veya Azure AD temel eğitim lisansı gerektirir.

Oluşturma veya düzenleme (örneğin, Outlook, Microsoft Teams, SharePoint, Exchange veya Planlayıcısı) iş yüklerinde oluşturulan grupları adlandırma ilkesi uygulanır. Grup adı ve grup diğer adı için uygulanır. Azure AD'de adlandırma ilkenizi ayarlayın ve varolan bir Exchange Grup İlkesi adlandırma varsa, ilke adlandırma Azure AD uygulanır.

## <a name="naming-policy-features"></a>Adlandırma ilkesi özellikleri
Office 365 grupları için adlandırma ilke iki farklı şekillerde uygulayabilir:

-   **Önek sonek adlandırma ilkesi** önekleri veya sonra otomatik olarak gruplarınızı adlandırma kuralı zorlamak için eklenen sonekleri tanımlayabilirsiniz (örneğin, grup adı'nda "GRP\_JAPONYA\_grubum\_ Mühendislik", GRP\_JAPONYA\_ öneki ve \_mühendislik soneki olan). 

-   **Özel engellenen sözcükleri** (örneğin "CEO, bordro, HR") kullanıcılar tarafından oluşturulan grupları engellenmesi kuruluşunuza engellenen sözcükler belirli bir dizi karşıya yükleyebilirsiniz.

### <a name="prefix-suffix-naming-policy"></a>Önek sonek adlandırma ilkesi

Genel adlandırma kuralı 'Önek [GroupName] sonek' yapısıdır. Birden çok önek ve sonek tanımlayabilirsiniz, ancak yalnızca [GroupName] tek bir örneğini ayarına sahip olabilir. Ön ekler veya sonekleri sabit dizeler veya kullanıcı öznitelikleri gibi olabilir \[departmanı\] , yerine grubu oluşturmayı kullanıcının göre. İzin verilen toplam birleştirilmiş, önek ve sonek dizeler için karakter sayısı 53 karakterdir. 

Önek ve sonek grup adı ve grup diğer desteklenen özel karakterleri içerebilir. Grup diğer desteklenmeyen herhangi bir karakteri öneki veya soneki hala grup adı'uygulanır, ancak Grup diğer adından kaldırıldı. Bu sınırlama nedeniyle, önek ve sonek grubun adını uygulanan Grup diğer uygulanan gördüğünüzden farklı olabilir. 

#### <a name="fixed-strings"></a>Sabit Dizeler

Dizeleri tarama ve grupları genel adres listesinde ve Grup iş yüklerinin sol gezinti bağlantıları ayırt daha kolay hale getirmek için kullanabilirsiniz. Gibi bazı ortak önekleri sözcükler ' Grp\_adı ', '\#adı ','\_adı '

#### <a name="user-attributes"></a>Kullanıcı öznitelikleri

Yardımcı olabilecek özniteliklerini kullanabilirsiniz ve kullanıcılarınızın hangi departmanı, office veya grubunun oluşturulduğu coğrafi bölge tanımlar. Örneğin, adlandırma ilkenizi olarak tanımlarsanız `PrefixSuffixNamingRequirement = “GRP [GroupName] [Department]”`, ve `User’s department = Engineering`, bir uygulanan Grup adı "GRP grubum mühendislik." olması Desteklenen Azure AD öznitelikleri \[departmanı\], \[şirket\], \[Office\], \[Eyaletveİl\], \[CountryOrRegion \], \[Başlık\]. Desteklenmeyen kullanıcı özniteliklerini sabit dizeler olarak kabul edilir; Örneğin, "\[postalCode\]". Uzantı öznitelikleri ve özel öznitelikler desteklenmez.

Kuruluşunuzdaki tüm kullanıcılar için doldurulan değerleri olan ve uzun değerlere sahip öznitelikleri kullanma öznitelikleri kullanmanızı öneririz.

### <a name="custom-blocked-words"></a>Özel engellenen sözcükler

Engellenen sözcük listesini grup adları ve diğer adları engellenmesi bir virgülle ayrılmış bir listesidir. Hiçbir alt dize aramalar gerçekleştirilir. Grup adı ve bir veya daha fazla özel engellenen sözcükler arasında tam bir eşleşme hata tetiklemek için gereklidir. Engellenen bir sözcük 'sınıf' olsa bile, kullanıcılar 'Sınıfı' gibi sık kullanılan sözcük kullanabilmesi için alt dize arama gerçekleştirilen değil.

Word listesi kurallarını engellendi:
- Engellenen sözcükler büyük küçük harfe duyarlı değildir.
- Bir kullanıcı bir grup adı bir parçası olarak engellenen bir sözcük girdiğinde, engellenen word ile bir hata iletisi görürler.
- Engellenen sözcükleri hiçbir karakter kısıtlamaları vardır.
- Engellenen sözcükler listesinde yapılandırılabilir 5000 tümcecikleri üst sınır yoktur. 

### <a name="administrator-override"></a>Yönetici geçersiz kılma

Seçili Yöneticiler bu ilkelerden tüm Grup iş yükleri ve uç noktaları, böylece engellenen sözcükleri kullanarak grupları oluşturabilirsiniz ve kendi adlandırma kuralları ile muaf. Yönetici rolleri İlkesi adlandırma grubundan muaf tutulan listesi verilmiştir.

- Genel yönetici
- İş ortağı Katman 1 destek
- İş ortağı Katman 2 Destek
- Kullanıcı Hesap Yöneticisi
- Dizin yazıcılar

## <a name="install-powershell-cmdlets-to-configure-a-naming-policy"></a>Bir adlandırma ilkesi yapılandırmak için PowerShell cmdlet'lerini yükleyin

Grafik için Windows PowerShell modülü için Azure Active Directory PowerShell herhangi bir eski sürümü kaldırın ve yüklemek mutlaka [grafiği - genel Önizleme sürümü 2.0.0.137 için Azure Active Directory PowerShell](https://www.powershellgallery.com/packages/AzureADPreview/2.0.0.137) çalıştırmadan önce PowerShell komutları. 

1. Windows PowerShell uygulamasını yönetici olarak açın.
2. AzureADPreview daha önceki sürümlerini kaldırın.
  
  ````
  Uninstall-Module AzureADPreview
  ````
3. AzureADPreview en son sürümünü yükleyin.
  
  ````
  Install-Module AzureADPreview
  ````
Güvenilmeyen bir depo erişme hakkında istenirse, yazın **Y**. Yeni modülün yüklemek birkaç dakika sürebilir.

## <a name="configure-the-group-naming-policy-for-a-tenant-using-azure-ad-powershell"></a>Azure AD PowerShell kullanarak bir kiracı için ilke adlandırma grubunu yapılandırma

1. Bilgisayarınızda Windows PowerShell penceresini açın. Yükseltilmiş ayrıcalıklar olmadan açabilirsiniz.

2. Cmdlet'leri çalıştırmak hazırlamak için aşağıdaki komutları çalıştırın.
  
  ````
  Import-Module AzureADPreview
  Connect-AzureAD
  ````
  İçinde **hesabınızda oturum** yönetici hesabınız ve parolanız hizmetinize bağlamak ve seçmek için açılır ve ekran girin **oturum**.

3. Adımları [Grup ayarlarını yapılandırmak için Azure Active Directory cmdlet'leri](active-directory-accessmanagement-groups-settings-cmdlets.md) bu Kiracı için Grup ayarları oluşturmak için.

### <a name="view-the-current-settings"></a>Geçerli ayarları görüntülemek

1. Geçerli ayarları görüntülemek için geçerli adlandırma ilkesi getirin.
  
  ````
  $Setting = Get-AzureADDirectorySetting -Id (Get-AzureADDirectorySetting | where -Property DisplayName -Value "Group.Unified" -EQ).id
  ````
  
2. Geçerli Grup ayarlarını görüntüler.
  
  ````
  $Setting.Values
  ````
  
### <a name="set-the-naming-policy-and-custom-blocked-words"></a>Özel engellenen sözcükleri ve adlandırma ilkesi ayarlama

1. Grup adı önekleri ve sonekleri Azure AD PowerShell'de ayarlayın.
  
  ````
  $Setting["PrefixSuffixNamingRequirement"] =“GRP_[GroupName]_[Department]"
  ````
  
2. Kısıtlamak istediğiniz özel engellenen sözcükler ayarlayın. Aşağıdaki örnek, kendi özel sözcükleri nasıl ekleyebileceğiniz gösterilmektedir.
  
  ````
  $Setting["CustomBlockedWordsList"]=“Payroll,CEO,HR"
  ````
  
3. Aşağıdaki örnekte etkili, gibi için yeni ilke için ayarları kaydedin.
  
  ````
  Set-AzureADDirectorySetting -Id (Get-AzureADDirectorySetting | where -Property DisplayName -Value "Group.Unified" -EQ).id -DirectorySetting $Setting
  ````
  
Bu kadar. Adlandırma ilkenizi ayarlayın ve engellenen sözcüklerinizi eklenen.

## <a name="export-or-import-the-list-of-custom-blocked-words"></a>Özel engellenen sözcüklerin listesini içeri veya dışarı

Daha fazla bilgi için bkz: [Grup ayarlarını yapılandırmak için Azure Active Directory cmdlet'leri](active-directory-accessmanagement-groups-settings-cmdlets.md).

Burada, birden çok engellenen sözcük dışarı aktarmak için bir PowerShell komut dosyası örneği verilmiştir:

````
$Words = (Get-AzureADDirectorySetting).Values | Where-Object -Property Name -Value CustomBlockedWordsList -EQ 
Add-Content "c:\work\currentblockedwordslist.txt" -Value $words.value.Split(",").Replace("`"","")  
````

Birden çok engellenen sözcük almak için PowerShell Betiği örnek aşağıda verilmiştir:

````
$BadWords = Get-Content "C:\work\currentblockedwordslist.txt"
$BadWords = [string]::join(",", $BadWords)
$Settings = Get-AzureADDirectorySetting | Where-Object {$_.DisplayName -eq "Group.Unified"}
if ($Settings.Count -eq 0)
    {$Template = Get-AzureADDirectorySettingTemplate | Where-Object {$_.DisplayName -eq "Group.Unified"}
    $Settings = $Template.CreateDirectorySetting()
    New-AzureADDirectorySetting -DirectorySetting $Settings
    $Settings = Get-AzureADDirectorySetting | Where-Object {$_.DisplayName -eq "Group.Unified"}}
$Settings["CustomBlockedWordsList"] = $BadWords
$Settings["EnableMSStandardBlockedWords"] = $True
Set-AzureADDirectorySetting -Id $Settings.Id -DirectorySetting $Settings 
````

## <a name="naming-policy-experiences-across-office-365-apps"></a>Office 365 uygulamalarında adlandırma ilkesi deneyimleri

Ne zaman bir kullanıcı bir Office 365 uygulamasında bir grup oluşturur, Azure AD'de bir grup adlandırma ilkesi ayarladıktan sonra bunlar bakın: 

* Grup adı kullanıcı türlerinde olan en kısa sürede Önizleme (ile önek ve sonek) adlandırma ilkenize göre adı A
* Kullanıcı engellenen sözcükler girerse, engellenen sözcükler kaldırabilmeniz için bunlar bir hata iletisi görürsünüz.

İş yükü | Uyumluluk
----------- | -------------------------------
Azure Active Directory portalları | Kullanıcı oluşturma veya bir grup düzenlerken Grup adını yazdığında Azure AD portalı ve erişim paneli portal adlandırma ilkesi adı gösterir. Bir kullanıcı özel engellenen word girdiğinde, böylece kullanıcı kaldırılacağına engellenen word ile bir hata iletisi görüntülenir.
Outlook Web Access (OWA) | Kullanıcı grubu adını ya da grup diğer yazdığında outlook Web Access adlandırma ilkesini gösteren adı zorunlu. Bir kullanıcı özel engellenen word girdiğinde, böylece kullanıcı kaldırmadan bir hata iletisi engellenen Word'ün yanı sıra kullanıcı arabiriminde gösterilir.
Outlook Masaüstü | Outlook Desktop'ta oluşturulan grupları adlandırma ilkesi ayarları ile uyumlu değildir. Outlook masaüstü uygulaması zorlanan grup adı önizlemesi henüz göstermez ve kullanıcı grubu adı girdiğinde özel engellenen word hata döndürmez. Ancak, adlandırma ilkesini oluştururken veya bir Grup düzenleme otomatik olarak uygulanır ve grup adı veya diğer özel engellenen sözcükleri varsa kullanıcılar hata iletilerine bakın.
Microsoft Teams | Microsoft Teams kullanıcı bir ekip adı girdiğinde ilkesi adı adlandırma grubunu gösterir. Bir kullanıcı özel engellenen word girdiğinde, böylece kullanıcı kaldırabilirsiniz engellenen Word'ün yanı sıra bir hata iletisi gösterilir.
SharePoint  |  Kullanıcı türleri bir site adı veya e-posta adresi grup, SharePoint adlandırma ilkesi adı görüntülenir. Kullanıcıyı kaldırmak için bir kullanıcı özel engellenen bir word olan bir hata iletisi girdiğinde, engellenen word ile birlikte gösterilir.
Microsoft Stream | Microsoft Stream kullanıcı grubu adı veya Grup e-posta diğer adını yazdığında ilkesi adı adlandırma grubunu gösterir. Bir kullanıcı özel engellenen word girdiğinde, kullanıcıyı kaldırmak için bir hata iletisi engellenen word ile gösterilir.
Outlook iOS ve Android uygulaması | Outlook uygulamalarında oluşturulan grupları yapılandırılmış adlandırma ilkesiyle uyumlu değildir. Outlook mobil uygulama adlandırma ilkesi adı önizlemesi henüz göstermez ve kullanıcı grubu adı girdiğinde özel engellenen word hata döndürmez. Ancak, adlandırma ilkesi Oluştur/Düzenle tıklatıldığında otomatik olarak uygulanır ve grup adı veya diğer özel engellenen sözcükleri varsa kullanıcılar hata iletilerine bakın.
Mobil uygulama grupları | Grupları mobil uygulamasında oluşturulan grupları adlandırma ilkesiyle uyumlu değildir. Mobil uygulama grupları adlandırma İlkesi'nin önizlemesini göstermez ve kullanıcı grup adı girdiğinde özel engellenen word hata döndürmez. Ancak adlandırma ilkesini oluştururken veya bir Grup düzenleme otomatik olarak uygulanır ve kullanıcıların Grup adı veya diğer özel engellenen sözcükleri varsa uygun hatalarla gösterilir.
Planner | Planlayıcısı adlandırma ilkesiyle uyumlu. Planlayıcısı adlandırma ilkesi Önizleme plan adı girerken gösterir. Bir kullanıcı özel engellenen word girdiğinde, plan oluştururken bir hata iletisi gösterilir.
Dynamics 365 müşteri katılım için | Dynamics 365 müşteri katılım için adlandırma ilkesiyle uyumlu. Kullanıcı grubu adı veya Grup e-posta diğer adını yazdığında Dynamics 365 adlandırma ilkesi adı görüntülenir. Kullanıcı bir özel engellenen sözcük girdiğinde, kullanıcıyı kaldırmak için bir hata iletisi engellenen word ile gösterilir.
Okul veri eşitleme (SDS) | İlke adlandırma ile SDS oluşturulan grupları uyumlu, ancak adlandırma ilkesi otomatik olarak uygulanmaz. Önek ve sonek için gruplar oluşturulur ve SDS için karşıya gereken sınıf adları eklemek SDS Yöneticiler sahiptir. Grup oluşturma veya düzenleme Aksi halde başarısız olur.
Outlook Customer Manager (OCM) | Outlook müşteri Outlook müşteri Yöneticisi'nde oluşturduğunuz Grup otomatik olarak uygulanan adlandırma ilkesiyle uyumlu yöneticisidir. Özel bir engellenen word algılanırsa, grup oluşturma OCM'deki engellenir ve OCM uygulamayı kullanarak kullanıcı engellendi.
Sınıf uygulama | Sınıf uygulamasında oluşturulan grupları adlandırma ilkesiyle uyumlu ancak adlandırma ilkesi otomatik olarak uygulanmaz ve bir sınıf adı yazarken, adlandırma ilkesi Önizleme kullanıcılara gösterilen değil. Kullanıcılar, önek ve sonek ile zorlanan sınıf grup adı girmeniz gerekir. Değilse, sınıf grubu oluşturun veya işlemi başarısız hatalarla düzenleyin.
Power BI | Power BI çalışma alanları adlandırma ilkesiyle uyumlu.    
Yammer | Bağlı gruplar yapılandırılmış adlandırma ilkesi zorlamaz yammer. Etkin ilke adlandırma ile kuruluşlar için Yammer adlandırma ilkesine uygun olmaz grupları için Office 365'e bağlı olmayan eski Yammer grupları oluşturur.
StaffHub  | StaffHub takımlar adlandırma ilkesi izlemeyin ancak temel alınan Office 365 grubunu desteklemez. StaffHub takım adı sonekleri ve önekleri uygulanmaz ve özel engellenen sözcükleri denetlemez. Ancak StaffHub önek ve sonek uygulamak ve engellenen sözcükleri temel Office 365 gruptan kaldırır.
Exchange PowerShell | Exchange PowerShell cmdlet'leri adlandırma ilkesiyle uyumlu. Kullanıcılar Grup adı ve grup diğer adı (mailNickname) adlandırma ilkesi izlemeyin ilgili hata iletilerini önerilen önek ve sonek ve özel engellenen sözcükleri alır.
Azure Active Directory PowerShell cmdlet'leri | Azure Active Directory PowerShell cmdlet'lerini, ilke adlandırma ile uyumludur. Kullanıcılar Grup adları ve grup diğer adlandırma kuralı izlemeyin ilgili hata iletilerini önerilen önek ve sonek ve özel engellenen sözcükleri alır.
Exchange yönetici merkezini | Exchange yönetici merkezini İlkesi adlandırma ile uyumludur. Kullanıcılar Grup adı ve grup diğer adlandırma kuralı izlemeyin ilgili hata iletilerini önerilen önek ve sonek ve özel engellenen sözcükleri alır.
Office 365 Yönetim Merkezi | Office 365 Yönetim Merkezi ilke adlandırma ile uyumludur. Ne zaman bir kullanıcı oluşturur veya düzenlemeleri grup adlarını adlandırma ilkesi otomatik olarak uygulanır ve özel engellenen sözcük girdiğinizde kullanıcılar uygun hataları alırsınız. Office 365 Yönetim Merkezi adlandırma ilkesi önizlemesi henüz göstermez ve kullanıcı grubu adı girdiğinde özel engellenen word hata döndürmez.

## <a name="next-steps"></a>Sonraki adımlar
Bu makaleler Azure AD grupları hakkında ek bilgiler sağlar.

* [Var olan grupları bakın](active-directory-groups-view-azure-portal.md)
* [Office 365 grupları için süre sonu ilkesi](active-directory-groups-lifecycle-azure-portal.md)
* [Bir grubu ayarlarını yönetme](active-directory-groups-settings-azure-portal.md)
* [Bir grubun üyelerini yönetmek](active-directory-groups-members-azure-portal.md)
* [Bir grubun üyeliğini yönetme](active-directory-groups-membership-azure-portal.md)
* [Bir gruptaki kullanıcılar için dinamik kurallarını yönet](active-directory-groups-dynamic-membership-azure-portal.md)
