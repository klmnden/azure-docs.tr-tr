## Yedekleme ilkesi tanımlama

Yedekleme ilkesi veri anlık görüntülerinin ne zaman alınacağının ve bu anlık görüntülerin ne kadar süreyle saklanacağının bir matrisini tanımlar. VM yedeklemesi için bir ilke tanımlandığında, yedekleme işini *günde bir kez* tetikleyebilirsiniz. Yeni bir ilke oluşturduğunuzda bu ilke kasaya uygulanır. Yedekleme İlkesi arabirimi şöyle görünür:

![Yedekleme ilkesi](./media/backup-create-policy-for-vms/backup-policy.png)

İlke oluşturmak için:

1. **İlke adı** için bir ad girin.

2. Verilerinizin anlık görüntüleri Günlük veya Haftalık aralıklarla alınabilir. Veri anlık görüntülerinin günlük mü, yoksa haftalık mı olacağını seçmek için **Yedekleme Sıklığı** açılan menüsünü kullanın.

    - Günlük aralığını seçerseniz, anlık görüntünün alınacağı günün saatini seçmek için vurgulanan denetimi kullanın. Saati değiştirmek için saat seçimini kaldırıp yeni saati seçin.

    ![Günlük yedekleme ilkesi](./media/backup-create-policy-for-vms/backup-policy-daily.png) <br/>

    - Haftalık aralığını seçerseniz, anlık görüntü alınacak haftanın günlerini ve günün saatini seçmek için vurgulanan denetimleri kullanın. Gün menüsünde, bir veya birden çok gün seçin. Saat menüsünde bir saat seçin. Saati değiştirmek için seçili saatin seçimini kaldırıp yeni saati seçin.

    ![Haftalık yedekleme ilkesi](./media/backup-create-policy-for-vms/backup-policy-weekly.png)

3. Varsayılan olarak, tüm **Elde Tutma Aralığı** seçenekleri seçilidir. Kullanmak istemediğiniz elde tutma aralığı sınırının seçimini kaldırın. Ardından, kullanılacak aralıkları belirtin.

    Aylık ve Yıllık elde tutma aralıkları günlük veya haftalık artışı temel alan anlık görüntüleri belirtmenizi sağlar.

    >[AZURE.NOTE] VM korurken yedekleme işi günde bir kez çalıştırılır. Yedeklemenin çalıştırıldığı saat her elde tutma aralığı için olanla aynıdır.

4. İlkeyle ilgili tüm seçeneklerin ayarlanmasından sonra, dikey pencerenin en üstünde **Kaydet**’e tıklayın.

    Yeni ilke hemen kasaya uygulanır.


<!--HONumber=Sep16_HO3-->


