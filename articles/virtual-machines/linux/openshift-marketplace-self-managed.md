---
title: OpenShift kapsayıcı platformu kendi kendini yöneten bir Market teklifi azure'da dağıtma | Microsoft Docs
description: Azure'da OpenShift kapsayıcı platformu kendi kendine yönetilen Market teklifi dağıtın.
services: virtual-machines-linux
documentationcenter: virtual-machines
author: haroldwongms
manager: mdotson
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/7/2019
ms.author: haroldw
ms.openlocfilehash: 9b981924dcaf715dd1d05d452b756a40b63f8dac
ms.sourcegitcommit: 2ce4f275bc45ef1fb061932634ac0cf04183f181
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2019
ms.locfileid: "65233089"
---
# <a name="configure-prerequisites"></a>Önkoşulları yapılandırma

Azure'da kendi kendini yöneten bir OpenShift kapsayıcı platformu kümesine dağıtmak için Market teklifi kullanmadan önce bazı Önkoşullar yapılandırılması gerekir.  Okuma [OpenShift önkoşulları](https://docs.microsoft.com/azure/virtual-machines/linux/openshift-prerequisites) makale oluşturma yönergeleri bir ssh anahtarı (olmadan, bir parola), Azure anahtar kasası, anahtar kasası gizli dizi ve bir hizmet sorumlusu.

 
## <a name="deploy-using-the-marketplace-offer"></a>Market teklifi kullanarak dağıtma

