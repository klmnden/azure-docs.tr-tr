---
title: Oluşturma ve Azure HDInsight Kurumsal güvenlik paketi kümeleri yapılandırma
description: Oluşturma ve Azure HDInsight Kurumsal güvenlik paketi kümelerini yapılandırma hakkında bilgi edinin
services: hdinsight
ms.service: hdinsight
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.topic: conceptual
ms.date: 05/09/2019
ms.openlocfilehash: 98bd222212d616a5d2c608779c607bb431d184b9
ms.sourcegitcommit: cf438e4b4e351b64fd0320bf17cc02489e61406a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/08/2019
ms.locfileid: "67657323"
---
# <a name="create-and-configure-enterprise-security-package-clusters-in-azure-hdinsight"></a>Oluşturma ve Azure HDInsight Kurumsal güvenlik paketi kümeleri yapılandırma

Azure HDInsight Kurumsal güvenlik paketi azure'da Apache Hadoop kümeleriniz için Active Directory tabanlı kimlik doğrulaması, çoklu kullanıcı desteği ve rol tabanlı erişim denetimi erişmenizi sağlar. ESP HDInsight kümeleri, hassas verileri güvenli bir şekilde işlemek için katı Kurumsal güvenlik ilkeleri için kullanan kuruluşlar, etkinleştirin.

Bu kılavuzun amacı bir ESP için şirket içi kullanıcılar oturum açabilir şekilde gerekli kaynakları doğru şekilde yapılandırmak için HDInsight kümesi etkindir. Bu makalede, Kurumsal güvenlik paketi etkin bir Azure HDInsight kümesi oluşturmak için gereken adımlarda size yol gösterir. & Active Directory etki alanı Hizmetleri (DNS) etkinleştirilmiş bir Windows Iaas VM oluşturma adımları verilmektedir. Bu sunucu bir ardılı olarak hareket edecek, **gerçek** şirket içi ortamınızdaki ve Kurulum ve yapılandırma adımları böylece daha sonra kendi ortamınızda yineleyebilirsiniz devam etmenize olanak tanır. Bu kılavuz ayrıca Azure Active Directory ile parola karma eşitlemesi kullanarak bir karma kimlik ortamı oluşturmanıza yardımcı olur.

Bu kılavuzu tamamlamak üzere tasarlanmıştır [HDInsight içinde kullanım Kurumsal güvenlik paketi](apache-domain-joined-architecture.md)

Bu işlem kendi ortamınızda kullanmadan önce Active Directory ve etki alanı Hizmetleri (DNS) ayarlayın. Ayrıca, Azure Active Directory eşitleme ve Azure Active Directory'yi şirket içi kullanıcı hesaplarına etkinleştirin.

![Mimari diyagramı](./media/apache-domain-joined-create-configure-enterprise-security-cluster/image002.png)

## <a name="create-on-premises-environment"></a>Şirket içi ortamı oluşturma

Genel Bakış: Bu bölümde, bir Azure hızlı dağıtım şablonu kullanacağınız yeni VM'ler oluşturabilir, yeni bir AD ormanı ve etki alanı Hizmetleri (DNS) yapılandırın.

