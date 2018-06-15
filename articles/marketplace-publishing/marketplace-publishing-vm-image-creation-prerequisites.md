---
title: Bir sanal makine görüntüsü için Azure Marketi oluşturma teknik önkoşulları | Microsoft Docs
description: Oluşturma ve bir sanal makine görüntüsü Azure satın almak için Marketinde başkalarının dağıtma gereksinimlerini anlayın.
services: marketplace-publishing
documentationcenter: ''
author: msmbaldwin
manager: mbaldwin
editor: ''
ms.assetid: 63c16966-0304-4b17-a715-368a0a5ccb2c
ms.service: marketplace
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: Azure
ms.workload: na
ms.date: 04/29/2016
ms.author: mbaldwin
ms.openlocfilehash: cf1f061c28dd0c106823d34ad39aac5e577c8b41
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/16/2018
ms.locfileid: "29936664"
---
# <a name="technical-prerequisites-for-creating-a-virtual-machine-image-for-the-azure-marketplace"></a>Bir sanal makine görüntüsü için Azure Marketi oluşturma teknik önkoşulları
İşlemi başlamadan önce baştan sona okuyun ve nerede ve neden her adım gerçekleştirilir anlayın. Mümkün olduğunca, şirket bilgilerinizi ve diğer verileri hazırlama, gerekli Araçları'nı indirmek ve/veya teknik bileşenleri teklifi oluşturma işlemi başlamadan önce oluşturun. Bu öğeler bu makaleyi gözden açık olmalıdır.  

## <a name="download-needed-tools--applications"></a>Gerekli Araçlar & uygulamaları yükle
Aşağıdaki öğeler işlemine başlamadan önce hazır olmalıdır:

* Hangi işletim sistemine bağlı olarak, hedeflediğiniz, yükleme [Azure PowerShell cmdlet'lerini](https://www.microsoft.com/web/handlers/webpi.ashx/getinstaller/WindowsAzurePowershellGet.3f.3f.3fnew.appids) veya [Linux komut satırı arabirimi aracı](https://go.microsoft.com/fwlink/?LinkId=253472&clcid=0x409) gelen [Azure indirmeleri](https://azure.microsoft.com/downloads/) sayfası.
* Azure Storage Gezgini Codeplex'ten yükleyin.
* Karşıdan yükle ve Azure sertifikalı için sertifika Test aracı yükleyin:
  * [http://go.microsoft.com/fwlink/?LinkID=526913](http://go.microsoft.com/fwlink/?LinkID=526913). Sertifika aracını çalıştırmak için Windows tabanlı bir bilgisayar gerekir. Windows tabanlı bir bilgisayarda kullanılabilir değilse, aracı Windows tabanlı bir VM ile Azure kullanarak çalıştırabilirsiniz.

## <a name="platforms-supported"></a>Desteklenen platformlar
Windows veya Linux Azure tabanlı VM'ler geliştirebilirsiniz. Bir Azure ile uyumlu sanal sabit disk (VHD)--farklı araçlarını kullanın ve hangi işletim sistemine bağlı olarak, kullandığınız adımları oluşturma gibi yayımlama işlemi--bazı öğeleri:  

* Linux kullanıyorsanız, "(Linux tabanlı) bir Azure uyumlu VHD oluşturma" bölümüne bakın [sanal makine görüntüsü yayımlama Kılavuzu](marketplace-publishing-vm-image-creation.md).
* Windows kullanıyorsanız, "(Windows tabanlı) bir Azure uyumlu VHD oluşturma" bölümüne bakın [sanal makine görüntüsü yayımlama Kılavuzu](marketplace-publishing-vm-image-creation.md).

> [!NOTE]
> Windows tabanlı bir makineye erişmeniz gerekir:
> 
> * Sertifika doğrulama aracını çalıştırın.
> * VHD sertifika göndermelerini VHD paylaşılan erişim imzası URL'SİNİN oluşturun.
> 
> 

## <a name="develop-your-vhd"></a>VHD geliştirin
Bulutta veya şirket içi Azure VHD'ler geliştirme yapabilirsiniz:

* Bulut tabanlı geliştirme tüm geliştirme adımları uzaktan Azure'da yerleşik bir VHD üzerinde gerçekleştirilen anlamına gelir.
* Şirket içi geliştirme VHD indiriliyor ve şirket içi altyapıyı kullanarak geliştirme gerektirir. Bu mümkün olsa da, bunu önermiyoruz. Windows veya SQL için geliştirme şirket içi ilgili lisans anahtarları olmasını gerektirir. Dahil etmek veya VM oluşturduktan sonra SQL Server yükleyin. Ayrıca, görüntüdeki onaylanmış SQL Azure portalından teklifiniz temel almanız gerekir. Şirket içi geliştirmek karar verirseniz, farklı bir şekilde bulutta geliştirme varsa bazı adımları uygulamanız gerekir. İlgili bilgiler bulabilirsiniz [şirket içi VM görüntü oluşturma](marketplace-publishing-vm-image-creation-on-premise.md).

[link-acct-creation]:marketplace-publishing-accounts-creation-registration.md
