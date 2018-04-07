---
title: Azure Aracısı olmadan yerel Windows parola sıfırlama | Microsoft Docs
description: Azure Konuk aracısı yüklü olan veya bir VM üzerinde çalışan olmadığında bir yerel Windows kullanıcı hesabı parolasını sıfırlama
services: virtual-machines-windows
documentationcenter: ''
author: iainfoulds
manager: jeconnoc
editor: ''
ms.assetid: cf353dd3-89c9-47f6-a449-f874f0957013
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 01/25/2018
ms.author: iainfou
ms.openlocfilehash: ad892aee646b1a5f8c96d5bdeca24b7a0d88f38e
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
# <a name="reset-local-windows-password-for-azure-vm-offline"></a>Azure VM için çevrimdışı yerel Windows parola sıfırlama
Azure kullanarak bir VM yerel Windows parolasını sıfırlama [Azure portalında veya Azure PowerShell](reset-rdp.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) Azure Konuk aracısı yüklü sağlanan. Bu yöntem, bir Azure VM için bir parola sıfırlama için birincil yoludur. Azure Konuk aracısı yanıt vermiyor ile sorunlarla ya da özel bir görüntü dosyalarını karşıya yükledikten sonra yüklemek başarısız, el ile yapabilecekleriniz Windows parola sıfırlama. Bu makalede kaynak işletim sistemi sanal disk için başka bir VM ekleyerek bir yerel hesap parolasını sıfırlama hakkında ayrıntılar. Bu makalede açıklanan adımları Windows etki alanı denetleyicileri için geçerli değildir. 

> [!WARNING]
> Yalnızca bu işlemi son çare olarak kullanın. Kullanarak bir parolayı sıfırlamak her zaman deneyin [Azure portalında veya Azure PowerShell](reset-rdp.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) ilk.
> 
> 

## <a name="overview-of-the-process"></a>İşlemine genel bakış
Azure Konuk aracısı için erişimi olmadığında sıfırlama Azure Windows VM için yerel bir parolası gerçekleştirmek için temel adımlar aşağıdaki gibidir:

* Kaynak VM silin. Sanal diskler korunur.
* Kaynak sanal makinenin işletim sistemi diski, Azure aboneliğinizin aynı konumdaki başka bir VM iliştirin. Bu VM sorun giderme VM olarak adlandırılır.
* Sorun giderme VM kullanarak, bazı yapılandırma dosyalarını kaynağı VM'ın işletim sistemi diski oluşturun.
* Sorun giderme VM VM'ın işletim sistemi diski kullanımdan çıkarın.
* Özgün sanal diski kullanarak bir VM oluşturmak için Resource Manager şablonunu kullanın.
* Yeni VM önyüklendiğinde, oluşturduğunuz yapılandırma dosyalarını gerekli kullanıcı parolasını güncelleştirin.

## <a name="detailed-steps"></a>Ayrıntılı adımlar

> [!NOTE]
> Adımlar Windows etki alanı denetleyicileri için geçerli değildir. Yalnızca tek başına sunucu veya bir etki alanının üyesi olan bir sunucu üzerinde çalışır.
> 
> 

Kullanarak bir parolayı sıfırlamak her zaman deneyin [Azure portalında veya Azure PowerShell](reset-rdp.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) aşağıdaki adımları denemeden önce. Başlamadan önce VM yedeği olduğundan emin olun. 

1. Azure portalında etkilenen VM silin. VM silme yalnızca VM içinden Azure başvuru meta verileri siler. VM silindiğinde sanal diskleri korunur:
   
   * Azure portalında VM seçin, *silmek*:
     
     ![Var olan VM silme](./media/reset-local-password-without-agent/delete_vm.png)
2. Kaynak sanal makinenin işletim sistemi diski sorun giderme VM'e ekleyin. Sorun giderme VM kaynak sanal makinenin işletim sistemi diski ile aynı bölgede olması gerekir (gibi `West US`):
   
   * Sorun giderme VM Azure portalında seçin. Tıklatın *diskleri* | *Attach varolan*:
     
     ![Varolan bir diski kullanıma açın](./media/reset-local-password-without-agent/disks_attach_existing.png)
     
     Seçin *VHD dosyasını* ve kaynağınız VM içeren depolama hesabı seçin:
     
     ![Depolama hesabı seçme](./media/reset-local-password-without-agent/disks_select_storageaccount.PNG)
     
     Kaynak kapsayıcısı seçin. Kaynak genellikle kapsayıcıdır *VHD'ler*:
     
     ![Depolama kapsayıcısı seçin](./media/reset-local-password-without-agent/disks_select_container.png)
     
     İşletim sistemi VHD'yi eklemek için seçin. Tıklatın *seçin* işlemi tamamlamak için:
     
     ![Kaynak sanal disk seçin](./media/reset-local-password-without-agent/disks_select_source_vhd.png)
3. Uzak Masaüstü kullanarak sorun giderme VM'ye bağlanın ve kaynak sanal makinenin işletim sistemi diski görünür olduğundan emin olun:
   
   * Azure portalında sorun giderme VM seçin ve tıklatın *Bağlan*.
   * Yüklemeleri için RDP dosyasını açın. Kullanıcı adı ve sorun giderme VM parolasını girin.
   * Dosya Gezgini'nde, bağlı veri diski için bakın. Kaynak sanal makinenin VHD sorun giderme VM'ye ekli veriler yalnızca disk ise, F: sürücü olmalıdır:
     
     ![Bağlı veri diski görüntüleme](./media/reset-local-password-without-agent/troubleshooting_vm_fileexplorer.png)
