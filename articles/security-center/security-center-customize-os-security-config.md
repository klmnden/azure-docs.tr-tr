---
title: İşletim sistemi güvenlik yapılandırmalarını (Önizleme) Azure Güvenlik Merkezi'nde özelleştirme | Microsoft Docs
description: Bu makalede, Güvenlik Merkezi değerlendirmeleri özelleştirmek gösterilmiştir
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: ''
ms.assetid: ''
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/25/2018
ms.author: terrylan
ms.openlocfilehash: f12441a960db9f1c45bca2a5b95f3669923c7e3d
ms.sourcegitcommit: ded74961ef7d1df2ef8ffbcd13eeea0f4aaa3219
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2018
ms.locfileid: "28200019"
---
# <a name="customize-os-security-configurations-in-azure-security-center-preview"></a>İşletim sistemi güvenlik yapılandırmalarını (Önizleme) Azure Güvenlik Merkezi'nde özelleştirme

Bu kılavuz, Azure Güvenlik Merkezi'nde Güvenlik Yapılandırması değerlendirmeleri işletim sistemi özelleştirme gösterilmiştir.

## <a name="what-are-os-security-configurations"></a>İşletim sistemi güvenlik yapılandırmalarını nelerdir?

Azure Güvenlik Merkezi güvenlik yapılandırmalarını bir dizi uygulayarak izler [150'den önerilen kurallar](https://gallery.technet.microsoft.com/Azure-Security-Center-a789e335) işletim sistemi sağlamlaştırma için kuralları dahil olmak üzere ilgili güvenlik duvarları, denetleme, parola ilkeleri ve daha fazla bilgi için. Bir güvenlik açığı yapılandırmaya sahip bir makine bulunursa, Güvenlik Merkezi güvenlik açısından oluşturur.

Kuralları özelleştirerek, kuruluşların kendi ortamı için daha uygun olan yapılandırma seçenekleri kontrol edebilirsiniz. Özelleştirilmiş değerlendirme ilkesi ayarlayın ve Abonelikteki tüm geçerli makinelerde uygulayın.

> [!NOTE]
> - Şu anda işletim sistemi güvenlik yapılandırmasının özelleştirme Windows Server 2008, 2008 R2, 2012 ve 2012 R2 işletim sistemleri için kullanılabilir.
> - Yapılandırma tüm VM'ler ve seçili abonelik altında tüm çalışma alanlarına bağlanan bilgisayarlar için geçerlidir.
> - İşletim sistemi güvenlik yapılandırması özelleştirmesi yalnızca Güvenlik Merkezi'nin standart katmanında mevcuttur.
>
>

Etkinleştirerek ve belirli bir kuralın devre dışı bırakma, mevcut bir kuralı için istenen ayarlarını değiştirme veya desteklenen kural türlerine (kayıt defteri, Denetim ilkesini ve güvenlik ilkesi) dayalı yeni bir kuralın eklenmesi, işletim sistemi güvenlik yapılandırma kuralları özelleştirebilirsiniz. Şu anda, istenen ayar tam bir değer olmalıdır.

Yeni kurallar aynı biçim ve yapısıyla aynı türde diğer kuralların olması gerekir.

> [!NOTE]
> İşletim sistemi güvenlik yapılandırmalarını özelleştirmek için rolü atanmış *abonelik sahibi*, *abonelik katkıda bulunan*, veya *Güvenlik Yöneticisi*.
>
>

## <a name="customize-the-default-os-security-configuration"></a>Varsayılan işletim sistemi güvenlik yapılandırması özelleştirme

Varsayılan işletim sistemi güvenlik yapılandırması Güvenlik Merkezi'nde özelleştirmek için aşağıdakileri yapın:

1.  **Güvenlik Merkezi** panosunu açın.

2.  Sol bölmede seçin **Güvenlik İlkesi**.  
    **Güvenlik Merkezi - Güvenlik İlkesi** penceresi açılır.

    ![Güvenlik İlkesi listesi](media/security-center-customize-os-security-config/open-security-policy.png)

3.  Özelleştirme için gerçekleştirmek istediğiniz aboneliği seçin.

4. Altında **İlkesi bileşenleri**seçin **Düzenle güvenlik yapılandırmalarını**.  
    **Düzenle güvenlik yapılandırmalarını** penceresi açılır.

    !["Güvenlik yapılandırmalarını Düzenle" penceresi](media/security-center-customize-os-security-config/blade.png)

5. Sağ bölmede, indirme, düzenleme ve değiştirilen dosya karşıya yükleme adımlarını izleyin.

   > [!NOTE]
   > Varsayılan olarak, indirdiğiniz yapılandırma dosyasının bulunduğu *json* biçimi. Bu dosyayı değiştirme hakkında yönergeler için Git [yapılandırma dosyasını özelleştirme](#customize-the-configuration-file).
   >

   Dosyası başarıyla kaydettikten sonra yapılandırma tüm VM'ler ve abonelik altında tüm çalışma alanlarına bağlanan bilgisayarlara uygulanır. İşlem genellikle birkaç dakika sürer ancak altyapı büyüklüğüne bağlı olarak uzun sürebilir.

6. Değişikliği kaydetmek için seçin **kaydetmek**. Aksi durumda, ilke depolanmaz.

    ![Kaydet düğmesi](media/security-center-customize-os-security-config/save-successfully.png)

Herhangi bir noktada varsayılan durumuna getirmek geçerli ilke yapılandırması sıfırlayabilirsiniz. Bunu yapmak için **Düzenle OS güvenlik yapılandırma kuralları** penceresinde, seçin **sıfırlama**. Bu seçeneği işaretleyerek onaylayın **Evet** onay açılır pencerede.

![Pencere Sıfırlama onayı](media/security-center-customize-os-security-config/edit-alert.png)

## <a name="customize-the-configuration-file"></a>Yapılandırma dosyasını özelleştirme

Özelleştirme dosyasında, desteklenen her işletim sistemi sürümü kuralları ya da ruleset içeriyor. Her ruleset, aşağıdaki örnekte gösterildiği gibi kendi adı ve benzersiz kimliği vardır:

![Ruleset adı ve kimliği](media/security-center-customize-os-security-config/custom-file.png)

> [!NOTE]
> Bu örnek dosyası Visual Studio'da düzenlendiği ancak JSON yüklü Görüntüleyici eklentisini varsa, Not Defteri de kullanabilirsiniz.
>
>

Özelleştirme dosyası düzenlediğinizde, bir kural veya tümünü değiştirebilirsiniz. Her ruleset içeren bir *kuralları* üç kategoriye ayrılır bölüm: kayıt defteri, Denetim ilkesini ve güvenlik ilkesi, aşağıda gösterildiği gibi:

![Üç ruleset kategorileri](media/security-center-customize-os-security-config/rules-section.png)

Her kategori kendi öznitelikleri kümesi vardır. Aşağıdaki öznitelikler değiştirebilirsiniz:

- **expectedValue**: Bu özniteliğin alanın veri türünü başına desteklenen değerler eşleşmelidir *kural türü*, örneğin:

  - **baselineRegistryRules**: değer eşleşmelidir [regValueType](https://msdn.microsoft.com/library/windows/desktop/ms724884) bu kuralda tanımlı.

  - **baselineAuditPolicyRules**: Aşağıdaki dize değerleri birini kullanın:

    - *Başarı ve başarısızlık*

    - *Başarılı*

  - **baselineSecurityPolicyRules**: Aşağıdaki dize değerleri birini kullanın:

    - *Hiç kimse*

    - Listesi izin kullanıcı grupları, örneğin: *Yöneticiler*, *Backup Operators*

-   **durumu**: dize seçenekleri içerebilir *devre dışı* veya *etkin*. Bu özel Önizleme sürümü için dize büyük/küçük harf duyarlıdır.

Bunlar, yapılandırılabilir yalnızca alanlardır. Dosya biçimi veya boyutu ihlal değişikliği kaydetmek mümkün olmayacaktır. Dosya işlendiğinde aşağıdaki hata iletisini oluşur:

![Güvenlik Yapılandırma hata iletisi](media/security-center-customize-os-security-config/invalid-json.png)

Diğer olası hataları listesi için bkz: [hata kodları](#error-codes).

Aşağıdaki üç bölümler önceki kuralları örnekleri içerir. *ExpectedValue* ve *durumu* öznitelikleri değiştirilebilir.

**baselineRegistryRules**
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

**baselineAuditPolicyRules**
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
    }
```

**baselineSecurityPolicyRules**
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
    }
```

Bazı kurallar farklı işletim sistemi türleri için yinelenir. Aynı olan yinelenen kurallar *originalId* özniteliği.

## <a name="create-custom-rules"></a>Özel kurallar oluşturun

Ayrıca yeni kurallar oluşturabilirsiniz. Yeni bir kural oluşturmadan önce aşağıdaki kısıtlamaları göz önünde bulundurun:

-   Şema sürümü *baselineId* ve *baselineName* değiştirilemez.

-   RuleSet kaldırılamaz.

-   RuleSet eklenemez.

-   Kurallar (varsayılan kuralları da dahil olmak üzere) izin verilen en fazla sayısını 1000'dir.

Yeni özel kurallar, yeni bir özel kaynak ile işaretlenmiş (! = "Microsoft"). *RuleId* alanı null veya boş olamaz. Boşsa, Microsoft bir oluşturur. Boş değilse, tüm kurallar (varsayılan ve özel) benzersiz olduğundan geçerli bir GUID olması gerekir. Çekirdek alanları için aşağıdaki kısıtlamaları gözden geçirin:

-   **originalId**: null veya boş olabilir. Varsa *originalId* olan geçerli bir GUID olmalıdır boş değil.

-   **cceId**: null veya boş olabilir. Varsa *cceId* olan boş değil benzersiz olmalıdır.

-   **ruleType**: (seçin) kayıt defteri, AuditPolicy veya SecurityPolicy.

-   **Önem derecesi**: (seçin) bilinmeyen, kritik, uyarı veya bilgilendirici.

-   **analyzeOperation**: olmalıdır *eşittir*.

-   **auditPolicyId**: geçerli bir GUID olmalıdır.

-   **regValueType**: (seçin) Int, Long, dize veya MultipleString.

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
**Denetim İlkesi**:
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

Gönderilen yapılandırma dosyası değerleri veya biçimlendirme hataları nedeniyle geçersizse, bir hata görüntülenir. Düzeltilen yapılandırma dosyası yeniden gönderin önce hataları düzeltin ve düzeltmek için ayrıntılı hatalar .csv rapor indirebilirsiniz.

!["Kaydet eylemi başarısız oldu" hata iletisi](media/security-center-customize-os-security-config/invalid-configuration.png)

Bir hata dosyası örneği:

![Hata dosyası örneği](media/security-center-customize-os-security-config/errors-file.png)

## <a name="error-codes"></a>Hata kodları

Tüm olası hatalar aşağıdaki tabloda listelenmiştir:

| **Hata**                                | **Açıklama**                                                                                                                              |
|------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------|
| BaselineConfiguratiohSchemaVersionError  | Özellik *schemaVersion* geçersiz veya boş bulunamadı. Değer ayarlamak *{0}*.                                                         |
| BaselineInvalidStringError               | Özellik *{0}* içeremez  *\\ n* .                                                                                                         |
| BaselineNullRuleError                    | Değerine sahip bir kural taban çizgisi yapılandırma kuralları listesi içeren *null*.                                                                         |
| BaselineRuleCceIdNotUniqueError          | CCE kimliği *{0}* benzersiz değil.                                                                                                                  |
| BaselineRuleEmptyProperty                | Özellik *{0}* eksik veya geçersiz.                                                                                                       |
| BaselineRuleIdNotInDefault               | Kuralın kaynak özelliğine *Microsoft* ancak Microsoft varsayılan kümesinde bulunamadı.                                                   |
| BaselineRuleIdNotUniqueError             | Kural kimliği benzersiz değil.                                                                                                                       |
| BaselineRuleInvalidGuid                  | Özellik *{0}* geçersiz bulundu. Değer geçerli bir GUID değil.                                                                             |
| BaselineRuleInvalidHive                  | Hive, LocalMachine olması gerekir.                                                                                                                   |
| BaselineRuleNameNotUniqueError           | Kural adı benzersiz değil.                                                                                                                 |
| BaselineRuleNotExistInConfigError        | Kural yeni yapılandırmada bulunamadı. Kural silinemiyor.                                                                     |
| BaselineRuleNotFoundError                | Kural taban çizgisi ruleset varsayılan bulunamadı.                                                                                        |
| BaselineRuleNotInPlace                   | Kural varsayılan kuralı {0} türü ile eşleşen ve {1} listesinde yer alır.                                                                       |
| BaselineRulePropertyTooLong              | Özellik *{0}* çok uzun. En fazla izin verilen uzunluk: {1}.                                                                                        |
| BaselineRuleRegTypeInvalidError          | Beklenen değer *{0}* tanımlanan kayıt defteri değeri türü eşleşmiyor.                                                              |
| BaselineRulesetAdded                     | Kimliğine sahip ruleset *{0}* Varsayılan yapılandırmada bulunamadı. Ruleset eklenemez.                                               |
| BaselineRulesetIdMustBeUnique            | Taban çizgisine ruleset *{0}* benzersiz olması gerekir.                                                                                           |
| BaselineRulesetNotFound                  | Kimliğine sahip ruleset *{0}* ve ad *{1}* belirli yapılandırmada bulunamadı. Ruleset silinemiyor.                                |
| BaselineRuleSourceNotMatch               | Kimliğine sahip kural *{0}* zaten tanımlandı.                                                                                                       |
| BaselineRuleTypeDoesntMatch              | Varsayılan kural türü *{0}*.                                                                                                              |
| BaselineRuleTypeDoesntMatchError         | Kural gerçek türü *{0}*, ancak *ruleType* özelliği *{1}*.                                                                          |
| BaselineRuleUnpermittedChangesError      | Yalnızca *expectedValue* ve *durumu* özellikleri değiştirilmesine izin verilir.                                                                       |
| BaselineTooManyRules                     | En fazla izin verilen özelleştirilmiş kuralları {0} kuralları sayısıdır. Verilen yapılandırma {1} kurallarını, {2} varsayılan kuralları ve özelleştirilmiş {3} kurallarını içerir. |
| ErrorNoConfigurationStatus               | Hiçbir yapılandırma durumu bulunamadı. İstenen yapılandırma durumunun durumu: *varsayılan* veya *özel*.                                    |
| ErrorNonEmptyRulesetOnDefault            | Varsayılan olarak durumu yapılandırmasını Ayarla. *BaselineRulesets* listesi null veya boş olması gerekir.                                                          |
| ErrorNullRulesetsPropertyOnCustom        | Verilen yapılandırma durumu *özel* ancak *baselineRulesets* özelliği null veya boş.                                             |
| ErrorParsingBaselineConfig               | Belirtilen yapılandırma geçersiz. Bir veya daha fazla değerlere bir null ya da geçersiz bir türe sahip.                                  |
| ErrorParsingIsDefaultProperty            | Verilen *configurationStatus* değeri *{0}* geçersiz. Değer yalnızca olabilir *varsayılan* veya *özel*.                                         |
| InCompatibleViewVersion                  | Görünüm sürüm *{0}* olan *değil* bu çalışma alanı türü desteklenmiyor.                                                                                   |
| InvalidBaselineConfigurationGeneralError | Verilen temel yapılandırma ile bir veya daha fazla türü doğrulama hataları bulundu.                                                          |
| ViewConversionError                      | Çalışma alanını destekleyen daha eski bir sürüm görünümüdür. Görüntüleme dönüştürme başarısız oldu: {0}.                                                                 |

Yeterli izinlere sahip değilseniz, aşağıda gösterildiği gibi bir genel hata oluştu. hata alabilirsiniz:

!["Kaydet eylemi başarısız oldu" hata iletisi](media/security-center-customize-os-security-config/general-failure-error.png)

## <a name="next-steps"></a>Sonraki adımlar
Bu makalede ele alınan işletim sistemi güvenlik yapılandırması değerlendirmeleri Güvenlik Merkezi'nde özelleştirmeyi. Yapılandırma kuralları ve düzeltme hakkında daha fazla bilgi için bkz:

- [Güvenlik Merkezi ortak yapılandırma tanımlayıcıları ve temel kurallar](https://gallery.technet.microsoft.com/Azure-Security-Center-a789e335).
- Güvenlik Merkezi, benzersiz tanımlayıcıları için yapılandırma kuralları atamak için yaygın yapılandırma numaralandırması (CCE) kullanır. Daha fazla bilgi için bkz: [CCE](https://nvd.nist.gov/config/cce/index).
- İşletim sistemi yapılandırması önerilen güvenlik yapılandırma kuralları eşleşmediğinde güvenlik açıkları çözümlemek için bkz: [güvenlik yapılandırmalarını düzeltmek](security-center-remediate-os-vulnerabilities.md).
