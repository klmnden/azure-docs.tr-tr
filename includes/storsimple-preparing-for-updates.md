<!--author=jgerend last changed: 03/16/16-->

## <a name="preparing-for-updates"></a>Güncelleştirmeler için hazırlama
Tarama ve güncelleştirmeyi uygulamadan önce aşağıdaki adımları gerçekleştirmesi gerekir:

1. Bir bulut anlık görüntüsünü cihaz verileri alın.
2. IP'leri sabit denetleyicinizi yönlendirilebilir ve Internet'e bağlanabildiğinizden emin olun. Bu sabit IP'leri Cihazınızı güncelleştirmeleri hizmet vermek için kullanılır. Bu cihazı Windows PowerShell arabiriminden her denetleyicisinde aşağıdaki cmdlet'i çalıştırarak test edebilirsiniz:
   
     `Test-Connection -Source <Fixed IP of your device controller> -Destination <Any IP or computer name outside of datacenter network> `
   
    **Örnek çıktı için sabit IP'ler Internet'e bağlanabildiğinizde Bağlantıyı Sına**

        Controller0>Test-Connection -Source 10.126.173.91 -Destination bing.com

        Source      Destination     IPV4Address      IPV6Address
        ----------------- -----------  -----------
        HCSNODE0  bing.com        204.79.197.200
        HCSNODE0  bing.com        204.79.197.200
        HCSNODE0  bing.com        204.79.197.200
        HCSNODE0  bing.com        204.79.197.200

        Controller0>Test-Connection -Source 10.126.173.91 -Destination  204.79.197.200

        Source      Destination       IPV4Address    IPV6Address
        ----------------- -----------  -----------
        HCSNODE0  204.79.197.200  204.79.197.200
        HCSNODE0  204.79.197.200  204.79.197.200
        HCSNODE0  204.79.197.200  204.79.197.200
        HCSNODE0  204.79.197.200  204.79.197.200

Bu el ile ön denetimleri başarıyla tamamladıktan sonra taramak ve güncelleştirmeleri yüklemek için devam edebilirsiniz.

