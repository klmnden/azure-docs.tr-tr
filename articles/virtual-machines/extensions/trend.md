---
title: Bir VM'ye Trend Micro Deep Security'yi yükleme | Microsoft Docs
description: Bu makalede, yükleme ve Azure Klasik dağıtım modelinde oluşturulan bir VM'de Trend Micro güvenlik yapılandırma açıklar.
services: virtual-machines-windows
documentationcenter: ''
author: roiyz-msft
manager: jeconnoc
editor: ''
tags: azure-service-management
ms.assetid: e991b635-f1e2-483f-b7ca-9d53e7c22e2a
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-multiple
ms.devlang: na
ms.topic: article
ms.date: 04/20/2018
ms.author: roiyz
ms.openlocfilehash: d7808fbff0199105a12c0570807876413d5c98c6
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60800117"
---
# <a name="how-to-install-and-configure-trend-micro-deep-security-as-a-service-on-a-windows-vm"></a>Bir Windows VM’de Hizmet Olarak Trend Micro Deep Security yükleme ve yapılandırma
[!INCLUDE [virtual-machines-extensions-deprecation-statement](../../../includes/virtual-machines-extensions-deprecation-statement.md)]
Bu makalede yükleme ve bir yeni veya var olan Windows Server çalıştıran sanal makine (VM) bir hizmet olarak Trend Micro Deep Security yapılandırma gösterilmektedir. Hizmet olarak DEEP Security'yi kötü amaçlı yazılımdan koruma, bir güvenlik duvarı, bir yetkisiz erişim önleme sistemine ve bütünlüğünü izleme içerir.

İstemci, VM Aracısı aracılığıyla güvenlik uzantısı yüklenir. Yeni bir sanal makinede VM Aracısı otomatik olarak Azure portal tarafından oluşturulan Deep Security Agent yükleyin.

Azure CLI veya PowerShell Azure portalını kullanarak oluşturduğunuz var olan bir VM'yi VM Aracısı olmayabilir. Mevcut bir sanal VM aracısı yüklü olmayan makine için indirip önce yüklemeniz gerekir. Bu makalede her iki durumları kapsar.

