---
title: Windows Server düğümü havuzlarını Azure Kubernetes Service (AKS) için kısıtlamalar
description: Azure Kubernetes Service (AKS) Windows Server düğüm havuzları ve uygulama iş yüklerini çalıştırırken bilinen sınırlamalar hakkında bilgi edinin
services: container-service
author: iainfoulds
ms.service: container-service
ms.topic: article
ms.date: 05/06/2019
ms.author: iainfou
ms.openlocfilehash: 3d249271995d96307722dadf6b3e012e63565e6a
ms.sourcegitcommit: 24fd3f9de6c73b01b0cee3bcd587c267898cbbee
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2019
ms.locfileid: "65956270"
---
# <a name="current-limitations-for-windows-server-node-pools-and-application-workloads-in-azure-kubernetes-service-aks"></a>Windows Server düğüm havuzları ve uygulama iş yüklerini Azure Kubernetes Service (AKS) için geçerli sınırlamalar

Azure Kubernetes Service (AKS), düğümler üzerinde konuk işletim sistemi Windows Server çalıştıran bir düğüm havuzunu oluşturabilirsiniz. Bu düğümler, .NET Framework üzerine inşa edilmiş gibi yerel Windows kapsayıcı uygulamalarının çalıştırabilirsiniz. Önemli farklar nasıl Linux ve Windows işletim sistemi sağlayan kapsayıcı desteği gibi bazı yaygın Kubernetes ve pod güvenlikle ilgili özellikler için Windows düğüm havuzları şu anda kullanılabilir değil.

Bu makalede, bazı sınırlamalar ve Windows Server AKS düğümleri işletim sistemi kavramlarını açıklar. Windows Server için düğüm havuzları şu anda Önizleme aşamasındadır.

> [!IMPORTANT]
> AKS Önizleme özellikleri, Self Servis ve kabul etme. Görüş ve hata topluluğumuza toplamak üzere önizlemeleri sağlanır. Ancak, Azure teknik destek birimi tarafından desteklenmez. Bir küme oluşturun veya var olan kümeleri için bu özellikleri ekleyin, bu özellik artık Önizleme aşamasındadır ve genel kullanılabilirlik (GA) mezunu kadar bu küme desteklenmiyor.
>
> Önizleme özellikleri sorunlarla karşılaşırsanız [AKS GitHub deposunda bir sorun açın] [ aks-github] hata başlığı önizleme özelliğini adı.

## <a name="limitations-for-windows-server-in-kubernetes"></a>Windows Server'da Kubernetes sınırlamaları

Windows Server kapsayıcıları bir Windows tabanlı bir kapsayıcı konağı üzerinde çalıştırmanız gerekir. Windows Server kapsayıcıları AKS çalıştırma için [Windows Server çalıştıran bir düğüm havuzunu oluşturma] [ windows-node-cli] konuk işletim sistemi olarak. Yukarı Akış Kubernetes projesi Windows Server'da parçası olan bazı sınırlamalar penceresi sunucu düğüm havuzu desteği içerir. Bu sınırlamalar, AKS için özgü değildir. Bu Yukarı Akış Kubernetes Windows Server desteği hakkında daha fazla bilgi için bkz. [Kubernetes sınırlamaları Windows Server kapsayıcıları][upstream-limitations].

Windows Server kapsayıcıları kubernetes için Yukarı Akış aşağıdaki sınırlamalar AKS için uygundur:

- Windows Server kapsayıcıları, yalnızca temel alınan Windows Server düğümün işletim sistemi ile eşleşen Windows Server 2019 kullanabilirsiniz.
    - Desteklenmeyen temel işletim sistemi olarak Windows Server 2016 kullanılarak oluşturulan kapsayıcı görüntüleri.
- Ayrıcalıklı kapsayıcıları kullanılamaz.
- Windows Server kapsayıcıları farklıkullanıcı, SELinux, AppArmor veya POSIX özellikleri gibi Linux'a özgü özellikleri kullanılamaz.
    - Kullanıcı izinleri UUI/GUID gibi Linux'a özgü olan dosya sistemi sınırlamaları da Windows Server kapsayıcıları mevcut değildir.
- Azure diskleri ve Azure dosyaları Windows Server kapsayıcı NTFS birimlerinde erişilebilir desteklenen birim türleri var.
    - NFS tabanlı depolama / birimler desteklenmez.

## <a name="aks-limitations-for-windows-server-node-pools"></a>Windows Server düğüm havuzları için AKS sınırlamaları

Windows Server düğüm havuzu desteği aks'deki aşağıdaki ek kısıtlamalar geçerlidir:

