---
title: Windows 10 Dolaşım ayarları başvurusu | Microsoft Docs
description: Dolaşıma açıldı veya Windows 10'da yedeklenen tüm ayarları tam bir listesi.
services: active-directory
keywords: Kurumsal durumda dolaşım, Microsoft Bulutu
documentationcenter: ''
author: MarkusVi
manager: mtillman
editor: curtand
ms.component: devices
ms.assetid: 17cffc3e-2928-4235-91f7-a685bd6bdcbf
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/23/2018
ms.author: markvi
ms.openlocfilehash: 40e67dfd4ffa427ac47198e88994762a4a45cc94
ms.sourcegitcommit: d211f1d24c669b459a3910761b5cacb4b4f46ac9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/06/2018
ms.locfileid: "44023360"
---
# <a name="windows-10-roaming-settings-reference"></a>Windows 10 dolaşım ayarları başvurusu
Dolaşıma açıldı veya Windows 10'da yedeklenen tüm ayarları tam bir listesi verilmiştir. 

## <a name="devices-and-endpoints"></a>Cihazların ve uç noktaları
Eşitlemeden, yedekleme, desteklenen bir hesap türleri ve cihazlar özeti için aşağıdaki tabloya bakın ve Windows 10'daki framework geri yükleyin.

| Hesap türü ve işlem | Masaüstü | Cep telefonu |
| --- | --- | --- |
| Azure Active Directory: eşitleme |Evet |Hayır |
| Azure Active Directory: yedekleme/geri yükleme |Hayır |Hayır |
| Microsoft hesabı: eşitleme |Evet |Evet |
| Microsoft hesabı: yedekleme/geri yükleme |Hayır |Evet |

## <a name="what-is-backup"></a>Yedekleme nedir?
Windows ayarları genellikle varsayılan olarak eşitlersiniz ama bazı ayarlar yalnızca, bir cihazda yüklü uygulamaların listesi'gibi desteklenir. Kurumsal durumda Dolaşım kullanıcı için mobil cihazlarda yalnızca ve şu anda kullanılamıyor yedeklemedir. Yedekleme, bir Microsoft hesabı kullanıyor ve OneDrive'ınıza ayarları ve uygulama verilerini depolar. Eşitleme ayarları uygulamasını kullanarak bir cihazdaki bir kullanıcı tarafından devre dışı bırakıldığında normalde eşitler uygulama verileri yalnızca yedekleme olur. Yedekleme verileri, yalnızca yeni bir cihaz ilk çalıştırma deneyimi sırasında geri yükleme işlemi erişilebilir. Yedeklemeler cihaz ayarları devre dışı ve yönetilebilir ve kullanıcının OneDrive hesabına silindi.

## <a name="windows-settings-overview"></a>Windows ayarlarına genel bakış
Aşağıdaki ayar grubu, son kullanıcılar, Windows 10 cihazlarında ayarları eşitleme etkinleştir/devre dışı bırakmak için kullanılabilir.

* Tema: Masaüstü arka plan, kullanıcı kutucuğunda, görev çubuğunun konumu, vb. 
* Internet Explorer ayarlarını: gözatma geçmişi, URL'ler, Sık Kullanılanlar yazdınız. 
* Parolalar: Wi-Fi profilleri de dahil olmak üzere Windows kimlik bilgisi Yöneticisi 
* Dil Tercihleri: yazım sözlük, Sistem dil ayarları 
* Erişim Kolaylığı: Ekran Okuyucusu, ekran klavyesi, Büyüteç 
* Diğer Windows ayarları: Windows ayarları ayrıntıları bakın
* Edge tarayıcı ayarları: Microsoft Edge Sık Kullanılanlar okuma listesi ve diğer ayarları

![](./media/active-directory-enterprise-state-roaming/active-directory-enterprise-state-roaming-syncyoursettings.png)

