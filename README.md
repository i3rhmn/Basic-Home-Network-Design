# Basic Home Network Design

# Project Overview

This project presents the design of a secure, reliable, scalable, and manageable two-story home network.

The original network consisted of six computers distributed across Rooms 1–6, two wireless routers, two switches, and one core router. The original design had several limitations, including a lack of network segmentation, single points of failure, inconsistent wireless coverage, and difficulties when adding new devices.

The proposed network design addresses these limitations through VLAN segmentation, dynamic routing, link aggregation, wireless access points, ACL-based security, and SNMP monitoring.

The project combines practical network design with security, reliability, scalability, monitoring, risk management, and project management concepts.

# Problem Statement

The original network had several technical and operational limitations:

- A failure of the core router or one of the switches could cause a complete network shutdown.
- The network had no proper segmentation between devices.
- A malware infection on one computer could potentially affect the wider network.
- Wireless coverage was inconsistent between the two floors.
- The wireless routers were separately controlled, which could create dead zones and mixed signals.
- Adding new devices required additional manual configuration.
- The overall network was difficult to maintain and had limited scalability.

# Project Objectives

The project was designed around five main objectives.

## Redundancy

Reduce single points of failure by introducing redundant uplinks and link aggregation.

LACP is used to combine multiple physical links into logical connections, improving resilience against individual link failures.

## Segmentation

Separate network traffic using VLANs.

Each floor is assigned to a different VLAN, providing logical separation between network segments.

## Managed Wireless

Provide wireless coverage across both floors using two wireless access points.

The design aims to provide more consistent wireless coverage and smoother connectivity between floors.

## Scalability

Create a network structure that allows additional wired and wireless devices to be added with minimal reconfiguration.

## Monitoring and Security

Improve network visibility through SNMP monitoring and control traffic using ACL-based filtering on the core router.

# Project Scope

## Included

The project includes:

- Six wired PCs located in Rooms 1–6
- Two Gigabit switches
- Two Home Router-PT-AC wireless devices
- One core router
- Wired network connectivity
- Wireless connectivity
- VLAN segmentation
- Inter-VLAN routing
- OSPF
- LACP
- ACL-based security
- SNMP monitoring

## Excluded

The following items were outside the project scope:

- IoT or smart-home devices such as printers, cameras, and sensors
- Guest Wi-Fi SSIDs
- Captive portals
- WAN redundancy
- Secondary ISP connections
- Advanced QoS beyond basic voice and data prioritization

The project focuses on the physical and logical network structure required to create a robust, segmented, and manageable home network.

# Network Design

The proposed network is designed for a two-story house.

The network contains:

- First-floor network
- Second-floor network
- Core router
- First-floor switch
- Second-floor switch
- Two wireless access points
- Six PCs
- ISP connection

The first floor and second floor are separated using different VLANs.

This provides logical separation while allowing routing between the networks when required.

# Network Topology

The network contains six PCs distributed across Rooms 1–6.

## First Floor

The first floor contains:

- Room 1
- Room 2
- Room 3
- First-floor switch
- First-floor wireless access point

## Second Floor

The second floor contains:

- Room 4
- Room 5
- Room 6
- Second-floor switch
- Second-floor wireless access point

The switches connect the floor devices to the core router.

The core router provides routing between the VLANs and connectivity toward the ISP.

# VLAN Design

VLANs are used to logically separate the network.

The project uses two primary VLANs:

| VLAN | Purpose | Network |
|---|---|---|
| VLAN 10 | First Floor | 192.168.10.0/24 |
| VLAN 20 | Second Floor | 192.168.20.0/24 |

VLAN segmentation creates separate broadcast domains.

This improves:

- Network organization
- Traffic control
- Security
- Scalability
- Troubleshooting

# VLAN 10 - First Floor

The first-floor network uses:

