---
title: Azure PowerShell örnekleri - App Service | Microsoft Docs
description: Azure PowerShell örnekleri - App Service
services: app-service
documentationcenter: app-service
author: syntaxc4
manager: erikre
editor: ggailey777
tags: azure-service-management
ms.assetid: b48d1137-8c04-46e0-b430-101e07d7e470
ms.service: app-service
ms.devlang: na
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: app-service
ms.date: 03/08/2017
ms.author: cfowler
ms.custom: mvc
ms.openlocfilehash: 211284f4eff112b9aebec7fa1a031f292a4a92f4
ms.sourcegitcommit: beb4fa5b36e1529408829603f3844e433bea46fe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/22/2018
ms.locfileid: "52290748"
---
# <a name="azure-powershell-samples"></a>Azure PowerShell Örnekleri

Aşağıdaki tablo, Azure PowerShell kullanılarak derlenen PowerShell betiklerinin bağlantılarını içerir.

| | |
|-|-|
|**Uygulama oluşturma**||
| [Github'dan dağıtım ile bir uygulama oluşturun](./scripts/app-service-powershell-deploy-github.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Kodu Github'dan çeken bir Azure web uygulaması oluşturur. |
| [Github'dan sürekli dağıtım ile bir uygulama oluşturma](./scripts/app-service-powershell-continuous-deployment-github.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Sürekli olarak kodunu github'dan dağıtır bir Azure web uygulaması oluşturur. |
| [Bir uygulama oluşturun ve FTP ile kod dağıtma](./scripts/app-service-powershell-deploy-ftp.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Bir Azure web app ve karşıya yükleme dosyaları FTP kullanarak yerel bir dizinden oluşturur. |
| [Uygulama oluşturma ve yerel Git deposundan kod dağıtma](./scripts/app-service-powershell-deploy-local-git.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Bir Azure web uygulaması oluşturur ve yerel Git deposundan kod gönderimi yapılandırır. |
| [Uygulama oluşturma ve hazırlama ortamına kod dağıtma](./scripts/app-service-powershell-deploy-staging-environment.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Kod değişikliklerini hazırlama için bir dağıtım yuvası ile bir Azure web uygulaması oluşturur. |
|**Uygulama yapılandırma**||
| [Bir uygulamaya özel bir etki alanını eşleme](./scripts/app-service-powershell-configure-custom-domain.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Bir Azure web uygulaması oluşturur ve bir özel etki alanı adı için eşleştirir. |
| [Bir uygulamaya özel bir SSL sertifikası bağlama](./scripts/app-service-powershell-configure-ssl-certificate.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Bir Azure web uygulaması oluşturur ve bir özel etki alanı adının SSL sertifikasını bağlar. |
|**Uygulama ölçeklendirme**||
| [Bir uygulamayı el ile ölçeklendirme](./scripts/app-service-powershell-scale-manual.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Bir Azure web uygulaması oluşturur ve 2 örneklerinde ölçeklendirir. |
| [Bir uygulamayı dünya çapında yüksek kullanılabilirlik mimarisi ile ölçeklendirme](./scripts/app-service-powershell-scale-high-availability.md?toc=%2fpowershell%2fmodule%2ftoc.json) | İki farklı coğrafi bölgelerde iki Azure web uygulaması oluşturur ve bunları Azure Traffic Manager'ı kullanarak tek bir uç nokta kullanılabilir hale getirir. |
|**Uygulama kaynaklarına bağlanma**||
| [Bir SQL veritabanı'na bir uygulamaya Bağlan](./scripts/app-service-powershell-connect-to-sql.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Bir Azure web uygulaması ve SQL veritabanı oluşturur ve ardından veritabanı bağlantı dizesi için uygulama ayarları ekler. |
| [Bir depolama hesabına bir uygulamaya Bağlan](./scripts/app-service-powershell-connect-to-storage.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Bir Azure web uygulaması ve depolama hesabı oluşturur ve ardından depolama bağlantı dizesi için uygulama ayarları ekler. |
|**Yedekleme ve geri yükleme uygulaması**||
| [Uygulama yedekleme](./scripts/app-service-powershell-backup-onetime.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Bir Azure web uygulaması oluşturur ve bunun için tek seferlik bir yedekleme oluşturur. |
| [Bir uygulama için zamanlanmış yedekleme oluşturma](./scripts/app-service-powershell-backup-scheduled.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Bir Azure web uygulaması oluşturur ve için zamanlanmış bir yedekleme oluşturur. |
| [Bir uygulama için bir yedek silme](./scripts/app-service-powershell-backup-delete.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Bir uygulama için var olan bir yedekleme siler. |
| [Uygulamayı yedekten geri yükleme](./scripts/app-service-powershell-backup-restore.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Bir uygulamayı önceden tamamlanmış bir yedekten geri yükler. |
| [Abonelikler arasında bir yedeklemeyi geri yükleme](./scripts/app-service-powershell-backup-restore-diff-sub.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Bir web uygulaması, başka bir abonelikte bir yedekten geri yükler. |
|**Uygulamayı İzle**||
| [Web sunucusu günlükleri ile bir uygulamayı izleme](./scripts/app-service-powershell-monitor.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Bir Azure web uygulaması oluşturur, bunun için günlük kaydını etkinleştirir ve günlüklerini yerel makinenize indirir. |
| | |
