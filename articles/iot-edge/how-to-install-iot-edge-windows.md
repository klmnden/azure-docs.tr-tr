---
title: Azure IOT Edge üzerinde Windows yükleme | Microsoft Docs
description: Windows 10, Windows Server ve Windows IOT Core Azure IOT Edge yükleme yönergeleri
author: kgremban
manager: philmea
ms.reviewer: veyalla
ms.service: iot-edge
services: iot-edge
ms.topic: conceptual
ms.date: 03/14/2019
ms.author: kgremban
ms.custom: seodec18
ms.openlocfilehash: 95e984f6f08af01a2ffd7b9b4e0ec598d73f4d05
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60595119"
---
# <a name="install-the-azure-iot-edge-runtime-on-windows"></a>Windows üzerinde Azure IOT Edge çalışma zamanı yükleme

Azure IOT Edge çalışma zamanı, ne bir cihaz ile IOT Edge cihazı kapatır ' dir. Çalışma zamanı, cihaz olarak endüstriyel sunucusu olarak büyük veya küçük bir Raspberry Pi üzerinde dağıtılabilir. Bir cihaz IOT Edge çalışma zamanı ile yapılandırıldıktan sonra iş mantığı buluttan dağıttıktan başlayabilirsiniz. 

IOT Edge çalışma zamanı hakkında daha fazla bilgi için bkz: [Azure IOT Edge çalışma zamanı ve mimarisini anlama](iot-edge-runtime.md).

Bu makalede Azure IOT Edge çalışma zamanı, Windows x64 (AMD/Intel) yüklemek için adımları listelenmektedir sistem. Windows Destek şu anda Önizleme aşamasındadır.

> [!NOTE]
> Bilinen bir Windows işletim sistemi sorun uyku ve güç durumlarını, IOT Edge modülleri (işlem yalıtılmış Windows Nano sunucu kapsayıcıları) çalıştırırken, hazırda bekleme moduna geçiş engeller. Bu sorun, cihazın pil ömrü etkiler.
>
> Geçici bir çözüm olarak komutunu `Stop-Service iotedge` bu güç durumlarını kullanmadan önce çalışan tüm IOT Edge modülleri durdurmak için. 

<!--
> [!NOTE]
> Using Linux containers on Windows systems is not a recommended or supported production configuration for Azure IoT Edge. However, it can be used for development and testing purposes.
-->

Linux kullanarak Windows sistemlerinde kapsayıcı bir Azure IOT Edge için önerilen veya desteklenen üretim yapılandırma değil. Ancak, geliştirme ve test amacıyla kullanılabilir. 

## <a name="prerequisites"></a>Önkoşullar

Bu bölümde, Windows cihazınız, IOT Edge desteği olup olmadığını gözden geçirin ve yüklemeden önce kapsayıcı altyapısı için hazırlamak için kullanın. 

### <a name="supported-windows-versions"></a>Desteklenen Windows sürümleri

Azure IOT Edge, Windows kapsayıcıları ya da Linux kapsayıcıları çalıştırmakta olduğunuz bağlı olarak Windows,'ün farklı sürümlerini destekler. 

Azure IOT Edge en son sürümünü Windows kapsayıcıları ile aşağıdaki Windows sürümleri üzerinde çalıştırabilirsiniz:
* Windows 10 veya IOT Core ile Ekim 2018 Güncelleştirmesi (derleme 17763)
* Windows Server 2019 (derleme 17763)

En son sürümünü Azure IOT Edge ile Linux kapsayıcıları, aşağıdaki Windows sürümleri üzerinde çalıştırabilirsiniz: 
* Windows 10 Yıldönümü Güncelleştirmesi (derleme 14393) veya daha yeni
* Windows Server 2016 veya daha yenisi

