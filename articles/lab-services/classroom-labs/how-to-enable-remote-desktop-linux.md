---
title: Azure Lab Services, Linux için Uzak masaüstünü etkinleştirme | Microsoft Docs
description: Azure Lab Services laboratuvarda, Linux sanal makineleri için Uzak masaüstünü etkinleştirme hakkında bilgi edinin.
services: lab-services
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/28/2019
ms.author: spelluru
ms.openlocfilehash: 6985bd0bbae858ad258e723ef4d6d6d687b2c86e
ms.sourcegitcommit: bf509e05e4b1dc5553b4483dfcc2221055fa80f2
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2019
ms.locfileid: "60005358"
---
# <a name="enable-and-use-remote-desktop-for-linux-virtual-machines-in-a-lab-in-azure-lab-services"></a>Etkinleştirme ve Linux sanal makineleri bir laboratuar ortamında Azure Lab Services içinde için Uzak Masaüstü kullanma
Bu makalede, aşağıdaki görevlerin nasıl yapılacağını gösterir:

- Linux VM için Uzak masaüstünü etkinleştir
- Nasıl Öğretmen, şablona VM Uzak Masaüstü Bağlantısı (RDP) üzerinden bağlanabilirsiniz.
- Nasıl Öğrenciler Öğrenci VM RDP aracılığıyla bağlanma

## <a name="enable-remote-desktop-for-linux-vm"></a>Linux VM için Uzak masaüstünü etkinleştir
Laboratuvar oluşturma sırasında Öğretmenler etkinleştirebilirsiniz **Uzak Masaüstü Bağlantısı** için **Linux** görüntüler. **Uzak Masaüstü Bağlantısı etkinleştirme** seçeneği, bir Linux görüntüsü şablonu seçildiğinde gösterilir. Bu seçenek etkinleştirildiğinde, Öğretmenler için şablon VM ve Öğrenci Vm'lere RDP (Uzak Masaüstü) bağlanabilirsiniz. 

![Bir Linux görüntüsü için Uzak Masaüstü bağlantısını etkinleştirme](../media/how-to-enable-remote-desktop-linux/enable-rdp-option.png)

> [!IMPORTANT] 
> Etkinleştirme **Uzak Masaüstü Bağlantısı** yalnızca açılır **RDP** Linux makinelerinde bağlantı noktası. Bir öğretmen olarak, ilk kez SSH kullanarak Linux makinesine bağlanma ve daha sonra RDP kullanarak bir Linux makineye bağlanabilmesi RDP ve GUI paketlerini yükleyin. Ardından, **yayımlama** yansıma böylece Öğrenciler RDP Öğrenci Linux Vm'leri için. 

## <a name="supported-operating-systems"></a>Desteklenen işletim sistemleri
Şu anda Uzak Masaüstü Bağlantısı, aşağıdaki işletim sistemlerinde desteklenir:

- openSUSE Leap 42.3
- CentOS tabanlı 7.5
- Debian 9 "Uzat"
- Ubuntu Server 16.04 LTS

## <a name="teachers-connecting-to-the-template-vm-using-rdp"></a>Öğretmenler şablona VM bağlanmak, RDP kullanarak
Öğretmenler, şablona VM bağlamalısınız önce SSH kullanarak ve RDP ve GUI paketlerini yükleyin. Ardından, Öğretmenler, RDP kullanarak Linux Vm'lerine bağlanmak için aşağıdaki adımları kullanabilirsiniz: 

Gördüğünüz **Uzak Masaüstü** Laboratuvar oluşturma sırasında şablona VM bağlanmak için seçeneği. 

![Oluşturma sırasında şablon RDP aracılığıyla bağlanma](../media/how-to-enable-remote-desktop-linux/connect-at-creation.png)

Gördüğünüz **Uzak Masaüstü** seçeneği laboratuvarı oluşturduktan sonra Laboratuvar giriş sayfasında ve VM şablonu başlatılır. VM şablonu zaten başlamamışsa başlatın. 

![Laboratuvar oluşturulduktan sonra şablon RDP aracılığıyla bağlanma](../media/how-to-enable-remote-desktop-linux/rdp-after-lab-creation.png) 

Seçtiğinizde, **RDP** seçeneği, bir RDP dosyasını indirir. Linux makinesine bağlanmak için açın. 

## <a name="teachers-connecting-to-a-student-vm-using-rdp"></a>Öğretmen bir öğrenci VM bağlanmak, RDP kullanarak
Laboratuvar sahibi (Öğretmen/Profesör) bir öğrenci VM bağlanabilir geçiş tarafından **sanal makineler** görüntülemek ve seçerek **bağlanma** simgesi. Bundan önce Öğretmenler gerekir **yayımlama** yüklü paketleri RDP ve GUI ile şablon görüntüsü. 

![Öğretmenler öğrencilerin VM bağlanma](../media/how-to-enable-remote-desktop-linux/teacher-connect-to-student-vm.png)

## <a name="students-connecting-to-the-student-vm"></a>Öğrenciler Öğrenci VM'ye bağlanma
Laboratuvar sahibi (Öğretmen/Profesör) sonra Öğrenci için kendi Linux vm'lere RDP **yayımlar** şablon RDP ve GUI paketleri ile sanal makinede yüklü. Adımlar aşağıdaki gibidir: 

1. Ne zaman bir öğrenci oturum açtığında Labs Portalı'na doğrudan (`http://labs.azure.com`) veya bir kayıt bağlantı kullanarak (`http://labs.azure.com/register/<registrationCode>`), her bir laboratuvar Öğrenci erişimi için bir kutucuk görüntülenir. 
2. Kutucuğa seçin **Başlat** VM durdurulduysa. 
3. **Bağlan**’ı seçin. Bu eylem, makinenizde oturum açın ve RDP dosyasını indirir. Dosyayı kaydedin ve Linux makineye RDP aracılığıyla bağlanmak için açın. 

    ![Öğrenci VM - RDP indirin](../media/how-to-enable-remote-desktop-linux/student-rdp-download.png)

    Linux VM'ye SSH kullanarak yine de bağlanabilir. Seçin **... (üç nokta)**  SSH seçeneği görmek için. 
    
    ![Öğrenci VM - SSH](../media/how-to-enable-remote-desktop-linux/student-ssh.png)

    Kopyalayabilir ve üzerinde SSH bağlantı dizesini kaydetmek **sanal makinenize bağlanın** iletişim kutusu. Bir SSH terminalden Bu bağlantı dizesini kullan (gibi [Putty](https://www.putty.org/)) sanal makineye bağlanmak için. 

## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki makalelere bakın:

- [Bir yönetici olarak oluşturun ve Laboratuvar hesaplarını yönetme](how-to-manage-lab-accounts.md)
- [Laboratuvar sahibi olarak oluşturun ve Laboratuvarları yönetin](how-to-manage-classroom-labs.md)
- [Laboratuvar sahibi olarak ayarlama ve şablonları Yayımlama](how-to-create-manage-template.md)
- [Bir laboratuvar kullanıcı olarak sınıf laboratuvarlarına erişim](how-to-use-classroom-lab.md)

