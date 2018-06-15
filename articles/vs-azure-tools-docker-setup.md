---
title: Docker ana bilgisayar ile VirtualBox yapılandırma | Microsoft Docs
description: Docker makine ve VirtualBox kullanarak varsayılan Docker örneği yapılandırmak için adım adım yönergeler
services: azure-container-service
documentationcenter: na
author: mlearned
manager: douge
editor: ''
ms.assetid: 0b1335a2-7720-42a8-8260-4e06fc00c9f6
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 06/08/2016
ms.author: mlearned
ms.openlocfilehash: 11e238fa901a164df1dfd896e38df828601e650b
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/23/2018
ms.locfileid: "30190407"
---
# <a name="configure-a-docker-host-with-virtualbox"></a>Docker ana bilgisayar ile VirtualBox yapılandırma
## <a name="overview"></a>Genel Bakış
Bu makalede, Docker makine ve VirtualBox kullanarak varsayılan Docker örneği yapılandırırken size rehberlik eder. Kullanıyorsanız, [Windows için Docker beta](http://beta.docker.com/), bu yapılandırma gerekli değildir.

## <a name="prerequisites"></a>Önkoşullar
Aşağıdaki araçları yüklü olması gerekir.

* [Docker Toolbox](https://github.com/docker/toolbox/releases)

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
