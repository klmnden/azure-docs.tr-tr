---
title: İşlem, Ağ ve Depolama sağlayıcıları | Microsoft Docs
description: Azure Resource Manager dağıtım modelinde Windows uygulamaları için İşlem, Ağ ve Depolama Kaynağı Sağlayıcılarına (CRP, NRP ve SRP) kavramsal genel bakış.
services: virtual-machines-windows
documentationcenter: ''
author: tfitzmac
manager: timlt
editor: tysonn
tags: azure-resource-manager,azure-service-management

ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 08/19/2015
ms.author: tomfitz

---
# Azure Resource Manager dağıtım modeli altında, Windows uygulamaları için Azure İşlem, Ağ ve Depolama Sağlayıcıları
Azure Resource Manager dağıtım modeline işlem, ağ ve depolama özellikleri eklenmesi, IaaS’da çalışan karmaşık uygulamaların dağıtımını ve yönetilmesini temelden basit hale getirecektir. Birçok uygulama, Sanal Ağ, Depolama hesabı, Sanal Makine ve Ağ Arabirimi dahil kaynakların birlikte kullanılmasını gerektirmektedir. Azure Resource Manager dağıtım modeli, tüm bu kaynakları tek bir uygulama olarak birlikte dağıtmak ve yönetmek amacıyla bir JSON şablonu oluşturabilme olanağı sunar.

[!INCLUDE [virtual-machines-common-compare-deployment-models](../../includes/virtual-machines-common-compare-deployment-models.md)]

<!--HONumber=Sep16_HO3-->


