---
title: Office 365 grupları - Azure Active Directory Grup adlandırma ilkesi zorlama | Microsoft Docs
description: Office 365 grupları Azure Active Directory'de adlandırma ilkesi ayarlama yapma
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
editor: ''
ms.service: active-directory
ms.workload: identity
ms.subservice: users-groups-roles
ms.topic: article
ms.date: 05/06/2019
ms.author: curtand
ms.reviewer: krbain
ms.custom: it-pro;seo-update-azuread-jan
ms.collection: M365-identity-device-management
ms.openlocfilehash: 0c13b95028975c5463217455c940bb84c3867899
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66734781"
---
# <a name="enforce-a-naming-policy-on-office-365-groups-in-azure-active-directory"></a>Office 365 grupları Azure Active Directory'de bir adlandırma ilkesini zorlama

Oluşturduğunuz veya düzenlediğiniz kullanıcılarınız tarafından Office 365 grupları için tutarlı adlandırma kuralları zorlamak için ilke Azure Active Directory (Azure AD) kiracılarınız için bir adlandırma ayarlayın. Örneğin, bir grup üyeliği, coğrafi bölgeyi işlevi iletişim kurmak için adlandırma ilkesi kullanabilir veya grup oluşturan. Kullanıcının adres defterinde grupları kategorilere ayırmaya yardımcı olacak adlandırma İlkesi'ni de kullanabilirsiniz. Belirli sözcükleri grubu adları ve diğer adları kullanılmasını engellemek için ilke kullanabilirsiniz.

> [!IMPORTANT]
> Sahip ancak mutlaka bir Azure Active Directory Premium P1 lisansı veya bir veya daha fazla Office 365 gruplarının üyesi olan her bir benzersiz kullanıcı için Azure AD temel EDU lisansı atamak için Office 365 grupları Azure AD adlandırma ilkesi kullanarak gerektirir.

Adlandırma ilkesi oluşturma ve düzenleme (örneğin, Outlook, Microsoft Teams, SharePoint, Exchange veya Planner) iş yüklerinde oluşturulan grupların uygulanır. Grup adı ve grup diğer adı için uygulanır. Adlandırma ilkesini Azure AD, Azure AD'de, adlandırma ilkesi ayarlayın ve varolan bir Exchange Grup adlandırma ilkesi varsa, kuruluşunuzda zorunlu tutulur.

## <a name="naming-policy-features"></a>Adlandırma ilkesi özellikleri

Gruplar için adlandırma ilkesi iki farklı şekillerde uygulayabilir:

- **Önek sonek adlandırma ilkesi** ön eklerin veya soneklerin sonra otomatik olarak gruplarınızı üzerinde bir adlandırma kuralı uygulamak için eklenen tanımlayabilirsiniz (örneğin, grup adı olarak "GRP\_JAPONYA\_grubunuza\_ Mühendisliğe"GRP\_JAPONYA\_ öneki ve \_mühendislik soneki olan). 

- **Özel engellenen sözcük** kullanıcılar (örneğin "CEO, bordro, ik") tarafından oluşturulan grupları engellenmesi, kuruluşunuz için engellenen sözcük belirli bir dizi karşıya yükleyebilirsiniz.

### <a name="prefix-suffix-naming-policy"></a>Önek sonek adlandırma ilkesi

Adlandırma kuralı genel yapısını 'Öneki [GroupName] soneki' dir. Birden çok önek ve sonek tanımlayabilirsiniz, ancak yalnızca ayar [GroupName] bir örneği olabilir. Ön eklerin veya soneklerin sabit dizeler ya da kullanıcı öznitelikleri gibi olabilir \[departmanı\] , yerine grubu oluşturan kullanıcıya göre. Toplam izin verilen en fazla karakter birleştirilmiş, önek ve sonek dizeleri 53 karakterdir. 

Önek ve sonek grubu adı ve grup diğer adının desteklenen özel karakterler içerebilir. Grup diğer desteklenmeyen herhangi bir karakter önek veya sonek olarak hala uygulanan Grup adı ', ancak Grup diğer addan kaldırıldı. Bu kısıtlama nedeniyle, önek ve sonek grubun adını uygulanan Grup diğer adının için uygulanan farklı olabilir. 

#### <a name="fixed-strings"></a>Sabit Dizeler

