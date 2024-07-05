---
title: "A YANG Data Model for Network Diagnosis by Scheduling Sequences of OAM Tests"
abbrev: "Scheduling OAM YANG"
category: info

docname: draft-contreras-opsawg-scheduling-oam-tests-latest
submissiontype: IETF  # also: "independent", "IAB", or "IRTF"
number:
date:
consensus: true
v: 3
area: "Operations and Management"
workgroup: "Operations and Management Area Working Group"
keyword:
 - OAM
 - Scheduling
 - Test sequences
venue:
  group: "Operations and Management Area Working Group"
  type: "Working Group"
  mail: "opsawg@ietf.org"
  arch: "https://mailarchive.ietf.org/arch/browse/opsawg/"
  github: "vlopezalvarez/draft-contreras-opsawg-scheduling-oam-tests"
  latest: "https://vlopezalvarez.github.io/draft-contreras-opsawg-scheduling-oam-tests/draft-contreras-opsawg-scheduling-oam-tests.html"

author:
 -
    fullname: Luis M. Contreras
    organization: Telefonica
    email: "luismiguel.contrerasmurillo@telefonica.com"
 -
    fullname: Victor Lopez
    organization: Nokia
    email: "victor.lopez@nokia.com"

normative:

informative:


--- abstract

This document defines a YANG data model for network diagnosis on-demand using Operations, Administration, and Maintenance (OAM) tests. This document defines both 'oam-unitary-test' and 'oam-test-sequence' data models to enable activation of network diagnosis procedures.

--- middle

# Introduction

Operations, Administration, and Maintenance (OAM) tasks are fundamental functionalities of the network management. Given the emerging of data models and their utilization in Service Provider's management, the management of OAM tests represent also an area of interest for operators, which requires to be defined as a data model. OAM functionalities provide the means to identify and isolate faults, measure and report of performance. {{!RFC5860}} defines the three main areas involved in OAM:

* Fault management, which allows network operators to quickly identify and isolate faults in the network. The OAM framework defines mechanisms for fault detection and isolation, such as continuity check, link trace, and loopback.
+ Performance management enables monitoring network performance and diagnosing performance issues (i.e., degradation). Some of the measurements such as frame delay measurement, frame delay variation measurement, and frame loss measurement.
- Security management defines mechanisms to protect OAM communications from unauthorized access and tampering.

{{!RFC6428}} defines several use cases for OAM tools, including:

* Continuity Check: This function verifies that a path exists between two points in a network and that the path is operational.
+ Loopback: This function allows a device to loop back a received packet back to the sender for diagnostic purposes.
+ Link Trace: This function allows a network operator to trace a path through a network from one device to another.
+ Performance Monitoring: This function allows a network operator to monitor the performance of a network and to identify and diagnose performance issues.
+ Security Management: This function allows a network operator to protect OAM communications from unauthorized access and tampering.
- Configuration Management: This function allows a network operator to manage the configuration of network devices.

More recently, Incident Management {{!I-D.ietf-nmop-network-incident-yang}} also considers the possibility of incident diagnosis, which can be favoured by dynamic invocation of OAM tests.  

{{!RFC8531}}, {{!RFC8532}} and, {{!RFC8533}} defined YANG models for OAM technologies. On the other hand, {{!RFC8531}} defines a YANG data model for connection-oriented OAM protocols. The main aim of this document is to define a generic YANG data model that can be used to configure, control and monitor connection-oriented OAM protocols such as MPLS-TP OAM, PBB-TE OAM, and G.7713.1 OAM. {{!RFC8532}} provides a generic YANG data model that can be used to configure, control and monitor connectionless OAM protocols such as BFD (Bidirectional Forwarding Detection), LBM (Loopback Messaging) and VCCV (Virtual Circuit Connectivity Verification). {{!RFC8533}} provides a YANG data model that can be used to retrieve information related to OAM protocols such as Bidirectional Forwarding Detection (BFD), Loopback Messaging (LBM) and Virtual Circuit Connectivity Verification (VCCV).

{{!RFC8913}} specifies a YANG data model for client and server implementations of the Two-Way Active Measurement Protocol (TWAMP).

Previous RFCs defined the parameters required for each of the different tests that are used in network elements today. This document covers how to use OAM for network-wide use cases. Following, some illustrative examples are presented.

The YANG data model resulting from this document will conform to the Network Management Datastore Architecture (NMDA) {{!RFC8342}}.

## Terminology and Notations

This document assumes that the reader is familiar with the contents of {{!RFC7950}}, {{!RFC8345}}, {{!RFC8346}} and {{!RFC8795}}.

Following terms are used for the representation of this data model.

