---
title: Kümesinde Azure'da bir HPC Pack işleri gönderme | Microsoft Docs
description: Azure'daki bir HPC Pack kümesine göndermek için bir şirket içi bilgisayarı kurmayı öğrenin
services: virtual-machines-windows
documentationcenter: ''
author: dlepow
manager: jeconnoc
editor: ''
tags: azure-resource-manager,azure-service-management,hpc-pack
ms.assetid: 78f6833c-4aa6-4b3e-be71-97201abb4721
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-multiple
ms.workload: big-compute
ms.date: 05/14/2018
ms.author: danlep
ms.openlocfilehash: ce8e2457c1d575e890174de3b9cf7faf6e16a7cb
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/07/2018
ms.locfileid: "51258825"
---
# <a name="submit-hpc-jobs-from-an-on-premises-computer-to-an-hpc-pack-cluster-deployed-in-azure"></a>Şirket içindeki bir bilgisayardan Azure’da dağıtılmış bir HPC Pack kümesine HPC işleri gönderme
[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

İş göndermek için bir şirket içi istemci bilgisayarı yapılandırma bir [Microsoft HPC Pack](https://technet.microsoft.com/library/cc514029) azure'da kümesi. Bu makalede, yerel bir bilgisayar istemci araçları ile azure'da bir kümeye HTTPS üzerinden işi göndermek için nasıl ayarlanacağı gösterilmektedir. Bu şekilde, birden çok küme kullanıcı işleri bulut tabanlı bir HPC Pack kümesine, ancak doğrudan baş düğümü VM'sine bağlanma veya bir Azure aboneliğine erişim olmadan gönderebilirsiniz.

![Azure'daki bir kümeye bir iş gönderdiniz][jobsubmit]

## <a name="prerequisites"></a>Önkoşullar
* **Dağıtılan bir Azure VM'de HPC paketi üstbilgi düğümü** -otomatik Araçlar gibi kullanmanızı öneririz bir [Azure Hızlı Başlangıç şablonu](https://azure.microsoft.com/documentation/templates/) baş düğüm ve küme dağıtmak için. Baş düğümün DNS adı ve bu makaledeki adımları tamamlayabilmeniz için bir Küme Yöneticisi kimlik bilgileri gerekir.
* **İstemci bilgisayar** -HPC Pack istemci programları çalıştırmak üzere bir Windows veya Windows Server istemci bilgisayara ihtiyacı vardır (bkz [sistem gereksinimleri](https://technet.microsoft.com/library/dn535781.aspx)). Yalnızca işleri göndermek için HPC Pack web portalı veya REST API'sini kullanmak istiyorsanız, tercih ettiğiniz herhangi bir istemci bilgisayarda kullanabilirsiniz.
* **HPC Pack yükleme medyasında** - HPC paketi en son sürümünü kullanılabilir HPC Pack istemci yardımcı programlar, ücretsiz yükleme paketini yüklemek için [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=56360). HPC Pack, üstbilgi düğüm VM'ine yüklenir aynı sürümünü indirdiğinizden emin olun.

## <a name="step-1-install-and-configure-the-web-components-on-the-head-node"></a>1. adım: Yükleme ve web bileşenleri baş düğümde yapılandırın
HTTPS üzerinden kümeye işleri göndermek bir REST arabirimini etkinleştirmek için HPC Pack web bileşenleri HPC paketi üstbilgi düğümü üzerinde yapılandırıldığından emin olun. Bunlar zaten yüklü değilse, öncelikle HpcWebComponents.msi yükleme dosyasını çalıştırarak web bileşenleri yükleyin. Ardından, HPC PowerShell betiğini çalıştırarak bileşenlerini yapılandırma **kümesi HPCWebComponents.ps1**.

Ayrıntılı yordamlar için bkz: [Microsoft HPC Pack Web bileşenlerini yükleme](https://technet.microsoft.com/library/hh314627.aspx).

> [!TIP]
> HPC Pack kümeleri için belirli Azure hızlı başlangıç şablonları, yükleyin ve web bileşenleri otomatik olarak yapılandırın.
> 
> 

**Web bileşenleri yüklemek için**

1. Baş düğüme sanal makine bir Küme Yöneticisi kimlik bilgilerini kullanarak bağlanın.
1. HPC Pack kurulum klasöründen HpcWebComponents.msi baş düğümünde çalıştırın.
1. Web bileşenleri yüklemek için sihirbazdaki adımları izleyin

**Web bileşenleri yapılandırmak için**

1. Baş düğümde HPC PowerShell'i yönetici başlatın.
1. Yapılandırma betiğini konumuna dizini değiştirmek için aşağıdaki komutu yazın:
   
    ```powershell
    cd $env:CCP_HOME\bin
    ```
1. REST arabirimi yapılandırmak ve HPC Web Hizmeti'ni başlatmak için aşağıdaki komutu yazın:
   
    ```powershell
    .\Set-HPCWebComponents.ps1 –Service REST –enable
    ```
1. Bir sertifika seçmeniz istendiğinde, baş düğümün genel DNS adı için karşılık gelen sertifikayı seçin. Örneğin, baş düğüm Klasik dağıtım modeli kullanılarak VM dağıtırsanız, sertifika adı CN gibi görünür =&lt;*HeadNodeDnsName*&gt;. cloudapp.net. Resource Manager dağıtım modelini kullanıyorsanız, sertifika adı CN gibi görünüyor. =&lt;*HeadNodeDnsName*&gt;.&lt; *bölge*&gt;. cloudapp.azure.com.
   
   > [!NOTE]
   > Bir şirket içi bilgisayardan baş düğüme işleri gönderdiğinizde, bu sertifika daha sonra seçin. Seçin veya baş düğüm Active Directory etki alanında bilgisayar adı için karşılık gelen bir sertifika yapılandırmanız yoksa (örneğin, CN =*MyHPCHeadNode.HpcAzure.local*).
   > 
   > 
1. İş gönderme web portalını yapılandırmak için aşağıdaki komutu yazın:
   
    ```powershell
    .\Set-HPCWebComponents.ps1 –Service Portal -enable
    ```
1. Betik tamamlandıktan sonra durdurmak ve aşağıdaki komutları yazarak HPC İş Zamanlayıcısı hizmeti yeniden başlatın:
   
    ```powershell
    net stop hpcscheduler
    net start hpcscheduler
    ```

## <a name="step-2-install-the-hpc-pack-client-utilities-on-an-on-premises-computer"></a>2. adım: bir şirket içi bilgisayarda HPC Pack istemci yardımcı programlarını yükleyin.
HPC Pack istemci programları bilgisayarınıza yüklemek istiyorsanız, HPC paketi Kurulum dosyaları (tam yükleme) indirin [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=56360). Yükleme başladığınızda için kurulum seçeneğini **HPC Pack istemci programları**.

Üstbilgi düğüm VM'ine işleri göndermek için HPC Pack istemci araçlarını kullanmak için ayrıca baş düğümünden bir sertifikayı dışarı aktarma ve istemci bilgisayara yüklemeniz gerekir. Sertifika olması gerekir. CER biçiminde.

**Baş düğümünden sertifikasını dışarı aktarmak için**

1. Baş düğümde yerel bilgisayar hesabı için bir Microsoft Yönetim Konsolu için Sertifikalar ek bileşenini ekleyin. Ek bileşenini ekleme adımları için bkz. [MMC'ye Sertifikalar ek bileşenini ekleme](https://technet.microsoft.com/library/cc754431.aspx).
1. Konsol ağacında genişletin **sertifikalar-yerel bilgisayar** > **kişisel**ve ardından **sertifikaları**.
1. HPC Pack web bileşenleri için yapılandırılmış sertifikayı bulun [1. adım: yükleme ve web bileşenleri baş düğümünde yapılandırma](#step-1-install-and-configure-the-web-components-on-the-head-node) (örneğin, CN =&lt;*HeadNodeDnsName* &gt;. cloudapp.net).
1. Sertifikaya sağ tıklayın ve **tüm görevler** > **dışarı**.
1. Sertifika Dışarı Aktarma Sihirbazı'nda tıklatın **sonraki**, olduğundan emin olun **Hayır, özel anahtarı verme** seçilir.
1. DER ile kodlanmış ikili X.509 sertifika vermek için sihirbazın kalan adımları izleyin (. CER) biçimi.

**İstemci bilgisayara sertifikayı içeri aktarmak için**

1. İstemci bilgisayardaki bir klasöre baş düğümünden dışarı aktardığınız sertifikayı kopyalayın.
1. İstemci bilgisayarda certmgr.msc çalıştırın.
1. Sertifika Yöneticisi'nde **Sertifikalar – Geçerli kullanıcı** > **güvenilen kök sertifika yetkilileri**, sağ **sertifikaları**ve ardından tıklayın **tüm görevler** > **alma**.
1. Sertifika Alma Sihirbazı'nda tıklatın **sonraki** ve güvenilen kök sertifika yetkilileri deposuna baş düğümünden dışarı aktardığınız sertifikayı içeri aktarmak için aşağıdaki adımları izleyin.

> [!TIP]
> Baş düğümüne sertifika yetkilisinde istemci bilgisayar tarafından tanınmadığından bir güvenlik uyarısı görebilirsiniz. Test amacıyla, bu uyarıyı yok sayın ve sertifika alma işlemi.
> 
> 

## <a name="step-3-run-test-jobs-on-the-cluster"></a>3. adım: Çalıştırma test işleri küme üzerinde
Yapılandırmanızı doğrulamak için şirket içi bilgisayardan azure'da kümede işleri çalıştırmayı deneyin. Örneğin, küme iş göndermek için HPC Pack GUI araçları veya komut satırı komutlarını kullanabilirsiniz. Web tabanlı bir portal, iş göndermek için de kullanabilirsiniz.

**İstemci bilgisayarda iş gönderme komutları çalıştırmak için**

1. HPC Pack istemci programları yüklendiği bir istemci bilgisayarda bir komut istemi başlatın.
1. Bir örnek komutu yazın. Örneğin, kümedeki tüm işleri listelemek için baş düğümün tam DNS adı bağlı olarak aşağıdakilerden birini benzeyen bir komut yazın:
   
    ```command
    job list /scheduler:https://<HeadNodeDnsName>.cloudapp.net /all
    ```
   
    or
   
    ```command
    job list /scheduler:https://<HeadNodeDnsName>.<region>.cloudapp.azure.com /all
    ```
   
   > [!TIP]
   > Baş düğüm, IP adresi değil tam DNS adını Zamanlayıcı URL'yi kullanın. IP adresi belirtirseniz, bir hata "sunucu sertifikasının geçerli bir güven zincirine sahip olmalı veya güvenilen kök deposunda yerleştirilecek gerekiyor." benzer görünür
   > 
   > 
1. İstendiğinde kullanıcı adını yazın (biçiminde &lt;DomainName&gt;\\&lt;kullanıcıadı&gt;) HPC Küme Yöneticisi veya yapılandırdığınız başka bir küme kullanıcı adını ve parolasını. Daha fazla iş işlemi için yerel kimlik bilgilerini depolamak üzere seçebilirsiniz.
   
    İşlerinin listesi görüntülenir.

**İstemci bilgisayarda HPC İş Yöneticisi'ni kullanmak için**

1. Daha önce etki alanı kimlik bilgilerini bir küme kullanıcısı için bir iş gönderirken depoladığınız alamadık, kimlik bilgisi Yöneticisi'nde kimlik bilgileri ekleyebilirsiniz.
   
    a. İstemci bilgisayardaki Denetim Masası'ndaki kimlik bilgisi Yöneticisi'ni başlatın.
   
    b. Tıklayın **Windows kimlik bilgileri** > **genel kimlik bilgisi Ekle**.
   
    c. Internet adresini belirtin (örneğin, https://&lt;HeadNodeDnsName&gt;.cloudapp.net/HpcScheduler veya https://&lt;HeadNodeDnsName&gt;.&lt; Bölge&gt;.cloudapp.azure.com/HpcScheduler) ve kullanıcı adını (&lt;DomainName&gt;\\&lt;kullanıcıadı&gt;) ve parolasını veya başka bir Küme Yöneticisi yapılandırdığınız kullanıcı kümesi.
1. İstemci bilgisayarda, HPC İş Yöneticisi'ni başlatın.
1. İçinde **baş düğüm Seç** iletişim kutusunda, Azure'da baş düğüm URL'sini yazın (örneğin, https://&lt;HeadNodeDnsName&gt;. cloudapp.net veya https://&lt;HeadNodeDnsName&gt;.&lt; Bölge&gt;. cloudapp.azure.com).
   
    HPC İş Yöneticisi'ni açar ve baş düğüme işlerin bir listesini gösterir.

**Baş düğüm üzerinde çalışan web portalı kullanmak için**

1. İstemci bilgisayarda bir web tarayıcısını Başlat ve baş düğümün tam DNS adı bağlı olarak şu adreslerden birini girin:
   
    ```
    https://<HeadNodeDnsName>.cloudapp.net/HpcPortal
    ```
   
    or
   
    ```
    https://<HeadNodeDnsName>.<region>.cloudapp.azure.com/HpcPortal
    ```
1. Görünen güvenlik iletişim kutusunda, HPC Küme Yöneticisi etki alanı kimlik bilgilerini yazın. (Diğer küme kullanıcıları da farklı rolleri ekleyebilirsiniz. Bkz: [küme kullanıcıları yönetme](https://technet.microsoft.com/library/ff919335.aspx).)
   
    Web portalı için iş listesi görünümü açılır.
1. "Merhaba Dünya" dizesini kümeden döndüren örnek iş göndermek için tıklatın **yeni iş** sol taraftaki gezinti.
1. Üzerinde **yeni iş** sayfasındaki **gönderimi sayfalarından**, tıklayın **HelloWorld**. İş gönderme sayfası açılır.
1. Tıklayın **gönderme**. İstenirse, HPC Küme Yöneticisi etki alanı kimlik bilgilerini sağlayın. İş gönderilir ve iş kimliği görünür **İşlerim** sayfası.
1. Gönderdiğiniz iş sonuçlarını görüntülemek için iş kimliği'ni tıklatın ve ardından **görünümü görevleri** komut çıktısını görüntülemek için (altında **çıkış**).

## <a name="next-steps"></a>Sonraki adımlar
* Azure kümesine ile işleri de gönderebilirsiniz [HPC Pack REST API'si](https://social.technet.microsoft.com/wiki/contents/articles/7737.creating-and-submitting-jobs-by-using-the-rest-api-in-microsoft-hpc-pack-windows-hpc-server.aspx).
* Python örnek, bir Linux istemcisinden küme işleri göndermek istiyorsanız, bkz [HPC Pack 2012 R2 SDK'sı ve örnek kod](https://www.microsoft.com/download/details.aspx?id=41633).

<!--Image references-->
[jobsubmit]: ./media/virtual-machines-windows-hpcpack-cluster-submit-jobs/jobsubmit.png
