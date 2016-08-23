<properties 
    pageTitle="Azure Mobile Engagement En İyi Uygulamalarla Başlangıç Kılavuzu"
    description="Ekleme için En İyi Uygulamalarla Azure Mobile Engagement Başlangıç Kılavuzu" 
    services="mobile-engagement" 
    documentationCenter="mobile" 
    authors="wesmc7777"
    manager="erikre"
    editor=""/>

<tags
    ms.service="mobile-engagement"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="mobile-multiple"
    ms.workload="mobile" 
    ms.date="07/07/2016"
    ms.author="wesmc;ricksal"/>

# Azure Mobile Engagement - En İyi Uygulamalarla Başlangıç Kılavuzu

## Genel Bakış

**Mobil ekran çok kalabalık bir alandır:** 2013'te yapılan bir çalışma ortalama bir mobil cihazda 27 uygulama yüklü olduğunu ortaya çıkardı. Kullanıcılar yükledikleri uygulamalarda genelde aylık 30 saat vakit harcıyor. Bu sürenin çoğunu sosyal ağlarda ve oyun oynama amacıyla harcanıyor (yaklaşık 20 saat). 2014 yılına gelindiğinde, Android markette kullanıcıların seçebileceği yaklaşık 1,5 milyon uygulama bulunuyordu. Apple mağazası yaklaşık 1,2 milyon uygulama içeriyordu. Geliştiricilerin bu büyüyen pazarda rekabeti devam ederken mobil uygulama kullanımı hala artmaktadır. 

Ortalama bir mobil kullanıcı değişen ilgi alanları ve uygulama içi deneyimlere bağlı olarak yüksek sıklıkta uygulama yüklemekte ve kaldırmaktadır. Bir uygulamanın başarısını belirlemek için uygulamanızı kaç kişinin yüklediğinin ötesindeki şeyleri bilmek önemli hale gelmiştir. Uygulamanızın ne kadar faydalı olduğunu ve kullanım eğiliminin değişip değişmediğini bilmek önemlidir. Aşağıdaki sorular önemli hale gelmiştir:

- Kullanıcılarınız uygulamanızı ilgi çekici olmayan ya da modası geçmiş olarak mı görmeye başladı? 
- Toplamda kaç kullanıcı uygulamanızı kullanmayı bıraktı? 
- Uygulama içi satın alma eğilimi artıyor mu yoksa azalıyor mu?
- Kullanıcılar uygulama ile ilgili sorunlar veya ilgi eksikliği nedeniyle iş akışlarını tamamlayamıyor mu? 
- Kullanıcı tabanınıza yeni içerik ekleyerek uygulamanızı faydalı ve ilgili tutmaya devam etmeniz mümkün mü? 
- Bu yeni içerik tüm kullanıcılar için aynı mı yoksa uygulamanızdaki davranışa göre kullanıcı kesimlerine mi odaklanıyor? 
 
Bunlara benzere soruları yanıtlamak uygulamanızın ömrünü ve gelirini sürdürmeye yardımcı olabilir. Bunlar kullanıcı tabanınızı tanımlamanıza ve tutmanıza da yardımcı olur. 

Medya ile ilgili uygulamalar kullanıcılar arasında en yüksek elde tutulma eğilimine sahip olanlardandır. Bunun bir nedeni bunların kullanıcılara sürekli yeni içerik sağlamasıdır. Bir kullanıcı kesimine yönelik kullanışlı anında iletme bildirimlerinin erken uyarlanması uygulama elde tutma üzerinde yüksek etkide bulunma eğilimlidir. 

Azure Mobile Engagement programı uygulamanızın kullanımına ilişkin ayrıntılı bilgiler toplamak ve çözümlemek üzere bir yöntem sağlayarak uygulamanızın ömrünü ve elde tutulmasını uzatmaya yardımcı olmak için tasarlanmıştır. Davranışa göre kullanıcı tabanınızı sınıflandırmaya yardımcı olur ve belirlenen kullanıcı kesimlerine anında iletme bildirimleri ve uygulama içi iletiler göndermek için odaklı kampanyalar oluşturur. Önemli Performans Göstergeleri (KPI) kullanıcılarınızın uygulamanızın farklı yönlerinde nasıl etkin olduğunu ölçer. Azure Mobile Engagement bu KPI'leri belirlemek için gereken yöntemleri sağlar. Mobil uygulamanız katılımı artırmak için size gereken altyapıyı sağlayarak yatırım getirinizi (ROI) yükseltmeye yardımcı olur. 

