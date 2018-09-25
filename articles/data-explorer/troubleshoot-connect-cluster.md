---
title: Azure veri Gezgini'nde bir kümeye bağlanma hatası
description: Bu makalede Azure.Data Gezgini'nde bir kümeye bağlanmak için sorun giderme adımları açıklanmaktadır.
author: orspod
ms.author: v-orspod
ms.reviewer: mblythe
ms.service: data-explorer
services: data-explorer
ms.topic: conceptual
ms.date: 09/24/2018
ms.openlocfilehash: f66dcc55b01b407c59c65ea300757ab4ee1002ac
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "46965067"
---
# <a name="troubleshoot-failure-to-connect-to-a-cluster-in-azure-data-explorer"></a>Sorunlarını giderme: Azure veri Gezgini'nde bir kümeye bağlanmak için hata

Azure veri Gezgini'nde bir kümeye bağlanmak için değilseniz, aşağıdaki adımları izleyin.

1. Bağlantı dizesinin doğru olduğundan emin olun. Biçiminde olmalıdır: `https://<ClusterName>.<Region>.kusto.windows.net`, aşağıdaki örnek gibi: `https://docscluster.westus.kusto.windows.net`.

1. Yeterli izinlere sahip olun. Aksi takdirde, yanıtı alırsınız *yetkisiz*.

    İzinler hakkında daha fazla bilgi için bkz. [veritabanı izinlerini yönetmek](manage-database-permissions.md). Gerekli, bunlar için uygun rolü ekleyebilmeniz için küme yöneticinize ile çalışır.

1. Küme silinmiş taşınmadığından doğrulayın: aboneliğinizdeki Etkinlik günlüğünü gözden geçirin.

1. Denetleme [Azure hizmet durumu Panosu](https://azure.microsoft.com/status/>). Azure veri Gezgini'nde bir kümeye bağlanmak için burada çalıştığınız bölge durumunu arayın.

    Durum yoksa **iyi** deneyin, sonra durum kümeye bağlanarak (yeşil onay işareti) geliştirir.

1. Hala sorununuzun çözümüyle ilgili yardıma ihtiyacınız varsa, Lütfen bir destek isteği açın [Azure portalında](https://portal.azure.com).