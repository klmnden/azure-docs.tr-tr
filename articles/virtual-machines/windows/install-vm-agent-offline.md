---
title: "Çevrimdışı modda Azure VM Aracısı'nı yükleme | Microsoft Docs"
description: "Çevrimdışı modda Azure VM Aracısı'nı yüklemeyi öğrenin."
services: virtual-machines-windows
documentationcenter: 
author: genlin
manager: timlt
editor: 
tags: azure-resource-manager
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 01/26/2018
ms.author: genli
ms.openlocfilehash: 3770cc3338c89a9bf88e2cf9ec3faa37e2ff109b
ms.sourcegitcommit: 4723859f545bccc38a515192cf86dcf7ba0c0a67
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/11/2018
---
# <a name="install-the-azure-virtual-machine-agent-in-offline-mode"></a>Çevrimdışı modda Azure sanal makine aracısını yükleme 

Azure sanal makine Aracısı (VM Aracısı), yerel yönetici parolasını sıfırlama ve itme betik gibi yararlı özellikleri sağlar. Bu makalede çevrimdışı bir Windows sanal makine (VM) VM aracısının nasıl yükleneceği gösterilmektedir. 

## <a name="when-to-use-the-vm-agent-in-offline-mode"></a>Çevrimdışı modda VM Aracısı kullanıldığı durumlar

Aşağıdaki senaryolarda çevrimdışı modda VM aracısını yükleyin:

- Dağıtılan Azure VM'de VM aracısının yüklü yok veya Aracısı çalışmıyor.
- VM için Yönetici parolanızı mı unuttunuz veya VM erişemiyor.

## <a name="how-to-install-the-vm-agent-in-offline-mode"></a>Çevrimdışı modda VM Aracısı yükleme

Çevrimdışı modda VM Aracısı'nı yüklemek için aşağıdaki adımları kullanın.

### <a name="step-1-attach-the-os-disk-of-the-vm-to-another-vm-as-a-data-disk"></a>1. adım: başka bir VM'e VM'nin işletim sistemi diski, bir veri diski ekleyin.

1.  VM silin. Seçtiğinizden emin olun **diskleri tutmak** VM sildiğinizde seçeneği.

2.  İşletim sistemi diski bir veri diski başka bir VM'e (olarak bilinen bir _sorun gidericisini_ VM). Daha fazla bilgi için bkz: [bir Windows VM Azure portalında bir veri diski ekleme](attach-managed-disk-portal.md).

3.  VM sorun gidericisini bağlayın. Açık **Bilgisayar Yönetimi** > **Disk Yönetimi**. İşletim sistemi diski çevrimiçi olduğunu ve sürücü harflerini disk bölümleri atandığını doğrulayın.

### <a name="step-2-modify-the-os-disk-to-install-the-azure-vm-agent"></a>2. adım: Azure VM Aracısı'nı yüklemek için işletim sistemi diski Değiştir

1.  Uzak Masaüstü Bağlantısı sorun gidericisini VM olun.

2.  Eklediğiniz işletim sistemi diski \windows\system32\config klasöre göz atın. Bir geri alma gerekli olması durumunda tüm dosyaların bir yedek olarak bu klasöre kopyalayın.

3.  Başlat **Kayıt Defteri Düzenleyicisi'ni** (regedit.exe).

4.  Seçin **HKEY_LOCAL_MACHINE** anahtarı. Menüsünde seçin **dosya** > **yığını**:

    ![Hive yükleme](./media/install-vm-agent-offline/load-hive.png)

5.  İliştirdiğiniz işletim sistemi diski \windows\system32\config\SYSTEM klasöre göz atın. Hive adını girin **BROKENSYSTEM**. Yeni kayıt defteri kovanı altında görüntülenen **HKEY_LOCAL_MACHINE** anahtarı.

6.  İliştirdiğiniz işletim sistemi diski \windows\system32\config\SOFTWARE klasöre göz atın. Hive yazılım adını girin **BROKENSOFTWARE**.

7.  VM Aracısı çalışmıyorsa, geçerli yapılandırma yedekleyin.

    >[!NOTE]
    >VM aracısının yüklü yoksa, 8. adımına geçin. 
      
    1. \Windowsazure klasörünü \windowsazure.old için yeniden adlandırın.

    2. Aşağıdaki kayıt defterleri dışarı aktarın:
        - HKEY_LOCAL_MACHINE\BROKENSYSTEM\ControlSet001\Services\WindowsAzureGuestAgent
        - HKEY_LOCAL_MACHINE\BROKENSYSTEM\\ControlSet001\Services\WindowsAzureTelemetryService
        - HKEY_LOCAL_MACHINE\BROKENSYSTEM\ControlSet001\Services\RdAgent

