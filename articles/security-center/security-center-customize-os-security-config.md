---
title: "İşletim sistemi güvenlik yapılandırmalarını Azure Güvenlik Merkezi'nde [Önizleme] özelleştirme | Microsoft Docs"
description: "Bu makalede Güvenlik Merkezi değerlendirmeleri özelleştirmek öğretir"
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/16/2018
ms.author: terrylan
ms.openlocfilehash: 3af59e1b38e70494dd9dc17e2682d31cf7b7d361
ms.sourcegitcommit: 5108f637c457a276fffcf2b8b332a67774b05981
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/17/2018
---
# <a name="customizing-os-security-configurations-in-azure-security-center-preview"></a>İşletim sistemi güvenlik yapılandırmalarını Azure Güvenlik Merkezi'nde [Önizleme] özelleştirme

Azure Güvenlik Merkezi bu kılavuzu kullanarak işletim sistemi güvenlik yapılandırması değerlendirmesinde özelleştirmeyi öğrenin.

## <a name="what-are-os-security-configurations"></a>İşletim sistemi güvenlik yapılandırmalarını nelerdir?

Güvenlik duvarları, denetleme, parola ilkeleri ve daha fazla bilgi için işletim sistemi sağlamlaştırmaya yönelik 150'den fazla önerilen kuralları kümesi kullanarak, azure Güvenlik Merkezi izleyiciler güvenlik yapılandırmalarını kuralları dahil olmak üzere ilgili. Bir güvenlik açığı yapılandırmaya sahip bir makine bulunursa, güvenlik açısından oluşturulur.

Kuralların özelleştirme, kuruluşların kendi ortamı için daha uygun olan yapılandırma seçenekleri denetlemek için yardımcı olabilir. Bu özellik, kullanıcıların bir özelleştirilmiş değerlendirme İlkesi ayarlamak ve Abonelikteki tüm geçerli makinelerde uygulama sağlar.

> [!NOTE]
> - Şu anda işletim sistemi güvenlik yapılandırması özelleştirme 2012, 2012 R2 işletim sistemleri yalnızca Windows Server 2008, 2008R2, kullanılabilir.
- Yapılandırma tüm sanal makineleri için geçerlidir ve bağlı bilgisayarlar için tüm çalışma alanları seçili abonelik altında.
- İşletim sistemi güvenlik yapılandırması özelleştirme Güvenlik Merkezi'nin standart katmanında yalnızca kullanılabilir.
>
>

İşletim sistemi güvenlik yapılandırması kurallarını özelleştirmek nasıl?

İşletim sistemi güvenlik yapılandırma kuralları, etkinleştirme ve belirli bir kuralın devre dışı bırakma, mevcut bir kuralı için istenen ayarlarını değiştirme ve desteklenen kural türlerine göre (kayıt defteri, Denetim ilkesini ve güvenlik ilkesi) yeni bir kural ekleyerek özelleştirebilirsiniz. Şu anda, istenen ayar tam bir değer olmalıdır.

Yeni kurallar aynı biçim ve yapısıyla aynı türde diğer kuralların olması gerekir.

> [!NOTE]
> İşletim sistemi güvenlik yapılandırmalarını özelleştirmek için abonelik sahibi, abonelik katkıda bulunan veya güvenlik yöneticisi rolüne atanması gerekir.
>
>

## <a name="customize-security-configuration"></a>Güvenlik Yapılandırması özelleştirme

Varsayılan işletim sistemi güvenlik yapılandırması Güvenlik Merkezi'nde özelleştirmek için:

1.  **Güvenlik Merkezi** panosunu açın.

2.  Güvenlik Merkezi ana menüsündeki seçin **Güvenlik İlkesi**.  **Güvenlik Merkezi - Güvenlik İlkesi** açar.

3.  Özelleştirme için gerçekleştirmek istediğiniz aboneliği seçin.

    ![](media/security-center-customize-os-security-config/open-security-policy.png)

4. Altında **İlkesi BİLEŞENLERİ**seçin **Düzenle güvenlik yapılandırmalarını**.

