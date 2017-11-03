---
title: "Azure Service Fabric kapsayıcıları izleme ve tanılama | Microsoft Docs"
description: "İzleme ve Microsoft Azure Service Fabric üzerinde OMS'ın kapsayıcıları çözümüyle bağımsızlıklar kapsayıcıları tanılama öğrenin."
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 10/15/2017
ms.author: dekapur
ms.openlocfilehash: 319ee2c0f7492389bc1767aa2669dd273f8cfa1b
ms.sourcegitcommit: a7c01dbb03870adcb04ca34745ef256414dfc0b3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/17/2017
---
# <a name="monitoring-windows-server-containers-with-oms"></a>Windows Server kapsayıcıları OMS ile izleme

## <a name="oms-containers-solution"></a>OMS kapsayıcıları çözümü

Operations Management Suite (OMS) günlük analizi kapsayıcıları izleme için kullanılabilen bir kapsayıcı çözümü vardır. Service Fabric çözüm yanı sıra, bu çözüm Service Fabric bağımsızlıklar kapsayıcı dağıtımlarını izlemek için harika bir araçtır. Çözüm panosunda benzer basit bir örnek aşağıda verilmiştir:

![Temel OMS Panosu](./media/service-fabric-diagnostics-containers-windowsserver/oms-containers-dashboard.png)

Ayrıca, değişik OMS günlük analizi aracında sorgulanabilir ve herhangi bir ölçümleri veya oluşturulan olaylar görselleştirmek için kullanılan günlükleri toplar. Toplanan günlük türleri şunlardır:

1. ContainerInventory: kapsayıcı konumunu, adı ve görüntüleri hakkında bilgi gösterir
2. ContainerImageInventory: bilgi kimlikleri veya boyutları dahil olmak üzere, dağıtılan görüntüler hakkında
3. ContainerLog: belirli hata günlüklerini, docker günlükleri (stdout, vb.) ve diğer girişleri
4. ContainerServiceLog: çalıştırılmış docker arka plan programı komutları
5. Perf: kapsayıcı dahil olmak üzere performans sayaçlarını cpu, bellek, ağ trafiğini, disk g/ç ve ana bilgisayar makinelerden özel ölçümleri

Bu makalede, kümeniz için kapsayıcı izleme ayarlamak için gerekli adımlar kapsanmaktadır. OMS'ın kapsayıcıları çözüm hakkında daha fazla bilgi için kullanıma kendi [belgelerine](../log-analytics/log-analytics-containers.md).

## <a name="1-set-up-a-service-fabric-cluster"></a>1. Bir Service Fabric kümesi

Bulunan Azure Resource Manager şablonunu kullanarak bir küme oluşturmak [burada](https://github.com/ChackDan/Service-Fabric/tree/master/ARM%20Templates/SF%20OMS%20Samples/Windows). Benzersiz bir OMS çalışma ad eklediğinizden emin olun. Bu şablon, ayrıca hizmet, bu da üretimde kullanılamaz ve farklı bir Service Fabric sürüme yükseltilemez dokusu (v255.255) önizleme derlemesinin bir kümede dağıtmak için varsayılan olarak ayarlanır. Bu şablon için kullanmaya karar verirseniz, uzun vadeli veya üretim kullanın, sürüm kararlı sürüm numarasını değiştirin.

Küme ayarladıktan sonra uygun sertifika yüklediyseniz ve olduğunuzdan emin olun, kümeye bağlanmak için onaylayın.

Kaynak grubunuzun doğru göre başlığını yukarı ayarlandığından emin olun [Azure portal](https://portal.azure.com/) ve dağıtım bulma. Kaynak grubu Service Fabric kaynaklarını içeren ve Service Fabric çözüm yanı sıra, bir günlük analizi çözüm de gerekir.

Var olan bir Service Fabric kümesi değiştirmek için:
* Tanılama etkinleştirilmiş olduğunu doğrulayın (Aksi takdirde, aracılığıyla etkinleştirmek [sanal makine ölçek kümesi güncelleştirme](/rest/api/virtualmachinescalesets/create-or-update-a-set))
* "Service Fabric Analytics" Çözüm Azure Market üzerinden oluşturarak bir OMS çalışma alanı Ekle
* Veri kaynakları (WAD tarafından ayarlanır) uygun Azure Storage tablolardaki verileri alması için Service Fabric çözümün kümesinin kaynak grubundaki Düzenle
* Aracısı olarak ekleme bir [sanal makine ölçek kümesi uzantısı](/powershell/module/azurerm.compute/add-azurermvmssextension) PowerShell aracılığıyla veya sanal makine güncelleştiriliyor aracılığıyla ölçek kümesi (Resource Manager şablonu değiştirmek için yukarıdaki gibi aynı bağlantı)

## <a name="2-deploy-a-container"></a>2. Bir kapsayıcı dağıtma

Küme hazır olduğundan ve ona erişebildiğinizden emin onayladıktan sonra bir kapsayıcı dağıtma. Şablon tarafından kümesi olarak Önizleme sürümü kullanmayı seçerseniz, Service Fabric'ın yeni docker de keşfedebilirsiniz işlevselliği oluşturun. Bir küme için bir kapsayıcı görüntüsü dağıtılan ilk kez, görüntünün boyutuna bağlı olarak yüklemek için birkaç dakika sürer aklınızda size aittir.

## <a name="3-add-the-containers-solution"></a>3. Kapsayıcıları çözüme ekleyin

Azure portalında kapsayıcıları kaynak oluşturma (izleme + Yönetimi altında kategori) Azure Market üzerinden. 

![Kapsayıcıları çözüm ekleme](./media/service-fabric-diagnostics-containers-windowsserver/containers-solution.png)

Oluşturma adımda bir OMS çalışma ister. Yukarıdaki dağıtımı ile oluşturulmuş bir tanesini seçin. Bu adım, OMS çalışma kapsayıcıları çözümünüzde ekler ve şablon tarafından dağıtılan OMS aracısı tarafından otomatik olarak algılanır. Aracı kümedeki kapsayıcıları işlemler üzerinde veri toplamayı başlatılır ve yaklaşık 10-15 dakika içinde yukarı çözüm ışık yukarıdaki Pano görüntüsü olduğu gibi verilerle görmeniz gerekir.

## <a name="4-next-steps"></a>4. Sonraki adımlar

OMS çalışma daha kullanışlı varsa yapmak için çeşitli araçlar sunar. Çözümü gereksinimlerinize özelleştirmek için aşağıdaki seçeneklerden keşfedin:
- İle familiarized [günlük arama ve sorgulama](../log-analytics/log-analytics-log-searches.md) günlük analizi bir parçası olarak sunulan özellikler
- Belirli performans sayaçlarını seçmek için OMS Aracısı'nı yapılandırma (çalışma giriş gidin > ayarları > veri > Windows performans sayaçlarını)
- Ayarlamak için OMS yapılandırma [uyarı otomatik](../log-analytics/log-analytics-alerts.md) algılama ve tanılama yardımcı olmak için kurallar