- Bir AKS kümesi, her zaman ilk düğüm havuzu olarak Linux düğüm havuzu içerir. AKS kümesi silinmedikçe bu ilk Linux tabanlı düğüm havuzu silinemiyor.
- AKS kümeleri Azure CNI (Gelişmiş) ağ modeli kullanmanız gerekir.
    - Kubernetes (Temel) ağ desteklenmiyor. Kubernetes kullanan bir AKS kümesi oluşturulamıyor. Ağ modelleri arasındaki farklılıklar hakkında daha fazla bilgi için bkz. [kavramları aks'deki uygulamalar için ağ][azure-network-models].
    - Azure CNI ağ modeli, ek planlama ve IP adresi yönetim değerlendirmeleri gerektirir. Planlama ve Azure CNI uygulama konusunda daha fazla bilgi için bkz. [Azure CNI yapılandırma ağ aks'deki][configure-azure-cni].
- Windows Server düğümleri aks'deki olmalıdır *yükseltilmiş* en son düzeltme eki korumak için en son Windows Server 2019 sürümü düzeltir ve güncelleştirir. Windows güncelleştirmeleri temel düğüm görüntüde aks'deki etkin değil. Düzenli bir zamanlamaya göre Windows Güncelleştirme sürüm döngüsü ve kendi doğrulama işlemi, AKS kümenizi yükseltme üzerinde Windows Server düğüm havuzları gerçekleştirmeniz gerekir. Bir Windows Server düğüm havuzu yükseltme hakkında daha fazla bilgi için bkz. [aks'deki bir düğüm havuzunu yükseltme][nodepool-upgrade].
    - Bu Windows Server yükseltir, geçici olarak eski düğüm kaldırılmadan önce yeni bir düğüm dağıtılmış olduğundan sanal ağ alt ağında ek IP adreslerini kullanır.
    - Yeni bir düğüm dağıtıldığında, vCPU kotaları abonelikte de geçici olarak kullandığı sonra eski düğüm kaldırıldı.
    - Otomatik olarak güncelleştirin ve yeniden başlatma kullanarak yönetme `kured` aks'deki Linux düğümleri ile.
- AKS kümesi en fazla sekiz düğüm havuzları sahip olabilir.
    - En fazla 400 düğümleri bu sekiz düğüm havuzları arasında olabilir.
- Windows Server düğüm havuzu adı, 6 karakterlik bir sınırı vardır.
- Windows Server düğümleri için olmayan ağ ilkesi ve küme ölçeklendirici, onaylı gibi AKS Önizleme özellikleri.
- Giriş denetleyicileri yalnızca Linux düğümlerinde bir NodeSelector kullanarak zamanlanmalıdır.
- Azure geliştirme alanları şu anda yalnızca Linux tabanlı düğüm havuzları için kullanılabilir.

## <a name="os-concepts-that-are-different"></a>Farklı işletim sistemi kavramları

Kubernetes tarihsel olarak Linux odaklıdır. Yukarı Akış olarak kullanılan birçok örneği [Kubernetes.io] [ kubernetes] Web sitesi kullanmak üzere Linux düğümleri yöneliktir. Windows Server kapsayıcıları, işletim sistemi düzeyinde Uygula, aşağıdaki konuları kullanan dağıtımlar oluşturduğunuzda:

- **Kimlik** -Linux UserID (UID) ve tamsayı türleri olarak temsil edilen GroupID (GID) kullanır. Kullanıcı ve grup adları kurallı olmayan - diğer adı içinde oldukları */etc/grupları* veya */etc/parola* UID + GID dönün.
    - Windows Server, Windows güvenlik erişim Yöneticisi (SAM) veritabanına depolanmış bir büyük ikili güvenlik tanımlayıcısı (SID) kullanır. Bu veritabanı, kapsayıcıları veya ana bilgisayar ve kapsayıcılar arasında paylaşılmaz.
- **Dosya izinleri** -Windows Server kullanan SID yerine bir bit maskesi UID + GID ve izinler dayalı erişim denetim listesi
- **Dosya yolları** -Windows Server'da kuraldır kullanılacak \ yerine /.
    - Bağlama birimleri pod özellikleri Windows Server kapsayıcıları için doğru yolu belirtin. Örneğin, yerine bağlama noktasını *birimi/mnt/* bir Linux kapsayıcısında bir sürücü harfi ve konum gibi belirtin */bin/birim* olarak takmak için *K:* sürücü.

## <a name="next-steps"></a>Sonraki adımlar

AKS, Windows Server kapsayıcıları kullanmaya başlamak için [AKS Windows Server çalıştıran bir düğüm havuzunu oluşturma][windows-node-cli].

<!-- LINKS - external -->
[upstream-limitations]: https://kubernetes.io/docs/setup/windows/#limitations
[aks-github]: https://github.com/azure/aks/issues]
[kubernetes]: https://kubernetes.io

<!-- LINKS - internal -->
[azure-network-models]: concepts-network.md#azure-virtual-networks
[configure-azure-cni]: configure-azure-cni.md
[nodepool-upgrade]: use-multiple-node-pools.md#upgrade-a-node-pool
[windows-node-cli]: windows-container-cli.md
