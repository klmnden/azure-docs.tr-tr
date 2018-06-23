---
title: Önceden oluşturulmuş bir etki alanı başvurusu - Azure | Microsoft Docs
titleSuffix: Azure
description: Hedefleri ve varlıkları gelen dil anlama akıllı Hizmetleri (HALUK), önceden oluşturulmuş koleksiyonlarıdır önceden oluşturulmuş etki referansı.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 06/20/2018
ms.author: v-geberr
ms.openlocfilehash: 8e04853e0044e045158642fea51c225378eb3ad6
ms.sourcegitcommit: 65b399eb756acde21e4da85862d92d98bf9eba86
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/22/2018
ms.locfileid: "36319063"
---
# <a name="prebuilt-domain-reference"></a>Önceden oluşturulmuş bir etki alanı başvurusu
Bu başvuru amaçları ve HALUK sunar varlıklar, önceden oluşturulmuş koleksiyonlarıdır önceden oluşturulmuş etki hakkında bilgi sağlar.

## <a name="list-of-prebuilt-domains"></a>Önceden oluşturulmuş etki alanlarının listesi
HALUK 20 önceden oluşturulmuş etki alanı sunar. 

| Önceden oluşturulmuş bir etki alanı | Açıklama | Desteklenen Diller |
| ---------------- |-----------------------|:------:|
| Takvim | Takvim etki alanı amacını ve ekleme, silme, veya bir randevu düzenleme, katılımcı kullanılabilirlik denetleniyor ve takvim olayı hakkında bilgi bulmak için varlıkları sağlar.| tr-TR<br/> zh-CN |
| Kamera | Kamera etki alanı, resim, video kaydetmek ve yayın video uygulamaya almak için hedefleri ve varlıkları sağlar.| tr-TR |
| İletişim | İleti gönderme ve telefon çağrıları yapma.| tr-TR <br/> zh-CN |
| Eğlence  | Müzik, filmler ve TV ile ilgili sorgular işleme.| tr-TR |
| Olaylar | Konsere, festivals, spor oyunları ve Komedi biletlerini kayıt gösterir.| tr-TR |
| Uygunluk | Uygunluk etkinlikleri izleme için ilgili istekleri işleme.| tr-TR |
| Oyun | Çok oyunculu oyun oyun parti ilgili istekleri işleme.| tr-TR |
| HomeAutomation | Akıllı ana aygıtlar ışık ve Gereçleri gibi denetleme.| tr-TR<br/> zh-CN |
| MovieTickets | Bilet filmler film tiyatro adresindeki kayıt.| tr-TR |
| Müzik | Müzik bir müzik çalar yürütülüyor.| tr-TR<br/> zh-CN |
| Not | Not etki alanı hedefleri ve varlıkları oluşturma, düzenleme ve notları bulma ile ilgili sağlar.| tr-TR<br/> zh-CN |
| OnDevice | Hedefleri ve cihaz denetlenmesi için ilgili varlıklar OnDevice etki alanı sağlar.| tr-TR<br/> zh-CN |
| Yerler  | İşletmeler, kurumlar, Restoran, ortak alanları ve adresleri gibi yerlerde ilgili sorgular işleme.| tr-TR<br/> zh-CN |
| Anımsatıcı | Oluşturma, düzenleme ve anımsatıcıları bulma ile ilgili istekleri işleme.| tr-TR<br/> zh-CN |
| RestaurantReservation | Restoran ayırmaları yönetmek için istekleri işleme.| tr-TR<br/> zh-CN |
| Ücreti | Bir ücreti kayıtları işleme.| tr-TR<br/> zh-CN |
| Çevir | Metin bir hedef dile çevirme.| tr-TR<br/> zh-CN |
| TV | TV denetleme.| tr-TR |
| Altyapı Hizmetleri  | "Yardım" gibi birden çok etki alanı içinde ortak olan istekleri işleme "yineleyin", "yeniden başlatın."| tr-TR |
| Hava durumu | Hava raporları ve tahminleri alınıyor.| tr-TR<br/> zh-CN |
| Web | Bir Web sitesine giderek.| tr-TR<br/> zh-CN |

Her etki alanı üzerinde daha ayrıntılı bilgi için aşağıdaki bölümlerde bakın.

## <a name="calendar"></a>Takvim 

Takvim etki alanı hedefleri ve ilgili takvim girişlerinin varlıkları sağlar. Takvim hedefleri ekleme, silme veya bir randevu düzenleme, kullanılabilirlik denetleniyor ve bir takvim girişi veya randevu hakkındaki bilgileri bulma içerir.

### <a name="intents"></a>Hedefler
| Hedefi adı | Açıklama | Örnekler |
| ---------------- |-----------------------|----|
| Ekle | Yeni bir kerelik öğesi takvime ekleyin.| Lisa ile bir randevu 2 pm Pazar'konumunda oluşturun <br/><br/>Toplantı zamanlamak istiyorum<br/><br/>Toplantı ayarlama gerekir|
| CheckAvailability | Bir randevu veya toplantı kullanıcının takvim veya başka bir kişinin takvim için kullanılabilirlik bulun.| Jim karşılamak kullanılabilir olduğunda? <br/><br/>Carol yarın kullanılabilir olduğunda göster<br/><br/>Chris Cumartesi günleri ücretsiz mi?|
| Sil | Takvim girişi silmek için istek.| Carol ile My randevu iptal edin. <br/><br/>My 09: 00 toplantı Sil<br/>|
| Düzenle | Varolan Toplantı veya Takvim Girişi değiştirme isteği.| My 09: 00 toplantı 10: 00 için taşıyın.<br/><br/>My zamanlama güncelleştirmek istediğiniz.<br/><br/>My toplantı Reschdule Ryan ile.|
| Bul | Haftalık takvimim görüntüler.| Bul hekimi randevu gözden geçirin. <br/><br/>takvimim Göster<br/>|

### <a name="entities"></a>Varlıklar
| Varlık adı | Açıklama | Örnekler |
| ---------------- |-----------------------|----|
| Konum | Takvim öğesi, toplantı veya randevu konumu. Adresleri, şehir ve bölge iyi konumları gösterilebilir.| 209 Nashville Spor salonu <br/><br/>897 Krep temizleme<br/><br/>Garaj|
| Konu | Toplantı veya randevu başlığı.| Hekimi'nın randevu <br/><br/>Jale ile Yemeği<br/><br/>Doktor randevusu|

## <a name="camera"></a>Kamera 
Kamera etki alanı hedefleri ve kamera kullanmayla ilgili varlıkları sağlar. Hedefleri bir fotoğraf, selfie, ekran veya görüntü yakalamak ve bir uygulamaya video yayınlamak kapsar.

