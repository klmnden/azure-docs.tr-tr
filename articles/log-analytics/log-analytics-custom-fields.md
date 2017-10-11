---
title: "Günlük analizi içinde özel alanlar | Microsoft Docs"
description: "Günlük analizi özel alanlar özelliği, toplanan kaydı Özellikler ekleme OMS verilerden aranabilir alanlarınızı oluşturmanıza olanak sağlar.  Bu makalede bir özel alan oluşturma işlemini açıklar ve örnek olay ile ayrıntılı bilgi sağlar."
services: log-analytics
documentationcenter: 
author: bwren
manager: jwhit
editor: tysonn
ms.assetid: 31572b51-6b57-4945-8208-ecfc3b5304fc
ms.service: log-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/18/2016
ms.author: bwren
ms.openlocfilehash: 9e02094f155eaade9bc5fb49c4fbb798e546e989
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="custom-fields-in-log-analytics"></a>Günlük analizi içinde özel alanlar
**Özel alanlar** günlük analizi özelliği OMS deposunda mevcut kayıtları kendi aranabilir alanlar ekleyerek genişletebilir olanak tanır.  Özel alanlar, aynı kayıt diğer özellikleri ayıklanan verilerinden otomatik olarak doldurulur.

![Özel alanlarına genel bakış](media/log-analytics-custom-fields/overview.png)

Örneğin, aşağıdaki örnek kaydı olay açıklamasında kaçınma yararlı veri yok.  Bu verileri ayrı özelliklerini ayıklama sıralama ve filtreleme gibi eylemlere için kullanılabilir yapar.

![Günlük Ara düğmesi](media/log-analytics-custom-fields/sample-extract.png)

> [!NOTE]
> Önizlemede, çalışma alanınızdaki 100 özel alanlar sınırlıdır.  Bu özellik genel kullanılabilirlik ulaştığında bu sınırı genişletilir.
> 
> 

## <a name="creating-a-custom-field"></a>Özel bir alan oluşturma
Özel bir alan oluşturduğunuzda, günlük analizi değerini doldurmak için kullanılacak veri anlamanız gerekir.  Bu verileri hızlı bir şekilde tanımlamak için Microsoft Research FlashExtract adlı bir teknolojisini kullanır.  Açık yönergeler sağlamak gerektirmek yerine, sağladığınız örneklerinden ayıklamak istediğiniz veriler hakkında günlük analizi öğrenir.

Aşağıdaki bölümler, özel bir alan oluşturmak için yordamı sağlar.  Bu makalenin alt kısmında örnek ayıklama bir kılavuz vardır.

> [!NOTE]
> Yalnızca özel alan oluşturulduktan sonra toplanan kayıtlarında görünür OMS veri deposuna belirtilen ölçütlerle eşleşen kayıtları eklendikçe özel alan doldurulur.  Özel alan oluşturulduğunda, veri deposunda zaten olan kayıtlara eklenmedi.
> 
> 

### <a name="step-1--identify-records-that-will-have-the-custom-field"></a>1. adım – özel alan olacaktır kayıtları tanımlayın
İlk adım, özel alan alacak kayıtların belirlemektir.  İle başlayan bir [standart günlük arama](log-analytics-log-searches.md) ve günlük analizi bilgi edineceksiniz model olarak davranmak üzere bir kayıt seçin.  Özel bir alana veri ayıklamak için kalacaklarını belirttiğinizde **alan ayıklama Sihirbazı** burada doğrulamak ve ölçütü iyileştirin açılır.

1. Git **günlük arama** ve bir [kayıtları almak için sorgu](log-analytics-log-searches.md) özel alan olacaktır.
2. Günlük analizi özel alan doldurmak için veri ayıklamak için bir model olarak görev yapması için kullanacağı bir kayıt seçin.  Bu kaydından ayıklamak istediğiniz verileri tanımlayacak ve günlük analizi özel alan tüm benzer kayıtları için doldurmak için mantığı belirlemek için bu bilgileri kullanır.
3. Düğmesini seçin ve kayıt herhangi bir metin özelliği solundaki **ayıklamak alanlardan**.
4. **Alan ayıklama Sihirbazı açıldığında**, ve seçtiğiniz kayıt görüntülenen **ana örnek** sütun.  Özel alan, seçili özellikler aynı değerleri kayıtları için tanımlanır.  
5. Seçimi tam istediğinizi ise, ölçüt daraltmak için ek alanlar seçin.  Ölçüt alan değerlerini değiştirmek için iptal etmek ve istediğiniz ölçütlerle eşleşen başka bir kayıt seçin.

