---
title: Anahtar kasası (Klasik) azure'da Windows sanal makinelerinde SQL Server ile tümleştirme | Microsoft Docs
description: Azure Key Vault ile kullanmak için SQL Server şifreleme yapılandırmasını otomatikleştirme hakkında bilgi edinin. Bu konuda, SQL Server sanal makineleri Klasik dağıtım modelinde oluşturma ile Azure anahtar kasası tümleştirmeyi kullanmayı açıklar.
services: virtual-machines-windows
documentationcenter: ''
author: MashaMSFT
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
ms.author: mathoma
ms.reviewer: jroth
ms.openlocfilehash: e20e2a094e1fd88dfc2a25b586dc6c894f92b418
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62108490"
---
# <a name="configure-azure-key-vault-integration-for-sql-server-on-azure-virtual-machines-classic"></a>Azure sanal makinelerinde (Klasik) SQL Server için Azure anahtar kasası tümleştirmesini yapılandırma
> [!div class="op_single_selector"]
> * [Resource Manager](../sql/virtual-machines-windows-ps-sql-keyvault.md)
> * [Klasik](../classic/ps-sql-keyvault.md)
> 
> 

## <a name="overview"></a>Genel Bakış
Birden çok SQL Server şifreleme özellikleri vardır, örneğin [saydam veri şifrelemesi (TDE)](https://msdn.microsoft.com/library/bb934049.aspx), [sütun düzeyinde şifrelemeyi (CLE)](https://msdn.microsoft.com/library/ms173744.aspx), ve [yedekleme şifrelemesi](https://msdn.microsoft.com/library/dn449489.aspx). Bu formlar şifreleme, şifreleme için kullandığınız şifreleme anahtarlarını depolamak ve yönetmek gerektirir. Azure Key Vault (AKV) hizmeti, güvenlik ve bu anahtarları güvenli ve yüksek oranda kullanılabilir bir konumda yönetimini iyileştirmek için tasarlanmıştır. [SQL Server Bağlayıcısı](https://www.microsoft.com/download/details.aspx?id=45344) Bu anahtarları Azure Key vault'tan kullanılacak SQL Server'ı etkinleştirir.

> [!IMPORTANT] 
> Azure'da oluşturmaya ve kaynaklarla çalışmaya yönelik iki farklı dağıtım modeli vardır: [Resource Manager ve klasik](../../../azure-resource-manager/resource-manager-deployment-model.md). Bu makalede, Klasik dağıtım modelini incelemektedir. Microsoft, yeni dağıtımların çoğunun Resource Manager modelini kullanmasını önerir.

SQL Server ile şirket içi makineleri çalıştırıyorsanız, vardır [şirket içi SQL Server makinenizden Azure Key Vault'a erişmesi için izlemeniz adımları](https://msdn.microsoft.com/library/dn198405.aspx). Ancak Azure vm'lerde SQL Server için kullanarak zaman kazanabilirsiniz *Azure anahtar kasası tümleştirmeyi* özelliği. Bu özelliği etkinleştirmek için birkaç Azure PowerShell cmdlet'leriyle, bir SQL VM anahtar kasanıza erişmek gerekli yapılandırmayı otomatik hale getirebilirsiniz.

Bu özellik etkinleştirildiğinde otomatik olarak SQL Server Bağlayıcısı, Azure Key Vault'a erişmesi için EKM sağlayıcısına yapılandırır, yüklenip kasanıza erişmek için izin vermek için kimlik bilgisi oluşturur. Şirket yukarıda belirtilen belgelere adımları sırasında görünüyorsa bu özelliği 2 ve 3. adımları otomatikleştiren görebilirsiniz. Yine de el ile yapmanız gerekir gereken tek şey, anahtarları ve anahtar kasası oluşturmaktır. Burada, tüm kurulum SQL vm'nizin otomatik hale getirilmiştir. Bu özellik, bu kurulum tamamlandıktan sonra veritabanları veya yedekleri normalde yaptığınız gibi şifreleme başlamak için T-SQL deyimleri yürütebilir.

[!INCLUDE [AKV Integration Prepare](../../../../includes/virtual-machines-sql-server-akv-prepare.md)]

## <a name="configure-akv-integration"></a>AKV tümleştirmesini yapılandırma
Azure anahtar kasası tümleştirmeyi yapılandırmak için PowerShell kullanın. Aşağıdaki bölümler, gerekli parametreleri ve örnek PowerShell Betiği bir genel bakış sağlar.

### <a name="install-the-sql-server-iaas-extension"></a>SQL Server Iaas uzantısı yükleme
İlk olarak, [SQL Server Iaas uzantısı yükleme](../classic/sql-server-agent-extension.md).

### <a name="understand-the-input-parameters"></a>Giriş parametreleri anlama
Aşağıdaki tabloda, sonraki bölümde PowerShell betiğini çalıştırmak için gereken parametreler listelenmektedir.

| Parametre | Açıklama | Örnek |
| --- | --- | --- |
| **$akvURL** |**Key vault URL'si** |"https:\//contosokeyvault.vault.azure.net/" |
| **$spName** |**Hizmet asıl adı** |"fde2b411-33d5-4e11-af04eb07b669ccf2" |
| **$spSecret** |**Hizmet sorumlusu gizli anahtarı** |"9VTJSQwzlFepD8XODnzy8n2V01Jd8dAjwm/azF1XDKM=" |
| **$credName** |**Kimlik bilgisi adı**: AKV tümleştirme VM'nin anahtar kasasına erişim sağlayan, SQL Server içinde bir kimlik bilgisi oluşturur. Bu kimlik bilgisi için bir ad seçin. |"mycred1" |
| **$vmName** |**Sanal makine adı**: Önceden oluşturulmuş bir SQL VM adı. |"myvmname" |
| **$serviceName** |**Hizmet adı**: SQL VM ile ilişkili bir bulut hizmeti adı. |"mycloudservicename" |

### <a name="enable-akv-integration-with-powershell"></a>PowerShell ile AKV tümleştirmesini etkinleştirme
**Yeni AzureVMSqlServerKeyVaultCredentialConfig** cmdlet'i Azure anahtar kasası tümleştirmeyi özelliği için bir yapılandırma nesnesi oluşturur. **Kümesi AzureVMSqlServerExtension** Bu tümleştirme sayesinde yapılandırır **KeyVaultCredentialSettings** parametresi. Aşağıdaki adımlar bu komutları kullanmak nasıl gösterir.

1. Azure PowerShell'de ilk giriş parametrelerini belirli değerlerinizle bu konunun önceki bölümlerinde anlatılan şekilde yapılandırın. Aşağıdaki komut bir örnektir.
   
        $akvURL = "https:\//contosokeyvault.vault.azure.net/"
        $spName = "fde2b411-33d5-4e11-af04eb07b669ccf2"
        $spSecret = "9VTJSQwzlFepD8XODnzy8n2V01Jd8dAjwm/azF1XDKM="
        $credName = "mycred1"
        $vmName = "myvmname"
        $serviceName = "mycloudservicename"
2. Ardından, yapılandırmak ve AKV tümleştirme sağlamak için aşağıdaki betiği kullanın.
   
        $secureakv =  $spSecret | ConvertTo-SecureString -AsPlainText -Force
        $akvs = New-AzureVMSqlServerKeyVaultCredentialConfig -Enable -CredentialName $credname -AzureKeyVaultUrl $akvURL -ServicePrincipalName $spName -ServicePrincipalSecret $secureakv
        Get-AzureVM -ServiceName $serviceName -Name $vmName | Set-AzureVMSqlServerExtension -KeyVaultCredentialSettings $akvs | Update-AzureVM

SQL Iaas Aracısı uzantısı, bu yeni yapılandırma ile SQL VM güncelleştirir.

[!INCLUDE [AKV Integration Next Steps](../../../../includes/virtual-machines-sql-server-akv-next-steps.md)]

