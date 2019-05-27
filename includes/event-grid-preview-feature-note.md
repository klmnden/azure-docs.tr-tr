---
title: include dosyası
description: include dosyası
services: event-grid
author: tfitzmac
ms.service: event-grid
ms.topic: include
ms.date: 11/06/2018
ms.author: tomfitz
ms.custom: include file
ms.openlocfilehash: 85a1579e32b4c216f234f77c76316bedeaea77b0
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/23/2019
ms.locfileid: "66119558"
---
Bu özellik önizlemede. Bunu kullanmak için bir önizleme uzantısı veya modül yüklemeniz gerekir.

### <a name="install-extension-for-azure-cli"></a>Uzantısı için Azure CLI'yı yükleme

Azure CLI için gereksinim duyduğunuz [Event Grid uzantısı](/cli/azure/azure-cli-extensions-list).

İçinde [CloudShell](/azure/cloud-shell/quickstart):

* Uzantıyı daha önce yüklediyseniz bu güncelleştirme `az extension update -n eventgrid`
* Uzantıyı daha önce yüklemediyseniz yükleyin `az extension add -n eventgrid`

Yerel bir yüklemesi için:

1. Azure CLI'yi yerel olarak kaldırın.
1. Yükleme [en son sürümü](/cli/azure/install-azure-cli) Azure CLI'ın.
1. Komut penceresini başlatın.
1. Uzantı'nın önceki sürümlerini kaldırma `az extension remove -n eventgrid`
1. Uzantıyı yükleme `az extension add -n eventgrid`

### <a name="install-module-for-powershell"></a>İçin PowerShell modülünü yükleme

PowerShell için gereksinim duyduğunuz [AzureRM.EventGrid Modülü](https://www.powershellgallery.com/packages/AzureRM.EventGrid/0.4.1-preview).

İçinde [CloudShell](/azure/cloud-shell/quickstart-powershell):

* Modülünü yükleme `Install-Module -Name AzureRM.EventGrid -AllowPrerelease -Force -Repository PSGallery`

Yerel bir yüklemesi için:

1. PowerShell konsolunu yönetici olarak açın.
1. Modülünü yükleme `Install-Module -Name AzureRM.EventGrid -AllowPrerelease -Force -Repository PSGallery`

Varsa `-AllowPrerelease` parametresi kullanılamaz, aşağıdaki adımları kullanın:

1. `Install-Module PowerShellGet -Force` öğesini çalıştırın
1. `Update-Module PowerShellGet` öğesini çalıştırın
1. PowerShell konsolunu kapatın
1. PowerShell'i yönetici olarak yeniden başlatın.
1. Modülünü yükleme `Install-Module -Name AzureRM.EventGrid -AllowPrerelease -Force -Repository PSGallery`
