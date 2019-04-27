---
title: Önceden derlenmiş etki alanı başvurusu
titleSuffix: Azure
description: Önceden oluşturulmuş koleksiyonları hedefleri ve varlıkların gelen Language Understanding Intelligent Services (LUIS) önceden oluşturulmuş etki alanları için başvuru.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: article
ms.date: 03/04/2019
ms.author: diberry
ms.openlocfilehash: e1e579233a5ad1af1ef8ee84019cd995959d3b2b
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60712639"
---
# <a name="prebuilt-domain-reference-for-your-luis-app"></a>LUIS uygulamanızı için önceden oluşturulmuş etki alanı başvurusu
Bu başvuru, hakkında bilgi sağlar. [önceden oluşturulmuş etki alanları](luis-how-to-use-prebuilt-domains.md), önceden oluşturulmuş koleksiyon hedefleri ve LUIS sunan varlıkların olduğu.

[Özel etki alanları](luis-how-to-start-new-app.md), aksine, hiçbir hedefleri ve modelleri başlayın. Herhangi bir önceden oluşturulmuş etki alanı hedefleri ve varlıklar için özel bir model ekleyebilirsiniz.

## <a name="list-of-prebuilt-domains"></a>Önceden oluşturulmuş etki alanlarının listesi
LUIS, 20 önceden oluşturulmuş etki alanı sunar. 

| Önceden oluşturulmuş etki alanı | Açıklama | Desteklenen Diller |
| ---------------- |-----------------------|:------:|
| Takvim | Amaç ve varlıkları ekleme, silme veya randevu düzenleme, katılımcı kullanılabilirlik denetimi ve bir takvim etkinliği hakkında bilgi bulma için Takvim etki alanı sağlar.| en-US<br/> zh-CN |
| Kamera | Kamera etki alanı, resim, video kaydetmek ve yayın video, uygulama almak için amaç ve varlıkları sağlar.| en-US |
| İletişim | İleti gönderme ve telefon çağrıları yapma.| en-US <br/> zh-CN |
| Eğlence  | Müzik, film ve TV ile ilgili sorgularını işleme.| en-US |
| Olaylar | Konserlerden, festivallerinden, spor oyunları ve Komedi biletlerini kayıt gösterir.| en-US |
| Uygunluk | Uygunluk etkinliklerini izleme ilgili istekleri işleme.| en-US |
| Oyun | Çok oyunculu oyun, oyun bir taraf ilgili istekleri işleme.| en-US |
| HomeAutomation | Cihazları ve ışıklar gibi Akıllı Giriş cihazları denetleme.| en-US<br/> zh-CN |
| MovieTickets | Bir film tiyatro filmleri bilet kayıt.| en-US |
| Müzik | Müziği bir müzik çalar.| en-US<br/> zh-CN |
| Not | Amaç ve varlıkları oluşturma, düzenleme ve notları bulma ile ilgili not etki alanı sağlar.| en-US<br/> zh-CN |
| OnDevice | Hedefleri ve cihaz denetlemek için ilgili varlıkları OnDevice etki alanı sağlar.| en-US<br/> zh-CN |
| Yerler  | İşletmeler, kurumlar, Restoran, ortak alanları ve adresleri gibi yerlerde ilgili sorgular işleme.| en-US<br/> zh-CN |
| Anımsatıcı | Oluşturma, düzenleme ve anımsatıcılar bulma ile ilgili istekleri işleme.| en-US<br/> zh-CN |
| RestaurantReservation | Restoran ayırmalarını yönetmek için istek işleme.| en-US<br/> zh-CN |
| Taksi | Rezervasyonlar bir taksi için işleme.| en-US<br/> zh-CN |
| Çevirme | Metni bir hedef dile çevirerek.| en-US<br/> zh-CN |
| TV | TV denetleme.| en-US |
| Altyapı Hizmetleri  | "Yardım" gibi birden çok etki alanı ortak olan isteklerin işlenmesinden "yinelemek", "baştan başlayın."| en-US |
| Hava durumu | Hava durumu raporları ve tahminlerini alınıyor.| en-US<br/> zh-CN |
| Web | Bir Web sitesine yönlendirir.| en-US<br/> zh-CN |

Aşağıdaki bölümlerde her etki alanı ile ilgili daha fazla ayrıntı için bkz.

## <a name="calendar"></a>Takvim 

Hedefleri ve takvim girişlerinin ilgili varlıkları Takvim etki alanı sağlar. Takvim hedefleri ekleme, silme veya randevu düzenleme, kullanılabilirliği denetleniyor ve takvim girişi veya randevu hakkında bilgi bulma içerir.

### <a name="intents"></a>Hedefler
| Hedefi adı | Açıklama | Örnekler |
| ---------------- |-----------------------|----|
| Ekle | Yeni bir kerelik öğe takviminize ekleyin.| Pazar günü Pasifik 2 sırasında Lisa ile bir randevu oluşturun <br/><br/>Toplantı zamanlamak istiyorum<br/><br/>Toplantı ayarlama gerekir|
| CheckAvailability | Kullanılabilirlik randevu veya kullanıcının takvim veya başka bir kişinin Takvim toplantı bulun.| Jim karşılamak kullanılabilir olduğunda? <br/><br/>Carol yarın kullanılabilir olduğunda göster<br/><br/>Chris Cumartesi günleri ücretsiz mi?|
| Sil | Bir takvime girişi silme isteği.| Carol ile My randevu iptal edin. <br/><br/>My 09: 00 toplantı Sil<br/>|
| Düzenle | Bir mevcut toplantı veya Takvim Girişi değiştirme isteği.| 10'da My 09: 00 toplantı taşıyın.<br/><br/>Zamanlamam güncelleştirmek istiyorsunuz.<br/><br/>My toplantı Ryan yeniden zamanlayın.|
| Bul | Haftalık takviminizde görüntüler.| Bul diş randevu gözden geçirin. <br/><br/>Takvimi Göster<br/>|

### <a name="entities"></a>Varlıklar
| Varlık adı | Açıklama | Örnekler |
| ---------------- |-----------------------|----|
| Konum | Takvim öğesi, toplantı veya randevu konumu. Adres, şehir ve bölgeler konumları, iyi örneklerdir.| 209 Nashville uygulamaları <br/><br/>897 Krep Merkezi<br/><br/>İş dışındaki vakitlerimi|
| Özne | Bir toplantıda veya randevu başlığı.| Diş'ın randevu <br/><br/>Öğle yemeği Julia ile<br/><br/>Doktor randevu|

## <a name="camera"></a>Kamera 
Hedefleri ve bir kamera kullanımıyla ilgili varlıkları kamera etki alanı sağlar. Intents bir fotoğraf, selfie, ekran görüntüsü veya görüntü yakalama ve video uygulamaya yayınlamak kapsar.