Azure Mobile Engagement’tan en iyi şekilde faydalanmak için, iyi tasarlanmış bir katılım planıyla başlamanız gerekir. Planınız kullanıcı tabanınızı kesimlere ayırabilmek üzere size gereken parçalı verileri belirlemenize yardımcı olacaktır. Bu davranışı ya da uygulama için deneyimleri temel alabilir. Planınızın başarılı olması için, uygulamanızın hedeflerini ölçecek KPI’i net şekilde belirlemek en iyi uygulamadır. Tanımlanan net performans göstergeleriyle, KPI’lerinizi çözümlemek ve değerlendirmek için kullanacağınız ayrıntılı verileri toplamak üzere gerekli mantığı uygulamanıza kolayca ekleyebilirsiniz. Bu konu, katılım planınızla kullanacağınız KPI’leri tanımlamaya ilişkin en iyi uygulamadır. 


## 1. Adım: BET modeline uyacak şekilde KPI’inizi tanımlama


KPI’lerin doğru şekilde tanımlanması tamamlanması zor bir görev olabilir. Farklı sektörler için tasarlanan uygulamalar kendi özelliklerine ve hedeflerine sahiptir. Bu durum yaklaşımınızı karmaşık hale getirmeye eğilimlidir. Bundan kaçınmak için, hedefler ve KPI'ler üç ana kategoride sınıflandırılmalıdır: **İş**, **Katılım** ve **Teknik**. Buna **BET modeli** adını veririz.

İyi bir plan genellikle BET modelinin aşağıdaki her bir kategorisindeki başarıları ölçene KPI’lerle hedeflere sahip olur. 


#### İş KPI'leri

İş KPI'leri oluşturması en kolay kısımdır. Büyük olasılıkla mobil uygulamanızı planlarken bunu bir biçimde zaten tanımlamıştınız. Bu KPI’ler genellikle uygulamanız için geliri ve yatırım getirisiniz ölçmeye yardımcı olur. Aşağıdaki listede, performans göstergelerinizi tanımlarken size yol gösterebilecek örnek bazı İş KPI’leri verilmiştir:

- Medya İş KPI'leri
    - Tıklanan reklam sayısı
    - Kullanıcı başına sayfa ziyareti sayısı
    - Geçerli abonelik sayısı
- Oyun İş KPI'leri
    - Uygulama içi satın alma sayısı
    - Kullanıcı başına ortalama gelir (ARPU)
    - Oturum başına harcanan süre
    - Oynanan gün sayısı ve geçerli oyun düzeyi
- E-ticaret İş KPI'leri
    - Uygulamanın kullanıldığı gün sayısı
    - Kullanıcı başına ortalama gelir (ARPU)
    - Satın alma sırasında sepetteki ortalama tutar
    - En çok görüntülenme ve satın alma için ürün kategorisi
- Banka ve sigorta İş KPI'leri
    - Hesap sayısı
    - Etkinleştirilen özellikler
    - Ziyaret edilen teklif sayfaları
    - Tıklanan veya etkinleştirilen uyarı sayısı      



#### Katılım KPI'leri

Katılım KPI’si kullanıcılarınızın katılımını ölçmeye yönelik bir performans göstergesidir. Bu alandaki eğilimler uygulamanızın elde tutulmasını belirlemeye yardımcı olur. Burada bu tür KPI için bazı örnek performans göstergeleri verilmiştir:

- Son 7 gün içindeki etkin kullanıcı sayısı
- Son 7 gün içindeki etkin olmayan kullanıcı sayısı
- Uygulamayı son 30 günde kullanmayan kullanıcı sayısı  

Bazı bariz dış etkenler bu alandaki göstergeleri etkileyebilir. Örneğin, bir mobil cihazın her zaman kullanıcıyla birlikte olduğunu düşünebilirsiniz. Bu doğru olabilir ya da olmayabilir. Bir oyuncu iş dışı ve okuldan sonraki saatlerde daha fazla oynadığından bir oyun uygulaması tatillerde daha yüksek kullanıma sahip olma eğilimli olabilir.   

Bu kategorideki iyi tanımlanmış KPI'ler uygulamanız ve müşterileriniz arasındaki ilişkiyi ölçmenize yardımcı olmalıdır.



#### Teknik KPI'ler

