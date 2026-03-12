---
title: Purchasing in Myriad
categories:
 - Myriad
---

# Purchasing in Myriad

!!! important "27 Jan 2026: Myriad node purchases paused"
    To ensure system stability, we are not accepting new hardware for Myriad until the networking refresh and operating system upgrades are finalized. We are avoiding further integrations on the current OS due to driver complexity. We anticipate reopening for purchases in March/April, pending successful infrastructure deployment.

    Please contact rc-support@ucl.ac.uk for prices and let us know your timescales. 

Researchers may purchase additional resources to be used as part of the [Myriad](../Clusters/Myriad.md) High Performance Computing cluster if the free service does not meet their needs. These resources can be made available in one of two ways:

 - Nodes purchased by researchers can be converted into a quarterly allocation of “priority cycles” equivalent to the amount of computation provided by the nodes, but usable across the whole cluster. We calculate how much time exists on your nodes for three months, and every three months you receive an allocation of that much priority time to use however you want on Myriad (GPU/large memory nodes cost more to use, and give you more if you buy them). We recommend this route.
 - The purchaser may request to buy nodes to be reserved for their group or department, restricting usage to the owned nodes. There may be an additional cost implication to this option (price on application).

Costs will include backend infrastructure – racks, switches, cables etc – needed to integrate the compute nodes into the facility. Both options can be costed into research proposals. These costs will be stated in an agreed Statement of Work document giving details of the number of each node type to be purchased, which must be approved by ARC and the purchaser before the purchase can go ahead.


## Node types

We have one standard compute nodes, and several variants of NVIDIA and AMD GPU nodes.

### Standard compute node

#### [CPU node] 
Lenovo ThinkSystem SD535 V3

- 1 x AMD EPYC 9554P 64C 360W 3.1GHz (64 cores)
- 768G RAM
- 2x 480GB M.2 7450 PRO NVMe SSD (960G)

### NVIDIA GPU nodes
Note that RAM and disk amounts are volatile just now and some of these specs are likely to change. The H200s are also being discontinued.

#### [GPU node 2U 01] 
Lenovo ThinkSystem SR655 V3 with 2x NVIDIA L40S GPUs

- 1x AMD EPYC 9355 32C 280W 3.55GHz Processor (32 cores)
- 768G RAM, 2x 960GB M.2 NVMe (1920GB)
- 2x NVIDIA L40S 48GB

#### [GPU node 2U 02] 
Lenovo ThinkSystem SR655 V3 with 2x NVIDIA H200

- 2 x AMD EPYC 9355 32C 280W 3.55GHz Processor (64 cores)
- 768G RAM, 2x 960GB M.2 NVMe (960GB)
- 2 x NVIDIA H200 NVL 141GB PCIe GPU

#### [GPU node 2U 03] 
Lenovo ThinkSystem SR655 V3 with 4x NVIDIA L4

- 1x AMD EPYC 9355 32C 280W 3.55GHz Processor (32 cores)
- 768G RAM, 2x 960GB M.2 NVMe (1920GB)
- 4x NVIDIA L4 24GB

#### [GPU node 2U 04]
Lenovo ThinkSystem SR655 V3 with 2x NVIDIA RTX 6000 (Max-Q)

- 1x AMD EPYC 9355 32C 280W 3.55GHz Processor (32 cores)
- 768GB RAM, 2x 480GB M.2 NVMe (960G)
- 2x RTX 6000 Ada 96GB

#### [GPU node 3U 03] 
Lenovo ThinkSystem SR675 V3 with 8x NVIDIA RTX PRO 6000 GPUs

- 2 x AMD EPYC 9455 48C 300W 3.15GHz Processor (96 cores)
- 768G RAM, 2 x M.2 VA 480GB NVMe SSD (960G)
- 8 x NVIDIA RTX PRO 6000 Blackwell Server Edition 96GB

#### [GPU node 3U 04] 
Lenovo ThinkSystem SR675 V3 with 8x NVIDIA H200 GPUs

- 2x AMD EPYC 9475F 48C 400W 3.65GHz Processor (96 cores total)
- 1536G RAM, 2x 15.36TB NVMe (30.72TB)
- 8x NVIDIA H200 NVL 141GB PCIe GPU + NVLink Bridges