### <a name="intents"></a>Hedefler
| Hedefi adı | Açıklama | Örnekler |
| ---------------- |-----------------------|----|
| CapturePhoto| Fotoğraf yakalayın.| Fotoğraf çek<br/><br/>Yakalama|
| CaptureScreenshot | Bir ekran görüntüsünü yakalayın.| Ekran görüntüsü<br/><br/>Ekran Yakalama.|
| CaptureSelfie | Bir selfie yakalayın.| Özçekim <br/><br/>Bana ait bir resim al |
| CaptureVideo | Video kaydı başlatın.| Kaydı başlat <br/><br/>Kaydı başlatmak|
| StartBroadcasting| Yayın video başlatın.| Facebook yayınlama Başlat|
| StopBroadcasting| Yayın video durdurun.| Yayını Durdur|
| StopVideoRecording| Bir video kaydı durdurun.| Yeterli olan<br/><br/>Kaydı Durdur|

### <a name="entities"></a>Varlıklar
| Varlık adı | Açıklama | Örnekler |
| ---------------- |-----------------------|----|
| AppName | Video yayınlamak için bir uygulama adı.| OneNote<br/><br/>Facebook<br/><br/>Skype|


## <a name="communication"></a>İletişim 
Hedefleri ve e-posta, iletileri ve telefon aramaları ilgili varlıkları iletişimi etki alanı sağlar.

### <a name="intents"></a>Hedefler
| Hedefi adı | Açıklama | Örnekler |
| ---------------- |-----------------------|----|
| AddContact| Yeni bir kişi, kullanıcının kişiler listesine ekleyin.|Yeni Kişi Ekle <br/><br/>Bu sayı kaydedin ve Carol adıyla yerleştirin|
| AddMore| Daha fazla e-posta veya metin tarafınızdaki e-posta veya metin birleşimin parçası ekleyin.|Daha fazla metin Ekle <br/><br/>Daha fazla gövdesi, e-posta Ekle|
| Yanıt| Gelen telefon aramasını yanıtlayın.|Yanıt arama <br/><br/>Bunu seçin|
| AssignContactNickname| Bir takma ad, bir kişi için atayın.|Isaac babanızın için değiştirin <br/>Can'ın takma adı Düzenle<br/>Patti Owens için takma ad ekleyin|
| CallVoiceMail| İçin kullanıcının sesli posta bağlanın.|Benim sesli posta kutuma Bağlan <br/>Sesli posta<br/>Sesli mesaj çağırın|
| CheckIMStatus| Skype kişi durumunu denetleyin.|Can'ın çevrimiçi durumu hemen ayarlanır? <br/>Carol, sohbet etmek kullanılabilir mi?|
| Onayla| Bir eylemi onaylayın.|Evet<br/>Tamam<br/>Anlaşıldı<br/>Bu e-posta göndermek istediğiniz onaylıyorum.<br/>|
| Arama| Telefon araması yapın.|Jim çağırın<br/>Lütfen 311 Çevir<br/>|
| FindContact| Ada göre iletişim bilgilerini bulabilir.|Carol'ın telefon numarası bulun<br/>Carol'ın numarasını göster<br/>|
| FindSpeedDial| Bir telefon numarası için ve tersi ayarlanır hızlı arama numarası bulun.|Arama numaramı 5 nedir?<br/>Kümesi çevirmek hızı var mı?<br/>941 5555 333 arama numaralı nedir?|
| GetForwardingsStatus| Çağrı iletme geçerli durumunu alın.|My çağrı iletme açık mı?<br/>Çağrı Durumum açık veya kapalı olma bildir<br/>|
| GoBack| Önceki adıma geri dönün.|Twitter dönün<br/>Bir adıma geri dönün<br/>Geri git|
| Yoksayma| Gelen bir arama yoksayın.|Yanıt yok<br/>Çağrı yoksay|
| IgnoreWithMessage| Gelen bir arama yoksay ve bunun yerine metin ile yanıtla.|Bu aramayı yanıtlamalı yoktur ancak bir ileti gönderin.<br/>Yoksay ve bir metin geri gönderin.|
| PressKey| Bir düğme veya sayı tuş takımında tuşuna basın.|Arama yıldız.<br/>1 2 3'e basın.|
| ReadAloud| Bir mesaj veya e-posta kullanıcıya okuyun.|Metni okuyun.<br/>Hangi Filiz iletisinde konuları?|
| TurnForwardingOff| Telefon araması yapın.|<br/><br/>|
| Arama| Arayın veya bir sayı yeniden çağırın.|Arayın.<br/>Benim son çağrı arayın.|
| Reddet| Bir gelen çağrıyı reddeder.|Çağrı Reddet<br/>Artık yanıt bulamadığınız<br/>Şu anda kullanılamıyor ve daha sonra geri çağırır.|
| SendEmail| E-posta gönderin. Bu amaç için e-posta ancak metin iletileri için geçerlidir.|Mike sularında için e-posta: Mike, Şimdi Akşam geçen hafta sende çok iyi.<br/>Bob için bir e-posta Gönder<br/>|
| SendMessage| SMS mesajı ya da anlık ileti gönderin.|Chris ve Carol metin Gönder|
| SetSpeedDial| Kişinin telefon numarası için hızlı arama kısayol ayarlayın.|Carol için hızlı arama birini ayarlayın.<br/>Mom için hızlı arama ayarlayın.|
| ShowNext| Sonraki öğeyi, örneğin, bir kısa mesaj veya e-postaları listesinde bakın.|Sonrakini Göster.<br/>Bir sonraki sayfasına gidin.|
| ShowPrevious| Önceki öğeyi, örneğin, bir kısa mesaj veya e-postaları listesinde görürsünüz.|Öncekine gösterir.<br/>Önceki<br/>Önceki gidin.|
| StartOver| Sistem baştan başlayın veya yeni bir oturum başlatın.|Yeniden başlayın<br/>Yeni oturum<br/>restart|
| TurnForwardingOff| Çağrı iletme devre dışı bırakın.|Çağrılarım iletme Durdur<br/>Çağrı iletme geçiş|
| TurnForwardingOn| Konuşmacı telefonu kapatın.|Çağrılarım 3333 için iletme<br/>Çağrı Yönlendirme 3333 üzerinde geçiş|
| TurnSpeakerOff| Konuşmacı telefonu kapatın.|Bana hoparlör kapalı yararlanın.<br/>Hoparlör devre dışı bırakın.<br/>|
| TurnSpeakerOn| Konuşmacı telefonda açın.|Hoparlör modu.<br/>Hoparlör koyabilirsiniz.<br/>|

### <a name="entities"></a>Varlıklar
| Varlık adı | Açıklama | Örnekler |
| ---------------- |-----------------------|----|
| AudioDeviceType | Ses cihazı (Konuşmacı, kulaklık, mikrofon, vb.) türü.| Konuşmacı<br/>Kendiliğinden<br/>Bluetooth|
| Kategori | Bir mesaj veya e-posta kategorisidir.| Önemli<br/>Yüksek öncelikli|
| ContactAttribute | Kişinin kullanıcı hakkında vazgeçilmez bir öznitelik.| Doğum günü bilgilerine<br/>Adres<br/>Telefon numarası|
| ContactName | Bir kişi veya ileti alıcı adı.| Carol<br/>Jim<br/>Chris|
| EmailSubject | E-posta konusu kullanılan metin.| RE: ilgi çekici hikayesi|
| Çizgi | Kullanıcının satırı çağrı yapmak veya bir metin/e-postadan göndermek için kullanmak istiyor.| İş satır<br/>İngiliz hücresi<br/>Skype|
| İleti | Bir e-posta veya metin olarak gönderilecek ileti.| Bugün toplantı harika. En kısa zamanda tekrar görün!|
| MessageType | Bir kişi veya ileti alıcı adı.| Metin<br/>Email|
| OrderReference | Listesini almak için bir öğeyi tanımlamak, sıralı ya da göreli konumu. Örneğin, "last" veya "son" içinde "I gönderilen son ileti neydi?"| Son<br/>En Son|
| Gönderenin adı | Gönderen adı.| Patti Owens|

