## 1. Mở đầu về K8S

- K8S là một dự án mã nguồn mở phục vụ nhu tự động triển khai mở rộng, quản lý các ứng dụng được đóng gói trên nền tảng Container
- K8S thực hiện nhóm logic các container của một dịch vụ nhằm mục đích dễ dàng quản lý và liên kết. K8S được Google xây dựng với tâm huyết trong 15 chạy workload các ứng dụng của họ.
- K8S được sử dụng trong nội bộ Google với khả năng quản lý tới hàng triệu Container của họ, khả năng cung cấp mở rộng ứng dụng của K8S mà không cần tác động của ops team là một trong những điểm nổi bật của K8S.
- K8S là một dự án mã nguồn mở có thể chạy trên các hệ thống tại chỗ, các dịch vụ Cloud.

## 2. Một số chức năng của K8S

- Liên kết các dịch vụ và cân bằng tải: Không cần chỉnh sửa các cấu hình trong dịch vụ để liên kết một service mới trong cấu trúc microservice. K8S cung cấp mỗi Pods một địa chỉ IP riêng và một DNS riêng cho một danh sách Pod và có thể thực hiện cân bằng tải giữa các container trong dịch vụ.
- Khả năng mở rộng các network endpoint cho K8S Cluster nhằm tăng hiệu năng.
- K8S hỗ trợ lưu trữ tại chỗ, hoặc các dịch vụ lưu trữ bên ngoài như iSCSI, NFS, GlusterFS, CEPH, Cinder, GPC
- Tự động triển khai và rollback ứng dụng
- Hỗ trợ dual-stack IPv4 IPv6 cho các Pods
- Định tuyến lưu trọng các dịch vụ dựa vào kiến trúc Cluster
- Tự động lựa chọn vị trí để đặt các Container dựa vào tài nguyên yêu cầu.
- Khả năng phục vụ các Contaner bị lỗi, tự động di chuyển các container trên các node bị chết
- Hỗ trợ thao tác qua Bash CLI hoặc Dashboard

END
