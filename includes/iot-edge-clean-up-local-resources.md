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
ms.openlocfilehash: cdb60ffb3ba3c31c336c8b74c46621792c707f74
ms.sourcegitcommit: 17fe5fe119bdd82e011f8235283e599931fa671a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/11/2018
ms.locfileid: "40046444"
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