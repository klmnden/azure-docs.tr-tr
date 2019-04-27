---
title: Hudson sürekli tümleştirme ile Azure bağımlı eklentisini kullanma | Microsoft Docs
description: Hudson sürekli tümleştirme ile Azure bağımlı eklentisini kullanmayı açıklar.
services: virtual-machines-linux
documentationcenter: ''
author: rmcmurray
manager: wpickett
editor: ''
ms.assetid: b2083d1c-4de8-4a19-a615-ccc9d9b6e1d9
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-multiple
ms.devlang: java
ms.topic: article
ms.date: 04/25/2017
ms.author: robmcm
ms.openlocfilehash: ef24e356c9ac8424fc519a3b16af5d37a20e706f
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60444213"
---
# <a name="how-to-use-the-azure-slave-plug-in-with-hudson-continuous-integration"></a>Hudson sürekli tümleştirme ile Azure bağımlı eklentisini kullanma
Azure bağımlı eklentisini için Hudson bağımlı düğümlerden azure'da dağıtılmış çalışan oluşturduğunda sağlamanıza olanak sağlar.

## <a name="install-the-azure-slave-plug-in"></a>Azure bağımlı eklentisini yükleme
1. Hudson Panoda tıklayın **yönetme Hudson**.
2. İçinde **yönetme Hudson** sayfasında, tıklayarak **Eklentileri Yönet**.
3. Tıklayın **kullanılabilir** sekmesi.
4. Tıklayın **arama** ve türü **Azure** ilgili eklentileri listeye sınırlamak için.
   
    Kullanılabilir eklentiler listesi boyunca kaydırın tercih ederseniz, Azure bağımlı eklentisini altında bulabilirsiniz **küme yönetimi ve dağıtılmış yapı** konusundaki **başkalarının** sekmesi.
5. Onay kutusunu seçip **Azure bağımlı eklentisi**.
6. **Yükle**'ye tıklayın.
7. Restart Hudson.

Eklenti artık yüklendiğine göre sonraki adımlar, Azure abonelik profillerindeki ile eklentiyi yapılandırmak ve bağımlı düğümü için bir VM oluşturulurken kullanılacak bir şablon oluşturmak için olacaktır.

## <a name="configure-the-azure-slave-plug-in-with-your-subscription-profile"></a>Abonelik profilinizi ile Azure bağımlı eklentisini yapılandırma
Da ayarları, yayım olarak adlandırılan bir abonelik profillerindeki güvenli kimlik bilgileri ve geliştirme ortamınızda Azure ile çalışmak için ihtiyacınız olacak bazı ek bilgiler içeren bir XML dosyasıdır. Azure bağımlı eklentisini yapılandırmak için gerekir:

* Abonelik kimliğiniz
* Aboneliğiniz için yönetim sertifikası

Bunlar bulunabilir, [Abonelik profili]. Bir abonelik profillerindeki örneği aşağıda verilmiştir.

    <?xml version="1.0" encoding="utf-8"?>

        <PublishData>

          <PublishProfile SchemaVersion="2.0" PublishMethod="AzureServiceManagementAPI">

        <Subscription

              ServiceManagementUrl="https://management.core.windows.net"

              Id="<Subscription ID>"

              Name="Pay-As-You-Go"
            ManagementCertificate="<Management certificate value>" />

          </PublishProfile>

    </PublishData>

Abonelik profilinizi oluşturduktan sonra Azure bağımlı eklentisini yapılandırmak için aşağıdaki adımları izleyin.

1. Hudson Panoda tıklayın **yönetme Hudson**.
2. Tıklayın **sistemini yapılandırmak**.
3. Bulmak için sayfayı aşağı kaydırın **bulut** bölümü.
4. Tıklayın **yeni bulut Ekle > Microsoft Azure**.
   
    ![Yeni bulut Ekle][add new cloud]
   
    Abonelik bilgilerinizi girmek gerek duyduğunuz bu alanları gösterir.
   
    ![profili yapılandırma][configure profile]
