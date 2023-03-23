---
layout: post
title:  "Microservices"
date:   2022-11-23 10:33:47 +0700
categories: code-academy
---

## Distributed computing

A distributed system is a collection of computer programs that utilize computational resources across multiple separate computation nodes to achieve a shared goal. Distributed systems help to improve system reliability and performance and allow easy scaling.

Well-known distributed systems are databases that rely on multiple nodes to provide store and query features. These nodes typically represent separate physical hardware devices but can also define different software processes or recursive encapsulated systems. Distributed systems aim to remove bottlenecks or central points of failure from a system.


Distributed computing systems have the following characteristics:

1. Resource sharing
> A distributed system can share hardware and software or data
2. Simultaneous processing
> Multiple machines can process the same function simultaneously
3. Scalability
> The computing and processing capacity can scale up as needed when extended to additional machines
4. Error detection
> Failures can be more easily detected
5. Transparency
> A node can access and communicate with other nodes in the system


**The difference between a centralized system and a distributed system**

![differences between centralized vs distributed system](https://wac-cdn.atlassian.com/dam/jcr:56587a16-e96f-49f2-b302-74c3e90d1199/Distributed-Architecture-article.png?cdnVersion=697)

A centralized computing system is where all computing is performed on a single machine in one location. The primary difference between centralized and distributed systems is the way of communication. With the centralized system, all requests are handled by one node, which creates a single point of failure, opposites with the distributed system, there are multiple nodes responsible for handling requests.

Database replication can be considered a form of a distributed system - Database replication is a process of copying data from one server to another to improve performance or increase availability. 

!["MariaDB replication"](https://mariadb.com/sites/default/files/pictures/Images/dbreplication173.png)

It helps to remove the single point of failure and provides the ability to scale. The dataset is persisted on multiple servers, when a server goes down there is always another active server available and the dataset is safe. Each server has a limited capacity for handling requests due to the hardware, to handle more requests upgrade the hardware (vertical scale) but it is costly, adding servers is a reasonable approach as it not only increases the capacity but also prevents a single point of failure.

The database is a single system for managing datasets, in the bigger picture database itself cannot provide a business. A banking system relies on 



Service-oriented architecture (SOA) is a predecessor of microservices. The main difference between SOA and microservices is node scope – the scope of microservice nodes exists at the feature level. In microservices, a node encapsulates the business logic to handle a specific feature set, such as payment processing. Microservices contain multiple disparate business logic nodes that interface with independent database nodes. Comparatively, SOA nodes encapsulate an entire application or enterprise division. The service boundary for SOA nodes typically includes an entire database system within the node.


## Microservices

Microservices - also known as microservice architecture - is an architectural style that structures an application as a collection of services that are
 
* Independently deployable
* Loosely coupled
* Organized around business capabilities
* Owned by a small team
* Highly maintainable and testable

The microservice architecture enables the rapid, frequent and reliable delivery of large, complex applications. It also enables an organization to evolve its technology stack.

# E-commerce startup

One man starts his own business by selling books online. Steps to setup



Anh ta sử dụng hệ thống Magento để bắt đầu hệ thống

Magento là phần mềm nguồn mở với nhiều tính năng gồm website bán hàng, quản lý sản phẩm/đơn hàng/khách hàng.

Website được sử dụng bởi số người mua hàng + 1

Việc buôn bán diễn ra thuận lợi và ngày càng nhiều đơn hàng hơn và vấn đề bắt đầu nảy sinh. 

Trung bình anh ta tốn khoảng 6 phút để đóng gói 1 đơn hàng, vị chi tối đa mỗi ngày chỉ có thể xử lý 480 đơn hàng / 8 tiếng / ngày làm việc. Số lượng đơn tăng lên vượt, nếu chỉ dành thời gian cho việc đóng gói hàng thì sẽ không còn thời gian làm việc khác như nhập hàng về bán, cập nhập nội dung website.

Nếu nội dung website không được cập nhập, khách hàng đặt hàng không được ảnh hưởng trực tiếp đến việc kinh doanh. Anh ta thuê thêm người để xử lý đơn hàng và anh ta tập trung vào việc nhập hàng và quản lý nội dung website.

Website được sử dụng bởi số người mua hàng + 2

Việc thuê thêm người mang lại hiệu quả, website luôn cập nhập nội dung mới nhất, đơn hàng được xử lý nhanh, càng có thêm nhiều đơn hàng.

Số lượng đơn hàng tiếp tục tăng lên, chính vì thế cần nhập thêm nhiều hàng hơn để đáp ứng số lượng đơn hàng, kho ban đầu không đủ chứa nên cần mở rộng kho. Không có đủ hàng sẽ dấn đến việc xử lý đơn hàng trễ giảm trải nghiệm mua sắm ảnh hưởng đến việc kinh doanh. Anh ta mướn thêm 1 người chuyên quản lý kho để dành thời gian tập trung cho việc quản lý nội dung website.

Khách hàng hài lòng với dịch vụ mua sắm ở đây, tiếp tục mua nhiều hơn và giới thiệu cho bạn bè.

Website được sử dụng bởi số người mua hàng + 3

Điều gì sẽ xảy ra nếu số lượng đơn hàng bắt đầu tăng dần lên? Nếu hệ thống đạt ngưỡng 480,000 đơn hàng thì công ty sẽ cần đến 1000 người đóng gói và 1000 vận hành kho.

Trong tình huống đó hệ thống Magento ban đầu phục vụ số khách mua hàng + 2000 nhân viên nội bộ, chưa kể lượt truy cập website đã tăng x1000 lần. Với thương mại điện tử website mua hàng phải luôn trong trạng thái sẵn sàng, mỗi giây đều được tính bằng tiền. Nếu hệ thống Magento sập thì ảnh hưởng toàn bộ cả khách hàng lẫn vận hành.

Anh ta xây dựng team dev để thiết lập phần mềm quản lý Odoo (Open ERP) kho cũng như tích hợp hệ thống Magento. ERP quản lý phần nhân sự cũng như quản lý phần mua hàng và lấy hàng. Việc tách phần quản lý kho và lấy hàng ra khỏi Magento giảm thiểu rủi ro.

**Independently deployable & Loosely coupled**
Nếu ERP có sự cố không ảnh hưởng đến việc mua sắm của khách hàng và ngược lại.

**Highly maintainable and testable**
Khi sự cố xảy ra ở hệ thống nào, team phụ trách quản lý riêng cho từng hệ thống.

**Organized around business capabilities**
Tại thời điểm ban đầu, vốn bỏ ra cần được đầu tư cho
* Nhập hàng về bán
* Website Magento (1 server)
* Chi phí nhân công

Việc phân bổ vốn rất quan trọng, bất kỳ sự bất cân đối nào cũng có thể gây ảnh hưởng đến việc kinh doanh. 

Nếu như đầu tư quá nhiều chi phí xây dựng cả 2 hệ thống Magento & ERP ngay từ đầu, không còn tiền đầu tư cho chi phí marketing, nhập hàng, nhân công.
ERP nếu xây dựng quá sớm không tối ưu hết được chi phí đầu tư, không sinh ra lợi nhuận và chôn vốn.

**Owned by a small team**
Mỗi hệ thống phục vụ đối tượng khác nhau, team ở đây có thể hiểu là các phòng ban/bộ phận. Magento phục vụ chủ yếu cho khách hàng và ERP phục vụ cho quản lý nội bộ. Công ty bao gồm cả 2 phần này nếu thiếu 1 trong 2 sẽ không còn là mô hình kinh doanh nữa










1. Buy bulks of books and store them in his garage
![](https://media1.citybeat.com/citybeat/imager/u/original/12087967/garage_1.5ed671f81e3c6.png)

2. Setup a website for selling books

!["Listing product"](https://bashooka.com/wp-content/uploads/2016/10/online-book-store-websites-7.jpg)

!["Order management system"](https://fabric.inc/wp-content/uploads/hubspot/magento-2-order-management-1.png)


The number of books increase rapidly 

View orders with NoSQL 