---
title: Bir Service Fabric için altyapıyı oluşturma Öğreticisi, Azure Vm'lerinde - Azure Service Fabric küme | Microsoft Docs
description: Bu öğreticide, bir Service Fabric kümesini çalıştırmak için Azure VM altyapısını ayarlamak konusunda bilgi edinin.
services: service-fabric
documentationcenter: .net
author: v-vasuke
manager: jpconnock
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: tutorial
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 3/25/2019
ms.author: v-vasuke
ms.custom: mvc
ms.openlocfilehash: b5f2f77b3caed483aed1736bd510096d44329284
ms.sourcegitcommit: a52d48238d00161be5d1ed5d04132db4de43e076
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67276009"
---
# <a name="tutorial-create-azure-vm-infrastructure-to-host-a-service-fabric-cluster"></a>Öğretici: Bir Service Fabric kümesini barındırmak için Azure VM altyapısı oluşturma

Service Fabric tek başına kümeleri, kendi ortamınızı seçme ve Service Fabric’in benimsediği "her işletim sistemi, her bulut" yaklaşımının bir parçası olarak bir küme oluşturma seçeneği sunar. Bu öğretici serisine Azure Vm'lerinde barındırılan tek başına küme oluşturma ve bir uygulamanın üzerine yükleyin.

Bu öğretici, bir dizinin birinci bölümüdür. Bu makalede, Service fabric'in tek başına kümenizi barındırmak için gereken Azure VM kaynakları oluşturur. Gelecek makalelerde, Service Fabric tek başına paketini yüklemeniz, kümenize bir örnek uygulama yüklemeniz ve son olarak kümenizi temizlemeniz gerekir.

Serinin birinci bölümünde şunları öğrenirsiniz:

> [!div class="checklist"]
> * AzureVM örnekleri kümesi oluşturma
> * Güvenlik grubunu değiştirme
> * Örneklerden birinde oturum açma
> * Service Fabric örneğini hazırlama

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi tamamlamak için bir Azure aboneliğinizin olması gerekir.  Zaten bir hesabınız yoksa, Git [Azure portalında](https://portal.azure.com) oluşturmak için.

## <a name="create-azure-virtual-machine-instances"></a>Azure sanal makine örnekleri oluşturma

1. Azure portal ve Seç'i açın **sanal makineler** (olmayan sanal makineler (Klasik)).

   ![Azure portalında VM][az-console]

2. Seçin **Ekle** açılacak düğmesi **sanal makine oluşturma** formu.

3. İçinde **Temelleri** sekmesinde, (kullanarak yeni bir kaynak grubu önerilir) istediğiniz aboneliği ve kaynak grubunu seçtiğinizden emin olun.

4. Değişiklik **görüntü** için yazın **Windows Server 2016 Datacenter**. 
 
5. Örnek **boyutu** için **standart DS2 v2**. Bir yönetici **kullanıcıadı** ve **parola**, bunların ne olduğunu belirtmeye.

6. Bırakın **gelen bağlantı noktası kuralları** ; şimdilik engellenen Biz bu sonraki bölümdeki yapılandırır.

7. İçinde **ağ** sekmesinde, yeni bir **sanal ağ** adını not alın.

8. Ardından, ayarlama **NIC ağ güvenlik grubu** için **Gelişmiş**. Adını belirterek yeni bir güvenlik grubu oluşturun ve tüm kaynaklarından gelen TCP trafiğine izin vermek için aşağıdaki kuralları oluşturun:

   ![SF gelen][sf-inbound]

   * Bağlantı noktası `3389`, RDP ve ICMP (temel bağlantı).
   * Bağlantı noktaları `19000-19003`, Service Fabric için.
   * Bağlantı noktaları `19080-19081`, Service Fabric için.
   * Bağlantı noktası `8080`, web tarayıcısı istekleri için.

   > [!TIP]
   > Service Fabric’te sanal makinelerinizi birbirine bağlamak için, altyapınızı barındıran VM’lerin aynı kimlik bilgilerine sahip olması gerekir.  Tutarlı kimlik bilgileri elde etmenin iki yaygın yolu vardır: tümünü aynı etki alanına eklemek veya her sanal makinede aynı yönetici parolasını ayarlamak. Neyse ki, Azure tüm sanal makinelerin aynı sağlar **sanal ağ** size sunduğumuz tüm örnekleri aynı ağ üzerinde yüklü olduğundan emin olur kolayca bağlanabileceğini.

9. Başka bir kural ekleyin. Kaynağı olmasını **hizmet etiketi** ve kaynak hizmet etiketi kümesine **VirtualNetwork**. Service Fabric kümesi içinde iletişim için açık olması için aşağıdaki bağlantı noktalarını gerektirir: 135,137-139,445,20001-20031,20606-20861.

   ![sanal ağa gelene][vnet-inbound]

10. Diğer seçenekleri varsayılan durumlarına kabul edilebilir. İsterseniz, bunları gözden geçirin ve sonra sanal makineniz başlatın.

## <a name="creating-more-instances-for-your-service-fabric-cluster"></a>Daha fazla örnek için Service Fabric kümenizi oluşturma

İki tane daha başlatın **sanal makineler**, önceki bölümde açıklanan aynı ayarları ayrıca korumanız özen gösterin. Özellikle, aynı yönetici kullanıcı adı ve parola korur. **Sanal ağ** ve **NIC ağ güvenlik grubu** değil yeniden oluşturulması; zaten oluşturduğunuz bulunan açılan menüden değerler seçin. Bu, her örneklerinizin dağıtılması için birkaç dakika sürebilir.

## <a name="connect-to-your-instances"></a>Kendi örneklerine bağlanabilirsiniz

1. Örneklerden birini **sanal makine** bölümü.

2. İçinde **genel bakış** sekmesinde, Not *özel* IP adresi. ' A tıklayarak **Connect**.

3. İçinde **RDP** sekmesinde, genel IP adresi ve bağlantı noktası 3389, biz özellikle daha önce açılan kullanıyoruz olduğunu unutmayın. RDP dosyasını indirin.
 
4. RDP dosyasını açın ve istendiğinde VM kurulumunda kullanıcı adı ve parolası girin.

5. Doğrulamak gereken bir örneğine bağlandıktan sonra uzak kayıt defteri çalışıyordu, SMB etkinleştirmek ve SMB ve uzak kayıt defteri için gerekli bağlantı noktalarını açın.

   SMB'yi etkinleştirmek için bu PowerShell komutu.

   ```powershell
   netsh advfirewall firewall set rule group="File and Printer Sharing" new enable=Yes
   ```

6. Buradaki güvenlik duvarında bağlantı noktalarını açmak için şu PowerShell komutunu kullanın:

   ```powershell
   New-NetFirewallRule -DisplayName "Service Fabric Ports" -Direction Inbound -Action Allow -RemoteAddress LocalSubnet -Protocol TCP -LocalPort 135, 137-139, 445
   ```

7. Özel IP adresleri yeniden belirtmekte diğer örnekleriniz için bu işlemi yineleyin.

## <a name="verify-your-settings"></a>Lütfen ayarlarınızı doğrulayın

1. Temel bağlantıyı doğrulamak için RDP kullanarak Vm'leri birine bağlanın.

2. Açın **komut istemi** içinde bu VM, sonra ping komutu başka değiştirerek bir VM'den bağlanabilirsiniz kullanmak özel IP'si biriyle IP adresi altında daha önce not ettiğiniz (değil IP bağlı olduğunuzdan VM'nin adresleri zaten).

   ```
   ping 172.31.20.163
   ```

   Çıktınız dört kez tekrar eden `Reply from 172.31.20.163: bytes=32 time<1ms TTL=128` gibi görünüyorsa, örnekler arasındaki bağlantınız çalışıyordur.

3. Şimdi aşağıdaki komutla SMB paylaşımınızın çalıştığını doğrulayın:

   ```
   net use * \\172.31.20.163\c$
   ```

   Çıktı olarak `Drive Z: is now connected to \\172.31.20.163\c$.` döndürülmelidir.


   Artık örneklerinizin Service Fabric için düzgün şekilde hazırlanır.

## <a name="next-steps"></a>Sonraki adımlar

Bölümünde bir dizi üç Azure VM örnekleri başlatın ve Service Fabric yükleme için yapılandırılmış alın öğrendiniz:

> [!div class="checklist"]
> * Azure VM örnekleri kümesi oluşturma
> * Güvenlik grubunu değiştirme
> * Örneklerden birinde oturum açma
> * Service Fabric örneğini hazırlama

Service Fabric’i kümenizde yapılandırmak için serinin ikinci bölümüne ilerleyin.

> [!div class="nextstepaction"]
> [Service Fabric yükleme](service-fabric-tutorial-standalone-create-service-fabric-cluster.md)

<!-- IMAGES -->
[az-console]: ./media/service-fabric-tutorial-standalone-azure-create-infrastructure/az-console.png
[sf-inbound]: ./media/service-fabric-tutorial-standalone-azure-create-infrastructure/sf-inbound.png
[vnet-inbound]: ./media/service-fabric-tutorial-standalone-azure-create-infrastructure/vnet-inbound.png