### <a name="intents"></a>Hedefler
| Hedefi adı | Açıklama | Örnekler |
| ---------------- |-----------------------|----|
| CapturePhoto| Fotoğraf yakalayın.| Fotoğraf Al<br/><br/>Yakalama|
| CaptureScreenshot | Bir ekran görüntüsü alma.| Ekran görüntüsü alın.<br/><br/>Ekran Yakalama.|
| CaptureSelfie | Bir selfie yakalayın.| Bir selfie alın <br/><br/>Bana resmini alın |
| CaptureVideo | Kayıt video başlatın.| Kaydı başlat <br/><br/>Kaydetmeye başlamak|
| StartBroadcasting| Yayın video başlatın.| Facebook yayınlama Başlat|
| StopBroadcasting| Yayın video durdurun.| Yayın Durdur|
| StopVideoRecording| Bir video kaydı durdurun.| Yeterli olmasıdır<br/><br/>durdurmanıza|

### <a name="entities"></a>Varlıklar
| Varlık adı | Açıklama | Örnekler |
| ---------------- |-----------------------|----|
| AppName | Video yayını için bir uygulama adı.| OneNote<br/><br/>Facebook<br/><br/>Skype|


## <a name="communication"></a>İletişim 
Hedefleri ve e-posta, iletileri ve telefon aramaları için ilgili varlıklar iletişimi etki alanı sağlar.

### <a name="intents"></a>Hedefler
| Hedefi adı | Açıklama | Örnekler |
| ---------------- |-----------------------|----|
| AddContact| Yeni bir kişi kullanıcının kişiler listesine ekleyin.|Yeni Kişi Ekle <br/><br/>Bu sayı kaydetmek ve Carol adıyla yerleştirin|
| AddMore| Bir e-posta veya metin, daha fazla step-wise e-posta veya metin birleşimin parçası ekleyin.|Daha fazla metin ekleme <br/><br/>Gövde e-posta ile daha fazla Ekle|
| Yanıt| Gelen telefon aramasını yanıtlayın.|yanıt çağrısı <br/><br/>Çekme|
| AssignContactNickname| Takma ad kişiye atayın.|Babanızın için Isaac değiştirme <br/>Jim'ın takma ad Düzenle<br/>Takma ad Patti Owens ekleme|
| CallVoiceMail| Kullanıcının sesli posta bağlayın.|Bana my sesli posta kutusuna bağlanın <br/>sesli posta<br/>sesli posta çağırın|
| CheckIMStatus| Skype kişi durumunu denetleyin.|Jim'ın çevrimiçi durumu hemen ayarlanır? <br/>Carol, sohbet etmek var mı?|
| Onayla| Bir eylem onaylayın.|Evet<br/>Tamam<br/>Anlaşıldı<br/>I bu e-posta göndermek istediğinizi onaylayın.<br/>|
| Arama| Bir telefon araması yapın.|Jim çağırın<br/>Lütfen 311 arama<br/>|
| FindContact| Kişi bilgileri ada göre bulur.|Carol'ın numarası Bul<br/>Carol'ın numarası göster<br/>|
| FindSpeedDial| Bir telefon numarası için ve tersi yönde ayarlamak hızlı arama numarasını bulun.|Arama numaramı 5 nedir?<br/>Set Çevir var mı?<br/>941 5555 333 arama numarası nedir?|
| GetForwardingsStatus| Çağrı yönlendirmenin geçerli durumunu alır.|My Çağrı yönlendirmenin açık mı?<br/>Çağrı Durumum açmak veya kapatmak olup olmadığını bildir<br/>|
| GoBack| Önceki adıma geri dönün.|Twitter için geri dönün<br/>Bir adımı geri dönün<br/>Geri dön|
| Yoksayma| Gelen bir arama yoksay.|Yanıt yok<br/>Çağrı yoksay|
| IgnoreWithMessage| Gelen bir arama yoksay ve bunun yerine metinle yanıt.|Bu çağrı alamıyorsanız, ancak bir ileti gönderin.<br/>Yoksay ve bir metin geri gönderin.|
| PressKey| Düğme veya sayı tuş takımında basın.|Arama yıldız.<br/>1 2 3'e basın.|
| ReadAloud| Bir ileti veya e-posta kullanıcıya okuyun.|Metni okuyun.<br/>Ne depoladığından iletinin söyleyin?|
| TurnForwardingOff| Bir telefon araması yapın.|<br/><br/>|
| Arama| Yeniden denemek mi yoksa bir sayı yeniden çağırın.|Arayın.<br/>My son çağrı arayın.|
| Reddet| Gelen çağrıyı reddeder.|Çağrı Reddet<br/>Şimdi yanıt olamaz<br/>Şu anda kullanılabilir değil ve daha sonra geri çağırır.|
| SendEmail| Bir e-posta gönderin. Bu amaç için e-posta ancak metin iletileri için geçerlidir.|Can sularında e-posta: Mike Yemeği geçen hafta sende çok iyi.<br/>Bob için bir e-posta Gönder<br/>|
| SendMessage| Bir metin iletisi veya anlık ileti gönderin.|Chris ve Carol için metin Gönder|
| SetSpeedDial| Hızlı arama kısayol için bir kişinin telefon numarası ayarlayın.|Hızlı Çevir bir Carol için ayarlayın.<br/>Mom için hızlı arama ayarlayın.|
| ShowNext| Sonraki öğeye, örneğin, bir kısa mesaj veya e-postaları listesinde bakın.|Bir sonraki gösterir.<br/>Sonraki sayfaya gidin.|
| ShowPrevious| Önceki öğesi, örneğin, bir kısa mesaj veya e-postaları listesinde bakın.|Öncekinin gösterir.<br/>Önceki<br/>Önceki gidin.|
| StartOver| Sistem üzerinde başlatmak veya yeni bir oturum başlatın.|Yeniden başlayın<br/>Yeni oturum<br/>restart|
| TurnForwardingOff| Çağrı yönlendirmenin devre dışı bırakın.|My çağrıları iletme Durdur<br/>Çağrı yönlendirmenin kapalı geçiş|
| TurnForwardingOn| Konuşmacı telefonu kapatın.|My çağrıları 3333 iletme<br/>Çağrı yönlendirmenin 3333 için geçiş|
| TurnSpeakerOff| Konuşmacı telefonu kapatın.|Bana Konuşmacı alın.<br/>Hoparlörlü devre dışı bırakın.<br/>|
| TurnSpeakerOn| Konuşmacı telefonda açın.|Hoparlörlü modu.<br/>Hoparlörlü yerleştirin.<br/>|

### <a name="entities"></a>Varlıklar
| Varlık adı | Açıklama | Örnekler |
| ---------------- |-----------------------|----|
| AudioDeviceType | Ses aygıtını (Konuşmacı, kulaklık, mikrofon, vb.) türü.| Konuşmacı<br/>Tutmadan<br/>Bluetooth|
| Kategori | Bir ileti veya e-posta kategorisi.| Önemli<br/>Yüksek öncelikli işler|
| ContactAttribute | Kullanıcı hakkında yapılabilecek Sorgulamalar kişi özniteliğidir.| Doğum<br/>Adres<br/>Telefon numarası|
| ContactName | Bir kişi veya ileti alıcı adı.| Carol<br/>Jim<br/>Chris|
| EmailSubject | Konu satırı bir e-postalar için kullanılan metin.| RE: öykü ilginç|
| Çizgi | Kullanıcının satırı çağırmaya ya da bir metin/e-posta göndermek için kullanmak istiyor.| İş satır<br/>İngiliz hücre<br/>Skype|
| İleti | Bir e-posta veya metin olarak gönderilecek ileti.| Bugün toplantı harika. En kısa zamanda tekrar bakın!|
| MessageType | Bir kişi veya ileti alıcı adı.| Metin<br/>Email|
| OrderReference | Almak için bir öğe tanımlayan bir liste sıralı veya göreli konumu. Örneğin, "son" veya "son" "I gönderilen son ileti neydi?"| Son<br/>En Son|
| Gönderenin adı | Gönderenin adı.| Patti Owens|

