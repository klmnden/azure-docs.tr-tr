---
title: Derin veri bilimi sanal makinesi öğrenme vm'si oluşturma
titleSuffix: Azure
description: Yapılandırma ve analiz için Azure'da bir derin öğrenme veri bilimi sanal makinesi oluşturma ve makine öğrenimi.
services: machine-learning
documentationcenter: ''
author: gopitk
manager: cgronlun
ms.custom: seodec18
ms.assetid: e1467c0f-497b-48f7-96a0-7f806a7bec0b
ms.service: machine-learning
ms.subservice: data-science-vm
ms.workload: data-services
ms.devlang: na
ms.topic: conceptual
ms.date: 03/16/2018
ms.author: gokuma
ms.openlocfilehash: 318df03c7c4447d051dfa396098462c0f8bbf423
ms.sourcegitcommit: 6f043a4da4454d5cb673377bb6c4ddd0ed30672d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/08/2019
ms.locfileid: "65410446"
---
# <a name="provision-a-deep-learning-virtual-machine-on-azure"></a>Derin azure'da sanal makine öğrenme vm'si sağlama 

Ayrıntılı öğrenme sanal makinesi (DLVM) bir özel yapılandırılmış popüler çeşididir [veri bilimi sanal makinesi](https://aka.ms/dsvm) GPU tabanlı VM kullanımını kolaylaştırmak için (DSVM) örneklerinin ayrıntılı öğrenme modelleri hızla eğitim. Temel olarak Windows 2016 veya Ubuntu DSVM ile desteklenir. Aynı çekirdekli VM görüntüleri DLVM paylaşır ve bu nedenle tüm zengin araç takımı DSVM üzerinde kullanılabilir. 

DLVM yapay ZEKA Microsoft Bilişsel araç seti, TensorFlow, Keras, Caffe2, bağlayıcı gibi popüler derin öğrenme çerçeveleri GPU sürümleri dahil olmak üzere çeşitli araçlar içerir; elde etmeye ve önceden işlem görüntü, metinsel veriler, Microsoft R Server Geliştirici sürümü, Anaconda Python Jupyter not defterleri için Python ve R, Python ve SQL, R IDE'ler gibi veri bilimi modelleme ve geliştirme etkinlikleri için araçları veritabanları ve diğer birçok Veri bilimi ve ML araçları. 

## <a name="create-your-deep-learning-virtual-machine"></a>Ayrıntılı öğrenme sanal makinesi oluşturma
Bir örneği, ayrıntılı öğrenme sanal makinesi oluşturmak için adımlar şunlardır: 

1. Sanal makine üzerinde listeleme gidin [Azure portalında](https://portal.azure.com/#create/microsoft-ads.dsvm-deep-learningtoolkit
).
2. Seçin **Oluştur** düğmesi Sihirbazı'na alınması için alt kısımdaki.![ oluşturma dlvm](./media/dlvm-provision-wizard.PNG)
3. DLVM oluşturmak için kullanılan sihirbaz gerektirir **girişleri** her biri için **dört adımı** bu şekilde sağ tarafındaki numaralandırılır. Bu adımların her biri yapılandırmak için gerekli girişleri şunlardır:

   <a name="basics"></a>   
   1. **Temel Bilgiler**
      
      1. **Ad**: Oluşturmakta olduğunuz veri bilimi sunucusunun adı.
      2. **Derin öğrenme VM için işletim sistemi türü seçin**: Windows veya Linux (için Windows 2016 ve Ubuntu Linux temel DSVM) seçin
      2. **Kullanıcı adı**: Yönetici hesabı oturum açma kimliği.
      3. **Parola**: Yönetici hesabı parolası.
      4. **Abonelik**: Birden fazla aboneliğiniz varsa, bir makine oluşturulması ve fatura olduğu seçin.
      5. **Kaynak grubu**: Yeni bir tane oluşturabilir veya bir **boş** aboneliğinizdeki mevcut bir Azure kaynak grubu.
      6. **Konum**: En uygun veri merkezi seçin. Genellikle, verilerinizden en iyi olduğundan veya en hızlı ağ erişimi için fiziksel konumunuza en yakın veri merkezi bulunur. 
      
      > [!NOTE]
      > DLVM tüm NC ve ND serisi GPU sanal makine örneğini destekler. DLVM sağlanırken GPU'ları olan Azure'da konumlardan birini seçmeniz gerekir. Denetleme [bölge sayfasına göre Azure ürünleri](https://azure.microsoft.com/regions/services/) sayfa için kullanılabilir konumların ve Ara **NC serisi**, **NCv2 serisi**, **NCv3 serisi** , veya **ND serisi** altında **işlem**. 

   1. **Ayarları**: NC serisi (NC NCv2, NCv3) veya maliyet kısıtlamaları ve işlevsel bir gereksinimi karşılayan, ND serisi GPU sanal makine boyutları birini seçin. Sanal Makineniz için bir depolama hesabı oluşturun.  ![dlvm-settings](./media/dlvm-provision-step-2.PNG)
   
   1. **Özet**: Girdiğiniz tüm bilgilerin doğru olduğunu doğrulayın.

   1. **Satın alma**: Tıklayın **satın** sağlamaya başlamak için. İşlemin koşullarının bağlantısı sunulur. VM, seçtiğiniz sunucu boyutu için işlem ötesinde herhangi bir ek ücreti yok **boyutu** adım. 

> [!NOTE]
> Sağlama yaklaşık 10-20 dakika sürer. Sağlama durumunu Azure portalında görüntülenir.
> 


## <a name="how-to-access-the-deep-learning-virtual-machine"></a>Nasıl erişileceğini ayrıntılı öğrenme sanal makinesi

### <a name="windows-edition"></a>Windows sürümü
VM oluşturulduktan sonra Uzak Masaüstü uygulamasına önceki yapılandırdığınız yönetici hesabı kimlik bilgilerini kullanarak yapabilecekleriniz **Temelleri** bölümü. 

### <a name="linux-edition"></a>Linux sürümü

VM oluşturulduktan sonra ona SSH kullanarak oturum açabilirsiniz. Oluşturduğunuz hesabı kimlik bilgilerini kullan [ **Temelleri** ](#basics) bölümünde metin kabuk arabirimi için adım 3. Azure Vm'leri için SSH bağlantıları hakkında daha fazla bilgi için bkz. [yükleyin ve azure'da bir Linux VM'ye bağlanmak için Uzak Masaüstü yapılandırma](/azure/virtual-machines/linux/use-remote-desktop). Bir Windows istemcisinde gibi bir SSH istemcisi aracı indirebilirsiniz [Putty](https://www.putty.org). Bir grafik desktop (X Windows sistemi) tercih ederseniz, Putty üzerinde iletme X11 kullanın veya X2Go istemciyi yükleyin. 

> [!NOTE]
> Bizim testimizde iletme X11 daha iyi X2Go istemci gerçekleştirdi. X2Go istemci masaüstü bir grafik arabirim için kullanmanızı öneririz.
> 
> 

#### <a name="installing-and-configuring-x2go-client"></a>Yükleme ve X2Go istemci yapılandırma
Linux DLVM zaten X2Go sunucusu ile sağlanan ve istemci bağlantılarını kabul etmeye hazır. Linux VM grafik masaüstüne bağlanmak için istemcinizi aşağıdaki yordamı tamamlayın:

1. İstemci platformunuza yönelik X2Go istemcisini indirme ve yükleme [X2Go](https://wiki.x2go.org/doku.php/doc:installation:x2goclient).    
2. X2Go istemci çalıştırıp seçeneğini **yeni oturumu**. Bu, birden fazla sekme ile bir yapılandırma penceresi açılır. Aşağıdaki yapılandırma parametrelerini girin:
   * **Oturum sekmesini**:
     * **Konak**: Ana bilgisayar adı veya IP adresini Linux veri bilimi sanal makinesi.
     * **Oturum açma**: Linux VM kullanıcı adı.
     * **SSH bağlantı noktası**: 22, varsayılan değeri bırakın.
     * **Oturum türü**: Bir değerle değiştirmek **XFCE**. Şu anda yalnızca Linux DSVM'sini XFCE Masaüstü destekler.
   * **Ortam sekmesini**: Ses desteği ve bunları kullanmak gerekmiyorsa Yazdırma İstemcisi kapatabilirsiniz.
   * **Paylaşılan Klasörler**: Linux VM'de bağlı istemci makinelerden dizinleri istiyorsanız bu sekmedeki VM ile paylaşmak istediğiniz istemci makine dizinlerine ekleyin.

VM'ye SSH istemcisi veya XFCE grafik Masaüstü X2Go istemcisi aracılığıyla kullanarak oturum açtıktan sonra yüklenmiş ve yapılandırılmış VM'de araçları kullanmaya başlamak hazırsınız. XFCE üzerinde çok sayıda araçla menüsü kısayolları uygulamalar ve masaüstü simgelerini görebilirsiniz.

Sanal makinenizin oluşturup sağlanan yüklenmiş ve yapılandırılmış araçları kullanmaya başlamak hazırsınız. Başlat menüsü kutucukları ve masaüstü simgelerini birçok araç vardır. 
