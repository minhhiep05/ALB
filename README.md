# DevOps Interview Handbook

# Part 1 - Introduce Your GitOps Project

---

## Câu 1. Hãy giới thiệu project của bạn trong khoảng 3 phút.

### Trả lời

Project của em là xây dựng một hệ thống GitOps CI/CD trên AWS nhằm tự động hóa toàn bộ quá trình triển khai ứng dụng lên Kubernetes.

Hạ tầng được triển khai bằng Terraform trên AWS EC2 bao gồm 3 Master Node và 2 Worker Node để mô phỏng môi trường Kubernetes High Availability.

Source code được quản lý trên GitLab. Khi Developer push code hoặc tạo Git Tag, GitLab CI sẽ tự động build Docker Image, quét lỗ hổng bảo mật bằng Trivy và push Image lên Harbor Registry.

Pipeline sau đó tự động cập nhật Helm Values Repository. ArgoCD sẽ theo dõi Repository này và tự động đồng bộ ứng dụng lên Kubernetes theo mô hình GitOps.

Hệ thống sử dụng NGINX Ingress Controller để expose ứng dụng ra Internet, cert-manager kết hợp Let's Encrypt để cấp TLS Certificate tự động.

Để giám sát hệ thống em triển khai Prometheus, Grafana và Loki. Cuối cùng sử dụng Velero để backup Kubernetes Cluster lên Amazon S3 phục vụ Disaster Recovery.

---

## Câu 2. Mục tiêu của project là gì?

### Trả lời

Mục tiêu của project là mô phỏng một quy trình triển khai ứng dụng gần giống môi trường Production.

Project giúp em học cách triển khai hạ tầng bằng Terraform, xây dựng Kubernetes Cluster, tự động hóa CI/CD bằng GitLab CI, áp dụng GitOps với ArgoCD và triển khai hệ thống Monitoring cũng như Backup cho Kubernetes.

Thông qua project này em hiểu được toàn bộ vòng đời triển khai ứng dụng từ khi Developer commit source code cho đến khi ứng dụng chạy trên Kubernetes.

---

## Câu 3. Vì sao em chọn GitOps?

### Trả lời

Em chọn GitOps vì Git trở thành Single Source of Truth.

Toàn bộ manifest Kubernetes và Helm Chart đều được quản lý trên Git.

ArgoCD sẽ liên tục so sánh trạng thái thực tế của Cluster với trạng thái mong muốn trong Git Repository.

Nếu có sự khác biệt ArgoCD sẽ tự động đồng bộ.

GitOps giúp hệ thống dễ audit, dễ rollback, tăng tính tự động và giảm thao tác thủ công.

---

## Câu 4. GitOps khác CI/CD truyền thống như thế nào?

### Trả lời

Trong CI/CD truyền thống, Pipeline sẽ sử dụng kubectl hoặc Helm để deploy trực tiếp lên Kubernetes.

Trong GitOps, Pipeline không deploy trực tiếp.

Pipeline chỉ cập nhật Repository chứa Helm Values hoặc Kubernetes Manifest.

ArgoCD sẽ theo dõi Repository đó và tự động triển khai lên Kubernetes.

Điều này giúp toàn bộ thay đổi đều được lưu trên Git và dễ kiểm soát hơn.

---

## Câu 5. Vì sao em chọn AWS?

### Trả lời

AWS là Cloud Platform phổ biến nhất hiện nay và được sử dụng rộng rãi trong doanh nghiệp.

AWS cung cấp đầy đủ các dịch vụ cần thiết để xây dựng hệ thống DevOps như EC2, VPC, IAM, Route53, S3 và Load Balancer.

Ngoài ra AWS cũng dễ tích hợp với Terraform và Kubernetes.

---

## Câu 6. Vì sao không dùng EKS?

### Trả lời

Mục tiêu của project là học Kubernetes từ nền tảng.

Nếu sử dụng EKS thì Control Plane sẽ do AWS quản lý.

Em sẽ không có cơ hội cài đặt kubeadm, cấu hình etcd, API Server, Scheduler và Controller Manager.

Do đó em chọn kubeadm để hiểu toàn bộ kiến trúc Kubernetes.

---

## Câu 7. Vì sao dùng kubeadm?

### Trả lời

kubeadm là công cụ chính thức của Kubernetes để khởi tạo Cluster.

Nó giúp triển khai Kubernetes gần giống môi trường thực tế nhưng vẫn cho phép quản trị toàn bộ Control Plane.

Qua kubeadm em hiểu được cách các thành phần của Kubernetes hoạt động thay vì sử dụng dịch vụ Managed Kubernetes.

---

## Câu 8. Vì sao triển khai 3 Master và 2 Worker?

### Trả lời

Mục tiêu là mô phỏng High Availability.

Ba Master Node giúp đảm bảo quorum của etcd.

Nếu một Master gặp sự cố thì hai Master còn lại vẫn duy trì hoạt động của Control Plane.

Hai Worker Node giúp ứng dụng vẫn hoạt động khi một Worker bị lỗi.

---

## Câu 9. Data Flow của project như thế nào?

### Trả lời

Developer commit source code lên GitLab.

GitLab CI được kích hoạt và build Docker Image.

Image được quét bảo mật bằng Trivy.

Sau khi đạt yêu cầu Image được push lên Harbor.

Pipeline cập nhật Helm Values Repository với Image Tag mới.

ArgoCD phát hiện thay đổi và đồng bộ Deployment lên Kubernetes.

Prometheus thu thập Metrics.

Grafana hiển thị Dashboard.

Loki thu thập Logs.

Velero backup tài nguyên Kubernetes lên Amazon S3.

---

## Câu 10. Vì sao dùng Harbor thay vì Docker Hub?

### Trả lời

Harbor là Private Container Registry.

Harbor hỗ trợ quản lý người dùng, phân quyền, Robot Account, Image Replication, Vulnerability Scanning và Retention Policy.

