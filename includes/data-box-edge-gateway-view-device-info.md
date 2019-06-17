---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 03/04/2019
ms.author: alkohli
ms.openlocfilehash: d5af557a62f4bd35c242d334c28a38c3d632f7cf
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66161156"
---
1. [PowerShell arabirimine bağlanma](#connect-to-the-powershell-interface).
2. Kullanım `Get-HcsApplianceInfo` cihazınız için bilgi almak için.

    Aşağıdaki örnek, bu cmdlet kullanımını gösterir:

    ```
    [10.100.10.10]: PS>Get-HcsApplianceInfo
    
    Id                            : b2044bdb-56fd-4561-a90b-407b2a67bdfc
    FriendlyName                  : DBE-NBSVFQR94S6
    Name                          : DBE-NBSVFQR94S6
    SerialNumber                  : HCS-NBSVFQR94S6
    DeviceId                      : 40d7288d-cd28-481d-a1ea-87ba9e71ca6b
    Model                         : Virtual
    FriendlySoftwareVersion       : Data Box Gateway 1902
    HcsVersion                    : 1.4.771.324
    IsClustered                   : False
    IsVirtual                     : True
    LocalCapacityInMb             : 1964992
    SystemState                   : Initialized
    SystemStatus                  : Normal
    Type                          : DataBoxGateway
    CloudReadRateBytesPerSec      : 0
    CloudWriteRateBytesPerSec     : 0
    IsInitialPasswordSet          : True
    FriendlySoftwareVersionNumber : 1902
    UploadPolicy                  : All
    DataDiskResiliencySettingName : Simple
    ApplianceTypeFriendlyName     : Data Box Gateway
    IsRegistered                  : False
    ```

    Bazı önemli cihaz bilgileri özetleyen bir tablo şu şekildedir:
    
    | Parametre                             | Açıklama                                                                                                                                                  |   |
    |--------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------|---|
    | FriendlyName                   | Yerel web UI cihaz dağıtım sırasında yapılandırdığınız cihaz kolay adı. Varsayılan kolay adı, cihaz seri numarasını şeklindedir.  |   |
    | seri numarası                   | Cihaz seri numarasını bir fabrikada atanmış benzersiz bir sayıdır.                                                                             |   |
    | Model                          | Veri kutusu Edge ya da veri kutusu ağ geçidi cihazınız için model. Veri kutusu ağ geçidi için sanal ve fiziksel veri kutusu Edge için modelidir.                   |   |
    | FriendlySoftwareVersion        | Cihaz yazılımı sürümüne karşılık gelen kolay dize. Preview çalıştıran bir sistem için kolay yazılım sürümü veri kutusu Edge 1902 olacaktır. |   |
    | HcsVersion                     | HCS yazılım sürümü, cihaz üzerinde çalışan. Örneğin, veri kutusu Edge 1902 için karşılık gelen HCS yazılım 1.4.771.324 sürümüdür.            |   |
    | LocalCapacityInMb              | Cihazın megabit cinsinden toplam yerel kapasite.                                                                                                        |   |
    | IsRegistered                   | Bu değer, Cihazınızı hizmetiyle etkinleştirilip etkinleştirilmediğini gösterir.                                                                                         |   |