En basit yolu, bir kendi kendini yöneten Azure OpenShift kapsayıcı platformu kümesi dağıtmayı kullanmaktır [Azure Marketi'nde teklif](https://azuremarketplace.microsoft.com/marketplace/apps/redhat.openshift-container-platform?tab=Overview).

Bu basit bir seçenektir, ancak özelleştirme özellikleri sınırlıdır. Market teklifi, OpenShift kapsayıcı platformu 3.11.82 dağıtır ve aşağıdaki yapılandırma seçeneklerini içerir:

- **Ana düğümler**: Yapılandırılabilir örneği ile ana düğümler üç (3) yazın.
- **Kızılötesi düğümleri**: Üç (3) altyapısı düğümleri yapılandırılabilir örneği ile yazın.
- **Düğümleri**: Düğüm sayısını (1 ila 9) ve örnek türüne yapılandırılabilir özelliktedir.
- **Disk türü**: Yönetilen diskler kullanılır.
- **Ağ**: Yeni veya var olan ağ ve özel CIDR aralığı desteği.
- **CNS**: CNS etkinleştirilebilir.
- **Ölçümleri**: Hawkular ölçümleri etkinleştirilebilir.
- **Günlüğe kaydetme**: EFK günlüğe kaydetme etkinleştirilebilir.
- **Azure bulut sağlayıcısı**: Varsayılan olarak etkin, devre dışı bırakılabilir.

Azure portalının üst sol tıklayın **kaynak Oluştur**'openshift kapsayıcı platformu' arama kutusuna girin ve Enter tuşuna basın.

   ![Yeni kaynak arama](media/openshift-marketplace-self-managed/ocp-search.png)  
<br>

Sonuçlar sayfası ile açılır **Red Hat OpenShift kapsayıcı platformu Self-Managed** listesinde. 

   ![Yeni kaynak arama sonucu](media/openshift-marketplace-self-managed/ocp-searchresult.png)  
<br>

Teklif Teklif ayrıntılarını görüntülemek için tıklayın. Bu teklif dağıtmak için **Oluştur**. Gerekli parametreleri girmek için kullanıcı Arabirimi görüntülenir. İlk ekranda **Temelleri** dikey penceresi.

   ![Teklif başlığı sayfası](media/openshift-marketplace-self-managed/ocp-titlepage.png)  
<br>

**Temel Bilgiler**

Tüm giriş parametrelerini Yardım almak için üzerine ***miyim*** parametre adının yanında.

Giriş parametreleri için değerleri girin ve tıklayın **Tamam**.

| Giriş parametresi | Parametre açıklaması |
|-----------------------|-----------------|
| Sanal makine yönetici kullanıcı adı | Tüm VM örneklerine oluşturulması için yönetici kullanıcı |
| SSH ortak anahtarı için yönetici kullanıcı | SSH ortak anahtarı - VM'de oturum açmak için kullanılan bir parola değil olmalıdır |
| Abonelik | Kümesine dağıtmak için azure aboneliği |
| Kaynak Grubu | Yeni bir kaynak grubu oluşturun veya var olan boş bir kaynak grubu için küme kaynaklarını seçin |
| Location | Kümesine dağıtmak için azure bölgesi |

   ![Teklif temel bilgiler dikey penceresi](media/openshift-marketplace-self-managed/ocp-basics.png)  
<br>

**Altyapı ayarları**

Giriş parametreleri için değerleri girin ve tıklayın **Tamam**.

| Giriş parametresi | Parametre açıklaması |
|-----------------------|-----------------|
| OCP küme adı ön eki | Tüm düğümleri için konak adları yapılandırmak için kullanılan önek küme. 1 ila 20 karakter |
| Ana düğüm boyutu | Varsayılan VM boyutu kabul edin veya **değiştirme boyutu** başka bir VM boyutu seçin.  İş yükünüz için uygun VM boyutunu seçin |
| Altyapı düğüm boyutu | Varsayılan VM boyutu kabul edin veya **değiştirme boyutu** başka bir VM boyutu seçin.  İş yükünüz için uygun VM boyutunu seçin |
| Uygulama düğüm sayısı | Varsayılan VM boyutu kabul edin veya **değiştirme boyutu** başka bir VM boyutu seçin.  İş yükünüz için uygun VM boyutunu seçin |
| Uygulama düğüm boyutu | Varsayılan VM boyutu kabul edin veya **değiştirme boyutu** başka bir VM boyutu seçin.  İş yükünüz için uygun VM boyutunu seçin |
| Savunma ana bilgisayar boyutu | Varsayılan VM boyutu kabul edin veya **değiştirme boyutu** başka bir VM boyutu seçin.  İş yükünüz için uygun VM boyutunu seçin |
| Yeni veya mevcut bir sanal ağ | (Varsayılan) yeni bir vNet oluşturun veya mevcut bir Vnet'i kullanma |
| Varsayılan CIDR ayarları seçin veya IP aralığı (CIDR) özelleştirme | Varsayılan CIDR aralıkları veya Select kabul **özel IP aralığı** CIDR bir özel bilgiyi girin.  Varsayılan ayarları vNet 10.0.0.0/14, ana alt ağ ile 10.1.0.0/16 ınfra, CIDR alt ağ ile 10.2.0.0/16 yanı sıra, işlem ve cns alt ağ ile 10.3.0.0/16 oluşturur |
| Key Vault kaynak grubu adı | Key Vault içeren kaynak grubunun adı |
| Key Vault Adı | Gizli dizi ile içeren anahtar kasası adını ssh özel anahtarı.  Yalnızca alfasayısal karakterler ve tire izin verilir ve 3 ila 24 karakter arasında olmalı |
| Gizli dizi adı | İçeren gizli dizi adı ssh özel anahtarı.  Yalnızca alfasayısal karakterler ve tire izin verilir |

   ![Teklif altyapı dikey penceresi](media/openshift-marketplace-self-managed/ocp-inframain.png)  
<br>

**Boyutunu değiştir**

Başka bir VM boyutu seçmek için tıklatın ***değiştirme boyutu***.  Sanal makine seçimi penceresi açılacaktır.  İstediğiniz VM boyutu seçin **seçin**.

   ![VM boyutunu seçin](media/openshift-marketplace-self-managed/ocp-selectvmsize.png)  
<br>

**Var olan sanal ağı**

| Giriş parametresi | Parametre açıklaması |
|-----------------------|-----------------|
| Var olan sanal ağ adı | Mevcut vNet adı |
| Ana düğüm için alt ağ adı | Ana düğüm için mevcut bir alt ağ adı.  RFC 1918 izleyin ve en az 16 IP adreslerini içerecek gerekiyor |
| Alt ağ adı için ınfra düğümleri | Adı için alt ağ altyapısı düğüm mevcut.  RFC 1918 izleyin ve en az 32 IP adreslerini içerecek gerekiyor |
| İşlem ve cns düğümleri için alt ağ adı | İşlem ve cns düğümleri için var olan bir alt ağ adı.  RFC 1918 izleyin ve en az 32 IP adreslerini içerecek gerekiyor |
| Var olan sanal ağın kaynak grubu | Mevcut vNet içeren kaynak grubunun adı |

   ![Var olan sanal ağ altyapısı sunar.](media/openshift-marketplace-self-managed/ocp-existingvnet.png)  
<br>

**Özel IP aralığı**

| Giriş parametresi | Parametre açıklaması |
|-----------------------|-----------------|
| Sanal ağın adres aralığı | Sanal ağ için CIDR özel |
| Ana düğüm içeren alt ağın adres aralığı | Ana alt ağ için CIDR özel |
| Altyapı düğümleri içeren alt ağ adres aralığı | Altyapı alt ağ için CIDR özel |
| İşlem ve cns düğümleri içeren bir alt ağ adres aralığı | İşlem ve cns düğümler için özel CIDR |

   ![Altyapı özel IP aralığı sunar](media/openshift-marketplace-self-managed/ocp-customiprange.png)  
<br>

**OpenShift kapsayıcı platformu**

Giriş parametreleri için değerleri girin ve tıklayın **Tamam**

| Giriş parametresi | Parametre açıklaması |
|-----------------------|-----------------|
| OpenShift yönetici kullanıcı parolası | İlk OpenShift kullanıcı parolası.  Bu kullanıcı aynı zamanda küme yönetim olacaktır |
| OpenShift yönetici kullanıcı parolası onaylama | OpenShift yönetici kullanıcı parolayı yeniden yazın |
| Red Hat Abonelik Yöneticisi kullanıcı adı | Red Hat aboneliklerini veya kuruluş kimliği erişmek için kullanıcı adı  Bu kimlik bilgisi aboneliğinize RHEL örneğini kaydetmek için kullanılır ve Microsoft veya Red Hat tarafından depolanmaz |
| Red Hat Abonelik Yöneticisi kullanıcı parolası | Red Hat aboneliklerini veya etkinleştirme anahtarı erişmek için parola.  Bu kimlik bilgisi aboneliğinize RHEL örneğini kaydetmek için kullanılır ve Microsoft veya Red Hat tarafından depolanmaz |
| Red Hat Abonelik Yöneticisi OpenShift Havuz kimliği | OpenShift kapsayıcı platformu yetkilendirmesini içeren Havuz kimliği. OpenShift kapsayıcı platformu Küme yükleme için yeterli yetkilendirmelerini sahip olduğunuzdan emin olun |
| Red Hat Abonelik Yöneticisi OpenShift Havuz kimliği aracısının / ana düğümler | Havuz aracısının OpenShift kapsayıcı platformu yetkilendirmeleri içeren kimliği / ana düğümleri. OpenShift kapsayıcı platformu Küme yükleme için yeterli yetkilendirmelerini olduğundan emin olun. Aracısı kullanılmıyorsa / Ana Havuz kimliği, uygulama düğümleri için havuz kimliği girin |
| Azure bulut sağlayıcısı yapılandırma | OpenShift Azure bulut sağlayıcısı kullanmak için yapılandırın. Azure disk kullanarak kalıcı birimleri eklerseniz gereklidir.  Varsayılan değer evet'tir |
| Azure AD hizmet sorumlusu istemci kimliği GUID | Azure AD hizmet sorumlusu istemci kimliği GUID - AppID olarak da bilinir. Azure bulut sağlayıcısını Yapılandır ayarlarsanız yalnızca gerekli **Evet** |
| Azure AD hizmet sorumlusu istemci kimliği parolası | Azure AD hizmet sorumlusu istemci kimliği parolası. Azure bulut sağlayıcısını Yapılandır ayarlarsanız yalnızca gerekli **Evet** |
 
   ![Teklif OpenShift dikey penceresi](media/openshift-marketplace-self-managed/ocp-ocpmain.png)  
<br>

**Ek ayarlar**

Ek ayarlar dikey penceresinde CNS glusterfs depolama yapılandırmasını sağlar. günlük, ölçüm ve yönlendirici alt etki alanı.  Varsayılan, bu seçeneklerden birini yüklenmiyor ve nip.io yönlendirici alt etki alanı test amacıyla kullanır. CNS etkinleştirme üç ek işlem düğümleri, glusterfs pod'ları barındıracak üç ek bağlı diskleri olan yükler.  

Giriş parametreleri için değerleri girin ve tıklayın **Tamam**

| Giriş parametresi | Parametre açıklaması |
|-----------------------|-----------------|
| Kapsayıcı yerel depolama (CNS) yapılandırma | OpenShift CNS yüklenir, küme ve depolama alanı olarak etkinleştirin. Azure sağlayıcısı devre dışı bırakılırsa varsayılan olacaktır |
| Küme günlük tutmayı yapılandırma | EFK günlüğü işlevini kümesine yükler.  Altyapısı uygun konak EFK pod'ların düğüm boyutu |
| Küme için ölçümleri yapılandırma | Hawkular ölçümleri OpenShift kümesine yükler.  Altyapısı uygun konak Hawkular ölçümleri pod'ların düğüm boyutu |
| Varsayılan yönlendirici alt etki alanı | Test veya üretim için kendi alt etki alanı girmeniz özel nipio seçin |
 
   ![Teklif ek dikey penceresi](media/openshift-marketplace-self-managed/ocp-additionalmain.png)  
<br>

**Ek ayarları - ek parametreler**

| Giriş parametresi | Parametre açıklaması |
|-----------------------|-----------------|
| (CNS) Düğüm boyutu | Varsayılan düğüm boyutu kabul edin veya seçin **değiştirme boyutu** yeni bir VM boyutu seçin |
| Kendi özel alt etki alanı girin | OpenShift kümede yönlendirici üzerinden uygulamaları açığa çıkarmak için kullanılacak özel Yönlendirme etki alanı.  Uygun bir joker karakter DNS girişini oluşturmak mutlaka] |
 
   ![Ek cns teklif yükleyin](media/openshift-marketplace-self-managed/ocp-additionalcnsall.png)  