## <a name="entertainment"></a>Eğlence  
Etki alanı hedefleri ve filmler, müzik, oyunlar ve TV aramayla ilgili varlıkları sağlar eğlence gösterir.

### <a name="intents"></a>Hedefler
| Hedefi adı | Açıklama | Örnekler |
| ---------------- |-----------------------|----|
| Arama| Arama filmler, müzik, uygulamalar, oyunlar ve TV için gösterilmektedir.|Halo mağazada arayın.<br/>Avatar arayın.|

### <a name="entities"></a>Varlıklar
| Varlık adı | Açıklama | Örnekler |
| ---------------- |-----------------------|----|
| ContentRating | Medya, film derecelendirmesi G veya R gibi içerik.|Video çocuk.<br/>PG derecelendirilir.|
| Tarzı | Film, oyun, uygulama veya şarkı Tarz.|Comedies<br/>Dramas<br/>Komik|
| Anahtar sözcüğü| Bir genel arama anahtar sözcüğü bir öznitelik belirtmemeye daha belirli ortam yuvalarda yok.|Parçalar<br/>Ay Irmağı<br/>Amelia Earhart|
| Dil | Ortamda, film veya şarkı konuşulan dili gibi kullanılan dil.|Fransızca <br/>Türkçe<br/>Korece|
| MediaFormat | İçinde medya biçimlendirilmiş ek özel teknik türü.|HD filmler<br/>3B filmler<br/>İndirilebilir|
| MediaSource | Depolama veya medya almak için Market.|Netflix<br/>Asal|
| MediaSubTypes| Medya türleri filmler ve oyunlar küçük.|Tanıtımlar<br/>DLC<br/>Tanıtımları|
| Uyruğu| Bir film, Göster ya da şarkı oluşturulduğu ülke.|Fransızca <br/>Almanca <br/>Korece|
| Kişi| Aktör, Müdür, üretici, müzisyen veya sanatçı bir film, uygulamayı, oyunu veya TV programı ilişkili.|Madonna<br/>Stanley Kubrick|
| Rol| Medya oluşturma kişinin oynadığı rolü.|İmzalar<br/>Yöneten<br/>Tarafından|
| Unvan| Bir film, uygulama, oyun, TV programı veya Şarkı adı.|Arkadaş<br/>Minecraft|
| Tür| Bir film, uygulama, oyun, TV programı veya şarkı türü veya medya biçimi.|Müzik<br/>MovieTV <br/>Gösterir|
| UserRating| Veya kullanıcı yıldız derecelendirmesi thumbs.|5 yıldız<br/>3 yıldız<br/>4 yıldız|

## <a name="events"></a>Olaylar 
Varlıkları ayırtmak konserlerden, festivallerinden, spor oyunları ve Komedi gibi olayları gösterir ilgili ve hedefleri olayları etki alanı sağlar.

### <a name="intents"></a>Hedefler
| Hedefi adı | Açıklama | Örnekler |
| ---------------- |-----------------------|----|
| Book| Bir olay için bilet satın alın.|Bu hafta symphony için bilet satın almak istiyorum.|


### <a name="entities"></a>Varlıklar
| Varlık adı | Açıklama | Örnekler |
| ---------------- |-----------------------|----|
| Adres | Olay konumu veya adresi. |Palo Alto<br/>300 112th Ave SE <br/> Seattle |
| Ad | Bir olayın adı.|Park içinde Shakespeare|
| PlaceName| Olay konum adı.|Louvre<br/>Opera Merkezi<br/>Broadway|
| PlaceType | Konum türü içinde olay gerçekleştirilecektir.|Cafe<br/>Tiyatro<br/>Kitaplık|
| Tür | Bir olay türü.|Konser<br/>Spor oyunu|

## <a name="fitness"></a>Uygunluk 
Hedefleri ve uygunluk etkinliklerini izleme ilgili varlıkları uygunluk etki alanı sağlar. Hedefleri, saati veya uzaklığı kalan veya etkinlik sonuçları kaydedilirken not kaydetme içerir.

### <a name="intents"></a>Hedefler
| Hedefi adı | Açıklama | Örnekler |
| ---------------- |-----------------------|----|
| AddNote| Ek notlar için izlenen bir etkinliği ekler.|Bu çalıştırmanın zorluk 6/10 idi<br/>Çalışan üzerinde ben Arazi asfalt olduğu<br/>3 hızı bisiklet kullanıyorum|
|GetRemaining| Kalan saati veya uzaklığı için bir etkinliği alır.|Ne kadar süre sonraki lap kasa?<br/>My çalıştırmasında kaç mil kalıyor? Bölme için ne kadar süre?|
| LogActivity| Tamamlanan etkinlik sonuçları günlüğü ya da kaydedin.|Son çalışma Alanım Kaydet<br/>My Cumartesi sabahı Yürüme oturum<br/>My önceki yüzme depolayın|
| LogWeight| Kullanıcının geçerli ağırlık oturum ya da kaydedin.|My geçerli ağırlık Kaydet<br/>My ağırlık şimdi oturum açın<br/>My geçerli gövdesi ağırlık depolayın|

### <a name="entities"></a>Varlıklar
| Varlık adı | Açıklama | Örnekler |
| ---------------- |-----------------------|----|
| ActivityType | İzleyen etkinlik türü. |Çalıştırın<br/>Yürüme<br/>Yüzme<br/>Döngüsü |
| Yiyecek | Gıda uygunluk uygulamada izlemek için bir tür. |Muz<br/>Somon<br/>Protein sallama|
| MealType| Bir sistem durumu veya uygunluk uygulamasında izlemek için bir paket türü.|Kahvaltı<br/>Akşam Yemeği<br/>Öğle yemeği<br/>Supper|
| Ölçüm| Ölçümleri zaman, uzaklığı veya ağırlık, uygunluk veya sistem durumu uygulamada kullanılmak için bir tür.|Kilometre<br/>Mil<br/>Dakika<br/>Kilogram|
| Sayı | Uygunluk veya sistem durumu bir uygulamada kullanmak için sayısal bir miktar.|19<br/>üç<br/>200<br/>bir|
| StatType | Uygunluk veya sistem durumu uygulamada kullanılmak toplanmış veriler üzerinde bir istatistik türü.|Toplam<br/>Ortalama<br/>Maksimum<br/>Minimum|

## <a name="gaming"></a>Oyun 
Hedefleri ve çok oyunculu oyun, oyun bir taraf yönetmeyle ilgili varlıkları oyun etki alanı sağlar.

