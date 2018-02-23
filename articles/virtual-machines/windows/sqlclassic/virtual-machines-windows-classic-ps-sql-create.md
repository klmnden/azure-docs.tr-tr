---
title: "Azure PowerShell'de (Klasik) bir SQL Server sanal makine oluşturun | Microsoft Docs"
description: "Bir Azure VM ile SQL Server sanal makineye Galerisi görüntüleri oluşturmak için adımlar ve PowerShell komut dosyaları sağlar. Bu konu, Klasik dağıtım modunu kullanır."
services: virtual-machines-windows
documentationcenter: na
author: rothja
manager: craigg
tags: azure-service-management
ms.assetid: b73be387-9323-4e08-be53-6e5928e3786e
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 08/07/2017
ms.author: jroth
ms.openlocfilehash: 66f44e27562f33373e0b67fe6e0ebf9c6bf99e03
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/21/2018
---
# <a name="provision-a-sql-server-virtual-machine-using-azure-powershell-classic"></a>Azure PowerShell (Klasik) kullanarak bir SQL Server sanal makine sağlama

Bu makalede PowerShell cmdlet'lerini kullanarak Azure'da bir SQL Server sanal makine oluşturmak adımlar sağlar.

> [!IMPORTANT] 
> Azure oluşturmak ve kaynaklarla çalışmak için iki farklı dağıtım modeli vardır: [Resource Manager ve klasik](../../../azure-resource-manager/resource-manager-deployment-model.md). Bu makalede, Klasik dağıtım modeli kullanarak yer almaktadır. Microsoft, yeni dağıtımların çoğunun Resource Manager modelini kullanmasını önerir.