Bu kategorideki performans göstergeleri uygulamanızın doğru mu davrandığı, askıda mı yoksa çökmekte mi olduğunu belirlemenize yardımcı olur. Bu göstergeler uygulamanızın durumunu ölçebilir ve kullanıcıların uygulamayı kullanmasını önleyebilecek kullanılabilirlik sorunlarını belirleyebilir. Bu kategori için toplanan bilgiler, pazarlama ekiplerini ilgilendirebilecek performans bilgilerini de içerebilir. Verileri ayrıca bildirilmeyen hataların belirlenmesine yardımcı olmak üzere BT ve destek ekipleri tarafından sorun giderme için de faydalı olabilir. 
 
Bazı Teknik KPI örnekleri şunlardır:

- İşlenen veya işlenmeyen özel durum bilgileri ve sayısı 
- Son çökme için zaman damgası
- Tıklanan son düğme ya da ziyaret edilen son sayfa
- Uygulamanın bellek kullanımı
- Uygulama kare hızı
- Uygulamanın üzerinden çalıştığı işletim sistemi sürümü
- Uygulama sürümü

Uygulama performansını ölçmeye ve potansiyel hataları bulmaya yardımcı olması için bu KPI’leri tanımlayın. Bu göstergeler müşterilerinize bir düzeltme sağlamak için gereken süreyi azaltmaya yardım etmelidir. Bunlar ayrıca belirli sorunlarla karşılaşan kullanıcı kesimini belirlemenize de yardımcı olabilir. Bu kullanıcı kesimlemesini müşteri memnuniyetini yeniden sağlamaya yardımcı olması amacıyla kullanılabilir düzeltmeler ve potansiyel promosyonlarla ilgili bildirimler göndermek üzere kampanyalar oluşturmakta kullanın. 


#### Playbook Alıştırması 1: KPI panonuzu oluşturma

Pazarlama stratejinizi tanımlarken, KPI'leriniz her bir ana hedefiniz için bir görünüm sunmalıdır. Bunlar uygulamanızı ve son kullanıcı davranışı izlemek üzere önemli bilgileri toplamanıza olanak tanıyan net tanımlanmış veri noktaları olmalıdır.

Aşağıdaki bilgileri içeren bir KPI panosu oluşturun

1.  Uygulamaya yönelik KPI'ler nelerdir?
2.  Her bir KPI’i göstermek için hangi veri noktalarını kullanacağım?
3.  Bu veriler uygulamamda nerede yer alıyor (örn, ekran, ayarlar, sistem...)?
4.  Bu KPI için Katılım sırası yürütebilir miyim?

Örnekler ve yönergeler için [Media Playbook Şablonumuzdaki][Media Playbook link] **KPI Builder** çalışma sayfasını kullanabilirsiniz.



## 2. Adım: Katılım Programınız


Mükemmel bir mobile engagement programı uygulamanız için temel bileşen kabul edilmelidir. Bu kesinlikle, uygulamanızın ilk kullanım günü kullanıcıya yürütülen harika bir karşılama programı içermelidir. Bu, uygulamanızın katılımı ve el de tutulmasında çok olumlu bir etki bırakma eğilimlidir. Çalışmalar, kullanıcıların çoğunluğunun yükledikten birkaç gün sonra uygulamayı kullanmayı bıraktığını göstermektedir. Müşteri uygulamanıza odaklanmaya devam ederken, müşteri beklentisi yönlendirmeli ilgiliyi erkenden karşılamak ya da aşmak için mücadele etmek istersiniz. Müşterilerinize uygulamanızın temel değerini ve avantajlarını sergilediğinizden emin olun. 


![](./media/mobile-engagement-getting-started-best-practices/unsegmented-push-notifications.png)

Mobil cihaz kullanıcılarıyla erken katılımlar için anında iletme bildirimleri en iyi yaklaşımdır. Ancak, anında iletme bildirimleri için kullanıcılar kesimlenirken çok dikkatli olunmalıdır. Çünkü bir kullanıcının istenmeyen posta ya da ilgilenmediği bildirimler aldığını düşünmesinin ciddi etkileri olabilir. Birkaç tıklama ile, kullanıcı asla geri dönmemek üzere uygulamanızı silebilir. Kullanıcı, genel istenmeyen posta yerine yüksek oranda kişiselleştirilmiş uygulama içi değer almalıdır.

Kullanıcılar etkin şekilde katıldığında, katılım programının uygulamanızın diğer yönlerini yönlendirmeye yardımcı olabilir.

Örneğin, etkin kullanıcılarınızdan uygulamanızı değerlendirmelerini isteyen bir kampanya düzenleyebilirsiniz. Bu kullanıcı kesimi uygulamanızla ilgili olarak en etkin ve en deneyimli olduğundan, bunlardan en doğru puanı almayı bekleyebilirsiniz. Yüksek uygulama puanı aldığınızda, bu uygulamanızın doğal indirilmesini artırmanın yanı sıra yeni müşteri edinme maliyetlerini azaltmaya da yardımcı olabilir.



