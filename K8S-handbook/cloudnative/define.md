

## Định nghĩa Cloud Native

![Pivotal](https://tanzu.vmware.com/) là đã đổi thành  VMware Tanzu là nơi khởi xướng cho cloud-native, họ đã phát hành nền tảng  Pivotal Cloud Foundry và thực hiện mã nguồn mở cho Spring ( Java deployment framework ), trở  thành những người đầu tiên dẫn đường và xây dựng kiến trúc cloud-native

## Đinh nghĩa ban đầu về Cloud Nactive của Pivotal

- Vào năm 2015, Pivotal đã định nghĩa kiến trúc của một ứng dụng cloud-native
    - Meet the 12-factor application
    - Microservice-oriented architecture
    - Self-service agile architecture
    - API-based collaboration
    - Anti-fragility

## Định nghĩa ban đầu về Cloud-native của CNCF
- Vào năm 2015, Google đã thành lập Cloud Native Computing Foundation ( CNCF), CNCF xác định Cloud Native bao gồm:
    - Các ứng dụng sẽ dưới dạng containerization
    - Kiến trúc microservice
    - Ứng dụng hỗ trợ môi trường containear


## Định nghĩa lại

- Năm 2018, nhiều hệ sinh thái Cloud phát triển lớn mạnh, các vendor chính trong cloud computing bắt đầu tham gia CNCF, có thể theo dõi các project tại đây ![ Cloud Native Landscape](https://landscape.cncf.io/). Tuy nhiên CNCF ngày càng mở rộng và đón nhất các project ban đầu không theo xu hướng Cloud-Native, cho nên định nghĩa về Cloud-Native đã thay đổi

Các công nghệ Cloud Native cho phép các tổ chức xây dựng và mở rộng các ứng dụng trên các môi trường linh hoạt và linh động như public private và hybird cloud. Container, service mesh, microservice, cơ sở hạ tầng không thay đổi, làm việc thông qua API là những tiêu chí hàng đầu

- Có thểm tham khảo chuỗi tài viết tại : https://blog.vietstack.vn/cloud-native-app-in-cloud-part-1/ để tìm hiểu thêm về Cloud Native
- https://kubernetes.io/blog/2019/05/17/kubernetes-cloud-native-and-the-future-of-software/



## Triết lý thiết kế dựa trên nền tảng đám mây

. Trong Cloud Native, bản thân Cloud không được gọi là kiến trúc, đây chỉ là hệ thống. Các ứng dụng chạy trên hệ thống đó mới được gọi là cloud native application. Chỉ những ứng dụng được thiết kế với triết lý dưới đây mới được gọi là ứng dụng được xây dựng trên kiến trúc cloud-native
For distributed design (Distribution): container, microservices, API-driven development;
- Configuration-oriented design (Configuration): one image, multiple environment configurations;
- Resilience design (Resistancy): fault tolerance and self-healing;
- Elasticity oriented: Elastic expansion and response to environmental changes (load);
- Delivery-oriented design (Delivery): automatically pulled up to shorten delivery time;
- Performance-oriented design (Performance): responsive, concurrency and efficient use of resources;
- Automation design (Automation): automated DevOps;
- Diagnosability-oriented (Diagnosability): cluster-level logging, metric and tracking;
- Security-oriented design (Security): secure endpoint, API Gateway, end-to-end encryption;