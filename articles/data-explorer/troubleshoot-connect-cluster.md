---
title: Azure veri Gezgini'nde bir kümeye bağlanma hatası
description: Bu makalede Azure.Data Gezgini'nde bir kümeye bağlanmak için sorun giderme adımları açıklanmaktadır.
author: orspod
ms.author: orspodek
ms.reviewer: mblythe
ms.service: data-explorer
services: data-explorer
ms.topic: conceptual
ms.date: 09/24/2018
ms.openlocfilehash: 66d77a11f0f68a2dc48802ed21acca8420012207
ms.sourcegitcommit: 563f8240f045620b13f9a9a3ebfe0ff10d6787a2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/01/2019
ms.locfileid: "58758555"
---
# <a name="troubleshoot-failure-to-connect-to-a-cluster-in-azure-data-explorer"></a>Sorun giderme: Azure veri Gezgini'nde bir kümeye bağlanma hatası

Azure veri Gezgini'nde bir kümeye bağlanmak için değilseniz, aşağıdaki adımları izleyin.

1. Bağlantı dizesinin doğru olduğundan emin olun. Biçiminde olmalıdır: `https://<ClusterName>.<Region>.kusto.windows.net`, aşağıdaki örnek gibi: `https://docscluster.westus.kusto.windows.net`.

1. Yeterli izinlere sahip olun. Aksi takdirde, yanıtı alırsınız *yetkisiz*.

    İzinler hakkında daha fazla bilgi için bkz. [veritabanı izinlerini yönetmek](manage-database-permissions.md). Gerekli, bunlar için uygun rolü ekleyebilmeniz için küme yöneticinize ile çalışır.

1. Küme silinmiş taşınmadığından doğrulayın: aboneliğinizdeki Etkinlik günlüğünü gözden geçirin.

1. Denetleme [Azure hizmet durumu Panosu](https://azure.microsoft.com/status/). Azure veri Gezgini'nde bir kümeye bağlanmak için burada çalıştığınız bölge durumunu arayın.

    Durum yoksa **iyi** deneyin, sonra durum kümeye bağlanarak (yeşil onay işareti) geliştirir.

1. Hala sorununuzun çözümüyle ilgili yardıma ihtiyacınız varsa, Lütfen bir destek isteği açın [Azure portalında](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview).