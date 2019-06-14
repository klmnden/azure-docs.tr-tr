---
title: Kullanım Azure Container Instances olarak bir Jenkins derleme aracısı
description: Aracı bir Jenkins derleme olarak Azure Container Instances'ı kullanmayı öğrenin.
services: container-instances
author: dlepow
ms.service: container-instances
ms.topic: article
ms.date: 08/31/2018
ms.author: danlep
ms.openlocfilehash: 2687a64bfd952888086862d929a3e5869aee5a4f
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60584190"
---
# <a name="use-azure-container-instances-as-a-jenkins-build-agent"></a>Kullanım Azure Container Instances olarak bir Jenkins derleme aracısı

Azure Container Instances'a (ACI) kapsayıcılı iş yüklerini çalıştırmak için isteğe bağlı, seri aktarıma uygun ve yalıtılmış bir ortam sağlar. ACI, nedeniyle bu öznitelikler, büyük bir ölçekte Jenkins derleme işleri çalıştırmak için harika bir platform sağlar. Bu makale, dağıtma ve önceden yapılandırılmış bir Jenkins sunucusu yapı hedefi ile ACI kullanarak rehberlik yapacaktır.

Azure Container Instances hakkında daha fazla bilgi için bkz. [Azure Container Instances hakkında][about-aci].

## <a name="deploy-a-jenkins-server"></a>Jenkins sunucusunu dağıtma

1. Azure portalında **kaynak Oluştur** araması **Jenkins**. Yayımcı Jenkins teklifle seçin **Microsoft**ve ardından **Oluştur**.

2. Aşağıdaki bilgileri girin **Temelleri** oluşturmak ve ardından **Tamam**.

   - **Ad**: Jenkins dağıtımı için bir ad girin.
   - **Kullanıcı adı**: Jenkins sanal makinenin yönetici kullanıcı için bir ad girin.
   - **Kimlik doğrulama türü**: Kimlik doğrulaması için SSH ortak anahtarı öneririz. Bu seçeneği belirlerseniz, Jenkins sanal makineye oturum açmak için kullanılacak bir SSH ortak anahtarını yapıştırın.
   - **Abonelik**: Bir Azure aboneliği seçin.
   - **Kaynak grubu**: Kaynak grubu oluşturun veya var olan bir grubu seçin.
   - **Konum**: Jenkins sunucusu için bir konum seçin.

   ![Jenkins portal dağıtım için temel ayarları](./media/container-instances-jenkins/jenkins-portal-01.png)

3. Üzerinde **ek ayarlar** formunda, aşağıdaki öğeleri tamamlayın:

   - **Boyutu**: Jenkins sanal makineniz için uygun boyutlandırma seçeneğini belirleyin.
   - **VM disk türü**: Seçeneklerinden birini belirtin **HDD** (sabit disk sürücüsü) veya **SSD** (katı hal sürücüsü) Jenkins sunucusu için.
   - **Sanal ağ**: Varsayılan ayarları değiştirmek istiyorsanız oku seçin.
   - **Alt ağlar**: Oku seçin, bilgileri doğrulayın ve seçin **Tamam**.
   - **Genel IP adresi**: Genel IP adresi, özel bir ad verin, SKU yapılandırma ve atama yöntemini ayarlamak için oku seçin.
   - **Etki alanı adı etiketi**: Jenkins sanal makinesi için tam bir URL oluşturmak için bir değer belirtin.
   - **Jenkins yayın türünü**: İstenen sürüm türü seçeneklerden birini belirleyin: **LTS**, **haftalık yapı**, veya **Azure doğrulandı**.

   ![Jenkins portal dağıtım için ek ayarlar](./media/container-instances-jenkins/jenkins-portal-02.png)

4. Hizmet sorumlusu tümleştirmesi seçin **Auto(MSI)** olmasını [kimliklerini Azure kaynakları için yönetilen] [ managed-identities-azure-resources] için bir kimlik doğrulama kimliğini otomatik olarak oluştur Jenkins örneği. Seçin **el ile** kendi hizmet sorumlusu kimlik bilgileri sağlamak için.

5. Jenkins derleme işleri için bulut tabanlı bir platform bulut aracılarını yapılandırın. Bu makalede amacıyla seçin **ACI**. ACI bulut Aracısı ile her bir Jenkins derleme işi container Instance üzerinde çalıştırılır.

   ![Jenkins portal dağıtım için bulut tümleştirme ayarları](./media/container-instances-jenkins/jenkins-portal-03.png)

6. Tümleştirme ayarları ile işiniz bittiğinde seçin **Tamam**ve ardından **Tamam** yeniden üzerinde doğrulama özeti. Seçin **Oluştur** üzerinde **kullanım koşullarını** özeti. Jenkins sunucusunu dağıtmak için birkaç dakika sürer.

## <a name="configure-jenkins"></a>Jenkins’i yapılandırma

1. Azure portalında Jenkins kaynak grubuna gidin, Jenkins sanal makineyi seçin ve DNS adını not edin.

   ![Jenkins sanal makinesi ayrıntılarını bir DNS adı](./media/container-instances-jenkins/jenkins-portal-fqdn.png)

