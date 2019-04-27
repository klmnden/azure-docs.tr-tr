---
title: İşletim sistemi güvenlik yapılandırmaları (Önizleme) Azure Güvenlik Merkezi'nde özelleştirme | Microsoft Docs
description: Bu makalede Güvenlik Merkezi'nde değerlendirmeleri özelleştirmek nasıl gösterir
services: security-center
documentationcenter: na
author: rkarlin
manager: barbkess
editor: ''
ms.assetid: ''
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/20/2019
ms.author: rkarlin
ms.openlocfilehash: c0c37724e61490c8c33b5e2d37879549bbc6d7ce
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60705504"
---
# <a name="customize-os-security-configurations-in-azure-security-center-preview"></a>İşletim sistemi güvenlik yapılandırmaları (Önizleme) Azure Güvenlik Merkezi'nde özelleştirme

Bu kılavuzda, Azure Güvenlik Merkezi'nde işletim sistemi güvenlik yapılandırması değerlendirmeleri özelleştirmek gösterilmiştir.

## <a name="what-are-os-security-configurations"></a>İşletim sistemi güvenlik yapılandırmaları nelerdir?

Azure Güvenlik Merkezi'nin izlediği güvenlik yapılandırmalarını bir dizi uygulayarak [150'den önerilen kurallar](https://gallery.technet.microsoft.com/Azure-Security-Center-a789e335) işletim sistemini sağlamlaştırma için kuralları dahil olmak üzere ilgili güvenlik duvarları, Denetim, parola ilkeleri ve daha fazla bilgi için. Güvenlik açığı bulunan bir yapılandırmaya sahip bir makine bulundu, Güvenlik Merkezi bir güvenlik önerisi oluşturur.

Kuralları özelleştirerek, kuruluşlar, kendi ortamlarından daha uygun olan yapılandırma seçenekleri denetleyebilirsiniz. Özelleştirilmiş değerlendirme ilkesi ayarlayabilir ve Abonelikteki tüm geçerli makinelerde uygulayın.

> [!NOTE]
> - Şu anda işletim sistemi güvenlik yapılandırması özelleştirmesini Windows Server 2008, 2008 R2, 2012, 2012 R2 ve 2016 sürümleri yalnızca işletim sistemleri için kullanılabilir.
> - Yapılandırma tüm Vm'leri ve seçilen abonelik altındaki tüm çalışma alanlarına bağlı olan bilgisayarlar için geçerlidir.
> - İşletim sistemi güvenlik yapılandırması özelleştirme, yalnızca Güvenlik Merkezi'nin standart katmanında kullanılabilir.
>
>

İşletim sistemi güvenlik yapılandırması kurallarını etkinleştirme ve belirli bir kural devre dışı bırakma, mevcut bir kuralı için istenen ayarlarını değiştirme veya desteklenen kural türlerinden (kayıt defteri, Denetim İlkesi ve güvenlik ilkesi) alarak yeni bir kural ekleyerek özelleştirebilirsiniz. Şu anda, istenen ayar tam bir değer olmalıdır.

Yeni kurallar aynı biçim ve yapısıyla aynı tür var olan diğer kuralları olması gerekir.

> [!NOTE]
> İşletim sistemi güvenlik yapılandırmaları özelleştirmek için rolü atanmış *abonelik sahibi*, *abonelik katkıda bulunanı*, veya *Güvenlik Yöneticisi*.
>
>

## <a name="customize-the-default-os-security-configuration"></a>Varsayılan işletim sistemi Güvenlik Yapılandırması'nı özelleştirme

Güvenlik Merkezi'nde varsayılan işletim sistemi güvenlik yapılandırmasını özelleştirmek için aşağıdakileri yapın:

1.  **Güvenlik Merkezi** panosunu açın.

2.  Sol bölmede seçin **Güvenlik İlkesi**.      

    ![Güvenlik İlkesi listesi](media/security-center-customize-os-security-config/manual-provision.png)

3.  Özelleştirmek istediğiniz abonelik satırının **ayarlarını Düzenle**.

4. Seçin **güvenlik yapılandırmalarını Düzenle**.  

    !["Güvenlik yapılandırmalarını Düzenle" penceresi](media/security-center-customize-os-security-config/blade.png)

5. İndirmek, düzenlemek ve değiştirilen dosya karşıya yükleme adımlarını izleyin.

   > [!NOTE]
   > Varsayılan olarak, karşıdan yapılandırma dosyasının bulunduğu *json* biçimi. Bu dosyayı değiştirme hakkında yönergeler için Git [yapılandırma dosyasını özelleştirme](#customize-the-configuration-file).
   >

6. Değişikliği kaydetmek için seçin **Kaydet**. Aksi takdirde, ilke depolanmaz.

    ![Kaydet düğmesi](media/security-center-customize-os-security-config/save-successfully.png)

   Dosya başarıyla kaydettikten sonra yapılandırma tüm Vm'lere ve abonelik altında çalışma alanlarına bağlı bilgisayarlara uygulanır. İşlem, genellikle birkaç dakika sürer ancak altyapı boyutuna bağlı olarak daha uzun sürebilir.

Herhangi bir noktada, varsayılan durumuna getirmek geçerli ilke yapılandırmasının sıfırlayabilirsiniz. Bunu yapmak için **Düzenle işletim sistemi güvenlik yapılandırması kurallarını** penceresinde **sıfırlama**. Bu seçeneği işaretleyerek onaylayın **Evet** onay açılır pencerede.

![Pencere Sıfırlama onayı](media/security-center-customize-os-security-config/edit-alert.png)

## <a name="customize-the-configuration-file"></a>Yapılandırma dosyasını özelleştirme

Özelleştirme dosyasında, desteklenen her işletim sistemi sürümü, bir dizi kural veya kural kümesi vardır. Her bir kural kümesi, aşağıdaki örnekte gösterildiği gibi kendi adı ve benzersiz bir kimlik vardır:

![Kural kümesi adını ve Kimliğini](media/security-center-customize-os-security-config/custom-file.png)

> [!NOTE]
> Bu örnek dosya Visual Studio'da düzenlendi, ancak JSON Görüntüleyici eklenti varsa de Not Defteri'ni kullanabilirsiniz.
>
>

Özelleştirme dosyası düzenlenirken bir kural veya tümünü değiştirebilirsiniz. Her bir kural kümesi içeren bir *kuralları* bölüm üç kategoriye ayrılır: Kayıt defteri, Denetim İlkesi ve güvenlik ilkesi, burada gösterildiği gibi:

![Üç ruleset kategorileri](media/security-center-customize-os-security-config/rules-section.png)

Her kategorinin kendi öznitelikleri kümesi vardır. Aşağıdaki öznitelikler değiştirebilirsiniz:

- **expectedValue**: Bu özniteliğin alanın veri türü başına desteklenen değerler eşleşmelidir *kural türü*, örneğin:

  - **baselineRegistryRules**: Değer eşleşmelidir [regValueType](https://msdn.microsoft.com/library/windows/desktop/ms724884) bu kuralda tanımlı.

  - **baselineAuditPolicyRules**: Aşağıdaki dize değerlerinden birini kullanın:

    - *Başarı ve başarısızlık*

    - *Başarılı*

  - **baselineSecurityPolicyRules**: Aşağıdaki dize değerlerinden birini kullanın:

    - *Hiç kimse*

    - Örneğin, izin verilen kullanıcı grupları listesi: *Yöneticiler*, *Yedekleme İşletmenleri*

-   **Durum**: Dize seçenekleri içerebilir *devre dışı bırakılmış* veya *etkin*. Bu sürüm için dize büyük/küçük harf duyarlıdır.

Bunlar, yapılandırılabilir tek alanlardır. Dosya biçimini veya boyutunu ihlal değişikliği kaydetmek mümkün olmayacaktır. Geçerli bir JSON yapılandırma dosyasını karşıya yüklemek için gereken bildiren bir hata alırsınız.

Diğer olası hataları listesi için bkz. [hata kodları](#error-codes).

Aşağıdaki üç bölümler önceki kuralların örnekleri içerir. *ExpectedValue* ve *durumu* öznitelikler değiştirilebilir.

**baselineRegistryRules**
```json
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
```json
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
```json
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

Bazı kurallar, farklı işletim sistemi türleri için çoğaltılır. Yinelenen kurallara sahip aynı *originalId* özniteliği.

## <a name="create-custom-rules"></a>Özel kurallar oluşturma

Ayrıca, yeni kurallar oluşturabilirsiniz. Yeni bir kural oluşturmadan önce aşağıdaki kısıtlamaları göz önünde bulundurun:

-   Şema sürümü *baselineId* ve *baselineName* değiştirilemez.

-   Kural kümesi kaldırılamıyor.

-   Kural kümesi eklenemez.

-   Kurallar (varsayılan kuralları dahil olmak üzere) izin verilen en fazla sayısı 1000'dir.

Yeni özel kurallar, yeni bir özel kaynak ile işaretlenir (! = "Microsoft"). *RuleId* alanı null veya boş olabilir. Boşsa, Microsoft bir tane oluşturur. Boş değilse, tüm kurallar arasında (varsayılan ve özel) benzersiz olan geçerli bir GUID olması gerekir. Temel alanlar için aşağıdaki kısıtlamaları gözden geçirin:

-   **originalId**: Null veya boş olabilir. Varsa *originalId* olan boş, geçerli bir GUID olması.

-   **cceId**: Null veya boş olabilir. Varsa *cceId* olduğundan boş değil benzersiz olmalıdır.

-   **kural türü**: (birini) kayıt defteri, AuditPolicy veya SecurityPolicy.

-   **Önem derecesi**: (birini) bilinmiyor, kritik, uyarı ve bilgilendirici.

-   **analyzeOperation**: Olmalıdır *eşittir*.

-   **auditPolicyId**: Geçerli bir GUID olmalıdır.

-   **regValueType**: (birini) Int, Long, dize veya MultipleString.

> [!NOTE]
> Hive olmalıdır *LocalMachine*.
>
>

Yeni bir özel kural örneği:

**Kayıt defteri**:
```json
    {
    "hive": "LocalMachine",
    "regValueType": "Int",
    "keyPath":
    "System\\\\CurrentControlSet\\\\Services\\\\Netlogon\\\\MyKeyName",
    "valueName": "MyValueName",
    "originalId": "",
    "cceId": "",
    "ruleName": "My new registry rule", "baselineRuleType": "Registry",
    "expectedValue": "123", "severity": "Critical",
    "analyzeOperation": "Equals",
    "source": "MyCustomSource",
    "state": "Enabled"
    }
```
**Güvenlik İlkesi**:
```json
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
```json
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

## <a name="file-upload-failures"></a>Karşıya dosya yükleme hataları

Değerleri veya biçimlendirme içindeki hatalar nedeniyle gönderilen yapılandırma dosyası geçersiz bir hata, aşağıdaki gibi görüntülenir **kaydetme eylemi başarısız oldu**. Düzeltilen yapılandırma dosyası yeniden önce hataları düzeltin ve düzeltmek için ayrıntılı hatalar .csv raporu indirebilirsiniz.

Bir hata dosyası örneği:

![Hata dosyası örneği](media/security-center-customize-os-security-config/errors-file.png)

## <a name="error-codes"></a>Hata kodları

Tüm olası hatalar aşağıdaki tabloda listelenmiştir:

| **Hata:**                                | **Açıklama**                                                                                                                              |
|------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------|
| BaselineConfigurationSchemaVersionError  | Özellik *schemaVersion* geçersiz veya boş bulunamadı. Değer ayarlanmalıdır *{0}*.                                                         |
| BaselineInvalidStringError               | Özellik *{0}* içeremez  *\\n*.                                                                                                         |
| BaselineNullRuleError                    | Değerine sahip bir kural taban çizgisi yapılandırma kuralları listesi içeren *null*.                                                                         |
| BaselineRuleCceIdNotUniqueError          | CCE kimliği *{0}* benzersiz değil.                                                                                                                  |
| BaselineRuleEmptyProperty                | Özellik *{0}* eksik veya geçersiz.                                                                                                       |
| BaselineRuleIdNotInDefault               | Kural için bir kaynak özelliği yok *Microsoft* ancak Microsoft varsayılan kümesinde bulunamadı.                                                   |
| BaselineRuleIdNotUniqueError             | Kural kimliği benzersiz değil.                                                                                                                       |
| BaselineRuleInvalidGuid                  | Özellik *{0}* geçersiz bulundu. Değer geçerli bir GUID değil.                                                                             |
| BaselineRuleInvalidHive                  | Hive LocalMachine olması gerekir.                                                                                                                   |
| BaselineRuleNameNotUniqueError           | Kural adı benzersiz değil.                                                                                                                 |
| BaselineRuleNotExistInConfigError        | Kural yeni yapılandırmada bulunamadı. Kural silinemiyor.                                                                     |
| BaselineRuleNotFoundError                | Varsayılan kural temel kural kümesi bulunamadı.                                                                                        |
| BaselineRuleNotInPlace                   | Kuralın bir varsayılan kural türüyle eşleşmesi {0} ve listelenen {1} listesi.                                                                       |
| BaselineRulePropertyTooLong              | Özellik *{0}* çok uzun. İzin verilen en fazla uzunluk: {1}.                                                                                        |
| BaselineRuleRegTypeInvalidError          | Beklenen değer *{0}* tanımlanan kayıt defteri değeri türü ile eşleşmiyor.                                                              |
| BaselineRulesetAdded                     | Kimlikli kural kümesi *{0}* Varsayılan yapılandırmada bulunamadı. Kural kümesi eklenemez.                                               |
| BaselineRulesetIdMustBeUnique            | Belirtilen temel ruleset *{0}* benzersiz olması gerekir.                                                                                           |
| BaselineRulesetNotFound                  | Kimlikli kural kümesi *{0}* ve ad *{1}* belirli yapılandırmada bulunamadı. Kural kümesi silinemiyor.                                |
| BaselineRuleSourceNotMatch               | Kimlikli kural *{0}* zaten tanımlandı.                                                                                                       |
| BaselineRuleTypeDoesntMatch              | Varsayılan kural türü *{0}*.                                                                                                              |
| BaselineRuleTypeDoesntMatchError         | Kural gerçek türü *{0}*, ancak *kural türü* özelliği *{1}*.                                                                          |
| BaselineRuleUnpermittedChangesError      | Yalnızca *expectedValue* ve *durumu* özellikleri değiştirilmesine izin verilir.                                                                       |
| BaselineTooManyRules                     | Maksimum sayısı izin verilen özelleştirilmiş kurallar {0} kuralları. Belirli bir yapılandırma içeriyor {1} kuralları {2} varsayılan kuralları ve {3} özelleştirilmiş kurallar. |
| ErrorNoConfigurationStatus               | Hiçbir yapılandırma durumu bulunamadı. İstenen yapılandırma durumunun durumu: *Varsayılan* veya *özel*.                                    |
| ErrorNonEmptyRulesetOnDefault            | Yapılandırma durumu, varsayılan olarak ayarlanır. *BaselineRulesets* listesi null veya boş olmalıdır.                                                          |
| ErrorNullRulesetsPropertyOnCustom        | Belirtilen yapılandırma durumu *özel* ancak *baselineRulesets* özelliği null veya boş.                                             |
| ErrorParsingBaselineConfig               | Belirtilen yapılandırma geçersiz. Bir veya daha fazla tanımlı değerlerin bir null değer ya da geçersiz bir türe sahip.                                  |
| ErrorParsingIsDefaultProperty            | Verilen *configurationStatus* değer *{0}* geçersiz. Değer yalnızca olabilir *varsayılan* veya *özel*.                                         |
| InCompatibleViewVersion                  | Görünüm sürümü *{0}* olduğu *değil* üzerinde bu çalışma alanı türü desteklenmiyor.                                                                                   |
| InvalidBaselineConfigurationGeneralError | Belirli bir taban çizgisi yapılandırmasını bir veya daha fazla tür doğrulama hataları bulundu.                                                          |
| ViewConversionError                      | Çalışma alanını destekleyen daha eski bir sürüm görünümüdür. Görüntüleme dönüştürme başarısız oldu: {0}.                                                                 |

Yeterli izinleriniz yoksa, burada gösterildiği gibi bir genel hata hatası alabilirsiniz:

!["Kaydetme eylemi başarısız oldu" hata iletisi](media/security-center-customize-os-security-config/general-failure-error.png)

## <a name="next-steps"></a>Sonraki adımlar
Bu makalede Güvenlik Merkezi'ndeki işletim sistemi güvenlik yapılandırması değerlendirmeleri özelleştirmek açıklanır. Yapılandırma kuralları ve düzeltme hakkında daha fazla bilgi için bkz:

- [Güvenlik Merkezi ortak yapılandırma tanımlayıcılarını ve temel kurallar](https://gallery.technet.microsoft.com/Azure-Security-Center-a789e335).
- Güvenlik Merkezi, benzersiz tanımlayıcıları için yapılandırma kuralları atamak için yaygın yapılandırma numaralandırması (CCE) kullanır. Daha fazla bilgi için [CCE](https://nvd.nist.gov/config/cce/index).
- İşletim sistemi yapılandırması önerilen güvenlik yapılandırması kurallarını eşleşmediğinde, güvenlik açıklarını gidermek için bkz: [güvenlik yapılandırmalarını düzeltme](security-center-remediate-os-vulnerabilities.md).
