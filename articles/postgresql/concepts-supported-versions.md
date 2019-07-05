---
title: PostgreSQL - tek bir sunucu için Azure veritabanı'nda desteklenen sürümler
description: -Tek bir sunucu PostgreSQL için Azure veritabanı'nda desteklenen sürümleri açıklar.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 06/11/2019
ms.custom: fasttrack-edit
ms.openlocfilehash: c328f0dc510c1b7d36b547e6560a292f98d72f6f
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67448035"
---
# <a name="supported-postgresql-database-versions"></a>Desteklenen PostgreSQL veritabanı sürümleri
PostgreSQL altyapısının n-2 sürümlerini - tek bir sunucu PostgreSQL için Azure veritabanı'nda desteklemek için Microsoft amaçlar. Sürümler, geçerli ana sürüme (n) azure'da ve iki önceki ana sürümü (-2) olacaktır.

PostgreSQL için Azure veritabanı şu anda aşağıdaki sürümlerini destekler:

## <a name="postgresql-version-112"></a>PostgreSQL sürümü 11.2
Başvurmak [PostgreSQL belgeleri](https://www.postgresql.org/docs/11/static/release-11-2.html) iyileştirmeler ve düzeltmeler alt bu sürüm hakkında daha fazla bilgi için.

>[!NOTE]
> PostgreSQL sürümü 11 önizlemede kullanılabilir. Azure portalını kullanarak oluşturma desteği kullanıma sunulacaktır ve henüz bölgenizde kullanılamıyor olabilir. Kullanabileceğiniz [Azure CLI](quickstart-create-server-database-azure-cli.md) herhangi bir bölgede Postgres 11 sunucu oluşturmak için. Örneğin, `az postgres server create -g group -n server -u username -p password -l westeurope --sku-name GP_Gen5_2 --version 11`.

## <a name="postgresql-version-107"></a>PostgreSQL sürümü 10.7
Başvurmak [PostgreSQL belgeleri](https://www.postgresql.org/docs/10/static/release-10-7.html) iyileştirmeler ve düzeltmeler alt bu sürüm hakkında daha fazla bilgi için.

## <a name="postgresql-version-9612"></a>PostgreSQL sürümü 9.6.12
Başvurmak [PostgreSQL belgeleri](https://www.postgresql.org/docs/9.6/static/release-9-6-12.html) iyileştirmeler ve düzeltmeler alt bu sürüm hakkında daha fazla bilgi için.

## <a name="postgresql-version-9516"></a>PostgreSQL sürümü 9.5.16
Başvurmak [PostgreSQL belgeleri](https://www.postgresql.org/docs/9.5/static/release-9-5-16.html) iyileştirmeler ve düzeltmeler hakkında bilgi edinmek için bu ikincil sürüm.

## <a name="managing-updates-and-upgrades"></a>Yönetme güncelleştirmeler ve yükseltmeler
PostgreSQL için Azure veritabanı, ikincil sürüm düzeltme eklerini otomatik olarak yönetir. Şu anda ana sürüm yükseltme desteklenmez. Örneğin, PostgreSQL 9.6 için PostgreSQL 9.5 ' yükseltme desteklenmiyor. Bir sonraki ana sürümüne yükseltmek istiyorsanız, bir veritabanı oluşturma [döküm ve geri yükleme](./howto-migrate-using-dump-and-restore.md) yeni altyapısı sürümü ile oluşturulmuş bir sunucu için.

> Sürüm 10, PostgreSQL önce dikkat [PostgreSQL sürüm oluşturma ilkesine](https://www.postgresql.org/support/versioning/) kabul bir _ana sürüm_ ilk artışı olacak şekilde yükseltme _veya_ numarası (ikinci Örneğin, 9.5 için 9.6 kabul bir _ana_ sürüm yükseltmesi).
> 10 sürümden itibaren bir ana sürüm yükseltmesi yalnızca ilk sayı bir değişiklik olarak kabul edilir (örneğin, 10.0 için 10.1 olan bir _küçük_ sürüm yükseltmesi ve 10-11 bir _ana_ sürüm yükseltmesi).

## <a name="next-steps"></a>Sonraki adımlar
PostgreSQL Uzantıları desteği hakkında daha fazla bilgi için bkz. [PostgreSQL uzantıları](concepts-extensions.md).
