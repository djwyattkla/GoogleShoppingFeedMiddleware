# 🛍️ Google Shopping Feed Generator (AWS Fargate)

![build](https://img.shields.io/badge/build-passing-brightgreen)
![aws](https://img.shields.io/badge/AWS-Fargate-orange)
![automation](https://img.shields.io/badge/Automated-Scheduled-green)
![language](https://img.shields.io/badge/Python-3.10-blue)
![status](https://img.shields.io/badge/Status-Production--Ready-brightgreen)

> A fully serverless pipeline that scrapes product data, generates a Google Shopping XML feed, and uploads it to Amazon S3 — all triggered by EventBridge using ECS Fargate.

---

# AWS Fargate Google Shopping Feed Pipeline

## ✅ What Was Built

A fully automated Google Shopping feed generation pipeline using:

- **AWS ECS Fargate** to run a Python scraper in a Docker container
- **Amazon S3** for storing input (`products.csv`) and output (`google_shopping_feed.xml`)
- **Amazon EventBridge** to schedule the task monthly
- **CloudWatch Logs** for full visibility into scraping, XML generation, and upload
- **IAM Roles** for secure access via `ecsTaskExecutionRole`

---

## 🛠 How It Works

1. You upload a new `products.csv` file to `s3://googleshoppingfeed/products/`
2. EventBridge triggers an ECS Fargate task monthly
3. The task:
   - Downloads the CSV
   - Scrapes product info
   - Generates a Google Shopping XML feed
   - Uploads it to `s3://googleshoppingfeed/feeds/google_shopping_feed.xml`
4. Logs are written to CloudWatch: `/ecs/google-feed-task`

---

## 🔁 Manual Trigger (CLI)

```bash
aws ecs run-task \
  --cluster google-feed-cluster-2 \
  --launch-type FARGATE \
  --network-configuration "awsvpcConfiguration={subnets=[subnet-08a1e77fb58679c1f,subnet-0d3f3e65ba893bdb4],securityGroups=[sg-051c83885579194d6],assignPublicIp=ENABLED}" \
  --task-definition google-feed-task
```

---

## 📁 File Structure

- `products.csv` → list of product URLs (S3 input)
- `google_shopping_feed.xml` → generated XML file for Google Shopping (S3 output)

---

## ✅ Status

**Production-ready** — ECS, S3, EventBridge, and CloudWatch fully integrated.