#### Katılım Sırası


Genel bir Katılım programı farklı katılım sıraları içerir. Her bir sıra çeşitli hedeflere ulaşmayı amaçlar.


###### Ömür anında iletme sırası


Ömür anında iletme sırasının hedefleri kullanıcının uygulamaya katılımının yaşam döngüsüne bağlı olarak farklıdır. Belirli bir kullanıcı yeni, etkin olmayan veya çok etkin olabilir. Katılım yaşam döngüsünün farklı aşamalarında, kullanıcılar ipuçları ve belgelere bağlantılar biçiminde yeni içeriğinizde faydalanabilir. 

Örneğin yeni bir kullanıcı, uygulamayı ilk başlattığındakine benzer şekilde, uygulama için yol gösterilmesine ya da yeni bir kullanıcı teşvikinden yararlanmaya ihtiyaç duyabilir.

*“Dahil olmanızdan memnunuz!” “İlk ayınızda oturum açmanın ücretsiz olduğunu unutmayın!”*


###### Davranışsal anında iletme sırası

Davranışsal iletme sırası, uygulama için toplanan kullanıcı davranışı temelinde kullanımı artırmayı amaçlar.  

Örneğin, fantezi futbol uygulamasının etkin bir kullanıcısı aşağıdaki anında iletme bildirimiyle katılımdan faydalanabilir...

*"John gerçek bir futbol taraftarısın!” NFL bölümümüzde oturum aç ve SuperBowl’a ücretsiz erişim kazan"*


###### Uyarı anında iletme sırası

Kullanıcılar kendi ilgi alanlarına odaklanan ilgili haberlerden hoşlanacaktır. Uyarı anında iletme sırası, kullanıcının açık olarak gösterdiği ilgi alanları temelinde uyarıla göndererek katılımı artırır. Bu, kullanıcı uygulamada kendi ilgili alanlarını seçtiğinde belirgin hale gelebilir. Bu ayrıca kullanıcının uygulamayla etkileşimi sırasından toplanan veriler temelinde açıkça belirlenir.

Örneğin, bir E-Ticaret uygulaması kullanıcısı belirli bir marka kahveyi düzenli olarak satın alabilir, siz de bunu iş KPI’si ile yakalamışsınızdır. Aşağıdaki uyarı bu kullanıcının uygulamaya katılımını artırabilir.
 
*"Merhaba Wes, en sevdiğin kahve markalarından biri Eylül 2015’in ilk haftasında %25 indirimde olacak. Müşterimiz olmandan memnunuz ve sana haber vermek istedik."*

###### Elde tutma anında iletme sırası

Bu sıra düzenli şekilde uygulamaya katılım alışkanlığı sağlamaya yardımcı olmak üzere tekrarlayan anında iletme bildirimleri kullanarak kullanıcıları tutmayı amaçlar. Bu, kullanıcı etkileşimlerden hoşlanırsa, uygulama elde tutmayı artırmaya yardımcı olabilir. 

Örneğin, sporla ilgili bir uygulama kullanıcısı kullanıcının desteklediği takımlar temelinde haftalık olarak aşağıdaki anında iletme bildirimini alabilir:

*“200 puan kazanma şansı için, bu hafta Toronto Blue Jays’e karşı New York Yankees’in kazanıp kazanamayacağına oy verin!”*


#### 3W yaklaşımı

Farklı anında iletme sıralarını kullanmak son kullanıcılarla katılımınıza yardımcı olur. Ancak, bildirimlerinizi kişiselleştirmek yine de 3W yaklaşımını kullanmanız gerekir. 3W yaklaşımı her bir bildirim için Kim, Ne ve Ne Zaman sorularıyla ilgilenmelidir. Bu üç sorunu yanıtını tatmin edici şekilde karşıladığınızda, bildirimleriniz katılıma uygun şekilde odaklanmış olur.

![](./media/mobile-engagement-getting-started-best-practices/who-what-when.png)



###### Kim: İletileri alacak kullanıcı kesimi.

Kullanıcılarınıza gönderilen anında iletme bildirimleri çok hassas bir iletişim kanalı olarak düşünülmelidir. Bir kullanıcı kesimine göndermeyi amaçladığınız bildirimlerin bu kullanıcı kesiminin ilgi alanlarına yönelik kapsamının iyi belirlendiğinden emin olun. Yanlış yönlendirilmiş bir bildirimin kullanıcı üzerinde olumsuz etkisi olması büyük olasılıktır. Bu kullanıcıların bildirimi istenmeyen posta olarak değerlendirmesi uygulamanızın kaldırılmasına yol açabilir. 

