---
title: AWS maliyetlerinden ve Azure maliyet Yönetimi'nde kullanım yönetme
description: Bu makalede, maliyet analizi ve bütçelerini maliyet Yönetimi'nde AWS maliyetlerinden ve kullanımını yönetmek için nasıl kullanılacağını anlamanıza yardımcı olur.
services: cost-management
keywords: ''
author: bandersmsft
ms.author: banders
ms.date: 04/26/2019
ms.topic: conceptual
ms.service: cost-management
manager: ormaoz
ms.custom: ''
ms.openlocfilehash: 57e66d449b194662bfc03f7e130cf49c02a15793
ms.sourcegitcommit: a52d48238d00161be5d1ed5d04132db4de43e076
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67275701"
---
# <a name="manage-aws-costs-and-usage-in-azure"></a>AWS maliyetlerinden ve kullanımı Azure ile yönetme

Ayarlama ve Azure maliyet yönetimi için AWS maliyet ve kullanım raporu tümleştirmesi yapılandırılmış sonra AWS maliyetlerinden ve kullanım yönetmeye başlamak hazırsınız. Bu makalede, maliyet analizi ve bütçelerini maliyet Yönetimi'nde AWS maliyetlerinden ve kullanımını yönetmek için nasıl kullanılacağını anlamanıza yardımcı olur.

Tümleştirme önce yapılandırmadıysanız, bkz. [kümesi ayarlama ve AWS kullanım raporu tümleştirmesini yapılandırma](aws-integration-set-up-configure.md).

_Başlamadan önce_: Maliyet Analizi ile bilmiyorsanız bkz [maliyet analizi ile maliyetleri analiz](quick-acm-cost-analysis.md) hızlı başlangıç. Ve azure'da bütçelerle emin değilseniz bkz [oluşturun ve Azure bütçelerini yönetin](tutorial-acm-create-budgets.md) öğretici.

## <a name="view-aws-costs-in-cost-analysis"></a>Görünüm AWS maliyetlerinden maliyet analizi

AWS maliyetlerinden aşağıdaki kapsamları maliyet analizi mevcuttur:

- Bir yönetim grubundaki bağlı AWS hesapları
- Bağlantılı AWS hesabı maliyetleri
- AWS hesabı maliyetleri birleştirilir.

Sonraki bölümlerde, her biri için maliyet ve kullanım verileri görebilmesi için kapsam kullanımını anlatmaktadır.

### <a name="view-aws-linked-accounts-under-a-management-group"></a>Bir yönetim grubundaki görünümü bağlı AWS hesapları

Yönetim Grup kapsamı kullanarak maliyetlerini görüntüleme farklı aboneliklerden gelen toplam maliyetleri görmek için tek yolu olduğundan ve bağlantılı hesaplar. Bir yönetim grubunu kullanarak, Bulutlar arası görünüm sağlar.

Maliyet analizi Kapsam Seçici'yi açın ve bağlı AWS hesaplarınızı tutar yönetim grubunu seçin. Azure portalında bir örnek görüntü şöyledir:

![Select kapsam görünümü örneği](./media/aws-integration-manage/select-scope01.png)



Maliyet analizi (Azure ve AWS) sağlayıcısı tarafından gruplandırılmış, yönetim grubu maliyetini gösteren bir örnek aşağıda verilmiştir.

![Üç aylık dönem içinde maliyet analizi için Azure ve AWS maliyetleri gösteren örnek](./media/aws-integration-manage/cost-analysis-aws-azure.png)

### <a name="view-aws-linked-account-costs"></a>Görünüm bağlı AWS hesabı maliyetleri

AWS bağlantı hesabı maliyetleri, Kapsam Seçici'yi açın ve AWS seçebilir hesabı bağlı. AWS bağlayıcısında tanımlandığı şekilde bağlantılı hesaplar bir yönetim grubu için ilişkili olduğunu unutmayın.

Will, AWS hesabı kapsam bağlı bir örnek aşağıda verilmiştir.

![Select kapsam görünümü örneği](./media/aws-integration-manage/select-scope02.png)



### <a name="view-aws-consolidated-account-costs"></a>Görünüm AWS hesabı maliyetleri birleştirilir.

AWS görüntülemek için hesap maliyetleri, Kapsam Seçici'yi açın ve AWS seçin birleştirilmiş hesabı birleştirilir. Will, AWS hesabı kapsam birleştirilmiş bir örnek aşağıda verilmiştir.

![Select kapsam görünümü örneği](./media/aws-integration-manage/select-scope03.png)