Trong doanh nghiệp việc sử dụng Private Registry sẽ an toàn hơn Docker Hub Public.

---

## Câu 11. Vì sao dùng Helm?

### Trả lời

Helm giúp quản lý Kubernetes Manifest theo dạng Template.

Thay vì phải sửa nhiều file YAML, em chỉ cần thay đổi values.yaml.

Helm giúp tái sử dụng cấu hình, dễ quản lý nhiều môi trường và đơn giản hóa việc nâng cấp ứng dụng.

---

## Câu 12. Vì sao dùng ArgoCD thay vì Jenkins Deploy?

### Trả lời

Nếu Jenkins deploy trực tiếp thì Jenkins phải có quyền truy cập Kubernetes Cluster.

GitOps với ArgoCD giúp tách biệt CI và CD.

GitLab chỉ chịu trách nhiệm Build.

ArgoCD chịu trách nhiệm Deploy.

Điều này giúp hệ thống an toàn hơn và dễ kiểm soát hơn.

---

## Câu 13. Vì sao dùng Prometheus?

### Trả lời

Prometheus được sử dụng để thu thập Metrics của Kubernetes Cluster và ứng dụng.

Các Metrics này giúp theo dõi CPU, Memory, Network, Pod Status và tạo Alert khi hệ thống gặp sự cố.

---

## Câu 14. Vì sao dùng Grafana?

### Trả lời

Grafana giúp trực quan hóa dữ liệu từ Prometheus.

Dashboard của Grafana cho phép quan sát tình trạng Cluster theo thời gian thực và hỗ trợ phân tích sự cố nhanh hơn.

---

## Câu 15. Vì sao dùng Loki?

### Trả lời

Prometheus chỉ lưu Metrics còn Loki được sử dụng để lưu Log.

Kết hợp Grafana và Loki giúp em vừa theo dõi Metrics vừa xem Log của Pod trên cùng một giao diện, giúp việc Troubleshooting hiệu quả hơn.


# Part 2 - Terraform Interview

---

## Câu 16. Tại sao em chọn Terraform thay vì tạo EC2 bằng AWS Console?

### Trả lời

Em chọn Terraform vì Terraform giúp quản lý toàn bộ Infrastructure dưới dạng mã nguồn (Infrastructure as Code).

Toàn bộ hạ tầng được định nghĩa trong các file .tf và lưu trên Git.

Điều này giúp:

- Triển khai tự động.
- Dễ tái sử dụng.
- Có thể version control.
- Hạn chế lỗi do thao tác thủ công.
- Dễ rollback khi thay đổi.

Trong project của em, toàn bộ EC2, VPC, Security Group, Route Table, Internet Gateway và IAM đều được tạo bằng Terraform.

---

## Câu 17. Terraform là gì?

### Trả lời

Terraform là công cụ Infrastructure as Code do HashiCorp phát triển.

Terraform sử dụng file cấu hình để mô tả hạ tầng và tự động tạo hoặc cập nhật tài nguyên trên nhiều Cloud Provider như AWS, Azure và GCP.

Terraform hoạt động theo mô hình Declarative, nghĩa là chỉ cần khai báo trạng thái mong muốn, Terraform sẽ tự tính toán các bước cần thực hiện.

---

## Câu 18. Terraform hoạt động như thế nào?

### Trả lời

Quy trình hoạt động gồm:

1. Viết file Terraform.
2. Chạy terraform init để tải Provider.
3. Chạy terraform plan để xem các thay đổi.
4. Chạy terraform apply để tạo hạ tầng.
5. Terraform lưu trạng thái vào State File.
6. Những lần chạy sau Terraform sẽ so sánh State với Infrastructure thực tế để quyết định Update hay Create.

---

## Câu 19. Terraform Provider là gì?

### Trả lời

Provider là plugin giúp Terraform giao tiếp với Cloud Provider.

Trong project của em em sử dụng AWS Provider.

Terraform sẽ gọi AWS API thông qua Provider để tạo EC2, VPC, IAM, Route53 và các tài nguyên khác.

---

## Câu 20. Resource là gì?

### Trả lời

Resource là thành phần mô tả một tài nguyên cụ thể.

Ví dụ:

- aws_instance
- aws_vpc
- aws_subnet
- aws_security_group

Mỗi Resource tương ứng với một tài nguyên thật trên AWS.

---

## Câu 21. Terraform State là gì?

### Trả lời

Terraform State là file lưu trạng thái của Infrastructure.

State giúp Terraform biết tài nguyên nào đã được tạo, ID của tài nguyên đó và những thuộc tính hiện tại.

Nhờ State Terraform có thể so sánh giữa trạng thái mong muốn và trạng thái thực tế để quyết định Create, Update hay Destroy.

---

## Câu 22. Vì sao cần Terraform State?

### Trả lời

Nếu không có State Terraform sẽ không biết tài nguyên nào đã tồn tại.

Mỗi lần Apply có thể tạo lại toàn bộ hạ tầng hoặc gây lỗi.

State giúp:

- Theo dõi tài nguyên.
- Tăng tốc Plan.
- Giảm số lượng API Call.
- Tránh tạo trùng Resource.

---

## Câu 23. Local State và Remote State khác nhau như thế nào?

### Trả lời

Local State lưu file terraform.tfstate trên máy tính.

Remote State lưu State trên một dịch vụ trung tâm như Amazon S3.

Trong doanh nghiệp Remote State được sử dụng phổ biến vì nhiều thành viên có thể làm việc trên cùng một Infrastructure.

Project của em sử dụng Amazon S3 làm Remote Backend.

---

## Câu 24. Vì sao em dùng S3 Backend?

### Trả lời

Em sử dụng Amazon S3 để lưu Terraform State vì:

- Không phụ thuộc vào máy cá nhân.
- Có thể chia sẻ cho cả nhóm.
- Hỗ trợ Versioning.
- Dễ Backup.
- Độ bền dữ liệu cao.