İletileri alacak kullanıcı kesimlerini tanımlarken belirli teknik ve davranışsal ölçütlerin birleşimini kullanın. Bir kullanıcı kesimini tanımlamanın basit bir örneği aşağıdaki deyime benzer olabilir:

“Bir mobil uygulamayı 3 gün önce ilk kez başlatan ve oturum açma işlemlerini tamamlamadan oturum açma sayfasını iki kez ziyaret eden tüm kullanıcılar”.
 
Bu deyim belirli bir senaryoyu desteklemek üzere toplamanız gereken verileri tanımlamanıza yardımcı olur.


###### Ne: Göndereceğiniz ileti

**Ton**

Katılımlarınızda, kesimlenmiş kullanıcılarınız için uygun olarak bir ton kullanın. Bu, kesinlikle sonu kullanıcılarınızla iletişim kurmanın ve kullanıcının uygulamanıza ilgisini artırmanın iyi bir yoludur. 

**Yönlendirme**

Anında iletme bildirimi uygulamayı açmaktan fazlası için kullanılabilir. Bildirim iletisi, haber yayınlama veya ürün tanıtımı gibi bir bağlam sağlarsa, bu bildirim uygulamadaki doğru içeriğe doğrudan bağlantı sağlayabilir. Bunu desteklemek için, uygulamanın yönlendirmeyi yönetmesine izin vermek amacıyla bir URL şeması oluşturmalısınız. Katılım sıralarınızla çalışırken, bu unutulmaması gereken bir adımdır.

Yönlendirme diğer sistemler için de kullanılabilir. Örneğin, bir Eylem URL’si ile sonu kullanıcıları aşağıdakiler dahil birçok diğer sisteme yönlendirmek mümkündür:

- Web sitesi
- E-posta önceden ayarlı posta kutusu
- SMS kutusu
- Arama hizmeti
- Uygulama derecelendirme için doğrudan uygulama mağazasına. 

Bu, son kullanıcıların katılımı ve performans iyileştirmek üzere otomatik kurallar oluşturmak için birçok fırsat sağlar.


**Biçim/İçerik**

Farklı türler ve Anında iletme bildirimi biçimleri:

1. **Duyuruları** : kullanıcılara farklı anlarda reklam iletileri göndermenizi sağlar (uygulama dışında, uygulama içi ya da herhangi bir zamanda).
2. **Anketler** : kendilerine sorular sorarak son kullanıcılardan bilgi toplamanızı sağlar. Bu yanıtlar daha sonra son kullanıcıları hedeflemek üzere ölçüt oluşturmakta kullanılır.
3. **Veri Gönderimleri** : uygulamayı güncelleştirmek için ikili veya base64 veri dosyası göndermenizi sağlar. veri gönderiminde yer alan bilgiler kullanıcıların uygulamanızdaki deneyimini kişiselleştirmek için uygulamaya gönderilir. Uygulamanız veri gönderimindeki verileri desteklemek üzere tasarlanmalıdır.
4. **Kutucuklar (yalnızca Windows Phone)** : XML Verileri içeren Yerel Gönderim Bildirimi göndermek için Microsoft Anında İletme Bildirimi Hizmeti’ni (MPNS) kullanmanızı sağlar (SDK sürüm 0.9.0’dan itibaren desteklenir. Kutucuklar için yük 32 kilobaytı aşamaz.). İleti doğrudan pano kutucuğunuzda görünür.
5. **Web görünümü** : Web görünümü web içeriği içeren açılır penceredir. Bu açılır pencere, son kullanıcı anında iletme bildirimine tıkladığında açılır. Web görünümü son kullanıcı ile daha fazla etkileşime girmenizi sağlar.
 
>[AZURE.NOTE] Anında iletme bildirimleri olarak gönderdiğiniz içeriğin, ilgili platformun (göndermeye, Android, Windows) uygulama geliştirme ve anında iletme bildirimi göndermeye ilişkin yönergelerine uygun olduğundan emin olun.

 


###### Ne Zaman: Kampanyanızın zamanlaması

Anında iletme bildirimini tetikleyen bir kampanyayı etkinleştirmek için en iyi zaman hangisidir? Bu el ile mi yoksa otomatik mi olmalı? Bu yinelenmeli mi? En iyi sonuçları elde ederek kullanıcılarla etkileşim için doğru zamanı ve sıklığı belirlemek önemlidir. Her katılım sırası ve senaryosu için, anında iletme bildirimi göndermenin en iyi zamanını belirtmelisiniz. Bazı olası örnekler şunlardır:

