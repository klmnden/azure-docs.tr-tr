<!--author=alkohli last changed:01/14/2016-->


#### <a name="to-create-a-new-service"></a>Yeni hizmet oluşturmak için
1. Microsoft hesabı kimlik bilgilerinizi, Azure Klasik portalında şu URL oturum açtığı kullanarak: [ https://manage.windowsazure.com/ ](https://manage.windowsazure.com/).
2. Klasik Azure portalında **Yeni** > **Veri Hizmetleri** > **StorSimple Yöneticisi** > **Hızlı Oluştur**’a tıklayın.
3. Görüntülenen formda şunları yapın:
   
   1. Hizmetinize benzersiz bir **Ad** verin. Hizmetinizi tanımlayabilmek için kullanılan kolay bir addır. Ad harf, rakam ve tirelerden oluşan 2-50 karakter arası uzunlukta olabilir. Ad bir harf veya sayıyla başlamalı ve bitmelidir.
   2. Hizmetiniz için bir **Konum** sağlayın. Genel olarak, cihazınızı dağıtmak istediğiniz coğrafi bölgeye yakın bir konum seçmek istersiniz. Aşağıdakilerin de etkili olmasını isteyebilirsiniz: 
      
      * Azure’da, StorSimple cihazınızla dağıtmak istediğiniz var olan iş yükleriniz varsa o veri merkezini kullanmanız gerekir.
      * StorSimple Yöneticisi hizmeti ve Azure Storage ayrı iki konumda olabilir. Böyle bir durumda, StorSimple Yöneticisi ve Azure Storage hesabını ayrı ayrı oluşturmanız gerekir. Azure Storage hesabı oluşturmak için Klasik Azure portalındaki Azure Storage hizmetine gidin ve [Azure Storage hesabı oluşturma](../articles/storage/common/storage-create-storage-account.md#create-a-storage-account)’daki adımları uygulayın. Bu hesabı oluşturduktan sonra [Hizmet için yeni bir depolama hesabı yapılandırma](../articles/storsimple/storsimple-deployment-walkthrough.md#configure-a-new-storage-account-for-the-service)’daki adımları uygulayarak bunu StorSimple Yöneticisi hizmetine ekleyin.
   3. Açılan listeden bir **Abonelik** seçin. Abonelik fatura hesabınıza bağlıdır. Bu alan bir aboneliğiniz olmadığı sürece yoktur.
   4. Otomatik olarak hizmeti olan bir depolama hesabı oluşturmak için **Yeni depolama hesabı oluştur**’u seçin. Bu depolama hesabının "storsimplebwv8c6dcnf" gibi özel bir adı olacaktır. Verilerinizin farklı bir konumda olması gerekiyorsa bu kutunun işaretini kaldırın. 
   5. Hizmeti oluşturmak için **StorSimple Yöneticisi Oluştur**’a tıklayın.
   
   ![StorSimple Yöneticisi oluşturma](./media/storsimple-create-new-service/HCS_CreateAService-include.png)
   
   **Hizmet** giriş sayfasına yönlendirileceksiniz. Hizmeti oluşturulması birkaç dakika alabilir. Hizmet sorunsuz oluşturulduktan sonra, uygun şekilde size bildirilir ve hizmetin durumu **Etkin** olarak değişir.
   
   ![Hizmet oluşturma](./media/storsimple-create-new-service/HCS_StorSimpleManagerServicePage-include.png)

![Video var](./media/storsimple-create-new-service/Video_icon.png) **Video var**

Yeni bir StorSimple Yöneticisi hizmetinin nasıl oluşturulduğunu gösteren bir video izlemek için [buraya](https://azure.microsoft.com/documentation/videos/create-a-storsimple-manager-service/) tıklayın.

