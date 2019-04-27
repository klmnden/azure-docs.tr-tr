---
title: Azure VM Aracısı çevrimdışı modda yükleme | Microsoft Docs
description: Azure VM Aracısı çevrimdışı modda yüklemeyi öğrenin.
services: virtual-machines-windows
documentationcenter: ''
author: genlin
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 10/31/2018
ms.author: genli
ms.openlocfilehash: e9fc8351b5e9a4f2274f0906d4071f86dcbcff26
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60640675"
---
# <a name="install-the-azure-virtual-machine-agent-in-offline-mode"></a>Azure sanal makine Aracısı çevrimdışı modda yükleme 

Azure sanal makine Aracısı (VM Aracısı), yerel yönetici parola sıfırlama ve komut gönderme gibi yararlı özellikleri sağlar. Bu makalede çevrimdışı bir Windows sanal makinesi (VM) için VM aracısının nasıl yükleneceği gösterilmektedir. 

## <a name="when-to-use-the-vm-agent-in-offline-mode"></a>Ne zaman çevrimdışı modda VM aracısını kullanılır?

Aşağıdaki senaryolarda çevrimdışı modda VM aracısını yükleyin:

- Dağıtılan Azure VM'yi yüklü VM aracısı yok veya Aracısı çalışmıyor.
- VM için Yönetici parolanızı mı unuttunuz veya VM'ye erişilemiyor.

## <a name="how-to-install-the-vm-agent-in-offline-mode"></a>Çevrimdışı modda VM aracısını yükleme

Çevrimdışı modda VM aracısını yüklemek için aşağıdaki adımları kullanın.

