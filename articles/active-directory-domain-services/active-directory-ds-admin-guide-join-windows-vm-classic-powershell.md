---
title: "Azure Active Directory etki alanı Hizmetleri: Yönetim Kılavuzu | Microsoft Docs"
description: "Windows sanal makinesi Azure PowerShell ve klasik dağıtım modeli kullanarak bir yönetilen etki alanına ekleyin."
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: stevenpo
editor: curtand
ms.assetid: 9143b843-7327-43c3-baab-6e24a18db25e
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/06/2017
ms.author: maheshu
ms.openlocfilehash: 1bb1546fb616131a1e1868a0d0610c4cad5d73e2
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="join-a-windows-server-virtual-machine-to-a-managed-domain-using-powershell"></a>Bir Windows Server sanal makine PowerShell kullanarak bir yönetilen etki alanına ekleyin
> [!div class="op_single_selector"]
> * [Klasik Azure portalı - Windows](active-directory-ds-admin-guide-join-windows-vm.md)
> * [PowerShell - Windows](active-directory-ds-admin-guide-join-windows-vm-classic-powershell.md)
>
>

<br>

> [!IMPORTANT]
> Azure’da kaynak oluşturmak ve bunlarla çalışmak için iki farklı dağıtım modeli vardır:  [Resource Manager ve klasik](../azure-resource-manager/resource-manager-deployment-model.md). Bu makale klasik dağıtım modelini incelemektedir. Azure AD etki alanı Hizmetleri, Resource Manager modeli şu anda desteklemiyor.
>
>

Bu adımlar oluşturmak ve bir yapı bloğu yaklaşımını kullanarak Windows tabanlı bir Azure sanal makine önceden Azure PowerShell komut kümesini özelleştirmek nasıl gösterir. Bu adımları Windows tabanlı bir Azure sanal makine oluşturun ve bir Azure AD etki alanı Hizmetleri yönetilen etki alanına katılmak yardımcı olur.

Bir doldurma-arada boşlukları yaklaşım Azure PowerShell komut kümelerini oluşturmak için aşağıdaki adımları izleyin. Bu yaklaşım, PowerShell için yeni olan veya ne başarılı yapılandırmayı belirtmek için değerleri bilmek istiyorsanız yararlı olabilir. Gelişmiş PowerShell kullanıcılar, komutları almak ve ("$" ile başlayan satırlar) değişkenleri için kendi değerlerinizi yerleştirin.