![](./media/mobile-engagement-getting-started-best-practices/campaign-timing-examples.png)

Günlük olarak birçok bildirim gönderiyorsanız, kullanıcılarınızın gönderdiğiniz iletişimleri istenmeyen posta olarak algılayabileceğini dikkatle değerlendirmelisiniz. 

Azure Mobile Engagement gönderdiğiniz iletişimlerin istenmeyen posta olarak algılanmasını önlemeye yardımcı olmak için iki yol sunar: İlk olarak,aynı kullanıcıları hedeflemediğinizden emin olmak için ayrıntılı kesimleme kullanın. Bunun yanı sıra, Azure Mobile Engagement bir "kota" özelliği de sağlar. Bu özellik bir kampanya için gönderilen bildirimleri sınırlayabilir. Örneğin, varsayılan kotanın haftada 5 olarak ayarlanması, kampanya kullanıcı kesiminin parçası olarak eklene bir kullanıcının haftada 5 bildirimden fazlasını almaması anlamına gelir.





#### Playbook Alıştırması 2: Kendi katılım programınızı oluşturma

Hedeflerinizi özetlemeye ve belirli sıraları kullanarak yürütmek istediğiniz kampanyaları tanımlamaya biraz zaman ayırın. Kampanyalarınızdaki bildirimlere 3W yaklaşımı uyguladığınızdan emin olun. 

Örnekler ve yönergeler için [Media Playbook Şablonumuzdaki][Media Playbook link] **Katılım Programı** çalışma sayfasını kullanın.


## 3. Adım: Uygulama Tümleştirmesi


#### Bir etiket planı oluşturma

Azure Mobile Engagement’ı uygulamanıza tümleştirmek için bir etiket planı oluşturmanız gerekir. Etiket planı projenin köşe taşıdır. Bu, pazarlama özellikleri,uygulamanın iş akışı ve KPI’leri ölçmek üzere uygulamada toplanan gerçek etiket verileri arasındaki ilişkiyi tanımlar. Bu, portalda hangi analizleri görebileceğinizi gösterir. Bu ayrıca kullanıcı kesimlerini tanımlamanıza ve son kullanıcılarınızla etkileşim için odaklı anında iletme bildirimleri göndermenize yardımcı olur. Etiketi planını tanımladığınızda, uygulamanıza tümleştirmek üzere kod eklemek Azure Mobile Engagement SDK’yı kullanarak kolaydır.

Bir etiket planı uygulamadaki her şeyi etiketlememelidir. Yalnızca mobile engagement stratejinizin bir parçası olarak etiket verilerini içermelidir. Bu büyük olasılıkla uygulamalar arasında farklı olacaktır. Azure Mobile Engagement tarafından sağlanan [Media Playbook Şablonu][Media Playbook link] belirtilen yöntemle bir etiket planı oluşturmanıza yardımcı olur. **Etiketi Planı** çalışma sayfasını etiketi planınızı oluşturmak için bir kılavuz olarak kullanın.

Çalışma sayfasında bir etiket bölümü tanımlarken, çok açık olun. Bu, karışıklığı önlemek için çok önemlidir. Her bir etiketin gönderileceği her senaryoyu ayrıntılı olarak açıklayın. Her bir etiketin ekleneceği etkinliği adını ekleyin. Bunların tümü çalışma sayfasının **Bilgi veren** kısmına eklenmelidir. Etiket planı çalışma sayfası test doğrulama için ana referans olmalıdır. 

**Toplanacak veri** bölümünde, geliştirme ekibiniz uygulamaya eklenecek her bir etiket için gereken türler, adlar, değerler ve fazla bilgi anahtar/değer çiftlerini bulmalıdır.

Projeyle ilişkili tüm ekiplerle Etiket planının gözden geçirilmesini öneririz. Gerekli düzeltmeleri yapın ve pazarlama ve geliştirme ekipleri için her şeyin net olduğunu onaylayın.

**İş bildirimi** çalışma sayfası projeye dahil herkes için yardımcı kılavuz olarak kullanılabilir.


#### Veri Türleri

Bunlar Azure Mobile Engagement tarafından desteklenen ortak veri türleridir.

###### Cihazlar ve kullanıcılar

