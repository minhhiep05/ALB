graph TB
    subgraph Internet["🌐 Internet / Users & Admin"]
        User(("👤 User / Client"))
        Admin(("👨‍💻 Admin / DevOps"))
    end

    subgraph AWS["☁️ AWS Cloud - VPC (10.0.0.0/16)"]
        IGW["🌐 Internet Gateway (IGW)"]

        subgraph PublicSubnet["🔓 Public Subnet (10.0.1.0/24)"]
            ALB["⚖️ Application Load Balancer (ALB)"]
            Bastion["🛡️ Bastion Host (EC2)"]
            Monitor["📊 Monitoring Server (EC2)<br/>(Prometheus + Grafana + Loki)"]
        end

        subgraph PrivateSubnet["🔒 Private Subnet (10.0.2.0/24)"]
            NAT["🔄 NAT Gateway"]
            
            subgraph ASG["📦 Auto Scaling Group (App Cluster)"]
                App1["🚀 Node.js App 1 (Docker + Promtail)"]
                App2["🚀 Node.js App 2 (Docker + Promtail)"]
            end
            
            Jenkins["⚙️ Jenkins Server (CI/CD)"]
        end
    end

    %% Connections
    Admin -->|1. SSH Tunnel / Port 22| Bastion
    Bastion -.->|SSH Local Access| Jenkins
    Bastion -.->|SSH Local Access| App1
    
    User -->|2. HTTP/HTTPS Port 80/443| ALB
    ALB -->|3. Forward Traffic| App1
    ALB -->|4. Forward Traffic| App2

    App1 -->|5. Outbound Update/Pull Image| NAT
    App2 -->|Outbound Traffic| NAT
    Jenkins -->|Outbound Traffic| NAT
    NAT --> IGW

    Admin -->|6. View Metrics & Logs| Monitor
    App1 -.->|7. Push Logs & Metrics| Monitor
    App2 -.->|Push Logs & Metrics| Monitor
    Jenkins -.->|Deploy Trigger| ASG