Dizeleri tarama ve genel adres listesinde ve sol gezinti bağlantıları grubu iş yükü gruplarını birbirinden daha kolay hale getirmek için kullanabilirsiniz. Bazı ortak ön ekleri gibi anahtar sözcükler ' Grp\_adı ', '\#adı ','\_adı '

#### <a name="user-attributes"></a>Kullanıcı öznitelikleri

Yardımcı olabilecek öznitelikleri kullanabilirsiniz ve kullanıcılarınızın departmanı, office veya grubunun oluşturulduğu coğrafi bölgeyi tanımlar. Örneğin, adlandırma ilkenizi olarak tanımlarsanız `PrefixSuffixNamingRequirement = "GRP [GroupName] [Department]"`, ve `User’s department = Engineering`, bir uygulanan Grup adı "GRP grubum Engineering" gelebilir Desteklenen Azure AD öznitelikleri \[departmanı\], \[şirket\], \[Office\], \[Eyaletveİl\], \[CountryOrRegion \], \[Başlık\]. Desteklenmeyen kullanıcı öznitelikleri, sabit dize olarak kabul edilir; Örneğin, "\[postalCode\]". Uzantı öznitelikleri ve özel öznitelikler desteklenmez.

Kuruluşunuzdaki tüm kullanıcılar için doldurulmuş değerler ve uzun değerli öznitelikleri kullanmayın öznitelikleri kullanmanızı öneririz.

### <a name="custom-blocked-words"></a>Özel engellenen sözcük

Tümce grubu adları ve diğer adları engellediği için virgülle ayrılmış bir liste engellenen sözcük listesidir. Hiçbir alt dize arama gerçekleştirilir. Grup adı ve bir veya daha fazla özel engellenen bir kelimelerin arasında bir tam eşleşme başarısız tetiklemek için gereklidir. Engellenen bir sözcük 'sınıf' olsa bile kullanıcılar 'Class' gibi ortak kelimeler kullanabilirsiniz, böylece alt dize araması yapılamaz.

Engellenen sözcük listesi kuralları:

- Engellenen bir sözcük büyük/küçük harfe duyarlı değildir.
- Bir kullanıcı grubu adı bir parçası olarak engellenen bir sözcük girdiğinde, engellenen sözcük içeren bir hata iletisi görürler.
- Engellenen bir sözcük hiçbir karakter kısıtlamaları vardır.
- Engellenen bir sözcük listesinde yapılandırılabilir 5000 tümcecikleri üst sınır yoktur. 

### <a name="administrator-override"></a>Yönetici geçersiz kılma

Seçili yöneticileri bu ilkeleri, tüm Grup iş yükleri ve uç noktaları, böylece bunlar engellenen sözcük kullanarak gruplar oluşturabilir ve kendi adlandırma kuralları ile muaf tutulabilir. Muaf tutulan gruptan adlandırma ilkesinin yönetici rolleri listesi verilmiştir.

- Genel yönetici
- İş ortağı Katman 1 destek
- İş ortağı Katman 2 Destek
- Kullanıcı Yöneticisi
- Dizin yazıcılar

## <a name="configure-naming-policy-in-azure-portal"></a>Azure portalında adlandırma ilkesi yapılandırma

