---
title: Azure IOT Edge üzerinde Windows yükleme | Microsoft Docs
description: Windows 10, Windows Server ve Windows IOT Core Azure IOT Edge yükleme yönergeleri
author: kgremban
manager: philmea
ms.reviewer: veyalla
ms.service: iot-edge
services: iot-edge
ms.topic: conceptual
ms.date: 05/06/2019
ms.author: kgremban
ms.custom: seodec18
ms.openlocfilehash: 8907ae61fb03b417a74eb32e1fd09aece75d5e2c
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/23/2019
ms.locfileid: "66151713"
---
# <a name="install-the-azure-iot-edge-runtime-on-windows"></a>Windows üzerinde Azure IOT Edge çalışma zamanı yükleme

Azure IOT Edge çalışma zamanı, ne bir cihaz ile IOT Edge cihazı kapatır ' dir. Çalışma zamanı, cihaz olarak endüstriyel sunucusu olarak büyük veya küçük bir Raspberry Pi üzerinde dağıtılabilir. Bir cihaz IOT Edge çalışma zamanı ile yapılandırıldıktan sonra iş mantığı buluttan dağıttıktan başlayabilirsiniz. 

IOT Edge çalışma zamanı hakkında daha fazla bilgi için bkz: [Azure IOT Edge çalışma zamanı ve mimarisini anlama](iot-edge-runtime.md).

Bu makalede Azure IOT Edge çalışma zamanı, Windows x64 (AMD/Intel) yüklemek için adımları listelenmektedir Windows kapsayıcıları kullanarak sistemi.

> [!NOTE]
> Bilinen bir Windows işletim sistemi sorun uyku ve güç durumlarını, IOT Edge modülleri (işlem yalıtılmış Windows Nano sunucu kapsayıcıları) çalıştırırken, hazırda bekleme moduna geçiş engeller. Bu sorun, cihazın pil ömrü etkiler.
>
> Geçici bir çözüm olarak komutunu `Stop-Service iotedge` bu güç durumlarını kullanmadan önce çalışan tüm IOT Edge modülleri durdurmak için. 

<!--
> [!NOTE]
> Using Linux containers on Windows systems is not a recommended or supported production configuration for Azure IoT Edge. However, it can be used for development and testing purposes.
-->

Windows sistemlerinde Linux kapsayıcıları kullanarak Azure IOT Edge için önerilen veya desteklenen üretim yapılandırma değildir. Ancak, geliştirme ve test amacıyla kullanılabilir. Daha fazla bilgi için bkz. [Linux kapsayıcıları çalıştırmak için Windows IOT Edge kullanımı](how-to-install-iot-edge-windows-with-linux.md).

