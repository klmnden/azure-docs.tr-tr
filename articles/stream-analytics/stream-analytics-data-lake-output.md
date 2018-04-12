---
title: Data Lake Store çıktısını Azure akış analizi
description: Kimlik doğrulama ve yetkilendirme Stream Analytics işi bir Azure Data Lake Store'da ve yapılandırma
services: stream-analytics
author: jseb225
ms.author: jeanb
manager: kfile
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 03/28/2017
ms.openlocfilehash: a0586b32fd12744c8bfce782583cdc4078979ef1
ms.sourcegitcommit: 3a4ebcb58192f5bf7969482393090cb356294399
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/12/2018
---
# <a name="stream-analytics-data-lake-store-output"></a>Stream Analytics Data Lake Store çıktı
Akış analizi işleri birkaç çıktı yöntemlerini destekleyen bir anda bir [Azure Data Lake Store](https://azure.microsoft.com/services/data-lake-store/). Azure Data Lake Store, büyük veri analitik iş yükleri için kuruluş çapında hiper ölçekli bir depodur. Data Lake Store herhangi boyutu, türü ve alım hızına işletimsel ve keşifsel analiz için verilerin depolamanıza olanak sağlar.

## <a name="authorize-a-data-lake-store-account"></a>Bir Data Lake Store hesabı yetki
1. Data Lake Store, Azure portalında bir çıkış olarak seçildiğinde, Data Lake Store için erişim istemek için veya varolan Data Lake Store kullanımını yetkilendirmek için istenir.
   
   ![](media/stream-analytics-data-lake-output/stream-analytics-data-lake-output-authorization.png)  
   
2. Data Lake Store'a erişim zaten varsa, "Şimdi Yetkilendir"'i tıklatın ve bir sayfa kısa bir süre için "Yeniden yönlendirmek için yetkilendirme" belirten yukarı pop. Sayfa otomatik olarak kapanır ve Data Lake Store çıkış yapılandırmanıza olanak tanır sayfayla sunulur.

Data Lake Store için kaydolmadıysanız, isteği başlatmak veya izleyin için "şimdi kaydolun" bağlantı izleyebilirsiniz [başlatılan yönergeleri almak](../data-lake-store/data-lake-store-get-started-portal.md).

## <a name="configure-the-data-lake-store-output-properties"></a>Data Lake Store çıktı özelliklerini yapılandırın
Kimliği doğrulanmış Data Lake Store hesabına sahip olduğunda, Data Lake Store çıktı için özelliklerini yapılandırabilirsiniz. Aşağıdaki tablo özellik adları ve Data Lake Store çıkış yapılandırmak için açıklamalarına listesidir.

<table>
<tbody>
<tr>
<td><B>ÖZELLİK ADI</B></td>
<td><B>AÇIKLAMA</B></td>
</tr>
<tr>
<td>Çıkış Diğer Adı</td>
<td>Bu, sorgu çıktısı bu Data Lake Store'a doğrudan sorgularda kullanılan kolay bir addır.</td>
</tr>
<tr>
<td>Data Lake Store hesabı</td>
<td>Burada, Çıkış göndermeyi depolama hesabı adı. Oturum açan kullanıcının erişimi Data Lake Store hesaplarının bir listesiyle birlikte sunulur.</td>
</tr>
<tr>
<td>Yol önek deseni [<I>isteğe bağlı</I>]</td>
<td>Belirtilen veri Gölü deposu hesabı içinde dosyalarınızı yazmak için kullanılan dosya yolu. <BR>{date} {time}<BR>Örnek 1: klasör1/logs / {date} / {time}<BR>Örnek 2: klasör1/logs / {date}</td>
</tr>
<tr>
<td>Tarih biçimi [<I>isteğe bağlı</I>]</td>
<td>Bir tarih belirteci önek yolunda kullanılırsa, dosyalarınızı organize edilmiştir tarih biçimi seçebilirsiniz. Örnek: YYYY/AA/GG</td>
</tr>
<tr>
<td>Saat biçimi [<I>isteğe bağlı</I>]</td>
<td>Zaman belirteci önek yolunda kullanılırsa, dosyalarınızı düzenlenmiş zaman biçimini belirtin. Şu anda desteklenen tek değer HH ' dir.</td>
</tr>
<tr>
<td>Olay Serileştirme Biçimi</td>
<td>Çıkış verileri seri hale getirme biçimi. JSON, CSV ve Avro desteklenir.</td>
</tr>
<tr>
<td>Encoding</td>
<td>Bir kodlama, CSV veya JSON biçiminde, belirtilmiş olması gerekir. Şu anda desteklenen tek kodlama biçimi UTF-8'dir.</td>
</tr>
<tr>
<td>Sınırlayıcı</td>
<td>Yalnızca, CSV serileştirme için de geçerlidir. Akış analizi, CSV verileri seri hale getirme için bir dizi ortak sınırlayıcıları destekler. Virgül, noktalı virgül, boşluk, sekme ve dikey çubuk bunun desteklenen değerlerdir.</td>
</tr>
<tr>
<td>Biçimlendir</td>
<td>Yalnızca JSON serileştirmesi için geçerlidir. Ayrılmış çizgi çıkış sahip yeni bir çizgiyle ayrılmış her bir JSON nesnesi olarak biçimlendirileceğini belirtir. Dizi çıkışı bir dizi JSON nesnesi biçimlendirileceğini belirtir.</td>
</tr>
</tbody>
</table>

## <a name="renew-data-lake-store-authorization"></a>Data Lake Store yetkilendirmeyi yenileyin
Şu anda, burada kimlik doğrulama belirteci 90 günde Data Lake Store çıktıyla tüm işleri el ile yenilenmesi gerekiyor bir sınırlama yoktur. İşinizi oluşturulmuş veya son kimliği doğrulanmış yana parolanızı değiştirdiyseniz Data Lake Store hesabınızı yeniden kimlik doğrulaması gerekecektir. Bu sorun belirtisi hiçbir iş çıktısı ve işlem günlükleri'ni yeniden yetkilendirme gereksinimini belirten bir hata var.

Bu sorunu çözmek için çalışan bir işi durdurmak ve Data Lake Store çıktısına gidin. "Yetkilendirmeyi yenileyin" bağlantısına tıklayın ve bir sayfa kısa bir süre için "Yeniden yönlendirmek için yetkilendirme.." belirten yukarı pop. Sayfa otomatik olarak kapatılacak ve başarılı olursa, "Yetkilendirme başarıyla yenilendi" gösterir. Sonra gerek sayfasının en altında "Kaydet"'i tıklatın ve işinizi zamandan son durduruldu veri kaybını önlemek için yeniden başlatarak devam edebilirsiniz.

![](media/stream-analytics-data-lake-output/stream-analytics-data-lake-output-renew-authorization.png)