```text
Network: 192.168.10.0/24
Gateway: 192.168.10.1
DHCP Range: 192.168.10.100 - 192.168.10.200

The first-floor PCs and wireless network are associated with VLAN 10.

VLAN 20 - Second Floor

The second-floor network uses:

Network: 192.168.20.0/24
Gateway: 192.168.20.1
DHCP Range: 192.168.20.100 - 192.168.20.200

The second-floor PCs and wireless network are associated with VLAN 20.

Inter-VLAN Routing

Inter-VLAN routing is provided by the core router using a Router-on-a-Stick design.

The router uses separate subinterfaces for each VLAN.

VLAN 10 Subinterface
interface Gig0/1.10
 encapsulation dot1Q 10
 ip address 192.168.10.1 255.255.255.0
VLAN 20 Subinterface
interface Gig0/1.20
 encapsulation dot1Q 20
 ip address 192.168.20.1 255.255.255.0

The router acts as the default gateway for both VLANs.

Traffic between VLAN 10 and VLAN 20 passes through the core router.

This also provides a central location where ACL rules can be applied.

OSPF Dynamic Routing

OSPF is used as the dynamic routing protocol.

The project uses:

OSPF Process: 1
Area: 0

The configured networks include:

network 192.168.10.0 0.0.0.255 area 0
network 192.168.20.0 0.0.0.255 area 0

OSPF provides dynamic route management instead of relying entirely on manually configured static routes.

The design also allows additional networks to be incorporated into the routing architecture in the future.

Why OSPF?

OSPF was selected because it supports dynamic route management and future network growth.

The main benefits considered in the design are:

Dynamic routing
Automatic route updates
Support for network expansion
Reduced dependency on static routes
Better management of multiple networks
Link Aggregation

LACP is used to provide link aggregation.

Link aggregation combines multiple physical connections into a logical connection.

The design uses LACP to improve:

Link redundancy
Network availability
Aggregate bandwidth
Resilience against cable failures

If one physical connection fails, the remaining connection within the aggregated link can continue providing connectivity.

Network Redundancy

Reducing single points of failure is one of the main goals of the project.

The design uses redundant uplinks and LACP to improve resilience.

The redundancy approach helps protect against individual link failures and improves overall network availability.

Wireless Network

Two Home Router-PT-AC devices are used to provide wireless coverage.

Each wireless device serves a different floor.

First Floor Wireless
SSID: Home_Floor1
VLAN: 10
Second Floor Wireless
SSID: Home_Floor2
VLAN: 20

The wireless devices operate in bridge mode and connect back to their respective switches.

Wireless Design

The wireless design aims to provide better coverage across the two floors.

Each floor has its own wireless access point.

This reduces dependence on a single wireless device and helps provide coverage in different areas of the house.

The design also focuses on smoother movement between wireless coverage areas.

Wireless Security

Wireless security is part of the proposed network design.

The project identifies WPA2-AES as the security mechanism for the wireless networks.

This helps protect wireless communications from unauthorized access.

ACL Security

Access Control Lists are implemented on the core router.

ACLs are used to control traffic between network segments.

The purpose of the ACL design is to:

Restrict unnecessary traffic
Control east-west traffic
Allow required management and routing traffic
Reduce the lateral attack surface
Improve network security

By placing traffic filtering at the core router, communication between VLANs can be controlled before traffic reaches another network segment.

Network Segmentation and Security

VLANs and ACLs work together to improve network security.

VLANs provide logical separation.

ACLs provide traffic filtering.

This combination can help limit the potential impact of a compromised device by controlling communication between network segments.

SNMP Monitoring

SNMP version 2c is used for network monitoring.

SNMP provides visibility into the condition and operation of network devices and interfaces.

The project uses SNMP-based health checking to support:

Network monitoring
Interface monitoring
Device visibility
Fault detection
Proactive troubleshooting
Network Visibility

Monitoring provides administrators with greater visibility into the network.

Instead of depending only on manual troubleshooting, SNMP can provide information about network device and interface status.

This supports faster identification of potential problems.

Scalability

The proposed network is designed to support future growth.

A structured VLAN and IP addressing scheme makes it easier to add:

New PCs
Wireless devices
Additional network segments
Future services

The design reduces the amount of reconfiguration required when expanding the network.

Reliability

The redesigned network improves reliability compared with the original flat network.

The main reliability features are:

LACP
Redundant uplinks
VLAN segmentation
Dynamic routing
Multiple wireless access points
Network monitoring
ACL-based traffic control
Innovation

The project introduces several improvements to a traditional home network.

Dynamic VLAN Segmentation

The design uses VLAN segmentation to separate different network areas.

The project also describes dynamic VLAN assignment using 802.1X/EAP concepts and managed switch trunks.

This can help isolate broadcast traffic and limit the potential spread of compromised or incorrectly configured devices.

Lightweight Wireless Management

Two wireless devices are used to provide coverage across the two floors.

The design focuses on consistent wireless coverage and coordinated wireless connectivity.

Link Aggregation and Resiliency

LACP is used to combine uplinks and improve resilience.

This provides protection against individual physical link failures while also increasing aggregate link capacity.

Built-in Monitoring and ACL Firewall

SNMP provides monitoring and visibility.

ACLs on the core router provide Layer 3 traffic filtering between VLANs.

Together, these technologies provide both monitoring and security controls.

Project Impact

The project identifies several expected improvements.

Security

VLAN segmentation and ACL filtering help restrict unnecessary communication between network segments.

This can reduce the potential spread of threats across the network.

Reliability

Redundant uplinks and LACP reduce the dependency on a single physical connection.

Scalability

The structured network design makes it easier to add new devices and expand the network.

User Experience

Improved wireless coverage is intended to provide more consistent connectivity across both floors.

Project Management

The project included project management activities to organize the work and track progress.

These activities included:

RACI Matrix
Gantt Chart
Work Breakdown Structure
Critical Path
Project Milestones
Project Risks
Risk Mitigation
Lessons Learned
Budget Planning
RACI Matrix

A RACI matrix was created to clarify team responsibilities.

RACI represents:

Responsible
Accountable
Consulted
Informed

The matrix helped distribute responsibilities across different project activities and reduce confusion between team members.

Project Plan

The project plan divided the work into several stages.

The main activities included:

Kick-off and Scope Sign-off
Requirements and High-Level Design
VLAN and OSPF Detailed Design
Device Configuration and LACP Setup
ACL and SNMP Monitoring Implementation
Integration Testing and Validation
Report Drafting and Internal Review
Demonstration Preparation and Rehearsals
Final Review and Submission
Work Breakdown Structure

The Work Breakdown Structure was used to divide the project into manageable activities.

It provided information about:

Activities
Dates
Owners
Status
Targets

This helped the team track project progress from planning through final submission.

Critical Path

A critical path was created to identify the dependencies between project activities.

The project workflow included:

Start
  ↓
Kick-off & Scope Sign-off
  ↓
Requirements & High-Level Design
  ↓
Device Configuration & LACP
  ↓
ACL & SNMP Implementation
  ↓
Integration Testing
  ↓
Report Draft & Review
  ↓
Demo Preparation & Rehearsals
  ↓
Final Review & Submission
  ↓
Finish
Project Milestones

The project included milestones covering:

Project initiation
Requirements
Network design
Device configuration
Security implementation
Monitoring implementation
Testing
Documentation
Demonstration
Final submission

The project documentation records these activities as completed milestones.

Project Risks

Several risks were identified during the project.

Risk	Mitigation
Single-Link / Device Failure	Use LACP and redundant uplinks
OSPF Misconfiguration	Validate OSPF configuration in Packet Tracer
VLAN Assignment Errors	Use standardized VLAN configuration and port mapping
Wireless Coverage Problems	Adjust AP placement and antenna orientation
Unauthorized Access	Use WPA2-AES security
ACL Misconfiguration	Test ACL rules in an isolated environment
SNMP Exposure	Consider migration to SNMPv3 with authentication and encryption
Firmware Vulnerabilities	Establish a regular firmware-update schedule
Risk Management

Risk management was incorporated into the project to identify and address technical issues.

Examples of identified issues included:

Wireless dead zones
ACL misconfiguration
VLAN assignment errors
OSPF configuration issues
SNMP security concerns
Firmware vulnerabilities

Mitigation actions were defined for the identified risks.

Lessons Learned
Teamwork and Role Clarity

The RACI matrix helped clarify responsibilities between team members.

Peer review also helped identify errors in network configurations and report sections before finalization.

Communication

Regular meeting minutes helped track:

Progress
Action items
Blockers
Testing activities

Group communication also helped resolve issues and coordinate testing.

Planning and Adaptability

Gantt charts and critical-path planning helped the team understand task dependencies.

The project was divided into planning, execution, and closure stages.

Technical Collaboration

Testing configurations in Cisco Packet Tracer before implementation helped identify VLAN and OSPF issues earlier.

Shared documentation was also used for:

Network configurations
IP addressing plans
ACL scripts
Network information
Risk Management and Quality Control

The project used a risk log to identify technical issues.

Wireless dead zones and ACL configuration problems were identified as issues that needed to be addressed before final handover.

Continuous Learning

The project provided practical experience with:

802.1Q trunking
LACP
SNMP
VLANs
OSPF
ACLs
Network monitoring
Network troubleshooting

The project also highlighted the importance of using checklists and standardized port-mapping tables to reduce configuration errors.

Budget

The project included a budget covering:

Core router and licensing
Managed switches
Wireless access points
Cabling and accessories
Planning and scope definition
Detailed network design
VLAN and OSPF configuration
ACL and SNMP implementation
Integration testing
Documentation
Demonstration preparation
Contingency

The documented overall project budget was:

BHD 2,609.84
Technologies Used
Cisco Packet Tracer
VLAN
802.1Q
Router-on-a-Stick
OSPF
LACP
ACL
SNMP v2c
DHCP
Wireless Networking
Bridge Mode
IPv4
Subnetting
Network Monitoring
Network Security
Network Redundancy
Skills Demonstrated
Network Design
Network Architecture
IPv4 Addressing
Subnetting
VLAN Configuration
VLAN Segmentation
Inter-VLAN Routing
Router-on-a-Stick
OSPF Configuration
LACP Configuration
ACL Configuration
Wireless Network Design
Wireless Security
SNMP Monitoring
Network Security
Network Redundancy
Network Troubleshooting
Cisco Packet Tracer
Risk Management
Project Planning
RACI Analysis
Technical Documentation
My Contribution

My documented contribution to the project focused on:

ACL & SNMP Monitoring Implementation

My contribution involved the security and monitoring aspects of the network design, including:

ACL implementation
Traffic filtering
Network security controls
SNMP monitoring
Network visibility
Monitoring-related validation
Project Deliverables

The repository contains the project documentation.

File	Description
README.md	Project overview and technical documentation
Basic-Home-Network-Report.docx	Complete project report
Project Report

The complete report contains the project's:

Problem Statement
Objectives
Scope
Proposed Solution
Network Design
VLAN Design
IP Addressing
Inter-VLAN Routing
OSPF Configuration
Wireless Configuration
LACP Design
ACL Security
SNMP Monitoring
Project Plan
RACI Matrix
Gantt Chart
Work Breakdown Structure
Critical Path
Project Milestones
Risk Management
Lessons Learned
Budget
Project Impact

Project Report:

Basic Home Network Report

Repository Structure
Basic-Home-Network-Design/
├── README.md
└── Basic-Home-Network-Report.docx
Team

This was a team project developed as an applied project.

Team members documented in the project report include:

Mohamed Fadhul
Mohamed ALasfoor
Abdulla Rashdan
Abdulrahman Altairey
Abdulla Selail
Academic Project

This project was developed as an applied networking project and combines practical network design with security, monitoring, redundancy, scalability, and project management.

The project demonstrates how a traditional flat home network can be redesigned into a more structured network using VLANs, dynamic routing, link aggregation, wireless segmentation, ACLs, and SNMP monitoring.

Disclaimer

This project was developed for academic and educational purposes using a simulated networking environment.
