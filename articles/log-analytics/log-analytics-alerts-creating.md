---
title: "OMS günlük analizi uyarıları oluşturma | Microsoft Docs"
description: "Günlük analizi uyarılarını OMS deponuzun önemli bilgileri tanımlamak ve önceden sorunları size bildiren veya düzeltmenize girişiminde Eylemler çağırma.  Bu makalede, bir uyarı kuralı ve ayrıntıları yapabilecekleri farklı eylemler oluşturmayı açıklar."
services: log-analytics
documentationcenter: 
author: bwren
manager: jwhit
editor: tysonn
ms.assetid: 6cfd2a46-b6a2-4f79-a67b-08ce488f9a91
ms.service: log-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/23/2017
ms.author: bwren
ms.openlocfilehash: c34fb7295e8f386f0e7cf2c1db6b26a3e49eae98
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="working-with-alert-rules-in-log-analytics"></a>Günlük analizi uyarı kurallarında ile çalışma
Uyarılar, günlük aramaları düzenli aralıklarla otomatik olarak çalışacak uyarı kuralları tarafından oluşturulur.  Sonuçları belirli ölçütlere uyan varsa bunlar bir uyarı kaydı oluşturun.  Kural, ileriye dönük olarak uyarı bildiren veya başka bir işlem çağırmak için bir veya daha fazla eylemleri otomatik olarak çalıştırabilirsiniz.   

Bu makalede oluşturup OMS Portalı'nı kullanarak uyarı kuralları düzenlemek için işlemleri açıklanır.  Farklı ayarlar ve gerekli mantığı uygulamanız hakkında daha fazla bilgi için bkz [günlük analizi anlama uyarıları](log-analytics-alerts.md).

>[!NOTE]
> Şu anda oluşturamaz veya Azure portalını kullanarak bir uyarı kuralı değiştirin. 

## <a name="create-an-alert-rule"></a>Bir uyarı kuralı oluştur

OMS Portalı'nı kullanarak bir uyarı kuralı oluşturmak için bir günlük arama uyarı çağıracağı kayıtlar için oluşturarak başlayın.  **Uyarı** düğmesini sonra kullanılabilir olacak oluşturmak ve uyarı kuralı yapılandırmak için.

>[!NOTE]
> En fazla 250 uyarı kuralları şu anda bir OMS çalışma alanında oluşturulabilir. 