5. Abonelik kimliği ve yönetim sertifikası abonelik profilinizden kopyalayın ve bunları uygun alanlara yapıştırın.
   
    Abonelik kimliği ve yönetim sertifikası kopyalarken **olmayan** değerlerini alın tırnak kaldırmayın.
6. Tıklayarak **yapılandırmayı doğrula**.
7. Yapılandırma başarıyla doğrulandığında tıklayın **Kaydet**.

## <a name="set-up-a-virtual-machine-template-for-the-azure-slave-plug-in"></a>Bir sanal makine şablonu için Azure bağımlı eklentisini ayarlama
Bir sanal makine şablonunda eklentinin Azure'da bir ikincil düğüm oluşturmak için kullanacağı parametreleri tanımlar. Aşağıdaki adımlarda biz şablonu için bir Ubuntu VM oluşturursunuz.

1. Hudson Panoda tıklayın **yönetme Hudson**.
2. Tıklayarak **sistemini yapılandırmak**.
3. Bulmak için sayfayı aşağı kaydırın **bulut** bölümü.
4. İçinde **bulut** bölümünde, bulma **Azure sanal makine şablonu ekleme** tıklatıp **Ekle** düğmesi.
   
    ![VM şablonu Ekle][add vm template]
5. Bir bulut hizmeti adını belirtin **adı** alan. Var olan bir bulut hizmeti için belirttiğiniz ad başvuruyorsa, VM, hizmet sağlanır. Aksi takdirde, Azure, yeni bir tane oluşturur.
6. İçinde **açıklama** oluşturmakta olduğunuz şablon açıklayan bir metin girin. Bu bilgiler yalnızca Belgesel amaçlarıyla ve bir VM sağlama kullanılmaz.
7. İçinde **etiketleri** alanına **linux**. Bu etiket, oluşturmakta olduğunuz şablon tanımlamak için kullanılır ve daha sonra şablon Hudson bir proje oluştururken başvurmak için kullanılır.
8. VM'nin oluşturulacağı bir bölge seçin.
9. Uygun VM boyutunu seçin.
10. VM'nin oluşturulacağı depolama hesabı belirtin. Kullanacaksınız bulut hizmetiyle aynı bölgede olduğundan emin olun. Oluşturulacak yeni depolama istiyorsanız bu alanı boş bırakabilirsiniz.
11. Elde tutma süresi, boş bir bağımlı Hudson silmeden önce dakika sayısını belirtir. Bu, 60 varsayılan değerini bırakın.
12. İçinde **kullanım**, bu ikincil düğüm kullanıldığında uygun koşulu seçin. Şimdilik seçin **bu düğümde mümkün olduğunca yararlanmak**.
    
     Bu noktada, formunuzu biraz benzer görünür:
    
     ![Şablon yapılandırma][template config]
13. İçinde **görüntü ailesi veya kimliği** , hangi sistem görüntüsü sanal Makinenize yüklenecek belirtmeniz gerekir. Görüntü aileleri listesinden seçin veya özel bir görüntü belirtin.
    
     Görüntü ailelerinin listeden seçmek istiyorsanız, ilk karakteri (büyük-küçük harfe duyarlı) görüntü ailesi adı girin. Örneğin, yazmaya **U** Ubuntu Server ailelerinin listesini ortaya çıkarır. Listeden seçtikten sonra Jenkins, sanal makine sağlanırken ailesinde, sistem görüntüsünü en son sürümünü kullanır.
    
     ![İşletim sistemi ailesi listesi][OS family list]
    
     Bunun yerine kullanmak istediğiniz özel bir görüntü varsa, bu özel görüntüyü adını girin. Adın doğru girildiğinden emin olmak zorunda özel görüntü adlarının bir listede gösterilmez.    
    
     Bu öğreticide, yazın **U** Ubuntu görüntülerin bir listesini ve seçmek için **Ubuntu Server 14.04 LTS**.
