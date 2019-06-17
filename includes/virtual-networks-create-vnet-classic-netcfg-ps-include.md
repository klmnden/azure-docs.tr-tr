---
title: include dosyası
description: include dosyası
services: virtual-network
author: genlin
ms.service: virtual-network
ms.topic: include
ms.date: 04/13/2018
ms.author: genli
ms.custom: include file
ms.openlocfilehash: bda289e73b9a782cd56c0c94b8f53e8002b1ccf4
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66116892"
---
## <a name="how-to-create-a-virtual-network-using-a-network-config-file-from-powershell"></a>Ağ yapılandırma dosyasından PowerShell kullanarak bir sanal ağ oluşturma
Azure aboneliği kullanılabilir tüm sanal ağları tanımlamak için bir xml dosyası kullanır. Bu dosyayı indirin, değiştirmek veya var olan sanal ağları silmek için düzenleyin ve yeni sanal ağ oluşturun. Bu öğreticide indirmek için ağ yapılandırma (veya netcfg) dosyası olarak adlandırılır. Bu dosya, öğrenin ve yeni bir sanal ağ oluşturmak için düzenleyin. Ağ yapılandırma dosyasını hakkında daha fazla bilgi için bkz: [Azure virtual network yapılandırma şeması](https://msdn.microsoft.com/library/azure/jj157100.aspx).

PowerShell kullanarak bir netcfg dosyasını ile bir sanal ağ oluşturmak için aşağıdaki adımları tamamlayın:

1. Azure PowerShell'i hiç kullanmadıysanız, adımları tamamlamanız [yükleme ve yapılandırma, Azure PowerShell](/powershell/azureps-cmdlets-docs) makalesi, sonra Azure'da oturum açın ve aboneliğinizi seçin.
2. Azure PowerShell konsolunu **Get-AzureVnetConfig** cmdlet'i aşağıdaki komutu çalıştırarak ağ yapılandırma dosyasını bilgisayarınızdaki bir dizine indirmek için: 
   
   ```powershell
   Get-AzureVNetConfig -ExportToFile c:\azure\NetworkConfig.xml
   ```
   
   Beklenen çıktı:
  
      ```
      XMLConfiguration                                                                                                     
      ----------------                                                                                                     
      <?xml version="1.0" encoding="utf-8"?>...
      ```

3. Herhangi bir XML veya metin düzenleyicisi uygulamayı kullanarak 2. adımda kaydettiğiniz dosyayı açın ve Ara  **\<VirtualNetworkSites >** öğesi. Önceden oluşturulmuş herhangi bir ağa varsa, her ağ kendi olarak görüntülenen  **\<VirtualNetworkSite >** öğesi.
4. Bu senaryoda açıklanan sanal ağ oluşturmak için aşağıdaki XML altına ekleyin  **\<VirtualNetworkSites >** öğesi:

   ```xml
         <?xml version="1.0" encoding="utf-8"?>
         <NetworkConfiguration xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/ServiceHosting/2011/07/NetworkConfiguration">
           <VirtualNetworkConfiguration>
             <VirtualNetworkSites>
                 <VirtualNetworkSite name="TestVNet" Location="East US">
                   <AddressSpace>
                     <AddressPrefix>192.168.0.0/16</AddressPrefix>
                   </AddressSpace>
                   <Subnets>
                     <Subnet name="FrontEnd">
                       <AddressPrefix>192.168.1.0/24</AddressPrefix>
                     </Subnet>
                     <Subnet name="BackEnd">
                       <AddressPrefix>192.168.2.0/24</AddressPrefix>
                     </Subnet>
                   </Subnets>
                 </VirtualNetworkSite>
             </VirtualNetworkSites>
           </VirtualNetworkConfiguration>
         </NetworkConfiguration>
   ```
   
5. Ağ yapılandırma dosyasını kaydedin.
6. Azure PowerShell konsolunu **kümesi AzureVnetConfig** cmdlet'i aşağıdaki komutu çalıştırarak ağ yapılandırma dosyasını karşıya yüklemek için: 
   
   ```powershell
   Set-AzureVNetConfig -ConfigurationPath c:\azure\NetworkConfig.xml
   ```
   
   Çıkış döndürdü:
   
      ```
      OperationDescription OperationId                          OperationStatus
      -------------------- -----------                          ---------------
      Set-AzureVNetConfig  <Id>                                 Succeeded 
      ```
   
   Varsa **OperationStatus** değil *başarılı* döndürülen Çıkışta, hatalar ve tam adım 6 için xml dosyasını yeniden denetleyin.

7. Azure PowerShell konsolunu **Get-AzureVnetSite** cmdlet'i yeni bir ağ aşağıdaki komutu çalıştırarak eklendiğini doğrulamak için: 

   ```powershell
   Get-AzureVNetSite -VNetName TestVNet
   ```
   
   Döndürülen (kısaltılmış) çıktı aşağıdaki metni içerir:
  
      ```
      AddressSpacePrefixes : {192.168.0.0/16}
      Location             : Central US
      Name                 : TestVNet
      State                : Created
      Subnets              : {FrontEnd, BackEnd}
      OperationDescription : Get-AzureVNetSite
      OperationStatus      : Succeeded
      ```