1. OMS genel bakış sayfasında **günlük arama**.
2. Yeni bir günlük arama sorgusu oluşturun ya da kaydedilmiş günlük Ara'yı seçin. 
3. Tıklatın **uyarı** açmak için sayfanın üstündeki **uyarı kuralı Ekle** ekran.
4. Bilgileri kullanarak uyarı kuralı yapılandırmak [uyarı kuralları ayrıntılarını](#details-of-alert-rules) aşağıda.
6. Tıklatın **kaydetmek** uyarı kuralı tamamlamak için.  Çalıştırdıktan hemen başlar.


## <a name="edit-an-alert-rule"></a>Bir uyarı kuralını Düzenle
Tüm uyarı kuralları listesini almak **uyarıları** günlük analizi menüde **ayarları**.  

![Uyarıları yönetme](./media/log-analytics-alerts/configure.png)

1. OMS konsol seçin **ayarları** döşeme.
2. Seçin **uyarıları**.

Bu görünümden birden çok eylemleri gerçekleştirebilirsiniz.

* Bir kural seçerek devre dışı **devre dışı** yanında.
* Bir uyarı kuralı yanında kalem simgesine tıklayarak düzenleyin.
* Bir uyarı kuralı tıklatarak kaldırın **X** yanındaki simge. 

## <a name="details-of-alert-rules"></a>Uyarı kurallarının ayrıntıları
Oluşturma veya OMS portalında bir uyarı kuralı düzenleme, birlikte çalıştığınız **uyarı kuralı Ekle** veya **uyarı kuralını Düzenle** sayfası.  Aşağıdaki tablolar, bu ekranı alanları açıklamaktadır.

![Uyarı kuralı](media/log-analytics-alerts/add-alert-rule.png)

### <a name="alert-information"></a>Uyarı bilgileri
Bu uyarı kuralı ve oluşturduğu uyarılar için temel ayarlarıdır.

| Özellik | Açıklama |
|:--- |:---|
| Ad | Uyarı kuralını tanımlamak için benzersiz bir ad. Bu ad, kural tarafından oluşturulan uyarılar dahil edilir.  |
| Açıklama | Uyarı kuralı isteğe bağlı bir açıklama. |
| Önem Derecesi |Bu kural tarafından oluşturulan uyarı önem derecesi. |

### <a name="search-query-and-time-window"></a>Arama sorgu ve zaman penceresi
Kayıtları döndürmeyi arama sorgu ve zaman penceresini herhangi bir uyarı oluşturulup oluşturulmayacağını belirlemek için değerlendirilir.

| Özellik | Açıklama |
|:--- |:---|
| Arama sorgusu | Çalıştırılacak sorgu budur.  Bu sorgu tarafından döndürülen kayıtları, bir uyarı oluşturulup oluşturulmayacağını belirlemek için kullanılır.<br><br>Seçin **kullanım geçerli arama sorgusunu** geçerli sorgu kullanma veya varolan kaydedilmiş aramayı listeden seçin.  Sorgu sözdizimi, gerekirse değiştirebileceğiniz metin kutusunda sağlanır. |
| Zaman penceresi |Sorgu için zaman aralığını belirtir.  Sorgu, geçerli zaman aralığında oluşturulan kayıtları döndürür.  Bu, 5 dakika ile 24 saat arasında herhangi bir değer olabilir.  Uyarı sıklığı eşit veya daha büyük olmalıdır.  <br><br> Örneğin, zaman penceresi 60 dakika olarak ayarlanmıştır ve sorgu 13: 15'te çalıştırırsanız, yalnızca saat 12: 15'e ve 13: 15'te arasında oluşturulan kayıtları döndürülür. |

Zaman penceresi için uyarı kuralı belirttiğinizde, bu zaman penceresi için arama ölçütleriyle eşleşen mevcut kayıt sayısı görüntülenir.  Bu, beklediğiniz sonuç sayısını verecektir sıklığı belirlemenize yardımcı olabilir.

### <a name="schedule"></a>Zamanlama
Arama sorgusu ne sıklıkta çalıştırmak tanımlar.

| Özellik | Açıklama |
|:--- |:---|
| Uyarı sıklığı | Sorgunun ne sıklıkta çalıştırılması gerektiğini belirtir. 5 dakika ile 24 saat arasında herhangi bir değer olabilir. Eşit veya bu zaman penceresi'den daha az olmalıdır.  Değeri zaman penceresi'den büyükse, eksik kayıtları riski oluşur.<br><br>Örneğin, 30 dakikalık bir zaman penceresi ve 60 dakika sıklığını göz önünde bulundurun.  Sorgu 1: 00'dan çalıştırırsanız, 12:30 ve 1:00 arasında kayıt döndürür.  Sorguyu çalıştırabilir sonraki 2:00 kayıtlar 1:30 ve 2:00 arasında ne zaman döndürecektir süresidir.  1:00-1:30 arasında oluşturulan kayıtları hiçbir zaman değerlendirilmesi. |


### <a name="generate-alert-based-on"></a>Temel uyarı oluştur
Bir uyarı oluşturulup oluşturulmayacağını belirlemek için arama sorgusunun sonuçları karşı değerlendirilecek ölçütleri tanımlar.  Bu ayrıntılar seçtiğiniz uyarı kuralı türüne bağlı olarak farklı olacaktır.  Farklı uyarı kuralı türlerinden için Ayrıntılar elde edebilirsiniz [günlük analizi anlama uyarıları](log-analytics-alerts.md).

| Özellik | Açıklama |
|:--- |:---|
| Uyarıları bastırma | Uyarı kuralı için gizleme etkinleştirdiğinizde, Eylemler kural için yeni bir uyarı oluşturduktan sonra tanımlı bir süre için devre dışı bırakılır. Kural hala çalışıyor ve ölçütü karşılanırsa uyarı kayıtları oluşturursunuz. Bu yinelenen Eylemler çalıştırmadan sorunu düzeltmek için zaman izin vermektir. |

#### <a name="number-of-results-alert-rules"></a>Sonuçları uyarı kuralları sayısı

| Özellik | Açıklama |
|:--- |:---|
| Sonuç sayısı |Sorgu tarafından döndürülen kayıt sayısını ya da ise bir uyarı oluşturulur **büyük** veya **değerinden** sağladığınız değeri.  |

#### <a name="metric-measurement-alert-rules"></a>Ölçüm ölçüm uyarı kuralları

| Özellik | Açıklama |
|:--- |:---|
| Toplam değeri | Sonuçları toplama her değer bir ihlal olarak kabul edilmesi için aşması gereken Eşiği değeri. |
| Dayalı tetikleyici Uyarısı | Oluşturulacak bir uyarı için ihlal sayısı.  Belirtebilirsiniz **toplam ihlal** ihlallerinden sonuçları arasında herhangi bir birleşimini ayarlamak veya **ardışık ihlallerini** ihlallerini ardışık örnekler içinde gerçekleşmelidir gerektirecek şekilde. |

### <a name="actions"></a>Eylemler
Uyarı kuralları her zaman oluşturacak bir [uyarı kaydı](#alert-records) eşiği karşılanıyorsa zaman.  Ayrıca, bir e-posta gönderme veya bir runbook'u başlatma gibi çalıştırmak için bir veya daha fazla yanıtları tanımlayabilirsiniz.



#### <a name="email-actions"></a>E-posta Eylemler
E-posta Eylemler bir veya daha fazla alıcıya uyarı ayrıntılarını içeren bir e-posta gönderin.

| Özellik | Açıklama |
|:--- |:---|
| E-posta ile bildirim |Belirtin **Evet** uyarı tetiklendiğinde gönderilecek e-posta istiyorsanız. |
| Konu |E-postayla konu.  Posta gövdesini değiştiremezsiniz. |
| Alıcıları |Tüm e-posta alıcıları adresleri.  Birden fazla adres belirtirseniz, adreslerini noktalı virgül (;) ayırın. |

#### <a name="webhook-actions"></a>Web kancası eylemleri
Web kancası eylemleri, bir dış işlem tek bir HTTP POST isteği üzerinden çağırma olanak tanır.

| Özellik | Açıklama |
|:--- |:---|
| Web Kancası |Belirtin **Evet** uyarı tetiklendiğinde bir Web kancası çağrı istiyorsanız. |
| Web kancası URL'si |Web kancası URL'si. |
| Özel JSON yükünü dahil et |Varsayılan yükü ile özel bir yükü değiştirmek istiyorsanız bu seçeneği belirleyin. |
| Özel JSON yükünüzü girin |Web kancası için özel yükü.  Ayrıntılar için önceki bölüme bakın. |

#### <a name="runbook-actions"></a>Runbook eylemleri
Runbook eylemleri, Azure Automation'da bir runbook başlatın. 

>[!NOTE]
> Bu eylemin etkinleştirilmesi çalışma alanınızdaki yüklü Otomasyon çözümünü olması gerekir. 


| Özellik | Açıklama |
|:--- |:---|
| Runbook | Belirtin **Evet** uyarı tetiklendiğinde bir Azure Otomasyonu runbook'u başlatmak istiyorsanız.  |
| Otomasyon hesabı | Runbook'ları seçildiği Otomasyon hesabı belirtir.  Bu çalışma alanı'na bağlı eylem hesabıdır. |
| Bir runbook seçin | Bir uyarı oluşturulduğunda başlatmak istediğiniz runbook'u seçin. |
| Üzerinde çalışır | Seçin **Azure** runbook bulutta çalıştırmak için.  Seçin **karma çalışanı** runbook'u ile bir aracı çalıştırmayı [karma Runbook çalışanı](../automation/automation-hybrid-runbook-worker.md ) yüklü.  |




## <a name="next-steps"></a>Sonraki adımlar
* Yükleme [uyarı yönetimi çözümü](log-analytics-solution-alert-management.md) System Center Operations Manager (SCOM) toplanan uyarılar ile birlikte günlük analizi oluşturulan uyarıların çözümlemek için.
* Daha fazla bilgi edinin [oturum aramaları](log-analytics-log-searches.md) uyarılar oluşturabilir.
* İzlenecek yollar için tamamlamak [bir webook yapılandırma](log-analytics-alerts-webhooks.md) bir uyarı kuralı ile.  
* Nasıl yazılacağını öğrenmek [Azure automation'daki runbook'lar](https://azure.microsoft.com/documentation/services/automation) uyarılar tarafından tanımlanan sorunları düzeltmek için.

