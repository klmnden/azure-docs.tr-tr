---
title: Windows Azure CLI kullanarak | Microsoft Docs
description: Windows Azure CLI kullanma
services: virtual-machines-windows
documentationcenter: virtual-machines
author: neilpeterson
manager: jeconnoc
editor: tysonn
tags: azure-service-management
ms.assetid: ''
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 02/14/2017
ms.author: nepeters
ms.openlocfilehash: 6eaad8bc4374cf84ee248e6f4b1be20fd4dfd955
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
# <a name="using-the-azure-cli-on-windows"></a>Windows Azure CLI kullanma

Azure komut satırı arabirimi (CLI), bir komut satırı ve oluşturmak ve Azure kaynaklarınızı yönetmek için komut dosyası ortamı sağlar. Azure CLI macOS, Linux ve Windows işletim sistemleri için kullanılabilir. İşletim sistemi belirli sözdiziminin farklı olabilir ancak bu işletim sistemleri arasında CLI komutları, aynıdır.

Bu belgede Azure CLI yüklenebilir ve Windows ve ayrıntıları söz dizimi konuları her çalıştırın, yolları ayrıntılarını verir. Ayrıntılı Azure CLI belgelere bakın, [Azure CLI belgelerine]( https://docs.microsoft.com/cli/azure).

## <a name="windows-subsystem-for-linux"></a>Linux için Windows alt sistem

Windows alt sistemi için Linux (WSL), Windows 10 Anniversary ve sonraki sürümleri bir Ubuntu Linux ortam sağlar. Etkinleştirildiğinde, WSL oluşturmak ve Azure CLI komut dosyaları çalıştırmak için kullanılan bir yerel Bash deneyimi sağlar. WSL yerel bir Bash deneyimi sağladığından, Azure CLI betikleri macOS, Linux ve Windows arasında değişiklik yapmadan paylaşılabilir.

Azure CLI WSL içinde kullanmak için aşağıdaki adımları tamamlayın.

|Görev | Yönergeler |
|---|---|
| WSL etkinleştir | [WSL belge yükleme ](https://msdn.microsoft.com/en-us/commandline/wsl/install_guide) |
| Azure CLI'yı yükleme |[WSL/Ubuntu 14.04 üzerinde CLI'yı yükleme](https://docs.microsoft.com/cli/azure/install-az-cli2#ubuntu)|

## <a name="powershell"></a>PowerShell

Windows Azure CLI yerel olarak çalıştırılabilir. Bu yapılandırmada, Azure CLI paketi Windows işletim sisteminde yüklü ve Powershell'den komutları çalıştırabilirsiniz. Platform özel komut dosyası sözdizimi gerekli değildir ancak bu yapılandırmada, Azure CLI komutları ve komut dosyaları Windows, desteklenen tüm sürümlerinde çalıştırılabilir. Bu nedenle, komut dosyaları mutlaka macOS, Linux ve Windows arasında değişiklik yapmadan paylaşılamaz.

Windows Azure CLI kullanmak için bu yönergeleri kullanarak paketi yükleyin [Windows CLI'yı yükleme](https://docs.microsoft.com/cli/azure/install-az-cli2#windows).

## <a name="docker-image"></a>Docker görüntüsü

Windows için Docker kullanırken, Azure CLI içeren bir Docker görüntüsü başlatılabilir. Bu görüntü dışına Linux tabanlı ve yerel bir Bash deneyimi içerir.  Windows için Docker ve Azure CLI görüntü macOS, Linux ve Windows arasında paylaşılmak üzere komut dosyaları kullanırken. 

Docker Windows için Azure CLI kullanmak için Windows için Docker çalıştığından emin olun ve aşağıdaki komutu çalıştırın.

```bash
docker run -it azuresdk/azure-cli-python:latest bash
```

Tamamlandığında, oturumu diğer bir deyişle başlatmak Bash ile Azure CLI araçlarını önceden yüklü.

## <a name="next-steps"></a>Sonraki Adımlar

[Azure sanal makineleri için CLI örnek](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

[Azure Web uygulamaları için CLI örnekleri](../../app-service/app-service-cli-samples.md)

[Azure SQL CLI örnekleri](../../sql-database/sql-database-cli-samples.md)
