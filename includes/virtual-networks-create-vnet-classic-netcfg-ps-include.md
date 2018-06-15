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
ms.openlocfilehash: 4ae4c3100ae13fdb05e17974b433b247128c1a50
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2018
ms.locfileid: "31805076"
---
## <a name="how-to-create-a-virtual-network-using-a-network-config-file-from-powershell"></a>Ağ yapılandırma dosyasından PowerShell kullanarak bir sanal ağ oluşturma
Azure aboneliği kullanılabilir tüm sanal ağları tanımlamak için bir xml dosyası kullanır. Bu dosyayı yükleyin, değiştirmek veya varolan sanal ağlar silmek için düzenlemek ve yeni sanal ağlar oluşturun. Bu öğreticide öğrenmek için ağ yapılandırması (veya netcfg) dosyası olarak adlandırılan bu dosyayı yükleyin ve yeni bir sanal ağ oluşturmak üzere düzenleyebilirsiniz. Ağ yapılandırma dosyası hakkında daha fazla bilgi için bkz: [Azure virtual network yapılandırma şeması](https://msdn.microsoft.com/library/azure/jj157100.aspx).

PowerShell kullanarak bir netcfg dosyasını bir sanal ağ oluşturmak için aşağıdaki adımları tamamlayın:

1. Azure PowerShell'i hiç kullanmadıysanız, bölümündeki adımları tamamlamanız [yükleme ve yapılandırma Azure PowerShell](/powershell/azureps-cmdlets-docs) makalesi, ardından Azure oturumu açın ve aboneliğinizi seçin.
2. Azure PowerShell konsolundan kullanmak **Get-AzureVnetConfig** aşağıdaki komutu çalıştırarak, bilgisayarınızdaki bir dizine ağ yapılandırma dosyası indirme girişiminde cmdlet: 
   
   ```powershell
   Get-AzureVNetConfig -ExportToFile c:\azure\NetworkConfig.xml
   ```
   
   Beklenen çıktı:
  
      ```
      XMLConfiguration                                                                                                     
      ----------------                                                                                                     
      <?xml version="1.0" encoding="utf-8"?>...
      ```

3. Herhangi bir XML veya metin düzenleyicisi uygulama kullanarak 2. adımda kaydettiğiniz dosyayı açın ve Ara **<VirtualNetworkSites>** öğesi. Önceden oluşturulmuş tüm ağlarınız varsa, her ağ kendi olarak görüntülenen **<VirtualNetworkSite>** öğesi.
4. Bu senaryoda açıklanan sanal ağ oluşturmak için aşağıdaki XML yalnızca altında eklemek **<VirtualNetworkSites>** öğe:

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
6. Azure PowerShell konsolundan kullanmak **kümesi AzureVnetConfig** cmdlet'i aşağıdaki komutu çalıştırarak ağ yapılandırma dosyasını karşıya yüklemek için: 
   
   ```powershell
   Set-AzureVNetConfig -ConfigurationPath c:\azure\NetworkConfig.xml
   ```
   
   Çıkış döndürdü:
   
      ```
      OperationDescription OperationId                          OperationStatus
      -------------------- -----------                          ---------------
      Set-AzureVNetConfig  <Id>                                 Succeeded 
      ```
   
   Varsa **OperationStatus** değil *başarılı* döndürülen çıkış hatalar ve tam adım 6 için xml dosyasını yeniden denetleyin.

7. Azure PowerShell konsolundan kullanmak **Get-AzureVnetSite** cmdlet yeni bir ağ aşağıdaki komutu çalıştırarak eklendiğini doğrulamak için: 

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