#### [GPU node 3U 05] 
Lenovo ThinkSystem SR675 V3 with 4x NVIDIA H200 GPUs

- 2x AMD EPYC 9355 32C 280W 3.55GHz (64 cores total)
- 768GB RAM, 2 x M.2 VA 480GB NVMe SSD (960GB)
- 4 x NVIDIA H200 NVL 141GB PCIe GPU

### AMD GPU nodes

#### [GPU node 3U AMD 01]
Lenovo SR675 V3 with 4x AMD Instinct MI210

- 2x AMD EPYC 9354P 32C 280W 3.25GHz (64 cores total)
- 1536GB RAM, 2x 960GB NVMe (1920GB)
- 4x AMD Instinct MI210

#### [GPU node 3U AMD 02]
Lenovo SR675 V3 with 8x AMD Instinct MI210

- 2x AMD EPYC 9354P 32C 280W 3.25GHz 
- 1536GB RAM, 2x E1.S 5.9mm 7450 PRO 3.84TB NVMe
- 8x AMD Instinct MI210

## FAQs

#### Can I add the cost of purchasing nodes to a grant application?

If you are putting together a grant application and think that you may need to ask for the cost of additional computing resources to be covered, please contact us. We will be able to assess your requirements, recommend appropriate hardware, and then provide an estimate and a supporting statement.

#### Can you give me advice on what hardware to buy?

Yes, we’d be happy to discuss this with you. Please contact [rc-support@ucl.ac.uk](mailto:rc-support@ucl.ac.uk).

#### What type of nodes can we purchase?

[Current node types](#node-types).

If you require an alternative/custom specification, we can’t guarantee that we will be able to accommodate this on the cluster, but we’re happy to look into it.

#### I want to apply for more than £50k worth of equipment, will we have to go through a tender process?

No. We have a framework agreement with the vendor which covers all hardware purchases.

#### I know what hardware I need. Can you send me a quote?

We can send ballpark quotes initially.

Before we can send you a final quote, we will need to agree on a detailed specification. Please email [rc-support@ucl.ac.uk](mailto:rc-support@ucl.ac.uk) with the following information:

 - Budget holder: name and contact details
 - Type and number of nodes you’d like to purchase

We will then either send you a specification to approve, or ask to meet to discuss your requirements further. Once this is agreed, we aim to get back to you with a quote within two weeks.

#### How do I manage who has permission to use our nodes/priority queue?

When you purchase nodes or priority cycles, we will ask you for a list of usernames of people who have permission to use the resource — access is managed using access control lists on Myriad.  If your list of users is an entire department, we can automatically generate this list nightly. Resource owners or designated resource administrators can request a change of membership of these groups by submitting a request in [MyServices](https://myservices.ucl.ac.uk) or emailing [rc-support@ucl.ac.uk](mailto:rc-support@ucl.ac.uk). 

#### What is the difference between paying for priority cycles and purchasing dedicated nodes?

Priority cycles is the better option for most people as it provides greater flexibility: priority cycles can be used across many nodes at once, and there is an entire allocation period to use them. Dedicated hardware however would need to be in use 24/7 in order to get the most out of it. Researchers might want dedicated nodes if they have particular requirements which mean they can only run their work on their own nodes; e.g., they have purchased non-standard nodes, or the software they are using requires a static licence tied to a particular node.

Please see [Paid-For Resources: How to Use](How_to_Use.md) for more information.

#### Will my 3-month priority cycle allocation roll over to the next quarter if I don’t use it?

No.

#### I want the flexibility of priority cycles, but my funder requires an invoice for specific hardware.

Even if you require an invoice for specific hardware, you can still convert the physical hardware into priority cycles. We will add the nodes you purchased to the general pool and give you the equivalent in priority cycles.

#### What happens to nodes I have purchased once they’ve reached the end of warranty?  

Node purchases have a three-year warranty as standard. After the warranty period ends, if the nodes still function 
they will be returned to the general use pool (if they were dedicated nodes) and continue running jobs until they 
fail, or until they are retired.

