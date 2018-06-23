---
title: Cortana önceden oluşturulmuş uygulama başvurusu | Microsoft Docs
description: Cortana kişisel Yardımcısı, önceden oluşturulmuş bir uygulama gelen dil anlama akıllı Hizmetleri (HALUK) referansı.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 12/13/2017
ms.author: v-geberr
ms.openlocfilehash: cd808c30a6781fc0252992c13ba247486e35ad44
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35351923"
---
# <a name="cortana-prebuilt-app-reference"></a>Cortana önceden oluşturulmuş uygulama başvurusu
Bu başvuru Cortana önceden oluşturulmuş uygulama tanıdığı varlıkları ve hedefleri listeler.

## <a name="cortana-prebuilt-intents"></a>Cortana önceden oluşturulmuş hedefleri
Önceden oluşturulmuş kişisel yardımcı uygulama aşağıdaki hedefleri tanımlayabilirsiniz:

| Hedefi | Örnek utterances |
|--------| ------------------|
| Builtin.intent.alarm.alarm_other|My 7:30 güncelleştirme sabah sekiz olmasını Uyarısı <br/>My alarm 8: 00 09: 00 için değiştirin. |
| Builtin.intent.alarm.delete_alarm|bir alarm Sil <br/>"uyandırmak" Benim alarm Sil|
| Builtin.intent.alarm.find_alarm|my Uyandırma alarm ne zaman ayarlanır? <br/> My Uyandırma alarm üzerinde mi? |
| Builtin.intent.alarm.set_alarm|My Uyandırma alarm Aç<br/>adlı 12 antibiotics almak için bir alarm ayarlayabilir miyim?|
| Builtin.intent.alarm.snooze|Uyarı için 5 dakika uykuda<br/>erteleme Uyarısı|
| Builtin.intent.alarm.time_remaining| "Uyandırma kadar" ne kadar uzun var mı? <br/> ne kadar süre my sonraki alarm kadar?|
| Builtin.intent.alarm.turn_off_alarm | My 7 am alarmı devre dışı bırakma <br/> My Uyandırma alarmı devre dışı bırakma |
| Builtin.intent.Calendar.change_calendar_entry| Randevu değiştirme<br/>My toplantı bugünden itibaren yarın için taşıma|
|Builtin.intent.Calendar.check_availability|Tim Cuma günü meşgul?<br/>Brian Cumartesi günleri boş değil|
|Builtin.intent.Calendar.connect_to_meeting|toplantıya bağlanma<br/>Çevrimiçi toplantıya katılma|
|Builtin.intent.Calendar.create_calendar_entry|Randevu ile gereken tony Cuma için<br/>lisa ile bir randevu 2 pm Pazar'konumunda oluşturun|
|Builtin.intent.Calendar.delete_calendar_entry|My Randevu Sil<br/>3 pm toplantıya yarın my Takvimden Kaldır|
|Builtin.intent.Calendar.find_calendar_entry|takvimim Göster<br/>Haftalık takvimim görüntüleme|
|Builtin.intent.Calendar.find_calendar_when|ne zaman my İleri Can ile karşılıyor mu?<br/>ne zaman larry ile my randevu Pazartesi günü nedir?|
|Builtin.intent.Calendar.find_calendar_where|my sabah 6 toplantı konumunu göster<br/>Bu toplantı Can ile nerede?|
|Builtin.intent.Calendar.find_calendar_who|Bu toplantı kim?<br/>başka Kime davet?|
|Builtin.intent.Calendar.find_calendar_why|my sabah 11 toplantı ayrıntılarını nelerdir?<br/>Bu toplantı ile CAN nedir?|
|Builtin.intent.Calendar.find_duration|My sonraki toplantı uzunluğu nedir<br/>kaç dakika uzun my toplantı bu öğleden sonra mı|
|Builtin.intent.Calendar.time_remaining|ne kadar süre my sonraki Randevu kadar?<br/>ne kadar daha uzun ı önce toplantı var mı?|
|Builtin.intent.Communication.add_contact|Bu sayı kaydetmek ve jose adıyla yerleştirin<br/>Kişiler listem Jason yerleştirme|
|Builtin.intent.Communication.answer_phone|yanıt<br/>yanıt çağrısı|
|Builtin.intent.Communication.assign_nickname|nickolas için takma ad Düzenle<br/>john doe için takma ad ekleyin|
|Builtin.intent.Communication.call_voice_mail|sesli posta<br/>sesli posta çağırın|
|Builtin.intent.Communication.find_contact|Kişiler Göster<br/>kişileri Bul|
|Builtin.intent.Communication.forwarding_off|My çağrıları iletme Durdur<br/>Çağrı yönlendirmenin kapalı geçiş|
|Builtin.intent.Communication.forwarding_on|ev telefon çağrıları göndermek için Çağrı yönlendirmenin ayarlayın<br/>ev telefonu iletme çağrıları|
|Builtin.intent.Communication.ignore_incoming|Bu çağrı yanıt yok<br/>Şimdi değil meşgul ben|
|Builtin.intent.Communication.ignore_with_message|Bu çağrı alamıyorsanız ancak bunun yerine ileti gönderme<br/>Yoksay ve bir metin geri gönderin|
|Builtin.intent.Communication.make_call|Bob ve john çağırın<br/>MOM ve dad çağırın|
|Builtin.intent.Communication.press_key|Arama yıldız<br/>1 2 3'e basın|
|Builtin.intent.Communication.read_aloud|metin okuma<br/>ne depoladığından iletinin söyleyin|
|Builtin.intent.Communication.redial|My son çağrı arayın<br/>Arama|
|Builtin.intent.Communication.send_email|e-posta Mike Yemeği geçen hafta sende çok iyi CAN sularında<br/>Kemal bir e-posta Gönder|
|Builtin.intent.Communication.send_text|bob ve john metin Gönder<br/>message|
|Builtin.intent.Communication.speakerphone_off|Bana Konuşmacı alın<br/>Hoparlörlü devre dışı bırakma|
|Builtin.intent.Communication.speakerphone_on|Hoparlörlü modu<br/>üzerinde Hoparlörlü yerleştirme|
|Builtin.intent.mystuff.find_attachment|Belge john bana dün e-posta ile Bul <br/>john belge Bul|
|Builtin.intent.mystuff.find_my_stuff|Dün alışveriş my listesinden düzenlenecek istiyorum<br/>Eylül ' My Kimya Notları bulma
|Builtin.intent.mystuff.search_messages|iletiyi açın <br/>sayısı|
|Builtin.intent.mystuff.transform_my_stuff|alışveriş listem my eş ile paylaşma<br/>alışveriş listem Sil|
|Builtin.intent.ondevice.open_setting|cortana Ayarları'nı açın <br/>bildirimleri göndermek için Atla|
|Builtin.intent.ondevice.Pause|Müzik devre dışı bırakma<br/>Müzik devre dışı|
|Builtin.intent.ondevice.play_music|Yalnız bir Kalp sahibi duymak istiyorum<br/>Bazı gospel müzik|
|Builtin.intent.ondevice.recognize_song|Bu şarkı nedir bildir<br/>Başlık şarkının almanıza ve çözümlemenize|
|Builtin.intent.ondevice.REPEAT|Bu parçayı Yinele<br/>Bu şarkıyı yeniden çalma|
|Builtin.intent.ondevice.Resume|Müzik yeniden başlatın<br/>Müzik yeniden Başlat|
|Builtin.intent.ondevice.skip_back|bir izleme yedekleyin<br/>önceki şarkı|
|Builtin.intent.ondevice.skip_forward|sonraki şarkıya<br/>bir izleme İleri atlayabilirsiniz|
|Builtin.intent.ondevice.turn_off_setting|WiFi devre dışı<br/>Uçak modu devre dışı bırak|
|Builtin.intent.ondevice.turn_on_setting|WiFi üzerinde<br/>bluetooth'u açma|
|Builtin.intent.Places.add_favorite_place|Bu adres Kullanılanlara Ekle<br/>Bu konum Kullanılanlara Kaydet|
|Builtin.intent.Places.book_public_transportation|Tren bilet satın alın<br/>Kitap tram geçişi|
|Builtin.intent.Places.book_taxi|Bana kılma bu saat adresindeki bulabilirim?<br/>bir ücreti Bul|
|Builtin.intent.Places.check_area_traffic|520 gibi trafiğinde nedir<br/>nasıl i-5 trafiğinde mi|
|Builtin.intent.Places.check_into_place|joya denetleyin<br/>Burada iade etme|
|Builtin.intent.Places.check_route_traffic|trafiği kirkland yolundaki Göster<br/>nasıl seattle trafiği mi?|
|Builtin.intent.Places.find_place|Burada ı canlı <br/>ilk 3 Japonca Restoran ver|
|Builtin.intent.Places.get_address|guu adresini robson üzerinde Sokak Göster<br/>en yakın starbucks adresi nedir?|
|Builtin.intent.Places.get_coupon|anlaşmalar TV üzerinde Alanım alanında bulunamadı.<br/>indirimleri dağ görünümünde|
|Builtin.intent.Places.get_distance|ne kadar uzakta tatil inn mi?<br/>tahoe uzaklık nedir|
|Builtin.intent.Places.get_hours|Pazartesi günü del corso'nın saat çubuğu nelerdir?<br/>ne zaman Kitaplığı açık mı?|
|Builtin.intent.Places.get_menu|applebee'nın menüsünü göster<br/>sizzler adresindeki menüsünde nedir|
|Builtin.intent.Places.get_phone_number|sayı giriş deposu için verin<br/>en yakın starbucks telefon numarasını nedir?|
|Builtin.intent.Places.get_price_range|ne kadar kırmızı lobster maliyetle Yemeği mu<br/>Mor kafe nasıl pahalı mi|
|Builtin.intent.Places.get_reviews|incelemeler yerel donanım depoları için Göster<br/>Restoran incelemeleri görmek istiyorum|
|Builtin.intent.Places.get_route|yönlendirmeler ver|pizza hut yayan almak mümkündür|
|Builtin.intent.Places.get_star_rating|kaç tane yıldız Fransızca Çamaşırlar var mı?<br/>monterrey içinde aquarium iyi mi?|
|Builtin.intent.Places.get_transportation_schedule|ne zaman san francisco bırakın ferry?<br/>seattle için en son tren olduğunda?|
|Builtin.intent.Places.get_travel_time|yönlendirmeli saati beyoğlu için BT nedir<br/>ne kadar san francisco buradan almak için sürer|
|Builtin.intent.Places.make_call|Çağrı bellevue Dr. Etikan<br/>Giriş deposu 1 sokakta çağırın|
|Builtin.intent.Places.rate_place|Bu Restoran için bir derecelendirme verin<br/>yelp oranı IL fornaio 5 yıldız verme|
|Builtin.intent.Places.show_map|my geçerli konumu nedir <br/>my konumu nedir|
|Builtin.intent.Places.takes_reservations|Zeytin bahçesi adresindeki rezervasyon yapmak mümkün mü<br/>Resim Galerisi ayırmalarını kabul ediyor mu|
|Builtin.intent.Reminder.change_reminder|bir anımsatıcı değiştirme<br/>My resim gün anımsatıcı 30 dakika taşıma|
|Builtin.intent.Reminder.create_single_reminder|7'de uyandırmak üzere hatırlat<br/>hatırlat eli veya 4: 40'pm ile luca değerlendirmesini Git|
|Builtin.intent.Reminder.delete_reminder|Bu anımsatıcı Sil<br/>My resim anımsatıcı Sil|
|Builtin.intent.Reminder.find_reminder|için ayarladığınız anımsatıcıları sahip Bugün<br/>bir anımsatıcı luca'nın taraf için Ayarla var mı|
|Builtin.intent.Reminder.read_aloud|Anımsatıcı sesli okuyun<br/>Bu anımsatıcı okuma|
|Builtin.intent.Reminder.snooze|Bu anımsatıcı kadar yarın uykuda<br/>Bu anımsatıcı uykuda|
|Builtin.intent.Reminder.turn_off_reminder|Anımsatıcı kapatılamıyor<br/>Havaalanı toplama anımsatıcı kapatın|
|Builtin.intent.weather.change_temperature_unit|Fahrenhayt kelvin için değiştirme<br/>Fahrenhayt derece için değiştirme|
|Builtin.intent.weather.check_weather|seattle için my yaklaşan seyahat için tahmini Göster<br/>nasıl hava bu hafta sonu olacak|
|Builtin.intent.weather.check_weather_facts|Aralık içinde gibi hava Havai içinde nedir?<br/>geçen yıl bu kez sıcaklık neydi?|
|Builtin.intent.weather.compare_weather|dallas ve houston, tx lows ve sıcaklık yüksek arasında bir karşılaştırma ver<br/>hava durumu, slc ve nyc nasıl karşılaştırmak|
|Builtin.intent.weather.get_frequent_locations|en sık rastlanan my konumu ver<br/>en sık durakları Göster|
|Builtin.intent.weather.get_weather_advisory|hava durumu uyarıları<br/>hava durumu için sacramento danışma Göster|
|Builtin.intent.weather.get_weather_maps|hava durumu haritası görüntüleme<br/>Afrika için hava durumu eşlemeleri Göster|
|Builtin.intent.weather.question_weather|sisli yarın sabah olacak?<br/>Bu hafta sonu kar shovel gerekecek mi?|
|Builtin.intent.weather.show_weather_progression|Yerel hava durumu radar Göster<br/>Radar başlayın|
|Builtin.intent.None|Kaç yaşındasınız<br/>Kamera açın|