2. Jenkins sanal makinenin DNS adına gidin ve döndürülen SSH dizesini kopyalayın.

   ![SSH dizesi ile Jenkins oturum açma yönergeleri](./media/container-instances-jenkins/jenkins-portal-04.png)

3. Geliştirme sisteminizde terminal oturumunu açın ve son adımı SSH dizeden yapıştırın. Güncelleştirme `username` Jenkins sunucusunu dağıttığınızda belirtilen kullanıcı adı için.

4. Sonra oturumu bağlı, ilk Yönetici parolasının almak için aşağıdaki komutu çalıştırın:

   ```
   sudo cat /var/lib/jenkins/secrets/initialAdminPassword
   ```

5. Tüneli çalıştıran ve SSH oturumu bırakın ve Git `http://localhost:8080` bir tarayıcıda. İlk Yönetici parolasının kutuya yapıştırın ve ardından **devam**.

   !["Jenkins'in kilidini açma" Ekran kutusu yönetici parolası](./media/container-instances-jenkins/jenkins-portal-05.png)

6. Seçin **önerilen eklentileri Yükle** Jenkins eklentileri yüklemeniz önerilir.

   !["Jenkins özelleştirme" ekranla "önerilen eklentileri Yükle" seçildi](./media/container-instances-jenkins/jenkins-portal-06.png)

7. Bir yönetici kullanıcı hesabı oluşturun. Bu hesap için oturum açmayı ve Jenkins örneğiniz ile çalışmak için kullanılır.

   !["İlk yönetici kullanıcıyı oluşturun" Ekran doldurulmuş kimlik bilgileri](./media/container-instances-jenkins/jenkins-portal-07.png)

8. Seçin **Kaydet ve Bitir**ve ardından **Jenkins kullanmaya başla** yapılandırmasını tamamlamak için.

Jenkins yapılandırılmıştır ve yapı ve kod dağıtmak için hazır. Bu örnekte, basit bir Java uygulaması, Azure Container Instances hakkında bir Jenkins derleme göstermek için kullanılır.

## <a name="create-a-build-job"></a>Bir derleme işi oluşturma

Artık, bir Azure kapsayıcı örneğinde Jenkins derlemeleri göstermek için bir Jenkins derleme işi oluşturulur.

1. Seçin **yeni öğe**, derleme proje gibi bir ad verin **aci-demo**seçin **Serbest tarzda proje**seçip **Tamam**.

   ![Derleme işi ve proje türleri listesinden kutusu adı](./media/container-instances-jenkins/jenkins-new-job.png)

2. Altında **genel**, emin **burada bu proje çalıştırılabilir kısıtlama** seçilir. Girin **linux** etiket ifadesi için. Bu yapılandırma, bu derleme işi ACI bulutta çalıştırmasını sağlar.

   ![Yapılandırma Ayrıntıları "Genel" sekmesi](./media/container-instances-jenkins/jenkins-job-01.png)

3. Altında **derleme**seçin **derleme adımı Ekle** seçip **Kabuğu Yürüt**. Girin `echo "aci-demo"` komutu.

   ![Derleme adımı için seçimleri "Derleme" sekmesi](./media/container-instances-jenkins/jenkins-job-02.png)

5. **Kaydet**’i seçin.

## <a name="run-the-build-job"></a>Derleme işi çalıştırma

Test derleme işi ve yapı platformu olarak Azure Container Instances gözlemek için bir yapıyı el ile başlatın.

1. Seçin **artık yapı** bir derleme işi başlatılamadı. İşin başlatılması birkaç dakika sürer. Aşağıdaki görüntüye benzer bir durum görmeniz gerekir:

   ![İş durumu bilgileri "Derleme geçmişi"](./media/container-instances-jenkins/jenkins-job-status.png)

2. İş çalışırken, Azure portalını açın ve Jenkins kaynak grubu arayın. Bir kapsayıcı örneği oluşturulduğunu görürsünüz. Jenkins işi içinde bu örneği çalışıyor.

   ![Kaynak grubundaki kapsayıcı örneği](./media/container-instances-jenkins/jenkins-aci.png)

3. Jenkins Jenkins yürütücüler (varsayılan 2) yapılandırılmış sayıdan daha fazla iş çalışırken, birden çok kapsayıcı örneği oluşturulur.

   ![Yeni oluşturulan kapsayıcı örnekler](./media/container-instances-jenkins/jenkins-aci-multi.png)

4. Kapsayıcı örnekleri, tüm derleme işleri tamamlandıktan sonra kaldırılır.

   ![Kaldırılan container Instances ile kaynak grubu](./media/container-instances-jenkins/jenkins-aci-none.png)

## <a name="troubleshooting-the-jenkins-plugin"></a>Jenkins eklentisiyle ilgili sorunları giderme

Jenkins eklentileriyle ilgili hatalarla karşılaşırsanız [Jenkins JIRA](https://issues.jenkins-ci.org/) sayfasında söz konusu bileşenle ilgili sorun bildirebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Azure üzerinde Jenkins hakkında daha fazla bilgi için bkz: [Azure ve Jenkins][jenkins-azure].

<!-- LINKS - internal -->
[about-aci]: ./container-instances-overview.md
[jenkins-azure]: ../jenkins/overview.md
[managed-identities-azure-resources]: ../active-directory/managed-identities-azure-resources/overview.md