## <a name="entertainment"></a>Eğlence  
Etki alanı amaçları ve filmler, müzik, oyun ve TV için aramayla ilgili varlıklar sağlar eğlence gösterir.

### <a name="intents"></a>Hedefler
| Hedefi adı | Açıklama | Örnekler |
| ---------------- |-----------------------|----|
| Arama| Filmler, müzik, uygulamalar, oyun ve TV için arama gösterir.|Hale mağazada arayın.<br/>Avatar arayın.|

### <a name="entities"></a>Varlıklar
| Varlık adı | Açıklama | Örnekler |
| ---------------- |-----------------------|----|
| ContentRating | Medya G veya R gibi bir derecelendirme filmler için içerik.|Çocuklar video.<br/>PG derecelendirilir.|
| Tarz | Film, oyun, uygulama veya şarkı tarzını.|Comedies<br/>Dramas<br/>Komik|
| Anahtar sözcüğü| Bir öznitelik belirten bir genel arama anahtar sözcüğü daha özel ortam yuvalarında yok.|Parçalar<br/>Ay Akarsu<br/>Amelia Earhart|
| Dil | Medya G veya R gibi bir derecelendirme filmler için içerik.|Fransızca <br/>Türkçe<br/>Kore dili|
| MediaFormat | Medya biçimlendirilmiş ek özel teknik türü.|HD filmler<br/>3B filmler<br/>İndirilebilir|
| MediaSource | Mağaza veya medya alınırken Market.|Netflix<br/>Asal|
| MediaSubTypes| Medya türleri film ve oyunlara küçük.|Gösterileri<br/>DLC<br/>Tanıtımları|
| Uyruğu| Film, Göster veya eser oluşturulduğu ülke.|Fransızca <br/>Almanca <br/>Kore dili|
| Kişi| Aktör, director, üretici, müzisyen veya sanatçı film, uygulama, oyun veya TV programı ile ilişkilendirilmiş.|Madonna<br/>Stanley Kubrick|
| Rol| Medya oluşturma bir kişi tarafından yürütülen bir roldür.|Sings<br/>Yöneten<br/>Tarafından|
| Unvan| Film, uygulama, oyun, TV programı veya Şarkı adı.|Arkadaş<br/>Minecraft|
| Tür| Film, uygulama, oyun, TV programı veya şarkı türü veya ortam biçimi.|Müzik<br/>MovieTV <br/>gösterir|
| UserRating| Kullanıcı yıldız veya derecelendirme başparmak.|5 yıldız<br/>3 yıldız<br/>4 yıldız|

## <a name="events"></a>Olaylar 
Olayları etki alanı hedefleri sağlar ve varlıkları ayırtmak konsere, festivals, spor oyunları ve Komedi gibi olaylar için ilgili gösterir.

### <a name="intents"></a>Hedefler
| Hedefi adı | Açıklama | Örnekler |
| ---------------- |-----------------------|----|
| Rehberi| Bir olay bilet satın alın.|Bu hafta sonu symphony için bir bilet satın istiyor.|


### <a name="entities"></a>Varlıklar
| Varlık adı | Açıklama | Örnekler |
| ---------------- |-----------------------|----|
| Adres | Olay konumu veya adresi. |Palo Alto<br/>300 112th Ave kullan <br/> Seattle |
| Ad | Bir olayın adı.|Park Shakespeare|
| PlaceName| Olay konumu adı.|Louvre<br/>Opera temizleme<br/>Broadway|
| PlaceType | Konumun türü, olay bekletilir.|Cafe<br/>THEATRE<br/>Kitaplık|
| Tür | Bir olay türü.|Birlikte<br/>Spor oyun|

## <a name="fitness"></a>Uygunluk 
Uygunluk etki alanı hedefleri ve uygunluk etkinlikleri izleme için ilgili varlıklar sağlar. Hedefleri zaman veya uzaklığı kalan veya etkinlik sonuçları kaydetme not kaydetme içerir.

### <a name="intents"></a>Hedefler
| Hedefi adı | Açıklama | Örnekler |
| ---------------- |-----------------------|----|
| AddNote| Ek Notlar izlenen bir aktivite ekler.|Bu farklı çalıştır zorluk 6/10 idi<br/>Üzerinde çalışan üzerinde ben terrain asphalt olduğu<br/>3 hızı bisiklet kullanıyorum|
|GetRemaining| Bir etkinlik için kalan süre veya uzaklığı alır.|Ne kadar süre sonraki diz kasa?<br/>Kaç tane mil my çalıştırmada kalıyor mu? Bölme için ne kadar süre?|
| LogActivity| Tamamlanan etkinlik sonuçları günlüğü ya da kaydedin.|My son çalıştırma Kaydet<br/>My Cumartesi sabah ilerlemesi oturum<br/>My önceki yüz depolama|
| LogWeight| Kullanıcının geçerli ağırlık oturum ya da kaydedin.|My geçerli ağırlık Kaydet<br/>My ağırlık şimdi oturum<br/>My geçerli gövde ağırlık depolama|

### <a name="entities"></a>Varlıklar
| Varlık adı | Açıklama | Örnekler |
| ---------------- |-----------------------|----|
| ActivityType | İzlemek için etkinlik türü. |Çalıştırın<br/>Yol<br/>Yüz<br/>Döngüsü |
| Yiyecek | Uygunluk uygulamada izlemek için yemek türü. |Muz<br/>SOM<br/>Protein sallama|
| MealType| Bir sistem durumu veya uygunluk uygulamasında izlemek için yemek türü.|Kahvaltı<br/>Yemeği<br/>Yemeği<br/>Supper|
| Ölçüm| Zaman, uzaklığı veya uygunluk veya sistem durumu uygulamanızda ağırlık ölçümleri türü.|Kilometre<br/>Mil<br/>Dakika<br/>Kilogram|
| Sayı | Uygunluk veya sistem durumu uygulamanızda sayısal bir miktar.|19<br/>üç<br/>200<br/>bir|
| StatType | İstatistik türü uygunluk veya sistem durumu uygulamanızda toplanan verileri.|Toplam<br/>Ortalama<br/>Maksimum<br/>Minimum|

## <a name="gaming"></a>Oyun 
Oyun etki alanı hedefleri ve çok oyunculu oyun oyun parti yönetmeyle ilgili varlıkları sağlar.

