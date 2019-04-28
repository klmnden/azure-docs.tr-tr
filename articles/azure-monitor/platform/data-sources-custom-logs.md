---
title: Azure İzleyici'de özel günlüklerini toplama | Microsoft Docs
description: Azure İzleyici, hem Windows hem de Linux bilgisayarlarda metin dosyalarından olayları toplayabilir.  Bu makalede yeni bir özel günlük ve Azure İzleyici'de oluşturdukları kayıtları ayrıntılarını nasıl tanımlanacağını açıklar.
services: log-analytics
documentationcenter: ''
author: bwren
manager: carmonm
editor: tysonn
ms.assetid: aca7f6bb-6f53-4fd4-a45c-93f12ead4ae1
ms.service: log-analytics
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/12/2019
ms.author: bwren
ms.openlocfilehash: c80736dcd8be0c7ff3aae850aaaf9659f47daf36
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60996526"
---
# <a name="custom-logs-in-azure-monitor"></a>Azure İzleyici'de özel günlükler
Azure İzleyici'de özel günlükleri veri kaynağı, hem Windows hem de Linux bilgisayarlarda metin dosyalarından olaylarını toplamanıza olanak sağlar. Birçok uygulama için Windows olay günlüğü veya Syslog gibi standart günlük hizmetlerinin yerine metin dosyaları bilgileri günlüğe kaydetmek. Toplandığında, verileri ayrı ayrı alanlara sorgularınızdaki ayrıştırmak veya sırasında ayrı alanlar koleksiyonuna olan verileri ayıklayın.

![Özel günlük toplama](media/data-sources-custom-logs/overview.png)

Günlük dosyaları toplanacak, aşağıdaki ölçütlere uymalıdır.

- Günlük her satırda tek bir giriş olan veya bir zaman damgası eşleşen her giriş başlangıcına aşağıdaki biçimlerden birini kullanın.

    YYYY-AA-GG SS: DD:<br>D.M.YYYY HH: MM: SS AM/PM<br>Ay gg, YYYY SS: dd:<br />yyMMdd HH:mm:ss<br />ddMMyy HH:mm:ss<br />MMM d ss: dd:<br />dd/MMM/yyyy:HH:mm:ss zzz<br />yyyy-aa-ddTHH:mm:ssK

- Günlük dosyası döngüsel günlüğü ya da günlük döndürme, dosyanın yeni girişlerle burada üzerine izin vermemesi gerekir.
- Günlük dosyası, ASCII veya UTF-8 kodlamasını kullanmanız gerekir.  UTF-16 gibi diğer biçimlerde desteklenmez.

>[!NOTE]
>Günlük dosyasında yinelenen girişler varsa, bunları Azure İzleyici toplar.  Ancak, sorgu sonuçları nerede sonucu sayısından daha fazla olay filtre sonuçlarını göster tutarsız olur.  Günlük oluşturduğu uygulama bu davranış neden olup olmadığını belirlemek ve eğer mümkünse bunu özel günlük koleksiyonu tanımı oluşturmadan önce çözmek için doğrulama önemli olacaktır.  
>
  
>[!NOTE]
> Her gün veya belirli bir boyuta ulaştığında, uygulamanızın yeni bir günlük dosyası oluşturur, yeniden başlatıldıktan sonra Linux için Log Analytics aracısını kadar bunları bulmaz. Aracı yalnızca numaralandırır ve başlatılması sırasında belirtilen günlükleri desenler için izlemeye başlar ve bu nedenle yeniden başlatma aracı işlemi otomatik hale getirerek etrafında planlamanız gerekir çünkü budur.  Bu sınırlama Windows için Log Analytics aracısı yok.  
>

>[!NOTE]
> Bir Log Analytics çalışma alanı aşağıdaki sınırları destekler:
> 
> * Yalnızca 500 özel günlükleri oluşturulabilir.
> * Bir tablo yalnızca en fazla 500 sütunları destekler. 
> * Sütun adı karakter sayısı 500'dür. 
>

## <a name="defining-a-custom-log"></a>Özel günlük tanımlama
Özel bir günlük dosyası tanımlamak için aşağıdaki yordamı kullanın.  Özel günlük ekleme örnek bir kılavuz bu makalenin sonuna kaydırın.

### <a name="step-1-open-the-custom-log-wizard"></a>1. Adım Özel günlük Sihirbazı'nı açın
Özel günlük Sihirbazı, Azure portalında çalışır ve toplamak için yeni bir özel günlük tanımlamanızı sağlar.

