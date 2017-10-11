---
title: "Windows 10 Dolaşım ayarları başvurusu | Microsoft Docs"
description: "Çıkan veya Windows 10'da yedeklenen tüm ayarların tam bir listesi."
services: active-directory
keywords: "Kurumsal durumda dolaşımı, windows bulut"
documentationcenter: 
author: tanning
manager: femila
editor: curtand
ms.assetid: 17cffc3e-2928-4235-91f7-a685bd6bdcbf
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/08/2017
ms.author: markvi
ms.openlocfilehash: 5082ed8d2f41e72fa979b978e2ac0b0840fdcdac
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="windows-10-roaming-settings-reference"></a>Windows 10 dolaşım ayarları başvurusu
Çıkan veya Windows 10'da yedeklenen tüm ayarların tam bir listesi verilmiştir. 

## <a name="devices-and-endpoints"></a>Aygıtları ve uç noktaları
Sync tarafından yedekleme, desteklenen hesap türleri ve cihazlar bir özeti için aşağıdaki tabloya bakın ve Windows 10 framework geri yükleyin.

| Hesap türü ve işlem | Masaüstü | Cep telefonu |
| --- | --- | --- |
| Azure Active Directory: eşitleme |Evet |Hayır |
| Azure Active Directory: yedekleme/geri yükleme |Hayır |Hayır |
| Microsoft hesabı: eşitleme |Evet |Evet |
| Microsoft hesabı: yedekleme/geri yükleme |Hayır |Evet |

## <a name="what-is-backup"></a>Yedekleme nedir?
Windows ayarlarını genellikle varsayılan olarak eşitlemesini, ancak bazı ayarlar yalnızca, bir aygıtta yüklü uygulamalar listesi gibi desteklenir. Kurumsal durumda Dolaşım kullanıcıların mobil cihazları yalnızca ve şu anda kullanılabilir yedekleme içindir. Yedekleme, bir Microsoft hesabı kullanır ve uygulama verilerini ve ayarlarını OneDrive depolar. Eşitleme ayarları uygulamasını kullanarak aygıtta bir kullanıcıyı devre dışı bırakır, normal olarak eşitlenen uygulama verileri yalnızca yedekleme haline gelir. Yedekleme verileri yalnızca yeni bir cihaz ilk çalıştırma deneyimi sırasında geri yükleme işlemi erişilebilir. Yedeklemeler aygıt ayarları devre dışı ve yönetilebilir ve kullanıcının OneDrive hesabına silindi.

## <a name="windows-settings-overview"></a>Windows ayarlarına genel bakış
Aşağıdaki ayarları grupları, Windows 10 cihazlarında ayarları eşitleme etkinleştir/devre dışı bırakılacak son kullanıcılar için kullanılabilir.

