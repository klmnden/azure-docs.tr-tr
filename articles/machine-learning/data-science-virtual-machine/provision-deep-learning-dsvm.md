---
title: Azure üzerinde veri bilimi sanal makine öğrenme derin sağlama | Microsoft Docs
description: Yapılandırma ve analizi için Azure üzerinde derin öğrenme veri bilimi sanal makine oluşturun ve makine.
services: machine-learning
documentationcenter: ''
author: gopitk
manager: cgronlun
ms.assetid: e1467c0f-497b-48f7-96a0-7f806a7bec0b
ms.service: machine-learning
ms.component: data-science-vm
ms.workload: data-services
ms.devlang: na
ms.topic: conceptual
ms.date: 03/16/2018
ms.author: gokuma
ms.openlocfilehash: 76149ce3864811cf2b5648f8dc0aa214e5820d9f
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
---
# <a name="provision-a-deep-learning-virtual-machine-on-azure"></a>Azure üzerinde sanal makine öğrenme derin sağlama 

Derin öğrenme sanal makine (DLVM) bir özel yapılandırılmış popüler çeşididir [veri bilimi sanal makine](http://aka.ms/dsvm) (GPU tabanlı VM kullanmayı daha kolay yapmak için DSVM) örnekler derin öğrenme modelleri hızla eğitmek için. Temel olarak Windows 2016 ya da Ubuntu DSVM ile desteklenir. Aynı çekirdeği VM görüntüleri DLVM paylaşır ve bu nedenle tüm zengin araç takımı DSVM üzerinde kullanılabilir. 

DLVM AI Microsoft Bilişsel araç seti, TensorFlow, Keras, Caffe2, bağlayıcı gibi popüler derin öğrenme çerçeveleri GPU sürümleri de dahil olmak üzere çeşitli araçlar içerir; almak için Araçlar ve önceden işlem görüntü metinsel veri, Microsoft R Server Geliştirici sürümü, Anaconda Python, Python için Jupyter not defterlerini ve R, Python ve R, SQL için IDE gibi veri bilimi modelleme ve geliştirme etkinlikleri için araçları veritabanları ve diğer birçok Veri bilimi ve ML araçları. 

## <a name="create-your-deep-learning-virtual-machine"></a>Sanal makine öğrenme, derin oluşturma
Örnek, derin öğrenme sanal makine oluşturmak için adımlar şunlardır: 

1. Sanal makine üzerinde listeleme gidin [Azure portal](https://portal.azure.com/#create/microsoft-ads.dsvm-deep-learningtoolkit
).
2. Seçin **oluşturma** Sihirbazı'na gerçekleştirilecek altındaki düğmesini.![ oluşturma dlvm](./media/dlvm-provision-wizard.PNG)
3. DLVM oluşturmak için kullanılan sihirbaz gerektirir **girişleri** her biri için **dört adım** Bu şekil sağ tarafta numaralandırılır. Bu adımların her biri yapılandırmak için gereken girdiler şunlardır:
   
   1. **Temel Bilgiler**
      
      1. **Ad**: oluşturduğunuz veri bilimi sunucunuzun adını yazın.
      2. **Derin öğrenme VM için işletim sistemi türü seçin**: seçin, Windows veya Linux (için Windows 2016 ve Ubuntu Linux temel DSVM)
      2. **Kullanıcı adı**: Yönetici hesap oturum açma kimliği.
      3. **Parola**: yönetici hesabı parolası.
      4. **Abonelik**: birden fazla aboneliğiniz varsa, bir makine olduğu oluşturulur ve fatura için seçin.
      5. **Kaynak grubu**: yeni bir tane oluşturabilirsiniz veya bir **boş** aboneliğinizde var olan Azure kaynak grubu.
      6. **Konum**: en uygun olan veri merkezi seçin. Genellikle verilerinizden en iyi veya en yakın fiziksel konumunuza en hızlı ağ erişimi için veri merkezi olur. 
      
> [!NOTE]
> DLVM Azure NC-serisi GPU VM örneklerinde sağlandıktan sonra Gpu'lara sahip Azure konumlardan birini seçmeniz gerekir. Şu anda GPU VM'ye sahip konumlardır: **Doğu ABD, Kuzey Orta ABD, Orta Güney ABD, Batı ABD 2, Kuzey Avrupa, Batı Avrupa**. En son listesi için denetleme [Azure ürünleri bölge sayfası tarafından](https://azure.microsoft.com/en-us/regions/services/) ve Ara **NC-serisi** altında **işlem**. 

   2. **Ayarları**: işlev gereksinimi ve maliyet kısıtlamaları karşılayan NC-serisi GPU sanal makine boyutu birini seçin. VM için bir depolama hesabı oluşturun.  ![dlvm-settings](./media/dlvm-provision-step-2.PNG)
   
   3. **Özet**: girdiğiniz tüm bilgiler doğru olduğundan emin olun.
   5. **Satın**: tıklatın **satın** sağlama başlatmak için. Bağlantı işlem koşullarını sağlanır. VM, seçtiğiniz sunucu boyutu işlem ötesinde herhangi bir ek ücret yok **boyutu** adım. 

> [!NOTE]
> Sağlama yaklaşık 10-20 dakika sürer. Sağlama durumu Azure portalda görüntülenir.
> 


## <a name="how-to-access-the-deep-learning-virtual-machine"></a>Derin öğrenme sanal makine erişme

### <a name="windows-edition"></a>Windows sürümü
VM oluşturulduktan sonra Uzak Masaüstü önceki yapılandırdığınız yönetici hesabı kimlik bilgilerini kullanarak içine yapabilecekleriniz **Temelleri** bölümü. 

### <a name="linux-edition"></a>Linux sürümü

VM oluşturulduktan sonra kendisine SSH kullanarak oturum açabilirsiniz. Oluşturduğunuz hesap kimlik bilgilerini kullanan **Temelleri** adım 3 metin kabuk arabirimi için bölüm. AWindows istemcide bir SSH istemcisi aracı gibi indirebilirsiniz [Putty](http://www.putty.org). Grafik Masaüstü (X Windows sistemi) tercih ederseniz, Putty iletme X11 kullanın veya X2Go istemcisi yükleyin.

> [!NOTE]
> X2Go istemci bizim iletme X11 daha iyi gerçekleştirilir. X2Go istemci için bir grafik Masaüstü arabirimi kullanmanızı öneririz.
> 
> 

#### <a name="installing-and-configuring-x2go-client"></a>Yükleme ve X2Go istemci yapılandırma
Linux DLVM X2Go sunucusu ile sağlanan ve istemci bağlantılarını kabul etmeye hazır zaten var. Linux VM grafik masaüstüne bağlanmak için istemci aşağıdaki yordamı tamamlayın:

1. İstemci platformunuzu X2Go istemci yükleyip [X2Go](http://wiki.x2go.org/doku.php/doc:installation:x2goclient).    
2. X2Go istemci çalıştırmak ve seçmek **yeni oturum**. İle birden çok sekme yapılandırma penceresi açar. Aşağıdaki yapılandırma parametrelerini girin:
   * **Oturum sekmesini**:
     * **Ana bilgisayar**: ana bilgisayar adı veya IP adresini, Linux veri bilimi VM.
     * **Oturum açma**: Linux VM kullanıcı adı.
     * **SSH bağlantı noktası**: 22, varsayılan değeri bırakın.
     * **Oturum türü**: değerini değiştirin **XFCE**. Şu anda Linux DSVM yalnızca XFCE Masaüstü destekler.
   * **Ortam sekmesini**: ses desteği ve bunları kullanmaya ihtiyaç duymuyorsanız Yazdırma İstemcisi devre dışı bırakabilir.
   * **Paylaşılan Klasörler**: Linux VM'de bağlı istemci makinelerden dizinleri istiyorsanız bu sekmedeki VM paylaşmak istediğiniz istemci makine dizinleri ekleyin.

VM SSH istemcisi veya XFCE grafik Masaüstü X2Go istemcisinden kullanarak oturum açtıktan sonra yüklenmiş ve yapılandırılmış VM Araçları'nı kullanmaya başlamak hazırsınız. XFCE üzerinde uygulamaları menüsü kısayolları ve masaüstü simgelerini araçları çoğunu görebilirsiniz.

VM oluşturup sağlanan sonra yüklenmiş ve yapılandırılmış Araçları'nı kullanmaya başlamak hazırsınız. Başlat menüsü döşeme ve araçlarının çoğu masaüstü simgelerini vardır. 
