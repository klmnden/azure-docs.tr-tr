---
title: Tablo denetim TDS Yönlendirme ve Azure SQL veritabanı için IP uç noktaları | Microsoft Docs
description: Denetim, TDS Yönlendirme ve IP uç noktası değişiklikleri Azure SQL veritabanı'nda Tablo denetleme uygularken öğrenin.
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: ronitr
ms.author: ronitr
ms.reviewer: vanto
manager: craigg
ms.date: 02/25/2019
ms.openlocfilehash: 2c95ec4d88e55af0becc73719bcc6126501267db
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61416535"
---
# <a name="sql-database----downlevel-clients-support-and-ip-endpoint-changes-for-table-auditing"></a>SQL veritabanı - alt düzey istemci desteği ve tablo denetimi için IP uç noktası değişiklikleri

> [!IMPORTANT]
> Bu belgede yalnızca tablo olan denetimi için geçerlidir. **artık kullanım dışı**.<br>
> Lütfen yeni kullanın [Blob denetimi](sql-database-auditing.md) yöntemi, hangi **yok** alt düzey istemci bağlantı dizesi değişiklikler gerektirir. Blob denetimi hakkında ek bilgiler bulunabilir [SQL veritabanı denetimini kullanmaya başlama](sql-database-auditing.md).

[Veritabanı denetim](sql-database-auditing.md) çalışır otomatik olarak TDS yeniden yönlendirmeyi destekleyen SQL istemcileri ile. Unutmayın. Bu yeniden yönlendirme Blob denetimi yöntemi kullanılırken geçerli değildir.

## <a id="subheading-1"></a>Alt düzey istemci desteği

TDS 7.4 uygulayan herhangi bir istemci yeniden yönlendirme de desteklemelidir. Bunun istisnası, JDBC, yeniden yönlendirme özelliği tam olarak desteklenmiyor ve hangi yeniden yönlendirmesi Node.JS için Tedious uygulanmadı 4.0 içerir.

"Alt düzey istemciler için", yani hangi destek TDS sürüm 7.3 ve aşağıda - bir sunucunun FQDN bağlantı dizesi değiştirilmelidir:

Bağlantı dizesindeki özgün sunucunun FQDN: <*sunucu adı*>. database.windows.net

Bağlantı dizesindeki değiştirilmiş sunucusu FQDN'si: <*sunucu adı*> .database. **güvenli**. windows.net

"Alt düzey istemciler" kısmi bir listesine içerir:

* .NET 4.0 ve aşağıdaki
* ODBC 10.0 ve altı.
* JDBC (JDBC TDS 7.4 desteklese de TDS yeniden yönlendirme özelliği tam olarak desteklenmiyor)
* (Node.JS için) tedious

**Açıklama:** Yukarıdaki sunucunun FQDN değişikliği her bir veritabanındaki (geçici azaltma) bir yapılandırma adımı gerek olmadan bir SQL sunucu düzeyi denetimi ilkesi uygulamak için de yararlı olabilir.

## <a id="subheading-2"></a>Denetim etkinleştirilirken IP uç noktası değişiklikleri

Tablo denetimi etkinleştirdiğinizde, IP uç veritabanınızın değişeceğini unutmayın. Katı güvenlik duvarı ayarları varsa, lütfen bu güvenlik duvarı ayarlarını güncelleştirin uygun şekilde.

Yeni veritabanı IP uç noktası veritabanı bölgesine bağlıdır:

| Veritabanı bölgesi | Olası IP uç noktaları |
| --- | --- |
| Çin Kuzey |139.217.29.176, 139.217.28.254 |
| Çin Doğu |42.159.245.65, 42.159.246.245 |
| Avustralya Doğu |104.210.91.32, 40.126.244.159, 191.239.64.60, 40.126.255.94 |
| Avustralya Güneydoğu |191.239.184.223, 40.127.85.81, 191.239.161.83, 40.127.81.130 |
| Güney Brezilya |104.41.44.161, 104.41.62.230, 23.97.99.54, 104.41.59.191 |
| Orta ABD |104.43.255.70, 40.83.14.7, 23.99.128.244, 40.83.15.176 |
| Orta ABD EUAP |52.180.178.16, 52.180.176.190 |
| Doğu Asya |23.99.125.133, 13.75.40.42, 23.97.71.138, 13.94.43.245 |
| Doğu ABD 2 |104.209.141.31, 104.208.238.177, 191.237.131.51, 104.208.235.50 |
| Doğu ABD |23.96.107.223, 104.41.150.122, 23.96.38.170, 104.41.146.44 |
| Doğu ABD EUAP |52.225.190.86, 52.225.191.187 |
| Orta Hindistan |104.211.98.219, 104.211.103.71 |
| Güney Hindistan |104.211.227.102, 104.211.225.157 |
| Batı Hindistan |104.211.161.152, 104.211.162.21 |
| Japonya Doğu |104.41.179.1, 40.115.253.81, 23.102.64.207, 40.115.250.196 |
| Japonya Batı |104.214.140.140, 104.214.146.31, 191.233.32.34, 104.214.146.198 |
| Orta Kuzey ABD |191.236.155.178, 23.96.192.130, 23.96.177.169, 23.96.193.231 |
| Kuzey Avrupa |104.41.209.221, 40.85.139.245, 137.116.251.66, 40.85.142.176 |
| Orta Güney ABD |191.238.184.128, 40.84.190.84, 23.102.160.153, 40.84.186.66 |
| Güneydoğu Asya |104.215.198.156, 13.76.252.200, 23.97.51.109, 13.76.252.113 |
| Batı Avrupa |104.40.230.120, 13.80.23.64, 137.117.171.161, 13.80.8.37, 104.47.167.215, 40.118.56.193, 104.40.176.73, 40.118.56.20 |
| Batı ABD |191.236.123.146, 138.91.163.240, 168.62.194.148, 23.99.6.91 |
| Batı ABD 2 |13.66.224.156, 13.66.227.8 |
| Batı Orta ABD |52.161.29.186, 52.161.27.213 |
| Orta Kanada |13.88.248.106, 13.88.248.110 |
| Doğu Kanada |40.86.227.82, 40.86.225.194 |
| Birleşik Krallık Güney |13.87.32.202, 13.87.32.226 |
