---
title: Anahtar kasası (Klasik) azure'da Windows VM üzerindeki SQL Server ile tümleştirme | Microsoft Docs
description: Azure anahtar kasası ile kullanmak için SQL Server şifrelemesi yapılandırmasını öğrenin. Bu konu Klasik dağıtım modelinde sanal makineler oluşturma SQL Server ile Azure anahtar kasası tümleştirmeyi kullanmayı açıklar.
services: virtual-machines-windows
documentationcenter: ''
author: rothja
manager: craigg
editor: ''
tags: azure-service-management
ms.assetid: ab8d41a7-1971-4032-ab71-eb435c455dc1
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 02/17/2017
ms.author: jroth
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 5fd0fb1f8ac9bb0132c64c195d4cc9c86ef8edd0
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/21/2018
ms.locfileid: "29399738"
---
# <a name="configure-azure-key-vault-integration-for-sql-server-on-azure-virtual-machines-classic"></a>SQL Server için Azure anahtar kasası tümleştirme Azure sanal makinelerde (Klasik) yapılandırma
> [!div class="op_single_selector"]
> * [Resource Manager](../sql/virtual-machines-windows-ps-sql-keyvault.md)
> * [Klasik](../classic/ps-sql-keyvault.md)
> 
> 

## <a name="overview"></a>Genel Bakış
Birden çok SQL Server şifreleme özellikleri vardır, gibi [saydam veri şifreleme (TDE)](https://msdn.microsoft.com/library/bb934049.aspx), [sütun düzeyinde şifreleme (Temizle)](https://msdn.microsoft.com/library/ms173744.aspx), ve [yedek şifreleme](https://msdn.microsoft.com/library/dn449489.aspx). Bu formlar şifreleme, şifreleme için kullandığınız şifreleme anahtarlarını depolamak ve yönetmek gerektirir. Azure anahtar kasası (AKV) hizmeti bu anahtarların güvenli ve yüksek oranda kullanılabilir bir konumda yönetim ve güvenlik artırmak için tasarlanmıştır. [SQL Server Connector](http://www.microsoft.com/download/details.aspx?id=45344) SQL Server'ın bu anahtarları Azure anahtar kasası kullanmaya sağlar.

> [!IMPORTANT] 
> Azure oluşturmak ve kaynaklarla çalışmak için iki farklı dağıtım modeli vardır: [Resource Manager ve klasik](../../../azure-resource-manager/resource-manager-deployment-model.md). Bu makalede, Klasik dağıtım modeli kullanarak yer almaktadır. Microsoft, yeni dağıtımların çoğunun Resource Manager modelini kullanmasını önerir.

Şirket içi makineler ile SQL Server çalışıyorsa, vardır [Azure anahtar kasası, şirket içi SQL Server makineden erişmek için izlemeniz adımları](https://msdn.microsoft.com/library/dn198405.aspx). Ancak Azure vm'lerinde SQL Server için kullanarak zaman kazanabilirsiniz *Azure anahtar kasası tümleştirmeyi* özelliği. Bu özelliği etkinleştirmek için birkaç Azure PowerShell cmdlet'leri ile bir SQL VM anahtar kasanızı erişmek gerekli yapılandırmayı otomatik hale getirebilirsiniz.

Bu özellik etkinleştirildiğinde, otomatik olarak SQL Server Bağlayıcısı'nı yükler, Azure anahtar kasası erişmek için EKM sağlayıcısına yapılandırır ve kasanızı erişmesine izin vermek için kimlik bilgisi oluşturur. Yukarıda açıklanan şirket içi belgelerindeki adımları sırasında görünüyorsa, bu özellik adım 2 ve 3 otomatikleştirir görebilirsiniz. Anahtar kasasını ve anahtarları hala el ile yapmanız gerekir tek şey oluşturmaktır. Buradan, tüm Kurulum, SQL VM otomatik hale getirilmiştir. Bu özellik, bu kurulum tamamlandıktan sonra veritabanları veya yedeklemeler normal olarak şifreleme başlamak için T-SQL deyimlerini yürütebilir.

[!INCLUDE [AKV Integration Prepare](../../../../includes/virtual-machines-sql-server-akv-prepare.md)]

## <a name="configure-akv-integration"></a>AKV tümleştirme yapılandırın
Azure anahtar kasası tümleştirmeyi yapılandırmak için PowerShell kullanın. Aşağıdaki bölümlerde gerekli parametreleri ve örnek PowerShell komut dosyasını bir bakış sağlar.

### <a name="install-the-sql-server-iaas-extension"></a>SQL Server Iaas uzantısını yükleyin
İlk olarak, [SQL Server Iaas uzantısını yüklemeniz](../classic/sql-server-agent-extension.md).

### <a name="understand-the-input-parameters"></a>Giriş parametreleri anlama
Aşağıdaki tabloda bir sonraki bölümde PowerShell betiğini çalıştırmak için gerekli olan parametreleri listeler.

| Parametre | Açıklama | Örnek |
| --- | --- | --- |
| **$akvURL** |**Anahtar kasası URL'si** |"https://contosokeyvault.vault.azure.net/" |
| **$spName** |**Hizmet asıl adı** |"fde2b411-33d5-4e11-af04eb07b669ccf2" |
| **$spSecret** |**Hizmet asıl gizli anahtarı** |"9VTJSQwzlFepD8XODnzy8n2V01Jd8dAjwm/azF1XDKM=" |
| **$credName** |**Kimlik bilgisi adı**: AKV Tümleştirme, VM’nin anahtar kasasına erişim sağlamasına izin vererek, SQL Server’da bir kimlik bilgisi oluşturur Bu kimlik bilgisi için bir ad seçin. |"mycred1" |
| **$vmName** |**Sanal makine adı**: önceden oluşturulmuş bir SQL VM adı. |"myvmname" |
| **$serviceName** |**Hizmet adı**: SQL VM ile ilişkili bulut hizmeti adını. |"mycloudservicename" |

### <a name="enable-akv-integration-with-powershell"></a>PowerShell ile AKV tümleştirmeyi etkinleştir
**Yeni AzureVMSqlServerKeyVaultCredentialConfig** cmdlet'i Azure anahtar kasası tümleştirme özelliği için bir yapılandırma nesnesi oluşturur. **Kümesi AzureVMSqlServerExtension** ile tümleştirme yapılandırır **KeyVaultCredentialSettings** parametresi. Aşağıdaki adımlar bu komutlarının nasıl kullanılacağını gösterir.

1. Azure PowerShell'de önce giriş parametrelerini belirli değerlerinizle, bu konunun önceki bölümlerinde açıklandığı gibi yapılandırın. Aşağıdaki komut dosyasını bir örnektir.
   
        $akvURL = "https://contosokeyvault.vault.azure.net/"
        $spName = "fde2b411-33d5-4e11-af04eb07b669ccf2"
        $spSecret = "9VTJSQwzlFepD8XODnzy8n2V01Jd8dAjwm/azF1XDKM="
        $credName = "mycred1"
        $vmName = "myvmname"
        $serviceName = "mycloudservicename"
2. Daha sonra yapılandırmak ve AKV tümleştirme etkinleştirmek için aşağıdaki komut dosyasını kullanın.
   
        $secureakv =  $spSecret | ConvertTo-SecureString -AsPlainText -Force
        $akvs = New-AzureVMSqlServerKeyVaultCredentialConfig -Enable -CredentialName $credname -AzureKeyVaultUrl $akvURL -ServicePrincipalName $spName -ServicePrincipalSecret $secureakv
        Get-AzureVM -ServiceName $serviceName -Name $vmName | Set-AzureVMSqlServerExtension -KeyVaultCredentialSettings $akvs | Update-AzureVM

SQL Iaas Aracısı uzantısı SQL VM yeni bu yapılandırmasıyla güncelleştirir.

[!INCLUDE [AKV Integration Next Steps](../../../../includes/virtual-machines-sql-server-akv-next-steps.md)]