Điều này giúp việc cộng tác an toàn hơn so với Local State.

---

## Câu 25. Vì sao cần DynamoDB Lock?

### Trả lời

Nếu hai người cùng chạy terraform apply thì State có thể bị ghi đồng thời dẫn đến hỏng State.

DynamoDB được dùng để Lock State.

Khi một người Apply Terraform sẽ tạo Lock trong DynamoDB.

Người khác phải chờ Lock được giải phóng mới có thể Apply tiếp.

Điều này giúp tránh State Corruption.

---

## Câu 26. terraform init dùng để làm gì?

### Trả lời

terraform init được sử dụng để khởi tạo Project.

Nó sẽ:

- Tải Provider.
- Khởi tạo Backend.
- Kiểm tra Module.
- Chuẩn bị môi trường để chạy Terraform.

Đây là lệnh đầu tiên cần chạy khi clone một Terraform Project.

---

## Câu 27. terraform plan là gì?

### Trả lời

terraform plan sẽ so sánh Infrastructure hiện tại với file Terraform.

Kết quả hiển thị những thay đổi dự kiến như:

- Create
- Update
- Destroy

Lệnh này không tạo tài nguyên mà chỉ mô phỏng trước khi Apply.

---

## Câu 28. terraform apply là gì?

### Trả lời

terraform apply sẽ thực hiện các thay đổi được tính toán từ terraform plan.

Terraform sẽ gọi AWS API để tạo hoặc cập nhật Infrastructure.

Sau khi hoàn thành Terraform sẽ cập nhật lại State File.

---

## Câu 29. terraform destroy dùng để làm gì?

### Trả lời

terraform destroy được dùng để xóa toàn bộ Infrastructure do Terraform quản lý.

Terraform sẽ đọc State File để biết tài nguyên nào cần xóa rồi gọi AWS API tương ứng.

---

## Câu 30. Nếu interviewer hỏi "Trong project em Terraform tạo những gì?"

### Trả lời

Trong project của em Terraform tạo:

- VPC
- Public Subnet
- Private Subnet
- Internet Gateway
- Route Table
- Security Group
- EC2 Instance
- Elastic IP
- IAM Role
- Network Load Balancer
- S3 Backend
- DynamoDB Lock Table

Sau khi hoàn thành Terraform trả về Output để Ansible hoặc các bước triển khai tiếp theo có thể sử dụng.


# Part 2 - Terraform Interview (Tiếp theo)

---

## Câu 31. Variable trong Terraform là gì?

### Trả lời

Variable giúp truyền giá trị động vào Terraform thay vì hard-code trong file cấu hình.

Trong project của em em sử dụng Variable cho:

- AWS Region
- EC2 Instance Type
- Key Pair
- VPC CIDR
- Subnet CIDR
- Số lượng Master Node
- Số lượng Worker Node

Điều này giúp tái sử dụng code và dễ triển khai nhiều môi trường khác nhau.

---

## Câu 32. Output trong Terraform là gì?

### Trả lời

Output dùng để xuất các giá trị sau khi Terraform triển khai xong.

Ví dụ trong project em Output gồm:

- Public IP của Master Node
- Public IP của Worker Node
- DNS của Load Balancer
- VPC ID

Các Output này được Ansible hoặc các bước triển khai tiếp theo sử dụng.

---

## Câu 33. Module là gì?

### Trả lời

Module là cách đóng gói các Resource thành một thành phần có thể tái sử dụng.

Ví dụ em có thể tạo:

- Module VPC
- Module EC2
- Module Security Group

Sau đó chỉ cần truyền Variable để triển khai nhiều lần.

Module giúp code gọn hơn và dễ bảo trì.

---

## Câu 34. Vì sao nên dùng Module?

### Trả lời

Module giúp:

- Tái sử dụng code.
- Giảm trùng lặp.
- Dễ bảo trì.
- Chuẩn hóa Infrastructure.
- Dễ mở rộng.

Trong doanh nghiệp gần như mọi Terraform Project đều sử dụng Module.

---

## Câu 35. depends_on dùng để làm gì?

### Trả lời

Terraform thường tự tính Dependency.

Tuy nhiên trong một số trường hợp Terraform không xác định được thứ tự tạo Resource.

Khi đó em sử dụng depends_on để ép Terraform tạo Resource theo đúng thứ tự.

Ví dụ EC2 phải đợi Internet Gateway hoặc IAM Role được tạo trước.

---

## Câu 36. count dùng để làm gì?

### Trả lời

count giúp tạo nhiều Resource giống nhau.

Ví dụ:

count = 3

Terraform sẽ tạo ba EC2 Instance giống nhau.

Trong project em có thể dùng count để tạo nhiều Worker Node.

---

## Câu 37. for_each khác count như thế nào?

### Trả lời

count sử dụng số thứ tự.

for_each sử dụng key.

Nếu Resource có tên khác nhau hoặc cấu hình khác nhau thì em sẽ dùng for_each.

Trong doanh nghiệp for_each được dùng nhiều hơn vì dễ quản lý.

---

## Câu 38. lifecycle là gì?

### Trả lời

lifecycle giúp điều khiển hành vi của Terraform khi tạo hoặc cập nhật Resource.

Ví dụ:

- create_before_destroy
- prevent_destroy
- ignore_changes

Điều này giúp hạn chế downtime và tránh xóa nhầm Resource quan trọng.

---

## Câu 39. create_before_destroy là gì?

### Trả lời

Thông thường Terraform sẽ xóa Resource cũ rồi tạo Resource mới.

create_before_destroy sẽ tạo Resource mới trước.

Sau khi Resource mới hoạt động Terraform mới xóa Resource cũ.

Điều này giúp giảm thời gian downtime.

---

## Câu 40. ignore_changes dùng để làm gì?

### Trả lời

