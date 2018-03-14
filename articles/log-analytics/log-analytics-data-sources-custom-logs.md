---
title: "Azure günlük analizi özel günlüklere toplamak | Microsoft Docs"
description: "Günlük analizi, hem Windows hem de Linux bilgisayarlarda metin dosyalarından olayları toplayabilir.  Bu makalede yeni bir özel günlük ve Ayrıntılar için günlük analizi çalışma alanında oluşturdukları kayıtlarının nasıl tanımlanacağını açıklar."
services: log-analytics
documentationcenter: 
author: bwren
manager: jwhit
editor: tysonn
ms.assetid: aca7f6bb-6f53-4fd4-a45c-93f12ead4ae1
ms.service: log-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 12/14/2017
ms.author: bwren
ms.openlocfilehash: 401fbb39194a24721274f55f0fc2a4cdc235a32b
ms.sourcegitcommit: a0be2dc237d30b7f79914e8adfb85299571374ec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/13/2018
---
# <a name="custom-logs-in-log-analytics"></a>Günlük analizi özel günlükleri
Günlük analizi özel günlükleri veri kaynağında Windows ve Linux bilgisayarlarda metin dosyalarından olayları toplamanızı sağlar. Birçok uygulama bilgileri Windows olay günlüğü veya Syslog gibi standart günlük hizmetlerini yerine metin dosyaları oturum açın.  Toplandığında, her tek tek alanların kullanarak oturum açma kaydında ayrıştıramıyor [özel alanlar](log-analytics-custom-fields.md) günlük analizi özelliğidir.

![Özel günlük toplama](media/log-analytics-data-sources-custom-logs/overview.png)

Toplanacak günlük dosyaları aşağıdaki ölçütlere uyan gerekir.

- Günlük, her satırda tek bir giriş olması gerekir ya da her girişin başında şu biçimlerden birini eşleşen bir zaman damgası kullanın.

    YYYY-AA-GG SS: DD:<br>D.M.YYYY HH: MM: SS AM/PM <br>MON DD, YYYY SS: dd:

- Günlük dosyası dosyanın yeni girişlerle burada üzerine döngüsel güncelleştirmeleri izin vermemelidir.
- Günlük dosyası, ASCII veya UTF-8 kodlamasını kullanmanız gerekir.  UTF-16 gibi diğer biçimlere desteklenmez.

>[!NOTE]
>Günlük dosyasında yinelenen girişler varsa günlük analizi bunları toplar.  Ancak, arama sonuçlarını filtre sonuçlarını sonuç sayısından daha fazla olay show burada tutarsız olur.  Özel günlük koleksiyonu tanımı oluşturmadan önce mümkünse adresi ve onu oluşturan uygulamayı Bu davranış neden olup olmadığını belirlemek için günlük doğrulamak önemli olacaktır.  
>
  
## <a name="defining-a-custom-log"></a>Özel günlük tanımlama
Özel bir günlük dosyası tanımlamak için aşağıdaki yordamı kullanın.  Özel günlük ekleme bir örnek bir kılavuz için bu makalenin sonuna kaydırın.

### <a name="step-1-open-the-custom-log-wizard"></a>1. Adım Özel günlük Sihirbazı'nı açın
Özel günlük Sihirbazı'nı Azure portalında çalışır ve toplamak için yeni bir özel günlük tanımlamanızı sağlar.

1. Azure portalında seçin **günlük analizi** > çalışma alanınızı > **Gelişmiş ayarları**.
2. Tıklayın **veri** > **özel günlükleri**.
3. Varsayılan olarak, tüm yapılandırma değişiklikleri otomatik olarak tüm aracıları için gönderilir.  Linux aracıları için bir yapılandırma dosyası için Fluentd veri toplayıcı gönderilir.  Bu dosyayı her Linux aracısında el ile değiştirmek isterseniz, kutunun işaretini *aşağıdaki yapılandırmayı Linux makinelerime Uygula*.
4. Tıklatın **Ekle +** özel günlük Sihirbazı'nı açın.

