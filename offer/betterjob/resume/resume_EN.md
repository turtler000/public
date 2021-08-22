### PERSONAL INFORMATION

Name：Han Huanyu

Birthday：1997/7/9

Educational background ： Shandong University Software Engineering（2015-2019）

Telephone：17864154606

Wechat：h2535988943

QQ: 2535988943

E-mail：hanhuanyu@126.com

### WORK EXPERIENCE

#### Inspur Group-Software Development Engineer（2019/7 - present) 

##### 1.Government Information Department   Business mid-platform products

The business mid-platform product is a collection of more than a dozen unified components. The project uses a spring-cloud microservice architecture, uses maven for version control, devops for development, operation and maintenance, and k8s deployment. Use nacos for service discovery, service governance, and unified configuration management. At present, the product has been used in dozens of provincial and municipal government cloud projects. The tasks I am responsible for in this product are as follows:

1.Design and develop a unified authentication module, complete the sso system based on oauth2, and realize a highly available single sign-on system. The content of the work completed in this project: modify the jwt authentication mode to the redis-token authentication mode; optimize the authentication scheme of the traditional JSP page based on the session, optimize the performance problems under the redis-token multi-instance; design and authenticate customers based on spring-security The end filter chain completes the authentication through annotation and configuration.

2.Design and develop a unified user module to complete a user center system with stable and available users, roles, and authentication functions. The content of the work completed in this project: realize the separation and encryption of the front and back ends of user registration, realize the authentication system based on role and function tree; design and docking of the centralized user server based on openldap, responsible for the deployment of openldap, node attribute design and docking users A series of work of the system; Based on Rabbitmq's synchronous user solution, the function of real-time push of synchronous data through the message queue is realized.

3.Design and develop a spring-cloud gateway-based gateway to implement routing and forwarding and permission verification gateway projects, combining unified authentication and unified users to complete the routing and forwarding of front-end requests, matching service forwarding requests according to the request context; requesting unified tokens carried in front-end requests The user obtains permissions and performs authentication.

4.Complete the spring-boot transformation of multiple traditional components, modify the tomcat startup method to jar package startup, modify the hsf request method to feign request, handle the conflict between the old sca framework and the spring framework, refactor the code logic to reduce the coupling, and split the transaction Optimize performance, support existing business projects without intrusive docking of new version components.

##### 2.Yunzhou products

The Yunzhou project is an industrial Internet project of Inspur Cloud. As a developer, I participated in part of the design and development work of Yunzhou. The main tasks are as follows:

1.Participate in the research and development of the data center module, including the design and development of data factories, data lakes, API open platforms and other products for the opening of the capabilities of enterprise cloud and government cloud. I am responsible for the design and development of the API open platform, and realize the functions of developers such as registering, calling and managing the API.

2.The performance optimization and transformation of the old project. In a government cloud project, there were serious performance problems in the workflow module. Through analysis, it was found that there were two reasons: 1. A single transaction took too long, resulting in a timeout when rpc calls were made. 2. The amount of data is too large, and the amount of data in some single tables reaches 100 million, which seriously affects performance after the project is converted from Oracle to mysql. The first problem is achieved by refactoring the code, splitting long-consuming transactions, and optimizing the code algorithm. The second question is realized by sub-database sub-table, using shardingsphere to realize sub-database sub-table and read-write separation.

### SKILLS

Front end: vue、js、jquery、jsp

Middleware：tomcat、nginx、rabbitmq、nacos

Version control：git、maven

Database：mysql、oracle、redis

Framework：spring、springboot、springcloud

Operation：linux、docker、k8s

Design：uml、axure

Language: CET-6、IELTS-5.5

### ADDITIONAL INFORMATION

Currently working in Jinan, there are no hard and fast requirements for the place of work. The expected salary is discussed in the interview. If I am lucky enough to pass the resume screening, please call or WeChat to arrange a remote interview. If the on-site interview needs to be arranged until the weekend. Looking forward to the opportunity to work with you.

​																																											  2021/8/22



