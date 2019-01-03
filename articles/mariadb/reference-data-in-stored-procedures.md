---
title: MariaDB veri çoğaltma için Azure veritabanı saklı yordamlar
description: Bu makalede, veri çoğaltma için kullanılan tüm depolanmış yordamları sunar.
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.topic: reference
ms.date: 09/24/2018
ms.openlocfilehash: 75dc10ba3d95fd12ea99e10d321237560ee28171
ms.sourcegitcommit: 71ee622bdba6e24db4d7ce92107b1ef1a4fa2600
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/17/2018
ms.locfileid: "53535361"
---
# <a name="azure-database-for-mariadb-data-in-replication-stored-procedures"></a>MariaDB veri çoğaltma için Azure veritabanı saklı yordamlar

Veri çoğaltma, sanal makineleri veya diğer bulut sağlayıcılarının MariaDB hizmeti için Azure veritabanı'nda barındırılan veritabanı hizmetleri, şirket içinde çalışan bir MariaDB sunucudan veri eşitlemenize olanak tanır.

Aşağıdaki saklı yordamlara ayarlayın veya verileri, bir ana çoğaltma arasında çoğaltmayı kaldırmak için kullanılır.

|**Saklı yordam adı**|**Giriş parametreleri**|**Çıktı parametreleri**|**Kullanım notu**|
|-----|-----|-----|-----|
|*MySQL.az_replication_change_master*|master_host<br/>master_user<br/>master_password<br/>master_port<br/>master_log_file<br/>master_log_pos<br/>master_ssl_ca|Yok|SSL modu ile veri aktarımı için CA sertifikanın bağlamda master_ssl_ca parametresine geçirin. </br><br>SSL olmayan veri aktarmak için boş bir dize içinde master_ssl_ca parametresine geçirin.|
|*MySQL.az_replication _başlatmayı*|Yok|Yok|Çoğaltma başlatır.|
|*MySQL.az_replication _hata*|Yok|Yok|Çoğaltmayı durdurur.|
|*MySQL.az_replication _remove_master*|Yok|Yok|Ana ve çoğaltma arasındaki çoğaltma ilişkisini kaldırır.|
|*MySQL.az_replication_skip_counter*|Yok|Yok|Bir çoğaltma hatası atlar.|

Veri çoğaltma bir ana ve Azure veritabanı'nda çoğaltma arasında MariaDB için ayarlamak için başvurmak [veri çoğaltma yapılandırma](howto-data-in-replication.md).
