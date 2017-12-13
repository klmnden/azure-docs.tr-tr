---
title: "Bir VM üzerinde eğilimi mikro derin güvenlik güncelleştirmesini | Microsoft Docs"
description: "Bu makalede, yükleme ve Azure Klasik dağıtım modelinde oluşturulmuş bir VM'de eğilimi mikro güvenlik yapılandırma açıklar."
services: virtual-machines-windows
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: e991b635-f1e2-483f-b7ca-9d53e7c22e2a
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-multiple
ms.devlang: na
ms.topic: article
ms.date: 03/30/2017
ms.author: iainfou
ms.openlocfilehash: 41b7ecf0d0c71b5c225454cc77ce87d5736c2165
ms.sourcegitcommit: a5f16c1e2e0573204581c072cf7d237745ff98dc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
---
# <a name="how-to-install-and-configure-trend-micro-deep-security-as-a-service-on-a-windows-vm"></a>Bir Windows VM’de Hizmet Olarak Trend Micro Deep Security yükleme ve yapılandırma
> [!IMPORTANT]
> Azure oluşturmak ve kaynaklarla çalışmak için iki farklı dağıtım modeli vardır: [Resource Manager ve klasik](../../../resource-manager-deployment-model.md). Bu makalede, Klasik dağıtım modeli kullanarak yer almaktadır. Microsoft, yeni dağıtımların çoğunun Resource Manager modelini kullanmasını önerir.
> [!INCLUDE [virtual-machines-common-classic-createportal](../../../../includes/virtual-machines-classic-portal.md)]

Bu makalede yükleme ve bir yeni veya var olan Windows Server çalıştıran sanal makine (VM) bir hizmet olarak eğilimi mikro derin güvenlik yapılandırma gösterilmektedir. Bir hizmet olarak derin güvenlik kötü amaçlı yazılımdan koruma, bir güvenlik duvarı, bir yetkisiz erişim önleme sisteminin ve bütünlüğü izleme içerir.

İstemci güvenlik uzantısını VM Aracısı üzerinden olarak yüklenir. VM Aracısı otomatik olarak Azure portal tarafından oluşturulan yeni bir sanal makinede derin güvenlik aracısı yükleyin.

Azure portalı, Azure CLI veya PowerShell kullanılarak oluşturulan mevcut bir VM'yi VM Aracısı sahip olmayabilir. Mevcut bir sanal VM aracısı yüklü olmayan makine için indirme ve önce yüklemeniz gerekir. Bu makalede her iki durumlarda kapsar.

