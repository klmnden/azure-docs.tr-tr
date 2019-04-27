---
title: Azure PowerShell (Klasik) bir SQL Server sanal makinesi oluşturun | Microsoft Docs
description: SQL Server sanal makine galeri görüntüleri ile Azure VM oluşturmak için adımları ve PowerShell betiklerini sağlar. Bu konuda, Klasik dağıtım modunu kullanır.
services: virtual-machines-windows
documentationcenter: na
author: MashaMSFT
manager: craigg
tags: azure-service-management
ms.assetid: b73be387-9323-4e08-be53-6e5928e3786e
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 08/07/2017
ms.author: mathoma
ms.reviewer: jroth
ms.openlocfilehash: ad8b59a9290c533a3687b5ff8956d8682fb6d9e9
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60607828"
---
# <a name="provision-a-sql-server-virtual-machine-using-azure-powershell-classic"></a>Azure PowerShell (Klasik) kullanarak bir SQL Server sanal makinesi sağlama

Bu makalede PowerShell cmdlet'lerini kullanarak Azure'da bir SQL Server sanal makinesi oluşturmak adımlar sağlar.

> [!IMPORTANT] 
> Azure'da oluşturmaya ve kaynaklarla çalışmaya yönelik iki farklı dağıtım modeli vardır: [Resource Manager ve klasik](../../../azure-resource-manager/resource-manager-deployment-model.md). Bu makalede, Klasik dağıtım modelini incelemektedir. Microsoft, yeni dağıtımların çoğunun Resource Manager modelini kullanmasını önerir.

