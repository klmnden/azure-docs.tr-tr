---
title: "Eklenti Azure ikincil Hudson sürekli tümleştirme ile kullanma | Microsoft Docs"
description: "Eklenti Azure ikincil Hudson sürekli tümleştirme ile kullanmayı açıklar."
services: virtual-machines-linux
documentationcenter: 
author: rmcmurray
manager: wpickett
editor: 
ms.assetid: b2083d1c-4de8-4a19-a615-ccc9d9b6e1d9
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-multiple
ms.devlang: java
ms.topic: article
ms.date: 04/25/2017
ms.author: robmcm
ms.openlocfilehash: c11b59f8ea432075b147a391de4b7bd3331e639e
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-use-the-azure-slave-plug-in-with-hudson-continuous-integration"></a>Eklenti Azure ikincil Hudson sürekli tümleştirme ile kullanma
Hudson için eklenti Azure ikincil dağıtılmış çalıştıran oluşturduğunda Azure bağımlı düğümlerinde sağlamanıza imkan sağlar.

## <a name="install-the-azure-slave-plug-in"></a>Azure ikincil eklentisini yükleme
1. Hudson panosunda tıklatın **yönetmek Hudson**.
2. İçinde **yönetmek Hudson** sayfasında, tıklatın **eklentileri yönetme**.
3. Tıklatın **kullanılabilir** sekmesi.
4. Tıklatın **arama** ve türü **Azure** ilgili eklentileri listesine sınırlamak için.
   
    Kullanılabilir eklentiler listesinde gezinin seçerseniz, Azure ikincil eklenti altında bulabilirsiniz **küme yönetimi ve dağıtılmış yapı** bölümüne **başkalarının** sekmesi.
5. Onay kutusunu seçip **Azure bağımlı eklentisi**.
6. **Yükle**'ye tıklayın.
7. Hudson yeniden başlatın.

Artık eklenti yüklendiğine göre sonraki adımlar eklenti Azure aboneliği profilinizle yapılandırmak ve VM ikincil düğüm için oluşturmada kullanılacak bir şablon oluşturmak için olacaktır.

## <a name="configure-the-azure-slave-plug-in-with-your-subscription-profile"></a>Abonelik profilinizle eklenti Azure ikincil yapılandırın
Da ayarları, yayım olarak adlandırılan bir abonelik profili güvenli kimlik bilgileri ve geliştirme ortamınızda Azure ile çalışması gerekir bazı ek bilgiler içeren bir XML dosyasıdır. Eklenti Azure ikincil yapılandırmak için gerekir:

* Abonelik kimliği
* Aboneliğiniz için bir yönetim sertifikası

Bunlar bulunabilir, [abonelik profili]. Aşağıda, bir abonelik profili örneğidir.

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

Abonelik profilinizi oluşturduktan sonra Azure ikincil eklentisini yapılandırmak için aşağıdaki adımları izleyin.

1. Hudson panosunda tıklatın **yönetmek Hudson**.
2. Tıklatın **sistem yapılandırma**.
3. Bulmak için sayfayı aşağı kaydırın **bulut** bölümü.
4. Tıklatın **yeni bulut Ekle > Microsoft Azure**.
   
    ![Yeni bulut ekleme][add new cloud]
   
    Abonelik ayrıntılarınızı girmenize gerek burada bu alanları gösterir.
   
    ![profili yapılandırma][configure profile]
5. Abonelik kimliği ve yönetim sertifikası abonelik profilinizden kopyalayın ve bunları uygun alanlara yapıştırın.
   
    Abonelik kimliği ve yönetim sertifikası kopyalarken **sağlamadığı** değerlerini alın tırnak işaretleri içine.
6. Tıklayın **doğrulama yapılandırma**.
7. Yapılandırma başarıyla doğrulandığında tıklatın **kaydetmek**.

## <a name="set-up-a-virtual-machine-template-for-the-azure-slave-plug-in"></a>Sanal makine şablon için Azure bağımlı eklenti ayarlama
Bir sanal makine şablonu eklenti Azure üzerinde bir ikincil düğüm oluşturmak için kullanacağı parametrelerini tanımlar. Aşağıdaki adımlarda şablonu için bir Ubuntu VM oluşturuluyor.

1. Hudson panosunda tıklatın **yönetmek Hudson**.
2. Tıklayın **sistem yapılandırma**.
3. Bulmak için sayfayı aşağı kaydırın **bulut** bölümü.
4. İçinde **bulut** bölümünde, Bul **Azure sanal makine şablonu ekleme** tıklatıp **Ekle** düğmesi.
   
    ![VM şablonu Ekle][add vm template]
