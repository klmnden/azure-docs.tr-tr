---
title: Azure Backup kullanarak SAP HANA veritabanlarını yedekleme sırasında karşılaşılan hataları giderme | Microsoft Docs
description: Bu kılavuzda, Azure Backup kullanarak SAP HANA veritabanlarını yedeklemek için çalışırken sık karşılaşılan sorunları giderme açıklanmaktadır.
services: backup
author: pvrk
manager: vijayts
ms.service: backup
ms.topic: conceptual
ms.date: 06/28/2019
ms.author: pullabhk
ms.openlocfilehash: 33f1b4674aad35d55ab014c45cd73753533931cb
ms.sourcegitcommit: 6cb4dd784dd5a6c72edaff56cf6bcdcd8c579ee7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2019
ms.locfileid: "67514175"
---
# <a name="troubleshoot-back-up-of-sap-hana-server-on-azure"></a>Yedekleme, Azure üzerinde SAP HANA sunucusu sorunlarını giderme

Bu makalede, Azure sanal Makineler'de SAP HANA veritabanları koruması sorun giderme bilgileri sağlar. Sorun giderme devam etmeden önce izinleri ve ayarları hakkında bazı önemli noktalara bakalım.

## <a name="understanding-pre-requisites"></a>Ön koşullar anlama

Bir parçası olarak [ön koşullar](backup-azure-sap-hana-database.md#prerequisites), ön kayıt betiği HANA doğru izinleri ayarlama, yüklü olduğu sanal makine üzerinde çalıştırılmalıdır.

### <a name="setting-up-permissions"></a>İzinleri ayarlama

Ön kayıt betiği yapar:

1. HANA sisteminde AZUREWLBACKUPHANAUSER oluşturur ve gerekli rolleri ve izinleri aşağıda listelenen ekler:
    - Veritabanı Yöneticisi - geri yükleme sırasında yeni veritabanları oluşturmak için
    - Yedekleme kataloğunu okumak için KATALOG okuma:
    - Birkaç özel tablo erişmeye SAP_INTERNAL_HANA_SUPPORT –
2. Tüm işlemler (veritabanının yedekleme yapılandırma, yedekleme işlemi, geri yükleme yaparsanız sorgulama) yapmak için HANA eklentisi için Hdbuserstore anahtar ekler
   
   - Anahtar oluşturma işlemini onaylamak için SIDADM kimlik bilgileriyle HANA makinedeki HDBSQL komutu çalıştırın:

    ``` hdbsql
    hdbuserstore list
    ```
    
    Komut çıktısı 'AZUREWLBACKUPHANAUSER' ' % s'anahtarı {SID} {DBNAME} kullanıcıyla görüntülemelidir.

> [!NOTE]
> Bir dizi benzersiz SSFS dosya yolunda olduğundan emin olun "/ usr/sap/{SID}/home/.hdb/". Bu yol altındaki tek bir klasör olmalıdır.

### <a name="setting-up-backint-parameters"></a>BackInt parametrelerini ayarlama

Bir veritabanı yedeklemesi için seçilen sonra Azure Backup hizmeti veritabanı düzeyinde backInt parametrelerini yapılandırın.

- [catalog_backup_using_backint:true]
- [enable_accumulated_catalog_backup:false]
- [parallel_data_backup_backint_channels:1]
- [log_backup_timeout_s:900)]
- [backint_response_timeout:7200]

> [!NOTE]
> Bu parametreleri olmadığından emin olun KONAK düzeyinde mevcut. Ana bilgisayar düzeyinde parametreleri, bu parametreler geçersiz kılar ve beklenenden farklı davranışlara neden olabilir.

## <a name="understanding-common-user-errors"></a>Yaygın kullanıcı hataları anlama

### <a name="usererrorinopeninghanaodbcconnection"></a>UserErrorInOpeningHanaOdbcConnection

| Hata iletisi | Olası nedenler | Önerilen eylem |
|---|---|---|
| Sisteminiz çalışır duruma HANA system.check için bağlantı kurulamadı.| Azure Backup hizmeti, HANA veritabanı çalışmadığı için HANA'ya bağlanmak kuramıyor. Veya HANA çalıştırıyor ancak bağlanmak Azure Backup hizmeti izin vermiyor | HANA DB/hizmet kapalı olup olmadığını denetleyin. HANA DB/hizmet çalışmaya ise, tüm izinleri belirtildiği gibi ayarlanıp ayarlanmadığını denetleyin [burada](#setting-up-permissions). Anahtar yoksa, yeni bir anahtar oluşturmak için ön kayıt betiğini yeniden çalıştırın. |

### <a name="usererrorinvalidbackintconfiguration"></a>UserErrorInvalidBackintConfiguration

| Hata iletisi | Olası nedenler | Önerilen eylem |
|---|---|---|
| Algılanan geçersiz Backint yapılandırması. Korumayı durdurun ve veritabanını yeniden yapılandırın.| Azure Backup için backInt parametreleri doğru belirtilmedi. | Belirtildiği gibi parametreleri olan denetleyin [burada](#setting-up-backint-parameters). Ana bilgisayar tabanlı backInt parametreleri varsa, bunları kaldırın. Parametreler ana bilgisayarında mevcut değildir, ancak veritabanı düzeyinde el ile değiştirdiniz, sonra bunları uygun değerlere yukarıda da belirtildiği gibi döndürün. Veya 'bekleterek korumayı durdurun veri ' Azure portalı ve 'yedeklemeyi Sürdür' bir kez daha.|