### <a name="intents"></a>Hedefler
| Hedefi adı | Açıklama | Örnekler |
| ---------------- |-----------------------|----|
| InviteParty| Bir kişi bir oyun taraf katılmaya davet ediyoruz.|Bu benim taraf Player'a davet edin<br/>My tarafa gelir<br/>My clan katılın|
|LeaveParty| Kalan saati veya uzaklığı için bir etkinliği alır.|Çıkış istiyorum<br/>Bu taraf için başka bir bırakarak<br/>Çıkma|
| StartParty| Çok oyunculu oyun oyun taraf başlatın.|Şimdi Şaka bir taraf Başlat<br/>bir taraf Başlat<br/>bir clan yemeğinde başlamanız gerekir|

### <a name="entities"></a>Varlıklar
| Varlık adı | Açıklama | Örnekler |
| ---------------- |-----------------------|----|
| İletişim| Çok oyunculu oyun kullanmak için bir kişi adı.|Carol<br/>Jim|


## <a name="homeautomation"></a>HomeAutomation 
Hedefleri ve ışıklar ve cihazları gibi Akıllı Giriş cihazları denetlemek için ilgili varlıkları HomeAutomation etki alanı sağlar.

### <a name="intents"></a>Hedefler
| Hedefi adı | Açıklama | Örnekler |
| ---------------- |-----------------------|----|
| Kapatma| Devre dışı bırakmak, kapatma veya bir cihazın kilidini açmak.|Işıkları aç<br/>Kahve Oluşturucu Durdur<br/>Kapat Garaj kapısının|
|Aç| Bir cihazda etkinleştirme veya belirli bir ayarı veya mod için cihazı ayarlama.|My Kahve Makinesi üzerinde Aç<br/>my Kahve Makinesi üzerinde kapatabilir miyim?<br/>Thermostat 72 derece ayarlayın.|


### <a name="entities"></a>Varlıklar
| Varlık adı | Açıklama | Örnekler |
| ---------------- |-----------------------|----|
| Cihaz | Açılıp kapatılabilir cihaz türü.|Kahve Oluşturucu<br/>Thermostat<br/>ışıklar|
| İşlem | Cihaz durumu.|Kilit<br/>açık<br/>açık<br/>kapalı|
| Oda | Konuma veya cihaz yer.|Oturma odası<br/>Yatak odası<br/>mutfak|

## <a name="movietickets"></a>MovieTickets 
Hedefleri ve ayırtmak için film tiyatro filmleri ilgili varlıkları MovieTickets etki alanı sağlar.

### <a name="examples"></a>Örnekler

|Örnekler|
|--|
|Bana iki biletleri lideri Omar ve iki Musketeers rezervasyonu|
|Anahtarları iptal et|
|Ne zaman lideri Omar gösteriliyor?|

### <a name="intents"></a>Hedefler
| Hedefi adı | Açıklama | Örnekler |
| ---------------- |-----------------------|----|
| Book | Film biletleri satın alın.|Bana iki biletleri lideri Omar ve iki musketeers rezervasyonu<br/>Yarının film için bilet satın almak istiyorum<br/>Bir bilet lideri Omar Kısım 2 için sonraki Çarşamba istiyorum|
|GetShowTime| Showtime filmin alın.|Ne zaman lideri Omar gösteriliyor?|


### <a name="entities"></a>Varlıklar
| Varlık adı | Açıklama | Örnekler |
| ---------------- |-----------------------|----|
| Adres | Bir film tiyatro adresi.|Palo Alto<br/>300 112th Ave SE<br/>Seattle|
| MovieTitle | Bir filmi başlığı.|Pi ömrü<br/>Hunger oyunlar<br/>Başlangıcı|
| PlaceName | Bir film tiyatro ya da film adı.|Sinema Amir<br/>Alexandria tiyatro<br/>New York tiyatro|
| PlaceType | Film adresindeki gösteren konum türü.|sinema<br/>tiyatro<br/>IMAX sinema|

## <a name="music"></a>Müzik 
Hedefleri ve bir müzik çalar müziği için ilgili varlıkları müzik etki alanı sağlar.

### <a name="examples"></a>Örnekler

|Örnekler|
|--|
|Beethoven Yürüt|
|İzleme birim artırın|
|Sonraki şarkıya atla|

### <a name="intents"></a>Hedefler
| Hedefi adı | Açıklama | Örnekler |
| ---------------- |-----------------------|----|
| DecreaseVolume | Cihaz birimin azaltın.|azaltmak İzle<br/>birim aşağı|
| IncreaseVolume | Cihaz birimin artırın.|İzleme birim artırın<br/>birimi|
| Sesi Kapat |Oyun müzik sessiz.|Sesi kapa şarkı<br/>Sessiz kanalında yerleştirin<br/>Sesi kapa müzik |
| Duraklat | Oyun müzik duraklatın.|Duraklat<br/>Duraklatma müzik<br/>Duraklatma İzle|
| PlayMusic | Bir cihazda müzik.|Kevin Durant Yürüt<br/>Coldplay tarafından Paradise Yürüt<br/>Adele tarafından Hello Yürüt|
| Yinele |Oyun müzik yineleyin.|Şarkı yineleyin<br/>İzleme kazanç Yürüt<br/>Müzik yineleyin|
| Sürdür | Oyun müzik sürdürün.|Şarkı Sürdür<br/>Müzik yeniden Başlat<br/>Devam ettirmek|
| SkipBack | Geriye dönük bir parça atlayın.|Sonraki şarkıya atla<br/>Bir sonraki şarkı Yürüt|
| SkipForward |İleriye doğru bir parça atlayın.|Önceki şarkıyı Yürüt<br/>Önceki izlemek için geri dönün |
| Durdur | Müzik kayıttan yürütmek üzere ilgili eylem durdurun. |Bu albüm yürütmeyi durdurur.|
| Sesi Aç | Müzik kayıttan yürütme cihaz sesi Aç.| Sesi Aç.|

### <a name="entities"></a>Varlıklar
| Varlık adı | Açıklama | Örnekler |
| ---------------- |-----------------------|----|
| ArtistName | Aktör, Müdür, üretici, yazıcı, müzisyen veya sanatçı bir cihazda medya ile ilişkili.|Elvis Presley<br/>Taylor Swift<br/>Adele<br/>Mozart|
| Tarzı | İstenen müzik Tarz.|Ülke müzik<br/>Broadway klasikler<br/>Klasik müzik Baroque dönemden Yürüt|

## <a name="note"></a>Not 
Amaç ve varlıkları oluşturma, düzenleme ve notları bulma ile ilgili not etki alanı sağlar.

### <a name="examples"></a>Örnekler

|Örnekler|
|--|
|My Market Not lettuce merak ekmek kahve Ekle|
|My Market listeden bananas kapalı denetleyin|
|Tatil listemdeki tüm öğeleri Kaldır|