Azure Mobile Engagement, her cihaz için benzersiz bir tanımlayıcı oluşturarak kullanıcıları tanımlar. Bu tanımlayıcıya, cihaz tanımlayıcısı (veya deviceID) adı verilir. Bu, aynı cihazda çalışan tüm uygulamaların aynı cihaz tanımlayıcısını paylaşmaları şeklinde oluşturulur.

###### Oturumlar ve etkinlikler

Oturum, kullanıcı tarafından çalıştırılan uygulamanın bir örneğidir. Oturum kullanıcının uygulamayı başlattığı zamandan durdurduğu zamana uzanır.

Etkinlik, bir uygulamanı oturum sırasında yapabileceği şeyler grubunun mantıksal bir gruplamasıdır. Bu, genellikle uygulamada belirli bir ekrandır ancak uygulama mantığı tarafından tanımlanan her şey olabilir. En azından, uygulamanız için her ekranı veya Etkinliği etiketlemelisiniz. Bu, kullanıcı yolunu anlamanıza olanak tanır.


###### Olaylar

Olaylar kullanıcının uygulamayla etkileşimini bildirmek için kullanılır. Bunlar, içerik paylaşımı ya da bir video yürütme gibi anlık eylemler olabilir. Olayları etiketlemek, size kullanıcıların uygulamayla etkileşimini gösteren veri koleksiyonları sağlar. 

###### İşler

İşler süresi olan eylemleri bildirmek için kullanılır. Bazı örnekler aşağıdakileri içerir:

- API çağrılarının yürütülmesi
- Reklamları görüntülenme zamanı
- Arka plan görevleri süresi 
- Satın alma işlemi süresi
- Video görüntüleme


###### Hatalar

Hatalar uygulama tarafından algılanan sorunları bildirmek için kullanılır. Örneğin, yanlış kullanıcı eylemleri veya API çağrısı hataları.

###### Uygulama bilgileri

Uygulama bilgileri (Uygulama Bilgisi) kullanıcının uygulamayla olan deneyimine ilişkin verileri etiketlemek için kullanılır. Kullanıcının uygulamayla olan etkileşimiyle oluşturulur. 

Belirtilen uygulama bilgisi anahtarı için, Azure Mobile Engagement yalnızca en son değeri izler (geçmiş yok) Uygulama bilgisi uygulamanızın veya son kullanıcılarınızın durumunu gösterir. Örneğin oturum açma durumu veya kullanıcının sık kullanılan ürün grubu.

###### Çökme verileri

Mobile Engagement SDK’sı tarafından otomatik olarak toplanan çökme verileri, uygulama tarafından işlenmeyen uygulama hatalarını bildirir. Örneğin, meydana gelen işlenmeyen özel durum


###### Ek veriler

Parametrelerle geliştirilebilen olaylar, hatalar, etkinlikler ve işler. Bir geliştiricinin uygulamadan alınan belirli veriler olarak sağlayabileceği ek bilgilerdir. Bu ayrıntılı kesimleme tanımlaması için önemlidir. 

Örneğin, bir "makale" etiketi değeri, bu makaleyi görüntüleyenler temelinde son kullanıcıları kesimlemenizi sağlar. Ancak bu yeterli olmayabilir. Bu aynı “makale” etiketine, etkinlikte “news_category” gibi ekstra bilgi eklenirse daha iyi olabilir. Bu kullanıcı için sık kullanılan kategorileri dinamik olarak belirlemekte yararlı olacaktır. 

Ek bilgiler bir anahtar/değer çifti olarak bildirilir. Bu medya uygulaması örneğinde, “news_category” ek bilgileri bu kategori için değer olabilir. Örneğin, "spor", "ekonomi" veya "siyaset".





#### Etiket ve SDK tümleştirmesi 

Azure Mobile Engagement SDK’sını uygulamanıza tümleştirmenin adım adım yönergeleri için, Azure web sitesinde [Engagement SDK’sı Tümleştirmesi](mobile-engagement-windows-store-integrate-engagement.md) belgelerini izleyin. Bu sayfanın üstündeki bağlantılardan hedef platformunuzu seçin.

Azure Mobile Engagement’ın en üstünde oluşturulan iki uygulama için proje oluşturmanızı öneririz. Biri geliştirme ve test hazırlığı, diğeri de üretim hazırlığı içindir. Kullanıcı kabul testi başarılı olduğunda, BT ekibiniz test hazırlığından üretim hazırlığına geçebilir.



#### Kullanıcı kabul testi (UAT)

Kullanıcı kabul testi (UAT) her şeyin tasarlandığı gibi çalıştığından emin olmayı içerir. İş akışları tamamlanabilir ve etiket planınız temelinde gerekli tüm veriler toplanır:
 
