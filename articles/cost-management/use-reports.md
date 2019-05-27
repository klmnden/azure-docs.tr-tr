---
title: Azure'da Cloudyn raporlarını kullanma | Microsoft Docs
description: Bu makalede, Cloudyn portalında, etkili bir şekilde kullanmanıza yardımcı olması için dahil edilen Cloudyn raporlarını amacını açıklar.
services: cost-management
keywords: ''
author: bandersmsft
ms.author: banders
ms.date: 05/20/2019
ms.topic: conceptual
ms.service: cost-management
manager: benshy
ms.custom: seodec18
ms.openlocfilehash: f056515e87d01d0a30fec7f792fcb6e5e91c0c89
ms.sourcegitcommit: e9a46b4d22113655181a3e219d16397367e8492d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/21/2019
ms.locfileid: "65969035"
---
# <a name="reports-available-in-the-cloudyn-portal"></a>Cloudyn portalında kullanılabilir raporlar

Bu makalede, Cloudyn portalında bulunan Cloudyn raporlarını amacını açıklar. Ayrıca, raporları etkili bir şekilde nasıl kullanabileceğinizi açıklar. Raporların çoğu, sezgisel ve Tekdüzen bir görünümüne sahip. Çoğu bir raporda gerçekleştirebileceğiniz eylemleri, diğer raporları da yapabilirsiniz. Özelleştirme ve kaydedin veya raporları zamanlamak üzere nasıl dahil olmak üzere, Cloudyn raporların nasıl kullanılacağı hakkında genel bir bakış için bkz. [maliyet raporlarını anlama](understanding-cost-reports.md).

Azure Maliyet Yönetimi, Cloudyn'e benzer işlevler sunar. Azure Maliyet Yönetimi, yerel Azure maliyet yönetimi çözümüdür. Maliyet analizi yapmanıza, bütçe oluşturup yönetmenize, verileri dışarı aktarmanıza ve tasarruf önerilerini gözden geçirip gerekli eylemleri gerçekleştirmenize yardımcı olur. Daha fazla bilgi için bkz. [Azure Maliyet Yönetimi](overview-cost-mgt.md).

## <a name="report-types"></a>Rapor türleri

Cloudyn raporlarını üç tür vardır:

- Aşırı zaman raporlar. Örneğin, zaman içinde Maliyet raporu. Aşırı zaman raporlar önceden tanımlanmış bir çözümleme seçili bir aralık boyunca bir zaman serisi verilerinin Göster ve son iki ay boyunca haftalık bir çözüm göster. Gruplandırma ve filtreleme, çeşitli veri noktalarına yakınlaştırmak için kullanabilirsiniz.
  - Aşırı zaman raporlar, eğilimleri görüntülemek ve ani artış ve anomalileri algılayın yardımcı olabilir.
- Analiz raporları. Örneğin, maliyet analizi raporu. Bu raporlar tanımlayın ve verileri filtreleme ve gruplandırma izin bir dönem boyunca toplanan verileri gösterir.
  - Analiz raporları görüntüleyin ani ve anomali nedenlerini belirlemek yardımcı olabilir ve verilerinizi ayrıntılı bir kesme aşağı göstermek için.
- Tablo raporlar. Tablo olarak herhangi bir raporu görüntüleyebilirsiniz, ancak bazı raporlar yalnızca tablo olarak görüntülenir. Bu raporlar, öğe listeleri içeren ayrıntılı sağlar.
  - Öneriler misiniz tablosal raporları — önerileri için herhangi bir görselleştirmenin vardır. Ancak, öneri sonuçları görselleştirebilirsiniz. Örneğin, zaman tasarrufu.
  - Tablo listeleri eylemlerin veya daha fazla işleme için verileri dışarı aktarma için olarak yararlı raporlardır. Örneğin, bir geri ödeme raporu.

Maliyet raporlarında gösterilir ya da _gerçek_ veya _amorti edilmiş_ maliyetlerini.

Seçilen bir zaman çerçevesinde yapılan ödemeleri gerçek maliyet raporlarını görüntüleyin. (RI) ayrılmış örnek satın alma gibi gibi tüm tek seferlik ücretler gerçek maliyet raporlarında maliyet ani olarak gösterilir.

Amorti edilmiş maliyet raporlarında, tek seferlik ücretler uygulandıkları bir döneme yayılır. Örneğin, RI satın alma işlemleri için tek seferlik ücretler ayırma dönemi üzerinden yayılan ve bir depo gösterilmez. Amorti edilmiş görünümü true eğilimleri görmek ve maliyet tahminleri yapmak için tek yoludur.

Bazı durumlarda, itfa ayrı bir rapor olarak sunulur. Maliyet analizi ve amorti edilmiş maliyet analizi raporları örneklerindendir. Diğer durumlarda, itfa, maliyet ayırma ve maliyet analizi raporları gibi bir rapor ilkesidir.

Herhangi bir rapordaki düzenli teslim için zamanlayabilirsiniz. Maliyet raporları, uyarılar için yararlı oldukları için bir eşik ayarı izin verir.

## <a name="cost-analysis-vs-cost-allocation"></a>Maliyet ayırma ve maliyet analizi

_Maliyet analizi_ raporları, bulut sağlayıcıları fatura verilerini görüntüler. Raporları kullanarak, Grup ve fatura dosyasından dökümü çeşitli veri kesimlerinde ayrıntısına. Raporları bulut satıcınızın ham faturalama verileri üzerinde ayrıntılı maliyet gezinmeyi etkinleştirme.