### <a name="intents"></a>Hedefler
| Hedefi adı | Açıklama | Örnekler |
| ---------------- |-----------------------|----|
| AddToNote | Bilgi nota ekleme.|My Market Not lettuce merak ekmek kahve Ekle<br/>Yapılacaklar listem Ekle<br/>my Wunderlist'e leziz çörekler Ekle|
| CheckOffItem | Önceden var olan bir not öğelerinden kapalı denetleyin.|My Market listeden bananas kapalı denetleyin<br/>Liste bitti olarak alışveriş my tatile peynirlerine ayırıyor pasta işaretle|
| Temizle | Tüm öğeleri önceden varolan bir notun temizleyin.|Tatil listemdeki tüm öğeleri Kaldır<br/>Tüm okuma listem Temizle|
| Onayla | Not ile ilgili bir eylemi onaylayın.|Benim tarafımdan uygundur<br/>evet<br/>Tüm öğeleri listelerde tutma onaylayan|
| Oluştur | Yeni bir not oluşturun. | Liste oluştur<br/>Bu Jason hatırlat için Mayıs ilk haftası piyasada olduğuna dikkat edin|
| Sil | Tüm bir not silin. |Tatil listem Sil <br/>My Market Notu Sil|
| DeleteNoteItem | Öğeleri önceden varolan bir nottan silin.| Market listemdeki yongaları Sil<br/>Liste alışveriş okulumun kalemler Kaldır|
| ReadAloud | Bir liste sesli okuyun.|Beni ilk oku<br/>Bana ayrıntıları okuyun|
| ShowNext | Notları listedeki sonraki öğeye bakın.|Sonrakini Göster<br/>Sonraki sayfa<br/>Sonraki|

### <a name="entities"></a>Varlıklar
| Varlık adı | Açıklama | Örnekler |
| ---------------- |-----------------------|----|
| AppName | Not Alma uygulamasını adı.|Wunderlist<br/>OneNote|
| ContactName | Not ilgili kişi adı.|Carol<br/>Jim<br/>Chris|
| Veri kaynağı | Notları konumu.|OneDrive<br/>Google docs<br/>Bilgisayarım|
| DataType | Dosya veya genellikle belirli yazılımlar ile ilişkili belge türü.|Slayt<br/>Elektronik tablo<br/>Çalışma sayfası|
| Metin | Not veya anımsatıcı metni.|Yürüyen önce Uzat<br/>uzun süre çalışan yarın|
| Unvan | Not ın başlığı.|Market<br/>çağrılacak kişiler<br/>Yapılacaklar|

## <a name="ondevice"></a>OnDevice 
Hedefleri ve cihaz denetlemek için ilgili varlıkları OnDevice etki alanı sağlar.

### <a name="examples"></a>Örnekler

|Örnekler|
|--|
|video oynatıcı kapatın|
|Kayıttan yürütme iptal et|
|Ekran rengin daha açık hale getirebilirsiniz?|


### <a name="intents"></a>Hedefler
| Hedefi adı | Açıklama | Örnekler |
| ---------------- |-----------------------|----|
| AreYouListening | Cihaz dinleyip dinlemediğini isteyin.|Bu, üzerinde mi?<br/>dinliyoruz?|
|CloseApplication|Cihaz uygulamasını kapatın.|video oynatıcı kapatın|
|FileBug|Cihazda hata bildirin.|Lütfen hata bildirin<br/>Bir hata için bir benioku dosyası?<br/>Bu hatayı bildirmek istiyorum|
|GoBack|Bir adıma geri dönün veya önceki adıma dönmek için isteyin.|Lütfen geri dönün<br/>Önceki ekrana gidin<br/>Git geri stop dinleme|
|Yardım| Yardım isteyin.|Lütfen yardımcı olun<br/>Merhaba<br/>Ne yapabilirsiniz?<br/>Yardım almam gerekiyor| 
|LocateDevice|Cihaz bulun.|Telefonumu bulabilirsiniz<br/>Tom'ın iphone'u Bul<br/>Telefonumu Bul|
|Oturum açma|Cihaz kullanarak bir hizmet için oturum açın.|Oturum açma Lütfen<br/>Facebook oturum açma<br/>LinkedIn oturum|
|Oturum kapatma|Cihaz kullanarak bir service oturumlarını kapatmalıdır.|Telefonumu oturum<br/>Twitter oturum açın<br/>Oturumu kapat|
|MainMenu|Bir cihazın ana menü görüntüleyin.|Görünüm menüsü.|
|OpenApplication|Cihazdaki bir uygulama açın.|Lütfen uyarı açın<br/>Kamerayı açın<br/>Takvim başlatın|
|OpenSetting|Bir aygıt ayarı açın.|Ağ Ayarları'nı açın.|
|PairDevice|Cihaz çifti.|Bluetooth sinyal telefona eşleştirme bana yardımcı olabilir<br/>Bluetooth'u açmasına ve dizüstü ile eşleştirin<br/>Dizüstü bilgisayarımda kayıtlı çifti Bluetooth sinyali|
|Kopyalanabilmesi | Cihaz devre dışı bırakın.|My Bilgisayarı Kapat<br/>Kapat<br/>My mobil Kapat|
|QueryBattery|Pil ömrü hakkında bilgi alın.|Pil ömrünü göster.<br/>Pil Durumum nedir<br/>Şimdi ne kadar pil kaldı?<br/>Pil Göster|
|QueryWifi|WiFi hakkında bilgi alın.|WiFi bilgi alın.|
|Yeniden Başlatma|Cihazı yeniden başlatın.|Lütfen yeniden başlatın.|
|RingDevice| Halka, cihaza, kaybetti gönderilirken için isteyin.|Telefonumu halka.| 
|SetBrightness|Cihaz parlaklık ayarlayın.|Ayarlama brightness Orta<br/>Ayarlama brightness yüksek<br/>Ayarlama brightness düşük|
|SetupDevice|Cihaz kurulumu başlatın.|İşletim sistemi kurulumu yüklemek istiyorum<br/>Lütfen kurulum<br/>Benim için kurulumu yapın|
|ShowAppBar|Bir uygulama çubuğunu göster.|Uygulama çubuğunu göster<br/>Uygulama çubuğu Lütfen<br/>Uygulama çubuğunu görmek istiyorum|
|ShowContextMenu|Bir bağlam menüsünü görüntüleyin.|Bağlam menüsünü görmek istiyorum<br/>Bağlam menüsü Lütfen<br/>Bana bağlam menüsü gösterebilirsiniz|
|Uyku|Uyku moduna cihaz yerleştirin.|Uyu<br/>Uyku<br/>My bilgisayar uyku modunda|
|SwitchApplication|Cihazda kullanmak için uygulamayı geçin.|My media player ile geçiş yapın.|
|TurnDownBrightness|Cihaz parlaklık bırakın.|Ekran karartma.|
|TurnOffSetting|Cihaz ayarı devre dışı bırakın.|Bluetooth devre dışı bırak<br/>Veri devre dışı bırak<br/>Bluetooth bağlantısını kes|
|TurnOnSetting|Bir aygıt ayarı etkinleştirin.|Açık <br/> Kapalı|
|TurnUpBrightness|Brightness cihaz açın.|Ekran rengin daha açık hale getirebilirsiniz?|

### <a name="entities"></a>Varlıklar
| Varlık adı | Açıklama | Örnekler |
| ---------------- |-----------------------|----|
| AppName | Cihazda bir uygulamanın adı.|SoundCloud<br/>YouTube|
| BrightnessLevel | Cihazda parlaklık düzeyini ayarlayın.|Yüzde yüz<br/>Elli<br/>40%|
| ContactName | Cihazda bir kişinin adı.|Paul<br/>Marlen Maks|
| deviceType | Cihaz türü. |Telefon<br/>Kindle<br/>Dizüstü bilgisayar|
| mediaType | Cihaz tarafından işlenen medya türü.|Müzik<br/>Film<br/>TV programları|
| SettingType | Ayar veya düzenlemek için kullanıcının istediği ayarlar paneli türü.|WiFi<br/>Kablosuz ağ<br/>Renk şeması<br/>Bildirim Merkezi|