ignore_changes yêu cầu Terraform bỏ qua một số thuộc tính khi so sánh State.

Ví dụ nếu Tag được hệ thống khác cập nhật thì Terraform sẽ không cố gắng ghi đè.

Điều này tránh việc Apply liên tục không cần thiết.

---

## Câu 41. Terraform Refresh là gì?

### Trả lời

Refresh giúp Terraform đồng bộ State với hạ tầng thực tế.

Nếu có thay đổi ngoài Terraform thì Refresh sẽ cập nhật lại State.

Trong các phiên bản mới việc Refresh thường được thực hiện trong quá trình Plan.

---

## Câu 42. Terraform Drift là gì?

### Trả lời

Drift xảy ra khi Infrastructure thực tế khác với Terraform State.

Ví dụ một EC2 bị xóa bằng AWS Console.

Terraform sẽ phát hiện Drift khi chạy terraform plan và đề xuất tạo lại Resource.

---

## Câu 43. Có nên sửa tài nguyên bằng AWS Console không?

### Trả lời

Không nên.

Infrastructure nên được quản lý hoàn toàn bằng Terraform.

Nếu sửa trực tiếp trên Console sẽ dễ gây Drift và mất tính nhất quán.

Trong trường hợp khẩn cấp có thể sửa nhưng sau đó cần cập nhật lại Terraform.

---

## Câu 44. terraform import dùng để làm gì?

### Trả lời

terraform import dùng để đưa Resource đã tồn tại vào Terraform State.

Sau khi Import Terraform có thể quản lý Resource đó mà không cần tạo mới.

---

## Câu 45. terraform fmt dùng để làm gì?

### Trả lời

terraform fmt tự động chuẩn hóa định dạng code Terraform.

Điều này giúp toàn bộ Project có coding style thống nhất.

---

## Câu 46. terraform validate dùng để làm gì?

### Trả lời

terraform validate kiểm tra cú pháp và cấu trúc của Project.

Lệnh này không tạo Resource mà chỉ kiểm tra tính hợp lệ của file Terraform.

---

## Câu 47. Trong project của em Terraform tạo Resource theo thứ tự nào?

### Trả lời

Terraform sẽ tạo:

- VPC
- Internet Gateway
- Route Table
- Subnet
- Security Group
- IAM Role
- EC2 Instance
- Load Balancer

Terraform tự tính Dependency giữa các Resource nên em không cần chỉ định toàn bộ thứ tự bằng tay.

---

## Câu 48. Điều khó nhất khi dùng Terraform là gì?

### Trả lời

Theo em điều khó nhất là quản lý Terraform State và xử lý Dependency giữa các Resource.

Nếu State bị lỗi hoặc nhiều người cùng Apply mà không có Lock thì rất dễ gây hỏng Infrastructure.

Đó là lý do em sử dụng S3 Backend kết hợp DynamoDB Lock.

---

## Câu 49. Nếu S3 Backend bị xóa thì sao?

### Trả lời

Nếu S3 Backend bị xóa thì Terraform sẽ mất State File.

Khi đó Terraform không biết Infrastructure hiện tại và có thể đề xuất tạo lại toàn bộ Resource.

Do đó cần bật Versioning cho S3 và Backup định kỳ.

---

## Câu 50. Nếu interviewer hỏi: "Điều em học được nhiều nhất từ Terraform Project là gì?"

### Trả lời

Điều em học được nhiều nhất là tư duy quản lý hạ tầng bằng mã nguồn.

Em hiểu cách tổ chức Infrastructure theo dạng khai báo, quản lý State, sử dụng Remote Backend, triển khai hạ tầng có thể tái sử dụng và tự động hóa toàn bộ quá trình Provisioning trên AWS.

Qua project này em cũng hiểu rằng Terraform không chỉ giúp tạo Resource mà còn giúp chuẩn hóa quy trình triển khai hạ tầng trong môi trường DevOps.

# Part 3 - Kubernetes Interview

---

## Câu 51. Kubernetes là gì?

### Trả lời

Kubernetes là nền tảng mã nguồn mở dùng để quản lý và điều phối các container.

Kubernetes giúp tự động triển khai, mở rộng, tự phục hồi và quản lý vòng đời của ứng dụng chạy trên nhiều máy chủ.

Trong project của em Kubernetes được triển khai bằng kubeadm trên AWS EC2 gồm 3 Master Node và 2 Worker Node.

---

## Câu 52. Vì sao em chọn Kubernetes?

### Trả lời

Em chọn Kubernetes vì đây là nền tảng Container Orchestration phổ biến nhất hiện nay.

Kubernetes hỗ trợ:

- High Availability
- Auto Scaling
- Self Healing
- Rolling Update
- Service Discovery
- Load Balancing

Đây đều là những tính năng rất quan trọng trong môi trường Production.

---

## Câu 53. Kubernetes Cluster gồm những thành phần nào?

### Trả lời

Một Kubernetes Cluster gồm hai thành phần chính:

Control Plane

- kube-apiserver
- etcd
- kube-scheduler
- kube-controller-manager

Worker Node

- kubelet
- kube-proxy
- Container Runtime

Ngoài ra còn có CNI như Calico để quản lý Network.

---

## Câu 54. Control Plane có nhiệm vụ gì?

### Trả lời

Control Plane là trung tâm điều khiển của Kubernetes.

Nó tiếp nhận mọi yêu cầu từ kubectl hoặc API.

Sau đó quyết định Pod sẽ chạy ở đâu, lưu trạng thái Cluster và giám sát toàn bộ hệ thống.

Nếu Control Plane ngừng hoạt động thì Cluster sẽ không thể tạo mới hoặc cập nhật tài nguyên.

---

## Câu 55. kube-apiserver là gì?

### Trả lời

kube-apiserver là thành phần trung tâm của Kubernetes.

Mọi yêu cầu từ kubectl, Helm, ArgoCD hoặc các Controller đều phải thông qua API Server.