Henüz yapmadıysanız, konusundaki yönergeleri kullanın [Azure PowerShell'i yükleme ve yapılandırma nasıl](/powershell/azure/overview) yerel bilgisayarınıza Azure PowerShell'i yüklemek için. Sonra bir Windows PowerShell komut istemi açın.

## <a name="step-1-add-your-account"></a>1. adım: hesabınızı ekleme
1. PowerShell komut isteminde yazın **Add-AzureAccount** tıklatıp **Enter**.
2. Azure aboneliğinizle ilişkili e-posta adresini yazın ve tıklatın **devam**.
3. Hesabınızın parolasını yazın.
4. Tıklatın **oturum**.

## <a name="step-2-set-your-subscription-and-storage-account"></a>2. adım: aboneliğiniz ve depolama hesabı ayarlama
Azure aboneliği ve depolama hesabı Windows PowerShell komut isteminde şu komutları çalıştırarak ayarlayın. Dahil olmak üzere tırnak işaretleri içindeki her şeyi değiştirin < ve > karakterleri doğru adlara sahip.

    $subscr="<subscription name>"
    $staccount="<storage account name>"
    Select-AzureSubscription -SubscriptionName $subscr –Current
    Set-AzureSubscription -SubscriptionName $subscr -CurrentStorageAccountName $staccount

Doğru abonelik adını çıktısını varlığıyla SubscriptionName özelliğinden alabilirsiniz **Get-AzureSubscription** komutu. Doğru depolama hesabı adı çıktısını etiket özelliğinden alabilirsiniz **Get-AzureStorageAccount** çalıştırdıktan sonra komut **Select-AzureSubscription** komutu.

## <a name="step-3-step-by-step-walkthrough---provision-the-virtual-machine-and-join-it-to-the-managed-domain"></a>Adım 3: Adım adım - sanal makine sağlama ve yönetilen bir etki alanına katılma
Okunabilirlik için her bloğu arasında boş satırlar ile bu sanal makine oluşturmak için karşılık gelen Azure PowerShell komutunu aşağıda verilmiştir.

Sağlanacak Windows sanal makine hakkındaki bilgileri belirtin.

    $family="Windows Server 2012 R2 Datacenter"
    $vmname="Contoso100-test"
    $vmsize="ExtraSmall"

D, DS veya G-serisi sanal makineler için InstanceSize değerleri için bkz: [sanal makine ve bulut hizmeti boyutları Azure](https://msdn.microsoft.com/library/azure/dn197896.aspx).

Yönetilen etki alanı hakkında bilgi sağlar.

    $domaindns="contoso100.com"
    $domacctdomain="contoso100"

Bulut hizmeti adını belirtin.

    $svcname="Contoso100-test"

VM birleştirilmelidir sanal ağın adını belirtin. AAD DS yönetilen etki alanını bu sanal ağda kullanılabilir olduğundan emin olun.

    $vnetname="MyPreviewVnet"

VM sağlamak için kullanılacak VM görüntüsünü seçin.

    $image=Get-AzureVMImage | where { $_.ImageFamily -eq $family } | sort PublishedDate -Descending | select -ExpandProperty ImageName -First 1

VM - kümesi VM adı, örnek boyutu & kullanılacak resmi yapılandırın.

    $vm1=New-AzureVMConfig -Name $vmname -InstanceSize $vmsize -ImageName $image

VM için yerel yönetici kimlik bilgilerini edinin. Güçlü bir yerel yönetici parolası seçin.

    $localadmincred=Get-Credential –Message "Type the name and password of the local administrator account."

VM yönetilen etki alanına katmak için 'AAD DC Yöneticiler' grubuna ait bir kullanıcı hesabı için kimlik bilgilerini edinin. Etki alanı adı - Örneğin, örneğimizde belirtmezseniz, biz 'bob' kullanıcı adı olarak belirtin.

    $domainadmincred=Get-Credential –Message "Now type the name (DO NOT INCLUDE THE DOMAIN) and password of an account in the AAD DC Administrators group, that has permission to add the machine to the domain."

VM yapılandırma - etki alanı katılma gereksinim & gerekli kimlik bilgilerini belirtin.

    $vm1 | Add-AzureProvisioningConfig -AdminUsername $localadmincred.Username -Password $localadmincred.GetNetworkCredential().Password -WindowsDomain -Domain $domacctdomain -DomainUserName $domainadmincred.Username -DomainPassword $domainadmincred.GetNetworkCredential().Password -JoinDomain $domaindns

Bir alt ağ için VM ayarlayın.

    $vm1 | Set-AzureSubnet -SubnetNames "Subnet-1"

İsteğe bağlı: etki alanı IP adresine işaret edecek. Sanal ağın DNS sunucuları olması için Azure AD etki alanı Hizmetleri yönetilen etki alanı IP adreslerini ayarlarsanız, bu adım gerekli değildir.

    $dns = New-AzureDns -Name 'contoso100-dc1' -IPAddress '10.0.0.4'

Şimdi, etki alanına katılmış Windows VM sağlayın.

    New-AzureVM –ServiceName $svcname -VMs $vm1 -VNetName $vnetname -Location "Central US" -DnsSettings $dns

<br>

## <a name="script-to-provision-a-windows-vm-and-automatically-join-it-to-an-aad-domain-services-managed-domain"></a>Bir Windows VM sağlamak ve otomatik olarak bir AAD etki alanı Hizmetleri yönetilen etki alanına eklemek için komut dosyası
Bu PowerShell komut kümesini iş sunucu için bir sanal makine oluşturur:

* Windows Server 2012 R2 Datacenter görüntüsü kullanır.
* Ek çok küçük bir sanal makine bulunuyor.
* Contoso100 test adına sahip.
* Otomatik olarak etki alanı contoso100 yönetilen etki alanına katılır.
* Aynı sanal ağa yönetilen etki alanı olarak eklenir.

Aşağıda, Windows sanal makine oluşturun ve otomatik olarak Azure AD etki alanı Hizmetleri yönetilen etki alanına katmak için tam örnek komut dosyası verilmiştir.

    $family="Windows Server 2012 R2 Datacenter"
    $vmname="Contoso100-test"
    $vmsize="ExtraSmall"

    $domaindns="contoso100.com"
    $domacctdomain="contoso100"

    $svcname="Contoso100-test"
    $vnetname="MyPreviewVnet"

    $image=Get-AzureVMImage | where { $_.ImageFamily -eq $family } | sort PublishedDate -Descending | select -ExpandProperty ImageName -First 1

    $vm1=New-AzureVMConfig -Name $vmname -InstanceSize $vmsize -ImageName $image

    $localadmincred=Get-Credential –Message "Type the name and password of the local administrator account."

    $domainadmincred=Get-Credential –Message "Now type the name (not including the domain) and password of an account in the AAD DC Administrators group, that has permission to add the machine to the domain."

    $vm1 | Add-AzureProvisioningConfig -AdminUsername $localadmincred.Username -Password $localadmincred.GetNetworkCredential().Password -WindowsDomain -Domain $domacctdomain -DomainUserName $domainadmincred.Username -DomainPassword $domainadmincred.GetNetworkCredential().Password -JoinDomain $domaindns

    $vm1 | Set-AzureSubnet -SubnetNames "Subnet-1"

    $dns = New-AzureDns -Name 'contoso100-dc1' -IPAddress '10.0.0.4'

    New-AzureVM –ServiceName $svcname -VMs $vm1 -VNetName $vnetname -Location "Central US" -DnsSettings $dns

<br>

## <a name="related-content"></a>İlgili İçerik
* [Azure AD etki alanı Hizmetleri - başlangıç kılavuzu](active-directory-ds-getting-started.md)
* [Azure AD Domain Services tarafından yönetilen etki alanını yönetme](active-directory-ds-admin-guide-administer-domain.md)
