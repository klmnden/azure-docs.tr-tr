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
ms.openlocfilehash: fe5649951b1b19ce52c13648f897f4a83e1f761b
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60835405"
---
# <a name="cli-samples-for-azure-app-service"></a>Azure App Service için CLI örnekleri

Aşağıdaki tablo, Azure CLI’si kullanılarak oluşturulan bash komut dosyalarına yönelik bağlantılar içerir.

| | |
|-|-|
|**Uygulama oluşturma**||
| [Bir uygulama oluşturun ve dosyalarını FTP ile dağıtma](./scripts/cli-deploy-ftp.md?toc=%2fcli%2fazure%2ftoc.json)| Bir App Service uygulaması oluşturur ve bir dosya FTP kullanarak dağıtır. |
| [Uygulama oluşturma ve github'dan kod dağıtma](./scripts/cli-deploy-github.md?toc=%2fcli%2fazure%2ftoc.json)| Bir App Service uygulaması oluşturur ve kodu genel bir GitHub deposundan dağıtır. |
| [Github'dan sürekli dağıtım ile bir uygulama oluşturma](./scripts/cli-continuous-deployment-github.md?toc=%2fcli%2fazure%2ftoc.json)| Size ait bir GitHub deposundan sürekli yayımlamayı ile bir App Service uygulaması oluşturur. |
| [Uygulama oluşturma ve yerel Git deposundan kod dağıtma](./scripts/cli-deploy-local-git.md?toc=%2fcli%2fazure%2ftoc.json) | Bir App Service uygulaması oluşturur ve yerel Git deposundan kod gönderimi yapılandırır. |
| [Uygulama oluşturma ve hazırlama ortamına kod dağıtma](./scripts/cli-deploy-staging-environment.md?toc=%2fcli%2fazure%2ftoc.json) | Kod değişikliklerini hazırlama için bir dağıtım yuvası ile bir App Service uygulaması oluşturur. |
| [Bir Docker kapsayıcısında ASP.NET Core uygulaması oluşturma](./scripts/cli-linux-docker-aspnetcore.md?toc=%2fcli%2fazure%2ftoc.json)| Linux üzerinde App Service uygulaması oluşturur ve Docker hub'dan bir Docker görüntüsü yükler. |
|**Uygulama yapılandırma**||
| [Bir uygulamaya özel bir etki alanını eşleme](./scripts/cli-configure-custom-domain.md?toc=%2fcli%2fazure%2ftoc.json)| Bir App Service uygulaması oluşturur ve bir özel etki alanı adı için eşleştirir. |
| [Bir uygulamaya özel bir SSL sertifikası bağlama](./scripts/cli-configure-ssl-certificate.md?toc=%2fcli%2fazure%2ftoc.json)| Bir App Service uygulaması oluşturur ve bir özel etki alanı adının SSL sertifikasını bağlar. |
|**Uygulama ölçeklendirme**||
| [Bir uygulamayı el ile ölçeklendirme](./scripts/cli-scale-manual.md?toc=%2fcli%2fazure%2ftoc.json) | Bir App Service uygulaması oluşturur ve 2 örneklerinde ölçeklendirir. |
| [Bir uygulamayı dünya çapında yüksek kullanılabilirlik mimarisi ile ölçeklendirme](./scripts/cli-scale-high-availability.md?toc=%2fcli%2fazure%2ftoc.json) | İki farklı coğrafi bölgelerde iki App Service uygulaması oluşturur ve bunları Azure Traffic Manager'ı kullanarak tek bir uç nokta kullanılabilir hale getirir. |
|**Uygulama kaynaklarına bağlanma**||
| [Bir SQL veritabanı'na bir uygulamaya Bağlan](./scripts/cli-connect-to-sql.md?toc=%2fcli%2fazure%2ftoc.json)| Bir App Service uygulaması ve SQL veritabanı oluşturur ve ardından veritabanı bağlantı dizesi için uygulama ayarları ekler. |
| [Bir depolama hesabına bir uygulamaya Bağlan](./scripts/cli-connect-to-storage.md?toc=%2fcli%2fazure%2ftoc.json)| Bir App Service uygulaması ve depolama hesabı oluşturur ve ardından depolama bağlantı dizesi için uygulama ayarları ekler. |
| [Bir uygulama için bir Azure önbelleği için Redis bağlanın.](./scripts/cli-connect-to-redis.md?toc=%2fcli%2fazure%2ftoc.json) | Bir App Service uygulaması ve bir Azure önbelleği için Redis oluşturur, ardından redis bağlantı ayrıntılarını uygulama ayarları ekler.) |
| [Cosmos DB'ye bir uygulamaya Bağlan](./scripts/cli-connect-to-documentdb.md?toc=%2fcli%2fazure%2ftoc.json) | Bir App Service uygulaması ve Cosmos DB oluşturur, ardından bu Cosmos DB bağlantı ayrıntılarını uygulama ayarları ekler. |
|**Yedekleme ve geri yükleme uygulaması**||
| [Uygulama yedekleme](./scripts/cli-backup-onetime.md?toc=%2fcli%2fazure%2ftoc.json) | Bir App Service uygulaması oluşturur ve bunun için tek seferlik bir yedekleme oluşturur. |
| [Bir uygulama için zamanlanmış yedekleme oluşturma](./scripts/cli-backup-scheduled.md?toc=%2fcli%2fazure%2ftoc.json) | Bir App Service uygulaması oluşturur ve için zamanlanmış bir yedekleme oluşturur. |
| [Bir uygulama bir yedekten geri yükler.](./scripts/cli-backup-restore.md?toc=%2fcli%2fazure%2ftoc.json) | Bir App Service uygulamasını yedekten geri yükler. |
|**Uygulamayı İzle**||
| [Web sunucusu günlükleri ile bir uygulamayı izleme](./scripts/cli-monitor.md?toc=%2fcli%2fazure%2ftoc.json) | Bir App Service uygulaması oluşturur, bunun için günlük kaydını etkinleştirir ve günlüklerini yerel makinenize indirir. |
| | |