API Server xác thực người dùng, kiểm tra quyền truy cập rồi lưu trạng thái vào etcd.

---

## Câu 56. etcd là gì?

### Trả lời

etcd là cơ sở dữ liệu Key-Value dùng để lưu toàn bộ trạng thái của Kubernetes Cluster.

Ví dụ:

- Pod
- Node
- Deployment
- Secret
- ConfigMap

Nếu etcd bị mất thì Kubernetes sẽ mất toàn bộ trạng thái của Cluster.

Đó là lý do trong project em triển khai 3 Master để đảm bảo quorum của etcd.

---

## Câu 57. Vì sao etcd cần số lẻ Node?

### Trả lời

etcd sử dụng thuật toán Raft.

Raft yêu cầu đa số Node đồng ý trước khi ghi dữ liệu.

Ví dụ:

3 Node

→ cần ít nhất 2 Node hoạt động.

5 Node

→ cần ít nhất 3 Node hoạt động.

Do đó người ta luôn triển khai số lẻ Master Node.

---

## Câu 58. kube-scheduler làm gì?

### Trả lời

kube-scheduler quyết định Pod sẽ chạy trên Worker Node nào.

Scheduler sẽ xem xét:

- CPU
- RAM
- Affinity
- Taints
- Tolerations
- Resource Requests

Sau đó chọn Node phù hợp nhất.

---

## Câu 59. kube-controller-manager làm gì?

### Trả lời

Controller Manager liên tục theo dõi trạng thái Cluster.

Nếu trạng thái thực tế khác với trạng thái mong muốn thì Controller sẽ tự động điều chỉnh.

Ví dụ:

Deployment yêu cầu 3 Pod.

Nếu còn 2 Pod thì Controller sẽ tạo thêm 1 Pod.

Đây chính là cơ chế Self Healing.

---

## Câu 60. kubelet là gì?

### Trả lời

kubelet chạy trên mỗi Worker Node.

Nó nhận lệnh từ API Server rồi điều khiển Container Runtime để tạo hoặc xóa Pod.

Ngoài ra kubelet còn gửi trạng thái Node về Control Plane.

---

## Câu 61. kube-proxy là gì?

### Trả lời

kube-proxy quản lý Network Rule trên Node.

Nó giúp Service có thể chuyển tiếp lưu lượng đến đúng Pod.

Nếu không có kube-proxy thì Service sẽ không thể hoạt động đúng.

---

## Câu 62. Container Runtime là gì?

### Trả lời

Container Runtime là thành phần chịu trách nhiệm chạy Container.

Hiện nay Kubernetes chủ yếu sử dụng:

- containerd
- CRI-O

Trong project của em sử dụng containerd.

---

## Câu 63. Calico dùng để làm gì?

### Trả lời

Calico là một CNI Plugin.

Calico cung cấp:

- Pod Network
- Network Policy
- Routing giữa các Node

Nếu không có CNI thì Pod trên các Node khác nhau sẽ không thể giao tiếp.

---

## Câu 64. Vì sao em chọn Calico?

### Trả lời

Calico là CNI phổ biến, ổn định và dễ triển khai với kubeadm.

Ngoài việc cung cấp Pod Network, Calico còn hỗ trợ Network Policy giúp kiểm soát lưu lượng giữa các Pod.

---

## Câu 65. Pod là gì?

### Trả lời

Pod là đơn vị triển khai nhỏ nhất trong Kubernetes.

Một Pod có thể chứa một hoặc nhiều Container.

Các Container trong cùng Pod:

- dùng chung IP
- dùng chung Network Namespace
- có thể chia sẻ Volume.

Thông thường mỗi Pod chỉ chạy một ứng dụng chính.


## Câu 66. Pod và Container khác nhau như thế nào?

### Trả lời

Container là nơi chạy ứng dụng.

Pod là đơn vị triển khai nhỏ nhất của Kubernetes, bên trong Pod có thể chứa một hoặc nhiều Container.

Thông thường mỗi Pod chỉ chạy một ứng dụng chính.

Các Container trong cùng Pod:

- Chung IP
- Chung Network Namespace
- Có thể chia sẻ Volume

Pod mới là đối tượng Kubernetes quản lý chứ không phải Container.

---

## Câu 67. Deployment là gì?

### Trả lời

Deployment là đối tượng dùng để quản lý Pod.

Deployment giúp:

- Tạo Pod
- Rolling Update
- Rollback
- Self Healing
- Scale Pod

Trong project của em Deployment được dùng để triển khai ứng dụng Flask.

---

## Câu 68. ReplicaSet là gì?

### Trả lời

ReplicaSet đảm bảo luôn có đúng số lượng Pod mong muốn.

Ví dụ Deployment yêu cầu 3 Pod.

Nếu một Pod bị chết thì ReplicaSet sẽ tự tạo lại Pod mới.

Thông thường ReplicaSet được Deployment quản lý và em không tạo ReplicaSet trực tiếp.

---

## Câu 69. Deployment khác ReplicaSet như thế nào?

### Trả lời

ReplicaSet chỉ đảm bảo số lượng Pod.

Deployment quản lý ReplicaSet.

Deployment hỗ trợ:

- Rolling Update
- Rollback
- Version Management

Do đó trong thực tế gần như luôn sử dụng Deployment.

---

## Câu 70. Service là gì?

### Trả lời

Service cung cấp địa chỉ IP ổn định để truy cập Pod.

Do Pod có thể bị tạo lại và IP thay đổi nên Service đóng vai trò trung gian giữa Client và Pod.

Service giúp cân bằng tải giữa nhiều Pod.

---

## Câu 71. Có những loại Service nào?

### Trả lời

Có bốn loại chính:

ClusterIP

- Chỉ truy cập trong Cluster.

NodePort

- Truy cập từ bên ngoài thông qua IP Node và Port.

LoadBalancer

- Tích hợp với Cloud Provider để tạo Load Balancer.

