---
title: include dosyası
description: include dosyası
services: iot-edge
author: kgremban
ms.service: iot-edge
ms.topic: include
ms.date: 08/10/2018
ms.author: kgremban
ms.custom: include file
ms.openlocfilehash: 2b61cc8c5c448c28e96b06afa3556688a82567ed
ms.sourcegitcommit: b7a44709a0f82974578126f25abee27399f0887f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2019
ms.locfileid: "67202691"
---
### <a name="delete-local-resources"></a>Yerel kaynakları silme

Bir cihazdan IoT Edge çalışma zamanını ve ilgili kaynakları kaldırmak isterseniz cihazınızın işletim sistemine uygun komutları kullanabilirsiniz. 

#### <a name="windows"></a>Windows

IoT Edge çalışma zamanını kaldırın.

   ```powershell
   . {Invoke-WebRequest -useb aka.ms/iotedge-win} | Invoke-Expression; `
   Uninstall-SecurityDaemon
   ```

IoT Edge çalışma zamanı kaldırıldığında, oluşturduğu kapsayıcılar durdurulur, ancak cihazınızda yer almaya devam eder. Tüm kapsayıcıları görüntüleyin.

   ```powershell
   docker ps -a
   ```

Cihazınızda oluşturulan çalışma zamanı kapsayıcılarını silin.

   ```powershell
   docker rm -f edgeHub
   docker rm -f edgeAgent
   ```

Kapsayıcı adlarına bakarak `docker ps` çıkışında listelenen ek kapsayıcıları silin. 

#### <a name="linux"></a>Linux

IoT Edge çalışma zamanını kaldırın.

   ```bash
   sudo apt-get remove --purge iotedge
   ```

IoT Edge çalışma zamanı kaldırıldığında, oluşturduğu kapsayıcılar durdurulur, ancak cihazınızda yer almaya devam eder. Tüm kapsayıcıları görüntüleyin.

   ```bash
   sudo docker ps -a
   ```

Cihazınızda oluşturulan çalışma zamanı kapsayıcılarını silin.

   ```bash
   docker rm -f edgeHub
   docker rm -f edgeAgent
   ```

Kapsayıcı adlarına bakarak `docker ps` çıkışında listelenen ek kapsayıcıları silin. 

Kapsayıcı çalışma zamanını kaldırın.

   ```bash
   sudo apt-get remove --purge moby
   ```