---
title: 'Öğretici: Service Fabric tek başına kümesini temizleme - Azure Service Fabric | Microsoft Docs'
description: Bu öğreticide, tek başına kümenizi temizleme hakkında bilgi edineceksiniz
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: chackdan
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: tutorial
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 05/11/2018
ms.author: dekapur
ms.custom: mvc
ms.openlocfilehash: 67334a0e8ae3e6dcca86830cd088e6e446331aee
ms.sourcegitcommit: 009334a842d08b1c83ee183b5830092e067f4374
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66306709"
---
# <a name="tutorial-clean-up-your-standalone-cluster"></a>Öğretici: Tek başına kümenizi Temizle

Service Fabric tek başına kümeleri, kendi ortamınızı seçme ve Service Fabric’in benimsediği "her işletim sistemi, her bulut" yaklaşımının bir parçası olarak bir küme oluşturma seçeneği sunar. Bu öğretici serisinde, AWS üzerinde barındırılan bir tek başına küme oluşturacak ve içine bir uygulama yükleyeceksiniz.

Bu öğretici, bir serinin dördüncü bölümüdür. Öğreticinin bu bölümünde, Service Fabric kümenizi barındırmak için oluşturduğunuz AWS kaynaklarını temizleme işlemi gösterilmektedir.

Serinin dördüncü kısmında öğrenecekleriniz:

> [!div class="checklist"]
> * Service Fabric kümesini temizleme
> * AWS kaynaklarınızı temizleme

## <a name="clean-up-service-fabric-cluster"></a>Service Fabric kümesini temizleme

* Service Fabric yüklemek için kullandığınız EC2 örneğine RDP uygulama
* PowerShell’i açın
* Dizini, ikinci öğreticide ayıklanan klasöre geçirin.
* Service Fabric kümesini kaldırmak için aşağıdaki komutu çalıştırın:

```powershell
.\RemoveServiceFabricCluster.ps1 -ClusterConfigFilePath .\ClusterConfig.Unsecure.MultiMachine.json
```

* Sorulduğunda `Y` girin. Başarılı olursa, çıktınız kendi IP adresinizi içerecek şekilde şuna benzer görünür:

```powershell
Best Practices Analyzer completed successfully.
Removing configuration from machine 172.31.21.141
Removing configuration from machine 172.31.27.1
Removing configuration from machine 172.31.20.163
Configuration removed from machine 172.31.21.141
Configuration removed from machine 172.31.27.1
Configuration removed from machine 172.31.20.163
The cluster is successfully removed.
```

## <a name="clean-up-aws-resources"></a>AWS kaynaklarını temizleme

* AWS hesabınızda oturum açın
* EC2 Konsolu'na gidin.
* Öğreticinin birinci bölümünde oluşturduğunuz üç düğümü seçin.
* **Eylemler** > **Örnek Durumu** > **Sonlandır**’a tıklayın

## <a name="next-steps"></a>Sonraki adımlar

Serinin dördüncü bölümünde, önceki adımlarda oluşturulan kaynaklarınızı nasıl temizleyeceğinizi öğrendiniz.

> [!div class="checklist"]
> * Kaynaklarınızı temizleme

> [!div class="nextstepaction"]
> [Başa dön](service-fabric-tutorial-standalone-create-infrastructure.md)