1. Azure portalında **Log Analytics çalışma alanları** > çalışma alanınızı > **Gelişmiş ayarlar**.
2. Tıklayarak **veri** > **özel günlükleri**.
3. Varsayılan olarak, tüm yapılandırma değişiklikleri tüm aracılara otomatik olarak gönderilir.  Linux aracıları için bir yapılandırma dosyası Fluentd veri toplayıcısı gönderilir.  Her bir Linux aracı bu dosyayı el ile değiştirmek istiyorsanız, kutunun işaretini kaldırın *aşağıdaki yapılandırmayı Linux makinelerime Uygula*.
4. Tıklayın **Ekle +** özel günlük Sihirbazı'nı açın.

### <a name="step-2-upload-and-parse-a-sample-log"></a>2. Adım Karşıya yükleme ve bir örnek günlük ayrıştırma
Özel günlük örneği yükleyerek başlarsınız.  Sihirbaz, ayrıştırma ve doğrulamak bu dosyayı girişleri görüntülemek.  Azure İzleyici, her kaydı tanımlamak için belirttiğiniz sınırlayıcıyı kullanır.

**Yeni satır** varsayılan sınırlayıcı ve her satırda tek bir giriş olan günlük dosyaları için kullanılır.  Bir tarih ve saatte kullanılabilir biçimlerden birinde ile satırda başlatılan ardından belirtebileceğiniz bir **zaman damgası** birden fazla satırı span girişleri destekleyen sınırlayıcı.

Zaman damgası sınırlayıcı kullandıysanız, Azure İzleyici'de depolanan her kaydın TimeGenerated özelliği bu günlük dosyası girişi için belirtilen tarih/saat ile doldurulur.  Yeni bir satır ayırıcı kullanılırsa, TimeGenerated tarih ve saat Azure İzleyici giriş toplanan ile doldurulur.


1. Tıklayın **Gözat** ve bir örnek dosyasına göz atın.  Bu düğme Not etiketli **Dosya Seç** bazı tarayıcılarda.
2. **İleri**’ye tıklayın.
3. Özel günlük Sihirbazı'nı dosyayı yükleyin ve tanımladığı kayıtları listesi.
4. Yeni bir kayıt tanımlamak ve günlük dosyanız kayıtlara en iyi şekilde tanımlayan bir sınırlayıcı seçmek için kullanılan sınırlayıcıyı değiştirin.
5. **İleri**’ye tıklayın.

### <a name="step-3-add-log-collection-paths"></a>3. Adım Günlük koleksiyonu yolları ekle
Özel günlük burada bulabilirsiniz aracı üzerinde bir veya daha fazla yol tanımlamanız gerekir.  Belirli bir yol ve günlük dosyasının adı ya da sağlayabilir veya adı için joker karakter içeren bir yol belirtebilirsiniz. Bu, her gün veya bir dosyada belirli bir boyuta ulaştığında yeni bir dosya oluşturun, uygulamaları destekler. Ayrıca, tek bir günlük dosyası için birden çok yol sağlayabilir.

Örneğin, adında log20100316.txt olduğu gibi dahil tarihi olan bir uygulama bir günlük dosyası her gün oluşturabilirsiniz. Bu tür bir günlük için bir düzen olabilir *günlük\*.txt* düzeni adlandırma uygulama aşağıdaki herhangi bir günlük dosyası için geçerli.

>[!NOTE]
> Her gün veya belirli bir boyuta ulaştığında, uygulamanızın yeni bir günlük dosyası oluşturur, yeniden başlatıldıktan sonra Linux için Log Analytics aracısını kadar bunları bulmaz. Aracı yalnızca numaralandırır ve başlatılması sırasında belirtilen günlükleri desenler için izlemeye başlar ve bu nedenle yeniden başlatma aracı işlemi otomatik hale getirerek etrafında planlamanız gerekir çünkü budur.  Bu sınırlama Windows için Log Analytics aracısı yok.  
>

Aşağıdaki tabloda farklı günlük dosyaları belirtmek için geçerli düzeni örnekleri sağlar.

| Açıklama | Yol |
|:--- |:--- |
| Tüm dosyaları *C:\Logs* Windows aracısında .txt uzantısı |C:\Logs\\\*.txt |
| Tüm dosyaları *C:\Logs* günlüğünü ve Windows aracısında bir .txt uzantısı ile başlayan bir ada sahip |C:\Logs\log\*.txt |
| Tüm dosyaları */var/log/audit* Linux aracısı üzerinde .txt uzantısı |/var/log/audit/*.txt |
| Tüm dosyaları */var/log/audit* günlüğü ve Linux Aracısı bir .txt uzantısı ile başlayan bir ada sahip |/var/log/Audit/log\*.txt |