14. İçin **başlatma yöntemi**seçin **SSH**.
15. Aşağıdaki betiği kopyalayıp yapıştırın **Init betik** alan.
    
         # Install Java
    
         sudo apt-get -y update
    
         sudo apt-get install -y openjdk-7-jdk
    
         sudo apt-get -y update --fix-missing
    
         sudo apt-get install -y openjdk-7-jdk
    
         # Install git
    
         sudo apt-get install -y git
    
         #Install ant
    
         sudo apt-get install -y ant
    
         sudo apt-get -y update --fix-missing
    
         sudo apt-get install -y ant
    
     **Init betik** VM oluşturulduktan sonra yürütülecek. Bu örnekte, komut dosyası, Java, git ve ant yükler.
16. İçinde **kullanıcıadı** ve **parola** alanları, sanal makinenizde oluşturulacak yönetici hesabı için tercih edilen değerlerinizi girin.
17. Tıklayarak **şablonu doğrula** , belirtilen parametreler geçerli olup olmadığını denetlemek için.
18. **Kaydet**'e tıklayın.

## <a name="create-a-hudson-job-that-runs-on-a-slave-node-on-azure"></a>Bağımlı düğümde azure'da çalışan bir Hudson işi oluşturma
Bu bölümde, Azure üzerinde bir bağımlı düğümde çalıştırılacak Hudson görev oluşturursunuz.

1. Hudson Panoda tıklayın **yeni iş**.
2. Oluşturmakta olduğunuz iş için bir ad girin.
3. İş türü için **derleme serbest stil yazılım işi**.
4. **Tamam** düğmesine tıklayın.
5. Proje Yapılandırması sayfasında seçin **burada bu proje çalıştırılabilir kısıtlama**.
6. Seçin **düğüm ve etiket menüsü** seçip **linux** (biz Bu etiket sanal makine şablonunu önceki bölümde oluştururken belirttiğiniz).
7. İçinde **derleme** bölümünde **derleme adımı Ekle** seçip **Kabuğu Yürüt**.
8. Aşağıdaki Düzen değiştirerek, betik **{github hesabınızın adını}**, **{projenizin adına}**, ve **{proje dizininiz}** uygun değerleri ile düzenlenmiş yapıştırın betik metin alanında görüntülenir.
   
        # Clone from git repo
   
        currentDir="$PWD"
   
        if [ -e {your project directory} ]; then
   
              cd {your project directory}
   
              git pull origin master
   
        else
   
              git clone https://github.com/{your github account name}/{your project name}.git
   
        fi
   
        # change directory to project
   
        cd $currentDir/{your project directory}
   
        #Execute build task
   
        ant
9. **Kaydet**'e tıklayın.
10. Hudson Panoda yeni oluşturduğunuz işi bulun ve tıklayın **bir derleme zamanlama** simgesi.

Hudson sonra önceki bölümde oluşturduğunuz şablonu kullanarak bir ikincil düğüm oluştur ve bu görev için derleme adımında belirtilen betiği yürütün.

## <a name="next-steps"></a>Sonraki Adımlar
Azure’u Java ile kullanma hakkında daha fazla bilgi edinmek için bkz. [Azure Java Geliştirici Merkezi].

<!-- URL List -->

[Azure Java Geliştirici Merkezi]: https://azure.microsoft.com/develop/java/
[Abonelik profili]: https://go.microsoft.com/fwlink/?LinkID=396395

<!-- IMG List -->

[add new cloud]: ./media/virtual-machines-azure-slave-plugin-for-hudson/hudson-setup-addcloud.png
[configure profile]: ./media/virtual-machines-azure-slave-plugin-for-hudson/hudson-setup-configureprofile.png
[add vm template]: ./media/virtual-machines-azure-slave-plugin-for-hudson/hudson-setup-addnewvmtemplate.png
[template config]: ./media/virtual-machines-azure-slave-plugin-for-hudson/hudson-setup-templateconfig1-withdata.png
[OS family list]: ./media/virtual-machines-azure-slave-plugin-for-hudson/hudson-oslist.png

