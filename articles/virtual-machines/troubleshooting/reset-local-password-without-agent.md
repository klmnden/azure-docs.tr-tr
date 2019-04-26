---
title: Azure Aracısı kullanmadan yerel Windows parola sıfırlama | Microsoft Docs
description: Azure Konuk aracısı yüklü olan veya çalışan bir VM'de olmadığında, yerel bir Windows kullanıcı hesabı parolasını sıfırlama
services: virtual-machines-windows
documentationcenter: ''
author: genlin
manager: jeconnoc
editor: ''
ms.assetid: cf353dd3-89c9-47f6-a449-f874f0957013
ms.service: virtual-machines-windows
ms.topic: troubleshooting
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 10/31/2018
ms.author: genli
ms.openlocfilehash: 6b77ceb2ab9abe232cec75254b30ce37c3dbbf60
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60307751"
---
# <a name="reset-local-windows-password-for-azure-vm-offline"></a>Azure VM için çevrimdışı ile yerel Windows parola sıfırlama
Kullanarak Azure'daki bir sanal makinenin yerel Windows parolasını sıfırlayabilir [Azure portal veya Azure PowerShell](reset-rdp.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) sağlanan Azure Konuk Aracısı yüklenir. Bu yöntem, bir Azure sanal makinesi için bir parola sıfırlama için birincil yoludur. Azure Konuk aracısı yanıt vermiyor ile sorunlarla ya da özel bir resim karşıya yüklendikten sonra yüklemek başarısız, el ile yapabilecekleriniz Windows parola sıfırlama. Bu makalede, kaynak işletim sistemi sanal disk başka bir sanal makineye ekleyerek bir yerel hesap parolası sıfırlama işlemi açıklanmaktadır. Bu makalede açıklanan adımları Windows etki alanı denetleyicileri için geçerli değildir. 

> [!WARNING]
> Bu işlemi yalnızca son çare olarak kullanın. Kullanarak parolalarını sıfırlamak her zaman deneyin [Azure portal veya Azure PowerShell](reset-rdp.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) ilk.
> 
> 

## <a name="overview-of-the-process"></a>İşlemine genel bakış
Azure Konuk aracısı için erişimi olmadığında sıfırlama azure'da bir Windows sanal makinesi için yerel bir parolası gerçekleştirmek için temel adımlar aşağıdaki gibidir:

* Kaynak VM silin. Sanal diskleri korunur.
* Azure aboneliğinizle aynı konumdaki başka bir VM için kaynak sanal makinenin işletim sistemi diski ekleyin. Bu VM, sorun giderme sanal makinesi adlandırılır.
* Sorun giderme sanal makinesi, bazı yapılandırma dosyalarına bağlı kaynak sanal makinenin işletim sistemi diski oluşturun.
* Sanal makinenin işletim sistemi diskini sorun giderme sanal makineden çıkarın.
* Orijinal sanal diski kullanarak, bir VM oluşturmak için Resource Manager şablonu kullanın.
* Yeni sanal makine önyüklendiğinde, oluşturduğunuz yapılandırma dosyaları gerekli kullanıcı parolasını güncelleştirin.