### <a name="intents"></a>Hedefler
| Hedefi adı | Açıklama | Örnekler |
| ---------------- |-----------------------|----|
| InviteParty| Bir kişi bir oyun taraf katılmak için davet edin.|Bu player my tarafa davet et<br/>My tarafa gelen<br/>My clan katılın|
|LeaveParty| Bir etkinlik için kalan süre veya uzaklığı alır.|Out ben<br/>Bu taraf için başka bir bırakmak<br/>Çıkma|
| StartParty| Çok oyunculu oyun bir oyun taraf başlatın.|Şimdi Şaka bir taraf Başlat<br/>bir taraf Başlat<br/>bir clan bu akşam başlamanız gerekir|

### <a name="entities"></a>Varlıklar
| Varlık adı | Açıklama | Örnekler |
| ---------------- |-----------------------|----|
| İletişim| Çok oyunculu oyun kullanmak için bir ilgili kişi adı.|Carol<br/>Jim|


## <a name="homeautomation"></a>HomeAutomation 
Hedefleri ve akıllı ev aygıtlar ışık ve Gereçleri gibi denetlenmesi için ilgili varlıklar HomeAutomation etki alanı sağlar.

### <a name="intents"></a>Hedefler
| Hedefi adı | Açıklama | Örnekler |
| ---------------- |-----------------------|----|
| Kapatma| Kapatmak, kapatmak veya bir cihaz kilidini açın.|Işık devre dışı bırakma<br/>Kahve Makinesi Durdur<br/>Kapat Garaj kapı|
|Sok| Bir aygıtı açın veya belirli bir ayarı veya mod için cihaz ayarlayın.|My Kahve Makinesi Aç<br/>my Kahve Makinesi kapatabilir miyim?<br/>Thermostat 72 derece ayarlayın.|


### <a name="entities"></a>Varlıklar
| Varlık adı | Açıklama | Örnekler |
| ---------------- |-----------------------|----|
| Cihaz | Üzerinde veya kapatılabilir aygıt türü.|Kahve Makinesi<br/>Thermostat<br/>ışık|
| İşlem | Aygıt ayarlanacak durumu.|kilitleme<br/>aç<br/>açık<br/>kapalı|
| Yer | Konum veya cihaz yer.|Oturma odası<br/>Yatak<br/>mutfak|

## <a name="movietickets"></a>MovieTickets 
Hedefleri ve filmler film tiyatro adresindeki kayıt bilet ilgili varlıkları MovieTickets etki alanı sağlar.

### <a name="examples"></a>Örnekler
```
Book me two tickets for Captain Omar and the two Musketeers
Cancel tickets
When is Captain Omar showing?
```

### <a name="intents"></a>Hedefler
| Hedefi adı | Açıklama | Örnekler |
| ---------------- |-----------------------|----|
| Rehberi | Film biletleri satın alın.|Bana lideri Omar ve iki musketeers için iki biletleri kitap<br/>Bir bilet yarın'ın Film için satın almak istiyorum<br/>Bir bilet Captian Omar Kısım 2 için sonraki Çarşamba istiyorum|
|GetShowTime| Bir filmi showtime alın.|Ne zaman lideri Omar gösteren?|


### <a name="entities"></a>Varlıklar
| Varlık adı | Açıklama | Örnekler |
| ---------------- |-----------------------|----|
| Adres | Film tiyatro adresi.|Palo Alto<br/>300 112th Ave kullan<br/>Seattle|
| MovieTitle | Film başlığı.|Pi sayısının ömrü<br/>Hunger oyunlar<br/>Başlangıcı|
| PlaceName | Film tiyatro veya sinema adı.|Sinema Amir<br/>Alexandria Theatre<br/>New York tiyatrosu|
| PlaceType | Bir filmi adresindeki gösteren konum türü.|sinema<br/>tiyatro<br/>IMAX sinema|

## <a name="music"></a>Müzik 
Müzik etki alanı hedefleri ve bir müzik çalar müzik Yürütülüyor ilgili varlıklar sağlar.

### <a name="examples"></a>Örnekler
```
play Beethoven
Increase track volume
Skip to the next song
```

### <a name="intents"></a>Hedefler
| Hedefi adı | Açıklama | Örnekler |
| ---------------- |-----------------------|----|
| DecreaseVolume | Cihaz birim azaltın.|azaltmak İzle<br/>Aşağı birim|
| IncreaseVolume | Cihaz birim artırın.|İzleme birim artırın<br/>birimi|
| Sessiz |Oyun müzik sessiz.|Sessiz şarkı<br/>Sessiz kaydı koyun<br/>Sessiz müzik |
| Duraklat | Oyun müzik duraklatın.|Duraklat<br/>Duraklatma müzik<br/>Duraklatma İzle|
| PlayMusic | Bir cihazda müzik.|Kevin Durant Yürüt<br/>Paradise Coldplay tarafından Yürüt<br/>Merhaba Adele tarafından Yürüt|
| Yinele |Oyun müzik yineleyin.|Yineleme şarkı<br/>İzleme kazanç Yürüt<br/>Müzik yineleyin|
| Sürdür | Oyun müzik sürdürün.|Şarkı Sürdür<br/>Müzik yeniden Başlat<br/>Devam ettirmek|
| SkipBack | Geriye dönük bir izleme atlayın.|Sonraki şarkıya atla<br/>Sonraki şarkıya Yürüt|
| SkipForward |İleri takip atlayın.|Önceki şarkıyı çalma<br/>Önceki parçaya geri dönün |
| Durdur | Müzik kayıttan yürütme için ilgili bir eylem durdurun. |Bu albüm yürütmeyi durdurur.|
| Sesi Aç | Müzik çalma aygıtı sesi Aç.| Sesi Aç.|

### <a name="entities"></a>Varlıklar
| Varlık adı | Açıklama | Örnekler |
| ---------------- |-----------------------|----|
| ArtistName | Aktör, director, üretici, yazıcı, müzisyen veya sanatçı bir aygıtta yürütmek için medya ile ilişkilendirilmiş.|Elvis Presley<br/>Taylor Swift<br/>Adele<br/>Mozart|
| Tarz | İstenen müzik tarzı.|Ülke müzik<br/>Broadway klasikler<br/>Klasik müzik Baroque dönemden Yürüt|

## <a name="note"></a>Not 
Not etki alanı hedefleri ve varlıkları oluşturma, düzenleme ve notları bulma ile ilgili sağlar.

### <a name="examples"></a>Örnekler
```
Add to my groceries note lettuce tomato bread coffee
Check off bananas from my grocery list
Remove all items from my vacation list
```

