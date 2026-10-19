# Windows Server & Active Directory Home Lab (AWS)

A hands-on lab where I build, configure, deliberately break, and troubleshoot a Windows Server 2022 Active Directory environment in AWS. Each phase is documented with the commands I ran, why I ran them, and screenshots proving the result.

## Architecture

| Host | Private IP | Role | Status |
|---|---|---|---|
| DC01 | 10.10.1.10 | Forest root domain controller, DNS, PDC emulator | Built |
| SRV01 | 10.10.1.20 | Member server, file shares | Planned |
| CLIENT01 | 10.10.1.50 | Domain-joined client | Planned |
| DC02 | 10.10.1.11 | Second DC for replication and FSMO practice | Planned |

- **Domain:** `corp.jhlab.internal` (NetBIOS `JHLAB`)
- **Network:** AWS VPC `10.10.0.0/16`, single subnet `10.10.1.0/24`, RDP restricted to one /32 source address
- **Why AWS:** Windows Server has no supported ARM64 build for Apple Silicon, and a single VPC keeps all lab traffic on private IPs without a VPN

## Progress

- [x] Phase 0: VPC, subnet, security group, key pair, cost alert
- [x] [Phase 1: Build DC01 and the forest](docs/phase1-build-dc01.md)
- [ ] Phase 2: Join member server and client, OUs, users, groups, file shares
- [ ] Phase 3: Group Policy and delegation
- [ ] Phase 4: Second DC, replication, FSMO roles
- [ ] Phase 5: Password policies, auditing, LAPS
- [ ] Break/fix scenarios: DNS, GPO, replication, accounts and security

## Highlights so far

- Converted an AWS-assigned address to a static IP in a single command so the remote session could not drop mid-change
- Promoted DC01 with `Install-ADDSForest` and configured DNS forwarding, a reverse lookup zone, and Amazon Time Sync as the domain time source
- Investigated a failed `dcdiag` DFSREvent test by reading the DFS Replication log and confirmed it was boot-time noise, not a replication fault

## Tools

Windows Server 2022, Active Directory Domain Services, DNS, PowerShell, AWS EC2 and VPC, Git
