---
title: Azure CLI örnekleri - App Service | Microsoft Docs
description: Azure CLI örnekleri - App Service
services: app-service
documentationcenter: app-service
author: syntaxc4
manager: erikre
editor: ggailey777
tags: azure-service-management
ms.assetid: 53e6a15a-370a-48df-8618-c6737e26acec
ms.service: app-service
ms.devlang: na
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: app-service
ms.date: 12/12/2017
ms.author: cfowler
ms.custom: mvc
ms.openlocfilehash: af28f0fc8a75f32678d94523a01f991ab3d27351
ms.sourcegitcommit: 7cd706612a2712e4dd11e8ca8d172e81d561e1db
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/18/2018
ms.locfileid: "53585202"
---
# <a name="cli-samples-for-azure-app-service"></a>Azure App Service için CLI örnekleri

Aşağıdaki tablo, Azure CLI’si kullanılarak oluşturulan bash komut dosyalarına yönelik bağlantılar içerir.

| | |
|-|-|
|**Uygulama oluşturma**||
| [Bir web uygulaması oluşturma ve FTP ile dosyaları dağıtma](./scripts/cli-deploy-ftp.md?toc=%2fcli%2fazure%2ftoc.json)| Bir Azure web uygulaması oluşturur ve bir dosya FTP kullanarak dağıtır. |
| [GitHub’dan web uygulaması oluşturma ve kod dağıtma](./scripts/cli-deploy-github.md?toc=%2fcli%2fazure%2ftoc.json)| Bir Azure web uygulaması oluşturur ve kodu genel bir GitHub deposundan dağıtır. |
| [GitHub’dan sürekli dağıtım ile bir web uygulaması oluşturma](./scripts/cli-continuous-deployment-github.md?toc=%2fcli%2fazure%2ftoc.json)| Size ait bir GitHub deposundan sürekli yayımlamayı ile bir Azure web uygulaması oluşturur. |
| [Yerel Git deposundan web uygulaması oluşturma ve kod dağıtma](./scripts/cli-deploy-local-git.md?toc=%2fcli%2fazure%2ftoc.json) | Bir Azure web uygulaması oluşturur ve yerel Git deposundan kod gönderimi yapılandırır. |
| [Hazırlık ortamında web uygulaması oluşturma ve kod dağıtma](./scripts/cli-deploy-staging-environment.md?toc=%2fcli%2fazure%2ftoc.json) | Kod değişikliklerini hazırlama için bir dağıtım yuvası ile bir Azure web uygulaması oluşturur. |
| [Bir Docker kapsayıcısında bir ASP.NET Core web uygulaması oluşturma](./scripts/cli-linux-docker-aspnetcore.md?toc=%2fcli%2fazure%2ftoc.json)| Linux üzerinde Azure web uygulaması oluşturur ve Docker hub'dan bir Docker görüntüsü yükler. |
|**Uygulama yapılandırma**||
| [Özel bir etki alanını bir web uygulaması ile eşleştirme](./scripts/cli-configure-custom-domain.md?toc=%2fcli%2fazure%2ftoc.json)| Bir Azure web uygulaması oluşturur ve bir özel etki alanı adı için eşleştirir. |
| [Özel bir SSL sertifikasını bir web uygulamasına bağlama](./scripts/cli-configure-ssl-certificate.md?toc=%2fcli%2fazure%2ftoc.json)| Bir Azure web uygulaması oluşturur ve bir özel etki alanı adının SSL sertifikasını bağlar. |
|**Uygulama ölçeklendirme**||
| [Web uygulamasını el ile ölçeklendirme](./scripts/cli-scale-manual.md?toc=%2fcli%2fazure%2ftoc.json) | Bir Azure web uygulaması oluşturur ve 2 örneklerinde ölçeklendirir. |
| [Web uygulaması dünya çapında yüksek kullanılabilirlik mimarisi ile ölçeklendirme](./scripts/cli-scale-high-availability.md?toc=%2fcli%2fazure%2ftoc.json) | İki farklı coğrafi bölgelerde iki Azure web uygulaması oluşturur ve bunları Azure Traffic Manager'ı kullanarak tek bir uç nokta kullanılabilir hale getirir. |
|**Uygulama kaynaklarına bağlanma**||
| [Bir web uygulamasını SQL veritabanına bağlama](./scripts/cli-connect-to-sql.md?toc=%2fcli%2fazure%2ftoc.json)| Bir Azure web uygulaması ve SQL veritabanı oluşturur ve ardından veritabanı bağlantı dizesi için uygulama ayarları ekler. |
| [Bir web uygulamasını bir depolama hesabına bağlama](./scripts/cli-connect-to-storage.md?toc=%2fcli%2fazure%2ftoc.json)| Bir Azure web uygulaması ve depolama hesabı oluşturur ve ardından depolama bağlantı dizesi için uygulama ayarları ekler. |
| [Bir web uygulaması için bir Azure önbelleği için Redis bağlanın.](./scripts/cli-connect-to-redis.md?toc=%2fcli%2fazure%2ftoc.json) | Bir Azure web uygulaması ve bir Azure önbelleği için Redis oluşturur, ardından redis bağlantı ayrıntılarını uygulama ayarları ekler.) |
| [Bir web uygulamasını Cosmos DB'ye bağlama](./scripts/cli-connect-to-documentdb.md?toc=%2fcli%2fazure%2ftoc.json) | Bir Azure web uygulaması ve Cosmos DB oluşturur, ardından bu Cosmos DB bağlantı ayrıntılarını uygulama ayarları ekler. |
|**Yedekleme ve geri yükleme uygulaması**||
| [Bir web uygulamasını yedekleme](./scripts/cli-backup-onetime.md?toc=%2fcli%2fazure%2ftoc.json) | Bir Azure web uygulaması oluşturur ve bunun için tek seferlik bir yedekleme oluşturur. |
| [Bir web uygulaması için zamanlanmış yedekleme oluşturma](./scripts/cli-backup-scheduled.md?toc=%2fcli%2fazure%2ftoc.json) | Bir Azure web uygulaması oluşturur ve için zamanlanmış bir yedekleme oluşturur. |
| [Bir web uygulamasını yedekten geri yükler.](./scripts/cli-backup-restore.md?toc=%2fcli%2fazure%2ftoc.json) | Bir Azure web uygulamasını yedekten geri yükler. |
|**Uygulamayı İzle**||
| [Web sunucusu günlükleri ile bir web uygulamasını izleme](./scripts/cli-monitor.md?toc=%2fcli%2fazure%2ftoc.json) | Bir Azure web uygulaması oluşturur, bunun için günlük kaydını etkinleştirir ve günlüklerini yerel makinenize indirir. |
| | |