- Bilgi etiketleme belgelerdeki AZME kavramlarına göre uygulanmalıdır
- Gereksinim duyduğunuz tüm bilgiler toplanır (Ek bilgi değeri, Uygulama bilgisi değeri dahil)
- Adlandırma Etiket Planı’na göre eşleşir
- Gönderilen yinelenen etiket yok

Uygulamanıza eklediğiniz tüm bildirim davranışı türlerini baştan sona test edin

- Uygulama dışı ve uygulama içi Duyurular, Anketler, veri gönderimleri
- Metin/Web görünümleri
- Gösterge güncelleştirme, Kategoriler



#### Kurulum

Azure Mobile Engagement kurulumu çok kolaydır. Kullanıcı arabirimine ilişkin tüm bilgiler Azure Mobile Engagement web sitesinde, [Kullanıcı arabiriminde gezinme](mobile-engagement-user-interface.md) başlığında yer alır.

Projenizin kullanıcıları için doğru rolleri ve rol üyeliklerini ayarlayarak başlamanız önerilir. Bu, tüm kullanıcılar için platforma düzgün erişimi yönetmenize yardımcı olur. Rolleriniz şunları içerebilir:

- Yöneticiler
- Geliştiriciler
- Görüntüleyiciler 

Daha sonra:
- Kendi cihazınızda test etmek için deviceID’nizi kaydedin.
- Hesap ayarlarınıza gidin ve saat diliminize ilişkin grafiklerin ve bildirim gönderme zamanının ayarlanması için saat dilimini ayarlayın.
- Uygulama ayarlarınıza gidin ve Reach dahilindeki son kullanıcıyı hedeflemek için gereken “Uygulama bilgisini” kaydedin.

İlk anında iletme bildirimi kampanyanızı nasıl çalıştıracağınız hakkında daha fazla bilgi için [Son kullanıcılarınıza ulaşmak için anında iletileri kullanmaya ve yönetmeye başlama](mobile-engagement-how-tos.md)’yı gözden geçirin.



## Sonuç


Katılım Programları yinelemelidir ve uygulamanız için en çok işe yarayan deneyiminizle kendinizinkini sürekli geliştirmelisiniz. 

Başlangıçta, katılım stratejileriyle deneyim kazanırken bütün bir genel katılım stratejisi oluşturmaya çalışmayın. KPI’lerinizi ve bunları nasıl kullanacağınızı tanımlayarak adım adım bir yaklaşım benimseyin. Katılım stratejisi her uygulama için benzersiz olacaktır.

Biraz deneyim kazandıktan sonra, katılım programlarınıza aşağıdakileri eklemeyi düşünebilirsiniz:

- İzleme: Kullanıcılar alır ve büyük olasılıkla veri koleksiyonu kaynakları tanımlarsınız. Azure mobile Engagement, veri koleksiyonu kaynaklarına bağlanabilir. Bu, her kaynağında performansını izlemenizi sağlar. Bu bilgiler edinme yatırımınızı en üst düzeye çıkarmayla ilgilidir. 

- A / B testi: Bu, Katılım programının temel bir parçasıdır. Her uygulamanın kendi özellikleri vardır. A/B testi ile, katılım programınızı geliştirebilirsiniz.

- Coğrafi konum: Bu, markalar için büyük bir fırsattır. Bu özellik sayesinde doğru yerde ve zamanda ulaşabilirsiniz. Coğrafi konum özelliğini kullanmaya başlamadan önce yeterli son kullanıcı davranışı verisi topladığınızı doğrulamanızı öneririz.

- Veri gönderimi: Veri gönderimi görünmez bir anında iletimdir. Veri gönderimi sonu kullanıcı davranışı temelinde uygulamanızı özelleştirmenize olanak tanır. Örneğin, bir kullanıcı kesimi genellikle yüksek teknoloji ürünlere başvuruyorsa, uygulama sahibi kendi giriş sayfasını yüksek teknoloji içeriğiyle özelleştirecek bir veri gönderimi gönderebilir.



## Sonraki Adımlar

- [Azure Mobile Engagement hesabı oluşturma](mobile-engagement-create.md).
- Mobile Engagement stratejinizi tanımlama hakkında daha fazla bilgi için [Mobile Engagement stratejinizi tanımlama](mobile-engagement-define-your-mobile-engagement-strategy.md)’yı ziyaret edin



  

<!--Image references-->


<!--Link references-->
[Media Playbook link]: https://github.com/Azure/azure-mobile-engagement-samples/tree/master/Playbooks



<!--HONumber=Aug16_HO1-->


