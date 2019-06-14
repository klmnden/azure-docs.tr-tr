---
title: Windows 10 Dolaşım ayarları başvurusu | Microsoft Docs
description: Dolaşıma açıldı veya Windows 10'da yedeklenen tüm ayarları tam bir listesi.
services: active-directory
keywords: Kurumsal durumda dolaşım, Microsoft Bulutu
documentationcenter: ''
author: MicrosoftGuyJFlo
manager: daveba
editor: curtand
ms.subservice: devices
ms.assetid: 17cffc3e-2928-4235-91f7-a685bd6bdcbf
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/30/2019
ms.author: joflore
ms.collection: M365-identity-device-management
ms.openlocfilehash: e6c80ee5d2a4d72be131c6a781cf793d1981e780
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60353270"
---
# <a name="windows-10-roaming-settings-reference"></a>Windows 10 dolaşım ayarları başvurusu
Dolaşıma açıldı veya Windows 10'da yedeklenen tüm ayarları tam bir listesi verilmiştir. 

## <a name="devices-and-endpoints"></a>Cihazların ve uç noktaları
Eşitlemeden, yedekleme, desteklenen bir hesap türleri ve cihazlar özeti için aşağıdaki tabloya bakın ve Windows 10'daki framework geri yükleyin.

| Hesap türü ve işlem | Masaüstü | Mobil |
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
* Parola: Wi-Fi profilleri de dahil olmak üzere Windows kimlik bilgileri Yöneticisi 
* Dil Tercihleri: yazım sözlük, Sistem dil ayarları 
* Erişim Kolaylığı: Ekran Okuyucusu, ekran klavyesi, Büyüteç 
* Diğer Windows ayarları: Windows ayarları ayrıntıları bakın
* Microsoft Edge tarayıcı ayarları: Microsoft Edge Sık Kullanılanlar okuma listesi ve diğer ayarları

![Ayarlarınızı eşitleyin](./media/enterprise-state-roaming-windows-settings-reference/active-directory-enterprise-state-roaming-syncyoursettings.png)

Microsoft Edge tarayıcı ayarları Grup (Sık Kullanılanlar, okuma listesi) eşitlemeyi etkinleştirilebilir veya Microsoft Edge tarayıcı ayarları menü seçeneğini aracılığıyla son kullanıcılar tarafından devre dışı.

![Hesap](./media/enterprise-state-roaming-windows-settings-reference/active-directory-enterprise-state-roaming-edge.png)

