---
title: "Docker ana bilgisayar ile VirtualBox yapılandırma | Microsoft Docs"
description: "Docker makine ve VirtualBox kullanarak varsayılan Docker örneği yapılandırmak için adım adım yönergeler"
services: azure-container-service
documentationcenter: na
author: mlearned
manager: douge
editor: 
ms.assetid: 0b1335a2-7720-42a8-8260-4e06fc00c9f6
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 06/08/2016
ms.author: mlearned
ms.openlocfilehash: e9465afb560a73d74f853c19094b3ee75b8c470c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="configure-a-docker-host-with-virtualbox"></a>Docker ana bilgisayar ile VirtualBox yapılandırma
## <a name="overview"></a>Genel Bakış
Bu makalede, Docker makine ve VirtualBox kullanarak varsayılan Docker örneği yapılandırırken size rehberlik eder. Kullanıyorsanız, [Windows için Docker beta](http://beta.docker.com/), bu yapılandırma gerekli değildir.

## <a name="prerequisites"></a>Ön koşullar
Aşağıdaki araçları yüklü olması gerekir.

* [Docker araç kutusu](https://www.docker.com/products/overview#/docker_toolbox)

## <a name="configuring-the-docker-client-with-windows-powershell"></a>Windows PowerShell ile Docker istemci yapılandırma
Docker istemciyi yapılandırmak için yalnızca Windows PowerShell'i açın ve aşağıdaki adımları gerçekleştirin:

1. Bir varsayılan docker ana bilgisayar örneği oluşturun.
   
    ```PowerShell
    docker-machine create --driver virtualbox default
    ```
2. Varsayılan örnek yapılandırılmış ve çalışıyor olduğunu doğrulayın. (Çalışan'default ' adlı bir örneği görmeniz gerekir.
   
    ```PowerShell
    docker-machine ls 
    ```
   
    ![docker makine ls çıktı][0]
3. Varsayılan geçerli konağı ayarlayın ve kabuğunuzu yapılandırın.
   
    ```PowerShell
    docker-machine env default | Invoke-Expression
    ```
4. Etkin Docker kapsayıcıları görüntüleyin. Liste boş olmalıdır.
   
    ```PowerShell
    docker ps
    ```
   
    ![docker ps çıktı][1]

> [!NOTE]
> Geliştirme makinenizi yeniden başlatmanız her zaman yerel docker ana bilgisayarınız yeniden başlatmanız gerekir.
> Bunu yapmak için bir komut isteminde aşağıdaki komutu sorun: `docker-machine start default`.
> 
> 

[0]: ./media/vs-azure-tools-docker-setup/docker-machine-ls.png
[1]: ./media/vs-azure-tools-docker-setup/docker-ps.png
