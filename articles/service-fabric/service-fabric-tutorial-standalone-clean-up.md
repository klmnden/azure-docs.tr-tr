---
title: 'Öğretici: Service Fabric tek başına kümesini temizleme - Azure Service Fabric | Microsoft Docs'
description: Bu öğreticide, tek başına kümenizi temizleme hakkında bilgi edinin
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
ms.openlocfilehash: 9127ad1fe148f85779913454adf6578a9a8a1055
ms.sourcegitcommit: a52d48238d00161be5d1ed5d04132db4de43e076
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67274098"
---
# <a name="tutorial-clean-up-your-standalone-cluster"></a>Öğretici: Tek başına kümenizi Temizle

Service Fabric tek başına kümeleri, kendi ortamınızı seçme ve Service Fabric’in benimsediği "her işletim sistemi, her bulut" yaklaşımının bir parçası olarak bir küme oluşturma seçeneği sunar. Bu öğretici serisine AWS ya da Azure üzerinde barındırılan tek başına küme oluşturma ve bir uygulama içine yükleyin.

Bu öğretici, bir serinin dördüncü bölümüdür. Öğreticinin bu bölümünde AWS veya Service Fabric kümenizi barındırmak için oluşturduğunuz Azure kaynaklarını temizlemek gösterilmektedir.

Serinin dördüncü kısmında öğrenecekleriniz:

> [!div class="checklist"]
> * Service Fabric kümesini temizleme
> * AWS veya Azure kaynakları temizlemek

## <a name="clean-up-service-fabric-cluster"></a>Service Fabric kümesini temizleme

1. Service Fabric için kullanılan VM RDP yüklü.
2. PowerShell’i açın.
3. Dizini, ikinci öğreticide ayıklanan klasöre geçirin.
4. Service Fabric kümesini kaldırmak için aşağıdaki komutu çalıştırın:

  ```powershell
  .\RemoveServiceFabricCluster.ps1 -ClusterConfigFilePath .\ClusterConfig.Unsecure.MultiMachine.json
  ```

5. Girin `Y` istendiğinde başarılı olduysa, çıkış içinde yerine kendi IP adresleriyle aşağıdaki gibi görünür:

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

1. AWS hesabınızda oturum açın.
2. EC2 Konsolu'na gidin.
3. Öğreticinin birinci bölümünde oluşturduğunuz üç düğümü seçin.
4. Tıklayarak **eylemleri** > **örnek durumu** > **sonlandırmak**.

## <a name="clean-up-azure-resources"></a>Azure kaynakları temizleme

1. Azure Portal’da oturum açın.
2. Git **sanal makineler** bölümü.
3. Öğreticinin birinci oluşturduğunuz üç düğüm için onay kutularını seçin.
4. Tıklayarak **Sil**.

## <a name="next-steps"></a>Sonraki adımlar

Serinin dördüncü bölümünde, önceki adımlarda oluşturulan kaynaklarınızı nasıl temizleyeceğinizi öğrendiniz.

> [!div class="checklist"]
> * Kaynaklarınızı temizleme

> [!div class="nextstepaction"]
> [Başa dön](service-fabric-tutorial-standalone-create-infrastructure.md)