1. Windows veya Linux seçin hangi yol biçimi belirtmek için ekliyoruz.
2. Tür yolu tıklayıp **+** düğmesi.
3. Herhangi bir ek yollar için işlemi tekrarlayın.

### <a name="step-4-provide-a-name-and-description-for-the-log"></a>4. Adım. Bir ad ve açıklama günlüğü sağlayın
Belirttiğiniz ad, yukarıda açıklandığı gibi günlük türü için kullanılacaktır.  Özel bir günlük ayırmak için _CL ile her zaman sona erecek.

1. Günlük için'bir ad yazın.   **\_CL** soneki otomatik olarak sağlanır.
2. İsteğe bağlı bir ekleme **açıklama**.
3. Tıklayın **sonraki** özel günlük tanımını kaydedin.

### <a name="step-5-validate-that-the-custom-logs-are-being-collected"></a>5. Adım. Özel günlükler toplanan doğrula
Bu ilk veriler için bir saat yeni bir özel günlüğünden Azure İzleyici'de görüntülenecek kadar sürebilir.  Girdileri toplamaya başlar özel günlük tanımlandığı noktadan itibaren belirttiğiniz günlüklerinden yolunda bulunamadı.  Özel günlük oluşturma sırasında yüklenen girişler korumaz, ancak varolan girişleri bulmadığı günlük dosyalarında toplar.

Azure İzleyici özel günlük toplama başladığında, bir günlük sorgusu ile kayıtlarını kullanılabilir.  Özel günlük olarak verdiğiniz ad **türü** sorgunuzda.

> [!NOTE]
> RawData özelliği sorguda eksik, tarayıcınızı kapatıp gerekebilir.


