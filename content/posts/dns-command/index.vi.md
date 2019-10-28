+++
title = "Thay đổi máy chủ DNS đơn giản bằng dòng lệnh"
date = "2019-10-19T14:53:23.322Z"
authors = ["stanleynguyen"]
cover = "/posts/dns-command/img/cover.png"
tags = ["dns", "privacy", "terminal", "network"]
keywords = ["dns", "privacy", "terminal", "bash", "network", "nameserver"]
description = "Hướng dẫn thay đổi máy chủ DNS nhanh chóng bằng dòng lệnh để không bị chặn bởi điểm truy cập"
showFullContent = false
+++

### Bài toán

Tất cả chúng ta, ở một thời điểm nào đó, đều ít nhất một lần thay đổi máy chủ DNS trên máy tính.
Dù là để tránh kiểm duyệt, hay tốc độ truy cập, hay bảo mật, hay để tránh hạn chế nội dung, chúng ta đều đã từng thay tên máy chủ DNS dùng máy chủ công cộng ví dụ như máy chủ của Google (chắc bạn vẫn nhớ 8.8.8.8 hay 8.8.4.4 chứ?)
Tôi cũng không phải là ngoại lệ, dùng [1.1.1.1](https://1.1.1.1/dns/) để bảo mật thông tin và tăng tốc độ truy cập.
Tuy nhiên, dùng những máy chủ DNS riêng này đi cùng một số hạn chế cho những ai hay di chuyển và phải dùng các mạng truy cập khác nhau.
Bạn sẽ nhận được cái này nếu như điểm truy cập mà bạn dùng chặn máy chủ của bạn

![blocked by network](/posts/dns-command/img/blocked.png)

Một vấn đề nữa là bạn sẽ không thể vào được trang đăng nhập của điểm truy cập với máy chủ DNS riêng.
Tất nhiên là việc thay đổi máy chủ DNS trở lại như cũ không hề khó vì bạn còn thay đổi được nó thành máy chủ riêng cơ mà?
Nhưng mà nếu bạn lại cần các máy chủ ấy sau đó thì sao?
Việc đổi đi đổi lại máy chủ DNS với tôi quá mệt mỏi nên tôi quyết định viết một cái script ngắn bằng bash để khỏi phải tự đổi nữa.

### 1.1.1.1

Trước khi đi vào phần chính, tôi muốn nhắc đến dự án tuyệt vời nhờ sự hợp tác của [Cloudflare](https://www.cloudflare.com/) and [APNIC](https://www.apnic.net/) - [1.1.1.1](https://1.1.1.1/dns/).
Dự án với mục đích giúp cho người dùng Internet có tốc độ cao hơn và bảo mật thông tin hơn (Máy chủ này nhanh gấp 2 lần máy chủ của Google 🚀🚀🚀).
Nhưng trên hết, nó đảm bảo **bí mật thông tin tuyệt đối**.
Bạn nghĩ rằng chỉ có những công ty công nghệ lớn như Google, Facebook theo dõi bạn (mà có thể chặn với [Privacy Badger](https://www.eff.org/privacybadger))?
Nhà cung cấp mạng của bạn cũng theo dõi nhưng trang web mà bạn truy cập và bán thông tin đó cho những tay quảng cáo, và họ làm điều đó bằng - không gì khác ngoài máy chủ DNS!

1.1.1.1 bảo vệ bạn khỏi những mối nguy hại đó với giả chỉ 0 đồng (vâng, bạn không đọc nhầm đâu).
Một máy chủ DNS **MIỄN PHÍ**, nhanh, bảo mật - quá khó tin phải không?
Ý nguyện của dự án này là để gây dựng một mạng Internet tốt đẹp hơn.
Bạn không cần phải tin suông vào lời của mình, đọc bài viết của [Cloudflare's](https://blog.cloudflare.com/announcing-1111/) và [APNIC's](https://labs.apnic.net/?p=1127) về nó.

### Thay đổi máy chủ bằng lệnh bash

Như là đã viết ở trên, việc thay đổi máy chủ DNS bằng UI Cài Đặt Mạng là quá phiền hà đối với tôi, nhất là khi cứ phải chuyển qua chuyển lại.
Như thường lệ, những gì quá phiền hà qua UI thì chúng ta sẽ thử làm nó bằng dòng lệnh.
Đó là khi mà tôi tìm thấy một ứng dụng terminal rất hữu ích trên nền tảng macOS - `networksetup`.
Với nó, việc thay đổi máy chủ DNS chỉ đơn giản là

```bash
# Thêm 1.1.1.1 vào danh sách máy chủ của tôi
networksetup -setdnsservers Wi-Fi 1.1.1.1
# Xoá tất cả các máy chủ riêng
networksetup -setdnsservers Wi-Fi empty
```

Giờ tôi có thể đơn giản thay đổi tên máy chủ theo hướng dẫn trên 1.1.1.1

```bash
# 4 máy chủ - 2 IPv4 & 2 IPv6
networksetup -setdnsservers Wi-Fi 1.1.1.1 1.0.0.1 2606:4700:4700::1111 2606:4700:4700::1001
```

Nhưng dòng lệnh này vẫn quá dài để viết và nhớ (mặc dù vẫn tốt hơn là dùng UI) nên tôi gói nó trong một hàm bash

```bash
function dns1111() {
    networksetup -setdnsservers Wi-Fi 1.1.1.1 1.0.0.1 2606:4700:4700::1111 2606:4700:4700::1001
}
```

Trông nó có vẻ khá tốt rồi phải không? Cơ mà khoan, còn đổi ngược lại về ban đầu thì sao?
Một đối số có lẽ sẽ khá đơn giản và hoàn thành được mục đích này, nhưng tôi muốn viết càng ít càng tốt và không muốn phải nhớ là mình đang dùng máy chủ DNS nào.
Với ý nghĩ đó, tôi tự hỏi tại sao mình lại không nhận diện xem là tên máy chủ nào đang được dùng và thay đổi tuỳ theo trạng thái đó?
Và tôi đã tìm thấy lựa chọn này trong networksetup mà sẽ thông báo cho tôi biết những máy chủ đang được dùng

```bash
networksetup -getdnsservers Wi-Fi
```

Nó sẽ liệt kê địa chỉ của các máy chủ đang được dùng, hoặc là in ra `There aren't any DNS Servers set on Wi-Fi` nếu không có máy chủ nào.
Giờ tôi có thể hoàn thành hàm của mình, với một số emojis hiển thị tuỳ theo các máy chủ riêng vừa được cài đặt hay không.

```bash
function dns1111() {
  if [[ $(networksetup -getdnsservers Wi-Fi) == "There aren't any DNS Servers set on Wi-Fi"* ]]; then
    networksetup -setdnsservers Wi-Fi 1.1.1.1 1.0.0.1 2606:4700:4700::1111 2606:4700:4700::1001 && echo 🚀 🚀 🚀
  else
    networksetup -setdnsservers Wi-Fi empty && echo 🚦 🚦 🚦
  fi
}
```

Giờ tôi có thể đơn giản chuyển giữa các máy chủ của 1.1.1.1 và máy chủ mặc định chỉ với một câu lệnh `dns1111`

### Bonus: Trên máy Linux

Vì `networksetup` không có trên các hệ điều hành thuộc gia đình Linux, chúng ta sẽ cần dùng một ứng dụng khác để thiết lập các máy chủ - `dnsmasq`.

Cài nó bằng package manager của bạn

```bash
# cài này dùng trên Ubuntu
sudo apt-get install dnsmasq
```

Cài gói này sẽ cho chúng ta một config file `/etc/dnsmasq.conf` mà có thể dùng để thay đổi máy chủ DNS.
Tôi viết `dns1111` cho các hệ điều hành Linux như dưới đây.
Nếu bạn nghĩ cách làm này chưa phải là tối ưu nhất, hãy liên lạc ngay với tôi qua [hung.ngn.the@gmail.com](mailto:hung.ngn.the@gmail.com)

```bash
function dns1111() {
  # xác định xem là có file backup nào không để nhận định là máy chủ riêng
  # đã được cài đặt hay chưa
  if [ -e /etc/dnsmasq.conf.bak ]]; then
    sudo mv /etc/dnsmasq.conf.bak /etc/dnsmasq.conf && echo 🚦 🚦 🚦
  else
    sudo cp /etc/dnsmasq.conf /etc/dnsmasq.conf.bak && \
    sudo echo "server=1.1.1.1
server=1.0.0.1
server=2606:4700:4700::1111
server=2606:4700:4700::1001" >> /etc/dnsmasq.conf && \
    sudo service dnsmasq restart && \
    sudo service network-manager restart && \
    echo 🚀 🚀 🚀
  fi
}
```
