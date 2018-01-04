---
title: "Windows 10 Dolaşım ayarları başvurusu | Microsoft Docs"
description: "Çıkan veya Windows 10'da yedeklenen tüm ayarların tam bir listesi."
services: active-directory
keywords: "Kurumsal durumda dolaşımı, windows bulut"
documentationcenter: 
author: tanning
manager: mtillman
editor: curtand
ms.assetid: 17cffc3e-2928-4235-91f7-a685bd6bdcbf
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/08/2017
ms.author: markvi
ms.openlocfilehash: 21d21c945b622c1695d8856c4baff02c098218cf
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
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
| **Hesapları**: hesap resmi |eşitle |X |Tema |
| **Hesapları**: diğer hesap ayarları |X |X | |
| **Mobil geniş bant Gelişmiş**: Internet Bağlantısı Paylaşımı (etkinleştirir Bluetooth aracılığıyla mobil Wi-Fi etkin noktalarına Otomatik Bulma) ağ adı |X |X |Parolalar |
| **Uygulama verileri**: tek tek uygulamalar veri eşitleme |Eşitleme yedekleme |Eşitleme yedekleme |İç |
| **Uygulama listesi**: yüklü uygulamaların listesi |X |yedekleme |Diğer |
| **Bluetooth**: tüm Bluetooth ayarları |X |X | |
| **Komut İstemi**: komut istemi "Varsayılan" ayarları |eşitle |X | |
| **Kimlik bilgileri**: kimlik bilgileri kasası |eşitle |eşitle |password |
| **Tarih, saat ve bölge**: otomatik saat (Internet zaman eşitleme) |eşitle |eşitle |Dil |
| **Tarih, saat ve bölge**: 24 saatlik |eşitle |X |Dil |
| **Tarih, saat ve bölge**: tarih ve saat |eşitle |X |Dil |
| **Tarih, saat ve bölge**: saat dilimi | |X |Dil |
| **Tarih, saat ve bölge**: günışığından yararlanma saati |eşitle |X |Dil |
| **Tarih, saat ve bölge**: ülke/bölge |eşitle |X |Dil |
| **Tarih, saat ve bölge**: haftanın ilk günü |eşitle |X |Dil |
| **Tarih, saat ve bölge**: bölge biçimini (yerel) |eşitle |X |Dil |
| **Tarih, saat ve bölge**: kısa tarih |eşitle |X |Dil |
| **Tarih, saat ve bölge**: uzun tarih |eşitle |X |Dil |
| **Tarih, saat ve bölge**: kısa bir süre |eşitle |X |Dil |
| **Tarih, saat ve bölge**: uzun zaman |eşitle |X |Dil |
| **Masaüstü kişiselleştirme**: Masaüstü teması (arka plan, sistem rengi, varsayılan sistem sesleri, ekran koruyucusu) |eşitle |X |Tema |
| **Masaüstü kişiselleştirme**: slayt gösterisi duvar kağıdı |eşitle |X |Tema |
| **Masaüstü kişiselleştirme**: görev çubuğu ayarları (konum, otomatik olarak Gizle, vb.) |eşitle |X |Tema |
| **Masaüstü kişiselleştirme**: Başlangıç ekranı düzeni |X |yedekleme | |
| **Aygıtları**: paylaşılan yazıcılara için bağlı |X |X |diğer |
| **Edge tarayıcısı**: liste okuma |eşitle |eşitle |İç |
| **Edge tarayıcısı**: Sık Kullanılanlar |eşitle |eşitle |İç |
| **Edge tarayıcısı**: üst siteleri <sup> [[1]](#footnote-1)</sup> |eşitle |eşitle |İç |
| **Edge tarayıcısı**: URL'leri yazılan <sup> [[1]](#footnote-1)</sup> |eşitle |eşitle |İç |
| **Edge tarayıcısı**: Sık Kullanılanlar çubuğu ayarları <sup> [[1]](#footnote-1)</sup> |eşitle |eşitle |İç |
| **Edge tarayıcısı**: Giriş düğmesini göster <sup> [[1]](#footnote-1)</sup> |eşitle |eşitle |İç |
| **Edge tarayıcısı**: açılır pencereleri engelle <sup> [[1]](#footnote-1)</sup> |eşitle |eşitle |İç |
| **Edge tarayıcısı**: ile her yükleme yapmanız gerekenler sor <sup> [[1]](#footnote-1)</sup> |eşitle |eşitle |İç |
| **Edge tarayıcısı**: parolaları kaydetmek teklif <sup> [[1]](#footnote-1)</sup> |eşitle |eşitle |İç |
| **Edge tarayıcısı**: gönderme istekleri izleme <sup> [[1]](#footnote-1)</sup> |eşitle |eşitle |İç |
| **Edge tarayıcısı**: form girişlerini kaydetmek <sup> [[1]](#footnote-1)</sup> |eşitle |eşitle |İç |
| **Edge tarayıcısı**: yazarken arama ve site önerilerini göster <sup> [[1]](#footnote-1)</sup> |eşitle |eşitle |İç |
| **Edge tarayıcısı**: tanımlama bilgileri tercih <sup> [[1]](#footnote-1)</sup> |eşitle |eşitle |İç |
| **Edge tarayıcısı**: korumalı medya lisansları aygıtımda Kaydet siteleri izin <sup> [[1]](#footnote-1)</sup> |eşitle |eşitle |İç |
| **Edge tarayıcısı**: ekran okuyucusu ayarı <sup> [[1]](#footnote-1)</sup> |eşitle |eşitle |İç |
| **Yüksek Karşıtlık**: açıp kapatma |eşitle |X |Erişim Kolaylığı |
| **Yüksek Karşıtlık**: tema ayarları |eşitle |X |Erişim Kolaylığı |
| **Internet Explorer**: açmak sekmeler (URL ve başlık) |eşitle |eşitle |Internet Explorer |
| **Internet Explorer**: liste okuma |eşitle |eşitle |Internet Explorer |
| **Internet Explorer**: URL'leri yazılan |eşitle |eşitle |Internet Explorer |
| **Internet Explorer**: geçmişiniz |eşitle |eşitle |Internet Explorer |
| **Internet Explorer**: Sık Kullanılanlar |eşitle |eşitle |Internet Explorer |
| **Internet Explorer**: dışlanan URL'leri |eşitle |eşitle |Internet Explorer |
| **Internet Explorer**: giriş sayfası |eşitle |eşitle |Internet Explorer |
| **Internet Explorer**: etki alanı önerileri |eşitle |eşitle |Internet Explorer |
| **Klavye**: kullanıcıların Aç/kapalı Klavyesi |eşitle |X |Erişim Kolaylığı |
| **Klavye**: Evet Yapışkan Aç (varsayılan olarak kapalıdır) |eşitle |X |Erişim Kolaylığı |
| **Klavye**: Filtre Tuşlarını etkinleştir (varsayılan olarak kapalıdır) |eşitle |X |Erişim Kolaylığı |
| **Klavye**: geçiş tuşlarını etkinleştir (varsayılan olarak kapalıdır) |eşitle |X |Erişim Kolaylığı |
| **Internet Explorer**: etki alanı dil: Çince (CHS) QWERTY - etkinleştirmek kendi kendine öğrenme |eşitle |X |Dil |
| **Dil**: QWERTY CHS - enable dinamik adayı sıralaması |eşitle |X |Dil |
| **Dil**: kümesi Basitleştirilmiş Çince CHS QWERTY - char grafiği |eşitle |X |Dil |
| **Dil**: kümesi Geleneksel Çince CHS QWERTY - char grafiği |eşitle |X |Dil |
| **Dil**: QWERTY CHS - belirsiz pinyin |eşitle |yedekleme |Dil |
| **Dil**: QWERTY CHS - belirsiz çiftleri |eşitle |yedekleme |Dil |
| **Dil**: QWERTY CHS - tam pinyin |eşitle |X |Dil |
| **Dil**: QWERTY CHS - çift pinyin |eşitle |X |Dil |
| **Dil**: CHS otomatik düzeltme okuma QWERTY - |eşitle |X |Dil |
| **Dil**: QWERTY CHS - C/E anahtar anahtarı, kaydırma |eşitle |X |Dil |
| **Dil**: QWERTY CHS - C/E anahtar anahtarı, Ctrl |eşitle |X |Dil |
| **Dil**: CHS WUBI - tek karakter giriş modu |eşitle |X |Dil |
| **Dil**: CHS WUBI - Göster adayı olan kodlama kalan |eşitle |X |Dil |
| **Dil**: CHS WUBI - bip 4 kodlama geçersiz olduğunda |eşitle |X |Dil |
| **Dil**: CHT Bopomofo - CJK Ext-A içerir |eşitle |X |Dil |
| **Dil**: Japonca IME - Tahmine dayalı yazarak ve özel sözcükler |eşitle |eşitle |Dil |
| **Dil**: Kore dili (KOR) IME |X |X |Dil |
| **Dil**: el yazısı tanıma |X |X |Dil |
| **Dil**: Dil profili |eşitle |yedekleme |Dil |
| **Dil**: yazım denetimi - otomatik düzeltme ve Vurgu yazım hatası |eşitle |yedekleme |Dil |
| **Dil**: klavyeler listesi |eşitle |yedekleme |Dil |
| **Kilit ekranı**: tüm kilit ekranı ayarları |X |X | |
| **Büyüteç'i**: Aç veya kapat (ana Değiştir) |X |X |Erişim kolaylığı |
| **Büyüteç'i**: ters çevirmeyi renk Aç veya kapat (varsayılan olarak kapalıdır) |eşitle |X |Erişim kolaylığı |
| **Büyüteç'i**: izleme - klavye odağını izle |eşitle |X |Erişim kolaylığı |
| **Büyüteç'i**: izleme - fare imlecini izleyin |eşitle |X |Erişim kolaylığı |
| **Büyüteç'i**: kullanıcılar oturum açtığında Başlat (varsayılan olarak kapalıdır) |eşitle |X |Erişim kolaylığı |
| **Fare**: fare imlecini boyutunu değiştirme |eşitle |X |diğer |
| **Fare**: fare imlecini rengini değiştirme |eşitle |X |diğer |
| **Fare**: tüm diğer ayarlar |X |X | |
| **Ekran okuyucusu**: Hızlı Başlat |eşitle |X |Erişim kolaylığı |
| **Ekran okuyucusu**: kullanıcılar, aralık konuşarak okuyucu değiştirebilir |eşitle |X |Erişim kolaylığı |
| **Ekran okuyucusu**: kullanıcılar Aç Aç veya kapat ortak öğeler için ipuçları okuma ekran okuyucusu (üzerinde varsayılan olarak) |eşitle |X |Erişim kolaylığı |
| **Ekran okuyucusu**: kullanıcılar Aç Aç veya kapat olup yazılan karakterleri duyar (üzerinde varsayılan olarak) |eşitle |X |Erişim kolaylığı |
| **Ekran okuyucusu**: kullanıcılar Aç Aç veya kapat olup yazılan sözcükler duyar (üzerinde varsayılan olarak) |eşitle |X |Erişim kolaylığı |
| **Ekran okuyucusu**: ekran okuyucusu aşağıdaki Ekle imleci sahip (üzerinde varsayılan olarak) |eşitle |X |Erişim kolaylığı |
| **Ekran okuyucusu**: ekran okuyucusu imleç visual vurgulama etkinleştir (üzerinde varsayılan olarak) |eşitle |X |Erişim kolaylığı |
| **Ekran okuyucusu**: ses yardımlar Yürüt (üzerinde varsayılan olarak) |eşitle |X |Erişim kolaylığı |
| **Ekran okuyucusu**:, parmak kaldırdığınızda dokunma tuşlarını etkinleştir (varsayılan olarak kapalıdır) |eşitle |X |Erişim kolaylığı |
| **Erişim Kolaylığı**: sönen bir imleç kalınlığı ayarlama |eşitle |X |Erişim kolaylığı |
| **Erişim Kolaylığı**: arka plan görüntüleri kaldırın (varsayılan olarak kapalıdır) |eşitle |X |Erişim kolaylığı |
| **Güç ve uyku**: tüm ayarlar |X |X | |
| **Ekran kişiselleştirme Başlat**: Aksan rengi (yalnızca telefon) |X |eşitle |Tema |
| **Yazmaya**: Yazım Sözlüğü |eşitle |yedekleme |Dil |
| **Yazmaya**: düzeltme yanlış yazılmış sözcük |eşitle |yedekleme |Dil |
| **Yazmaya**: yanlış yazılmış sözcükleri Vurgula |eşitle |yedekleme |Dil |
| **Yazmaya**: yazarken metin önerilerini göster |eşitle |yedekleme |Dil |
| **Yazmaya**: metin öneride seçtikten sonra bir alanı Ekle |eşitle |yedekleme |Dil |
| **Yazmaya**: t çift ara çubuğuna dokunma sonra bir nokta ekleyin |eşitle |yedekleme |Dil |
| **Yazmaya**: her tümcenin ilk harfini büyük harf |eşitle |yedekleme |Dil |
| **Yazmaya**: t çift SHIFT tuşuna dokunun tümüyle büyük harfe kullanın |eşitle |yedekleme |Dil |
| **Yazmaya**: yazarken anahtar ses çalma |eşitle |yedekleme |Dil |
| **Yazmaya**: Kişiselleştirme verileri için touch klavye |eşitle |yedekleme |Dil |
| **Wi-Fi**: Wi-Fi profilleri (yalnızca WPA) |eşitle |eşitle |Parolalar |

###### <a name="footnote-1"></a>Dipnot 1
Desteklenen en düşük işletim sistemi sürümü Windows oluşturucuları güncelleştirme (yapı 15063). 

## <a name="related-topics"></a>İlgili konular
* [Kurumsal durum gezici genel bakış](active-directory-windows-enterprise-state-roaming-overview.md)
* [Azure Active Directory'de gezici Kurumsal durumunu etkinleştir](active-directory-windows-enterprise-state-roaming-enable.md)
* [Ayarlar ve veri dolaşımı SSS](active-directory-windows-enterprise-state-roaming-faqs.md)
* [Grup İlkesi ve ayarları eşitleme için MDM ayarları](active-directory-windows-enterprise-state-roaming-group-policy-settings.md)
* [Sorun giderme](active-directory-windows-enterprise-state-roaming-troubleshooting.md)
