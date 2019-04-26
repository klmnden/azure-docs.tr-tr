---
ms.openlocfilehash: aa2e6d80620f0a4cf5063919a6de53e4de20f706
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60301452"
---
1. Yükleyiciyi, korumak istediğiniz sunucuda yerel bir klasöre (örneğin, / tmp) kopyalayın. Bir terminal penceresinde aşağıdaki komutları çalıştırın:
     ```
     cd /tmp;
     tar -xvzf Microsoft-ASR_UA*release.tar.gz
     ```
2. Mobility hizmetini yüklemek için aşağıdaki komutu çalıştırın:

     ```
     sudo ./install -d <Install Location> -r MS -v VmWare -q
     ```
3. Mobility hizmeti yüklemesi tamamlandıktan sonra yapılandırma sunucusuna kaydedilmesi gerekir. Mobility hizmeti ile yapılandırma sunucusunu kaydetmek için aşağıdaki komutu çalıştırın:

     ```
     /usr/local/ASR/Vx/bin/UnifiedAgentConfigurator.sh -i <CSIP> -P /var/passphrase.txt
     ```

#### <a name="mobility-service-installer-command-line"></a>Mobility hizmeti yükleyicisi komut satırı

```
Usage:
./install -d <Install Location> -r <MS|MT> -v VmWare -q
```

|Parametre|Tür|Açıklama|Olası değerler|
|-|-|-|-|
|-r |Zorunlu|Mobility hizmeti (MS) yüklenmelidir ya da olan ana hedef (MT) yüklenmesi gerektiğini belirtir.|MS <br /> MT|
|-d |İsteğe bağlı|Mobility Hizmeti'nin yüklendiği konum.|/usr/local/ASR|
|-v|Zorunlu|Mobility hizmetinin yüklendiği platformunu belirtir. <br /> <br />- **VMware**: Üzerinde çalışan bir sanal makinesi üzerinde Mobility hizmetini yüklerseniz, bu değeri kullanın *VMware vSphere ESXi konakları*, *Hyper-V konakları*, ve *fiziksel sunucuları*. <br /> - **Azure**: Azure Iaas sanal makinesine bir aracıyı yüklerseniz, bu değeri kullanın.| VMware <br /> Azure|
|-q|İsteğe bağlı|Yükleyici sessiz modda çalıştırmak için belirtir.| Yok|

#### <a name="mobility-service-configuration-command-line"></a>Mobility hizmeti yapılandırma komut satırı

```
Usage:
cd /usr/local/ASR/Vx/bin
UnifiedAgentConfigurator.sh -i <CSIP> -P <PassphraseFilePath>
```

|Parametre|Tür|Açıklama|Olası değerler|
|-|-|-|-|
|-i |Zorunlu|Yapılandırma sunucusu IP'si|Herhangi bir geçerli IP adresi|
|-P |Zorunlu|Burada tümcecik geçirmek bağlantı dosyasının tam dosya yolunu kaydedildi|Herhangi bir geçerli klasörü|
<!--Update_Description: wording update-->
<!--ms.date: 03/05/2018-->