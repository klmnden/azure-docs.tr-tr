---
title: Bir Windows VM Azure PowerShell komut dosyalarını çalıştır
description: Bu konu, PowerShell komut dosyalarını çalıştır komutunu kullanarak bir Azure Windows sanal makine içinde çalıştırmayı açıklar
services: automation
ms.service: automation
author: georgewallace
ms.author: gwallace
ms.date: 06/06/2018
ms.topic: article
manager: carmonm
ms.openlocfilehash: ddbac24020110e32792286a1ac64070316cfb081
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "36332723"
---
# <a name="run-powershell-scripts-in-your-windows-vm-with-run-command"></a>PowerShell komut dosyalarını çalıştır komutu ile Windows VM'nizi çalıştırın

Bir Azure Windows VM dahilinde Kabuk PowerShell betikleri çalıştırmak için VM Aracısı farklı çalıştır komutunu kullanır. Bu komut dosyalarını genel makine ya da uygulama yönetimi için kullanılan ve hızla tanılamak ve VM erişimi ve ağ sorunları çözmek ve VM iyi bir durumda almak için kullanılabilir.

## <a name="benefits"></a>Avantajlar

Sanal makinelerinize erişmek için kullanılan birden fazla seçeneği vardır. Çalıştır komutunu kullanarak uzaktan VM Aracısı, sanal makinelerde çalıştırabilir. Azure Portalı aracılığıyla çalıştırma komutu kullanılabilir [REST API](/rest/api/compute/virtual%20machines%20run%20commands/runcommand), [Azure CLI](/cli/azure/vm/run-command?view=azure-cli-latest#az-vm-run-command-invoke), veya [PowerShell](/powershell/module/azurerm.compute/invoke-azurermvmruncommand).

Bu yetenek olduğu bir sanal makine içinde bir komut dosyası çalıştırmak istediğiniz ve sorun giderme ve RDP sahip olmayan bir sanal makine düzeltmek için yalnızca yollarından biri, veya SSH bağlantı noktası açın hatalı ağ veya yönetici kullanıcı nedeniyle tüm senaryolarda kullanışlıdır yapılandırma.

## <a name="restrictions"></a>Kısıtlamalar

Çalıştır komutunu kullanırken aşağıdaki kısıtlamalar geçerlidir:

* Çıktı son 4096 bayt ile sınırlı
* Bir komut dosyasını çalıştırmak için en düşük saat yaklaşık 20 saniyenin altındadır
* Sistemi olarak Windows üzerinde çalışan komut dosyaları
* Bir defada tek bir betik çalıştırabilir
* Çalışan bir komut iptal edilemez
* Bir komut dosyası çalıştırabilirsiniz en uzun süre 90 hangi BT sonra zaman aşımına uğrar, dakikadır

**PermissionsConfig OrchestratorUsersGroup***GroupName***- OrchestratorUser***kullanıcıadı***\-uzaktan** 

## <a name="run-a-command"></a>Bir komutu çalıştırın

Bir VM gidin [Azure](https://portal.azure.com) seçip **komutu çalıştırın** altında **OPERATIONS**. VM üzerinde çalıştırmak için kullanılabilir komutların listesini ile sunulur.

![Komut listesini çalıştırın.](./media/run-command/run-command-list.png)

Bir komut çalıştırmak için seçin. Bazı komutlar, isteğe bağlı veya gerekli giriş parametresi olabilir. Bu komutları için parametreleri metin alanları için giriş değerleri sağlamanızı olarak sunulur. Her komut genişleterek çalıştırılmakta olan komut dosyası görüntüleyebilirsiniz **görüntülemek betik**. **RunPowerShellScript** , kendi özel bir komut dosyası sağlamanıza olanak verir gibi diğer komutlarından farklıdır.

> [!NOTE]
> Yerleşik komutları düzenlenebilir değildir.

Komutu seçildikten sonra tıklayın **çalıştırmak** komut dosyasını çalıştırmak için. Betik çalışır ve tamamlandığında, çıkış ve hataları çıktı penceresinde döndürür. Aşağıdaki ekran görüntüsünde çalışan bir örnek çıkış şunları gösterir **RDPSettings** komutu.

![Komut betiği çıktısını çalıştırın](./media/run-command/run-command-script-output.png)

## <a name="commands"></a>Komutlar

Bu tablo Windows VM'ler için kullanılabilir komutların listesini gösterir. **RunPowerShellScript** komutu, istediğiniz özel komut dosyaları çalıştırmak için kullanılabilir.

|**Ad**|**Açıklama**|
|---|---|
|**RunPowerShellScript**|Bir PowerShell Betiği yürütür|
|**EnableRemotePS**|Uzak PowerShell etkinleştirmek için makineyi yapılandırır.|
|**EnableAdminAccount**|Yerel yönetici hesabı devre dışı bırakılır ve öyleyse sağlar olmadığını denetler.|
|**Ipconfig**| Ayrıntılı bilgiler görüntüler IP adresi, alt ağ maskesi ve varsayılan ağ geçidi TCP/IP'ye her bağdaştırıcı için.|
|**RDPSettings**|Kayıt defteri ayarları ve etki alanı ilkesi ayarlarını denetler. Makine bir etki alanının parçası olan veya ayarlarını varsayılan değerlerine değiştiriyorsa ilke eylemleri önerir.|
|**ResetAccountPassword**| Yerleşik yönetici hesabı parolasını sıfırlar.|
|**ResetRDPCert**|RDP dinleyicisini bağlı SSL sertifikası kaldırır ve RDP listerner güvenlik varsayılana geri yükler. Sertifika herhangi bir sorun görürseniz, bu komut dosyasını kullanın.|
|**SetRDPPort**|Ayarlar varsayılan veya kullanıcı, Uzak Masaüstü bağlantıları için bağlantı noktası numarası belirtildi. Bağlantı noktasına gelen erişim için güvenlik duvarı kuralı sağlar.|

## <a name="powershell"></a>PowerShell

Aşağıda bir örnek verilmiştir kullanarak [Invoke-AzureRmVMRunCommand](/powershell/module/azurerm.compute/invoke-azurermvmruncommand) cmdlet'ini Azure VM temelinde bir PowerShell betiğini çalıştırın.

```azurepowershell-interactive
Invoke-AzureRmVMRunCommand -ResourceGroupName '<myResourceGroup>' -Name '<myVMName>' -CommandId 'RunPowerShellScript' -ScriptPath '<pathToScript>' -Parameter @{"arg1" = "var1";"arg2" = "var2"}
```

## <a name="limiting-access-to-run-command"></a>Komutu Çalıştır erişimi sınırlandırma

Çalıştırma komutları listeleme veya bir komut ayrıntılarını gösteren gerektiren `Microsoft.Compute/locations/runCommands/read` izni olan yerleşik [okuyucu](../../role-based-access-control/built-in-roles.md#reader) rol ve daha yüksek.

Bir komut çalıştırmak gerektirir `Microsoft.Compute/virtualMachines/runCommand/action` izni, hangi [katkıda bulunan](../../role-based-access-control/built-in-roles.md#virtual-machine-contributor) rolü ve daha yüksek.

Aşağıdakilerden birini kullanabilirsiniz [yerleşik](../../role-based-access-control/built-in-roles.md) rolleri veya oluşturma bir [özel](../../role-based-access-control/custom-roles.md) rol Çalıştır komutunu kullanın.

## <a name="next-steps"></a>Sonraki adımlar

Bakın, [Windows VM'nizi komut dosyalarını Çalıştır](run-scripts-in-vm.md) betikleri ve komutları uzaktan, VM'yi çalıştırmak için diğer yolları hakkında bilgi edinmek için.