ExternalName

- Ánh xạ Service tới DNS bên ngoài.

Trong project của em em sử dụng ClusterIP kết hợp NGINX Ingress.

---

## Câu 72. Vì sao em dùng Ingress thay vì NodePort?

### Trả lời

NodePort phải mở một Port riêng cho từng Service.

Ingress cho phép nhiều Service dùng chung cổng 80 và 443.

Ingress còn hỗ trợ:

- HTTPS
- TLS
- Host-based Routing
- Path-based Routing

Điều này phù hợp hơn với môi trường Production.

---

## Câu 73. NGINX Ingress Controller là gì?

### Trả lời

NGINX Ingress Controller là thành phần đọc Ingress Resource và cấu hình NGINX để chuyển tiếp request tới đúng Service.

Trong project của em NGINX Ingress Controller expose ứng dụng ra Internet và kết hợp cert-manager để tự động cấp HTTPS.

---

## Câu 74. ConfigMap là gì?

### Trả lời

ConfigMap dùng để lưu cấu hình không nhạy cảm.

Ví dụ:

- URL
- Hostname
- Environment Variable

ConfigMap giúp thay đổi cấu hình mà không cần build lại Docker Image.

---

## Câu 75. Secret là gì?

### Trả lời

Secret dùng để lưu dữ liệu nhạy cảm.

Ví dụ:

- Password
- API Key
- Token
- Certificate

Trong project của em em sử dụng Sealed Secrets để lưu Secret trên Git một cách an toàn.

---

## Câu 76. Vì sao không lưu Password trong ConfigMap?

### Trả lời

ConfigMap không mã hóa dữ liệu.

Bất kỳ ai có quyền đọc ConfigMap đều có thể xem nội dung.

Password nên lưu trong Secret hoặc Sealed Secrets để tăng tính bảo mật.

---

## Câu 77. HPA là gì?

### Trả lời

HPA là Horizontal Pod Autoscaler.

HPA tự động tăng hoặc giảm số lượng Pod dựa trên Metrics như:

- CPU
- Memory
- Custom Metrics

Trong project của em HPA scale ứng dụng từ 1 Pod lên 5 Pod khi CPU vượt ngưỡng cấu hình.

---

## Câu 78. HPA hoạt động như thế nào?

### Trả lời

Metrics Server thu thập CPU của Pod.

HPA định kỳ đọc Metrics.

Nếu CPU trung bình vượt ngưỡng thì HPA tăng Replica.

Khi CPU giảm xuống dưới ngưỡng trong một khoảng thời gian thì HPA giảm số lượng Pod.

Toàn bộ quá trình diễn ra tự động.

---

## Câu 79. Self Healing là gì?

### Trả lời

Self Healing là khả năng Kubernetes tự phục hồi khi Pod hoặc Node gặp sự cố.

Nếu Pod bị xóa hoặc Container bị Crash thì ReplicaSet sẽ tạo Pod mới.

Nếu Worker Node bị lỗi thì Scheduler sẽ chuyển Pod sang Node khác nếu Cluster còn đủ tài nguyên.

Trong project của em em kiểm chứng Self Healing bằng cách cordon và drain Worker Node.

---

## Câu 80. Rolling Update là gì?

### Trả lời

Rolling Update là cơ chế cập nhật ứng dụng mà không gây downtime.

Deployment sẽ tạo Pod mới trước, sau đó mới xóa Pod cũ.

Người dùng vẫn có thể truy cập ứng dụng trong suốt quá trình cập nhật.

Đây là cơ chế mặc định của Deployment trong Kubernetes.

# Part 3 - Kubernetes Interview (81-100)

---

## Câu 81. Rolling Update hoạt động như thế nào?

### Trả lời

Rolling Update là cơ chế cập nhật ứng dụng từng Pod một thay vì dừng toàn bộ hệ thống.

Ví dụ Deployment có 4 Pod.

Kubernetes sẽ:

- Tạo Pod mới.
- Chờ Pod mới Ready.
- Xóa 1 Pod cũ.
- Tiếp tục tạo Pod mới tiếp theo.

Quá trình lặp lại cho đến khi toàn bộ Pod được cập nhật.

Điều này giúp ứng dụng không bị downtime.

---

## Câu 82. Rollback là gì?

### Trả lời

Rollback là quá trình quay về phiên bản Deployment trước đó khi phiên bản mới gặp lỗi.

Kubernetes lưu lại lịch sử Deployment.

Khi Rollback Kubernetes sẽ sử dụng ReplicaSet cũ để khôi phục hệ thống.

Trong project của em ArgoCD cũng hỗ trợ rollback thông qua Git History.

---

## Câu 83. Namespace là gì?

### Trả lời

Namespace giúp chia Kubernetes Cluster thành nhiều môi trường logic khác nhau.

Ví dụ:

- dev
- staging
- production
- monitoring
- argocd

Namespace giúp:

- Tách tài nguyên.
- Dễ quản lý.
- Phân quyền RBAC.

Trong project của em em tách riêng namespace cho Monitoring, ArgoCD và Application.

---

## Câu 84. Resource Request là gì?

### Trả lời

Request là lượng tài nguyên tối thiểu mà Pod cần.

Scheduler sẽ dựa vào Request để quyết định Pod chạy trên Node nào.

Ví dụ:

CPU Request = 500m

Memory Request = 512Mi

Scheduler chỉ chọn Node còn đủ tài nguyên.

---

## Câu 85. Resource Limit là gì?

### Trả lời

Limit là lượng tài nguyên tối đa Pod được phép sử dụng.

Nếu Container vượt quá Memory Limit thì Kubernetes sẽ OOMKill Container.

CPU vượt Limit sẽ bị giới hạn tốc độ sử dụng CPU.

Điều này giúp tránh một Pod sử dụng toàn bộ tài nguyên của Node.

---

## Câu 86. Request khác Limit như thế nào?