5. Bir bulut hizmet adı belirtmeniz **adı** alan. Belirttiğiniz ad var olan bir bulut hizmetini başvuruyorsa, VM bu hizmetinde sağlanacak. Aksi takdirde, Azure yeni bir tane oluşturun.
6. İçinde **açıklama** alan, oluşturduğunuz şablon açıklayan metin girin. Bu bilgiler yalnızca documentary amaçlıdır ve bir VM sağlama kullanılmaz.
7. İçinde **etiketleri** alanına, **linux**. Bu etiket oluşturmakta olduğunuz şablon tanımlamak için kullanılır ve daha sonra bir Hudson işi oluştururken şablonu başvurmak için kullanılır.
8. VM oluşturulacağı bir bölge seçin.
9. Uygun VM boyutunu seçin.
10. VM oluşturulacağı bir depolama hesabı belirtin. , Kullandığınız bulut hizmeti ile aynı bölgede olduğundan emin olun. Oluşturulacak yeni depolama istiyorsanız bu alanı boş bırakabilirsiniz.
11. Saklama süresi Hudson boşta bir ikincil silmeden önce dakika sayısını belirtir. 60 varsayılan değerini bırakın.
12. İçinde **kullanım**, bu ikincil düğüm kullanıldığında uygun koşulu seçin. Şimdilik, seçin **bu düğüm mümkün olduğunca kullanma**.
    
     Bu noktada, formunuz için biraz benzer görünür:
    
     ![Şablon yapılandırma][template config]
13. İçinde **görüntü ailesi veya kimliği** hangi sistem görüntüsü de kendi VM'nizi yüklenecek belirtmeniz gerekir. Görüntü aileleri listesinden seçin veya özel bir görüntü belirtin.
    
     Görüntü aileleri listesinden seçmek istiyorsanız, ilk karakteri (büyük küçük harfe duyarlı) görüntü ailesi adı girin. Örneğin, yazarak **U** Ubuntu Server ailelerinin listesini getirir. Listeden seçtikten sonra Jenkins VM ayrılırken bu aile, sistem görüntüsünü en son sürümünü kullanır.
    
     ![İşletim sistemi ailesi listesi][OS family list]
    
     Bunun yerine kullanmak istediğiniz özel bir görüntü varsa, bu özel görüntü adı girin. Adın doğru girildiğinden emin olmanız gerekir böylece özel resim adları bir listede gösterilmez.    
    
     Bu öğretici için yazın **U** Ubuntu görüntülerin listesini getirmek ve seçmek için **Ubuntu Server 14.04 LTS**.
14. İçin **başlatma yöntemi**seçin **SSH**.
15. Aşağıdaki betiği kopyalayın ve yapıştırın **Init betik** alan.
    
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
    
     **Init betik** VM oluşturulduktan sonra yürütülür. Bu örnekte, Java, git ve ant komut dosyasını yükler.
16. İçinde **kullanıcıadı** ve **parola** alanları, VM üzerinde oluşturulacak yönetici hesabı için tercih edilen değerlerinizi girin.
17. Tıklayın **doğrulayın şablonu** , belirtilen parametreler geçerli olup olmadığını denetlemek için.
18. **Kaydet**'e tıklayın.

## <a name="create-a-hudson-job-that-runs-on-a-slave-node-on-azure"></a>Azure üzerinde bir ikincil düğüm üzerinde çalışan bir Hudson iş oluşturma
Bu bölümde, Azure üzerinde bir ikincil düğüm üzerinde çalışacak Hudson görev oluşturma.

1. Hudson panosunda tıklatın **yeni iş**.
2. Oluşturmakta olduğunuz işi için bir ad girin.
3. İş türü için **serbest stili yazılım işi yapı**.
4. **Tamam** düğmesine tıklayın.
5. İş yapılandırma sayfasında seçin **burada bu proje çalıştırılabilir sınırla**.
6. Seçin **düğümü ve etiket menü** seçip **linux** (biz Bu etiket önceki bölümde sanal makine şablonu oluştururken, belirtilen).
7. İçinde **yapı** 'yi tıklatın **Ekle derleme adımı** seçip **Kabuk yürütme**.
8. Aşağıdaki Düzen komut dosyası, değiştirme **{github hesap adınız}**, **{projenizin adına}**, ve **{proje dizininiz}** uygun değerleri ile düzenlenmiş komut dosyasını görüntülenen metin alanına yapıştırın.
   
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
10. Hudson panosunda, az önce oluşturduğunuz işi bulun ve tıklayın **bir yapı zamanlama** simgesi.

Hudson sonra önceki bölümde oluşturduğunuz şablonu kullanarak bir ikincil düğüm oluşturabilir ve bu görev için yapı adımında belirtilen betiğini yürütün.

## <a name="next-steps"></a>Sonraki Adımlar
Azure’u Java ile kullanma hakkında daha fazla bilgi edinmek için bkz. [Azure Java Geliştirici Merkezi].

<!-- URL List -->

[Azure Java Geliştirici Merkezi]: https://azure.microsoft.com/develop/java/
[abonelik profili]: http://go.microsoft.com/fwlink/?LinkID=396395

<!-- IMG List -->

[add new cloud]: ./media/virtual-machines-azure-slave-plugin-for-hudson/hudson-setup-addcloud.png
[configure profile]: ./media/virtual-machines-azure-slave-plugin-for-hudson/hudson-setup-configureprofile.png
[add vm template]: ./media/virtual-machines-azure-slave-plugin-for-hudson/hudson-setup-addnewvmtemplate.png
[template config]: ./media/virtual-machines-azure-slave-plugin-for-hudson/hudson-setup-templateconfig1-withdata.png
[OS family list]: ./media/virtual-machines-azure-slave-plugin-for-hudson/hudson-oslist.png