Windows 10 sürüm 1803 veya üzeri, Internet Explorer ayar grubu için (Sık Kullanılanlar, yazdığınız URL'leri) eşitlemeyi etkinleştirilebilir veya Internet Explorer ayarlarını menü seçeneği üzerinden son kullanıcılar tarafından devre dışı. 

![Ayarlar](./media/enterprise-state-roaming-windows-settings-reference/active-directory-enterprise-state-roaming-ie.png)

## <a name="windows-settings-details"></a>Windows ayarları ayrıntıları
Aşağıdaki tabloda, ayarları Grup sütunu içindeki diğer girişler ayarlarına giderek devre dışı bırakılabilir ayarları başvurduğu > hesaplar > ayarlarınızı eşitleme > diğer Windows ayarları. 

İç ayarlar grubunda sütun girişleri ayarları ve yalnızca uygulama içinde veya mobil cihaz Yönetimi (MDM) veya Grup İlkesi ayarlarını kullanarak tüm cihaz eşitleme devre dışı bırakarak eşitlenmesini devre dışı bırakılabilir uygulamaları bakın.
Dolaşımda yoksa ayarlar veya eşitleme grubuna ait değil.

| Ayarlar | Masaüstü | Mobil | Grup |
| --- | --- | --- | --- |
| **Hesapları**: hesap resmi |Eşitleme |X |Tema |
| **Hesapları**: diğer hesap ayarları |X |X | |
| **Mobil geniş bant Gelişmiş**: Internet bağlantısı paylaşımının ağ adı (Bluetooth aracılığıyla mobil Wi-Fi etkin noktalarına otomatik bulma etkinleştirir) |X |X |Parolalar |
| **Uygulama verilerini**: tek tek uygulamalar verileri Eşitle |Eşitleme yedekleme |Eşitleme yedekleme |İç |
| **Uygulama listesi**: yüklü uygulamalar listesi |X |yedekleme |Diğer |
| **Bluetooth**: tüm Bluetooth ayarları |X |X | |
| **Komut İstemi**: Komut istemi "Varsayılan" ayarları |Eşitleme |X |İç |
| **kimlik bilgileri**: Kimlik bilgileri kasası |Eşitleme |Eşitleme |password |
| **Tarih, saat ve bölge**: otomatik saat (Internet zaman eşitleme) |Eşitleme |Eşitleme |language |
| **Tarih, saat ve bölge**: 24 saatlik düzende |Eşitleme |X |language |
| **Tarih, saat ve bölge**: tarih ve saat |Eşitleme |X |language |
| **Tarih, saat ve bölge**: saat dilimi | |X |language |
| **Tarih, saat ve bölge**: gün ışığından yararlanma saatine |Eşitleme |X |language |
| **Tarih, saat ve bölge**: ülke/bölge |Eşitleme |X |language |
| **Tarih, saat ve bölge**: haftanın ilk günü |Eşitleme |X |language |
| **Tarih, saat ve bölge**: bölge format (yerel) |Eşitleme |X |language |
| **Tarih, saat ve bölge**: kısa tarih |Eşitleme |X |language |
| **Tarih, saat ve bölge**: uzun tarih |Eşitleme |X |language |
| **Tarih, saat ve bölge**: kısa süre |Eşitleme |X |language |
| **Tarih, saat ve bölge**: uzun saat |Eşitleme |X |language |
| **Masaüstü kişiselleştirme**: Masaüstü tema (arka plan, sistem renk, varsayılan sistem ses ve ekran koruyucu) |Eşitleme |X |Tema |
| **Masaüstü kişiselleştirme**: slayt gösterisi duvar kağıdı |Eşitleme |X |Tema |
| **Masaüstü kişiselleştirme**: görev çubuğu ayarları (konum, otomatik gizleme, vb.) |Eşitleme |X |Tema |
| **Masaüstü kişiselleştirme**: Başlangıç ekranı düzeni |X |yedekleme | |
| **Cihazları**: paylaşılan yazıcılara için bağlı |X |X |diğer |
| **Microsoft Edge tarayıcısı**: listesini okuma |Eşitleme |Eşitleme |İç |
| **Microsoft Edge tarayıcısı**: Sık Kullanılanlar |Eşitleme |Eşitleme |İç |
| **Microsoft Edge tarayıcısı**: üst siteleri <sup> [[1]](#footnote-1)</sup> |Eşitleme |Eşitleme |İç |
| **Microsoft Edge tarayıcısı**: URL'ler yazılan <sup> [[1]](#footnote-1)</sup> |Eşitleme |Eşitleme |İç |
| **Microsoft Edge tarayıcısı**: Sık Kullanılanlar çubuğu ayarları <sup> [[1]](#footnote-1)</sup> |Eşitleme |Eşitleme |İç |
| **Microsoft Edge tarayıcısı**: Giriş düğmesini göster <sup> [[1]](#footnote-1)</sup> |Eşitleme |Eşitleme |İç |
| **Microsoft Edge tarayıcısı**: açılır pencereleri engelle <sup> [[1]](#footnote-1)</sup> |Eşitleme |Eşitleme |İç |
| **Microsoft Edge tarayıcısı**: her indirme yapmanız gerekenler sor <sup> [[1]](#footnote-1)</sup> |Eşitleme |Eşitleme |İç |
| **Microsoft Edge tarayıcısı**: parolaları kaydetmek için teklif <sup> [[1]](#footnote-1)</sup> |Eşitleme |Eşitleme |İç |
| **Microsoft Edge tarayıcısı**: gönderme istekleri izleme <sup> [[1]](#footnote-1)</sup> |Eşitleme |Eşitleme |İç |
| **Microsoft Edge tarayıcısı**: form girişlerinin Kaydet <sup> [[1]](#footnote-1)</sup> |Eşitleme |Eşitleme |İç |
| **Microsoft Edge tarayıcısı**: yazarken arama ve site önerilerini göster <sup> [[1]](#footnote-1)</sup> |Eşitleme |Eşitleme |İç |
| **Microsoft Edge tarayıcısı**: tanımlama bilgilerini tercih <sup> [[1]](#footnote-1)</sup> |Eşitleme |Eşitleme |İç |
| **Microsoft Edge tarayıcısı**: korumalı medya lisansları cihazıma Kaydet siteleri izin <sup> [[1]](#footnote-1)</sup> |Eşitleme |Eşitleme |İç |
| **Microsoft Edge tarayıcısı**: ekran okuyucu ayarlama <sup> [[1]](#footnote-1)</sup> |Eşitleme |Eşitleme |İç |
| **Yüksek Karşıtlık**: Veya kapat |Eşitleme |X |Erişim Kolaylığı |
| **Yüksek Karşıtlık**: Tema Ayarları |Eşitleme |X |Erişim Kolaylığı |
| **Internet Explorer**: sekmeler (URL ve başlık) açın |Eşitleme |Eşitleme |Internet Explorer |
| **Internet Explorer**: listesini okuma |Eşitleme |Eşitleme |Internet Explorer |
| **Internet Explorer**: yazılan URL'leri |Eşitleme |Eşitleme |Internet Explorer |
| **Internet Explorer**: Tarama geçmişi |Eşitleme |Eşitleme |Internet Explorer |
| **Internet Explorer**: Sık Kullanılanlar |Eşitleme |Eşitleme |Internet Explorer |
| **Internet Explorer**: dışlanan URL'leri |Eşitleme |Eşitleme |Internet Explorer |
| **Internet Explorer**: giriş sayfaları |Eşitleme |Eşitleme |Internet Explorer |
| **Internet Explorer**: öneriler etki alanı |Eşitleme |Eşitleme |Internet Explorer |
| **Klavye**: kullanıcılar Aç/Kapat Klavyesi |Eşitleme |X |Erişim Kolaylığı |
| **Klavye**: Evet Yapışkan Aç (varsayılan olarak kapalıdır) |Eşitleme |X |Erişim Kolaylığı |
| **Klavye**: Filtre Tuşlarını etkinleştir (varsayılan olarak kapalıdır) |Eşitleme |X |Erişim Kolaylığı |
| **Klavye**: geçiş tuşlarını etkinleştir (varsayılan olarak kapalıdır) |Eşitleme |X |Erişim Kolaylığı |
| **Internet Explorer**: etki alanı dil: Çince (CHS) - QWERTY etkinleştirme kendi kendine öğrenme |Eşitleme |X |Dil |
| **Dil**: CHS QWERTY - derecelendirme dinamik aday etkinleştir |Eşitleme |X |Dil |
| **Dil**: QWERTY - CHS char-Basitleştirilmiş Çince set |Eşitleme |X |Dil |
| **Dil**: QWERTY - CHS char-Geleneksel Çince set |Eşitleme |X |Dil |
| **Dil**: CHS QWERTY - belirsiz PinYin'e |Eşitleme |yedekleme |Dil |
| **Dil**: CHS QWERTY - belirsiz çiftleri |Eşitleme |yedekleme |Dil |
| **Dil**: CHS QWERTY - tam PinYin'e |Eşitleme |X |Dil |
| **Dil**: CHS QWERTY - çift PinYin'e |Eşitleme |X |Dil |
| **Dil**: Otomatik Düzeltme okuma CHS QWERTY- |Eşitleme |X |Dil |
| **Dil**: CHS QWERTY - C/E anahtarı anahtar, kaydırma |Eşitleme |X |Dil |
| **Dil**: Ctrl tuşu CHS QWERTY - C/E geçiş |Eşitleme |X |Dil |
| **Dil**: CHS WUBI - tek karakter giriş modu |Eşitleme |X |Dil |
| **Dil**: CHS WUBI - adayı kodlama kalan Göster |Eşitleme |X |Dil |
| **Dil**: CHS WUBI - 4 kodlama geçersiz olduğunda bip sesi |Eşitleme |X |Dil |
| **Dil**: -CHT Bopomofo CJK Ext A içerir |Eşitleme |X |Dil |
| **Dil**: Japonca IME - Tahmine dayalı yazma ve özel sözcük |Eşitleme |Eşitleme |Dil |
| **Dil**: Kore dili (KOR) IME |X |X |Dil |
| **Dil**: el yazısı tanıma |X |X |Dil |
| **Dil**: Dil profili |Eşitleme |yedekleme |Dil |
| **Dil**: yazım denetimi - otomatik düzeltme ve Vurgu yazım hatası |Eşitleme |yedekleme |Dil |
| **Dil**: klavyeler listesi |Eşitleme |yedekleme |Dil |
| **Kilit ekranı**: tüm kilit ekranı ayarları |X |X | |
| **Büyüteç'i**: açık veya kapalı (ana Aç/Kapat) |X |X |Erişim Kolaylığı |
| **Büyüteç'i**: ters çevirmeyi renk Aç veya kapat (varsayılan olarak kapalıdır) |Eşitleme |X |Erişim Kolaylığı |
| **Büyüteç'i**: izleme - klavye odağı izleyin |Eşitleme |X |Erişim Kolaylığı |
| **Büyüteç'i**: izleme - fare imlecini takip edin |Eşitleme |X |Erişim Kolaylığı |
| **Büyüteç'i**: kullanıcılar oturum açtığında Başlat (varsayılan olarak kapalıdır) |Eşitleme |X |Erişim Kolaylığı |
| **Fare**: fare imlecini boyutunu değiştirme |Eşitleme |X |diğer |
| **Fare**: fare imlecini rengini değiştirme |Eşitleme |X |diğer |
| **Fare**: diğer tüm ayarlar |X |X | |
| **Ekran okuyucusu**: hızlı başlatma |Eşitleme |X |Erişim Kolaylığı |
| **Ekran okuyucusu**: kullanıcılar, ekran okuyucusu aralık Konuşmayı değiştirebilir |Eşitleme |X |Erişim Kolaylığı |
| **Ekran okuyucusu**: kullanıcıları açın veya ekran okuyucu ipuçları ortak öğeler için okuma Kapat (üzerinde varsayılan olarak) |Eşitleme |X |Erişim Kolaylığı |
| **Ekran okuyucusu**: kullanıcıları açın veya yazılan karakter olup olmadığını duyabileceğiniz Kapat (üzerinde varsayılan olarak) |Eşitleme |X |Erişim Kolaylığı |
| **Ekran okuyucusu**: kullanıcılar, açık veya kapalı olup olmadığını yazılan sözcükleri duyabileceğiniz kapatabilir (üzerinde varsayılan olarak) |Eşitleme |X |Erişim Kolaylığı |
| **Ekran okuyucusu**: ekran okuyucusu aşağıdaki INSERT imlece sahip (üzerinde varsayılan olarak) |Eşitleme |X |Erişim Kolaylığı |
| **Ekran okuyucusu**: ekran okuyucusu imleç visual vurgulamasını etkinleştirmenin (üzerinde varsayılan olarak) |Eşitleme |X |Erişim Kolaylığı |
| **Ekran okuyucusu**: ses ipuçları Yürüt (üzerinde varsayılan olarak) |Eşitleme |X |Erişim Kolaylığı |
| **Ekran okuyucusu**: parmağınızı kaldırın touch tuşlarını etkinleştirmek (varsayılan olarak kapalıdır) |Eşitleme |X |Erişim Kolaylığı |
| **Erişim Kolaylığı**: sönen bir imleç kalınlığını ayarlayın |Eşitleme |X |Erişim Kolaylığı |
| **Erişim Kolaylığı**: arka plan görüntülerini kaldırmak (varsayılan olarak kapalıdır) |Eşitleme |X |Erişim Kolaylığı |
| **Güç ve uyku**: tüm ayarlar |X |X | |
| **Başlat ekranı kişiselleştirme**: Vurgu rengi (yalnızca phone) |X |Eşitleme |Tema |
| **Yazarak**: yazım denetimi sözlüğü |Eşitleme |yedekleme |Dil |
| **Yazarak**: otomatik düzeltme sözcük yanlış |Eşitleme |yedekleme |Dil |
| **Yazarak**: yanlış yazılan sözcükleri Vurgula |Eşitleme |yedekleme |Dil |
| **Yazarak**: yazarken metin önerilerini göster |Eşitleme |yedekleme |Dil |
| **Yazarak**: bir metin önerisi seçmem sonra boşluk Ekle |Eşitleme |yedekleme |Dil |
| **Yazarak**: Ben çift ara çubuğuna dokunun sonra bir nokta ekleyin |Eşitleme |yedekleme |Dil |
| **Yazarak**: her cümle ilk harfini büyük harfe çevirme |Eşitleme |yedekleme |Dil |
| **Yazarak**: Ben çift-shift tuşunu dokunduğunuzda tümüyle büyük harfe kullanın |Eşitleme |yedekleme |Dil |
| **Yazarak**: yazarken anahtar ses çal |Eşitleme |yedekleme |Dil |
| **Yazarak**: Kişiselleştirme verileri için dokunmatik klavye |Eşitleme |yedekleme |Dil |
| **Wi-Fi**: Wi-Fi profilleri (yalnızca WPA) |Eşitleme |Eşitleme |Parolalar |

###### <a name="footnote-1"></a>Dipnot 1
Desteklenen en düşük işletim sistemi sürümü Windows Creators Update (derleme 15063). 

## <a name="next-steps"></a>Sonraki adımlar

Genel bakış için bkz. [Kurumsal durumda dolaşıma genel bakış](enterprise-state-roaming-overview.md).