### <a name="step-2-upload-and-parse-a-sample-log"></a>2. Adım Karşıya yükleme ve bir örnek günlük ayrıştırma
Özel günlük örneği yükleyerek başlatın.  Sihirbaz ayrıştırma ve doğrulamak bu dosyada girişleri görüntüleyin.  Günlük analizi her kaydı tanımlamak için belirttiğiniz sınırlayıcı kullanır.

**Yeni satır** varsayılan sınırlayıcı ve her satırda tek bir giriş sahip günlük dosyaları için kullanılır.  Satır bir tarih ve saat kullanılabilir biçimlerden birinde ile başlayan ardından belirtebilirsiniz bir **zaman damgası** birden fazla satır span girişler destekleyen sınırlayıcısı.

Bir zaman damgası ayırıcısı kullanılırsa, günlük analizi saklanan her kaydı TimeGenerated özelliği bu giriş günlük dosyası için belirtilen tarih/saat ile doldurulur.  Yeni satır ayırıcı kullanılırsa, TimeGenerated tarih ve saat günlük analizi giriş toplanan ile doldurulur.


1. Tıklatın **Gözat** ve bir örnek dosyasına göz atın.  Bu düğme Not etiketli **Dosya Seç** bazı tarayıcılarda.
2. **İleri**’ye tıklayın.
3. Özel günlük Sihirbazı'nı dosyasını karşıya yükleyin ve tanımladığı kayıtları listeler.
4. Yeni bir kayıt tanımlamak ve en iyi kayıtları, günlük dosyasında tanımlar sınırlayıcıyı seçmek için kullanılan sınırlayıcıyı değiştirin.
5. **İleri**’ye tıklayın.

### <a name="step-3-add-log-collection-paths"></a>3. Adım Günlük koleksiyonu yolları Ekle
Özel günlük burada bulabilirsiniz Aracısı'nı bir veya daha fazla yol tanımlamanız gerekir.  Belirli yolu ve günlük dosyasının adını ya da sağlayabilir veya adı için bir joker karakter içeren bir yol belirtin.  Bu, her gün veya bir dosya belirli bir boyuta ulaştığında yeni bir dosya oluşturun uygulamaları destekler.  Ayrıca, tek bir günlük dosyası için birden fazla yol sağlayabilir.

Örneğin, log20100316.txt olduğu gibi ad dahil tarihi ile bir uygulama bir günlük dosyası her gün oluşturabilirsiniz. Bu tür bir oturum için bir desen olabilir *günlük\*.txt* düzeni adlandırma uygulama aşağıdaki herhangi bir günlük dosyası için geçerli olur.

Aşağıdaki tabloda farklı günlük dosyaları belirtmek için geçerli düzeni örnekleri sağlar.

| Açıklama | Yol |
|:--- |:--- |
| Tüm dosyaları *C:\Logs* .txt uzantısı Windows aracı |C:\Logs\\\*.txt |
| Tüm dosyaları *C:\Logs* günlüğünü ve Windows aracısında .txt uzantısı ile başlayan bir ada sahip |C:\Logs\log\*.txt |
| Tüm dosyaları */var/log/audit* Linux Aracısı .txt uzantısı ile |/var/log/audit/*.txt |
| Tüm dosyaları */var/log/audit* günlük ve Linux aracısı üzerinde .txt uzantısı ile başlayan bir ada sahip |/var/log/audit/log\*.txt |

1. Select Windows veya Linux hangi yol biçimi belirtmek için ekliyorsunuz.
2. ' I tıklatın ve yolunu yazın  **+**  düğmesi.
3. Tüm ek yollar için bu işlemi yineleyin.

### <a name="step-4-provide-a-name-and-description-for-the-log"></a>4. Adım. Bir ad ve açıklama günlüğü sağlayın
Belirttiğiniz ad, yukarıda açıklandığı gibi günlük türü için kullanılır.  Özel bir günlük ayırt etmek için _CL ile her zaman sona erer.

1. Günlük için bir ad yazın.   **\_CL** soneki otomatik olarak sağlanır.
2. İsteğe bağlı bir ekleme **açıklama**.
3. Tıklatın **sonraki** özel günlük tanımını kaydetmek için.