8.  VM sorun gidericisini varolan dosyaların depo olarak VM Aracısı yüklemesi için kullanın. Aşağıdaki adımları tamamlayın:

    1. Sorun gidericisini VM, aşağıdaki alt anahtarları (.reg) kayıt defteri biçiminde dışarı aktarın: 
        - HKEY_LOCAL_MACHINE  \SYSTEM\ControlSet001\Services\WindowsAzureGuestAgent
        - HKEY_LOCAL_MACHINE  \SYSTEM\ControlSet001\Services\WindowsAzureTelemetryService
        - HKEY_LOCAL_MACHINE  \SYSTEM\ControlSet001\Services\RdAgent

        ![Kayıt defteri alt anahtarları dışarı aktarma](./media/install-vm-agent-offline/backup-reg.png)

    2. Kayıt defteri dosyalarını düzenleyin. Giriş değeri her dosya içine değiştirme **sistem** için **BROKENSYSTEM** (aşağıdaki görüntüleri gösterildiği gibi) ve dosyayı kaydedin.

        ![Kayıt defteri alt anahtarı değerlerini değiştirin](./media/install-vm-agent-offline/change-reg.png)

    3. Kayıt defteri dosyaları, her kayıt defteri dosyasını çift tıklatarak depoya içeri aktarın.

    4. Aşağıdaki üç alt anahtarları içine başarıyla içeri aktarıldı onaylayın **BROKENSYSTEM** hive:
        - WindowsAzureGuestAgent
        - WindowsAzureTelemetryService
        - RdAgent

9.  C:\windowsazure\packages için VM Aracısı klasörüne kopyalamak &lt;bağlı işletim sistemi diski&gt;: \windowsazure\packages.

    ![VM Aracısı dosyalarını işletim sistemi diske kopyalama](./media/install-vm-agent-offline/copy-package.png)
      
    >[!NOTE]
    >Kopyalama **günlükleri** klasör. Hizmeti başlatıldıktan sonra yeni günlükler oluşturulur.

10.  Seçin **BROKENSYSTEM**. Menüsünden seçin **dosya** > **yığın**.

11.  Seçin **BROKENSOFTWARE**. Menüsünden seçin **dosya** > **yığın**.

12.  İşletim sistemi diski kullanımdan çıkarın ve ardından işletim sistemi diski kullanarak VM oluşturun.

13.  VM erişin. RdAgent çalıştırmak ve günlükleri oluşturulan dikkat edin.

Klasik dağıtım modeli kullanarak VM oluşturduysanız bitirdiniz.


### <a name="use-the-provisionguestagent-property-for-vms-created-with-azure-resource-manager"></a>Azure Resource Manager ile oluşturulan VM'ler için ProvisionGuestAgent özelliğini kullanın

Resource Manager dağıtım modeli kullanarak VM oluşturduysanız güncelleştirmek için Azure PowerShell modülü kullanın **ProvisionGuestAgent** özelliği. Özelliği Azure VM VM Aracısı'nın yüklü olduğunu bildirir.

Ayarlamak için **ProvisionGuestAgent** özelliği, Azure PowerShell'de aşağıdaki komutları çalıştırın:

   ```powershell
   $vm = Get-AzureVM –ServiceName <cloud service name> –Name <VM name>
   $vm.VM.ProvisionGuestAgent = $true
   Update-AzureVM –Name <VM name> –VM $vm.VM –ServiceName <cloud service name>
   ```

Ardından çalıştırın `Get-AzureVM` komutu. Dikkat **GuestAgentStatus** özelliği şimdi verilerle doldurulur:

   ```powershell
   Get-AzureVM –ServiceName <cloud service name> –Name <VM name>
   GuestAgentStatus:Microsoft.WindowsAzure.Commands.ServiceManagement.Model.PersistentVMModel.GuestAgentStatus
   ```

## <a name="next-steps"></a>Sonraki adımlar

- [Azure sanal makine aracısını genel bakış](agent-user-guide.md)
- [Sanal makine uzantıları ve özellikleri Windows için](extensions-features.md)
