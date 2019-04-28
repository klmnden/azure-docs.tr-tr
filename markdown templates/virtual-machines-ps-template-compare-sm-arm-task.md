---
ms.openlocfilehash: aec6282daadc61b4e1bcf6bbaf1266d9bc98cdac
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61232911"
---
<!--save a copy of this file to your local repo. It's important that you follow the naming conventions by starting with the service name, and use lowercase only for the file name. See "file-names-and-locations.md" under the "contributor-guide" folder in your repo.

Info to help you use the template are enclosed in the Markdown comments using the caret, hyphen, dash syntax. Delete these from your file.

Text not wrapped in comment syntax is intended to be used as is, or with updates enclosed in [  ]. Add the info and delete the bracket. 

Pay attention to spacing and indents. They affect formatting. 

--> 

<!--replace this with Properties and Tags sections. These are required sections. See "article-metadata.md" in under the "contributor-guide" folder in your repo. Attributes in each section can be placed on separate lines to make them easier to read and check-->

# <a name="use-azure-powershell-to-task"></a>[Görev] için Azure PowerShell'i kullanma
Bu makalede, Azure modülü hem Azure Resource Manager modülüyle komutları kullanarak [Görev], işlemini göstermektedir. Bu yeni komutlar yanı sıra mevcut betikleri için yeni komutlar geçirme öğrenmenize yardımcı olmak için tasarlanmıştır.

## <a name="prerequisite-install-a-recent-version-of-azure-powershell"></a>Önkoşul: En güncel Azure PowerShell sürümünü yükleyin
En az henüz yapmadıysanız, yerel bilgisayarınızda Azure PowerShell [sürüm numarası] sürümü yükleyin. Önceki bir sürümünü kullanıyorsanız, bu makalede açıklanan Azure Resource Manager cmdlet'leri olmaz. Ayrıntılar için bkz.

* [Azure PowerShell'i yükleme ve yapılandırma işlemini](install-configure-powershell.md) Azure PowerShell ayarlama ayarlama hakkında yönergeler için.
* [Resource Manager ile Windows PowerShell kullanarak](powershell-azure-resource-manager.md) için Resource Manager'ı kullanma hakkında temel bilgiler.

> [!NOTE]
> Çoğu görevler, bir yönetici düzeyinde Azure PowerShell komut istemini kullanmayı gerektirir.
> 
> 

## <a name="command-comparison"></a>Komut karşılaştırma
Bu [tablo | bölüm] komut sözdizimi görülmektedir.

<!--[optional image - to use an image in this article, add a folder with the same name as the article file name without extension, inside the Media folder of the repo. Use only this folder to store the images. Don't attempt to use a common folder to share images you want to use in more than 1 file.]
Then, use the following syntax to add a reference to the image in your article:
![](./media/name-of-file-without-extension/image-name-no-spaces.png)
-->

<!--if a command string uses variables, define the variables first, using the  following construction. In some cases the variable is straightforward and doesn't need much explanation. But parameters such as location and size can benefit from brief explanation or listing all accepted values:--> 

Bu komut örnekleri aşağıdaki değişkenleri kullanır:

$FriendlyName"<Describe value>"

<!-- if it makes more sense to present this in a table, use this. Otherwise, delete. The table won't render until it's in GitHub or published to Sandbox.-->

| Hizmet Yönetimi | Resource Manager |
| --- | --- |
| `syntax` |`syntax` |

<!--if it makes more sense to present this one command block after the other instead of a table, use this. Otherwise, delete-->

[Komut hakkında kısa bir giriş cümle. Gerçekten çok şey yoksa söylemek atlayın. Ancak bu yaklaşımlar kullanıyorsa, bir işlem hattı biçimde açıklayan]:

    [command string]

## <a name="script-examples"></a>Betik örnekleri
[Görev] için [cmdlet adları)] kullanan bir örnek aşağıdadır. Komutlar içerir:

* [kısa fiil, kullandığı sahip, olduğundan, vb.]
* [sonraki kısa fiili] 

<!--include this statement if it uses variables that weren't introduced earlier--> Aşağıdaki değişkenler içerir:

* [1 değişkeni]
* [2 değişkeni]

<!--This shows you how a recent example was presented as well as how it was formatted. Preceding each line with one tab or four spaces to format in a code block-->

    $family="Windows Server 2012 R2 Datacenter"
    $image=Get-AzureVMImage | where { $_.ImageFamily -eq $family } | sort PublishedDate -Descending | select -ExpandProperty ImageName -First 1
    $vmname="AZDC1"
    $vmsize="Medium"
    $vm1=New-AzureVMConfig -Name $vmname -InstanceSize $vmsize -ImageName $image

    $cred=Get-Credential -Message "Type the name and password of the local administrator account."
    $vm1 | Add-AzureProvisioningConfig -Windows -AdminUsername $cred.GetNetworkCredential().Username -Password $cred.GetNetworkCredential().Password

    $vm1 | Set-AzureSubnet -SubnetNames "BackEnd"

    $vm1 | Set-AzureStaticVNetIP -IPAddress 192.168.244.4

    $disksize=20
    $disklabel="DCData"
    $lun=0
    $hcaching="None"
    $vm1 | Add-AzureDataDisk -CreateNew -DiskSizeInGB $disksize -DiskLabel $disklabel -LUN $lun -HostCaching $hcaching

    $svcname="Azure-TailspinToys"
    $vnetname="AZDatacenter"
    New-AzureVM –ServiceName $svcname -VMs $vm1 -VNetName $vnetname


## <a name="additional-resources"></a>Ek Kaynaklar
<!--At a minimum, include a link back to the migration task list article. Use the formats shown below. See create-links-markdown.md for more info -->
<!--use this format for links to other articles, such as the migration task list. -->
[Kullanılabilirliği yönetme](../articles/virtual-machines/windows/manage-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

<!--To link to an ACOM page outside the /documentation/ subdomain (such as a pricing page, SLA page or anything else that is not a documentation article), use an absolute URL, but omit the locale:

    [link text](https://azure.microsoft.com/pricing/details/virtual-machines/)-->

<!--use this for URLs outside of ACOM. Be sure to locale, and if you're linking to the Azure library on MSDN, include the '/azure/' part of the URL-->
[Sanal makineler belgeleri](/previous-versions/azure/jj156003(v=azure.100))