Trend Micro geçerli bir abonelik için bir şirket içi çözüm varsa, Azure sanal makinelerinizin korunmasına yardımcı olmak için kullanabilirsiniz. Bir müşteri henüz değilseniz, bir deneme aboneliğine kaydolabilirsiniz. Bu çözüm hakkında daha fazla bilgi için Trend Micro yazısına bakın [Microsoft Azure VM Aracısı uzantısı için Deep Security](https://go.microsoft.com/fwlink/p/?LinkId=403945).

## <a name="install-the-deep-security-agent-on-a-new-vm"></a>Yeni bir sanal makine üzerinde Deep Security Agent'ı yükleyin

[Azure portalında](https://portal.azure.com) , Trend Micro güvenlik uzantıyı görüntüden kullandığınızda yüklemenize imkan tanır **Market** sanal makine oluşturmak için. Tek bir sanal makine oluşturuyorsanız, portalı kullanarak koruma için Trend Micro eklemek için kolay bir yoludur.

Bir giriş kullanarak **Market** açılır yardımcı olacak bir sihirbaz sanal Makine'yi ayarlayın. Kullandığınız **ayarları** dikey penceresinde, Trend Micro güvenlik uzantıyı yüklemek için sihirbazın üçüncü Pano.  Genel yönergeler için bkz: [Azure Portal'da Windows çalıştıran bir sanal makine oluşturma](../windows/classic/tutorial.md).

Ulaştığınızda **ayarları** Sihirbazı'nın dikey penceresinde aşağıdaki adımları uygulayın:

1. Tıklayın **uzantıları**, ardından **uzantısı ekleme** sonraki bölmesinde.

   ![Uzantı eklemeye başlayın][1]

2. Seçin **Deep Security Agent** içinde **yeni kaynak** bölmesi. Deep Security Agent bölmesinden **Oluştur**.

   ![DEEP Security Agent tanımlayın][2]

3. Girin **Kiracı tanımlayıcısı** ve **Kiracı etkinleştirme parolası** uzantısı. İsteğe bağlı olarak, girdiğiniz bir **güvenlik ilkesi tanımlayıcısı**. ' A tıklayarak **Tamam** istemcisi eklemek için.

   ![Uzantı ayrıntılarını sağlayın][3]

## <a name="install-the-deep-security-agent-on-an-existing-vm"></a>Deep Security Agent mevcut bir VM'ye yükleyin
Mevcut bir VM aracısını yüklemek için aşağıdaki öğeler gerekir:

* Azure PowerShell modülü, sürüm 0.8.2 veya yeni, yerel bilgisayarınızda yüklü. Azure PowerShell kullanarak yüklü sürümü denetleyebilirsiniz **Get-Module azure | format-table sürüm** komutu. Yönergeler ve en son sürüme bir bağlantı için bkz. [Azure PowerShell'i yükleme ve yapılandırma işlemini](/powershell/azure/overview). Kullanarak Azure aboneliği için oturum açın `Add-AzureAccount`.
* VM aracısının hedef sanal makinede yüklü.

İlk olarak, VM Aracısı zaten yüklü olduğunu doğrulayın. Bulut hizmeti adı ve sanal makine adı girin ve ardından yönetici düzeyinde Azure PowerShell komut isteminde aşağıdaki komutları çalıştırın. Dahil olmak üzere, tırnak işaretleri içindeki her şeyi değiştirin < ve > karakterleri.

    $CSName = "<cloud service name>"
    $VMName = "<virtual machine name>"
    $vm = Get-AzureVM -ServiceName $CSName -Name $VMName
    write-host $vm.VM.ProvisionGuestAgent

Bulut hizmeti ve sanal makine adını bilmiyorsanız, çalıştırma **Get-AzureVM** geçerli aboneliğinizdeki tüm sanal makineler için bu bilgileri görüntülemek için.

Varsa **write-host** komutu tarafından döndürülen **True**, VM Aracısı yüklenir. Döndürürse **False**, yönergeleri ve Azure Web günlüğü gönderisinde indirme bağlantısını [VM aracısı ve uzantıları - 2. bölüm](https://go.microsoft.com/fwlink/p/?LinkId=403947).

VM aracısı yüklü değilse şu komutları çalıştırın.

    $Agent = Get-AzureVMAvailableExtension TrendMicro.DeepSecurity -ExtensionName TrendMicroDSA

    Set-AzureVMExtension -Publisher TrendMicro.DeepSecurity –Version $Agent.Version -ExtensionName TrendMicroDSA -VM $vm | Update-AzureVM

## <a name="next-steps"></a>Sonraki adımlar
Aracı yüklendiğinde çalıştırmaya başlamak birkaç dakika sürer. Bundan sonra bir Deep Security Manager tarafından yönetilebilmesi Deep Security sanal makine üzerinde etkinleştirmeniz gerekir. Ek yönergeler için şu makalelere bakın:

* Bu çözüm, ilgili eğilim'ın makale [Instant-On Cloud Security için Microsoft Azure](https://go.microsoft.com/fwlink/?LinkId=404101)
* A [örnek Windows PowerShell komut dosyası](https://go.microsoft.com/fwlink/?LinkId=404100) sanal makineyi yapılandırmak için
* [Yönergeler](https://go.microsoft.com/fwlink/?LinkId=404099) örnek

## <a name="additional-resources"></a>Ek kaynaklar
[Windows Server çalıştıran bir sanal makine için oturum açma]

[Azure VM uzantıları ve özellikleri]

<!-- Image references -->
[1]: ./media/trend/new_vm_Blade3.png
[2]: ./media/trend/find_SecurityAgent.png
[3]: ./media/trend/SecurityAgentDetails.png

<!-- Link references -->
[Windows Server çalıştıran bir sanal makine için oturum açma]:../windows/classic/connect-logon.md
[Azure VM uzantıları ve özellikleri]: https://go.microsoft.com/fwlink/p/?linkid=390493&clcid=0x409