> [!NOTE]
> Aşağıdaki işlemleri otomatik hale getirebilirsiniz:
>
> - Sorun giderme sanal makinesi oluşturma
> - İşletim sistemi diski ekleme
> - Orijinal VM'yi yeniden oluşturuluyor
> 
> Bunu yapmak için [Azure VM kurtarma betikleri](https://github.com/Azure/azure-support-scripts/blob/master/VMRecovery/ResourceManager/README.md). Azure VM kurtarma betiklerini kullanmayı seçerseniz, aşağıdaki işlem "ayrıntılı adımlar" bölümünde kullanabilirsiniz:
> 1. Etkilenen sanal Makinenin işletim sistemi diskini bir kurtarma VM'si eklemek için komut dosyalarını kullanarak Atla adım 1 ve 2.
> 2. Risk azaltma işlemleri uygulamak için 3-6. adımları izleyin.
> 3. Adım 7-9, VM'yi yeniden oluşturmak için komut dosyalarını kullanarak atlayın.
> 4. 10 ve 11. adımları izleyin.

## <a name="detailed-steps"></a>Ayrıntılı adımlar

> [!NOTE]
> Adımları Windows etki alanı denetleyicileri için geçerli değildir. Yalnızca tek başına sunucu veya bir etki alanının üyesi olan bir sunucu üzerinde çalışır.
> 
> 

Kullanarak parolalarını sıfırlamak her zaman deneyin [Azure portal veya Azure PowerShell](reset-rdp.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) aşağıdaki adımları denemeden önce. Başlamadan önce sanal Makinenizin yedeğini sahip olduğunuzdan emin olun. 

1. Azure portalında etkilenen VM'yi silin. VM'yi sildiğinizde, yalnızca Azure içinde bir VM'nin başvuru meta verileri siler. Sanal diskler, VM silindiğinde korunur:
   
   * Azure portalında bir VM seçin, *Sil*:
     
     ![Mevcut VM'yi silin](./media/reset-local-password-without-agent/delete_vm.png)
2. Kaynak sanal makinenin işletim sistemi diski, sorun giderme VM'e ekleyin. Sorun giderme sanal makinesi kaynak sanal makinenin işletim sistemi diski ile aynı bölgede olması gerekir (gibi `West US`):
   
   * Azure Portalı'nda sorun giderme sanal Makineyi seçin. Tıklayın *diskleri* | *iliştirme varolan*:
     
     ![Var olan bir diski ekle](./media/reset-local-password-without-agent/disks_attach_existing.png)
     
     Seçin *VHD dosyasını* ve kaynak VM'NİZİN içeren depolama hesabı seçin:
     
     ![Depolama hesabı seçme](./media/reset-local-password-without-agent/disks_select_storageaccount.PNG)
     
     Kaynak kapsayıcı seçin. Kaynak genellikle kapsayıcıdır *VHD'ler*:
     
     ![Depolama kapsayıcısı seçin](./media/reset-local-password-without-agent/disks_select_container.png)
     
     İşletim sistemi vhd'si eklemek için seçin. Tıklayın *seçin* işlemi tamamlamak için:
     
     ![Kaynak sanal disk seçin](./media/reset-local-password-without-agent/disks_select_source_vhd.png)
3. Uzak Masaüstü kullanarak sorun giderme sanal makinesine bağlanın ve kaynak sanal makinenin işletim sistemi diski görünür olduğundan emin olun:
   
   * Azure Portalı'nda sorun giderme sanal makinesi seçip tıklayın *Connect*.
   * İndirilen RDP dosyasını açın. Kullanıcı adı ve sorun giderme sanal parolasını girin.
   * Dosya Gezgini'nde, bağlı veri diski için bakın. Kaynak sanal makinenin VHD sorun giderme sanal makinesine bağlı yalnızca bir veri diski ise, F: sürücü olmalıdır:
     
     ![Bağlı veri diski görüntüleme](./media/reset-local-password-without-agent/troubleshooting_vm_fileexplorer.png)
4. Oluşturma `gpt.ini` içinde `\Windows\System32\GroupPolicy` kaynak sanal makinenin sürücüde (gpt.ini.bak için yeniden adlandırın gpt.ini varsa):
   
   > [!WARNING]
   > Yanlışlıkla aşağıdaki dosyaları C:\Windows, sorun giderme sanal makinesi için işletim sistemi sürücüsünü içinde oluşturduğunuz değil emin olun. Aşağıdaki dosyalar, kaynak veri diski olarak bağlı olduğu VM için işletim sistemi sürücüsünü oluşturun.
   > 
   > 
   
   * İçine aşağıdaki satırları ekleyin `gpt.ini` oluşturduğunuz dosyası:
     
     ```
     [General]
     gPCFunctionalityVersion=2
     gPCMachineExtensionNames=[{42B5FAAE-6536-11D2-AE5A-0000F87571E3}{40B6664F-4972-11D1-A7CA-0000F87571E3}]
     Version=1
     ```
     
     ![GPT.ini oluşturma](./media/reset-local-password-without-agent/create_gpt_ini.png)
5. Oluşturma `scripts.ini` içinde `\Windows\System32\GroupPolicy\Machine\Scripts\Startup`. Gizli klasörlere gösterilen emin olun. Gerekirse Oluştur `Machine` veya `Scripts` klasörleri.
   
   * Aşağıdaki satırları ekleyin `scripts.ini` oluşturduğunuz dosyası:
     
     ```
     [Startup]
     0CmdLine=C:\Windows\System32\FixAzureVM.cmd
     0Parameters=
     ```
     
     ![Scripts.ini oluşturma](./media/reset-local-password-without-agent/create_scripts_ini.png)
6. Oluşturma `FixAzureVM.cmd` içinde `\Windows\System32` değiştirerek aşağıdaki içeriklerle `<username>` ve `<newpassword>` kendi değerlerinizle:
   
    ```
    net user <username> <newpassword> /add
    net localgroup administrators <username> /add
    net localgroup "remote desktop users" <username> /add
    ```

    ![Create FixAzureVM.cmd](./media/reset-local-password-without-agent/create_fixazure_cmd.png)
   
    Yeni parola tanımlarken, sanal makine için yapılandırılan parola karmaşıklık gereksinimleri karşılaması gerekir.
7. Azure Portalı'nda sorun giderme VM'den bir diski ayırma:
   
   * Azure Portalı'nda sorun giderme sanal makinesi seçin, *diskleri*.
   * Veri diski, 2. adımda eklenen seçin *ayırma*:
     
     ![Diski kullanımdan çıkar](./media/reset-local-password-without-agent/detach_disk.png)
8. VM oluşturmadan önce kaynak işletim sistemi diski için URI alın:
   
   * Azure portalında depolama hesabı seçin, *Blobları*.
   * Kapsayıcıyı seçin. Kaynak genellikle kapsayıcıdır *VHD'ler*:
     
     ![Depolama hesabı blob'u seçin](./media/reset-local-password-without-agent/select_storage_details.png)
     
     VM işletim sistemi VHD'si kaynak seçip tıklayın *kopyalama* düğmesinin yanındaki *URL* adı:
     
     ![Kopyalama-diski-URİ'si](./media/reset-local-password-without-agent/copy_source_vhd_uri.png)
9. Kaynak sanal makinenin işletim sistemi diskinden VM oluşturma:
   
   * Kullanım [bu Azure Resource Manager şablonu](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-specialized-vhd-new-or-existing-vnet) özelleştirilmiş bir VHD'den VM oluşturma. Tıklayın `Deploy to Azure` düğmesini sizin için doldurulur şablonlu ayrıntılı Azure portalını açın.
   * VM için tüm önceki ayarları korumak isteyip istemediğinizi seçin *şablonu Düzen* mevcut bir VNet, alt ağ, ağ bağdaştırıcısı veya genel IP sağlamak için.
   * İçinde `OSDISKVHDURI` parametre metin kutusu, yapıştırma kaynağınızı VHD URI'si, önceki adımda elde:
     
     ![Şablondan VM oluşturma](./media/reset-local-password-without-agent/create_new_vm_from_template.png)
10. Yeni sanal makine çalışmaya başladıktan sonra belirtilen yeni parola ile Uzak Masaüstü kullanarak sanal makineye bağlanma `FixAzureVM.cmd` betiği.
11. Yeni VM, uzak oturumunuzda, ortamı temizlemek için aşağıdaki dosyaları kaldırın:
    
    * %Windir%\System32
      * remove FixAzureVM.cmd
    * From %windir%\System32\GroupPolicy\Machine\
      * scripts.ini Kaldır
    * From %windir%\System32\GroupPolicy
      * GPT.ini (gpt.ini daha önce mevcut ve gpt.ini.bak, .bak dosyası için gpt.ini geri yeniden adlandırma için adlandırdığınız varsa) Kaldır

## <a name="next-steps"></a>Sonraki adımlar
Uzak Masaüstü kullanarak'yı yine de bağlanamıyorsanız, bkz. [RDP sorun giderme kılavuzu](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). [Ayrıntılı sorun giderme kılavuzu RDP](detailed-troubleshoot-rdp.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) belirli adımları yerine yöntemleri sorun giderme sırasında görünür. Ayrıca [bir Azure destek isteği açın](https://azure.microsoft.com/support/options/) uygulamalı Yardım almak için.

