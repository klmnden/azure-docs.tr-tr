---
title: Azure Market sanal makine görüntüsü oluşturmak için teknik Önkoşullar | Microsoft Docs
description: Oluşturma ve bir sanal makine görüntüsü dağıtma satın almak için Azure Market'ten için diğer gereksinimleri öğrenin.
services: marketplace-publishing
documentationcenter: ''
author: HannibalSII
manager: hascipio
editor: ''
ms.assetid: 63c16966-0304-4b17-a715-368a0a5ccb2c
ms.service: marketplace
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: Azure
ms.workload: na
ms.date: 04/29/2016
ms.author: hascipio; v-divte
ms.openlocfilehash: 79fb9869b37e82df3f41a50e4425e7c0cd08c841
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/07/2018
ms.locfileid: "51255277"
---
# <a name="technical-prerequisites-for-creating-a-virtual-machine-image-for-the-azure-marketplace"></a>Azure Market sanal makine görüntüsü oluşturmak için teknik Önkoşullar
İşlem başlamadan önce baştan sona okuyun ve nerede ve neden gerçekleştirilen her adımla anlayın. Mümkün olduğunca, şirketinizin bilgilerini ve diğer verileri hazırlama, gerekli Araçları'nı indirin ve/veya teklif oluşturma işlemi başlamadan önce teknik bileşenler oluşturun. Bu öğeler bu makalede gözden geçirmeden açık olmalıdır.  

## <a name="download-needed-tools--applications"></a>Gerekli araçları ve uygulamaları indirme
Aşağıdaki öğeler işlemine başlamadan önce hazır olmanız gerekir:

* Hedeflediğiniz işletim sistemine bağlı olarak, yükleme [Azure PowerShell cmdlet'lerini](https://www.microsoft.com/web/handlers/webpi.ashx/getinstaller/WindowsAzurePowershellGet.3f.3f.3fnew.appids) veya [Linux komut satırı arabirimi aracı](https://go.microsoft.com/fwlink/?LinkId=253472&clcid=0x409) gelen [Azure indirmeleri](https://azure.microsoft.com/downloads/) sayfası.
* Azure Depolama Gezgini'ni Codeplex'ten yükleyin.
* İndirin ve Azure sertifikası için sertifika Test aracı yükleyin:
  * [http://go.microsoft.com/fwlink/?LinkID=526913](https://go.microsoft.com/fwlink/?LinkID=526913). Sertifika Aracı'nı çalıştırmak için Windows tabanlı bir bilgisayara ihtiyacınız vardır. Windows tabanlı bir bilgisayarda kullanılabilir değilse, Azure'da bir Windows tabanlı sanal makine kullanarak aracı çalıştırabilirsiniz.

## <a name="platforms-supported"></a>Desteklenen platformlar
Windows veya Linux üzerinde Azure tabanlı Vm'leri geliştirebilirsiniz. Bir Azure ile uyumlu sanal sabit disk (VHD) farklı araçları ve adımları hangi işletim sistemine bağlı olarak, kullandığınız oluşturma gibi yayımlama işlemi--öğelerinden bazıları:  

* Linux kullanılıyorsa, "Azure ile uyumlu bir VHD oluşturma (Linux tabanlı)" bölümüne bakın [sanal makine görüntüsü yayımlama Kılavuzu](marketplace-publishing-vm-image-creation.md).
* Windows kullanıyorsanız, "Azure ile uyumlu VHD'nizi (Windows tabanlı) oluşturma" bölümüne bakın [sanal makine görüntüsü yayımlama Kılavuzu](marketplace-publishing-vm-image-creation.md).

> [!NOTE]
> Windows tabanlı bir makine erişmeniz gerekir:
> 
> * Sertifika doğrulama aracını çalıştırın.
> * VHD sertifika gönderim VHD'si paylaşılan erişim imzası URL'sini oluşturun.
> 
> 

## <a name="develop-your-vhd"></a>VHD'nizi geliştirin
Bulutta veya şirket içi Azure VHD'leri geliştirebilirsiniz.

* Bulut tabanlı geliştirme, tüm geliştirme adımlarını uzaktan Azure'da yerleşik bir VHD üzerinde gerçekleştirilen anlamına gelir.
* Şirket içi geliştirme VHD yükleme ve şirket içi altyapıyı kullanarak geliştirme gerektirir. Bu mümkün olsa da bunu önermiyoruz. Windows veya SQL geliştirmeyi şirket içi şirket ilgili lisans anahtarlarını sahip olmanız gerekir. İçeremez veya bir VM oluşturduktan sonra SQL Server'ı yükleyin. Ayrıca, Azure portalından onaylı bir SQL görüntüye teklifinizi temel almanız gerekir. Şirket içi geliştirme karar verirseniz, farklı bir şekilde bulutta geliştirme, bazı adımlar gerçekleştirmeniz gerekir. İlgili bilgiler bulabilirsiniz [bir şirket içi VM görüntüsü oluşturma](marketplace-publishing-vm-image-creation-on-premise.md).

[link-acct-creation]:marketplace-publishing-accounts-creation-registration.md
