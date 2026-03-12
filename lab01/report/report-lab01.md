# Lab Report: PLGrid

**Name:** Robert Mesek  
**Lab:** 1  
**Date:** March 12, 2026

---

## PLGrid Consortium  
<!-- https://www.plgrid.pl/about-us#plgrid-consortium -->
PLGrid is a unique environment connecting world-class IT resources and specialized competences, built to support the research and development domain in Poland. It facilitates the solution of important research problems and provide tools to accelerate the development of innovative technologies.

The Polish PLGrid infrastructure is managed by the PLGrid Consortium, established in January 2007, which includes the following computing centres:

- Academic Computer Centre Cyfronet AGH in Cracow
- Interdisciplinary Centre for Mathematical and Computational Modelling in Warsaw
- Poznan Supercomputing and Networking Center
- Centre of Informatics Tricity Academic Supercomputer and Network
- Wrocław Centre for Networking and Supercomputing
- National Centre for Nuclear Research in Otwock-Świerk

## Academic Computer Center CYFRONET AGH
<!-- https://www.cyfronet.pl/en/about-us/what-we-do -->
<!-- https://www.cyfronet.pl/en/supercomputers/our-supercomputers -->
The Academic Computer Center CYFRONET AGH is the longest-operating and one of the largest supercomputing and networking centres in Poland, with a history of providing access to supercomputing resources dating back to 1975.

For years, ACC Cyfronet AGH has been the operator of the fastest supercomputers in Poland, repeatedly listed on the TOP500 world list, as well as very high-capacity data storage systems. It has three data centres, its own fibre-optic network, as well as technical facilities, personnel and competencies, allowing it to operate 24 hours a day, 365 days a year.

Cyfronet is the organiser and leader of the PLGrid Consortium, consolidating national computing resources and providing a range of unique computing and IT support services for science, as well as the leader of the National Competence Center in HPC, which acts as a contact and access point for HPC for both academia and innovative entities in the economy and public administration.

### ACC Cyfronet AGH Tasks
- providing computing power and other IT services to research teams and educational institutions,
- conducting scientific research and R&D activities, either alone or in cooperation with other units, mainly in the field of high performance computers, computer networks and IT and telecommunications services,
- actions to achieve the objectives and programs of the Polish government, contained in the assumptions of the ministries responsible for science and education, in the field of usage of the new information techniques and technologies in science, education, management and business,
- construction, maintenance and development of the IT infrastructure operated by the Centre,
- studies, analyses and implementations of new techniques and technologies, which could be potentially used in the design, construction and operation of an IT infrastructure,
- advice, expertise, training and staff skills improvement as well as other activities in the field of computer science, computer networks, high performance computers and computer services,
- identifying, evaluating and promoting new solutions in the scope of the activities of the Centre to be used in the fields of science, education, administration, economy and management,
- providing computing power, ICT infrastructure and other services - based on the Centre's potential - to entities interested in their implementation or use,
- conducting the payable research and cooperation with the economy in the area and innovating solutions.

### Operational Supercomputers

#### Helios
Helios has 37 PFLOPS of theoretical computing power, more than 131 thousand computing cores, 393 TB of RAM, and 17.5 PB of disk system capacity, which together offer performance of almost 2 TB/s. 

The supercomputer was built according to Cyfronet's design by Hewlett-Packard Enterprise based on the HPE Cray EX4000 platform. It consists of three computing partitions:

- CPU equipped with 98 304 AMD Zen4 computing cores and 288.8 TB of DDR5 RAM,
- GPU equipped with 440 NVIDIA Grace Hopper GH200 superchips,
- INT for interactive work, equipped with 24 NVIDIA H100 accelerators and fast local NVMe memory.

Helios' computing power for AI computing is 1.8 ExaFlops.


#### Athena
Athena achieves a theoretical computing power of over 7.7 PFlops. Until the installation of Helios, it was the fastest supercomputer in Poland. 

Athena provides the Polish scientific community and economy with computing resources based on processors and GPGPU accelerators, along with the necessary data storage subsystem based on very fast flash memory. Athena's configuration includes: 48 servers with AMD EPYC processors and 1 TB of RAM (a total of 6144 CPU computing cores) and 384 NVIDIA A100 GPGPU cards.

#### Ares
The Ares supercomputer offers a total computing power of over 4 PFlops (the theoretical performance of the CPU part is over 3.5 PFlops, and the GPU part is over 0.5 PFlops). Ares' power is obtained from computing servers with Intel Xeon Platinum and Xeon Gold processors (37,824 computing cores) and 72 NVIDIA Tesla V100 computing cards.

Ares' computing servers can be divided into three groups:

- 532 servers, each equipped with 192 GB of RAM,
- 256 servers, each with 384 GB of RAM,
- 9 servers, each with 8 NVIDIA Tesla V100 cards.


#### Prometheus
Prometheus, operating at ACK Cyfronet AGH, has been listed 15 times in a row on the TOP500 list of the world's fastest supercomputers since its launch in 2015, with the highest position at 38. Its high placement on the TOP500 list was ensured by its computing power of 2.65 PFlops (PetaFlops), which was achieved primarily by using high-performance servers of the HP Apollo 8000 platform and connecting them with a 56 Gbps superfast InfiniBand network.

The supercomputer has 53,748 computing cores (energy-efficient and high-performance Intel Haswell and Intel Skylake processors) and 283.5 TB of DDR4 technology operating memory. The Prometheus supercomputer has two file systems with a total capacity of 10 PB and an access speed of 180 GB/s. It also features NVIDIA Tesla cards with GPGPUs. Prometheus was built by Hewlett-Packard according to the assumptions developed by Cyfronet experts.

#### Faeton
The future technology cluster Faeton, with a theoretical computing power of 288 TFlops, is built from 64 computing servers, each equipped with two Intel Xeon Platinum 8352s processors - supporting application memory encryption, 1 TB of RAM, and 100 Gb/s low-latency Ethernet network adapters. Additionally, 4 computing servers are equipped with 8 TB of Intel Optane SCM memory.

Faeton also includes service servers and 12 storage servers, offering over 1 PB of NVMe disk storage and 12 TB of SCM memory.