### Trả lời

Request dùng để Scheduler lựa chọn Node.

Limit dùng để giới hạn tài nguyên khi Pod đang chạy.

Request ảnh hưởng đến Scheduling.

Limit ảnh hưởng đến Runtime.

---

## Câu 87. Liveness Probe là gì?

### Trả lời

Liveness Probe dùng để kiểm tra Container còn hoạt động hay không.

Nếu Probe thất bại nhiều lần Kubernetes sẽ tự động Restart Container.

Điều này giúp Self Healing hoạt động.

---

## Câu 88. Readiness Probe là gì?

### Trả lời

Readiness Probe kiểm tra ứng dụng đã sẵn sàng nhận Request hay chưa.

Nếu Readiness thất bại Service sẽ không gửi Traffic đến Pod đó.

Container vẫn chạy nhưng không nhận Request.

---

## Câu 89. Startup Probe là gì?

### Trả lời

Startup Probe dành cho ứng dụng khởi động chậm.

Trong thời gian Startup Probe chưa thành công Kubernetes sẽ không chạy Liveness Probe.

Điều này tránh việc ứng dụng bị Restart liên tục khi khởi động.

---

## Câu 90. DaemonSet là gì?

### Trả lời

DaemonSet đảm bảo mỗi Worker Node đều chạy một Pod.

Ví dụ:

- Fluent Bit
- Node Exporter
- Calico
- kube-proxy

Đây đều là các thành phần cần chạy trên tất cả Worker Node.

---

## Câu 91. StatefulSet là gì?

### Trả lời

StatefulSet dùng cho ứng dụng có trạng thái.

Ví dụ:

- MySQL
- PostgreSQL
- Kafka
- Elasticsearch

StatefulSet giữ nguyên:

- Tên Pod.
- Volume.
- Thứ tự khởi động.

Khác với Deployment.

---

## Câu 92. Job là gì?

### Trả lời

Job dùng để chạy tác vụ một lần.

Ví dụ:

- Backup Database.
- Import dữ liệu.
- Migration.

Sau khi hoàn thành Job sẽ kết thúc.

---

## Câu 93. CronJob là gì?

### Trả lời

CronJob là Job chạy theo lịch.

Ví dụ:

- Backup lúc 0h mỗi ngày.
- Xóa log mỗi tuần.
- Đồng bộ dữ liệu mỗi giờ.

CronJob sử dụng cú pháp giống Linux Cron.

---

## Câu 94. PVC là gì?

### Trả lời

PersistentVolumeClaim là yêu cầu sử dụng Storage.

Pod sẽ sử dụng PVC thay vì truy cập trực tiếp Storage.

Điều này giúp Pod có dữ liệu bền vững khi bị Restart.

---

## Câu 95. PV là gì?

### Trả lời

PersistentVolume là tài nguyên lưu trữ trong Kubernetes.

PV có thể được tạo từ:

- NFS
- EBS
- EFS
- Local Disk

PVC sẽ Bind tới PV phù hợp.

---

## Câu 96. PVC khác PV như thế nào?

### Trả lời

PV là Storage thật.

PVC là yêu cầu sử dụng Storage.

Pod chỉ làm việc với PVC.

Administrator quản lý PV.

---

## Câu 97. Node Selector là gì?

### Trả lời

Node Selector giúp Pod chỉ chạy trên Node có Label phù hợp.

Ví dụ:

label:

environment=production

Deployment chỉ chạy trên Node có Label này.

---

## Câu 98. Taint là gì?

### Trả lời

Taint đánh dấu Node để từ chối Pod.

Pod bình thường sẽ không được Schedule lên Node có Taint.

Taint thường dùng cho:

- Master Node.
- GPU Node.
- Database Node.

---

## Câu 99. Toleration là gì?

### Trả lời

Toleration cho phép Pod được phép chạy trên Node có Taint.

Taint và Toleration luôn đi cùng nhau.

Không có Toleration thì Pod sẽ không chạy trên Node đã bị Taint.

---

## Câu 100. Nếu một Worker Node bị chết thì Kubernetes xử lý như thế nào?

### Trả lời

Kubelet trên Node sẽ ngừng gửi Heartbeat.

Control Plane phát hiện Node NotReady.

ReplicaSet hoặc Deployment sẽ tạo Pod mới trên Worker Node khác còn đủ tài nguyên.

Người dùng gần như không bị ảnh hưởng nếu Cluster còn đủ Node.

Đó chính là cơ chế Self Healing của Kubernetes.


# Part 4 - Kubernetes Troubleshooting Interview (101-120)

---

## Câu 101. Pod đang ở trạng thái Pending. Em kiểm tra gì đầu tiên?

### Trả lời

Đầu tiên em sẽ chạy:

kubectl get pods

Sau đó:

kubectl describe pod <pod-name>

Em sẽ kiểm tra các nguyên nhân:

- Không đủ CPU
- Không đủ RAM
- Không có Node phù hợp
- NodeSelector sai
- Taint/Toleration
- PVC chưa bind
- Scheduler chưa schedule được

Pending nghĩa là Pod chưa được Scheduler gán vào Node.

---

## Câu 102. Pod bị CrashLoopBackOff là gì?

### Trả lời

CrashLoopBackOff nghĩa là Container khởi động xong rồi bị crash liên tục.

Kubernetes sẽ restart Container nhiều lần.

Sau mỗi lần restart sẽ tăng thời gian chờ.

Nguyên nhân thường gặp:

- Sai command
- Sai ENV
- Thiếu ConfigMap
- Thiếu Secret
- Không kết nối được Database
- Application bị lỗi

Em sẽ dùng:

kubectl logs

và

kubectl describe pod

để kiểm tra.

---

## Câu 103. Em debug CrashLoopBackOff như thế nào?

### Trả lời

Các bước em thực hiện:

Bước 1

kubectl describe pod

Xem Event.

Bước 2

