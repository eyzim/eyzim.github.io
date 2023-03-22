# 排除 Ubuntu 20.04 網路問題


這篇是為了記錄 Ubuntu 明明 ifconfig 有 IP 卻無連線上網的問題。

在以 DHCP 作為得到 IP 的管道，但 ping 不到外網，或是 GUI 不能連上 `google.com`, `github.com` 等網址。

{{< image src="/images/set_network_for_ubuntu/set_network_for_ubuntu_check_net.png"  height="200"
          src_s="/images/set_network_for_ubuntu/set_network_for_ubuntu_check_net.png"
          src_l="/images/set_network_for_ubuntu/set_network_for_ubuntu_check_net.png"
alt="`ifconfig` and check the name of lan" caption="`ifconfig` and check the name of lan" linked="false">}}

上圖中，我們可以看到 lan 的名稱是 enp5s0，請根據自己要設定的 lan 名稱進行設定哦。

## 燒 mac address

拿到機器時，最好先在 cmd tool 下 `ifconfig` 確認機器是否有 MAC address，

{{< image src="/images/set_network_for_ubuntu/set_network_for_ubuntu_check_mac_address.png"  height="200"
          src_s="/images/set_network_for_ubuntu/set_network_for_ubuntu_check_mac_address.png"
          src_l="/images/set_network_for_ubuntu/set_network_for_ubuntu_check_mac_address.png"
alt="`ifconfig` and check the mac address of lan" caption="`ifconfig` and check the mac address of lan" linked="false">}}

圖中的 ether 部分就是 MAC address。

如果看起來是 00:11:22:33:44:55，最好自己燒一個上去。

1. 先關掉這個 lan

    ```
    sudo ifconfig enp5s0 down
    ```

2. 加入 mac address

    ```
    sudo ifconfig enp5s0 hw ether AA:BB:CC:DD:EE:FF (mac address)
    ```

3. 重新開啟 lan

    ```
    sudo dhclient enp5s0
    ```

## 重新跟 DHCP 要一個新的 IP

在已經確認網路線有插好，Lan 燈有亮後，還可能是 DHCP ~~太廢~~

也許是其他人亂設 static IP 造成問題，這時我們就只好重啟網路，跟 DHCP 重新要一個新的 IP

1. 一樣關閉 lan

    ```
    sudo ifconfig enp5s0 down
    ```

2. 重啟 lan

    ```
    sudo ifconfig enp5s0 up
    ```

3. ~~強迫~~跟 DHCP 重新要一個 IP

    ```
    sudo dhclient enp5s0
    ```

## Reference

1. https://fostips.com/find-mac-address-ubuntu/
2. https://www.cyberciti.biz/faq/howto-linux-renew-dhcp-client-ip-address/

