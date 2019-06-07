---
title: Azure Cloud Shell özellikleri | Microsoft Docs
description: Azure Cloud Shell'deki Bash hizmetinde özelliklerine genel bakış
services: Azure
documentationcenter: ''
author: maertendMSFT
manager: timlt
tags: azure-resource-manager
ms.assetid: ''
ms.service: azure
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 04/26/2019
ms.author: damaerte
ms.openlocfilehash: 6b5f0e96b90ee0515c0a86f41c6ee2161d6c54a6
ms.sourcegitcommit: 45e4466eac6cfd6a30da9facd8fe6afba64f6f50
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/07/2019
ms.locfileid: "66752723"
---
# <a name="features--tools-for-azure-cloud-shell"></a>Özellikler ve Azure Cloud Shell için Araçlar

[!INCLUDE [features-introblock](../../includes/cloud-shell-features-introblock.md)]

Azure Cloud Shell çalıştığı `Ubuntu 16.04 LTS`.

## <a name="features"></a>Özellikler

### <a name="secure-automatic-authentication"></a>Güvenli otomatik kimlik doğrulaması

Cloud Shell'i güvenli bir şekilde ve otomatik olarak Azure PowerShell ve Azure CLI için hesap erişimi doğrular.

### <a name="home-persistence-across-sessions"></a>Oturumlar arasında $HOME kalıcılığı

Dosyaları oturumlarda kalıcı hale getirilmesi için Cloud Shell ilk kez başlattığınızda bir Azure dosya paylaşımı ekleme aracılığıyla size.
Tamamlandığında, Cloud Shell depolama alanınızı otomatik olarak eklenecek (takılamadı `$HOME\clouddrive`) gelecekteki tüm oturumları.
Ayrıca, `$HOME` dizin olarak Azure dosya paylaşımınızdaki bir .img kalıcıdır.
Dışında dosyaları `$HOME` ve makine durumu oturumlar arasında sürdürülmez. SSH anahtarları gibi gizli dizilerin depolanmasında en iyi uygulamaları kullanın. Gibi hizmetleri [Azure anahtar kasası kurulumu için öğreticiler sahip](https://docs.microsoft.com/azure/key-vault/key-vault-manage-with-cli2#prerequisites).

[Cloud shell'de kalıcı dosyaları hakkında daha fazla bilgi edinin.](persisting-shell-storage.md)

### <a name="azure-drive-azure"></a>Azure sürücüsü (Azure:)

Cloud shell'de PowerShell Azure sürücüsü başlar (`Azure:`).
Azure sürücüsü kolay keşfe ve işlem, ağ, depolama vb. için dosya sistemi gezintisi benzer gibi Azure kaynaklarının gezinme sağlar.
Tanıdık kullanmaya devam edebilirsiniz [Azure PowerShell cmdlet'lerini](https://docs.microsoft.com/powershell/azure) bulunduğunuz sürücü bağımsız olarak bu kaynakları.
Azure kaynakları, doğrudan Azure portalında veya Azure PowerShell cmdlet'leri aracılığıyla yapılan ya da yapılan tüm değişiklikler Azure sürücüde yansıtılır.  Çalıştırabileceğiniz `dir -Force` kaynaklarınızı yenilenemedi.

![](media/features-powershell/azure-drive.png)

### <a name="manage-exchange-online"></a>Exchange Online yönetme

Cloud shell'de PowerShell, Exchange Online modülünün özel bir yapıyı içerir.  Çalıştırma `Connect-EXOPSSession` , Exchange cmdlet'lerini alma.

![](media/features-powershell/exchangeonline.png)

 `Get-Command -Module tmp_*` öğesini çalıştırın
> [!NOTE]
> Modül adı ile başlamalıdır `tmp_`, aynı ön ekine sahip modülleri yüklediyseniz, içerdikleri cmdlet'ler de gösterilir. 

![](media/features-powershell/exchangeonlinecmdlets.png)

### <a name="deep-integration-with-open-source-tooling"></a>Açık kaynak araçları ile kapsamlı tümleştirme

Cloud Shell'de önceden yapılandırılmış kimlik doğrulama için Terraform, Ansible ve Chef InSpec gibi açık kaynaklı araçları içerir. Örnek izlenecek deneyin.

## <a name="tools"></a>Araçlar

|Kategori   |Ad   |
|---|---|
|Linux araçları            |Bash<br> zsh<br> Göster<br> tmux<br> dıg<br>               |
|Azure Araçları            |[Azure CLI](https://github.com/Azure/azure-cli) ve [Klasik Azure CLI](https://github.com/Azure/azure-xplat-cli)<br> [AzCopy](https://docs.microsoft.com/previous-versions/azure/storage/storage-use-azcopy#writing-your-first-azcopy-command)<br> [Service Fabric CLI](https://docs.microsoft.com/azure/service-fabric/service-fabric-cli)<br> [Batch Shipyard](https://github.com/Azure/batch-shipyard)<br> [blobxfer](https://github.com/Azure/blobxfer)|
|Metin düzenleyiciler           |kod (Cloud Shell Düzenleyicisi)<br> vim<br> nano<br> emacs    |
|Kaynak denetimi         |git                    |
|Derleme araçları            |Olun<br> Maven<br> npm<br> pip         |
|Kapsayıcılar             |[Docker Makinesi](https://github.com/docker/machine)<br> [Kubectl](https://kubernetes.io/docs/user-guide/kubectl-overview/)<br> [Helm](https://github.com/kubernetes/helm)<br> [DC/OS CLI'Sİ](https://github.com/dcos/dcos-cli)         |
|Veritabanları              |MySQL istemci<br> PostgreSql istemcisi<br> [sqlcmd Utility](https://docs.microsoft.com/sql/tools/sqlcmd-utility)<br> [mssql-scripter](https://github.com/Microsoft/sql-xplat-cli) |
|Diğer                  |Ipython istemci<br> [Cloud Foundry CLI](https://github.com/cloudfoundry/cli)<br> [Terraform](https://www.terraform.io/docs/providers/azurerm/)<br> [Ansible](https://www.ansible.com/microsoft-azure)<br> [Chef InSpec](https://www.chef.io/inspec/)|

## <a name="language-support"></a>Dil desteği

|Dil   |Version   |
|---|---|
|.NET Core  |2.0.0       |
|Git         |1.9        |
|Java       |1,8        |
|Node.js    |8.9.4      |
|PowerShell |[6.2.0](https://github.com/PowerShell/powershell/releases)       |
|Python     |2.7 ve 3.5 (varsayılan)|

## <a name="next-steps"></a>Sonraki adımlar
[Bash cloud Shell hızlı başlangıçta](quickstart.md) <br>
[PowerShell Cloud Shell hızlı başlangıç](quickstart-powershell.md) <br>
[Azure CLI hakkında bilgi edinin](https://docs.microsoft.com/cli/azure/) <br>
[Azure PowerShell hakkında bilgi edinin](https://docs.microsoft.com/powershell/azure/) <br>
