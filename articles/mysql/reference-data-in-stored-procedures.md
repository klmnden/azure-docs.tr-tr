---
title: MySQL veri çoğaltma için Azure veritabanı saklı yordamlar
description: Bu makalede, veri çoğaltma için kullanılan tüm depolanmış yordamları sunar.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.topic: conceptual
ms.date: 08/31/2018
ms.openlocfilehash: a3c88953eea95871529e8ab257f52b694db443a9
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61244319"
---
# <a name="azure-database-for-mysql-data-in-replication-stored-procedures"></a>MySQL veri çoğaltma için Azure veritabanı saklı yordamlar

Veri çoğaltma, verileri bir MySQL sunucusu şirket içi sanal makineleri veya diğer bulut sağlayıcılarının MySQL hizmeti için Azure veritabanı'nda barındırılan veritabanı Hizmetleri çalıştıran eşitlemenize olanak tanır.

Aşağıdaki saklı yordamlara ayarlayın veya verileri, bir ana çoğaltma arasında çoğaltmayı kaldırmak için kullanılır.

|**Saklı yordam adı**|**Giriş parametreleri**|**Çıktı parametreleri**|**Kullanım notu**|
|-----|-----|-----|-----|
|*mysql.az_replication_change_master*|master_host<br/>master_user<br/>master_password<br/>master_port<br/>master_log_file<br/>master_log_pos<br/>master_ssl_ca|Yok|SSL modu ile veri aktarımı için CA sertifikanın bağlamda master_ssl_ca parametresine geçirin. </br><br>SSL olmayan veri aktarmak için boş bir dize içinde master_ssl_ca parametresine geçirin.|
|*mysql.az_replication _start*|Yok|Yok|Çoğaltma başlatır.|
|*mysql.az_replication _stop*|Yok|Yok|Çoğaltmayı durdurur.|
|*mysql.az_replication _remove_master*|Yok|Yok|Ana ve çoğaltma arasındaki çoğaltma ilişkisini kaldırır.|
|*mysql.az_replication_skip_counter*|Yok|Yok|Bir çoğaltma hatası atlar.|

Veri çoğaltma bir ana ve Azure veritabanı'nda çoğaltma arasında MySQL için ayarlamak için başvurmak [veri çoğaltma yapılandırma](howto-data-in-replication.md).
