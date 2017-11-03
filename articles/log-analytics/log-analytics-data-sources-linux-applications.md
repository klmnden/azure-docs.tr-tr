---
title: "Linux uygulama performansını OMS günlük analizi toplamak | Microsoft Docs"
description: "Bu makalede, MySQL ve Apache HTTP Server için performans sayaçları toplanamadı Linux için OMS aracısının yapılandırma ayrıntıları sağlar."
services: log-analytics
documentationcenter: 
author: mgoedtel
manager: carmonm
editor: tysonn
ms.assetid: f1d5bde4-6b86-4b8e-b5c1-3ecbaba76198
ms.service: log-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/04/2017
ms.author: magoedte
ms.openlocfilehash: 04ea6f728e59ec8b47e54fe45e1adc6cbbfb85ff
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="collect-performance-counters-for-linux-applications-in-log-analytics"></a>Günlük analizi Linux uygulamaları için performans sayaçlarını Topla 
Bu makalede yapılandırmak için Ayrıntılar sunulmaktadır [Linux için OMS aracısının](https://github.com/Microsoft/OMS-Agent-for-Linux) belirli uygulamalar için performans sayaçları toplanamadı.  Bu makalede bulunan uygulamalar şunlardır:  

- [MySQL](#MySQL)
- [Apache HTTP Server](#apache-http-server)

## <a name="mysql"></a>MySQL
MySQL Server veya MariaDB Server OMS aracısı yüklü olduğunda bilgisayarda algılanırsa, bir performans izleme sağlayıcısı MySQL sunucusu için otomatik olarak yüklenir. Bu sağlayıcı performans istatistiklerini kullanıma sunmak için yerel MySQL/MariaDB sunucusuna bağlanır. MySQL kullanıcı kimlik bilgileri sağlayıcısı MySQL Server erişebilmesi için yapılandırılması gerekir.

### <a name="configure-mysql-credentials"></a>MySQL kimlik bilgilerini yapılandırın
MySQL OMI sağlayıcı önceden yapılandırılmış bir MySQL kullanıcının gerektiren ve performans ve sistem durumu bilgilerini MySQL örneğinden sorgulamak için MySQL istemci kitaplıkları yüklü.  Bu kimlik bilgileri Linux Aracısı'nı depolanan bir kimlik doğrulama dosyasında depolanır.  Kimlik doğrulama dosyasını hangi bağlama adresi ve bağlantı noktası MySQL örneğine dinlediği ve ne ölçümleri toplamak için kullanılacak kimlik bilgilerini belirtir.  

Linux MySQL OMI için OMS aracısının yüklemesi sırasında sağlayıcısı bağ adresi ve bağlantı noktası için MySQL my.cnf yapılandırma dosyalarını (varsayılan konumları) tarama ve MySQL OMI kimlik doğrulama dosyasını kısmen ayarlayın.

MySQL kimlik doğrulaması dosya konumuna depolandı `/var/opt/microsoft/mysql-cimprov/auth/omsagent/mysql-auth`.


### <a name="authentication-file-format"></a>Kimlik doğrulama dosya biçimi
MySQL OMI kimlik doğrulaması dosya için biçimlendirme aşağıda verilmiştir

    [Port]=[Bind-Address], [username], [Base64 encoded Password]
    (Port)=(Bind-Address), (username), (Base64 encoded Password)
    (Port)=(Bind-Address), (username), (Base64 encoded Password)
    AutoUpdate=[true|false]

Giriş kimlik doğrulama dosyası aşağıdaki tabloda açıklanmıştır.

| Özellik | Açıklama |
|:--|:--|
| Bağlantı noktası | MySQL örneğine dinlediği geçerli bağlantı noktası temsil eder. Aşağıdaki özellikler varsayılan örnek için kullanılan bağlantı noktası 0 belirtir. |
| Bağ adresi| Geçerli MySQL bağlama-adresi. |
| kullanıcı adı| MySQL kullanıcı MySQL server örneği izlemek üzere kullanmak için kullanılır. |
| Parola Base64 ile kodlanmış| Base64 ile kodlanmış MySQL izleme kullanıcının parolası. |
| Otomatik güncelleştirme| My.cnf dosyasındaki değişiklikleri için yeniden tarayın ve MySQL OMI sağlayıcı yükseltildiğinde MySQL OMI kimlik doğrulaması bu dosyanın üzerine belirtir. |

### <a name="default-instance"></a>Varsayılan örneği
MySQL OMI kimlik doğrulama dosyasını daha kolay bir Linux ana bilgisayarda birden çok MySQL örneklerini yönetme yapmak için bir varsayılan örneği ve bağlantı noktası numarası tanımlayabilirsiniz.  Varsayılan örneği, bağlantı noktası 0 ile bir örneği tarafından belirtilir. Tüm ek örnekleri farklı değerler belirtmediğiniz sürece varsayılan örnekten ayarlanan özellikleri devralır. MySQL örneği '3308' numaralı bağlantı noktasını dinlemeye eklediyseniz, örneğin, varsayılan örneğinin bağ adresi, kullanıcı adı ve Base64 ile kodlanmış parola deneyin ve 3308 üzerinde dinleme örneği izlemek için kullanılacak. 3308 örneğinde başka bir adresine bağlanır ve kullanıcı adı ve parola aynı MySQL çifti kullanıyorsa yalnızca bağ adresi gereklidir ve diğer özellikleri devralınır.

Aşağıdaki tabloda örnek örneği ayarlara sahip 

| Açıklama | Dosya |
|:--|:--|
| Varsayılan örneği ve bağlantı noktası 3308 ile örneği. | `0=127.0.0.1, myuser, cnBwdA==`<br>`3308=, ,`<br>`AutoUpdate=true` |
| Varsayılan örneği ve bağlantı noktası 3308 ve farklı bir kullanıcı adı ve parola ile örneği. | `0=127.0.0.1, myuser, cnBwdA==`<br>`3308=127.0.1.1, myuser2,cGluaGVhZA==`<br>`AutoUpdate=true` |


### <a name="mysql-omi-authentication-file-program"></a>MySQL OMI kimlik doğrulaması dosya programı
MySQL OMI sağlayıcısı yüklemesinde MySQL OMI kimlik doğrulama dosyasını düzenlemek için kullanılan bir MySQL OMI kimlik doğrulaması dosya programdır. Kimlik doğrulama dosya programı şu konumda bulunabilir.

    /opt/microsoft/mysql-cimprov/bin/mycimprovauth

> [!NOTE]
> Kimlik bilgileri dosyası omsagent hesap tarafından okunabilir olması gerekir. Omsgent mycimprovauth komutunun çalıştırılması önerilir.

Aşağıdaki tabloda, mycimprovauth kullanmak için söz dizimi hakkında ayrıntılar sağlar.

| İşlem | Örnek | Açıklama
|:--|:--|:--|
| Otomatik güncelleştirme * false\|TRUE * | mycimprovauth otomatik güncelleştirme false | Desteklemediğini kimlik doğrulama dosyasını otomatik olarak güncelleştirilecek kümeleri üzerinde yeniden başlatın veya güncelleştirin. |
| Varsayılan *bağ adresi kullanıcı adı parolası* | mycimprovauth varsayılan 127.0.0.1 kök pwd | Varsayılan örnek MySQL OMI kimlik doğrulama dosyasını ayarlar.<br>Parola alanı düz metin olarak girilmelidir - MySQL OMI authentication dosyasındaki parola Base 64 kodlu olacaktır. |
| silme * default\|port_num * | mycimprovauth 3308 | Belirtilen örnek ya da varsayılan olarak veya bağlantı noktası numarasına göre siler. |
| Yardım | mycimprov Yardım | Kullanılacak komutların listesini yazdırır. |
| Yazdırma | mycimprov yazdırma | Yazdırır kolay bir kimlik doğrulama dosyasını MySQL OMI okuyun. |
| port_num güncelleştirme *bağ adresi kullanıcı adı parolası* | mycimprov güncelleştirme 3307 127.0.0.1 kök pwd | Belirtilen örnek güncelleştirir veya henüz yoksa örneği ekler. |

Aşağıdaki örnek komutlar MySQL sunucusu için varsayılan bir kullanıcı hesabı üzerinde localhost tanımlayın.  Parola alanı düz metin olarak girilmelidir - MySQL OMI authentication dosyasındaki parola Base 64 kodlu olacak

    sudo su omsagent -c '/opt/microsoft/mysql-cimprov/bin/mycimprovauth default 127.0.0.1 <username> <password>'
    sudo /opt/omi/bin/service_control restart

### <a name="database-permissions-required-for-mysql-performance-counters"></a>MySQL performans sayaçları için gereken veritabanı izinleri
MySQL kullanıcı MySQL Server performans verilerini toplamak için aşağıdaki sorguları erişim gerektirir. 

    SHOW GLOBAL STATUS;
    SHOW GLOBAL VARIABLES:


MySQL kullanıcı aynı zamanda aşağıdaki varsayılan tabloları seçme erişim gerektirir.

- INFORMATION_SCHEMA
- MySQL. 

Bu ayrıcalıklar aşağıdaki grant komutlarını çalıştırarak verilebilir.

    GRANT SELECT ON information_schema.* TO ‘monuser’@’localhost’;
    GRANT SELECT ON mysql.* TO ‘monuser’@’localhost’;


> [!NOTE]
> MySQL için izinleri vermek için izleme kullanıcı izni veriliyor verilmeden ayrıcalık yanı sıra, 'GRANT option' ayrıcalığı olmalıdır.

### <a name="define-performance-counters"></a>Performans sayaçları tanımlayın

Günlük analizi veri göndermek Linux için OMS aracısının yapılandırdıktan sonra Toplanacak performans sayaçlarını yapılandırmanız gerekir.  Yordamı kullanın [Windows ve Linux performans veri kaynaklarında günlük analizi](log-analytics-data-sources-windows-events.md) aşağıdaki tabloda sayaçlarla.

| Nesne adı | Sayaç adı |
|:--|:--|
| MySQL Veritabanı | Disk alanı bayt |
| MySQL Veritabanı | Tablolar |
| MySQL sunucusu | Bağlantı durduruldu Pct |
| MySQL sunucusu | Bağlantı kullanım yüzdesi |
| MySQL sunucusu | Disk alanı kullanımını bayt |
| MySQL sunucusu | Tam tablo tarama Pct |
| MySQL sunucusu | Pct InnoDB arabellek havuzu ulaştı. |
| MySQL sunucusu | InnoDB arabellek havuzu kullanım yüzdesi |
| MySQL sunucusu | InnoDB arabellek havuzu kullanım yüzdesi |
| MySQL sunucusu | Anahtar önbelleği isabet yüzdesi |
| MySQL sunucusu | Anahtar önbelleği kullanım yüzdesi |
| MySQL sunucusu | Anahtar önbelleği yazma Pct |
| MySQL sunucusu | Sorgu önbellek isabet yüzdesi |
| MySQL sunucusu | Sorgu önbellek ayıklar Pct |
| MySQL sunucusu | Sorgu önbellek kullanım yüzdesi |
| MySQL sunucusu | Tablo önbelleği isabet yüzdesi |
| MySQL sunucusu | Tablo Önbellek kullanım yüzdesi |
| MySQL sunucusu | Tablo kilit çekişmesini Pct |

## <a name="apache-http-server"></a>Apache HTTP Server 
Apache HTTP Server omsagent paket yüklü olduğunda bilgisayarda algılanırsa, bir performans izlemesi için Apache HTTP Server sağlayıcısı otomatik olarak yüklenir. Bu sağlayıcı Apache HTTP Server performans verilerini erişmek için yüklenmesi gereken bir Apache modülü kullanır. Modül aşağıdaki komutla yüklenebilir:
```
sudo /opt/microsoft/apache-cimprov/bin/apache_config.sh -c
```

Apache izleme modülü kaldırmak için aşağıdaki komutu çalıştırın:
```
sudo /opt/microsoft/apache-cimprov/bin/apache_config.sh -u
```

### <a name="define-performance-counters"></a>Performans sayaçları tanımlayın

Günlük analizi veri göndermek Linux için OMS aracısının yapılandırdıktan sonra Toplanacak performans sayaçlarını yapılandırmanız gerekir.  Yordamı kullanın [Windows ve Linux performans veri kaynaklarında günlük analizi](log-analytics-data-sources-windows-events.md) aşağıdaki tabloda sayaçlarla.

| Nesne adı | Sayaç adı |
|:--|:--|
| Apache HTTP Server | Meşgul çalışanların |
| Apache HTTP Server | Boşta çalışan |
| Apache HTTP Server | PCT meşgul çalışanların |
| Apache HTTP Server | Toplam Pct CPU |
| Apache sanal ana bilgisayar | Dakikada - istemci hataları |
| Apache sanal ana bilgisayar | Dakikada - sunucu hataları |
| Apache sanal ana bilgisayar | İstek başına KB |
| Apache sanal ana bilgisayar | Saniye başına istek KB |
| Apache sanal ana bilgisayar | Saniye başına istek sayısı |



## <a name="next-steps"></a>Sonraki adımlar
* [Performans sayaçlarını Topla](log-analytics-data-sources-performance-counters.md) Linux aracılardan gelen.
* Hakkında bilgi edinin [oturum aramaları](log-analytics-log-searches.md) veri kaynakları ve çözümleri toplanan verileri çözümlemek için. 