Edge tarayıcısı ayar grubu (Sık Kullanılanlar, okuma listesi) eşitleniyor etkinleştirilebilir veya son kullanıcıların Edge tarayıcısı ayarları menü seçeneğini devre dışı.

![](./media/active-directory-enterprise-state-roaming/active-directory-enterprise-state-roaming-edge.png)

Windows 10 sürüm 1803 veya üzeri, Internet Explorer ayar grubu için (Sık Kullanılanlar, yazdığınız URL'leri) eşitlemeyi etkinleştirilebilir veya Internet Explorer ayarlarını menü seçeneği üzerinden son kullanıcılar tarafından devre dışı. 

![](./media/active-directory-enterprise-state-roaming/active-directory-enterprise-state-roaming-ie.png)

## <a name="windows-settings-details"></a>Windows ayarları ayrıntıları
Aşağıdaki tabloda, ayarları Grup sütunu içindeki diğer girişler ayarlarına giderek devre dışı bırakılabilir ayarları başvurduğu > hesaplar > ayarlarınızı eşitleme > diğer Windows ayarları. 

İç ayarlar grubunda sütun girişleri ayarları ve yalnızca uygulama içinde veya mobil cihaz Yönetimi (MDM) veya Grup İlkesi ayarlarını kullanarak tüm cihaz eşitleme devre dışı bırakarak eşitlenmesini devre dışı bırakılabilir uygulamaları bakın.
Dolaşımda yoksa ayarlar veya eşitleme grubuna ait değil.

| Ayarlar | Masaüstü | Cep telefonu | Grup |
| --- | --- | --- | --- |
| **Hesapları**: hesap resmi |eşitle |X |Tema |
| **Hesapları**: diğer hesap ayarları |X |X | |
| **Mobil geniş bant Gelişmiş**: Internet bağlantısı paylaşımının ağ adı (Bluetooth aracılığıyla mobil Wi-Fi etkin noktalarına otomatik bulma etkinleştirir) |X |X |Parolalar |
| **Uygulama verilerini**: tek tek uygulamalar verileri Eşitle |Eşitleme yedekleme |Eşitleme yedekleme |İç |
| **Uygulama listesi**: yüklü uygulamalar listesi |X |yedekleme |Diğer |
| **Bluetooth**: tüm Bluetooth ayarları |X |X | |
| **Komut İstemi**: komut istemi "Varsayılan" ayarları |eşitle |X |İç |
| **Kimlik bilgileri**: kimlik bilgileri kasası |eşitle |eşitle |password |
| **Tarih, saat ve bölge**: otomatik saat (Internet zaman eşitleme) |eşitle |eşitle |dil |
| **Tarih, saat ve bölge**: 24 saatlik düzende |eşitle |X |dil |
| **Tarih, saat ve bölge**: tarih ve saat |eşitle |X |dil |
| **Tarih, saat ve bölge**: saat dilimi | |X |dil |
| **Tarih, saat ve bölge**: gün ışığından yararlanma saatine |eşitle |X |dil |
| **Tarih, saat ve bölge**: ülke/bölge |eşitle |X |dil |
| **Tarih, saat ve bölge**: haftanın ilk günü |eşitle |X |dil |
| **Tarih, saat ve bölge**: bölge format (yerel) |eşitle |X |dil |
| **Tarih, saat ve bölge**: kısa tarih |eşitle |X |dil |
| **Tarih, saat ve bölge**: uzun tarih |eşitle |X |dil |
| **Tarih, saat ve bölge**: kısa süre |eşitle |X |dil |
| **Tarih, saat ve bölge**: uzun saat |eşitle |X |dil |
| **Masaüstü kişiselleştirme**: Masaüstü tema (arka plan, sistem renk, varsayılan sistem ses ve ekran koruyucu) |eşitle |X |Tema |
| **Masaüstü kişiselleştirme**: slayt gösterisi duvar kağıdı |eşitle |X |Tema |
| **Masaüstü kişiselleştirme**: görev çubuğu ayarları (konum, otomatik gizleme, vb.) |eşitle |X |Tema |
| **Masaüstü kişiselleştirme**: Başlangıç ekranı düzeni |X |yedekleme | |
| **Cihazları**: paylaşılan yazıcılara için bağlı |X |X |diğer |
| **Edge tarayıcısı**: listesini okuma |eşitle |eşitle |İç |
| **Edge tarayıcısı**: Sık Kullanılanlar |eşitle |eşitle |İç |
| **Edge tarayıcısı**: üst siteleri <sup> [[1]](#footnote-1)</sup> |eşitle |eşitle |İç |
| **Edge tarayıcısı**: URL'ler yazılan <sup> [[1]](#footnote-1)</sup> |eşitle |eşitle |İç |
| **Edge tarayıcısı**: Sık Kullanılanlar çubuğu ayarları <sup> [[1]](#footnote-1)</sup> |eşitle |eşitle |İç |
| **Edge tarayıcısı**: Giriş düğmesini göster <sup> [[1]](#footnote-1)</sup> |eşitle |eşitle |İç |
| **Edge tarayıcısı**: açılır pencereleri engelle <sup> [[1]](#footnote-1)</sup> |eşitle |eşitle |İç |
| **Edge tarayıcısı**: her indirme yapmanız gerekenler sor <sup> [[1]](#footnote-1)</sup> |eşitle |eşitle |İç |
| **Edge tarayıcısı**: parolaları kaydetmek için teklif <sup> [[1]](#footnote-1)</sup> |eşitle |eşitle |İç |
| **Edge tarayıcısı**: gönderme istekleri izleme <sup> [[1]](#footnote-1)</sup> |eşitle |eşitle |İç |
| **Edge tarayıcısı**: form girişlerinin Kaydet <sup> [[1]](#footnote-1)</sup> |eşitle |eşitle |İç |
| **Edge tarayıcısı**: yazarken arama ve site önerilerini göster <sup> [[1]](#footnote-1)</sup> |eşitle |eşitle |İç |
| **Edge tarayıcısı**: tanımlama bilgilerini tercih <sup> [[1]](#footnote-1)</sup> |eşitle |eşitle |İç |
| **Edge tarayıcısı**: korumalı medya lisansları cihazıma Kaydet siteleri izin <sup> [[1]](#footnote-1)</sup> |eşitle |eşitle |İç |
| **Edge tarayıcısı**: ekran okuyucu ayarlama <sup> [[1]](#footnote-1)</sup> |eşitle |eşitle |İç |
| **Yüksek Karşıtlık**: veya kapat |eşitle |X |Erişim Kolaylığı |
| **Yüksek Karşıtlık**: tema ayarları |eşitle |X |Erişim Kolaylığı |
| **Internet Explorer**: sekmeler (URL ve başlık) açın |eşitle |eşitle |Internet Explorer |
| **Internet Explorer**: listesini okuma |eşitle |eşitle |Internet Explorer |
| **Internet Explorer**: yazılan URL'leri |eşitle |eşitle |Internet Explorer |
| **Internet Explorer**: Tarama geçmişi |eşitle |eşitle |Internet Explorer |
| **Internet Explorer**: Sık Kullanılanlar |eşitle |eşitle |Internet Explorer |
| **Internet Explorer**: dışlanan URL'leri |eşitle |eşitle |Internet Explorer |
| **Internet Explorer**: giriş sayfaları |eşitle |eşitle |Internet Explorer |
| **Internet Explorer**: öneriler etki alanı |eşitle |eşitle |Internet Explorer |
| **Klavye**: kullanıcılar Aç/Kapat Klavyesi |eşitle |X |Erişim Kolaylığı |
| **Klavye**: Evet Yapışkan Aç (varsayılan olarak kapalıdır) |eşitle |X |Erişim Kolaylığı |
| **Klavye**: Filtre Tuşlarını etkinleştir (varsayılan olarak kapalıdır) |eşitle |X |Erişim Kolaylığı |
| **Klavye**: geçiş tuşlarını etkinleştir (varsayılan olarak kapalıdır) |eşitle |X |Erişim Kolaylığı |
| **Internet Explorer**: etki alanı dil: Çince (CHS) - QWERTY etkinleştirme kendi kendine öğrenme |eşitle |X |Dil |
| **Dil**: QWERTY CHS - derecelendirme dinamik aday etkinleştir |eşitle |X |Dil |
| **Dil**: kümesi Basitleştirilmiş Çince CHS QWERTY - char grafiği |eşitle |X |Dil |
| **Dil**: kümesi Geleneksel Çince CHS QWERTY - char grafiği |eşitle |X |Dil |
| **Dil**: QWERTY CHS - belirsiz PinYin'e |eşitle |yedekleme |Dil |
| **Dil**: QWERTY CHS - belirsiz çiftleri |eşitle |yedekleme |Dil |
| **Dil**: QWERTY CHS - tam PinYin'e |eşitle |X |Dil |
| **Dil**: QWERTY CHS - çift PinYin'e |eşitle |X |Dil |
| **Dil**: CHS otomatik düzeltme okuma QWERTY - |eşitle |X |Dil |
| **Dil**: QWERTY CHS - C/E anahtarı anahtar, kaydırma |eşitle |X |Dil |
| **Dil**: QWERTY CHS - C/E anahtar tuşu, Ctrl |eşitle |X |Dil |
| **Dil**: CHS WUBI - tek karakter giriş modu |eşitle |X |Dil |
| **Dil**: CHS WUBI - adayı kodlama kalan Göster |eşitle |X |Dil |
| **Dil**: 4 kodlama geçersiz olduğunda CHS WUBI - bip sesi |eşitle |X |Dil |
| **Dil**: CHT Bopomofo - CJK Ext A içerir |eşitle |X |Dil |
| **Dil**: Japonca IME - Tahmine dayalı yazma ve özel sözcük |eşitle |eşitle |Dil |
| **Dil**: Kore dili (KOR) IME |X |X |Dil |
| **Dil**: el yazısı tanıma |X |X |Dil |
| **Dil**: Dil profili |eşitle |yedekleme |Dil |
| **Dil**: yazım denetimi - otomatik düzeltme ve Vurgu yazım hatası |eşitle |yedekleme |Dil |
| **Dil**: klavyeler listesi |eşitle |yedekleme |Dil |
| **Kilit ekranı**: tüm kilit ekranı ayarları |X |X | |
| **Büyüteç'i**: açık veya kapalı (ana Aç/Kapat) |X |X |Erişim kolaylığı |
| **Büyüteç'i**: ters çevirmeyi renk Aç veya kapat (varsayılan olarak kapalıdır) |eşitle |X |Erişim kolaylığı |
| **Büyüteç'i**: izleme - klavye odağı izleyin |eşitle |X |Erişim kolaylığı |
| **Büyüteç'i**: izleme - fare imlecini takip edin |eşitle |X |Erişim kolaylığı |
| **Büyüteç'i**: kullanıcılar oturum açtığında Başlat (varsayılan olarak kapalıdır) |eşitle |X |Erişim kolaylığı |
| **Fare**: fare imlecini boyutunu değiştirme |eşitle |X |diğer |
| **Fare**: fare imlecini rengini değiştirme |eşitle |X |diğer |
| **Fare**: diğer tüm ayarlar |X |X | |
| **Ekran okuyucusu**: hızlı başlatma |eşitle |X |Erişim kolaylığı |
| **Ekran okuyucusu**: kullanıcılar, ekran okuyucusu aralık Konuşmayı değiştirebilir |eşitle |X |Erişim kolaylığı |
| **Ekran okuyucusu**: kullanıcıları açın veya ekran okuyucu ipuçları ortak öğeler için okuma Kapat (üzerinde varsayılan olarak) |eşitle |X |Erişim kolaylığı |
| **Ekran okuyucusu**: kullanıcıları açın veya yazılan karakter olup olmadığını duyabileceğiniz Kapat (üzerinde varsayılan olarak) |eşitle |X |Erişim kolaylığı |
| **Ekran okuyucusu**: kullanıcılar, açık veya kapalı olup olmadığını yazılan sözcükleri duyabileceğiniz kapatabilir (üzerinde varsayılan olarak) |eşitle |X |Erişim kolaylığı |
| **Ekran okuyucusu**: ekran okuyucusu aşağıdaki INSERT imlece sahip (üzerinde varsayılan olarak) |eşitle |X |Erişim kolaylığı |
| **Ekran okuyucusu**: ekran okuyucusu imleç visual vurgulamasını etkinleştirmenin (üzerinde varsayılan olarak) |eşitle |X |Erişim kolaylığı |
| **Ekran okuyucusu**: ses ipuçları Yürüt (üzerinde varsayılan olarak) |eşitle |X |Erişim kolaylığı |
| **Ekran okuyucusu**: parmağınızı kaldırın touch tuşlarını etkinleştirmek (varsayılan olarak kapalıdır) |eşitle |X |Erişim kolaylığı |
| **Erişim Kolaylığı**: sönen bir imleç kalınlığını ayarlayın |eşitle |X |Erişim kolaylığı |
| **Erişim Kolaylığı**: arka plan görüntülerini kaldırmak (varsayılan olarak kapalıdır) |eşitle |X |Erişim kolaylığı |
| **Güç ve uyku**: tüm ayarlar |X |X | |
| **Başlat ekranı kişiselleştirme**: Vurgu rengi (yalnızca phone) |X |eşitle |Tema |
| **Yazarak**: yazım denetimi sözlüğü |eşitle |yedekleme |Dil |
| **Yazarak**: otomatik düzeltme sözcük yanlış |eşitle |yedekleme |Dil |
| **Yazarak**: yanlış yazılan sözcükleri Vurgula |eşitle |yedekleme |Dil |
| **Yazarak**: yazarken metin önerilerini göster |eşitle |yedekleme |Dil |
| **Yazarak**: bir metin önerisi seçmem sonra boşluk Ekle |eşitle |yedekleme |Dil |
| **Yazarak**: Ben çift ara çubuğuna dokunun sonra bir nokta ekleyin |eşitle |yedekleme |Dil |
| **Yazarak**: her cümle ilk harfini büyük harfe çevirme |eşitle |yedekleme |Dil |
| **Yazarak**: Ben çift-shift tuşunu dokunduğunuzda tümüyle büyük harfe kullanın |eşitle |yedekleme |Dil |
| **Yazarak**: yazarken anahtar ses çal |eşitle |yedekleme |Dil |
| **Yazarak**: Kişiselleştirme verileri için dokunmatik klavye |eşitle |yedekleme |Dil |
| **Wi-Fi**: Wi-Fi profilleri (yalnızca WPA) |eşitle |eşitle |Parolalar |

###### <a name="footnote-1"></a>Dipnot 1
Desteklenen en düşük işletim sistemi sürümü Windows Creators Update (derleme 15063). 

## <a name="related-topics"></a>İlgili konular
* [Kurumsal Dolaşım durumuna genel bakış](active-directory-windows-enterprise-state-roaming-overview.md)
* [Kurumsal durumda Dolaşım Azure Active Directory'de etkinleştir](active-directory-windows-enterprise-state-roaming-enable.md)
* [Ayarlar ve veri dolaşımı hakkında SSS](active-directory-windows-enterprise-state-roaming-faqs.md)
* [Grup İlkesi ve MDM ayarları için ayarları eşitleme](active-directory-windows-enterprise-state-roaming-group-policy-settings.md)
* [Sorun giderme](active-directory-windows-enterprise-state-roaming-troubleshooting.md)