Bu kapsam hesapları birleşik AWS hesapla ilişkili tüm AWS toplu bir görünümünü bağlı sağlar. Hesabı, hizmet adına göre gruplandırılmış bir AWS maliyetlerini birleştirilmiş gösteren bir örnek aşağıda verilmiştir.

![Maliyet analizi maliyetleri AWS gösteren örnek birleştirilmiş](./media/aws-integration-manage/cost-analysis-aws-consolidated.png)

### <a name="dimensions-available-for-filtering-and-grouping"></a>Filtreleme ve gruplandırma için kullanılabilir boyutları

Aşağıdaki tablo, Grup ve maliyet analizi tarafından filtresi için kullanılabilir boyutları açıklar.

| Boyut | Amazon Yinele üstbilgisi | Kapsamlar | Açıklamalar |
| --- | --- | --- | --- |
| Kullanılabilirlik alanı | lineitem/AvailabilityZone | Tümü |   |
| Location | Ürün/bölge | Tümü |   |
| Ölçüm |   | Tümü |   |
| Ölçüm kategorisi | lineItem/ProductCode | Tümü |   |
| Ölçüm alt kategorisi | lineitem/UsageType | Tümü |   |
| İşlem | İşlem başına lineItem | Tümü |   |
| Resource | lineItem/ResourceId | Tümü |   |
| Kaynak türü | Ürün/Instancetype | Tümü | Ürün/Instancetype null ise lineItem/UsageType kullanılır. |
| ResourceGuid | Yok | Tümü | Azure ölçüm GUID. |
| Hizmet adı | Ürün/ProductName | Tümü | Ürün/ProductName null ise lineItem/ProductCode kullanılır. |
| Hizmet katmanı |   |   |   |
| Abonelik Kimliği | lineItem/UsageAccountId | Birleştirilmiş bir hesap ve yönetim grubu |   |
| Abonelik adı | Yok | Birleştirilmiş bir hesap ve yönetim grubu | Hesap adlarını, AWS kuruluş API'si kullanılarak toplanır. |
| Etiket | resourceTags /\* | Tümü | _Kullanıcı:_ önek, Bulutlar arası etiketleri izin vermek için kullanıcı tanımlı etiketlerini kaldırılır. _Aws:_ önek değişmeden. |
| Fatura hesap kimliği | Fatura/PayerAccountId | Yönetim grubu |   |
| Fatura hesap adı | Yok | Yönetim grubu | Hesap adlarını, AWS kuruluş API'si kullanılarak toplanır. |
| Sağlayıcı | Yok | Yönetim grubu | AWS veya Azure. |

## <a name="set-budgets-on-aws-scopes"></a>AWS kapsamları kümesi bütçeleri

Bütçeleri, maliyetleri ve sürücü sorumluluk kuruluşunuzdaki proaktif bir şekilde yönetmek için kullanın. Bütçe birleştirilmiş AWS hesabı ve AWS bağlı hesabı kapsamları ayarlanır. Maliyet Yönetimi'nde gösterilen birleştirilmiş AWS hesabınız bütçelerini örneği aşağıda verilmiştir:

![Hesap için bir AWS bütçelerini gösteren örnek birleştirilmiş](./media/aws-integration-manage/budgets-aws-consolidated-account01.png)

## <a name="aws-data-collection-process"></a>AWS veri toplama işlemi

AWS Bağlayıcısı'nı ayarladıktan sonra veri toplama ve bulma işlemleri başlatın. Bu, tüm kullanım verilerini toplamak için birkaç saat sürebilir. Süre bağlıdır:

- AWS S3 demetini geçerli dosyaları işlemek için gereken süre.
- AWS bağlı hesabı kapsamları ve AWS birleştirilmiş bir hesap oluşturmak için gereken süre.
- AWS sıklığını ve zaman Maliyet ve kullanım raporu dosyaları içinde S3 demetini yazma

## <a name="aws-integration-pricing"></a>AWS tümleştirme fiyatlandırması

Her bir AWS Bağlayıcısı 90 gün ücretsiz kullanım hizmeti deneme alır. Genel Önizleme süresince ücretsizdir.

Liste fiyatının % AWS aylık maliyetlerinizi 1 ' dir. Önceki ayın Faturalanan maliyetlerinizi ödersiniz her ay temel.

AWS API'leri erişme, ek ücrete neden.

## <a name="aws-integration-limitations"></a>AWS tümleştirme sınırlamaları