6.  **Güvenlik yapılandırmalarını Düzenle** açar. İndirmek, düzenlemek ve değiştirilen dosya yüklemek için ekran görüntüsünde vurgulanan adımları izleyin.

    ![](media/security-center-customize-os-security-config/blade.png)

  > [!NOTE]
  > Varsayılan olarak, indirdiğiniz yapılandırma dosyasının bulunduğu *json* biçimi. Bu dosyayı değiştirmek hakkında daha fazla yönerge için Git [yapılandırma dosyasını özelleştirme](#customize-the-configuration-file).
  >

7. Başarıyla dosyayı kaydettikten sonra yapılandırma için tüm sanal makineleri uygulanır ve bağlı bilgisayarlar için tüm çalışma alanları seçili abonelik altında. Bu işlem biraz zaman, genellikle birkaç dakika sürebilir ancak altyapı boyutuna bağlı olduğundan, uzun sürebilir. Seçin **kaydetmek** değişikliği kaydetmek için Aksi takdirde İlkesi saklanmaz.

    ![](media/security-center-customize-os-security-config/save-successfully.png)

Herhangi bir noktada, geçerli ilke yapılandırması için varsayılan ilke durumu seçerek sıfırlayabilirsiniz **sıfırlama** seçeneğini **işletim sistemi güvenlik yapılandırmasını düzenle kuralları**. Bu seçeneği seçerseniz, aşağıdaki açılır uyarı alırsınız. Seçin **Evet** onaylamak için.

![](media/security-center-customize-os-security-config/edit-alert.png)

## <a name="customize-the-configuration-file"></a>Yapılandırma dosyasını özelleştirme

Özelleştirme dosyasında desteklenen her işletim sistemi sürümü kuralları (ruleset) kümesi içeriyor. Aşağıdaki örnekte gösterildiği gibi kendi adı ve benzersiz bir kimlik her kural kümesi vardır:

![](media/security-center-customize-os-security-config/custom-file.png)

> [!NOTE]
> Bu dosya, Visual Studio kullanarak düzenlendi ancak JSON Görüntüleyicisi eklentisinin yüklü olduğu sürece, Not Defteri de kullanabilirsiniz.
>
>

Bu dosyayı düzenlerken, bir kural veya tümünü değiştirebilirsiniz. Her ruleset içeren bir *kuralları* kuralları 3 kategorilere ayrılmış kuralları içeren bölümü: kayıt defteri, Denetim ilkesini ve güvenlik ilkesi, aşağıdaki göster:

![](media/security-center-customize-os-security-config/rules-section.png)

Her kategori kendi öznitelikleri kümesi vardır. Varolan kuralları için aşağıdaki Listedekilerin değişiklikleri yapabilirsiniz:

- expectedValue: Bu özniteliğin alanın veri türünü her bir kural türünün başına desteklenen değerler örneğin eşleşmesi gerekir:

  - baselineRegistryRules: değer eşleşmelidir [regValueType](https://msdn.microsoft.com/library/windows/desktop/ms724884) bu kuralda tanımlı.

  - baselineAuditPolicyRules: değer bir dize değeri, şunlardan biri olmalıdır:

    - Başarı ve Hata

    - Başarılı

  - baselineSecurityPolicyRules: değer bir dize değeri, şunlardan biri olmalıdır:

    - "Tek"

    - Listesi izin kullanıcı grupları, örneğin: "Yöneticileri, Backup Operators"

-   Durum: "Devre dışı" veya "Enabled" seçenekleri içerebilir dize. Bu özel Önizleme sürümü için dize büyük/küçük harf duyarlıdır.

Bunlar, yapılandırılabilir yalnızca alanlardır. Dosya biçimi veya boyutu ihlal değişikliği kaydetmek mümkün olmayacaktır. Dosya işlendiğinde aşağıdaki hata iletisini oluşur:

![](media/security-center-customize-os-security-config/invalid-json.png)

Bkz: [hata kodları](#error-codes) olası hataları listesi.

Aşağıda, bu kurallar bazı örnekleri vardır. Öznitelikleri 'expectedValue' ve 'state' değiştirilebilir:

**Kuralları bölümünde:** baselineRegistryRules
```
{

    "hive": "LocalMachine",
    "regValueType": "Int",
    "keyPath":
    "System\\\\CurrentControlSet\\\\Services\\\\LanManServer\\\\Parameters",
    "valueName": "restrictnullsessaccess",
    "ruleId": "f9020046-6340-451d-9548-3c45d765d06d",
    "originalId": "0f319931-aa36-4313-9320-86311c0fa623",
    "cceId": "CCE-10940-5",
    "ruleName": "Network access: Restrict anonymous access to Named Pipes and
    Shares",
    "ruleType": "Registry",
    "expectedValue": "1",
    "severity": "Warning",
    "analyzeOperation": "Equals",
    "source": "Microsoft",
    "state": "Disabled"

}
```

**Kuralları bölümünde:** baselineAuditPolicyRules
```
{
"auditPolicyId": "0cce923a-69ae-11d9-bed3-505054503030",
"ruleId": "37745508-95fb-44ec-ab0f-644ec0b16995",
"originalId": "2ea0de1a-c71d-46c8-8350-a7dd4d447895",
"cceId": "CCE-11001-5",
"ruleName": "Audit Policy: Account Management: Other Account Management Events",
"ruleType": "AuditPolicy",
"expectedValue": "Success and Failure",
"severity": "Critical",
"analyzeOperation": "Equals",
"source": "Microsoft",
"state": "Enabled"
},
```

**Kuralları bölümler:** baselineSecurityPolicyRules
```
{
"sectionName": "Privilege Rights",
"settingName": "SeIncreaseWorkingSetPrivilege",
"ruleId": "b0ec9d5e-916f-4356-83aa-c23522102b33",
"originalId": "b61bd492-74b0-40f3-909d-36b9bf54e94c",
"cceId": "CCE-10548-6",
"ruleName": "Increase a process working set",
"ruleType": "SecurityPolicy",
"expectedValue": "Administrators, Local Service",
"severity": "Warning",
"analyzeOperation": "Equals",
"source": "Microsoft",
"state": "Enabled"
},
```

Farklı işletim sistemi türleri için yinelenen bazı kurallar vardır. Yinelenen kuralları aynı 'originalId' var.

## <a name="adding-a-new-custom-rule"></a>Yeni bir özel kural ekleme

Yeni bir kural oluşturabilirsiniz. Yeni bir kural oluşturmadan önce aşağıdaki kısıtlamaları göz önünde bulundurun:

-   Şema sürümü *baselineId* ve *baselineName* değiştirilemez.

-   RuleSet kaldırılamaz.

-   RuleSet eklenemez.

-   Kurallar (varsayılan kuralları da dahil olmak üzere) izin verilen en fazla sayısını 1000 kuralları ' dir.

Yeni özel kurallar, yeni bir özel kaynak ile işaretlenmiş (! = "Microsoft"). *RuleId* alanı null veya boş olabilir. Boşsa, Microsoft bir oluşturur. Boş değilse, tüm kurallar (varsayılan ve özel) geçerli bir GUID benzersiz olması gerekir. Aşağıda çekirdek alanları ile ilgili kısıtlamalar gözden geçirin:

-   *originalId* -null veya boş olabilir. Varsa *originalId* olan geçerli bir GUID olmalıdır boş değil.

-   *cceId* -null veya boş olabilir. Varsa *cceId* olan boş değil benzersiz olmalıdır.

-   *ruleType* - bir olarak: kayıt defteri, AuditPolicy veya SecurityPolicy.

-   *Önem derecesi* - bir olarak: Bilinmeyen, kritik, uyarı veya bilgilendirici.

-   *analyzeOperation* -eşit olmalıdır.

-   *auditPolicyId* -geçerli bir GUID olmalıdır.

-   *regValueType* -biri olmalıdır: Int, Long, dize veya MultipleString.

> [!NOTE]
> Hive olmalıdır *LocalMachine*.
>
>

Yeni bir özel kural örneği:

**Kayıt defteri**:
```
    {
    "hive": "LocalMachine",
    "regValueType": "Int",
    "keyPath":
    "System\\\\CurrentControlSet\\\\Services\\\\Netlogon\\\\MyKeyName",
    "valueName": "MyValueName",
    "originalId": "",
    "cceId": "",
    "ruleName": "My new registry rule”, "baselineRuleType": "Registry",
    "expectedValue": "123", "severity": "Critical",
    "analyzeOperation": "Equals",
    "source": "MyCustomSource",
    "state": "Enabled"
   }
```
**Güvenlik İlkesi**:
```
{

   "sectionName": "Privilege Rights",
   "settingName": "SeDenyBatchLogonRight",
   "originalId": "",
   "cceId": "",
   "ruleName": "My new security policy rule", "baselineRuleType":
   "SecurityPolicy",
   "expectedValue": "Guests",
   "severity": "Critical",
   "analyzeOperation": "Equals", "source": " MyCustomSource ",
   "state": "Enabled"
   }
```
**Denetim İlkesi:**
```
   {
   "auditPolicyId": "0cce923a-69ae-11d9-bed3-505054503030",
   "originalId": "",
   "cceId": "",
   "ruleName": " My new audit policy rule ", "baselineRuleType": "AuditPolicy",
   "expectedValue": " Failure",
   "severity": "Critical",
   "analyzeOperation": "Equals", "source": " MyCustomSource ",
   "state": "Enabled"
   }
```

## <a name="file-upload-failures"></a>Dosya karşıya yükleme hatası

Gönderilen yapılandırma dosyası geçersizse değerleri veya biçimlendirme hataları nedeniyle bir hata gösterilir. Düzeltilen yapılandırma dosyası yeniden göndermeden önce hataları düzeltin ve düzeltmek için ayrıntılı hatalar csv rapor indirebilirsiniz.

![](media/security-center-customize-os-security-config/invalid-configuration.png)

Hataları dosyası örneği:

![](media/security-center-customize-os-security-config/errors-file.png)

## <a name="error-codes"></a>Hata kodları

Aşağıdaki liste, tüm olası hatalar var:

| **Hata**                                | **Açıklama**                                                                                                                              |
|------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------|
| BaselineConfiguratiohSchemaVersionError  | Özellik 'schemaVersion', geçersiz veya boş bulundu. Değeri '{0}' olarak ayarlanmalıdır.                                                         |
| BaselineInvalidStringError               | '{0}' özelliği içeremez '\\n'.                                                                                                         |
| BaselineNullRuleError                    | Temel yapılandırma kuralları listesi 'null' değerine sahip bir kural içerir.                                                                         |
| BaselineRuleCceIdNotUniqueError          | CCE-ID '{0}' benzersiz değil.                                                                                                                  |
| BaselineRuleEmptyProperty                | Özellik: '{0}' eksik veya geçersiz olur.                                                                                                       |
| BaselineRuleIdNotInDefault               | Kural bir kaynak özelliği 'Microsoft' var. ancak Microsoft varsayılan kümesinde bulunamadı.                                                   |
| BaselineRuleIdNotUniqueError             | Kural kimliği benzersiz değil.                                                                                                                       |
| BaselineRuleInvalidGuid                  | '{0}' özelliği geçersiz bulundu. Değer geçerli bir GUID değil.                                                                             |
| BaselineRuleInvalidHive                  | Hive, LocalMachine olması gerekir.                                                                                                                   |
| BaselineRuleNameNotUniqueError           | Kural adı benzersiz değil.                                                                                                                 |
| BaselineRuleNotExistInConfigError        | Kural yeni yapılandırmada bulunamadı. Kural silinemiyor.                                                                     |
| BaselineRuleNotFoundError                | Varsayılan kural temel kurallar kümesi bulunamadı.                                                                                        |
| BaselineRuleNotInPlace                   | Kural varsayılan kuralı {0} türü ile eşleşen ve {1} listesinde yer alır.                                                                       |
| BaselineRulePropertyTooLong              | Özellik: '{0}' çok uzun. En fazla izin verilen uzunluk: {1}.                                                                                        |
| BaselineRuleRegTypeInvalidError          | Beklenen değer '{0}', tanımlanan kayıt defteri değeri türü eşleşmiyor.                                                              |
| BaselineRulesetAdded                     | '{0}' kimlikli kurallar kümesi Varsayılan yapılandırmada bulunamadı. Kurallar kümesi eklenemez.                                               |
| BaselineRulesetIdMustBeUnique            | Verilen temel kurallar kümesi '{0}' benzersiz olması gerekir.                                                                                           |
| BaselineRulesetNotFound                  | Kuralları kümesi kimliği '{0}' ve adı '{1}' belirli yapılandırmada bulunamadı. Kurallar kümesi silinemiyor.                                |
| BaselineRuleSourceNotMatch               | '{0}' kimlikli kural zaten tanımlandı.                                                                                                       |
| BaselineRuleTypeDoesntMatch              | '{0}' varsayılan kural türü değil.                                                                                                              |
| BaselineRuleTypeDoesntMatchError         | Kural gerçek türü: {0} ancak ruleType özelliğidir: {1}.                                                                          |
| BaselineRuleUnpermittedChangesError      | Yalnızca 'expectedValue' ve 'state' özelliklerinin değiştirilmesine izin verilmez.                                                                       |
| BaselineTooManyRules                     | Özelleştirilmiş kuralları sayısı izin verilen maksimum {0} kuralları ' dir. Verilen yapılandırma {1} kurallarını içerir. ({2} varsayılan kuralları + özelleştirilmiş {3} kuralları. |
| ErrorNoConfigurationStatus               | Hiçbir yapılandırma durumu bulunamadı. İstenen yapılandırma durumu - 'Default' veya 'Custom' durumu.                                    |
| ErrorNonEmptyRulesetOnDefault            | Yapılandırma durumu, varsayılan olarak ayarlanır. BaselineRulesets listesi null veya boş olması gerekir.                                                          |
| ErrorNullRulesetsPropertyOnCustom        | Verilen yapılandırma durumu 'Custom' ancak 'baselineRulesets' özelliği null veya boş.                                             |
| ErrorParsingBaselineConfig               | Belirtilen yapılandırma geçersiz. Bir veya daha fazla tanımlanan değerlerden bir null ya da geçersiz bir türe sahip.                                  |
| ErrorParsingIsDefaultProperty            | 'Verilen 'configurationStatus' {0}' değeri geçersiz. Değer, yalnızca 'Default' veya 'Custom' olabilir.                                         |
| InCompatibleViewVersion                  | Görünüm sürümü: {0} Bu çalışma alanı türü desteklenmiyor.                                                                                   |
| InvalidBaselineConfigurationGeneralError | Verilen temel yapılandırma ile bir veya daha fazla türü doğrulama hataları bulundu.                                                          |
| ViewConversionError                      | Bu çalışma desteklenen eski sürümü görünümüdür. Görüntüleme dönüştürme başarısız oldu: {0}                                                                 |

Yeterli izinlere sahip değilseniz, genel bir hata alabilirsiniz:

![](media/security-center-customize-os-security-config/general-failure-error.png)

## <a name="next-steps"></a>Sonraki adımlar
Bu makalede, işletim sistemi güvenlik yapılandırması değerlendirmeleri Güvenlik Merkezi'nde özelleştirmek nasıl oluşturulacağını gösterir. Yapılandırma kuralları ve düzeltme hakkında daha fazla bilgi için bkz:

- [Güvenlik Merkezi ortak yapılandırma tanımlayıcıları ve temel kurallar](https://gallery.technet.microsoft.com/Azure-Security-Center-a789e335).
- Güvenlik Merkezi, yapılandırma kuralları için benzersiz tanımlayıcı atamak için CCE (ortak yapılandırma sıralaması) kullanır. Ziyaret [CCE](https://nvd.nist.gov/config/cce/index) daha fazla bilgi için site.
- [Güvenlik yapılandırmalarını düzeltmek](security-center-remediate-os-vulnerabilities.md) güvenlik açıkları, işletim sistemi yapılandırması önerilen güvenlik yapılandırma kuralları eşleşmediğinde çözmek gösterilmektedir.