### <a name="step-2---perform-initial-extract"></a>2. adım - ilk extract yapar.
Özel alan olacaktır kayıtları tanımladıktan sonra ayıklamak istediğiniz verileri tanımlamak.  Günlük analizi benzer kayıtları benzer düzenleri tanımlamak için bu bilgileri kullanır.  Bundan sonra adımda sonuçları doğrulamak ve daha fazla kendi çözümlemede kullanmak günlük analizi ayrıntılarını sağlamak mümkün olacaktır.

1. Özel alan doldurmak için kullanmak istediğiniz örnek kayıt metinde vurgulayın.  Ardından alan için bir ad verin ve ilk ayıklama gerçekleştirmek için bir iletişim kutusu sunulur.  Karakterleri  **\_CF** otomatik olarak eklenir.
2. Tıklatın **ayıklamak** toplanan kayıtları analizini gerçekleştirmek için.  
3. **Özet** ve **arama sonuçları** bölümleri, kendi doğruluğu incelemek için Ayıkla sonuçları görüntülenir.  **Özet** kaydeder ve sayı her tanımlanan veri değerleri tanımlamak için kullanılan ölçüt görüntüler.  **Arama sonuçlarında** ölçütlerle eşleşen kayıtları ayrıntılı bir listesini sağlar.

### <a name="step-3--verify-accuracy-of-the-extract-and-create-custom-field"></a>Adım 3 – Ayıkla doğruluğunu onaylamak ve özel alan oluşturma
İlk Ayıkla gerçekleştirdikten sonra günlük analizi zaten toplanan verilere dayalı sonuçlarını görüntüler.  Daha sonra sonuçları doğru bakarsanız ile başka hiçbir iş özel alan oluşturabilirsiniz.  Aksi durumda, böylece günlük analizi, mantığını artırabilir sonuçları geliştirebilirsiniz.

1. İlk Ayıkla herhangi bir değer doğru değilse, ardından **Düzenle** bir yanlış yanındaki simge seçin ve kayıt **bu Vurgu değiştirme** seçimi değiştirmek için.
2. Giriş kopyalanır **ek örnekler** altında bölümünde **ana örnek**.  Günlük analizi yaptığınız seçimi anlamanıza yardımcı olması için vurgulama burada ayarlayabilirsiniz.
3. Tıklatın **ayıklamak** varolan tüm kayıtları değerlendirmek için bu yeni bilgiler kullanılacak.  Sonuçları farklı dayalı bu yeni Intelligence üzerinde yalnızca değiştirilen kayıtlar için değiştirilebilir.
4. Extract tüm kayıtları doğru yeni özel alan doldurmak için verileri tanımlayan kadar düzeltmeleri eklemeye devam edin.
5. Tıklatın **Kaydet ayıklamak** Sonuçlardan memnun olduğunda.  Özel alan şimdi tanımlı, ancak bunu henüz herhangi bir kayıt eklenmeyecek.
6. Toplanan ve günlük arama yeniden çalıştırmak için belirtilen ölçütlerle eşleşen yeni kayıtlar için bekleyin. Yeni kayıtlar özel alan olması gerekir.
7. Herhangi bir kayıt özelliği gibi özel alan kullanın.  Veri toplama ve Grup kullanın ve hatta yeni Öngörüler üretmek için kullanın.

## <a name="viewing-custom-fields"></a>Özel alanları görüntüleme
Yönetim grubunuzun içindeki tüm özel alanların listesini görüntüleyebilirsiniz **ayarları** OMS Pano parçasına.  Seçin **veri** ve ardından **özel alanlar** çalışma alanınızdaki tüm özel alanlar listesi.  

![Özel alanlar](media/log-analytics-custom-fields/list.png)

## <a name="removing-a-custom-field"></a>Özel bir alan kaldırma
Özel bir alanı kaldırmak için iki yolu vardır.  İlk **kaldırmak** yukarıda açıklandığı gibi tam listesini görüntülerken, her bir alan için seçeneği.  Diğer yöntem olduğu bir kayıt almak ve alanın solundaki düğmesine tıklayın.  Menü özel alan kaldırmak için bir seçenek içerir.

## <a name="sample-walkthrough"></a>Örnek gözden geçirme
Aşağıdaki bölümde, özel bir alan oluşturma tam bir örnek anlatılmaktadır.  Bu örnek, bir hizmetin durumunu değiştirme belirten Windows olayları hizmetin adı ayıklar.  Bu Windows bilgisayarlarda sistem günlüğündeki hizmet denetimi yöneticisi tarafından oluşturulan olayları kullanır.  Bu örnek izleyin istiyorsanız, olmalıdır [sistem günlüğü bilgilerini olay toplama](log-analytics-data-sources-windows-events.md).