### <a name="intents"></a>Hedefler
| Hedefi adı | Açıklama | Örnekler |
| ---------------- |-----------------------|----|
| AddToNote | Bilgi nota ekleme.|My Market Not lettuce merak ekmek kahve ekleme<br/>Yapılacaklar listem ekleme<br/>my Wunderlist leziz çörekler ekleme|
| CheckOffItem | Önceden varolan not öğelerinden kapalı denetleyin.|My Market listeden muzlar kapalı denetleyin<br/>Liste yapıldığı şekilde alışveriş my tatile Peynir kek işaretle|
| Temizle | Tüm öğeleri önceden varolan not temizleyin.|Tüm öğeleri my tatil listeden kaldırın<br/>My okuma listeden Temizle tüm|
| Onayla | Not için ilgili eylemi onaylayın.|Benim tarafımdan kesebilirsiniz.<br/>evet<br/>Tüm öğeleri listelerde tutma onaylayan|
| Oluştur | Yeni bir not oluşturun. | Liste oluştur<br/>Bu Jason hatırlat için May ilk haftasında piyasada olduğuna dikkat edin|
| Sil | Tüm Not silin. |Tatil listem Sil <br/>My Market not silme|
| DeleteNoteItem | Öğeleri önceden varolan bir not silin.| Yongaları my Market listeden sil<br/>Kalemler listesi alışveriş my Okul Kaldır|
| ReadAloud | Bir liste sesli okuyun.|Birinci beni oku<br/>Beni Oku ayrıntıları|
| ShowNext | Bir listedeki sonraki öğeye notların bakın.|Bir sonraki Göster<br/>Sonraki sayfa<br/>Sonraki|

### <a name="entities"></a>Varlıklar
| Varlık adı | Açıklama | Örnekler |
| ---------------- |-----------------------|----|
| AppName | Not Alma uygulama adı.|Wunderlist<br/>OneNote|
| ContactName | Not ilgili kişi adı.|Carol<br/>Jim<br/>Chris|
| Veri kaynağı | Notlar konumu.|OneDrive<br/>Google belgeleri<br/>Bilgisayarım|
| DataType | Dosya veya genellikle belirli yazılım programları ile ilgili belge türü.|Slayt<br/>Elektronik tablo<br/>Çalışma sayfası|
| Metin | Not veya anımsatıcı metni.|taramasını önce uzatma<br/>Yarın uzun çalışma|
| Unvan | Not başlığı.|Market<br/>çağrılacak kişiler<br/>Yapılacaklar|

## <a name="ondevice"></a>OnDevice 
Hedefleri ve cihaz denetlenmesi için ilgili varlıklar OnDevice etki alanı sağlar.

### <a name="examples"></a>Örnekler
```
Close video player
Cancel playback
Can you make the screen brighter?
```

### <a name="intents"></a>Hedefler
| Hedefi adı | Açıklama | Örnekler |
| ---------------- |-----------------------|----|
| AreYouListening | Cihaz dinlenip dinlenmediğini isteyin.|Bu, üzerinde mi?<br/>dinleme?|
|CloseApplication|Cihaz uygulamayı kapatın.|video oynatıcı kapatın|
|FileBug|Cihazda dosyalama.|Lütfen dosyalama<br/>Bir hata için Benioku dosyası?<br/>Bu hatayı bildirmek izin ver|
|GoBack|Tek bir adımda geri dönün veya önceki adıma dönmek için isteyin.|Lütfen geri dönün<br/>Önceki ekrana gidin<br/>Git geri Dur dinleme|
|Yardım| Yardım isteyin.|Lütfen yardımcı olun<br/>Merhaba<br/>Ne yapabilirsiniz?<br/>Yardım almam gerekiyor| 
|LocateDevice|Aygıtı bulun.|Telefonum bulabilir<br/>Can'ın iphone'umu Bul<br/>Telefonumu Bul|
|Oturum açma|Cihaz kullanarak bir hizmet için oturum açın.|Oturum açma Lütfen<br/>Facebook oturum açma<br/>LinkedIn günlüğüne|
|Oturum kapatma|Aygıtı kullanan bir hizmet dışı oturum açın.|Telefonum oturum<br/>Twitter oturum açın<br/>Oturumu kapat|
|MainMenu|Bir aygıtın ana menü görüntüleyin.|Görünüm menüsü.|
|OpenApplication|Aygıtta bir uygulamayı açın.|Uyarı Lütfen açın<br/>Kamerayı açın<br/>Takvim başlatma|
|OpenSetting|Bir aygıt ayarı açın.|Ağ Ayarları'nı açın.|
|PairDevice|Cihaz çifti.|Bluetooth sinyal telefona eşleştirme bana yardımcı olabilir<br/>Bluetooth'u açmasına ve dizüstü bilgisayar ile eşleştirin<br/>Dizüstü bilgisayarımda kayıtlı çifti Bluetooth sinyali|
|Kapalı | Aygıt devre dışı bırakın.|Bilgisayarımı kapatmalı<br/>Kapat<br/>My mobil devre dışı bırakma|
|QueryBattery|Pil ömrünün hakkında bilgi alın.|Pil ömrünün göster.<br/>Pil Durumum nedir<br/>Şimdi ne kadar pil kaldı?<br/>Pil Göster|
|QueryWifi|WiFi hakkında bilgi alın.|WiFi bilgi alın.|
|Yeniden Başlatma|Cihaz yeniden başlatın.|Lütfen yeniden başlatın.|
|RingDevice| Kayıp olduğunda bulmak için ring cihaza isteyin.|Telefonum halka.| 
|SetBrightness|Cihaz parlaklığını ayarlayın.|Set parlaklığını Orta<br/>Set parlaklığını yüksek<br/>Set parlaklığını düşük|
|SetupDevice|Cihaz kurulumu başlatın.|İşletim sistemi kurulumu yükleme istiyorum<br/>Lütfen kurulum<br/>Benim için kurulumu yapın|
|ShowAppBar|Bir uygulama çubuğunu göster.|Uygulama çubuğunu göster<br/>Uygulama çubuğunu Lütfen<br/>Uygulama çubuğunu görmek izin ver|
|ShowContextMenu|Bir bağlam menüsü gösterir.|Bağlam menüsü bkz izin ver<br/>Bağlam menüsü Lütfen<br/>Bana bağlam menüsü gösterebilir|
|Uyku|Uyku moduna aygıt yerleştirin.|Uyu<br/>Uyku<br/>My bilgisayar uyku modunda|
|SwitchApplication|Uygulamayı cihazda kullanmaya geçiş yapın.|My media player geçin.|
|TurnDownBrightness|Cihaz parlaklığını açın.|Ekran dim.|
|TurnOffSetting|Bir aygıt ayarı devre dışı bırakmak.|Bluetooth devre dışı bırakma<br/>Veri devre dışı bırak<br/>Bluetooth bağlantısını kesme|
|TurnOnSetting|Bir aygıt ayarı etkinleştirin.|Açık <br/> Kapalı|
|TurnUpBrightness|Cihaz parlaklığını açın.|Daha açık ekranda yapabilirsiniz?|

