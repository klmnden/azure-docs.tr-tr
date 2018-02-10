---
title: "Çevrimdışı modda Azure VM Aracısı'nı yükleme | Microsoft Docs"
description: "Çevrimdışı modda Azure VM Aracısı'nı yükleme hakkında bilgi edinin."
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
ms.openlocfilehash: b38bfaffad3e214f3b854715e4d8030cde432bae
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/09/2018
---
# <a name="install-the-vm-agent-in-offline-mode-in-an-azure-windows-vm"></a>Bir Azure Windows VM çevrimdışı modda VM Aracısı'nı yükleme 

VM aracısı yerel yönetici parolasını sıfırlama ve komut dosyası gönderme gibi yararlı özellikleri sağlar. Bu makalede, çevrimdışı bir VM için VM aracısının nasıl yükleneceği gösterilmektedir. 

## <a name="when-to-use-offline-mode"></a>Ne zaman çevrimdışı modunu kullanmak için

Aşağıdaki senaryoda çevrimdışı modda VM Aracısı'nı yüklemeniz gerekir:

- VM aracısı yüklü veya VM Aracısı çalışmıyor bir Azure VM dağıtın.
- Yönetici parolasını anımsamıyorsanız veya VM erişemiyor.

Bu senaryoda, çevrimdışı modda VM Aracısı'nı yüklemeniz gerekir. 



## <a name="detailed-steps"></a>Ayrıntılı adımlar

**1. adım: başka bir VM'e VM'nin işletim sistemi diski, bir veri diski ekleyin.**

1.  VM silin. Seçtiğinizden emin olun **diskleri tutmak** bunu yaptığınızda seçeneği.

2.  İşletim sistemi diski bir veri diski başka bir VM'e (bir sorun gidericisini VM) ekleyin. Daha fazla bilgi edinmek için bkz. [Azure portalında bir Windows sanal makinesine veri diski ekleme](attach-managed-disk-portal.md).

3.  Sorun giderme işlemlerini yapacağınız VM'ye bağlanın. Açık **Bilgisayar Yönetimi** > **Disk Yönetimi**. İşletim sistemi diski çevrimiçi olduğunu ve bölümleri atanan sürücü harfleri sahip olduğunuzdan emin olun.

**2. adım: VM aracısı yüklemek için işletim sistemi diski Değiştir**

1.  Bir Uzak Masaüstü Bağlantısı VM giderilir olun.

2.  İliştirdiğiniz OS diske gidin **\windows\system32\config**. Bir geri alma gerekli olması durumunda yedekleme olarak tüm dosyaları kopyalayın.

3.  Kayıt Defteri Düzenleyicisi'ni (regedit.exe) başlatın.

4.  Tıklatın **HKEY_LOCAL_MACHINE** anahtar ve ardından **dosya** > **yığını** menüsünde.

    ![Yığını Yükle](./media/install-vm-agent-offline/load-hive.png)

5.  Gidin **\windows\system32\config\SYSTEM** işletim sistemi, bağlı disk ve hive adı olarak BROKENSYSTEM yazın. Bunu yaptıktan sonra kayıt defteri altında hive görürsünüz **HKEY_LOCAL_MACHINE**.

6.  Gidin **\windows\system32\config\SOFTWARE** üzerinde işletim sistemi diski, hive adı olarak BROKENSOFTWARE yazın bağlı.

7.  Çalışmadığını Aracısı'nın geçerli bir sürüm varsa, geçerli yapılandırma bir yedeğini alın. VM VM aracısının yüklü sahip değilse bir sonraki adıma gidin.  
      
    1. Klasör \windowsazure \windowsazure.old için yeniden adlandırma

    2. Aşağıdaki kayıt defterleri dışarı aktarma
        - HKEY_LOCAL_MACHINE\BROKENSYSTEM\ControlSet001\Services\WindowsAzureGuestAgent
        - HKEY_LOCAL_MACHINE \BROKENSYSTEM\\ControlSet001\Services\WindowsAzureTelemetryService
        - HKEY_LOCAL_MACHINE\BROKENSYSTEM\ControlSet001\Services\RdAgent

8.  Sorun giderme VM varolan dosyalar için VM Aracısı yükleme depo olarak kullanın: 

    1. Sorun gidericisini VM, aşağıdaki alt anahtarları (.reg) kayıt defteri biçiminde dışarı aktarın: 

        - HKEY_LOCAL_MACHINE  \SYSTEM\ControlSet001\Services\WindowsAzureGuestAgent
        - HKEY_LOCAL_MACHINE  \SYSTEM\ControlSet001\Services\WindowsAzureTelemetryService
        - HKEY_LOCAL_MACHINE  \SYSTEM\ControlSet001\Services\RdAgent

        ![Görüntünün verme kayıt defteri anahtarları hakkında](./media/install-vm-agent-offline/backup-reg.png)

    2. Bu üç kayıt defteri dosyaları düzenlemek, değişiklik **sistem** anahtar girişe **BROKENSYSTEM**ve ardından dosyaları kaydedin.
        ![Görüntünün değişiklik kayıt defteri anahtarları hakkında](./media/install-vm-agent-offline/change-reg.png)
    3. Bunları almak için kayıt defteri dosyaları çift.
    4. Aşağıdaki anahtarları BROKENSYSTEM hive başarıyla aktarıldığından emin olun: WindowsAzureGuestAgent, WindowsAzureTelemetryService ve RdAgent.

9.  C:\windowsazure\packages için VM Aracısı klasörüne kopyalamak &lt;bağlı işletim sistemi diski&gt;: \windowsazure\packages.
    ![Görüntünün kopyalama dosyaları hakkında](./media/install-vm-agent-offline/copy-package.png)
      
    **Not** günlükleri klasöründe bulundurmanıza gerek yoktur. Hizmeti başlatıldıktan sonra yeni günlük oluşturulmayacak.

10.  BROKENSYSTEM tıklayın ve **dosya** > **yığın** menüsünde.

11.  BROKENSOFTWARE tıklayın ve **dosya** > **yığın** menüsünde.

12.  Diski kullanımdan çıkarın ve ardından işletim sistemi diski kullanarak VM oluşturun.

13.  VM erişirseniz çalıştıran RDAgent ve oluşturulan günlükleri şimdi görürsünüz.

14. Bu Klasik dağıtım modeli kullanılarak oluşturulmuş bir VM ise gerçekleştirilir. Ancak bu Resource Manager dağıtım modeli kullanılarak oluşturulmuş bir VM ise, Azure PowerShell Bu Azure VM aracısının yüklü olduğunu bilmesi için ProvisionGuestAgent özelliğini güncelleştirmek için de kullanmalıdır. Bunu gerçekleştirmek için Azure PowerShell'de aşağıdaki komutları çalıştırın:

        $vm = Get-AzureVM –ServiceName <cloud service name> –Name <VM name>
        $vm.VM.ProvisionGuestAgent = $true
        Update-AzureVM –Name <VM name> –VM $vm.VM –ServiceName <cloud service name>

    Çalıştırırsanız **Get-AzureVM** bu adımları uyguladıktan sonra **GuestAgentStatus** özellik boş bir sayfa görüntülenmesine yerine doldurulmuş:

        Get-AzureVM –ServiceName <cloud service name> –Name <VM name>
        GuestAgentStatus:Microsoft.WindowsAzure.Commands.ServiceManagement.Model.PersistentVMModel.GuestAgentStatus

## <a name="next-steps"></a>Sonraki adımlar

- [Azure sanal makine aracısını genel bakış](agent-user-guide.md)
- [Sanal makine uzantıları ve özellikleri Windows için](extensions-features.md)