1. Oturum [Azure AD yönetim merkezini](https://aad.portal.azure.com) kullanıcı yönetici hesabıyla.
1. Seçin **grupları**, ardından **adlandırma ilkesinin** adlandırma ilkesi sayfasını açın.

    ![Yönetim merkezinde adlandırma ilkesi sayfasını açın](./media/groups-naming-policy/policy.png)

### <a name="view-or-edit-the-prefix-suffix-naming-policy"></a>Görüntülemek veya önek sonek adlandırma ilkesini Düzenle

1. Üzerinde **adlandırma ilkesinin** sayfasında **Grup adlandırma ilkesi**.
1. Görüntüleyebilir veya geçerli bir önek veya sonek ilkeleri tek tek öznitelikler veya adlandırma ilkesinin bir parçası uygulamak istediğiniz dizeleri seçerek adlandırma düzenleyin.
1. Bir önek veya sonek listeden kaldırmak için önek veya sonek seçin ve sonra seçin **Sil**. Aynı anda birden çok öğe silinebilir.
1. Yürürlüğe seçerek gitmek yeni ilke için yaptığınız değişiklikleri kaydetmek **Kaydet**.

### <a name="edit-custom-blocked-words"></a>Özel engellenen sözcük Düzenle

1. Üzerinde **adlandırma ilkesinin** sayfasında **engellenen sözcük**.

    ![Düzenle ve adlandırma ilkesi için engellenen sözcük listesi karşıya yükleyin](./media/groups-naming-policy/blockedwords.png)

1. Görüntülemek veya seçerek özel engellenen sözcük geçerli listesini düzenleyin **indirme**.
1. Yeni özel engellenen sözcüklerin listesi dosyası simgesini seçerek karşıya yükleyin.
1. Yürürlüğe seçerek gitmek yeni ilke için yaptığınız değişiklikleri kaydetmek **Kaydet**.

## <a name="install-powershell-cmdlets"></a>PowerShell cmdlet'lerini yükleme

PowerShell komutlarını çalıştırmadan önce Windows PowerShell Graph için Azure Active Directory PowerShell Modülünün eski sürümlerini kaldırın ve [Graph için Azure Active Directory PowerShell - Genel Önizleme Sürümünü 2.0.0.137](https://www.powershellgallery.com/packages/AzureADPreview/2.0.0.137) yükleyin.

1. Windows PowerShell uygulamasını yönetici olarak açın.
2. Eski AzureADPreview sürümlerini kaldırın.
  
   ``` PowerShell
   Uninstall-Module AzureADPreview
   ```

3. En son AzureADPreview sürümünü yükleyin.
  
   ``` PowerShell
   Install-Module AzureADPreview
   ```

   Güvenilmeyen bir depoya erişme hakkında istenirse, girin **Y**. Yeni modülün yüklenmesi birkaç dakika sürebilir.

## <a name="configure-naming-policy-in-powershell"></a>PowerShell'de adlandırma ilkesi yapılandırma

1. Bilgisayarınızda bir Windows PowerShell penceresi açın. Yükseltilmiş ayrıcalıklar olmadan açabilirsiniz.

1. Ortamı cmdlet'leri çalıştırmaya hazır hale getirmek için aşağıdaki komutları çalıştırın.
  
   ``` PowerShell
   Import-Module AzureADPreview
   Connect-AzureAD
   ```

   Açılan **Hesabınızda oturum açın** ekranında hizmetinizle bağlantı kurmak için yönetici hesabınızın adını ve parolasını girin **Oturum aç**'ı seçin.

1. Bu kiracının grup ayarlarını oluşturmak için [Grup ayarlarını yapılandırmak için Azure Active Directory cmdlet'leri](groups-settings-cmdlets.md) adımlarını izleyin.

### <a name="view-the-current-settings"></a>Geçerli ayarları görüntüleyebilir

1. Geçerli ayarları görüntülemek için geçerli adlandırma ilkesi getirin.
  
   ``` PowerShell
   $Setting = Get-AzureADDirectorySetting -Id (Get-AzureADDirectorySetting | where -Property DisplayName -Value "Group.Unified" -EQ).id
   ```
  
1. Geçerli grup ayarlarını görüntüleyin.
  
   ``` PowerShell
   $Setting.Values
   ```
  
### <a name="set-the-naming-policy-and-custom-blocked-words"></a>Özel engellenen sözcükler ve adlandırma ilkesi ayarlama

1. Azure AD PowerShell'de grup adı ön ve son eklerini ayarlayın. Düzgün bir şekilde çalışması için özelliği için [GroupName] ayarı eklenmesi gerekir.
  
   ``` PowerShell
   $Setting["PrefixSuffixNamingRequirement"] =“GRP_[GroupName]_[Department]"
   ```
  
1. Sınırlamak istediğiniz özel engellenen sözcükleri belirleyin. Aşağıdaki örnekte kendi özel sözcüklerinizi ekleme adımları gösterilmektedir.
  
   ``` PowerShell
   $Setting["CustomBlockedWordsList"]=“Payroll,CEO,HR"
   ```
  
1. Aşağıdaki örnekte olduğu gibi yürürlüğe gitmek için yeni ilke için ayarları kaydedin.
  
   ``` PowerShell
   Set-AzureADDirectorySetting -Id (Get-AzureADDirectorySetting | where -Property DisplayName -Value "Group.Unified" -EQ).id -DirectorySetting $Setting
   ```
  
İşte bu kadar. Adlandırma ilkenizi ve engellenen kelimeleriniz eklendi.

## <a name="export-or-import-custom-blocked-words"></a>Özel engellenen sözcük içeri veya dışarı aktarma

Daha fazla bilgi için bkz [Grup ayarlarını yapılandırmak için Azure Active Directory cmdlet'leri](groups-settings-cmdlets.md).

Birden çok engellenen sözcük dışarı aktarmak için bir PowerShell Betiği örneği aşağıda verilmiştir:

``` PowerShell
$Words = (Get-AzureADDirectorySetting).Values | Where-Object -Property Name -Value CustomBlockedWordsList -EQ 
Add-Content "c:\work\currentblockedwordslist.txt" -Value $words.value.Split(",").Replace("`"","")  
```

Birden çok engellenen sözcük içeri aktarmak için PowerShell Betiği bir örnek aşağıda verilmiştir:

``` PowerShell
$BadWords = Get-Content "C:\work\currentblockedwordslist.txt"
$BadWords = [string]::join(",", $BadWords)
$Settings = Get-AzureADDirectorySetting | Where-Object {$_.DisplayName -eq "Group.Unified"}
if ($Settings.Count -eq 0)
    {$Template = Get-AzureADDirectorySettingTemplate | Where-Object {$_.DisplayName -eq "Group.Unified"}
    $Settings = $Template.CreateDirectorySetting()
    New-AzureADDirectorySetting -DirectorySetting $Settings
    $Settings = Get-AzureADDirectorySetting | Where-Object {$_.DisplayName -eq "Group.Unified"}}
$Settings["CustomBlockedWordsList"] = $BadWords
Set-AzureADDirectorySetting -Id $Settings.Id -DirectorySetting $Settings 
```

## <a name="remove-the-naming-policy"></a>Adlandırma ilkesini Kaldır

### <a name="remove-the-naming-policy-using-azure-portal"></a>Azure portalını kullanarak bir adlandırma ilkesini Kaldır

1. Üzerinde **adlandırma ilkesinin** sayfasında **silme ilkesi**.
1. Silme işlemini onayladıktan sonra adlandırma ilkesi, tüm ön eki soneki dahil olmak üzere kaldırılır adlandırma ilkesi ve özel engellenen sözcük.

### <a name="remove-the-naming-policy-using-azure-ad-powershell"></a>Azure AD PowerShell kullanarak bir adlandırma ilkesini Kaldır

1. Grup adı önek ve sonek Azure AD PowerShell'de boş.
  
   ``` PowerShell
   $Setting["PrefixSuffixNamingRequirement"] =""
   ```
  
1. Özel engellenen sözcük boş.
  
   ``` PowerShell
   $Setting["CustomBlockedWordsList"]=""
   ```
  
1. Ayarları kaydedin.
  
   ``` PowerShell
   Set-AzureADDirectorySetting -Id (Get-AzureADDirectorySetting | where -Property DisplayName -Value "Group.Unified" -EQ).id -DirectorySetting $Setting
   ```

## <a name="experience-across-office-365-apps"></a>Deneyiminiz boyunca Office 365 uygulamaları

Bir kullanıcı grubu bir Office 365 uygulama oluşturduğunda, Azure AD'de bir grup adlandırma ilkesi ayarladıktan sonra görürler:

- Grup adı, kullanıcı türleri olan en kısa sürede Önizleme adlandırma ilkenizle (önek ve sonek) göre adı A
- Kullanıcı, engellenen bir sözcük girerse, bunlar engellenen sözcük kaldırabilmeniz için bir hata iletisi görürler.

İş yükü | Uyumluluk
----------- | -------------------------------
Azure Active Directory portalları | Kullanıcı, grup adını oluştururken ya da bir grubu düzenleme yazdığında Azure AD erişim paneli portalı ve adlandırma zorlanan ilke adını gösterir. Bir kullanıcı özel engellenen bir sözcük girdiğinde, böylece kullanıcı kaldırabilirsiniz engellenen sözcük içeren bir hata iletisi görüntülenir.
Outlook Web Access (OWA) | Kullanıcı grubu adı ya da grup diğer adının yazdığında, outlook Web Access adlandırma ilkesi gösterir adı zorunlu. Bir kullanıcı özel engellenen bir sözcük girdiğinde, kullanıcı kaldırabilirsiniz, böylece bir hata iletisi engellenen sözcük yanı sıra kullanıcı Arabirimi gösterilir.
Outlook Masaüstü | Outlook Masaüstü oluşturulan grupların adlandırma ilkesi ayarları ile uyumlu olması gerekir. Outlook masaüstü uygulaması henüz Önizleme uygulanan Grup adının göstermez ve kullanıcı grubu adı girdiğinde özel engellenen sözcük hata döndürmez. Ancak, adlandırma ilkesi oluştururken veya düzenlerken bir grubu otomatik olarak uygulanır ve grup adını ya da diğer özel engellenen sözcük yoksa, kullanıcılar hata iletilerini görmek.
Microsoft Teams | Microsoft Teams zorlanan ilke adı, kullanıcı bir takım adı girdiğinde adlandırma grubu gösterir. Bir kullanıcı özel engellenen bir sözcük girdiğinde, böylece kullanıcı kaldırabilirsiniz engellenen sözcük yanı sıra bir hata iletisi gösterilir.
SharePoint  |  SharePoint site bir kullanıcı türleri ad veya e-posta adresi Grup adlandırma zorlanan ilke adını gösterir. Kullanıcıyı kaldırmak için bir kullanıcı özel engellenen bir sözcük olan bir hata iletisi girdiğinde, engellenen bir sözcük ile birlikte gösterilir.
Microsoft Stream | Microsoft Stream, kullanıcı grubu adı ya da Grup e-posta diğer adı yazdığında zorlanan ilke adı adlandırma gösterir. Bir kullanıcı özel engellenen bir sözcük girdiğinde, kullanıcıyı kaldırmak için engellenen bir sözcük ile bir hata iletisi gösterilir.
Outlook iOS ve Android uygulaması | Outlook uygulamalarında oluşturulan grupları yapılandırılmış adlandırma ilkesi ile uyumlu olması gerekir. Outlook mobil uygulamasının henüz adlandırma zorlanan ilke adı önizlemesiyle göstermez ve kullanıcı grubu adı girdiğinde özel engellenen sözcük hata döndürmez. Ancak, adlandırma ilkesi Oluştur/Düzenle'ye tıkladığınızda otomatik olarak uygulanır ve grup adını ya da diğer özel engellenen sözcük yoksa, kullanıcılar hata iletilerini görmek.
Mobil uygulama grupları | Grupları mobil uygulamasında oluşturulan grupların adlandırma ilkesi ile uyumlu olması gerekir. Mobil uygulama grupları Önizleme adlandırma ilkesinin göstermez ve kullanıcı grubu adı girdiğinde özel engellenen sözcük hata döndürmez. Ancak adlandırma ilkesi oluştururken veya düzenlerken bir grubu otomatik olarak uygulanır ve kullanıcı grubu adı veya diğer özel engellenen sözcük varsa uygun hatalarla sunulur.
Planner | Planner adlandırma ilkesi ile uyumludur. Plan adı girerken, Planner adlandırma ilkesi Önizleme gösterir. Bir kullanıcı özel engellenen bir sözcük girdiğinde, planı oluşturulurken bir hata iletisi gösterilir.
Müşteri Etkileşimi için Dynamics 365 | Dynamics 365 müşteri katılımı için adlandırma ilkesi ile uyumludur. Kullanıcı grubu adı ya da Grup e-posta diğer adı yazdığında Dynamics 365 adlandırma zorlanan ilke adını gösterir. Kullanıcıyı kaldırmak için bir hata iletisi, kullanıcı özel engellenen bir sözcük girdiğinde, engellenen bir sözcük ile gösterilir.
School Data Sync'i (SDS) | SDS ile oluşturulan grupların adlandırma ilkesi ile uyumlu, ancak adlandırma ilkesi otomatik olarak uygulanmaz. SDS yöneticilerinin, önek ve sonek gruplarının oluşturulması ve ardından SDS için karşıya gerekir sınıf adları eklemek vardır. Grup oluşturma veya düzenleme, aksi takdirde başarısız olur.
Outlook Customer Manager'a (OCM) | Outlook Customer Manager, Outlook Customer Manager'a oluşturduğunuz gruba otomatik olarak uygulanan adlandırma ilkesi ile uyumludur. Özel engellenen bir sözcük algılanırsa, grup oluşturma OCM'deki engellenir ve kullanıcı OCM uygulama kullanımından engellenir.
Classroom uygulaması | Classroom uygulamasında oluşturulan grupların adlandırma ilkesiyle uyumlu ancak adlandırma ilkesi otomatik olarak uygulanmaz ve bir sınıf grup adı girerken, adlandırma ilkesi Önizleme kullanıcılara gösterilen değil. Kullanıcılar, önek ve sonek zorlanan classroom grup adıyla girmeniz gerekir. Aksi halde classroom grubu oluşturma veya düzenleme hatalarla işlemi başarısız.
Power BI | Power BI çalışma alanları adlandırma ilkesi ile uyumlu olması gerekir.    
Yammer | Azure Active Directory hesaplarıyla Yammer'a açan bir kullanıcı bir grup oluşturur ya da bir grup adı düzenler adlandırma ilkesinin grup adı uyacaktır. Bu, hem Office 365 bağlı grupları ve diğer tüm Yammer grupları için geçerlidir.<br>Grup adı otomatik olarak bir Office 365 bağlı Grup adlandırma ilkesi yerinde önce oluşturulduysa adlandırma ilkeleri izlenmez. Bir kullanıcı grubu adı düzenlediğinde öneki ve soneki eklemeniz istenir.
StaffHub  | StaffHub takımlar adlandırma ilkesi izlemeyin, ancak temel alınan Office 365 grubu yok. StaffHub takım adı bir önek ve sonek geçerli değildir ve özel engellenen sözcükler için denetlemez. Ancak StaffHub önek ve sonek geçerli ve engellenen bir sözcük temel alınan bir Office 365 grubundan kaldırır.
Exchange PowerShell | Exchange PowerShell cmdlet'leri adlandırma ilkesi ile uyumlu olması gerekir. Grup adı ve grup diğer adının (mailNickname) adlandırma ilkesi izlerseniz yoksa, kullanıcılar önerilen önek ve sonek ve özel engellenen sözcükler için uygun hata iletileri alır.
Azure Active Directory PowerShell cmdlet'leri | Azure Active Directory PowerShell cmdlet'lerini adlandırma ilkesi ile uyumlu olması gerekir. Kullanıcılar grubu adları ve grup diğer adının adlandırma kuralını takip etmiyorsa önerilen önek ve sonek ve özel engellenen sözcükler için uygun hata iletilerini alır.
Exchange yönetici merkezini | Exchange yönetici merkezini adlandırma ilkesi ile uyumludur. Grup adı ve grup diğer adının adlandırma kuralı izlerseniz yoksa, kullanıcılar önerilen önek ve sonek ve özel engellenen sözcükler için uygun hata iletileri alır.
Microsoft 365 Yönetim Merkezi | Microsoft 365 Yönetim merkezini adlandırma ilkesi ile uyumludur. Ne zaman bir kullanıcı oluşturur veya düzenlemeleri grup adlarını adlandırma ilkesi otomatik olarak uygulanır ve bunlar özel engellenen sözcük girdiğinizde kullanıcılar uygun hataları alırsınız. Microsoft 365 Yönetim Merkezi önizlemesi adlandırma ilkesi henüz göstermez ve kullanıcı grubu adı girdiğinde özel engellenen sözcük hataları dönmez.

## <a name="next-steps"></a>Sonraki adımlar

Bu makaleler, Azure AD grupları hakkında ek bilgi sağlar.

- [Var olan grupları görme](../fundamentals/active-directory-groups-view-azure-portal.md)
- [Office 365 gruplarının süre sonu ilkesi](groups-lifecycle.md)
- [Bir grubun ayarlarını yönetme](../fundamentals/active-directory-groups-settings-azure-portal.md)
- [Bir grubun üyelerini yönetme](../fundamentals/active-directory-groups-members-azure-portal.md)
- [Bir grubun üyeliklerini yönetme](../fundamentals/active-directory-groups-membership-azure-portal.md)
- [Bir gruptaki kullanıcılar için dinamik kuralları yönetme](groups-dynamic-membership.md)