Bazı _maliyet analizi_ raporları maliyetlerini Kaynak etiketlerine göre grup yok. Ve etiket tabanlı faturalandırma bilgileri kullanarak bir maliyet modeli oluşturarak maliyetleri ayırdıktan sonra raporları yalnızca görünür [360 maliyet](tutorial-manage-costs.md#use-custom-tags-to-allocate-costs).

_Maliyet tahsisatı_ kullanarak bir maliyet modeli oluşturduktan sonra raporlar kullanılabilir [360 maliyet](tutorial-manage-costs.md#use-custom-tags-to-allocate-costs). Cloudyn maliyet ve fatura verilerini işler ve _eşleşen_ kullanım ve etiket verilerini bulut hesaplarınız için veriler. Verileri eşleştirmek için Cloudyn kullanım verilerinize erişim gerektirir. Kimlik bilgileri eksik olan hesapları varsa, bunlar olarak etiketlenmiştir _Kategorilere ayrılmamış kaynaklar_.

## <a name="dashboards"></a>Panolar

Cloudy panoları, raporları üst düzey bir görünümünü sağlar. Pano pencere öğelerinizi yapılır ve aslında bir rapor küçük resim her pencere öğesi ise. Olduğunda, [raporları özelleştirme](understanding-cost-reports.md#save-and-schedule-reports)raporlarım için Kaydet ve panonuza. Panolar hakkında daha fazla bilgi için bkz. [panolar sayesinde önemli maliyet ölçümleri görüntülemek](dashboards.md).

## <a name="budget-information-in-reports"></a>Raporlardaki bilgilerin bütçe

El ile bir oluşturduktan sonra birçok Cloudyn raporlarını bütçe bilgileri gösterir. Bu nedenle bir bütçe oluşturana kadar raporlar bütçe bilgileri gösterilmez. Daha fazla bilgi için [bütçe yönetimi ayarları](#budget-management-settings).

## <a name="reports-and-reporting-features"></a>Raporlar ve raporlama özellikleri

Cloudyn aşağıdaki raporlar ve raporlama özelliklerini içerir.

### <a name="cost-navigator-report"></a>Gezgin rapor maliyeti

Gezgin Maliyet raporu, Pano görünümü kullanılarak fatura tüketiminiz görüntülemek için hızlı bir yoludur. Bir alt kümesini filtreler ve hemen bir Özet görünümü kuruluşun maliyetleri göstermek için temel görünümleri var. Tarihe göre maliyetleri gösterilir. Raporun ilk bir maliyetlerinizi görünüm olarak tasarlandığından kadar esnek veya olarak kapsamlı değil diğer birçok raporlar veya kendi oluşturduğunuz özel panolar.

Varsayılan olarak, ana görünümleri raporunda göster:

- Bir çalışma haftası çubuk grafik görünümü gösteren zaman içinde maliyet. Değiştirebileceğiniz **tarih aralığı** Tarih aralık çubuğu grafiği değiştirmek için.
- Bir pasta grafiğini kullanarak hizmeti tarafından masrafı.
- Bir pasta grafiğini kullanarak etiketleriyle kaynak kategori.
- Bir pasta grafiğini kullanarak maliyet varlıkları tarafından masrafı.
- Liste görünümünde tarihine göre toplam maliyeti.

### <a name="cost-analysis-report"></a>Maliyet Analizi raporu

Maliyet analizi raporunda, hesaplama ve Ücret yansıtma, ilkesini temel alarak, hesaplamasıdır. Bu, bulut tüketimi bir seçili saat için maliyet ayırma kuralların uygulandıktan sonra çerçevesi sırasında toplar. Örneğin, maliyetleri etiketlere göre hesaplar, maliyetleri etiketlenmemiş kaynakların yeniden atar ve isteğe bağlı olarak ayrılmış örnekler'ın kullanımı ayırır.

İlkeler kümesinde [360 maliyet](tutorial-manage-costs.md#use-custom-tags-to-allocate-costs) kullanılan maliyet analizi raporu ve sonuçları sonra bulut satıcınızın ham verilerden bilgi ile birleştirilir.

Bu rapor nasıl hesaplanır? Cloudyn hizmeti ayırma uygulayarak her bağlı bir hesabı bütünlüğünü korur sağlar _hesap benzeşim_. Belirli bir hizmete kullanmayan bir hesap maliyetlerin ayrılmış bu hizmetin yok benzeşimi sağlar. Hesap bu hesabında kalır ve ayırma ilkeleri tarafından hesaplanmaz maliyetleri tahakkuk. Örneğin, beş bağlantılı hesaplar olabilir. Yalnızca depolama hizmetleri üç tanesi kullanın, ardından depolama hizmetleri maliyetini yalnızca üç hesaplarında etiketleri arasında ayrılır.

Maliyet analizi raporu kullanın:

- Kuruluş geri ödeme/Ücret hesaplama
- Tüm maliyetlerinizi kategorilere ayırma
- Belirli bir zaman çerçevesi için tüm dağıtımınızı toplu bir görünümünü görüntüler.
- Maliyetleri maliyet modelinde oluşturulan ilkelerine bağlı olarak etiket kategorilere göre görüntüleyin.

Maliyet analizi raporunda kullanmak için:

1. Bir tarih aralığı seçin.
2. Etiketler, gerektiği gibi ekleyin.
3. Grupları ekleyin.
4. Daha önce oluşturduğunuz maliyet modelini seçin.

### <a name="cost-over-time-report"></a>Zaman İçinde Maliyet raporu

Maliyet süresi raporu üzerinden zaman serisi maliyet dağıtma sonuçlarını görüntüler. Eğilimleri inceleyin ve dağıtımınızda sürdürmenin algılamak sağlar. Aslında, tanımlı bir dönem boyunca dağıtılmış maliyetleri gösterir. Rapor eden maliyetler ve seçilen bir zaman çerçevesinde harcanan tek seferlik ayrılmış örnek ücretleri dahil olmak üzere, ana maliyete katkıda içerir. İlkeler kümesinde [360 maliyet](tutorial-manage-costs.md#use-custom-tags-to-allocate-costs) Bu raporda kullanılır.

Zaman içinde Maliyet raporu kullanın:

- Zaman ve hangi etkiler sonraki bir gün (veya tarih aralığını) değiştirme üzerinden değişiklikleri görebilirsiniz.
- Belirli bir örneği için zaman içinde maliyet çözümleyin.
- Anlamak oluştu neden belirli bir örneği için bir maliyet artışı.

Zaman içinde Maliyet raporu kullanmak için:

1. Bir tarih aralığı seçin.
2. Etiketler, gerektiği gibi ekleyin.
3. Grupları ekleyin.
4. Daha önce oluşturduğunuz maliyet modelini seçin.
5. Gerçek maliyet veya amorti edilmiş maliyet seçin.
6. Görünüm veri görünümü faturalama ham ayırma kuralları uygulanıp uygulanmayacağını seçin veya için maliyet görünümü yeniden hesaplanır.

### <a name="actual-cost-analysis-report"></a>Gerçek maliyet analizi raporu

Gerçek maliyet analizi raporunda herhangi bir değişiklik yapılmadıysa sağlayıcısı maliyetleri gösterir. Bu, devam eden maliyetler ve tek seferlik ücretler dahil olmak üzere ana maliyet bulunanların gösterir.

Abonelikleriniz için maliyet bilgilerini görüntülemek için raporu kullanabilirsiniz. Raporda, Azure abonelikleri olarak gösterilen **hesap adı** ve **hesap numarası**. **Bağlı hesaplar** AWS abonelikleri göster. Abonelik maliyetleri, her hesap için bir döküm başına altında görüntülemek için **grupları**, sahip olduğunuz abonelik türünü seçin.

Gerçek maliyet analizi raporu kullanın:

- Analiz ve belirtilen bir zaman çerçevesi sırasındaki harcanan ham sağlayıcısı maliyetleri izleyin.
- Bir eşiği uyarı zamanlayın.
- Varlıklar ve hesapları değiştirilmemiş maliyetleri analiz edin.

### <a name="actual-cost-over-time-report"></a>Gerçek zaman içinde Maliyet raporu

Zaman içinde gerçek maliyet raporunu üzerinde tanımlanan zaman çözüm maliyet dağıtma standart maliyet analizi raporunu ' dir. Harcama eğilimleri inceleyin ve harcama sürdürmenin algılamak olanak tanımak için zaman içinde rapor görüntüler. Bu rapor, devam eden maliyetler ve seçilen bir zaman çerçevesinde harcanan tek seferlik ayrılmış örnek ücretleri dahil olmak üzere, ana maliyete katkıda gösterir.

Zaman içinde gerçek maliyet raporu kullanın:

- Zaman içinde maliyet eğilimlerinizi görmenin.
- Sürdürmenin maliyetine bulun.
- Bulut sağlayıcıları ile ilgili tüm maliyet ile ilgili soruları bulun.

### <a name="amortized-cost-reports"></a>Amorti edilmiş maliyet raporlarında

Amorti edilmiş maliyet raporları doğrusal duruma gösterir olmayan kullanım bu dizi, hizmet ücretlerini veya tek seferlik borç maliyetleri ve bunların kullanım ömrü boyunca eşit olarak zaman içinde maliyetlerine yayılabilir. Örneğin, tek seferlik ücretler şunlar olabilir:

- Yıllık destek ücretleri
- Yıllık güvenlik bileşen ücretleri
- Ayrılmış örnekler satın alma ücreti
- Bazı Azure Market öğeleri

Tek seferlik ücretler fatura dosyasında belirlenir, hizmet tüketimi başlangıç ve bitiş tarihleri (zaman damgası) eşit değerlere sahip. Cloudyn hizmet daha sonra bunları amorti edilmiş gibi tek seferlik ücretler tanır. İsteğe bağlı kullanım maliyetleri tüketim tabanlı diğer hizmetlerle amorti edilmiş değil.

Amorti edilmiş maliyet raporlarında şunlardır:

- Amorti edilmiş maliyet analizi
- Zaman içinde amorti edilmiş maliyet

### <a name="cost-analysis-report"></a>Maliyet Analizi raporu

Maliyet analizi raporunda, bulut tüketimi ve seçilen bir zaman çerçevesinde harcama öngörü sağlar. İlkeler kümesinde [360 maliyet](tutorial-manage-costs.md#use-custom-tags-to-allocate-costs) maliyet analizi raporda kullanılır.

Cloudyn, bu rapor nasıl hesaplar?

Cloudyn sağlar ayırma uygulayarak her bağlı bir hesabı bütünlüğünü korur _hesap benzeşim_. Belirli bir hizmete kullanmayan bir hesabı da maliyetlerin ayrılmış bu hizmetin yok benzeşimi sağlar. Hesap bu hesabında kalır ve ayırma ilkeleri tarafından hesaplanan olmayan maliyet tahakkuk. Örneğin, beş bağlantılı hesaplar olabilir. Yalnızca depolama hizmetleri üç tanesi kullanın, ardından depolama hizmetleri maliyetini yalnızca üç hesaplarında etiketleri arasında ayrılır.

Maliyet analizi raporu kullanın:

- Belirli bir zaman çerçevesi için tüm dağıtımınızı toplu bir görünümünü görüntüler.
- Maliyetleri maliyet modelinde oluşturulan ilkelerine bağlı olarak etiket kategorilere göre görüntüleyin.

### <a name="cost-over-time-report"></a>Zaman İçinde Maliyet raporu

Zaman içinde Maliyet raporu, dağıtımınızdaki eğilimleri ve bildirim sürdürmenin nokta için zaman içinde harcama görüntüler. Aslında, tanımlı bir dönem boyunca dağıtılmış maliyetleri gösterir. Rapor eden maliyetler ve seçilen bir zaman çerçevesinde harcanan tek seferlik ayrılmış örnek ücretleri dahil olmak üzere, ana maliyete katkıda içerir. İlkeler kümesinde [360 maliyet](tutorial-manage-costs.md#use-custom-tags-to-allocate-costs) Bu raporda kullanılır.

Zaman içinde Maliyet raporu kullanın:

- Zaman ve hangi etkiler sonraki bir gün (veya tarih aralığını) değiştirme üzerinden değişiklikleri görebilirsiniz.
- Belirli bir örneği için zaman içinde maliyet çözümleyin.
- Anlamak oluştu neden belirli bir örneği için bir maliyet artışı.

### <a name="custom-charges-report"></a>Özel ücretleri rapor

Enterprise ve CSP kullanıcılar kendilerine eklenen Hizmetleri kendi bulut kaynak tüketimi ek olarak, dış veya iç müşterilere sunuyor genellikle bulun. Ek hizmetler veya Müşteri faturalama veya geri ödeme raporlarını özel satır öğeleri olarak eklenir indirimleri özel ücretleri tanımlarsınız.

Normalde bir fatura gösterilmeyen Hizmetleri özel hizmet ücretlerini yansıtır. Oluşturduğunuz özel ücretleri, daha sonra maliyet raporlarında gösterilir.

*Özel ücretleri bulunmayan özel fiyatlandırma*. Özel ücretleri listesinde, ücretlendirme, farklı oranları göstermez. Örneğin, AWS faturalandırma ücretleri, yalnızca ücretlendirilir olarak gösterilir.

Özel bir ücret oluşturmak için:

1. İçinde **özel ücretleri**, tıklayın **yeni Ekle**. _Ekleyebilir, yeni özel ücret_ iletişim kutusu görüntülenir.
2. İçinde **sağlayıcı adı**, sağlayıcı adını girin.
3. İçinde **hizmet adı**, hizmet türünü girin.
4. İçinde **açıklama**, özel ücreti için bir açıklama ekleyin.
5. İçinde **türü**, select girin **yüzdesi** ve Hizmetleri açılır menüden maliyet raporlarında özel ücretleri olarak eklemek için hizmetleri seçin.
6. İçinde **ödeme**ücretsiz olarak bir kerelik ücret veya yinelenen ücret ise seçin. Yinelenen bir ücret ücreti ise amorti edilmiş ve kaç aya ilişkin seçmek için ücret istiyorsanız Amortized belirleyin.
7. İçinde **tarihleri**tek seferlik bir ücret seçiliyse, **geçerlilik tarihi**, ücretsiz Ücretli bir tarih girin. Yinelenen ücret seçili ise, başlangıç tarihi ve ücretsiz olarak bitiş tarihi dahil olmak üzere tarih aralığı girin.
8. İçinde **varlıklar ağacı**, ücretsiz olarak uygulayın ve ardından istediğiniz varlıkları seçin **üzerinde**.

_Bir varlığa ücretleri atandığında, kullanıcılar bunları değiştiremez. Bir yönetici tarafından bir üst varlığa eklenen ücretleri salt okunurdur._

Özel harcamalarını görüntülemek için:

Özel ücretleri maliyet raporlarında gösterilir. Örneğin, gerçek maliyet analizi raporu altında açılacağını **genişletilmiş filtreler**seçin **tek başına**. Ardından görüntülemek için filtre **özel ücretleri**.

### <a name="cost-allocation-360"></a>360 maliyet ayırma

360 maliyet, maliyetleri tüketilen bulut kaynaklarına atamak için özel bir maliyet ayırma modelleri oluşturmak için kullanın. Birçok raporlar ile özel bir maliyet modelleri oluşturduğunuz özel maliyet modelleri bilgileri gösterir. Ayrıca, maliyet ayırma ile özel bir maliyet modeli oluşturduktan sonra bazı raporlar yalnızca bilgileri gösterir.

Özel bir maliyet modelleri oluşturma hakkında daha fazla bilgi için bkz. [Öğreticisi: Cloudyn kullanarak maliyetleri yönetme](tutorial-manage-costs.md).

### <a name="cost-vs-budget-over-time-report"></a>Maliyet vs. Zaman içinde Bütçe Raporu

Maliyet vs. Zaman içinde bütçe rapor bütçenizi karşı ana maliyete katkıda karşılaştırmanızı sağlar. Zaman içinde (üzerinden/altında/nominal) bütçe tüketiminiz görüntüleyebilmesi için atanan bütçe raporda görünür. Raporun üst kısmında alanları Göster/Gizle kullanarak görünüm maliyeti, bütçe, birikmiş maliyeti ve toplam bütçe seçebilirsiniz.

### <a name="current-month-projected-cost-report"></a>Geçerli ay öngörülen maliyet raporu

Geçerli ay öngörülen maliyet raporu, geçerli ayın başından bu yana maliyet özeti hakkında Öngörüler sağlar. Bu rapor önceki ay, ayın başından itibaren maliyetlerinizi görüntüler ve toplamı için geçerli ay öngörülen maliyet. Geçerli ay öngörülen maliyet güncel aylık maliyet ve son 30 gün içinde izlenen maliyeti temel bir projeksiyon toplamı olarak hesaplanır.

Geçerli ay öngörülen maliyet raporu kullanın:

- Proje hizmeti tarafından aylık maliyetler
- Proje aylık maliyetlerini hesaba göre

### <a name="annual-projected-cost-report"></a>Yıllık öngörülen maliyet raporu

Yıllık öngörülen maliyet raporu, yıllık öngörülen maliyet önceki harcama eğilimlerine göre görüntülemenizi sağlar. Bu, sonraki 12 ay genel tahmini maliyetleri gösterir. Tahminleri, bir sonraki 12 ay boyunca kullanım son 30 gün ile ilişkili maliyetleri dayalı ortaya çıkabilecek eğilim işlevi kullanılarak yapılır.

### <a name="budget-management-settings"></a>Bütçe yönetimi ayarları

Bütçe yönetimi, mali yıl için bütçe ayarlamanıza olanak sağlar.

Bir varlığa bir bütçe eklemek için:

1. Bütçe Yönetimi sayfasında altında **varlıkları**, bütçe oluşturmak istediğiniz varlığı seçin.
2. Bütçe yılda bütçe oluşturmak istediğiniz yıl seçin.
3. Her ay içinde bütçenizi ayarlamak tıklayın ve sonra **Kaydet**.

Yıllık bütçenin bir dosyayı içe aktarmak için:

1. Altında **eylemleri**seçin **dışarı** bütçe, temel olarak kullanmak için boş bir CSV şablonu indirilemedi.
2. Bütçe girişlerinizi içeren CSV dosyası doldurun ve yerel olarak kaydedin.
3. Altında **eylemleri**seçin **alma**.
4. Kaydettiğiniz dosyayı seçin ve ardından **Tamam**.

Tamamlanan bütçenizi altında bir CSV dosyası olarak dışarı aktarmak için **eylemleri**seçin **dışarı** dosyasını indirmek için.

Tamamlandığında, bütçenizi maliyet vs'de ve maliyet analizi raporlarında gösterilir. Zaman içinde bütçe rapor. Ayrıca, bütçe eşiklere dayanarak raporları zamanlayabilirsiniz.

### <a name="azure-resource-explorer-report"></a>Azure kaynak Gezgini raporu

Azure kaynak Gezgini rapor Cloudyn'de kullanılabilir tüm Azure kaynaklarını toplu listesini gösterir. Rapor etkili bir şekilde kullanmak için ölçümleri etkin şekilde Azure hesaplarınızı genişletilmiş. Genişletilmiş ölçümler, Azure Vm'lerinizi Cloudyn erişim sağlar. Daha fazla bilgi için [ölçümleri Azure sanal makineleri için genişletilmiş Ekle](azure-vm-extended-metrics.md).

### <a name="azure-resources-over-time-report"></a>Azure kaynakları zaman içinde rapor

Azure kaynakları zaman içinde rapor, belirli bir süre içinde çalışan tüm kaynakları dökümünü gösterir. Rapor etkili bir şekilde kullanmak için ölçümleri etkin şekilde Azure hesaplarınızı genişletilmiş. Genişletilmiş ölçümler, Azure Vm'lerinizi Cloudyn erişim sağlar. Daha fazla bilgi için [ölçümleri Azure sanal makineleri için genişletilmiş Ekle](azure-vm-extended-metrics.md).

### <a name="instance-explorer-report"></a>Örnek Gezgini raporu

Örnek Gezgini rapor, sanal makinelerinizin varlıklar için çeşitli ölçümleri görüntülemek için kullanılır. Aşağıdaki gibi bilgileri görüntülemek için belirli örnekler ayrıntıya yapabilecekleriniz:
- Aralıkları çalışan örneği
- Seçili dönem içinde yaşam döngüsü
- CPU kullanımı
- Ağ giriş
- Çıkış trafiği
- Etkin diskleri

Örnek Gezgini rapor tanımlı tarih aralığı içinde çalışan tüm aralıkları toplar ve buna uygun olarak veri toplar. Her çalışan aralıkları tarih aralığı içinde görüntülemek için örneği genişletin. Maliyet her örneğinin Seçili aralık temelinde AWS ve Azure listesi fiyatları tarihini hesaplanır. Hiçbir indirimi uygulanmaz. Ek alanları Göster/Gizle alanlarını kullanarak raporu ekleyebilirsiniz.

Örnek Gezgini rapora kullanın:

- Makine başına tahmini maliyet hesaplayın.
- Bir zaman aralığında etkin tüm makinelerin toplanmış çalışan saat dahil olmak üzere, tam bir liste oluşturur.
- Bulut hizmeti sağlayıcısı veya hesabı tarafından bir liste oluşturur.
- Makineleri görüntüle oluşturulduğunda veya bir zaman aralığı sonlandırıldı.
- Tüm şu anda durdu makinelerin görüntüleyin.
- Her makine etiketlerini görüntüleyin.

### <a name="instances-over-time-report"></a>Zaman içinde örnekleri raporu

Zamana göre örnekler raporunu kullanarak, seçili zaman aralığı sırasında her etkin olan makineler sayısı görebilirsiniz. Haftalık veya aylık tanımlanmış bir çözümlemeyi ise, sonuçlar sayısı makineler söz konusu ay sırasında belirli bir günde etkin olur. Raporda görünmesini istediğiniz filtreleri seçmek için bir tarih aralığı seçin.

### <a name="instance-utilization-over-time-report"></a>Örnek zaman içinde kullanımı raporu

Bu rapor, tüm örnekleri için zaman içinde herhangi bir CPU veya bellek kullanımı dökümünü gösterir.

### <a name="compute-power-cost-over-time-report"></a>İşlem zaman içinde Power Maliyet raporu

Zaman içinde işlem gücü rapor, belirtilen tarih aralığında dökümünü işlem gücü sağlar. Diğer raporları makine ya da çalışma zamanı saat çalışan sayısı gösterilmektedir, ancak bu rapor, çekirdek saatleri, işlem birimi saatlerini ve GB RAM saat gösterir.

Rapora kullanın:

- Belirtilen tarih aralığı içinde bilgi işlem gücü denetleyin.
- Zaman temelinde maliyet ayırma modeller üzerinde işlem görünümü.

Bu rapor bağlantılıdır, [360 maliyet](tutorial-manage-costs.md#use-custom-tags-to-allocate-costs) sonuçları gösterilecek şekilde ilkeleri, seçili maliyet ilkenizi tanımlı etiketleme ve ilkeleri üzerinde temel. Oluşturulan bir ilke yoksa, ardından sonuçların gösterilmediği.

### <a name="compute-power-average-cost-over-time-report"></a>İşlem gücü ortalama zaman içinde Maliyet raporu

İşlem gücü ortalama zaman içinde Maliyet raporu, en fazla çalışan her makine maliyeti görüntülemek için kullanın. Rapor örneği, çekirdek saat, işlem birimi saati ve GB RAM saat başına maliyet, ortalama gösterir. Bu rapor, dağıtımınızın verimliliğini öngörü sağlar.

Bu rapor bağlantılıdır, [360 maliyet](tutorial-manage-costs.md#use-custom-tags-to-allocate-costs) sonuçları görüntülenecek şekilde ilke tabanlı tanımlanmış etiketleme ve ilkeleri üzerinde seçili maliyet ilkenizi. Oluşturulan bir ilke yoksa, ardından sonuçların gösterilmediği.

### <a name="s3-cost-over-time-report"></a>Zaman içinde S3 Maliyet raporu

Zaman içinde S3 Maliyet raporu, belirtilen bir zaman çerçevesi için zaman içinde Amazon basit depolama hizmeti (S3 için) maliyetlerini demet başına dökümünü sağlar. Rapor ana maliyet sürücülerdir demetler bulmanıza yardımcı olur ve, S3 eğilimleri gösterir, kullanımı ve harcamayı.

### <a name="s3-distribution-of-cost-report"></a>S3 dağıtım maliyeti raporu

S3 maliyetiniz son bir ay için demet ve depolama sınıfı tarafından analiz etmek için raporu kullanın. Görünürlük eşiği ayarlamak için pasta grafiği görünümü kullanabilirsiniz. Ya da alt toplamları görmek için Tablo görünümü kullanabilirsiniz.

### <a name="s3-bucket-properties-report"></a>S3 Demetini özellikleri raporu

S3 demetini özelliklerini görüntülemek için raporu kullanın. Görünürlük eşiği ayarlamak için pasta grafiği görünümü kullanabilirsiniz. Ya da alt toplamları görmek için Tablo görünümü kullanabilirsiniz.

### <a name="rds-instances-over-time-report"></a>RDS örnekleri zaman içinde rapor

Belirtilen dönemde çalıştırılan tüm Amazon ilişkisel veritabanı hizmeti (RDS) örneklerinin dökümünü görüntülemek için raporu kullanın.

### <a name="rds-active-instances-report"></a>RDS etkin örnekleri raporu

RDS etkin örnekler çözümlemek için bu raporu kullanın. Raporda ek bilgileri görüntülemek için çizgi öğesini genişletin.

### <a name="azure-reserved-instances-report"></a>Azure ayrılmış örnekleri raporu

Azure ayrılmış örnekleri raporun tüm Azure ayrılmış örnekler tek bir görünümünü sağlar. Bu rapor her satın alma, kendi satır öğesi olarak görüntüler. Rapor ayrıca, satın alma ve örnek türüne, vb. kalan gün sayısı türü satın hesabı gibi ilgili satın alma ayrıntılarını gösterir. Göster veya gizle alanları Göster/Gizle kullanarak rapor verileri.

Azure ayrılmış örnekleri raporu görüntülemek için kullanın:

- Tüm rezervasyon satın alma tarihe göre listesi.
- RI süresi dolana kadar kalan süre.
- Tek seferlik ücretler.
- Satın alınan RI'ları, hesap ve ne zaman.

### <a name="aws-reserved-instances-report"></a>AWS ayrılmış örnekleri raporu

AWS ayrılmış örnekleri bu rapor, tek bir görünümde, tüm AWS ayrılmış örnekleri sağlar. Bu rapor her satın alma, kendi satır öğesi ve ilgili satın alma, satın alma ve örnek türüne, vb. kalan gün sayısı türü satın hesabı gibi ayrıntılarını görüntüler olduğu. Göster veya gizle alanları Göster/Gizle kullanarak rapor verileri.

AWS ayrılmış örnekleri raporu görüntülemek için kullanın:

- Tüm rezervasyon satın alma tarihe göre listesi.
- RI süresi dolana kadar kalan süre.
- Tek seferlik ücretler.
- Orijinal satın alma kodu (Rezervasyon kimliği).
- RI satın hesabı ve ne zaman.

### <a name="ec2-ri-buying-recommendations-report"></a>EC2 RI satın alma önerileri raporu

Bulut kaynak tüketimi temelini burada kaynaklara yalnızca kullanıldığı zaman ücret talep üzerine modelidir. Ön taahhüt vardır — kullandığınızda yalnızca, kullandıklarınız için ödeme yaparsınız.

AWS fiyatlandırma modeli, elastik bulut bilgi işlem (EC2) Hizmetleri için bir alternatif sunar — ayrılmış örnek (RI). RI süresince gerektiğinde bu fiyatlandırma modeli kullanıcılar kapasitesini garanti eder. RI önemli fiyat indirimleri, isteğe bağlı fiyat üzerinden sunar. Buna karşılık, kullanıcıları, sanal bir örneğinin kullanılması için bir ön taahhüt yapar. Taahhüt belirli ailesi, boyut, kullanılabilirlik alanı (AZ) ve işletim sistemi için taahhüt (bir veya üç yıl) döneme bağlı. RI AWS hizmetlerini kullanarak müşteri taahhüt kazanmak için geleceğe yönelik kapasite, da verimli bir şekilde planlamak sağlar.

RI'ları, ama tüm ön üç ödeme seçenekleri:

- En yüksek indirimi sunan gün 0 ise, toplu Topla
- Hiçbir ön maliyet -, RI maliyetini aylık RI süresince taksitli, en düşük indirimi sunan
- Ön maliyet, hangi ¼ inç - fiyatının ½ Önden, ücretli kısmi ve düşük olan indirim oranı ile aylık taksitlere geri kalanı ancak, tüm ön hızını kapatın

Cloudyn, son 30 güne ait her makinenin çalışma süresi değerlendirir. Cloudyn, makine ile bir RI geçerli çalışma süresi düzeyinde çalıştırmak için daha uygun maliyetli olduğunda RI satın alma önerir.

Rapor yıldan çoğu tasarruf öneriler gerekçesi gösterir. Öneriler üzerine örnekleri RI ile değiştirme önerin. Raporu doğrudan RI satın alabilirsiniz.

Her sekme, tam bir rapor olarak açılır. Sekmelerdeki önemli bölümleri şunlardır:

- **EC2 RI satın alma etkisi** -Bu bölümde, isteğe bağlı vs ayrılmış örnekleri arasındaki fark benzetimini sağlar. Tıklayın **yakınlaştırmak**, öneri için zaten tanımlanmış filtreleri içeren tam EC2 RI satın alma etkisi raporu görmek için. Bu rapor, satın alma etkisi, tüm olası RI satın alma işlemleri gösterir. EC2 ayrılmış örnekler satın kaydederken olası görmek için beklenen ortalama çalışma süresi ayarlayabilirsiniz.

- **Analiz kaydetme** -Bu bölümde elde edilen olası tasarruf ve tasarruf Cloudyn önerileri takip ederken actualized ay sağlar. Gerçek tasarruf miktarı ve yüzde kaydedilen kırmızıyla vurgulanır.

- **EC2 RI türü karşılaştırma** -Bu bölümde, Cloudyn'ın önerilen dağıtım, tüm ilgili seçenekleri dahil olmak üzere ROI konular vurgulanıyor vurgular. Bu rapor sonuçlarında, makine % 100 açık kalma süresi çalıştırdığı varsayılmaktadır. Tıklayın **Yakınlaştır** ayrıntılı bir rapor açın.

- **Zamana göre örnekler** -Bu bölümde bir öneri, OnDemand, ayrılmış örnekleri ve nokta ile ilişkilendirilmiş tüm örnekler dökümünü gösterir. Tıklayın **Yakınlaştır** ayrıntılı bir rapor açın.
- **Breakeven noktaları** -Bu bölümde, tüm olası önerilen dağıtımları ve yatırım Getirisi ve yatırım Getirisi oluştuğunda ayın bir tablo görüntülenir. Tıklayın **Yakınlaştır** ayrıntılı bir rapor açın.

### <a name="ec2-reservations-over-time-report"></a>EC2 ayırmaları zaman içinde rapor

EC2 ayırmaları zaman içinde rapor, satın alınan EC2 RI'larınızı kullanım durumunu izler. Saat, gün veya hafta çözümleme raporunun ayarlayabilirsiniz.

Rapora kullanın:

- Satın alınan kullanılan ve kullanılmayan ayırmaları görüntülenir.
- Çözümü saat / saat RI kullanımını görmek için ayrıntılara girin.

### <a name="savings-over-time-report"></a>Zaman içinde tasarruf raporu

Ayrılmış örnekler, hem de spot örnekleri kullanarak elde edilen tasarrufu görüntülemek için zaman içinde tasarruf raporu kullanın. Rapor RI gerçekleştirilen elde edilen zaman içinde elde edilen ROI gösterir.

Grup tarafından sonuçları elde edilen tasarrufu RI'ları görüntülemek için **fiyat Modeli'ne** seçip **ayırma**. Belirli bir hesabı ya da örnek türü tarafından elde RI tasarrufu görüntülemek için hesap veya örnek türüne ilgili gruplandırma ve filtreleme ekleyin.

Nokta örneği kullanımdan tasarruf görmek için filtre **fiyat Modeli'ne** için **nokta**. Bu raporun varsayılan filtresi RI ve Spot örnekleri ' dir.

### <a name="rds-ri-buying-recommendations-report"></a>RDS RI satın alma önerileri raporu

RDS RI satın alma önerileri raporu yerine isteğe bağlı örnekleri RDS RI'ları kullanmak ne zaman önerir.

Her sekme, tam bir rapor olarak açılır. Sekmelerdeki önemli bölümleri şunlardır:

- **RDS RI satın alma etkisi** -Bu bölümde benzetimini arasındaki fark, isteğe bağlı olarak vs ayrılmış örnekleri sağlar. Tıklayın **yakınlaştırmak** , öneri için zaten tanımlanmış filtreleri içeren tam RDS RI satın alma etkisi raporu görmek için. Bu rapor, tüm olası RI satın alma işlemleri, satın alma etkisi görmenizi sağlar.  Beklenen ortalama çalışma süresi ayarlama ve kaydetme RI satın alarak olası bakın.
- **Analiz kaydetme** – Bu bölüm, olası tasarruf elde edilen ve tasarruf Cloudyn önerileri takip ederken actualized ay sağlar. Gerçek tasarruf miktarı ve yüzde kaydedilen kırmızıyla vurgulanır.

- **RDS RI türü karşılaştırma** -Bu bölümde tüm ilgili seçenekleri dahil olmak üzere önerilen dağıtım ROI konular vurgulanıyor vurgular. Bu rapor sonuçlarında, makine % 100 açık kalma süresi çalıştırdığı varsayılmaktadır. Tıklayın **Yakınlaştır** seçilen makine için ayrıntılı bir rapor açın.
- **Zamana göre örnekler** – Bu bölüm öneri, OnDemand, ayrılmış örnekleri ve nokta ile ilişkili tüm örnekleri dökümünü görüntüler. Tıklayın **Yakınlaştır** ayrıntılı bir rapor açın.

- **Breakeven noktaları** – Bu bölümde, tüm olası önerilen dağıtımları ve yatırım Getirisi ve yatırım Getirisi oluştuğunda ayın bir tablo görüntülenir. Tıklayın **Yakınlaştır** ayrıntılı bir rapor açın.

### <a name="rds-reservations-over-time-report"></a>RDS ayırmaları zaman içinde rapor

RDS ayırma zaman içinde rapor, belirtilen süre, her iki kullanılan ve kullanılmayan ayırmaları dökümünü görüntülemek için kullanın.

### <a name="reserved-instance-purchase-impact-report"></a>Ayrılmış örnek satın alma etkisi raporu

EC2 RI satın alma etkisi raporu zamanla ayrılmış örnek maliyeti ile isteğe bağlı maliyetin benzetimini sağlar. Daha iyi satın alma kararları vermenize yardımcı olabilir. Ortalama çalışma zamanı, terim, platform ve başkalarının RI satın alma işlemleri göz önüne aldığınızda bilgiye dayalı kararlar gibi filtrelerini ayarlayın.

### <a name="cost-effective-sizing-recommendations-report"></a>Uygun maliyetli boyutlandırma önerileri raporu

Uygun maliyetli boyutlandırma önerileri raporu, AWS ve Azure için sonuçlar sağlar. AWS kullanıcıları için RI satın alma işlemleri dikkate alınır ve sonuçları RI'ın çalışan makineler içermez. Bu rapor, downsize için aday niteliği taşıyan az kullanılan örneklerinin bir listesini sağlar. Öneriler, son 30 güne ait kullanım ve performans verilerini temel alır. Her bir öneri downsize için aday downsize için gerekçe göstermesi ve tam ayrıntılarını görüntülemek için bir bağlantı listesi ve performans ölçümlerinden birinde bir örneği var. Ve ne zaman ilgili öneriler için yeni nesil örnek türlerini değiştirme.

Örneği bu rapordan downsize için önerilen kimlikleri listesi indirilemiyor. Örnek kimlikleri indirmek için tüm boyutlandırma önerileri raporu kullanın.

Aşağıdaki örnek downsizing göz önünde bulundurun:

Altı m3.xlarge çalışan örneklerini var. Cloudyn analiz bunları beş düşük CPU kullanımı olduğunu gösterir. Bunları downsizing göz önünde bulundurun.

Maliyet etkileri maliyet etkisi hesaplanır. Bu örnekte, satır öğesi genişleterek, geçerli fiyat (Linux/Unix) m3.xlarge örneği için saat ve bir m3.large (Linux/Unix) örneği maliyetleri başına 0.266 0.133 saatlik maliyetleri görebilirsiniz. Bu nedenle, 11,651 kullanımı % 100 çalışan beş m3.xlarge örnekler için yıllık maliyeti. 5,825 kullanımı % 100 çalışan beş m3.large örnekler için yıllık maliyetidir. Olası tasarruf $5,825 ' dir.

Uygun maliyetli boyutlandırma Gerekçeleri görüntülemek için + satır öğesi genişletin. İçinde **ayrıntıları**:

- **Öneri gerekçe** bölümü geçerli dağıtımı ve önerilen downsize için örnek sayısını görüntüler.
- **Maliyet etkisi** bölümü, olası tasarruf belirlemek için kullanılan hesaplama görüntüler.
- **Olası yıllık tasarrufları** bölüm Cloudyn'ın öneri downsizing olduğunda olası yıllık tasarrufları görüntüler.

### <a name="all-sizing-recommendations-report"></a>Tüm boyutlandırma önerileri raporu

Bu rapor, downsize için aday niteliği taşıyan az kullanılan örneklerinin bir listesini sağlar. Öneriler, son 30 güne ait kullanım ve performans verilerini temel alır. Her bir öneri tüm ayrıntılar ve performans ölçümlerinden birinde örneği görüntüleyebilirsiniz.

Bu rapor, AWS ayrılmış örnekler satın aldıysanız RI'ları çalışan örnekleri dahil olmak üzere tüm çalışan örnekler için sonuçları içerir.

Tüm boyutlandırma önerileri raporu kullanın:

- Downsize için aday niteliği taşıyan tüm örneklerinizin bir listesini görürsünüz.
- Örnek adları ve kimlikleri içeren bir rapor listesini dışarı aktarın.

Belirli bir örneği için öneri ayrıntılarını görüntülemek için tıklayın **+** ayrıntıları genişletin. Öneri ayrıntıları bölümü, öneri genel bir bakış sağlar.

**Etiketleri** bölümü, seçilen örnek için etiket anahtarları ve değerleri listesi sağlar. Etiketler bölümü filtrelemek için sol bölmede kullanın.

**CPU kullanımı** bölüm güne göre CPU kullanımı örneği için geçen ayki sağlar.

Detaya gitme ve örnek CPU üzerinden zaman örnekleri dökümünü görmek için raporu açmak için grafiğe tıklayın.

- Kullanım **alanları Göster/Gizle** alanları eklemek veya kaldırmak için: Zaman damgası, ortalama CPU, en düşük CPU, en fazla CPU.
- Kullanım **tarih aralığı** bir tarih veya tarih aralığı girin ve belirli bir InstanceId detayına gidin.
- Kullanım **genişletilmiş filtreler** Tümünü Göster ya da belirli bir örnek kimliği
- Tıklayın **yakınlaştırmak** CPU kullanımı raporu açmak için

Örneği 30 gün boyunca izlenen edilmemiş varsa, eksik veri gösterilir.

**Bellek kullanımı (GB)** bölüm bellek hakkında bilgi sağlar. AWS kullanıcıları için bellek ölçümlerini otomatik olarak bulunmaz ve AWS aracılığıyla örnek başına eklenmesi gerekir. AWS EC2 örneği için bellek ölçümleri etkinleştirmek üzere uygular.

**Bellek kullanımı (%)** bölümünde kullanılan bellek yüzdesini görüntüler.

**Giriş trafiği** bölümü, seçilen örnek için ağ trafiği, ortalama ve maksimum, zaman içinde bir anlık görüntüsünü görüntüler. Bu kez tarih ve en çok trafiği görmek için satırları üzerine gelin. Tıklayın **Yakınlaştır** ağ giriş trafiği raporu açın.

**Çıkış trafiği** bölümü, seçilen örnek için çıkış trafiği anlık görüntüsünü görüntüler. Bu kez tarih ve en çok trafiği görmek için satırları üzerine gelin. Tıklayın **Yakınlaştır** çıkış trafiği raporu açın.

### <a name="instance-metrics-explorer-report"></a>Ölçüm Gezgini rapor örneği

Ölçüm Gezgini örneği rapor örnek başına Bulutlar arası performans ölçümleri gösterir. CPU, bellek ve ağ ölçüm eşikleri bağlı olarak üzerinde veya altında kullanılan örnekleri görüntülemek için raporu kullanın.

Örnek başına Bulutlar arası performansını görüntülemek için:

1. İçinde **tarih aralığı**, performans görüntülemek istediğiniz tarih aralığı seçin.
2. İçinde **etiketleri**, görüntülemek istediğiniz herhangi bir etiket seçin.
3. İçinde **filtreleri**, raporda görüntülemek istediğiniz filtreleri seçin.
4. İçinde **genişletilmiş filtreler**, rapor eşikleri ayarlayın:
    - Ortalama CPU
    - En fazla CPU
    - Ortalama bellek
    - En fazla bellek
5. İçinde **genişletilmiş filtreler**, tıklayın **Göster** ve örnekleri görüntülemek için türünü seçin.

Zaman içinde belirli bir örneğine ait ölçümleri görüntülemek için:

- Rapor için örnek ölçüm Gezgini ve tıklayın **+** ayrıntılarını görüntülemek için.

### <a name="rds-sizing-recommendations-report"></a>RDS boyutlandırma önerileri raporu

RDS boyutlandırma önerileri raporu RDS boyutlandırma bulut kullanımınızı iyileştirmek için öneriler sağlar. Bu, downsize için aday niteliği taşıyan az kullanılan örneklerinin bir listesini sağlar. Cloudyn, öneriler, son 30 gün içinde kullanım ve performans verileri üzerinde temel alır. Hesap adı, bölgeye, örnek türüne ve durum göre öneriler filtreleyebilirsiniz.

### <a name="sizing-threshold-manager-report"></a>Boyutlandırma eşiği Manager raporu

Cloudyn'ın yerleşik boyutlandırma önerileri, doğru boyutlandırma önerileri sağlamak için bir karmaşık algoritma kullanılarak hesaplanır. Downsizing önerileri için eşikler ayarlayabilir.

Eşik boyutlandırma önerileri el ile ayarlamak için:

1. İstediğiniz gibi boyutlandırma eşiği Yöneticisi'nde, aşağıdaki eşikleri ayarlayın:
    - Ortalama CPU %
    - Maksimum CPU %
    - Ortalama bellek %
    - Maksimum bellek %
3. Tıklayın **Uygula** değişiklikleri kaydedin.
4. Değişiklikler, tüm önerilerinizi hemen uygulanır.

Varsayılan eşikler geri yüklemek için:

- Boyutlandırma eşiği Yöneticisi'nde **Varsayılanları Geri Yükle**.

### <a name="compute-instance-types-report"></a>Örnek türleri rapor işlem

Örnek türleri raporu kullanın:

- Hizmet, aile, API adı ve ad örneği türlerini görüntüleyin.
- CPU, ECU, RAM gibi ayrıntılarını görüntülemek ve ağ.

Kullanabileceğiniz **arama** belirli satır öğeleri bulmak için.

## <a name="next-steps"></a>Sonraki adımlar

- Özelleştirme ya da kaydedin ve raporları zamanlama, bkz'dahil olmak üzere raporları kullanma hakkında bilgi edinin [maliyet raporlarını anlama](understanding-cost-reports.md).
- Cloudyn'de dahil panolar ve kendi özel panolar oluşturabilir, bkz hakkında bilgi edinin [panolar sayesinde önemli maliyet ölçümleri görüntülemek](dashboards.md).
