---
title: include dosyası
description: include dosyası
services: machine-learning
author: sdgilley
ms.service: machine-learning
ms.author: sgilley
manager: cgronlund
ms.custom: include file
ms.topic: include
ms.date: 07/27/2018
ms.openlocfilehash: 12f0f17a7c25696d2c8ff5b427f4b103617e5678
ms.sourcegitcommit: 707bb4016e365723bc4ce59f32f3713edd387b39
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/19/2018
ms.locfileid: "49431831"
---
Bir komut istemi veya kabuk penceresinde, numpy ve cython ile conda paket yöneticisi ortamını oluşturup etkinleştirin. Bu örnekte Python 3.6 kullanılmıştır.

  + Windows'da:
       ```sh 
       conda create -n myenv Python=3.6 cython numpy
       conda activate myenv
       ```

  + Linux veya MacOS üzerinde:
       ```sh 
       conda create -n myenv Python=3.6 cython numpy
       source activate myenv
       ```

SDK’yı yükleyin.
   ```sh 
   pip install azureml-sdk
   ```