## <a name="places"></a>Yerler  
Basamak etki alanı, işletmelerin, kurum, Restoran, ortak alanları ve adresleri gibi yerlerde ilgili sorguları işlemek için hedefleri sağlar.

### <a name="examples"></a>Örnekler

|Örnekler|
|--|
|Bu konumda sık kullanılanlara Kaydet|
|Tatil Inn nasıl uzakta mi?|
|Ne zaman Safeway kapatmak?|


### <a name="intents"></a>Hedefler
| Hedefi adı | Açıklama | Örnekler |
| ---------------- |-----------------------|----|
| AddFavoritePlace | Konum, kullanıcının Sık Kullanılanlar listesine ekleyin.|Bu konumda sık kullanılanlara Kaydet<br/>Bu adres Sık Kullanılanlara Ekle|
|CheckAccident|Belirtilen yolda bir yanlışlık olduğunu isteyin.|Bir yanlışlık üzerinde 880 mı?<br/>Yanlışlıkla bilgi göster|
|CheckAreaTraffic|Genel alan veya belirtilen bir rotaya göre değil, Otoyol trafiği denetleyin.|Seattle trafiği<br/>Seattle gibi trafiği nedir?|
|CheckIntoPlace|Sosyal medya kullanarak bir yere iade edin.|Bana Foursquare üzerinde iade et<br/>Buraya bakın|
|CheckRouteTraffic| Kullanıcı tarafından belirtilen belirli bir yol trafiği denetleyin.|Nasıl Mashiko trafiği mi?<br/>Sağlayan: Kirkland trafiği Göster<br/>Seattle trafiği nasıl mi?| 
|Onayla|Bir yere ilgili eylemi onaylayın.|Restoran ayırmamın onaylayın.|
|Çıkış|Bir görev bir yere ilgili çıkmak için eylem.|Lütfen çıkın<br/>Bana yönergeleri veren Çık|
|FindPlace|Bir yer (iş, kurum, Restoran, ortak alan, adresi) arayın.|En yakın kitaplık nerede?<br/>Bana bir iyi İtalyanca Restoran Sıradağlar Görünümü'nde bulun|
|GetAddress| Bir yer adresi isteyin.|Robson sokakta Guu adresi Göster<br/>En yakın kahve adresini nedir?| 
|GetDistance|Belirli bir yere uzaklık hakkında isteyin.|Tatil Inn nasıl uzakta mi?<br/>Buradan Bellevue kare için ne kadar<br/>uzaklık Tahoe nedir|
|GetHours|Bir alan için çalışma saatleri hakkında isteyin.|Ne zaman Safeway kapatmak?<br/>Giriş Depot saatliğine nelerdir?<br/>Kahve hala açık mı?|
|GetMenu|Bir Restoran için menü öğeleri için isteyin.|Herhangi bir şey Zucca sunmuyor vegan?<br/>Sizzler menüde nedir<br/>Applebee'nın menüsünü göster|
|GetPhoneNumber| Bir yer için telefon numarası isteyin.|En yakın kahve telefon numarasını nedir?<br/>Sayı giriş deposu için verin| 
|GetPriceRange| Bir yer fiyat aralığının ister.|Zucca ucuz mi?<br/>Cineplex yarı fiyatına Çarşamba günleri mi?<br/>Ne kadar Sizzler lobster tüm Akşam Yemeği maliyetle?|
|GetReviews|Bir yerde gözden geçirmeler için isteyin.|Cheesecake fabrikası için gözden geçirmeleri Göster<br/>Yelp Cineplex incelemelerde okuyun|
|GetRoute|Bir yere yönergeleri isteyin.|Bellevue kare yürütmek nasıl<br/>Buradan 8 ve 59th en kısa yolu Göster<br/>Bana Sıradağlar görünümü CA'ya yönergeleri alın|
|GetStarRating|Bir yer için yıldız derecelendirmesi isteyin.|Nasıl Zucca Yelp göre derecelendirilmiştir?<br/>Kaç yıldızı Fransızca Çamaşırlar var mı?<br/>Aquarium Monterrey içinde iyi mi?|
|GetTransportationSchedule|Veri yolu zamanlama için bir yer alın.|Ne zaman şehir merkezinde sonraki yoluna nedir?<br/>İçinde King County yollarına Göster|
|GetTravelTime|Seyahat süresini belirtilen bir hedefe isteyin.|Ne kadar süre San Francisco için buradan almak için sürer<br/>Sürüş süresi için Denver SF nedir|
|MakeCall|Telefon görüşmesi için bir yer.|MOM çağırın<br/>Anna'nın Skype çağrısı yapmak istiyorum<br/>Jim çağırın|
|MakeReservation|Bir restoran veya diğer iş için bir ayırma isteği.|İki tonight için Zucca ayırın<br/>Kitap yarın için bir tablo<br/>Tablo için 8 Palo Alto 3.|
|MapQuestions|Yönergeleri veya olup belirtilen yol bir hedefe giden hakkında daha fazla bilgi isteyin.|13 şehir merkezi geçirin mu?<br/>İçin Oakland 880 yararlanabilir miyim?|
|Derecelendirme|Bir restoran veya yer derecelendirmesi açıklamasını alın.|Kaç yıldızı Contoso Inn var mı?|
|ReadAloud|Basamak listesini sesli okuyun.|Beni ilk oku<br/>Bana ayrıntıları okuyun|
|SelectItem|Bir alan veya yer ile ilgili seçimler listesinden bir öğe seçin.|İkinci seçin<br/>İlk seçin|
|ShowMap|Bir alan haritasını göster.|İkincisi için bir haritasını Göster<br/>Haritayı göster<br/>Harita üzerinde San Francisco Bul|
|ShowNext|Bir dizinin sonraki öğeyi gösterir.|Sonrakini Göster<br/>Sonraki Sayfaya Git|
|ShowPrevious|Önceki öğeyle bir dizide gösterir.|Öncekine Göster<br/>önceki<br/>Öncekine git|
|StartOver|Uygulamayı yeniden başlatın veya yeni bir oturum başlatın.|Yeniden başlayın<br/>Yeni oturum<br/>
restart|
|TakesReservations|Bir yer rezervasyonları kabul edip etmeyeceğini isteyin.|Sanat galerisini ayırmalarını kabul ediyor mu<br/>Ayırma sırasında Zeytin bahçesi yapmak mümkündür

