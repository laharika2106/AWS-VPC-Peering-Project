# AWS VPC Peering – EWB Courses Student Project

**Student:** Rayudu Laharika  
**Training:** EWB Courses – AWS Architect Associate  
**Region:** Asia Pacific (Mumbai), `ap-south-1`  
**Status:** Partially demonstrated; outstanding workbook tests are listed below.

## Project objective
Create private communication between EC2 workloads in two non-overlapping VPCs using VPC peering, route tables and security groups. The EWB workbook additionally requires a public bastion host for SSH administration.

## Architecture (observed vs. workbook)

```text
VPC A: 10.10.0.0/16                VPC B: 10.20.0.0/16
EC2 VPC-A-Test-Server              EC2 VPC-B-Test-Server
10.10.147.115 (observed)           10.20.2.43 (observed)
       |                                  |
       +------ VPC peering (pcx) --------+
```

The diagram shows the observed EC2-to-EC2 test path, not the full required bastion topology. The workbook's sample subnets/IPs differ from the actual lab addresses; record actual values in the final submission.

## Evidence supported by screenshots
- Two running `t3.micro` EC2 instances, one for each VPC.
- VPC A and VPC B route table configuration with peering targets.
- VPC A EC2 session ping to `10.20.2.43`: **4 packets received, 0% packet loss**.
- `ssm` and `ssmmessages` endpoint DNS resolves to private addresses; HTTP 400 from an unsigned `curl -I` request is **not** evidence of endpoint failure.
- IAM instance role with SSM and CloudWatch managed policies.

## Required evidence still to verify
- Public bastion host and SSH ProxyJump to App-B (not shown in supplied screenshots).
- Baseline ping failure before peering (not shown).
- Reverse ping from VPC B to VPC A (not shown).
- Controlled ICMP-rule removal failure and recovery (not shown).
- Final cleanup and cost review (not shown).

## Screenshots
See [`screenshots/`](screenshots/) for uploaded AWS console evidence. These files are retained with neutral filenames; identify each one and remove irrelevant images before final submission.

## Routing explanation
VPC A needs a route for `10.20.0.0/16` toward the peering connection. VPC B needs a return route for `10.10.0.0/16` toward the same connection. Security groups must permit the relevant traffic. An active peering connection alone does not ensure connectivity.

## Security
The lab uses private IP communication. Do not upload `.pem` files, AWS keys, credentials, access tokens, or secrets. Restrict inbound rules to the minimum required sources and protocols.

## Reference
EWB Courses, *AWS VPC Peering – Student Project Workbook*, October 2026 (14 pages). This repository documents the student's implementation and is not an official AWS publication.
