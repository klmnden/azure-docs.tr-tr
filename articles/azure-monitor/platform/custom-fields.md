---
title: Azure İzleyici'de özel alanlar | Microsoft Docs
description: Azure İzleyicisi'nin özel alanlar özelliğini kendi aranabilir alanları bir Log Analytics çalışma alanında toplanan kayıt özellikleri bir kayıt oluşturmanıza olanak sağlar.  Bu makalede, özel bir alan oluşturma işlemini açıklar ve bir örnek olay ile ayrıntılı bir kılavuz sağlar.
services: log-analytics
documentationcenter: ''
author: bwren
manager: jwhit
editor: tysonn
ms.assetid: 31572b51-6b57-4945-8208-ecfc3b5304fc
ms.service: log-analytics
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/29/2019
ms.author: bwren
ms.openlocfilehash: 974a3391c592a1caf7bdcc6d9e01032f0c73aaa6
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61461937"
---
# <a name="create-custom-fields-in-a-log-analytics-workspace-in-azure-monitor"></a>Azure İzleyici'de bir Log Analytics çalışma alanında özel alanlar oluşturma

> [!NOTE]
> Bu makalede, bir Log Analytics çalışma alanında metin verileri toplandıktan olarak ayrıştırmak açıklar. Bölümünde anlatıldığı gibi toplandıktan sonra bir sorguda metin verilerini ayrıştırma avantajları vardır [ayrıştırma metin verilerini Azure İzleyici'de](../log-query/parse-text.md).

**Özel alanlar** özelliği, Azure İzleyici sayesinde var olan kayıtların Log Analytics çalışma alanınızda kendi arama yapılabilir alanlar ekleyerek genişletebilir.  Özel alanlar, diğer özellikleri aynı kaydın ayıklanan verilerden otomatik olarak doldurulur.

![Genel Bakış](media/custom-fields/overview.png)

Örneğin, aşağıdaki örnek kayıt olay açıklamasında kaçınma yararlı verileri vardır. Bu verileri ayrı bir özellikte ayıklama olarak sıralama ve filtreleme gibi işlemler için kullanılabilir yapar.

![Örnek ayıklayın](media/custom-fields/sample-extract.png)

> [!NOTE]
> Önizleme sürümünde, çalışma alanınızdaki 100 özel alanlar sınırlıdır.  Bu özellik genel kullanılabilirlik ulaştığında bu sınırı genişletilir.

## <a name="creating-a-custom-field"></a>Özel alan oluşturuluyor
Özel alan oluşturduğunuzda, Log Analytics veri değerini doldurmak için kullanılacak anlamanız gerekir.  Bu verileri hızlı bir şekilde tanımlamak için bir Microsoft Research adlı FlashExtract teknolojisini kullanır.  Azure İzleyici, açık yönergeler sağlamanıza gerek kalmadan, sağladığınız örneklerden ayıklamak istediğiniz veriler hakkındaki öğrenir.

Aşağıdaki bölümler, özel bir alan oluşturmak için yordamı sağlar.  Bu makalenin alt kısmında bir örnek ayıklama kılavuz olur.

> [!NOTE]
> Özel alan oluşturulduktan sonra toplanan kayıtları yalnızca görünür Log Analytics çalışma alanına belirtilen ölçütlerle eşleşen kayıtlar eklendikçe özel alan doldurulur.  Özel alan oluşturulduğunda, veri deposunda zaten olan kayıtları eklenmedi.
> 

### <a name="step-1--identify-records-that-will-have-the-custom-field"></a>1. adım – özel alan olan kayıtların tanımlanması
İlk adım, özel alan alacak kayıtların belirlemektir.  İle başlayan bir [standart günlük sorgusu](../log-query/log-query-overview.md) ve Azure İzleyici'den öğreneceksiniz model olarak görev yapacak bir kaydı seçin.  Bir özel alana, verileri ayıklamak için önerilere şu belirttiğinizde **alan ayıklama Sihirbazı** burada doğrulamak ve kriterlerinizi açılır.

1. Git **günlükleri** ve bir [kayıtları almak için sorgu](../log-query/log-query-overview.md) özel alanı olacaktır.
2. Log Analytics özel alanını doldurmak için veri yönelik bir model olarak davranmak için kullanacağı bir kaydı seçin.  Bu kayıttaki ayıklamak istediğiniz veri tanımlayacak ve Log Analytics benzer tüm kayıtlar için özel alanını doldurmak için mantığı belirlemek için bu bilgileri kullanır.
3. Kayıt Özellikleri'ni genişletin, sol üst kayıt özelliğinin üç nokta işaretine ve seçin **alanları Ayıkla**.
4. **Alan ayıklama Sihirbazı** açılır ve seçtiğiniz kayıt görüntülenen **ana örnek** sütun.  Özel alan seçilen özelliklerinde aynı değerlerle kayıtları için tanımlanır.  
5. Seçimi tam olarak istediğiniz varsa, ölçütleri daraltmak için ek alanlar seçin.  Ölçütleri alan değerlerini değiştirmek için iptal etmek ve istediğiniz ölçütlerle eşleşen farklı bir kaydı seçin.

### <a name="step-2---perform-initial-extract"></a>2. adım - ilk ayıklama gerçekleştirin.
Özel alan olan kayıtları belirledikten sonra ayıklamak istediğiniz verileri belirleyin.  Log Analytics, benzer kayıtları benzer desenleri tanımlamak için bu bilgileri kullanır.  Bundan sonra bir adımda sonuçları doğrulamak ve daha fazla Log Analytics, kendi analizde kullanılacak ayrıntılarını sağlamak mümkün olacaktır.

1. Örnek kaydındaki özel alanını doldurmak için kullanmak istediğiniz metni vurgulayın.  Ardından alan için bir ad ve veri türü sağlamak ve ilk ayıklama gerçekleştirmek için bir iletişim kutusu karşınıza çıkar.  Karakterleri  **\_CF** otomatik olarak eklenir.
2. Tıklayın **ayıklamak** toplanan kayıtları analizini gerçekleştirme.  
3. **Özeti** ve **arama sonuçları** bölümleri doğruluğunun inceleyebilirsiniz. Bu nedenle Ayıkla sonuçlarını görüntüler.  **Özet** kayıtları ve sayı her tanımlanan veri değerlerini tanımlamak için kullanılan ölçüt görüntüler.  **Arama sonuçları** ölçütlerle eşleşen kayıtları ayrıntılı bir listesini sağlar.

### <a name="step-3--verify-accuracy-of-the-extract-and-create-custom-field"></a>3. adım – ayıklama doğruluğunu ve özel alan oluşturma
İlk Ayıkla gerçekleştirdikten sonra Log Analytics zaten toplanmış olan verileri temel alan, sonuçları görüntüler.  Ardından sonuçları doğru bakarsanız başka hiçbir iş ile özel alan oluşturabilirsiniz.  Aksi durumda, Log Analytics mantığını geliştirebilmek sonuçları geliştirebilirsiniz.

1. İlk Ayıkla herhangi bir değer doğru değilse, ardından **Düzenle** bir yanlış yanındaki simgeye kayıt ve seçim **Bu vurgulamayı Değiştir** seçimi değiştirmek için.
2. Giriş kopyalanır **ek örnekler** altında bölümünde **ana örnek**.  Log Analytics yaptığınız seçimi anlamanıza yardımcı olması için vurgulama burada ayarlayabilirsiniz.
3. Tıklayın **ayıklamak** var olan tüm kayıtlardan değerlendirmek için bu yeni bilgiler kullanılacak.  Sonuçları, bu yeni zekasına dayalı yalnızca değiştirilmiş bir dışında kayıtları için değiştirilebilir.
4. Extract içindeki tüm kayıtlar yeni özel alan doldurmak için verileri doğru bir şekilde tanımlayan kadar düzeltmeleri eklemeye devam edin.
5. Tıklayın **Kaydet ayıklamak** Sonuçlardan memnun olduğunuzda.  Özel alan artık tanımlanır, ancak, henüz herhangi bir kayıt eklenmeyecek.
6. Toplanan ve günlük araması'ı yeniden çalıştırmak için belirtilen ölçütlerle eşleşen yeni kayıtlar için bekleyin. Yeni kayıtlar özel alanı olmalıdır.
7. Özel alanı gibi herhangi bir kayıt özelliğini kullanın.  Veri toplama ve Grup kullanın ve hatta yeni bir kavrayış düzeyi oluşturmak için kullanın.

## <a name="viewing-custom-fields"></a>Özel alanları görüntüleme
Kullanarak yönetim grubunuzdaki tüm özel alanların listesini görüntüleyebilirsiniz **Gelişmiş ayarlar** Azure portalında Log Analytics çalışma alanınızın menüsü.  Seçin **veri** ardından **özel alanlar** çalışma alanınızdaki tüm özel alanların bir listesi.  

![Özel alanlar](media/custom-fields/list.png)

## <a name="removing-a-custom-field"></a>Özel bir alan kaldırma
Özel bir alanı kaldırmak için iki yolu vardır.  İlk **Kaldır** yukarıda açıklandığı gibi tam bir listesi görüntülerken, her bir alan için seçeneği.  Diğer kayıt alıp alanın sol düğmesine yöntemidir.  Menü, özel bir alanı kaldırmak için bir seçenek içerir.

## <a name="sample-walkthrough"></a>Örnek kılavuz
Aşağıdaki bölümde, bir özel alan oluşturuluyor, tam bir örneği açıklanmaktadır.  Bu örnek bir hizmetin durumunu değiştirme belirten Windows olayları hizmeti adını ayıklar.  Bu Windows bilgisayarlarda sistem başlatma sırasında hizmet denetimi yöneticisi tarafından oluşturulan olayları kullanır.  Bu örneği takip etmek istiyorsanız, olmalıdır [sistem günlüğü bilgilerini olay toplama](data-sources-windows-events.md).

Biz, hizmet denetimi yöneticisinden olay, hizmet başlatma veya durdurma gösteren olay Kimliğini 7036 sahip tüm olaylar döndürmek için aşağıdaki sorguyu girin.

![Sorgu](media/custom-fields/query.png)

Biz bu seçin ve herhangi bir olay kimliği 7036 kayıtla genişletin.

![Kaynak kaydı](media/custom-fields/source-record.png)

Üst özelliğin yanındaki üç nokta işaretine tıklayarak özel alanlar tanımlarız.

![Alanları Ayıkla](media/custom-fields/extract-fields.png)

**Alan ayıklama Sihirbazı** açılır ve **EventLog** ve **EventID** içinde seçili alanları **ana örnek** sütun.  Bu, özel alan sistem günlüğüne 7036 olay Kimliğini içeren olayları için tanımlanan olduğunu gösterir.  Bu yeterlidir, böylece herhangi bir alan seçmek gerekmez.

![Ana örnek](media/custom-fields/main-example.png)

Biz hizmeti adını vurgulayın **RenderedDescription** özelliği ve kullanım **hizmet** hizmet adını belirlemek için.  Özel alan çağrılacak **Service_CF**. Alan türü bu durumda bir dize ise, böylece biz değiştirilmemiş bırakabilirsiniz.

![Alan başlığı](media/custom-fields/field-title.png)

Hizmet adı için diğer ancak bazı kayıtlar için düzgün şekilde tanımlanır görüyoruz.   **Arama sonuçları** göstermek için bu kısmını **WMI Performans bağdaştırıcısı** seçili değildi.  **Özeti** tanımlanan bir kaydın gösterir **Modül Yükleyicisi** yerine **Windows modülleri yükleyici**.  

![Arama sonuçları](media/custom-fields/search-results-01.png)

Biz başlayın **WMI Performans bağdaştırıcısı** kaydı.  Biz, düzenleme simgesine tıklayın ve ardından **Bu vurgulamayı Değiştir**.  

![Vurgulamayı Değiştir](media/custom-fields/modify-highlight.png)

Biz sözcüğünü eklemediğinizden vurgulama artırmak **WMI** ve ayıklama yeniden çalıştırın.  

![Ek örnek](media/custom-fields/additional-example-01.png)

Görebiliriz girdilerini **WMI Performans bağdaştırıcısı** düzeltildi, Log Analytics de kullanılan ve bu bilgileri kayıtları düzeltmek için **Windows Modülü Yükleyicisi**.

![Arama sonuçları](media/custom-fields/search-results-02.png)

Biz artık doğrulayan bir sorgu çalıştırabilirsiniz **Service_CF** oluşturulur ancak henüz herhangi bir kayıt eklenir. Özel alan karşı var olan kayıtların çalışmaz, bu yüzden toplanacak yeni kayıtlar için beklenecek olmasıdır.

![Başlangıç sayısı](media/custom-fields/initial-count.png)

Süre kadar yeni geçtikten sonra toplanan olayları, görebiliriz **Service_CF** alanı bizim ölçütle eşleşen kayıtları için eklenmiş.

![Son sonuçları](media/custom-fields/final-results.png)

Şimdi özel bir alan herhangi bir kayıt özelliği gibi kullanabiliriz.  Yeni gruplar bir sorgu oluştururuz Bunu açıklamak üzere; **Service_CF** hangi hizmetlerin en etkin olan incelemek için alan.

![Sorgu tarafından Grup](media/custom-fields/query-group.png)

## <a name="next-steps"></a>Sonraki adımlar
* Hakkında bilgi edinin [oturum sorguları](../log-query/log-query-overview.md) özel alanlar için ölçütleri kullanarak sorguları oluşturmak için.
* İzleyici [özel günlük dosyalarını](data-sources-custom-logs.md) , özel alanlara kullanarak ayrıştırılamıyor.

