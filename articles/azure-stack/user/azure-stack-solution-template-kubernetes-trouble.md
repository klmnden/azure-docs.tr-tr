---
title: Azure Stack'te Kubernetes dağıtımı sorunlarını giderme | Microsoft Docs
description: Azure Stack'te Kubernetes dağıtımı sorunlarını gidermeyi öğrenin.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.author: mabrigg
ms.date: 04/02/2019
ms.reviewer: waltero
ms.lastreviewed: 03/20/2019
ms.openlocfilehash: 2a9eccfa109292b7d142092f69f4a664b0ff8f20
ms.sourcegitcommit: a60a55278f645f5d6cda95bcf9895441ade04629
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2019
ms.locfileid: "58878137"
---
# <a name="troubleshoot-kubernetes-deployment-to-azure-stack"></a>Azure Stack için Kubernetes dağıtımı sorunlarını giderme

*Şunlara uygulanır Azure Stack tümleşik sistemleri ve Azure Stack Geliştirme Seti*

> [!Note]  
> Azure Stack'te Kubernetes önizlemeye sunuldu. Azure Stack bağlantısı kesilmiş senaryo preview tarafından şu anda desteklenmiyor.

Aşağıdaki makalede Kubernetes kümeniz sorun giderme sırasında arar. Dağıtım uyarıyı gözden geçirin ve dağıtımınızın durumunu dağıtım için gerekli öğeler tarafından gözden geçirin. Azure Stack veya Linux sanal makineleri barındıran Kubernetes dağıtım günlüklerini toplamak gerekebilir. Bir yönetim uç noktasından günlükleri almak için Azure Stack yöneticinizle birlikte çalışmak gerekebilir.

## <a name="overview-of-kubernetes-deployment"></a>Kubernetes dağıtımına genel bakış

Kümenizi sorunlarını gidermeye başlamadan önce Azure Stack Kubernetes Küme dağıtımı işlemi gözden geçirmek isteyebilirsiniz. Dağıtım, sanal makineler oluşturmak ve kümeniz için ACS altyapısı yüklemek için bir Azure Resource Manager çözüm şablonu kullanır.

### <a name="kubernetes-deployment-workflow"></a>Kubernetes dağıtımı iş akışı

Küme dağıtımı için genel süreç Aşağıdaki diyagramda gösterilmektedir.

![Kubernetes işlem dağıtma](media/azure-stack-solution-template-kubernetes-trouble/002-Kubernetes-Deploy-Flow.png)

### <a name="deployment-steps"></a>Dağıtım adımları

1. Market öğesi giriş parametrelerini toplamak.

    Kubernetes kümesini ayarlamak için ihtiyacınız olan değerlere girin dahil olmak üzere:
    -  **Kullanıcı adı**: Kubernetes kümesi ve DVM parçası olan bir Linux sanal makineleri için kullanıcı adı.
    -  **SSH ortak anahtarı**: Kubernetes kümesi ve DVM parçası olarak oluşturulan tüm Linux makinelerinin yetkilendirme için kullanılan anahtar.
    -  **Hizmet sorumlusu**: Kubernetes Azure bulut sağlayıcısı tarafından kullanılan kimliği. İstemci kimliği, hizmet sorumlusu oluştururken sağladığınız uygulama kimliği olarak tanımlanır. 
    -  **İstemci gizli anahtarı**: Bunlar, hizmet sorumlunuzu oluşturduğunuzda oluşturulan anahtar.

2. VM dağıtımı oluşturmak ve özel betik uzantısı.
    -  Market Linux görüntüsü kullanarak Linux VM dağıtımı oluşturma **Ubuntu Server 16.04 LTS**.
    -  İndirin ve özel betik uzantısı marketten çalıştırın. Komut dosyası **Linux 2.0 için özel betik**.
    -  DVM özel betiği çalıştırın. Betik aşağıdaki görevleri gerçekleştirir:
        1. Galeri uç noktası Azure Resource Manager meta veri uç noktasından alır.
        2. Active directory kaynak kimliği Azure Resource Manager meta veri uç noktasından alır.
        3. ACS altyapısı için API modelini yükler.
        4. ACS altyapısı için Kubernetes kümesi dağıtır ve Azure Stack bulut profiline kaydeder `/etc/kubernetes/azurestackcloud.json`.
3. Ana VM'ler oluşturun.

4. İndirin ve özel betik Uzantıları'nı çalıştırın.

