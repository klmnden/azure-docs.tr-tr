---
title: Azure İzleyici'de Linux uygulama performansı toplamak | Microsoft Docs
description: Bu makalede, MySQL ve Apache HTTP Server için performans sayaçları toplamak Linux için Log Analytics aracısını yapılandırmak için ayrıntıları sağlar.
services: log-analytics
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: tysonn
ms.assetid: f1d5bde4-6b86-4b8e-b5c1-3ecbaba76198
ms.service: log-analytics
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/04/2017
ms.author: magoedte
ms.openlocfilehash: ea74440a5c8a9a2584e742ec72ccf888b6bb5ad9
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60628923"
---
# <a name="collect-performance-counters-for-linux-applications-in-azure-monitor"></a>Azure İzleyici'de Linux uygulamaları için performans sayaçlarını Topla 
[!INCLUDE [log-analytics-agent-note](../../../includes/log-analytics-agent-note.md)]
Yapılandırma ayrıntıları bu makalede sağlar [Linux için Log Analytics aracısını](https://github.com/Microsoft/OMS-Agent-for-Linux) Azure İzleyici ile belirli uygulamalar için performans sayaçları toplamak için.  Bu makalede bulunan uygulamalar şunlardır:  

- [MySQL](#mysql)
- [Apache HTTP Server](#apache-http-server)

## <a name="mysql"></a>MySQL
Log Analytics aracısını yüklendiğinde sunucu MySQL veya MariaDB sunucu bilgisayarda algılanırsa, bir performans izleme sağlayıcısı MySQL sunucusu için otomatik olarak yüklenir. Bu sağlayıcı performans istatistiklerini kullanıma sunmak için yerel MySQL/MariaDB sunucusuna bağlanır. Sağlayıcı MySQL sunucusuna erişebilmesi için MySQL kullanıcı kimlik bilgilerinin yapılandırılması gerekir.

### <a name="configure-mysql-credentials"></a>MySQL kimlik bilgilerini yapılandırma
MySQL OMI sağlayıcısı, önceden yapılandırılmış bir MySQL kullanıcısı gerektirir ve sorgu performansı ve sistem durumu bilgilerinden MySQL örneği için MySQL istemci kitaplıkları yüklü.  Bu kimlik bilgileri, Linux aracısı üzerinde depolanan bir kimlik doğrulama dosyasında depolanır.  Kimlik doğrulama dosyasını hangi bağlama adresi ve bağlantı noktası MySQL örneği dinleyen ve hangi ölçümleri toplamak için kullanılacak kimlik bilgilerini belirtir.  

MySQL OMI Linux için Log Analytics aracısını yüklenmesi sırasında sağlayıcısı MySQL my.cnf yapılandırma dosyalarını (varsayılan konumlar) bağlama adresi ve bağlantı noktası tarama ve MySQL OMI kimlik doğrulama dosyasını kısmen ayarlayın.

MySQL kimlik doğrulaması dosya konumunda depolanır `/var/opt/microsoft/mysql-cimprov/auth/omsagent/mysql-auth`.


### <a name="authentication-file-format"></a>Kimlik doğrulaması dosya biçimi
Aşağıdaki MySQL OMI kimlik doğrulaması dosya biçimi

    [Port]=[Bind-Address], [username], [Base64 encoded Password]
    (Port)=(Bind-Address), (username), (Base64 encoded Password)
    (Port)=(Bind-Address), (username), (Base64 encoded Password)
    AutoUpdate=[true|false]

Kimlik doğrulaması dosyasındaki girişler aşağıdaki tabloda açıklanmıştır.

| Özellik | Açıklama |
|:--|:--|
| Bağlantı noktası | MySQL örneğinin dinlediği geçerli bağlantı noktasını temsil eder. Bağlantı noktası 0, aşağıdaki özellikler varsayılan örneği için kullanıldığını belirtir. |
| Bağlama adresi| Geçerli MySQL bağlama-adresi. |
| kullanıcı adı| MySQL kullanıcı MySQL sunucu örneğinde izlemek üzere kullanmak için kullanılır. |
| Parola Base64 ile kodlanmış| Base64 ile kodlanmış MySQL izleme kullanıcının parolası. |
| Otomatik güncelleştirme| My.cnf dosyasındaki değişiklikleri için yeniden tarayın ve MySQL OMI sağlayıcısı yükseltildiğinde MySQL OMI kimlik doğrulaması dosyanın üzerine belirtir. |

### <a name="default-instance"></a>Varsayılan örnek
MySQL OMI kimlik doğrulaması dosyası yapmak daha kolay bir Linux ana bilgisayarda birden çok MySQL örneklerini yönetmek için bir varsayılan örneği ve bağlantı noktası numarası tanımlayabilirsiniz.  Varsayılan örneği, örnek bağlantı noktası 0 ile belirtilir. Tüm ek örnekleri, bunlar farklı değerler belirtmediğiniz sürece varsayılan örnekten kümesi özellikleri devralır. MySQL örneği '3308' bağlantı noktası üzerinde dinleme eklenirse, örneğin, varsayılan örneği bağlama adresi, kullanıcı adı ve Base64 kodlu parola deneyin ve 3308 üzerinde dinleme örneği izlemek için kullanılır. 3308 örneğinde için başka bir adresi ve kullanıcı adı ve parola aynı MySQL çiftini kullanan yalnızca bağlama adresi gereklidir ve diğer özellikleri devralınır.

Aşağıdaki tabloda örnek örneği ayarları vardır. 

| Açıklama | Dosya |
|:--|:--|
| Varsayılan örneği ve bağlantı noktası 3308 örneği. | `0=127.0.0.1, myuser, cnBwdA==`<br>`3308=, ,`<br>`AutoUpdate=true` |
| Varsayılan örneği ve bağlantı noktası 3308 ve farklı bir kullanıcı adı ve parola ile örneği. | `0=127.0.0.1, myuser, cnBwdA==`<br>`3308=127.0.1.1, myuser2,cGluaGVhZA==`<br>`AutoUpdate=true` |


### <a name="mysql-omi-authentication-file-program"></a>MySQL OMI kimlik doğrulaması dosya programı
MySQL OMI sağlayıcı yüklemesi ile MySQL OMI kimlik doğrulama dosyasını düzenlemek için kullanılabilecek bir MySQL OMI kimlik doğrulaması dosya programı içerir. Kimlik doğrulaması dosya programı şu konumda bulunabilir.

    /opt/microsoft/mysql-cimprov/bin/mycimprovauth

> [!NOTE]
> Kimlik bilgileri dosyası omsagent hesap tarafından okunabilir olması gerekir. Omsgent mycimprovauth komutunun çalıştırılması önerilir.

Aşağıdaki tabloda, mycimprovauth kullanmak için söz dizimi hakkında ayrıntılar sağlar.

| İşlem | Örnek | Açıklama
|:--|:--|:--|
| Otomatik güncelleştirme *yanlış veya doğru* | mycimprovauth autoupdate false | Olup olmadığı kimlik doğrulama dosyasını otomatik olarak güncelleştirilir kümeleri üzerinde yeniden başlatın veya güncelleştirin. |
| Varsayılan *bağlama adresi kullanıcı adı parola* | mycimprovauth 127.0.0.1 varsayılan kök pwd | Varsayılan örnek MySQL OMI kimlik doğrulama dosyasını ayarlar.<br>Parola alanı düz metin biçiminde girilmesi gerekir - MySQL OMI kimlik doğrulaması dosyasındaki parola Base 64 kodlamalı olur. |
| silme *varsayılan veya port_num* | mycimprovauth 3308 | Belirtilen örnek ya da varsayılan olarak veya bağlantı noktası numarasına göre siler. |
| Yardım | mycimprov Yardım | Kullanılacak komutların listesini yazdırır. |
| Yazdırma | mycimprov yazdırma | Yazdırır bir kolayca MySQL OMI kimlik doğrulaması dosyası okunamıyor. |
| port_num güncelleştirme *bağlama adresi kullanıcı adı parola* | mycimprov güncelleştirme 3307 127.0.0.1 kök pwd | Belirtilen örnek güncelleştirir veya henüz yoksa örneği ekler. |

Aşağıdaki örnek komutlar, komut localhost üzerinde MySQL sunucusu için bir varsayılan kullanıcı hesabı tanımlayın.  Parola alanı düz metin biçiminde girilmesi gerekir - MySQL OMI kimlik doğrulaması dosyasındaki parola Base 64 kodlamalı olur

    sudo su omsagent -c '/opt/microsoft/mysql-cimprov/bin/mycimprovauth default 127.0.0.1 <username> <password>'
    sudo /opt/omi/bin/service_control restart

### <a name="database-permissions-required-for-mysql-performance-counters"></a>MySQL performans sayaçları için gereken veritabanı izinleri
MySQL kullanıcı MySQL sunucusu performans verilerini toplamak için aşağıdaki sorguları erişim gerektirir. 

    SHOW GLOBAL STATUS;
    SHOW GLOBAL VARIABLES:


MySQL kullanıcı ayrıca aşağıdaki varsayılan tablolar için seçme erişimi gerektirir.

- INFORMATION_SCHEMA
- mysql. 

Bu ayrıcalıkları verme aşağıdaki komutları çalıştırarak verilebilir.

    GRANT SELECT ON information_schema.* TO ‘monuser’@’localhost’;
    GRANT SELECT ON mysql.* TO ‘monuser’@’localhost’;


> [!NOTE]
> Bir MySQL için izinleri vermek için izleme kullanıcı ekibi tarafından verilmesinin verilmeden ayrıcalık yanı sıra, 'GRANT option' ayrıcalığı olmalıdır.

### <a name="define-performance-counters"></a>Performans sayaçları tanımlayın

Azure İzleyici için veri göndermek Linux için Log Analytics aracısını yapılandırdıktan sonra Toplanacak performans sayaçlarını yapılandırmanız gerekir.  Yordamı kullanın [Azure İzleyici'de Windows ve Linux performans veri kaynakları](data-sources-performance-counters.md) aşağıdaki tabloda sayaçlarla.

| Nesne Adı | Sayaç Adı |
|:--|:--|
| MySQL Veritabanı | Disk alanı bayt |
| MySQL Veritabanı | Tablolar |
| MySQL Server | Bağlantı durduruldu Pct |
| MySQL Server | Bağlantısı kullanım yüzdesi |
| MySQL Server | Disk alanı kullanımını bayt |
| MySQL Server | Tam tablo tarama Pct |
| MySQL Server | Innodb arabellek havuzu Pct isabet |
| MySQL Server | Innodb arabellek havuzu kullanımı yüzdesi |
| MySQL Server | Innodb arabellek havuzu kullanımı yüzdesi |
| MySQL Server | Pct anahtar önbellek isabet |
| MySQL Server | Anahtar önbellek kullanımı yüzdesi |
| MySQL Server | Anahtar önbellek yazma Pct |
| MySQL Server | Sorgu önbelleği isabet yüzdesi |
| MySQL Server | Sorgu önbelleği ayıklar Pct |
| MySQL Server | Sorgu önbelleği kullanımı yüzdesi |
| MySQL Server | Tablo önbelleği isabet yüzdesi |
| MySQL Server | Tablo önbellek kullanımı yüzdesi |
| MySQL Server | Tablo kilit Çekişme yüzdesi |

## <a name="apache-http-server"></a>Apache HTTP Server 
Apache HTTP Server omsagent paket yüklü olduğunda bilgisayarda algılanırsa, bir performans izleme için Apache HTTP Server sağlayıcısı otomatik olarak yüklenir. Bu sağlayıcı performans verilerine erişmek için Apache HTTP Server içinde yüklenmesi gereken bir Apache modülü kullanır. Şu komutla modülü yüklenebilir:
```
sudo /opt/microsoft/apache-cimprov/bin/apache_config.sh -c
```

Apache izleme modülünü kaldırmak için aşağıdaki komutu çalıştırın:
```
sudo /opt/microsoft/apache-cimprov/bin/apache_config.sh -u
```

### <a name="define-performance-counters"></a>Performans sayaçları tanımlayın

Azure İzleyici için veri göndermek Linux için Log Analytics aracısını yapılandırdıktan sonra Toplanacak performans sayaçlarını yapılandırmanız gerekir.  Yordamı kullanın [Azure İzleyici'de Windows ve Linux performans veri kaynakları](data-sources-performance-counters.md) aşağıdaki tabloda sayaçlarla.

| Nesne Adı | Sayaç Adı |
|:--|:--|
| Apache HTTP Server | Meşgul çalışanların |
| Apache HTTP Server | Boşta çalışan |
| Apache HTTP Server | Meşgul çalışanların PCT |
| Apache HTTP Server | Toplam CPU yüzdesi |
| Apache sanal ana bilgisayar | Dakikada - istemci hataları |
| Apache sanal ana bilgisayar | Dakikada - sunucu hataları |
| Apache sanal ana bilgisayar | İstek başına KB |
| Apache sanal ana bilgisayar | Saniye başına istek KB |
| Apache sanal ana bilgisayar | Saniye başına istek sayısı |



## <a name="next-steps"></a>Sonraki adımlar
* [Performans sayaçlarını Topla](data-sources-performance-counters.md) Linux aracılardan.
* Hakkında bilgi edinin [oturum sorguları](../log-query/log-query-overview.md) veri kaynakları ve çözümlerinden toplanan verileri analiz etmek için. 