4. Oluşturma `gpt.ini` içinde `\Windows\System32\GroupPolicy` kaynak VM sürücüsündeki (gpt.ini.bak için yeniden adlandırın gpt.ini varsa):
   
   > [!WARNING]
   > Yanlışlıkla aşağıdaki dosyaları C:\Windows, sorun giderme VM için işletim sistemi sürücüsü içinde oluşturmazsanız olduğundan emin olun. Kaynağınız bir veri diski bağlı olduğu VM için işletim sistemi sürücüsündeki aşağıdaki dosyaları oluşturur.
   > 
   > 
   
   * İçine aşağıdaki satırları ekleyin `gpt.ini` oluşturduğunuz dosyası:
     
     ```
     [General]
     gPCFunctionalityVersion=2
     gPCMachineExtensionNames=[{42B5FAAE-6536-11D2-AE5A-0000F87571E3}{40B6664F-4972-11D1-A7CA-0000F87571E3}]
     Version=1
     ```
     
     ![Create gpt.ini](./media/reset-local-password-without-agent/create_gpt_ini.png)
5. Oluşturma `scripts.ini` içinde `\Windows\System32\GroupPolicy\Machine\Scripts`. Gizli klasörlere gösterilen emin olun. Gerekirse, oluşturma `Machine` veya `Scripts` klasörler.
   
   * Aşağıdaki satırları ekleyin `scripts.ini` oluşturduğunuz dosyası:
     
     ```
     [Startup]
     0CmdLine=C:\Windows\System32\FixAzureVM.cmd
     0Parameters=
     ```
     
     ![Scripts.ini oluşturma](./media/reset-local-password-without-agent/create_scripts_ini.png)
6. Oluşturma `FixAzureVM.cmd` içinde `\Windows\System32` değiştirerek aşağıdaki içeriğe sahip `<username>` ve `<newpassword>` kendi değerlerinizi ile:
   
    ```
    net user <username> <newpassword> /add
    net localgroup administrators <username> /add
    net localgroup "remote desktop users" <username> /add
    ```

    ![Create FixAzureVM.cmd](./media/reset-local-password-without-agent/create_fixazure_cmd.png)
   
    Yeni parola tanımlarken, VM için yapılandırılmış parola karmaşıklığı gereksinimlerini karşılamalıdır.
7. Azure Portalı'nda, sorun giderme VM diski kullanımdan çıkarın:
   
   * Azure portalında sorun giderme VM seçin, *diskleri*.
   * Veri diski, 2. adımda eklenen seçin *ayırma*:
     
     ![Diski kullanımdan çıkar](./media/reset-local-password-without-agent/detach_disk.png)
8. Bir VM oluşturmadan önce kaynak işletim sistemi diski için URI alın:
   
   * Azure Portal'da depolama hesabı seçin, *BLOB'lar*.
   * Kapsayıcıyı seçin. Kaynak genellikle kapsayıcıdır *VHD'ler*:
     
     ![Depolama hesabı blob seçin](./media/reset-local-password-without-agent/select_storage_details.png)
     
     Kaynağınız VM OS VHD tıklatıp *kopya* düğmesine *URL* adı:
     
     ![Kopya disk URI](./media/reset-local-password-without-agent/copy_source_vhd_uri.png)
9. Kaynak sanal makinenin işletim sistemi disk, bir VM oluşturun:
   
   * Kullanım [bu Azure Resource Manager şablonu](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-specialized-vhd) özelleştirilmiş bir VHD'den bir VM oluşturmak için. Tıklatın `Deploy to Azure` sizin için doldurulur şablonlu ayrıntılarla Azure Portalı'nı açmak için düğmeye.
   * VM için tüm önceki ayarları korumak isterseniz, seçin *Düzen şablonu* varolan sanal ağ, alt ağ, ağ bağdaştırıcısı veya genel IP sağlamak için.
   * İçinde `OSDISKVHDURI` parametresi metin kutusu, VHD Kaynak URI önceki adımda elde Yapıştır:
     
     ![Bir VM şablonu oluşturma](./media/reset-local-password-without-agent/create_new_vm_from_template.png)
10. Yeni VM çalıştırdıktan sonra belirttiğiniz yeni parola ile Uzak Masaüstü kullanarak VM bağlanmak `FixAzureVM.cmd` komut dosyası.
11. Yeni VM, uzak oturumunuzda, ortamını temizleyecek aşağıdaki dosyaları kaldırın:
    
    * %Windir%\System32
      * remove FixAzureVM.cmd
    * From %windir%\System32\GroupPolicy\Machine\
      * remove scripts.ini
    * %Windir%\System32\GroupPolicy
      * (önce gpt.ini var ve gpt.ini.bak, .bak dosyası için gpt.ini geri yeniden adlandırma için adlandırdığınız varsa) GPT.ini Kaldır

## <a name="next-steps"></a>Sonraki adımlar
Uzak Masaüstü kullanarak hala bağlanamazsa, bkz: [RDP sorun giderme kılavuzu](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). [Sorun giderme kılavuzu RDP ayrıntılı](detailed-troubleshoot-rdp.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) belirli adımları yerine yöntemleri sorun giderme sırasında arar. Ayrıca [Azure destek isteği açma](https://azure.microsoft.com/support/options/) uygulamalı Yardım için.

