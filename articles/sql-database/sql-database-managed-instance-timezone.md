---
title: Azure SQL veritabanı yönetilen örneği saat dilimi | "Microsoft Docs
description: Azure SQL veritabanı yönetilen örneği, saat dilimi özellikleri hakkında bilgi edinin
services: sql-database
ms.service: sql-database
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: MladjoA
ms.author: mlandzic
ms.reviewer: ''
manager: craigg
ms.date: 04/09/2019
ms.openlocfilehash: c9be129624c87ca0bff1e85a5d1c71425ef06b89
ms.sourcegitcommit: 6e32f493eb32f93f71d425497752e84763070fad
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/10/2019
ms.locfileid: "59471915"
---
# <a name="time-zone-in-azure-sql-database-managed-instance"></a>Azure SQL veritabanı yönetilen örneği'saat diliminde

Eşgüdümlü Evrensel Saat (UTC) kullanarak veri katmanını bulut çözümleri için önerilen bir uygulama olsa da, Azure SQL veritabanı yönetilen örneği, tarih ve saat değerleri depolamak ve çağrı tarih mevcut uygulamaların gereksinimlerini karşılamak için saat dilimi seçeneği sunar ve belirli bir saat dilimi örtük bir bağlamla saat işlevleri.

T-SQL işlevleri ister [GETDATE()](https://docs.microsoft.com/sql/t-sql/functions/getdate-transact-sql) veya CLR kod örneğinde düzeyinde ayarlanan saat dilimini gözlemleyin. SQL Aracısı işleri de zamanlama örneğinin saat dilimine göre izleyin.

  >[!NOTE]
  > Yönetilen örnek, saat dilimi ayarını destekler, Azure SQL veritabanı'nın yalnızca bir dağıtım seçeneğidir. Diğer dağıtım seçenekleri, her zaman UTC izleyin.
Kullanım [AT TIME ZONE](https://docs.microsoft.com/sql/t-sql/queries/at-time-zone-transact-sql) olmayan UTC saat diliminde tarih ve saat bilgilerini yorumlamak gerekiyorsa, tek ve havuza alınmış SQL veritabanları.

## <a name="supported-time-zones"></a>Desteklenen saat dilimleri

Yönetilen örnek temel işletim sistemi desteklenen saat dilimlerini birtakım devralınır ve bunu düzenli olarak yeni saat dilimi tanımları almak ve varolanları değişiklikleri yansıtmak için güncelleştiriliyor.

Desteklenen saat dilimlerini adlarıyla bir liste aracılığıyla kullanıma [sys.time_zone_info](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-time-zone-info-transact-sql) sistem görünümü.

## <a name="setting-time-zone"></a>Saat dilimini ayarlama

Yönetilen örnek, bir saat dilimi örnek oluşturma sırasında ayarlanabilir. Varsayılan saat dilimini Eşgüdümlü Evrensel Saat (UTC) ' dir.

  >[!NOTE]
  > Mevcut bir yönetilen örnek saat dilimini değiştirilemez.

### <a name="setting-the-time-zone-through-azure-portal"></a>Azure portalı üzerinden bir saat dilimi ayarlama

Parametreler için yeni örnek girerken, desteklenen saat dilimlerini listesinden bir saat dilimi seçin:
  
![Örnek oluşturma sırasında saat dilimi ayarlama](media/sql-database-managed-instance-timezone/01-setting_timezone-during-instance-creation.png)

### <a name="azure-resource-manager-template"></a>Azure Resource Manager şablonu

Saat dilimi kimliği özelliğini belirtin, [Resource Manager şablonu](https://aka.ms/sql-mi-create-arm-posh) örnek oluşturma sırasında saat dilimini ayarlamak için.

```json
"properties": {
                "administratorLogin": "[parameters('user')]",
                "administratorLoginPassword": "[parameters('pwd')]",
                "subnetId": "[parameters('subnetId')]",
                "storageSizeInGB": 256,
                "vCores": 8,
                "licenseType": "LicenseIncluded",
                "hardwareFamily": "Gen5",
                "collation": "Serbian_Cyrillic_100_CS_AS",
                "timezoneId": "Central European Standard Time"
            },

```

Saat dilimi kimliği özelliği için desteklenen değerler listesi bu makalenin sonunda bulunur.

Belirtilmezse, saat dilimi UTC değerine ayarlanır.

## <a name="checking-the-time-zone-of-instance"></a>Örneğinin saat dilimini denetleniyor

[CURRENT_TIMEZONE](https://docs.microsoft.com/sql/t-sql/functions/current-timestamp-transact-sql) işlevi örneğinin saat dilimini görünen adını döndürür.

## <a name="cross-feature-considerations"></a>Çapraz-özellik konuları

### <a name="restore-and-import"></a>Geri yükleme ve içeri aktarma

Yedekleme dosyası geri yükleme ya da veri yönetilen örnek için bir örnek veya bir sunucu ile farklı bir saat dilimi ayarlarını alma. Ancak, dikkatli ve uygulama davranışı ve sorguları ve raporları, sonuçlarını analiz etmek için yalnızca iki SQL Server örneği ile farklı bir saat dilimi ayarlarını arasında veri aktarımı yaparken gibi emin olun.

### <a name="point-in-time-restore"></a>Belirli bir noktaya geri yükleme

Belirli bir noktaya geri yükleme gerçekleştirilirken için geri yükleme süresini belirsizlik nedeniyle Yaz Saati ve olası değişikliklerini önlemek için UTC saati olarak yorumlanır.

### <a name="auto-failover-groups"></a>Otomatik yük devretme grupları

Yük devretme birincil ve ikincil örneği arasında aynı saat diliminde kullanarak Grup zorunlu değildir ancak önemle tavsiye edilir.
  >[!IMPORTANT]
  > Farklı bir saat dilimi olması için geçerli senaryoları varken coğrafi-ikincil örneği yalnızca okuma ölçek için kullanılan, ikincil örneğine el ile veya otomatik yük devretme durumunda, özgün kendi saat dilimiyle koruyacağını lütfen unutmayın.

## <a name="limitations"></a>Sınırlamalar

- Mevcut yönetilen örnek saat dilimini değiştirilemez.
- Gelen SQL Aracısı işleri dış işlem örneği saat dilimini Gözlemleme.
- Yönetilen örnek yerel [yeni AzSqlInstance](https://docs.microsoft.com/powershell/module/az.sql/new-azsqlinstance) PowerShell cmdlet'i değil destek geçirme saat dilimi parametresi henüz. İle kullanımı PowerShell sarmalayıcı [Resource Manager şablonu](https://aka.ms/sql-mi-create-arm-posh) yerine.
- CLI komutunu [az sql mı oluşturmak](https://docs.microsoft.com/cli/azure/sql/mi?view=azure-cli-latest#az-sql-mi-create) saat dilimi parametresi henüz desteklemiyor.

## <a name="list-of-supported-time-zones"></a>Desteklenen zaman bölgelerinin listesi

| **Saat dilimi kimliği** | **Saat dilimi görünen adı** |
| --- | --- |
| Tarih Çizgisi Standart Saati | (UTC-12:00) Uluslararası Tarih Çizgisi Batı |
| UTC-11 | (UTC-11:00) Eşgüdümlü Evrensel Saat-11 |
| Aleutian Standart Saati | (UTC-10:00) Aleutian Adaları |
| Hawaii Standart Saati | (UTC-10:00) Hawaii |
| Marquesas Standart Saati | (UTC-09:30) Marquesas Adaları |
| Alaska Standart Saati | (UTC-09:00) Alaska |
| UTC-09 | (UTC-09:00) Eşgüdümlü Evrensel Saat-09 |
| Pasifik Standart Saati (Mexico) | (UTC-08:00) Baja California |
| UTC-08 | (UTC-08:00) Eşgüdümlü Evrensel Saat-08 |
| Pasifik Standart Saati | (UTC-08:00) Pasifik Saati (ABD ve Kanada) |
| ABD Sıradağlar Standart Saati | (UTC-07:00) Arizona |
| Sıradağlar Standart Saati (Meksika) | (UTC-07:00) Chihuahua, La Paz, Mazatlan |
| Sıradağlar Standart Saati | (UTC-07:00) Sıradağlar Saati (ABD ve Kanada) |
| Orta Amerika Standart Saati | (UTC-06:00) Orta Amerika |
| Merkezi Standart Saat | (UTC-06:00) Merkezi Saat (ABD ve Kanada) |
| Paskalya Adası Standart Saati | (UTC-06:00) Paskalya Adası |
| Merkezi Standart Saat (Meksika) | (UTC-06:00) Guadalahara, Mexico City, Monterey |
| Kanada Orta Standart Saati | (UTC-06:00) Saskatchewan |
| GA Pasifik Standart Saati | (UTC-05:00) Bogota, Lima, Quito, Rio Branco |
| Doğu Standart Saati (Meksika) | (UTC-05:00) Chetumal |
| Doğu Standart Saati | (UTC-05:00) Doğu Saati (ABD ve Kanada) |
| Haiti Standart Saati | (UTC-05:00) Haiti |
| Küba Standart Saati | (UTC-05:00) Havana |
| ABD Doğu Standart Saati | (UTC-05:00) Indiana (Doğu) |
| Turks ve Caicos Standart Saati | (UTC-05:00) Turks ve Caicos |
| Paraguay Standart Saati | (UTC-04:00) Asuncion |
| Atlantik Standart Saati | (UTC-04:00) Atlantik Saati (Kanada) |
| Venezuela Standart Saati | (UTC-04:00) Karakas |
| Orta Brezilya Standart Saati | (UTC-04:00) Cuiaba |
| GA Batı Standart Saati | (UTC-04:00) Georgetown, La Paz, Manaus, San Juan |
| Pasifik GA Standart Saati | (UTC-04:00) Santiago |
| Newfoundland Standart Saati | (UTC-03:30) Newfoundland |
| Tocantins Standart Saati | (UTC-03:00) Araguaina |
| E. Güney Amerika Standart Saati | (UTC-03:00) Brezilya |
| GA Doğu Standart Saati | (UTC-03:00) Cayenne, Fortaleza |
| Arjantin Standart Saati | (UTC-03:00) Buenos Aires |
| Grönland Standart Saati | (UTC-03:00) Grönland |
| Montevideo Standart Saati | (UTC-03:00) Montevideo |
| Magallanes Standart Saati | (UTC-03:00) Punta Arenas |
| Saint Pierre Standart Saati | (UTC-03:00) Saint Pierre ve Miquelon |
| Bahia Standart Saati | (UTC-03:00) Salvador |
| UTC-02 | (UTC-02:00) Eşgüdümlü Evrensel Saat-02 |
| Orta Atlantik Standart Saati | (UTC-02:00) Orta Atlantik - Eski |
| Azor Adaları Standart Saati | (UTC-01:00) Azor Adaları |
| Cabo Verde Standart Saati | (UTC-01:00) Cape Verde Adaları |
| UTC | (UTC) Eşgüdümlü Evrensel Saat |
| GMT Standart Saati | (UTC+00:00) Dublin, Edinburgh, Lizbon, Londra |
| Greenwich Standart Saati | (UTC+00:00) Monrovya, Reykjavik |
| W. Avrupa Standart Saati | (UTC+01:00) Amsterdam, Berlin, Bern, Roma, Stockholm, Viyana |
| Orta Avrupa Standart Saati | (UTC+01:00) Belgrat, Bratislava, Budapeşte Ljubljana, Prag |
| Latin Standart Saati | (UTC+01:00) Brüksel, Kopenhag, Madrid, Paris |
| Fas Standart Saati | (UTC+01:00) Kazablanka |
| Sao Tome Standart Saati | (UTC+01:00) Sao Tome |
| Orta Avrupa Standart Saati | (UTC+01:00) Saraybosna, Üsküp, Varşova, Zagreb |
| W. Orta Afrika Standart Saati | (UTC+01:00) Orta Batı Afrika |
| Ürdün Standart Saati | (UTC+02:00) Amman |
| GTB Standart Saati | (UTC+02:00) Atina, Reykjavik |
| Ortadoğu Standart Saati | (UTC+02:00) Beyrut |
| Mısır Standart Saati | (UTC+02:00) Kahire |
| E. Avrupa Standart Saati | (UTC+02:00) Chisinau |
| Suriye Standart Saati | (UTC+02:00) Şam |
| Batı Şeria Standart Saati | (UTC+02:00) Gazze, Hebron |
| Güney Afrika Standart Saati | (UTC+02:00) Harare, Pretoria |
| FLE Standart Saati | (UTC+02:00) Helsinki, Kiev, Riga, Sofya, Tallinn, Vilnius |
| İsrail Standart Saati | (UTC+02:00) Kudüs |
| Kaliningrad Standart Saati | (UTC+02:00) Kaliningrad |
| Sudan Standart Saati | (UTC+02:00) Hartum |
| Libya Standart Saati | (UTC+02:00) Tripoli |
| Namibia Standart Saati | (UTC+02:00) Windhoek |
| Arap Standart Saati | (UTC+03:00) Bağdat |
| Türkiye Standart Saati | (UTC+03:00) İstanbul |
| Arap Standart Saati | (UTC+03:00) Kuveyt, Riyad |
| Belarus Standart Saati | (UTC+03:00) Minsk |
| Rus Standart Saati | (UTC+03:00) Moskova, St. Petersburg |
| E. Afrika Standart Saati | (UTC+03:00) Nairobi |
| İran Standart Saati | (UTC+03:30) Tahran |
| Arabistan Standart Saati | (UTC+04:00) Abu Dabi, Muscat |
| Astrahan Standart Saati | (UTC+04:00) Astrahan, Ulyanovsk |
| Azerbaycan Standart Saati | (UTC+04:00) Bakü |
| Rusya saat dilimi 3 | (UTC+04:00) İjevsk, Samara |
| Mauritius Standart Saati | (UTC+04:00) Port Louis |
| Saratov Standart Saati | (UTC+04:00) Saratov |
| Georgia Standart Saati | (UTC+04:00) Tiflis |
| Volgograd Standart Saati | (UTC+04:00) Volgograd |
| Kafkasya Standart Saati | (UTC+04:00) Erivan |
| Afganistan Standart Saati | (UTC+04:30) Kabil |
| Batı Asya Standart Saati | (UTC+05:00) Aşkabad, Taşkent |
| Ekaterinburg Standart Saati | (UTC+05:00) Ekaterinburg |
| Pakistan Standart Saati | (UTC+05:00) İslamabat, Karaçi |
| Hindistan Standart Saati | (UTC+05:30) Chennai, Kalküta, Mumbai, Yeni Delhi |
| Sri Lanka Standart Saati | (UTC+05:30) Sri Jayawardenepura |
| Nepal Standart Saati | (UTC+05:45) Kathmandu |
| Orta Asya Standart Saati | (UTC+06:00) Astana |
| Bangladeş Standart Saati | (UTC+06:00) Dakka |
| Omsk Standart Saati | (UTC+06:00) Omsk |
| Myanmar Standart Saati | (UTC+06:30) Yangon (Rangoon) |
| Güneydoğu Asya Standart Saati | (UTC+07:00) Bangkok, Hanoi, Cakarta |
| Altay Standart Saati | (UTC+07:00) Barnaul, Gorno-Altaysk |
| W. Moğolistan Standart Saati | (UTC+07:00) Hovd |
| Kuzey Asya Standart Saati | (UTC+07:00) Krasnoyarsk |
| N. Orta Asya Standart Saati | (UTC+07:00) Novosibirsk |
| Tomsk Standart Saati | (UTC+07:00) Tomsk |
| Çin Standart Saati | (UTC+08:00) Pekin, Chongqing, Hong Kong, Urumçi |
| Kuzey Asya Doğu Standart Saati | (UTC+08:00) Irkutsk |
| Singapur Standart Saati | (UTC+08:00) Kuala Lumpur, Singapur |
| W. Avustralya Standart Saati | (UTC+08:00) Pert |
| Taipei Standart Saati | (UTC+08:00) Taipei |
| Ulanbator Standart Saati | (UTC+08:00) Ulanbator |
| Orta Batı Avustralya Standart Saati | (UTC+08:45) Eucla |
| Transbaykal Standart Saati | (UTC+09:00) Chita |
| Tokyo Standart Saati | (UTC+09:00) Osaka, Sapporo, Tokyo |
| Kuzey Kore Standart Saati | (UTC+09:00) Pyongyang |
| Güney Kore Standart Saati | (UTC+09:00) Seul |
| Yakutsk Standart Saati | (UTC+09:00) Yakutsk |
| Orta Avustralya Standart Saati | (UTC+09:30) Adelaide |
| Orta Avustralya Standart Saati | (UTC+09:30) Darwin |
| E. Avustralya Standart Saati | (UTC+10:00) Brisbane |
| Doğu Avustralya Standart Saati | (UTC+10:00) Kanbera, Melbourne, Sidney |
| Batı Pasifik Standart Saati | (UTC+10:00) Guam, Port Moresby |
| Tazmanya Standart Saati | (UTC+10:00) Hobart |
| Vladivostok Standart Saati | (UTC+10:00) Vladivostok |
| Lord Howe Standart Saati | (UTC+10:30) Lord Howe Adası |
| Bougainville Standart Saati | (UTC+11:00) Bougainville Adası |
| Rusya saat dilimi 10 | (UTC+11:00) Chokurdakh |
| Magadan Standart Saati | (UTC+11:00) Magadan |
| Norfolk Standart Saati | (UTC+11:00) Norfolk Adası |
| Sakhalin Standart Saati | (UTC+11:00) Sakhalin |
| Orta Pasifik Standart Saati | (UTC+11:00) Solomon Adaları, Yeni Kaledonya |
| Rusya saat dilimi 11 | (UTC+12:00) Anadır, Petropavlovsk-Kamçatski |
| Yeni Zelanda Standart Saati | (UTC+12:00) Auckland, Wellington |
| UTC+12 | (UTC+12:00) Eşgüdümlü Evrensel Saat+12 |
| Fiji Standart Saati | (UTC+12:00) Fiji |
| Kamçatka Standart Saati | (UTC+12:00) Petropavlovsk-Kamchatsky - Eski |
| Chatham Adaları Standart Saati | (UTC+12:45) Chatham Adaları |
| UTC+13 | (UTC+13:00) Eşgüdümlü Evrensel Saat+13 |
| Tonga Standart Saati | (UTC+13:00) Nuku'alofa |
| Samoa Standart Saati | (UTC+13:00) Samoa |
| Line Adaları Standart Saati | (UTC+14:00) Kiritimati Adası |

## <a name="see-also"></a>Ayrıca bkz.

- [CURRENT_TIMEZONE (Transact-SQL)](https://docs.microsoft.com/sql/t-sql/functions/current-timezone-transact-sql)
- [SAAT DİLİMİ (Transact-SQL),](https://docs.microsoft.com/sql/t-sql/queries/at-time-zone-transact-sql)
- [sys.time_zone_info (Transact-SQL)](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-time-zone-info-transact-sql)