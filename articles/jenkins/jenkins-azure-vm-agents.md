---
title: "Jenkins ile sürekli tümleştirme için Azure VM aracılarını kullanın."
description: "Jenkins alt sunucuları olarak Azure VM aracıları."
services: multiple
documentationcenter: 
author: mlearned
manager: douge
editor: 
ms.assetid: 
ms.service: multiple
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 6/7/2017
ms.author: mlearned
ms.custom: mvc
ms.translationtype: Human Translation
ms.sourcegitcommit: 1e6f2b9de47d1ce84c4043f5f6e73d462e0c1271
ms.openlocfilehash: 5f2df414b4d0e8798b7ed6d90d0ea0fb79d42fc2
ms.contentlocale: tr-tr
ms.lasthandoff: 06/21/2017

---
<a id="use-azure-vm-agents-for-continuous-integration-with-jenkins" class="xliff"></a>

# Jenkins ile sürekli tümleştirme için Azure VM aracılarını kullanın.

Bu hızlı başlangıç, Azure’da isteğe bağlı bir Linux (Ubuntu) aracısı oluşturmak üzere Jenkins Azure VM Aracıları eklentisinin nasıl kullanılacağını gösterir.

<a id="prerequisites" class="xliff"></a>

## Ön koşullar

Bu hızlı başlangıcı tamamlamak için:

* Henüz bir Jenkins ana sunucunuz yoksa, [Çözüm Şablonu](install-jenkins-solution-template.md) ile başlayabilirsiniz 
* Henüz bir Azure hizmet sorumlunuz yoksa, [Azure CLI 2.0 ile Azure Hizmet sorumlusu oluşturma](https://docs.microsoft.com/en-us/cli/azure/create-an-azure-service-principal-azure-cli?toc=%2fazure%2fazure-resource-manager%2ftoc.json) bölümüne bakın.

<a id="install-azure-vm-agents-plugin" class="xliff"></a>

## Azure VM Aracıları eklentisini yükleme

İşleme [Çözüm Şablonu](install-jenkins-solution-template.md) ile başlarsanız, Azure VM Aracısı eklentisi Jenkins ana sunucusuna yüklenir.

Aksi takdirde, **Azure VM Aracısı** eklentisini Jenkins panosundan yükleyin.

<a id="configure-the-plugin" class="xliff"></a>

## Eklentiyi yapılandırma

* Jenkins panosunda **Jenkins’i Yönet -> Sistem Yapılandırma ->** öğesine tıklayın. Sayfanın en altına kadar kaydırın ve **Yeni bulut ekle** açılır listesini içeren bölümü bulun. Menüden **Microsoft Azure VM Aracıları**’nı seçin
* Azure Kimlik Bilgiler açılır listesinden mevcut bir hesabı seçin.  Yeni bir **Microsoft Azure Hizmet Sorumlusu** eklemek için şu değerleri girin: Abonelik Kimliği, İstemci Kimliği, İstemci Gizli Anahtarı ve OAuth 2.0 Belirteç Uç Noktası.

![Azure Kimlik Bilgileri](./media/jenkins-azure-vm-agents/service-principal.png)

* Profil yapılandırmasının doğru olduğundan emin olmak için **Yapılandırmayı doğrula** öğesine tıklayın.
* Yapılandırmayı kaydedip sonraki adıma geçin.

<a id="template-configuration" class="xliff"></a>

## Şablon yapılandırması

<a id="general-configuration" class="xliff"></a>

### Genel yapılandırma
Bundan sonra, bir Azure VM aracısı tanımlamak için kullanılacak şablonu yapılandırın. 

* Şablon eklemek için **Ekle**’ye tıklayın. 
* Şablonunuza bir ad verin. 
* Etiket için "ubuntu" yazın. Bu etiket, iş yapılandırması sırasında kullanılır.
* Birleşik giriş kutusundan istediğiniz bölgeyi seçin.
* İstenen VM boyutunu seçin.
* Azure Depolama hesabı adını belirtin veya adı "jenkinsarmst" varsayılan adını kullanmak için boş bırakın.
* Bekletme süresini dakika cinsinden belirtin. Bu ayar, boştaki bir aracıyı otomatik olarak silmeden önce Jenkins’in kaç dakika bekleyeceğini belirler. Boştaki aracıların otomatik olarak silinmesini istemiyorsanız 0 değerini belirtin.

![Genel yapılandırma](./media/jenkins-azure-vm-agents/general-config.png)

<a id="image-configuration" class="xliff"></a>

### Görüntü yapılandırma

Bir Linux (Ubuntu) aracısı oluşturmak için **Görüntü başvurusu**’nu seçin ve örnek olarak aşağıdaki yapılandırmayı kullanın. Azure tarafından desteklenen en son görüntüler için [Azure Market](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/category/compute?subcategories=virtual-machine-images&page=1)’e bakın.

* Görüntü Yayımcısı: Canonical
* Görüntü Teklifi: UbuntuServer
* Görüntü Sku’su: 14.04.5-LTS
* Görüntü sürümü: en son
* İşletim Sistemi Türü: Linux
* Başlatma yöntemi: SSH
* Yönetici kimlik bilgilerini sağlama
* VM başlatma betiği için şunu girin:
```
# Install Java
sudo apt-get -y update
sudo apt-get install -y openjdk-7-jdk
sudo apt-get -y update --fix-missing
sudo apt-get install -y openjdk-7-jdk
```
![Görüntü yapılandırma](./media/jenkins-azure-vm-agents/image-config.png)

* Yapılandırmayı doğrulamak için **Şablonu Doğrula**’ya tıklayın.
* **Kaydet** düğmesine tıklayın.

<a id="create-a-job-in-jenkins" class="xliff"></a>

## Jenkins içinde iş oluşturma

* Jenkins panosunda **Yeni Öğe**’ye tıklayın. 
* Bir ad girip **Serbest tarzda proje**’yi seçin ve **Tamam**’a tıklayın.
* **Genel** sekmesinde "Projenin nerede çalıştırılabileceğini kısıtla" öğesini seçip Etiket İfadesinde "ubuntu" yazın. Bu durumda açılır listede "ubuntu" seçeneğini görürsünüz.
* **Kaydet** düğmesine tıklayın.

![İşi ayarlama](./media/jenkins-azure-vm-agents/job-config.png)

<a id="build-your-new-project" class="xliff"></a>

## Yeni projenizi derleme

* Jenkins panosuna geri dönün.
* Yeni oluşturduğunuz işe sağ tıkladıktan sonra **Şimdi derle**’ye tıklayın. Derleme başlatılır. 
* Derleme tamamlandıktan sonra **Konsol çıktısı**’na gidin. Derlemenin Azure üzerinde uzaktan gerçekleştirildiğini görürsünüz.

![Konsol çıktısı](./media/jenkins-azure-vm-agents/console-output.png)

<a id="reference" class="xliff"></a>

## Başvuru

* Azure Cuma videosu: [Azure VM aracılarını kullanarak Jenkins ile Sürekli Tümleştirme](https://channel9.msdn.com/Shows/Azure-Friday/Continuous-Integration-with-Jenkins-Using-Azure-VM-Agents)
* Destek bilgileri ve yapılandırma seçenekleri: [Azure VM Aracısı Jenkins Eklentisi Wiki](https://wiki.jenkins-ci.org/display/JENKINS/Azure+VM+Agents+Plugin) 