* OAM unitary test: it is a set of parameters that define a type of OAM test to be invoked. As an example, it includes the type test, configuration parameters, and target results.

* OAM test sequence: it is a set of OAM unitary tests that are run based on a set of time constraints, number of repetitions, order, and reporting outputs.

## Requirements Language

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED", "NOT RECOMMENDED", "MAY", and "OPTIONAL" in this document are to be interpreted as described in  {{!RFC2119}}, {{!RFC8174}} when, and only when, they appear in all capitals, as shown here.

## Prefix in Data Node Names

  In this document, names of data nodes and other data model objects will be prefixed using the standard prefix associated with the corresponding YANG imported modules, as shown in the following table.

| Prefix | Yang Module            | Reference    |
| ------ | ---------------------- | ------------ |
| oamut  | ietf-oam-unitary-tests | RFCXXX       |
| oamts  | ietf-oam-test-sequence | RFCXXX       |
| yang   | ietf-yang-types        | {{!RFC6991}} |
{: #tab-prefixes title="Prefixes and corresponding YANG modules"}

RFC Editor Note:
Please replace XXXX with the RFC number assigned to this document if the document becomes a RFC. Please remove this note in that case.

# Network-wide OAM Use Cases

## Troubleshooting

After the detection of a problem in the network, OAM tests are performed to find the root cause for the detected issue. However, a problem detected can be caused by a variety of factors, such as a misconfiguration, hardware failure, or a software bug. OAM tests can help to find the root cause by testing specific components of the network and looking for anomalies or issues.

There are a variety of different OAM tests that can be executed depending on the nature of the scenario. For example, if the issue is related to a L2 capability, tests can be run to check the status of the path via Ethernet Linktrace and later run an Ethernet Loopback to a concrete network element. If these tests are correct, the operator may want to check the availability of the service or its performance.

Even though the troubleshooting process may be different depending on the problem detected, there are certain common procedures or logic that can be executed in order to narrow down the cause of the problem.

## Birth Certificate

The aim of a birth certificate process is to validate that all relevant parameters are correct for a specific network service. The birth certificate process is done once the configuration of the network elements is completed and they are ready for service.

If the birth certificate is successful, it means that the network service is functioning correctly and meets the requirements defined by the operator. The process requires running a set of OAM tests to verify that the service is performing as expected.

The set of OAM tests done as part of a birth certificate process depends on the network service that is tested. For example, if the service is a virtual private network (VPN) Two-Way Active Measurement Protocol (TWAMP) Light will be used, while if the service is an E-LINE, Ethernet CFM tests will be executed.

Once the birth certificate process has been completed and the OAM tests have been run, the test results are stored as part of the documentation process performed by the operator.

## Proactive Supervision

There are communication services that require to fulfill Service Level Agreements (SLAs). SLAs define performance parameters that the service must fulfill in order to meet the requirements of the customer or end user.

Proactive testing ensures the SLAs are met. Proactive supervision requires running tests on service components to identify and resolve issues before they impact the customer or end user, or to minimize the impact of the end user.

Proactive testing can be done via OAM tests. These tests can be run periodically at regular intervals depending on the specific SLA requirements and the network operator procedures. These procedures may require documenting the test results for future auditing processes with the customers.

## Performance-based Path Routing

Path Computation Elements (PCEs) allow computing end-to-end paths in a network. PCEs are used to facilitate traffic engineering and can be used to optimize network performance, reduce congestion, and improve the overall user experience.

There are different algorithms to calculate a path in the network for some of them the PCE requires traffic engineering information. TE information includes data such as link metrics, bandwidth availability, and routing constraints. By using this information, the PCE can compute the optimal path for a particular service, taking into account its constraints and requirements. OAM techniques allow obtaining link metrics like delay and loss which can be used in the PCE algorithms.

# Modelling the Scheduling of OAM Tests

This document proposes two models: OAM unitary test and OAM test sequence models.

## OAM Unitary Test

The OAM unitary test model encompasses parameters that define a specific type of OAM test to be performed. The YANG model includes a container named "oam-unitary-tests" that serves as a container for activating OAM unitary tests for network diagnosis procedures. Inside the container, there is a list called "oam-unitary-test" representing a list of specific OAM unitary tests. The list key is defined as "name", which provides a unique name for each test. Each OAM test in the list references a test type with its concrete parameters. The test types are out of the scope of this document. Moreover, each OAM unitary test has two temporal parameters: "period-of-time" and "recurrence". Both are imported from the "ietf-schedule" module from {{!I-D.ietf-netmod-schedule-yang}}. "period-of-time" identifies the period values that contain a precise period of time, while "recurrence" identifies the properties that contain a recurrence rule specification. "unitary-test-status" enumerates the state of the OAM unitary test.

{{oam-uni-test-tree-st}} contains the tree of the proposed model. Tree diagrams used in this document follow the notation defined in {{!RFC8340}}.

~~~~
module: ietf-oam-unitary-test
  +--rw oam-unitary-tests
     +--rw oam-unitary-test* [name]
        +--rw name                   string
        +--rw (test-type)
        +--rw period-of-time
        |  +--rw (forms)?
        |     +--:(period-explicit)
        |     |  +--rw explicit-start?   yang:date-and-time
        |     |  +--rw explicit-end?     yang:date-and-time
        |     +--:(period-start)
        |        +--rw start?            yang:date-and-time
        |        +--rw duration?         duration
        +--rw recurrence
        |  +--rw freq           enumeration
        |  +--rw (recurrence-bound)?
        |  |  +--:(until)
        |  |  |  +--rw until?   union
        |  |  +--:(count)
        |  |     +--rw count?   uint32
        |  +--rw interval?      uint32
        |  +--rw bysecond*      uint32
        |  +--rw byminute*      uint32
        |  +--rw byhour*        uint32
        |  +--rw byday* [weekday]
        |  |  +--rw direction*   int32
        |  |  +--rw weekday      schedule:weekday
        |  +--rw bymonthday*    int32
        |  +--rw byyearday*     int32
        |  +--rw byyearweek*    int32
        |  +--rw byyearmonth*   uint32
        |  +--rw bysetpos*      int32
        |  +--rw wkst?          schedule:weekday
        +--rw unitary-test-status?   enumeration
~~~~
{: #oam-uni-test-tree-st title="OAM unitary test" artwork-align="center"}

The 'unitary-test-status' state machine is shown in {{st-unitary-test-status}}. The state machine includes the following states:

* "planned": The initial state where the test is planned.
* "configured": The state where the test is being configured.
* "ready": The state where the test is ready to be executed.
* "on-going": The state where the test is currently running.
* "stop": The state where the test is manually stopped.
* "error": The state where an error occurs during the test.
* "finished": The final state where the test is completed.

~~~~

   +---------+      +----------+      +---------+
+->| planned |----->|configured|----->|  ready  |
|  +---------+      +----------+      +---------+
|      A                 |                |
|      |                 |                V
|      |     +-------+   |          +----------+
|      ------| error |<--+----------| on-going |
|      |     +-------+              +----------+
|      |                                  |
|      V                                  |
|  +---------+      +--------+            |
+--| finished|<-----|  stop  |<------------+
   +---------+      +--------+            |
       A                                  |
       |                                  |
       +----------------------------------+

~~~~
{: #st-unitary-test-status title="OAM unitary test state machine" artwork-align="center"}

## OAM Test Sequence

The OAM test sequence model consists of a collection of OAM unitary tests that are executed based on specified time constraints, repetitions, ordering, and reporting outputs. These sequences provide a structured approach to running multiple OAM tests in a coordinated manner. Each OAM test sequence references a OAM unitary test type with its concrete parameters. Each OAM test sequence has two temporal parameters: "period-of-time" and "recurrence". Both are imported from the "ietf-schedule" module from {{!I-D.ietf-netmod-schedule-yang}}. "period-of-time" identifies the period values that contain a precise period of time, while "recurrence" identifies the properties that contain a recurrence rule specification. "unitary-test-status" enumerates the state of the OAM test. Finally, "test-sequence-status" shows the state of the OAM test sequence.

An example of the proposed structure {{oam-test-sequence-tree-st}}.

~~~~

module: ietf-oam-test-sequence
  +--rw oam-test-sequence
     +--rw test-sequence* [name]
        +--rw name                   string
        +--rw test-ref* [name]
        |  +--rw name             string
        |  +--rw (test-type)
        |  +--rw numexecutions?   uint32
        +--rw period-of-time
        |  +--rw (forms)?
        |     +--:(period-explicit)
        |     |  +--rw explicit-start?   yang:date-and-time
        |     |  +--rw explicit-end?     yang:date-and-time
        |     +--:(period-start)
        |        +--rw start?            yang:date-and-time
        |        +--rw duration?         duration
        +--rw recurrence
        |  +--rw freq           enumeration
        |  +--rw (recurrence-bound)?
        |  |  +--:(until)
        |  |  |  +--rw until?   union
        |  |  +--:(count)
        |  |     +--rw count?   uint32
        |  +--rw interval?      uint32
        |  +--rw bysecond*      uint32
        |  +--rw byminute*      uint32
        |  +--rw byhour*        uint32
        |  +--rw byday* [weekday]
        |  |  +--rw direction*   int32
        |  |  +--rw weekday      schedule:weekday
        |  +--rw bymonthday*    int32
        |  +--rw byyearday*     int32
        |  +--rw byyearweek*    int32
        |  +--rw byyearmonth*   uint32
        |  +--rw bysetpos*      int32
        |  +--rw wkst?          schedule:weekday
        +--ro sequence?   enumeration

~~~~
{: #oam-test-sequence-tree-st title="OAM test sequence" artwork-align="center"}


The 'test-sequence-status' state machine is shown in {{st-test-sequence-status}}. The state machine includes the following states:

* "planned": The initial state where the test is planned.
* "configured": The state where the test is being configured.
* "ready": The state where the test is ready to be executed.
* "on-going": The state where the test is currently running.
* "stop": The state where the test is manually stopped.
* "success": The success state when all unitary tests were successful.
* "failure": The success state when One or more tests in the sequence got an error.
* "error": The state where an error occurs during the test.

~~~~

    +---------+      +----------+      +---------+
 +->| planned |----->|configured|----->|  ready  |
 |  +---------+      +----------+      +---------+
 |    A   A                 |              |
 |    |   |                 |              V
 |    |   |     +-------+   |          +----------+
 |    |   ------| error |<--+----------| on-going |
 |    |         +-------+              +----------+
 |    |                                    |
 |    |                                    |
 | +---------+      +--------+             |
 | | failure |<-----|  stop  |<------------+
 | +---------+      +--------+             |
 |                                         |
 | +---------+                             |
 +-| success |<----------------------------+
   +---------+

~~~~
{: #st-test-sequence-status title="OAM unitary test state machine" artwork-align="center"}


# YANG Data Models for Scheduling OAM Tests

## YANG Models Overview

This document proposes two models: OAM unitary test and OAM test sequence. OAM unitary test is a set of parameters that define a type of OAM test to be invoked. As an example, it includes the type test, configuration parameters, and target results. The OAM test sequences are a set of OAM unitary tests that are run based on a set of time constraints, number of repetitions, order, and reporting outputs.

## YANG Model for Scheduling OAM Unitary Test

~~~~~~~~~~
<CODE BEGINS> file ietf-oam-unitary-test@2023-07-10.yang
{::include ./Yang/ietf-oam-unitary-test.yang}

<CODE ENDS>
~~~~~~~~~~

## YANG Model for OAM Test Sequence

~~~~~~~~~~
<CODE BEGINS> file ietf-oam-test-sequence@2023-07-10.yang

{::include ./Yang/ietf-oam-test-sequence.yang}

<CODE ENDS>
~~~~~~~~~~

# Using Device Mode Within OAM Scheduling Models

This section discusses the problematic of reusing device models already defined in IETF within the context of scheduling OAM tests. There are two main approaches to enable OAM scheduling models:
  * Importing YANG model into the OAM scheduling models. This approach will copy the device model into the OAM unitary test model to enable the configuration and utilization of the desire OAM test. This approach requires to recreate new YANG models for each new test type or variation of the device models.
  * Schema-mount allows mounting a data model at a specified location of another (parent) schema. The main difference with importing the YANG modules is that they don't have to be prepared for mounting; any existing modules such as "ietf-twamp" can be mounted without any modifications.

As an exmaple, we will use {{!RFC8913}}, which defines a YANG data model for TWAMP, to illustrate how device models could be used.


# Security Considerations

The YANG module targeted in this document defines a schema for data that is designed to be accessed via network management protocols such as NETCONF {{!RFC6241}} or RESTCONF {{!RFC8040}}.  The lowest NETCONF layer is the secure transport layer, and the mandatory-to-implement secure transport is Secure Shell (SSH) {{!RFC6242}}. The lowest RESTCONF layer is HTTPS, and the mandatory-to-implement secure transport is TLS {{!RFC5246}}.

The NETCONF access control model {{!RFC6536}} provides the means to restrict access for particular NETCONF or RESTCONF users to a preconfigured subset of all available NETCONF or RESTCONF protocol operations and content.

There are a number of data nodes defined in this YANG module that are writable/creatable/deletable (i.e., config true, which is the default).  These data nodes may be considered sensitive or vulnerable   in some network environments.  Write operations (e.g., edit-config) to these data nodes without proper protection can have a negative effect on network operations.

In which refers to the scheduling of the tests, security considerations in {{!I-D.ietf-netmod-schedule-yang}} are also applicable here.


# IANA Considerations

TBC

# Implementation Status

This section will be used to track the status of the implementations of the model. It is aimed at being removed if the document becomes RFC.

--- back

# Acknowledgments
{:numbered="false"}

TODO acknowledge.