### <a name="step-5-validate-that-the-custom-logs-are-being-collected"></a>5. Adım. Özel günlükler toplanmakta olan doğrula
Bu ilk veriler için bir saat için yeni bir özel günlüğünden günlük analizi görünmesi kadar sürebilir.  Girişleri toplamaya başlayacaktır özel günlük tanımlanan noktasından belirttiğiniz yolda bulunan günlüklerinden.  Özel günlük oluşturma sırasında karşıya girişleri bulamayacaktır ancak bulmadığı günlük dosyası zaten mevcut olan girişleri toplar.

Özel günlük toplama günlük analizi başladıktan sonra bir günlük arama kayıtlarını kullanılabilir.  Özel günlük olarak verdiğiniz ad **türü** Sorgunuzdaki.

> [!NOTE]
> RawData özelliği arama eksikse, tarayıcınızı kapatıp gerekebilir.
>
>

### <a name="step-6-parse-the-custom-log-entries"></a>6. Adım. Özel günlük girişlerini ayrıştırılamıyor
Tüm günlük girişi olarak adlandırılan tek bir özellikte depolanacak **RawData**.  Büyük olasılıkla kayıtta depolanan ayrı ayrı Özellikler içinde her giriş bilgilerini farklı parçalarının ayrı isteyeceksiniz.  Kullanarak bunu [özel alanlar](log-analytics-custom-fields.md) günlük analizi özelliğidir.

Özel günlük girişinin ayrıştırma için ayrıntılı adımlar burada sağlanmaz.  Lütfen [özel alanlar](log-analytics-custom-fields.md) bu bilgi için.

## <a name="removing-a-custom-log"></a>Özel günlük kaldırma
Aşağıdaki işlem Azure Portalı'nda önceden tanımlanmış özel bir günlük kaldırmak için kullanın.

1. Gelen **veri** menüde **Gelişmiş ayarları** , çalışma alanınızı seçin **özel günlükler** tüm özel günlüklerini listelemek için.
2. Tıklatın **kaldırmak** kaldırmak için özel günlük yanındaki.


## <a name="data-collection"></a>Veri toplama
Günlük analizi yaklaşık her 5 dakikada her özel günlüğünden yeni girişler toplar.  Aracı, onun yerine üzerinden topladığı her günlük dosyasına kaydeder.  Aracı bir süre için çevrimdışı olursa, aracıyı çevrimdışıyken girişler oluşturulmuş olsalar bile sonra günlük analizi girişleri son devre dışı kaldığı toplar.

Günlük girişinin tüm içeriğini adlı tek bir özellik için yazılan **RawData**.  Bu, analiz ve ayrı olarak tanımlayarak aranır birden fazla özelliklerini içine ayrıştıramıyor [özel alanlar](log-analytics-custom-fields.md) özel günlük oluşturduktan sonra.

## <a name="custom-log-record-properties"></a>Özel günlük kaydı Özellikler
Özel günlük kayıtları aşağıdaki tabloda sağladığınız günlük adı ve özellikleri ile bir türe sahip.

| Özellik | Açıklama |
|:--- |:--- |
| TimeGenerated |Tarih ve saat kaydı günlük analizi tarafından toplanır.  Günlük bir zaman tabanlı ayırıcısı kullanıyorsa bu girdisinden toplanan zamanı gelmiştir. |
| SourceSystem |Aracı kaydı toplandığı türü. <br> OpsManager – Windows aracı, ya da doğrudan bağlanın veya System Center Operations Manager <br> Linux – tüm Linux aracıları |
| RawData |Toplanan girişi tam metni. |
| ManagementGroupName |Aracıları System Center işlemlerini yönetmek için yönetim grubu adı.  Diğer aracılar için AOI - budur\<çalışma alanı kimliği\> |

## <a name="log-searches-with-custom-log-records"></a>Özel günlük kayıtları ile günlük aramalar
Özel günlükler kayıtlarından günlük analizi çalışma alanı gibi başka bir veri kaynağına ait kayıtları depolanır.  Bunlar belirli bir günlüğünden toplanan kayıtları almak üzere aramanızda Type özelliği kullanabilmeniz için günlüğü tanımladığınızda, sağladığınız adıyla eşleşen bir türe sahip.