### <a name="entities"></a>Varlıklar
| Varlık adı | Açıklama | Örnekler |
| ---------------- |-----------------------|----|
| AbsoluteLocation | Konumu veya bir yerde adresidir.|Palo Alto<br/>300 112th Ave SE<br/>Seattle|
| Kullanılmıyordu | Bir yer hedefi özellikleri/avantajları.|Çocuklar ücretsiz yemek<br/>rıhtımının<br/>Park ücretsiz|
| Atmosfer | Atmosfer bir yer.|Çocuklara yönelik<br/>sıradan bir restoran<br/>sporty|
| Cuisine | Cuisine bir yer. |Akdeniz<br/>İtalyanca<br/>Hindistan|
| DestinationAddress| Bir hedef konum veya adresi.|Palo Alto<br/>300 112th Ave SE<br/>Seattle|
| DestinationPlaceName| Bir iş, Restoran, ortak bir çekim veya kurum bir hedef adı.|Orta parka<br/>Safeway<br/>Walmart|
| DestinationPlaceType | Yerel işletme, Restoran, ortak bir çekim veya kurum bir hedef türü. |Restoran<br/>Opera<br/>sinema|
| Uzaklık | Bir yer uzaklık.|15 mil<br/>5 mili<br/>10 mil uzaklıkta|
| MealType | Kahvaltı veya öğle yemeği türü. |Kahvaltı<br/>Akşam Yemeği<br/>Öğle yemeği<br/>Supper|
| OpenStatus | Bir yer açık veya kapalı olup olmadığını belirtir.|Açık<br/>Kapalı<br/>Açma|
| PlaceName | Bir alan adı.|Cheesecake Fabrika|
| PlaceType | Bir alan türü.|Cafe<br/>Tiyatro<br/>Kitaplık|
| PreferredRoute | Kullanıcı tarafından belirtilen tercih edilen yol. | 101 <br/>202 <br/>Rota 401|
| Ürün | Bir yer tarafından sunulan ürün. | Giyim<br/>ASR dijital kameraları<br/>Yeni balık | 
| PublicTransportationRoute | Kullanıcı arıyor taşımacılık rotanın adı. | Kuzey Doğu corridor eğitme<br/>Veri yolu rota 3 X |
| Derecelendirme | Yer derecelendirmesi. | 5 yıldız<br/>3 yıldız<br/>4 yıldız|
| RouteAvoidanceCriteria | Kazalar, yapılarını veya Ücretli geçişler önleme gibi belirli yollar önleme ölçütleri | Ücretli geçişler <br/>Yapılarını<br/>Rota 11|
| ServiceProvided | Bu bir işletmeyi ya da yöneticinize gibi bir yerde tarafından sağlanan hizmetidir plowing, peyzaj kar. | yöneticinize<br/>Teknisyenin<br/>plumber|
| TransportationCompany | Aktarım sağlayıcısının adı.|Amtrak<br/>Acela<br/>Greyhound|
| TransportationType | Taşıma türü.|Veri yolu<br/>Eğitim<br/>Sürüş|

## <a name="reminder"></a>Anımsatıcı 
Anımsatıcı etki alanı amaç ve varlıkları oluşturma, düzenleme ve bulma anımsatıcılar sağlar.

### <a name="examples"></a>Örnekler

|Örnekler|
|--|
|My röportajı yarın 09: 00 için değiştirin|
|Yol giriş geri üzerinde sütlü satın hatırlat|
|Anımsatıcıyı Christine'nın Doğum günü hakkında sorularım varsa kontrol edebilirim?|


### <a name="intents"></a>Hedefler
| Hedefi adı | Açıklama | Örnekler |
| ---------------- |-----------------------|----|
| Değiştir| Anımsatıcı değiştirin.|My röportajı yarın 09: 00 için değiştirin<br/>Yarın için benim atama anımsatıcı Taşı|
| Oluştur| Yeni bir anımsatıcı oluşturun.|Anımsatıcı oluşturma<br/>Sütlü satın hatırlat<br/>Evde yapmazsam Rebecca çağrılacak unutmayın istiyorum|
| Sil | Anımsatıcıyı Sil.|Benim bir resmi anımsatıcı Sil<br/>Bu anımsatıcı Sil|
| Bul | Anımsatıcı bulun.|Anımsatıcı my Yıldönümü hakkında sahip?<br/>Anımsatıcıyı Christine'nın Doğum günü hakkında sorularım varsa kontrol edebilirim?|

### <a name="entities"></a>Varlıklar
| Varlık adı | Açıklama | Örnekler |
| ---------------- |-----------------------|----|
| Metin | Anımsatıcı metin açıklaması.|kuru temizleme seçin<br/>My araba hizmeti Merkezi'nde bırakmayı devre dışı|

## <a name="restaurantreservation"></a>RestaurantReservation 
Hedefleri ve Restoran ayırmaları yönetmeyle ilgili varlıkları RestaurantReservation etki alanı sağlar.

### <a name="examples"></a>Örnekler

|Örnekler|
|--|
|İki tonight için Zucca ayırın|
|Kitap yarın için BJ'ın, bir tablo|
|Tablo 7, 3 Palo Alto'için|

### <a name="intents"></a>Hedefler
| Hedefi adı | Açıklama | Örnekler |
| ---------------- |-----------------------|----|
| ayırma | Bir Restoran için bir ayırma isteği. |İki tonight için Zucca ayırın<br/>Kitap yarın için bir tablo<br/>Tablo 7, 3 Palo Alto'için|

### <a name="entities"></a>Varlıklar
| Varlık adı | Açıklama | Örnekler |
| ---------------- |-----------------------|----|
| Adres| Olay konumu veya bir ayırma adresi.|Palo Alto<br/>300 112th Ave SE<br/>Seattle|
| Kullanılmıyordu | Bir yer kullanılmıyordu açıklayan öznitelik.|Okyanusu görüntüle<br/>olmayan İçilmez|
| AppName | Rezervasyonları yapmak için bir uygulama adı.|TabloAç<br/>Yelp<br/>TripAdvisor|
| Atmosfer | Bir restoran veya başka bir yerde Atmosfer açıklaması.|Romantik<br/>sıradan<br/>gruplar için iyi|
| Cuisine | Gıda, cuisine veya cuisine Uyruğu türü. |Çince<br/>İtalyanca<br/>Meksika|
| MealType | Ayırma ile ilişkili bir paket türü.|Kahvaltı<br/>Akşam Yemeği<br/>Öğle yemeği<br/>Supper|
| PlaceName | Yerel işletme, Restoran, ortak bir çekim veya kurum adı.|IHOP<br/>Cheesecake Fabrika<br/>Louvre|
| PlaceType | Yerel işletme, Restoran, ortak bir çekim veya kurum türü.|Restoran<br/>Opera<br/>sinema|
| Derecelendirme | Çalındığında ya da Restoran derecesi.|5 yıldız<br/>3 yıldız<br/>4 yıldız|

## <a name="taxi"></a>Taksi 
 
Amaç ve varlıkları oluşturma ve yönetme taksi rezervasyonlarının taksi etki alanı sağlar.

### <a name="examples"></a>Örnekler

|Örnekler|
|--|
|Bana bir cab 3 saat Al|
|Ne kadar uzun my taksi için beklemek zorunda?|
|My Uber iptal et|

### <a name="intents"></a>Hedefler
| Hedefi adı | Açıklama | Örnekler |
| ---------------- |-----------------------|----|
| Book | Bir taksi çağırın. |Bana bir cab Al<br/>Bir taksi bulun<br/>Bana bir uber kitap x|
| İptal | Bir taksi kayıt için ilgili eylemi iptal eder.|My taksi iptal et<br/>My Uber iptal et|
| İzle | Taksi yolu izleyin.|Ne kadar uzun my taksi için beklemek zorunda?<br/>My Uber nerede?|

