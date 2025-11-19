# 🚀 AWS Auto Scaling & Load Balancer Project  
A fully automated, scalable, and self-healing web infrastructure using EC2, Load Balancer, ASG, Route 53, CloudWatch, and SNS.

---

## 📘 Overview  
This project implements a **highly available**, **fault-tolerant**, **auto-scaling** web service hosted on AWS.  
The website shows dynamic output:  
✔ Which EC2 instance is responding  
✔ Auto scaling launches new EC2 automatically  
✔ SNS email sent on every scaling event  
✔ Route 53 domain routing  
✔ Classic Load Balancer distributes traffic  

---

# 🏗 Architecture

```text
Users → Route 53 (Domain)
          ↓
      Internet Gateway
          ↓
  Classic Load Balancer
          ↓
  ┌─────────────────────────┐
  │  Auto Scaling Group      │
  │   ┌───────────────┐     │
  │   │ EC2 Instance 1 │     │
  │   └───────────────┘     │
  │   ┌───────────────┐     │
  │   │ EC2 Instance 2 │     │
  │   └───────────────┘     │
  └─────────────────────────┘
          ↓
     CloudWatch → SNS Alerts

---

# 🛠 Tech Stack

| AWS Service | Use |
|------------|------|
| EC2 | Runs Apache web server |
| Auto Scaling Group | Auto-heals & expands instances |
| Classic Load Balancer | Distributes traffic |
| Route 53 | Domain mapping |
| CloudWatch Alarms | Detects high CPU |
| SNS | Sends email notifications |
| S3 (optional) | Stores logs |

---

# 📄 User Data Script (Apache install)

Location: `user-data/install_apache.sh`

```bash
#!/bin/bash
yum update -y
yum install -y httpd

INSTANCE_ID=$(curl -s http://169.254.169.254/latest/meta-data/local-hostname)

cat <<EOF > /var/www/html/index.html
<html>
<h1>My auto web page is coming from</h1>
<p>$INSTANCE_ID</p>
</html>
EOF

systemctl enable httpd
systemctl start httpd
```

---

# 🌐 Domain Setup (Route 53)

```
A Record:
www.yourdomain.com → load-balancer-dns.amazonaws.com
```

---

# 📦 Auto Scaling Details

### **Scaling Policy (Target Tracking)**  
- CPU Target: **30%**
- If CPU > 30% → Scale Out (+1 instance)  
- If CPU < 30% → Scale In (–1 instance)

---

# 📨 SNS Notification Example

File stored:  
`screenshots/scaling-email.png`

---

# 🖼 Sample Web Output

```
My auto web page is coming from
ip-10-0-1-35.ap-south-1.compute.internal
```

```
My auto web page is coming from
ip-10-0-2-94.ap-south-1.compute.internal
```

---

# 🧪 Testing Auto Scaling

```bash
sudo amazon-linux-extras install epel -y
sudo yum install stress -y
stress --cpu 2 --timeout 120
```
---

# 🎯 Achievements

- Multi-AZ auto scaling
- Load balanced traffic
- Fully automated EC2 provisioning
- Real-time SNS alerts
- Dynamic webpage showing instance origin
- Clean GitHub documentation

---

# 🧱 Future Enhancements

- HTTPS with ACM
- ALB instead of CLB
- CloudFront global edge caching
- Complete Terraform automation

---

# 👨‍💻 Author  
**Durgesh Dhore**  
Cloud | DevOps | Python | Linux  