5. Ana betiği çalıştırın.

    Betik aşağıdaki görevleri gerçekleştirir:
    - Etcd, Docker ve Kubernetes yükler kubelet gibi kaynakları. etcd makine kümesi arasında verileri depolamak için bir yol sağlayan bir dağıtılmış anahtar değer deposudur. Docker kapsayıcıları olarak bilinen tam kemikler işletim sistemi düzeyinde virtualizations destekler. Kubelet her Kubernetes düğümü üzerinde çalışan düğümü aracısıdır.
    - Ayarlar **etcd** hizmeti.
    - Ayarlar **kubelet** hizmeti.
    - Kubelet başlatır. Bu görev, aşağıdaki adımları içerir:
        1. API hizmetini başlatır.
        2. Denetleyici Hizmeti başlatır.
        3. Scheduler hizmetini başlatır.
6. Aracı VM'ler oluşturun.

7. İndirin ve özel betik uzantısı'nı çalıştırın.

7. Aracı betiği çalıştırın. Aracısı özel betik aşağıdaki görevleri gerçekleştirir:
    - Yükler **etcd**.
    - Ayarlar **kubelet** hizmeti.
    - Kubernetes kümesini birleştirir.

## <a name="steps-for-troubleshooting"></a>Sorun giderme adımları

Kubernetes kümenizi destekleyen sanal makinelere günlüklerini toplayabilir. Ayrıca dağıtım günlüğünü gözden geçirebilirsiniz. Azure Stack, kullanılacağını ve Azure yığından dağıtımınızla ilgili günlüklerini almak için gereken sürümünü doğrulamak için Azure Stack yöneticinizle konuşun gerekebilir.