<br>

**Özet**

Bu aşamada, çekirdek kotası toplam sayısı küme için seçilen Vm'leri dağıtmak yeterli olduğunu denetlemek için doğrulama gerçekleşir.  Girilen tüm parametreleri gözden geçirin.  Girişleri kabul edilebilir ise tıklayın **Tamam** devam etmek için.

   ![Teklif özeti dikey penceresi](media/openshift-marketplace-self-managed/ocp-summary.png)  
<br>

**Satın Al**

Satın alma sayfasında kişi bilgilerini doğrulayın ve tıklatın **satın alma** kullanım koşullarını kabul edin ve OpenShift kapsayıcı platformu kümesi dağıtımını başlatmak için.

   ![Teklifi satın alma dikey penceresi](media/openshift-marketplace-self-managed/ocp-purchase.png)  
<br>


## <a name="connect-to-the-openshift-cluster"></a>OpenShift kümeye bağlanma

Dağıtım tamamlandığında, bağlantı, dağıtım çıktı bölümünden alın. OpenShift konsoluna tarayıcınızla kullanarak bağlanma **OpenShift Konsolu URL'si**. Ayrıca savunma ana bilgisayara SSH kullanabilirsiniz. Yönetici kullanıcı adı clusteradmin ve savunma genel IP DNS FQDN bastiondns4hawllzaavu6g.eastus.cloudapp.azure.com olduğu yerde bir örneği verilmiştir:

```bash
$ ssh clusteradmin@bastiondns4hawllzaavu6g.eastus.cloudapp.azure.com
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Kullanım [az grubu Sil](/cli/azure/group) komutunu kullanarak kaynak grubunu, OpenShift küme kaldırmak için ve artık gerekmediğinde tüm ilgili kaynakları.

```azurecli 
az group delete --name openshiftrg
```

## <a name="next-steps"></a>Sonraki adımlar

- [Dağıtım sonrası görevler](./openshift-post-deployment.md)
- [Azure'da OpenShift dağıtım sorunlarını giderme](./openshift-troubleshooting.md)
- [OpenShift kapsayıcı platformu ile çalışmaya başlama](https://docs.openshift.com/container-platform)