### <a name="step-6-parse-the-custom-log-entries"></a>6. Adım. Özel günlük girişlerini ayrıştırılamıyor
Tüm günlük girişi adlı tek bir özellik içinde saklanan **RawData**.  Her kayıt için ayrı ayrı Özellikler içinde her giriş bilgilerinin farklı parçaları ayırmak büyük olasılıkla isteyeceksiniz. Başvurmak [ayrıştırma metin verilerini Azure İzleyici'de](../log-query/parse-text.md) ayrıştırma seçenekleri için **RawData** birden çok özellikleri.

## <a name="removing-a-custom-log"></a>Özel günlük kaldırılıyor
Aşağıdaki işlemi Azure portalında önceden tanımlanmış özel bir günlük kaldırmak için kullanın.

1. Gelen **veri** menüde **Gelişmiş ayarlar** çalışma alanınız için seçin **özel günlükleri** özel günlüklerinizi listelemek için.
2. Tıklayın **Kaldır** kaldırmak için özel günlük yanında.


## <a name="data-collection"></a>Veri toplama
Azure İzleyici yaklaşık her 5 dakikada her özel günlüğünden yeni girişler toplar.  Aracı, onun yerine toplar, her bir günlük dosyasına kaydeder.  Aracıyı bir süre için çevrimdışı olursa, girişler aracının çevrimdışı durumdayken oluşturulmuş olsalar bile sonra Azure İzleyici girişleri son devre dışı kaldığı toplar.

Günlük giriş öğesinin tüm içeriğini adlı tek bir özellik için yazılan **RawData**.  Bkz [ayrıştırma metin verilerini Azure İzleyici'de](../log-query/parse-text.md) her ayrıştırmak yöntemler için birden çok özellik günlük girişine içeri aktarıldı.

## <a name="custom-log-record-properties"></a>Özel günlük kaydı özellikleri
Özel günlük kayıtları, aşağıdaki tabloda günlük adını sağlayan ve özellikleri ile bir türü vardır.

| Özellik | Açıklama |
|:--- |:--- |
| TimeGenerated |Tarih ve saat kaydı, Azure İzleyici tarafından toplanan.  Zamana bağlı bir sınırlayıcı günlük kullanıyorsa, ardından bu girişi toplanan zamandır. |
| SourceSystem |Aracı kaydı toplandığı türü. <br> OpsManager – Windows Aracısı, doğrudan bağlanın veya System Center Operations Manager <br> Linux – tüm Linux aracıları |
| RawData |Toplanan girişinin tam metin. Büyük olasılıkla isteyeceksiniz [ayrı ayrı Özellikler içinde bu verileri ayrıştırmak](../log-query/parse-text.md). |
| ManagementGroupName |Aracıları System Center Operations yönetmek için yönetim grubunun adı.  Diğer aracılar için AOI - budur\<çalışma alanı kimliği\> |


## <a name="sample-walkthrough-of-adding-a-custom-log"></a>Özel günlük ekleme izlenecek örnek yol
Aşağıdaki bölümde, özel bir günlük oluşturmanın bir örneği açıklanmaktadır.  Toplanan örnek günlük bir tarih ve saat ve ardından virgülle ayrılmış alanlar için kod, durum ve ileti başlayarak her satırın tek bir giriş vardır.  Aşağıda birkaç örnek girdi gösterilmektedir.

    2016-03-10 01:34:36 207,Success,Client 05a26a97-272a-4bc9-8f64-269d154b0e39 connected
    2016-03-10 01:33:33 208,Warning,Client ec53d95c-1c88-41ae-8174-92104212de5d disconnected
    2016-03-10 01:35:44 209,Success,Transaction 10d65890-b003-48f8-9cfc-9c74b51189c8 succeeded
    2016-03-10 01:38:22 302,Error,Application could not connect to database
    2016-03-10 01:31:34 303,Error,Application lost connection to database

### <a name="upload-and-parse-a-sample-log"></a>Karşıya yükleme ve bir örnek günlük ayrıştırma
Biz, günlük dosyalarından birini belirtin ve bu toplama olayları görebilirsiniz.  Bu durumda yeni bir satır yeterli sınırlayıcı ' dir.  Tek bir giriş günlüğüne çok satırlı yine de kapsayabilir, zaman damgası sınırlayıcı kullanılacak gerekir.

![Karşıya yükleme ve bir örnek günlük ayrıştırma](media/data-sources-custom-logs/delimiter.png)

### <a name="add-log-collection-paths"></a>Günlük koleksiyonu yolları ekle
Günlük dosyaları yer *C:\MyApp\Logs*.  Her gün düzende tarih içeren bir ad ile oluşturulan yeni dosyaları *appYYYYMMDD.log*.  Bu günlük dosyası için yeterli bir düzen olacaktır *C:\MyApp\Logs\\\*.log*.

![Günlük koleksiyonu yolu](media/data-sources-custom-logs/collection-path.png)

### <a name="provide-a-name-and-description-for-the-log"></a>Bir ad ve açıklama günlüğü sağlayın
Adını kullanıyoruz *MyApp_CL* ve yazın bir **açıklama**.

![Günlük adı](media/data-sources-custom-logs/log-name.png)

### <a name="validate-that-the-custom-logs-are-being-collected"></a>Özel günlükler toplanan doğrula
Bir sorgu kullandığımız *türü MyApp_CL =* toplanan günlük tüm kayıtları döndürmek için.

![Özel alan ile günlük sorgusu](media/data-sources-custom-logs/query-01.png)

### <a name="parse-the-custom-log-entries"></a>Özel günlük girişlerini ayrıştırılamıyor
Özel alanları tanımlamak için kullanırız *EventTime*, *kod*, *durumu*, ve *ileti* alanları ve kayıtları farkı görebilirsiniz Bu sorgu tarafından döndürülür.

![Özel alanlarla günlük sorgusu](media/data-sources-custom-logs/query-02.png)

## <a name="alternatives-to-custom-logs"></a>Özel günlükler için alternatifleri
Özel günlükler verilerinize hakkında listelenen ölçütlere uyan, ancak aşağıdaki gibi durumlar kullanışlı olsa da, başka bir strateji gereken:

- Veriler, zaman damgası farklı bir biçimde sahip gibi gerekli yapısı sığmıyor.
- Günlük dosyası, dosya kodlama gibi gereksinimleri veya desteklenmeyen klasör yapısı uymayan.
- Veri ön işleme veya önce koleksiyonu filtreleme gerektirir. 

Burada özel günlükleri ile verilerinizi toplanamıyor durumlarda aşağıdaki alternatif stratejiler göz önünde bulundurun:

- Veri yazılan özel bir betiği veya başka bir yöntem kullanmak [Windows olayları](data-sources-windows-events.md) veya [Syslog](data-sources-syslog.md) Azure İzleyici tarafından toplanır. 
- Azure İzleyicisi'ni kullanarak doğrudan verileri Gönder [HTTP veri toplayıcı API'sini](data-collector-api.md). Azure Otomasyonu'nda runbook'ları kullanarak bir örnek sağlanır [Azure İzleyici toplama günlüğü verileri bir Azure Otomasyonu runbook ile](runbook-datacollect.md).

## <a name="next-steps"></a>Sonraki adımlar
* Bkz [ayrıştırma metin verilerini Azure İzleyici'de](../log-query/parse-text.md) her ayrıştırmak yöntemler için birden çok özellik günlük girişine içeri aktarıldı.
* Hakkında bilgi edinin [oturum sorguları](../log-query/log-query-overview.md) veri kaynakları ve çözümlerinden toplanan verileri analiz etmek için.