Hangi işletim sistemleri şu anda desteklenen daha fazla bilgi için bkz. [Azure IOT Edge desteği](support.md#operating-systems). 

IOT Edge en son sürümüne nelerin dahil olduğunu hakkında daha fazla bilgi için bkz. [Azure IOT Edge serbest](https://github.com/Azure/azure-iotedge/releases).

### <a name="prepare-for-a-container-engine"></a>Bir kapsayıcı altyapısı için hazırlama 

Azure IOT Edge dayanır bir [OCI uyumlu](https://www.opencontainers.org/) container altyapısı. Üretim senaryoları için Windows Cihazınızda Windows kapsayıcılarını çalıştırmaya yönelik yükleme betiğini dahil Moby altyapısını kullanır. Geliştirme ve test etme, Windows Cihazınızda Linux kapsayıcıları çalıştırabilirsiniz ancak yüklemek ve IOT Edge yüklemeden önce bir kapsayıcı altyapısını yapılandırmak gerekmez. Her iki senaryo için önkoşulları Cihazınızı hazırlamak aşağıdaki bölümlere bakın. 

IOT Edge bir sanal makineye yüklemek isterseniz, iç içe sanallaştırmayı etkinleştirmek ve en az 2 GB bellek ayrılamadı. İç içe sanallaştırma nasıl etkinleştirmeniz hiper yöneticide kullanımınıza bağlı olarak farklılık gösterir. Hyper-V için 2. nesil sanal makineler varsayılan olarak etkin iç içe sanallaştırmayı. VMWare için sanal makinenizde özelliği etkinleştirmek için bir geçiş yoktur. 

#### <a name="moby-engine-for-windows-containers"></a>Windows kapsayıcıları için Moby altyapısı

Üretim senaryolarında IOT Edge çalıştıran Windows cihazlar için Moby yalnızca resmi olarak desteklenen kapsayıcı altyapısıdır. Cihazınızda otomatik olarak yükler Moby altyapısı IOT Edge yüklemeden önce yükleme komut dosyası. Cihazınız kapsayıcıları özelliğini etkinleştirerek hazırlayın. 

1. Arama Başlat çubuğunda **kapatma Windows özelliklerini aç veya Kapat** ve Denetim Masası programını açın.
2. Bulmak ve seçmek **kapsayıcıları**.
3. **Tamam**’ı seçin. 

#### <a name="docker-for-linux-containers"></a>Linux kapsayıcıları için docker

Geliştirme ve test kapsayıcıları Linux cihazları için Windows kullanıyorsanız, kullanabileceğiniz [için Docker Windows](https://www.docker.com/docker-windows) kapsayıcı altyapınız olarak. Docker için yapılandırılabilir [Linux kapsayıcılarını](https://docs.docker.com/docker-for-windows/#switch-between-windows-and-linux-containers). Docker'ı yüklemeniz ve IOT Edge yüklemeden önce yapılandırmanız gerekir. Linux kapsayıcılarını üretim ortamında, Windows cihazlarında desteklenmez. 

IOT Edge Cihazınızı bir Windows bilgisayar olduğunda bu karşıladığından emin olun [sistem gereksinimleri](https://docs.microsoft.com/virtualization/hyper-v-on-windows/reference/hyper-v-requirements) Hyper-V için.

## <a name="install-iot-edge-on-a-new-device"></a>IOT Edge yeni bir cihaza yükleme

>[!NOTE]
>Azure IOT Edge yazılım paketlerini (lisans dizininde) paketleri bulunan lisans koşullarına tabidir. Paket kullanarak önce lisans koşullarını okuyun. Bu koşulları kabul etmeniz, yükleme ve kullanım paket oluşturur. Lisans koşullarını kabul etmiyorsanız, paket kullanmayın.

Bir PowerShell Betiği indirir ve Azure IOT Edge güvenlik daemon'ı yükler. Güvenlik daemon daha sonra diğer modüllerin uzak dağıtımları sağlayan IOT Edge Aracısı ilk iki çalışma zamanı modüllerinin başlatır. 

IOT Edge çalışma zamanı, bir cihaz üzerinde ilk kez yüklediğinizde, bir IOT hub'ından kimliğe sahip cihazı sağlamak gerekir. Tek bir IOT Edge cihazı IOT Hub tarafından sağlanan cihaz bağlantı dizesini kullanarak el ile sağlanabilir. Veya, cihaz sağlama hizmeti otomatik olarak ayarlamak için birçok cihaz olduğunda kullanışlı olan cihazları sağlamak için kullanabilirsiniz. Sağlama seçiminize bağlı olarak, uygun yükleme komut dosyasını seçin. 

Aşağıdaki bölümlerde yaygın kullanım örnekleri ve IOT Edge yükleme betik parametreleri için yeni bir cihazda açıklanmaktadır. 

### <a name="option-1-install-and-manually-provision"></a>1. seçenek: Yükleme ve el ile sağlama

İlk seçenek, cihazı hazırlamak için IOT Hub tarafından üretilen cihaz bağlantı dizesi sağlayın. 

Bağlantısındaki [yeni bir Azure IOT Edge cihazı kaydedin](how-to-register-device-portal.md) Cihazınızı kaydedemedik ve cihaz bağlantı dizesini almak için. 

Bu örnek, Windows kapsayıcıları ile el ile yükleme gösterir:

```powershell
. {Invoke-WebRequest -useb aka.ms/iotedge-win} | Invoke-Expression; `
Install-SecurityDaemon -Manual -ContainerOs Windows -DeviceConnectionString '<connection-string>'
```

Yüklediğinizde ve bir cihazı el ile sağlama yükleme dahil olmak üzere değiştirmek için ek parametreler kullanabilirsiniz:
* Bir ara sunucu üzerinden gitmesi için trafiği
* Yükleyici çevrimdışı bir dizine işaret
* Belirli bir aracı kapsayıcı görüntüsü bildirme ve salt okunur ise bir özel kayıt defteri kimlik bilgilerini sağlayın
* Moby CLI yüklemeyi atla

Bu yükleme seçenekleri hakkında daha fazla bilgi için bu makaleyi okumaya devam edin veya hakkında bilgi edinmek için atla [tüm yükleme parametrelerini](#all-installation-parameters).

### <a name="option-2-install-and-automatically-provision"></a>2. seçenek: Yükleme ve otomatik olarak sağlama

Bu ikinci seçenek, IOT Hub cihaz sağlama Hizmeti'ni kullanarak cihaz sağlama. Sağlamak **kapsam kimliği** cihaz sağlama hizmeti örneğinden ve **kayıt kimliği** cihazınızdan.

Bağlantısındaki [oluşturma ve sağlama Windows sanal bir TPM Edge cihazında](how-to-auto-provision-simulated-device-windows.md) cihaz sağlama hizmetini ayarlama ve almak için kendi **kapsam kimliği**almak ve bir TPM cihazını benzetme kendi  **Kayıt Kimliği**, sonra bireysel kayıt oluşturma. Cihazınızı IOT hub'ına kaydedildiğinde, yükleme işlemine devam edin.  

   >[!TIP]
   >TPM simülatörünü yüklenmesi sırasında açık ve test çalıştıran pencereyi tutun. 

Aşağıdaki örnek, Windows kapsayıcıları ile otomatik bir yüklemesini göstermektedir:

```powershell
. {Invoke-WebRequest -useb aka.ms/iotedge-win} | Invoke-Expression; `
Install-SecurityDaemon -Dps -ContainerOs Windows -ScopeId <DPS scope ID> -RegistrationId <device registration ID>
```

Yüklediğinizde ve bir cihazı el ile sağlama yükleme dahil olmak üzere değiştirmek için ek parametreler kullanabilirsiniz:
* Bir ara sunucu üzerinden gitmesi için trafiği
* Yükleyici çevrimdışı bir dizine işaret
* Belirli bir aracı kapsayıcı görüntüsü bildirme ve salt okunur ise bir özel kayıt defteri kimlik bilgilerini sağlayın
* Moby CLI yüklemeyi atla

Bu yükleme seçenekleri hakkında daha fazla bilgi için bu makaleyi okumaya devam edin veya hakkında bilgi edinmek için atla [tüm yükleme parametrelerini](#all-installation-parameters).

## <a name="update-an-existing-installation"></a>Varolan bir yüklemeyi güncelleştirme

Önceden IOT Edge çalışma zamanı önce bir cihazda yüklü ve IOT Hub'ından bir kimlikle sağlanan ise, Basitleştirilmiş Kurulum komut dosyası kullanabilirsiniz. Bayrağı `-ExistingConfig` bir IOT Edge yapılandırma dosyası cihazda zaten olduğunu bildirir. Yapılandırma dosyası, cihaz kimlik yanı sıra sertifika ve ağ ayarları hakkında bilgi içerir. Cihazınızı el ile veya otomatik olarak başlangıçta sağlanan olup, bu yükleme seçeneğini kullanabilirsiniz. 

Daha fazla bilgi için [IOT Edge güvenlik arka plan programı ve çalışma zamanını güncelleştirme](how-to-update-iot-edge.md).

Bu örnekte, var olan bir yapılandırma dosyasını işaret eder ve Windows kapsayıcıları'nı kullanan bir yüklemesini göstermektedir: 

```powershell
. {Invoke-WebRequest -useb aka.ms/iotedge-win} | Invoke-Expression; `
Install-SecurityDaemon -ExistingConfig -ContainerOs Windows
```

IOT Edge var olan bir yapılandırma dosyasından yüklediğinizde, yükleme değiştirmek için ek parametreler kullanabilirsiniz dahil olmak üzere:
* Bir ara sunucu üzerinden gitmesi için trafiği
* Yükleyici çevrimdışı bir dizine işaret veya 
* Moby CLI yüklemeyi atlayın. 

Bu bilgileri zaten bir önceki yükleme yapılandırma dosyasında ayarlandığından betik parametreleri, IOT Edge Aracısı kapsayıcı görüntüsüne bildiremezsiniz. Aracı kapsayıcı görüntüsünü değiştirmek istiyorsanız, bunu config.yaml dosyasında yapın. 

Bu yükleme seçenekleri hakkında daha fazla bilgi için bu makaleyi okumaya devam edin veya hakkında bilgi edinmek için atla [tüm yükleme parametrelerini](#all-installation-parameters).

## <a name="offline-installation"></a>Çevrimdışı yükleme

Yükleme sırasında dört dosyaları karşıdan yüklenir: 
* IOT Edge güvenlik arka plan programı (iotedgd) zip 
* Moby altyapısı zip
* Moby CLI zip
* Visual C++ yeniden dağıtılabilir paket (VC Çalışma zamanı) MSI

Cihaza birini veya tümünü bu dosyaları önceden indir ve sonra yükleme betiğini noktası dosyalarını içeren dizin. Yükleyici, dizinin ilk denetler ve yalnızca bulunan olmayan bileşenleri indirir. Tüm dört dosyaları çevrimdışı varsa, internet bağlantısı ile yükleyebilirsiniz. Bu özellik, çevrimiçi bir veya daha fazla bileşenlerin sürümlerini geçersiz kılmak için de kullanabilirsiniz.  

Önceki sürümleri ile birlikte en son yükleme dosyaları için bkz. [Azure IOT Edge serbest bırakır](https://github.com/Azure/azure-iotedge/releases)

Çevrimdışı bileşenleri yüklemek için kullanın `-OfflineInstallationPath` parametresi ve dosya dizinine mutlak yolunu belirtin. Örneğin,

```powershell
. {Invoke-WebRequest -useb aka.ms/iotedge-win} | Invoke-Expression; `
Install-SecurityDaemon -Manual -DeviceConnectionString '<connection-string>' -OfflineInstallationPath C:\Downloads\iotedgeoffline
```

## <a name="all-installation-parameters"></a>Tüm yükleme parametreleri

Önceki bölümlerde yükleme senaryoları ile yükleme komut dosyasını değiştirmek için parametrelerini kullanma örnekleri kullanıma sunuldu. Bu bölümde, IOT Edge yüklemek için kullanabileceğiniz geçerli parametrelerin bir başvuru tablosu sağlar. Daha fazla bilgi için `get-help Install-SecurityDaemon -full` bir PowerShell penceresinde. 

IOT Edge ile var olan yapılandırmasını yüklemek için bu ortak parametreleri, yükleme komutunu kullanabilirsiniz: 

| Parametre | Kabul edilen değerler | Yorumlar |
| --------- | --------------- | -------- |
| **El ile** | None | **Anahtarı parametre**. Her yükleme ya da el ile dağıtım noktaları, bildirilmesi gerekir veya existingconfig.<br><br>Cihazı el ile sağlamak için bir cihaz bağlantı dizesi sağlayacak bildirir |
| **DPS** | None | **Anahtarı parametre**. Her yükleme ya da el ile dağıtım noktaları, bildirilmesi gerekir veya existingconfig.<br><br>Bir cihaz sağlama hizmeti (DPS) kapsam kimliği ve sağlama DPS aracılığıyla cihazınızın kayıt kimliği sağlayacak bildirir.  |
| **ExistingConfig** | None | **Anahtarı parametre**. Her yükleme ya da el ile dağıtım noktaları, bildirilmesi gerekir veya existingconfig.<br><br>Config.yaml dosya sağlama bilgilerini ile bir cihazda zaten olduğunu bildirir. |
| **DeviceConnectionString** | Bir bağlantı dizesinden bir IOT Hub'ı tek tırnak içinde kayıtlı bir IOT Edge cihazı | **Gerekli** el ile yükleme. Betik parametreleri bağlantı dizesinde sağlamazsanız, bir yükleme sırasında istenir. |
| **ScopeId** | Cihaz sağlama hizmeti ile IOT Hub'ınıza ilişkili örneğinden kapsam kimliği. | **Gerekli** dağıtım noktaları yükleme. Komut parametrelerinde bir kapsam kimliği sağlamıyorsa, bir yükleme sırasında istenir. |
| **RegistrationId** | Cihazınız tarafından oluşturulan bir kayıt kimliği | **Gerekli** dağıtım noktaları yükleme. Betik parametreleri bir kayıt Kimliğini sağlamazsanız, bir yükleme sırasında istenir. |
| **ContainerOs** | **Windows** veya **Linux** | İşletim sistemi belirtilen hiçbir kapsayıcı mevcut değilse, Linux varsayılan değerdir. Windows kapsayıcıları için bir kapsayıcı altyapısı yükleme dahil edilir. Linux kapsayıcıları için bir kapsayıcı altyapısı yüklemeye başlamadan önce yüklemeniz gerekir. Windows üzerinde çalışan Linux kapsayıcılar yararlı geliştirme senaryosu, ancak üretim ortamında desteklenmez. |
| **Proxy** | Proxy URL'si | Cihazınız, İnternet'e erişmek için Ara sunucu üzerinden gitmesi gerekiyorsa, bu parametreyi dahil edin. Daha fazla bilgi için [bir proxy sunucu üzerinden iletişim kurmak için IOT Edge cihazı yapılandırma](how-to-configure-proxy-support.md). |
| **InvokeWebRequestParameters** | Hashtable parametreler ve değerler | Yükleme sırasında birkaç web isteklerinin yapılma. Bu web istekleri parametrelerini ayarlamak için bu alanı kullanın. Bu parametre, proxy sunucuları için kimlik bilgilerini yapılandırmak yararlıdır. Daha fazla bilgi için [bir proxy sunucu üzerinden iletişim kurmak için IOT Edge cihazı yapılandırma](how-to-configure-proxy-support.md). |
| **OfflineInstallationPath** | Dizin yolu | Bu parametreyi dahil ise, yükleyici iotedged zip, Moby altyapısı zip, Moby CLI zip ve yükleme için gerekli VC Çalışma zamanı MSI dosyaları için dizin denetler. Tüm dört dosyaları dizinde ise IOT Edge sırasında yükleyebilirsiniz çevrimdışı. Bu parametre, belirli bir bileşeni'nın çevrimiçi sürümünü geçersiz kılmak için de kullanabilirsiniz. |
| **AgentImage** | IOT Edge Aracısı görüntü URI'si | Varsayılan olarak, yeni bir IOT Edge yükleme son çalışırken etiketi IOT Edge Aracısı görüntüsünü kullanır. Görüntü sürümü için belirli bir etiket ayarlamak veya kendi Aracısı görüntüsü sağlamak için bu parametreyi kullanın. Daha fazla bilgi için [anlamak IOT Edge etiketler](how-to-update-iot-edge.md#understand-iot-edge-tags). |
| **Kullanıcı Adı** | Kapsayıcı kayıt defteri kullanıcı adı | Yalnızca bir kapsayıcıya - AgentImage parametre özel bir kayıt defteri ayarını yaparsanız, bu parametreyi kullanın. Kayıt defteri erişimi olan bir kullanıcı adı sağlayın. |
| **Parola** | Güvenli parola dizesi | Yalnızca bir kapsayıcıya - AgentImage parametre özel bir kayıt defteri ayarını yaparsanız, bu parametreyi kullanın. Kayıt defterine erişim için parola sağlayın. | 
| **SkipMobyCli** | None | Yalnızca Windows için - ContainerOS ayarlanmışsa geçerlidir. Moby CLI (docker.exe) için $MobyInstallDirectory yüklemeyin. |

## <a name="verify-successful-installation"></a>Yüklemenin başarılı olduğunu doğrulamak

IOT Edge hizmetinin durumu kontrol edebilirsiniz: 

```powershell
Get-Service iotedge
```

Kullanarak son 5 dakika hizmet günlükleri inceleyin:

```powershell

# Displays logs from last 5 min, newest at the bottom.

Get-WinEvent -ea SilentlyContinue `
  -FilterHashtable @{ProviderName= "iotedged";
    LogName = "application"; StartTime = [datetime]::Now.AddMinutes(-5)} |
  select TimeCreated, Message |
  sort-object @{Expression="TimeCreated";Descending=$false} |
  format-table -autosize -wrap
```

Ve modüller ile çalışan listesi:

```powershell
iotedge list
```

Çalışan olduğunu gördüğünüz yalnızca modülü yeni bir yüklemeden sonra **edgeAgent**. Çalıştırdıktan sonra [IOT Edge modüllerini dağıtmak](how-to-deploy-modules-portal.md), diğerleri görürsünüz. 

## <a name="manage-module-containers"></a>Modül kapsayıcıları yönetin

IOT Edge hizmetinin, cihaz üzerinde çalışan bir container altyapısı gerektirir. IOT Edge çalışma zamanı kapsayıcı altyapısı, bir modül bir cihaza dağıttığınızda, bulutta bir kayıt defterinden kapsayıcı görüntüsü çekin için kullanır. IOT Edge hizmetinin modüllerinizi ile etkileşim kurmanızı ve günlüklerini sağlar, ancak bazen kapsayıcı ile etkileşim kurmak için kapsayıcı altyapısı kullanmak isteyebilirsiniz. 

Modül kavramları hakkında daha fazla bilgi için bkz. [anlamak Azure IOT Edge modülleri](iot-edge-modules.md). 

Windows IOT Edge Cihazınızda Windows kapsayıcıları çalıştırıyorsanız, IOT Edge yükleme Moby container altyapısı dahil. Windows geliştirme makinenizde Linux kapsayıcıları geliştiriyorsanız, büyük olasılıkla Docker Masaüstü kullanıyorsunuz. Moby altyapısı Docker aynı standartlarını temel ve Docker masaüstü ile aynı makinede paralel olarak çalıştırmak için tasarlanmıştır. Bu nedenle, hedef kapsayıcı Moby altyapısı tarafından yönetilen istiyorsanız Docker yerine bu altyapıyı özel olarak hedef vardır. 

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

## <a name="uninstall-iot-edge"></a>IOT Edge kaldırma

Windows cihazınızın IOT Edge yükleme kaldırmak istiyorsanız, yönetici bir PowerShell penceresinden aşağıdaki komutu kullanın. Bu komut, yapılandırmanızda ve Moby altyapısı verileri ile birlikte IOT Edge çalışma zamanı kaldırır. 

```powershell
. {Invoke-WebRequest -useb aka.ms/iotedge-win} | Invoke-Expression; `
Uninstall-SecurityDaemon -DeleteConfig -DeleteMobyDataRoot
```

IOT Edge Cihazınızda yeniden yüklemek istiyorsanız, dışlamayı `-DeleteConfig` ve `-DeleteMobyDataRoot` parametreleri böylece güvenlik daemon daha sonra mevcut yapılandırma bilgileriyle birlikte yeniden yükleyebilirsiniz. 

## <a name="next-steps"></a>Sonraki adımlar

Yüklü olan çalışma zamanı ile sağlanan bir IOT Edge cihazına sahip olduğunuza göre şunları yapabilirsiniz [IOT Edge modüllerini dağıtmak](how-to-deploy-modules-portal.md).

IOT Edge düzgün yüklerken sorunlarla karşılaşıyorsanız, kullanıma [sorun giderme](troubleshoot.md) sayfası.

Var olan bir yüklemesini IOT Edge en yeni sürüme güncelleştirmek için bkz: [IOT Edge güvenlik arka plan programı ve çalışma zamanını güncelleştirme](how-to-update-iot-edge.md).
