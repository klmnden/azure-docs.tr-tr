---
title: Azure Active Directory'de Cloud App Discovery hizmetine ayarlama | Microsoft Docs
description: Bul ve bulut kullanım ve gölge BT üzerinde işlem yapılabilir bilgi sağlamak için Cloud App Discovery uygulamalarla yönetin.
services: active-directory
keywords: cloud app discovery'yi, uygulamaları yönetme
documentationcenter: ''
author: curtand
manager: mtillman
tags: ignite
ms.service: active-directory
ms.workload: identity
ms.component: users-groups-roles
ms.topic: article
ms.date: 04/25/2018
ms.author: curtand
ms.reviewer: nigu
ms.openlocfilehash: 3e4907cbeaf06a519761d76a41708eacd3843ed4
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
---
# <a name="set-up-cloud-app-discovery-in-azure-ad"></a>Azure AD cloud App Discovery ayarlayın

Azure ad cloud App Discovery, artık Microsoft Cloud App Security kullanılabilir veri ile tümleştirme dayanır. Bulut kullanım ve gölge BT üzerinde devam eden bilgi sağlamak için trafik günlüklerinizi 15. 000'den bulut uygulamalarının Cloud App Security kataloğuna Cloud App Discovery karşılaştırır. Bu makalede, Kurulum işlemi açıklar ve her adımı için ayrıntılı bilgilere bağlantılar içerir. Ayrıca güvenlik duvarı ve proxy bilgilerini ve günlük açıklar dosya desteği.

## <a name="prerequisites"></a>Önkoşullar