- Maliyet yönetimi, birden çok para birimi türü içeren maliyet raporları desteklemiyor. Birden çok para birimi olan bir kapsam seçin, bir hata iletisi gösterilir.
- AWS GovCloud (ABD), AWS Devleti veya AWS Çin bulut bağlayıcılarını desteklemez.
- Maliyet yönetimi, AWS gösterir _kullanım maliyetleri_ yalnızca. Vergi, destek, para iadesi, RI, KREDİLERİ veya herhangi bir ücret türü henüz desteklenmiyor.

## <a name="troubleshooting-aws-integration"></a>AWS tümleştirme sorunlarını giderme

Sık karşılaşılan sorunları çözmek için aşağıdaki sorun giderme bilgileri kullanın.

### <a name="no-permission-to-aws-linked-accounts"></a>AWS bağlantılı hesaplar için izin yok

**Hata kodu:** _Yetkisiz_

Bağlantılı AWS hesapları maliyetleri erişim izni almak için iki yolu vardır:

- AWS bağlantılı hesapları olan yönetim grubuna erişim elde eder.
- Biri bağlantılı AWS hesabına izin vermesini vardır.

Varsayılan olarak, AWS bağlayıcı oluşturan bağlayıcı oluşturulan tüm nesnelerin sahiptir. Dahil olmak üzere, AWS birleştirilmiş bir hesap ve AWS hesabına bağlanır.

Bağlayıcı ayarlarını doğrulamak için en az bir katkıda bulunan rolü gerekir, okuyucu bağlayıcı ayarları doğrulanamıyor

### <a name="collection-failed-with-assumerole"></a>Koleksiyon AssumeRole ile başarısız oldu

**Hata kodu:** _FailedToAssumeRole_

Bu hata, maliyet Yönetimi AWS AssumeRole API'sini çağırmak oluşturulamıyor olduğu anlamına gelir. Bu sorun, rol tanımı ile ilgili bir sorun nedeniyle oluşabilir. Aşağıdaki koşulların doğru olduğundan emin olun:

- Dış kimlik, bir rol tanımı ve bağlayıcı tanımı ile aynıdır.
- Rol türü **siz veya 3. taraf için başka bir AWS hesabı ait.**
- **MFA gerektiren** seçim temizlenir.
- AWS rolündeki güvenilen AWS hesabı _432263259397_.

### <a name="collection-failed-with-access-denied"></a>Erişim reddedildi ile başarısız oldu

- **Hata kodu:** _AccessDeniedReportDefinitions_ 
- **Hata kodu:** _AccessDeniedListReports_ 
- **Hata kodu:** _AccessDeniedDownloadReport_ 

Bu hata, maliyet Yönetimi Amazon S3 demetini Yinele Çantanızdaki erişemiyor olduğu anlamına gelir iletileri. Rolüne eklenmiş AWS JSON ilkesi alt kısmında gösterilen örneğe benzeyen emin [AWS'de bir rol ve ilke oluşturma](aws-integration-set-up-configure.md#create-a-role-and-policy-in-aws) bölümü.

### <a name="collection-failed-since-we-did-not-find-the-cost-and-usage-report"></a>Maliyet ve kullanım raporu bulamadık koleksiyonu başarısız oldu

**Hata kodu:** _FailedToFindReport_

Bu hata, maliyet Yönetimi Bağlayıcısı tanımlandı maliyet ve kullanım raporu bulunamıyor anlamına gelir. Değil silindi ve rolüne eklenmiş AWS JSON ilkesi alt kısmında gösterilen örneğe benzeyen emin [AWS'de bir rol ve ilke oluşturma](aws-integration-set-up-configure.md#create-a-role-and-policy-in-aws) bölümü.

### <a name="unable-to-create-or-verify-connector-due-to-cost-and-usage-report-definitions-mismatch"></a>Oluşturulamıyor veya maliyet ve kullanım raporu tanımlarını uyuşmazlığı nedeniyle bağlayıcı doğrulayın

**Hata kodu:** _ReportIsNotValid_

Bu hata, AWS maliyet ve kullanım raporu tanımına ilişkili Biz bu rapor için özel ayarlar gereklidir, gereksinimleri görüntüleyin [AWS'de bir maliyet ve kullanım raporu oluşturma](aws-integration-set-up-configure.md#create-a-cost-and-usage-report-in-aws)

## <a name="next-steps"></a>Sonraki adımlar

- Yönetim grubuyla Azure ortamınızda önce yapılandırmadıysanız, bkz. [ilk kurulumu yönetim gruplarının](../governance/management-groups/index.md#initial-setup-of-management-groups).
