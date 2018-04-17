---
title: Azure - bir coğrafi yapay zeka sanal makinede sağlamak Azure | Microsoft Docs
description: Azure coğrafi AI sanal makinede sağlamasını yapma.
keywords: derin öğrenme, AI, veri bilimi araçları, veri bilimi sanal makine, Jeo-uzamsal analizi
services: machine-learning
documentationcenter: ''
author: gopitk
manager: cgronlun
ms.assetid: ''
ms.service: machine-learning
ms.component: data-science-vm
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 03/05/2018
ms.author: gokuma
ms.openlocfilehash: 93dfe6594aeaf45a6905fe8cb55c98dd37cc9599
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
---
# <a name="provision-a-geo-artificial-intelligence-virtual-machine-on-azure"></a>Azure üzerinde bir coğrafi yapay zeka sanal makine sağlama 

Coğrafi AI veri bilimi sanal makine (coğrafi-DSVM) popüler uzantısıdır [Azure veri bilimi sanal makine](http://aka.ms/dsvm) özel AI ve Jeo-uzamsal analytics birleştirmek üzere yapılandırılmış. VM Jeo-uzamsal analytics'te tarafından sağlanmıştır [ArcGIS Pro](https://www.arcgis.com/features/index.html). Veri bilimi VM öğrenimi modellerinin oluşturulmasına ve modellerinde coğrafi bilgilerle Zenginleştirilmiş veri bile derin öğrenme makinenin hızlı eğitim sağlar. Yalnızca Windows 2016 DSVM üzerinde desteklenmiyor. 

Coğrafi DSVM AI dahil etmek için çeşitli araçlar içerir:

- Popüler derin öğrenme çerçeveleri GPU sürümleri Microsoft Bilişsel araç seti, TensorFlow, Keras, Caffe2, bağlayıcı ister; 
- edinmeye araçları ve önceden işlem resmi, metin verileri 
- Microsoft R Server Geliştirici sürümü, Anaconda Python, Python için Jupyter not defterlerini ve R, Python ve R, SQL için IDE gibi geliştirme etkinlikleri için veritabanı araçları
- AI uygulamalarınızı Jeo-uzamsal verilerle çalışabilirsiniz Python ve R arabirimleri birlikte ESRI'ın ArcGIS Pro masaüstü yazılımı. 


## <a name="create-your-geo-ai-data-science-vm"></a>Coğrafi AI veri bilimi VM oluşturma

Coğrafi AI veri bilimi VM örneği oluşturmak için gereken adımlar şunlardır: 


1. Sanal makine üzerinde listeleme gidin [Azure portal](https://ms.portal.azure.com/#create/microsoft-ads.geodsvmwindows).
2. Seçin **oluşturma** Sihirbazı'na gerçekleştirilecek altındaki düğmesini.
![create-geo-ai-dsvm](./media/provision-geo-ai-dsvm/Create-Geo-AI.png)
3. Coğrafi DSVM oluşturmak için kullanılan sihirbaz gerektirir **girişleri** her biri için **dört adım** Bu şekil sağ tarafta numaralandırılır. Bu adımların her biri yapılandırmak için gereken girdiler şunlardır:



   - **Temel Bilgiler**

      1. **Ad**: oluşturduğunuz veri bilimi sunucusunun adı.

      2. **Kullanıcı adı**: Yönetici hesap oturum açma kimliği.

      3. **Parola**: yönetici hesabı parolası.

      4. **Abonelik**: birden fazla aboneliğiniz varsa, bir makine olduğu oluşturulur ve fatura için seçin.

      5. **Kaynak grubu**: yeni bir tane oluşturabilirsiniz veya bir **boş** aboneliğinizde var olan Azure kaynak grubu.

      6. **Konum**: en uygun olan veri merkezi seçin. Genellikle verilerinizden en iyi veya en yakın fiziksel konumunuza en hızlı ağ erişimi için veri merkezi olur. GPU derin öğrenmeyi gerçekleştirmeniz gerekiyorsa, NC-serisi GPU VM örnekleri olan Azure konumlardan birini seçmeniz gerekir. Şu anda GPU VM'ye sahip konumlardır: **Doğu ABD, Kuzey Orta ABD, Orta Güney ABD, Batı ABD 2, Kuzey Avrupa, Batı Avrupa**. En son listesi için denetleme [Azure ürünleri bölge sayfası tarafından](https://azure.microsoft.com/regions/services/) ve Ara **NC-serisi** altında **işlem**. 


   - **Ayarları**: derin öğrenme GPU coğrafi DSVM üzerinde çalıştırmayı planlıyorsanız NC-serisi GPU sanal makine boyutu birini seçin. Aksi takdirde, örnek tabanlı CPU birini seçebilirsiniz.  VM için bir depolama hesabı oluşturun. 
   
   - **Özet**: girdiğiniz tüm bilgiler doğru olduğundan emin olun.

   - **Satın**: tıklatın **satın** sağlama başlatmak için. Bir bağlantı hizmet koşulları sağlanır. VM, seçtiğiniz sunucu boyutu işlem ötesinde herhangi bir ek ücret yok **boyutu** adım. 

>[!NOTE]
> Sağlama yaklaşık 20-30 dakika sürer. Sağlama durumu Azure portalda görüntülenir.


## <a name="how-to-access-the-geo-ai-data-science-virtual-machine"></a>Coğrafi AI veri bilimi sanal makine erişme

VM oluşturulduktan sonra üzerinde yüklü ve önceden yapılandırılmış araçları kullanmaya başlamak hazırsınız. Başlat menüsü döşeme ve araçlarının çoğu masaüstü simgelerini vardır. Önceki yapılandırdığınız yönetici hesabı kimlik bilgilerini kullanarak içine Uzak Masaüstü için **Temelleri** bölümü. 


## <a name="using-arcgis-pro-installed-in-the-vm"></a>VM'yi ArcGIS Pro kullanarak yüklü

Coğrafi DSVM ArcGIS Pro Masaüstü önceden yüklenmiş ve DSVM tüm araçlar çalışmak için önceden yapılandırılmış ortam zaten içeriyor. ArcGIS başlattığınızda oturum açma ArcGIS hesabınıza ister. Zaten bir ArcGIS hesabı varsa ve yazılım lisansları, var olan kimlik bilgilerinizi kullanabilirsiniz.  

![Yay CBS oturum](./media/provision-geo-ai-dsvm/ArcGISLogon.png)

Aksi takdirde, yeni ArcGIS hesabı ve lisans için kaydolun veya alın bir [ücretsiz deneme sürümü](https://www.arcgis.com/features/free-trial.html). 

![ArcGIS ücretsiz deneme](./media/provision-geo-ai-dsvm/ArcGIS-Free-Trial.png)

Kaydolma bir ya da için ücretli veya ücretsiz bir deneme ArcGIS hesabı bulduktan sonra siz ArcGIS Pro hesabınız için yönergeleri takip ederek yetkilendirebilir [ArcGIS Uzmanı Belgeleri ile çalışmaya başlama](http://www.esri.com/library/brochures/getting-started-with-arcgis-pro.pdf). 

ArcGIS Pro masaüstüne ArcGIS hesabınızla oturum açtıktan sonra yüklü olan ve VM Jeo-uzamsal analizi ve makine öğrenimi projeleri için yapılandırılan veri bilimi Araçlar'ı kullanmaya başlamak hazırsınız.

## <a name="next-steps"></a>Sonraki adımlar

Aşağıdaki konular rehberliğinde coğrafi AI veri bilimi VM kullanarak başlayın:

* [Coğrafi AI veri bilimi VM kullanın](use-geo-ai-dsvm.md)