Aşağıdaki tabloda özel günlüklerinden kayıtları almak günlük arama farklı örnekleri sağlar.

| Sorgu | Açıklama |
|:--- |:--- |
| MyApp_CL |Özel bir tüm olayları adlandırılmış MyApp_CL oturum açın. |
| MyApp_CL &#124; where Severity_CF=="error" |Özel bir tüm olayları adlandırılmış MyApp_CL değeriyle oturum *hata* adlı özel bir alandaki *Severity_CF*. |


## <a name="sample-walkthrough-of-adding-a-custom-log"></a>Özel günlük ekleme örnek gözden geçirme
Aşağıdaki bölümde, özel bir günlük oluşturma örneği anlatılmaktadır.  Toplanmakta olan örnek günlük bir tarih ve saat ve ardından virgülle ayrılmış alanlar için kodu, durum ve iletinizi başlayarak her satırda tek bir giriş vardır.  Aşağıda birkaç örnek girdileri gösterilmektedir.

    2016-03-10 01:34:36 207,Success,Client 05a26a97-272a-4bc9-8f64-269d154b0e39 connected
    2016-03-10 01:33:33 208,Warning,Client ec53d95c-1c88-41ae-8174-92104212de5d disconnected
    2016-03-10 01:35:44 209,Success,Transaction 10d65890-b003-48f8-9cfc-9c74b51189c8 succeeded
    2016-03-10 01:38:22 302,Error,Application could not connect to database
    2016-03-10 01:31:34 303,Error,Application lost connection to database

### <a name="upload-and-parse-a-sample-log"></a>Karşıya yükleme ve bir örnek günlük ayrıştırma
Biz, günlük dosyalarından birini sağlayın ve bu toplama olayları görebilirsiniz.  Bu durumda yeni bir yeterli ayırıcısı satırıdır.  Tek bir giriş günlüğüne çok satırlı ancak kapsayabilir, bir zaman damgası ayırıcısı kullanılması gerekir.

![Karşıya yükleme ve bir örnek günlük ayrıştırma](media/log-analytics-data-sources-custom-logs/delimiter.png)

### <a name="add-log-collection-paths"></a>Günlük koleksiyonu yolları Ekle
Günlük dosyaları yer *C:\MyApp\Logs*.  Her gün düzende tarih içeren bir ad ile yeni bir dosya oluşturulur *appYYYYMMDD.log*.  Bu günlük dosyası için yeterli bir deseni olacaktır *C:\MyApp\Logs\\\*.log*.

![Günlük koleksiyonu yolu](media/log-analytics-data-sources-custom-logs/collection-path.png)

### <a name="provide-a-name-and-description-for-the-log"></a>Bir ad ve açıklama günlüğü sağlayın
Adını kullanıyoruz *MyApp_CL* ve yazın bir **açıklama**.

![Günlük adı](media/log-analytics-data-sources-custom-logs/log-name.png)

### <a name="validate-that-the-custom-logs-are-being-collected"></a>Özel günlükler toplanmakta olan doğrula
Bir sorgu kullanırız *türü MyApp_CL =* toplanan günlükteki tüm kayıtları döndürmek için.

![Hiçbir özel alanlarla günlüğü sorgusu](media/log-analytics-data-sources-custom-logs/query-01.png)

### <a name="parse-the-custom-log-entries"></a>Özel günlük girişlerini ayrıştırılamıyor
Özel alanları tanımlamak için kullanırız *EventTime*, *kod*, *durum*, ve *ileti* alanları ve biz sorgu tarafından döndürülen kayıt farkı görebilirsiniz.

![Özel alanlarla günlüğü sorgusu](media/log-analytics-data-sources-custom-logs/query-02.png)

## <a name="next-steps"></a>Sonraki adımlar
* Kullanım [özel alanlar](log-analytics-custom-fields.md) tek tek alanların özel oturum açma girdileri ayrıştırılamıyor.
* Hakkında bilgi edinin [oturum aramaları](log-analytics-log-searches.md) veri kaynakları ve çözümleri toplanan verileri çözümlemek için.
