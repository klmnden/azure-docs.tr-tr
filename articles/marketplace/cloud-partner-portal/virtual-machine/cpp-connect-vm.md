---
title: Microsoft Azure tabanlı sanal makinenize bağlanın | Microsoft Docs
description: Azure üzerinde oluşturulan yeni sanal makineye bağlanmak kullanılan nasıl açıklanmaktadır.
services: Azure, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: v-miclar
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: article
ms.date: 10/19/2018
ms.author: pbutlerm
ms.openlocfilehash: fd68846b9144c3efcc71dec369d64119427758a3
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60744557"
---
# <a name="connect-to-your-azure-based-virtual-machine"></a>Azure tabanlı sanal makinenize bağlanın

Bu makalede, bağlanma ve Azure üzerinde oluşturulan sanal makinelerin (VM'ler) oturum açıklanmaktadır.  Başarıyla bağlandıktan sonra kendi ana bilgisayar sunucusuna yerel olarak oturum gibi sanal makine ile çalışabilirsiniz. 

## <a name="connect-to-a-windows-based-vm"></a>Windows tabanlı bir sanal Makineye bağlanma

Azure üzerinde barındırılan Windows tabanlı sanal makineye bağlanmak için Uzak Masaüstü İstemcisi kullanır.  Çoğu Windows sürümleri, yerel olarak Uzak Masaüstü Protokolü (RDP) için destek içerir.  Diğer makineler için istemcileri hakkında daha fazla bilgi bulabilirsiniz [Uzak Masaüstü istemcilerini](https://docs.microsoft.com/windows-server/remote/remote-desktop-services/clients/remote-desktop-clients).  

Aşağıdaki makalede VM'nize bağlanmak için yerleşik Windows RDP desteği kullanmayı ayrıntıları: [Bağlanma ve bir Azure Windows çalıştıran sanal makine için oturum açma nasıl](../../../virtual-machines/windows/connect-logon.md).  

>[!TIP]
> Güvenlik Uyarıları işlemi sırasında örneğin .rdp dosyasının bilinmeyen bir yayımcıdan olduğunu veya kullanıcı kimlik bilgilerinizi doğrulanamıyor karşılaşabilirsiniz.  Bu uyarıların yoksayılabilir.


## <a name="connect-to-a-linux-based-vm"></a>Linux tabanlı bir sanal Makineye bağlanma

Linux tabanlı VM bağlanmak için güvenli Kabuk (SSH) protokolünü istemci gerekir.  Bu tartışma ücretsiz kullanacağı [PuTTY](https://www.ssh.com/ssh/putty/) SHH terminal.

1. İçinde **sanal makineler** dikey [Azure portalında](https://ms.portal.azure.com), bağlanmak istediğiniz sanal Makineyi seçin.  
2. **Başlangıç** zaten çalışmıyorsa, VM.
3. Açmak için sanal makinenin adına tıklayın, **genel bakış** sayfası.
4. Genel IP adresi ve DNS adını not edin.  (Bu değer ayarlanmaz sonra yapmanız gerekenler [ağ arabirimini oluşturun](https://docs.microsoft.com/azure/virtual-network/virtual-network-network-interface#create-a-network-interface)

   ![VM genel ayarları](./media/publishvm_019.png)
 
5. PuTTY uygulamasını açın.  
6. PuTTY yapılandırma iletişim kutusunda, IP adresini veya VM'nizi DNS adını girin. 

   ![Terminal puTTY ayarları](./media/publishvm_020.png)
 
7. Tıklayın **açın** PuTTY bir terminal açın.  
8. İstendiğinde, Linux VM hesabınızın parola ve hesap adını girin. 

   Bağlantı sorunları yaşıyorsanız, SSH istemcinizin belgelerine örneğin başvurduğu [Bölüm 10: Genel hata iletileri](https://www.ssh.com/ssh/putty/putty-manuals/0.68/Chapter10.html#errors).

Sağlanan bir Linux VM için bir masaüstü ekleme dahil olmak üzere daha fazla bilgi için bkz. [yükleyin ve azure'da bir Linux VM'ye bağlanmak için Uzak Masaüstü yapılandırma](../../../virtual-machines/linux/use-remote-desktop.md).


## <a name="stop-unused-vms"></a>Kullanılmayan Vm'lerini Durdur
Bir VM çalışırken sanal makine barındırma için azure faturaları *veya boşta*.  Bu nedenle şu anda kullanılmayan sanal makineleri durdurmak iyi bir uygulamadır.  Örneğin, test, yedekleme veya kullanımdan kaldırılan sanal makineleri kapatma için aday niteliği. Bir sanal makineyi için aşağıdaki adımları gerçekleştirin:

1. Üzerinde **sanal makineler** dikey penceresinde, durdurmak istediğiniz VM'yi seçin. 
2. Sayfanın üst kısmındaki araç çubuğunda, tıklayarak **Durdur** düğmesi.

   ![VM durdurma](./media/publishvm_018.png)

Azure VM olarak adlandırılan bir işlem içinde hızla durdurur *ayırmayı kaldırma*, yalnızca sanal Makinenin işletim sistemini kapatır, ancak ayrıca için daha önce sağlanmış olan donanım ve ağ kaynakları serbest bırakır.

Daha sonra VM'yi durdurduğunuzda yeniden etkinleştirmek istiyorsanız, onu seçin ve tıklayın **Başlat** düğmesi.


## <a name="next-steps"></a>Sonraki adımlar

Uzaktan bağlantı kurulduktan sonra hazır olduğunuz [VM'nizi yapılandırma](./cpp-configure-vm.md).
