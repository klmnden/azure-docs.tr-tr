---
title: Azure Data Box REST arabirimini kullanarak için sorun giderme | Microsoft Docs
description: Veri kopyalama REST arabirimi olduğunda Azure Data Box görülen sorunların nasıl giderileceği açıklanmaktadır.
services: databox
author: alkohli
ms.service: databox
ms.subservice: disk
ms.topic: article
ms.date: 04/19/2019
ms.author: alkohli
ms.openlocfilehash: c5ceeb2e6419cab7945454087edd4c821db28343
ms.sourcegitcommit: 0568c7aefd67185fd8e1400aed84c5af4f1597f9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65204212"
---
# <a name="troubleshoot-issues-related-to-azure-data-box-blob-storage"></a>Azure veri kutusu Blob depolama alanına ilgili sorunları giderme

Bu makalede ayrıntılarıyla sorunları gidermeye ilişkin verileri kopyalamak için Data Box'ınızı REST arabirimi aracılığıyla veri kutusu Blob Depolama kullanarak görebilirsiniz. Veri kutusu Blob Depolama diğer uygulama veya Azure Depolama Gezgini, AzCopy ve Azure depolama kitaplığı gibi istemci kitaplıkları ile Python için kullanırken bu sorunları yüzeyi.

## <a name="errors-seen-in-azure-storage-explorer"></a>Azure depolama Gezgini'nde görüldüğü hataları

Bu bölümde Azure Depolama Gezgini ile veri kutusu Blob Depolama kullanırken karşılaşılan sorunlardan bazıları açıklanmaktadır.

|Hata iletisi  |Önerilen eylem |
|---------|---------|
|Alt kaynakları alınamıyor. HTTP üstbilgileri birinin değeri doğru biçimde değil.|Gelen **Düzenle** menüsünde **hedef Azure Stack API'leri**. <br>Azure Depolama Gezgini'ni yeniden başlatın.|
|`getaddrinfo ENOTFOUND <accountname>.blob.<serialnumber>.microsoftdatabox.com` |Bu maddeyi uç nokta adı `<accountname>.blob.<serialnumber>.microsoftdatabox.com` bu yolda hosts dosyasına eklendi: <li>`C:\Windows\System32\drivers\etc\hosts` Windows, şirket veya </li><li> `/etc/hosts` Linux üzerinde.</li>|
|Alt kaynakları alınamıyor. <br>Ayrıntıları: otomatik olarak imzalanan sertifika |Cihazınız için SSL sertifikası, Azure depolama Gezgini'ne içeri aktarın: <li>Sertifika, Azure portalından indirin. Daha fazla bilgi için Git [sertifikayı indirip](data-box-deploy-copy-data-via-rest.md#download-certificate).</li><li>Gelen **Düzenle** menüsünde **SSL sertifikaları** seçip **sertifikaları içeri aktar**.</li>|

## <a name="errors-seen-in-azcopy-for-windows"></a>Windows için AzCopy görülen hataları

Bu bölümde, veri kutusu Blob Depolama ile AzCopy için Windows kullanırken karşılaşılan sorunlardan bazıları açıklanmaktadır.

|Hata iletisi  |Önerilen eylem |
|---------|---------|
|Bu hata göstermeden önce bir dakika kesmek için AzCopy komut görünür: <br>Dizin https:// numaralandırılamadı... Uzak ad çözümlenemedi `<accountname>.blob.<serialnumber>.microsoftdatabox.com`|Bu maddeyi uç nokta adı `<accountname>.blob.<serialnumber>.microsoftdatabox.com` hosts dosyasına eklendi: `C:\Windows\System32\drivers\etc\hosts`.|
|Bu hata göstermeden önce bir dakika kesmek için AzCopy komut görünür: <br>Kaynak konumu ayrıştırma hatası oluştu. Temel alınan bağlantı kapatıldı: SSL/TLS güvenli kanal için güven ilişkisi kurulamadı.|Cihazınız için SSL sertifikası sistemin sertifika deposuna aktarın. Daha fazla bilgi için Git [sertifikayı indirip](data-box-deploy-copy-data-via-rest.md#download-certificate).|


## <a name="errors-seen-in-azcopy-for-linux"></a>Linux için AzCopy içinde görülen hataları

Bu bölümde, veri kutusu Blob Depolama ile Linux için AzCopy kullanılırken karşılaşılan sorunlardan bazıları açıklanmaktadır.

|Hata iletisi  |Önerilen eylem |
|---------|---------|
|Bu hata göstermeden önce 20 dakika boyunca kesmek için AzCopy komut görünür: <br>Kaynak konumu ayrıştırma hatası `https://<accountname>.blob.<serialnumber>.microsoftdatabox.com/<cntnr>`. Böyle bir cihaz veya adres|Bu maddeyi uç nokta adı `<accountname>.blob.<serialnumber>.microsoftdatabox.com` hosts dosyasına eklendi: `/etc/hosts`.|
|Bu hata göstermeden önce 20 dakika boyunca kesmek için AzCopy komut görünür: <br>Kaynak konumu ayrıştırma hatası oluştu... SSL bağlantısı kurulamadı.|Cihazınız için SSL sertifikası sistemin sertifika deposuna aktarın. Daha fazla bilgi için Git [sertifikayı indirip](data-box-deploy-copy-data-via-rest.md#download-certificate).|

## <a name="errors-seen-in-azure-storage-library-for-python"></a>Python için Azure depolama Kitaplığı'nda görülen hataları

Bu bölümde Data Box Disk dağıtımı sırasında bir Linux istemcisi için veri kopyalama kullanılırken karşılaşılan en önemli sorunlardan bazıları açıklanmaktadır.

|Hata iletisi  |Önerilen eylem |
|---------|---------|
|HTTP üstbilgileri birinin değeri doğru biçimde değil. |Python için Microsoft Azure depolama Kitaplığı'nın yüklü sürümü veri kutusu tarafından desteklenmiyor. Azure veri kutusu Blob Depolama alanı gereksinimleri, desteklenen sürümleri için bkz.|
|… [SSL: CERTIFICATE_VERIFY_FAILED]...|Python çalıştırmadan önce SSL Base64 kodlamalı sertifika dosyasının yolunu REQUESTS_CA_BUNDLE ortam değişkenini ayarlamak (bkz. nasıl [sertifikayı indirip](data-box-deploy-copy-data-via-rest.md#download-certificate)). <br>Örneğin:<br>`export REQUESTS_CA_BUNDLE=/tmp/mycert.cer` <br>`python` <br>Alternatif olarak, sistemin sertifika deposuna sertifika ekleyin ve ardından bu ortam değişkeni, deposunun yolunu ayarlayın. <br> Örneğin, Ubuntu üzerinde: <br>`export REQUESTS_CA_BUNDLE=/etc/ssl/certs/ca-certificates.crt` <br>`python`|


## <a name="common-errors"></a>Sık karşılaşılan hatalar

Bu hatalar, herhangi bir uygulamaya özgü değildir.

|Hata iletisi  |Önerilen eylem |
|---------|---------|
|Bağlantı zaman aşımına uğradı. |Data Box cihazda oturum açıp kilitli olup olmadığını denetleyin. Cihaz yeniden başlatmaları için istediğiniz zaman, biri oturum açtığında kadar kilitli kalır.|

## <a name="next-steps"></a>Sonraki adımlar

- Hakkında bilgi edinin [veri kutusu Blob Depolama sistem gereksinimleri](data-box-system-requirements-rest.md).