* Tema: Masaüstü arka planı, kullanıcı döşemesi, görev çubuğu konumunu, vb. 
* Internet Explorer ayarlarını: geçmişiniz, URL'ler, Sık Kullanılanlar vb. belirtilmiş. 
* Parolaları: [Windows kimlik bilgileri kasası](https://technet.microsoft.com/library/jj554668.aspx), Wi-Fi profilleri de dahil olmak üzere 
* Dil Tercihleri: yazım sözlük, Sistem dil ayarları 
* Erişim Kolaylığı: Ekran Okuyucusu, ekran klavyesi, Büyüteç 
* Diğer Windows ayarları: Windows ayarları ayrıntıları bakın

![](./media/active-directory-enterprise-state-roaming/active-directory-enterprise-state-roaming-individual-sync-settings.png)

Edge tarayıcı ayar grubu (Sık Kullanılanlar, okuma listesi) eşitleniyor etkinleştirilebilir veya Edge tarayıcısı ayarları menü seçeneği aracılığıyla son kullanıcılar tarafından devre dışı bırakıldı.

![](./media/active-directory-enterprise-state-roaming/active-directory-enterprise-state-roaming-sync-content.png)

## <a name="windows-settings-details"></a>Windows ayarları ayrıntıları
Aşağıdaki tabloda, ayarları Grup sütunu diğer girdiler ayarlarına giderek devre dışı bırakılabilir ayarları başvurduğu > hesapları > ayarlarınızı eşitleyin > diğer Windows ayarları. 

Ayarları ve yalnızca uygulama içinde veya mobil cihaz Yönetimi (MDM) veya Grup İlkesi ayarlarını kullanarak tüm cihaz için eşitleme devre dışı bırakarak eşitlenmesini devre dışı bırakılabilir uygulamaları ayarları grubu sütununun iç girişlere bakın.
Dolaşan yok ayarlar veya eşitleme bir gruba ait değil.

| Ayarlar | Masaüstü | Cep telefonu | Grup |
| --- | --- | --- | --- |
| **Hesapları**: hesap resmi |Eşitleme |X |Tema |
| **Hesapları**: diğer hesap ayarları |X |X | |
| **Mobil geniş bant Gelişmiş**: Internet Bağlantısı Paylaşımı (etkinleştirir Bluetooth aracılığıyla mobil Wi-Fi etkin noktalarına Otomatik Bulma) ağ adı |X |X |Parolaları |
| **Uygulama verileri**: tek tek uygulamalar veri eşitleme |Eşitleme yedekleme |Eşitleme yedekleme |İç |
| **Uygulama listesi**: yüklü uygulamaların listesi |X |yedekleme |Diğer |
| **Bluetooth**: tüm Bluetooth ayarları |X |X | |
| **Komut İstemi**: komut istemi "Varsayılan" ayarları |Eşitleme |X | |
| **Kimlik bilgileri**: kimlik bilgileri kasası |Eşitleme |Eşitleme |password |
| **Tarih, saat ve bölge**: otomatik saat (Internet zaman eşitleme) |Eşitleme |Eşitleme |Dil |
| **Tarih, saat ve bölge**: 24 saatlik |Eşitleme |X |Dil |
| **Tarih, saat ve bölge**: tarih ve saat |Eşitleme |X |Dil |
| **Tarih, saat ve bölge**: saat dilimi | |X |Dil |
| **Tarih, saat ve bölge**: günışığından yararlanma saati |Eşitleme |X |Dil |
| **Tarih, saat ve bölge**: ülke/bölge |Eşitleme |X |Dil |
| **Tarih, saat ve bölge**: haftanın ilk günü |Eşitleme |X |Dil |
| **Tarih, saat ve bölge**: bölge biçimini (yerel) |Eşitleme |X |Dil |
| **Tarih, saat ve bölge**: kısa tarih |Eşitleme |X |Dil |
| **Tarih, saat ve bölge**: uzun tarih |Eşitleme |X |Dil |
| **Tarih, saat ve bölge**: kısa bir süre |Eşitleme |X |Dil |
| **Tarih, saat ve bölge**: uzun zaman |Eşitleme |X |Dil |
| **Masaüstü kişiselleştirme**: Masaüstü teması (arka plan, sistem rengi, varsayılan sistem sesleri, ekran koruyucusu) |Eşitleme |X |Tema |
| **Masaüstü kişiselleştirme**: slayt gösterisi duvar kağıdı |Eşitleme |X |Tema |
| **Masaüstü kişiselleştirme**: görev çubuğu ayarları (konum, otomatik olarak Gizle, vb.) |Eşitleme |X |Tema |
| **Masaüstü kişiselleştirme**: Başlangıç ekranı düzeni |X |yedekleme | |
| **Aygıtları**: paylaşılan yazıcılara için bağlı |X |X |diğer |
| **Edge tarayıcısı**: liste okuma |Eşitleme |Eşitleme |İç |
| **Edge tarayıcısı**: Sık Kullanılanlar |Eşitleme |Eşitleme |İç |
| **Edge tarayıcısı**: üst siteleri <sup> [[1]](#footnote-1)</sup> |Eşitleme |Eşitleme |İç |
| **Edge tarayıcısı**: URL'leri yazılan <sup> [[1]](#footnote-1)</sup> |Eşitleme |Eşitleme |İç |
| **Edge tarayıcısı**: Sık Kullanılanlar çubuğu ayarları <sup> [[1]](#footnote-1)</sup> |Eşitleme |Eşitleme |İç |
| **Edge tarayıcısı**: Giriş düğmesini göster <sup> [[1]](#footnote-1)</sup> |Eşitleme |Eşitleme |İç |
| **Edge tarayıcısı**: açılır pencereleri engelle <sup> [[1]](#footnote-1)</sup> |Eşitleme |Eşitleme |İç |
| **Edge tarayıcısı**: ile her yükleme yapmanız gerekenler sor <sup> [[1]](#footnote-1)</sup> |Eşitleme |Eşitleme |İç |
| **Edge tarayıcısı**: parolaları kaydetmek teklif <sup> [[1]](#footnote-1)</sup> |Eşitleme |Eşitleme |İç |
| **Edge tarayıcısı**: gönderme istekleri izleme <sup> [[1]](#footnote-1)</sup> |Eşitleme |Eşitleme |İç |
| **Edge tarayıcısı**: form girişlerini kaydetmek <sup> [[1]](#footnote-1)</sup> |Eşitleme |Eşitleme |İç |
| **Edge tarayıcısı**: yazarken arama ve site önerilerini göster <sup> [[1]](#footnote-1)</sup> |Eşitleme |Eşitleme |İç |
| **Edge tarayıcısı**: tanımlama bilgileri tercih <sup> [[1]](#footnote-1)</sup> |Eşitleme |Eşitleme |İç |
| **Edge tarayıcısı**: korumalı medya lisansları aygıtımda Kaydet siteleri izin <sup> [[1]](#footnote-1)</sup> |Eşitleme |Eşitleme |İç |
| **Edge tarayıcısı**: ekran okuyucusu ayarı <sup> [[1]](#footnote-1)</sup> |Eşitleme |Eşitleme |İç |
| **Yüksek Karşıtlık**: açıp kapatma |Eşitleme |X |Erişim Kolaylığı |
| **Yüksek Karşıtlık**: tema ayarları |Eşitleme |X |Erişim Kolaylığı |
| **Internet Explorer**: açmak sekmeler (URL ve başlık) |Eşitleme |Eşitleme |Internet Explorer |
| **Internet Explorer**: liste okuma |Eşitleme |Eşitleme |Internet Explorer |
| **Internet Explorer**: URL'leri yazılan |Eşitleme |Eşitleme |Internet Explorer |
| **Internet Explorer**: geçmişiniz |Eşitleme |Eşitleme |Internet Explorer |
| **Internet Explorer**: Sık Kullanılanlar |Eşitleme |Eşitleme |Internet Explorer |
| **Internet Explorer**: dışlanan URL'leri |Eşitleme |Eşitleme |Internet Explorer |
| **Internet Explorer**: giriş sayfası |Eşitleme |Eşitleme |Internet Explorer |
| **Internet Explorer**: etki alanı önerileri |Eşitleme |Eşitleme |Internet Explorer |
| **Klavye**: kullanıcıların Aç/kapalı Klavyesi |Eşitleme |X |Erişim Kolaylığı |
| **Klavye**: Evet Yapışkan Aç (varsayılan olarak kapalıdır) |Eşitleme |X |Erişim Kolaylığı |
| **Klavye**: Filtre Tuşlarını etkinleştir (varsayılan olarak kapalıdır) |Eşitleme |X |Erişim Kolaylığı |
| **Klavye**: geçiş tuşlarını etkinleştir (varsayılan olarak kapalıdır) |Eşitleme |X |Erişim Kolaylığı |
| **Internet Explorer**: etki alanı dil: Çince (CHS) QWERTY - etkinleştirmek kendi kendine öğrenme |Eşitleme |X |Dil |
| **Dil**: QWERTY CHS - enable dinamik adayı sıralaması |Eşitleme |X |Dil |
| **Dil**: kümesi Basitleştirilmiş Çince CHS QWERTY - char grafiği |Eşitleme |X |Dil |
| **Dil**: kümesi Geleneksel Çince CHS QWERTY - char grafiği |Eşitleme |X |Dil |
| **Dil**: QWERTY CHS - belirsiz pinyin |Eşitleme |yedekleme |Dil |
| **Dil**: QWERTY CHS - belirsiz çiftleri |Eşitleme |yedekleme |Dil |
| **Dil**: QWERTY CHS - tam pinyin |Eşitleme |X |Dil |
| **Dil**: QWERTY CHS - çift pinyin |Eşitleme |X |Dil |
| **Dil**: CHS otomatik düzeltme okuma QWERTY - |Eşitleme |X |Dil |
| **Dil**: QWERTY CHS - C/E anahtar anahtarı, kaydırma |Eşitleme |X |Dil |
| **Dil**: QWERTY CHS - C/E anahtar anahtarı, Ctrl |Eşitleme |X |Dil |
| **Dil**: CHS WUBI - tek karakter giriş modu |Eşitleme |X |Dil |
| **Dil**: CHS WUBI - Göster adayı olan kodlama kalan |Eşitleme |X |Dil |
| **Dil**: CHS WUBI - bip 4 kodlama geçersiz olduğunda |Eşitleme |X |Dil |
| **Dil**: CHT Bopomofo - CJK Ext-A içerir |Eşitleme |X |Dil |
| **Dil**: Japonca IME - Tahmine dayalı yazarak ve özel sözcükler |Eşitleme |Eşitleme |Dil |
| **Dil**: Kore dili (KOR) IME |X |X |Dil |
| **Dil**: el yazısı tanıma |X |X |Dil |
| **Dil**: Dil profili |Eşitleme |yedekleme |Dil |
| **Dil**: yazım denetimi - otomatik düzeltme ve Vurgu yazım hatası |Eşitleme |yedekleme |Dil |
| **Dil**: klavyeler listesi |Eşitleme |yedekleme |Dil |
| **Kilit ekranı**: tüm kilit ekranı ayarları |X |X | |
| **Büyüteç'i**: Aç veya kapat (ana Değiştir) |X |X |Erişim kolaylığı |
| **Büyüteç'i**: ters çevirmeyi renk Aç veya kapat (varsayılan olarak kapalıdır) |Eşitleme |X |Erişim kolaylığı |
| **Büyüteç'i**: izleme - klavye odağını izle |Eşitleme |X |Erişim kolaylığı |
| **Büyüteç'i**: izleme - fare imlecini izleyin |Eşitleme |X |Erişim kolaylığı |
| **Büyüteç'i**: kullanıcılar oturum açtığında Başlat (varsayılan olarak kapalıdır) |Eşitleme |X |Erişim kolaylığı |
| **Fare**: fare imlecini boyutunu değiştirme |Eşitleme |X |diğer |
| **Fare**: fare imlecini rengini değiştirme |Eşitleme |X |diğer |
| **Fare**: tüm diğer ayarlar |X |X | |
| **Ekran okuyucusu**: Hızlı Başlat |Eşitleme |X |Erişim kolaylığı |
| **Ekran okuyucusu**: kullanıcılar, aralık konuşarak okuyucu değiştirebilir |Eşitleme |X |Erişim kolaylığı |
| **Ekran okuyucusu**: kullanıcılar Aç Aç veya kapat ortak öğeler için ipuçları okuma ekran okuyucusu (üzerinde varsayılan olarak) |Eşitleme |X |Erişim kolaylığı |
| **Ekran okuyucusu**: kullanıcılar Aç Aç veya kapat olup yazılan karakterleri duyar (üzerinde varsayılan olarak) |Eşitleme |X |Erişim kolaylığı |
| **Ekran okuyucusu**: kullanıcılar Aç Aç veya kapat olup yazılan sözcükler duyar (üzerinde varsayılan olarak) |Eşitleme |X |Erişim kolaylığı |
| **Ekran okuyucusu**: ekran okuyucusu aşağıdaki Ekle imleci sahip (üzerinde varsayılan olarak) |Eşitleme |X |Erişim kolaylığı |
| **Ekran okuyucusu**: ekran okuyucusu imleç visual vurgulama etkinleştir (üzerinde varsayılan olarak) |Eşitleme |X |Erişim kolaylığı |
| **Ekran okuyucusu**: ses yardımlar Yürüt (üzerinde varsayılan olarak) |Eşitleme |X |Erişim kolaylığı |
| **Ekran okuyucusu**:, parmak kaldırdığınızda dokunma tuşlarını etkinleştir (varsayılan olarak kapalıdır) |Eşitleme |X |Erişim kolaylığı |
| **Erişim Kolaylığı**: sönen bir imleç kalınlığı ayarlama |Eşitleme |X |Erişim kolaylığı |
| **Erişim Kolaylığı**: arka plan görüntüleri kaldırın (varsayılan olarak kapalıdır) |Eşitleme |X |Erişim kolaylığı |
| **Güç ve uyku**: tüm ayarlar |X |X | |
| **Ekran kişiselleştirme Başlat**: Aksan rengi (yalnızca telefon) |X |Eşitleme |Tema |
| **Yazmaya**: Yazım Sözlüğü |Eşitleme |yedekleme |Dil |
| **Yazmaya**: düzeltme yanlış yazılmış sözcük |Eşitleme |yedekleme |Dil |
| **Yazmaya**: yanlış yazılmış sözcükleri Vurgula |Eşitleme |yedekleme |Dil |
| **Yazmaya**: yazarken metin önerilerini göster |Eşitleme |yedekleme |Dil |
| **Yazmaya**: metin öneride seçtikten sonra bir alanı Ekle |Eşitleme |yedekleme |Dil |
| **Yazmaya**: t çift ara çubuğuna dokunma sonra bir nokta ekleyin |Eşitleme |yedekleme |Dil |
| **Yazmaya**: her tümcenin ilk harfini büyük harf |Eşitleme |yedekleme |Dil |
| **Yazmaya**: t çift SHIFT tuşuna dokunun tümüyle büyük harfe kullanın |Eşitleme |yedekleme |Dil |
| **Yazmaya**: yazarken anahtar ses çalma |Eşitleme |yedekleme |Dil |
| **Yazmaya**: Kişiselleştirme verileri için touch klavye |Eşitleme |yedekleme |Dil |
| **Wi-Fi**: Wi-Fi profilleri (yalnızca WPA) |Eşitleme |Eşitleme |Parolaları |

###### <a name="footnote-1"></a>Dipnot 1
Desteklenen en düşük işletim sistemi sürümü Windows oluşturucuları güncelleştirme (yapı 15063). 

## <a name="related-topics"></a>İlgili konular
* [Kurumsal durum gezici genel bakış](active-directory-windows-enterprise-state-roaming-overview.md)
* [Azure Active Directory'de gezici Kurumsal durumunu etkinleştir](active-directory-windows-enterprise-state-roaming-enable.md)
* [Ayarlar ve veri dolaşımı SSS](active-directory-windows-enterprise-state-roaming-faqs.md)
* [Grup İlkesi ve ayarları eşitleme için MDM ayarları](active-directory-windows-enterprise-state-roaming-group-policy-settings.md)
* [Sorun giderme](active-directory-windows-enterprise-state-roaming-troubleshooting.md)