IOT Edge en son sürümüne nelerin dahil olduğunu hakkında daha fazla bilgi için bkz: [Azure IOT Edge serbest](https://github.com/Azure/azure-iotedge/releases).

## <a name="prerequisites"></a>Önkoşullar

Bu bölümde, Windows cihazınız, IOT Edge desteği olup olmadığını gözden geçirin ve yüklemeden önce kapsayıcı altyapısı için hazırlamak için kullanın. 

### <a name="supported-windows-versions"></a>Desteklenen Windows sürümleri

Geliştirme ve test senaryoları için Windows kapsayıcıları ile Azure IOT Edge üzerinde Windows 10 kapsayıcıları özelliğini destekleyen Windows Server 2019 (derleme 17763) veya herhangi bir sürümü yüklenebilir. Hangi işletim sistemleri şu anda desteklenen üretim senaryoları için daha fazla bilgi için bkz: [Azure IOT Edge desteklenen sistemler](support.md#operating-systems). 

### <a name="prepare-for-a-container-engine"></a>Bir kapsayıcı altyapısı için hazırlama 

Azure IOT Edge dayanır bir [OCI uyumlu](https://www.opencontainers.org/) container altyapısı. Üretim senaryoları için Windows Cihazınızda Windows kapsayıcılarını çalıştırmaya yönelik yükleme betiğini dahil Moby altyapısını kullanır. 

## <a name="install-iot-edge-on-a-new-device"></a>IOT Edge yeni bir cihaza yükleme

>[!NOTE]
>Azure IOT Edge yazılım paketlerini (lisans dizininde) paketleri bulunan lisans koşullarına tabidir. Paket kullanarak önce lisans koşullarını okuyun. Bu koşulları kabul etmeniz, yükleme ve kullanım paket oluşturur. Lisans koşullarını kabul etmiyorsanız, paket kullanmayın.

Bir PowerShell Betiği indirir ve Azure IOT Edge güvenlik daemon'ı yükler. Güvenlik daemon daha sonra diğer modüllerin uzak dağıtımları sağlayan IOT Edge Aracısı ilk iki çalışma zamanı modüllerinin başlatır. 

IOT Edge çalışma zamanı, bir cihaz üzerinde ilk kez yüklediğinizde, bir IOT hub'ından kimliğe sahip cihazı sağlamak gerekir. Tek bir IOT Edge cihazı IOT Hub tarafından sağlanan cihaz bağlantı dizesini kullanarak el ile sağlanabilir. Veya, cihaz sağlama hizmeti otomatik olarak ayarlamak için birçok cihaz olduğunda kullanışlı olan cihazları sağlamak için kullanabilirsiniz. Sağlama seçiminize bağlı olarak, uygun yükleme komut dosyasını seçin. 

Aşağıdaki bölümlerde yaygın kullanım örnekleri ve IOT Edge yükleme betik parametreleri için yeni bir cihazda açıklanmaktadır. 

### <a name="option-1-install-and-manually-provision"></a>1. seçenek: Yükleme ve el ile sağlama

İlk seçenek, sağladığınız bir **cihaz bağlantı dizesini** cihaz sağlamak için IOT Hub tarafından üretilen. 

Bu örnek, Windows kapsayıcıları ile el ile yükleme göstermektedir:

1. Henüz yapmadıysanız, yeni bir IOT Edge cihazı kaydetme ve alma **cihaz bağlantı dizesini**. Bu bölümde daha sonra kullanmak üzere bağlantı dizesini kopyalayın. Bu adımı aşağıdaki araçları kullanarak tamamlayabilirsiniz:

   * [Azure portal](how-to-register-device-portal.md)
   * [Azure CLI](how-to-register-device-cli.md)
   * [Visual Studio Code](how-to-register-device-vscode.md)

2. PowerShell'i yönetici olarak çalıştırın.

   >[!NOTE]
   >IOT Edge, PowerShell (x86) yüklemek için bir AMD64 PowerShell oturumu kullanın. Kullanmakta olduğunuz hangi oturum türü emin değilseniz, aşağıdaki komutu çalıştırın:
   >
   >```powershell
   >(Get-Process -Id $PID).StartInfo.EnvironmentVariables["PROCESSOR_ARCHITECTURE"]
   >```

3. **Dağıt IoTEdge** komutu, Windows makinenizde desteklenen bir sürümü, kapsayıcıları özelliğini açar ve sonra moby çalışma zamanı ve IOT Edge çalışma zamanı yükler denetler. Komutu Windows kapsayıcıları için kullanılacak varsayılan olarak kullanır. 

   ```powershell
   . {Invoke-WebRequest -useb aka.ms/iotedge-win} | Invoke-Expression; `
   Deploy-IoTEdge
   ```

4. Bu noktada, IOT Core cihazları otomatik olarak yeniden başlatılabilir. Diğer Windows 10 veya Windows Server cihazları yeniden başlatmanızı isteyebilir. Bu durumda, cihazınız artık yeniden başlatın. PowerShell'i yönetici olarak yeniden çalıştırın, cihaz hazır olduktan sonra.

5. **Başlatma IoTEdge** komut, IOT Edge çalışma zamanı makinenizde yapılandırır. Komutu ile Windows kapsayıcıları el ile sağlama için varsayılan olarak kullanır. 

   ```powershell
   . {Invoke-WebRequest -useb aka.ms/iotedge-win} | Invoke-Expression; `
   Initialize-IoTEdge
   ```

6. İstendiğinde, 1. adımda aldığınız cihaz bağlantı dizesi sağlayın. Cihaz bağlantı dizesini fiziksel cihaz IOT hub cihaz kimliği ile ilişkilendirir. 

   Cihaz bağlantı dizesi aşağıdaki biçimi alır ve tırnak işaretleri içermemesi gerekir: `HostName={IoT hub name}.azure-devices.net;DeviceId={device name};SharedAccessKey={key}`

7. İçindeki adımları kullanın [yüklemenin başarılı olduğunu doğrulamak](#verify-successful-installation) IOT Edge Cihazınızda durumunu denetlemek için. 

Yüklediğinizde ve bir cihazı el ile sağlama yükleme dahil olmak üzere değiştirmek için ek parametreler kullanabilirsiniz:
* Bir ara sunucu üzerinden gitmesi için trafiği
* Yükleyici çevrimdışı bir dizine işaret
* Belirli bir aracı kapsayıcı görüntüsü bildirme ve salt okunur ise bir özel kayıt defteri kimlik bilgilerini sağlayın

Hakkında bilgi edinmek için bu yükleme seçenekleri hakkında daha fazla bilgi için İleri atlayabilirsiniz [tüm yükleme parametrelerini](#all-installation-parameters).

### <a name="option-2-install-and-automatically-provision"></a>2. seçenek: Yükleme ve otomatik olarak sağlama

Bu ikinci seçenek, IOT Hub cihaz sağlama Hizmeti'ni kullanarak cihaz sağlama. Sağlamak **kapsam kimliği** cihaz sağlama hizmeti örneğinden ve **kayıt kimliği** cihazınızdan.

Aşağıdaki örnek, bir otomatik yükleme Windows kapsayıcıları ile gösterir:

1. Bağlantısındaki [oluşturma ve sağlama Windows üzerinde sanal bir TPM IOT Edge cihazı](how-to-auto-provision-simulated-device-windows.md) cihaz sağlama hizmetini ayarlama ve almak için kendi **kapsam kimliği**kendi almakvebirTPMcihazınıbenzetme**Kayıt kimliği**, sonra bireysel kayıt oluşturma. Cihazınızı IOT hub'ına kaydedildiğinde, bu yükleme adımlarla devam edin.  

   >[!TIP]
   >TPM simülatörünü yüklenmesi sırasında açık ve test çalıştıran pencereyi tutun. 

2. PowerShell'i yönetici olarak çalıştırın.

   >[!NOTE]
   >IOT Edge, PowerShell (x86) yüklemek için bir AMD64 PowerShell oturumu kullanın. Kullanmakta olduğunuz hangi oturum türü emin değilseniz, aşağıdaki komutu çalıştırın:
   >
   >```powershell
   >(Get-Process -Id $PID).StartInfo.EnvironmentVariables["PROCESSOR_ARCHITECTURE"]
   >```

3. **Dağıt IoTEdge** komutu, Windows makinenizde desteklenen bir sürümü, kapsayıcıları özelliğini açar ve sonra moby çalışma zamanı ve IOT Edge çalışma zamanı yükler denetler. Komutu Windows kapsayıcıları için kullanılacak varsayılan olarak kullanır. 

   ```powershell
   . {Invoke-WebRequest -useb aka.ms/iotedge-win} | Invoke-Expression; `
   Deploy-IoTEdge
   ```

4. Bu noktada, IOT Core cihazları otomatik olarak yeniden başlatılabilir. Diğer Windows 10 veya Windows Server cihazları yeniden başlatmanızı isteyebilir. Bu durumda, cihazınız artık yeniden başlatın. PowerShell'i yönetici olarak yeniden çalıştırın, cihaz hazır olduktan sonra.

6. **Başlatma IoTEdge** komut, IOT Edge çalışma zamanı makinenizde yapılandırır. Komutu ile Windows kapsayıcıları el ile sağlama için varsayılan olarak kullanır. Kullanma `-Dps` el ile sağlama yerine cihaz sağlama hizmeti kullanmak için bayrak.

   ```powershell
   . {Invoke-WebRequest -useb aka.ms/iotedge-win} | Invoke-Expression; `
   Initialize-IoTEdge -Dps
   ```

7. İstendiğinde, cihaz sağlama hizmeti kapsam kimliği ve ikisi için de 1. adımda aldığınız cihazınızdan kayıt kimliği sağlayın.

8. İçindeki adımları kullanın [yüklemenin başarılı olduğunu doğrulamak](#verify-successful-installation) IOT Edge Cihazınızda durumunu denetlemek için. 

Yüklediğinizde ve bir cihazı el ile sağlama yükleme dahil olmak üzere değiştirmek için ek parametreler kullanabilirsiniz:
* Bir ara sunucu üzerinden gitmesi için trafiği
* Yükleyici çevrimdışı bir dizine işaret
* Belirli bir aracı kapsayıcı görüntüsü bildirme ve salt okunur ise bir özel kayıt defteri kimlik bilgilerini sağlayın

Bu yükleme seçenekleri hakkında daha fazla bilgi için bu makaleyi okumaya devam edin veya hakkında bilgi edinmek için atla [tüm yükleme parametrelerini](#all-installation-parameters).

## <a name="offline-installation"></a>Çevrimdışı yükleme

Yükleme sırasında iki dosya yüklenir: 
* Microsoft Azure IOT Edge cab IOT Edge güvenlik arka plan programı (iotedged), Moby container altyapısı ve Moby CLI içerir.
* Visual C++ yeniden dağıtılabilir paket (VC Çalışma zamanı) MSI

Cihaza biri veya her ikisi önceden bu dosyaları indir ve sonra yükleme betiğini dosyalarını içeren dizin noktası. Yükleyici, dizinin ilk denetler ve yalnızca bulunan olmayan bileşenleri indirir. Tüm dosyaları çevrimdışı varsa, internet bağlantısı ile yükleyebilirsiniz. Bu özellik, bileşenlerin belirli bir sürümünü yüklemek için de kullanabilirsiniz.  

En son IOT Edge yükleme dosyalarıyla birlikte önceki sürümleri, bkz: [Azure IOT Edge serbest](https://github.com/Azure/azure-iotedge/releases).

Çevrimdışı bileşenleri yüklemek için kullanın `-OfflineInstallationPath` Dağıt IoTEdge bir parçası olarak parametresi komut ve dosya dizinine mutlak yolunu belirtin. Örneğin,

```powershell
. {Invoke-WebRequest -useb aka.ms/iotedge-win} | Invoke-Expression; `
Deploy-IoTEdge -OfflineInstallationPath C:\Downloads\iotedgeoffline
```

Bu makalenin sonraki bölümlerinde sunulan güncelleştirme IoTEdge komutu, çevrimdışı yükleme path parametresini de kullanabilirsiniz. 

## <a name="verify-successful-installation"></a>Yüklemenin başarılı olduğunu doğrulamak

IoT Edge hizmetinin durumunu kontrol edin. Olarak listelenmesi gerekir.  

```powershell
Get-Service iotedge
```

Son 5 dakika Hizmeti günlüklerini inceleyin. IOT Edge çalışma zamanı yükleme yalnızca tamamlandıysa çalışan arasındaki zamanı hatalarından listesini görebilirsiniz **Dağıt IoTEdge** ve **başlatma IoTEdge**. Hizmeti, yapılandırılmış önce başlatmaya çalışıyor gibi bu hata beklenir. 

```powershell
. {Invoke-WebRequest -useb aka.ms/iotedge-win} | Invoke-Expression; Get-IoTEdgeLog
```

Çalışan modülleri listeleyin. Çalışan olduğunu gördüğünüz yalnızca modülü yeni bir yüklemeden sonra **edgeAgent**. Çalıştırdıktan sonra [IOT Edge modüllerini dağıtmak](how-to-deploy-modules-portal.md), diğerleri görürsünüz. 

```powershell
iotedge list
```

## <a name="manage-module-containers"></a>Modül kapsayıcıları yönetin

IOT Edge hizmetinin, cihaz üzerinde çalışan bir container altyapısı gerektirir. IOT Edge çalışma zamanı kapsayıcı altyapısı, bir modül bir cihaza dağıttığınızda, bulutta bir kayıt defterinden kapsayıcı görüntüsü çekin için kullanır. IOT Edge hizmetinin modüllerinizi ile etkileşim kurmanızı ve günlüklerini sağlar, ancak bazen kapsayıcı ile etkileşim kurmak için kapsayıcı altyapısı kullanmak isteyebilirsiniz. 

Modül kavramları hakkında daha fazla bilgi için bkz. [anlamak Azure IOT Edge modülleri](iot-edge-modules.md). 

Windows IOT Edge Cihazınızda Windows kapsayıcıları çalıştırıyorsanız, IOT Edge yükleme Moby container altyapısı dahil. Moby altyapısı Docker aynı standartlarını temel ve Docker masaüstü ile aynı makinede paralel olarak çalıştırmak için tasarlanmıştır. Bu nedenle, hedef kapsayıcı Moby altyapısı tarafından yönetilen istiyorsanız Docker yerine bu altyapıyı özel olarak hedef vardır. 

Örneğin, tüm Docker görüntülerini listelemek için aşağıdaki komutu kullanın:

```powershell
docker images
```

Tüm Moby görüntülerini listelemek için aynı komutu Moby altyapısı işaretçiyle değiştirin: 

```powershell
docker -H npipe:////./pipe/iotedge_moby_engine images
```

URI altyapısı yükleme komut çıktısında listelenir veya config.yaml dosyanın kapsayıcı çalışma zamanı ayarları bölümünde bulabilirsiniz. 

![config.yaml moby_runtime URI](./media/how-to-install-iot-edge-windows/moby-runtime-uri.png)

Kapsayıcılar ve Cihazınızda çalışan görüntülerinin ile etkileşim kurmak için kullanabileceğiniz komutlar hakkında daha fazla bilgi için bkz. [Docker komut satırı arabirimlerinden](https://docs.docker.com/engine/reference/commandline/docker/).

## <a name="update-an-existing-installation"></a>Varolan bir yüklemeyi güncelleştirme

Ardından önceden IOT Edge çalışma zamanı önce bir cihazda yüklü ve IOT Hub'ından bir kimlikle sağlanan, çalışma zamanı, cihaz bilgileri yeniden girmek zorunda kalmadan güncelleştirebilirsiniz. 

Daha fazla bilgi için [IOT Edge güvenlik arka plan programı ve çalışma zamanını güncelleştirme](how-to-update-iot-edge.md).

Bu örnekte, var olan bir yapılandırma dosyasını işaret eder ve Windows kapsayıcıları'nı kullanan bir yüklemesini göstermektedir: 

```powershell
. {Invoke-WebRequest -useb aka.ms/iotedge-win} | Invoke-Expression; `
Update-IoTEdge
```

IOT Edge güncelleştirirken, güncelleştirme değiştirmek için ek parametreler kullanabilirsiniz dahil olmak üzere:
* Bir ara sunucu üzerinden gitmesi için trafiği doğrudan veya
* Yükleyici çevrimdışı bir dizine işaret 
* Gerekirse, istem olmadan yeniden başlatılıyor

Bu bilgileri zaten bir önceki yükleme yapılandırma dosyasında ayarlandığından betik parametreleri bir IOT Edge Aracısı kapsayıcı görüntüsüyle bildiremezsiniz. Aracı kapsayıcı görüntüsünü değiştirmek istiyorsanız, bunu config.yaml dosyasında yapın. 

Bunlar hakkında daha fazla bilgi için güncelleştirme seçenekleri, komutunu `Get-Help Update-IoTEdge -full` veya [tüm yükleme parametrelerini](#all-installation-parameters).

## <a name="uninstall-iot-edge"></a>IOT Edge kaldırma

Windows cihazınızın IOT Edge yükleme kaldırmak istiyorsanız, yönetici bir PowerShell penceresinden aşağıdaki komutu kullanın. Bu komut, yapılandırmanızda ve Moby altyapısı verileri ile birlikte IOT Edge çalışma zamanı kaldırır. 

```powershell
Uninstall-IoTEdge
```

Uninstall-IoTEdge komutu Windows IOT Core üzerinde çalışmaz. Windows IOT Core cihazlardan IOT Edge kaldırmak için Windows IOT Core görüntünüzü dağıtmanız gerekir. 

Kaldırma seçenekleri hakkında daha fazla bilgi için komutu kullanın `Get-Help Uninstall-IoTEdge -full`. 

## <a name="all-installation-parameters"></a>Tüm yükleme parametreleri

Önceki bölümlerde yükleme senaryoları ile yükleme komut dosyasını değiştirmek için parametrelerini kullanma örnekleri kullanıma sunuldu. Bu bölümde yüklemek, güncelleştirmek veya IOT Edge kaldırmak için kullanılan ortak parametrelerinin başvuru tabloları sağlar. 

### <a name="deploy-iotedge"></a>IoTEdge dağıtma

Dağıtma-IoTEdge komut indirir ve IOT Edge güvenlik arka plan programı'nı ve bağımlılıklarını dağıtır. Dağıtım komutu, diğerlerinin yanı sıra bu ortak parametreleri kabul eder. Tam liste için komutunu `Get-Help Deploy-IoTEdge -full`.  

| Parametre | Kabul edilen değerler | Açıklamalar |
| --------- | --------------- | -------- |
| **ContainerOs** | **Windows** veya **Linux** | Kapsayıcı işletim sistemi belirtilmezse, Windows varsayılan değerdir.<br><br>Windows kapsayıcıları için IOT Edge yüklemede moby container altyapısı kullanır. Linux kapsayıcıları için bir kapsayıcı altyapısı yüklemeye başlamadan önce yüklemeniz gerekir. |
| **Proxy** | Proxy URL'si | Cihazınız, İnternet'e erişmek için Ara sunucu üzerinden gitmesi gerekiyorsa, bu parametreyi dahil edin. Daha fazla bilgi için [bir proxy sunucu üzerinden iletişim kurmak için IOT Edge cihazı yapılandırma](how-to-configure-proxy-support.md). |
| **OfflineInstallationPath** | Dizin yolu | Bu parametreyi dahil ise, yükleyici yükleme için gerekli VC Çalışma zamanı MSI dosyaları ve IOT Edge cab listelenen dizinini kontrol edecektir. Dizinde bulunamadı. tüm dosyalar indirilir. Dizindeki her iki dosya varsa, internet bağlantısı olmadan IOT Edge yükleyebilirsiniz. Bu parametre, belirli bir sürümünü kullanmak için de kullanabilirsiniz. |
| **InvokeWebRequestParameters** | Hashtable parametreler ve değerler | Yükleme sırasında birkaç web isteklerinin yapılma. Bu web istekleri parametrelerini ayarlamak için bu alanı kullanın. Bu parametre, proxy sunucuları için kimlik bilgilerini yapılandırmak yararlıdır. Daha fazla bilgi için [bir proxy sunucu üzerinden iletişim kurmak için IOT Edge cihazı yapılandırma](how-to-configure-proxy-support.md). |
| **RestartIfNeeded** | yok | Bu bayrak gerekirse dağıtım betiği sormadan, makineyi yeniden başlatmanızı sağlar. |

### <a name="initialize-iotedge"></a>Initialize-IoTEdge

Initialize-IoTEdge komutu, cihaz bağlantı dizesini ve işletimsel ayrıntıları ile IOT Edge yapılandırır. Bu komut tarafından oluşturulan bilgilerin çoğunu, ardından iotedge\config.yaml dosyasında depolanır. Başlatma komutu, diğerlerinin yanı sıra bu ortak parametreleri kabul eder. Tam liste için komutunu `Get-Help Initialize-IoTEdge -full`. 

| Parametre | Kabul edilen değerler | Açıklamalar |
| --------- | --------------- | -------- |
| **El ile** | None | **Anahtarı parametre**. Varsayılan değer, sağlama türü yok belirtilirse, el ile gerçekleştirilir.<br><br>Cihazı el ile sağlamak için bir cihaz bağlantı dizesi sağlayacak bildirir |
| **DPS** | None | **Anahtarı parametre**. Varsayılan değer, sağlama türü yok belirtilirse, el ile gerçekleştirilir.<br><br>Bir cihaz sağlama hizmeti (DPS) kapsam kimliği ve sağlama DPS aracılığıyla cihazınızın kayıt kimliği sağlayacak bildirir.  |
| **DeviceConnectionString** | Bir bağlantı dizesinden bir IOT Hub'ı tek tırnak içinde kayıtlı bir IOT Edge cihazı | **Gerekli** el ile yükleme. Betik parametreleri bağlantı dizesinde sağlamazsanız, bir yükleme sırasında istenir. |
| **ScopeId** | Cihaz sağlama hizmeti ile IOT Hub'ınıza ilişkili örneğinden kapsam kimliği. | **Gerekli** dağıtım noktaları yükleme. Komut parametrelerinde bir kapsam kimliği sağlamıyorsa, bir yükleme sırasında istenir. |
| **RegistrationId** | Cihazınız tarafından oluşturulan bir kayıt kimliği | **Gerekli** dağıtım noktaları yükleme. Betik parametreleri bir kayıt Kimliğini sağlamazsanız, bir yükleme sırasında istenir. |
| **ContainerOs** | **Windows** veya **Linux** | Kapsayıcı işletim sistemi belirtilmezse, Windows varsayılan değerdir.<br><br>Windows kapsayıcıları için IOT Edge yüklemede moby container altyapısı kullanır. Linux kapsayıcıları için bir kapsayıcı altyapısı yüklemeye başlamadan önce yüklemeniz gerekir. |
| **InvokeWebRequestParameters** | Hashtable parametreler ve değerler | Yükleme sırasında birkaç web isteklerinin yapılma. Bu web istekleri parametrelerini ayarlamak için bu alanı kullanın. Bu parametre, proxy sunucuları için kimlik bilgilerini yapılandırmak yararlıdır. Daha fazla bilgi için [bir proxy sunucu üzerinden iletişim kurmak için IOT Edge cihazı yapılandırma](how-to-configure-proxy-support.md). |
| **AgentImage** | IOT Edge Aracısı görüntü URI'si | Varsayılan olarak, yeni bir IOT Edge yükleme son çalışırken etiketi IOT Edge Aracısı görüntüsünü kullanır. Görüntü sürümü için belirli bir etiket ayarlamak veya kendi Aracısı görüntüsü sağlamak için bu parametreyi kullanın. Daha fazla bilgi için [anlamak IOT Edge etiketler](how-to-update-iot-edge.md#understand-iot-edge-tags). |
| **Kullanıcı Adı** | Kapsayıcı kayıt defteri kullanıcı adı | Yalnızca bir kapsayıcıya - AgentImage parametre özel bir kayıt defteri ayarını yaparsanız, bu parametreyi kullanın. Kayıt defteri erişimi olan bir kullanıcı adı sağlayın. |
| **Parola** | Güvenli parola dizesi | Yalnızca bir kapsayıcıya - AgentImage parametre özel bir kayıt defteri ayarını yaparsanız, bu parametreyi kullanın. Kayıt defterine erişim için parola sağlayın. | 

### <a name="update-iotedge"></a>Güncelleştirme IoTEdge

| Parametre | Kabul edilen değerler | Açıklamalar |
| --------- | --------------- | -------- |
| **ContainerOs** | **Windows** veya **Linux** | İşletim sistemi belirtilen hiçbir kapsayıcı mevcut değilse, Windows varsayılan değerdir. Windows kapsayıcıları için bir kapsayıcı altyapısı yükleme dahil edilir. Linux kapsayıcıları için bir kapsayıcı altyapısı yüklemeye başlamadan önce yüklemeniz gerekir. |
| **Proxy** | Proxy URL'si | Cihazınız, İnternet'e erişmek için Ara sunucu üzerinden gitmesi gerekiyorsa, bu parametreyi dahil edin. Daha fazla bilgi için [bir proxy sunucu üzerinden iletişim kurmak için IOT Edge cihazı yapılandırma](how-to-configure-proxy-support.md). |
| **InvokeWebRequestParameters** | Hashtable parametreler ve değerler | Yükleme sırasında birkaç web isteklerinin yapılma. Bu web istekleri parametrelerini ayarlamak için bu alanı kullanın. Bu parametre, proxy sunucuları için kimlik bilgilerini yapılandırmak yararlıdır. Daha fazla bilgi için [bir proxy sunucu üzerinden iletişim kurmak için IOT Edge cihazı yapılandırma](how-to-configure-proxy-support.md). |
| **OfflineInstallationPath** | Dizin yolu | Bu parametreyi dahil ise, yükleyici yükleme için gerekli VC Çalışma zamanı MSI dosyaları ve IOT Edge cab listelenen dizinini kontrol edecektir. Dizinde bulunamadı. tüm dosyalar indirilir. Dizindeki her iki dosya varsa, internet bağlantısı olmadan IOT Edge yükleyebilirsiniz. Bu parametre, belirli bir sürümünü kullanmak için de kullanabilirsiniz. |
| **RestartIfNeeded** | yok | Bu bayrak gerekirse dağıtım betiği sormadan, makineyi yeniden başlatmanızı sağlar. |


### <a name="uninstall-iotedge"></a>Uninstall-IoTEdge

| Parametre | Kabul edilen değerler | Açıklamalar |
| --------- | --------------- | -------- |
| **Zorla** | yok | Önceki kaldırma girişimi başarısız oldu durumunda bu bayrak kaldırma işlemini zorlar. 
| **RestartIfNeeded** | yok | Bu bayrak, gerekirse kaldırma betiğini sormadan, makineyi yeniden başlatmanızı sağlar. |


## <a name="next-steps"></a>Sonraki adımlar

Yüklü olan çalışma zamanı ile sağlanan bir IOT Edge cihazına sahip olduğunuza göre şunları yapabilirsiniz [IOT Edge modüllerini dağıtmak](how-to-deploy-modules-portal.md).

IOT Edge düzgün yüklerken sorunlarla karşılaşıyorsanız, kullanıma [sorun giderme](troubleshoot.md) sayfası.

Var olan bir yüklemesini IOT Edge en yeni sürüme güncelleştirmek için bkz: [IOT Edge güvenlik arka plan programı ve çalışma zamanını güncelleştirme](how-to-update-iot-edge.md).