## <a name="prebuilt-entities"></a>Önceden oluşturulmuş varlıklar 

Önceden oluşturulmuş kişisel yardımcı uygulama tanımlayabilirsiniz varlıklar bazı örnekleri şunlardır:

|Varlık |Utterance varlık örneği |
|-------|------------------|
|Builtin.alarm.alarm_state | kapatma `off` my Uyandırma Uyarısı    <br/> My Uyandırma uyarının `on`  | 
|Builtin.alarm.Duration |için erteleme `10 minutes`    <br/> erteleme alarmı `5 minutes`  |
|Builtin.alarm.start_date | için bir alarm kurmak `monday` 7 sabah   <br/> için bir alarm kurmak `tomorrow` öğlen adresindeki   |
|Builtin.alarm.start_time | için bir uyarı oluştur `30 minutes`    <br/> Git uyarı ayarlama `in 20 minutes`   |
|Builtin.alarm.Title | olan my `wake up` alarm açık <br/> 12 çeyreğe Pazartesi ile Cuma adlı için bir alarm ayarlayabilir miyim `take antibiotics` |
|Builtin.Calendar.absolute_location | Yarın randevu oluşturma `123 main street`   <br/> Toplantı yer alacak `cincinnati` üzerinde 5, Haziran    |
|Builtin.Calendar.contact_name|Pazarlama toplantı takvimim üzerinde koyun ve olduğundan emin olun `joe` var. <br/>IL fornaio adresindeki Yemeği ayarlama ve davet etmek istiyorum `paul` |   
|Builtin.Calendar.destination_calendar|Bunun için ekleme my `work` zamanlama   <br/>Bu yerleştirin my `personal` Takvim |
|Builtin.Calendar.Duration| randevu ayarlama `an hour` bu akşam 6 adresindeki <br/>  Kitap bir `2 hour` joe ile toplantı |  
|Builtin.Calendar.end_date | Tatil kadar yarın çağrılır bir takvim girişi oluşturun `next monday` <br/>    My sürede kadar meşgul engelle `monday, october 5th` | 
|Builtin.Calendar.end_time | Toplantı uçta `5:30 PM` <br/> Bu zamanlama için 11 başlangıcı `noon`  |   
|Builtin.Calendar.implicit_location | dmv adresindeki randevu iptal et <br/> mil Doğum günü konumunu değiştirme `poppy restaurant` |    
|Builtin.Calendar.move_earlier_time | anında iletme toplantı ilet `an hour` <br/> hekimi'nın randevu Yukarı Taşı `30 minutes`       |
|Builtin.Calendar.move_later_time | My hekimi randevu taşıma `30 minutes`    <br/> Toplantı itme `an hour` |  
|Builtin.Calendar.original_start_date | My randevu 'Salı' dan şeritli Çarşamba için adresindeki yeniden zamanla <br/> ken ile My toplantı taşıma `monday` Salı için |
|Builtin.Calendar.original_start_time | My toplantı yeniden zamanla `2:00` 3  <br/> My hekimi randevusu değiştirmek `3:30` 4 |  
|Builtin.Calendar.start_date | ne zaman my taraf üzerinde başlatmak `flag day` <br/> zamanlama için yemeği `friday after next` öğlen adresindeki 
|Builtin.Calendar.start_time | İçin zamanlamak istiyorum `this morning` <br/> İçinde zamanlamak istiyorum `morning` |  
|Builtin.Calendar.Title | `vet appointment`  <br/> `dentist` Salı |
|Builtin.Communication.audio_device_type | Çağrı kullanarak yapın `bluetooth`  <br/> çağrıda my `headset` | 
|Builtin.Communication.contact_name | metin `bob jones`  <br/> | Arama `sarah`|
|Builtin.Communication.destination_platform | Dave Çağır `london` <br/> HIS çağırın `work line` |    
|Builtin.Communication.from_relationship_name | gelen çağrıları Göster my `daughter` <br/> gelen e-postasını oku `mom`   |   
|Builtin.Communication.Key | Arama `star` <br/>  basın `hash` anahtarı    |
|Builtin.Communication.Message | e-posta söylemek için carly `i'm running late` <br/> metin angus smith `good luck on your exam` |    
|Builtin.Communication.message_category | Yeni e-posta için işaretlenmiş `follow up`  <br/> Yeni e-posta işaretli `high priority` |    
|Builtin.Communication.message_type | Gönderme bir `email`   <br/> Okuma my `text` sesli iletileri |
|Builtin.Communication.phone_number | Aramak istediğiniz `1-800-328-9459` <br/>     Arama `555-555-5555` |   
|Builtin.Communication.relationship_name | metin my `husband` <br/>  E-posta `family`| 
|Builtin.Communication.slot_attribute | değiştirme `recipient` <br/>    değiştirme `text` | 
|Builtin.comminication.source_platform | ona gelen çağırın `skype` <br/> ona gelen çağrı my `personal line` |
|Builtin.mystuff.Attachment| belgeler `attached` <br/> e-posta Bul `attachment` gönderilen bob |
|Builtin.mystuff.contact_name| lisa gönderilen elektronik tablo Bul `me` <br/> burada i belge gönderildiği `susan` son gece |
|Builtin.mystuff.data_source | `c:\dev\` <br/> Uygulamam `desktop` <br/> |`"resolution": { "resolution_type": "metadataItems", "metadataType": CanonicalEntity", "value": "desktop" }`|
|Builtin.mystuff.data_type | bulun `document` i son gece üzerinde çalışılan <br/> |`"resolution": {"resolution_type": "metadataItems", "metadataType": "CanonicalEntity", "value":Document" }` <br/> Mert'in Getir `visio diagram`  |
|Builtin.mystuff.end_date| i üzerinde çalışılan belgeleri göster `between yesterday and today`   <br/> hangi belge i çalıştığı Bul `before thursday the 31st` |
|Builtin.mystuff.end_time| dosyaları bulmak i kaydedildi `before noon` <br/> hangi belge i çalıştığı Bul `before 4pm`|      
|Builtin.mystuff.file_action| Elektronik tabloyu açın ı `saved` dün <br/> Elektronik tabloyu bulmak kevin `created`| 
|Builtin.mystuff.from_contact_name | Teklif Bul `jason` bana gönderilen <br/> Açık `isaac` ait en son e-posta |
|Builtin.mystuff.Keyword | Göster `french conjugation` dosyaları <br/> Bul `marketing plan` i drafted dün |
|Builtin.mystuff.location | konumundaki i belge düzenlenmiş `work` <br/> Fotoğraf i sürdü `paris` |
|Builtin.mystuff.message_category | Aranan my `new` e-postaları <br/> Arama my `high priority` e-posta |
|Builtin.mystuff.message_type | denetleme my `email` <br/>  Göster my `text` iletileri  |
|Builtin.mystuff.source_platform | Arama my `hotmail` john e-posta için e-posta    <br/> i belge gönderildiği Bul `work` |
|Builtin.mystuff.start_date | bulma ile ilgili notlar `january` <br/> i e-posta Gönder kitap Bul `after january 1st` |
|Builtin.mystuff.start_time | Bu e-posta Bul i gönderilen kitap süre 2 pm önce ancak `after noon`? <br/> çalışma sayfası kristin ı düzenlenen bana gönderilen Bul `last night` | 
|Builtin.mystuff.Title | `c:\dev\mystuff.txt` <br/> `*.txt` |
|Builtin.mystuff.transform_action | `download` Dosya john bana gönderilen <br/> `open` My ek açıklama yönergeleri belge |
|Builtin.Note.note_text | Market dahil listesi oluşturma `pork chops, applesauce and milk` <br/> bir not edin `buy milk` |
|Builtin.Note.Title | adlı not edin `grocery list` <br/> adlı not edin `people to call` |
|Builtin.ondevice.music_artist_name | tarafından her şeyi Yürüt `rufus wainwright` <br/> Yürüt `garth brooks` müzik |   
|Builtin.ondevice.music_genre | Göster `classic rock` şarkıya <br/> Yürüt my `classical` baroque dönemden müzik |
|Builtin.ondevice.music_playlist | gelen tüm britney spears karışık `workout` çalma listesi <br/> Yürüt `breakup` çalma listesi
|Builtin.ondevice.music_song_name | Yürüt `summertime` <br/> Yürüt `me and bobby mcgee` | 
|Builtin.ondevice.setting_type | `quiet hours` <br/> `airplane mode` |
|Builtin.Places.absolute_location | Benim için kesişimi alın `5th and pike` <br/> Hayır, yönlendirmeler istiyorum `1 microsoft way redmond wa 98052` |
|Builtin.Places.atmosphere | Ara `interesting` Git yerler    <br/> nereden bulabilirim bir `casual` Restoran |  
|Builtin.Places.audio_device_type |Postane çağırmak `hands free` <br/> Çağrı papa Can'ın ile `speakerphone` |    
|Builtin.Places.avoid_route | kaçının `toll road` <br/> Bana san francisco kaçınmak için alma `construction on 101` |  
|Builtin.Places.cuisine | `halal` deli Sıradağlar görünüm yakın   <br/> `kosher` üzerinde yarımada yemek ince |
|Builtin.Places.Date | bir rezervasyon yapmak `next friday the 12th` <br/>  mashiko açıktır `mondays` ? |
|Builtin.Places.discount_type | bulma bir `coupon` macy'nın için <br/> find me bir `coupon` |
|Builtin.Places.distance | iyi bir diner yoktur `within 5 miles` Burada, <br/> olanları Bul `within 15 miles` |   
|Builtin.Places.from_absolute_location | gönderilen yönergeleri `45 elm street` giriş için <br/> Bana gönderilen yönergeleri almak `san francisco` palo alto için  |
|Builtin.Places.from_place_name | gelen yürüten `post office` 56 merkezi Sokak için <br/> Bana gönderilen yönergeleri almak `home depot` lowes için |
|Builtin.Places.from_place_type | Şehir merkezi yönergeleri `work` <br/>  Bana gönderilen yönergeleri almak `drug store` giriş için |
|Builtin.Places.meal_type | yakındaki yerler `dinner` <br/> bir iş için uygun bir yerdir Bul `lunch` | 
|Builtin.Places.nearby | Bazı harika dükkanları Göster `near` bana  <br/> Tüm iyi Lübnan Restoran vardır `around` burada? |
|Builtin.Places.open_status | küçük olduğunda `closed` <br/> Bana almak `opening` deposunun saatleri | 
|Builtin.Places.place_name | Bana alın `central park` <br/> Arama `eiffel tower` |   
|Builtin.Places.place_type | `atms` <br/> `post office` |
|Builtin.Places.prefer_route | tarafından yönergeleri Göster `shortest` yol <br/> ele `fastest` yol | 
|Builtin.Places.price_range| olan yerlerde ver `moderately affordable` <br/>  İstiyorum bir `expensive` bir   |
|Builtin.Places.Product | nereden alabilirim `fresh fish` burada geçici  <br/> Burada sattığı geçici yeri `bare minerals` |
|Builtin.Places.public_transportation_route | veri yolu zamanlamasını `m2` veri yolu <br/> veri yolu rota `3x` |  
|Builtin.Places.rating | Göster `3 star` Restoran <br/> sonuçları göster `3 stars or higher`  |
|Builtin.Places.reservation_number | bir tablo için kitap `seven` kişiler <br/>  bir rezervasyon yapmak `two` IL fornaio adresindeki |
|Builtin.Places.results_number | Göster `10` kafeterya için burada en yakın <br/> üst Göster `3` aquariums  
|Builtin.Places.service_provided | Burada miyim Git `whale watch` veri yolu tarafından? <br/> Bir teknisyenin gerekir `fix my brakes` |    
|Builtin.Places.Time | Cumartesi açık olan yerlerde istiyorum `8 am`| mashiko açık mı `now` |
|Builtin.Places.transportation_company | Tren zamanlamalarını `new jersey transit` <br/> var. alabilirim `bart` | 
|Builtin.Places.transportation_type | Müzik deposu i olduğu için elde edebilirsiniz `on foot`? <br/>  ver `biking` mashiko yönergeleri|
|Builtin.Places.travel_time | Sürücü kurabilmesi istiyorum `less than 15 minutes` <br/>   Bir yere ı elde edebilirsiniz istiyorum `under 15 minutes` |   
|Builtin.Reminder.absolute_location | my dad i güden oluşturulduğunda çağrılacak hatırlat `chicago` <br/> ne zaman alıyorum geri için `seattle` hatırlat gaz almak için |
|Builtin.Reminder.contact_name | zaman `bob` çağrıları, anımsatmak bana ona şaka bildirmek için <br/> Okul veri yolu için konuşurken belirtmeyi anımsatıcı oluşturma `arthur` |
|Builtin.Reminder.leaving_absolute_location | ayrılırken anımsatıcı çekme craig `1200 main` |
|Builtin.Reminder.leaving_implicit_location | hatırlat ı ayrıldığında gaz almak için `work`  |
|Builtin.Reminder.original_start_date | Çim hakkında anımsatma değiştirmek `saturday` pazar için <br/> Okul hakkında My anımsatıcı taşıma `monday` Salı için   |
|Builtin.Reminder.relationship_name | zaman my `husband` çağrıları, anımsatmak bana ona hakkında pta toplantı bildirmek için <br/> yeniden zaman hatırlat `mom` çağrıları|
|Builtin.Reminder.reminder_text | Bana hatırlat `bring up my small spot of patchy skin` zaman dr smith'calls  <br/> için hatırlat `pick up dry cleaning` 4:40 adresindeki   |
|Builtin.Reminder.start_date | hatırlat `thursday after next` 8 pm adresindeki  <br/> hatırlat `next thursday the 18th` 8 pm adresindeki    |
|Builtin.Reminder.start_time | bir anımsatıcı oluşturma `in 30 minutes` <br/> bitkilerin su anımsatıcısı oluşturma `this evening at 7` |  
|Builtin.weather.absolute_location | içinde sizi `boston` <br/> Tahmin için nedir `seattle`?  |
|Builtin.weather.date_range | nyc, hava durumu `this weekend` <br/>   Arama `five day` içinde hollywood tahmin İzmir ' |
|Builtin.weather.suitable_for | bölümüne gidebilirsiniz `hiking` kısayoluyla bu hafta içinde?   <br/> için yeterince iyi olacaktır `walk` oyuna Bugün? | 
|Builtin.weather.temperature_unit | Sıcaklık bugün nedir `kelvin`   <br/> temps Göster `celsius` |  
|Builtin.weather.time_range | Bu snow gibi olabilseler `tonight`? <br/> Rüzgarlı hangisi `now`?  |
|Builtin.weather.weather_condition | Göster `precipitation` <br/> nasıl kalın olan `snow` şimdi lake tahoe adresindeki?  |