### <a name="entities"></a>Varlıklar
| Varlık adı | Açıklama | Örnekler |
| ---------------- |-----------------------|----|
| Adres| Bir taksi kayıt ile ilişkili adres. |Palo Alto<br/>300 112th Ave SE<br/>Seattle|
| DestinationAddress| Bir hedef konum veya adresi. |Palo Alto<br/>300 112th Ave SE<br/>Seattle|
| DestinationPlaceName | Yerel işletme, Restoran, ortak bir çekim veya kurum bir hedef adı. |Orta parka<br/>Safeway<br/>Walmart|
| DestinationPlaceType | Yerel işletme, Restoran, ortak bir çekim veya kurum bir hedef türü. |Restoran<br/>Opera<br/>sinema|
| PlaceName | Yerel işletme, Restoran, ortak bir çekim veya kurum adı. |Orta parka<br/>Safeway<br/>Walmart|
| PlaceType| Yerinde bir taksi rezervasyonu için bir istek türü.|Restoran<br/>Opera<br/>sinema|
| TransportationCompany | Aktarım sağlayıcısının adı.|Amtrak<br/>Acela<br/>Greyhound|
| TransportationType | Taşıma türü.|Veri yolu<br/>Eğitim<br/>Sürüş|

## <a name="translate"></a>Çevirme 
Hedefleri ve metni bir hedef dile çevirmek için ilgili varlıkları Çevir etki alanı sağlar.

### <a name="examples"></a>Örnekler

|Örnekler|
|--|
|Fransızca Çevir|
|Almanca Hello Çevir|
|İngilizce için bu cümleyi Çevir|


### <a name="intents"></a>Hedefler
| Hedefi adı | Açıklama | Örnekler |
| ---------------- |-----------------------|----|
| Çevirme| Metni başka bir dile çevirir.|Fransızca Çevir<br/>Almanca Hello Çevir|


### <a name="entities"></a>Varlıklar
| Varlık adı | Açıklama | Örnekler |
| ---------------- |-----------------------|----|
| TargetLanguage | Bir çeviri hedef dili.|Fransızca <br/>Almanca <br/>Korece|
| Metin | Çevrilecek metin.|Hello World<br/>Günaydın<br/>İyi akşamlar|

## <a name="tv"></a>TV 
 
TV etki alanı, TV denetlemek için amaç ve varlıkları sağlar.

### <a name="examples"></a>Örnekler

|Örnekler|
|--|
|BBC geçiş kanalı|
|TV Kılavuzu Göster|
|Ulusal coğrafi izleyin|

### <a name="intents"></a>Hedefler
| Hedefi adı | Açıklama | Örnekler |
| ---------------- |-----------------------|----|
| ChangeChannel| Bir TV kanalda değiştirin.|CNN kanala Değiştir<br/>BBC geçiş kanalı<br/>4 kanala gidin|
| ShowGuide| TV Kılavuzu gösterir.|TV Kılavuzu Göster<br/>Film kanal artık nedir?<br/>My program listesini göster|
| WatchTV| Bir televizyon kanalı izlemek isteyebilirsiniz.|Disney kanal izlemek istiyorum<br/>TV Lütfen gidin<br/>Ulusal coğrafi izleyin|

### <a name="entities"></a>Varlıklar
| Varlık adı | Açıklama | Örnekler |
| ---------------- |-----------------------|----|
| ChannelName | Bir TV kanalın adı.|CNN<br/>BBC<br/>Film kanal|

## <a name="utilities"></a>Altyapı Hizmetleri  
Yardımcı programları etki alanı hedefleri greetings, iptal, onay, Yardım, yineleme ve gezintisi, başlatma ve durdurma gibi birçok görev için ortak olan görevleri sağlar.

### <a name="examples"></a>Örnekler

|Örnekler|
|--|
|Twitter'da geri dönün|
|Lütfen yardımcı olun|
|Lütfen son soru tekrarlayın|


### <a name="intents"></a>Hedefler
| Hedefi adı | Açıklama | Örnekler |
| ---------------- |-----------------------|----|
| İptal | Eylemi iptal eder.|İletiyi iptal et<br/>Artık e-posta göndermek istemiyorum|
| Onayla | Bir eylemi onaylayın.|Evet oh olduğumu onaylıyorum<br/>İyi onaylayan<br/>Tamamdır onaylayan|
| FinishTask | Kullanıcı başlatılan bir görevi tamamlayın.|İşim bitti<br/>İşim bitti<br/>Yapıldığını|
| GoBack | Bir adıma geri dönün veya bir önceki adıma geri dönün.|Twitter'da geri dönün<br/>Bir adıma geri dönün<br/>Geri git|
| Yardım | Yardım isteği.|Lütfen yardımcı olun<br/>Yardımı<br/>Yardım|
| Yinele | Bir eylemi yineleyin.|Lütfen son soru tekrarlayın<br/>Son şarkı yineleyin|
| ShowNext | Bir dizinin sonraki öğeyi gösterir. |Sonrakini Göster<br/>Sonraki Sayfaya Git|
| ShowPrevious | Önceki öğeyle bir dizide gösterir.|Öncekine Göster|
| StartOver | Uygulamayı yeniden başlatın veya yeni bir oturum başlatın.|Yeniden başlayın<br/>Yeni oturum<br/>restart|
| Durdur | Bir eylem durdurun.| Bu Lütfen belirten Durdur<br/>Kes sesini<br/>Lütfen durdurma|

## <a name="weather"></a>Hava durumu 
Hava durumu etki alanı, hava durumu raporları ve öngörüleri almak için amaç ve varlıkları sağlar.

### <a name="examples"></a>Örnekler

|Örnekler|
|--|
|hava durumu, Eylül ayının Londra|
|Ne? s 10 gün tahmini?|
|Eylül'de bulunan ortalama sıcaklık nedir?|


### <a name="intents"></a>Hedefler
| Hedefi adı | Açıklama | Örnekler |
| ---------------- |-----------------------|----|
| GetCondition | Hava durumu için ilgili geçmiş gerçekleri öğrenin. |hava durumu, Eylül ayının Londra<br/>Eylül'de bulunan ortalama sıcaklık nedir?|
| GetForecast | Geçerli hava durumunu alın ve birkaç gün için tahmin edin. |Nasıl hava durumu hemen mi?<br/>10 gün tahmini nedir?<br/>Hava durumu, bu hafta nasıl olacak?|

### <a name="entities"></a>Varlıklar
| Varlık adı | Açıklama | Örnekler |
| ---------------- |-----------------------|----|
| Konum| Hava durumu isteği için bir mutlak konumu.|Seattle<br/>Paris<br/>Palo Alto|

## <a name="web"></a>Web 
Web etki alanı bir hedefi için bir Web sitesine gezinme sağlar.

### <a name="examples"></a>Örnekler

|Örnekler|
|--|
|İçin facebook.com gidin|
|Www.twitter.com için Git|
|İçin www.Bing.com gidin|

### <a name="intents"></a>Hedefler
| Hedefi adı | Açıklama | Örnekler |
| ---------------- |-----------------------|----|
| Gidin | Belirtilen bir Web sitesine gidin isteği. |İçin facebook.com gidin<br/>Www.twitter.com için Git|