1. Gözden geçirme [dağıtım durumu](#review-deployment-status) ve ana düğüm Kubernetes kümenize günlüklerini almak.
2. Azure Stack en son sürümünü kullandığınızdan emin olun. Hangi sürümü kullandığınızdan emin değilseniz, Azure Stack yöneticinize başvurun.
3.  VM oluşturma dosyalarınızı gözden geçirin. Aşağıdaki sorunları vardı:  
    - Ortak anahtar geçersiz olabilir. Oluşturduğunuz anahtarı gözden geçirin.  
    - VM oluşturma veya oluşturma hatası tetikleyen bir iç hata tetiklenen. Azure Stack aboneliğiniz için kapasite sınırlamaları dahil olmak üzere hata, bir dizi etkene neden olabilir.
    - Sanal makine için tam etki alanı adı (FQDN) bir yinelenen öneki ile başladığından emin olun.
4.  VM **Tamam**, DVM değerlendirebilirsiniz. DVM bir hata iletisi varsa:

    - Ortak anahtar geçersiz olabilir. Oluşturduğunuz anahtarı gözden geçirin.  
    - Ayrıcalıklı uç noktaları kullanarak, Azure Stack için günlükleri almak için Azure Stack yöneticinize başvurmanız gerekir. Daha fazla bilgi için [Azure Stack'te tanılama araçları](https://docs.microsoft.com/azure/azure-stack/azure-stack-diagnostics).
5. Dağıtımınız hakkında bir sorunuz varsa, yayınlamak veya birisi zaten sorusuna cevap verdi varsa bkz [Azure Stack Forumu](https://social.msdn.microsoft.com/Forums/azure/home?forum=azurestack). 

## <a name="review-deployment-status"></a>Dağıtım durumunu gözden geçirin

Kubernetes kümesini dağıtırken, herhangi bir sorun için kontrol etmek için dağıtım durumunu gözden geçirebilirsiniz.

1. Açık [Azure Stack portalı](https://portal.local.azurestack.external).
2. Seçin **kaynak grupları**ve ardından, Kubernetes kümesini dağıtırken kullandığınız kaynak grubu adını seçin.
3. Seçin **dağıtımları**ve ardından **dağıtım adı**.

    ![Sorun giderme](media/azure-stack-solution-template-kubernetes-trouble/azure-stack-kub-trouble-report.png)

4.  Sorun giderme penceresi başvurun. Dağıtılan her bir kaynak, aşağıdaki bilgileri sağlar:
    
    | Özellik | Açıklama |
    | ----     | ----        |
    | Kaynak | Kaynak adı. |
    | Type | Kaynak sağlayıcıya ve kaynak türü. |
    | Durum | Öğenin durumu. |
    | Zaman Damgası | Saat UTC zaman damgası. |
    | İşlem ayrıntıları | İşlem ayrıntıları işlemi, kaynak uç noktası ve kaynağın adını söz konusuydu kaynak sağlayıcısı gibi. |

    Her öğenin bir durum simgesi yeşil veya kırmızı vardır.

## <a name="review-deployment-logs"></a>Dağıtım günlüklerini gözden geçirin

Azure Stack portal sorunlarını giderme veya dağıtım hatalarını gidermek için yeterli bilgi sağlamazsa, sonraki adım kümesi günlüklerine dig sağlamaktır. El ile dağıtım günlüklerini almak için genellikle bir kümenin ana sanal makinelerin bağlanmanız gerekir. Basit bir alternatif yaklaşım indirin ve aşağıdaki olacaktır [Bash betiği](https://aka.ms/AzsK8sLogCollectorScript) Azure Stack ekibi tarafından sağlanan. Bu betik DVM ve kümenin sanal makinelere bağlar, ilgili sistem ve küme günlükleri toplar ve bunları geri istasyonunuza indirir.

### <a name="prerequisites"></a>Önkoşullar

Azure Stack yönetmek için kullandığınız makine bir Bash isteminde gerekir. Bir Windows makinede bir Bash yükleyerek komut istemi alabilirsiniz [Git için Windows](https://git-scm.com/downloads). Yüklendikten sonra Ara _Git Bash_ , Başlat menüsünde.

### <a name="retrieving-the-logs"></a>Günlükleri alma

Toplamak ve küme günlükleri indirmek için aşağıdaki adımları izleyin:

1. Bir Bash istemi açın. Bir Windows makineden açın _Git Bash_ veya çalıştırın: `C:\Program Files\Git\git-bash.exe`.

2. Günlük Toplayıcı komut, Bash isteminde aşağıdaki komutları çalıştırarak yükleyin:

    ```Bash  
    mkdir -p $HOME/kuberneteslogs
    cd $HOME/kuberneteslogs
    curl -O https://raw.githubusercontent.com/msazurestackworkloads/azurestack-gallery/master/diagnosis/getkuberneteslogs.sh
    chmod 744 getkuberneteslogs.sh
    ```

3. Komut dosyası için gereken bilgileri arayın ve çalıştırın:

    | Parametre           | Açıklama                                                                                                      | Örnek                                                                       |
    |---------------------|------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------|
    | -d, --vmd-host      | Genel IP veya DVM tam etki alanı adını (FQDN). Sanal makine adı ile başlayan `vmd-`. | IP: 192.168.102.38<br>DNS: vmd-myk8s.local.cloudapp.azurestack.external |
    | -h, --help  | Komut kullanımını yazdırın. | |
    | -i,--dosya kimliği | RSA özel anahtar dosyası, Kubernetes kümesini oluştururken Market öğesine geçirildi. Kubernetes düğümleri uzaktan içinde gerekli. | C:\data\id_rsa.pem (Putty)<br>~/.ssh/id_rsa (SSH)
    | m-,--ana konak   | Genel IP veya Kubernetes ana düğümünün tam etki alanı adını (FQDN). Sanal makine adı ile başlayan `k8s-master-`. | IP: 192.168.102.37<br>FQDN: k8s-12345.local.cloudapp.azurestack.external      |
    | u-,--kullanıcı          | Kullanıcı adı, Kubernetes kümesini oluştururken Market öğesine geçirildi. Uzak öğesini Kubernetes düğümleri için gerekli | azureuser (varsayılan değer) |


   Parametre değerleriniz eklediğinizde, komutunuz şuna benzeyebilir:

    ```Bash  
    ./getkuberneteslogs.sh --identity-file "C:\id_rsa.pem" --user azureuser --vmd-host 192.168.102.37
     ```

4. Birkaç dakika sonra komut dosyasını toplanan günlükler adlı bir dizine çıkarır `KubernetesLogs_{{time-stamp}}`. Burada kümeye ait her bir sanal makine için bir dizin bulacaksınız.

    Günlük Toplayıcı betiği ayrıca günlük dosyalarındaki hataları aramak ve bilinen bir sorunu bulmak için olursa sorun giderme adımlarını içerir. Bilinen sorunlar bulma olasılığını artırmak için komut dosyasının en son sürümü çalıştırdığınızdan emin olun.

> [!Note]  
> Bu GitHub denetleyin [depo](https://github.com/msazurestackworkloads/azurestack-gallery/tree/master/diagnosis) günlük Toplayıcı betik hakkında daha fazla ayrıntı öğrenin.

## <a name="next-steps"></a>Sonraki adımlar

[Kubernetes için Azure Stack dağıtma](azure-stack-solution-template-kubernetes-deploy.md)

[Bir Kubernetes kümesi Market'te (Azure Stack operatörü için) ekleyin.](../azure-stack-solution-template-kubernetes-cluster-add.md)

[Azure’da Kubernetes](https://docs.microsoft.com/azure/container-service/kubernetes/container-service-kubernetes-walkthrough)