Bu konuda Resource Manager sürümü için bkz: [Azure PowerShell Resource Manager'ı kullanarak bir SQL Server sanal makinesi sağlama](../sql/virtual-machines-windows-ps-sql-create.md).

### <a name="install-and-configure-powershell"></a>PowerShell'i yükleme ve yapılandırma:
1. Bir Azure hesabınız yoksa, [Azure ücretsiz deneme](https://azure.microsoft.com/pricing/free-trial/)yi ziyaret edin.
2. [En son Azure PowerShell komutlarını yükleyip](/powershell/azure/overview).
3. Windows PowerShell'i başlatın ve Azure aboneliğinize bağlayın **Add-AzureAccount** komutu.

   ```powershell
   Add-AzureAccount
   ```

## <a name="determine-your-target-azure-region"></a>Hedef Azure bölgeniz belirleme

SQL Server sanal makinenizi, belirli bir Azure bölgesinde bulunan bir bulut hizmetinde barındırılacak. Aşağıdaki adımlar, bölge, depolama hesabı belirlemeye yardımcı olması ve bu öğreticinin geri kalanını için kullanılacak olan bulut.

1. SQL Server VM'nize barındırmak için kullanmak istediğiniz veri merkezini belirler. Aşağıdaki PowerShell komutu kullanılabilir bölge adlarının listesini görüntüler.

   ```powershell
   (Get-AzureLocation).Name
   ```

2. Tercih edilen konumunuz belirledikten sonra adlı bir değişken ayarlamak **$dcLocation** bu bölge için. Örneğin, aşağıdaki komut, "Doğu ABD" bölgeye ayarlar:

   ```powershell
   $dcLocation = "East US"
   ```

## <a name="set-your-subscription-and-storage-account"></a>Abonelik ve depolama hesabınızı ayarlama

1. Yeni sanal makine için kullanacağınız Azure aboneliğini belirleyin.

   ```powershell
   (Get-AzureSubscription).SubscriptionName
   ```

2. Hedef Azure aboneliğinin atamak için **$subscr** değişkeni. Ardından bunu geçerli Azure aboneliğiniz ayarlayın.

   ```powershell
   $subscr="<subscription name>"
   Select-AzureSubscription -SubscriptionName $subscr –Current
   ```

3. Ardından, var olan depolama hesapları için olup olmadığını denetleyin. Aşağıdaki betik, seçili bölgesinde mevcut tüm depolama hesapları görüntüler:

   ```powershell
   (Get-AzureStorageAccount | where { $_.GeoPrimaryLocation -eq $dcLocation }).StorageAccountName
   ```

   > [!NOTE]
   > Yeni bir depolama hesabı gerekiyorsa, aşağıdaki örnekte olduğu gibi New-AzureStorageAccount komutu ilk küçük tüm depolama hesap adı oluşturun: `New-AzureStorageAccount -StorageAccountName "<storage account name>" -Location $dcLocation`

4. Hedef depolama hesabı adına atama **$staccount**. Ardından **Set-AzureSubscription** geçerli depolama hesabı ve aboneliği ayarlamak için.

   ```powershell
   $staccount="<storage account name>"
   Set-AzureSubscription -SubscriptionName $subscr -CurrentStorageAccountName $staccount
   ```

## <a name="select-a-sql-server-virtual-machine-image"></a>SQL Server sanal makine görüntüsü seçin

1. Kullanılabilir SQL Server sanal makineler görüntü galerisinden listesini edinin. Tüm bu görüntüleri bir **ImageFamily** "SQL" ile başlayan bir özelliği. Aşağıdaki sorgu kullanılabilir görüntü ailesi, SQL Server'ın önceden yüklenmiş olması görüntüler.

   ```powershell
   Get-AzureVMImage | where { $_.ImageFamily -like "SQL*" } | select ImageFamily -Unique | Sort-Object -Property ImageFamily
   ```

2. Sanal makine görüntüsü ailesi bulduğunuzda, bu iş ailesindeki birden çok yayımlanmış görüntülerin olabilir. Seçilen görüntü ailenize en son yayımlanan sanal makine görüntü adını bulmak için aşağıdaki betiği kullanın (gibi **Windows Server 2012 R2'de SQL Server 2016 RTM Enterprise**):

   ```powershell
   $family="<ImageFamily value>"
   $image=Get-AzureVMImage | where { $_.ImageFamily -eq $family } | sort PublishedDate -Descending | select -ExpandProperty ImageName -First 1

   echo "Selected SQL Server image name:"
   echo "   $image"
   ```

## <a name="create-the-virtual-machine"></a>Sanal makineyi oluşturma

Son olarak, PowerShell ile sanal makine oluşturun:

1. Yeni VM'yi barındırmak için bir bulut hizmeti oluşturun. Bu da mevcut bir bulut hizmetini kullanmayı mümkün olduğunu unutmayın. Yeni değişken oluşturma **$svcname** kısa bulut hizmeti adı.

   ```powershell
   $svcname = "<cloud service name>"
   New-AzureService -ServiceName $svcname -Label $svcname -Location $dcLocation
   ```

2. Sanal makine adı ve bir boyut belirtin. Sanal makine boyutları hakkında daha fazla bilgi için bkz. [Azure için sanal makine boyutlarını](../sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

   ```powershell
   $vmname="<machine name>"
   $vmsize="<Specify one: Large, ExtraLarge, A5, A6, A7, A8, A9, or see the link to the other VM sizes>"
   $vm1=New-AzureVMConfig -Name $vmname -InstanceSize $vmsize -ImageName $image
   ```

3. Yerel yönetici hesabı ve parolayı belirtin.

   ```powershell
   $cred=Get-Credential -Message "Type the name and password of the local administrator account."
   $vm1 | Add-AzureProvisioningConfig -Windows -AdminUsername $cred.GetNetworkCredential().Username -Password $cred.GetNetworkCredential().Password
   ```

4. Sanal makine oluşturmak için aşağıdaki betiği çalıştırın.

   ```powershell
   New-AzureVM –ServiceName $svcname -VMs $vm1
   ```

> [!NOTE]
> Ek açıklama ve yapılandırma seçenekleri için bkz: **komut kümenizi oluşturmak** konusundaki [oluşturma ve önceden yapılandırma Windows tabanlı sanal makineler için Azure PowerShell kullanarak](../classic/create-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

## <a name="example-powershell-script"></a>Örnek PowerShell Betiği

Aşağıdaki komut dosyasını oluşturur tam bir komut dosyası bir örnek sağlar bir **Windows Server 2012 R2'de SQL Server 2016 RTM Enterprise** sanal makine. Bu komut dosyası kullanırsanız, bu konu başlığı altında önceki adımlarda göre ilk değişkenlerini özelleştirmeniz gerekir.

```powershell
# Customize these variables based on your settings and requirements:
$dcLocation = "East US"
$subscr="mysubscription"
$staccount="mystorageaccount"
$family="SQL Server 2016 RTM Enterprise on Windows Server 2012 R2"
$svcname = "mycloudservice"
$vmname="myvirtualmachine"
$vmsize="A5"

# Set the current subscription and storage account
# Comment out the New-AzureStorageAccount line if the account already exists
Select-AzureSubscription -SubscriptionName $subscr –Current
New-AzureStorageAccount -StorageAccountName $staccount -Location $dcLocation
Set-AzureSubscription -SubscriptionName $subscr -CurrentStorageAccountName $staccount

# Select the most recent VM image in this image family:
$image=Get-AzureVMImage | where { $_.ImageFamily -eq $family } | sort PublishedDate -Descending | select -ExpandProperty ImageName -First 1

# Create the new cloud service; comment out this line if cloud service exists already:
New-AzureService -ServiceName $svcname -Label $svcname -Location $dcLocation

# Create the VM config:
$vm1=New-AzureVMConfig -Name $vmname -InstanceSize $vmsize -ImageName $image

# Set administrator credentials:
$cred=Get-Credential -Message "Type the name and password of the local administrator account."
$vm1 | Add-AzureProvisioningConfig -Windows -AdminUsername $cred.GetNetworkCredential().Username -Password $cred.GetNetworkCredential().Password

# Create the SQL Server VM:
New-AzureVM –ServiceName $svcname -VMs $vm1
```

## <a name="connect-with-remote-desktop"></a>Uzak Masaüstü ile bağlanın

1. RDP dosyalarını Kurulumu tamamlamak için bu sanal makineler başlatmak için geçerli kullanıcının belge klasörü oluşturun:

   ```powershell
   $documentspath = [environment]::getfolderpath("mydocuments")
   Get-AzureRemoteDesktopFile -ServiceName $svcname -Name $vmname -LocalPath "$documentspath\vm1.rdp"
   ```

2. Belge dizinde RDP dosyasını başlatın. Yönetici kullanıcı adı ve parola daha önce sağlanan bağlantı (örneğin, kullanıcı adınızı VMAdmin olduysa, "\VMAdmin" kullanıcı olarak belirtin ve parola).

   ```powershell
   cd $documentspath
   .\vm1.rdp
   ```

## <a name="complete-the-configuration-of-the-sql-server-machine-for-remote-access"></a>Uzaktan erişim için SQL Server makinesine yapılandırmasını tamamlama

Makineyle bir Uzak Masaüstü'nü açın oturum açtıktan sonra SQL Server'ın yönergelerini temel yapılandırma [Azure VM'deki SQL Server bağlantısı yapılandırma adımları](virtual-machines-windows-classic-sql-connect.md#steps-for-configuring-sql-server-connectivity-in-an-azure-vm).

## <a name="next-steps"></a>Sonraki adımlar

PowerShell ile sanal makineler sağlamak için ek yönergeler bulabilirsiniz [sanal makineler belgeleri](../classic/create-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

Çoğu durumda, bu yeni bir SQL Server VM veritabanlarınızı geçirme sonraki adımdır. Veritabanı Geçiş Kılavuzu için bkz. [bir veritabanını Azure VM'deki SQL Server'a geçirme](../sql/virtual-machines-windows-migrate-sql.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fsqlclassic%2ftoc.json).

Ayrıca SQL sanal makineleri oluşturmak için Azure portalını kullanarak konusu ilginizi çekiyorsa [azure'da bir SQL Server sanal makinesi sağlama](../sql/virtual-machines-windows-portal-sql-server-provision.md). Portal üzerinden anlatan öğretici PowerShell Bu konuda kullanılan Klasik modeli yerine önerilen Resource Manager modeli kullanarak VM'ler oluşturur unutmayın.

Bu kaynaklara ek olarak, gözden geçirmenizi öneririz [ilgili diğer konular SQL Server Azure sanal Makineler'de çalışan](../sql/virtual-machines-windows-sql-server-iaas-overview.md).