* Kuruluşunuz, ürünü kullanmak için bir Azure AD Premium P1 Lisansınızın olması gerekir. Daha fazla bilgi için bkz: [Azure Active Directory fiyatlandırma](https://azure.microsoft.com/pricing/details/active-directory/).
* Cloud App discovery'yi ayarlama için genel yönetici veya Azure Active Directory'de güvenlik okuyucusu olması gerekir.

## <a name="setup-steps"></a>Kurulum adımları

1. [Anlık görüntü raporları ayarlama](cloudappdiscovery-set-up-snapshots.md) denetlemek için günlük biçimi günlüklerinizi Cloud App Discovery için faydalı bilgiler sağlayan emin olun. Bunlar, ayrıca, güvenlik duvarları ve proxy sunucuları el ile karşıya trafiği günlükleri geçici görünürlük sağlayabilir.

2. [Set up sürekli reporting](https://docs.microsoft.com/cloud-app-security/discovery-docker) Cloud App Security günlük Toplayıcı kullanarak ağ üzerinden iletilen tüm günlüklerini çözümlemek için. Yeni uygulamalar ve kullanım eğilimleri belirlemek için kullanabilirsiniz.

3. Günlüklerinizi şu anda desteklenmemektedir, [özel günlük ayrıştırıcı kümesi](https://docs.microsoft.com/cloud-app-security/custom-log-parser) böylece Cloud App Discovery analiz edebilirsiniz.
  
## <a name="log-processing-flow"></a>Günlük akışı işlemi

Herhangi bir yere birkaç dakikadan birkaç saate kadar veri miktarına bağlı olarak raporlar üretmek sürebilir. İşte analiz edilir:

* **Karşıya yükleme** ağınızdaki Web trafiği günlükleri portala yüklenir.
* **Ayrıştırma** Cloud App Security ayrıştırır ve her veri kaynağı için özel bir ayrıştırıcıyla trafik günlüklerinden trafik verileri ayıklar.
* **Analiz** trafik verileri 15. 000'den fazla bulut uygulamalarını tanımlamak için Cloud App Security Kataloğu'na göre Analiz. Etkin Kullanıcılar ve IP adresleri de analiz bir parçası olarak tanımlanır.
* **Raporu oluşturmak** Cloud App Security günlük dosyalarından ayıklanan verilerin bir rapor oluşturur ve Cloud App Discovery için kullanılabilir hale getirir.

> [!NOTE]
> * Sürekli rapor verilerini günde iki kez analiz edilir.
> * Bunu yüklenmeden önce günlük Toplayıcı verileri sıkıştırır. Günlük Toplayıcı üzerinde giden trafiğe yaklaşık alınan trafik günlükleri boyutunun % 10'dur.

## <a name="using-traffic-logs-for-cloud-app-discovery"></a>Cloud App Discovery için trafiği günlüklerini kullanma

Cloud App Discovery verileri trafik günlüklerinizi kullanır. Daha fazla ayrıntı, günlük, size daha iyi görünürlük ekleyebilirsiniz. Cloud App Discovery aşağıdaki özniteliklerle web trafiği verilerini gerektirir:

* İşlem tarihi
* Kaynak IP adresi
* Kaynak kullanıcı **önerilir**
* Hedef IP adresi
* Hedef URL **önerilen** (URL'leri sağlamak IP adreslerinin sayısından daha fazla doğruluk bulut uygulama algılama için)
* Toplam veri miktarı
* Miktarı karşıya veya indirilen bulut uygulaması kullanımınıza desenleri hakkında Öngörüler için veri
* (İzin verilen/engellenen) gerçekleştirilecek eylem

Cloud App Discovery göster veya, günlüklerinize dahil edilmeyen öznitelikleri çözümleme. Örneğin, **Cisco ASA Güvenlik Duvarı'nı** standart günlük biçimi içermiyor **işlem başına karşıya yüklenen bayt miktarı**, **kullanıcıadı**, veya **hedef URL**  ancak yalnızca hedef IP adresi. Bu nedenle, bu veri kaynağı'ndan daha az bulut uygulamalarında görünürlük olabilir. Cisco ASA güvenlik duvarlarını, 6.1 için bilgi düzeyini ayarlayın.

Cloud App Discovery rapor başarıyla oluşturmak için trafik günlüklerinizi aşağıdaki koşulları karşılaması gerekir:

1.  Veri kaynağı [desteklenen bir güvenlik duvarı veya proxy sunucu](#supported-firewalls-and-proxies).
2.  Günlük biçimi standart biçimiyle eşleşir. Bu, yükleme zamanında denetlenir. Uour günlük biçimini en iyi duruma getirmek için bkz: [oluşturma anlık görüntü Cloud App Discovery raporları](cloudappdiscovery-set-up-snapshots.md).
3.  Olayları 90 günden daha eski değildir.
4.  Günlük dosyası geçerli değil ve giden trafiği bilgileri içerir.

## <a name="supported-firewalls-and-proxy-servers"></a>Desteklenen güvenlik duvarları ve proxy sunucular

* Barracuda - Web uygulaması Güvenlik Duvarı'nı (W3C)
* Blue Coat Proxy SG - erişim günlüğü (W3C)
* Denetim Noktası
* Cisco ASA FirePOWER
* Cisco ASA Güvenlik Duvarı'nı (Cisco ASA için güvenlik duvarları, ayarlayın bilgi düzeyi 6)
* Cisco IronPort WSA
* Cisco ScanSafe
* Cisco Meraki – URL günlüğü
* Clavister NGFW (Syslog)
* Dell Sonicwall
* Dijital Arts i FİLTRESİ
* Fortinet Fortigate
* Juniper SRX
* Juniper SSG
* McAfee güvenli Web ağ geçidi
* Microsoft Forefront Threat Management Gateway (W3C)
* Palo Alto serisi güvenlik duvarı
* Sophos SG
* Sophos Cyberoam
* SQUID (ortak)
* SQUID (yerel)
* WebSense - Web Security Solutions - araştırma ayrıntı raporu (CSV)
* WebSense - Web Security Solutions - Internet etkinliği günlüğü (CEF)
* Zscaler

> [!NOTE]
> Cloud App Discovery hem IPv4 hem de IPv6 adreslerini destekler.

Günlüğünüz desteklenmiyorsa seçin **diğer** olarak **veri kaynağı** ve cihaz ve karşıya yüklemeye çalıştığınız günlük belirtin. Cloud App Security bulut analist ekibi tarafından günlüğünüz gözden geçirilir. Günlük türü size bildirim, ancak bunun yerine, günlüğünüzün biçiminin eşleşen özel bir ayrıştırıcı tanımlayabilirsiniz zaman desteği eklenir. Daha fazla bilgi için bkz: [özel günlük ayrıştırıcı kullanmak](https://docs.microsoft.com/cloud-app-security/custom-log-parser).

## <a name="data-attributes-according-to-vendor-documentation"></a>Veri öznitelikleri (göre satıcı belgelerini)

| Veri kaynağı         | Hedeflenen uygulama URL'si | Hedeflenen uygulama IP adresi | Kullanıcı adı | Kaynak IP adresi | Toplam trafik | Karşıya yüklenen bayt |
|-----------------------------------------|----------------|---------------|----------|-----------|---------------|----------------|
| Barracuda                               | **Evet**        | **Evet**       | **Evet**  | **Evet**   | Hayır            | Hayır             |
| Blue Coat                               | **Evet**        | Hayır            | **Evet**  | **Evet**   | **Evet**       | **Evet**        |
| Kontrol noktası                              | Hayır             | **Evet**       | Hayır       | **Evet**   | Hayır            | Hayır             |
| Cisco ASA                               | Hayır             | **Evet**       | Hayır       | **Evet**   | **Evet**       | Hayır             |
| Cisco FWSM                              | Hayır             | **Evet**       | Hayır       | **Evet**   | **Evet**       | Hayır             |
| Cisco Ironport WSA                      | **Evet**        | **Evet**       | **Evet**  | **Evet**   | **Evet**       | **Evet**        |
| Cisco Meraki                            | **Evet**        | **Evet**       | Hayır       | **Evet**   | Hayır            | Hayır             |
| Clavister NGFW (Syslog)                 | **Evet**        | **Evet**       | **Evet**  | **Evet**   | **Evet**       | **Evet**        |
| Dell SonicWall                          | **Evet**        | **Evet**       | Hayır       | **Evet**   | **Evet**       | **Evet**        |
| Fortigate                               | Hayır             | **Evet**       | Hayır       | **Evet**   | **Evet**       | **Evet**        |
| Juniper SRX                             | Hayır             | **Evet**       | Hayır       | **Evet**   | **Evet**       | **Evet**        |
| Juniper SSG                             | Hayır             | **Evet**       | Hayır       | **Evet**   | **Evet**       | **Evet**        |
| McAfee SWG                              | **Evet**        | Hayır            | Hayır       | **Evet**   | **Evet**       | **Evet**        |
| MS TMG                                  | **Evet**        | Hayır            | **Evet**  | **Evet**   | **Evet**       | **Evet**        |
| Palo Alto Networks                      | Hayır             | **Evet**       | **Evet**  | **Evet**   | **Evet**       | **Evet**        |
| Sophos                                  | **Evet**        | **Evet**       | **Evet**  | **Evet**   | **Evet**       | Hayır             |
| SQUID (ortak)                          | **Evet**        | Hayır            | **Evet**  | **Evet**   | Hayır            | **Evet**        |
| SQUID (yerel)                          | **Evet**        | Hayır            | **Evet**  | **Evet**   | Hayır            | **Evet**        |
| WebSense - araştırma raporu (CSV)   | **Evet**        | **Evet**       | **Evet**  | **Evet**   | **Evet**       | **Evet**        |
| WebSense - Internet etkinliği günlüğü (CEF)  | **Evet**        | **Evet**       | **Evet**  | **Evet**   | **Evet**       | **Evet**        |
| Zscaler                                 | **Evet**        | **Evet**       | **Evet**  | **Evet**   | **Evet**       | **Evet**        |


## <a name="next-steps"></a>Sonraki adımlar
Azure AD Cloud App Discovery ayarlamaya devam etmek için aşağıdaki bağlantıları kullanın.

* [Anlık görüntü raporları oluşturma](cloudappdiscovery-set-up-snapshots.md)
* [Sürekli raporlamayı yapılandırma](https://docs.microsoft.com/cloud-app-security/discovery-docker)
* [Özel günlük ayrıştırıcı kullanma](https://docs.microsoft.com/cloud-app-security/custom-log-parser)
