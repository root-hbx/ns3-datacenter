### Isolation

- Working Directory: `/home/bxhu/ns3-datacenter/simulator/ns-3.39/examples/DSH`
- Plot Directory: `/home/bxhu/ns3-datacenter/simulator/ns-3.39/examples/DSH/plot`
- Print Directory: `/home/bxhu/ns3-datacenter/simulator/ns-3.39/examples/DSH/plot`

You can replace `bxhu` and use your `$NAME` here.

#### Script

##### Running Scripts

```bash
cd /home/bxhu/ns3-datacenter/simulator/ns-3.39/examples/DSH
python exp_isolation_InsRoom.py
```

##### Plot Scripts

```bash
cd /home/bxhu/ns3-datacenter/simulator/ns-3.39/examples/DSH/plot
python plot_isolation_insurance_headroom.py
```

##### Print Scripts

```bash
cd /home/bxhu/ns3-datacenter/simulator/ns-3.39/examples/DSH/plot
python print_isolationFct.py
```

##### Corresponding Folders

After running these scripts above, they will generate corresponding folders automatically:

- Running Script
	- `DSH/data/exp_isolation_InsRoom_{mmu_kind}-{sche}-{cc}-{burst_hosts}h-{mx_pg+1}pg-{total_inf_burst_load}bstload`
- Plot Script
	- `DSH/image`
- Print Script
	- `DSH/csv`

#### Configuration

![alt text](./readme-image/4h-version2.png)

In this experiment, we have 2 groups, each group has 5 hosts, and one more switch.

In this scenario, host0/1/4/5/6 form group1 (in red), host2/3/7/8/9 form group2 (in black) and host10 is a switch.

In each group, we have to let CoS-1 flows generated from different hosts towards different destinations to avoid the interference. Besides, CoS2-7 flows generated from different hosts must arrive at the same destination to make it easier to generate congesting queues.

- **topology**: 10 hosts (2 groups, each group has 5 hosts), 1 switch
- **link**: 100Gbps, 2us
- **running duration**: 2s
- **flow generating duration**: 1s
- **CC**: DCQCN / HPCC / PowerTCP
- **buffer**: 16MB
- **total reserve**: 3,072B \* 32(16) \* 7
- **total insurance headroom**: 56,840B \* 32(16)port (DSH), 56840B \* 32(16)port (DSHnoSH), 56840B \* 32(16)port \* 7pg (Normal)
- **mmu**: DSH / DSHnoSH / SIH
- **DT alpha**: 1/16
- **burst size**: 64KB x 2 x 4 (4 groups, each one has 2 bursty flows)
- **flow intro**: background flow (CoS: 1, load: 0.2, mode: web-search) + bursty flow (CoS: 2-7, total load: 0.2-0.8, each vary from 0.2/6 to 0.8/6, mode: bursty)
- **flow configuration**: (We use "->" to replace "towards" here)
	- background flow: 
		- Group1: host0 -> host4 | host1 -> host5
		- Group2: host2 -> host7 | host3 -> host8
	- bursty flow: 
		- Group1: host0 -> host6 | host1 -> host6
		- Group2: host2 -> host9 | host3 -> host9

#### Output

##### Running Scripts

- **fct file**: `exp_isolation_InsRoom_{mmu_kind}-{sche}-{cc}-{burst_hosts}h-{mx_pg+1}pg-{total_inf_burst_load}bstload/fct.txt`
- **mmu**: DSH / DSHnoSH / Normal
- **sche**: DWRR
- **cc**: DCQCN / HPCC / PowerTCP
- **burst_hosts**: 4
- **mx_pg**: 7 (which means pg equals to 8)
- **total_inf_burst_load**: 0.2 - 0.8
- **fct data format** (separated by spaces): src_ip | dst_ip | src_port | dst_port | pg | flow_size (B) | flow_start_time (ns) | flow_running_time (flow_end_time - flow_start_time) (ns)

##### Plot Scripts

Data Unit: ms

**Image file (DSH / DSHnoSH / Normal)**: 

- `DSH/image/InsHeadroom-DCQCN.png` / `DSH/image/InsHeadroom-HPCC.png` / `DSH/image/InsHeadroom-PowerTCP.png` / `DSH/image/InsHeadroom-None.png`

**Image file (DSH / Normal)**: 

- `DSH/image/InsHeadroom-DCQCN-DSH_SIH.png` / `DSH/image/InsHeadroom-HPCC-DSH_SIH.png` / `DSH/image/InsHeadroom-PowerTCP-DSH_SIH.png` / `DSH/image/InsHeadroom-None-DSH_SIH.png`

##### Print Scripts

Data Unit: ms

__Printed CSV (DSH / DSHnoSH / Normal):__

- `DSH/csv/DCQCN_DSH_DSHnoSH_SIH.csv` / `DSH/csv/HPCC_DSH_DSHnoSH_SIH.csv` / `DSH/csv/PowerTCP_DSH_DSHnoSH_SIH.csv` / `DSH/csv/None_DSH_DSHnoSH_SIH.csv`