Biz, hizmet denetimi yöneticisinden olay hangi başlayan veya durdurulan bir hizmeti gösterir olay kimliği 7036 olan tüm olayları döndürmek için aşağıdaki sorguyu girin.

![Sorgu](media/log-analytics-custom-fields/query.png)

Biz sonra olay kimliği 7036 içeren herhangi bir kayıt seçin.

![Kaynak kaydı](media/log-analytics-custom-fields/source-record.png)

Görünür hizmet adı istiyoruz **RenderedDescription** özelliği ve bu özelliğin yanındaki düğmesini seçin.

![Alanları ayıklayın](media/log-analytics-custom-fields/extract-fields.png)

**Alan ayıklama Sihirbazı** açıldığında ve **EventLog** ve **EventID** içinde seçili alanları **ana örnek** sütun.  Bu, özel alan 7036 olay kimliği sistem günlüğündeki olaylar için tanımlanan olduğunu gösterir.  Bu, size herhangi bir alan seçin gerek kalmaması yeterlidir.

![Ana örneği](media/log-analytics-custom-fields/main-example.png)

Biz hizmeti adını vurgulayın **RenderedDescription** özelliği ve kullanım **hizmet** hizmet adını belirlemek için.  Özel alan çağrılacağı **Service_CF**.

![Alan başlığı](media/log-analytics-custom-fields/field-title.png)

Hizmet adı için diğer ancak bazı kayıtlar için düzgün şekilde tanımlanır bakın.   **Arama sonuçları** adı kısmı Göster **WMI Performans bağdaştırıcısı** seçili değildi.  **Özet** dört kayıtlar gösterir **DPRMA** hizmet yanlış dahil ek bir sözcük ve tanımlanan iki kayıt **Modül Yükleyicisi** yerine**Windows Modül Yükleyicisi**.  

![Arama sonuçları](media/log-analytics-custom-fields/search-results-01.png)

Biz başlayın **WMI Performans bağdaştırıcısı** kaydı.  Biz, düzenleme simgesine tıklayın ve ardından **bu Vurgu değiştirme**.  

![Vurgu değiştirme](media/log-analytics-custom-fields/modify-highlight.png)

Biz sözcüğünü eklemediğinizden Vurgu artırmak **WMI** Ayıkla yeniden çalıştırın.  

![Ek örnek](media/log-analytics-custom-fields/additional-example-01.png)

Görebiliriz girişleri **WMI Performans bağdaştırıcısı** düzeltildi, günlük analizi ayrıca kullanılır ve bu bilgileri kayıtlarını düzeltmek için **Windows Modülü Yükleyicisi**.  İçinde görebiliriz **Özet** rağmen bölüm **DPMRA** hala doğru tanımlanamadı.

![Arama sonuçları](media/log-analytics-custom-fields/search-results-02.png)

Biz DPMRA hizmetini içeren bir kayıt kaydırın ve o kaydın düzeltmek için aynı işlemi kullanın.

![Ek örnek](media/log-analytics-custom-fields/additional-example-02.png)

 Biz ayıklama çalıştırdığınızda, tüm sonuçları şimdi doğruluğunu görebiliriz.

![Arama sonuçları](media/log-analytics-custom-fields/search-results-03.png)

Görebiliriz **Service_CF** oluşturuldu ancak henüz herhangi bir kayıt eklenmedi.

![İlk sayımı](media/log-analytics-custom-fields/initial-count.png)

Bu nedenle yeni bir süre geçtikten sonra olayları toplanır, görebiliriz, **Service_CF** alan bizim ölçütle eşleşen kayıtları eklenmiş.

![Son sonuçları](media/log-analytics-custom-fields/final-results.png)

Biz, artık herhangi bir kayıt özelliği gibi özel alan kullanabilirsiniz.  Yeni gruplar bir sorgu oluşturuyoruz bunu göstermek için **Service_CF** hangi hizmetlerin en etkin olan incelemek için alan.

![Sorgu tarafından Grup](media/log-analytics-custom-fields/query-group.png)

## <a name="next-steps"></a>Sonraki adımlar
* Hakkında bilgi edinin [oturum aramaları](log-analytics-log-searches.md) özel alanlar için ölçüt kullanarak sorguları oluşturmak için.
* İzleyici [özel günlük dosyalarını](log-analytics-data-sources-custom-logs.md) , özel alanlar kullanarak ayrıştırılamıyor.