> [!NOTE]
> Çevrimdışı modda VM aracısını yükleme işlemi otomatik hale getirebilirsiniz.
> Bunu yapmak için [Azure VM kurtarma betikleri](https://github.com/Azure/azure-support-scripts/blob/master/VMRecovery/ResourceManager/README.md). Azure VM kurtarma betiklerini kullanmayı seçerseniz, aşağıdaki yordamı kullanabilirsiniz:
> 1. Etkilenen sanal Makinenin işletim sistemi diskini bir kurtarma VM'si eklemek için komut dosyalarını kullanarak 1. adıma geçin.
> 2. Risk azaltma işlemleri uygulamak için 2-10 arasındaki adımları izleyin.
> 3. VM yeniden oluşturmak için komut dosyalarını kullanarak 11. adıma geçin.
> 4. 12. adımı izleyin.

### <a name="step-1-attach-the-os-disk-of-the-vm-to-another-vm-as-a-data-disk"></a>1. Adım: Sanal Makinenin işletim sistemi diskini başka bir VM'ye veri diski olarak ekleyin.

1.  VM'yi silin. Seçtiğinizden emin olun **diskleri tutmak** VM'yi sildiğinizde seçeneği.

2.  İşletim sistemi diskini başka bir VM'ye veri diski olarak ekleyin (olarak bilinen bir _sorun giderici_ VM). Daha fazla bilgi için [Azure portalında bir Windows sanal makinesine veri diski](../windows/attach-managed-disk-portal.md).

3.  Sorun giderici VM bağlanın. Açık **Bilgisayar Yönetimi** > **Disk Yönetimi**. İşletim sistemi diskinin çevrimiçi olduğundan ve disk bölümleri için sürücü harfleri atandığını doğrulayın.

### <a name="step-2-modify-the-os-disk-to-install-the-azure-vm-agent"></a>2. Adım: Azure VM Aracısı'nı yüklemek için işletim sistemi diskini değiştirme

1.  Uzak Masaüstü Bağlantısı VM sorun giderici olun.

2.  Bağlı işletim sistemi diskinde \windows\system32\config klasörüne göz atın. Bir geri alma gerekli olması durumunda tüm dosyaların yedek olarak, bu klasöre kopyalayın.

3.  Başlangıç **Kayıt Defteri Düzenleyicisi'ni** (regedit.exe).

4.  Seçin **HKEY_LOCAL_MACHINE** anahtarı. Menüsünde **dosya** > **yığını**:

    ![Hive'ı yükleme](./media/install-vm-agent-offline/load-hive.png)

5.  Bağlı işletim sistemi diskinden \windows\system32\config\SYSTEM klasörüne göz atın. Hive için adı girin **BROKENSYSTEM**. Yeni kayıt defteri kovanını altında görüntülenen **HKEY_LOCAL_MACHINE** anahtarı.

6.  Bağlı işletim sistemi diski \windows\system32\config\SOFTWARE klasörüne göz atın. Hive yazılım adını girin **BROKENSOFTWARE**.

7. Ekli işletim sistemi diskinin VM aracısının yüklü olduğu bir yedeğini geçerli yapılandırmasını gerçekleştirin. VM aracısı yüklü olmaması durumunda, sonraki adıma taşıyın.
      
    1. \Windowsazure klasörü \windowsazure.old için yeniden adlandırın.

    2. Aşağıdaki kayıt defterleri dışarı aktarın:
        - HKEY_LOCAL_MACHINE\BROKENSYSTEM\ControlSet001\Services\WindowsAzureGuestAgent
        - HKEY_LOCAL_MACHINE\BROKENSYSTEM\\ControlSet001\Services\WindowsAzureTelemetryService
        - HKEY_LOCAL_MACHINE\BROKENSYSTEM\ControlSet001\Services\RdAgent

8.  Mevcut dosyaları giderici VM için VM Aracısı yüklemesini bir deposu olarak kullanın. Aşağıdaki adımları tamamlayın:

    1. VM gidericisinden aşağıdaki biçimi (.reg) kayıt defteri anahtarlarında dışarı aktarın: 
        - HKEY_LOCAL_MACHINE \SYSTEM\ControlSet001\Services\WindowsAzureGuestAgent
        - HKEY_LOCAL_MACHINE  \SYSTEM\ControlSet001\Services\WindowsAzureTelemetryService
        - HKEY_LOCAL_MACHINE \SYSTEM\ControlSet001\Services\RdAgent

          ![Kayıt defteri alt anahtarları Dışarı Aktar](./media/install-vm-agent-offline/backup-reg.png)

    2. Kayıt defteri dosyaları düzenleyin. Her dosyada giriş değeri değiştirmek **sistem** için **BROKENSYSTEM** (aşağıdaki görüntüde gösterildiği gibi) ve dosyayı kaydedin. Unutmayın **ImagePath** geçerli VM Aracısı'nın. Biz ekli işletim sistemi diski için ilgili klasörüne kopyalamanız gerekir. 

        ![Kayıt defteri alt anahtarı değerlerini değiştirme](./media/install-vm-agent-offline/change-reg.png)

    3. Kayıt defteri dosyaları, depoya her kayıt defteri dosyasını çift tıklayarak aktarın.

    4. Aşağıdaki üç alt içine başarıyla içeri aktarıldı onaylayın **BROKENSYSTEM** hive:
        - WindowsAzureGuestAgent
        - WindowsAzureTelemetryService
        - RdAgent

    5. Geçerli VM aracısını yükleme klasörü için ekli işletim sistemi diski kopyalayın: 

        1.  Bağlı işletim sistemi diskinde, kök yolu WindowsAzure adlı bir klasör oluşturun.

        2.  Sorun giderici VM için C:\WindowsAzure gidin, C:\WindowsAzure\GuestAgent_X.X.XXXX.XXX ada sahip herhangi bir klasörü arayın. C:\WindowsAzure en son sürüm numarasını ekli işletim sistemi diski WindowsAzure klasörüne sahip GuestAgent klasöre kopyalayın. Klasörü kopyalanması gereken emin değilseniz tüm GuestAgent klasörlerini kopyalayın. Aşağıdaki görüntüde, ekli işletim sistemi diskine kopyalanır GuestAgent klasörü örneği gösterilmektedir.

             ![GuestAgent klasörünü kopyalayın](./media/install-vm-agent-offline/copy-files.png)

9.  Seçin **BROKENSYSTEM**. Menüden **dosya** > **yığın**.

10.  Seçin **BROKENSOFTWARE**. Menüden **dosya** > **yığın**.

11.  İşletim sistemi diskini ve VM'nin işletim sistemi diski kullanarak yeniden.

12.  VM erişin. RdAgent çalışıyor ve günlükleri üretilir dikkat edin.

Resource Manager dağıtım modeli kullanarak bir VM oluşturduysanız, hazırsınız.

### <a name="use-the-provisionguestagent-property-for-classic-vms"></a>Klasik VM'ler için ProvisionGuestAgent özelliğini kullanın

Klasik modeli kullanarak bir VM oluşturduysanız, güncelleştirmek için Azure PowerShell modülü kullanın **ProvisionGuestAgent** özelliği. Özelliği, Azure VM VM aracısının yüklü olduğunu bildirir.

Ayarlanacak **ProvisionGuestAgent** özelliği, Azure PowerShell'de aşağıdaki komutları çalıştırın:

   ```powershell
   $vm = Get-AzureVM –ServiceName <cloud service name> –Name <VM name>
   $vm.VM.ProvisionGuestAgent = $true
   Update-AzureVM –Name <VM name> –VM $vm.VM –ServiceName <cloud service name>
   ```

Ardından çalıştırın `Get-AzureVM` komutu. Dikkat **GuestAgentStatus** özelliği verilerle doldurulur:

   ```powershell
   Get-AzureVM –ServiceName <cloud service name> –Name <VM name>
   GuestAgentStatus:Microsoft.WindowsAzure.Commands.ServiceManagement.Model.PersistentVMModel.GuestAgentStatus
   ```

## <a name="next-steps"></a>Sonraki adımlar

- [Azure sanal makine Aracısı genel bakış](../extensions/agent-windows.md)
- [Sanal makine uzantıları ve özellikleri Windows için](../extensions/features-windows.md)