### <a name="entities"></a>Varlıklar
| Varlık adı | Açıklama | Örnekler |
| ---------------- |-----------------------|----|
| AppName | Aygıtta bir uygulamanın adı.|SoundCloud<br/>YouTube|
| BrightnessLevel | Cihazda parlaklığını düzeyini ayarlayın.|Yüz yüzde<br/>Elli<br/>40%|
| ContactName | Aygıtta bir ilgili kişi adı.|Paul<br/>Marlen Max|
| DeviceType | Aygıt türü. |Telefon<br/>Kindle<br/>Dizüstü bilgisayar|
| MediaType | Aygıt tarafından işlenen medya türü.|Müzik<br/>Film<br/>TV programı|
| SettingType | Ayar veya düzenlemek için kullanıcının istediği Ayarları panelini türü.|WiFi<br/>Kablosuz ağ<br/>Renk şeması<br/>Bildirim Merkezi|

## <a name="places"></a>Yerler  
Yerler etki alanı işletmeler, kurum, Restoran, ortak alanları ve adresleri gibi yerlerde ilgili sorguları işlemek için hedefleri sağlar.

### <a name="examples"></a>Örnekler
```
Save this location to my favorites
How far away is Holiday Inn?
At what time does Safeway close?
```

### <a name="intents"></a>Hedefler
| Hedefi adı | Açıklama | Örnekler |
| ---------------- |-----------------------|----|
| AddFavoritePlace | Bir konuma eklemeniz kullanıcının Sık Kullanılanlar listesi.|Bu konum Kullanılanlara Kaydet<br/>Bu adres Kullanılanlara Ekle|
|CheckAccident|Belirtilen yolda bir yanlışlık olup olmadığını isteyin.|Herhangi bir yanlışlık 880 üzerinde var mı?<br/>Yanlışlıkla bilgi göster|
|CheckAreaTraffic|Genel alan veya belirtilen bir rotaya değil, Otoyol trafiği denetleyin.|Seattle trafiği<br/>Gibi trafik Seattle'da nedir?|
|CheckIntoPlace|Sosyal medya kullanarak bir yere iade edildi.|Bana Foursquare üzerinde iade etme<br/>Burada iade etme|
|CheckRouteTraffic| Kullanıcı tarafından belirtilen belirli bir yolu, trafiği denetleyin.|Nasıl Mashiko trafiği mi?<br/>Kirkland için traffice Göster<br/>Nasıl Seattle trafiği mi?| 
|Onayla|Bir yere ilgili eylemi onaylayın.|My Restoran ayırma onaylayın.|
|Çıkış|Bir yere ilgili bir görevi'ndan çıkmak için eylem.|Lütfen çıkın<br/>Bana yönergeleri vermiş çıkın|
|FindPlace|Bir yerde (iş, kurum, Restoran, ortak alanı, adresi) için arama yapın.|Yakın kitaplık nerede?<br/>Bana iyi İtalyanca Restoran Sıradağlar Görünümü'nde bulur|
|GetAddress| Bir yerin adresini isteyin.|Robson sokakta Guu adresini Göster<br/>En yakın Starbucks adresi nedir?| 
|GetDistance işlevi|Belirli bir yere uzaklığı hakkında isteyin.|Ne kadar uzakta tatil Inn mi?<br/>Buradan kare Bellevue için ne kadar nedir<br/>Tahoe uzaklık nedir|
|GetHours|Bir yerde işletim saattir hakkında isteyin.|Ne zaman Safeway kapatmak?<br/>Giriş deposu saattir nelerdir?<br/>Starbucks hala açık mı?|
|GetMenu|Bir restoran menü öğelerinin isteyin.|Zucca herhangi bir şey hizmet mu vegan?<br/>Sizzler adresindeki menüsünde nedir<br/>Applebee'nın menüsünü göster|
|GetPhoneNumber| Bir yerde telefon numarası için isteyin.|En yakın Starbucks telefon numarasını nedir?<br/>Sayı giriş deposu için verin| 
|GetPriceRange| Bir yerde fiyat aralığının sorar.|Zucca ucuz mi?<br/>Cineplex yarım fiyat Çarşamba günleri mi?<br/>Ne kadar Sizzler tüm lobster Yemeği maliyetlerine mı çalışıyor?|
|GetReviews|Bir yerde incelemeler için isteyin.|Gözden geçirmeler Cheesecase Fabrika Göster<br/>Yelp içinde Cineplex değerlendirmeleri|
|GetRoute|Bir yere yönergeleri için isteyin.|Nasıl Bellevue kare yol<br/>8 ve 59th en kısa yolu buradan Göster<br/>Bana Sıradağlar görünüm CA'ya yönleri Al|
|GetStarRating|Bir yerde yıldız derecesiyle için isteyin.|Nasıl Zucca Yelp göre derecelendirilmiş?<br/>Kaç tane yıldız Fransızca Çamaşırlar var mı?<br/>Monterrey içinde aquarium iyi mi?|
|GetTransportationSchedule|Veri yolu zamanlama için bir yer alır.|Ne zaman şehir merkezi sonraki yoluna mi?<br/>Kol ilçe yollarına Göster|
|GetTravelTime|Seyahat süresi belirtilen hedef için isteyin.|Ne kadar San Francisco buradan almak için sürer<br/>Yönlendirmeli saati Beyoğlu için BT nedir|
|MakeCall|Bir yere bir telefon araması yapın.|MOM çağırın<br/>Gamze için Skype çağrı yapmak istiyorsunuz<br/>Jim çağırın|
|MakeReservation|Bir restoran ya da diğer iş için bir ayırma isteği.|İki tonight için Zucca ayırmak<br/>Kitap yarın için bir tablo<br/>Tablo 8 Palo Alto içinde 3 için|
|MapQuestions|Yönergeleri veya olup belirtilen yol bir hedefe gider hakkında bilgi istemek.|13 şehir merkezi geçirmek mu?<br/>İçin Oakland 880 devam edebilir mi?|
|Derecelendirme|Derecelendirme açıklaması bir restoran ya da yer alır.|Kaç tane yıldız Contoso Inn var mı?|
|ReadAloud|Yerlerin listesi sesli okuyun.|Birinci beni oku<br/>Beni Oku ayrıntıları|
|SelectItem|Bir yer veya yerlerde ilgili seçenekleri listesinden bir öğe seçin.|İkincisi seçin<br/>İlk seçin|
|ShowMap|Bir alanı haritasını gösterin.|İkincisi için bir eşleme Göster<br/>Haritayı göster<br/>San Francisco haritada Bul|
|ShowNext|Sonraki öğe bir seri gösterir.|Bir sonraki Göster<br/>sonraki sayfaya gidin|
|ShowPrevious|Önceki öğeyi bir seri gösterir.|öncekinin Göster<br/>önceki<br/>Önceki gidin|
|StartOver|Uygulamayı yeniden başlatın veya yeni bir oturum başlatın.|Yeniden başlayın<br/>Yeni oturum<br/>
restart|
|TakesReservations|Bir yerde ayırmalarını kabul edip etmeyeceğini isteyin.|Resim Galerisi ayırmalarını kabul ediyor mu<br/>Zeytin bahçesi rezervasyon yapmak mümkün mü

