# Điều khiển BLDC motor bằng esp8266

Mục đích sử dụng vi điều khiển esp8266 tạo xung PMW điều tốc các loại motor không chổi than dùng xung PWM, đặc biệt các model chuyên chạy quạt của Nidec có 3 dây (VCC, GND, PWM) rất dễ mua trên các trang bán hàng online như Shopee, Lazada,... Sau đó, tích hợp vào Home Assistant, và Homekit để điều khiển bằng giọng nói qua Siri.

![516b8f9af2a894ad63a6f6362e2707eb](https://user-images.githubusercontent.com/56484469/130795755-a8e43cad-ace3-4660-a304-ab39391690a2.jpg)

**Lưu ý:** Code viết dựa trên Esphome, một add-on của Home Assistant nên yêu cầu phải có sẵn Home Assistant và Esphome để compile thành firmware.

## Vật tư cần thiết
* Motor BLDC PWM (chạy 12-24V)
* D1 mini
* Rotary encoder
* Ổn áp 7805
* Tụ 35V 470mF
* Terminal 2P
* Công tắc
* Header 3P, 5P kèm dây ra
* Bảng mạch trống
* Dây điện

## Sơ đồ đấu nối