Bu konuda Resource Manager sürümü için bkz: [Azure PowerShell Kaynak Yöneticisi'ni kullanarak bir SQL Server sanal makine sağlama](../sql/virtual-machines-windows-ps-sql-create.md).

### <a name="install-and-configure-powershell"></a>PowerShell'i yükleme ve yapılandırma:
1. Bir Azure hesabınız yoksa, [Azure ücretsiz deneme](https://azure.microsoft.com/pricing/free-trial/)yi ziyaret edin.
2. [En son Azure PowerShell komutlarını yükleyip](/powershell/azure/overview).
3. Windows PowerShell'i başlatın ve Azure aboneliğinizle bağlanmak **Add-AzureAccount** komutu.

   ```powershell
   Add-AzureAccount
   ```

## <a name="determine-your-target-azure-region"></a>Azure bölgesi hedef belirleme

SQL Server sanal makineniz, belirli bir Azure bölgesinde bulunan bir bulut hizmetinde barındırılan. Aşağıdaki adımlar bölge, depolama hesabı belirlemeye yardımcı olmak ve bulut öğreticinin geri kalanını için kullanılacak hizmeti.

1. SQL Server VM'nize barındırmak için kullanmak istediğiniz veri merkezini belirler. Aşağıdaki PowerShell komutunu kullanılabilir bölge adlarının bir listesini görüntüler.

   ```powershell
   (Get-AzureLocation).Name
   ```

2. Tercih edilen konumunuz tanımladıktan sonra adlı bir değişken ayarlamak **$dcLocation** bu bölge için. Örneğin, aşağıdaki komut, "Doğu ABD" bölgesine ayarlar:

   ```powershell
   $dcLocation = "East US"
   ```

## <a name="set-your-subscription-and-storage-account"></a>Abonelik ve depolama hesabınızı ayarlama

1. Yeni sanal makine için kullanacağınız Azure aboneliği belirler.

   ```powershell
   (Get-AzureSubscription).SubscriptionName
   ```

2. Azure aboneliği hedef atamak için **$subscr** değişkeni. Sonra geçerli bir Azure aboneliğinizin ayarlayın.

   ```powershell
   $subscr="<subscription name>"
   Select-AzureSubscription -SubscriptionName $subscr –Current
   ```

3. Var olan depolama hesapları için denetleyin. Aşağıdaki komut dosyası, seçilen bölgenizde mevcut tüm depolama hesapları görüntüler:

   ```powershell
   (Get-AzureStorageAccount | where { $_.GeoPrimaryLocation -eq $dcLocation }).StorageAccountName
   ```

   > [!NOTE]
   > İlk yeni bir depolama hesabı gerekiyorsa, bir küçük tüm depolama hesabı adı aşağıdaki örnekteki gibi yeni AzureStorageAccount komutuyla oluşturun: `New-AzureStorageAccount -StorageAccountName "<storage account name>" -Location $dcLocation`

4. Hedef depolama hesabı adı atamak **$staccount**. Ardından **Set-AzureSubscription** geçerli depolama hesabı ve aboneliği ayarlamak için.

   ```powershell
   $staccount="<storage account name>"
   Set-AzureSubscription -SubscriptionName $subscr -CurrentStorageAccountName $staccount
   ```

## <a name="select-a-sql-server-virtual-machine-image"></a>Bir SQL Server sanal makine görüntüsü seçin

1. Kullanılabilir SQL Server sanal makine görüntüleri galerisinden listesi öğrenin. Tüm bu görüntüleri bir **ImageFamily** "SQL" ile başlayan özelliği. Aşağıdaki sorgu kullanılabilir görüntü ailesi, SQL Server'ın önceden yüklenmiş olması görüntüler.

   ```powershell
   Get-AzureVMImage | where { $_.ImageFamily -like "SQL*" } | select ImageFamily -Unique | Sort-Object -Property ImageFamily
   ```

2. Sanal makine görüntüsü ailesi bulduğunuzda, bu ailesinde birden fazla yayımlanan görüntü olabilir. Seçilen görüntü ailesinin en son yayımlanan sanal makine görüntü adını bulmak için aşağıdaki komut dosyasını kullanın (gibi **Windows Server 2012 R2'de SQL Server 2016 RTM Enterprise**):

   ```powershell
   $family="<ImageFamily value>"
   $image=Get-AzureVMImage | where { $_.ImageFamily -eq $family } | sort PublishedDate -Descending | select -ExpandProperty ImageName -First 1

   echo "Selected SQL Server image name:"
   echo "   $image"
   ```

## <a name="create-the-virtual-machine"></a>Sanal makineyi oluşturma

Son olarak, PowerShell ile sanal makine oluşturun:

1. Yeni VM barındırmak için bir bulut hizmeti oluşturun. Bu aynı zamanda var olan bir bulut hizmetini kullanmayı mümkün olduğunu unutmayın. Yeni bir değişken oluşturmak **$svcname** bulut hizmeti kısa adı.

   ```powershell
   $svcname = "<cloud service name>"
   New-AzureService -ServiceName $svcname -Label $svcname -Location $dcLocation
   ```

2. Sanal makine adı ve bir boyut belirtin. Sanal makine boyutları hakkında daha fazla bilgi için bkz: [Azure için sanal makine boyutlarını](../sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

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
> Ek açıklama ve yapılandırma seçenekleri için bkz: **komut kümesini yapı** bölümüne [oluşturmak ve Windows tabanlı sanal makineleri önceden için kullanım Azure PowerShell](../classic/create-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

## <a name="example-powershell-script"></a>Örnek PowerShell komut dosyası

Aşağıdaki komut dosyasını oluşturur tam bir komut dosyası örneği sağlar bir **Windows Server 2012 R2'de SQL Server 2016 RTM Enterprise** sanal makine. Bu komut dosyası kullanırsanız, bu konunun önceki adımlarda göre ilk değişkenlerini özelleştirmeniz gerekir.

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

## <a name="connect-with-remote-desktop"></a>İle Uzak Masaüstü Bağlantısı

1. RDP dosyalarını Kurulumu tamamlamak için bu sanal makineleri başlatmak için geçerli kullanıcının belge klasörü oluşturun:

   ```powershell
   $documentspath = [environment]::getfolderpath("mydocuments")
   Get-AzureRemoteDesktopFile -ServiceName $svcname -Name $vmname -LocalPath "$documentspath\vm1.rdp"
   ```

2. Belge dizinde RDP dosyasını çalıştırın. Yönetici kullanıcı adı ve parola daha önce sağlanan bağlantı (örneğin, kullanıcı adınızı VMAdmin olduysa, "\VMAdmin" kullanıcı olarak belirtin ve parola belirtin).

   ```powershell
   cd $documentspath
   .\vm1.rdp
   ```

## <a name="complete-the-configuration-of-the-sql-server-machine-for-remote-access"></a>Uzaktan erişim için SQL Server makine yapılandırmasını tamamlama

Uzak Masaüstü makineyle açın oturum açtıktan sonra SQL Server'ın yönergelere göre yapılandırma [bir Azure VM'de SQL Server bağlantısı yapılandırma adımları](virtual-machines-windows-classic-sql-connect.md#steps-for-configuring-sql-server-connectivity-in-an-azure-vm).

## <a name="next-steps"></a>Sonraki adımlar

PowerShell ile sanal makineleri sağlamaya yönelik ek yönergeler bulabilirsiniz [virtual machines belgeleri](../classic/create-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

Çoğu durumda, sonraki adım, veritabanlarınızı bu yeni bir SQL Server VM geçirmektir. Veritabanı geçiş yönergeleri için bkz [SQL Server'a Azure VM'de bir veritabanını geçirme](../sql/virtual-machines-windows-migrate-sql.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fsqlclassic%2ftoc.json).

Ayrıca SQL sanal makineler oluşturmak için bkz: Azure portalını kullanarak ilginizi çekiyorsa [Azure üzerinde bir SQL Server sanal makine sağlama](../sql/virtual-machines-windows-portal-sql-server-provision.md). Portalı aracılığıyla anlatan öğretici önerilen Resource Manager modeli, yerine bu PowerShell konuda kullanılan Klasik modeli kullanarak VM'ler oluşturur unutmayın.

Bu kaynakların ek olarak, gözden geçirmenizi öneririz [ilgili diğer konular SQL Server Azure sanal makinelerde çalışan için](../sql/virtual-machines-windows-sql-server-iaas-overview.md).