### <a name="entities"></a>Varlıklar
| Varlık adı | Açıklama | Örnekler |
| ---------------- |-----------------------|----|
| AbsoluteLocation | Konum veya bir yere adresidir.|Palo Alto<br/>300 112th Ave kullan<br/>Seattle|
| S | Amaç özellikleri/avantajları bir yer.|Çocuklar ücretsiz yemek<br/>rıhtımının<br/>Park boş|
| Atmosfer | Bir yerde Atmosfer.|Çocuklara yönelik<br/>sıradan Restoran<br/>sporty|
| Cuisine | Bir yerde cuisine. |Akdeniz<br/>İtalyanca<br/>Hindistan|
| DestinationAddress| Bir hedef konum veya adresi.|Palo Alto<br/>300 112th Ave kullan<br/>Seattle|
| DestinationPlaceName| Bir iş, Restoran, ortak çekim veya kuruluştan bir hedef adı.|Orta park<br/>safeway<br/>walmart|
| DestinationPlaceType | Yerel iş, Restoran, ortak çekim veya kuruluştan bir hedef türü. |Restoran<br/>Opera<br/>sinema|
| Uzaklık | Bir yere uzaklığı.|15 mil<br/>5 mil<br/>10 mil koyma|
| MealType | Yemek kahvaltı veya yemek gibi türü. |Kahvaltı<br/>Yemeği<br/>Yemeği<br/>Supper|
| OpenStatus | Bir yerde açık veya kapalı olup olmadığını gösterir.|Açık<br/>Kapalı<br/>açma|
| PlaceName | Bir alan adı.|Cheesecake Fabrika|
| PlaceType | Bir Yerleştir türüdür.|Cafe<br/>THEATRE<br/>Kitaplık|
| PreferredRoute | Kullanıcı tarafından belirtilen tercih edilen yol. | 101 <br/>202 <br/>Rota 401|
| Ürün | Bir yerde tarafından sunulan ürün. | Elbise<br/>Dijital ASR kameraları<br/>Yeni balık | 
| PublicTransportationRoute | Kullanıcı arıyor ortak taşıma rotanın adı. | Kuzey Doğu corridor eğitimi<br/>Veri yolu rota 3 X |
| Derecelendirme | Bir yerde derecesi. | 5 yıldız<br/>3 yıldız<br/>4 yıldız|
| RouteAvoidanceCriteria | KAZALARDAN dolayı RID'ler, kurulumlarını veya tolls önleme gibi belirli yollar önleme ölçütleri | Tolls <br/>Kurulumlarını<br/>Rota 11|
| ServiceProvided | Bu bir iş veya yöneticinize gibi yer tarafından sağlanan hizmetidir plowing, peyzaj kar. | yöneticinize<br/>Teknisyenin<br/>plumber|
| TransportationCompany | Aktarım sağlayıcısının adı.|Amtrak<br/>Acela<br/>Greyhound|
| TransportationType | Taşıma türü.|Veri yolu<br/>Eğitin<br/>Sürüş|

## <a name="reminder"></a>Anımsatıcı 
Hedefleri ve varlıkları oluşturma, düzenleme ve anımsatıcıları bulma için anımsatıcı etki alanı sağlar.

### <a name="examples"></a>Örnekler
```
Change my interview to 9 am tomorrow
Remind me to buy milk on my way back home
Can you check if I have a reminder about Christine's birthday?
```

### <a name="intents"></a>Hedefler
| Hedefi adı | Açıklama | Örnekler |
| ---------------- |-----------------------|----|
| Değiştir| Bir anımsatıcı olarak değiştirin.|09: 00 için yarın My görüşme değiştirme<br/>My atama anımsatıcı yarın için taşıma|
| Oluştur| Yeni bir anımsatıcı oluşturun.|bir anımsatıcı oluşturma<br/>Hatırlat sütlü satın almak için<br/>Evde ben Rebecca çağrılacak unutmayın istiyorum|
| Sil | Bir anımsatıcı silin.|My resim anımsatıcı Sil<br/>Bu anımsatıcı Sil|
| Bul | Bir anımsatıcı bulun.|My Yıldönümü hakkında bir anımsatıcı var mı?<br/>Bir anımsatıcı Christine'nın Doğum günü hakkında varsa göz atabilirsiniz?|

### <a name="entities"></a>Varlıklar
| Varlık adı | Açıklama | Örnekler |
| ---------------- |-----------------------|----|
| Metin | Bir anımsatıcı metin açıklaması.|kuru temizleme seçin<br/>Hizmet merkezinde My araba bırakarak devre dışı|

## <a name="restaurantreservation"></a>RestaurantReservation 
Hedefleri ve varlıklarla Restoran ayırmaları yönetmeyle ilgili RestaurantReservation etki alanı sağlar.

### <a name="examples"></a>Örnekler
```
Reserve at Zucca for two for tonight
Book a table at BJ's for tomorrow
Table for 3 in Palo Alto at 7
```

### <a name="intents"></a>Hedefler
| Hedefi adı | Açıklama | Örnekler |
| ---------------- |-----------------------|----|
| Ayırma | Bir Restoran için bir ayırma isteği. |İki tonight için Zucca ayırmak<br/>Kitap yarın için bir tablo<br/>Tablo 7 Palo Alto içinde 3 için|

### <a name="entities"></a>Varlıklar
| Varlık adı | Açıklama | Örnekler |
| ---------------- |-----------------------|----|
| Adres| Olay konumu veya bir ayırma adresi.|Palo Alto<br/>300 112th Ave kullan<br/>Seattle|
| S | Bir yerde s açıklayan bir öznitelik.|Okyanusu görünümü<br/>olmayan İçilmez|
| AppName | Ayırmalar yapmak için bir uygulama adı.|TabloAç<br/>Yelp<br/>TripAdvisor|
| Atmosfer | Bir restoran veya başka bir yerde Atmosfer açıklaması.|Romantik<br/>sıradan<br/>gruplar için iyi|
| Cuisine | Yemek, cuisine veya cuisine Uyruğu türü. |Çince<br/>İtalyanca<br/>Meksika|
| MealType | Ayırma ile ilişkili yemek türü.|Kahvaltı<br/>Yemeği<br/>Yemeği<br/>Supper|
| PlaceName | Yerel iş, Restoran, ortak çekim veya kurum adı.|IHOP<br/>Cheesecake Fabrika<br/>Louvre|
| PlaceType | Yerel iş, Restoran, ortak çekim veya kurum türü.|Restoran<br/>Opera<br/>sinema|
| Derecelendirme | Yer veya Restoran derecesi.|5 yıldız<br/>3 yıldız<br/>4 yıldız|

## <a name="taxi"></a>Ücreti 
 
Hedefleri ve varlıkları oluşturma ve yönetme ücreti kayıtları ücreti etki alanı sağlar.

### <a name="examples"></a>Örnekler
```
Get me a cab at 3 pm
How much longer do I have to wait for my taxi?
Cancel my Uber
```

### <a name="intents"></a>Hedefler
| Hedefi adı | Açıklama | Örnekler |
| ---------------- |-----------------------|----|
| Rehberi | Bir ücreti çağırın. |Bana bir cab Al<br/>bir ücreti Bul<br/>Bana bir uber kitap x|
| İptal | Bir ücreti kayıt için ilgili eylemi iptal eder.|My ücreti iptal et<br/>My Uber iptal et|
| İzle | Bir ücreti yol izler.|My ücreti için beklenecek ne kadar uzun var mı?<br/>My Uber nerede?|

