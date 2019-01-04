---
title: MySQL veri çoğaltma için Azure veritabanı saklı yordamlar
description: Bu makalede, veri çoğaltma için kullanılan tüm depolanmış yordamları sunar.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.topic: conceptual
ms.date: 08/31/2018
ms.openlocfilehash: a3c88953eea95871529e8ab257f52b694db443a9
ms.sourcegitcommit: 71ee622bdba6e24db4d7ce92107b1ef1a4fa2600
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/17/2018
ms.locfileid: "53544864"
---
# <a name="azure-database-for-mysql-data-in-replication-stored-procedures"></a>MySQL veri çoğaltma için Azure veritabanı saklı yordamlar

Veri çoğaltma, verileri bir MySQL sunucusu şirket içi sanal makineleri veya diğer bulut sağlayıcılarının MySQL hizmeti için Azure veritabanı'nda barındırılan veritabanı Hizmetleri çalıştıran eşitlemenize olanak tanır.

Aşağıdaki saklı yordamlara ayarlayın veya verileri, bir ana çoğaltma arasında çoğaltmayı kaldırmak için kullanılır.

|**Saklı yordam adı**|**Giriş parametreleri**|**Çıktı parametreleri**|**Kullanım notu**|
|-----|-----|-----|-----|
|*MySQL.az_replication_change_master*|master_host<br/>master_user<br/>master_password<br/>master_port<br/>master_log_file<br/>master_log_pos<br/>master_ssl_ca|Yok|SSL modu ile veri aktarımı için CA sertifikanın bağlamda master_ssl_ca parametresine geçirin. </br><br>SSL olmayan veri aktarmak için boş bir dize içinde master_ssl_ca parametresine geçirin.|
|*MySQL.az_replication _başlatmayı*|Yok|Yok|Çoğaltma başlatır.|
|*MySQL.az_replication _hata*|Yok|Yok|Çoğaltmayı durdurur.|
|*MySQL.az_replication _remove_master*|Yok|Yok|Ana ve çoğaltma arasındaki çoğaltma ilişkisini kaldırır.|
|*MySQL.az_replication_skip_counter*|Yok|Yok|Bir çoğaltma hatası atlar.|

Veri çoğaltma bir ana ve Azure veritabanı'nda çoğaltma arasında MySQL için ayarlamak için başvurmak [veri çoğaltma yapılandırma](howto-data-in-replication.md).