Geçerli bir eğilim mikro abonelikten bir şirket içi çözüm varsa, Azure sanal makinelerinizi korunmasına yardımcı olmak için kullanabilirsiniz. Bir müşteri henüz değilseniz, deneme aboneliği için kaydolabilirsiniz. Bu çözüm hakkında daha fazla bilgi için eğilim mikro blog gönderisine bakın [Microsoft Azure VM Aracısı uzantısı için ayrıntılı güvenlik](http://go.microsoft.com/fwlink/p/?LinkId=403945).

## <a name="install-the-deep-security-agent-on-a-new-vm"></a>Yeni bir VM üzerinde derin güvenlik aracısı yükleyin

[Azure portal](http://portal.azure.com) , bir görüntüden kullandığınızda eğilimi mikro güvenlik uzantısı yüklemenize olanak tanıyan **Market** sanal makine oluşturulamıyor. Tek bir sanal makine oluşturuyorsanız, Portalı'nı kullanarak koruma eğilimi mikro eklemek için kolay bir yoludur.

Bir giriş kullanılarak **Market** yardımcı olan bir Sihirbazı'nı Ayarla sanal makineyi açar. Kullandığınız **ayarları** dikey penceresinde, eğilim mikro güvenlik uzantıyı yüklemek için sihirbazın üçüncü Pano.  Genel yönergeler için bkz: [Azure portalında Windows çalıştıran bir sanal makine oluşturma](tutorial.md).

Ulaştığınızda **ayarları** dikey Sihirbazı'nın şu adımları uygulayın:

1. Tıklatın **uzantıları**, ardından **uzantısı Ekle** sonraki bölmesinde.

   ![Uzantı eklemeye başlayın][1]

2. Seçin **derin güvenlik aracısı** içinde **yeni kaynak** bölmesi. Derin güvenlik aracısı bölmesinde **oluşturma**.

   ![Derin güvenlik aracısı tanımlama][2]

3. Girin **Kiracı tanımlayıcı** ve **Kiracı etkinleştirme parola** uzantısı. İsteğe bağlı olarak, girebilirsiniz bir **güvenlik ilkesi tanımlayıcısı**. Ardından **Tamam** istemcisi eklemek için.

   ![Uzantı ayrıntılarını sağlayın][3]

## <a name="install-the-deep-security-agent-on-an-existing-vm"></a>Var olan bir VM üzerinde derin güvenlik aracısı yükleyin
Var olan bir VM Aracısı'nı yüklemek için aşağıdaki öğeleri gerekir:

* Azure PowerShell modülü, sürüm 0.8.2 veya daha yeni, yerel bilgisayarınızda yüklü. Kullanarak yüklediyseniz Azure PowerShell sürümü kontrol edebilirsiniz **Get-Module azure | Tablo Biçimlendir sürüm** komutu. Yönergeler ve en son sürüme bir bağlantı için bkz: [Azure PowerShell'i yükleme ve yapılandırma nasıl](/powershell/azure/overview). Oturum açma kullanarak Azure aboneliği `Add-AzureAccount`.
* VM aracısının hedef sanal makinede yüklü.

İlk olarak, VM Aracısı zaten yüklü olduğunu doğrulayın. Bulut hizmeti adı ve sanal makine adı girin ve ardından bir yönetici düzeyi Azure PowerShell komut isteminde aşağıdaki komutları çalıştırın. Dahil olmak üzere tırnak işaretleri içindeki her şeyi değiştirin < ve > karakterleri.

    $CSName = "<cloud service name>"
    $VMName = "<virtual machine name>"
    $vm = Get-AzureVM -ServiceName $CSName -Name $VMName
    write-host $vm.VM.ProvisionGuestAgent

Bulut hizmeti ve sanal makine adını bilmiyorsanız, çalıştırmak **Get-AzureVM** geçerli aboneliğinizdeki tüm sanal makineler için bu bilgileri görüntülemek için.

Varsa **write-host** komutu döndürür **doğru**, VM aracısının yüklü olduğundan. Döndürürse **False**, yönergeler ve Azure blog postası karşıdan yükleme bağlantısı bkz [VM aracısı ve uzantılar - 2. parça](http://go.microsoft.com/fwlink/p/?LinkId=403947).

VM Aracısı yüklüyse, bu komutları çalıştırın.

    $Agent = Get-AzureVMAvailableExtension TrendMicro.DeepSecurity -ExtensionName TrendMicroDSA

    Set-AzureVMExtension -Publisher TrendMicro.DeepSecurity –Version $Agent.Version -ExtensionName TrendMicroDSA -VM $vm | Update-AzureVM

## <a name="next-steps"></a>Sonraki adımlar
Aracı yüklü olduğunda çalıştıran başlatmak birkaç dakika sürer. Bundan sonra ayrıntılı bir güvenlik yöneticisi tarafından yönetilecek şekilde sanal makinede derin güvenlik etkinleştirmeniz gerekir. Ek yönergeler için aşağıdaki makalelere bakın:

* Bu çözümle ilgili eğilim'ın makale [Instant-On Cloud Security Microsoft Azure](http://go.microsoft.com/fwlink/?LinkId=404101)
* A [örnek Windows PowerShell komut dosyası](http://go.microsoft.com/fwlink/?LinkId=404100) sanal makineyi yapılandırmak için
* [Yönergeler](http://go.microsoft.com/fwlink/?LinkId=404099) örnek

## <a name="additional-resources"></a>Ek kaynaklar
[Windows Server çalıştıran bir sanal makine için oturum açma]

[Azure VM uzantıları ve özellikleri]

<!-- Image references -->
[1]: ./media/install-trend/new_vm_Blade3.png
[2]: ./media/install-trend/find_SecurityAgent.png
[3]: ./media/install-trend/SecurityAgentDetails.png

<!-- Link references -->
[Windows Server çalıştıran bir sanal makine için oturum açma]:connect-logon.md
[Azure VM uzantıları ve özellikleri]: http://go.microsoft.com/fwlink/p/?linkid=390493&clcid=0x409