### <a name="entities"></a>Varlıklar
| Varlık adı | Açıklama | Örnekler |
| ---------------- |-----------------------|----|
| Adres| Bir ücreti kayıt ile ilişkili adresi. |Palo Alto<br/>300 112th Ave kullan<br/>Seattle|
| DestinationAddress| Bir hedef konum veya adresi. |Palo Alto<br/>300 112th Ave kullan<br/>Seattle|
| DestinationPlaceName | Yerel iş, Restoran, ortak çekim veya kuruluştan bir hedef adı. |Orta Park<br/>Safeway<br/>Walmart|
| DestinationPlaceType | Yerel iş, Restoran, ortak çekim veya kuruluştan bir hedef türü. |Restoran<br/>Opera<br/>sinema|
| PlaceName | Yerel iş, Restoran, ortak çekim veya kurum adı. |Orta Park<br/>Safeway<br/>Walmart|
| PlaceType| Bir ücreti kitap isteği yerinde türü.|Restoran<br/>Opera<br/>sinema|
| TransportationCompany | Aktarım sağlayıcısının adı.|Amtrak<br/>Acela<br/>Greyhound|
| TransportationType | Taşıma türü.|Veri yolu<br/>Eğitin<br/>Sürüş|

## <a name="translate"></a>Çevir 
Çeviri etki alanı hedefleri ve metin hedef dile çevirme için ilgili varlıklar sağlar.

### <a name="examples"></a>Örnekler
```
Translate to French
Translate hello to German
Translate this sentence to English
```

### <a name="intents"></a>Hedefler
| Hedefi adı | Açıklama | Örnekler |
| ---------------- |-----------------------|----|
| Çevir| Başka bir dilde metne çevir.|Fransızca Çevir<br/>Almanca Hello Çevir|


### <a name="entities"></a>Varlıklar
| Varlık adı | Açıklama | Örnekler |
| ---------------- |-----------------------|----|
| TargetLanguage | Çeviri hedef dili.|Fransızca <br/>Almanca <br/>Kore dili|
| Metin | Çevrilecek metin.|Hello World<br/>Günaydın<br/>İyi akşamlar|

## <a name="tv"></a>TV 
 
TV etki alanı, TV denetlemek için hedefleri ve varlıkları sağlar.

### <a name="examples"></a>Örnekler
```
Switch channel to BBC
Show TV guide
Watch National Geographic
```

### <a name="intents"></a>Hedefler
| Hedefi adı | Açıklama | Örnekler |
| ---------------- |-----------------------|----|
| ChangeChannel| TV kanalında değiştirin.|CNN kanala Değiştir<br/>BBC geçiş kanalı<br/>4 kanal Git|
| ShowGuide| TV kılavuz gösterir.|TV Kılavuzu Göster<br/>Film kanalda şimdi nedir?<br/>program listem Göster|
| WatchTV| TV kanalı izlemek isteyin.|Disney kanal izlemek istediğiniz<br/>TV için lütfen gidin<br/>Ulusal coğrafi izleyin|

### <a name="entities"></a>Varlıklar
| Varlık adı | Açıklama | Örnekler |
| ---------------- |-----------------------|----|
| ChannelName | TV kanal adı.|CNN<br/>BBC<br/>Film kanal|

## <a name="utilities"></a>Altyapı Hizmetleri  
Yardımcı programlar etki alanı hedefleri Tebrikler, iptal, onay, Yardım, yineleme, başlatma ve durdurma Gezinti gibi pek çok görev için ortak olan görevleri sağlar.

### <a name="examples"></a>Örnekler
```
Go back to Twitter
Please help
Repeat last question please
```

### <a name="intents"></a>Hedefler
| Hedefi adı | Açıklama | Örnekler |
| ---------------- |-----------------------|----|
| İptal | Eylemi iptal eder.|İletiyi iptal etme<br/>Artık e-posta göndermek istemiyorum|
| Onayla | Bir eylem onaylayın.|Evet ı Onayla<br/>Onaylama iyi<br/>Onaylama Tamam|
| FinishTask | Kullanıcı başlatılmış bir görevi tamamlayın.|İşlemi bitirdim<br/>Yüklemeyi bitirdim<br/>Yapıldığını|
| GoBack | Tek bir adımda geri dönün veya önceki adıma geri dönün.|Twitter hesabına geri dönün<br/>Bir adımı geri dönün<br/>Geri dön|
| Yardım | Yardım iste.|Lütfen yardımcı olun<br/>Yardımı<br/>Yardım|
| Yinele | Bir eylem yineleyin.|Son bir soru Lütfen tekrarlayın<br/>Son şarkının yineleyin|
| ShowNext | Sonraki öğe bir seri gösterir. |Bir sonraki Göster<br/>sonraki sayfaya gidin|
| ShowPrevious | Önceki öğeyi bir seri gösterir.|öncekinin Göster|
| StartOver | Uygulamayı yeniden başlatın veya yeni bir oturum başlatın.|Yeniden başlayın<br/>Yeni oturum<br/>restart|
| Durdur | Bir eylem durdurun.| Lütfen belirten Durdur<br/>Kes sesini<br/>Lütfen durdurma|

## <a name="weather"></a>Hava durumu 
Hava durumu etki alanı, hava durumu raporları ve tahminleri almak için hedefleri ve varlıkları sağlar.

### <a name="examples"></a>Örnekler
```
weather in London in september
What?s the 10 day forecast?
What's the average temperature in India in september?
```

### <a name="intents"></a>Hedefler
| Hedefi adı | Açıklama | Örnekler |
| ---------------- |-----------------------|----|
| GetCondition | Hava durumu için ilgili geçmiş gerçekleri öğrenin. |Eylül Londra'da hava durumu<br/>Eylül'Hindistan ortalama sıcaklık nedir?|
| GetForecast | Geçerli hava durumu alın ve sonraki birkaç gün için tahmini. |Nasıl hava bugün mi?<br/>10 tahmin günü nedir?<br/>Nasıl hava bu hafta sonu olacak?|

### <a name="entities"></a>Varlıklar
| Varlık adı | Açıklama | Örnekler |
| ---------------- |-----------------------|----|
| Konum| Hava durumu isteği için mutlak konumu.|Seattle<br/>Paris<br/>Palo Alto|

## <a name="web"></a>Web 
Web etki alanı, bir Web sitesine gezinmek için amacına sağlar.

### <a name="examples"></a>Örnekler
```
Navigate to facebook.com
Go to www.twitter.com
Navigate to www.bing.com
```

### <a name="intents"></a>Hedefler
| Hedefi adı | Açıklama | Örnekler |
| ---------------- |-----------------------|----|
| Gidin | Belirtilen bir Web sitesine gitmek için bir istek. |İçin facebook.com gidin<br/>Www.twitter.com için Git|