1. Git [yeni bir AD ormanı ile Azure VM oluşturma](https://azure.microsoft.com/resources/templates/active-directory-new-domain/), hızlı dağıtım şablonu görüntülemek üzere.

1. Tıklayarak **Azure'a dağıtma**.
1. Azure aboneliğinizde oturum açın.
1. Üzerinde **yeni bir AD ormanı ile Azure VM oluşturma** ekranında, aşağıdaki adımları tamamlayın:
    1. Üzerinden dağıtılan kaynakları istediğiniz aboneliği seçin **abonelik** açılır.
    1. Seçin **Yeni Oluştur** yanındaki **kaynak grubu** ve adını girin **OnPremADVRG**
    1. Şablon alanlarını geri kalanı için aşağıdaki ayrıntıları girin:

        * **Konum**: Orta ABD
        * **Yönetici kullanıcı adı**: HDIFabrikamAdmin
        * **Yönetici parolası**: < YOUR_PASSWORD >
        * **Etki alanı**: HDIFabrikam.com
        * **DNS ön eki**: hdifabrikam

        ![Şablon, Azure VM ve AD ormanı oluşturma](./media/apache-domain-joined-create-configure-enterprise-security-cluster/create-azure-vm-ad-forest.png)

    1. Tıklayın **satın alma**
    1. Dağıtımını izlemek ve tamamlanmasını bekleyin.
    1. Kaynakları doğru kaynak grubu altında oluşturulan onaylayın `OnPremADVRG`.

## <a name="configure-users-and-groups-for-cluster-access"></a>Kümeye erişim için kullanıcıları ve grupları yapılandırma

Genel Bakış: Bu bölümde, sonuna kadar bu kılavuzda HDInsight kümesine erişimi olacak kullanıcılar oluşturur.

1. Uzak Masaüstü kullanarak etki alanı denetleyicisine bağlanın.
    1. Başında belirtilen şablonunu kullandıysanız, etki alanı denetleyicisi olarak adlandırılan bir vm'dir **adVM** içinde `OnPremADVRG` kaynak grubu.
    1. Azure portalı > **kaynak grupları** > **OnPremADVRG** > **adVM** > **Connect**.
    1. Tıklayın **RDP** sekmesine ve ardından **RDP dosyasını indir**.
    1. Dosyayı bilgisayarınıza kaydedin ve açın.
    1. Kimlik bilgileri istendiğinde kullanın `HDIFabrikam\HDIFabrikamAdmin` kullanıcı adı olarak ve ardından yönetici hesabı için seçtiğiniz parolayı girin.

1. Uzak Masaüstü oturumunuzun bir etki alanı denetleyicisi VM'SİNİN açıldıktan sonra Başlat **Active Directory Kullanıcıları ve Bilgisayarları** gelen **Sunucu Yöneticisi'ni** Pano. Tıklayın **Araçları** sağ üst köşedeki ardından **Active Directory Kullanıcıları ve Bilgisayarları** açılır listeden.

    ![Sunucu Yöneticisi'ni Aç Active Directory Yönetimi](./media/apache-domain-joined-create-configure-enterprise-security-cluster/server-manager-active-directory-screen.png)

1. İki yeni kullanıcı oluşturma **HDIAdmin**, **HDIUser**. Bu iki kullanıcı, oturum açmak için HDInsight kümeleri için kullanılır.

    1. İçinde **Active Directory Kullanıcıları ve Bilgisayarları** ekranında **eylem** > **yeni** > **kullanıcı**.

        ![Yeni Active Directory kullanıcısı oluşturma](./media/apache-domain-joined-create-configure-enterprise-security-cluster/create-new-user.png)

    1. İçinde **yeni nesne - kullanıcı** ekranında, girin `HDIUser` olarak **kullanıcı oturum açma adı** tıklatıp **sonraki**.

        ![İlk yönetici kullanıcıyı oluşturun](./media/apache-domain-joined-create-configure-enterprise-security-cluster/image020.png)

    1. Açılan pencerede görünen yeni hesap için istediğiniz parolayı girin. İfadesini içeren kutuyu **parola her zaman geçerli olsun**. HDIClick **Tamam**.
    1. Tıklayın **son** yeni bir hesap oluşturmak için.
    1. Başka bir kullanıcı oluşturma `HDIAdmin`.

        ![İkinci yönetici kullanıcı oluşturma](./media/apache-domain-joined-create-configure-enterprise-security-cluster/image024.png)

1. İçinde **Active Directory Kullanıcıları ve Bilgisayarları** ekranında **eylem** > **yeni** > **grubu**. Oluşturma `HDIUserGroup` yeni bir grup olarak.

    ![Yeni Active Directory grubu oluşturun](./media/apache-domain-joined-create-configure-enterprise-security-cluster/create-new-group.png)

    ![Yeni grup2 oluşturma](./media/apache-domain-joined-create-configure-enterprise-security-cluster/image028.png)

1. Ekleme **HDIUser** önceki adımda oluşturduğunuz **HDIUserGroup** üyesi olarak.

    1. Sağ tıklayın **HDIUserGroup** tıklatıp **özellikleri**.
    1. Git **üyeleri** sekmesine **Ekle**.
    1. ENTER `HDIUser` etiketli kutunun içinde **Seçilecek nesne adlarını girin** tıklatıp **Tamam**.
    1. Diğer hesap için önceki adımı yineleyin `HDIAdmin`

        ![grubuna üye Ekle](./media/apache-domain-joined-create-configure-enterprise-security-cluster/active-directory-add-users-to-group.png)

İki kullanıcı yanı sıra Active Directory ortamınızı ve bir kullanıcı grubu HDInsight kümesine erişmek için oluşturdunuz.

Bu kullanıcılar, Azure AD ile eşitlenir.

### <a name="create-a-new-azure-active-directory"></a>Yeni bir Azure Active Directory oluşturma

1. Azure Portal’da oturum açın.
1. Tıklayın **kaynak Oluştur** ve türü **dizin**. Seçin **Azure Active Directory** > **oluşturma**.
1. Girin **HDIFabrikam** altında **kuruluş adı**.
1. Girin **HDIFabrikamoutlook** altında **ilk etki alanı adı**.
1.           **Oluştur**'a tıklayın.
1. Azure portalında sol tarafta, tıklayın **Azure Active Directory**.
1. Gerekirse, **dizini Değiştir** oluşturduğunuz yeni dizine değiştirmek için **HDIFabrikamoutlook**.
1. Altında **Yönet** tıklayın **özel etki alanı adları** > **özel etki alanı Ekle**.
1. Girin **HDIFabrikam.com** altında **özel etki alanı adı** tıklatıp **etki alanı Ekle**.

![Yeni bir azure active directory oluşturun](./media/apache-domain-joined-create-configure-enterprise-security-cluster/create-new-directory.png)

![Yeni bir özel etki alanı oluşturma](./media/apache-domain-joined-create-configure-enterprise-security-cluster/create-custom-domain.png)

## <a name="configure-your-azure-ad-tenant"></a>Azure AD kiracınızı yapılandırın

Genel Bakış: Böylece kullanıcılar ve grupları şirket içi eşitleyebilmeniz için Azure AD kiracınıza yapılandıracağınız artık bulutta AD.

1. Bir AD Kiracı Yöneticisi oluşturun.
    1. Azure portalında oturum açın ve Azure AD kiracınızı seçin **HDIFabrikam**
    1. Seçin **kullanıcılar** altında **Yönet** ardından **yeni kullanıcı**.
    1. Yeni kullanıcı için aşağıdaki ayrıntıları girin:

        * Ad: fabrikamazureadmin
        * Kullanıcı adı: fabrikamazureadmin@hdifabrikam.com
        * Parola: güvenli bir parola, tercih ettiğiniz

    1. Tıklayarak **grupları** bölümünde, arama **AAD DC Administrators**, tıklatıp **seçin**.

        ![Gruplar](./media/apache-domain-joined-create-configure-enterprise-security-cluster/image038.png)

    1. Tıklayarak **dizin rolü** seçin ve bölüm **genel yönetici** sağ tarafındaki. **Tamam**’a tıklayın.

        ![Dizin rolü](./media/apache-domain-joined-create-configure-enterprise-security-cluster/image040.png)

    1. Kullanıcı için bir parola girin.           **Oluştur**'a tıklayın.

1. Yeni oluşturulan kullanıcı parolasını değiştirmek istiyorsanız <fabrikamazureadmin@hdifabrikam.com>. Oturum açma kimliği ve ardından, Azure portal kullanarak parolayı değiştirmesi istenir.

## <a name="sync-on-premises-users-to-azure-ad"></a>Eşitleme şirket kullanıcıları Azure AD

### <a name="download-and-install-microsoft-azure-active-directory-connect"></a>Karşıdan yükleme ve Microsoft Azure Active Directory'ye bağlanın

1. [Azure AD Connect'i indirme](https://www.microsoft.com/download/details.aspx?id=47594).

1. Yükleme Microsoft Azure Active Directory etki alanı denetleyicisinde bağlanın.
    1. Önceki adımda indirdiğiniz yürütülebilir dosyayı açın ve lisans koşullarını kabul etmiş olursunuz.           **Devam**'a tıklayın.

        ![Azure AD Connect](./media/apache-domain-joined-create-configure-enterprise-security-cluster/image052.png)

    1. Tıklayın **hızlı ayarları kullan** ve yüklemeyi tamamlayın.

        ![Hızlı ayarları kullan](./media/apache-domain-joined-create-configure-enterprise-security-cluster/image054.png)

### <a name="configure-sync-with-on-premises-domain-controller"></a>Şirket içi etki alanı denetleyicisi ile eşitlemeyi yapılandırma

1. Üzerinde **Azure ad Connect** ekranında, Azure AD için kullanıcı adı ve genel yönetici parolasını girin. Tıklayın **sonraki**. Bu kullanıcı adı: `fabrikamazureadmin@hdifabrikam.com` AD kiracınıza yapılandırırken oluşturulur.
    ![Azure AD'ye Bağlanma](./media/apache-domain-joined-create-configure-enterprise-security-cluster/image058.png)
1. Üzerinde **Active Directory Domain Services Bağlan** ekranında, bir kuruluş yöneticisi hesabının kullanıcı adı ve parola girin. Tıklayın **sonraki**. Bu kullanıcı adı: `HDIFabrikam\HDIFabrikamAdmin` ve daha önce oluşturduğunuz eşleştirme parolası.

   ![Active Directory etki alanı Hizmetleri'ne bağlanın](./media/apache-domain-joined-create-configure-enterprise-security-cluster/image060.png)
1. Üzerinde **Azure AD oturum açma yapılandırması** sayfasında **sonraki**.
    ![Azure AD oturum açma yapılandırması](./media/apache-domain-joined-create-configure-enterprise-security-cluster/image062.png)
1. Hazır ekran yapılandırmak için tıklayın **yükleme**.
    ![Yükleme](./media/apache-domain-joined-create-configure-enterprise-security-cluster/image064.png)
1. Zaman **yapılandırmasını tamamlamak** ekranı görüntülenir, tıklayın **çıkış**.
    ![Yapılandırma tamamlandı](./media/apache-domain-joined-create-configure-enterprise-security-cluster/image078.png)

1. Eşitleme tamamlandıktan sonra IAAS Active Directory'de oluşturduğunuz kullanıcıların bir Azure Active Directory'ye eşitlenen onaylayın.
    1. Azure Portal’da oturum açın.
    1. Seçin **Azure Active Directory** > **HDIFabrikam** > **kullanıcılar**.

### <a name="create-an-user-assigned-managed-identity"></a>Bir kullanıcı tarafından atanan bir yönetilen kimlik oluşturma

Kullanıcı tarafından atanan ve Azure Active Directory etki alanı Hizmetleri (Azure AD DS) yapılandırmak için kullanılan bir yönetilen kimlik oluşturun. Kullanıcı tarafından atanan bir yönetilen kimlik oluşturma hakkında daha fazla bilgi için bkz. [oluşturun, liste, delete veya Azure portalını kullanarak bir kullanıcı tarafından atanan yönetilen kimlik rol atama](../../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-portal.md).

1. Azure Portal’da oturum açın.
1. Tıklayın **kaynak Oluştur** ve türü **yönetilen kimliği**. Seçin **kullanıcı atanmış yönetilen kimlik** > **oluşturma**.
1. Girin **HDIFabrikamManagedIdentity** olarak **kaynak adı**.
1. Aboneliğinizi seçin.
1. Altında **kaynak grubu** tıklayın **Yeni Oluştur** girin **HDIFabrikam CentralUS**.
1. Seçin **Orta ABD** altında **konumu**.
1.           **Oluştur**'a tıklayın.

![Yeni bir kullanıcı tarafından atanan yönetilen kimliği oluşturma](./media/apache-domain-joined-create-configure-enterprise-security-cluster/image082.png)

### <a name="enable-azure-active-directory-domain-services"></a>Azure Active Directory Domain Services’ı etkinleştirme

Daha fazla bilgi için [etkinleştirme Azure Active Directory etki alanı Azure portalını kullanarak Hizmetleri](https://docs.microsoft.com/azure/active-directory-domain-services/active-directory-ds-getting-started).

1. Azure AD DS'yi barındırmak için sanal ağ oluşturun. Şu powershell kodunu çalıştırın.

    ```powershell
    Connect-AzAccount
    Get-AzSubscription
    Set-AzContext -Subscription 'SUBSCRIPTION_ID'
    $virtualNetwork = New-AzVirtualNetwork -ResourceGroupName 'HDIFabrikam-CentralUS' -Location 'Central US' -Name 'HDIFabrikam-AADDSVNET' -AddressPrefix 10.1.0.0/16
    $subnetConfig = Add-AzVirtualNetworkSubnetConfig -Name 'AADDS-subnet' -AddressPrefix 10.1.0.0/24 -VirtualNetwork $virtualNetwork
    $virtualNetwork | Set-AzVirtualNetwork
    ```

1. Azure Portal’da oturum açın.
1. Tıklayın **kaynak oluşturma**, girin **etki alanı Hizmetleri** seçip **Azure AD Domain Services**.
1. Üzerinde **Temelleri** ekran aşağıdaki adımları tamamlayın:
    1. Altında **dizin adı** Azure Active Directory'ı bu makale boyunca oluşturduğunuz seçin **HDIFabrikam**.
    1. Girin bir **DNS etki alanı adı** , **HDIFabrikam.com**.
    1. Aboneliğinizi seçin.
    1. Kaynak grubu belirtin **HDIFabrikam CentralUS** ve **konumu** , **Orta ABD**.

        ![Azure ad ds temel ayrıntıları](./media/apache-domain-joined-create-configure-enterprise-security-cluster/image084.png)

1. Üzerinde **ağ** tam ekran, ağı seçin (**HDIFabrikam VNET**) ve alt ağ (**AADDS alt**) önceki powershell komut dosyasını oluşturduğunuz. Ya da **Yeni Oluştur** artık bir sanal ağ oluşturma seçeneği.

    ![Ağ seçin](./media/apache-domain-joined-create-configure-enterprise-security-cluster/image086.png)

1. Üzerinde **yönetici grubuna** ekran, adlı bir grubu bir bildirim görmeniz **AAD DC Administrators** bu Grup yönetmek için zaten oluşturuldu. İsteğe bağlı olarak, bu grubun üyeliğini değiştirebilirsiniz, ancak bu makalede bir adım için gerekli değildir.           **Tamam**'ı tıklatın.

    ![Yönetici grubu görünümü](./media/apache-domain-joined-create-configure-enterprise-security-cluster/image088.png)

1. Üzerinde **eşitleme** ekranında, tam eşitleme seçerek etkinleştirin **tüm** ve ardından **Tamam**.

    ![eşitlemeyi etkinleştirme](./media/apache-domain-joined-create-configure-enterprise-security-cluster/image090.png)

1. Üzerinde **özeti** ekran, Azure AD DS için ayrıntılarını doğrulayın ve tıklayın **Tamam**.

    ![ayrıntılarını doğrulayın](./media/apache-domain-joined-create-configure-enterprise-security-cluster/image092.png)

1. Azure AD DS'yi etkinleştirdikten sonra yerel bir etki alanı adı hizmeti (DNS) sunucusu AD sanal makinelerde (VM) çalışır.

### <a name="configure-your-azure-ad-ds-virtual-network"></a>Azure AD DS'yi sanal ağınızı yapılandırmak

Bu bölümdeki adımlarda, Azure AD DS'yi sanal ağınızın yapılandırmanıza yardımcı olur (**HDIFabrikam AADDSVNET**), özel DNS sunucuları kullanılacak.

1. Özel DNS sunucularınızın IP adreslerini bulun. Tıklayarak **HDIFabrikam.com** AD DS kaynak tıklayın **özellikleri** altında **Yönet**   altında listelenen IP adreslerini bakın **IP Sanal ağ adresi**.

    ![Özel DNS IP adresleri için Azure AD DS bulun](./media/apache-domain-joined-create-configure-enterprise-security-cluster/image096.png)

1. Yapılandırma **HDIFabrikam AADDSVNET** özel IP'ler için `10.0.0.4` ve `10.0.0.5`.

    1. Seçin **DNS sunucuları** altında **ayarları** kategorisi. radyo düğmesinin yanındaki ardından **özel**, ilk IP adresini (10.0.0.4) metin kutusuna girin ve tıklayın **Kaydet**.
    1. Ek IP aynı adımları kullanarak adresleri (10.0.0.5) ekleyin.

1. Senaryomuzda Azure AD DS'yi, aşağıdaki görüntüde Göster olarak AADDS VNet üzerinde aynı ayarı IP adresleri 10.0.0.4 ve 10.0.0.5, IP adresi kullanacak şekilde yapılandırıldı.

    ![Özel dns sunucularını görüntüleme](./media/apache-domain-joined-create-configure-enterprise-security-cluster/image098.png)

## <a name="securing-ldap-traffic"></a>LDAP trafiği güvenli hale getirme

Hafif Dizin Erişim Protokolü (LDAP), okuma ve yazma için Active Directory için kullanılır. Güvenli Yuva Katmanı (SSL) kullanarak gizli ve güvenli LDAP trafiği yapabileceğiniz / Aktarım Katmanı Güvenliği (TLS) teknolojisi. Düzgün şekilde biçimlendirilmiş bir sertifika yükleyerek, LDAP (LDAPS) SSL üzerinden etkinleştirebilirsiniz.

Güvenli LDAP hakkında daha fazla bilgi için bkz. [yapılandırma güvenli LDAP (LDAPS) için bir Azure AD Domain Services yönetilen etki alanı](https://docs.microsoft.com/azure/active-directory-domain-services/active-directory-ds-admin-guide-configure-secure-ldap).

Bu bölümde, otomatik olarak imzalanan bir sertifika oluşturmak, sertifikayı indirin ve güvenli LDAP (LDAPS) için yapılandırma **hdifabrikam** Azure AD DS'yi yönetilen etki alanı.

Aşağıdaki betiği hdifabrikam için bir sertifika oluşturur. Sertifika yolun altındaki "LocalMachine" kaydedilir.

> [!Note] 
> Herhangi bir yardımcı programı veya geçerli bir PKCS oluşturan uygulama \#10 istek SSL sertifika isteği oluşturmak için kullanılabilir.

```powershell
$lifetime = Get-Date
New-SelfSignedCertificate -Subject hdifabrikam.com `
-NotAfter $lifetime.AddDays(365) -KeyUsage DigitalSignature, KeyEncipherment `
-Type SSLServerAuthentication -DnsName *.hdifabrikam.com, hdifabrikam.com
```

Sertifikanın bilgisayarda yüklü olduğunu doğrulayın\'s kişisel depolama. Aşağıdaki adımları tamamlayın:

1. Microsoft Yönetim Konsolu (MMC) başlatın.
1. Sertifikaları yerel bilgisayardaki yönetir Sertifikalar ek bileşenini ekleyin.
1. Genişletin **sertifikalar (yerel bilgisayar)** , genişletme **kişisel**ve ardından **sertifikaları**. Yeni bir sertifika kişisel depoda bulunmalıdır. Bu sertifika için tam bir ana bilgisayar adı verilir.

    ![Sertifika oluşturma işlemini doğrulayın](./media/apache-domain-joined-create-configure-enterprise-security-cluster/image102.png)

1. Sağ bölmede, önceki adımda oluşturduğunuz sertifikaya sağ tıklayın işaret **tüm görevler**ve ardından **dışarı**.

1. Üzerinde **özel anahtarı dışarı** sayfasında **Evet, özel anahtarı dışarı aktar**. Burada anahtarı içeri aktarılacak bilgisayardan okumak şifrelenmiş iletileri için özel anahtar gereklidir.

    ![özel anahtarı dışarı aktar](./media/apache-domain-joined-create-configure-enterprise-security-cluster/image103.png)

1. Üzerinde **dışarı aktarma dosyası biçimi** sayfasında varsayılan ayarları bırakın ve ardından **sonraki**. 
1. Üzerinde **parola** sayfasında, select özel anahtarı için bir parola yazın **TripleDES SHA1** için **şifreleme** tıklatıp **sonraki**.
1. Üzerinde **dışarı aktarılan dosya** sayfasında, yol ve dışa aktarılan sertifika dosyasını adını yazın ve ardından **sonraki**.
1. Dosya adı uzantısı .pfx olması gerekir, bu dosyayı güvenli bir bağlantı kurmak için Azure portalında yapılandırılır.
1. Güvenli LDAP (LDAPS) bir Azure AD Domain Services yönetilen etki alanı için etkinleştirin.
    1. Etki alanını seçin **HDIFabrikam.com** Azure portalından.
    1. Tıklayın **güvenli LDAP** altında **yönetme**.
    1. Üzerinde **güvenli LDAP** ekranında **etkinleştirme** altında **güvenli LDAP**.
    1. Bilgisayarınızda dışarı aktardığınız .pfx sertifika dosyasına göz atın.
    1. Sertifika parolasını girin.

    ![Güvenli LDAP'yi etkinleştirme](./media/apache-domain-joined-create-configure-enterprise-security-cluster/image113.png)

1. Güvenli LDAP Etkin, bağlantı noktası 636'ı etkinleştirerek erişilebilir olduğundan emin olun.
    1. Ağ güvenlik grubu tıklayın **AADDS HDIFabrikam.com NSG** içinde **HDIFabrikam CentralUS** kaynak grubu.
    1. Altında **ayarları** tıklayın **gelen güvenlik kuralları** > **Ekle**.
    1. Üzerinde **gelen Güvenlik Kuralı Ekle** ekranında, aşağıdaki özellikleri girin ve tıklatın **Ekle**:

        | Özellik | Değer |
        |---|---|
        | Source | Any |
        | Source port ranges | * |
        | Hedef | Any |
        | Destination port range | 636 |
        | Protocol | Any |
        | Action | Allow |
        | Öncelik | \<İstenen sayı\> |
        | Ad | Port_LDAP_636 |

    ![gelen güvenlik kuralı](./media/apache-domain-joined-create-configure-enterprise-security-cluster/add-inbound-security-rule.png)

1. `HDIFabrikamManagedIdentity` yönetilen kullanıcı tarafından atanan kimliği, HDInsight etki alanı Hizmetleri katkıda bulunan rolü okumak, oluşturmak, değiştirmek ve etki alanı Hizmetleri işlemleri silmek bu kimlik sağlayan yönetilen kimlik için etkin olur.

    ![Kullanıcı tarafından atanan yönetilen kimliği oluşturma](./media/apache-domain-joined-create-configure-enterprise-security-cluster/image117.png)

## <a name="creating-enterprise-security-package-enabled-hdinsight-cluster"></a>HDInsight küme etkin Kurumsal güvenlik paketi oluşturma

Bu adım, aşağıdaki önkoşulları gerektirir:

1. Yeni bir kaynak grubu oluşturma `HDIFabrikam-WestUS` konumda `West US`.
1. Bir sanal oluşturma ESP barındıracak ağ etkin HDInsight kümesi.

    ```powershell
    $virtualNetwork = New-AzVirtualNetwork -ResourceGroupName 'HDIFabrikam-WestUS' -Location 'West US' -Name 'HDIFabrikam-HDIVNet' -AddressPrefix 10.1.0.0/16
    $subnetConfig = Add-AzVirtualNetworkSubnetConfig -Name 'SparkSubnet' -AddressPrefix 10.1.0.0/24 -VirtualNetwork $virtualNetwork
    $virtualNetwork | Set-AzVirtualNetwork
    ```

1. AADDS barındıran sanal ağ arasında eş ilişki oluşturma (`HDIFabrikam-AADDSVNET`) ve HDInsight kümesi ESP barındıracak sanal ağ etkin (`HDIFabrikam-HDIVNet`). Bu iki sanal ağı eşleme için şu powershell kodunu kullanın.

    ```powershell
    Add-AzVirtualNetworkPeering -Name 'HDIVNet-AADDSVNet' -RemoteVirtualNetworkId (Get-AzVirtualNetwork -ResourceGroupName 'HDIFabrikam-CentralUS').Id -VirtualNetwork (Get-AzVirtualNetwork -ResourceGroupName 'HDIFabrikam-WestUS')

    Add-AzVirtualNetworkPeering -Name 'AADDSVNet-HDIVNet' -RemoteVirtualNetworkId (Get-AzVirtualNetwork -ResourceGroupName 'HDIFabrikam-WestUS').Id -VirtualNetwork (Get-AzVirtualNetwork -ResourceGroupName 'HDIFabrikam-CentralUS')
    ```

1. Yeni bir Azure Data Lake depolama Gen2 hesabı oluşturma **Hdigen2store**, yani kullanıcı yönetilen kimlikle yapılandırılmış **HDIFabrikamManagedIdentity**. Etkin kullanıcı ile Data Lake depolama Gen2 hesapları oluşturma hakkında daha fazla bilgi için bkz: yönetilen kimlikler [kullanımı Azure Data Lake depolama Gen2 Azure HDInsight ile kümeleri](../hdinsight-hadoop-use-data-lake-storage-gen2.md).

1. Özel DNS Kurulumu'nu **HDIFabrikam AADDSVNET** sanal ağ.
    1. Azure portalı > **kaynak grupları** > **OnPremADVRG** > **HDIFabrikam AADDSVNET**  >   **DNS sunucuları**.
    1. Seçin **özel** girin `10.0.0.4` ve `10.0.0.5`.
    1. **Kaydet**’e tıklayın.

        ![Özel dns ayarlarını Kaydet](./media/apache-domain-joined-create-configure-enterprise-security-cluster/image123.png)

1. Yeni bir ESP özellikli HDInsight Spark kümesi oluşturun.
    1. Tıklayın **özel (boyut, ayarları, uygulamalar)** .
    2. Bölüm 1'için istenen ayrıntılarını girin **Temelleri**. Emin **küme türü** olduğu **Spark 2.3 (HDI 3.6)** ve **kaynak grubu** olduğu **HDIFabrikam CentralUS**

    1. 2\. bölümünde **güvenlik + ağ**, aşağıdaki adımları tamamlayın:
        1. Tıklayın **etkin** altında **Kurumsal güvenlik paketi**.
        1. Tıklayın **kümenin yönetici kullanıcı** seçip **HDIAdmin** şirket içinde yönetici kullanıcı olarak daha önce oluşturduğunuz hesabı. Tıklayın **seçin**.

        1. Tıklayın **küme erişim grubu** seçip **HDIUserGroup**. Gelecekte bu gruba eklediğiniz herhangi bir kullanıcı, HDInsight kümeleri erişmesini mümkün olacaktır.

            ![Küme erişim grubu seçin](./media/apache-domain-joined-create-configure-enterprise-security-cluster/image129.jpg)

    1. Küme yapılandırması, diğer adımları tamamlayın ve ayrıntılarını doğrulayın **küme özeti**.           **Oluştur**'a tıklayın.

1. Oturum açın yeni oluşturulan küme için Ambari UI `https://CLUSTERNAME.azurehdinsight.net` yönetici kullanıcı adınızı kullanarak `hdiadmin@hdifabrikam.com` ve parola.

    ![Ambari için oturum açın](./media/apache-domain-joined-create-configure-enterprise-security-cluster/image135.jpg)

1. Tıklayın **rolleri** küme panosundan.
1. Üzerinde **rolleri** sayfasında, grubun girin **hdiusergroup** atamak **Küme Yöneticisi** rolü altında **rol atama bu**.

    ![hdiusergroup için Küme Yönetici rolü atama](./media/apache-domain-joined-create-configure-enterprise-security-cluster/image137.jpg)

1. Küme kullanarak oturum açma ve SSH istemcisi açın **hdiuser** şirket içi Active Directory'de daha önce oluşturduğunuz.

    ![kümeye SSH ile oturum açma](./media/apache-domain-joined-create-configure-enterprise-security-cluster/image139.jpg)

Ardından bu hesap ile oturum açabilmeniz varsa, ESP kümenizi şirket içi active directory ile eşitlemek için doğru şekilde yapılandırmış olmanız.

## <a name="next-steps"></a>Sonraki adımlar

* [Kurumsal güvenlik paketi ile Apache Hadoop güvenliğine giriş](apache-domain-joined-introduction.md)
