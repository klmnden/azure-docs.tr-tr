---
title: "Sorun giderme kılavuzu - itme/ulaşma azure Mobile Engagement"
description: "Azure Mobile Engagement'de kullanıcı etkileşimi ve bildirim sorunlarını giderme"
services: mobile-engagement
documentationcenter: 
author: piyushjo
manager: erikre
editor: 
ms.assetid: 3f1886b7-1fdd-47f4-b6b0-d79f158d5ef3
ms.service: mobile-engagement
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: mobile-multiple
ms.workload: mobile
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: ef6f34404b97a6972fc136262920a1bdbc4117b0
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="troubleshooting-guide-for-push-and-reach-issues"></a>İletme ve Erişim sorunları için sorun giderme kılavuzu
Nasıl Azure Mobile Engagement bilgileri kullanıcılarınıza gönderir ile karşılaşabileceğiniz olası sorunlar şunlardır:

## <a name="push-failures"></a>Hataları gönderme
### <a name="issue"></a>Sorun
* Gönderim (uygulamasında, uygulama veya her ikisi de dışında) çalışmıyor.

### <a name="causes"></a>Neden olur.
* Birçok kez gönderme hatası Azure Mobile Engagement, Reach veya başka bir gelişmiş özellik Azure Mobile Engagement'ın doğru şekilde entegre olduğunu veya bir yükseltme ile yeni bir işletim sistemi veya cihaz platformu bilinen bir sorun gidermek için SDK'ın gerekli göstergesidir.
* Yalnızca bir uygulama içinde anında iletme ve bir şey bir uygulama içinde veya dışında uygulama sorunu olup olmadığını belirlemek için yalnızca bir uygulama dışı itme sınayın.
* Kullanıcı Arabirimi hem API hangi ek hata bilgileri kullanılabilir olduğunu görmek için sorun giderme adımı olarak her iki yerde sınayın.
* Azure Mobile Engagement ve Reach SDK'ın tümleşik sürece uygulamanın oturumunu iter çalışmaz.
* Sertifika geçerli değil veya üretim vs kullanıyorsanız iter çalışmaz. Geliştirme doğru (yalnızca iOS). (**Not:** "uygulama oturumunu" anında iletme bildirimlerini iOS, hem geliştirme (Geliştirme) varsa ve aynı cihaza yüklü güvenlik belirteci, sertifikayla ilişkilendirilmiş için uygulamanızı üretim (üretim) sürümlerini Apple tarafından geçersiz dağıtılmamış olabilir. Bu sorunu çözmek için uygulama geliştirme ve üretim sürümlerini kaldırın ve yalnızca bir sürümünü yeniden yükleyin.)
* Uygulama gönderme dışında sayıları farklı farklı platformlarda işlenir (IOS gösterir Android daha az bilgi bir cihazda yerel gönderim devre dışı bırakılırsa, API itme istatistikleri üzerinde kullanıcı arabirimini daha fazla bilgi sağlayabilir).
* Uygulamanın oturumunu iter, işletim sistemi düzeyinde (iOS ve Android) müşteriler tarafından engellenebilir.
* Uygulamanın oturumunu iter doğru tümleşik değildir ancak API'SİNDEN sessizce başarısız olabilir Azure Mobile Engagement Arabiriminde devre dışı olarak gösterilir.
* Azure Mobile Engagement ve Reach SDK'ın tümleşik sürece uygulamada iter çalışmaz.
* Azure Mobile Engagement ve belirli sunucu SDK'ın (yalnızca Android) tümleşiktir sürece GCM ve ADM iter çalışmaz.
* Uygulama ve uygulama dışı iter ayrı ayrı bir itme veya Reach sorun olup olmadığını belirlemek için test edilmelidir.
* Uygulama iter uygulama alınmak üzere açık olmasını gerektirir.
* Uygulamasında iter çoğunlukla bir katılımı veya vazgeçme uygulama bilgisi etikete göre filtre uygulanabilir Kurulum olur.
* Uygulama bildirimleri görüntülemek için erişim özel bir kategori kullanın, doğru yaşam döngüsünü bildirim izlemeniz gerekir, aksi takdirde, bildirim Temizlenmemiş zaman kullanıcı yok sayın.
* Bir kampanya ile Bitiş tarihi başlangıç ve bir cihaz söz uygulama alırsa bildirim ancak mu henüz, kullanıcı hala alacak uygulamaya oturum açtığında bildirim kampanya el ile sona olsa bile görüntüleme.
* Anında iletme API ile sorunları için gerçekten (Reach API'sini daha sık kullanıldığından) anında API Reach API'sini yerine kullanmak istediğiniz olduğunu ve "yükü" ve "bildirim" parametreleri kafa değil onaylayın.
* Anında iletme kampanyanızı aygıtıyla WIFI ve 3 G ağ bağlantı sorunlarının olası bir kaynak olarak ortadan kaldırmak için üzerinden bağlanan her iki test edin.

## <a name="push-testing"></a>Sınama bildirme
### <a name="issue"></a>Sorun
* Gönderim bir cihaz kimliği göre belirli bir cihaza gönderilebilecek

### <a name="causes"></a>Neden olur.
* Test aygıtlar Kurulum her platform için farklı şekilde, ancak tüm platformlar için cihaz Kimliğinizi bulmak için bir test cihazda uygulamanızda olaya neden ve cihaz Kimliğinizi portalında arayan çalışması gerekir.
* Test cihazlarını farklı IDFA vs ile çalışır. IDFV (yalnızca iOS).

## <a name="push-customization"></a>Anında iletme özelleştirme
### <a name="issue"></a>Sorun
* Anında iletme öğesi çalışmayacak içerik Gelişmiş (göstergeye, halka, Titret, resim, vb.).
* Gönderim bağlantılarından (uygulama dışında uygulama içindeki bir konuma bir Web sitesi için uygulama) çalışmıyor.
* Anında iletme istatistiklerini push beklendiği gibi birçok kişi (çok fazla sayıda veya yeterli) gönderilmediğini gösterir.
* Yinelenen ve iki kez alınan iletin.
* Test cihazı Azure Mobile Engagement iter için (kendi üretim ya da geliştirme uygulamayla) kaydedilemiyor.

### <a name="causes"></a>Neden olur.
* Uygulama belirli bir konuma bağlantı sağlamak için "Kategoriler" (yalnızca Android) gerektirir.
* Kullanıcılar bir anında iletme bildirimi tıkladıktan sonra alternatif bir konuma yönlendirmek için derin bağlama şemaları içinde oluşturulan ve uygulamanız ve Mobile Engagement tarafından aygıt işletim sistemi tarafından doğrudan yönetilen gerekir. (**Not:** ile Android olabildiğince uygulamanın oturumunu bildirimleri doğrudan iOS app konumlarda bağlayamazsınız.)
* HTTP "GET" kullanabilmek için gereken harici görüntü sunucuları ve "(yalnızca Android) çalışmaya büyük resmi iter için HEAD".
* Kodunuzda klavye açıldığında Azure Mobile Engagement Aracısı'nı devre dışı bırakabilir ve kodunuzu klavye bildiriminizin (yalnızca iOS) görünümünü etkilemez böylece klavye kapatıldıktan sonra Azure Mobile Engagement Aracısı'nı yeniden etkinleştirin.
* Bazı öğeler test benzetimleri, ancak yalnızca gerçek kampanyaları çalışmıyor (göstergeye, halka, Titret, resim, vb.).
* "Gönderim test etmek için" düğmesini kullandığınızda hiçbir sunucu tarafı veriler günlüğe kaydedilir. Veriler yalnızca gerçek anında iletme kampanyalarını için günlüğe kaydedilir.
* Sorununuzu gidermeye yardımcı olmak için birlikte sorunlarını giderme: benzetimini, test ve bunlar her biraz farklı şekilde çalışır olduğundan gerçek bir kampanya.
* Bir kampanya yalnızca "uygulama" kampanya çalışırken olan kullanıcıların (ve bunların aygıt ayarları "uygulama dışında" bildirimleri alacak şekilde ayarlanmış olan kullanıcıların) teslim edilecek beri çalıştırmak için "uygulama" ve "dilediğiniz zaman" Kampanyalar zamanlanmış süre teslim numaraları efekt.
* Android ve iOS uygulamasının bildirimleri dışında nasıl işleneceğini arasındaki farklar kılar doğrudan uygulamanızın Android ve iOS sürümünü arasında itme istatistikleri karşılaştırmanıza zordur. Android, iOS daha daha fazla işletim sistemi düzeyinde bildirim bilgi sağlar. Yerel bir bildirim alındığında, tıklattınız veya bildirim merkezi aracılığıyla silindi ancak iOS bildirim tıklandığında sürece bu bilgiler bildirmez Android bildirir. 
* "Verilen" numaraları "teslim" numaraları için Kampanyalar ulaşması daha farklı "uygulama" ve "uygulama dışında" bildirimleri farklı sayılır olandan farklı ana nedeni. "Uygulama" bildirimler Mobile Engagement tarafından işlenen, ancak "uygulama dışında" bildirimlerini bildirim merkezi aracılığıyla cihazınızın işletim sistemi tarafından işlenir.

## <a name="push-targeting"></a>Hedefleme bildirme
### <a name="issue"></a>Sorun
* Yerleşik hedefleme işe yaramazsa beklendiği gibi.
* Uygulama bilgileri etiketi hedefleme beklendiği gibi çalışmıyor.
* Coğrafi konum hedefleme beklendiği gibi çalışmıyor.
* Dil Seçenekleri beklendiği gibi çalışmıyor.

### <a name="causes"></a>Neden olur.
* Uygulama bilgileri etiketleri Azure Mobile Engagement UI veya API aracılığıyla karşıya yüklediğiniz emin olun.
* Gönderim hızı veya uygulama düzeyinde gönderim kotası azaltma ya da İzleyici kampanya düzeyde sınırlandırma bir kişi diğer hedefleme ölçütlerinize uyan olsa bile belirli bir itme almasını engelleyebilirsiniz. 
* "Language" ayarı hedefleme ülkeye göre tabanlı veya coğrafi konuma göre de hedefleme daha farklı olan yerel bir telefon konumu veya GPS konumuna göre farklıdır.
* "Varsayılan dilde" iletisi cihazlarını, belirttiğiniz diğer dillere birine ayarlayın sahip olmayan herhangi bir müşteriye gönderilir.

## <a name="push-scheduling"></a>Zamanlama bildirme
### <a name="issue"></a>Sorun
* Anında iletme zamanlama beklendiği gibi (çok erken veya Gecikmeli gönderilen) çalışmıyor.

### <a name="causes"></a>Neden olur.
* Saat dilimleri özellikle son kullanıcının saat dilimi kullanırken, zamanlama, sorunlar olabilir.
* Gelişmiş gönderme özellikleri iter gecikmeye yol açabilir.
* Azure Mobile Engagement bir itme göndermeden önce telefon gerçek zamanlı veri istemek sağlanmış olabileceği (yerine uygulama bilgisi etiketleri) ayarları geciktirebilir telefonla üzerinde hedefleme iter.
* Bitiş tarihi oluşturulan Kampanyalar itme cihazda yerel olarak depolamak ve kampanya el ile sona erdi olsa bile uygulama sonraki açılışında gösterir.
* Aynı anda birden fazla kampanya başlangıç kullanıcı tabanınızı (eski kullanıcıların taranması gerekmez yalnızca bir kampanya aynı anda en fazla dört, ile de yalnızca etkin kullanıcılar için hedef başlatın deneyin) tarama uzun zaman alabilir.
* Kampanya otomatik olarak göndermez, Reach kampanya "Kampanyası" bölümünde "Yoksay İzleyici API aracılığıyla kullanıcılara bir gönderim" seçeneğini kullanırsanız, el ile ulaşmak API üzerinden göndermek gerekir.
* Uygulama bildirimleri görüntülemek için erişim özel bir kategori kullanın, doğru yaşam döngüsünü bir bildirim izlemeniz gerekir, aksi takdirde, bildirim Temizlenmemiş zaman kullanıcı yok sayın.

