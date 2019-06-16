---
title: MariaDB için Azure veritabanı'nda bağlantı mimarisi
description: MariaDB için Azure veritabanı sunucunuza için bağlantı mimarisini açıklar.
author: kummanish
ms.author: manishku
ms.service: mariaDB
ms.topic: conceptual
ms.date: 05/23/2019
ms.openlocfilehash: 126e7ce4d9784a2f6a59f39562dff9a0b9d60ea0
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66735399"
---
# <a name="connectivity-architecture-in-azure-database-for-mariadb"></a>MariaDB için Azure veritabanı'nda bağlantı mimarisi
Bu makalede, hem içinde hem de Azure dışındaki istemcilerden MariaDB örneği için Azure veritabanı sunucunuza trafiği yönlendirilmiş nasıl bağlantı mimarisi de MariaDB için Azure veritabanı açıklanmaktadır.

## <a name="connectivity-architecture"></a>Bağlantı mimarisi

MariaDB için Azure veritabanınıza bağlantı sunucunuzun bizim kümelerdeki fiziksel konuma yönlendirme gelen bağlantıları sorumlu olduğu bir ağ geçidi üzerinden kurulur. Aşağıdaki diyagram, trafik akışını gösterir.

![Bağlantı mimarisine genel bakış](./media/concepts-connectivity-architecture/connectivity-architecture-overview-proxy.png)

İstemci olarak veritabanına bağlanmak, bunların ağ geçidine bağlayan bir bağlantı dizesini alın. Bu ağ geçidi 3306 numaralı bağlantı noktasını dinleyen bir genel IP adresi vardır. Veritabanı kümesi içinde trafik, MariaDB için uygun Azure veritabanı'na iletilir. Bu nedenle, Kurumsal ağlardan gibi sunucunuza bağlanması için ağ geçitleriyle ulaşmak giden trafiğe izin vermek için istemci tarafı Güvenlik Duvarı'nı açmak gereklidir. Aşağıda, bölge başına ağ geçitleriyle tarafından kullanılan IP adresleri, tam listesini bulabilirsiniz.

## <a name="azure-database-for-mariadb-gateway-ip-addresses"></a>Ağ geçidi IP adreslerini MariaDB için Azure veritabanı

Aşağıdaki tabloda birincil ve ikincil IP'ler MariaDB ağ geçidi için tüm veri bölgeleri için Azure veritabanı'nın listeler. Birincil IP adresi geçerli bir ağ geçidi IP adresini ve ikinci IP adresini bir yük devretme IP adresi birincil başarısız olması durumunda. Belirtildiği gibi müşterilerin her iki IP adreslerine giden izin vermelidir. Bağlantıları kabul etmek üzere MariaDB için Azure veritabanı tarafından etkinleştirilene kadar ikinci IP adresini hizmetlerin üzerinde dinlemez.

| **Bölge adı** | **Birincil IP adresi** | **İkincil IP adresi** |
|:----------------|:-------------|:------------------------|
| Avustralya Doğu | 13.75.149.87 | 40.79.161.1 |
| Avustralya Güneydoğu | 191.239.192.109 | 13.73.109.251 |
| Güney Brezilya | 104.41.11.5 | |
| Orta Kanada | 40.85.224.249 | |
| Doğu Kanada | 40.86.226.166 | |
| Orta ABD | 23.99.160.139 | 13.67.215.62 |
| Çin Doğu 1 | 139.219.130.35 | |
| Çin Doğu 2 | 40.73.82.1 | |
| Çin Kuzey 1 | 139.219.15.17 | |
| Çin Kuzey 2 | 40.73.50.0 | |
| Doğu Asya | 191.234.2.139 | 52.175.33.150 |
| Doğu ABD 1 | 191.238.6.43 | 40.121.158.30 |
| Doğu ABD 2 | 191.239.224.107 | 40.79.84.180 * |
| Fransa Orta | 40.79.137.0 | 40.79.129.1 |
| Almanya Orta | 51.4.144.100 | |
| Hindistan Orta | 104.211.96.159 | |
| Hindistan Güney | 104.211.224.146 | |
| Hindistan Batı | 104.211.160.80 | |
| Japonya Doğu | 191.237.240.43 | 13.78.61.196 |
| Japonya Batı | 191.238.68.11 | 104.214.148.156 |
| Kore Orta | 52.231.32.42 | |
| Kore Güney | 52.231.200.86 |  |
| Orta Kuzey ABD | 23.98.55.75 | 23.96.178.199 |
| Kuzey Avrupa | 191.235.193.75 | 40.113.93.91 |
| Orta Güney ABD | 23.98.162.75 | 13.66.62.124 |
| Güneydoğu Asya | 23.100.117.95 | 104.43.15.0 |
| Birleşik Krallık Güney | 51.140.184.11 | |
| Birleşik Krallık Batı | 51.141.8.11| |
| Batı Avrupa | 191.237.232.75 | 40.68.37.158 |
| Batı ABD 1 | 23.99.34.75 | 104.42.238.205 |
| Batı ABD 2 | 13.66.226.202 | |
||||

> [!NOTE]
> *Doğu ABD 2* üçüncül bir IP adresi de sahip `52.167.104.0`.

## <a name="next-steps"></a>Sonraki adımlar

* [Oluşturma ve Azure veritabanı Azure portalını kullanarak MariaDB için güvenlik duvarı kurallarını yönetme](./howto-manage-firewall-portal.md)
* [Oluşturma ve Azure veritabanı Azure CLI kullanarak MariaDB için güvenlik duvarı kurallarını yönetme](./howto-manage-firewall-cli.md)
