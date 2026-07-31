---
title: "Blog 1"
date: 2026-07-31
weight: 1
chapter: false
pre: " <b> 3.1. </b> "
---

# Can Increasing AWS Lambda Memory Reduce Costs? Why?

When configuring AWS Lambda, many developers assume that choosing the lowest memory setting is always the best way to reduce costs. However, AWS Lambda pricing depends not only on the allocated memory but also on the execution duration. Increasing memory also increases the amount of CPU and other computing resources available to the function, which can significantly reduce execution time.

This article explains the relationship between **memory allocation, CPU resources, execution time, and Lambda pricing (GB-seconds)**. It demonstrates why a function configured with higher memory may complete its work much faster and, in some cases, cost less than the same function running with a lower memory allocation.

## Main Topics

* How AWS Lambda allocates CPU based on configured memory.
* Understanding Lambda pricing using the GB-second model.
* Why increasing memory can reduce execution time and sometimes lower total cost.
* Workloads that benefit most from higher memory (CPU-bound, memory-bound, and data processing workloads).
* Cases where increasing memory provides little or no benefit.
* Best practices for selecting the optimal memory configuration.
* Using **AWS Lambda Power Tuning** to benchmark different memory settings.
* Using **AWS Compute Optimizer** to receive memory recommendations based on production workloads.
* A practical example of optimizing a Lambda function used for processing AQI datasets.

## Key Takeaways

Through this article, readers can better understand that the optimal memory configuration is not necessarily the smallest one. Instead, it should be selected based on performance testing and workload characteristics to achieve the best balance between **performance** and **cost**. AWS recommends benchmarking different memory configurations because additional CPU resources provided with higher memory settings can significantly improve performance and sometimes even reduce overall compute costs. :contentReference[oaicite:0]{index=0}

## Blog Link

Facebook (AWS Study Group FCJ):  
https://www.facebook.com/groups/660548818043427/?multi_permalinks=2228234364608190&ref=share

## References

* AWS Lambda – Configure Function Memory  
  https://docs.aws.amazon.com/lambda/latest/dg/configuration-memory.html

* AWS Lambda Best Practices  
  https://docs.aws.amazon.com/lambda/latest/dg/best-practices.html

* AWS Lambda Pricing  
  https://aws.amazon.com/lambda/pricing/

* AWS Compute Optimizer – AWS Lambda Recommendations  
  https://docs.aws.amazon.com/compute-optimizer/latest/ug/view-lambda-recommendations.html

* AWS Lambda Power Tuning (GitHub)  
  https://github.com/alexcasalboni/aws-lambda-power-tuning

* Building Well-Architected Serverless Applications – Optimizing Application Costs  
  https://aws.amazon.com/blogs/compute/building-well-architected-serverless-applications-optimizing-application-costs/

* Troubleshoot AWS Lambda Functions  
  https://docs.aws.amazon.com/lambda/latest/dg/troubleshooting-execution.html

* AWS Lambda Runtime Environment  
  https://docs.aws.amazon.com/lambda/latest/dg/lambda-runtime-environment.html