kubectl logs pod-name

Đọc log.

Bước 3

Kiểm tra ConfigMap.

Bước 4

Kiểm tra Secret.

Bước 5

Kiểm tra Image.

Bước 6

Kiểm tra ENV.

Sau khi xác định nguyên nhân em sửa Deployment rồi rollout lại.

---

## Câu 104. ImagePullBackOff là gì?

### Trả lời

ImagePullBackOff xảy ra khi Kubernetes không kéo được Docker Image.

Nguyên nhân:

- Sai Image Name
- Sai Tag
- Harbor/DockerHub không hoạt động
- Thiếu ImagePullSecret
- Không có Internet

Em sẽ:

kubectl describe pod

để xem Event.

---

## Câu 105. ErrImagePull khác ImagePullBackOff như thế nào?

### Trả lời

ErrImagePull là lần kéo Image đầu tiên bị lỗi.

ImagePullBackOff là Kubernetes đã retry nhiều lần nhưng vẫn thất bại.

ImagePullBackOff là bước sau của ErrImagePull.

---

## Câu 106. ContainerCreating là gì?

### Trả lời

ContainerCreating nghĩa là Pod đã được Scheduler gán Node nhưng Container chưa khởi động.

Nguyên nhân:

- Đang pull Image
- Đang mount Volume
- CNI chưa sẵn sàng
- Secret chưa mount
- PVC chưa bind

---

## Câu 107. Pod bị OOMKilled là gì?

### Trả lời

OOMKilled nghĩa là Container dùng nhiều Memory hơn Limit.

Linux Kernel sẽ kill Container.

Em sẽ:

kubectl describe pod

để xem Exit Code.

Sau đó tăng Memory Limit hoặc tối ưu ứng dụng.

---

## Câu 108. Node NotReady là gì?

### Trả lời

Node NotReady nghĩa là Worker không gửi Heartbeat về Control Plane.

Nguyên nhân:

- kubelet chết
- Network lỗi
- CPU/RAM quá tải
- Disk đầy

Em sẽ SSH vào Node và kiểm tra:

systemctl status kubelet

journalctl -u kubelet

---

## Câu 109. kubectl logs dùng để làm gì?

### Trả lời

kubectl logs dùng để xem log của Container.

Ví dụ:

kubectl logs nginx-pod

Nếu Pod có nhiều Container:

kubectl logs pod -c container-name

Đây là lệnh em sử dụng nhiều nhất khi debug.

---

## Câu 110. kubectl describe dùng để làm gì?

### Trả lời

kubectl describe hiển thị chi tiết Resource.

Ví dụ:

- Event
- Volume
- Image
- Node
- Status
- Probe
- Conditions

Lệnh này rất quan trọng khi Troubleshooting.

---

## Câu 111. kubectl exec dùng để làm gì?

### Trả lời

kubectl exec cho phép truy cập vào Container đang chạy.

Ví dụ:

kubectl exec -it nginx -- bash

Em thường dùng để:

- ping
- curl
- cat file
- env
- ps
- netstat

---

## Câu 112. kubectl top dùng để làm gì?

### Trả lời

kubectl top hiển thị CPU và Memory.

Ví dụ:

kubectl top pod

kubectl top node

Lệnh này cần Metrics Server.

---

## Câu 113. Nếu Metrics Server chết thì sao?

### Trả lời

Nếu Metrics Server chết:

- kubectl top không hoạt động.
- HPA không lấy được CPU.
- HPA sẽ không scale Pod.

Ứng dụng vẫn chạy bình thường nhưng Autoscaling sẽ không hoạt động.

---

## Câu 114. Em kiểm tra Deployment lỗi như thế nào?

### Trả lời

Em sẽ:

kubectl get deployment

↓

kubectl describe deployment

↓

kubectl rollout status deployment

↓

kubectl get rs

↓

kubectl get pods

↓

kubectl logs

↓

kubectl describe pod

---

## Câu 115. rollout status dùng để làm gì?

### Trả lời

Lệnh:

kubectl rollout status deployment app

giúp kiểm tra Deployment đã cập nhật thành công hay chưa.

Nếu có Pod bị lỗi sẽ hiển thị nguyên nhân.

---

## Câu 116. rollout undo dùng để làm gì?

### Trả lời

Nếu phiên bản mới bị lỗi em sẽ:

kubectl rollout undo deployment app

Deployment sẽ quay lại ReplicaSet trước đó.

Đây là cách Rollback nhanh nhất.

---

## Câu 117. Nếu Pod Running nhưng website không truy cập được?

### Trả lời

Em sẽ kiểm tra theo thứ tự:

1. Pod Running chưa
2. Container Port
3. Service
4. Endpoints
5. Ingress
6. DNS
7. TLS
8. Security Group
9. NGINX Ingress Controller

---

## Câu 118. Em kiểm tra Service như thế nào?

### Trả lời

kubectl get svc

↓

kubectl describe svc

↓

kubectl get endpoints

Nếu Endpoints rỗng thì thường Label Selector không khớp với Pod.

---

## Câu 119. Endpoints là gì?

### Trả lời

Endpoints là danh sách IP của Pod phía sau Service.

Nếu Service không có Endpoint thì Service sẽ không thể chuyển tiếp Request.

---

## Câu 120. Nếu interviewer hỏi:
"Khi có sự cố em debug theo quy trình nào?"

### Trả lời

Em thường debug theo quy trình:

Bước 1:

kubectl get pods

Bước 2:

kubectl describe pod

Bước 3:

kubectl logs

Bước 4:

kubectl exec

Bước 5:

Kiểm tra Service

Bước 6:

Kiểm tra Endpoints

Bước 7:

Kiểm tra Ingress

Bước 8:

Kiểm tra Node

Bước 9:

Kiểm tra kubelet

Bước 10:

Kiểm tra Prometheus và Grafana để xác định thời điểm xảy ra lỗi.






