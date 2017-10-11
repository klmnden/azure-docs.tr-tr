---
title: "HPC Pack işlerini küme Azure'da gönderme | Microsoft Docs"
description: "Azure'da bir HPC Pack kümeye iş göndermek için bir şirket içi bilgisayarı ayarlama öğrenin"
services: virtual-machines-windows
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: azure-resource-manager,azure-service-management,hpc-pack
ms.assetid: 78f6833c-4aa6-4b3e-be71-97201abb4721
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-multiple
ms.workload: big-compute
ms.date: 10/14/2016
ms.author: danlep
ms.openlocfilehash: d5953f1e1dd2deb4d871bd67352a6a5b2ae13dbf
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="submit-hpc-jobs-from-an-on-premises-computer-to-an-hpc-pack-cluster-deployed-in-azure"></a>Şirket içindeki bir bilgisayardan Azure’da dağıtılmış bir HPC Pack kümesine HPC işleri gönderme
[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

İşlerini göndermek için bir şirket içi istemci bilgisayarı yapılandırmak bir [Microsoft HPC Pack](https://technet.microsoft.com/library/cc514029) Azure kümesinde. Bu makalede bir yerel bilgisayarda istemci araçları ile azure'da kümeye HTTPS üzerinden işi göndermek için nasıl ayarlanacağını gösterir. Bu şekilde, birden çok küme kullanıcı işleri bulut tabanlı bir HPC Pack kümeye ancak doğrudan baş düğümüne VM bağlanmak veya bir Azure aboneliği erişme olmadan gönderebilirsiniz.

![Azure'da bir küme için bir iş gönderme][jobsubmit]

## <a name="prerequisites"></a>Ön koşullar
* **Bir Azure VM dağıtılan HPC paketi üstbilgi düğümü** -otomatik araçları gibi kullanmanızı öneririz bir [Azure Hızlı Başlangıç şablonu](https://azure.microsoft.com/documentation/templates/) veya bir [Azure PowerShell Betiği](classic/hpcpack-cluster-powershell-script.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json) baş düğümü dağıtma ve Küme. Baş düğüm DNS adını ve bu makaledeki adımları tamamlamak için bir Küme Yöneticisi kimlik bilgileri gerekir.
* **İstemci bilgisayar** -HPC Pack istemci yardımcı programları çalıştırmak bir Windows veya Windows Server istemci bilgisayar gerekir (bkz [sistem gereksinimleri](https://technet.microsoft.com/library/dn535781.aspx)). Yalnızca iş göndermek için HPC Pack web portalı veya REST API'yi kullanmak istiyorsanız, tercih ettiğiniz herhangi bir istemci bilgisayarı kullanabilirsiniz.
* **HPC Pack yükleme medyasını** - HPC Pack (HPC Pack 2012 R2) en son sürümünü kullanılabilir HPC Pack istemci yardımcı programları, ücretsiz yükleme paketini yüklemek için [Microsoft Download Center](http://go.microsoft.com/fwlink/?LinkId=328024). VM baş düğümünde yüklü HPC Pack aynı sürümünü yüklediğinizden emin olun.

## <a name="step-1-install-and-configure-the-web-components-on-the-head-node"></a>Adım 1: Yükleme ve web bileşenleri baş düğümünde yapılandırın
HTTPS üzerinden kümeye iş göndermek bir REST arabirimini etkinleştirmek için HPC paketi web bileşenleri HPC paketi üstbilgi düğümü üzerinde yapılandırıldığından emin olun. Bunlar zaten yüklü değilse, ilk HpcWebComponents.msi yükleme dosyasını çalıştırarak web bileşenleri yükleyin. Ardından, HPC PowerShell betiğini çalıştırarak bileşenlerini yapılandırma **kümesi HPCWebComponents.ps1**.

Ayrıntılı yordamlar için bkz: [Microsoft HPC Pack Web bileşenleri yüklemeniz](http://technet.microsoft.com/library/hh314627.aspx).

> [!TIP]
> HPC Pack için belirli Azure hızlı başlangıç şablonlarını yükleyin ve web bileşenleri otomatik olarak yapılandırın. Kullanırsanız [HPC Pack Iaas dağıtım betiği](classic/hpcpack-cluster-powershell-script.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json) küme oluşturmak için isteğe bağlı olarak yükleyebilir ve web bileşenleri dağıtımının bir parçası yapılandırın.
> 
> 

**Web bileşenleri yüklemek için**

1. Baş düğüm VM, bir Küme Yöneticisi kimlik bilgilerini kullanarak bağlanın.
2. HPC Pack kurulum klasöründen HpcWebComponents.msi baş düğümünde çalıştırın.
3. Web bileşenleri yüklemek için sihirbazdaki adımları izleyin

**Web bileşenleri yapılandırmak için**

1. Baş düğümünde HPC PowerShell'i yönetici olarak başlatın.
2. Dizin yapılandırma komut dosyası konumunu değiştirmek için aşağıdaki komutu yazın:
   
    ```powershell
    cd $env:CCP_HOME\bin
    ```
3. REST arabirimi yapılandırmak ve HPC Web hizmetini başlatmak için aşağıdaki komutu yazın:
   
    ```powershell
    .\Set-HPCWebComponents.ps1 –Service REST –enable
    ```
4. Bir sertifika seçmeniz istendiğinde baş düğüm ortak DNS adına karşılık gelen sertifika seçin. Baş düğüm Klasik dağıtım modeli kullanarak VM dağıtmak, örneğin, sertifika adı CN gibi durur =&lt;*HeadNodeDnsName*&gt;. cloudapp.net. Resource Manager dağıtım modeli kullanırsanız, sertifika adı CN gibi görünüyor =&lt;*HeadNodeDnsName*&gt;.&lt; *bölge*&gt;. cloudapp.azure.com.
   
   > [!NOTE]
   > Bir şirket içi bilgisayardan baş düğüme işleri gönderdiğinizde, bu sertifika daha sonra seçin. Yok seçin veya Active Directory etki alanındaki baş düğüm bilgisayar adına karşılık gelen bir sertifika yapılandırın (örneğin, CN =*MyHPCHeadNode.HpcAzure.local*).
   > 
   > 
5. İş gönderme web portalı yapılandırmak için aşağıdaki komutu yazın:
   
    ```powershell
    .\Set-HPCWebComponents.ps1 –Service Portal -enable
    ```
6. Betik tamamlandıktan sonra durdurmak ve aşağıdaki komutları yazarak HPC İş Zamanlayıcısı hizmetini yeniden başlatın:
   
    ```powershell
    net stop hpcscheduler
    net start hpcscheduler
    ```

## <a name="step-2-install-the-hpc-pack-client-utilities-on-an-on-premises-computer"></a>2. adım: bir şirket içi bilgisayar HPC Pack istemci yardımcı programları yükle
HPC Pack istemci yardımcı programları bilgisayarınıza yüklemek istiyorsanız, HPC paketi Kurulum dosyaları (tam yükleme) indirin [Microsoft Download Center](http://go.microsoft.com/fwlink/?LinkId=328024). Yüklemeye başlamak için kurulum seçeneğini seçin **HPC Pack istemci yardımcı programları**.

Üstbilgi düğüm VM'ine işleri göndermek için HPC Pack istemci araçları kullanmak için ayrıca baş düğümünden bir sertifika verin ve istemci bilgisayara yüklemeniz gerekir. Sertifika olması gerekir. CER biçimi.

**Sertifika baş düğümünden dışarı aktarma**

1. Baş düğümünde yerel bilgisayar hesabı için bir Microsoft Yönetim Konsolu için Sertifikalar ek bileşenini ekleyin. Ek bileşenini eklemek adımlar için bkz: [Sertifikalar ek bileşenini MMC'ye ekleme](https://technet.microsoft.com/library/cc754431.aspx).
2. Konsol ağacında **sertifikalar – yerel bilgisayar** > **kişisel**ve ardından **Sertifikalar**.
3. HPC Pack web bileşenleri için yapılandırılmış sertifikayı bulun [adım 1: yükleme ve web bileşenleri baş düğümünde yapılandırma](#step-1:-install-and-configure-the-web-components-on-the-head-node) (örneğin, CN =&lt;*HeadNodeDnsName* &gt;. cloudapp.net).
4. Sertifikayı sağ tıklatın ve **tüm görevler** > **verme**.
5. Sertifika Dışarı Aktarma Sihirbazı'nda tıklatın **sonraki**ve emin **Hayır, özel anahtarı verme** seçilir.
6. DER ile kodlanmış ikili X.509 sertifikasını dışarı aktarmak için sihirbazın kalan adımları izleyin (. CER) biçimi.

**İstemci bilgisayara sertifikayı içeri aktarmak için**

1. İstemci bilgisayarı üzerindeki bir klasöre baş düğümünden dışarı aktardığınız sertifikayı kopyalayın.
2. İstemci bilgisayarda certmgr.msc çalıştırın.
3. Sertifika Yöneticisi'nde **Sertifikalar – Geçerli kullanıcı** > **güvenilen kök sertifika yetkilileri**, sağ **sertifikaları**ve ardından tıklatın **tüm görevler** > **alma**.
4. Sertifika Alma Sihirbazı'nda tıklatın **sonraki** baş düğümünden güvenilen kök sertifika yetkilileri deposuna dışarı aktardığınız sertifikayı almak için adımları izleyin.

> [!TIP]
> Baş düğümüne sertifika yetkilisinde istemci bilgisayar tarafından tanınmadığından bir güvenlik uyarısı görebilirsiniz. Test amacıyla, bu uyarıyı yok sayın ve sertifika alma tamamlayın.
> 
> 

## <a name="step-3-run-test-jobs-on-the-cluster"></a>3. adım: Çalıştır test işleri küme üzerinde
Yapılandırmanızı doğrulamak için şirket içi bilgisayarından Azure kümesinde işleri çalıştırmayı deneyin. Örneğin, kümeye iş göndermek için HPC Pack GUI araçları veya komut satırı komutlarını kullanabilirsiniz. İşlerini göndermek için web tabanlı portal de kullanabilirsiniz.

**İstemci bilgisayarda iş gönderme komutları çalıştırmak için**

1. HPC Pack istemci yardımcı programları yüklü olduğu bir istemci bilgisayarda bir komut istemi başlatın.
2. Bir örnek komutu yazın. Örneğin, kümedeki tüm işleri listelemek için tam DNS adı baş düğümü bağlı olarak aşağıdakilerden birini benzeyen bir komut yazın:
   
    ```command
    job list /scheduler:https://<HeadNodeDnsName>.cloudapp.net /all
    ```
   
    or
   
    ```command
    job list /scheduler:https://<HeadNodeDnsName>.<region>.cloudapp.azure.com /all
    ```
   
   > [!TIP]
   > Baş düğüm, IP adresi değil tam DNS adını Zamanlayıcı URL'yi kullanın. IP adresi belirtirseniz, bir hata "sunucu sertifikası ya da geçerli bir güven zinciri veya güvenilir kök deposuna yerleştirilecek gerekiyor." benzer görünür
   > 
   > 
3. İstendiğinde, kullanıcı adını yazın (biçiminde &lt;DomainName&gt;\\&lt;kullanıcıadı&gt;) ve HPC Küme Yöneticisi ya da yapılandırdığınız başka bir küme kullanıcının parolası. Yerel olarak daha fazla iş işlemleri için kimlik bilgilerini depolamak üzere seçebilirsiniz.
   
    İşlerini listesi görüntülenir.

**İstemci bilgisayarda HPC İş Yöneticisi'ni kullanmak için**

1. Daha önce bir küme kullanıcının etki alanı kimlik bilgilerini bir işi gönderirken depoladığınız alamadık, kimlik bilgileri Yöneticisi'nde kimlik bilgileri ekleyebilirsiniz.
   
    a. İstemci bilgisayardaki Denetim Masası'ndaki kimlik bilgisi Yöneticisi'ni başlatın.
   
    b. Tıklatın **Windows kimlik bilgileri** > **genel bir kimlik bilgisi Ekle**.
   
    c. Internet adresini belirtin (örneğin, https://&lt;HeadNodeDnsName&gt;.cloudapp.net/HpcScheduler veya https://&lt;HeadNodeDnsName&gt;.&lt; Bölge&gt;.cloudapp.azure.com/HpcScheduler) ve kullanıcı adı (&lt;DomainName&gt;\\&lt;kullanıcıadı&gt;) ve Küme Yöneticisi veya başka bir parola yapılandırdığınız küme kullanıcı.
2. İstemci bilgisayarda HPC İş Yöneticisi'ni başlatın.
3. İçinde **baş düğüm Seç** iletişim kutusunda, Azure'da baş düğüm URL'sini yazın (örneğin, https://&lt;HeadNodeDnsName&gt;. cloudapp.net veya https://&lt;HeadNodeDnsName&gt;.&lt; Bölge&gt;. cloudapp.azure.com).
   
    HPC İş Yöneticisi'ni açar ve baş düğümünde işlerin bir listesini gösterir.

**Baş düğüm üzerinde çalışan web portalı kullanmak için**

1. İstemci bilgisayarda bir web tarayıcı başlatmak ve baş düğüm tam DNS adını bağlı olarak aşağıdaki adresleri birini girin:
   
    ```
    https://<HeadNodeDnsName>.cloudapp.net/HpcPortal
    ```
   
    or
   
    ```
    https://<HeadNodeDnsName>.<region>.cloudapp.azure.com/HpcPortal
    ```
2. Görüntülenen güvenlik iletişim kutusunda HPC Küme Yöneticisi etki alanı kimlik bilgilerini yazın. (Diğer küme kullanıcılar da farklı rolleri ekleyebilirsiniz. Bkz: [küme kullanıcıları yönetme](https://technet.microsoft.com/library/ff919335.aspx).)
   
    Web portalı iş liste görünümü açılır.
3. "Hello World" dizesi kümeden döndüren bir örnek işi göndermek için tıklatın **yeni iş** sol gezinti içinde.
4. Üzerinde **yeni iş** sayfasında **gönderme sayfalarından**, tıklatın **HelloWorld**. İş gönderme sayfası görüntülenir.
5. Tıklatın **gönderme**. İstenirse, HPC Küme Yöneticisi etki alanı kimlik bilgilerini sağlayın. İş gönderildiğinde ve iş kimliği kasasındaki **İşlerim** sayfası.
6. Gönderdiğiniz iş sonuçlarını görüntülemek için iş kimliği'ni tıklatın ve ardından **görünümü görevleri** komut çıktısı görüntülemek için (altında **çıkış**).

## <a name="next-steps"></a>Sonraki adımlar
* Ayrıca Azure kümeye ile iş gönderebilirsiniz [HPC Pack REST API](http://social.technet.microsoft.com/wiki/contents/articles/7737.creating-and-submitting-jobs-by-using-the-rest-api-in-microsoft-hpc-pack-windows-hpc-server.aspx).
* Bir Linux istemciden kümeye iş göndermek istiyorsanız, Python örnekte bkz [HPC Pack 2012 R2 SDK'sı ve örnek kod](https://www.microsoft.com/download/details.aspx?id=41633).

<!--Image references-->
[jobsubmit]: ./media/virtual-machines-windows-hpcpack-cluster-submit-jobs/jobsubmit.png
